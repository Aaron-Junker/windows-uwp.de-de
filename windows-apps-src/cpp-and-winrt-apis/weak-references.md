---
description: Windows-Runtime ist ein System mit Verweiszählung. Es ist wichtig, dass Sie mit der Bedeutung und dem Unterschied zwischen starken und schwachen Verweisen vertraut sind.
title: Schwache Verweise in C++/WinRT
ms.date: 05/16/2019
ms.topic: article
keywords: Windows 10, uwp, Standard, c++, cpp, winrt, Projektion, stark, schwach, Verweis
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3ad6bb9a98b0fe2a699580001698740e44cea14f
ms.sourcegitcommit: cba3ba9b9a9f96037cfd0e07d05bd4502753c809
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2019
ms.locfileid: "67870320"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Starke und schwache Verweise in C++/WinRT

Windows-Runtime ist ein System mit Verweiszählung. Es ist wichtig, dass Sie mit der Bedeutung und dem Unterschied zwischen starken und schwachen Verweisen vertraut sind (und mit Verweisen, die keins von beidem sind, wie etwa der implizite *this*-Zeiger). Wie Sie in diesem Thema erfahren werden, kann das Wissen um den korrekten Umgang mit diesen Verweisen den Unterschied zwischen einem zuverlässigen System bedeuten, das reibungslos läuft, und einem, das zu unvorhersehbaren Abstürzen neigt. Durch die Bereitstellung von Hilfsfunktionen mit tiefgreifender Unterstützung in der Sprachprojektion kommt Ihnen [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) bei Ihrer Arbeit, komplexere Systeme einfach und ordnungsgemäß aufzubauen, auf halbem Weg entgegen.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Sicherer Zugriff auf den *this*-Zeiger in einer Klassenmember-Coroutine

Weitere Informationen zu Coroutinen und Codebeispiele finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

Die Codeauflistung unten zeigt ein typisches Beispiel einer Coroutine, die eine Memberfunktion einer Klasse ist. Sie können dieses Beispiel kopieren und es in einem neuen **Windows-Konsolenanwendung (C++/WinRT)** -Projekt in die angegebenen Dateien einfügen.

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

**MyClass::RetrieveValueAsync** arbeitet eine Zeit lang und gibt schließlich eine Kopie des `MyClass::m_value`-Datenmembers zurück. Das Aufrufen von **RetrieveValueAsync** bewirkt die Erstellung eines asynchronen Objekts, und dieses Objekt verfügt über einen impliziten *this*-Zeiger (über den schließlich der Zugriff auf `m_value` erfolgt).

Beachten Sie, dass in einer Coroutine die Ausführung bis zum ersten Anhaltepunkt, an dem die Steuerung an den Aufrufer zurückgegeben wird, synchron verläuft. In **RetrieveValueAsync** ist das erste `co_await` der erste Anhaltepunkt. Bis die Coroutine fortgesetzt wird (in diesem Fall ca. 5 Sekunden später), kann mit dem impliziten *this*-Zeiger, über den der Zugriff auf `m_value` erfolgt, alles Mögliche geschehen sein.

Hier sehen Sie die vollständige Abfolge der Ereignisse.

1. In **main** wird eine Instanz von **MyClass** erstellt (`myclass_instance`).
2. Das `async`-Objekt wird erstellt und verweist (mithilfe seines *this*) auf `myclass_instance`.
3. Die Funktion **winrt::Windows::Foundation::IAsyncAction::get** trifft auf den ersten Anhaltepunkt, wird einige Sekunden lang blockiert und gibt dann das Ergebnis von **RetrieveValueAsync** zurück.
4. **RetrieveValueAsync** gibt den Wert von `this->m_value` zurück.

Schritt 4 ist nur unbedenklich, solange *this* gültig bleibt.

Was aber, wenn die Klasseninstanz zerstört wird, bevor der asynchrone Vorgang abgeschlossen wird? Die Klasseninstanz kann auf alle möglichen Weise aus ihrem Bereich gelangen, bevor die asynchrone Methode abgeschlossen ist. Wir können das aber simulieren, indem wir die Klasseninstanz auf `nullptr` festlegen.

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

Nach dem Punkt, an dem wir die Klasseninstanz zerstören, sieht es so aus, als ob wir nicht direkt auf sie verweisen würden. Aber natürlich weist das asynchrone Objekt einen *this*-Zeiger auf und versucht, diesen zu verwenden, um den in der Klasseninstanz gespeicherten Wert zu kopieren. Die Coroutine ist eine Memberfunktion und erwartet, ihren *this*-Zeiger straffrei verwenden zu können.

