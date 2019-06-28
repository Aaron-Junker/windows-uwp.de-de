---
description: Dieses Tutorial veranschaulicht das Hinzufügen von UWP XAML-Benutzeroberflächen, MSIX-Pakete erstellen und andere moderne Komponenten in Ihrer WPF-Anwendung integrieren.
title: 'Tutorial: Modernisieren von WPF-app'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, Uwp, Windows Forms, Wpf, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 5e7179d4aeb66cad547e31e2456da2e8264ebbcd
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420080"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutorial: Modernisieren von WPF-app 

Es gibt viele Möglichkeiten, [modernisieren](index.md) vorhandene desktop-apps durch die Integration der neuesten Windows-Features in den vorhandenen Quellcode, anstatt das Umschreiben der apps von Grund auf neu. In diesem Tutorial werden wir mehrere Möglichkeiten, eine vorhandene WPF-Line-of-Business-app durch Verwendung dieser Features modernisieren erkunden:

* .NET Core 3
* UWP XAML-Steuerelemente mit XAML-Inseln
* Mit Adaptive Cards und Windows 10 Benachrichtigungen
* MSIX-Bereitstellung

Dieses Lernprogramm erfordert die folgenden entwicklungsfähigkeiten:

* Bei der Entwicklung von Windows-desktop-apps mit WPF auftreten.
* Grundlegende Kenntnisse der C# und XAML.
* Grundlegende Kenntnisse der UWP.

## <a name="overview"></a>Übersicht

Dieses Tutorial bietet den Code für eine einfache WPF-Line-of-Business-app mit dem Namen Contoso-Ausgaben. Im fiktiven Szenario des Lernprogramms ist Contoso-Ausgaben einer internen app-Manager der Contoso Corporation verwendet wird, zum Nachverfolgen der Ausgaben, die von ihrer Berichte übermittelt. Die Manager sind jetzt mit Touch-fähigen Geräten ausgestattet, und sie die Contoso-Ausgaben-app ohne Maus oder Tastatur verwenden möchten. Leider ist die aktuelle Version der app nicht Touch Anzeigenamen.

Contoso möchte, dass dieser app mit Mitarbeitern spesenabrechnungen effizienter erstellen ermöglichen neue Windows-Features modernisieren. Viele der Funktionen können einfach implementiert werden, erstellen Sie eine neue UWP-app. Allerdings wird die vorhandene app ist komplex und ist das Ergebnis der vielen Jahren der Entwicklung von verschiedenen Teams. Daher ist die umgeschrieben wurde von Grund auf neu mit neuer Technologie keine Option. Das Team sucht nach der beste Ansatz für die vorhandenen Codebasis neue Features hinzuzufügen.

Am Anfang des Lernprogramms, Contoso-Ausgaben ist auf .NET Framework 4.7.2 "und" die folgenden 3rd Party-Bibliotheken verwendet:

* MVVM Light, eine grundlegende Implementierung für das MVVM-Muster.
* Unity und DI-Containern.
* LiteDb, einer eingebetteten NoSQL-Lösung zum Speichern der Daten.
* Gefälschtes, ein Tool zum Generieren von falschen Daten.

In diesem Tutorial werden Sie Contoso-Ausgaben mit den neuen Windows-Features verbessern:

* Migrieren einer vorhandenen WPF-app auf .NET Core 3.0. Dadurch wird in Zukunft neue und wichtige Szenarios geöffnet.
* Verwenden von XAML-Inseln auf Host der **InkCanvas** und **MapControl** umschlossen von Steuerelementen, die vom Windows-Community Toolkit bereitgestellt wird.
* XAML-Inseln zum Hosten jedes standard UWP XAML-Steuerelement verwenden (in diesem Fall eine **CalendardView**).
* Integrieren Sie mit Adaptive Cards und Windows 10-Benachrichtigungen in die app.
* Paket, die die app mit MSIX und richten Sie eine CI/CD-pipeline zu Azure DevOps, damit automatisch neue Versionen der app für Tester und Benutzer bereitstellen kann, sobald diese verfügbar ist.

## <a name="prerequisites"></a>Vorraussetzungen

Um dieses Lernprogramm ausführen zu können, muss Ihrem Entwicklungscomputer diese erforderlichen Komponenten installiert haben:

