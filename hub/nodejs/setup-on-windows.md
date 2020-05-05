---
title: Einrichten von NodeJS nativ unter Windows
description: Diese Anleitung soll dir helfen, deine Node.js-Entwicklungsumgebung direkt unter Windows einzurichten.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, Windows 10, nativ unter Windows, direkt unter Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: fe1943da8c1de4f4fced5dec67079522d83f9a19
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82173466"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Einrichten einer Node.js-Entwicklungsumgebung direkt unter Windows

Im Folgenden findest du eine ausführliche Anleitung für die ersten Schritte bei der Verwendung von Node.js in einer nativen Windows-Entwicklungsumgebung.

## <a name="install-nvm-windows-nodejs-and-npm"></a>Installieren von nvm-windows, Node.js und npm

Node.js kann auf unterschiedliche Weise installiert werden. Es wird empfohlen, einen Versions-Manager zu verwenden, da sich die Versionen sehr schnell ändern. Du musst wahrscheinlich je nach den Anforderungen einzelner Projekte, an denen du arbeitest, zwischen verschiedenen Versionen wechseln. Node Version Manager (meist als „nvm“ bezeichnet) ist die beliebteste Methode zum Installieren mehrerer Versionen von Node.js, die allerdings nur für Mac und Linux verfügbar ist und unter Windows nicht unterstützt wird. Stattdessen werden hier die Schritte zum Installieren von nvm-windows durchlaufen, um anschließend Node.js und Node Package Manager (npm) zu installieren. Es gibt [alternative Versions-Manager](#alternative-version-managers), die im nächsten Abschnitt behandelt werden.

> [!IMPORTANT]
> Es wird immer empfohlen, vorhandene Installationen von Node.js oder npm vom Betriebssystem zu entfernen, bevor du einen Versions-Manager installierst, da die unterschiedlichen Installationstypen zu ungewöhnlichen und verwirrenden Konflikten führen können. Dies umfasst auch das Löschen der vorhandenen nodejs-Installationsverzeichnisse (z. B. „C:\Programme\nodejs“), die möglicherweise verbleiben. Der von nvm generierte Symlink überschreibt vorhandene (auch leere) Installationsverzeichnisse nicht. Hilfe beim Entfernen früherer Installationen findest du unter [Vollständiges Entfernen von Node.js aus Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows).)

1. Öffne das [windows-nvm-Repository](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) in deinem Internetbrowser, und wähle den Link **Jetzt herunterladen** aus.
2. Lade die Datei **nvm-setup.zip** für das neueste Release herunter.
3. Öffne nach dem Herunterladen die ZIP-Datei und dann die Datei **nvm-setup.exe**.
4. Der nvm-Installations-Assistent für Windows führt dich durch die Einrichtungsschritte, einschließlich der Auswahl des Verzeichnisses, in dem sowohl nvm-windows als auch Node.js installiert werden.

    ![nvm für Windows – Installations-Assistent](../images/install-nvm-for-windows-wizard.png)

5. Führe nach Abschluss der Installation folgende Schritte aus. Öffne PowerShell, und verwende „windows-nvm“, um aufzulisten, welche Versionen von Node derzeit installiert sind (zu diesem Zeitpunkt keine): `nvm ls`

    ![nvm-Liste ohne Node-Versionen](../images/windows-nvm-powershell-no-node.png)

6. Installiere das aktuelle Release von Node.js (zum Testen der neuesten Featureverbesserungen, das aber eher Fehler enthält als die LTS-Version): `nvm install latest`

7. Installiere das neueste stabile LTS-Release von Node.js (empfohlen), indem du zuerst nach der aktuellen LTS-Versionsnummer suchst: `nvm list available`. Installiere dann diese LTS-Versionsnummer mit: `nvm install <version>` (`<version>` durch die Versionsnummer ersetzen, also `nvm install 12.14.0`).

    ![nvm-Liste der verfügbaren Versionen](../images/windows-nvm-list.png)

8. Liste die installierten Versionen von Node auf: `nvm ls`. Nun sollten die beiden Versionen, die du soeben installiert hast, aufgeführt werden.

    ![nvm-Liste mit installierten Node-Versionen](../images/windows-nvm-node-installs.png)

9. Wähle nach der Installation der benötigten Node.js-Versionsnummern die gewünschte Version aus, indem du Folgendes eingibst: `nvm use <version>` (wobei `<version>` durch die Nummer ersetzt wird, also `nvm use 12.9.0`).

10. Um die Version von Node.js zu ändern, die du für ein Projekt verwenden möchtest, erstellst du ein neues Projektverzeichnis `mkdir NodeTest` und wechselst in das Verzeichnis (`cd NodeTest`). Gib dann `nvm use <version>` ein, und ersetze `<version>` durch die gewünschte Versionsnummer (z. B. „v10.16.3“).

11. Mit `npm --version` überprüfst du, welche Version von npm installiert ist. Diese Versionsnummer wird automatisch in die npm-Version geändert, die deiner aktuellen Version von Node.js zugeordnet ist.

## <a name="alternative-version-managers"></a>Alternative Versions-Manager

Obwohl windows-nvm derzeit der beliebteste Versions-Manager für Node ist, gibt es auch Alternativen:

