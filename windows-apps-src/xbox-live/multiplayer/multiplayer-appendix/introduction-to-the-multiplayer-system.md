---
title: Einführung in das Multiplayer-System
description: Bietet eine allgemeine Einführung in das Xbox Live Multiplayer-2015-System.
ms.assetid: d025bd2b-2ca4-4ba9-9394-4950d96ad264
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, Multiplayer-2015
ms.localizationpriority: medium
ms.openlocfilehash: 4a739aaa650a7086dfe58b1b8ca170e15b3b2ef0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629415"
---
# <a name="introduction-to-the-multiplayer-system"></a>Einführung in das Multiplayer-System

| Hinweis                                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dieser Artikel ist für die erweiterte-API-Nutzung.  Als Ausgangspunkt können Sie sehen Sie sich die [Multiplayer-Manager-API](../multiplayer-manager.md) der erheblich vereinfacht die Entwicklung.  Informieren Sie Ihre DAM wissen, ob Sie ein nicht unterstütztes Szenario im Multiplayer-Manager finden. |

Dieses Dokument enthält die folgenden Abschnitte
* Über das Multiplayer-System
* Multiplayer-Komponenten, Schnittstellen und Architekturen
* Parteien, die von 2015 Multiplayer unterstützt
* Multiplayer-Terminologie
* Neuerungen in 2015 Multiplayer-Spiele
* Unterschiede zwischen Xbox 360 und Xbox ein MPSD Sitzung Funktionen

## <a name="about-the-multiplayer-system"></a>Über das Multiplayer-System


2015 Multiplayer optimiert die direkte Verwendung von MPSD und Spiele Sitzungen. Es bietet eine bessere Unterstützung für mehrere gleichzeitige Sitzungen für einen einzelnen Titel oder einen Benutzer, und aktualisiert die Xbox-Services-API (XSAPI) zum Aktivieren von Titeln zu:

-   Kündigen Sie die aktuelle Aktivität eines Benutzers und die Verfügbarkeit für Joins.
-   Senden von Einladungen für Sitzungen, zusammen mit Kontextmenü für den Benutzer sichtbare (Titel angegeben)-Zeichenfolgen.
-   Entdecken und Sitzungen über Titel, Code zu verknüpfen.
-   Verwalten Sie WebSocket-Verbindungen auf MPSD, damit diese kurze Benachrichtigungen (Schulter Taps) für Sitzung ändert, z. B. Updates empfangen können, die Abonnements für Ereignisse und statusänderungen für die Verbindung ändern widerspiegeln. Außerdem wird vom MPSD WebSocket-Verbindungen verwendet, um schnell erkennen und reagieren auf die Trennung von Client verwendet wird.
-   Verwenden Sie SmartMatch Vermittlung.

## <a name="multiplayer-components-interfaces-and-architectures"></a>Multiplayer-Komponenten, Schnittstellen und Architekturen

### <a name="components-of-2015-multiplayer"></a>Komponenten von 2015 Multiplayer-Spiele

Multiplayer-Spiele ist ein System, das aus mehreren Komponenten besteht aus. Es ist flexibel genug, um andere Komponenten, z. B. dedizierte Server und Vermittlung von externen Systemen zu ermöglichen.
#### <a name="multiplayer-session-directory-mpsd"></a>Multiplayer Session Directory (MPSD)


Multiplayer-Sitzungsverzeichnis (MPSD) ist ein Dienst, der eine Auflistung von Sitzungen enthält. Eine Sitzung ist als ein Schutz von Dokumenten in der Cloud gespeicherten und Darstellung einer Gruppe von Personen, die ein Spiel definiert. MPSD Details, finden Sie unter [Multiplayer-Sitzung-Verzeichnis (MPSD)](multiplayer-session-directory.md).


#### <a name="multiplayer-apis"></a>Multiplayer-APIs

