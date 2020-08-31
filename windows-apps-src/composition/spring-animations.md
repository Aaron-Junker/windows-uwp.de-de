---
title: Federanimationen
description: Erfahren Sie, wie Sie mithilfe der naturalmotionanimation-APIs Spring Motion-Erfahrungen in ihren apps erstellen.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: ecfb6fc001fbf42f70d40ee16abc45aa221c0a75
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053870"
---
# <a name="spring-animations"></a>Federanimationen

Der Artikel zeigt, wie Spring naturalmotionanimationen verwendet werden.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen davon aus, dass Sie mit den Konzepten vertraut sind, die in den folgenden Artikeln erläutert werden:

- [Naturbewegungs Animationen](natural-animations.md)

## <a name="why-springs"></a>Warum?

Quellen sind eine gängige Bewegungserfahrung, die wir zu einem beliebigen Zeitpunkt im Leben haben. von den Bereichen "Slinky Toys" bis "Physik Classroom" mit einem Spring-gebundenen Block. Die oskaerende Bewegung einer Spring-Bewegung erbt häufig eine spielerische und helle emotionale Reaktion von denjenigen, die Sie beobachten. Folglich übersetzt die Bewegung einer Spring-Bewegung die Benutzeroberfläche der Anwendung für Benutzer, die eine lebendige Bewegungs Oberfläche erstellen möchten, die dem Endbenutzer mehr als eine herkömmliche kubische Bezier-Funktion erscheint. In diesen Fällen erzeugt Spring Motion nicht nur eine lebendige Bewegungs Darstellung, sondern auch die Aufmerksamkeit auf neue oder derzeit animierte Inhalte. Abhängig vom Anwendungs Branding oder der Bewegungssprache ist die Schwingung deutlicher und sichtbar, aber in anderen Fällen ist Sie feiner.

