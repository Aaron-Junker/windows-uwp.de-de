---
title: DELETE (/users/xuid({xuid})/devices/current/titles/current)
assetID: 3bf75247-0a2a-0e4c-afcc-9e7654a89648
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentdelete.html
description: " DELETE (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: dd916fee5455276d45e4437e4ee90cacbde30bf9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604215"
---
# <a name="delete-usersxuidxuiddevicescurrenttitlescurrent"></a>DELETE (/users/xuid({xuid})/devices/current/titles/current)
Entfernen Sie das Vorhandensein einer schließenden Titel, anstatt abzuwarten, bis die [PresenceRecord](../../json/json-presencerecord.md) abläuft. Die Domäne für diese URIs ist `userpresence.xboxlive.com`.
 
  * [URI-Parameter](#ID4EZ)
  * [Autorisierung](#ID4EEB)
  * [Erforderlichen Anforderungsheader](#ID4ERD)
  * [Optionale Anforderungsheader](#ID4EVF)
  * [Anforderungstext](#ID4EVG)
  * [Antworttext](#ID4EAH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Zielbenutzers.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Autorisierung
 
| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Ja| Xbox Benutzer-ID (XUID) des Aufrufers| 403 Forbidden| 
| titleId| Ja| TitleId des Titels| 403 Forbidden| 
| deviceId| Ja für alle mit Ausnahme von Windows und Web| Geräte-ID des Aufrufers| 403 Forbidden| 
| deviceType| Ja für alle außer Web| "DeviceType" des Aufrufers| 403 Forbidden| 
  
<a id="ID4ERD"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| x-xbl-contract-version| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Beispielwerte: 3, Vnext.| 
| Content-Type| string| Der MIME-Typ, der den Hauptteil der Anforderung Beispielwert: Application/Json.| 
| Content-Length| string| Die Länge des Anforderungstexts. Beispielwert: 312.| 
| Host| string| Der Domänenname des Servers. Beispielwert: presencebeta.xboxlive.com.| 
  
<a id="ID4EVF"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.| 
  
<a id="ID4EVG"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EAH"></a>

 
## <a name="response-body"></a>Antworttext
 
Bei Erfolg wird die HTTP-Statuscode ohne Antworttext zurückgegeben.
 
Bei einem Fehler (HTTP 4xx oder 5xx) wird der entsprechende Fehlerinformationen im Antworttext zurückgegeben.
  
<a id="ID4ELH"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ENH"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

   