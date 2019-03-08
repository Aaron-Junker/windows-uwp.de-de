---
title: GET (/users/me)
assetID: 726c279b-73fb-02ea-cbff-700ff2dc31af
permalink: en-us/docs/xboxlive/rest/uri-usersmeget.html
description: " GET (/users/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b06305fde989d0c30570beda5d4b0aabe7bf0518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606725"
---
# <a name="get-usersme"></a>GET (/users/me)
Abrufen des aktuellen Benutzers [PresenceRecord](../../json/json-presencerecord.md) ohne Wissen des Benutzers XUID zu müssen.
Die Domäne für diese URIs ist `userpresence.xboxlive.com`.

  * [Abfragezeichenfolgen-Parameter](#ID4EZ)
  * [Autorisierung](#ID4EIC)
  * [Erforderlichen Anforderungsheader](#ID4ELD)
  * [Optionale Anforderungsheader](#ID4EPF)
  * [Anforderungstext](#ID4EPG)
  * [Antworttext](#ID4E1G)

<a id="ID4EZ"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| level| string| Optional. <ul><li><b>user</b>: Nur der Benutzerknoten zurückgegeben.</li><li><b>device</b>: Benutzer zum Knoten "Geräte" und "Knoten zurückgegeben.</li><li><b>title</b>: Default. Gibt die gesamte Struktur mit Ausnahme der Aktivität zurück.</li><li><b>all</b>: Gibt die gesamte Struktur, einschließlich auf Vorhandensein zurück.</li></ul> | 

<a id="ID4EIC"></a>


## <a name="authorization"></a>Autorisierung

| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen|
| --- | --- | --- | --- | --- | --- | --- |
| XUID| Ja| Xbox Benutzer-ID (XUID) des Aufrufers| 403 Forbidden|

<a id="ID4ELD"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".|
| x-xbl-contract-version| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Beispielwerte: 3, Vnext.|
| Annehmen| string| Inhaltstypen, die zulässig sind. Der einzige unterstützte durch Vorhandensein ist Application/Json, aber es muss angegeben werden, in der Kopfzeile.|
| Accept-Language| string| Zulässige Gebietsschema für Zeichenfolgen in der Antwort. Beispielwerte: En-US.|
| Host| string| Der Domänenname des Servers. Beispielwert: presencebeta.xboxlive.com.|

<a id="ID4EPF"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.|

<a id="ID4EPG"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4E1G"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4EAH"></a>


### <a name="sample-response"></a>Beispielantwort

Diese Methode gibt eine [PresenceRecord](../../json/json-presencerecord.md).


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


<a id="ID4EQH"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ESH"></a>


##### <a name="parent"></a>Parent

[/users/me](uri-usersme.md)
