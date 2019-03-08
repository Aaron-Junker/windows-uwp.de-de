---
title: GameClip (JSON)
assetID: 204cb702-4ce4-85a8-f231-3b4fb243405f
permalink: en-us/docs/xboxlive/rest/json-gameclip.html
description: " GameClip (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 4cfc2ac0a635e4aacdc9eeefb5097c6bd946a518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646505"
---
# <a name="gameclip-json"></a>GameClip (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclip"></a>GameClip
 
GameClip Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| <b>gameClipId</b>| string| Die ID des Spiels Clips zugewiesen.| 
| <b>state</b>| GameClipState| Der Status des Spiels Clips im System.| 
| <b>dateRecorded</b>| DateTime| Das Datum und Uhrzeit, die die Aufzeichnung, können Sie auch in UTC (ISO 8601-Format gestartet wurde).| 
| <b>lastModified</b>| DateTime| Uhrzeit des Spiels Clips oder seine Metadaten in UTC (ISO 8601-Format) der letzten Änderung.| 
| <b>userCaption</b>| string| Die neben der freien Eingabe nicht-lokalisierte Zeichenfolge für den Clip spielen.| 
| <b>type</b>| GameClipTypes| Der Typ des Clips. Mehrere Werte sind möglich, und werden durch Kommas getrennt sein, wenn dies der Fall.| 
| <b>source</b>| GameClipSource| Wie der Clip erstellt wurde.| 
| <b>visibility</b>| GameClipVisibility| Die Sichtbarkeit des Spiels Clips nach der Veröffentlichung im System.| 
| <b>durationInSeconds</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Die Dauer des Spiels Clips in Sekunden.| 
| <b>scid</b>| string| SCID, dem der Spiele Clip zugeordnet ist.| 
| <b>rating</b>| Die Gleitkommazahl mit doppelter Genauigkeit| Die Bewertung des Spiels Clips, im Bereich von 0,0 bis 5.0. zugeordnet.| 
| <b>ratingCount</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Die Anzahl der Häufigkeit, mit die dieser Clip bewertet wurde.| 
| <b>views</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Die Anzahl der Ansichten des Spiels Clips zugeordnet werden soll.| 
| <b>titleData</b>| string| Der Titel-spezifische Eigenschaftensammlung.| 
| <b>titleData</b>| string| Die Konsole-spezifische Eigenschaftensammlung.| 
| <b>thumbnails</b>| Array von GameClipThumbnail| Array von GameClipThumbnail-Objekten.| 
| <b>gameClipUris</b>| Array von GameClipUri| Array von GameClipUri-Objekten.| 
| <b>xuid</b>| string| Die XUID des Besitzers des Spiels Clips, als eine Zeichenfolge gemarshallt werden soll.| 
| <b>clipName</b>| string| Die lokalisierte Version des Clips Namen basierend auf dem Gebietsschema der Anforderung als Eingabe aus dem Verwaltungssystem Titel gesucht.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
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
   }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   