---
Description: Verwende die Eigenschaften für Ausrichtung, Rand und Abstand, um das Layout von Elementen auf einer Seite anzuordnen.
title: Ausrichtung, Rand und Abstand beim Layout
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0d7f702d145740703b9fbc4ca2e7fd8eba8957cc
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684461"
---
# <a name="alignment-margin-padding"></a>Ausrichtung, Rand, Abstand

In UWP-Apps erben die meisten Oberflächenelemente (UI-Elemente) von der [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)-Klasse. Jedes FrameworkElement hat Eigenschaften für Dimensionen, Ausrichtung, Rand und Abstand, die das Layoutverhalten beeinflussen. Die folgende Anleitung gibt eine Übersicht über die Verwendung dieser Layouteigenschaften, um sicherzustellen, dass die Benutzeroberfläche deiner App in jedem Kontext lesbar und einfach zu bedienen ist.

## <a name="dimensions-height-width"></a>Dimensionen (Höhe, Breite)
Die richtige Dimensionierung stellt sicher, dass alle Inhalte übersichtlich und lesbar sind. Benutzer sollten nicht scrollen oder zoomen müssen, um primäre Inhalte zu entziffern.

![Diagramm zu Dimensionen](images/dimensions.svg)

- [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) und [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) geben die Größe eines Elements an. Die Standardwerte sind mathematisch NaN (Not a Number, keine Zahl). Du kannst feste Werte, die in [effektiven Pixeln](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) gemessen werden, oder eine **automatische** oder [proportionale Größenanpassung](layout-panels.md#grid) verwenden.

- [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) und [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) sind schreibgeschützte Eigenschaften, die die Größe eines Elements zur Laufzeit bereitstellen. Wenn dynamische Layouts wachsen oder schrumpfen, ändern sich die Werte in einem [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)-Ereignis. Beachte, dass durch [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) die Werte ActualHeight und ActualWidth nicht geändert werden.

- [**MinWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) und [**MinHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) legen Werte fest, die die Größe eines Elements begrenzen, während weiterhin eine dynamische Größenanpassung möglich ist.

- [**FontSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize) und andere Texteigenschaften steuern die Layoutgröße für Textelemente. Textelemente haben zwar keine explizit deklarierten Dimensionen, es werden jedoch trotzdem ActualWidth und ActualHeight berechnet. 

## <a name="alignment"></a>Ausrichtung
Durch Ausrichtung erhält deine Benutzeroberfläche ein ordentliches, organisiertes und ausgewogenes Aussehen, und sie kann auch dazu verwendet werden, visuelle Hierarchien und Beziehungen aufzubauen.

![Diagramm zur Ausrichtung](images/alignment.svg)

- [**HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) und [**VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) geben an, wie ein Element innerhalb des übergeordneten Containers positioniert werden soll.
    - Die Werte für **HorizontalAlignment** sind **Left**, **Center**, **Right** und **Stretch**.
    - Die Werte für **VerticalAlignment** sind **Top**, **Center**, **Bottom** und **Stretch**.

- **Stretch** ist der Standard für beide Eigenschaften. Elemente füllen den gesamten Platz aus, der ihnen im übergeordneten Container zur Verfügung steht. Echte Zahlen für Height und Width heben einen Stretch-Wert auf, der dann stattdessen als Center-Wert fungiert. Einige Steuerelemente, z. B. Schaltflächen, setzen den Stretch-Standardwert in ihrem Standardstil außer Kraft.

- [**HorizontalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) und [**VerticalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) legen fest, wie untergeordnete Elemente innerhalb eines Containers positioniert werden.

- Die Ausrichtung kann sich auf das Beschneiden innerhalb eines Layoutbereichs auswirken. Beispielsweise wird bei `HorizontalAlignment="Left"` die rechte Seite des Elements beschnitten, wenn der Inhalt größer als die ActualWidth ist.

- Textelemente verwenden die [**TextAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment)-Eigenschaft. Generell wird empfohlen, den Standardwert left-alignment zu verwenden. Weitere Informationen zur Gestaltung von Text findest du unter [Typografie](../style/typography.md).

## <a name="margin-and-padding"></a>Rand und Abstand
Eigenschaften für Rand und Abstand verhindern, dass die Benutzeroberfläche zu überladen oder zu spärlich aussieht. Außerdem ist es mit ihnen häufig einfacher, bestimmte Eingaben wie Stift und Touch zu verwenden. Hier ist eine Illustration, die Ränder und Abstände für einen Container und seinen Inhalt anzeigt.

![Diagramm zu XAML-Rändern und -Abständen](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>Margin
[**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin) steuert den Leerraum um ein Element. Margin fügt ActualHeight und ActualWidth keine Pixel hinzu und wird bei Zugriffstests und bei Sourcingeingabeereignissen nicht als Teil des Elements betrachtet.

- Die Margin-Werte können einheitlich oder eindeutig sein. Mit `Margin="20"` wird ein einheitlicher Rand von 20 Pixeln auf das Element auf der linken, oberen, rechten und unteren Seite angewandt. Mit `Margin="0,10,5,25"` werden die Werte links, oben, rechts und unten (in dieser Reihenfolge) angewandt. 

- Ränder sind additiv. Wenn zwei Elemente einen einheitlichen Rand von 10 Pixel angeben und in beliebiger Ausrichtung nebeneinander liegen, beträgt der Abstand zwischen ihnen 20 Pixel.

- Negative Ränder sind zulässig. Die Verwendung eines negativen Rands kann jedoch oftmals Beschneidungen oder Überzeichnungen von Peers verursachen. Die Verwendung negativer Ränder ist daher keine übliche Technik.

- Margin-Werte werden als Letztes eingeschränkt, daher musst du vorsichtig mit Rändern umgehen, da Container Elemente beschneiden oder einschränken können. Ein Margin-Wert kann die Ursache dafür sein, dass ein Element nicht gerendert wird. Wenn ein Rand angewandt wird, kann die Dimension eines Elements auf 0 beschränkt werden.

### <a name="padding"></a>Abstand
[**Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding) steuert den Abstand zwischen dem inneren Rand eines Elements und seinem Inhalt oder seinen untergeordneten Elementen. Ein positiver Padding-Wert verkleinert den Inhaltsbereich des Elements. 

Im Gegensatz zu Margin ist Padding keine Eigenschaft von FrameworkElement. Es gibt mehrere Klassen, die jeweils eine eigene Padding-Eigenschaft definieren:

-   [**Control.Padding:** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding) wird für alle von [**Control**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls) abgeleiteten Klassen vererbt. Nicht alle Steuerelemente weisen Inhalte auf, sodass bei diesen Steuerelementen das Festlegen der Eigenschaft nichts bewirkt. Wenn das Steuerelement einen Rahmen aufweist, gilt der Abstand innerhalb dieses Rahmens.
-   [**Border.Padding:** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding) definiert den Abstand zwischen der von [**BorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush) gebildeten Rechtecklinie und dem [**Child**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child)-Element.
-   [**ItemsPresenter.Padding:** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding) trägt zur Darstellung der Objekte für Elemente in Elementsteuerelementen bei. Dabei wird der angegebene Abstand um die einzelnen Elemente herum platziert.
-   [**TextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding) und [**RichTextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding): erweitern den Begrenzungsrahmen um den Text des Textelements. Diese Textelemente weisen keine **Background**-Eigenschaft auf, sodass sie schwierig zu erkennen sein können. Verwende für [**Block**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin)-Container daher stattdessen [**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block)-Einstellungen.

In allen diesen Fällen weisen Elemente auch eine Margin-Eigenschaft auf. Wenn sowohl Margin als auch Padding angewandt werden, sind sie additiv: Der erkennbare Abstand zwischen einem äußeren Container und beliebigem innerem Inhalt beträgt Margin plus Padding.

### <a name="example"></a>Beispiel
Sehen wir uns nun die Effekte „Margin“ und „Padding“ auf echte Steuerelemente an. Hier sehen Sie ein „TextBox“ innerhalb eines „Grid“ mit dem Standardwert 0 für „Margin“ und „Padding“.

![TextBox mit Rand und Abstand von 0](images/xaml-layout-textbox-no-margins-padding.svg)

Hier sehen Sie das gleiche „“TextBox und „Grid“ mit „Margin“- und „Padding“-Werten für das „TextBox“, wie in diesem XAML dargestellt.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="16,24"/>
</Grid>
```

![TextBox mit positiven Rand- und Abstandswerten](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>Stilressourcen
Sie müssen nicht jeden Eigenschaftswert einzeln für ein Steuerelement festlegen. Es ist in der Regel effizienter, Eigenschaftswerte in einer [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)-Ressource zu gruppieren und den Stil auf ein Steuerelement anzuwenden. Dies gilt insbesondere dann, wenn Sie die gleichen Eigenschaftswerte auf viele Steuerelemente anwenden müssen. Weitere Informationen zum Verwenden von Stilen findest du unter [XAML-Stile](../controls-and-patterns/xaml-styles.md).

## <a name="general-recommendations"></a>Allgemeine Empfehlungen
- Wende Maßwerte nur auf bestimmte Schlüsselelemente an, und verwende das dynamische Layoutverhalten für die anderen Elemente. Dies sorgt für eine [reaktionsschnelle Benutzeroberfläche](responsive-design.md), wenn die Fensterbreite geändert wird.

- Wenn du Maßwerte verwendest, sollten **alle Dimensionen, Ränder und Abstände ein Vielfaches von 4 epx sein**. Wenn UWP [effektive Pixel und Skalierung](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) verwendet, um deine Anwendung auf allen Geräten und Bildschirmgrößen lesbar zu machen, werden die UI-Elemente um ein Vielfaches von 4 skaliert. Die Verwendung von Werten in Schritten von 4 sorgt durch Ausrichtung auf ganze Pixel für optimales Rendering.

- Für kleine Fensterbreiten (unter 640 Pixel) werden Bundstege von 12 epx empfohlen, für größere Fensterbreiten Bundstege von 24 epx.

![Empfohlene Bundstege](images/12-gutter.svg)

## <a name="related-topics"></a>Verwandte Themen
* [**FrameworkElement.Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
