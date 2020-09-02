---
title: API-Referenz zum Geräte Portal http-Monitor
description: Erfahren Sie, wie Sie mithilfe der Rest-API des/ext/httpmonitor/Sessions Xbox-Geräte Portals auf Echt Zeit-HTTP-Datenverkehr von der fokussierten App auf einer Xbox zugreifen.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 6ea53f6356aa89a83f3b267a65f65b32aad5749d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304532"
---
# <a name="http-monitor-api-reference"></a>HTTP-Monitor-API-Referenz   
Sie können über diese API auf Echt Zeit-HTTP-Datenverkehr für die fokussierte App zugreifen, wenn der HTTP-Monitor in der Xbox-Konsole aktiviert wurde, indem Sie das Kontrollkästchen in der Entwicklungs Homepage aktivieren.

## <a name="get-if-the-http-monitor-is-enabled"></a>Get, wenn der HTTP-Monitor aktiviert ist

**Anforderung**

Sie können festnehmen, ob der HTTP-Monitor in der Entwicklungs Homepage aktiviert wurde.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/httpmonitor/sessions

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Auf**   
Ein JSON-Objekt mit den folgenden Feldern:

* Aktiviert-(bool) gibt an, ob der HTTP-Monitor in der Xbox-Konsole aktiviert wurde, indem Sie das Kontrollkästchen in dev Home aktivieren.

**Status Code**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="get-http-traffic-from-the-focused-app"></a>HTTP-Datenverkehr aus der fokussierten APP erhalten

**Anforderung**

HTTP-Datenverkehr von der fokussierten App auf der Xbox, sofern es sich nicht um eine System-App handelt, in Echtzeit, wenn der HTTP-Monitor von dev Home aus aktiviert wurde.

Methode      | Anforderungs-URI
:------     | :-----
WebSocket | /ext/httpmonitor/sessions

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Auf**   
Ein JSON-Objekt mit den folgenden Feldern:

* Sitzungen
    * Requestheaders-(JSON-Objekt) die Anforderungs Header aus der HTTP-Anforderung.
    * Requestcontentheaders-(JSON-Objekt) die Anforderungs Inhalts Header aus der HTTP-Anforderung.
    * RequestURL-(Zeichenfolge) die Anforderungs-URL.
    * Requestmethod-(Zeichenfolge) die Anforderungs Methode.
    * RequestMessage-(Zeichenfolge) die Anforderungs Nachricht, die derzeit nur JSON-und Textinhalte unterstützt.
    * ResponseHeaders-(JSON-Objekt) die Antwortheader aus der HTTP-Antwort.
    * Responsecontentheaders-(JSON-Objekt) die Antwort Inhalts Header aus der HTTP-Antwort.
    * Statuscode-(Zahl) der Antwortstatus Code.
    * Reasponanphrase-(String) der Antwort Grund Ausdruck.
    * Response Message-(Zeichenfolge) die Antwortnachricht, die derzeit nur JSON-und Textinhalte unterstützt.

**Status Code**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
403 | Der HTTP-Monitor ist deaktiviert, muss in der Entwicklungs Homepage aktiviert werden.
5XX | Fehlercodes


**Verfügbare Gerätefamilien**

* Windows Xbox