---
title: Ermittlung von Xbox-Turniere
description: Informationen Sie zum Erstellen der Benutzeroberfläche für die Einnahmen durch Spiele-Turniers Ermittlung.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Bereich, -Turniers, ux
ms.localizationpriority: medium
ms.openlocfilehash: 71a065d6d4da290f2ce3345c7da0026d4c116335
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632475"
---
# <a name="discovering-xbox-tournaments"></a>Ermittlung von Xbox-Turniere

Spieler haben mehrere Optionen in der Xbox-Dashboard oder der Xbox App auf einem PC, Weitere Informationen zum teilnehmen oder ein Turnier ansehen. Es gibt auch die Gelegenheit, Turnier Ermittlung aus, in der Titelleiste Ihres zu steigern.  

Ein Spieler finden Turniere von Arena-fähigen:

* Öffnen ein vor kurzem Gespielte Spiel Turniere Hub von zu Hause für Xbox aus.
* Besuchen ein Profil der Player für Turniere gespielt, jetzt wiedergeben oder für registriert.
* Reagieren auf eine Suche nach Gruppe (LFG) zu veröffentlichen, in einem Club Team gehören.
* Überwachen einen Turnier-Stream, in der Mixer-app, Website oder das Pivot sehen Sie sich in der Game-Hub/Bereich.
* Besuchen einen spielen-Hub im Xbox-Dashboard.

## <a name="promoting-tournaments-in-game"></a>Höherstufen von Turniere im Spiel

Personen ansehen Turniere, da sie den Titel interessiert sind. Xbox-Spiele, haben eine einzigartige Möglichkeit zum Abfangen von Spielern zu diesem Zeitpunkt entscheidend, wenn sie neue Möglichkeiten zum Erweitern ihrer Gaming-Erfahrung suchen.

> [!TIP]
> **UX-Empfehlung:** Höher gestuft werden Turniere zu entscheidungsfindung Zeitpunkten.
>
> Bewusstsein für Turniere manchmal ausgelöst, wenn Spieler müssen entscheiden, ob Sie beibehalten, spielen, oder beenden (z. B. am Starten des Spiels, im Hauptmenü, Multiplayer-Menü oder Ende der Übereinstimmung).

### <a name="1--use-a-dynamic-announcement-feature-example-message-of-the-day"></a>1.  Eine dynamische Ankündigung-Funktion verwenden (Beispiel: Die Nachricht des Tages)

Titel, die eine Ankündigung-Funktion in ihre Spiele Benutzeroberfläche, in der Regel erstellt haben verwenden Sie ein Content Management System für sofortige Updates. Dies ist nützlich für die neuen Inhalte, die den Spielern übertragen, ohne Rückgriff auf eine Aktualisierung von Inhalt oder die herunterladbare inhaltsfreigabe. Halo verwendet beispielsweise eine Funktion "Nachrichten des Tages" Surface-Community-Ankündigungen und Tipps. Turnier heraufstufung ist perfekt für dieses Szenario.

**Benutzerauswirkungen**

* Spieler werden zu einer neuen Verkaufschance von wettbewerbsfähig Spiele eingeführt, vor dem Initiieren einer spielen-Sitzungs.
* Offenlegung wird zeitweilig (auf "Start", "in Kombination mit anderen Ankündigungen").
* Weitere Informationen werden es Spielern ermöglichen, lassen das Spiel, und fahren mit der Spielbereich Hub geleitet.
* Die Funktion erfordert keine Aktualisierung von Inhalte zum Auffüllen.
* Relevanz und zeitliche Steuerung der heraufstufung bleibt flexibel.

###### <a name="ui-example-message-of-the-day"></a>Beispiel der Benutzeroberfläche: "Meldung des Tages"

![Turnier Nachrichten des Tages](../../images/arena/arena-ux-motd.png)

#### <a name="a-main-heading-ex-play-watch-learn"></a>A. Main, z. B. Überschrift: Spielen, ansehen, erfahren Sie mehr  

Spielern informiert über alle Turniere oder ein bestimmtes.

#### <a name="b-go-to-arena-hub"></a>B. Wechseln Sie zum Bereich-Hub  

