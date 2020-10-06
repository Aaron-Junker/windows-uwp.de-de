---
title: Geometryshaderphase (GS)
description: Die Geometrie-Shader-Phase (GS) verarbeitet ganze primitive, Linien und Punkte sowie die angrenzenden Scheitel Punkte.
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- Geometryshaderphase (GS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e4573547ebffa0a58b4e2347162799035d8f898
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750006"
---
# <a name="geometry-shader-gs-stage"></a>Geometryshaderphase (GS)


Die Geometrie-Shader-Phase (GS) verarbeitet vollständige primitive: Dreiecke, Linien und Punkte, zusammen mit ihren angrenzenden Scheitel Punkten. Dies ist nützlich für Algorithmen, einschließlich Punkt Sprite-Erweiterung, dynamische Partikelsysteme und die Generierung von schattenvolumes. Es unterstützt die Geometrie Verstärkung und die Aufhebung der deverstärkung.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


Die Geometry-Shader-Stufe verarbeitet ganze primitive: Dreiecke (3 Scheitel Punkte mit bis zu 3 angrenzenden Scheitel Punkten), Linien (2 Scheitel Punkte mit bis zu 2 angrenzenden Scheitel Punkten) und Punkte (1 Scheitel Punkte).

![Abbildung eines Dreiecks und einer Linie mit angrenzenden Vertices](images/d3d10-gs.png)

Der Geometry-Shader unterstützt auch die eingeschränkte Geometrie Verstärkung und die Aufhebung der deverstärkung. Wenn ein Eingabe primitiv angegeben ist, kann der Geometry-Shader den primitiven verwerfen oder eine oder mehrere neue primitive ausgeben.

Die Stufe "Geometry-Shader (GS)" ist eine programmierbare Shader-Stufe. Sie wird im [Grafik Pipeline](graphics-pipeline.md) Diagramm als abgerundeter Block angezeigt. Diese Shader-Phase stellt eine eigene einzigartige Funktionalität bereit, die auf den shadermodellen basiert (siehe [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)).

Die Geometrie-Shader-Stufe eignet sich gut für Algorithmen, einschließlich:

-   Punkt Sprite-Erweiterung
-   Dynamische Partikelsysteme
-   Pelz/FIN-Generierung
-   Schattenvolumengenerierung
-   Einzelne Pass-to-cubemap
-   Pro primitiver Material Austausch
-   Einrichtung pro primitiver Materialien: Diese Funktion umfasst das Generieren von baryzentrierten Koordinaten als primitive Daten, sodass ein Pixelshader benutzerdefinierte Attribut Interpolationen durchführen kann.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Der


Die Geometrie-Shader-Stufe führt den von der Anwendung angegebenen Shader-Code mit vollständigen primitiven als Eingabe und die Möglichkeit zum Generieren von Scheitel Punkten bei der Ausgabe aus. Anders als bei Vertex-Shadern, die auf einem einzelnen Scheitelpunkt arbeiten, sind die Eingaben des Geometry-Shaders die Scheitel Punkte für einen vollständigen primitiven (drei Scheitel Punkte für Dreiecke, zwei Scheitel Punkte für Linien oder einzelner Scheitelpunkt für Punkt). Geometry-Shader können auch die Scheitelpunkt Daten für die Edge-angrenzenden primitiven als Eingabe (weitere drei Scheitel Punkte für ein Dreieck, eine zusätzliche zwei Scheitel Punkte für eine Linie) einbringen.

Die Geometrie-Shader-Stufe kann den vom System generierten **SV \_ primitiveid** -Wert nutzen, der automatisch von der [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md)generiert wird. Dies ermöglicht das Abrufen und berechnen primitiver Daten, wenn gewünscht.

Wenn ein Geometry-Shader aktiv ist, wird er einmal für jedes primitive aufgerufen, das in der Pipeline weiter unten oder generiert wurde. Bei jedem Aufruf des Geometry-Shaders werden die Daten für den aufrufenden primitiven als Eingabe angezeigt, unabhängig davon, ob es sich um einen einzelnen Punkt, eine einzelne Zeile oder ein einzelnes Dreieck handelt. Ein Dreiecks Streifen von früheren in der Pipeline führt zu einem Aufruf des Geometry-Shaders für jedes einzelne Dreieck im Strip (als wäre der Strip in eine Dreiecks Liste ausgelagert). Alle Eingabedaten für jeden Scheitelpunkt in der einzelnen primitiven sind verfügbar (d. h. 3 Scheitel Punkte für ein Dreieck) sowie angrenzende Scheitelpunkt Daten, sofern zutreffend und verfügbar.

Allgemeine Scheitelpunkt Abkürzungen:

| Abkürzung | Begriff |
| ------------ | ---- |
| TV  | Dreiecks Scheitelpunkt |
| LV  | Zeilen Scheitelpunkt     |
| Staff  | Angrenzender Scheitelpunkt |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgeben


Die Geometrie-Shader-Phase (GS) kann mehrere Scheitel Punkte ausstellen, die eine einzelne ausgewählte Topologie bilden. Verfügbare Geometry-Shader-ausgabetopologien sind **tristrip**, **linestrip**und **PointList**. Die Anzahl der ausgegebenen primitiven kann in jedem Aufruf des Geometry-Shaders frei variieren, obwohl die maximale Anzahl von Scheitel Punkten, die ausgegeben werden konnten, statisch deklariert werden muss. Von einem Geometry-Shader-Aufruf ausgegebene Bereichs Längen können beliebig sein, und neue Bänder können über die Funktion " [restartstrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL" erstellt werden.

Die Ausführung einer Geometry-shaderinstanz ist atomarisch von anderen aufrufen, mit dem Unterschied, dass die den Streams hinzugefügten Daten seriell sind. Die Ausgaben eines gegebenen Aufrufs eines Geometry-Shaders sind von anderen aufrufen unabhängig (obwohl die Reihenfolge beachtet wird). Ein Geometry-Shader, der Dreiecks Streifen erzeugt, startet bei jedem Aufruf einen neuen Strip.

Die Geometrie-Shader-Ausgabe kann in der Raster-Phase und/oder in einem Vertex-Puffer im Arbeitsspeicher über die Stream-Ausgabe Phase ausgegeben werden. Die Ausgabe in den Arbeitsspeicher wird auf einzelne Punkt-/Linien-/dreiterlisten erweitert (genau so, wie Sie an den Rasterizer übermittelt werden).

Ein Geometry-Shader gibt Daten jeweils einen Scheitelpunkt aus, indem er Vertices an ein ausgabestreamobjekt anfügt. Die Topologie der Streams wird durch eine Fixed-Deklaration bestimmt, wobei ein " **zanglestream**", " **LineStream** " und " **pointstream** " als Ausgabe für die GS-Stufe ausgewählt werden.

Es stehen drei Arten von Streamobjekten zur Verfügung: " **dreianglestream**", " **LineStream** " und " **pointstream**", bei denen es sich um Vorlagen Objekte handelt. Die Topologie der Ausgabe wird durch den jeweiligen Objekttyp bestimmt, während das Format der Scheitel Punkte, die an den Stream angehängt werden, durch den Vorlagentyp bestimmt wird.

Wenn eine Geometrie-Shader-Ausgabe als vom System interpretierter Wert (z. b. **SV \_ rendertargetarrayindex** oder **SV- \_ Position**) identifiziert wird, untersucht die Hardware diese Daten und führt ein gewisses Verhalten aus, das von dem Wert abhängt, zusätzlich zur Möglichkeit, die Daten selbst an die nächste Shader-Stufe für die Eingabe zu übergeben. Wenn eine solche Datenausgabe aus dem Geometry-Shader für die Hardware auf primitiver Basis (z. b. **SV \_ rendertargetarrayindex** oder **SV \_ viewportarrayindex**) und nicht pro Scheitelpunkt (z. b. **SV \_ clipdistance \[ n \] ** oder **SV \_ Position**) Bedeutung hat, werden die pro-primitive Daten aus dem führenden Vertex entnommen, der für das primitive ausgegeben wurde

Teilweise abgeschlossene primitive können vom Geometry-Shader generiert werden, wenn der Geometrie-Shader endet und der primitive unvollständig ist. Unvollständige primitive werden automatisch verworfen. Dies ähnelt der Art und Weise, in der die IA teilweise abgeschlossene primitive behandelt.

Der Geometry-Shader kann Lade-und Textur-samplingvorgänge ausführen, bei denen keine Bildschirm breiten Ableitungen erforderlich sind (**samplelevel**, **samplecmplevelzero**, **samplegrad**).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 