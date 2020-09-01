---
title: Pufferanordnung
description: Eine Puffer Ressource ist in Kacheln mit 64 KB unterteilt, wobei ein Leerzeichen in der letzten Kachel leer ist, wenn die Größe kein Vielfaches von 64 KB ist.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Pufferanordnung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdb78dc2cff6ccedec58acbc946068e14511fdc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156354"
---
# <a name="buffer-tiling"></a>Pufferanordnung


Eine [Puffer](introduction-to-buffers.md) Ressource ist in Kacheln mit 64 KB unterteilt, wobei ein Leerzeichen in der letzten Kachel leer ist, wenn die Größe kein Vielfaches von 64 KB ist.

Für strukturierte Puffer ist keine Einschränkung auf dem STRIDE erforderlich, um gekachelt zu werden. Aber mögliche Leistungsoptimierungen in der Hardware für die Verwendung von [**structuredbuffers**](/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) können durch das Anordnen der Kacheln an erster Stelle geopfert werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[So unterteilen Sie den Bereich einer Streamingressource](how-a-streaming-resource-s-area-is-tiled.md)

 

 