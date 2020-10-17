---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Verwenden von Pinseln
description: Mit Brush-Objekten werden Innenbereiche oder Ränder von Formen, Text und Teilen von Steuerelementen gezeichnet, damit das gezeichnete Objekt auf einer Benutzeroberfläche sichtbar ist.
ms.date: 04/28/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1bf42e75ed8bb6d22fe8d4829aa6df32fa130230
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860043"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>Verwenden von Pinseln zum Zeichnen von Vor- und Hintergründen sowie Rändern

Verwende [**Pinsel**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Objekte, um das Innere und die Umrisse von XAML-Formen, -Text und -Steuerelementen zu zeichnen, sodass Sie in der Benutzeroberfläche der Anwendung sichtbar werden.

> **Wichtige APIs:**  [Brush-Klasse](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>Einführung in Pinsel

Zum Malen von auf dem App-Canvas angezeigten [**Formen**](/uwp/api/Windows.UI.Xaml.Shapes.Shape), Texten oder [**Steuerelementteilen**](/uwp/api/Windows.UI.Xaml.Controls.Control) legen Sie die Eigenschaft [**Füllen**](/uwp/api/windows.ui.xaml.shapes.shape.fill) der **Form** oder die Eigenschaften [**Hintergrund**](/uwp/api/windows.ui.xaml.controls.control.background) und [**Vordergrund**](/uwp/api/windows.ui.xaml.controls.control.foreground) eines **Steuerelements** auf einen **Pinsel**-Wert fest.

Es gibt folgende Pinseltypen: 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) 
-   [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>Einfarbige Pinsel

Ein [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) zeichnet einen Bereich mit einer einzelnen [**Color**](/uwp/api/Windows.UI.Color), z. B. Rot oder Blau. Dies ist der einfachste Pinsel. Es gibt in XAML drei Möglichkeiten, einen **SolidColorBrush** und die zugehörige Farbe zu definieren: vordefinierte Farbnamen, hexadezimale Farbwerte und die Syntax für Eigenschaftselemente.

### <a name="predefined-color-names"></a>Vordefinierte Farbnamen

Sie können einen vordefinierten Farbnamen wie [**Yellow**](/uwp/api/windows.ui.colors.yellow) oder [**Magenta**](/uwp/api/windows.ui.colors.magenta) verwenden. Es stehen 256 benannte Farben zur Verfügung. Der XAML-Parser wandelt den Farbnamen in eine [**Color**](/uwp/api/Windows.UI.Color)-Struktur mit den richtigen Farbkanälen um. Die 256 benannten Farben basieren auf den *X11*-Farbnamen der CSS3-Spezifikation (Cascading Style Sheets, Level 3). Möglicherweise kennen Sie diese Liste benannter Farben also bereits, wenn Sie über Vorkenntnisse in Webentwicklung oder -design verfügen.

In diesem Beispiel wird die [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill)-Eigenschaft eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) auf die vordefinierte Farbe [**Red**](/uwp/api/windows.ui.colors.red) festgelegt.

```xaml
<Rectangle Width="100" Height="100" Fill="Red" />
```

![Ein gerenderter SolidColorBrush](images/brushes-solidcolorbrush.jpg)

*Auf ein Rechteck angewendeter SolidColorBrush*

Wenn Sie einen [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) mit Code anstatt mit XAML definieren, sind die einzelnen benannten Farben als statische Eigenschaftswerte der [**Colors**](/uwp/api/windows.ui.colors)-Klasse verfügbar. Wenn Sie z. B. einen [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color)-Wert eines **SolidColorBrush** zum Darstellen der benannten Farbe „Orchid“ deklarieren möchten, legen Sie den **Color**-Wert auf den statischen Wert [**Colors.Orchid**](/uwp/api/windows.ui.colors.orchid) fest.

### <a name="hexadecimal-color-values"></a>Hexadezimale Farbwerte

Sie können eine Zeichenfolge im Hexadezimalformat verwenden, um präzise 24-Bit-Farbwerte mit 8-Bit-Alphakanal für einen [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) zu deklarieren. Zwei Zeichen im Bereich 0 bis F definieren die einzelnen Komponentenwerte. Die Reihenfolge der Komponentenwerte für die Hexadezimalzeichenfolge lautet wie folgt: Alphakanal (Deckkraft), roter Kanal, grüner Kanal und blauer Kanal (**ARGB**). Der Hexadezimalwert „\#FFFF0000“ definiert z. B. ein vollständig deckendes Rot (Alpha=„FF“, Rot=„FF“, Grün=„00“ und Blau=„00“).

In diesem XAML-Beispiel wird die [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill)-Eigenschaft eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) auf den Hexadezimalwert „\#FFFF0000“ festgelegt. Das Ergebnis ist das gleiche wie bei der Verwendung der benannten Farbe [**Colors.Red**](/uwp/api/windows.ui.colors.red).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="property-element-syntax"></a>Syntax für Eigenschaftselemente

