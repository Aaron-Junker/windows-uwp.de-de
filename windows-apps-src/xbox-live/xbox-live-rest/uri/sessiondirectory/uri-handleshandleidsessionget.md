---
title: GET (/handles/{handleId}/session)
assetID: 1f22954c-e77b-69c2-63f4-741fbd965f8f
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionget.html
description: " GET (/handles/{handleId}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 41911dc53b316f4f323b9859d9101581ec88e497
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593895"
---
# <a name="get-handleshandleidsession"></a>GET (/handles/{handleId}/session)
Ruft ein Objekt für die angegebene Handle-ID ab.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4EDB)
  * [HTTP-Statuscodes](#ID4EOB)
  * [Anforderungstext](#ID4EVB)
  * [Antworttext](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP-REST-Methode ruft ein Objekt vom Server ab, mit dem angegebenen aufseiten des Dienstes Zeiger für die Sitzung (Handle) ab. Die Rückgabe ist das Sitzungsobjekt mit seine Attribute. Diese Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**.

Der Aufrufer dieser Methode erhält die Handle-ID vom des Spielers **MultiplayerActivityDetails** Objekt. Der Aufrufer ruft die ID hingegen aus einer protokollaktivierung, nachdem ein Benutzer eine Spiele Einladung akzeptiert hat.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| handleId| GUID| Die eindeutige ID des Handles für die Sitzung.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4E6B"></a>


## <a name="response-body"></a>Antworttext
Finden Sie in der antwortstruktur in [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EIC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EKC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}/session](uri-handleshandleidsession.md)
