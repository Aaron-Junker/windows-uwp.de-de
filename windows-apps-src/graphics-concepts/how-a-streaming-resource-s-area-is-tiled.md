---
title: So unterteilen Sie den Bereich einer Streamingressource
description: Wenn Sie eine streamingressource erstellen, bestimmen die Dimensionen, die Format Elementgröße und die Anzahl von Mipmaps-und/oder Array Slices (falls zutreffend) die Anzahl der Kacheln, die für die gesamte Oberfläche benötigt werden.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- So unterteilen Sie den Bereich einer Streamingressource
- Ressourcenbereich, gekachelt
- Größen Tabellen, Ressourcen, Kacheln
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fae56f673aa3d952b7e85490ec79676c2ac6a48a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220343"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>So unterteilen Sie den Bereich einer Streamingressource


Wenn Sie eine streamingressource erstellen, bestimmen die Dimensionen, die Format Elementgröße und die Anzahl von Mipmaps-und/oder Array Slices (falls zutreffend) die Anzahl der Kacheln, die für die gesamte Oberfläche benötigt werden. Das Pixel-/bytelayout in Kacheln wird von der-Implementierung bestimmt. Die Anzahl der Pixel, die je nach Format Elementgröße in eine Kachel passen, ist fest und identisch, unabhängig davon, ob Sie einen Standardwert verwenden oder nicht.

Die Anzahl der Kacheln, die von einer bestimmten Oberflächen Größe und Format Elementbreite verwendet werden, ist basierend auf den Tabellen in den folgenden Abschnitten gut definiert und vorhersagbar. Für Ressourcen, die Mipmaps oder Fälle enthalten, in denen keine Kachel durch Oberflächen Dimensionen aufgefüllt wird, gibt es einige Einschränkungen. Weitere Informationen finden Sie unter [MipMap Packaging](mipmap-packing.md).

Unterschiedliche Streamingressourcen können auf identischen Arbeitsspeicher mit unterschiedlichen Formaten zeigen, wenn Anwendungen nicht von den Ergebnissen des Schreibvorgangs in den Arbeitsspeicher mit einem Format und dem Lesen mit einem anderen Format abhängig sind. Anwendungen können sich jedoch auf die Ergebnisse des Schreibvorgangs in den Arbeitsspeicher mit einem Format und das Lesen mit einem anderen-Objekt verlassen, wenn sich die Formate in derselben Format Familie befinden (d. h., Sie haben dasselbe typlose übergeordnete Format). Beispielsweise sind DXGI \_ \_ -Format R8G8B8A8 \_ unorm und DXGI- \_ Format \_ R8G8B8A8 \_ uint kompatibel, aber nicht mit dem DXGI- \_ Format \_ R16G16 \_ unorm.

Eine Ausnahme besteht darin, dass das Löschen von Daten von einem Format in ein anderes klar definiert ist: Wenn eine Kachel vollständig 0 für alle Bits enthält, kann diese Kachel in jedem Format verwendet werden, das diese Speicherinhalte als 0 (unabhängig vom Speicher Layout) interpretiert. Daher kann eine Kachel mit dem Format DXGI- \_ Format \_ R8 unorm auf 0x00 \_ und dann mit einem Format wie DXGI- \_ Format R32G32 float gelöscht werden, und es wird angezeigt, dass \_ \_ der Inhalt weiterhin (0,0 f, 0,0 f) ist.

Das Layout der Daten in einer Kachel hängt nicht davon ab, wo die Kachel in einer Ressource insgesamt zugeordnet ist. So kann z. b. eine Kachel an verschiedenen Positionen einer-Oberfläche gleichzeitig wieder verwendet werden, wobei an allen Standorten ein konsistentes Verhalten verwendet wird.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="texture2d-and-texture2darray-subresource-tiling.md">Unterteilung von Texture2D- und Texture2DArray-Unterressourcen</a></p></td>
<td align="left"><p>Diese Tabellen zeigen, wie <a href="/windows/desktop/direct3dhlsl/sm5-object-texture2d"><strong>Texture2D</strong></a> und <a href="/windows/desktop/direct3dhlsl/sm5-object-texture2darray"><strong>Texture2DArray</strong></a> -unter Ressourcen nebeneinander angeordnet werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Unterteilung von Texture3D-Unterressourcen</a></p></td>
<td align="left"><p>Diese Tabelle zeigt, wie <a href="/windows/desktop/direct3dhlsl/sm5-object-texture3d"><strong>Texture3D</strong></a> -unter Ressourcen nebeneinander angeordnet werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">Pufferanordnung</a></p></td>
<td align="left"><p>Eine <a href="introduction-to-buffers.md">Puffer</a> Ressource ist in Kacheln mit 64 KB unterteilt, wobei ein Leerzeichen in der letzten Kachel leer ist, wenn die Größe kein Vielfaches von 64 KB ist.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">MipMap-Verpackung</a></p></td>
<td align="left"><p>Eine bestimmte Anzahl von MIPS (pro Array Slice) kann in einer Reihe von Kacheln verpackt werden, abhängig von den Dimensionen, dem Format, der Anzahl von Mipmaps und den Array Slices einer streamingressource.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Erstellen von Streamingressourcen](creating-streaming-resources.md)

 

 