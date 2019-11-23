---
title: Einrichten von nodejs auf WSL 2
description: Eine Anleitung, die Sie beim Einrichten der Node. js-Entwicklungsumgebung unter Windows-Subsystem für Linux (WSL) unterstützt.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Microsoft, Learning NodeJS, Node on Windows, Node on WSL, Node on Linux on Windows, install Node on Windows, NodeJS with vs Code, Develop with Node on Windows, Develop with NodeJS on Windows, install Node on WSL, NodeJS on Windows Subsystem für Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: e5875f0bf7ce73d3615aa131d57c2384c73dd8a1
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517838"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Einrichten der Node. js-Entwicklungsumgebung mit WSL 2

Im folgenden finden Sie eine Schritt-für-Schritt-Anleitung, die Sie beim Einrichten der Node. js-Entwicklungsumgebung mithilfe des Windows-Subsystems für Linux (WSL) unterstützt. Für diese Anleitung ist es erforderlich, dass Sie einen Windows Insider Preview-Build installieren und ausführen, um [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)zu installieren und zu verwenden. WSL 2 hat eine beträchtliche Geschwindigkeit und Leistungsverbesserung gegenüber WSL 1, insbesondere in Bezug auf Node. js. Viele NPM-Module und Tutorials für die Node. js-Webentwicklung sind für Linux-Benutzer geschrieben und verwenden Linux-basierte Paket Erstellung und Installations Tools. Die meisten Web-Apps werden auch unter Linux bereitgestellt, sodass die Verwendung von WSL 2 die Konsistenz zwischen Ihrer Entwicklungs-und Produktionsumgebung gewährleistet.

> [!NOTE]
> Wenn Sie beabsichtigen, Node. js direkt unter Windows zu verwenden, oder eine Windows Server-Produktionsumgebung verwenden möchten, finden Sie in unserem Handbuch Informationen zum [direkten Einrichten der Node. js-Entwicklungsumgebung unter Windows](./setup-on-windows.md).

## <a name="install-windows-10-insider-preview-build"></a>Installieren des Windows 10 Insider Preview-Builds

