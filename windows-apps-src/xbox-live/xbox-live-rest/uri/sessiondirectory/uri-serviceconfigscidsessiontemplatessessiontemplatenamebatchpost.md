---
title: POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
assetID: 1a0a62ca-e120-e705-3c93-efd4697e2ccf
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.html
description: " POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 833b3c6b532dd65856342678d646798c5f24a6c1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637185"
---
# <a name="post-serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>POST (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch)
Erstellt ein Batch-Abfragen von mehreren Xbox-Benutzer-IDs an.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4EKB)
  * [Abfragezeichenfolgen-Parameter](#ID4EVB)
  * [HTTP-Statuscodes](#ID4EGF)
  * [Anforderungstext](#ID4ENF)
  * [Antworttext](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP/REST-Methode erstellt die Batch-Abfragen von mehreren Xbox-Benutzer-IDs auf der Sitzungsebene für die Vorlage. Diese Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**.

Für 2015 Multiplayer-Spiele, Sie können kombinieren viele Abfragen in einem einzigen Aufruf, wenn alle Abfragen den gleichen, jedoch sind Umgang mit unterschiedlichen Xbox-Benutzer-IDs (XUIDs), dargestellt in der *Xuid* Abfragezeichenfolgen-Parameter. Beachten Sie, dass die Abfragezeichenfolgen-Parameter und die Antworten für Abfragen mit regulären und Batch-Abfragen.

Für eine Batchabfrage durchgeführt, wurden die Sitzungen, die zu jeder XUID gehören in die Antwort in der gleichen Reihenfolge wie die *Xuid* Parameter werden in der Anforderung angezeigt. Es ist möglich, dass der gleichen Sitzung mehrere Male in der Antwort für die einzelnen einmal *Xuid* , die ihr entspricht.

<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 die Sitzungs-ID.|
| sessionTemplateName| string| Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2, der die Sitzungs-ID.|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

Eine Abfrage kann mit dem Abfragezeichenfolgen-Parameter in der folgenden Tabelle geändert werden.

| <b>Parameter</b>| <b>Typ</b>| <b>Beschreibung</b>|
| --- | --- | --- | --- | --- | --- | --- |
| keyword| string| Ein Schlüsselwort, z. B. "Foo", müssen in Sitzungen oder Vorlagen gefunden werden, wenn sie sind, abgerufen werden sollen. |
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Die Xbox Benutzer-ID, z. B. "123", für Sitzungen, die in der Abfrage eingeschlossen werden sollen. Standardmäßig muss der Benutzer in der Sitzung für enthalten war aktiv sein. |
| Reservierungen| boolean| "True", die diesen Sitzungen für die der Benutzer als ein reserviertes Player festgelegt ist jedoch nicht um einen aktiven Player werden verknüpft. Dieser Parameter wird nur beim Abfragen von Ihren eigenen Sitzungen oder beim Abfragen eines bestimmten Benutzers Sitzungen Server-zu-Server verwendet. |
| inaktiv| boolean| True, um Sitzungen zu enthalten, die der Benutzer hat akzeptiert, aber gegenwärtig nicht abgespielt wird. Anzahl der Sitzungen, in denen der Benutzer "ready", aber nicht "aktiv" ist, als inaktiv. |
| Private| boolean| True, um private Sitzungen einzuschließen. Dieser Parameter wird nur beim Abfragen von Ihren eigenen Sitzungen oder beim Abfragen eines bestimmten Benutzers Sitzungen Server-zu-Server verwendet. |
| visibility| string| Der Sichtbarkeitszustand für die Sitzungen. Mögliche Werte werden definiert, durch die <b>MultiplayerSessionVisibility</b>. Wenn dieser Parameter auf "Öffnen" festgelegt ist, sollte die Abfrage nur offene Sitzungen enthalten. Wenn sie auf "Privat" festgelegt ist die <i>private</i> -Parameter muss festgelegt werden auf "true". |
| version| 32-Bit-Ganzzahl mit Vorzeichen| Die maximale Sitzungsdauer-Version, die einbezogen werden sollen. Z. B. der Wert 2 Gibt an, nur Sitzungen mit einer wichtigen Sitzung Version 2 oder weniger eingeschlossen werden sollen. Die Versionsnummer muss kleiner als oder gleich der Anforderung Vertrag Version mod 100 sein. |
| Take| 32-Bit-Ganzzahl mit Vorzeichen| Die maximale Anzahl von Sitzungen abgerufen werden soll. Diese Zahl muss zwischen 0 und 100 sein. |


Einstellung *private* oder *Reservierungen* auf "true" ist auf Serverebene, auf die Sitzung erforderlich. Alternativ müssen diese Einstellungen des Aufrufers XUID Anspruch dem XUID Filter im URI entsprechen. Andernfalls wird der Statuscode HTTP 403/zurückgegeben, und zwar unabhängig davon, ob eine solche Sitzung tatsächlich vorhanden sind.

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4ENF"></a>


## <a name="request-body"></a>Anforderungstext


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


## <a name="response-body"></a>Antworttext

Die Rückgabe dieser Methode ist ein JSON-Array von Sitzung verweisen, mit einigen Sitzung enthaltenen Daten Inline.


```cpp
{
    "results": [ {
      "xuid": "9876",    // If the session was found from a xuid, that xuid.
      "startTime": "2009-06-15T13:45:30.0900000Z",
      "sessionRef": {
        "scid": "foo",
        "templateName": "bar",
        "name": "session-seven"
      },
      "accepted": 4,    // Approximate number of non-reserved members.
      "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
      "visibility": "open",    // or "private", "visible", or "full"
      "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
      "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
      "keywords": [ "one", "two" ]
    }
  ]
}
```


<a id="ID4EDG"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EFG"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)
