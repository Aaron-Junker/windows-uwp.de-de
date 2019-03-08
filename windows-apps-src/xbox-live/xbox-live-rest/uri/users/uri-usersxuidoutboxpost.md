---
title: POST (/users/xuid({xuid})/outbox)
assetID: de991d88-efe0-04f2-f6b2-0bc3e68bfd46
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutboxpost.html
description: " POST (/users/xuid({xuid})/outbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 2b225e8441fee3d499f172e2e5701096cdbc161a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594285"
---
# <a name="post-usersxuidxuidoutbox"></a>POST (/users/xuid({xuid})/outbox)
Sendet eine angegebene Meldung an eine Liste von Empfängern.
Die Domäne für diese URIs ist `msg.xboxlive.com`.

  * ["Hinweise"](#ID4EV)
  * [URI-Parameter](#ID4EAB)
  * [Autorisierung](#ID4ENB)
  * [Auswirkungen der datenschutzeinstellungen für Ressource](#ID4EYB)
  * [Anforderungstext](#ID4E3F)
  * [HTTP-Statuscodes](#ID4ETCAC)
  * [Antworttext](#ID4E1EAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Hinweise

Der nur Inhaltstyp, die, den diese API unterstützt, ist "Application/Json", der in der HTTP-Header, der bei jedem Aufruf erforderlich ist.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| xuid | 64-Bit-Ganzzahl ohne Vorzeichen | Die Xbox User ID (XUID) des Spielers, die die Anforderung stammt. |

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorisierung

Sie müssen Ihre eigenen Benutzeranspruch und ein gültiges gold-Abonnement, zum Senden einer Benutzernachricht verfügen.

<a id="ID4EYB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Auswirkungen der datenschutzeinstellungen für Ressource

Senden eine Meldung für den Benutzer wurde erfolgreich an einen Player, ob der Spieler ein Friend oder nicht ist, führt einen Ergebniscode 200. Wenn Sie eine Nachricht an jemanden, die Sie blockiert hat senden, jedoch der Empfänger nicht die Meldung erhalten, und erhalten Sie keine Hinweis darauf, dass die Nachricht nicht erfolgreich war.

Es gibt auch Grenzwerte auf wie viele Nachrichten pro Tag und wie viele Freunden und nicht-Freunde, wie folgt gesendet werden können.

   * 20 fremden pro Nachricht
   * 200 fremden pro 24 Stunden
   * Gesamtzahl der 250 Nachrichten pro 24 Stunden
   * 2500 Empfänger pro 24 Stunden

| Anfordernden Benutzer| Ziel der Datenschutzeinstellungen des Benutzers| Verhalten|
| --- | --- | --- | --- | --- | --- |
| Ich| -| Wie beschrieben.|
| Friend| Alle Benutzer| 200 OK|
| Friend| Nur Freunde| 200 OK|
| Friend| gesperrt| 200 OK|
| nicht-Friend-Benutzer| Alle Benutzer| 200 OK|
| nicht-Friend-Benutzer| Nur Freunde| 200 OK|
| nicht-Friend-Benutzer| gesperrt| 200 OK|
| Drittanbieter-Website| Alle Benutzer| 200 OK|
| Drittanbieter-Website| Nur Freunde| 200 OK|
| Drittanbieter-Website| gesperrt| 200 OK|

<a id="ID4E3F"></a>


## <a name="request-body"></a>Anforderungstext

| Eigenschaft| Typ| Maximale Länge| Consumer| Hinweise|
| --- | --- | --- | --- | --- |
| header| Header|  | Alle| Benutzer-Nachrichtenheader|
| messageText| string| 250| Alle Plattformen außer Windows 8| Meldungstext "Benutzer" (UTF-8)|

#### <a name="header"></a>Header

| Eigenschaft| Typ| Maximale Länge| Consumer| Hinweise|
| --- | --- | --- | --- | --- |
| Empfänger| Benutzer]| 20| Alle| Liste von Empfängern der Nachricht|

#### <a name="user"></a>Benutzer

| Eigenschaft| Typ| Maximale Länge| Consumer| Hinweise|
| --- | --- | --- | --- | --- |
| xuid| ulong|  | Alle| XUID des Empfängers. Nicht verwendet, wenn Gamertag gesendet wird.|
| Gamertag| string| 15| Alle| Gamertag des Empfängers. Nicht verwendet, wenn XUID gesendet wird.|

#### <a name="sample-request-body"></a>Inhalt der beispielanforderung 

```cpp
{
          "header":
          {
            "recipients":
            [{"gamertag":"GoTeamEmily"},
            {"gamertag":"Longstreet360"}]
          },
          "messageText":"random user text"
        }

```


<a id="ID4ETCAC"></a>


## <a name="http-status-codes"></a>HTTP-Statuscodes

Der Dienst gibt einen-Statuscodes in diesem Abschnitt als Reaktion auf eine Anforderung, die mit dieser Methode für diese Ressource zurück. Eine vollständige Liste der standard-HTTP-Statuscodes, die mit Xbox Live Services verwendet, finden Sie unter [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md).

| Code| Beschreibung|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Erfolg.|
| 400| Liste der Empfänger ist leer oder überschreitet die maximale Länge; oder sowohl für Gamertag XUID angegeben wurden; oder MessageText ist zu lang.|
| 403| XUID kann nicht konvertiert werden.|
| 404| Gamertag ist ungültig, oder Benutzer wurde nicht gefunden.|
| 407| Benutzer hat das tägliche Begrenzung, die vom System erreicht.|
| 500| Allgemeiner serverseitigen Fehler.|

<a id="ID4E1EAC"></a>


## <a name="response-body"></a>Antworttext

Es werden keine Objekte im Text der Antwort gesendet.

<a id="ID4EJFAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ELFAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/outbox](uri-usersxuidoutbox.md)


<a id="ID4EZFAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Verweis [Standard-HTTP-Statuscodes](../../additional/httpstatuscodes.md)
