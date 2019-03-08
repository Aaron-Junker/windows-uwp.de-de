---
title: /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}
assetID: 25deb7fe-859c-01d2-d14f-455a36c08a7c
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketid.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 9722d06af64c5fd24f53485a1bcfe765f89b08cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627315"
---
# <a name="serviceconfigsscidhoppershoppernameticketsticketid"></a>/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}

Unterstützt einen Löschvorgang für ein Ticket für die Übereinstimmung an.

> [!IMPORTANT]
> Dieser URI ist für die Verwendung mit dem Vertrag 103 oder höher vorgesehen und erfordert eine Header-Element der X-Xbl-Contract-Version: 103 oder höher bei jeder Anforderung.

<a id="ID4ER"></a>


## <a name="domain"></a>Domäne
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>Hinweise
Dieser URI unterstützt die Werte Xuid Gt und mich für die Besitzer-ID in der Konfiguration des Zielbenutzers an.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Der Service Configuration Bezeichner (SCID) für die Sitzung.|
| name| string| Der Name des der Hopper.|
| ticketId| GUID| Das Ticket-ID an.|

<a id="ID4EJC"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})](uri-scidhoppernameticketiddelete.md)

&nbsp;&nbsp;Entfernt ein Ticket für die Übereinstimmung an.

<a id="ID4ETC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent  

[Vermittlung-URIs](atoc-reference-matchtickets.md)
