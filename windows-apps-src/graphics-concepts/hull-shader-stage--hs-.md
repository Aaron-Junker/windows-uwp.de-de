---
title: Hullshaderphase (HS)
description: Die Hull-Shader-Stufe ist eine der Tesselationsstufen, auf der eine durchgehende Fläche eines Modell effizient in vielen Dreiecke unterteilt wird.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- Hullshaderphase (HS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dd45408c484534009bf632eb81be11e6ef4d58b7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370580"
---
# <a name="hull-shader-hs-stage"></a>Hullshaderphase (HS)

Die Hull-Shader-Stufe ist eine der Tesselationsstufen, auf der eine durchgehende Fläche eines Modell effizient in vielen Dreiecke unterteilt wird. Die Hull-Shader-Stufe (HS-Stufe) erzeugt einen Geometriepatch (und Patchkonstanten), der jedem Eingabepatch (Quadrat, Dreieck oder Linie) entspricht. Ein Hull-Shader wird einmal pro Patch aufgerufen. Er transformiert Eingabekontrollpunkte, die eine Oberfläche niederer Ordnung definieren, in Kontrollpunkte, die einen Patch bilden. Er führt außerdem pro Patch einige Berechnungen aus, um der [Tessellatorstufe (TS-Stufe)](tessellator-stage--ts-.md) und der [Domain-Shader-Stufe (DS-Stufe)](domain-shader-stage--ds-.md) Daten bereitzustellen.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


![Diagramm der Hull-Shader-Stufe](images/d3d11-hull-shader.png)

Die drei Tesselationsstufen arbeiten zusammen, um Oberflächen höherer Ordnung (die das Modell einfach und effizient halten) in viele Dreiecke umzuwandeln, die in der Grafikpipeline detailliert gerendert werden. Die Tesselationsstufen sind die Hull-Shader-Stufe (HS-Stufe), die [Tessellatorstufe (TS-Stufe)](tessellator-stage--ts-.md) und die [Domain-Shader-Stufe (DS-Stufe)](domain-shader-stage--ds-.md).

Die Hull-Shader-Stufe (HS-Stufe) ist eine programmierbare Shaderstufe. Ein Hull-Shader wird mit einer HLSL-Funktion implementiert.

Ein Hull-Shader arbeitet in zwei Stufen – einer Kontrollpunktstufe und einer Patchkonstantenstufe, die von der Hardware parallel ausgeführt werden. Der HLSL-Compiler extrahiert die parallelen Hull-Shader-Ergebnisse und wandelt sie in Bytecode um, der von der Hardware verarbeitet wird.

-   Die Kontrollpunktstufe liest jeden Kontrollpunkt für einen Patch und generiert daraus einen Ausgabekontrollpunkt (gekennzeichnet durch eine **ControlPointID**).
-   Die Patchkonstantenstufe generiert pro Patch Randtesselationsfaktoren und andere Patchkonstanten. Intern können viele Patchkonstantenstufen gleichzeitig ausgeführt werden. Die Patchkonstantenstufe verfügt über schreibgeschützten Zugriff auf alle Eingabe- und Ausgabekontrollpunkte.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


1 bis 32 Eingabekontrollpunkte, die zusammen eine Fläche niederer Ordnung definieren.

-   Der Hull-Shader deklariert den erforderlichen Zustand für die [Tessellatorstufe (TS-Stufe)](tessellator-stage--ts-.md). Dazu gehören Informationen wie die Anzahl der Kontrollpunkte, die Art der Patchfläche und die Art der beim Tesselieren zu verwendenden Partitionierung. Diese Informationen stehen als Deklarationen in der Regel am Anfang des Shadercodes.
-   Tesselationsfaktoren bestimmen, wie oft jeder Patch unterteilt werden soll.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


1 bis 32 Ausgabekontrollpunkte, die zusammen einen Patch bilden.

-   Die Shaderausgabe umfasst 1 bis 32 Kontrollpunkte, unabhängig von der Anzahl der Tesselationsfaktoren. Die Kontrollpunktausgabe eines Hull-Shaders kann von der Domain-Shader-Stufe weiterverarbeitet werden. Patchkonstanten können von einem Domain-Shader weiterverarbeitet werden. Tesselationsfaktoren können von der [Tessellatorstufe (TS-Stufe)](tessellator-stage--ts-.md) und der [Domain-Shader-Stufe (DS-Stufe) weiterverarbeitet werden](domain-shader-stage--ds-.md).
-   Wenn der Hull-Shader Randtesselationsfaktoren auf Werte ≤ 0 oder NaN setzt, wird der Patch nicht verwendet. Somit wird die Tessellatorstufe nur manchmal ausgeführt, der Domain-Shader nicht, und für den Patch wird keine sichtbare Ausgabe erstellt.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


```hlsl
[patchsize(12)]
[patchconstantfunc(MyPatchConstantFunc)]
MyOutPoint main(uint Id : SV_ControlPointID,
     InputPatch<MyInPoint, 12> InPts)
{
     MyOutPoint result;
     
     ...
     
     result = TransformControlPoint( InPts[Id] );

     return result;
}
```

Finden Sie unter [Vorgehensweise: Erstellen Sie einen Hullshader](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-advanced-stages-hull-shader-create).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




