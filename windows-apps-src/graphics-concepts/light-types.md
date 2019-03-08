---
title: Lichttypen
description: 'Mit der Eigenschaft „Lichttyp“ legen Sie fest, welchen Lichtquellentyp Sie verwenden. In Direct3D gibt es drei Lichttypen: Punktlichter, Spotlights und gerichtetes Licht.'
ms.assetid: 57748CAF-6F08-4D1D-9ED6-8FAA8C5FE314
keywords:
- Lichttypen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1815f0956fbc175fec5ca892dbeeec92b2f939ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594695"
---
# <a name="light-types"></a>Lichttypen


Mit der Eigenschaft „Lichttyp“ legen Sie fest, welchen Lichtquellentyp Sie verwenden. In Direct3D gibt es drei Lichttypen: Punktlichter, Spotlights und gerichtetes Licht. Mit variierendem Rechenaufwand beleuchtet jeder Lichttyp die Objekte in einer Szene unterschiedlich.

## <a name="span-idpointlightspanspan-idpointlightspanspan-idpointlightspanpoint-light"></a><span id="Point_Light"></span><span id="point_light"></span><span id="POINT_LIGHT"></span>Punktuelles Licht


Punktlichter haben in einer Szene Farben und Positionen, aber keine Richtung. Sie geben das Licht, wie in der folgenden Abbildung dargestellt, in alle Richtungen ab.

![Abbildung: Punktlichter](images/ptlight.png)

Eine Glühbirne ist ein gutes Beispiel für ein Punktlicht. Punktlichter werden von Dämpfung und Reichweite beeinflusst, und erhellen auf einer Scheitelpunkt-zu-Scheitelpunkt-Basis ein Gitter. Während der Beleuchtung verwendet Direct3D die Position des Punktlichts im Weltraum und die Koordinaten der leuchtenden Eckpunkte, um einen Vektor für die Richtung des Lichts und der vom Licht zurückgelegten Entfernung herzuleiten. Beides wird zusammen mit der Scheitelpunktnormalen verwendet, um die Verteilung des Lichts zur Beleuchtung der Oberfläche zu berechnen.

## <a name="span-iddirectionallightspanspan-iddirectionallightspanspan-iddirectionallightspandirectional-light"></a><span id="Directional_Light"></span><span id="directional_light"></span><span id="DIRECTIONAL_LIGHT"></span>Gerichtetes Licht


Gerichtetes Licht hat zwar Farbe und Richtung, aber keine Position. Es strahlt paralleles Licht ab. Das bedeutet, dass die durch gerichtetes Licht erzeugte Beleuchtung sich in der gleichen Richtung durch eine Szene bewegt. Stellen Sie sich ein gerichtetes Licht als Lichtquelle in nahezu unendlich weiter Entfernung vor, beispielsweise die Sonne. Gerichtetes Licht wird nicht von Dämpfung und Reichweite beeinflusst, so dass die von Ihnen angegebene Richtung und Farbe die einzigen Faktoren bilden, die Direct3D bei der Berechnung der Scheitelpunktfarben berücksichtigt. Durch die geringe Anzahl von Beleuchtungsfaktoren handelt es sich hierbei um die Beleuchtung mit dem geringsten Rechenaufwand.

## <a name="span-idspotlightspanspan-idspotlightspanspan-idspotlightspanspotlight"></a><span id="SpotLight"></span><span id="spotlight"></span><span id="SPOTLIGHT"></span>SpotLight


Spotlights haben Farbe, Position und Richtung, in der sie Licht abstrahlen. Das von einem Spotlight abgestrahlte Licht besteht aus einem hellen inneren Kegel und einem größeren äußeren Kegel, mit abnehmender Lichtintensität dazwischen, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Spotlights mit einem inneren und einem äußeren Kegel](images/spotlt.png)

Spotlights werden von Farbverlauf, Dämpfung und Reichweite beeinflusst. Diese Faktoren sowie die zurückgelegte Entfernung des Lichts zu jedem Scheitelpunkt werden bei der Berechnung von Beleuchtungseffekten für Objekte in einer Szene einbezogen. Die Berechnung dieser Effekte für jeden Scheitelpunkt macht Spotlights von allen Lichttypen in Direct3D zu demjenigen mit dem größten Rechenaufwand.

Farbverlaufs-, Theta- und Phi-Werte werden nur von Spotlights verwendet. Diese Werte steuern, wie groß oder klein der innere und äußere Kegel des Spotlights für ein Objekt sein muss und wie das Licht zwischen beiden abnimmt.

