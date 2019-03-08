---
title: ServiceErrorResponse (JSON)
assetID: a2077df8-f76c-0233-8e41-68267b681862
permalink: en-us/docs/xboxlive/rest/json-serviceerrorresponse.html
description: " ServiceErrorResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 86f9389f6f76c1c51955a6c784393e9b05909298
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597505"
---
# <a name="serviceerrorresponse-json"></a>ServiceErrorResponse (JSON)
Wenn ein Dienstfehler aufgetreten ist, wird ein entsprechender HTTP-Fehlercode zurückgegeben. Der Dienst kann optional auch ein ServiceErrorResponse-Objekt enthalten, wie unten definiert. In produktionsumgebungen möglicherweise weniger Daten enthalten. 
<a id="ID4EN"></a>

 
## <a name="serviceerrorresponse"></a>ServiceErrorResponse
 
ServiceErrorResponse Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| <b>errorCode</b>| 32-Bit-Ganzzahl mit Vorzeichen| Code des Fehlers (kann null sein).| 
| <b>errorMessage</b>| string| Weitere Informationen zu diesem Fehler.| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
   "errorCode": 8377,
   "errorMessage": "XUID specified in the claim does not match URI XUID."
 }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   