---
title: Pufferanordnung
description: Eine Puffer-Ressource ist in 64 KB große Kacheln unterteilt und verfügt über einen leeren Bereich in der letzten Kachel, wenn die Größe keine Potenz von 64 KB ist.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Pufferanordnung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 34201c889ed984b27d50126bd2a04e9b320a17a3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370653"
---
# <a name="buffer-tiling"></a>Pufferanordnung


Eine [Puffer](introduction-to-buffers.md)-Ressource ist in 64 KB große Tiles unterteilt, mit Leerraum in der letzten Tile, wenn die Größe kein Vielfaches von 64 KB ist.

Strukturierte Puffer dürfen keine Einschränkung für die Breite haben, wenn sie als Kacheln angeordnet werden sollen. Es kann auf mögliche Leistungsoptimierungen in der Hardware bei der Verwendung von [**StructuredBuffers**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) verzichtet werden, indem sie von Anfang an als Kacheln angeordnet werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Wie ein streaming Ressourcenbereich gekachelt wird](how-a-streaming-resource-s-area-is-tiled.md)

 

 




