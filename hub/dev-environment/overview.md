---
title: Einrichten der Entwicklungsumgebung unter Windows 10
description: Eine umfassende Anleitung, die Ihnen hilft, Ihre Entwicklungsumgebung unter Windows einzurichten und Ihre bevorzugten Tools und Codesprachen zu installieren.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Fenster einrichten, Entwicklungsumgebung, Entwicklertools, Entwicklungspfade, Microsoft, Windows, Developer, Tipps, Leistung, WSL, Terminal, nodejs, python
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 58c68522fdb3ffe11c4e11dff48fb49faa79b830
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824034"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>Einrichten der Entwicklungsumgebung unter Windows 10

Dieser Leitfaden erleichtert Ihnen den Einstieg bei der Installation der Sprachen und Tools, die Sie für die Entwicklung mithilfe von Windows oder Windows-Subsystem für Linux benötigen.

## <a name="development-paths"></a>Entwicklungspfade

:::row:::
    :::column:::
       [![JavaScrip NodeJS-Symbol](../images/nodejs-logo.png)](../nodejs/index.yml)<br>
        **[Erste Schritte mit NodeJS](../nodejs/index.yml)**<br>
        Installieren von NodeJS und Einrichten Ihrer Entwicklungsumgebung unter Windows oder Windows-Subsystem für Linux.
    :::column-end:::
    :::column:::
       [![Python-Symbol](../images/python-logo.png)](../python/index.yml)<br>
        **[Erste Schritte mit Python](../python/index.yml)**<br>
        Installieren von Python und Einrichten Ihrer Entwicklungsumgebung unter Windows oder Windows-Subsystem für Linux.
    :::column-end:::
    :::column:::
       [![Android-Symbol](../images/android-logo.png)](/windows/android)<br>
        **[Erste Schritte mit Android](/windows/android)**<br>
        Installieren von Android Studio, oder Auswählen eine plattformübergreifenden Lösung wie Xamarin, React oder Cordova und Einrichten Ihrer Entwicklungsumgebung unter Windows.
    :::column-end:::
    :::column:::
       [![Windows Desktop-Symbol](../images/windows-logo.png)](../apps/index.yml)<br>
        **[Erste Schritte mit Windows Desktop](../apps/index.yml)**<br>
        Erste Schritte beim Entwickeln von Desktop-Apps für Windows 10 mithilfe von UWP, Win32, WPF, Windows Forms oder Aktualisieren und Bereitstellen vorhandener Desktop-Apps mit MSIX und XAML Islands.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C / C++](../images/c-logo.png)](/cpp/)<br>
        **[Erste Schritte mit C++ und C](/cpp/)**<br>
        Beginnen Sie mit dem Entwickeln von Apps, Diensten und Tools mit C++, C und Assembly.
    :::column-end:::
    :::column:::
       [![C#-Symbol](../images/csharp-logo.png)](/dotnet/csharp/)<br>
        **[Erste Schritte mit C#](/dotnet/csharp/)**<br>
        Beginnen Sie mit dem Entwickeln von Apps mit C# und .NET Core.
    :::column-end:::
    :::column:::
       [![Symbol für Docker-Desktop für Windows](../images/docker-logo.png)](../dev-environment/docker/overview.md)<br>
        **[Erste Schritte mit Docker Desktop für Windows](../dev-environment/docker/overview.md)**<br>
        Erstellen Sie Remoteentwicklungscontainer mit Unterstützung von Visual Studio, VS Code, .NET, Windows-Subsystem für Linux oder einer Vielzahl von Azure-Diensten.
    :::column-end:::
    :::column:::
       [![PowerShell-Symbol](../images/powershell.png)](/powershell/)<br>
        **[Erste Schritte mit PowerShell](/powershell/)**<br>
        Beginnen Sie mit der plattformübergreifenden Aufgabenautomatisierung und Konfigurationsverwaltung unter Verwendung von PowerShell, einer Befehlszeilenshell und einer Skriptsprache.
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>Tools und Plattformen

:::row:::
    :::column:::
       [![WSL-Symbol](../images/windows-linux-dev-env.png)](/windows/wsl/)<br>
        **[Windows-Subsystem für Linux](/windows/wsl/)**<br>
        Verwenden Ihrer bevorzugten Linux-Distribution vollständig in Windows integriert (kein Dual-Boot mehr erforderlich).<br>
        [Installieren von WSL](/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows Terminal-Symbol](../images/terminal.png)](/windows/terminal/)<br>
        **[Windows-Terminal](/windows/terminal/)**<br>
        Anpassen Ihrer Terminal-Umgebung für die Zusammenarbeit mit mehreren Befehlszeilenshells.
        <br>
        [Installieren des Terminals](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows-Paket-Manager-Symbol](../images/winget.png)](../package-manager/index.md)<br>
        **[Windows-Paket-Manager](../package-manager/index.md)**<br>
        Verwenden des winget.exe-Clients, dem umfassenden Paket-Manager, mit Ihrer Befehlszeile zum Installieren von Anwendungen unter Windows 10.<br>
        [Installieren des Windows-Paket-Managers (öffentliche Vorschau)](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys-Symbol](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        Verfeinern und Optimieren Ihrer Windows-Erfahrung für erhöhte Produktivität mit diesem Satz von Hilfsprogrammen für Poweruser.<br>
        [Installieren von PowerToys (öffentliche Vorschau)](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code-Symbol](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        Ein schlanker Quellcode-Editor mit integrierter Unterstützung für JavaScript, TypeScript, Node.js, einem umfangreichen Ökosystem von Erweiterungen (C++, C#, Java, Python, PHP, Go) und Runtimes (wie .NET und Unity).<br>
        [Installieren von VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio-Symbol](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio](/visualstudio/windows/)**<br>
        Eine integrierte Entwicklungsumgebung, mit der Sie Code bearbeiten, debuggen und erstellen sowie Apps veröffentlichen können, einschließlich Compilern, IntelliSense-Codevervollständigung und vielen weiteren Funktionen.<br>
        [Installieren von Visual Studio](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure-Symbol](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        Eine vollständige Cloudplattform zum Hosten Ihrer vorhandenen Apps und zur Optimierung neuer Entwicklungen. Azure-Dienste integrieren alles, was Sie benötigen, um Ihre Apps zu entwickeln, zu testen, bereitzustellen und zu verwalten.<br>
        [Einrichten eines Azure-Kontos](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET-Symbol](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](/dotnet/standard/get-started/)**<br>
        Eine Open Source-Entwicklungsplattform mit Tools und Bibliotheken zum Entwickeln jedes App-Typs, einschließlich Web, Mobil, Desktop, Gaming, IoT, Cloud und Microservices.<br>
        [Installieren von .NET](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

## <a name="run-windows-and-linux"></a>Ausführen von Windows und Linux

Das Windows-Subsystem für Linux (WSL) ermöglicht es Entwicklern, ein Linux-Betriebssystem parallel zu Windows auszuführen. Beide teilen sich die gleiche Festplatte (und können auf die Dateien des jeweils anderen zugreifen), die Zwischenablage unterstützt das Kopieren und Einfügen zwischen den beiden, kein duales Booten erforderlich. WSL ermöglicht Ihnen die Verwendung von BASH und stellt eine Umgebung bereit, die Mac-Benutzern sehr vertraut ist.
- Weitere Informationen finden Sie in den [WSL-Dokumenten](/windows/wsl) oder [WSL-Videos auf Channel 9](https://channel9.msdn.com/Search?term=wsl&lang-en=true).

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

Sie können auch Windows Terminal verwenden, um alle Ihre bevorzugten Befehlszeilentools im selben Fenster mit mehreren Registerkarten oder in mehreren Fenstern zu öffnen, egal ob es sich dabei um PowerShell, Windows-Eingabeaufforderung, Ubuntu, Debian, Azure CLI, Oh-my-Zsh, Git Bash oder alle oben genannten handelt.

Weitere Informationen finden Sie in den [Windows Terminal-Dokumenten](/windows/terminal) oder [Windows Terminal-Videos auf Channel 9](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true).

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>Übergang zwischen Mac und Windows

Sehen Sie sich unsere [Anleitung für den Übergang zwischen einer Mac- und Windows-](./mac-to-windows.md)-Entwicklungsumgebung (oder einer Windows-Subsystem für Linux-Entwicklungsumgebung) an. Dies kann Ihnen dabei helfen, folgende Unterschiede abzubilden:

- [Tastenkombinationen](./mac-to-windows.md#keyboard-shortcuts)
- [Trackpad-Tastenkombinationen](./mac-to-windows.md#trackpad-shortcuts)
- [Terminal- und Shell-Tools](./mac-to-windows.md#command-line-shells-and-terminals)
- [Apps und Hilfsprogramme](./mac-to-windows.md#apps-and-utilities)

![Office-Image](../images/flashy-office3.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Tipps zum Verbessern Ihres Workflows](./tips.md)
- [Storys von Entwicklern, die von Mac zu Windows gewechselt haben](./dev-stories.md)
- [Beliebte Tutorials, Kurse und Codebeispiele](./tutorials.md)
- [Dokumentation zum Game Stack von Microsoft](/gaming/)