---
title: Bewertung
description: Erfahren Sie, wie Xbox Live Reputation Service verwendet, um positive Spiel zu empfehlen.
ms.assetid: f8966184-5db7-4cab-93ca-9a0250a6077d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Zuverlässigkeit, sozialen Plattform
ms.localizationpriority: medium
ms.openlocfilehash: 4e7763294458f19509d5b797a6fa10e03678abee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641815"
---
# <a name="reputation"></a>Bewertung

Reputation ist eine Benutzer-Statistik, genau wie jede andere, und befindet sich im Aufrufe zum Abrufen aller Statistiken für einen Benutzer und Ihre Verwendung in Berichten und überwachen. Der Ruf selbst wird dargestellt, durch die **Reputation Klasse**. Die **ReputationService Klasse**Reputation Service darstellt. Die entsprechenden URIs werden in beschrieben **Reputation URIs**.

> [!IMPORTANT]
> Reputation Statistiken gelten global für den Dienst nicht an einem bestimmten Titel gebunden werden. Die Dienstkonfiguration ID (SCID) für den Dienst ist die globale SCID verwendet, um die Zuverlässigkeit Statistiken zugreifen.


## <a name="features-of-the-reputation-service"></a>Features des Diensts Reputation

Der Reputation Service:

-   Feedback und Beschwerden behandelt die gleiche Weise. Entitäten Übermitteln von Feedback zu einem Benutzer, und dieses Feedback wirkt sich auf der Benutzer die Bewertung. Das Feedback kann dann an das Team Erzwingung für weitere Aktionen weitergeleitet werden.
-   Ermöglicht Benutzern das Feedback für andere Benutzer zu senden. Titel können Feedback automatisch senden.
-   Können Titel direkten Zugriff auf die API. Ein Benutzer kann Übermitteln von Feedback direkt von innerhalb eines Spiels, als auch von im Rahmen der Spielbereich, in denen der Benutzer aktuell ist.
-   Handles niedrige Ruf hat Auswirkungen auf, welche Benutzer auf der Xbox Live und in Titel werden können. Daher müssen Benutzer auf ihren Ruf und Act besser beim Onlinespiel verfolgen.
-   Positive rückmeldungen ermöglicht, als auch negatives Feedback. Benutzer, die helfen, die Xbox-Community oder des Titels-Community für ihre Arbeit erhalten können, und sie können auch spezielle Berechtigungen erhalten.
-   Leitet eine einzige, allgemeine Ruf, dargestellt durch die **Reputation.OverallReputation Eigenschaft**. Es wird aus den folgenden Reputation abgeleitet:

    -   Fair Play. Dargestellt durch die **Reputation.FairplayReputation Eigenschaft**.
    -   Die Kommunikation. Dargestellt durch die **Reputation.CommunicationsReputation Eigenschaft**.
    -   Vom Benutzer generierte Benutzergenerierten Inhalten. Dargestellt durch die **Reputation.UserGeneratedContentReputation Eigenschaft**.

Finden Sie unter **ResetReputation (JSON)** für Weitere Informationen.


## <a name="usage-flow-for-the-reputation-service"></a>Nutzung der Ablauf für den Bewertungsdienst

Der allgemeine Ablauf für den-zuverlässigkeitsdienst erfolgt in zwei Phasen: reporting Ruf und Vermittlung Reputation gefiltert.


## <a name="reporting-reputation"></a>Ruf Reporting

Wenn genügend negatives Feedback für einen bestimmten Benutzer gemeldet wurde, legt der Titel der **Reputation.OverallReputation Eigenschaft** Ruf für diesen Benutzer (JSON-Attribut OverallReputationIsBad) an. Wenn Zeit ohne Beschwerden, eines Benutzers Reputation langsam verbessert und es ist möglich, dass ein Benutzer mit der nach dem Ruf, einen guten Ruf hat erneut zu erreichen.


## <a name="reputation-filtered-matchmaking"></a>Ruf gefilterte Vermittlung

Für Reputation gefilterte Vermittlung, angegeben durch Xbox-Anforderung (XR) ruft der Titel des Spielers **Reputation.OverallReputation Eigenschaft**. Dieser Wert ist eine Bewertung, die den Status des allgemeine Ruf des Spielers angibt.

> [!NOTE]
> Wenn es sich bei Ihrem Titel sucht das OverallReputationIsBad-Attribut in einer JSON-Datei und nicht finden kann, ist es davon ausgehen, dass der Benutzer einen guten Ruf hat. Dies kann bei neuen Konten geschehen, bis für den Benutzer Feedback verarbeitet wurde. Playern mit Feedback für andere Benutzer weisen immer Reputation Statistiken und einen Wert für die **Reputation.OverallReputation** Eigenschaft.


