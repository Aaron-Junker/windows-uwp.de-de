---
title: Senden von Player-Feedback von Ihrem Titel
description: Erfahren Sie, wie dem Titel des positive playerideen heraufstufen, indem Sie die Player-Feedback an den Xbox Live-zuverlässigkeitsdienst senden.
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Zuverlässigkeit, Player-feedback
ms.localizationpriority: medium
ms.openlocfilehash: 3e8d82fc9b195f174bf7a46a8d21fb20b2df9551
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592235"
---
# <a name="sending-player-feedback-from-your-title"></a>Senden von Player-Feedback von Ihrem Titel
Die Mehrheit der Xbox Live-Mitglieder sind großartig, aber es gibt ein kleinen Prozentsatz der "Ungültige Apples", die Spiel Erfahrungen anderer Benutzer zu beeinträchtigen. Wir diese kleine Prozentsätze der Benutzer durch Benutzer zu identifizieren und den Titel von übermitteltem Feedback. Wir unterstützen den Rest der Auffüllung zu schützen, indem Sie sicherstellen, dass diese "Ungültige Apples" einen begrenzten Multiplayer-Erfahrung verfügen, in denen können nicht sie gute Spieler Spiele beeinträchtigen. Xbox basiert in weiten Teilen auf Benutzer, die zum Melden von anderen Benutzern des Systems korrekt beizubehalten können, Titel im Xbox One jedoch direkt teilnehmen und erheblich verbessern die Genauigkeit der Anzahl der Benutzer-Bewertung.

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>Schritte zum Übermitteln von Feedback in Titel oder Titel-Dienst
1. Hinzufügen von Feedback Momente von Titeln oder Titel-Dienst
2. Bestimmen Sie den richtigen feedbacktyp
3. Aufrufen von Reputation Feedback-APIs mit Feedback

### <a name="adding-feedback-moments-to-title-or-title-service"></a>Hinzufügen von Feedback Minuten, bis Sie Titel oder Titel-Dienst
Alle Spieler hatten schlechten Erfahrungen mit Teammitgliedern, die eigene Seite, Spieler, die nur für eigenständige anstelle der aktiven Audiowiedergabe oder Cheaters, die auflöst, ihre Spiele zu sabotieren. Xbox Live ermöglicht Benutzern, die diese problematischen Player direkt zu melden, aber Feedback der Benutzer ist nicht perfekt. Titel können ganz einfach ermitteln, einfache Dinge wie Spielern im Spiel im Leerlauf und einem frühen Zeitpunkt zu beenden und manchmal sogar bestimmen, wenn jemand Aktivitäten ist. Titel des kann in einer Vielzahl Feedback nach kurzer Zeit, dadurch wird die Arbeit erleichtern aller gute Spieler, Feedback senden.

### <a name="determining-the-correct-feedback-type"></a>Bestimmen die richtige Feedback geben
Die reputationssystem verfügt über viele Arten von Feedback, die die verschiedenen Möglichkeiten zum Erfassen, dass ein Benutzer, Feedback erfordern kann vorgesehen sind. Sie sind vollständig in Tabelle 1 unten aufgeführt. Die meisten von ihnen ist negativ, aber es ist möglich, eines Benutzers Ruf mit auch positive rückmeldungen zu entwickeln.

Xbox-System-Benutzeroberfläche bietet eine Möglichkeit für Spieler zum Übermitteln von Feedback für andere Benutzer im Spiel. Dieses Feedback der Benutzer-an-Benutzer ist nicht viel Gewicht, da der Benutzer für Griefing anfällig sind, voneinander beim Ausführen verlieren. Titel können das System UI, dazu wird die Benutzeroberfläche bereit, um direkt auf einem anderen, Feedback zu ergänzen, aber stattdessen wird es bevorzugt, dass der Titel für den Titel selbst Übermitteln von Feedback mithilfe von Partner-Feedback. Partnerfeedback ist sehr vertrauenswürdigen.

## <a name="example-partner-feedback-usage-scenarios"></a>Beispielszenarien für die Verwendung von Partner Feedback

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>Benutzer, die beim Abbrechen eines Spiels in der Mitte einer Übereinstimmung
Ein Spieler verliert ein Spiel und des Spiels im Menü zum Beenden des Spiels, verwendet, die durch das Abbrechen ihrer Teamkollegen. Wenn ein Titel dieses Verhalten erkennt können sie das Verhalten mit melden **FairPlayQuitter.**

