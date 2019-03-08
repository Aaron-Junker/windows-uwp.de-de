---
title: Person (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
description: " Person (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 175d66ffc7744ca8203fe7681fcb0167e150f012
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640865"
---
# <a name="person-json"></a>Person (JSON)
Metadaten über eine einzelne Person im System Personen. 
<a id="ID4EN"></a>

 
## <a name="person"></a>Person
 
Das Person-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| string| Erforderlich. Xbox User ID (XUID) im Dezimalformat. Beispielwert: 2603643534573573.| 
| isFavorite| Boolescher Wert| Erforderlich. Gibt an, ob diese Person eine ist, die den Benutzer mehr wichtig. Da Benutzer eine sehr große Anzahl von Personen in der Liste der Personen haben können, bevorzugte Personen in die Oberflächen Ihrer Priorität und angezeigt werden, bevor Sie anderen Benutzern, die keine Favoriten sind.| 
| isFollowingCaller| Boolescher Wert| Optional. Ob diese Person der Benutzer folgt in dessen Namen der API-Aufruf erfolgte.| 
| socialNetworks| Array von Zeichenfolgen| Optional. In der externen Netzwerken haben Benutzer und diese Person eine Beziehung.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>Verweis 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   