---
title: Vertexshaderphase (VS)
description: Die Vertex-Shader (VS)-Phase verarbeitet Scheitel Punkte, die in der Regel Vorgänge wie Transformationen, das Skinning und die Beleuchtung ausführen. Ein Vertex-Shader nimmt einen einzelnen Eingabe Scheitelpunkt und erzeugt einen einzelnen Ausgabe Scheitelpunkt.
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- Vertexshaderphase (VS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88a990470d0ff8aea5c6415584bd63b2b4b65981
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156054"
---
# <a name="vertex-shader-vs-stage"></a>Vertexshaderphase (VS)


Die Vertex-Shader (VS)-Phase verarbeitet Scheitel Punkte, die in der Regel Vorgänge wie Transformationen, das Skinning und die Beleuchtung ausführen. Ein Vertex-Shader nimmt einen einzelnen Eingabe Scheitelpunkt und erzeugt einen einzelnen Ausgabe Scheitelpunkt.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


Die Vertex-Shader-Phase (VS) wird für einzelne Verarbeitung pro Scheitelpunkt verwendet, z. b.:

-   Transformationen
-   Skinning
-   Morphing
-   Pro-Vertex-Beleuchtung

Die Vertex-Shader-Stufe ist eine programmierbare Shader-Stufe. Sie wird im [Grafik Pipeline](graphics-pipeline.md) Diagramm als abgerundeter Block angezeigt. Diese Shader-Phase verwendet Shader Model 4,0 [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core).

Die Vertex-Shader (VS)-Phase verarbeitet Vertices aus dem Eingabe Assembler. Vertex-Shader arbeiten immer auf einem einzelnen Eingabe Scheitelpunkt und liefern einen einzelnen Ausgabe Scheitelpunkt. Die Vertex-Shader-Stufe muss immer aktiv sein, damit die Pipeline ausgeführt werden muss. Wenn keine Scheitelpunkt Änderung oder-Transformation erforderlich ist, muss ein Pass-Through-Vertex-Shader erstellt und auf die Pipeline festgelegt werden.

Jeder Vertex-Shader-Eingabe Scheitelpunkt kann aus bis zu 16 32-Bit-Vektoren bestehen (jeweils bis zu 4 Komponenten). Jeder Ausgabe Scheitelpunkt kann aus bis zu 16 32-Bit-vier-Komponenten-Vektoren bestehen. Alle Vertex-Shader müssen mindestens eine Eingabe und eine Ausgabe aufweisen, die nur einen skalaren Wert aufweisen kann.

Die Vertex-Shader-Phase kann zwei vom systemgenerierte Werte aus dem Eingabe Assembler verarbeiten: vertexid und InstanceId (siehe System Werte und Semantik). Da vertexid und InstanceId auf einer Scheitelpunkt Ebene aussagekräftig sind und von der Hardware generierte IDs nur in die erste Phase eingespeist werden können, die Sie versteht, können diese ID-Werte nur in die Vertex-Shader-Phase eingespeist werden.

Vertex-Shader werden immer auf allen Scheitel Punkten ausgeführt, einschließlich angrenzender Scheitel Punkte in eingegebenen primitiven Topologien. Die Häufigkeit, mit der der Vertex-Shader ausgeführt wurde, kann mithilfe der Pipeline Statistik der vsinvocations von der CPU abgefragt werden.

Ein Vertex-Shader kann Lade-und Textur-samplingvorgänge ausführen, bei denen keine Bildschirmspeicher Ableitungen erforderlich sind (mit systeminternen HLSL-Funktionen: [Sample (DirectX HLSL Texture Object)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-sample), [samplecmplevelzero (DirectX HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero)Texture Object) und [samplegrad (DirectX HLSL Texture Object)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplegrad)).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Der


Ein einzelner Scheitelpunkt mit vom System generierten Werten "vertexid" und "InstanceId". Jeder Vertex-Shader-Eingabe Scheitelpunkt kann aus bis zu 16 32-Bit-Vektoren bestehen (jeweils bis zu 4 Komponenten).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgeben


Ein einzelner Scheitelpunkt. Jeder Ausgabe Scheitelpunkt kann aus bis zu 16 32-Bit-vier-Komponenten-Vektoren bestehen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 