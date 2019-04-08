---
description: Dieses Thema zeigt, wie man eine Komponente für Windows-Runtime erstellt, die eine Laufzeitklasse enthält, die Ereignisse auslöst. Es zeigt außerdem eine App, die die Komponente nutzt und die Ereignisse verarbeitet.
title: Erstellen von Ereignissen mit C++/WinRT
ms.date: 07/18/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, erstellen, ereignis
ms.localizationpriority: medium
ms.openlocfilehash: ace1c276b878d07f5750483740dfe90ed8cb6211
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644485"
---
# <a name="author-events-in-cwinrt"></a>Erstellen von Ereignissen mit C++/WinRT

Dieses Thema zeigt, wie man eine Komponente für Windows-Runtime erstellt, die eine Laufzeitklasse für ein Bankkonto enthält, die ein Ereignis auslöst, wenn sein Saldo ins Minus gerät. Es demonstriert außerdem eine Core App, die die Bankkonto-Laufzeitklasse nutzt, eine Funktion zur Anpassung des Saldos aufruft und alle daraus resultierenden Ereignisse verarbeitet.

> [!NOTE]
> Informationen zum Installieren und Verwenden der [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) finden Sie in Visual Studio-Erweiterung (VSIX) (die projektunterstützung für die Vorlage bereitstellt) [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe, die Ihr Verständnis für die Verwendung von Laufzeitklassen mit C++/WinRT unterstützen, finden Sie unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) und [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Erstellen einer Komponente für Windows-Runtime (BankAccountWRC)

Erstellen Sie zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie eine **Visual C++** > **Windows Universal** > **Windows-Runtime-Komponente (C++ / WinRT)** Projekt, und nennen Sie sie  *BankAccountWRC* (für "Kontoverbindung Windows-Runtime-Komponente").

Das neu erstellte Projekt enthält eine Datei mit dem Namen `Class.idl`. Benennen Sie die Datei `BankAccount.idl` (Umbenennen der `.idl` Datei umbenennt automatisch die abhängigen `.h` und `.cpp` Dateien, zu). Ersetzen Sie den Inhalt der `BankAccount.idl` mit der folgenden Auflistung.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

Speichern Sie die Datei. Erstellen des Projekts wird nicht bis zum Abschluss der Moment, aber jetzt ist ein nützlich, da es die Quellcodedateien generiert, in dem Sie implementieren, die **BankAccount** -Runtime-Klasse. Also los, und erstellen Sie jetzt (haben Sie die Buildfehler, die Sie erwarten können, um in dieser Phase finden Sie unter mit `Class.h` und `Class.g.h` nicht gefunden wurde). Während des Buildprozesses der `midl.exe` Tool wird ausgeführt, um die Komponente des Windows-Runtime-Metadatendatei zu erstellen (d.h. `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Dann wird das `cppwinrt.exe`-Tool ausgeführt (mit der Option `-component`), um Quelltextdateien zu erzeugen, die Sie bei der Erstellung Ihrer Komponente unterstützen. Diese Dateien umfassen Stubs, um Ihnen den Einstieg implementieren die **BankAccount** -Runtime-Klasse, die Sie in der IDL-Datei deklariert. Diese Stubs sind `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` und `BankAccount.cpp`.

Maustaste auf den Projektknoten, und klicken Sie auf **Ordner in Datei-Explorer öffnen**. Dadurch wird den Projektordner im Datei-Explorer geöffnet. Kopieren Sie die Stub-Dateien, `BankAccount.h` und `BankAccount.cpp` aus dem Ordner `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` und in den Ordner, die Ihre Projektdateien enthält, ist `\BankAccountWRC\BankAccountWRC\`, und Ersetzen Sie die Dateien in das Ziel. Nun öffnen wir `BankAccount.h` und `BankAccount.cpp` und implementieren unsere Laufzeitklasse. Fügen Sie in `BankAccount.h` zwei private Mitglieder zur Implementierung von BankAccount hinzu (*nicht* zur Factory-Implementierung).

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        float m_balance{ 0.f };
    };
}
...
```

Wie Sie oben sehen können, wird das Ereignis in Form von implementiert die [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) Struct-Vorlage, die durch eines bestimmten Delegattyps parametrisiert wird.

Implementieren Sie in `BankAccount.cpp` die Funktionen wie im folgenden Codebeispiel gezeigt. In C++/WinRT wird ein IDL-deklariertes Ereignis als ein Set überladener Funktionen implementiert (ähnlich wie eine Eigenschaft als ein Paar von überladenen Get- und Set-Funktionen implementiert wird). Eine Überladung übernimmt einen zu registrierenden Delegaten und gibt einen Token zurück. Die andere übernimmt einen Token und widerruft die Registrierung des zugeordneten Delegats.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token)
    {
        m_accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f) m_accountIsInDebitEvent(*this, m_balance);
    }
}
```

Sie müssen nicht die Überladung für den Ereignis-Revoker implementieren (weitere Informationen siehe [Einen registrierten Delegaten widerrufen](handle-events.md#revoke-a-registered-delegate)) – dies übernimmt die C++/WinRT-Projektion für Sie. Die anderen Überladungen sind nicht in die Projektion integriert, um Ihnen die Flexibilität zu geben, sie für Ihr Szenario optimal zu implementieren. Ein derartiger Aufruf von [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) und [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) ist eine effiziente und parallele/threadsichere Standardmethode. Wenn Sie jedoch über eine große Anzahl von Ereignissen verfügen, möchten Sie möglicherweise nicht für jedes ein Ereignisfeld, sondern vielmehr eine Implementierung mit geringer Dichte.

Sie sehen oben, dass die Implementierung der Funktion **AdjustBalance** das Ereignis **AccountIsInDebit** auslöst, wenn der Saldo negativ wird.

Wenn alle Warnungen von der Erstellung verhindern, klicken Sie dann entweder lösen oder legen Sie die Projekteigenschaft **C/C++-** > **allgemeine** > **Warnungen als Fehler behandeln** zu **No (/ WX-)**, und erstellen Sie das Projekt erneut.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Erstellen einer Core App (BankAccountCoreApp) zum Testen der Komponente für Windows-Runtime

Erstellen Sie nun ein neues Projekt (entweder in Ihrer `BankAccountWRC`-Lösung oder in einer neuen). Erstellen Sie eine **Visual C++** > **Windows Universal** > **Core-App (C++ / WinRT)** Projekt, und nennen Sie sie *BankAccountCoreApp* .

Fügen Sie einen Verweis hinzu, und navigieren Sie zu `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (oder einen Projekt-zu-Projekt-Verweis hinzufügen, wenn die beiden Projekte in der gleichen Projektmappe sind). Klicken Sie auf **Hinzufügen** und dann auf **OK**. Erstellen Sie jetzt BankAccountCoreApp. Im unwahrscheinlichen Fall, dass ein Fehler angezeigt, die die Nutzlastdatei `readme.txt` nicht vorhanden sind, schließen Sie die Datei aus dem Windows-Runtime-Komponente-Projekt, erstellen Sie ihn neu, und BankAccountCoreApp neu zu erstellen.

