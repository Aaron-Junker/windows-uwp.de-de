---
title: MatchTicket (JSON)
assetID: 12617677-47f2-e517-af53-5ab9687eea2a
permalink: en-us/docs/xboxlive/rest/json-matchticket.html
description: " MatchTicket (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 4bc638dfe7735856295ed92f35e244213be7bc1e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608335"
---
# <a name="matchticket-json"></a>MatchTicket (JSON)
Ein JSON-Objekt, das eine Match-Ticket, das vom Spieler zum Auffinden von anderen Spielern über das Verzeichnis Multiplayer-Sitzung (MPSD) verwendet darstellt. 
<a id="ID4EN"></a>

  
 
Das MatchTicket JSON-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| serviceConfig| GUID| Bezeichner der Service-Konfiguration (SCID) für die Sitzung.| 
| hopperName| string| Der Name des der Hopper, in dem dieses Ticket platziert werden soll.| 
| giveUpDuration| 32-Bit-Ganzzahl mit Vorzeichen| Maximale Wartezeit (ganzzahlige Anzahl von Sekunden).| 
| preserveSession| Enumeration| Ein Wert, der angibt, ob die Sitzung als die Sitzung in die entsprechend wiederverwendet werden muss. Mögliche Werte sind "immer" oder "nie". | 
| ticketSessionRef| MultiplayerSessionReference| <b>MultiplayerSessionReference</b> Objekt für die Sitzung, in dem der Spieler oder die Gruppe zurzeit wiedergegeben wird. Dieses Element ist immer erforderlich. | 
| ticketAttributes| Array von Objekten| Auflistung der vom Benutzer bereitgestellte Attribute und Werte über die Tickets für den Spieler.| 
| Spieler| Array von Objekten| Sammlung von Player-Objekten, jeweils einen Eigenschaftenbehälter, der vom Benutzer bereitgestellte Attribute. | 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
        "serviceConfig": "07617C5B-3423-4505-B6C6-10A16E1E5DDB",
        "hopperName": "TestHopper",
        "giveUpDuration": 10,
        "preserveSession": "never",
        "ticketSessionRef": {
        "scid": "AFFEABDF-0000-0000-0000-000000000001",
        "templateName": "TestTemplate",
        "sessionName": "5E551041-0000-0000-0000-000000000001"
    },
    "ticketAttributes": {
        "desiredMap": "Test Map",
        "desiredGameType": "Test GameType"
    },
    "players": [
        {
            "xuid": 123412345123,
            "playerAttributes": {
                "skill": 5,
                "ageRange": "teenager"
            }
        },
        {
          "xuid": 123412345124,
          "playerAttributes": {
              "skill": 15,
              "ageRange": "twenty-something"
          }
        }
      ]
    }
  
    
```

  
<a id="ID4EEB"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EGB"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   