---
title: /users/xuid({xuid})/achievements/{scid}/{achievementid}
assetID: 656a6d63-1a11-b0a5-63d2-2b010abd62e7
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementid.html
description: " /users/xuid({xuid})/achievements/{scid}/{achievementid}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 00c577f60b67f15f75c47b5e737ca12819695110
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599445"
---
# <a name="usersxuidxuidachievementsscidachievementid"></a>/users/xuid({xuid})/achievements/{scid}/{achievementid}
Gibt Details zum Erreichen der Zertifikation, einschließlich der konfigurierten Metadaten und die benutzerspezifischen Daten zurück. 

> [!NOTE] 
> Nur unterstützt für die Plattform. 

 
Die Domäne für diese URIs ist `achievements.xboxlive.com`.
 
  * [URI-Parameter](#ID4E2)
 
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Benutzers, dessen Ressource zugegriffen wird. Muss die XUID des authentifizierten Benutzers entsprechen.| 
| scid| GUID| Eindeutiger Bezeichner der Dienstkonfiguration aus, deren Leistung zugegriffen wird.| 
| achievementid| 32-Bit-Ganzzahl ohne Vorzeichen| (Innerhalb der angegebenen SCID) Eindeutiger Bezeichner des das Erreichen der Zertifikation, die auf den zugegriffen wird.| 
  
<a id="ID4EMC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})](uri-usersxuidachievementsscidachievementidget.md)

&nbsp;&nbsp;Ruft die Details des Erfolgs ab.
 
<a id="ID4EWC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EYC"></a>

 
##### <a name="parent"></a>Parent 

[Erfolge-URIs](atoc-reference-achievementsv2.md)

   