Durch diese Änderung am Code bekommen wir in Schritt 4 ein Problem, da die Klasseninstanz zerstört wurde und *this* nicht mehr gültig ist. Sobald das asynchrone Objekt versucht, auf die Variable innerhalb der Klasseninstanz zuzugreifen, stürzt es ab (oder tut etwas völlig Undefiniertes).

Die Lösung besteht darin, den asynchronen Vorgang &mdash; die Coroutine &mdash; mit einem eigenen starken Verweis auf die Klasseninstanz auszustatten. So, wie sie aktuell geschrieben ist, enthält die Coroutine effektiv einen nackten *this*-Zeiger auf die Klasseninstanz, das ist aber nicht ausreichend, um die Klasseninstanz beizubehalten.

Um die Klasseninstanz beizubehalten, ändern Sie die Implementierung von **RetrieveValueAsync** in die unten abgebildete.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Eine C++/WinRT-Klasse leitet sich direkt oder indirekt aus der Vorlage [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) ab. Daher kann das C++/WinRT-Objekt seine geschützte Memberfunktion [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) aufrufen, um einen starken Verweis auf ihren *this*-Zeiger abzurufen. Beachten Sie, dass es nicht erforderlich ist, die `strong_this`-Variable im Codebeispiel oben tatsächlich zu verwenden; einfach durch das Aufrufen von **get_strong** wird der Verweiszähler des C++/WinRT-Objekts erhöht und die Gültigkeit dessen impliziten *this*-Zeigers bewahrt.

