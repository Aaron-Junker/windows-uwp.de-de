---
title: GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
assetID: 613ba53f-03cb-5ed3-a5ba-be59e5a146d1
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus-get.html
description: " GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 793d634bc1e3dc431b3797759751afb6dfd9c00a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650855"
---
# <a name="get-titlestitleidsessionssessionidallocationstatus"></a>GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
Gibt den Zuordnungsstatus der der Sessionhost identifizierte die SessionId zurück. Die Domänen für diese URIs sind `gameserverds.xboxlive.com` und `gameserverms.xboxlive.com`.
 
  * [Erforderlichen Anforderungsheader](#ID4E4)
  * [Erforderlichen Antwortheader](#ID4EEB)
  * [Antworttext](#ID4ELB)
 
<a id="ID4E4"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
Keine
  
<a id="ID4EEB"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
Keine
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst ein JSON-Objekt mit den folgenden Elementen zurück.
 
| Mitglied| Beschreibung| 
| --- | --- | 
| description| Gibt eine leere Zeichenfolge (links im für die Abwärtskompatibilität Kompatibilität).| 
| clusterId| Gibt eine leere Zeichenfolge (links im für die Abwärtskompatibilität Kompatibilität).| 
| hostName| Die URL des Hosts Sitzung.| 
| status| Gibt an, entweder in die Warteschlange eingereiht, erfüllt oder wurde abgebrochen.| 
| sessionHostId| Die ID der Sitzung Host.| 
| sessionId| Der Client (zum Zeitpunkt der Zuordnung) bereitgestellten Sitzungs-ID.| 
| secureContext| Die Adresse für sicheres Gerät.| 
| portMappings| Die portzuordnungen für die Instanz.| 
| region| Der Speicherort der Instanz.| 
| ticketId| Die aktuelle Sitzungs-ID (von Links für die Abwärtskompatibilität Kompatibilität).| 
| gameHostId| Die aktuelle SessionHostId (von Links für die Abwärtskompatibilität Kompatibilität).| 
 
<a id="ID4EGD"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
        "hostName": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.cloudapp.net",
        "portMappings": [
        {
        "Key": "GSDKTCPTest",
        "Value": {
        "internal": 10100,
        "external": 10103
        }
        },
        {
        "Key": "GSDKUDPTest",
        "Value": {
        "internal": 5000,
        "external": 5000
        }
        }
        ],
        "status",:"Fulfilled",
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g==",
        "sessionId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "description": "",
        "clusterId": "",
        "sessionHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0",
        "ticketId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "gameHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0"

      
```

  
<a id="remarks"></a>

 
### <a name="remarks"></a>Hinweise
 
Ein Titel sollte nur wiederholt den Aufruf an den Dienst, wenn die folgenden Antwortcodes empfangen werden:
 
   * 200 – Erfolg 
   * 400 – die Anforderung enthält ungültige Parameter 
   * 401—Unauthorized 
   * 404 – die Titel oder Ticket-ID war ungültig oder wurde nicht gefunden 
   * 500 – Unerwarteter Serverfehler. 
    