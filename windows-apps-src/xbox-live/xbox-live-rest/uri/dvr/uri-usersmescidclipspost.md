---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
description: " POST (/users/me/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7a8973390ccbf5dd9980410f60f03a7edd78c134
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608785"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
Stellen Sie eine Anforderung des anfänglichen Uploads. Die Domänen für diese URIs sind `gameclipsmetadata.xboxlive.com` und `gameclipstransfer.xboxlive.com`, je nachdem, für die Funktion des betreffenden URI.
 
  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4EFB)
  * [Autorisierung](#ID4EQB)
  * [Erforderlichen Anforderungsheader](#ID4EKC)
  * [Optionale Anforderungsheader](#ID4ENE)
  * [Anforderungstext](#ID4ENF)
  * [Beispiel für eine Anforderung](#ID4E1F)
  * [HTTP-Statuscodes](#ID4EDG)
  * [Antworttext](#ID4EVAAC)
  * [Beispielantwort](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Hinweise
 
Dies ist der erste Teil des Hochladens GameClip. Bei der Erfassung eines Videos ansehen wird empfohlen, den GameClips Dienst sofort erhalten die ID und den URI für den Upload der Bits, aufrufen, auch wenn der Upload nicht geplant ist, um sofort zu starten. Dieser Aufruf führt Benutzer Kontingent-Überprüfungen und andere Prüfungen durch Content-Isolierung, Datenschutz, und so weiter, um festzustellen, ob sich ein Video für den Upload durch den Client auch geplant werden soll. Eine positive Antwort von diesen Aufruf gibt an, dass der Dienst den Videoclip für den Upload zu akzeptieren. Alle Clips hochgeladen müssen mit einem bestimmten Titel (über eine SCID) verknüpft werden soll, um im System akzeptiert werden.
 
Dieser Aufruf ist nicht Idempotent. nachfolgende Aufrufe bewirkt, dass unterschiedliche IDs und URIs ausgestellt werden. Wiederholungsversuche bei Fehler sollte es sich um eine clientseitige Backoff-Standardverhalten folgen.
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| string| Dienst-Config-ID der Ressource, die auf den zugegriffen wird. Muss die SCID des authentifizierten Benutzers entsprechen.| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>Autorisierung
 
Die folgenden Ansprüche sind erforderlich, damit diese Methode:
 
   * xuid
   * "DeviceType" - muss auf Gerät hochladen
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwerte: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispiele: 1, Vnext.| 
| Content-Type| string| MIME-Typ des Antworttexts. Beispiel: <b>Application/Json</b>.| 
| Annehmen| string| Die zulässigen Werte des Content-Type. Beispiel: <b>Application/Json</b>.| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Zulässige komprimierungscodierungen. Beispielwerte: Gzip, deflate, Identität.| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>Anforderungstext
 
Der Hauptteil der Anforderung muss ein [InitialUploadRequest](../../json/json-initialuploadrequest.md) Objekt im JSON-Format.
  
<a id="ID4E1F"></a>

 
## <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
{
   "clipNameStringId": "1193045",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
      
```

  
<a id="ID4EDG"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.| 
| 400| Ungültige Anforderung.| Im Hauptteil Anforderung ein Fehler aufgetreten, oder der Benutzer, die über das Kontingent ist.| 
| 401| Nicht autorisiert| Es ist ein Problem mit dem token Auth-Format in der Anforderung.| 
| 403| Verboten| Einige erforderliche Ansprüche sind nicht vorhanden, oder "DeviceType" ist nicht.| 
| 503| Nicht akzeptabel| Der Dienst oder eine downstream-Abhängigkeiten sind ausgefallen. Wiederholen Sie den standard-Backoff-Verhalten.| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>Antworttext
 
Die Antwort kann lauten ein [InitialUploadResponse](../../json/json-initialuploadresponse.md) Objekt oder ein [ServiceErrorResponse](../../json/json-serviceerrorresponse.md) Objekt im JSON-Format.
  
<a id="ID4EFBAC"></a>

 
## <a name="sample-response"></a>Beispielantwort
 

```cpp
{
   "clipName": "ClipName",
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752",  
   "titleName": "TitleName",
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
         
```

  
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

   