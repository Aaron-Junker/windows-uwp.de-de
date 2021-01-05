---
title: PowerToys-Tastatur-Manager-Hilfsprogramm für Windows 10
description: Ein Hilfsprogramm, das Benutzern das Neudefinieren von Schlüsseln auf der Tastatur ermöglicht
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb17cd5a7ad76728e6b063f76369c8d194a5e12c
ms.sourcegitcommit: 1a997d7e0100e58886150f9fba33d7b205f41df1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2021
ms.locfileid: "97865465"
---
# <a name="keyboard-manager-utility"></a>Tastatur-Manager

Mit dem PowerToys-Tastatur-Manager können Sie Schlüssel auf der Tastatur neu definieren.

Beispielsweise können Sie den Buchstaben <kbd>A</kbd> für den Buchstaben <kbd>D</kbd> auf der Tastatur austauschen. Wenn Sie die <kbd>a</kbd> -Taste auswählen, wird eine <kbd>D</kbd> angezeigt.

![Bildschirm Abbildung von PowerToys-Tastatur-Manager-Neuzuordnen](../images/powertoys-keyboard-remap.png)

Sie können auch Tastenkombinationen austauschen. Beispielsweise kopiert die Tastenkombination <kbd>STRG</kbd> + <kbd>C</kbd>Text in Microsoft Word. Mit dem PowerToys-Tastatur-Manager-Hilfsprogramm können Sie diese Verknüpfung für <kbd>⊞ Win</kbd> + <kbd>C</kbd>) austauschen. Nun kopiert <kbd>⊞ Win</kbd> + <kbd>C</kbd>) Text. Wenn Sie im PowerToys-Tastatur-Manager keine Zielanwendung angeben, wird der Kontext Austausch Global über Windows angewendet.

Der PowerToys-Tastatur-Manager muss aktiviert sein (mit PowerToys, der im Hintergrund ausgeführt wird), damit neu zugeordnete Tasten und Verknüpfungen angewendet werden. Wenn PowerToys nicht ausgeführt wird, wird das erneute Zuordnen von Schlüsseln nicht mehr angewendet.

![Screenshot der Tastenkombinationen für PowerToys-Tastatur-Manager](../images/powertoys-keyboard-shortcuts.png)

> [!NOTE]
> Es gibt einige Tastenkombinationen, die für das Betriebssystem reserviert sind und nicht ersetzt werden können. Folgende Schlüssel können nicht neu zugeordnet werden:
> - `⊞ Win`+`L`und `Ctrl`  +  `Alt`  +  `Del` können nicht neu zugeordnet werden, da Sie vom Windows-Betriebssystem reserviert werden.
> - Der `Fn` (-Funktions-) Schlüssel kann (in den meisten Fällen) nicht neu zugeordnet werden. Die `F1` - `F12` Schlüssel (und `F13` - `F24` ) können zugeordnet werden.
> - `Pause` sendet nur ein sngle-KeyDown-Ereignis. Wenn Sie die Zuordnung beispielsweise mit der RÜCKTASTE-Taste durchführt und durch Drücken von + halten, wird nur ein einzelnes Zeichen gelöscht.

## <a name="settings"></a>Einstellungen

Um Zuordnungen mit dem Tastatur-Manager zu erstellen, haben Sie folgende Möglichkeiten:

- Fenster "Remap-Tastatur Einstellungen" starten, indem Sie <kbd>einen Schlüssel</kbd> neu zuordnen auswählen
- Öffnen Sie das Fenster für die Neuzuordnung von Verknüpfungen, indem Sie die <kbd>Verknüpfung</kbd> neu zuordnen auswählen.

## <a name="remap-keys"></a>Neuzuordnen von Schlüsseln

Um einen Schlüssel neu zuzuordnen und in neuen Wert zu ändern, starten Sie das Fenster "Umwandlungs-Tastatur Einstellungen" mit der Schaltfläche " <kbd>Umwandlungs a Key</kbd> ". Beim ersten Start werden keine vordefinierten Zuordnungen angezeigt. Sie müssen die <kbd>+</kbd> Schaltfläche auswählen, um eine neue Neuzuordnung hinzuzufügen.

Wenn eine neue Zeile neu zuordnen angezeigt wird, wählen Sie den Schlüssel aus, dessen Ausgabe Sie in der Spalte "Key" in der Spalte "Key"**ändern** möchten. Wählen Sie den neuen Schlüsselwert aus, der in der Spalte "zugeordnet zu" zugewiesen werden soll.