Deep-Link zum Spiel Hub/Turniere - empfohlen, wenn alle Turniere, die im Zusammenhang mit der Ihr Spiel höher gestuft.  
–Oder–  
Deep-Link zum Spiel Hub/Turnier/Details - empfohlen, wenn ein bestimmtes Turnier Ankündigung.

#### <a name="c-game-exit-confirmation"></a>C. Spiel beenden Bestätigung

Bildschirmelemente A und B führen den Benutzer, das Spiel an das Dashboard für die Xbox. Legen Sie diese Annahme des Titels Benutzeroberfläche vor dem Spieler macht eine Entscheidung.

###### <a name="ui-example-exit-game-confirmation"></a>Beispiel der Benutzeroberfläche: Spiel beenden-Bestätigung
![Spiele Turnier Exit-Bestätigung](../../images/arena/arena-ux-exit-confirm.png)

### <a name="2--promote-tournaments-on-the-main-menu"></a>2.  Heraufstufen Sie Turniere im Hauptmenü

In diesem Beispiel wird die Ankündigung eine interaktive Anzeige für ein zukünftiges Ereignis. Sie erhalten die genügend Informationen, um den Spieler Weitere verleiten. Auf **wählen**, der Spieler wird in eine Detailseite Turniere, auf dem Hub Spielbereich geleitet, in dem sie ihre Registrierung verwalten können.

###### <a name="ui-example-a-tournament-ad-displayed-alongside-the-main-menu"></a>Beispiel der Benutzeroberfläche: Ein Turnier Ad zusammen mit der Sie im Hauptmenü angezeigt

![](../../images/arena/arena-ux-promo.png)

> [!TIP]
> **UX-Empfehlung**  
> Die Ankündigung sollte Details gerade ausreichen, um zu entscheiden, ob sie mehr erfahren möchten es Spielern ermöglichen, enthalten. Z. B. ***Beschreibung***, ***Beliebtheit***, und ***Timing***.

## <a name="browsing-tournaments-in-game"></a>Durchsuchen von Turniere im Spiel

Der Spielbereich-APIs bieten genügend Daten für den Titel aus, um eine Browsingumgebung in das Spiel zu erstellen. Wenn Sie beabsichtigen, eine Funktion zum Durchsuchen zu erstellen, fügen Sie eine **Turniere** Einstiegspunkt im Multiplayer-Abschnitt.

> [!TIP]
> **UX-Empfehlungen**  
> Turniere als einen Multiplayer-Spiele-Modus vorhanden.  
> Da Xbox-Bereich ein neues Feature ist, achten Sie darauf, dass Sie auf der Ebene 1 und Ebene 2 Ebene sichtbar bleiben.

###### <a name="ui-example-tournaments-listed-as-an-additional-mode-in-the-multiplayer-section"></a>Beispiel der Benutzeroberfläche: Turniere, die als eines zusätzlichen Modus im Abschnitt Multiplayer-

![Turnier-Modus](../../images/arena/arena-ux-tournament-mode.png)

### <a name="provide-a-comprehensive-list-of-tournaments"></a>Geben Sie eine umfassende Liste der Turniere

Ermöglichen Sie es Spielern ermöglichen, um den Status des Turniere schnell zu überprüfen, die sie derzeit angemeldet sind und Turniere zwischen Gaming-Sitzungen zu durchsuchen.

#### <a name="user-impact"></a>Benutzerauswirkungen

* Minimiert das Spieler erforderlich, lassen Sie das Spiel um Turnier Status im Dashboard zu überprüfen.
* Dient als eine hervorragende Lösung für die Sicherung ein, wenn Spieler Xbox Spielbereich Popupbenachrichtigungen verpasst haben.
* Zusätzliche Kosten für Spiele-Entwickler zum Verwalten von umfassen.
* Führt Spieler, zu der Spielbereich-Benutzeroberfläche für das beitreten, Registrierung, Einchecken, das seeding und das Team Informationen.

> [!NOTE]  
> Der Spielbereich-APIs bieten keine Abfrage für vom Benutzer generierte Turniere (Club generiert).


> [!TIP]
> **UX-Empfehlung**  
> Erstellen Sie eine Benutzeroberfläche, die zu skalieren und bieten Ihnen Methoden für die es Spielern ermöglichen, die eine umfangreiche Liste von Turniere zu verwalten.

### <a name="filters"></a>Filter

