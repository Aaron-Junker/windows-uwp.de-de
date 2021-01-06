---
title: Grafikpipeline
description: Die Direct3D-Grafikpipeline dient zum Erstellen von Grafiken für Echtzeitspiele. Von der Eingabe zur Ausgabe fließen Daten durch die einzelnen konfigurier- oder programmierbaren Phasen.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Grafikpipeline
- Pipelinestufen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2032a57b598f08b24c1d52cecfa4f92b90591ec0
ms.sourcegitcommit: 48702934676ae366fd46b7d952396c5e2fb2cbbe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927793"
---
# <a name="graphics-pipeline"></a>Grafikpipeline

Die Direct3D-Grafikpipeline dient zum Erstellen von Grafiken für Echtzeitspiele. Von der Eingabe zur Ausgabe fließen Daten durch die einzelnen konfigurier- oder programmierbaren Phasen.

Alle Stufen können mithilfe der Direct3D-API konfiguriert werden. Phasen mit allgemeinen shaderkernen (die abgerundeten rechteckigen Blöcke) können mithilfe der [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl) -Programmiersprache Programmiersprache verwendet werden. Dadurch ist die Pipeline äußerst flexibel und anpassbar.

Die am häufigsten verwendeten sind die Vertex-Shader-Phase (VS) und die Pixel-Shader-Stufe (PS). Wenn Sie selbst diese Shader-Stufen nicht bereitstellen, werden ein standardmäßiger, Pass-Through-Vertex und Pixel-Shader verwendet.

