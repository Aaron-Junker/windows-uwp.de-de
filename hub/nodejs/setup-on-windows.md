---
title: Einrichten von nodejs auf System eigenem Windows
description: Eine Anleitung, die Ihnen hilft, ihre Node. js-Entwicklungsumgebung direkt unter Windows einzurichten.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node. js, Windows 10, native Fenster, direkt unter Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 456aac17f61ab0add3d35a48c74e151fa15e9e83
ms.sourcegitcommit: 8efeb6672f759b1ea7e3e9e2f90e764480791142
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728471"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Einrichten der Node. js-Entwicklungsumgebung direkt unter Windows

Im folgenden finden Sie eine Schritt-für-Schritt-Anleitung für den Einstieg in die Verwendung von Node. js in einer nativen Windows-Entwicklungsumgebung.

## <a name="install-nvm-windows-nodejs-and-npm"></a>Installieren von NVM: Windows, Node. js und NPM

Es gibt mehrere Möglichkeiten, Node. js zu installieren. Es wird empfohlen, einen Versions-Manager zu verwenden, wenn sich Versionen sehr schnell ändern. Sie müssen wahrscheinlich zwischen mehreren Versionen wechseln, je nach den Anforderungen verschiedener Projekte, an denen Sie arbeiten. Knoten Versions-Manager, üblicherweise NVM genannt, ist die beliebteste Methode zum Installieren mehrerer Versionen von Node. js, ist aber nur für Mac/Linux verfügbar und wird unter Windows nicht unterstützt. Stattdessen durchlaufen wir die Schritte zum Installieren von NVM-Windows und verwenden Sie dann zum Installieren von Node. js und Node Package Manager (NPM). Es gibt [Alternative Versions Manager](#alternative-version-managers) , die im nächsten Abschnitt behandelt werden müssen.

> [!IMPORTANT]
> Es wird immer empfohlen, vorhandene Installationen von Node. js oder NPM von Ihrem Betriebssystem zu entfernen, bevor Sie einen Versions-Manager installieren, da die verschiedenen Installationstypen zu seltsamen und verwirrenden Konflikten führen können. Dies umfasst das Löschen aller vorhandenen nodejs-Installationsverzeichnisse (z. b. "c:\programme\nodejs"), die möglicherweise verbleiben. Der von NVM generierte Symlink überschreibt ein vorhandenes (noch leeres) Installationsverzeichnis nicht. Hilfe zum Entfernen vorheriger Installationen finden Sie unter [Vollständiges Entfernen von Node. js aus Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows).)

