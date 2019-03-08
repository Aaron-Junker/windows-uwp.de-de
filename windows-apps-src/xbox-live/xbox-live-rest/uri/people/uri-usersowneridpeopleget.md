---
title: GET (/users/{ownerId}/people)
assetID: c948d031-ec19-7571-31ef-23cb9c5ebfaf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopleget.html
description: " GET (/users/{ownerId}/people)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 6c8672188a93b2e8d27a081ae068387e7ee7aa42
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623765"
---
# <a name="get-usersowneridpeople"></a>GET (/users/{ownerId}/people)
Ruft des Aufrufers Personen Auflistung ab.
Die Domäne für diese URIs ist `social.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4E5)
  * [Abfragezeichenfolgen-Parameter](#ID4EJB)
  * [Autorisierung](#ID4ERD)
  * [Erforderlichen Anforderungsheader](#ID4EZE)
  * [Optionale Anforderungsheader](#ID4EYF)
  * [Anforderungstext](#ID4E5G)
  * [HTTP-Statuscodes](#ID4EJH)
  * [Erforderlichen Antwortheader](#ID4EBBAC)
  * [Antworttext](#ID4ENCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

GET-Vorgängen wird keine Ressourcen ändern, damit diese die gleichen Ergebnisse erzielt werden, wenn einmal oder mehrmals ausgeführt.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| ownerId| string| Der Bezeichner des Benutzers, dessen Ressource zugegriffen wird. Muss den authentifizierten Benutzer übereinstimmen. Die möglichen Werte sind "me", xuid({xuid}) oder gt({gamertag}).|

<a id="ID4EJB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| Ansicht| string| Zurückgeben der Personen, die mit einer Ansicht verknüpft ist. Der Standardwert ist "all". Die möglichen Werte sind: <ul><li><b>Alle</b> -alle Personen, die in der Liste der Personen zurückgibt. Dies ist der Standardwert.</li><li><b>Bevorzugte</b> -gibt alle Personen, die in der Liste der Personen, die das Favoriten-Attribut aufweisen.</li><li><b>LegacyXboxLiveFriends</b> -gibt alle Personen, die in der Liste der Personen, die auch ältere Xbox LIVE-Freunde.</li></br>**Hinweis:**  Nur die **alle** Wert wird unterstützt, wenn der aufrufende Benutzer der zuständige Benutzer unterscheidet.|
| startIndex| 32-Bit-Ganzzahl ohne Vorzeichen| Gibt die Elemente, beginnend am angegebenen Index zurück.  
| "MaxItems"| 32-Bit-Ganzzahl ohne Vorzeichen| Maximale Anzahl der Personen an, aus der Auflistung, beginnend beim startIndex zurückgegeben. Der Dienst kann einen Standardwert angeben, wenn <b>"MaxItems"</b> ist nicht vorhanden und kann weniger als zurück <b>"MaxItems"</b> (selbst wenn die letzte Seite der Ergebnisse noch nicht zurückgegeben).|

<a id="ID4ERD"></a>


## <a name="authorization"></a>Autorisierung

| Typ| Erforderlich| Beschreibung| Die Antwort bei fehlen|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| Ja| Aufrufer hat des Benutzers Xbox Benutzer-ID (XUID).| 401 nicht autorisiert|

<a id="ID4EZE"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| Eine Zeichenfolge. Der Autorisierungsdaten für Xbox LIVE. Dies ist normalerweise ein verschlüsseltes XSTS-Token. Beispielwert: <b>XBL3.0 x=&lt;userhash>;&lt;token></b>.|

<a id="ID4EYF"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Standardwert: 1.|
| Annehmen| Eine Zeichenfolge. Inhaltstypen, die der Aufrufer in der Antwort akzeptiert. Alle Antworten bleiben <b>Application/Json</b>.|

<a id="ID4E5G"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4EJH"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Erfolg.|
| 400| Ungültige Anforderung.| Abfrageparameter oder Benutzer-IDs wurden falsch formatiert.|
| 403| Verboten| XUID Anspruch konnte nicht aus dem Autorisierungsheader analysiert werden.|

<a id="ID4EBBAC"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| 32-Bit-Ganzzahl ohne Vorzeichen| Länge in Bytes, des Antworttexts. Beispielwert: 22.|
| Content-Type| string| MIME-Typ des Antworttexts. Immer <b>Application/Json</b>.|

<a id="ID4ENCAC"></a>


## <a name="response-body"></a>Antworttext

Wenn der Aufruf erfolgreich ist, gibt der Dienst die Gesamtanzahl der Mitarbeiter des Aufrufers Personen Auflistung und ein Array, das der Aufrufer Personen Auflistung enthält. Finden Sie unter [PeopleList (JSON)](../../json/json-peoplelist.md).

<a id="ID4EZCAC"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFollowingCaller": false,
            "isFavorite": false
        },
    ],
    "totalCount": 3
}

```


<a id="ID4EDDAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EFDAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people](uri-usersowneridpeople.md)
