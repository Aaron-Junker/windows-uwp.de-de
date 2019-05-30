---
title: Vertexshaderphase (VS)
description: Die Scheitelpunkt-Shader- (VS) Phase verarbeitet Scheitelpunkte und führt dabei in der Regel Vorgänge wie Transformationen, das Anwenden von Skins und Beleuchtung durch. Ein Vertex-Shader verarbeitet immer nur einen Eingabevertex und erzeugt daraus einen Ausgabevertex.
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- Vertexshaderphase (VS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5893719e43314eb15c684948a31de5a025a926fc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370829"
---
# <a name="vertex-shader-vs-stage"></a>Vertexshaderphase (VS)


Die Scheitelpunkt-Shader- (VS) Phase verarbeitet Scheitelpunkte und führt dabei in der Regel Vorgänge wie Transformationen, das Anwenden von Skins und Beleuchtung durch. Ein Vertex-Shader verarbeitet immer nur einen Eingabevertex und erzeugt daraus einen Ausgabevertex.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


Die Scheitelpunkt-Shader- (VS) Phase wird für einzelne Scheitelpunktverarbeitungen verwendet, wie etwa:

-   Transformationen
-   Anwenden von Skins
-   Morphing
-   Scheitelpunktbeleuchtung

Die Scheitelpunkt-Shader-Stufe ist eine programmierbare Shader-Stufe. Sie wird im [Grafikpipeline](graphics-pipeline.md)- Diagramm als abgerundeter Block angezeigt. Dieser Shader-Stufe verwendet Shadermodell 4.0 [gemeinsamer Shaderkern](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core).

Die Scheitelpunkt-Shader- (VS) Phase verarbeitet Scheitelpunkte aus dem Eingabe-Assembler. Ein Scheitelpunkt-Shader operiert immer auf einem einzigen Eingabescheitelpunkt und erzeugt daraus einen einzigen Ausgabescheitelpunkt. Die Scheitelpunkt-Shader-Phase muss immer für auszuführende Pipeline aktiv sein. Wenn keine Scheitelpunktänderung oder -transformation erforderlich ist, muss ein Pass-Through-Scheitelpunkt-Shader erstellt und auf die Pipeline festgelegt werden.

Jeder Eingabevertex für einen Vertex-Shader kann aus mehreren 32-Bit-Vektoren bestehen (maximal 16, mit jeweils bis zu 4-Komponenten). Jeder Ausgabevertex kann aus mehreren 32-Bit-Vektoren bestehen (maximal 16 mit jeweils 4 Komponenten). Alle Scheitelpunkt-Shader benötigen mindestens eine Eingabe und eine Ausgabe, die so klein wie ein Skalarwert sein können.

Die Vertex-Shader-Stufe kann zwei vom System generierte Werte aus der Eingabe-Assembler verwenden: VertexID und Instanz-ID (Siehe Systemwerte und Semantik). Da Scheitelpunkt-ID und Instanz-ID auf Scheitelpunktebene bedeutsam sind und von der Hardware generierte IDs nur in di erste Phase eingegeben werden können, die sie versteht, können diese ID-Werte nur in die Scheitelpunkt-Shader-Phase eingegeben werden.

Scheitelpunkt-Shader werden immer auf allen Scheitelpunkten ausgeführt, einschließlich benachbarter Scheitelpunkte in Eingabe-Grundtyp-Topologien mit Nachbarschaft. Die Ausführungshäufigkeit des Scheitelpunkt-Shaders kann von der CPU mithilfe der VSInvocations-Pipelinestatistik abgefragt werden.

Ein Vertex-Shader kann laden und Ausführen textursampling Vorgänge Bildschirmbereich ableitungen sind, in denen nicht erforderlich (Verwenden der systeminternen Funktionen "HLSL": [Beispiel (HLSL von DirectX-Textur-Objekt)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-sample), [SampleCmpLevelZero (HLSL von DirectX-Textur-Objekt)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero), und [SampleGrad (HLSL von DirectX-Textur-Objekt)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplegrad)).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


Ein einzelner Vertex mit systemgenerierten Werten wie VertexID und InstanceID. Jeder Eingabevertex für einen Vertex-Shader kann aus mehreren 32-Bit-Vektoren bestehen (maximal 16, mit jeweils bis zu 4-Komponenten).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


Ein einzelner Vertex. Jeder Ausgabevertex kann aus mehreren 32-Bit-Vektoren bestehen (maximal 16 mit jeweils 4 Komponenten).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




