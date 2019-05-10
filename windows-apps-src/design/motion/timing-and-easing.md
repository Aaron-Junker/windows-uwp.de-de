---
Description: Erfahren Sie, wie Fluent Motion verwendet Zeitsteuerungssystem und Beschleunigungsfunktionen.
title: Timing und Geschwindigkeitsverlauf – Animation in UWP-Apps
label: Timing and easing
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b736a10a7284e3cc9aa193e082dc654e908afe40
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444171"
---
# <a name="timing-and-easing"></a>Timing und Geschwindigkeitsverlauf

Bewegung basiert zwar auf der realen Welt, aber auch in einem digitalen Medium werden Geschwindigkeit und Leistung erwartet.

## <a name="examples"></a>Beispiele

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/EasingFunction">öffnen Sie die app, und vereinfachen von Funktionen in Aktion erleben</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Fluent-Bewegungen und Timing

Timing ist ein wichtiges Element, um die Bewegung von Objekten natürlich erscheinen zu lassen, die in die Benutzeroberfläche eintreten, sie verlassen oder sich darin bewegen.

1. Objekte oder Szenen, die in das Sichtfeld eintreten, sind schnell und auffällig. Die Animationen für diese Elemente dauern in der Regel längere als die für austretende Elemente, um den hierarchischen Aufbau einer Szene zu ermöglichen.
1. Objekte oder Szenen, die das Sichtfeld verlassen, sind sehr schnell. Der Benutzer sollte nachvollziehen können, wo die UI verbleibt. Nachdem die UI jedoch geschlossen wurde, sollte sie aus dem Weg gehen.
1. Objekte, die sich durch eine Szene bewegen, sollten dafür eine Dauer benötigen, die der zurückzulegenden Entfernung entspricht.

## <a name="timing-in-fluent-motion"></a>Timing für Fluent-Bewegungen

Für das Timing von Fluent-Bewegungen gelten 500 ms (eine halbe Sekunde) als Grundeinheit, da dies die maximale Zeit ist, die ein Benutzer als unmittelbar empfindet.

![Favoritenbild](images/time.gif)

### <a name="150ms-exit"></a>**150 ms** (Verlassen)

:::row:::
    :::column:::
Verwenden Sie für Objekte oder Seiten, die die Szene zu beenden oder schließen.
Sie ermöglicht eine sehr schnelle direktionale Rückmeldung der UI, bei der das Timing die Framerate nicht beeinträchtigt und so eine flüssige Animation unterstützt.
    :::column-end:::
    :::column:::
        ![150ms motion](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 ms** (Eintreten)

:::row:::
    :::column:::
Verwenden Sie für Objekte oder Seiten, die die Szene eingeben oder Sie öffnen.
Dies ist eine angemessene Zeitspanne, um Inhalte zu erkennen, wenn sie in die Szene eintreten.
    :::column-end:::
    :::column:::
        ![300ms motion](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤ 500 ms** (Durchqueren)

:::row:::
    :::column:::
Verwenden Sie nach Objekten, die in einer einzigen Szene oder mehrerer Szenen übersetzen. 
    :::column-end:::
    :::column:::
        ![500ms motion](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Geschwindigkeitsverlauf in Fluent-Bewegungen

Die Änderung des Geschwindigkeitsverlaufs ist eine Möglichkeit, die Bewegungsdauer eines Objekts anzupassen. Der Geschwindigkeitsverlauf ist der Leim, der alle Fluent-Bewegungserfahrungen verbindet. Die Verwendung eines Geschwindigkeitsverlaufs kann die Anmutung von Objekten vereinheitlichen, die sich durch das System bewegen. Dies ist eine Möglichkeit zum Simulieren der realen Welt und lässt bewegte Objekte in ihrer Umgebung natürlich aussehen.

![Favoritenbild](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Anwenden eines Geschwindigkeitsverlaufs auf Bewegungen

Die folgenden Geschwindigkeitsverläufe vermitteln ein natürliches Verhalten und sind die Grundwerte, die wird für Fluent-Bewegungen verwenden.

Die Codebeispiele zeigen, wie die empfohlene Werte auf Storyboardanimationen (XAML) oder Kompositionsanimationen (C#) angewendet werden.

### <a name="accelerate-exit"></a>**Beschleunigen** (Verlassen)

:::row:::
    :::column:::
Verwenden Sie für die Benutzeroberfläche oder Objekte, die die Szene Vorgang beendet werden.

Objekte werden unterstützt und Momentum erhalten, bis sie die Escape-Geschwindigkeit erreichen.
Das resultierende Verhalten ist, dass das Objekt, die am schwersten versucht zu nutzen Sie die Benutzer und Platz für neue Inhalte eingehen.
    :::column-end:::
    :::column:::
        ![accelerate easing](images/accelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**Verlangsamen** (Eintreten)

:::row:::
    :::column:::
Verwenden Sie für Objekte oder Eingeben der Szene Benutzeroberfläche navigieren, oder erstellen.

Nach der Szene, wird das Objekt mit extremer Reibung, erfüllt die verlangsamt, das Objekt wird, das rest.
Das resultierende Verhalten ist, dass das Objekt aus einer langen Entfernung zurückgelegt mit einer Geschwindigkeit extreme eingegeben und ist schnell und auf einen Rest-Status.

Auch wenn es einen Moment an fehlender Reaktionsfähigkeit vorangestellt ist, hat die Geschwindigkeit des eingehenden Objekts die Auswirkungen der empfunden, schnell und reaktionsfähig.
    :::column-end:::
    :::column:::
        ![decelerate easing](images/decelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**Standard** (Durchqueren)

:::row:::
    :::column:::
Dies ist die Baseline, die bei einer parameteränderung der animierten innerhalb des Systems zu vereinfachen.
Verwenden Sie diesen Standard-Geschwindigkeitsverlauf für Objekte, die auf dem Bildschirm von Status zu Status wechseln, z. B. bei einer einfachen Positionsänderung. Verwenden Sie ihn zudem für Objekte, die sich in der Szene kontinuierlich verändern (Morphing), beispielsweise wachsen.

Das resultierende Verhalten ist, dass Objekte ändern des Status von A nach B überwinden sind, erstellt von, natürlichen erzwingt.
    :::column-end:::
    :::column:::
        ![standard easing](images/standardEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>Verwandte Artikel

- [Motion-Übersicht](index.md)
- [Direktionalität und die Schwerkraft](directionality-and-gravity.md)
