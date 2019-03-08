---
title: POST (/users/batchfeedback)
assetID: f94dcf19-a4e3-5bd0-5276-23e43bdcae52
permalink: en-us/docs/xboxlive/rest/uri-reputationusersbatchfeedbackpost.html
description: " POST (/users/batchfeedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 0906d32a0e15b2eaaf9c33e7f658e9e9f0cd5124
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622725"
---
# <a name="post-usersbatchfeedback"></a>POST (/users/batchfeedback)
Von der Titel des Diensts verwendet zum Senden von Feedback im Batch-Format außerhalb des Titels-Benutzeroberfläche. Die Domäne für diese URIs ist `reputation.xboxlive.com`.
 
  * [Anforderungstext](#ID4EX)
  * [Erforderliche Header](#ID4E3E)
  * [HTTP-Statuscodes](#ID4EWG)
  * [Antworttext](#ID4EDAAC)
 
<a id="ID4EX"></a>

 
## <a name="request-body"></a>Anforderungstext 
 
Aufrufer müssen ihr Zertifikat Ansprüchen im Abschnitt ClientCertificates ihrer Web-Request-Objekt enthalten.
 
<a id="ID4EBB"></a>

 
### <a name="required-members"></a>Erforderliche Member 
 
Die Anforderung muss ein Array von enthalten **BatchFeedback** Objekte. 
  
<a id="ID4EPB"></a>

 
### <a name="prohibited-members"></a>Unzulässige Mitglieder 
 
Alle anderen Elemente sind in einer Anforderung nicht zulässig.
  
<a id="ID4E3B"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung 
 

```cpp
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Killed 19 team members in a single session",
            "evidenceId": null
        },
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayQuitter",
            "textReason": "Quit 6 times from 9 sessions",
            "evidenceId": null
        }
    ]
}

      
```

 
| <b>Feld</b>| <b>JSON-Typ</b>| <b>Beschreibung</b>| 
| --- | --- | --- | 
| items| array| Eine Auflistung von Feedback JSON-Dokumente.| 
| targetXuid| string| Der Zielbenutzer XUID| 
| titleId| string| Der Titel, der von diesem Feedback gesendet wurde, oder NULL.| 
| sessionRef| object| Ein Objekt, das die Sitzung MPSD beschreibt bezieht sich auf dieses Feedback, oder NULL.| 
| feedbackType| string| Eine Zeichenfolgenversion eines Werts in der FeedbackType-Enumeration.| 
| textReason| string| Partner bereitgestellte Text, der der Absender hinzufügen kann, um weitere Details zu Ihrem Feedback zu geben, die gesendet wurde.| 
| evidenceId| string| Die ID der Ressource, die als Beweis für das Feedback übermittelten verwendet werden kann. z. B. die ID einer Videodatei.| 
   
<a id="ID4E3E"></a>

 
## <a name="required-headers"></a>Erforderliche Header
 
Die folgenden Header sind erforderlich, wenn Sie eine Xbox Live Services-Anforderung ausführen. 

> [!NOTE] 
> Ein Zertifikat des Hostpartners Ansprüche muss mit der Anforderung zum Übermitteln von Feedback für Batch gesendet werden, um. 


 
| Header| Wert| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| API-Vertrag-Version.| 
| Content-Type| Anwendung/JSON| Typ der Daten, die gesendet wird.| 
| Autorisierung| "XBL3.0 x=&lt;userhash>;&lt;token>"| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung.| 
| X-RequestedServiceVersion| 101| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter.| 
  
<a id="ID4EWG"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 500| Interner Serverfehler.| Der Server hat eine unerwartete Bedingung die Anforderung konnte.| 
| 503| Dienst nicht verfügbar.| Anforderung wurde gedrosselt, und wiederholen Sie dann die Anforderung der Client-Retry-Wert in Sekunden (z. B. 5 Sekunden später).| 
  
<a id="ID4EDAAC"></a>

 
## <a name="response-body"></a>Antworttext 
 
Wenn der Aufruf erfolgreich ist, werden keine Objekte aus dieser Antwort zurückgegeben. Der Dienst andernfalls eine [ServiceError](../../json/json-serviceerror.md) Objekt.
  
<a id="ID4EXAAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EZAAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

  
<a id="ID4EFBAC"></a>

 
##### <a name="reference"></a>Verweis 

[Feedback (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   