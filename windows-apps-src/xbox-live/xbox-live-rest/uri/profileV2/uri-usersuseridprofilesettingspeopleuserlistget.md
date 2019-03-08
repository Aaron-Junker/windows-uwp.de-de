---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: f868fdf4f3d5cd36000784d9c5a3437fa5d67ffa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593855"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
Abrufen des Profils für einen Benutzer oder Benutzer, mit Menschen Moniker zu unterstützen. Die Domäne für diese URIs ist `profile.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EKB)
  * [Abfragezeichenfolgen-Parameter](#ID4EVB)
  * [Erforderlichen Anforderungsheader](#ID4EQC)
  * [Anforderungstext](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
**UserList** und **UserIds** sind gegenseitig-Parameter. Wenn beide oder nur eine angegeben werden, erhalten Sie eine **"BadRequest"** zurück. **UserList** ist ein Array für zukunftsfähigkeit in Szenarios, in denen mehrere benannte Listen nützlich, um die Anforderung. **Benutzer-IDs** besteht aus der Dezimalzeichenfolgen für XUIDs - JSON ist ungültig, auf 64-Bit-Ganzzahlen ohne Vorzeichen zu serialisieren. Abschließend Einstellungen im Xbox One werden Einstellungen, mit normalen lesbaren Namen benannt werden, statt 64-Bit-Ganzzahlen ohne Vorzeichen oder ungewöhnliche Konstanten wie **XONLINE_PROFILE_ASDF**.
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| userId| string| Entweder 'xuid(12345)', "gt(myGamertag)" oder "me" kann sein.| 
| userList| string| Eine benannte Liste der Einstellungen für zu bekommen. Benutzer ist derzeit die einzige Liste unterstützt.| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| Einstellungen| string| Eine durch Trennzeichen getrennte Liste von Namen festlegen.| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 32-Bit-Ganzzahl mit Vorzeichen| Wert = 2| 
| content-type| string| Value = <code>application/json</code>| 
  
<a id="ID4E2D"></a>

 
## <a name="request-body"></a>Anforderungstext
 
<a id="ID4EBE"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
GET /users/me/profile/settings/people/people?settings=GameDisplayName,GameDisplayPicRaw,Gamerscore,Gamertag
      
```

  
<a id="ID4EKE"></a>

  
 
<a id="ID4EME"></a>

 
##### <a name="response-body"></a>Antworttext 
Die Antwort ist ein **ReadMultiSettingsResponseV2** Objekt. Wenn der aufrufenden Benutzers hat nur ein Friend:
  

```cpp
{
      "profileUsers":[
         {
            "id":"2533274791381930",
            "settings":[
               {
                  "id":"GameDisplayName",
                  "value":"John Smith"
               },
               {
                  "id":"GameDisplayPicRaw",
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F&format=png&w=64&h=64"
               },
               {
                  "id":"Gamerscore",
                  "value":"0"
               },
               {
                  "id":"Gamertag",
                  "value":"CracklierJewel9"
               }
            ]
         }
      ]
   }
         
```

   
<a id="ID4E3E"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E5E"></a>

 
##### <a name="parent"></a>Parent 

[/users/{userId}/profile/settings/people/{userList}?settings={settings}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [Profil (JSON)](../../json/json-profile.md)

   