Mithilfe der Syntax für Eigenschaftselemente können Sie einen [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) definieren. Diese Syntax ist ausführlicher als die zuvor gezeigten Methoden, und Sie können zusätzliche Eigenschaftswerte für ein Element angeben, wie z. B. die [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity) des Pinsels. Weitere Informationen zur XAML-Syntax, einschließlich der Syntax für Eigenschaftselemente, finden Sie in der [XAML-Übersicht](../../xaml-platform/xaml-overview.md) und der [Anleitung zur XAML-Syntax](../../xaml-platform/xaml-syntax-guide.md).

In den voranstehenden Beispielen wird der erstellte Pinsel implizit und automatisch erstellt und ist Teil einer durchdachten Kurzschrift der XAML-Sprache, mit der die UI-Definitionen für die am häufigsten Anwendungsfälle unkompliziert gehalten werden. Im nächsten Beispiel werden ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) und dann explizit der [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) als Elementwert für eine [**Rectangle.Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill)-Eigenschaft erstellt. Die [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) von **SolidColorBrush** wird auf [**Blue**](/uwp/api/windows.ui.colors.blue) und die [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity) auf 0,5 festgelegt.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="linear-gradient-brushes"></a>Lineare Farbverlaufspinsel

Ein [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) zeichnet einen Bereich mit einem Farbverlauf entlang einer Linie. Diese Linie wird als *Farbverlaufsachse* bezeichnet. Sie geben die Farben für den Farbverlauf und ihre Position an der Farbverlaufsachse mithilfe von [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop)-Objekten an. Standardmäßig verläuft die Farbverlaufsachse von der oberen linken Ecke zur unteren rechten Ecke des Bereichs, den der Pinsel zeichnet. Dies führt zu einer diagonalen Schattierung.

Der [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) bildet die Grundlage für einen Farbverlaufspinsel. Ein Farbverlaufsstopp gibt die [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) des Pinsels an einem [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) entlang der Farbverlaufsachse an, wenn der Pinsel auf den zu zeichnenden Bereich angewendet wird.

Die [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color)-Eigenschaft des Farbverlaufsstopps gibt die Farbe am Farbverlaufsstopp an. Sie können diese Farbe mit einem vordefinierten Farbnamen oder mit hexadezimalen **ARGB**-Werten angeben.

Die [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset)-Eigenschaft eines [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) gibt die Position für jeden **GradientStop** entlang der Farbverlaufsachse an. Der **Offset** ist ein **double**-Wert zwischen 0 und 1. Mit einem **Offset** von 0 wird der **GradientStop** am Anfang der Farbverlaufsachse platziert, also in der Nähe des [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint). Ein **Offset** von 1 platziert den **GradientStop** am [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint). Ein sinnvoller [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) sollte mindestens über zwei **GradientStop**-Werte verfügen. Dabei sollte jeder **GradientStop** eine andere [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) und einen anderen **Offset** zwischen 0 und 1 aufweisen.

Dieses Beispiel erstellt einen linearen Farbverlauf mit vier Farben, der zum Zeichnen eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) verwendet wird.

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

Die Farbe der einzelnen Punkte zwischen den Farbverlaufsstopps wird linear als eine Kombination der von den beiden umgebenden Farbverlaufsstopps angegebenen Farben interpoliert. In der folgenden Abbildung sind die Farbverlaufsstopps aus dem vorherigen Beispiel hervorgehoben. Die Kreise kennzeichnen die Position der Farbverlaufsstopps, und die gestrichelte Linie zeigt die Farbverlaufsachse.

![Diagramm mit den Farbverlaufsstopps 1 bis 4, die in der oberen linken Ecke des Diagramms beginnen und nach unten und rechts abfallen, bis sie die untere rechte Ecke des Diagramms erreichen.](images/linear-gradients-stops.png)