Wenn Sie z. B. <kbd>eine</kbd> drücken und die Taste <kbd>B</kbd>  angezeigt wird:

- Schlüssel: "A"
- Zugeordnet zu: "B"

Fügen Sie zum Austauschen von Schlüsselpositionen zwischen den Schlüsseln "A" und "B" eine weitere Neuzuordnung mit hinzu:

- Schlüssel: "B"
- Zugeordnet zu: "A"

![Bildschirm Abbildung von Tastatur Umwandlungs Schlüsseln](../images/powertoys-keyboard-remap-a-b.png)

## <a name="key-to-shortcut"></a>Tastenkombination

Geben Sie die Tastenkombination in der Spalte "zugeordnet zu" ein, um einen Schlüssel einer Verknüpfung zuzuordnen (Tastenkombination).

Wenn Sie z. b. den Schlüssel "C" auswählen und ihn zu "STRG + V" führen möchten:

- Schlüssel: "C"
- Zugeordnet zu: "STRG + V"

> [!NOTE]
> Das Neuzuordnen von Schlüsseln wird auch dann beibehalten, wenn der neu zugeordnete Schlüssel in einer anderen Verknüpfung verwendet wird. Wenn Sie z. b. die Tastenkombination "Alt + C" eingeben, ergibt sich "ALT + STRG + v", da die C-Taste zu "Strg + v" neu zugeordnet wurde.

## <a name="remap-shortcuts"></a>Neuzuordnen von Tastenkombinationen

Um eine Tastenkombination neu zuzuordnen, wie z. b. "Strg + v" <kbd>, wählen Sie neu zuordnen, um</kbd> das Fenster für die Neuzuordnung von Verknüpfungen zu starten.

Beim ersten Start werden keine vordefinierten Zuordnungen angezeigt. Sie müssen die <kbd>+</kbd> Schaltfläche auswählen, um eine neue Neuzuordnung hinzuzufügen.

Wenn eine neue Zeile neu zuordnen angezeigt wird, wählen Sie den Schlüssel aus, dessen Ausgabe Sie in der Spalte "Verknüpfung" _*_ändern_*_ möchten. Wählen Sie den neuen Verknüpfungs Wert aus, der in der Spalte "zugeordnet zu" zugewiesen werden soll.

Beispielsweise kopiert die Tastenkombination <kbd>STRG</kbd> + <kbd>C</kbd> den markierten Text. Zum erneuten Zuordnen dieser Verknüpfungen zur Verwendung der <kbd>alt</kbd> -Taste anstelle der <kbd>STRG</kbd> -Taste:

- Verknüpfung: "Strg" + "C"
- Zugeordnet zu: "alt" + "C"

![Bildschirm Abbildung der Tastatur Umwandlungs Kombination](../images/powertoys-keyboard-remap-shortcut.png)

Beim Neuzuordnen von Verknüpfungen müssen einige Regeln befolgt werden (diese Regeln gelten nur für die Spalte "Verknüpfung"):

- Verknüpfungen müssen mit einer Modifizierertaste beginnen: <kbd>STRG</kbd>, <kbd>UMSCHALT</kbd>, <kbd>alt</kbd>oder <kbd>⊞ Win</kbd>
- Verknüpfungen müssen mit einem Aktions Schlüssel (alle nicht-modifiziererschlüssel) enden: A, B, C, 1, 2, 3 usw.
- Verknüpfungen dürfen nicht länger als 3 Schlüssel sein.  

## <a name="remap-a-shortcut-to-a-single-key"></a>Zuordnen einer Verknüpfung zu einem einzelnen Schlüssel

Es ist möglich, eine Verknüpfung (Tastenkombination) einem einzelnen Tastendruck zuzuordnen.

Um z. b. die Tastenkombination <kbd>⊞ Win</kbd>  +  <kbd>D</kbd> (Anzeigen/Ausblenden von Windows-Desktop-Apps) durch einen einzelnen Tastendruck zu ersetzen, <kbd>alt</kbd>:

- Verknüpfung: "⊞ Win" (Windows-Taste) + "D"
- Zugeordnet zu: "alt"

> [!NOTE]
> Die Neuzuordnung von Verknüpfungen wird auch dann beibehalten, wenn die neu zugeordnete Taste in einer anderen Verknüpfung verwendet wird. Wenn Sie z. b. die Tastenkombination "alt" + "Tab" eingeben, nachdem Sie den Schlüssel "alt" neu zugeordnet haben, ergibt sich "⊞ Win" + "D" + "Tab".

