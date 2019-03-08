---
title: Neuerungen für die Xbox Live SDK – September 2015
description: Neuerungen für die Xbox Live SDK – September 2015
ms.assetid: 84b82fde-f6f3-4dc2-b2df-c7c7313a2cc3
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: c618a738dc2670d3d3de1fa2f4c4108c24130eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602275"
---
# <a name="whats-new-for-the-xbox-live-sdk---september-2015"></a>Neuerungen für die Xbox Live SDK – September 2015

Informieren Sie sich die [– August 2015 Neuigkeiten](1508-whats-new.md) Artikel was hinzugefügt wurde im Release vom August 2015.

Die September-Version des Xbox Live SDK umfasst die folgenden Updates:

## <a name="os-and-tool-support"></a>Unterstützung für Betriebssystem und Tools ##
Das Xbox Live-SDK unterstützt Windows 10 RTM [Version 10.0.10240] und Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="contextual-search-apis"></a>Kontextbezogene Such-APIs
* Aktivieren Sie Titel oder Anwendung von Broadcasts von Ihrem Spiel(e) Real-Time-Stats Ihrer Wahl zu durchsuchen.
* Informieren Sie sich die neuen APIs im Microsoft::Xbox::Services::ContextualSearch

## <a name="app-insights-for-events"></a>Application Insights für Ereignisse

| Hinweis |
|------|
| Application Insights gilt nur für UWP-Titel.  Wenn Sie einen Titel xdk-Version entwickeln, gilt in diesem Abschnitt nicht für Sie |

<p/>

* Mit write_in_game_event() geschriebene Ereignisse können mithilfe von AppInsights angezeigt werden
* Dokumentation ist dazu jedoch in Zukunft in der Zwischenzeit wenden Sie sich mit Ihrem DAM zugreifen

## <a name="logging"></a>Protokollierung
* service_call_logging_config in xbox::services::experimental
* Starten und Beenden von ablaufverfolgungen über xbTrace.exe in der Konsole, müssen Sie Register_for_protocol_activation für die Service_call_logging_config-Klasse aufrufen.  Stellen Sie diesen Aufruf einmal während der Initialisierung des Spiels.

## <a name="resync-for-rta"></a>Erneute Synchronisierung für die RTA
* Erneute Synchronisierung tritt auf, wenn der RTA-Dienst ist der Auffassung, dass der Benutzerinformationen veraltet sind
* Titel sollten entsprechende HTTP-Aufrufe für die Abonnements aufrufen, denen sie abonniert haben
* Titel müssen nicht erneut abonnieren
* Hinzugefügte xbox::services::real_time_activity_service::add_resync_handler
* Entfernte xbox::services::real_time_activity_service::remove_resync_handler
* Hinzugefügte http_status_429_too_many_requests
* Dieser Fehler bedroht werden angezeigt werden, wenn Sie ein Titel für das Senden von http-Anforderungen, die zu viele eingeschränkt wird

## <a name="documentation"></a>Dokumentation
* Migrieren zu Xbox Live-Dienste-API 2.0
* Fehlerbehandlung
* Xbox Live-Authentifizierung in Windows 10
* Kontextbezogene Suche