*Kombination der von den beiden umgebenden Farbverlaufsstopps angegebenen Farben*

Sie können die Linie ändern, an der die Farbverlaufsstopps liegen, indem Sie die [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint)-Eigenschaft und die [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint)-Eigenschaft auf andere Werte als die Standardausgangswerte `(0,0)` und `(1,1)` festlegen. Durch Ändern der Koordinatenwerte für **StartPoint** und **EndPoint** können Sie horizontale oder vertikale Farbverläufe erstellen, die Richtung des Farbverlaufs umkehren oder die Farbverlaufsausdehnung verkleinern und so auf einen kleineren Bereich als den gesamten gezeichneten Bereich anwenden. Zum Verkleinern des Farbverlaufs legen Sie die Werte für **StartPoint** und/oder **EndPoint** auf einen Wert zwischen 0 und 1 fest. Angenommen, Sie möchten einen horizontalen Farbverlauf erstellen, bei dem der Farbverlauf nur in der linken Hälfte des Pinsels erfolgt und die rechte Hälfte nur die letzte [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop)-Farbe hat. Hierfür geben Sie einen **StartPoint** von `(0,0)` und einen **EndPoint** von `(0.5,0)` an.

### <a name="use-tools-to-make-gradients"></a>Verwenden von Tools zum Erstellen von Farbverläufen

Nachdem Sie nun wissen, wie lineare Farbverläufe funktionieren, können Sie Visual Studio oder Blend nutzen, um diese Farbverläufe einfacher erstellen zu können. Um einen Farbverlauf zu erstellen, wählen Sie in der Entwurfsoberfläche oder in der XAML-Ansicht das Objekt aus, auf das ein Farbverlauf angewendet werden soll. Erweitern Sie **Pinsel**, und wählen Sie die Registerkarte **Linearer Farbverlauf** aus.

![Erstellen eines linearen Farbverlaufs in Visual Studio](images/tool-gradient-brush-1.png)

*Erstellen eines linearen Farbverlaufs in Visual Studio*

Jetzt können Sie die Farben der Farbverlaufsstopps ändern und ihre Positionen unter Verwendung der Leiste am unteren Rand verschieben. Sie können auch neue Farbverlaufsstopps hinzufügen, indem Sie auf die Leiste klicken, und Farbverlaufsstopps entfernen, indem Sie sie von der Leiste herunter ziehen (siehe nächstes Bildschirmfoto).

![Leiste am unteren Rand des Eigenschaftenfensters zur Steuerung der Farbverlaufsstopps](images/tool-gradient-brush-2.png)

*Schieberegler für Farbverlauf*

## <a name="radial-gradient-brushes"></a>Radiale Farbverlaufspinsel

Ein [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) wird innerhalb einer Ellipse gezeichnet, die durch die Eigenschaften [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) und [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) definiert wird. Farben für den Farbverlauf beginnen in der Mitte der Ellipse und enden am Radius.

Die Farben für den radialen Farbverlauf werden durch Farbstopps definiert, die der [**GradientStops**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientstops)-Sammlungseigenschaft hinzugefügt werden. Jeder Farbverlaufsstopp gibt eine Farbe und einen Versatz entlang des Farbverlaufs an.

Der Farbverlaufsursprung ist standardmäßig zentriert und kann mithilfe der [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin)-Eigenschaft versetzt werden.

[MappingMode](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) definiert, ob [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx), [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) und [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) relative oder absolute Koordinaten angeben.

Wenn [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) auf `RelativeToBoundingBox` festgelegt ist, werden die X- und Y-Werte der drei Eigenschaften als relativ zu den Elementgrenzen behandelt, wobei `(0,0)` die obere linke und `(1,1)` die untere rechte Ecke der Elementgrenzen für die Eigenschaften [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) und [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) und `(0,0)` den Mittelpunkt für die Eigenschaft [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) angibt.

Wenn [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) auf `Absolute` festgelegt ist, werden die X- und Y-Werte der drei Eigenschaften als absolute Koordinaten innerhalb der Elementgrenzen behandelt.

Dieses Beispiel erstellt einen linearen Farbverlauf mit vier Farben, der zum Zeichnen eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) verwendet wird.

