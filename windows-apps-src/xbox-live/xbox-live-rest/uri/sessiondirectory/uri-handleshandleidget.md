---
title: GET (/handles/{handle-id})
assetID: c95b5ab5-d56a-f70d-20d8-afb48d122ccd
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidget.html
description: " GET (/handles/{handle-id})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 501d36f4d1ac079af15d6bb7f35a90d5328fc8db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598465"
---
# <a name="get-handleshandle-id"></a>GET (/handles/{handle-id})
Ruft ab, Handles, die gemäß der Handle-ID an.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4EDB)
  * [HTTP-Statuscodes](#ID4EOB)
  * [Anforderungstext](#ID4EUB)
  * [Antworttext](#ID4E5B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP-REST-Methode ruft die aktuelle Aktivität des Benutzers für die Sitzung, für die angegebenen Handles. Die Rückgabe ist das Sitzungsobjekt mit seine Attribute. Diese Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**.

Der Aufrufer dieser Methode erhält die Handle-ID vom des Spielers **MultiplayerActivityDetails** Objekt. Der Aufrufer ruft die ID hingegen aus einer protokollaktivierung, nachdem ein Benutzer eine Spiele Einladung akzeptiert hat.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| handleId| GUID| Die eindeutige ID des Handles für die Sitzung.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EUB"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4E5B"></a>


## <a name="response-body"></a>Antworttext
Finden Sie in der antwortstruktur in [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EKC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EMC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}](uri-handleshandleid.md)
