---
description: Dieser Artikel enthält Anweisungen zum Installieren der Project Reunion-Erweiterung für Visual Studio 2019 auf Ihrem Entwicklungscomputer und zum Verwenden von Project Reunion in neuen oder vorhandenen Projekten.
title: Erste Schritte mit Project Reunion
ms.topic: article
ms.date: 03/19/2021
keywords: Windows Win32, Desktopentwicklung, Project Reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: af934c2b1f4eeaa04693c3587915421a6d1453d1
ms.sourcegitcommit: df14e7768acdb243190e3418db5afa5d65c5ff88
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107574645"
---
# <a name="get-started-with-project-reunion"></a>Erste Schritte mit Project Reunion

Dieser Artikel enthält Anweisungen zum Installieren der Project Reunion-Erweiterung für Visual Studio 2019 auf Ihrem Entwicklungscomputer und zum Verwenden von Project Reunion in neuen oder vorhandenen Projekten. Bevor Sie Project Reunion installieren und verwenden, sehen Sie sich die [Einschränkungen und bekannten Probleme an.](index.md#limitations-and-known-issues)

> [!NOTE]
> Wenn Sie ein Projekt mit einer früheren Vorschauversion oder Releaseversion von Project Reunion oder WinUI 3 erstellt haben, können Sie [das Projekt aktualisieren, um das neueste Release zu verwenden.](update-existing-projects-to-the-latest-release.md)

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

1. Stelle sicher, dass auf deinem Entwicklungscomputer Windows 10 (Version 1809, Build 17763) oder eine höhere Betriebssystemversion installiert ist.

2. Installieren Sie [Visual Studio 2019, Version 16.10 Preview](https://visualstudio.microsoft.com/vs/preview/) (oder höher), falls Sie dies nicht bereits getan haben.

    > [!NOTE]
    > Visual Studio 2019 unterstützt Version 16.9 auch Project Reunion, aber nicht alle WinUI 3-Toolfeatures. Weitere Informationen zur Unterstützung von WinUI 3-Tools finden Sie unter [Windows UI Library 3 - Project Reunion 0.5](../winui/winui3/index.md).

    Bei der Installation von Visual Studio müssen Sie die folgenden Komponenten einschließen:
    - Vergewissern Sie sich auf der Registerkarte **Workloads** im Installationsdialogfeld, dass **Entwicklung mit der Universellen Windows-Plattform (UWP)** ausgewählt ist.
    - Stellen Sie sicher, dass auf der Registerkarte **Einzelne Komponenten** im Installationsdialogfeld im Abschnitt **SDKs, Bibliotheken und Frameworks** die Option **Windows 10 SDK (10.0.19041.0)** ausgewählt ist.

    Zum Erstellen von .NET-Apps müssen Sie außerdem die folgenden Komponenten einschließen:
    - Vergewissern Sie sich, dass auf der Registerkarte **Workloads** im Installationsdialogfeld die Option **.NET Desktopentwicklung** ausgewählt ist.

    Zum Erstellen von C++-Apps müssen Sie außerdem die folgenden Komponenten einschließen:
    - Vergewissern Sie sich, dass auf der Registerkarte **Workloads** im Installationsdialogfeld die Option **Desktopentwicklung mit C++** ausgewählt ist.
    - Vergewissern Sie sich, dass im Bereich **Installationsdetails** auf der rechten Seite des Installationsdialogfelds im Abschnitt **Entwicklung für die universelle Windows-Plattform** die optionale Komponente **C++ (v142) Tools für die universelle Windows-Plattform** ausgewählt ist.

3. Wenn Sie zuvor die [WinUI 3-Vorschauerweiterung für Visual Studio](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates)installiert haben, deinstallieren Sie die Erweiterung. Weitere Informationen zum Deinstallieren einer Erweiterung finden Sie unter [Verwalten von Erweiterungen für Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions).

4. Stellen Sie sicher, dass auf Ihrem System eine NuGet-Paketquelle für den offiziellen NuGet-Dienstindex unter aktiviert `https://api.nuget.org/v3/index.json` ist. 

    1. Klicken Sie in Visual Studio auf  ->  **Extras NuGet Paket-Manager**  ->  **Paket-Manager Einstellungen,** um das Dialogfeld **Optionen** zu öffnen. 
    2. Wählen Sie im linken Bereich des Dialogfelds **Optionen** die Registerkarte **Paketquellen aus,** und stellen Sie sicher, dass eine Paketquelle für **nuget.org** vorhanden ist, `https://api.nuget.org/v3/index.json` die als Quell-URL auf verweist. Weitere Informationen finden Sie unter [Allgemeine NuGet-Konfigurationen.](/nuget/consume-packages/configuring-nuget-behavior)

5. Laden Sie die Projektreunion 0.5-Erweiterung für Visual Studio herunter, und installieren Sie sie. Es gibt zwei Versionen der Erweiterung: eine für Desktop-Apps (C#/.NET 5 oder C++/WinRT) und eine für UWP-Apps.

    So verwenden Sie Project Reunion in Desktop-Apps (C#/.NET 5 oder C++/WinRT):
    - Klicken Sie in Visual Studio 2019 auf **Erweiterungen** > **Erweiterungen verwalten**, suchen Sie nach **Project Reunion**, und installieren Sie die **Project Reunion**-Erweiterung.
    - Alternativ können Sie die [Project Reunion 0.5-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion) direkt aus dem Visual Studio Marketplace herunterladen und installieren.

    Um Project Reunion in UWP-Apps zu verwenden, müssen Sie eine Vorschauversion der Erweiterung installieren, die für Produktionsumgebungen nicht unterstützt wird:
    - Deinstallieren Sie alle vorhandenen Versionen von Project Reunion VSIX.
    - Klicken Visual Studio 2019 auf Erweiterungen Erweiterungen verwalten, und klicken Sie in der unteren linken Ecke auf Einstellungen für  >  Erweiterungen ändern.  Deaktivieren Sie automatische Updates für Pakete, die für alle Benutzer installiert wurden, bevor Sie die ältere Version installieren.
    - Laden Sie die [Erweiterung Project Reunion 0.5 Preview](https://download.microsoft.com/download/9/9/8/9981a84b-8fd8-4645-9dce-c62761601f17/ProjectReunion.Extension.vsix)herunter, und installieren Sie sie.

    Anweisungen zum Hinzufügen des VSIX-Pakets zu Visual Studio finden Sie unter [Verwalten von Erweiterungen für Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions).

    ![Screenshot: Installierte Projekt-Reunion-Erweiterung](images/reunion-extension-install.png)

6. Um WinUI 3-Tools wie Live Visual Tree, Hot Reload und Live Property Explorer in Visual Studio 2019 16.10 Preview zu verwenden, müssen Sie WinUI 3-Tools mit Visual Studio Preview-Funktionen aktivieren. Anweisungen finden Sie unter [How to Enable UI Tooling for WinUI 3 in VS 16.9 Preview 4 (Aktivieren von Benutzeroberflächentools für WinUI 3 in VS 16.9 Preview 4).](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)

7. Um alle Fehlerbehebungen aus der neuesten stabilen Version von Project Reunion 0.5 zu erhalten, müssen Sie Ihr .NET SDK explizit auf die neueste Version festlegen. Fügen Sie hierzu der CSPROJ-Datei die folgende Elementgruppe hinzu, und speichern Sie das Projekt:

    ```xml
    <ItemGroup>            
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" RuntimeFrameworkVersion="10.0.18362.16" />
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" TargetingPackVersion="10.0.18362.16" />
    </ItemGroup>
    ```

    Beachten Sie, dass diese Zeilen entfernt werden können, sobald .NET 5.0.6 im Mai verfügbar ist. 
    
## <a name="create-a-new-project-that-uses-project-reunion"></a>Erstellen eines neuen Projekts, das Project Reunion verwendet

Die Project Reunion 0.5-Erweiterungen für Visual Studio 2019 (einschließlich der Erweiterung für Desktop-Apps und der Vorschauerweiterung für UWP-Apps) stellen Projektvorlagen bereit, die Projekte mit einer WinUI 3-basierten Benutzeroberflächenebene generieren und Zugriff auf alle anderen Project Reunion-APIs bieten. Weitere Informationen zu den verfügbaren Projektvorlagen finden Sie unter [WinUI 3-Projektvorlagen in Visual Studio](..\winui\winui3\winui-project-templates-in-visual-studio.md).

> [!NOTE]
> Die Desktopprojektvorlagen (C#/.NET 5 und C++/WinRT) werden für die Verwendung in Produktionsumgebungen unterstützt. Die UWP-Projektvorlagen sind nur als Entwicklervorschau verfügbar und können nicht zum Erstellen von Apps für Produktionsumgebungen verwendet werden.

So erstellen Sie ein neues Projekt, das Project Reunion 0.5 verwendet:

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

Wenn Sie über ein vorhandenes Projekt verfügen, in dem Sie Project Reunion verwenden möchten, können Sie das NuGet-Paket Project Reunion 0.5 in Ihrem Projekt installieren. Dieses Szenario weist [einige Einschränkungen auf.](index.md#using-the-project-reunion-nuget-package-in-existing-projects)

1. Öffnen Sie ein vorhandenes Desktopprojekt (entweder C#/.NET 5 oder C++/WinRT) oder UWP-Projekt in Visual Studio 2019.

    > [!NOTE]
    > Wenn Sie über ein C#/.NET 5-Desktopprojekt verfügen, stellen Sie sicher, dass das **TargetFramework-Element** in der Projektdatei einem Windows 10-spezifischen .NET 5-Moniker wie **net5.0-windows10.0.19041.0** zugewiesen ist, damit Windows-Runtime-APIs aufgerufen werden können. Weitere Informationen finden Sie in [diesem Abschnitt](../../apps/desktop/modernize/desktop-to-uwp-enhance.md#net-5-use-the-target-framework-moniker-option).

2. Vergewissern Sie sich, dass [Paketverweise](/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf **Extras > NuGet-Paket-Manager > Paket-Manager-Einstellungen**.
    2. Achten Sie darauf, dass die Einstellung **Standardformat für Paketverwaltung** auf **PackageReference** festgelegt ist.

3. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf dein Projekt, und wähle **NuGet-Pakete verwalten** aus.

4. Wählen Sie im Fenster **NuGet-Paket-Manager** die Registerkarte **Durchsuchen** aus, und suchen Sie nach `Microsoft.ProjectReunion`.

5. Nachdem das **Microsoft.ProjectReunion**-Paket gefunden wurde, klicken Sie im rechten Bereich des Fensters **NuGet-Paket-Manager** auf **Installieren**.

    ![Screenshot der Installation des NuGet-Pakets "Project Reunion"](images/reunion-nuget-install.png)

6. Nachdem Sie das Paket installiert haben, können Sie die folgenden Project Reunion-APIs und -Komponenten in Ihrem Projekt verwenden:

    - [Verwalten von Ressourcen im MRT Core](mrtcore/mrtcore-overview.md)
    - [Rendern von Text mit DWriteCore](dwritecore.md)

## <a name="samples"></a>Beispiele

Die folgenden Project Reunion-Beispiele sind derzeit für Sie verfügbar.

- [Beispiel für den DWriteCore-Katalog](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery): Diese Beispielanwendung veranschaulicht die [DWriteCore](dwritecore.md)-API.
- [Beispiel für MRT Core](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore): Diese Beispielanwendung veranschaulicht die [MRT Core](mrtcore/mrtcore-overview.md)-API.
- [Hallo Welt-Beispiel](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp): Dieses Beispiel veranschaulicht eine grundlegende Integration in das Project Reunion-NuGet-Paket.
- [Xaml-Steuerelementkatalog:](https://aka.ms/winui3/xcg)Dies ist eine Beispiel-App, die alle WinUI 3-Steuerelemente in Aktion zeigt. 

## <a name="related-topics"></a>Zugehörige Themen

- [Erstellen von Windows-Desktop-Apps mit Project Reunion](index.md)
- [Bereitstellen von Apps, die Project Reunion verwenden](deploy-apps-that-use-project-reunion.md)
