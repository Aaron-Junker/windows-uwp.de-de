---
description: Erfahren Sie mehr über Project Reunion, welche Vorteile es für Entwickler bietet, was jetzt für Entwickler bereitsteht und wie Sie Feedback geben können.
title: Projektzusammenführung
ms.topic: article
ms.date: 03/09/2021
keywords: Windows Win32, Desktopentwicklung, Project Reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2354e7f42ab9c487275c66c9f709f8791ba5005e
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629240"
---
# <a name="build-desktop-windows-apps-with-project-reunion-05-preview-march-2021"></a>Erstellen von Windows-Desktop-Apps mit Project Reunion 0.5 Preview (März 2021)

Project Reunion ist der neue Satz von Entwicklerkomponenten und -tools, die die nächste Weiterentwicklung der Windows-App-Entwicklungsplattform darstellen. Project Reunion bietet einen einheitlichen Satz von APIs und Tools, die von jeder Desktop-Apps auf einer breiten Palette von Zielversionen des Windows 10-Betriebssystems auf konsistente Weise verwendet werden können. 

Project Reunion ersetzt nicht die bestehenden Windows-Desktop-App-Plattformen und Frameworks wie .NET (einschließlich Windows Forms und WPF) und C++/Win32. sondern ergänzt diese bestehenden Plattformen mit einem gemeinsamen Satz von APIs und Tools, auf die sich Entwickler plattformübergreifend verlassen können.

> [!NOTE]
> Project Reunion 0.5 Preview ist eine Entwicklervorschau. Wir empfehlen Ihnen, diese Version in Ihrer Entwicklungsumgebung zu testen. Berücksichtigen Sie jedoch, dass sich Project Reunion bis zur Veröffentlichung von Release 1.0 in vielerlei Hinsicht verändern wird. Project Reunion 0.5 Preview wird für Apps, die in Produktionsumgebungen verwendet werden, nicht unterstützt. **Project Reunion** ist ein Codename, der in einem zukünftigen Release geändert werden kann.

## <a name="benefits-of-project-reunion-for-windows-app-developers"></a>Vorteile von Project Reunion für Windows-App-Entwickler

Project Reunion stellt eine breite Palette von Windows-APIs mit Implementierungen bereit, die vom Betriebssystem entkoppelt sind und Entwicklern über NuGet-Pakete zur Verfügung gestellt werden. Project Reunion soll das Windows SDK nicht ersetzen. Das Windows SDK wird weiterhin unverändert funktionieren, und es gibt viele Kernkomponenten von Windows, die mit APIs, die über Betriebssystem- und Windows SDK-Versionen bereitgestellt werden, weiterentwickelt werden. Entwicklern wird empfohlen, Project Reunion in Ihrem eigenen Tempo zu übernehmen.

#### <a name="unified-api-surface-across-desktop-app-platforms"></a>Unified API-Oberfläche über Desktop-App-Plattformen hinweg

Entwickler, die Windows-Desktop-Apps erstellen möchten, müssen zwischen mehreren App-Plattformen und -Frameworks wählen. Jede Plattform stallt zwar viele Features und APIs bereit, die von mit anderen Plattformen erstellten Apps verwendet werden können, einige Features und APIs können jedoch nur von bestimmten Plattformen verwendet werden. Mit Project Reunion wird der Zugriff auf Windows-APIs für alle Windows 10-Desktop-Apps vereinheitlicht. Unabhängig davon, für welches App-Modell Sie sich entscheiden, haben Sie Zugriff auf den gleichen Satz von Windows-APIs, die in Project Reunion verfügbar sind.

Wir planen im Laufe der Zeit weitere Investitionen in Project Reunion, um die Unterschiede zwischen den verschiedenen App-Modellen zu verringern. Project Reunion wird sowohl WinRT-APIs als auch native C-APIs enthalten.

#### <a name="consistent-support-across-windows-10-versions"></a>Konsistente Unterstützung über Windows 10-Versionen hinweg

Da sich die Windows-APIs mit neuen Betriebssystemversionen weiterentwickeln, müssen Entwickler Techniken wie [versionsadaptiven Code](/windows/uwp/debug-test-perf/version-adaptive-code) verwenden, um alle Versionsunterschiede zu berücksichtigen und so die Zielgruppe ihrer Anwendung zu erreichen. Dadurch kann der Code und die Entwicklungsumgebung komplexer werden.

