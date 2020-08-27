---
title: API-Referenz zur Bereitstellung des Geräte Portals
description: Erfahren Sie, wie Sie die Rest-API DeployInfo des Xbox-Geräte Portals verwenden, um Bereitstellungs Informationen für mindestens ein installiertes Paket anzufordern.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 5260125625ced6c258a683bcfb9b552e57d07f06
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943000"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Fordert Bereitstellungs Informationen für mindestens ein installiertes Paket an.

**Anforderung**

Methode      | Anforderungs-URI
:------     | :------
POST | /ext/app/deployinfo
<br />
**URI-Parameter**

 - Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

Ein JSON-Array im folgenden Format:

* DeployInfo
  * Packagefullname: der Name des Pakets, zu dem Informationen angefordert werden.
  * Overlayfolder: optionaler Pfad zu einem Überlagerungs Ordner Pfad, wenn dieses Feature verwendet wird.

###<a name="response"></a>Antwort

**Antworttext**

Ein JSON-Array im folgenden Format (einige Felder sind optional):

* DeployInfo
  * Packagefullname: Name des Pakets, über das wir Informationen erhalten.
  * Deploytype: der Bereitstellungstyp.
  * Deploypthorspecifieres: ein Bereitstellungs Pfad für lose bereit Stellungen oder installierte Spezifizierer für gepackte bereit Stellungen.
  * Deploydrive: das Laufwerk, auf dem das Paket für anwendbare Bereitstellungs Typen bereitgestellt wird.
  * Bereitstellungstyp: die Größe des Pakets in Bytes für anwendbare Bereitstellungs Typen.
  * Overlayfolder: der über Lagerungs Ordner für bereit Stellungen, die diese Funktion unterstützen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
200 | Erfolg
4XX | Fehlercodes
5XX | Fehlercodes
<br />

**Verfügbare Gerätefamilien**

* Windows Xbox