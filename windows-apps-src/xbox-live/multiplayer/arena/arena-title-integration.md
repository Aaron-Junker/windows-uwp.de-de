---
title: Integrationshandbuch für Spielbereich-Titel
description: Erfahren Sie, wie ein Titel in der Unterstützung für die Xbox Live Spielbereich-Plattform erstellen kann.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Bereich, Turnier
ms.localizationpriority: medium
ms.openlocfilehash: 891fa8da1ca6e26128e0a33d28a505a18e99662a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659885"
---
# <a name="arena-title-integration-guide"></a>Integrationshandbuch für Spielbereich-Titel

## <a name="introduction"></a>Einführung

Die Spielbereich-Plattform für Xbox kann Turnier Organisatoren (TOs) zum Erstellen und Ausführen von Turniere für Titel, die Arena zu unterstützen. Spielbereich unterstützt sowohl eine Xbox Live mit dem Verleger und dem Benutzer-Run Turniere ermöglicht, als auch von Drittanbietern: Themen zur Vorgehensweise, die in der Arena integrieren. Das Format des Turniers, z. B. einzelne Eliminierung von Duplikaten oder die Schweiz, richtet sich nach an.

Dieses Thema führt Sie als Entwickler Titel durch die Unterstützung für Bereich in der Titelleiste Ihres implementieren. Nachdem ein Spiel Spielbereich aktiviert ist, funktioniert es mit jedem unterstützten Turnier Format ausgewählt, indem an. SO organisiert Übereinstimmungen für den Titel gemäß der Struktur der einzelnen turnierteilnehmer an. Der Spielbereich-fähigen Titel klicken Sie dann die richtigen Benutzer in der jede Übereinstimmung innerhalb des Spiels platziert und meldet die übereinstimmenden Ergebnisse zurück an.

## <a name="overview"></a>Übersicht

Xbox-Spielbereich verwendet Konzepte, die Xbox Multiplayer-Spiele-Entwicklung vertraut sind. Wenn Sie nicht mit dem Xbox-multiplayer-System vertraut sind, empfehlen wir, dass Sie, bevor Sie fortfahren, die Dokumentation überprüfen. Der Prozess ist vergleichbar mit der ein Benutzer eine Einladung in eines Multiplayer-Spiels akzeptiert. Wenn es Zeit für einen Benutzer aus, um eine Übereinstimmung in einen Spielbereich-Turniers wiederzugeben, wird ein Popup angezeigt, um den Benutzer darüber zu informieren. Akzeptieren den Toast bewirkt, dass dem Titel des normalen Spielbereich Protokoll Activation-Ereignis zu empfangen. Wenn Sie der Titel nicht bereits ausgeführt wird, wird sie zu diesem Zeitpunkt gestartet.

Die Turnier Übereinstimmung selbst wird mithilfe einer Sitzungs Multiplayer-Sitzung-Verzeichnis (MPSD) koordiniert. Im Fall von Arena wird die Sitzung von der Plattform Spielbereich statt nach Ihrem Titel erstellt. Dies erfolgt mithilfe einer sitzungsvorlage und zusätzliche Spielkonfiguration, die von Ihnen erstellt wurde, wie der Title-Verleger. Teilnehmer Turnier Übereinstimmung werden die Sitzung bereits hinzugefügt wurde. Wie der Verleger Titel müssen Sie die sitzungsvorlage für Spielbereich zur Unterstützung der Vermittlung für Ergebnisse, die berichterstellung konfigurieren. Die vollständigen Anforderungen für die Sitzung sind weiter unten in diesem Dokument enthalten. Titel des kann auch zusätzlichen Sitzungen zu erstellen, die es benötigt.

