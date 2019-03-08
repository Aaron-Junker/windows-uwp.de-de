---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: d35b3f89f8b866a5236e8f5ac91eb37d9a82d306
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598555"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
Erstellt, aktualisiert oder eine Sitzung beitritt.

> [!IMPORTANT]
> Diese URI-Methode erfordert eine Header-Element der X-Xbl-Contract-Version: 104/105 oder höher bei jeder Anforderung.

  * ["Hinweise"](#ID4ET)
  * [URI-Parameter](#ID4EYB)
  * [HTTP-Statuscodes](#ID4EFC)
  * [Anforderungstext](#ID4EOC)
  * [Antworttext](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>Hinweise

Diese HTTP-REST-Methode erstellt, Verknüpfungen oder aktualisiert eine Sitzung, je nachdem, welcher Teil die gleichen JSON-Textvorlage gesendet wird. Bei Erfolg wird eine **MultiplayerSession** Objekt mit der Antwort vom Server zurückgegeben. Die Attribute darin unterscheiden aus den Attributen in der übergegebenen **MultiplayerSession** Objekt. Diese Methode kann durch eingebunden werden **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**.

Erstellung und Aktualisierung Vorgänge einer Sitzung verwenden Sie PUT mit Application/Json-Text, der die anzuwendenden Änderungen darstellt. Vorgänge sind Idempotent, d. h., mehrere Anwendungen die gleichen Änderungen keine weiteren Auswirkungen.

Der JSON-Anforderungstext spiegelt die Datenstruktur für die Sitzung. Alle Felder und untergeordnete Felder sind optional.

Das Wire-Format für der PUT-Methode erstellen der Sitzung oder Verknüpfen von Modus ist unten dargestellt.

> [!NOTE]
> Mit diesem Muster kümmern. Geprüfte Blind, unabhängig von den aktuellen Status der Sitzung angewendet werden.



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



Das Wire-Format für die PUT-Methode Sitzung Aktualisierungsmodus ist unten dargestellt.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



Das Wire-Format für die PUT-Methode zum Aktualisieren der Eigenschaften von leistungssitzungen ist unten dargestellt. Dies entspricht einem PUT-Vorgang für die URI-Sitzung mit einem Text von müssen lediglich das Objekt unter als Eigenschaften. Der Unterschied besteht darin, dass dieser Vorgang Fehlercode 404 zurückgibt, nicht gefunden werden, wenn die Sitzung nicht vorhanden ist. Dieser Vorgang unterstützt die If-Match-Header.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- |
| scid| GUID| Konfiguration Dienstbezeichner (SCID). Teil 1 die Sitzungs-ID.|
| sessionTemplateName| string| Der Name der aktuellen Instanz der Vorlage für die Sitzung. Teil 2, der die Sitzungs-ID.|
| sessionName| GUID| Eindeutige ID der Sitzung. Teil 3 von den Sitzungsbezeichner.|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes
Angewendet auf MPSD, gibt der Dienst einen HTTP-Statuscode zurück.  
<a id="ID4EOC"></a>


## <a name="request-body"></a>Anforderungstext

Im folgenden finden Sie ein beispielanforderungstext für das Erstellen oder beitreten zu einer Sitzung. Die folgenden Elemente des Anforderungstexts sind optional. Alle anderen möglichen Elemente sind in einer Anforderung nicht zulässig.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Konstanten| object| Nur-Lese Einstellungen, die zusammengeführt werden, mit der sitzungsvorlage, um die Konstanten für die Sitzung zu erstellen. |
| Eigenschaften | object | Änderungen an den Eigenschaften von leistungssitzungen zusammengeführt werden soll.|
| Members.Me | object| Konstanten und Eigenschaften, die ähnlich funktionieren wie ihre äquivalente der obersten Ebene. Eine PUT-Methode muss der Benutzer ein Mitglied der Sitzung sein, und fügt den Benutzer hinzu, falls erforderlich. Wenn Sie "me" als null angegeben wird, wird das Element, das die Anforderung aus der Sitzung entfernt. |
| Elemente | object| Andere Objekte, die Benutzer auf die Sitzung hinzufügen durch einen nullbasierten Index als Schlüssel darstellen. Die Anzahl der Elemente in einer Anforderung beginnt immer mit 0 (null), auch wenn die Sitzung bereits Mitglieder enthält. Elemente werden für die Sitzung in der Reihenfolge hinzugefügt, in denen sie in der Anforderung angezeigt werden. Elementeigenschaften können nur vom Benutzer festgelegt werden, zu denen sie gehören. |
| -Server | object| Werte, der angibt, Updates und Ergänzungen für die Sitzung die von Teilnehmern für zugeordnete Server festgelegt. Wenn ein Server als Null angegeben ist, wird dieser Eintrag aus der Sitzung entfernt. |



```cpp
{
  "properties": {
    "custom": {"KANWE": "MGMSY"},
    "system": {}
  },
  "constants": {
    "custom": {},
    "system": {"visibility": "open"}
  },
  "members": {
    "reserve_0": {
    "constants": {
      "custom": {"type": "leader"},
      "system": {"xuid": "5500461"} }}
   }
}

```


<a id="ID4E4C"></a>


## <a name="response-body"></a>Antworttext

Beispiel-Antworttext für das Erstellen oder beitreten zu einer Sitzung:


```cpp
{
  "contractVersion": 104,
  "correlationId": "0FE81338-EE96-46E3-A3B5-2DBBD6C41C3B",
  "nextTimer": "2009-06-15T13:45:30.0900000Z",

  "initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
  },

  "hostCandidates": [ "ab90a362", "99582e67" ],

  "constants": {
    "system": {"visibility": "open"},
    "custom": {}
  },

  "properties": {
     "system": { "turn": [] },
     "custom": { "myProperty": "myValue" }
  },

  "members": {
      "1": {
        "properties": {
        "system": { },
        "custom": { }
      },

      "constants": {
        "system": { "xuid": "5500461" },
        "custom": { }
      }

      "gamertag": "stacy",
      "deviceToken": "9f4032ba7",
      "reserved": true,
      "activeTitleId": "8397267",
      "joinTime": "2009-06-15T13:45:30.0900000Z",
      "turn": true,
      "initializationFailure": "latency",
      "initializationEpisode": 1,
      "next": 4
    },
  },

  "membersInfo": {
      "first": 1,
      "next": 4,
      "count": 1,
      "accepted": 0
  },

  "servers": {
      "name": {
        "constants": { },
        "properties": { }
      }
  }
}

```


<a id="ID4EID"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EKD"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
