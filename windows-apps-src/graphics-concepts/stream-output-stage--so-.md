---
title: Streamausgabephase (SO)
description: Die Datenstrom Ausgabe (so) gibt Vertex-Daten aus der vorherigen aktiven Phase kontinuierlich aus (oder streamt Sie an einen oder mehrere Speicherpuffer). Daten, die in den Arbeitsspeicher gestreamt werden, können wieder in die Pipeline als Eingabedaten oder aus der CPU zurückgeleitet werden.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- Streamausgabephase (SO)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f56036ecc083d72f552954860d04750c1c83b8b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156204"
---
# <a name="stream-output-so-stage"></a>Streamausgabephase (SO)


Die Datenstrom Ausgabe (so) gibt Vertex-Daten aus der vorherigen aktiven Phase kontinuierlich aus (oder streamt Sie an einen oder mehrere Speicherpuffer). Daten, die in den Arbeitsspeicher gestreamt werden, können wieder in die Pipeline als Eingabedaten oder aus der CPU zurückgeleitet werden.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


![Diagramm des Speicher Orts der Stream-Ausgabestufe in der Pipeline](images/d3d10-pipeline-stages-so.png)

Die Stream-Output-Phase streamt primitive Daten aus der Pipeline in den Arbeitsspeicher, sodass Sie an den Rasterizer weitergeben können. Daten aus der vorherigen Phase können in den Arbeitsspeicher gestreamt und/oder an den Rasterizer übermittelt werden. Daten, die in den Arbeitsspeicher gestreamt werden, können wieder in die Pipeline als Eingabedaten oder aus der CPU zurückgeleitet werden.

Daten, die in den Arbeitsspeicher gestreamt werden, können in einem nachfolgenden Renderingdurchlauf wieder in die Pipeline eingelesen werden oder in eine stagingressource kopiert werden (damit Sie von der CPU gelesen werden kann). Die Menge der Daten, die gestreamt werden, kann variieren. Direct3D ist so konzipiert, dass die Daten verarbeitet werden, ohne dass eine Abfrage (GPU) zur Menge der geschriebenen Daten erforderlich ist.&gt;

Es gibt zwei Möglichkeiten, Datenstrom Ausgabedaten in die Pipeline zu übermitteln:

-   Stream-Ausgabedaten können wieder in die Eingabe Assembler-Stufe (IA) eingegeben werden.
-   Stream-Ausgabedaten können von programmierbaren Shadern mithilfe von [Lade](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load) Funktionen gelesen werden.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Der


Vertex-Daten aus einer vorherigen Shader-Stufe.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgeben


Die "Stream Output (so)"-Phase gibt Vertex-Daten aus der vorherigen aktiven Phase, wie z. b. der Geometrie-Shader-Phase (GS), kontinuierlich aus. Wenn die Geometrie-Shader-Phase (GS) inaktiv ist, gibt die Datenstrom Ausgabe (so) kontinuierlich Vertex-Daten aus der Domänen-Shader-Stufe (DS) an Puffer im Speicher aus (oder, wenn DS auch inaktiv ist, von der Vertex-Shader-Phase (VS)).

Wenn ein Dreieck oder ein Zeilen Streifen an die Eingabe Assembler-Stufe (IA) gebunden ist, wird jeder Strip in eine Liste konvertiert, bevor diese gestreamt werden. Scheitel Punkte werden immer als umfassende primitive geschrieben (z. b. 3 Scheitel Punkte gleichzeitig für Dreiecke); unvollständige primitive werden niemals gestreamt. Primitive Typen mit der Konsistenz verwerfen vor dem Streamen von Daten die Daten der Daten.

Die Stream-Output-Stufe unterstützt bis zu 4 Puffer gleichzeitig.

-   Wenn Sie Daten in mehrere Puffer streamen, kann jeder Puffer nur ein einzelnes Element (bis zu 4 Komponenten) von pro-Vertex-Daten erfassen, wobei ein impliziter Daten Sprung gleich der Elementbreite in den einzelnen Puffern ist (kompatibel mit der Art und Weise, wie einzelne Element Puffer für Eingaben in Shader-Stufen gebunden werden können). Wenn die Puffer über unterschiedliche Größen verfügen, wird der Schreibvorgang beendet, sobald ein Puffer voll ist.
-   Wenn Sie Daten in einen einzelnen Puffer streamen, kann der Puffer bis zu 64 skalare Komponenten von pro-Vertex-Daten (256 Bytes oder weniger) erfassen, oder der Scheitelpunkt Schritt kann bis zu 2048 Byte betragen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 