Titel des verwendet die Sitzung, um die Ergebnisse Übereinstimmung und Bericht einzurichten. Reagieren auf protokollaktivierung und Interaktion mit der Sitzung MPSD ist die minimale Ebene der Integration von Arena Titel erforderlich. Durchsuchen und die Registrierung für Turniere wird über die Benutzeroberfläche der Xbox-Bereich unterstützt. Optional können Titel zusätzliche Informationen über das Turnier aus den Turniere Hub-Dienst ermöglicht umfangreichere im Titel abrufen. Z. B. mit dem Hub Turniere, kann Ihre Titel Kontext wie z. B. den Namen des Teams angezeigt, und entdecken neue Übereinstimmung Sitzungen für einen Benutzer. Dieser Datenfluss wird durch die grüne Pfeile in der folgenden Abbildung dargestellt, und es wird ausführlich beschrieben die [auftreten, Anforderungen und best Practices](#experience-requirements-and-best-practices) Abschnitt. Die ausgeblendeten Pfeile zeigen, dass der Verweis vom Team für die Sitzung im Laufe der Zeit ändert, wenn der Benutzer von Übereinstimmung, Übereinstimmung innerhalb der einzelnen turnierteilnehmer bewegt.

![](../../images/arena/tournament-flow.png)


Der Spielbereich protokollaktivierung URI enthält die Informationen der einzelnen turnierteilnehmer der Sitzung für die Übereinstimmung und einen deep-Link, den Titel des aufrufen kann, wenn sich die Übereinstimmung befindet. Der deep-Link gibt den Benutzer auf der Xbox Spielbereich-Benutzeroberfläche zurück. Diese URI-Komponenten werden ausführlicher beschrieben die [Protokoll Aktivierung](#1protocol-activation) Abschnitt [grundlegende Anforderungen für die Integration von Arena](#basic-requirements-for-arena-integration).

## <a name="basic-requirements-for-arena-integration"></a>Grundlegende Anforderungen für die Integration von Arena

Dieser Abschnitt enthält technische Anleitungen und Informationen für die Integration Ihrer Titels in die Mindestanforderungen für die Unterstützung der Spielbereich. Diese Art der Integration nutzt den Datenfluss durch die orangefarbene Pfeile in der folgenden Übersicht über die Abbildung dargestellt.

![](../../images/arena/arena-data-flow.png)

Der Titel ist-Protokoll aktiviert werden, über die Benutzeroberfläche der Xbox-Bereich. Dies kann aus einer Toast-Benachrichtigung, auf der Detailseite für das Turnier oder jedem anderen Einstiegspunkt für die Übereinstimmung stammen. Die Schritte, die auftreten, wenn eine Übereinstimmung ein Benutzer beteiligt sind:

1.  Der Titel ist-Protokoll aktiviert werden, von der Xbox-Benutzeroberfläche einnehmen.
2.  Der Titel Aktivierungsparametern der zum Wiedergeben einer einzelnen Übereinstimmung sucht.
3.  Wenn sich die Übereinstimmung befindet, Analyseergebnisse der Titel die für die Spiele MPSD-Sitzung.
4.  Titel des erhält der Benutzer der Möglichkeit, die an der Spielbereich Benutzeroberfläche zurückzugeben.

In den folgenden Abschnitten wechseln Sie über die einzelnen Schritte im Detail.

### <a name="1--protocol-activation"></a>1.  Protokollaktivierung

Die Benutzeroberfläche der Spielbereich startet Dinge von Protokoll aktivierende Titel des Wenn eine Übereinstimmung für den Benutzer bereit ist. Es gibt zwei Fälle: der Titel zu diesem Zeitpunkt gestartet werden kann, oder es möglicherweise bereits ausgeführt. Beide Fälle sind ähnlich, was geschieht, wenn Sie ein Titel, die als Reaktion auf ein Benutzer eine Einladung zu einem Multiplayer-Spiel akzeptiert aktiviert ist.

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>Wenn Ihre Titel zum Zeitpunkt der protokollaktivierung gestartet wird

Die **Activated** Ereignis ausgelöst wird, zum ersten Mal, wenn der Titel des gestartet wird. Ist diese erste Aktivierung einen Spielbereich protokollaktivierung, wurde der Titel vom ein Benutzer versucht, für die Wiedergabe in ein Turnier gestartet. Haben Sie den Titel, so schnell wie möglich auf die Übereinstimmung, umgehen im Menü, und melden Sie sich-Bildschirme zu überspringen. Die Xbox-Benutzer-ID (XUID) von den beteiligten Benutzer wird bei der Aktivierung URI bereitgestellt und sollte bereits angemeldet werden.

#### <a name="if-your-title-is-already-running"></a>Wenn Ihre Titel bereits ausgeführt wird

Die **Activated** Ereignis kann auch mit einer Spielbereich protokollaktivierung ausgelöst, während Ihre Titel bereits ausgeführt wird. Die **Aktivierung** Ereignis wird ausgelöst, nur durch eine explizite Benutzeraktion (, die auf ein Popup, der die Übereinstimmung angibt, oder der Beginn der Übereinstimmung über die Benutzeroberfläche der Xbox-Bereich). Wenn Sie der Titel in einem Menü Bildschirm gewartet hat, haben Sie es sofort auf die Turnier Übereinstimmung zu wechseln. Der Titel der protokollaktivierung während Spiel erhält, müssen Sie es dem Benutzer bieten die Möglichkeit, lassen das Spiel, und geben die Turnier Übereinstimmung bei der zweckmäßigste möglich. Weisen Sie dem Benutzer die Möglichkeit, ihr Spiel bei Bedarf zu speichern.

Protokoll-Aktivierungen werden Ihre Titel über übermittelt die `CoreApplicationView.Activated` Ereignis. Wenn die `IActivatedEventArgs.Kind` der Ereignisargumente-Eigenschaftensatz auf `Protocol`, die Aktivierung ist eine protokollaktivierung und die Argumente umgewandelt werden können, um die `ProtocolActivatedEventArgs` -Klasse, der protokollaktivierung URI, in denen über ist die `Uri` Eigenschaft.

Haben Sie den Titel der XUID auf der protokollaktivierung URI zu überprüfen. Wenn sie den aktuellen Spieler nicht übereinstimmen, müssen Ihre Titel auch Benutzerkontexten wechseln.

#### <a name="the-protocol-activation-uri"></a>Der URI-Protokoll-Aktivierung

Die Vorlage für den URI ist:

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

Für den Titel der Zeitraum in der Konsole ist das Schema für die Aktivierung geringfügig:

```
ms-xbl-{titleIdHex}://
```

Dies ist identisch mit dem Einladungen. Zu diesem Zweck muss die Title-ID acht hexadezimale Zeichen sein, daher führende Nullen, ggf. enthalten werden.

Stellen Sie zunächst sicher, dass die `Uri.Host` (der Name, der unmittelbar nach dem Trennzeichen "://") ist "Turnier". Das ist wie Arena Protokoll Aktivierungen von Aktivierungen anderer Funktionen, z. B. Spiele Einladungen unterschieden werden.

Die Abfragezeichenfolgenargumente werden URL-codiert und wird Groß-/Kleinschreibung. Titel des sollte nicht der Reihenfolge der Parameter abhängig sind, und es unbekannte Parameter ignorieren soll.


Paramater(s) | Beschreibung
--- | ---
**action** | Die einzige unterstützte Aktion ist "JoinGame". Wenn die **Aktion** ist nicht vorhanden oder ein anderer Wert angegeben ist, ignorieren Sie die Aktivierung.
**joinerXuid** | Die **JoinerXuid** der XUID des angemeldeten Benutzers versucht, eine Übereinstimmung Turnier spielen wird. Titel des muss zulassen, zum dieses Benutzers Kontext zu wechseln. Wenn die **JoinerXuid** ist nicht signiert, Ihre Titel muss der Benutzer aufgefordert, dies tun, indem die Kontoauswahl nachsehen. XUIDs werden immer im Dezimalformat angegeben.
**organizerId, tournamentId** | Die **OrganizerId** und **TournamentId**, wenn kombiniert, die Form der eindeutige Bezeichner für das Turnier, die die Übereinstimmung zugeordnet ist. Verwenden Sie diesen Bezeichner zum Abrufen von ausführliche Informationen zu der einzelnen turnierteilnehmer aus dem Hub Turniere, möchten Sie es in Ihrem Titel angezeigt.
**teamId** | Die **TeamId** ist ein eindeutiger Bezeichner für das Team im Kontext des Tourniers teilnehmen, die der Benutzer (gemäß der **JoinerXuid** Parameter) ist ein Mitglied. Wie die **OrganizerId** und **TournamentId** Parameter können Sie die **TeamId** optional Abrufen von Informationen über das Team aus dem Hub Turniere.
**scid, templateName, name** | Zusammen identifizieren diese Sitzung. Dies sind die gleichen drei Parameter im MPSD URI-Pfad für die Sitzung:</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`.</br></br>In XSAPI, sind die drei Parameter für die `multiplayer_session_reference `Konstruktor.
**returnUri, returnPfn** | Die **ReturnUri** ist ein URI-Protokoll-Aktivierung den Benutzer auf der Xbox Spielbereich Benutzeroberfläche zurückkehren. Die **ReturnPfn** Parameter kann oder nicht vorhanden sein. Wenn es sich handelt, ist es die Product Family Name (PFN) für die app, die dafür vorgesehen ist, behandelt der **ReturnUri**. Beispielcode, der zeigt, wie Sie diese Werte verwenden, finden Sie unter [zurückgeben, die auf der Xbox Spielbereich Benutzeroberfläche](#4returning-to-the-xbox-arena-ui).

### <a name="2--playing-the-match"></a>2.  Wiedergabe der Übereinstimmung

Wenn die Sitzung MPSD vom Organisator-Turniers erstellt wird, wird der Benutzer als Mitglied der Sitzung inaktiv festgelegt. Titel des muss sofort den Spieler auf aktiv festlegen, mit `multiplayer_session::join()`. Dies weist darauf hin, Xbox Live und anderen Benutzern in der Übereinstimmung, die der Benutzer in Ihrem Titel und wiedergegeben werden können.

Die Startzeit für die Übereinstimmung wird in der Sitzung zu `/servers/arbitration/constants/system/startTime` als DateTime-Wert in das standard-UTC-Format (z.B. "2009-06-15T13:45:30.0900000Z"). (Startzeit steht auch im XSAPI als `multiplayer_session_arbitration_server::arbitration_start_time()`ab, in der xdk-Version 1703). So kann die Sitzung so weit im voraus die Startzeit erstellen werden sollen (einschließlich gleichzeitig mit der Startzeit). Übereinstimmung Benachrichtigungen werden an die Teilnehmer Übereinstimmung zur Startzeit ausgeführt, gesendet, damit der Titel nicht vor der Startzeit aktiviert werden. Die Startzeit ist auch die früheste Uhrzeit, an dem MPSD Ergebnisse gemeldet werden für die Übereinstimmung werden kann.

Titel des untersucht jedes Mitglieds `/member/{index}/constants/system/team` Eigenschaft (`multiplayer_session_member::team_id()`) um zu ermitteln, ist das team, dass jeder Benutzer.

Titel des verwendet auch die Sitzung zum Bestimmen der Match-Konfigurationseinstellungen, wie Map und den Modus an. Dieser Titel-spezifische Einstellungen können in der sitzungsvorlage oder als Teil der Definition des Turniers, als benutzerdefinierte Konstanten festgelegt werden. (Weitere Informationen finden Sie unter [konfigurieren einen Titel für Spielbereich](#configuring-a-title-for-arena).) Hier ist ein Beispiel:

```json
{
    "constants": {
        "custom": {
            "enableCheats": false,
            "bestOf": 3,
            "map": "winter-fall",
            "mode": "capture-the-throne"
        }
    }
}
```

Sie können diese Einstellungen aus der Sitzung als JavaScript Object Notation (JSON)-Objekt abrufen, indem Sie mit der `multiplayer_session_constants::session_custom_constants_json()` API.

Im Allgemeinen sollten Ihrem Titel behandeln die Spielbereich-Sitzung die gleiche Weise, wie sie ihre eigene Sitzung MPSD behandeln würde. Beispielsweise können sie Handles zu erstellen und RTA-Benachrichtigungen abonnieren. Es gibt jedoch einige Unterschiede. Z. B. der Titel ändern Sie die Liste der game-Sitzung nicht, oder verwenden Sie die Quality of Service (QoS)-Funktionen der Sitzung, und es muss die Vermittlung teilnehmen. Die vollständigen Details der Sitzung werden weiter unten in diesem Dokument bereitgestellt.

### <a name="3--reporting-results"></a>3.  Ergebnisbericht

Die Ergebnisse der Übereinstimmung werden wieder Spielbereich und an über die Sitzung ein Feature namens Vermittlung mit gemeldet. Vermittlung ist ein Framework für die Verwendung einer Sitzungs sicher ein Übereinstimmung und einen Bericht ein Ergebnis zu spielen.

Die Sitzung bereitgestellt, um Ihre Titel in der protokollaktivierung Schritt werden eine arbitrated-Sitzung, was bedeutet, dass es sich um einen festen Zeitplan verfügt, den das Framework für die Vermittlung erzwingt. Dieses Diagramm zeigt die Zeitachse für die Vermittlung.

![](../../images/arena/arbitration-timeline.png)

Mindestens ein einziger Player muss in der Sitzung vor dem Zeitpunkt der einbehaltene, die die Startzeit ist aktiv (`multiplayer_session_arbitration_server::arbitration_start_time()`) sowie das Timeout einbehaltene. Das Timeout einbehaltene ist in der Sitzung als Anzahl von Millisekunden auf `/constants/system/arbitration/forfeitTimeout` (`multiplayer_session_constants::forfeit_timeout()`). Wenn niemand die Sitzung als aktiv vor dem Zeitpunkt der einbehaltene verknüpft ist, wird die Übereinstimmung abgebrochen.

Das Timeout für die Vermittlung wird in Millisekunden an `/constants/system/arbitration/arbitrationTimeout` (`multiplayer_session_constants::arbitration_timeout()`). Vermittlung Zeit ist die Startzeit und der Wert der Timeout für die Vermittlung, und stellt die Zeit dar, bei der die Spieler abgeschlossen Übereinstimmung und Ergebnisse gemeldet haben müssen. Dieser Wert wird vom Herausgeber in der Vorlage "Sitzung" oder ein Spielmodus festgelegt. Legen Sie sie so viel Zeit wie nötig ab, der Titel für die Übereinstimmung abgeschlossen werden können.

Titel des kann die Ergebnisse zu einem beliebigen Zeitpunkt zwischen der Startzeit und die Uhrzeit für die Vermittlung melden. Vermittlung tritt auf, zu einem beliebigen Zeitpunkt zwischen dem einbehaltene und dem Zeitpunkt der Vermittlung nach jeder aktives Mitglied der Sitzung Ergebnisse übermittelt hat. Z. B. wenn nur ein Element in der Sitzung zum einbehaltene Zeitpunkt aktiv ist, sie können (und sollten) veröffentlichen wird ein Ergebnis und Vermittlung erfolgt. Unabhängig davon, wie viele Ergebnisse zur Vermittlung Zeit verfügbar sind treten Vermittlung, wenn dies noch nicht der Fall. Wenn keine Ergebnisse gesendet werden, bei der Vermittlung erreicht wird, werden alle Teilnehmer in der Übereinstimmung Verlust angegeben.

Es ist auch möglich, ein Spiel Server für die Vermittlung zu einem beliebigen Zeitpunkt erzwingen, indem Sie ein arbitrated Ergebnis schreiben.

Wenn ein Benutzer in einer Sitzung ist, die bereits arbitrated wurde wurde (da entweder die Vermittlung Timeout abgelaufen ist, ein Gameserver vermittelt die Sitzung oder der Benutzer später hinzugefügt), der Titel, endet die Übereinstimmung, und das arbitrated Ergebnis für den Benutzer angezeigt.

Vermittlung Ergebnisse enthalten immer das Ergebnis für jedes Team. Wenn ein einzelner Player ein Ergebnis in der Sitzung schreibt, wird es nicht nur ihrer Teams Ergebnis enthält, aber der vollständige Satz von Ergebnissen für jedes Team.

Die Ergebnisse im JSON-Format wird wie folgt aussehen.

```json
"results": {
    "red": {
        "outcome": "rank",
        "ranking": 1
    },
    "blue": {
        "outcome": "rank",
        "ranking": 2
    }
}
```

In diesem Beispiel werden das Team-IDs "Rot" und "blue". Das red Team kam Sekunde in die erste und das blue Team. Weitere mögliche Werte für **Ergebnis** sind:

* "Win"
* "Ausfall"
* "Zeichnen"
* "Noshow"

Wenn **Ergebnis** ist alles andere als "Rank", die Rangfolge für das Team fehlt.

Einzelne Benutzer schreiben, die übereinstimmenden Ergebnisse, `/members/{index}/properties/system/arbitration` (`multiplayer_session::set_current_user_member_arbitration_results()`). Das folgende Beispiel veranschaulicht, wie möglicherweise einen Titel erstellen und senden eine Reihe von Ergebnissen, vorausgesetzt, dass der Titel des Team-basierte ist nicht (d. h. alle Spieler in Teams, die von einem sind).

```c++
void Sample::SubmitResultsForArbitration()
{
    std::unordered_map<string_t, tournament_team_result> results;

    for (auto& member : arbitratedSession->members())
    {
        tournament_team_result teamResult;
        teamResult.set_state(tournament_game_result_state::rank);
        teamResult.set_ranking(memberRank);

        results.insert(std::pair<string_t, tournament_team_result>(
            member->team_id(),
            teamResult));
    }

    arbitratedSession->set_current_user_member_arbitration_results(results);
    xboxLiveContext->multiplayer_service().write_session(
arbitratedSession,
multiplayer_session_write_mode::update_existing)
    .then([](xbox_live_result<std::shared_ptr<multiplayer_session>> sessionResult)
    {
        if (sessionResult.err())
        {
            // Handle error.
        }
        else
        {
            // Update local session cache.
        }
    });
}
```

Nach der Vermittlung wird platziert MPSD die Endergebnisse in `/servers/arbitration/properties/system` (`multiplayer_session::arbitration_server()`), zusammen mit einigen anderen Eigenschaften, wie hier gezeigt.

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

Möglichkeiten für **ResultState** enthalten:

* "abgeschlossen": Alle aktiven Spieler hat ein Ergebnis gemeldet.
* "Partialresults": Einige, aber nicht alle aktive Spieler hat ein Ergebnis vor dem Timeout für die Vermittlung gemeldet.
* "Noresults": Keines der aktive Spieler hat ein Ergebnis vor dem Timeout für die Vermittlung gemeldet.
* "abgebrochen": Keine aktive Spieler bei, die vor dem Timeout einbehaltene eingetroffen sind.

Im Fall von "Noresults" und "abgebrochen" werden die Ergebnisse ausgelassen.

Die **ResultSource** "verteilen" ist, wenn MPSD der Vermittlung, basierend auf den Ergebnissen gemeldet von einzelnen Playern ausführt. Ein Gameserver kann in /servers/arbitration/properties/system selbst, umgehen die Vermittlung schreiben. In diesem Fall gibt es eine **ResultSource** von "Server". Es ist auch möglich, für einen Gameserver, neu zu schreiben (d. h. zu beheben oder angepasst) die Ergebnisse, in dem Sie die Schreibweise legt **ResultSource** "angepasst".

Die **ResultConfidenceLevel** ist eine ganze Zahl zwischen 0 und 100, der die Ebene der Konsens im Ergebnis angibt. 100 bedeutet, dass alle Beteiligten akzeptieren. 0 bedeutet, dass gab es keine Konsens und ein Ergebnis wurde nach dem Zufallsprinzip im Wesentlichen von den übermittelt. Wenn Sie ein Gameserver die Vermittlung Ergebnisse festlegt, wird die **ResultConfidenceLevel**-in der Regel auf 100 erhöht, es sei denn, Sie aus irgendeinem Grund sogar die Spiele-Server selbst ist nicht sicher (z. B., wenn sie die Ergebnisse der eigenen pro-Player meldet Abstimmungsverfahren). Legen Sie die **ResultConfidenceLevel** auch wenn Ergebnisse das Vertrauen in die angepassten Ergebnisse entsprechend angepasst werden.

Wenn die **ResultSource** "verteilen" ist (und die **ResultState** ist "completed" oder "Partialresults"), die Ergebnisse sind eine exakte Kopie mindestens einer der Spieler gemeldeten Ergebnisse.

### <a name="4--returning-to-the-xbox-arena-ui"></a>4.  Klicken Sie auf der Xbox-Spielbereich Benutzeroberfläche zurückgibt

Wenn die Übereinstimmung über (oder ggf. als Antwort auf Anforderung des Spielers, verwerfen Sie die Übereinstimmung in Bearbeitung) ist, stellen Sie eine Option für den Player, die zurück an die Benutzeroberfläche der Xbox-Bereich aus, in dem sie die Übereinstimmung verknüpft. Dies kann von einem nach der übereinstimmenden Ergebnisse-Bildschirm oder jede andere Ende-des-Game-Benutzeroberfläche erfolgen.

Um auf die Benutzeroberfläche der Spielbereich zurückzugeben, rufen Sie die **ReturnUri** , die übergeben wurde aus dem URI-Protokoll-Aktivierung durch Verwendung der `Windows::System::Launcher` Klasse. Achten Sie darauf, dass den Benutzerkontext enthalten.

Die Start-API wird etwas anders als Zeitraum Spiele als universelle Windows-Plattform (UWP) Spiele verfügbar gemacht. Der Zeitraum Version der API nicht den PFN bereitgestellt werden, nicht zulassen, damit es sich bei der PFN sogar ignoriert werden kann auf der die Aktivierung URI vorhanden.

Hier ist Beispielcode für einen Zeitabschnitt vornehmen, die Benutzer an der Spielbereich Benutzeroberfläche zurückkehren.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

UWP-Spiele können festlegen, die **TargetApplicationPackageFamilyName** Eigenschaft **LauncherOptions** auf den return PFN, wenn eine auf der protokollaktivierung URI angegeben wurde. Damit wird sichergestellt, dass die bestimmte app gestartet wird und der Benutzer auf den Store durchgeführt, wenn die app nicht bereits installiert ist.

Hier ist Beispielcode für eine UWP-app den Benutzer an der Spielbereich Benutzeroberfläche zurückkehren.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, String ^returnPfn, User ^currentUser)
{
    auto options = ref new LauncherOptions();

    if (returnPfn != nullptr)
    {
        options->TargetApplicationPackageFamilyName = returnPfn;
    }

    Launcher::LaunchUriForUserAsync(currentUser, returnUri, options);
}
```

Nach dem Aufrufen der Spielbereich-Benutzeroberfläche, Ihre Titel weiter ausgeführt, und möglicherweise auf diese einzelnen Bildschirm anzeigen und ein anderes Protokoll Aktivierungsereignis warten. Auf diese Weise verfügt der Spieler eine weitere Übereinstimmung zu spielen, ist dem Titel des einsatzbereit. Dies beschleunigt die benutzerfreundlichkeit der Spieler von Übereinstimmung, Übereinstimmung wird zwischen Ihrem Titel und die Xbox Spielbereich Benutzeroberfläche wechseln.

### <a name="handling-errors-and-edge-cases"></a>Behandeln von Fehlern und Grenzfälle

Ablauf für das im vorherigen Abschnitt wird beschrieben, die golden Pfad durch die Wiedergabe einer Übereinstimmung. Es gibt viele Stellen, in denen möglicherweise unerwartetes Verhalten angezeigt oder können Probleme auftreten. In diesem Abschnitt werden einige mögliche Fehler oder Grenzfälle, und bietet Empfehlungen für den Umgang damit in Ihrem Titel.

#### <a name="game-session-not-found"></a>Spiele-Sitzung nicht gefunden.

Wenn Ihre Titel, mit einer Ref-Sitzung nach einer Übereinstimmung gestartet wird und der Client nicht klicken Sie dann eine Fehlermeldung erhalten, wenn Sie diese Sitzung von MPSD anfordern, stellen Sie einen Fehler an den Benutzer, der angibt, dass sie nicht die Übereinstimmung verknüpfen können.

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>Benutzer versucht, eine Übereinstimmung zu verknüpfen, die wiedergegeben wurde

Wenn ein Benutzer versucht, eine Übereinstimmung zu verknüpfen, nachdem die Ergebnisse gemeldet wurden, Sperren Sie diesen Client starten Sie eine neue Übereinstimmung und anzeigen Sie der mit einem Fehler. Sie können überprüfen, ob die Ergebnisse gemeldet wurden durch Durchlaufen der Elemente in der Sitzung angezeigt werden, bei denen eine `/members/{index}/arbitrationStatus` (`multiplayer_session_member::tournament_arbitration_status`) auf "complete" festgelegt ist.

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>Spielclients kann eine p2p-Verbindung hergestellt

Wenn Ihre Titel zwischen Benutzern (p2p)-Verbindungen zwischen Clients für Multiplayer-Spiele verwendet und keine Unterstützung für Relays, möglicherweise Instanzen, in dem Benutzer, die miteinander verglichen werden keine p2p-Verbindung bilden können.

Wenn der Client kann zum Abrufen der Sitzungs für die Übereinstimmung aber kann nicht für andere Clients verbinden, melden ein "Wort Zeichnen" Ergebnis für jedes Team unter `/members/{index}/properties/system/arbitration/results` (`multiplayer_session::set_current_user_member_arbitration_results()`). Dies zeigt Xbox Live, dass die Übereinstimmung nicht stattgefunden hat. Zudem verhindert es Exploits, in denen Benutzer über ein Turnier fahren Sie fort, können durch einen p2p-Verbindungsfehler zu erzwingen.

#### <a name="game-client-disconnects-mid-match"></a>Spiele Client trennt mittlere Übereinstimmung

Eines der häufigsten Fehler Szenarios ist, wenn Clients getrennt, während eine Übereinstimmung wiedergegeben wird. Abhängig vom Status der Übereinstimmung und Ihre Implementierung gibt es einige Optionen zur Behandlung dieses Falls:

* Wenn die Turnier einer im Vergleich zu einer entspricht, oder wenn alle Mitglieder eines Teams von den anderen Clients und/oder Ihrem Spiel Dienst trennen möchten, wir empfehlen, Bericht, den Sie "Verlust" für das Team, das nicht mehr verbunden ist. Dies verhindert, dass den Fall, in dem Benutzer eine Trennung der Verbindung aus einer Übereinstimmung erzwingen können, die sie verlieren, um zu verhindern, dass das Ergebnis gemeldet wird.
* Wenn die Übereinstimmung zwischen den Teams mit mehreren Elementen, und eine Teilmenge der Clients für eine Teamprojektsammlung trennen Mid Übereinstimmung, können Sie auswählen, zum Ende des Abgleichs an diesem Punkt oder erlauben, dass Sie die restlichen Benutzer fortzusetzen. Wenn Sie die Übereinstimmung fortfahren möchten, können Sie optional auch Benutzer, der die Übereinstimmung erneut beitreten getrennt.

#### <a name="full-teams-not-present"></a>Vollständige Teams, die nicht vorhanden

Wenn Ihre Titel Übereinstimmungen unterstützt mit Teams von mindestens zwei Fälle, in dem einige Mitglieder eines Teams starten Sie den Titel für die Übereinstimmung nicht, auftreten können. Sie können in diesem Fall können die Mitglieder des Teams wiedergeben, ohne Sie zu ihrem Team-Element, und optional zulassen, dass dieses Teammitglieds, um die Übereinstimmung in Bearbeitung zu verknüpfen.

Alternativ können Sie automatisch ein Ergebnis erzwingen. Wenn Sie dies tun, warten Sie, bis die ForfeitTimeout-Zeit, um alle Teilnehmer Möglichkeit um die Übereinstimmung zu verknüpfen, bevor Sie erzwingen, dass das Ergebnis. Dadurch können Sie das Fenster mit den Änderungen an den Spielmodus für das Turnier statt Aktualisieren des Titels anzupassen.

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>Die Länge der Übereinstimmung überschreitet arbitrationTimeout

Legen Sie den Wert für **ArbitrationTimeout** eine Übereinstimmung möglicherweise dauert größer als die maximale Länge der Zeit sein. Davon abgesehen kann der Titel feature Modi bei der die mögliche Übereinstimmung-Länge sind sehr lange, oder in denen kein Maximum vorhanden ist. In diesen Fällen sollten Sie entweder Sie:

* Einschränkende Turnier Play nur Modi mit festen, maximale Länge der Zeit.
* Informiert den Benutzer, der das Zeitlimit und berichterstellung ein Ergebnis, bevor Sie die **ArbitrationTimeout** basierend auf einer in-Match-Regel ins Spiel kommt.

Da **ArbitrationTimeout** Ablauf löst ein automatisches Ergebnis für die Übereinstimmung, warten Sie nicht die vollständige **ArbitrationTimeout** Zeitraum, bevor gemeldet wird, ein Ergebnis aus dem Titel.  Stattdessen erstellen Sie in einem Zeitpuffer (z. B. **ArbitrationTimeout** -1.000 ms), oder verwenden Sie einen separaten Wert, um die maximale Übereinstimmung Länge steuern.

#### <a name="other-cases"></a>Anderen Fällen

Die allgemeine Empfehlung werden für alle anderen Fehler Fällen aus informiert den Benutzer über den Fehler, und geben Sie den Benutzer an der Benutzeroberfläche der Xbox-Bereich zurück.

## <a name="experience-requirements-and-best-practices"></a>Benutzerfreundlichkeit Anforderungen und best practices

Da Spielbereich Titel des immer nur mit einzelnen Übereinstimmungen enthält, können neue Formate im Laufe der Zeit eingeführt werden, ohne Updates für den Titel. Aus diesem Grund formatiert die Baseline-benutzererfahrung für einen Spielbereich integrierte Titel einfach und flexibel genug, um die Arbeit mit mehreren Wettbewerb sein muss.

Optional können können Daten aus dem Hub-Turniers im Titel Sie Turniere einen integrierten Bestandteil der in-Game-Umgebung vornehmen.  Diese Funktion finden Sie unter "Xbox::services::tournaments".  Mithilfe dieser APIs können Sie:
* Surface-Turniers Details im Titel UI (Name des Turniers, Teamname usw.).
* Geben Sie die Ermittlung für Turniere in Ihrem Titel
* Behalten Sie die Benutzer in Ihrem Titel zwischen Spielbereich Übereinstimmungen

## <a name="configuring-a-title-for-arena"></a>Konfigurieren einen Titel für Bereich

Um einen Titel für den Bereich zu aktivieren, einige zusätzliche Schritte sind erforderlich, wenn Sie ihn in die Xbox-Entwickler-Portal (XDP) konfigurieren oder [Partner Center](https://partner.microsoft.com/dashboard).

### <a name="enabling-arena-for-your-title"></a>Aktivieren von Arena für den Titel

Um den Bereich zu aktivieren, wechseln Sie zur Konfigurationsseite für Ihre Titel im XDP Dienst aus, und wählen Sie "Bereich".

![](../../images/arena/arena-configure-xdp.png)

Hier haben Sie mehrere Optionen:

* **Spielbereich aktiviert** – aktivieren Sie dieses Kontrollkästchen, um Spielbereich für diese Sandbox zu aktivieren.
* **Features der Spielbereich** – dieser Abschnitt enthält Kontrollkästchen benutzergeneriertem Turniere in der Sandbox zu aktivieren und cross-Wiedergabe, zu aktivieren, die Benutzer auf mehreren Plattformen die gleichen Turniere teilnehmen zu können.
* **Spielbereich Plattformen** – Sie können die Plattformen, die auf dem Turniere wiedergegeben werden können für den Titel auswählen.
* **Turnier Assets** – (früher in der "Multiplayer-Spiele und Vermittlung"-Abschnitt.) Hierbei handelt es sich um die Turnier-Images für den Titel.

Spielbereich kann ebenfalls aktiviert werden, im Partner Center in der **Turnier** Menü unter dem Xbox Live-Dienst.

![Menü "Bereich" im Partner Center](../../images/arena/Arena_On_WDC.JPG)

Sie müssen die Dienstkonfiguration für die Änderungen wirksam werden veröffentlichen. Spielbereich-Self-service-Konfiguration wird durch UDC derzeit nicht unterstützt. Wenn Sie UDC für Dienstkonfiguration verwenden, arbeiten Sie mit Ihrer Entwicklung Account Manager für die Integration mit Bereich.

### <a name="setting-up-game-modes"></a>Einrichten von Spielmodi

Spielmodi werden eine Xbox Live-Feature, mit den Verleger, um die Einstellungen für eine Übereinstimmung Turnier vorkonfigurieren können. Diese Spielmodi werden bei der Erstellung des Turniers zum Festlegen von der Regeln von Xbox Live und Ihre Titel für das Turnier erzwungen. Wie der Verleger Titel benötigen Sie mindestens einen Spielmodus benutzergeneriertem Turniere für den Titel zu aktivieren. Elemente, die von den Spielmodus enthalten:

#### <a name="supports-ugt"></a>Unterstützt UGT?

Spielmodi können für die Verwendung mit vom Benutzer generierte Turniere (UGTs) aktiviert werden. Wenn Sie der Titel für die Unterstützung von UGT konfiguriert ist, können Sie Spielmodi markieren, damit sie beim Erstellen von Turniere Titel Ihres Xbox Live-Benutzer ausgewählt werden können.

#### <a name="team-size"></a>Teamgröße

Dies ist die Anzahl der Benutzer, die der einzelnen turnierteilnehmer als Team teilnehmen kann. Für einen Einzelbenutzer-Titel oder einen-Turniers hat den Spielmodus ein Team dem Wert 1. Spielbereich unterstützt derzeit keine Variable teamgrößen für Turniere.

#### <a name="forfeittimeout"></a>forfeitTimeout

Dies ist die Anzahl der Millisekunden, nach die Übereinstimmung Startzeit, bevor die Übereinstimmung abgebrochen wird, wenn keine Spieler anzeigen für sie (d.h., wenn keine Spieler selbst, in der Sitzung aktiv festlegen). Legen Sie diese Option, um eine Zeitspanne, die mindestens groß genug ist, für einen Player, starten Sie den Titel und die Übereinstimmung Turnier ab dem Zeitpunkt, die sie sehen, dass der Toast-Benachrichtigung.

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

Dies ist die Anzahl der Millisekunden, nach die Übereinstimmung Startzeit, nach dem keine weiteren Ergebnisse akzeptiert werden. Legen Sie sie an, um das Timeout einbehaltene sowie einen Zeitraum angeben, die länger als die längste Übereinstimmung möglicherweise dauern. Auf diese Weise auch, wenn der Player wird direkt vor dem Zeitpunkt der einbehaltene experimentieren, haben sie immer noch genügend Zeit für die Übereinstimmung fertig zu stellen. Wenn keine Ergebnisse, mit der Zeit von gemeldet werden der **ArbitrationTimeout**, der Übereinstimmung alle Teilnehmer empfangen Verlust. Xbox-Spielbereich wartet auch nach oben, bis die **ArbitrationTimeout** für alle aktiven Elemente Ergebnisse vor dem verteilen.

Dieses Timeout dient als ein Sicherheitsnetz, falls etwas schiefgeht, und ein Ergebnis für einen Player nicht gemeldet. Anstatt treten die gesamte-Turniers, setzt dieses Timeout eine Obergrenze für die Menge an Zeit, die der einzelnen turnierteilnehmer gewartet wird. Aus diesem Grund sollte nicht es zu hoch festgelegt werden, da die maximale Länge des einzelnen round-Turniers bestimmt.

#### <a name="name"></a>Name

Dies ist der Name der benutzerseitigen für Spielmodus. Dieser Wert kann lokalisiert werden.

#### <a name="description"></a>Beschreibung

Dies ist die Beschreibung benutzerseitigen Spielmodus. Dieser Wert kann lokalisiert werden.

#### <a name="custom"></a>Benutzerdefiniert

Die **benutzerdefinierte** im Abschnitt für den Spielmodus ein Eigenschaftenbehälter ist, in dem Entwickler Title-spezifische Konfigurationseinstellungen für die Übereinstimmung Turnier einfügen können. Werte, die als Teil der benutzerdefinierten Abschnitt definiert werden geschrieben, auf die Übereinstimmung MPSD Sitzung `/properties/custom/`.

Spielmodus-Setup wird derzeit nicht über XDP oder UDC unterstützt. Wenden Sie sich an Ihren Account Manager, Entwickler, um Spielmodi für den Titel zu erstellen.

### <a name="requirements-for-the-session-template"></a>Anforderungen für die sitzungsvorlage

Als Entwickler Titel, müssen Sie für an für die Verwendung die sitzungsvorlage bereitstellen, beim Erstellen von Sitzungen für Übereinstimmungen. Es gibt in Bezug auf den Inhalt der Vorlage ein.

```json
{
    "version": 1,
    "inviteProtocol": "tournamentGame",
    "memberInitialization": null,

    "capabilities": {
        "gameplay": true,
        "arbitration": true,

        "large": false,
        "broadcast": false,
        "blockBadMsaReputation": false
    },

    "maxMembersCount": {maxMembersCount},
}
```

Andere Eigenschaften, die hier nicht gezeigt, können beliebig festgelegt werden.

Spielbereich Sitzungen sind immer 1-Version. Festlegen der **InviteProtocol** "TournamentGame" ermöglicht die Match-Ready-Benachrichtigungen an Turnier Teilnehmer gesendet werden. **MemberInitialization** QoS deaktivieren auf null festgelegt ist. Die Gaming-Funktion muss festgelegt werden, da die Sitzung, eine Übereinstimmung darstellt, und die Vermittlung-Funktion für Gruppenrichtlinienergebnisse erforderlich ist. Die **große**, **Broadcast**, und **BlockBadMsaReputation** Funktionen müssen deaktiviert sein, da sie den Vorgang der Sitzung beeinträchtigen würde.

Titel des kann eigene Einstellungen in der benutzerdefinierten Abschnitt der Vorlage für die Einstellungen angeben, die einen festen Wert für alle Turniere verfügen, die die Vorlage zu verwenden. Hier ist ein Beispiel.

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

Zum Schluss die **MaxMembersCount** Systemeinstellung ist erforderlich. Legen Sie sie an, auf die Gesamtanzahl der Spieler, die in einer Übereinstimmung Turnier spielen können. Die Anzahl der Spieler Spielmodus Einstellungen kann weiter eingeschränkt werden, also stellen Sie sicher, dass der Wert festgelegt, in der Sitzung die höchste Gesamtanzahl der Spieler unterstützt für den Titel darstellt. Wenn beispielsweise die maximale Anzahl der Spieler, die Ihr Spiel nach einer Übereinstimmung unterstützt 5 Visual Studio ist. Legen Sie 5 **MaxMembersCount** auf 10. Mit dieser Vorlage MPSD kann verwendet werden, um entspricht einer beliebigen Anzahl von Spielern zu unterstützen, bis die **MaxMembersCount**.
