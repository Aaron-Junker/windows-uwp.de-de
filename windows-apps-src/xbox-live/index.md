---
title: Xbox Live – Entwicklerhandbuch
description: 'Es wird beschrieben, wie Sie Xbox Live-Dienste verwenden, um für Ihr Spiel eine Verbindung mit dem Xbox Live-Gamingnetzwerk herzustellen.'
ms.date: 08/22/2017
ms.topic: article
keywords: 'windows 10, uwp, games, xbox, xbox live'
ms.localizationpriority: medium
---
# <a name="what-is-xbox-live"></a>Was ist Xbox Live?

Xbox Live ist ein erstklassiges Gamingnetzwerk, das Millionen von Spielern weltweit verbindet. Sie können Xbox Live Ihrem Windows 10- oder Xbox One-Spiel hinzufügen, um die Xbox Live-Features und -Dienste zu nutzen.

Im Rahmen des Xbox Live Creators-Programms können alle Benutzer mit einem [Partner Center](https://partner.microsoft.com/dashboard)-Konto ein Xbox Live-fähiges UWP-Spiel (Universelle Windows-Plattform) entwickeln, das sowohl auf Windows 10-PCs als auch auf Xbox One-Konsolen ausgeführt werden kann.

Für Spieleentwickler, die die gesamte Xbox Live-Erfahrung nutzen möchten, z. B. Multiplayer-Modus, Erfolge und native Xbox-Konsolenentwicklung, gibt es zusätzliche Entwicklerprogramme, die unter [Übersicht über das Entwicklerprogramm](developer-program-overview.md) ausführlicher beschrieben werden.

Hier sind einige Gründe für das Hinzufügen von Xbox Live zu Ihrem Spiel aufgeführt:

- Xbox Live verbindet Gamer über Xbox One und Windows 10 hinweg, damit diese mit ihren Freunden spielen und Zugang zu einer großen Spielercommunity erhalten.
- Mit Xbox Live können Spieler ihren Gaming-Status aufbauen, indem Sie Erfolge entsperren, Clips mit spannenden Spielverläufen teilen, Gamerscore-Punkte sammeln und ihren Avatar perfektionieren.
- Xbox Live ermöglicht es Spielern, an dem Punkt weiterzuspielen, an dem sie sich auf einer anderen Xbox One bzw. einem PC zuletzt befunden haben. Alle gespeicherten Spielstände sind dann auf dem neuen Gerät verfügbar.
- Xbox Live ist auf Leistung, Geschwindigkeit und Zuverlässigkeit ausgelegt, und jeden Monat werden mehr als 1 Milliarde Multiplayer-Spiele gespielt.
- Im geräteübergreifenden Multiplayer-Modus können Gamer unabhängig davon, ob diese eine Xbox One oder einen Windows 10-PC verwenden, zusammen mit ihren Freunden spielen.

> [!note]
> Diese Themen sind für Spieleentwickler bestimmt, die ihrem Spiel Unterstützung für Xbox Live hinzufügen möchten. Verbraucherinformationen zu Xbox Live finden Sie unter [Xbox Live](https://www.xbox.com/live/).

## <a name="how-xbox-live-works"></a>Funktionsweise von Xbox Live

Aus technischer Sicht ist Xbox Live eine Sammlung von Microservices,mit denen Xbox Live-Features verfügbar gemacht werden, z. B. Profil, Freunde und Anwesenheit, Statistiken, Bestenlisten, Erfolge, Multiplayer-Modus und Spielersuche. Xbox Live-Daten werden in der Cloud gespeichert, und der Zugriff darauf ist über REST-Endpunkte und sichere WebSockets möglich, die über eine Reihe von clientseitigen APIs für Spieleentwickler zugänglich sind.

Zusätzlich zu den REST-APIs gibt es auch clientseitige APIs, die die REST-Funktionalität umschließen. Weitere Informationen finden Sie unter [Einführung in Xbox Live APIs](introduction-to-xbox-live-apis.md).

### <a name="get-started-with-xbox-live"></a>Erste Schritte mit Xbox Live

Die folgenden Leitfäden können Ihnen als Hilfe beim Einstieg in die Xbox Live-Entwicklung dienen. Dies gilt unabhängig davon, ob Sie für UWP oder die Xbox-Konsole entwickeln.  Es sind auch Leitfäden zur Einrichtung von Spielengines vorhanden.

| Thema                                                                                                                                             | Beschreibung                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Übersicht über das Entwicklerprogramm](developer-program-overview.md) | Es werden unterschiedlichen Entwicklerprogramme beschrieben, die die Xbox Live-Entwicklung ermöglichen. |
| [Erste Schritte – Xbox Live Creators-Programm](get-started-with-creators/get-started-with-xbox-live-creators.md) | Enthält Informationen zum Einstieg in Xbox Live im Rahmen des Xbox Live Creators-Programms. |
| [Erste Schritte mit Xbox Live als ID@Xbox- oder verwalteter Entwickler](get-started-with-partner/get-started-with-xbox-live-partner.md) | Es wird beschrieben, wie Sie als Entwickler im Rahmen des ID@Xbox-Programms in Xbox Live einsteigen können. |

### <a name="using-xbox-live"></a>Verwenden von Xbox Live

Nachdem Sie einen Titel erstellt haben und die grundlegenden Dinge funktionieren, können Sie die erforderlichen Hintergrundinformationen in diesem Abschnitt nutzen, bevor Sie mit dem Codieren beginnen.

| Thema                                                                                                                                             | Beschreibung                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Verwenden von Xbox Live](using-xbox-live/using-xbox-live.md) | Nachdem Sie Ihren Titel eingerichtet und das Xbox Live SDK integriert haben, können Sie die Anmeldung implementieren und sich weiter über die Xbox Live-Programmierung informieren.
| [Bewährte Methoden zum Aufrufen von Xbox Live](using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) | Machen Sie sich mit den Grundlagen von Xbox Live-Aufrufmustern und den bewährten Methoden vertraut, um sicherzustellen, dass Ihre Titel eine gute Leistung aufweisen und keine Begrenzung der Übertragungsrate erfolgt.
| [Problembehandlung für die Xbox-Live-Dienste-API](using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md) | Enthält Informationen zu häufigen Problemen, die auftreten können, sowie Vorschläge zu ihrer Behebung.

