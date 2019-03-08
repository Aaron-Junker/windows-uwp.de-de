---
title: GET (/users/xuid({xuid})/history/titles)
assetID: c0a6cb3b-bab6-03b8-a79e-87defaaa6421
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesgetv2.html
description: " GET (/users/xuid({xuid})/history/titles)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: cadbf38385bbc321ef5bf23eb93c3fbc5c1a2417
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655585"
---
# <a name="get-usersxuidxuidhistorytitles"></a>GET (/users/xuid({xuid})/history/titles)
Ruft eine Liste von Titeln für die der Benutzer entsperrt oder Fortschritte auf seiner Erfolge. Diese API gibt keinen Titel wiedergegeben oder gestartet vollständigen Verlauf eines Benutzers zurück. Die Domäne für diese URIs ist `achievements.xboxlive.com`.
 
  * [URI-Parameter](#ID4EY)
  * [Abfragezeichenfolgen-Parameter](#ID4EDB)
  * [Autorisierung](#ID4EFD)
  * [Optionale Anforderungsheader](#ID4EGE)
  * [Anforderungstext](#ID4ERF)
 
<a id="ID4EY"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Benutzers, dessen Verlauf Titel zugegriffen wird.| 
  
<a id="ID4EDB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Erforderlich| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | 
| skipItems| Nein| 32-Bit-Ganzzahl mit Vorzeichen| Zurückgeben Sie Elemente, die ab dem die angegebene Anzahl von Elementen. Z. B. <b>SkipItems = "3"</b> ruft Elemente ab, mit dem vierten Element abgerufen. | 
| continuationToken| Nein| string| Gibt die Elemente, die beginnend ab des angegebenen Fortsetzungstokens zurück. | 
| "MaxItems"| Nein| 32-Bit-Ganzzahl mit Vorzeichen| Maximale Anzahl von Elementen aus der Auflistung zurückgegeben, die mit kombiniert werden können <b>SkipItems</b> und <b>"continuationtoken"</b> um einen Bereich von Elementen zurückzugeben. Der Dienst kann einen Standardwert angeben, wenn <b>"MaxItems"</b> ist nicht vorhanden, und möglicherweise weniger als zurück <b>"MaxItems"</b>, auch wenn die letzte Seite der Ergebnisse noch nicht zurückgegeben wurde. | 
  
<a id="ID4EFD"></a>

 
## <a name="authorization"></a>Autorisierung
 
| Anspruch| Erforderlich?| Beschreibung| Verhalten bei fehlen| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Benutzer| Aufrufer ist ein autorisierter Benutzer mit Xbox LIVE.| Der Aufrufer muss ein gültiger Benutzer auf Xbox LIVE.| 403 Forbidden| 
  
<a id="ID4EGE"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird.| 
| <b>x-xbl-contract-version</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Wenn Sie vorhanden und auf 2 festgelegt ist, wird die V2-Version dieser API verwendet werden. Andernfalls V1.| 
  
<a id="ID4ERF"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/history/titles](uri-titlehistoryusersxuidhistorytitlesv2.md)

  
<a id="ID4EPG"></a>

 
##### <a name="reference"></a>Verweis 

[UserTitle (JSON)](../../json/json-usertitlev2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [Paging-Parametern](../../additional/pagingparameters.md)

   