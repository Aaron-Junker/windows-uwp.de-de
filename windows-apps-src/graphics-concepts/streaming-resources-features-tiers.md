---
title: Ebenen der Features von Streamingressourcen
description: Direct3D unterstützt Streamingressourcen in drei Featureebenen.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- Ebenen der Features von Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c872d289c67161e414671d3d509401f0539a7675
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631445"
---
# <a name="streaming-resources-features-tiers"></a>Ebenen der Features von Streamingressourcen


Direct3D unterstützt Streamingressourcen in drei Featureebenen.

Ebene 1 stellt grundlegende Funktionen für Streamingressourcen bereit.

Ebene 2 fügt über die Ebene 1 hinausgehende Funktionen hinzu, z. B. Garantieren eines nicht gepackten Textur-Mipmap, wenn die Größe mindestens eine Standardkachelform beträgt, Shaderanweisungen zur Klammerung der Detailebene (Level-of-Detail, LOD) und zum Abrufen des Status des Shadervorgangs sowie das Lesen aus NULL-zugeordneten Kacheln behandeln, deren Samplingwert null ergab.

Ebene 3 fügt über Ebene 2 hinausgehende Texture3D-Funktionen hinzu.

In den Versionen von Direct3D sind Abfragefunktionen zum Überprüfen der Hardware und der Treiberunterstützung für Streamingressourcen sowie der Ebene verfügbar.

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
<td align="left"><p><a href="tier-1.md">Ebene 1</a></p></td>
<td align="left"><p>In diesem Abschnitt wird die Unterstützung für die Ebene 1 beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">Ebene 2</a></p></td>
<td align="left"><p>Die Unterstützung der Ebene 2 für Streamingressourcen fügt über die Ebene 1 hinausgehende Funktionen hinzu, z. B. Garantieren eines nicht gepackten Textur-Mipmap, wenn die Größe mindestens eine Standardkachelform beträgt, Shaderanweisungen zur Klammerung der Detailebene (Level-of-Detail, LOD) und zum Abrufen des Status des Shadervorgangs sowie das Lesen aus NULL-zugeordneten Kacheln behandeln, deren Samplingwert null ergab.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">Ebene 3</a></p></td>
<td align="left"><p>Ebene 3 fügt zusätzlich zu den Funktionen der <a href="tier-2.md">Ebene 2</a> die Unterstützung für Texture3D für Streamingressourcen hinzu.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Streaming von Ressourcen](streaming-resources.md)

 

 




