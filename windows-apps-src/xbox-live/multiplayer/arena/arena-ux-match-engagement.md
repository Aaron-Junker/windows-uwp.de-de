---
title: Engagement übereinstimmen
description: Beschreibt die UX-Phasen der Spieler, die durch eine Turnier fortgesetzt.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Bereich, -Turniers, ux
ms.localizationpriority: medium
ms.openlocfilehash: edc81ff7c692c4789855d341917c54acf4d24594
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598355"
---
# <a name="match-engagement"></a>Engagement übereinstimmen

Nachdem ein Spieler wurde registriert und für ein Turnier eingecheckt, sind sie eingerichtet, wiederzugeben. Ein Teilnehmer kann eine der vier Phasen sein. Jede Stufe enthält einen eindeutigen Satz von Kriterien, die für die Benutzer über die in-Game-Turniers-Funktionalität. Die vier Phasen sind:

1.  Bereit (Spielbereich Benutzeroberfläche oder im Spiel)
2.  (In-Game) wiedergeben
3.  Ergebnisse (Bereich Benutzeroberfläche oder im Spiel)
4.  Ende (Spielbereich Benutzeroberfläche oder im Spiel)

> [!NOTE]  
> Die Wiedergabe ist als einzige Phase der vier, die in der Benutzeroberfläche der Spielbereich nicht unterstützt wird. Zumindest muss Titel des Teilnehmer an der Spielbereich Benutzeroberfläche am Ende jeder Übereinstimmung (wenn die Stufe spielen befindet), umleiten, damit diese Ergebnisse anzeigen können.

###### <a name="diagram-the-four-stages-of-participant-progression-after-check-in"></a>Diagramm: Die vier Phasen der beteiligten Fortschritt, nach dem Einchecken.

![Übereinstimmung Engagement-Flussdiagramm](../../images/arena/arena-ux-match-flow.png)

![Übereinstimmung Engagement Flow Balken](../../images/arena/arena-ux-match-flow-bar.png)

## <a name="1-ready-waiting-for-match"></a>1. Bereit ("wartet auf Übereinstimmung")

![Warten auf Übereinstimmung Diagramm](../../images/arena/arena-ux-flow-ready.png)

### <a name="user-impact"></a>Benutzerauswirkungen

Klicken Sie in dieser Phase der Spieler geht davon aus, die in einer Übereinstimmung zu spielen, aber der Gegner ist nicht noch bekannt. Diese Phase wird ausgelöst, wenn ein Turnier gestartet wurde, und der Teilnehmer darauf, dass ihre Sitzung Übereinstimmung wartet, um zu beginnen. Diese Wartezeit kann folgende Ursachen haben:

* Der Punkt-Check-in ist noch geöffnet.
* Die nächste Runde wurde nicht gestartet.
* Der Teilnehmer verfügt über eine Bye in der aktuellen gerundet werden.

### <a name="when-a-bye-is-needed"></a>Wenn eine Bye erforderlich ist

Eine Bye kann in jeder Runde der ein Turnier, geschehen, obwohl es das seeding verteilt, um zu versuchen, dies zu vermeiden.

Jedes Mal, wenn eine einzelne-Eliminierung von Duplikaten oder Double-Wert-Eliminierung von Duplikaten Turnier umfasst eine Reihe von Playern oder Teams, die keine Potenz von 2 (d. h., 4, 8, 16, 32, 64, 128 oder 256) ist, müssen Bytes vergeben werden. Eine Bye bedeutet einfach, dass ein Team muss keine Teilnahme an der ersten Runde des Tourniers teilnehmen, und ruft stattdessen einen kostenlosen Zugriffs auf die zweite runde ab.

### <a name="discovery"></a>Ermittlung

![Screenshot der Team-Ermittlung](../../images/arena/arena-ux-team-discovery.png)

Erfahren Sie Teilnehmer zu ihrem Status an diesen Orten, in der Phase bereit:

