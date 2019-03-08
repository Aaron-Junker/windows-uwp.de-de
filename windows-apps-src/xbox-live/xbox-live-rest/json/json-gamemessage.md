---
title: GameMessage (JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
description: " GameMessage (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a2bddd9e26b4716fd1e33c4b5bbde56672b5d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651295"
---
# <a name="gamemessage-json"></a>GameMessage (JSON)
Ein jsonobjekt, das Definieren von Daten für eine Nachricht in einer Spiele-Sitzung Nachrichtenwarteschlange. 
<a id="ID4EN"></a>

  
 
Das GameMessage JSON-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| Daten| Array von 8-Bit-Ganzzahlen ohne Vorzeichen| Die Base64-codierte Daten, die möchte, dass der game-Client an den anderen Spiele Clients zu senden. Dieser Wert ist für den Server nicht transparent ist. | 
| senderXuid| 64-Bit-Ganzzahl ohne Vorzeichen| Die Xbox-Benutzer-ID des Players Senden der Nachricht. | 
| sequenceNumber| 32-Bit-Ganzzahl mit Vorzeichen| Die Sequenznummer der Nachricht spielen. Dieser Wert wird vom Server zugewiesen. Sequenznummern werden garantiert monoton erhöht werden, aber möglicherweise nicht aufeinander folgende. Sequenznummern sind eindeutig innerhalb einer Nachrichtenwarteschlange, jedoch nicht zwischen Meldungswarteschlangen. | 
| queueIndex| 32-Bit-Ganzzahl mit Vorzeichen| Der Index der Sitzung Nachrichten-Warteschlange für die Nachricht. Mögliche Werte sind 0 bis 3.| 
| timeStamp| DateTime| Zeitpunkt die game-Nachricht in der Warteschlange durch den Server in UTC erstellt wurde. | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
    "queueIndex": 0,
    "sequenceNumber": 5,
    "senderXuid": 65536,
    "timestamp": "2011-06-23T18:49:50Z",
    "data": null
}
    
```

  
<a id="ID4E1C"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E3C"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EGD"></a>

 
##### <a name="reference"></a>Verweis 

[GameSession (JSON)](json-gamesession.md)

   