---
title: Feder-Animationen
description: Lernen Sie, wie natürliche Spring-Bewegungsanimationen verwenden.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 9e00aa383bcce17b7cd6b67514647c2f6137cc32
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601945"
---
# <a name="spring-animations"></a>Feder-Animationen

Der Artikel zeigt, wie man Feder-NaturalMotionAnimations verwendet.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen hier davon aus, dass Sie mit den in diesen Artikeln behandelten Konzepten vertraut sind:

- [Natürliche Animationen](natural-animations.md)

## <a name="why-springs"></a>Warum Federanimationen?

Federanimationen sind ein allgemeines Bewegungserlebnis, das wir alle irgendwann einmal in unserem Leben erlebt haben; von Spielzeug bis hin zum Physikunterricht mit einem Federblock. Die oszillierende Bewegung einer Feder löst oft eine spielerische und heitere emotionale Reaktion der Betrachter aus. Das Ergebnis ist, dass die Bewegung einer Feder sich gut in eine Benutzeroberfläche für die Anwendung übersetzen lässt, die ein lebendigeres Bewegungserlebnis schaffen möchte, das dem Endbenutzer mehr überzeugt als eine traditionelle Cubic-Bezier-Animation. In diesen Fällen sorgt die Federbewegung nicht nur für ein lebendigeres Bewegungserlebnis, sondern kann auch die Aufmerksamkeit auf neue oder gerade animierte Inhalte lenken. Je nach Anwendungs-Branding oder Bewegungssprache ist die Federbewegung ausgeprägter und sichtbarer und in anderen Fällen subtiler.

