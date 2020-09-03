---
title: Erste Schritte bei der Verwendung von Python unter Windows für Anfänger
description: Eine Anleitung für den Einstieg in die Verwendung von Python unter Windows ohne Vorkenntnisse.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Python lernen, Python unter Windows für Anfänger, Python aus dem Microsoft Store installieren, Python mit VS Code, pygame unter Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 076c62658431ba75bdbd7f385ced86f27fb2f7d0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174144"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Erste Schritte bei der Verwendung von Python unter Windows für Anfänger

Die folgende ausführliche Anleitung richtet sich an Einsteiger, die sich mit Python unter Windows 10 vertraut machen möchten.

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Für Einsteiger, die noch nicht mit Python vertraut sind, wird empfohlen, [Python über den Microsoft Store zu installieren](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). Bei der Installation über den Microsoft Store wird der grundlegende Python3-Interpreter verwendet, jedoch werden zusätzlich zur Bereitstellung automatischer Updates Ihre PATH-Einstellungen für den aktuellen Benutzer eingerichtet (sodass kein Administratorzugriff erforderlich ist). Dies ist besonders hilfreich, wenn Sie sich in einer Schulungsumgebung oder in einer Organisation befinden, die Berechtigungen oder den administrativen Zugriff auf Ihren Computer einschränkt.

