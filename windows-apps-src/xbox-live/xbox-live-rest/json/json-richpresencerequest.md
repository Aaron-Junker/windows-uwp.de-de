---
title: RichPresenceRequest (JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
description: " RichPresenceRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 4c49da63ecd091a886a68f508af09e33fb9c58ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654125"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest (JSON)
Anfordern von Informationen über die umfassende Anwesenheitsinformationen verwendet werden soll. 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
RichPresenceRequest Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| id| string| Die <b>FriendlyName</b> von die umfassender vorhanden, die zu verwendende Zeichenfolge.| 
| scid| string| Scid, das angibt, auf die umfangreichen Anwesenheit Zeichenfolgen definiert werden.| 
| params| Array von Zeichenfolgen| Array von <b>FriendlyName</b> Zeichenfolgen, die umfangreiche Anwesenheit Zeichenfolge fertig zu stellen. Sollte nur Enumeration-Anzeigenamen angegeben werden, nicht die Statistiken. Diesem leer bleibt, werden alle vorherigen Werte entfernt.| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   