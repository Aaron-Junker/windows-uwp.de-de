---
title: /users/xuid({xuid})/outbox
assetID: 0b66b885-15ff-be55-f8be-e6e9d85d087e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutbox.html
description: " /users/xuid({xuid})/outbox"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 88f3f3753aeac99db0a8a53e0a2ddde21d034ac5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607545"
---
# <a name="usersxuidxuidoutbox"></a>/users/xuid({xuid})/outbox
Bietet Sendeberechtigungen beschränkten Zugriff auf einen Benutzer die messaging-Ausgangsbox für Xbox LIVE Services. Die Domäne für diese URIs ist `msg.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter 
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid | 64-Bit-Ganzzahl ohne Vorzeichen | Die Xbox User ID (XUID) des Spielers, die die Anforderung stammt. | 
  
<a id="ID4EXB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden 

[POST (/users/xuid({xuid})/outbox)](uri-usersxuidoutboxpost.md)

&nbsp;&nbsp;Sendet eine angegebene Meldung an eine Liste von Empfängern. 
 
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent  

[Benutzer-URIs](atoc-reference-users.md)

   