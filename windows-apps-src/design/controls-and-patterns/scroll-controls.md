---
Description: Mittels Verschiebung und Bildlauf können Benutzer Inhalte erreichen, die sich jenseits der Bildschirmgrenzen befinden.
title: Bildlaufanzeige-Steuerelemente
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scrollbars
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: Abarlow, pagildea
design-contact: ksulliv
dev-contact: regisb
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3a3a2adee262a521bde77d657aa4f5283b68ddcf
ms.sourcegitcommit: e0ae346eadda864dcad1453cd1644668549e66e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603432"
---
# <a name="scroll-viewer-controls"></a>Bildlaufanzeige-Steuerelemente



Wenn mehr UI-Inhalte anzuzeigen sind, als in einen Bereich passen, verwenden Sie das Bildlaufanzeige-Steuerelement.

> **Wichtige APIs:** [ScrollViewer-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [ScrollBar-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.scrollbar)

Mithilfe von Bildlaufanzeigen kann Inhalt über die Grenzen des Anzeigebereichs (sichtbarer Bereich) hinausgehen. Benutzer können diesen Inhalt durch Bedienen der Bildlaufanzeigenoberfläche über Toucheingabe, Mausrad, Tastatur oder ein Gamepad oder mithilfe des Maus- oder Stiftcursors anzeigen, um mit der Bildlaufleiste der Bildlaufanzeige zu interagieren. Diese Abbildung zeigt mehrere Beispiele für Bildlaufanzeige-Steuerelemente.

![Screenshot mit einem standardmäßigen Bildlaufleistensteuerelement](images/ScrollBar_Standard.jpg)

Abhängig von der Situation verwendet die Bildlaufleiste der Bildlaufanzeige zwei verschiedene Visualisierungen, die in der folgenden Abbildung gezeigt werden: den Verschiebungsindikator (links) und die herkömmliche Bildlaufleiste (rechts).

![Beispiel für standardmäßige Bildlaufleisten- und Verschiebungsindikatoren-Steuerelemente](images/SCROLLBAR.png)

Die Bildlaufanzeige erkennt die Eingabemethode des Benutzers und ermittelt damit die anzuzeigende Visualisierung.

* Wenn in einem Bereich ein Bildlauf durchgeführt wird, ohne die Bildlaufleiste direkt zu benutzen, z. B. durch Berühren, wird der Verschiebungsindikator eingeblendet, welcher die aktuelle Bildlaufposition anzeigt.
* Wenn der Maus- oder Stiftcursor über den Verschiebungsindikator bewegt wird, verwandelt sich dieser in eine herkömmliche Bildlaufleiste.  Durch Ziehen des Ziehpunkts der Bildlaufleiste wird der Bildlaufbereich verändert.

<!--
<div class="microsoft-internal-note">
See complete redlines in [UNI]http://uni/DesignDepot.FrontEnd/#/ProductNav/3378/0/dv/?t=Windows|Controls|ScrollControls&f=RS2
</div>
-->

![Bildlaufleisten in Aktion](images/conscious-scroll.gif)

> [!NOTE]
> Wenn die Bildlaufleiste sichtbar ist, überlagert sie mit 16px den Inhalt innerhalb des ScrollViewer. Für ein gutes Benutzeroberflächendesign sollten Sie sicherstellen, dass durch diese Überlagerung keine interaktiven Inhalte verdeckt werden. Wenn sich Benutzeroberflächen lieber nicht überlappen sollen, lassen Sie vom Rand des Anzeigebereichs 16px Abstand für die Bildlaufleiste.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ScrollViewer">die App zu öffnen und ScrollViewer in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-scroll-viewer"></a>Erstellen einer Bildlaufanzeige

Um Ihrer Seite einen vertikalen Bildlauf hinzuzufügen, umschließen Sie den Seiteninhalt in einer Bildlaufanzeige.

```xaml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1">

    <ScrollViewer>
        <StackPanel>
            <TextBlock Text="My Page Title" Style="{StaticResource TitleTextBlockStyle}"/>
            <!-- more page content -->
        </StackPanel>
    </ScrollViewer>
</Page>
```

Dieser XAML-Code veranschaulicht das Aktivieren des horizontalen Scrollens, das Einfügen eines Bilds in eine Bildlaufanzeige und das Aktivieren des Zooms.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10"
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## <a name="scrollviewer-in-a-control-template"></a>ScrollViewer in einer Steuerelementvorlage

Normalerweise ist das ScrollViewer-Steuerelement Teil von anderen Steuerelementen. Eine ScrollViewer-Komponente zeigt zusammen mit der [ScrollContentPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollContentPresenter)-Klasse zur Unterstützung nur dann einen Viewport sowie Bildlaufleisten an, wenn der Layoutbereich des Hoststeuerelements einschränkt wird und kleiner als die Größe des erweiterten Inhalts ist. Dies ist häufig bei Listen der Fall, daher enthalten [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)- und [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)-Vorlagen immer ScrollViewer. [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) und [RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) umfassen ebenfalls ScrollViewer in der Vorlage.

Wenn eine **ScrollViewer**-Komponente in einem Steuerelement vorhanden ist, ist im Hoststeuerelement häufig die Ereignisbehandlung für bestimmte Eingabeereignisse und Bearbeitungen integriert, mit denen ein Bildlauf für den Inhalt durchgeführt werden kann. GridView interpretiert z. B. eine Wischbewegung, wodurch für den Inhalt ein horizontaler Bildlauf durchgeführt wird. Die Eingabeereignisse und Manipulationen von Rohdaten, die das Hoststeuerelement empfängt, werden als durch das Steuerelement behandelt betrachtet, und untergeordnete Ereignisse wie [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) werden nicht ausgelöst oder per Bubbling an übergeordnete Container weitergeleitet. Du kannst die integrierte Steuerelementverarbeitung teilweise ändern, indem du eine Steuerelementklasse und die virtuellen **On**_Event_-Methoden für Ereignisse überschreibst oder eine neue Vorlage für das Steuerelement verwendest. In beiden Fällen ist es allerdings nicht unkompliziert, das ursprüngliche Standardverhalten zu reproduzieren, das in der Regel vorhanden ist, damit das Steuerelement wie erwartet auf Ereignisse und Eingabeaktionen und -gesten des Benutzers reagiert. Sie sollten daher genau überlegen, ob das Eingabeereignis wirklich ausgelöst werden soll. Sie sollten überprüfen, ob andere Eingabeereignisse oder Gesten vorhanden sind, die nicht von dem Steuerelement behandelt werden, und diese im Entwurf für die App oder die Steuerelementinteraktion verwenden.

Damit Steuerelemente, die einen ScrollViewer enthalten, einige Verhaltensweisen und Eigenschaften innerhalb der ScrollViewer-Komponente steuern können, definiert ScrollViewer eine Reihe von angefügten XAML-Eigenschaften, die in Stilen festgelegt und in Vorlagenbindungen verwendet werden können. Weitere Informationen zu angefügten Eigenschaften finden Sie unter [Übersicht über angefügte Eigenschaften](../../xaml-platform/attached-properties-overview.md).

**Angefügte XAML-Eigenschaften für ScrollViewer**

ScrollViewer definiert die folgenden angefügten XAML-Eigenschaften:

- [ScrollViewer.BringIntoViewOnFocusChange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange)
- [ScrollViewer.HorizontalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility)
- [ScrollViewer.HorizontalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode)
- [ScrollViewer.IsDeferredScrollingEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled)
- [ScrollViewer.IsHorizontalRailEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled)
- [ScrollViewer.IsScrollInertiaEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled)
- [ScrollViewer.IsVerticalRailEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled)
- [ScrollViewer.IsVerticalScrollChainingEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled)
- [ScrollViewer.IsZoomChainingEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled)
- [ScrollViewer.IsZoomInertiaEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled)
- [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty)
- [ScrollViewer.VerticalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode)
- [ScrollViewer.ZoomMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode)

Diese angefügten XAML-Eigenschaften sind für Fälle vorgesehen, in denen ScrollViewer implizit ist, z. B. wenn der ScrollViewer in der Standardvorlage für ListView oder GridView vorhanden ist und Sie das Bildlaufverhalten des Steuerelements ohne Zugriff auf Vorlagenelemente steuern möchten.

Im Folgenden wird z. B. erläutert, wie Sie die vertikalen Bildlaufleisten für die integrierte Bildlaufanzeige einer ListView immer sichtbar machen.

```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/>
```

In Fällen, in denen im XAML-Code wie im Beispielcode gezeigt ein ScrollViewer explizit vorhanden ist, müssen Sie keine Syntax mit angefügten Eigenschaften verwenden. Verwenden Sie einfach eine Attributsyntax, z. B. `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`.


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Verwenden Sie möglichst ein Design für einen vertikalen Bildlauf und nicht für einen horizontalen Bildlauf.
- Verwenden Sie die Verschiebung entlang einer Achse für Inhaltsbereiche, die über eine Viewportgrenze (vertikal oder horizontal) hinausgehen. Verwenden Sie die Verschiebung entlang zweier Achsen für Inhaltsbereiche, die über beide Viewportgrenzen (vertikal und horizontal) hinausgehen.
- Verwenden Sie in der Listen- und Rasteransicht sowie im Kombinationsfeld, Listenfeld, Texteingabefeld und für Hubsteuerelemente die integrierte Bildlauffunktionalität. Wenn die Anzahl von Elementen so groß ist, dass sie nicht alle gleichzeitig angezeigt werden können, hat der Benutzer mit diesen Steuerelementen die Möglichkeit, einen vertikalen oder horizontalen Bildlauf durch die Elementliste durchzuführen.
- Wenn der Benutzer die Verschiebung in beide Richtungen um einen größeren Bereich herum ausführen und möglicherweise auch zoomen soll (wenn Sie dem Benutzer beispielsweise das Verschieben und Zoomen über ein Bild in voller Größe ermöglichen möchten, anstatt ein Bild mit an den Bildschirm angepasster Größe zu verwenden), positionieren Sie das Bild in einer Bildlaufanzeige.
- Wenn der Benutzer in einer langen Textpassage einen Bildlauf ausführen wird, konfigurieren Sie die Bildlaufanzeige ausschließlich für den vertikalen Bildlauf.
- Bei Verwendung einer Bildlaufanzeige darf diese nur ein Objekt umfassen. Beachten Sie, dass es sich bei dem einen Objekt um einen Layoutbereich handeln kann, der wiederum eine beliebige Anzahl eigener Objekte enthält.
- Platzieren Sie kein [Pivot](pivot.md)-Steuerelement in einer Bildlaufanzeige, um Konflikte mit der Pivot-Bildlauflogik zu vermeiden.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Verwandte Themen

**Für Entwickler (XAML)**

* [ScrollViewer-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)
