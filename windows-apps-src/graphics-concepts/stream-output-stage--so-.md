---
title: Streamausgabephase (SO)
description: Die Streamausgabephase (Stream Output, SO) gibt (oder streamt) kontinuierlich Vertexdaten aus der vorherigen aktiven Phase an einen oder mehrere Puffer im Speicher aus. In den Arbeitsspeicher gestreamte Daten können als Eingabedaten der Pipeline wieder zugeführt oder von der CPU eingelesen werden.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- Streamausgabephase (SO)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 87eb6562c6ee66ca1d409d3748e688861d5f3920
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605045"
---
# <a name="stream-output-so-stage"></a>Streamausgabephase (SO)


Die Streamausgabephase (Stream Output, SO) gibt (oder streamt) kontinuierlich Vertexdaten aus der vorherigen aktiven Phase an einen oder mehrere Puffer im Speicher aus. In den Arbeitsspeicher gestreamte Daten können als Eingabedaten der Pipeline wieder zugeführt oder von der CPU eingelesen werden.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


![Diagramm der Position der Streamausgabephase in der Pipeline](images/d3d10-pipeline-stages-so.png)

Die Streamausgabephase streamt Grundtypdaten auf dem Weg zum Rasterizer aus der Pipeline in den Arbeitsspeicher. Daten aus der vorherigen Phase können in den Arbeitsspeicher gestreamt und/oder in den Rasterizer übergeben werden. In den Arbeitsspeicher gestreamte Daten können als Eingabedaten der Pipeline wieder zugeführt oder von der CPU eingelesen werden.

In den Arbeitsspeicher gestreamte Daten können in einem nachfolgenden Renderingdurchlauf zurück in die Pipeline gelesen oder in eine Stagingressource kopiert werden (damit sie von der CPU gelesen werden können). Die Menge der gestreamten Daten kann variieren. Direct3D behandelt die Daten, ohne dass die GPU abgefragt werden muss, welche Menge an Daten geschrieben wurde.&gt;

Es gibt zwei Möglichkeiten, die Streamausgabedaten in die Pipeline zu stellen:

-   Streamausgabedaten können der Eingabeassemblerphase (IA) wieder zugeführt werden.
-   Streamausgabedaten können von programmierbaren Shadern mit [Load](https://msdn.microsoft.com/library/windows/desktop/bb509694)-Funktionen gelesen werden.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


Vertexdaten aus einer vorherigen Shaderphase.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


Die Streamoutputstufe (SO-Stufe) liefert (streamt) kontinuierlich Vertexdaten aus der vorherigen aktiven Stufe (z. B. der Geometry-Shader-Stufe (GS-Stufe) ) in einen oder mehrere Puffer im Speicher. Die Phase des Geometrie-Shader (GS) inaktiv ist, wird ausgegeben die Stream-Ausgabe (SO)-Phase kontinuierlich Vertexdaten, von der Stufe Domain-Shader (DS), um Puffer im Arbeitsspeicher (oder wenn DS auch inaktiv ist, von der Vertex-Shader (VS)-Stufe ist).

Wenn ein Dreiecks- oder Zeilenstrip an die Eingabeassemblerphase (IA) gebunden ist, wird jeder Strip in eine Liste konvertiert, bevor er gestreamt wird. Vertices werden immer als vollständige Grundtypen (z. B. gleichzeitig 3 Vertices für Dreiecke) ausgegeben. Unvollständige Grundtypen werden niemals gestreamt. Angrenzende Grundtypen verwerfen vor dem Streaming die angrenzenden Daten.

Die Streamausgabephase unterstützt gleichzeitig bis zu 4 Puffer.

-   Wenn Sie Daten in mehrere Puffer streamen, kann jeder Puffer nur ein einzelnes Element (bis zu 4 Komponenten) pro Vertexdaten erfassen, mit einem impliziten Datenversatz, der der Elementbreite im jedem Puffer entspricht (kompatibel mit der Art, wie einzelne Elementpuffer für die Eingabe in Shaderphasen gebunden werden können). Wenn Puffer unterschiedliche Größen haben, wird darüber hinaus der Schreibvorgang angehalten, sobald einer der Puffer voll ist.
-   Wenn Sie Daten in einen einzelnen Puffer streamen, kann der Puffer bis zu 64 skalare Komponenten pro Vertexdaten erfassen (256 Byte oder weniger) oder der Vertexversatz kann bis zu 2048 Byte betragen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




