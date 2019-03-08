---
title: /users/xuid({xuid})/inbox
assetID: 352740c6-42e2-0000-495d-bf384dc3e941
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinbox.html
description: " /users/xuid({xuid})/inbox"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 8ded70b32dfd291d17a43a1741b26710f681a397
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640895"
---
# <a name="usersxuidxuidinbox"></a>/users/xuid({xuid})/inbox
Bietet Zugriff auf die ein Benutzer die Eingangsbox für Xbox LIVE Services messaging. Die Domäne für diese URIs ist `msg.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid | 64-Bit-Ganzzahl ohne Vorzeichen | Die Xbox User ID (XUID) des Spielers, die die Anforderung stammt. | 
| messageId | string[50] | Die ID der Nachricht, die abgerufen oder gelöscht. | 
  
<a id="ID4EDC"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden 

[GET (/users/xuid({xuid})/inbox)](uri-usersxuidinboxget.md)

&nbsp;&nbsp;Ruft eine angegebene Anzahl von Zusammenfassungen von Benutzer-Nachricht vom Dienst ab. 

[DELETE (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageiddelete.md)

&nbsp;&nbsp;Löscht eine Benutzernachricht im Posteingang des Benutzers.

[GET (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageidget.md)

&nbsp;&nbsp;Ruft den Text ausführliche Meldung für eine bestimmte Benutzernachricht, markieren es als lesen, für den Dienst ab. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent  

[Benutzer-URIs](atoc-reference-users.md)

   