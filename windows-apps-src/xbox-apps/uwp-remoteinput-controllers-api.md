---
title: API-Referenz für Geräte Portal Controller
description: Erfahren Sie, wie Sie die Anzahl angefügter physischer Controller erhalten und Sie Programm gesteuert deaktivieren.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 535846d7dbeb2d29328b5c5d01b06d4449a53790
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254185"
---
# <a name="controller-api-reference"></a>Controller-API-Referenz

Sie können die Anzahl angefügter physischer Controller erhalten und diese mithilfe dieser Rest-API ausschalten.

## <a name="determine-the-number-of-attached-physical-controllers"></a>Bestimmen der Anzahl angefügter physischer Controller

**Anforderung**

Sie können die Anzahl angefügter physischer Controller auf dem Gerät mithilfe der folgenden Anforderung überprüfen.

Methode | Anforderungs-URI |
-------|-------------|
| GET | /ext/remoteinput/controllers |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- JSON-Zahlen Eigenschaft connectedcontrollercount, die die Anzahl angefügter physischer Controller angibt.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode | BESCHREIBUNG |
|------------------|-------------|
| 200 | Erfolg |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>Trennen Sie alle physischen Controller im devkit.

**Anforderung**

Mithilfe der folgenden Anforderung können Sie alle physischen Controller auf dem Gerät trennen.

| Methode | Anforderungs-URI |
|--------|-------------|
| DELETE | /ext/remoteinput/controllers |

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
| 204 | Die Anforderung zum Trennen der Controller war erfolgreich. |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Xbox
