---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Kompositionspinsel
description: Ein Pinsel zeichnet den Bereich eines Visual-Objekts mit der zugehörigen Ausgabe. Verschiedene Pinsel haben unterschiedliche Ausgabetypen.
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d51bc945c721ae15889dece8f84959f9a78192bc
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63790516"
---
# <a name="composition-brushes"></a>Kompositionspinsel
Alles, was auf dem Bildschirm einer UWP-Anwendung sichtbar ist, wird angezeigt, da es mit einem Pinsel gezeichnet wurde. Mithilfe von Pinseln können Sie Benutzeroberflächenobjekte (UI-Objekte) mit Inhalt zeichnen, angefangen bei einfachen, soliden Farben bis hin zu Bildern oder Zeichnungen mit komplexen verketteten Effekten. In diesem Thema werden die Konzepte der Zeichnung mit CompositionBrush behandelt.

Beachten Sie bei der Arbeit mit einer XAML-UWP-App, dass Sie ein UI-Element mit einem [XAML-Pinsel](/windows/uwp/design/style/brushes) oder [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) zeichnen können. In der Regel ist es einfacher und ratsam, einen XAML-Pinsel auszuwählen, wenn Ihr Szenario von einem XAML-Pinsel unterstützt wird. Dies gilt, wenn Sie beispielsweise die Farbe einer Taste animieren, einen Text oder eine Form mit einem Bild ändern möchten. Andererseits, wenn Sie versuchen, etwas zu tun, die nicht von einem XAML-Pinsel, wie man Sie mit der eine animierte Maske oder eine animierte ninegrid-Stretch oder eine Kette wirksam unterstützt wird, können Sie eine CompositionBrush "UIElement" mithilfe des zu zeichnenden [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Bei der Arbeit mit der visuellen Ebene, muss ein CompositionBrush verwendet werden, um im Bereich von [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) zu zeichnen.

-   [Voraussetzungen](./composition-brushes.md#prerequisites)
-   [Zeichnen mit CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Zeichnen Sie mit einer Volltonfarbe](./composition-brushes.md#paint-with-a-solid-color)
    -   [Zeichnen Sie mit einem linearen Farbverlauf](./composition-brushes.md#paint-with-a-linear-gradient) 
    -   [Zeichnen Sie mit einem radialen Farbverlauf](./composition-brushes.md#paint-with-a-radial-gradient)
    -   [Zeichnen mit einem Bild](./composition-brushes.md#paint-with-an-image)
    -   [Mit einer benutzerdefinierte Zeichnung zu zeichnen](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Zeichnen mit einem video](./composition-brushes.md#paint-with-a-video)
    -   [Zeichnen Sie mit einem Filtereffekt](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Zeichnen Sie mit einem CompositionBrush mit einer Deckkraftmaske](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Zeichnen Sie mit einem CompositionBrush mit NineGrid stretch](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Mit Hintergrund Pixel gezeichnet werden soll](./composition-brushes.md#paint-using-background-pixels)
-   [Kombinieren von CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Verwenden einen XAML-Pinsel im Vergleich. CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Verwandte Themen](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Vorraussetzungen
In dieser Übersicht wird davon ausgegangen, dass Sie mit der Struktur einer einfachen Kompositionsanwendung, wie unter [Übersicht über die visuelle Ebene](visual-layer.md) beschrieben, vertraut sind.

## <a name="paint-with-a-compositionbrush"></a>Zeichnen mit einem CompositionBrush

Ein [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) zeichnet den Bereich mit der zugehörigen Ausgabe. Verschiedene Pinsel haben unterschiedliche Ausgabetypen. Einige Pinsel zeichnen einen Bereich mit einer Volltonfarbe, andere mit einem Farbverlauf, einem Bild, einer benutzerdefinierten Zeichnung oder einem Effekt. Es gibt auch spezielle Pinsel, die das Verhalten der anderen Pinsel ändern. Eine Deckkraftmaske kann beispielsweise verwendet werden, um zu steuern, welcher Bereich durch einen CompositionBrush gezeichnet wird, oder ein Raster mit neun Bereichen kann verwendet werden, um das Stretching auf einem CompositionBrush zu steuern, wenn Sie einen Bereich zeichnen. CompositionBrush können folgende Formate haben:

|Klasse                                   |Details                                         |Eingeführt in|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Zeichnet einen Bereich mit einer Volltonfarbe                        |Windows 10, Version 1511 (10586-SDK)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Zeichnet einen Bereich mit den Inhalten einer [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Windows 10, Version 1511 (10586-SDK)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Zeichnet einen Bereich mit den Inhalten eines Kompositionseffekts |Windows 10, Version 1511 (10586-SDK)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Zeichnet einen visuellen Bereich mit einem CompositionBrush mit einer Deckkraftmaske |Windows 10 Version 1607 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Zeichnet einen Bereich mit einem CompositionBrush mit Nine-Grid-Stretching |Windows 10 Version 1607 (SDK 14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Zeichnet einen Bereich mit einem linearen Farbverlauf                    |Windows 10, Version 1709 (16299-SDK)
|[CompositionRadialGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionradialgradientbrush)|Zeichnet einen Bereich mit einem radialen Farbverlauf                    |Windows 10, Version 1903 (Insider Preview SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Zeichnet einen Bereich durch Sampling von Hintergrundpixeln in der Anwendung oder von Pixel direkt hinter dem Fenster der Anwendung auf dem Desktop. Wird als Eingabe für einen anderen CompositionBrush wie ein CompositionEffectBrush verwendet | Windows 10 Version 1607 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Zeichnen mit einer Volltonfarbe

Ein [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) zeichnet ein Visual-Element mit einer Volltonfarbe. Es gibt eine Vielzahl von Möglichkeiten, um die Farbe des SolidColorBrush anzugeben. Sie können z. B. die Kanäle Alpha, Rot, Blau und Grün (ARGB) angeben oder eine der vordefinierten Farben verwenden, die in der Klasse [Farben](https://docs.microsoft.com/uwp/api/windows.ui.colors) zur Verfügung gestellt werden.

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

Ein [CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) zeichnet einen Bereich mit einem linearen Farbverlauf. Ein linearer Farbverlauf mischt zwei oder mehr Farben entlang einer Linie: der Farbverlaufsachse. So verwenden Sie GradientStop-Objekte, um Farben im Farbverlauf und deren Position festzulegen.

Die folgende Abbildung und der Code zeigt ein SpriteVisual-Element mit einem LinearGradientBrush mit 2 Stopps in Rot und Gelb.

![CompositionLinearGradientBrush](images/composition-compositionlineargradientbrush.png)

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

### <a name="paint-with-a-radial-gradient"></a>Zeichnen Sie mit einem radialen Farbverlauf

Ein [CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush) zeichnet einen Bereich mit einem radialen Farbverlauf. Ein Radialer Farbverlauf mischt zwei oder mehr Farben mit der Farbverlauf vom Mittelpunkt der Ellipse beginnt und endet beim des Radius der Ellipse. GradientStop-Objekte werden zum Definieren von Farben und ihre Position im Farbverlauf.

Der folgenden Abbildung und der Code zeigt eine SpriteVisual mit einem RadialGradientBrush mit 2 GradientStops gezeichnet.

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

Ein [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) zeichnet einen Bereich mit Pixeln, die auf einer ICompositionSurface gerendert werden. Beispielsweise kann ein CompositionSurfaceBrush verwendet werden, um einen Bereich mit einem auf einer ICompositionSurface Surface gerenderten Bild mithilfe von [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface)-API zu zeichnen.

Die folgende Abbildung und der Code zeigt ein SpriteVisual-Element, das mit einer Lakritz-Bitmap gezeichnet und mithilfe von LoadedImageSurface auf einer ICompositionSurface-Schnittstelle gerendert wird. Die Eigenschaften des CompositionSurfaceBrush können zum Strecken und Ausrichten der Bitmap innerhalb der Grenzen des visuellen Elements verwendet werden.

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
Ein [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) kann auch verwendet werden, um einen Bereich mit Pixeln aus einer gerenderten ICompositionSurface zu zeichnen [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm) (oder D2D).

Der folgende Code zeigt ein im Text gezeichnetes Spritevisual-Element, das unter Verwendung von Win2D auf einer ICompositionSurface gerendert wurde. Hinweis: um Win2D zu verwenden, müssen Sie Ihrem Projekt das [Win2D-NuGet](https://www.nuget.org/packages/Win2D.uwp)-Paket hinzufügen.

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

Der CompositionSurfaceBrush kann ebenfalls verwendet werden, um ein Spritevisual-Element mit einer SwapChain mit Win2D-Interoperabilität zu zeichnen. [In diesem Beispiel](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) zeigen wir, wie Win2D verwendet werden kann, um ein Spritevisual-Element mit einem Swapchain zu zeichnen.

### <a name="paint-with-a-video"></a>Zeichnen mit einem Video
Ein [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) kann auch verwendet werden, um einen Bereich mit Pixeln aus einer gerenderten ICompositionSurface mit einem Video aus der [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse zu zeichnen.

Der folgende Code zeigt ein gezeichnetes Spritevisual-Element, das in einem Video auf einer ICompositionSurface hochgeladen wurde.

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

Ein [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) zeichnet einen Bereich mit der zugehörigen Ausgabe eines CompositionEffects. Effekte in der visuellen Ebene können als animierte Filtereffekte verstanden werden, die auf verschiedene Quellinhalte wie z. B. Farben, Farbverläufe, Bilder, Videos, Swapchains, Bereiche der Benutzeroberfläche oder Strukturen von visuellen Elementen angewendet werden. Der Quellinhalt wird normalerweise mit einem anderen CompositionBrush angegeben.

Die folgende Abbildung und der Code zeigt ein Spritevisual-Element mit dem Bild einer Katze, auf das Sättigungsfiltereffekte angewendet wurden.

![CompositionEffectBrush](images/composition-cat-desaturated.png)

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

Weitere Informationen zum Erstellen eines Effekts mit CompositionBrushes finden Sie unter [Effekten in der visuellen Ebene](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Zeichnen mit einem CompositionBrush mit einer angewendeten Deckkraftmaske

Ein [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) zeichnet einen Bereich mit einem CompositionBrush, auf den eine Deckkraftmaske angewendet wird. Die Quelle der Deckkraftmaske kann jeder CompositionBrush des Typs CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush oder CompositionNineGridBrush sein. Die Deckkraftmaske muss als CompositionSurfaceBrush angegeben werden.

Die folgende Abbildung und der Code zeigt ein Spritevisual-Element, das mit einem CompositionMaskBrush gezeichnet wurde. Die Quelle der Maske ist ein CompositionLinearGradientBrush, der wie ein Kreis mit einem Bild des Kreises als eine Maske maskiert ist.

![CompositionMaskBrush](images/composition-compositionmaskbrush.png)

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Zeichnen mit einem CompositionBrush mit Nine-Grid-Stretching

Ein [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) zeichnet einen Bereich mit einem CompositionBrush, der mit einer Nine-Grid-Metapher gestreckt wird. Mit der Neun-Raster-Metapher können Sie die Ränder und Ecken eines CompositionBrush als anders strecken, als den Mittelpunkt. Die Quelle des Nine-Grid-Stretching kann jeder CompositionBrush des Typs CompositionColorBrush, CompositionSurfaceBrush oder CompositionEffectBrush sein.

Der folgende Code zeigt ein Spritevisual-Element, das mit einem CompositionNineGridBrush gezeichnet wurde. Die Quelle der Maske ist ein CompositionSurfaceBrush der mit einem Nine-Grid gestreckt wird.

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

### <a name="paint-using-background-pixels"></a>Zeichnen mit Hintergrundpixeln

Ein [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) zeichnet einen Bereich mit den Inhalt hinter den Bereich. Ein CompositionBackdropBrush wird nie alleine verwendet, aber stattdessen als Eingabe für einen anderen CompositionBrush wie ein EffectBrush verwendet. Beispielsweise können Sie mithilfe eines CompositionBackdropBrush als Eingabe für einen Weichzeichnereffekt einen Milchglas-Effekt erzielen.

Der folgende Code zeigt eine kleine visuelle Struktur, um ein Image mithilfe des CompositionSurfaceBrush und einem Milchglas-Overlay über dem Bild zu erzielen. Das Milchglas-Overlay wird erzielt, indem ein mit einem EffectBrush gefülltes Spritevisual-Element über das Bild platziert wird. Die EffectBrush verwendet einen CompositionBackdropBrush als Eingabe für den Weichzeichnereffekt.

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

## <a name="combining-compositionbrushes"></a>Kombinieren von CompositionBrushes
Eine Reihe von CompositionBrushes verwenden andere CompositionBrushes als Eingaben. Beispielsweise kann die SetSourceParameter-Methode verwendet werden, um einen anderen CompositionBrush als Eingabe für einen CompositionEffectBrush festzulegen. In der folgenden Tabelle werden die unterstützten Kombinationen des CompositionBrushes beschrieben. Beachten Sie, dass durch eine nicht unterstützte Kombination eine Ausnahme ausgelöst wird.

<table>
<tbody>
<tr>
<th>Brush</th>
<th>EffectBrush.SetSourceParameter()</th>
<th>MaskBrush.Mask</th>
<th>MaskBrush.Source</th>
<th>NineGridBrush.Source</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>JA</td>
<td>JA</td>
<td>JA</td>
<td>JA</td>
</tr>
<tr>
<td>CompositionLinear<br />GradientBrush</td>
<td>JA</td>
<td>JA</td>
<td>JA</td>
<td>NEIN</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>JA</td>
<td>JA</td>
<td>JA</td>
<td>JA</td>
</tr>
<tr>
<td>CompositionEffectBrush</td>
<td>NEIN</td>
<td>NEIN</td>
<td>JA</td>
<td>NEIN</td>
</tr>
<tr>
<td>CompositionMaskBrush</td>
<td>NEIN</td>
<td>NEIN</td>
<td>NEIN</td>
<td>NEIN</td>
</tr>
<tr>
<td>CompositionNineGridBrush</td>
<td>JA</td>
<td>JA</td>
<td>JA</td>
<td>NEIN</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>JA</td>
<td>NEIN</td>
<td>NEIN</td>
<td>NEIN</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Verwenden einen XAML-Pinsel im Vergleich. CompositionBrush

Die folgende Tabelle enthält eine Liste von Szenarien und gibt an, ob XAML oder Kompositionspinsel vorgeschrieben werden, wenn ein UIElement oder ein Spritevisual-Element in der Anwendung gezeichnet wird. 

> [!NOTE]
> Wenn ein CompositionBrush für ein XAML-UIElement vorgeschlagen wird, wird davon ausgegangen, dass der CompositionBrush mit einer XamlCompositionBrushBase gepackt ist.

|Szenario                                                                   | XAML-UI-Element                                                                                                |SpriteVisual-Komposition
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Einen Bereich mit einer Volltonfarbe zeichnen                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Einen Bereich mit einer animierten Farbe zeichnen                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Einen Bereich mit einem statischen Farbverlauf zeichnen                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Einen Bereich mit animierten Farbverlaufsstopps zeichnen                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Einen Bereich mit einem Bild zeichnen                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Einen Bereich mit einer Website zeichnen                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |Nicht zutreffend
|Einen Bereich mit einem Bild mit Nine-Grid-Stretching zeichnen                         |[Image-Steuerelement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Einen Bereich mit einem animierten Nine-Grid-Stretching zeichnen                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Einen Bereich mit einer Swapchain zeichnen                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) mit Swapchain-Interoperabilität
|Einen Bereich mit einem Video zeichnen                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) mit Media-Interoperabilität
|Einen Bereich mit benutzerdefinierter 2D-Zeichnung zeichnen                                       |[CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) von Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) mit Win2D-Interoperabilität
|Einen Bereich mit einer nicht animierten Maske zeichnen                                       |Verwenden Sie XAML [Formen](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes), um eine Maske zu definieren   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Einen Bereich mit einer animierten Maske zeichnen                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Einen Bereich mit einem animierten Filtereffekt zeichnen                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Einen Bereich mit einem auf Hintergrund-Pixel angewendeten Effekt zeichnen        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Verwandte Themen

[Komposition systemeigenen DirectX und Direct2D-Interop mit "beginDraw" und "EndDraw"](composition-native-interop.md)

[XAML-Pinsel-Interoperabilität mit XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