### <a name="idling-after-match-found-in-game"></a>Im Leerlauf nach einer Übereinstimmung im Spiel
Ein Spieler ruft stimmt überein, mit anderen Spielern spielen, aber konsistent steht im Leerlauf, im Spiel anstelle des Teams zu unterstützen. Der Titel kann melden das Player-Verhalten mit **FairplayIdler.**

### <a name="killing-teammates-in-game"></a>Abbrechen von Teamkollegen im Spiel
Ein Spieler in einem Spiel wird ständig Teamkollegen zum Spaß zu beenden. Wenn Sie ein Titel erkennt, dass ein Benutzer konsistent ist kann es Team-Abbrechen den Spieler mit melden **FairPlayKillsTeammates.**

### <a name="title-has-community-kickvote-feature"></a>Titel hat Community Kick/Vote-Funktion
Ein Spieler hat von Playern in die Runde aus der Sitzung für schlechtes Verhalten zu entfernenden bewertet. Wenn Sie der Titel der Spieler aus der Sitzung entfernt kann werden gemeldet, dass den Benutzer mit der **FairPlayKicked.**

### <a name="helpful-player-community-vote"></a>Hilfreiche Player Community Stimme
Nachdem einige Zeit damit verbracht eines Spiels bietet der Titel eine Option aus, um eine Person auswählen, die die größte beigetragen hat. Wenn Sie ein Titel dieser Aktion wird können sie melden, das Verhalten mit **PositiveHelpfulPlayer.**

### <a name="high-quality-ugc-user-generated-content"></a>Hohe Qualität benutzergenerierte Inhalte-(vom Benutzer generierte Inhalte)
Ein Titel verfügt über eine Szene in der ein Spieler den optimalen Entwurf für die ein Fahrzeug auswählen kann. Wenn Sie ein Titel dieser Aktion wird können sie melden, das Verhalten mit **PositiveHighQualityUGC.**

### <a name="skilled-player"></a>Erfahrene Player
Nachdem einige Zeit damit verbracht eines Spiels bietet der Titel eine Option zum Auswählen von einer Person, die dem MVP-Muster ist, wer der beste Player wurde. Wenn ein Titel kann sehen, dass es sich bei verdient einen Player Consistenly MVP-Status sie meldet das Verhalten mit **PositiveSkilledPlayer.**

### <a name="user-reports-questionable-ugc-in-title"></a>Benutzer meldet fragwürdige benutzergenerierte Inhalte im Titel
Ein Titel verfügt über eine Szene in der ein Spieler Vehicle-Entwürfe anzeigen kann. Ein Spieler bemerkt eine anstößige entwerfen und sie melden will. Wenn Sie ein Titel dieser Aktion wird kann werden gemeldet, dass den Täter mit **UserContentInappropriateUGC.**

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>Titel möchte eine XBL Sperren Überprüfung für einen Player, deren Position
Einen Titel für die Community-Manager verfügt über einen Low-Reputation Player bemerkt, der konsistent in ihr Spiel Probleme verursacht wird. Ein Titel kann anfordern, eine XBL-Richtlinie und die Erzwingung Team Überprüfung verwenden **FairPlayUserBanRequest.**

## <a name="complete-behavior-feedback-options"></a>Führen Sie die Optionen für Kundenfeedback Verhalten
Die folgende Tabelle listet die Feedback-Typen, die Sie, zum Übermitteln von Feedback von Benutzern für den Titel verwenden können. Der Reputation Service ist flexibel und kann problemlos neue Feedback-Typen hinzufügen, wenn Sie glauben, dass diese des Titels-Anforderungen nicht gerecht werden. Bitte teilen Sie Ihren Account Manager, wenn Sie, sehen Sie einen neuen feedbacktyp hinzugefügt möchten.

Tabelle 1: Die verschiedenen Partnerfeedback Typen der Ruf unterstützt.

