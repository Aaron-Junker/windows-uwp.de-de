---
title: GET (/serviceconfigs/{scid}/hoppers/{name}/stats)
assetID: 4de5b07d-93e1-8ff0-05dd-1d3bb1802088
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestatsget.html
description: " GET (/serviceconfigs/{scid}/hoppers/{name}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 95de95b35de496331dd3fe0a4c69f18e047c1020
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621695"
---
# <a name="get-serviceconfigsscidhoppersnamestats"></a>GET (/serviceconfigs/{scid}/hoppers/{name}/stats)

Ruft die Statistiken für eine Hopper ab.

> [!IMPORTANT]
> Diese Methode ist für die Verwendung mit dem Vertrag 103 oder höher vorgesehen und erfordert eine Header-Element der X-Xbl-Contract-Version: 103 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4E5)
  * [Autorisierung](#ID4EJB)
  * [HTTP-Statuscodes](#ID4E3C)
  * [Anforderungstext](#ID4EFD)
  * [Antworttext](#ID4EQD)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise
Diese HTTP-REST-Methode ruft Statistiken aus den benannten Hopper an der Dienstkonfiguration ID (SCID) Ebene ab. Diese Methode kann eingebunden werden, indem die **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.GetHopperStatisticsAsync** API.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Der Service Configuration Bezeichner (SCID) für die Sitzung.|
| name| string| Der Name des der Hopper.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorisierung

| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (Benutzer-ID)| Ja| Der Benutzer die Anforderung muss ein Mitglied der Ticket-Sitzung, die durch das Ticket bezeichnet werden. | 403|
| Berechtigungen und der Typ des Geräts| Ja| Wenn der Benutzer "DeviceType" festgelegt ist, an die Konsole, dürfen nur Benutzer mit die Multiplayer-Berechtigung in ihre Ansprüche dem Vermittlungsdienst aufrufen. | 403|
| Titel-ID/für die Machbarkeitsstudie Purchase/Gerätetyp| Ja| Der Titel, der in die Übereinstimmung verwendet wird, muss Vermittlung für den angegebenen Titel-Anspruch, Gerät Kombination zulassen. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4EQD"></a>


## <a name="response-body"></a>Antworttext

| Mitglied| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| hopperName| string| Der Name des dem ausgewählten Hopper.|
| waitTime| 32-Bit-Ganzzahl mit Vorzeichen| Die durchschnittliche Zeit für das Hopper (eine ganzzahlige Anzahl von Sekunden) Übereinstimmung. |
| Auffüllung| 32-Bit-Ganzzahl mit Vorzeichen| Die Anzahl der Personen, die Übereinstimmungen in der Hopper gewartet werden soll.|

<a id="ID4E1D"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }


```


<a id="ID4EJE"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ELE"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)