2015 Multiplayer-Spiele bietet eine Multiplayer WinRT-API, über implementiert die **Microsoft.Xbox.Services.Multiplayer Namespace**. Seine Komponenten sind die **MultiplayerService Klasse**, definieren einen Webdienst, der MPSD umschließt.

Ebenfalls enthalten ist eine Multiplayer-REST-API. Sie definiert die URIs und JSON-Objekte, die von der WinRT-API-Methoden aufgerufen werden. Die REST-Funktionalität in direkte Aufrufe aus Ihrem Titel verwendet werden kann, aber es wird empfohlen, MPSD indirekt über die WinRT-API zugreifen. Weitere Informationen finden Sie unter **aufrufen MPSD**.

#### <a name="xbox-party-system"></a>Xbox-Partei System

In 2015 Multiplayer-Spiele unterstützt das Xbox-Partei-System nur Chat Parteien als externe Entitäten. Titel können mit dem System Partei zum Ermitteln der Mitgliedschaft der Chat-Parteien interagieren. Weitere Informationen finden Sie unter **Parteien, die von 2015 Multiplayer unterstützt**.

Das Party-System unterstützt jetzt Spiele direkt über die MPSD-Sitzung, anstatt eine Spiele-Partei. Es ist Aufgabe der Titel, Sitzungen verwenden, um Vorgänge wie das Element Interaktionen, der Spieler in einem Spiel abrufen, sobald Platz verfügbar ist und Engagement der Benutzer warten Zeiten zu aktivieren.


### <a name="2015-multiplayer-interfaces"></a>2015 Multiplayer-Schnittstellen

2015 Multiplayer verwendet mehrere Schnittstellen mit anderen Xbox-Komponenten.
#### <a name="xbox-secure-sockets"></a>Xbox-SSL

2015 Multiplayer unterstützt Low-Level-Netzwerkkommunikation zwischen den Geräten mithilfe von SSL für Xbox und Winsock. Die Funktionen des Netzwerke verwendet Internet Protocol Security (IPSec), um Titel sicheres gerätezuordnung zu ermöglichen. Finden Sie unter **Networking Übersichten** für Details der Xbox secure Sockets-Funktion.


#### <a name="xbox-live-real-time-activity-service"></a>Xbox Live in Echtzeit Aktivitätsdienst

2015 Multiplayer verwendet die **aktivitätsüberwachung in Echtzeit Service** zum Zulassen von Titeln zu abonnieren MPSD Sitzung ändert, und Aktivieren der automatischen Ermittlung des Clients die Verbindung trennt. Weitere Informationen finden Sie im [Real-Time-Aktivität (RTA) Dienst](../../real-time-activity-service/real-time-activity-service.md).


#### <a name="xbox-live-matchmaking-service"></a>Xbox Live-Vermittlungsdienst

Die **Vermittlungsdienst** Gruppen von Playern, je nach ihren Einstellungen und Daten sowie Informationen, die bei der Vermittlung von SmartMatch bereitgestellte bildet. Details der Multiplayer-Spiele für diesen Dienst verwenden möchten, finden Sie unter [SmartMatch Vermittlung](smartmatch-matchmaking.md).


#### <a name="xbox-live-reputation-service"></a>Xbox Live-Zuverlässigkeitsdienst

Die *-zuverlässigkeitsdienst* Reputation und bei der Vermittlung Reputation gefilterte Statistiken verwaltet. Es wird von 2015 Multiplayer über SmartMatch Vermittlung verwendet. Weitere Informationen finden Sie unter [Reputation](../../social-platform/people-system/reputation.md).


#### <a name="xbox-live-compute"></a>Xbox Live-Serverdienst

Xbox Live Compute bietet die Cloud, die verarbeitungsleistung für Titel, die mithilfe von 2015 Multiplayer-Spiele. Weitere Informationen zur Verwendung von Xbox Live Compute finden Sie unter [mithilfe von Xbox Live Compute in Multiplayer](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer).


### <a name="typical-2015-multiplayer-architectures"></a>Typische 2015 Multiplayer-Architekturen