![Während der Übertragung mit Spring-Animation](images/animation/offset-spring.gif)
![während der Übertragung mit kubische Bezier-Animation](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>Verwendung von Federbewegungen in Ihrem UI

Wie bereits erwähnt, können Federanimationen eine nützliche Bewegung sein, um sich in Ihre App zu integrieren und ein sehr vertrautes und spielerisches UI-Erlebnis einzuführen. Übliche Verwendung von Federanimationen in UIs sind:

| Beschreibung der Verwendung der Federanimation | Visuelles Beispiel |
| ------------------------ | -------------- |
| Eine Bewegungserfahrung „herausstechen“ und lebendiger aussehen lassen. (Animating Scale) | ![Skalierungsbewegung mit Federanimation](images/animation/scale-spring.gif) |
| Eine Bewegungserfahrung subtil energetischer wirken lassen (Animating Offset) | ![Offset-Bewegung mit Federanimation](images/animation/offset-spring.gif) |

In jedem dieser Fälle kann die Federbewegung entweder durch „Auffedern“ und Schwingen um einen neuen Wert oder Schwingen um den aktuellen Wert mit einer Anfangsgeschwindigkeit ausgelöst werden.

![Federanimation-Schwingung](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>Definition Ihrer Federbewegung

Mit den NaturalMotionAnimation-APIs schaffen Sie eine Federbewegung. Insbesondere erstellen Sie eine SpringNaturalMotionAnimation mit den Create*-Methoden des Compositors. Anschließend können Sie die folgenden Eigenschaften der Bewegung definieren:

- DampingRatio – Gibt den Dämpfungsgrad der in der Animation verwendeten Federbewegung an.

| Damping-Ratio-Wert | Beschreibung |
| ------------------- | ----------- |
| DampingRatio = 0 | Nicht gedämpft – Feder wird für längere Zeit schwingen |
| 0 < DampingRatio < 1 | Wenig gedämpft – Feder wird von leicht zu viel schwingen |
| DampingRatio = 1 | Criticallydamped – Feder führ keine Schwingung aus |
| DampingRation > 1 | Stark gedämpft – Feder erreicht schnell ihr Ziel mit einer abrupten Verlangsamung und keine Schwingung |

- Period – Die Zeit zum Ausführen eines einzelnen Schwingungsvorgangs.
- Final-/Starting-Wert – Definierte Start- und Endpositionen der Federbewegung (wenn nicht definiert, wird Startwert bzw. Endwert aktueller Wert sein).
- Initial Velocity – Programmatische Anfangsgeschwindigkeit für die Bewegung.

Sie können auch eine Reihe von Eigenschaften der Bewegung definieren, die mit KeyFrameAnimations identisch sind:

- DelayTime/Delay Behavior
- StopBehavior

In den üblichen Fällen, in denen Offset und Skalierung/Größe animiert werden, werden die folgenden Werte für DampingRatio und Period für verschiedene Federtypen vom Windows Design Team empfohlen:

| Eigenschaft | Normal Federung | Gedämpfte Federung | Weniger gedämpfte Federung |
| -------- | ------------- | --------------- | -------------------- |
| Offset | Damping Ratio = 0,8 <br/> Period = 50 ms | Damping Ratio = 0,85 <br/> Period = 50 ms | Damping Ratio = 0,65 <br/> Period = 60 ms |
| Skalierung/Größe | Damping Ratio = 0,7 <br/> Period = 50 ms | Damping Ratio = 0,8 <br/> Period = 50 ms | Damping Ratio = 0,6 <br/> Period = 60 ms |

Nachdem Sie die Eigenschaften definiert haben, können Sie Ihre Feder-NaturalMotionAnimation an die Methode StartAnimation eines CompositionObjects oder die Motion-Eigenschaft eines InteractionTracker InertiaModifier übergeben.

## <a name="example"></a>Beispiel

In diesem Beispiel erstellen Sie eine Navigations- und Canvas-Benutzeroberfläche, in der ein Navigationsbereich durch Klicken auf eine Erweitern-Schaltfläche mit einer federnden Pendelbewegung animiert wird.

![Federanimation per Klick](images/animation/spring-animation-on-click.gif)

Definieren Sie zunächst die Federanimation innerhalb des angeklickten Ereignisses für das Erscheinen des Navigationsbereichs. Anschließend definieren Sie die Eigenschaften der Animation, indem Sie die Funktion InitialValueExpression verwenden, um einen Ausdruck zur Definition des FinalValue zu verwenden. Sie können außerdem verfolgen, ob der Bereich geöffnet ist oder nicht und die Animation starten.

```csharp
private void Button_Clicked(object sender, RoutedEventArgs e)
{
 _springAnimation = _compositor.CreateSpringScalarAnimation();
 _springAnimation.DampingRatio = 0.75f;
 _springAnimation.Period = TimeSpan.FromSeconds(0.5);

 if (!_expanded)
 {
 _expanded = true;
 _propSet.InsertBoolean("expanded", true);
 _springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue + 250”;
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue - 250”;
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

Was wäre, wenn Sie diese Eingabe mit der Bewegung verknüpfen wollten? Wenn der Endbenutzer heraus wischt, soll eine Federbewegung erscheinen. Noch wichtiger ist, wenn der Benutzer härter oder schneller wischt, passt sich die Bewegung an die Geschwindigkeit des Endbenutzers an.

![Federanimation beim Wischen](images/animation/spring-animation-on-swipe.gif)

Dazu können Sie die gleiche Federanimation nehmen und in einen InertiaModifier mit InteractionTracker übergeben. Weitere Informationen zu InputAnimations und InteractionTracker finden Sie unter [Benutzerdefinierte Manipulationserfahrungen mit InteractionTracker](interaction-tracker-manipulations.md). Wir gehen davon aus, dass Sie für dieses Codebeispiel bereits Ihren InteractionTracker und VisualInteractionSource eingerichtet haben. Wir konzentrieren uns auf die Entwicklung der InertiaModifier, die eine NaturalMotionAnimation, in diesem Fall eine Feder, aufnehmen.

```csharp
// InteractionTracker and the VisualInteractionSource previously setup
// The open and close ScalarSpringAnimations defined earlier
private void SetupInput()
{
 // Define the InertiaModifier to manage the open motion
 var openMotionModifer = InteractionTrackerInertiaNaturalMotion.Create(compositor);

 // Condition defines to use open animation if panes in non-expanded view
 // Property set value to track if open or closed is managed in other part of code
 openMotionModifer.Condition = _compositor.CreateExpressionAnimation(
"propset.expanded == false");
 openMotionModifer.Condition.SetReferenceParameter("propSet", _propSet);
 openMotionModifer.NaturalMotion = _openSpringAnimation;

 // Define the InertiaModifer to manage the close motion
 var closeMotionModifier = InteractionTrackerInertiaNaturalMotion.Create(_compositor);

 // Condition defines to use close animation if panes in expanded view
 // Property set value to track if open or closed is managed in other part of code
 closeMotionModifier.Condition = 
_compositor.CreateExpressionAnimation("propSet.expanded == true");
 closeMotionModifier.Condition.SetReferenceParameter("propSet", _propSet);
 closeMotionModifier.NaturalMotion = _closeSpringAnimation;

 _tracker.ConfigurePositionXInertiaModifiers(new 
InteractionTrackerInertiaNaturalMotion[] { openMotionModifer, closeMotionModifier});

 // Take output of InteractionTracker and assign to the pane
 var exp = _compositor.CreateExpressionAnimation("-tracker.Position.X");
 exp.SetReferenceParameter("tracker", _tracker);
 ElementCompositionPreview.GetElementVisual(pageNavigation).
StartAnimation("Translation.X", exp);
}
```

Jetzt haben Sie sowohl eine programmatische als auch eine eingabegesteuerte Federanimation in Ihrer Benutzeroberfläche!

Zusammenfassend sehen die Schritte zur Verwendung von Federanimationen so aus:

1. Erstellen Sie Ihre SpringAnimation aus Ihrem Compositor.
1. Definieren Sie Eigenschaften der SpringAnimation, wenn Sie nicht die Standardwerte wünschen:
    - DampingRatio
    - Zeitraum
    - Final Value
    - Anfangswert
    - Initial Velocity
1. Zum Ziel zuweisen.
    - Wenn Sie eine CompositionObject-Eigenschaft animieren, übergeben Sie SpringAnimation als Parameter an StartAnimation.
    - Wenn Sie mit Eingaben arbeiten wollen, setzen Sie die NaturalMotion-Eigenschaft eines InertiaModifier auf SpringAnimation.