**FairPlay-Feedback-Typen**               | **Beschreibung**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | Melden Sie einen Player, der absichtlich zu ihren eigenen Teamkollegen beenden ist
FairplayCheater                           | Melden Sie einen Player, die sicherlich Aktivitäten ist
FairplayTampering                         | Melden Sie einen Player, die sicher mit dem Spiel Datenträger meddled hat oder andernfalls manipuliert die Spiele-Software oder hardware
FairplayUserBanRequest                    | Melden Sie, dass ein Spieler, die Sie denken, führt zu eine Unterbrechung erhalten hat
FairplayConsoleBanRequest                 | Bericht eine Konsole, die Sie denken sollten gesperrt werden, von der Verbindung mit der Xbox Live
FairplayUnsporting                        | Melden Sie einen Player, der sicherlich in unsportsmanlike Ermittlung interessanter ist.
FairplayIdler                             | Melden Sie ein Player, Multiplayer-eingibt, entspricht jedoch nicht aktiv wiedergegeben wird
FairplayLeaderboardCheater                | Melden Sie einen Player, die geschwindelt sicherlich ganz oben auf die Bestenliste angezeigt wurde
**Kommunikation-Feedback-Typen**         |
CommsInappropriateVideo                   | Melden Sie ein Spieler, im Videochat nicht zulässig wird
**Vom Benutzer generierte Inhalte Feedback-Typen** |
UserContentInappropriateUGC               | Melden einer ungeeigneten Teil des Inhalts, die ein Spieler in der Titelleiste Ihres erstellt
UserContentReviewRequest                  | Melden Sie ein Inhaltselement proaktiv, damit das Team XBLPET es überprüfen
UserContentReviewRequestBroadcast         | Melden Sie einen Broadcast proaktiv, damit das Team XBLPET es überprüfen
UserContentReviewRequestGameDVR           | Melden Sie einen Clip GameDVR proaktiv, damit das Team XBLPET es überprüfen
UserContentReviewRequestScreenshot        | Melden Sie einen Screenshot proaktiv, damit das Team XBLPET es überprüfen
**Positive Rückmeldungen**                     |
PositiveSkilledPlayer                     | Wenn Benutzer abstimmen können, um zu bestimmen, ein MVP zu werden, melden Sie einen erfahrenen Player beim sicher, dass der Spieler positive rückmeldungen verdient
PositiveHelpfulPlayer                     | Wenn ein Spiel, die Benutzeroberfläche für einen Player bereitstellt Berichten, dass eine andere hilfreich war, melden Sie den hilfreich player
PositiveHighQualityUGC                    | Wenn ein Spiel Benutzeroberfläche für einen Player zum Ergänzen eines anderen Benutzers Inhalt bereitstellt, melden Sie den Inhalt werde, positiv

## <a name="feedback-api-calls"></a>Feedback-API-Aufrufe
Titel können zwei Strategien zum Aufrufen des Diensts für die Bewertung verwenden. Die bevorzugte Methode ist für die benutzerberichterstattung im Aggregat von einem Partnerdienst mit einer Diensttoken für die Authentifizierung. Titel können Benutzer direkt aus dem Client auch melden. Die Client-API verfügt über Betrugsbekämpfung Technologie in erfordert mehrere Berichte auf einem Benutzer, die als gültig eingestuft wird. Beide APIs sind Batch, und es können mehrere Benutzer gleichzeitig melden.

Die folgenden Xbox Live-APIs können der Titel um Player Reputation Feedback zu senden:

Sprache | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

Alternativ kann der Titel der folgenden direkten REST-Methoden verwenden:

API          | URL                                                      | Auth-Anforderungen
------------ | -------------------------------------------------------- | ---------------------------------------
POST-Dienst | https://reputation.xboxlive.com/users/batchfeedback      | S-Token mit Ansprüchen für Partner und die sandbox
POST-Client  | https://reputation.xboxlive.com/users/batchtitlefeedback | Xtoken mit Titel und Sandbox-Ansprüchen

## <a name="feedback-object"></a>Feedback-Objekt
Das Feedback-Objekt hat die folgende Spezifikation für die neueste Version, 101. Beide APIs erwarten, dass einen Batch von der folgenden Objekte.

