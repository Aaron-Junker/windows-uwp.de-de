---
title: Häufig gestellte Fragen zum Verwenden von Python unter Windows
description: Holen Sie sich Hilfe, indem Sie die Antworten auf häufig gestellte Fragen (FAQs) zur Verwendung von Python unter Windows zur Entwicklung durchgehen.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, pip, py.exe, Dateipfade, PYTHONPATH, Python-Entwicklung, Python-Paketerstellung
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 4504e7550d19d2cc713284abebed43b6305b5dbd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174124"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Häufig gestellte Fragen zum Verwenden von Python unter Windows

## <a name="why-cant-i-pip-install-a-certain-package"></a>Warum kann ich ein bestimmtes Paket nicht mithilfe von pip installieren?

Es gibt eine Reihe von Gründen, warum bei einer Installation Fehler auftreten. In den meisten Fällen empfiehlt es sich, sich an den Paketentwickler zu wenden.

Die häufigste Ursache für Probleme ist die Installation an einem Speicherort, für den Sie nicht über die Berechtigung zum Ändern verfügen. Beispielsweise sind für das Standardinstallationsverzeichnis möglicherweise Administratorrechte erforderlich, Python weist diese jedoch standardmäßig nicht auf. Die beste Lösung besteht darin, eine virtuelle Umgebung zu erstellen und die Installation dort durchzuführen.

Einige Pakete enthalten nativen Code, für dessen Installation ein C- oder C++-Compiler erforderlich ist. Im Allgemeinen sollten Paketentwickler vorkompilierte Versionen veröffentlichen, dies ist aber nicht oft der Fall. Einige dieser Pakete werden möglicherweise ausgeführt, wenn Sie [Buildtools für Visual Studio installieren](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) und die C++-Option auswählen. In den meisten Fällen müssen Sie sich jedoch an den Paketentwickler wenden.

