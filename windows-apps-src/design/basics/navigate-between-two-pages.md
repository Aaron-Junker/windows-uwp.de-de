---
Description: Erfahren Sie, wie Sie die Peer-zu-Peer-Navigation zwischen den zwei grundlegenden Seiten in einer app für die universelle Windows-Plattform (UWP) zu ermöglichen.
title: Peer-zu-Peer-Navigation zwischen zwei Seiten
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: e72dc726143d17f605283fa801f8e286c2c58878
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616205"
---
# <a name="implement-navigation-between-two-pages"></a>Implementieren der Navigation zwischen zwei Seiten

Hier erfahren Sie, wie Sie einen Rahmen und Seiten verwenden, um eine grundlegende Peer-to-Peer-Navigation in Ihrer App zu ermöglichen. 

> **Wichtige APIs:** [**Windows.UI.Xaml.Controls.Frame** ](https://msdn.microsoft.com/library/windows/apps/br242682) -Klasse, [ **Windows.UI.Xaml.Controls.Page** ](https://msdn.microsoft.com/library/windows/apps/br227503) -Klasse, [ **Windows.UI.Xaml.Navigation** ](https://msdn.microsoft.com/library/windows/apps/br243300) Namespace

![Peer-to-Peer-Navigation](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. Erstellen einer leeren app

1.  Klicken Sie im Microsoft Visual Studio-Menü auf **Datei** > **Neues Projekt**.
2.  Erweitern Sie im linken Bereich des Dialogfelds **Neues Projekt** den Knoten **Visual C#** > **Windows** > **Universell** oder **Visual C++** > **Windows** > **Universell**.
3.  Wählen Sie im mittleren Bereich die Option **Leere App** aus.
4.  Geben Sie in das Feld **Name** den Wert **NavApp1** ein, und klicken Sie anschließend auf **OK**.
    Die Projektmappe wird erstellt, und die Projektdateien werden im **Projektmappen-Explorer** angezeigt.
5.  Klicken Sie zum Ausführen des Programms im Menü auf **Debuggen** > **Debugging starten**, oder drücken Sie F5.
    Es wird eine leere Seite angezeigt.
6.  Um das Debuggen zu beenden und zu Visual Studio zurückzukehren, beenden Sie die App oder klicken Sie im Menü auf **Debuggen beenden**.

## <a name="2-add-basic-pages"></a>2. Hinzufügen von Standardseiten

Fügen Sie im nächsten Schritt zwei Seiten zum Projekt hinzu.

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten **BlankApp**, um das Kontextmenü zu öffnen.
2.  Wählen Sie im Kontextmenü **Hinzufügen** > **Neues Element** aus.
3.  Wählen Sie im Dialogfeld **Neues Element hinzufügen** im mittleren Bereich die Option **Leere Seite** aus.
4.  Geben Sie in das Feld **Name** den Wert **Page1** (oder **Page2**) ein, und wählen Sie anschließend **Hinzufügen**.
5. Wiederholen Sie die Schritte 1-4, um die zweite Seite hinzuzufügen.

Diese Dateien sollten nun als Teil des Projekts „NavApp1“ aufgeführt werden.

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

Fügen Sie in Page1.xaml den folgenden Inhalt hinzu:

-   Ein [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element mit der Bezeichnung `pageTitle` als untergeordnetes Element des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Stammelements. Ändern Sie die [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676)-Eigenschaft in `Page 1`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   Ein [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Element als untergeordnetes Element des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Stammelements und nach dem `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element.
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Fügen Sie in der Datei Page1.xaml Code-Behind den folgenden Code hinzu, um das `Click`-Ereignis des [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)s zu behandeln, den Sie hinzugefügt haben, um zu Page2.xaml zu navigieren.

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

Fügen Sie in Page2.xaml den folgenden Inhalt hinzu:

-   Ein [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element mit der Bezeichnung `pageTitle` als untergeordnetes Element des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Stammelements. Ändern Sie den Wert der [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676)-Eigenschaft in `Page 2`:
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   Ein [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)-Element als untergeordnetes Element des [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704)-Stammelements und nach dem `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)-Element.
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Fügen Sie in der Datei Page2.xaml Code-Behind den folgenden Code hinzu, um das `Click`-Ereignis des [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)s zu behandeln, den Sie hinzugefügt haben, um zu Page1.xaml zu navigieren.

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

Da wir die Seiten nun vorbereitet haben, müssen wir festlegen, dass „Page1.xaml“ beim Starten der App angezeigt wird.

Öffnen Sie die Code-Behind-Datei App.xaml, und ändern Sie den `OnLaunched`-Handler.

Hier geben wir `Page1` im Aufruf von [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) anstelle von `MainPage` an.

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
> Hier der Code verwendet den Rückgabewert der [ **Navigate** ](https://msdn.microsoft.com/library/windows/apps/br242694) eine app-Ausnahme ausgelöst, wenn die Navigation zum ersten Fensterrahmen der app ein Fehler auftritt. Wenn **Navigate** den Wert **true** zurückgibt, findet die Navigation statt.

Erstellen Sie nun die App, und führen Sie sie aus. Klicken Sie auf den Link „Click to go to page 2“. Die zweite Seite mit der Bezeichnung „Seite 2“ wird geladen und im Frame angezeigt.

### <a name="about-the-frame-and-page-classes"></a>Über die Rahmen- und Seitenklassen

Bevor wir der App weitere Funktionen hinzufügen, betrachten wir zunächst, inwiefern die hinzugefügten Seiten Navigationsunterstützung für die App bereitstellen.

Zuerst wird ein [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) (`rootFrame`) für die App in der `App.OnLaunched`-Methode der Code-Behind-Datei „App.xaml“ erstellt. Die **Frame**-Klasse unterstützt verschiedene Navigationsmethoden, z. B. [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) oder [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693) und Eigenschaften wie [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) oder [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995).
 
Die [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)-Methode wird zum Anzeigen von Inhalt im **Frame** verwendet. Standardmäßig lädt diese Methode MainPage.xaml. In unserem Beispiel wird `Page1` an die Methode **Navigate** übergeben, so dass die Methode `Page1` in den **Frame** geladen wird. 

`Page1` ist eine Unterklasse der [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)-Klasse. Die **Page**-Klasse hat eine schreibgeschützte **Frame**-Eigenschaft, die den **Frame** mit der **Page**-Klasse abruft. Wenn der **Click**-Ereignis-Handler des **HyperlinkButton**s in `Page1``this.Frame.Navigate(typeof(Page2))` aufruft, zeigt das **Frame** den Inhalt von Page2.xaml an.

Wenn eine Seite in das Frame geladen wird, wird diese Seite als [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) zum [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) oder [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) des [**Frames**](https://msdn.microsoft.com/library/windows/apps/br227504) hinzugefügt, was eine [Historie und Rückwärtsnavigation ermöglicht](navigation-history-and-backwards-navigation.md).

## <a name="3-pass-information-between-pages"></a>3. Übergeben von Informationen zwischen Seiten

Unsere App navigiert zwischen zwei Seiten, sie bietet jedoch noch keine interessanten Funktionen. Bei vielen Apps mit mehreren Seiten müssen die Seiten Informationen freigeben. Übergeben wir also einige Informationen der ersten Seite an die zweite Seite.

Ersetzen Sie in "Page1.xaml", die **HyperlinkButton** zuvor mit den folgenden hinzugefügten [ **StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635).

Hier fügen wir eine [ **TextBlock** ](https://msdn.microsoft.com/library/windows/apps/br209652) Bezeichnung und einen [ **Textfeld** ](https://msdn.microsoft.com/library/windows/apps/br209683) `name` für die Eingabe einer Textzeichenfolge.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Fügen Sie im `HyperlinkButton_Click`-Ereignis-Handler der Code-Behind-Datei „Page1.xaml“ einen Parameter hinzu, der die `Text`-Eigenschaft von `name` **TextBox** auf die `Navigate`-Methode verweist.

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

Ersetzen Sie in „Page2.xaml“ das zuvor hinzugefügte **HyperlinkButton**-Element mit der folgenden **StackPanel**-Klasse.

Hier fügen wir einen [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) für das Anzeigen einer von Seite 1 übergebenen Textzeichenfolge hinzu.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Fügen Sie im Code-Behind der Datei Page2.xaml folgendes hinzu, um die `OnNavigatedTo`-Methode zu überschreiben:

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

Wenn das **Click**-Ereignis des **HyperlinkButton**s in `Page1``this.Frame.Navigate(typeof(Page2), name.Text)` aufruft, wird die `name.Text`-Eigenschaft an `Page2` übergeben und der Wert aus den Ereignisdaten wird für die auf der Seite angezeigte Nachricht verwendet.

## <a name="4-cache-a-page"></a>4. Zwischenspeichern einer Seite

Seiteninhalt und -status werden standardmäßig nicht zwischengespeichert. Wenn Sie also Informationen zwischenspeichern möchten, müssen Sie diese in jeder Seite Ihrer App aktivieren.

In unserem einfachen Peer-zu-Peer-Beispiel gibt es keine Zurück-Schaltfläche (Infos zur Rückwärtsnavigation finden Sie unter [Rückwärtsnavigation](navigation-history-and-backwards-navigation.md)). Würden Sie aber auf `Page2` auf eine Zurück-Schaltfläche klicken, würden das **TextBox**-Element (und alle anderen Felder) auf `Page1` auf den Standardzustand zurückgesetzt werden. Eine Möglichkeit zur Umgehung dieses Problems ist die Verwendung der [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506)-Eigenschaft, um anzugeben, dass eine Seite zum Seitencache des Frames hinzugefügt werden soll. 

Im Konstruktor von `Page1` können Sie **NavigationCacheMode** auf **Enabled** setzen, um alle Inhalte und Statuswerte für die Seite beizubehalten, bis der Seiten-Cache für den Frame überschritten wird. Setzen Sie [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) auf [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284), wenn Sie [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683)-Limits ignorieren wollen (gibt die Anzahl der Seiten in der Navigationshistorie an, die für den Frame zwischengespeichert werden können). Einschränkungen der Cachegröße können je nach der Arbeitsspeichergrenze eines Geräts jedoch äußerst wichtig sein.

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
* [Grundlagen des Berichtsentwurfs Navigation für UWP-apps](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [Richtlinien für die Registerkarten und pivots nutzen](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [Richtlinien zum Navigationsbereich](https://msdn.microsoft.com/library/windows/apps/dn997766)
