---
title: Neuerungen für die Xbox Live SDK – März 2017
description: Neuerungen für die Xbox Live SDK – März 2017
ms.assetid: 03180585-6f87-4929-acfc-750bd78988a0
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: be8127e01d8eaae96a1d71f71967a653c00b0280
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595345"
---
# <a name="whats-new-for-the-xbox-live-sdk---march-2017"></a>Neuerungen für die Xbox Live SDK – März 2017

Informieren Sie sich die [– Dezember 2016 Neuigkeiten](1612-whats-new.md) Artikel was hinzugefügt wurde im Dezember 2016-Release.

## <a name="xbox-services-api"></a>Xbox-Services-API

### <a name="data-platform-2017"></a>Datenplattform 2017

Wir haben eine vereinfachte Stats-API eingeführt.  Bisher mussten Sie zum Senden von Ereignissen, die Stat für XDP oder über das Partner Center definierten Regeln entspricht, und diese würde die Stat-Werte in der Cloud aktualisieren.  Wir bezeichnen dieses Modell als Stats 2013.

Mit Statistiken 2017 ist der Titel des jetzt Kontrolle über die Stat-Werte.  Rufen Sie einfach eine API mit dem die letzte Stat-Wert, und wird, die an den Dienst direkt ohne die Notwendigkeit der Ereignisse gesendet.  Diese verwendet die neue `StatsManager` API erfahren Sie mehr dazu in [Player-Statistiken](../leaderboards-and-stats-2017/player-stats.md)

### <a name="github"></a>GitHub

Xbox Live-API (XSAPI) steht jetzt auf GitHub unter [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api).  Verwenden die Binärdateien, die enthalten sind, mit der xdk-Version oder als NuGet Pakete wird weiterhin empfohlen, aber Sie Willkommen bei der Quelle verwendet werden, und wir freuen uns über Beiträge von Source Code.  

## <a name="xbox-live-creators-program"></a>Xbox Live Creators-Programm

Die Xbox Live Creators-Programm ist eine Developer-Programm bietet eine Teilmenge der Xbox Live-Funktionalität auf eine breitere Zielgruppe der Entwickler erstellt.  Wenn Sie bereits in sind die ID@Xbox Programm dies keine Auswirkungen auf Sie haben.

Erfahren Sie mehr über das Programm in [Übersicht über die Developer-Programm](../developer-program-overview.md).

## <a name="documentation"></a>Dokumentation

Es gibt die folgenden neuen Artikeln

| Artikel | Beschreibung |
|---------|-------------|
|[Xbox Live-Dienstkonfiguration](../xbox-live-service-configuration.md) | Aktualisierte Informationen zum Ausführen der Dienstkonfiguration für Ihre Xbox Live-Titel
| [Konfigurieren von Xbox Live in Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | Neue Informationen zu Unity-Setup für Entwickler für Xbox Live Creators-Programm |
| [Xbox Live-Sandboxes](../xbox-live-sandboxes.md) | Eine vereinfachte Anleitung an Sandboxen Xbox Live und inhaltsisolation |
| [Xbox Live-Testkonten](../xbox-live-test-accounts.md) | Informationen testen, Konten arbeiten, und wie sie im Partner Center erstellt |
