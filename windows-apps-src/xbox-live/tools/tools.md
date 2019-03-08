---
title: Für die Xbox Live-Entwicklungstools
description: Informationen Sie zu Tools, die zum Entwickeln und Testen Ihrer Xbox Live Titel aktiviert bereitgestellt werden.
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.date: 6/13/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Tools, Player zurücksetzen, live-Ablaufverfolgung Analyzer, LTA, Xbox live-Konto-Tool,
ms.localizationpriority: medium
ms.openlocfilehash: ad43ecbd3bfd266d4a237253380bca223302fe54
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623515"
---
# <a name="development-tools-for-xbox-live"></a>Für die Xbox Live-Entwicklungstools

Dieser Abschnitt enthält verschiedene Tools, die Sie verwenden können, können Sie die für Xbox Live zu entwickeln. Viele der Tools finden Sie auf die [Xbox Live Developer Tools GitHub](https://github.com/Microsoft/xbox-live-developer-tools) Repository. Können Sie auch die [Entwicklungstools Bibliothek](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools) eigene benutzerdefinierte Tools erstellen. Alle eigenständigen Developer Tools heruntergeladen werden können, auf [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).

> [!NOTE]
> Die MatchSim und XboxLiveCompute-Tools, die im Download enthaltene können nur von verwalteten Partnern verwendet werden, oder Partner registriert wird, der [ ID@Xbox ](https://www.xbox.com/Developers/id) Programm. Weitere Informationen zu den verfügbaren Developer-Programmen finden Sie in der [Übersicht über die Developer-Programm](https://docs.microsoft.com/windows/uwp/xbox-live/developer-program-overview). 

## <a name="global-storage"></a>Globalen Speicher
Globale Title-Speicher wird verwendet, um Daten zu speichern, die jeder, z. B. Dienstplänen, Zuordnungen, Herausforderungen und Art von Ressourcen lesen kann. Es ist eine Art von [Titel Storage](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md). Das globale Storage-Tool wird verwendet, zum Verwalten von globalen Titel Speicher im Test-Sandboxes. Daten müssen immer noch auf "RETAIL" über Partner Center oder Xbox-Entwickler-Portal (XDP) veröffentlicht werden. Das Tool ist über die Befehlszeile verfügbar als Teil der [Entwicklungstools](https://aka.ms/xboxliveuwptools) Zip. Benutzerdefinierte Tools können erstellt werden, mit der [Entwicklungstools Bibliothek](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools).

## <a name="multiplayer-session-history-viewer"></a>Verlaufsanzeige Multiplayer-Sitzung
Multiplayer-Sitzung History Viewer bietet Ihnen die Möglichkeit, eine historische Zeitachse für alle Änderungen des Dokuments eine Multiplayer-Sitzung Verlauf (einschließlich gelöschte Dokumente) anzuzeigen. Mit diesem Tool erhalten Sie ein tieferes Verständnis der Funktionsweise mit Dokumenten MPSD Sitzung sich im Laufe der Zeit ändert. Es steht als eigenständiges Tool in der [Entwicklungstools](https://aka.ms/xboxliveuwptools) Zip.

## <a name="player-data-reset"></a>Zurücksetzen der Player-Daten
Zurücksetzen des Spielers Daten im Test-Sandboxes, kann das Tool zum Zurücksetzen von Player Daten verwendet werden. Sie können Daten wie z. B. zurücksetzen. Erfolge, Bestenlisten, Statistiken und Titel Verlauf. Das Tool ist über die Befehlszeile verfügbar als Teil der [Entwicklungstools](https://aka.ms/xboxliveuwptools) Zip. Benutzerdefinierte Tools können erstellt werden, mit der [Entwicklungstools Bibliothek](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools).

## <a name="xbox-live-developer-account"></a>Xbox Live Developer-Konto
Das Tool für die Xbox Live Developer-Konto wird zum Verwalten der Authentifizierung von einem Entwicklerkonto anzumelden. Dies ist erforderlich, andere Entwickler-Tools interagieren, die Entwickler Anmeldeinformationen, z. B. Player zurücksetzen und globalen Speicher erfordern. Das Tool ist über die Befehlszeile verfügbar als Teil der [Entwicklungstools](https://aka.ms/xboxliveuwptools) Zip.

## <a name="xbox-live-trace-analyzer"></a>Xbox Live-Ablaufverfolgung Analyzer
Mithilfe von [Xbox Live-Ablaufverfolgung Analyzer](analyze-service-calls.md), können Sie alle Aufrufe zu erfassen und analysieren Sie offline klicken Sie dann für alle Verstöße Muster aufzurufen. -Dienst Aufruf Ablaufverfolgung kann mit dem Befehlszeilentool Xbtrace aktiviert, oder durch die protokollaktivierung für Weitere erweiterte Szenarien. Aufruf direkt aus dem Titel Code nachverfolgen aktivieren, wird ebenfalls unterstützt. Das Tool ist über die Befehlszeile verfügbar als Teil der [Entwicklungstools](https://aka.ms/xboxliveuwptools) Zip.

## <a name="xbox-live-account-tool"></a>Xbox Live-Konto-Tool  
Die [Xbox Live-Kontotool](xbox-live-account-tool.md) soll beim Einrichten eines vorhandenen Testkonten für Spiele Testszenarios zu helfen. Sie können z. B. Xbox Live-Konto-Tool verwenden, um ein Konto des Gamertag zu ändern oder schnell 1000 Follower Freundesliste des Kontos hinzufügen. Das Tool ist über die Befehlszeile verfügbar als Teil der [Entwicklungstools](https://aka.ms/xboxliveuwptools) Zip.

## <a name="config-as-source"></a>Konfiguration als Quelle
[Konfiguration als Quelle](https://github.com/Microsoft/xbox-live-developer-tools/blob/master/CONFIGASSOURCE.md) ist eine Suite von Tools, die Microsoft um advanced-Benutzern zu ermöglichen, durch die Bereitstellung der offiziell unterstützten Tools und APIs für die Integration in unsere Dienste entwickelt. Diese Xbox Live-Dienste sind normalerweise für den Titel im Partner Center, einschließlich der Dienste, die zwischen der Bestenlisten, Erfolge, Webdienste und vertrauenden Seiten konfiguriert. Für viele Spieleentwickler reicht über Partner Center. Für fortgeschrittene Benutzer besteht jedoch der Wunsch, allgemeine Konfigurationsaufgaben in ihre eigenen Prozesse und Tools zu integrieren.  Konfiguration als Quelle dient zur Unterstützung dieser Szenarios, indem Sie Befehlszeilentools und neue APIs zur Unterstützung der benutzerdefinierten Integration in Ihre vorhandenen Workflows und Pipelines. Das Tool ist über die Befehlszeile verfügbar als Teil der [Entwicklungstools](https://aka.ms/xboxliveuwptools) Zip.
