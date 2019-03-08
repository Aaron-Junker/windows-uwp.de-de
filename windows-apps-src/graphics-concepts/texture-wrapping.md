---
title: Texturumbruch
description: Der Texturumbruch ändert die grundlegende Weise, in der Direct3D texturierte Vielecke rasterisiert, unter Verwendung der für jeden Scheitelpunkt angegebenen Texturkoordinaten.
ms.assetid: C28FB369-9A91-4D57-A96D-4A5D36484B35
keywords:
- Texturumbruch
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6618b7573be7cd39f703299b9418d1575297120e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622625"
---
# <a name="texture-wrapping"></a>Texturumbruch


Der Texturumbruch ändert die grundlegende Weise, in der Direct3D texturierte Vielecke rasterisiert, unter Verwendung der für jeden Scheitelpunkt angegebenen Texturkoordinaten. Beim Rastern eines Vielecks interpoliert das System zwischen den Texturkoordinaten an jedem der Scheitelpunkte des Vielecks, um die Texel zu bestimmen, die für jedes Pixel des Vielecks zu verwenden sind. Normalerweise behandelt das System die Textur als 2D-Ebene und interpoliert neue Texel auf dem kürzesten Weg von Punkt A in der Textur zu Punkt B. Wenn Punkt A die u-/v-Position (0,8, 0,1) repräsentiert und sich Punkt B bei (0,1, 0,1) befindet, sieht die Interpolationslinie wie im folgenden Diagramm aus.

![Diagramm einer Interpolationslinie zwischen zwei Punkten](images/interp1.png)

Beachten Sie, dass der kürzeste Abstand zwischen A und B in dieser Abbildung ungefähr durch die Mitte der Textur verläuft. Die Aktivierung des u-/v-Texturkoordinatenumbruchs ändert, wie Direct3D den kürzesten Weg zwischen Texturkoordinaten in u- und v-Richtung wahrnimmt. Definitionsgemäß bewirkt der Texturumbruch, dass der Rasterizer den kürzesten Weg zwischen den Texturkoordinatensätzen nimmt, wobei angenommen wird, dass 0,0 und 1,0 zusammenfallen. Der letzte Aspekt ist der schwierige Teil: Sie können sich vorstellen, dass die Aktivierung Textur, die in einer Richtung wrapping wird das System in einer Textur zu behandeln, als wäre sie auf einen Zylinder umschlossen wurden. Betrachten Sie beispielsweise das folgende Diagramm.

![Diagramm einer Textur und zweier Punkte um einen Zylinder](images/interp2.png)

Die obige Abbildung zeigt, wie sich der Umbruch in u-Richtung darauf auswirkt, wie das System Texturkoordinaten interpoliert. Bei Verwendung derselben Punkte wie in dem Beispiel für normale bzw. nicht umgebrochene Texturen sehen Sie, dass der kürzeste Weg zwischen den Punkten A und B nicht mehr durch die Mitte der Textur führt, sondern über den Rand, wo 0,0 und 1,0 zusammen vorhanden sind. Der Umbruch in v-Richtung hat den ähnlichen Effekt, mit der Ausnahme, dass die Textur jetzt um einen auf der Seite liegenden Zylinder gewickelt ist. Der Umbruch in u- und v-Richtung ist komplexer. In diesem Fall können Sie sich die Textur als Torus vorstellen.

Die am häufigsten verwendete praktische Anwendung für den Texturumbruch ist die Durchführung von Umgebungszuordnungen. In der Regel erscheint ein mit einer Umgebungszuordnung texturiertes Objekt sehr reflektiv und zeigt ein Spiegelbild seiner Umgebung in der Szene. Stellen Sie sich dazu einen Raum mit vier Wänden vor, auf denen jeweils die Buchstaben R, G, B, Y und die entsprechenden Farben (rot, grün, blau und gelb) zu sehen sind. Die Umgebungszuordnung für einen solchen einfachen Raum könnte dann etwa wie in der folgenden Abbildung aussehen.

![Illustration vertikaler Streifen in roter, grüner, blauer und gelber Farbe.](images/envmap.png)

Stellen Sie sich vor, dass die Decke des Raumes von einer vollständig reflektierenden vierseitigen Säule gestützt wird. Die Zuordnung der Umgebungszuordnungstextur zu dieser Säule ist einfach; dafür zu sorgen, dass die Säule so aussieht, als ob sie die Buchstaben und die Farben reflektieren würde, ist nicht so einfach. Das folgende Diagramm zeigt einen Drahtrahmen der Säule mit einer Auflistung der jeweiligen Texturkoordinaten in der Nähe der oberen Scheitelpunkte. Der Saum für den Umbruch um die Ränder der Textur wird durch eine gepunktete Linie angezeigt.

![Diagramm eines Rechtecks mit einer gepunkteten Halbierungslinie](images/seam.png)

Wenn der Umbruch in u-Richtung aktiviert ist, zeigt die texturierte Säule die Farben und Symbole der Umgebungszuordnung, und am Saum vor der Textur wählt der Rasterizer in korrekter Weise den kürzesten Weg zwischen den Texturkoordinaten, unter der Annahme, dass sich die u-Koordinaten 0,0 und 1,0 am selben Ort befinden. Die texturierte Säule sieht wie in der folgenden Abbildung aus.

![Illustration einer Säule, die aus roten, grünen, blauen und gelben Quadranten besteht](images/tex-seam.png)

Wenn der Texturumbruch nicht aktiviert ist, interpoliert der Rasterizer nicht in der benötigten Richtung, um ein glaubwürdiges, reflektiertes Bild zu schaffen. Stattdessen enthält der Bereich an der Vorderseite der Säule ein horizontal komprimierte Version der Texel zwischen den u-Koordinaten 0,175 und 0,875 durch die Mitte der Textur. Der Umbrucheffekt geht so verloren.

Verwechseln Sie nicht den Texturumbruch mit den ähnlich bezeichneten Texturadressierungsmodi. Der Texturumbruch wird vor der Texturadressierung durchgeführt. Achten Sie darauf die Textur, umschließen die Daten enthält keine Texturkoordinaten außerhalb des Bereichs der \[0,0; 1,0\] , da dies nicht definierten Ergebnissen führen wird. Weitere Informationen zur Texturadressierung finden Sie unter [Texturadressierungsmodi](texture-addressing-modes.md).

## <a name="span-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspandisplacement-map-wrapping"></a><span id="Displacement_Map_Wrapping"></span><span id="displacement_map_wrapping"></span><span id="DISPLACEMENT_MAP_WRAPPING"></span>Wrapping Verschiebung-Karte


Ersetzungszuordnungen werden von der Mosaik-Engine interpoliert. Da der Umbruchmodus für die Mosaik-Engine nicht angegeben werden kann, ist ein Texturumbruch für Ersetzungszuordnungen nicht möglich. Eine Anwendung kann eine Reihe von Scheitelpunkten verwenden, die den Umbruch der Interpolation in einer beliebigen Richtung erzwingt. Die Anwendung kann auch angeben, dass die Interpolation als einfache lineare Interpolation durchgeführt wird.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturen](textures.md)

 

 




