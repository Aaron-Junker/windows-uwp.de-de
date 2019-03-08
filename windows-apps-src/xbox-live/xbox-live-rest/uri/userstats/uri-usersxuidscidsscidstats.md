---
title: /users/xuid({xuid})/scids/{scid}/stats
assetID: 3cf9ffd4-9a8b-2658-402b-2e933f7f6f1b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstats.html
description: " /users/xuid({xuid})/scids/{scid}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 53a6c7bb0e7390b024b01e221d8061316a80509e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650415"
---
# <a name="usersxuidxuidscidsscidstats"></a>/users/xuid({xuid})/scids/{scid}/stats
Greift auf eine Dienstkonfiguration, die von einer durch Trennzeichen getrennte Liste der Statistik Benutzernamen für den angegebenen Benutzer begrenzt. Die Domäne für diese URIs ist `userstats.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| GUID| Xbox User ID (XUID) des Benutzers auf dessen Namen auf der Dienstkonfiguration.| 
| scid| GUID| Der Bezeichner der Dienstkonfiguration aus, die die gewünschte Ressource enthält.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET](uri-usersxuidscidsscidstatsget.md)

&nbsp;&nbsp;Ruft ab eine Dienstkonfiguration, die von einer durch Trennzeichen getrennte Liste der Statistik Benutzernamen für den angegebenen Benutzer begrenzt.

[Mit Wertmetadaten abrufen](uri-usersxuidscidsscidstatsgetvaluemetadata.md)

&nbsp;&nbsp;Ruft eine Liste der angegebenen Statistiken, wie die Statistik-Werte, für einen Benutzer in einer angegebenen Dienstkonfiguration zugeordneten Metadaten ab.
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 

[Benutzer-Statistik-URIs](atoc-reference-userstats.md)

   