---
title: Feedback (JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
description: " Feedback (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1cb4ecc12219d54af1c4ab420671f2bbfa81f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644765"
---
# <a name="feedback-json"></a>Feedback (JSON)
Enthält Angaben zum Feedback über einen Player.
<a id="ID4EN"></a>


## <a name="feedback"></a>Feedback senden

Das Feedback-Objekt hat den folgenden Spezifikationen.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- |
| sessionRef| object | Ein Objekt, das die Sitzung MPSD beschreibt bezieht sich auf dieses Feedback, oder null. |
| feedbackType| string | Der Typ des Feedbacks. Mögliche Werte werden definiert, der <b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>. |
| textReason| string| Benutzerdefinierten Text, der der Absender hinzugefügt, um zu erklären, warum das Feedback wurde gesendet. |
| voiceReasonId| string| Die ID der eine benutzerdefinierte Voice-Datei von Kinect, die der Absender hinzugefügt, die erklären, warum das Feedback wurde gesendet (Base-64). |
| evidenceId| string| Die ID einer Ressource, die als Beweis für das Feedback übermittelten verwendet werden kann, aufgezeichnet beispielsweise eine Videodatei während des Spiels. |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>Feedback-Typen

Die Spalte "Per" gibt an, wer das Feedback absenden können.

   * "User" bedeutet, dass es von der Konsole mit einem XToken für die Authentifizierung übermittelt werden kann, daher die API lässt **SubmitFeedback**.
   * "Partner" bedeutet, dass es von einem Partner gesendet werden kann mithilfe eines Zertifikats für die Ansprüche, daher die API lässt **SubmitBatchFeedback**.
   * "Datenschutz" bedeutet, dass nur der SLS Datenschutz-Dienst auf das Feedback senden kann.
   * "None" bedeutet, dass das Feedback wird intern von der SLS Reputation Service, für die Überwachung generiert und kann nicht von einer der Aufrufer gesendet werden.

| Typ| Gesendet von| Anmerkungen|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| Benutzer| Benutzer senden Sie Feedback zum Bericht ungeeignet Sprachkommunikation aus innerhalb eines Titels und über das Dashboard für die Xbox. |
| CommsInappropriateVideo| Benutzer, Partner| Senden Feedback an das ungeeignete Video von innerhalb eines Titels und über das Dashboard für die Xbox zu melden, Benutzern und Partnern. |
| CommsMuted| Vertraulichkeit| Wenn ein Benutzer einen anderen Player schaltet stumm, sendet Datenschutz dieses Feedback an den Bewertungsdienst an. |
| CommsPhishing| Benutzer| Benutzer senden, dieses Feedback auf eine Phishing zurückgegeben wird. |
| CommsPictureMessage| Benutzer| Der posteingangsdienst ruft Reputation Service, die die Grundlage von Kommunikation eines Bildes absenderzuverlässigkeit aktualisiert und meldet das Feedback an das Team für die Erzwingung. |
| CommsSpam| Benutzer| Benutzer senden, dieses Feedback, um eine Spam-Meldung zu melden. |
| CommsTextMessage| Benutzer| Der posteingangsdienst ruft Reputation Service, aktualisiert die der Ruf des Absenders und meldet das Feedback an das Team erzwingen. **Hinweis:** Die Inbox UI müssen sich auf eine Schaltfläche, um Benutzern das Kennzeichnen einer Nachricht ermöglichen. |
  | CommsVoiceMessage | Benutzer | Der posteingangsdienst ruft Reputation Service, die die Grundlage von Kommunikation einer Sprachnachricht absenderzuverlässigkeit aktualisiert und meldet das Feedback an das Team für die Erzwingung.  |
  | FairPlayBlock | Vertraulichkeit | Datenschutz sendet dieses Feedback an den Reputation Service, wenn ein Benutzer einen anderen Player blockiert.  |
  | FairPlayCheater | Benutzer, Partner | Titel, die bestimmen, dass ein Benutzer Aktivitäten ist, können dieses Feedback ohne Eingreifen des Benutzers senden.  |
  | FairPlayConsoleBanRequest | Partner | Ein Partner sendet dieses Feedback wird empfohlen, eine Konsole von Xbox Live auszuschließen.  |
  | FairPlayIdler | Benutzer, Partner | Softwaretitel an, die bestimmen, ob ein Benutzer im Leerlauf absichtlich in einem Spiel nach "Round", in der Regel round steht, können dieses Feedback ohne Eingreifen des Benutzers senden.  |
  | FairPlayKicked | Benutzer, Partner | Softwaretitel an, die erkennt, dass ein Benutzer aus einem Spiel (gestartet) gewählt wird, können dieses Feedback ohne Eingreifen des Benutzers senden.  |
  | FairPlayKillsTeammates | Benutzer, Partner | Softwaretitel an, die automatisch bestimmen können, wenn ein Player Killls kann seine teamkamerad dieses Feedback ohne Eingreifen des Benutzers senden.  |
  | FairPlayQuitter | Benutzer, Partner | Titel, die bestimmen, dass ein Benutzer ein Spiel frühzeitig beenden können dieses Feedback ohne Eingreifen des Benutzers senden.  |
  | FairPlayTampering | Benutzer, Partner | Titel, die bestimmen, dass ein Benutzer auf dem Datenträger Inhalt manipuliert wurde, können dieses Feedback ohne Eingreifen des Benutzers senden.  |
  | FairPlayUnblock | Vertraulichkeit | Datenschutz sendet dieses Feedback an den Bewertungsdienst, wenn ein Benutzer einen anderen Player Blockierung aufgehoben wird.  |
  | FairPlayUserBanRequest | Partner | Ein Partner sendet dieses Feedback wird empfohlen, einen Benutzer von Xbox Live auszuschließen.  |
  | InternalAmbassadorScoreUpdated | Keine | Dies ist eine interne feedbacktyp nicht für die Verwendung von Aufrufern.  |
  | InternalReputationReset | Keine | Dies ist eine interne feedbacktyp nicht für die Verwendung von Aufrufern.  |
  | InternalReputationUpdated | Keine | Dies ist eine interne feedbacktyp nicht für die Verwendung von Aufrufern.  |
  | PositiveHelpfulPlayer | Benutzer, Partner | Senden Benutzern und Partnern dieses Feedback zum Übermitteln von positive Informationen hilfreich, andere Spieler aus innerhalb von Spielen, Foren und So weiter.  |
  | PositiveHighQualityUGC | Benutzer, Partner | Benutzer und Partner senden dieses Feedback, um anzugeben, dass der Titel positive rückmeldungen auf freigegebenen benutzergenerierte Inhalte aus in das Spiel übermitteln durch Benutzer zulassen soll z. B. Setups in Forza optimieren.  |
  | PositiveSkilledPlayer | Benutzer, Partner | Senden Benutzern und Partnern dieses Feedback, um anzugeben, dass der Titel Benutzer am Ende einer Sitzung MPSD MVP abstimmen können.  |
  | UserContentGamerpic | Benutzer | Benutzer senden, dieses Feedback auf ein Spieler ungeeigneten Bild direkt auf der Karte Spieler zu melden.  |
  | UserContentGamertag | Benutzer | Benutzer senden, dieses Feedback zum Melden eines ungeeigneten Spieler Tags direkt auf der Karte Spieler.  |
  | UserContentInappropriateUGC | Benutzer, Partner | Senden Benutzern und Partnern dieses Feedback, um anzugeben, dass der Titel Benutzer so kennzeichnen Sie ungeeignet freigegebenen benutzergenerierte Inhalte von innerhalb des Spiels, z. B. Paint Aufträge in Forza zulassen soll.  |
  | UserContentPersonalInfo | Benutzer | Benutzer senden, dieses Feedback, um eine Biografie und andere persönliche Informationen direkt von der Spieler Karte zu melden.  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax


```json
{
"sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Halo556932",
  },
  "feedbackType": "CommsAbusiveVoice",
  "textReason": "He called me a doodoo-head!",
  "voiceReasonId": null,
  "evidenceId": null
}

```


<a id="ID4EOEAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EQEAC"></a>


##### <a name="parent"></a>Parent

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
