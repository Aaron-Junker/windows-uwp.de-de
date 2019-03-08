---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 5d2858b11bf8fb902ea07a016c8f41db375013f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640955"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
Erhalten Sie einen spielen Clip aus dem System, wenn alle IDs erleichtern bekannt sind. Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.
 
  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4EVB)
  * [Autorisierung](#ID4EAC)
  * [Erforderlichen Anforderungsheader](#ID4EUH)
  * [Optionale Anforderungsheader](#ID4EBCAC)
  * [Anforderungstext](#ID4ETDAC)
  * [HTTP-Statuscodes](#ID4E5DAC)
  * [Erforderlichen Antwortheader](#ID4EQIAC)
  * [Optionale Antwortheader](#ID4EJLAC)
  * [Antworttext](#ID4EJMAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Hinweise
 
Alle Daten Abfragen, für der Clip ist verfügbar in der **GameClip** Objekte aus einer Metadatenabfrage zurückgegeben. **XUID**, **ServiceConfigId**, **GameClipId** und **SandboxId** in den Ansprüchen der Anforderung sind erforderlich, um ein Spiel Clip über diese API zu erhalten. Wenn für die Erzwingung des Spiels Clips gekennzeichnet wurde, oder Inhaltsisolation oder des Datenschutzes feststellen überprüft, dass der Benutzer nicht über die Berechtigung zum Abrufen des bestimmten Spiel Clips verfügt, wird die API eine HTTP-Statuscode 404 (nicht gefunden) zurück.
 
**SandboxId** wird jetzt von den Anspruch in der XToken abgerufen und erzwungen. Wenn die **SandboxId** ist nicht vorhanden ist, und klicken Sie dann die Unterhaltung Discovery Services (EDS) einen Fehler 400 Ungültige Anforderung ausgelöst wird.
 
Diese API muss auch verwendet werden, um abgelaufene URIs zu aktualisieren. Wenn die Abfrage abgeschlossen ist, werden alle abgelaufenen URIs für die Spiele Clip entsprechend aktualisiert werden. 

> [!NOTE] 
> Eine URI-Aktualisierung dauert bis zu 30 bis 40 Sekunden nach dieser Anforderung abgeschlossen ist. Wenn der URI abgelaufen ist, erhalten es wird versucht, die sie sofort für streaming-Vorgänge verwendet einen HTTP 500-Statuscode aus dem IIS Smooth Streaming-Server. Wir arbeiten an Möglichkeiten, dies zu verkürzen, und dieser Hinweis entsprechend im Verlauf der Arbeit aktualisiert werden. 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | 
| ownerId| string| Die Benutzeridentität des Benutzers, dessen Ressource zugegriffen wird. Unterstützte Formate: "me" oder "xuid(123456789)". Maximale Länge: 16.| 
| scid| string| Dienst-Config-ID der Ressource, die auf den zugegriffen wird. Muss die SCID des authentifizierten Benutzers entsprechen.| 
| gameClipId| string| GameClip-ID der Ressource, die auf den zugegriffen wird.| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>Autorisierung
 
Autorisierungsansprüche verwendet | Anspruch| Typ| Erforderlich?| Beispielwert| Hinweise| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl mit Vorzeichen| Ja| 1234567890|  | 
| TitleId| 64-Bit-Ganzzahl mit Vorzeichen| Ja| 1234567890| Zum <b>Inhalte-Isolation</b> überprüfen.| 
| SandboxId| hexadezimale Binärdatei| Ja|  | Weist das System an den richtigen Bereich für die Suche auf, und zum <b>Inhalte-Isolation</b> überprüfen.| 
  
Auswirkungen der Datenschutzeinstellungen für Ressource | Anfordernden Benutzer| Ziel der Datenschutzeinstellungen des Benutzers| Verhalten| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Ich| -| Wie beschrieben.| 
| Friend| Alle Benutzer| Ist unzulässig.| 
| Friend| Nur Freunde| Ist unzulässig.| 
| Friend| gesperrt| Ist unzulässig.| 
| nicht-Friend-Benutzer| Alle Benutzer| Ist unzulässig.| 
| nicht-Friend-Benutzer| Nur Freunde| Ist unzulässig.| 
| nicht-Friend-Benutzer| gesperrt| Ist unzulässig.| 
| Drittanbieter-Website| Alle Benutzer| Ist unzulässig.| 
| Drittanbieter-Website| Nur Freunde| Ist unzulässig.| 
| Drittanbieter-Website| gesperrt| Ist unzulässig.| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwerte: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Zulässige komprimierungscodierungen. Beispielwerte: Gzip, deflate, Identität.| 
| ETag| string| Für Cache-Optimierung verwendet. Beispielwert: "686897696a7c876b7e".| 
| Bereich| string|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.| 
| 301| Permanent verschoben|  | 
| 307| Temporäre Umleitung|  | 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout| Die Anforderung dauerte zu lange dauert.| 
| 410| Aufgetreten| Die angeforderte Ressource ist nicht mehr verfügbar.| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.| 
| Retry-After| string| Weist der Client im Fall einer nicht verfügbaren Server es später erneut versuchen. Beispiel: <b>Application/Json</b>.| 
| Variieren| string| Weist downstream-Proxys, wie zum Zwischenspeichern von Antworten. Beispiel: <b>Application/Json</b>.| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>Optionale Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| Für Cache-Optimierung verwendet. Beispielwert: "686897696a7c876b7e".| 
  
<a id="ID4EJMAC"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4EPMAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

```cpp
{
 "gameClip": {
   "xuid": "2716903703773872",
   "clipName": "1234567890",
   "titleName": "",
   "gameClipId": "cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
   "state": "Published",
   "dateRecorded": "2013-05-08T21:32:17.4201279Z",
   "lastModified": "2013-05-08T21:34:48.8117829Z",
   "userCaption": "",
   "type": "DeveloperInitiated",
   "source": "Console",
   "visibility": "Public",
   "durationInSeconds": 30,
   "scid": "00000000-0000-0012-0023-000000000070",
   "titleId": 0,
   "rating": 0,
   "ratingCount": 0,
   "views": 0,
   "titleData": "",
   "systemProperties": "",
   "savedByUser": false,
   "thumbnails": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/large",
       "fileSize": 0,
       "thumbnailType": "Large"
     },
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/small",
       "fileSize": 0,
       "thumbnailType": "Small"
     }
   ],
   "gameClipUris": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
       "fileSize": 9332015,
       "uriType": "Download",
       "expiration": "9999-12-31T23:59:59.9999999"
     }
   ]
 }
}
         
```

   
<a id="ID4EZMAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E2MAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

  
<a id="ID4EFNAC"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Marketplace-URIs](../marketplace/atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   