## <a name="app-specific-shortcuts"></a>App-spezifische Verknüpfungen

Mit dem Tastatur-Manager können Sie Verknüpfungen nur für bestimmte apps neu zuordnen (anstatt Global über Windows).

Beispielsweise wird in der Outlook-e-Mail-APP die Verknüpfung "STRG + E" standardmäßig festgelegt, um nach einer e-Mail zu suchen. Wenn Sie stattdessen "Strg + F" festlegen möchten, um Ihre e-Mail zu durchsuchen (anstatt eine standardmäßig festgelegte e-Mail weiterzuleiten), können Sie die Verknüpfung mit "Outlook" (als Ziel-App festgelegt) neu zuordnen.

Der Tastatur-Manager verwendet die Prozessnamen (nicht Anwendungsnamen), um Apps als Ziel zu verwenden. Microsoft Edge ist beispielsweise auf "msedge" (Prozess Name) und nicht auf "Microsoft Edge" (Anwendungsname) festgelegt. Öffnen Sie PowerShell, und geben Sie den Befehl ein, und geben Sie den Befehl ein, um den Prozessnamen einer Anwendung zu ermitteln `get-process` `tasklist` . Dies führt zu einer Liste von Prozessnamen für alle Anwendungen, die Sie zurzeit geöffnet haben. Im folgenden finden Sie eine Liste einiger beliebter Anwendungsprozess Namen.

  | _ *Anwendung**   | **Prozessname**|
  | ------------------| --------------|
  | Microsoft Edge    |  msedge.exe   |
  | OneNote           |  onenote.exe  |
  | Outlook           |  outlook.exe  |
  | Teams             |  Teams.exe    |
  | Adobe Photoshop   |  Photoshop.exe|
  | Datei-Explorer     |  explorer.exe |
  | Spotify-Musik     |  spotify.exe  |
  | Google Chrome     |  chrome.exe   |
  | Excel             |  excel.exe    |
  | Word              |  winword.exe  |
  | PowerPoint        |  powerpnt.exe |

### <a name="keys-that-cannot-be-remapped"></a>Schlüssel, die nicht neu zugeordnet werden können

Es gibt bestimmte Tastenkombinationen, die für die Neuzuordnung nicht zulässig sind. Dazu gehören:

- <kbd>STRG</kbd> + <kbd>Alt</kbd> +  <kbd>Del</kbd> (Interupt-Befehl)
- <kbd>⊞ Win</kbd> + <kbd>L</kbd> (Sperren Ihres Computers)
- Die Funktionstaste " <kbd>FN</kbd>" kann (in den meisten Fällen) nicht neu zugeordnet werden, aber die <kbd>F1</kbd>-Taste - <kbd>F12</kbd> kann zugeordnet werden.

## <a name="how-to-select-a-key"></a>Auswählen eines Schlüssels

Zum Auswählen eines Schlüssels oder einer Verknüpfung für die Neuzuordnung können Sie folgende Aktionen ausführen:

- Verwenden Sie die Schaltfläche <kbd>Typschlüssel</kbd> .
- Verwenden Sie das Dropdown Menü.

Nachdem Sie die <kbd>typtaste/die Tastenkombination</kbd> ausgewählt haben, wird ein Dialogfeld angezeigt, in dem Sie den Schlüssel oder die Verknüpfung mit der Tastatur eingeben können. Wenn Sie mit der Ausgabe zufrieden sind, halten Sie die <kbd>Eingabe</kbd> Taste, um fortzufahren. Wenn Sie den Dialog verlassen möchten, halten Sie die <kbd>ESC</kbd> -Taste gedrückt.

Mithilfe des Dropdown Menüs können Sie mit dem Schlüsselnamen suchen, und weitere Dropdown Werte werden angezeigt, wenn Sie den Vorgang fortsetzen. Sie können jedoch die typschlüsselfunktion nicht verwenden, während das Dropdown Menü geöffnet ist.

## <a name="orphaning-keys"></a>Verwaisen von Schlüsseln

Das verwaisen eines Schlüssels bedeutet, dass Sie ihn einem anderen Schlüssel zugeordnet haben und ihm nichts mehr zugeordnet ist.

Wenn der Schlüssel z. B. von A-> B neu zugeordnet wird, ist auf der Tastatur kein Schlüssel mehr vorhanden, der zu einer führt.

