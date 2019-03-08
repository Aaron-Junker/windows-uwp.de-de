---
title: GET (/users/{ownerId}/summary)
assetID: 754190c9-b15d-f34b-1dca-5c92f6f67d12
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummaryget.html
description: " GET (/users/{ownerId}/summary)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3b228adab7b035ec8f4e65fc8b7458228a677987
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613995"
---
# <a name="get-usersowneridsummary"></a>GET (/users/{ownerId}/summary)
Ruft die Zusammenfassungsdaten über den Besitzer aus Sicht des Aufrufers ab.

  * [URI-Parameter](#ID4EQ)
  * [Autorisierung](#ID4E2)
  * [Erforderlichen Anforderungsheader](#ID4EBC)
  * [Optionale Anforderungsheader](#ID4EHD)
  * [Anforderungstext](#ID4EXE)
  * [HTTP-Statuscodes](#ID4ECF)
  * [Erforderlichen Antwortheader](#ID4EZG)
  * [Antworttext](#ID4EGAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| ownerId| string| Der Bezeichner des Benutzers, dessen Ressource zugegriffen wird. Die möglichen Werte sind "me", xuid({xuid}) oder gt({gamertag}). Beispielwerte: <code>me</code>, <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>|

<a id="ID4E2"></a>


## <a name="authorization"></a>Autorisierung

| <b>Name</b>| <b>Typ</b>| <b>Beschreibung</b>|
| --- | --- | --- | --- | --- | --- |
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Erforderlich. Benutzer-ID des Aufrufers. Beispielwert: 2533274790395904|

<a id="ID4EBC"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung| string| Der Autorisierungsdaten für. Dies ist normalerweise ein verschlüsseltes XSTS-Token. Beispielwert: <b>XBL3.0 x = [Hash]; [Token] </b>.|

<a id="ID4EHD"></a>


## <a name="optional-request-headers"></a>Optionale Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-xbl-contract-version| string| Erstellen Sie Namen und die Nummer des Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur für diesen Dienst weitergeleitet werden, nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in den Authentifizierungstoken usw. verwendet wird. Beispielwerte: 1|
| Annehmen| string| Inhaltstypen, die zulässig sind. Alle Antworten werden <code>application/json</code>.|

<a id="ID4EXE"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4ECF"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.|
| 400| Ungültige Anforderung.| Benutzer-IDs wurden falsch formatiert.|
| 403| Verboten| XUID Anspruch konnte nicht aus dem Autorisierungsheader analysiert werden.|

<a id="ID4EZG"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| string| Die Anzahl der Bytes, die in der Antwort gesendet werden. Beispielwert: 232.|
| Content-Type| string| MIME-Typ des Antworttexts. Dies muss <b>Application/Json</b>.|

<a id="ID4EGAAC"></a>


## <a name="response-body"></a>Antworttext

Finden Sie unter [PersonSummary (JSON)](../../json/json-personsummary.md).

<a id="ID4ESAAC"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}

```


<a id="ID4E3AAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E5AAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/summary](uri-usersowneridsummary.md)
