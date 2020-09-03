---
description: Dieses Thema zeigt, wie Event-Handling-Delegaten mit C++/WinRT registriert und widerrufen werden.
title: Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, projiziert, Projizierung, behandeln, Ereignis, Delegat
ms.localizationpriority: medium
ms.openlocfilehash: 2d2470b1aa52f8aa4be7e07bf1dfe5213054b005
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166244"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT

In diesem Thema erfährst du, wie du Delegaten, die Ereignisse behandeln, mit [C++/WinRT](./intro-to-using-cpp-with-winrt.md) registrierst und widerrufst. Du kannst ein Ereignis mit jedem funktionsähnlichen C++-Standardobjekt behandeln.

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="using-visual-studio-2019-to-add-an-event-handler"></a>Hinzufügen eines Ereignishandlers mithilfe von Visual Studio 2019

Mit der XAML-Designer-Benutzeroberfläche in Visual Studio 2019 lassen sich Ereignishandler einfach zu Projekten hinzufügen. Öffnen Sie im XAML-Designer die XAML-Seite, und wählen Sie das Steuerelement aus, dessen Ereignis behandelt werden soll. Klicken Sie auf der Eigenschaftenseite für dieses Steuerelement auf das Blitzsymbol, um alle Ereignisse aufzulisten, die mit dem Steuerelement verknüpft sind. Doppelklicken Sie dann auf das Ereignis, das behandelt werden soll, z. B.auf *OnClicked*.

Der XAML-Designer fügt den Quelldateien den entsprechenden Ereignishandler-Funktionsprototyp (und eine Stubimplementierung) hinzu, den Sie durch eine eigene Implementierung ersetzen können.

> [!NOTE]
> Die Ereignishandler müssen in der Regel nicht in der Midl-Datei (`.idl`) beschrieben werden. Deshalb fügt der XAML-Designer keine Ereignishandler-Funktionsprototypen zur Midl-Datei hinzu. Er fügt sie nur den `.h`- und `.cpp`-Dateien hinzu.

## <a name="register-a-delegate-to-handle-an-event"></a>Registrieren eines Delegaten zum Behandeln eines Ereignisses

Ein einfaches Beispiel ist die Behandlung des Klickereignisses einer Schaltfläche. In der Regel wird XAML-Markup verwendet, um eine Memberfunktion für die Behandlung des Ereignisses zu registrieren:

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.h
void ClickHandler(
    winrt::Windows::Foundation::IInspectable const& sender,
    winrt::Windows::UI::Xaml::RoutedEventArgs const& args);

// MainPage.cpp
void MainPage::ClickHandler(
    IInspectable const& /* sender */,
    RoutedEventArgs const& /* args */)
{
    Button().Content(box_value(L"Clicked"));
}
```

Alternativ kannst du auch imperativ eine Memberfunktion für die Behandlung eines Ereignisses registrieren, anstatt eine Deklaration im Markup zu verwenden. Es ist vielleicht nicht auf den ersten Blick ersichtlich, aber das Argument für den Aufruf [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) im folgenden Code ist eine Instanz des Delegaten [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). In diesem Fall verwenden wir die Konstruktorüberladung **RoutedEventHandler**, die ein Objekt und eine Pointer-to-Member-Funktion akzeptiert.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> Beim Registrieren des Delegaten wird im obigen Codebeispiel ein unformatierter Zeiger vom Typ *this* übergeben, der auf das aktuelle Objekt verweist. In [Wenn Sie eine Memberfunktion als Stellvertretung verwenden](weak-references.md#if-you-use-a-member-function-as-a-delegate) wird beschrieben, wie Sie einen starken oder schwachen Verweis auf das aktuelle Objekt erstellen.

Hier ist ein Beispiel, das eine statische Memberfunktion verwendet; beachten Sie die einfachere Syntax.

```cppwinrt
// MainPage.h
static void ClickHandler(
    winrt::Windows::Foundation::IInspectable const& sender,
    winrt::Windows::UI::Xaml::RoutedEventArgs const& args);

// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click( MainPage::ClickHandler );
}
void MainPage::ClickHandler(
    IInspectable const& /* sender */,
    RoutedEventArgs const& /* args */) { ... }