![Motion with Spring Animation ](images/animation/offset-spring.gif)
 ![ Motion with kubische Bezier-Animation](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>Verwenden von Quellen in der Benutzeroberfläche

Wie bereits erwähnt, können Quellen eine nützliche Bewegung für die Integration in Ihre APP sein, um eine sehr vertraute und Spiel Bare Benutzeroberfläche bereitzustellen. Allgemeine Verwendung von Quellen in der Benutzeroberfläche:

| Beschreibung der Spring-Verwendung | Visuelles Beispiel |
| ------------------------ | -------------- |
| Einen Bewegungs Vorgang für "Pop" durchführen und die Lebendigkeit sehen. (Animieren der Skala) | ![Bewegung mit Spring Animation skalieren](images/animation/scale-spring.gif) |
| Eine Bewegungserfahrung mit einem sehr viel kräftigsten Verhalten zu gestalten (Animation Offset) | ![Offset Bewegung mit Spring Animation](images/animation/offset-spring.gif) |

In jedem dieser Fälle kann die Spring-Bewegung ausgelöst werden, indem "Spring to" ausgelöst wird, und um einen neuen Wert zu bewegen oder um den aktuellen Wert durch eine anfängliche Geschwindigkeit zu durchlaufen.

![Spring Animation-osziierung](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>Definieren von Spring Motion

Sie erstellen eine Spring-Darstellung mithilfe der naturalmotionanimation-APIs. Insbesondere erstellen Sie eine springnaturalmotionanimation, indem Sie die Create *-Methoden aus dem Compositor verwenden. Anschließend können Sie die folgenden Eigenschaften der Bewegung definieren:

- Dämpfung – drückt den Grad der Dämpfung der Spring Motion aus, die in der Animation verwendet wird.

| Wert für das Dämpfungs Verhältnis | BESCHREIBUNG |
| ------------------- | ----------- |
| Dämpfingratio = 0 | Undamped – die Spring-Zeit wird für einen langen Zeitraum in der springenden |
| 0 < < 1 für "Dämpfung" | Unterschätzt – Spring wird von etwas zu viel zu laufen. |
| "Dämpfingratio = 1" | Criticallydämpfed – die Spring Vorgang führt keine Schwingungen aus. |
| Dämpfung > 1 | Überlastet – der Spring Vorgang erreicht schnell sein Ziel mit einer abrupten Verlangsamung und ohne Schwingung |

- Period – die Zeit, die der Spring Vorgang zum Ausführen einer einzelnen Schwingung benötigt.
- Final/Startwert – definierte Anfangs-und Endpositionen der Spring Motion (wenn nicht definiert, ist der Startwert und/oder der endgültige Wert der aktuelle Wert).
- Anfängliche Geschwindigkeit – programmgesteuerte anfängliche Geschwindigkeit für die Bewegung.

Sie können auch einen Satz von Eigenschaften der Bewegung definieren, die mit Keyframeanimation identisch sind:

- Delta Zeit-/Verzögerungs Verhalten
- Stopbehavior

In den häufigen Fällen der Animation von Offset und Skalierung/Größe werden die folgenden Werte vom Windows-Entwurfs Team für "Dämpfung" und "Period" für unterschiedliche Arten von Sprüngen empfohlen:

| Eigenschaft | Normaler Spring | Gedämpfte Spring | Weniger gedämpft Spring |
| -------- | ------------- | --------------- | -------------------- |
| Offset | Dämpfungs Verhältnis = 0,8 <br/> Zeitraum = 50 ms | Dämpfungs Verhältnis = 0,85 <br/> Zeitraum = 50 ms | Dämpfungs Verhältnis = 0,65 <br/> Zeitraum = 60 ms |
| Skalierung/Größe | Dämpfungs Verhältnis = 0,7 <br/> Zeitraum = 50 ms | Dämpfungs Verhältnis = 0,8 <br/> Zeitraum = 50 ms | Dämpfungs Verhältnis = 0,6 <br/> Zeitraum = 60 ms |

Nachdem Sie die Eigenschaften definiert haben, können Sie die Spring naturalmotionanimation an die startanimation-Methode eines compositionobject oder die Motion-Eigenschaft eines Interaction Tracker-inertiamodifier übergeben.

## <a name="example"></a>Beispiel

In diesem Beispiel erstellen Sie eine Benutzeroberfläche für Navigation und Canvas, bei der der Benutzer auf eine Erweiterungs Schaltfläche klickt, sodass ein Navigationsbereich mit einer springenden, schwingenden Bewegung animiert wird.

![Spring Animation beim Klicken](images/animation/spring-animation-on-click.gif)

Definieren Sie zunächst die Spring Animation innerhalb des angeklickten Ereignisses, wenn der Navigationsbereich angezeigt wird. Anschließend definieren Sie die Eigenschaften der Animation mithilfe der Funktion "initialvalueexpression", um einen Ausdruck zum Definieren von finalvalue zu verwenden. Außerdem behalten Sie den Überblick, ob der Bereich geöffnet ist, und starten die Animation, wenn Sie bereit sind.

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
 _springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue + 250";
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue - 250";
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

Was ist, wenn Sie diese Bewegung mit Eingaben verknüpfen möchten? Wenn der Endbenutzer also den Bereich verlässt, werden die Bereiche mit Spring Motion angezeigt? Noch wichtiger ist, dass sich die Bewegung basierend auf der Geschwindigkeit des Endbenutzers anpasst, wenn der Benutzer härter oder schneller schwimmt.

![Spring Animation beim Schwenken](images/animation/spring-animation-on-swipe.gif)

Zu diesem Zweck können Sie die gleiche Spring Animation verwenden und Sie mit interaktiontracker an einen inertiamodifier übergeben. Weitere Informationen zu inputanimationen und interaktiontracker finden Sie unter [benutzerdefinierte Bearbeitungsfunktionen mit interaktiontracker](interaction-tracker-manipulations.md). Wir gehen davon aus, dass Sie für dieses Codebeispiel bereits interaktiontracker und visualinteraktionsource eingerichtet haben. Wir konzentrieren uns auf das Erstellen der inertiamodifiers, die eine naturalmotionanimation, in diesem Fall eine Spring.

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

Nun haben Sie sowohl eine programmgesteuerte als auch eine Eingabe gesteuerte Spring Animation in Ihrer Benutzeroberfläche!

Zusammengefasst sind die Schritte zum Verwenden einer Spring-Animation in Ihrer APP:

1. Erstellen Sie Ihre springanimation aus Ihrem Compositor.
1. Definieren Sie die Eigenschaften der springanimung, wenn Sie nicht standardmäßige Werte wünschen:
    - Dämpfingratio
    - Zeitraum
    - Endwert
    - Anfangswert
    - Anfängliche Geschwindigkeit
1. Ziel zuweisen.
    - Wenn Sie eine compositionobject-Eigenschaft animieren, übergeben Sie springanimation als Parameter an startanimation.
    - Wenn Sie with Input verwenden möchten, legen Sie die NaturalMotion-Eigenschaft eines inertiamodifier auf springanimations fest.

