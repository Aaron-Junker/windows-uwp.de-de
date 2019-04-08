---
description: Die Vorgehensweise zum Definieren einer Benutzeroberfläche in Form von deklarativem XAML-Markup lässt sich sehr gut von universellen 8.1-Apps auf Apps für die universelle Windows-Plattform (UWP) übertragen.
title: Portieren von Windows-Runtime 8.x-XAML und -UI zu UWP
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7b8bb652c3d8b978d631da2e529662a455310458
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616345"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Portieren von Windows-Runtime 8.x-XAML und -UI zu UWP


Im vorherigen Thema haben wir uns mit der [Problembehandlung](w8x-to-uwp-troubleshooting.md) beschäftigt.

Die Vorgehensweise zum Definieren einer Benutzeroberfläche in Form von deklarativem XAML-Markup lässt sich sehr gut von universellen 8.1-Apps auf Apps für die universelle Windows-Plattform (UWP) übertragen. Sie werden feststellen, dass der Großteil des Markups kompatibel ist. Unter Umständen sind jedoch einige Anpassungen bei Systemressourcenschlüsseln oder benutzerdefinierten Vorlagen erforderlich. Beim imperativen Code in Ihren Ansichtsmodellen sind nur geringfügige oder gar keine Änderungen erforderlich. Und sogar ein Großteil des Codes in Ihrer Darstellungsschicht zur Manipulierung von UI-Elementen dürfte sich problemlos portieren lassen.

## <a name="imperative-code"></a>Imperativer Code

Falls Sie lediglich die Projekterstellungsphase erreichen möchten, können Sie sämtlichen nicht unbedingt erforderlichen Code auskommentieren. Klicken Sie dann den Vorgang wiederholen, ein Problem zu einem Zeitpunkt, und finden Sie in den folgenden Themen in diesem Abschnitt (und im vorherigen Thema: [Problembehandlung bei](w8x-to-uwp-troubleshooting.md)), bis die Build- und Probleme sind Aufbügeln Skalierung und der Port ist abgeschlossen.

## <a name="adaptiveresponsive-ui"></a>Adaptive/reaktionsfähige Benutzeroberfläche

Da Ihre App potenziell auf vielen verschiedenen Geräten ausgeführt werden kann – jeweils mit eigener Bildschirmgröße und -auflösung – ist es ratsam, nicht nur die mindestens erforderlichen Schritte zum Portieren Ihrer App auszuführen und die UI so zu gestalten, dass sie auf allen Geräten möglichst gut aussieht. Sie können das adaptive Visual State-Manager-Feature nutzen, um die Fenstergröße dynamisch zu ermitteln und als Reaktion darauf das Layout zu ändern. Ein Beispiel zur Vorgehensweise finden Sie im Abschnitt [Adaptive UI](w8x-to-uwp-case-study-bookstore2.md) im Thema mit der Bookstore2-Fallstudie.

## <a name="back-button-handling"></a>Behandeln der Schaltfläche „Zurück“

Für Universal 8.1 verfügen apps, Windows-Runtime 8.x-apps und Windows Phone Store-apps für verschiedene Ansätze für die Benutzeroberfläche, die Sie anzeigen und die Ereignisse, die Sie für die zurück-Schaltfläche zu behandeln. Aber Sie können einen einzelnen Ansatz in Ihrer app verwenden, für Windows 10-apps. Auf mobilen Geräten wird die Schaltfläche für Sie als kapazitive Schaltfläche auf dem Gerät oder als Schaltfläche in der Shell bereitgestellt. Auf einem Desktopgerät fügen Sie den Chromelementen der App eine Schaltfläche hinzu, wenn die Rückwärtsnavigation in der App möglich ist. Sie wird für Apps mit Fenstern in der Titelleiste und im Tabletmodus in der Taskleiste angezeigt. Das Ereignis der Schaltfläche „Zurück“ ist ein universelles Konzept, das für alle Gerätefamilien gilt. Für Schaltflächen, die in Hardware oder Software implementiert werden, wird das gleiche [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596)-Ereignis ausgelöst.

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

Sie müssen Ihren Code ändern, die Charms integriert, aber Sie müssen einige Elemente der Benutzeroberfläche der app erfolgen die Charms-Leiste, die nicht Teil der Windows 10-Shell ist hinzufügen. Eine Universal 8.1-app unter Windows 10 verfügt über eigene Ersetzung von Chrome System gerendert, in der Titelleiste der Anwendung bereitgestellte Benutzeroberfläche.

## <a name="controls-and-control-styles-and-templates"></a>Steuerelemente und Steuerelementstile/-vorlagen

Eine Universal 8.1-app unter Windows 10 behält das 8.1 aussehen und Verhalten in Bezug auf die Steuerelemente. Aber wenn Sie diese app in einer Windows 10-app portieren, es gibt einige Unterschiede in Darstellung und Verhalten zu beachten. Die Architektur und Entwurf von Steuerelementen ist im Grunde unverändert geblieben für Windows 10-apps, damit die Änderungen hauptsächlich rund um die Design-Sprache, Vereinfachung und benutzerfreundlichkeit Verbesserungen sind.

