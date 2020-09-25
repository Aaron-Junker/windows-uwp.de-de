---
title: Pipelinezugriff auf Streamingressourcen
description: Streaming-Ressourcen können in Shader-Ressourcen Ansichten (SRV), renderzielsichten (RTV), Datenquellen Sichten (Datenquellen Sicht) und unsortierter Zugriffs Sichten (UAV) sowie einigen Bindungs Punkten verwendet werden, bei denen Sichten nicht verwendet werden, wie z. b. Vertex-Puffer Bindungen.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Pipelinezugriff auf Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 15b37e471e45a1c2ca604c1a5bf28ace69e35ad3
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220323"
---
# <a name="pipeline-access-to-streaming-resources"></a>Pipelinezugriff auf Streamingressourcen


Streaming-Ressourcen können in Shader-Ressourcen Ansichten (SRV), renderzielsichten (RTV), Datenquellen Sichten (Datenquellen Sicht) und unsortierter Zugriffs Sichten (UAV) sowie einigen Bindungs Punkten verwendet werden, bei denen Sichten nicht verwendet werden, wie z. b. Vertex-Puffer Bindungen. Die Liste der unterstützten Bindungen finden Sie unter [Streaming-Ressourcen Erstellungs Parameter](streaming-resource-creation-parameters.md). Die verschiedenen D3D-Kopiervorgänge funktionieren auch für Streamingressourcen.

Wenn mehrere Kachel Koordinaten in einer oder mehreren Ansichten an denselben Speicherort gebunden sind, werden Lese-und Schreibvorgänge aus unterschiedlichen Pfaden desselben Speichers in einer nicht deterministischen und nicht wiederholbaren Reihenfolge von Speicherzugriffen ausgeführt.

Wenn alle Kacheln hinter einer Speicherzugriffs Beanspruchung von einem Shader eindeutigen Kacheln zugeordnet sind, ist das Verhalten auf allen Implementierungen mit der Oberfläche identisch, die den gleichen Speicherinhalt auf nicht gekachelte Weise aufweisen.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">SRV-verhalten bei nicht zugeordneten Kacheln</a></p></td>
<td align="left"><p>Das Verhalten von Lesevorgängen in der Shader-Ressourcen Ansicht (SRV), die nicht zugeordnete Kacheln einschließen, hängt von der Hardwareunterstützung ab.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">UAV-verhalten bei nicht zugeordneten Kacheln</a></p></td>
<td align="left"><p>Das Verhalten von Lese-und Schreibvorgängen für die ungeordnete Zugriffs Ansicht (UAV) hängt von der Hardwareunterstützung ab.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Rasterizerverhalten bei nicht zugeordneten Kacheln</a></p></td>
<td align="left"><p>In diesem Abschnitt wird das Verhalten des Rasterizers mit nicht zugeordneten Kacheln beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Kachelzugriffseinschränkungen bei doppelten Zuordnungen</a></p></td>
<td align="left"><p>Es gibt Einschränkungen bezüglich des Kachel Zugriffs mit doppelten Zuordnungen, z. b. beim Kopieren von Streamingressourcen mit überlappenden Quellen und Zielen oder beim Rendern von Kacheln, die im Renderbereich freigegeben werden</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Textursampling-Features für Streamingressourcen</a></p></td>
<td align="left"><p>Zu den Funktionen für die Textur Stichproben für Streaming-Ressourcen gehört das Abrufen des shaderstatus-Feedbacks zu zugeordneten Bereichen, das überprüfen, ob alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurde, das Anspannen an hilfshader, um Bereiche in falsch zugeordneten Streamingressourcen zu vermeiden, die bekanntermaßen nicht zugeordnet sind</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Belichtung von HLSL-Streamingressourcen</a></p></td>
<td align="left"><p>Zur Unterstützung von Streamingressourcen in <a href="/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5">Shader-Modell 5</a>ist eine bestimmte Microsoft High Level Shader Language (HLSL)-Syntax erforderlich.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Streamingressourcen](streaming-resources.md)

 

 