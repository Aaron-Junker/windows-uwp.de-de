---
title: Dämpfung- und Spotlight-Faktor
description: Die diffusen und spiegelnden Beleuchtungskomponenten der globalen Beleuchtungsgleichung enthalten Begriffe, die die Abschwächung des Lichts und den Spotlicht-Kegel beschreiben.
ms.assetid: F61D4ACB-09AB-4087-9E2D-224E472D6196
keywords:
- Dämpfung- und Spotlight-Faktor
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8126ac8fa738a2b8a9680d215179fe23f77c5d44
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659295"
---
# <a name="attenuation-and-spotlight-factor"></a>Dämpfung- und Spotlight-Faktor


Die diffusen und spiegelnden Beleuchtungskomponenten der globalen Beleuchtungsgleichung enthalten Begriffe, die die Abschwächung des Lichts und den Spotlicht-Kegel beschreiben. Diese Begriffe werden nachfolgend beschrieben.

## <a name="span-idattenuationspanspan-idattenuationspanspan-idattenuationspanattenuation"></a><span id="Attenuation"></span><span id="attenuation"></span><span id="ATTENUATION"></span>Attenuation


Die Lichtdämpfung hängt vom Typ des Lichts und dem Abstand zwischen dem Licht und der Vertexposition ab. Verwenden Sie eine der folgenden Formeln, um die Dämpfung zu berechnen.

Atten = 1 / (att0<sub>ich</sub> + att1<sub>ich</sub> \* d + att2<sub>ich</sub> \* d²)

Dabei gilt:

| Parameter        | Standardwert | Typ           | Beschreibung                                     | Bereich          |
|------------------|---------------|----------------|-------------------------------------------------|----------------|
| att0<sub>i</sub> | 0.0           | Gleitkomma | Konstanter Dämpfungsfaktor                     | 0 bis +unendlich |
| att1<sub>i</sub> | 0.0           | Gleitkomma | Linearer Dämpfungsfaktor                       | 0 bis +unendlich |
| att2<sub>i</sub> | 0.0           | Gleitkomma | Quadratischer Dämpfungsfaktor                    | 0 bis +unendlich |
| d                | n. a.           | Gleitkomma | Abstand zwischen Vertexposition und Position der Lichtquelle | n. a.            |

 

-   Dämpfung = 1, wenn das Licht ein gerichtetes Licht ist.
-   Dämpfung = 0, wenn der Abstand zwischen Licht und Vertex die Reichweite des Lichts überschreitet.

Der Abstand zwischen Lichtquelle und Vertexposition ist immer positiv.

d = | L<sub>dir</sub> |

Dabei gilt:

| Parameter       | Standardwert | Typ                                             | Beschreibung                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dir</sub> | n. a.           | 3D-Vektor mit X-, Y- und Z-Gleitkommawerten | Richtungsvektor von der Vertexposition bis zur Position der Lichtquelle |

 

Wenn d größer als die Reichweite des Lichts ist, nimmt Direct3D keine weiteren abnehmenden Berechnungen vor und wendet keine Effekte von der Lichtquelle bis zum Vertex an.

Die Dämpfungskonstanten fungieren in der Formel als Koeffizient – Sie können eine Vielzahl von Dämpfungskurven erstellen, indem Sie diese einfach anpassen. Sie können Dämpfung1 auf 1,0 einstellen, um ein Licht zu erstellen, das sich nicht dämpfen lässt aber trotzdem von der Reichweite begrenzt ist. Sie können ebenfalls mit verschiedenen Werten experimentieren, um verschiedene Dämpfungseffekte zu erzielen.

Die Dämpfung bei maximaler Reichweite des Lichts ist 0,0. Um zu verhindern, dass Lichter plötzlich angezeigt werden, wenn sie sich in Reichweite des Lichts befinden, kann eine Anwendung die Reichweite des Lichts erhöhen. Die Anwendung kann ebenfalls Dämpfungskonstanten so einrichten, dass der Dämpfungsfaktor nahe an der Reichweite 0,0 des Lichts liegt. Der Dämpfungswert wird mit den roten, grünen und blauen Komponenten der Farbe des Lichts multipliziert, um die Intensität des Lichts als Faktor der Distanz zu skalieren, die das Licht bis zum Vertex zurücklegt.

## <a name="span-idspotlight-factorspanspan-idspotlight-factorspanspan-idspotlight-factorspanspotlight-factor"></a><span id="Spotlight-Factor"></span><span id="spotlight-factor"></span><span id="SPOTLIGHT-FACTOR"></span>Spotlight-Faktor


Die folgende Gleichung legt den Spotlight-Faktor fest.

![Gleichung für den Spotlight-Faktor](images/dx8light9.png)

| Parameter         | Standardwert | Typ           | Beschreibung                              | Bereich                    |
|-------------------|---------------|----------------|------------------------------------------|--------------------------|
| rho<sub>i</sub>   | n. a.           | Gleitkomma | Kosinus(Winkel) für Spotlight i            | n. a.                      |
| phi<sub>i</sub>   | 0.0           | Gleitkomma | Halbschatten-Winkel für Spotlight i nach Bogenmaß | \[Theta<sub>ich</sub>, Pi) |
| theta<sub>i</sub> | 0.0           | Gleitkomma | Kernschatten-Winkel für Spotlight i nach Bogenmaß    | \[0, Pi)                 |
| Farbverlauf           | 0.0           | Gleitkomma | Farbverlaufsfaktor                           | (-unendlich +unendlich)   |

 

Dabei gilt:

rho = norm(L<sub>dcs</sub>)<sup>.</sup>norm(L<sub>dir</sub>)

und

| Parameter       | Standardwert | Typ                                             | Beschreibung                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dcs</sub> | n. a.           | 3D-Vektor mit X-, Y- und Z-Gleitkommawerten | Der negativen Wert der Lichteinfallsrichtung im Kamerabereich         |
| L<sub>dir</sub> | n. a.           | 3D-Vektor mit X-, Y- und Z-Gleitkommawerten | Richtungsvektor von der Vertexposition bis zur Position der Lichtquelle |

 

Nach dem Berechnen der Lichtdämpfung berücksichtigt Direct3D auch eventuelle Spotlight-Effekte, den vom Licht von der Oberfläche reflektierten Winkel und den Reflexionsgrad des aktuellen Materials, um die diffusen und Glanzlichtkomponenten für den Vertex zu berechnen. Siehe auch Spotlight unter [Lichttypen](light-types.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Mathematik der Beleuchtung](mathematics-of-lighting.md)

 

 




