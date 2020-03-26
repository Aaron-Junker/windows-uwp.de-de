---
title: Verwenden von Inertia-Modifiern zum Erstellen von Einrastpunkten
description: Lernen Sie, wie man mit dem InertiaModifier-Feature eines InteractionTracker Bewegungserlebnisse erzeugen kann, die an einem bestimmten Punkt einrasten.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 46f20a4f63decfad063332d2e8e494c15563e398
ms.sourcegitcommit: f2832e1e04cbf472f7fd51c08144489c510ff470
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "80270395"
---
# <a name="create-snap-points-with-inertia-modifiers"></a>Erstellen von Einrastpunkten mit Inertia-Modifiern

In diesem Artikel gehen wir näher darauf ein, wie man mit dem InertiaModifier-Feature eines InteractionTracker Bewegungserlebnisse erzeugen kann, die an einem bestimmten Punkt einrasten.

## <a name="prerequisites"></a>Erforderliche Komponenten

Wir gehen hier davon aus, dass Sie mit den in diesen Artikeln behandelten Konzepten vertraut sind:

- [Eingabe gesteuerte Animationen](input-driven-animations.md)
- [Benutzerdefinierte Manipulations Erfahrung mit interaktiontracker](interaction-tracker-manipulations.md)
- [Beziehungs basierte Animationen](relation-animations.md)

## <a name="what-are-snap-points-and-why-are-they-useful"></a>Was sind Einrastpunkte und warum sind sie nützlich?

Beim Erstellen benutzerdefinierter Manipulationserfahrungen ist es manchmal hilfreich, spezielle _Positionspunkte_ innerhalb der scrollbaren/zoomfähigen Canvas zu erstellen, an denen InteractionTracker immer zur Ruhe kommt. Diese werden häufig als _Einrastpunkte_ bezeichnet.

Beachten Sie im folgenden Beispiel, wie das Scrollen die Benutzeroberfläche zwischen den verschiedenen Bildern an einer ungünstigen Position belassen kann:

![Scrollen ohne Einrastpunkte](images/animation/snap-points-none.gif)

Wenn Sie Einrastpunkte hinzufügen, „rasten“ sie an einer bestimmten Position ein, wenn Sie das Scrollen zwischen den Bildern stoppen. Mit Einrastpunkten wird das Scrollen durch Bilder viel sauberer und schneller.

![Scrollen mit einem einzelnen Einrastpunkt](images/animation/snap-points-single.gif)

## <a name="interactiontracker-and-inertiamodifiers"></a>InteractionTracker und InertiaModifier

Wenn Sie mit InteractionTracker benutzerdefinierte Manipulationserfahrungen erstellen, können Sie mithilfe von InertiaModifiern Einrastpunkt-Erlebnisse erzeugen. InertiaModifier sind im Wesentlichen eine Möglichkeit, um zu definieren, wo oder wie der InteractionTracker sein Ziel erreicht, wenn er in den Inertia-Zustand eintritt. Sie können InertiaModifier anwenden, um die X- oder Y-Position oder Scale Eigenschaften von InteractionTracker zu beeinflussen.

Es gibt 3 Arten von InertiaModifiern:

- InteractionTrackerInertiaRestingValue – Eine Möglichkeit, die Endposition nach einer Interaktion oder programmatischen Beschleunigung zu verändern. Eine vordefinierte Bewegung bringt den InteractionTracker in diese Position.
- InteractionTrackerInertiaMotion – Eine Möglichkeit zur Definition einer bestimmten Bewegung, die InteractionTracker nach einer Interaktion oder programmatische Beschleunigung durchführt. Aus dieser Bewegung wird die endgültige Position abgeleitet.
- InteractionTrackerInertiaNaturalMotion – Eine Möglichkeit, die endgültige Ruheposition nach einer Interaktion oder programmatischen Beschleunigung zu definieren, aber mit einer physikalisch basierten Animation (NaturalMotionAnimation).

InteractionTracker wertet bei Inertia alle ihm zugeordneten InertiaModifier aus und stellt fest, ob einer von ihnen zutrifft. Das bedeutet, dass Sie mehrere InertiaModifier anlegen und einem InteractionTracker zuweisen können:

1. Definieren der Bedingung – Ein Ausdruck, der die bedingte Anweisung definiert, wann dieser spezifische InertiaModifier stattfinden soll. Dies erfordert oft einen Blick auf die NaturalRestingPosition des InteractionTracker (Standard-Inertia des Ziels).
1. Definieren von RestingValue/Motion/NaturalMotion – Definieren Sie den tatsächlichen Resting Value Expression, Motion Expression oder NaturalMotionAnimation, der bei Erfüllung der Bedingung stattfindet.

> [!NOTE]
> Der Zustandsaspekt des InertiaModifier wird nur einmalig ausgewertet, wenn der InteractionTracker in Inertia eintritt. Allerdings wird nur bei InertiaMotion der Motion-Expression für jeden Frame des Modifikators, dessen Bedingung wahr ist, ausgewertet.

## <a name="example"></a>Beispiel

Sehen wir uns nun an, wie Sie InertiaModifier verwenden können, um einige Einrastpunkte zu erzeugen, mit denen Sie die scrollende Canvas mit Bildern nachstellen können. In diesem Beispiel soll jede Manipulation potentiell durch ein einzelnes Bild gehen – dies wird oft als „Single Mandatory Snap Points“ bezeichnet.

Beginnen wir mit der Einrichtung von InteractionTracker, der VisualInteractionSource und dem Ausdruck, der die Position von InteractionTracker nutzen wird.

```csharp
private void SetupInput()
{
    _tracker = InteractionTracker.Create(_compositor);
    _tracker.MinPosition = new Vector3(0f);
    _tracker.MaxPosition = new Vector3(3000f);

    _source = VisualInteractionSource.Create(_root);
    _source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    _source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
    _tracker.InteractionSources.Add(_source);

    var scrollExp = _compositor.CreateExpressionAnimation("-tracker.Position.Y");
    scrollExp.SetReferenceParameter("tracker", _tracker);
    ElementCompositionPreview.GetElementVisual(scrollPanel).StartAnimation("Offset.Y", scrollExp);
}
```

Da ein einzelnes obligatorisches Einrastpunktverhalten den Inhalt entweder nach oben oder unten verschieben wird, benötigen Sie zwei verschiedene Inertia-Modifier: einen, der den Inhalt nach oben und einen, der nach unten bewegt.

```csharp
// Snap-Point to move the content up
var snapUpModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
// Snap-Point to move the content down
var snapDownModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
```

Ob das Einrasten Auf- oder Abwärts erfolgen soll, hängt davon ab, wo der InteractionTracker im Verhältnis zum Einrastabstand – dem Abstand zwischen den Einrastpositionen – auf natürliche Weise landen würde. Wenn die Hälfte der Strecke überschritten ist, geht es nach unten. Andernfalls nach oben. (In diesem Beispiel speichern Sie den Einrastabstand in einem PropertySet)

```csharp
// Is NaturalRestingPosition less than the halfway point between Snap Points?
snapUpModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y < (this.StartingValue - " + 
"mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapUpModifier.Condition.SetReferenceParameter("prop", _propSet);
// Is NaturalRestingPosition greater than the halfway point between Snap Points?
snapDownModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y >= (this.StartingValue - " + 
"mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapDownModifier.Condition.SetReferenceParameter("prop", _propSet);
```

Dieses Diagramm gibt eine visuelle Beschreibung der Logik, die gerade geschieht:

![Inertia-Modifier-Diagramm](images/animation/inertia-modifier-diagram.png)

Nun müssen Sie nur noch die Resting-Values für jeden InertiaModifier definieren: entweder bewegen Sie die Position des InteractionTracker in die vorherige oder die nächste Einrastposition.

```csharp
snapUpModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue - mod(this.StartingValue, prop.snapDistance)");
snapUpModifier.RestingValue.SetReferenceParameter("prop", _propSet);
snapDownModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue + prop.snapDistance - mod(this.StartingValue, " + 
"prop.snapDistance)");
snapDownModifier.RestingValue.SetReferenceParameter("prop", _propSet);
```

Fügen Sie schließlich die InertiaModifier zu InteractionTracker hinzu. Wenn InteractionTracker nun in den InertiaState eintritt, wird er die Bedingungen Ihrer InertiaModifier prüfen, um zu sehen, ob seine Position geändert werden sollte.

```csharp
var modifiers = new InteractionTrackerInertiaRestingValue[] { 
snapUpModifier, snapDownModifier };
_tracker.ConfigurePositionYInertiaModifiers(modifiers);
```