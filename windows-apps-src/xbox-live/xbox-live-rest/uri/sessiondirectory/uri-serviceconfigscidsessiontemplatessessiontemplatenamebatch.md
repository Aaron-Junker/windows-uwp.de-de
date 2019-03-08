---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
assetID: 4f8e1ece-2ba8-9ea4-e551-2a69c499d7b9
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 93ecaa95488493fa8779a44d76bf7d635d1751ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612595"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch
Unterstützt einen POST-Vorgang zum Erstellen einer Batchabfrage auf der Sitzungsebene für die Vorlage an.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

<a id="ID4ER"></a>


## <a name="domain"></a>Domäne
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 die Sitzungs-ID.|
| sessionTemplateName| string| Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2, der die Sitzungs-ID.|

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.md)

&nbsp;&nbsp;Erstellt ein Batch-Abfragen von mehreren Xbox-Benutzer-IDs an.

<a id="ID4EFC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EHC"></a>


##### <a name="parent"></a>Parent

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)
