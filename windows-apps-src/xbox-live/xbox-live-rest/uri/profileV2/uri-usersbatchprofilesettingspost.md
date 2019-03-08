---
title: POST (/users/batch/profile/settings)
assetID: 2a619148-a626-f413-bda1-a2790063075d
permalink: en-us/docs/xboxlive/rest/uri-usersbatchprofilesettingspost.html
description: " POST (/users/batch/profile/settings)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 0f859a58e32624223d59d918d46f6230a3abd6db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662225"
---
# <a name="post-usersbatchprofilesettings"></a>POST (/users/batch/profile/settings)
Rufen Sie das Profil für mindestens einen Benutzer an. Die Domäne für diese URIs ist `profile.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [Autorisierung](#ID4EFB)
  * [Erforderlichen Anforderungsheader](#ID4EOB)
  * [Anforderungstext](#ID4EZC)
  * [Antworttext](#ID4EJD)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Dies ist nur vollqualifizierte URL des Profils in zulässig. Alle anderen Profil-APIs von Clients werden blockiert.
  
<a id="ID4EFB"></a>

 
## <a name="authorization"></a>Autorisierung
 
Um ein Profil zuzugreifen, sind nur ein normaler Authentifizierungstoken und Ansprüche erforderlich.
  
<a id="ID4EOB"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | 
| x-xbl-contract-version| 32-Bit-Ganzzahl ohne Vorzeichen| Die Contract-Version muss auf 2 festgelegt werden, um diesen Aufruf aus der Xbox 360-API zu unterscheiden.| 
| content-type| string| Value = <code>application/json</code>| 
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>Anforderungstext
 
<a id="ID4E6C"></a>

 
### <a name="sample-request"></a>Beispiel für eine Anforderung
 

```cpp
POST /users/batch/profile/settings
   {
      "userIds":[
         "2533274791381930"
       ],
      "settings":[
         "GameDisplayName",
         "GameDisplayPicRaw",
         "Gamerscore",
         "Gamertag"
      ]
   }
      
```

   
<a id="ID4EJD"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4EPD"></a>

 
### <a name="sample-response"></a>Beispielantwort
 

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
                  "value":"https://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F"
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

   
<a id="ID4EZD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E2D"></a>

 
##### <a name="parent"></a>Parent 

[/users/batch/profile/settings](uri-usersbatchprofilesettings.md)

  
<a id="ID4EFE"></a>

 
##### <a name="reference"></a>Verweis 

[Profil (JSON)](../../json/json-profile.md)

   