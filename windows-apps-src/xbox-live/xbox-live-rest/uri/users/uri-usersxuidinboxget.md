---
title: GET (/users/xuid({xuid})/inbox)
assetID: c603910d-b430-f157-2634-ceddea89f2bd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxget.html
description: " GET (/users/xuid({xuid})/inbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 05f75510f15f6e6c5f1b1673673428c00f7a6c16
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632245"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
Ruft eine angegebene Anzahl von Zusammenfassungen von Benutzer-Nachricht vom Dienst ab.
Die Domäne für diese URIs ist `msg.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EEB)
  * [Abfragezeichenfolgen-Parameter](#ID4EIC)
  * [Autorisierung](#ID4EGE)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4ETE)
  * [HTTP-Statuscodes](#ID4E5E)
  * [JavaScript Object Notation (JSON)-Antwort](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Eine Benutzernachricht, die Zusammenfassung enthält nur den Betreff der Nachricht. Dies ist für Nachrichten vom Benutzer generierte derzeit 20 ersten Zeichen des Texts der. Meldungen des System möglicherweise eine alternative Antragstellers, z. B. "LIVE-System" bereit.

Nachrichten werden in der umgekehrten Reihenfolge zurückgegeben, die sie gesendet wurden. d. h. werden neuere Nachrichten zuerst zurückgegeben.

Der nur Inhaltstyp, die, den diese API unterstützt, ist "Application/Json", der in der HTTP-Header, der bei jedem Aufruf erforderlich ist.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| xuid| 64-Bit-Ganzzahl ohne Vorzeichen| Die Xbox User ID (XUID) des Spielers, die die Anforderung stammt.|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

| Eigenschaft| Typ| Maximale Länge| Hinweise|
| --- | --- | --- | --- | --- | --- | --- |
| "MaxItems"| int| 100| Maximale Anzahl der Nachrichten zurückgegeben.|
| continuationToken| string|  | Zeichenfolge, die in einem vorherigen Aufruf für die Enumeration zurückgegeben werden. verwendet, um die Enumeration fortzusetzen.|
| skipItems| int| 100| Die Anzahl der Nachrichten zu überspringen; ignoriert, wenn "continuationtoken" vorhanden ist.|

<a id="ID4EGE"></a>


## <a name="authorization"></a>Autorisierung

Sie müssen Ihre eigenen Benutzer Anspruch zum Abrufen einer Zusammenfassung von Benutzer verfügen.

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource

Sie können nur eigene benutzermeldungen auflisten.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Die Anforderung war erfolgreich.|
| 400| Dienst verstanden falsch formatierte Anforderung nicht. In der Regel ein ungültiger Parameter.|
| 403| Die Anforderung ist für den Benutzer oder der Dienst nicht zulässig.|
| 404| Eine gültige XUID fehlt im URI.|
| 407| Die zugrunde liegende Auflistung, die basierend auf das Fortsetzungstoken, das übergeben wurde geändert.|
| 416| Die Anzahl der zu überspringenden Elemente ist größer als die Anzahl der verfügbaren Elemente.|
| 500| Allgemeiner serverseitigen Fehler.|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON)-Antwort

Wenn erfolgreich aufgerufen wird, gibt der Dienst die Ergebnisdaten in einem JSON-Format.

| Eigenschaft| Typ| Maximale Länge| Hinweise|
| --- | --- | --- | --- |
| Ergebnisse| Message[]| 100| Array von benutzermeldungen|
| pagingInfo| PagingInfo|  | Informationen für den aktuellen Satz von Ergebnissen|

#### <a name="message"></a>Meldung

| Eigenschaft| Typ| Maximale Länge| Hinweise|
| --- | --- | --- | --- |
| header| Header|  | Benutzer-Nachrichtenheader|
| messageSummary| string| 20| UTF-8; in der Regel 20 ersten Zeichen der Nachricht|

#### <a name="header"></a>Header

| Eigenschaft| Typ| Maximale Länge| Hinweise|
| --- | --- | --- | --- |
| id| string| 50| Nachrichten-ID, für das Abrufen von Details zur Nachricht oder Löschen von Nachrichten verwendet.|
| isRead| bool|  | Flag zur Angabe, dass der Benutzer die Details der bereits gelesen hat.|
| Gesendet| DateTime|  | UTC-Datum und Uhrzeit, die die Nachricht gesendet wurde. (Bereitgestellt vom Dienst.)|
| Ablaufdatum| DateTime|  | UTC-Datum und Uhrzeit, die die Nachricht abläuft. (Alle Nachrichten haben eine maximale Lebensdauer, in der Zukunft bestimmt werden soll.)|
| messageType| string| 50| Nachrichtentypen: User, System, FriendRequest, Video, QuickChat, VideoChat, PartyChat, Title, GameInvite.|
| senderXuid| ulong|  | XUID des Absenders.|
| Sender| string| 15| Gamertag des Absenders.|
| hasAudio| bool|  | Gibt an, ob die Nachricht eine Anlage Audio (Sprache) ist.|
| hasPhoto| bool|  | Gibt an, ob die Nachricht eine Foto-Anlage ist.|
| hasText| bool|  | Gibt an, ob die Nachricht Text enthält.|

#### <a name="paging-info"></a>Paging-Informationen

| Eigenschaft| Typ| Maximale Länge| Hinweise|
| --- | --- | --- | --- |
| continuationToken| string| 100| Optional können zurückgegeben vom Server. Ermöglicht die spätere Aufrufe an die Enumeration fortzusetzen.|
| totalItems| int|  | Gesamtanzahl der Nachrichten im Posteingang.|

#### <a name="sample-response"></a>Beispielantwort

```cpp
{
          "results":
          [
            {
              "header":
              {
                "expiration":"2011-10-11T23:59:59.9999999",
                "id":"opaqueBlobOfText",
                "messageType":"User",
                "isRead":false,
                "senderXuid":"123456789",
                "sender":"Striker",
                "sent":"2011-05-08T17:30:00Z",
                "hasAudio":false,
                "hasPhoto":false,
                "hasText":true
              },
            "messageSummary":"first 20 chars"
          },
          ...
        ],
        "pagingInfo":
          {
          "continuationToken":"opaqueBlobOfText"
          "totalItems":5,
          }
        }

```

#### <a name="error-response"></a>Fehlerantwort

Im Fall eines Fehlers kann der Dienst ein ErrorResponse-Objekt zurück, die Werte aus der Umgebung des Diensts enthalten kann.

| Eigenschaft| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| string| Ein Hinweis darauf, wo der Fehler verursacht wurde.|
| errorCode| int| Numerischen Code für den Fehler (kann null sein).|
| errorMessage| string| Details des Fehlers, wenn Sie so konfiguriert, dass die Details anzuzeigen.|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Verweis [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md)
