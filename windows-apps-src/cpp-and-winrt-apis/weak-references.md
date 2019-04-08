---
description: Windows-Runtime ist ein System mit Verweiszählung. Es ist wichtig, dass Sie mit der Bedeutung und dem Unterschied zwischen starken und schwachen Verweisen vertraut sind.
title: Schwache Referenzen in C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, Uwp, standard, c++, Cpp, Winrt, Projektion, sicheres, schwache, Referenz
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 507b3cee71819df1d0163380a494e6a15936109f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630815"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Starke und schwache Verweise in C++ / WinRT

Die Windows-Runtime ist ein System mit referenzzählung. und in einem solchen System es wichtig, dass Sie ist wissen, über die Bedeutung und Unterscheidung zwischen, starke und schwache Verweise (und Verweise, die keine, z. B. den impliziten *dies* Zeiger). Wie Sie in diesem Thema sehen werden, kann zu wissen, wie Sie diese Verweise korrekt verwalten bedeutet, dass den Unterschied zwischen ein zuverlässiges System, das reibungslos ausgeführt wird, und die schwankungen stürzt ab. Durch die Bereitstellung von Hilfsfunktionen, die umfassende Unterstützung in der sprachprojektion [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) erfüllen Sie Ihre Arbeit zum Erstellen komplexer Systeme, einfach und ordnungsgemäß in der Mitte.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Sicheren Zugriff auf die *dies* Zeiger in einer Coroutine Klassenmembern

Die codeauflistung unten zeigt ein typisches Beispiel für eine Coroutine, die eine Memberfunktion einer Klasse ist.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

struct MyClass : winrt::implements<MyClass, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    IAsyncOperation<winrt::hstring> RetrieveValueAsync()
    {
        co_await 5s;
        co_return m_value;
    }
};

