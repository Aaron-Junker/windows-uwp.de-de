---
description: Dieser Artikel enthält Anweisungen zum Installieren der Project-Reunion-Erweiterung für Visual Studio 2019 auf dem Entwicklungs Computer und zum Verwenden von Project Reunion in neuen oder vorhandenen Projekten.
title: Erste Schritte mit Project Reunion
ms.topic: article
ms.date: 03/19/2021
keywords: Windows Win32, Desktopentwicklung, Project Reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 5b07229f381575da29d25353ca8147e712482bef
ms.sourcegitcommit: 3942f09c620e3f3065cae91dc51505e86ec0969b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106376588"
---
# <a name="get-started-with-project-reunion"></a>Erste Schritte mit Project Reunion

Dieser Artikel enthält Anweisungen zum Installieren der Project-Reunion-Erweiterung für Visual Studio 2019 auf dem Entwicklungs Computer und zum Verwenden von Project Reunion in neuen oder vorhandenen Projekten. Bevor Sie Project Reunion installieren und verwenden, finden Sie unter [Einschränkungen und bekannte Probleme](index.md#limitations-and-known-issues)Weitere Informationen.

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

1. Stelle sicher, dass auf deinem Entwicklungscomputer Windows 10 (Version 1809, Build 17763) oder eine höhere Betriebssystemversion installiert ist.

2. Installieren Sie [Visual Studio 2019, Version 16.10 Preview](https://visualstudio.microsoft.com/vs/preview/) (oder höher), falls Sie dies nicht bereits getan haben.

    > [!NOTE]
    > Visual Studio 2019, Version 16,9, unterstützt auch Project Reunion, unterstützt jedoch nicht alle WinUI 3-Toolfeatures. Weitere Informationen zur Unterstützung von WinUI 3-Tools finden Sie unter [Windows UI Library 3-Project Reunion 0,5](../winui/winui3/index.md).

    Bei der Installation von Visual Studio müssen Sie die folgenden Komponenten einschließen:
    - Vergewissern Sie sich auf der Registerkarte **Workloads** im Installationsdialogfeld, dass **Entwicklung mit der Universellen Windows-Plattform (UWP)** ausgewählt ist.
    - Stellen Sie sicher, dass auf der Registerkarte **Einzelne Komponenten** im Installationsdialogfeld im Abschnitt **SDKs, Bibliotheken und Frameworks** die Option **Windows 10 SDK (10.0.19041.0)** ausgewählt ist.

    Zum Erstellen von .NET-Apps müssen Sie außerdem die folgenden Komponenten einschließen:
    - Vergewissern Sie sich, dass auf der Registerkarte **Workloads** im Installationsdialogfeld die Option **.NET Desktopentwicklung** ausgewählt ist.

    Zum Erstellen von C++-Apps müssen Sie außerdem die folgenden Komponenten einschließen:
    - Vergewissern Sie sich, dass auf der Registerkarte **Workloads** im Installationsdialogfeld die Option **Desktopentwicklung mit C++** ausgewählt ist.
    - Vergewissern Sie sich, dass im Bereich **Installationsdetails** auf der rechten Seite des Installationsdialogfelds im Abschnitt **Entwicklung für die universelle Windows-Plattform** die optionale Komponente **C++ (v142) Tools für die universelle Windows-Plattform** ausgewählt ist.

3. Wenn Sie zuvor die [WinUI 3-Vorschau Erweiterung für Visual Studio](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates)installiert haben, deinstallieren Sie die Erweiterung. Weitere Informationen zum Deinstallieren einer Erweiterung finden Sie unter [Verwalten von Erweiterungen für Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions).

4. Stellen Sie sicher, dass in Ihrem System eine nuget-Paketquelle für den offiziellen nuget-Dienst Index unter aktiviert ist `https://api.nuget.org/v3/index.json` . 

    1. Klicken Sie in Visual Studio   ->  auf Extras **nuget Paket-Manager**  ->  **Paket-Manager Einstellungen** , um das Dialogfeld **Optionen** zu öffnen. 
    2. Wählen Sie im linken Bereich des Dialog Felds **Optionen** die Registerkarte **Paketquellen** aus, und stellen Sie sicher, dass eine Paketquelle für **nuget.org** vorhanden ist, die auf `https://api.nuget.org/v3/index.json` als Quell-URL verweist. Weitere Informationen finden Sie unter [Allgemeine nuget-Konfigurationen](/nuget/consume-packages/configuring-nuget-behavior).

5. Laden Sie die Project Reunion 0,5-Erweiterung für Visual Studio herunter, und installieren Sie Sie. Es gibt zwei Versionen der Erweiterung: eine für Desktop-Apps (c#/.net 5 oder C++/WinRT) und eine für UWP-apps.

    So verwenden Sie die Projekt Zusammenführung in Desktop-Apps (c#/.net 5 oder C++/WinRT):
    - Klicken Sie in Visual Studio 2019 auf **Erweiterungen** > **Erweiterungen verwalten**, suchen Sie nach **Project Reunion**, und installieren Sie die **Project Reunion**-Erweiterung.
    - Alternativ können Sie die [Project Reunion 0,5-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion) direkt vom Visual Studio Marketplace herunterladen und installieren.

    Wenn Sie die Projekt Zusammenführung in UWP-Apps verwenden möchten, müssen Sie eine Vorschauversion der Erweiterung installieren, die für Produktionsumgebungen nicht unterstützt wird:
    - Deinstallieren Sie alle vorhandenen Versionen der Projekt Zusammenführung VSIX.
    - Klicken Sie in Visual Studio 2019 auf **Erweiterungen** Erweiterungen  >  **Verwalten**, und klicken Sie in der unteren linken Ecke auf **Einstellungen für Erweiterungen ändern** . Deaktivieren Sie automatische Updates für Pakete, die für alle Benutzer installiert wurden, bevor Sie die ältere Version installieren.
    - Laden Sie die [Vorschau Erweiterung Project Reunion 0,5](https://download.microsoft.com/download/9/9/8/9981a84b-8fd8-4645-9dce-c62761601f17/ProjectReunion.Extension.vsix)herunter, und installieren Sie Sie.

    Anweisungen zum Hinzufügen des VSIX-Pakets zu Visual Studio finden Sie unter [Verwalten von Erweiterungen für Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions).

    ![Screenshot der installierten Project Reunion-Erweiterung](images/reunion-extension-install.png)

6. Wenn Sie WinUI 3-Tools wie Live Visual Tree, Hot Neuladen und den Live-Eigenschaften-Explorer in Visual Studio 2019 16,10 Preview verwenden möchten, müssen Sie WinUI 3-Tools mit Features der Visual Studio-Vorschau aktivieren. Anweisungen hierzu finden Sie unter Aktivieren von UI-Tools [für WinUI 3 in vs 16,9 Preview 4](https://github.com/microsoft/microsoft-ui-xaml/issues/4140).

## <a name="create-a-new-project-that-uses-project-reunion"></a>Erstellen eines neuen Projekts, das Project Reunion verwendet

Die Project Reunion 0,5-Erweiterungen für Visual Studio 2019 (einschließlich der Erweiterung für Desktop-Apps und die Vorschau Erweiterung für UWP-Apps) enthalten Projektvorlagen, die Projekte mit einer WinUI 3-basierten UI-Schicht generieren und Zugriff auf alle anderen Project-Reunion-APIs bereitstellen. Weitere Informationen zu den verfügbaren Projektvorlagen finden Sie unter [WinUI 3-Projektvorlagen in Visual Studio](..\winui\winui3\winui-project-templates-in-visual-studio.md).

> [!NOTE]
> Die Projektvorlagen "Desktop" (c#/.net 5 und C++/WinRT) werden für die Verwendung in Produktionsumgebungen unterstützt. Die UWP-Projektvorlagen sind nur als Entwicklervorschau verfügbar und können nicht zum Erstellen von Apps für Produktionsumgebungen verwendet werden.

So erstellen Sie ein neues Projekt, das Project Reunion 0,5 verwendet:

1. Befolgen Sie die Anweisungen im folgenden Artikel:

    - [Erste Schritte mit WinUI 3 für Desktop-Apps](..\winui\winui3\get-started-winui3-for-desktop.md)
    - [Erste Schritte mit WinUI 3 für UWP-Apps (Vorschau)](..\winui\winui3\get-started-winui3-for-uwp.md)
    - [Erstellen einer einfachen WinUI 3-Desktop-App](..\winui\winui3\desktop-build-basic-winui3-app.md)

2. Nachdem Sie Ihr Projekt erstellt haben, erhalten Sie zusätzlich zu allen anderen Windows- und .NET-APIs, die typischerweise für Desktop- und UWP-Apps verfügbar sind, Zugriff auf die folgenden Project Reunion-APIs und -Komponenten.

    - [Windows-UI-Bibliothek 3](../winui/winui3/index.md)
    - [Verwalten von Ressourcen im MRT Core](mrtcore/mrtcore-overview.md)
    - [Rendern von Text mit DWriteCore](dwritecore.md)

Um sicherzustellen, dass Ihr neues Projekt Project Reunion verwendet, erweitern Sie den Knoten **Abhängigkeiten** > **Pakete** unter Ihrem Projekt im **Projektmappen-Explorer**. Unter diesem Knoten sollten mehrere **Microsoft.ProjectReunion**-Pakete aufgeführt sein, ähnlich wie in der folgenden Abbildung.

![Screenshot von Project Reunion-Paketen im Projektmappen-Explorer-Bereich](images/reunion-packages.png)

## <a name="use-project-reunion-in-an-existing-project"></a>Verwenden von Project Reunion in einem vorhandenen Projekt

Wenn Sie über ein vorhandenes Projekt verfügen, in dem Sie die Projekt Zusammenführung verwenden möchten, können Sie das nuget-Paket Project Reunion 0,5 in Ihrem Projekt installieren. Dieses Szenario weist [einige Einschränkungen](#limitations-for-using-project-reunion-in-existing-projects)auf.

1. Öffnen Sie ein vorhandenes Desktopprojekt (entweder C#/.NET 5 oder C++/WinRT) oder UWP-Projekt in Visual Studio 2019.

    > [!NOTE]
    > Wenn Sie über ein c#/.net 5-Desktop Projekt verfügen, stellen Sie sicher, dass das **TargetFramework** -Element in der Projektdatei einem Windows 10-spezifischen .net 5-Moniker (z. b. **net 5.0-Windows 10.0.19041.0**) zugewiesen ist, damit er Windows-Runtime-APIs aufrufen kann. Weitere Informationen finden Sie in [diesem Abschnitt](../../apps/desktop/modernize/desktop-to-uwp-enhance.md#net-5-use-the-target-framework-moniker-option).

2. Vergewissern Sie sich, dass [Paketverweise](/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf **Extras > NuGet-Paket-Manager > Paket-Manager-Einstellungen**.
    2. Achten Sie darauf, dass die Einstellung **Standardformat für Paketverwaltung** auf **PackageReference** festgelegt ist.

3. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf dein Projekt, und wähle **NuGet-Pakete verwalten** aus.

4. Wählen Sie im Fenster **NuGet-Paket-Manager** die Registerkarte **Durchsuchen** aus, und suchen Sie nach `Microsoft.ProjectReunion`.

5. Nachdem das **Microsoft.ProjectReunion**-Paket gefunden wurde, klicken Sie im rechten Bereich des Fensters **NuGet-Paket-Manager** auf **Installieren**.

    ![Screenshot des installierenden nuget-Pakets für Project Reunion](images/reunion-nuget-install.png)

6. Nachdem Sie das Paket installiert haben, können Sie die folgenden Project Reunion-APIs und -Komponenten in Ihrem Projekt verwenden:

    - [Verwalten von Ressourcen im MRT Core](mrtcore/mrtcore-overview.md)
    - [Rendern von Text mit DWriteCore](dwritecore.md)

### <a name="limitations-for-using-project-reunion-in-existing-projects"></a>Einschränkungen für die Verwendung der Projekt Zusammenführung in vorhandenen Projekten

Wenn Sie Project Reunion 0,5 in vorhandenen Projekten verwenden möchten, beachten Sie die folgenden Einschränkungen:

- Wenn Sie das nuget-Paket Project Reunion 0,5 für ein vorhandenes Projekt installieren, können Sie nur nicht-WinUI 3-Komponenten verwenden, die Teil der Projekt Zusammenführung in Ihrem Projekt sind. Um WinUI 3 zu verwenden, müssen Sie ein neues Projekt mit einer der WinUI 3-Projektvorlagen erstellen, wie im vorherigen Abschnitt beschrieben.
- Das nuget-Paket Project Reunion 0,5 (mit dem Namen **Microsoft. projectreunion**) enthält andere untergeordnete Pakete (einschließlich **Microsoft. projectreunion. Foundation** und **Microsoft. projectreunion. WinUI**), die die Implementierungen für Komponenten enthalten, einschließlich WinUI, MRT Core und dwrite tecore. In der aktuellen Version können Sie diese Unterpakete nicht einzeln installieren, um nur auf bestimmte Komponenten in Ihrem Projekt zu verweisen. Sie müssen das Paket **Microsoft. projectreunion** installieren, das alle-Komponenten enthält.  
- Das Installieren des nuget-Pakets Project Reunion 0,5 wird zurzeit nicht in WPF-Projekten unterstützt.
- Das nuget-Paket Project Reunion 0,5 wird für die Verwendung mit Desktop Projekten (c#/.net 5 und C++/WinRT) in Produktionsumgebungen unterstützt. Es ist als Entwicklervorschau für UWP-Projekte verfügbar und wird nicht für die Verwendung mit UWP-Projekten in Produktionsumgebungen unterstützt.

## <a name="samples"></a>Beispiele

Die folgenden Project Reunion-Beispiele sind derzeit für Sie verfügbar.

- [Beispiel für den DWriteCore-Katalog](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery): Diese Beispielanwendung veranschaulicht die [DWriteCore](dwritecore.md)-API.
- [Beispiel für MRT Core](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore): Diese Beispielanwendung veranschaulicht die [MRT Core](mrtcore/mrtcore-overview.md)-API.
- [Hallo Welt-Beispiel](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp): Dieses Beispiel veranschaulicht eine grundlegende Integration in das Project Reunion-NuGet-Paket.
- Katalog für [XAML](https://aka.ms/winui3/xcg)-Steuerelemente: Dies ist eine Beispiel-APP, die alle WinUI 3-Steuerelemente in Aktion präsentiert. 

## <a name="related-topics"></a>Zugehörige Themen

- [Erstellen von Windows-Desktop-Apps mit Project Reunion](index.md)
- [Bereitstellen von Apps, die Project Reunion verwenden](deploy-apps-that-use-project-reunion.md)
