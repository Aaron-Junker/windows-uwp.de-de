---
title: Erstellen von Streamingressourcen
description: Streamingressourcen werden erstellt, indem Sie beim Erstellen einer Ressource laut Kennzeichen angeben, dass die Ressource eine Streamingressource ist.
ms.assetid: B3F3E43C-54D4-458C-9E16-E13CB382C83F
keywords:
- Erstellen von Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec96f6245969d32357563c44107f539fb9043aac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618245"
---
# <a name="creating-streaming-resources"></a>Erstellen von Streamingressourcen


Streamingressourcen werden erstellt, indem Sie beim Erstellen einer Ressource laut Kennzeichen angeben, dass die Ressource eine Streamingressource ist.

Einschränkungen für das Erstellen einer Ressource als Streamingressource werden unter [Parameter für das Erstellen von Streamingressourcen](streaming-resource-creation-parameters.md) beschrieben.

Beim Erstellen der Ressource wird dem Grafiksystem ein Nicht-Streamingressourcenspeicher zugewiesen, z. B. bei der Zuordnung für ein Array von 2D-Texturen.

Wenn eine Streamingressource erstellt wird, weist das Grafiksystem dem Inhalt der Ressource keinen Speicher hinzu. Wenn eine Anwendung eine Streamingressource erstellt, reserviert das Grafiksystem stattdessen einen Adressbereich nur für die nebeneinander angeordnete Oberfläche und ermöglicht dann der Anwendung, die Zuordnung der Kacheln zu steuern. Die „Zuordnung” einer Kachel ist der physische Standort im Arbeitsspeicher, auf den die logische Kachel in einer Ressource verweist (oder **NULL** für eine nicht zugeordnete Kachel).

Verwechseln Sie dieses Konzept nicht mit der Zuordnung einer Direct3D-Ressource für den CPU-Zugriff - trotz des gleichen Namens sind diese komplett unabhängig voneinander. Sie können die Zuordnung jeder einzelnen Kachel nach Bedarf definieren und ändern. Nicht alle Kacheln für eine Oberfläche müssen zu einem bestimmten Zeitpunkt zugeordnet werden, wodurch die Größe des verfügbaren Arbeitsspeichers effektiv verwaltet wird.

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
<td align="left"><p><a href="mappings-are-into-a-tile-pool.md">Zuordnungen sind in einem Pool für die Kachel "</a></p></td>
<td align="left"><p>Beim Erstellen einer Ressource als Streaming-Ressource stammen die Kacheln, die die Ressource bilden, aus Speicherorten in einem Kachelpool. Ein Kachelpool ist ein Speicherpool (gesichert durch eine oder mehrere Zuordnungen im Hintergrund – nicht sichtbar für die Anwendung).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-creation-parameters.md">Stream-Parameter für die Ressource</a></p></td>
<td align="left"><p>Es gibt einige Einschränkungen für den Direct3D-Ressourcentyp, den Sie als Streamingressource erstellen können.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tile-pool-creation-parameters.md">Kachel "-Parameter zum Erstellen von Pools</a></p></td>
<td align="left"><p>Verwenden Sie die Parameter in diesem Abschnitt, um beim Erstellen eines Puffers den Kachelpool zu definieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-cross-process-and-device-sharing.md">Streamen von Ressource Prozess- und gemeinsame Nutzung von Geräten</a></p></td>
<td align="left"><p>Kachelpools können von anderen Prozessen wie herkömmliche Ressourcen freigegeben werden. Streamingressourcen, die auf Kachelpools verweisen, können nicht auf allen Geräten und Prozessen freigegeben werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="operations-available-on-streaming-resources.md">Vorgänge, die für das streaming von Ressourcen verfügbar sind</a></p></td>
<td align="left"><p>Dieser Abschnitt enthält Vorgänge, die Sie auf Streaming-Ressourcen ausführen können.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="operations-available-on-tile-pools.md">Vorgänge, die auf die Kachel "-Pools verfügbar sind</a></p></td>
<td align="left"><p>Vorgänge für Kachelpools umfassen das Ändern der Größe eines Kachelpools, das Anbieten von Ressourcen (Gewinnen von vorübergehendem Speicherplatz für das System für den gesamten Kachelpool) und das Zurückfordern von Ressourcen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="how-a-streaming-resource-s-area-is-tiled.md">Wie ein streaming Ressourcenbereich gekachelt wird</a></p></td>
<td align="left"><p>Wenn Sie eine Streamingressource erstellen, bestimmen Dimensionen, Größe des Formats und Anzahl der Mipmaps und/oder Array-Segmente (falls zutreffend) die Anzahl der erforderlichen Kacheln, um die gesamte Oberfläche zu sichern.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Streaming von Ressourcen](streaming-resources.md)

 

 




