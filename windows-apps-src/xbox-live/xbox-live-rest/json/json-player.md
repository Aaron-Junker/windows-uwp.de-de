---
title: Player (JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
description: " Player (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a5967cbfecd47c5675926bd45939442c45dda7b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622495"
---
# <a name="player-json"></a>Player (JSON)
Enthält Daten für einen Player in einer Spiele-Sitzung an. 
<a id="ID4EN"></a>

 
## <a name="player"></a>Player
 
Das Player-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| customData| Array von 8-Bit-Ganzzahl ohne Vorzeichen| 1024 Bytes der Base64-codierte spezifischen Player-Daten. Dieser Wert ist für den Server nicht transparent ist.| 
| Gamertag| string| B. Gamertag, maximal 15 Zeichen, des Spielers. Der Client sollte dieser Wert in der Benutzeroberfläche verwenden, wenn den Spieler identifiziert. | 
| isCurrentlyInSession| Boolescher Wert| Gibt an, ob der Spieler befindet sich derzeit in der Sitzung oder die Sitzung bleibt.| 
| seatIndex| 32-Bit-Ganzzahl mit Vorzeichen| Der Index des Players in der Sitzung.| 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Die Xbox User ID (XUID) des Spielers.| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "xuid": 2533274790412952,
    "gamertag":"MyTestUser",
    "seatindex": 3
    "customData":"AIHJ2?iE?/jiKE!l5S=T..."
    "isCurrentlyInSession":"true"
}
    
```

  
<a id="ID4EFD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EHD"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>Verweis 

[GameSession (JSON)](json-gamesession.md)

   