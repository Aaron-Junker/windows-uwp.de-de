---
title: Xbox Live-Test - Referenz zu den Benutzerverwaltungs-APIs
description: Erfahren Sie, wie Sie programmgesteuert auf die Benutzerverwaltungs-APIs zugreifen.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: c934a88dd1825fb0111083d71eb25e477956d79c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627365"
---
#<a name="xbox-live-user-management"></a>Xbox Live-Benutzer-Management-

**Anforderung**

Sie können die Liste der Benutzer auf der Konsole abrufen oder aktualisieren – Benutzer hinzufügen, entfernen, anmelden, abmelden oder vorhandene Benutzer ändern.

| Methode        | Anforderungs-URI     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |
<br>

**URI-Parameter**

* Keine

**Anforderungsheader**

* Keine

**Anforderungstext**

PUT-Aufrufe müssen ein JSON-Array mit folgender Struktur enthalten:

* Benutzer
  * AutoSignIn (optional): Ein boolescher Wert, der die automatische Anmeldung für das durch EmailAddress oder UserId angegebene Konto deaktiviert oder aktiviert.
  * E-Mail-Adresse (optional – muss angegeben werden, wenn "UserID" nicht angegeben wird, es sei denn, Anmelden bei einem Sponsor Benutzer): E-Mail-Adresse, die den Benutzer ändern/hinzufügen/löschen angeben.
  * Kennwort (optional – muss angegeben werden, wenn der Benutzer derzeit in der Konsole nicht): Das Kennwort für das Hinzufügen eines neuen Benutzers in der Konsole verwendet.
  * SignedIn (optional): Ein boolescher Wert, der angibt, ob das angegebene Konto an- oder abgemeldet werden soll.
  * Benutzer-ID (optional – muss angegeben werden, wenn e-Mail-Adresse nicht angegeben wird, es sei denn, Anmelden bei einem Sponsor Benutzer): "UserID" Angeben des Benutzers ändern/hinzufügen/löschen.
  * SponsoredUser (optional): Ein boolescher Wert, der angibt, ob ein gesponserter Benutzer hinzugefügt werden soll.
  * (Optional) löschen: "bool", die angeben, dass dieser Benutzer aus der Konsole löschen

###<a name="response"></a>Antwort ###

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
  
**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode   | Beschreibung     | 
| ------------------ |-----------------|
| 200                | Der GET-Aufruf war erfolgreich, und im Antworttext wurde ein JSON-Array mit Benutzern zurückgegeben. |
| 204                | Der PUT-Aufruf war erfolgreich, und die Benutzer auf der Konsole wurden aktualisiert. |
| 4XX                | Verschiedene Fehler für ungültige Anforderungsdaten oder ein ungültiges Anforderungsformat |
| 5XX                | Fehlercodes für unerwartete Fehler |
<br>


