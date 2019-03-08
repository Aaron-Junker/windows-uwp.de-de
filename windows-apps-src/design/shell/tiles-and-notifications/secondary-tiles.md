---
Description: Mit sekundären Kacheln können Benutzer für den einfachen zukünftigen Zugriff auf den Inhalt Ihrer App bestimmte Inhalte und Deep-Links von der App an das Startmenü heften.
title: Sekundäre Kacheln
label: Secondary tiles
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP, sekundäre Kacheln
ms.localizationpriority: medium
ms.openlocfilehash: 0f95699fd6eb0803824c9bcdea46d275ab4383ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653185"
---
# <a name="secondary-tiles"></a>Sekundäre Kacheln


Mit sekundären Kacheln können Benutzer für den einfachen zukünftigen Zugriff auf den Inhalt Ihrer App bestimmte Inhalte und Deep-Links von der App an das Startmenü heften.

![Screenshot von sekundären Kacheln](images/secondarytiles.png)

Benutzer können z. B. das Wetter für verschiedene Orte an das Startmenü anheften, um (1) einfache, auf einen Blick erkennbare Live-Informationen über das aktuelle Wetter dank Live-Kachel und (2) einen schnellen Einstiegspunkt für die bestimmte Stadt Wetter, u. a. zu erhalten. Benutzer können auch bestimmte Aktien, Nachrichtenartikel und weitere Elemente anheften, die Ihnen wichtig sind.

Durch das Hinzufügen von sekundären Kacheln zur App, kann der Benutzer schnell und effizient erneut Kontakt mit Ihrer App aufnehmen, und ihm den einfachen Zugriff ermöglichen, den sekundäre Kacheln bereitstellen.

**Sekundäre Kacheln können nur vom Benutzer auf der Startseite angeheftet werden. Von Apps können sekundäre Kacheln nicht programmgesteuert ohne Zustimmung des Anwenders angeheftet werden.** Der Benutzer muss explizit auf eine Schaltfläche "Anheften" in der App klicken, worauf Sie die API verwenden, um die Anforderung an eine sekundäre Kachel zu erstellen. Das System zeigt anschließend ein Dialogfeld an, das den Benutzer auffordert, zu bestätigen, ob die die Kachel angeheftet werden soll.

## <a name="quick-links"></a>Direktlink

| Artikel | Beschreibung |
| --- | --- |
| [Anleitungen, sekundäre Kacheln](secondary-tiles-guidance.md) | Hier erfahren Sie, wann und wo sekundäre Kacheln verwendet werden sollen. |
| [Sekundäre Kacheln anheften](secondary-tiles-pinning.md) | Hier erfahren Sie, wie Sie eine sekundäre Kachel anheften. |
| [Anheften von desktop-Anwendung](secondary-tiles-desktop-pinning.md) | Windows-Desktopanwendungen können sekundäre Kacheln dank der Desktop-Brücke anheften! |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>Verhältnis zwischen sekundären Kacheln und primären Kacheln

Sekundäre Kacheln sind einer einzigen übergeordneten App zugeordnet. Sie sind auf dem Startmenü angeheftet. Der Benutzer kann dadurch auf einheitliche und effiziente Weise direkt auf einen häufig verwendeten Bereich der übergeordneten App zugreifen. Das kann ein allgemeiner Unterbereich der übergeordneten App mit häufig aktualisierten Inhalten sein oder ein Deep-Link zu einem bestimmten Bereich der App.

Beispielszenarien für sekundäre Kacheln:

* Aktuelle Wetterinformationen in einer Wetter-App
* Eine Zusammenfassung anstehender Termine in einer Kalender-App
* Status und Updates eines wichtigen Kontakts in einer sozialen App
* Bestimmte Feeds in einem RSS-Reader
* Eine Musikwiedergabeliste
* Ein Blog

Sich häufig ändernde Inhalte, die ein Benutzer im Blick behalten möchte, können hervorragend als sekundäre Kachel dargestellt werden. Nachdem die sekundäre Kachel angeheftet wurde, sieht der Benutzer daran sofort jede Änderung und kann damit direkt die übergeordnete App starten.

Sekundäre Kacheln sind in vielen Punkten mit primären Kacheln vergleichbar:

* Sie verwenden Kachelbenachrichtigungen, um umfangreichen Inhalte anzuzeigen.
* Für den Standardinhalt der Kachel muss ein 150 x 150 Pixel großes Logo vorhanden sein.
* Sie können optional die anderen Logo-Größen enthalten, um größere Kachel zu aktivieren.
* Sie können Benachrichtigungen und Signale anzeigen.
* Sie können auf dem Startmenü neu angeordnet werden.
* Sie werden beim Deinstallieren der App automatisch gelöscht.
* Ausführliche Statusinfos für Signale und die Sperre können auf dem Sperrbildschirm angezeigt werden.

In einigen entscheidenden Punkten unterscheiden sich sekundäre Kacheln jedoch von primären Kacheln:

* Benutzer können ihre sekundären Kacheln jederzeit löschen, ohne dabei die übergeordnete App zu löschen.
* Sekundäre Kacheln können zur Laufzeit erstellt werden. App-Kacheln können nur bei der Installation erstellt werden.
* Vom Benutzer wird in einem Flyout eine Bestätigung angefordert, bevor eine sekundäre Kachel hinzugefügt wird.
* Sie können für den Sperrbildschirm nicht programmgesteuert über eine Anfrage an den Benutzer ausgewählt werden. Der Benutzer muss die sekundäre Kachel über die Seite "anpassen" manuell in die PC-Einstellungen hinzufügen.

Für Kachel- und Signalupdater und Pushbenachrichtigungskanäle für sekundäre Kacheln gibt es besondere Methoden, um Benachrichtigungen zu senden. Sie entsprechen den Versionen, die mit primären Kacheln verwendet werden. Z. B. CreateBadgeUpdaterForApplication im Vergleich zu CreateBadgeUpdaterForSecondaryTile.


## <a name="guidance-on-secondary-tiles"></a>Anleitung für sekundäre Kacheln
Informationen darüber, wann und wo Sie sekundäre Kacheln verwenden sollten und weitere Hinweise zur Verwendung finden Sie unter [Anleitung für sekundäre Kacheln](secondary-tiles-guidance.md)


## <a name="pinning-secondary-tiles"></a>Anheften von Sekundärkacheln
Weitere Informationen zum Anheften von sekundärer Kacheln finden Sie unter [Sekundäre Kacheln anheften](secondary-tiles-pinning.md).


## <a name="desktop-applications-and-secondary-tiles"></a>Desktop-Apps und sekundäre Kacheln
Weitere Informationen darüber, wie Sie zu sekundären Kacheln von Ihrer Desktopanwendung über die Desktop-Brücke verwenden können finden Sie unter [Sekundäre Kacheln von Desktop-Anwendung anheften](secondary-tiles-desktop-pinning.md).
