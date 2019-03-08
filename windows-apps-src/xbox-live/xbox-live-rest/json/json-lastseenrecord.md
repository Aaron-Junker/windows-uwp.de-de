---
title: LastSeenRecord (JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
description: " LastSeenRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: e06de31cabaedb68ed57d3d4f2ff30614ceb6317
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613375"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord (JSON)
Informationen, wenn das System einen Benutzer verfügbar, wenn der Benutzer keine gültige DeviceRecord hat zuletzt wurde. 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
LastSeenRecord Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| deviceType| string| Der Typ des Geräts auf dem der Benutzer zuletzt vorhanden war.| 
| titleId| 32-Bit-Ganzzahl ohne Vorzeichen| Der Bezeichner des Titels auf dem der Benutzer zuletzt vorhanden war.| 
| titleName| string| Der Name des Titels auf dem der Benutzer zuletzt vorhanden war.| 
| Zeitstempel| DateTime| UTC-Zeitstempel, der angibt, wenn der Benutzer zuletzt vorhanden war.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
  deviceType:W8,    
  titleId:"23452345",
  titleName:"My Awesome Game",
  timestamp:"2012-09-17T07:15:23.4930000"
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>Verweis   