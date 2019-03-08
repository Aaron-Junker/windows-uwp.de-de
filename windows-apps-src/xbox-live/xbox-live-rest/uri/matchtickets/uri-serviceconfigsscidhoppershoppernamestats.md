---
title: /serviceconfigs/{scid}/hoppers/{name}/stats
assetID: 56bb4398-445b-e8c5-a4ce-1651576ee7e7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestats.html
description: " /serviceconfigs/{scid}/hoppers/{name}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 04376505b76296e052ea431f2a4e5fcfeac7b9e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613935"
---
# <a name="serviceconfigsscidhoppersnamestats"></a>/serviceconfigs/{scid}/hoppers/{name}/stats

Unterstützt einen GET-Vorgang zum Abrufen von Statistiken für eine Hopper an.

> [!IMPORTANT]
> Dieser URI ist für die Verwendung mit dem Vertrag 103 oder höher vorgesehen und erfordert eine Header-Element der X-Xbl-Contract-Version: 103 oder höher bei jeder Anforderung.

<a id="ID4ER"></a>


## <a name="domain"></a>Domäne
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>Hinweise
Dieser URI unterstützt die Werte Xuid Gt und mich für die Besitzer-ID in der Konfiguration des Zielbenutzers an. Nur ein Ticket für den Ersteller kann das Ticket löschen oder Abrufen des Status des URI.  
<a id="ID4E6"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Der Service Configuration Bezeichner (SCID) für die Sitzung.|
| name| string| Der Name des der Hopper.|

<a id="ID4EEC"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[Abrufen (/serviceconfigs/ {scid} /hoppers/ {Name} / Stats)](uri-serviceconfigsscidhoppershoppernamestatsget.md)

&nbsp;&nbsp;Ruft die Statistiken für eine Hopper ab.

<a id="ID4EQC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ESC"></a>


##### <a name="parent"></a>Parent  

[Vermittlung-URIs](atoc-reference-matchtickets.md)
