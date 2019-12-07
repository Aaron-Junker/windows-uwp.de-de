---
title: Xbox Live-Test - Referenz zu den Benutzerverwaltungs-APIs
description: Erfahren Sie, wie Sie programmgesteuert auf die Benutzerverwaltungs-APIs zugreifen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: 52f333af73084ed14982b9d09b6770c8294980f7
ms.sourcegitcommit: 6169660ea437915265165c4631d9702587e4793d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74902528"
---
# <a name="xbox-live-user-management"></a>Xbox Live-Benutzerverwaltung

## <a name="request"></a>Anfordern

Sie können die Liste der Benutzer auf der Konsole abrufen oder aktualisieren – Benutzer hinzufügen, entfernen, anmelden, abmelden oder vorhandene Benutzer ändern.

| Methode        | Anforderungs-URI     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |


**URI-Parameter**

* Keine

**Anforderungsheader**

* Keine

**Anforderungstext**

PUT-Aufrufe müssen ein JSON-Array mit folgender Struktur enthalten:

* Benutzer
  * AutoSignIn (optional): Ein boolescher Wert, der die automatische Anmeldung für das durch EmailAddress oder UserId angegebene Konto deaktiviert oder aktiviert.
  * EmailAddress (optional – muss angegeben werden, wenn UserId nicht angegeben ist, es sei denn, ein gesponserter Benutzer wird angemeldet): Die E-Mail-Adresse, die den Benutzer angibt, der geändert/hinzugefügt/gelöscht werden soll.
  * Password (optional – muss angegeben werden, wenn der Benutzer derzeit nicht auf der Konsole enthalten ist): Das Kennwort, das zum Hinzufügen eines neuen Benutzers zur Konsole verwendet wird.
  * SignedIn (optional): Ein boolescher Wert, der angibt, ob das angegebene Konto an- oder abgemeldet werden soll.
  * UserId (optional – muss angegeben werden, wenn EmailAddress nicht angegeben ist, es sei denn, ein gesponserter Benutzer wird angemeldet): Die Benutzer-ID, die den Benutzer angibt, der geändert/hinzugefügt/gelöscht werden soll.
  * SponsoredUser (optional): Ein boolescher Wert, der angibt, ob ein gesponserter Benutzer hinzugefügt werden soll.
  * Delete (optional): bool, das angibt, dass dieser Benutzer aus der Konsole gelöscht werden soll

## <a name="response"></a>Response

**Antworttext**

GET-Aufrufe geben ein JSON-Array mit folgenden Eigenschaften zurück:

* Benutzer
  * AutoSignIn (optional)
  * EmailAddress (optional)
  * Gamertag
  * SignedIn
  * UserId
  * XboxUserId
  * SponsoredUser (optional)
  
**Status Code**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode   | Beschreibung     | 
| ------------------ |-----------------|
| 200                | Der GET-Aufruf war erfolgreich, und im Antworttext wurde ein JSON-Array mit Benutzern zurückgegeben. |
| 204                | Der PUT-Aufruf war erfolgreich, und die Benutzer auf der Konsole wurden aktualisiert. |
| 4XX                | Verschiedene Fehler für ungültige Anforderungsdaten oder ein ungültiges Anforderungsformat |
| 5XX                | Fehlercodes für unerwartete Fehler |
