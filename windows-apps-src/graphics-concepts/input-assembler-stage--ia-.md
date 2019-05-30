---
title: Eingabeassemblerphase (IA)
description: Die Eingabeassemblerphase (IA) liefert Grundtypen und angrenzende Daten an die Pipeline, wie beispielsweise Dreiecke, Linien und Punkte mit Semantik-IDs, damit die Shader effizienter arbeiten können, da die Verarbeitung von noch nicht verarbeiteten Grundtypen reduziert wird.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- Eingabeassemblerphase (IA)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 70957cbce10da25943b3c6347ccbbc81aafb5739
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370773"
---
# <a name="input-assembler-ia-stage"></a>Eingabeassemblerphase (IA)


Die Eingabeassemblerphase (IA) liefert Grundtypen und angrenzende Daten an die Pipeline, wie beispielsweise Dreiecke, Linien und Punkte mit Semantik-IDs, damit die Shader effizienter arbeiten können, da die Verarbeitung von noch nicht verarbeiteten Grundtypen reduziert wird.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Zweck und Verwendung


Die Eingabeassemblerphase (IA) dient dem Zweck, Grundtypendaten (Punkte, Linien und Dreiecke) aus den von Benutzer gefüllten Puffern zu lesen und die Daten zu Grundtypen zusammenzusetzen, die dann von den anderen Pipeline-Phasen verwendet werden, und [systemgenerierte Werte](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) anzuhängen, damit die Shader effizienter arbeiten können. Bei systemgenerierten Werten handelt es sich um Textzeichenfolgen, die auch als Semantiken bezeichnet werden. Die programmierbaren Shader-Phasen werden aus einem herkömmlichen Shader-Kern konstruiert, der systemgenerierte Werte (wie Grundtyp-ID, Instanz-ID oder eine Scheitelpunkt-ID) verwendet, so dass die Shader-Phase die Verarbeitung auf diese Grundtypen, Instanzen und Scheitelpunkte reduzieren kann, die noch nicht verarbeitet wurden.

Eine IA-Phase kann Scheitelpunkte zu mehreren verschiedenen [Grundtypen](primitive-topologies.md) (wie Linienlisten, Dreieckstreifen oder Grundtypen mit angrenzenden Daten) zusammenstellen. Grundtypen wie Dreiecklisten mit angrenzenden Daten und eine Linienliste mit angrenzenden Daten unterstützten die [Geometrieshaderphase (GS)](geometry-shader-stage--gs-.md).

Angrenzende Informationen sind für die Anwendung nur in einem Geometrieshader sichtbar. Wenn beispielsweise ein Geometrieshader mit einem Dreieck mit angrenzenden Daten aufgerufen wurde, enthalten die Eingabedaten drei Scheitelpunkte für jedes Dreieck und drei Scheitelpunkte für angrenzende Daten pro Dreieck.

Wird die IA-Phase aufgefordert, die angrenzenden Daten auszugeben, müssen die Eingabedaten angrenzende Daten enthalten. Dazu muss möglicherweise ein Dummy-Scheitelpunkt bereitgestellt werden (der ein fehlerhaftes Dreieck darstellt) oder vielleicht durch Kennzeichnung eines der Scheitelpunktattribute, ob der Scheitelpunkt vorhanden ist oder nicht. Die müsste dann auch von einem Geometrieshader erkannt und bearbeitet werden, obwohl das Culling von fehlerhafter Geometrie in der Rasterizerphase stattfindet.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


Die IA-Phase liest Daten aus dem Speicher: Grundtypendaten (Punkte, Linien und/oder Dreiecke) aus vom Benutzer gefüllten Puffern.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


Die IA-Phase stellt die Daten zu Grundtypen zusammen und hängt systemgenerierte Werte an, und gibt diese als Grundtypen aus, die in der [Vertexshaderphase (VS)](vertex-shader-stage--vs-.md) und danach in anderen Pipelinephasen verwendet werden.

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
<td align="left"><p><a href="primitive-topologies.md">Primitive Topologien</a></p></td>
<td align="left"><p>Direct3D unterstützt mehrere primitive Topologien, die definieren, wie Scheitelpunkte, z. B. Punktlisten, Linienlisten und Dreieckstreifen von der Pipeline interpretiert und gerendert werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">Verwenden von vom System generierte Werte</a></p></td>
<td align="left"><p>Systemgenerierte Werte werden von der Eingabeassemblerphase (IA) generiert (basierend auf vom Benutzer bereitgestellten Eingaben <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics">Semantiken</a>), um bestimmte Effizienzvorteile in Shaderabläufen zu ermöglichen. Durch das Anhängen von Daten, wie beispielsweise der Instanz-ID (sichtbar für die <a href="vertex-shader-stage--vs-.md">Vertexshaderphase (VS)</a>), einer Scheitelpunkt-ID (sichtbar für VS) oder einer Grundtypen-ID (sichtbar für die <a href="geometry-shader-stage--gs-.md">Geometrieshaderphase (GS)</a>/<a href="pixel-shader-stage--ps-.md">Pixelshaderphase (PS)</a>), kann eine nachfolgende Shaderphase nach diesen Systemwerten suchen, um die Verarbeitung in dieser Phase zu verbessern.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




