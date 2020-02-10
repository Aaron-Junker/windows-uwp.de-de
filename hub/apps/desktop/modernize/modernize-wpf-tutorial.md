---
description: In diesem Tutorial wird veranschaulicht, wie Sie UWP-XAML-Benutzeroberflächen hinzufügen, msix-Pakete erstellen und andere moderne Komponenten in Ihre WPF-App integrieren.
title: 'Tutorial: modernisieren einer WPF-App'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 397c301564c0d4799c6b41db209da9659725103d
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089306"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: modernisieren einer WPF-App 

Es gibt viele Möglichkeiten, vorhandene Desktop-Apps zu [modernisieren](index.md) , indem die neuesten Windows-Features in den vorhandenen Quellcode integriert werden, anstatt die apps von Grund auf neu zu schreiben. In diesem Tutorial werden verschiedene Möglichkeiten zum modernisieren einer vorhandenen WPF-Branchen-App mithilfe der folgenden Features erläutert:

* .NET Core 3
* UWP-XAML-Steuerelemente mit XAML-Inseln
* Adaptive Karten und Windows 10-Benachrichtigungen
* MSIX-Bereitstellung

Dieses Lernprogramm erfordert die folgenden Entwicklungsfähigkeiten:

* Erfahren Sie, wie Sie Windows-Desktop-Apps mit WPF entwickeln.
* Grundlegende Kenntnisse C# von und XAML.
* Grundlegende Kenntnisse der UWP.

## <a name="overview"></a>Übersicht

Dieses Tutorial enthält den Code für eine einfache WPF-Branchen-App mit dem Namen "" mit der Bezeichnung "" von "". Im fiktiven Szenario des Tutorials ist die Conto-Kosten eine interne APP, die von Managern von congeso Corporation verwendet wird, um die von ihren Berichten gesendeten Ausgaben nachzuverfolgen. Die Vorgesetzten sind nun mit Touchscreen-fähigen Geräten ausgestattet, und Sie möchten die APP für die kostenpflichtige app ohne Maus oder Tastatur verwenden. Leider ist die aktuelle Version der APP nicht Berührungs freundlich.

In diesem Fall möchte ich diese APP mit neuen Windows-Features modernisieren, damit Mitarbeiter kosteneffizientere Berichte erstellen können. Viele der Features können problemlos implementiert werden, indem eine neue UWP-App aufgebaut wird. Allerdings ist die vorhandene App komplex und ist das Ergebnis von vielen Jahren der Entwicklung durch verschiedene Teams. Daher ist es keine Option, Sie von Grund auf neu zu schreiben. Das Team sucht nach dem besten Ansatz zum Hinzufügen neuer Features zur vorhandenen CodeBase.

Zu Beginn des Tutorials hat die Kosten für die Zusammenarbeit von "4.7.2" auf den .NET Framework und verwendet die folgenden Bibliotheken von Drittanbietern:

* MVVM Light, eine grundlegende Implementierung für das MVVM-Muster.
* Unity, ein Container für die Abhängigkeitsinjektion.
* Litedb, eine eingebettete nosql-Lösung zum Speichern der Daten.
* Bogus, ein Tool zum Generieren gefälschter Daten.

In diesem Tutorial verbessern Sie die Kosten für die Zusammenarbeit mit den neuen Windows-Features:

* Migrieren einer vorhandenen WPF-App zu .net Core 3,0. Dadurch werden in Zukunft neue und wichtige Szenarien geöffnet.
* Verwenden Sie XAML-Inseln, um die umschließbaren Steuerelemente **InkCanvas** und **mapcontrol** im Windows Community Toolkit zu hosten.
* Verwenden Sie XAML-Inseln zum Hosten beliebiger Standard-UWP-XAML-Steuerelemente (in diesem Fall eine **calendardview**).
* Integrieren Sie Adaptive Karten und Windows 10-Benachrichtigungen in die app.
* Packen Sie die APP mit msix, und richten Sie eine CI/CD-Pipeline für Azure devops ein, damit Sie automatisch neue Versionen der APP an Tester und Benutzer übermitteln können, sobald Sie verfügbar ist.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Ausführen dieses Tutorials müssen auf dem Entwicklungs Computer die folgenden Komponenten installiert sein:

