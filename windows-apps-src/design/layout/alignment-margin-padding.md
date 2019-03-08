---
Description: Verwenden Sie Alignment, Margin und padding-Eigenschaften, um das Layout der Elemente auf einer Seite anzuordnen.
title: Ausrichtung, Rand und Abstand beim Layout
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3c7ca724279a6a4d41b1f7757428af8eab403549
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600935"
---
# <a name="alignment-margin-padding"></a>Ausrichtung, Rand, Abstand

In UWP-Apps erben die meisten Oberflächenelemente (UI-Elemente) von der [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)-Klasse. Jedes FrameworkElement hat Dimension-, Ausrichtung-, Rand- und Abstand-Eigenschaften, die das Layoutverhalten beeinflussen. Die folgende Anleitung gibt einen Überblick über die Verwendung dieser Layout-Eigenschaften, um sicherzustellen, dass die Benutzeroberfläche Ihrer App in jedem Kontext lesbar und einfach zu bedienen ist.

## <a name="dimensions-height-width"></a>Dimensionen (Höhe, Breite)
Die richtige Dimensionierung stellt sicher, dass alle Inhalte übersichtlich und lesbar sind. Benutzer sollten nicht scrollen oder zoomen müssen, um primäre Inhalte zu entziffern.

![Diagramm zu Dimensionen](images/dimensions.svg)

