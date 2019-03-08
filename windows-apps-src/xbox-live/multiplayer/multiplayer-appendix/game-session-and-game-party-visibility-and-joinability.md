---
title: Spiele-Sitzung Sichtbarkeit und joinability
description: Beschreibt Spiel Xbox Live-Sitzung und Spiele Partei Sichtbarkeit und Joinability.
ms.assetid: 39b6dac1-0c6b-4dc1-9fe0-3cb7c471fbab
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 65da0d60080cd790b84ce46f8598c2f6f638ff05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619715"
---
# <a name="game-session-and-game-party-visibility-and-joinability"></a>Spiele-Sitzung als auch spieleskripts Partei Sichtbarkeit und joinability

Für Xbox One die *Sichtbarkeit* und *Joinability* Einstellungen für Spiele Sitzungen und Spiele Parteien, den Zugriff auf multiplayer-Ergebnisse, steuern. Um ein optimales Benutzererlebnis in Spielen Sitzungen und Parteien zum Verknüpfen der Sitzung, und für einladende Spieler bereitzustellen, müssen Titel-Entwickler, um diese Einstellungen verstehen. In diesem Whitepaper werden die Unterschiede zwischen Sichtbarkeit und Joinability, und es wird erläutert, die spezifischen Einstellungen, die es wird empfohlen, dass der Titel verwenden, um den Kunden den besten Multiplayer-benutzerflow ermöglichen.

## <a name="game-session-visibility"></a>Sichtbarkeit der Game-Sitzung


Multiplayer-Sitzung-Verzeichnis (MPSD) Zugriff auf eine Sitzung wird durch zwei Einstellungen gated: Sitzung *Sichtbarkeit* und der angegebenen Sitzung *Joinability*. Diese Einstellungen können verwendet werden, um Zugriff auf eine Sitzung auf der Ebene MPSD zu beschränken.

Der erste Aspekt von Access Control ist die Sichtbarkeit der Sitzung. Sichtbarkeit der Sitzung ist eine Konstante, die zum Zeitpunkt der sitzungserstellung festgelegt ist. Es ist in der Regel in der sitzungsvorlage definiert, und bestimmt, welche Arten von Benutzern gelesen haben und Schreibzugriff auf eine Sitzung. Die Werte für die sichtbarkeitseinstellung sind:

-   *Öffnen Sie –* aller Benutzer lesen und Schreiben in die Sitzung können.

-   *Visible:* aller Benutzer lesen, aber nur verknüpft und reserviert können Spieler schreiben, mit der Sitzung.

-   *Privat –* nur verknüpft und reserviert Spieler Lese- oder Schreibzugriff für die Sitzung.

> **Hinweis:** Einstellen der Sichtbarkeit in privaten Wiederholungen erfordern kann, auf die **join()** aufrufen, indem Sie den eingeladenen Player. Wenn über die UI-Plattform eine INVITE-Nachricht gesendet wird, kann es durch einen anderen Player empfangen werden, bevor der Spieler in der Sitzung reserviert wurde. Diese Racebedingung kann dazu führen, dass eine **join()** rufen Sie für einen eingeladenen Player fehlschlägt, da sichtbare oder -Sitzungen eine Reservierung für den verknüpften Player erforderlich sind.
>
> Wenn keine Reservierung vorhanden ist, wird der Fehler "Zugriff" in der Sitzung (HTTP/403) ausgelöst. Der eingeladene Player müsste dann wiederholen den **join()** Vorgang, aber Wiederholungen müssen nicht häufiger als alle fünf Sekunden ausgeführt werden. Es wird empfohlen, dass Titel Sichtbarkeit der Sitzung zu öffnen, um Racebedingungen zu vermeiden, während des Join-Datenflusses für einen Player festgelegt.

## <a name="game-session-joinability"></a>Spiele-Sitzung joinability