* Spielbereich Benutzeroberfläche: Die Detailseite des Turniers
* Im Spiel: Liste Turnier, Promotion, Übereinstimmung Lobby

> [!TIP]  
> **UX-Empfehlungen**  
> Geben Sie, dass der Player für die nächste Übereinstimmung im Ereignis wartet. Beispiele: "Abgleichen mit Ausstehend", oder klicken Sie mit der Benachrichtigung "Sie haben eine Bye erhalten".  
>
> Wenn diese Phase unterstützt wird, geben Sie eine Methode zur Detailseite-Turniers zurückgeben.  
>
> Geben Sie Kontext des Ereignisses: Turnier Name, Status, Beschreibung, Format beginnen Zeit/End Time usw.


###### <a name="ui-example-in-game-multiplayer-lobby"></a>Beispiel der Benutzeroberfläche: Multiplayer-in-Game-Lobby

![Bildschirmabbildung von Lobby Turnier](../../images/arena/arena-ux-tournament-lobby.png)

Immer vorhanden Sie das Turnier Status und der Grund in einer konsistenten Position, wo Turnier Übereinstimmung höher gestuft wird oder Übereinstimmung Details werden angezeigt.

Dies ist die wichtige Anleitungen für die es Spielern ermöglichen, wenn sie entscheiden, müssen:

* Lassen Sie das Spiel um mehr zu erfahren.
* Welche nächsten Schritte sind erforderlich, während es in ein Turnier verstreichen.

### <a name="under-the-hood"></a>Im Hintergrund
Wenn diese Phase nach der Titel unterstützt wird, fortgesetzt werden auf die Bühne spielen, wenn die Übereinstimmung bereit ist.

 
## <a name="2-playing"></a>2. Wiedergeben

![Spielen Sie das Diagramm Übereinstimmung](../../images/arena/arena-ux-flow-play.png)

### <a name="user-impact"></a>Benutzerauswirkungen

Des Spielers nächste Übereinstimmung von ist bekannt, und die Multiplayer-Sitzung erstellt wurde. In dieser Phase verschiebt der Titel der Spieler über dem normalen Fluss eines Spiels (Match-Setup-Bildschirme, Spiele Lobby usw.). Dies ist die Stufe, die Benutzer zu erreichen, wenn die Übereinstimmung bereit ist.

### <a name="discovery"></a>Ermittlung

Es gibt zwei Möglichkeiten, einen Turnier Teilnehmer-Warnung aus, und aktivieren, um eine Übereinstimmung Turnier aus außerhalb eines Spiels zu verknüpfen:

* **Xbox-Popupbenachrichtigung** – Wenn die Benachrichtigung angezeigt wird, kann der Teilnehmer enthalten, die ![Nexus Schaltfläche](../../images/xbox-nexus.png) (Nexus Schaltfläche), um das Spiel zu starten, und geben eine Übereinstimmung.

  ![Übereinstimmung bereit toast](../../images/arena/arena-ux-match-ready-toast.png)

* Seite mit Details zu Spielbereich Turnier – die Teilnehmer kann auswählen, **spielen** das Spiel zu starten, und geben eine Übereinstimmung
 
> [!TIP]  
> **UX-Empfehlung:** Match-Eintrag zu bestätigen.  
> Zeigen Sie die Benutzeroberfläche, die darüber informiert, und/oder bestätigt, dass der Teilnehmer wird direkt in einer Match-Lobby eingeben.

###### <a name="ui-example-match-entry-confirmation-dialog-box"></a>UI-Beispiel: Bestätigungsdialogfeld Match-Eintrag

![Bestätigungsdialogfeld für Übereinstimmung Eintrag](../../images/arena/arena-ux-confirm-match-entry.png)

Diese Benutzeroberfläche möchte, dass der Teilnehmer ein Turnier geben an einem Speicherort außerhalb des Spiels angezeigt. Dies ist notwendig, nachdem ein Spiel gestartet wurde und bevor Sie eingeben der Übereinstimmung Lobby ist.

