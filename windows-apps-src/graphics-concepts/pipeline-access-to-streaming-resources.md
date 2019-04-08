---
title: Pipelinezugriff auf Streamingressourcen
description: Streamingressourcen können in Shaderressourcenansichten (SRV), Renderzielansichten (RTV), Tiefenschablonenansichten (DSV) und in unsortierten Zugriffsansichten (UAV) sowie in bestimmten Bindungen ohne Ansichten, z. B. Vertex-Pufferbindungen, verwendet werden.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Pipelinezugriff auf Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6d95ffc14e9ae6d4ea59a4b3bdc33fd215cb61be
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616375"
---
# <a name="pipeline-access-to-streaming-resources"></a>Pipelinezugriff auf Streamingressourcen


Streamingressourcen können in Shaderressourcenansichten (SRV), Renderzielansichten (RTV), Tiefenschablonenansichten (DSV) und in unsortierten Zugriffsansichten (UAV) sowie in bestimmten Bindungen ohne Ansichten, z. B. Vertex-Pufferbindungen, verwendet werden. Die Liste der unterstützten Bindungen finden Sie unter [Parameter für das Erstellen von Streamingressourcen](streaming-resource-creation-parameters.md). Die verschiedenen D3D-Kopiervorgänge funktionieren ebenfalls bei Streamingressourcen.

Wenn mehrere Kachelkoordinaten in eine oder mehreren Ansichten an dieselbe Speicheradresse gebunden ist, finden Lese- und Schreibvorgänge aus unterschiedlichen Pfaden in einer nicht deterministischen und nicht wiederholbaren Speicherzugriff-Reihenfolge statt.

Wenn alle Kacheln hinter einem Speicherzugriffsbedarf von einem Shader eindeutigen Kacheln zugeordnet sind, entspricht das Verhalten für alle Implementierungen der Oberfläche dem Verhalten gleicher Speicherinhalte ohne Kacheln.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">SRV-Verhalten bei nicht zugeordnete Kacheln</a></p></td>
<td align="left"><p>Das Verhalten der Lesevorgänge der Shaderressourcenansicht (SRV), die nicht zugeordnete Kacheln umfassen, hängt von der Ebene der Hardwareunterstützung ab.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">UAV-Verhalten bei nicht zugeordnete Kacheln</a></p></td>
<td align="left"><p>Das Verhalten der Lese- und Schreibvorgänge der unsortierten Zugriffsansicht (Unordered Access View, UAV) hängt von der Hardwareunterstützung ab.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Verhalten des Rasterizers, mit Kacheln, die nicht zugeordnete</a></p></td>
<td align="left"><p>Dieser Abschnitt beschreibt Rasterizerverhalten bei nicht zugeordneten Kacheln.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Kachel "Access-Einschränkungen mit doppelten Zuordnungen</a></p></td>
<td align="left"><p>Bei doppelten Zuordnungen gibt es Kachelzugriffseinschränkungen, wie z. B. beim Kopieren von Streamingressourcen mit Quellen- und Zielüberlappung oder beim Rendern von Kacheln innerhalb des Bereichs Rendern freigegeben.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Streaming Ressourcen texture-Sampling-Funktionen</a></p></td>
<td align="left"><p>Textursampling-Features für Streamingressourcen enthalten: Abrufen von Feedback zum Shaderstatus zugeordneter Bereiche, Überprüfen, ob alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurden, Klammerung, damit Shader Bereiche in Mipmap-Streamingressourcen vermeiden, die nicht zugeordnet wurden, und Ermitteln der minimalen Detailtiefe (Level-of-Detail, LOD), die für den gesamten Speicherbedarf einer Texturfilterung vollständig zugeordnet ist.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Offenlegung von streaming-Ressourcen "HLSL"</a></p></td>
<td align="left"><p>Eine spezielle Syntax für die Microsoft High Level Shader Language (HLSL) ist für die Unterstützung von Streaming Ressourcen in <a href="https://msdn.microsoft.com/library/windows/desktop/ff471356">Shadermodell 5</a> erforderlich.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Streamingressourcen](streaming-resources.md)

 

 