[Verfolgen Sie die Diskussion auf Stack Overflow](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

## <a name="what-is-pyexe"></a>Was ist „py.exe“?

Möglicherweise sind auf Ihrem Computer mehrere Python-Versionen installiert, da Sie verschiedene Python-Projekttypen bearbeiten. Da für alle diese Projekttypen der Befehl `python` verwendet wird, ist es möglicherweise nicht offensichtlich, welche Python-Version Sie verwenden. Standardmäßig wird empfohlen, den Befehl `python3` zu verwenden (oder `python3.7`, um eine bestimmte Version auszuwählen).

Mit dem [„py.exe“-Startprogramm](https://docs.python.org/3/using/windows.html#launcher) wird automatisch die neueste installierte Version von Python ausgewählt. Sie können außerdem Befehle wie `py -3.7` verwenden, um eine bestimmte Version auszuwählen, oder `py --list`, um anzuzeigen, welche Versionen verwendet werden können. Das „py.exe“-Startprogramm funktioniert jedoch **NUR**, wenn Sie eine Version von Python verwenden, die über [python.org](https://www.python.org/downloads/windows/) installiert wurde. Wenn Sie Python über den Microsoft Store installieren, ist der Befehl `py`**nicht enthalten**. Für Linux, macOS, WSL und die Microsoft Store-Version von Python sollten Sie den Befehl `python3` (oder `python3.7`) verwenden.

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>Warum wird beim Ausführen von „python.exe“ der Microsoft Store geöffnet?

Um neuen Benutzern zu helfen, eine gute Installation von Python zu finden, haben wir Windows eine Verknüpfung hinzugefügt, mit der Sie direkt zur neuesten Version des Pakets der Community gelangen, die im Microsoft Store veröffentlicht ist. Dieses Paket kann problemlos ohne Administratorrechte installiert werden und ersetzt die Standardbefehle `python` und `python3` durch die richtigen Befehle.

Wenn Sie die ausführbare Datei für die Verknüpfung mit Befehlszeilenargumenten ausführen, wird ein Fehlercode zurückgegeben, um anzugeben, dass Python nicht installiert wurde. Dadurch wird verhindert, dass die Store-App über Batchdateien und Skripts geöffnet wird, obwohl dies wahrscheinlich nicht vorgesehen war.

Wenn Sie Python mithilfe der Installationsprogramme von [python.org](https://www.python.org/downloads/windows/) installieren und die Option „Zu PATH hinzufügen“ auswählen, erhält der neue Befehl `python` Vorrang vor der Verknüpfung. Beachten Sie, dass `python` über andere Installationsprogramme als die integrierte Verknüpfung möglicherweise mit einer _niedrigeren_ Priorität hinzugefügt wird.

Sie können die Verknüpfungen deaktivieren, ohne Python zu installieren, indem Sie „Manage app execution aliases“ (App-Ausführungsaliase verwalten) im Startmenü öffnen, die Python-Einträge für „App-Installer“ suchen und diese auf „Aus“ festlegen.

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>Warum funktionieren Dateipfade in Python nicht, wenn ich sie kopiere und einfüge?

Python-Zeichenfolgen verwenden Escapezeichen für Sonderzeichen. Wenn Sie z. B. ein neues Zeilenzeichen in einer Zeichenfolge einfügen möchten, geben Sie `\n` ein. Da in Dateipfaden unter Windows umgekehrte Schrägstriche verwendet werden, werden einige Teile möglicherweise in Sonderzeichen umgewandelt.

Fügen Sie das Präfix `r` hinzu, um einen Pfad als Zeichenfolge in Python einzufügen. Dies gibt an, dass es sich um eine `raw`-Zeichenfolge handelt und mit Ausnahme von \" keine Escapezeichen verwendet werden (möglicherweise müssen Sie den letzten umgekehrten Schrägstrich im Pfad entfernen). Ihr Pfad könnte also wie folgt aussehen: `r"C:\Users\MyName\Documents\Document.txt"`.

Bei Verwendung von Pfaden in Python empfiehlt es sich, das standardmäßige pathlib-Modul zu verwenden. Auf diese Weise können Sie die Zeichenfolge in ein Rich-Path-Objekt konvertieren, das Pfadbearbeitungen konsistent durchführt, unabhängig davon, ob Sie Schrägstriche oder umgekehrte Schrägstriche verwenden, sodass der Code in verschiedenen Betriebssystemen besser funktioniert.

## <a name="what-is-pythonpath"></a>Was ist PYTHONPATH?

Mit der Umgebungsvariable PYTHONPATH kann in Python eine Liste von Verzeichnissen angegeben werden, aus denen Module importiert werden können. Bei der Ausführung können Sie die Variable `sys.path` überprüfen, um festzustellen, welche Verzeichnisse beim Import durchsucht werden.

Verwenden Sie `set PYTHONPATH=list;of;paths`, um diese Variable an der Eingabeaufforderung festzulegen.

Um diese Variable über PowerShell festzulegen, verwenden Sie `$env:PYTHONPATH=’list;of;paths’` direkt vor dem Starten von Python.

Die globale Festlegung dieser Variable über die Einstellungen für **Umgebungsvariablen** wird **nicht** empfohlen, da sie möglicherweise in einer beliebigen Python-Version verwendet wird und nicht in der Version, die verwendet werden soll.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>Wo finde ich Hilfe zur Paketerstellung und Bereitstellung?

[Docker:](https://code.visualstudio.com/docs/azure/docker) Mit der [VS Code-Erweiterung](https://code.visualstudio.com/docs/azure/docker) können Sie mithilfe von Dockerfile- und docker-compose.yml-Vorlagen Dateien schnell packen und bereitstellen. (Generieren Sie die entsprechenden Dockerfiles für Ihr Projekt.)

Mit [Azure Kubernetes Service (AKS)](/azure/aks/) können Sie Containeranwendungen bereitstellen und verwalten und Ressourcen bedarfsgesteuert skalieren.

## <a name="what-if-i-need-to-work-across-different-machines"></a>Wie gehe ich vor, wenn ich auf verschiedenen Computern arbeiten muss?

Über die [Synchronisierungseinstellungen](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) können Sie die VS Code-Einstellungen in verschiedenen Installationen über GitHub synchronisieren. Wenn Sie auf verschiedenen Computern arbeiten, können Sie die Umgebung auf diese Weise konsistent halten.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>Wie gehe ich vor, wenn ich normalerweise PyCharm, Atom, Sublime Text, Emacs oder Vim verwende?

Mit der VS Code-Erweiterung [Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) lässt sich die Umgebung anpassen.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Wie sind Mac-Tastenkombinationen Windows-Tastenkombinationen zugeordnet?

Einige Tastenkombinationen und Systemverknüpfungen unterscheiden sich geringfügig zwischen Windows- und Macintosh-Computern. Allgemeine Informationen dazu finden Sie in dieser [Anleitung](../dev-environment/mac-to-windows.md).