---
title: XuidList (JSON)
assetID: 06938a52-e582-a15b-ec7f-4b053dfc28ad
permalink: en-us/docs/xboxlive/rest/json-xuidlist.html
description: " XuidList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1d8172063d40f8df77827ab845c4dfd0c0799811
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627915"
---
# <a name="xuidlist-json"></a>XuidList (JSON)
Liste der XUIDs für den einen Vorgang ausgeführt werden soll. 
<a id="ID4EN"></a>

 
## <a name="xuidlist"></a>XuidList
 
XuidList Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| xuids| Array von Zeichenfolgen| Liste von Werten Xbox-Benutzer-ID (XUID), klicken Sie auf dem ein Vorgang ausgeführt werden soll, oder Daten zurückgegeben werden sollen.| 
  
<a id="ID4EMB"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
    
```

  
<a id="ID4EVB"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EXB"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EBC"></a>

 
##### <a name="reference"></a>Verweis 

[POST (/ Users / {"ownerid"} / People/Xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   