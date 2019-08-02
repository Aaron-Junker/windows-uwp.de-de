---
Description: Verwenden Sie die appwindow-Klasse, um verschiedene Teile der app in separaten Fenstern anzuzeigen.
title: Verwenden der appwindow-Klasse zum Anzeigen sekundärer Fenster für eine APP
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f9b247a4a8eb69fa390a9e250e0f88deff9311bf
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68730518"
---
# <a name="show-multiple-views-with-appwindow"></a>Mehrere Ansichten mit appwindow anzeigen

[Appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) und zugehörige APIs vereinfachen die Erstellung von apps mit mehreren Fenstern, indem Sie Ihre APP-Inhalte in sekundären Fenstern anzeigen lassen, während Sie in jedem Fenster immer noch an demselben UI-Thread arbeiten.

> [!NOTE]
> Appwindow befindet sich derzeit in der Vorschau Phase. Dies bedeutet, dass Sie apps, die appwindow verwenden, an den Store übermitteln können, einige Plattform-und frameworkkomponenten jedoch bekanntermaßen nicht mit appwindow arbeiten (siehe [Einschränkungen](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).

Hier werden einige Szenarios für mehrere Fenster mit einer Beispiel-App mit `HelloAppWindow`dem Namen angezeigt. Die Beispiel-App veranschaulicht die folgenden Funktionen:

- Das Andocken eines Steuer Elements auf der Hauptseite und Öffnen dieses Steuer Elements in einem neuen Fenster.
- Öffnen Sie neue Instanzen einer Seite in neuen Fenstern.
- Programm gesteuertes anpassen und positionieren neuer Fenster in der app.
- Ordnen Sie dem entsprechenden Fenster in der APP einen contentdialog zu.

![Beispiel-App mit einem einzelnen Fenster](images/hello-app-window-single.png)
  
> _Beispiel-App mit einem einzelnen Fenster_

![Beispiel-App mit nicht angedockten Farbauswahl und sekundärem Fenster](images/hello-app-window-multi.png)

> _Beispiel-App mit nicht angedockten Farbauswahl und sekundärem Fenster_

> **Wichtige APIs:** [Windows. UI. windowmanagement-Namespace](/uwp/api/windows.ui.windowmanagement), [appwindow-Klasse](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>API-Übersicht

Die [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) -Klasse und andere APIs im [windowmanagement](/uwp/api/windows.ui.windowmanagement) -Namespace sind ab Windows 10, Version 1903 (SDK 18362) verfügbar. Wenn Ihre APP auf frühere Versionen von Windows 10 ausgerichtet ist, müssen Sie [die applicationview zum Erstellen sekundärer Fenster verwenden](application-view.md). Windowmanagement-APIs befinden sich noch in der Entwicklung und weisen [Einschränkungen](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) auf, wie in der API-Referenz Dokumentation beschrieben.

Hier finden Sie einige wichtige APIs, mit denen Sie Inhalt in einem appwindow anzeigen können.

### <a name="appwindow"></a>AppWindow

Die [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) -Klasse kann verwendet werden, um einen Teil einer Windows-Runtime-app in einem sekundären Fenster anzuzeigen. Dies ähnelt dem Konzept einer [applicationview](/uwp/api/windows.ui.viewmanagement.applicationview), aber nicht dem Verhalten und der Lebensdauer. Eine Hauptfunktion von appwindow besteht darin, dass jede Instanz denselben UI-Verarbeitungs Thread nutzt (einschließlich des Ereignis Verteilers), von dem Sie erstellt wurden, wodurch mehr Fenster Anwendungen vereinfacht werden.

Sie können nur XAML-Inhalte mit Ihrem appwindow verbinden, es gibt keine Unterstützung für nativen DirectX-oder Holographic-Inhalt. Sie können jedoch ein XAML- [Tausch Panel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) anzeigen, das DirectX-Inhalt hostet.

### <a name="windowingenvironment"></a>WindowingEnvironment

Mit der [windowingenvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) -API können Sie sich über die Umgebung informieren, in der Ihre APP angezeigt wird, sodass Sie Ihre APP nach Bedarf anpassen können. Beschreibt die Art des Fensters, das von der Umgebung unterstützt wird. beispielsweise, `Overlapped` wenn die APP auf einem PC ausgeführt wird oder `Tiled` wenn die APP auf einer Xbox ausgeführt wird. Außerdem wird ein Satz von displayregion-Objekten bereitgestellt, die die Bereiche beschreiben, in denen eine APP auf einer logischen Anzeige angezeigt werden kann.

### <a name="displayregion"></a>DisplayRegion

Die [displayregion](/uwp/api/windows.ui.windowmanagement.displayregion) -API beschreibt die Region, in der einem Benutzer eine Ansicht auf einer logischen Anzeige angezeigt werden kann. Beispielsweise ist auf einem Desktop-PC dies die vollständige Anzeige abzüglich des Bereichs der Taskleiste. Dabei handelt es sich nicht unbedingt um eine 1:1-Zuordnung mit dem physischen Anzeigebereich des Sicherungs Monitors. Es können mehrere Anzeige Bereiche innerhalb desselben Monitors vorhanden sein, oder eine Display Region kann so konfiguriert werden, dass Sie sich über mehrere Monitore erstreckt, wenn diese Monitore in allen Aspekten einheitlich sind.

### <a name="appwindowpresenter"></a>AppWindowPresenter

Mithilfe der [appwindowpresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) -API können Sie Windows problemlos in vordefinierte Konfigurationen wie `FullScreen` oder `CompactOverlay`wechseln. Diese Konfigurationen ermöglichen dem Benutzer ein konsistentes Verhalten auf allen Geräten, die die Konfiguration unterstützen.

### <a name="uicontext"></a>UIContext

[UIContext](/uwp/api/windows.ui.uicontext) ist ein eindeutiger Bezeichner für ein App-Fenster oder eine Ansicht. Sie wird automatisch erstellt, und Sie können die [UIElement. UIContext](/uwp/api/windows.ui.xaml.uielement.uicontext) -Eigenschaft verwenden, um den UIContext abzurufen. Jedes UIElement in der XAML-Struktur hat denselben UIContext.

 UIContext ist wichtig, da APIs wie [Window. Current](/uwp/api/Windows.UI.Xaml.Window.Current) und `GetForCurrentView` das-Muster darauf basieren, dass ein einzelnes applicationview/corewindow mit einer einzelnen XAML-Struktur pro Thread verwendet wird, um mit zu arbeiten. Dies ist nicht der Fall, wenn Sie ein appwindow verwenden, sodass Sie stattdessen UIContext verwenden, um ein bestimmtes Fenster zu identifizieren.

### <a name="xamlroot"></a>XamlRoot

Die [xamlroot](/uwp/api/windows.ui.xaml.xamlroot) -Klasse enthält eine XAML-Elementstruktur, verbindet Sie mit dem Fenster Host Objekt (z. b. [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) oder [applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)) und stellt Informationen wie Größe und Sichtbarkeit bereit. Sie erstellen ein xamlroot-Objekt nicht direkt. Stattdessen wird eine erstellt, wenn Sie ein XAML-Element an ein appwindow-Element anfügen. Sie können dann die [UIElement. xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot) -Eigenschaft verwenden, um das xamlroot-Element abzurufen.

Weitere Informationen zu UIContext und xamlroot finden Sie unter [Portier bares codieren über windowinghosts](show-multiple-views.md#make-code-portable-across-windowing-hosts).

## <a name="show-a-new-window"></a>Neues Fenster anzeigen

Sehen wir uns nun die Schritte zum Anzeigen von Inhalten in einem neuen Anwendungsfenster an.

**So zeigen Sie ein neues Fenster an**

1. Rufen Sie die statische [appwindow. trykreateasync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync) -Methode auf, um ein neues [appwindow-Fenster](/uwp/api/windows.ui.windowmanagement.appwindow)zu erstellen.

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. Erstellen Sie den Fensterinhalt.

    In der Regel erstellen Sie einen XAML- [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame)und navigieren dann in den Frame zu einer XAML- [Seite](/uwp/api/Windows.UI.Xaml.Controls.Page) , auf der Sie den App-Inhalt definiert haben. Weitere Informationen zu Frames und Seiten finden Sie unter [Peer-to-Peer-Navigation zwischen zwei Seiten](../basics/navigate-between-two-pages.md).

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    Allerdings können Sie XAML-Inhalte im appwindow anzeigen, nicht nur als Frame und Seite. Sie können z. b. nur ein einzelnes Steuerelement anzeigen, z. b. [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker), oder Sie können ein [Austausch](/uwp/api/windows.ui.xaml.controls.swapchainpanel) Element anzeigen, das DirectX-Inhalte hostet.

1. Rufen Sie die Methode [elementcompositionpreview. ltappwindowcontent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent) auf, um den XAML-Inhalt an appwindow anzufügen.

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    Der-Aufrufe dieser Methode erstellt ein [xamlroot](/uwp/api/windows.ui.xaml.xamlroot) -Objekt und legt dieses als [xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot) -Eigenschaft für das angegebene UIElement fest.

    Diese Methode kann nur einmal pro appwindow-Instanz aufgerufen werden. Nachdem der Inhalt festgelegt wurde, schlagen weitere Aufrufe von setappwindowcontent für diese appwindow-Instanz fehl. Wenn Sie versuchen, den appwindow-Inhalt zu trennen, indem Sie ein UIElement-NULL-Objekt übergeben, schlägt der-Vorgang fehl.

1. Rufen Sie die [appwindow. tryshowasync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) -Methode auf, um das neue Fenster anzuzeigen.

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>Freigeben von Ressourcen, wenn ein Fenster geschlossen wird

Das Ereignis [appwindow. Closed](/uwp/api/windows.ui.windowmanagement.appwindow.closed) sollte immer behandelt werden, um XAML-Ressourcen (den appwindow-Inhalt) und Verweise auf das appwindow-Ereignis freizugeben.

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>Nachverfolgen von Instanzen von appwindow

Je nachdem, wie Sie mehrere Fenster in der App verwenden, müssen Sie möglicherweise die von Ihnen erstellten Instanzen von appwindow nachverfolgen. Das `HelloAppWindow` Beispiel zeigt verschiedene Möglichkeiten, wie Sie in der Regel ein [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)verwenden können. Hier erfahren Sie, warum diese Fenster nachverfolgt werden sollten und wie Sie dies tun.

### <a name="simple-tracking"></a>Einfache Nachverfolgung

Das Fenster Farbauswahl hostet ein einzelnes XAML-Steuerelement, und der Code für die Interaktion mit der Farbauswahl `MainPage.xaml.cs` befindet sich in der Datei. Das Fenster für die Farbauswahl lässt nur eine einzelne Instanz zu und ist im `MainWindow`Grunde eine Erweiterung von. Um sicherzustellen, dass nur eine Instanz erstellt wird, wird das Farbauswahl Fenster mit einer Variablen auf Seitenebene nachverfolgt. Bevor Sie ein neues Farbauswahl Fenster erstellen, überprüfen Sie, ob eine Instanz vorhanden ist. wenn dies der Fall ist, überspringen Sie die Schritte zum Erstellen eines neuen Fensters, und rufen Sie einfach [tryshowasync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) im vorhandenen Fenster auf.

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

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>Nachverfolgen einer appwindow-Instanz im gehosteten Inhalt

Das `AppWindowPage` Fenster hostet eine vollständige XAML-Seite, und der Code für die Interaktion mit `AppWindowPage.xaml.cs`der Seite befindet sich in. Sie ermöglicht mehrere-Instanzen, von denen jede unabhängig funktioniert.

Mithilfe der Funktionalität der Seite können Sie das Fenster ändern, es auf `FullScreen` oder `CompactOverlay`festlegen und auf [appwindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) -Ereignisse lauschen, um Informationen über das Fenster anzuzeigen. Um diese APIs aufzurufen, `AppWindowPage` benötigt einen Verweis auf die appwindow-Instanz, in der Sie gehostet wird.

Wenn dies alles ist, was Sie benötigen, können Sie eine Eigenschaft in `AppWindowPage` erstellen und Sie der [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) -Instanz zuweisen, wenn Sie Sie erstellen.

**AppWindowPage.xaml.cs**

Erstellen `AppWindowPage`Sie in eine Eigenschaft, die den appwindow-Verweis enthalten soll.

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

Rufen `MainPage`Sie in einen Verweis auf die Seiten Instanz ab, und weisen Sie der-Eigenschaft in `AppWindowPage`das neu erstellte appwindow zu.

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

### <a name="tracking-app-windows-using-uicontext"></a>Überwachen von App-Fenstern mit UIContext

Möglicherweise möchten Sie auch über andere Teile Ihrer APP Zugriff auf die [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) -Instanzen haben. Beispielsweise `MainPage` könnte die Schaltfläche "alle schließen" vorhanden sein, die alle überwachten Instanzen von appwindow schließt.

In diesem Fall sollten Sie den eindeutigen [UIContext](/uwp/api/windows.ui.uicontext) -Bezeichner verwenden, um die Fenster Instanzen in einem [Wörterbuch](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0)zu verfolgen.

**MainPage.xaml.cs**

Erstellen `MainPage`Sie in das Wörterbuch als statische Eigenschaft. Fügen Sie dann die Seite dem Wörterbuch hinzu, wenn Sie es erstellen, und entfernen Sie es, wenn die Seite geschlossen wird. Sie können den UIContext aus dem Inhalts [Rahmen](/uwp/api/Windows.UI.Xaml.Controls.Frame) (`appWindowContentFrame.UIContext`) abrufen, nachdem Sie [elementcompositionpreview. abtappwindowcontent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)aufgerufen haben.

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

Um die [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) -Instanz in Ihrem `AppWindowPage` Code zu verwenden, verwenden Sie den [UIContext](/uwp/api/windows.ui.uicontext) der Seite, um Sie aus dem `MainPage`statischen Wörterbuch in abzurufen. Sie sollten dies im [geladenen](/uwp/api/windows.ui.xaml.frameworkelement.loaded) Ereignishandler der Seite statt im Konstruktor tun, damit UIContext nicht NULL ist. Sie können den UIContext von der folgenden Seite erhalten `this.UIContext`:.

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
> Das `HelloAppWindow` Beispiel zeigt beide Möglichkeiten, das Fenster in `AppWindowPage`zu verfolgen, aber Sie verwenden normalerweise eine oder die andere, nicht beides.

## <a name="request-window-size-and-placement"></a>Größe und Platzierung des Anforderungs Fensters

Die [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) -Klasse verfügt über mehrere Methoden, mit denen Sie die Größe und die Platzierung des Fensters steuern können. Entsprechend den Methodennamen kann das System die angeforderten Änderungen abhängig von den Umgebungsfaktoren oder nicht beachten.

" [Requestsize](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize) " aufrufen, um eine gewünschte Fenstergröße wie diese anzugeben.

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

Die Methoden zum Verwalten der Fensterplatzierung heißen _requestmove *_ : [Requestapvedetailenttocurrentview](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview), [requestapvedetailenttowindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow), [requestmuverelativetodisplayregion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion), [requestvtodisplayregion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion).

In diesem Beispiel verschiebt dieser Code das Fenster in eine neben der Hauptansicht, aus der das Fenster erzeugt wird.

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

Um Informationen zur aktuellen Größe und Platzierung des Fensters abzurufen, nennen Sie [getPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement). Dadurch wird ein [appwindowplacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement) -Objekt zurückgegeben, das den aktuellen [Display Region](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion), den [Offset](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)und die [Größe](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size) des Fensters bereitstellt.

Beispielsweise können Sie diesen Code aufrufen, um das Fenster in die rechte obere Ecke der Anzeige zu verschieben. Dieser Code muss aufgerufen werden, nachdem das Fenster angezeigt wurde. Andernfalls ist die vom getPlacement-Rückruf zurückgegebene Fenstergröße 0, 0, und der Offset ist falsch.

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>Präsentations Konfiguration anfordern

Mithilfe der [appwindowpresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) -Klasse können Sie ein [appwindow-Fenster](/uwp/api/windows.ui.windowmanagement.appwindow) mit einer vordefinierten Konfiguration anzeigen, die für das Gerät geeignet ist, auf dem es angezeigt wird. Sie können einen [appwindowpresentationconfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration) -Wert verwenden, um das Fenster `FullScreen` in `CompactOverlay` den-oder-Modus zu versetzen.

Dieses Beispiel zeigt, wie Sie die folgenden Schritte ausführen:

- Verwenden Sie das Ereignis [appwindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) , um benachrichtigt zu werden, wenn sich die verfügbaren Fenster Präsentationen ändern.
- Verwenden Sie die [appwindow. Presenter](/uwp/api/windows.ui.windowmanagement.appwindow.presenter) -Eigenschaft, um die aktuelle [appwindowpresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)-Datei zu erhalten.
- Rufen Sie [iationsentationsupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported) auf, um festzustellen, ob eine bestimmte [appwindowpresentationkind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) unterstützt wird.
- [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration) aufrufen, um zu überprüfen, welche Art von Konfiguration zurzeit verwendet wird.
- Aufrufen von [requestpresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation) , um die aktuelle Konfiguration zu ändern.

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

## <a name="reuse-xaml-elements"></a>Wieder verwenden von XAML-Elementen

Mit einem [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) können Sie mehrere XAML-Strukturen mit demselben UI-Thread haben. Ein XAML-Element kann jedoch nur einmal zu einer XAML-Struktur hinzugefügt werden. Wenn Sie einen Teil der Benutzeroberfläche von einem Fenster in ein anderes verschieben möchten, müssen Sie die Platzierung in der XAML-Struktur verwalten.

Dieses Beispiel zeigt, wie ein [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) -Steuerelement beim Verschieben zwischen dem Hauptfenster und einem sekundären Fenster wieder verwendet werden kann.

Die Farbauswahl wird in der XAML für `MainPage`deklariert, wodurch Sie in der `MainPage` XAML-Struktur platziert wird.

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

Wenn die Farbauswahl getrennt wird, damit Sie in ein neues appwindow eingefügt werden kann, müssen Sie Sie zunächst aus `MainPage` der XAML-Struktur entfernen, indem Sie Sie aus dem übergeordneten Container entfernen. Obwohl dies nicht erforderlich ist, wird in diesem Beispiel auch der übergeordnete Container ausgeblendet.

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

Anschließend können Sie es der neuen XAML-Struktur hinzufügen. Hier erstellen Sie zuerst ein [Raster](/uwp/api/windows.ui.xaml.controls.grid) , das als übergeordneter Container für ColorPicker verwendet wird, und fügen die ColorPicker als untergeordnetes Element des Rasters hinzu. (Auf diese Weise können Sie den ColorPicker leicht später aus dieser XAML-Struktur entfernen.) Anschließend legen Sie das Raster als Stamm der XAML-Struktur im neuen Fenster fest.

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

Wenn das [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow) geschlossen ist, wird der Prozess umgekehrt. Entfernen Sie zuerst den [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) aus dem [Raster](/uwp/api/windows.ui.xaml.controls.grid), und fügen Sie ihn dann als untergeordnetes Element des [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) `MainPage`-Elements hinzu.

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

## <a name="show-a-dialog-box"></a>Dialogfeld anzeigen

Inhaltsdialogfelder werden standardmäßig modal relativ zum Stamm [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) angezeigt. Wenn Sie in einem [appwindow](/uwp/api/windows.ui.windowmanagement.appwindow)-Element einen [contentdialog](/uwp/api/windows.ui.xaml.controls.contentdialog) verwenden, müssen Sie das xamlroot-Element im Dialogfeld manuell auf den Stamm des XAML-Hosts festlegen.

Legen Sie zu diesem Zweck die [xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot) -Eigenschaft von contentdialog auf denselben [xamlroot](/uwp/api/windows.ui.xaml.xamlroot) -Wert fest, der bereits im appwindow vorhanden ist. Hier befindet sich dieser Code im Click-Ereignishandler einer Schaltfläche, sodass Sie den _Absender_ (die Schaltfläche mit dem [Klick](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ) verwenden können, um das xamlroot-Element zu erhalten.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

Wenn Sie zusätzlich zum Hauptfenster (applicationview) ein oder mehrere appwindows geöffnet haben, kann jedes Fenster versuchen, ein Dialogfeld zu öffnen, da das modale Dialogfeld nur das Fenster sperrt, in dem es sich befindet. Es kann jedoch nur jeweils ein [contentdialog-Dialog](/uwp/api/windows.ui.xaml.controls.contentdialog) Feld pro Thread geöffnet werden. Ein Versuch, zwei „ContentDialogs“ zu öffnen, löst eine Ausnahme selbst dann aus, wenn das Öffnen in getrennten „AppWindows“ geschehen soll.

Um dies zu verwalten, sollten Sie das Dialogfeld in einem `try/catch` -Block zumindest öffnen, um die Ausnahme abzufangen, falls ein anderes Dialogfeld bereits geöffnet ist.

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

Eine weitere Möglichkeit zum Verwalten von Dialogfeldern besteht darin, das momentan geöffnete Dialogfeld zu verfolgen und zu schließen, bevor Sie versuchen, ein neues Dialogfeld zu öffnen. Hier erstellen Sie eine statische Eigenschaft in `MainPage` , die für diesen Zweck aufgerufen `CurrentDialog` wird.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

Dann überprüfen Sie, ob ein aktuell geöffnetes Dialogfeld vorhanden ist. wenn dies der Fall ist, können Sie es mit der [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide) -Methode schließen. Weisen Sie schließlich das neue Dialogfeld `CurrentDialog`zu, und versuchen Sie, es anzuzeigen.

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

Wenn es nicht wünschenswert ist, dass ein Dialogfeld Programm gesteuert geschlossen wird, weisen Sie `CurrentDialog`es nicht als zu. Hier wird `MainPage` ein wichtiges Dialogfeld angezeigt, das nur verworfen werden sollte, wenn `Ok`mit der Maus geklickt wird. Da Sie nicht als `CurrentDialog`zugewiesen ist, wird nicht versucht, Sie Programm gesteuert zu schließen.

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

### <a name="appwindowpagexaml"></a>Appwindowpage. XAML

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

## <a name="related-topics"></a>Verwandte Themen

- [Mehrere Ansichten anzeigen](show-multiple-views.md)
- [Anzeigen mehrerer Ansichten mit applicationview](application-view.md)