Mitglied       | Typ   | Beschreibung
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | object | Ein Objekt, das die Sitzung MPSD beschreibt bezieht sich auf dieses Feedback, oder null.
feedbackType | string | Der Typ des Feedbacks. Mögliche Werte werden in der ReputationFeedbackType-Enumeration definiert.
textReason   | string | Benutzerdefinierten Text, der der Absender hinzugefügt, um zu erklären, warum das Feedback wurde gesendet. Dies ist sehr wertvoll für unser Team der Richtlinie erzwingen.
evidenceId   | string | Die ID einer Ressource, die als Beweis für das Feedback übermittelten verwendet werden kann, aufgezeichnet beispielsweise eine Videodatei während des Spiels oder ein Aktivitätsfeed-Element.
titleID      | Zeichenfolge | Die ID der Titel des Titels Gespielte; nur erforderlich, wenn im Token nicht vorhanden.
targetXuid   | Zeichenfolge | Die XUID des Spielers, die Sie melden möchten.

Beispiel:

```json
POST https://reputation.xboxlive.com/users/batchtitlefeedback
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId" : null,
            "sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Title56932",
           },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Title detected this player killing team members 19 times",
            "evidenceId": null
        }
    ]
}
```

## <a name="feedback-qa"></a>Feedback, Fragen und Antworten

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>Q: Kann ich senden einen Hinweis auf das System mit Menschen zu helfen, die den Player-Bericht gesucht werden kann?
A: Ja, und es ist sehr hilfreich. Verwenden Sie die "TextReason"-Parameter, um die Enforcer zu helfen, die uns letzten Endes an das Feedback. Für eine Idler, können Sie z. B. einen Grund Text hinzufügen, der besagt "Wir keine Benutzereingaben von diesem Spieler nach der ersten fünf Sekunden des Spiels empfangen". Aus diesem Grund Text kann für die XBLPET Erzwingung Agents sehr nützlich sein, müssen Sie sicherstellen, dass der Text nützlich und beschreibend unterliegt.

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>Q: Ich sollte wie oft ich Feedback für einen Benutzer senden kümmern?
A: Titel sollten Reputation Service aufrufen, wenn sie sicher sind, dass ein Benutzer Feedback erhalten hat. Das System hat mehrere Sicherheit abfängt, um zu verhindern, dass Titel und die Benutzer können zu Auswirkungen, die Benutzer.

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>Q: Kann ich anpassen, dass die Gewichtung des Feedbacks gesendet werden?
A: Nein, bestimmt der Reputation Service die Gewichtung des Feedbacks. Wir sind immer die Gewichtungen, damit das System besser anpassen.

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>Q: Kann ich Feedback rückgängig gemacht, die ich für einen Benutzer gesendet haben?
A: Nein, ist die Feedback endgültig. Wenn Sie glauben, dass Ihre Titel, einen Fehler aufweist, und fehlerhafte Feedback gesendet werden, informieren Sie uns, und wir werden Ihre Titel hinzufügen, bis Sie den Fehler beheben.

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>Q: Kann ich sehen, dass das Feedback für mein Titel von Benutzern gesendet?
A: Diese Information ist derzeit nicht verfügbar Self-Service. Lassen Sie Ihr Konto-Manager, und wir haben Daten pro Titel, die wir freigeben können. In Kürze möchten wir dies für Sie direkt zur Verfügung stellen, und auch Mischung von Benutzern mit mit schlechtem Ruf, die mit Ihrem Titel angezeigt.

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>Q: Wenn ich in die Konsole senden, oder führen Sie die Benutzer sperren reviewanforderung wie weiß ich, was passiert ist?
A: Derzeit wird die Informationen für den Review XBL Richtlinie und die Durchsetzung Team gesendet, aber derzeit ist Sie nicht zum Sperren Review aktualisiert.

### <a name="q-is-there-a-reputation-score-per-title"></a>Q: Gibt es eine Bewertung der Zuverlässigkeit pro Titel?
A: Nein. Es gibt derzeit Sub-Bewertungen für Zuverlässigkeit für Fairplay, Kommunikation und benutzergeneriertem Inhalt, aber nicht nach Titel. Wir können hinzufügen, dass dieses Feature in der Zukunft liegt genügend Nachfrage, lassen Sie Ihren Account Manager wissen, ob Sie diese Funktion anfordern möchten.