In diesem Abschnitt werden die häufigsten Multiplayer-2015-Architekturen.
#### <a name="peer-to-peer-architecture"></a>Peer-zu-Peer-Architektur

In Peer-zu-Peer-Architektur wird mithilfe des Titels MPSD und SmartMatch Vermittlung um Peer-Adressen zu ermitteln. Die Adressen werden Verbindung Peers mit Xbox-SSL verwendet. Weitere Informationen finden Sie unter *Einführung in die Winsock für Xbox One*.

![Diagramm der Peer-zu-Peer-Architektur](../../images/multiplayer/Mult2015ArchPeer.png)


#### <a name="client-server-architecture"></a>Client / Server-Architektur

In der Multiplayer-Client / Server-Architektur verwendet der Titel MPSD und SmartMatch Vermittlung, um die Adressen von dedizierten Server ermittelt. Diese werden dann verwendet, für die Verbindung mit dem dedizierten Server mit secure Sockets Xbox. Weitere Informationen finden Sie unter *Einführung in die Winsock für Xbox One*.

| Hinweis                                                                         |
|-------------------------------------------------------------------------------------------|
| Xbox Live Compute-Instanzen können als die Server in der Client / Server-Architektur verwendet werden. |

![Diagramm der Clientarchitektur für server](../../images/multiplayer/Mult2015ArchClientServer.png)


## <a name="parties-supported-by-2015-multiplayer"></a>Parteien, die von 2015 Multiplayer unterstützt
2015 Multiplayer-Spiele für Xbox One macht die Partei"game" nicht als Konstrukt auf Systemebene verfügbar. Allerdings unterstützt es die Partei"Chat" auf Systemebene, wie in der 2014 Multiplayer-Spiele.

| Hinweis                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------|
| Ihre Titel können Benutzeroberflächen, die ähnliche Implementierung unter Verwendung der game Partei, aber verwenden Sie stattdessen MPSD Sitzungen implementieren. |


### <a name="chat-party"></a>Chat-Partei

Eine Partei Chat ist eine Gruppe von Personen, die miteinander durch den Benutzer über die Xbox-App der einer Partei verwaltet chatten sind. Benutzer können während der Wiedergabe in einer Spiele-Sitzung oder beim Ausführen einer anderen Konsole-Aktivität in einer Partei Chat sein. Es gibt jedoch keine Bindungen zwischen den Benutzern im Chat Partei und andere Aktivitäten für diesen Benutzer.

Die Chat-Partei wird mit verfügbar gemacht. die *PartyChat Klasse*, die ermöglicht, dass des Titels, der den Zustand der Partei Chat zu untersuchen. Z. B. ein Titel kann Listen Sie die Member der Chat Partei mithilfe der *PartyChat.GetPartyChatViewAsync Methode*.


### <a name="implementing-features-similar-to-those-related-to-the-game-party"></a>Implementieren von Funktionen, wie Sie die an die Partei Spiele

In Multiplayer-Spiele 2014 verarbeitet die Spiele Partei mehrere Zwecke. Es darf es sich um einen Titel hinzu:

-   Kündigen Sie aktuelle Aktivität eines Benutzers und die Verfügbarkeit für joins
-   Senden von Einladungen (Einladungen) mit Sitzungen
-   Ermitteln und Verknüpfen von Sitzungen
-   Empfangen von Benachrichtigungen auf bestimmte Änderungen an die Sitzungen, die an die Partei registriert
-   Verwenden Sie SmartMatch Vermittlung
-   Eine Gruppe von Personen zusammen über mehrere Spiele-Sitzungen hinweg beibehalten

2015 Multiplayer unterstützt alle oben genannten Features direkt über MPSD-Sitzungen.

## <a name="multiplayer-terminology"></a>Multiplayer-Terminologie


