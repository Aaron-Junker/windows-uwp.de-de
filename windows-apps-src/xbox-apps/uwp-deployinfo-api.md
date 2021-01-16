---
title: API-Referenz zur Bereitstellung des Geräte Portals
description: Erfahren Sie, wie Sie die Rest-API DeployInfo des Xbox-Geräte Portals verwenden, um Bereitstellungs Informationen für mindestens ein installiertes Paket anzufordern.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2819b21e12d25aca941808e1feeb8a7539750a91
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254225"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Fordert Bereitstellungs Informationen für mindestens ein installiertes Paket an.

**Anforderung**

| Methode | Anforderungs-URI |
|--------|-------------|
| POST | /ext/app/deployinfo |

**URI-Parameter**

 - Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

Ein JSON-Array im folgenden Format:

* DeployInfo
  * Packagefullname: der Name des Pakets, zu dem Informationen angefordert werden.
  * Overlayfolder: optionaler Pfad zu einem Überlagerungs Ordner Pfad, wenn dieses Feature verwendet wird.

### <a name="response"></a>Antwort

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

| HTTP-Statuscode | BESCHREIBUNG |
|------------------|-------------|
| 200 | Erfolg |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Xbox