---
title: GET (/users/xuid({xuid})/achievements)
assetID: 381d49d1-7a4b-4a1e-1baf-cf674f7e0d54
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsgetv2.html
description: " GET (/users/xuid({xuid})/achievements)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 720170e88808db7ef95b88896fbca4f1cda4a091
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655265"
---
# <a name="get-usersxuidxuidachievements"></a>GET (/users/xuid({xuid})/achievements)
Ruft die Liste der Leistungen, die auf den Titel, die vom Benutzer entsperrt oder diejenigen, die der Benutzer in Bearbeitung befindlichen definiert. Die Domäne für diese URIs ist `achievements.xboxlive.com`.
 
  * [URI-Parameter](#ID4EX)
  * [Abfragezeichenfolgen-Parameter](#ID4ECB)
  * [Autorisierung](#ID4ENF)
  * [Erforderlichen Anforderungsheader](#ID4ESG)
  * [Optionale Anforderungsheader](#ID4ESH)
  * [Anforderungstext](#ID4EIBAC)
  * [Antworttext](#ID4ETBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Benutzers, dessen (Ressource) zugegriffen wird. Muss die XUID des authentifizierten Benutzers entsprechen.| 
  
<a id="ID4ECB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Erforderlich| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | 
| <b>skipItems</b>| Nein| 32-Bit-Ganzzahl mit Vorzeichen| Zurückgeben Sie Elemente, die ab dem die angegebene Anzahl von Elementen. Z. B. <b>SkipItems = "3"</b> ruft Elemente ab, mit dem vierten Element abgerufen. | 
| <b>continuationToken</b>| Nein| string| Gibt die Elemente, die beginnend ab des angegebenen Fortsetzungstokens zurück. | 
| <b>maxItems</b>| Nein| 32-Bit-Ganzzahl mit Vorzeichen| Maximale Anzahl von Elementen aus der Auflistung zurückgegeben, die mit kombiniert werden können <b>SkipItems</b> und <b>"continuationtoken"</b> um einen Bereich von Elementen zurückzugeben. Der Dienst kann einen Standardwert angeben, wenn <b>"MaxItems"</b> ist nicht vorhanden, und möglicherweise weniger als zurück <b>"MaxItems"</b>, auch wenn die letzte Seite der Ergebnisse noch nicht zurückgegeben wurde. | 
| <b>titleId</b>| Nein| string| Ein Filter für die zurückgegebenen Ergebnisse. Akzeptiert eine oder mehrere durch Kommas getrennt, decimal Title-Bezeichner.| 
| <b>unlockedOnly</b>| Nein| Boolescher Wert| Filter für die zurückgegebenen Ergebnisse. Wenn auf festgelegt <b>"true"</b>, werden nur Rückgabe der Erfolge für den Benutzer entsperrt. Standardmäßig <b>"false"</b>.| 
| <b>possibleOnly</b>| Nein| Boolescher Wert| Filter für die zurückgegebenen Ergebnisse. Wenn auf festgelegt <b>"true"</b>, alle möglichen Ergebnisse zurück, aber nicht nur entsperrt Metadaten – die Auszeichnung Informationen von XMS. Standardmäßig <b>"false"</b>.| 
| <b>types</b>| Nein| string| Ein Filter für die zurückgegebenen Ergebnisse. Kann "Persistent" oder "Abfrage". Der Standardwert ist alle unterstützten Typen.| 
| <b>orderBy</b>| Nein| string| Gibt die Reihenfolge, in dem die Ergebnisse zurückgegeben. "Ungeordnete", "Title", "UnlockTime" oder "EndingSoon" möglich. Der Standardwert ist "Ungeordnet".| 
  
<a id="ID4ENF"></a>

 
## <a name="authorization"></a>Autorisierung
 
| Anspruch| Erforderlich?| Beschreibung| Verhalten bei fehlen| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Benutzer| Aufrufer ist ein autorisierter Benutzer mit Xbox LIVE.| Der Aufrufer muss ein gültiger Benutzer auf Xbox LIVE.| 403 Forbidden| 
  
<a id="ID4ESG"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
  
<a id="ID4ESH"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Standardwert: 1.| 
| <b>x-xbl-contract-version</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Wenn Sie vorhanden und auf 2 festgelegt ist, wird die V2-Version dieser API verwendet werden. Andernfalls V1.| 
| <b>Accept-Language</b>| string| Liste der gewünschten Gebietsschemas und Fallbacks (z. B. "fr-FR", "fr", "En-GB", "En-WW", "En-US"). Der Dienst Erfolge funktioniert durch die Liste bis gefundenen übereinstimmenden lokalisierte Zeichenfolgen. Wenn keiner gefunden werden, versucht, die dem Standort entsprechen definiert im Benutzertoken, die über die IP-Adresse des Benutzers enthalten ist. Wenn noch keine übereinstimmenden lokalisierten Zeichenfolgen gefunden werden, wird die Standardzeichenfolgen, die vom Titel Entwickler/Herausgeber bereitgestellt. | 
  
<a id="ID4EIBAC"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4ETBAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Wenn der Aufruf erfolgreich ist, wird der Dienst gibt ein Array von [Auszeichnung (JSON)](../../json/json-achievementv2.md) Objekte und eine [PagingInfo (JSON)](../../json/json-paginginfo.md) Objekt.
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
    "achievements":
    [{
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
                "achievementState":"Achieved",
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
        }],
        "pagingInfo":
        {
                "continuationToken":null,
                "totalRecords":1
        }
}
         
```

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/achievements](uri-achievementsusersxuidachievementsv2.md)

  
<a id="ID4E2CAC"></a>

 
##### <a name="reference"></a>Verweis 

[Leistung (JSON)](../../json/json-achievementv2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [Paging-Parametern](../../additional/pagingparameters.md)

   