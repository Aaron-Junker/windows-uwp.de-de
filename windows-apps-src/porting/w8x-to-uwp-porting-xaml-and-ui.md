---
description: Die Vorgehensweise zum Definieren einer Benutzeroberfläche in Form von deklarativem XAML-Markup lässt sich sehr gut von universellen 8.1-Apps auf Apps für die universelle Windows-Plattform (UWP) übertragen.
title: Portieren von Windows-Runtime 8.x-XAML und -UI zu UWP
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 19e754fd6a52880c7bc636818acaeda815f9da16
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259104"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Portieren von Windows-Runtime 8.x-XAML und -UI zu UWP


Im vorherigen Thema haben wir uns mit der [Problembehandlung](w8x-to-uwp-troubleshooting.md) beschäftigt.

Die Vorgehensweise zum Definieren einer Benutzeroberfläche in Form von deklarativem XAML-Markup lässt sich sehr gut von universellen 8.1-Apps auf Apps für die universelle Windows-Plattform (UWP) übertragen. Sie werden feststellen, dass der Großteil des Markups kompatibel ist. Unter Umständen sind jedoch einige Anpassungen bei Systemressourcenschlüsseln oder benutzerdefinierten Vorlagen erforderlich. Beim imperativen Code in Ihren Ansichtsmodellen sind nur geringfügige oder gar keine Änderungen erforderlich. Und sogar ein Großteil des Codes in Ihrer Darstellungsschicht zur Manipulierung von UI-Elementen dürfte sich problemlos portieren lassen.

## <a name="imperative-code"></a>Imperativer Code

Falls Sie lediglich die Projekterstellungsphase erreichen möchten, können Sie sämtlichen nicht unbedingt erforderlichen Code auskommentieren. Gehen Sie anschließend nacheinander die einzelnen Probleme anhand der Informationen in den folgenden Themen dieses Abschnitts (einschließlich des vorherigen Themas [Problembehandlung](w8x-to-uwp-troubleshooting.md)) durch, bis alle Erstellungs- und Laufzeitprobleme behoben sind und die Portierung abgeschlossen ist.

## <a name="adaptiveresponsive-ui"></a>Adaptive/reaktionsfähige Benutzeroberfläche

Da Ihre App potenziell auf vielen verschiedenen Geräten ausgeführt werden kann – jeweils mit eigener Bildschirmgröße und -auflösung – ist es ratsam, nicht nur die mindestens erforderlichen Schritte zum Portieren Ihrer App auszuführen und die UI so zu gestalten, dass sie auf allen Geräten möglichst gut aussieht. Sie können das adaptive Visual State-Manager-Feature nutzen, um die Fenstergröße dynamisch zu ermitteln und als Reaktion darauf das Layout zu ändern. Ein Beispiel zur Vorgehensweise finden Sie im Abschnitt [Adaptive UI](w8x-to-uwp-case-study-bookstore2.md) im Thema mit der Bookstore2-Fallstudie.

## <a name="back-button-handling"></a>Behandeln der Schaltfläche „Zurück“

For Universal 8.1 apps, Windows Runtime 8.x apps and Windows Phone Store apps have different approaches to the UI you show and the events you handle for the back button. But, for Windows 10 apps, you can use a single approach in your app. Auf mobilen Geräten wird die Schaltfläche für Sie als kapazitive Schaltfläche auf dem Gerät oder als Schaltfläche in der Shell bereitgestellt. Auf einem Desktopgerät fügen Sie den Chromelementen der App eine Schaltfläche hinzu, wenn die Rückwärtsnavigation in der App möglich ist. Sie wird für Apps mit Fenstern in der Titelleiste und im Tabletmodus in der Taskleiste angezeigt. Das Ereignis der Schaltfläche „Zurück“ ist ein universelles Konzept, das für alle Gerätefamilien gilt. Für Schaltflächen, die in Hardware oder Software implementiert werden, wird das gleiche [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested)-Ereignis ausgelöst.

Das Beispiel unten funktioniert für alle Gerätefamilien und eignet sich gut für Fälle, in denen für alle Seiten die gleiche Verarbeitung gilt und in denen während der Navigation keine Bestätigung erforderlich ist (z. B. bei einer Warnung vor ungespeicherten Änderungen).

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

Es gibt auch einen einheitlichen Ansatz zum programmgesteuerten Beenden der App für alle Gerätefamilien.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>Charms

You don't need to change any of your code that integrates with charms, but you do need to add some UI to your app to take the place of the Charms bar, which is not a part of the Windows 10 shell. A Universal 8.1 app running on Windows 10 has its own replacement UI provided by system-rendered chrome in the app's title bar.

## <a name="controls-and-control-styles-and-templates"></a>Steuerelemente und Steuerelementstile/-vorlagen

A Universal 8.1 app running on Windows 10 will retain the 8.1 appearance and behavior with respect to controls. But, when you port that app to a Windows 10 app, there are some differences in appearance and behavior to be aware of. The architecture and design of controls is essentially unchanged for Windows 10 apps, so the changes are mostly around the design language, simplification, and usability improvements.

