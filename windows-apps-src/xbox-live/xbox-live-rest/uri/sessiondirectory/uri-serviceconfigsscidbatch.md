---
title: /serviceconfigs/{scid}/batch
assetID: eb1b510f-d92e-ae9b-a3e6-0edf58b4f075
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatch.html
description: " /serviceconfigs/{scid}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 208ee92106563372dd4d92a8c800cc08f513e8c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589715"
---
# <a name="serviceconfigsscidbatch"></a>/serviceconfigs/{scid}/batch
Unterstützt einen POST-Vorgang für eine Batchabfrage auf der Dienstebene des Konfigurations-ID an.

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

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[POST (/serviceconfigs/ {scid} / Batch-Out)](uri-serviceconfigsscidbatchpost.md)

&nbsp;&nbsp;Erstellt ein Batch-Abfragen von mehreren Xbox Benutzer-IDs für die Dienstkonfiguration an.

<a id="ID4E3B"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E5B"></a>


##### <a name="parent"></a>Parent

[Sitzung Directory URIs](atoc-reference-sessiondirectory.md)
