---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
description: " GET (/qosservers)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 02d24dbf1d189b759784dbbfa7052e2c218ec27e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632065"
---
# <a name="get-qosservers"></a>GET (/qosservers)
URI, die von einem Client zum Abrufen der Liste der verfügbaren QoS-Server für die Verwendung mit Xbox Live Compute bezeichnet werden. Die Domänen für diese URIs sind `gameserverds.xboxlive.com` und `gameserverms.xboxlive.com`.
 
  * [Erforderlichen Anforderungsheader](#ID4EBB)
  * [Erforderlichen Antwortheader](#ID4EUC)
  * [Antworttext](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Hostname

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
Wenn eine Anforderung ausführen, müssen die Header in der folgenden Tabelle gezeigt.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | 
| Content-Type| Anwendung/JSON| Typ der Daten, die gesendet wird.| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | Die Länge des Anforderungsobjekts.| 
| x-xbl-contract-version| 1| API-Vertrag-Version.| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
Eine Antwort enthält immer die Header in der folgenden Tabelle gezeigt.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| Anwendung/JSON| Typ der Daten im Antworttext.| 
| Content-Length|  | Die Länge des Antworttexts.| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst ein JSON-Objekt mit den folgenden Elementen zurück.
 
| Mitglied| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| Ein Array von Serverinformationen.| 
| serverFqdn| Der vollqualifizierte Domänenname des Servers.| 
| serverSecureDeviceAddress| Die sicheres Gerät-Adresse des Servers.| 
| targetLocation| Der geografische Standort des Servers.| 
 
<a id="ID4EUE"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{ 
  "qosServers" : 
  [ 
    { "serverFqdn" : "xblqosncus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "North Central US" },
    { "serverFqdn" : "xblqoswus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "West US" },
  ]
}

      
```

   
<a id="ID4EBF"></a>

 
## <a name="see-also"></a>Siehe auch
 [/qosservers](uri-qosservers.md)

  