## <a name="reputation-as-an-indicator-of-behavior"></a>Ruf ein Indikator des Verhaltens

Ruf bietet einen Indikator, der das Verhalten des Benutzers auf dem System. Beispielsweise ist die Person, die einen benutzerfreundlichen Player oder nicht? Feedback von den Mitgliedern der Sitzung bestimmt des Spielers Ruf. Jeder Benutzer hat eine Reputation-Wert, der übermittelt wird, für diese Person überall auf der Xbox One. Es wird an gut sichtbarer Stelle an soziale Netzwerke stellen verfügbar gemacht, die Freunde sehen können, sodass sie Peer-Druck an einen Player besser Verhalten anwenden können. Zum Beispiel:

-   Er befindet sich auf der Profilkarte jedes Benutzers. Jede Person im System kann das Profil des Benutzers mit einem einzigen Klick anzeigen.
-   Es wird in Multiplayer-Spiele angezeigt. Wenn Benutzer zusammen spielen online den Ruf des Spielers mit den niedrigsten Ruf in der Gruppe die Gruppeninhalte Reputation entspricht. Die Gruppe wird nur für andere Benutzer mit ähnlichen Reputation abgeglichen. Weitere Entwicklungen verwenden Ruf um zu entscheiden, welche Art der Spieler in diesem Spiel sind, und entscheiden, ob sie das Spiel hinzufügen möchten.


## <a name="key-elements-of-reputation"></a>Wichtige Elemente der Zuverlässigkeit

Ein Titel kann bestimmen, wenn ein Benutzer einem vermeiden Sie mir für Fair Play, Kommunikation oder Kategorien von Benutzern generierte Benutzergenerierten Inhalten erreicht hat. Der Benutzer hat mich vermeiden Sie für die zugeordnete Kategorie erreicht, wenn eine der folgenden Eigenschaften auf 1 festgelegt ist:

-   **Reputation.OverallReputation Eigenschaft**. Die zugehörige JSON-Attribut ist OverallReputationIsBad.
-   **Reputation.FairplayReputation Eigenschaft**. Die zugehörige JSON-Attribut ist FairplayReputationIsBad.
-   **Reputation.CommunicationsReputation Eigenschaft**. Die zugehörige JSON-Attribut ist CommsReputationIsBad.
-   **Reputation.UserGeneratedContentReputation Property**. Die zugehörige JSON-Attribut ist UserContentReputationIsBad.


## <a name="good-game-reports"></a>Gute Game-Berichte

Zusätzlich zur berichterstellung von Benutzern für fehlerhafte Aktionen, können Benutzer auch miteinander gutes Spiel Berichte, senden die im Titel als stimmen Sie über die wichtigsten Player erstellt werden kann.


## <a name="feedback-history"></a>Feedback-Verlauf

Feedback-Verlauf werden Informationen wie z. B. die Zeit, wenn der Benutzer wurde zuletzt gemeldet, und was die Person, die, gemeldet werden, z. B. "kürzlich Feedback zu kommunikationsstil empfangen."


## <a name="xbox-requirement-for-reputation"></a>Xbox-Anforderung für die Bewertung

Xbox-Anforderung (XR) 068, Vermittlung Filtern Ruf, erfordert die Trennung von Playern mit mit schlechtem Ruf, von denen mit hoher Zuverlässigkeit. Dies erfolgt anhand der **Reputation.OverallReputation** Wert für einen Spieler und des Spielers OverallReputationIsBad-Attribut in Statistiken.

Titel des kann durch Übergabe von "OverallReputation" eines Benutzers Reputation abrufen, an die *StatisticName* Parameter, der die **UserStatisticsService.GetSingleUserStatisticsAsync-Methode (String, String, String)**. Ruft die WinRT-Methode **abrufen (/users/xuid({xuid})/scids/{scid}/stats)** wie in den folgenden JSON-Text dargestellt.

```json
    {
      "requestedusers": [
        "2533274792693551"
      ],
      "requestedscids": [
        {
          "scid": "7492baca-c1b4-440d-a391-b7ef364a8d40",
          "requestedstats": [
            "OverallReputationIsBad",
            "FairplayReputationIsBad",
            "CommsReputationIsBad",
            "UserContentReputationIsBad"
          ]
        }
      ]
    }
```

Wenn eines Benutzers aus dem Feedback von den anderen Spieler ein niedriges Niveau erreicht, wird OverallReputationIsBad für den Benutzer auf 1 festgelegt. Personen, für den **Reputation.OverallReputation** ist 1 übereinstimmen soll nur mit anderen Personen mit **OverallReputation** auf 1 festgelegt. In der Standardeinstellung, wenn Benutzer eine Übereinstimmung, geben Sie sie in der Regel keine Umgang mit Menschen mit nicht vertrauenswürdigen. Titel können optional einen Player einen guten Ruf zu nutzen, mit Low-Reputation Playern übereinstimmen.

