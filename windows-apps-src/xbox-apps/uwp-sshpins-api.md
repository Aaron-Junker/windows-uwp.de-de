---
title: Referenz zur SSH-Pins-API des Geräte Portals
description: Erfahren Sie, wie Sie alle vertrauenswürdigen Secure Shell (SSH)-Pins Programm gesteuert mithilfe der/ext/App/sshpins Xbox Device Portal-Rest-API entfernen.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 307af4cdd4e998832f4a2fe7a8f874615fe10ad7
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043532"
---
# <a name="ssh-pins-api-reference"></a>SSH-Pins-API-Referenz
Mit dieser Rest-API können Sie alle vertrauenswürdigen SSH-Pins in Ihrem devkit entfernen.

## <a name="remove-trusted-ssh-pins"></a>Entfernen vertrauenswürdiger SSH-Pins

**Anforderung**

Methode      | Anforderungs-URI
:------     | :-----
Delete | /ext/app/sshpins
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- Keine 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
204 | Die Anforderung zum Löschen der Pins war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

<br />
**Verfügbare Gerätefamilien**

* Windows Xbox

