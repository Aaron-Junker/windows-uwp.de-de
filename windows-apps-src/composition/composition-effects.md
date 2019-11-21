---
ms.assetid: 6e9b9ff2-234b-6f63-0975-1afb2d86ba1a
title: Kompositionseffekte
description: Mithilfe von Effekt-APIs können Entwickler anpassen, wie ihre Benutzeroberfläche gerendert wird.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 57236b6780a7afe996fb1e68ac474d8d8077ca69
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255905"
---
# <a name="composition-effects"></a>Kompositionseffekte

Mit der API [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition) können Echtzeiteffekte mithilfe animierbarer Effekteigenschaften auf Bilder und Benutzeroberflächen angewendet werden. In dieser Übersicht erläutern wir die Funktionen, über die Effekte auf visuelle Kompositionselemente angewendet werden können.

Um die Konsistenz der [Universellen Windows-Plattform (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp) für Entwickler zu gewährleisten, die Effektbeschreibungen in ihren Anwendungen verwenden, nutzen Kompositionseffekte die „IGraphicsEffect“-Schnittstelle von Win2D, um die Effektbeschreibungen über den [Microsoft.Graphics.Canvas.Effects](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm)-Namespace anzuwenden.

Pinseleffekte werden verwendet, um Bereiche einer Anwendung farbig zu gestalten. Dabei werden Effekte auf eine Gruppe vorhandener Bilder angewendet. Die Kompositionseffekt-APIs in Windows 10 sind auf visuelle Sprite-Elemente ausgerichtet. Das SpriteVisual-Element ermöglicht hohe Flexibilität und Interaktion bei der Farb-, Bild- und Effektgestaltung. SpriteVisual ist ein visueller Kompositionstyp, der ein 2D-Rechteck mit einem Pinsel füllen kann. Das visuelle Element definiert die Grenzen des Rechtecks und der Pinsel die Pixel zum Zeichnen des Rechtecks.

Effektpinsel werden für visuelle Elemente von Kompositionsstrukturen verwendet, deren Inhalt der Ausgabe eines Effektgraphen entnommen wird. Effekte können auf vorhandene Oberflächen/Texturen verweisen, aber nicht auf die Ausgabe anderer Kompositionsstrukturen.

Effekte können auch für XAML-UI-Elemente mit einem Effektpinsel mit [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) angewendet werden.

## <a name="effect-features"></a>Effektfeatures

- [Effect Library](./composition-effects.md#effect-library)
- [Chaining Effects](./composition-effects.md#chaining-effects)
- [Animation Support](./composition-effects.md#animation-support)
- [Constant vs. Animated Effect Properties](./composition-effects.md#constant-vs-animated-effect-properties)
- [Multiple Effect Instances with Independent Properties](./composition-effects.md#multiple-effect-instances-with-independent-properties)

### <a name="effect-library"></a>Effektbibliothek

Derzeit unterstützt die Komposition folgende Effekte:

| Auswirkung               | Beschreibung                                                                                                                                                                                                                |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2D-affine Transformation  | Wendet eine 2D-affine Transformationsmatrix auf ein Bild an. Dieser Effekt wurde verwendet, um die Alphamaske in unseren [Effektbeispielen](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects) zu animieren.       |
| Arithmetische Komposition | Kombiniert zwei Bilder mittels einer flexiblen Gleichung. Eine arithmetische Komposition wurde verwendet, um einen Überblendungseffekt in unseren [Beispielen](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects) zu erzeugen. |
| Fülleffekt         | Erzeugt einen Fülleffekt, der zwei Bilder kombiniert. Die Komposition stellt 21 der 26 in Win2D unterstützten [Füllmethoden](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_Effects_BlendEffectMode.htm) bereit.        |
| Farbquelle         | Generiert ein Bild, das eine Volltonfarbe enthält.                                                                                                                                                                               |
| Komposition            | Kombiniert zwei Bilder. Die Komposition stellt alle 13 in Win2D unterstützten [Kompositionsmodi](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasComposite.htm) bereit.                                              |
| Kontrast             | Erhöht oder verringert den Kontrast eines Bilds.                                                                                                                                                                           |
| Belichtung             | Erhöht oder verringert die Belichtung eines Bilds.                                                                                                                                                                           |
| Graustufen            | Konvertiert ein Bild in ein monochromes Graustufenbild.                                                                                                                                                                                   |
| Gammakorrektur       | Ändert die Farben eines Bilds, indem die Gammakorrekturfunktion pro Kanal angewendet wird.                                                                                                                                           |
| Farbtondrehung           | Ändert die Farbe eines Bilds durch Rotation der Farbtonwerte.                                                                                                                                                                   |
| Invertierung               | Invertiert die Farben eines Bilds.                                                                                                                                                                                            |
| Sättigung             | Ändert die Sättigung eines Bilds.                                                                                                                                                                                         |
| Sepia                | Konvertiert ein Bild in Sepiatöne.                                                                                                                                                                                          |
| Temperatur und Farbton | Passt die Temperatur und/oder den Farbton eines Bilds an.                                                                                                                                                                           |

Ausführliche Informationen finden Sie in der Beschreibung des Win2D-Namespaces [Microsoft.Graphics.Canvas.Effects](https://microsoft.github.io/Win2D/html/N_Microsoft_Graphics_Canvas_Effects.htm). Effects not supported in composition are noted as \[NoComposition\].

### <a name="chaining-effects"></a>Verketten von Effekten

Effekte können verkettet werden, um einer Anwendung die gleichzeitige Nutzung mehrerer Effekte in einem Bild zu ermöglichen. Effektgraphen unterstützen mehrere Effekte, die aufeinander verweisen können. Bei der Beschreibung eines Effekts fügen Sie Ihrem Effekt einfach einen Effekt als Eingabe hinzu.

```cs
IGraphicsEffect graphicsEffect =
new Microsoft.Graphics.Canvas.Effects.ArithmeticCompositeEffect
{
  Source1 = new CompositionEffectSourceParameter("source1"),
  Source2 = new SaturationEffect
  {
    Saturation = 0,
    Source = new CompositionEffectSourceParameter("source2")
  },
  MultiplyAmount = 0,
  Source1Amount = 0.5f,
  Source2Amount = 0.5f,
  Offset = 0
}
```

Im obigen Beispiel wird der Effekt einer arithmetischen Komposition mit zwei Eingaben beschrieben. Die zweite Eingabe hat einen Sättigungseffekt mit der Sättigungseigenschaft „0,5“.

### <a name="animation-support"></a>Animationsunterstützung

Effekteigenschaften bieten Unterstützung für Animationen. Während der Effektkompilierung können Sie angeben, welche Effekteigenschaften animiert und welche als Konstanten vorgegeben werden können. Die animierbaren Eigenschaften werden über Zeichenfolgen im Format „Effektname.Eigenschaftenname“ angegeben. Diese Eigenschaften können unabhängig über mehrere Instanziierungen des Effekts animiert werden.

### <a name="constant-vs-animated-effect-properties"></a>Konstante im Vergleich zu animierten Effekteigenschaften

Während der Effektkompilierung können Sie Effekteigenschaften als dynamisch oder als Eigenschaften angeben, die konstant fest sind. Die dynamischen Eigenschaften werden über Zeichenfolgen in folgendem Format angegeben: „<effect name>.<property name>“. Die dynamischen Eigenschaften können auf einen bestimmten Wert festgelegt oder mithilfe des Kompositionsanimationssystems animiert werden.

Beim Kompilieren der oben angegebenen Effektbeschreibung können Sie die Sättigung entweder als Konstante mit dem Wert „0,5“ vorgeben oder sie dynamisch gestalten (sie also dynamisch festlegen oder animieren).

Kompilieren eines Effekts mit als Konstante festgelegter Sättigung:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
```

Kompilieren eines Effekts mit dynamischer Sättigung:

```cs
var effectFactory = _compositor.CreateEffectFactory(graphicsEffect, new[]{"SaturationEffect.Saturation"});
_catEffect = effectFactory.CreateBrush();
_catEffect.SetSourceParameter("mySource", surfaceBrush);
_catEffect.Properties.InsertScalar("saturationEffect.Saturation", 0f);
```

Die Sättigungseigenschaft des oben genannten Effekts kann dann entweder auf einen statischen Wert festgelegt oder mithilfe der Expression- oder ScalarKeyFrame-Animation animiert werden.

Sie können ein ScalarKeyFrame-Element erstellen, das wie folgt zum Animieren der Sättigungseigenschaft eines Effekts verwendet wird:

```cs
ScalarKeyFrameAnimation effectAnimation = _compositor.CreateScalarKeyFrameAnimation();
            effectAnimation.InsertKeyFrame(0f, 0f);
            effectAnimation.InsertKeyFrame(0.50f, 1f);
            effectAnimation.InsertKeyFrame(1.0f, 0f);
            effectAnimation.Duration = TimeSpan.FromMilliseconds(2500);
            effectAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
```

Starten Sie wie folgt die Animation der Sättigungseigenschaft des Effekts:

```cs
catEffect.Properties.StartAnimation("saturationEffect.Saturation", effectAnimation);
```

Im Beispiel [Entsättigung – Animation](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/Desaturation - Animation) finden Sie mithilfe von Keyframes animierte Effekteigenschaften. Das [AlphaMask-Beispiel](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/AlphaMask) enthält Informationen zur Verwendung von Effekten und Ausdrücken.

### <a name="multiple-effect-instances-with-independent-properties"></a>Mehrere Effektinstanzen mit unabhängigen Eigenschaften

Wenn angegeben wird, dass ein Parameter während der Effektkompilierung dynamisch sein soll, kann der Parameter pro Effektinstanz geändert werden. Dadurch können zwei visuelle Elemente denselben Effekt verwenden, aber mit unterschiedlichen Effekteigenschaften gerendert werden. Weitere Informationen finden Sie im [Beispiel](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/BasicCompositonEffects/ColorSource and Blend) zum ColorSource- und Blend-Effekt.

## <a name="getting-started-with-composition-effects"></a>Erste Schritte mit Kompositionseffekten

In diesem Schnellstart-Lernprogramm erfahren Sie, wie Sie einige der grundlegenden Effektfunktionen nutzen.

- [Installing Visual Studio](./composition-effects.md#installing-visual-studio)
- [Creating a new project](./composition-effects.md#creating-a-new-project)
- [Installing Win2D](./composition-effects.md#installing-win2d)
- [Setting your Composition Basics](./composition-effects.md#setting-your-composition-basics)
- [Creating a CompositionSurface Brush](./composition-effects.md#creating-a-compositionsurface-brush)
- [Creating, Compiling and Applying Effects](./composition-effects.md#creating-compiling-and-applying-effects)

### <a name="installing-visual-studio"></a>Installieren von Visual Studio

- Wenn Sie keine unterstützte Version von Visual Studio installiert haben, wechseln Sie [hier](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs) zur Visual Studio-Downloadseite.

### <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

- Wählen Sie „Datei->Neu->Projekt“ aus.
- Wählen Sie „Visual C#“ aus.
- Erstellen Sie eine App vom Typ „(Leere App (Windows – universell)“ (Visual Studio 2015).
- Geben Sie einen Projektnamen Ihrer Wahl ein.
- Klicken Sie auf „OK“.

### <a name="installing-win2d"></a>Installieren von Win2D

Win2D wird als „Nuget.org“-Paket freigegeben und muss installiert werden, damit Sie Effekte nutzen können.

Es gibt zwei Paketversionen: eine für Windows 10 und eine für Windows 8.1. Für Kompositionseffekte verwenden Sie die Windows 10-Version.

- Starten Sie den NuGet-Paket-Manager, indem Sie „Extras → NuGet-Paket-Manager → NuGet-Pakete für Projektmappe verwalten“ auswählen.
- Suchen Sie nach „Win2D“, und wählen Sie das entsprechende Paket für die Zielversion von Windows aus. Da Windows.UI. Composition Windows 10 (aber nicht 8.1) unterstützt, wählen Sie „Win2D.uwp“ aus.
- Akzeptieren Sie den Lizenzvertrag.
- Klicken Sie auf „Schließen“.

In den nächsten Schritten verwenden wir Composition-APIs, um einen Sättigungseffekt auf dieses Katzenfoto anzuwenden, durch den die gesamte Sättigung entfernt wird. In diesem Modell wird der Effekt erstellt und dann auf ein Bild angewendet.

![Quellbild](images/composition-cat-source.png)
### <a name="setting-your-composition-basics"></a>Festlegen der Grundlagen für die Komposition

Anhand des [Beispiels für die visuelle Kompositionsstruktur](https://github.com/microsoft/WindowsCompositionSamples/tree/master/Demos/Reference Demos/CompositionImageSample) auf GitHub erfahren Sie, wie Sie den „Windows.UI.Composition“-Kompositor einrichten, den „ContainerVisual“-Stamm angeben und diesen dem Hauptfenster zuordnen.

```cs
_compositor = new Compositor();
_root = _compositor.CreateContainerVisual();
_target = _compositor.CreateTargetForCurrentView();
_target.Root = _root;
_imageFactory = new CompositionImageFactory(_compositor)
Desaturate();
```

### <a name="creating-a-compositionsurface-brush"></a>Erstellen eines CompositionSurface-Pinsels

```cs
CompositionSurfaceBrush surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(surfaceBrush);
```

### <a name="creating-compiling-and-applying-effects"></a>Erstellen, Kompilieren und Anwenden von Effekten

1. Erstellen Sie den Grafikeffekt

    ```cs
    var graphicsEffect = new SaturationEffect
    {
      Saturation = 0.0f,
      Source = new CompositionEffectSourceParameter("mySource")
    };
    ```

1. Kompilieren Sie den Effekt, und erstellen Sie einen Effektpinsel

    ```cs
    var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);

    var catEffect = effectFactory.CreateBrush();
    catEffect.SetSourceParameter("mySource", surfaceBrush);
    ```

1. Erstellen Sie ein SpriteVisual-Element in der Kompositionsstruktur, und wenden Sie den Effekt an

    ```cs
    var catVisual = _compositor.CreateSpriteVisual();
      catVisual.Brush = catEffect;
      catVisual.Size = new Vector2(219, 300);
      _root.Children.InsertAtBottom(catVisual);
    }
    ```

1. Erstellen Sie die zu ladende Bildquelle.

    ```cs
    CompositionImage imageSource = _imageFactory.CreateImageFromUri(new Uri("ms-appx:///Assets/cat.png"));
    CompositionImageLoadResult result = await imageSource.CompleteLoadAsync();
    if (result.Status == CompositionImageLoadStatus.Success)
    ```

1. Passen Sie die Größe der SpriteVisual-Oberfläche an, und füllen Sie sie

    ```cs
    brush.Surface = imageSource.Surface;
    ```

1. Führen Sie die App aus, um ein Katzenfoto ohne Sättigung zu erhalten:

![Bild ohne Sättigung](images/composition-cat-desaturated.png)

## <a name="more-information"></a>Weitere Informationen

- [Microsoft – Composition GitHub](https://github.com/microsoft/WindowsCompositionSamples)
- [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
- [Windows Composition team on Twitter](https://twitter.com/wincomposition)
- [Composition Overview](https://blogs.windows.com/buildingapps/2015/12/08/awaken-your-creativity-with-the-new-windows-ui-composition/)
- [Visual Tree Basics](composition-visual-tree.md)
- [Composition Brushes](composition-brushes.md)
- [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)
- [Animation Overview](composition-animation.md)
- [Composition native DirectX and Direct2D interoperation with BeginDraw and EndDraw](composition-native-interop.md)
