---
title: quotaInfo (JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
description: " quotaInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: f3499fdba972d6e953813fc490d080910921698e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654115"
---
# <a name="quotainfo-json"></a>quotaInfo (JSON)
Enthält Kontingentinformationen zu einer Gruppe Titel. 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
Das Objekt QuotaInfo gelten die folgenden Spezifikationen.
 
Für den globalen Speicher
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| quotaBytes| 32-Bit-Ganzzahl mit Vorzeichen | Maximale Anzahl von Bytes, die von den Titel verwendet werden kann.| 
| usedBytes| 32-Bit-Ganzzahl mit Vorzeichen | Anzahl der Bytes, die durch den Titel verwendet.| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 
Das folgende Beispiel zeigt die Antwort für den globalen Speicher an:
 

```json
{
    "quotaInfo":
    {
        "usedBytes":4194304,
        "quotaBytes":536870912
    }
}
      
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   