- [**Höhe** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) und [ **Breite** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) Geben Sie die Größe eines Elements. Die Standardwerte sind mathematisch NaN (Not A Number). Sie können feste Werte verwenden, die in [effektiven Pixeln](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) gemessen werden, oder Sie können eine **automatische** oder [proportionale Größenanpassung](layout-panels.md#grid) verwenden.

- [**ActualHeight** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) und [ **ActualWidth** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) sind schreibgeschützte Eigenschaften, die die Größe eines Elements zur Laufzeit bereitstellen. Wenn Fluid-Layouts wachsen oder schrumpfen, ändern sich die Werte in einem [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)-Ereignis. Beachten Sie, dass eine [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) die Werte ActualHeight und ActualWidth nicht ändert.

- [**MinWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) und [ **MinHeight** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight) / [ **MaxHeight** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) Werte angeben, die die Größe eines Elements zu beschränken und fließende Ändern der Größe.

- [**FontSize** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize) und anderen Texteigenschaften steuern die Layoutgröße für Text-Elemente. Textelemente haben zwar keine explizit deklarierten Dimensionen, aber trotzdem werden ActualWidth und ActualHeight berechnet. 

## <a name="alignment"></a>Ausrichtung
Die Ausrichtung lässt Ihre Benutzeroberfläche ordentlich, organisiert und ausgewogen aussehen und kann auch dazu verwendet werden, visuelle Hierarchien und Beziehungen aufzubauen.

![Diagramm zur Ausrichtung](images/alignment.svg)

- [**HorizontalAlignment** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) und [ **VerticalAlignment** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) angeben, wie ein Element innerhalb seines übergeordneten Containers positioniert werden soll.
    - Die Werte für **HorizontalAlignment** sind **Left**, **Center**, **Right** und **Stretch**.
    - Die Werte für **VerticalAlignment** sind **Top**, **Center**, **Bottom** und **Stretch**.

- **Stretch** ist der Standard für beide Eigenschaften. Elemente füllen den gesamten Platz, den sie im übergeordneten Container zur Verfügung haben. Echte Zahlen für Height und Width heben einen Stretch-Wert auf, der stattdessen als Center-Wert fungiert. Einige Steuerelemente, wie z. B. Schaltflächen, überschreiben den Standardwert Stretch in ihrem Standardstil.

- [**"HorizontalContentAlignment"** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) und [ **"VerticalContentAlignment"** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) anzugeben, wie die untergeordneten Elemente in einem Container positioniert werden.

- Die Ausrichtung kann sich auf das Beschneiden innerhalb eines Layout-Panels auswirken. Beispielsweise wird bei `HorizontalAlignment="Left"` die rechte Seite des Elements beschnitten, wenn der Inhalt größer als die ActualWidth ist.

- Textelemente verwenden die Eigenschaft [**TextAlignment**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.textalignment). Generell empfehlen wir, den Standardwert left-alignment zu verwenden. Weitere Informationen zur Gestaltung von Text finden Sie unter [Typografie](../style/typography.md).

## <a name="margin-and-padding"></a>Rand und Abstand
Rand- und Abstand-Eigenschaften verhindern, dass die Benutzeroberfläche zu überladen oder zu spärlich aussieht. Sie können es außerdem einfacher machen, bestimmte Eingaben wie Stift und Touch zu verwenden. Hier ist eine Illustration, die Ränder und Abstände für einen Container und seinen Inhalt anzeigt.

![Diagramm zu XAML-Rändern und -Abständen](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>Margin
[**Rand** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin) steuert die Menge an leerem Platz um ein Element. Margin fügt keine Pixel zu ActualHeight und ActualWidth hinzu und wird beim Hit-Testing und bei Sourcing-Eingabe-Ereignisse nicht als Teil des Elements betrachtet.

- Die Margin-Werte können einheitlich oder eindeutig sein. Mit `Margin="20"` würde ein einheitlicher Rand von 20 Pixeln auf das Element auf der linken, oberen, rechten und unteren Seite angewendet werden. Mit `Margin="0,10,5,25"` werden die Werte links, oben, rechts und unten (in dieser Reihenfolge) angewendet. 

- Ränder sind additiv. Wenn zwei Elemente einen einheitlichen Rand von 10 Pixeln angeben und in beliebiger Ausrichtung nebeneinander liegen, beträgt der Abstand zwischen ihnen 20 Pixel.

- Negative Ränder sind zulässig. Die Verwendung eines negativen Rands kann jedoch oftmals Beschneidungen oder Überzeichnungen von Peers verursachen. Die Verwendung negativer Ränder ist daher keine übliche Technik.

- Margin-Werte werden als letztes eingeschränkt, also seien Sie vorsichtig mit Rändern, da Container Elemente beschneiden oder einschränken können. Ein Margin-Wert kann die Ursache dafür sein, dass ein Element nicht gerendert wird. Wenn ein Rand angewendet wird, kann die Dimension eines Elements auf 0 beschränkt werden.

### <a name="padding"></a>Abstand
[**Padding** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding) gesteuert, wie viel Platz zwischen den inneren Rahmen eines Elements und dessen untergeordneten Inhalt oder die Elemente. Ein positiver Padding-Wert verkleinert den Inhaltsbereich des Elements. 

Im Gegensatz zu Margin ist Padding keine Eigenschaft von FrameworkElement. Es gibt mehrere Klassen, die jeweils ihre eigene Padding-Eigenschaft definieren:

-   [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding): für alle erbt [ **Steuerelement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls) abgeleiteten Klassen. Nicht alle Steuerelemente haben Inhalt, sodass bei diesen Steuerelementen das Setzen der Eigenschaft nichts bewirkt. Wenn das Steuerelement einen Rahmen hat, gilt der Abstand innerhalb dieses Rahmens.
-   [**Border.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding): definiert von Leerzeichen zwischen der Rechteck Zeile erstellt [ **BorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**  ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush) und [ **untergeordneten** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child) Element.
-   [**ItemsPresenter.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding): trägt zur Darstellung der Elemente im Datenelement-Steuerelemente, platzieren für die einzelnen Elemente des angegebenen paddings.
-   [**TextBlock.Padding** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding) und [ **RichTextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding): Erweitern Sie den Text des Textelements umgebende Feld. Diese Textelemente haben keine **Background**-Eigenschaft, sodass es schwierig sein kann, sie zu sehen. Verwenden Sie für [**Block**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block)-Container daher stattdessen [**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin)-Einstellungen.

In jedem dieser Fälle haben Elemente auch eine Margin-Eigenschaft. Wenn sowohl Margin als auch Padding angewendet werden, sind sie additiv: der scheinbare Abstand zwischen einem äußeren Container und einem beliebigen inneren Inhalt ist Margin plus Padding.

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
Sie müssen nicht jeden Eigenschaftswert einzeln für ein Steuerelement festlegen. Es ist in der Regel effizienter, Eigenschaftswerte in einer [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)-Ressource zu gruppieren und den Stil auf ein Steuerelement anzuwenden. Dies gilt insbesondere dann, wenn Sie die gleichen Eigenschaftswerte auf viele Steuerelemente anwenden müssen. Weitere Informationen zum Verwenden von Stilen finden Sie unter [XAML-Stile](../controls-and-patterns/xaml-styles.md).

## <a name="general-recommendations"></a>Allgemeine Empfehlungen
- Wenden Sie Maßwerte nur auf bestimmte Schlüsselelemente an und verwenden Sie das Fluid-Layout-Verhalten für die anderen Elemente. Dies sorgt für eine [dynamische Benutzeroberfläche](responsive-design.md), wenn sich die Fensterbreite ändert.

- Wenn Sie Maßwerte verwenden, sollten **alle Dimensionen, Ränder und Abstände ein Vielfaches von 4 epx sein**. Wenn UWP [effektive Pixel und Skalierung](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) verwendet, um Ihre Anwendung auf allen Geräten und Bildschirmgrößen lesbar zu machen, skaliert es UI-Elemente um ein Vielfaches von 4. Die Verwendung von Werten in 4er-Schritten ergibt durch Ausrichtung auf ganze Pixel das beste Rendering.

- Für kleine Fensterbreiten (weniger als 640 Pixel) empfehlen wir 12 epx-Schritte, für größere Fensterbreiten 24 epx-Schritte.

![Empfohlene Schritte](images/12-gutter.svg)

## <a name="related-topics"></a>Verwandte Themen
* [**FrameworkElement.Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