```

Ein Handler vom Typ **RoutedEventHandler** kann auch noch auf andere Weise erstellt werden. Der folgende Syntaxblock stammt aus dem Dokumentationsthema für [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). (Wähle rechts oben auf dieser Webseite im Dropdownmenü **Sprache** die Option *C++/WinRT* aus.) Beachte die verschiedenen Konstruktoren: Einer akzeptiert eine Lambda-Funktion, ein anderer eine freie Funktion und ein weiterer (der oben verwendete) ein Objekt sowie eine Pointer-to-Member-Funktion.

```cppwinrt
struct RoutedEventHandler : winrt::Windows::Foundation::IUnknown
{
    RoutedEventHandler(std::nullptr_t = nullptr) noexcept;
    template <typename L> RoutedEventHandler(L lambda);
    template <typename F> RoutedEventHandler(F* function);
    template <typename O, typename M> RoutedEventHandler(O* object, M method);
    void operator()(winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e) const;
};
```

Die Syntax des Funktionsaufrufoperators ist ebenfalls hilfreich. Sie gibt Aufschluss über die benötigten Delegatparameter. Wie du siehst, entspricht in diesem Fall die Syntax des Funktionsaufrufoperators den Parametern unseres Handlers vom Typ **MainPage::ClickHandler**.

> [!NOTE]
> Navigiere zum Ermitteln der Details eines Ereignisdelegaten sowie der zugehörigen Parameter zunächst zum Dokumentationsthema für das Ereignis. Nehmen wir beispielsweise das [Ereignis „UIElement.KeyDown“](/uwp/api/windows.ui.xaml.uielement.keydown). Rufe dieses Thema auf, und wähle in der Dropdownliste **Sprache** die Option  *C++/WinRT* aus. Der Syntaxblock am Anfang des Themas enthält Folgendes:
> 
> ```cppwinrt
> // Register
> event_token KeyDown(KeyEventHandler const& handler) const;
> ```
>
> Hier kannst du sehen, dass das Ereignis **UIElement.KeyDown** (das aufgerufene Thema) den Delegattyp **KeyEventHandler** besitzt, da dieser Typ beim Registrieren eines Delegaten mit diesem Ereignistyp übergeben wird. Klicke in dem Thema auf den Link zu diesem Delegattyp ([KeyEventHandler](/uwp/api/windows.ui.xaml.input.keyeventhandler)). Der dort bereitgestellte Syntaxblock enthält einen Funktionsaufrufoperator. Und dieser gibt wie bereits erwähnt Aufschluss über die benötigten Delegatparameter.
> 
> ```cppwinrt
> void operator()(
    winrt::Windows::Foundation::IInspectable const& sender,
    winrt::Windows::UI::Xaml::Input::KeyRoutedEventArgs const& e) const;
> ```
>
>  Wie du siehst, muss der Delegat deklariert werden, um ein Element vom Typ **IInspectable** als Absender und eine Instanz der [Klasse „KeyRoutedEventArgs“](/uwp/api/windows.ui.xaml.input.keyroutedeventargs) als Argumente zu akzeptieren.
>
> Sehen wir uns ein weiteres Beispiel an: das [Ereignis „Popup.Closed“](/uwp/api/windows.ui.xaml.controls.primitives.popup.closed). Sein Delegattyp ist [EventHandler\<IInspectable\>](/uwp/api/windows.foundation.eventhandler). Dein Delegat akzeptiert also ein Element vom Typ **IInspectable** als Absender und ein weiteres Element vom Typ **IInspectable** als Argumente (da es sich dabei um den Typparameter von **EventHandler** handelt).

Wenn dein Ereignishandler nicht viel zu tun hat, kannst du anstelle einer Memberfunktion eine Lambda-Funktion verwenden. Auch hier ist es vielleicht nicht auf den ersten Blick ersichtlich, aber im folgenden Codebeispiel wird ein Delegat vom Typ **RoutedEventHandler** auf der Grundlage einer Lambda-Funktion erstellt, und auch hier muss wieder die entsprechende Syntax des oben erläuterten Funktionsaufrufoperators verwendet werden.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click([this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        Button().Content(box_value(L"Clicked"));
    });
}
```

Ein Delegat kann auch etwas expliziter konstruiert werden. Beispielsweise, wenn du ihn weitergeben oder mehrmals verwenden möchtest.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const& /* args */)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    Button().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>Widerrufen eines registrierten Delegaten

Wenn du einen Delegaten registrierst, wird in der Regel ein Token zurückgegeben. Mit diesem Token kannst du später deinen Delegaten widerrufen. Dadurch wird die Registrierung des Delegaten für das Ereignis aufgehoben, und er wird nicht mehr aufgerufen, wenn das Ereignis erneut ausgelöst wird.