Um dieses Problem zu beheben, verwenden Sie +, um einen weiteren neu zugeordneten Schlüssel zu erstellen, der dem Ergebnis einer zugeordnet ist. Um sicherzustellen, dass dies nicht versehentlich geschieht, wird eine Warnung für alle verwaisten Schlüssel angezeigt.

![Verwaister Schlüssel für PowerToys-Tastatur-Manager](../images/powertoys-keyboard-remap-orphaned.png)

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="i-remapped-the-wrong-keys-how-can-i-stop-it-quickly"></a>Ich habe die falschen Schlüssel neu zugeordnet, wie kann ich Sie schnell Abbrechen?

Damit das erneute Zuordnen von Schlüsseln funktioniert, muss PowerToys im Hintergrund ausgeführt werden, und der Tastatur-Manager muss aktiviert sein. Um neu zugeordnete Schlüssel zu beenden, schließen Sie PowerToys, oder deaktivieren Sie den Tastatur-Manager in den PowerToys-Einstellungen.

### <a name="can-i-use-keyboard-manager-at-my-log-in-screen"></a>Kann ich den Tastatur-Manager auf meinem Anmeldebildschirm verwenden?

Nein, der Tastatur-Manager ist nur verfügbar, wenn PowerToys ausgeführt wird und auf keinem Kenn Wort Bildschirm funktioniert, einschließlich als Administrator ausführen.

### <a name="do-i-have-to-turn-off-my-computer-for-the-remapping-to-take-effect"></a>Muss ich meinen Computer deaktivieren, damit die Neuzuordnung wirksam wird?

Nein, das Neuzuordnen sollte unmittelbar nach dem Drücken von **Apply** erfolgen.

### <a name="where-are-the-maclinux-profiles"></a>Wo sind die Mac-/Linux-profile?

Derzeit sind keine Mac-und Linux-Profile enthalten.

### <a name="will-this-work-on-video-games"></a>Wird dies bei Videospielen funktionieren?

Das hängt davon ab, wie das Spiel auf Ihre Schlüssel zugreift. Bestimmte Tastatur-APIs können nicht mit dem Tastatur-Manager verwendet werden.

### <a name="will-remapping-work-if-i-change-my-input-language"></a>Werden die Aufgaben neu zugeordnet, wenn ich meine Eingabe Sprache ändere?

Ja, es wird angezeigt. Wenn Sie nun <kbd>a</kbd> auf Englisch (US)-Tastatur neu zuordnen und dann die Spracheinstellung in Französisch ändern, würde die Eingabe <kbd>eines</kbd> auf der französischen Tastatur (<kbd>Q</kbd> auf der physischen US-Tastatur <kbd>) zu</kbd> <kbd>b</kbd>führen. Dies entspricht der Art und Weise, wie Windows mehrsprachige Eingaben behandelt.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie versucht haben, einen Schlüssel oder eine Verknüpfung neu zuzuordnen, und Probleme auftreten, kann dies eine der folgenden Probleme haben:

- **Als Administrator ausführen:** Die Neuzuordnung funktioniert nicht in einer APP/einem Fenster, wenn dieses Fenster im Administrator Modus (erweitert) ausgeführt wird und PowerToys nicht als Administrator ausgeführt wird. Versuchen Sie, PowerToys als Administrator auszuführen.

- **Schlüssel werden nicht abgefangen:** Der Tastatur-Manager fängt die Tastatur Hooks ab, um die Schlüssel neu zuzuordnen. Einige apps, die dies ebenfalls tun und den Tastatur-Manager beeinträchtigen können. Um dieses Problem zu beheben, navigieren Sie zu den Einstellungen, und aktivieren Sie dann den Tastatur-Manager erneut.

## <a name="known-issues"></a>Bekannte Probleme

- [Obergrenzen Licht Indikator nicht ordnungsgemäß umschalten](https://github.com/microsoft/PowerToys/issues/1692)

- [Neuzuordnung funktioniert nicht für fancyzones und Verknüpfungs Handbuch](https://github.com/microsoft/PowerToys/issues/3079)

- [Das Neuzuordnen von Schlüsseln wie Win, STRG, alt oder Shift kann die Gesten und einige sonderschaltflächen unterbrechen.](https://github.com/microsoft/PowerToys/issues/3703)

Sehen Sie sich die Liste der [offenen Tastatur-Manager-Probleme](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3A%22Product-Keyboard+Shortcut+Manager%22)an.
