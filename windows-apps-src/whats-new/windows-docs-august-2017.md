---
author: QuinnRadich
title: Neuigkeiten in der Windows-Dokumentation im August 2017 – Entwicklung von UWP-Apps
description: Neue Features, Videos und Entwicklerleitfäden in der Entwicklerdokumentation für Windows10 im August2017
keywords: Neuigkeiten, Update, Features, Anleitungen für Entwickler, Windows10, 1708
ms.author: quradic
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8ff4d66285770c483de6b7be846088135f0c1544
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "1015110"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>Neuigkeiten in der Windows-Entwicklerdokumentation im August 2017

Die Entwicklerdokumentation für die Windows-Plattform wird ständig mit Informationen über neue Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Entwicklerleitfäden und Videos wurden erst kürzlich bereitgestellt und enthalten neue oder aktualisierte Informationen für Windows-Entwickler.

Nach der [Installation der Tools und des SDKs](http://go.microsoft.com/fwlink/?LinkId=821431) unter Windows10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/your-first-app.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="windows-template-studio"></a>Windows Template Studio

Verwenden Sie die neue Erweiterung [Windows Template Studio](https://aka.ms/wtsinstall) für Visual Studio2017, um UWP-Apps mit den gewünschten Seiten, Frameworks und Features zu erstellen. Diese assistentenbasierte Umgebung implementiert bewährte Methoden und Muster, damit Sie Zeit sparen, wenn Sie Ihrer App Features hinzufügen.

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>Bedingte XAML

Sie können jetzt [bedingte XAML](../debug-test-perf/conditional-xaml.md) ausprobieren, um [versionsadaptive Apps](../debug-test-perf/version-adaptive-apps.md) zu erstellen. Mit bedingter XAML können Sie die Methode ApiInformation.IsApiContractPresent im XAML-Markup verwenden. Damit sind Sie in der Lage, im Markup nur dann Eigenschaften festzulegen und Objekte zu initialisieren, wenn die entsprechende API vorhanden ist, ohne Code-Behind zu verwenden.

### <a name="game-mode"></a>Spielmodus

Mithilfe der [Game Mode](https://msdn.microsoft.com/library/windows/desktop/mt808808)-APIs für die Universelle Windows-Plattform (UWP) sorgen Sie für ein optimiertes Spielerlebnis, indem Sie den Spielmodus in Windows10 nutzen. Diese APIs befinden sich im Header **&lt;expandedresources.h&gt;**.

![Spielmodus](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>Übermittlungs-API unterstützt Videotrailer und Spieloptionen

Die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) ermöglicht Ihnen jetzt, [Videotrailer](../monetize/manage-app-submissions.md#trailer-object) und [Spieloptionen](../monetize/manage-app-submissions.md#gaming-options-object) mit Ihrer App zu übermitteln.


## <a name="developer-guidance"></a>Anleitungen für Entwickler

### <a name="data-schemas-for-store-products"></a>Datenschemata für Store-Produkte

Wir haben den Artikel [Datenschemata für Store-Produkte](../monetize/data-schemas-for-store-products.md) hinzugefügt. Dieser Artikel enthält Schemata für auf den Store bezogenen Daten für mehrere Objekte im Namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx), z.B. [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) und [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense).

### <a name="desktop-bridge"></a>Desktop-Brücke

Wir haben zwei Anleitungen hinzugefügt, mit deren Hilfe Sie für Benutzer von Windows10 die neuen Möglichkeiten moderner Umgebungen bereitstellen können.

Informieren Sie sich im Artikel [Verbessern Sie Ihre Desktop-Anwendung für Windows10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance) über die richtigen Dateien, und schreiben Sie dann Code, um die UWP-Erfahrung für Windows10-Benutzer zu verbessern.  

Im Artikel [Erweitern Sie Ihre Desktop-Anwendung mit modernen UWP-Komponenten](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) erfahren Sie, wie Sie moderne XAML-Benutzeroberflächen und andere UWP-Funktionen verwenden, die in einem UWP-App-Container ausgeführt werden müssen.

### <a name="getting-started-with-point-of-service"></a>Erste Schritte mit Point Of Service-Geräten

Wir haben den neuen Leitfaden [Erste Schritte mit Point Of Service-Geräten](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-get-started) hinzugefügt. Es umfasst Themen wie Geräteenumeration, Überprüfen von Gerätefunktionen, Anfordern von Geräten und die gemeinsame Nutzung von Geräten. 

### <a name="xbox-live"></a>Xbox Live

Wir haben Dokumentationen für Xbox Live-Entwickler hinzugefügt. Die Informationen beziehen sich sowohl auf UWP-Spiele als auch auf Xbox Developer Kit (XDK)-Spiele.

Im [Xbox Live-Entwicklerhandbuch](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/) erfahren Sie, wie Sie die Xbox Live-APIs verwenden, um Ihr Spiel in das soziale Xbox Live-Spielenetzwerk zu integrieren.

Im [Xbox Live Creators-Programm](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) können alle UWP-Spielentwickler für Xbox-Live geeignete Spiele entwickeln und veröffentlichen, sowohl für den PC als auch für Xbox One.

Weitere Informationen über die Programme und Features für Xbox Live-Entwickler finden Sie unter [Programmübersicht für Xbox Live-Entwickler](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview).

## <a name="videos"></a>Videos

### <a name="mixed-reality"></a>Mixed Reality

Für [Microsoft HoloLens – Kurs 250](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality_250) wurde eine Reihe von neuen Videolernprogrammen veröffentlicht. Sie finden in diesen Videokursen Informationen zum Erstellen gemeinsamer Erfahrungen auf Mixed Reality-Geräten. Vorausgesetzt wird, dass Sie die Tools bereits installiert haben und mit den Grundlagen der Entwicklung für Mixed Reality vertraut sind.

### <a name="narrator-and-dev-mode"></a>Sprachausgabe und Entwicklermodus

Sie wissen möglicherweise bereits, dass Sie die [Sprachausgabe](https://support.microsoft.com/help/22798/windows-10-narrator-get-started) verwenden können, um zu testen, wie gut Ihre App auf dem Bildschirm lesen kann. Die Sprachausgabe bietet aber auch einen Entwicklermodus für eine gute visuelle Darstellung der Informationen, die für die Sprachausgabe verfügbar sind. [Sehen Sie sich das Video an](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode), und erfahren Sie dann mehr über [Sprachausgabe im Entwicklermodus](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode).

### <a name="windows-template-studio"></a>Windows Template Studio

Eine ausführlichere Übersicht über Windows Template Studio erhält [dieses Video](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio). Wenn Sie fertig sind, [installieren Sie die Erweiterung](https://aka.ms/wtsinstall) oder [sehen Sie sich den Quellcode und die Dokumentation an](https://aka.ms/wtsinstall).