Der zweite Aspekt an der Zugriffssteuerung ist Joinability. Es kann dynamisch während der Lebensdauer der Sitzung festgelegt werden, und bestimmt, welche Arten von Benutzern eine Sitzung beitreten können. Die Werte für die Sitzung Joinability sind:

-   *Keine* (Standardwert) – Es gibt keine Einschränkungen auf, die der Sitzung teilnehmen kann.

-   *Lokale*– nur lokale Benutzer der Sitzung teilnehmen können.

-   *Folgen –* nur lokale Benutzer und Benutzer, die andere Sitzung Member folgen der Sitzung ohne Reservierung teilnehmen können.

Ein Sitzungshost oder ein Arbiter kann eine private Sitzung durch die Einstellung für Joinability erstellen: Beschränken des Zugriffs auf eine Spiele-Sitzung, und legen Sie ihn private wird Joinability machen, entweder lokal oder gefolgt.

Darüber hinaus die Sitzungshost oder Arbiter sollten verfolgen die Joinability einer Sitzung, der, damit ältere Sitzung Einladungen auf der Hostebene zurückgewiesen werden können, falls erforderlich. Z. B. ob jeder eingeladene Spieler nicht angekommen sind, um eine Sitzung und/oder Partei zu verknüpfen, bis die Sitzung bereits voll ist, kann den Host oder -Arbiter Verknüpfung Player anweisen, dass die Sitzung wurde gesperrt, und sie müssen die Sitzung beenden und/oder Partei automatisch.

**Hinweis:** Spiele-Sitzung Joinability ist in der App-Benutzeroberfläche von Drittanbietern nicht übernommen. Auch wenn Joinability beschränkt ist, laden die Partei-App UI und **ShowSendInvitesAsync** können Spiele Sitzung Einladungen.

## <a name="game-party-joinability"></a>Spiele Partei joinability


Zusätzlich zum Steuern der Sitzungs MPSD, hat den Titel auch die Kontrolle über den Status des Spiels Partei Joinability. Diese Einstellung kann verwendet werden, spielen Partei den Zugriff auf parteiebene einschränken.

Partei Joinability kann dynamisch geändert werden, nachdem die Spiele Partei erstellt wurde. Der Status der Joinability einer Spiele-Partei wirkt sich die Partei einladen Benutzeroberfläche. Es kann festgelegt werden:

-   *Einladung nur –* diese Einstellung erfordert eine Einladung, um das Spiel Partei beitreten.

-   *Beigetreten werden kann, indem Sie Freunde* (Standardwert)*–* diese Einstellung erfordert eine Friend-Beziehung für einen Player, verknüpfen Sie die Spiele Partei (um eine Partei zu verknüpfen, eine Partei Mitglied muss sich den verknüpften Player führen).

Joinability kann so erstellen Sie eine Einladung nur Spiele Partei verwendet werden. Zum Einschränken des Zugriffs auf eine Partei und erfordern, dass der Spieler eine Einladung zum Beitritt erhalten haben, sollte Joinability "nur einladen" festgelegt werden.

**Hinweis:** Partei Joinability beeinflussen nicht die Sitzung Joinability, aber die Joinability Partei in der App-Benutzeroberfläche von Drittanbietern wiedergegeben wird. Die Partei Joinability in der Partei-App manuell außerhalb eines Titels können Spieler geändert werden.

## <a name="recommendations"></a>Empfehlungen


Die folgenden Empfehlungen gelten für die häufigsten Szenarien für den Titel. Titel sollte diesen Einstellungen folgen, wenn möglich, und sie sollten in der Title-Logik verwenden, um die Stellen der letzte, autoritativen Bestimmung, ob ein neues Players in die Sitzung oder nicht zugelassen wird.

### <a name="recommended-game-session-visibility-open"></a>Sichtbarkeit des Spiels Sitzung empfohlen: Öffnen

