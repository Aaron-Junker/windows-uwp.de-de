---
title: GET (/users/xuid({xuid})/scids/{scid}/stats)
assetID: af117e87-6f1d-6448-9adf-7cf890d1380f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: baf965dcbd23bf00d7d0953726f9f20852324e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662385"
---
# <a name="get-usersxuidxuidscidsscidstats"></a>GET (/users/xuid({xuid})/scids/{scid}/stats)
Ruft ab eine Dienstkonfiguration, die von einer durch Trennzeichen getrennte Liste der Statistik Benutzernamen für den angegebenen Benutzer begrenzt.
Die Domäne für diese URIs ist `userstats.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EEB)
  * [Abfragezeichenfolgen-Parameter](#ID4EPB)
  * [Autorisierung](#ID4EUC)
  * [Erforderlichen Anforderungsheader](#ID4EPD)
  * [Optionale Anforderungsheader](#ID4EYE)
  * [Anforderungstext](#ID4E3F)
  * [HTTP-Statuscodes](#ID4EHG)
  * [Antworttext](#ID4E5BAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Clients benötigen eine Möglichkeit zum Lesen und Schreiben von Titel Statistiken für Spieler in unser neues Player statistiksystem.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| xuid| GUID| Xbox User ID (XUID) des Benutzers auf dessen Namen auf der Dienstkonfiguration.|
| scid| GUID| Der Bezeichner der Dienstkonfiguration aus, die die gewünschte Ressource enthält.|

<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| statNames| string| Nur Abfragezeichenfolgen-Parameters ist das Komma-getrennt Benutzer Statistik Namen URI Nomen. Der folgende URI z. B. teilt dem Dienst mit, dass vier Statistiken für die Benutzer-Id im URI angegebene angefordert werden. `https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots`Wird ein Limit für die Anzahl der Statistiken, die in einem einzigen Aufruf angefordert werden können, und dieser Grenzwert wird sorgfältig "Sweet-Spot" für Entwickler der Einfachheit halber Visual Studio. Möglichkeit der Länge des URI. Beispielsweise kann das Limit von entweder 600 Zeichen sollte der Text für Statistik (einschließlich Kommas) oder 10 Statistiken, die maximale ermittelt werden. Aktivieren wie folgt einen einfachen GET ermöglicht, HTTP-Zwischenspeicherung für häufig angeforderte Statistiken, die Anrufaufkommen zu viele Clients reduziert. |

<a id="ID4EUC"></a>


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


<a id="ID4EPD"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: "XBL3.0 x=&lt;userhash>;&lt;token>".|

<a id="ID4EYE"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Erstellen Sie Namen und die Nummer des Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche im Token Authentication service, und so weiter. Standardwert: 1.|

<a id="ID4E3F"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4EHG"></a>


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

<a id="ID4E5BAC"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4EECAC"></a>


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
                "value": 40
            },
            {
                "statname": "Kills",
                "type": "Integer",
                "value": 700
            },
            {
                "statname": "KDRatio",
                "type": "Double",
                "value": 2.23
            },
            {
                "statname": "Headshots",
                "type": "Integer",
                "value": 173
            }
        ],
    }
}

```


<a id="ID4EOCAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EQCAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
