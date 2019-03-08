---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 29b4c57468148a431a10e0d74f85d360ff0992b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618055"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
Ruft den Text ausführliche Meldung für eine bestimmte Benutzernachricht, markieren es als lesen, für den Dienst ab.
Die Domäne für diese URIs ist `msg.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EEB)
  * [Autorisierung](#ID4ERB)
  * [Anforderungstext](#ID4E3B)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4EJC)
  * [HTTP-Statuscodes](#ID4EUC)
  * [JavaScript Object Notation (JSON)-Antwort](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Der Get-Vorgang kann nur auf die Nachrichtentypen Benutzer-, System- und FriendRequest ausgeführt werden.

Dieser URI muss es sich um eine Aktualisierung auf "Xbox.com". Derzeit wird die Xbox 360 nicht den Lese-/ungelesener Status aktualisiert, bis ein Benutzer meldet sich ab und wieder.

Der nur Inhaltstyp, die, den diese API unterstützt, ist "Application/Json", der in der HTTP-Header, der bei jedem Aufruf erforderlich ist.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| xuid | 64-Bit-Ganzzahl ohne Vorzeichen | Die Xbox User ID (XUID) des Spielers, die die Anforderung stammt. |
| messageId | string[50] | Die ID der Nachricht, die abgerufen oder gelöscht. |

<a id="ID4ERB"></a>


## <a name="authorization"></a>Autorisierung

Sie benötigen Ihre eigenen Benutzer, die vorgeben, eine Meldung für den Benutzer abrufen.

<a id="ID4E3B"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource

Sie können nur Ihre eigenen Benutzernachrichten abrufen.

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Beschreibung|
| --- | --- | --- | --- | --- |
| 200| Erfolg.|
| 400| Die XUID kann nicht ordnungsgemäß konvertiert werden.|
| 403| Die XUID kann nicht konvertiert werden, oder ein gültigen XUID Anspruch kann nicht gefunden werden.|
| 404| Eine gültige XUID ist nicht vorhanden, oder die Nachrichten-ID wurde nicht gefunden oder wird nicht ordnungsgemäß analysiert.|
| 500| Allgemeine serverseitigen Fehler oder den Nachrichtentyp gilt nicht für GET.|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON)-Antwort

Wenn erfolgreich aufgerufen wird, gibt der Dienst die Ergebnisdaten in einem JSON-Format. Das Stammobjekt ist ein UserMessageHeader-Objekt.

#### <a name="usermessageheader"></a>UserMessageHeader

| Eigenschaft| Typ| Maximale Länge| Hinweise|
| --- | --- | --- | --- |
| header| Header|  | JSON-Objekt|
| messageText| string| 256| UTF-8|

#### <a name="header"></a>Header

| Eigenschaft| Typ| Maximale Länge| Hinweise|
| --- | --- | --- | --- |
| Gesendet| DateTime|  | Datum und Uhrzeit, die die Nachricht gesendet wurde. (Bereitgestellt vom Dienst.)|
| Ablaufdatum| DateTime|  | Datum und Uhrzeit, an die Nachricht abläuft. (Alle Nachrichten haben eine maximale Lebensdauer, in der Zukunft bestimmt werden soll.)|
| messageType| string| 13| Nachrichtentypen: User, System, FriendRequest.|
| senderXuid| ulong|  | XUID des Absenders.|
| Sender| string| 15| Gamertag des Absenders.|
| hasAudio| bool|  | Gibt an, ob die Nachricht eine Anlage Audio (Sprache) ist.|
| hasPhoto| bool|  | Gibt an, ob die Nachricht eine Foto-Anlage ist.|
| hasText| bool|  | Gibt an, ob die Nachricht Text enthält.|

#### <a name="sample-response"></a>Beispielantwort

```cpp
{
          "header":
          {
            "expiration":"2011-10-11T23:59:59.9999999",
            "messageType":"User",
            "senderXuid":"123456789",
            "sender":"Striker",
            "sent":"2011-05-08T17:30:00Z",
            "hasAudio":false,
            "hasPhoto":false,
            "hasText":true
          },
        "messageText":"random user text up to 256 characters"
        }

```

#### <a name="error-response"></a>Fehlerantwort

Im Fall eines Fehlers kann der Dienst ein ErrorResponse-Objekt zurück, die Werte aus der Umgebung des Diensts enthalten kann.

| Eigenschaft| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| string| Ein Hinweis darauf, wo der Fehler verursacht wurde.|
| errorCode| int| Numerischen Code für den Fehler (kann null sein).|
| errorMessage| string| Details des Fehlers, wenn Sie so konfiguriert, dass die Details anzuzeigen.|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Verweis [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md)
