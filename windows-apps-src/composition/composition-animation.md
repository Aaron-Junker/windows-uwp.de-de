---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Kompositionsanimationen
description: Viele Eigenschaften von Kompositionsobjekten und Effekten können mit Keyframeanimationen und Ausdrucksanimationen animiert werden. Dadurch können sich Eigenschaften eines UI-Elements im Laufe der Zeit oder auf der Grundlage einer Berechnung verändern.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 18208986d7d07e4d437e52dce844deecc03cf1f6
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240028"
---
# <a name="composition-animations"></a>Kompositionsanimationen

Mit der Windows.UI.Composition API können Sie Kompositorobjekte in einer einheitlichen API-Ebene erstellen, animieren, umwandeln und bearbeiten. Kompositionsanimationen bieten eine leistungsstarke und effiziente Möglichkeit, Animationen auf der Benutzeroberfläche Ihrer Anwendung auszuführen. Sie wurden von Grund auf neu entwickelt, um sicherzustellen, dass Ihre Animationen mit 60 F/s unabhängig vom UI-Thread ausgeführt werden, und um Ihnen die Flexibilität zu bieten, tolle Funktionen zu erstellen, die zur Steuerung von Animationen nicht nur Zeit, sondern auch Eingaben und andere Eigenschaften verwenden.

## <a name="motion-in-windows"></a>Während der Übertragung in Windows

Stellen Sie sich Motion-Design wie einen Film vor. Nahtlose Übergänge halten Sie auf die Geschichte konzentriert und erwecken Erlebnisse zum Leben. Wir können dieses Gefühl in unsere Entwürfe einbringen und Menschen von einer Aufgabe zur nächsten führen. Während der Übertragung ist häufig den unterscheidenden Faktor zwischen einer Benutzeroberfläche und eine Benutzeroberfläche.

Geben Sie als ein wesentlicher Baustein von der Windows-UI-Plattform CompositionAnimations ein leistungsfähiges und effizientes Verfahren zum Erstellen von Motion-Erfahrungen in der Benutzeroberfläche Ihrer Anwendung. Der Animations-Engine wurde von Grund auf neu entwickelt, um sicherzustellen, dass Ihre Bewegung auf 60 FPS, unabhängig von der UI-Thread ausgeführt wird. Diese Animationen sollen die Flexibilität zum Erstellen von innovative Motion-Anwendungen, die basierend auf der Zeit, Eingabe und andere Eigenschaften bereitstellen.

### <a name="examples-of-motion"></a>Beispiele für Bewegung

Im Folgenden finden Sie einige Beispiele für Bewegung in einer App.

Hier verwendet eine App eine verbundene Animation, um ein Elementbild zu animieren, wenn es als Teil der Überschrift der nächsten Seite weiterbesteht. Dieser Effekt trägt dazu bei, Benutzerkontext beim Übergang beizubehalten.

![Ein Beispiel für verbundene Animationen](images/animation/connected-animation-example.gif)

Hier werden bei einem Bildlauf oder Schwenken der UI verschiedene Objekte mithilfe eines Parallax-Effekts unterschiedlich schnell verschoben. Dadurch entsteht ein Gefühl von Tiefe, Perspektive und Bewegung.

![Beispiel für Parallax mit einer Liste und einem Hintergrundbild](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Erstellen während der Übertragung mithilfe von CompositionAnimations

Um während der Übertragung in die Benutzeroberfläche zu generieren, können Entwickler Animationen in XAML oder in der visuellen Ebene zugreifen. Animationen in der visuellen Ebene bieten Entwicklern eine Reihe von Vorteilen:

- Leistung – anstelle der herkömmlichen UI-Thread-Bound-Animation, Animationen auf der Benutzeroberfläche der Windows-Plattform, die auf einem unabhängigen Thread mit 60 FPS, die von einem nahtlosen Übergangs Benutzererlebnis ausgeführt werden.
- Vorlagenmodell – Animationen in der Windows-UI-Ebene sind Vorlagen, Bedeutung verwenden Sie eine einzelne Animation auf mehrere Objekte und Eigenschaften ändern kann oder Parameter unabhängig von der vorherigen sitzt verwendet.
- Anpassung: die Windows-UI-Ebene nicht nur vereinfacht ansprechende Benutzeroberfläche machen, aber mit vollständigem Umfang von Animationstypen, Erfahrungen aus möglich ist, erstellen Sie neue und beeindruckende mit einem Farbverlauf von Anpassungen

Als Entwickler Erstellen von Funktionen auf der Windows-Benutzeroberflächenebene haben Sie Zugriff auf eine Vielzahl von Animation-Konzepte, die Ihre Entwürfe zum Leben zu erwecken. Sie können eines der folgenden Konzepte zum Animieren einer Eigenschaft oder für die Komponente (falls zutreffend), der alle "compositionobject" subchannel verwenden.

> [!NOTE]
> Es sind nicht alle Eigenschaften für ein "compositionobject" animierbaren. Finden Sie in der Dokumentation zu den einzelnen "compositionobject", um zu bestimmen, ob eine Eigenschaft animierbar ist.

> [!NOTE]
> Der Begriff _Teilchannel_ bezieht sich auf eine Form von Komponenten einer Eigenschaft. Z. B. die X- oder XY einer Vector3 Offset-Eigenschaft für subchannel.

| Animation-Konzept | Beschreibung |
| ----------------- | ----------- |
| [Zeitbasierte während der Übertragung mit KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations werden verwendet, um eine Benutzeroberfläche während der Übertragung über einen Zeitraum während des gesamten Entwicklungsprozesses direkt zu steuern. Entwickler, die einer Bewegung des Start, Ende, Interpolation zwischen und Dauer auf herkömmliche Samplegröße Weise beschreibt. |
| [Relative während der Übertragung mit ExpressionAnimations](relation-animations.md)  | ExpressionAnimations wird beschrieben, wie eine Bewegung der Eigenschaft eines Objekts relativ zu einem anderen Objekt-Eigenschaft gesteuert werden soll. Entwickler definieren eine mathematische Gleichung, die die Referenz-basierte Beziehung definiert. |
| ImplicitAnimations | Diese Animationen basieren auf Trigger und werden separat von der Anwendungslogik Core definiert. ImplicitAnimations wird beschrieben, wie und wann Animationen als Antwort auf direkte eigenschaftenänderungen auftreten. |
| [Eingabe-driven während der Übertragung mit der Eingabe-Animationen](input-driven-animations.md)  | Geben Sie Animationen behandelt eine Reihe von Szenarios, mit die Entwickler Manipulation-basierte während der Übertragung per Touch oder andere eingabemöglichkeiten beschreiben können. Diese Animationen, werden basierend auf aktiven Benutzereingaben oder Gesten gesteuert. |
| [Physik-basierte während der Übertragung mit NaturalMotionAnimations](natural-animations.md)  | NaturalMotionAnimations dienen zum Beschreiben von gewohnte Umgebung während der Übertragung, dass basierte auf echten Erfahrungen testgesteuerte während der Übertragung zu erzwingen. Anstatt Zeit definieren, definieren Entwickler Merkmale der die Bewegung (z. B. das Verhältnis für Federn Benachrichtigungswarteschlange) |