- [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) ist eine plattformübergreifende Alternative zu `nvm` mit der Möglichkeit der [Integration mit VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

- [Volta](https://github.com/volta-cli/volta#installing-volta) ist ein neuer Versions-Manager des LinkedIn-Teams, der mit höherer Geschwindigkeit und plattformübergreifender Unterstützung wirbt.

Wechsele zum Installieren von Volta als Versions-Manager (anstelle von windows-nvm) zum Abschnitt **Windows-Installation** im [Leitfaden für die ersten Schritte](https://docs.volta.sh/guide/getting-started). Lade anschließend das Windows-Installationsprogramm herunter, führe es aus, und befolge die Setupanweisungen.

> [!IMPORTANT]
> Vor der Installation von Volta musst du sicherstellen, dass auf dem Windows-Computer der [Entwicklermodus](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) aktiviert ist.

Weitere Informationen zur Verwendung von Volta zum Installieren mehrerer Versionen von Node.js unter Windows findest du in der [Volta-Dokumentation](https://docs.volta.sh/guide/understanding#managing-your-toolchain).

## <a name="install-your-favorite-code-editor"></a>Installieren des bevorzugten Code-Editors

Es wird empfohlen, [VS Code](https://code.visualstudio.com) sowie das [Node.js-Erweiterungspaket](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack) für die Entwicklung mit Node.js unter Windows zu installieren. Installiere entweder alles, oder wähle aus, welche Komponenten für dich am nützlichsten sind.

So installierst du das Node.js-Erweiterungspaket

1. Öffne in VS Code das Fenster **Erweiterungen** (STRG+UMSCHALT+X).
2. Gib im Suchfeld am oberen Rand des Fensters „Erweiterungen“ Folgendes ein: „Node Extension Pack“ (oder den Name der gewünschten Erweiterung).
3. Wähle **Installieren** aus. Nach der Installation wird die Erweiterung im Fenster **Erweiterungen** im Ordner „Aktiviert“ angezeigt. Du kannst Einstellungen deaktivieren, deinstallieren oder konfigurieren, indem du das Zahnradsymbol neben der Beschreibung der neuen Erweiterung auswählst.

Folgende zusätzliche Erweiterungen solltest du ebenfalls in Erwägung ziehen:

- [Debugger für Chrome:](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code) Nachdem du die serverseitige Entwicklung mit Node.js abgeschlossen hast, musst du die Clientseite entwickeln und testen. Diese Erweiterung integriert deinen VS Code-Editor mit dem Debugdienst deines Chrome-Browsers, sodass du effizienter arbeiten kannst.
- [Tastaturlayouts anderer Editoren:](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) Durch diese Erweiterungen wird die Arbeit in deiner Umgebung vereinfacht, wenn du von einem anderen Text-Editor umsteigst (z. B. Atom, Sublime, Vim, emacs, Notepad++ usw.).
- [Einstellungssynchronisierung:](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) Damit kannst du die VS Code-Einstellungen in verschiedenen Installationen über GitHub synchronisieren. Wenn Sie auf verschiedenen Computern arbeiten, können Sie die Umgebung auf diese Weise konsistent halten.

## <a name="install-git-optional"></a>Installieren von Git (optional)

Wenn du beabsichtigst, zusammen mit anderen zusammenzuarbeiten oder das Projekt an einem Open-Source-Standort (wie GitHub) zu hosten, unterstützt VS Code die [Versionskontrolle mit Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). Auf der Registerkarte „Quellcodeverwaltung“ in VS Code werden alle Änderungen nachverfolgt und gängige Git-Befehle („Add“, „Commit“, „Push“, „Pull“) direkt in die Benutzeroberfläche integriert. Sie müssen zunächst Git installieren, um den Bereich „Quellcodeverwaltung“ zu aktivieren.

1. Laden Sie Git für Windows von der [git-scm-Website](https://git-scm.com/download/win) herunter, und installieren Sie Git.

2. Ein Installations-Assistent ist enthalten, der Ihnen eine Reihe von Fragen zu den Einstellungen für die Git-Installation stellt. Sie sollten alle Standardeinstellungen verwenden, sofern kein bestimmter Grund vorliegt, etwas zu ändern.

3. Wenn Sie noch nie zuvor mit Git gearbeitet haben, können Sie [GitHub-Leitfäden](https://guides.github.com/) für den Einstieg nutzen.

4. Es wird empfohlen, deinen Node-Projekten eine Datei [.gitignore](https://help.github.com/en/articles/ignoring-files) hinzuzufügen. Dies ist die [GITIGNORE-Standardvorlage für Node.js von GitHub](https://github.com/github/gitignore/blob/master/Node.gitignore).

## <a name="use-windows-subsystem-for-linux-for-production"></a>Verwenden des Windows-Subsystems für Linux für die Produktion

Die direkte Verwendung von Node.js unter Windows eignet sich hervorragend zum Erlernen und Experimentieren mit den Möglichkeiten. Wenn du bereit bist, produktionsfertige Web-Apps zu erstellen, die in der Regel auf einem Linux-basierten Server bereitgestellt werden, solltest du das Windows-Subsystem für Linux Version 2 (WSL 2) für die Entwicklung von Node.js-Web-Apps verwenden. Viele Node.js-Pakete und -Frameworks werden für eine *nix-Umgebung erstellt, und die meisten Node.js-Apps werden unter Linux bereitgestellt. Die Entwicklung im WSL stellt daher die Konsistenz zwischen deiner Entwicklungsumgebung und den Produktionsumgebungen sicher. Informationen zum Einrichten einer WSL-Entwicklungsumgebung findest du unter [Einrichten der Node.js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md).

> [!NOTE]
> Wenn du sich in der (sehr seltenen) Situation befindest, eine Node.js-App auf einem Windows-Server zu hosten, empfiehlt sich die [Verwendung eines Reverseproxys](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca). Es gibt dafür zwei Möglichkeiten: 1) [iisnode](https://harveywilliams.net/blog/installing-iisnode) oder [direkt](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). Wir verwalten diese Ressourcen nicht und empfehlen [die Verwendung von Linux-Servern zum Hosten deiner Node.js-Apps](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs).
