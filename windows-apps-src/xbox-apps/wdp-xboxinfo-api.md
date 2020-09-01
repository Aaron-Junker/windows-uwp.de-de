---
title: API-Referenz zum Geräte Portal Xbox Info
description: Erfahren Sie, wie Sie mithilfe der Get-Methode der Xbox-Geräte Portal-Rest-API auf Xbox One-Geräteinformationen zugreifen.
ms.date: 04/18/2019
ms.topic: article
keywords: Windows 10, UWP, Xbox, Geräte Portal
ms.localizationpriority: medium
ms.openlocfilehash: c2a4cecaf3340818b3679dfd3f64b9759ea46752
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174654"
---
# <a name="xbox-info-api-reference"></a>Xbox Info-API-Referenz   
Mit dieser API können Sie auf Xbox One-Geräteinformationen zugreifen.

## <a name="get-xbox-one-device-information"></a>Get Xbox One-Geräteinformationen

## <a name="request"></a>Anforderung

Sie können Geräteinformationen über Ihre Xbox One erhalten.

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

* OSVersion (Zeichenfolge) die Version des Betriebssystems.
* Oabdition-(Zeichenfolge) die Edition des Betriebssystems, z. b. "März 2017" oder "März 2017 QFE 1".
* Consoleid-(Zeichenfolge) die ID der Konsole.
* DeviceID-(Zeichenfolge) die Xbox Live-Geräte-ID der Konsole.
* SerialNumber-(Zeichenfolge) die Seriennummer der Konsole.
* DEVMODE-(Zeichenfolge) der aktuelle Entwicklermodus der Konsole, z. b. "None" oder "Retail".
* Consoletype-(Zeichenfolge) der Typ der Konsole, z. b. "Xbox One" oder "Xbox One s".
* Devkitcertificateexpirationtime-(Zahl) die UTC-Zeit in Sekunden, zu der das Developer Kit-Zertifikat der Konsole abläuft.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

**Verfügbare Gerätefamilien**

* Windows Xbox