```xml
<!-- This rectangle is painted with a radial gradient. -->
<Rectangle Width="200" Height="200">
    <Rectangle.Fill>
        <media:RadialGradientBrush>
            <GradientStop Color="Blue" Offset="0.0" />
            <GradientStop Color="Yellow" Offset="0.2" />
            <GradientStop Color="LimeGreen" Offset="0.4" />
            <GradientStop Color="LightBlue" Offset="0.6" />
            <GradientStop Color="Blue" Offset="0.8" />
            <GradientStop Color="LightGray" Offset="1" />
        </media:RadialGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

Die Farbe der einzelnen Punkte zwischen den Farbverlaufsstopps wird radial als eine Kombination der von den beiden umgebenden Farbverlaufsstopps angegebenen Farben interpoliert. In der folgenden Abbildung sind die Farbverlaufsstopps aus dem vorherigen Beispiel hervorgehoben. 

![Screenshot eines radialen Farbverlaufs.](images/radial-gradient.png)

*Farbverlaufsstopps*

## <a name="image-brushes"></a>Bildpinsel

Mit einem [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) wird in einem Bereich ein Bild gezeichnet. Dabei stammt das zu zeichnende Bild aus einer Bilddateiquelle. Legen Sie die [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource)-Eigenschaft auf den Pfad des Bilds fest, das Sie laden möchten. Normalerweise stammt die Bildquelle aus einem **Content**-Element, das Teil der App-Ressourcen ist.

Ein [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) streckt das Bild standardmäßig so weit, dass es den gezeichneten Bereich ausfüllt. Dadurch wird möglicherweise das Bild verzerrt, wenn der gezeichnete Bereich ein anderes Seitenverhältnis als das Bild aufweist. Sie können dieses Verhalten ändern, indem Sie die [**Stretch**](/uwp/api/windows.ui.xaml.media.tilebrush.stretch)-Eigenschaft vom Standardwert **Fill** in **None**, **Uniform** oder **UniformToFill** ändern.

Im nächsten Beispiel wird ein [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) erstellt und [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) auf das Bild „licorice.jpg“ festgelegt, das Sie als Ressource in die App aufnehmen müssen. Der **ImageBrush** zeichnet dann den von einer [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)-Form definierten Bereich.

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

![Ein gerenderter ImageBrush](images/brushes-imagebrush.jpg)

*Ein gerenderter ImageBrush*

[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) und [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) verweisen beide mit einem URI (Uniform Resource Identifier) auf eine Bildquelldatei. Die Bildquelldatei liegt dabei in verschiedenen Formaten vor. Diese Bildquelldateien werden als URIs angegeben. Weitere Informationen zum Angeben von Bildquellen, zu den verwendbaren Bildformaten und zum Packen dieser Dateien in einer App finden Sie unter [„Image“ und „ImageBrush“](../controls-and-patterns/images-imagebrushes.md).

## <a name="brushes-and-text"></a>Pinsel und Text

Sie können mit Pinseln auch Renderingmerkmale auf Textelemente anwenden. Die [**Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground)-Eigenschaft von [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) akzeptiert z. B. einen [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Sie können alle hier beschriebenen Pinsel auf Text anwenden. Gehen Sie jedoch beim Anwenden von Pinseln auf Text mit Bedacht vor, da der Text möglicherweise nicht lesbar ist, wenn Sie Pinsel verwenden, die in den Hintergrund auslaufen. Verwenden Sie den [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush), der i. d. R. keine negativen Auswirkungen auf die Lesbarkeit hat. Es sei denn, Sie möchten das Textelement besonders dekorativ gestalten.

Achten Sie auch bei Volltonfarben immer darauf, dass die ausgewählte Textfarbe einen ausreichenden Kontrast zur Hintergrundfarbe des Layoutcontainers für den Text aufweist. Die Stärke des Kontrasts zwischen Textvordergrund und Textcontainer sollte im Hinblick auf die Barrierefreiheit festgelegt werden.

## <a name="webviewbrush"></a>WebViewBrush

Bei einem [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) handelt es sich um einen besonderen Pinseltyp, mit dem auf die Inhalte zugegriffen werden kann, die normalerweise in einem [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView)-Steuerelement angezeigt werden. Anstatt den Inhalt im rechteckigen **WebView**-Steuerelementbereich zu rendern, zeichnet **WebViewBrush** diesen Inhalt in ein anderes Element mit einer [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Typeigenschaft für eine Renderingoberfläche. **WebViewBrush** ist nicht für jedes Pinselszenario geeignet, kann aber für Übergänge einer **WebView** hilfreich sein. Weitere Informationen finden Sie unter [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) ist eine Basisklasse zum Erstellen benutzerdefinierter Pinsel, die [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) verwenden, um XAML-UI Elemente zu zeichnen.

Dies ermöglicht die „Dropdown”-Interoperabilität zwischen den Ebenen „Windows.UI.Xaml“ und „Windows.UI.Composition“, wie in der [**Übersicht über die visuelle Ebene**](../../composition/visual-layer.md) beschrieben. 

Zum Erstellen eines benutzerdefinierten Pinsels erstellen Sie eine neue Klasse, die von „XamlCompositionBrushBase“ erbt und die erforderlichen Methoden implementiert.

Dies kann beispielsweise zum Anwenden von [**Effekten**](/uwp/composition/composition-effects) auf XAML-UI-Elemente verwendet werden, wobei ein [**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush) verwendet wird, z. B. ein **GaussianBlurEffect** oder ein [**SceneLightingEffect**](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect), der die reflektierenden Eigenschaften eines XAML-UI-Elements steuert, wenn es durch ein [**XamlLight**](/uwp/api/windows.ui.xaml.media.xamllight) beleuchtet wird.

Codebeispiele finden Sie unter [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="brushes-as-xaml-resources"></a>Pinsel als XAML-Ressourcen

Sie können jeden Pinsel als XAML-Ressource mit Schlüssel in einem XAML-Ressourcenwörterbuch deklarieren. So können die gleichen Pinselwerte bequem dupliziert und auf eine Vielzahl von Elementen auf einer Benutzeroberfläche angewendet werden. Die Pinselwerte werden dann freigegeben und in allen Fällen angewendet, in denen Sie auf die Pinselressource mit einer [{StaticResource}](/uwp/xaml-platform/staticresource-markup-extension)-Verwendung im XAML verweisen. Hierzu gehören auch Fälle, in denen mit einer XAML-Steuerelementvorlage auf den freigegebenen Pinsel verwiesen wird und es sich bei der Steuerelementvorlage selbst um eine XAML-Ressource mit Schlüssel handelt.

## <a name="brushes-in-code"></a>Pinsel in Code

Pinsel werden in der Regel mit XAML und nicht mit Code angegeben. Das liegt daran, dass Pinsel normalerweise als XAML-Ressourcen definiert und Pinselwerte häufig von Entwicklungstools ausgegeben werden oder Teil einer XAML-Benutzeroberflächendefinition sind. Für die Gelegenheiten, bei denen Sie einen Pinsel mithilfe von Code definieren möchten, stehen weiterhin alle [**Pinsel**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Typen für die Codeinstanziierung zur Verfügung.

Verwenden Sie zum Erstellen eines [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)-Elements in Code den Konstruktor, der einen [**Color**](/uwp/api/Windows.UI.Color)-Parameter akzeptiert. Übergeben Sie einen Wert, bei dem es sich um eine statische Eigenschaft der [**Colors**](/uwp/api/windows.ui.colors)-Klasse handelt:

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

Verwenden Sie für [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) und [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) den Standardkonstruktor, und rufen Sie dann andere APIs auf, bevor Sie diesen Pinsel für eine UI-Eigenschaft verwenden.

-   Für [**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty) ist ein [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (kein URI) erforderlich, wenn Sie einen [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) mithilfe von Code definieren. Falls es sich bei Ihrer Quelle um einen Datenstrom handelt, initialisieren Sie den Wert mit der [**SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync)-Methode. Ist Ihre Quelle ein URI (wozu auch App-Inhalte gehören, die das Schema **ms-appx** oder **ms-resource** verwenden), verwenden Sie den [**BitmapImage**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage)-Konstruktor, der einen URI akzeptiert. Für den Fall, dass beim Abrufen oder Decodieren der Bildquelle Probleme mit der Zeitsteuerung auftreten und Sie den alternativen Inhalt anzeigen müssen, bis die Bildquelle verfügbar ist, empfiehlt es sich unter Umständen auch, das [**ImageOpened**](/uwp/api/windows.ui.xaml.media.imagebrush.imageopened)-Ereignis zu behandeln.
-   Für [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) müssen Sie möglicherweise [**Redraw**](/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw) aufrufen, wenn Sie kürzlich die [**SourceName**](/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename)-Eigenschaft zurückgesetzt haben oder der Inhalt von [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) ebenfalls mittels Code geändert wird.

Codebeispiele finden Sie unter [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) und [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).
 

 