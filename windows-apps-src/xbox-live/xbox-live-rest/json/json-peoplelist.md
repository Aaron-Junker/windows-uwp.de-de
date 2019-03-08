---
title: PeopleList (JSON)
assetID: ac538652-c10c-44e5-c1e3-5314ebe8ba83
permalink: en-us/docs/xboxlive/rest/json-peoplelist.html
description: " PeopleList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1f9ab412088707752d62cc20fd54da2639f26ddc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613325"
---
# <a name="peoplelist-json"></a>PeopleList (JSON)
Sammlung von [Person](json-person.md) Objekte. 
<a id="ID4ER"></a>

 
## <a name="peoplelist"></a>PeopleList
 
PeopleList Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| Kontakte| Array von [Person](json-person.md)| Die [Person](json-person.md) Objekte, die die Liste der Personen zu bilden.| 
| totalCount| 32-Bit-Ganzzahl ohne Vorzeichen| Gesamtanzahl von [Person](json-person.md) Objekte in der Gruppe verfügbar. Dieser Wert kann von Clients verwendet werden, für paging, da es sich um die Größe des gesamten Satzes, nicht nur die letzte Antwort darstellt. Beispielwert: 680.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVC"></a>

 
##### <a name="reference"></a>Verweis 

[Abrufen (/ Users / {"ownerid"} / Benutzer)](../uri/people/uri-usersowneridpeopleget.md)

 [POST (/ Users / {"ownerid"} / People/Xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   