**Note**   The PointerOver visual state is relevant in custom styles/templates in Windows 10 apps and in Windows Runtime 8.x apps, but not in Windows Phone Store apps. For this reason (and because of the system resource keys that are supported for Windows 10 apps), we recommend that you re-use the custom styles/templates from your Windows Runtime 8.x apps when you're porting your app to Windows 10.
If you want to be certain that your custom styles/templates are using the latest set of visual states, and are benefitting from performance improvements made to the default styles/templates, then edit a copy of the new Windows 10 default template and re-apply your customization to that. Ein Beispiel für eine Leistungsverbesserung besteht darin, dass alle **Border**-Elemente, die ein **ContentPresenter**- Element oder Panel-Element bisher eingeschlossen haben, entfernt wurden. Der Rahmen wird jetzt von einem untergeordneten Element gerendert.

Unten sind einige speziellere Beispiele für Änderungen an Steuerelementen angegeben.

| Name des Steuerelements | Änderung |
|--------------|--------|
| **AppBar**   | If you are using the **AppBar** control ([**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) is recommended instead), then it is not hidden by default in a Windows 10 app. Sie können dies mit der [**AppBar.ClosedDisplayMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode)-Eigenschaft steuern. |
| **AppBar**, [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In a Windows 10 app, **AppBar** and [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) have a **See more** button (the ellipsis). |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In a Windows Runtime 8.x app, the secondary commands of a [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) are always visible. In a Windows Phone Store app, and in a Windows 10 app, the don't appear until the command bar opens. |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Bei einer Windows Phone Store-App hat der Wert von [**CommandBar.IsSticky**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.issticky) keinen Einfluss darauf, ob die Leiste einfach ausgeblendet werden kann. For a Windows 10 app, if **IsSticky** is set to true, then the **CommandBar** disregards a light dismiss gesture. |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In a Windows 10 app, [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) does not handle the [**EdgeGesture.Completed**](https://docs.microsoft.com/uwp/api/windows.ui.input.edgegesture.completed) nor [**UIElement.RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped) events. Außerdem wird nicht auf ein Tippen oder das Wischen nach oben reagiert. Sie haben dennoch die Möglichkeit, diese Ereignisse zu behandeln und [**IsOpen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.isopen) festzulegen. |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | Überprüfen Sie, wie Ihre App mit den visuellen Änderungen an [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) und [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) aussieht. For a Windows 10 app running on a mobile device, these controls no longer navigate to a selection page but instead use a light-dismissible popup. |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | In a Windows 10 app, you can't put [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) or [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) inside a fly-out. If you want those controls to be displayed in a popup-type control, then you can use [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) and [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout). |
| **GridView**, **ListView** | Informationen zu **GridView**/**ListView** finden Sie unter [GridView-/ListView-Änderungen](#gridview-and-listview-changes). |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | In einer Windows Phone Store-App wird ein [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)-Steuerelement vom letzten Abschnitt in den ersten Abschnitt umgebrochen. In a Windows Runtime 8.x app, and in a Windows 10 app, hub sections do not wrap around. |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | In einer Windows Phone Store-App wird das Hintergrundbild eines [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)-Steuerelements im Parallaxmodus relativ zu den Hubabschnitten verschoben. In a Windows Runtime 8.x app, and in a Windows 10 app, parallax is not used. |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)  | In einer universellen 8.1-App bewirkt die [**HubSection.IsHeaderInteractive**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hubsection.isheaderinteractive)-Eigenschaft, dass die Abschnittsüberschrift – und eine daneben gerenderte Chevronglyphe – interaktiv werden. In a Windows 10 app, there is an interactive "See more" affordance beside the header, but the header itself is not interactive. Mit **IsHeaderInteractive** wird weiterhin bestimmt, ob eine Interaktion das [**Hub.SectionHeaderClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hub.sectionheaderclick)-Ereignis auslöst. |
| **MessageDialog** | Wenn Sie **MessageDialog** verwenden, sollten Sie stattdessen ggf. das flexiblere [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)-Element nutzen. Informationen hierzu erhalten Sie auch im Beispiel [XAML-UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics). |
| **ListPickerFlyout**, **PickerFlyout**  | **ListPickerFlyout** and **PickerFlyout** are deprecated for a Windows 10 app. Verwenden Sie für ein einzelnes Auswahlflyout das [**MenuFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)-Element und bei komplexeren Oberflächen das [**Flyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)-Element. |
| [**PasswordBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) | The [**PasswordBox.IsPasswordRevealButtonEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.ispasswordrevealbuttonenabled) property is deprecated in a Windows 10 app, and setting it has no effect. Use [**PasswordBox.PasswordRevealMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) instead, which defaults to **Peek** (in which an eye glyph is displayed, like in a Windows Runtime 8.x app). Weitere Informationen finden Sie unter [Richtlinien für Kennwortfelder](https://docs.microsoft.com/windows/uwp/controls-and-patterns/password-box). |
| [**Pivot**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) | Das [**Pivot**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)-Steuerelement ist jetzt universell. Das heißt, seine Verwendung ist nicht mehr nur auf mobile Geräte beschränkt. |
| [**SearchBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SearchBox) | Obwohl [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) in der universellen Gerätefamilie implementiert ist, ist es auf mobilen Geräten nicht voll funktionsfähig. Weitere Informationen finden Sie unter [SearchBox eingestellt zugunsten von AutoSuggestBox](#searchbox-deprecated-in-favor-of-autosuggestbox). |
| **SemanticZoom** | Informationen zu **SemanticZoom** finden Sie unter [Änderungen an „SemanticZoom“](#semanticzoom-changes). |
| [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)  | Einige Standardeigenschaften von [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) haben sich geändert. [**HorizontalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) is **Auto**, [**VerticalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) is **Auto**, and [**ZoomMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode) is **Disabled**. Wenn die neuen Standardwerte für Ihre App nicht geeignet sind, können Sie sie entweder in einem Stil oder als lokale Werte für das Steuerelement selbst ändern.  |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | In a Windows Runtime 8.x app, spell-checking is off by default for a [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). In a Windows Phone Store app, and in a Windows 10 app, it is on by default. |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Die Standardschriftgröße für ein [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Element hat sich von 11 in 15 geändert. |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Der Standardwert von [**TextBox.TextReadingOrder**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.textreadingorder) wurde von **Default** in **DetectFromContent** geändert. Falls Sie dies nicht wünschen, können Sie **UseFlowDirection** verwenden. **Default** ist überholt. |
| Verschiedene | Accent color applies to a Windows Phone Store apps, and to Windows 10 apps, but not to Windows Runtime 8.x apps.  |

Weitere Informationen zu Steuerelementen für UWP-Apps finden Sie unter [Steuerelemente nach Funktion](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function), [Liste der Steuerelemente](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/) und [Richtlinien für Steuerelemente](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/index).

##  <a name="design-language-in-windows10"></a>Design language in Windows 10

There are some small but important differences in design language between Universal 8.1 apps and Windows 10 apps. Alle Details finden Sie unter [Design](https://developer.microsoft.com/en-us/windows/apps/design). Trotz der Änderungen bei der Entwurfssprache gelten nach wie vor dieselben Designprinzipien: Gestalten Sie Ihre App mit Liebe zum Detail, versuchen Sie aber, alles möglichst einfach zu halten, indem Sie sich auf den Inhalt, nicht auf das Chrom konzentrieren, visuelle Elemente weitgehend reduzieren und für die digitale Welt authentisch bleiben. Nutzen Sie insbesondere bei der Typografie eine visuelle Hierarchie. Entwerfen Sie Ihre App basierend auf einem Raster, und erwecken Sie Ihre Benutzeroberflächen mit flüssigen Animationen zum Leben.

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>Effektive Pixel, Abstand zum Bildschirm und Skalierungsfaktoren

Bisher wurden Anzeigepixel zum Abstrahieren der Größe und des Layouts von UI-Elementen von der tatsächlichen physischen Größe und Auflösung von Geräten verwendet. Anzeigepixel wurden zu effektiven Pixeln weiterentwickelt, und hier finden Sie eine Erklärung dieses Begriffs sowie Informationen zum zusätzlichen Wert.

Der Begriff „Auflösung“ bezeichnet ein Maß für die Pixeldichte und nicht wie allgemein angenommen für die Pixelanzahl. Die „effektive Auflösung“ ist die Art und Weise, wie die physischen Pixel, aus denen sich ein Bild oder eine Glyphe zusammensetzt, je nach Abstand zum Bildschirm und physischer Pixelgröße des Geräts für das Auge des Betrachters aufgelöst werden (die Pixeldichte ist der Kehrwert der physischen Pixelgröße). Die effektive Auflösung ist benutzerorientiert und somit eine gute Metrik für die Erstellung einer Benutzeroberfläche. Wenn Sie diese Faktoren verstehen und die Größe von UI-Elementen entsprechend steuern, können Sie eine optimale Benutzerfreundlichkeit erreichen.

Unterschiedliche Geräte besitzen eine unterschiedliche effektive Breite (angegeben in der Anzahl von Pixeln) – von 320 Epx bei besonders kleinen Geräten bis hin zu 1024 Epx bei Monitoren mittlerer Größe (und weit darüber hinaus für eine noch größere Breite). Sie müssen lediglich weiterhin Elemente mit automatischer Größenanpassung und dynamische Layoutbereiche verwenden. Im bestimmten Fällen werden die Eigenschaften der UI-Elemente im XAML-Markup auf eine feste Größe festgelegt. Auf Ihre App wird abhängig davon, auf welchem Gerät sie ausgeführt wird und welche Anzeigeeinstellungen der Benutzer festgelegt hat, automatisch ein Skalierungsfaktor angewendet. Dieser Skalierungsfaktor bewirkt, dass UI-Elemente mit fester Größe trotz unterschiedlicher Bildschirmgröße als Touchziel (und Leseziel) mit mehr oder weniger konstanter Größe angezeigt werden. In Kombination mit dem dynamischen Layout wird Ihre Benutzeroberfläche nicht nur auf verschiedenen Geräten optisch skaliert, auch die Inhaltsmenge wird an den verfügbaren Platz angepasst.

Damit Ihre App auf allen Displays optimal funktioniert, empfiehlt es sich, die einzelnen Bitmap-Ressourcen in verschiedenen Größen zu erstellen, die jeweils für einen bestimmten Skalierungsfaktor geeignet sind. Durch die Bereitstellung von Ressourcen mit einer Skalierung von 100 %, 200 % und 400 % (in dieser Prioritätsreihenfolge) erhalten Sie in den meisten Fällen auch bei allen Skalierungsfaktoren dazwischen hervorragende Ergebnisse.

**Note**  If, for whatever reason, you cannot create assets in more than one size, then create 100%-scale assets. In Microsoft Visual Studio bietet die Standardprojektvorlage für UWP-Apps Brandingressourcen (Bilder für Kacheln und Logos) in nur einer Größe, jedoch nicht mit einer Skalierung von 100 %. Befolgen Sie bei der Erstellung von Ressourcen für Ihre eigene App die Informationen in diesem Abschnitt, stellen Sie die Ressourcen mit einer Skalierung von 100 %, 200 % und 400 % bereit, und verwenden Sie Ressourcenpakete.

Falls Sie komplexe Grafiken verwenden, sollten Sie mehr Skalierungen bereitstellen. Wenn Sie mit Vektorgrafiken beginnen, ist es relativ einfach, qualitativ hochwertige Ressourcen für beliebige Skalierungsfaktoren zu generieren.

We don't recommend that you try to support all of the scale factors, but the full list of scale factors for Windows 10 apps is 100%, 125%, 150%, 200%, 250%, 300%, and 400%. Wenn Sie sie bereitstellen, wählt der Store für jedes Gerät die Ressourcen mit der passenden Größe aus, und es werden nur diese Ressourcen heruntergeladen. Die Auswahl der herunterzuladenden Ressourcen erfolgt auf Grundlage des DPI-Werts eines Geräts. You can re-use assets from your Windows Runtime 8.x app at scale factors such as 140% and 220%, but your app will run at one of the new scale factors and so some bitmap scaling will be unavoidable. Testen Sie Ihre App auf verschiedenen Geräten, um jeweils zu überprüfen, ob ein zufriedenstellendes Ergebnis erzielt wird.

You may be re-using XAML markup from a Windows Runtime 8.x app where literal dimension values are used in the markup (perhaps to size shapes or other elements, perhaps for typography). But, in some cases, a larger scale factor is used on a device for a Windows 10 app than for a Universal 8.1 app (for example, 150% is used where 140% was before, and 200% is used where 180% was). So, if you find that these literal values are now too big on Windows 10, then try multiplying them by 0.8. Weitere Informationen finden Sie unter [Reaktionsfähiges Design – Grundlagen für UWP-Apps](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

## <a name="gridview-and-listview-changes"></a>GridView-/ListView-Änderungen

An den Standardstilsettern für [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) wurden verschiedene Änderungen vorgenommen, damit der Bildlauf des Steuerelements vertikal verläuft (anstatt wie bisher standardmäßig horizontal). Wenn Sie in Ihrem Projekt eine Kopie des Standardstils bearbeitet haben, enthält Ihre Kopie diese Änderungen nicht. Sie müssen sie daher manuell vornehmen. Nachfolgend finden Sie eine Liste der Änderungen:

-   Der Setter für [**ScrollViewer.HorizontalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) wurde von **Auto** in **Disabled** geändert.
-   Der Setter für [**ScrollViewer.VerticalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) wurde von **Disabled** in **Auto** geändert.
-   Der Setter für [**ScrollViewer.HorizontalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) wurde von **Enabled** in **Disabled** geändert.
-   Der Setter für [**ScrollViewer.VerticalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) wurde von **Disabled** in **Enabled** geändert.
-   Im Setter für [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) wurde der Wert von [**ItemsWrapGrid.Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid.orientation) von **Vertical** in **Horizontal** geändert.

Falls Ihnen die letzte Änderung (die Änderung an **Orientation**) widersprüchlich erscheint, beachten Sie, dass wir von einem Umbruchraster sprechen. Ein horizontal ausgerichtetes Umbruchraster (der neue Wert) ähnelt einem Schriftsystem, bei dem Text horizontal fließt und am Ende einer Seite ein Zeilenumbruch erfolgt. Bei einer solchen Seite mit Text erfolgt der Bildlauf in vertikaler Richtung. Im Gegensatz dazu ähnelt ein vertikal ausgerichtetes Umbruchraster (der vorherige Wert) einem Schriftsystem, in dem Text vertikal fließt und daher ein horizontaler Bildlauf erfolgt.

Here are the aspects of [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) and [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) that have change or are not supported in Windows 10.

-   The [**IsSwipeEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isswipeenabled) property (Windows Runtime 8.x apps only) is not supported for Windows 10 apps. Die API ist noch vorhanden, ihre Verwendung hat aber keine Auswirkung. Mit Ausnahme von der Wischbewegung nach unten (wird nicht unterstützt, da sie den Daten zufolge nicht gefunden wird) und dem Rechtsklick (ist für die Anzeige eines Kontextmenüs reserviert) werden alle bisherigen Auswahlbewegungen unterstützt.
-   The [**ReorderMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.reordermode) property (Windows Phone Store apps only) is not supported for Windows 10 apps. Die API ist noch vorhanden, ihre Verwendung hat aber keine Auswirkung. Legen Sie stattdessen [**AllowDrop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.allowdrop) und [**CanReorderItems**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) für Ihre **GridView** oder **ListView** auf „true” fest. Der Benutzer kann die Anordnung dann durch Drücken und Halten (oder Klicken und Ziehen) ändern.
-   When developing for Windows 10, use [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter) instead of [**GridViewItemPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemPresenter) in your item container style, both for [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) and for [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView). Wenn Sie eine Kopie der Standard-Elementcontainerstile bearbeiten, erhalten Sie den richtigen Typ.
-   The selection visuals have changed for a Windows 10 app. Wenn Sie [**SelectionMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) auf **Multiple** festlegen, wird standardmäßig für jedes Element ein Kontrollkästchen gerendert. Bei der Standardeinstellung für **ListView**-Elemente wird das Kontrollkästchen inline neben dem Element angeordnet, wodurch der vom restlichen Element belegte Platz etwas reduziert und verschoben wird. Bei **GridView**-Elementen überlagert das Kontrollkästchen standardmäßig das Element. In beiden Fällen können Sie jedoch wie im folgenden Beispiel gezeigt in Ihrem Elementcontainerstil das Layout (Inline oder Überlagerung) der Kontrollkästchen (mit der [**CheckMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode)-Eigenschaft) steuern und (mit der [**SelectionCheckMarkVisualEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled)-Eigenschaft) festlegen, ob sie überhaupt für das [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter)-Element angezeigt werden.
-   In Windows 10, the [**ContainerContentChanging**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) event is raised twice per item during UI virtualization: once for the reclaim, and once for the re-use. Wenn der Wert der [**InRecycleQueue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.inrecyclequeue) gleich **true** ist und Sie keine spezielle Freigabe vornehmen müssen, können Sie Ihren Ereignishandler sofort mit der Gewissheit beenden, dass er erneut aktiviert wird, wenn das gleiche Element wiederverwendet wird (dabei lautet **InRecycleQueue** dann **false**).

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![ListViewItemPresenter-Element mit Inlinekontrollkästchen](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

ListViewItemPresenter-Element mit Inlinekontrollkästchen

![ListViewItemPresenter-Element mit überlagertem Kontrollkästchen](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

ListViewItemPresenter-Element mit überlagertem Kontrollkästchen

-   Da die Wischbewegung nach unten und der Rechtsklick als Auswahlbewegungen (aus den oben genannten Gründen) entfernt wurden, hat sich das Interaktionsmodell geändert. Unter anderem schließen sich das [**ItemClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick)-Ereignis und das [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Ereignis nicht mehr gegenseitig aus. For your Windows 10 app, review your scenarios and decide whether to adopt the "selection" or the "invoke" interaction model. Ausführliche Informationen finden Sie unter [So wird's gemacht: Ändern des Interaktionsmodus](https://docs.microsoft.com/previous-versions/windows/apps/hh780625(v=win.10)).
-   Die zum Formatieren des [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter)-Elements verwendeten Eigenschaften wurden teilweise geändert. Folgende Eigenschaften sind neu: [**CheckBoxBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkboxbrush), [**PressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pressedbackground), [**SelectedPressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpressedbackground) und [**FocusSecondaryBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.focussecondaryborderbrush). Properties that are ignored for a Windows 10 app are [**Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.padding) (use [**ContentMargin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.contentmargin) instead), [**CheckHintBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkhintbrush), [**CheckSelectingBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkselectingbrush), [**PointerOverBackgroundMargin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pointeroverbackgroundmargin), [**ReorderHintOffset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.reorderhintoffset), [**SelectedBorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedborderthickness), and [**SelectedPointerOverBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpointeroverborderbrush).

In der folgenden Tabelle werden die Änderungen an den visuellen Zuständen und Gruppen visueller Zustände in der [**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem)-Steuerelementvorlage und der [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem)-Steuerelementvorlage beschrieben.

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | Normal                  |                   | Normal              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Gedrückt                 |                   | Gedrückt             |
|                     | PointerOverPressed      |                   | [nicht verfügbar]       |
|                     | Deaktiviert                |                   | [nicht verfügbar]       |
|                     | [nicht verfügbar]           |                   | PointerOverSelected |
|                     | [nicht verfügbar]           |                   | Ausgewählt            |
|                     | [nicht verfügbar]           |                   | PressedSelected     |
| [nicht verfügbar]       |                         | DisabledStates    |                     |
|                     | [nicht verfügbar]           |                   | Deaktiviert            |
|                     | [nicht verfügbar]           |                   | Enabled             |
| SelectionHintStates |                         | [nicht verfügbar]     |                     |
|                     | VerticalSelectionHint   |                   | [nicht verfügbar]       |
|                     | HorizontalSelectionHint |                   | [nicht verfügbar]       |
|                     | NoSelectionHint         |                   | [nicht verfügbar]       |
| [nicht verfügbar]       |                         | MultiSelectStates |                     |
|                     | [nicht verfügbar]           |                   | MultiSelectDisabled |
|                     | [nicht verfügbar]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [nicht verfügbar]     |                     |
|                     | Unselecting             |                   | [nicht verfügbar]       |
|                     | Unselected              |                   | [nicht verfügbar]       |
|                     | UnselectedPointerOver   |                   | [nicht verfügbar]       |
|                     | UnselectedSwiping       |                   | [nicht verfügbar]       |
|                     | Selecting               |                   | [nicht verfügbar]       |
|                     | Ausgewählt                |                   | [nicht verfügbar]       |
|                     | SelectedSwiping         |                   | [nicht verfügbar]       |
|                     | SelectedUnfocused       |                   | [nicht verfügbar]       |

Wenn Sie über eine benutzerdefinierte [**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem)-Steuerelementvorlage oder [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem)-Steuerelementvorlage verfügen, überprüfen Sie sie im Hinblick auf die oben genannten Änderungen. Wir empfehlen, eine Kopie der neuen Standardvorlage zu bearbeiten und Ihre Anpassungen erneut auf diese Kopie anzuwenden. Wenn dies aus irgendeinem Grund nicht möglich ist und Sie Ihre vorhandene Vorlage bearbeiten müssen, können Sie nach den folgenden allgemeinen Richtlinien vorgehen.

-   Fügen Sie die neue Gruppe visueller Zustände „MultiSelectStates” hinzu.
-   Fügen Sie den neuen visuellen Zustand „MultiSelectDisabled” hinzu.
-   Fügen Sie den neuen visuellen Zustand „MultiSelectEnabled” hinzu.
-   Fügen Sie die neue Gruppe visueller Zustände „DisabledStates” hinzu.
-   Fügen Sie den neuen visuellen Zustand „Enabled” hinzu.
-   Entfernen Sie in der Gruppe visueller Zustände „CommonStates” den visuellen Zustand „PointerOverPressed”.
-   Verschieben den visuellen Zustand „Disabled” in die Gruppe visueller Zustände „DisabledStates”.
-   Fügen Sie den neuen visuellen Zustand „PointerOverSelected” hinzu.
-   Fügen Sie den neuen visuellen Zustand „PressedSelected” hinzu.
-   Entfernen Sie die Gruppe visueller Zustände „SelectedHintStates”.
-   Verschieben Sie in der Gruppe visueller Zustände „SelectionStates” den visuellen Zustand „Selected” in die Gruppe visueller Zustände „CommonStates”.
-   Entfernen Sie die gesamte Gruppe visueller Zustände „SelectionStates”.

## <a name="localization-and-globalization"></a>Lokalisierung und Globalisierung

Die Dateien vom Typ „Resources.resw“ aus Ihrem universellen 8.1-Projekt können in Ihrem UWP-App-Projekt wiederverwendet werden. Fügen Sie die kopierte Datei dem Projekt hinzu, und legen Sie **Buildvorgang** auf **PRIResource** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** fest. Im Thema [**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) wird beschrieben, wie Sie gerätefamilienspezifische Ressourcen auf der Grundlage des Ressourcenauswahlfaktors für die Gerätefamilie laden.

## <a name="play-to"></a>Wiedergeben auf

The APIs in the [**Windows.Media.PlayTo**](https://docs.microsoft.com/uwp/api/Windows.Media.PlayTo) namespace are deprecated for Windows 10 apps in favor of the [**Windows.Media.Casting**](https://docs.microsoft.com/uwp/api/Windows.Media.Casting) APIs.

## <a name="resource-keys-and-textblock-style-sizes"></a>Ressourcenschlüssel und Größen von TextBlock-Stilen

The design language has evolved for Windows 10 and consequently certain system styles have changed. In einigen Fällen empfiehlt sich die Überarbeitung des visuellen Designs Ihrer Ansichten, damit sie mit den geänderten Stileigenschaften in Einklang stehen.

In anderen Fällen werden die Ressourcenschlüssel nicht mehr unterstützt. Der XAML-Markup-Editor in Visual Studio hebt Verweise auf Ressourcenschlüssel hervor, die nicht aufgelöst werden können. Der XAML-Markup-Editor unterstreicht z. B. einen Verweis auf den Stilschlüssel `ListViewItemTextBlockStyle` mit einer roten Wellenlinie. Wird dieser Fehler nicht behoben, wird die App sofort beendet, wenn Sie versuchen, sie im Emulator oder auf dem Gerät bereitzustellen. Daher ist es wichtig, die Richtigkeit des XAML-Markups sicherzustellen. Sie werden feststellen, dass sich solche Fehler mit Visual Studio hervorragend abfangen lassen.

Für weiterhin unterstützte Schlüssel bedeuten Änderungen der Entwurfssprache, dass sich die Eigenschaften geändert haben, die von einigen Stilen festgelegt werden. For example, `TitleTextBlockStyle` sets **FontSize** to 14.667px in a Windows Runtime 8.x app and 18.14px in a Windows Phone Store app. But, the same style sets **FontSize** to a much larger 24px in a Windows 10 app. Überprüfen Sie Ihre Entwürfe und Layouts, und verwenden Sie die passenden Stile an den richtigen Stellen. Weitere Informationen finden Sie unter [Richtlinien für Schriftarten](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) und [Entwerfen von UWP-Apps](https://developer.microsoft.com/en-us/windows/apps/design).

Im Anschluss finden Sie die vollständige Liste der nicht mehr unterstützten Schlüssel.

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   ControlHeaderContentPresenterStyle
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   HeaderContentPresenterStyle
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   LeftFullWindowMargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>SearchBox eingestellt zugunsten von AutoSuggestBox

Obwohl [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) in der universellen Gerätefamilie implementiert ist, ist es auf mobilen Geräten nicht voll funktionsfähig. Verwenden Sie [**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) für Ihre universelle Sucherfahrung. Nachfolgend finden Sie die gängige Vorgehensweise zur Implementierung einer Sucherfahrung mit **AutoSuggestBox**.

Sobald der Benutzer mit der Eingabe beginnt, wird das **TextChanged**-Ereignis mit der Ursache **UserInput** ausgelöst. Füllen Sie anschließend die Liste der Vorschläge auf, und legen Sie die **ItemsSource** von [**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) fest. Während der Benutzer durch die Liste navigiert, wird das **SuggestionChosen**-Ereignis ausgelöst (und wenn Sie **TextMemberDisplayPath** festgelegt haben), wird das Textfeld automatisch mit der angegebenen Eigenschaft aufgefüllt. Wenn der Benutzer eine Auswahl mit der EINGABETASTE übermittelt, wird das **QuerySubmitted**-Ereignis ausgelöst. An dieser Stelle können Sie auf diesen Vorschlag reagieren (in diesem Fall wahrscheinlich das Navigieren zu einer anderen Seite mit weiteren Details zu den angegebenen Inhalten). Beachten Sie, dass die **LinguisticDetails**-Eigenschaft und die **Language**-Eigenschaft von **SearchBoxQuerySubmittedEventArgs** nicht mehr unterstützt werden (es gibt entsprechende APIs, um diese Funktion zu unterstützen). Und **KeyModifiers** wird nicht mehr unterstützt.

[**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) also has support for input method editors (IMEs). Und wenn Sie ein „Suchen“-Symbol anzeigen möchten, ist das ebenfalls möglich (durch die Interaktion mit dem Symbol wird das **QuerySubmitted**-Ereignis ausgelöst).

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

Siehe auch [Beispiel für AutoSuggestBox-Portierung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox).

## <a name="semanticzoom-changes"></a>Änderungen an „SemanticZoom”

Die Bewegung zum Verkleinern für [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) wurde im Windows Phone-Modell zusammengeführt. Hierzu wird auf einen Gruppenkopf getippt oder geklickt (auf Desktopcomputern wird also das Angebot der Schaltfläche „Minus“ zum Verkleinern nicht mehr angezeigt). Jetzt erhalten wir das gleiche, einheitliche Verhalten kostenlos auf allen Geräten. Es besteht der kosmetische Unterschied zum Windows Phone-Modell, dass die verkleinerte Ansicht (Sprungliste) die vergrößerte Ansicht ersetzt, anstatt sie zu überlagern. Aus diesem Grund können Sie alle halbtransparenten Hintergründe aus verkleinerten Ansichten entfernen.

In a Windows Phone Store app, the zoomed-out view expands to the size of the screen. In a Windows Runtime 8.x app, and in a Windows 10 app, the size of the zoomed-out view is constrained to the bounds of the **SemanticZoom** control.

In einer Windows Phone Store-App scheinen Inhalte hinter der verkleinerten Ansicht (in Z-Reihenfolge) durch, wenn für den Hintergrund der verkleinerten Ansicht eine Transparenz festgelegt wurde. In a Windows Runtime 8.x app, and in a Windows 10 app, nothing is visible behind the zoomed out view.

In a Windows Runtime 8.x app, when the app is deactivated and reactivated, the zoomed-out view is dismissed (if it was being shown) and the zoomed-in view is shown instead. In a Windows Phone Store app, and in a Windows 10 app, the zoomed-out view will remain showing if it was being shown.

In a Windows Phone Store app, and in a Windows 10 app, the zoomed-out view is dismissed when the back button is pressed. For a Windows Runtime 8.x app, there is no built-in back button processing, so the question doesn't apply.

## <a name="settings"></a>„Einstellungen“

The Windows Runtime 8.x **SettingsPane** class is not appropriate for Windows 10. Zusätzlich zur Erstellung einer Einstellungsseite sollten Sie Benutzern stattdessen die Möglichkeit geben, von der App aus auf die Seite zuzugreifen. Es wird empfohlen, diese Seite mit App-Einstellungen auf der obersten Ebene als letztes angeheftetes Element im Navigationsbereich verfügbar zu machen. Hier sehen Sie jedoch den vollständigen Satz von Optionen.

-   Navigationsbereich. Einstellungen sollten das letzte Element in der Navigationsliste und am Ende der Liste angeheftet sein.
-   App-Leiste/Symbolleiste (innerhalb einer Registerkartenansicht oder eines Pivot-Layouts). Einstellungen sollten das letzte Element im Flyout „Menü“ der App-Leiste oder Symbolleiste sein. Es wird davon abgeraten, Einstellungen als eines der Elemente der obersten Ebene in der Navigation zu verwenden.
-   Hub. Einstellungen sollte innerhalb des Flyouts „Menü“ platziert werden (über das Menü der App-Leiste oder Symbolleiste im Hublayout).

Es wird davon abgeraten, Einstellungen in einem Master-/Detailbereich zu verstecken.

Die Einstellungsseite sollte das gesamte App-Fenster ausfüllen, und außerdem sollten „Info“ und „Feedback“ auf der Einstellungsseite zu finden sein. Richtlinien für den Entwurf der Einstellungsseite finden Sie unter [Richtlinien für App-Einstellungen](https://docs.microsoft.com/windows/uwp/app-settings/guidelines-for-app-settings).

## <a name="text"></a>Text

Der Text (bzw. die Typografie) ist ein wichtiger Aspekt einer UWP-App. Beim Portieren ist es ratsam, das grafische Design Ihrer Ansichten noch einmal darauf zu prüfen, ob es zur neuen Entwurfssprache passt. Verwenden Sie die folgenden Abbildungen, um nach den verfügbaren **TextBlock**-Systemstilen der universellen Windows-Plattform (UWP) zu suchen. Find the ones that correspond to the Windows Phone Silverlight styles you used. Alternatively, you can create your own universal styles and copy the properties from the Windows Phone Silverlight system styles into those.

![textblock-Systemstile für Windows 10-Apps](images/label-uwp10stylegallery.png) <br/>System TextBlock styles for Windows 10 apps

In Windows Runtime 8.x apps and Windows Phone Store apps, the default font family is Global User Interface. In a Windows 10 app, the default font family is Segoe UI. Daher kann die Schriftartmetrik in Ihrer App Unterschiede aufweisen. Wenn Sie die Darstellung Ihres 8.1-Texts reproduzieren möchten, können Sie Ihre eigene Metrik mit Eigenschaften wie [**LineHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) und [**LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy) festlegen.

In Windows Runtime 8.x apps and Windows Phone Store apps, the default language for text is set to the language of the build, or to en-us. In a Windows 10 app, the default language is set to the top app language (font fallback). Sie können [**FrameworkElement.Language**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.language) explizit festlegen, aber Sie erhalten ein besseres Fallbackverhalten für Schriftarten, wenn Sie für diese Eigenschaft keinen Wert festlegen.

Weitere Informationen finden Sie unter [Richtlinien für Schriftarten](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) und [Entwerfen von UWP-Apps](https://developer.microsoft.com/). Informationen zu Änderungen von Textsteuerelementen finden Sie oben im Abschnitt [Steuerelemente](#controls-and-control-styles-and-templates).

## <a name="theme-changes"></a>Designänderungen

Bei einer universellen 8.1-App ist das Standarddesign standardmäßig dunkel. For Windows 10 devices, the default theme has changed, but you can control the theme used by declaring a requested theme in App.xaml. Wenn Sie z. B. auf allen Geräten ein dunkles Design verwenden möchten, fügen Sie dem Stammelement der App `RequestedTheme="Dark"` hinzu.

## <a name="tiles-and-toasts"></a>Kacheln und Popups

For tiles and toasts, the templates you're currently using will continue to work in your Windows 10 app. Es sind jedoch neue adaptive Vorlagen verfügbar. Diese werden unter [Benachrichtigungen, Kacheln, Popups und Signale](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications) beschrieben.

Auf Desktopcomputern war eine Popupbenachrichtigung früher eine vorübergehende Nachricht. Sie wurde ausgeblendet und konnte nicht mehr aufgerufen werden, nachdem sie geschlossen oder ignoriert wurde. Wenn unter Windows Phone eine Popupbenachrichtigung ignoriert oder vorerst geschlossen wird, wird sie ins Info-Center verschoben. Das Info-Center ist nun nicht mehr auf die Mobilgerätefamilie beschränkt.

Zum Senden einer Popupbenachrichtigung muss keine Funktion mehr deklariert werden.

## <a name="window-size"></a>Fenstergröße

Bei einer universellen 8.1-App wird das App-Manifestelement [**ApplicationView**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema2013/element-applicationview) verwendet, um eine minimale Fensterbreite zu deklarieren. In Ihrer UWP-App können Sie eine Mindestgröße (Breite und Höhe) mit imperativem Code angeben. Die Standardmindestgröße beträgt 500 x 320 Epx. Dies ist auch die kleinste zulässige Mindestgröße. Die größte zulässige Mindestgröße ist 500 x 500 Epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

Das nächste Thema ist [Portieren: E/A, Gerät und App-Modell](w8x-to-uwp-input-and-sensors.md).

