---
title: Webentwicklung mit Python unter Windows
description: Erfahren Sie mehr über die ersten Schritte bei der Verwendung von Python für die Webentwicklung unter Windows, einschließlich der Einrichtung von Frameworks wie Flask und Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: python, Windows 10, Microsoft, python unter Windows, Python Web mit WSL, Python-Web-App mit Windows-Subsystem für Linux, python-Webentwicklung unter Windows, Flask-app unter Windows, Django-app unter Windows, Python Web, Flask Web dev unter Windows, Django Web dev unter Windows, Windows Web dev mit Python, vs Code Python Web dev, Remote-WSL-Erweiterung, Ubuntu, WSL, venv, PIP, Microsoft python-Erweiterung, python unter Windows ausführen, python unter Windows verwenden, mit Python unter Windows erstellen
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: eafe85ac7e954d1a76708b059a191c14526afff8
ms.sourcegitcommit: 210034519678ba1a59744bc3a0b613b000921537
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68473685"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Beginnen Sie mit der Verwendung von Python für die Webentwicklung unter Windows.

Im folgenden finden Sie eine Schritt-für-Schritt-Anleitung für den Einstieg in die Verwendung von Python für die Webentwicklung unter Windows mithilfe des Windows-Subsystems für Linux (WSL).

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Bei der Erstellung von Webanwendungen wird die Installation von python auf WSL empfohlen. Viele der Tutorials und Anleitungen für die python-Webentwicklung sind für Linux-Benutzer geschrieben und verwenden die Linux-basierten Verpackungs-und Installations Tools. Die meisten Web-Apps werden auch unter Linux bereitgestellt. Dadurch wird sichergestellt, dass Sie Konsistenz zwischen ihren Entwicklungs-und Produktionsumgebungen haben.

