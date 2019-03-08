---
title: ActivityRequest (JSON)
assetID: 9eb03ab7-352c-4465-ec86-d544e76f63f9
permalink: en-us/docs/xboxlive/rest/json-activityrequest.html
description: " ActivityRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 791a566d278b92aeb34ab36d38719b44e9cc6c8f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628155"
---
# <a name="activityrequest-json"></a>ActivityRequest (JSON)
Eine Anforderung für Informationen zu einem oder mehreren Benutzern umfassende vorhanden. 
<a id="ID4EN"></a>

 
## <a name="activityrequest"></a>ActivityRequest
 
ActivityRequest Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| richPresence| [RichPresenceRequest](json-richpresencerequest.md)| Der Anzeigename der Zeichenfolge umfassender vorhanden, die verwendet werden soll.| 
| Medien| MediaRequest| Medieninformationen für was der Benutzer ansehen oder überwachen.| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f"
    }
  }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   