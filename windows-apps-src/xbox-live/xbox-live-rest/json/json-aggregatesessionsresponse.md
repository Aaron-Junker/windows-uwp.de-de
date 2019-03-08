---
title: AggregateSessionsResponse (JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
description: " AggregateSessionsResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: ef026fd5096d047b2014faaf95a667c69827e043
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608305"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse (JSON)
Enthält aggregierte Daten für Sitzungen, die Eignung eines Benutzers an. 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
AggregateSessionsResponse Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| totalDurationInSeconds| 64-Bit-Ganzzahl mit Vorzeichen| Die Gesamtdauer der Sitzungen in Sekunden, während des aggregationszeitraums.| 
| totalJoules| 64-Bit-Ganzzahl mit Vorzeichen| Gesamtanzahl der verschwendeten Energie entfällt, im Joule – während des aggregationszeitraums. | 
| totalSessions| 64-Bit-Ganzzahl mit Vorzeichen| Die Gesamtzahl der Sitzungen während des aggregationszeitraums.| 
| weightedAverageMets| Die Gleitkommazahl mit einfacher Genauigkeit | Gewichtete durchschnittliche "metabolische" entsprechende (MWS) Aufgabenwert während des aggregationszeitraums. Der MWS-Wert ist das Verhältnis von einer Einzelperson "metabolische" Rate, während eine Aktivität in Bezug auf die einzelnen "metabolische" Rate im Ruhezustand. Da die "metabolische" Rate für nachverarbeiten 1.0 unabhängig von einer Einzelperson Gewichtung ist und MWS-Werte sind relativ zu einer Einzelperson nachverarbeiten "metabolische" Rate, können sie zum Vergleichen der Intensität einer Aktivität, die von Personen andere Gewichtungen verwendet werden.| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
   "totalSessions" : 300,
   "totalDurationInSeconds" : 1240,
   "totalJoules" :  21600,
   "weightedAvgMet" : 21,
}

    
```

  
<a id="ID4E2C"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E4C"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   