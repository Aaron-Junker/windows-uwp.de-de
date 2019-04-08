---
description: Dieses Thema zeigt, wie man Event-Handling-Delegaten mit C++/WinRT registriert und widerruft.
title: Verarbeiten von Ereignissen über Delegaten in C++/WinRT
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projiziert, projizierung, varbeiten, ereignis, delegat
ms.localizationpriority: medium
ms.openlocfilehash: 193d821b44722e150f38da7430504f5d528770a4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602425"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>Verarbeiten von Ereignissen über Delegaten in C++/WinRT

In diesem Thema wird gezeigt, wie zum Registrieren und Sperren für die Ereignisbehandlung Delegaten mit [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Sie können ein Ereignis mit jedem Objekt verarbeiten, das einer normalen C++ Funktion entspricht.

> [!NOTE]
> Informationen zum Installieren und Verwenden von C++ / WinRT Visual Studio-Erweiterung (VSIX) (die bereitstellt, projektunterstützung für die Vorlage als auch C++ / WinRT-MSBuild-Eigenschaften und Ziele) finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="register-a-delegate-to-handle-an-event"></a>Einen Delegaten für die Verarbeitung eines Ereignisses registrieren

Ein einfaches Beispiel ist die Verarbeitung des Klickereignisses einer Schaltfläche. Es ist typisch, XAML-Markup zu verwenden, um eine Member-Funktion zu registrieren, um das Ereignis wie folgt zu verarbeiten.

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
{
    Button().Content(box_value(L"Clicked"));
}
```

Anstatt dies im Markup zu deklarieren, können Sie explizit eine Member-Funktion registrieren, um ein Ereignis zu verarbeiten. Es mag nicht offensichtlich sein, aber das Argument für den [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Aufruf ist eine Instanz des [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler)-Delegaten. In diesem Fall verwenden wir die Überladung des **RoutedEventHandler**-Konstruktors, die ein Objekt und eine Pointer-to-Member-Funktion benötigt.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> Bei der Registrierung des Delegats übergibt im obenstehenden Codebeispiel wird eine unformatierte *dies* Zeiger (zeigt auf das aktuelle Objekt). Um zu erfahren, wie Sie einen starken oder einen schwachen Verweis auf das aktuelle Objekt herzustellen, finden Sie unter den **bei Verwendung eine Memberfunktion als Delegat** Unterabschnitt im Abschnitt [problemlos den Zugriff auf die *dies* Zeiger mit einem Delegaten zur Verarbeitung von Ereignissen](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

Es gibt andere Möglichkeiten, ein **RoutedEventHandler** zu erstellen. Unten finden Sie den Syntaxblock aus dem Dokumentationsthema für [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (wählen Sie *C++/WinRT* im Dropdown-Menü **Sprache** auf der Seite aus). Beachten Sie die verschiedenen Konstruktoren: Einer nimmt ein Lambda entgegen (ein anderer eine freie Funktion) und ein weiterer (der oben verwendete) nimmt ein Objekt und eine Pointer-to-Member-Funktion entgegen.

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

Die Syntax des Funktionsaufrufoperators ist ebenfalls hilfreich. Sie sagt Ihnen, welche Parameter Ihr Delegat haben muss. Wie Sie sehen, entspricht in diesem Fall die Syntax des Funktionsaufrufoperators den Parametern unseres **MainPage::ClickHandler**.

Wenn Sie nicht viel in Ihrem Ereignis-Handler erledigen, dann können Sie eine Lambda-Funktion anstelle einer Mitgliedsfunktion verwenden. Auch hier ist es vielleicht nicht offensichtlich, aber ein **RoutedEventHandler**-Delegat wird aus einer Lambda-Funktion erstellt, die wiederum der Syntax des Funktionsaufrufoperators entsprechen muss.

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

Sie können sich dafür entscheiden, etwas konkreter zu werden, wenn Sie Ihren Delegaten konstruieren. Beispielsweise bei der Weitergabe oder der mehrfachen Verwendung.

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

## <a name="revoke-a-registered-delegate"></a>Einen registrierten Delgaten widerrufen

Wenn Sie einen Deleganten registrieren, wird Ihnen in der Regel ein Token zurückgegeben. Mit diesem Token können Sie anschließend Ihren Delegaten widerrufen, d. h. die Registrierung des Delegaten für das Ereignis wird aufgehoben. Er wird nicht mehr aufgerufen, wenn das Ereignis erneut ausgelöst wird. Der Einfachheit halber hat keines der obigen Codebeispiele gezeigt, wie das geht. Das nächste Codebeispiel speichert das Token jedoch im privaten Datenelement der Struktur und widerruft seinen Handler im Destruktor.

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

Statt einen starken Verweis, wie im obigen Beispiel können Sie einen schwachen Verweis auf die Schaltfläche zum Speichern (siehe [starke und schwache Verweise in C++ / WinRT](weak-references.md)).

Wenn Sie einen Delegaten registrieren, Sie können auch angeben **winrt::auto_revoke** (Dies ist ein Wert vom Typ [ **winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) ein Ereignis zugeordneter Rücknahmeschlüssel (der anfordern Typ [ **winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)). Das Ereignis zugeordneter Rücknahmeschlüssel enthält einen schwachen Verweis auf die Ereignisquelle (das Objekt, das das Ereignis auslöst), für Sie. Sie können manuell widerrufen, indem Sie die Mitgliedsfunktion **event_revoker::revoke** aufrufen; der Event-Revoker ruft diese Funktion aber selbst automatisch auf, wenn sie ihren Gültigkeitsbereich verlässt. Die **revoke**-Funktion überprüft, ob die Ereignisquelle noch vorhanden ist, und widerruft in diesem Fall den Delegaten. In diesem Beispiel gibt es keine Notwendigkeit, die Ereignisquelle zu speichern und daher auch keinen Bedarf für einen Destruktor.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

Unten ist der Syntaxblock aus dem Dokumentationsthema für das [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis zu sehen. Es zeigt die drei verschiedenen Registrierungs- und Widerrufsfunktionen. Sie können genau sehen, welche Art von Event-Revoker Sie für die dritten Überladung deklarieren müssen.

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
> Im obigen Codebeispiel `Button::Click_revoker` ist ein Typalias für `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`. Ein ähnliches Muster gilt für alle C++/WinRT-Ereignisse. Jede Windows-Runtime-Ereignis verfügt über eine Überladung der Revoke-Funktion, die ein Ereignis zugeordneter Rücknahmeschlüssel zurückgibt, und die zugeordneter Rücknahmeschlüssel-Typ ist ein Mitglied der Ereignisquelle. Wenn wir also, ein weiteres Beispiel, das [ **CoreWindow::SizeChanged** ](/uwp/api/windows.ui.core.corewindow.sizechanged) Ereignis verfügt über eine Registrierung funktionsüberladung, die einen Wert vom Typ zurückgibt **CoreWindow::SizeChanged_revoker**.


In einem Seitennavigationsszenario kann es sinnvoll sein, Handler zu widerrufen. Wenn Sie wiederholt auf eine Seite navigieren und diese dann wieder verlassen, können Sie alle Handler widerrufen, wenn Sie von der Seite weg navigieren. Wenn Sie dieselbe Seiteninstanz wiederverwenden, dann überprüfen Sie alternativ den Wert Ihres Tokens und registrieren Sie sich nur, wenn er noch nicht festgelegt ist (`if (!m_token){ ... }`). Eine dritte Möglichkeit besteht darin, einen Event-Revoker als Datenelement der Seite zu speichern. Und eine vierte Möglichkeit (wird später in diesem Thema beschrieben) besteht darin, eine starke oder schwache Referenz auf das *this*-Objekt in Ihrer Lambda-Funktion zu verwenden.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Delegattypen für asynchrone Aktionen und Vorgänge

Die obigen Beispiele verwenden den Delegattyp **RoutedEventHandler**, aber es gibt natürlich auch viele andere Delegattypen. Beispielsweise haben asynchrone Aktionen und Vorgänge (mit und ohne Fortschritt) Completed- und/oder Progress-Ereignisse, die Delegaten des entsprechenden Typs erwarten. Beispielsweise erfordert das Progress-Ereignis eines asynchronen Vorgangs mit Fortschritt (betrifft alles, das [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) implementiert) einen Delegaten vom Typ [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler). Hier ist ein Codebeispiel für die Erstellung eines solchen Delegaten mit einer Lambda-Funktion. Das Beispiel zeigt auch, wie man einen [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler)-Delegaten erstellt.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& /* sender */, RetrievalProgress const& args)
    {
        uint32_t bytes_retrieved = args.BytesRetrieved;
        // use bytes_retrieved;
    });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const /* asyncStatus */)
    {
        SyndicationFeed syndicationFeed = sender.GetResults();
        // use syndicationFeed;
    });
    
    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

Wie aus dem obigen „coroutine“-Kommentar hervorgeht, werden Sie es wahrscheinlich natürlicher finden, Coroutinen zu verwenden, anstatt einen Delegaten für die Completed-Ereignisse asynchroner Aktionen und Vorgänge zu verwenden. Details und Codebeispiele finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md).

> [!NOTE]
> Es ist nicht korrekt implementiert mehrere *Abschlusshandler* für eine asynchrone Aktion oder Operation. Sie können entweder einen einzelnen Delegaten für das abgeschlossene Ereignis verwenden, oder Sie können `co_await` es. Wenn Sie beides haben, misslingt die zweite.

Wenn Sie Delegaten anstelle einer Coroutine weiterhin verwenden, können Sie für eine einfachere Syntax optional.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>Delegattypen, die einen Wert zurückgeben

Einige Delegattypen müssen selbst einen Wert zurückgeben. Ein Beispiel ist [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler), der einen String zurückgibt. Hier ist ein Beispiel für die Erstellung eines solchen Delegaten (beachten Sie, dass die Lambda-Funktion einen Wert zurückgibt).

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

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Sicheren Zugriff auf die *dies* Zeiger durch einen Delegaten zur Verarbeitung von Ereignissen

Wenn Sie ein Ereignis mit einer Objektmemberfunktion behandeln oder aus in einer Lambda-Funktion innerhalb eines Objekts-Memberfunktion, Sie die relative Lebensdauer von der Ereignis-Empfänger (das Objekt, das das Ereignis zu behandeln) und die Ereignisquelle (das Objekt berücksichtigen müssen Auslösen des Ereignisses). Weitere Informationen und Codebeispiele, finden Sie unter [starke und schwache Verweise in C++ / WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

## <a name="important-apis"></a>Wichtige APIs
* [WinRT::auto_revoke_t Marker-Struktur](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [WinRT::Implements::get_weak-Funktion](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [WinRT::Implements::get_strong-Funktion](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen von Ereignissen in C++ / WinRT](author-events.md)
* [Parallelität und asynchrone Vorgänge mit C++ / WinRT](concurrency.md)
* [Starke und schwache Verweise in C++/WinRT](weak-references.md)
