---
title: Konfigurieren der Tiefenschablonenfunktionalität
description: In diesem Abschnitt werden die Schritte zum Einrichten des tiefen Schablone-Puffers und der Status der tiefen Schablone für die Ausgabe-Fusion-Phase behandelt.
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- Konfigurieren der Tiefenschablonenfunktionalität
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2d9c33e9625f36bbf183df46d5cd590f9e66d3c4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162804"
---
# <a name="span-iddirect3dconceptsconfiguring_depth-stencil_functionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>Konfigurieren der Tiefenschablonenfunktionalität


In diesem Abschnitt werden die Schritte zum Einrichten des tiefen Schablone-Puffers und der Status der tiefen Schablone für die Ausgabe-Fusion-Phase behandelt.

Wenn Sie wissen, wie Sie den tiefen Schablone-Puffer und den entsprechenden Status der tiefen Schablone verwenden, finden Sie weitere Informationen unter [Advanced-Stencil-Techniken](#advanced-stencil-techniques).

## <a name="span-idcreate_depth_stencil_statespanspan-idcreate_depth_stencil_statespanspan-idcreate_depth_stencil_statespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>Status der tiefen Schablone erstellen


Der Status der tiefen Schablone teilt der Ausgabe-Fusion-Phase mit, wie der [tiefen Schablonen Test](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)durchgeführt werden soll. Der tiefen Schablone-Test bestimmt, ob ein bestimmtes Pixel gezeichnet werden soll.

## <a name="span-idbind_depth_stencil_to_the_om_stagespanspan-idbind_depth_stencil_to_the_om_stagespanspan-idbind_depth_stencil_to_the_om_stagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>Binden von tiefen Schablonen Daten an die OM-Phase


Binden Sie den Status der tiefen Schablone.

Binden Sie die Tiefe der tiefen Schablone mithilfe einer Ansicht.

Renderziele müssen alle den gleichen Ressourcentyp aufweisen. Wenn Multisampling-Antialiasing verwendet wird, müssen alle gebundenen Renderingziele und tiefen Puffer dieselbe Stichproben Anzahl aufweisen.

Wenn ein Puffer als Renderziel verwendet wird, werden tiefen Schablonen Tests und mehrere Renderingziele nicht unterstützt.

-   Bis zu 8 Renderziele können gleichzeitig gebunden werden.
-   Alle Renderziele müssen in allen Dimensionen (Breite und Höhe) und Tiefe für die 3D-oder Array Größe für Array Typen die gleiche Größe aufweisen \* .
-   Jedes Renderziel weist möglicherweise ein anderes Datenformat auf.
-   Schreib Masken steuern, welche Daten in ein Renderziel geschrieben werden. Mit dem Ausgabe Schreibvorgang werden Masken Steuerelemente für ein pro-Renderziel und jede Komponentenebene geschrieben, welche Daten in die Renderziele geschrieben werden.

## <a name="span-idadvanced_stencil_techniquesspanspan-idadvanced_stencil_techniquesspanspan-idadvanced_stencil_techniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>Erweiterte Schablonen Techniken


Der Schablone-Teil des tiefen Schablonen Puffers kann zum Erstellen von renderingeffekten wie zusammensetzen, herabstufen und Gliederung verwendet werden.

-   [Zusammensetzung](#compositing)
-   [Wird abgebrochen](#decaling)
-   [Kontur und Silhouetten](#outlines-and-silhouettes)
-   [Zweiseitige Schablone](#two-sided-stencil)
-   [Lesen des tiefen Schablonen Puffers als Textur](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Zusammensetzung

Die Anwendung kann mit dem Schablonen Puffer 2D-oder 3D-Bilder in eine 3D-Szene zusammensetzen. Eine Maske im Schablone-Puffer wird verwendet, um einen Bereich der Renderingzieloberfläche zu okzieren. Gespeicherte 2D-Informationen, wie z. b. Text oder Bitmaps, können dann in den Bereich "okded" geschrieben werden. Alternativ kann die Anwendung zusätzliche 3D-primitive in den mit der Schablone maskierten Bereich der Renderingzieloberfläche rendern. Sie kann sogar eine ganze Szene darstellen.

Spiele sind häufig mehrere 3D-Szenen zusammengesetzt. Beispielsweise wird bei Fahr spielen in der Regel eine Spiegelung der Rückseite angezeigt. Die Spiegel Datenbank enthält die Ansicht der 3D-Szene hinter dem Treiber. Es handelt sich im Wesentlichen um eine zweite 3D-Szene, die mit der vorwärts Ansicht des Treibers zusammengesetzt wird

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Wird abgebrochen

Direct3D-Anwendungen verwenden die-Verwendung, um zu steuern, welche Pixel von einem bestimmten primitiven Bild auf die Renderingzieloberfläche gezeichnet werden. Anwendungen wenden die Abbilder von primitiven an, damit Coplanar-Polygone ordnungsgemäß wieder hergestellt werden können.

Wenn beispielsweise Reifen Markierungen und gelbe Linien auf eine Straßenebene angewendet werden, sollten die Markierungen direkt auf der Straße angezeigt werden. Die z-Werte der Markierungen und der Straße sind jedoch identisch. Daher erzeugt der tiefen Puffer möglicherweise keine saubere Trennung zwischen den beiden. Einige Pixel im Hintergrundtyp können über dem Front-primitiv und umgekehrt gerendert werden. Das resultierende Bild wird angezeigt, um von Frame zu Rahmen zu vershicheln. Dieser Effekt heißt z-Fighting oder Flimmern.

Um dieses Problem zu beheben, verwenden Sie eine Schablone, um den Abschnitt des hintergrundprimitivs zu maskieren, in dem die Client Zugriffslizenz angezeigt wird. Deaktivieren Sie die z-Pufferung, und Renderern Sie das Bild des Front-Primitivs in den maskierten Bereich der Renderziel-Oberfläche.

Zum lösen dieses Problems können mehrere Textur Mischungs Möglichkeiten verwendet werden.

### <a name="span-idoutlines_and_silhouettesspanspan-idoutlines_and_silhouettesspanspan-idoutlines_and_silhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">Kontur und Silhouetten

Sie können den Schablonen Puffer für abstraktere Effekte verwenden, z. b. Gliederung und Silhouette.

Wenn die Anwendung zwei Renderungs Ergebnisse übergibt: eine zum Generieren der Schablonen Maske und eine zweite, um die Schablonen Maske auf das Bild anzuwenden, aber mit den primitiven, die im zweiten Durchlauf etwas kleiner sind, enthält das resultierende Bild nur den Umriss des primitiven. Die Anwendung kann dann den Bereich der Schablone maskiert mit einer voll Tonfarbe ausfüllen, wodurch dem primitiven ein geprägte Bild angezeigt wird.

Wenn die Schablone-Maske dieselbe Größe und Form wie das primitive-Format aufweist, das Sie rendern, enthält das resultierende Bild eine Lücke, in der der primitive entsprechen sollte. Die Anwendung kann dann das Loch mit Black auffüllen, um eine Silhouette des primitiven zu schaffen.

### <a name="span-idtwo_sided_stencilspanspan-idtwo_sided_stencilspanspan-idtwo_sided_stencilspanspan-idtwo-sided-stenciltwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span><span id="two-sided-stencil">Zweiseitige Schablone

Schattenvolumes werden zum Zeichnen von Schatten mit dem Schablonen Puffer verwendet. Die Anwendung berechnet die schattenvolumes, die durch die Schattierung von Geometrie umgewandelt werden, indem die Silhouette-Kanten berechnet und vom Licht in eine Gruppe von 3D-Volumes aufgeteilt werden. Diese Volumes werden dann zweimal im Schablonen Puffer gerendert.

Das erste Rendering zeichnet nach vorne gerichtete Polygone und erhöht die Schablonen Puffer Werte. Das zweite Rendering zeichnet die rückwärts gerichteten Polygone des Schatten Volumens und Dekremente die Schablonen Puffer Werte.

Normalerweise brechen alle inkrementierten und dekrementierten Werte einander ab. Die Szene wurde jedoch bereits mit normaler Geometrie gerendert, sodass einige Pixel den z-Puffer-Test nicht bestanden haben, während das Schatten Volume gerendert wird. Werte, die im Schablonen Puffer verbleiben, entsprechen den im Schatten entsprechenden Pixeln. Diese verbleibenden Schablone-Puffer Inhalte werden als Maske verwendet, um einen großen, umfassenden, vier Vierfache in die Szene zu mischen. Wenn der Schablone-Puffer als Maske fungiert, besteht das Ergebnis darin, die Pixel in den Schatten zu verbergen.

Dies bedeutet, dass die Schatten Geometrie zweimal pro Lichtquelle gezeichnet wird und somit den Scheitelpunkt Durchsatz der GPU erhöht. Das zweiseitige Schablonen Feature wurde entwickelt, um diese Situation zu mindern. Bei dieser Vorgehensweise gibt es zwei Sätze von Schablone State (unten genannt), jeweils eine Menge für die Vorder-und die andere für die mit der Rückseite gerichteten Dreiecke. Auf diese Weise wird nur ein einzelner Durchlauf pro Schatten Volume pro Licht gezeichnet.

### <a name="span-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>Lesen des tiefen Schablonen Puffers als Textur

Ein inaktiver tiefen Schablone-Puffer kann von einem Shader als Textur gelesen werden. Eine Anwendung, die einen tiefen Schablone-Puffer liest, wenn eine Textur in zwei Durchläufen gerendert wird, wird der erste Pass in den tiefen Schablone-Puffer geschrieben, und der zweite Durchlauf liest aus dem Puffer. Dadurch kann ein Shader Tiefe-oder Schablonen Werte, die zuvor in den Puffer geschrieben wurden, mit dem Wert für das Pixel vergleichen, das in der Vergangenheit gerendert wird. Das Ergebnis des Vergleichs kann verwendet werden, um Effekte wie z. b. eine Schatten Zuordnung oder weiche Partikel in einem Partikelsystem zu erstellen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ausgabezusammenführungsphase (OM)](output-merger-stage--om-.md)

[Grafikpipeline](graphics-pipeline.md)

[Ausgabe-Fusion-Phase](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)