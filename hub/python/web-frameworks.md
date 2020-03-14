---
title: Webentwicklung mit Python unter Windows
description: Erste Schritte bei der Verwendung von Python für die Webentwicklung unter Windows, einschließlich der Einrichtung für Frameworks wie Flask und Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Python unter Windows, Python-Web mit WSL, Python-Web-App mit Windows-Subsystem für Linux, Python-Webentwicklung unter Windows, Flask-App unter Windows, Django-App unter Windows, Python-Web, Flask-Webentwicklung unter Windows, Django-Webentwicklung unter Windows, Windows-Webentwicklung mit Python, VS Code Python-Webentwicklung, Remote-WSL-Erweiterung, Ubuntu, WSL, venv, PIP, Microsoft Python-Erweiterung, Python unter Windows ausführen, Python unter Windows verwenden, mit Python unter Windows erstellen
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d883007168e0baf35f8a0ab0827505b683cfd291
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79208975"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Erste Schritte bei der Verwendung von Python für die Webentwicklung unter Windows

Im Folgenden finden Sie eine ausführliche Anleitung für den Einstieg in die Verwendung von Python für die Webentwicklung unter Windows mithilfe des Windows-Subsystems für Linux (WSL).

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Bei der Erstellung von Webanwendungen wird die Installation von Python unter WSL empfohlen. Viele der Tutorials und Anleitungen für die Python-Webentwicklung sind für Linux-Benutzer geschrieben und verwenden Linux-basierte Verpackungs- und Installationstools. Die meisten Web-Apps werden auch unter Linux bereitgestellt, sodass Sie sicher sein können, dass die Konsistenz zwischen Ihrer Entwicklungs- und Produktionsumgebung gewährleistet ist.