Wenn Sie Python unter Windows für **Webentwicklung** verwenden, wird ein anderes Setup für Ihre Entwicklungsumgebung empfohlen. Anstelle einer direkten Installation unter Windows empfiehlt es sich, Python über das Windows-Subsystem für Linux zu installieren und zu verwenden. Hilfe finden Sie unter: [Erste Schritte bei der Verwendung von Python für die Webentwicklung unter Windows](./web-frameworks.md). Wenn Sie gängige Aufgaben im Betriebssystem automatisieren möchten, finden Sie entsprechende Informationen in unserer Anleitung [Erste Schritte bei der Verwendung von Python unter Windows für Skripts und Automatisierung](./scripting.md). Für einige erweiterte Szenarien (wenn es z. B. erforderlich ist, auf installierte Python-Dateien zuzugreifen oder sie zu ändern, Kopien von Binärdateien zu erstellen oder Python-DLLs direkt zu erstellen) sollten Sie ein bestimmtes Python-Release direkt unter [python.org](https://www.python.org/downloads/) herunterladen oder eine [Alternative](https://www.python.org/download/alternatives) installieren, z. B. Anaconda, Jython, PyPy, WinPython, IronPython usw. Dies wird nur empfohlen, wenn Sie ein versierter Python-Programmierer sind und bestimmte Gründe für die Auswahl einer alternativen Implementierung haben.

## <a name="install-python"></a>Installieren von Python

So installieren Sie Python mit dem Microsoft Store:

1. Wechseln Sie zum Menü **Start** (unteres linkes Windows-Symbol), geben Sie „Microsoft Store“ ein, und wählen Sie den Link zum Öffnen des Stores aus.

2. Sobald der Store geöffnet ist, wählen Sie im oberen rechten Menü **Suchen** aus, und geben Sie „Python“ ein. Öffnen Sie „Python 3.7“ in den Ergebnissen unter „Apps“. Wählen Sie **Abrufen** aus.

3. Nachdem Python den Download- und Installationsprozess abgeschlossen hat, öffnen Sie Windows PowerShell über das Menü **Start** (unteres linkes Windows-Symbol). Wenn PowerShell geöffnet ist, geben Sie `Python --version` ein, um zu bestätigen, dass Python 3 auf Ihrem Computer installiert wurde.

4. Die Microsoft Store-Installation von Python umfasst **PIP**, den standardmäßigen Paket-Manager. Über PIP können Sie zusätzliche Pakete installieren und verwalten, die nicht Teil der Python-Standardbibliothek sind. Geben Sie `pip --version` ein, um zu bestätigen, dass Ihnen auch PIP zum Installieren und Verwalten von Paketen zur Verfügung steht.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Wenn Sie VS Code als Text-Editor/integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) verwenden, können Sie [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (eine Codevervollständigungshilfe), [Linting](https://code.visualstudio.com/docs/python/linting) (hilft, Fehler in Ihrem Code zu vermeiden), [Debugunterstützung](https://code.visualstudio.com/docs/python/debugging) (hilft Ihnen, Fehler in Ihrem Code nach der Ausführung zu finden), [Codeausschnitte](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (Vorlagen für kleine wiederverwendbare Codeblöcke) und [Komponententests](https://code.visualstudio.com/docs/python/unit-testing) (Testen der Codeschnittstelle mit unterschiedlichen Eingabetypen) nutzen.

VS Code enthält auch ein [integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal), mit dem Sie eine Python-Befehlszeile mit der Windows-Eingabeaufforderung, PowerShell oder einem anderen Tool Ihrer Wahl öffnen können. Dadurch wird ein nahtloser Workflow zwischen dem Code-Editor und der Befehlszeile eingerichtet.

1. Wenn Sie VS Code installieren möchten, laden Sie das Tool für Windows herunter: [https://code.visualstudio.com](https://code.visualstudio.com).

2. Nachdem VS Code installiert wurde, müssen Sie auch die Python-Erweiterung installieren. Zum Installieren der Python-Erweiterung können Sie den [VS Code Marketplace-Link](https://marketplace.visualstudio.com/items?itemName=ms-python.python) auswählen oder VS Code öffnen und im Erweiterungsmenü (STRG+UMSCHALT+X) nach **Python** suchen.

3. Python ist eine interpretierte Sprache, und zum Ausführen von Python-Code müssen Sie VS Code mitteilen, welcher Interpreter verwendet werden soll. Wir empfehlen die Verwendung von Python 3.7, sofern Sie keinen besonderen Grund für eine andere Auswahl haben. Wählen Sie nach der Installation der Python-Erweiterung einen Python 3-Interpreter aus, indem Sie die **Befehlspalette** öffnen (STRG+UMSCHALT+P) und mit der Eingabe des Befehls **Python: Select Interpreter** beginnen, um nach dem Befehl zu suchen und ihn auszuwählen. Sie können auch die Option **Select Python Environment** (Python-Umgebung auswählen) auf der unteren Statusleiste verwenden (möglicherweise wird dort bereits ein ausgewählter Interpreter angezeigt). Mit dem Befehl wird eine Liste der verfügbaren Interpreter angezeigt, die VS Code automatisch finden kann, einschließlich virtueller Umgebungen. Wenn der gewünschte Interpreter nicht angezeigt wird, lesen Sie unter [Konfigurieren von Python-Umgebungen](https://code.visualstudio.com/docs/python/environments) nach.

    ![Auswählen des Python-Interpreters in VS Code](../images/interpreterselection.gif)

4. Zum Öffnen des Terminals in VS Code wählen Sie **Anzeigen** > **Terminal** aus, oder verwenden Sie alternativ die Tastenkombination **STRG+`** (mit dem Graviszeichen). Das Standardterminal ist PowerShell.

5. Öffnen Sie Python in Ihrem VS Code-Terminal, indem Sie den Befehl `python` eingeben.

6. Probieren Sie den Python-Interpreter aus, indem Sie Folgendes eingeben: `print("Hello World")`. Python gibt Ihre Anweisung „Hello World“ zurück.

    ![Python-Befehlszeile in VS Code](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Installieren von Git (optional)

Wenn Sie beabsichtigen, zusammen mit anderen an Ihrem Python-Code zu arbeiten oder das Projekt an einem Open-Source-Standort (wie GitHub) zu hosten, unterstützt VS Code die [Versionskontrolle mit Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). Auf der Registerkarte „Quellcodeverwaltung“ in VS Code werden alle Änderungen nachverfolgt und gängige Git-Befehle („Add“, „Commit“, „Push“, „Pull“) direkt in die Benutzeroberfläche integriert. Sie müssen zunächst Git installieren, um den Bereich „Quellcodeverwaltung“ zu aktivieren.

1. Laden Sie Git für Windows von der [git-scm-Website](https://git-scm.com/download/win) herunter, und installieren Sie Git.

2. Ein Installations-Assistent ist enthalten, der Ihnen eine Reihe von Fragen zu den Einstellungen für die Git-Installation stellt. Sie sollten alle Standardeinstellungen verwenden, sofern kein bestimmter Grund vorliegt, etwas zu ändern.

3. Wenn Sie noch nie zuvor mit Git gearbeitet haben, können Sie [GitHub-Leitfäden](https://guides.github.com/) für den Einstieg nutzen.

## <a name="hello-world-tutorial-for-some-python-basics"></a>„Hallo Welt“-Tutorial für einige Python-Grundlagen

Python ist laut Erfinder Guido van Rossum eine „Programmiersprache auf hoher Ebene mit der Lesbarkeit von Code als zentrale Entwurfsphilosophie und einer Syntax, die Programmierern das Ausdrücken von Konzepten in wenigen Codezeilen ermöglicht“.

Python ist eine interpretierte Programmiersprache. Im Gegensatz zu kompilierten Sprachen, in denen der geschriebene Code in Computercode übersetzt werden muss, damit er vom Prozessor des Computers ausgeführt werden kann, wird Python-Code direkt an einen Interpreter übergeben und ausgeführt. Sie geben einfach Ihren Code ein und führen ihn aus. Probieren Sie es aus!

1. Geben Sie bei geöffneter PowerShell-Befehlszeile `python` ein, um den Python 3-Interpreter auszuführen. (Bei einigen Anleitungen wird die Verwendung des Befehls `py` oder `python3` bevorzugt, die ebenfalls funktionieren sollten.) Sie wissen, dass Sie erfolgreich waren, da als Eingabeaufforderung „>>>“ (drei Größer-als-Symbole) angezeigt wird.

2. Es gibt mehrere integrierte Methoden, mit denen Sie Änderungen an Zeichenfolgen in Python vornehmen können. Erstellen Sie eine Variable mit `variable = 'Hello World!'`. Drücken Sie die EINGABETASTE, um eine neue Zeile zu beginnen.

3. Drucken Sie Ihre Variable mit `print(variable)` aus. Dadurch wird der Text „Hello World!“ angezeigt.

4. Ermitteln Sie die Länge Ihrer Zeichenfolgenvariablen (die Anzahl der verwendeten Zeichen) mit `len(variable)`. Dadurch wird angezeigt, dass 12 Zeichen verwendet werden. (Beachten Sie, dass das Leerzeichen für die Gesamtlänge mitgezählt wird.)

5. Konvertieren Sie die Zeichenfolgenvariable in Großbuchstaben: `variable.upper()`. Konvertieren Sie die Zeichenfolgenvariable nun in Kleinbuchstaben: `variable.lower()`.

6. Zählen Sie, wie oft der Buchstabe „l“ in der Zeichenfolgenvariable enthalten ist: `variable.count("l")`.

7. Suchen Sie in der Zeichenfolgenvariable nach einem bestimmten Zeichen – dem Ausrufezeichen: `variable.find("!")`. Dadurch wird angezeigt, dass sich das Ausrufezeichen an 11. Stelle in der Zeichenfolge befindet.

8. Ersetzen Sie das Ausrufezeichen durch ein Fragezeichen: `variable.replace("!", "?")`.

9. Um Python zu beenden, können Sie `exit()`, `quit()`oder „STRG+Z“ eingeben.

![PowerShell-Screenshot dieses Tutorials](../images/hello-world-basics.png)

Wir hoffen, dass Sie Spaß mit einigen der integrierten Methoden von Python zum Ändern von Zeichenfolgen hatten. Versuchen Sie nun, eine Python-Programmdatei zu erstellen und mit VS Code auszuführen.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>„Hallo Welt“-Tutorial zur Verwendung von Python mit VS Code

Das VS Code-Team hat ein hervorragendes Tutorial zu den [ersten Schritten mit Python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) erstellt, das Sie durch das Erstellen eines „Hallo Welt“-Programms mit Python, das Ausführen der Programmdatei, das Konfigurieren des Debuggers und das Installieren von Paketen wie *matplotlib* und *numpy* führt. Mit diesen Paketen können Sie grafische Darstellungen in einer virtuellen Umgebung erstellen.

1. Öffnen Sie PowerShell, und erstellen Sie einen leeren Ordner mit dem Namen „hello“. Navigieren Sie zu diesem Ordner, und öffnen Sie ihn in VS Code:

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. Nach dem Öffnen von VS Code wird der neue Ordner *hello* im linken **Explorer**-Fenster angezeigt. Öffnen Sie im unteren Bereich von VS Code ein Befehlszeilenfenster, indem Sie **STRG+`** (Graviszeichen) drücken oder **Ansicht** > **Terminal** auswählen. Wenn Sie VS Code in einem Ordner starten, wird dieser Ordner automatisch Ihr „Arbeitsbereich“. VS Code speichert die spezifischen Einstellungen für diesen Arbeitsbereich, die nicht zu den global gespeicherten Benutzereinstellungen gehören, in der Datei „.vscode/settings.json“.

3. Fahren Sie mit dem Tutorial in der Dokumentation zu VS Code fort: [Create a Python Hello World source code file](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file) (Erstellen einer „Hallo Welt“-Quellcodedatei in Python).

## <a name="create-a-simple-game-with-pygame"></a>Erstellen eines einfachen Spiels mit Pygame

![Pygame mit ausgeführtem Beispielspiel](../images/pygame-shmup.jpg)

Pygame ist ein beliebtes Python-Paket für das Entwickeln von Spielen, das Schüler und Studenten das Programmieren auf spaßige Weise beibringen soll. Pygame zeigt Grafiken in einem neuen Fenster an. Aus diesem Grund funktioniert es nicht bei Verwendung der WSL-Methode an der Befehlszeile. Wenn Sie Python jedoch wie in diesem Tutorial beschrieben über den Microsoft Store installiert haben, funktioniert es problemlos.

1. Nachdem Sie Python installiert haben, installieren Sie pygame über die Befehlszeile (oder das Terminal in VS Code), indem Sie `python -m pip install -U pygame --user` eingeben.

2. Testen Sie die Installation, indem Sie ein Beispielspiel ausführen: `python -m pygame.examples.aliens`.

3. Wenn alles funktioniert, öffnet das Spiel ein Fenster. Schließen Sie das Fenster, wenn Sie genug gespielt haben.

So beginnen Sie mit dem Erstellen Ihres eigenen Spiels

1. Öffnen Sie PowerShell (oder die Windows-Eingabeaufforderung), und erstellen Sie einen leeren Ordner mit dem Namen „bounce“. Navigieren Sie zu diesem Ordner, und erstellen Sie darin eine Datei mit dem Namen „bounce.py“. Öffnen Sie den Ordner in VS Code:

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. Geben Sie mithilfe VS Code den folgenden Python-Code ein (oder kopieren Sie ihn, und fügen Sie ihn ein):

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. Speichern Sie die Datei unter: `bounce.py`.

4. Führen Sie die Datei am PowerShell-Terminal aus, indem Sie Folgendes eingeben: `python bounce.py`.

    ![Pygame mit dem nächsten großen Erfolg](../images/pygame.jpg)

Versuchen Sie, einige der Zahlen anzupassen, um zu sehen, welche Auswirkungen sie auf die hüpfende Kugel haben.

Weitere Informationen zum Erstellen von Spielen mit pygame finden Sie unter [pygame.org](http://www.pygame.org).

## <a name="resources-for-continued-learning"></a>Ressourcen für das weitere Lernen

Wir empfehlen Ihnen die folgenden Ressourcen, um mehr über die Python-Entwicklung unter Windows zu erfahren.

### <a name="online-courses-for-learning-python"></a>Onlinekurse zum Erlernen von Python

- [Einführung in Python auf Microsoft Learn:](/learn/modules/intro-to-python/) Testen Sie die interaktive Microsoft Learn-Plattform, und gewinnen Sie Erfahrungspunkte für den Abschluss dieses Moduls mit Grundlagen zum Schreiben von einfachem Python-Code, zum Deklarieren von Variablen und zum Arbeiten mit Konsoleneingaben und -ausgaben. Die interaktive Sandboxumgebung ist ein idealer Ausgangspunkt für Benutzer, die noch keine Python-Entwicklungsumgebung eingerichtet haben.

- [Python bei Pluralsight: 8 Kurse, 29 Stunden:](https://app.pluralsight.com/paths/skills/python) Der Python-Lernpfad bei Pluralsight bietet Onlinekurse, die eine Vielzahl von Themen im Zusammenhang mit Python abdecken, einschließlich eines Tools, mit dem Sie Ihre Fähigkeiten messen und Lücken finden können.

- [Tutorials auf LearnPython.org:](https://www.learnpython.org/) Machen Sie sich mit diesen kostenlosen interaktiven Python-Tutorials von den Leuten bei DataCamp mit den ersten Schritten mit Python vertraut, ohne etwas installieren oder einrichten zu müssen.

- [Tutorials auf python.org:](https://docs.python.org/3/tutorial/index.html) Hier finden Sie eine Einführung in die grundlegenden Konzepte und Features der Programmiersprache und des Systems Python.

- [Erlernen von Python auf Lynda.com:](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html) Eine grundlegende Einführung in Python.

### <a name="working-with-python-in-vs-code"></a>Arbeiten mit Python in VS Code

- [Bearbeiten von Python in VS Code:](https://code.visualstudio.com/docs/python/editing) Erfahren Sie mehr darüber, wie Sie die Unterstützung für AutoVervollständigen und IntelliSense von VS Code für Python nutzen können, einschließlich der Anpassung des Verhaltens – oder wie Sie diese Funktionen einfach deaktivieren.

- [Linting in Python:](https://code.visualstudio.com/docs/python/linting) Linting ist der Prozess der Ausführung eines Programms, mit dem Ihr Code auf mögliche Fehler analysiert wird. Erfahren Sie mehr über die verschiedenen Formen der Lintingunterstützung in VS Code für Python und wie Sie Linting einrichten.

- [Debuggen von Python:](https://code.visualstudio.com/docs/python/debugging) Beim Debuggen werden Fehler von einem Computerprogramm ermittelt und entfernt. In diesem Artikel wird beschrieben, wie Sie das Debuggen für Python mit VS Code initialisieren und konfigurieren, wie Sie Haltepunkte festlegen und überprüfen, ein lokales Skript anfügen, das Debugging für verschiedene App-Typen oder auf einem Remotecomputer ausführen und einige grundlegende Fehlerkorrekturen durchführen.

- [Komponententests in Python:](https://code.visualstudio.com/docs/python/unit-testing) Erfahren Sie über eine exemplarische Vorgehensweise einige Hintergrundinformationen zu den Komponententests, die Aktivierung eines Testframeworks, das Erstellen und Ausführen von Tests und das Debuggen von Tests sowie über die Testkonfigurationseinstellungen.