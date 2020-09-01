---
title: Xbox Live-Test - Referenz zu den Benutzerverwaltungs-APIs
description: Erfahren Sie, wie Sie die Liste der Benutzer in der Konsole mithilfe der Xbox-Geräte Portal-Rest-API erhalten oder aktualisieren.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: 0f05bc84469585fc10bfff6a7f0d0f0976a0080d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174674"
---
# <a name="xbox-live-user-management"></a>Xbox Live-Benutzerverwaltung

## <a name="request"></a>Anforderung

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

## <a name="response"></a>Antwort

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
  
**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode   | BESCHREIBUNG     | 
| ------------------ |-----------------|
| 200                | Der GET-Aufruf war erfolgreich, und im Antworttext wurde ein JSON-Array mit Benutzern zurückgegeben. |
| 204                | Der PUT-Aufruf war erfolgreich, und die Benutzer auf der Konsole wurden aktualisiert. |
| 4XX                | Verschiedene Fehler für ungültige Anforderungsdaten oder ein ungültiges Anforderungsformat |
| 5XX                | Fehlercodes für unerwartete Fehler |
