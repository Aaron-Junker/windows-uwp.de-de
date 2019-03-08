---
title: HopperStatsResults (JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
description: " HopperStatsResults (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 38e345fc20e92cdf6446c6ae1100e347fe634eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646205"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults (JSON)
Ein JSON-Objekt, das die Statistiken für eine Hopper darstellt. 
<a id="ID4EN"></a>

  
 
Das HopperStatsResults JSON-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| hopperName| string| Der Name des dem ausgewählten Hopper.| 
| waitTime| 32-Bit-Ganzzahl mit Vorzeichen| Die durchschnittliche Zeit für das Hopper (eine ganzzahlige Anzahl von Sekunden) Übereinstimmung. | 
| Auffüllung| 32-Bit-Ganzzahl mit Vorzeichen| Die Anzahl der Personen, die Übereinstimmungen in der Hopper gewartet werden soll.| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax 
 

```json
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }
  
    
```

  
<a id="ID4EGB"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EIB"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUB"></a>

 
##### <a name="reference"></a>Verweis 

[Abrufen (/serviceconfigs/ {scid} /hoppers/ {Name} / Stats)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   