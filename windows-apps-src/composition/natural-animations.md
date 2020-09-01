---
title: Naturbewegungs Animationen
description: Erfahren Sie mehr übernatürliche Animations Animationen und deren Verwendung in ihrer App-Benutzeroberfläche.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: 02c76991a60205042642f57fed475755db8c8071
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174084"
---
# <a name="natural-motion-animations"></a>Naturbewegungs Animationen

Dieser Artikel bietet eine kurze Übersicht über den naturalmotionanimation-Bereich und erläutert, wie Sie sich konzeptionell Gedanken über die Verwendung dieser Animations Typen in der Benutzeroberfläche machen.

## <a name="making-motion-feel-familiar-and-natural"></a>Machen Sie sich mit der Bewegung vertraut.

Großartige apps sind solche, die Benutzeroberflächen erstellen, die die Benutzer Aufmerksamkeit erfassen und beibehalten, und Benutzer durch Aufgaben leiten. Motion ist der entscheidende Faktor, bei dem eine Benutzeroberfläche von einer Benutzeroberfläche getrennt wird – eine Verbindung zwischen Benutzern und der Anwendung, mit der Sie interagieren. Je besser die Verbindung, desto höher das Engagement und die Zufriedenheit der Endbenutzer.

Eine Möglichkeit, diese Verbindung zu erstellen, besteht darin, Benutzeroberflächen zu erstellen, die den Benutzern vertraut sind. Benutzer haben eine unbewusste Erwartung, wie Sie Bewegung auf der Grundlage von realen Erfahrungen erkennen. Wir sehen, wie sich Objekte im Boden befinden, aus der Tabelle springen, aufeinander springen und mit einer springenden Seite schwingen. Bewegung, die diese Erwartung durch die Verwendung auf der realen Physik nutzt, sieht in unserer Ansicht etwas natürlicher aus. Der bewegungsprozess wird natürlicher und interaktiver, aber noch wichtiger ist, dass die gesamte-Darstellung etwas wichtiger wird.

![Bewegung ohne Animations ](images/animation/scale-no-animation.gif)
 ![ Skalierungs Bewegung mit kubischer Bézier- ](images/animation/scale-cubic-bezier.gif)
 ![ Skalierungs Bewegung mit Spring Animation skalieren](images/animation/scale-spring.gif)

Das Ergebnis ist ein höheres Benutzer Engagement und eine Beibehaltung der app.

## <a name="balancing-control-and-dynamism"></a>Ausgleichs Kontrolle und-Dynamik

In der herkömmlichen Benutzeroberfläche sind [Keyframeanimation](/uwp/api/windows.ui.composition.keyframeanimation)s die vorherrschende Methode zum Beschreiben von Bewegung. Keyframes bieten Designern und Entwicklern die größtmögliche Kontrolle über das Definieren von Start, Ende und interpolung. Obwohl dies in vielen Fällen sehr nützlich ist, sind Keyframe-Animationen nicht sehr dynamisch. die Bewegung ist nicht adaptiv und wird unter jeder Bedingung identisch sein.

Am anderen Ende des Spektrums werden Simulationen häufig in Gaming-und Physik-Engines angezeigt. Diese Oberflächen sind häufig die gängigsten und dynamischen Möglichkeiten, mit denen Benutzer interagieren – und so den Sinn von Ambiente und Zufälligkeit erzielen, die Benutzern jeden Tag angezeigt werden. Obwohl das Gefühl der Bewegung lebendiger und dynamischer ist, haben Designer und Entwickler weniger Kontrolle, sodass es schwieriger ist, Sie in die herkömmliche Benutzeroberfläche zu integrieren.

![Diagramm für das steuerungsspektrum](images/animation/natural-motion-diagram.png)

[Naturalmotionanimation](/uwp/api/windows.ui.composition.naturalmotionanimation)s ist vorhanden, um diese Unterteilung zu unterstützen – die Aktivierung eines Steuer Elements für die wichtigen Elemente einer Animation wie Start/Finish, aber die Aufrechterhaltung von Bewegung, die sich in natürlicher und dynamischer Weise sieht.

> [!NOTE]
> Naturalmotionanimationen sind nicht als Ersatz für Keyframe-Animationen gedacht – es gibt immer noch Positionen in der fließenden Entwurfs Sprache, in denen Keyframes empfohlen werden. "Naturalmotionanimationen" ist für die Verwendung an Stellen gedacht, an denen Bewegung erforderlich ist, Keyframe-Animationen aber nicht dynamisch genug sind.

## <a name="using-naturalmotionanimations"></a>Verwenden von naturalmotionanimationen

Ab dem Fall Creators Update haben Sie Zugriff auf eine neue Bewegungs Darstellung: Spring- **Animationen**. Eine ausführlichere Exemplarische Vorgehensweise von Quellen finden Sie unter [Spring Animationen](spring-animations.md) .

Dieser bewegstyp wird mithilfe der neuen naturalmotionanimation erreicht – ein neuer Animationstyp, der darauf ausgerichtet ist, Entwicklern den Einstieg in die Benutzeroberfläche zu ermöglichen und ein ausgewogenes Verhältnis zwischen Kontrolle und Dynamik zu erzielen. Sie stellen die folgenden Funktionen zur Verfügung:

- Hiermit werden die Anfangs-und Endwerte definiert.
- Definieren Sie InitialVelocity, und binden Sie die Eingabe mit interaktiontracker ein.
- Definieren von Bewegungs spezifischen Eigenschaften (z. b. "dämpfingratio" für Springs)

Allgemeine Formel für den Einstieg:

1. Erstellen Sie die naturalmotionanimation über den Compositor, indem Sie eine der **Create** -Methoden verwenden.
1. Definieren Sie die Eigenschaften der Animation.
1. Übergeben Sie die "naturalmotionanimation" als Parameter an den startanimation-Rückruf von einem "compositionobject".
    - Oder auf die Motion-Eigenschaft eines interaktiontracker-inertiamodifier festgelegt ist.

Ein einfaches Beispiel für die Verwendung von Spring naturalmotionanimation, um eine visuelle Spring-Eigenschaft an einem neuen X-Offset-Speicherort zu erstellen:

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```