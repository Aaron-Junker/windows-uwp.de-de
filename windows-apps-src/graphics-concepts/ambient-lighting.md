---
title: Umgebungslicht
description: Umgebungsbeleuchtung bietet konstante Beleuchtung für eine Szene.
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- Umgebungslicht
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ac958a93fcafbb33a9025196b49398e2e3269e55
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291838"
---
# <a name="ambient-lighting"></a>Umgebungslicht

Umgebungsbeleuchtung bietet konstante Beleuchtung für eine Szene. Sie leuchtet alle Objekteckpunkte gleich aus, da sie nicht von anderen Beleuchtungsfaktoren abhängig ist, wie der Eckpunktnormalen, der Richtung des Lichts, der Position der Lichtquelle, der Reichweite oder Abschwächung des Lichts. Umgebungsbeleuchtung ist in alle Richtungen konstant und färbt alle Pixel eines Objekts identisch. Sie lässt sich schnell berechnen, bewirkt jedoch, dass Objekte flach und unrealistisch aussehen.

Das Umgebungslicht ist zwar der schnellste Beleuchtungstyp, es erzeugt allerdings die unrealistischsten Ergebnisse. Direct3D enthält eine einzige globale Umgebungslichteigenschaft, die Sie ohne Erstellen des Lichts verwenden können. Alternativ können Sie jede Lichtquellen als Umgebungslicht festlegen.

Das Umgebungslicht für eine Szene wird durch die folgende Gleichung beschrieben.

Umgebender = Cₐ\*\[Gₐ + Sum (Atten<sub>ich</sub>\*Stelle<sub>ich</sub>\*L<sub>Ai</sub>)\]

Dabei gilt:

| Parameter         | Standardwert | Typ          | Beschreibung                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| Cₐ                | (0,0,0,0)     | D3DCOLORVALUE | Materielle Umgebungsfarbe                                                                                            |
| Gₐ                | (0,0,0,0)     | D3DCOLORVALUE | Globale Umgebungsfarbe                                                                                              |
| Atten<sub>i</sub> | (0,0,0,0)     | D3DCOLORVALUE | Dämpfung der ith-Beleuchtung. Siehe [Abschwächungs- und Spotlicht-Faktor](attenuation-and-spotlight-factor.md). |
| Spot<sub>i</sub>  | (0,0,0,0)     | D3DVECTOR     | Spotlight-Faktor der ith-Beleuchtung. Siehe [Abschwächungs- und Spotlicht-Faktor](attenuation-and-spotlight-factor.md).  |
| Summe               | Nicht zutreffend           | Nicht zutreffend           | Summe des Umgebungslichts                                                                                          |
| L<sub>ai</sub>    | (0,0,0,0)     | D3DVECTOR     | Helle Umgebungsfarbe der ith-Beleuchtung                                                                              |

 

Der Wert für Cₐ ist entweder:

-   Vertex "Farbe1", wenn AMBIENTMATERIALSOURCE = D3DMCS\_"Farbe1", und der ersten Vertexfarbe in der Vertexdeklaration angegeben wird.
-   Vertex-color2, wenn AMBIENTMATERIALSOURCE = D3DMCS\_COLOR2 und die zweite Vertexfarbe in der Vertexdeklaration angegeben wird.
-   Materielle Umgebungsfarbe.

**Beachten Sie**    Wenn entweder AMBIENTMATERIALSOURCE-Option verwendet wird, und die Vertexfarbe wurde nicht angegeben, wird die Material Ambiente-Farbe verwendet.

 

Um die materielle Umgebungsfarbe anzuwenden, verwenden Sie „SetMaterial” wie im folgenden Beispielcode dargestellt.

Gₐ ist die globale Umgebungsfarbe. Mithilfe von SetRenderState festgelegt (D3DRS\_AMBIENT). In einer Direct3D-Szene gibt es eine globale Umgebungsfarbe. Dieser Parameter ist keinem Direct3D-Lichtobjekt zugeordnet.

L<sub>ai</sub> ist die Umgebungsfarbe der ith-Beleuchtung in der Szene. Jedes Direct3D-Licht hat eine Reihe von Eigenschaften, von denen eine die Umgebungsfarbe ist. Der Begriff „Summe” (L<sub>Ai</sub>) beschreibt die Summe aller Umgebungsfarben in der Szene.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


In diesem Beispiel wird das Objekt durch die Farbe des Umgebungslichts der Szene und einer materiellen Umgebungsfarbe hervorgehoben.

```cpp
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

Gemäß der Gleichung ist die resultierende Farbe für die Objektvertices eine Kombination aus der Materialfarbe und der Lichtfarbe.

Die folgenden beiden Abbildungen zeigen die materielle Farbe (in Grau) und die Farbe des Lichts (hellrot).

![Abbildung einer grauen Kugel](images/amb1.jpg)![Abbildung einer roten Kugel](images/lightred.jpg)

Die resultierende Szene wird in der folgenden Abbildung dargestellt. Das einzige Objekt in der Szene ist eine Kugel. Das Umgebungslicht beleuchtet alle Objekt-Schnittpunkte mit derselben Farbe. Es ist nicht von der Vertexnormale oder Lichteinfallsrichtung abhängig. Die Kugel sieht wie ein 2D-Kreis aus, da es keinen Unterschied in der Schattierung der Oberfläche des Objekts gibt.

![Abbildung einer Kugel mit Umgebungslicht](images/lighta.jpg)

Verwenden Sie neben dem Umgebungslicht diffuses oder Glanzlicht, um Objekten ein realistischeres Aussehen zu verleihen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Mathematik der Beleuchtung](mathematics-of-lighting.md)

 

 




