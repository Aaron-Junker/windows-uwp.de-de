---
title: POST (/titles/{Title Id}/sessionhosts)
assetID: 8558b336-1af9-8143-9752-477ceb3a8e4e
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts-post.html
description: " POST (/titles/{Title Id}/sessionhosts)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 47e3ecbf0a519b92ae467199e5d454523864310a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655145"
---
# <a name="post-titlestitle-idsessionhosts"></a>POST (/titles/{Title Id}/sessionhosts)
Erstellen Sie neue Clusteranforderung. Die Domäne für diese URIs ist `gameserverms.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Erforderlichen Anforderungsheader](#ID4EGB)
  * [Anforderungstext](#ID4E5B)
  * [Erforderlichen Antwortheader](#ID4ELD)
  * [Antworttext](#ID4ESD)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Beschreibung| 
| --- | --- | 
| titleId| Die ID des Titels, der die Anforderung angewendet werden soll.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Hostname

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
Wenn eine Anforderung ausführen, müssen die Header in der folgenden Tabelle gezeigt.
 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | 
| Content-Type| Anwendung/JSON| Typ der Daten, die gesendet wird.| 
  
<a id="ID4E5B"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Die Anforderung muss ein JSON-Objekt für die folgenden Elemente enthalten.
 
| Mitglied| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| Dies ist der Aufrufer mit dem angegebenen Bezeichner. Er ist an den Sitzungshost zugeordnet, die zugeordnet und zurückgegeben wird. Später können Sie die spezifischen Sessionhost von diesem Bezeichner verweisen. Es muss global eindeutig sein (d. h. GUID).| 
| SandboxId| Der Sandbox möchten Sie den Sitzungshost eingerichtet werden.| 
| cloudGameId| Der Cloud-game-Bezeichner.| 
| Speicherorte| Die geordnete Liste der bevorzugten Standorte, die die Sitzung, die aus zugeordnet werden sollen.| 
| sessionCookie| Dies ist ein Aufrufer angegeben nicht transparente Zeichenfolge. Er bezieht sich auf die Sessionhost und kann in den spielcode verwiesen werden. Verwenden Sie dieses Element, um eine kleine Menge an Informationen, die vom Client an den Server übergeben (die maximale Größe beträgt 4KB).| 
| gameModelId| Der Spielmodus-Bezeichner.| 
 
<a id="ID4EDD"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{
        "sessionId": "3536d3e6-e85d-4f47-b898-9617d19dabcd",
        "sandboxId": "ISST.0",
        "cloudGameId": "1b7f9925-369c-4301-b1f7-1125dce25776",
        "locations": [
        "West US",
        "East US",
        "West Europe"
        ],
        "sessionCookie": "Caller provided opaque string",
        "gameModeId": "2162d32c-7ac8-40e9-9b1f-56676b8b2513"
        }
      
```

   
<a id="ID4ELD"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
Keine
  
<a id="ID4ESD"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, gibt der Dienst ein JSON-Objekt mit den folgenden Elementen zurück.
 
| Mitglied| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| hostName| Der Hostname der Instanz.| 
| portMappings| Die portzuordnungen.| 
| region| Region der Instanz wird in gehostet.| 
| secureContext| Die Adresse für sicheres Gerät.| 
 
<a id="ID4ESE"></a>

 
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
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g=="
        }
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>Hinweise
 
Ein Titel sollte nur wiederholt den Aufruf an den Dienst, wenn die folgenden Antwortcodes empfangen werden:
 
   * 200: Erfolg: Antwort zurückgegeben.
   * 400 – Ungültige Parameter oder falsch formatierte Anforderungstext.
   * 401—Unauthorized
   * 404 – Titel-Id verfügt nicht über keine Abonnements zugewiesen.
   * 409 – Wenn identische Anforderung erfolgen (gleichen SessionId) zu ungefähr zur gleichen Zeit zu können, ist diese Antwort möglich. Ist ein Allocate angefordert wird und ein Host für Remotedesktopsitzungen bereits die angegebene SessionId und es bereits aktiv ist werden wir Informationen, die mit diesem Sessionhost zurück. Ist der Sitzungshost allerdings nicht aktiv, noch installieren müssen, erhalten Sie einen Konflikt.
   * 500 – Unerwarteter Serverfehler.
   * 503 – keine Sessionhosts StandingBy. Wiederholen Sie die Anforderung aus, wenn einige dieser Ressourcen kostenlos sind.
   
<a id="ID4EFG"></a>

 
## <a name="see-also"></a>Siehe auch
 [/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

  