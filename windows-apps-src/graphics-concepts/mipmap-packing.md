---
title: MipMap-Verpackung
description: Eine gewisse Anzahl an Mips (pro Array-Segment) kann in einer gewissen Anzahl von Kacheln verpackt sein, in Abhängigkeit von den Dimensionen der Streaming-Ressource, dem Format, der Anzahl der Mip-Maps und Array-Segmente.
ms.assetid: 906C3CAC-4E84-4947-B508-06788551BE85
keywords:
- MipMap-Verpackung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b2733da1f843062a1fa7f2b4a7969326523d54e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608105"
---
# <a name="mipmap-packing"></a>MipMap-Verpackung


Eine gewisse Anzahl an Mips (pro Array-Segment) kann in einer gewissen Anzahl von Kacheln verpackt sein, in Abhängigkeit von den Dimensionen der Streaming-Ressource, dem Format, der Anzahl der Mip-Maps und Array-Segmente.

Je nach [Ebene](streaming-resources-features-tiers.md) des Streaming-Ressourcen-Supports folgen Mip-Maps mit bestimmten Dimensionen nicht den Standard-Kachel Formen und gelten als in einer für die Anwendung unverständlichen Weise miteinander verpackt. Höhere Supportebenen zeichnen sich durch breiter gefasste Garantien darüber aus, welche Arten an Oberflächendimensionen in die Standard-Kachel Formen passen (und können daher einzeln durch Anwendungen zugeordnet werden).

Die möglichen Unterschiede zwischen Implementierungen bestehen darin, dass – angesichts der Dimensionen der Streaming-Ressource, des Formates, der Anzahl an Mip-Maps und Array-Segmente – eine Anzahl M an Mips (pro Array-Segment) in eine bestimmte Anzahl N an Kacheln gepackt werden kann. Wenn Sie die Ressourcenkachelinformationen für ein Gerät erhalten, meldet der Treiber an die Anwendung die Werte für M und N (neben anderen Informationen über die Oberfläche, die standardmäßig sind und nicht zwischen Hardwareherstellern variieren). Die Kacheln für die verpackten Mips sind weiterhin 64 KB groß und können einzeln verschiedenen Positionen in einem Kachelpool zugeordnet werden.

Jedoch sind die Pixelform der Kacheln und wie die Mip-Maps in die Kacheln passen für einen Hardware-Anbieter spezifisch und zu komplex zu erläutern. Daher müssen Anwendungen entweder gleichzeitig alle Kacheln zuordnen, welche als verpackt festgelegt sind, oder keine. Andernfalls ist das Verhalten für den Zugriff auf die Streaming-Ressource nicht definiert.

Für angeordnete Oberflächen gelten die Anzahl an verpackten Mips und die Anzahl an verpackten Kacheln, welche diese Mips speichern (M und N, wie vorstehend beschrieben) einzeln für jedes Array-Segment.

Dedizierte API für das Kopieren von Kacheln können nicht auf verpackte Mips zugreifen. Anwendungen, die Daten in und aus verpackten Mips kopieren möchten, können dazu alle Nicht-Streaming-Ressourcenspezifische API zum Kopieren und zum Rendern von Oberflächen verwenden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Wie ein streaming Ressourcenbereich gekachelt wird](how-a-streaming-resource-s-area-is-tiled.md)

 

 