Theta ist der Radiantenwinkel des inneren Spotlight-Kegels, während der Phi-Wert den Winkel des äußeren Lichtkegels darstellt. Der Farbverlauf steuert, wie die Lichtintensität zwischen der äußeren Kante des inneren Kegels und der inneren Kante des äußeren Kegels abnimmt. Um einen gleichmäßigen Farbverlauf zwischen den Kegeln zu halten, setzen die meisten Anwendungen den Farbverlauf auf 1,0. Sie können bei Bedarf aber andere Werte einstellen.

In der folgenden Abbildung wird das Verhältnis zwischen diesen Werten und wie sie sich auf den inneren und äußeren Lichtkegel auswirken dargestellt.

![Abbildung der Auswirkung von Phi- und Theta-Werten auf die Spotlight-Kegel](images/spotlt2.png)

Spotlights strahlen einen zweitteiligen Lichtkegel ab: einen hellen inneren Kegel und einen äußeren Kegel. Im inneren Kegel ist das Licht am hellsten, und außerhalb des äußeren Kegels ist es, mit abnehmender Lichtintensität zwischen den beiden Bereichen, nicht mehr vorhanden. Diese Art der Dämpfung wird auch als Farbverlauf bezeichnet.

Die Lichtmenge, die ein Scheitelpunkt erhält, basiert auf der Position des Scheitelpunkts in den inneren oder äußeren Kegeln. Direct3D berechnet das Skalarprodukt des Richtungsvektors (L) des Spotlights und des Vektors vom Licht zum Scheitelpunkt (D). Dieser Wert entspricht dem Kosinus des Winkels zwischen den beiden Vektoren und dient als Angabe der Position des Scheitelpunkts. Diese Position kann mit dem Kegelwinkel des Lichts verglichen werden, um zu bestimmen, wo der Scheitelpunkt im inneren oder äußeren Kegel liegt. Die folgende Abbildung stellt den Zusammenhang zwischen diesen beiden Vektoren graphisch dar.

![Abbildung des Spotlight-Richtungsvektors und des Vektors vom Scheitelpunkt zum Spotlight](images/spotalg1.png)

Das System vergleicht diesen Wert mit dem Kosinus der inneren und äußeren Kegelwinkel des Spotlights. Die Theta- und Phi-Werte des Lichts stellen die Gesamtkegelwinkel für die inneren und äußeren Kegel dar. Da eine Dämpfung eintritt, je weiter sich der Scheitelpunkt von der Beleuchtungsmitte entfernt (als über den Gesamtkegelwinkel), werden diese Kegelwinkel zur Laufzeit durch zwei geteilt, bevor deren Kosinus berechnet wird.

Wenn das Skalarprodukt der Vektoren L und D kleiner oder gleich dem Kosinus des äußeren Kegelwinkels ist, liegt der Scheitelpunkt außerhalb des äußeren Kegels und erhält kein Licht. Wenn das Skalarprodukt von L und D größer als der Kosinus des inneren Kegelwinkels ist, liegt der Scheitelpunkt im inneren Kegel und erhält die maximale Lichtmenge, jedoch unter Berücksichtigung der Dämpfung über die Entfernung. Befindet sich der Scheitelpunkt zwischen den beiden Regionen, wird der Farbverlauf mit folgender Gleichung berechnet.

![Formel für Lichtintensität am Scheitelpunkt, nach Farbverlauf](images/falloff.png)

Dabei gilt Folgendes:

-   I<sub>f</sub> ist die Lichtintensität nach dem Farbverlauf
-   Alpha ist der Winkel zwischen den Vektoren L und D
-   Theta ist der innere Kegelwinkel
-   Phi ist der äußere Kegelwinkel
-   p ist der Farbverlauf

Diese Formel ergibt einen Wert zwischen 0,0 und 1,0, der die Lichtintensität am Scheitelpunkt unter Berücksichtigung des Farbverlaufs skaliert. Dämpfung wird als ein Faktor der Entfernung des Scheitelpunkts vom Licht angewendet. Die folgende Grafik zeigt, wie unterschiedlich sich Farbverlaufwerte auf die Farbverlaufkurve auswirken können.

![Grafik der Lichtintensität gegenüber dem Abstand des Scheitelpunkts vom Licht](images/fallgraf.png)

Die Auswirkung unterschiedlicher Farbverlaufwerte auf die tatsächliche Beleuchtung ist subtil. Durch das Formen der Farbverlaufkurve mit Farbverlaufwerten ungleich 1,0 entsteht eine kleine Leistungseinbuße. Aus diesem Grund wird dieser Wert in der Regel auf 1,0 gesetzt.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Lichter und Materialien](lights-and-materials.md)

 

 




