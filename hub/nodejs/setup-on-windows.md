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
ms.openlocfilehash: eaeee6e2d55bcb9221d88bd87ebeafc7c45d0a5d
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315084"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Einrichten der Node. js-Entwicklungsumgebung direkt unter Windows

Im folgenden finden Sie eine Schritt-für-Schritt-Anleitung für den Einstieg in die Verwendung von Node. js in einer nativen Windows-Entwicklungsumgebung.

> [!NOTE]
> Die Verwendung von Node. js unter Windows ist sicherlich eine brauchbare Option. Wir empfehlen im Allgemeinen, Windows-Subsystem für Linux (WSL) für die Entwicklung von Node. js-Web-Apps zu verwenden. Viele Node. js-Pakete und-Frameworks werden im Hinblick auf eine * nix-Umgebung erstellt, und die meisten Node. js-apps werden unter Linux bereitgestellt, sodass die Entwicklung auf WSL die Konsistenz zwischen ihren Entwicklungs-und Produktionsumgebungen sicherstellt. Informationen zum Einrichten einer WSL-Entwicklungsumgebung finden Sie unter [Einrichten der Node. js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md).

## <a name="install-nvm-windows-nodejs-and-npm"></a>Installieren von NVM: Windows, Node. js und NPM

Es gibt mehrere Möglichkeiten, Node. js zu installieren. Es wird empfohlen, einen Versions-Manager zu verwenden, wenn sich Versionen sehr schnell ändern. Sie müssen wahrscheinlich zwischen mehreren Versionen wechseln, je nach den Anforderungen verschiedener Projekte, an denen Sie arbeiten. Knoten Versions-Manager, üblicherweise NVM genannt, ist die beliebteste Methode zum Installieren mehrerer Versionen von Node. js, ist aber nur für Mac/Linux verfügbar und wird unter Windows nicht unterstützt. Stattdessen durchlaufen wir die Schritte zum Installieren von NVM-Windows und verwenden Sie dann zum Installieren von Node. js und Node Package Manager (NPM). Es gibt [Alternative Versions Manager](#alternative-version-managers) , die im nächsten Abschnitt behandelt werden müssen.

> [!IMPORTANT]
> Es wird immer empfohlen, vorhandene Installationen von Node. js oder NPM von Ihrem Betriebssystem zu entfernen, bevor Sie einen Versions-Manager installieren, da die verschiedenen Installationstypen zu seltsamen und verwirrenden Konflikten führen können. Dies umfasst das Löschen aller vorhandenen nodejs-Installationsverzeichnisse (z. b. "c:\programme\nodejs"), die möglicherweise verbleiben. Der von NVM generierte Symlink überschreibt ein vorhandenes (noch leeres) Installationsverzeichnis nicht. Hilfe zum Entfernen vorheriger Installationen finden Sie unter [Vollständiges Entfernen von Node. js aus Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows).)