**Beachten Sie**    visuellen Zustand der PointerOver benutzerdefinierte Stile/Templates in Windows 10-apps und in Windows-Runtime 8.x-apps, aber nicht in Windows Phone Store-apps ist. Aus diesem Grund (und aufgrund der Systemressourcenschlüssel, die für Windows 10-apps unterstützt werden), empfehlen wir, dass Sie benutzerdefinierten Stile/Vorlagen erneut aus den Windows-Runtime 8.x-apps verwenden, wenn Sie Ihre app auf Windows 10 portieren.
Wenn Sie sicher sein, dass möchten benutzerdefinierten Stile/Vorlagen werden mithilfe des aktuellen Satzes von visuellen Zuständen sind profitieren von leistungsverbesserungen, die versucht, die Stile/Standardvorlagen, und klicken Sie dann eine Kopie der neuen Windows 10-Standardvorlage und bearbeiten erneut anwenden Ihrer die Anpassung mit. Ein Beispiel für eine Leistungsverbesserung besteht darin, dass alle **Border**-Elemente, die ein **ContentPresenter**- Element oder Panel-Element bisher eingeschlossen haben, entfernt wurden. Der Rahmen wird jetzt von einem untergeordneten Element gerendert.

Unten sind einige speziellere Beispiele für Änderungen an Steuerelementen angegeben.

