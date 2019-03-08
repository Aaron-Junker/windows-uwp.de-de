---
title: Geometryshaderphase (GS)
description: Die Geometry-Shader-Stufe (GS-Stufe) verarbeitet vollständige Grundformen (Dreiecke, Linien und Punkte) zusammen mit deren angrenzenden Vertices.
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- Geometryshaderphase (GS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63c678f4b2dde1a5e35c0131b5154493c9703951
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623375"
---
# <a name="geometry-shader-gs-stage"></a>Geometryshaderphase (GS)


Die Geometry-Shader-Stufe (GS-Stufe) verarbeitet vollständige Grundformen (Dreiecke, Linien und Punkte) zusammen mit deren angrenzenden Vertices. Sie ist nützlich für Algorithmen wie Point Sprite Expansion, Dynamic Particle Systems, und Shadow Volume Generation. Sie unterstützt Geometrieverstärkung und -abschwächung.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


Die Geometry-Shader-Stufe verarbeitet vollständige Grundformen: Dreiecke (3 Vertices mit bis zu 3 angrenzenden Vertices), Linien (2 Vertices mit bis zu 2 angrenzenden Vertices) und Punkte (1 Vertex).

![Illustration eines Dreiecks und einer Linie mit angrenzenden Vertices](images/d3d10-gs.png)

Der Geometry-Shader unterstützt auch beschränkte Geometrieverstärkung und -abschwächung. Der Geometry-Shader kann eine eingegebene Grundform verwerfen oder daraus eine oder mehrere neue Grundformen generieren.

Die Geometry-Shader-Stufe (GS-Stufe) ist eine programmierbare Shaderstufe. Sie wird als abgerundeter Block im [Grafikpipeline](graphics-pipeline.md)-Diagramm angezeigt. Diese Shaderstufe stellt eine eigene, auf Shadermodellen basierende Funktionalität bereit (s. [Gemeinsamer Shaderkern](https://msdn.microsoft.com/library/windows/desktop/bb509580))

Die Geometry-Shader-Stufe ist besonders geeignet für folgende Algorithmen:

-   Point Sprite Expansion
-   Dynamic Particle Systems
-   Fur/Fin Generation
-   Shadow Volume Generation
-   Single Pass Render-to-Cubemap
-   Per-Primitive Material Swapping
-   Per-Primitive Material Setup: Hierzu gehört das Erstellen baryzentrischer Koordinaten als Daten für Grundformen, sodass ein Pixel-Shader allgemeine Attribute interpolieren kann.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


Die Geometry-Shader-Stufe führt anwendungsspezifischen Shadercode aus, mit vollständigen Grundformen als Eingabe und der Fähigkeit, Vertices als Ausgabe zu generieren. Im Gegensatz zu den Eingaben für einen Vertex-Shader, der einen einzelnen Vertex verarbeitet, sind die Eingaben für einen Geometry-Shader die Vertices vollständiger Grundformen (drei Vertices für ein Dreieck, zwei Vertices für eine Linie oder ein Vertex für einen Punkt). Geometry-Shader können auch die Vertexdaten für die am Rand anschließenden Grundformen als Eingabe übernehmen (drei zusätzliche Vertices für ein Dreieck, zwei zusätzliche Vertices für eine Line).

Die Geometrie-Shader-Stufe nutzen kann die **SV\_PrimitiveID** vom System generierter Wert, der automatisch generierten durch die [Eingabe-Assembler (IA) Phase](input-assembler-stage--ia-.md). Damit können Grundformdaten abgerufen oder bei Bedarf verarbeitet werden.

Wenn ein Geometry-Shader aktiv ist, wird er einmal für jede Grundform aufgerufen, die übergeben oder weiter oben in der Pipeline generiert wurde. Für jeden Aufruf des Geometry-Shaders dienen Daten der aufrufenden Grundform (ein einzelner Punkt, eine einzelne Line oder ein einzelnes Dreieck) als Eingabe. Ein Dreieckstrip von weiter oben in die Pipeline würde für jedes einzelne Dreieck im Strip zum Aufruf des Geometry-Shaders führen (so als würde der Strip in eine Dreiecksliste erweitert). Alle Eingabedaten für jeden Vertex in der einzelnen Grundform sind verfügbar ist (also 3 Vertices für ein Dreieck), dazu die Daten angrenzender Vertices (falls vorhanden und verfügbar).

Übliche Vertexabkürzungen:

|     |                 |
|-----|-----------------|
| TV  | Triangle vertex (Dreiecksvertex) |
| LV  | Line vertex (Linienvertex)     |
| AV  | Adjacent vertex (angrenzender Vertex) |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


Die Geometry-Shader-Stufe (GS-Stufe) kann mehrere Vertices ausgeben, die eine einzelne ausgewählte Topologie darstellen. Ausgabetopologien des Geometry-Shaders sind **Tristrip**, **Linestrip** und **Pointlist**. Die Anzahl der ausgegebenen Grundformen kann mit jedem Aufruf des Geometry-Shaders variieren. Die maximale Anzahl auszugebender Vertices muss allerdings statisch deklariert werden muss. Die nach einem Geometry-Shader-Aufruf ausgegeben Strips können beliebig lang sein. Neue Strips können mit der HLSL-Funktion [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) erstellt werden.

Die Ausführung einer Geometry-Shader-Instanz erfolgt unabhängig von anderen Aufrufen, nur die Daten werden den Streams seriell hinzugefügt. Die Ausgaben eines bestimmten Geometry-Shader-Aufrufs sind unabhängig von anderen Aufrufen (jedoch wird die Reihenfolge berücksichtigt). Ein Geometry-Shader, der Dreieckstrips generiert, beginnt nach jedem Aufruf mit einem neuen Strip.

Eine Geometry-Shader-Ausgabe kann über die Streamausgabestufe in eine Rasterizerstufe und/oder in einen Vertexpuffer im Speicher geleitet werden. In den Speicher geleitete Ausgaben werden in einzelne Punkt-, Zeilen- oder Dreieckslisten erweitert (genau so, als würden sie an den Rasterizer übergeben).

Ein Geometry-Shader gibt jeweils Daten für einen Vertex aus, der einem Ausgabestreamobjekt angefügt wird. Die Topologie der Streams wird durch eine Deklaration festgelegt, wobei ein **TriangleStream**, **LineStream** oder **PointStream** als Ausgabe einer GS-Stufe vorgegeben werden kann.

Es gibt drei Arten von Streamobjekten verfügbar: **TriangleStream**, **LineStream** und **PointStream**, die alle auf Vorlagen basierende Objekte sind. Die Topologie der Ausgabe wird durch ihren jeweiligen Objekttyp bestimmt, während das Format der in den Datenstrom angefügten Vertices durch den Vorlagentyp bestimmt wird.

Wenn eine Geometrie-Shader-Ausgabe als einen Wert für die System-interpretiert identifiziert wird (z. B. **SV\_RenderTargetArrayIndex** oder **SV\_Position**), Hardware untersucht diese Daten und führt einige Verhalten hängt von dem Wert, sondern um die Daten selbst in die nächste shaderstufe für die Eingabe übergeben. Wenn solche Daten aus der Geometrie-Shader Bedeutung für die Hardware pro primitiven aufweist (z. B. **SV\_RenderTargetArrayIndex** oder **SV\_ViewportArrayIndex**), statt auf einer pro-Vertex-Basis (z. B. **SV\_ClipDistance\[n\]**  oder **SV\_Position**), die primitiven Daten erstellt aus der führende Vertex ausgegeben, für den primitiven Typ.

Vom Geometry-Shader können unvollständige Grundformen ausgegeben werden, wenn der Geometry-Shader beendet wird, bevor die Grundform fertig ist. Solche unvollständigen Grundformen werden ohne Rückmeldung verworfen. Das entspricht der Art und Weise, in der ein Input-Assembler (IA) unvollständige Grundformen behandelt.

Die Geometry-Shader kann Lade- und Textur Sampling-Vorgänge ausführen, für die keine Screenspaceableitungen erforderlich sind (**Samplelevel**, **Samplecmplevelzero**, **Samplegrad**).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




