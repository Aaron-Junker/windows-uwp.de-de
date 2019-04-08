---
title: Grafikpipeline
description: Die Direct3D-Grafikpipeline dient zum Generieren von Grafiken für Echtzeitspiele. Datenflüsse vom Eingang zum Ausgang durch jede konfigurierbare oder programmierbare Phase
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Grafikpipeline
- Pipelinestufen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 55621cec768e0aac680c3a84fd803e591459a97d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605435"
---
# <a name="graphics-pipeline"></a>Grafikpipeline


Die Direct3D-Grafikpipeline dient zum Generieren von Grafiken für Echtzeitspiele. Datenflüsse vom Eingang zum Ausgang durch jede konfigurierbare oder programmierbare Phase

Alle Stufen können mithilfe der Direct3D-API konfiguriert werden. Stufen, die allgemeine Shaderkerne (die abgerundeten rechteckige Blöcke) bereitstellen, sind mit der [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)-Programmiersprache programmierbar. Dadurch wird die Pipeline extrem flexibel und anpassbar.

Die am häufigsten verwendeten Stufen sind die Vertex-Shader-Stufe (VS-Stufe) und die Pixel-Shader-Stufe (PS-Stufe). Wenn Sie nicht einmal diese Shaderstufen bereitstellen, werden standardmäßige No-Op- und Pass-Through-Vertex- und Pixel-Shader verwendet.

