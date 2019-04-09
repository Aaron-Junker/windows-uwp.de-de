---
title: Referenz zur API des Geräteportals für Xbox-Informationen
description: Erfahren Sie, wie Sie auf Xbox-Geräteinformationen zugreifen.
ms.date: 11/072017
ms.topic: article
keywords: Windows 10 "," Uwp "," Xbox "," Device-portal
ms.localizationpriority: medium
ms.openlocfilehash: 7aa8b11bc439266d36fbb27a7eaa7b07e924a17c
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244036"
---
# <a name="xbox-info-api-reference"></a>Referenz zur API für Xbox-Informationen   
Sie können mit dieser API auf die Xbox One-Geräteinformationen zugreifen.

## <a name="get-xbox-one-device-information"></a>Abrufe von Xbox One-Geräteinformationen

## <a name="request"></a>Anforderung

Sie können Geräteinformationen zu Ihrer Xbox One abrufen.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/xbox/info

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

## <a name="response"></a>Antwort
Ein JSON-Objekt mit den folgenden Feldern:

* OsVersion (Zeichenfolge): Die Version des Betriebssystems.
* OsEdition (Zeichenfolge): Die Edition des Betriebssystems, z. B. „März 2017“ oder „März 2017 QFE 1“.
* ConsoleId (Zeichenfolge): Die ID der Konsole.
* DeviceId (Zeichenfolge): Die Xbox Live-Geräte-ID der Konsole.
* SerialNumber (Zeichenfolge): Die Seriennummer der Konsole.
* DevMode (Zeichenfolge): Der aktuelle Entwicklermodus der Konsole, z. B. „Keiner“ oder „Einzelhandel“.
* ConsoleType (Zeichenfolge): Der Konsolentyp, z. B. „Xbox One“ oder „Xbox One S“.
* DevkitCertificateExpirationTime (Zahl): Die UTC-Zeit in Sekunden, zu der das Developer Kit-Zertifikat der Konsole abläuft.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

**Verfügbare Gerätefamilien**

* Windows Xbox