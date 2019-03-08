---
title: GET (/public/scids/{scid}/clips)
assetID: 15b3e873-1f96-b1da-2f79-6dac1369a4c0
permalink: en-us/docs/xboxlive/rest/uri-publicscidclipsget.html
description: " GET (/public/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 5bce1dd261e0ad1172068a0287519cd0480da85f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589805"
---
# <a name="get-publicscidsscidclips"></a>GET (/public/scids/{scid}/clips)
Listet die öffentlichen Clips. Die Datendomäne für diesen URI besteht `gameclipsmetadata.xboxlive.com`.
 
  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4ECB)
  * [Abfragezeichenfolgen-Parameter](#ID4ENB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Hinweise
 
Mit dieser API können für verschiedene Möglichkeiten zur Liste Clips, die öffentlich sind. Die Liste der Features wird auf die Datenschutz-Überprüfungen und Überprüfungen im Hinblick auf die anfordernde XUID inhaltsisolation basierend zurückgegeben.
 
Abfragen werden pro Dienst-Konfigurations-ID (SCID) optimiert. Weitere Filter oder Sortierreihenfolgen als die unten aufgeführten Standardwerte angeben kann in einigen Fällen zurückgeben dauert länger. Dies ist offensichtlich für größere Gruppen von Videos. Abfragen können keine aufsteigende Sortierreihenfolge angeben.
 
Der Qualifizierer ist erforderlich, um die Datenbanksammlung Ofpublic Clips zu erhalten. Der anfordernde Benutzer Zugriff auf die angeforderte SCID benötigen, werden andernfalls HTTP 403 zurückgegeben.
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| string| Der primäre Dienst-Bezeichner der Konfiguration der öffentlichen Clips.| 
| titleid| string| Die TitleId der öffentlichen Clips. Kann nicht in den gleichen URI die SCID angegeben werden. Wenn angegeben, wird verwendet, um die primäre SCID zu suchen.| 
  
<a id="ID4ENB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| <b>?achievementId={achievementId}</b>| Die letzte Übereinstimmung den angegebenen Clips <b>AchievementId</b>.| Zusätzliche Sortierung/Filterung wird nicht unterstützt.| 
| <b>?greatestMomentId={greatestMomentId}</b>| Die letzte Übereinstimmung den angegebenen Clips <b>GreatestMomentId</b>.| Zusätzliche Sortierung/Filterung wird nicht unterstützt.| 
| <b>?qualifier=created </b>| Die letzte| Erforderlich.| 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent 

[/public/scids/{scid}/clips](uri-publicscidclips.md)

   