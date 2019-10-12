---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Kompositionsanimationen
description: Viele Eigenschaften von Kompositionsobjekten und Effekten können mit Keyframeanimationen und Ausdrucksanimationen animiert werden. Dadurch können sich Eigenschaften eines UI-Elements im Laufe der Zeit oder auf der Grundlage einer Berechnung verändern.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5991b5f9f3c63a25a7476cc35621488c6cb60b2c
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281836"
---
# <a name="composition-animations"></a>Kompositionsanimationen

Mit der Windows.UI.Composition API können Sie Kompositorobjekte in einer einheitlichen API-Ebene erstellen, animieren, umwandeln und bearbeiten. Kompositionsanimationen bieten eine leistungsstarke und effiziente Möglichkeit, Animationen auf der Benutzeroberfläche Ihrer Anwendung auszuführen. Sie wurden von Grund auf neu entwickelt, um sicherzustellen, dass Ihre Animationen mit 60 F/s unabhängig vom UI-Thread ausgeführt werden, und um Ihnen die Flexibilität zu bieten, tolle Funktionen zu erstellen, die zur Steuerung von Animationen nicht nur Zeit, sondern auch Eingaben und andere Eigenschaften verwenden.

## <a name="motion-in-windows"></a>Bewegung in Windows

Stellen Sie sich Motion-Design wie einen Film vor. Nahtlose Übergänge halten Sie auf die Geschichte konzentriert und erwecken Erlebnisse zum Leben. Wir können dieses Gefühl in unsere Entwürfe einbringen und Menschen von einer Aufgabe zur nächsten führen. Bewegung ist oft der differenzierende Faktor zwischen einer Benutzeroberfläche und einer Benutzeroberfläche.

Als grundlegender Baustein der Windows-UI-Plattform bieten compositionanimationen eine leistungsfähige und effiziente Möglichkeit, Bewegungs Umgebungen in der Benutzeroberfläche Ihrer Anwendung zu erstellen. Die Animations-Engine wurde von Grund auf neu entworfen, um sicherzustellen, dass Ihre Bewegung mit 60 fps unabhängig vom UI-Thread ausgeführt wird. Diese Animationen sind so konzipiert, dass Sie die Flexibilität zum Erstellen innovativer Bewegungs Erfahrungen basierend auf Zeit, Eingabe und anderen Eigenschaften bieten.

### <a name="examples-of-motion"></a>Beispiele für Bewegung

Im Folgenden finden Sie einige Beispiele für Bewegung in einer App.

Hier verwendet eine App eine verbundene Animation, um ein Elementbild zu animieren, wenn es als Teil der Überschrift der nächsten Seite weiterbesteht. Dieser Effekt trägt dazu bei, Benutzerkontext beim Übergang beizubehalten.

![Ein Beispiel für eine verbundene Animation](images/animation/connected-animation-example.gif)

Hier werden bei einem Bildlauf oder Schwenken der UI verschiedene Objekte mithilfe eines Parallax-Effekts unterschiedlich schnell verschoben. Dadurch entsteht ein Gefühl von Tiefe, Perspektive und Bewegung.

![Beispiel für Parallax mit einer Liste und einem Hintergrundbild](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Verwenden von compositionanimationen zum Erstellen von Bewegung

Um Motion in der Benutzeroberfläche zu generieren, können Entwickler entweder in XAML oder in der visuellen Ebene auf Animationen zugreifen. Animationen auf der visuellen Ebene bieten Entwicklern eine Reihe von Vorteilen:

- Leistung – anstelle der herkömmlichen Thread gebundenen Animation für die Benutzeroberfläche arbeiten Animationen auf der Windows-UI-Plattform mit einem unabhängigen Thread bei 60 fps und ermöglichen reibungslose Bewegungsmöglichkeiten.
- Vorlagen Modell – Animationen in der Windows-Benutzeroberflächen Ebene sind Vorlagen, was bedeutet, dass eine einzelne Animation auf mehreren Objekten verwendet und Eigenschaften oder Parameter angepasst werden können, ohne dass vorherige Verwendungen behindert werden müssen.
- Anpassung – die Windows-Benutzeroberflächen Schicht vereinfacht nicht nur die Erstellung einer wunderbaren Benutzeroberfläche, sondern mit einer vollständigen Palette von Animations Typen, die möglich sind, um neue und beeindruckende Umgebungen mit einem Farbverlauf von Anpassungen zu erstellen.

Als Entwickler, der Erfahrungen auf der Windows-Benutzeroberflächen Ebene erstellt, haben Sie Zugriff auf eine Vielzahl von Animations Konzepten, um Ihre Entwürfe zu verbringen. Sie können jedes dieser Konzepte verwenden, um eine Eigenschaft oder unter Kanal Komponente (falls zutreffend) eines beliebigen compositionobject zu animieren.

> [!NOTE]
> Nicht alle Eigenschaften eines compositionobject sind animabel. Informationen zum bestimmen, ob eine Eigenschaft animiert werden kann, finden Sie in der Dokumentation des einzelnen compositionobject.

> [!NOTE]
> Der Begriff _Subchannel_ verweist auf eine Komponenten Form einer Eigenschaft. Beispielsweise der X-oder XY-Subchannel einer Vector3 Offset-Eigenschaft.

| Animations Konzept | Beschreibung |
| ----------------- | ----------- |
| [Zeitbasierte Bewegung mit keyframeanimationen](time-animations.md)  | Keyframeanimationen werden verwendet, um die gesamte Bewegungs Darstellung über einen bestimmten Zeitraum direkt zu steuern. Entwickler, die den Beginn, das Ende und die Interpolation einer Bewegung beschreiben, und die Dauer in einer herkömmlichen keyframed-Weise. |
| [Relative Bewegung mit expressionanimationen](relation-animations.md)  | Expressionanimationen werden verwendet, um zu beschreiben, wie eine Bewegung der-Eigenschaft eines Objekts in Bezug auf die-Eigenschaft eines anderen Objekts gesteuert werden soll. Entwickler definieren eine mathematische Gleichung, die die Verweis basierte Beziehung definiert. |
| Implicitanimationen | Diese Animationen sind triggerbasiert und werden getrennt von der APP-Kern Logik definiert. Implicitanimationen werden verwendet, um zu beschreiben, wie und wann Animationen als Antwort auf Änderungen direkter Eigenschaften auftreten. |
| [Eingabe gesteuerte Bewegung mit Eingabe Animationen](input-driven-animations.md)  | Eingabe Animationen umfassen eine Reihe von Szenarios, mit denen Entwickler Manipulations basierte Bewegungen über Berührungs-oder andere Eingabe Modalitäten beschreiben können. Diese Animationen werden basierend auf der aktiven Benutzereingabe oder-Gesten gesteuert. |
| [Physik-basierte Bewegung mit naturalmotionanimationen](natural-animations.md)  | Naturalmotionanimationen werden verwendet, um natürliche und vertraute Bewegungsmöglichkeiten auf der Grundlage der realen Bewegung zu beschreiben. Anstatt die Zeit zu definieren, definieren Entwickler die Merkmale der Bewegung (z. b. das Dämpfungs Verhältnis für Springs). |