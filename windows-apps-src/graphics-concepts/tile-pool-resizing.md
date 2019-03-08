---
title: Ändern der Größe des Kachelpools
description: Ändern der Größe eines Kachelpools zum Vergrößern eines Kachelpools, wenn die Anwendung mehr Arbeit der Streaming-Ressourcen benötigt, welche in diese zuordnen, bzw. zum Verkleinern des Kachelpools, wenn weniger Speicherplatz benötigt wird.
ms.assetid: A54A06DC-BDDB-42DC-85E8-C64241100ED5
keywords:
- Ändern der Größe des Kachelpools
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7e08447c575e99178e503e99eb651cd5e225a898
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607775"
---
# <a name="tile-pool-resizing"></a>Ändern der Größe des Kachelpools


Ändern der Größe eines Kachelpools zum Vergrößern eines Kachelpools, wenn die Anwendung mehr Arbeit der Streaming-Ressourcen benötigt, welche in diese zuordnen, bzw. zum Verkleinern des Kachelpools, wenn weniger Speicherplatz benötigt wird. Eine weitere Anwendungsmöglichkeit ist die Zuordnung zusätzlicher Kachelpools für neue Streamingressourcen. Wenn jedoch eine einzelne Streamingressource mehr Speicherplatz benötigt als anfangs im Kachelpool verfügbar ist, dann ist die Vergrößerung des Kachelpools eine gute Wahl. Eine Streamingressource kann nicht Zuordnungen in mehreren Kachel Pools gleichzeitig haben.

Wenn ein Kachelpool vergrößert wurde, werden weitere Kacheln durch den Anzeigetreiber am Ende über eine oder mehrere neue Vergaben hinzugefügt. Diese Aufschlüsselung der Vergaben ist in der Anwendung nicht sichtbar. Vorhandener Speicher im Kachelpool bleibt unverändert, und vorhandene Zuordnungen der Streamingressourcen im Arbeitsspeicher bleiben erhalten.

Wenn ein Kachelpool verkleinert wird, werden die Kacheln ab dem Ende entfernt. Kacheln werden auch bis auf 0, d. h. bis auf die ursprüngliche Vergabegröße, entfernt. Das bedeutet, dass über die neue Größe hinaus keine neuen Zuordnungen hergestellt werden können. Bestehende Zuordnung über das Ende der neuen Größe hinaus bleiben jedoch unverändert und verwendbar. Der Anzeigetreiber wird den Arbeitsspeicher behalten, so lange Zuordnungen zu einem Teil der Vergaben, die der Treiber für die Kachelpoolspeicher verwendet, bestehen. Wenn nach dem Verkleinern ein Teil des Speichers aktiv gehalten wird, da Kachelzuordnungen darauf zeigen und der Kachelpool erneut (mit beliebiger Menge) vergrößert wurde, wird der vorhandene Arbeitsspeicher zunächst wiederverwendet, bevor jegliche zusätzlichen Vergaben für den Vergrößerungsvorgang erfolgen.

Um Speicherplatz sparen zu können, muss eine Anwendung nicht nur einen Kachelpool verkleinern, sondern auch bestehende Zuordnungen über dem Ende der neuen, kleineren Kachelpoolgröße hinaus entfernen/erneut zuordnen.

Der Vorgang zur Verkleinerung (und Entfernen von Zuordnungen) resultiert nicht unbedingt sofort in Arbeitsspeichereinsparungen. Das Freigeben von Arbeitsspeicher hängt davon ab, wie präzise die dem Anzeigetreiber zugrunde liegenden Freigaben für den Kachelpool sind. Wenn die Verkleinerung dafür ausreicht, die Vergabe des Anzeigetreibers überflüssig zu machen, kann der Anzeigetreiber sie freigeben. Wurde ein Kachel-Pool vergrößert, ist das Verkleinern auf bisherige Größen (und das entsprechende Entfernen/Neuzuordnen der Kachelzuordnungen) die beste Option, um Arbeitsspeicher einzusparen. Dies ist jedoch nicht gewährleistet, falls die Größen nicht genau mit den zugrunde liegenden und vom Anzeigetreiber ausgewählten Vergabegrößen übereinstimmen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Zuordnungen sind in einem Pool für die Kachel "](mappings-are-into-a-tile-pool.md)

 

 




