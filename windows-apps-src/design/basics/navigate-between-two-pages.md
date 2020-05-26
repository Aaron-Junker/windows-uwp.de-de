---
Description: Hier erfahren Sie, wie Sie die Peer-zu-Peer-Navigation zwischen zwei einfachen Seiten in einer Windows-App ermöglichen.
title: Peer-zu-Peer-Navigation zwischen zwei Seiten
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.date: 07/13/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 50211600931fc67c43aa577fabe23f1277a0e897
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83233825"
---
# <a name="implement-navigation-between-two-pages"></a>Implementieren der Navigation zwischen zwei Seiten

Hier erfährst du, wie du einen Frame und Seiten verwendest, um in deiner App eine einfache Peer-zu-Peer-Navigation zu ermöglichen. 

> **Wichtige APIs:** Klasse [**Windows.UI.Xaml.Controls.Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame), Klasse [**Windows.UI.Xaml.Controls.Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page), Namespace [**Windows.UI.Xaml.Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation)

![Peer-to-Peer-Navigation](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. Erstellen einer leeren App

1.  Wähle im Menü von Microsoft Visual Studio **Datei** > **Neues Projekt** aus.
2.  Wähle im linken Bereich des Dialogfelds **Neues Projekt** den Knoten **Visual C#**  > **Windows** > **Universell** oder **Visual C++**  > **Windows** > **Universell** aus.
3.  Wählen Sie im mittleren Bereich die Option **Leere App** aus.
4.  Geben Sie in das Feld **Name** den Wert **NavApp1** ein, und klicken Sie anschließend auf **OK**.
    Die Projektmappe wird erstellt, und die Projektdateien werden im **Projektmappen-Explorer** angezeigt.
5.  Klicken Sie zum Ausführen des Programms im Menü auf **Debuggen** > **Debugging starten**, oder drücken Sie F5.
    Es wird eine leere Seite angezeigt.
6.  Um das Debuggen zu beenden und zu Visual Studio zurückzukehren, kannst du entweder die App beenden oder im Menü auf **Debuggen beenden** klicken.

## <a name="2-add-basic-pages"></a>2. Hinzufügen von Standardseiten

Füge als Nächstes deinem Projekt zwei Seiten hinzu.

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten **BlankApp**, um das Kontextmenü zu öffnen.
2.  Wählen Sie im Kontextmenü **Hinzufügen** > **Neues Element** aus.
3.  Wählen Sie im Dialogfeld **Neues Element hinzufügen** im mittleren Bereich die Option **Leere Seite** aus.
4.  Geben Sie in das Feld **Name** den Wert **Page1** (oder **Page2**) ein, und wählen Sie anschließend **Hinzufügen**.
5. Wiederhole die Schritte 1 bis 4, um die zweite Seite hinzuzufügen.

Die Dateiliste des Projekts „NavApp1“ sollte nun folgende Dateien enthalten:

<table>
<thead>
<tr class="header">
<th align="left">C#</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h

</li>
</ul></td>
</tr>
</tbody>
</table>

Füge in „Page1.xaml“ den folgenden Inhalt hinzu:

-   Ein [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element mit der Bezeichnung `pageTitle` als untergeordnetes Element des [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)-Stammelements. Ändern Sie die [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text)-Eigenschaft in `Page 1`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   Ein [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)-Element als untergeordnetes Element des [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)-Stammelements (nach dem `pageTitle` [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element).
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Füge in der CodeBehind-Datei „Page1.xaml“ den folgenden Code hinzu, um das `Click`-Ereignis des [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)-Elements zu behandeln, das du für die Navigation zu „Page2.xaml“ hinzugefügt hast.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>());
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

Füge in „Page2.xaml“ den folgenden Inhalt hinzu:

-   Ein [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element mit der Bezeichnung `pageTitle` als untergeordnetes Element des [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)-Stammelements. Ändern Sie den Wert der [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text)-Eigenschaft in `Page 2`:
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   Ein [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)-Element als untergeordnetes Element des [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)-Stammelements (nach dem `pageTitle` [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element).
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Füge in der CodeBehind-Datei „Page2.xaml“ den folgenden Code hinzu, um das `Click`-Ereignis des [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)-Elements für die Navigation zu „Page1.xaml“ zu behandeln.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

```cppwinrt
void Page2::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page1>());
}
```

```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

> [!NOTE]
> Im Fall von C++-Projekten müssen Sie in der Headerdatei jeder Seite, die auf eine andere Seite verweist, eine `#include`-Anweisung hinzufügen. Für das hier gezeigte Beispiel für die seitenübergreifende Navigation enthält die Datei „page1.xaml.h“ `#include "Page2.xaml.h"`, und die Datei „page2.xaml.h“ wiederum enthält `#include "Page1.xaml.h"`.

Nachdem wir die Seiten vorbereitet haben, müssen wir dafür sorgen, dass beim Start der App „Page1.xaml“ angezeigt wird.

Öffne die CodeBehind-Datei „App.xaml“, und ändere den `OnLaunched`-Handler.

Hier geben wir `Page1` im Aufruf von [**Frame.Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) anstelle von `MainPage` an.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
 
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();
        rootFrame.NavigationFailed += OnNavigationFailed;
 
        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }
 
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
 
    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = Frame();

        rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
        {
            // Restore the saved session state only when appropriate, scheduling the
            // final launch steps after the restore is complete
        }

        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Place the frame in the current Window
            Window::Current().Content(rootFrame);
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
    else
    {
        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
{
    auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = ref new Frame();

        rootFrame->NavigationFailed += 
            ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
                this, &App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }
        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;
    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

> [!NOTE]
> In diesem Beispielcode wird der Rückgabewert von [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) verwendet, um eine App-Ausnahme auszulösen, wenn die Navigation zum anfänglichen Fensterrahmen der App einen Fehler verursacht. Wenn **Navigate** den Wert **true** zurückgibt, findet die Navigation statt.

Erstellen Sie nun die App, und führen Sie sie aus. Klicken Sie auf den Link „Click to go to page 2“. Die zweite Seite mit der Bezeichnung „Seite 2“ wird geladen und im Frame angezeigt.

### <a name="about-the-frame-and-page-classes"></a>Informationen zur Frame- und Page-Klasse

Bevor wir der App weitere Funktionen hinzufügen, sehen wir uns zunächst an, wie die hinzugefügten Seiten die Navigation in unserer App ermöglichen.

Zuerst wird in der CodeBehind-Datei „App.xaml“ in der Methode `App.OnLaunched` eine [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)-Klasse namens `rootFrame` für die App erstellt. Die **Frame**-Klasse unterstützt verschiedene Navigationsmethoden wie [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate), [**GoBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback)und [**GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward) sowie Eigenschaften wie [**BackStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack), [**ForwardStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.forwardstack) und [**BackStackDepth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstackdepth).
 
Die [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate)-Methode wird zum Anzeigen von Inhalt im **Frame** verwendet. Standardmäßig lädt diese Methode „MainPage.xaml“. In unserem Beispiel wird `Page1` an die Methode **Navigate** übergeben, sodass durch die Methode `Page1` im **Frame** geladen wird. 

`Page1` ist eine Unterklasse der [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)-Klasse. Die **Page**-Klasse verfügt über eine schreibgeschützte **Frame**-Eigenschaft, die den **Frame** abruft, der **Page** enthält. Wenn der **Click**-Ereignishandler von **HyperlinkButton**`this.Frame.Navigate(typeof(Page2))` in `Page1` aufruft, zeigt das **Frame**-Element den Inhalt von „Page2.xaml“ an.

Und wenn eine Seite in den Frame geladen wird, wird sie als [**PageStackEntry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation.PageStackEntry) zu [**BackStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack) oder [**ForwardStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.forwardstack) des [**Frame**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.frame)-Elements hinzugefügt, um die Verwendung von [Verlauf und Rückwärtsnavigation](navigation-history-and-backwards-navigation.md) zu ermöglichen.

## <a name="3-pass-information-between-pages"></a>3. Übergeben von Informationen zwischen Seiten

Unsere App navigiert zwischen zwei Seiten, sie bietet jedoch noch keine interessanten Funktionen. Bei vielen Apps mit mehreren Seiten müssen die Seiten Informationen freigeben. Übergeben wir also einige Informationen der ersten Seite an die zweite Seite.

Ersetze in „Page1.xaml“ das zuvor hinzugefügte **HyperlinkButton**-Element durch die folgende [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)-Klasse.

Hier fügen wir eine [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Bezeichnung und ein [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Element (`name`) zum Eingeben einer Textzeichenfolge hinzu.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Füge der `Navigate`-Methode in der CodeBehind-Datei „Page1.xaml“ im `HyperlinkButton_Click`-Ereignishandler einen Parameter hinzu, der auf die `Text`-Eigenschaft von `name` **TextBox** verweist.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>(), winrt::box_value(name().Text()));
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

Ersetze in „Page2.xaml“ das zuvor hinzugefügte **HyperlinkButton**-Element durch die folgende **StackPanel**-Klasse.

Hier fügen wir einen Textblock ([**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)) zum Anzeigen der von Seite 1 übergebenen Textzeichenfolge hinzu.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Füge der CodeBehind-Datei „Page2.xaml“ Folgendes hinzu, um die `OnNavigatedTo`-Methode zu überschreiben:

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string && !string.IsNullOrWhiteSpace((string)e.Parameter))
    {
        greeting.Text = $"Hi, {e.Parameter.ToString()}";
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

```cppwinrt
void Page2::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto propertyValue{ e.Parameter().as<Windows::Foundation::IPropertyValue>() };
    if (propertyValue.Type() == Windows::Foundation::PropertyType::String)
    {
        greeting().Text(L"Hi, " + winrt::unbox_value<winrt::hstring>(e.Parameter()));
    }
    else
    {
        greeting().Text(L"Hi!");
    }
    __super::OnNavigatedTo(e);
}
```

```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi, " + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

Führen Sie die App aus, geben Sie Ihren Namen in das Textfeld ein, und klicken Sie auf den Link **Click to go to page 2**. 

Wenn `this.Frame.Navigate(typeof(Page2), name.Text)` durch das **Click**-Ereignis des **HyperlinkButton**-Elements in `Page1` aufgerufen wird, wird die `name.Text`-Eigenschaft an `Page2` übergeben, und der Wert aus den Ereignisdaten wird für die auf der Seite angezeigte Nachricht verwendet.

## <a name="4-cache-a-page"></a>4. Zwischenspeichern einer Seite

Seiteninhalt und -zustand werden standardmäßig nicht zwischengespeichert. Wenn du also Informationen zwischenspeichern möchtest, musst du dies auf jeder Seite deiner App aktivieren.

In unserem einfachen Peer-zu-Peer-Beispiel gibt es keine Zurück-Schaltfläche. (Informationen zur Rückwärtsnavigation findest du [hier](navigation-history-and-backwards-navigation.md).) Wenn jedoch eine Zurück-Schaltfläche vorhanden wäre und du auf `Page2` darauf klicken würdest, hätte dies zur Folge, dass das **TextBox**-Element (und alle anderen Felder) auf `Page1` wieder auf den Standardzustand zurückgesetzt werden. Eine Möglichkeit zur Umgehung dieses Problems ist die Verwendung der [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode)-Eigenschaft, um anzugeben, dass eine Seite zum Seitencache des Frames hinzugefügt werden soll. 

Im Konstruktor von `Page1` kannst du **NavigationCacheMode** auf **Enabled** festlegen, um alle Inhalte und Zustandswerte für die Seite beizubehalten, bis der Seitencache für den Frame überschritten wird. Lege [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) auf [**Required**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation.NavigationCacheMode) fest, wenn du [**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize)-Limits ignorieren möchtest. (Diese Limits geben die Anzahl von Seiten an, die im Navigationsverlauf für den Frame zwischengespeichert werden können.) Die Beschränkung der Cachegröße kann allerdings je nach Arbeitsspeichergröße eines Geräts äußerst wichtig sein.

```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

```cppwinrt
Page1::Page1()
{
    InitializeComponent();
    NavigationCacheMode(Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled);
}
```

```cpp
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

## <a name="related-articles"></a>Verwandte Artikel
* [Grundlagen des Navigationsdesigns für Windows-Apps](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)
* [Pivot](../controls-and-patterns/pivot.md)
* [Navigationsansicht](../controls-and-patterns/navigationview.md)
