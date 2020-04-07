---
description: In diesem Tutorial wird veranschaulicht, wie du UWP-XAML-Benutzeroberflächen hinzufügst, MSIX-Pakete erstellst und weitere moderne Komponenten in deine WPF-App integrierst.
title: 'Tutorial: Modernisieren einer WPF-App'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 21049c995d467209b22fe8ea5c40d303911f2c2c
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521284"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: Modernisieren einer WPF-App 

Es gibt zahlreiche Möglichkeiten, vorhandene Desktop-Apps durch Integration der neuesten Windows-Features in den vorhandenen Quellcode zu [modernisieren](index.md), statt die Apps von Grund auf neu zu schreiben. In diesem Tutorial werden verschiedene der Möglichkeiten untersucht, um eine vorhandene WPF-Branchen-App unter Verwendung dieser Features zu modernisieren:

* .NET Core 3
* UWP-XAML-Steuerelementen mit XAML Islands
* Adaptive Karten und Windows 10-Benachrichtigungen
* MSIX-Bereitstellung

Für dieses Tutorial sind die folgenden Entwicklungskenntnisse erforderlich:

* Erfahrung in der Entwicklung von Windows-Desktop-Apps mit WPF
* Grundkenntnisse in C# und XAML
* Grundkenntnisse in UWP

## <a name="overview"></a>Übersicht

In diesem Tutorial wird der Code für eine einfache WPF-Branchen-App namens „Contoso Expenses“ bereitgestellt. Im fiktionalen Szenario des Tutorials ist Contoso Expenses eine App, die von leitenden Mitarbeitern der Contoso Corporation verwendet wird, um die von den unterstellten Mitarbeitern eingereichten Spesen nachzuverfolgen. Die leitenden Mitarbeiter sind mit Touchgeräten ausgestattet und möchten die Contoso Expenses-App ohne Maus oder Tastatur verwenden. Leider unterstützt die aktuelle Version der App keine Touchfunktionalität.

Contoso möchte der bestehenden App neue Windows-Features hinzufügen, um die Benutzerfreundlichkeit beim Erstellen von Spesenberichten zu erhöhen. Viele der Features könnten durch das Entwickeln einer neuen UWP-App problemlos implementiert werden. Die vorhandene App ist jedoch komplex und das Ergebnis jahrelanger Arbeit durch verschiedene Teams. Deshalb stellt eine von Grund auf mit neuen Technologie entwickelte App keine Option dar. Das Team sucht nach dem besten Ansatz, der vorhandenen Codebasis neue Features hinzuzufügen.

Zu Beginn des Tutorials ist die Contoso Expenses-App für .NET Framework 4.7.2 konzipiert und verwendet die folgenden Drittanbieterbibliotheken:

* MVVM Light – eine Basisimplementierung für das MVVM-Muster.
* Unity – ein Container für die Abhängigkeitseinschleusung.
* LiteDb – eine eingebettete NoSQL-Projektmappe zum Speichern der Daten.
* Bogus – ein Tool zum Generieren von Pseudodaten.

Im Tutorial erweiterst du die Contoso Expenses-App mit neuen Windows-Features:

* Migrieren einer vorhandenen WPF-App zu .NET Core 3.0, um künftig neue und wichtige Szenarien unterstützen zu können
* Verwenden von XAML Islands zum Hosten der umschlossenen Steuerelemente **InkCanvas** und **MapControl**, die vom Windows-Community-Toolkit bereitgestellt werden
* Verwenden von XAML Islands zum Hosten eines beliebigen UWP-XAML-Standardsteuerelements (in diesem Fall **CalendardView**)
* Integrieren adaptiver Karten und Windows 10-Benachrichtigungen in die App
* Packen der App mit MSIX und Einrichten einer CI/CD-Pipeline in Azure DevOps, um bei Verfügbarkeit automatisch neue App-Versionen für Tester und Benutzer bereitzustellen

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial zu bearbeiten, müssen auf deinem Entwicklungscomputer die folgenden erforderlichen Komponenten installiert sein:

