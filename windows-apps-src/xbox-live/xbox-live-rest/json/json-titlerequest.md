---
title: TitleRequest (JSON)
assetID: 43aeb6f9-726d-9260-e2ba-f005ea688bf1
permalink: en-us/docs/xboxlive/rest/json-titlerequest.html
description: " TitleRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a90f42c2f830ba6f04f77a1acaba067a2746a062
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593795"
---
# <a name="titlerequest-json"></a>TitleRequest (JSON)
Anfordern von Informationen über einen Titel. 
<a id="ID4EN"></a>

 
## <a name="titlerequest"></a>TitleRequest
 
TitleRequest Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| id| 32-Bit-Ganzzahl ohne Vorzeichen| Der Bezeichner des Titels.| 
| Aktivität| [ActivityRequest](json-activityrequest.md)| In-Titel-Informationen, einschließlich umfassende Informationen zu Vorhandensein und Medien, falls verfügbar.| 
| Status| string| Gibt an, ob ein Benutzer aktiv ist. Dieses Feld ist erforderlich, um einen Benutzer als inaktiv zu markieren. Der Standardwert ist "aktiv".| 
| Platzierung| string| Der Platzierungsmodus des Titels. Mögliche Werte sind "full", "füllen", "angedockt" oder "Hintergrund". Der Standardwert ist "full".| 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
  id:"12341234",
  placement:"snapped",
  state:"active",
  activity:
  {
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"82b11353-08ba-48ca-9f1a-21627b189b0f"
    }
  }
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>Verweis 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   