> [!TIP]  
> **UX-Empfehlung:** Geben Sie eine Option zum abzubrechen.

Teilnehmer können, dass andere Aufgaben, die sie benötigen zum Abschluss der einzelnen turnierteilnehmer Vorbereitung vorhanden sind. Es ist wichtig, um ihnen die Flexibilität, ihre Meinung ändern zu gewähren. Titel des bieten zusätzlicher Optionen:

* Bleiben Sie in's Spiel. (Der Teilnehmer wird zum Hauptmenü erstellt.)
* An der Spielbereich Benutzeroberfläche zurück.
* Wenn der Teilnehmer auswählt, das Spiel zu verlassen, es wird jedoch empfohlen, der Titel informieren, den Teilnehmer an einer die verbleibende der Zeit, bevor die einbehaltene Timeout-Limit erreicht wurde.

###### <a name="ui-example-match-entry-confirmation--leave-game-warning"></a>Beispiel der Benutzeroberfläche: Eintrag Bestätigung überein – lassen Sie Spiele Warnung

![Bestätigungsdialogfeld für Übereinstimmung Eintrag einbehaltene](../../images/arena/arena-ux-confirm-match-cancel.png)

### <a name="playing-a-match"></a>Wiedergeben einer Übereinstimmung

Die Startzeit für die Übereinstimmung wird in der sitzungsvorlage definiert. Sie werden als eine Datums-/ Uhrzeitangabe in die UTC-Standardformat festgelegt, z. B. `2009-06-15T13:45:30.0900000Z`. Der Organisator Turnier kann sollen, einschließlich der gleichzeitig mit der Startzeit die Sitzung so weit im voraus die Startzeit erstellen. Übereinstimmung Benachrichtigungen werden an die Teilnehmer Übereinstimmung zur Startzeit ausgeführt, gesendet, damit der Titel nicht vor der Startzeit aktiviert werden. Im Allgemeinen Behandeln der Spielbereich-Sitzungs die gleiche Weise, die Sie Ihrem des Titels Multiplayer-Sitzung behandeln würde. Es gibt jedoch einige Unterschiede:

* Titel des kann die Liste der game-Sitzung nicht ändern.
* Es ist nicht das übliche sicherheitsüberprüfung Verfahren für Spieler mit problematischen verbindungsgeschwindigkeiten aus.
* Titel des muss Vermittlung teilnehmen.

### <a name="match-lobby-details"></a>Übereinstimmung Lobby details

Ein Turnier Übereinstimmung Lobby sollte Details enthalten, die verdeutlichen, dass es sich um ein Turnier ist und standardmäßigen Multiplayer-Übereinstimmung wird. Beispiele für solche Details sind Timing, Team-Abhängigkeit und aufeinanderfolgenden runden und Phasen. Wenn sich die Übereinstimmung befindet, fahren Sie fort mit Schritt 3 (Warten auf Ergebnisse), oder den Spieler auf der Benutzeroberfläche der Spielbereich zurück.

###### <a name="ui-example-match-lobby-details"></a>Beispiel der Benutzeroberfläche: Übereinstimmung Lobby details

![Übereinstimmung Lobby Detailbildschirm](../../images/arena/arena-ux-match-lobby-details.png)

**Ein** -sicherzustellen, dass dies eine Übereinstimmung für "-Turniers" oder "Bereich" ist.  
**B** -Turniers-Details  
   1. Name des Turniers (maximal 128 Zeichen)  
   2. Stil, Spielmodus  
   3. Round oder Phasennummer  

**C** – bereit Registrierung / Wiedergabeschaltfläche – Teilnehmer können ein Turnier eingegeben haben, aber erfordert zusätzliche Zeit vor Beginn der Sitzungs überein. Beispielsweise kann eine Übereinstimmung eingerichtet ist, wie z. B. Zeichenauswahl erforderlich. Diese Unterstützung kann die anderen Teammitglieder informieren, wenn sie bereit sind.  
**D** -Countdown-Timer
  * Der Zeitgeber wird basierend auf den **einbehaltene Timeouts** (festgelegt durch das Spiel).
  * Dies gibt eine Warnung aus sowohl Teams, wenn die minimale teamanforderungen bis zu diesem Zeitpunkt nicht erfüllt werden, damit das Spiel verfallen ist und die aktiven Team gewinnt.  

