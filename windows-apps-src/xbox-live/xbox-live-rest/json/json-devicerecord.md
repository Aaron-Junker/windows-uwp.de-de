---
title: DeviceRecord (JSON)
assetID: aca4f4d3-f9b4-8919-5b6d-5ae0fe11e162
permalink: en-us/docs/xboxlive/rest/json-devicerecord.html
description: " DeviceRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 9746706c00a09cd8b64913b4ae8b5c3426551e48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646145"
---
# <a name="devicerecord-json"></a>DeviceRecord (JSON)
Informationen zu einem Gerät, einschließlich ihres Typs und die Titel auf sie aktiv. 
<a id="ID4EN"></a>

 
## <a name="devicerecord"></a>DeviceRecord
 
DeviceRecord Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| type| string| Der Device-Typ des Geräts. Sie können beispielsweise "D", "Der Xbox360", "MoLIVE" (Windows), "WindowsPhone", "WindowsPhone7" und "PC" (G4WL). Wenn der Typ (zum Beispiel für iOS, Android oder einen Titel, eingebettet in einem Webbrowser) bekannt ist, wird "Web" zurückgegeben.| 
| Titel| Array von [TitleRecord](json-titlerecord.md)| Die Liste der Titel, die auf diesem Gerät aktiv.| 
  
<a id="ID4EWB"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
[{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  }]
    
```

  
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ENC"></a>

 
##### <a name="reference"></a>Verweis   