---
description: Dieses Thema zeigt, wie eine Komponente für Windows-Runtime erstellt wird, die eine Laufzeitklasse zum Auslösen von Ereignissen enthält. Es zeigt außerdem eine App, die die Komponente nutzt und die Ereignisse verarbeitet.
title: Erstellen von Ereignissen in C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, erstellen, Ereignis
ms.localizationpriority: medium
ms.openlocfilehash: c70ad8efcb8bb84272a044824d8058813ed30def
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646233"
---
# <a name="author-events-in-cwinrt"></a>Erstellen von Ereignissen in C++/WinRT

Dieses Thema baut auf der Windows-Runtime-Komponente sowie der nutzenden Anwendung auf, deren Erstellung im Thema [Windows-Runtime-Komponenten mit C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) beschrieben wird.

Hier finden Sie die neuen Funktionen, die in diesem Thema hinzugefügt wurden.
- Aktualisierung der Thermometerlaufzeitklasse, um ein Ereignis auszulösen, wenn die Temperatur unter den Gefrierpunkt sinkt.
- Aktualisierung der Core-App, die die Thermometerlaufzeitklasse nutzt, sodass sie dieses Ereignis verarbeiten kann.

> [!NOTE]
> Informationen zum Installieren und Verwenden der Visual Studio-Erweiterung (VSIX) [C++/WinRT](./intro-to-using-cpp-with-winrt.md) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio support for C++/WinRT, XAML, the VSIX extension, and the NuGet package](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) (Visual Studio-Unterstützung für C++/WinRT, XAML, die VSIX-Erweiterung und das NuGet-Paket).

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe im Zusammenhang mit der Nutzung und Erstellung von Laufzeitklassen mit C++/WinRT findest du unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) sowie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="create-thermometerwrc-and-thermometercoreapp"></a>Erstellen von **ThermometerWRC-** und **ThermometerCoreApp**

Wenn Sie die in diesem Thema gezeigten Updates parallel durchführen möchten, damit Sie den Code erstellen und ausführen können, besteht Ihr erster Schritt darin, die exemplarische Vorgehensweise im Thema [Windows-Runtime-Komponenten mit C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) durchzuführen. Hierdurch erhalten Sie die Windows-Runtime-Komponente **ThermometerWRC** sowie die Core-App **ThermometerCoreApp-** , die diese nutzt.

## <a name="update-thermometerwrc-to-raise-an-event"></a>Aktualisieren von **ThermometerWRC**, damit sie ein Ereignis auslöst

Aktualisieren Sie `Thermometer.idl` so, dass sie wie das folgende Listung aussieht. Auf diese Weise deklarieren Sie ein Ereignis, dessen Delegattyp [**EventHandler**](/uwp/api/windows.foundation.eventhandler-1) mit einem Argument einer Gleitkommazahl mit einfacher Genauigkeit ist.

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
        event Windows.Foundation.EventHandler<Single> TemperatureIsBelowFreezing;
    };
}
```

Speichern Sie die Datei. Das Projekt wird in seinem aktuellen Zustand nicht bis zum Abschluss erstellt, aber führen Sie jetzt in jedem Fall eine Erstellung aus, um aktualisierte Versionen von `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` und den `Thermometer.cpp`-Stubdateien zu generieren. In diesen Dateien können Sie jetzt Stubimplementierungen des **TemperatureIsBelowFreezing**-Ereignisses sehen. In C++/WinRT wird ein IDL-deklariertes Ereignis als Gruppe überladener Funktionen implementiert (ähnlich wie eine Eigenschaft, die als ein Paar überladener Get- und Set-Funktionen implementiert wird). Eine Überladung akzeptiert einen zu registrierenden Delegaten und gibt ein Token zurück (ein [**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). Die andere akzeptiert ein Token und widerruft die Registrierung des zugeordneten Delegaten.

Öffnen Sie nun `Thermometer.h` und `Thermometer.cpp`, und aktualisieren Sie die Implementierung der **Thermometer**-Laufzeitklasse. Fügen Sie in `Thermometer.h` die beiden überladenen **TemperatureIsBelowFreezing**-Funktionen sowie einen privaten Ereignisdatenmember hinzu, der in der Implementierung dieser Funktionen verwendet werden soll.

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...
        winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler);
        void TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept;

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_temperatureIsBelowFreezingEvent;
        ...
    };
}
...
```