**E** - Broadcast Erinnerung  
**F** -Team und Teilnehmer

 
### <a name="under-the-hood"></a>Im Hintergrund

#### <a name="protocol-activation"></a>Protokollaktivierung

Die Benutzeroberfläche der Spielbereich startet Dinge direkt in einen Titel Teilnehmer senden, wenn ein Benutzer zum Abspielen einer Übereinstimmung bereit ist – z. B. beim Akzeptieren des "Match bereit" toasts per der Spielbereich. Protokollaktivierung in zwei Mal auftreten kann: Titel gestartet wird, oder wenn er bereits ausgeführt wird. In beiden Fällen ist die Aktivierung ähnelt, was geschieht, wenn Sie ein Titel, die als Reaktion auf ein Benutzer eine Einladung an das Spiel akzeptiert aktiviert ist.

* **Wenn der Titel gestartet wird** – die **Activated** Ereignis ausgelöst wird, zum ersten Mal, wenn der Titel wird gestartet. Wenn diese erste Aktivierung einen Spielbereich protokollaktivierung ist, bedeutet dies, dass es sich bei der Titel von einem Benutzer, bei dem Versuch, ein Turnier spielen gestartet wurde. Der Titel sollte so schnell wie möglich mit der Übereinstimmung unter Umgehung der Haupt- und Anmeldebildschirme fortfahren. Die Xbox-Benutzer-ID (XUID) von den beteiligten Benutzer wird bei der Aktivierung URI bereitgestellt, und es sollte bereits angemeldet sein.

* **Wenn der Titel bereits ausgeführt wurde** – die **Aktivierung** Ereignis kann auch mit einer Spielbereich protokollaktivierung ausgelöst, während der Titel bereits ausgeführt wird. Wenn Sie der Titel im Leerlauf befindet, wenn sie die protokollaktivierung empfängt, sollte es sofort zur Turnier Übereinstimmung wechseln. Es ist sicher, da das Aktivierungsereignis, nur durch eine explizite Benutzeraktion ausgelöst wird (, die auf ein Popup, das zur Übereinstimmung führt, oder die Übereinstimmung der Beginn der Spielbereich-Benutzeroberfläche) dies. Wenn der Titel der protokollaktivierung während der Spiele empfängt, sollte es dem Benutzer bieten die Möglichkeit, lassen das Spiel (Speichern Sie es zuerst, falls erforderlich) und geben die Übereinstimmung Turnier zweckmäßigste wie möglich.

* **Wenn ein Teilnehmer die Xbox-Popupbenachrichtigung umgeht** – es wird empfohlen, ein Titel ein Spielbereich-fähigen Abfragen Spielbereich immer beim Start, um festzustellen, ob der Benutzer in einer aktiven Übereinstimmung ist. Wenn Sie also der Titel angezeigt werden sollen, "bestätigen Übereinstimmung Eintrag" Benutzeroberfläche den Benutzer mitzuteilen, sie haben eine Übereinstimmung, und bitten Sie, ob sie ihn eingeben möchten.  

  Dadurch wird sichergestellt, dass Teilnehmer des Übergangs in eine Übereinstimmung Turnier richtig machen, Fall, dass sie den Übereinstimmung Toast übersehen, und starten Sie einfach das Spiel (erhalten, die in deren Übereinstimmung erwartet). Oder, im Fall des Spiels stürzt ab, und der Teilnehmer relaunches nur den Titel in ihre Übereinstimmung zurückzukehren.

> [!NOTE]  
> Der Titel sollte die XUID auf der protokollaktivierung URI überprüfen. Wenn sie den aktuellen Spieler des Titels nicht übereinstimmt, muss der Titel auch Benutzerkontexten wechseln.

