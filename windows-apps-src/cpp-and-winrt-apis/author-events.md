---
description: Dieses Thema zeigt, wie eine Komponente für Windows-Runtime erstellt wird, die eine Laufzeitklasse zum Auslösen von Ereignissen enthält. Es zeigt außerdem eine App, die die Komponente nutzt und die Ereignisse verarbeitet.
title: Erstellen von Ereignissen in C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, erstellen, Ereignis
ms.localizationpriority: medium
ms.openlocfilehash: e8bb86bd8d52ff96f010bf41758f1e4602330d52
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393479"
---
# <a name="author-events-in-cwinrt"></a>Erstellen von Ereignissen in C++/WinRT

In diesem Thema erfährst du, wie du eine Komponente für Windows-Runtime erstellst, die eine Laufzeitklasse für ein Bankkonto enthält und ein Ereignis auslöst, wenn der Saldo negativ wird. Außerdem wird eine Core-App gezeigt, die die Laufzeitklasse „BankAccount“ nutzt, eine Funktion zur Anpassung des Saldos aufruft und alle daraus resultierenden Ereignisse verarbeitet.

> [!NOTE]
> Informationen zum Installieren und Verwenden der Visual Studio-Erweiterung (VSIX) [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio support for C++/WinRT, XAML, the VSIX extension, and the NuGet package](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) (Visual Studio-Unterstützung für C++/WinRT, XAML, die VSIX-Erweiterung und das NuGet-Paket).

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe im Zusammenhang mit der Nutzung und Erstellung von Laufzeitklassen mit C++/WinRT findest du unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) sowie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Erstellen einer Komponente für Windows-Runtime (BankAccountWRC)

Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Erstelle ein Projekt vom Typ **Leere App (C++/WinRT)** , und nenne es *BankAccountWRC* (Bankkonto-Komponente für Windows-Runtime). Führe noch keinen Buildvorgang für das Projekt aus.

Das neu erstellte Projekt enthält eine Datei namens `Class.idl`. Ändere den Namen der Datei in `BankAccount.idl`. (Durch die Umbenennung der Datei vom Typ `.idl` werden automatisch auch die abhängigen Dateien `.h` und `.cpp` umbenannt.) Ersetze den Inhalt von `BankAccount.idl` durch das folgende Listing:

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

Speichern Sie die Datei. Im aktuellen Zustand wird das Projekt zwar nicht vollständig erstellt, die Erstellung ist jedoch hilfreich, da dadurch die Quellcodedateien generiert werden, in denen die Laufzeitklasse **BankAccount** implementiert wird. Erstelle daher als Nächstes das Projekt. (Die in dieser Phase zu erwartenden Buildfehler sind darauf zurückzuführen, dass `Class.h` und `Class.g.h` nicht gefunden wurden.)

Während des Buildprozesses wird das Tool `midl.exe` ausgeführt, um die Windows-Runtime-Metadatendatei (`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`) deiner Komponente zu erstellen. Danach wird das Tool `cppwinrt.exe` (mit der Option `-component`) ausgeführt, um Quellcodedateien zu generieren, die dich bei der Erstellung deiner Komponente unterstützen. Diese Dateien enthalten Stubs zur Implementierung der Laufzeitklasse **BankAccount**, die du in deiner IDL deklariert hast. Diese Stubs sind `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` und `BankAccount.cpp`.

