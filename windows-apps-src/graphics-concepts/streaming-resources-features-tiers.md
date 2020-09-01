---
title: Ebenen der Features von Streamingressourcen
description: Greifen Sie auf Artikel zu den drei Funktions Funktionen für Direct3D-Streamingressourcen zu, die zuvor als gekachelte Ressourcen bezeichnet wurden.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- Ebenen der Features von Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ee27244c4d4c2797b71c9d5c8c2c5185a99596b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173054"
---
# <a name="streaming-resources-features-tiers"></a>Ebenen der Features von Streamingressourcen


Direct3D unterstützt Streaming-Ressourcen in drei Ebenen von Funktionen.

Ebene 1 stellt grundlegende Funktionen für das Streamen von Ressourcen bereit.

Ebene 2 bietet Funktionen, die über Ebene 1 hinausgehen, z. b. das garantieren einer nicht verpackten Textur-MipMap, wenn die Größe mindestens einer Standard Kachel Form entspricht. shaderanweisungen zum Spannen der Detailebene (LOD) und zum Abrufen des Status über den shadervorgang. beim Lesen von mit NULL zugeordneten Kacheln wird der Stichproben Wert auch als 0 (null) behandelt.

Ebene 3 bietet Texture3D-Funktionen, die über Ebene 2 hinausgehen.

Abfragefunktionen stehen in den Versionen von Direct3D zur Verfügung, um die Unterstützung von Hardware und Treibern für Streamingressourcen und auf der Ebene der Ebene zu überprüfen.

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
<td align="left"><p><a href="tier-1.md">Ebene 1</a></p></td>
<td align="left"><p>In diesem Abschnitt wird die Unterstützung für Ebene 1 beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">Ebene 2</a></p></td>
<td align="left"><p>Die Unterstützung der Ebene 2 für Streaming-Ressourcen fügt Funktionen über Ebene 1 hinaus hinzu, z. b. die Gewährleistung der nicht gepackten Textur MipMap, wenn die Größe mindestens eine Standard Kachel Form ist shaderanweisungen zum Spannen der Detailebene (LOD) und zum Abrufen des Status über den shadervorgang. beim Lesen von mit NULL zugeordneten Kacheln wird der Stichproben Wert auch als 0 (null) behandelt.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">Ebene 3</a></p></td>
<td align="left"><p>Auf Ebene 3 wird zusätzlich zu den Funktionen der <a href="tier-2.md">Ebene 2</a> Texture3D für Streaming-Ressourcen unterstützt.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Streamingressourcen](streaming-resources.md)

 

 