| Begriff                                 | Beschreibung|
| --- | --- |
| Aktiven-Player                        | Ein Spieler, die auf den aktiven Status innerhalb der Sitzung festgelegt wurde. Der Titel stellt einen Player zu diesem Zustand, wenn der Spieler an ein Spiel beteiligt ist. Weitere Informationen finden Sie unter [Sitzung Benutzerstatus](mpsd-session-details.md).                                                                                                                                                                                                                                                                                          |
| Arbiter                              | Die Konsole in einer Spiele-Sitzung, die den Status der Sitzung Multiplayer-Sitzung Verzeichnis (MPSD) für ein Spiel, z. B. verwaltet wird, auf die Nutzung der game-Sitzungs mit Vermittlung, um mehr Spieler zu finden. Der Arbiter, die durch den Titel festgelegt ist. Der Arbiter ist nicht immer der Host des Spiels. Finden Sie unter [Sitzung Arbiter](mpsd-session-details.md).                                                                                                                                                                            |
| Angeordnete Spiel                        | Ein Typ des Spiels, das nur über ein einziger Player einladen von anderen Spielern genutzt, ohne jegliche Beteiligung der Vermittlung erstellt wird.                                                                                                                                                                                                                                                                                                                                                                                                    |
| Chat-Partei                           | Eine Gruppe von Personen, die zusammen chatten sind. Die Personen in der gleichen Aktivität beteiligt werden können, oder sie können in unterschiedlichen Aktivitäten, z. B. Spiele, Musik oder apps beauftragt werden. Finden Sie unter **Parteien, die von 2015 Multiplayer unterstützt**.                                                                                                                                                                                                                                                                                 |
| Spiele einladen                          | Eine Einladung zur Teilnahme eine Spiele-Sitzung.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Spiele-Sitzung                         | Eine Sitzung, in der Benutzer tatsächlich zusammen spielen. Alle multiplayer-Szenarien, z. B. Vermittlung oder beitreten zu einer Lobby, schließlich letztendlich in einer Spiele-Sitzung. Die Sitzung wird häufig als der Benutzer die aktuellen Aktivitäten zu Joins angekündigt. Sie dient auch zum Erstellen von der aktuellen Liste der Spieler. Finden Sie unter [MPSD Sitzungsdetails](mpsd-session-details.md).                                                                                                                                                           |
| Spiele-Sitzungshost                    | Die Konsole Ausführen des Spiels spielen Simulation für Titel, basiert auf einer Peer-zu-Peer hostbasierte Netzwerkarchitektur. Diese Konsole ist in der Regel identisch mit dem Arbiter, aber es muss nicht identisch sein.                                                                                                                                                                                                                                                                                                                            |
| Handle (oder Sitzungshandle)           | Ein Verweis auf eine MPSD-Sitzung, die zusätzlichen Zustand und Verhalten zugeordnet ist. Finden Sie unter [MPSD verarbeitet Sitzungen](multiplayer-session-directory.md).                                                                                                                                                                                                                                                                                                                                                                    |
| Inaktive Player                      | Ein Spieler, die in den inaktiven Zustand innerhalb der Sitzung festgelegt wurde. Den Titel festlegt einen Player zu diesem Zustand, wenn ein Spiel angehalten wird, oder andernfalls inaktiv, gemäß den Titel. In einigen Fällen MPSD möglicherweise auch einen Player als inaktiv festlegen, aber es ist in erster Linie der Verantwortung des Titels, der dazu. Weitere Informationen finden Sie unter [Sitzung Benutzerstatus](mpsd-session-details.md).                                                                                                                           |
| Hopper                               | Eine Hopper umfasst gesteuerte Übereinstimmung Tickets. Ein Titel kann mehrere Hoppers, jedoch nur Tickets innerhalb der gleichen Hopper abgeglichen werden können. Beispielsweise können Sie durch ein Titel ein Hopper erstellen, für welche Spieler Fähigkeit das wichtigste Element für den Abgleich ist. Sie können einen anderen Hopper verwenden, in dem Spieler nur verglichen werden, wenn sie den gleichen herunterladbaren Inhalt erworben haben. Weitere Informationen dazu, in denen Hoppers in den Workflow SmartMatch passen, finden Sie unter [SmartMatch Laufzeitvorgängen](smartmatch-matchmaking.md) |
| Verknüpfen von in Bearbeitung                     | Nach Beginn der Spiels, ist das Konzept der verknüpfen einen anderen Player spielen. Spieler können einen Freund Spiel durch die Freunde des Freundes Spieler Karte hinzufügen. Der Titel, können Sie die Spieler in die Spiele-Sitzung zum richtigen Zeitpunkt verschieben.                                                                                                                                                                                                                                                                                                      |
| Lobby-Sitzung                        | Eine Helper-Sitzung für eingeladene Spieler, die darauf warten, Spiele Sitzung nicht beitreten. Finden Sie unter [MPSD Sitzungsdetails](mpsd-session-details.md).                                                                                                                                                                                                                                                                                                                                                                                       |
| Übereinstimmung Zielsitzung                 | Eine Match-Sitzung so eingerichtet, dass während der SmartMatch Vermittlung die Übereinstimmung darstellen. Finden Sie unter [SmartMatch Vermittlung](smartmatch-matchmaking.md).                                                                                                                                                                                                                                                                                                                                                                                              |
| Match-Ticket-Sitzung                 | Richten Sie eine vorläufige Übereinstimmung Sitzung während der SmartMatch Vermittlung. Finden Sie unter [SmartMatch Vermittlung](smartmatch-matchmaking.md).                                                                                                                                                                                                                                                                                                                                                                                                         |
| MPSD-Sitzung                         | Eine sichere Dokument, das in das Verzeichnis mit dem Multiplayer-Sitzung (MPSD), auf der Xbox Live-Cloud befindet. Sie enthält eine Gruppe von Benutzern, die während der Ausführung eines Titels für Xbox One sowie Metadaten über die Benutzer und ihr Spiel verbunden sein könnte. Finden Sie unter [MPSD Sitzungsdetails](mpsd-session-details.md).                                                                                                                                                                                                                  |
| Multiplayer Session Directory (MPSD) | Der Dienst ausgeführt wird, in der Cloud, die das Multiplayer-System zum Speichern und Abrufen von Sitzungen verwendet. Finden Sie unter [Multiplayer-Sitzungsverzeichnis (MPSD)](multiplayer-session-directory.md).                                                                                                                                                                                                                                                                                                                                                                |
| Partei App                            | Ein Xbox One-System-snap-app, mit der Benutzer zum Anzeigen und verwalten ihre Seiten.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Server-Sitzung                       | Eine Spiele-Sitzung erstellt, die durch die Xbox Live Compute-Verarbeitung. Finden Sie unter [MPSD Sitzungsdetails](mpsd-session-details.md).                                                                                                                                                                                                                                                                                                                                                                                                            |
| Schulter Tap                         | Eine Benachrichtigung von MPSD zu einem Titel, den für den Dienst eine potenziell interessante Änderung aufgetreten ist. Tippen Sie auf die Schulter ist eine schnelle Erinnerung, oft weniger als eine reguläre Benachrichtigung nur zu Informationszwecken. Finden Sie unter [MPSD Benachrichtigungsverarbeitung ändern, und Trennen von Erkennung](multiplayer-session-directory.md).                                                                                                                                                                                                                     |
| SmartMatch Matchmaking               | Eine Xbox Live Vermittlung Funktion für Xbox One Titel, von dem Vermittlungsdienst implementiert. MPSD und Vermittlung verwenden, den Titel sendet eine Anforderung zum abgeglichen werden und wird benachrichtigt, dass eine passende Gruppe gefunden wurde später noch mal. Finden Sie unter [SmartMatch Vermittlung](smartmatch-matchmaking.md).                                                                                                                                                                                                                                  |