| Name des Steuerelements | Änderung |
|--------------|--------|
| **App-Leiste**   | Bei Verwendung der **AppBar** Steuerelement ([**CommandBar** ](https://msdn.microsoft.com/library/windows/apps/hh701927) wird stattdessen empfohlen), wird es nicht in einer Windows 10-app standardmäßig ausgeblendet. Sie können dies mit der [**AppBar.ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/dn633872)-Eigenschaft steuern. |
| **AppBar**, [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | In einer Windows 10-app **AppBar** und [ **CommandBar** ](https://msdn.microsoft.com/library/windows/apps/hh701927) haben eine **weitere** Schaltfläche (die Auslassungspunkte). |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | In einer Windows-Runtime 8.x-app, die sekundäre Befehle von einem [ **CommandBar** ](https://msdn.microsoft.com/library/windows/apps/hh701927) sind immer sichtbar. In einer Windows Phone Store-app, und klicken Sie in einer Windows 10-app die nicht angezeigt, bis die Befehlsleiste geöffnet wird. |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Bei einer Windows Phone Store-App hat der Wert von [**CommandBar.IsSticky**](https://msdn.microsoft.com/library/windows/apps/hh701944) keinen Einfluss darauf, ob die Leiste einfach ausgeblendet werden kann. Für eine Windows 10-app Wenn **IsSticky** nastaven NA hodnotu true, klicken Sie dann die **CommandBar** eine light-Dismiss-Geste ignoriert. |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | In einer Windows 10-app [ **CommandBar** ](https://msdn.microsoft.com/library/windows/apps/hh701927) behandelt nicht das [ **EdgeGesture.Completed** ](https://msdn.microsoft.com/library/windows/apps/hh701622) noch [  **UIElement.RightTapped** ](https://msdn.microsoft.com/library/windows/apps/br208984) Ereignisse. Außerdem wird nicht auf ein Tippen oder das Wischen nach oben reagiert. Sie haben dennoch die Möglichkeit, diese Ereignisse zu behandeln und [**IsOpen**](https://msdn.microsoft.com/library/windows/apps/hh701939) festzulegen. |
| [**"DatePicker"**](https://msdn.microsoft.com/library/windows/apps/dn298584), [ **TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | Überprüfen Sie, wie Ihre App mit den visuellen Änderungen für [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584) und [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) aussieht. Für eine Windows 10-app auf einem mobilen Gerät ausgeführt wird diese Steuerelemente nicht mehr zu Auswahlseite navigieren, eine, aber verwenden stattdessen ein Light-ablehnbarer Popup. |
| [**"DatePicker"**](https://msdn.microsoft.com/library/windows/apps/dn298584), [ **TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | Sie können nicht in einer Windows 10-app einfügen [ **"DatePicker"** ](https://msdn.microsoft.com/library/windows/apps/dn298584) oder [ **TimePicker** ](https://msdn.microsoft.com/library/windows/apps/dn299280) in ein Flyout. Wenn Sie diese Steuerelemente in einem Steuerelement im Popupstil anzeigen möchten, können Sie [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) und [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313) verwenden. |
| **GridView**, **ListView** | Informationen zu **GridView**/**ListView** finden Sie unter [GridView-/ListView-Änderungen](#gridview-and-listview-changes). |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) | In einer Windows Phone Store-App wird ein [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843)-Steuerelement vom letzten Abschnitt in den ersten Abschnitt umgebrochen. In einer Windows-Runtime 8.x-app und in einer Windows 10-app umbrechen um nicht hubsection. |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) | In einer Windows Phone Store-App wird das Hintergrundbild eines [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843)-Steuerelements im Parallaxmodus relativ zu den Hubabschnitten verschoben. In einer Windows-Runtime 8.x-app und in einer Windows 10-app wird der Parallax nicht verwendet. |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843)  | In einer universellen 8.1-App bewirkt die [**HubSection.IsHeaderInteractive**](https://msdn.microsoft.com/library/windows/apps/dn251917)-Eigenschaft, dass die Abschnittsüberschrift – und eine daneben gerenderte Chevronglyphe – interaktiv werden. In einer Windows 10-app neben des Headers ist eine interaktive Unterstützung für die "Weitere Informationen", aber der Header selbst ist nicht interaktiv. Mit **IsHeaderInteractive** wird weiterhin bestimmt, ob die Interaktion das [**Hub.SectionHeaderClick**](https://msdn.microsoft.com/library/windows/apps/dn251953)-Ereignis auslöst. |
| **MessageDialog** | Wenn Sie **MessageDialog** verwenden, sollten Sie stattdessen ggf. das flexiblere [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/dn633972)-Element nutzen. Informationen hierzu erhalten Sie auch im Beispiel [XAML-UI-Grundlagen](https://go.microsoft.com/fwlink/p/?linkid=619992). |
| **ListPickerFlyout**, **PickerFlyout**  | **ListPickerFlyout** und **PickerFlyout** sind für eine app für Windows 10 als veraltet markiert. Verwenden Sie für ein einzelnes Auswahlflyout das [**MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)-Element und bei komplexeren Oberflächen das [**Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)-Element. |
| [**PasswordBox-Element**](https://msdn.microsoft.com/library/windows/apps/br227519) | Die [ **PasswordBox.IsPasswordRevealButtonEnabled** ](https://msdn.microsoft.com/library/windows/apps/hh702579) Eigenschaft ist in einer Windows 10-app veraltet, und diese Einstellung hat keine Auswirkungen. Verwendung [ **PasswordBox.PasswordRevealMode** ](https://msdn.microsoft.com/library/windows/apps/dn890867) stattdessen, die standardmäßig auf **einsehen** (in dem ein Symbol für die Augen, wie z. B. in einer Windows-Runtime 8.x-app angezeigt wird). Weitere Informationen finden Sie unter [Richtlinien für Kennwortfelder](https://msdn.microsoft.com/library/windows/apps/dn596103). |
| [**Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241) | Das [**Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241)-Steuerelement ist jetzt universell. Das heißt, die Verwendung ist nicht mehr nur auf mobile Geräte beschränkt. |
| [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252771) | Obwohl [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803) in der universellen Gerätefamilie implementiert ist, ist es auf mobilen Geräten nicht voll funktionsfähig. Weitere Informationen finden Sie unter [SearchBox eingestellt zugunsten von AutoSuggestBox](#searchbox-deprecated-in-favor-of-autosuggestbox). |
| **SemanticZoom** | Informationen zu **SemanticZoom** finden Sie unter [Änderungen an „SemanticZoom“](#semanticzoom-changes). |
| [**ScrollViewer-Element**](https://msdn.microsoft.com/library/windows/apps/br209527)  | Einige Standardeigenschaften von [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) wurden geändert. [**HorizontalScrollMode** ](https://msdn.microsoft.com/library/windows/apps/br209549) ist **automatisch**, [ **VerticalScrollMode** ](https://msdn.microsoft.com/library/windows/apps/br209589) ist **automatisch**, und [ **ZoomMode** ](https://msdn.microsoft.com/library/windows/apps/br209601) ist **deaktiviert**. Wenn die neuen Standardwerte für Ihre App nicht geeignet sind, können Sie sie entweder in einem Stil oder als lokale Werte für das Steuerelement selbst ändern.  |
| [**Textfeld**](https://msdn.microsoft.com/library/windows/apps/br209683) | In einer Windows-Runtime 8.x-app, Rechtschreibprüfung ist standardmäßig deaktiviert, für eine [ **Textfeld**](https://msdn.microsoft.com/library/windows/apps/br209683). In einer Windows Phone Store-app und in einer Windows 10-app ist es standardmäßig aktiviert. |
| [**Textfeld**](https://msdn.microsoft.com/library/windows/apps/br209683) | Die Standardschriftgröße für ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)-Element hat sich von 11 in 15 geändert. |
| [**Textfeld**](https://msdn.microsoft.com/library/windows/apps/br209683) | Der Standardwert von [**TextBox.TextReadingOrder**](https://msdn.microsoft.com/library/windows/apps/dn252859) wurde von **Default** in **DetectFromContent** geändert. Falls Sie dies nicht wünschen, können Sie **UseFlowDirection** verwenden. **Default** ist veraltet. |
| Verschiedene | Farbe für Akzente gilt für eine Windows Phone Store-apps und Windows 10-Apps, aber nicht für Windows-Runtime 8.x-apps.  |

Weitere Informationen zu Steuerelementen für UWP-Apps finden Sie unter [Steuerelemente nach Funktion](https://msdn.microsoft.com/library/windows/apps/mt185405), [Liste der Steuerelemente](https://msdn.microsoft.com/library/windows/apps/mt185406) und [Richtlinien für Steuerelemente](https://msdn.microsoft.com/library/windows/apps/dn611856).

##  <a name="design-language-in-windows10"></a>Design-Sprache in Windows 10

Es gibt einige kleine, jedoch wichtige Unterschiede in Design Language Universal 8.1-apps und Windows 10-apps. Alle Details finden Sie unter [Design](https://developer.microsoft.com/en-us/windows/apps/design). Trotz der Änderungen bei der Entwurfssprache gelten nach wie vor dieselben Designprinzipien: Gestalten Sie Ihre App mit Liebe zum Detail, versuchen Sie aber, alles möglichst einfach zu halten, indem Sie sich auf den Inhalt, nicht auf das Chrom konzentrieren, visuelle Elemente weitgehend reduzieren und für die digitale Welt authentisch bleiben. Nutzen Sie insbesondere bei der Typografie eine visuelle Hierarchie. Entwerfen Sie Ihre App basierend auf einem Raster, und erwecken Sie Ihre Benutzeroberflächen mit flüssigen Animationen zum Leben.

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>Effektive Pixel, Abstand zum Bildschirm und Skalierungsfaktoren

Bisher wurden Anzeigepixel zum Abstrahieren der Größe und des Layouts von UI-Elementen von der tatsächlichen physischen Größe und Auflösung von Geräten verwendet. Anzeigepixel wurden zu effektiven Pixeln weiterentwickelt, und hier finden Sie eine Erklärung dieses Begriffs sowie Informationen zum zusätzlichen Wert.

Der Begriff „Auflösung“ bezeichnet ein Maß für die Pixeldichte und nicht wie allgemein angenommen für die Pixelanzahl. Die „effektive Auflösung“ ist die Art und Weise, wie die physischen Pixel, aus denen sich ein Bild oder eine Glyphe zusammensetzt, je nach Abstand zum Bildschirm und physischer Pixelgröße des Geräts für das Auge des Betrachters aufgelöst werden (die Pixeldichte ist der Kehrwert der physischen Pixelgröße). Die effektive Auflösung ist benutzerorientiert und somit eine gute Metrik für die Erstellung einer Benutzeroberfläche. Wenn Sie diese Faktoren verstehen und die Größe von UI-Elementen entsprechend steuern, können Sie eine optimale Benutzerfreundlichkeit erreichen.

Unterschiedliche Geräte besitzen eine unterschiedliche effektive Breite (angegeben in der Anzahl von Pixeln) – von 320 Epx bei besonders kleinen Geräten bis hin zu 1024 Epx bei Monitoren mittlerer Größe (und weit darüber hinaus für eine noch größere Breite). Sie müssen lediglich weiterhin Elemente mit automatischer Größenanpassung und dynamische Layoutbereiche verwenden. Im bestimmten Fällen werden die Eigenschaften der UI-Elemente im XAML-Markup auf eine feste Größe festgelegt. Auf Ihre App wird abhängig davon, auf welchem Gerät sie ausgeführt wird und welche Anzeigeeinstellungen der Benutzer festgelegt hat, automatisch ein Skalierungsfaktor angewendet. Dieser Skalierungsfaktor bewirkt, dass UI-Elemente mit fester Größe trotz unterschiedlicher Bildschirmgröße als Touchziel (und Leseziel) mit mehr oder weniger konstanter Größe angezeigt werden. In Kombination mit dem dynamischen Layout wird Ihre Benutzeroberfläche nicht nur auf verschiedenen Geräten optisch skaliert, auch die Inhaltsmenge wird an den verfügbaren Platz angepasst.

Damit Ihre App auf allen Displays optimal funktioniert, empfiehlt es sich, die einzelnen Bitmap-Ressourcen in verschiedenen Größen zu erstellen, die jeweils für einen bestimmten Skalierungsfaktor geeignet sind. Durch die Bereitstellung von Ressourcen mit einer Skalierung von 100 %, 200 % und 400 % (in dieser Prioritätsreihenfolge) erhalten Sie in den meisten Fällen auch bei allen Skalierungsfaktoren dazwischen hervorragende Ergebnisse.

**Beachten Sie**  If, aus irgendeinem Grund, Sie können nicht erstellen Ressourcen in mehr als eine Größe, dann erstellen Sie 100 %-Skalierung-Ressourcen. In Microsoft Visual Studio bietet die Standardprojektvorlage für UWP-Apps Brandingressourcen (Bilder für Kacheln und Logos) in nur einer Größe, jedoch nicht mit einer Skalierung von 100 %. Befolgen Sie bei der Erstellung von Ressourcen für Ihre eigene App die Informationen in diesem Abschnitt, stellen Sie die Ressourcen mit einer Skalierung von 100 %, 200 % und 400 % bereit, und verwenden Sie Ressourcenpakete.

Falls Sie komplexe Grafiken verwenden, sollten Sie mehr Skalierungen bereitstellen. Wenn Sie mit Vektorgrafiken beginnen, ist es relativ einfach, qualitativ hochwertige Ressourcen für beliebige Skalierungsfaktoren zu generieren.

Es wird nicht empfohlen, dass Sie versuchen, alle Skalierungsfaktoren unterstützen, aber die vollständige Liste der Skalierungsfaktoren für Windows 10-apps 100 %, 125 %, 150 %, 200 %, 250 %, 300 % und 400 ist %. Wenn Sie sie bereitstellen, wählt der Store für jedes Gerät die Ressourcen mit der passenden Größe aus, und es werden nur diese Ressourcen heruntergeladen. Die Auswahl der herunterzuladenden Ressourcen erfolgt auf Grundlage des DPI-Werts eines Geräts. Erneut können Sie Objekte aus der Windows-Runtime 8.x-app im Skalierungsfaktoren wie z. B. 140 % und 220 %, aber die app auf die neue Skalierungsfaktoren ausgeführt, und einige Bitmapskalierung werden unvermeidlich. Testen Sie Ihre App auf verschiedenen Geräten, um jeweils zu überprüfen, ob ein zufriedenstellendes Ergebnis erzielt wird.

Sie können erneut verwenden XAML-Markup aus einer Windows-Runtime 8.x-app, in dem literal Dimensionswerte werden im Markup (vielleicht um Größe von Formen oder andere Elemente, z. B. für Typografie) verwendet. Aber in einigen Fällen ein größere Skalierungsfaktor auf einem Gerät für eine Windows 10-app als für Universal 8.1-Apps verwendet wird (z. B. 150 % dient, in dem zuvor von 140 % und 200 % wird verwendet, bei denen 180 % war). Also, wenn Sie feststellen, dass diese literale Werte jetzt unter Windows 10 zu groß sind, versuchen Sie diese mit 0,8 multipliziert. Weitere Informationen finden Sie unter [Reaktionsfähiges Design – Grundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958435).

## <a name="gridview-and-listview-changes"></a>GridView-/ListView-Änderungen

An den Standardstilsettern für [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) wurden verschiedene Änderungen vorgenommen, damit der Bildlauf des Steuerelements vertikal verläuft (anstatt wie bisher standardmäßig horizontal). Wenn Sie in Ihrem Projekt eine Kopie des Standardstils bearbeitet haben, enthält Ihre Kopie diese Änderungen nicht. Sie müssen sie daher manuell vornehmen. Nachfolgend finden Sie eine Liste der Änderungen:

-   Der Setter für [**ScrollViewer.HorizontalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209547) wurde von **Auto** in **Disabled** geändert.
-   Der Setter für [**ScrollViewer.VerticalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209587) wurde von **Disabled** in **Auto** geändert.
-   Der Setter für [**ScrollViewer.HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549) wurde von **Enabled** in **Disabled** geändert.
-   Der Setter für [**ScrollViewer.VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589) wurde von **Disabled** in **Enabled** geändert.
-   Im Setter für [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/br242826) wurde der Wert von [**ItemsWrapGrid.Orientation**](https://msdn.microsoft.com/library/windows/apps/dn298907) von **Vertical** in **Horizontal** geändert.

Wenn Ihnen die letzte Änderung (die Änderung an **Orientation**) widersprüchlich erscheint, beachten Sie, dass wir von einem Umbruchraster sprechen. Ein horizontal ausgerichtetes Umbruchraster (der neue Wert) ähnelt einem Schriftsystem, bei dem Text horizontal fließt und am Ende einer Seite ein Zeilenumbruch erfolgt. Bei einer solchen Seite mit Text erfolgt der Bildlauf in vertikaler Richtung. Im Gegensatz dazu ähnelt ein vertikal ausgerichtetes Umbruchraster (der vorherige Wert) einem Schriftsystem, in dem Text vertikal fließt und daher ein horizontaler Bildlauf erfolgt.

Hier sind die Aspekte der [ **GridView** ](https://msdn.microsoft.com/library/windows/apps/br242705) und [ **ListView** ](https://msdn.microsoft.com/library/windows/apps/br242878) , geändert haben, oder werden in Windows 10 nicht unterstützt.

-   Die [ **IsSwipeEnabled** ](https://msdn.microsoft.com/library/windows/apps/hh702518) Eigenschaft (nur Windows-Runtime 8.x apps) wird für Windows 10-apps nicht unterstützt. Die API ist noch vorhanden, ihre Verwendung hat aber keine Auswirkung. Mit Ausnahme von der Wischbewegung nach unten (wird nicht unterstützt, da sie den Daten zufolge nicht gefunden wird) und dem Rechtsklick (ist für die Anzeige eines Kontextmenüs reserviert) werden alle bisherigen Auswahlbewegungen unterstützt.
-   Die [ **ReorderMode** ](https://msdn.microsoft.com/library/windows/apps/dn625099) Eigenschaft (nur Windows Phone Store-apps) wird für Windows 10-apps nicht unterstützt. Die API ist noch vorhanden, ihre Verwendung hat aber keine Auswirkung. Legen Sie stattdessen [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/br208912) und [**CanReorderItems**](https://msdn.microsoft.com/library/windows/apps/br242882) für Ihre **GridView** oder **ListView** auf „true” fest. Der Benutzer kann die Anordnung dann durch Drücken und Halten (oder Klicken und Ziehen) ändern.
-   Verwenden Sie beim Entwickeln für Windows 10 [ **ListViewItemPresenter** ](https://msdn.microsoft.com/library/windows/apps/dn298500) anstelle von [ **GridViewItemPresenter** ](https://msdn.microsoft.com/library/windows/apps/dn279298) in Ihrem Elementcontainer Stil für [ **ListView** ](https://msdn.microsoft.com/library/windows/apps/br242878) und [ **GridView**](https://msdn.microsoft.com/library/windows/apps/br242705). Wenn Sie eine Kopie der Standard-Elementcontainerstile bearbeiten, erhalten Sie den richtigen Typ.
-   Die visuellen Elemente der Auswahl für eine Windows 10-app geändert. Wenn Sie [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/br242915) auf **Multiple** festlegen, wird standardmäßig für jedes Element ein Kontrollkästchen gerendert. Bei der Standardeinstellung für **ListView**-Elemente wird das Kontrollkästchen inline neben dem Element angeordnet, wodurch der vom restlichen Element belegte Platz etwas reduziert und verschoben wird. Bei **GridView**-Elementen überlagert das Kontrollkästchen standardmäßig das Element. In beiden Fällen können Sie jedoch wie im folgenden Beispiel gezeigt in Ihrem Elementcontainerstil das Layout (Inline oder Überlagerung) der Kontrollkästchen (mit der [**CheckMode**](https://msdn.microsoft.com/library/windows/apps/dn913923)-Eigenschaft) steuern und (mit der [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298541)-Eigenschaft) festlegen, ob sie überhaupt für das [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx)-Element angezeigt werden.
-   In Windows 10 die [ **ContainerContentChanging** ](https://msdn.microsoft.com/library/windows/apps/dn298914) Ereignis wird zweimal pro Element ausgelöst, während der UI-Virtualisierung: einmal für die bei der Rückgewinnung und einmal für die erneute Verwendung. Wenn der Wert der [**InRecycleQueue**](https://msdn.microsoft.com/library/windows/apps/dn279443) gleich **true** ist und Sie keine spezielle Freigabe vornehmen müssen, können Sie Ihren Ereignishandler sofort mit der Gewissheit beenden, dass er erneut aktiviert wird, wenn das gleiche Element wiederverwendet wird (dabei lautet **InRecycleQueue** dann **false**).

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

-   Da die Wischbewegung nach unten und der Rechtsklick als Auswahlbewegungen (aus den oben genannten Gründen) entfernt wurden, hat sich das Interaktionsmodell geändert. Unter anderem schließen sich das [**ItemClick**](https://msdn.microsoft.com/library/windows/apps/br242904)-Ereignis und das [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776)-Ereignis nicht mehr gegenseitig aus. Überprüfen Sie Ihre Szenarien, und entscheiden Sie, ob "Auswahl" oder das Interaktionsmodell "aufrufen" verwenden, für Ihre Windows 10-app. Ausführliche Informationen finden Sie unter [So wird's gemacht: Ändern des Interaktionsmodus](https://msdn.microsoft.com/library/windows/apps/xaml/hh780625).
-   Die zum Formatieren des [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx)-Elements verwendeten Eigenschaften wurden teilweise geändert. Folgende Eigenschaften sind neu: [**CheckBoxBrush**](https://msdn.microsoft.com/library/windows/apps/dn913905), [**PressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913931), [**SelectedPressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913937) und [**FocusSecondaryBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn898370). Eigenschaften, die für Windows 10-Apps ignoriert werden, sind [ **Padding** ](https://msdn.microsoft.com/library/windows/apps/dn424775) (verwenden Sie [ **ContentMargin** ](https://msdn.microsoft.com/library/windows/apps/dn424773) stattdessen), [ **CheckHintBrush**](https://msdn.microsoft.com/library/windows/apps/dn298504), [ **CheckSelectingBrush**](https://msdn.microsoft.com/library/windows/apps/dn298506), [ **PointerOverBackgroundMargin** ](https://msdn.microsoft.com/library/windows/apps/dn424778), [ **ReorderHintOffset**](https://msdn.microsoft.com/library/windows/apps/dn298528), [ **SelectedBorderThickness**](https://msdn.microsoft.com/library/windows/apps/dn298533), und [  **SelectedPointerOverBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn298539).

In der folgenden Tabelle werden die Änderungen an den visuellen Zuständen und Gruppen visueller Zustände in der [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919)-Steuerelementvorlage und der [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501)-Steuerelementvorlage beschrieben.

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

Wenn Sie über eine benutzerdefinierte [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919)-Steuerelementvorlage oder [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501)-Steuerelementvorlage verfügen, überprüfen Sie sie im Hinblick auf die oben genannten Änderungen. Wir empfehlen, eine Kopie der neuen Standardvorlage zu bearbeiten und Ihre Anpassungen erneut auf diese Kopie anzuwenden. Wenn dies aus irgendeinem Grund nicht möglich ist und Sie Ihre vorhandene Vorlage bearbeiten müssen, können Sie nach den folgenden allgemeinen Richtlinien vorgehen.

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

Die Dateien vom Typ „Resources.resw“ aus Ihrem universellen 8.1-Projekt können in Ihrem UWP-App-Projekt wiederverwendet werden. Fügen Sie die kopierte Datei dem Projekt hinzu, und legen Sie **Buildvorgang** auf **PRIResource** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** fest. Im Thema [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) wird beschrieben, wie Sie gerätefamilienspezifische Ressourcen auf der Grundlage des Ressourcenauswahlfaktors für die Gerätefamilie laden.

## <a name="play-to"></a>Wiedergeben auf

Die APIs in der [ **Windows.Media.PlayTo** ](https://msdn.microsoft.com/library/windows/apps/br207025) Namespace sind veraltet, für Windows 10-apps gegenüber der der [ **Windows.Media.Casting** ](https://msdn.microsoft.com/library/windows/apps/dn972568) APIs.

## <a name="resource-keys-and-textblock-style-sizes"></a>Ressourcenschlüssel und Größen von TextBlock-Stilen

Die Design-Sprache wurde entwickelt, für Windows 10 und sich daher bestimmte Systemstile geändert haben. In einigen Fällen empfiehlt sich die Überarbeitung des visuellen Designs Ihrer Ansichten, damit sie mit den geänderten Stileigenschaften in Einklang stehen.

In anderen Fällen werden die Ressourcenschlüssel nicht mehr unterstützt. Der XAML-Markup-Editor in Visual Studio hebt Verweise auf Ressourcenschlüssel hervor, die nicht aufgelöst werden können. Der XAML-Markup-Editor unterstreicht z. B. einen Verweis auf den Stilschlüssel `ListViewItemTextBlockStyle` mit einer roten Wellenlinie. Wird dieser Fehler nicht behoben, wird die App sofort beendet, wenn Sie versuchen, sie im Emulator oder auf dem Gerät bereitzustellen. Daher ist es wichtig, die Richtigkeit des XAML-Markups sicherzustellen. Sie werden feststellen, dass sich solche Fehler mit Visual Studio hervorragend abfangen lassen.

Für weiterhin unterstützte Schlüssel bedeuten Änderungen der Entwurfssprache, dass sich die Eigenschaften geändert haben, die von einigen Stilen festgelegt werden. Z. B. `TitleTextBlockStyle` legt **FontSize** zu 14.667px in einer Windows-Runtime 8.x-app und 18.14px in einer Windows Phone Store-app. Aber das gleiche Format wird **FontSize** auf eine viel größere 24px in einer Windows 10-app. Überprüfen Sie Ihre Entwürfe und Layouts, und verwenden Sie die passenden Formate an den richtigen Stellen. Weitere Informationen finden Sie unter [Richtlinien für Schriftarten](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx) und [Entwerfen von UWP-Apps](https://developer.microsoft.com/en-us/windows/apps/design).

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

Obwohl [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803) in der universellen Gerätefamilie implementiert ist, ist es auf mobilen Geräten nicht voll funktionsfähig. Verwenden Sie [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) für Ihre universelle Sucherfahrung. Nachfolgend finden Sie die gängige Vorgehensweise zur Implementierung einer Sucherfahrung mit **AutoSuggestBox**.

Sobald der Benutzer mit der Eingabe beginnt, wird das **TextChanged**-Ereignis mit der Ursache **UserInput** ausgelöst. Füllen Sie anschließend die Liste der Vorschläge auf, und legen Sie die **ItemsSource** von [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) fest. Während der Benutzer durch die Liste navigiert, wird das **SuggestionChosen**-Ereignis ausgelöst (und wenn Sie **TextMemberDisplayPath** festgelegt haben), wird das Textfeld automatisch mit der angegebenen Eigenschaft aufgefüllt. Wenn der Benutzer eine Auswahl mit der EINGABETASTE übermittelt, wird das **QuerySubmitted**-Ereignis ausgelöst. An dieser Stelle können Sie auf diesen Vorschlag reagieren (in diesem Fall wahrscheinlich das Navigieren zu einer anderen Seite mit weiteren Details zu den angegebenen Inhalten). Beachten Sie, dass die **LinguisticDetails**-Eigenschaft und die **Language**-Eigenschaft von **SearchBoxQuerySubmittedEventArgs** nicht mehr unterstützt werden (es gibt entsprechende APIs, um diese Funktion zu unterstützen). Und **KeyModifiers** wird nicht mehr unterstützt.

[**AutoSuggestBox** ](https://msdn.microsoft.com/library/windows/apps/dn633874) verfügt auch über Unterstützung für den Eingabemethoden-Editoren (IME). Und wenn Sie ein „Suchen“-Symbol anzeigen möchten, ist das ebenfalls möglich (durch die Interaktion mit dem Symbol wird das **QuerySubmitted**-Ereignis ausgelöst).

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

Siehe auch [Beispiel für AutoSuggestBox-Portierung](https://go.microsoft.com/fwlink/p/?linkid=619996).

## <a name="semanticzoom-changes"></a>SemanticZoom-Änderungen

Die Bewegung zum Verkleinern für [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) wurde im Windows Phone-Modell zusammengeführt. Hierzu wird auf einen Gruppenkopf getippt oder geklickt (auf Desktopcomputern wird also das Angebot der Schaltfläche „Minus“ zum Verkleinern nicht mehr angezeigt). Jetzt erhalten wir das gleiche, einheitliche Verhalten kostenlos auf allen Geräten. Es besteht der kosmetische Unterschied zum Windows Phone-Modell, dass die verkleinerte Ansicht (Sprungliste) die vergrößerte Ansicht ersetzt, anstatt sie zu überlagern. Aus diesem Grund können Sie alle halbtransparenten Hintergründe aus verkleinerten Ansichten entfernen.

Erweitern in einer Windows Phone Store-app die verkleinerte Ansicht auf die Größe des Bildschirms. In einer Windows-Runtime 8.x-app und in einer Windows 10-app, ist die Größe der verkleinerte Ansicht auf die Grenzen des beschränkt die **SemanticZoom** Steuerelement.

In einer Windows Phone Store-App scheinen Inhalte hinter der verkleinerten Ansicht (in Z-Reihenfolge) durch, wenn für den Hintergrund der verkleinerten Ansicht eine Transparenz festgelegt wurde. In einer Windows-Runtime 8.x-app und in einer Windows 10-app ist "nothing" hinter die verkleinerte Ansicht sichtbar.

In einer Windows-Runtime 8.x-app können Sie bei der die app deaktiviert und erneut aktiviert wird, die verkleinerte Ansicht ist geschlossen (wenn er angezeigt wird, wurde) und die vergrößerte Ansicht wird stattdessen angezeigt. In einer Windows Phone Store-app und in einer Windows 10-app verbleibt die verkleinerte Ansicht angezeigt, wenn es angezeigt wurde.

In einer Windows Phone Store-app und in einer Windows 10-app ist die verkleinerte Ansicht geschlossen, wenn die zurück-Taste gedrückt wird. Für eine Windows-Runtime 8.x-app besteht keine integrierte Schaltfläche "zurück" verarbeiten, sodass die Frage nicht zutrifft.

## <a name="settings"></a>Einstellungen

Die Windows-Runtime 8.x **"settingspane"** Klasse ist nicht für Windows 10 geeignet. Zusätzlich zur Erstellung einer Einstellungsseite sollten Sie Benutzern stattdessen die Möglichkeit geben, von der App aus auf die Seite zuzugreifen. Es wird empfohlen, diese Seite mit App-Einstellungen auf der obersten Ebene als letztes angeheftetes Element im Navigationsbereich verfügbar zu machen. Hier sehen Sie jedoch den vollständigen Satz von Optionen.

-   Navigationsbereich. Einstellungen sollten das letzte Element in der Navigationsliste und am Ende der Liste angeheftet sein.
-   App-Leiste/Symbolleiste (innerhalb einer Registerkartenansicht oder eines Pivot-Layouts). Einstellungen sollten das letzte Element im Flyout „Menü“ der App-Leiste oder Symbolleiste sein. Es wird davon abgeraten, Einstellungen als eines der Elemente der obersten Ebene in der Navigation zu verwenden.
-   Hub. Einstellungen sollte innerhalb des Flyouts „Menü“ platziert werden (über das Menü der App-Leiste oder Symbolleiste im Hublayout).

Es wird davon abgeraten, Einstellungen in einem Master-/Detailbereich zu verstecken.

Die Einstellungsseite sollte das gesamte App-Fenster ausfüllen, und außerdem sollten „Info“ und „Feedback“ auf der Einstellungsseite zu finden sein. Richtlinien für den Entwurf der Einstellungsseite finden Sie unter [Richtlinien für App-Einstellungen](https://msdn.microsoft.com/library/windows/apps/hh770544).

## <a name="text"></a>Text

Der Text (bzw. die Typografie) ist ein wichtiger Aspekt einer UWP-App. Beim Portieren ist es ratsam, das grafische Design Ihrer Ansichten noch einmal darauf zu prüfen, ob es zur neuen Entwurfssprache passt. Verwenden Sie die folgenden Abbildungen, um nach den verfügbaren **TextBlock**-Systemstilen der universellen Windows-Plattform (UWP) zu suchen. Suchen Sie diejenigen, die die Windows Phone Silverlight-Formaten entsprechen, die Sie verwendet. Alternativ können Sie Ihre eigenen universelle Stile erstellen, und kopieren Sie die Eigenschaften aus der Windows Phone Silverlight-System-Stile in die.

![system textblock styles foder windows 10 apps](images/label-uwp10stylegallery.png) <br/>TextBlock Systemstile für Windows 10-apps

In Windows-Runtime 8.x-apps und Windows Phone Store-apps ist die Standardschriftfamilie Global User Interface. In einer Windows 10-app ist die Standardschriftfamilie Segoe UI. Daher kann die Schriftartmetrik in Ihrer App Unterschiede aufweisen. Wenn Sie die Darstellung Ihres 8.1-Texts reproduzieren möchten, können Sie Ihre eigene Metrik mit Eigenschaften wie [**LineHeight**](https://msdn.microsoft.com/library/windows/apps/br209671) und [**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/br244362) festlegen.

In Windows-Runtime 8.x-apps und Windows Phone Store-apps, die Standardsprache für Text festgelegt ist, um die Sprache des Builds oder En-us. In einer Windows 10-app wird die Standardsprache für die Top-app-Sprache (Schriftart-fallback) festgelegt. Sie können [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066) explizit festlegen, aber Sie erhalten ein besseres Fallbackverhalten für Schriftarten, wenn Sie für diese Eigenschaft keinen Wert festlegen.

Weitere Informationen finden Sie unter [Richtlinien für Schriftarten](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx) und [Entwerfen von UWP-Apps](https://go.microsoft.com/fwlink/p/?LinkID=533896). Informationen zu Änderungen von Textsteuerelementen finden Sie oben im Abschnitt [Steuerelemente](#controls-and-control-styles-and-templates).

## <a name="theme-changes"></a>Designänderungen

Bei einer universellen 8.1-App ist das Standarddesign standardmäßig dunkel. Für Windows 10-Geräten das Standarddesign hat sich geändert, aber Sie können steuern, das Design für eine angeforderte Design in "App.xaml" deklariert. Wenn Sie z. B. auf allen Geräten ein dunkles Design verwenden möchten, fügen Sie dem Stammelement der App `RequestedTheme="Dark"` hinzu.

## <a name="tiles-and-toasts"></a>Kacheln und Popups

Für Kacheln und Popupbenachrichtigungen werden die Vorlagen, die Sie derzeit verwenden, weiterhin in Ihrer Windows 10-app funktionieren. Es sind jedoch neue adaptive Vorlagen verfügbar. Diese werden unter [Benachrichtigungen, Kacheln, Popups und Signale](https://msdn.microsoft.com/library/windows/apps/mt185606) beschrieben.

Auf Desktopcomputern war eine Popupbenachrichtigung früher eine vorübergehende Nachricht. Sie wurde ausgeblendet und konnte nicht mehr aufgerufen werden, nachdem sie geschlossen oder ignoriert wurde. Wenn unter Windows Phone eine Popupbenachrichtigung ignoriert oder vorerst geschlossen wird, wird sie ins Info-Center verschoben. Das Info-Center ist nun nicht mehr auf die Mobilgerätefamilie beschränkt.

Zum Senden einer Popupbenachrichtigung muss keine Funktion mehr deklariert werden.

## <a name="window-size"></a>Fenstergröße

Bei einer universellen 8.1-App wird das App-Manifestelement [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/dn391667) verwendet, um eine minimale Fensterbreite zu deklarieren. In Ihrer UWP-App können Sie eine Mindestgröße (Breite und Höhe) mit imperativem Code angeben. Die Standardmindestgröße beträgt 500 x 320 Epx. Dies ist auch die kleinste zulässige Mindestgröße. Die größte zulässige Mindestgröße ist 500 x 500 Epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

Das nächste Thema ist [Portieren: E/A, Gerät und App-Modell](w8x-to-uwp-input-and-sensors.md).

