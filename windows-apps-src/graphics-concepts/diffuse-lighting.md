---
title: Diffuse Beleuchtung
description: Die diffuse Beleuchtung arbeitet mit der Beleuchtungsrichtung und dem Normalvektor der Objektoberfläche.
ms.assetid: 8AF78742-76B1-4BBB-86E3-94AE6F48B847
keywords:
- Diffuse Beleuchtung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1785b06aa2217e8ec15aeaa560bd98a65522df2e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603375"
---
# <a name="diffuse-lighting"></a>Diffuse Beleuchtung


*Diffuse Beleuchtung* hängt sowohl von der Richtung des Lichts als auch von der Flächennormalen des Objekts ab. Diffuse Beleuchtung ist auf der Oberfläche eines Objekts durch die Änderung der Richtung des Lichts und den sich ändernden Oberflächenzahlvektor unterschiedlich. Es dauert länger, diffuse Beleuchtung zu berechnen, da sie sich für jeden Eckpunkt des Objektes ändert; der Vorteil der Verwendung liegt jedoch darin, dass sie Objekten einen Schatten und eine dreidimensionale (3D) Tiefe verleiht.

Nach dem Anpassen der Lichtintensität für Dämpfungseffekte berechnet die Beleuchtungs-Engine wie viel verbleibendes Licht entsprechend des Winkels des Vertex-Normalvektors und der Beleuchtungsrichtung durch einen Vertex reflektiert wird. Die Beleuchtungs-Engine überspringt diesen Schritt für direktionale Beleuchtungsquellen, denn diese werden nicht durch Entfernung gedämpft. Das System berücksichtigt zwei Reflektionstypen, die Diffuse- und Glanz-Reflektion. Es verwendet unterschiedliche Formeln zur Ermittlung des jeweils reflektierten Lichts.

Nachdem der Berechnung der reflektierten Lichtmenge wendet Direct3D die neuen Werte auf die Eigenschaften für die Diffuse- und Glanz-Reflektion des aktuellen Materials an. Die resultierenden Farbwerte sind Diffuse- und Glanz-Komponenten, die vom Rasterizer für das Gouraud-Shading und die Glanz-Hervorhebung genutzt werden.

Die diffuse Beleuchtung ist durch die folgende Gleichung definiert.

Diffuse Beleuchtung = Summe\[C<sub>d</sub>\*L<sub>d</sub>\*(N<sup>.</sup> L<sub>Dir</sub>)\*Atten\*Stelle\]

| Parameter       | Standardwert | Typ          | Beschreibung                                                                                      |
|-----------------|---------------|---------------|--------------------------------------------------------------------------------------------------|
| Summe             | n. a.           | n. a.           | Summe der Diffuse-Komponente der einzelnen Lichtquellen.                                                     |
| C<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Diffuse-Farbe.                                                                                   |
| L<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Licht-Diffuse-Farbe.                                                                             |
| N               | n. a.           | D3DVECTOR     | Vertexnormale                                                                                    |
| L<sub>dir</sub> | n. a.           | D3DVECTOR     | Richtungsvektor vom Objektvertext zur Lichtquelle                                                |
| Dämpfung           | n. a.           | FLOAT         | Lichtdämpfung Siehe [Abschwächungs- und Spotlicht-Faktor](attenuation-and-spotlight-factor.md). |
| Spotlight            | n. a.           | FLOAT         | Spotlight-Faktor. Siehe [Abschwächungs- und Spotlicht-Faktor](attenuation-and-spotlight-factor.md).  |

 

Weitere Informationen zur Berechnung der Dämpfung- (Atten) oder Spotlight-Eigenschaften (Spot) finden Sie unter [Dämpfung- und Spotlight-Faktor](attenuation-and-spotlight-factor.md).

Nach der separaten Verarbeitung und Interpolation aller Lichtquellen werden Diffuse-Komponenten auf Werte zwischen 0 und 255 beschränkt. Der resultierende Diffuse-Beleuchtungswert ist eine Kombination aus der Ambiente-, Diffuse- und Emission-Lichtwerte.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


In diesem Beispiel wird das Objekt über die Diffuse-Lichtfarbe und die Diffuse-Materialfarbe eingefärbt.

Gemäß der Gleichung ist die resultierende Farbe für die Objektvertices eine Kombination aus der Materialfarbe und der Lichtfarbe.

Die folgenden beiden Abbildungen zeigen die materielle Farbe (in Grau) und die Farbe des Lichts (hellrot).

![Abbildung einer grauen Kugel](images/amb1.jpg)![Abbildung einer roten Kugel](images/lightred.jpg)

Die resultierende Szene wird in der folgenden Abbildung dargestellt. Das einzige Objekt in der Szene ist eine Kugel. Die Berechnung der diffusen Beleuchtung nimmt die Diffuse-Material- und die Diffuse-Lichtfarbe und ändert diese über das Skalarprodukt entsprechend des Winkels zwischen der Beleuchtungsrichtung und der Vertext-Normalen. Aus diesem Grund ist die Rückseite der Kugel dunkler. Denn hier zeigt die Oberfläche der Kugel vom Licht weg.

![Abbildung einer Kugel mit diffuser Beleuchtung](images/lightd.jpg)

Durch die Kombination von diffuser Beleuchtung und Abiente-Beleuchtung aus dem vorherigen Beispiel ist die gesamte Fläche des Objekts schattiert. Die Ambiente-Beleuchtung schattiert die gesamte Oberfläche. Die diffuse Beleuchtung hebt die 3D-Form des Objektes hervor (siehe nächste Abbildung).

![Abbildung einer Kugel mit diffuser Beleuchtung und Ambiente-Beleuchtung](images/lightad.jpg)

Die Berechnung der diffusen Beleuchtung ist aufwendig als die der Ambiente-Beleuchtung. Da sie von der Vertex-Normalen und die Beleuchtungsrichtung abhängig ist, können Sie die Geometrie des Objektes im 3D-Raum erkennen. Dies sorgt für eine realistischer Beleuchtung als bei der Ambiente-Beleuchtung. Sie können Glanzlichter nutzen, um ein realistischeres Erscheinungsbild zu erreichen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Mathematik der Beleuchtung](mathematics-of-lighting.md)

 

 




