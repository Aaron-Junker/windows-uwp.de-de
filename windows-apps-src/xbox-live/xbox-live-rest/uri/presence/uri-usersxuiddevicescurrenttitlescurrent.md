---
title: /users/xuid({xuid})/devices/current/titles/current
assetID: f149c68b-9874-e348-4e1d-6acf5d305c49
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrent.html
description: " /users/xuid({xuid})/devices/current/titles/current"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 0b5c17f1791d69ca8a4c902b6c37736c4da28a31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645665"
---
# <a name="usersxuidxuiddevicescurrenttitlescurrent"></a>/users/xuid({xuid})/devices/current/titles/current
Zugriff auf das Vorhandensein von einen Titel oder einen Titel für den Benutzer. Die Domäne für diese URIs ist `userpresence.xboxlive.com`.
 
  * [URI-Parameter](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Zielbenutzers.| 
  
<a id="ID4EUB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[DELETE (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentdelete.md)

&nbsp;&nbsp;Entfernen Sie das Vorhandensein einer schließenden Titel, anstatt abzuwarten, bis die [PresenceRecord](../../json/json-presencerecord.md) abläuft.

[POST (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentpost.md)

&nbsp;&nbsp;Aktualisieren Sie einen Titel mit Anwesenheit eines Benutzers ein.
 
<a id="ID4EBC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EDC"></a>

 
##### <a name="parent"></a>Parent 

[Präsenz-URIs](atoc-reference-presence.md)

   