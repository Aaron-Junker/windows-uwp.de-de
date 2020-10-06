---
description: Die Vorgehensweise zum Definieren einer Benutzeroberfläche in Form von deklarativem XAML-Markup lässt sich sehr gut von universellen 8.1-Apps auf Apps für die universelle Windows-Plattform (UWP) übertragen.
title: Portieren von Windows-Runtime 8.x-XAML und -UI zu UWP
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 62085377da89d64c8ba0799dc6bab13c17675f90
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750676"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Portieren von Windows-Runtime 8.x-XAML und -UI zu UWP


Im vorherigen Thema haben wir uns mit der [Problembehandlung](w8x-to-uwp-troubleshooting.md) beschäftigt.

Die Vorgehensweise zum Definieren einer Benutzeroberfläche in Form von deklarativem XAML-Markup lässt sich sehr gut von universellen 8.1-Apps auf Apps für die universelle Windows-Plattform (UWP) übertragen. Sie werden feststellen, dass der Großteil des Markups kompatibel ist. Unter Umständen sind jedoch einige Anpassungen bei Systemressourcenschlüsseln oder benutzerdefinierten Vorlagen erforderlich. Beim imperativen Code in Ihren Ansichtsmodellen sind nur geringfügige oder gar keine Änderungen erforderlich. Und sogar ein Großteil des Codes in Ihrer Darstellungsschicht zur Manipulierung von UI-Elementen dürfte sich problemlos portieren lassen.

## <a name="imperative-code"></a>Imperativer Code

Falls Sie lediglich die Projekterstellungsphase erreichen möchten, können Sie sämtlichen nicht unbedingt erforderlichen Code auskommentieren. Gehen Sie anschließend nacheinander die einzelnen Probleme anhand der Informationen in den folgenden Themen dieses Abschnitts (einschließlich des vorherigen Themas [Problembehandlung](w8x-to-uwp-troubleshooting.md)) durch, bis alle Erstellungs- und Laufzeitprobleme behoben sind und die Portierung abgeschlossen ist.

## <a name="adaptiveresponsive-ui"></a>Adaptive/reaktionsfähige Benutzeroberfläche

Da Ihre App potenziell auf vielen verschiedenen Geräten ausgeführt werden kann – jeweils mit eigener Bildschirmgröße und -auflösung – ist es ratsam, nicht nur die mindestens erforderlichen Schritte zum Portieren Ihrer App auszuführen und die UI so zu gestalten, dass sie auf allen Geräten möglichst gut aussieht. Sie können das adaptive Visual State-Manager-Feature nutzen, um die Fenstergröße dynamisch zu ermitteln und als Reaktion darauf das Layout zu ändern. Ein Beispiel zur Vorgehensweise finden Sie im Abschnitt [Adaptive UI](w8x-to-uwp-case-study-bookstore2.md) im Thema mit der Bookstore2-Fallstudie.

## <a name="back-button-handling"></a>Behandeln der Schaltfläche „Zurück“

Bei universellen 8,1-apps haben Windows-Runtime 8. x-apps und Windows Phone Store-Apps verschiedene Ansätze für die Benutzeroberfläche, die Sie anzeigen, und die Ereignisse, die Sie für die Schaltfläche "zurück" behandeln. Bei Windows 10-Apps können Sie in Ihrer App aber einen einheitlichen Ansatz verwenden. Auf mobilen Geräten wird die Schaltfläche für Sie als kapazitive Schaltfläche auf dem Gerät oder als Schaltfläche in der Shell bereitgestellt. Auf einem Desktopgerät fügen Sie den Chromelementen der App eine Schaltfläche hinzu, wenn die Rückwärtsnavigation in der App möglich ist. Sie wird für Apps mit Fenstern in der Titelleiste und im Tabletmodus in der Taskleiste angezeigt. Das Ereignis der Schaltfläche „Zurück“ ist ein universelles Konzept, das für alle Gerätefamilien gilt. Für Schaltflächen, die in Hardware oder Software implementiert werden, wird das gleiche [**BackRequested**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested)-Ereignis ausgelöst.

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

Der Code für die Charm-Integration muss zwar nicht geändert werden, Sie müssen Ihrer App aber einige UI-Elemente hinzufügen, die den Platz der Charm-Leiste einnehmen, da diese in Windows 10 nicht enthalten ist. Für eine universelle 8.1-App unter Windows 10 sind vom systemeigenen Chrom bereitgestellte UI-Elemente auf der Titelleiste der App verfügbar.

## <a name="controls-and-control-styles-and-templates"></a>Steuerelemente und Steuerelement Stile und-Vorlagen

Eine universelle 8.1-App behält unter Windows 10 das Aussehen und Verhalten von 8.1 in Bezug auf die Steuerelemente bei. Wenn Sie diese App zu einer Windows 10-App portieren, sind aber einige Unterschiede beim Aussehen und Verhalten zu beachten. Die Architektur und der Entwurf von Steuerelementen sind für Windows 10-apps im Wesentlichen unverändert, sodass sich die Änderungen größtenteils auf die Entwurfs Sprache, die Vereinfachung und die Verbesserungen bei der Benutzerfreundlichkeit erst

