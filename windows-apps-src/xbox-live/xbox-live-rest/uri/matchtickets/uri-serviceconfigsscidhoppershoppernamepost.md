---
title: POST (/serviceconfigs/{scid}/hoppers/{hoppername})
assetID: 8cbf62aa-d639-e920-1e39-099133af17f8
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamepost.html
description: " POST (/serviceconfigs/{scid}/hoppers/{hoppername})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7682b92ec61c98679904825e360d73318e9fee90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659835"
---
# <a name="post-serviceconfigsscidhoppershoppername"></a>POST (/serviceconfigs/{scid}/hoppers/{hoppername})

Erstellt das Ticket angegebenen überein.

> [!IMPORTANT]
> Diese Methode ist für die Verwendung mit dem Vertrag 103 oder höher vorgesehen und erfordert eine Header-Element der X-Xbl-Contract-Version: 103 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4E5)
  * [Autorisierung](#ID4EJB)
  * [HTTP-Statuscodes](#ID4E3C)
  * [Anforderungstext](#ID4EFD)
  * [Antworttext](#ID4E3G)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP-REST-Methode erstellt eine Match-Ticket für eine Hopper mit einem bestimmten Namen auf der Dienstkonfiguration ID (SCID) Ebene. Diese Methode kann eingebunden werden, indem die **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.CreateMatchTicketAsync** Methode.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Der Service Configuration Bezeichner (SCID) für die Sitzung.|
| hoppername | string | Der Name des der Hopper. |

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorisierung

| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Berechtigungen und der Typ des Geräts| Ja| Wenn der Benutzer "DeviceType" festgelegt ist, an die Konsole, dürfen nur Benutzer mit die Multiplayer-Berechtigung in ihre Ansprüche dem Vermittlungsdienst aufrufen. | 403|
| Gerätetyp| Ja| Wenn des Benutzers "DeviceType" nicht vorhanden ist, oder festgelegt ist, nicht über die Konsole, den Titel in abgeglichen wird müssen nicht nur die Konsole Titel sein. | 403|
| Titel-ID/für die Machbarkeitsstudie Purchase/Gerätetyp| Ja| Der Titel, der in die Übereinstimmung verwendet wird, muss Vermittlung für den angegebenen Titel-Anspruch, Gerät Kombination zulassen. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>Anforderungstext

<a id="ID4ELD"></a>


### <a name="required-members"></a>Erforderliche Member

| Mitglied| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| serviceConfig| GUID| SCID für die Sitzung.|
| hopperName| string| Der Name des der Hopper.|
| giveUpDuration| 32-Bit-Ganzzahl mit Vorzeichen| Maximale Wartezeit (ganzzahlige Anzahl von Sekunden).|
| preserveSession| Enumeration| Ein Wert, der angibt, wenn die Sitzung als die Sitzung in die entsprechend wiederverwendet wird. Mögliche Werte sind "immer" und "nie". |
| ticketSessionRef| MultiplayerSessionReference| Das MultiplayerSessionReference-Objekt, für die Sitzung, in der der Spieler oder die Gruppe aktuell wiedergegebenen ist. |
| ticketAttributes| Auflistung von Objekten| Attribute und Werte, die vom Benutzer zu den Spielern bereitgestellt.|

<a id="ID4EXF"></a>


### <a name="prohibited-members"></a>Unzulässige Mitglieder

Alle anderen Elemente sind in einer Anforderung nicht zulässig.

<a id="ID4ECG"></a>


### <a name="sample-request"></a>Beispiel für eine Anforderung

Die Sitzung verweist die **TicketSessionRef** Objekt muss erstellt werden, bevor ein Ticket für die Übereinstimmung erstellt werden kann, und die Sitzung muss der Spieler, um die zugeordnet werden, zusammen mit ihren Player-spezifischen Attributen enthalten. Jeder Spieler muss erstellen oder verknüpfen die Sitzung für die MPSD, die der Sitzung zugeordnete Match-Attribute hinzugefügt. Die Match-Attribute werden in einem Feld für benutzerdefinierte Eigenschaften MatchAttrs für jeden Spieler aufgerufen platziert.

Eine Anforderung zum Erstellen oder Verknüpfung wird übermittelt, um **https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templatename}/sessions/{sessionname}** und könnte wie folgt aussehen:


```cpp
{
   "members": {
     "me": {
       "constants": {
         "system": {
           "xuid": 2535285330879750
         }
      },
      "properties": {
         "custom": {
           "matchAttrs": {
             "skill": 5,
             "ageRange": "teenager"
           }
         }
      }
    }
  }
}

```


Sobald die Sitzung erstellt wurde, kann der Titel der Vermittlungsdienst zum Erstellen von Tickets für diese Sitzung aufrufen.


> [!NOTE] 
> Ein Titel kann Benutzer diesen Aufruf zu wiederholen, aber Sie sollten nicht es automatisch wiederholt, wenn die Daten können nicht.  



```cpp
POST /serviceconfigs/{scid}/hoppers/{hoppername}

{
  "giveUpDuration":10,
  "preserveSession": "never",
  "ticketSessionRef": {
     "scid": "ABBACDDC-0000-0000-0000-000000000001",  
     "templateName": "TestTemplate",
     "name": "5E55104-0000-0000-0000-000000000001"
  },
  "ticketAttributes": {
    "desiredMap": "Test Map",
    "desiredGameType": "Test GameType"
  }
}

```


<a id="ID4E3G"></a>


## <a name="response-body"></a>Antworttext

| Mitglied| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ticketId| GUID| Die ID für das Ticket erstellt wird.|
| waitTime| 32-Bit-Ganzzahl mit Vorzeichen| Die durchschnittliche Wartezeit für das Hopper (ganzzahlige Anzahl von Sekunden) an.|


```cpp
{
  "ticketId":"0584338f-a2ff-4eb9-b167-c0e8ddecae72",
  "waitTime":60
}

```


<a id="ID4EHAAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EJAAC"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)
