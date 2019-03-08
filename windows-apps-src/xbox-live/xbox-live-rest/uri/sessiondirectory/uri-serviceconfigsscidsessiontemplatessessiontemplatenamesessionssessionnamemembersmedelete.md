---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)
assetID: aa5de623-7787-a47c-b7e4-305693b9fe35
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3de35398f4685a0b0cfda1a251c65ed6a74956d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637195"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)
Entfernt ein Mitglied aus einer Sitzung.

> [!IMPORTANT]
> Diese URI-Methode erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4E3)
  * [HTTP-Statuscodes](#ID4EHB)
  * [Anforderungstext](#ID4ENB)
  * [Antworttext](#ID4EYB)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise
Alle Vorgänge einer Sitzung Member Ressource erfordert eine Autorisierung für die Xbox-Benutzer-ID (XUID).  
<a id="ID4E3"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 die Sitzungs-ID.|
| sessionTemplateName| string| Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2, der die Sitzungs-ID.|
| sessionName| GUID| Eindeutige ID der Sitzung. Teil 3 von den Sitzungsbezeichner.|

<a id="ID4EHB"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4ENB"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4EYB"></a>


## <a name="response-body"></a>Antworttext
Finden Sie in der antwortstruktur in [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EBC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EDC"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)
