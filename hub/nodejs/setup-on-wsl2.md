---
title: Einrichten von Node.js in WSL 2
description: Diese Anleitung soll dir helfen, deine Node.js-Entwicklungsumgebung im Windows-Subsystem für Linux (WSL) einzurichten.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, Erlernen von Node.js, Node unter Windows, Node im WSL, Node unter Linux unter Windows, Installieren von Node unter Windows, NodeJS mit VS Code, Entwickeln mit Node unter Windows, Entwickeln mit NodeJS unter Windows, Installieren von Node in WSL, NodeJS im Windows-Subsystem für Linux
ms.localizationpriority: medium
ms.date: 07/28/2020
ms.openlocfilehash: f8474b0617df7e332dc3bc7b35d50bf8f7ad1583
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254337"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Einrichten einer Node.js-Entwicklungsumgebung mit WSL 2

Die folgende Schrittanleitung soll dir helfen, deine Node.js-Entwicklungsumgebung mit dem Windows-Subsystem für Linux (WSL) einzurichten.

Es empfiehlt sich, das aktualisierte WSL 2 zu installieren und auszuführen, um von erheblichen Verbesserungen bei Leistungsgeschwindigkeit und Systemaufrufkompatibilität zu profitieren, einschließlich der Möglichkeit, [Docker Desktop](https://docs.docker.com/docker-for-windows/wsl-tech-preview/#download) auszuführen. Viele der npm-Module und Tutorials für die Node.js-Webentwicklung wurden für Linux-Benutzer geschrieben und verwenden Linux-basierte Paketerstellungs- und Installationstools. Die meisten Web-Apps werden auch unter Linux bereitgestellt, sodass mit WSL 2 die Konsistenz zwischen Entwicklungs- und Produktionsumgebung sichergestellt wird.

> [!NOTE]
> Wenn du beabsichtigst, Node.js direkt unter Windows zu verwenden oder eine Windows Server-Produktionsumgebung zu verwenden, solltest du dir die Anleitung [Einrichten einer Node.js-Entwicklungsumgebung direkt unter Windows](./setup-on-windows.md) ansehen.

## <a name="install-wsl-2"></a>Installieren von WSL 2

Zum Aktivieren und Installieren von WSL 2 folgen Sie den Schritten in der [Dokumentation zur WSL-Installation](/windows/wsl/install-win10). Diese Schritte umfassen die Auswahl einer Linux-Verteilung (z. B. Ubuntu).

Nachdem Sie WSL 2 und eine Linux-Verteilung installiert haben, öffnen Sie die Linux-Verteilung (sie befindet sich im Windows-Startmenü), und überprüfen Sie die Version und den Codenamen mit dem Befehl `lsb_release -dc`.

Es empfiehlt sich, die Linux-Verteilung regelmäßig zu aktualisieren, z. B. auch unmittelbar nach der Installation, um sicherzustellen, dass Sie über die neuesten Pakete verfügen. Dieses Update wird unter Windows nicht automatisch verarbeitet. Verwenden Sie den Befehl `sudo apt update && sudo apt upgrade`, um die Verteilung zu aktualisieren.  

## <a name="install-windows-terminal-optional"></a>Installieren von Windows-Terminal (optional)

Das neue Windows Terminal aktiviert mehrere Registerkarten (schnelles Umschalten zwischen mehreren Linux-Befehlszeilen, Windows-Eingabeaufforderung, PowerShell, Azure CLI usw.) und ermöglicht, benutzerdefinierte Tastenkombinationen zu erstellen (Tastenkombinationen zum Öffnen oder Schließen von Registerkarten, zum Kopieren und Einfügen usw.) sowie die Suchfunktion und benutzerdefinierte Designs zu verwenden (Farbschemata, Schriftschnitte und Schriftgrade, Hintergrundbild/Weichzeichnen/Transparenz). [Weitere Informationen](/windows/terminal)

1. Hole dir [Windows-Terminal im Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701): Durch die Installation über den Store werden Updates automatisch durchgeführt.

2. Öffne nach der Installation das Windows-Terminal, und wähle **Einstellungen** aus, um dein Terminal mithilfe der Datei `settings.json` anzupassen.

    ![Windows-Terminal-Einstellungen](../images/windows-terminal-settings.png)

## <a name="install-nvm-nodejs-and-npm"></a>Installieren von nvm, Node.js und npm

Node.js kann auf unterschiedliche Weise installiert werden. Es wird empfohlen, einen Versions-Manager zu verwenden, da sich die Versionen sehr schnell ändern. Du musst wahrscheinlich je nach den Anforderungen einzelner Projekte, an denen du arbeitest, zwischen verschiedenen Versionen wechseln. Node Version Manager (meist als nvm bezeichnet) ist die beliebteste Methode zum Installieren mehrerer Versionen von Node.js. Hier werden die Schritte zum Installieren von nvm durchlaufen, um anschließend Node.js und Node Package Manager (npm) zu installieren. Es gibt [alternative Versions-Manager](#alternative-version-managers), die im nächsten Abschnitt behandelt werden.

> [!IMPORTANT]
> Es wird immer empfohlen, vorhandene Installationen von Node.js oder npm vom Betriebssystem zu entfernen, bevor du einen Versions-Manager installierst, da die unterschiedlichen Installationstypen zu ungewöhnlichen und verwirrenden Konflikten führen können. Beispielsweise ist die Version von Node, die mit dem Befehl `apt-get` von Ubuntu installiert werden kann, zurzeit veraltet. Hilfe beim Entfernen früherer Installationen findest du unter [Entfernen von Node.js aus Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).)

1. Öffne die Befehlszeile von Ubuntu 18.04.
2. Installiere cURL (ein Tool zum Herunterladen von Inhalten aus dem Internet an der Befehlszeile) mit `sudo apt-get install curl`.
3. Installiere nvm mit `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`.

> [!NOTE]
> Zum Zeitpunkt der Veröffentlichung war NVM v0.35.3 die neueste verfügbare Version. Du findest das [neueste nvm-Release auf der GitHub-Projektseite](https://github.com/nvm-sh/nvm). Passe den obigen Befehl so an, dass er die neueste Version enthält.
Durch die Installation der neueren Version von nvm mithilfe von cURL wird die ältere Version ersetzt. Die Node-Version, die du für die Installation von nvm verwendet hast, bleibt intakt. Beispiel: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

4. Gib zum Überprüfen der Installation Folgendes ein: `command -v nvm`. Dies sollte „nvm“ zurückgeben. Wenn du als Antwort „Befehl nicht gefunden“ oder gar keine Antwort erhältst, schließe das aktuelle Terminal, öffne es erneut, und versuche es noch einmal. [Weitere Informationen findest du im GitHub-Repository zu nvm.](https://github.com/nvm-sh/nvm)
5. Liste mit `nvm ls` die Versionen von Node auf, die derzeit installiert sind (sollte zu diesem Zeitpunkt keine sein).

    ![nvm-Liste ohne Node-Versionen](../images/nvm-no-node.png)

6. Installiere das aktuelle Release von Node.js (zum Testen der neuesten Featureverbesserungen, das aber wahrscheinlich einige Fehler enthält): `nvm install node`
7. Installiere das neueste stabile LTS-Release von Node.js (empfohlen): `nvm install --lts`
8. Liste die installierten Versionen von Node auf: `nvm ls`. Nun sollten die beiden Versionen, die du soeben installiert hast, aufgeführt werden.

    ![nvm-Liste mit LTS und aktuellen Node-Versionen](../images/nvm-node-installed.png)

9. Überprüfe mit `node --version`, ob Node.js installiert ist und die neueste Version aufweist. Überprüfe dann, ob du auch über npm verfügst: `npm --version` (Du kannst auch `which node` oder `which npm` verwenden, um den Pfad anzuzeigen, der für die Standardversionen verwendet wird.)
10. Um die Version von Node.js zu ändern, die du für ein Projekt verwenden möchtest, erstellst du ein neues Projektverzeichnis `mkdir NodeTest` und wechselst in das Verzeichnis (`cd NodeTest`). Gib dann `nvm use node` ein, und wechsele zur aktuellen Version, oder verwende `nvm use --lts`, um zur LTS-Version zu wechseln. Du kannst auch eine spezifische andere Version verwenden, die du installiert hast, z. B. `nvm use v8.2.1`. (Um alle verfügbaren Versionen von Node.js aufzulisten, verwendest du den Befehl `nvm ls-remote`.)

Wenn du nvm verwendest, um Node.js und npm zu installieren, musst du nicht den Befehl „sudo“ verwenden, um neue Pakete zu installieren.

## <a name="alternative-version-managers"></a>Alternative Versions-Manager

Obwohl nvm derzeit der beliebteste Versions-Manager für Node ist, gibt es auch Alternativen:

- [n](https://www.npmjs.com/package/n#installation) ist eine langfristige Alternative zu `nvm`, die das gleiche mit leicht abweichenden Befehlen erreicht und über `npm` anstelle eines Bash-Skripts installiert wird.
- [fnm](https://github.com/Schniz/fnm#using-a-script) ist ein neuerer Versions-Manager, der schneller als `nvm` sein soll. (Außerdem verwendet er [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops).)
- [Volta](https://github.com/volta-cli/volta#installing-volta) ist ein neuer Versions-Manager des LinkedIn-Teams, der mit höherer Geschwindigkeit und plattformübergreifender Unterstützung wirbt.
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm) ist eine einzelne Befehlszeilenschnittstelle für mehrere Sprachen, z. B. ike gvm, nvm, rbenv und pyenv (usw.).
- [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) ist eine plattformübergreifende Alternative zu `nvm` mit der Möglichkeit der [Integration mit VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Installieren des bevorzugten Code-Editors

Es wird empfohlen, Visual Studio Code mit der Remote-WSL-Erweiterung für Node.js-Projekte zu verwenden. Dadurch wird VS Code in eine Client-Server-Architektur aufgeteilt, wobei der Client (die VS Code-Benutzeroberfläche) auf einem Windows-Betriebssystem und der Server (der Code, Git, Plug-Ins usw.) remote auf Ihrer WSL Linux-Verteilung ausgeführt wird. 

> [!NOTE]
> Dieses „Remoteszenario“ unterscheidet sich geringfügig von dem, was Sie vielleicht gewohnt sind. Die WSL unterstützt eine echte Linux-Verteilung, bei der Ihr Projektcode getrennt von Ihrem Windows-Betriebssystem, aber immer noch auf Ihrem lokalen Computer ausgeführt wird. Die Remote-WSL-Erweiterung stellt eine Verbindung mit Ihrem Linux-Subsystem her, als handle es sich um einen Remoteserver, auch wenn sie nicht in der Cloud ausgeführt wird… Sie wird weiterhin auf dem lokalen Computer in der WSL-Umgebung ausgeführt, die Sie für die Ausführung parallel zu Windows aktiviert haben. 

- Linux-basierte Funktionen wie IntelliSense und Linting werden unterstützt.
- Dein Projekt wird automatisch unter Linux erstellt.
- Du kannst alle Erweiterungen verwenden, die unter Linux ausgeführt werden ([ES Lint, npm IntelliSense, ES6-Codeausschnitte usw](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack).).

Terminalbasierte Text-Editoren (Vim, emacs, nano) sind ebenfalls hilfreich, um schnell Änderungen direkt an der Konsole vorzunehmen. (In [diesem Artikel](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) werden die Unterschiede und die Verwendung der einzelnen Editoren recht gut beschrieben.)

> [!NOTE]
> Bei einigen Editoren mit grafischer Benutzeroberfläche (Atom, Sublime Text, Eclipse) können Probleme beim Zugriff auf den freigegebenen WSL-Netzwerkort auftreten (\\wsl$\ubuntu\home\). Sie könnten versuchen, deine Linux-Dateien gegen deinen Wunsch mithilfe von Windows-Tools zu erstellen. Diese Kompatibilität wird von der Remote-WSL-Erweiterung in VS Code sichergestellt.

So installierst du VS Code und die Remote-WSL-Erweiterung

1. [Laden Sie VS Code für Windows herunter, und installieren Sie es.](https://code.visualstudio.com) VS Code ist auch für Linux verfügbar. Da das Windows-Subsystem für Linux jedoch keine GUI-Apps unterstützt, muss VS Code unter Windows installiert werden. Die Integration in die Befehlszeile und die Tools von Linux ist jedoch durch Verwendung der Erweiterung „Remote – WSL“ problemlos möglich.

2. Installieren Sie die [Erweiterung „Remote – WSL“](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) in VS Code. Dies ermöglicht Ihnen die Verwendung von WSL als integrierte Entwicklungsumgebung, während die Verarbeitung der Kompatibilität und der Pfadzuordnung automatisch erfolgt. [Weitere Informationen](https://code.visualstudio.com/docs/remote/remote-overview)

> [!IMPORTANT]
> Wenn Sie VS Code bereits installiert haben, müssen Sie sicherstellen, dass Sie über das [Release vom Mai (1.35)](https://code.visualstudio.com/updates/v1_35) oder höher verfügen, um die [Erweiterung „Remote – WSL“](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) installieren zu können. Es wird davon abgeraten, WSL in VS Code ohne die Erweiterung „Remote – WSL“ zu verwenden, da dann keine Unterstützung für automatisches Vervollständigen, Debuggen, Linting usw. vorhanden ist. Hinweis: Diese WSL-Erweiterung wird unter „$HOME/.vscode-server/extensions“ installiert.

### <a name="helpful-vs-code-extensions"></a>Hilfreiche Erweiterung für VS Code

Obwohl VS Code viele bereits integrierte Features für die Entwicklung mit Node.js bietet, gibt es im [Node.js-Erweiterungspaket](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack) einige hilfreiche Erweiterungen, die du installieren kannst. Installiere entweder alles, oder wähle aus, welche Komponenten für dich am nützlichsten sind.

So installierst du das Node.js-Erweiterungspaket

1. Öffne in VS Code das Fenster **Erweiterungen** (STRG+UMSCHALT+X).

    Das Fenster „Erweiterungen“ ist nun in drei Abschnitte unterteilt (da du die Remote-WSL-Erweiterung installiert hast).
    - „Lokal – installiert“: Die für die Verwendung mit dem Windows-Betriebssystem installierten Erweiterungen.
    - "WSL:Ubuntu-18.04-Installed": Die für die Verwendung mit dem Ubuntu-Betriebssystem (WSL) installierten Erweiterungen.
    - „Empfohlen“: Erweiterungen, die von VS Code basierend auf den Dateitypen in deinem aktuellen Projekt empfohlen werden.

    ![Lokale und Remoteerweiterungen für VS Code](../images/vscode-extensions-local-remote.png)

2. Gib im Suchfeld am oberen Rand des Fensters „Erweiterungen“ Folgendes ein: **Node Extension Pack** (oder den Namen der gewünschten Erweiterung). Die Erweiterung wird für deine lokalen oder WSL-Instanzen von VS Code installiert, je nachdem, wo du das aktuelle Projekt geöffnet hast. Du kannst das erkennen, indem du in der linken unteren Ecke des VS Code-Fensters den Remotelink auswählst (grün). Du kannst dann entweder eine Remoteverbindung öffnen oder eine schließen. Installiere die Node.js-Erweiterungen in der Umgebung „WSL:Ubuntu-18.04“.

    ![Remotelink in VS Code](../images/wsl-remote-extension.png)

Folgende zusätzliche Erweiterungen solltest du ebenfalls in Erwägung ziehen:

- [Debugger für Chrome:](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code) Nachdem du die serverseitige Entwicklung mit Node.js abgeschlossen hast, musst du die Clientseite entwickeln und testen. Diese Erweiterung integriert deinen VS Code-Editor mit dem Debugdienst deines Chrome-Browsers, sodass du effizienter arbeiten kannst.
- [Tastaturlayouts anderer Editoren:](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) Durch diese Erweiterungen wird die Arbeit in deiner Umgebung vereinfacht, wenn du von einem anderen Text-Editor umsteigst (z. B. Atom, Sublime, Vim, emacs, Notepad++ usw.).
- [Einstellungssynchronisierung:](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) Damit kannst du die VS Code-Einstellungen in verschiedenen Installationen über GitHub synchronisieren. Wenn Sie auf verschiedenen Computern arbeiten, können Sie die Umgebung auf diese Weise konsistent halten.

## <a name="set-up-git-optional"></a>Einrichten von Git (optional)

Informationen zum Einrichten von Git für ein NodeJS-Projekt in WSL finden Sie im Artikel [Erste Schritte mit Git im Windows-Subsystem für Linux](/windows/wsl/tutorials/wsl-git) in der WSL-Dokumentation.

## <a name="next-steps"></a>Nächste Schritte

Du hast damit eine Node.js-Entwicklungsumgebung eingerichtet. Um mit der Verwendung der Node.js-Umgebung zu beginnen, solltest du eines der folgenden Tutorials ausprobieren:

- [Erste Schritte mit Node.js für Anfänger](./beginners.md)
- [Erste Schritte mit Node.js-Webframeworks unter Windows](./web-frameworks.md)
- [Erste Schritte beim Verbinden von Node.js-Apps mit einer Datenbank](/windows/wsl/tutorials/wsl-database)
- [Erste Schritte bei der Verwendung von Docker-Containern mit Node.js](./containers.md)
