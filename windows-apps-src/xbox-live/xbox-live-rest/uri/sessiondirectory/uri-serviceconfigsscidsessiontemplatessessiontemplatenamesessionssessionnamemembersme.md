---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
assetID: a6c2fa17-8bed-d0df-d7ff-db1aa60f44b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 2b0ecadb543434383ef32c7fd2a749760d5bcc98
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608455"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
Unterstützt einen DELETE-Vorgang zum Entfernen von Mitgliedern der Sitzung.
<a id="ID4EO"></a>


## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="remarks"></a>Hinweise

Alle Vorgänge einer Sitzung Member Ressource erfordern, ein Xbox-Benutzer-ID (XUID) Benutzeranspruch Autorisierung.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 die Sitzungs-ID.|
| sessionTemplateName| string| Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2, der die Sitzungs-ID.|
| sessionName| GUID| Eindeutige ID der Sitzung. Teil 3 von den Sitzungsbezeichner.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.md)

&nbsp;&nbsp;Entfernt ein Mitglied aus einer Sitzung.

<a id="ID4EYC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)
