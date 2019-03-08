---
title: /serviceconfigs/{scid}/hoppers/{hoppername}
assetID: ba1e129d-b4c4-6535-46ce-fd184465c85f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppername.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1db069f59eeba565a257531907d6be0b1dcd189d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598425"
---
# <a name="serviceconfigsscidhoppershoppername"></a>/serviceconfigs/{scid}/hoppers/{hoppername}

Unterstützt einen POST-Vorgang zum Erstellen von Tickets für Übereinstimmung an.

> [!IMPORTANT]
> Dieser URI ist für die Verwendung mit dem Vertrag 103 oder höher vorgesehen und erfordert eine Header-Element der X-Xbl-Contract-Version: 103 oder höher bei jeder Anforderung.

<a id="ID4ER"></a>


## <a name="domain"></a>Domäne
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Der Service Configuration Bezeichner (SCID) für die Sitzung.|
| hoppername | string | Der Name des der Hopper. |

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[POST (/serviceconfigs/ {scid} /hoppers/ {Hoppername})](uri-serviceconfigsscidhoppershoppernamepost.md)

&nbsp;&nbsp;Erstellt das Ticket angegebenen überein.

<a id="ID4EFC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EHC"></a>


##### <a name="parent"></a>Parent  

[Vermittlung-URIs](atoc-reference-matchtickets.md)
