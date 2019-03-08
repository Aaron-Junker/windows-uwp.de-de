---
title: Angepasste Manipulation mit InteractionTracker
description: In diesem Artikel zeigen wir Ihnen, wie Sie mit InteractionTracker benutzerdefinierte Manipulationserlebnisse erstellen können.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 9d2c965bcfbf81efe73ce8aff93cdb8b31163fbd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653845"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>Angepasste Manipulation mit InteractionTracker

In diesem Artikel zeigen wir Ihnen, wie Sie mit InteractionTracker benutzerdefinierte Manipulationserlebnisse erstellen können.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen hier davon aus, dass Sie mit den in diesen Artikeln behandelten Konzepten vertraut sind:

- [Eingabe-driven Animationen](input-driven-animations.md)
- [Beziehung basierend Animationen](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>Warum benutzerdefinierte Manipulationserlebnisse erstellen?

In den meisten Fällen ist die Verwendung der vorkonfigurierten Manipulationssteuerelementen ausreichend, um UI-Erlebnisse zu erzeugen. Aber was wäre, wenn Sie sich von den üblichen Steuerelementen unterscheiden wollten? Was wäre, wenn Sie ein spezifisches Erlebnis schaffen wollten, das durch Eingaben gesteuert wird oder eine Benutzeroberfläche haben wollten, in der eine traditionelle Manipulationsbewegung nicht ausreicht? Hier setzt das Erstellen benutzerdefinierter Erlebnisse an. Sie ermöglichen App-Entwicklern und Designern, kreativer zu sein – sie erwecken Bewegungserlebnisse zum Leben, die ihr Branding und ihre eigene Designsprache besser umsetzen. Sie erhalten Zugang zu den richtigen Bausteinen, um das gesamte Manipulationserlebnis individuell gestalten zu können – von der Art und Weise, wie Bewegungen mit dem Finger auf dem Bildschirm reagieren sollen, bis hin zu Andockpunkten und der Verkettung von Eingaben.

Nachfolgend finden Sie einige Beispiele, wann Sie eine benutzerdefinierte Manipulation erstellen könnten:

- Hinzufügen eines benutzerdefinierten Wisch-, Lösch-/Schließen-Verhaltens
- Eingabegesteuerte Effekte (Schwenken führt zu unscharfen Inhalten)
- Benutzerdefinierte Steuerelemente mit maßgeschneiderten Manipulationsbewegungen (angepasstes ListView, ScrollViewer usw.).

![Beispiel zum Scrollen durch Wischen](images/animation/swipe-scroller.gif)

![Beispiel zum Ziehen zum Animieren](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>Warum die InteractionTracker-Klasse verwenden?

InteractionTracker wurde in der 10586 SDK-Version im Namespace Windows.UI.Composition.Interactions eingeführt. InteractionTracker ermöglicht folgendes:

- **Absolute Flexibilität** – Wir möchten, dass Sie in der Lage sind, jeden Aspekt eines Manipulationserlebnisses individuell anzupassen. Insbesondere die genauen Bewegungen, die während oder in Reaktion auf Eingaben auftreten. Wenn Sie mit InteractionTracker ein benutzerdefiniertes Manipulationserlebnis erstellen, stehen Ihnen alle Möglichkeiten zur Verfügung, die Sie benötigen.
- **Flüssige Ausführung** – Eine der Herausforderungen bei Manipulationserfahrungen ist, dass ihre Performance vom UI-Thread abhängig ist. Dies kann sich negativ auf jede Manipulationserfahrung auswirken, wenn die Benutzeroberfläche beschäftigt ist. InteractionTracker wurde entwickelt, um die neue Animationsengine zu nutzen, die mit einem unabhängigen Thread mit 60 FPS arbeitet, was zu einer flüssigen Darstellung führt.

## <a name="overview-interactiontracker"></a>Übersicht: InteractionTracker

Bei der Erstellung angepasster Manipulationserlebnisse gibt es zwei Hauptkomponenten, mit denen Sie interagieren. Wir besprechen diese zuerst:

- [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) – Das Kernobjekt, das eine State-Machine verwaltet, deren Eigenschaften durch aktive Benutzereingaben oder direkte Updates und Animationen gesteuert werden. Diese wird an eine CompositionAnimation gebunden, um die Manipulationsbewegung zu erzeugen.
- [VisualInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource) – Ein Ergänzungsobjekt, das definiert, wann und unter welchen Bedingungen Eingaben an InteractionTracker gesendet werden. Es definiert sowohl CompositionVisual, die für das Hit-Testen verwendet wird, als auch andere Eigenschaften der Eingabekonfiguration.

Als State-Machine können die Eigenschaften von InteractionTracker durch eine der folgenden Optionen gesteuert werden:

- Direkte Benutzerinteraktion – Der Endanwender manipuliert direkt innerhalb der VisualInteractionSource-Hit-Testregion
- Trägheit – Entweder durch programmgesteuerte Beschleunigung oder eine Geste des Benutzers werden Eigenschaften von InteractionTracker über eine Trägheitskurve animiert
- CustomAnimation – Eine angepasste Animation für eine Eigenschaft von InteractionTracker

### <a name="interactiontracker-state-machine"></a>InteractionTracker-State-Machine

Wie bereits erwähnt, ist InteractionTracker Zustandsautomat mit 4 Zuständen – jeweils in einem der anderen vier Zustände übergehen kann. (Weitere Informationen darüber, wie InteractionTracker zwischen diesen Zuständen übergeht, finden Sie in der [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker)-Klassendokumentation.)

| Status | Beschreibung |
|-------|-------------|
| Idle | Nicht aktiv, keine Eingaben oder Animationen |
| Interaktion | Aktive Benutzereingaben erkannt |
| Inertia | Aktive Bewegung aus aktiver Eingabe oder programmgesteuerter Beschleunigung |
| CustomAnimation | Aktive Bewegung, die durch eine angepasste Animation entsteht |

In jedem der Fälle, in denen sich der Zustand des InteractionTracker ändert, wird ein Ereignis (oder Callback) generiert, auf das Sie warten können. Damit Sie auf diese Ereignisse warten können, müssen sie die Schnittstelle [IInteractionTrackerOwner](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) implementieren und ihr InteractionTracker-Objekt mit der Methode CreateWithOwner erzeugen. Das folgende Diagramm zeigt auch, wann die verschiedenen Ereignisse ausgelöst werden.

![InteractionTracker-State-Machine](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>Verwendung der VisualInteractionSource

Damit InteractionTracker von Eingaben gesteuert werden kann, müssen Sie eine VisualInteractionSource (VIS) verbinden. Die VIS wird als Ergänzungsobjekt mit Hilfe eines CompositionVisual erstellt. Es definiert folgendes:

1. Die Hit-Test-Region, in der die Eingabe verfolgt wird, und der Koordinatenraum, in dem Gesten erkannt werden
1. Die Konfigurationen der Eingaben, die erkannt und geroutet werden. Einige von diesen sind:
    - Erkennbare Gesten: Position-X und Y (horizontale und vertikale schwenken), Skalierung (verkleinern)
    - Inertia
    - Führungsschienen und Verkettung
    - Umleitungsmodi: Welche Eingaben von Daten werden automatisch an InteractionTracker umgeleitet?

> [!NOTE]
> Da VisualInteractionSource auf der Grundlage der Hit-Test-Position und des Koordinatenraumes eines Visual erstellt wird, wird empfohlen, kein Visual zu verwenden, das sich in Bewegung befindet oder seine Position ändert.

> [!NOTE]
> Sie können mehrere VisualInteractionSource-Instanzen mit dem gleichen InteractionTracker verwenden, wenn es mehrere Hit-Test-Regionen gibt. Der häufigste Fall ist jedoch, dass nur ein einziges VIS verwendet wird.

Die VisualInteractionSource ist auch für die Verwaltung zuständig, wenn Eingabedaten aus verschiedenen Modalitäten (Touch, PTP, Pen) an den InteractionTracker geroutet werden. Dieses Verhalten wird durch die ManipulationRedirectionMode-Eigenschaft definiert. Standardmäßig werden alle Pointer-Eingaben an den UI-Thread und Precision-Touchpad-Eingaben an VisualInteractionSource und den InteractionTracker gesendet.

Wenn Sie also Touch und Pen (Creators Update) benötigen, um eine Manipulation durch VisualInteractionSource und InteractionTracker zu steuern, müssen Sie die Methode VisualInteractionSource.TryRedirectForManipulation aufrufen. Im kurzen Codeausschnitt unten aus einer XAML-App wird die Methode aufgerufen, wenn ein Touch-Pressed-Ereignis im obersten UIElement-Grid auftritt:

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>Binden an ExpressionAnimations

Wenn Sie InteractionTracker verwenden, um Manipulationserfahrungen zu erstellen, interagieren Sie hauptsächlich mit den Scale- und Positions-Eigenschaften. Wie andere CompositionObject-Eigenschaften können diese Eigenschaften sowohl das Zielobjekt als auch eine CompositionAnimation-Referenz sein. Am häufigsten referenzieren Sie ExpressionAnimations.

Um InteractionTracker innerhalb eines Ausdrucks zu verwenden, verweisen Sie auf die Position- oder Scale-Eigenschaft des Trackers, wie im Beispiel unten. Da die Eigenschaft von InteractionTracker aufgrund einer der zuvor beschriebenen Bedingungen geändert wird, ändert sich auch die Ausgabe des Ausdrucks.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> Wenn Sie die Position des InteractionTracker in einem Ausdruck referenzieren, müssen Sie den Wert negieren, damit der resultierende Ausdruck in die richtige Richtung bewegt wird. Dies liegt daran, dass die Bewegung von InteractionTracker vom Ursprung auf einem Graphen basiert und es Ihnen erlaubt, zur Bewegung von InteractionTracker echte Koordinaten zu nutzen (wie z. B. den Abstand von seinem Ursprung).

## <a name="get-started"></a>Erste Schritte

Gehen Sie folgendermaßen vor, um mit der Verwendung von InteractionTracker zu beginnen:

1. Erstellen Sie Ihr InteractionTracker-Objekt mit InteractionTracker.Create oder InteractionTracker.CreateWithOwner.
    - (Wenn Sie CreateWithOwner verwenden, stellen Sie sicher, dass Sie die Schnittstelle IInteractionTrackerOwner implementieren.)
1. Legen Sie die Min- und Max-Position des neu erstellten InteractionTracker fest.
1. Erstellen Sie Ihre VisualInteractionSource mit einem CompositionVisual.
    - Stellen Sie sicher, dass das von Ihnen übergebene Visual eine Größe ungleich Null hat. Andernfalls wird es nicht korrekt auf Treffer geprüft.
1. Legen Sie die Eigenschaften der VisualInteractionSource fest.
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - Führungsschienen und Verkettung
1. Fügen Sie die VisualInteractionSource über InteractionTracker.InteractionSources.Add zum InteractionTracker hinzu.
1. Richten Sie TryRedirectForManipulation für die Touch- und Pen-Eingabe ein.
    - Für XAML geschieht dies typischerweise im PointerPressed-Ereignis des UIElements.
1. Erstellen Sie eine ExpressionAnimation, die die Position von InteractionTracker referenziert und deren Ziel eine CompositionObject Eigenschaft ist.

Hier ist ein kurzes Codestück, das die Schritte 1-5 in Aktion zeigt:

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSourc
    var source = VisualInteractionSource.Create(hitTestRoot);

    // #4 Set the properties for the VisualInteractionSource
    source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    source.PositionXSourceMode = InteractionSourceMode.EnabledWithInertia;
    source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;

    // #5 Add the VisualInteractionSource to InteractionTracker
    tracker.InteractionSources.Add(source);
}
```

Für weiterführende Anwendungen von InteractionTracker lesen Sie bitte die folgenden Artikel:

- [Erstellen von Ausrichtungspunkte mit InertiaModifiers](inertia-modifiers.md)
- [Zum Aktualisieren Ziehen mit SourceModifiers](source-modifiers.md)
