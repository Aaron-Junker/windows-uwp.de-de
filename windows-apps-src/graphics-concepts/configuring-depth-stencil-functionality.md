---
title: Konfigurieren der Tiefenschablonenfunktionalität
description: In diesem Abschnitt werden die Schritte zum Einrichten des Tiefenschablonenpuffers und der Tiefenschablonenphase für die Ausgabezusammenführungsphase behandelt.
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- Konfigurieren der Tiefenschablonenfunktionalität
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 98cb6c62248fbf273a9d7ca1ef0d1d82293122eb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656185"
---
# <a name="span-iddirect3dconceptsconfiguringdepth-stencilfunctionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>Konfigurieren der Funktionalität der tiefenschablone


In diesem Abschnitt werden die Schritte zum Einrichten des Tiefenschablonenpuffers und der Tiefenschablonenphase für die Ausgabezusammenführungsphase behandelt.

Wenn Sie mit dem Tiefenschablonenpuffer und der entsprechenden Tiefenschablonenphase umgehen können, finden Sie weitere Informationen unter [erweiterte Schablonen-Techniken](#advanced-stencil-techniques).

## <a name="span-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>Status der Tiefenschablone erstellen


Die Tiefenschablonenphase zeigt den Zustand der Ausgabezusammenführungsphase zum Ausführen des [Tiefenschablonen-Test](https://msdn.microsoft.com/library/windows/desktop/bb205120) an. Der Tiefenschablonen-Test bestimmt, ob eine gegebene Pixel gezeichnet werden soll oder nicht.

## <a name="span-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>Tiefenschablone Datenbindung der OM-Phase


Binden der Tiefenschablonenphase

Binden der Tiefenschablonen-Ressource mithilfe der Ansicht.

Renderziele müssen alle den gleichen Ressourcentyp verwenden. Wenn Multisampling-Antialiasing verwendet wird, müssen alle gebundenen Renderziele und Tiefenpuffer über die gleiche Beispielanzahl verfügen.

Wenn ein Puffer als Renderziel verwendet wird, werden Tiefenschablonen-Tests und mehrere Renderziele nicht unterstützt.

-   Es können bis zu 8 Renderziele gleichzeitig gebunden werden.
-   Allen renderzielen müssen dieselbe Größe aufweisen, in allen Dimensionen (Breite und Höhe und Tiefe für 3D oder Array-Größe für \*Arraytypen).
-   Jedes Renderziel kann ein anderes Datenformat haben.
-   Schreib-Masken überwachen, welche Daten in ein Renderziel geschrieben werden. Die Ausgabe-Schreib-Maske überwacht pro Renderziel und pro Komponentenebene, welche Daten in ein Renderziel geschrieben werden.

## <a name="span-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>Erweiterte Schablone Techniken


Der Schablonen-Anteil des Tiefenschablonenpuffers kann zur Erstellung von Rendering-Effekten, wie Zusammensetzen, Decals und Gliedern verwendet werden.

-   [Zusammensetzung](#compositing)
-   [Decaling](#decaling)
-   [Beschreibt und Silhouetten](#outlines-and-silhouettes)
-   [Zweiseitigen Schablone](#two-sided-stencil)
-   [Beim Lesen des Tiefenschablone Puffers als Textur](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Zusammensetzung

Ihre Anwendung kann die Schablonenpuffer verwenden, um 2D- oder 3D-Bilder mit einer 3D-Szene zu kombinieren. Auf der Renderzieloberfläche wird mit einer Maske in dem Schablonenpuffer ein Bereich verdeckt. Gespeicherte 2D-Informationen, z. B. Text oder Bitmaps, können dann in den verdeckten Bereich geschrieben werden. Alternativ kann die Anwendung weitere 3D-Grundtypen auf den durch die Schablone maskierten Bereich der Renderzieloberfläche rendern. Es kann sogar eine ganze Szene gerendert werden.

In Spielen sind häufig mehrere 3D-Szenen zusammengesetzt. Zum Beispiel gibt es in Fahrspielen normalerweise einen Rückspiegel. Im Spiegel ist die Ansicht der 3D-Szene hinter dem Fahrer sichtbar. Das ist im Grunde eine zweite 3D-Szene, die mit der Sicht des Fahrers nach vorne zusammengesetzt ist.

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Decaling

Direct3D-Anwendungen steuern über die Rasterung, welche Pixel aus einem bestimmten Grundtypbild auf die Renderzieloberfläche gezeichnet werden. Anwendungen wenden Rasterungen für die Bilder der Grundtypen an, um komplanare Polygone richtig zu rendern.

Zum Beispiel sollten für die Darstellung von Reifenspuren und gelben Linien auf einer Straße die Reifenspuren direkt auf der Straße angezeigt werden. Die Z-Werte für die Reifenspuren und die Straße sind aber gleich. Der Tiefenpuffer kann deshalb eventuell keine klare Trennung zwischen den beiden erzeugen. Einige Pixel im Hintergrund-Grundtyp werden möglicherweise auf dem Vordergrund-Grundtyp (und umgekehrt) gerendert. Das resultierende Bild scheint von Frame zu Frame zu schimmern. Dieser Effekt wird „Z-Konflikt” oder „Flimmern” genannt.

Um dieses Problem zu beheben, verwenden Sie eine Schablone, mit der der Abschnitt des Hintergrund-Grundtyps maskiert wird, an dem die Rasterung angezeigt wird. Deaktivieren Sie die Z-Pufferung, und rendern Sie das Bild des Vordergrund-Grundtyps in den maskierten Bereich der Renderzieloberfläche.

Mischen Sie unterschiedliche Texturen, um das Problem zu lösen.

### <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">Beschreibt und Silhouetten

Sie können den Schablonenpuffer für abstraktere Effekte, wie Umrisse und Silhouetten, verwenden.

Wenn Ihre Anwendung zwei Render-Durchgänge durchführt – einen zum Erstellen der Schablonenmaske und den zweiten zum Anwenden der Schablonenmaske auf das Bild, wobei die Grundtypen beim zweiten Durchgang etwas kleiner sind - enthält das resultierende Bild nur den Umriss des Grundtyps. Die Anwendung kann dann den durch die Schablone maskierten Bereich des Bilds mit einer Volltonfarbe ausfüllen. Der Grundtyp erhält so ein geprägtes Aussehen.

Wenn die Schablonenmaske die gleiche Größe und Form wie der Grundtyp hat, den Sie rendern, enthält das resultierende Bild an der Stelle eine Lücke, an der sich der Grundtyp befinden sollte. Die Anwendung kann die Lücke mit Schwarz ausfüllen, um eine Silhouette des Grundtyps zu erstellen.

### <a name="span-idtwosidedstencilspanspan-idtwosidedstencilspanspan-idtwosidedstencilspantwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span>Zweiseitigen Schablone

Zum Zeichnen von Schatten mit dem Schablonenpuffer werden Schattenvolumen verwendet. Die Anwendung berechnet die Schattenvolumenumwandlung durch verdeckende Geometrie, indem die Ränder der Silhouette berechnet und aus dem Licht in eine Gruppe von 3D-Volumen extrudiert werden. Diese Volumen werden dann zweimal in den Schablonenpuffer gerendert.

Beim ersten Rendern werden die nach vorne gerichteten Polygone gezeichnet und die Werte im Schablonenpuffer erhöht. Beim zweiten Rendern werden die nach hinten gerichteten Polygone des Schattenvolumens gezeichnet und die Werte im Schablonenpuffer verringert.

In der Regel heben sich alle erhöhten oder verringerten Werte gegenseitig auf. Da die Szene aber bereits mit normaler Geometrie gerendert wurde, ist der Z-Puffer-Test für einige Pixel beim Rendern des Schattenvolumens nicht erfolgreich. Werte, die im Schablonenpuffer verblieben sind, entsprechen den Pixeln, die sich im Schatten befinden. Dieser verbliebene Inhalt des Schablonenpuffers wird als Maske verwendet, um ein großes, allumfassendes schwarzes Viereck in der Szene per Alpha-Überblendung zu überlagern. Das Ergebnis der Verwendung des Schablonenpuffers als Maske besteht darin, dass Pixel, die im Schatten liegen, dunkler werden.

Dies bedeutet, dass die Schattengeometrie zweimal pro Lichtquelle gezeichnet wird, und somit der Vertexdurchsatz der GPU stark belastet wird. Das Feature „zweiseitige Schablone” wurde entwickelt, um diese Situation abzuschwächen. In diesen Ansatz gibt es zwei Gruppen von Schablonenzuständen, einen für die nach vorne gerichteten Dreiecke und den anderen für die nach hinten gerichteten Dreiecke. Auf diese Weise wird nur ein einziger Durchlauf pro Schattenvolumen und pro Licht gezeichnet.

### <a name="span-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>Beim Lesen des Tiefenschablone Puffers als Textur

Ein inaktiver Tiefenschablonenpuffer kann von einem Shader als Textur gelesen werden. Eine Anwendung, die einen Tiefenschablonenpuffer als Textur liest, rendert in zwei Durchgängen. Der erste Durchgang schreibt in den Tiefenschablonenpuffer und der zweite Durchgang liest aus dem Puffer. Dadurch kann der Shader die vorher in den Puffer geschriebenen Tiefen- oder Schablonenwerte mit dem aktuell gerenderten Pixelwert vergleichen. Das Ergebnis des Vergleichs kann zur Erstellung von Effekten wie Schattenabbildungen oder weichen Partikeln in einem bestimmten System verwendet werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Fusion (OM)-Ausgabeschritt](output-merger-stage--om-.md)

[Grafikpipeline](graphics-pipeline.md)

[Ausgabezusammenführung-Phase](https://msdn.microsoft.com/library/windows/desktop/bb205120)
