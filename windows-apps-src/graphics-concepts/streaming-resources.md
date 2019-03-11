---
title: Streamingressourcen
description: Streamingressourcen sind große logische Ressourcen, die wenig physischen Speicher belegen. Anstatt die gesamte große Ressource zu übergeben, werden nur kleine Teile der Ressource bei Bedarf gestreamt. Streamingressourcen wurden früher als unterteilte Ressourcen bezeichnet.
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- Streamingressourcen
- Ressourcen, Streaming
- Ressourcen, unterteilte
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c15c8a82219109a96d0a9ca192c4dfff5d86c9aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598235"
---
# <a name="streaming-resources"></a>Streamingressourcen


*Streamingressourcen* sind große logische Ressourcen, die wenig physischen Speicher belegen. Anstatt die gesamte große Ressource zu übergeben, werden nur kleine Teile der Ressource bei Bedarf gestreamt. Streaming-Ressourcen wurden vorher als *unterteilte Ressourcen* bezeichnet.

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
<td align="left"><p><a href="the-need-for-streaming-resources.md">Die Notwendigkeit für das streaming von Ressourcen</a></p></td>
<td align="left"><p>Streamingressourcen sind erforderlich, damit der GPU-Speicher nicht unnötig durch die Speicherung von Oberflächenbereichen belegt wird, auf die nicht zugegriffen wird, und um der Hardware mitzuteilen, wie angrenzende Kacheln gefiltert werden sollen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="creating-streaming-resources.md">Erstellen von streaming-Ressourcen</a></p></td>
<td align="left"><p>Streamingressourcen werden erstellt, indem Sie beim Erstellen einer Ressource laut Kennzeichen angeben, dass die Ressource eine Streamingressource ist.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pipeline-access-to-streaming-resources.md">Pipeline-Zugriff auf Ressourcen streaming</a></p></td>
<td align="left"><p>Streamingressourcen können in Shaderressourcenansichten (SRV), Renderzielansichten (RTV), Tiefenschablonenansichten (DSV) und in unsortierten Zugriffsansichten (UAV) sowie in bestimmten Bindungen ohne Ansichten, z. B. Vertex-Pufferbindungen, verwendet werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resources-features-tiers.md">Streaming der Tarife der Ressourcen-Funktionen</a></p></td>
<td align="left"><p>Direct3D unterstützt Streamingressourcen in drei Featureebenen.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Schulungsleitfaden für Direct3D-Grafiken](index.md)

[Ressourcen](resources.md)

 

 




