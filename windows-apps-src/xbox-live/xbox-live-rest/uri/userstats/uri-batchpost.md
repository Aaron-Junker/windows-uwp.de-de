---
title: POST (/batch)
assetID: f5997c8e-82c7-0fba-9991-ce1116db4830
permalink: en-us/docs/xboxlive/rest/uri-batchpost.html
description: " POST (/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a854fc830c87afbf675a379599916bf3db919539
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645835"
---
# <a name="post-batch"></a>POST (/batch)
POST-Methode, der als eine GET-Methode für komplexe Batchanforderungen für mehrere Spieler Statistiken über mehrere Titel fungiert. Die Domäne für diese URIs ist `userstats.xboxlive.com`.
 
<a id="ID4ET"></a>

 
## <a name="remarks"></a>Hinweise
 
Titel-Entwickler können die Statistiken als offen oder eingeschränkt XDP oder Partner Center kennzeichnen. Leaderboards sind Statistiken öffnen. Statistiken öffnen können durch Smartglass, als auch iOS, Android, Windows, Windows Phone und Webanwendungen zugegriffen werden, solange der Benutzer mit der Sandbox autorisiert ist. Benutzerautorisierung einer Sandbox wird über XDP oder über das Partner Center verwaltet.
  
  * ["Hinweise"](#ID4ET)
  * ["Hinweise"](#ID4EFB)
  * [Autorisierung](#ID4EUB)
  * [Erforderlichen Anforderungsheader](#ID4ETC)
  * [Optionale Anforderungsheader](#ID4E3D)
  * [Anforderungstext](#ID4EAF)
  * [HTTP-Statuscodes](#ID4EWF)
  * [Antworttext](#ID4ENBAC)
 
<a id="ID4EFB"></a>

 
## <a name="remarks"></a>Hinweise
 
Der Aufrufer bietet ein Array von Benutzern, die Dienstkonfiguration IDs (SCIDs) und eine Liste der Namen von Statistiken pro SCIDs für das Abrufen dieser Statistiken einen Nachrichtentext.
 
Umständen ist es sinnvoller, überprüfen die einfache, einzelne-Statistik [erhalten](uri-usersxuidscidsscidstatsget.md) -Methode auf, bevor diese komplexer, im Batchmodus-Seite zu lesen.
  
<a id="ID4EUB"></a>

 
## <a name="authorization"></a>Autorisierung
 
Es gibt Autorisierungslogik für Inhalte-Isolation und Access Control-Szenarios implementiert.
 
   * Sowohl Benutzer als auch Bestenlisten Statistiken können von Clients auf allen Plattformen, gelesen werden, vorausgesetzt, dass der Aufrufer ein gültiges XSTS-Token mit der Anforderung sendet. Schreibvorgänge sind offensichtlich auf unterstützten Clients begrenzt die.
   * Titel-Entwickler können die Statistiken als offen oder eingeschränkt XDP oder Partner Center kennzeichnen. Leaderboards sind Statistiken öffnen. Statistiken öffnen können durch Smartglass, als auch iOS, Android, Windows, Windows Phone und Webanwendungen zugegriffen werden, solange der Benutzer mit der Sandbox autorisiert ist. Benutzerautorisierung einer Sandbox wird über XDP oder über das Partner Center verwaltet.
  
Das folgende Beispiel ist der Pseudocode für die Überprüfung:
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4ETC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
  
<a id="ID4E3D"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>Anforderungstext
 
<a id="ID4EIF"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 
Die folgende POST-Text wird dem Dienst mitgeteilt, aus zwei verschiedenen SCIDs für zwei verschiedene Benutzer vier Statistiken angefordert werden.
 

```cpp
{    
"requestedusers": [
                1234567890123460,
                1234567890123234
            ],
            "requestedscids": [
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c1212",
                    "requestedstats": [
                        "Test4FirefightKills",
                        "Test4FirefightHeadshots"
                    ]
                },
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c0343",
                    "requestedstats": [
                        "OverallTestKills",
                        "TestHeadshots"
                    ]
                }
            ] 
}
      
```

   
<a id="ID4EWF"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.| 
| 304| Nicht geändert.| Ressource nicht geändert seit dem letzten angefordert.| 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout| Die Ressourcenversion wird nicht unterstützt. von der MVC-Ebene abgelehnt werden sollte.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4EXBAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{    
   "users":[          
       {    
      "xuid": "123456789"
        "gamertag": "WarriorSaint",
        "scids":[
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c1212",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":7
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":4
             }]
                        },
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c0343",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":3434
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":41
             }]
          }],
                   },
    {    
                   "gamertag":"TigerShark",
                   "xuid":1234567890123234
        "scids":[
          {
             "scid":"123456789",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":63
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":12
             }]
                        },
          {
"scid":"987654321",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":375
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":34
             }]
          }],
                   }]
}
         
```

   
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>Parent 

[/batch](uri-batch.md)

   