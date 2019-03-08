---
title: Zeigerbasierte Animationen
description: Lernen Sie, wie man die Position eines Zeigers verwendet, um dynamische „Stick to the Cursor“-Erfahrungen zu erzeugen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 3512d47c8b3e689b0baadec26c1d8f0f510e03ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639925"
---
# <a name="pointer-based-animations"></a>Zeigerbasierte Animationen

Dieser Artikel zeigt, wie man die Position eines Zeigers verwendet, um dynamische „Stick to the Cursor“-Erfahrungen zu erzeugen.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen hier davon aus, dass Sie mit den in diesen Artikeln behandelten Konzepten vertraut sind:

- [Eingabe-driven Animationen](input-driven-animations.md)
- [Beziehung basierend Animationen](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>Warum zeigerpositionsgesteuerte Erlebnisse erstellen?

In der Fluent Design-Sprache ist Touch nicht die einzige Möglichkeit, mit der Benutzeroberfläche zu interagieren. Da sich UWP über mehrere Geräteformfaktoren erstreckt, interagieren Endbenutzer mit Apps mit anderen Eingabemodalitäten wie Maus und Stift. Die Verwendung von Positionsdaten aus diesen anderen Eingabemodalitäten bietet die Möglichkeit, Endbenutzern das Gefühl zu geben, dass sie sich noch mehr mit Ihrer Anwendung verbunden fühlen.

Zeigerpositionsgesteuerte Erlebnisse ermöglichen es Ihnen, die On-Screen-Position einer Pointer-Eingabemodalität zu nutzen, um zusätzliche Motion- und UI-Erlebnisse für Ihre App zu erzeugen. Diese Erfahrungen können den Endbenutzern oft zusätzliche Informationen über das Verhalten und die Struktur der Benutzeroberfläche liefern. Die Erfahrung ist nicht mehr nur ein Einweg-Stream, sondern beginnt zu einem Zweiwege-Stream zu werden, bei dem der Endbenutzer Eingaben mit seiner Eingabemodalität liefert und die App-Oberfläche darauf reagieren kann.

Beispiele:

- Animieren der Position eines Spotlights, um dem Cursor zu folgen

    ![Zeiger-Spotlight-Beispiel](images/animation/spotlight-reveal.gif)

- Drehen eines Bildes basierend auf der Position eines Zeigers

    ![Zeiger-Rotations-Beispiel](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>Verwenden von PointerPositionPropertySet

Diese Erlebnisse können Sie mit dem PointerPositionPropertySet erzeugen. Dieses PropertySet wird für ein UIElement erstellt, um die Position des Zeigers beizubehalten, während das UIElement positiv Hit-getestet wird. Der Positionswert ist relativ zum Koordinatenraum des UIElements (eine Position <0,0> ist die linke obere Ecke des UIElements). Sie können dann diese Eigenschaft, die in einer Animation festgelegt wurde, nutzen, um die Bewegung einer anderen Eigenschaft zu steuern.

Für jeden der anderen Zeiger Eingabemöglichkeiten gibt es eine Anzahl von Eingabe Zustände, in denen die Eingabe sein kann, in dem die Position ändert: Zeigen Sie mit, geklickt, gedrückt und verschoben. Das PointerPositionPropertySet behält nur die Position des Zeigers in den Zuständen „Hover“, „Pressed“ und „Pressed and Moved“ für Mouse und Pen bei.

Allgemeine Schritte für den Einstieg:

1. Identifizieren Sie das UIElement, in dem Sie die Position des Zeigers verfolgen möchten.
1. Zugriff auf das PointerPositionPropertySet über ElementCompositionPreview.
    - Übergeben Sie UIElement in die Methode ElementCompositionPreview.GetPointerPositionPropertySet.
1. Erstellen Sie eine ExpressionAnimation, die auf die Eigenschaft Position im PropertySet verweist.
    - Vergessen Sie nicht, Ihren Referenzparameter einzustellen!
1. Mit der ExpressionAnimation können Sie die Eigenschaft eines CompositionObjects gezielt ansteuern.

> [!NOTE]
> Es wird empfohlen, das PropertySet, das von der Methode GetPointerPositionPropertySet zurückgegeben wird, einer Klassenvariablen zuzuordnen. Dies stellt sicher, dass das Property-Set nicht durch die Garbage-Collection bereinigt wird und hat somit keinen Einfluss auf die ExpressionAnimation, in der es referenziert wird. ExpressionAnimations hat keine starke Referenz auf die in der Gleichung verwendeten Objekte.

## <a name="example"></a>Beispiel

Betrachten wir ein Beispiel, bei dem wir die Hover-Position einer Maus und eines Stifts nutzen, um ein Bild dynamisch zu drehen.

![Zeiger-Rotations-Beispiel](images/animation/pointer-rotate.gif)

Das Bild ist ein UIElement, also lassen Sie uns zuerst einen Verweis auf das PointerPositionPropertySet erhalten.

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

In diesem Beispiel sind zwei Ausdrücke im Spiel:

- Ein Ausdruck, bei dem sich das Bild aufgrund der Entfernung des Mauszeigers von der Bildmitte aus dreht. Je weiter weg, desto mehr Drehung.
- Ein Ausdruck, bei dem sich die Rotationsachse aufgrund der Position des Zeigers ändert. Die Drehachse soll senkrecht zum Positionsvektor stehen.

Definieren wir nun die beiden Ausdrücke. Einen, der auf die RotationAngle-Eigenschaft zielt und den anderen die RotationAxis-Eigenschaft. Sie referenzieren das PointerPositionPropertySet wie jedes andere PropertySet.

In diesem Beispiel erstellen Sie Ausdrücke mit Hilfe der ExpressionBuilder-Klassen.

```csharp
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