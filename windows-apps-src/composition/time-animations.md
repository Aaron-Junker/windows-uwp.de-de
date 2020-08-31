---
title: Zeitanimationen
description: Erfahren Sie, wie Sie Keyframeanimation-Klassen verwenden, um zeitbasierte Animationen zu erstellen, die Benutzer durch Änderungen an der Benutzeroberfläche leiten.
ms.date: 12/12/2018
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: c63f59e7bcf282dc829d0fb8fa5971113f7638ad
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053590"
---
# <a name="time-based-animations"></a>Zeitbasierte Animationen

Wenn eine Komponente in oder eine gesamte Benutzer Änderung geändert wird, beobachten Endbenutzer diese häufig auf zwei Arten: im Lauf der Zeit oder sofort. Auf der Windows-Plattform wird die erste Benutzerumgebung, die sich sofort ändert, oft verwirrend und überraschen, da Sie nicht in der Lage sind, zu verfolgen, was passiert ist. Der Endbenutzer spürt die Benutzeroberflächen dann als jarringvorgang und unnatürlich.

Stattdessen können Sie die Benutzeroberfläche im Laufe der Zeit ändern, um den Endbenutzer zu unterstützen, oder Sie über Änderungen an der Benutzeroberfläche benachrichtigen. Auf der Windows-Plattform erfolgt dies mithilfe zeitbasierter Animationen, auch als Keyframeanimation bezeichnet. Mit keyframeanimationen können Sie eine Benutzeroberfläche im Laufe der Zeit ändern und die einzelnen Aspekte der Animation steuern, einschließlich der Art und Weise, wann Sie gestartet wird und wie der Endzustand erreicht wird. Beispielsweise ist das Animieren eines Objekts auf eine neue Position über 300 Millisekunden angenehmer als das sofortige "teleportieren" des Objekts. Wenn Sie Animationen anstelle von sofortigen Änderungen verwenden, ist das net-Ergebnis eine angenehmere und ansprechendere Darstellung.

## <a name="types-of-time-based-animations"></a>Typen von zeitbasierten Animationen

Es gibt zwei Kategorien von zeitbasierten Animationen, die Sie verwenden können, um unter Windows eine schöne Benutzererfahrung zu erstellen:

**Explizite Animationen** – wie der Name bedeutet, starten Sie die Animation explizit, um Updates vorzunehmen.
**Implizite Animationen** – diese Animationen werden vom System in Ihrem Auftrag gestartet, wenn eine Bedingung erfüllt ist.

In diesem Artikel wird erläutert, wie Sie _explizite_ zeitbasierte Animationen mit keyframeanimationen erstellen und verwenden.

Bei expliziten und impliziten zeitbasierten Animationen gibt es verschiedene Typen, die den unterschiedlichen Typen von Eigenschaften von compositionobjects entsprechen, die Sie animieren können.

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>Erstellen zeitbasierter Animationen mit keyframeanimationen

Bevor Sie beschreiben, wie Sie explizite zeitbasierte Animationen mit keyframeanimationen erstellen, betrachten wir einige Konzepte.

- Keyframes – Dies sind die einzelnen "Momentaufnahmen", die von einer Animation animiert werden.
  - Definiert als Schlüssel &-Wertpaare. Der Schlüssel stellt den Fortschritt zwischen 0 und 1 dar, d..., wenn in der Lebensdauer der Animation diese "Momentaufnahme" stattfindet. Der andere Parameter stellt den Eigenschafts Wert zu diesem Zeitpunkt dar.
- Keyframeanimation-Eigenschaften – Anpassungsoptionen, die Sie anwenden können, um die Anforderungen der Benutzeroberfläche zu erfüllen.
  - Delta time – Zeit, bevor eine Animation gestartet wird, nachdem startanimation aufgerufen wurde.
  - Dauer – Dauer der Animation.
  - Iterationbehavior – Anzahl oder unendliches Wiederholungs Verhalten für eine Animation.
  - IterationCount – die Anzahl der Endzeiten, die eine Keyframe-Animation wiederholt.
  - Keyframe-Anzahl – lesen Sie die Anzahl der Keyframes in einer bestimmten Keyframe-Animation.
  - Stoppverhalten – legt das Verhalten eines Animationseigenschaftswerts fest, wenn „StopAnimation“ aufgerufen wird.
  - Direction – gibt die Richtung der Animation für die Wiedergabe an.
- Animations Gruppe – mehrere Animationen gleichzeitig zu starten.
  - Wird häufig verwendet, wenn mehrere Eigenschaften gleichzeitig animiert werden sollen.

Weitere Informationen finden Sie unter [compositionanimationgroup](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimationgroup).

Mit diesen Konzepten sprechen wir über die allgemeine Formel zum Erstellen einer Keyframeanimation:

1. Identifizieren Sie das compositionobject und die entsprechende Eigenschaft, die Sie animieren müssen.
1. Erstellen Sie eine Keyframeanimation-typvorlage des Compositors, der mit dem Typ der zu animierenden Eigenschaft übereinstimmt.
1. Mit der Animations Vorlage können Sie Keyframes hinzufügen und Eigenschaften der Animation definieren.
    - Mindestens ein Keyframe ist erforderlich (der 100%-oder 1F-Keyframe).
    - Es wird empfohlen, auch eine Dauer zu definieren.
1. Wenn Sie bereit sind, diese Animation auszuführen, rufen Sie startanimation (...) für das compositionobject auf, und wählen Sie die Eigenschaft aus, die Sie animieren möchten. Dies gilt insbesondere in folgenden Fällen:
    - `visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. Wenn Sie über eine laufende Animation verfügen und die Animation oder Animations Gruppe abbrechen möchten, können Sie diese APIs verwenden:
    - `visual.StopAnimation("targetProperty");`
    - `visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

Sehen wir uns ein Beispiel an, um diese Formel in Aktion zu sehen.

## <a name="example"></a>Beispiel

In diesem Beispiel möchten Sie den Offset eines visuellen Elements von <0, 0, 0> auf <200, 0, 0> über 1 Sekunde animieren. Außerdem möchten Sie das visuelle animieren zwischen diesen Positionen 10 mal sehen.

![Keyframe-Animation](images/animation/animated-rectangle.gif)

Zuerst identifizieren Sie zunächst das compositionobject und die Eigenschaft, die Sie animieren möchten. In diesem Fall wird das rote Quadrat durch eine Kompositions Visualisierung namens dargestellt `redVisual` . Sie starten die Animation von diesem-Objekt aus.

Da Sie nun die Offset-Eigenschaft animieren möchten, müssen Sie eine Vector3KeyFrameAnimation erstellen (Offset ist vom Typ Vector3). Außerdem definieren Sie die entsprechenden Keyframes für die Keyframeanimation.

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

Dann definieren Sie die Eigenschaften der Keyframeanimation, um deren Dauer und das Verhalten für die Animation zwischen den beiden Positionen (aktuell und <200, 0,0>) zehnmal zu beschreiben.

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

Um schließlich eine Animation auszuführen, müssen Sie Sie für eine Eigenschaft eines compositionobject-Objekts starten.

```csharp
redVisual.StartAnimation("Offset", animation);
```

Im folgenden finden Sie den vollständigen Code.

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redVisual)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    redVisual.StartAnimation("Offset", animation);
} 
```
