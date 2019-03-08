---
title: POST (/users/xuid({xuid})/devices/current/titles/current)
assetID: d5e2d12d-ba75-2d04-2805-c69a4c143f73
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentpost.html
description: " POST (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b29a0bc796712d7b7c44a1fe8512f99bf09eb4fc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649525"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
Aktualisieren Sie einen Titel mit Anwesenheit eines Benutzers ein. Die Domäne für diese URIs ist `userpresence.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EEB)
  * [Autorisierung](#ID4EPB)
  * [Erforderlichen Anforderungsheader](#ID4ENE)
  * [Optionale Anforderungsheader](#ID4ERG)
  * [Anforderungstext](#ID4ERH)
  * [Antworttext](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Dieser URI kann durch alle Titel nicht über die Konsole Plattformen verwendet werden, hinzufügen und aktualisieren das Vorhandensein, umfangreiche Präsenz und Mediendaten der Anwesenheit für einen Titel.
 
**SandboxId** wird jetzt von den Anspruch in der XToken abgerufen und erzwungen. Wenn die **SandboxId** ist nicht vorhanden ist, und klicken Sie dann die Unterhaltung Discovery Services (EDS) einen Fehler 400 Ungültige Anforderung ausgelöst wird.
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Zielbenutzers.| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Autorisierung
 
| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Ja| Xbox Benutzer-ID (XUID) des Aufrufers| 403 Forbidden| 
| titleId| Ja| TitleId des Titels| 403 Forbidden| 
| deviceId| Ja für alle mit Ausnahme von Windows und Web| Geräte-ID des Aufrufers| 403 Forbidden| 
| deviceType| Ja für alle außer Web| "DeviceType" des Aufrufers| 403 Forbidden| 
| sandboxId| Ja für Anrufe von | Sandbox des Aufrufers| 403 Forbidden| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| x-xbl-contract-version| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Beispielwerte: 3, Vnext.| 
| Content-Type| string| Der MIME-Typ, der den Hauptteil der Anforderung Beispielwert: Application/Json.| 
| Content-Length| string| Die Länge des Anforderungstexts. Beispielwert: 312.| 
| Host| string| Der Domänenname des Servers. Beispielwert: presencebeta.xboxlive.com.| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Das Anforderungsobjekt ist eine [TitleRequest](../../json/json-titlerequest.md). Es werden nur die Eigenschaften, die tatsächlich vorhanden ist, im Text aktualisiert. Alle Eigenschaften, vorhanden sind, nicht Teil des Texts jedoch auf dem Server werden nicht geändert werden.
 
<a id="ID4EAAAC"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{
  id:"12341234",
  placement:"snapped",
  state:"active"
}
      
```

   
<a id="ID4EKAAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Bei Erfolg wird ein HTTP-Statuscode 200 oder 201 Created zurückgegebenen, je nach Bedarf.
 
Bei einem Fehler (HTTP 4xx oder 5xx) wird der entsprechende Fehlerinformationen im Antworttext zurückgegeben.
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Marketplace-URIs](../marketplace/atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   