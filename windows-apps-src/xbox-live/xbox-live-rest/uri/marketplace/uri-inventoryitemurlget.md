---
title: GET (/inventory/{itemID})
assetID: d3ca14a5-0214-ef42-091e-3f05f2a3482d
permalink: en-us/docs/xboxlive/rest/uri-inventoryitemurlget.html
description: " GET (/inventory/{itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 446197eb20820304088ddac4a6379fa3b2510873
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606695"
---
# <a name="get-inventoryitemid"></a>GET (/inventory/{itemID})
Stellt den vollständigen Satz von Details für einen bestimmten Lagerartikel bereit. Die Domäne für diese URIs ist `inventory.xboxlive.com`.
 
  * ["Hinweise"](#ID4EX)
  * [URI-Parameter](#ID4EAB)
  * [Antworttext](#ID4ELB)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Hinweise
 
Keine Richtlinie eincheckt, Erzwingung oder Filterung erfolgt als Teil dieses Aufrufs.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| itemID| string| die ID für jeden Benutzer für eine einzelne Lagerartikel eindeutig| 
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4ERB"></a>

 
### <a name="sample-response"></a>Beispielantwort
 
Die Antwort auf eine GET-Anforderung, vorausgesetzt, es Authentifizierung erfolgreich ist und die entsprechenden Autorisierungskontext, ist eine einzelne Lagerartikel mit dem vollständigen Satz von Eigenschaften des Elements.
 

```cpp
{inventoryItem}
         
```

   
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 

[GET (/inventory/ {ItemID})](uri-inventoryget.md)

  
<a id="ID4EJC"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Allgemeine EDS-Header](../../additional/edscommonheaders.md)

 [EDS-Parameter](../../additional/edsparameters.md)

 [EDS Abfragen Raffinierer](../../additional/edsqueryrefiners.md)

 [Marketplace-URIs](atoc-reference-marketplace.md)

 [Zusätzliche Referenz](../../additional/atoc-xboxlivews-reference-additional.md)

   