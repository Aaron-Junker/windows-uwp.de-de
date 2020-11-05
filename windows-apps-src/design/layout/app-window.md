---
description: Verwende die AppWindow-Klasse, um verschiedene Teile der App in einzelnen Fenstern anzuzeigen.
title: Verwenden der AppWindow-Klasse zum Anzeigen sekundärer Fenster für eine App
ms.date: 07/19/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a3f8644612954c4693ad28d3c1b41870855b37ca
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034883"
---
# <a name="show-multiple-views-with-appwindow"></a>Anzeigen mehrerer Ansichten mit AppWindow

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) und die zugehörigen APIs vereinfachen die Erstellung von Apps mit mehreren Fenstern, indem du deinen App-Inhalt in sekundären Fenstern anzeigen lassen kannst, während in jedem Fenster immer noch am selben Benutzeroberflächenthread gearbeitet wird.

> [!NOTE]
> AppWindow ist zurzeit als Vorschau verfügbar. Du kannst daher Apps, die AppWindow verwenden, an den Store übermitteln. Es ist jedoch von einigen Plattform- und Frameworkkomponenten bekannt, dass sie nicht mit AppWindow funktionieren (siehe [Einschränkungen](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).

Hier werden einige Szenarien für mehrere Fenster mit einer Beispiel-App namens `HelloAppWindow` gezeigt. Die Beispiel-App veranschaulicht die folgenden Funktionen:

- Abdocken eines Steuerelements auf der Hauptseite und Öffnen dieses Steuerelements in einem neuen Fenster
- Öffnen von neuen Instanzen einer Seite in neuen Fenstern
- Programmgesteuertes Anpassen und Positionieren neuer Fenster in der App
- Zuordnen eines ContentDialog zum entsprechenden Fenster in der App

![Beispiel-App mit einem einzelnen Fenster](images/hello-app-window-single.png)
  
> _Beispiel-App mit einem einzelnen Fenster_

![Beispiel-App mit abgedockter Farbauswahl und sekundärem Fenster](images/hello-app-window-multi.png)

> _Beispiel-App mit abgedockter Farbauswahl und sekundärem Fenster_

> **Wichtige APIs:** [Namespace Windows.UI.WindowManagement](/uwp/api/windows.ui.windowmanagement), [AppWindow-Klasse](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>API-Übersicht

Die [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)-Klasse und andere APIs im Namespace [WindowManagement](/uwp/api/windows.ui.windowmanagement) sind ab Windows 10 Version 1903 (SDK 18362) verfügbar. Wenn deine App auf frühere Versionen von Windows 10 ausgerichtet ist, musst du [sekundäre Fenster mithilfe von ApplicationView erstellen](application-view.md). WindowManagement-APIs befinden sich noch in der Entwicklung und weisen [Einschränkungen](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) auf, wie in der API-Referenzdokumentation beschrieben.

Hier findest du einige wichtige APIs, mit denen du Inhalte in einem AppWindow anzeigen kannst.

### <a name="appwindow"></a>AppWindow

Die [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)-Klasse kann verwendet werden, um einen Teil einer Windows-Runtime-App in einem sekundären Fenster anzuzeigen. Dies ähnelt dem Konzept einer [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview), unterscheidet sich jedoch im Verhalten und der Lebensdauer. Eines der Hauptfeatures von AppWindow besteht darin, dass jede Instanz denselben Benutzeroberflächen-Verarbeitungsthread nutzt (einschließlich des Ereignisverteilers), in dem sie erstellt wurde. Dadurch werden Anwendungen mit mehreren Fenstern vereinfacht.

Du kannst mit deinem AppWindow nur XAML-Inhalte verbinden, native DirectX- oder Holographic-Inhalte werden nicht unterstützt. Du kannst jedoch ein XAML-[SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) anzeigen, in dem DirectX-Inhalte gehostet werden.

### <a name="windowingenvironment"></a>WindowingEnvironment

Mit der [WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment)-API kannst du über die Umgebung informiert werden, in der deine App dargestellt wird, damit du die App nach Bedarf anpassen kannst. Sie beschreibt den Fenstertyp, der in der Umgebung unterstützt wird, etwa `Overlapped`, wenn die App auf einem PC ausgeführt wird, oder `Tiled`, wenn sie auf einer Xbox ausgeführt wird. Außerdem stellt sie eine Gruppe von DisplayRegion-Objekten bereit, die die Bereiche beschreiben, in denen eine App auf einer logischen Anzeige dargestellt werden kann.

### <a name="displayregion"></a>DisplayRegion

Die [DisplayRegion](/uwp/api/windows.ui.windowmanagement.displayregion)-API beschreibt die Region, in der eine Ansicht für einen Benutzer auf einer logischen Anzeige dargestellt werden kann. Beispielsweise ist dies auf einem Desktop-PC der vollständige Anzeigebereich abzüglich der Taskleiste. Dabei handelt es sich nicht unbedingt um eine genaue Zuordnung des physischen Anzeigebereichs des zugrunde liegenden Monitors. Es können mehrere Anzeigebereiche auf demselben Monitor vorhanden sein, oder eine DisplayRegion kann so konfiguriert werden, dass sie sich über mehrere Monitore erstreckt, wenn diese Monitore in allen Aspekten einheitlich sind.

### <a name="appwindowpresenter"></a>AppWindowPresenter

Mit der [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)-API kannst du Fenster problemlos in vordefinierte Konfigurationen wie `FullScreen` oder `CompactOverlay` überführen. Diese Konfigurationen stellen dem Benutzer ein konsistentes Verhalten auf allen Geräten bereit, die die jeweilige Konfiguration unterstützen.

### <a name="uicontext"></a>UIContext

[UIContext](/uwp/api/windows.ui.uicontext) ist ein eindeutiger Bezeichner für ein App-Fenster oder eine App-Ansicht. Der UIContext wird automatisch erstellt, und du kannst ihn mithilfe der [UIElement.UIContext](/uwp/api/windows.ui.xaml.uielement.uicontext)-Eigenschaft abrufen. Jedes UIElement in der XAML-Struktur weist denselben UIContext auf.

 UIContext ist wichtig, da APIs wie [Window.Current](/uwp/api/Windows.UI.Xaml.Window.Current) und das Muster `GetForCurrentView` darauf aufbauen, dass ein einzelnes ApplicationView-/CoreWindow-Element mit einer einzelnen XAML-Struktur pro Thread vorhanden ist, mit der gearbeitet wird. Dies ist nicht der Fall, wenn du ein AppWindow verwendest, sodass du stattdessen mithilfe von UIContext ein bestimmtes Fenster identifizierst.

### <a name="xamlroot"></a>XamlRoot

Die [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)-Klasse enthält eine XAML-Elementstruktur, verbindet diese mit dem Fensterhostobjekt (z. B. [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) oder [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)) und stellt Informationen wie Größe und Sichtbarkeit bereit. XamlRoot-Objekte werden nicht direkt erstellt. Stattdessen wird eines erstellt, wenn du ein XAML-Element an ein AppWindow-Element anfügst. Du kannst dann mithilfe der [UIElement.XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)-Eigenschaft das XamlRoot-Element abrufen.

Weitere Informationen zu UIContext und XamlRoot findest du unter [Gestalten von portablem Code für alle Fensterhosts](show-multiple-views.md#make-code-portable-across-windowing-hosts).

## <a name="show-a-new-window"></a>Anzeigen eines neuen Fensters

Sieh dir nun die Schritte zum Anzeigen von Inhalten in einem neuen AppWindow an.

**So zeigst du ein neues Fenster an**

1. Rufe die statische [AppWindow.TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync)-Methode auf, um ein neues [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) zu erstellen.

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. Erstelle den Fensterinhalt.

    In der Regel erstellt du einen XAML-[Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) und navigierst dann den Frame zu einer XAML-[Seite](/uwp/api/Windows.UI.Xaml.Controls.Page), auf der du den App-Inhalt definiert hast. Weitere Informationen zu Frames und Seiten findest du unter [Peer-zu-Peer-Navigation zwischen zwei Seiten](../basics/navigate-between-two-pages.md).

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    Du kannst jedoch beliebige XAML-Inhalte im AppWindow anzeigen, nicht nur Frames und Seiten. Du kannst z. B. nur ein einzelnes Steuerelement wie [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) anzeigen, oder du kannst ein [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) anzeigen, in dem DirectX-Inhalte gehostet werden.

1. Rufe die [ElementCompositionPreview.SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)-Methode auf, um den XAML-Inhalt an das AppWindow anzufügen.

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    Beim Aufruf dieser Methode wird ein [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)-Objekt erstellt und als [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)-Eigenschaft für das angegebene UIElement festgelegt.

    Diese Methode kann nur einmal pro AppWindow-Instanz aufgerufen werden. Nachdem der Inhalt festgelegt wurde, treten bei weiteren Aufrufen von SetAppWindowContent für diese AppWindow-Instanz Fehler auf. Wenn du versuchst, den AppWindow-Inhalt durch Übergeben eines UIElement-NULL-Objekts zu trennen, tritt ebenfalls ein Fehler auf.

1. Rufe die [AppWindow.TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync)-Methode auf, um das neue Fenster anzuzeigen.

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>Freigeben von Ressourcen beim Schließen eines Fensters

Du solltest immer das [AppWindow.Closed](/uwp/api/windows.ui.windowmanagement.appwindow.closed)-Ereignis behandeln, um XAML-Ressourcen (den AppWindow-Inhalt) und Verweise auf das AppWindow freizugeben.

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>Nachverfolgen von Instanzen von AppWindow

Je nachdem, wie du mehrere Fenster in deiner App verwendest, musst du möglicherweise die erstellten Instanzen von AppWindow nachverfolgen. Das Beispiel `HelloAppWindow` zeigt verschiedene Möglichkeiten, wie du ein [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) normalerweise verwenden kannst. Im Folgenden erfährst du, warum diese Fenster nachverfolgt werden sollten und wie dies möglich ist.

### <a name="simple-tracking"></a>Einfaches Nachverfolgen

Im Farbauswahlfenster wird ein einzelnes XAML-Steuerelement gehostet, und der Code für die Interaktion mit der Farbauswahl befindet sich in der Datei `MainPage.xaml.cs`. Das Farbauswahlfenster lässt nur eine einzelne Instanz zu und ist grundsätzlich eine Erweiterung von `MainWindow`. Um sicherzustellen, dass nur eine Instanz erstellt werden kann, wird das Farbauswahlfenster mit einer Variable auf Seitenebene nachverfolgt. Bevor du ein neues Farbauswahlfenster erstellst, überprüfe, ob bereits eine Instanz vorhanden ist. Wenn dies der Fall ist, überspringe die Schritte zum Erstellen eines neuen Fensters, und rufe einfach [TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) im vorhandenen Fenster auf.

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>Nachverfolgen einer AppWindow-Instanz im gehosteten Inhalt

Im Fenster `AppWindowPage` wird eine vollständige XAML-Seite gehostet, und der Code für die Interaktion mit der Seite befindet sich in `AppWindowPage.xaml.cs`. Dabei sind mehrere Instanzen möglich, die jeweils unabhängig funktionieren.

Mithilfe der Funktionalität der Seite kannst du das Fenster ändern, indem du es auf `FullScreen` oder `CompactOverlay` festlegst, und du kannst auf [AppWindow.Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed)-Ereignisse lauschen, um Informationen zum Fenster anzuzeigen. Um diese APIs aufzurufen, benötigt `AppWindowPage` einen Verweis auf die AppWindow-Instanz, in der das Element gehostet wird.

Wenn du nicht mehr als das benötigst, kannst du in `AppWindowPage` eine Eigenschaft erstellen und dieser beim Erstellen die [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)-Instanz zuweisen.

**AppWindowPage.xaml.cs**

Erstelle in `AppWindowPage` eine Eigenschaft, die den AppWindow-Verweis enthalten soll.

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

Rufe in `MainPage` einen Verweis auf die Seiteninstanz ab, und weise das neu erstellte AppWindow der Eigenschaft in `AppWindowPage` zu.

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>Nachverfolgen von App-Fenstern mit UIContext

Möglicherweise möchtest du auch in anderen Teilen deiner App Zugriff auf die [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)-Instanzen haben. Beispielsweise könnte `MainPage` eine Schaltfläche „Alle schließen“ aufweisen, die alle überwachten Instanzen von AppWindow schließt.

In diesem Fall solltest du den eindeutigen Bezeichner [UIContext](/uwp/api/windows.ui.uicontext) verwenden, um die Fensterinstanzen in einem [Wörterbuch](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0) nachzuverfolgen.

**MainPage.xaml.cs**

Erstelle in `MainPage` das Wörterbuch als statische Eigenschaft. Füge dann dem Wörterbuch die Seite beim Erstellen hinzu, und entferne sie beim Schließen der Seite. Du kannst den UIContext aus dem [Inhaltsframe](/uwp/api/Windows.UI.Xaml.Controls.Frame) (`appWindowContentFrame.UIContext`) abrufen, nachdem du [ElementCompositionPreview.SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent) aufgerufen hast.

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

Wenn du die [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)-Instanz in deinem Code für `AppWindowPage` verwenden möchtest, verwende den [UIContext](/uwp/api/windows.ui.uicontext) der Seite, um sie in `MainPage` aus dem statischen Wörterbuch abzurufen. Dies sollte im [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded)-Ereignishandler der Seite erfolgen, nicht im Konstruktor, damit UIContext nicht NULL ist. Du kannst den UIContext von der folgenden Seite erhalten: `this.UIContext`.

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> Das Beispiel `HelloAppWindow` zeigt beide Möglichkeiten, das Fenster in `AppWindowPage` nachzuverfolgen, aber normalerweise verwendest du nicht beide, sondern nur eine.

## <a name="request-window-size-and-placement"></a>Größe und Platzierung des Anforderungsfensters

Die [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)-Klasse verfügt über mehrere Methoden zum Steuern der Größe und der Platzierung des Fensters. Wie die Methodennamen bereits andeuten, kann das System die angeforderten Änderungen abhängig von den Umgebungsfaktoren beachten oder nicht beachten.

Rufe [RequestSize](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize) wie folgt auf, um die gewünschte Fenstergröße anzugeben.

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

Die Methoden zum Verwalten der Fensterplatzierung sind mit _RequestMove*_ benannt: [RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview), [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow), [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion), [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion).

In diesem Beispiel verschiebt dieser Code das Fenster neben die Hauptansicht, aus der das Fenster erzeugt wird.

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

Rufe zum Erhalten von Informationen zur aktuellen Größe und Platzierung des Fensters [GetPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement) auf. Diese Funktion gibt ein [AppWindowPlacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement)-Objekt zurück, das die aktuellen Werte für [DisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion), [Offset](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset) und [Size](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size) des Fensters bereitstellt.

Beispielsweise kannst du diesen Code aufrufen, um das Fenster in die rechte obere Ecke der Anzeige zu verschieben. Dieser Code muss aufgerufen werden, nachdem das Fenster angezeigt wurde. Andernfalls ist die vom GetPlacement-Rückruf zurückgegebene Fenstergröße „0,0“, und der Offset ist falsch.

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>Anfordern einer Darstellungskonfiguration

Mit der [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)-Klasse kannst du ein [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) mithilfe einer vordefinierten geeigneten Konfiguration für das entsprechende Gerät anzeigen. Du kannst den Wert [AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration) verwenden, um das Fenster in einem der Modi `FullScreen` oder `CompactOverlay` zu platzieren.

Dieses Beispiel zeigt die folgende Vorgehensweise:

- Verwenden des [AppWindow.Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed)-Ereignisses, um benachrichtigt zu werden, wenn sich die verfügbaren Fensterpräsentationen ändern
- Verwenden der [AppWindow.Presenter](/uwp/api/windows.ui.windowmanagement.appwindow.presenter)-Eigenschaft, um den aktuellen [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) abzurufen
- Aufrufen von [IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported), um festzustellen, ob ein bestimmter Wert für [AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) unterstützt wird
- Anwenden von [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration), um zu überprüfen, welche Art von Konfiguration derzeit verwendet wird
- Aufrufen von [RequestPresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation), um die aktuelle Konfiguration zu ändern

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>Wiederverwenden von XAML-Elementen

Bei einem [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) kannst du über mehrere XAML-Strukturen mit demselben UI-Thread verfügen. Ein XAML-Element kann jedoch nur einmal einer XAML-Struktur hinzugefügt werden. Wenn du einen Teil der Benutzeroberfläche von einem Fenster in ein anderes verschieben möchtest, musst du die Platzierung in der XAML-Struktur verwalten.

In diesem Beispiel wird gezeigt, wie ein [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker)-Steuerelement beim Verschieben zwischen dem Hauptfenster und einem sekundären Fenster wiederverwendet werden kann.

Die Farbauswahl wird in der XAML-Datei für `MainPage` deklariert und in der XAML-Struktur für `MainPage` platziert.

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

Wenn die Farbauswahl getrennt wird, damit sie in ein neues AppWindow eingefügt werden kann, musst du sie zunächst aus der XAML-Struktur von `MainPage` entfernen, indem du sie aus dem übergeordneten Container entfernst. Obwohl dies nicht erforderlich ist, wird in diesem Beispiel auch der übergeordnete Container ausgeblendet.

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

Anschließend kannst du sie der neuen XAML-Struktur hinzufügen. Hier erstellst du zunächst ein [Raster](/uwp/api/windows.ui.xaml.controls.grid), das als übergeordneter Container für den ColorPicker verwendet wird, und fügst den ColorPicker als untergeordnetes Element des Rasters hinzu. (Auf diese Weise kannst du den ColorPicker später einfach aus dieser XAML-Struktur entfernen.) Anschließend legst du das Raster als Stamm der XAML-Struktur im neuen Fenster fest.

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

Beim Schließen des [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) wird der Prozess umgekehrt. Entferne zunächst den [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) aus dem [Raster](/uwp/api/windows.ui.xaml.controls.grid), und füge ihn dann als untergeordnetes Element des [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) in `MainPage` hinzu.

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>Anzeigen eines Dialogfelds

Inhaltsdialogfelder werden standardmäßig modal relativ zum Stamm [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) angezeigt. Wenn du einen [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) in einem [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) verwendest, musste du den Wert für XamlRoot im Dialogfeld manuell auf den Stamm des XAML-Hosts festlegen.

Lege hierzu die [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)-Eigenschaft des ContentDialog auf denselben [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)-Wert wie bei einem im AppWindow bereits vorhandenen Element fest. Hier befindet sich dieser Code im [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignishandler einer Schaltfläche, sodass du das XamlRoot-Element über das _sendende Element_ (geklickte Schaltfläche) abrufen kannst.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

Wenn zusätzlich zum Hauptfenster (ApplicationView) andere AppWindows geöffnet sind, kann jedes Fenster versuchen, ein Dialogfeld zu öffnen, da das modale Dialogfeld nur das Fenster sperrt, dem es angehört. Es kann allerdings jeweils nur ein [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) pro Thread geöffnet sein. Ein Versuch, zwei „ContentDialogs“ zu öffnen, löst eine Ausnahme selbst dann aus, wenn das Öffnen in getrennten „AppWindows“ geschehen soll.

Um dies zu verwalten, solltest du das Dialogfeld zumindest in einem `try/catch`-Block öffnen, um die Ausnahme abzufangen, falls ein anderes Dialogfeld bereits geöffnet ist.

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

Eine weitere Möglichkeit zum Verwalten von Dialogfeldern besteht darin, das momentan geöffnete Dialogfeld zu verfolgen und es zu schließen, bevor ein neues Dialogfeld geöffnet wird. Hier erstellst du eine statische Eigenschaft in `MainPage`, die für diesen Zweck `CurrentDialog` genannt wird.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

Anschließend überprüfst du, ob derzeit ein Dialogfeld geöffnet ist. In diesem Fall rufst du die [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide)-Methode auf, um es zu schließen. Weise schließlich `CurrentDialog` das neue Dialogfeld zu, und versuche, es anzuzeigen.

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

Wenn das programmgesteuerte Schließen eines Dialogfelds nicht wünschenswert ist, weise es nicht als `CurrentDialog` zu. Hier zeigt `MainPage` ein wichtiges Dialogfeld an, das nur verworfen werden sollte, wenn der Benutzer auf `Ok` klickt. Da es nicht als `CurrentDialog` zugewiesen ist, wird nicht versucht, es programmgesteuert zu schließen.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>Vollständiger Code

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>Zugehörige Themen

- [Anzeigen mehrerer Ansichten](show-multiple-views.md)
- [Anzeigen mehrerer Ansichten mit ApplicationView](application-view.md)