* Windows 10, Version 1903 (Build 18362) oder eine höhere Version.
* [Visual Studio 2019](https://www.visualstudio.com).
* [.Net Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (installieren Sie die neueste Version).

Stellen Sie sicher, dass Sie die folgenden Workloads und optionalen Features mit Visual Studio 2019 installieren:

* .Net-Desktop Entwicklung
* Entwicklung für die universelle Windows-Plattform
* Windows 10 SDK (10.0.18362.0 oder höher)

## <a name="get-the-contoso-expenses-sample-app"></a>Holen Sie sich die Beispiel-App für die kostenpflichtige app

Bevor Sie mit dem Tutorial beginnen, laden Sie den Quellcode für die APP für die kostenpflichtige app-Ausgabe herunter, und stellen Sie sicher, dass Sie den Code in Visual Studio erstellen können.

1. Laden Sie den Quellcode der APP von der Registerkarte **Releases** im anwendungsrepository " [appconsult winappsmodernisierungsrepository](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop)" herunter. Der direkte Link ist [https://aka.ms/wamwc](https://aka.ms/wamwc).
2. Öffnen Sie die ZIP-Datei, und extrahieren Sie den gesamten Inhalt in das Stammverzeichnis des Laufwerks **C:\\** . Es wird ein Ordner mit dem Namen **c:\winappsmodernizationworkshop**erstellt.
3. Öffnen Sie Visual Studio 2019, und doppelklicken Sie auf die Datei **c:\winappsmodernizationworkshop\lab\exercise1\01-start\contosoexpenses\contosoexpenses.sln** , um die Projekt Mappe zu öffnen.
4. Stellen Sie sicher, dass Sie das WPF-Projekt für die Zusammenarbeit **mit dem Projekt** von "Build" erstellen, ausführen und Debuggen können.

## <a name="get-started"></a>Erste Schritte

Nachdem Sie den Quellcode für die Beispiel-App für die Zusammenarbeit mit der kostenpflichtigen Seite erstellt haben und Sie sich vergewissern können, dass Sie ihn in Visual Studio erstellen können, können Sie das Tutorial starten:

* [Teil 1: Migrieren der apptoso-Ausgaben-APP zu .net Core 3](modernize-wpf-tutorial-1.md)
* [Teil 2: Hinzufügen eines UWP InkCanvas-Steuer Elements mithilfe von XAML-Inseln](modernize-wpf-tutorial-2.md)
* [Teil 3: Hinzufügen eines UWP CalendarView-Steuer Elements mithilfe von XAML-Inseln](modernize-wpf-tutorial-3.md)
* [Teil 4: Hinzufügen von Windows 10-Benutzeraktivitäten und-Benachrichtigungen](modernize-wpf-tutorial-4.md)
* [Teil 5: Verpacken und Bereitstellen mit msix](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Wichtige Konzepte

Die folgenden Abschnitte enthalten Hintergrundinformationen zu einigen der wichtigsten Konzepte, die in diesem Tutorial erläutert werden. Wenn Sie mit diesen Konzepten bereits vertraut sind, können Sie diesen Abschnitt überspringen.

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

In Windows 8 hat Microsoft ein neues Framework namens "Windows-Runtime (WinRT)" eingeführt. Im Gegensatz zum .NET Framework ist WinRT eine native Ebene von APIs, die direkt für apps verfügbar gemacht werden. WinRT führte auch sprach Projektionen ein, bei denen es sich um Ebenen handelt, die oberhalb der Laufzeit hinzugefügt wurden, um Entwicklern die C# Interaktion mit der Anwendung zu C++ermöglichen, und zwar zusätzlich zu. Projektionen ermöglichen Entwicklern das Erstellen von apps, die auf WinRT basieren und dieselben C# und XAML-Kenntnisse nutzen, die Sie beim Erstellen von apps mit dem .NET Framework erworben haben. 

In Windows 10 hat Microsoft die [universelle Windows-Plattform (UWP)](/windows/uwp/get-started/universal-application-platform-guide)eingeführt, die auf WinRT basiert. Das wichtigste Feature von UWP ist, dass es einen gemeinsamen Satz von APIs für jede Geräteplattform bietet: unabhängig davon, ob die APP auf einem Desktop, auf einer Xbox One oder in einem hololens ausgeführt wird, können Sie dieselben APIs verwenden.

In Zukunft werden die meisten neuen Features von Windows 10 über WinRT-APIs verfügbar gemacht, einschließlich Features wie Timeline, Project Rom und Windows Hello.

### <a name="msix-packaging"></a>Msix-Paket Erstellung

[Msix](/windows/msix/) ist das moderne Paket Erstellungs Modell für Windows-apps. Msix unterstützt sowohl UWP-Apps als auch Desktop-Apps, die mithilfe von Technologien wie Win32, WPF, Windows Forms, Java, Elektronen und mehr aufgebaut werden. Wenn Sie eine Desktop-app in einem msix-Paket Verpacken, können Sie die APP auf dem Microsoft Store veröffentlichen. Ihre Desktop-App erhält bei der Installation auch die Paket Identität, sodass Ihre Desktop-App einen breiteren Satz von WinRT-APIs verwenden kann.

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verpacken von Desktop Anwendungen](/windows/uwp/porting/desktop-to-uwp-root)
* [Im Hintergrund ihrer gepackten Desktop Anwendung](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML-Inseln

Ab Windows 10, Version 1903, können Sie UWP-Steuerelemente in nicht-UWP-Desktop-Apps mithilfe eines Features namens *XAML-Inseln*hosten. Diese Funktion ermöglicht es Ihnen, das Aussehen, das Gefühl und die Funktionalität Ihrer vorhandenen Desktop-Apps mit den neuesten Windows 10-Benutzeroberflächen Features zu verbessern, die nur über UWP-Steuerelemente zur Verfügung stehen. Dies bedeutet, dass Sie UWP-Funktionen wie z. b. Windows Ink und Steuerelemente verwenden können, die das fließende Entwurfs System in Ihren vorhandenen C++ WPF-, Windows Forms-und Win32-apps unterstützen.

Weitere Informationen finden Sie unter [UWP-Steuerelemente in Desktop Anwendungen (XAML-Inseln)](/windows/uwp/xaml-platform/xaml-host-controls). Dieses Tutorial führt Sie durch den Prozess der Verwendung von zwei verschiedenen Typen von XAML-Steuerelementen:

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [mapcontrol](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) im Windows Community Toolkit. Diese WPF-Steuerelemente wrappen die-Schnittstelle und die Funktionalität der entsprechenden UWP-Steuerelemente und können wie jedes andere WPF-Steuerelement im Visual Studio-Designer verwendet werden.

* Das UWP- [Kalenderansicht](/windows/uwp/design/controls-and-patterns/calendar-view) -Steuerelement. Dabei handelt es sich um ein Standardmäßiges UWP-Steuerelement, das Sie mit dem [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement im Windows Community Toolkit hosten werden.

### <a name="net-core-3"></a>.NET Core 3

[.Net Core](https://docs.microsoft.com/dotnet/core/) ist ein Open-Source-Framework, das eine plattformübergreifende, einfache und leicht erweiterbare Version der vollständigen .NET Framework implementiert. Im Vergleich zum vollständigen .NET Framework ist die .net Core-Startzeit viel schneller, und viele der APIs wurden optimiert.

In den ersten Releases von .net Core war der Schwerpunkt auf der Unterstützung von Web-oder Back-End-apps. Mit .net Core können Sie problemlos skalierbare Web-Apps oder APIs erstellen, die unter Windows, Linux oder in microservice-Architekturen wie docker-Containern gehostet werden können.

.Net Core 3 ist die neueste Version von .net Core. Das Highlight dieser Version ist der Support für Windows-Desktop-Apps, einschließlich Windows Forms- und WPF-Apps. Sie können neue und vorhandene Windows-Desktop-Apps unter .NET Core 3 ausführen und alle Vorteile von .NET Core genießen. UWP-Steuerelemente, die über [XAML-Inseln](xaml-islands.md) gehostet werden, können auch in Windows Forms- und WPF-Apps verwendet werden, die für .NET Core 3 bestimmt sind.

> [!NOTE]
> WPF und Windows Forms werden nicht plattformübergreifend entwickelt, und Sie können keine WPF-oder-Windows Forms unter Linux und MacOS ausführen. Die Benutzeroberflächen Komponenten von WPF und Windows Forms sind weiterhin vom Windows-Renderingsystem abhängig.

Weitere Informationen finden Sie unter [Neues in .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).