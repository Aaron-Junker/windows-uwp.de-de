---
title: UAV-verhalten bei nicht zugeordneten Kacheln
description: Das Verhalten der Lese- und Schreibvorgänge der unsortierten Zugriffsansicht (Unordered Access View, UAV) hängt von der Hardwareunterstützung ab.
ms.assetid: CDB224E2-CC07-4568-9AAC-C8DC74536561
keywords:
- UAV-verhalten bei nicht zugeordneten Kacheln
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c72931d2f6bf1e892e174bc409f20a042d5e4c92
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631625"
---
# <a name="span-iddirect3dconceptsuavbehaviorwithnon-mappedtilesspanuav-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.uav_behavior_with_non-mapped_tiles"></span>UAV-Verhalten bei nicht zugeordnete Kacheln


Das Verhalten der Lese- und Schreibvorgänge der unsortierten Zugriffsansicht (Unordered Access View, UAV) hängt von der Hardwareunterstützung ab. Eine Aufschlüsselung der Anforderungen finden Sie im Thema zum allgemeinen Lese- und Schreibverhalten unter [Ebenen der Features von Streamingressourcen](streaming-resources-features-tiers.md). In diesem Abschnitt wird das ideale Verhalten zusammengefasst.

Shader-Operationen, die von einer nicht-zugeordneten Kachel in einer UAV lesen, geben in allen nicht-fehlenden Komponenten des Formats und im Standard für fehlende Komponenten 0 zurück.

Wenn Shader-Operationen versuchen, auf eine nicht-zugeordnete Kachel zu schreiben, wird nichts in den nicht-zugeordneten Bereich geschrieben (während die Schreibvorgänge in einen zugeordneten Bereich fortgesetzt werden). Diese ideale Definition für die Behandlung von Schreibvorgängen ist für [Ebene 2](tier-2.md) nicht erforderlich. Schreibvorgänge an nicht zugeordnete Kacheln können zu einem Cache führen, den nachfolgende Lesevorgänge aufnehmen können.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Pipeline-Zugriff auf Ressourcen streaming](pipeline-access-to-streaming-resources.md)

 

 




