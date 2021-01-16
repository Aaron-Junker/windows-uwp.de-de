---
title: Referenz zur SSH-Pins-API des Geräte Portals
description: Erfahren Sie, wie Sie alle vertrauenswürdigen Secure Shell (SSH)-Pins Programm gesteuert mithilfe der/ext/App/sshpins Xbox Device Portal-Rest-API entfernen.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 999a77051b0eebe6ae5d8be9e4c2640709431b6d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254205"
---
# <a name="ssh-pins-api-reference"></a>SSH-Pins-API-Referenz

Mit dieser Rest-API können Sie alle vertrauenswürdigen SSH-Pins in Ihrem devkit entfernen.

## <a name="remove-trusted-ssh-pins"></a>Entfernen vertrauenswürdiger SSH-Pins

**Anforderung**

| Methode | Anforderungs-URI |
|--------|-------------|
| DELETE | /ext/app/sshpins |

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

| HTTP-Statuscode | BESCHREIBUNG |
|------------------|-------------|
| 204 | Die Anforderung zum Löschen der Pins war erfolgreich. |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Xbox