int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };

    winrt::hstring result{ async.get() };
    std::wcout << result.c_str() << std::endl;
}
```

**MyClass::RetrieveValueAsync** funktioniert für eine Weile, und klicken Sie dann letztendlich gibt eine Kopie der `MyClass::m_value` -Datenmember. Aufrufen von **RetrieveValueAsync** bewirkt, dass ein asynchroner Objekt erstellt, und dieses Objekt verfügt über einen impliziten *dies* Zeiger (über die schließlich `m_value` zugegriffen wird).

Hier ist die vollständige Abfolge der Ereignisse.

1. In **main**, eine Instanz von **MyClass** wird erstellt (`myclass_instance`).
2. Die `async` -Objekt erstellt wird, verweist (über dessen *dies*) zu `myclass_instance`.
3. Die **Winrt::Windows::Foundation::IAsyncAction::get** Funktion ein paar Sekunden lang blockiert und gibt dann das Ergebnis des **RetrieveValueAsync**.
4. **RetrieveValueAsync** gibt den Wert der `this->m_value`.

Schritt 4 ist sicher solange *dies* gültig ist.

Aber was geschieht, wenn die Instanz der Klasse zerstört wird, bevor der asynchrone Vorgang abgeschlossen ist? Es gibt bietet verschiedene Möglichkeiten, die die Instanz der Klasse außerhalb des gültigen Bereichs wechseln kann, bevor die asynchrone Methode abgeschlossen ist. Aber wir können simuliert werden, indem Sie die Instanz der Klasse auf `nullptr`.

```cppwinrt
int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };
    myclass_instance = nullptr; // Simulate the class instance going out of scope.

    winrt::hstring result{ async.get() }; // Behavior is now undefined; crashing is likely.
    std::wcout << result.c_str() << std::endl;
}
```

Nach dem Zeitpunkt, in dem wir die Klasseninstanz zerstören, offenbar direkt verweisen wir auf ihn wieder nicht. Aber natürlich das asynchrone Objekt verfügt über eine *dies* Zeiger, und versucht, Sie verwenden, um den Wert in die Instanz der Klasse gespeichert zu kopieren. Die Coroutine ist eine Memberfunktion, und er davon ausgeht, verwenden die *dies* Zeiger mit verhüten.

Durch diese Änderung am Code wir ausgeführt werden, auf ein Problem in Schritt 4, da die Instanz der Klasse zerstört wurde, und *dies* ist nicht mehr gültig. Sobald das asynchrone Objekt versucht, die auf die Variable in der Klasseninstanz zugreifen, wird es stürzt ab (auch etwas Unternehmen, nicht vollständig definiert).

Die Lösung besteht darin, geben Sie den asynchronen Vorgang&mdash;die Coroutine&mdash;eigene starken Verweis auf die Instanz der Klasse. Im aktuellen Zustand die Coroutine enthält einen unformatierten *dies* Zeiger auf die Instanz der Klasse; Dies ist jedoch nicht ausreichend, um die Instanz der Klasse beibehalten werden soll.

Um die Instanz der Klasse beibehalten werden soll, ändern Sie die Implementierung der **RetrieveValueAsync** unten dargestellten.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Da C++ / WinRT-Objekt direkt oder indirekt leitet sich von der [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) Vorlage C++ / WinRT-Objekt aufrufen können, die [ **implements.get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) geschützte Memberfunktion zum Abrufen von eines starken Verweis auf die *dies* Zeiger. Beachten Sie, dass keine Notwendigkeit besteht, verwenden die `strong_this` Variable, die den Aufruf von **Get_strong** Ihr Verweiszähler erhöht und hält Ihre implizite *dies* Zeiger ungültig.

Dies behebt das Problem, das wir vorher hatten, wenn wir mit Schritt 4 haben. Selbst wenn alle anderen Verweise auf die Instanz der Klasse nicht mehr angezeigt, wurde die Coroutine unternommen, entsprechende Vorsichtsmaßnahmen treffen garantieren, dass seine Abhängigkeiten stabil sind.

Wenn ein starker Verweis nicht geeignet ist, rufen Sie stattdessen [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) abzurufenden einen schwachen Verweis auf *dies*. Bestätigen Sie, dass Sie vor dem Zugriff auf einen starken Verweis abrufen können *dies*.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto weak_this{ get_weak() }; // Maybe keep *this* alive.

    co_await 5s;

    if (auto strong_this{ weak_this.get() })
    {
        co_return m_value;
    }
    else
    {
        co_return L"";
    }
}
```

Im obigen Beispiel behalten nicht der schwache Verweis Sie die Instanz der Klasse von zerstört wird, wenn keine starken Verweise bleiben. Doch nun können Sie eine Möglichkeit zum Überprüfen, ob ein starker Verweis abgerufen werden kann, bevor Sie den Zugriff auf die Membervariable.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Sicheren Zugriff auf die *dies* Zeiger durch einen Delegaten zur Verarbeitung von Ereignissen

### <a name="the-scenario"></a>Das Szenario

Allgemeine Informationen zur Verarbeitung von Ereignissen finden Sie unter [Behandeln von Ereignissen mithilfe von Delegaten in C++ / WinRT](handle-events.md).

Im vorherigen Abschnitt hervorgehoben, potenzielle Probleme mit der Objektlebensdauer in den Bereichen von Coroutinen und Parallelität. Aber, wenn Sie ein Ereignis mit der Memberfunktion des Objekts, oder von behandeln eine Lambda-Funktion in einer Objektmemberfunktion, Sie denken müssen, die relative Lebensdauer der den Empfänger Ereignis (das Objekt, das das Ereignis) und die Ereignisquelle (das Objekt Auslösen des Ereignisses). Sehen wir uns einige Codebeispiele an.

Die codeauflistung unten zuerst definiert eine einfache **EventSource** -Klasse, die ein generisches Ereignis auslöst, die von allen Delegaten behandelt wird, die sie hinzugefügt wurden. Diese Beispielereignis auftritt, verwenden die [ **Windows::Foundation::EventHandler** ](/uwp/api/windows.foundation.eventhandler) Delegattyp, aber die Probleme und Lösungen hier gelten, für alle Delegattypen.

