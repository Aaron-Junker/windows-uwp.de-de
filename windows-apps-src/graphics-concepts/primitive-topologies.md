---
title: Primitive Topologien
description: "Direct3D unterstützt mehrere primitive Topologien, die definieren, wie Scheitelpunkte, z.B. Punktelisten, Zeilenlisten und Dreieckstrips von der Pipeline interpretiert und gerendert werden."
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords: Primitive Topologien
author: mtoepke
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: d0d6bed3cbccd37fcd4fc835273099e8d26f671d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="primitive-topologies"></a>Primitive Topologien


Direct3D unterstützt mehrere primitive Topologien, die definieren, wie Scheitelpunkte, z.B. Punktelisten, Linienlisten und Dreieckstrips von der Pipeline interpretiert und gerendert werden.

## <a name="span-idprimitivetypesspanspan-idprimitivetypesspanspan-idprimitivetypesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>Grundlegende primitive Topologien


Die folgenden grundlegenden primitiven Topologien (bzw. Grundtypen) werden unterstützt:

-   [Punktelisten](point-lists.md)
-   [Zeilenlisten](line-lists.md)
-   [Zeilenstrips](line-strips.md)
-   [Dreieckslisten](triangle-lists.md)
-   [Dreiecksstrips](triangle-strips.md)

Eine Visualisierung der einzelnen Grundtypen finden Sie im Diagramm weiter unten in diesem Thema unter [Wicklungsrichtung und führende Scheitelpunktpositionen](#winding-direction-and-leading-vertex-positions).

Die [Eingabeassemblerphase (IA)](input-assembler-stage--ia-.md) liest Daten aus Vertex- und Indexpuffern, setzt die Daten in diesen Grundtypen zusammen und sendet die Daten dann an die verbleibenden Pipelinephasen.

## <a name="span-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>Grundtypen mit angrenzenden Daten


Alle Direct3D-Grundtypen (mit Ausnahme der Punkteliste) sind in zwei Versionen verfügbar: einem Grundtyp mit angrenzenden Daten und einen Grundtyp ohne angrenzende Daten. Grundtypen mit angrenzenden Daten enthalten einige der umgebenden Scheitelpunkte, während Grundtypen ohne angrenzende Daten nur die Scheitelpunkte des Zielgrundtyps enthalten. Beispielsweise ist für den Grundtyp „Zeilenliste“ ein entsprechender Grundtyp vorhanden, der angrenzende Daten umfasst.

Grundtypen mit angrenzenden Daten stellen weitere Informationen über die Geometrie bereit und können nur über einen Geometrieshader angezeigt werden. Angrenzende Daten eignen sich für Geometrieshader, die Silhouettenerkennung, Schattenvolumenextrusion usw. verwenden.

Beispiel: Sie möchten eine Dreiecksliste mit angrenzenden Daten zeichnen. Eine Dreiecksliste, die 36 Scheitelpunkte (mit angrenzenden Daten) enthält, ergibt 6 abgeschlossene Grundtypen. Grundtypen mit angrenzenden Daten (außer Zeilenstrips) enthalten genau doppelt so viele Scheitelpunkte wie der entsprechende Grundtyp ohne angrenzende Daten, wobei jeder zusätzliche Scheitelpunkt ein angrenzender Scheitelpunkt ist.

## <a name="span-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>Wicklungsrichtung und führende Scheitelpunktpositionen


Wie in der folgenden Abbildunggezeigt, ist ein führender Scheitelpunkt der nicht erste nicht angrenzende Scheitelpunkt in einen Grundtyp. Für einen Grundtyp können mehrere führende Scheitelpunkte definiert sein, vorausgesetzt, jeder wird für einen anderen Grundtyp verwendet.

-   Für einen Dreiecksstrip mit angrenzenden Daten sind die führenden Scheitelpunkte 0, 2, 4, 6 usw.
-   Für einen Zeilenstrip mit angrenzenden Daten sind die führenden Scheitelpunkte 1, 2, 3 usw.
-   Andererseits hat ein Grundtyp mit angrenzenden Daten keinen führenden Scheitelpunkt.

Die folgende Abbildungzeigt die Scheitelpunktanordnung für alle Grundtypen, die der Eingabeassembler erzeugen kann.

![Diagramm der Scheitelpunktanordnungen für Grundtypen](images/d3d10-primitive-topologies.png)

In der folgenden Tabelle werden die Symbole aus der obigen Abbildungbeschrieben.

| Symbol                                                                                   | Name              | Beschreibung                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![Symbol für einen Scheitelpunkt](images/d3d10-primitive-topologies-vertex.png)                     | Scheitelpunkt            | Ein Punkt im 3D-Raum.                                                                |
| ![Symbol für Wicklungsrichtung](images/d3d10-primitive-topologies-winding-direction.png) | Wicklungsrichtung | Die Scheitelpunktanordnung beim Zusammensetzen eines Grundtyps. Kann im Uhrzeigersinn oder gegen den Uhrzeigersinn verlaufen. |
| ![Symbol für führenden Scheitelpunkt](images/d3d10-primitive-topologies-leading-vertex.png)       | Führender Scheitelpunkt    | Der erste Scheitelpunkt ohne angrenzende Daten in einem Grundtyp, der Daten pro Konstante enthält.       |

 

## <a name="span-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>Generieren mehrerer Strips


Mehrere Strips lassen sich mittels Schneiden von Strips generieren. Sie können einen Stripschnitt durchführen, indem Sie ausdrücklich die HSLS-Funktion [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) aufrufen, oder indem Sie einen besonderen Indexwert in den Indexpuffer einfügen. Dieser Wert ist-1, was 0xffffffff für 32-Bit-Indizes oder 0xffff für 16-Bit-Indizes entspricht.

Ein Index von -1 gibt ein explizites „Ausschneiden“ oder „Neustarten“ des aktuellen Strips an. Der vorherige Index schließt den vorherigen Grundtyp oder Strip ab, und der nächsten Index beginnt einen neuen Grundtyp oder Strip.

Weitere Informationen zum Generieren von mehreren Strips finden Sie unter [Geometryshaderphase (GS)](geometry-shader-stage--gs-.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Eingabeassemblerphase (IA)](input-assembler-stage--ia-.md)

[Grafikpipeline](graphics-pipeline.md)

 

 




