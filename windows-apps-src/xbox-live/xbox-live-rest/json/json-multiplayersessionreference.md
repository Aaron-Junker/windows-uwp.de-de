---
title: MultiplayerSessionReference (JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
description: " MultiplayerSessionReference (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 5986079e1cae3338d8cc24a9e85f6941cf4fbec4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651245"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference (JSON)
Ein JSON-Objekt, das **MultiplayerSessionReference**. 
<a id="ID4EQ"></a>

  
 
Das MultiplayerSessionReference JSON-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 die Sitzungs-ID.| 
| templateName | string | Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2, der die Sitzungs-ID. | 
| name | string | Der Name der Sitzung. Teil 3 von den Sitzungsbezeichner. | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax 
 

```json
{
  "scid": "8d050174-412b-4d51-a29b-d55a34edfdb7",
  "templateName": "integration",
  "name": "19de0095d8bb41048f19edbbb6bc6b04"
}
  
    
```

  
<a id="ID4EJB"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ELB"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVB"></a>

 
##### <a name="reference"></a>Verweis 

[MultiplayerSession (JSON)](json-multiplayersession.md)

   