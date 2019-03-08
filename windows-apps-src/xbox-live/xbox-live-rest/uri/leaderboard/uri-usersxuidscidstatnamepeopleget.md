---
title: GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
assetID: 942cf0d7-f988-0495-cf28-cdac608b8109
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeopleget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 674e52d15d115560d4813edcd9687c2fe9694c9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650765"
---
# <a name="get-usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
Gibt eine soziale Bestenliste Rangfolge, die die Stat (Bewertungen) für entweder alle bekannten Kontakte des aktuellen Benutzers oder nur die Kontakte, die als bevorzugte Personen festgelegt wird, von diesem Benutzer Werte zurück.
Die Domäne für diese URIs ist `leaderboards.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EAB)
  * [Abfragezeichenfolgen-Parameter](#ID4ELB)
  * [Autorisierung](#ID4EQD)
  * [Erforderlichen Anforderungsheader](#ID4EGE)
  * [Optionale Anforderungsheader](#ID4EXF)
  * [Anforderungstext](#ID4ETG)
  * [HTTP-Statuscodes](#ID4ECEAC)
  * [Erforderlichen Antwortheader](#ID4E1HAC)
  * [Optionale Antwortheader](#ID4EDJAC)
  * [Antworttext](#ID4EDKAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Leaderboard-APIs sind alle schreibgeschützt und unterstützen daher nur das GET-Verb (HTTPS). Diese wider Rang und sortierter "Seiten" von indizierten Player-Statistiken, die von Statistiken für einzelne Benutzer abgeleitet werden, die über die Datenplattform geschrieben wurden. Vollständige Leaderboard-Indizes können sehr groß sein und Aufrufer werden nicht mehr nachfragen, um eine in seiner Gesamtheit anzuzeigen, diesen URI unterstützt daher mehrere Abfrage-Zeichenfolgenargumente, die gestattet es einem Aufrufer, über welche Art von Ansicht spezifisch sein, die sie für diese Bestenliste anzeigen möchte.

GET-Vorgängen wird keine Ressourcen ändern, damit diese die gleichen Ergebnisse erzielt werden, wenn einmal oder mehrmals ausgeführt.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| xuid| string| Der Bezeichner des Benutzers.|
| scid| string| Der Bezeichner der Dienstkonfiguration aus, die die gewünschte Ressource enthält.|
| statname| string| Eindeutiger Bezeichner der Stat benutzerressource aus, auf die zugegriffen wird.|
| all|Favorit| Enumeration| Angibt, ob die Stat rank-Werte (Bewertungen) für alle bekannten Kontakte des aktuellen Benutzers oder nur die Kontakte, die als bevorzugte Personen von diesem Benutzer festgelegt.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| "MaxItems"| 32-Bit-Ganzzahl ohne Vorzeichen| Maximale Anzahl der Leaderboard-Datensätze, die auf einer Seite mit Ergebnissen zurück. Wenn nicht angegeben, wird eine Standardanzahl (10) zurückgegeben. Der maximale Wert für "MaxItems" noch nicht definiert ist, aber wir großen Datasets vermeiden möchten, damit dieser Wert möglicherweise soll das größte festlegen, die einen Tuner, die Benutzeroberfläche, die pro Aufruf verarbeiten kann. |
| skipToRank| 32-Bit-Ganzzahl ohne Vorzeichen| Zurückgeben einer Seite mit Ergebnissen, die mit dem angegebenen Leaderboard-Rang ab. Der Rest der Ergebnisse werden in der Sortierreihenfolge nach Rang. Dieser Abfragezeichenfolge kann ein Fortsetzungstoken, das die eingelesen werden kann wieder in einer nachfolgenden Abfrage abrufen "der nächste Seite" Ergebnisse zurück. |
| skipToUser| string| Eine Seite des Leaderboard-Ergebnisse, um die angegebenen Spieler Xuid, unabhängig davon, Rang oder die Bewertung des Benutzers zurückgegeben. Die Seite wird nach QUANTILSRANG mit dem angegebenen Benutzer in der letzten Position der Seite für die vordefinierten Ansichten oder in der Mitte für Stat Leaderboard-Ansichten sortiert werden. Es gibt keine <b>"continuationtoken"</b> für diese Art von Abfrage bereitgestellt. |
| continuationToken| string| Wenn ein vorheriger Aufruf zurückgegeben wurde, eine <b>"continuationtoken"</b>, und der Aufrufer dieses Token "wie besehen" wieder in einer Abfragezeichenfolge zum Abrufen der nächsten Seite der Ergebnisse übergeben kann. |
| sort| string| Gibt an, ob die Liste der Spieler von niedrig zu hoch Wert-Reihenfolge ("aufsteigend") rank oder hoch bis Niedrig Wert-Reihenfolge ("absteigend"). Dies ist ein optionaler Parameter. Der Standardwert ist eine absteigende Reihenfolge.|

<a id="ID4EQD"></a>


## <a name="authorization"></a>Autorisierung

Xuid Autorisierung ist erforderlich.

Im Rahmen der inhaltsisolation und Szenarien der Zugriffssteuerung wird die Autorisierungslogik implementiert.

Sowohl Benutzer als auch Bestenlisten Statistiken können von Clients auf allen Plattformen, gelesen werden, vorausgesetzt, dass der Aufrufer ein gültiges XSTS-Token mit der Anforderung sendet. Schreibvorgänge werden auf Clients, die von der Data-Plattform unterstützt (offensichtlich) beschränkt.

Titel-Entwickler können die Statistiken als offen oder eingeschränkt XDP oder Partner Center kennzeichnen. Leaderboards sind Statistiken öffnen. Statistiken öffnen können durch Smartglass, als auch iOS, Android, Windows, Windows Phone und Webanwendungen zugegriffen werden, solange der Benutzer mit der Sandbox autorisiert ist. Benutzerautorisierung einer Sandbox wird über XDP oder über das Partner Center verwaltet.

<a id="ID4EGE"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| Eine Zeichenfolge. Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".|
| Content-Type| Eine Zeichenfolge. Der MIME-Typ des Anforderungstexts. Beispielwert: "Application/Json".|
| X-RequestedServiceVersion| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.|
| Annehmen| Eine Zeichenfolge. Content-Type-Werte zulässig. Beispielwert: "Application/Json".|

<a id="ID4EXF"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| If-None-Match| Eine Zeichenfolge. Entitätstag - verwendet, wenn der Client Zwischenspeichern unterstützt. Beispielwert: "686897696a7c876b7e".|

<a id="ID4ETG"></a>


## <a name="request-body"></a>Anforderungstext

Um die Fähigkeit von einer der Aufrufer, die Daten zu verstehen, die sie für die ordnungsgemäße Anzeige zurück erhält zu maximieren, wird jeder Stat-Wert für jeden Benutzer als Zeichenfolge im Format zurückgegeben werden in der er sollte werden angezeigt, und formatiert entsprechend des Gebietsschemas, die in der Accept-Language angegeben der Header in der Anforderung. Eine lokalisierte "Anzeigename" werden auch für Statname für diese Bestenliste zurückgegeben werden.

<a id="ID4E2G"></a>


### <a name="required-members"></a>Erforderliche Member

| Mitglied| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <b>pagingInfo</b>| Im Abschnitt| Optional. Zurückgegeben, wenn der Rang des letzten Eintrag auf der Seite TotalItems unterschritten wird. In diesem Abschnitt wird auch nicht zurückgegeben, wenn SkipToUser in der Anforderung angegeben wird.|
| continuationToken| string| Erforderlich. Gibt an, welcher Wert um zurück an den Abfrageparameter "ContinuationToken", um nächsten Seite mit Ergebnissen für diesen URI zu erhalten, bei Bedarf zu übergeben. Wenn keine PagingInfo zurückgegeben wird ist kein "Nächste Seite" der Daten abgerufen werden sollen.|
| totalItems| 64-Bit-Ganzzahl ohne Vorzeichen| Erforderlich. Die Gesamtanzahl der Einträge in die Bestenliste.|
| <b>leaderboardInfo</b>| Im Abschnitt| Erforderlich. Immer zurückgegeben. Enthält die Metadaten über die Bestenliste angefordert.|
| displayName| string| Erforderlich. Lokalisierte Anzeigename für die vordefinierten Bestenliste anzeigen. Beispielwert: "Total Flags erfasst."|
| totalCount| string| Erforderlich. Die Gesamtanzahl der Einträge in die Bestenliste.|
| Spalten| array| Erforderlich.|
| displayName| string| Erforderlich. Eine Spalte in die Bestenliste entspricht.|
| statName| string| Erforderlich. Eine Spalte in die Bestenliste entspricht.|
| type| string| Erforderlich. Eine Spalte in die Bestenliste entspricht.|
| <b>userList</b>| Im Abschnitt| Erforderlich. Immer zurückgegeben. Enthält die tatsächlichen Benutzer-Ergebnisse für die Bestenliste angefordert.|
| Gamertag| string| Erforderlich. Der Benutzer im Leaderboard-Eintrag entspricht.|
| xuid| 32-Bit-Ganzzahl mit Vorzeichen| Erforderlich. Der Benutzer im Leaderboard-Eintrag entspricht.|
| Perzentil| string| Erforderlich. Der Benutzer im Leaderboard-Eintrag entspricht.|
| Rang| string| Erforderlich. Der Benutzer im Leaderboard-Eintrag entspricht.|
| Werte| array| Erforderlich. Jede durch Trennzeichen getrennte Werte entspricht einer Spalte in die Bestenliste.|

<a id="ID4ECEAC"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.|
| 304| Nicht geändert.|  |
| 400| Ungültige Anforderung.| | Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.|
| 401| Nicht autorisiert| | Die Anforderung ist eine Benutzerauthentifizierung erforderlich.|
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.|
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.|
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt. von der MVC-Ebene abgelehnt werden sollte.|
| 408| Anforderungstimeout| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.|

<a id="ID4E1HAC"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| Der MIME-Typ des Texts der Antwort. Beispielwerte: "Application/Json".|
| Content-Length| string| Die Anzahl der Bytes, die in der Antwort gesendet werden. Beispielwert: "232".|

<a id="ID4EDJAC"></a>


## <a name="optional-response-headers"></a>Optionale Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| string| Für Cache-Optimierung verwendet. Beispielwert: "686897696a7c876b7e".|

<a id="ID4EDKAC"></a>


## <a name="response-body"></a>Antworttext

Anforderung für die sozialen Bestenliste anzeigen, die kein Paging:

https://leaderboards.xboxlive.com/users/xuid(2533274916402282)/scids/c1ba92a9-0000-0000-0000-000000000000/stats/EnemyDefeats/people/all?sort=descending

<a id="ID4ENKAC"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
    "pagingInfo": null,
    "leaderboardInfo": {
        "displayName": "Kills",
        "totalCount": 3,
        "columns": [
            {
                "displayName": "Kills",
                "statName": "enemydefeats",
                "type": "integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag":"xxxSniper39",
            "xuid":"1234567890123555",
            "percentile":1.0,
            "rank":1,
            "values": [
                "47"
            ]
        },
        {
            "gamertag":"WarriorSaint",
            "xuid":"2533274916402282",
            "percentile":0.66,
            "rank":2,
            "values": [
                "42"
            ]
        },
        {
            "gamertag":"WhockaWhocka",
            "xuid":"1234567890123666",
            "percentile":0.33,
            "rank":3,
            "values": [
                "12"
            ]
        }
    ]
}

```


<a id="ID4EXKAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EZKAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite}](uri-usersxuidscidstatnamepeople.md)
