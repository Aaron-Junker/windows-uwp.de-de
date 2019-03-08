---
title: SessionEntry (JSON)
assetID: b5cf5c3d-83b8-635f-d1a5-0be5d9434ea5
permalink: en-us/docs/xboxlive/rest/json-sessionentry.html
description: " SessionEntry (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 73133f898ff219477cb60f54798cbd81acb87ebe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589735"
---
# <a name="sessionentry-json"></a>SessionEntry (JSON)
Enthält Daten für eine Fitness-Sitzung. 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
SessionEntry Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| durationInSeconds| 32-Bit-Ganzzahl mit Vorzeichen | Dauer, in Sekunden – der Sitzung. | 
| Joule| 32-Bit-Ganzzahl mit Vorzeichen | Energie – in Joule – gebrannt in der Sitzung. | 
| erfüllt| Die Gleitkommazahl mit einfacher Genauigkeit| Erfüllt Durchschnittswert über die Dauer der Sitzung. Der MWS-Wert ist das Verhältnis von einer Einzelperson "metabolische" Rate, während eine Aktivität in Bezug auf die einzelnen "metabolische" Rate im Ruhezustand. Da die "metabolische" Rate für nachverarbeiten 1.0 unabhängig von einer Einzelperson Gewichtung ist und MWS-Werte sind relativ zu einer Einzelperson nachverarbeiten "metabolische" Rate, können sie zum Vergleichen der Intensität einer Aktivität, die von Personen andere Gewichtungen verwendet werden.| 
| serverTimestamp| DateTime| Time – basierend auf UTC-Zeit – Server Eintrag eingegeben wurde. | 
| Quelle| 8-Bit-Ganzzahl ohne Vorzeichen| Quell-Sitzung.| 
| Zeitstempel| DateTime| Time – basierend auf der koordinierten Weltzeit (UTC) – Eintrag auf dem Client erstellt wurde. | 
| titleId| 64-Bit-Ganzzahl ohne Vorzeichen| Title: Dezimal –, die den Eintrag erstellt.| 
  
<a id="ID4EFE"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
   "titleId" : "1234567",
   "timestamp" : "2011-11-18T08:08:46Z",
   "serverTimestamp" : "2011-11-20T04:04:23Z",
   "durationInSeconds" : 240,
   "joules" :  1600,
   "met" :  "124"
   "source" :  "1"
}
    
```

  
<a id="ID4EOE"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EQE"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   