Während des Buildprozesses wird das `cppwinrt.exe`-Tool ausgeführt, um die referenzierte `.winmd`-Datei in Quellcodedateien zu verarbeiten, die projizierte Typen enthalten, um Sie bei der Verwendung Ihrer Komponente zu unterstützen. Der Header für die projizierten Typen für die Laufzeitklassen Ihrer Komponente (mit dem Namen `BankAccountWRC.h`) wird im Ordner `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` generiert.

Fügen Sie diesen Header in `App.cpp` ein.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

Fügen Sie in `App.cpp` ebenfalls den folgenden Code ein, um ein BankAccount zu instanziieren (unter Verwendung des Standardkonstruktors des projizierten Typs), registrieren Sie einen Ereignis-Handler und sorgen Sie dann dafür, dass das Konto ins Minus geht.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.AccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

Jedes Mal, wenn Sie auf das Fenster klicken, ziehen Sie 1 vom Kontostand ab. Um zu veranschaulichen, dass das Ereignis wie erwartet ausgelöst wird, fügen Sie einen Haltepunkt innerhalb der Lambda-Ausdruck, der verarbeitet die **AccountIsInDebit** -Ereignis, führen Sie die app, und klicken Sie in das Fenster.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Parametrisierten Delegaten und einfache Signale, über eine ABI

Wenn das Ereignis über eine anwendungsbinärdateischnittstelle (ABI) zugänglich sein muss&mdash;z. B. zwischen einer Komponente und die verarbeitende Anwendung&mdash;Ihr Ereignis muss einen Delegattyp für die Windows-Runtime verwenden. Im Beispiel oben wird der [ **Windows::Foundation::EventHandler\<T\>**  ](/uwp/api/windows.foundation.eventhandler) Windows-Runtime-Delegattyp. [**TypedEventHandler\<TSender, TResult\>**  ](/uwp/api/windows.foundation.eventhandler) ist ein weiteres Beispiel für einen Windows-Runtime-Delegattyp.