1. **[Installieren Sie die neueste Version von Windows 10](https://www.microsoft.com/software-download/windows10)** : Wählen Sie **Jetzt aktualisieren** aus, um den Update-Assistenten herunterzuladen. Öffnen Sie nach dem Herunterladen den Update-Assistenten, um festzustellen, ob Sie zurzeit die aktuelle Version von Windows ausführen, und wählen Sie dann im Assistenten Fenster **Jetzt aktualisieren** aus, um den Computer zu aktualisieren. *(Dieser Schritt ist optional, wenn Sie eine relativ aktuelle Version von Windows 10 ausführen.)*

    ![Windows Update-Assistent](../images/windows-update-assistant2019.png)

2. Wechseln Sie **[zu Start > Einstellungen > Windows-Insider-Programm](ms-settings:windowsinsider)** : **Klicken Sie im** Windows Insider-Programmfenster auf erste Schritte, und verknüpfen Sie dann **ein Konto**.

    ![Windows-Insider-Programmeinstellungen](../images/windows-insider-program-settings.png)

3. **[Als Windows-Insider registrieren](https://insider.windows.com/getting-started/#register)** : Wenn Sie nicht beim Insider-Programm registriert sind, müssen Sie dies mit Ihrem [Microsoft-Konto](https://account.microsoft.com/account)tun.

    ![Windows-Insider-Registrierung](../images/windows-insider-account.png)

4. Wählen Sie den Empfang **schneller Ring** Aktualisierungen aus, oder fahren Sie **mit dem nächsten Windows-releaseinhalt fort** . Vergewissern Sie sich, dass Sie **später neu starten**. Vor dem Neustart müssen einige zusätzliche Einstellungen geändert werden.

    ![Windows Insider fast Ring](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Aktivieren des Windows-Subsystems für Linux und Plattform für virtuelle Computer

1. Suchen Sie in den **Windows-Einstellungen**nach **Windows-Features aktivieren oder deaktivieren**.
2. Nachdem die Liste der **Windows-Features** angezeigt wird, Scrollen Sie zu **virtuelle Computerplattform** **Suchen und Windows-Subsystem für Linux**, und stellen Sie sicher, dass das Kontrollkästchen aktiviert ist, um beides zu **aktivieren.**
3. Starten Sie den Computer neu, wenn Sie dazu aufgefordert werden.

    ![Aktivieren von Windows-Features](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Installieren einer Linux-Distribution

Es stehen mehrere Linux-Distributionen zur Verfügung, die auf WSL ausgeführt werden können. Sie können Ihre Favoriten in der Microsoft Store finden und installieren. Wir empfehlen, mit [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) zu beginnen, da es aktuell, beliebt und gut unterstützt wird.

1. Öffnen Sie diesen [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) -Link, öffnen Sie den Microsoft Store, und wählen Sie **Get**aus. *(Dieser Download ist recht umfangreich und kann einige Zeit in Anspruch nehmen.)*

2. Nachdem der Download abgeschlossen ist, wählen Sie **starten** von der Microsoft Store oder starten aus, indem Sie im **Startmenü** "Ubuntu 18,04 LTS" eingeben.

3. Wenn Sie die Verteilung zum ersten Mal ausführen, werden Sie aufgefordert, einen Kontonamen und ein Kennwort zu erstellen. Anschließend werden Sie standardmäßig automatisch als dieser Benutzer angemeldet. Sie können einen beliebigen Benutzernamen und ein Kennwort auswählen. Sie haben keinen Einfluss auf den Windows-Benutzernamen.

    ![Linux-Distributionen im Microsoft Store](../images/store-linux-distros.png)

Sie können die Linux-Distribution, die Sie zurzeit verwenden, überprüfen, indem Sie Folgendes eingeben: `lsb_release -dc`. Verwenden Sie zum Aktualisieren der Ubuntu-Distribution: `sudo apt update && sudo apt upgrade`. Wir empfehlen, regelmäßig zu aktualisieren, um sicherzustellen, dass Sie über die neuesten Pakete verfügen Dieses Update wird von Windows nicht automatisch verarbeitet. Links zu anderen verfügbaren Linux-Distributionen, die im Microsoft Store verfügbar sind, finden Sie unter [Windows-Subsystem für Linux-Installationshandbuch für Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="install-wsl-2"></a>Installieren von WSL 2

WSL 2 ist eine [neue Version der Architektur](https://docs.microsoft.com/windows/wsl/wsl2-about) in WSL, die ändert, wie Linux-Distributionen mit Windows interagieren, die Leistung verbessert und eine vollständige System aufrufskompatibilität hinzugefügt wird.

1. Geben Sie in PowerShell den Befehl: `wsl -l` ein, um die Liste der auf Ihrem Computer installierten WSL-Distributionen anzuzeigen. Nun sollte Ubuntu-18,04 in dieser Liste angezeigt werden.
2. Geben Sie nun den folgenden Befehl ein: `wsl --set-version Ubuntu-18.04 2`, um die Ubuntu-Installation für die Verwendung von WSL 2 festzulegen.
3. Überprüfen Sie die WSL-Version, die von den einzelnen installierten Distributionen verwendet wird: `wsl --list --verbose` (oder `wsl -l -v`).

    ![Windows-Subsystem für Linux-Set-Version](../images/wsl-versions.png)

> [!TIP]
> Sie können eine beliebige Linux-Distribution, die Sie auf WSL 2 installiert haben, gemäß denselben Anweisungen (mithilfe von PowerShell) festlegen. ändern Sie einfach "Ubuntu-18,04" in den Namen der installierten Distribution, die Sie als Ziel verwenden möchten. Um zurück zu WSL 1 zu wechseln, führen Sie den gleichen Befehl wie oben aus, ersetzen Sie jedoch "2" durch "1".  Sie können WSL 2 auch als Standard für alle neu installierten Distributionen festlegen, indem Sie Folgendes eingeben: `wsl --set-default-version 2`.

## <a name="install-nvm-nodejs-and-npm"></a>Installieren von NVM, Node. js und NPM

Es gibt mehrere Möglichkeiten, Node. js zu installieren. Es wird empfohlen, einen Versions-Manager zu verwenden, wenn sich Versionen sehr schnell ändern. Sie müssen wahrscheinlich zwischen mehreren Versionen wechseln, je nach den Anforderungen verschiedener Projekte, an denen Sie arbeiten. Knoten Versions-Manager, üblicherweise NVM genannt, ist die beliebteste Methode zum Installieren mehrerer Versionen von Node. js. Wir führen Sie durch die Schritte zum Installieren von NVM und verwenden Sie dann zum Installieren von Node. js und Node Package Manager (NPM). Es gibt [Alternative Versions Manager](#alternative-version-managers) , die im nächsten Abschnitt behandelt werden müssen.

> [!IMPORTANT]
> Es wird immer empfohlen, vorhandene Installationen von Node. js oder NPM von Ihrem Betriebssystem zu entfernen, bevor Sie einen Versions-Manager installieren, da die verschiedenen Installationstypen zu seltsamen und verwirrenden Konflikten führen können. Beispielsweise ist die Version des Knotens, die mit dem `apt-get` Befehl von Ubuntu installiert werden kann, zurzeit veraltet. Hilfe zum Entfernen vorheriger Installationen finden Sie unter Vorgehens [Weise beim Entfernen von nodejs aus Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).)

1. Öffnen Sie die Befehlszeile von Ubuntu 18,04.
2. Installieren Sie curl (ein Tool zum Herunterladen von Inhalten aus dem Internet in der Befehlszeile) mit: `sudo apt-get install curl`
3. Installieren Sie NVM mit: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
4. Geben Sie zum Überprüfen der Installation Folgendes ein: `command -v nvm`... Dies sollte "NVM" zurückgeben, wenn Sie den Befehl "Befehl wurde nicht gefunden" oder gar keine Antwort erhalten, schließen Sie das aktuelle Terminal, öffnen Sie es erneut, und versuchen Sie es noch mal. [Weitere Informationen finden Sie im GitHub-Repository NVM](https://github.com/nvm-sh/nvm).
5. Listet die Versionen von Knoten auf, die derzeit installiert sind (sollte an diesem Punkt "None" lauten): `nvm ls`

    ![NVM-Liste, die keine Knoten Versionen anzeigt](../images/nvm-no-node.png)

6. Installieren Sie die aktuelle Version von Node. js (zum Testen der neuesten Verbesserungen der Features, aber mit höherer Wahrscheinlichkeit Probleme): `nvm install node`
7. Installieren Sie die neueste stabile LTS-Version von Node. js (empfohlen): `nvm install --lts`
8. Listet die installierten Versionen von Knoten auf: `nvm ls`... Nun sollten Sie sehen, dass die beiden Versionen, die Sie soeben installiert haben, aufgeführt sind.

    ![NVM-Liste mit LTS und aktuellen Knoten Versionen](../images/nvm-node-installed.png)

9. Überprüfen Sie, ob Node. js installiert ist und die aktuelle Standardversion mit: `node --version`. Vergewissern Sie sich dann, dass Sie auch über NPM verfügen, mit: `npm --version` (Sie können auch `which node` oder `which npm` verwenden, um den für die Standardversionen verwendeten Pfad anzuzeigen).
10. Um die Version von Node. js zu ändern, die Sie für ein Projekt verwenden möchten, erstellen Sie ein neues Projektverzeichnis `mkdir NodeTest`und geben Sie das Verzeichnis `cd NodeTest`ein. Geben Sie dann `nvm use node` ein, um zur aktuellen Version zu wechseln, oder `nvm use --lts`, um zur LTS-Version zu wechseln. Sie können auch die spezifische Anzahl für alle zusätzlichen Versionen verwenden, die Sie installiert haben, wie z. b. `nvm use v8.2.1`. (Um alle verfügbaren Versionen von Node. js aufzulisten, verwenden Sie den Befehl: `nvm ls-remote`).

> [!TIP]
> Wenn Sie "NVM" verwenden, um Node. js und NPM zu installieren, müssen Sie nicht den Befehl "sudo" verwenden, um neue Pakete zu installieren.

> [!NOTE]
> Zum Zeitpunkt der Veröffentlichung war NVM v 0.34.0 die aktuellste verfügbare Version. Sie können die [GitHub-Projektseite für die neueste Version von NVM](https://github.com/nvm-sh/nvm)überprüfen und den obigen Befehl so anpassen, dass Sie die neueste Version enthält.
Durch die Installation der neueren Version von NVM mithilfe von cURL wird die ältere Version ersetzt, sodass die Version des Knotens, den Sie für die Installation von NVM verwendet haben, intakt ist. Beispiel: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash`

## <a name="alternative-version-managers"></a>Alternative Versions-Manager

Obwohl NVM zurzeit der beliebteste Versions-Manager für Node ist, sind einige Alternativen zu beachten:

- [n](https://www.npmjs.com/package/n#installation) ist eine langfristige `nvm` Alternative, die das gleiche mit leicht unterschiedlichen Befehlen erreicht und über `npm` anstelle eines Bash-Skripts installiert wird.
- [FNM](https://github.com/Schniz/fnm#using-a-script) ist ein neuerer Versions Manager, der eine viel schnellere Leistung als `nvm`vorgibt. (Außerdem wird [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)verwendet.)
- [Volta](https://github.com/volta-cli/volta#installing-volta) ist ein neuer Versions Manager des LinkedIn-Teams, der höhere Geschwindigkeit und plattformübergreifende Unterstützung beansprucht.
- [asdf-VM](https://asdf-vm.com/#/core-manage-asdf-vm) ist eine einzelne CLI für mehrere Sprachen, wie z. b. IKE GVM, NVM, rbenv & pyenv (usw.).
- [NVS](https://github.com/jasongin/nvs) (Node-Version Switcher) ist eine plattformübergreifende `nvm` Alternative mit der Möglichkeit, in [vs Code zu integrieren](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Installieren Sie Ihren bevorzugten Code-Editor.

Es wird empfohlen, **Visual Studio Code** mit der **Remote-WSL-Erweiterung** für Node. js-Projekte zu verwenden. Dadurch wird vs Code in eine Client-Server-Architektur aufgeteilt, wobei der Client (die Benutzeroberfläche) auf Ihrem Windows-Computer und der Server (Code, git, Plug-ins usw.) Remote ausgeführt wird.

- Die Linux-basierte IntelliSense-und linting-Funktionen werden unterstützt.
- Ihr Projekt wird in Linux automatisch erstellt.
- Sie können alle Erweiterungen verwenden, die unter Linux ausgeführt werden ([es Lint, NPM IntelliSense, ES6 Snippets usw.](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)).

Terminal basierte Text-Editoren (vim, emacs, Nano) sind ebenfalls hilfreich, um schnell Änderungen direkt in der Konsole vorzunehmen. (In[diesem Artikel](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) werden die Unterschiede erläutert und erläutert, wie Sie diese verwenden können.)

> [!NOTE]
> Einige GUI-Editoren (Atom, Sublime Text, Eclipse) können Probleme beim Zugriff auf den freigegebenen WSL-Netzwerkort haben (\\WSL $ \ubuntu\home\) und versuchen, Ihre Linux-Dateien mithilfe von Windows-Tools zu erstellen, was Ihnen möglicherweise nicht entspricht. Diese Kompatibilität wird von der Remote-WSL-Erweiterung in vs Code behandelt.

So installieren Sie vs Code und die Remote-WSL-Erweiterung:

1. [Laden Sie vs Code für Windows herunter, und installieren Sie](https://code.visualstudio.com)es. VS Code ist auch für Linux verfügbar, aber das Windows-Subsystem für Linux unterstützt keine GUI-apps, daher müssen wir es unter Windows installieren. Keine Sorge, Sie können mit der Remote-WSL-Erweiterung weiterhin in Ihre Linux-Befehlszeile und-Tools integrieren.

2. Installieren Sie die [Remote-WSL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) auf vs Code. Dies ermöglicht Ihnen die Verwendung von WSL als integrierte Entwicklungsumgebung und übernimmt die Kompatibilität und das richtige für Sie. [Weitere Informationen](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Wenn Sie bereits vs Code installiert haben, müssen Sie sicherstellen, dass Sie über die [Version 1,35](https://code.visualstudio.com/updates/v1_35) oder höher verfügen, um die [Remote-WSL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)installieren zu können. Es wird nicht empfohlen, WSL in vs Code ohne die Remote-WSL-Erweiterung zu verwenden, da Sie die Unterstützung für automatisches vervollständigen, Debuggen, linting usw. verlieren. Spaß Fakt: Diese WSL-Erweiterung wird in $Home/.vscode-Server/Extensions. installiert.

### <a name="helpful-vs-code-extensions"></a>Hilfreiche vs Code Erweiterungen

Obwohl vs Code viele Features für die Node. js-Entwicklung enthalten, gibt es einige hilfreiche Erweiterungen, die Sie im [node. js-Erweiterungspaket](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)als verfügbar installieren sollten. Installieren Sie alle, oder wählen Sie aus, und wählen Sie aus, was für Sie am nützlichsten erscheint.

So installieren Sie das Node. js-Erweiterungspaket:

1. Öffnen Sie das Fenster " **Erweiterungen** " (STRG + UMSCHALT + X) in vs Code.

    Das Fenster Erweiterungen ist nun in drei Abschnitte unterteilt (weil Sie die Remote-WSL-Erweiterung installiert haben).
    - "Lokal-installiert": die für die Verwendung mit dem Windows-Betriebssystem installierten Erweiterungen.
    - "WSL: Ubuntu-18,04-installiert": die Erweiterungen, die für die Verwendung mit Ihrem Ubuntu-Betriebssystem (WSL) installiert sind.
    - "Empfohlen": Erweiterungen, die von vs Code basierend auf den Dateitypen in Ihrem aktuellen Projekt empfohlen werden.

    ![Lokale und Remote-Erweiterungen für vs Code Erweiterungen](../images/vscode-extensions-local-remote.png)

2. Geben Sie im Suchfeld am oberen Rand des Fensters Erweiterungen Folgendes ein: **Node Extension Pack** (oder den Namen der gewünschten Erweiterung). Die Erweiterung wird entweder für Ihre lokalen oder WSL-Instanzen von vs Code installiert, je nachdem, wo Sie das aktuelle Projekt geöffnet haben. Sie können erkennen, indem Sie in der linken unteren Ecke des vs Code Fensters den Remote Link auswählen (in grün). Sie erhalten die Möglichkeit, eine Remote Verbindung zu öffnen oder zu schließen. Installieren Sie die Node. js-Erweiterungen in der Umgebung "WSL: Ubuntu-18,04".

    ![Remote Verbindung vs Code](../images/wsl-remote-extension.png)

Einige zusätzliche Erweiterungen, die Sie berücksichtigen sollten, sind u. A.:

- [Debugger für Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): Nachdem Sie die Entwicklung auf dem Server mit Node. js abgeschlossen haben, müssen Sie die Clientseite entwickeln und testen. Diese Erweiterung integriert ihren vs Code-Editor in ihren Chrome-Browser-Debugdienst, sodass Dinge etwas effizienter werden.
- [Keymaps von anderen Editoren](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): Diese Erweiterungen können Ihre Umgebung zu Hause machen, wenn Sie von einem anderen Text-Editor wechseln (z. b. Atom, Sublime, vim, emacs, Notepad + + usw.).
- [Einstellungs Synchronisierung](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): ermöglicht die Synchronisierung Ihrer vs Code Einstellungen über verschiedene Installationen hinweg mithilfe von github. Wenn Sie auf unterschiedlichen Computern arbeiten, können Sie Ihre Umgebung auf diese Weise konsistent halten.

## <a name="install-windows-terminal-optional"></a>Installieren des Windows-Terminals (optional)

Das neue Windows-Terminal ermöglicht mehrere Registerkarten (schnelles Umschalten zwischen Eingabeaufforderung, PowerShell oder mehreren Linux-Verteilungen), benutzerdefinierte Tastenbindungen (erstellen Sie eigene Tastenkombinationen zum Öffnen oder Schließen von Registerkarten, kopieren und Einfügen usw.), Emojis-☺ und benutzerdefinierte Designs (Farbschemas, Schriftarten und Größen, Hintergrundbild/weich/Transparenz). [Weitere Informationen](https://devblogs.microsoft.com/commandline/).

1. Holen Sie sich [das Windows-Terminal (Vorschau) im Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701): durch die Installation über den Store werden Updates automatisch verarbeitet.

2. Öffnen Sie nach der Installation das Windows-Terminal, und wählen Sie **Einstellungen** aus, um das Terminal mithilfe der `profile.json` Datei anzupassen. [Erfahren Sie mehr über das Bearbeiten von Windows-Terminal Einstellungen](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md).

    ![Windows-Terminal Einstellungen](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Einrichten von git (optional)

Wenn Sie beabsichtigen, mit anderen zusammenzuarbeiten oder das Projekt auf einem Open-Source-Standort (wie GitHub) zu hosten, unterstützt vs Code die [Versionskontrolle mit git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). Auf der Registerkarte Quell Code Verwaltung in vs Code werden alle Änderungen nachverfolgt, und es werden allgemeine git-Befehle (Add, Commit, Push, Pull) direkt in die Benutzeroberfläche integriert.

1. Git wird mit dem Windows-Subsystem für Linux-Distributionen installiert. Sie müssen jedoch ihre git-Konfigurationsdatei einrichten. Geben Sie hierzu im Terminal Folgendes ein: `git config --global user.name "Your Name"` und dann `git config --global user.email "youremail@domain.com"`. Wenn Sie noch nicht über ein git-Konto verfügen, können Sie sich [für ein Konto auf GitHub registrieren](https://github.com/join). Wenn Sie noch nie mit git gearbeitet haben, können Ihnen [GitHub-Leitfäden](https://guides.github.com/) beim Einstieg helfen. Wenn Sie Ihre git-Konfiguration bearbeiten müssen, können Sie dies mit einem integrierten Text-Editor wie Nano: `nano ~/.gitconfig`tun.

2. Es wird empfohlen, Ihren Knoten Projekten eine [gitignore-Datei](https://help.github.com/en/articles/ignoring-files) hinzuzufügen. Hier finden Sie [die gitignore-Standardvorlage von GitHub für Node. js](https://github.com/github/gitignore/blob/master/Node.gitignore). Wenn Sie [ein neues Repository mithilfe der GitHub-Website erstellen](https://help.github.com/articles/create-a-repo), sind Kontrollkästchen verfügbar, mit denen Sie Ihr Repository mit einer Infodatei, einer gitignore-Datei für Node. js-Projekte und Optionen zum Hinzufügen einer Lizenz bei Bedarf initialisieren können.

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt eine Node. js-Entwicklungsumgebung eingerichtet. Um mit der Verwendung der Node. js-Umgebung zu beginnen, sollten Sie eines der folgenden Tutorials ausprobieren:

- [Einstieg in Node. js für Einsteiger](./beginners.md)
- [Einstieg in Node. js-Webframe Works unter Windows](./web-frameworks.md)
- [Einstieg in das Verbinden von Node. js-apps mit einer Datenbank](./databases.md)
- [Einstieg in die Verwendung von Docker-Containern mit Node. js](./containers.md)
