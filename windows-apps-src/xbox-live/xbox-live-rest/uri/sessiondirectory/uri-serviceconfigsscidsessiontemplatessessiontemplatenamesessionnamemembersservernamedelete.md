---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
assetID: 39c921d1-a166-74b9-fcbc-ea3c0c58cc40
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservernamedelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 65da52284d49d4d9384685d073f13bd93b10a30b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658315"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameserversserver-name"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
Entfernt den angegebenen Server aus einer Sitzung an.

> [!IMPORTANT]
> Diese URI-Methode erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * [URI-Parameter](#ID4ET)
  * [HTTP-Statuscodes](#ID4E5)
  * [Anforderungstext](#ID4EFB)
  * [Antworttext](#ID4EOB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 die Sitzungs-ID.|
| sessionTemplateName| string| Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2, der die Sitzungs-ID.|
| sessionName| GUID| Eindeutige ID der Sitzung. Teil 3 von den Sitzungsbezeichner.|

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EFB"></a>


## <a name="request-body"></a>Anforderungstext
Finden Sie in der Struktur der Anforderung in [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md).  
<a id="ID4EOB"></a>


## <a name="response-body"></a>Antworttext
Finden Sie in der antwortstruktur in [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4E1B"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E3B"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)
