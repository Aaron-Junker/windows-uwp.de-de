---
title: Einstieg in die Verwendung von Python unter Windows für Einsteiger
description: Eine Anleitung für den Einstieg in die Verwendung von Python unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: python, Windows 10, Microsoft, Erlernen von Python, python unter Windows für Einsteiger, Installieren von Python mit Microsoft Store, Python mit vs Code, pygame unter unter Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9ef2349b296e5518d6bbb85a035526d7de25ea5c
ms.sourcegitcommit: 210034519678ba1a59744bc3a0b613b000921537
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68473680"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Einstieg in die Verwendung von Python unter Windows für Einsteiger

Im folgenden finden Sie eine Schritt-für-Schritt-Anleitung für Einsteiger, die für das Erlernen von Python mit Windows 10 interessiert sind.

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Für Einsteiger, die noch nicht mit python vertraut sind, wird empfohlen, [python über den Microsoft Store zu installieren](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). Bei der Installation über das Microsoft Store wird der grundlegende Python3-Interpreter verwendet. es werden jedoch die Pfad Einstellungen für den aktuellen Benutzer eingerichtet (wodurch kein Administrator Zugriff erforderlich ist) und automatische Updates bereitgestellt. Dies ist besonders hilfreich, wenn Sie sich in einer Schulungs Umgebung oder in einer Organisation befinden, die Berechtigungen oder den administrativen Zugriff auf Ihren Computer einschränkt.