**Hinweis**    Der visuelle Zustand "pointerover" ist für benutzerdefinierte Stile/Vorlagen in Windows 10-apps und in Windows-Runtime 8. x-apps relevant, aber nicht für Windows Phone Store-Apps. Aus diesem Grund (und aufgrund der Systemressourcen Schlüssel, die für Windows 10-apps unterstützt werden) empfiehlt es sich, die benutzerdefinierten Stile/Vorlagen aus Ihren Windows-Runtime 8. x-apps erneut zu verwenden, wenn Sie Ihre APP auf Windows 10 portieren.
Wenn Sie sichergehen möchten, dass für Ihre benutzerdefinierten Stile/Vorlagen die aktuelle Gruppe der Ansichtszustände verwendet wird und dass Sie von Leistungsverbesserungen profitieren, die an den standardmäßigen Stilen/Vorlagen vorgenommen wurden, sollten Sie eine Kopie der neuen Windows 10-Standardvorlage bearbeiten und Ihre Anpassungen darauf anwenden. Ein Beispiel für eine Leistungsverbesserung besteht darin, dass alle **Border**-Elemente, die ein **ContentPresenter**- Element oder Panel-Element bisher eingeschlossen haben, entfernt wurden. Der Rahmen wird jetzt von einem untergeordneten Element gerendert.

Unten sind einige speziellere Beispiele für Änderungen an Steuerelementen angegeben.