Turniere durch Filtern **alle**, **Active** (die alles abdeckt, bis der einzelnen turnierteilnehmer endet), **Canceled**, und **abgeschlossen** Zustände und Turniere ein Spieler hat (z. B. meine Turniere) teilnehmen sollen.

###### <a name="ui-example"></a>Beispiel der Benutzeroberfläche:

![Turnier Anzeige "Filter"](../../images/arena/arena-ux-filters.png)

> [!NOTE]  
> Hinzufügen von benutzerdefinierten Filtern, außerhalb der Art der Unterstützung durch der API ist möglich, *jedoch nicht empfohlen*.

Titel des kann nach anderen Eigenschaften filtern, nachdem die Anforderung, Sie aber es kann nicht auf die Abfrageparameter angeben. Dadurch wird das Filtern nach anderen Eigenschaften-unzuverlässigen für diese Art von Benutzeroberfläche. Der Spielbereich-APIs eine Titel angegebene Anzahl von Ergebnissen abzurufen, zu einem Zeitpunkt – z. B. der Titel geben möglicherweise eine **"MaxItems"** maximal 10 Turniere. Dadurch können den Titel, die Verwaltung der Leistung und reduziert das Risiko einer Überlastung des Benutzers mit einer großen Liste. Alle benutzerdefinierten Filter, die bereitgestellt werden, indem Sie Ihr Spiel gelten würde jedoch nur für die 10 Elemente, die zu diesem Zeitpunkt angefordert. Dies kann nach jeder API-Aufruf in eine inkonsistente Anzahl von gefilterten Ergebnissen führen. Der erste Satz von 10 Turniere zurückgegeben kann z. B. 2 enthalten, die im Zusammenhang mit einen benutzerdefinierten Filter, z. B. "Fair." Aber die nächsten 10 9 solche Elemente anzeigen kann und so weiter. Die einzig zuverlässige Möglichkeit, Ihre Titel jedes benutzerdefinierten Filter vollständig aufgefüllt ist, fordern alle aktiven Turnier und Filtern sie alle, die es wird empfohlen, nicht.

#### <a name="filter-recommendations"></a>Filtern von Empfehlungen:

##### <a name="all"></a>ALL

Zeigen Sie alle **Active**, **Canceled**, und **abgeschlossen** Turniere, sortiert nach den meisten aktuellen.

Ergebnisse:

* Turnier-Status
* Startdatum des Turniers
* Status des Turniers – z. B. Registrierung öffnen
* Deep-Link zu der Detailseite des Turniers im dashboard

##### <a name="my-tournaments"></a>MEINE TURNIERE

Turniere anzeigen, denen ein Teilnehmer im derzeit registriert ist.

Ergebnisse:

* Turniere, denen der derzeit angemeldeten Benutzer beteiligt ist
* Turnier-status
* Eine Methode, um eine Übereinstimmung Turnier (für "Match bereit" Status "") eingeben
* Deep-Link zu einer Seite-Turniers im Dashboard

##### <a name="active"></a>AKTIV

Anzeigen aller Turniere, die erstellt wurden: bevorstehender, für die Registrierung, Einchecken und Wiedergabe geöffnet.

Ergebnisse:

* Turniere, die geöffnet sind, um Sie zu, dass der aktuelle Benutzer nicht bereits hinzugefügt wurde, registrieren
* Zeitspanne bis zum Schließen der Registrierung

##### <a name="completedcanceled"></a>ABSCHLUSS/ABGEBROCHEN

Letzte Ergebnisse anzeigen. Turniere, die gerade zu Ende können im Laufe der Zeit, anstatt einfach ausgeblendet werden, nachdem sie abgeschlossen sind veraltet.

Ergebnisse:

* Deep-Link zu einer Seite-Turniers im Dashboard
* Turnier-Status
* Enddatum/-Uhrzeit

##### <a name="canceled"></a>ABGEBROCHEN

Turniere anzeigen, die abgebrochen wurden.

Ergebnisse:

* Deep-Link zu einer Seite-Turniers im Dashboard
* Datum/Uhrzeit des Abbruchs

> [!div class="nextstepaction"]
> [Verknüpfen Sie ein Turnier mithilfe der Spielbereich-Benutzeroberfläche](arena-ux-join-tournament.md)
