---
title: Komprimierte Texturressourcen
description: Texturabbildungen sind digitale Bilder, die auf dreidimensionale Formen gezeichnete werden, um diesen mehr Details zu verleihen.
ms.assetid: 2DD5FF94-A029-4694-B103-26946C8DFBC1
keywords:
- Komprimierte Texturressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9318479fffc415e94407166bd1be20a93691a179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646215"
---
# <a name="compressed-texture-resources"></a>Komprimierte Texturressourcen


Texturabbildungen sind digitale Bilder, die auf dreidimensionale Formen gezeichnete werden, um diesen mehr Details zu verleihen. Sie werden während der Rasterung in diesen Formen wiedergegeben. Der Prozess kann große Mengen des Systembuses und des Speichers verbrauchen. Um den von den Texturen verbrauchten Speicherplatz zu reduzieren, unterstützt Direct3D die Komprimierung von Texturoberflächen. Einige Direct3D-Geräte bieten eine systemeigene Unterstützung für komprimierte Texturoberflächen. Wenn Sie auf solchen Geräten eine komprimierte Oberfläche erstellt und die Daten geladen haben, kann die Oberfläche in Direct3D wie jede andere Texturoberfläche verwendet werden. Direct3D verarbeitet die Dekomprimierung, wenn die Textur einem 3D-Objekt zugeordnet wird.

## <a name="span-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanstorage-efficiency-and-texture-compression"></a><span id="Storage-Efficiency-and-Texture-Compression"></span><span id="storage-efficiency-and-texture-compression"></span><span id="STORAGE-EFFICIENCY-AND-TEXTURE-COMPRESSION"></span>Speicherkomprimierung Effizienz und Textur


Alle Formate für die Texturkomprimierung sind Potenzen von 2. Obwohl dies nicht bedeutet, dass eine Textur unbedingt quadratisch ist, bedeutet es, dass es sich bei x und y um eine Potenz von 2 handelt. Wenn eine Textur z. B. ursprünglich 512 x 128 Byte ist, wäre das nächsten Mipmapping 256 x 64 usw., wobei jede Ebene mit einer Potenz von 2 verkleinert wird. Auf niedrigeren Ebenen, wenn die Textur auf 16 x 2 und 8 x 1 gefiltert wird, gibt es „verschwendete” Bits, da der Komprimierungsblock immer ein 4 x 4-Texelblock ist. Nicht genutzte Teile des Blocks werden aufgefüllt.

Obwohl auf der untersten Ebene verschwendete Bits existieren, ist der Gesamtgewinn erheblich. Der schlimmste Fall ist theoretisch eine 2000 x 1 Textur (2⁰ Potenz). Dabei wird nur eine einzelne Zeile der Pixel pro Block codiert, während der Rest des Blocks nicht verwendet wird.

## <a name="span-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanmixing-formats-within-a-single-texture"></a><span id="Mixing-Formats-Within-a-Single-Texture"></span><span id="mixing-formats-within-a-single-texture"></span><span id="MIXING-FORMATS-WITHIN-A-SINGLE-TEXTURE"></span>Mischen von Formaten in einer einzelnen Struktur


Es muss beachtet werden, dass jede einzelne Textur angeben muss, ob ihre Daten als 64 oder 128 Bit pro Gruppe von 16 Texeln gespeichert werden. Wenn 64-Bit-Blöcke - im BC1-Blockkomprimierungformat – für die Textur verwendet werden, ist es möglich, undurchsichtige und 1-Bit-Alpha-Kanal-Formate auf einer pro-Block-Basis innerhalb der gleichen Textur zu kombinieren. Das heißt, der Vergleich Ganzzahl ohne Vorzeichen als Maßeinheit Farbe\_0 und die Farbe\_1 eindeutig für jeden Block von 16 Texel durchgeführt wird.

Wenn 128-Bit-Blöcke verwendet werden, muss der Alphakanal entweder im expliziten ( BC2-Format) oder interpolierten Modus (BC3-Format) für die gesamte Textur angegeben werden. Was die Farbe anbetrifft, können beim aktiven interpolierten Modus (BC3-Modus) entweder acht interpolierte Alphamodi oder sechs interpolierte Alphamodi auf einer nach Blöcken gestaffelten Basis verwendet werden. Erneut der Größe Vergleich Alpha\_0 und Alpha\_1 erfolgt eindeutig auf Basis von Blöcken.

Direct3D bietet Services zum Komprimieren von Oberflächen, die für das Anwenden von Texturen bei 3D-Modellen verwendet werden. Dieser Abschnitt enthält Informationen zum Erstellen und Bearbeiten der Daten in einer komprimierten Texturoberfläche.

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
<td align="left"><p><a href="opaque-and-1-bit-alpha-textures.md">Nicht transparent und 1-Bit-alphatexturen</a></p></td>
<td align="left"><p>Das Texturformat BC1 ist für Texturen, die undurchsichtig sind oder eine transparente Farbe haben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures-with-alpha-channels.md">Texturen mit alpha-Kanäle</a></p></td>
<td align="left"><p>Es gibt zwei Möglichkeiten, Texturabbildungen mit komplexer Transparenz zu codieren. In jedem Fall geht ein Block, der die Transparenz beschreibt, dem bereits beschriebenen 64-Bit-Block voraus. Die Transparenz wird entweder als 4 x 4-Bitmap mit 4 Bits pro Pixel (explizite Codierung) oder mit weniger Bits und linearer Interpolation dargestellt, die mit der verwendeten Codierung von Farben vergleichbar ist.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="block-compression.md">Blockkomprimierung</a></p></td>
<td align="left"><p>Blockkomprimierung ist ein verlustbehaftetes Texturkomprimierungsverfahren zum Reduzieren der Texturgröße und des Speicherbedarfs und zur Steigerung der Leistung. Eine blockkomprimierte Textur kann kleiner als eine Textur mit 32 Bit pro Farbe sein.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="compressed-texture-formats.md">Komprimierten texturformate</a></p></td>
<td align="left"><p>Dieser Abschnitt enthält Informationen über die interne Organisation komprimierter Texturformate. Diese Informationen sind nicht zur Verwendung von komprimierten Texturen notwendig, da Sie die Direct3D-Funktionen für eine Konvertierung in und von komprimierten Formate verwenden können. Sie sind jedoch hilfreich, wenn Sie direkt auf komprimierten Oberflächendaten arbeiten möchten.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturen](textures.md)

 

 




