---
ms.openlocfilehash: 9508a15492b2b2d6a27d042ddea323f142eb8103
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "104804331"
---
> [!IMPORTANT]
> Sie müssen ihren Frame initialisieren und das Fenster wie den **ongestarteten** -Code aktivieren. **Ongestartete wird nicht aufgerufen, wenn der Benutzer auf** den Popup klickt, auch wenn die app geschlossen wurde und zum ersten Mal gestartet wird. Häufig wird empfohlen, **ongestartete** und **onaktivierte** Methoden in ihrer eigenen Methode zu kombinieren `OnLaunchedOrActivated` , da die gleiche Initialisierung in beiden Fällen erfolgen muss.