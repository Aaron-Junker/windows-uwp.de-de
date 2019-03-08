---
title: GameResult (JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
description: " GameResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b408b1aaae5e6f54958a016575c4a2c37765f1e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602335"
---
# <a name="gameresult-json"></a>GameResult (JSON)
Ein JSON-Objekt, die Daten, die beschreibt, die Ergebnisse einer Spiele-Sitzung darstellt. 
<a id="ID4EN"></a>

  
 
Das GameResult JSON-Objekt hat die folgenden Elemente.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| blob| Array von 8-Bit-Ganzzahlen ohne Vorzeichen| Der benutzerdefinierten Titel-spezifischen Ergebnisdaten.| 
| Ergebnis| string| Das Ergebnis des Spielers Beteiligung in der game-Sitzung. Gültige Werte sind "Win", "Verlust" oder "Verknüpfen". | 
| Ergebnis| 64-Bit-Ganzzahl mit Vorzeichen| Das Ergebnis, die der Spieler in der game-Sitzung empfangen.| 
| Zeit| 64-Bit-Ganzzahl mit Vorzeichen| Der Player Zeit für die Spiele-Sitzung.| 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Die Xbox-Benutzer-ID des Players für die die Ergebnisse gelten.| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
   "xuid": 2533274790412952,
   "outcome": "Win",
   "score": 100
}
    
```

  
<a id="ID4EYC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E1C"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   