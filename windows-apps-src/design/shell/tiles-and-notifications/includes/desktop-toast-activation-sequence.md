---
ms.openlocfilehash: 62470070b726aa7101b8299296dc1f4ef67f30e4
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "104804336"
---
Wenn der Benutzer auf eine ihrer Benachrichtigungen klickt (oder auf eine Schaltfläche in der Benachrichtigung), geschieht Folgendes...

**Wenn Ihre APP aktuell ausgeführt wird**...

1. Das Ereignis " **deastnotificationmanagercompat. onaktivi"** wird in einem Hintergrund Thread aufgerufen.

**Wenn Ihre APP zurzeit geschlossen ist**...

1. Die exe-APP wird gestartet und `ToastNotificationManagerCompat.WasCurrentProcessToastActivated()` gibt true zurück, um anzugeben, dass der Prozess aufgrund einer modernen Aktivierung gestartet wurde und dass der Ereignishandler bald aufgerufen wird.
1. Anschließend wird das Ereignis " **deastnotificationmanagercompat. onaktivi"** in einem Hintergrund Thread aufgerufen.