Wenn Sie python unter Windows für die **Webentwicklung**verwenden, empfehlen wir eine andere Einrichtung für Ihre Entwicklungsumgebung. Anstatt direkt unter Windows zu installieren, empfiehlt es sich, python über das Windows-Subsystem für Linux zu installieren und zu verwenden. Hilfe finden Sie unter: [Beginnen Sie mit der Verwendung von Python für die Webentwicklung unter Windows](./python-for-web.md). Wenn Sie daran interessiert sind, gängige Aufgaben für Ihr Betriebssystem zu automatisieren, finden Sie weitere Informationen im Leitfaden: [Beginnen Sie mit der Verwendung von Python unter Windows für die Skripterstellung und Automatisierung](./python-for-scripting.md). Für einige erweiterte Szenarien (z. b. die Notwendigkeit, auf installierte Dateien von python zuzugreifen oder diese zu ändern, Kopien von Binärdateien zu erstellen oder python-DLLs direkt zu verwenden), empfiehlt es sich, eine bestimmte Python-Version direkt von [python.org](https://www.python.org/downloads/) herunterzuladen oder die Installation von eine [Alternative](https://www.python.org/download/alternatives), wie z. b. Anaconda, jython, pypy, winpython, IronPython usw. Dies wird nur empfohlen, wenn Sie ein fortschrittlicher python-Programmierer mit einem bestimmten Grund für die Auswahl einer alternativen Implementierung sind.

## <a name="install-python"></a>Installieren von python

So installieren Sie python mithilfe der Microsoft Store:

1. Wechseln Sie zu  Ihrem Startmenü (unten links Windows-Symbol), geben Sie "Microsoft Store" ein, und wählen Sie den Link zum Öffnen des Stores aus.

2. Nachdem der Speicher geöffnet ist, wählen Sie im Menü oben rechts die Option **Suchen** aus, und geben Sie "python" ein. Öffnen Sie "Python 3,7" in den Ergebnissen unter apps. Wählen Sie **Get**aus.

3. Nachdem python den Download-und Installationsprozess abgeschlossen hat, öffnen Sie Windows PowerShell  über das Startmenü (unten links Windows-Symbol). Nachdem PowerShell geöffnet ist, geben `Python --version` Sie ein, um zu bestätigen, dass Python3 auf Ihrem Computer installiert ist.

4. Die Microsoft Store Installation von python umfasst **PIP**, den Standardpaket-Manager. PIP ermöglicht Ihnen, zusätzliche Pakete zu installieren und zu verwalten, die nicht Teil der python-Standardbibliothek sind. Geben `pip --version`Sie ein, um zu bestätigen, dass Sie auch PIP zum Installieren und Verwalten von Paketen zur Verfügung haben.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Wenn Sie vs Code als Text-Editor/integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) verwenden, können Sie [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (eine Code Vervollständigungs Hilfe), [linting](https://code.visualstudio.com/docs/python/linting) (hilft, Fehler in Ihrem Code zu vermeiden) und [Debugunterstützung](https://code.visualstudio.com/docs/python/debugging) nutzen (hilft beim Auffinden von Fehlern in Ihren Code nach dem Ausführen), [Code Ausschnitte](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (Vorlagen für kleine wiederverwendbare Code Blöcke) und Komponenten [Tests](https://code.visualstudio.com/docs/python/unit-testing) (Testen Sie die Schnittstelle Ihres Codes mit verschiedenen Eingabetypen).

VS Code enthält auch ein [integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) , mit dem Sie eine python-Befehlszeile mit Windows-Eingabeaufforderung, PowerShell oder je nach Bedarf öffnen können, indem Sie einen nahtlosen Workflow zwischen dem Code-Editor und der Befehlszeile erstellen.

1. Um vs Code zu installieren, laden Sie vs Code für [https://code.visualstudio.com](https://code.visualstudio.com)Windows herunter:.

2. Python ist eine interpretierte Sprache, und zum Ausführen von Python-Code müssen Sie vs Code, welcher Interpreter verwendet werden soll. Wir empfehlen die Verwendung von Python 3,7, es sei denn, Sie haben einen bestimmten Grund für die Auswahl etwas anderes. Wählen Sie einen Python 3-Interpreter aus, indem Sie die **Befehls Palette** öffnen (STRG + UMSCHALT + P) **, und beginnen Sie mit der Eingabe des Befehls python: Wählen Sie** Interpreter für die Suche aus, und wählen Sie dann den Befehl Sie können auch die Option **python-Umgebung auswählen** in der unteren Status Leiste verwenden (sofern verfügbar). Mit dem Befehl wird eine Liste der verfügbaren Interpreter angezeigt, die vs Code automatisch finden kann, einschließlich virtueller Umgebungen. Wenn der gewünschte Interpreter nicht angezeigt wird, finden Sie weitere Informationen unter [Konfigurieren von python-Umgebungen](https://code.visualstudio.com/docs/python/environments).

    ![Python-Interpreter in vs Code auswählen](../../images/interpreterselection.gif)

3. Um das Terminal in vs Code zu öffnen, wählen Sie**Terminal** **anzeigen** > aus, oder verwenden Sie alternativ die Tastenkombination **Strg + "** (mit dem Graviszeichen-Zeichen). Das Standard Terminal ist PowerShell.

4. Öffnen Sie in Ihrem vs Code Terminal python, indem Sie einfach den folgenden Befehl eingeben:`python`

5. Probieren Sie den Python-Interpreter aus, `print("Hello World")`indem Sie Folgendes eingeben:. Python gibt Ihre Anweisung "Hallo Welt" zurück.

    ![Python-Befehlszeile in vs Code](../../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Installieren von git (optional)

Wenn Sie beabsichtigen, mit anderen in Ihrem Python-Code zusammenzuarbeiten oder das Projekt auf einem Open-Source-Standort (wie GitHub) zu hosten, unterstützt vs Code die [Versionskontrolle mit git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). Auf der Registerkarte Quell Code Verwaltung in vs Code werden alle Änderungen nachverfolgt, und es werden allgemeine git-Befehle (Add, Commit, Push, Pull) direkt in die Benutzeroberfläche integriert. Sie müssen zuerst git installieren, um die Quell Code Verwaltung zu über schalten.

1. Laden Sie git für Windows von [der git-SCM-Website](https://git-scm.com/download/win)herunter, und installieren Sie es.

2. Ein Installations-Assistent ist enthalten, mit dem Sie eine Reihe von Fragen zu den Einstellungen für die git-Installation erhalten. Es wird empfohlen, alle Standardeinstellungen zu verwenden, es sei denn, Sie haben einen bestimmten Grund, etwas zu ändern.

3. Wenn Sie noch nie mit git gearbeitet haben, können Ihnen [GitHub-Leitfäden](https://guides.github.com/) beim Einstieg helfen.

## <a name="hello-world-tutorial-for-some-python-basics"></a>Hallo Welt Tutorial für einige python-Grundlagen

Python ist, laut Ersteller von Guido van Rossum, eine "Programmiersprache auf hoher Ebene", und seine Kern Entwurfs Philosophie geht alles um die Lesbarkeit von Code und eine Syntax, die Programmierern das Ausdrücken von Konzepten in wenigen Codezeilen ermöglicht. "

Python ist eine interpretierte Sprache. Im Gegensatz zu kompilierten Sprachen, in denen der Code, den Sie schreiben, in Computercode übersetzt werden muss, damit er vom Prozessor des Computers ausgeführt werden kann, wird der Python-Code direkt an einen Interpreter geleitet und direkt ausgeführt. Sie geben einfach den Code ein und führen ihn aus. Probieren Sie es aus!

1. Geben Sie `python` bei geöffneter PowerShell-Befehlszeile ein, um den Python 3-Interpreter auszuführen. (Einige Anweisungen bevorzugen die Verwendung des Befehls `py` oder `python3`, Sie sollten ebenfalls funktionieren). Sie werden wissen, dass Sie erfolgreich sind, weil ein > > > Aufforderung mit drei mehr als Symbolen angezeigt wird.

2. Es gibt mehrere integrierte Methoden, mit denen Sie Änderungen an Zeichen folgen in python vornehmen können. Erstellen Sie eine Variable mit: `variable = 'Hello World!'`. Drücken Sie die EINGABETASTE für eine neue Zeile.

3. Drucken Sie Ihre Variable mit `print(variable)`:. Dadurch wird der Text "Hallo Welt!" angezeigt.

4. Ermitteln Sie die Länge der Zeichen folgen Variablen, wie viele Zeichen verwendet werden, mit: `len(variable)`. Dadurch wird angezeigt, dass 12 Zeichen verwendet werden. (Beachten Sie, dass der Leerraum als Zeichen in der Gesamtlänge gezählt wird.)

5. Konvertieren Sie die Zeichen folgen Variable in Großbuchstaben: `variable.upper()`. Konvertieren Sie nun die Zeichen folgen Variable in Kleinbuchstaben: `variable.lower()`.

6. Zählt, wie oft der Buchstabe "l" in der Zeichen folgen Variablen verwendet wird `variable.count("l")`:.

7. Suchen Sie in der Zeichen folgen Variablen nach einem bestimmten Zeichen, und suchen Sie das Ausrufezeichen mit `variable.find("!")`:. Dadurch wird angezeigt, dass sich das Ausrufezeichen im 11. Positions Zeichen der Zeichenfolge befindet.

8. Ersetzen Sie das Ausrufezeichen durch ein Fragezeichen `variable.replace("!", "?")`:.

9. Um Python zu beenden, können Sie `exit()`, `quit()`oder drücken, oder drücken Sie STRG + Z.

![PowerShell-Screenshot dieses Tutorials](../../images/hello-world-basics.png)

Hoffen Sie, dass Sie sich mit einigen der integrierten Zeichen folgen Änderungs Methoden von python lustig gemacht haben. Versuchen Sie nun, eine Python-Programmdatei zu erstellen und mit vs Code auszuführen.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>Hallo Welt Tutorial zur Verwendung von Python mit vs Code

Das vs Code Team hat ein hervorragende Lernprogramm für die ersten Schritte [mit python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) durchlaufen, in dem erläutert wird, wie Sie ein Hallo Welt Programm mit python erstellen, die Programmdatei ausführen, den Debugger konfigurieren und ausführen und Pakete wie *matplotlib* *installieren. numpy* , um eine grafische Darstellung in einer virtuellen Umgebung zu erstellen.

1. Öffnen Sie PowerShell, und erstellen Sie einen leeren Ordner mit dem Namen "Hello". Navigieren Sie zu diesem Ordner, und öffnen Sie ihn in vs Code:

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. Sobald vs Code geöffnet ist, wird der neue Ordner " *Hello* " im linken **Explorer** -Fenster angezeigt, öffnen Sie im unteren Bereich des vs Code ein Befehlszeilenfenster, indem Sie **Strg + "** (mit dem Graviszeichen-Zeichen) drücken oder **Ansicht**  > auswählen. **Terminal**. Wenn Sie vs Code in einem Ordner starten, wird dieser Ordner zum Arbeitsbereich. VS Code speichert Einstellungen, die für diesen Arbeitsbereich spezifisch sind, in ". vscode/Settings. JSON", die von den Global gespeicherten Benutzereinstellungen getrennt sind.

3. Fahren Sie mit dem Tutorial in den vs Code docs fort: [Erstellen Sie eine python-Hallo Welt Quell Code Datei](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file).

## <a name="create-a-simple-game-with-pygame"></a>Erstellen eines einfachen Spiels mit pygame

![Pygame, das ein Beispiel Spiel führt](../../images/pygame-shmup.jpg)

Pygame ist ein beliebtes Python-Paket zum Schreiben von spielen, die Schüler und Studenten zum Erlernen der Programmierung bei der Erstellung von etwas Spaß machen. Pygame zeigt Grafiken in einem neuen Fenster an und funktioniert daher nicht bei der Befehlszeilen basierten WSL-Methode. Wenn Sie python jedoch wie in diesem Tutorial beschrieben über die Microsoft Store installiert haben, funktioniert es problemlos.

1. Nachdem Sie python installiert haben, installieren Sie pygame unter über die Befehlszeile (oder das Terminal innerhalb vs Code), indem `python -m pip install -U pygame --user`Sie eingeben.

2. Testen Sie die Installation, indem Sie ein Beispiel Spiel ausführen:`python -m pygame.examples.aliens`

3. Alles gut, das Spiel öffnet ein Fenster. Schließen Sie das Fenster, wenn Sie die Wiedergabe abgeschlossen haben.

So beginnen Sie mit dem Schreiben Ihres eigenen Spiels.

1. Öffnen Sie PowerShell (oder Windows-Eingabeaufforderung), und erstellen Sie einen leeren Ordner mit dem Namen "Bounce". Navigieren Sie zu diesem Ordner, und erstellen Sie eine Datei mit dem Namen "Bounce.py". Öffnen Sie den Ordner in vs Code:

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. Geben Sie mithilfe vs Code den folgenden Python-Code ein (oder kopieren Sie ihn, und fügen Sie ihn ein):

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

3. Speichern Sie die Datei `bounce.py`unter:.

4. Führen Sie das PowerShell-Terminal aus, indem Sie `python bounce.py`Folgendes eingeben:.

    ![Pygame führt das nächste große Ding aus](../../images/pygame.jpg)

Versuchen Sie, einige der Zahlen anzupassen, um zu sehen, welche Auswirkung Sie auf die Sprung Kugel haben.

Weitere Informationen zum Schreiben von spielen mit pygame unter finden Sie unter [pygame.org](http://www.pygame.org).

## <a name="resources-for-continued-learning"></a>Ressourcen für fortgesetztes lernen

Wir empfehlen Ihnen die folgenden Ressourcen, um weitere Informationen zur Python-Entwicklung unter Windows zu erhalten.

### <a name="online-courses-for-learning-python"></a>Online Kurse zum Erlernen von python

- [Einführung in python auf Microsoft Learn](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/): Probieren Sie die interaktive Microsoft Learn Plattform aus, und gewinnen Sie Erfahrungspunkte für dieses Modul, das die Grundlagen zum Schreiben von grundlegender Python-Code, zum Deklarieren von Variablen und zum Arbeiten mit Konsolen Eingaben und-Ausgaben umfasst. Die interaktive Sandbox-Umgebung ist ein idealer Ausgangspunkt für Benutzer, die noch nicht Ihre python-Entwicklungsumgebung eingerichtet haben.

- [Python in Pluralsight: 8 Kurse, 29 Stunden](https://app.pluralsight.com/paths/skills/python): Der python-Lernpfad in Pluralsight bietet Onlinekurse, die eine Vielzahl von Themen im Zusammenhang mit python abdecken, einschließlich eines Tools, mit dem Sie Ihre Fähigkeiten Messen und ihre Lücken finden.

- [LearnPython.org-Tutorials](https://www.learnpython.org/): Machen Sie sich mit den ersten Schritten zum Erlernen von python vertraut, ohne dass Sie mit diesen kostenlosen interaktiven Python-Tutorials von den Leuten im datacamp arbeiten müssen.

- [Die python.org-Tutorials](https://docs.python.org/3/tutorial/index.html): Führt den Reader informell in die grundlegenden Konzepte und Features der Python-Sprache und des python-Systems ein.

- [Erlernen von python auf Lynda.com](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html): Eine grundlegende Einführung in Python.

### <a name="working-with-python-in-vs-code"></a>Arbeiten mit python in vs Code

- [Bearbeiten von python in vs Code](https://code.visualstudio.com/docs/python/editing): Erfahren Sie mehr darüber, wie Sie die Auto Vervollständigen-und IntelliSense-Unterstützung von vs Code für python nutzen können, einschließlich der Vorgehensweise zum Anpassen von behvior... oder deaktivieren Sie Sie einfach.

- [Linting von python](https://code.visualstudio.com/docs/python/linting): Linting ist der Prozess der Ausführung eines Programms, mit dem Code auf mögliche Fehler analysiert wird. Erfahren Sie mehr über die verschiedenen Formen der linting-Unterstützung vs Code die für python bereitstellt, und wie Sie Sie einrichten.

- [Debugging von python](https://code.visualstudio.com/docs/python/debugging): Beim Debuggen werden Fehler von einem Computerprogramm identifiziert und entfernt. In diesem Artikel wird beschrieben, wie Sie das Debuggen für python mit vs Code initialisieren und konfigurieren, wie Sie Haltepunkte festlegen und überprüfen, ein lokales Skript anfügen, das Debugging für verschiedene App-Typen oder auf einem Remote Computer ausführen und einige grundlegende Problembehandlung durchführen.

- Komponenten [Tests für python](https://code.visualstudio.com/docs/python/unit-testing): Hier finden Sie einige Hintergrundinformationen zu den Komponententests, eine exemplarische Vorgehensweise, die Aktivierung eines Test-Frameworks, das Erstellen und Ausführen von Tests, das Debuggen von Tests und die Test Konfigurationseinstellungen. 
