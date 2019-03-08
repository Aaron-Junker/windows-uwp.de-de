---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 89f3b53631f5570ab6d0d0619f6678fc3e3c2dd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645825"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
Spiele Clip-Metadaten für die Daten des Benutzers aktualisiert. Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.
 
  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4EAB)
  * [Erforderlichen Anforderungsheader](#ID4ELB)
  * [Optionale Anforderungsheader](#ID4EXD)
  * [Anforderungstext](#ID4EAF)
  * [Erforderlichen Antwortheader](#ID4EVF)
  * [Optionale Antwortheader](#ID4EJAAC)
  * [Antworttext](#ID4EJBAC)
  * [Verwandte URIs](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Hinweise
 
Die API zum Aktualisieren von Metadaten des Spiels Clip fällt in zwei Kategorien unterteilt, Aktualisieren der Metadaten einen eigenen Spiele Clips wie Eingabehilfen und Titel oder Aktualisieren von den öffentlichen Attributen (z. B. eine Bewertung anwenden oder erhöhen die Anzahl der Seitenansichten) von einem beliebigen anderen Spiele Clip. Wenn die XUID im URI der XUID im Anspruch nicht übereinstimmt, wird nur die öffentlichen Daten bearbeitet werden können, und verweigert alle Anforderungen an den anderen Daten bearbeiten. Im Fall versuchen mehrere Felder, die bearbeitet werden und eine davon für die Anforderung ungültig ist, würde die gesamte Anforderung fehlschlagen.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| string| Dienst-Config-ID der Ressource, die auf den zugegriffen wird. Muss die SCID des authentifizierten Benutzers entsprechen.| 
| gameClipId| string| GameClip-ID der Ressource, die auf den zugegriffen wird.| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwerte: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Zulässige komprimierungscodierungen. Beispielwerte: Gzip, deflate, Identität.| 
| ETag| string| Für Cache-Optimierung verwendet. Beispielwert: "686897696a7c876b7e".| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Der Hauptteil der Anforderung muss ein [UpdateMetadataRequest](../../json/json-updatemetadatarequest.md) Objekt im JSON-Format. Beispiele:
 
Clip-Benutzernamen und Sichtbarkeit ändern:
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
Ändern nur die Title-Eigenschaften (Dies ist nur ein Beispiel, da das Schema der dieses Feld hängt vom Aufrufer ist):
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
| Retry-After| string| Weist der Client im Fall einer nicht verfügbaren Server es später erneut versuchen.| 
| Variieren| string| Weist downstream-Proxys, wie zum Zwischenspeichern von Antworten.| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>Optionale Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| Für Cache-Optimierung verwendet. Beispiel: "686897696a7c876b7e".| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Bei der erfolgreicher Aktualisierung der Metadaten einer HTTP-Statuscode 200 wird zurückgegeben werden.
 
Andernfalls wird ein ServiceErrorResponse-Objekt im JSON-Format mit einem entsprechenden HTTP-Statuscode zurückgegeben.
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>Verwandte URIs
 
Aktualisieren Sie die folgenden URIs öffentliche Felder in den Metadaten. Es ist kein Nachrichtentext erforderlich, diese Anforderungen. Bei der erfolgreicher Aktualisierung der Metadaten einer HTTP-Statuscode 200 wird zurückgegeben werden. Andernfalls wird ein ServiceErrorResponse-Objekt im JSON-Format mit einem entsprechenden HTTP-Statuscode zurückgegeben.
 
   * **POST/Users / {"ownerid"} {scid}-/scids/ /clips/ {GameClipId} /ratings/ {Bewertungswert}** -wendet die angegebene Bewertung auf den angegebenen Clip. Bewertung der Wert muss eine ganze Zahl zwischen 1 und 5.
   * **POST/Users / {"ownerid"} {scid}-/scids/ /clips/ {GameClipId} / flag** -kennzeichnet den Clip, um die fragwürdige Inhalt vom Erzwingung überprüft werden.
   * **POST/Users / {"ownerid"} {scid}-/scids/ /clips/ {GameClipId} / Ansichten** -inkrementiert die Ansichtenanzahl für den angegebenen Spiel Clip. Es wird empfohlen, dass dies nicht aufgerufen wird rechts bei der Wiedergabe gestartet wird, aber sobald 75 % - 80 % der Wiedergabe abgeschlossen ist.
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   