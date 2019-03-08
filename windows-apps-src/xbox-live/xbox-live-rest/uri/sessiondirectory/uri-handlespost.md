---
title: POST (/handles)
assetID: 21f3e289-0b0e-2731-befb-bd4c0d71973e
permalink: en-us/docs/xboxlive/rest/uri-handlespost.html
description: " POST (/handles)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: ed3482b8e629749d294ed25944db16372cc7fee6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594745"
---
# <a name="post-handles"></a>POST (/handles)
Legt die Multiplayer-Sitzung für die aktuelle Aktivität des Benutzers und Sitzung Elemente lädt, falls erforderlich.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4EHB)
  * [HTTP-Statuscodes](#ID4EPB)
  * [Anforderungstext](#ID4EVB)
  * [Antworttext](#ID4EJC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP-REST-Methode kann verwendet werden, um die Sitzung für die aktuelle Aktivität festzulegen. In diesem Fall kann die Methode eingebunden werden, indem **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SetActivityAsync**. Der Anforderungstext muss die Sitzung definieren verweisen, mit der **SessionRef** Objekt in der JSON-Datei mit dem Typfeld, um "Aktivität". Es wird kein Antworttext abgerufen. Definitionen der Elemente in einer Sitzung Verweis angegeben wird, finden Sie unter **Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference**.

Diese POST-Methode kann auch zum Einladen von Benutzern angegeben, die von den Handles zu einer Sitzung verwendet werden. In diesem Fall kann die Methode eingebunden werden, indem **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SendInvitesAsync**. Diese Verwendung der POST-Methode erfordert Anforderungstext definieren Sie die Referenz zu Sitzung jedoch mit dem Feld auf "einladen" festgelegt ist. Der Antworttext ist ein Handle für die INVITE-Nachricht.

<a id="ID4EHB"></a>


## <a name="uri-parameters"></a>URI-Parameter

Keine

<a id="ID4EPB"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>Anforderungstext

<a id="ID4E1B"></a>


### <a name="request-body-for-setting-activity"></a>Der Anforderungstext zum Festlegen der Aktivität


```cpp
{
  "version": 1,
  "type": "activity",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    // The session name is optional in a POST; if not specified, MPSD fills in a GUID.//
    "name": "session-49"
  },
}

```


<a id="ID4EBC"></a>


### <a name="request-body-for-sending-invites"></a>Anforderungstext für das Senden von Einladungen


```cpp
{
  // Common handle fields
  "id: "47ca0049-a5ba-4bc1-aa22-fcf834ce4c13",
  "version": 1,
  "type": "invite",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    "name": "session-49"
   },
   "inviteAttributes": {
     "titleId" : {titleId}, // The title being invited to, in decimal uint32. This value is used to find the title name and/or image.
     "context" : {context}, // The title defined context token. Must be 256 characters or less when URI-encoded.
     "contextString" : {contextstring}, // The string name of a custom invite string to display in the invite notification.
     "senderString" : {sender} // The string name of the sender when the sender is a service.
   },
   "invitedXuid": "3210",
}

```


<a id="ID4EJC"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4EOC"></a>


### <a name="response-body-for-setting-activity"></a>Antworttext für das Festlegen der Aktivität
Keine  
<a id="ID4ESC"></a>


### <a name="response-body-for-sending-invites"></a>Antworttext für das Senden von Einladungen
Ein Handle INVITE-Nachricht.   
<a id="ID4EXC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EZC"></a>


##### <a name="parent"></a>Parent

[/handles](uri-handles.md)
