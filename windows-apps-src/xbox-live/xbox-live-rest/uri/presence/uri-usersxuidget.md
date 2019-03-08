---
title: GET (/users/xuid({xuid}))
assetID: c97ef943-8bea-8a41-90d7-faea874284c8
permalink: en-us/docs/xboxlive/rest/uri-usersxuidget.html
description: " GET (/users/xuid({xuid}))"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 5fcc5d3b6a172eccab0656da39e6896b4df50840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650925"
---
# <a name="get-usersxuidxuid"></a>GET (/users/xuid({xuid}))
Entdecken Sie das Vorhandensein von einem anderen Benutzer oder Client.
Die Domäne für diese URIs ist `userpresence.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EDB)
  * [Abfragezeichenfolgen-Parameter](#ID4EOB)
  * [Autorisierung](#ID4E4C)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4EAE)
  * [Erforderlichen Anforderungsheader](#ID4EVH)
  * [Optionale Anforderungsheader](#ID4E1BAC)
  * [Anforderungstext](#ID4E1CAC)
  * [Antworttext](#ID4EFDAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Die Antwort gefiltert werden kann, um Teil des bieten die [PresenceRecord](../../json/json-presencerecord.md) Wenn der Consumer nicht das gesamte Objekt interessiert ist.

> [!NOTE] 
> Die zurückgegebenen Daten werden durch Datenschutz- und Inhalt Isolationsregeln eingeschränkt.



<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- |
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Xbox User ID (XUID) des Zielbenutzers.|

<a id="ID4EOB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- |
| level| string| Optional. <ul><li><b>user</b>: Nur der Benutzerknoten zurückgegeben.</li><li><b>device</b>: Benutzer zum Knoten "Geräte" und "Knoten zurückgegeben.</li><li><b>title</b>: Default. Gibt die gesamte Struktur mit Ausnahme der Aktivität zurück.</li><li><b>all</b>: Gibt die gesamte Struktur, einschließlich auf Vorhandensein zurück.</li></ul> |

<a id="ID4E4C"></a>


## <a name="authorization"></a>Autorisierung

| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| Ja| Xbox Benutzer-ID (XUID) des Aufrufers| 403 Forbidden|

<a id="ID4EAE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource

Diese Methode immer 200 OK zurück, aber Inhalte möglicherweise nicht im Antworttext zurück.

| Anfordernden Benutzer| Ziel der Datenschutzeinstellungen des Benutzers| Verhalten|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Ich| -| 200 OK|
| Friend| Alle Benutzer| 200 OK|
| Friend| Nur Freunde| 200 OK|
| Friend| gesperrt| 200 OK|
| nicht-Friend-Benutzer| Alle Benutzer| 200 OK|
| nicht-Friend-Benutzer| Nur Freunde| 200 OK|
| nicht-Friend-Benutzer| gesperrt| 200 OK|
| Drittanbieter-Website| Alle Benutzer| 200 OK|
| Drittanbieter-Website| Nur Freunde| 200 OK|
| Drittanbieter-Website| gesperrt| 200 OK|

<a id="ID4EVH"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".|
| x-xbl-contract-version| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Beispielwerte: 3, Vnext.|
| Annehmen| string| Inhaltstypen, die zulässig sind. Der einzige unterstützte durch Vorhandensein ist Application/Json, aber es muss angegeben werden, in der Kopfzeile.|
| Accept-Language| string| Zulässige Gebietsschema für Zeichenfolgen in der Antwort. Beispielwerte: En-US.|
| Host| string| Der Domänenname des Servers. Beispielwert: presencebeta.xboxlive.com.|

<a id="ID4E1BAC"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.|

<a id="ID4E1CAC"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4EFDAC"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4ELDAC"></a>


### <a name="sample-response"></a>Beispielantwort

Wenn keine vorhandener Datensatz für den Benutzer vorhanden ist, wird ein Datensatz ohne Geräte zurückgegeben.


```cpp
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:W8,
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page",
      }
    }]
  }]
}

```


<a id="ID4EXDAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EZDAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})](uri-usersxuid.md)