Offene game-Sitzungen erfordern keine Player Reservierungen, um so vereinfachen den Ablauf für Einladungen. Der Host für Remotedesktopsitzungen oder Arbiter reserviert keinen Spieler in der Sitzung Dokument nach dem eine Einladung gesendet wurde. Der Host für Remotedesktopsitzungen oder Arbiter verfolgt stattdessen nur lokal eingeladenen Spieler. Dadurch wird ein Spieler, um sofort eine Verbindung mit dem Host herstellen und zu bestimmen, ob sie einer Sitzung beitreten soll, zurückgewiesen werden, oder warten soll, (wenn es sich um eine warten-Player unterstützt werden). Der Arbiter oder der Host ist die endgültige Instanz reagiert und weist das neue Element zu behalten, oder lassen die Sitzung.

**Hinweis:** Dies erfordert, dass den eingeladenen Player starten Sie einen Titel ein, und auf dem Host oder -Arbiter verbinden, bevor die endgültige Entscheidung vorgenommen wurde. Es ist akzeptabel, eine Fehlermeldung angezeigt, die dem Benutzer angezeigt wird, wenn eine Sitzung voll ist, oder eine INVITE-Nachricht zurückgewiesen wurde.

> **Hinweis:** Um eine Verbindung mit dem Host oder -Arbiter herzustellen, ist eine sichere Adresse des Geräts erforderlich. Die **HostDeviceToken** in der Sitzung zu verwendende um anzugeben, dass die Sitzung-Element der aktuellen Host oder der Arbiter einer Sitzung und die sichere Geräteadresse ein eingeladener Player muss mit dem.

### <a name="recommended-game-session-joinability-default-not-set"></a>Spiele-Sitzung Joinability empfohlen: Standard (*nicht festgelegt*)

Derzeit Spiel Sitzung Joinability keinen Einfluss auf die Partei-App-Benutzeroberfläche und wird für den Benutzer nicht angezeigt. Zur Vereinfachung des Title-Flows Spiel Joinability den Standardwert belassen werden soll, und alle Join Zertifizierungsstelle über das Arbiter oder Host behandelt werden sollen.

### <a name="recommended-game-party-joinability"></a>Spiele Partei Joinability empfohlen

Spiele Partei Joinability ist abhängig von dem Typ der Sitzung, die ein Titel für einem Benutzer bereitstellen möchte. Die beiden Szenarien sind:

-   Öffnen Sie die Spiel – für ein open-Spiel, das Spiel Partei Joinability sollte den Standardwert bleiben: Indem Sie Freunde beigetreten. Dadurch wird das Spiel Partei (und somit die Spiele-Sitzung) Verknüpfen von Freunden ohne eine INVITE-Nachricht.

-   Private Spiel – für ein privates oder geschlossenen-Spiel, das Spiel Partei Joinability, nur mit einer Einladung festgelegt werden soll. Dies schränkt Weitere Entwicklungen von der Partei beitreten (und durch Erweiterung der game-Sitzung) ohne eine INVITE-Nachricht.

**Hinweis**: Spieler können die Joinability einer Spiele-Sitzung über die Partei-App manuell ändern.

Für beide spielen sollten die Arbiter oder Host weiterhin die endgültige Instanz aus, um zu bestimmen, die in einer Sitzung akzeptiert wird. Dies behebt alle Racebedingungen, die von einem umschalten Spielfluss aus öffnen, um private auftreten können. Lehnt die Arbiter-Host einen Player, sollte die Instanz abgelehnt Titel den Player aus der Sitzung entfernen und eine in-Game-Benutzeroberfläche können auch einen Player, die Partei zu verlassen.

### <a name="maximum-session-size"></a>Maximale Sitzungsgröße

Die maximale Member-Größe für eine Sitzung kann verwendet werden, um die Anzahl der Spieler beitreten zu einer spielen-Sitzungs zu beschränken. Dieser Grenzwert kann jedoch ein geöffnet zeigt an, weiterhin angezeigt wird, wie voll während bestimmter Szenarien trennen führen. Z. B. wenn ein Player durch ein Netzwerk oder Stromausfall getrennt wird, wird die Verzögerung nicht sofort in der Sitzung wiedergegeben. Das Element wird auf inaktiv festgelegt werden, sobald der Anwesenheit-Dienst die Trennung der Verbindung (bis zu 20 Sekunden erkennt), und der Player wird dann entfernt, nachdem das inaktive-Timeout abgelaufen ist.