Klicke mit der rechten Maustaste auf den Projektknoten, und klicke auf **Ordner in Datei-Explorer öffnen**. Dadurch wird der Projektordner im Datei-Explorer geöffnet. Kopiere dort die Stub-Dateien `BankAccount.h` und `BankAccount.cpp` aus dem Ordner `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` in den Ordner mit deinen Projektdateien (`\BankAccountWRC\BankAccountWRC\`), und ersetze die Dateien am Ziel. Als Nächstes öffnen wir `BankAccount.h` und `BankAccount.cpp` und implementieren unsere Laufzeitklasse. Füge der Implementierung von „BankAccount“ (*nicht* der Factory-Implementierung) in `BankAccount.h` zwei private Member hinzu.

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

Wie oben zu sehen, wird das Ereignis als Strukturvorlage [**winrt::event**](/uwp/cpp-ref-for-winrt/event) implementiert und durch einen bestimmten Delegattyp parametrisiert.

Implementiere in `BankAccount.cpp` die Funktionen, wie im folgenden Codebeispiel gezeigt. In C++/WinRT wird ein IDL-deklariertes Ereignis als Gruppe überladener Funktionen implementiert (ähnlich wie eine Eigenschaft, die als ein Paar überladener Get- und Set-Funktionen implementiert wird). Eine Überladung akzeptiert einen zu registrierenden Delegaten und gibt ein Token zurück. Die andere akzeptiert ein Token und widerruft die Registrierung des zugeordneten Delegaten.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token) noexcept
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

> [!NOTE]
> Informationen über automatische Ereignis-Revoker finden Sie unter [Einen registrierten Delegaten widerrufen](handle-events.md#revoke-a-registered-delegate). Sie erhalten eine kostenlose Implementierung des automatischen Ereignis-Revokers für das Ereignis. Das heißt, Sie müssen nicht die Überladung für den Ereignis-Revoker implementieren – er wird von der C++/WinRT-Projektion für Sie bereitgestellt.

Die anderen Überladungen (die Überladungen für Registrierung und manuellen Rückruf) sind *nicht* in die Projektion integriert. Dies bietet Ihnen die Flexibilität, sie optimal für Ihr Szenario zu implementieren. Der in diesen Implementierungen gezeigte Aufruf von [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) und [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) ist eine effiziente und parallele/threadsichere Standardmethode. Bei einer großen Anzahl von Ereignissen empfiehlt sich jedoch unter Umständen eine Implementierung mit geringer Dichte anstatt einer Implementierung, in der jedes Ereignis ein eigenes Ereignisfeld besitzt.

Oben ist außerdem zu sehen, dass die Implementierung der Funktion **AdjustBalance** das Ereignis **AccountIsInDebit** auslöst, wenn der Saldo negativ wird.

Sollte die Erstellung aufgrund von Warnungen nicht möglich sein, behebe entweder die Ursachen, oder lege die Projekteigenschaft **C/C++**  > **Allgemein** > **Warnungen als Fehler behandeln** auf **Nein (/WX-)** fest, und erstelle das Projekt erneut.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Erstellen einer Core-App (BankAccountCoreApp) zum Testen der Komponente für Windows-Runtime

Erstelle ein neues Projekt (entweder in der Projektmappe `BankAccountWRC`oder in einer neuen Projektmappe). Erstelle ein Projekt vom Typ **Core-App (C++/WinRT)** , und nenne es *BankAccountCoreApp*.

Füge einen Verweis hinzu, und navigiere zu `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (oder füge einen Verweis von Projekt zu Projekt hinzu, falls sich die beiden Projekte in der gleichen Projektmappe befinden). Klicke auf **Hinzufügen** und anschließend auf **OK**. Führe als Nächstes einen Buildvorgang für „BankAccountCoreApp“ aus. Sollte ein Fehler mit dem Hinweis angezeigt werden, dass die Nutzlastdatei `readme.txt` nicht vorhanden ist, schließe diese Datei aus dem Projekt „Komponente für Windows-Runtime“ aus, erstelle es neu, und erstelle anschließend „BankAccountCoreApp“ neu. (Dieser Fehler ist allerdings nicht sehr wahrscheinlich.)

Während des Buildprozesses wird das Tool `cppwinrt.exe` ausgeführt, um die referenzierte Datei vom Typ `.winmd` zu Quellcodedateien mit projizierten Typen zu verarbeiten, die dich bei der Nutzung deiner Komponente unterstützen. Der Header für die projizierten Typen für die Laufzeitklassen deiner Komponente (`BankAccountWRC.h`) wird im Ordner `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` generiert.

Schließe diesen Header in `App.cpp` ein.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

Füge in `App.cpp` außerdem den folgenden Code hinzu, um ein Bankkonto zu instanziieren (unter Verwendung des Standardkonstruktors des projizierten Typs), registriere einen Ereignishandler, und sorge dann dafür, dass das Konto ins Minus geht.

`WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

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
            WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
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

Bei jedem Klick auf das Fenster wird der Saldo des Bankkontos um „1“ verringert. Um zu sehen, dass das Ereignis wie erwartet ausgelöst wird, platziere einen Haltepunkt im Lambda-Ausdruck, der das Ereignis **AccountIsInDebit** behandelt, führe die App aus, und klicke innerhalb des Fensters.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Parametrisierte Delegaten und einfache Signale innerhalb einer ABI

Wenn dein Ereignis innerhalb einer binären Anwendungsschnittstelle (Application Binary Interface, ABI) zugänglich sein muss (etwa zwischen einer Komponente und der zugehörigen verwendenden Anwendung), muss dein Ereignis einen Windows-Runtime-Delegattyp verwenden. Im obigen Beispiel wird der Windows-Runtime-Delegattyp [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) verwendet. [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) ist ein weiteres Beispiel für einen Windows-Runtime-Delegattyp.

Da die Typparameter für diese beiden Delegattypen die ABI durchlaufen müssen, müssen die Typparameter ebenfalls Windows-Runtime-Typen sein. Dies schließt Laufzeitklassen von Erst- und Drittanbietern sowie primitive Typen wie Zahlen und Zeichenfolgen mit ein. Solltest du diese Einschränkung vergessen, tritt ein Compilerfehler mit dem Hinweis auf, dass ein*WinRT-Typ erforderlich ist* .

Wenn du mit deinem Ereignis keine Parameter oder Argumente übergeben musst, kannst du einen eigenen einfachen Windows-Runtime-Delegattyp definieren. Das folgende Beispiel zeigt eine einfachere Version der Laufzeitklasse **BankAccount**. Darin wird ein Delegattyp namens **SignalDelegate** deklariert und anschließend verwendet, um ein Signaltypereignis auszulösen (anstatt eines Ereignisses mit einem Parameter).

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Parametrisierte Delegaten, einfache Signale und Rückrufe in einem Projekt

Wenn dein Ereignis nicht binärdateiübergreifend, sondern nur intern innerhalb deines C++/WinRT-Projekts verwendet wird, verwendest du zwar weiterhin die Strukturvorlage [**winrt::event**](/uwp/cpp-ref-for-winrt/event), parametrisierst sie aber mit der Windows-Runtime-fremden C++/WinRT-Strukturvorlage [**winrt::delegate&lt;... T&gt;** ](/uwp/cpp-ref-for-winrt/delegate). Dabei handelt es sich um einen effizienten Delegaten mit Verweiszählung. Er unterstützt eine beliebige Anzahl von Parametern, und diese sind nicht auf Windows-Runtime-Typen beschränkt.

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

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen von APIs mit C++/WinRT](author-apis.md)
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT](handle-events.md)
