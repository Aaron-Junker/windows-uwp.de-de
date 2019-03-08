---
title: UserSettings (JSON)
assetID: 17c030cb-05e0-f78e-5ab1-cdbd8b801ceb
permalink: en-us/docs/xboxlive/rest/json-usersettings.html
description: " UserSettings (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 5451c59ab608105677a657ade41154bd2b622f5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655045"
---
# <a name="usersettings-json"></a>UserSettings (JSON)
Gibt Einstellungen für den aktuellen authentifizierten Benutzers zurück. 
<a id="ID4EN"></a>

 
## <a name="usersettings"></a>UserSettings
 
UserSettings Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| id| 32-Bit-Ganzzahl ohne Vorzeichen| Der Bezeichner der Einstellung.| 
| Quelle| 32-Bit-Ganzzahl ohne Vorzeichen| Stellt die Quelle der Einstellung an. | 
| titleId| 32-Bit-Ganzzahl ohne Vorzeichen| Der Bezeichner des Titels der Einstellung zugeordnet werden soll. | 
| value| Array von 8-Bit-Ganzzahl ohne Vorzeichen| Stellt den Wert der Einstellung. Abrufen von Einstellungen für Clients müssen verstehen, das Darstellungsformat, um die Daten lesen zu können. | 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
   "id":268697600,
   "source":1,
   "titleId":1297287259,
   "value":"CAAAAA=="
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   