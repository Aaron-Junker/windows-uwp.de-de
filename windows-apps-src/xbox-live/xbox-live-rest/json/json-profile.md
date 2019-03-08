---
title: Profile (JSON)
assetID: b92b1750-c2df-39b6-6c5c-f9e8068c8097
permalink: en-us/docs/xboxlive/rest/json-profile.html
description: " Profile (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7299fcb4d375a3fc35ad67306b70f5fa4afde963
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607515"
---
# <a name="profile-json"></a>Profile (JSON)
Die Einstellungen der persönlichen Profil für einen Benutzer. 
<a id="ID4EN"></a>

 
## <a name="profile"></a>Profil
 
Der Profil-Objekt hat den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| AppDisplayName| string| Der Name für die Anzeige von in-apps. Dies kann es sich um den Namen des Benutzers"echten" oder ihre Gamertag, je nach Datenschutz sein. Diese Einstellung steht für des Benutzers Identity-Zeichenfolge, die für die Anzeige in apps verwendet werden soll.| 
| GameDisplayName| string| Der Name für die Anzeige in spielen. Dies kann es sich um den Namen des Benutzers"echten" oder ihre Gamertag, je nach Datenschutz sein. Diese Einstellung steht für des Benutzers Identity-Zeichenfolge, die für die Anzeige in Spielen verwendet werden soll.| 
| Gamertag| string| Des Benutzers Gamertag.| 
| AppDisplayPicRaw| string| RAW-app-Pic URL anzeigen (siehe unten).| 
| GameDisplayPicRaw| string| URL für unformatierten Spiel Anzeige PIC-Standards (siehe unten).| 
| AccountTier| string| Welche Art von Konto besitzt der Benutzer? Gold, Silber oder FamilyGold?| 
| TenureLevel| 32-Bit-Ganzzahl ohne Vorzeichen| Wie viele Jahre wurde der Benutzer mit Xbox Live?| 
| Gamerscore| 32-Bit-Ganzzahl ohne Vorzeichen| Gamerscore des Benutzers.| 
  


> [!NOTE] 
> Bilder können es sich um das Bild des Benutzers"echten" oder ihre XboxOne Gamerpic, je nach Datenschutz sein. Diese Einstellungen repräsentieren die Bild-Url des Benutzers, der für die Anzeige auf dem Client verwendet werden soll. Dieses Image ist möglicherweise leer ist (gibt an, dass der Benutzer noch nicht um ein Bild festgelegt). 


 
Die Basis-URL ist eine URL mit veränderbarer Größe. Sie dienen kann, geben Sie einen der folgenden Größen und Dateiformate mit durch Anfügen `&format={format}&w={width}&h={height}` an den URI:
 
Format: Png
 
Größe: 64 x 64, 208 x 208, 424 x 424
 
<a id="ID4E2D"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4E4D"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   