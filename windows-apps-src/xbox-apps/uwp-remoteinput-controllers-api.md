---
title: Geräteportal-Controller – API-Referenz
description: Hier erfahren Sie, wie Sie die Anzahl der angeschlossenen physischen Controller abrufen und sie programmgesteuert deaktivieren.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8b5061f9193d78d4ff23f5fa707b0bea67a10f98
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657005"
---
# <a name="controller-api-reference"></a>Controller – API-Referenz   
Mit dieser REST-API können Sie die Anzahl der angeschlossenen physischen Controller abrufen und deaktivieren.

## <a name="determine-the-number-of-attached-physical-controllers"></a>Bestimmen der Anzahl der angeschlossenen physischen Controller

**Anforderung**

Sie können die Anzahl der angeschlossenen physischen Controller auf dem Gerät mit der folgenden Anforderung überprüfen.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/remoteinput/controllers
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- Die JSON-Number-Eigenschaft ConnectedControllerCount, die die Anzahl der angeschlossenen physischen Controller angibt.

**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Möglich
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>Trennen aller physischen Controller im Entwickler-Kit

**Anforderung**

Sie können alle physischen Controller auf dem Gerät mithilfe der folgenden Anforderung trennen.

Methode      | Anforderungs-URI
:------     | :-----
DELETE | /ext/remoteinput/controllers
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- Keine 

**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
204 | Die Anforderung zum Trennen der Controller war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

<br />
**Gerätefamilien verfügbar**

* Windows Xbox