#### <a name="forfeit-time-out"></a>Einbehaltene-Timeout

Mindestens ein einziger Player muss in der Sitzung vor dem Zeitpunkt einbehaltene aktiv sein, wird die Startzeit plus der einbehaltene-Timeout (Wiedergabe Diagramm finden Sie unter). Wenn niemand die Sitzung als aktiv vor dem Zeitpunkt der einbehaltene verknüpft ist, wird die Übereinstimmung wird abgebrochen, und beide Teams einen Verlust über Spielbereich Vermittlung erhalten.
 
## <a name="3-results"></a>3. Ergebnisse

![Ergebnisse des Diagramms Übereinstimmung](../../images/arena/arena-ux-flow-results.png)

Dies ist die Phase, wenn ein Player die Übereinstimmung abgeschlossen ist und die Ergebnisse auf arbitrated werden gemeldet wurden. An diesem Punkt wird es noch nicht bewertet wurde, ob das Team noch in der einzelnen turnierteilnehmer ist. In der Zwischenzeit sollte die Erfahrung der "End Game"-Bildschirm für die berichterstellung weiterhin die Ergebnisse der Sitzung (wie in der jede Multiplayer-Übereinstimmung).

Die Ergebnisse des Turniers – keine Wettbewerb (ohne Spiel), Windows-Taste, Verlust, zeichnen, Rang oder keine anzeigen – folgen.

Es ist möglich, für diese Phase übersprungen werden soll, wenn das Ergebnis sofort verfügbar ist.

### <a name="reporting-results"></a>Ergebnisbericht

Die Ergebnisse der Übereinstimmung werden gemeldet Spielbereich und der Organisator Turnier, über die Sitzung mit einem Feature namens *Vermittlung*. Vermittlung ist ein Framework für die Verwendung einer Match-Sitzungs um sicher eine Übereinstimmung und einen Bericht ein Ergebnis wiederzugeben.

Wenn eine Übereinstimmung Sitzung beginnt, eine arbitrated Sitzung gilt. Er verfügt über eine feste Zeitachse, die das Framework für die Vermittlung erzwingt.
 
### <a name="arbitration-time-out"></a>Timeout für die Vermittlung

Das Timeout für die Vermittlung ist die maximale Zeitspanne, nach die Übereinstimmung Startzeit, die Teilnehmer spielen und Ergebnisse zu melden. Dieser Wert wird in der sitzungsvorlage für Turnier Planer-Run-Ereignisse und im Spielbereich Multiplayer-Spiel Modus für UGT und Franchise-Run-Ereignisse, festgelegt, damit sie soviel Zeit wie nötig ab, der Titel werden kann.

Der Titel kann die Ergebnisse zu einem beliebigen Zeitpunkt zwischen der Startzeit und die Uhrzeit für die Vermittlung melden. Vermittlung tritt auf, zu einem beliebigen Zeitpunkt zwischen dem einbehaltene und dem Zeitpunkt der Vermittlung nach jeder aktives Mitglied der Sitzung Ergebnisse übermittelt hat. Z. B. wenn nur ein Element in der Sitzung zum einbehaltene Zeitpunkt aktiv ist, sie können (und sollten) veröffentlichen wird ein Ergebnis und Vermittlung erfolgt. Unabhängig davon, wie viele Ergebnisse zur Vermittlung Zeit verfügbar sind treten Vermittlung, wenn dies noch nicht der Fall.

Wenn ein Benutzer in einer Sitzung, die bereits arbitrated wurde wurde (da das Timeout für die Vermittlung abgelaufen ist, ein Gameserver die Sitzung vermittelt oder der Benutzer später verknüpft), ist, Ihre Titel sollte Ende des Abgleichs und das arbitrated Ergebnis für den Benutzer angezeigt.