Dies wurde der Einfachheit halber in keinem der obigen Codebeispiele gezeigt. Im nächsten Codebeispiel wird das Token allerdings im privaten Datenmember der Struktur gespeichert und der zugehörige Handler im Destruktor widerrufen.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            // ...
        });
    }
    ~Example()
    {
        m_button.Click(m_token);
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button m_button;
    winrt::event_token m_token;
};
```

Anstelle eines starken Verweises wie im obigen Beispiel kannst du einen schwachen Verweis auf die Schaltfläche speichern (siehe [Starke und schwache Verweise in C++/WinRT](weak-references.md)).

> [!NOTE]
> Wenn eine Ereignisquelle die Ereignisse synchron auslöst, kannst du den Handler widerrufen und sicher sein, dass keine weiteren Ereignisse empfangen werden. Bei asynchronen Ereignissen kann jedoch auch nach dem Widerrufen (insbesondere bei einem Widerruf innerhalb des Destruktors) ein In-Flight-Ereignis das Objekt erreichen, nachdem mit der Zerstörung begonnen wurde. Das Problem lässt sich möglicherweise minimieren, indem die Abonnierung vor der Zerstörung aufgehoben wird. Eine stabilere Lösung findest du jedoch unter [Sicherer Zugriff auf den *this*-Zeiger in einer Klassenmember-Coroutine](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

Alternativ kannst du bei der Registrierung eines Delegaten **winrt::auto_revoke** (ein Wert vom Typ [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) angeben, um einen Ereignis-Revoker (vom Typ [**winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)) anzufordern. Der Ereignis-Revoker enthält einen schwachen Verweis auf die Ereignisquelle (das Objekt, das das Ereignis auslöst). Zum manuellen Widerrufen kannst du die Memberfunktion **event_revoker::revoke** aufrufen. Der Ereignis-Revoker ruft diese Funktion bei Verlassen des Gültigkeitsbereichs aber automatisch selbst auf. Die Funktion **revoke** überprüft, ob die Ereignisquelle noch vorhanden ist, und widerruft in diesem Fall deinen Delegaten. In diesem Beispiel muss die Ereignisquelle nicht gespeichert werden, und es besteht auch kein Bedarf für einen Destruktor.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(
            winrt::auto_revoke,
            [this](IInspectable const& /* sender */,
            RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

Der folgende Syntaxblock stammt aus dem Dokumentationsthema für das Ereignis [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Er zeigt die drei verschiedenen Registrierungs- und Widerrufsfunktionen. Anhand der dritten Überladung siehst du ganz genau, welche Art von Ereignis-Revoker du deklarieren musst. Und Sie können die gleichen Arten von Delegaten sowohl an die *Registrieren*- als auch an die *Widerrufen mit event_revoker*-Überladungen übergeben.

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
Button::Click_revoker Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

> [!NOTE]
> Im obigen Codebeispiel ist `Button::Click_revoker` ein Typalias für `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`. Ein ähnliches Muster gilt für alle C++/WinRT-Ereignisse. Jedes Windows-Runtime-Ereignis verfügt über eine Widerrufsfunktionsüberladung, die einen Ereignis-Revoker zurückgibt, und dieser Revokertyp gehört zur Ereignisquelle. Nehmen wir als weiteres Beispiel das Ereignis [**CoreWindow::SizeChanged**](/uwp/api/windows.ui.core.corewindow.sizechanged): Dieses Ereignis verfügt über eine Registrierungsfunktionsüberladung, die einen Wert vom Typ **CoreWindow::SizeChanged_revoker** zurückgibt.

Das Widerrufen von Handlern kann etwa in einem Seitennavigationsszenario sinnvoll sein. Wenn du wiederholt zu einer Seite navigierst und sie wieder verlässt, kannst du beim Verlassen der Seite alle Handler widerrufen. Bei Wiederverwendung der gleichen Seiteninstanz kannst du alternativ den Wert deines Tokens überprüfen und die Registrierung nur ausführen, wenn er noch nicht festgelegt ist (`if (!m_token){ ... }`). Eine dritte Möglichkeit besteht darin, einen Ereignis-Revoker auf der Seite als Datenmember zu speichern. Weiter unten in diesem Thema wird außerdem noch eine vierte Möglichkeit beschrieben: die Erfassung eines starken oder schwachen Verweises auf das *this*-Objekt in deiner Lambda-Funktion.

### <a name="if-your-auto-revoke-delegate-fails-to-register"></a>Wenn das Registrieren des Delegaten fehlschlägt

Wenn Sie beim Registrieren eines Delegaten [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t) angeben und das Ergebnis eine [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface)-Ausnahme ist, bedeutet dies in der Regel, dass die Ereignisquelle schwache Verweise nicht unterstützt. Diese Situation tritt z. B. häufig im [**Windows.UI.Composition**](/uwp/api/windows.ui.composition)-Namespace ein. In diesem Fall können Sie nicht den automatischen Widerruf verwenden. Sie müssen die Ereignishandler manuell widerrufen.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Delegattypen für asynchrone Aktionen und Vorgänge

In den obigen Beispielen wird der Delegattyp **RoutedEventHandler** verwendet. Es gibt aber natürlich noch viele andere Delegattypen. Asynchrone Aktionen und Vorgänge (mit und ohne Fortschritt) verfügen beispielsweise über Abschluss- und/oder Fortschrittsereignisse, die Delegaten des entsprechenden Typs erwarten. So erfordert etwa das Fortschrittsereignis eines asynchronen Vorgangs mit Fortschritt (sprich: jegliche Implementierung von [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)) einen Delegaten vom Typ [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler-2). Im Anschluss findest du ein Codebeispiel für die Erstellung eines solchen Delegattyps mit einer Lambda-Funktion. Das Beispiel veranschaulicht auch die Erstellung eines Delegaten vom Typ [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler-2).

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](
            IAsyncOperationWithProgress<SyndicationFeed,
            RetrievalProgress> const& /* sender */,
            RetrievalProgress const& args)
        {
            uint32_t bytes_retrieved = args.BytesRetrieved;
            // use bytes_retrieved;
        });

    async_op_with_progress.Completed(
        [](
            IAsyncOperationWithProgress<SyndicationFeed,
            RetrievalProgress> const& sender,
            AsyncStatus const /* asyncStatus */)
        {
            SyndicationFeed syndicationFeed = sender.GetResults();
            // use syndicationFeed;
        });

    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

Wie im obigen Kommentar zu Coroutinen bereits angedeutet, ist die Verwendung von Coroutinen für dich wahrscheinlich naheliegender als die Verwendung eines Delegaten mit den Abschlussereignissen asynchroner Aktionen und Vorgänge. Ausführlichere Informationen und Codebeispiele findest du unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md).

> [!NOTE]
> Implementiere für asynchrone Aktionen oder Vorgänge nicht mehrere *Abschlusshandler*. Du kannst entweder einen einzelnen Delegaten für das Abschlussereignis verwenden oder `co_await` dafür ausführen. Wenn Sie beide Abschlusshandler nutzen, schlägt der zweite fehl.

Wenn du dich für Delegaten und gegen eine Coroutine entscheidest, kannst du eine einfachere Syntax verwenden.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>Delegattypen, die einen Wert zurückgeben

Einige Delegattypen müssen selbst einen Wert zurückgeben. Ein Beispiel ist [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler), der eine Zeichenfolge zurückgibt. Hier ist ein Beispiel für die Erstellung eines solchen Delegaten. (Beachte, dass die Lambda-Funktion einen Wert zurückgibt.)

```cppwinrt
using namespace winrt::Windows::UI::Xaml::Controls;

winrt::hstring f(ListView listview)
{
    return ListViewPersistenceHelper::GetRelativeScrollPosition(listview, [](IInspectable const& item)
    {
        return L"key for item goes here";
    });
}
```

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Sicheres Zugreifen auf den *this*-Zeiger mit einem Delegaten für die Ereignisbehandlung

Wenn du ein Ereignis mit der Memberfunktion eines Objekts oder im Rahmen einer Lambda-Funktion innerhalb der Memberfunktion eines Objekts verarbeitest, musst du dir Gedanken zur relativen Lebensdauer des Ereignisempfängers (das Objekt, das das Ereignis behandelt) und der Ereignisquelle (das Objekt, das das Ereignis auslöst) machen. Weitere Informationen und Codebeispiele findest du unter [Starke und schwache Verweise in C++/WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

## <a name="important-apis"></a>Wichtige APIs
* [Markerstruktur „winrt::auto_revoke_t“](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [Funktion „winrt::implements::get_weak“](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [Funktion „winrt::implements::get_strong“](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen von Ereignissen in C++/WinRT](./author-events.md)
* [Parallelität und asynchrone Vorgänge mit C++/WinRT](./concurrency.md)
* [Starke und schwache Verweise in C++/WinRT](./weak-references.md)