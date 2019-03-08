---
title: Texturfilterung
description: Die Texturfilterung erzeugt eine Farbe für jedes Pixel im 2D-gerenderten Bild des Grundtyps, wenn dieser durch die Abbildung eines 3D-Grundtyps auf einem 2D-Bildschirm gerendert wird.
ms.assetid: 1CCF4138-5D48-4B07-9490-996844F994D8
keywords:
- Texturfilterung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 449a31d92235efc50119bcd0db11b3532f523cd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633725"
---
# <a name="texture-filtering"></a>Texturfilterung


Die Texturfilterung erzeugt eine Farbe für jedes Pixel im 2D-gerenderten Bild des Grundtyps, wenn dieser durch die Abbildung eines 3D-Grundtyps auf einem 2D-Bildschirm gerendert wird.

Wenn Direct3D einen Grundtyp rendert, bildet es einen 3D-Grundtyp auf einem 2D-Bildschirm ab. Wenn Grundtyp eine Textur hat, muss Direct3D diese Textur verwenden, um eine Farbe für jedes Pixel im gerenderten 2D-Bild des Grundtyps zu erzeugen. Für jeden Pixel im Bildschirmbild des Grundtyps muss ein Farbwert aus der Textur abgerufen werden. Dieser Prozess wird als *Texturfilterung* bezeichnet.

Wenn ein Texturfilterungsvorgang durchgeführt wird, wird die verwendete Textur typischerweise auch vergrößert oder verkleinert. Anders ausgedrückt: Sie wird einem Grundtypenbild zugeordnet, das kleiner oder größer ist als sie selbst. Die Vergrößerung einer Textur kann dazu führen, dass viele Pixel einem Texel zugeordnet werden. Das Ergebnis kann dann eine ungleichmäßige Darstellung sein. Die Verkleinerung einer Textur bedeutet häufig, dass ein Pixel mehreren Texeln zugeordnet wird. Das resultierende Bild kann dann verschwommen aussehen. Um diese Probleme zu beheben, muss eine gewisse Mischung der Texelfarben durchgeführt werden, um zu einer Farbe für das Pixel zu kommen.

Direct3D vereinfacht den komplexen Vorgang der Texturfilterung. Es bietet Ihnen drei Arten der Texturfilterung: lineare Filterung, anisotropische Filterung und Mipmap-Filterung. Wenn Sie sich für keine Texturfilterung entscheiden, verwendet Direct3D die als „Sampling am nächstgelegenen Punkt“ bezeichnete Technik.

Jede Art der Texturfilterung hat Vor- und Nachteile. So kann die lineare Texturfilterung etwa zu gezackten Rändern oder einem ungleichmäßigen Erscheinungsbild des fertigen Biĺdes führen. Andererseits ist dies eine Texturfilterungsmethode, die nur geringe Rechenleistung erfordert. Die Filterung mit Mipmaps liefert gewöhnlich die besten Ergebnisse, besonders wenn dazu noch die anisotropische Filterung verwendet wird. Dies ist jedoch von allen Techniken, die Direct3D unterstützt, die mit dem größten Speicherbedarf.

## <a name="span-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspantypes-of-texture-filtering"></a><span id="Types-of-texture-filtering"></span><span id="types-of-texture-filtering"></span><span id="TYPES-OF-TEXTURE-FILTERING"></span>Typen von texturfilterung


Direct3D unterstützt die folgenden Verfahren für die Texturfilterung.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="nearest-point-sampling.md">Nächste Point-sampling</a></p></td>
<td align="left"><p>Apps müssen nicht die Texturfilterung verwenden. Direct3D kann so eingerichtet werden, dass es die Texel-Adresse berechnet, die häufig nicht auf ganze Zahlen geschätzt wird, und die Farbe des Texels mit der nächstgelegenen ganzzahligen Adresse kopiert. Dieser Prozess wird bezeichnet als <em>Sampling am nächstgelegenen Punkt</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="bilinear-texture-filtering.md">Bilineare texturfilterung</a></p></td>
<td align="left"><p>Die <em>bilineare Filterung</em> berechnet den gewichteten Durchschnitt der 4 Texel, die dem Sampling-Punkt am nächsten liegen. Diese Filtermethode ist präziser und gängiger als das Filtern am nächstgelegenen Punkt. Dieser Ansatz ist effizient, da er in moderner Grafikhardware implementiert wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="anisotropic-texture-filtering.md">Anisotrope texturfilterung</a></p></td>
<td align="left"><p><em>Anisotropie</em> ist die sichtbare Verzerrung bei Texeln eines 3D-Objekts, dessen Oberfläche gegenüber der Bildschirmebene in einem Winkel ausgerichtet ist. Wenn der Pixel eines anisotropischen Grundtyps einem Texel zugeordnet ist, wird die Form verzerrt.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering-with-mipmaps.md">Textur mit Mipmaps filtern</a></p></td>
<td align="left"><p>Ein <em>Mipmap</em> ist eine Sequenz von Texturen, von denen jede eine Darstellung desselben Bildes mit schrittweise niedrigerer Auflösung ist. Höhe und Breite der einzelnen Bilder bzw. Ebenen des Mipmaps sind jeweils um eine Zweierpotenz geringer als die der vorherigen Ebene.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturen](textures.md)

 

 




