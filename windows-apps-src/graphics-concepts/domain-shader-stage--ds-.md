---
title: Domainshaderphase (DS)
description: Die Domainshaderphase (DS) berechnet die Vertexposition eines unterteilten Punkts im Ausgabefeld. Sie Berechnet die Vertexposition, die dem jeweiligen Domainsample entspricht.
ms.assetid: 673CC04A-A74F-495F-AFB7-49157538749C
keywords:
- Domainshaderphase (DS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c38259c303673c3c3c5494470d721856fcaaf6c9
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291718"
---
# <a name="domain-shader-ds-stage"></a>Domainshaderphase (DS)

Die Domainshaderphase (DS) berechnet die Vertexposition eines unterteilten Punkts im Ausgabefeld. Sie Berechnet die Vertexposition, die dem jeweiligen Domainsample entspricht. Ein Domain-Shader wird einmal pro Ausgabepunkt der Tessellatorstufe ausgeführt und hat schreibgeschützten Zugriff auf die UV-Koordinaten der Ausgabe der Tessellatorstufe, den Ausgabepatch des Hull-Shaders und dessen Ausgabepatchkonstanten.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


Die Domainshaderphase (DS) gibt die Vertex-Position eines unterteilen Punkts im Ausgabe Patch aus (basierend auf der Eingabe der [Hull-Shader-Phase (HS)](hull-shader-stage--hs-.md) und der [Tessellator-Phase (TS)](tessellator-stage--ts-.md)).

![Diagramm der Domainshaderphase](images/d3d11-domain-shader.png)

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


-   Ein Domainshader nimmt die Ausgabe-Kontrollpunkte aus der [Hull-Shader-Phase (HS)](hull-shader-stage--hs-.md) entgegen. Die Hull-Shader-Ausgaben umfassen:
    -   Kontrollpunkte.
    -   Patch-Konstantendaten.
    -   Tessellation-Faktoren. Die Tesselation-Faktoren können die vom Fixed-Function-Tessellator verwendeten Werte und die Rohwerte (z. B. vor der Rundung durch die Integer-Tesselation) umfassen. Diese werden beispielsweise für das Geomorphing verwendet.
-   Ein Domainshader wird einmal pro Ausgabekoordinate über die [Tessellator-Phase (TS)](tessellator-stage--ts-.md) aufgerufen.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


-   Die Domain-Shader-Stufe (DS-Stufe) gibt die Vertexposition eines unterteilten Punkts im Ausgabepatch aus.

Nachdem der Domainshader abgeschlossen ist, wird die Tesselation abgeschlossen und die Pipelinedaten werden an die nächste Pipelinephase weitergeleitet (z. B. die [Geometry-Shader-Phase (GS)](geometry-shader-stage--gs-.md) und die [Pixel-Shader-Phase (PS)](pixel-shader-stage--ps-.md)). Ein Geometrieshader erwartet Primitiven mit strukturierten Daten (z. B. 6 Vertizes pro Dreieck). Er ist bei aktiver Tesselation nicht gültig (dies führt zu einem unerwartetem Verhalten, das einen Fehler der Debugschicht auslöst).

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


```hlsl
void main( out    MyDSOutput result, 
           float2 myInputUV : SV_DomainPoint, 
           MyDSInput DSInputs,
           OutputPatch<MyOutPoint, 12> ControlPts, 
           MyTessFactors tessFactors)
{
     ...

     result.Position = EvaluateSurfaceUV(ControlPoints, myInputUV);
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




