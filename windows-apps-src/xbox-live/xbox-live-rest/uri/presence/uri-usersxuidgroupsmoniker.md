---
title: /users/xuid({xuid})/groups/{moniker}
assetID: 7c73236b-95ee-723b-e5e0-68252c953e14
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmoniker.html
description: " /users/xuid({xuid})/groups/{moniker}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 25ddc8120f05f04d5285fbbe4efc5a41f98265ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617555"
---
# <a name="usersxuidxuidgroupsmoniker"></a>/users/xuid({xuid})/groups/{moniker}
Greift auf die PresenceRecord für eine Gruppe aus. Die Domäne für diese URIs ist `userpresence.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| string| Xbox User ID (XUID) des Benutzers im Zusammenhang mit der XUIDs in der Gruppe.| 
| moniker| string| Zeichenfolge, die die Gruppe von Benutzern zu definieren. Der einzige akzeptierte Moniker derzeit ist die "People", mit dem Großbuchstaben "P".| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/users/xuid({xuid})/groups/{moniker} )](uri-usersxuidgroupsmonikerget.md)

&nbsp;&nbsp;Ruft die PresenceRecord für eine Gruppe ab.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[Präsenz-URIs](atoc-reference-presence.md)

   