---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Verwenden von Pinseln
description: Mit Brush-Objekten werden Innenbereiche oder Ränder von Formen, Text und Teilen von Steuerelementen gezeichnet, damit das gezeichnete Objekt auf einer Benutzeroberfläche sichtbar ist.
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6e5f8dfc780b50e70f92fc388a04258ce7be11a4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366841"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>Verwenden von Pinseln zum Zeichnen von Vor- und Hintergründen und Skizzen

Die [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Objekte werden verwendet, um Innenbereiche oder Konturen von XAML-Formen, Texten und Teilen von Steuerelementen zu zeichnen, damit das gezeichnete Objekt auf einer Benutzeroberfläche sichtbar ist. Machen Sie sich im Folgenden mit den verfügbaren Pinseln und deren Verwendung vertraut.

> **Wichtige APIs:**  [Brush-Klasse](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>Einführung in Pinsel

Verwenden Sie für das Zeichnen von Objekten wie einer [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) oder Teilen eines [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control), das auf der App-Canvas angezeigt wird, einen [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Sie können z. B. die [**Fill**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill)-Eigenschaft der **Shape** oder die [**Background**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background)-Eigenschaft und die [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground)-Eigenschaft eines **Control** auf einen **Brush**-Wert festlegen. Dieser **Brush** bestimmt, wie das UI-Element in der UI gezeichnet oder gerendert wird. 

Es gibt folgende Pinseltypen: 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>Einfarbige Pinsel

Ein [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) zeichnet einen Bereich mit einer einzelnen [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), z. B. Rot oder Blau. Dies ist der einfachste Pinsel. Es gibt drei Möglichkeiten, in XAML einen **SolidColorBrush** und die zugehörige Volltonfarbe zu definieren: vordefinierte Farbnamen, hexadezimale Farbwerte und die Syntax für Eigenschaftselemente.

### <a name="predefined-color-names"></a>Vordefinierte Farbnamen

Sie können einen vordefinierten Farbnamen wie [**Yellow**](https://docs.microsoft.com/uwp/api/windows.ui.colors.yellow) oder [**Magenta**](https://docs.microsoft.com/uwp/api/windows.ui.colors.magenta) verwenden. Es stehen 256 benannte Farben zur Verfügung. Der XAML-Parser wandelt den Farbnamen in eine [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color)-Struktur mit den richtigen Farbkanälen um. Die benannten Farben vom 256 basieren auf der *X11* Farbnamen von Cascading Style Sheets, Level 3 (CSS3)-Spezifikation, sodass Sie möglicherweise bereits vertraut sind, mit dieser Liste nannte Farben aus, wenn Sie bereits Erfahrung mit Web-Entwicklung haben oder Entwurf.

In diesem Beispiel wird die [**Fill**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill)-Eigenschaft eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) auf die vordefinierte Farbe [**Red**](https://docs.microsoft.com/uwp/api/windows.ui.colors.red) festgelegt.

```xml
<Rectangle Width="100" Height="100" Fill="Red" />
```

Die folgende Abbildung zeigt den [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush), der auf das [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) angewendet wurde.

![Ein gerenderter SolidColorBrush](images/brushes-solidcolorbrush.jpg)

Wenn Sie einen [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) mit Code anstatt mit XAML definieren, sind die einzelnen benannten Farben als statische Eigenschaftswerte der [**Colors**](https://docs.microsoft.com/uwp/api/windows.ui.colors)-Klasse verfügbar. Wenn Sie z. B. einen [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.solidcolorbrush.color)-Wert eines **SolidColorBrush** zum Darstellen der benannten Farbe „Orchid“ deklarieren möchten, legen Sie den **Color**-Wert auf den statischen Wert [**Colors.Orchid**](https://docs.microsoft.com/uwp/api/windows.ui.colors.orchid) fest.

### <a name="hexadecimal-color-values"></a>Hexadezimale Farbwerte

Sie können eine Zeichenfolge im Hexadezimalformat verwenden, um präzise 24-Bit-Farbwerte mit 8-Bit-Alphakanal für einen [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) zu deklarieren. Zwei Zeichen im Bereich 0 bis F definieren die einzelnen Komponentenwerte. Die Reihenfolge der Komponentenwerte für die Hexadezimalzeichenfolge lautet wie folgt: Alphakanal (Deckkraft), roter Kanal, grüner Kanal und blauer Kanal (**ARGB**). Beispielsweise den hexadezimalen Wert "\#FFFF0000" vollständig deckendes Rot definiert (Alpha = "FF", Rot = "FF", Grün = "00", und die blaue = "00").

In diesem XAML-Beispiel wird die [ **füllen** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill) Eigenschaft eine [ **Rechteck** ](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) auf den hexadezimalen Wert "\#FFFF0000", und bietet das gleiche Ergebnis wie mit die benannte Farbe [ **Colors.Red**](https://docs.microsoft.com/uwp/api/windows.ui.colors.red).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="span-idpropertyelementsyntaxspanspan-idpropertyelementsyntaxspanspan-idpropertyelementsyntaxspanproperty-element-syntax"></a><span id="Property_element_syntax__"></span><span id="property_element_syntax__"></span><span id="PROPERTY_ELEMENT_SYNTAX__"></span>Eigenschaftenelement-syntax

Mithilfe der Syntax für Eigenschaftselemente können Sie einen [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) definieren. Diese Syntax ist ausführlicher als die zuvor gezeigten Methoden, und Sie können zusätzliche Eigenschaftswerte für ein Element angeben, wie z. B. die [**Opacity**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.opacity) des Pinsels. Weitere Informationen zur XAML-Syntax, einschließlich der Syntax für Eigenschaftselemente, finden Sie unter [XAML-Übersicht](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview) und [Anleitung zur XAML-Syntax](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-syntax-guide).

In den vorhergehenden Beispielen kam die Zeichenfolge „SolidColorBrush“ in der Syntax nicht vor. Der erstellte Pinsel wird implizit und automatisch erstellt und ist Teil einer durchdachten Kurzschrift der XAML-Sprache, mit der die UI-Definition für die am häufigsten Anwendungsfälle unkompliziert gehalten wird. Im nächsten Beispiel werden ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) und dann explizit der [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) als Elementwert für eine [**Rectangle.Fill**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill)-Eigenschaft erstellt. Die [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) von **SolidColorBrush** wird auf [**Blue**](https://docs.microsoft.com/uwp/api/windows.ui.colors.blue) und die [**Opacity**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.opacity) auf 0,5 festgelegt.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="span-idlineargradientbrushesspanspan-idlineargradientbrushesspanspan-idlineargradientbrushesspanlinear-gradient-brushes"></a><span id="Linear_gradient_brushes_"></span><span id="linear_gradient_brushes_"></span><span id="LINEAR_GRADIENT_BRUSHES_"></span>Linearer Farbverlaufspinsel

Ein [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) zeichnet einen Bereich mit einem Farbverlauf entlang einer Linie. Diese Linie wird als *Farbverlaufsachse* bezeichnet. Sie geben die Farben für den Farbverlauf und ihre Position an der Farbverlaufsachse mithilfe von [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop)-Objekten an. Standardmäßig verläuft die Farbverlaufsachse von der oberen linken Ecke zur unteren rechte Ecke des Bereichs, den der Pinsel zeichnet. Dies führt zu einer diagonalen Schattierung.

Der [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop) bildet die Grundlage für einen Farbverlaufspinsel. Ein Farbverlaufsstopp gibt die [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.color) des Pinsels an einem [**Offset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.offset) entlang der Farbverlaufsachse an, wenn der Pinsel auf den zu zeichnenden Bereich angewendet wird.

Die [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.color)-Eigenschaft des Farbverlaufsstopps gibt die Farbe am Farbverlaufsstopp an. Sie können diese Farbe mit einem vordefinierten Farbnamen oder mit hexadezimalen **ARGB**-Werten angeben.

Die [**Offset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.offset)-Eigenschaft eines [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop) gibt die Position für jeden **GradientStop** entlang der Farbverlaufsachse an. Der **Offset** ist ein **double**-Wert zwischen 0 und 1. Mit einem**Offset** von 0 wird der **GradientStop** am Anfang der Farbverlaufsachse platziert, also in der Nähe des [**StartPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint). Ein **Offset** von 1 platziert den **GradientStop** am [**EndPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint). Ein sinnvoller [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) sollte mindestens über zwei **GradientStop**-Werte verfügen. Dabei sollte jeder **GradientStop** eine andere [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.color) und einen anderen **Offset** zwischen 0 und 1 aufweisen.

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

Die Farbe der einzelnen Punkte zwischen den Farbverlaufsstopps wird linear als eine Kombination der von den beiden umgebenden Farbverlaufsstopps angegebenen Farben interpoliert. In der Abbildung sind die Farbverlaufsstopps aus dem vorherigen Beispiel hervorgehoben. Die Kreise kennzeichnen die Position der Farbverlaufsstopps, und die gestrichelten Linie zeigt die Farbverlaufsachse.

![Farbverlaufsstopps](images/linear-gradients-stops.png) können Sie die Zeile, an dem die Farbverlaufsstopps werden durch Festlegen von positioniert, Ändern der [ **"StartPoint"** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) und [ **Endpunkt** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) Eigenschaften werden andere Werte als die `(0,0)` und `(1,1)` Standardwerte ab. Durch Ändern der Koordinatenwerte für **StartPoint** und **EndPoint** können Sie horizontale oder vertikale Farbverläufe erstellen, die Richtung des Farbverlaufs umkehren oder die Farbverlaufsausdehnung verkleinern und so auf einen kleineren Bereich als den gesamten gezeichneten Bereich anwenden. Zum Verkleinern des Farbverlaufs legen Sie die Werte für **StartPoint** und/oder **EndPoint** auf einen Wert zwischen 0 und 1 fest. Angenommen, Sie möchten einen horizontalen Farbverlauf erstellen, bei dem der Farbverlauf nur in der linken Hälfte des Pinsels erfolgt und die rechte Hälfte nur die letzte [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop)-Farbe hat. Hierfür geben Sie einen **StartPoint** von `(0,0)` und einen **EndPoint** von `(0.5,0)` an.

### <a name="span-idusetoolstomakegradientsspanspan-idusetoolstomakegradientsspanspan-idusetoolstomakegradientsspanuse-tools-to-make-gradients"></a><span id="Use_tools_to_make_gradients"></span><span id="use_tools_to_make_gradients"></span><span id="USE_TOOLS_TO_MAKE_GRADIENTS"></span>Verwenden der Tools für Farbverläufe zu machen

Nachdem Sie nun wissen, wie lineare Farbverläufe funktionieren, können Sie Visual Studio oder Blend nutzen, um diese Farbverläufe einfacher erstellen zu können. Um einen Farbverlauf zu erstellen, wählen Sie in der Entwurfsoberfläche oder in der XAML-Ansicht das Objekt aus, auf das ein Farbverlauf angewendet werden soll. Erweitern Sie **Pinsel**, und wählen Sie die Registerkarte **Linearer Farbverlauf** aus (siehe nächstes Bildschirmfoto).

![Erstellen Sie einen linearen Farbverlauf mithilfe von Visual Studio.](images/tool-gradient-brush-1.png)

Jetzt können Sie die Farben der Farbverlaufsstopps ändern und ihre Positionen unter Verwendung der Leiste am unteren Rand verschieben. Sie können auch neue Farbverlaufsstopps hinzufügen, indem Sie auf die Leiste klicken, und Farbverlaufsstopps entfernen, indem Sie sie von der Leiste herunter ziehen (siehe nächstes Bildschirmfoto).

![Die Leiste am unteren Rand des Eigenschaftenfensters, die zur Steuerung der Farbverlaufsstopps dient.](images/tool-gradient-brush-2.png)

## <a name="span-idimagebrushesspanspan-idimagebrushesspanspan-idimagebrushesspanimage-brushes"></a><span id="Image_brushes"></span><span id="image_brushes"></span><span id="IMAGE_BRUSHES"></span>Bildpinsel

Mit einem [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) wird in einem Bereich ein Bild gezeichnet. Dabei stammt das zu zeichnende Bild aus einer Bilddateiquelle. Legen Sie die [**ImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageSource)-Eigenschaft auf den Pfad des Bilds fest, das Sie laden möchten. Normalerweise stammt die Bildquelle aus einem **Content**-Element, das Teil der App-Ressourcen ist.

Ein [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) streckt das Bild standardmäßig so weit, dass es den gezeichneten Bereich ausfüllt. Dadurch wird möglicherweise das Bild verzerrt, wenn der gezeichnete Bereich ein anderes Seitenverhältnis als das Bild aufweist. Sie können dieses Verhalten ändern, indem Sie die [**Stretch**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.tilebrush.stretch)-Eigenschaft vom Standardwert **Fill** in **None**, **Uniform** oder **UniformToFill** ändern.

Im nächsten Beispiel wird ein [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) erstellt und [**ImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageSource) auf das Bild „licorice.jpg“ festgelegt, das Sie als Ressource in die App aufnehmen müssen. Der **ImageBrush** zeichnet dann den von einer [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)-Form definierten Bereich.

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

So sieht der gerenderte [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) aus.

![Ein gerenderter ImageBrush](images/brushes-imagebrush.jpg)

[ **"ImageBrush"** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) und [ **Image** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) Verweisdaten eine Image-Quelldatei von Uniform Resource Identifier (URI), der Image-Quelldatei, in denen mehrere mögliche Bildformate verwendet. Diese Bildquelldateien werden als URIs angegeben. Weitere Informationen zum Angeben von Bildquellen, zu den verwendbaren Bildformaten und zum Packen dieser Dateien in einer App finden Sie unter [„Image“ und „ImageBrush“](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes).

## <a name="brushes-and-text"></a>Pinsel und Text

Sie können mit Pinseln auch Renderingmerkmale auf Textelemente anwenden. Die [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.foreground)-Eigenschaft von [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) akzeptiert z. B. einen [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Sie können alle hier beschriebenen Pinsel auf Text anwenden. Seien Sie jedoch beim Anwenden von Pinseln auf Text vorsichtig, da der Text unlesbar werden kann. Dies ist z. B. der Fall, wenn die verwendeten Pinsel mit dem Hintergrund verlaufen, über dem der Text gerendert wird, oder von den Konturen der Textzeichen ablenken. Verwenden Sie den [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush), der i. d. R. keine negativen Auswirkungen auf die Lesbarkeit hat. Es sei denn, Sie möchten das Textelement besonders dekorativ gestalten.

Achten Sie auch bei Volltonfarben immer darauf, dass die ausgewählte Textfarbe einen ausreichenden Kontrast zur Hintergrundfarbe des Layoutcontainers für den Text aufweist. Die Stärke des Kontrasts zwischen Textvordergrund und Textcontainer sollte im Hinblick auf die Barrierefreiheit festgelegt werden.

## <a name="webviewbrush"></a>WebViewBrush

Bei einem [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) handelt es sich um einen besonderen Pinseltyp, mit dem auf die Inhalte zugegriffen werden kann, die normalerweise in einem [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)-Steuerelement angezeigt werden. Anstatt den Inhalt im rechteckigen **WebView**-Steuerelementbereich zu rendern, zeichnet **WebViewBrush** diesen Inhalt in ein anderes Element mit einer [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Typeigenschaft für eine Renderingoberfläche. **WebViewBrush** ist nicht für jedes Pinselszenario geeignet, kann aber für Übergänge einer **WebView** hilfreich sein. Weitere Informationen finden Sie unter [**WebViewBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase** ](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) ist eine Basisklasse, die zum Erstellen von benutzerdefinierten Pinseln mit [ **CompositionBrush** ](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.CompositionBrush) XAML UI Elemente gezeichnet.

Dies ermöglicht die „Dropdown”-Interoperabilität zwischen den Ebenen Windows.UI.Xaml und Windows.UI.Composition, wie in der [**Übersicht über die visuelle Ebene**](/windows/uwp/composition/visual-layer) beschrieben. 

Um einen benutzerdefiniertes Pinsel zu erstellen, erstellen Sie eine neue Klasse, die von XamlCompositionBrushBase erbt und die erforderlichen Methoden implementiert.

Dies kann beispielsweise zum Anwenden von [**Effekten**](/windows/uwp/composition/composition-effects) auf XAML-UI-Elemente verwendet werden, wobei ein [**CompositionEffectBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.CompositionEffectBrush) verwendet wird, z. B. ein **GaussianBlurEffect** oder ein [**SceneLightingEffect**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect), der die reflektierenden Eigenschaften eines XAML-UI-Elements steuert, wenn es durch ein [**XamlLight**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamllight) beleuchtet wird.

Codebeispiele finden Sie auf der Referenzseite für [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="brushes-as-xaml-resources"></a>Pinsel als XAML-Ressourcen

Sie können jeden Pinsel als XAML-Ressource mit Schlüssel in einem XAML-Ressourcenwörterbuch deklarieren. So können die gleichen Pinselwerte bequem dupliziert und auf eine Vielzahl von Elementen in einer UI angewendet werden. Die Pinselwerte werden dann freigegeben und in allen Fällen angewendet, in denen Sie auf die Pinselressource mit einer [{StaticResource}](https://docs.microsoft.com/windows/uwp/xaml-platform/staticresource-markup-extension)-Verwendung im XAML verweisen. Hierzu gehören auch Fälle, in denen mit einer XAML-Steuerelementvorlage auf den freigegebenen Pinsel verwiesen wird und es sich bei der Steuerelementvorlage selbst um eine XAML-Ressource mit Schlüssel handelt.

## <a name="brushes-in-code"></a>Pinsel in Code

Pinsel werden in der Regel mit XAML und nicht mit Code angegeben. Das liegt daran, dass Pinsel normalerweise als XAML-Ressourcen definiert und Pinselwerte häufig von Entwicklungstools ausgegeben werden oder Teil einer XAML-UI-Definition sind. Für die Gelegenheiten, bei denen Sie einen Pinsel mithilfe von Code definieren möchten, stehen weiterhin alle [**Pinsel**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Typen für die Codeinstanziierung zur Verfügung.

Verwenden Sie zum Erstellen eines [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)-Elements in Code den Konstruktor, der einen [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color)-Parameter akzeptiert. Übergeben Sie einen Wert, bei dem es sich um eine statische Eigenschaft der [**Colors**](https://docs.microsoft.com/uwp/api/windows.ui.colors)-Klasse handelt:

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

Verwenden Sie für [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) und [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) den Standardkonstruktor, und rufen Sie dann andere APIs auf, bevor Sie diesen Pinsel für eine UI-Eigenschaft verwenden.

-   [ **"ImageSource"** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty) erfordert eine [ **"BitmapImage"** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (keinen URI) beim Definieren einer [ **"ImageBrush"** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) mit Code. Falls es sich bei Ihrer Quelle um einen Datenstrom handelt, initialisieren Sie den Wert mit der [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync)-Methode. Ist Ihre Quelle ein (URI), der Ihrer App Inhalt mit dem **ms-appx**- oder dem **ms-resource**-Schema hinzufügt, verwenden Sie den [**BitmapImage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.)-Konstruktor, für den ein URI angegeben wird. Wenn beim Abrufen oder Decodieren der Bildquelle Probleme mit der Zeitsteuerung auftreten und Sie alternativen Inhalt anzeigen müssen, bis die Bildquelle verfügbar ist, können Sie auch das [**ImageOpened**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imageopened)-Ereignis behandeln.
-   Für [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) müssen Sie möglicherweise [**Redraw**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw) aufrufen, wenn Sie kürzlich die [**SourceName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename)-Eigenschaft zurückgesetzt haben oder der Inhalt von [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) ebenfalls mittels Code geändert wird.

Codebeispiele finden Sie auf den Referenzseiten für [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) und [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).
 

 




