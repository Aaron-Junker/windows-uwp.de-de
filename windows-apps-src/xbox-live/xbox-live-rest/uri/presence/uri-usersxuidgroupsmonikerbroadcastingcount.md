---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting/count
assetID: 535c8d46-7001-c31e-3e9d-82ad275095ae
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcount.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting/count"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7a39bc9c3302ba26949700774997355a216fe70d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651355"
---
# <a name="usersxuidxuidgroupsmonikerbroadcastingcount"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting/count
Greift im Zusammenhang mit der XUID, die im URI angezeigt wird die Anzahl der Broadcasting-Benutzer, die vom Moniker Gruppen angegeben. Die Domäne für diese URIs ist `userpresence.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| string| Xbox User ID (XUID) des Benutzers im Zusammenhang mit der XUIDs in der Gruppe.| 
| moniker| string| Zeichenfolge, die die Gruppe von Benutzern zu definieren. Der einzige akzeptierte Moniker derzeit ist die "People", mit dem Großbuchstaben "P".| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/ Benutzer/Xuid ({Xuid}) /groups/ {Moniker} / Broadcasting/Count)](uri-usersxuidgroupsmonikerbroadcastingcountget.md)

&nbsp;&nbsp;Ruft die Anzahl der Gruppen-Moniker im Zusammenhang mit der XUID, das in der URI wird angegebenen Broadcasting Benutzer ab.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[Präsenz-URIs](atoc-reference-presence.md)

   