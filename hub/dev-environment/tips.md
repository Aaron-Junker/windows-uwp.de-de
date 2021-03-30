---
title: Workflow- und Leistungstipps für Windows 10
description: Tipps zur Verbesserung Ihres Entwicklungsworkflows unter Windows 10.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Developer, Tipps, Leistung, WSL
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 9df1bd3e903854281306825a9dba3f181e8c8030
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254617"
---
# <a name="tips-for-improving-performance-and-development-workflows"></a>Tipps zum Verbessern von Leistungs und Entwicklungsworkflows

Wir haben einige Tipps gesammelt, von denen wir hoffen, dass Sie Ihnen dabei helfen, Ihren Workflow effizienter und angenehmer zu gestalten. Haben Sie zusätzliche Tipps, die Sie teilen möchten? Reichen Sie oben über die Schaltfläche „Bearbeiten“ oder unten über die Schaltfläche „Feedback“ ein Pull Request ein, und wir fügen es eventuell zur Liste hinzu.

> [!NOTE]
> Ob Leistungsprobleme im Zusammenhang mit der Entwicklung unter Windows 10 auftreten, wie z. B:
> - Entwicklungstools (z. B. Compiler, Linker usw.), die unter Windows langsamer laufen als erwartet.
> - Laufzeitplattformen (z. B. Node, .NET, Python), die unter Windows langsamer laufen als auf anderen Plattformen.
> - Ihre Apps haben Leistungsprobleme im Zusammenhang mit Datei-E/A, Netzwerken, Prozesserstellung. 
> 
> Informieren Sie uns, indem Sie im [Windows Developer (WinDev) Issues-Repository](https://github.com/microsoft/WinDev) ein Problem melden.

## <a name="use-shortcuts-to-open-a-project-in-vs-code-or-windows-file-explorer"></a>Verwenden von Verknüpfungen, um ein Projekt in VS Code oder im Windows-Datei-Explorer zu öffnen

Sie können VS Code von der Befehlszeile aus in das Projekt starten, das Sie mit dem folgenden Befehl geöffnet haben: `code .`. Oder Sie öffnen Ihr Projektverzeichnis über die Befehlszeile mit dem Windows-Datei-Explorer, indem Sie `explorer.exe .` aus Windows oder Ihrer WSL-Distribution verwenden. Möglicherweise müssen Sie die ausführbare VS Code-Datei Ihrer PATH-Umgebungsvariablen hinzufügen, wenn sie nicht standardmäßig funktioniert. Weitere Informationen zum [Starten von der Befehlszeile aus](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line).

![Windows-Datei-Explorer-Screenshot](../images/wsl-file-explorer.png)

## <a name="use-the-credential-manager-to-your-streamline-authentication-process"></a>Verwenden der Anmeldeinformationsverwaltung zur Optimierung Ihres Authentifizierungsvorgangs

Wenn Sie Git für die Versionskontrolle und Zusammenarbeit verwenden, können Sie Ihren Authentifizierungsprozess optimieren, indem Sie [Git Credential Manager einrichten](/windows/wsl/tutorials/wsl-git#git-credential-manager-setup), um Ihre Tokens in der Windows-Anmeldeinformationsverwaltung zu speichern. Es wird ferner empfohlen, Ihrem Projekt [eine .gitignore-Datei hinzuzufügen](/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file).

## <a name="use-wsl-for-testing-your-production-pipeline-before-deploying-to-the-cloud"></a>Verwenden von WSL zum Testen Ihrer Produktionspipeline vor der Bereitstellung in der Cloud

Mit dem Windows-Subsystem für Linux können Entwickler eine GNU-/Linux-Umgebung (einschließlich der meisten Befehlszeilentools, Hilfsprogramme und Anwendungen) direkt unter Windows unverändert ausführen, ohne den Mehraufwand eines traditionellen virtuellen Computers oder eines Dual-Boot-Setups betreiben zu müssen.

Die WSL richtet sich an ein Entwicklerpublikum mit der Absicht, als Teil einer inneren Entwicklungsschleife verwendet zu werden. Nehmen wir an, Sam erstellt eine CI/CD-Pipeline (Continuous Integration & Continuous Delivery) und möchte diese zunächst auf einem lokalen Computer (Laptop) testen, bevor er sie in der Cloud bereitstellt. Sam kann WSL (& WSL 2 zur Verbesserung von Geschwindigkeit und Leistung) aktivieren und dann lokal (auf dem Laptop) eine Linux-Ubuntu-Originalinstanz mit den von jeweiligen bevorzugten Bash-Befehlen und Tools verwenden. Sobald die Entwicklungspipeline lokal überprüft wurde, kann Sam diese CI/CD-Pipeline in die Cloud pushen, indem er sie in einen Dockercontainer umwandelt, und den Container in eine Cloudinstanz pushen, wo er auf einer produktionsbereiten Ubuntu-VM ausgeführt wird.

Weitere Möglichkeiten zur Verwendung von WSL finden Sie in dieser [Episode von „Tabs vs Spaces“ zu WSL 2](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux).

## <a name="improve-performance-speed-for-wsl-by-not-crossing-over-file-systems"></a>Verbessern der Leistungsgeschwindigkeit für WSL durch Vermeiden von Überschreitungen zwischen von Dateisystemen

Wenn Sie sowohl mit Windows als auch dem Windows-Subsystem für Linux arbeiten, haben Sie zwei Dateisysteme installiert: NTSF (Windows) und WSL (Ihre Linux-Distribution). Für eine schnelle Leistung stellen Sie sicher, dass Ihre Projektdateien im selben System wie die von Ihnen verwendeten Tools gespeichert sind. Erfahren Sie mehr über [Auswählen des richtigen Dateisystems für schnellere Leistung](/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance).

## <a name="improve-build-speeds-by-adding-windows-defender-exclusions"></a>Verbessern der Erstellungsgeschwindigkeit durch Hinzufügen von Ausnahmen für Windows Defender

Sie können Ihre Buildgeschwindigkeit verbessern, indem Sie Ihre Windows Defender-Einstellungen aktualisieren, um Ausschlüsse für Projektordner oder Dateitypen hinzuzufügen, die Sie als ausreichend vertrauenswürdig einstufen, um eine Überprüfung auf Sicherheitslücken zu vermeiden. Weitere Informationen zum [Aktualisieren der Windows Defender-Einstellungen zur Verbesserung der Leistung](../android/defender-settings.md).

![Windows Defender-Screenshot](../images/windows-defender-exclusions.png)

## <a name="launch-all-your-command-lines-in-windows-terminal-at-once"></a>Gleichzeitiges Starten aller Befehlszeilen in Windows Terminal

* Sie können mehrere Befehlszeilen wie PowerShell, Ubuntu und Azure CLI in einem einzelnen Fenster mit mehreren Bereichen starten, indem Sie [Windows-Terminal-Befehlszeilenargumente](/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes) verwenden. Nachdem Sie [Windows-Terminal](/windows/terminal/get-started), [WSL/Ubuntu](/windows/wsl/install-win10) und [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) installiert haben, geben Sie diesen Befehl in PowerShell ein, um ein neues Fenster mit mehreren Bereichen mit allen drei Befehlszeilen zu öffnen:

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

## <a name="share-your-tips"></a>Teilen Sie Ihre Tipps

Haben Sie Tipps, wie Sie anderen Entwicklern, die Windows verwenden, helfen können, ihren Workflow zu verbessern? [Senden Sie eine Pull-Anforderung](https://github.com/MicrosoftDocs/windows-uwp/edit/docs/hub/dev-environment/overview.md), und fügen Sie Ihren Tipp zur Seite hinzu, oder [melden Sie ein Problem](https://github.com/MicrosoftDocs/windows-uwp/issues/new?title=&body=%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20Details%0A%0A%E2%9A%A0%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20docs.microsoft.com%20%E2%9E%9F%20GitHub%20issue%20linking.*%0A%0A*%20ID%3A%207779352b-7b4e-dad8-7c1b-b9aba2c5e561%0A*%20Version%20Independent%20ID%3A%20a5b81b80-87a1-b6e2-8936-baf6c1a0b9c5%0A*%20Content%3A%20%5BSet%20up%20your%20Windows%2010%20development%20environment%5D(https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows%2Fdev-environment%2Foverview)%0A*%20Content%20Source%3A%20%5Bhub%2Fdev-environment%2Foverview.md%5D(https%3A%2F%2Fgithub.com%2FMicrosoftDocs%2Fwindows-uwp%2Fblob%2Fdocs%2Fhub%2Fdev-environment%2Foverview.md)%0A*%20Product%3A%20**dev-environment**%0A*%20Technology%3A%20**windows-nodejs**), wenn Sie einen Tipp zu einem bestimmten Thema hinzufügen möchten.

Haben Sie leistungsbezogene Probleme, um die wir uns kümmern sollen? Melden Sie sie im neuen [WinDev Issues-Repository ](https://github.com/microsoft/windev).

Unser Dank geht an die Entwickler. Wir hören zu und versuchen, Ihre Erfahrungen zu verbessern!
