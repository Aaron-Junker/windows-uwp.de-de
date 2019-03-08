---
title: POST (/handles/query)
assetID: a1a47d49-5c3f-8021-a213-13eb8bddf16a
permalink: en-us/docs/xboxlive/rest/uri-handlesquerypost.html
description: " POST (/handles/query)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 7540c117931c01c24c79cef6c8ab6540cb65bbcb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651505"
---
# <a name="post-handlesquery"></a>POST (/handles/query)
Erstellt Abfragen für die Sitzung verarbeitet.

> [!IMPORTANT]
> Diese Methode wird von der 2015 Multiplayer verwendet und gilt nur für diese Multiplayer-Version und höher. Es dient zur Verwendung mit Vorlage Vertrag 104/105 oder höher und erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4EDB)
  * [Abfragezeichenfolgen-Parameter](#ID4EQB)
  * [HTTP-Statuscodes](#ID4EBF)
  * [Anforderungstext](#ID4EIF)
  * [Antworttext](#ID4ETF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP/REST-Methode erstellt Abfragen nur die Daten verarbeiten, ohne Sitzungsinformationen. Es kann eingebunden werden, indem **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForSocialGroupAsync**.

Das Typfeld im Anforderungstext für diese Methode muss auf "Aktivität" festgelegt werden. Die Elemente im Antworttext direkt an den Eigenschaften des Zuordnen der **MultiplayerActivityDetails**.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI-Parameter

<a id="ID4EQB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

Eine Abfrage kann mit dem Abfragezeichenfolgen-Parameter in der folgenden Tabelle geändert werden.

| <b>Parameter</b>| <b>Typ</b>| <b>Beschreibung</b>|
| --- | --- | --- | --- |
| keyword| string| Ein Schlüsselwort, z. B. "Foo", müssen in Sitzungen oder Vorlagen gefunden werden, wenn sie sind, abgerufen werden sollen. |
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Die Xbox Benutzer-ID, z. B. "123", für Sitzungen, die in der Abfrage eingeschlossen werden sollen. Standardmäßig muss der Benutzer in der Sitzung für enthalten war aktiv sein. |
| Reservierungen| boolean| "True", die diesen Sitzungen für die der Benutzer als ein reserviertes Player festgelegt ist jedoch nicht um einen aktiven Player werden verknüpft. Dieser Parameter wird nur beim Abfragen von Ihren eigenen Sitzungen oder beim Abfragen eines bestimmten Benutzers Sitzungen Server-zu-Server verwendet. |
| inaktiv| boolean| True, um Sitzungen zu enthalten, die der Benutzer hat akzeptiert, aber gegenwärtig nicht abgespielt wird. Anzahl der Sitzungen, in denen der Benutzer "ready", aber nicht "aktiv" ist, als inaktiv. |
| Private| boolean| True, um private Sitzungen einzuschließen. Dieser Parameter wird nur beim Abfragen von Ihren eigenen Sitzungen oder beim Abfragen eines bestimmten Benutzers Sitzungen Server-zu-Server verwendet. |
| visibility| string| Der Sichtbarkeitszustand für die Sitzungen. Mögliche Werte werden definiert, durch die <b>MultiplayerSessionVisibility</b>. Wenn dieser Parameter auf "Öffnen" festgelegt ist, sollte die Abfrage nur offene Sitzungen enthalten. Wenn sie auf "Privat" festgelegt ist die <i>private</i> -Parameter muss festgelegt werden auf "true". |
| version| 32-Bit-Ganzzahl mit Vorzeichen| Die maximale Sitzungsdauer-Version, die einbezogen werden sollen. Z. B. der Wert 2 Gibt an, nur Sitzungen mit einer wichtigen Sitzung Version 2 oder weniger eingeschlossen werden sollen. Die Versionsnummer muss kleiner als oder gleich der Anforderung Vertrag Version mod 100 sein. |
| Take| 32-Bit-Ganzzahl mit Vorzeichen| Die maximale Anzahl von Sitzungen abgerufen werden soll. Diese Zahl muss zwischen 0 und 100 sein. |


Einstellung *private* oder *Reservierungen* auf "true" ist auf Serverebene, auf die Sitzung erforderlich. Alternativ müssen diese Einstellungen des Aufrufers XUID Anspruch dem XUID Filter im URI entsprechen. Andernfalls wird der Statuscode HTTP 403/zurückgegeben, und zwar unabhängig davon, ob eine solche Sitzung tatsächlich vorhanden sind.

<a id="ID4EBF"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EIF"></a>


## <a name="request-body"></a>Anforderungstext

Für alle Aktivitäten für die des Benutzers "Favoriten" social Graph sieht der Anforderungstext folgendermaßen aus:


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


<a id="ID4ETF"></a>


## <a name="response-body"></a>Antworttext

Die Handles werden als ein Array von Handles Strukturen, die mit einer eindeutigen ID, die in jeder Struktur eingebettete abgerufen.


```cpp
{
  "results": [
    {
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
    }
  }]
}

```


<a id="ID4E4F"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E6F"></a>


##### <a name="parent"></a>Parent

[/handles/query](uri-handlesquery.md)
