---
title: URIs für Anwesenheit
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
description: " URIs für Anwesenheit"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1a46ecd48c2b0bf523ab234a5f20cf9ed6669e75
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632605"
---
# <a name="presence-uris"></a>URIs für Anwesenheit
 
Dieser Abschnitt enthält Informationen zu Universal Resource Identifier (URI)-Adressen und zugehörigen Hypertext Transport Protocol (HTTP)-Methoden von Xbox Live Services für *Anwesenheit*.
 
Nur Spiele, die auf eine Xbox 360, auf einem Windows Phone-Gerät, oder unter Windows können diesen Dienst verwenden.
 
Die Domäne für diese URIs ist userpresence.xboxlive.com.
 
Sie können die Änderungen eines Benutzers vorhanden mit dem Dienst Real-Time-Aktivität (RTA) abonnieren.
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;Access-Anwesenheit für einen Batch von Benutzern.

[/users/me](uri-usersme.md)

&nbsp;&nbsp;Zugriff auf die aktuelle die Anwesenheit des Benutzers.

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;Greift auf die PresenceRecord für meine Gruppe.

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;Zugriff auf das Vorhandensein von einem anderen Benutzer oder Client.

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;Zugriff auf das Vorhandensein von einen Titel oder einen Titel für den Benutzer.

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;Greift auf die PresenceRecord für eine Gruppe aus.

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;Greift im Zusammenhang mit der XUID, die im URI angezeigt wird des Datensatzes vorhanden Broadcasting Benutzer anhand der Gruppen-Moniker.

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;Greift im Zusammenhang mit der XUID, die im URI angezeigt wird die Anzahl der Broadcasting-Benutzer, die vom Moniker Gruppen angegeben.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   