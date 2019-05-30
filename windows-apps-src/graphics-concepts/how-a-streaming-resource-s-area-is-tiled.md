---
title: So unterteilen Sie den Bereich einer Streamingressource
description: Wenn Sie eine Streamingressource erstellen, bestimmen Dimensionen, Größe des Formats und Anzahl der Mipmaps und/oder Array-Segmente (falls zutreffend) die Anzahl der erforderlichen Kacheln, um die gesamte Oberfläche zu sichern.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- So unterteilen Sie den Bereich einer Streamingressource
- Ressourcenbereich, strukturiert
- Größentabellen, Ressourcen, strukturiert
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28fac411ad814167dcef3b02424c866bd3453344
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370595"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>So unterteilen Sie den Bereich einer Streamingressource


Wenn Sie eine Streamingressource erstellen, bestimmen Dimensionen, Größe des Formats und Anzahl der Mipmaps und/oder Array-Segmente (falls zutreffend) die Anzahl der erforderlichen Kacheln, um die gesamte Oberfläche zu sichern. Das Pixel/Byte-Layout in den Tiles wird durch die Implementierung bestimmt. Die Anzahl von Pixeln, die in eine Tile passen, ist je nach Formatelementgröße konstant und identisch, ob Sie einen standardmäßigen Swizzle verwenden oder nicht.

Die Anzahl von Tiles, die bei einer bestimmten Oberflächengröße und Formatelementbreite verwendet wird, ist klar definiert und kann den Tabellen in den folgenden Abschnitten entnommen werden. Für Ressourcen, die Mipmaps enthalten, oder für Fälle, in denen eine Tile nicht von der Oberfläche ausgefüllt wird, gelten einige Einschränkungen. Informationen dazu finden Sie unter [Mipmap-Verpackung](mipmap-packing.md).

Verschiedene Streamingressourcen dürfen auf identische Speicher mit verschiedenen Formaten zeigen, solange sich Anwendungen nicht auf die Ergebnisse verlassen, wenn in einen Speicher mit einem Format geschrieben und mit einem anderen gelesen wird Anwendungen können sich jedoch auf die Ergebnisse verlassen, wenn in einen Speicher mit einem Format geschrieben und mit einem anderen gelesen wird, sofern die Formate zu derselben Formatfamilie gehören (also dasselbe typlose übergeordnete Format besitzen). Z. B. DXGI\_FORMAT\_R8G8B8A8\_UNORM und DXGI\_FORMAT\_R8G8B8A8\_UINT sind kompatibel mit anderen jedoch nicht mit der DXGI\_FORMAT\_R16G16\_UNORM.

Eine Ausnahme sind definierte Übergänge von Daten von einem Format in ein anderes: Wenn die Bits einer Tile alle den Wert 0 haben, kann die Tile mit jedem Format verwendet werden, das diese Speicherinhalte als 0 interpretiert (unabhängig vom Speicherlayout). Daher kann eine Kachel gelöscht werden, 0 x 00, mit der DXGI-Format\_FORMAT\_R8\_UNORM und klicken Sie dann mit einem Format wie z. B. DXGI verwendet\_FORMAT\_R32G32\_"float", und es würde der Inhalt angezeigt trotzdem (0, 0F, 0, 0F).

Das Layout der Daten in einer Tile ist nicht davon abhängig, wo sie in eine Ressource zugeordnet ist. Daher kann eine Tile beispielsweise gleichzeitig und mit einheitlichem Verhalten an verschiedenen Positionen einer Oberfläche verwendet werden.

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
<td align="left"><p><a href="texture2d-and-texture2darray-subresource-tiling.md">Texture2D und Texture2DArray Unterteilung von unterressourcen</a></p></td>
<td align="left"><p>Diese Tabellen zeigen die Unterteilung von <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d"><strong>Texture2D</strong></a>- und <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray"><strong>Texture2DArray</strong></a>-Unterressourcen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Texture3D Unterteilung von unterressourcen</a></p></td>
<td align="left"><p>Diese Tabelle zeigt die Unterteilung von <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d"><strong>Texture3D</strong></a>-Unterressourcen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">Puffer tiling</a></p></td>
<td align="left"><p>Eine <a href="introduction-to-buffers.md">Puffer</a>-Ressource ist in 64 KB große Tiles unterteilt, mit Leerraum in der letzten Tile, wenn die Größe kein Vielfaches von 64 KB ist.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">MipMap-Komprimierung</a></p></td>
<td align="left"><p>Eine gewisse Anzahl an Mips (pro Array-Segment) kann in einer gewissen Anzahl von Kacheln verpackt sein, in Abhängigkeit von den Dimensionen der Streaming-Ressource, dem Format, der Anzahl der Mip-Maps und Array-Segmente.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Erstellen von streaming-Ressourcen](creating-streaming-resources.md)

 

 