Wenn Sie Python für andere Vorgänge als die Webentwicklung verwenden, empfiehlt es sich, Python über den Microsoft Store direkt unter Windows 10 zu installieren. WSL unterstützt GUI-Desktops und GUI-Anwendungen nicht (z. B. PyGame, Gnome oder KDE). Installieren und verwenden Sie Python in diesen Fällen direkt unter Windows. Wenn Sie noch nicht mit Python vertraut sind, lesen Sie unsere Anleitung: [Erste Schritte bei der Verwendung von Python unter Windows für Anfänger](./beginners.md). Wenn Sie gängige Aufgaben im Betriebssystem automatisieren möchten, finden Sie entsprechende Informationen in unserer Anleitung [Erste Schritte bei der Verwendung von Python unter Windows für Skripts und Automatisierung](./scripting.md). Für einige erweiterte Szenarien empfiehlt es sich, ein bestimmtes Python-Release direkt unter [python.org](https://www.python.org/downloads/windows/) herunterzuladen oder eine [Alternative](https://www.python.org/download/alternatives) zu installieren, z. B. Anaconda, Jython, PyPy, WinPython, IronPython usw. Dies wird nur empfohlen, wenn Sie ein versierter Python-Programmierer sind und bestimmte Gründe für die Auswahl einer alternativen Implementierung haben.

## <a name="enable-windows-subsystem-for-linux"></a>Aktivieren des Windows-Subsystems für Linux

WSL ermöglicht Ihnen, eine GNU/Linux-Umgebung – einschließlich der meisten Befehlszeilentools, Hilfsprogramme und Anwendungen – direkt unter Windows auszuführen, unverändert und vollständig in Ihr Windows-Dateisystem und in bevorzugte Tools wie Visual Studio Code integriert. Überprüfen Sie vor dem Aktivieren von WSL, ob Sie die [neueste Version von Windows 10](https://www.microsoft.com/software-download/windows10) installiert haben.

Zum Aktivieren von WSL auf Ihrem Computer ist Folgendes erforderlich:

1. Wechseln Sie zum **Startmenü** (Windows-Symbol unten links), geben Sie „Windows-Features aktivieren oder deaktivieren“ ein, und wählen Sie den Link zur **Systemsteuerung** aus, um das Popupmenü **Windows-Features** zu öffnen. Suchen Sie in der Liste den Eintrag „Windows-Subsystem für Linux“, und aktivieren Sie das Kontrollkästchen, um das Feature zu aktivieren.

2. Starten Sie den Computer neu, wenn Sie dazu aufgefordert werden.

## <a name="install-a-linux-distribution"></a>Installieren einer Linux-Distribution

Mehrere Linux-Distributionen sind zur Ausführung unter dem WSL verfügbar. Sie können Ihre bevorzugte Version im Microsoft Store suchen und installieren. Es wird empfohlen, mit [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) zu beginnen, da es aktuell und beliebt ist und gut unterstützt wird.

1. Öffnen Sie den Link [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), öffnen Sie den Microsoft Store, und wählen Sie **Abrufen** aus. *(Dies ist ein ziemlich großer Download, und die Installation kann eine Weile dauern.)*

2. Wählen Sie nach Abschluss des Downloads die Option **Starten** im Microsoft Store aus, oder geben Sie „Ubuntu 18.04 LTS“ im **Startmenü** ein.

3. Sie werden aufgefordert, einen Kontonamen und das Kennwort zu erstellen, wenn Sie die Distribution zum ersten Mal ausführen. Anschließend werden Sie standardmäßig automatisch als dieser Benutzer angemeldet. Sie können beliebige Werte für Benutzername und Kennwort auswählen. Sie müssen sich nicht an Ihrem Windows-Benutzernamen orientieren.

Sie können die derzeit verwendete Linux-Distribution überprüfen, indem Sie Folgendes eingeben: `lsb_release -d`. Verwenden Sie `sudo apt update && sudo apt upgrade`, um die Ubuntu-Distribution zu aktualisieren. Eine regelmäßige Aktualisierung empfiehlt sich, um sicherzustellen, dass Sie über die neuesten Pakete verfügen. Dieses Update wird unter Windows nicht automatisch verarbeitet. Links zu anderen im Microsoft Store verfügbaren Linux-Distributionen sowie Informationen zu alternativen Installationsmethoden oder zur Problembehandlung finden Sie unter [Windows-Subsystem für Linux: Installationsleitfaden für Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

> [!TIP]
> Sie sollten das neue [Windows-Terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) ausprobieren, wenn Sie mehrere Befehlszeilen (Ubuntu, PowerShell, Windows-Eingabeaufforderung usw.) verwenden oder [das Terminal anpassen](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md) möchten, einschließlich Text, Hintergrundfarben, Tastenbindungen usw.

## <a name="set-up-visual-studio-code"></a>Einrichten von Visual Studio Code

Durch Verwendung von Visual Studio Code können Sie [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), [Linting](https://code.visualstudio.com/docs/python/linting), die [Debugunterstützung](https://code.visualstudio.com/docs/python/debugging), [Codeausschnitte](https://code.visualstudio.com/docs/editor/userdefinedsnippets) und [Komponententests](https://code.visualstudio.com/docs/python/unit-testing) nutzen. VS Code lässt sich mühelos in das Windows-Subsystem für Linux integrieren und bietet ein [integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) für einen nahtlosen Workflow zwischen dem Code-Editor und der Befehlszeile zusätzlich zur Unterstützung von [Git für die Versionskontrolle](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) mit allgemeinen Git-Befehlen (add, commit, push, pull), die direkt in die Benutzeroberfläche integriert sind.

1. [Laden Sie VS Code für Windows herunter, und installieren Sie es.](https://code.visualstudio.com) VS Code ist auch für Linux verfügbar. Da das Windows-Subsystem für Linux jedoch keine GUI-Apps unterstützt, muss VS Code unter Windows installiert werden. Die Integration in die Befehlszeile und die Tools von Linux ist jedoch durch Verwendung der Erweiterung „Remote – WSL“ problemlos möglich.

2. Installieren Sie die [Erweiterung „Remote – WSL“](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) in VS Code. Dies ermöglicht Ihnen die Verwendung von WSL als integrierte Entwicklungsumgebung, während die Verarbeitung der Kompatibilität und der Pfadzuordnung automatisch erfolgt. [Weitere Informationen](https://code.visualstudio.com/docs/remote/remote-overview)

> [!IMPORTANT]
> Wenn Sie VS Code bereits installiert haben, müssen Sie sicherstellen, dass Sie über das [Release vom Mai (1.35)](https://code.visualstudio.com/updates/v1_35) oder höher verfügen, um die [Erweiterung „Remote – WSL“](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) installieren zu können. Es wird davon abgeraten, WSL in VS Code ohne die Erweiterung „Remote – WSL“ zu verwenden, da dann keine Unterstützung für automatisches Vervollständigen, Debuggen, Linting usw. vorhanden ist. Hinweis: Diese WSL-Erweiterung wird unter „$HOME/.vscode-server/extensions“ installiert.

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

Wir erstellen nun ein neues Projektverzeichnis im Linux-Dateisystem (Ubuntu), in dem dann Linux-Apps und Linux-Tools mit VS Code verwendet werden.

1. Schließen Sie VS Code, und öffnen Sie Ubuntu 18.04 (die WSL-Befehlszeile), indem Sie im **Startmenü** (Windows-Symbol unten links) Folgendes eingeben: „Ubuntu 18.04“.

2. Navigieren Sie in der Ubuntu-Befehlszeile zu dem Speicherort, an dem das Projekt abgelegt werden soll, und erstellen Sie ein Verzeichnis für das Projekt: `mkdir HelloWorld`.

![Ubuntu-Terminal](../images/ubuntu-terminal.png)

> [!TIP]
> Beachten Sie bei der Verwendung des Windows-Subsystems für Linux (WSL) unbedingt, dass **Sie nun in zwei unterschiedlichen Dateisystemen** arbeiten: 1) im Windows-Dateisystem und 2) im Linux-Dateisystem (WSL), in diesem Beispiel Ubuntu. Es ist daher darauf zu achten, wo Sie Pakete installieren und Dateien speichern. Sie können eine Version eines Tools oder Pakets im Windows-Dateisystem und eine andere Version im Linux-Dateisystem installieren. Die Aktualisierung des Tools im Windows-Dateisystem wirkt sich nicht auf das Tool im Linux-Dateisystem aus und umgekehrt. Mit WSL werden die lokalen Festplattenlaufwerke auf dem Computer unter dem Ordner `/mnt/<drive>` in der Linux-Distribution eingebunden. Das Windows-Laufwerk C: wird beispielsweise unter `/mnt/c/` eingebunden. Sie können auf die Windows-Dateien über das Ubuntu-Terminal zugreifen und Linux-Apps und -Tools für diese Dateien verwenden und umgekehrt. Es wird empfohlen, für die Python-Webentwicklung das Linux-Dateisystem zu verwenden, da ein Großteil der Webtools ursprünglich für Linux geschrieben wurde und in einer Linux-Produktionsumgebung bereitgestellt wird. Außerdem wird so vermieden, dass sich die Dateisystemsemantik vermischt (z. B. muss unter Windows für Dateinamen die Groß- und Kleinschreibung nicht berücksichtigt werden). Jedoch kann in WSL nun zwischen dem Linux- und dem Windows-System gewechselt werden, sodass Sie Ihre Dateien in beiden Systemen hosten können. [Weitere Informationen](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/) Wir freuen uns auch, mitteilen zu können, dass [WSL2 in Kürze für Windows verfügbar ist](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) und weitreichende Verbesserungen bietet. Sie können es [nun im Windows Insiders-Build 18917](https://docs.microsoft.com/windows/wsl/wsl2-install) testen.

## <a name="install-python-pip-and-venv"></a>Installieren von Python, pip und venv

In Ubuntu 18.04 LTS ist Python 3.6 bereits installiert, dagegen sind einige Module nicht enthalten, die Sie möglicherweise mit anderen Python-Installationen erhalten. **pip**, der standardmäßige Paket-Manager für Python, und **venv**, das zum Erstellen und Verwalten von einfachen virtuellen Umgebungen verwendete Standardmodul, müssen noch installiert werden.  

1. Vergewissern Sie sich, dass Python3 bereits installiert ist, indem Sie das Ubuntu-Terminal öffnen und Folgendes eingeben: `python3 --version`. Dadurch sollte die Python-Versionsnummer zurückgegeben werden. Wenn Sie Ihre Python-Version aktualisieren müssen, aktualisieren Sie zunächst die Ubuntu-Version, indem Sie Folgendes eingeben: `sudo apt update && sudo apt upgrade`. Aktualisieren Sie anschließend Python unter Verwendung von `sudo apt upgrade python3`.

2. Installieren Sie **pip** mit folgender Eingabe: `sudo apt install python3-pip`. Über pip können Sie zusätzliche Pakete installieren und verwalten, die nicht Teil der Python-Standardbibliothek sind.

3. Installieren Sie **venv** mit folgender Eingabe: `sudo apt install python3-venv`.

## <a name="create-a-virtual-environment"></a>Erstellen einer virtuellen Umgebung

Die Verwendung virtueller Umgebungen wird als bewährte Methode für Python-Entwicklungsprojekte empfohlen. Durch das Erstellen einer virtuellen Umgebung können Sie Ihre Projekttools isolieren und Versionskonflikte mit Tools für andere Projekte vermeiden. Beispielsweise können Sie ein älteres Webprojekt verwalten, für das das Django 1.2-Webframework erforderlich ist, und dann ein neues Projekt mit Django 2.2 erstellen. Wenn Sie Django global außerhalb einer virtuellen Umgebung aktualisieren, können später Versionsprobleme auftreten. Mit virtuellen Umgebungen können Sie nicht nur versehentliche Versionskonflikte verhindern, sondern auch Pakete ohne Administratorrechte installieren und verwalten.

1. Öffnen Sie das Terminal, und erstellen Sie im Projektordner *HelloWorld* mit dem Befehl `python3 -m venv .venv` die virtuelle Umgebung mit dem Namen **.venv**.

2. Aktivieren Sie die virtuelle Umgebung mit der Eingabe `source .venv/bin/activate`. Bei erfolgreicher Aktivierung sollte **(.venv)** vor der Eingabeaufforderung angezeigt werden. Sie verfügen damit über eine eigenständige Umgebung, in der Code geschrieben und Pakete installiert werden können. Wenn Sie die virtuelle Umgebung nicht mehr verwenden möchten, aktivieren Sie sie mit dem Befehl `deactivate`.

    ![Erstellen einer virtuellen Umgebung](../images/wsl-venv.png)

> [!TIP]
> Es wird empfohlen, die virtuelle Umgebung in dem Verzeichnis zu erstellen, in dem das Projekt gespeichert werden soll. Da jedes Projekt über ein separates Verzeichnis verfügen muss, weist jedes Projekt auch eine eigene virtuelle Umgebung auf, sodass keine eindeutige Benennung erforderlich ist. Es empfiehlt sich, entsprechend der Python-Konvention den Namen **.venv** zu verwenden. Auch bei einigen Tools (z. B. pipenv) wird dieser Name standardmäßig verwendet, wenn Sie sie direkt im Projektverzeichnis installieren. Verwenden Sie nicht den Namen **.env**, da dies mit den Definitionsdateien für Umgebungsvariablen in Konflikt steht. Im Allgemeinen wird von Namen ohne vorangestellten Punkt abgeraten, da andernfalls mit `ls` ständig daran erinnert wird, dass das Verzeichnis vorhanden ist. Außerdem wird empfohlen, der GITIGNORE-Datei **.venv** hinzuzufügen. (Siehe dazu als Referenz die [GitHub-GITIGNORE-Standardvorlage für Python](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106).) Weitere Informationen zur Verwendung virtueller Umgebungen in VS Code finden Sie unter [Using Python environments in VS Code](https://code.visualstudio.com/docs/python/environments) (Verwenden von Python-Umgebungen in VS Code).

## <a name="open-a-wsl---remote-window"></a>Öffnen eines Fensters der Erweiterung „Remote – WSL“

In VS Code wird mit der (zuvor installierten) Erweiterung „Remote – WSL“ das Linux-Subsystem als Remoteserver behandelt. Dies ermöglicht Ihnen die Verwendung von WSL als integrierte Entwicklungsumgebung. [Weitere Informationen](https://code.visualstudio.com/docs/remote/wsl) 

1. Öffnen Sie den Projektordner in VS Code über das Ubuntu-Terminal, indem Sie `code .` eingeben (mit „.“ wird in VS Code angegeben, dass der aktuelle Ordner zu öffnen ist).

2. Eine Sicherheitswarnung von Windows Defender wird angezeigt. Wählen Sie „Zugriff zulassen“ aus. Nach dem Öffnen von VS Code sollte unten links die Anzeige des Remoteverbindungshosts zu sehen sein, sodass Sie wissen, dass die Bearbeitung unter **WSL: Ubuntu-18.04** erfolgt.

    ![Anzeige des Remoteverbindungshosts in VS Code](../images/wsl-remote-extension.png)

3. Schließen Sie das Ubuntu-Terminal. Von nun an wird das in VS Code integrierte WSL-Terminal verwendet.

4. Öffnen Sie das WSL-Terminal in VS Code durch Drücken von **STRG+`** (Graviszeichen) oder Auswählen von **Ansicht** > **Terminal**. Dadurch wird eine Bash-Befehlszeile (WSL) für den Projektordnerpfad geöffnet, den Sie im Ubuntu-Terminal erstellt haben.

    ![VS Code mit WSL-Terminal](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Installieren der Microsoft Python-Erweiterung

Sie müssen alle VS Code-Erweiterungen für die Erweiterung „Remote – WSL“ installieren. Erweiterungen, die in VS Code bereits lokal installiert wurden, sind nicht automatisch verfügbar. [Weitere Informationen](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)

1. Öffnen Sie das Fenster der VS Code-Erweiterungen, indem Sie **STRG+UMSCHALT+X** drücken (oder navigieren Sie im Menü zu **Ansicht** > **Erweiterungen**).

2. Geben Sie im oberen Feld **Nach Extensions in Marketplace suchen** Folgendes ein:  **Python**.

3. Suchen Sie die Erweiterung **Python (ms-python.python) by Microsoft**, und wählen Sie die grüne Schaltfläche **Install** (Installieren) aus.

4. Nachdem die Installation der Erweiterung abgeschlossen ist, müssen Sie die blaue Schaltfläche **Reload Required** (Erneutes Laden erforderlich) auswählen. Dadurch wird VS Code erneut geladen, und der Bereich **WSL: UBUNTU-18.04 – Installed** (WSL: UBUNTU-18.04 – Installiert) wird im Fenster der VS Code-Erweiterungen angezeigt und gibt somit an, dass die Python-Erweiterung installiert ist.

## <a name="run-a-simple-python-program"></a>Ausführen eines einfachen Python-Programms

Python ist eine interpretierte Sprache und unterstützt verschiedene Arten von Interpretern (Python2, Anaconda, PyPy usw.). In VS Code sollte standardmäßig der Interpreter ausgewählt sein, der Ihrem Projekt zugeordnet ist. Wenn Sie diesen Interpreter aus einem bestimmten Grund ändern möchten, wählen Sie den aktuell in der blauen Leiste unten im VS Code-Fenster angezeigten Interpreter aus, oder öffnen Sie die **Befehlspalette** (STRG+UMSCHALT+P), und geben Sie den Befehl **Python: Select Interpreter** ein. Dadurch wird eine Liste der derzeit installierten Python-Interpreter angezeigt. [Weitere Informationen zum Konfigurieren von Python-Umgebungen](https://code.visualstudio.com/docs/python/environments).

Wir erstellen nun ein einfaches Python-Programm als Test, führen es aus und überprüfen, ob der richtige Python-Interpreter ausgewählt ist.

1. Öffnen Sie den VS Code-Datei-Explorer, indem Sie **STRG+UMSCHALT+E** drücken (oder im Menü zu**Ansicht** > **Explorer** navigieren).

2. Wenn es noch nicht geöffnet ist, öffnen Sie das integrierte WSL-Terminal, indem Sie **STRG+UMSCHALT+`** drücken, und vergewissern Sie sich, dass der Ordner des Python-Projekts **HelloWorld** ausgewählt ist.

3. Erstellen Sie eine Python-Datei, indem Sie `touch test.py` eingeben. Die soeben erstellte Datei sollte im Explorer-Fenster unter den bereits im Projektverzeichnis vorhandenen Ordnern „.venv“ und „.vscode“ angezeigt werden.

4. Wählen Sie die soeben erstellte Datei **test.py** im Explorer-Fenster aus, um sie in VS Code zu öffnen. Da durch die Erweiterung „.py“ im Dateinamen in VS Code erkannt wird, dass es sich um eine Python-Datei handelt, wird mit der zuvor geladenen Python-Erweiterung automatisch ein Python-Interpreter ausgewählt und geladen, der dann unten im VS Code-Fenster angezeigt wird.

    ![Auswählen des Python-Interpreters in VS Code](../images/interpreterselection.gif)

5. Fügen Sie den folgenden Python-Code in der Datei „test.py“ ein, und speichern Sie dann die Datei (STRG+S). 

    ```python
    print("Hello World")
    ```

6. Wählen Sie zum Ausführen des soeben erstellten „Hallo Welt“-Python-Programms im VS Code-Explorer-Fenster die Datei **test.py** aus, und klicken Sie dann mit der rechten Maustaste auf die Datei, um ein Menü mit Optionen anzuzeigen. Wählen Sie **Run Python File in Terminal** (Python-Datei im Terminal ausführen) aus. Alternativ können Sie `python test.py` im integrierten WSL-Terminalfenster eingeben, um das „Hallo Welt“-Programm auszuführen. Der Python-Interpreter gibt „Hello World“ im Terminalfenster aus.

Herzlichen Glückwunsch! Sie können nun Python-Programme erstellen und ausführen. Nun erstellen wir die „Hallo Welt“-App mit zwei der beliebtesten Python-Webframeworks: Flask und Django.

## <a name="hello-world-tutorial-for-flask"></a>Tutorial zur „Hallo Welt“-App für Flask

[Flask](http://flask.pocoo.org/) ist ein Webanwendungsframework für Python. In diesem kurzen Tutorial erstellen Sie eine kleine „Hallo Welt“-Flask-App unter Verwendung von VS Code und WSL.

1. Öffnen Sie Ubuntu 18.04 (die WSL-Befehlszeile), indem Sie im **Startmenü** (Windows-Symbol unten links) Folgendes eingeben: „Ubuntu 18.04“.

2. Erstellen Sie ein Verzeichnis für das Projekt: `mkdir HelloWorld-Flask`, und geben Sie dann mit `cd HelloWorld-Flask` das Verzeichnis ein.

3. Erstellen Sie eine virtuelle Umgebung, um die Projekttools zu installieren: `python3 -m venv .venv`

4. Öffnen Sie das Projekt **HelloWorld-Flask** in VS Code durch Eingeben des Befehls `code .`.

5. Öffnen Sie in VS Code das integrierte WSL-Terminal (bzw. Bash), indem Sie **STRG+UMSCHALT+`** drücken. (Der Projektordner **HelloWorld-Flask** sollte bereits ausgewählt sein.) *Schließen Sie die Ubuntu-Befehlszeile, da ab jetzt das in VS Code integrierte WSL-Terminal verwendet wird.*

6. Aktivieren Sie die in Schritt 3 erstellte virtuelle Umgebung über das Bash-Terminal in VS Code: `source .venv/bin/activate`. Bei erfolgreicher Aktivierung sollte „(.venv)“ vor der Eingabeaufforderung angezeigt werden.

7. Installieren Sie Flask in der virtuellen Umgebung, indem Sie `python3 -m pip install flask` eingeben. Überprüfen Sie durch Eingeben von `python3 -m flask --version`, ob Flask installiert ist.

8. Erstellen Sie eine neue Datei für den Python-Code: `touch app.py`

9. Öffnen Sie die Datei **app.py** im Datei-Explorer von VS Code (`Ctrl+Shift+E`, dann die Datei „app.py“ auswählen). Dadurch wird die Python-Erweiterung aktiviert, sodass ein Interpreter ausgewählt wird. Standardmäßig sollte **Python 3.6.8 64-bit ('.venv': venv)** angezeigt werden. Beachten Sie, dass auch die virtuelle Umgebung erkannt wurde.

    ![Aktivierte virtuelle Umgebung](../images/virtual-environment.png)

10. Fügen Sie in der Datei **app.py** Code zum Importieren von Flask und Erstellen einer Instanz des Flask-Objekts hinzu:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. Fügen Sie in **app.py** außerdem eine Funktion hinzu, die den Inhalt zurückgibt, in diesem Fall eine einfache Zeichenfolge. Verwenden Sie den Flask-Decorator **app.route**, um dieser Funktion die URL-Route „/“ zuzuordnen:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Sie können mehrere Decorators für dieselbe Funktion verwenden, jeweils einen pro Zeile, je nachdem, wie viele verschiedene Routen Sie derselben Funktion zuordnen möchten.

12. Speichern Sie die Datei **app.py** (**STRG+S**).

13. Führen Sie die App im Terminal aus, indem Sie den folgenden Befehl eingeben:

    ```python
    python3 -m flask run
    ```

    Dadurch wird der Flask-Entwicklungsserver ausgeführt. Der Entwicklungsserver sucht standardmäßig nach **app.py**. Beim Ausführen von Flask sollte eine Ausgabe ähnlich der folgenden angezeigt werden:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Öffnen Sie die gerenderte Seite im Standardwebbrowser, und klicken Sie bei gedrückter **STRG-Taste** auf die URL http://127.0.0.1:5000/ im Terminal. Im Browser sollte die folgende Meldung angezeigt werden:

    ![Hello, Flask!](../images/hello-flask.png)

15. Beachten Sie, dass beim Aufrufen einer URL wie „/“ im Debugterminal eine Meldung mit der HTTP-Anforderung angezeigt wird:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Beenden Sie die App durch Drücken von **STRG+C** im Terminal.

> [!TIP]
> Wenn Sie einen anderen Dateinamen als **app.py** verwenden möchten, z. B. **program.py**, definieren Sie eine Umgebungsvariable mit dem Namen **FLASK_APP**, und legen Sie ihren Wert auf die ausgewählte Datei fest. Auf dem Flask-Entwicklungsserver wird dann der Wert von **FLASK_APP** anstelle der Standarddatei **app.py** verwendet. Weitere Informationen finden Sie in der [Dokumentation zur Befehlszeilenschnittstelle von Flask](http://flask.pocoo.org/docs/1.0/cli/).

Herzlichen Glückwunsch, Sie haben eine Flask-Webanwendung mithilfe von Visual Studio Code und des Windows-Subsystems für Linux erstellt! Ein ausführlicheres Tutorial unter Verwendung von VS Code und Flask finden Sie im [Tutorial zu Flask in Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Tutorial zur „Hallo Welt“-App für Django

[Django](https://www.djangoproject.com) ist ein Webanwendungsframework für Python. In diesem kurzen Tutorial erstellen Sie eine kleine „Hallo Welt“-Django-App unter Verwendung von VS Code und WSL.

1. Öffnen Sie Ubuntu 18.04 (die WSL-Befehlszeile), indem Sie im **Startmenü** (Windows-Symbol unten links) Folgendes eingeben: „Ubuntu 18.04“.

2. Erstellen Sie ein Verzeichnis für das Projekt: `mkdir HelloWorld-Django`, und geben Sie dann mit `cd HelloWorld-Django` das Verzeichnis ein.

3. Erstellen Sie eine virtuelle Umgebung, um die Projekttools zu installieren: `python3 -m venv .venv`

4. Öffnen Sie das Projekt **HelloWorld-Django** in VS Code durch Eingeben des Befehls `code .`.

5. Öffnen Sie in VS Code das integrierte WSL-Terminal (bzw. Bash), indem Sie **STRG+UMSCHALT+`** drücken. (Der Projektordner **HelloWorld-Django** sollte bereits ausgewählt sein.) *Schließen Sie die Ubuntu-Befehlszeile, da ab jetzt das in VS Code integrierte WSL-Terminal verwendet wird.*

6. Aktivieren Sie die in Schritt 3 erstellte virtuelle Umgebung über das Bash-Terminal in VS Code: `source .venv/bin/activate`. Bei erfolgreicher Aktivierung sollte „(.venv)“ vor der Eingabeaufforderung angezeigt werden.

7. Installieren Sie Django mit dem Befehl `python3 -m pip install django` in der virtuellen Umgebung. Überprüfen Sie durch Eingeben von `python3 -m django --version`, ob Django installiert ist.

8. Führen Sie anschließend den folgenden Befehl aus, um das Django-Projekt zu erstellen:

    ```bash
    django-admin startproject web_project .
    ```

    Bei dem Befehl `startproject` wird davon ausgegangen (durch Verwendung von `.` am Ende), dass es sich bei dem aktuellen Ordner um den Projektordner handelt, und in diesem Ordner wird Folgendes erstellt:

    - `manage.py`: Das Verwaltungsprogramm der Django-Befehlszeile für das Projekt. Verwaltungsbefehle für das Projekt führen Sie mithilfe von `python manage.py <command> [options]` aus.

    - Ein Unterordner mit dem Namen `web_project`, der die folgenden Dateien enthält:
        - `__init__.py`: eine leere Datei, die für Python angibt, dass es sich bei dem Ordner um ein Python-Paket handelt.
        - `wsgi.py`: einen Einstiegspunkt für WSGI-kompatible Webserver zur Bereitstellung des Projekts. Sie lassen diese Datei normalerweise unverändert, da sie die Hooks für Produktionswebserver enthält.
        - `settings.py`: Einstellungen für das Django-Projekt, die Sie während der Entwicklung einer Web-App ändern.
        - `urls.py`: ein Inhaltsverzeichnis für das Django-Projekt, das Sie auch während der Entwicklung ändern.

9. Zum Überprüfen des Django-Projekts starten Sie den Django-Entwicklungsserver mithilfe des Befehls `python3 manage.py runserver`. Der Server wird auf dem Standardport 8000 ausgeführt. Es sollte eine Ausgabe ähnlich der folgenden im Terminalfenster angezeigt werden:

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    Wenn Sie den Server erstmalig ausführen, wird in der Datei `db.sqlite3` eine SQLite-Standarddatenbank erstellt, die für Entwicklungszwecke vorgesehen ist, aber in der Produktionsumgebung für Web-Apps mit geringem Volumen verwendet werden kann. Außerdem ist der integrierte Webserver von Django *nur* für lokale Entwicklungszwecke vorgesehen. Bei der Bereitstellung auf einem Webhost wird in Django jedoch stattdessen der Webserver des Hosts verwendet. Das Modul `wsgi.py` im Django-Projekt übernimmt die Einbindung in die Produktionsserver.

    Wenn Sie einen anderen Port als den Standardport 8000 verwenden möchten, geben Sie die Portnummer an der Befehlszeile an, z. B. `python3 manage.py runserver 5000`.

10. `Ctrl+click` auf die URL `http://127.0.0.1:8000/` im Terminalausgabefenster, um diese Adresse im Standardbrowser zu öffnen. Wenn Django ordnungsgemäß installiert und das Projekt gültig ist, wird eine Standardseite angezeigt. Im VS Code-Terminalausgabefenster wird auch das Serverprotokoll angezeigt.

11. Wenn Sie fertig sind, schließen Sie das Browserfenster, und beenden Sie den Server in VS Code durch Drücken von `Ctrl+C`, wie im Terminalausgabefenster angegeben.

12. Führen Sie nun zum Erstellen einer Django-App den Befehl `startapp` des Verwaltungsprogramms im Projektordner aus (in dem sich `manage.py` befindet):

    ```bash
    python3 manage.py startapp hello
    ```

    Mit dem Befehl wird der Ordner `hello` erstellt, der verschiedene Codedateien und einen Unterordner enthält. Von diesen Codedateien verwenden Sie häufig `views.py` (enthält die Funktionen, mit denen Seiten in der Web-App definiert werden) und `models.py` (enthält die Klassen zum Definieren der Datenobjekte). Im Ordner `migrations` werden im Django-Verwaltungsprogramm die Datenversionen verwaltet (siehe weiter unten in diesem Tutorial). Auf die Dateien `apps.py` (App-Konfiguration), `admin.py` (zum Erstellen einer Verwaltungsoberfläche) und `tests.py` (für Tests), wird hier nicht weiter eingegangen.

13. Ändern Sie `hello/views.py` entsprechend dem folgenden Code, mit dem eine einzelne Ansicht für die Startseite der App erstellt wird:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Erstellen Sie die Datei `hello/urls.py` mit den folgenden Inhalten. In der Datei `urls.py` geben Sie Muster an, um verschiedene URLs an die entsprechenden Ansichten weiterzuleiten. Der folgende Code enthält eine Route zum Zuordnen der Stamm-URL der App (`""`) zu der Funktion `views.home`, die Sie soeben `hello/views.py` hinzugefügt haben:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. Der Ordner `web_project` enthält auch die Datei `urls.py`, in der das URL-Routing tatsächlich verarbeitet wird. Öffnen Sie `web_project/urls.py`, und ändern Sie sie entsprechend dem folgenden Code (die Anweisungskommentare können gegebenenfalls beibehalten werden). Über diesen Code wird die Datei `hello/urls.py` der App mithilfe von `django.urls.include` abgerufen, sodass die Routen der App innerhalb der App beibehalten werden. Diese Trennung ist hilfreich, wenn ein Projekt mehrere Apps enthält.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Speichern Sie alle geänderten Dateien.

17. Führen Sie im VS Code-Terminal den Entwicklungsserver mit `python manage.py runserver` aus, und öffnen Sie `http://127.0.0.1:8000/` in einem Browser, um die Seite anzuzeigen, auf der „Hello, Django“ ausgegeben wird.

Herzlichen Glückwunsch, Sie haben eine Django-Webanwendung mithilfe von Visual Studio Code und des Windows-Subsystems für Linux erstellt! Ein ausführlicheres Tutorial unter Verwendung von VS Code und Django finden Sie im [Tutorial zu Django in Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Tutorial zu Python in Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial): Ein Einführungstutorial zu VS Code als Python-Umgebung, im Wesentlichen Informationen zum Bearbeiten, Ausführen und Debuggen von Code.
- [Git-Unterstützung in VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): Informationen zur Verwendung der Git-Versionskontrolle in VS Code.  
- [Erfahren Sie mehr über die demnächst mit WSL 2 verfügbaren Updates](https://docs.microsoft.com/windows/wsl/wsl2-index): Diese neue Version ändert die Interaktion von Linux-Distributionen mit Windows. Dadurch wird die Leistung des Dateisystems erhöht und vollständige Kompatibilität mit Systemaufrufen hinzugefügt.
- [Verwenden mehrerer Linux-Distributionen unter Windows](https://docs.microsoft.com/windows/wsl/wsl-config): Informationen zum Verwalten mehrerer verschiedener Linux-Distributionen auf Ihrem Windows-Computer.
