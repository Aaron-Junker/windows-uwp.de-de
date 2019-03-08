---
title: InitialUploadResponse (JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
description: " InitialUploadResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: dab59fefb389cf550a1bc4fc6429f6b0970f50ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589835"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse (JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| <b>gameClipId</b>| string| ID für die Anforderung zum Hochladen von Daten zugeordnet wird.| 
| <b>uploadUri</b>| URI| Der Speicherort, der der Spiele Clip hochgeladen werden soll.| 
| <b>largeThumbnailUri</b>| URI| Optional. Der Speicherort, der die große Miniaturansicht hochgeladen werden soll. Vorhandensein dieses Feld richtet sich nach der [ThumbnailSource Enumeration](../enums/gvr-enum-thumbnailsource.md) Wert in der <b>InitialUploadRequest</b> (vorhanden, wenn der Upload angegeben, werden.).| 
| <b>smallThumbnailUri</b>| URI| Optional. Der Speicherort auf dem die Miniaturansicht hochgeladen werden sollen. Vorhandensein dieses Feld richtet sich nach der [ThumbnailSource Enumeration](../enums/gvr-enum-thumbnailsource.md) Wert in der <b>InitialUploadRequest</b> (vorhanden, wenn der Upload angegeben, werden.).| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752"  ,  
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
    
```

  
<a id="ID4EBD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EDD"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   