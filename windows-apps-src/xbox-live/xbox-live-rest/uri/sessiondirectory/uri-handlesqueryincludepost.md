---
title: POST (/handles/query?include=relatedInfo)
assetID: 66ecd1fe-24d4-4cd5-256d-8950ac658529
permalink: en-us/docs/xboxlive/rest/uri-handlesqueryincludepost.html
description: " POST (/handles/query?include=relatedInfo)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: eb30561c91a085902dd9d79b6c4a2045dc709bfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632555"
---
# <a name="post-handlesqueryincluderelatedinfo"></a>POST (/handles/query?include=relatedInfo)
Erstellt Abfragen für die Sitzung verarbeitet, die verwandten Sitzungs-Informationen enthalten.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4ECB)
  * [Abfragezeichenfolgen-Parameter](#ID4EPB)
  * [HTTP-Statuscodes](#ID4EAF)
  * [Anforderungstext](#ID4EHF)
  * [Antworttext](#ID4EZF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP-REST-Methode erstellt Abfragen für die Behandlung der Daten mit Sitzungsinformationen, die in der Abfragezeichenfolge "include" angegeben. Den Wert der Abfragezeichenfolge ist auf Erweiterbarkeit ausgelegt zur Unterstützung zukünftiger Abfrageoptionen, die eine durch Trennzeichen getrennte Liste, z. B. Unterstützung werden "? enthalten = RelatedInfo, Sitzung". Die POST-Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForUsersAsync**.

Das Typfeld im Anforderungstext für diese Methode muss auf "Aktivität" festgelegt werden. Die Elemente im Antworttext direkt an den Eigenschaften des Zuordnen der **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI-Parameter

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

Eine Abfrage kann mit dem Abfragezeichenfolgen-Parameter in der folgenden Tabelle geändert werden.

| <b>Parameter</b>| <b>Typ</b>| <b>Beschreibung</b>|
| --- | --- | --- | --- |
| keyword| string| Ein Schlüsselwort, z. B. "Foo", müssen in Sitzungen oder Vorlagen gefunden werden, wenn sie sind, abgerufen werden sollen. |
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Die Xbox Benutzer-ID, z. B. "123", für Sitzungen, die in der Abfrage eingeschlossen werden sollen. Standardmäßig muss der Benutzer in der Sitzung für enthalten war aktiv sein. |
| Reservierungen| boolean| "True", die diesen Sitzungen für die der Benutzer als ein reserviertes Player festgelegt ist jedoch nicht um einen aktiven Player werden verknüpft. Dieser Parameter wird nur beim Abfragen von Ihren eigenen Sitzungen oder beim Abfragen eines bestimmten Benutzers Sitzungen Server-zu-Server verwendet. |
| inaktiv| boolean| True, um Sitzungen zu enthalten, die der Benutzer hat akzeptiert, aber gegenwärtig nicht abgespielt wird. Anzahl der Sitzungen, in denen der Benutzer "ready", aber nicht "aktiv" ist, als inaktiv. |
| Private| boolean| True, um private Sitzungen einzuschließen. Dieser Parameter wird nur beim Abfragen von Ihren eigenen Sitzungen oder beim Abfragen eines bestimmten Benutzers Sitzungen Server-zu-Server verwendet. |
| visibility| string| Der Sichtbarkeitszustand für die Sitzungen. Mögliche Werte werden definiert, durch die <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b>. Wenn dieser Parameter auf "Öffnen" festgelegt ist, sollte die Abfrage nur offene Sitzungen enthalten. Wenn sie auf "Privat" festgelegt ist die <i>private</i> -Parameter muss festgelegt werden auf "true". |
| version| 32-Bit-Ganzzahl mit Vorzeichen| Die maximale Sitzungsdauer-Version, die einbezogen werden sollen. Z. B. der Wert 2 Gibt an, nur Sitzungen mit einer wichtigen Sitzung Version 2 oder weniger eingeschlossen werden sollen. Die Versionsnummer muss kleiner als oder gleich der Anforderung Vertrag Version mod 100 sein. |
| Take| 32-Bit-Ganzzahl mit Vorzeichen| Die maximale Anzahl von Sitzungen abgerufen werden soll. Diese Zahl muss zwischen 0 und 100 sein. |


Einstellung *private* oder *Reservierungen* auf "true" ist auf Serverebene, auf die Sitzung erforderlich. Alternativ müssen diese Einstellungen des Aufrufers XUID Anspruch dem XUID Filter im URI entsprechen. Andernfalls wird der Statuscode HTTP 403/zurückgegeben, und zwar unabhängig davon, ob eine solche Sitzung tatsächlich vorhanden sind.

<a id="ID4EAF"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EHF"></a>


## <a name="request-body"></a>Anforderungstext

<a id="ID4ENF"></a>


### <a name="sample-request"></a>Beispiel für eine Anforderung

Rufen Sie alle Aktivitäten für die des Benutzers "Favoriten" social Graph sieht die POST-Text wie folgt aus:


```cpp
{
  "type": "activity",
  "scid": "B5B1F71F-A328-4371-89E0-C3AD222D0E92"  // optional filter on scid
  "owners": {
     "people": {
       "moniker": "favorites",
       "monikerXuid": "3210"
     }
  }
}

```


<a id="ID4EZF"></a>


## <a name="response-body"></a>Antworttext

Die Ergebnisse werden als Array von Handles Strukturen, die mit einer eindeutigen ID, die in jeder Handle eingebettet zurückgegeben. Die spezifische Sitzungs-Informationen werden zurückgegeben, der **RelatedInfo** Objekt. Beachten Sie, dass diese Informationen nicht auf diesen URI für den regulären POST-Methode zurückgegeben wird.

<a id="ID4EDG"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
    "results": [{
        "id": "11111111-ebe0-42da-885f-033860a818f6",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "party",
            "name": "e3a836aeac6f4cbe9bcab985494d3175"
        },

    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": true,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    },
    {
        "id": "11111111-ebe0-42da-885f-033860a818f7",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "TitleStorageTestDefault",
            "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
        },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": false,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    }]
}

```


<a id="ID4ENG"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EPG"></a>


##### <a name="parent"></a>Parent

[/handles/query](uri-handlesquery.md)
