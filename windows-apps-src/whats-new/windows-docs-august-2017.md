---
title: Neuerungen in der Windows-Dokumentation im August 2017 – Entwickeln von UWP-Apps
description: Neue Features, Videos und Entwicklerleitfäden wurden der Entwicklerdokumentation für Windows 10 im August 2017 hinzugefügt.
keywords: Neuigkeiten, Update, Features, Anleitungen für Entwickler, Windows 10, 1708
ms.date: 08/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b4cc5f10ba23c942851f34ef64afd887d265dba1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259780"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>Neuerungen in der Windows-Entwicklerdokumentation im August 2017

Die Entwicklerdokumentation für die Windows-Plattform wird ständig mit Informationen zu neuen Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Entwicklerleitfäden und Videos wurden erst kürzlich bereitgestellt und enthalten neue oder aktualisierte Informationen für Windows-Entwickler.

Nach der [Installation der Tools und des SDKs](https://developer.microsoft.com/windows/downloads#_blank) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/your-first-app.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="windows-template-studio"></a>Windows Template Studio

Verwenden Sie die neue Erweiterung [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio) für Visual Studio 2019, um UWP-Apps mit den gewünschten Seiten, Frameworks und Features zu erstellen. Diese assistentenbasierte Umgebung implementiert bewährte Methoden und Muster, damit Sie Zeit sparen, wenn Sie Ihrer App Features hinzufügen.

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>Bedingtes XAML

Sie können jetzt [bedingte XAML](../debug-test-perf/conditional-xaml.md) ausprobieren, um [versionsadaptive Apps](../debug-test-perf/version-adaptive-apps.md) zu erstellen. Mit bedingter XAML können Sie die Methode ApiInformation.IsApiContractPresent im XAML-Markup verwenden. Damit sind Sie in der Lage, im Markup nur dann Eigenschaften festzulegen und Objekte zu initialisieren, wenn die entsprechende API vorhanden ist, ohne Code-Behind zu verwenden.

### <a name="game-mode"></a>Spielmodus

Mithilfe der [Game Mode](https://docs.microsoft.com/previous-versions/windows/desktop/gamemode/game-mode-portal)-APIs für die Universelle Windows-Plattform (UWP) sorgen Sie für ein optimiertes Spielerlebnis, indem Sie den Spielmodus in Windows 10 nutzen. Diese APIs befinden sich im Header **&lt;expandedresources.h&gt;** .

![Spielmodus](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>Übermittlungs-API unterstützt Videotrailer und Spieloptionen

Die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) ermöglicht Ihnen jetzt, [Videotrailer](../monetize/manage-app-submissions.md#trailer-object) und [Spieloptionen](../monetize/manage-app-submissions.md#gaming-options-object) mit Ihrer App zu übermitteln.


## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="data-schemas-for-store-products"></a>Datenschemas für Store-Produkte

Wir haben den Artikel [Datenschemas für Store-Produkte](../monetize/data-schemas-for-store-products.md) hinzugefügt. Dieser Artikel enthält Schemas für auf den Store bezogenen Daten für mehrere Objekte im Namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store), z.B. [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) und [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense).

### <a name="desktop-bridge"></a>Desktop-Brücke

Wir haben zwei Anleitungen hinzugefügt, mit deren Hilfe Sie für Benutzer von Windows 10 die neuen Möglichkeiten moderner Umgebungen bereitstellen können.

Informieren Sie sich im Artikel [Verbessern Ihrer Desktopanwendung für Windows 10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance) über die richtigen Dateien, und schreiben Sie dann Code, um die UWP-Erfahrung für Benutzer von Windows 10 zu verbessern.  

Im Artikel [Erweitern Ihrer Desktopanwendung mit modernen UWP-Komponenten](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) erfahren Sie, wie Sie moderne XAML-Benutzeroberflächen und andere UWP-Funktionen verwenden, die in einem UWP-App-Container ausgeführt werden müssen.

### <a name="getting-started-with-point-of-service"></a>Erste Schritte mit Point Of Service-Geräten

Wir haben den neuen Leitfaden [Erste Schritte mit Point Of Service-Geräten](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-get-started) hinzugefügt. Er umfasst Themen wie Geräteenumeration, Überprüfen von Gerätefunktionen, Anfordern von Geräten und die gemeinsame Nutzung von Geräten. 

### <a name="xbox-live"></a>Xbox Live

Wir haben Dokumentationen für Xbox Live-Entwickler hinzugefügt. Die Informationen beziehen sich sowohl auf UWP-Spiele als auch auf XDK-Spiele (Xbox Developer Kit).

Im [Xbox Live-Entwicklerhandbuch](https://docs.microsoft.com//gaming/xbox-live/index) erfahren Sie, wie Sie die Xbox Live-APIs verwenden, um Ihr Spiel in das soziale Xbox Live-Spielenetzwerk zu integrieren.

Im [Xbox Live Creators-Programm](https://docs.microsoft.com//gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) können alle UWP-Spielentwickler für Xbox-Live geeignete Spiele entwickeln und veröffentlichen, sowohl für den PC als auch für Xbox One.

Weitere Informationen zu den Programmen und Features für Xbox Live-Entwickler finden Sie unter [Programmübersicht für Xbox Live-Entwickler](https://docs.microsoft.com//gaming/xbox-live/developer-program-overview).

## <a name="videos"></a>Videos

### <a name="mixed-reality"></a>Mixed Reality

Für [Microsoft HoloLens: Kurs 250](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality_250) wurde eine Reihe von neuen Videolernprogrammen veröffentlicht. Sie finden in diesen Videokursen Informationen zum Erstellen gemeinsamer Erfahrungen auf Mixed Reality-Geräten. Vorausgesetzt wird, dass Sie die Tools bereits installiert haben und mit den Grundlagen der Entwicklung für Mixed Reality vertraut sind.

### <a name="narrator-and-dev-mode"></a>Sprachausgabe und Entwicklermodus

Sie wissen möglicherweise bereits, dass Sie die [Sprachausgabe](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator) verwenden können, um die Lesemodusfunktionen Ihrer App zu testen. Die Sprachausgabe bietet aber auch einen Entwicklermodus für eine gute visuelle Darstellung der Informationen, die für die Sprachausgabe verfügbar sind. [Sehen Sie sich das Video an](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode), und erfahren Sie mehr über [Sprachausgabe im Entwicklermodus](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode).

### <a name="windows-template-studio"></a>Windows Template Studio

[Dieses Video](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio) enthält eine ausführlichere Übersicht über Windows Template Studio. Wenn Sie fertig sind, [installieren Sie die Erweiterung](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio), oder [sehen Sie sich den Quellcode und die Dokumentation an](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio).