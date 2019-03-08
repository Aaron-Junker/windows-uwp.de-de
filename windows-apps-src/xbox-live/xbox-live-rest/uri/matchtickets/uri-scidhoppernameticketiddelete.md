---
title: DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})
assetID: d9ff3f21-aa70-af41-afa1-9a9244fcdb95
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketiddelete.html
description: " DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: fdd28cb94b31102d9af98aa95afde45424dadce9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589665"
---
# <a name="delete-serviceconfigsscidhoppershoppernameticketsticketid"></a>DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})

Entfernt ein Ticket für die Übereinstimmung an.

> [!IMPORTANT]
> Diese Methode ist für die Verwendung mit dem Vertrag 103 oder höher vorgesehen und erfordert eine Header-Element der X-Xbl-Contract-Version: 103 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4E2)
  * [Autorisierung](#ID4EGB)
  * [HTTP-Statuscodes](#ID4EOC)
  * [Anforderungstext](#ID4EXC)
  * [Antworttext](#ID4ECD)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP-REST-Methode löscht die angegebene Ticket-ID aus der benannten Hopper an der Dienstkonfiguration ID (SCID) Ebene an. Diese Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.DeleteMatchTicketAsync**.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Der Service Configuration Bezeichner (SCID) für die Sitzung.|
| name| string| Der Name des der Hopper.|
| ticketId| GUID| Das Ticket-ID an.|

<a id="ID4EGB"></a>


## <a name="authorization"></a>Autorisierung

| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (Benutzer-ID)| Ja| Der Benutzer die Anforderung muss ein Mitglied der Ticket-Sitzung, die durch das Ticket bezeichnet werden.| 403|
| Berechtigungen und der Typ des Geräts| Ja| Wenn der Benutzer "DeviceType" festgelegt ist, an die Konsole, dürfen nur Benutzer mit die Multiplayer-Berechtigung in ihre Ansprüche dem Vermittlungsdienst aufrufen.| 403|

<a id="ID4EOC"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EXC"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4ECD"></a>


## <a name="response-body"></a>Antworttext

Es werden keine Objekte im Text der Antwort gesendet.

<a id="ID4EPD"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ERD"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)