Vermittlung Ergebnisse enthalten immer das Ergebnis für jedes Team. Wenn eine einzelne Spielergebnis gemeldet wird, enthält sie nicht nur ihrer Teams Ergebnis, aber der vollständige Satz von Ergebnissen für jedes Team.
Vermittlung wird automatisch gestartet, sobald der erste Client meldet die Ergebnisse zu Xbox Live. Sie endet, sobald alle Clients melden die Ergebnisse. Es gibt möglicherweise Instanzen, in dem Teilnehmer länger als für die Vermittlung angegeben, in der sitzungsvorlage wiedergeben. Dies verursacht ein Risiko, dass eine Übereinstimmung erzwungen wird, bevor die Teilnehmer bereit sind.

> [!TIP]  
> **UX-Empfehlung:** Geben Sie einen Zeitrahmen für den festen Match-Sitzung.
>
> Vordefinierte Zeitlimits bei Match-Sitzungen reduziert das Risiko von Problemen, die von Vermittlung Timeout auftritt.

Es gibt zwei Optionen, die verhindern, dass eine Übereinstimmung beenden, bevor der Teilnehmer haben während der Wiedergabe:

* Festlegen eines großzügigen Zeitraums für die Vermittlung Timeout an.
* Wenn einer festen Zeitrahmen steht in Konflikt mit Ihrer Titelformat Gaming, führen Sie die folgende Lösung aus:
   1.   Bestimmen Sie, dass die Ergebnisse nicht angezeigt worden sein und das Timeout für die Vermittlung wird in Kürze abläuft (z. B. fünf Minuten verbleiben).
   2.   Anzeigen einer Benutzeroberfläche, die die Teilnehmer warnt, ihr Spiel in X beendet wird Minuten. Diese Option kann für alle Turniere implementiert werden, und wenn das Timeout für die Vermittlung großzügigen ist, die meisten Teilnehmer nicht bemerkt.

### <a name="returning-to-the-arena-ui-by-using-a-dedicated-a-button-press"></a>Zurückgeben der Spielbereich-Benutzeroberfläche mithilfe einer dedizierten Klicken einer Schaltfläche

Wenn der Titel des Turniers Ergebnisse oder Phase Fortschritt im Spiel Oberfläche nicht, empfehlen wir, dass der Teilnehmer haben eine Möglichkeit, auf der Benutzeroberfläche zurückzugeben, die diese Informationen verfügbar ist. Dies kann die Benutzeroberfläche des Bereich oder einer Drittanbieter-Turniers Organisator app sein. Im Idealfall wird diese Unterstützung angezeigt, nachdem Übereinstimmung über oder möglicherweise als Antwort auf Anforderung des Spielers, verwerfen Sie die Übereinstimmung in Bearbeitung ist. Dies könnte eine Ende-des-Game-berichtsbildschirms erfolgen.

> [!TIP]  
> **UX-Empfehlung**  
> Verwenden Sie eine Schaltfläche "Controller" als eine schnelle Möglichkeit für die es Spielern ermöglichen, auf den Spielbereich Hub im Xbox-Dashboard zurück.  
>
> ![Schaltfläche "Controller" der Spielbereich-hub](../../images/arena/arena-ux-arena-hub-button.png)

### <a name="redirect-participants-at-the-end-of-a-match"></a>Umleiten der Teilnehmer am Ende der Übereinstimmung

Teilnehmer die Übereinstimmung abgeschlossen haben, ist es wichtig, dass es sich bei Ihrem Namen zu geben die Richtung zu den nächsten Schritten zu löschen. Dies bietet eine Methode, um round-Turniers oder Phase Ergebnisse zu überprüfen (wenn nicht im Spiel dargestellt).

> [!TIP]  
> **UX-Empfehlung:** Verwenden Sie ein Popup Overlay an.
>
> Diese Benutzeroberfläche wird angezeigt, wenn Turnier Ergebnisse werden in die End-Game-Erfahrung ist abgeschlossen.

### <a name="recommended-ui-data"></a>Empfohlene UI-Daten:

* Name des Turniers
* Nächste Klammer
* Nächste Runde oder eine Stufe-details
* Team ständigen
* Arbitrated Ergebnisse
* Deep-Link-Turniers-Details
* Möglichkeit haben, im Spiel

