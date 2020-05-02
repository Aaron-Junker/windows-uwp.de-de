---
title: Skripts und Automatisierung mit Python unter Windows
description: Erste Schritte bei der Verwendung von Python für die Skripterstellung, Automatisierung und Systemverwaltung unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Python-Systemverwaltung, Python-Dateiautomatisierung, Python-Skripts unter Windows, Einrichten von Python unter Windows, Python-Entwicklerumgebung unter Windows, Python-Entwicklungsumgebung unter Windows, Python mit PowerShell, Python-Skripts für Dateisystemtasks
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d465d46a0524345a45dff9b1cc7c425e4cb468a4
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "79208995"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Erste Schritte bei der Verwendung von Python unter Windows für Skripts und Automatisierung

Im Folgenden finden Sie eine Schritt-für-Schritt-Anleitung für das Einrichten Ihrer Entwicklerumgebung und den Einstieg in die Verwendung von Python für die Skripterstellung und Automatisierung von Dateisystemvorgängen unter Windows.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie Ihre Umgebung einrichten, um einige der nützlichen Bibliotheken in Python zu verwenden, die Aufgaben plattformübergreifend automatisieren können, wie z. B. das Durchsuchen des Dateisystems, das Zugreifen auf das Internet, das Auswerten von Dateitypen usw. mit einem Windows-zentrierten Ansatz. Informationen zu Windows-spezifischen Vorgängen finden Sie unter [ctypes](https://docs.python.org/3/library/ctypes.html), einer C-kompatiblen Fremdfunktionsbibliothek für Python, [winreg](https://docs.python.org/3/library/winreg.html), Funktionen, die die Windows-Registrierungs-API für Python verfügbar machen, und [Python/WinRT](https://pypi.org/project/winrt/), um den Zugriff auf Windows-Runtime-APIs von Python zu ermöglichen.

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Wenn Sie Python zum Schreiben von Skripts verwenden, die Dateisystemvorgänge ausführen, sollten Sie [Python aus dem Microsoft Store installieren](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). Bei der Installation über den Microsoft Store wird der grundlegende Python3-Interpreter verwendet, jedoch werden zusätzlich zur Bereitstellung automatischer Updates Ihre PATH-Einstellungen für den aktuellen Benutzer eingerichtet (sodass kein Administratorzugriff erforderlich ist).

Wenn Sie Python zur **Webentwicklung** unter Windows verwenden, empfehlen wir eine andere Einrichtung mithilfe des Windows-Subsystems für Linux. Eine exemplarische Vorgehensweise finden Sie in unserem Handbuch: [Erste Schritte bei der Verwendung von Python für die Webentwicklung unter Windows](./web-frameworks.md). Wenn Sie noch nicht mit Python vertraut sind, testen Sie unser Handbuch: [Erste Schritte bei der Verwendung von Python unter Windows für Anfänger](./beginners.md). Für einige erweiterte Szenarien (wenn es z. B. erforderlich ist, auf installierte Python-Dateien zuzugreifen oder sie zu ändern, Kopien von Binärdateien zu erstellen oder Python-DLLs direkt zu erstellen) sollten Sie ein bestimmtes Python-Release direkt unter [python.org](https://www.python.org/downloads/) herunterladen oder eine [Alternative](https://www.python.org/download/alternatives) installieren, z. B. Anaconda, Jython, PyPy, WinPython, IronPython usw. Dies wird nur empfohlen, wenn Sie ein versierter Python-Programmierer sind und bestimmte Gründe für die Auswahl einer alternativen Implementierung haben.

## <a name="install-python"></a>Installieren von Python

So installieren Sie Python mit dem Microsoft Store:

1. Wechseln Sie zum Menü **Start** (unteres linkes Windows-Symbol), geben Sie „Microsoft Store“ ein, und wählen Sie den Link zum Öffnen des Stores aus.

2. Sobald der Store geöffnet ist, wählen Sie im oberen rechten Menü **Suchen** aus, und geben Sie „Python“ ein. Öffnen Sie „Python 3.7“ in den Ergebnissen unter „Apps“. Wählen Sie **Abrufen** aus.

3. Nachdem Python den Download- und Installationsprozess abgeschlossen hat, öffnen Sie Windows PowerShell über das Menü **Start** (unteres linkes Windows-Symbol). Sobald PowerShell geöffnet ist, geben Sie `Python --version` ein, um zu bestätigen, dass Python3 auf Ihrem Computer installiert wurde.

4. Die Microsoft Store-Installation von Python umfasst **PIP**, den standardmäßigen Paket-Manager. Über PIP können Sie zusätzliche Pakete installieren und verwalten, die nicht Teil der Python-Standardbibliothek sind. Geben Sie `pip --version` ein, um zu bestätigen, dass Ihnen auch PIP zum Installieren und Verwalten von Paketen zur Verfügung steht.

## <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Wenn Sie VS Code als Text-Editor/integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) verwenden, können Sie [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (eine Codevervollständigungshilfe), [Linting](https://code.visualstudio.com/docs/python/linting) (hilft, Fehler in Ihrem Code zu vermeiden), [Debugunterstützung](https://code.visualstudio.com/docs/python/debugging) (hilft Ihnen, Fehler in Ihrem Code nach der Ausführung zu finden), [Codeausschnitte](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (Vorlagen für kleine wiederverwendbare Codeblöcke) und [Komponententests](https://code.visualstudio.com/docs/python/unit-testing) (Testen der Codeschnittstelle mit unterschiedlichen Eingabetypen) nutzen.

Laden Sie VS Code für Windows herunter, und folgen Sie den Installationsanweisungen: [https://code.visualstudio.com](https://code.visualstudio.com).

## <a name="install-the-microsoft-python-extension"></a>Installieren der Microsoft Python-Erweiterung

Sie müssen die Microsoft Python-Erweiterung installieren, um die Features der VS Code-Unterstützung nutzen zu können. [Weitere Informationen](https://code.visualstudio.com/docs/languages/python)

1. Öffnen Sie das Fenster der VS Code-Erweiterungen, indem Sie **STRG+UMSCHALT+X** drücken (oder navigieren Sie im Menü zu **Ansicht** > **Erweiterungen**).

2. Geben Sie im oberen Feld **Nach Extensions in Marketplace suchen** Folgendes ein:  **Python**.

3. Suchen Sie die Erweiterung **Python (ms-python.python) von Microsoft**, und wählen Sie die grüne Schaltfläche **Installieren** aus.

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>Öffnen des integrierten PowerShell-Terminals in VS Code

VS Code enthält ein [integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal), mit dem Sie eine Python-Befehlszeile mit PowerShell öffnen können. Dadurch wird ein nahtloser Workflow zwischen dem Code-Editor und der Befehlszeile eingerichtet.

1. Öffnen Sie das Terminal in VS Code, wählen Sie **Anzeigen** > **Terminal** aus, oder verwenden Sie alternativ die Verknüpfung **STRG+`** (mit dem Graviszeichen).

    > [!NOTE]
    > Das Standardterminal sollte PowerShell sein. Wenn Sie es jedoch ändern müssen, verwenden Sie **STRG+UMSCHALT+P**, um die Befehlspalette aufzurufen. Geben Sie **Terminal: Standardshell auswählen** ein, und eine Liste von Terminaloptionen mit PowerShell, Eingabeaufforderung, WSL usw. wird angezeigt. Wählen Sie die gewünschte Option aus, und geben Sie **STRG+UMSCHALT+`** (mit dem Graviszeichen) ein, um ein neues Terminal zu erstellen.

2. Öffnen Sie Python in Ihrem VS Code-Terminal, indem Sie Folgendes eingeben: `python`.

3. Probieren Sie den Python-Interpreter aus, indem Sie Folgendes eingeben: `print("Hello World")`. Python gibt Ihre Anweisung „Hello World“ zurück.

    ![Python-Befehlszeile in VS Code](../images/python-in-vscode.png)

4. Um Python zu beenden, können Sie `exit()`, `quit()`oder „STRG+Z“ eingeben.

## <a name="install-git-optional"></a>Installieren von Git (optional)

Wenn Sie beabsichtigen, zusammen mit anderen an Ihrem Python-Code zu arbeiten oder das Projekt an einem Open-Source-Standort (wie GitHub) zu hosten, unterstützt VS Code die [Versionskontrolle mit Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). Auf der Registerkarte „Quellcodeverwaltung“ in VS Code werden alle Änderungen nachverfolgt und gängige Git-Befehle („Add“, „Commit“, „Push“, „Pull“) direkt in die Benutzeroberfläche integriert. Sie müssen zunächst Git installieren, um den Bereich „Quellcodeverwaltung“ zu aktivieren.

1. Laden Sie Git für Windows von der [git-scm-Website](https://git-scm.com/download/win) herunter, und installieren Sie Git.

2. Ein Installations-Assistent ist enthalten, der Ihnen eine Reihe von Fragen zu den Einstellungen für die Git-Installation stellt. Sie sollten alle Standardeinstellungen verwenden, sofern kein bestimmter Grund vorliegt, etwas zu ändern.

3. Wenn Sie noch nie zuvor mit Git gearbeitet haben, können Sie [GitHub-Leitfäden](https://guides.github.com/) für den Einstieg nutzen.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>Beispielskript zum Anzeigen der Struktur Ihres Dateisystemverzeichnisses

Allgemeine Systemverwaltungsaufgaben können sehr lange dauern, aber mit einem Python-Skript können Sie diese Aufgaben automatisieren, sodass Sie überhaupt keine Zeit investieren müssen. Python kann z. B. den Inhalt des Dateisystems Ihres Computers lesen und Vorgänge wie die Ausgabe einer Gliederung Ihrer Dateien und Verzeichnisse, das Verschieben von Ordnern von einem Verzeichnis in ein anderes oder das Umbenennen Hunderter von Dateien durchführen. Bei manueller Ausführung können solche Aufgaben normalerweise einige Zeit in Anspruch nehmen. Verwenden Sie stattdessen ein Python-Skript!

Beginnen wir mit einem einfachen Skript, das eine Verzeichnisstruktur durchläuft und die Verzeichnisstruktur anzeigt.

1. Öffnen Sie PowerShell über das Menü **Start** (unteres linkes Windows-Symbol).

2. Erstellen Sie ein Verzeichnis für das Projekt: `mkdir python-scripts`, und öffnen Sie dann das Verzeichnis: `cd python-scripts`.

3. Erstellen Sie einige Verzeichnisse zur Verwendung mit unserem Beispielskript:

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. Erstellen Sie in diesen Verzeichnissen einige Dateien zur Verwendung mit unserem Skript:

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. Erstellen Sie eine neue Python-Datei in Ihrem Python-Skriptverzeichnis:

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. Öffnen Sie das Projekt in VS Code, indem Sie Folgendes eingeben: `code .`.

7. Öffnen Sie den VS Code-Datei-Explorer, indem Sie **STRG+UMSCHALT+E** drücken (oder im Menü zu**Ansicht** > **Explorer** navigieren) und die gerade erstellte Datei „list-directory-contents.py“ auswählen. Die Microsoft Python-Erweiterung lädt automatisch einen Python-Interpreter. Sie können sehen, welcher Interpreter am unteren Rand des VS Code-Fensters geladen wurde.

    > [!NOTE]
    > Python ist eine übersetzte Sprache, d. h., sie fungiert als virtueller Computer, der einen physischen Computer emuliert. Verschiedene Arten von Python-Interpretern stehen Ihnen zur Verwendung zur Verfügung: Python 2, Python 3, Anaconda, PyPy etc. Zum Ausführen von Python-Code und Abrufen von Python IntelliSense müssen Sie VS Code mitteilen, welcher Interpreter verwendet werden soll. Sie sollten bei dem Interpreter bleiben, den VS Code standardmäßig auswählt (in unserem Fall Python 3), es sei denn, Sie haben einen bestimmten Grund für eine andere Auswahl. Um den Python-Interpreter zu ändern, wählen Sie den aktuell in der blauen Leiste unten im VS Code-Fenster angezeigten Interpreter aus, oder öffnen Sie die **Befehlspalette** (STRG+UMSCHALT+P), und geben Sie den Befehl **Python: Interpreter auswählen** ein. Dadurch wird eine Liste der derzeit installierten Python-Interpreter angezeigt. [Weitere Informationen zum Konfigurieren von Python-Umgebungen](https://code.visualstudio.com/docs/python/environments).

    ![Auswählen des Python-Interpreters in VS Code](../images/interpreterselection.gif)

8. Fügen Sie den folgenden Code in die „list-directory-contents.py“-Datei ein, und wählen Sie dann **Speichern** aus:

    ```python
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory:', directory)
        for name in subdir_list:
            print('Subdirectory:', name)
        for name in file_list:
            print('File:', name)
        print()
    ```

9. Öffnen Sie das integrierte VS Code-Terminal (**STRG +`** , verwenden Sie das Graviszeichen), und geben Sie das src-Verzeichnis ein, in dem Sie soeben das Python-Skript gespeichert haben:

    ```powershell
    cd src
    ```

10. Führen Sie das Skript in PowerShell aus mit:

    ```powershell
    python3 .\list-directory-contents.py
    ```

    Eine Ausgabe wie die folgende sollte angezeigt werden:

    ```powershell
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ..\food\fruits\apples
    File: honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: mandarin.txt

    Directory: ..\food\vegetables
    File: carrot.txt
    ```

11. Verwenden Sie Python, um das Dateisystemverzeichnis in eine eigene Textdatei auszugeben, indem Sie diesen Befehl direkt in Ihrem PowerShell-Terminal eingeben: `python3 list-directory-contents.py > food-directory.txt`.

Herzlichen Glückwunsch! Sie haben soeben ein automatisiertes Systemverwaltungsskript geschrieben, das das erstellte Verzeichnisse und die erstellten Dateien liest und mit Python die Verzeichnisstruktur anzeigt und anschließend in eine eigene Textdatei ausgibt.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>Beispielskript zum Ändern aller Dateien in einem Verzeichnis

In diesem Beispiel werden die Dateien und Verzeichnisse verwendet, die Sie soeben erstellt haben. Dabei wird jede der Dateien umbenannt, indem das Datum der letzten Änderung dem Dateinamen vorangestellt wird.

1. Erstellen Sie im Ordner **src** des **python-scripts**-Verzeichnisses eine neue Python-Datei für das Skript:

    ```powershell
    new-item update-filenames.py
    ```

2. Öffnen Sie die Datei „update-filenames.py“, fügen Sie den folgenden Code in die Datei ein, und speichern Sie sie:

    > [!NOTE]
    > „os.getmtime“ gibt einen Zeitstempel in Ticks zurück, der nicht leicht lesbar ist. Er muss zuerst in eine standardmäßige DateTime-Zeichenfolge konvertiert werden.

    ```python
    import datetime
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = os.path.join(directory, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = os.path.join(directory, f'{modified_date}_{name}')

            print(f'Renaming: {source_name} to: {target_name}')

            os.rename(source_name, target_name)
    ```

3. Testen Sie das update-filenames.py-Skript, indem Sie es ausführen: `python3 update-filenames.py`, und führen Sie dann das list-directory-contents.py-Skript erneut aus: `python3 list-directory-contents.py`.

4. Eine Ausgabe wie die folgende sollte angezeigt werden:

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    PS C:\src\python-scripting\src> python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. Verwenden Sie Python, um die neuen Dateisystemverzeichnis-Namen mit dem vorangestellten Zeitstempel der letzten Änderung in eine eigene Textdatei auszugeben, indem Sie diesen Befehl direkt in Ihrem PowerShell-Terminal eingeben: `python3 list-directory-contents.py > food-directory-last-modified.txt`.

Sie haben jetzt bestimmt einige interessante Aspekte der Verwendung von Python-Skripts für die Automatisierung grundlegender Systemverwaltungsaufgaben kennengelernt. Natürlich können Sie hierzu noch viel mehr lernen, aber wir hoffen, dass Sie hiermit einen guten Einstieg haben. Unten finden Sie einige zusätzliche, weiterführende Ressourcen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Python Docs: File and Directory Access](https://docs.python.org/3.7/library/filesys.html) (Python-Dokumente: Datei- und Verzeichniszugriff): Python-Dokumentation zum Arbeiten mit Dateisystemen und Verwenden von Modulen zum Lesen der Eigenschaften von Dateien, zum Ändern von Pfaden zur Portierbarkeit und Erstellen temporärer Dateien.
- [Learn Python: String_Formatting Tutorial](https://www.learnpython.org/en/String_Formatting) (Python lernen: Tutorial zur Zeichenfolgenformatierung): Weitere Informationen zur Verwendung des Operators „%“ zur Zeichenfolgenformatierung.
- [10 Python File System Methods You Should Know](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2) (10 Python-Dateisystemmethoden, die Sie kennen sollten): Weiterführender Artikel zur Bearbeitung von Dateien und Ordnern mit `os` und `shutil`.
- [The Hitchhikers Guide to Python: Systems Administration](https://docs.python-guide.org/scenarios/admin/) (Per Anhalter durch Python: Systemadministration): Ein „dogmatisches Handbuch", dass Übersichten und bewährte Vorgehensweisen zu Python enthält. In diesem Abschnitt werden die Tools und Frameworks zur Systemadministratuion behandelt. Dieses Handbuch wird auf GitHub gehostet, sodass Sie Probleme vortragen und Beiträge leisten können.