Project Reunion-APIs können unter Windows 10, Version 1809, und allen späteren Versionen von Windows 10 verwendet werden. Das bedeutet, dass Sie, solange Ihre Kunden Windows 10, Version 1809 oder höher verwenden, neue Project Reunion-APIs und -Features verwenden können, sobald sie veröffentlicht werden, und keinen an die Version angepassten Code schreiben müssen.

#### <a name="faster-release-cadence"></a>Schnellerer Versionsrhythmus

Neue Windows-APIs und -Funktionen sind in der Regel an Betriebssystemreleases gebunden, die ein- oder zweimal im Jahr erscheinen. Project Reunion wird Updates in einem kürzeren Intervall bereitstellen, sodass Sie früher und schneller Zugriff auf Innovationen in der Windows-Entwicklungsplattform erhalten, sobald diese erstellt werden.

## <a name="components-available-with-project-reunion"></a>Mit Project Reunion verfügbare Komponenten

Project Reunion 0.5 Preview enthält die folgenden Komponenten.

| Komponente | BESCHREIBUNG |
|---------|-------------|
| [Windows-UI-Bibliothek 3](../winui/winui3/index.md) | Windows-UI-Bibliothek (WinUI) 3 ist die nächste Generation der Benutzererfahrungs-Plattform (User Experience, UX) sowohl für Desktop (.NET und C++/Win32) als auch für UWP-Apps. Dieses Release enthält Visual Studio-Projektvorlagen, die Ihnen beim [Erstellen von Apps mit einer WinUI-basierten Benutzeroberfläche](..\winui\winui3\index.md#create-winui-projects) helfen, sowie ein NuGet-Paket, das die WinUI-Bibliotheken enthält.  |
| [Verwalten von Ressourcen mit MRT Core](mrtcore/mrtcore-overview.md) | MRT Core bietet APIs zum Laden und Verwalten von Ressourcen, die von Ihrer App verwendet werden. MRT Core ist eine optimierte Version des modernen [Windows-Ressourcenverwaltungssystems](/windows/uwp/app-resources/resource-management-system). |
| [Rendern von Text mit DWriteCore](dwritecore.md) | DWriteCore bietet Zugriff auf alle aktuellen DirectWrite-Features für die Textdarstellung, einschließlich eines geräteunabhängigen Textlayoutsystems, hardwarebeschleunigtem Text, Multiformat-Text und breiter Sprachunterstützung.  |

Weitere Informationen zu den zukünftigen Plänen der Integration weiterer Komponenten in Project Reunion finden Sie [hier](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md).

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

1. Stelle sicher, dass auf deinem Entwicklungscomputer Windows 10 (Version 1809, Build 17763) oder eine höhere Betriebssystemversion installiert ist.

2. Installieren Sie [Visual Studio 2019, Version 16.10 Preview](https://visualstudio.microsoft.com/vs/preview/) (oder höher), falls Sie dies nicht bereits getan haben.

    Bei der Installation von Visual Studio müssen Sie die folgenden Komponenten einschließen:
    - Vergewissern Sie sich auf der Registerkarte **Workloads**, dass **Entwicklung mit der Universellen Windows-Plattform (UWP)** ausgewählt ist.
    - Stellen Sie sicher, dass auf der Registerkarte **Einzelne Komponenten** im Abschnitt **SDKs, Bibliotheken und Frameworks** die Option **Windows 10 SDK (10.0.19041.0)** ausgewählt ist.

    Zum Erstellen von .NET-Apps müssen Sie außerdem die folgenden Komponenten einschließen:
    - Workload **Desktopentwicklung mit .NET**.

    Zum Erstellen von C++-Apps müssen Sie außerdem die folgenden Komponenten einschließen:
    - Workload **Desktopentwicklung mit C++** .
    - Die optionale Komponente **C++ (v142) UWP-Tools (Universelle Windows-Plattform)** für dem Workload **Entwicklung mit der Universellen Windows-Plattform (UWP)** .

3. Wenn Sie zuvor die [WinUI 3 Preview-Erweiterung](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates) von einem früheren WinUI 3 Preview-Release installiert haben, deinstallieren Sie die Erweiterung. Weitere Informationen zum Deinstallieren einer Erweiterung finden Sie unter [Verwalten von Erweiterungen für Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions).

4. Stellen Sie sicher, dass auf Ihrem System eine NuGet-Paketquelle für **nuget.org** aktiviert ist. Weitere Informationen finden Sie unter [Allgemeine NuGet-Konfigurationen](/nuget/consume-packages/configuring-nuget-behavior).

5. Klicken Sie in Visual Studio 2019 auf **Erweiterungen** > **Erweiterungen verwalten**, suchen Sie nach **Project Reunion**, und installieren Sie die **Project Reunion**-Erweiterung. Alternativ können Sie die [Project Reunion-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion) auch direkt vom Visual Studio Marketplace herunterladen und installieren. Anweisungen zum Hinzufügen des VSIX-Pakets zu Visual Studio finden Sie unter [Verwalten von Erweiterungen für Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions).

6. Um die WinUI 3-Tools wie Live Visual Tree, Hot Reload und Live Property Explorer verwenden zu können, müssen Sie die WinUI 3-Tools mit den Visual Studio Preview-Features, wie [hier in der Anleitung](https://github.com/microsoft/microsoft-ui-xaml/issues/4140) beschrieben.

## <a name="get-started-developing-with-project-reunion"></a>Einstieg in die Entwicklung mit Project Reunion

Sie können Project Reunion 0.5 Preview in neuen Apps verwenden, die mithilfe der WinUI 3-Projektvorlagen erstellt wurden, die in der Project Reunion-Erweiterung enthalten sind, oder Sie können Project Reunion-Komponenten in bestehenden Projekten verwenden, indem Sie ein NuGet-Paket installieren.

> [!NOTE]
> Project Reunion 0.5 Preview unterstützt nur MSIX-App-Pakete.

#### <a name="create-a-new-project-that-uses-project-reunion"></a>Erstellen eines neuen Projekts, das Project Reunion verwendet

Die Project Reunion 0.5 Preview-Erweiterung für Visual Studio 2019 enthält Projektvorlagen, die Projekte mit einem WinUI 3-basierten UI-Layer generieren und Zugriff auf alle anderen Project Reunion-APIs bieten. Sie können diese Vorlagen verwenden, um Desktop-Apps, die als MSIX-Pakete vorliegen (C#/.NET 5 oder C++/WinRT), oder UWP-Apps, die Project Reunion verwenden, zu erstellen. Weitere Informationen zu den verfügbaren Projektvorlagen finden Sie unter [Erstellen von WinUI-Projekten](..\winui\winui3\index.md#create-winui-projects).

Erstellen eines neuen Projekts, das Project Reunion 0.5 Preview verwendet:

1. Befolgen Sie die Anweisungen im folgenden Artikel:

    - [Erste Schritte mit WinUI 3 für Desktop-Apps](..\winui\winui3\get-started-winui3-for-desktop.md)
    - [Erste Schritte mit WinUI 3 für UWP-Apps](..\winui\winui3\get-started-winui3-for-uwp.md)

2. Nachdem Sie Ihr Projekt erstellt haben, erhalten Sie zusätzlich zu allen anderen Windows- und .NET-APIs, die typischerweise für Desktop- und UWP-Apps verfügbar sind, Zugriff auf die folgenden Project Reunion-APIs und -Komponenten.

    - [Windows-UI-Bibliothek 3](../winui/winui3/index.md)
    - [Verwalten von Ressourcen im MRT Core](mrtcore/mrtcore-overview.md)
    - [Rendern von Text mit DWriteCore](dwritecore.md)

Um sicherzustellen, dass Ihr neues Projekt Project Reunion verwendet, erweitern Sie den Knoten **Abhängigkeiten** > **Pakete** unter Ihrem Projekt im **Projektmappen-Explorer**. Unter diesem Knoten sollten mehrere **Microsoft.ProjectReunion**-Pakete aufgeführt sein, ähnlich wie in der folgenden Abbildung.

![Screenshot von Project Reunion-Paketen im Projektmappen-Explorer-Bereich](images/reunion-packages.png)

#### <a name="use-project-reunion-in-an-existing-project"></a>Verwenden von Project Reunion in einem vorhandenen Projekt

Wenn Sie über ein vorhandenes Desktopprojekt (entweder C#/.NET 5 oder C++/WinRT) verfügen, in dem Sie Project Reunion verwenden möchten, können Sie das Project Reunion 0.5 Preview-NuGet-Paket in Ihrem Projekt installieren. 

> [!NOTE]
> In diesem Szenario können Sie nicht-WinUI 3-Komponenten in Ihrem Projekt nur verwenden, wenn sie Teil von Project Reunion sind. Um WinUI 3 zu verwenden, müssen Sie ein neues Projekt mit einer der WinUI 3-Projektvorlagen erstellen, wie im vorherigen Abschnitt beschrieben.

1. Öffnen Sie ein vorhandenes Desktopprojekt (entweder C#/.NET 5 oder C++/WinRT) oder UWP-Projekt in Visual Studio 2019.

2. Vergewissern Sie sich, dass [Paketverweise](/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf **Extras > NuGet-Paket-Manager > Paket-Manager-Einstellungen**.
    2. Achten Sie darauf, dass die Einstellung **Standardformat für Paketverwaltung** auf **PackageReference** festgelegt ist.

3. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf dein Projekt, und wähle **NuGet-Pakete verwalten** aus.

4. Wählen Sie im Fenster **NuGet-Paket-Manager** die Registerkarte **Durchsuchen** aus, und suchen Sie nach `Microsoft.ProjectReunion`.

5. Nachdem das **Microsoft.ProjectReunion**-Paket gefunden wurde, klicken Sie im rechten Bereich des Fensters **NuGet-Paket-Manager** auf **Installieren**.

6. Nachdem Sie das Paket installiert haben, können Sie die folgenden Project Reunion-APIs und -Komponenten in Ihrem Projekt verwenden:

    - [Verwalten von Ressourcen im MRT Core](mrtcore/mrtcore-overview.md)
    - [Rendern von Text mit DWriteCore](dwritecore.md)

#### <a name="deploying-apps-that-use-project-reunion"></a>Bereitstellen von Apps, die Project Reunion verwenden

Derzeit müssen Apps, die Project Reunion verwenden, [MSIX-Pakete](/windows/msix) sein. Wenn Sie ein Projekt mit einer der WinUI-Projektvorlagen erstellen, die mit der Project Reunion-Erweiterung für Visual Studio bereitgestellt werden, enthält Ihr Projekt ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass die App in einem MSIX-Paket erstellt wird. Weitere Informationen zum Konfigurieren dieses Projekts, um ein MSIX-Paket für Ihre App zu erstellen, finden Sie unter [Verpacken einer Desktop- oder UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps).

Nachdem Sie ein MSIX-Paket für Ihre App erstellt haben, stehen Ihnen mehrere Möglichkeiten für die Bereitstellung auf anderen Computern zur Verfügung. Weitere Informationen finden Sie unter [Verwalten Ihrer MSIX-Bereitstellung](/windows/msix/desktop/managing-your-msix-deployment-overview).

> [!NOTE]
> Apps, die Project Reunion 0.5 Preview verwenden, können nicht im Store veröffentlicht werden.

## <a name="samples"></a>Beispiele

Die folgenden Project Reunion-Beispiele sind derzeit für Sie verfügbar.

- [Beispiel für den DWriteCore-Katalog](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery): Diese Beispielanwendung veranschaulicht die [DWriteCore](dwritecore.md)-API.
- [Beispiel für MRT Core](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore): Diese Beispielanwendung veranschaulicht die [MRT Core](mrtcore/mrtcore-overview.md)-API.
- [Hallo Welt-Beispiel](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp): Dieses Beispiel veranschaulicht eine grundlegende Integration in das Project Reunion-NuGet-Paket.

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

- Dieses Release wird für Apps, die in Produktionsumgebungen verwendet werden, nicht unterstützt. Es sind Fehler, Einschränkungen und andere Probleme zu erwarten.
- Dieses Release kann nur in Desktop-Apps, die als MSIX-Pakete vorliegen (C#/.NET 5 oder C++/Win32), verwendet werden. Sie kann nicht in nicht verpackten Desktop-Apps verwendet werden.
- Die [Tooleinschränkungen für WinUI 3](..\winui\winui3\index.md#developer-tools) gelten auch für alle Projekte, die Project Reunion 0.5 Preview verwenden.

## <a name="developer-roadmap"></a>Roadmap für Entwickler

Die neuesten Project Reunion-Pläne finden Sie auf unserer [GitHub-Seite](https://github.com/microsoft/ProjectReunion).

## <a name="give-feedback-and-contribute"></a>Feedback und Mitwirkung

Wir entwickeln Project Reunion als Open-Source-Projekt. Auf unserer [Github-Seite](https://github.com/microsoft/ProjectReunion) finden Sie viele weitere Informationen darüber, wie wir Project Reunion in die Tat umsetzen wollen, und wir möchten Sie einladen, Teil des Entwicklungsprozesses zu werden. Schauen Sie sich unseren [Leitfaden für Mitwirkende](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md) an, um Fragen zu stellen, Diskussionen zu starten oder Featurevorschläge zu machen. Wir möchten sicherstellen, dass Project Reunion Entwicklern wie Ihnen den größtmöglichen Nutzen bringt.
