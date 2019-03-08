---
title: ResetReputation (JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
description: " ResetReputation (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: d09c8bbc1130f91dfea3d4c35e391dcf9adcf127
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649415"
---
# <a name="resetreputation-json"></a>ResetReputation (JSON)
Enthält die neuen Basis Reputation-Bewertungen, die in denen vorhandenen Spielstände eines Benutzers geändert werden soll. 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
ResetReputation Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| fairplayReputation| number| Die gewünschte basieren neue Fairplay Reputation-Wert für den Benutzer (gültiger Bereich 0 bis 75).| 
| commsReputation| number| Die gewünschte basieren neue aus Reputation-Wert für den Benutzer (gültiger Bereich 0 bis 75).| 
| userContentReputation| number| Die gewünschte basieren neue UserContent Reputation-Wert für den Benutzer (gültiger Bereich 0 bis 75).| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   