Wie oben zu sehen, wird ein Ereignis von der Strukturvorlage [**winrt::event**](/uwp/cpp-ref-for-winrt/event) dargestellt und durch einen bestimmten Delegattyp parametrisiert (der wiederum selbst durch einen Argumenttyp (args) parametrisiert werden kann).

Implementieren Sie in `Thermometer.cpp` die beiden überladenen **TemperatureIsBelowFreezing**-Funktionen.

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_temperatureIsBelowFreezingEvent.add(handler);
    }

    void Thermometer::TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept
    {
        m_temperatureIsBelowFreezingEvent.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f) m_temperatureIsBelowFreezingEvent(*this, m_temperatureFahrenheit);
    }
}
```

> [!NOTE]
> Informationen über automatische Ereignis-Revoker finden Sie unter [Einen registrierten Delegaten widerrufen](handle-events.md#revoke-a-registered-delegate). Sie erhalten eine kostenlose Implementierung des automatischen Ereignis-Revokers für das Ereignis. Das heißt, Sie müssen nicht die Überladung für den Ereignis-Revoker implementieren – er wird von der C++/WinRT-Projektion für Sie bereitgestellt.

Die anderen Überladungen (die Überladungen für Registrierung und manuellen Rückruf) sind *nicht* in die Projektion integriert. Dies bietet Ihnen die Flexibilität, sie optimal für Ihr Szenario zu implementieren. Der in diesen Implementierungen gezeigte Aufruf von [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) und [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) ist eine effiziente und parallele/threadsichere Standardmethode. Bei einer großen Anzahl von Ereignissen empfiehlt sich jedoch unter Umständen eine Implementierung mit geringer Dichte anstatt einer Implementierung, in der jedes Ereignis ein eigenes Ereignisfeld besitzt.

Oben ist außerdem zu sehen, dass die Implementierung der Funktion **AdjustTemperature** so aktualisiert wurde, dass sie nun das Ereignis **TemperatureIsBelowFreezing** auslöst, wenn die Temperatur unter den Gefrierpunkt fällt.

## <a name="update-thermometercoreapp-to-handle-the-event"></a>Aktualisieren von **ThermometerCoreApp**, damit sie das Ereignis behandelt

Nehmen Sie im **ThermometerCoreApp**-Projekt in `App.cpp` die folgenden Änderungen am Code zum Registrieren eines Ereignishandlers vor, und sorgen Sie dann dafür, dass die Temperatur unter den Gefrierpunkt sinkt.

`WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto &, float temperatureFahrenheit)
        {
            WINRT_ASSERT(temperatureFahrenheit < 32.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.TemperatureIsBelowFreezing(m_eventToken);
    }
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

Beachten Sie die Änderung an der **OnPointerPressed**-Methode. Jetzt wird bei jedem Klick auf das Fenster 1 Grad Fahrenheit von der Temperatur des Thermometers *abgezogen*. Und jetzt verarbeitet die App das Ereignis, das ausgelöst wird, wenn die Temperatur unter den Gefrierpunkt sinkt. Um zu sehen, dass das Ereignis wie erwartet ausgelöst wird, platziere einen Haltepunkt im Lambda-Ausdruck, der das Ereignis **TemperatureIsBelowFreezing** behandelt, führe die App aus, und klicke innerhalb des Fensters.

## <a name="parameterized-delegates-across-an-abi"></a>Parametrisierte Delegaten innerhalb einer ABI

Wenn dein Ereignis innerhalb einer binären Anwendungsschnittstelle (Application Binary Interface, ABI) zugänglich sein muss (etwa zwischen einer Komponente und der zugehörigen verwendenden Anwendung), muss dein Ereignis einen Windows-Runtime-Delegattyp verwenden. Im obigen Beispiel wird der Windows-Runtime-Delegattyp [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) verwendet. [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) ist ein weiteres Beispiel für einen Windows-Runtime-Delegattyp.

Da die Typparameter für diese beiden Delegattypen die ABI durchlaufen müssen, müssen die Typparameter ebenfalls Windows-Runtime-Typen sein. Dies schließt Laufzeitklassen von Windows und Drittanbietern sowie primitive Typen wie Zahlen und Zeichenfolgen mit ein. Solltest du diese Einschränkung vergessen, tritt ein Compilerfehler mit dem Hinweis auf, dass ein*WinRT-Typ erforderlich ist* .

Im Folgenden finden Sie ein Beispiel in Form von Codeauflistungen. Beginnen Sie mit den Projekten **ThermometerWRC** und **ThermometerCoreApp**, die Sie zuvor in diesem Thema erstellt haben, und bearbeiten Sie den Code in diesen Projekten so, dass er dem Code in diesen Auflistungen entspricht.

Diese erste Liste ist für das Projekt **ThermometerWRC** bestimmt. Nachdem Sie `ThermometerWRC.idl` wie unten dargestellt bearbeitet haben, erstellen Sie das Projekt, und kopieren Sie `MyEventArgs.h` und `.cpp` in das Projekt (aus dem Ordner `Generated Files`), wie Sie es zuvor mit `Thermometer.h` und `.cpp` getan haben.

```cppwinrt
// ThermometerWRC.idl
namespace ThermometerWRC
{
    [default_interface]
    runtimeclass MyEventArgs
    {
        Single TemperatureFahrenheit{ get; };
    }

    [default_interface]
    runtimeclass Thermometer
    {
        ...
        event Windows.Foundation.EventHandler<ThermometerWRC.MyEventArgs> TemperatureIsBelowFreezing;
        ...
    };
}

// MyEventArgs.h
#pragma once
#include "MyEventArgs.g.h"

namespace winrt::ThermometerWRC::implementation
{
    struct MyEventArgs : MyEventArgsT<MyEventArgs>
    {
        MyEventArgs() = default;
        MyEventArgs(float temperatureFahrenheit);
        float TemperatureFahrenheit();

    private:
        float m_temperatureFahrenheit{ 0.f };
    };
}

// MyEventArgs.cpp
#include "pch.h"
#include "MyEventArgs.h"
#include "MyEventArgs.g.cpp"

namespace winrt::ThermometerWRC::implementation
{
    MyEventArgs::MyEventArgs(float temperatureFahrenheit) : m_temperatureFahrenheit(temperatureFahrenheit)
    {
    }

    float MyEventArgs::TemperatureFahrenheit()
    {
        return m_temperatureFahrenheit;
    }
}

// Thermometer.h
...
struct Thermometer : ThermometerT<Thermometer>
{
...
    winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler);
...
private:
    winrt::event<Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs>> m_temperatureIsBelowFreezingEvent;
...
}
...

// Thermometer.cpp
#include "MyEventArgs.h"
...
winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler) { ... }
...
void Thermometer::AdjustTemperature(float deltaFahrenheit)
{
    m_temperatureFahrenheit += deltaFahrenheit;

    if (m_temperatureFahrenheit < 32.f)
    {
        auto args = winrt::make_self<winrt::ThermometerWRC::implementation::MyEventArgs>(m_temperatureFahrenheit);
        m_temperatureIsBelowFreezingEvent(*this, *args);
    }
}
...
```

Diese Liste ist für das Projekt **ThermometerCoreApp** bestimmt.

```cppwinrt
// App.cpp
...
void Initialize(CoreApplicationView const&)
{
    m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto&, ThermometerWRC::MyEventArgs args)
    {
        float degrees = args.TemperatureFahrenheit();
        WINRT_ASSERT(degrees < 32.f); // Put a breakpoint here.
    });
}
...
```

## <a name="simple-signals-across-an-abi"></a>Einfache Signale innerhalb einer ABI

Wenn du mit deinem Ereignis keine Parameter oder Argumente übergeben musst, kannst du einen eigenen einfachen Windows-Runtime-Delegattyp definieren. Das folgende Beispiel zeigt eine einfachere Version der Laufzeitklasse **Thermometer**. Darin wird ein Delegattyp namens **SignalDelegate** deklariert und anschließend verwendet, um ein Signaltypereignis auszulösen (anstatt eines Ereignisses mit einem Parameter).

```idl
// ThermometerWRC.idl
namespace ThermometerWRC
{
    delegate void SignalDelegate();

    runtimeclass Thermometer
    {
        Thermometer();
        event ThermometerWRC.SignalDelegate SignalTemperatureIsBelowFreezing;
        void AdjustTemperature(Single value);
    };
}
```

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

        winrt::event_token SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler);
        void SignalTemperatureIsBelowFreezing(winrt::event_token const& token);
        void AdjustTemperature(float deltaFahrenheit);

    private:
        winrt::event<ThermometerWRC::SignalDelegate> m_signal;
        float m_temperatureFahrenheit{ 0.f };
    };
}
```

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void Thermometer::SignalTemperatureIsBelowFreezing(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.SignalTemperatureIsBelowFreezing([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.SignalTemperatureIsBelowFreezing(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Parametrisierte Delegaten, einfache Signale und Rückrufe in einem Projekt

Wenn Sie Ereignisse benötigen, die im Rahmen Ihres Visual Studio-Projekts intern sind (nicht Binary-übergreifend) und nicht auf Windows-Runtime-Typen eingeschränkt, können Sie trotzdem die Klassenvorlage [**winrt::event**](/uwp/cpp-ref-for-winrt/event)\<Delegate\> verwenden. Verwenden Sie einfach [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) anstelle eines tatsächlichen Windows Runtime-Delegattyps, da **winrt::delegate** auch Parameter unterstützt, die nicht aus Windows Runtime stammen.

Das folgende Beispiel zeigt zuerst eine Delegatsignatur, die keine Parameter akzeptiert (also im Prinzip ein einfaches Signal), und anschließend eine, die eine Zeichenfolge akzeptiert.

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

Beachte, dass du dem Ereignis beliebig viele abonnierende Delegaten hinzufügen kannst. Für ein Ereignis ist jedoch etwas mehr Aufwand erforderlich. Wenn du lediglich einen einfachen Rückruf mit nur einem einzelnen abonnierenden Delegaten benötigst, kannst du [**winrt::delegate&lt;... T&gt;** ](/uwp/cpp-ref-for-winrt/delegate) für sich allein verwenden.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Wenn du eine C++/CX-Codebasis portierst, bei der Ereignisse und Delegaten intern innerhalb eines Projekts verwendet werden, hilft dir **winrt::delegate** dabei, dieses Muster in C++/WinRT zu replizieren.

## <a name="design-guidelines"></a>Entwurfsrichtlinien

Es wird empfohlen, als Funktionsparameter keine Delegaten, sondern Ereignisse zu übergeben. Einzige Ausnahme ist die Funktion **add** von [**winrt::event**](/uwp/cpp-ref-for-winrt/event), da in diesem Fall ein Delegat übergeben werden muss. Der Grund für diese Richtlinie: Delegaten können in unterschiedlichen Windows-Runtime-Sprachen unterschiedliche Formen haben. (Manche unterstützen nur eine einzelne Clientregistrierung, andere dagegen mehrere.) Ereignisse sind dank ihres Modells mit mehreren Abonnenten berechenbarer und konsistenter.

Die Signatur für einen Ereignishandlerdelegaten muss aus zwei Parametern bestehen: *sender* (**IInspectable**) und *args* (ein Argumenttyp, etwa [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Diese Richtlinien gelten möglicherweise nicht, wenn du eine interne API entwirfst. Interne APIs werden Laufe der Zeit allerdings häufig zu öffentlichen APIs.

## <a name="related-topics"></a>Zugehörige Themen
* [Erstellen von APIs mit C++/WinRT](author-apis.md)
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT](handle-events.md)
* [Windows-Runtime-Komponenten mit C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)