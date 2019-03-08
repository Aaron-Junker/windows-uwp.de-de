---
title: Animationen für XAML-Eigenschaft
description: Animieren von XAML-Elementen mit kompositionsanimationen aus.
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 81da1e769ab171e47a4f4046e8ec7e7c84ecf2d1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630355"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animieren von XAML-Elementen mit kompositionsanimationen

Dieser Artikel enthält die neue Eigenschaften, mit denen Sie ein XAML-UIElement mit der Leistung der kompositionsanimationen und die Einfachheit der XAML-Einstellungseigenschaften zu animieren.

Vor Windows 10, Version 1809, mussten Sie 2 Auswahlmöglichkeiten Animationen in UWP-apps zu erstellen:

- Verwenden von XAML-Konstrukten wie [niedergeschrieben Animationen](storyboarded-animations.md), oder die _* ThemeTransition_ und _* ThemeAnimation_ Klassen in der [ Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation) Namespace.
- kompositionsanimationen verwenden, siehe [XAML mit der visuellen Ebene](../../composition/using-the-visual-layer-with-xaml.md).

Mithilfe der visuellen Ebene bietet eine bessere Leistung als mit dem XAML erstellt werden. Während mit ["elementcompositionpreview"](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) das Element abgerufen Komposition zugrunde liegenden [Visual](/uwp/api/windows.ui.composition.visual) Objekt, und klicken Sie dann animieren das visuelle Element mit kompositionsanimationen, ist Sie schwieriger zu verwenden.

Ab Windows 10, Version 1809, können Sie Eigenschaften für ein UIElement, das direkt mit kompositionsanimationen ohne die Anforderung zum Abrufen der zugrunde liegenden Kompositions Visual animieren.

> [!NOTE]
> Um diese Eigenschaften auf "UIElement" verwenden zu können, muss es sich bei der UWP-Projekt-Zielversion 1809 oder höher sein. Weitere Informationen zum Konfigurieren Ihrer Projektversion finden Sie unter [versionsabhängig adaptive apps](../../debug-test-perf/version-adaptive-apps.md).

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Neue Renderingeigenschaften ersetzen alten Renderingeigenschaften

Diese Tabelle zeigt die Eigenschaften können Sie das Rendering eines "UIElement", zu ändern, die auch mit animiert werden, können eine ["compositionanimation"](/uwp/api/windows.ui.composition.compositionanimation).

| Eigenschaft | Typ | Beschreibung |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | Den Grad der Deckkraft des Objekts |
| [Translation](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Verschieben Sie die X/Y/Z-Position des Elements |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | Die Transformationsmatrix, die für das Element gelten |
| [Skalieren](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Skalieren Sie das Element, dessen Mitte sich an den CenterPoint |
| [Drehung](/uwp/api/windows.ui.xaml.uielement.rotation) | Gleitkomma | Das Element, um die RotationAxis und Mittelpunkt gedreht. |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | Die Achse der Drehung |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | Der Mittelpunkt der Skalierung und Drehung |

Der Wert der "transformMatrix"-Eigenschaft wird mit den Eigenschaften für Skalierung, Drehung und Übersetzung in der folgenden Reihenfolge kombiniert:  TransformMatrix, Scale, Rotation, Translation.

Diese Eigenschaften wirken sich nicht auf das Element des Layouts, so dass die Änderung dieser Eigenschaften nicht dazu, dass ein neues [Measure](/uwp/api/windows.ui.xaml.uielement.measure)/[anordnen](/uwp/api/windows.ui.xaml.uielement.arrange) übergeben.

Diese Eigenschaften verfügen über denselben Zweck und dasselbe Verhalten wie die Eigenschaften mit dem von der Zusammensetzung [Visual](/uwp/api/windows.ui.composition.visual) Klasse (mit Ausnahme der Übersetzung, die auf Visual ist).

### <a name="example-setting-the-scale-property"></a>Beispiel: Festlegen der Eigenschaft "Skalierung"

Dieses Beispiel zeigt, wie Sie die Eigenschaft "Skalierung" in einer Schaltfläche festzulegen.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Gegenseitige Ausschließlichkeit zwischen den alten und neuen Eigenschaften

> [!NOTE]
> Die **Deckkraft** Eigenschaft erzwingt nicht die gegenseitige Exklusivität, die in diesem Abschnitt beschrieben. Sie verwenden die gleiche Opacity-Eigenschaft, ob Sie mit XAML oder Komposition Animationen.

Die Eigenschaften, die mit einem "compositionanimation" animiert werden können, sind Ersetzungen für mehrere vorhandene Eigenschaften von "UIElement":

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projektion](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Beim festlegen (oder animiert) keines der neuen Eigenschaften, können nicht Sie die alte Eigenschaften verwenden. Wenn Sie festlegen (oder eine Animation) die alten Eigenschaften, können nicht Sie dagegen die neuen Eigenschaften verwenden.

Sie können nicht die neuen Eigenschaften auch verwenden, wenn Sie die "elementcompositionpreview" zum Abrufen und Verwalten der Visualisierung selbst die Verwendung dieser Methoden verwenden:

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> Es wird versucht, die die Verwendung der beiden Sätze von Eigenschaften kombinieren führt dazu, dass die API-Aufruf einen Fehler, und erstellen eine Fehlermeldung angezeigt.

Es ist möglich, wechseln aus einem Satz von Eigenschaften durch das Löschen, aber aus Gründen der Einfachheit nicht empfohlen wird. Wenn die Eigenschaft von DependencyProperty unterstützt wird (z. B. UIElement.Projection durch UIElement.ProjectionProperty unterstützt wird), rufen Sie dann auf ClearValue, um es in den Zustand "nicht verwendet" wiederherzustellen. Andernfalls (z. B. die Eigenschaft "Skalierung"), legen Sie die-Eigenschaft auf den Standardwert zurück.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animieren von Eigenschaften von "UIElement" mit "compositionanimation"

Sie können die Renderingeigenschaften, die in der Tabelle mit einer "compositionanimation" aufgeführten animieren. Diese Eigenschaften können auch verweisen eine [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Verwenden der ["startanimation"](/uwp/api/windows.ui.xaml.uielement.startanimation) und [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) Methoden für "UIElement", um die Eigenschaften von "UIElement" zu animieren.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Beispiel: Animieren der Eigenschaft "Skalierung" mit einem Vector3KeyFrameAnimation

Dieses Beispiel zeigt, wie Sie die Skalierung einer Schaltfläche zu animieren.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Beispiel: Animieren der Eigenschaft "Skalierung" mit einem ExpressionAnimation

Eine Seite verfügt über zwei Schaltflächen. Die zweite Schaltfläche animiert wird, um doppelt so groß ist (über skalieren) werden wie die erste Schaltfläche.

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>Verwandte Themen

- [Niedergeschrieben Animationen](storyboarded-animations.md)
- [Mithilfe der visuellen Ebene mit XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Übersicht über Transformationen](../layout/transforms.md)
