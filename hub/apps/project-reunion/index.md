---
description: Erfahren Sie mehr über Project Reunion, welche Vorteile es für Entwickler bietet, was jetzt für Entwickler bereitsteht und wie Sie Feedback geben können.
title: Erstellen von Windows-Desktop-Apps mit Project Reunion 0.5
ms.topic: article
ms.date: 03/19/2021
keywords: Windows Win32, Desktopentwicklung, Project Reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 6674b18c0a042b857b14972c55e7e57f01acc03d
ms.sourcegitcommit: df14e7768acdb243190e3418db5afa5d65c5ff88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107574615"
---
# <a name="build-desktop-windows-apps-with-project-reunion-05"></a>Erstellen von Windows-Desktop-Apps mit Project Reunion 0.5

Project Reunion ist ein Satz von neuen Entwicklerkomponenten und -tools, die die nächste Weiterentwicklung der Windows-App-Entwicklungsplattform darstellen. Project Reunion bietet einen einheitlichen Satz von APIs und Tools, die von jeder Desktop-Apps auf einer breiten Palette von Zielversionen des Windows 10-Betriebssystems auf konsistente Weise verwendet werden können.

Project Reunion ersetzt nicht die bestehenden Windows-Desktop-App-Plattformen und Frameworks wie .NET (einschließlich Windows Forms und WPF) und C++/Win32. sondern ergänzt diese bestehenden Plattformen mit einem gemeinsamen Satz von APIs und Tools, auf die sich Entwickler plattformübergreifend verlassen können. Weitere Informationen finden Sie unter [Vorteile von Project Reunion](#benefits-of-project-reunion-for-windows-developers).

> [!NOTE]
> Project Reunion 0.5 wird für die Verwendung in als MSIX-Paket vorliegenden Desktop-Apps (C#/.NET 5 oder C++/Win32) in Produktionsumgebungen unterstützt. Gepackte Desktop-Apps, die Project Reunion 0.5 verwenden, können im Microsoft Store veröffentlicht werden. Für UWP-Apps ist Project Reunion 0.5 nur als Vorschau verfügbar. Dieses Release wird für UWP-Apps, die in Produktionsumgebungen verwendet werden, nicht unterstützt.
>
>**Project Reunion** ist ein Codename, der in einem zukünftigen Release geändert werden kann.

## <a name="overview"></a>Übersicht

Project Reunion bietet eine Erweiterung für Visual Studio 2019, die für die Verwendung von Project Reunion-Komponenten in neuen Projekten konfigurierte Projektvorlagen enthält. Die Project Reunion-Bibliotheken sind auch über ein NuGet-Paket verfügbar, das Sie in bestehenden Projekten installieren können. Weitere Informationen finden Sie unter [Erste Schritte mit Project Reunion](get-started-with-project-reunion.md).

Nachdem Sie eine App erstellt haben, die Project Reunion verwendet, können Sie sie auf anderen Computern bereitstellen. Weitere Informationen finden Sie unter [Bereitstellen von Apps, die Project Reunion verwenden](deploy-apps-that-use-project-reunion.md).

Project Reunion 0.5 enthält die folgenden Sätze von APIs und Komponenten, die Sie in Ihren Apps verwenden können. Weitere Informationen zu den zukünftigen Plänen der Integration weiterer Komponenten in Project Reunion finden Sie [hier](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md).

| Komponente | BESCHREIBUNG |
|---------|-------------|
| [Windows-UI-Bibliothek 3](../winui/winui3/index.md) | Windows-UI-Bibliothek (WinUI) 3 ist die nächste Generation der Benutzererfahrungsplattform (User Experience, UX) für Windows-Apps. Dieses Release enthält Visual Studio-Projektvorlagen, die Ihnen beim [Erstellen von Apps mit einer WinUI-basierten Benutzeroberfläche](..\winui\winui3\winui-project-templates-in-visual-studio.md) helfen, sowie ein NuGet-Paket, das die WinUI-Bibliotheken enthält.  |
| [Verwalten von Ressourcen mit MRT Core](mrtcore/mrtcore-overview.md) | MRT Core bietet APIs zum Laden und Verwalten von Ressourcen, die von Ihrer App verwendet werden. MRT Core ist eine optimierte Version des modernen [Windows-Ressourcenverwaltungssystems](/windows/uwp/app-resources/resource-management-system). |
| [Rendern von Text mit DWriteCore](dwritecore.md) | DWriteCore bietet Zugriff auf alle aktuellen DirectWrite-Features für die Textdarstellung, einschließlich eines geräteunabhängigen Textlayoutsystems, hardwarebeschleunigtem Text, Multiformat-Text und breiter Sprachunterstützung.  |

## <a name="benefits-of-project-reunion-for-windows-developers"></a>Vorteile von Project Reunion für Windows-Entwickler

Project Reunion stellt eine breite Palette von Windows-APIs mit Implementierungen bereit, die vom Betriebssystem entkoppelt sind und Entwicklern über NuGet-Pakete zur Verfügung gestellt werden. Project Reunion soll das Windows SDK nicht ersetzen. Das Windows SDK wird weiterhin unverändert funktionieren, und es gibt viele Kernkomponenten von Windows, die mit APIs, die über Betriebssystem- und Windows SDK-Versionen bereitgestellt werden, weiterentwickelt werden. Entwicklern wird empfohlen, Project Reunion in Ihrem eigenen Tempo zu übernehmen.

### <a name="unified-api-surface-across-desktop-app-platforms"></a>Unified API-Oberfläche über Desktop-App-Plattformen hinweg

Entwickler, die Windows-Desktop-Apps erstellen möchten, müssen zwischen mehreren App-Plattformen und -Frameworks wählen. Jede Plattform stallt zwar viele Features und APIs bereit, die von mit anderen Plattformen erstellten Apps verwendet werden können, einige Features und APIs können jedoch nur von bestimmten Plattformen verwendet werden. Mit Project Reunion wird der Zugriff auf Windows-APIs für alle Windows 10-Desktop-Apps vereinheitlicht. Unabhängig davon, für welches App-Modell Sie sich entscheiden, haben Sie Zugriff auf den gleichen Satz von Windows-APIs, die in Project Reunion verfügbar sind.

Wir planen im Laufe der Zeit weitere Investitionen in Project Reunion, um die Unterschiede zwischen den verschiedenen App-Modellen zu verringern. Project Reunion wird sowohl WinRT-APIs als auch native C-APIs enthalten.

### <a name="consistent-support-across-windows-10-versions"></a>Konsistente Unterstützung über Windows 10-Versionen hinweg

Da sich die Windows-APIs mit neuen Betriebssystemversionen weiterentwickeln, müssen Entwickler Techniken wie [versionsadaptiven Code](/windows/uwp/debug-test-perf/version-adaptive-code) verwenden, um alle Versionsunterschiede zu berücksichtigen und so die Zielgruppe ihrer Anwendung zu erreichen. Dadurch kann der Code und die Entwicklungsumgebung komplexer werden.

Project Reunion-APIs können unter Windows 10, Version 1809, und allen späteren Versionen von Windows 10 verwendet werden. Das bedeutet, dass Sie, solange Ihre Kunden Windows 10, Version 1809 oder höher verwenden, neue Project Reunion-APIs und -Features verwenden können, sobald sie veröffentlicht werden, und keinen an die Version angepassten Code schreiben müssen.

### <a name="faster-release-cadence"></a>Schnellerer Versionsrhythmus

Neue Windows-APIs und -Funktionen sind in der Regel an Betriebssystemreleases gebunden, die ein- oder zweimal im Jahr erscheinen. Project Reunion wird Updates in einem kürzeren Intervall bereitstellen, sodass Sie früher und schneller Zugriff auf Innovationen in der Windows-Entwicklungsplattform erhalten, sobald diese erstellt werden.

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

Die folgenden Einschränkungen und bekannten Probleme gelten generell für Project Reunion 0.5.

- **Desktop-Apps (C#/.NET 5 oder C++/Win32)** : Project Reunion 0.5 kann nicht in ungepackten Desktop-Apps (C#/.NET 5 oder C++/Win32) verwendet werden. Dieses Release wird nur für die Verwendung in als MSIX-Paket vorliegenden Desktop-Apps unterstützt.
- **UWP-Apps**: Project Reunion 0.5 wird für UWP-Apps, die in Produktionsumgebungen verwendet werden, nicht unterstützt. Um Project Reunion in UWP-Apps zu verwenden, müssen Sie eine Vorschauversion der Project Reunion 0.5-Erweiterung verwenden. Diese wird für Produktionsumgebungen nicht unterstützt. Weitere Informationen zum Installieren der Vorschauerweiterung finden Sie unter [Einrichten der Entwicklungsumgebung](get-started-with-project-reunion.md#set-up-your-development-environment).
- Die [Einschränkungen des WinUI 3-Entwicklertools](..\winui\winui3\index.md#developer-tools) gelten auch für alle Projekte, die Project Reunion 0.5 verwenden.

Die folgenden Einschränkungen und bekannten Probleme gelten für bestimmte Entwicklerszenarien.

### <a name="using-the-project-reunion-nuget-package-in-existing-projects"></a>Verwenden des Project Reunion-NuGet-Pakets in bestehenden Projekten

Wenn Sie das [Project Reunion 0.5-NuGet-Paket in bestehenden Projekten verwenden](get-started-with-project-reunion.md#use-project-reunion-in-an-existing-project) möchten, beachten Sie bitte die folgenden Einschränkungen:

- Das Project Reunion 0.5-NuGet-Paket wird für die Verwendung mit Desktopprojekten (C#/.NET 5 und C++/WinRT) in Produktionsumgebungen unterstützt. Es ist als Entwicklervorschau für UWP-Projekte verfügbar und wird nicht für die Verwendung mit UWP-Projekten in Produktionsumgebungen unterstützt.
- Das Project Reunion 0.5-NuGet-Paket (namens **Microsoft.ProjectReunion**) enthält weitere Unterpakete (einschließlich **Microsoft.ProjectReunion.Foundation** und **Microsoft.ProjectReunion.WinUI**), die die Implementierungen für Komponenten wie WinUI, MRT Core und DWriteCore umfassen. Sie können diese Unterpakete nicht einzeln installieren, um nur bestimmte Komponenten in Ihrem Projekt zu referenzieren. Sie müssen das Paket **Microsoft.ProjectReunion** installieren, in dem alle Komponenten enthalten sind.  
- Wenn Sie das Project Reunion 0.5-NuGet-Paket in ein bestehendes Projekt installieren, können Sie nur Nicht-WinUI 3-Komponenten, die Teil von Project Reunion sind, in Ihrem Projekt verwenden. Um WinUI 3 zu verwenden, müssen Sie ein neues Projekt mit einer der WinUI 3-Projektvorlagen erstellen, wie im vorherigen Abschnitt beschrieben.


#### <a name="asta-to-sta-threading-model"></a>ASTA zu STA-Threadingmodell

Wenn Sie Code aus einer bestehenden UWP-App zu einem neuen C#/.NET 5- oder C++/Win32-WinUI 3-Projekt migrieren, das Project Reunion verwendet, beachten Sie, dass das neue Projekt das [Single-Thread-Apartment (STA)](/windows/win32/com/single-threaded-apartments)-Threadingmodell und nicht das von UWP-Apps verwendete [Application STA (ASTA)](https://devblogs.microsoft.com/oldnewthing/20210224-00/?p=104901)-Threadingmodells verwendet. Wenn Ihr Code das nicht wieder aufrufbare Verhalten des ASTA-Threadingmodells annimmt, verhält sich Ihr Code möglicherweise nicht wie erwartet.

## <a name="developer-roadmap"></a>Roadmap für Entwickler

Die neuesten Project Reunion-Pläne finden Sie in unserer [Roadmap](https://github.com/microsoft/ProjectReunion/blob/main/docs/roadmap.md).

## <a name="give-feedback-and-contribute"></a>Feedback und Mitwirkung

Wir entwickeln Project Reunion als Open-Source-Projekt. Auf unserer [Github-Seite](https://github.com/microsoft/ProjectReunion) finden Sie viele weitere Informationen zu unserer Project Reunion-Entwicklung und darüber, wie Sie Teil des Entwicklungsprozesses werden können. Schauen Sie sich unseren [Leitfaden für Mitwirkende](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md) an, um Fragen zu stellen, Diskussionen zu starten oder Featurevorschläge zu machen. Wir möchten sicherstellen, dass Project Reunion Entwicklern wie Ihnen den größtmöglichen Nutzen bringt.

## <a name="related-topics"></a>Zugehörige Themen

- [Erstellen von Windows-Desktop-Apps mit Project Reunion](index.md)
- [Erste Schritte mit Project Reunion](get-started-with-project-reunion.md)
- [Bereitstellen von Apps, die Project Reunion verwenden](deploy-apps-that-use-project-reunion.md)
- [Windows-UI-Bibliothek 3](../winui/winui3/index.md)
- [Verwalten von Ressourcen mit MRT Core](mrtcore/mrtcore-overview.md)
- [Rendern von Text mit DWriteCore](dwritecore.md)