Anschließend wird die **EventRecipient** Klasse stellt einen Handler für die **EventSource::Event** Ereignisses in Form einer Lambda-Funktion.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;

struct EventSource
{
    winrt::event<EventHandler<int>> m_event;

    void Event(EventHandler<int> const& handler)
    {
        m_event.add(handler);
    }

    void RaiseEvent()
    {
        m_event(nullptr, 0);
    }
};

struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event([&](auto&& ...)
        {
            std::wcout << m_value.c_str() << std::endl;
        });
    }
};

int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_source.RaiseEvent();
}
```

Das Muster ist, dass der Empfänger Ereignis einen Lambda-Ereignishandler mit Abhängigkeiten verfügt, auf dessen *dies* Zeiger. Wenn der Empfänger Ereignis länger ist die Ereignisquelle als, überdauert er diese Abhängigkeiten. Und in diesen Fällen, die häufig verwendet werden, das Muster funktioniert gut. Einige dieser Fälle sind offensichtlich, z. B. wenn eine UI-Seite ein Ereignis verarbeitet, das von einem Steuerelement ausgelöst wird, das sich auf der Seite befindet. Die Seite länger ist als die Schaltfläche mit den&mdash;also der Handler auch länger ist als die Schaltfläche. Dies gilt immer dann, wenn der Empfänger die Quelle besitzt (z. B. als Datenelement), oder wenn der Empfänger und die Quelle gleichgeordnet sind und sich direkt im Besitz eines anderen Objekts befinden. Wenn Sie sicher sind, dass Sie einen Fall haben, in dem der Handler das *this*-Objekt nicht überleben wird, können Sie *this*normal verwenden, ohne Rücksicht auf eine starke oder schwache Lebensdauer zu nehmen.

Aber es weiterhin Fälle gibt, in dem *dies* nicht überleben dessen Verwendung in einem Ereignishandler (einschließlich der Handler für Ereignisse, die sich auf Abschluss und Status von asynchronen Aktionen und Vorgängen ausgelöst), und es ist wichtig zu wissen, wie Sie den Umgang mit ihnen.

- Wenn Sie eine Coroutine erstellen, um eine asynchrone Methode zu implementieren, dann ist dies möglich.
- In seltenen Fällen mit bestimmten XAML-UI-Framework-Objekten (z. B. [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel)) ist dies möglich wenn der Empfänger finalisiert wird, ohne die Registrierung für die Ereignisquelle aufzuheben.

### <a name="the-issue"></a>Das Problem

Nächste Version des der **main** Funktion simuliert, was geschieht, wenn der Empfänger Ereignis zerstört wird (z. B. darin außerhalb des gültigen Bereichs) während die Ereignisquelle immer noch Auslösen von Ereignissen ist.

```cppwinrt
int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_recipient = nullptr; // Simulate the event recipient going out of scope.
    event_source.RaiseEvent(); // Behavior is now undefined within the lambda event handler; crashing is likely.
}
```

Der Empfänger Ereignis zerstört wird, aber der Lambda-Ereignishandler darin noch abonniert die **Ereignis** Ereignis. Wenn das Ereignis ausgelöst wird, versucht der Lambda-Ausdruck zu dereferenzieren der *dies* Zeiger, der zu diesem Zeitpunkt ungültig ist. Daher ergibt eine zugriffsverletzung Code im Ereignishandler (oder in der Coroutine Fortsetzung) versuchen ihn anzuwenden.

> [!IMPORTANT]
> Wenn eine derartigen Situation auftreten, müssen Sie über die Lebensdauer des nachzudenken der *dies* Objekts ist und davon, ob die erfassten *dies* Objekt länger ist als die Erfassung. Wenn dies nicht der Fall, klicken Sie dann erfassen sie mit einem starken oder einen schwachen Verweis wie unten.
>
> Oder&mdash;, wenn es für Ihr Szenario sinnvoll und threading Überlegungen können sogar&mdash;und dann eine andere Möglichkeit besteht, den Handler zu widerrufen, nachdem der Empfänger mit dem Ereignis, oder klicken Sie im des Empfängers-Destruktor ausgeführt wird. Finden Sie unter [Widerrufen von registrierten Delegaten](handle-events.md#revoke-a-registered-delegate).

Dies ist, wie wir den Handler registriert sind.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Der Lambda-Ausdruck erfasst automatisch alle lokalen Variablen als Verweis. Also in diesem Beispiel konnte wir gleichwertig dies geschrieben haben.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

In beiden Fällen wir sind gerade erfasst die unformatierte *dies* Zeiger. Und die keine Auswirkungen auf die verweiszählung, damit nichts blockiert das aktuelle Objekt zerstört wird.

### <a name="the-solution"></a>Die Lösung

Die Lösung besteht darin, einen starken Verweis zu erfassen. Ein starker Verweis *ist* inkrementiert den Verweiszähler und *ist* keep-alive für das aktuelle Objekt. Sie deklarieren eine erfassungsvariable (aufgerufen `strong_this` in diesem Beispiel), und initialisieren Sie es mit einem Aufruf von [ **implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function), die einen starken Verweis auf abruft unsere  *Dies* Zeiger.

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Sie können sogar lassen Sie die automatische Erfassung des aktuellen Objekts und Zugriff auf die Data-Member über die erfassungsvariable statt über den impliziten *dies*.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Wenn ein starker Verweis nicht geeignet ist, rufen Sie stattdessen [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) abzurufenden einen schwachen Verweis auf *dies*. Bestätigen Sie, dass Sie einen starken Verweis weiterhin daraus abrufen können, bevor Sie den Zugriff auf Member.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Wenn Sie eine Memberfunktion als Delegat verwenden

Als auch Lambda-Funktionen können diese Prinzipien gelten auch für eine Member-Funktion als Ihren Delegaten verwenden. Die Syntax unterscheidet, lassen Sie uns einige Codes untersucht. Hier folgt zunächst einmal die potenziell unsichere Member-Funktion Ereignishandler mit einem unaufbereiteten *dies* Zeiger.

```cppwinrt
struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event({ this, &EventRecipient::OnEvent });
    }

    void OnEvent(IInspectable const& /* sender */, int /* args */)
    {
        std::wcout << m_value.c_str() << std::endl;
    }
};
```

Dies ist der standard, herkömmliche Möglichkeit zum Verweisen auf ein Objekt und seine Memberfunktion. Um dies sicher zu machen, können Sie&mdash;ab Version 10.0.17763.0 (Windows 10, Version 1809) des Windows SDK&mdash;herstellen, einen starken oder einen schwachen Verweis an dem Punkt, in dem der Handler registriert ist. An diesem Punkt ist das Ereignisobjekt Empfänger bekannt, noch aktiv sein.

Rufen Sie einfach eine starke Referenz [ **Get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) anstelle der unformatierten *dies* Zeiger. C++ / WinRT stellt sicher, dass der resultierende Delegat einen starken Verweis auf das aktuelle Objekt enthält.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Rufen Sie für einen schwachen Verweis [ **Get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function). C++ / WinRT stellt sicher, dass der resultierende Delegat einen schwachen Verweis enthält. In letzter Minute hinter den Kulissen der Delegaten versucht, den schwachen Verweis zu einer starken aufzulösen, und nur die Member-Funktion aufruft, wenn er erfolgreich ausgeführt wird.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Ein schwacher Verweis mit **SwapChainPanel::CompositionScaleChanged**

In diesem Beispiel verwenden wir die [ **SwapChainPanel::CompositionScaleChanged** ](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) Ereignis über eine andere Darstellung der schwache Verweise. Der Code registriert einen Ereignishandler mit einem Lambda-Ausdruck, der einen schwachen Verweis an den Empfänger erfasst.

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weak_this{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strong_this{ weak_this.get() })
        {
            strong_this->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

In der Lamba-Bedingung wird eine temporäre Variable erzeugt, die eine schwache Referenz auf *this* darstellt. In der Lambda wird die Funktion **OnCompositionScaleChanged** aufgerufen, wenn eine starke Referenz auf *this* abgerufen werden kann. Auf diese Weise kann *this* innerhalb von **OnCompositionScaleChanged** sicher verwendet werden.

## <a name="weak-references-in-cwinrt"></a>Schwache Referenzen in C++/WinRT

Oben haben wir gesehen, schwache Verweise, die verwendet wird. Im Allgemeinen sind sie gut für wichtige zyklische Verweise. Beispielsweise für die native Implementierung des XAML-basierten Benutzeroberflächen-Frameworks&mdash;aufgrund der im Design des Frameworks&mdash;die schwachen Verweisen Mechanismus in C++ / WinRT ist erforderlich, um zyklische Verweise zu behandeln. Außerhalb von XAML, allerdings müssen Sie wahrscheinlich wird nicht schwache Verweise zu verwenden (nicht, dass nichts grundsätzlich XAML-spezifischen Informationen). Anstatt Sie sollten meistens, Lage entwerfen Sie Ihr c++ / WinRT-APIs in so, dass die Notwendigkeit zyklische Verweise und schwache Verweise zu vermeiden. 

Bei einem von Ihnen deklarierten Typ ist es für C++/WinRT nicht sofort ersichtlich, ob oder wann schwache Referenzen benötigt werden. Daher bietet C++/WinRT für die Strukturvorlage [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) automatisch eine Unterstützung von schwache Referenzen. Von dieser werden Ihre eigenen C++/WinRT-Typen direkt oder indirekt abgeleitet. Dies kostet Sie nichts, es sei denn, Ihr Objekt wird tatsächlich auf [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) abgefragt. Und Sie können sich explizit [gegen diese Unterstützung](#opting-out-of-weak-reference-support) entscheiden.

### <a name="code-examples"></a>Codebeispiele
Die Strukturvorlage [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) ist eine Option, um eine schwache Referenz auf eine Klasseninstanz zu erhalten.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

Oder Sie können die Hilfsfunktion [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) verwenden.

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

Die Erstellung einer schwachen Referenz hat keinen Einfluss auf die Anzahl der Referenzen auf das Objekt selbst, sondern bewirkt lediglich die Zuweisung eines Kontrollblocks. Dieser Kontrollblock kümmert sich um die Implementierung der schwachen Referenzsemantik. Sie können dann versuchen, den schwachen Verweis auf einen starken Verweis hochzustufen (und, wenn dies erfolgreich ist, ihn zu verwenden).

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

Sofern noch eine andere starke Referenz existiert, erhöht der Aufruf von [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) die Referenzanzahl und gibt die starke Referenz an den Aufrufer zurück.

### <a name="opting-out-of-weak-reference-support"></a>Opt-out der Unterstützung von schwachen Referenzen
Die Unterstützung schwacher Referenzen erfolgt automatisch. Sie können diese Unterstützung jedoch explizit deaktivieren, indem Sie die [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref)-Markerstruktur als template-Argument an Ihre Basisklasse übergeben.

Wenn Sie direkt von **winrt::implements** ableiten.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Wenn Sie eine Laufzeitklasse schreiben.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

Dabei spielt es keine Rolle, wo im Variadic-Parameterpaket die Markerstruktur erscheint. Wenn Sie eine schwache Referenz für einen Opted-Out-Typ anfordern, dann hilft Ihnen der Compiler mit der Meldung „*Dies ist nur für die Unterstützung schwacher Referenzen*”.

## <a name="important-apis"></a>Wichtige APIs
* [Implements::get_weak-Funktion](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Vorlage für WinRT::make_weak-Funktion](/uwp/cpp-ref-for-winrt/make-weak)
* [WinRT::no_weak_ref Marker-Struktur](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [Vorlage für WinRT::weak_ref-Struktur](/uwp/cpp-ref-for-winrt/weak-ref)
