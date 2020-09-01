---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Kompositionspinsel
description: Ein Pinsel zeichnet den Bereich eines Visual-Objekts mit der zugehörigen Ausgabe. Verschiedene Pinsel haben unterschiedliche Ausgabetypen.
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e3c0454b5596490631f32c3481675f3c70d4eb76
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175644"
---
# <a name="composition-brushes"></a>Kompositionspinsel
Alle Elemente, die auf dem Bildschirm aus einer UWP-Anwendung sichtbar sind, sind sichtbar, da Sie von einem Pinsel gezeichnet wurden. Mithilfe von Pinseln können Sie Benutzeroberflächen Objekte (UI) mit Inhalt von einfachen, voll Tonfarben bis hin zu Bildern oder Zeichnungen bis hin zu komplexen Effektketten zeichnen. In diesem Thema werden die Konzepte für das Zeichnen mit compositionbrush vorgestellt.

Beachten Sie, dass Sie bei der Arbeit mit der XAML-UWP-App ein UIElement mit einem [XAML-Pinsel](../design/style/brushes.md) oder einem [compositionbrush](/uwp/api/Windows.UI.Composition.CompositionBrush)zeichnen können. In der Regel ist es einfacher und ratsam, einen XAML-Pinsel auszuwählen, wenn Ihr Szenario von einem XAML-Pinsel unterstützt wird. Beispielsweise das Animieren der Farbe einer Schaltfläche, das Ändern des Füllens eines Texts oder einer Form mit einem Bild. Wenn Sie jedoch versuchen, etwas zu tun, das von einem XAML-Pinsel wie z. b. mit einer animierten Maske oder einer animierten neun-Raster-Streckung oder einer Effekt Kette nicht unterstützt wird, können Sie ein UIElement mithilfe von [xamlcompositionbrushbase](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)zeichnen.

Beim Arbeiten mit der visuellen Ebene muss ein compositionbrush-Element verwendet werden, um den Bereich eines [spritevisual](/uwp/api/Windows.UI.Composition.SpriteVisual)zu zeichnen.

