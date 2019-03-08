---
title: APIs UI-Metadaten im Bereich
description: Enthält eine umfassende Liste der UI-Metadaten für Xbox Spielbereich-APIs.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Bereich, -Turniers, ux
ms.localizationpriority: medium
ms.openlocfilehash: 63151d2c584218090f98a3c5f8089bf24bdb9b22
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659765"
---
# <a name="arena-apis-a-comprehensive-list-of-ui-metadata"></a>Spielbereich-APIs: Eine umfassende Liste der UI-Metadaten

Die APIs der Spielbereich Zurückgeben der Metadaten-Turniers, Übereinstimmung und Player-Details in einem Spiel liefert. Hier ist eine vollständige Liste.

TURNIER-DETAILS  | MATCH-DETAILS | TEAM – DETAILS  | REGISTRIERUNGSDETAILS
--- | --- | --- | ---
Planer-name | Endzeit: Datum/Uhrzeit, die diese Turnier hat den Zustand "Abgebrochen" oder "Abgeschlossen". Dies wird automatisch festgelegt, wenn ein Turnier aktualisiert wird. | Vorherigen Übereinstimmung (Tournament Spiel Ergebnis gibt an, Rangfolgen Endzeit, Startzeit der Übereinstimmung ist eine Bye, Beschreibung, gegensätzlichen Team-IDs) | Der anbieterregistrierungsstatus des (unbekannt, Ausstehend "," zurückgezogen, abgelehnt, registriert, abgeschlossen)
Name des Turniers (max. 128 Zeichen) | Ist eine Bye   | Team-Zustand (unbekannt, registriert, auf der Warteliste, standby, eingecheckt, spielen, abgeschlossen) | Registrierung Grund (unbekannt oder geschlossenen, Member, die bereits registriert, vollständige-Turniers, Team entfernt, Turnier abgeschlossen)
Turnier-Beschreibung (maximal 1000 Zeichen) | Turnier Spiel Ergebnis Zustände (keine Wettbewerb/abgebrochen, Win, Verlust, zeichnen, Rang, nicht angezeigt) | Grund des Teams befindet sich in Status "abgeschlossen" (abgelehnt, entfernt, entfernt, abgeschlossen) | Minimale und maximale Anzahl von Teams registriert.
Registrierung Start-bzw. Endzeit | Status der Vermittlung: Abgeschlossen, keine "," abgebrochen, ohne Ergebnisse, die Teilergebnisse | Registrierungsdatum: Datum und Uhrzeit, die ein Team registriert wurde. |
Überprüfen Sie die im Start-bzw. Endzeit | Übereinstimmen, aussagekräftigen 'Bezeichnung': ("letzte, Heat-1") | Team ständigen | Ist eine Registrierung öffnen
Wiedergabe Start-bzw. Endzeit | Startzeit | Teamname | Ist, überprüfen Sie im geöffneten
Hat einen Preis | Gegnerischen Teams-Ids | Endgültige Rangliste Team | Registrierung Start-bzw. Endzeit
Min/Max-Teamgröße | | Fortsetzung URI, dauert die Mitglieder des Teams Spielbereich-Benutzeroberfläche zurück | Überprüfen Sie die im Start-bzw. Endzeit
Spielmodus | | Aktuellen Übereinstimmung Zeigt Metadaten (Beschreibung, Startzeit, Bye, gegensätzlichen Team-IDs) | Anzahl von Teams, die registriert
Turnier-Stil (einzelne Eliminierung von Duplikaten, Round-Robin) | | Team-Zusammenfassung (Team-Status, Rangfolge) |
Ist eine Registrierung öffnen | | Gamertags |
Ist, überprüfen Sie im geöffneten | | |
Anzahl der registrierten Teams | | |
Wiedergabe von ist geöffnet | | |