### <a name="under-the-hood"></a>Im Hintergrund

Nach dem Aufrufen der Spielbereich-Benutzeroberfläche, Ihre Titel sollte weiterhin ausgeführt werden, möglicherweise in diesem gleichen Bildschirm warten auf ein anderes protokollaktivierung Ereignis. Klicken Sie dann, wenn der Spieler eine weitere Übereinstimmung spielen hat, wird der Titel einsatzbereit sein. Sie können die benutzerfreundlichkeit beschleunigen, der Spieler von Übereinstimmung, Übereinstimmung wird zwischen den Titel und die Benutzeroberfläche der Spielbereich wechseln.

###### <a name="ui-example-tournament-arbitrated-resultswinning-team"></a>Beispiel der Benutzeroberfläche: Turnier arbitrated Ergebnisse – ausschlaggebende Team

![Bildschirm gewonnen hat](../../images/arena/arena-ux-won-game-redirect.png)

###### <a name="ui-example-end-of-tournament-resultsmatch-ended"></a>Beispiel der Benutzeroberfläche: Ende-des-Turniers Ergebnisse – Übereinstimmung endete 

![Bildschirm verloren gegangen ist](../../images/arena/arena-ux-lost-game-redirect.png)

## <a name="4-end"></a>4. Ende

![Ende des Diagramms Übereinstimmung](../../images/arena/arena-ux-flow-end.png)

Die End-Phase eines Flows Turnier tritt auf, wenn es sich bei der einzelnen turnierteilnehmer selbst wurde beendet, und die Endergebnisse gemeldet werden.

Teilnehmer werden informiert, über das Turnier befindet oder dass der Spieler behoben wurde.

### <a name="discovery"></a>Ermittlung

Der Spielbereich Benutzeroberfläche zeigt Ergebnisse an, und seit der letzten Gewinner auf dem Markt:

* Turnier Seite "Details"
* Ergebnisse, die von Teilnehmern in ihren Aktivitätsfeed Player oder Vereine gemeinsam verwendet werden
* Turnier Ergebnisse des Spielers-Profil bereitgestellt

Titel des kann Turnier Ergebnisse im Spiel Oberfläche:

* Nach dem Ende-des-Game-Erfahrung für die finale bewertungsrunde der Teilnehmer abgeschlossen.
* Aufgeführt **Meine Turniere**, in einem Feature-Turniers zu durchsuchen.


###### <a name="ui-example-end-of-tournament-resultstournament-ended"></a>Beispiel der Benutzeroberfläche: Ende des Turniers Ergebnisse – Turnier beendet

![Turnier über den Bildschirm](../../images/arena/arena-ux-tournament-completed.png)

* Wenn diese Phase unterstützt wird, geben Sie eine Option aus, um zu den Spielbereich UI Turnier Organisator app zurückzukehren.
* Der Name des Ereignisses angezeigt.
* Angezeigt werden den Teamnamen ein, wenn das Team mehr als ein Member aufweist.
* Zeigen Sie des Teams ständigen, in dem Fall, sofern verfügbar. Wenn keine ständigen verfügbar ist (oder optional zusätzlich zu den Rang) sollten Titel des laut des Spielers Teilnahme an das Ereignis beendet geschaltet wurde.
* Titel des schlägt nächste Schritte vor (z. B. "Sehen Sie sich livestreams," "Finden eine andere Turnier,", "bleiben im Spiel").
* Titel des bietet eines Grund, warum das Team Engagement beendet wurde:
  * Abgelehnt: das Team nicht, um die Qualifikationen für die Eingabe der einzelnen turnierteilnehmer zu erfüllen.
  * Entfernt, das Team hat die Übereinstimmung Turnier verloren.
  * Entfernt, das Team aus einer aktiven Turnier entfernt wurde.
  * Abgeschlossen: das Team abgeschlossen finale bewertungsrunde des Tourniers teilnehmen.