-   [Voraussetzungen](./composition-brushes.md#prerequisites)
-   [Zeichnen mit compositionbrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Zeichnen mit einer voll Tonfarbe](./composition-brushes.md#paint-with-a-solid-color)
    -   [Zeichnen mit einem linearen Farbverlauf](./composition-brushes.md#paint-with-a-linear-gradient) 
    -   [Zeichnen mit einem radialen Farbverlauf](./composition-brushes.md#paint-with-a-radial-gradient)
    -   [Zeichnen mit einem Bild](./composition-brushes.md#paint-with-an-image)
    -   [Zeichnen mit einer benutzerdefinierten Zeichnung](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Zeichnen mit einem Video](./composition-brushes.md#paint-with-a-video)
    -   [Zeichnen mit einem Filtereffekt](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Zeichnen mit einem "compositionbrush" mit einer Deck Kraft Maske](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Zeichnen mit einem "compositionbrush" mithilfe von "NineGrid Stretch"](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Zeichnen mithilfe von Hintergrund Pixeln](./composition-brushes.md#paint-using-background-pixels)
-   [Kombinieren von compositionbrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Verwenden eines XAML-Pinsels im Vergleich zu compositionbrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Verwandte Themen](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Voraussetzungen
In dieser Übersicht wird davon ausgegangen, dass Sie mit der Struktur einer grundlegenden Kompositions Anwendung vertraut sind, wie in der [Übersicht über die visuelle Ebene](visual-layer.md)beschrieben.

## <a name="paint-with-a-compositionbrush"></a>Zeichnen mit einem compositionbrush

Ein [compositionbrush](/uwp/api/Windows.UI.Composition.CompositionBrush) "zeichnet" einen Bereich mit seiner Ausgabe. Verschiedene Pinsel haben unterschiedliche Ausgabetypen. Einige Pinsel zeichnen einen Bereich mit einer voll Tonfarbe, andere mit einem Farbverlauf, einem Bild, einer benutzerdefinierten Zeichnung oder einem Effekt. Es gibt auch spezialisierte Pinsel, die das Verhalten anderer Pinsel verändern. Beispielsweise kann die Deck Kraft Maske verwendet werden, um zu steuern, welcher Bereich von einem compositionbrush gezeichnet wird, oder ein neun Raster kann verwendet werden, um die Streckung zu steuern, die beim Zeichnen eines Bereichs auf einen compositionbrush angewendet wird. Compositionbrush kann einen der folgenden Typen aufweisen:

|Klasse                                   |Details                                         |Eingeführt in|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Zeichnet einen Bereich mit einer voll Tonfarbe.                        |Windows 10, Version 1511 (SDK 10586)|
|[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Zeichnet einen Bereich mit dem Inhalt einer [icompositionsurface](/uwp/api/Windows.UI.Composition.ICompositionSurface) -Schnittstelle.|Windows 10, Version 1511 (SDK 10586)|
|[Compositioneffectbrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Zeichnet einen Bereich mit dem Inhalt eines Kompositions Effekts. |Windows 10, Version 1511 (SDK 10586)|
|[Compositionmaskbrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Zeichnet ein visuelles Element mit einem compositionbrush-Element mit einer Deck Kraft Maske. |Windows 10, Version 1607 (SDK 14393)
|[Compositionninegridbrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Zeichnet einen Bereich mit einem compositionbrush mithilfe einer NineGrid-Streckung. |Windows 10, Version 1607 (SDK 14393)
|[Compositionlineargradientbrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Zeichnet einen Bereich mit einem linearen Farbverlauf                    |Windows 10, Version 1709 (SDK 16299)
|[CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush)|Zeichnet einen Bereich mit einem radialen Farbverlauf                    |Windows 10, Version 1903 (Insider-Vorschau-SDK)
|[Compositionbackdropbrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Zeichnet einen Bereich, indem Hintergrund Pixel entweder aus der Anwendung oder aus Pixel direkt hinter dem Fenster der Anwendung auf dem Desktop entnommen werden. Wird als Eingabe für einen anderen compositionbrush-Befehl wie compositioneffectbrush verwendet. | Windows 10, Version 1607 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Zeichnen mit einer voll Tonfarbe

Ein [compositioncolorbrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush) zeichnet einen Bereich mit einer voll Tonfarbe. Es gibt eine Vielzahl von Möglichkeiten, die Farbe von SolidColorBrush anzugeben. Sie können z. b. die Alpha-, rot-, blau-und Grün-Kanäle (ARGB) angeben oder eine der vordefinierten Farben verwenden, die von der [Colors](/uwp/api/windows.ui.colors) -Klasse bereitgestellt werden.

Die Abbildung und der Code zeigen im Folgenden eine kleine visuelle Struktur. Es wird ein Rechteck erstellt, dessen Konturen mit einem schwarzen Pinsel gezeichnet sind und das mit einem Pinsel in Volltonfarbe ausgefüllt ist, die den Farbwert „0x9ACD32“ hat.

![CompositionColorBrush](images/composition-compositioncolorbrush.png)

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual _colorVisual1, _colorVisual2;
CompositionColorBrush _blackBrush, _greenBrush;

_compositor = Window.Current.Compositor;
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
_colorVisual1= _compositor.CreateSpriteVisual();
_colorVisual1.Brush = _blackBrush;
_colorVisual1.Size = new Vector2(156, 156);
_colorVisual1.Offset = new Vector3(0, 0, 0);
_container.Children.InsertAtBottom(_colorVisual1);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
_colorVisual2 = _compositor.CreateSpriteVisual();
_colorVisual2.Brush = _greenBrush;
_colorVisual2.Size = new Vector2(150, 150);
_colorVisual2.Offset = new Vector3(3, 3, 0);
_container.Children.InsertAtBottom(_colorVisual2);
```

### <a name="paint-with-a-linear-gradient"></a>Zeichnen mit einem linearen Farbverlauf

Ein [compositionlineargradientbrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush) zeichnet einen Bereich mit einem linearen Farbverlauf. Ein linearer Farbverlauf kombiniert mindestens zwei Farben in einer Linie, der Farbverlaufs Achse. Sie verwenden gradientstoppt-Objekte, um die Farben im Farbverlauf und deren Positionen anzugeben.

In der folgenden Abbildung und im folgenden Code wird ein spritevisual dargestellt, das mit einem LinearGradientBrush mit 2 Stopps und einer roten und gelben Farbe gezeichnet ist.

![Compositionlineargradientbrush](images/composition-compositionlineargradientbrush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionLinearGradientBrush _redyellowBrush;

_compositor = Window.Current.Compositor;

_redyellowBrush = _compositor.CreateLinearGradientBrush();
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Red));
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.Yellow));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = _redyellowBrush;
_gradientVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-radial-gradient"></a>Zeichnen mit einem radialen Farbverlauf

Ein [compositionradialgradientbrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush) zeichnet einen Bereich mit einem radialen Farbverlauf. Ein Radialer Farbverlauf kombiniert zwei oder mehr Farben mit dem Farbverlauf beginnend mit der Mitte der Ellipse und endet mit dem Radius der Ellipse. Gradientstopps-Objekte werden verwendet, um die Farben und deren Position im Farbverlauf zu definieren.

Die folgende Abbildung und der Code zeigt ein spritevisual, das mit einem RadialGradientBrush mit 2 gradientstopps gezeichnet wurde.

![CompositionRadialGradientBrush](images/radial-gradient-brush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionRadialGradientBrush RGBrush;

_compositor = Window.Current.Compositor;

RGBrush = _compositor.CreateRadialGradientBrush();
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Aquamarine));
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.DeepPink));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = RGBrush;
_gradientVisual.Size = new Vector2(200, 200);
```

### <a name="paint-with-an-image"></a>Zeichnen mit einem Bild

Mit einem [compositionsurfacebrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) -Element wird ein Bereich gezeichnet, und auf einer icompositionsurface gerenderte Pixel. Beispielsweise kann ein compositionsurfacebrush verwendet werden, um einen Bereich mit einem Bild zu zeichnen, das mithilfe der [loadedimagesurface](/uwp/api/windows.ui.xaml.media.loadedimagesurface) -API auf einer icompositionsurface-Oberfläche gerendert wird.

In der folgenden Abbildung und im Code wird ein spritevisual gezeigt, das mit einer Bitmap einer Licorice gezeichnet wurde, die auf einem icompositionsurface mithilfe von loadedimagesurface gerendert wird. Die Eigenschaften von compositionsurfacebrush können verwendet werden, um die Bitmap innerhalb der Grenzen des visuellen Elements zu Strecken und auszurichten.

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)

```cs
Compositor _compositor;
SpriteVisual _imageVisual;
CompositionSurfaceBrush _imageBrush;

_compositor = Window.Current.Compositor;

_imageBrush = _compositor.CreateSurfaceBrush();

// The loadedSurface has a size of 0x0 till the image has been been downloaded, decoded and loaded to the surface. We can assign the surface to the CompositionSurfaceBrush and it will show up once the image is loaded to the surface.
LoadedImageSurface _loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_imageBrush.Surface = _loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _imageBrush;
_imageVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-custom-drawing"></a>Zeichnen mit einer benutzerdefinierten Zeichnung
Ein [compositionsurfacebrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) kann auch verwendet werden, um einen Bereich mit Pixel aus einer icompositionsurface zu zeichnen, die mithilfe von [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm) (oder D2D) gerendert wird.

Der folgende Code zeigt ein spritevisual-Element, das mit einer auf icompositionsurface gerenderten Textzeile mithilfe von Win2D gezeichnet wird. Beachten Sie, dass Sie das [Win2D nuget](https://www.nuget.org/packages/Win2D.uwp) -Paket in Ihr Projekt einschließen müssen, um Win2D verwenden zu können.

```cs
Compositor _compositor;
CanvasDevice _device;
CompositionGraphicsDevice _compositionGraphicsDevice;
SpriteVisual _drawingVisual;
CompositionSurfaceBrush _drawingBrush;

_device = CanvasDevice.GetSharedDevice();
_compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(_compositor, _device);

_drawingBrush = _compositor.CreateSurfaceBrush();
CompositionDrawingSurface _drawingSurface = _compositionGraphicsDevice.CreateDrawingSurface(new Size(256, 256), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied);

using (var ds = CanvasComposition.CreateDrawingSession(_drawingSurface))
{
     ds.Clear(Colors.Transparent);
     var rect = new Rect(new Point(2, 2), (_drawingSurface.Size.ToVector2() - new Vector2(4, 4)).ToSize());
     ds.FillRoundedRectangle(rect, 15, 15, Colors.LightBlue);
     ds.DrawRoundedRectangle(rect, 15, 15, Colors.Gray, 2);
     ds.DrawText("This is a composition drawing surface", rect, Colors.Black, new CanvasTextFormat()
     {
          FontFamily = "Comic Sans MS",
          FontSize = 32,
          WordWrapping = CanvasWordWrapping.WholeWord,
          VerticalAlignment = CanvasVerticalAlignment.Center,
          HorizontalAlignment = CanvasHorizontalAlignment.Center
     }
);

_drawingBrush.Surface = _drawingSurface;

_drawingVisual = _compositor.CreateSpriteVisual();
_drawingVisual.Brush = _drawingBrush;
_drawingVisual.Size = new Vector2(156, 156);
```

Ebenso kann der compositionsurfacebrush auch verwendet werden, um ein spritevisual mithilfe von Win2D-Interop mit einer SwapChain zu zeichnen. [Dieses](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) Beispiel enthält ein Beispiel für die Verwendung von Win2D zum Zeichnen eines spritevisual mit einem SwapChain.

### <a name="paint-with-a-video"></a>Zeichnen mit einem Video
Ein [compositionsurfacebrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) kann auch verwendet werden, um einen Bereich mit Pixel aus einer icompositionsurface zu zeichnen, die mithilfe eines Videos gerendert wird, das über die [Media Player](/uwp/api/Windows.Media.Playback.MediaPlayer) -Klasse geladen wurde.

Der folgende Code zeigt ein spritevisual, das mit einem Video gezeichnet wird, das auf einem icompositionsurface-Element geladen wurde.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("https://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
var item = new MediaPlaybackItem(source);
_mediaPlayer.Source = item;
_mediaPlayer.IsLoopingEnabled = true;

// Get the surface from MediaPlayer and put it on a brush
_videoSurface = _mediaPlayer.GetSurface(_compositor);
_videoBrush = _compositor.CreateSurfaceBrush(_videoSurface.CompositionSurface);

_videoVisual = _compositor.CreateSpriteVisual();
_videoVisual.Brush = _videoBrush;
_videoVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-filter-effect"></a>Zeichnen mit einem Filtereffekt

Ein [compositioneffectbrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush) zeichnet einen Bereich mit der Ausgabe eines compositioneffect. Die Auswirkungen in der visuellen Schicht können sich auf eine Auflistung von Quell Inhalten, wie Farben, Farbverläufe, Bilder, Videos, SwapChain, Bereiche Ihrer Benutzeroberfläche oder Struktur von visuellen Elementen, auswirken. Der Quell Inhalt wird in der Regel mit einem anderen compositionbrush-Wert angegeben.

In der folgenden Abbildung und im Code wird ein spritevisual gezeigt, das mit einem Bild einer Cat gezeichnet wurde, für die der Filtereffekt der desättigung übernommen wurde.

![Compositioneffectbrush](images/composition-cat-desaturated.png)

```cs
Compositor _compositor;
SpriteVisual _effectVisual;
CompositionEffectBrush _effectBrush;

_compositor = Window.Current.Compositor;

var graphicsEffect = new SaturationEffect {
                              Saturation = 0.0f,
                              Source = new CompositionEffectSourceParameter("mySource")
                         };

var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
_effectBrush = effectFactory.CreateBrush();

CompositionSurfaceBrush surfaceBrush =_compositor.CreateSurfaceBrush();
LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/cat.jpg"));
SurfaceBrush.surface = loadedSurface;

_effectBrush.SetSourceParameter("mySource", surfaceBrush);

_effectVisual = _compositor.CreateSpriteVisual();
_effectVisual.Brush = _effectBrush;
_effectVisual.Size = new Vector2(156, 156);
```

Weitere Informationen zum Erstellen eines Effekts mithilfe von compositionbrushes finden Sie unter [Effekte in der visuellen Ebene](./composition-effects.md) .

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Zeichnen mit einem compositionbrush mit angewendetem Deck Kraft Maske

Ein [compositionmaskbrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush) zeichnet einen Bereich mit einem compositionbrush, auf den eine Deck Kraft Maske angewendet wird. Die Quelle der Deck Kraft Maske kann ein beliebiger compositionbrush des Typs "compositioncolorbrush", "compositionlineargradientbrush", "compositionsurfacebrush", "compositioneffectbrush" oder "compositionninegridbrush" sein. Die Deckkraft Maske muss als compositionsurfacebrush angegeben werden.

Die folgende Abbildung und der Code zeigt ein spritevisual, das mit einem compositionmaskbrush gezeichnet wurde. Die Quelle der Maske ist ein compositionlineargradientbrush, der so maskiert wird, dass er wie ein Kreis aussieht, wobei ein Bild des Kreises als Maske verwendet wird.

![Compositionmaskbrush](images/composition-compositionmaskbrush.png)

```cs
Compositor _compositor;
SpriteVisual _maskVisual;
CompositionMaskBrush _maskBrush;

_compositor = Window.Current.Compositor;

_maskBrush = _compositor.CreateMaskBrush();

CompositionLinearGradientBrush _sourceGradient = _compositor.CreateLinearGradientBrush();
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(0,Colors.Red));
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(1,Colors.Yellow));
_maskBrush.Source = _sourceGradient;

LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/circle.png"), new Size(156.0, 156.0));
_maskBrush.Mask = _compositor.CreateSurfaceBrush(loadedSurface);

_maskVisual = _compositor.CreateSpriteVisual();
_maskVisual.Brush = _maskBrush;
_maskVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Zeichnen mit einem "compositionbrush" mithilfe von "NineGrid Stretch"

Ein [compositionninegridbrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) -Wert zeichnet einen Bereich mit einem compositionbrush, der mithilfe der neun-Raster-Metapher gestreckt wird. Die neun Raster-Metapher ermöglicht es Ihnen, Ränder und Ecken eines compositionbrush anders als in der Mitte zu Strecken. Die Quelle der neun Raster Streckung kann von einem beliebigen compositionbrush des Typs compositioncolorbrush, compositionsurfacebrush oder compositioneffectbrush verwendet werden.

Der folgende Code zeigt ein spritevisual, das mit einem compositionninegridbrush gezeichnet wurde. Die Quelle der Maske ist ein compositionsurfacebrush, das mit einem neun Raster gestreckt wird.

```cs
Compositor _compositor;
SpriteVisual _nineGridVisual;
CompositionNineGridBrush _nineGridBrush;

_compositor = Window.Current.Compositor;

_ninegridBrush = _compositor.CreateNineGridBrush();

// nineGridImage.png is 50x50 pixels; nine-grid insets, as measured relative to the actual size of the image, are: left = 1, top = 5, right = 10, bottom = 20 (in pixels)

LoadedImageSurface _imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/nineGridImage.png"));
_nineGridBrush.Source = _compositor.CreateSurfaceBrush(_imageSurface);

// set Nine-Grid Insets

_ninegridBrush.SetInsets(1, 5, 10, 20);

// set appropriate Stretch on SurfaceBrush for Center of Nine-Grid

sourceBrush.Stretch = CompositionStretch.Fill;

_nineGridVisual = _compositor.CreateSpriteVisual();
_nineGridVisual.Brush = _ninegridBrush;
_nineGridVisual.Size = new Vector2(100, 75);
```

### <a name="paint-using-background-pixels"></a>Zeichnen mithilfe von Hintergrund Pixeln

Ein [compositionbackdropbrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)  zeichnet einen Bereich mit dem Inhalt hinter dem Bereich. Ein compositionbackdropbrush wird nie eigenständig verwendet, sondern als Eingabe für einen anderen compositionbrush-Wert wie ein effectbrush. Wenn Sie z. b. einen compositionbackdropbrush als Eingabe für einen Weichzeichnereffekt verwenden, können Sie einen satiniertem Glass-Effekt erzielen.

Der folgende Code zeigt eine kleine visuelle Struktur zum Erstellen eines Bilds mit compositionsurfacebrush und einem satiniertem Glass-Overlay oberhalb des Bilds. Das satiniertem Glass-Overlay wird erstellt, indem ein spritevisual mit einem effectbrush über dem Bild platziert wird. Effectbrush verwendet einen compositionbackdropbrush als Eingabe für den Weichzeichnereffekt.

```cs
Compositor _compositor;
SpriteVisual _containerVisual;
SpriteVisual _imageVisual;
SpriteVisual _backdropVisual;

_compositor = Window.Current.Compositor;

// Create container visual to host the visual tree
_containerVisual = _compositor.CreateContainerVisual();

// Create _imageVisual and add it to the bottom of the container visual.
// Paint the visual with an image.

CompositionSurfaceBrush _licoriceBrush = _compositor.CreateSurfaceBrush();

LoadedImageSurface loadedSurface = 
    LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_licoriceBrush.Surface = loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _licoriceBrush;
_imageVisual.Size = new Vector2(156, 156);
_imageVisual.Offset = new Vector3(0, 0, 0);
_containerVisual.Children.InsertAtBottom(_imageVisual)

// Create a SpriteVisual and add it to the top of the containerVisual.
// Paint the visual with an EffectBrush that applies blur to the content
// underneath the Visual to create a frosted glass effect.

GaussianBlurEffect blurEffect = new GaussianBlurEffect(){
                                    Name = "Blur",
                                    BlurAmount = 1.0f,
                                    BorderMode = EffectBorderMode.Hard,
                                    Source = new CompositionEffectSourceParameter("source");
                                    };

CompositionEffectFactory blurEffectFactory = _compositor.CreateEffectFactory(blurEffect);
CompositionEffectBrush _backdropBrush = blurEffectFactory.CreateBrush();

// Create a BackdropBrush and bind it to the EffectSourceParameter source

_backdropBrush.SetSourceParameter("source", _compositor.CreateBackdropBrush());
_backdropVisual = _compositor.CreateSpriteVisual();
_backdropVisual.Brush = _licoriceBrush;
_backdropVisual.Size = new Vector2(78, 78);
_backdropVisual.Offset = new Vector3(39, 39, 0);
_containerVisual.Children.InsertAtTop(_backdropVisual);
```

## <a name="combining-compositionbrushes"></a>Kombinieren von compositionbrushes
Eine Reihe von compositionbrush-Werten verwendet andere compositionbrushes als Eingaben. Beispielsweise kann mithilfe der setsourceparameter-Methode ein anderer compositionbrush als Eingabe für einen compositioneffectbrush festgelegt werden. In der folgenden Tabelle werden die unterstützten Kombinationen von compositionbrushes beschrieben. Beachten Sie, dass eine nicht unterstützte Kombination eine Ausnahme auslöst.

<table>
<tbody>
<tr>
<th>Brush</th>
<th>Effectbrush. setsourceparameter ()</th>
<th>Maskbrush. Mask</th>
<th>Maskbrush. Source</th>
<th>Ninegridbrush. Source</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
</tr>
<tr>
<td>Compositionlinear<br />GradientBrush</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
<td>Nein</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
</tr>
<tr>
<td>Compositioneffectbrush</td>
<td>Nein</td>
<td>Nein</td>
<td>YES</td>
<td>Nein</td>
</tr>
<tr>
<td>Compositionmaskbrush</td>
<td>Nein</td>
<td>Nein</td>
<td>Nein</td>
<td>Nein</td>
</tr>
<tr>
<td>Compositionninegridbrush</td>
<td>YES</td>
<td>YES</td>
<td>YES</td>
<td>Nein</td>
</tr>
<tr>
<td>Compositionbackdropbrush</td>
<td>YES</td>
<td>Nein</td>
<td>Nein</td>
<td>Nein</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Verwenden eines XAML-Pinsels im Vergleich zu compositionbrush

In der folgenden Tabelle finden Sie eine Liste der Szenarien und darüber, ob die Verwendung von XAML-oder Kompositions Pinseln beim Zeichnen eines UIElement-oder spritevisual-Elements in der Anwendung vorgeschrieben ist. 

> [!NOTE]
> Wenn ein compositionbrush für ein XAML-UIElement vorgeschlagen wird, wird davon ausgegangen, dass der compositionbrush mithilfe von xamlcompositionbrushbase gepackt wird.

|Szenario                                                                   | XAML UIElement                                                                                                |Komposition spritevisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Zeichnen eines Bereichs mit einer voll Tonfarbe                                             |[SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|Zeichnen eines Bereichs mit animierter Farbe                                          |[SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|Zeichnen eines Bereichs mit einem statischen Farbverlauf                                       |[LinearGradientBrush](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)                            |[Compositionlineargradientbrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Zeichnen eines Bereichs mit animierten Farbverlaufs Stopps                                 |[Compositionlineargradientbrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[Compositionlineargradientbrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Zeichnen eines Bereichs mit einem Bild                                                |[ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)                                     |[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)
|Zeichnen eines Bereichs mit einer Webseite                                               |[WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)                                   |Nicht zutreffend
|Zeichnen eines Bereichs mit einem Bild mithilfe der NineGrid-Streckung                         |[Image-Steuerelement](/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[Compositionninegridbrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Zeichnen eines Bereichs mit animierter NineGrid-Streckung                               |[Compositionninegridbrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[Compositionninegridbrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Zeichnen eines Bereichs mit einer vorhandenes SwapChain                                             |[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[Compositionsurfacebrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) -/SwapChain-Interop
|Zeichnen eines Bereichs mit einem Video                                                 |[MediaElement](../design/controls-and-patterns/media-playback.md)                                                                                                  |[Compositionsurfacebrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) -/medieninterop
|Zeichnen eines Bereichs mit benutzerdefiniertem 2D-Zeichen                                       |[Canvascontrol](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) aus Win2D                                                                                                 |[Compositionsurfacebrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) w/Win2D-Interop
|Zeichnen eines Bereichs mit nicht animierter Maske                                       |Verwenden von XAML- [Formen](../design/controls-and-patterns/shapes.md) zum Definieren einer Maske   |[Compositionmaskbrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Zeichnen eines Bereichs mit einer animierten Maske                                        |[Compositionmaskbrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[Compositionmaskbrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Zeichnen eines Bereichs mit einem animierten Filtereffekt                               |[Compositioneffectbrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[Compositioneffectbrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Zeichnen eines Bereichs mit einem Effekt, der auf Hintergrund Pixel angewendet wird        |[Compositionbackdropbrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[Compositionbackdropbrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Verwandte Themen

[Komposition Native DirectX und Direct2D Interop mit beginDraw und EndDraw](composition-native-interop.md)

[XAML-Pinsel-Interop mit xamlcompositionbrushbase](../design/style/brushes.md#xamlcompositionbrushbase)