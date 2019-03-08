---
title: Was ist neu in der Xbox Live
description: Neuigkeiten für die Xbox Live SDK
ms.date: 10/23/2018
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 33e5b6afbf0d60679bfce1789be2d965fd881f1c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614115"
---
# <a name="whats-new-for-xbox-live"></a>Neuigkeiten für Xbox Live
Sie können auch überprüfen, die [Xbox Live-API-GitHub-Commit-Verlauf](https://github.com/Microsoft/xbox-live-api/commits/master) alle den codeänderungen den Xbox Live-APIs anzeigen.

## <a name="in-this-article"></a>In diesem Artikel

* [Juni 2018](#june-2018)
* [August 2017](#august-2017)
* [Juli 2017](#july-2017)
* [Juni 2017](#june-2017)
* [Mai 2017](#may-2017)
* [April 2017](#april-2017)
* [März 2017](#march-2017)
* [Archiviert](#archived)

## <a name="june-2018"></a>Juni 2018

### <a name="xbox-live-features"></a>Xbox Live-Features

#### <a name="c-api-layer-for-xsapi"></a>C-API-Ebene für XSAPI

C-APIs sind nun für einige Xbox Live-Features verfügbar. Die neue API-Schicht enthält eine Reihe von Vorteilen für die unterstützten Funktionen, einschließlich benutzerdefinierter Speicherverwaltung, manuelle Threadverwaltung für asynchrone Vorgänge und eine neue HTTP-Bibliothek.

Weitere Informationen finden Sie unter [Xbox Live-C-APIs](../xsapi-flat-c.md).

## <a name="august-2017"></a>August 2017

### <a name="xbox-live-features"></a>Xbox Live-Funktionen

#### <a name="in-game-clubs"></a>In-Game-clubs

Entwickler können jetzt "in-Game-Clubs" erstellen. In-Game-Clubs unterscheiden von standard Xbox Kreuz bei, dass sie von einem Entwickler vollständig anpassbar und werden, innerhalb und außerhalb des Spiels verwendet können. Als Entwickler von Spielen können sie Sie schnell alle Arten von permanenten fassen Sie Szenarien in Ihre Spiele wie Teams, Clans, Squads, Guilds usw. zu erstellen, die Ihrer individuellen Anforderungen entsprechen.

Xbox live-Member in-Game-Clubs außerhalb Ihres Spiels über bleiben alle Xbox-Umgebung zugreifen können miteinander und an Ihrem Spiel mithilfe von Club-Features wie Chat verbunden, die feed-, LFG und Mixer frei auf der Xbox-Konsole, PC oder IOS-oder Android-Geräte.

APIs sind zum Erstellen und Verwalten von in-Game-Clubs direkt aus in Ihr Spiel verfügbar. Diese APIs sind in den Xbox::services::clubs-Namespace vorhanden.

## <a name="july-2017"></a>Juli 2017

### <a name="xbox-live-features"></a>Xbox Live-Funktionen

#### <a name="tournaments"></a>Turniere

Neue APIs wurden hinzugefügt, um Turniere zu unterstützen. Sie können jetzt die Xbox::services::tournaments::tournament_service-Klasse verwenden, Zugriff auf die Turniere aus dem Titel des.
Diese neue Turnier APIs ermöglichen die folgenden Szenarien:

* Fragen Sie den Dienst, um alle vorhandenen Turniere für den aktuellen Titel finden.
* Abrufen von Details zu ein Turnier aus dem Dienst.
* Fragen Sie den Dienst, um eine Liste der Teams für ein Turnier abzurufen.
* Abrufen von Details zu den Teams für ein Turnier aus dem Dienst.
* Nachverfolgen von Änderungen mithilfe von Echtzeit-Aktivität (RTA) Abonnements Turniere und Teams.

## <a name="june-2017"></a>Juni 2017

### <a name="xbox-live-features"></a>Xbox Live-Funktionen

#### <a name="game-chat-2"></a>Game Chat 2

Eine aktualisierte und verbesserte Version von Spiel Chat ist jetzt verfügbar. Weitere Informationen finden Sie unter den [Spiel, Chat-2 (Übersicht)](../multiplayer/chat/game-chat-2-overview.md).

### <a name="xbox-live-tools"></a>Xbox Live-tools

#### <a name="xbox-live-powershell-module"></a>Xbox Live-PowerShell-Modul

* PowerShell-Module wurden hinzugefügt, um das Wechseln von Sandkästen auf Ihrem Entwicklungscomputer zu vereinfachen. Weitere Informationen finden Sie unter [Tools](../tools/tools.md)

#### <a name="bug-fixes"></a>Fehlerbehebungen

* Verschiedene Fehlerbehebungen. Überprüfen Sie die [GitHub-Commit-Verlauf](https://github.com/Microsoft/xbox-live-api/commits/master) für eine vollständige Liste.

## <a name="may-2017"></a>Mai 2017

### <a name="xbox-services-apis"></a>Xbox-Services-APIs

#### <a name="multiplayer"></a>Multiplayer

* Bei der Suche jetzt Abfragen enthält die benutzerdefinierte Eigenschaften in der Antwort.

#### <a name="bug-fixes"></a>Fehlerbehebungen

* Korrektur von "Ungültige Json", die anstelle einer gültigen HTTP-Fehlercode zurückgegeben wird.

## <a name="april-2017"></a>April 2017

### <a name="xbox-services-apis"></a>Xbox-Services-APIs

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Die Xbox Live-APIs wurden zur Unterstützung von Visual Studio 2017, für die universelle Windows-Plattform (UWP) und Xbox One Titel aktualisiert.

#### <a name="tournaments"></a>Turniere

Neue APIs wurden hinzugefügt, um Turniere zu unterstützen. Sie können nun die `xbox::services::tournaments::tournament_service` Klasse, um den Titel den Turniere-Dienst zugreifen.

Diese neue Turnier APIs ermöglichen die folgenden Szenarien:

* Fragen Sie den Dienst, um alle vorhandenen Turniere für den aktuellen Titel finden.
* Abrufen von Details zu ein Turnier aus dem Dienst.
* Fragen Sie den Dienst, um eine Liste der Teams für ein Turnier abzurufen.
* Abrufen von Details zu den Teams für ein Turnier aus dem Dienst.
* Nachverfolgen von Änderungen mithilfe von Echtzeit-Aktivität (RTA) Abonnements Turniere und Teams.

## <a name="march-2017"></a>März 2017

### <a name="xbox-services-api"></a>Xbox-Services-API

#### <a name="data-platform-2017"></a>Datenplattform 2017

Wir haben eine vereinfachte Stats-API eingeführt.  Bisher mussten Sie zum Senden von Ereignissen, die Stat für XDP oder über das Partner Center definierten Regeln entspricht, und diese würde die Stat-Werte in der Cloud aktualisieren.  Wir bezeichnen dieses Modell als Stats 2013.

Mit Statistiken 2017 ist der Titel des jetzt Kontrolle über die Stat-Werte.  Rufen Sie einfach eine API mit dem die letzte Stat-Wert, und wird, die an den Dienst direkt ohne die Notwendigkeit der Ereignisse gesendet.  Diese verwendet die neue `StatsManager` API erfahren Sie mehr dazu in [Player-Statistiken](../leaderboards-and-stats-2017/player-stats.md)

#### <a name="github"></a>GitHub

Xbox Live-API (XSAPI) steht jetzt auf GitHub unter [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api).  Verwenden die Binärdateien, die enthalten sind, mit der xdk-Version oder als NuGet Pakete wird weiterhin empfohlen, aber Sie Willkommen bei der Quelle verwendet werden, und wir freuen uns über Beiträge von Source Code.  

### <a name="xbox-live-creators-program"></a>Xbox Live Creators-Programm

Die Xbox Live Creators-Programm ist eine Developer-Programm bietet eine Teilmenge der Xbox Live-Funktionalität auf eine breitere Zielgruppe der Entwickler erstellt.  Wenn Sie bereits in sind die ID@Xbox Programm dies keine Auswirkungen auf Sie haben.

Erfahren Sie mehr über das Programm in [Übersicht über die Developer-Programm](../developer-program-overview.md).

### <a name="documentation"></a>Dokumentation

Es gibt die folgenden neuen Artikeln:

| Artikel | Beschreibung |
|---------|-------------|
|[Xbox Live-Dienstkonfiguration](../xbox-live-service-configuration.md) | Aktualisierte Informationen zum Ausführen der Dienstkonfiguration für Ihre Xbox Live-Titel
| [Konfigurieren von Xbox Live in Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | Neue Informationen zu Unity-Setup für Entwickler für Xbox Live Creators-Programm |
| [Xbox Live-Sandboxes](../xbox-live-sandboxes.md) | Eine vereinfachte Anleitung an Sandboxen Xbox Live und inhaltsisolation |
| [Xbox Live-Testkonten](../xbox-live-test-accounts.md) | Informationen testen, Konten arbeiten, und wie sie im Partner Center erstellt |

## <a name="archived"></a>Archiviert

* [Dezember 2016](1612-whats-new.md)
* [November 2016](1611-whats-new.md)
* [August 2016](1608-whats-new.md)
* [Juni 2016](1606-whats-new.md)
* [April 2016](1603-whats-new.md)
* [März 2016](1603-whats-new.md)
* [Februar 2016](1602-whats-new.md)
* [Oktober 2015](1510-whats-new.md)
* [Im September 2015](1509-whats-new.md)
* [August 2015](1508-whats-new.md)
* [Juni 2015](1506-whats-new.md)
