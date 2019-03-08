---
title: GameSession (JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
description: " GameSession (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: ca7276ccdc13d896d19873811b4fa9df9a831cd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646515"
---
# <a name="gamesession-json"></a>GameSession (JSON)
Ein JSON-Objekt, das für eine Multiplayer-Sitzung Spieledaten darstellt. 
<a id="ID4ER"></a>

  
 
Das GameSession JSON-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| creationTime| DateTime| Das Datum und Zeit, wenn die Sitzung, in UTC angegeben erstellt wurde. | 
| customData| Array von 8-Bit-Ganzzahlen ohne Vorzeichen| 1024 Bytes der spezifischen Daten. Dieser Wert ist für den Server nicht transparent ist. | 
| displayName| string| Die Anzeige Namen des Spiels Sitzung mit einer maximalen Länge von 128 Zeichen lang sein. Dieser Wert ist für den Server nicht transparent ist. | 
| hasEnded| Boolescher Wert| True, wenn die Sitzung beendet wurde, und false, andernfalls. Wenn dieses Feld auf "true" markiert die Spiele-Sitzung als schreibgeschützt ist, verhindert, dass weitere Daten an die Sitzung übermittelt werden. | 
| isClosed| Boolescher Wert| True, wenn die Sitzung geschlossen wird und keine weitere Spieler können hinzugefügt werden, und false, andernfalls. Wenn dieser Wert auf "true" festgelegt ist, werden Anforderungen an der Sitzung beizutreten abgelehnt. | 
| maxPlayers| 32-Bit-Ganzzahl mit Vorzeichen| Maximale Anzahl von Playern geschützt, die gleichzeitig in der Sitzung sein kann. Der Wertebereich ist 2-16. Sobald die Sitzung die maximale Anzahl der Spieler enthält, werden weitere Anforderungen an der Sitzung beizutreten abgelehnt. | 
| playersCanBeRemovedBy| PlayerAcl| Ein Wert, der den Spieler soll mit der Berechtigung, andere Spieler aus der Sitzung zu entfernen. Mögliche Werte sind NoOne, Self-Service und AnyPlayer. | 
| Benutzerliste| Array von Spielern Objekte| Ein Array der Spieler in der Sitzung. Die Liste enthält die aktuellen Akteuren und Player, die zuvor in der Sitzung wurden, jedoch verlassen haben. Die Reihenfolge der Spieler in der Benutzerliste bleibt unverändert. Neue Spieler werden am Ende des Arrays hinzugefügt werden. | 
| seatsAvailable| 32-Bit-Ganzzahl mit Vorzeichen| Die Anzahl der Spieler, die die Sitzung, bevor die maximale Anzahl der Spieler erreicht, wird immer noch hinzugefügt werden soll. Dieser Wert ist schreibgeschützt, und es ist immer kleiner als der Wert des Felds MaxPlayers. | 
| sessionId| string| Die Sitzungs-ID, die von der MPSD zugewiesen werden, wenn die Sitzung erstellt wird. Dieser Wert wird in der Regel im URI enthalten, beim Zugriff auf die Informationen in einer Sitzung gespeichert.| 
| titleId| 32-Bit-Ganzzahl ohne Vorzeichen| Die ID des Titels die game-Sitzung erstellen.| 
| Variant| 32-Bit-Ganzzahl mit Vorzeichen| Die Spiele-Variante. Dieser Wert ist für den Server nicht transparent ist.| 
| visibility| VisibilityLevel| Ein Wert, der Sitzung Sichtbarkeit angibt. Mögliche Werte: PlayersCurrentlyInSession PlayersEverInSession und alle Benutzer.| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "maxPlayers": 4,
    "seatsAvailable": 4,
    "isClosed": false,
    "hasEnded": false,
    "roster": [],
    "visibility": "PlayersCurrentlyInSession",
    "playersCanBeRemovedBy": "Self",
 }
    
```

  
<a id="ID4ENF"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EPF"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>Verweis 

[GameMessage (JSON)](json-gamemessage.md)

 [GameSessionSummary (JSON)](json-gamesessionsummary.md)

 [Player (JSON)](json-player.md)

   