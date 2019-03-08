---
title: GET (/scids/{scid}/leaderboards/{leaderboardname})
assetID: 4adea46c-e910-8709-c28e-ce9178e712ef
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnameget.html
description: " GET (/scids/{scid}/leaderboards/{leaderboardname})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b0d313262a642ee0db956f6d3264025931e63e8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629495"
---
# <a name="get-scidsscidleaderboardsleaderboardname"></a>GET (/scids/{scid}/leaderboards/{leaderboardname})
 
Ruft einen vordefinierten globalen Rangliste ab.
 
Die Domäne für diese URIs ist `leaderboards.xboxlive.com`.
 
  * ["Hinweise"](#ID4EY)
  * [URI-Parameter](#ID4EDB)
  * [Abfragezeichenfolgen-Parameter](#ID4EOB)
  * [Autorisierung](#ID4EID)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4EDE)
  * [Erforderlichen Anforderungsheader](#ID4EME)
  * [Optionale Anforderungsheader](#ID4E1F)
  * [HTTP-Statuscodes](#ID4E1G)
  * [Antwortheader](#ID4ERCAC)
  * [Antworttext](#ID4EOEAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>Hinweise
 
Leaderboard-APIs sind alle schreibgeschützt und unterstützt daher nur das GET-Verb. Diese wider Rang und sortierter "Seiten" von indizierten Player-Statistiken, die von Statistiken für einzelne Benutzer abgeleitet werden, die über die Datenplattform geschrieben wurden. Vollständige Leaderboard-Indizes können sehr groß sein und Aufrufer werden nicht mehr nachfragen, um eine in seiner Gesamtheit anzuzeigen, diesen URI unterstützt daher mehrere Abfrage-Zeichenfolgenargumente, die gestattet es einem Aufrufer, über welche Art von Ansicht spezifisch sein, die sie für diese Bestenliste anzeigen möchte.
 
GET-Vorgängen wird keine Ressourcen ändern, damit diese die gleichen Ergebnisse erzielt werden, wenn einmal oder mehrmals ausgeführt.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| scid| GUID| Der Bezeichner für die Dienstkonfiguration enthält die Ressource, auf die zugegriffen wird.| 
| leaderboardname| string| Eindeutiger Bezeichner der vordefinierten Leaderboard-Ressource, die auf die zugegriffen wird.| 
  
<a id="ID4EOB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | 
| "MaxItems"| 32-Bit-Ganzzahl ohne Vorzeichen| Maximale Anzahl der Leaderboard-Datensätze, die auf einer Seite mit Ergebnissen zurück. Wenn nicht angegeben, wird eine Standardanzahl (10) zurückgegeben. Der maximale Wert für "MaxItems" noch nicht definiert ist, aber wir großen Datasets vermeiden möchten, damit dieser Wert möglicherweise soll das größte festlegen, die einen Tuner, die Benutzeroberfläche, die pro Aufruf verarbeiten kann.| 
| skipToRank| 32-Bit-Ganzzahl ohne Vorzeichen| Zurückgeben einer Seite mit Ergebnissen, die mit dem angegebenen Leaderboard-Rang ab. Der Rest der Ergebnisse werden in der Sortierreihenfolge nach Rang. Dieser Abfragezeichenfolge kann ein Fortsetzungstoken, das die eingelesen werden kann wieder in einer nachfolgenden Abfrage abrufen "der nächste Seite" Ergebnisse zurück.| 
| skipToUser| string| Eine Seite des Leaderboard-Ergebnisse, um die angegebenen Spieler Xuid, unabhängig davon, Rang oder die Bewertung des Benutzers zurückgegeben. Die Seite wird nach QUANTILSRANG mit dem angegebenen Benutzer in der letzten Position der Seite für die vordefinierten Ansichten oder in der Mitte für Stat Leaderboard-Ansichten sortiert werden. Es gibt keine "continuationtoken" für diese Art von Abfrage bereitgestellt.| 
| continuationToken| string| Wenn ein vorheriger Aufruf einer "continuationtoken" zurückgegeben, kann der Aufrufer wieder dieses Token "wie besehen" in einer Abfragezeichenfolge zum Abrufen der nächsten Seite der Ergebnisse übergeben.| 
  
<a id="ID4EID"></a>

 
## <a name="authorization"></a>Autorisierung
 
Es gibt Autorisierungslogik für Inhalte-Isolation und Access Control-Szenarios implementiert.
 
   * Sowohl Benutzer als auch Bestenlisten Statistiken können von Clients auf allen Plattformen, gelesen werden, vorausgesetzt, dass der Aufrufer ein gültiges XSTS-Token mit der Anforderung sendet. Schreibvorgänge werden auf Clients, die von der Data-Plattform unterstützt natürlich beschränkt.
   * Titel-Entwickler können die Statistiken als offen oder eingeschränkt XDP oder Partner Center kennzeichnen. Leaderboards sind Statistiken öffnen. Statistiken öffnen können durch Smartglass, als auch iOS, Android, Windows, Windows Phone und Webanwendungen zugegriffen werden, solange der Benutzer mit der Sandbox autorisiert ist. Benutzerautorisierung einer Sandbox wird über XDP oder über das Partner Center verwaltet.
  
Der Pseudocode für die Überprüfung sieht folgendermaßen aus:
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EDE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource
 
Beim Lesen von Daten der Bestenliste anzeigen, werden keine Datenschutz-Überprüfungen ausgeführt.
  
<a id="ID4EME"></a>

 
## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorisierung| Eine Zeichenfolge. Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: <b>XBL3.0 x=&lt;userhash>;&lt;token></b>| 
| X-RequestedServiceVersion| Eine Zeichenfolge. Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Standardwert: 1.| 
| Annehmen| Eine Zeichenfolge. Inhaltstypen, die zulässig sind. Beispielwert: <b>Application/Json</b>| 
  
<a id="ID4E1F"></a>

 
## <a name="optional-request-headers"></a>Optionale Anforderungsheader
 
| Header| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| Eine Zeichenfolge. Entitätstag, verwendet, wenn der Client Zwischenspeichern unterstützt. Beispielwert: <b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4E1G"></a>

 
## <a name="http-status-codes"></a>HTTP-Statuscodes
 
Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).
 
| Code| Ursachentext| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.| 
| 304| Nicht geändert.| Ressource nicht geändert seit dem letzten angefordert.| 
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.| 
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.| 
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.| 
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.| 
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.| 
| 408| Anforderungstimeout| Die Ressourcenversion wird nicht unterstützt. von der MVC-Ebene abgelehnt werden sollte.| 
  
<a id="ID4ERCAC"></a>

 
## <a name="response-headers"></a>Antwortheader
 
| Header| Typ| Beschreibung| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| string| Erforderlich. Der MIME-Typ des Texts der Antwort. Beispiel: <b>Application/Json</b>.| 
| Content-Length| string| Erforderlich. Die Anzahl der Bytes, die in der Antwort gesendet werden. Beispiel: <b>232</b>.| 
| ETag| string| Optional. Für Cache-Optimierung verwendet. Beispiel: <b>686897696a7c876b7e</b>.| 
  
<a id="ID4EOEAC"></a>

 
## <a name="response-body"></a>Antworttext
 
<a id="ID4EUEAC"></a>

 
### <a name="response-members"></a>Antwort-Member
 
pagingInfo | pagingInfo| Im Abschnitt| Optional. Nur vorhanden Sie, wenn "MaxItems" in der Anforderung angegeben wird.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| continuationToken| 64-Bit-Ganzzahl ohne Vorzeichen| Erforderlich. Gibt an, welchen Wert zum Übertragen von zurück in die <b>SkipItems</b> Abfrageparameter, um die nächste Seite der Ergebnisse für diesen URI zu erhalten, falls gewünscht. Wenn kein <b>PagingInfo</b> wird zurückgegeben, dann gibt es keine nächste Seite der Daten abgerufen werden sollen.| 
| totalCount| 64-Bit-Ganzzahl ohne Vorzeichen| Erforderlich. Die Gesamtanzahl der Einträge in die Bestenliste. Beispielwert: 1234567890| 
 
leaderboardInfo | leaderboardInfo| Im Abschnitt| Erforderlich. Immer zurückgegeben. Enthält die Metadaten über die Bestenliste angefordert.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| string| Nicht verwendet.| 
| totalCount| Zeichenfolge (64-Bit-Ganzzahl ohne Vorzeichen)| Erforderlich. Die Gesamtanzahl der Einträge in die Bestenliste. Beispielwert: 1234567890| 
| Spalten| array| Erforderlich. Spalten in die Bestenliste.| 
| displayName| string| Erforderlich. Eine Spalte in die Bestenliste entspricht.| 
| statName| string| Erforderlich. Eine Spalte in die Bestenliste entspricht.| 
| type| string| Erforderlich. Eine Spalte in die Bestenliste entspricht.| 
 
userList | userList| Im Abschnitt| Erforderlich. Immer zurückgegeben. Enthält die tatsächlichen Benutzer-Ergebnisse für die Bestenliste angefordert.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Gamertag| string| Erforderlich. Der Benutzer im Leaderboard-Eintrag entspricht.| 
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Erforderlich. Der Benutzer im Leaderboard-Eintrag entspricht.| 
| Perzentil| string| Erforderlich. Der Benutzer im Leaderboard-Eintrag entspricht.| 
| Rang| string| Erforderlich. Der Benutzer im Leaderboard-Eintrag entspricht.| 
| Werte| array| Erforderlich. Jede durch Trennzeichen getrennte Werte entspricht einer Spalte in die Bestenliste.| 
  
<a id="ID4EGKAC"></a>

 
### <a name="sample-response"></a>Beispielantwort
 
Die folgende Anforderungs-URI zeigt die Rangfolge in einer globalen Rangliste überspringen.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured/columns/deaths?maxItems=3&skipToRank=15000
         
```

 
Der URI, der oben genannten gibt die folgende JSON-Antwort zurück.
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "152",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columns": [
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            },
            {
                "displayName": "Deaths",
                "statName": "deaths",
                "type": "Integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": 1234567890123444,
            "percentile": 0.64,
            "rank": 15000,
            "values": [
                1000,
                47
            ]
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": 1234567890123555,
            "percentile": 0.64,
            "rank": 15001,
            "values": [
                998,
                17
            ]
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": 1234567890123666,
            "percentile": 0.64,
            "rank": 15002,
            "values": [
                996,
                2
            ]
        }
    ]
}
         
```

 
Die folgende Anforderungs-URI zeigt, an den Benutzer auf einer globalen Rangliste überspringen.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/totalflagscaptured?maxItems=3&skipToUser=2343737892734237
         
```

 
Der URI, der oben genannten gibt die folgende JSON-Antwort zurück.
 

```cpp
{
    "leaderboardInfo": 
    {   
       "displayName": "Total flags captured",
       "totalCount": 23437,
       "columns": [
              {
                  "displayName": "Flags Captured",
                  "statName": "flagscaptured",
                  "type": "Integer"
              }
       ]
    },
    "userList": [
        {
            "gamertag": "AverageJoe",
            "percentile": 55.00,
            "rank": 11718,
            "value": 1219,
            "xuid": 1234567890123444
        },
        {
            "gamertag": "AreUWatchinMe",
            "percentile": 60.00,
            "rank": 14162,
            "value": 1062,
            "xuid": 2343737892734333
        },
         {
            "gamertag": "WarriorSaint",
            "percentile": 64.39,
            "rank": 15000,
            "value ": 1000,
            "xuid": 1234567890123455
        }
    ]
}
         
```

   
<a id="ID4E5KAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EALAC"></a>

 
##### <a name="parent"></a>Parent 

[/scids/{scid}/leaderboards/{leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   