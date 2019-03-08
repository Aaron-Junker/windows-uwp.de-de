---
title: Datentypübersicht
assetID: c154a6fa-e7b2-4652-f6fc-f946f74480e9
permalink: en-us/docs/xboxlive/rest/datatypeoverview.html
description: " Datentypübersicht"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 62932a921d51a988a5533d7ee08f4968bb67a29d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659655"
---
# <a name="data-type-overview"></a>Datentypübersicht
 
Xbox Live Services verwendet eine Vielzahl von Datentypen, die im Zusammenhang mit der Identität und Authentifizierung. Dieses Thema enthält eine Übersicht über diese Typen.
 
| Typ| Beschreibung| 
| --- | --- | 
| Gamertag| Eine eindeutige, lesbare Bildschirmname des Benutzers.| 
| Player| Ein jsonobjekt, das mit XUID des Benutzers und das Gamertag, sowie des Spielers Index in der Sitzung (oder "Sitz"), ob der Spieler noch die Sitzung, und eines kleinen BLOBs benutzerdefinierter Daten beteiligt ist.| 
| Profil| Informationen über den Benutzer erfolgt über die Profil-URI-Adressen und HTTP-Methoden, in der Regel des Benutzers UserSettings, aber möglicherweise auch einschließlich-Spielerkarte, Gamertag, XUID und So weiter.| 
| Einstellung| Einer der Title-spezifischen Einstellungen in einem UserSettings-Objekt.| 
| UserClaims| Ein einfaches jsonobjekt, das mit XUID und das Gamertag des Benutzers.| 
| UserSettings| Ein JSON-Objekt, das eine Auflistung von Title-spezifischen Einstellungen oder die Einstellungen für den aktuellen authentifizierten Benutzer enthält. UserSettings kann beliebige Daten, die möglicherweise im Spiel-Aktivität zur enthalten.| 
| XUID| Xbox Benutzer-ID des Benutzers, einen eindeutigen unsigned long Integer. Sollen nicht lesbar sein.| 
 
<a id="ID4E6D"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EBE"></a>

 
##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ENE"></a>

 
##### <a name="reference--player-jsonjsonjson-playermd"></a>Verweis [Player (JSON)](../json/json-player.md)

 [Userclaims (JSON).](../json/json-userclaims.md)

 [UserSettings (JSON)](../json/json-usersettings.md)

   