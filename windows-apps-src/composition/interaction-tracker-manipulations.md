---
title: Benutzerdefinierte Manipulationen mit interaktiontracker
description: Verwenden Sie die interaktiontracker-APIs, um benutzerdefinierte Bearbeitungs Umgebungen zu erstellen.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: 8a4b682f009a4ac1350ceee3b8c23fe5e772150d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163594"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>Benutzerdefinierte Manipulations Erfahrung mit interaktiontracker

In diesem Artikel wird gezeigt, wie Sie mit interaktiontracker benutzerdefinierte Bearbeitungsfunktionen erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen davon aus, dass Sie mit den Konzepten vertraut sind, die in den folgenden Artikeln erläutert werden:

- [Eingabegesteuerte Animationen](input-driven-animations.md)
- [Beziehungs basierte Animationen](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>Warum benutzerdefinierte Manipulationsmöglichkeiten erstellen?

In den meisten Fällen sind die vorkonfigurierten Bearbeitungs Steuerelemente zum Erstellen von Benutzeroberflächen Erlebnissen ausreichend. Aber was ist, wenn Sie von den allgemeinen Steuerelementen unterscheiden möchten? Was geschieht, wenn Sie eine bestimmte Benutzeroberfläche erstellen möchten, die von der Eingabe gesteuert wird, oder eine Benutzeroberfläche haben, bei der eine herkömmliche Manipulations Bewegung nicht ausreicht? Dies ist der Ort, an dem benutzerdefinierte Erfahrungen erstellt werden. Dadurch können App-Entwickler und Designer kreativer werden – mit der Lebensdauer von Bewegungs Erlebnissen, die das Branding und die benutzerdefinierte Entwurfs Sprache besser veranschaulichen. Von Grund auf erhalten Sie Zugriff auf die richtigen Bausteine, um eine Bearbeitungs Darstellung vollständig anzupassen – von der Art und Weise, wie Motion mit dem Finger auf dem Bildschirm auf den Bildschirm und die Eingabe Verkettung reagieren soll.

Im folgenden finden Sie einige gängige Beispiele für das Erstellen einer benutzerdefinierten Bearbeitungsfunktion:

- Hinzufügen eines benutzerdefinierten Schwenk Verhaltens, Löschen/verwerfen
- Eingabe bezogene Effekte (Schwenken bewirkt, dass Inhalt weich ist)
- Benutzerdefinierte Steuerelemente mit angepassten Bearbeitungs Bewegungen (benutzerdefinierte ListView, ScrollViewer usw.)

![Beispiel für ein Swipe-Scroller](images/animation/swipe-scroller.gif)

![Pull to animieren (Beispiel)](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>Gründe für die Verwendung von interaktiontracker

Interaktiontracker wurde in der Version 10586 SDK in den Windows. UI. Composition. Interaktionen-Namespace eingeführt. Interaktiontracker ermöglicht Folgendes:

- **Umfassende Flexibilität** – wir möchten, dass Sie alle Aspekte der Bearbeitung anpassen und anpassen können. insbesondere die exakten Bewegungen, die während oder als Reaktion auf eine Eingabe auftreten. Wenn Sie eine benutzerdefinierte Bearbeitungsfunktion mit interaktiontracker entwickeln, stehen Ihnen alle benötigten Knoten zur Verfügung.
- **Reibungslose Leistung** – eine der Herausforderungen bei der Manipulations Erfahrung besteht darin, dass die Leistung vom UI-Thread abhängig ist. Dies kann sich negativ auf die Manipulation auswirken, wenn die Benutzeroberfläche ausgelastet ist. Interaktiontracker wurde erstellt, um die neue Animations-Engine zu verwenden, die auf einem unabhängigen Thread mit 60 fps betrieben wird, was zu einer reibungslosen Bewegung führt.

## <a name="overview-interactiontracker"></a>Übersicht: interaktiontracker

Beim Erstellen benutzerdefinierter Bearbeitungsfunktionen gibt es zwei Hauptkomponenten, mit denen Sie interagieren. Diese werden zuerst besprochen:

- [Interaktiontracker](/uwp/api/windows.ui.composition.interactions.interactiontracker) – das Hauptobjekt, das einen Zustands Automaten verwaltet, dessen Eigenschaften durch aktive Benutzereingaben oder direkte Updates und Animationen gesteuert werden. Er soll dann mit einer compositionanimation verknüpft werden, um die benutzerdefinierte Manipulations Bewegung zu erstellen.
- [Visualinteraktionsource](/uwp/api/windows.ui.composition.interactions.visualinteractionsource) – ein Komplement Objekt, das definiert, wann und unter welchen Bedingungen Eingaben an interaktiontracker gesendet werden. Es definiert sowohl das für Treffer Tests verwendete compositionvisuelles Element als auch andere Eingabe Konfigurations Eigenschaften.

Als Zustands Automat können Eigenschaften von interaktiontracker durch folgende Aktionen gesteuert werden:

- Direkte Benutzerinteraktion – der Endbenutzer wird direkt innerhalb des "visualinteraktionsource"-Treffer Testbereichs bearbeitet.
- Trägheit – entweder von der programmgesteuerten Geschwindigkeit oder einer Benutzer Geste animieren die Eigenschaften von interaktiontracker unter einer Trägheit-Kurve.
- Customanimation – eine benutzerdefinierte Animation, die direkt auf eine Eigenschaft von interaktiontracker abzielt

### <a name="interactiontracker-state-machine"></a>Interaktiontracker-Zustands Automat

Wie bereits erwähnt, handelt es sich bei interaktiontracker um einen Zustands Automat mit vier Zuständen – von denen jeder zu einem der anderen vier Zustände übergehen kann. (Weitere Informationen dazu, wie interaktiontracker zwischen diesen Zuständen übergeht, finden Sie in der Dokumentation zur [interaktiontracker](/uwp/api/windows.ui.composition.interactions.interactiontracker) -Klasse.)

| State | BESCHREIBUNG |
|-------|-------------|
| Idle | Keine aktiven, treibenden Eingaben oder Animationen |
| Interaktion | Aktive Benutzereingabe erkannt |
| Trägheit | Aktiver Bewegungs Ergebnis durch aktive Eingabe oder programmgesteuerte Geschwindigkeit |
| Customanimation | Aus einer benutzerdefinierten Animation resultierende aktive Bewegung |

In jedem Fall, in dem sich der Status von interaktiontracker ändert, wird ein Ereignis (oder ein Rückruf) generiert, das Sie überwachen können. Damit Sie diese Ereignisse überwachen können, müssen Sie die [iinteraktiontrackerowner](/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) -Schnittstelle implementieren und Ihr interaktiontracker-Objekt mit der Methode "kreatewithowner" erstellen. Im folgenden Diagramm wird auch erläutert, wann die verschiedenen Ereignisse ausgelöst werden.

![Interaktiontracker-Zustands Automat](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>Verwenden von visualinteraktionsource

Damit interaktiontracker von der Eingabe gesteuert wird, müssen Sie eine visualinteraktionsource (VIS) mit ihr verbinden. Das VIS wird als Komplement Objekt erstellt, wobei ein compositionvisual zum Definieren von verwendet wird:

1. Der Treffer Testbereich, in dem die Eingabe nachverfolgt wird, und die Gesten des Koordinaten Raums werden erkannt.
1. Die Konfigurationen von Eingaben, die erkannt und weitergeleitet werden, sind u. a.:
    - Erkennbare Gesten: Position X und Y (horizontal und vertikal Schwenken), Skalierung (Pinch)
    - Trägheit
    - Rails & Verkettung
    - Umleitungs Modi: welche Eingabedaten werden automatisch an interaktiontracker umgeleitet?

> [!NOTE]
> Da visualinteraktionsource basierend auf der Position des Treffer Tests und des Koordinaten Raums eines visuellen Elements erstellt wird, empfiehlt es sich nicht, ein visuelles Element zu verwenden, das sich in Bewegung befindet oder die Position ändert.

> [!NOTE]
> Sie können mehrere visualinteraktionsource-Instanzen mit demselben interaktiontracker verwenden, wenn mehrere Treffer Testbereiche vorhanden sind. Der häufigste Fall ist jedoch die Verwendung von nur einem VIS.

Die visualinteraktionsource ist auch für die Verwaltung zuständig, wenn Eingabedaten aus unterschiedlichen Modalitäten (berühren, PTP, Stift) an interaktiontracker weitergeleitet werden. Dieses Verhalten wird durch die Eigenschaft "manipulationredirectionmode" definiert. Standardmäßig werden alle Zeiger Eingaben an den UI-Thread gesendet, und die Eingabe von Precision Touchpad wechselt zu visualinteraktionsource und interaktiontracker.

Wenn Sie also "berühren" und "Pen" (Creators Update) benötigen, um eine Manipulation durch eine visualinteraktionsource und interaktiontracker zu erzielen, müssen Sie die visualinteraktionsource. tryredirectformanipulation-Methode aufrufen. Im kurzen Code Ausschnitt aus einer XAML-APP wird die-Methode aufgerufen, wenn ein Fingerabdruck-Ereignis am oberen Rand des UIElement-Rasters auftritt:

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>Mit expressionanimation verknüpfen

Wenn Sie interaktiontracker verwenden, um eine Manipulation zu fördern, interagieren Sie hauptsächlich mit den Eigenschaften "Skala" und "Position". Wie bei anderen compositionobject-Eigenschaften können diese Eigenschaften sowohl das Ziel als auch das referenzierte in einer compositionanimation sein, in den meisten Fällen expressionanimationen.

Wenn Sie interaktiontracker innerhalb eines Ausdrucks verwenden möchten, verweisen Sie auf die Eigenschaft Position (oder Skala) der Tracker, wie im folgenden Beispiel gezeigt. Da die Eigenschaft von interaktiontracker aufgrund einer der zuvor beschriebenen Bedingungen geändert wird, ändert sich auch die Ausgabe des Ausdrucks.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> Wenn Sie in einem Ausdruck auf die Position von Interaction Tracker verweisen, müssen Sie den Wert für den resultierenden Ausdruck ändern, um in die richtige Richtung zu wechseln. Dies liegt daran, dass der Fortschritt von interaktiontracker vom Ursprung in einem Diagramm abhängt und es Ihnen ermöglicht, den Fortschritt von interaktiontracker in "realen" Koordinaten (z. b. Entfernung vom Ursprung) zu übernehmen.

## <a name="get-started"></a>Erste Schritte

So beginnen Sie mit der Verwendung von interaktiontracker zum Erstellen benutzerdefinierter Manipulationsmöglichkeiten:

1. Erstellen Sie das interaktiontracker-Objekt mithilfe von interaktiontracker. Create oder interaktiontracker. kreatewithowner.
    - (Wenn Sie "kreatewithowner" verwenden, stellen Sie sicher, dass Sie die iinteraktiontrackerowner-Schnittstelle implementieren.)
1. Legen Sie die minimale und maximale Position der neu erstellten interaktiontracker fest.
1. Erstellen Sie die visualinteraktionsource mit einer compositionvisual.
    - Stellen Sie sicher, dass die übergebene Visualisierung eine Größe ungleich 0 (null) aufweist. Andernfalls wird der Treffer Test nicht ordnungsgemäß ausgeführt.
1. Legen Sie die Eigenschaften von visualinteraktionsource fest.
    - Visualinteraction sourceredirectionmode
    - Positionxsourcemode, positionysourcemode, scalesourcemode
    - Rails & Verkettung
1. Fügen Sie die visualinteraktionsource mithilfe von interaktiontracker. interaktionsources. Add zu interaktiontracker hinzu.
1. Richten Sie tryredirectformanipulation für ein, wenn Berührungs-und Stift Eingaben erkannt werden.
    - Bei XAML erfolgt dies in der Regel auf dem pointerpressed-Ereignis des UIElement.
1. Erstellen Sie eine expressionanimation, die auf die Position von interaktiontracker verweist und Ziel einer compositionobject-Eigenschaft ist.

Im folgenden finden Sie einen kurzen Code Ausschnitt, der #1 – 5 in Aktion anzeigt:

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSource
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

Erweiterte Verwendungsmöglichkeiten von interaktiontracker finden Sie in den folgenden Artikeln:

- [Erstellen von Andock Punkten mit inertiamodifiers](inertia-modifiers.md)
- [Pull-to-refresh mit SourceModifier](source-modifiers.md)