| Steuerelementname | Change |
|--------------|--------|
| **AppBar**   | Wenn Sie das **AppBar**-Steuerelement verwenden (empfohlen wird [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar)), ist es in einer Windows 10-App standardmäßig nicht ausgeblendet. Sie können dies mit der [**AppBar.ClosedDisplayMode**](/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode)-Eigenschaft steuern. |
| **AppBar**, [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In einer Windows 10-App verfügen **AppBar** und [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) über die Schaltfläche **Mehr anzeigen** (Auslassungspunkte). |
| [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In einer Windows-Runtime 8. x-APP sind die sekundären Befehle einer [**Befehlsleiste**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) immer sichtbar. In einer Windows Phone Store-App und in einer Windows 10-App werden sie erst angezeigt, wenn die Befehlsleiste geöffnet wird. |
| [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Bei einer Windows Phone Store-App hat der Wert von [**CommandBar.IsSticky**](/uwp/api/windows.ui.xaml.controls.appbar.issticky) keinen Einfluss darauf, ob die Leiste einfach ausgeblendet werden kann. Bei einer Windows 10-App ignoriert die **CommandBar** eine Bewegung für einfaches Ausblenden, wenn **IsSticky** auf „true“ festgelegt ist. |
| [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) | In einer Windows 10-App wird vom [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar)-Element weder das [**EdgeGesture.Completed**](/uwp/api/windows.ui.input.edgegesture.completed)-Ereignis noch das [**UIElement.RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)-Ereignis behandelt. Außerdem wird nicht auf ein Tippen oder das Wischen nach oben reagiert. Sie haben dennoch die Möglichkeit, diese Ereignisse zu behandeln und [**IsOpen**](/uwp/api/windows.ui.xaml.controls.appbar.isopen) festzulegen. |
| [**DatePicker**](/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [**TimePicker**](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | Überprüfen Sie, wie Ihre App mit den visuellen Änderungen für [**DatePicker**](/uwp/api/Windows.UI.Xaml.Controls.DatePicker) und [**TimePicker**](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) aussieht. Bei einer auf einem mobilen Gerät ausgeführten Windows 10-App rufen diese Steuerelemente keine Auswahlseite mehr auf, sondern ein Popup, das einfach ausgeblendet werden kann. |
| [**DatePicker**](/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [**TimePicker**](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | In einer Windows 10-App können Sie [**DatePicker**](/uwp/api/Windows.UI.Xaml.Controls.DatePicker) oder [**timePicker**](/uwp/api/Windows.UI.Xaml.Controls.TimePicker) nicht in einen Ablaufbetrieb einfügen. Wenn Sie möchten, dass diese Steuerelemente in einem Popup-Steuerelement angezeigt werden, können Sie [**datepickerflyout**](/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) und [**timepickerflyout**](/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout)verwenden. |
| **GridView**, **ListView** | Informationen zu **GridView** / -**ListView**finden Sie unter [GridView-und ListView-Änderungen](#gridview-and-listview-changes). |
| [**SK**](/uwp/api/Windows.UI.Xaml.Controls.Hub) | In einer Windows Phone Store-App wird ein [**Hub**](/uwp/api/Windows.UI.Xaml.Controls.Hub)-Steuerelement vom letzten Abschnitt in den ersten Abschnitt umgebrochen. In einer Windows-Runtime 8. x-APP und in einer Windows 10-App werden die Hub-Abschnitte nicht umschlossen. |
| [**SK**](/uwp/api/Windows.UI.Xaml.Controls.Hub) | In einer Windows Phone Store-App wird das Hintergrundbild eines [**Hub**](/uwp/api/Windows.UI.Xaml.Controls.Hub)-Steuerelements im Parallaxmodus relativ zu den Hubabschnitten verschoben. In einer Windows-Runtime 8. x-APP und in einer Windows 10-APP wird "Parser" nicht verwendet. |
| [**SK**](/uwp/api/Windows.UI.Xaml.Controls.Hub)  | In einer universellen 8.1-App bewirkt die [**HubSection.IsHeaderInteractive**](/uwp/api/windows.ui.xaml.controls.hubsection.isheaderinteractive)-Eigenschaft, dass die Abschnittsüberschrift – und eine daneben gerenderte Chevronglyphe – interaktiv werden. In einer Windows 10-App wird neben der Überschrift das interaktive Angebot vom Typ „Mehr anzeigen“ verwendet, aber die eigentliche Überschrift ist nicht interaktiv. Mit **IsHeaderInteractive** wird weiterhin bestimmt, ob die Interaktion das [**Hub.SectionHeaderClick**](/uwp/api/windows.ui.xaml.controls.hub.sectionheaderclick)-Ereignis auslöst. |
| **MessageDialog** | Wenn Sie **MessageDialog** verwenden, sollten Sie stattdessen ggf. das flexiblere [**ContentDialog**](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)-Element nutzen. Informationen hierzu erhalten Sie auch im Beispiel [XAML-UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics). |
| **ListPickerFlyout**, **PickerFlyout**  | **ListPickerFlyout** und **PickerFlyout** sind für Windows 10-App veraltet. Verwenden Sie [**menuflyout**](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), um eine einzelne Auswahl zu erhalten. Verwenden Sie für komplexere Erfahrungen [**Flyout**](/uwp/api/Windows.UI.Xaml.Controls.Flyout). |
| [**PasswordBox**](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) | Die [**PasswordBox.IsPasswordRevealButtonEnabled**](/uwp/api/windows.ui.xaml.controls.passwordbox.ispasswordrevealbuttonenabled)-Eigenschaft ist in einer Windows 10-App veraltet, und das Festlegen hat keinerlei Auswirkung. Verwenden Sie stattdessen [**PasswordBox. passwordrevealmode**](/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) , das standardmäßig **Peek** ist (in dem ein Eye-Symbol angezeigt wird, z. b. in einer Windows-Runtime 8. x-APP). Weitere Informationen finden Sie unter [Richtlinien für Kennwortfelder](../design/controls-and-patterns/password-box.md). |
| [**Pivot**](/uwp/api/Windows.UI.Xaml.Controls.Pivot) | Das [**Pivot**](/uwp/api/Windows.UI.Xaml.Controls.Pivot)-Steuerelement ist jetzt universell. Das heißt, die Verwendung ist nicht mehr nur auf mobile Geräte beschränkt. |
| [**SearchBox**](/uwp/api/Windows.UI.Xaml.Controls.SearchBox) | Obwohl [**SearchBox**](/uwp/api/windows.ui.xaml.controls.searchbox) in der universellen Gerätefamilie implementiert ist, ist es auf mobilen Geräten nicht voll funktionsfähig. Weitere Informationen finden Sie [unter "SearchBox" als veraltet](#searchbox-deprecated-in-favor-of-autosuggestbox). |
| **SemanticZoom** | Informationen zu **semanticzoom**finden Sie unter [semanticzoom-Änderungen](#semanticzoom-changes). |
| [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)  | Einige Standardeigenschaften von [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) wurden geändert. [**HorizontalScrollMode**](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) entspricht **Auto**, [**VerticalScrollMode**](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) entspricht **Auto**, und [**ZoomMode**](/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode) entspricht **Disabled**. Wenn die neuen Standardwerte für Ihre App nicht geeignet sind, können Sie sie entweder in einem Stil oder als lokale Werte für das Steuerelement selbst ändern.  |
| [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) | In einer Windows-Runtime 8. x-APP ist die Rechtschreibprüfung für ein [**Textfeld**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)standardmäßig deaktiviert. In einer Windows Phone Store-App und einer Windows 10-App ist sie standardmäßig aktiviert. |
| [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Die Standardschriftgröße für ein [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Element hat sich von 11 in 15 geändert. |
| [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Der Standardwert von [**TextBox.TextReadingOrder**](/uwp/api/windows.ui.xaml.controls.textblock.textreadingorder) wurde von **Default** in **DetectFromContent** geändert. Falls Sie dies nicht wünschen, können Sie **UseFlowDirection** verwenden. **Default** ist veraltet. |
| Verschiedene | Die Akzentfarbe gilt für eine Windows Phone Store-Apps und für Windows 10-apps, aber nicht für die Windows-Runtime von 8. x-apps.  |

Weitere Informationen zu Steuerelementen für UWP-Apps finden Sie unter [Steuerelemente nach Funktion](../design/controls-and-patterns/controls-by-function.md), [Liste der Steuerelemente](../design/controls-and-patterns/index.md) und [Richtlinien für Steuerelemente](../design/controls-and-patterns/index.md).

##  <a name="design-language-in-windows10"></a>Entwurfssprache in Windows 10

Zwischen universellen 8.1-Apps und Windows 10-Apps gibt es einige kleine, aber wichtige Unterschiede hinsichtlich der Entwurfssprache. Alle Details finden Sie unter [Design](https://developer.microsoft.com/windows/apps/design). Trotz der Änderungen bei der Entwurfssprache gelten nach wie vor dieselben Designprinzipien: Gestalten Sie Ihre App mit Liebe zum Detail, versuchen Sie aber, alles möglichst einfach zu halten, indem Sie sich auf den Inhalt, nicht auf das Chrom konzentrieren, visuelle Elemente weitgehend reduzieren und für die digitale Welt authentisch bleiben. Nutzen Sie insbesondere bei der Typografie eine visuelle Hierarchie. Entwerfen Sie Ihre App basierend auf einem Raster, und erwecken Sie Ihre Benutzeroberflächen mit flüssigen Animationen zum Leben.

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>Effektive Pixel, Abstand zum Bildschirm und Skalierungsfaktoren

Bisher wurden Anzeigepixel zum Abstrahieren der Größe und des Layouts von UI-Elementen von der tatsächlichen physischen Größe und Auflösung von Geräten verwendet. Anzeigepixel wurden zu effektiven Pixeln weiterentwickelt, und hier finden Sie eine Erklärung dieses Begriffs sowie Informationen zum zusätzlichen Wert.

Der Begriff „Auflösung“ bezeichnet ein Maß für die Pixeldichte und nicht wie allgemein angenommen für die Pixelanzahl. Die „effektive Auflösung“ ist die Art und Weise, wie die physischen Pixel, aus denen sich ein Bild oder eine Glyphe zusammensetzt, je nach Abstand zum Bildschirm und physischer Pixelgröße des Geräts für das Auge des Betrachters aufgelöst werden (die Pixeldichte ist der Kehrwert der physischen Pixelgröße). Die effektive Auflösung ist benutzerorientiert und somit eine gute Metrik für die Erstellung einer Benutzeroberfläche. Wenn Sie diese Faktoren verstehen und die Größe von UI-Elementen entsprechend steuern, können Sie eine optimale Benutzerfreundlichkeit erreichen.

Unterschiedliche Geräte besitzen eine unterschiedliche effektive Breite (angegeben in der Anzahl von Pixeln) – von 320 Epx bei besonders kleinen Geräten bis hin zu 1024 Epx bei Monitoren mittlerer Größe (und weit darüber hinaus für eine noch größere Breite). Sie müssen lediglich weiterhin Elemente mit automatischer Größenanpassung und dynamische Layoutbereiche verwenden. Im bestimmten Fällen werden die Eigenschaften der UI-Elemente im XAML-Markup auf eine feste Größe festgelegt. Auf Ihre App wird abhängig davon, auf welchem Gerät sie ausgeführt wird und welche Anzeigeeinstellungen der Benutzer festgelegt hat, automatisch ein Skalierungsfaktor angewendet. Dieser Skalierungsfaktor bewirkt, dass UI-Elemente mit fester Größe trotz unterschiedlicher Bildschirmgröße als Touchziel (und Leseziel) mit mehr oder weniger konstanter Größe angezeigt werden. In Kombination mit dem dynamischen Layout wird Ihre Benutzeroberfläche nicht nur auf verschiedenen Geräten optisch skaliert, auch die Inhaltsmenge wird an den verfügbaren Platz angepasst.

Damit Ihre App auf allen Displays optimal funktioniert, empfiehlt es sich, die einzelnen Bitmap-Ressourcen in verschiedenen Größen zu erstellen, die jeweils für einen bestimmten Skalierungsfaktor geeignet sind. Durch die Bereitstellung von Ressourcen mit einer Skalierung von 100 %, 200 % und 400 % (in dieser Prioritätsreihenfolge) erhalten Sie in den meisten Fällen auch bei allen Skalierungsfaktoren dazwischen hervorragende Ergebnisse.

**Hinweis**    Wenn Sie aus irgendeinem GRUNDRESSOURCEN nicht in mehr als einer Größe erstellen können, erstellen Sie 100% große Ressourcen. In Microsoft Visual Studio bietet die Standardprojektvorlage für UWP-Apps Brandingressourcen (Bilder für Kacheln und Logos) in nur einer Größe, jedoch nicht mit einer Skalierung von 100 %. Befolgen Sie bei der Erstellung von Ressourcen für Ihre eigene App die Informationen in diesem Abschnitt, stellen Sie die Ressourcen mit einer Skalierung von 100 %, 200 % und 400 % bereit, und verwenden Sie Ressourcenpakete.

Falls Sie komplexe Grafiken verwenden, sollten Sie mehr Skalierungen bereitstellen. Wenn Sie mit Vektorgrafiken beginnen, ist es relativ einfach, qualitativ hochwertige Ressourcen für beliebige Skalierungsfaktoren zu generieren.

Obwohl wir davon abraten, alle Skalierungsfaktoren zu unterstützen, möchten wir Ihnen die vollständige Liste der Skalierungsfaktoren für Windows 10-Apps nicht vorenthalten: 100 %, 125 %, 150 %, 200 %, 250 %, 300 % und 400 %. Wenn Sie sie bereitstellen, wählt der Store für jedes Gerät die Ressourcen mit der passenden Größe aus, und es werden nur diese Ressourcen heruntergeladen. Die Auswahl der herunterzuladenden Ressourcen erfolgt auf Grundlage des DPI-Werts eines Geräts. Sie können Ressourcen aus Ihrer Windows-Runtime 8. x-APP mit Skalierungsfaktoren wie 140% und 220% wieder verwenden, aber Ihre APP wird mit einem der neuen Skalierungsfaktoren ausgeführt, sodass einige bitmapskalierungen unvermeidlich sind. Testen Sie Ihre App auf verschiedenen Geräten, um jeweils zu überprüfen, ob ein zufriedenstellendes Ergebnis erzielt wird.

Möglicherweise verwenden Sie XAML-Markup aus einer Windows-Runtime 8. x-APP, bei der literaldimensionswerte im Markup verwendet werden (vielleicht zum Anpassen von Formen oder anderen Elementen, vielleicht für typografiezeichen). In einigen Fällen wird für eine Windows 10-App ein größerer Skalierungsfaktor als für eine universelle 8.1-App verwendet (z. B. werden 150 % anstatt der vorherigen Skalierung von 140 % und 200 % anstatt 180 % verwendet). Wenn Sie feststellen, dass diese Literalwerte unter Windows 10 jetzt zu groß sind, können Sie sie mit dem Wert 0,8 multiplizieren. Weitere Informationen finden Sie unter [reaktionsfähiges Design 101 für UWP-apps](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="gridview-and-listview-changes"></a>GridView-und ListView-Änderungen

An den Standardstilsettern für [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) wurden verschiedene Änderungen vorgenommen, damit der Bildlauf des Steuerelements vertikal verläuft (anstatt wie bisher standardmäßig horizontal). Wenn Sie in Ihrem Projekt eine Kopie des Standardstils bearbeitet haben, enthält Ihre Kopie diese Änderungen nicht. Sie müssen sie daher manuell vornehmen. Nachfolgend finden Sie eine Liste der Änderungen:

-   Der Setter für [**ScrollViewer. HorizontalScrollBarVisibility**](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) wurde von " **automatisch** " in " **deaktiviert**" geändert.
-   Der Setter für [**ScrollViewer.VerticalScrollBarVisibility**](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) wurde von **Disabled** in **Auto** geändert.
-   Der Setter für [**ScrollViewer. horizontalscrollmode**](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) wurde von " **aktiviert** " in " **deaktiviert**" geändert.
-   Der Setter für [**ScrollViewer. verticalscrollmode**](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) wurde von " **deaktiviert** " in " **aktiviert**" geändert.
-   Im Setter für [**ItemsPanel**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)hat sich der Wert von [**itemtaurapgrid. Orientation**](/uwp/api/windows.ui.xaml.controls.itemswrapgrid.orientation) von **vertikal** in **Horizontal**geändert.

Wenn Ihnen die letzte Änderung (die Änderung an **Orientation**) widersprüchlich erscheint, beachten Sie, dass wir von einem Umbruchraster sprechen. Ein horizontal ausgerichtetes Umbruchraster (der neue Wert) ähnelt einem Schriftsystem, bei dem Text horizontal fließt und am Ende einer Seite ein Zeilenumbruch erfolgt. Bei einer solchen Seite mit Text erfolgt der Bildlauf in vertikaler Richtung. Im Gegensatz dazu ähnelt ein vertikal ausgerichtetes Umbruchraster (der vorherige Wert) einem Schriftsystem, in dem Text vertikal fließt und daher ein horizontaler Bildlauf erfolgt.

Folgende Aspekte von [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) und [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) wurden geändert oder werden unter Windows 10 nicht unterstützt.

-   Die [**isswipeer-aktivierte**](/uwp/api/windows.ui.xaml.controls.listviewbase.isswipeenabled) Eigenschaft (nur Windows-Runtime 8. x-Apps) wird für Windows 10-apps nicht unterstützt. Die API ist noch vorhanden, ihre Verwendung hat aber keine Auswirkung. Mit Ausnahme von der Wischbewegung nach unten (wird nicht unterstützt, da sie den Daten zufolge nicht gefunden wird) und dem Rechtsklick (ist für die Anzeige eines Kontextmenüs reserviert) werden alle bisherigen Auswahlbewegungen unterstützt.
-   Die [**ReorderMode**](/uwp/api/windows.ui.xaml.controls.listviewbase.reordermode)-Eigenschaft (nur Windows Phone Store-Apps) wird für Windows 10-Apps nicht unterstützt. Die API ist noch vorhanden, ihre Verwendung hat aber keine Auswirkung. Legen Sie stattdessen [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) und [**CanReorderItems**](/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) für Ihre **GridView** oder **ListView** auf „true” fest. Der Benutzer kann die Anordnung dann durch Drücken und Halten (oder Klicken und Ziehen) ändern.
-   Verwenden Sie bei der Entwicklung für Windows 10 sowohl für [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) als auch für [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) in Ihrem Elementcontainerstil [**ListViewItemPresenter**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter) anstelle von [**GridViewItemPresenter**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemPresenter). Wenn Sie eine Kopie der Standard-Elementcontainerstile bearbeiten, erhalten Sie den richtigen Typ.
-   Die visuellen Auswahlelemente für eine Windows 10-App haben sich geändert. Wenn Sie [**SelectionMode**](/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) auf **Multiple** festlegen, wird standardmäßig für jedes Element ein Kontrollkästchen gerendert. Bei der Standardeinstellung für **ListView**-Elemente wird das Kontrollkästchen inline neben dem Element angeordnet, wodurch der vom restlichen Element belegte Platz etwas reduziert und verschoben wird. Bei **GridView**-Elementen überlagert das Kontrollkästchen standardmäßig das Element. In beiden Fällen können Sie jedoch wie im folgenden Beispiel gezeigt in Ihrem Elementcontainerstil das Layout (Inline oder Überlagerung) der Kontrollkästchen (mit der [**CheckMode**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode)-Eigenschaft) steuern und (mit der [**SelectionCheckMarkVisualEnabled**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled)-Eigenschaft) festlegen, ob sie überhaupt für das [**ListViewItemPresenter**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter)-Element angezeigt werden.
-   In Windows 10 wird das [**ContainerContentChanging**](/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging)-Ereignis während der UI-Virtualisierung zweimal pro Element ausgelöst: einmal für die Freigabe und einmal für die erneute Verwendung. Wenn der Wert von [**inrecyclequeue**](/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.inrecyclequeue) den Wert **true** hat und Sie keine besondere Freigabe arbeiten müssen, können Sie den Ereignishandler sofort mit der Gewissheit beenden, dass er erneut eingegeben wird, wenn das gleiche Element wieder verwendet wird (zu diesem Zeitpunkt ist **inrecyclequeue** **false**).

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

-   Da die Wischbewegung nach unten und der Rechtsklick als Auswahlbewegungen (aus den oben genannten Gründen) entfernt wurden, hat sich das Interaktionsmodell geändert. Unter anderem schließen sich das [**ItemClick**](/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick)-Ereignis und das [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Ereignis nicht mehr gegenseitig aus. Überprüfen Sie Ihre Szenarien für die Windows 10-App, und entscheiden Sie, ob Sie das Interaktionsmodell „Auswahl” oder „Aufrufen” verwenden möchten. Weitere Informationen finden [Sie unter Ändern des Interaktionsmodus](/previous-versions/windows/apps/hh780625(v=win.10)).
-   Die zum Formatieren des [**ListViewItemPresenter**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter)-Elements verwendeten Eigenschaften wurden teilweise geändert. Folgende Eigenschaften sind neu: [**CheckBoxBrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkboxbrush), [**PressedBackground**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pressedbackground), [**SelectedPressedBackground**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpressedbackground) und [**FocusSecondaryBorderBrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.focussecondaryborderbrush). Eigenschaften, die für eine Windows 10-App ignoriert [**werden,**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.padding) sind Auffüll Vorgänge (verwenden Sie stattdessen [**contentmargin**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.contentmargin) ), [**checkhintbrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkhintbrush), [**checkselectingbrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkselectingbrush), [**pointeroverbackgroundmargin**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pointeroverbackgroundmargin), [**reorderhinto ffset**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.reorderhintoffset), [**selectedborderdicke**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedborderthickness)und [**selectedpointeroverborderbrush**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpointeroverborderbrush).

In der folgenden Tabelle werden die Änderungen an den visuellen Zuständen und Gruppen visueller Zustände in der [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem)-Steuerelementvorlage und der [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem)-Steuerelementvorlage beschrieben.

| 8.1                 | Funktionsstatus           | Windows 10        | Funktionsstatus       |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | Normal                  |                   | Normal              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Gedrückt                 |                   | Gedrückt             |
|                     | PointerOverPressed      |                   | [nicht verfügbar]       |
|                     | Disabled                |                   | [nicht verfügbar]       |
|                     | [nicht verfügbar]           |                   | PointerOverSelected |
|                     | [nicht verfügbar]           |                   | Ausgewählt            |
|                     | [nicht verfügbar]           |                   | PressedSelected     |
| [nicht verfügbar]       |                         | DisabledStates    |                     |
|                     | [nicht verfügbar]           |                   | Disabled            |
|                     | [nicht verfügbar]           |                   | Aktiviert             |
| SelectionHintStates |                         | [nicht verfügbar]     |                     |
|                     | VerticalSelectionHint   |                   | [nicht verfügbar]       |
|                     | HorizontalSelectionHint |                   | [nicht verfügbar]       |
|                     | NoSelectionHint         |                   | [nicht verfügbar]       |
| [nicht verfügbar]       |                         | MultiSelectStates |                     |
|                     | [nicht verfügbar]           |                   | MultiSelectDisabled |
|                     | [nicht verfügbar]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [nicht verfügbar]     |                     |
|                     | Unselecting             |                   | [nicht verfügbar]       |
|                     | Nicht markiert              |                   | [nicht verfügbar]       |
|                     | UnselectedPointerOver   |                   | [nicht verfügbar]       |
|                     | UnselectedSwiping       |                   | [nicht verfügbar]       |
|                     | Wählen Sie Folgendes aus:               |                   | [nicht verfügbar]       |
|                     | Ausgewählt                |                   | [nicht verfügbar]       |
|                     | SelectedSwiping         |                   | [nicht verfügbar]       |
|                     | SelectedUnfocused       |                   | [nicht verfügbar]       |

Wenn Sie über eine benutzerdefinierte [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem)-Steuerelementvorlage oder [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem)-Steuerelementvorlage verfügen, überprüfen Sie sie im Hinblick auf die oben genannten Änderungen. Wir empfehlen, eine Kopie der neuen Standardvorlage zu bearbeiten und Ihre Anpassungen erneut auf diese Kopie anzuwenden. Wenn dies aus irgendeinem Grund nicht möglich ist und Sie Ihre vorhandene Vorlage bearbeiten müssen, können Sie nach den folgenden allgemeinen Richtlinien vorgehen.

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

Die Dateien vom Typ „Resources.resw“ aus Ihrem universellen 8.1-Projekt können in Ihrem UWP-App-Projekt wiederverwendet werden. Fügen Sie die kopierte Datei dem Projekt hinzu, und legen Sie **Buildvorgang** auf **PRIResource** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** fest. Im Thema [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) wird beschrieben, wie Sie gerätefamilienspezifische Ressourcen auf der Grundlage des Ressourcenauswahlfaktors für die Gerätefamilie laden.

## <a name="play-to"></a>Wiedergeben auf

Die APIs im [**Windows.Media.PlayTo**](/uwp/api/Windows.Media.PlayTo)-Namespace gelten für Windows 10-Apps als veraltet, weil jetzt die [**Windows.Media.Casting**](/uwp/api/Windows.Media.Casting)-APIs verwendet werden.

## <a name="resource-keys-and-textblock-style-sizes"></a>Ressourcenschlüssel und Größen von TextBlock-Stilen

Die Entwurfssprache für Windows 10 wurde weiterentwickelt, und daher haben sich bestimmte Systemstile geändert. In einigen Fällen empfiehlt sich die Überarbeitung des visuellen Designs Ihrer Ansichten, damit sie mit den geänderten Stileigenschaften in Einklang stehen.

In anderen Fällen werden die Ressourcenschlüssel nicht mehr unterstützt. Der XAML-Markup-Editor in Visual Studio hebt Verweise auf Ressourcenschlüssel hervor, die nicht aufgelöst werden können. Der XAML-Markup-Editor unterstreicht z. B. einen Verweis auf den Stilschlüssel `ListViewItemTextBlockStyle` mit einer roten Wellenlinie. Wird dieser Fehler nicht behoben, wird die App sofort beendet, wenn Sie versuchen, sie im Emulator oder auf dem Gerät bereitzustellen. Daher ist es wichtig, die Richtigkeit des XAML-Markups sicherzustellen. Sie werden feststellen, dass sich solche Fehler mit Visual Studio hervorragend abfangen lassen.

Für weiterhin unterstützte Schlüssel bedeuten Änderungen der Entwurfssprache, dass sich die Eigenschaften geändert haben, die von einigen Stilen festgelegt werden. Beispielsweise `TitleTextBlockStyle` legt **FontSize** in einer Windows-Runtime 8. x-APP und 18.14 px in einer Windows Phone Store-App auf 14.667 px fest. In einer Windows 10-App legt der gleiche Stil **FontSize** aber auf den deutlich höheren Wert 24 px fest. Überprüfen Sie Ihre Entwürfe und Layouts, und verwenden Sie die passenden Formate an den richtigen Stellen. Weitere Informationen finden Sie unter [Richtlinien für Schriftarten](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) und [Entwerfen von UWP-apps](https://developer.microsoft.com/windows/apps/design).

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
-   RechterRand
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

Obwohl [**SearchBox**](/uwp/api/windows.ui.xaml.controls.searchbox) in der universellen Gerätefamilie implementiert ist, ist es auf mobilen Geräten nicht voll funktionsfähig. Verwenden Sie [**AutoSuggestBox**](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) für Ihre universelle Sucherfahrung. Nachfolgend finden Sie die gängige Vorgehensweise zur Implementierung einer Sucherfahrung mit **AutoSuggestBox**.

Sobald der Benutzer mit der Eingabe beginnt, wird das **TextChanged**-Ereignis mit der Ursache **UserInput** ausgelöst. Füllen Sie anschließend die Liste der Vorschläge auf, und legen Sie die **ItemsSource** von [**AutoSuggestBox**](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) fest. Während der Benutzer durch die Liste navigiert, wird das **SuggestionChosen**-Ereignis ausgelöst (und wenn Sie **TextMemberDisplayPath** festgelegt haben), wird das Textfeld automatisch mit der angegebenen Eigenschaft aufgefüllt. Wenn der Benutzer eine Auswahl mit der EINGABETASTE übermittelt, wird das **QuerySubmitted**-Ereignis ausgelöst. An dieser Stelle können Sie auf diesen Vorschlag reagieren (in diesem Fall wahrscheinlich das Navigieren zu einer anderen Seite mit weiteren Details zu den angegebenen Inhalten). Beachten Sie, dass die **LinguisticDetails**-Eigenschaft und die **Language**-Eigenschaft von **SearchBoxQuerySubmittedEventArgs** nicht mehr unterstützt werden (es gibt entsprechende APIs, um diese Funktion zu unterstützen). Und **KeyModifiers** wird nicht mehr unterstützt.

[**AutoSuggestBox**](/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) bietet außerdem Unterstützung für Eingabemethoden-Editoren (IMEs). Und wenn Sie ein „Suchen“-Symbol anzeigen möchten, ist das ebenfalls möglich (durch die Interaktion mit dem Symbol wird das **QuerySubmitted**-Ereignis ausgelöst).

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

Siehe auch [Beispiel für AutoSuggestBox-Portierung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox).

## <a name="semanticzoom-changes"></a>SemanticZoom-Änderungen

Die Bewegung zum Verkleinern für [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) wurde im Windows Phone-Modell zusammengeführt. Hierzu wird auf einen Gruppenkopf getippt oder geklickt (auf Desktopcomputern wird also das Angebot der Schaltfläche „Minus“ zum Verkleinern nicht mehr angezeigt). Jetzt erhalten wir das gleiche, einheitliche Verhalten kostenlos auf allen Geräten. Es besteht der kosmetische Unterschied zum Windows Phone-Modell, dass die verkleinerte Ansicht (Sprungliste) die vergrößerte Ansicht ersetzt, anstatt sie zu überlagern. Aus diesem Grund können Sie alle halbtransparenten Hintergründe aus verkleinerten Ansichten entfernen.

In einer Windows Phone Store-App wird die verkleinerte Ansicht auf die Größe des Bildschirms erweitert. In einer Windows-Runtime 8. x-APP und in einer Windows 10-APP wird die Größe der Zoomansicht auf die Begrenzungen des **semanticzoom** -Steuer Elements beschränkt.

In einer Windows Phone Store-App scheinen Inhalte hinter der verkleinerten Ansicht (in Z-Reihenfolge) durch, wenn für den Hintergrund der verkleinerten Ansicht eine Transparenz festgelegt wurde. In einer Windows-Runtime 8. x-APP und in einer Windows 10-APP ist keine Anzeige hinter der Zoomansicht sichtbar.

Wenn die app in einer Windows-Runtime 8. x-APP deaktiviert und erneut aktiviert wird, wird die Zoomansicht verworfen (wenn Sie angezeigt wird), und stattdessen wird die Zoomansicht angezeigt. In einer Windows Phone Store-App und einer Windows 10-App wird die verkleinerte Ansicht weiter angezeigt, sofern sie aktiviert wurde.

In einer Windows Phone Store-App und einer Windows 10-App wird die verkleinerte Ansicht geschlossen, wenn die Schaltfläche „Zurück“ betätigt wird. Bei einer Windows-Runtime 8. x-APP gibt es keine integrierte Schaltflächen-Schaltfläche, daher trifft die Frage nicht zu.

## <a name="settings"></a>Einstellungen

Die **SettingsPane**-Klasse von Windows-Runtime 8.x ist nicht für Windows 10 geeignet. Zusätzlich zur Erstellung einer Einstellungsseite sollten Sie Benutzern stattdessen die Möglichkeit geben, von der App aus auf die Seite zuzugreifen. Es wird empfohlen, diese Seite mit App-Einstellungen auf der obersten Ebene als letztes angeheftetes Element im Navigationsbereich verfügbar zu machen. Hier sehen Sie jedoch den vollständigen Satz von Optionen.

-   Navigationsbereich. Einstellungen sollten das letzte Element in der Navigationsliste und am Ende der Liste angeheftet sein.
-   App-Leiste/Symbolleiste (innerhalb einer Registerkartenansicht oder eines Pivot-Layouts). Einstellungen sollten das letzte Element im Flyout „Menü“ der App-Leiste oder Symbolleiste sein. Es wird davon abgeraten, Einstellungen als eines der Elemente der obersten Ebene in der Navigation zu verwenden.
-   Hub. Einstellungen sollte innerhalb des Flyouts „Menü“ platziert werden (über das Menü der App-Leiste oder Symbolleiste im Hublayout).

Es wird davon abgeraten, Einstellungen in einem Master-/Detailbereich zu verstecken.

Die Einstellungsseite sollte das gesamte App-Fenster ausfüllen, und außerdem sollten „Info“ und „Feedback“ auf der Einstellungsseite zu finden sein. Anleitungen zum Entwerfen der Seite "Einstellungen" finden Sie unter [Richtlinien für App-Einstellungen](../design/app-settings/guidelines-for-app-settings.md).

## <a name="text"></a>Text

Der Text (bzw. die Typografie) ist ein wichtiger Aspekt einer UWP-App. Beim Portieren ist es ratsam, das grafische Design Ihrer Ansichten noch einmal darauf zu prüfen, ob es zur neuen Entwurfssprache passt. Verwenden Sie die folgenden Abbildungen, um nach den verfügbaren **TextBlock**-Systemstilen der universellen Windows-Plattform (UWP) zu suchen. Suchen Sie nach den Stilen, die zu den von Ihnen verwendeten Windows Phone Silverlight-Stilen passen. Alternativ können Sie eigene universelle Stile erstellen und die Eigenschaften aus den Windows Phone Silverlight-Systemstilen in diese Stile kopieren.

![system textblock styles foder windows 10 apps](images/label-uwp10stylegallery.png) <br/>TextBlock-Systemstile für Windows 10-Apps

In Windows-Runtime 8. x-apps und Windows Phone Store-Apps ist die Standardschrift Familie Global User Interface. In einer Windows 10-App wird standardmäßig die Schriftfamilie Segoe UI verwendet. Daher kann die Schriftartmetrik in Ihrer App Unterschiede aufweisen. Wenn Sie die Darstellung Ihres 8.1-Texts reproduzieren möchten, können Sie Ihre eigene Metrik mit Eigenschaften wie [**LineHeight**](/uwp/api/windows.ui.xaml.controls.textblock.lineheight) und [**LineStackingStrategy**](/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy) festlegen.

In Windows-Runtime 8. x-apps und Windows Phone Store-Apps ist die Standardsprache für Text auf die Sprache des Builds oder auf "en-US" festgelegt. In einer Windows 10-App ist die Standardsprache auf die oberste App-Sprache (Fallback für Schriftarten) festgelegt. Sie können [**FrameworkElement.Language**](/uwp/api/windows.ui.xaml.frameworkelement.language) explizit festlegen, aber Sie erhalten ein besseres Fallbackverhalten für Schriftarten, wenn Sie für diese Eigenschaft keinen Wert festlegen.

Weitere Informationen finden Sie unter [Richtlinien für Schriftarten](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) und [Entwerfen von UWP-apps](https://developer.microsoft.com/). Informationen zu Änderungen von Textsteuerelementen finden Sie oben im Abschnitt [Steuerelemente](#controls-and-control-styles-and-templates).

## <a name="theme-changes"></a>Designänderungen

Bei einer universellen 8.1-App ist das Standarddesign standardmäßig dunkel. Für Windows 10-Geräte hat sich das Standarddesign geändert. Sie können das Design aber ändern, indem Sie in „App.Xaml“ ein angefordertes Design deklarieren. Wenn Sie z. B. auf allen Geräten ein dunkles Design verwenden möchten, fügen Sie dem Stammelement der App `RequestedTheme="Dark"` hinzu.

## <a name="tiles-and-toasts"></a>Kacheln und Popups

Für Kacheln und Popups in Ihren Windows 10-Apps können weiterhin die derzeit verwendeten Vorlagen genutzt werden. Es sind jedoch neue adaptive Vorlagen verfügbar. Diese werden unter [Benachrichtigungen, Kacheln, Popups und Signale](../design/shell/tiles-and-notifications/index.md) beschrieben.

Auf Desktopcomputern war eine Popupbenachrichtigung früher eine vorübergehende Nachricht. Sie wurde ausgeblendet und konnte nicht mehr aufgerufen werden, nachdem sie geschlossen oder ignoriert wurde. Wenn unter Windows Phone eine Popupbenachrichtigung ignoriert oder vorerst geschlossen wird, wird sie ins Info-Center verschoben. Das Info-Center ist nun nicht mehr auf die Mobilgerätefamilie beschränkt.

Zum Senden einer Popupbenachrichtigung muss keine Funktion mehr deklariert werden.

## <a name="window-size"></a>Fenstergröße

Bei einer universellen 8.1-App wird das App-Manifestelement [**ApplicationView**](/uwp/schemas/appxpackage/appxmanifestschema2013/element-applicationview) verwendet, um eine minimale Fensterbreite zu deklarieren. In Ihrer UWP-App können Sie eine Mindestgröße (Breite und Höhe) mit imperativem Code angeben. Die Standardmindestgröße beträgt 500 x 320 Epx. Dies ist auch die kleinste zulässige Mindestgröße. Die größte zulässige Mindestgröße ist 500 x 500 Epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

Das nächste Thema ist das [Portieren von e/a-, Geräte-und App-Modellen](w8x-to-uwp-input-and-sensors.md).