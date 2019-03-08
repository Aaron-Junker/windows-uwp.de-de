---
title: /users/{userId}/profile/settings/people/{userList}?settings={settings}
assetID: 0ba20eba-f0ab-28ab-61d3-b4f9e4c07bc5
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlist.html
description: " /users/{userId}/profile/settings/people/{userList}?settings={settings}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 24b58c817156a7c372a8e6acfab895e6b7c51207
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636965"
---
# <a name="usersuseridprofilesettingspeopleuserlistsettingssettings"></a>/users/{userId}/profile/settings/people/{userList}?settings={settings}
Zugriff auf das Profil für einen Benutzer oder Benutzer, mit Menschen Moniker zu unterstützen. Die Domäne für diese URIs ist `profile.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| userId| string| Entweder 'xuid(12345)', "gt(myGamertag)" oder "me" kann sein.| 
| userList| string| Eine benannte Liste der Einstellungen für zu bekommen. Benutzer ist derzeit die einzige Liste unterstützt.| 
  
<a id="ID4E1B"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/users/{userId}/profile/settings/people/{userList})](uri-usersuseridprofilesettingspeopleuserlistget.md)

&nbsp;&nbsp;Abrufen des Profils für einen Benutzer oder Benutzer, mit Menschen Moniker zu unterstützen.
 
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Parent 

[Profile-URIs](atoc-reference-profiles.md)

 [Profil (JSON)](../../json/json-profile.md)

   