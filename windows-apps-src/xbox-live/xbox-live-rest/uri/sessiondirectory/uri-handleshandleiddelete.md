---
title: DELETE (/handles/{handleId})
assetID: 71cceff4-3a74-dc05-12db-cfe3f20a8aea
permalink: en-us/docs/xboxlive/rest/uri-handleshandleiddelete.html
description: " DELETE (/handles/{handleId})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 354f3563c48139edc5d5cc041e8304998af55620
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613405"
---
# <a name="delete-handleshandleid"></a>DELETE (/handles/{handleId})
Löscht die Handles, die gemäß der Handle-ID an.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4EAB)
  * [HTTP-Statuscodes](#ID4ELB)
  * [Anforderungstext](#ID4ESB)
  * [Antworttext](#ID4E4B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise
Diese HTTP-REST-Methode löscht die Handles für die angegebene ID und löscht aktuelle Aktivität des Benutzers, für die Sitzung. Diese Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.ClearActivityAsync**.  
<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| handleId| GUID| Die eindeutige ID des Handles für die Sitzung.|

<a id="ID4ELB"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4ESB"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4E4B"></a>


## <a name="response-body"></a>Antworttext

Es werden keine Objekte im Text der Antwort gesendet.

<a id="ID4EIC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}](uri-handleshandleid.md)
