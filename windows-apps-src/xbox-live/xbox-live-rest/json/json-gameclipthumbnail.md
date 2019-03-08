---
title: GameClipThumbnail (JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
description: " GameClipThumbnail (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: ad2d35431cb4c40690978f4f3920f2e47f2b9bc0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606565"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail (JSON)
Enthält die Informationen im Zusammenhang mit einer einzelnen Miniaturansicht. Es kann mehrere Größen pro dem Clip vorhanden sein, und es obliegt dem Client, das richtige Zertifikat für die Anzeige auszuwählen. 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| <b>uri</b>| string| Der URI für die Miniaturansicht.| 
| <b>fileSize</b>| 32-Bit-Ganzzahl ohne Vorzeichen| Die Gesamtdateigröße der Miniaturansicht.| 
| <b>thumbnailType</b>| ThumbnailType| Der Typ der Miniaturansicht.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   