* Windows 10, Version 1903 (build 18362) oder eine höhere Version.
* [Visual Studio 2019](https://www.visualstudio.com).
* [.NET Core 3 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (installieren die neuesten verfügbaren Preview-Version).

Stellen Sie sicher, dass Sie die folgenden Workloads und optionalen Features mit Visual Studio-2019 installieren:

* .NET Desktopentwicklung
* Entwicklung für die universelle Windows-Plattform
* Windows 10 SDK (10.0.18362.0 oder höher)

## <a name="get-the-contoso-expenses-sample-app"></a>Abrufen der Ausgaben von Contoso-Beispiel-app

Bevor Sie das Tutorial beginnen, herunterladen Sie den Quellcode für die Contoso-Ausgaben-app, und stellen Sie sicher, dass Sie den Code in Visual Studio erstellen können.

1. Laden Sie den Quellcode der app aus der **Versionen** Registerkarte die [AppConsult WinAppsModernization Workshop Repository](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). Ist der direkte Link [ https://aka.ms/wamwc ](https://aka.ms/wamwc).
2. Öffnen Sie die Zipdatei, und extrahieren Sie alle Inhalte in das Stammverzeichnis des Ihre **C:\\**  Laufwerk. Es erstellt einen Ordner namens **C:\WinAppsModernizationWorkshop**.
3. Öffnen Sie Visual Studio-2019 aus, und klicken Sie mit der Doppelklicken auf die **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** Datei, um die Projektmappe zu öffnen.
4. Stellen Sie sicher, dass Sie zu erstellen, ausführen und der Contoso-Ausgaben WPF-Projekt durch Drücken Debuggen der **starten** Taste oder STRG + F5.

## <a name="get-started"></a>Beginnen

Nachdem Sie den Quellcode für die Ausgaben der Contoso-Beispiel-app haben, und Sie können bestätigen, dass Sie es in Visual Studio erstellen können, können Sie das Lernprogramm zu starten:

* [Teil 1: Migrieren von Contoso Ausgaben-app auf .NET Core 3](modernize-wpf-tutorial-1.md)
* [Teil 2: Fügen Sie ein UWP-InkCanvas-Steuerelement, das mithilfe von XAML-Inseln](modernize-wpf-tutorial-2.md)
* [Teil 3: Hinzufügen eines UWP-CalendarView-Steuerelements, das mithilfe von XAML-Inseln](modernize-wpf-tutorial-3.md)
* [Teil 4: Hinzufügen von Windows 10-Benutzeraktivitäten und Benachrichtigungen](modernize-wpf-tutorial-4.md)
* [Teil 5: Packen und Bereitstellen mit MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Wichtige Konzepte

Die folgenden Abschnitte enthalten den Hintergrund für einige der Schlüsselkonzepte, die in diesem Tutorial erläutert. Wenn Sie bereits mit diesen Konzepten vertraut sind, können Sie diesen Abschnitt überspringen.

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

In Windows 8 hat Microsoft ein neues Framework aufgerufen, die Windows-Runtime (WinRT) eingeführt. Im Gegensatz zu .NET Framework stellt WinRT eine systemeigene APIs, die direkt für apps verfügbar gemacht werden. WinRT sprachprojektionen, die Ebenen oberhalb der Runtime ermöglichen Entwicklern die Interaktion mit Sprachen wie z. B. hinzugefügt sind außerdem C# und JavaScript, zusätzlich zu C++. Projektionen können Entwickler auf WinRT-apps zu erstellen, die die gleiche nutzen C# und XAML-Kenntnisse, die sie beim Entwickeln von apps mit .NET Framework abgerufen. 

In Windows 10, Microsoft führte die [universelle Windows-Plattform (UWP)](/windows/uwp/get-started/universal-application-platform-guide), die basiert auf WinRT. Die wichtigste Funktion der UWP ist, bietet jedoch einen gemeinsamen Satz von APIs für jede Geräteplattform: unabhängig davon, wenn die app, auf einem Desktop, auf der Xbox One oder auf eine HoloLens ausgeführt wird, Sie können die gleichen APIs verwenden.

Ab sofort die meisten neuen Windows 10 werden Funktionen über WinRT-APIs, einschließlich Features wie die Zeitachse, Projekt "ROME" und Windows Hello verfügbar gemacht.

### <a name="msix-packaging"></a>MSIX-paketerstellung

[MSIX](http://aka.ms/msix) (früher als AppX) ist das moderne Paketerstellungsmodell für Windows-apps. MSIX unterstützt die UWP-apps als auch desktop-apps erstellen, mit Technologien wie Win32, WPF, Windows Forms, Java, Electron und vieles mehr. Beim Packen einer desktop-app in einem Paket MSIX können Sie Ihre app auf dem Microsoft Store veröffentlichen. Ihre desktop-app auch abrufen Paketidentität bei der Installation dadurch Ihre desktop-app eine größere Gruppe von WinRT-APIs verwenden.

Weitere Informationen finden Sie in diesen Artikeln:

* [Paket-desktop-Anwendungen](/windows/uwp/porting/desktop-to-uwp-root)
* [Hinter den Kulissen der gepackten desktop-Anwendung](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML-Inseln

Ab Windows 10, Version 1903 sein, können Sie UWP-Steuerelementen in nicht-UWP-desktop-apps mit einem Feature hosten *XAML-Inseln*. Dieses Feature ermöglicht Sie erhöhen die aussehen, Verhalten und Funktionalität Ihrer vorhandenen desktop-apps mit den neuesten Windows 10-Benutzeroberflächen-Features, die nur über die UWP-Steuerelemente verfügbar sind. Dies bedeutet, dass Sie die UWP-Features wie z. B. Windows Ink und Steuerelemente, die das Fluent Design-System in Ihrer vorhandenen Windows Forms, WPF unterstützen verwenden können und C++ Win32-apps.

Weitere Informationen finden Sie unter [UWP-Steuerelementen in desktop-Anwendungen (XAML-Inseln)](/windows/uwp/xaml-platform/xaml-host-controls). Dieses Tutorial führt Sie durch den Prozess der Verwendung von zwei verschiedene Arten von XAML-Island-Steuerelemente:

* Die [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) in das Windows-Community-Toolkit. Diese WPF-Steuerelemente umschließen der Schnittstelle und die Funktionalität der entsprechenden UWP-Steuerelemente und können wie jedes andere WPF-Steuerelement in Visual Studio-Designer verwendet werden.

* Die UWP [Kalenderansicht](/windows/uwp/design/controls-and-patterns/calendar-view) Steuerelement. Dies ist ein UWP-Standardsteuerelement, die Sie mithilfe Hosten der [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement in das Windows-Community-Toolkit.

### <a name="net-core-3"></a>.NET Core 3

[.NET Core](https://docs.microsoft.com/dotnet/core/) ist ein Open-Source-Framework, die eine plattformübergreifende, leicht und problemlos erweiterbare Version des vollständigen .NET Framework implementiert. Im Vergleich zu vollständigen .NET Framework, .NET Core-Startzeit ist deutlich schneller und viele der APIs wurden optimiert.

Über die ersten Versionen war der Schwerpunkt von .NET Core Web- oder Back-End-apps unterstützen. Mit .NET Core können Sie ganz einfach erstellen, skalierbare Web-apps oder APIs, die auf Windows, Linux oder in Micro-Service-Architekturen wie Docker-Container gehostet werden können.

.NET Core 3 ist die nächste Hauptversion von .NET Core. Das Highlight dieser anstehenden Version ist der Support für Windows-Desktop-Apps, einschließlich Windows Forms- und WPF-Apps. Sie können neue und vorhandene Windows desktop-apps für .NET Core 3 ausführen und alle die Vorteile, die .NET Core zu bieten hat. UWP-Steuerelemente, die über [XAML-Inseln](xaml-islands.md) gehostet werden, können auch in Windows Forms- und WPF-Apps verwendet werden, die für .NET Core 3 bestimmt sind.

> [!NOTE]
> WPF und Windows Forms sind nicht immer die plattformübergreifende, und kann kein WPF- oder Windows Forms für Linux und MacOS ausgeführt. Die UI-Komponenten von WPF und Windows Forms haben immer noch eine Abhängigkeit auf dem Windows-Rendering-System.

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Ankündigung: .NET Core 3.0 (Vorschauversion 1)](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-1-and-open-sourcing-windows-desktop-frameworks/)
* [Ankündigung: .NET Core 3.0 (Vorschauversion 2)](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-2/)
* [Ankündigung: .NET Core 3.0 (Vorschauversion 3)](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-3/)
* [Ankündigung: .NET Core 3.0 (Vorschauversion 4)](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-preview-4/)
* [Neuerungen in .NET Core 3.0 (Vorschauversion 2)](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).