### <a name="xbox-live-social-platform"></a>Soziale Plattform von Xbox Live

Die sozialen Eigenschaften der Xbox Live-Funktionen können Ihre Zielgruppe organisch auf 55 Millionen aktive Benutzer erweitern.  In diesem Abschnitt wird beschrieben, wie Sie in die Features der sozialen Plattform von Xbox Live einsteigen können.

| Thema                                                                                                                                             | Beschreibung                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Soziale Plattform von Xbox Live](social-platform/social-platform.md) | Wenn Sie einen Benutzer anmelden können, können Sie mit der Nutzung der Features für die soziale Plattform von Xbox Live beginnen, z. B. Nutzen des Soziogramms eines Benutzers, Rich Presence und andere. |

### <a name="xbox-live-data-platform"></a>Xbox Live-Datenplattform

Mit der Xbox Live-Datenplattform wird die Nutzung von Spielerstatistiken, Erfolgen und Bestenlisten gesteuert.  Lesen Sie sich die Themen dieser Reihe durch, um sich darüber zu informieren, wie Sie dies in Ihrem Titel nutzen können.

| Thema                                                                                                                                             | Beschreibung                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live-Datenplattform](data-platform/data-platform.md) | Enthält eine kurze Übersicht über die Datenplattform sowie eine Anleitung zur bestmöglichen Einbindung von Statistiken, Bestenlisten und Erfolgen in Ihren Titel.
| [Spielerstatistiken](leaderboards-and-stats-2017/player-stats.md) | Statistiken sind die Grundlage von Bestenlisten.  Hier wird beschrieben, wie Sie sie definieren und nutzen.
| [Bestenlisten](leaderboards-and-stats-2017/leaderboards.md) | Steigern Sie den Wettbewerb zwischen den Benutzern, indem Sie auf intelligente Weise Bestenlisten einbauen.
| [Erfolge](achievements-2017/achievements.md) | Erfolge sind eines der bekanntesten Features in Xbox Live und hervorragend geeignet, um Spieler zu binden. Es wird beschrieben, wie Sie diese in Ihrem Titel verwenden.

### <a name="xbox-live-multiplayer-platform"></a>Xbox Live-Multiplayer-Plattform

Der Multiplayer-Modus ist eine hervorragende Möglichkeit, um die Lebensdauer Ihres Titels zu verlängern und dafür zu sorgen, dass das Spielen nicht langweilig wird.  Xbox Live verfügt über umfassende Features für den Multiplayer-Modus und die Spielersuche.  Darüber hinaus verfügen Sie über mehrere API-Optionen mit unterschiedlichen Ebenen in Bezug auf Einfachheit und Flexibilität.

| Thema                                                                                                                                             | Beschreibung                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live-Multiplayer-Plattform](multiplayer/multiplayer-intro.md) | Wenn die Xbox Live-Multiplayer-Entwicklung neu für Sie ist oder Sie noch nicht mit den neuen APIs vertraut sind, z. B. Multiplayer-Manager und Xbox Integrated Multiplayer (XIM), können Sie hier beginnen. |
| [Multiplayer-Szenarien](multiplayer/multiplayer-scenarios.md) | Enthält Vorschläge und Anleitungen, wie Sie den Multiplayer-Modus in Ihren Titel integrieren können. |
| [Xbox Integrated Multiplayer](multiplayer/xbox-integrated-multiplayer.md) | Xbox Integrated Multiplayer (XIM) ist eine einfache eigenständige Schnittstelle zum Hinzufügen des Multiplayer-Modus, von Echtzeit-Netzwerkfunktionen und einer Chatfunktion für Ihren Titel. |
| [Multiplayer-Manager](multiplayer/multiplayer-manager.md) | Mit Multiplayer-Manager wird eine API für häufige Multiplayer-Szenarien bereitgestellt. |

### <a name="xbox-live-storage-platform"></a>Xbox Live-Speicherplattform

Die Xbox Live-Speicherplattform verfügt über Titelspeicher und Onlinespeicher.  Dies sind zwei unterschiedliche Dienste, die sich gegenseitig ergänzen.  Per Onlinespeicher können Sie gespeicherte Spielstände in der Cloud implementieren, die dann unabhängig davon, auf welchen Geräten sich ein Benutzer anmeldet, abgerufen werden können.  Mit dem Titelspeicher können Sie Blobs mit Daten speichern, die pro Benutzer oder Titel vorliegen und für mehrere Benutzer freigegeben werden können.

| Thema                                                                                                                                             | Beschreibung                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live-Speicherplattform](storage-platform/storage-platform.md) | Verwenden Sie die Xbox Live-Speicherdienste zum Speichern von Spielständen, Wiederholungen, Benutzereinstellungen und anderen Daten in der Cloud. |
| [Onlinespeicher](storage-platform/connected-storage/connected-storage-technical-overview.md) | Enthält eine Übersicht und einen Programmierleitfaden für Onlinespeicher. |
| [Titelspeicher](storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) | Enthält eine Übersicht und einen Programmierleitfaden für Titelspeicher. |
