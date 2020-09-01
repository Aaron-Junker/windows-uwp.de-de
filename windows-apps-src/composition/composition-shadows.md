---
title: Kompositions Schatten
description: Mit den Schatten-APIs können Sie Benutzeroberflächen Inhalten dynamisch anpassbare Schatten hinzufügen.
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 29ca04ac3faf3a2884bcc2346177f49222cf786e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166424"
---
# <a name="shadows-in-windows-ui"></a>Schatten in der Windows-Benutzeroberfläche

Die [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) -Klasse bietet die Möglichkeit, einen konfigurierbaren Schatten zu erstellen, der auf ein [spritevisual](/uwp/api/windows.ui.composition.spritevisual) oder [layervisual](/uwp/api/windows.ui.composition.layervisual) (Teilstruktur von visuellen Elementen) angewendet werden kann. Wie bei Objekten in der visuellen Ebene üblich, können alle Eigenschaften von DropShadow mithilfe von compositionanimationen animiert werden.

## <a name="basic-drop-shadow"></a>Grundlegender Schlag Schatten

Um einen grundlegenden Schatten zu erstellen, erstellen Sie einfach einen neuen DropShadow und ordnen ihn dem visuellen Element zu. Der Schatten ist standardmäßig rechteckig. Ein Standardsatz von Eigenschaften ist verfügbar, um das Aussehen und das Gefühl Ihres Schattens zu optimieren.

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![Rechteckige Visualisierung mit grundlegender DropShadow](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Strukturieren des Schattens

Es gibt mehrere Möglichkeiten, die Form für Ihren DropShadow zu definieren:

- **Standard verwenden** : Standardmäßig wird die DropShadow-Form durch den Standardmodus für compositiondropshadowsourcepolicy definiert. Der Standardwert für spritevisual ist rechteckig, es sei denn, es wird eine Maske bereitgestellt. Bei layervisual wird standardmäßig eine Maske mit dem Alpha des visuellen Elements geerbt.
- **Festlegen einer Maske** – Sie können die [Mask](/uwp/api/windows.ui.composition.dropshadow.mask) -Eigenschaft festlegen, um eine Deckkraft Maske für den Schatten zu definieren.
- **Geben Sie an, dass geerbte Maske verwendet** werden soll – legen Sie die [sourcepolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) -Eigenschaft auf [compositiondropshadowsourcepolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)fest. Vererfromvisualcontent, um die Maske zu verwenden, die aus dem Alpha des visuellen Elements generiert wurde.

## <a name="masking-to-match-your-content"></a>Maskierung entsprechend ihren Inhalten

Wenn Sie möchten, dass ihr Schatten mit dem Inhalt der Visualisierung identisch ist, können Sie entweder den Pinsel des visuellen Elements für ihre Schatten Masken Eigenschaft verwenden oder den Schatten so festlegen, dass die Maske automatisch vom Inhalt geerbt wird. Wenn ein layervisual verwendet wird, erbt der Schatten standardmäßig die Maske.

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Verbundenes Webbild mit maskiertem Schlag Schatten](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>Verwenden einer alternativen Maske

In einigen Fällen möchten Sie den Schatten so strukturieren, dass er nicht mit dem Inhalt Ihrer Visualisierung identisch ist. Um diesen Effekt zu erzielen, müssen Sie die Mask-Eigenschaft mithilfe eines Pinsels mit Alpha explizit festlegen.

Im folgenden Beispiel werden zwei Oberflächen geladen: eine für den visuellen Inhalt und eine für die Schatten Maske:

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Verbundenes Webbild mit Kreis maskiertem Schlag Schatten](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animieren

Wie in der visuellen Schicht Standard, können DropShadow-Eigenschaften mithilfe von Kompositions Animationen animiert werden. Im folgenden wird der Code aus dem obigen Sprinkles-Beispiel geändert, um den Weichzeichnerradius für den Schatten zu animieren.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>Schatten in XAML

Wenn Sie komplexeren Frameworkelementen einen Schatten hinzufügen möchten, gibt es mehrere Möglichkeiten, mit Schatten zwischen XAML und Komposition zu interagieren:

1. Verwenden Sie das [dropshadowpanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) , das im Windows Community Toolkit verfügbar ist. Ausführliche Informationen zur Verwendung finden Sie in der [dropshadowpanel-Dokumentation](/windows/uwpcommunitytoolkit/controls/DropShadowPanel) .
1. Erstellen Sie ein visuelles Element, das als Schatten Host verwendet werden soll, & es mit dem XAML-Handout-visuellen Element verknüpfen.
1. Verwenden Sie das benutzerdefinierte compositionshadow-Steuerelement [samplescommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) der Komposition Sample Gallery. Informationen zur Verwendung finden Sie hier.

## <a name="performance"></a>Leistung

Obwohl auf der visuellen Ebene viele Optimierungen vorhanden sind, um Effekte effizient und verwendbar zu machen, kann das Erzeugen von Schatten in Abhängigkeit von den von Ihnen festgelegten Optionen ein relativ kostspieliger Vorgang sein. Unten sind die Kosten für unterschiedliche Schatten Typen aufgeführt. Beachten Sie, dass obwohl bestimmte Schatten vielleicht kostspielig sein können, dass Sie in bestimmten Szenarien weiterhin sparsam eingesetzt werden können.

Schatten Merkmale| „Cost“ (Kosten)
------------- | -------------
Rechteck    | Low (Niedrig)
Shadow. Mask      | High
Compositiondropshadowsourcepolicy. vererfromvisualcontent | High
Statischer weichzeichradius | Low (Niedrig)
Animieren des weichungs RADIUS | High

## <a name="additional-resources"></a>Weitere Ressourcen

- [Komposition-DropShadow-API](/uwp/api/Windows.UI.Composition.DropShadow)
- [GitHub-Repository windowsuidevlabs](https://github.com/microsoft/WindowsCompositionSamples)