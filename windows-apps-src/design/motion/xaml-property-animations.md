---
title: XAML-Eigenschafts Animationen
description: Erfahren Sie, wie Sie Eigenschaften für ein XAML-UIElement direkt mithilfe von universelle Windows-Plattform (UWP)-Kompositions Animationen animieren.
ms.date: 09/13/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a7ab119df407b45fb391aebf50df5d85f89ec0f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238295"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animieren von XAML-Elementen mit Kompositions Animationen

In diesem Artikel werden neue Eigenschaften vorgestellt, mit denen Sie ein XAML-UIElement mit der Leistung von Kompositions Animationen und der einfachen Festlegung von XAML-Eigenschaften animieren können.

Vor Windows 10, Version 1809, hatten Sie zwei Möglichkeiten, um Animationen in ihren UWP-apps zu erstellen:

- Verwenden Sie XAML-Konstrukte, wie z. b. [Storyboarding-Animationen](storyboarded-animations.md), oder die Klassen _*_ die-und *-Programm _Animation_ im [Windows. UI. XAML. Media. Animation](/uwp/api/windows.ui.xaml.media.animation) -Namespace.
- Verwenden Sie Kompositions Animationen wie unter [Verwenden der visuellen Ebene mit XAML](../../composition/using-the-visual-layer-with-xaml.md)beschrieben.

Die Verwendung der visuellen Ebene bietet eine bessere Leistung als die Verwendung der XAML-Konstrukte. Wenn Sie [elementcompositionpreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) verwenden, um das zugrunde liegende [Visual](/uwp/api/windows.ui.composition.visual) -Kompositions Objekt des Elements zu erhalten, und dann die Visualisierung mit Kompositions Animationen animieren, ist die Verwendung komplexer.

Ab Windows 10, Version 1809, können Sie die Eigenschaften für ein UIElement direkt mithilfe von Kompositions Animationen animieren, ohne dass Sie das zugrunde liegende Kompositions visuelle Element erhalten müssen.

> [!NOTE]
> Um diese Eigenschaften für UIElement zu verwenden, muss die Zielversion des UWP-Projekts 1809 oder höher sein. Weitere Informationen zum Konfigurieren der Projekt Version finden Sie unter [Version Adaptive apps](../../debug-test-perf/version-adaptive-apps.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn die <strong style="font-weight: semi-bold">XAML</strong> -Steuerelement Katalog-App installiert ist, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/XamlCompInterop">die APP zu öffnen, und sehen Sie sich die Animation Interop in Aktion an</a></p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Neue Renderingeigenschaften ersetzen alte Renderingeigenschaften

Diese Tabelle zeigt die Eigenschaften, die Sie verwenden können, um das Rendering eines UIElement zu ändern, das auch mit einer [compositionanimation](/uwp/api/windows.ui.composition.compositionanimation)animiert werden kann.

| Eigenschaft | type | BESCHREIBUNG |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | Der Grad der Deckkraft des Objekts. |
| [Übersetzung](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | X/Y/Z-Position des Elements verschieben |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | Die Transformationsmatrix, die auf das Element angewendet werden soll. |
| [Skalieren](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Skalieren des Elements, zentriert am Mittelpunkt |
| [Drehung](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | Drehen Sie das Element um die RotationAxis und den Centerpoint. |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | Die Achse der Drehung. |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | Mittelpunkt der Skalierung und Drehung |

Der transformMatrix-Eigenschafts Wert wird in der folgenden Reihenfolge mit den Eigenschaften für Skalierung, Drehung und Übersetzung kombiniert: transformMatrix, Skalierung, Drehung, Übersetzung.

Diese Eigenschaften wirken sich nicht auf das Layout des Elements aus. das Ändern dieser Eigenschaften führt daher [Measure](/uwp/api/windows.ui.xaml.uielement.measure)nicht zum / [anordnen](/uwp/api/windows.ui.xaml.uielement.arrange) eines neuen Measures.

Diese Eigenschaften haben den gleichen Zweck und das gleiche Verhalten wie die like-Named-Eigenschaften in der [visuellen](/uwp/api/windows.ui.composition.visual) Kompositionsklasse (mit Ausnahme der Übersetzung, die nicht in der Visualisierung enthalten ist).

### <a name="example-setting-the-scale-property"></a>Beispiel: Festlegen der Scale-Eigenschaft

In diesem Beispiel wird gezeigt, wie die Scale-Eigenschaft für eine Schaltfläche festgelegt wird.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Gegenseitige Exklusivität zwischen neuen und alten Eigenschaften

> [!NOTE]
> Die **Opacity** -Eigenschaft erzwingt nicht die gegenseitige Exklusivität, die in diesem Abschnitt beschrieben wird. Sie verwenden die gleiche Opacity-Eigenschaft, unabhängig davon, ob Sie XAML-oder Kompositions Animationen verwenden.

Die Eigenschaften, die mit einer compositionanimation animiert werden können, sind Ersetzungen für mehrere vorhandene UIElement-Eigenschaften:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projektion](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Wenn Sie eine der neuen Eigenschaften festlegen (oder animieren), können Sie die alten Eigenschaften nicht verwenden. Wenn Sie hingegen eine der alten Eigenschaften festlegen (oder animieren), können Sie die neuen Eigenschaften nicht verwenden.

Sie können auch die neuen Eigenschaften nicht verwenden, wenn Sie elementcompositionpreview verwenden, um das visuelle Element mithilfe der folgenden Methoden zu erhalten und zu verwalten:

- [Elementcompositionpreview. getelementvisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [Elementcompositionpreview./tistranslationaktivierte](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> Wenn Sie versuchen, die Verwendung der beiden Eigenschaften Sätze zu kombinieren, führt dies dazu, dass der API-Aufrufe fehlschlägt und eine Fehlermeldung erzeugt.

Es ist möglich, aus einem Satz von Eigenschaften zu wechseln, indem Sie Sie löschen. aus Gründen der Einfachheit wird dies jedoch nicht empfohlen. Wenn die Eigenschaft durch eine DependencyProperty (z. b. "UIElement. Projection" unterstützt durch "UIElement. projectionproperty") unterstützt wird, wird ClearValue aufgerufen, um den Status "nicht verwendet" wiederherzustellen. Andernfalls (z. b. die Scale-Eigenschaft) legen Sie die-Eigenschaft auf ihren Standardwert fest.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animieren von UIElement-Eigenschaften mit compositionanimation

Sie können die in der Tabelle aufgeführten Renderingeigenschaften mit einer compositionanimation animieren. Auf diese Eigenschaften kann auch durch eine [expressionanimation](/uwp/api/windows.ui.composition.expressionanimation)verwiesen werden.

Verwenden Sie die Methoden [startanimation](/uwp/api/windows.ui.xaml.uielement.startanimation) und [stopanimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) für UIElement, um die UIElement-Eigenschaften zu animieren.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Beispiel: Animieren der Scale-Eigenschaft mit einem Vector3KeyFrameAnimation

Dieses Beispiel zeigt, wie Sie die Skalierung einer Schaltfläche animieren.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Beispiel: Animieren der Scale-Eigenschaft mit einer expressionanimation

Eine Seite verfügt über zwei Schaltflächen. Mit der zweiten Schaltfläche wird eine Animation als erste Schaltfläche erstellt.

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

## <a name="related-topics"></a>Zugehörige Themen

- [Storyboardanimationen](storyboarded-animations.md)
- [Benutzung des Visual Layer mit XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Übersicht über Transformationen](../layout/transforms.md)
