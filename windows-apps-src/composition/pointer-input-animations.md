---
title: Zeigerbasierte Animationen
description: Erfahren Sie, wie Sie die Position eines Zeigers verwenden können, um die Benutzeroberfläche "an Cursor halten" zu erstellen.
ms.date: 03/23/2021
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: 80fd46af6f84d4a23329ad34fc77a14faf97b913
ms.sourcegitcommit: 34f532fd023af2849c3e975baf7aa6771d7e53b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104893860"
---
# <a name="pointer-based-animations"></a>Zeigerbasierte Animationen

In diesem Artikel wird gezeigt, wie die Position eines Zeigers verwendet wird, um die Benutzeroberfläche "an Cursor halten" zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen davon aus, dass Sie mit den Konzepten vertraut sind, die in den folgenden Artikeln erläutert werden:

- [Eingabegesteuerte Animationen](input-driven-animations.md)
- [Auf Beziehungen basierende Animationen](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>Warum werden Zeiger Position-Driven Erfahrungen erstellt?

In der [Sprache "fließend Design](/windows/apps/fluent-design-system)" ist die Berührungs Methode nicht die einzige Möglichkeit, mit der Benutzeroberfläche zu interagieren. Da UWP sich über mehrere Geräte Formfaktoren erstreckt, interagieren Endbenutzer mit apps mit anderen Eingabe Modalitäten wie Maus und Stift. Die Verwendung von Positionsdaten aus diesen anderen Eingabe Modalitäten bietet die Möglichkeit, Endbenutzern eine noch größere Verbindung mit Ihrer APP zu ermöglichen.

Mithilfe von Zeiger Positions gesteuerten Oberflächen können Sie die Bildschirmposition einer zeigereingabemodalität nutzen, um zusätzliche Bewegungs-und Benutzeroberflächen Umgebungen für Ihre APP zu erstellen. Diese Umgebungen können Endbenutzern in Bezug auf das Verhalten und die Struktur der Benutzeroberfläche häufig zusätzlichen Kontext und Feedback bereitstellen. Die Oberfläche ist kein unidirektionaler Stream, sondern wird stattdessen zu einem bidirektionalen Stream, in dem der Endbenutzer Eingaben mit der eingegebenen Modalität bereitstellt und die Benutzeroberfläche der APP zurück reagieren kann.

Beispiele hierfür sind:

- Animieren der Position eines [Spotlight](/uwp/api/windows.ui.composition.spotlight) , um dem Cursor zu folgen

    ![Beispiel für Zeiger Spotlight](images/animation/spotlight-reveal.gif)

- Drehen eines Bilds basierend auf der Position eines Zeigers

    ![Beispiel für Zeiger Drehung](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>Verwenden von pointerpositionpropertyset

Sie können diese Benutzeroberflächen mithilfe von pointerpositionpropertyset erstellen. Dieses PropertySet wird für ein UIElement erstellt, um die Position des Zeigers beizubehalten, während das UIElement positiv auf Treffer getestet wird. Der Positionswert ist relativ zum Koordinaten Bereich des UIElement-Elements (die Position <0, 0> die linke obere Ecke des UIElement-Elements). Anschließend können Sie diese Eigenschaft in einer Animation verwenden, um die Bewegung einer anderen Eigenschaft zu steuern.

Für jede der unterschiedlichen Zeiger Eingabe Modalitäten gibt es eine Reihe von Eingabe Zuständen, an denen sich die Position ändert: hover, gedrückt, gedrückt & verschoben. Das pointerpositionpropertyset behält nur die Position des Zeigers in den Hover-, gedrückten und gedrückten und verschobenen Zuständen für Maus und Stift.

Allgemeine Schritte für den Einstieg:

1. Identifizieren Sie das UIElement-Element, in dem die Position des Zeigers nachverfolgt werden soll.
1. Greifen Sie über elementcompositionpreview auf das pointerpositionpropertyset zu.
    - Übergeben Sie UIElement an die [elementcompositionpreview. getpointerpositionpropertyset](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getpointerpositionpropertyset) -Methode.
1. Erstellen Sie eine expressionanimation, die auf die Positions Eigenschaft in der PropertySet-Klasse verweist.
    - Vergessen Sie nicht, ihren Verweis Parameter festzulegen.
1. Richten Sie die-Eigenschaft eines compositionobject mit der expressionanimation ein.

> [!NOTE]
> Es wird empfohlen, dass Sie das von der getpointerpositionpropertyset-Methode zurückgegebene PropertySet einer Klassen Variablen zuweisen. Dadurch wird sichergestellt, dass der Eigenschaften Satz nicht von der Garbage Collection bereinigt wird und somit keine Auswirkung auf die expressionanimation hat, in der auf Sie verwiesen wird. Expressionanimationen behalten keinen starken Verweis auf eines der in der Gleichung verwendeten-Objekte bei.

## <a name="example"></a>Beispiel

Sehen wir uns ein Beispiel an, in dem wir die Hover-Position der Maus-und Stift Eingabe-Modalität zum dynamischen Drehen eines Bilds nutzen.

![Beispiel für Zeiger Drehung](images/animation/pointer-rotate.gif)

Das Bild ist ein UIElement. wir erhalten also zuerst einen Verweis auf das pointerpositionpropertyset.

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

In diesem Beispiel gibt es zwei Ausdrücke:

- Ein Ausdruck, bei dem das Bild abhängig von der Mitte des Bilds rotiert. Der weiter Weg, umso mehr Rotation.
- Ein Ausdruck, bei dem die Drehungs Achse basierend auf der Position des Zeigers geändert wird. Sie möchten, dass die Drehungs Achse senkrecht zum Vektor der Position ist.

Hier definieren Sie die beiden Ausdrücke, eine für die RotationAngle-Eigenschaft und die andere die RotationAxis-Eigenschaft. Sie verweisen auf das pointerpositionpropertyset wie auf jedes andere PropertySet.

In diesem Beispiel werden Ausdrücke mithilfe der [ExpressionBuilder](/windows/communitytoolkit/animations/expressions) -Klassen aufgebaut.

```csharp
// using EF = ExpressionBuilder.ExpressionFunctions;
// || DEFINE THE EXPRESSION FOR THE ROTATION ANGLE ||
var hoverPosition = _pointerPositionPropSet.GetSpecializedReference
<PointerPositionPropertySetReferenceNode>().Position;
var angleExpressionNode =
EF.Conditional(
 hoverPosition == new Vector3(0, 0, 0),
 ExpressionValues.CurrentValue.CreateScalarCurrentValue(),
 35 * ((EF.Clamp(EF.Distance(center, hoverPosition), 0, distanceToCenter) % distanceToCenter) / distanceToCenter));
_tiltVisual.StartAnimation("RotationAngleInDegrees", angleExpressionNode);

// || DEFINE THE EXPRESSION FOR THE ROTATION AXIS ||
var axisAngleExpressionNode = EF.Vector3(
-(hoverPosition.Y - center.Y) * EF.Conditional(hoverPosition.Y == center.Y, 0, 1),
 (hoverPosition.X - center.X) * EF.Conditional(hoverPosition.X == center.X, 0, 1),
 0);
_tiltVisual.StartAnimation("RotationAxis", axisAngleExpressionNode);
```

## <a name="see-also"></a>Siehe auch

- [Beispiel für Zeiger Drehung](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK%2015063/PointerRotate)
- [Pointerpositionpropertyeintreferencenode-Klasse](/dotnet/api/microsoft.toolkit.uwp.ui.animations.expressions.pointerpositionpropertysetreferencenode?view=win-comm-toolkit-dotnet-7.0&preserve-view=true)