---
description: Informieren Sie sich über die Wichtigkeit der zeitlichen Steuerung und Beschleunigung bei der Bewegung von Bewegungs Charakter für Objekte, die in der Benutzeroberfläche eintreten, beenden oder verschieben.
title: Timing und Beschleunigung
label: Timing and easing
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fe776361276341e368db1fbdf8e332a1e5dc70b5
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220193"
---
# <a name="timing-and-easing"></a>Timing und Beschleunigung

Während Motion in der realen Welt basiert, sind wir auch ein digitales Medium, das eine Erwartung der Geschwindigkeit und Leistung hat.

## <a name="examples"></a>Beispiele

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn die <strong style="font-weight: semi-bold">XAML</strong> -Steuerelement Katalog-App installiert ist, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/EasingFunction">die APP zu öffnen, und sehen Sie sich die Beschleunigungsfunktionen in Aktion an</a></p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Verwendung von Time in der flüssigen Bewegung

Die zeitliche Steuerung ist ein wichtiges Element, um Bewegungs Charakter für Objekte zu gestalten, die in der Benutzeroberfläche eintreten, beenden oder verschieben.

1. Objekte oder Szenen, die in die Ansicht eintreten, sind schnell, werden aber gefeiert. Diese Animationen sind in der Regel länger als die Ende, um eine hierarchische Erstellung einer Szene zu ermöglichen.
1. Objekte oder Szenen, die die Ansicht verlassen, sind sehr schnell. Der Benutzer sollte wissen können, wohin die Benutzeroberfläche gegangen ist. Nachdem die Benutzeroberfläche verworfen wurde, sollte Sie jedoch Weg kommen.
1. Objekte, die über eine Szene übertragen werden, sollten eine Dauer aufweisen, die für die Menge der Strecke geeignet ist.

## <a name="timing-in-fluent-motion"></a>Zeitliche Steuerung in Bewegung

Der Zeitpunkt der Bewegung bei der Bewegung verwendet 500 ms (oder eine halbe Sekunde) als Baseline, da dies die maximale Zeitspanne ist, die ein Benutzer als sofort wahrnimmt.

![Herobild](images/time.gif)

### <a name="150ms-exit"></a>**150 ms** (Beenden)

:::row:::
    :::column:::
Verwenden Sie für Objekte oder Seiten, die die Szene beenden oder schließen.
Ermöglicht ein sehr schnelles direktionales Feedback über das Beenden der Benutzeroberfläche, wobei die zeitliche Steuerung nicht bei der Framerate eine reibungslose Animation bewirkt.
    :::column-end:::
    :::column:::
        ![150 ms Bewegung](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 ms** (EINGABETASTE)

:::row:::
    :::column:::
Verwenden Sie für Objekte oder Seiten, die in die Szene eintreten oder geöffnet werden.
Ermöglicht eine angemessene Zeitspanne zum Feiern von Inhalten, während Sie in die Szene gelangt.
    :::column-end:::
    :::column:::
        ![300 ms Bewegung](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**500 ms** (verschieben)

:::row:::
    :::column:::
Verwenden Sie für Objekte, die über eine einzelne Szene oder mehrere Szenen übersetzt werden. 
    :::column-end:::
    :::column:::
        ![500 ms Bewegung](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Beschleunigung in fließender Bewegung

Die Beschleunigung ist eine Möglichkeit, die Geschwindigkeit eines Objekts bei der Übertragung zu verändern. Es ist der Kleber, der alle fließenden Bewegungsmöglichkeiten verbindet. Im Extremfall hilft die im System verwendete Beschleunigung dabei, das physische Gefühl von Objekten zu vereinheitlichen, die im gesamten System verschoben werden. Dies ist eine Möglichkeit, die reale Welt nachzuahmen und die Objekte in Bewegung zu machen, die Sie in Ihrer Umgebung angehören.

![Herobild](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Beschleunigung auf Bewegung anwenden

Diese Lösungen helfen Ihnen, ein natürlicheres Verhalten zu erreichen, und sind die Baseline, die wir für eine fließende Bewegung verwenden.

In den Codebeispielen wird gezeigt, wie Empfohlene Beschleunigungswerte auf Storyboard-Animationen (XAML) oder Kompositions Animationen (c#) angewendet werden.

### <a name="accelerate-exit"></a>**Beschleunigung** (Beenden)

:::row:::
    :::column:::
Verwenden Sie für die Benutzeroberfläche oder Objekte, die die Szene beenden.

Objekte werden eingeschaltet und gewinnen einen Schwung, bis Sie die escapegeschwindigkeit erreichen.
Das resultierende Gefühl ist, dass das Objekt versucht, die Benutzeroberfläche zu verlassen und Platz für neue Inhalte zu schaffen.
    :::column-end:::
    :::column:::
        ![Beschleunigung beschleunigen](images/accelEase.gif)
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

### <a name="decelerate-enter"></a>**Verlangsamt** (EINGABETASTE)

:::row:::
    :::column:::
Verwenden Sie für Objekte oder Benutzeroberflächen, die in die Szene wechseln, entweder Navigieren oder erzeugen.

In der Szene wird das Objekt mit extremer Reibung erreicht, wodurch das Objekt auf Rest verlangsamt wird.
Das resultierende Gefühl ist, dass das Objekt von einem langen Abstand entfernt und mit extremer Geschwindigkeit eingegeben wurde oder schnell in einen Rest-Zustand zurückkehrt.

Auch wenn Ihnen ein Zeitpunkt der nicht Reaktionsfähigkeit vorangestellt ist, hat die Geschwindigkeit des eingehenden Objekts den Effekt, dass es schnell und reaktionsfähig ist.
    :::column-end:::
    :::column:::
        ![Beschleunigung verlangsamen](images/decelEase.gif)
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

### <a name="standard-easing-move"></a>**Standard** Beschleunigung (verschieben)

:::row:::
    :::column:::
Dies ist die grundlegende Beschleunigung für alle animierten Parameteränderungen innerhalb des Systems.
Verwenden Sie die standardmäßige Beschleunigung für Objekte, die sich auf dem Bildschirm vom Zustand in den Zustand ändern, z. b. eine einfache Positionsänderung Verwenden Sie diese auch für Objekte, die in Szene verwandelt werden, z. b. ein Objekt, das wächst.

Das resultierende Gefühl ist, dass Objekte, die den Zustand von A zu B ändern, von den natürlichen Kräften überwunden und übernommen werden.
    :::column-end:::
    :::column:::
        ![Standard Beschleunigung](images/standardEase.gif)
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

- [Übersicht über die Bewegung](index.md)
- [Richtung und Schwerkraft](directionality-and-gravity.md)
