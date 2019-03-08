---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
description: " POST (/users/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 47376338a1c515aa00a7e0247df4cc16ee0db8d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655675"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
Anwesenheit für einen Batch von Benutzern zu erhalten.
Die Domäne für diese URIs ist `userpresence.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [Autorisierung](#ID4EAB)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4EDC)
  * [Erforderlichen Anforderungsheader](#ID4EYF)
  * [Optionale Anforderungsheader](#ID4EGAAC)
  * [Anforderungstext](#ID4EGBAC)
  * [Antworttext](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Diese Methode sollte jeder Client, Dienst oder Titel lernen möchten, können die Anwesenheitsinformationen für einen Batch von Benutzern verwendet werden.

Die Antworten für diese Batchanforderung können Filtern anhand von Tiefe und Pfad sein. Kunden können Hiermit können Sie ermitteln und das Vorhandensein zu einer Gruppe von Benutzern anzuzeigen. Der Filter für diese API arbeiten als OR in eine Eigenschaft, aber Ausdrücke auf Eigenschaften.

<a id="ID4EAB"></a>


## <a name="authorization"></a>Autorisierung

| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen|
| --- | --- | --- | --- |
| XUID| Ja| Xbox Benutzer-ID (XUID) des Aufrufers| 403 Forbidden|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource

Diese Methode immer 200 OK zurück, aber Inhalte möglicherweise nicht im Antworttext zurück.

| Anfordernden Benutzer| Ziel der Datenschutzeinstellungen des Benutzers| Verhalten|
| --- | --- | --- | --- | --- | --- | --- |
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

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".|
| x-xbl-contract-version| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Beispielwerte: 3, Vnext.|
| Annehmen| string| Inhaltstypen, die zulässig sind. Der einzige unterstützte durch Vorhandensein ist Application/Json, aber es muss angegeben werden, in der Kopfzeile.|
| Accept-Language| string| Zulässige Gebietsschema für Zeichenfolgen in der Antwort. Beispielwerte: En-US.|
| Host| string| Der Domänenname des Servers. Beispielwert: presencebeta.xboxlive.com.|
| Content-Length| string| Die Länge des Anforderungstexts. Beispielwert: 312.|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>Anforderungstext

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>Erforderliche Member

| Mitglied| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Benutzer| Liste XUIDs von Benutzern, deren Anwesenheit, die Sie mit einem Maximum von 1100 XUIDs zu einem Zeitpunkt erfahren möchten.|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>Optionale Elemente

| Mitglied| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| Liste der Gerätetypen, die der Benutzer, die, denen Sie wissen möchten. Wenn das Array leer ist, wird standardmäßig alle möglichen Einheitentypen (d. h. keine werden herausgefiltert).|
| Titel| Liste mit Geräte-Typen, deren Benutzer Sie wissen möchten. Wenn das Array leer ist, wird standardmäßig alle möglichen Titel (d. h. keine werden herausgefiltert).|
| level| Mögliche Werte: <ul><li>Benutzer – Get-User-Knoten</li><li>Gerät - Get-Benutzer und Geräteknoten</li><li>Titel: Informieren Sie sich grundlegende Titel auf</li><li>All: umfassende Anwesenheitsinformationen, Informationen zu den Tasksequenzmedien oder beides zu erhalten</li></ul><br> Der Standardwert ist "Title".|
| onlineOnly| Wenn diese Eigenschaft auf "true" festgelegt ist, filtert der Batchvorgang Datensätze heraus für offline-Benutzer (einschließlich verdeckte werden). Wenn sie nicht angegeben ist, werden online und offline-Benutzer zurückgegeben.|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>Unzulässige Mitglieder

Alle anderen Elemente sind in einer Anforderung nicht zulässig.

<a id="ID4EIEAC"></a>


### <a name="sample-request"></a>Beispiel für eine Anforderung


```cpp
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}

```


<a id="ID4ESEAC"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4E1EAC"></a>


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


<a id="ID4EKFAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EMFAC"></a>


##### <a name="parent"></a>Parent

[/users/batch](uri-usersbatch.md)
