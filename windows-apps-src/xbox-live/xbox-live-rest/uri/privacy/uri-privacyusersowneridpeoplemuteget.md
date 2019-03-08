---
title: GET (/users/{ownerId}/people/mute)
assetID: 49b6c830-95f7-3200-0e46-0a1af573971c
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemuteget.html
description: " GET (/users/{ownerId}/people/mute)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 94e2bf4d04619ffa3348ae08fc37964cdc58e7b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661585"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
Ruft die stumm schalten Liste für einen Benutzer ab.

  * ["Hinweise"](#ID4EQ)
  * [URI-Parameter](#ID4EZ)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4EEB)
  * [Autorisierung](#ID4ENB)
  * [Erforderlichen Anforderungsheader](#ID4ESC)
  * [Anforderungstext](#ID4EPE)
  * [HTTP-Statuscodes](#ID4E1E)
  * [Erforderlichen Antwortheader](#ID4E3G)
  * [Antworttext](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Hinweise

Wenn ein Ziel angegeben ist, gibt dieser URI nur für diesen Benutzer zurück, wenn der Benutzer in der Liste stumm schalten oder leer ist, wenn der Benutzer nicht.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| ownerId| string| Erforderlich. Der Bezeichner des Benutzers, dessen Ressource zugegriffen wird. Die möglichen Werte sind "me", <code>xuid({xuid})</code>, oder gt({gamertag}). Der authentifizierte Benutzer muss sein werden. Beispielwerte: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>. Maximale Größe: keine. |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource

Keine

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorisierung

Autorisierungsansprüche verwendet | Anspruch| Typ| Erforderlich?| Beispielwert|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64-Bit-Ganzzahl mit Vorzeichen| Ja| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>Erforderlichen Anforderungsheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorisierung | string| Der Authentifizierungsanmeldeinformationen für den HTTP-Authentifizierung. Beispielwert: <code>Xauth=&lt;authtoken></code>. Maximale Größe: keine.|
| X-RequestedServiceVersion| string| Erstellen Sie Namen und die Nummer des Xbox LIVE-Diensts, der diese Anforderung gesendet werden soll. Die Anforderung wird nur an weitergeleitet werden, die nach dem Überprüfen der Gültigkeit der Header, die Ansprüche in das Autorisierungstoken, service und so weiter. Beispielwerte: <code>1</code>, <code>vnext</code>. Maximale Größe: keine.|
| Annehmen| string| Inhaltstypen, die zulässig sind. Beispielwert: <code>application/json</code>. Maximale Größe: keine.|

<a id="ID4EPE"></a>


## <a name="request-body"></a>Anforderungstext

Keine Objekte werden im Text dieser Anforderung gesendet.

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Ursachentext| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Erfolgreiche Anforderung stumm schalten Liste.|
| 400| Ungültige Anforderung.| Die Ziel-ID im URI angegebene ist ungültig.|
| 403| Verboten| Der im URI angegebene Besitzer ist nicht der authentifizierte Benutzer.|
| 404| Nicht gefunden| Der im URI angegebene Besitzer ist nicht vorhanden.|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>Erforderlichen Antwortheader

| Header| Typ| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| Der MIME-Typ der Hauptteil der Anforderung. Beispielwert: <code>application/json</code>|
| Content-Length| string| Die Anzahl der Bytes, die in der Antwort gesendet werden. Beispielwert: 34|
| Cache-Control| string| Netzwerkschnittstellenbibliothek Anforderung vom Server zum Verhalten beim Zwischenspeichern angeben. Beispiel: <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>Antworttext

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>Beispielantwort

Finden Sie unter [UserList](../../json/json-userlist.md).


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EJBAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ELBAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people/mute](uri-privacyusersowneridpeoplemute.md)
