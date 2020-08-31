---
title: Tessellatorphase (TS)
description: Die Mosaik Phase (TS) erstellt ein Samplings-Muster der Domäne, die den Geometry-Patch darstellt, und generiert einen Satz kleinerer Objekte (Dreiecke, Punkte oder Linien), die diese Beispiele verbinden.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- Tessellatorphase (TS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 90dfb8d28be786cb542e72fde5a24bed4de68f78
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167974"
---
# <a name="tessellator-ts-stage"></a>Tessellatorphase (TS)


Die Mosaik Phase (TS) erstellt ein Samplings-Muster der Domäne, die den Geometry-Patch darstellt, und generiert einen Satz kleinerer Objekte (Dreiecke, Punkte oder Linien), die diese Beispiele verbinden.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


Im folgenden Diagramm werden die Phasen der Direct3D-Grafik Pipeline hervorgehoben.

![Diagramm der Direct3D 11-Pipeline, mit der die Stufen "Hull-Shader", "Mosaik Bereich" und "Domänen-Shader" hervorgehoben werden](images/d3d11-pipeline-stages-tessellation.png)

Das folgende Diagramm zeigt den Fortschritt durch die Mosaik Phasen.

![Diagramm der Mosaik Entwicklung](images/tess-prog.png)

Der Fortschritt beginnt mit der Oberfläche für die Unterteilung mit niedriger Detailgenauigkeit. Im nächsten Schritt wird der Eingabe Patch mit dem entsprechenden Geometry-Patch, den Domänen Beispielen und den Dreiecken hervorgehoben, mit denen diese Beispiele verbunden werden. Der Fortschritt verdeutlicht schließlich die Scheitel Punkte, die diesen Beispielen entsprechen.

Die Direct3D-Laufzeit unterstützt drei Phasen, die Mosaik Elemente implementieren, mit denen untergeordnete Oberflächen mit niedriger Detailgenauigkeit auf der GPU in mehr Detail primitive konvertiert werden. Mosaik Kacheln (oder unterbrechen) hoch geordnete Oberflächen in geeignete Strukturen zum Rendern.

Die Mosaik Stufen arbeiten zusammen, um eine höhere Reihenfolge von Oberflächen (die das Modell einfach und effizient halten) auf viele Dreiecke zu konvertieren, um das ausführliche Rendering innerhalb der Direct3D-Grafik Pipeline zu erhalten.

Das Mosaik verwendet die GPU, um eine ausführlichere Oberfläche aus einer Oberfläche zu berechnen, die aus Quad-Patches, Dreiecks Patches oder Isolationen erstellt wird. Um der hoch geordneten Oberfläche zu nähern, wird jeder Patch mithilfe von Mosaik Faktoren in Dreiecke, Punkte oder Zeilen unterteilt. Die Direct3D-Grafik Pipeline implementiert Mosaik mithilfe von drei Pipeline Stufen:

-   [Hull-Shader-Phase (HS)](hull-shader-stage--hs-.md) : eine programmierbare Shader-Stufe, die einen Geometry-Patch (und Patch-Konstanten) erzeugt, die den einzelnen Eingabe Patches (Quad, Dreieck oder Line) entsprechen.
-   Tessellator (TS)-Phase: eine Pipeline Phase mit fester Funktionsweise, die ein Samplings-Muster der Domäne erstellt, das den Geometry-Patch darstellt, und einen Satz kleinerer Objekte (Dreiecke, Punkte oder Linien) generiert, die diese Beispiele verbinden.
-   [Domänen-Shader (DS)-Phase](domain-shader-stage--ds-.md) : eine programmierbare Shader-Stufe, die die Vertex-Position berechnet, die den einzelnen Domänen Beispielen entspricht.

Durch die Implementierung von Mosaik in Hardware kann eine Grafik Pipeline niedrigere Detailmodelle auswerten (niedrigere Polygon Anzahl) und die Darstellung ausführlicher darstellen. Obwohl das Software Mosaik durchgeführt werden kann, kann durch Hardware implementiertes Mosaik eine unglaubliche Menge an visuellen Details generieren (einschließlich der Unterstützung für die Verschiebungs Zuordnung), ohne die visuellen Details zu den Modellgrößen hinzuzufügen und die Aktualisierungs Raten zu erhöhen.

Vorteile von Mosaik:

-   Das Mosaik spart viel Arbeitsspeicher und Bandbreite, sodass eine Anwendung detailliertere Oberflächen aus Modellen mit niedriger Auflösung darstellen kann. Das Mosaik Verfahren, das in der Direct3D-Grafik Pipeline implementiert ist, unterstützt auch die Verschiebungs Zuordnung, die beeindruckende Mengen von Oberflächendetails liefern kann.
-   Das Mosaik unterstützt skalierbare Renderingverfahren, z. b. fortlaufende oder Ansichts abhängige Detailebenen, die im Handumdrehen berechnet werden können.
-   Das Mosaik erhöht die Leistung, indem teure Berechnungen mit niedrigerer Häufigkeit durchgeführt werden (Berechnungen in einem Modell mit niedrigerer Genauigkeit). Dies kann das Kombinieren von Berechnungen mithilfe von Blend-Formen oder Morph-Zielen für realistische Animationen oder Physik Berechnungen für die Konflikterkennung oder die Dynamik des Soft Texts einschließen.

Die Direct3D-Grafik Pipeline implementiert das Mosaik in Hardware, das die Arbeit von der CPU auf die GPU auslädt. Dies kann zu sehr großen Leistungsverbesserungen führen, wenn eine Anwendung eine große Anzahl von Morph-Zielen und/oder komplexeren Modelle für das Skinning/-debuggingmodell implementiert.

Der Mosaik Prozess ist eine Stufe mit fester Funktionsweise, die durch das Binden eines [Hull-Shaders](hull-shader-stage--hs-.md) an die Pipeline initialisiert wird. (Weitere Informationen finden [Sie unter Gewusst wie: Initialisieren der Mosaik Stufe](/windows/desktop/direct3d11/direct3d-11-advanced-stages-tessellator-initialize)). Der Zweck der Mosaik Stufe besteht darin, eine Domäne (Quad, Tri oder Line) in viele kleinere Objekte (Dreiecke, Punkte oder Linien) zu unterteilen. Der Mosaik-Mosaik Kachel eine kanonische Domäne in einem normalisierten (null-zu-eins-) Koordinatensystem. Beispielsweise wird eine Quad-Domäne zu einem Einheits Quadrat.

### <a name="span-idphases_in_the_tessellator__ts__stagespanspan-idphases_in_the_tessellator__ts__stagespanspan-idphases_in_the_tessellator__ts__stagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>Phasen in der Mosaik Phase (TS)

Die Mosaik Phase (TS) funktioniert in zwei Phasen:

-   In der ersten Phase werden die Mosaik Faktoren verarbeitet, Rundungs Probleme behoben, die Verarbeitung sehr kleiner Faktoren, das reduzieren und Kombinieren von Faktoren mithilfe der 32-Bit-Gleit Komma Arithmetik durchgesetzt.
-   In der zweiten Phase werden Punkt-oder topologielisten basierend auf dem ausgewählten Partitionierungstyp generiert. Dabei handelt es sich um die Hauptaufgabe der Mosaik Phase, die 16-Bit-Bruchzahlen mit fest Komma-Arithmetik verwendet. Mit fester Punkt Arithmetik wird die Hardwarebeschleunigung unter Beibehaltung zulässiger Genauigkeit ermöglicht. Bei einem 64-Meter weiten Patch kann diese Genauigkeit z. b. Punkte bei einer Auflösung von 2 mm platzieren.

    | Partitionierungstyp | Range                       |
    |----------------------|-----------------------------|
    | \_Ungerade Bruchzahlen      | \[1... 63\]                  |
    | Sogar Bruchteile \_     | Mosaik Faktor Bereich: \[ 2.. 64\] |
    | Integer              | Mosaik Faktor Bereich: \[ 1.. 64\] |
    | Pow2                 | Mosaik Faktor Bereich: \[ 1.. 64\] |

     

Das Mosaik ist mit zwei programmierbaren shaderphasen implementiert: ein [Hull-Shader](hull-shader-stage--hs-.md) und ein [Domänen-Shader](domain-shader-stage--ds-.md). Diese Shader-Stufen werden mit dem in Shader-Modell 5 definierten HLSL-Code programmiert. Die Shader-Ziele lauten: HS \_ 5 \_ 0 und DS \_ 5 \_ 0. Der Titel erstellt den Shader. Anschließend wird der Code für die Hardware aus kompilierten Shadern extrahiert, die an die Laufzeit weitergeleitet werden, wenn Shader an die Pipeline gebunden sind.

### <a name="span-idenabling_disabling_tessellationspanspan-idenabling_disabling_tessellationspanspan-idenabling_disabling_tessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>Aktivieren/Deaktivieren von Mosaik

Aktivieren Sie das Mosaik, indem Sie einen Hull-Shader erstellen und ihn an die Hülle-Shader-Stufe binden (Dadurch wird die Mosaik Phase automatisch eingerichtet). Um die abschließenden Vertex-Positionen aus den Mosaik Patches zu generieren, müssen Sie auch einen [Domänen-Shader](domain-shader-stage--ds-.md) erstellen und an die Domäne-Shader-Stufe binden. Sobald das Mosaik aktiviert ist, muss die Dateneingabe für die Eingabe Assembler-Stufe (IA) Patchdaten sein. Bei der Eingabe-Assembler-Topologie muss es sich um eine patchkonstante Topologie handeln.

Um das Mosaik zu deaktivieren, legen Sie den Hull-Shader und den Domänen-Shader auf **null**fest. Weder der [Geometrie-Shader-Phase](geometry-shader-stage--gs-.md) noch der [Stream-Ausgabe (so)-Phase](stream-output-stage--so-.md) kann Hull-Shader-Ausgabe Steuerungs Punkte oder Patchdaten lesen.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Der


Der Mosaik Prozess arbeitet einmal pro Patch mit den Mosaik Faktoren (die angeben, wie fein die Domäne verarbeitet werden soll) und dem Partitionierungstyp (der den Algorithmus angibt, der zum Aufteilen eines Patches verwendet wird), der aus der Hülle-Shader-Stufe übergangen wird.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgeben


Der Mosaik Prozess gibt UV-Koordinaten (und optional w) und die Oberflächen Topologie für die Domäne-Shader-Phase aus.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 