---
title: Hullshaderphase (HS)
description: Die Hull-Shader-Phase (HS) ist eine der Mosaik Stufen, die eine einzelne Oberfläche eines Modells effizient in viele Dreiecke zerlegen.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- Hullshaderphase (HS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 11c9f0a52c4a320306cf5dfd5ce90fd5bbe2c407
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165014"
---
# <a name="hull-shader-hs-stage"></a>Hullshaderphase (HS)

Die Hull-Shader-Phase (HS) ist eine der Mosaik Stufen, die eine einzelne Oberfläche eines Modells effizient in viele Dreiecke zerlegen. Die Hull-Shader-Stufe (HS) erzeugt einen Geometry-Patch (und Patch-Konstanten), die den einzelnen Eingabe Patches (Quad, Dreieck oder Line) entsprechen. Ein Hull-Shader wird einmal pro Patch aufgerufen und transformiert Eingabe Steuerungs Punkte, die eine niedrige Ordnung definieren, in Steuerungs Punkte, die einen Patch bilden. Außerdem werden einige pro-Patch-Berechnungen durchführt, um Daten für die Mosaik [Stufe (TS)](tessellator-stage--ts-.md) und die [Domänen-Shader (DS)](domain-shader-stage--ds-.md)bereitzustellen.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


![Diagramm der Hülle-Shader-Phase](images/d3d11-hull-shader.png)

Die drei Mosaik Phasen arbeiten zusammen, um eine höhere Reihenfolge von Oberflächen (die das Modell einfach und effizient halten) auf viele Dreiecke zu konvertieren, um das ausführliche Rendering innerhalb der Grafik Pipeline zu gewährleisten. Die Mosaik Phasen umfassen die Phase "Hull Shader (HS)", "Mosaik Stufe" [(TS)](tessellator-stage--ts-.md)und " [Domänen-Shader (DS)](domain-shader-stage--ds-.md)".

Die Hull-Shader-Phase (HS) ist eine programmierbare Shader-Stufe. Ein Hull-Shader wird mit einer HLSL-Funktion implementiert.

Ein Hull-Shader arbeitet in zwei Phasen: einer Steuerungspunkt Phase und einer patchkonstantenphase, die parallel von der Hardware ausgeführt werden. Der HLSL-Compiler extrahiert die Parallelität in einem Hull-Shader und codiert Sie in Bytecode, der die Hardware steuert.

-   Die Steuerungspunkt Phase wird für jeden Steuerungspunkt einmal betrieben, die Kontrollpunkte für einen Patch gelesen und ein Ausgabe Steuerungspunkt (durch eine **controlpointid**identifiziert) erzeugt.
-   Die Patch-Konstante-Phase arbeitet einmal pro Patch, um Edge-Mosaik Faktoren und andere pro-Patch-Konstanten zu generieren. Intern können viele patchkonstantenphasen gleichzeitig ausgeführt werden. Die patchkonstantenphase hat schreibgeschützten Zugriff auf alle Eingabe-und Ausgabe Steuerungs Punkte.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Der


Zwischen 1 und 32 Eingabe Steuerungs Punkten, die gemeinsam eine Oberfläche für eine niedrige Ordnung definieren.

-   Der Hull-Shader deklariert den Zustand, der für die Mosaik [Stufe (TS)](tessellator-stage--ts-.md)erforderlich ist. Hierzu gehören Informationen wie z. b. die Anzahl der Kontrollpunkte, der Typ der Patchfläche und der Partitionierungstyp, der beim Mosaik Vorgang verwendet werden soll. Diese Informationen werden in der Regel am Anfang des Shader-Codes als Deklarationen angezeigt.
-   Die Mosaik Faktoren legen fest, wie viel zwischen den einzelnen Patches unterteilt werden soll.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgeben


Zwischen 1 und 32 Ausgabe Steuerungs Punkten, die gemeinsam einen Patch bilden.

-   Die Shader-Ausgabe liegt zwischen 1 und 32 Kontrollpunkten, unabhängig von der Anzahl der Mosaik Faktoren. Die Kontrollpunkte, die von einem Hull-Shader ausgegeben werden, können von der Domänen-Shader-Stufe verwendet werden. Patch-konstantendaten können von einem Domänen-Shader genutzt werden. Mosaik Faktoren können von der Mosaik [Stufe (TS)](tessellator-stage--ts-.md) und der [Domänen-Shader-Stufe (DS)](domain-shader-stage--ds-.md)genutzt werden.
-   Wenn der Hull-Shader einen beliebigen Edge-Mosaik Faktor auf den Wert "-0" oder "NaN" festlegt, wird der Patch als "herausgefiltert werden" (ausgelassen) festgelegt. Folglich kann die Mosaik Phase nicht ausgeführt werden, und der Domänen-Shader wird nicht ausgeführt, und für diesen Patch wird keine sichtbare Ausgabe erzeugt.

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

Weitere Informationen finden [Sie unter Gewusst wie: Erstellen eines Hull-Shaders](/windows/desktop/direct3d11/direct3d-11-advanced-stages-hull-shader-create).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 