* Windows 10, Version 1903 (Build 18362) oder eine höhere Version
* [Visual Studio 2019](https://www.visualstudio.com)
* [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (installiere die aktuelle Version)

Installiere die folgenden Workloads und optionalen Features mit Visual Studio 2019:

* .NET-Desktopentwicklung
* Entwicklung für die universelle Windows-Plattform
* Windows 10 SDK (10.0.18362.0 oder höher)

## <a name="get-the-contoso-expenses-sample-app"></a>Herunterladen der Contoso Expenses-Beispiel-App

Bevor du mit dem Tutorial beginnst, lade dir den Quellcode für die Contoso Expenses-App herunter, und stelle sicher, dass du den Code in Visual Studio kompilieren kannst.

1. Lade den Quellcode für die App über die Registerkarte **Releases** des [AppConsult-WinAppsModernizationWorkshop-Repositorys](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop) herunter. Der direkte Link lautet [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases).
2. Öffne die ZIP-Datei, und extrahiere sämtliche Inhalte in den Stamm deines **C:\\** -Laufwerks. Es wird ein Ordner namens **C:\WinAppsModernizationWorkshop** erstellt.
3. Öffne Visual Studio 2019, und doppelklicke auf die Datei **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln**, um die Projektmappe zu öffnen.
4. Stelle sicher, dass du das Contoso Expenses-WPF-Projekt kompilieren, ausführen und debuggen kannst, indem du auf die Schaltfläche **Starten** klickst oder STRG+F5 drückst.

## <a name="get-started"></a>Erste Schritte

Nachdem du nun über den Quellcode für die Contoso Expenses-Beispiel-App verfügst und bestätigt hast, dass eine Kompilierung in Visual Studio möglich ist, kannst du mit dem Tutorial beginnen:

* [Teil 1: Migrieren der Contoso Expenses-App zu .NET Core 3](modernize-wpf-tutorial-1.md)
* [Teil 2: Hinzufügen eines UWP-InkCanvas-Steuerelements mithilfe von XAML Islands](modernize-wpf-tutorial-2.md)
* [Teil 3: Hinzufügen eines UWP-CalendarView-Steuerelements mithilfe von XAML Islands](modernize-wpf-tutorial-3.md)
* [Teil 4: Hinzufügen von Windows 10-Benutzeraktivitäten und -Benachrichtigungen](modernize-wpf-tutorial-4.md)
* [Teil 5: Packen und Bereitstellen mit MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Liste wichtiger Konzepte

Die folgenden Abschnitte enthalten Hintergrundinformationen zu einigen der wichtigsten Konzepte, die in diesem Tutorial erläutert werden. Wenn du bereits mit diesen Konzepten vertraut bist, kannst du diesen Abschnitt überspringen.

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

In Windows 8 hat Microsoft ein neues Framework namens Windows-Runtime (WinRT) eingeführt. Im Gegensatz zum .NET Framework ist WinRT eine native Ebene mit APIs, die Apps direkt verfügbar gemacht werden. Mit WinRT wurden auch Sprachprojektionen eingeführt, bei denen es sich um Ebenen handelt, die zusätzlich zur Runtime hinzugefügt werden, damit Entwickler neben C++ auch mit Sprachen wie C# und JavaScript interagieren können. Projektionen ermöglichen es Entwicklern, Anwendungen basierend auf WinRT zu erstellen und dabei das C#- und XAML-Wissen zu nutzen, das sie bei der Entwicklung mit dem .NET Framework erworben haben. 

In Windows 10 hat Microsoft die [universelle Windows-Plattform (Universal Windows Platform, UWP)](/windows/uwp/get-started/universal-application-platform-guide) eingeführt, die auf WinRT basiert. Das wichtigste Merkmal von UWP ist, dass es einen gemeinsamen Satz von APIs für alle Geräteplattformen bietet: Unabhängig davon, ob die App auf einem Desktop, einer Xbox One oder einem HoloLens-Gerät ausgeführt wird, können die gleichen APIs genutzt werden.

In Zukunft werden die meisten neuen Windows 10-Features über WinRT-APIs verfügbar gemacht, darunter beispielsweise Zeitachse, Project Rome und Windows Hello.

### <a name="msix-packaging"></a>MSIX-Paketerstellung

[MSIX](/windows/msix/) ist das moderne Paketerstellungsmodell für Windows-Apps. MSIX unterstützt sowohl UWP-Apps als auch die Erstellung von Desktop-Apps mit Technologien wie Win32, WPF, Windows Forms, Java, Electron und anderen. Wenn du eine Desktop-App als MSIX-Paket packst, kannst du deine App im Microsoft Store veröffentlichen. Deine Desktop-App kann bei der Installation auch eine Paketidentität abrufen, wodurch sie eine größere Anzahl von WinRT-APIs nutzen kann.

Weitere Informationen findest du in diesen Artikeln:

* [Packen von Desktopanwendungen](/windows/uwp/porting/desktop-to-uwp-root)
* [Hintergrundinformationen zu deiner gepackten Desktopanwendung](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML Islands

Ab Windows 10, Version 1903, kannst du mithilfe eines Features namens *XAML Islands* UWP-Steuerelemente auch in anderen als UWP-Desktop-Apps hosten. Mit diesem Feature kannst du Aussehen, Verhalten und Funktionalität deiner vorhandenen Desktop-Apps mit den aktuellen Features der Windows 10-Benutzeroberfläche erweitern, die nur in Form von UWP-Steuerelementen verfügbar sind. Dies bedeutet, dass du UWP-Features, wie etwa Windows Ink, und Steuerelemente mit Unterstützung des Fluent Design-Systems in deinen vorhandenen WPF-, Windows Forms- und C++-Win32-Apps nutzen kannst.

Weitere Informationen findest du unter [Hosten von UWP XAML-Steuerelementen in Desktop-Apps (XAML Islands)](/windows/uwp/xaml-platform/xaml-host-controls). Dieses Tutorial führt dich durch die Vorgehensweise zur Verwendung von zwei verschiedenen Arten von XAML Islands-Steuerelementen:

* Die [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)- und [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol)-Steuerelemente im Windows-Community-Toolkit. Diese WPF-Steuerelemente kapseln Schnittstelle und Funktionalität der entsprechenden UWP-Steuerelemente und können wie jedes andere WPF-Steuerelement im Visual Studio-Designer verwendet werden.

* Das UWP-Steuerelement [CalendarView](/windows/uwp/design/controls-and-patterns/calendar-view). Hierbei handelt es sich um ein UWP-Standardsteuerelement, das du mithilfe des [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelements im Windows-Community-Toolkit hostest.

### <a name="net-core-3"></a>.NET Core 3

[.NET Core](https://docs.microsoft.com/dotnet/core/) ist ein Open Source-Framework, das eine plattformübergreifende, schlanke und leicht zu erweiternde Version des vollständigen .NET Framework implementiert. Im Vergleich zum vollständigen .NET Framework ist die Startzeit von .NET Core deutlich kürzer, und viele der APIs wurden optimiert.

Während der ersten Releases lag der Schwerpunkt von .NET Core auf der Unterstützung von Web- oder Back-End-Apps. Mit .NET Core kannst du problemlos skalierbare Web-Apps oder APIs erstellen, die unter Windows, Linux oder in Microservice-Architekturen wie Docker-Containern gehostet werden können.

.NET Core 3 ist die neueste Version von .NET Core. Das Highlight dieser Version ist der Support für Windows-Desktop-Apps, einschließlich Windows Forms- und WPF-Apps. Du kannst ab sofort neue und vorhandene Windows-Desktop-Apps unter .NET Core 3 ausführen und alle Vorteile von .NET Core genießen. UWP-Steuerelemente, die über [XAML-Inseln](xaml-islands.md) gehostet werden, können auch in Windows Forms- und WPF-Apps verwendet werden, die für .NET Core 3 entwickelt wurden.

> [!NOTE]
> WPF und Windows Forms sind nicht plattformübergreifend, und es ist nicht möglich, WPF oder Windows Forms unter Linux und macOS auszuführen. Die Benutzeroberflächenkomponenten von WPF und Windows Forms hängen weiterhin vom Windows-Renderingsystem ab.

Weitere Informationen findest du unter [Neuerungen in .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).
