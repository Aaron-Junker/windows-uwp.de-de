---
title: PUT (/handles/{handle-id}/session)
assetID: d8a3d473-1192-ec0c-3935-c301f4f61e03
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionput.html
description: " PUT (/handles/{handle-id}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3a1872857d8b8e692f67e3c7b2a067ae86663c00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641605"
---
# <a name="put-handleshandle-idsession"></a>PUT (/handles/{handle-id}/session)
Erstellt oder aktualisiert eine Sitzung durch die Dereferenzierung eines Handles.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4ECB)
  * [HTTP-Statuscodes](#ID4ENB)
  * [Anforderungstext](#ID4EUB)
  * [Antworttext](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Schreibt diese HTTP-REST-Methode eine neue oder aktualisierte-Sitzung mit dem Multiplayer-Dienst, mit der angegebenen Sitzungs-Handle-ID Das Ergebnis ist ein Objekt, die neue oder aktualisierte Sitzung darstellt, wie vom Server zurückgegeben. Diese Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionByHandleAsync**.

Der Aufrufer dieser Methode erhält die Handle-ID vom des Spielers **MultiplayerActivityDetails** Objekt. Der Aufrufer ruft die ID hingegen aus einer protokollaktivierung, nachdem ein Benutzer eine Spiele Einladung akzeptiert hat.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| handleId| GUID| Die eindeutige ID des Handles für die Sitzung.|

<a id="ID4ENB"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EUB"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4E6B"></a>


## <a name="response-body"></a>Antworttext

Es werden keine Objekte im Text der Antwort gesendet.

<a id="ID4EKC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EMC"></a>


##### <a name="parent"></a>Parent

[/handles/{handleId}/session](uri-handleshandleidsession.md)
