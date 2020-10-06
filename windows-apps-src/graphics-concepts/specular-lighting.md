---
title: Spiegelbeleuchtung
description: Glanz Beleuchtung identifiziert die hellen Glanzlichter, die auftreten, wenn Light auf eine Objekt Oberfläche trifft und sich wieder auf die Kamera auswirkt.
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- Spiegelbeleuchtung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f60bd4019f330058d4396a5b0d75d00f90ecff09
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750186"
---
# <a name="specular-lighting"></a>Spiegelbeleuchtung


Glanz *Beleuchtung* identifiziert die hellen Glanzlichter, die auftreten, wenn Light auf eine Objekt Oberfläche trifft und sich wieder auf die Kamera auswirkt. Die Glanz Beleuchtung ist intensiver als diffuses Licht und wird schneller auf der Objekt Oberfläche angezeigt. Die Berechnung der Glanz Beleuchtung dauert länger als bei der diffusen Beleuchtung, aber der Vorteil der Verwendung besteht darin, dass Sie einer Oberfläche deutliche Details hinzufügt.

Das Modellieren von Glanz Reflektion erfordert, dass das System weiß, in welcher Richtung das Licht unterwegs ist, und die Richtung des viewerauges. Das System verwendet eine vereinfachte Version des Phong-reflektionsmodells, das einen halben Vektor verwendet, um die Intensität der Glanz Reflektion zu annähern.

Der Standard Beleuchtungs Zustand berechnet keine Glanzlichter.

## <a name="span-idspecular_lighting_equationspanspan-idspecular_lighting_equationspanspan-idspecular_lighting_equationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>Glanzlicht Gleichung


Die Glanz Beleuchtung wird durch die folgende Gleichung beschrieben.

> Glanz Beleuchtung = CS \* Sum \[ ls \* (N) H)<sup>P</sup> \* - \* Punkt-Punkt\]

Die Variablen, ihre Typen und ihre Bereiche lauten wie folgt:

| Parameter    | Standardwert | type                                                             | BESCHREIBUNG                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| CS           | (0, 0, 0, 0)     | Die Transparenz von rot, grün, blau und Alpha (Gleit Komma Werte) | Glanz Farbe.                                                                                        |
| Sum          | –           | –                                                              | Sumpunktions Zeichen für die Glanz Komponente der einzelnen Leuchten.                                                          |
| N            | –           | 3D-Vektor (x, y und z Gleit Komma Werte)                    | Vertex normal.                                                                                         |
| H            | –           | 3D-Vektor (x, y und z Gleit Komma Werte)                    | Halber-Wege-Vektor. Weitere Informationen finden Sie im Abschnitt zum halben Vektor.                                                |
| <sup>P</sup> | 0,0           | Gleitkomma                                                   | Glanz Fähigkeit der Reflektion. Der Bereich liegt zwischen 0 und + unendlich.                                                     |
| Ls           | (0, 0, 0, 0)     | Die Transparenz von rot, grün, blau und Alpha (Gleit Komma Werte) | Helle Glanz Farbe.                                                                                  |
| Ratte        | –           | Gleitkomma                                                   | Heller Dämpfungswert. Siehe [Dämpfung und Spotlight-Faktor](attenuation-and-spotlight-factor.md). |
| Sofortige Zahlung         | –           | Gleitkomma                                                   | Spotlight-Faktor. Siehe [Dämpfung und Spotlight-Faktor](attenuation-and-spotlight-factor.md).        |

 

Der Wert für CS lautet wie folgt:

-   Vertex Color 1, wenn es sich bei der Glanz materialquelle um die diffuse Scheitelpunkt Farbe handelt und die erste Scheitelpunkt Farbe in der Vertexdeklaration angegeben wird.
-   Vertex Color 2, wenn eine Glanz materialquelle die Glanz Farbe des Vertex ist, und die zweite Scheitelpunkt Farbe in der Vertexdeklaration.
-   Material Glanz Farbe

