---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
description: " GET (/users/{ownerId}/people/avoid)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 745893b4b975b5fbf64fe76591ec15d18af59d73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641615"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
Ruft die für einen Benutzer ab.

  * ["Hinweise"](#ID4EQ)
  * [URI-Parameter](#ID4EZ)
  * [Autorisierung](#ID4EEB)
  * [Erforderlichen Anforderungsheader](#ID4EJC)
  * [HTTP-Statuscodes](#ID4EYD)
  * [Erforderlichen Antwortheader](#ID4E1F)
  * [Antworttext](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Hinweise

Wenn ein Ziel angegeben ist, gibt dieser Benutzer nur, wenn sie auf die Blockliste oder leer sind, wenn dies nicht der Fall.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| ownerId| string| Erforderlich. Der Bezeichner des Benutzers, dessen Ressource zugegriffen wird. Die möglichen Werte sind <code>xuid({xuid})</code>. Der authentifizierte Benutzer muss sein werden. Beispielwert: <code>xuid(2603643534573581)</code>. Maximale Größe: keine. |

<a id="ID4EEB"></a>


## <a name="authorization"></a>Autorisierung

Autorisierungsansprüche verwendet | Anspruch| Typ| Erforderlich?| Beispielwert|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64-Bit-Ganzzahl mit Vorzeichen| Ja| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung | string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: <code>Xauth=&lt;authtoken></code>. Maximale Größe: keine.|
| Annehmen| string| Inhaltstypen, die zulässig sind. Beispielwert: <code>application/json</code>. Maximale Größe: keine.|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Die Sitzung wurde erfolgreich abgerufen.|
| 400| Ungültige Anforderung.| Die Ziel-ID im URI angegebene ist ungültig.|
| 403| Verboten| Der im URI angegebene Besitzer ist nicht der authentifizierte Benutzer.|
| 404| Nicht gefunden| Der im URI angegebene Besitzer ist nicht vorhanden.|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| Der MIME-Typ der Hauptteil der Anforderung. Beispielwert: <code>application/json</code>. Maximale Größe: keine.|
| Content-Length| string| Die Anzahl der Bytes, die in der Antwort gesendet werden. Beispielwert: 34. Maximale Größe: keine.|
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung vom Server zum Verhalten beim Zwischenspeichern angeben. Beispielwert: <code>application/json</code>. Maximale Größe: keine.|

<a id="ID4ESH"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4EYH"></a>


### <a name="sample-response"></a>Beispielantwort


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EDAAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EFAAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people/avoid](uri-privacyusersxuidpeopleavoid.md)
