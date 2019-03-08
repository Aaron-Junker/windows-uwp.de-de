---
title: User (JSON)
assetID: dbc733e4-0348-0e3d-1f55-17b465e599d6
permalink: en-us/docs/xboxlive/rest/json-user.html
description: " User (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 5c1b3a34ef329696d51e615dd79d57783a132d05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641355"
---
# <a name="user-json"></a>User (JSON)
Enthält die Bestenliste Benutzerdaten. 
<a id="ID4EN"></a>

 
## <a name="user"></a>Benutzer
 
Das User-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| Gamertag| string| Gamertag des Spielers (maximal 15 Zeichen). Der Client sollte dieser Wert in der Benutzeroberfläche verwenden, wenn den Spieler identifiziert.| 
| Rang| 32-Bit-Ganzzahl mit Vorzeichen| Der Rang des Benutzers relativ zu der Benutzer, der die Bestenliste Daten anfordert.| 
| rating| string| Die Bewertung des Benutzers.| 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Die Xbox User ID (XUID) des Benutzers.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{ 
   "gamertag":"TrueBlue402",
   "rank":2,
   "rating":"2:19:21.17",
   "xuid":1234567890123456 
}
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   