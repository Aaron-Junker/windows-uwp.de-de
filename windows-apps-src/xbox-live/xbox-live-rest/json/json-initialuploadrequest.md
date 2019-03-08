---
title: InitialUploadRequest (JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
description: " InitialUploadRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 2fbb2417f743d97b487ad16abd241fcb5eea62bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646635"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest (JSON)
Der Hauptteil einer POST-GameClip hochladen Anforderung. 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
InitialUploadRequest Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| string| Die Zeichenfolgen-ID für den Text als Namen für den Clip zu verwenden. Dies ist die verwaltet und in der Config-Datei für den Titel lokalisiert wird, vom Entwickler des Titels.| 
| <b>userCaption</b>| string| Optional. Alternative Benutzer eingegebener Name für Spiele Clip bis zu maximal 250 Zeichen zur Verfügung.| 
| <b>sessionRef</b>| string| Optional. Ein Verweis, der Game-Sitzung während der die Aufzeichnung abgeschlossen wurde.| 
| <b>dateRecorded</b>| DateTime| Die Zeit, die die Aufzeichnung, die können Sie auch in UTC angegeben gestartet wurde. Gemarshallt als Zeichenfolge im ISO 8601-Format (finden Sie unter <a href="https://www.w3.org/TR/NOTE-datetime">Datums- und Uhrzeitformate</a> Informationen).| 
| <b>durationInSeconds</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Die Länge des Clips in Sekunden.| 
| <b>expectedBlocks</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Optional. Anzahl der Blöcke, die in denen Datei geteilt werden soll. Lassen Sie Weg, wenn die Datei in einer einzelnen Anforderung gesendet wird.| 
| <b>fileSize</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Dateigröße in Bytes des Videos, die hochgeladen wird.| 
| <b>type</b>| [GameClipType-Enumeration](../enums/gvr-enum-gamecliptypes.md)| Der Typ der Clip gemarshallt als Zeichenfolgenwert der Enumeration, die durch Kommas getrennt wird.| 
| <b>source</b>| [GameClipSource-Enumeration](../enums/gvr-enum-gameclipsource.md)| Gibt an, wie der Clip stammt, als ein String-Wert der Enumeration gemarshallt.| 
| <b>visibility</b>| [GameClipVisibility-Enumeration](../enums/gvr-enum-gameclipvisibility.md)| Gibt die Sichtbarkeit des Spiels Clips an, nach der Veröffentlichung im System.| 
| <b>titleData</b>| string| Optional. Im Eigenschaftenbehälter für Titel-spezifischen Eigenschaften, die diesen Clip zugeordnet. Gespeichert und als zurückgegeben – ist. Titel-Entwickler können dieses Feld verwenden, um ihre eigenen Metadaten über einen Clip beizubehalten.| 
| <b>titleData</b>| string| Optional. Im Eigenschaftenbehälter für Konsole-spezifischen Eigenschaften, die diesen Clip zugeordnet. Gespeichert und als zurückgegeben – ist. Console-Plattform kann dieses Feld verwenden, um ihre eigenen Metadaten über einen Clip beizubehalten.| 
| <b>systemProperties</b>| string| Optional. Im Eigenschaftenbehälter für Konsole-spezifischen Eigenschaften, die diesen Clip zugeordnet. Gespeichert und zurückgegeben wird. Console-Plattform kann dieses Feld verwenden, um ihre eigenen Metadaten über einen Clip beizubehalten.| 
| <b>usersInSession</b>| Array von Zeichenfolgen| Optional. Eine Liste der Benutzer in der aktuellen Sitzung.| 
| <b>thumbnailSource</b>| [ThumbnailSource-Enumeration](../enums/gvr-enum-thumbnailsource.md)| Optional. Die Quelle der Miniaturansicht.| 
| <b>thumbnailOffsetMillseconds</b>| 32-Bit-Ganzzahl mit Vorzeichen| Gibt den Offset (in Millisekunden) für Offset generierte Miniaturansichten an. Nur wenn angegebene <b>ThumbnailSource</b> Offset festgelegt ist.| 
| <b>savedByUser</b>| Boolescher Wert| Optional. Legt fest, den Clip in das Kontingent des Benutzers anstelle der FIFO-Speicher gespeichert werden. Der Standardwert ist "false".| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
   "greatestMomentId": "123abc",
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

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   