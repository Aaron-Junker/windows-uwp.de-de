---
title: Primitive Topologien
description: Direct3D unterstützt mehrere primitive Topologien, die definieren, wie Scheitel Punkte von der Pipeline interpretiert und gerendert werden, z. b. Punkt Listen, Linien Listen und Dreieck Streifen.
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- Primitive Topologien
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 45abb0c356b4ee6923bf6edd0b462f568749de5e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156334"
---
# <a name="primitive-topologies"></a>Primitive Topologien


Direct3D unterstützt mehrere primitive Topologien, die definieren, wie Scheitel Punkte von der Pipeline interpretiert und gerendert werden, z. b. Punkt Listen, Linien Listen und Dreieck Streifen.

## <a name="span-idprimitive_typesspanspan-idprimitive_typesspanspan-idprimitive_typesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>Grundlegende primitive Topologien


Die folgenden grundlegenden primitiven Topologien (oder primitive Typen) werden unterstützt:

-   [Punktelisten](point-lists.md)
-   [Zeilenlisten](line-lists.md)
-   [Zeilenstrips](line-strips.md)
-   [Dreieckslisten](triangle-lists.md)
-   [Dreiecksstrips](triangle-strips.md)

Eine Visualisierung der einzelnen primitiven Typen finden Sie im Diagramm weiter unten in diesem Thema in der [Richtung "Richtung" und führenden Scheitelpunkt Positionen](#winding-direction-and-leading-vertex-positions).

Die [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md) liest Daten aus Scheitel Punkten und Index Puffern, assembliert die Daten in diese primitiven und sendet die Daten dann an die restlichen Pipeline Stufen.

## <a name="span-idprimitive_adjacencyspanspan-idprimitive_adjacencyspanspan-idprimitive_adjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>Primitive Nähe


Alle primitiven Direct3D-Typen (außer der Punkt Liste) stehen in zwei Versionen zur Verfügung: ein primitiver Typ mit der Nähe und ein primitiver Typ ohne Unterschied. Primitive mit einer bestimmten Konsistenz enthalten einige der umgebenden Scheitel Punkte, während primitive ohne die unter Verwendung nur die Scheitel Punkte des zielprimitives enthalten. Beispielsweise verfügt der primitiv der Zeilen Liste über eine entsprechende Zeilen Listen primitive, die Informationen zu den Gründen enthält.

Angrenzende primitive dienen dazu, weitere Informationen über die Geometrie bereitzustellen, die nur über einen Geometry-Shader sichtbar sind. Die Verwendung ist nützlich für Geometry-Shader, die die Silhouette-Erkennung, die Schatten Volumeverschlüsselung usw. verwenden.

Nehmen Sie z. b. an, Sie möchten eine Dreiecks Liste mit Informationen zeichnen. Eine Dreiecks Liste, die 36 Scheitel Punkte (mit der Konsistenz) enthält, ergibt 6 abgeschlossene primitive. Primitive mit der Verwendung (mit Ausnahme von Zeilen Streifen) enthalten genau doppelt so viele Scheitel Punkte wie die äquivalente primitive, wobei jeder weitere Scheitelpunkt ein angrenzender Scheitelpunkt ist.

## <a name="span-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>Richtung und führende Scheitelpunkt Positionen


Wie in der folgenden Abbildung dargestellt, ist ein führender Scheitelpunkt der erste nicht benachbarte Scheitelpunkt in einem primitiven. Für einen primitiven Typ können mehrere führende Vertices definiert werden, sofern jeder für einen anderen primitiven verwendet wird.

-   Bei einem Dreiecks Streifen mit der Sicherheit sind die führenden Scheitel Punkte 0, 2, 4, 6 usw.
-   Bei einem Zeilen Streifen mit der Sicherheit sind die führenden Vertices 1, 2, 3 usw.
-   Ein angrenzender primitiv hat andererseits keinen führenden Scheitelpunkt.

Die folgende Abbildung zeigt die Scheitelpunkt Anordnung für alle primitiven Typen, die vom Eingabe Assembler erzeugt werden können.

![Diagramm der Scheitelpunkt Anordnung für primitive Typen](images/d3d10-primitive-topologies.png)

Die Symbole in der vorangehenden Abbildung werden in der folgenden Tabelle beschrieben.

| Symbol                                                                                   | name              | BESCHREIBUNG                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![Symbol für einen Scheitelpunkt](images/d3d10-primitive-topologies-vertex.png)                     | Scheitelpunkt            | Ein Punkt im 3D-Raum.                                                                |
| ![Symbol für die Richtung der Richtung](images/d3d10-primitive-topologies-winding-direction.png) | Richtung | Die Scheitelpunkt Reihenfolge beim Assemblieren eines primitiven. Kann im Uhrzeigersinn oder gegen den Uhrzeigersinn stehen. |
| ![Symbol für führenden Scheitelpunkt](images/d3d10-primitive-topologies-leading-vertex.png)       | Führender Scheitelpunkt    | Der erste nicht benachbarte Scheitelpunkt in einem primitiven, der Daten pro Konstante enthält.       |

 

## <a name="span-idgenerating_multiple_stripsspanspan-idgenerating_multiple_stripsspanspan-idgenerating_multiple_stripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>Erzeugen mehrerer Striche


Sie können mehrere Striche durch die Bereichs Bearbeitung generieren. Sie können einen Strip-Cut ausführen, indem Sie die Funktion [restartstrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL explizit aufrufen oder einen speziellen Indexwert in den Index Puffer einfügen. Dieser Wert ist – 1, d. h. 0xFFFFFFFF für 32-Bit-Indizes oder 0xFFFF für 16-Bit-Indizes.

Ein Index von – 1 gibt einen expliziten ' Cut ' oder ' restart ' des aktuellen Streifens an. Der vorherige Index schließt den vorherigen primitiven oder Strip ab, und der nächste Index startet einen neuen primitiven oder Strip.

Weitere Informationen zum Erstellen mehrerer Striche finden Sie unter [Geometry-Shader (GS)-Phase](geometry-shader-stage--gs-.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Eingabeassemblerphase (IA)](input-assembler-stage--ia-.md)

[Grafikpipeline](graphics-pipeline.md)

 

 