![Diagramm des Datenflusses in der programmierbaren Direct3D 11-Pipeline](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Eingabe Assembler-Stufe

In der [Eingabe Assembler-Stufe (IA)](input-assembler-stage--ia-.md) werden primitive und Daten in der Pipeline zur Verfügung gestellt, wie z. b. Dreiecke, Linien und Punkte, einschließlich Semantik-IDs, um Shader effizienter zu gestalten, indem die Verarbeitung auf primitive Elemente reduziert wird, die noch nicht verarbeitet wurden.

- Eingabe

   Primitive Daten (Dreiecke, Linien und/oder Punkte) aus vom Benutzer gefüllten Puffer im Arbeitsspeicher. Und möglicherweise Daten. Ein Dreieck wäre 3 Scheitel Punkte für jedes Dreieck und möglicherweise 3 Scheitel Punkte für die Daten der Daten pro Dreieck.

- Ausgabe

   Primitive mit vom System generierten Werten (z. b. eine primitive ID, eine Instanz-ID oder eine Scheitelpunkt-ID).

## <a name="vertex-shader-stage"></a>Vertex-Shader-Phase

Die [Vertex-Shader (VS)-Phase verarbeitet Scheitel](vertex-shader-stage--vs-.md) Punkte, die in der Regel Vorgänge wie Transformationen, das Skinning und die Beleuchtung ausführen. Ein Vertex-Shader nimmt einen einzelnen Eingabe Scheitelpunkt und erzeugt einen einzelnen Ausgabe Scheitelpunkt. Einzelne Vorgänge pro Scheitelpunkt (z. b. Transformationen, Skinning, morphung und pro-Vertex-Beleuchtung).

- Eingabe

   Ein einzelner Scheitelpunkt mit vom System generierten Werten "vertexid" und "InstanceId". Jeder Vertex-Shader-Eingabe Scheitelpunkt kann aus bis zu 16 32-Bit-Vektoren bestehen (jeweils bis zu 4 Komponenten).

- Ausgabe

   Ein einzelner Scheitelpunkt. Jeder Ausgabe Scheitelpunkt kann aus bis zu 16 32-Bit-vier-Komponenten-Vektoren bestehen.

## <a name="hull-shader-stage"></a>Hull-Shader-Phase

Die [Hull-Shader-Phase (HS)](hull-shader-stage--hs-.md) ist eine der Mosaik Stufen, die eine einzelne Oberfläche eines Modells effizient in viele Dreiecke zerlegen. Ein Hull-Shader wird einmal pro Patch aufgerufen und transformiert Eingabe Steuerungs Punkte, die eine niedrige Ordnung definieren, in Steuerungs Punkte, die einen Patch bilden. Außerdem werden einige pro-Patch-Berechnungen durchführt, um Daten für die Mosaik Stufe (TS) und die Domänen-Shader (DS) bereitzustellen.

- Eingabe

   Zwischen 1 und 32 Eingabe Steuerungs Punkten, die gemeinsam eine Oberfläche für eine niedrige Ordnung definieren.

- Ausgabe

   Zwischen 1 und 32 Ausgabe Steuerungs Punkten, die gemeinsam einen Patch bilden. Der Hull-Shader deklariert den Zustand für die Mosaik Stufe (TS), einschließlich der Anzahl der Kontrollpunkte, des Typs der Patchfläche und des partitionierungstyps, der beim Mosaik Prozess verwendet werden soll.

## <a name="tessellator-stage"></a>Mosaik Phase

Die Mosaik [Phase (TS)](tessellator-stage--ts-.md) erstellt ein Samplings-Muster der Domäne, die den Geometry-Patch darstellt, und generiert einen Satz kleinerer Objekte (Dreiecke, Punkte oder Linien), die diese Beispiele verbinden.

- Eingabe

   Der Mosaik Prozess arbeitet einmal pro Patch mit den Mosaik Faktoren (die angeben, wie fein die Domäne verarbeitet werden soll) und dem Partitionierungstyp (der den Algorithmus angibt, der zum Aufteilen eines Patches verwendet wird), der aus der Hülle-Shader-Stufe übergangen wird.

- Ausgabe

   Der Mosaik Prozess gibt UV-Koordinaten (und optional w) und die Oberflächen Topologie für die Domäne-Shader-Phase aus.

## <a name="domain-shader-stage"></a>Domäne-Shader-Phase

In der [Domänen-Shader-Phase (DS)](domain-shader-stage--ds-.md) wird die Scheitelpunkt Position eines unterteilten Punkts im Ausgabe Patch berechnet. die Vertex-Position, die den einzelnen Domänen Beispielen entspricht, wird berechnet. Ein Domänen-Shader wird einmal pro Tess-Phasen-Ausgabepunkt ausgeführt und hat schreibgeschützten Zugriff auf den Hull-Shader-Ausgabe Patch und die Ausgabe Patch-Konstanten, und die Mosaik Phase gibt die UV-Koordinaten an.

- Eingabe

   Ein Domänen-Shader nutzt Ausgabe Steuerungs Punkte aus der [Hull-Shader-Phase (HS)](hull-shader-stage--hs-.md). Die Hull-shaderausgaben umfassen Folgendes: Steuerungs Punkte, Patch-Konstante Daten und Mosaik Faktoren (die Mosaik Faktoren können die Werte enthalten, die vom Mosaik der fixierten Funktion sowie die unformatierten Werte verwendet werden, bevor Sie durch ein ganzzahliges Mosaik abgerundet werden, das beispielsweise die geomsche Verzierung vereinfacht). Ein Domänen-Shader wird einmal pro Ausgabe Koordinate aus der Mosaik [Phase (TS)](tessellator-stage--ts-.md)aufgerufen.

- Ausgabe

   Die Domänen-Shader-Phase (DS) gibt die Scheitelpunkt Position eines unterteilten Punkts im Ausgabe Patch aus.

## <a name="geometry-shader-stage"></a>Geometry-Shader-Phase

Die [Geometrie-Shader-Phase (GS)](geometry-shader-stage--gs-.md) verarbeitet vollständige primitive: Dreiecke, Linien und Punkte, zusammen mit ihren angrenzenden Scheitel Punkten. Es unterstützt die Geometrie Verstärkung und die Aufhebung der deverstärkung. Sie eignet sich für Algorithmen wie die Punkt Sprite-Erweiterung, dynamische Partikelsysteme, die Erstellung von samplingvolumes, die Generierung von schattenvolumes, das Rendering von einzelnen Pass-to-cubemap-Dateien, das Austauschen von Per-Primitive Material und das Einrichten von Per-Primitive Material, einschließlich der Generierung von baryzentrierten Koordinaten als primitive Daten.

- Eingabe

   Anders als bei Vertex-Shadern, die auf einem einzelnen Scheitelpunkt arbeiten, sind die Eingaben des Geometry-Shaders die Scheitel Punkte für einen vollständigen primitiven (drei Scheitel Punkte für Dreiecke, zwei Scheitel Punkte für Linien oder einzelner Scheitelpunkt für Punkt).

- Ausgabe

   Die Geometrie-Shader-Phase (GS) kann mehrere Scheitel Punkte ausstellen, die eine einzelne ausgewählte Topologie bilden. Verfügbare Geometry-Shader-ausgabetopologien sind **tristrip**, **linestrip** und **PointList**. Die Anzahl der ausgegebenen primitiven kann in jedem Aufruf des Geometry-Shaders frei variieren, obwohl die maximale Anzahl von Scheitel Punkten, die ausgegeben werden konnten, statisch deklariert werden muss. Von einem Geometry-Shader-Aufruf ausgegebene Bereichs Längen können beliebig sein, und neue Bänder können über die Funktion " [restartstrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL" erstellt werden.

## <a name="stream-output-stage"></a>Stream-Ausgabe Phase

Die Daten [Stromausgabe (so)](stream-output-stage--so-.md) gibt Vertex-Daten aus der vorherigen aktiven Phase kontinuierlich aus (oder streamt Sie an einen oder mehrere Speicherpuffer). Daten, die in den Arbeitsspeicher gestreamt werden, können als Eingabedaten zurück in die Pipeline oder aus der CPU zurückgeleitet werden.

- Eingabe

   Vertex-Daten aus einer vorherigen Pipeline Phase.

- Ausgabe

   Die "Stream Output (so)"-Phase gibt Vertex-Daten aus der vorherigen aktiven Phase, wie z. b. der Geometrie-Shader-Phase (GS), kontinuierlich aus. Wenn die Geometrie-Shader-Stufe (GS) inaktiv ist und die Datenstrom Ausgabe (so) aktiv ist, gibt Sie kontinuierlich Vertex-Daten aus der Domänen-Shader-Phase (DS) an Puffer im Speicher aus (oder, wenn die DS auch inaktiv ist, von der Vertex-Shader-Phase (VS)).

## <a name="rasterizer-stage"></a>Rasterizer-Phase

Die [Rasterizer (RS)-Phase](rasterizer-stage--rs-.md) schneidet Grundtypen ab, die nicht in der Ansicht angezeigt werden, bereitet primitive für die Stufe Pixel Shader (PS) vor und bestimmt, wie Pixel-Shader aufgerufen werden. Konvertiert Vektor Informationen (bestehend aus Formen oder primitiven) in ein Rasterbild (bestehend aus Pixeln), um Echt Zeit 3D-Grafiken anzuzeigen.

- Eingabe

   Bei Scheitel Punkten (x, y, z, w), die in die Rasterizer-Phase kommen, wird davon ausgegangen, dass es sich um einen homogenen Clip Bereich handelt. In diesem Koordinaten Bereich zeigt die X-Achse nach rechts, Y zeigt nach oben und Z Punkte von der Kamera.

- Ausgabe

   Die tatsächlichen Pixel, die gerendert werden müssen. Umfasst einige Vertex-Attribute für die Interpolation durch den Pixelshader.

## <a name="pixel-shader-stage"></a>Pixel-Shader-Phase

Die [Pixel-Shader-Stufe (PS)](pixel-shader-stage--ps-.md) empfängt interpoliert Daten für einen primitiven und generiert Pixel-Daten wie z. b. Color.

- Eingabe

   Wenn die Pipeline ohne einen Geometry-Shader konfiguriert ist, ist ein Pixelshader auf 16, 32-Bit, 4-komponenteneingaben beschränkt. Andernfalls kann ein Pixelshader bis zu 32, 32-Bit, 4-komponenteneingaben annehmen. Die Pixel-Shader-Eingabedaten enthalten Scheitelpunkt Attribute (die mit oder ohne Perspektiven Korrektur interpoliert werden können) oder als primitive Konstanten behandelt werden können. Pixel-shadereingaben werden aus den Vertex-Attributen des primitiven, das auf der Grundlage des deklarierten Interpolations Modus basiert, interpoliert. Wenn ein primitiver vor der rasterisierung abgeschnitten wird, wird der Interpolations Modus während des clippingprozesses ebenfalls berücksichtigt.

- Ausgabe

   Ein Pixelshader kann bis zu 8, 32-Bit, 4-Komponentenfarben oder keine Farbe ausgeben, wenn das Pixel verworfen wird. Die Pixel-Shader-Ausgabe Register Komponenten müssen deklariert werden, bevor Sie verwendet werden können. jedem Register ist eine eindeutige Ausgabe-/Schreibmaske gestattet.

## <a name="output-merger-stage"></a>Ausgabe Zusammenführungs Phase

Die [Phase Output Merger (OM)](output-merger-stage--om-.md) kombiniert verschiedene Typen von Ausgabedaten (Pixel-Shader-Werte, tiefen-und Schablonen Informationen) mit dem Inhalt des Renderziels und der tiefen/Schablonen Puffer, um das abschließende Pipeline Ergebnis zu generieren.

- Eingabe

   Ausgabe Zusammenführungs Eingaben sind der Pipeline Status, die Pixeldaten, die von den Pixel-Shadern generiert werden, der Inhalt der Renderziele und der Inhalt der tiefen/Schablonen Puffer.

- Ausgabe

   Die endgültige gerenderte Pixelfarbe.

## <a name="related-topics"></a>Zugehörige Themen

- [Direct3D Grafik Lernhandbuch](index.md)

- [Berechnen der Pipeline](compute-pipeline.md)