---
title: QueryClipsResponse (JSON)
assetID: 5d668588-54d6-3cf3-20ad-bb2600a156b3
permalink: en-us/docs/xboxlive/rest/json-queryclipsresponse.html
description: " QueryClipsResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a0369de617fd60fcc71e7d69b46ced6b499e8de5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650505"
---
# <a name="queryclipsresponse-json"></a>QueryClipsResponse (JSON)
Umschließt die Liste der return Spiel Clips zusammen mit Informationen für die Liste an. 
<a id="ID4EN"></a>

 
## <a name="queryclipsresponse"></a>QueryClipsResponse
 
QueryClipsResponse Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| <b>gameClips</b>| Array von GameClip| Ein Array von Spielen Clips, die die Abfrage bis zu der Grenzwert für Anforderungen erfüllt (<b>"MaxItems"</b>).| 
| <b>pagingInfo</b>| PagingInfo| Enthält Informationen, die erforderlich sind für die Fortsetzung und Paging für nachfolgende Aufrufe für Listen, die die Anforderung Grenzwert überschreiten (<b>"MaxItems"</b>).| 
  
<a id="ID4E2B"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
 "gameClips": [
   {
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
     "eventId": "mymagicmomentABCDEFG",
     "type": "DeveloperInitiated, Achievement",
     "source": "TitleDirect",
     "visibility": "Default",
     "durationInSeconds": 30,
     "scid": "ecb5497e-76d4-4a8a-870d-e76a26306b7d",
     "rating": 1.0,
     "views": 5,
     "thumbnails": [
       {
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
     ]
   },
   {
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-12-23T12:00:00Z",
     "clipName": "Halo 4 Magic Moment",
     "userCaption": "You've been pwn3d!",
     "eventId": "123456",
     "type": "Achievement",
     "source": "Console",
     "visibility": "Public",
     "durationInSeconds": 50,
     "scid": "d4dd889f-813f-42cf-ad2f-3063e6ded066",
     "rating": 0.5,
     "views": 5,
     "properties": "{ 'Boss': 'The Invincible' }",
     "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
     "thumbnails": [
       {
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234567,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       },
       {
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/manifest",
         "fileSize": 0,
         "uriType": "SmoothStreaming",
         "expiration": "2013-01-18T11:25:51.6522794Z"
       }
     ]
   }
 ],
 "pagingInfo": {
   "continuationToken": "",
   "totalItems": 2
 }
}
    
```

  
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ESC"></a>

 
##### <a name="reference"></a>Verweis   