Wenn Sie python für etwas anderes als die Webentwicklung verwenden, empfiehlt es sich, python mithilfe der Microsoft Store direkt unter Windows 10 zu installieren. WSL unterstützt keine GUI-Desktops oder-Anwendungen (wie pygame, gnome, KDE usw.). Installieren und verwenden Sie python in diesen Fällen direkt unter Windows. Wenn Sie noch nicht mit python vertraut sind, lesen Sie unser Handbuch: [Beginnen Sie mit der Verwendung von Python unter Windows für Einsteiger](./python-for-education.md). Wenn Sie daran interessiert sind, gängige Aufgaben für Ihr Betriebssystem zu automatisieren, finden Sie weitere Informationen im Leitfaden: [Beginnen Sie mit der Verwendung von Python unter Windows für die Skripterstellung und Automatisierung](./python-for-scripting.md). Für einige erweiterte Szenarien empfiehlt es sich, eine bestimmte Python-Version direkt von [python.org](https://www.python.org/downloads/windows/) herunterzuladen oder eine [Alternative](https://www.python.org/download/alternatives)zu installieren, z. b. Anaconda, jython, pypy, winpython, IronPython usw. Dies wird nur empfohlen, wenn Sie ein fortschrittlicher python-Programmierer mit einem bestimmten Grund für die Auswahl einer alternativen Implementierung sind.

## <a name="enable-windows-subsystem-for-linux"></a>Aktivieren des Windows-Subsystems für Linux

Mithilfe von WSL können Sie eine GNU/Linux-Umgebung ausführen, einschließlich der meisten Befehlszeilen Tools, Hilfsprogramme und Anwendungen direkt unter Windows, unverändert und vollständig in Ihr Windows-Dateisystem integriert und mit bevorzugten Tools wie Visual Studio Code. Vergewissern Sie sich vor dem Aktivieren von WSL, dass Sie über die [neueste Version von Windows 10](https://www.microsoft.com/software-download/windows10)verfügen.

Um WSL auf Ihrem Computer zu aktivieren, müssen Sie folgende Schritte ausführen:

1. Wechseln Sie zu  Ihrem Startmenü (unten links Windows-Symbol), geben Sie "Windows-Funktionen ein-oder ausschalten" ein, und klicken Sie auf den Link zur **Systemsteuerung** , um das Popup Menü " **Windows-Features** " zu öffnen. Suchen Sie in der Liste "Windows-Subsystem für Linux", und aktivieren Sie das Kontrollkästchen, um das Feature zu aktivieren.

2. Starten Sie den Computer bei entsprechender Aufforderung neu.

## <a name="install-a-linux-distribution"></a>Installieren einer Linux-Distribution

Es stehen mehrere Linux-Distributionen zur Verfügung, die auf WSL ausgeführt werden können. Sie können Ihre Favoriten in der Microsoft Store finden und installieren. Wir empfehlen, mit [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) zu beginnen, da es aktuell, beliebt und gut unterstützt wird.

1. Öffnen Sie diesen [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) -Link, öffnen Sie den Microsoft Store, und wählen Sie **Get**aus. *(Dieser Download ist recht umfangreich und kann einige Zeit in Anspruch nehmen.)*

2. Nachdem der Download abgeschlossen ist, wählen Sie **starten** von der Microsoft Store oder starten aus, indem Sie im Startmenü  "Ubuntu 18,04 LTS" eingeben.

3. Wenn Sie die Verteilung zum ersten Mal ausführen, werden Sie aufgefordert, einen Kontonamen und ein Kennwort zu erstellen. Anschließend werden Sie standardmäßig automatisch als dieser Benutzer angemeldet. Sie können einen beliebigen Benutzernamen und ein Kennwort auswählen. Sie haben keinen Einfluss auf den Windows-Benutzernamen.

Sie können die Linux-Distribution, die Sie zurzeit verwenden, über `lsb_release -d`prüfen, indem Sie Folgendes eingeben:. Verwenden Sie zum Aktualisieren der Ubuntu-Distribution `sudo apt update && sudo apt upgrade`:. Wir empfehlen, regelmäßig zu aktualisieren, um sicherzustellen, dass Sie über die neuesten Pakete verfügen Dieses Update wird von Windows nicht automatisch verarbeitet. Links zu anderen verfügbaren Linux-Distributionen, die im Microsoft Store verfügbar sind, finden Sie unter [Windows-Subsystem für Linux-Installationshandbuch für Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="set-up-visual-studio-code"></a>Einrichten Visual Studio Code

Profitieren Sie von [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), [linting](https://code.visualstudio.com/docs/python/linting), [Debug-Unterstützung](https://code.visualstudio.com/docs/python/debugging), [Code Ausschnitten](https://code.visualstudio.com/docs/editor/userdefinedsnippets)und Komponenten [Tests](https://code.visualstudio.com/docs/python/unit-testing) , indem Sie vs Code verwenden. VS Code ist gut in das Windows-Subsystem für Linux integriert und bietet ein [integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) , mit dem Sie einen nahtlosen Workflow zwischen Ihrem Code-Editor und der Befehlszeile einrichten können, zusätzlich zur Unterstützung von [git für die Versionskontrolle](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) mit Common git Befehle (Add, Commit, Push, Pull), die direkt in die Benutzeroberfläche integriert wurden.

1. [Laden Sie vs Code für Windows herunter, und installieren Sie](https://code.visualstudio.com)es. VS Code ist auch für Linux verfügbar, aber das Windows-Subsystem für Linux unterstützt keine GUI-apps, daher müssen wir es unter Windows installieren. Keine Sorge, Sie können mit der Remote-WSL-Erweiterung weiterhin in Ihre Linux-Befehlszeile und-Tools integrieren.

2. Installieren Sie die [Remote-WSL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) auf vs Code. Dies ermöglicht Ihnen die Verwendung von WSL als integrierte Entwicklungsumgebung und übernimmt die Kompatibilität und das richtige für Sie. [Weitere Informationen](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Wenn Sie bereits vs Code installiert haben, müssen Sie sicherstellen, dass Sie über die [Version 1,35](https://code.visualstudio.com/updates/v1_35) oder höher verfügen, um die [Remote-WSL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)installieren zu können. Es wird nicht empfohlen, WSL in vs Code ohne die Remote-WSL-Erweiterung zu verwenden, da Sie die Unterstützung für automatisches vervollständigen, Debuggen, linting usw. verlieren. Spaß Fakt: Diese WSL-Erweiterung wird in $Home/.vscode-Server/Extensions. installiert.

## <a name="create-a-new-project"></a>Erstellt ein neues Projekt

Wir erstellen nun ein neues Projektverzeichnis auf dem Linux-Dateisystem (Ubuntu), mit dem wir dann mit Linux-apps und-Tools mit vs Code arbeiten werden.

1. Schließen Sie vs Code, und öffnen Sie Ubuntu 18,04 (Ihre WSL-Befehlszeile)  , indem Sie in das Startmenü (unten links Fenster) klicken und Folgendes eingeben: "Ubuntu 18,04".

2. Navigieren Sie in der Ubuntu-Befehlszeile zu dem Speicherort, an dem das Projekt abgelegt werden soll, und erstellen `mkdir HelloWorld`Sie ein Verzeichnis für dieses:.

![Ubuntu-Terminal](../../images/ubuntu-terminal.png)

> [!TIP]
> Beachten Sie bei der Verwendung des Windows-Subsystems für Linux (WSL) unbedingt, dass **Sie nun zwischen zwei unterschiedlichen Dateisystemen arbeiten**: 1) Ihr Windows-Dateisystem und 2) Ihr Linux-Dateisystem (WSL), das für unser Beispiel Ubuntu ist. Sie müssen darauf achten, wo Sie Pakete installieren und Dateien speichern. Sie können eine Version eines Tools oder Pakets im Windows-Dateisystem und eine völlig andere Version im Linux-Dateisystem installieren. Das Aktualisieren des Tools im Windows-Dateisystem wirkt sich nicht auf das Tool im Linux-Dateisystem aus und umgekehrt. WSL stellt die Festplattenlaufwerke auf Ihrem Computer unter dem Ordner<drive> im in Ihrer Linux-Distribution bereit. Beispielsweise wird Ihr Windows C:-Laufwerk unter `/mnt/c/`eingebunden. Sie können auf Ihre Windows-Dateien über das Ubuntu-Terminal zugreifen und Linux-apps und-Tools für diese Dateien verwenden und umgekehrt. Es wird empfohlen, im Linux-Dateisystem für die python-Webentwicklung zu arbeiten, da ein Großteil der WebTools ursprünglich für Linux geschrieben und in einer Linux-Produktionsumgebung bereitgestellt wurde. Außerdem wird vermieden, dass die Dateisystem Semantik gemischt wird (z. b. bei Windows wird die Groß-/Kleinschreibung nicht beachtet Allerdings unterstützt WSL jetzt das Springen zwischen den Linux-und Windows-Dateisystemen, sodass Sie Ihre Dateien auf beiden Computern hosten können. [Weitere Informationen](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/). Wir freuen uns auch, dass [WSL2 bald an Windows weitergegeben wird](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) und einige tolle Verbesserungen bietet. Sie können [es jetzt auf Windows Insider Build 18917 testen](https://docs.microsoft.com/windows/wsl/wsl2-install).

## <a name="install-python-pip-and-venv"></a>Installieren von Python, PIP und venv

Ubuntu 18,04 LTS ist bereits mit Python 3,6 installiert, aber es gibt noch nicht einige der Module, die Sie möglicherweise mit anderen python-Installationen erhalten. Wir müssen weiterhin **PIP**, den Standardpaket-Manager für python und **venv**installieren, das Standardmodul, das zum Erstellen und Verwalten von virtuellen Lightweight-Umgebungen verwendet wird.  

1. Vergewissern Sie sich, dass Python3 bereits installiert ist, indem Sie das Ubuntu `python3 --version`-Terminal öffnen und Folgendes eingeben:. Dies sollte Ihre python-Versionsnummer zurückgeben. Wenn Sie Ihre Python-Version aktualisieren müssen, aktualisieren Sie zunächst die Ubuntu-Version, indem `sudo apt update && sudo apt upgrade`Sie Folgendes eingeben:, `sudo apt upgrade python3`und aktualisieren Sie anschließend python mithilfe von.

2. Installieren Sie **PIP** , indem `sudo apt install python3-pip`Sie Folgendes eingeben:. PIP ermöglicht Ihnen, zusätzliche Pakete zu installieren und zu verwalten, die nicht Teil der python-Standardbibliothek sind.

3. Installieren Sie **venv** , indem `sudo apt install python3-venv`Sie Folgendes eingeben:.

## <a name="create-a-virtual-environment"></a>Erstellen einer virtuellen Umgebung

Die Verwendung virtueller Umgebungen wird als bewährte Vorgehensweise für python-Entwicklungsprojekte empfohlen. Durch das Erstellen einer virtuellen Umgebung können Sie Ihre Projekt Tools isolieren und Versions Konflikte mit Tools für die anderen Projekte vermeiden. Beispielsweise können Sie ein älteres Webprojekt verwalten, das das Django 1,2-Webframework erfordert, aber dann wird ein spannendes neues Projekt mit Django 2,2 verwendet. Wenn Sie Django Global außerhalb einer virtuellen Umgebung aktualisieren, können Sie später einige Versions Probleme beheben. Mit virtuellen Umgebungen können Sie nicht nur versehentliche Versions Verwaltungs Konflikte verhindern, sondern auch Pakete ohne Administratorrechte installieren und verwalten.

1. Öffnen Sie das Terminal, und erstellen Sie in Ihrem *HelloWorld* -Projektordner den folgenden Befehl, um eine virtuelle Umgebung mit dem Namen `python3 -m venv .venv` **. venv**: zu erstellen.

2. Geben Sie Folgendes ein, um die virtuelle `source .venv/bin/activate`Umgebung zu aktivieren:. Wenn es funktioniert hat, sollte vor der Eingabeaufforderung **(. venv)** angezeigt werden. Sie verfügen jetzt über eine eigenständige Umgebung, die zum Schreiben von Code und Installieren von Paketen bereit ist. Wenn Sie mit der virtuellen Umgebung fertig sind, geben Sie den folgenden Befehl ein, um `deactivate`Sie zu deaktivieren:.

    ![Erstellen einer virtuellen Umgebung](../../images/wsl-venv.png)

> [!TIP]
> Es wird empfohlen, die virtuelle Umgebung in dem Verzeichnis zu erstellen, in dem Sie das Projekt erstellen möchten. Da jedes Projekt über ein eigenes separates Verzeichnis verfügen sollte, verfügt jedes Projekt über seine eigene virtuelle Umgebung, sodass keine eindeutige Benennung erforderlich ist. Unser Vorschlag ist, dass Sie "Name **. venv** " verwenden, um die python-Konvention zu befolgen. Einige Tools (z. b. pipv) werden auch standardmäßig auf diesen Namen eingestellt, wenn Sie in Ihrem Projektverzeichnis installieren. Sie möchten nicht " **. env** " verwenden, da dies mit Konflikten mit Umgebungsvariablen-Definitions Dateien in Konflikt steht. Im Allgemeinen empfehlen wir nicht-Punkt-führende Namen, da Sie nicht ständig daran `ls` erinnert werden müssen, dass das Verzeichnis vorhanden ist. Außerdem wird empfohlen, " **. venv** " zu ihrer gitignore-Datei hinzuzufügen. (Im folgenden finden Sie [die githunore-Standardvorlage von GitHub für python](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) .) Weitere Informationen zum Arbeiten mit virtuellen Umgebungen in vs Code finden Sie unter [Verwenden von python-Umgebungen in vs Code](https://code.visualstudio.com/docs/python/environments).

## <a name="open-a-wsl---remote-window"></a>Öffnen eines WSL-Remote Fensters

VS Code verwendet die Remote-WSL-Erweiterung (zuvor installiert), um Ihr Linux-Subsystem als Remote Server zu behandeln. Dies ermöglicht Ihnen die Verwendung von WSL als integrierte Entwicklungsumgebung. [Weitere Informationen](https://code.visualstudio.com/docs/remote/wsl). 

1. Öffnen Sie den Projektordner in vs Code aus Ihrem Ubuntu-Terminal, `code .` indem Sie Folgendes eingeben: (die "." weist vs Code an, den aktuellen Ordner zu öffnen).

2. Eine Sicherheitswarnung wird von Windows Defender angezeigt, und wählen Sie "Zugriff zulassen" aus. Nachdem vs Code geöffnet wurde, sollte der Host Indikator für Remote Verbindungen in der unteren linken Ecke angezeigt werden, in dem Sie wissen, dass Sie **WSL bearbeiten: Ubuntu-18,04**.

    ![Host Indikator für vs Code Remote Verbindung](../../images/wsl-remote-extension.png)

3. Schließen Sie das Ubuntu-Terminal. In Zukunft wird das in vs Code integrierte WSL-Terminal verwendet.

4. Öffnen Sie das WSL-Terminal in vs Code, indem Sie **Strg + "** (mit dem Graviszeichen-Zeichen) drücken oder**Terminal** **anzeigen** > auswählen. Dadurch wird die Befehlszeile bash (WSL) geöffnet, die für den Projektordner Pfad geöffnet ist, den Sie in Ihrem Ubuntu-Terminal erstellt haben.

    ![VS Code mit dem WSL-Terminal](../../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Installieren der Microsoft python-Erweiterung

Sie müssen alle vs Code Erweiterungen für die Remote-WSL-Datei installieren. Erweiterungen, die bereits lokal auf vs Code installiert sind, sind nicht automatisch verfügbar. [Weitere Informationen](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions).

1. Öffnen Sie das Fenster vs Code Erweiterungen, indem Sie **STRG + UMSCHALT + X** eingeben (oder verwenden Sie das Menü, um zu **Anzeige** > **Erweiterungen**zu navigieren).

2. Geben Sie im Feld Top **Search Extensions in Marketplace** Folgendes ein:  **Python**.

3. Suchen Sie die **python-Erweiterung (MS-python. python) nach Microsoft** , und wählen Sie die grüne Schaltfläche **Installieren** aus.

4. Nachdem die Installation der Erweiterung abgeschlossen ist, müssen Sie die blaue Schaltfläche zum erneuten **Laden erforderlich** auswählen. Dadurch wird vs Code neu geladen und ein **WSL angezeigt: Der Abschnitt "Ubuntu-** 18,04-installiert" im Fenster "vs Code Erweiterungen" zeigt an, dass Sie die python-Erweiterung installiert haben.

## <a name="run-a-simple-python-program"></a>Ausführen eines einfachen python-Programms

Python ist eine interpretierte Sprache und unterstützt verschiedene Arten von interpretoren (Python2, Anaconda, pypy usw.). VS Code sollte standardmäßig den Interpreter haben, der dem Projekt zugeordnet ist. Wenn Sie einen Grund für die Änderung haben, wählen Sie den derzeit in der blauen Leiste angezeigten Interpreter am unteren Rand des vs Code Fensters aus, oder öffnen Sie die **Befehls Palette** (STRG + UMSCHALT + P) **, und geben Sie den Befehl python ein: Wählen Sie**Interpreter aus. Dadurch wird eine Liste der derzeit installierten Python-Interpreter angezeigt. [Erfahren Sie mehr über das Konfigurieren von python-Umgebungen](https://code.visualstudio.com/docs/python/environments).

Erstellen Sie ein einfaches Python-Programm als Test und führen Sie es aus. Stellen Sie sicher, dass der richtige Python-Interpreter ausgewählt ist.

1. Öffnen Sie das Fenster vs Code Datei-Explorer, indem Sie **STRG + UMSCHALT + E** eingeben (oder verwenden Sie das Menü, um zum **Ansicht** > -**Explorer**zu navigieren).

2. Wenn Sie nicht bereits geöffnet ist, öffnen Sie das integrierte WSL-Terminal durch Drücken von **STRG + UMSCHALT + "** , und stellen Sie sicher, dass Ihr **HelloWorld** -python-Projektordner ausgewählt ist.

3. Erstellen Sie eine python-Datei, `touch test.py`indem Sie Folgendes eingeben: Sie sollten sehen, dass die soeben erstellte Datei in Ihrem Explorer-Fenster unter den Ordnern. venv und. vscode bereits in Ihrem Projektverzeichnis angezeigt wird.

4. Wählen Sie die **Test.py** -Datei aus, die Sie soeben im Explorer-Fenster erstellt haben, um Sie in vs Code zu öffnen. Da die py-Datei in unserem Dateinamen vs Code, dass es sich hierbei um eine python-Datei handelt, wählt die zuvor geladene python-Erweiterung automatisch einen Python-Interpreter aus, der am unteren Rand des vs Code Fensters angezeigt wird.

    ![Python-Interpreter in vs Code auswählen](../../images/interpreterselection.gif)

5. Fügen Sie diesen Python-Code in die Test.py-Datei ein, und speichern Sie die Datei (STRG + S): 

    ```python
    print("Hello World")
    ```

6. Um das Python-Programm "Hallo Welt" auszuführen, das Sie gerade erstellt haben, wählen Sie die Datei **Test.py** im Fenster vs Code Explorer aus, und klicken Sie dann mit der rechten Maustaste auf die Datei, um ein Menü mit Optionen anzuzeigen. Wählen Sie **python-Datei im Terminal ausführen aus**. Geben Sie `python test.py` alternativ in Ihrem integrierten WSL-Terminalfenster Folgendes ein, um das Programm "Hallo Welt" auszuführen. Der Python-Interpreter druckt "Hallo Welt" in Ihrem Terminalfenster.

Gratul. Sie sind so eingerichtet, dass Sie Python-Programme erstellen und ausführen können. Nun versuchen wir, eine Hallo Welt-App mit zwei der beliebtesten python-Webframe Works zu erstellen: Flask und Django.

## <a name="hello-world-tutorial-for-flask"></a>Hallo Welt Tutorial für Flask

[Flask](http://flask.pocoo.org/) ist ein Webanwendungs Framework für python. In diesem kurzen Tutorial erstellen Sie eine kleine "Hallo Welt" Flask-APP, die vs Code und WSL verwendet.

1. Öffnen Sie Ubuntu 18,04 (Ihre WSL-Befehlszeile), indem  Sie zu Ihrem Startmenü wechseln (unten links Windows-Symbol), und geben Sie Folgendes ein: "Ubuntu 18,04".

2. Erstellen Sie ein Verzeichnis für Ihr Projekt `mkdir HelloWorld-Flask`:, `cd HelloWorld-Flask` und geben Sie dann das Verzeichnis ein.

3. Erstellen Sie eine virtuelle Umgebung, um Ihre Projekt Tools zu installieren:`python3 -m venv .venv`

4. Öffnen Sie das Projekt **HelloWorld-Flask** in vs Code, indem Sie den folgenden Befehl eingeben:`code .`

5. Öffnen Sie in vs Code das integrierte WSL-Terminal (auch bash), indem Sie **STRG + UMSCHALT + "** drücken (Ihr **HelloWorld-Flask-** Projektordner sollte bereits ausgewählt sein). *Schließen Sie Ihre Ubuntu-Befehlszeile, da wir im WSL-Terminal arbeiten, das in vs Code in der Zukunft integriert ist.*

6. Aktivieren Sie die virtuelle Umgebung, die Sie in Schritt #3 mithilfe Ihres bash-Terminals in `source .venv/bin/activate`vs Code erstellt haben:. Wenn es funktioniert hat, sollte vor der Eingabeaufforderung (. venv) angezeigt werden.

7. Installieren Sie Flask in der virtuellen Umgebung, indem `python3 -m pip install flask`Sie Folgendes eingeben:. Vergewissern Sie sich, dass es installiert ist `python3 -m flask --version`, indem Sie Folgendes eingeben:

8. Erstellen Sie eine neue Datei für Ihren Python-Code:`touch app.py`

9. Öffnen Sie die Datei **app.py** im Datei-Explorer vs Code`Ctrl+Shift+E`(und wählen Sie dann Ihre APP.py-Datei aus). Dadurch wird die python-Erweiterung aktiviert, um einen Interpreter auszuwählen. Der Standardwert ist **python 3.6.8 in 64-Bit (". venv": venv)** . Beachten Sie, dass auch die virtuelle Umgebung erkannt wurde.

    ![Aktivierte virtuelle Umgebung](../../images/virtual-environment.png)

10. Fügen Sie in **app.py**Code zum Importieren von Flask hinzu, und erstellen Sie eine Instanz des Flask-Objekts:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. Fügen Sie in **app.py**außerdem eine Funktion hinzu, die den Inhalt zurückgibt, in diesem Fall eine einfache Zeichenfolge. Verwenden Sie den " **app. Route** "-Decorator von Flask, um die URL-Route "/" zu dieser Funktion zuzuordnen:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Je nachdem, wie viele verschiedene Routen Sie derselben Funktion zuordnen möchten, können Sie mehrere Decorator-Funktionen für dieselbe Funktion verwenden.

12. Speichern Sie die Datei **app.py** (**STRG + S**).

13. Führen Sie die APP im Terminal aus, indem Sie den folgenden Befehl eingeben:

    ```python
    python3 -m flask run
    ```

    Dadurch wird der Flask Development Server ausgeführt. Der Entwicklungs Server sucht standardmäßig nach **app.py** . Wenn Sie Flask ausführen, sollte eine Ausgabe ähnlich der folgenden angezeigt werden:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Öffnen Sie den Standard Webbrowser der gerenderten Seite, und **Klicken Sie** auf die http://127.0.0.1:5000/ URL im Terminal. In Ihrem Browser sollte die folgende Meldung angezeigt werden:

    ![Hallo, Flask!](../../images/hello-flask.png)

15. Beachten Sie, dass beim Besuch einer URL wie "/" im Debug-Terminal eine Meldung angezeigt wird, die die HTTP-Anforderung anzeigt:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Mit **STRG + C** im Terminal wird die APP beendet.

> [!TIP]
> Wenn Sie einen anderen Dateinamen als **app.py**verwenden möchten, z. b. **Program.py**, definieren Sie eine Umgebungsvariable mit dem Namen **FLASK_APP** , und legen Sie Ihren Wert auf die ausgewählte Datei fest. Der Entwicklungs Server von Flask verwendet dann den Wert von **FLASK_APP** anstelle der Standarddatei **app.py**. Weitere Informationen finden Sie in [der Dokumentation zur Befehlszeilenschnittstelle von Flask](http://flask.pocoo.org/docs/1.0/cli/).

Herzlichen Glückwunsch, Sie haben eine Flask-Webanwendung mit Visual Studio Code und Windows-Subsystem für Linux erstellt! Ein ausführlichere Tutorial mit vs Code und Flask finden Sie unter [Flask Tutorial in Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Hallo Welt Tutorial für Django

[Django](https://www.djangoproject.com) ist ein Webanwendungs Framework für python. In diesem kurzen Tutorial erstellen Sie eine kleine Django-App mit dem Hallo Welt "", die vs Code und WSL verwendet.

1. Öffnen Sie Ubuntu 18,04 (Ihre WSL-Befehlszeile), indem  Sie zu Ihrem Startmenü wechseln (unten links Windows-Symbol), und geben Sie Folgendes ein: "Ubuntu 18,04".

2. Erstellen Sie ein Verzeichnis für Ihr Projekt `mkdir HelloWorld-Django`:, `cd HelloWorld-Django` und geben Sie dann das Verzeichnis ein.

3. Erstellen Sie eine virtuelle Umgebung, um Ihre Projekt Tools zu installieren:`python3 -m venv .venv`

4. Öffnen Sie das Projekt **HelloWorld-DJango** in vs Code, indem Sie den folgenden Befehl eingeben:`code .`

5. Öffnen Sie in vs Code das integrierte WSL-Terminal (bash), indem Sie **STRG + UMSCHALT + '** (Ihr **HelloWorld-Django-** Projektordner sollte bereits ausgewählt sein). *Schließen Sie Ihre Ubuntu-Befehlszeile, da wir im WSL-Terminal arbeiten, das in vs Code in der Zukunft integriert ist.*

6. Aktivieren Sie die virtuelle Umgebung, die Sie in Schritt #3 mithilfe Ihres bash-Terminals in `source .venv/bin/activate`vs Code erstellt haben:. Wenn es funktioniert hat, sollte vor der Eingabeaufforderung (. venv) angezeigt werden.

7. Installieren Sie django in der virtuellen Umgebung mit dem folgenden `python3 -m pip install django`Befehl:. Vergewissern Sie sich, dass es installiert ist `python3 -m django --version`, indem Sie Folgendes eingeben:

8. Führen Sie als nächstes den folgenden Befehl aus, um das Django-Projekt zu erstellen:

    ```bash
    django-admin startproject web_project .
    ```

    Der `startproject` Befehl setzt (bei Verwendung von `.` am Ende) voraus, dass der aktuelle Ordner Ihr Projektordner ist, und erstellt darin Folgendes:

    - `manage.py`: Das Befehlszeilen-Verwaltungs Programm Django für das Projekt. Sie führen mithilfe `python manage.py <command> [options]`von administrative Befehle für das Projekt aus.

    - Ein Unterordner mit `web_project`dem Namen, der die folgenden Dateien enthält:
        - `__init__.py`: eine leere Datei, die python mitteilt, dass es sich bei diesem Ordner um ein Python-Paket handelt.
        - `wsgi.py`: ein Einstiegspunkt für wsgi-kompatible Webserver zur Bereitstellung Ihres Projekts. Sie lassen diese Datei in der Regel unverändert, da Sie die Hooks für produktionsweb Server bereitstellt.
        - `settings.py`: enthält Einstellungen für das Django-Projekt, das Sie im Rahmen der Entwicklung einer Web-App ändern.
        - `urls.py`: enthält ein Inhaltsverzeichnis für das Django-Projekt, das Sie auch im Verlauf der Entwicklung ändern.

9. Um das Django-Projekt zu überprüfen, starten Sie den Entwicklungs Server `python3 manage.py runserver`von Django mit dem Befehl. Der Server wird auf dem Standardport 8000 ausgeführt, und es sollte eine Ausgabe wie die folgende Ausgabe im Terminalfenster angezeigt werden:

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    Wenn Sie den Server erstmalig ausführen, wird in der Datei `db.sqlite3`eine SQLite-Standarddatenbank erstellt, die für Entwicklungszwecke vorgesehen ist, aber in der Produktionsumgebung für Web-Apps mit geringem Volumen verwendet werden kann. Außerdem ist der integrierte Webserver von Django *nur* für lokale Entwicklungszwecke vorgesehen. Wenn Sie jedoch auf einem Webhost bereitstellen, verwendet Django stattdessen den Webserver des Hosts. Das `wsgi.py` Modul im Django-Projekt übernimmt die Einbindung in die Produktionsserver.

    Wenn Sie einen anderen Port als den Standardwert 8000 verwenden möchten, geben Sie die Portnummer in der Befehlszeile an, `python3 manage.py runserver 5000`z. b.

10. `Ctrl+click`die `http://127.0.0.1:8000/` URL im Terminal Ausgabefenster, um den Standardbrowser mit dieser Adresse zu öffnen. Wenn Django ordnungsgemäß installiert ist und das Projekt gültig ist, wird eine Standardseite angezeigt. Im Fenster vs Code Terminal Ausgabe wird auch das Server Protokoll angezeigt.

11. Wenn Sie fertig sind, schließen Sie das Browserfenster, und beenden Sie den Server `Ctrl+C` in vs Code mithilfe von, wie im Fenster Terminal Ausgabe angegeben.

12. Wenn Sie nun eine Django- `startapp` app erstellen möchten, führen Sie den Befehl des Verwaltungs Programms im Projektordner (in dem `manage.py` sich befindet) aus:

    ```bash
    python3 manage.py startapp hello
    ```

    Der Befehl erstellt einen Ordner mit `hello` dem Namen, der eine Reihe von Code Dateien und einen Unterordner enthält. Dabei arbeiten Sie häufig mit `views.py` (das die Funktionen enthält, die Seiten in Ihrer Web-App definieren) und `models.py` (die Klassen enthalten, die ihre Datenobjekte definieren). Der `migrations` Ordner wird vom Verwaltungs Programm von Django zum Verwalten von Daten Bank Versionen verwendet, wie weiter unten in diesem Tutorial erläutert. Es gibt auch die Dateien `apps.py` (app-Konfiguration) `admin.py` , (zum Erstellen einer administrativen Oberfläche) `tests.py` und (für Tests), die hier nicht behandelt werden.

13. Ändern `hello/views.py` Sie den Code entsprechend dem folgenden Code, der eine einzelne Ansicht für die Startseite der App erstellt:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Erstellen Sie eine Datei `hello/urls.py`mit dem unten stehenden Inhalt. In `urls.py` der Datei geben Sie Muster an, um unterschiedliche URLs an die entsprechenden Ansichten weiterzuleiten. Der folgende Code enthält eine Route zum Zuordnen der Stamm-URL der APP`""`() zu `views.home` der Funktion `hello/views.py`, die Sie soeben hinzugefügt haben:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. Der `web_project` Ordner enthält auch eine `urls.py` Datei, in der das URL-Routing tatsächlich behandelt wird. Öffnen `web_project/urls.py` Sie, und ändern Sie ihn so, dass er dem folgenden Code entspricht (Sie können die aufschlussreichen Kommentare bei Bedarf beibehalten). Dieser Code Ruft die App mithilfe `hello/urls.py` `django.urls.include`von ab, die die in der APP enthaltenen Routen der APP beibehält. Diese Trennung ist hilfreich, wenn ein Projekt mehrere apps enthält.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Speichern Sie alle geänderten Dateien.

17. Führen Sie im vs Code Terminal den Entwicklungs Server mit `python manage.py runserver` aus, und öffnen Sie einen Browser, `http://127.0.0.1:8000/` um eine Seite anzuzeigen, die "Hello, Django" rendert.

Herzlichen Glückwunsch, Sie haben eine Django-Webanwendung mit vs Code und Windows-Subsystem für Linux erstellt! Ein ausführlichere Lernprogramm zur Verwendung von vs Code und Django finden Sie unter [Django-Tutorial in Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Python-Tutorial mit vs Code](https://code.visualstudio.com/docs/python/python-tutorial): Ein Intro-Tutorial zum vs Code als python-Umgebung, in erster Linie zum Bearbeiten, ausführen und Debuggen von Code.
- [Git-Unterstützung in vs Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): Erfahren Sie, wie Sie die Grundlagen der git-Versionskontrolle in vs Code.  
- [Erfahren Sie mehr über Updates, die demnächst mit WSL 2!](https://docs.microsoft.com/windows/wsl/wsl2-index)verfügbar sind: Diese neue Version ändert die Interaktion von Linux-Distributionen mit Windows, das Erhöhen der Dateisystem Leistung und das Hinzufügen von vollständigen Systemaufrufs
- [Arbeiten mit mehreren Linux-Distributionen unter Windows](https://docs.microsoft.com/windows/wsl/wsl-config): Erfahren Sie, wie Sie mehrere verschiedene Linux-Distributionen auf Ihrem Windows-Computer verwalten.
