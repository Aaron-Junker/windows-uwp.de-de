---
title: GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
assetID: 890e3f93-4fdc-955f-d849-ba9579d5c1eb
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsgetvaluemetadata.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 55ad44d4c29a2d7a43c76c4df2a78e08462fa65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612715"
---
# <a name="get-usersxuidxuidscidsscidstatsincludevaluemetadata"></a>GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
Ruft eine Liste der angegebenen Statistiken, wie die Statistik-Werte, für einen Benutzer in einer angegebenen Dienstkonfiguration zugeordneten Metadaten ab.
Die Domäne für diese URIs ist `userstats.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EAB)
  * [Abfragezeichenfolgen-Parameter](#ID4ELB)
  * [Autorisierung](#ID4EWC)
  * [Erforderlichen Anforderungsheader](#ID4ERD)
  * [Optionale Anforderungsheader](#ID4EDF)
  * [Anforderungstext](#ID4EHG)
  * [HTTP-Statuscodes](#ID4ESG)
  * [Antworttext](#ID4EJCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Die? enthalten = Valuemetadata Abfrageparameter kann die Antwort auf alle Metadaten, die die Benutzer Stat-Werte, z. B. das Modell und die Farbe eines Fahrzeugs verwendet, um auf eine rennbahn erreichen zugeordnet sind.

Um die Wertmetadaten in der Antwort enthalten, muss die Anforderungsaufruf auch den Wert von Header X-Xbl-Contract-Version auf 3 festgelegt.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| xuid| GUID| Xbox User ID (XUID) des Benutzers auf dessen Namen auf der Dienstkonfiguration.|
| scid| GUID| Der Bezeichner der Dienstkonfiguration aus, die die gewünschte Ressource enthält.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| statNames| string| Eine durch Trennzeichen getrennte Liste von Benutzernamen für die Statistik an. Der folgende URI z. B. teilt dem Dienst mit, dass vier Statistiken für die Benutzer-Id im URI angegebene angefordert werden. {:: Nomakrdown}<br/><br/>`https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots?include=valuemetadata`| 
| include=valuemetadata| string| Gibt an, dass die Antwort enthält den Wertmetadaten der Schalter-Stat-Werten zugeordnet werden.|

<a id="ID4EWC"></a>


## <a name="authorization"></a>Autorisierung

Es gibt Autorisierungslogik für Inhalte-Isolation und Access Control-Szenarios implementiert.

   * Sowohl Benutzer als auch Bestenlisten Statistiken können von Clients auf allen Plattformen, gelesen werden, vorausgesetzt, dass der Aufrufer ein gültiges XSTS-Token mit der Anforderung sendet. Schreibvorgänge können nur Clients, die von der Data-Plattform unterstützt.
   * Titel-Entwickler können die Statistiken als offen oder eingeschränkt XDP oder Partner Center kennzeichnen. Leaderboards sind Statistiken öffnen. Statistiken öffnen können durch Smartglass, als auch iOS, Android, Windows, Windows Phone und Webanwendungen zugegriffen werden, solange der Benutzer mit der Sandbox autorisiert ist. Benutzerautorisierung einer Sandbox wird über XDP oder über das Partner Center verwaltet.

Der Pseudocode für die Überprüfung sieht folgendermaßen aus:


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4ERD"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".|
| X-Xbl-Contract-Version| string| Gibt an, welche Version der API zu verwenden. Dieser Wert muss auf "3" festgelegt werden, um die Wertmetadaten in der Antwort enthalten.|

<a id="ID4EDF"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.|

<a id="ID4EHG"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4ESG"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.|
| 304| Nicht geändert.| Ressource nicht geändert seit dem letzten angefordert.|
| 400| Ungültige Anforderung.| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.|
| 401| Nicht autorisiert| Die Anforderung ist eine Benutzerauthentifizierung erforderlich.|
| 403| Verboten| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.|
| 404| Nicht gefunden| Die angegebene Ressource konnte nicht gefunden werden.|
| 406| Nicht akzeptabel| Die Ressourcenversion wird nicht unterstützt.|
| 408| Anforderungstimeout| Die Ressourcenversion wird nicht unterstützt. von der MVC-Ebene abgelehnt werden sollte.|

<a id="ID4EJCAC"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4EPCAC"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
  "user": {
    "xuid": "123456789",
    "gamertag": "WarriorSaint",
    "stats": [
      {
        "statname": "Wins",
        "type": "Integer",
        "value": 40,
        "valuemetadata" : "{\"region\" : \"EU\", \"isRanked\" : true}"
      },
      {
        "statname": "Kills",
        "type": "Integer",
        "value": 700,
        "valuemetadata" : "{\"longestKillStreak" : 15, \"favoriteTarget\" : \"CrazyPigeon\"}"
      },
      {
        "statname": "KDRatio",
        "type": "Double",
        "value": 2.23,
        "valuemetadata" : "{\"totalKills\" : 700, \"totalDeaths\" : 314}"
      },
      {
        "statname": "Headshots",
        "type": "Integer",
        "value": 173,
        "valuemetadata" : ""
      }
    ],
  }
}

```


<a id="ID4EZCAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E2CAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