Im Gegensatz dazu einem Peermesh, die einen Takt verwendet, um eine Trennung zu erkennen ist häufig innerhalb von zwei bis drei Sekunden eine Trennung der Verbindung kennen, und konnte öffnen Sie die Player-Slot sofort. Andere Member kann jedoch nicht dem Arbiter oder Host entfernen.

Um dieses Problem zu beheben, sollte die Größe der maximalen Member einer Sitzung stets höher als die maximale Anzahl der Spieler festgelegt werden, die ein Titel tatsächlich unterstützt. Z. B. wenn die Anzahl der Spieler 8 ist, sollte der Titel festgelegt der maximale Sitzungsgröße auf 12. Auf diese Weise können neue Spieler verknüpfen, auch wenn die alte Spieler noch nicht abgelaufen haben. Die Arbiter oder Host bestimmt, ob die Sitzung voll ist und legt dann eine benutzerdefinierte Eigenschaft, die bestimmt, ob neue Spieler noch hinzugefügt werden können (**IsFull** : "true"). Dies ermöglicht eine schnelle Überprüfung Spieler beitritt, wenn die Sitzung beigetreten ist.

Verwalten der Arbiter oder der Host sollte auch eine benutzerdefinierte Eigenschaft, der angibt, welche Elemente nach Index Zeitüberschreitung (z. B. **TimedOutPlayers** : “3, 5, 9”). Dadurch wird ein neuer Spieler die Elemente der aktuellen Sitzung ordnungsgemäß zu identifizieren.

### <a name="notifying-waiting-players"></a>Benachrichtigen der warten-Player

Ein Titel kann wartenden Spieler durch Verwalten einer Liste in der Warteschlange Player neben der game Partei unterstützen. Wenn ein Player stellt eine Verbindung mit dem Host her, und die game-Sitzung voll ist, wird die Liste interner Wartevorgang auf dem Host der Spieler hinzugefügt. Der Spieler in der Warteschlange ist nicht an die Spiele-Sitzung teilnehmen, bleibt jedoch im Spiel Partei. Um den Netzwerkverkehr zu minimieren, trennt der warten-Player des Hosts an diesem Punkt.

Wenn ein Slot in der game-Sitzung für den warten-Player geöffnet wird, der Arbiter oder der Host Fügt eine Reservierung für den Player durch Aufrufen der **AddMemberReservation** Methode. An diesem Punkt der warten-Player ist noch nicht kennen diese Reservierung, der Arbiter oder der Host hat daher zum Aufrufen der **PullReservedPlayersAsync** Methode. Dies bewirkt, dass eine Benutzeroberfläche Toast oder **GameSessionReady** Benachrichtigung für alle Spieler reserviert, je nach Toast-Konfiguration und Fokuszustands Titel. Die neuen Players erneut eine Verbindung mit dem Host herstellt, wenn diese Benachrichtigung empfangen wird und die Sitzung verknüpft.

Alternativ kann ein Player mit dem Host verbunden bleiben, und verknüpfen die Sitzung, wenn der Host ein Steckplatz verfügbar ist, dem Spieler Warnungen. In diesem Flow kann jedoch ein UI-Popup außerhalb des Titels angezeigt werden.

**Hinweis:** Spieler werden automatisch von der Partei Spiel entfernt werden, wenn der Titel angehalten bzw. beendet und keine weiteren Benachrichtigungen erhalten.

<a name="client-multiplayer-flow"></a>Multiplayer-clientfluss
-----------------------
![](../../images/whitepapers/gamesessionvisibility_image1.png)
![](../../images/whitepapers/gamesessionvisibility_image2.png)
