---
title: DELETE (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
description: " DELETE (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 391e4d79a389c358dea83509b52782d086201ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622835"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>DELETE (/users/me/scids/{scid}/clips/{gameClipId})
Delete Spiel Clip die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.
 
  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4ECB)
  * [Autorisierung](#ID4ENB)
  * [Erforderlichen Anforderungsheader](#ID4EYB)
  * [Optionale Anforderungsheader](#ID4EEE)
  * [Anforderungstext](#ID4ENF)
  * [HTTP-Statuscodes](#ID4EYF)
  * [Erforderlichen Antwortheader](#ID4EIAAC)
  * [Optionale Antwortheader](#ID4E2CAC)
  * [Antworttext](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Hinweise
 
Stellt einen Mechanismus für das Löschen des Benutzers Video aus dem GameClips-Dienst. Beim Löschen werden alle Metadaten und die tatsächliche Videoassets (generierte und ursprünglichen) aus dem System entfernt. Dies ist ein endgültiger Vorgang. 

> [!NOTE] 
> Die Besitzer-ID angegeben, muss den Aufrufer in der Autorisierungstoken für die Delete-Anforderung erfolgreich ist übereinstimmen. 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | 
| scid| string| Dienst-Config-ID der Ressource, die auf den zugegriffen wird. Muss die SCID des authentifizierten Benutzers entsprechen.| 
| gameClipId| string| GameClip-ID der Ressource, die auf den zugegriffen wird.| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>Autorisierung
 
Nur der Xuid-Anspruch ist für diese Methode erforderlich.
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwerte: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Zulässige komprimierungscodierungen. Beispielwerte: Gzip, deflate, Identität.| 
| ETag| string| Für Cache-Optimierung verwendet. Beispielwert: "686897696a7c876b7e".| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Keine Objekte werden im Text dieser Anforderung gesendet.
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| OK| Erfolgreiche Löschen des Clips.| 
| 401| Nicht autorisiert| Es ist ein Problem mit dem token Auth-Format in der Anforderung.| 
| 403| Verboten| Einige erforderliche Ansprüche vorhanden sind.| 
| 404| Nicht gefunden| Der in der URL angegebenen Clip war nicht vorhanden (oder es wurde ein zweites Mal gelöscht).| 
| 503| Nicht akzeptabel| Der Dienst oder eine downstream-Abhängigkeiten sind ausgefallen. Wiederholen Sie den standard-Backoff-Verhalten.| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>Erforderlichen Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung Verhalten beim Zwischenspeichern angeben.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
| Retry-After| string| Weist der Client im Fall einer nicht verfügbaren Server es später erneut versuchen.| 
| Variieren| string| Weist downstream-Proxys, wie zum Zwischenspeichern von Antworten.| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>Optionale Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| Für Cache-Optimierung verwendet. Beispiel: "686897696a7c876b7e".| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Der Dienst antwortet mit einer HTTP-Statuscode 204 (kein Inhalt) bei erfolgreicher Ausführung. Versuch, löschen Sie das gleiche Objekt oder eine nicht existierende Objekt gibt 404 zurück.
 
Wenn Fehler auftreten eine **ServiceErrorResponse** Objekt zurückgegeben.
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   