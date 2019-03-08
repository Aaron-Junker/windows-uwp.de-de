---
title: Neuerungen für die Xbox Live SDK – Februar 2016
description: Neuerungen für die Xbox Live SDK – Februar 2016
ms.assetid: 7ff34ea4-f07d-4584-98e4-13977994ccca
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c13b2893397a0d84e8919146435ece338bc1afde
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594735"
---
# <a name="whats-new-for-the-xbox-live-sdk---february-2016"></a>Neuerungen für die Xbox Live SDK – Februar 2016

Informieren Sie sich die [– Oktober 2015 Neuigkeiten](1510-whats-new.md) Artikel was 1510 hinzugefügt wurde

## <a name="os-and-tool-support"></a>Unterstützung für Betriebssystem und Tools
Das Xbox Live-SDK unterstützt Windows 10 RTM [Version 10.0.10240] und Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="throttling"></a>Drosselung
- Differenzierte Drosselung werden in Kürze auf die meisten Xbox Live Services eingeführt.  Xbox-Dienst-API (XSAPI) automatisch Behandeln von Wiederholungen und informiert Sie über Aufrufe, die während der Entwicklung gedrosselt werden.  Weitere Informationen finden Sie der [Best Practices Aufrufen von Xbox Live](../using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) Artikel in der Dokumentation.

## <a name="leaderboards"></a>Bestenlisten
- Für mehrere Spalten Bestenlisten können jetzt von der GetLeaderboard-API zugegriffen werden. Wenn Sie einen Vektor mit den Namen der zusätzlichen Spalten angeben, wird der Vektor, der Spalten, auf dem Ergebnis ausgefüllt werden, wenn diese Spalten vorhanden sind.

## <a name="documentation"></a>Dokumentation
- [Application Insights](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/application-insights) Dokumentation ist hier.  Sie können Application Insights mit einem kostenlosen Azure-Konto verwenden, um Datenplattform-Ereignisse in nahezu in Echtzeit anzuzeigen.  Diese Funktion ist derzeit nur für UWP-Anwendungen, die unter Windows 10 ausgeführt werden, auf dem Desktop zur Verfügung.
- Aktualisierte Dokumentation auf der Xbox-Common-Ereignistool für UWP-Entwickler erörtert, wie Sie für das Senden von Ereignissen der Datenplattform Wrapper erstellt werden.  Beachten Sie, dass dieser Schritt optional ist, und Sie die WriteInGameEvent-API verwenden weiterhin können, falls gewünscht.
- Verwendung von Fiddler zur Debugereignisse Data-Plattform, und stellen Sie sicher, dass sie ordnungsgemäß gesendet werden.  Dies ist nur für UWP-Ereignisse.
- Informationen zum Erfassen von Protokollen für das Live-Ablaufverfolgung Analyzer-Tool ist verfügbar.  Finden Sie unter den [analysieren Sie Aufrufe von Xbox Live Services](../tools/analyze-service-calls.md) Artikel.