1. Öffnen Sie das [Repository "Windows-NVM](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) " in Ihrem Internetbrowser, und wählen Sie den Link **jetzt herunterladen** aus.
2. Laden Sie die Datei **NVM-Setup. zip** für die neueste Version herunter.
3. Öffnen Sie nach dem herunterladen die ZIP-Datei, und öffnen Sie die Datei **NVM-Setup. exe** .
4. Der Installations-Assistent für Setup-NVM-for-Windows führt Sie durch die Einrichtungsschritte, einschließlich der Auswahl des Verzeichnisses, in dem sowohl NVM-Windows als auch Node. js installiert werden.

    ![NVM für Windows-Installations-Assistent](../images/install-nvm-for-windows-wizard.png)

5. Gehen Sie nach Abschluss der Installation wie folgt vor: Öffnen Sie PowerShell, und verwenden Sie Windows-NVM, um aufzulisten, welche Versionen von Knoten derzeit installiert sind (sollte an diesem Punkt "None" lauten): `nvm ls`

    ![NVM-Liste, die keine Knoten Versionen anzeigt](../images/windows-nvm-powershell-no-node.png)

6. Installieren Sie die aktuelle Version von Node. js (zum Testen der neuesten Verbesserungen der Features, aber eher Probleme als die LTS-Version): `nvm install latest`
7. Installieren Sie die neueste stabile LTS-Version von Node. js (empfohlen), indem Sie zuerst nach der aktuellen LTS-Versionsnummer suchen: `nvm list available`, und dann die LTS-Versionsnummer mit: `nvm install <version>` ersetzen (ersetzen Sie `<version>` durch die Zahl, d. h.: `nvm install 12.14.0`).

    ![NVM-Liste der verfügbaren Versionen](../images/windows-nvm-list.png)

8. Listet die installierten Versionen von Knoten auf: `nvm ls`... Nun sollten Sie sehen, dass die beiden Versionen, die Sie soeben installiert haben, aufgeführt sind.

    ![NVM-Liste mit installierten Knoten Versionen](../images/windows-nvm-node-installs.png)

9. Geben Sie Folgendes ein, um zu überprüfen, welche Version von Node. js derzeit die Standardversion ist: `node --version`
10. Um die Version von Node. js zu ändern, die Sie für ein Projekt verwenden möchten, erstellen Sie ein neues Projektverzeichnis `mkdir NodeTest`und geben Sie das Verzeichnis `cd NodeTest`ein. Geben Sie dann `nvm use <version>` ersetzen `<version>` durch die gewünschte Versionsnummer (IE v 10.16.3 ") ein.
11. Überprüfen Sie, welche Version von NPM installiert ist: `npm --version`, wird diese Versionsnummer automatisch in die aktuelle Version von Node. js geändert.

## <a name="alternative-version-managers"></a>Alternative Versions-Manager

Obwohl Windows-NVM derzeit der beliebteste Versions-Manager für Node ist, gibt es Alternativen zu berücksichtigende Aspekte:

- [NVS](https://github.com/jasongin/nvs) (Node-Version Switcher) ist eine plattformübergreifende `nvm` Alternative mit der Möglichkeit, in [vs Code zu integrieren](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

- [Volta](https://github.com/volta-cli/volta#installing-volta) ist ein neuer Versions Manager des LinkedIn-Teams, der höhere Geschwindigkeit und plattformübergreifende Unterstützung beansprucht.

Zum Installieren von Volta als Versions Manager (anstelle von Windows-NVM) wechseln Sie in der [Anleitung](https://docs.volta.sh/guide/getting-started)für die ersten Schritte zum Abschnitt **Windows-Installation** , und führen Sie dann den Windows Installer aus, und befolgen Sie die Anweisungen in der Anleitung.

> [!IMPORTANT]
> Vor der Installation von Volta müssen Sie sicherstellen, dass der [Entwicklermodus](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) auf Ihrem Windows-Computer aktiviert ist.

Weitere Informationen zur Verwendung von Volta zum Installieren mehrerer Versionen von Node. js unter Windows finden Sie in der [Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain)-Dokumentation.

## <a name="install-your-favorite-code-editor"></a>Installieren Sie Ihren bevorzugten Code-Editor.

Es wird empfohlen, vs Code sowie das [node. js-Erweiterungspaket](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)für die Entwicklung mit Node. js unter Windows zu [Installieren](https://code.visualstudio.com). Installieren Sie alle, oder wählen Sie aus, und wählen Sie aus, was für Sie am nützlichsten erscheint.

So installieren Sie das Node. js-Erweiterungspaket:

1. Öffnen Sie das Fenster " **Erweiterungen** " (STRG + UMSCHALT + X) in vs Code.
2. Geben Sie im Suchfeld am oberen Rand des Fensters Erweiterungen Folgendes ein: "Node Extension Pack" (oder den Namen der gewünschten Erweiterung).
3. Wählen Sie **Installieren**aus. Nach der Installation wird die Erweiterung im Ordner "aktiviert" des Fensters **Erweiterungen** angezeigt. Sie können Einstellungen deaktivieren, deinstallieren oder konfigurieren, indem Sie das Zahnrad Symbol neben der Beschreibung der neuen Erweiterung auswählen.

Einige zusätzliche Erweiterungen, die Sie berücksichtigen sollten, sind u. A.:

- [Debugger für Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): Nachdem Sie die Entwicklung auf dem Server mit Node. js abgeschlossen haben, müssen Sie die Clientseite entwickeln und testen. Diese Erweiterung integriert ihren vs Code-Editor in ihren Chrome-Browser-Debugdienst, sodass Dinge etwas effizienter werden.
- [Keymaps von anderen Editoren](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): Diese Erweiterungen können Ihre Umgebung zu Hause machen, wenn Sie von einem anderen Text-Editor wechseln (z. b. Atom, Sublime, vim, emacs, Notepad + + usw.).
- [Einstellungs Synchronisierung](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): ermöglicht die Synchronisierung Ihrer vs Code Einstellungen über verschiedene Installationen hinweg mithilfe von github. Wenn Sie auf verschiedenen Computern arbeiten, können Sie die Umgebung auf diese Weise konsistent halten.

## <a name="install-git-optional"></a>Installieren von Git (optional)

Wenn Sie beabsichtigen, mit anderen zusammenzuarbeiten oder das Projekt auf einem Open-Source-Standort (wie GitHub) zu hosten, unterstützt vs Code die [Versionskontrolle mit git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). Auf der Registerkarte „Quellcodeverwaltung“ in VS Code werden alle Änderungen nachverfolgt und gängige Git-Befehle („Add“, „Commit“, „Push“, „Pull“) direkt in die Benutzeroberfläche integriert. Sie müssen zunächst Git installieren, um den Bereich „Quellcodeverwaltung“ zu aktivieren.

1. Laden Sie Git für Windows von der [git-scm-Website](https://git-scm.com/download/win) herunter, und installieren Sie Git.

2. Ein Installations-Assistent ist enthalten, der Ihnen eine Reihe von Fragen zu den Einstellungen für die Git-Installation stellt. Sie sollten alle Standardeinstellungen verwenden, sofern kein bestimmter Grund vorliegt, etwas zu ändern.

3. Wenn Sie noch nie zuvor mit Git gearbeitet haben, können Sie [GitHub-Leitfäden](https://guides.github.com/) für den Einstieg nutzen.

4. Es wird empfohlen, Ihren Knoten Projekten eine [gitignore-Datei](https://help.github.com/en/articles/ignoring-files) hinzuzufügen. Hier finden Sie [die gitignore-Standardvorlage von GitHub für Node. js](https://github.com/github/gitignore/blob/master/Node.gitignore).

## <a name="use-windows-subsystem-for-linux-for-production"></a>Verwenden des Windows-Subsystems für Linux für die Produktion

Die direkte Verwendung von Node. js unter Windows eignet sich hervorragend zum Erlernen und experimentieren, was Sie tun können. Wenn Sie bereit sind, Produktions bereite Webanwendungen zu erstellen, die in der Regel auf einem Linux-basierten Server bereitgestellt werden, wird die Verwendung des Windows-Subsystems für Linux Version 2 (WSL 2) für die Entwicklung von Node. js-Web-Apps empfohlen. Viele Node. js-Pakete und-Frameworks werden im Hinblick auf eine * nix-Umgebung erstellt, und die meisten Node. js-apps werden unter Linux bereitgestellt, sodass die Entwicklung auf WSL die Konsistenz zwischen ihren Entwicklungs-und Produktionsumgebungen sicherstellt. Informationen zum Einrichten einer WSL-Entwicklungsumgebung finden Sie unter [Einrichten der Node. js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md).

> [!NOTE]
> Wenn Sie sich in der (etwas seltenen) Situation befinden, in der eine Node. js-App auf einem Windows-Server gehostet werden muss, scheint das häufigste Szenario [ein Reverseproxy zu verwenden](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca). Hierfür gibt es zwei Möglichkeiten: 1) mithilfe von [iisnode](https://harveywilliams.net/blog/installing-iisnode) oder [direkt](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). Wir behalten diese Ressourcen nicht bei und empfehlen die [Verwendung von Linux-Servern zum Hosten Ihrer Node. js-apps](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs).
