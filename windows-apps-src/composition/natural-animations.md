---
title: Natürliche Bewegungsanimationen
description: Lernen Sie natürliche Animationen und deren Nutzung in der App-Benutzeroberfläche kennen.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 7fde0cbf5335b4f5c3da2f21f692fc2c23455776
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630375"
---
# <a name="natural-motion-animations"></a>Natürliche Bewegungsanimationen

Dieser Artikel gibt einen kurzen Überblick über den NaturalMotionAnimation-Raum und wie man konzeptionell mit diesen Animationen in der Benutzeroberfläche umgeht.

## <a name="making-motion-feel-familiar-and-natural"></a>Bewegung vertraut und natürlich machen

Tolle Apps sind solche, die Erlebnisse schaffen, die die Aufmerksamkeit der Benutzer auf sich ziehen und behalten und helfen, sie durch Aufgaben zu führen. Bewegung ist der entscheidende Unterscheidungsfaktor, der eine schlichte Benutzeroberfläche von einer Benutzererfahrung trennt, und eine Verbindung zwischen Benutzern und der Anwendung herstellt. Je besser diese Verbindung, desto mehr Engagement und Zufriedenheit der Endanwender.

Eine Möglichkeit dabei ist durch Bewegung Erfahrungen zu schaffen, die vertraut aussehen und sich vertraut anfühlen. Die Benutzer haben eine unbewusste Erwartung, wie sie Bewegungen wahrnehmen, die auf realen Lebenserfahrungen basieren. Wir sehen, wie Gegenstände über den Boden rutschen, vom Tisch fallen, ineinander hüpfen und mit einer Feder oszillieren. Bewegung, die diese Erwartung nutzt, indem sie auf der realen Welt der Physik basiert, sehen und fühlen sich natürlicher an. Die Bewegung wird natürlicher und interaktiver. Aber noch wichtiger ist, dass das gesamte Erlebnis unvergesslicher und angenehmer wird.

![Skalieren Sie während der Übertragung ohne Animation](images/animation/scale-no-animation.gif)
![Skalierung während der Übertragung mit kubische Bezier](images/animation/scale-cubic-bezier.gif)
![Skalierung während der Übertragung mit Spring-Animation](images/animation/scale-spring.gif)

Das Ergebnis ist eine höhere Bindung und Bindung der Nutzer an die App.

## <a name="balancing-control-and-dynamism"></a>Balance von Kontrolle und Dynamik

In der traditionellen Benutzeroberfläche sind [KeyFrameAnimations](https://docs.microsoft.com/uwp/api/windows.ui.composition.keyframeanimation) die vorherrschende Art, Bewegung zu beschreiben. KeyFrames lieferte Designern und Entwicklern die größtmögliche Kontrolle, um Anfang, Ende und Interpolation zu definieren. Obwohl dies in vielen Fällen sehr nützlich ist, sind KeyFrame-Animationen nicht sehr dynamisch; die Bewegung ist nicht adaptiv und sieht unter allen Umständen gleich aus.

Am anderen Ende des Spektrums gibt es Simulationen, wie sie oft in Gaming- und Physik-Engines zu sehen sind. Diese Erfahrungen sind oft die lebensechtesten und dynamischsten, mit denen Benutzer interagieren – sie schaffen das Gefühl von Ambiente und Zufälligkeit, das sie jeden Tag sehen. Obwohl sich Bewegungen dadurch lebendiger und dynamischer anfühlen, haben Designer und Entwickler weniger Kontrolle, wodurch die Integration in herkömmliche Benutzeroberflächen erschwert wird.

![Kontrollspektrumdiagramm](images/animation/natural-motion-diagram.png)

[NaturalMotionAnimations](https://docs.microsoft.com/uwp/api/windows.ui.composition.naturalmotionanimation) gibt es, um diese Kluft zu überbrücken – um eine Balance zwischen Kontrolle für die wichtigen Elemente einer Animation wie Start/Ziel zu ermöglichen, aber eine Bewegung beizubehalten, die natürlich aussieht und sich natürlich und dynamisch anfühlt.

> [!NOTE]
> NaturalMotionAnimations sind nicht als Ersatz für KeyFrame-Animationen gedacht – es gibt immer noch Stellen in der Fluent Design-Sprache, an denen KeyFrames empfohlen werden. NaturalMotionAnimations sind für den Einsatz an Orten gedacht, an denen Bewegung benötigt wird, KeyFrame Animationen aber nicht dynamisch genug sind.

## <a name="using-naturalmotionanimations"></a>Verwenden von NaturalMotionAnimations

Mit dem Fall Creators Update haben Sie Zugriff auf ein neues Bewegungserlebnis: **Federanimationen**. Siehe [Federanimationen](spring-animations.md) für ausführliche Informationen.

Erreicht wird diese Bewegungsart durch den Einsatz der neuen NaturalMotionAnimation – ein neuer Animationstyp, der es Entwicklern ermöglicht, vertraute und natürliche Bewegungen in ihre Benutzeroberfläche einzubauen, mit einer Balance aus Kontrolle und Dynamik. Sie bietet die folgenden Fähigkeiten:

- Angeben von Start- und Endwerten.
- Definieren von InitialVelocity und Anbindung an die Eingaben mit InteractionTracker.
- Bewegungsspezifische Eigenschaften definieren (z. B. DampingRatio für Federanimationen).

Allgemeine Formel für den Einstieg:

1. Erstellen Sie die NaturalMotionAnimation aus dem Compositor mit einer der **Create**-Methoden.
1. Definieren Sie die Eigenschaften der Animation.
1. Übergeben Sie die NaturalMotionAnimation als Parameter an den StartAnimation-Aufruf eines CompositionObjects.
    - Oder setzen Sie die Motion-Eigenschaft eines InteractionTracker InertiaModifiers.

Ein einfaches Beispiel mit einer NaturalMotionAnimation-Federanimation, um eine visuelle „Feder“ zu einer neuen X-Offset-Position zu bewegen:

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```
