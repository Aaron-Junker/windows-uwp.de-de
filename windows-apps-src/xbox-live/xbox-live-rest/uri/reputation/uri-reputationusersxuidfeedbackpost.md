---
title: POST (/users/xuid({xuid})/feedback)
assetID: 48a7a510-a658-f2a3-c6bc-28a3610006e7
permalink: en-us/docs/xboxlive/rest/uri-reputationusersxuidfeedbackpost.html
description: " POST (/users/xuid({xuid})/feedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: d8a6e118811203fd34c310840e8688c2255c6beb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660045"
---
# <a name="post-usersxuidxuidfeedback"></a>POST (/users/xuid({xuid})/feedback)
Von Ihrem Titel verwendet, wenn Sie wünschen, Option "Feedback" in Ihrem Spiel, im Gegensatz zur Verwendung der Shell hinzufügen. Die Domäne für diese URIs ist `reputation.xboxlive.com`.
 
  * [URI-Parameter](#ID4EZ)
  * [Erforderlichen Anforderungsheader](#ID4EEB)
  * [Anforderungstext](#ID4ENC)
  * [Erforderliche Header](#ID4EDE)
  * [Autorisierung](#ID4EXF)
  * [HTTP-Statuscodes](#ID4EEG)
  * [Antworttext](#ID4EZH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| ulong| Xbox User ID (XUID) des Benutzers, der gemeldet wird.| 
  
<a id="ID4EEB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".| 
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 101.| 
  
<a id="ID4ENC"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
<a id="ID4EVC"></a>

 
### <a name="required-members"></a>Erforderliche Member 
 
Die Anforderung sollte enthalten eine [Feedback](../../json/json-feedback.md) Objekt. 
  
<a id="ID4EED"></a>

 
### <a name="prohibited-members"></a>Unzulässige Mitglieder 
 
Alle anderen Elemente sind in einer Anforderung nicht zulässig.
  
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung 
 

```cpp
{
    "sessionRef":
    {
        "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
        "templateName": "CaptureFlag5",
        "name": "Halo556932",
    },
    "feedbackType": "CommsAbusiveVoice",
    "textReason": "He called me a doodoo-head!",
    "voiceReasonId": null,
    "evidenceId": null
}

      
```

   
<a id="ID4EDE"></a>

 
## <a name="required-headers"></a>Erforderliche Header
 
Die folgenden Header sind erforderlich, wenn Sie eine Xbox Live Services-Anforderung ausführen.
 
| <b>Header</b>| <b>Wert</b>| <b>Deacription</b>| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| API-Vertrag-Version.| 
| Autorisierung| XBL3.0 x = [Hash]; [Token]| STS-Authentifizierungstoken. STSTokenString wird durch die von der Authentifizierungsanforderung zurückgegebene Token ersetzt.| 
Content-Type| 
Anwendung/JSON| 
Typ der Daten, die gesendet wird.| 
  
<a id="ID4EXF"></a>

 
## <a name="authorization"></a>Autorisierung
 
Die Anforderung muss einen gültige Xbox Live Authorization-Header enthalten. Wenn der Aufrufer den Zugriff auf diese Ressource nicht zulässig ist, gibt der Dienst einen 403 Verboten-Code. Wenn der Header ungültig oder nicht vorhanden ist, gibt der Dienst eine 401 nicht autorisierter Code.
  
<a id="ID4EEG"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| Kein Inhalt| Die Anforderung abgeschlossen wurde, jedoch keine Inhalte zurückgegeben.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout| Die Anforderung dauerte zu lange dauert.| 
| 407| Konflikt| Das Fortsetzungstoken ist nicht mehr gültig.| 
  
<a id="ID4EZH"></a>

 
## <a name="response-body"></a>Antworttext 
 
Wenn der Aufruf erfolgreich ist, werden keine Objekte aus dieser Antwort zurückgegeben. Der Dienst andernfalls eine [ServiceError](../../json/json-serviceerror.md) Objekt.
  
<a id="ID4EOAAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EQAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

  
<a id="ID4E3AAC"></a>

 
##### <a name="reference"></a>Verweis 

[Feedback (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   