Die neueste Version der XRs, die für die Bewertung gelten, finden Sie unter [Xbox Anforderungen](https://developer.xboxlive.com/en-us/platform/certification/requirements/Pages/home.aspx) auf der Game Developer Network (GDN).


## <a name="creating-users-with-bad-overall-reputation-for-testing"></a>Erstellen von Benutzern mit allgemeinen in dem Ruf für Tests

Zum Testen, können Benutzer mit einer sehr schlechten Ruf zum Testen der Übereinstimmung, die Implementierung für einen Titel Filtern eingerichtet werden. Legen Sie einen bestimmten Wert für einen Benutzer, den Test-Titel oder Tool Aufrufe **POST (/users/xuid({xuid})/resetreputation)** eine JSON-Datei, die der Benutzer bestimmte Reputation Werte festlegt. Finden Sie unter **ResetReputation (JSON)** für Weitere Informationen.

Beispielsweise legt der folgende JSON-Text eines Benutzers Fair Play Ruf auf einen sehr schlechten Wert:

```json
    {
      "fairplayReputation": 5,
      "commsReputation": 75,
      "userContentReputation": 75
    }
```

Partner können diesen Aufruf für alle Sandboxes mit Ausnahme von RETAIL vornehmen. Diese Anforderung legt die Spielstände eines Benutzers Basis Ruf und des Spielers Gewichtungen für positive rückmeldungen werden alle werden auf NULL gesetzt werden. Tatsächliche Ruf des Benutzers, nach dem Aufruf besteht aus dieser Basis Bewertungen sowie des Spielers botschafter Bonus sowie des Spielers Follower Bonus. Dieser Mechanismus wird ein Benutzer erstellt, mit der eine niedrige Bewertung und **Reputation.OverallReputation** auf 1 festgelegt, um die Übereinstimmung Filter XR testen. Der Titel erhalten die Benutzer Reputation-Wert für den Benutzer aus dem Dienst Benutzer Statistiken, wie im Abschnitt "Anforderungen für die Bewertung Xbox" dieses Themas beschrieben.


## <a name="resetting-a-users-reputation-to-the-defaults"></a>Ruf hat ein Benutzer zurücksetzen auf die Standardwerte

Der Titel wird das OverallReputationIsBad-Attribut, um anzugeben, dass der Benutzer einen guten Ruf hat. Ruft **POST (/ Benutzer/me/Resetreputation)** und legt alle Werte auf 75. Ein einzelner Aufruf **/users/xuid({xuid})/deleteuserdata** können verwendet werden, um mehrere Xbox-Benutzer-IDs zurücksetzen. Der Anforderungstext ist:

```json
    {
      "xuids": [
        "2814659110958830"
      ]
    }
```

## <a name="sending-feedback-from-titles"></a>Senden von Feedback von Titeln

Titel können Übereinstimmungen Fair Play-Feedback zu den Spielern senden. Dies erfolgt entweder direkt über den Titel oder aus dem Title-Dienst in Batches. Können Sie der Titel der **ReputationService.SubmitReputationFeedbackAsync Methode** oder die folgenden REST-Methoden:

|                      |                                                             |
|----------------------|-------------------------------------------------------------|
| POST-Client          | https://reputation.xboxlive.com/users/xuid{XUID}/feedback |
| Dienst (Batch) POST | https://reputation.xboxlive.com:10443/users/batchfeedback   |

Finden Sie unter **Feedback (JSON)** für eine Beispiel-JSON-Text für die Übermittlung und Definitionen der zulässigen Felder.


## <a name="types-of-feedback-allowed"></a>Arten von Feedback zulässig

Die Kategorien von Feedback, das gesendet werden können, sind in definiert **Feedback (JSON)**.


## <a name="when-titles-should-send-feedback"></a>Titel sollte Wenn Feedback senden.

Ein Titel sollte negatives Feedback senden, wenn ein Ereignis sich negativ auf die Player-Benutzererlebnis sorgen, in einem Spiel betrifft. Als allgemeine Regel sollte auf der Titel nur eine Feedback pro Typ pro senden runde wiedergegeben. Beispielsweise sollten der Titel:

1.  Senden Sie nur eine FairPlayKillsTeammates Feedbackelement für eine Person pro Roundtrip, 3 oder mehr Teamkollegen, ein Ereignis jedes Mal, wenn die Person, die einem Teamkollegen beendet gesendet werden, anstatt zu beenden.
2.  Senden Sie das Feedbackelement FairplayQuitter aus, wenn jemand eine Übereinstimmung absichtlich frühzeitig beendet.
3.  Senden Sie das Feedbackelement FairplayUnsporting, einmal pro Race, wenn ein Benutzer in einer autorennspiel rückwärts gesteuert wird.
