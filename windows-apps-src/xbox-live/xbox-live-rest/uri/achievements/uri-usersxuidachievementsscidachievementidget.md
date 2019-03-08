---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 19083937d48d8c312215f1734513d83c59f52f0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627505"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
Ruft die Details des Erfolgs ab. Die Domäne für diese URIs ist `achievements.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
  * [Autorisierung](#ID4EAB)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4E4C)
  * [Erforderlichen Anforderungsheader](#ID4EPG)
  * [Optionale Anforderungsheader](#ID4EPH)
  * [Anforderungstext](#ID4ECBAC)
  * [HTTP-Statuscodes](#ID4ENBAC)
  * [Antworttext](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Benutzers, dessen Ressource zugegriffen wird. Muss die XUID des authentifizierten Benutzers entsprechen.| 
| scid| GUID| Eindeutiger Bezeichner der Dienstkonfiguration aus, deren Leistung zugegriffen wird.| 
| achievementid| 32-Bit-Ganzzahl ohne Vorzeichen| (Innerhalb der angegebenen SCID) Eindeutiger Bezeichner des das Erreichen der Zertifikation, die auf den zugegriffen wird.| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>Autorisierung
 
Autorisierungsansprüche verwendet | Anspruch| Erforderlich?| Beschreibung| Verhalten bei fehlen| 
| --- | --- | --- | --- | --- | --- | --- | 
| Benutzer| Ja| Ein gültiger Benutzer auf Xbox LIVE für den die Anforderung gestellt wird.| 403 Forbidden| 
| Titel| Nein| Der aufrufende Titel.| Hängt von der AuthZ. Ab 1. Mai 2013 stellt AuthZ keinen Anspruch fehlt und wird daher Verweigern des Zugriffs auf alle SCIDs nicht als öffentlich markiert.| 
| Sandkasten| Nein| Die Sandbox aus der die Ergebnisse abgerufen werden soll.| Hängt von der AuthZ. Ab 1. Mai 2013 stellt AuthZ einen Standard-Anspruch nicht bereit, wenn nicht vorhanden.| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource
 
Auswirkungen der Datenschutzeinstellungen für Ressource | Anfordernden Benutzer| Ziel der Datenschutzeinstellungen des Benutzers| Verhalten| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Ich| -| Wie beschrieben.| 
| Friend| Alle Benutzer| OK| 
| Friend| Nur Freunde| OK| 
| Friend| gesperrt| Ist unzulässig.| 
| nicht-Friend-Benutzer| Alle Benutzer| OK| 
| nicht-Friend-Benutzer| Nur Freunde| Ist unzulässig.| 
| nicht-Friend-Benutzer| gesperrt| Ist unzulässig.| 
| Drittanbieter-Website| Alle Benutzer| OK| 
| Drittanbieter-Website| Nur Freunde| Ist unzulässig.| 
| Drittanbieter-Website| gesperrt| Ist unzulässig.| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.| 
| x-xbl-contract-version| string| Der Standardwert ist V1.| 
| Accept-Language| string| Liste der gewünschten Gebietsschemas und Fallbacks (z. B. "fr-FR", "fr", "En-GB", "En-WW", "En-US"). Der Dienst Erfolge funktioniert durch die Liste bis gefundenen übereinstimmenden lokalisierte Zeichenfolgen. Wenn keiner gefunden werden, versucht, die dem Standort entsprechen definiert im Benutzertoken, die über die IP-Adresse des Benutzers enthalten ist. Wenn noch keine übereinstimmenden lokalisierten Zeichenfolgen gefunden werden, wird die Standardzeichenfolgen, die vom Titel Entwickler/Herausgeber bereitgestellt. | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.| 
| 301| Permanent verschoben| Der Dienst wurde auf einen anderen URI verschoben.| 
| 307| Temporäre Umleitung| Der URI für diese Ressource wurde vorübergehend geändert werden.| 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt. von der MVC-Ebene abgelehnt werden sollte.| 
| 408| Anforderungstimeout| Die Anforderung dauerte zu lange dauert.| 
| 410| Aufgetreten| Die angeforderte Ressource ist nicht mehr verfügbar.| 
  
<a id="ID4EBGAC"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4EHGAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
    "achievement":
    {
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
                "requirements":null,
                "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }
}
         
```

   
<a id="ID4ERGAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ETGAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   