Die Typparameter für diese zwei Delegattypen müssen die ABI, überschreiten, sodass die Typparameter zu Windows-Runtime-Typen sein müssen. Dazu gehören das erste und Drittanbieter-Runtime-Klassen als auch primitive Typen wie z. B. Zahlen und Zeichenfolgen. Der Compiler hilft Ihnen, mit einer "*WinRT-Typ muss*"-Fehler aus, wenn Sie vergessen, dass diese Einschränkung.

Wenn Sie keine Parameter oder Argumente mit Ihrem Ereignis übergeben müssen, können Sie eigene einfache Windows-Runtime-Delegattyp definieren. Das folgende Beispiel zeigt eine einfachere Version des der **BankAccount** -Runtime-Klasse. Deklariert einen Delegattyp, der mit dem Namen **SignalDelegate** und dann verwendet, die zum Auslösen einer Signaltyp Ereignisses statt auf ein Ereignis mit einem Parameter.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    delegate void SignalDelegate();

    runtimeclass BankAccount
    {
        BankAccount();
        event BankAccountWRC.SignalDelegate SignalAccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

        winrt::event_token SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler);
        void SignalAccountIsInDebit(winrt::event_token const& token);
        void AdjustBalance(float value);

    private:
        winrt::event<BankAccountWRC::SignalDelegate> m_signal;
        float m_balance{ 0.f };
    };
}
```

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void BankAccount::SignalAccountIsInDebit(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f)
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
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.SignalAccountIsInDebit([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.SignalAccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Parametrisierten Delegaten, einfachen Signale und Rückrufe in einem Projekt

Wenn das Ereignis nur intern verwendet wird, innerhalb von Ihrer C++ / WinRT-Projekt (nicht für alle Binärdateien), und Sie weiterhin verwenden, die [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) Struct-Vorlage, aber Sie parametrisieren Sie es mit C++ / WinRT nicht-Windows-Runtime [ **winrt::delegate&lt;... T&gt;**  ](/uwp/cpp-ref-for-winrt/delegate) Struktur-Vorlage, die einen Delegaten effizienter und mit verweiszählung ist. Unterstützt eine beliebige Anzahl von Parametern, und sie sind nicht auf Windows-Runtime-Typen beschränkt.

Das folgende Beispiel zeigt zunächst einen Delegaten, Signatur, die keine Parameter (im Wesentlichen eine einfache Signal) verwendet werden, und klicken Sie dann eine, die eine Zeichenfolge annimmt.

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

Beachten Sie, wie Sie mit dem Ereignis hinzufügen können beliebig viele abonnierende Delegaten wie gewünscht. Es ist jedoch etwas Aufwand verbunden, die mit einem Ereignis. Wenn Sie benötigen lediglich eine einfache Rückruffunktion mit nur einem einzelnen Abonnement Delegaten, können Sie [ **winrt::delegate&lt;... T&gt;**  ](/uwp/cpp-ref-for-winrt/delegate) selbst.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Wenn Sie von einer C++ Portieren c++ / CX-Codebasis, in dem Ereignisse und Delegaten intern in einem Projekt, klicken Sie dann verwendet werden **winrt::delegate** finden Sie Informationen zu replizieren, Muster in C++ / WinRT.

## <a name="design-guidelines"></a>Entwurfsrichtlinien

Es wird empfohlen, dass Sie Ereignisse und Delegaten nicht als Parameter übergeben. Die **hinzufügen** Funktion [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) die einzige Ausnahme ist, da in diesem Fall einen Delegaten übergeben werden müssen. Der Grund für diese Richtlinie ist, da es sich bei Delegaten in verschiedenen Windows-Runtime-Sprachen (im Hinblick auf gibt an, ob sie eine Clientregistrierung, oder mehrere unterstützen) verschiedene Formen annehmen können. Ereignisse durch ihren Modell mit mehreren Abonnenten bilden eine viel besser vorhersagbare und konsistente-Option.

Die Signatur für einen Ereignishandlerdelegaten sollte bestehen aus zwei Parameter: *Absender* (**"iinspectable"**), und *Args* (einige Ereignisargumenttyp, z. B. [ **RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Beachten Sie, dass diese Richtlinien nicht unbedingt angewendet werden, wenn Sie eine interne API entwerfen. Obwohl die internen APIs im Laufe der Zeit häufig öffentlich werden.

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen von APIs mit C++/WinRT](author-apis.md)
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Behandeln von Ereignissen mithilfe von Delegaten in C++ / WinRT](handle-events.md)
