---
title: Häufig gestellte Fragen zur Verwendung von Python unter Windows
description: Häufig gestellte Fragen zur Verwendung von Python unter Windows
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: python, Windows 10, Microsoft, PIP, py. exe, Dateipfade, PYTHONPATH, python-Bereitstellung, Python-Paket Erstellung
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 4f7f5c325dfd114093e1434259489459a8c78151
ms.sourcegitcommit: 161eac985af11faaff78797d86343d4fa7d6a05f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2019
ms.locfileid: "68366728"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Häufig gestellte Fragen zur Verwendung von Python unter Windows

## <a name="why-cant-i-pip-install-a-certain-package"></a>Warum kann ich ein bestimmtes Paket nicht "PIP install"?

Es gibt zahlreiche Gründe, warum eine Installation fehlschlägt. in den meisten Fällen besteht die richtige Lösung darin, den Paket Entwickler zu kontaktieren.

Die häufigste Ursache für Probleme ist die Installation an einem Speicherort, für den Sie nicht über die Berechtigung zum Ändern verfügen. Beispielsweise erfordert der Standard Installations Speicherort möglicherweise Administratorrechte, diese werden von python jedoch standardmäßig nicht verwendet. Die beste Lösung besteht darin, eine virtuelle Umgebung zu erstellen und dort zu installieren.

Einige Pakete enthalten systemeigenen Code, der einen C C++ -oder-Compiler für die Installation erfordert. Im Allgemeinen sollten Paket Entwickler vorkompilierte Versionen veröffentlichen, aber dies ist oft nicht der Fall. Einige dieser Pakete funktionieren möglicherweise, wenn Sie Buildtools [für Visual Studio installieren](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) und C++ die Option auswählen. in den meisten Fällen müssen Sie sich jedoch an den Paket Entwickler wenden.

[Befolgen Sie die Diskussion zu StackOverflow](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

## <a name="what-is-pyexe"></a>Was ist py. exe?

Möglicherweise verfügen Sie über mehrere Versionen von Python, die auf Ihrem Computer installiert sind, da Sie an verschiedenen python-Projekttypen arbeiten. Da diese alle den `python` Befehl verwenden, ist es möglicherweise nicht offensichtlich, welche Verwendung Sie verwenden. Das [Starter. exe](https://docs.python.org/3/using/windows.html#launcher) -Start Programm wählt automatisch die neueste Version von python aus, die Sie installiert haben. Sie können auch Befehle wie `py -3.7` verwenden, um eine bestimmte Version auszuwählen oder `py --list` um festzustellen, welche Versionen verwendet werden können. Das Starter. exe-Start Programm funktioniert **jedoch**nur, wenn Sie eine Version von Python verwenden, die von [python.org](https://www.python.org/downloads/windows/)installiert wird. Wenn Sie python von der Microsoft Store installieren, wird `py` der Befehl **nicht eingeschlossen**. Für Linux, macOS, WSL und die Microsoft Store-Version von python sollten Sie den `python3` Befehl verwenden.

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>Warum funktionieren Dateipfade in python nicht, wenn ich Sie kopieren und Einfügen?

Python-Zeichen folgen verwenden "Escapezeichen" für Sonderzeichen. Wenn Sie z. b. ein neues Zeilenzeichen in eine Zeichenfolge einfügen möchten `\n`, geben Sie ein. Da Dateipfade unter Windows umgekehrte Schrägstriche verwenden, werden einige Teile möglicherweise in Sonderzeichen konvertiert.

Fügen Sie das `r` Präfix hinzu, um einen Pfad als Zeichenfolge in python einzufügen. Dies weist darauf hin, dass `raw` es sich um eine Zeichenfolge handelt und keine Escapezeichen mit Ausnahme von \ "verwendet werden (möglicherweise müssen Sie den letzten umgekehrten Schrägstrich im Pfad entfernen). Ihr Pfad könnte also wie folgt aussehen: r "c:\users\meinname\documents\document.txt".

Beim Arbeiten mit Pfaden in python empfiehlt es sich, das standardmäßige pathlib-Modul zu verwenden. Auf diese Weise können Sie die Zeichenfolge in ein Rich-Path-Objekt konvertieren, das Pfad Manipulationen konsistent durchführt, unabhängig davon, ob Sie Schrägstriche oder umgekehrte Schrägstriche verwenden, sodass Ihr Code in verschiedenen Betriebssystemen besser funktioniert.

## <a name="what-is-pythonpath"></a>Was ist PYTHONPATH?

Die PYTHONPATH-Umgebungsvariable wird von Python verwendet, um eine Liste von Verzeichnissen anzugeben, aus denen Module importiert werden können. Wenn Sie ausführen, können Sie die `sys.path` Variable überprüfen, um festzustellen, welche Verzeichnisse durchsucht werden, wenn Sie etwas importieren.

Verwenden Sie zum Festlegen dieser Variablen an der Eingabeaufforderung: `set PYTHONPATH=list;of;paths`.

Um diese Variable aus PowerShell festzulegen, verwenden `$env:PYTHONPATH=’list;of;paths’` Sie: direkt vor dem Starten von Python.

Die globale Festlegung dieser Variablen über die **Umgebungsvariablen** Einstellungen ist **nicht** empfehlenswert, da Sie möglicherweise von einer beliebigen Version von python anstelle derjenigen verwendet wird, die Sie verwenden möchten.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>Wo finde ich Hilfe bei der Paket Erstellung und Bereitstellung?

[Docker](https://code.visualstudio.com/docs/azure/docker): Die [vscode-Erweiterung](https://code.visualstudio.com/docs/azure/docker) unterstützt Sie beim schnellen Verpacken und Bereitstellen mit dockerfile-und docker-Compose. yml-Vorlagen (Generieren der richtigen docker-Dateien für Ihr Projekt).

Mit [Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/) können Sie Container Anwendungen bereitstellen und verwalten, während Sie Ressourcenbedarfs gesteuert skalieren.

## <a name="what-if-i-need-to-work-across-different-machines"></a>Was geschieht, wenn ich über verschiedene Computer hinweg arbeiten muss?

Mit der [Synchronisierungs Einstellungen](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) können Sie Ihre vs Code Einstellungen mithilfe von GitHub über verschiedene Installationen hinweg synchronisieren. Wenn Sie auf unterschiedlichen Computern arbeiten, können Sie Ihre Umgebung auf diese Weise konsistent halten.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>Was geschieht, wenn ich pycharm, Atom, Sublime Text, emacs oder vim verwendet habe?

Die vscode-Erweiterung [keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) kann Ihnen bei Ihrer Umgebung helfen, sich zu Hause zu fühlen.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Wie werden Mac-Tastenkombinationen Windows-Tastenkombinationen zugeordnet?

Einige der Tastenkombinationen und System Verknüpfungen unterscheiden sich geringfügig von einem Windows-Computer und einem Macintosh-Computer. In diesem [Leitfaden zur Tastatur Zuordnung](https://support.microsoft.com/help/970299/keyboard-mappings-using-a-pc-keyboard-on-a-macintosh) von Microsoft-Support werden die Grundlagen behandelt.
