---
title: Neuerungen für die Xbox Live SDK – August 2016
description: Neuerungen für die Xbox Live SDK – August 2016
ms.assetid: fa52e7bd-2c2c-4c25-94ab-761036a7ca79
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 2a498fea1ed0974935a273c9ee72ba2c95d15959
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627905"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2016"></a>Neuerungen für die Xbox Live SDK – August 2016

Informieren Sie sich die [– Juni 2016 Neuigkeiten](1606-whats-new.md) Artikel was hinzugefügt wurde im Release vom Juni 2016.

## <a name="os-and-tool-support"></a>Unterstützung für Betriebssystem und Tools
Das Xbox Live-SDK unterstützt Windows 10 RTM [Version 10.0.10240] und Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="documentation"></a>Dokumentation
- Wenn Sie eine UWP-Anwendung schreiben, und die Möglichkeit, Benutzer einladen, ein Spiel implementieren, sind Anweisungen, auf die ```.appxmanifest``` in erforderlichen Änderungen [konfigurieren Ihre AppXManifest für Multiplayer](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md).  Dies wurde zuvor erläutert, auf die [Foren](https://forums.xboxlive.com) und [Xbox live Code aus der Zeitraum für die Uwp Portieren](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) Artikel
- Die [Einführung in sozialen Netzwerken Manager](../social-platform/intro-to-social-manager.md) Artikel wurde aktualisiert, um die aktuelle API-Änderungen widerspiegeln, und geben Sie weitere Informationen zu Rückgabecodes für einige der Funktionen.

## <a name="unity-samples"></a>Unity-Beispiele
Einige neue Beispiele wurden für Unity-Entwickler, die Schreiben einer UWP-Anwendung hinzugefügt.
- Steht jetzt eine Unity-Version des Beispiels sozialer Netzwerke, Sie finden diese unter dem Verzeichnis für Beispiele/Social Media-/Unity.
- Es gibt auch ein Beispiel, das beschreibt, wie verbundene Speicher verwenden.  Das Beispiel finden Sie Beispiele/GameSave/Unity.
Es ist eine Unity-Version von AchievementsLeaderboard unter Samples/AchievementsLeaderboard/Unity

## <a name="social-manager"></a>Soziale Medien-Manager
Zusätzlich zu den oben genannten Dokumentationsupdates gibt es einige neue APIs, die hinzugefügt wurden.  Diese werden nachfolgend beschrieben, und sehen Sie ausführlicher social_manager.h

- Hinzugefügte neue API, die Aktualisierung der sozialen Gruppen ohne Neuerstellung von ermöglicht:

```cpp
    _XSAPIIMP xbox_live_result<void> update_social_user_group(
        _In_ const std::shared_ptr<xbox_social_user_group>& group,
        _In_ const std::vector<string_t>& users
        );
```
- Eine abgeschlossene Aktualisierung der soziale Gruppe wird angegeben, indem die ```social_user_group_updated``` Ereignis


## <a name="multiplayer"></a>Multiplayer
Verbesserte Sitzung Durchsuchen ist jetzt verfügbar und können Sie neue Multiplayer-APIs um zu nutzen.

Verwenden die neuen APIs, können Sie Filtern für Tags, Zeichenfolgen und andere umfangreichen Daten, damit Benutzer eine Sitzung leichter zu finden, die sie wiedergeben möchten.

Wir veröffentlichen eine umfassendere Dokumentation in den kommenden Monaten, aber kurz können Sie jetzt zuordnen "Suchhandle" eine MPSD-Sitzung mit ```set_search_handle``` und anschließend können die Benutzer für Sitzungen, die einen stabilen Mechanismus für die Filterung durch den die Title-Aufruf durchsuchen ```get_search_handles```

Die neuen APIs sind unten aufgeführt.  Bitte versuchen Sie es, und wenn Probleme auftreten, Posten Sie einen Support-Thread in der [Foren](https://forums.xboxlive.com) oder wenden Sie sich an Ihre DAM.  Haben wir Beispiele dafür, wie Sie diese APIs in Kürze zu verwenden.

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> set_search_handle(
    _In_ multiplayer_search_handle_request searchHandleRequest
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<std::vector<multiplayer_search_handle_details>>> get_search_handles(
    _In_ const string_t& serviceConfigurationId,
    _In_ const string_t& sessionTemplateName,
    _In_ const string_t& orderBy,
    _In_ bool orderAscending,
    _In_ const string_t& searchQuery
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> clear_search_handle(_In_ const string_t& handleId);
```

### <a name="xbox-integrated-multiplayer"></a>Xbox-integrierten Multiplayer-Spiele

Wir haben für die Xbox integrierte Multiplayer-Spiele (XIM) API-Dokumentation enthalten.  Die API selbst wird in einer späteren Version des Xbox Live SDK verfügbar sein, aber die Dokumentation und den Header vorgenommen werden als Vorschau verfügbar.

XIM ist eine eigenständige Benutzeroberfläche für Multiplayer-Netzwerk und Chat Echtzeitkommunikation an Ihrem Spiel über die Leistungsfähigkeit von Xbox Live-Dienste auf einfache Weise hinzuzufügen.

Diese Vorschau der API Dokumentation wird hier gemeinsam genutzt, um Feedback von Kunden und Umfragen zu fördern. Wir haben über diese API weiter oben auf der Xfest 2016, und sehen Sie archivierte [Präsentationsmaterial auf der Entwicklerwebsite verwalteten Partner](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2016.aspx) aus den Vortrag "Einsetzbare Multiplayer Networking und Chat". Beachten Sie, dass dieser Preview-Dokumentation nur für die C++-API. WinRT-Entsprechungen für C# und anderen Sprachen werden später im Jahr veröffentlicht.

Wenn Sie XIMs Funktionen interessiert sind, haben Sie Feedback oder andere Fragen zu diesem Projekt Sie gerne auf veröffentlichte die [Xbox-Entwicklerforum](https://forums.xboxlive.com/) oder wenden Sie sich über Ihren Developer Account Manager.

Sie sehen, dass dieser neue Dokumentation in xbox_integrated_multiplayer.chm im Verzeichnis Docs des Xbox Live SDK.  Die Include-Datei ist als Vorschau im \include\xim\XboxIntegratedMultiplayer.h verfügbar.  