## <a name="whats-new-in-2015-multiplayer"></a>Neuerungen in 2015 Multiplayer-Spiele

| Achtung                                                                                                                                                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wenn 2015 Multiplayer-Spiele verwenden zu können, ist es wichtig zu beachten, dass die Partei-bezogenen Klassen im 2014 Multiplayer-Spiele nicht mehr verwendet werden soll. Mischen von 2015 Multiplayer-Funktionalität mit der Partei-bezogenen Klassen führt inkohärente Verhalten und muss nicht ausgeführt werden. |


### <a name="new-concepts-in-2015-multiplayer"></a>Neuen Konzepte in 2015 Multiplayer-Spiele


#### <a name="web-socket-connections-to-mpsd"></a>WebSocket-Verbindungen auf MPSD

MPSD ermöglicht jetzt die Titel für das WebSocket-Verbindungen mit ihm zu verwalten. Diese Verbindungen können Clients, Benachrichtigungen zu empfangen, wenn Sitzungen ändern. Weitere Informationen finden Sie unter [MPSD Änderung Benachrichtigung verarbeiten und Trennen von Erkennung](multiplayer-session-directory.md).


#### <a name="mpsd-session-handles"></a>MPSD Sitzung Handles

2015 Multiplayer unterstützt MPSD Sitzung Handles, die Verweise auf Sitzungen sind, die typisierte Daten enthalten kann. Weitere Informationen finden Sie unter [MPSD verarbeitet Sitzungen](multiplayer-session-directory.md).


