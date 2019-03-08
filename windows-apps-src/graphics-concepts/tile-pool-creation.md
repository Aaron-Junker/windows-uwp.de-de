---
title: Erstellung eines Kachelpools
description: Anwendungen können einen oder mehrere Kachelpools pro Direct3D-Gerät erstellen. Die Gesamtgröße der jeder Kachel-Pool ist auf Direct3D 11 Resource-größenbeschränkung, handelt es sich etwa 1/4 der Grafikprozessor (GPU) Arbeitsspeicher beschränkt.
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- Erstellung eines Kachelpools
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ce3824ab2d435b42df9586a6c229b68db10a0c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612805"
---
# <a name="tile-pool-creation"></a>Erstellung eines Kachelpools


Anwendungen können einen oder mehrere Kachelpools pro Direct3D-Gerät erstellen. Die Gesamtgröße der jeder Kachel-Pool ist auf Direct3D 11 Resource-größenbeschränkung, handelt es sich etwa 1/4 der Grafikprozessor (GPU) Arbeitsspeicher beschränkt.

Ein Kachelpool besteht aus 64 KB großen Kacheln, aber das Betriebssystem (Anzeigetreiber) verwaltet im Hintergrund den gesamten Pool als ein oder mehrere Vergaben – die Aufteilung ist nicht für Anwendungen sichtbar. Streamingressourcen definieren den Inhalt durch das Zeigen auf Kacheln in einem Kachelpool. Das Aufheben der Zuordnung einer Kachel über eine Streamingressource erfolgt durch das Zeigen der Kachel auf **NULL**. Solche nicht zugeordneten Kacheln unterliegen Regeln, die über das Verhalten von Lese- und Schreibvorgängen bestimmen. Mehr Informationen finden Sie im [Vergleich von Kachelpoolressourcen und der Gefahrennachverfolgung](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Zuordnungen sind in einem Pool für die Kachel "](mappings-are-into-a-tile-pool.md)

 

 