![Diagramm des Datenflusses in der programmierbaren Pipeline für Direct3D 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Input Assembler-Stufe

|-|-| |Die [Input-Assembler-Stufe (IA-Stufe)](input-assembler-stage--ia-.md) stellt für die Pipeline die Daten der grafischen Grundformen (Dreiecke, Linien und Punkte) und zugehörige Daten bereit, einschließlich semantischer IDs, welche die Effizienz der Shader erhöhen, weil damit Grundformen identifiziert werden können, die noch nicht verarbeitet wurden, sodass nur diese verarbeitet werden müssen. | |Eingabe|Primitive Daten (Dreiecke, Linien, und/oder Punkte), aus von Benutzern ausgefüllten Puffern im Arbeitsspeicher. Und möglicherweise zugehörige Daten. Ein Dreieck würde aus 3 Vertices für jedes Dreiecke und möglicherweise 3 Vertices pro Dreieck für angrenzende Daten bestehen.| | Ausgabe | Primitive Daten mit angefügten systemgenerierten Werten (z. B. eine primitive ID, eine Instanz-ID oder eine Vertex-ID).|

## <a name="vertex-shader-stage"></a>Vertex-Shader-Stufe

|-|-| |Zweck|Die [Vertex-Shader-Stufe](vertex-shader-stage--vs-.md) verarbeitet Vertices und führt dabei in der Regel Vorgänge wie Transformationen oder das Anwenden von Skins und Beleuchtungen aus. Ein Vertex-Shader verarbeitet immer nur einen Eingabevertex und erzeugt daraus einen Ausgabevertex. Einzelne Vertex-Vorgänge wie Transformationen, Anwenden von Skins, Morphing und Beleuchtung pro Vertex.| | Eingabe | Ein einzelner Vertex mit Vertex-ID und Instanzen-ID, systemgenerierte Werte. Jeder Eingabevertex für einen Vertex-Shader kann aus mehreren 32-Bit-Vektoren bestehen (maximal 16, mit jeweils bis zu 4-Komponenten).| |Ausgabe|Ein einzelner Vertex. Jeder Ausgabevertex kann aus mehreren 32-Bit-Vektoren bestehen (maximal 16 mit jeweils 4 Komponenten).|
 
## <a name="hull-shader-stage"></a>Hull-Shader-Stufe
 
|-|-| |Zweck|Die [Hull-Shader-Stufe](hull-shader-stage--hs-.md) ist eine der Tessellatorstufen, auf der eine durchgehende Fläche eines Modell effizient in vielen Dreiecke unterteilt wird. Ein Hull-Shader wird einmal pro Patch aufgerufen. Er transformiert Eingabekontrollpunkte, die eine Oberfläche niederer Ordnung definieren, in Kontrollpunkte, die einen Patch bilden. Es werden auch einige Berechnungen pro Patch zum Bereitstellen von Daten für die Tessellatorstufe (TS-Stufe) und die Domain-Shader-Stufe (DS-Stufe) durchgeführt.| | Eingabe | Zwischen 1 und 32 Steuerelemente, die zusammen eine untere Oberfläche definieren.| | Ausgabe | Zwischen 1 und 32 Ausgabe Kontrollpunkte, die zusammen den Patch bilden. Der Hull-Shader deklariert den für die Tessellatorstufe (TS-Stufe) erforderlichen Status. Dies schließt bestimmte Informationen ein, wie z. B. die Anzahl der Kontrollpunkte, den Typ der Patchfläche und die Art der Partitionierung, die bei der Tesselierung verwendet werden soll.|

## <a name="tessellator-stage"></a>Tessellatorstufe

|-|-| |Zweck | Die [Tessellatorstufe (TS-Stufe)](tessellator-stage--ts-.md) erstellt ein Samplingmuster der Domäne, das die Geometrie des Patches darstellt und eine Reihe kleinerer Objekte (Dreiecke, Punkte oder Linien) generiert, die diese Beispiele verbinden.| |Eingabe |Der Tessellator arbeitet einmal pro Patch und verwendet die Tessellationsfaktoren (die angeben, wie detailliert die Domäne tesseliert wird) und den Partitionierungstyp (der den zum Aufteilen eines Patches in Slices verwendeten Algorithmus angibt), die von der Hüllen-Shaderphase übergeben werden. | |Ausgabe|Die Tessellator gibt UV-Koordinaten (und optional W-Koordinaten) und die Oberflächentopologie an die Domänen-Shader-Stufe aus.|

## <a name="domain-shader-stage"></a>Domain-Shader-Stufe

|-|-| |Zweck|Die [Domain-Shader-Stufe (DS-Stufe)](domain-shader-stage--ds-.md) berechnet die Vertexposition eines unterteilten Punkts im Ausgabepatch, also die Vertexposition, die jedem Domänenbeispiel entspricht. Ein Domain-Shader wird einmal pro Ausgabepunkt der Tessellatorstufe ausgeführt und hat schreibgeschützten Zugriff auf die UV-Koordinaten der Ausgabe der Tessellatorstufe, den Ausgabepatch des Hull-Shaders und dessen Ausgabepatchkonstanten.| |Eingabe|Ein Domain-Shader übernimmt Ausgabekontrollpunkte aus der [Hull-Shader-Stufe (HS-Stufe)](hull-shader-stage--hs-.md). Die Hull-Shader-Ausgaben umfassen: Steuern Sie, Punkte, Patch constant-Daten und mosaikfaktoren (die mosaikfaktoren können enthalten die Werte, die von der mosaikeingabefaktoren festen Funktionen sowie die Rohwerte - verwendet werden, vor der Rundung von Mosaik für ganze Zahl - die Geomorphing, z. B. erleichtert ). Ein Domain-Shader wird von der [Tessellatorstufe (TS-Stufe)](tessellator-stage--ts-.md) einmal pro Ausgabekoordinate aufgerufen.| |Ausgabe |Die Domain-Shader-Stufe (DS-Stufe) gibt die Vertexposition eines unterteilten Punkts im Ausgabepatch aus.|

## <a name="geometry-shader-stage"></a>Geometry-Shader-Stufe (GS-Stufe)

|-|-| |Zweck|Die [Geometry-Shader-Stufe (GS-Stufe)](geometry-shader-stage--gs-.md) verarbeitet vollständige Grundformen (Dreiecke, Linien und Punkt) zusammen mit deren angrenzenden Vertices. Sie unterstützt Geometrieverstärkung und -abschwächung. Sie ist nützlich für Algorithmen wie Point Sprite Expansion, Dynamic Particle Systems, Fur/Fin Generation, Shadow Volume Generation, Single Pass Render-to-Cubemap, Per-Primitive Material Swapping und Per-Primitive Material Setup (dazu gehört das Erstellen baryzentrischer Koordinaten als Daten für Grundformen, sodass ein Pixel-Shader allgemeine Attribute interpolieren kann). | | Eingabe|Im Gegensatz zu den Eingaben für einen Vertex-Shader, der einen einzelnen Vertex verarbeitet, sind die Eingaben für einen Geometry-Shader die Vertices vollständiger Grundformen (drei Vertices für ein Dreieck, zwei Vertices für eine Linie oder ein Vertex für einen Punkt).| |Ausgabe|Die Geometry-Shader-Stufe (GS-Stufe) kann mehrere Vertices ausgeben, die eine einzelne ausgewählte Topologie darstellen. Ausgabetopologien des Geometry-Shaders sind <strong>Tristrip</strong>, <strong>Linestrip</strong> und <strong>Pointlist</strong>. Die Anzahl der ausgegebenen Grundformen kann mit jedem Aufruf des Geometry-Shaders variieren. Die maximale Anzahl auszugebender Vertices muss allerdings statisch deklariert werden muss. Die nach einem Geometry-Shader-Aufruf ausgegeben Strips können beliebig lang sein. Neue Strips können mit der HLSL-Funktion [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) erstellt werden.|

## <a name="stream-output-stage"></a>Streamoutputstufe (SO-Stufe)

|-|-| |Zweck|Die [Streamoutputstufe (SO-Stufe)](stream-output-stage--so-.md) liefert (streamt) kontinuierlich Vertexdaten aus der vorherigen aktiven Stufe einen oder mehrere Puffer im Speicher. In den Arbeitsspeicher gestreamte Daten können als Eingabedaten der Pipeline wieder zugeführt oder von der CPU eingelesen werden.| |Eingabe|Vertexdaten aus einer vorherigen Pipelinestufe.| |Ausgabe|Die Streamausgabephase (Stream Output, SO) gibt (oder streamt) kontinuierlich Vertexdaten aus der vorherigen aktiven Phase an einen oder mehrere Puffer im Speicher aus. Wenn die Geometry-Shader-Stufe (GS-Stufe) inaktiv ist und die Streamoutputstufe (SO-Stufe) aktiv ist, liefert diese kontinuierlich Vertexdaten der Domain-Shader-Stufe (DS-Stufe) in Puffer im Speicher. Wenn auch DS inaktiv ist, stammen die Daten aus der Vertex-Shader-Stufe (VS-Stufe).|

## <a name="rasterizer-stage"></a>Rasterizerstufe (RS-Stufe)

|-|-| |Zweck|Die [Rasterizerstufe](rasterizer-stage--rs-.md) blendet Grundformen aus, die nicht im Sichtfeld liegen, bereitet Grundformen für den Pixel-Shader vor und bestimmt, wie Pixel-Shader aufgerufen werden. Konvertiert Vektorinformationen (bestehend aus Formen oder Grundformen) in ein Rasterbild (bestehend aus Pixeln) zum Darstellen von 3D-Grafiken in Echtzeit.| |Eingabe | Für Vertices (x,y,z,w), die in die Rasterizerstufe einfließen, wird angenommen, dass sie sich im homogenen Clipbereich befinden. In diesem Koordinatenraum zeigt die X-Achse nach rechts, die Y-Achse nach oben und die Z-Achse von der Kamera weg. | |Ausgabe|Die Pixel, die momentan gerendert werden müssen. Enthält einige Vertexattribute zur Verwendung bei der Interpolation durch den Pixel-Shader.|

## <a name="pixel-shader-stage"></a>Pixel-Shader-Stufe
 
|-|-| | Zweck | Die [Pixel-Shader-Stufe (PS-Stufe)](pixel-shader-stage--ps-.md) empfängt interpolierte Daten einer Grundform und generiert pixelbezogene Daten wie z. B. Farben.| |Eingabe|Wenn die Pipeline ohne einen Geometry-Shader konfiguriert wurde, ist ein Pixel-Shader auf 16 Eingaben mit je 32 Bits und 4 Komponenten beschränkt. Andernfalls kann ein Pixel-Shader bis zu 32 Eingaben mit je 32 Bits und 4 Komponente verarbeiten. Zu den Eingabedaten für den Pixel-Shader gehören Vertexattribute, die mit oder ohne perspektivische Korrekturen interpoliert werden können oder als Konstanten pro Grundform behandelt werden können. Pixel-Shader-Eingaben werden anhand der Vertexattribute der zu rasternden Grundform interpoliert, basierend auf dem deklarierten Interpolationsmodus. Wenn eine Grundform vor der Rasterung gekappt wird, wird der Interpolationsmodus auch während des Clippingvorgangs Clipping berücksichtigt. | |Ausgabe|Ein Pixel-Shader kann bis zu 8 Farben mit je 32 Bits und 4 Komponente ausgeben oder keine Farbe, wenn das Pixel verworfen wird. Registerkomponenten für die Pixel-Shader-Ausgabe müssen deklariert werden, bevor sie verwendet werden können. Jedes Register kann eine unterschiedliche Ausgabeschreibmaske besitzen.|

## <a name="output-merger-stage"></a>Output-Merger-Stufe
 
|-|-| |Zweck|Die [Output-Merger-Stufe (OM)](output-merger-stage--om-.md) kombiniert verschiedene Ausgabedaten (Pixel-Shader-Werte, Tiefen- und Schabloneninformationen) mit dem Inhalt des Renderziels und Tiefen-/Schablonenpuffern, um das endgültige Pipelineergebnis zu generieren.| |Eingabe|Eingaben für Output-Merger sind der Pipelinezustand, die vom Pixel-Shader generierten Pixeldaten, die Inhalte der Renderziele und die Inhalte des Tiefen-/Schablonenpuffers.| |Ausgabe | Die endgültige gerenderte Pixelfarbe.|

## <a name="related-topics"></a>Verwandte Themen

- [Schulungsleitfaden für Direct3D-Grafiken](index.md)

- [Compute-Pipeline](compute-pipeline.md)