### <a name="summary-of-new-2015-multiplayer-winrt-api-functionality"></a>Zusammenfassung der neuen 2015 Multiplayer-WinRT-API-Funktionen

Neue Funktionen für mehrere Spieler WinRT-API basiert auf der vorhandenen XSAPI, zu vereinfachen, die der Übergang für titles bereits die 2014 Multiplayer-Spiele in Kombination mit der API spielen Partei verwenden.

2015 Multiplayer fügt die *MultiplayerActivityDetails Klasse*, die Details der aktuellen Aktivität des Benutzers, z. B. darstellt, an der Sitzungs, in dem der Benutzer beigetreten ist.

Neue Funktionen hinzugefügt wurde die *MultiplayerService Klasse*. Beispiele sind die Methoden und Eigenschaften Umgang mit Aktivitäten für Benutzer und Gruppen mit sozialen Abrufen von Sitzungen, die verschiedene Filter und verarbeitet, liest und schreibt mithilfe von Handles von Spiele-Einladungen, und senden.

Die *MultiplayerSession Klasse* funktionell mit Sitzung Änderungstypen, die Abonnements für Benachrichtigungen, die Sitzung Vergleich und eine Sitzung festlegen, nach dem Schließen.

Die *MultiplayerSessionReference Klasse* geändert wird, um die von erben **IMultiplayerSessionReference**, um die Unterstützung von Cross-Namespace aufrufen. Die Klasse hat auch neuen Analysemethoden URI-Pfad.

| Hinweis                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zur Unterstützung von Ereignissen und Abonnements für Benachrichtigungen 2015 Multiplayer funktionell die *Microsoft.Xbox.Services.RealTimeActivity Namespace*. Ebenfalls enthalten, in der neuen Funktionalität *SystemUI.ShowSendGameInvitesAsync Methode*verwendet, um das Spiel Einladung Benutzeroberfläche für 2015 Multiplayer-Spiele anzeigen. |

## <a name="differences-between-xbox-360-and-xbox-one-mpsd-session-functions"></a>Unterschiede zwischen Xbox 360 und Xbox ein MPSD Sitzung Funktionen

