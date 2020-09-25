---
title: Eingabe Assembler-Stufe (IA)
description: In der Eingabe Assembler-Stufe (IA) werden primitive und Daten in der Pipeline zur Verfügung gestellt, wie z. b. Dreiecke, Linien und Punkte, einschließlich Semantik-IDs, um Shader effizienter zu gestalten, indem die Verarbeitung auf primitive Elemente reduziert wird, die noch nicht verarbeitet wurden.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- Eingabe Assembler-Stufe (IA)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 12a7c7ebd250fec8d944c4cba467a92ff67bd33d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219793"
---
# <a name="input-assembler-ia-stage"></a>Eingabe Assembler-Stufe (IA)


In der Eingabe Assembler-Stufe (IA) werden primitive und Daten in der Pipeline zur Verfügung gestellt, wie z. b. Dreiecke, Linien und Punkte, einschließlich Semantik-IDs, um Shader effizienter zu gestalten, indem die Verarbeitung auf primitive Elemente reduziert wird, die noch nicht verarbeitet wurden.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Zweck und Verwendung


Der Zweck der Eingabe Assembler-Stufe (IA) ist das Lesen primitiver Daten (Punkte, Linien und Dreiecke) aus vom Benutzer gefüllten Puffern und Assemblieren der Daten in primitive, die von den anderen Pipeline Stufen verwendet werden, und das Anfügen von [System generierten Werten](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) , damit Shader effizienter werden. Vom System generierte Werte sind Text Zeichenfolgen, die auch als Semantik bezeichnet werden. Die programmierbaren shaderphasen werden aus einem gemeinsamen Shader-Kern erstellt, der vom systemgenerierte Werte (z. b. eine primitive ID, eine Instanz-ID oder eine Scheitelpunkt-ID) verwendet, damit die Shader-Stufe die Verarbeitung nur auf die primitiven, Instanzen oder Scheitel Punkte reduzieren kann, die noch nicht verarbeitet wurden.

In der IA-Phase können Vertices in mehreren unterschiedlichen [primitiven Typen](primitive-topologies.md) (z. b. Linien Listen, Dreiecks Streifen oder primitive) zusammengestellt werden. Primitive Typen, wie z. b. eine Dreiecks Liste, und eine Zeilen Liste mit der Konsistenz, unterstützen die [Geometrie-Shader-Stufe (GS)](geometry-shader-stage--gs-.md).

Informationen zu Informationen sind für eine Anwendung nur in einem Geometry-Shader sichtbar. Wenn ein Geometry-Shader mit einem Dreieck aufgerufen wurde (z. a. die Nähe), enthalten die Eingabedaten beispielsweise drei Scheitel Punkte für jedes Dreieck und drei Scheitel Punkte für Daten pro Dreieck.

Wenn die IA-Phase für die Ausgabe von Anforderungsdaten angefordert wird, müssen die Eingabedaten Daten enthalten. Hierfür ist möglicherweise ein dummyscheitel Punkt (aus einem degenerierten Dreieck) erforderlich, oder Sie können in einem der Scheitelpunkt Attribute kennzeichnen, ob der Scheitelpunkt vorhanden ist oder nicht. Dies müsste auch von einem Geometry-Shader erkannt und behandelt werden, obwohl die Erkennung degenerierter Geometrie in der Raster-Stufe stattfindet.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Der


Die IA-Phase liest Daten aus dem Arbeitsspeicher: primitive Daten (Punkte, Linien und/oder Dreiecke) aus vom Benutzer gefüllten Puffer.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgeben


In der IA-Phase werden die Daten in primitive assembliert und vom systemgenerierte Werte angefügt. Diese werden als primitive ausgegeben, die von der [Vertex-Shader-Phase (VS)](vertex-shader-stage--vs-.md) und anderen Pipeline Stufen verwendet werden.

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
<td align="left"><p><a href="primitive-topologies.md">Primitive Topologien</a></p></td>
<td align="left"><p>Direct3D unterstützt mehrere primitive Topologien, die definieren, wie Scheitel Punkte von der Pipeline interpretiert und gerendert werden, z. b. Punkt Listen, Linien Listen und Dreieck Streifen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">Verwenden von systemgenerierten Werten</a></p></td>
<td align="left"><p>Vom System generierte Werte werden von der Eingabe Assembler-Stufe (IA) generiert (basierend auf der vom Benutzer bereitgestellten Eingabe <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics">Semantik</a>), um bestimmte Effizienz bei shadervorgängen zu ermöglichen. Durch das Anfügen von Daten, z. b. eine Instanz-ID (sichtbar für die <a href="vertex-shader-stage--vs-.md">Vertex-Shader-Phase</a>), eine Scheitelpunkt-ID (sichtbar für vs) oder eine primitive ID (sichtbar für <a href="geometry-shader-stage--gs-.md">Geometrie-Shader (GS) Stage</a> / <a href="pixel-shader-stage--ps-.md">Pixel Shader (PS)</a>), kann eine nachfolgende Shader-Phase nach diesen System Werten suchen, um die Verarbeitung in</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 