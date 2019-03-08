---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
assetID: 55ce6459-1714-49bc-6231-b547ddf04143
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: da8d61634d0a849d51e2ab6ff143469ec05cc75c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657475"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
Unterstützt die PUT- und GET-Vorgänge zum Erstellen und Abrufen von Sitzungen.
<a id="ID4EO"></a>


## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 die Sitzungs-ID.|
| sessionTemplateName| string| Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2, der die Sitzungs-ID.|
| sessionName| GUID| Eindeutige ID der Sitzung. Teil 3 von den Sitzungsbezeichner.| 

<a id="ID4EBC"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[GET (/serviceconfigs/ {scid} /sessiontemplates/ {SessionTemplateName} /sessions/ {SessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.md)

&nbsp;&nbsp;Ruft ein Objekt ab.

[PUT (/serviceconfigs/ {scid} /sessiontemplates/ {SessionTemplateName} /sessions/ {SessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

&nbsp;&nbsp;Erstellt, aktualisiert oder eine Sitzung beitritt.

<a id="ID4EOC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)
