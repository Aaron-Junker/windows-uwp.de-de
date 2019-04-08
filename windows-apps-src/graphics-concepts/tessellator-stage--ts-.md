---
title: Tessellatorphase (TS)
description: Die Tessellatorphase (TS) erstellt ein Samplingmuster der Domäne, das den Geometriepatch darstellt und eine Reihe kleinerer Objekte (Dreiecke, Punkte oder Linien) generiert, die diese Samplings verbinden.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- Tessellatorphase (TS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7768d63405281d3155affc6c9f09c62568761718
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607395"
---
# <a name="tessellator-ts-stage"></a>Tessellatorphase (TS)


Die Tessellatorphase (TS) erstellt ein Samplingmuster der Domäne, das den Geometriepatch darstellt und eine Reihe kleinerer Objekte (Dreiecke, Punkte oder Linien) generiert, die diese Samplings verbinden.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


Das folgende Diagramm zeigt die Phasen der Direct3D-Grafikpipeline.

![Diagramm der Direct3D 11-Pipeline, das die Phasen Hüllen-Shader, Tessellator und Domänen-Shader zeigt.](images/d3d11-pipeline-stages-tessellation.png)

Das folgende Diagramm zeigt die einzelnen Tessellationsphasen.

![Diagramm der Tessellationsphasen](images/tess-prog.png)

Der Verlauf beginnt mit der Oberfläche der detailarmen Teilfläche. Als Nächstes wird der Eingabepatch mit dem entsprechenden Geometriepatch, den Domänensamplings und den Dreiecken, die diese Samplings verbinden, hervorgehoben. Schließlich werden die Vertices, die diesen Samplings entsprechen, hervorgehoben.

Die Direct3D-Laufzeit unterstützt drei Phasen, die die Tessellation implementieren, bei der detailarme Teilflächen in detailreichere Grundtypen auf der GPU konvertiert werden. Bei der Tessellation werden Oberflächen einer höheren Ordnung in geeignete Strukturen für das Rendering aufgeteilt (bzw. aufgebrochen).

Die Tessellationsphasen arbeiten zusammen, um Oberflächen einer höheren Ordnung (die das Modell einfach und effizient halten) in viele Dreiecke für das detaillierte Rendern in der Direct3D-Grafikpipeline zu konvertieren.

Tessellation berechnet mit der GPU eine detailreichere Oberfläche aus einer aus viereckigen Patches, dreieckigen Patches oder Isolinien erstellten Oberfläche. Zur Annäherung an die Oberfläche der höheren Ordnung wird jeder Patch unter Verwendung der Tessellationsfaktoren in Dreiecke, Punkte oder Linien unterteilt. Die Direct3D-Grafikpipeline implementiert die Tessellation mithilfe von drei Pipelinephasen:

-   [Hüllen-Shaderphase (HS)](hull-shader-stage--hs-.md) – Eine programmierbare Shaderphase, die einen Geometriepatch (und Patchkonstanten) für den jeweiligen Eingabepatch (Viereck, Dreieck oder Linie) erzeugt.
-   Die Tessellatorphase (TS) – Eine Pipeline mit fester Funktion, die ein Samplingmuster der Domäne erstellt, das den Geometriepatch darstellt und eine Reihe kleinerer Objekte (Dreiecke, Punkte oder Linien) erzeugt, die diese Samplings verbinden.
-   [Domänen-Shaderphase (DS)](domain-shader-stage--ds-.md) – Eine programmierbare Shaderphase, die die Vertexposition für das jeweilige Domänensampling berechnet.

Durch Implementierung der Tessellation in die Hardware kann eine Grafikpipeline detailärmere Modelle (mit einer geringeren Anzahl an Polygonen) auswerten und mit einer höheren Detailtiefe rendern. Im Gegensatz zu einer softwarebasierten Tessellation kann eine in die Hardware implementierte Tessellation eine unglaubliche Anzahl an visuellen Details generieren (einschließlich einer Unterstützung für Ersetzungszuordnung), ohne dass dabei den Modellgrößen visuelle Details hinzugefügt oder die Aktualisierungsraten beeinträchtigt werden müssten.

Vorteile bei der Tessellation:

-   Tessellation spart viel Speicher und Bandbreite, wodurch eine Anwendung höher detaillierte Oberflächen aus niedrigauflösenden Modellen rendern kann. Die in die Direct3D-Grafikpipeline implementierte Tessellationstechnik unterstützt auch die Ersetzungszuordnung, die beeindruckende Mengen von Oberflächendetails erzeugen kann.
-   Tessellation unterstützt skalierbare Renderingtechniken, z. B. kontinuierliche oder ansichtsabhängige Detailtiefen, die spontan berechnet werden können.
-   Tessellation verbessert die Leistung durch die Ausführung aufwendiger Berechnungen bei niedrigerer Frequenz (die Berechnungen erfolgen am detailärmeren Modell). Dies könnte Übergangsberechnungen mit Übergangsformen oder Morphzielen für realistische Animationen oder physikalische Berechnungen für Kollisionserkennung oder die Dynamik weicher Körper einschließen.

Die Direct3D-Grafikpipeline implementiert Tessellation in Hardware, wodurch die Arbeit von der CPU auf die GPU verlagert wird. Dies kann zu sehr großen Leistungsverbesserungen führen, wenn eine Anwendung eine große Anzahl von Morphzielen und/oder ausgeklügelten Skinning-/Deformationsmodellen implementiert.

Der Tessellator ist eine Phase mit fester Funktion, die durch Binden eines [Hüllen-Shaders](hull-shader-stage--hs-.md) an die Pipeline initialisiert wird. (finden Sie unter [so wird's gemacht: Initialisieren die Mosaikstufe](https://msdn.microsoft.com/library/windows/desktop/ff476341)). Die Aufgabe der Tessellatorphase besteht darin, eine Domäne (Viereck, Dreieck oder Linie) in viele kleinere Objekte (Dreiecke, Punkte oder Linien) zu unterteilen. Die Tessellator unterteilt eine kanonische Domäne in einem normalisierten (Null-zu-Eins) Koordinatensystem. Eine viereckige Domäne wird z. B. als Einheitsquadrat unterteilt (tesselliert).

### <a name="span-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>Phasen in der Phase Mosaikeingabefaktoren (TS)

Die Tessellatorphase (TS) arbeitet in zwei Phasen:

-   Die erste Phase verarbeitet die Tessellationsfaktoren, wobei mit der 32-Bit-Gleitkommaarithmetik Rundungsprobleme behoben, sehr kleine Faktoren behandelt, Faktoren reduziert und kombiniert werden.
-   Die zweite Phase generiert Punkt- oder Topologielisten auf Grundlage des ausgewählten Partitionierungstyps. Dies ist die Hauptaufgabe der Tessellatorphase, und hier werden 16-Bit-Bruchzahlen mit Festkommaarithmetik verwendet. Festpunktarithmetik ermöglicht Hardwarebeschleunigung bei gleichzeitiger Beibehaltung einer akzeptablen Genauigkeit. Beispielsweise können bei einem 64 Meter breiten Patch mit dieser Genauigkeit Punkte mit einer Auflösung von 2 mm platziert werden.

    | Partitionierungstyp | Bereich                       |
    |----------------------|-----------------------------|
    | Sekundenbruchteile\_ungerade      | \[1... 63\]                  |
    | Sekundenbruchteile\_sogar     | TessFactor Bereich: \[2..64\] |
    | Ganze Zahl              | TessFactor Bereich: \[1..64\] |
    | Pow2                 | TessFactor Bereich: \[1..64\] |

     

Tessellation wird mit zwei programmierbaren Shaderphasen implementiert: einem [Hüllen-Shader](hull-shader-stage--hs-.md) und einem [Domänen-Shader](domain-shader-stage--ds-.md). Diese Shaderphasen werden mit HLSL-Code programmiert, der im Shadermodell 5 definiert ist. Der Shader-Ziele sind: hs\_5\_0 und ds\_5\_0. Der Titel erstellt den Shader, dann wird der Code für die Hardware aus den kompilierten Shadern extrahiert und an die Laufzeit übergeben, wenn Shader an die Pipeline gebunden werden.

### <a name="span-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>Aktivieren/Deaktivieren der Tesselierung

Aktivieren Sie die Tessellation, indem Sie einen Hüllen-Shader erstellen und diesen an die Hüllen-Shaderphase binden (dadurch wird die Tessellatorphase automatisch eingerichtet). Um die abschließenden Vertexpositionen aus den tessellierten Patches zu generieren, müssen Sie auch einen [Domänen-Shader](domain-shader-stage--ds-.md) erstellen und diesen an die Domänen-Shaderphase binden. Sobald die Tessellation aktiviert ist, muss die Dateneingabe für die Eingabeassemblerphase (IA) aus Patchdaten bestehen. Die Eingabeassemblertopologie muss eine Patchkonstantentopologie sein.

Um die Tessellation zu deaktivieren, legen Sie den Hüllen-Shader und den Domänen-Shader auf **NULL** fest. Weder die [Geometrie-Shaderphase (GS)](geometry-shader-stage--gs-.md) noch die [Streamausgabephase (SO)](stream-output-stage--so-.md) kann Ausgabekontrollpunkte des Hüllen-Shaders oder Patchdaten lesen.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


Der Tessellator arbeitet einmal pro Patch und verwendet dabei die Tessellationsfaktoren (die angeben, wie detailliert die Domäne tesselliert wird) und den Partitionierungstyp (der den Algorithmus angibt, mit dem ein Patch unterteilt wird), die von der Hüllen-Shaderphase übergeben werden.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


Die Tessellator gibt UV-Koordinaten (und optional W-Koordinaten) und die Oberflächentopologie an die Domänen-Shaderphase aus.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grafikpipeline](graphics-pipeline.md)

 

 