**Hinweis**    Wenn eine der beiden Optionen für die Quell Text Quelle verwendet wird und die Scheitelpunkt Farbe nicht angegeben wird, wird die Glanz Farbe für das Material verwendet.

 

Glanz Komponenten werden an einen Wert zwischen 0 und 255 gebunden, nachdem alle Lichter separat verarbeitet und interpoliert wurden.

## <a name="span-idthe_halfway_vectorspanspan-idthe_halfway_vectorspanspan-idthe_halfway_vectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>Der halbe Vektor


Der halbe Vektor (H) ist in der Mitte zwischen zwei Vektoren vorhanden: der Vektor von einem Objekt Scheitelpunkt zur hellen Quelle und der Vektor von einem Objekt Scheitelpunkt bis zur Kameraposition. Direct3D bietet zwei Möglichkeiten, den halbvektor zu berechnen. Wenn für die Kamera relative Glanzlichter aktiviert sind (anstelle von orthogonalen Glanzlichtern), berechnet das System den halben Vektor mithilfe der Position der Kamera und der Position des Scheitel Punkts, zusammen mit dem Richtungsvektor des Lichts. Dies wird in der folgenden Formel veranschaulicht.

> H = Norm (Norm (CP-VP) + L-<sub>dir</sub>)

 

| Parameter       | Standardwert | type                                          | BESCHREIBUNG                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| Erfolgen              | –           | 3D-Vektor (x, y und z Gleit Komma Werte) | Kameraposition.                                             |
| Füttern              | –           | 3D-Vektor (x, y und z Gleit Komma Werte) | Vertex-Position.                                             |
| L-<sub>dir</sub> | –           | 3D-Vektor (x, y und z Gleit Komma Werte) | Richtung Vektor von Scheitelpunkt Position zur hellen Position. |

 

Die Bestimmung des halb möglichen Vektors auf diese Weise kann Rechen intensiv sein. Alternativ weist die Verwendung von orthogonalen Glanzlichtern (anstelle von Kamera-relativen Glanzlichtern) das System an, so zu agieren, als ob der Standpunkt auf der z-Achse unendlich weit entfernt ist. Dies wird in der folgenden Formel dargestellt.

> H = Norm ((0,0) + L-<sub>dir</sub>)

Diese Einstellung ist weniger Rechen intensiv, aber weitaus weniger genau. Sie wird daher am besten von Anwendungen verwendet, die eine orthogonale Projektion verwenden.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


In diesem Beispiel wird das-Objekt mithilfe der Glanzlicht Farbe der Szene und einer hellen Glanz Farbe gefärbt.

Gemäß der Gleichung ist die resultierende Farbe für die Objekt Vertices eine Kombination aus der Material Farbe und der hellen Farbe.

Die folgende Abbildung zeigt die Glanz Farbe, die grau ist, und die helle helle Farbe, die weiß ist.

![Abbildung einer grauen Kugel](images/amb1.jpg)![Abbildung einer weißen Kugel](images/lightwhite.jpg)

Die sich ergebende Glanz Markierung wird in der folgenden Abbildung dargestellt.

![Abbildung der Glanz Markierung](images/lights.jpg)

Die Kombination der Glanz Markierung mit der Ambient-und diffuse Beleuchtung ergibt die folgende Abbildung. Wenn alle drei Arten von Beleuchtung angewendet werden, ähnelt dies deutlich einem realistischen Objekt.

![Darstellung der Kombination der Glanz Markierung, der Umgebungsbeleuchtung und der diffusen Beleuchtung](images/lightads.jpg)

Die Glanz Beleuchtung ist intensiver zu berechnen als diffuses Beleuchtung. Sie wird in der Regel verwendet, um visuelle Hinweise zum Oberflächenmaterial bereitzustellen. Die Glanz Markierung variiert in Größe und Farbe mit dem Material der-Oberfläche.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Beleuchtungsmathematik](mathematics-of-lighting.md)

 

 




