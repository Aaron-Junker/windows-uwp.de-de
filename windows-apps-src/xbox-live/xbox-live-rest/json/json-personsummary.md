---
title: PersonSummary (JSON)
assetID: 22fedb5f-5602-98d8-04a6-786fe3905921
permalink: en-us/docs/xboxlive/rest/json-personsummary.html
description: " PersonSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a787992507405a70185140e879be731d72806eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612625"
---
# <a name="personsummary-json"></a>PersonSummary (JSON)
Sammlung von [Person (JSON)](json-person.md) Objekte. 
<a id="ID4ER"></a>

 
## <a name="personsummary"></a>PersonSummary
 
PersonSummary Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| hasCallerMarkedTargetAsFavorite| Boolescher Wert| Gibt an, ob der Aufrufer das Ziel als Favorit markiert hat. Beispielwerte: "true"| 
| hasCallerMarkedTargetAsKnown| Boolescher Wert| Gibt an, ob der Aufrufer das Ziel als bezeichnet markiert. Beispielwerte: "true"| 
| isCallerFollowingTarget| Boolescher Wert| Gibt an, ob der Aufrufer das Ziel folgt. Beispielwerte: "true"| 
| isTargetFollowingCaller| Boolescher Wert| Gibt an, ob das Ziel den Aufrufer folgt. Beispielwerte: "true"| 
| legacyFriendStatus| string| Ältere Friend-Status des Ziels wie durch den Aufrufer. Dabei kann es sich um "None", "MutuallyAccepted", "OutgoingRequest" oder "IncomingRequest" sein. Beispielwerte: "MutuallyAccepted"| 
| recentChangeCount| 32-Bit-Ganzzahl ohne Vorzeichen| Optional. Die Anzahl der zuletzt vorgenommenen Änderungen in den sozialen Zieldiagramm. Dieser Wert ist nur vorhanden, wenn ein Benutzer ihre eigenen Zusammenfassung angezeigt wird. Beispielwerte: 5| 
| targetFollowerCount| > 32-Bit-Ganzzahl ohne Vorzeichen| Anzahl der Personen, die das Ziel eingehalten werden. Beispielwerte: 1308| 
| targetFollowingCount| 32-Bit-Ganzzahl ohne Vorzeichen| Anzahl der Personen, die das Ziel folgt. Beispielwerte: 112| 
| Wasserzeichen| string| Optional. Wasserzeichen der letzten Änderung für das Ziel. Dieser Wert ist nur vorhanden, wenn ein Benutzer ihre eigenen Zusammenfassung angezeigt wird. Beispielwerte: 5| 
  
<a id="ID4E4D"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}
    
```

  
<a id="ID4EGE"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EIE"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   