> [!IMPORTANT]
> Da **get_strong** eine Memberfunktion der Strukturvorlage **winrt::implements** ist, können Sie sie nur aus einer Klasse aufrufen, die direkt oder indirekt von **winrt::implements** abgeleitet ist, wie etwa eine C++/WinRT-Klasse. Weitere Informationen für das Ableiten aus **winrt::implements** und Beispiele finden Sie unter [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

Dies behebt das Problem, das wir zuvor beim Erreichen von Schritt 4 hatten. Selbst wenn alle anderen Verweise auf die Klasseninstanz verschwinden, hat die Coroutine Vorsichtsmaßnahmen ergriffen, um sicherzustellen, dass ihre Abhängigkeiten stabil sind.

Wenn ein starker Verweis nicht geeignet ist, können Sie stattdessen [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) aufrufen, um einen schwachen Verweis auf *this* abzurufen. Bestätigen Sie einfach, dass Sie einen starken Verweis abrufen können, bevor Sie auf *this* zugreifen. Auch **get_weak** stellt eine Memberfunktion der Strukturvorlage **winrt::implements** dar.

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

Im Beispiel oben verhindert der schwache Verweis die Zerstörung der Klasseninstanz nicht, wenn keine starken Verweise verbleiben. Sie gibt Ihnen aber die Möglichkeit, zu prüfen, ob ein starker Verweis erworben werden kann, bevor Sie auf die Membervariable zugreifen.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Sicherer Zugriff auf den *this*-Zeiger mit einer Stellvertretung zum Ereignishandling

### <a name="the-scenario"></a>Das Szenario

Allgemeine Informationen zum Ereignishandling finden Sie unter [Verarbeiten von Ereignissen über Delegaten in C++/WinRT](handle-events.md).

Im vorherigen Abschnitt wurden potenzielle Probleme mit der Lebensdauer in den Bereichen von Coroutinen und Parallelität behandelt. Wenn Sie aber ein Ereignis innerhalb der Memberfunktion eines Objekts oder innerhalb einer Lambda-Funktion innerhalb der Mitgliedsfunktion eines Objekts verarbeiten, dann müssen Sie über die relative Lebensdauer des Ereignisempfängers (das Objekt, das das Ereignis verarbeitet) und der Ereignisquelle (das Objekt, das das Ereignis auslöst) nachdenken. Sehen wir uns einige Codebeispiele an.

Die Codeauflistung unten definiert zunächst eine einfache **EventSource**-Klasse, die ein allgemeines Ereignis auslöst, das von allen Stellvertretungen verarbeitet wird, die ihr hinzugefügt wurden. In diesem Beispielereignis wird der Stellvertretungstyp [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) verwendet, die Probleme und Lösungen hier gelten aber für alle Stellvertretungstypen.

Anschließend stellt die **EventRecipient**-Klasse einen Handler für das **EventSource::Event**-Ereignis in Form einer Lambdafunktion bereit.

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

Das Muster besteht darin, dass der Ereignisempfänger einen Lambda-Ereignishandler mit Abhängigkeiten von seinem *this*-Zeiger aufweist. Immer, wenn der Ereignisempfänger länger besteht als die Ereignisquelle, besteht er länger als diese Abhängigkeiten. Und in diesen Fällen, die häufig sind, funktioniert das Muster gut. Einige dieser Fälle sind offensichtlich, z.B. wenn eine UI-Seite ein Ereignis verarbeitet, das von einem Steuerelement ausgelöst wird, das sich auf der Seite befindet. Die Seite besteht länger als die Schaltfläche &mdash; also besteht der Handler auch länger als die Schaltfläche. Dies gilt immer dann, wenn der Empfänger die Quelle besitzt (z.B. als Datenelement), oder wenn der Empfänger und die Quelle gleichgeordnet sind und sich direkt im Besitz eines anderen Objekts befinden.

Wenn Sie wissen, dass der Handler nicht länger als der *this*-Zeiger besteht, von dem er abhängt, können Sie *this* auf normale Weise erfassen, ohne eine starke oder schwache Lebensdauer zu berücksichtigen.

Aber es gibt trotzdem Fälle, in denen *this* seine Verwendung in einem Handler nicht überlebt (einschließlich Handlern für Completion- und Progress-Ereignisse, die durch asynchrone Aktionen und Vorgänge ausgelöst werden), und es ist wichtig, zu wissen, wie mit ihnen umzugehen ist.

- Wenn eine Ereignisquelle die Ereignisse *synchron* auslöst, kannst du den Handler widerrufen und sicher sein, dass keine weiteren Ereignisse empfangen werden. Bei asynchronen Ereignissen kann jedoch auch nach dem Widerrufen (insbesondere bei einem Widerruf innerhalb des Destruktors) ein In-Flight-Ereignis das Objekt erreichen, nachdem mit der Zerstörung begonnen wurde. Das Problem lässt sich möglicherweise minimieren, indem die Abonnierung vor der Zerstörung aufgehoben wird. Im Folgenden stellen wir jedoch eine stabilere Lösung vor.
- Wenn Sie eine Coroutine erstellen, um eine asynchrone Methode zu implementieren, dann ist dies möglich.
- In seltenen Fällen mit bestimmten XAML-UI-Framework-Objekten (z.B. [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel)) ist dies möglich, wenn der Empfänger finalisiert wird, ohne die Registrierung für die Ereignisquelle aufzuheben.

### <a name="the-issue"></a>Das Problem

Diese nächste Version der **main**-Funktion simuliert, was geschieht, wenn der Ereignisempfänger zerstört wird (möglicherweise überschreitet er seine Lebensdauer), während die Ereignisquelle weiterhin Ereignisse auslöst.

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

Der Ereignisempfänger wird zerstört, aber der Lambda-Ereignishandler in seinem Innern hat immer noch das **Event**Ereignis abonniert. Wenn das Ereignis ausgelöst wird, versucht die Lambda, den *this*-Zeiger zu dereferenzieren, der an diesem Punkt ungültig ist. In diesen Fällen kommt es zu einer Zugriffsverletzung durch Code in einem Handler (oder in der Fortsetzung einer Coroutine), der versucht, ihn zu verwenden.

> [!IMPORTANT]
> Wenn Sie auf eine derartige Situation stoßen, dann müssen Sie über die Lebensdauer des *this*-Objekts nachdenken. Sie müssen feststellen, ob das verwendete *this*-Objekt überlebt oder nicht. Wenn nicht, dann verwenden Sie es mit einem starken oder schwachen Verweis, wie wir unten verdeutlichen werden.
>
> Wenn es in Ihrem Szenario sinnvoll ist und wenn Threading-Überlegungen dies zulassen, dann besteht eine andere Möglichkeit darin, den Handler zu widerrufen, nachdem der Empfänger mit dem Ereignis fertig ist, bzw im Destruktor des Empfängers. Mehr dazu erfahren Sie unter [Einen registrierten Delegaten widerrufen](handle-events.md#revoke-a-registered-delegate).

Dies ist die Art, in der wir den Handler registrieren.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Die Lambda erfasst automatisch alle lokalen Variablen durch Verweis. Im Rahmen dieses Beispiels hätten wir also äquivalent dies schreiben können.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

In beiden Fällen erfassen wir lediglich den nackten *this*-Zeiger. Und das geschieht ohne Auswirkungen auf die Verweiszählung, es verhindert also nichts die Zerstörung des aktuellen Objekts.

### <a name="the-solution"></a>Die Lösung

Die Lösung besteht darin, einen starken Verweis zu erfassen (oder, wie wir sehen werden, einen schwachen Verweis, wenn ein solcher besser geeignet ist.). Ein starker Verweis *setzt* den Verweiszähler herauf, und er *hält* das aktuelle Objekt am Leben. Sie deklarieren einfach eine Erfassungsvariable (die in diesem Beispiel `strong_this` heißt) und initialisieren Sie mit einem Aufruf an [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function), die einen starken Verweis auf unseren *this*-Zeiger abruft.

> [!IMPORTANT]
> Da **get_strong** eine Memberfunktion der Strukturvorlage **winrt::implements** ist, können Sie sie nur aus einer Klasse aufrufen, die direkt oder indirekt von **winrt::implements** abgeleitet ist, wie etwa eine C++/WinRT-Klasse. Weitere Informationen für das Ableiten aus **winrt::implements** und Beispiele finden Sie unter [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Sie können sogar die automatische Erfassung des aktuellen Objekts fortlassen und über die Erfassungsvariable auf das Datenmember zugreifen, statt über den impliziten *this*-Verweis.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Wenn ein starker Verweis nicht geeignet ist, können Sie stattdessen [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) aufrufen, um einen schwachen Verweis auf *this* abzurufen. Ein schwacher Verweis behält das aktuelle Objekt *nicht* bei. Bestätige einfach, dass vor dem Zugriff auf Member weiterhin ein starker Verweis aus dem schwachen Verweis abgerufen werden kann.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

Wenn du einen Rohzeiger erfasst, musst du sicherstellen, dass das Objekt, auf das gezeigt wird, beibehalten wird.

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Wenn Sie eine Memberfunktion als Stellvertretung verwenden

Ebenso wie für Lambda-Funktionen gelten diese Prinzipien auch für die Verwendung einer Memberfunktion als Stellvertretung. Die Syntax unterscheidet sich, also werfen wir einen Blick auf etwas Code. Zunächst ist hier der potenziell unsichere Memberfunktions-Ereignishandler, der einen nackten *this*-Zeiger verwendet.

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

Dies ist das herkömmliche Standardverfahren, um auf ein Objekt und seine Memberfunktion zu verweisen. Um dies sicher zu gestalten, können Sie &mdash; beginnend mit Version 10.0.17763.0 (Windows 10, Version 1809) des Windows SDK &mdash; einen starken oder schwachen Verweis an dem Punkt einrichten, an dem der Handler registriert wird. An diesem Punkt ist bekannt, dass das Ereignisempfängerobjekt noch besteht.

Rufen Sie für einen starken Verweis einfach [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) anstelle des nackten *this*-Zeigers auf. C++/ WinRT stellt sicher, dass die resultierende Stellvertretung einen starken Verweis auf das aktuelle Objekt enthält.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Das Erfassen eines starken Verweises bedeutet, dass das Objekt erst zur Zerstörung verfügbar wird, nachdem die Registrierung des Handlers aufgehoben wurde und alle ausstehenden Rückrufe zurückgegeben wurden. Diese Garantie gilt aber nur zu dem Zeitpunkt, zu dem das Ereignis ausgelöst wird. Wenn der Ereignishandler asynchron ist, muss die Coroutine einen starken Verweis auf die Klasseninstanz vor dem ersten Anhaltepunkt erhalten (Informationen und Code dazu findest du im Abschnitt [Sicherer Zugriff auf den*this*-Zeiger in einer Klassenmember-Coroutine](#safely-accessing-the-this-pointer-in-a-class-member-coroutine) weiter oben in diesem Thema). Dadurch entsteht jedoch ein Zirkelbezug zwischen der Ereignisquelle und deinem Objekt, daher muss dieser durch Widerrufen des Ereignisses explizit unterbrochen werden.

Für einen schwachen Verweis rufen Sie [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) auf. C++/ WinRT stellt sicher, dass die resultierende Stellvertretung einen schwachen Verweis enthält. Hinter den Kulissen versucht die Stellvertretung in letzter Minute, den schwachen Verweis in einen starken aufzulösen und ruft die Memberfunktion nur auf, wenn dies Erfolg hat.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

Wenn der Delegat die Memberfunktion *tatsächlich* aufruft, behält C++/WinRT das Objekt bei, bis der Handler zurückgegeben wird. Wenn der Handler jedoch asynchron ist, wird er an Anhaltepunkten zurückgegeben, daher muss die Coroutine vor dem ersten Anhaltepunkt einen starken Verweis auf die Klasseninstanz erhalten. Weitere Informationen findest du ebenfalls im Abschnitt [Sicherer Zugriff auf den *this*-Zeiger in einer Klassenmember-Coroutine](#safely-accessing-the-this-pointer-in-a-class-member-coroutine) weiter oben in diesem Thema.

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Beispiel für einen schwachen Verweis mithilfe von **SwapChainPanel::CompositionScaleChanged**

In diesem Codebeispiel verwenden wir das Ereignis [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) als eine weitere Demonstration für schwache Verweise. Der Code registriert einen Ereignishandler unter Verwendung einer Lambda-Funktion, die einen schwachen Verweis auf den Empfänger erfasst.

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

In der Lambda-Erfassungsklausel wird eine temporäre Variable erstellt, die einen schwachen Verweis auf *this* darstellt. In der Lambda wird die Funktion **OnCompositionScaleChanged** aufgerufen, wenn ein starker Verweis auf *this* abgerufen werden kann. Auf diese Weise kann *this* innerhalb von **OnCompositionScaleChanged** sicher verwendet werden.

## <a name="weak-references-in-cwinrt"></a>Schwache Verweise in C++/WinRT

Oben haben wir die Verwendung schwacher Verweise gesehen. Im Allgemeinen sind sie zum Aufbrechen von Zirkelverweisen zu gebrauchen. Beispielsweise ist der schwache Verweismechanismus bei der nativen Implementierung des XAML-basierten UI-Frameworks aufgrund des historischen Entwurfs des Frameworks in C++/WinRT notwendig, um Zirkelverweise zu verarbeiten. Außerhalb von XAML werden Sie schwache Verweise aber vermutlich nicht verwenden müssen (nicht, dass sie etwas XAML-Spezifisches an sich hätten). Sie sollten vielmehr meistens in der Lage sein, Ihre eigenen C++/WinRT-APIs so zu gestalten, dass Zirkelverweise und schwache Verweise vermieden werden. 

Bei einem von Ihnen deklarierten Typ ist es für C++/WinRT nicht sofort ersichtlich, ob oder wann schwache Verweise benötigt werden. Daher bietet C++/WinRT für die Strukturvorlage [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) automatisch eine Unterstützung von schwachen Verweisen. Von dieser werden Ihre eigenen C++/WinRT-Typen direkt oder indirekt abgeleitet. Dies kostet Sie nichts, es sei denn, Ihr Objekt wird tatsächlich auf [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) abgefragt. Und Sie können sich explizit [gegen diese Unterstützung](#opting-out-of-weak-reference-support) entscheiden.

### <a name="code-examples"></a>Codebeispiele
Die Strukturvorlage [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) ist eine Option, um einen schwachen Verweis auf eine Klasseninstanz zu erhalten.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

Oder Sie können die Hilfsfunktion [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) verwenden.

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

Die Erstellung eines schwachen Verweises hat keinen Einfluss auf die Anzahl der Verweise auf das Objekt selbst, sondern bewirkt lediglich die Zuweisung eines Kontrollblocks. Dieser Kontrollblock kümmert sich um die Implementierung der schwachen Verweissemantik. Sie können dann versuchen, den schwachen Verweis auf einen starken Verweis hochzustufen (und, wenn dies erfolgreich ist, ihn zu verwenden).

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

Sofern noch ein anderer starker Verweis existiert, erhöht der Aufruf von [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) den Verweiszähler und gibt den starken Verweis an den Aufrufer zurück.

### <a name="opting-out-of-weak-reference-support"></a>Verzicht auf die Unterstützung von schwachen Verweisen
Die Unterstützung schwacher Verweise erfolgt automatisch. Sie können diese Unterstützung jedoch explizit deaktivieren, indem Sie die [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref)-Markerstruktur als Vorlagenargument an Ihre Basisklasse übergeben.

Wenn Sie direkt von **winrt::implements** ableiten.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Wenn Sie eine Laufzeitklasse erstellen.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

Dabei spielt es keine Rolle, wo im variadic-Parameterpaket die Markerstruktur verwendet wird. Wenn Sie einen schwachen Verweis für einen Opted-Out-Typ anfordern, dann hilft Ihnen der Compiler mit der Meldung „*Dies ist nur für die Unterstützung schwacher Verweise*“.

## <a name="important-apis"></a>Wichtige APIs
* [implements::get_weak-Funktion](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak-Funktionsvorlage](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref Markerstruktur](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref-Strukturvorlage](/uwp/cpp-ref-for-winrt/weak-ref)
