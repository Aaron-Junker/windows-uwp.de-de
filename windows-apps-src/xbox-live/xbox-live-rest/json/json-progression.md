---
title: Progression (JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
description: " Progression (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: dda124e5be9a4d21a1ee5b9d6130290207e31921
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593825"
---
# <a name="progression-json"></a>Progression (JSON)
Der Benutzer den Fortschritt für das Erreichen der Zertifikation entsperren. 
<a id="ID4EN"></a>

 
## <a name="progression"></a>Fortschritt
 
Das progressionsobjekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| Anforderungen| Array von Anforderungen| Die Anforderungen für das Erreichen der Zertifikation verdienen und wie weit der Benutzer auf die es entsperrt.| 
| timeUnlocked| DateTime| Die Zeit, die das Erreichen der Zertifikation zunächst entsperrt wurde.| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
  "requirements":
  [{
    "id":"12345678-1234-1234-1234-123456789012",
    "current":null,
    "target":"100"
  }],
  "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
}
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   