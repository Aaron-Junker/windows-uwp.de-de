---
title: Komposition Schatten
description: Der Schattenkopie-APIs können Sie die Inhalte der Benutzeroberfläche dynamische anpassbaren Schatten hinzugefügt.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9541ea1c00d473bc4881a80d8597625592e278f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630835"
---
# <a name="shadows-in-windows-ui"></a>Zeichnen von Schatten im Windows-Benutzeroberfläche

Die [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) Klasse bietet die Möglichkeit, erstellen einen konfigurierbaren Schatten, die angewendet werden kann eine [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) oder [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (eine Unterstruktur von visuellen Elementen). Wie üblich, für Objekte in der visuellen Ebene ist, können alle Eigenschaften der DropShadow-Steuerelements mit CompositionAnimations animiert werden.

## <a name="basic-drop-shadow"></a>Grundlegende Schlagschatten

Um einen grundlegenden Schatten zu erstellen, Erstellen eines neuen DropShadow-Steuerelements und dem visuellen zuordnen. Der Schatten ist standardmäßig rechteckig. Ein standardmäßigen Satz von Eigenschaften sind verfügbar, um das Aussehen und Verhalten von Ihrem Schatten zu optimieren.

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

![Rechteckige Visual mit grundlegenden DropShadow-Steuerelements](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Strukturieren des Schattens

Es gibt mehrere Möglichkeiten, um die Form für Ihre DropShadow-Steuerelements zu definieren:

- **Verwenden Sie die Standardeinstellung** – standardmäßig ist die Form "DropShadow" durch den Modus "Standard" auf CompositionDropShadowSourcePolicy definiert. Für SpriteVisual ist die Standardeinstellung rechteckig, es sei denn, eine Maske bereitgestellt wird. Für LayerVisual wird standardmäßig eine Maske, mit dem Alphawert des visuellen Objekts Pinsels zu erben.
- **Festlegen eine Maske** – Sie können festlegen, der [Maske](/uwp/api/windows.ui.composition.dropshadow.mask) Eigenschaft, um eine Deckkraftmaske für den Schatten zu definieren.
- **Angeben, um geerbte Maske verwenden** : durch Festlegen der [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) Eigenschaft, die [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent mit der Maske, die aus der Alphaversion des visuellen Objekts Pinsels generiert.

## <a name="masking-to-match-your-content"></a>Maskierung entsprechend Ihrer Inhalte

Wenn Sie möchten Ihre Schattenkopien entsprechend der Inhalt des visuellen Objekts, Sie können entweder Verwenden des visuellen Objekts Pinsel für die Schattenkopien Mask-Eigenschaft, oder den Schatten Maske automatisch aus dem Inhalt zu erben. Wenn Sie eine LayerVisual verwenden zu können, wird der Schatten die Maske standardmäßig erben.

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

![Verbundene WebImage mit maskierten Schatten](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>Verwenden eine andere Maske

In einigen Fällen empfiehlt es sich um den Schatten strukturieren, dass die Visualisierung des Inhalts keine Übereinstimmung. Um diesen Effekt zu erzielen, müssen Sie explizit die Mask-Eigenschaft, die mit einem Pinsel Alpha festgelegt.

In der folgenden Beispiel laden wir zwei Oberflächen – eine für den visuellen Inhalt und eine für die Schattenkopie-Maske:

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

![Verbundene Web-Image mit dem Kreis maskiert Schlagschatten](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animieren von

Als standard in der visuellen Ebene ist, können die DropShadow-Eigenschaften verwenden: Kompositionsanimationen animiert werden. Im folgenden ändern wir den Code aus der ein wenig Präsentationslogik Beispiel oben, um den Weichzeichnerradius für den Schatten zu animieren.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>Shadows in XAML

Wenn Sie einen Schatten komplexere Frameworkelemente hinzufügen möchten, stehen Ihnen mehrere Möglichkeiten, Ihnen das Zusammenarbeiten mit Schatten zwischen XAML und dem Kompositionsthread:

1. Verwenden der [DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) in das Windows-Community-Toolkit verfügbar. Finden Sie unter den [DropShadowPanel Dokumentation](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel) ausführliche Anleitungen verwenden.
1. Erstellen Sie eine Visualisierung verwenden als Host für die Schattenkopien und Binde ihn dem Handout XAML Visual.
1. Verwenden Sie die Kompositions-Beispielkatalog [SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) benutzerdefiniertes CompositionShadow-Steuerelement. In diesem Beispiel für die Nutzung wird angezeigt.

## <a name="performance"></a>Leistung

Obwohl der visuellen Ebene zahlreiche Optimierungen, die Auswirkungen auf effiziente und Benutzerfreundlicher machen verfügt, möglich Generieren von Schatten ein relativ teurer Vorgang je nachdem welche Optionen, die Sie festlegen. Im folgenden sind high Level "Kosten" für verschiedene Arten von Schatten. Beachten Sie, dass obwohl bestimmte Shadows teuer sein können sie immer noch nur selten in bestimmten Szenarien verwendet werden können.

Volumeschattenkopie-Eigenschaften| Kosten
------------- | -------------
„Rechteckiges Ausschneiden“    | Niedrig
Shadow.Mask      | Hoch
CompositionDropShadowSourcePolicy.InheritFromVisualContent | Hoch
Statische Blur Radius | Niedrig
Animieren von Blur Radius | Hoch

## <a name="additional-resources"></a>Weitere Ressourcen

- [Komposition DropShadow-API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs-GitHub-Repository](https://github.com/Microsoft/WindowsUIDevLabs)