1. Öffnen Sie das [Repository "Windows-NVM](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) " in Ihrem Internetbrowser, und wählen Sie den Link **jetzt herunterladen** aus.
2. Laden Sie die Datei **NVM-Setup. zip** für die neueste Version herunter.
3. Öffnen Sie nach dem herunterladen die ZIP-Datei, und öffnen Sie die Datei **NVM-Setup. exe** .
4. Der Installations-Assistent für Setup-NVM-for-Windows führt Sie durch die Einrichtungsschritte, einschließlich der Auswahl des Verzeichnisses, in dem sowohl NVM-Windows als auch Node. js installiert werden.

    ![NVM für Windows-Installations-Assistent](../images/install-nvm-for-windows-wizard.png)

5. Nachdem die Installation beendet wurde. Öffnen Sie PowerShell, und verwenden Sie Windows-NVM, um aufzulisten, welche Versionen von Knoten zurzeit installiert sind (sollte an diesem Punkt "None" lauten): `nvm ls`

    ![NVM-Liste, die keine Knoten Versionen anzeigt](../images/windows-nvm-powershell-no-node.png)

6. Installieren Sie die aktuelle Version von Node. js (zum Testen der neuesten Verbesserungen der Features, aber eher Probleme als die LTS-Version): `nvm install latest`
7. Installieren Sie die neueste stabile LTS-Version von Node. js (empfohlen), indem Sie zuerst nach der aktuellen LTS-Versionsnummer suchen: `nvm list available` und dann die LTS-Versionsnummer mit: `nvm install <version>` (Ersetzen von `<version>` durch die Nummer, IE: `nvm install 10.16.3`).

    ![NVM-Liste der verfügbaren Versionen](../images/windows-nvm-list.png)

8. Listet die installierten Versionen von Knoten auf: `nvm ls`... Nun sollten Sie sehen, dass die beiden Versionen, die Sie soeben installiert haben, aufgeführt sind.

    ![NVM-Liste mit installierten Knoten Versionen](../images/windows-nvm-node-installs.png)

9. Geben Sie Folgendes ein, um zu überprüfen, welche Version von Node. js derzeit die Standardversion ist: `node --version`
10. Um die Version von Node. js zu ändern, die Sie für ein Projekt verwenden möchten, erstellen Sie ein neues Projektverzeichnis `mkdir NodeTest`, und geben Sie das Verzeichnis `cd NodeTest` ein. Geben Sie dann `nvm use <version>` ein, und ersetzen Sie `<version>` durch die Versionsnummer, die Sie verwenden möchten (IE v 10.16.3 ").
11. Überprüfen Sie, welche Version von NPM installiert ist: `npm --version` wird diese Versionsnummer automatisch in die aktuelle Version von Node. js geändert.

## <a name="alternative-version-managers"></a>Alternative Versions-Manager

Obwohl Windows-NVM derzeit der beliebteste Versions-Manager für Node ist, gibt es Alternativen zu berücksichtigende Aspekte:

- [NVS](https://github.com/jasongin/nvs) (Node-Version Switcher) ist eine plattformübergreifende `nvm`-Alternative mit der Möglichkeit, in [vs Code zu integrieren](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).
- 
- [Volta](https://github.com/volta-cli/volta#installing-volta) ist ein neuer Versions Manager des LinkedIn-Teams, der höhere Geschwindigkeit und plattformübergreifende Unterstützung beansprucht.

Zum Installieren von Volta als Versions Manager (anstelle von Windows-NVM) wechseln Sie in der [Anleitung](https://docs.volta.sh/guide/getting-started)für die ersten Schritte zum Abschnitt **Windows-Installation** , und führen Sie dann den Windows Installer aus, und befolgen Sie die Anweisungen in der Anleitung.

> [!IMPORTANT]
> Vor der Installation von Volta müssen Sie sicherstellen, dass der [Entwicklermodus](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) auf Ihrem Windows-Computer aktiviert ist.

Weitere Informationen zur Verwendung von Volta zum Installieren mehrerer Versionen von Node. js unter Windows finden Sie in der [Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain)-Dokumentation.

## <a name="install-your-favorite-code-editor"></a>Installieren Sie Ihren bevorzugten Code-Editor.

Es wird empfohlen, vs Code sowie das [node. js-Erweiterungspaket](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)für die Entwicklung mit Node. js unter Windows zu [Installieren](https://code.visualstudio.com).

Das Node. js-Erweiterungspaket umfasst Folgendes:

- ES lint: ein Tool zum "linting" des Codes. Linting analysiert Ihren Code und warnt Sie vor potenziellen Fehlern.
- NPM: führen Sie NPM-Skripts über die Befehls Palette aus, und überprüfen Sie die in "Package. JSON" definierten installierten Module.
- JavaScript-Ausschnitte (ES6): Fügt Code Ausschnitte für die JavaScript-Entwicklung in der ES6-Syntax hinzu.
- Suche node_modules: Suchen Sie in Ihrem Projekt schnell nach Knoten Modulen.
- NPM IntelliSense: fügt IntelliSense für NPM-Module in Ihrem Code hinzu.
- Path IntelliSense: schließt Dateinamen in Ihrem Code automatisch ab.

Installieren Sie alle, oder wählen Sie aus, und wählen Sie aus, was für Sie am nützlichsten erscheint.

So installieren Sie das Node. js-Erweiterungspaket:

1. Öffnen Sie das Fenster " **Erweiterungen** " (STRG + UMSCHALT + X) in vs Code.
2. Geben Sie im Suchfeld am oberen Rand des Fensters Erweiterungen Folgendes ein: "Node Extension Pack" (oder der Name der gewünschten Erweiterung).
3. Wählen Sie **Installieren**aus. Nach der Installation wird die Erweiterung im Ordner "aktiviert" des Fensters **Erweiterungen** angezeigt. Sie können Einstellungen deaktivieren, deinstallieren oder konfigurieren, indem Sie das Zahnrad Symbol neben der Beschreibung der neuen Erweiterung auswählen.

Einige zusätzliche Erweiterungen, die Sie berücksichtigen sollten, sind u. A.:

- [Debugger für Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): Wenn Sie die Entwicklung auf dem Server mit Node. js abgeschlossen haben, müssen Sie die Clientseite entwickeln und testen. Diese Erweiterung integriert ihren vs Code-Editor in ihren Chrome-Browser-Debugdienst, sodass Dinge etwas effizienter werden.
- [Keymaps von anderen Editoren](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): Diese Erweiterungen können Ihre Umgebung zu Hause machen, wenn Sie von einem anderen Text-Editor wechseln (z. b. Atom, Sublime, vim, emacs, Notepad + + usw.).
- [Synchronisierungs Einstellungen](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): Ermöglicht die Synchronisierung Ihrer vs Code Einstellungen über verschiedene Installationen mithilfe von github. Wenn Sie auf unterschiedlichen Computern arbeiten, können Sie Ihre Umgebung auf diese Weise konsistent halten.

## <a name="install-git-optional"></a>Installieren von git (optional)

Wenn Sie beabsichtigen, mit anderen zusammenzuarbeiten oder das Projekt auf einem Open-Source-Standort (wie GitHub) zu hosten, unterstützt vs Code die [Versionskontrolle mit git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). Auf der Registerkarte Quell Code Verwaltung in vs Code werden alle Änderungen nachverfolgt, und es werden allgemeine git-Befehle (Add, Commit, Push, Pull) direkt in die Benutzeroberfläche integriert. Sie müssen zuerst git installieren, um die Quell Code Verwaltung zu über schalten.

1. Laden Sie git für Windows von [der git-SCM-Website](https://git-scm.com/download/win)herunter, und installieren Sie es.

2. Ein Installations-Assistent ist enthalten, mit dem Sie eine Reihe von Fragen zu den Einstellungen für die git-Installation erhalten. Es wird empfohlen, alle Standardeinstellungen zu verwenden, es sei denn, Sie haben einen bestimmten Grund, etwas zu ändern.

3. Wenn Sie noch nie mit git gearbeitet haben, können Ihnen [GitHub-Leitfäden](https://guides.github.com/) beim Einstieg helfen.

4. Es wird empfohlen, Ihren Knoten Projekten eine [gitignore-Datei](https://help.github.com/en/articles/ignoring-files) hinzuzufügen. Hier finden Sie [die gitignore-Standardvorlage von GitHub für Node. js](https://github.com/github/gitignore/blob/master/Node.gitignore).