| Funktion | Xbox 360 | Xbox One |
|---|---|---|
| **Abrufen von Sitzungsinformationen für Spiele** | XSessionGetDetails, XSessionSearchByID oder Titel wird nachverfolgen. | Anforderungen Sitzungsinformationen zu Softwaretiteln von MPSD. |
|**Migrieren Sie den host** | Wenn benötigt, ruft Titel XSessionMigrateHost an. | Je nach der Ursache der Migration Titel möglicherweise weisen Sie einen neuen Host für die Sitzung oder möglicherweise eine neue MPSD-Sitzung erstellt. |
| **Mehrere Player-Sitzungen** | Knifflig, mehr als eine Sitzung zu einem Zeitpunkt, z. B. XNetReplaceKey im Vergleich zu XNetUnregisterKey zu behandeln. | Dienstbasierte Sitzung vereinfacht die Verwaltung einer Sitzung übersichtlicher und vereinfacht die Verarbeitung von mehreren Sitzungen. |
| **Signouts und trennt die Verbindung** | Titel verfügt, behandelt die Verbindung trennt und Signout anders mit XCloseHandle oder XSessionDelete. | MPSD Signouts vereinfacht, und trennen Sie behandeln und Timeouts, die in der game-Konfiguration festlegen. |
| **Vermittlung** | Clientbasierte Vermittlung Abfragen | Dienstbasierte Vermittlung, die eine bessere Übereinstimmung Qualität und einfacher Hintergrund Vermittlung innerhalb des Titels ermöglicht. |


### <a name="sessions"></a>Sessions

Auf der Xbox 360 dargestellt eine Sitzung eine Instanz des Spiels. Benutzer für Sitzungen, in dem Vermittlungsdienst durchsucht und Statistiken am Ende einer Sitzung gemeldet.

Für Xbox One eine Sitzung mehr generisch ist, und stellt eine Gruppe von Playern. Eine Sitzung für die Netzwerkkonnektivität zwischen Konsolen erforderlich ist, und enthält Informationen, die von allen Benutzern in der Sitzung freigegeben werden soll. Einige Beispiele für die diese Informationen umfassen die Anzahl der Spieler in der Sitzung, die sichere Adresse der einzelnen Konsole in der Sitzung, und klicken Sie auf benutzerdefinierte Spieledaten zulässig.


### <a name="xbox-matchmaking"></a>Xbox-Vermittlung

Auf der Xbox 360 ausgeführt Titel Vermittlung, indem Sie ein Schema von Attributen und einen Satz von Abfragen zum Durchsuchen dieser Attribute konfigurieren. Zur Laufzeit entschied sich der Titel, einem Host, einer Sitzung oder suchen Sie eine.

Für Xbox One Vermittlung basiert auf der Server, und Player und Titel nicht mehr entscheiden Sie, ob hosten, oder suchen. Stattdessen wird jedes vorab gebildete Gruppe der Spieler erstellt eine Sitzung "Ticket" und sendet diese Sitzung mit dem Vermittlungsdienst. Dann wird der Dienst sucht nach anderen Sitzungen und kombiniert die Gruppen aus, um eine neue "Target" Sitzung bilden. Die Clients die Übereinstimmung benachrichtigt werden sollen, und führen die Dienstqualität (QoS)-Verbindung mit anderen Mitgliedern der Sitzung zu überprüfen, vor dem Starten des Spiels.


### <a name="xbox-live-compute-service"></a>Xbox Live-Serverdiensts

Die Xbox Live Compute-Dienst ist ein neues Angebot für Xbox One. Er ermöglicht es Entwicklern, die elastischen computefunktionen Leistungsfähigkeit der Cloud nutzen, und ermöglicht es größeren multiplayer-Szenarien als in einem Peer-zu-Peer-Netzwerk möglich waren. Weitere Informationen zu den Xbox Live Compute-Dienst finden Sie unter der Xbox Live Compute-Dokumentation in der Dokumentation xdk-Version.


## <a name="see-also"></a>Siehe auch

[Multiplayer-Sitzungsverzeichnis (MPSD)](multiplayer-session-directory.md)

[SmartMatch Vermittlung](smartmatch-matchmaking.md)

[Aktivitätsüberwachung in Echtzeit (RTA)-Dienst](../../real-time-activity-service/real-time-activity-service.md)

[Zuverlässigkeit](../../social-platform/people-system/reputation.md)

[Verwenden von Xbox Live Compute in Multiplayer-Spiele (verwaltete Partnerzugriff erforderlich)](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)
