---
title: Hilfe beim Wechsel von Mac (Unix) zu Windows
description: Eine Anleitung, die dich beim Übergang von einer Mac-Entwicklungsumgebung (Unix) zu einer Windows-Entwicklungsumgebung unterstützt, einschließlich Tastenkombinationszuordnung und einer kurzen Übersicht über die Unterschiede zwischen Mac und Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac zu Windows, Tastenkombinationszuordnung, Wechsel von UNIX zu Windows, Umstellung von Mac auf Windows, Unterstützung des Wechsels von MacBook zu Surface, Verwendung von Windows für einen Macintosh-Benutzer, Wechsel von Macintosh zu Windows, Unterstützung der Änderung von Entwicklungsumgebungen, Mac OS X zu Windows, Unterstützung des Wechsels von Mac zu PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 457abcec97247afcc0d63c983c8a6cda2de51c66
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81643700"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Leitfaden zum Ändern der Entwicklungsumgebung von Mac zu Windows

Die folgenden Tipps und Steuerelementäquivalente sollten dich beim Wechsel von einer Mac- zu einer Windows-Entwicklungsumgebung (oder WSL/Linux) unterstützen.

Das nächste Äquivalent zu Xcode bei der App-Entwicklung ist [Visual Studio](https://visualstudio.microsoft.com). Es gibt auch eine Version von [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/) für den Fall, dass du zurückkehren möchtest. Für die plattformübergreifende Quellcodebearbeitung (und eine große Anzahl von Plug-Ins) ist [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) die beliebteste Wahl.

## <a name="keyboard-shortcuts"></a>Tastenkombinationen

| **Vorgang** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Kopieren | COMMAND+C | STRG+C |
| Ausschneiden | COMMAND+X | STRG+X |
| Einfügen | COMMAND+V | STRG+V |
| Rückgängig machen | COMMAND+Z | STRG+Z |
| Speichern | COMMAND+S | STRG+S |
| Öffnen Sie den | COMMAND+O | STRG+O |
| Computer sperren | COMMAND+STRG+Q | WINDOWS-TASTE+L |
| Desktop anzeigen | COMMAND+F3 | WINDOWS-TASTE+D |
| Dateibrowser öffnen | COMMAND+N | WINDOWS-TASTE+E |
| Fenster minimieren | COMMAND+M | WINDOWS-TASTE+M |
| Suchen | COMMAND+LEERTASTE | WINDOWS-TASTE |
| Aktives Fenster schließen | COMMAND+W | STRG+W |
| Aktuelle Aufgabe wechseln | COMMAND+TAB | ALT+TAB |
| Fenster zu Vollbild maximieren | STRG+COMMAND+F | WINDOWS-TASTE+NACH OBEN |
| Bildschirm speichern (Screenshot) | COMMAND+UMSCHALT+3 | WINDOWS-TASTE+UMSCHALT+S |
| Fenster speichern | COMMAND+UMSCHALT+4 | WINDOWS-TASTE+UMSCHALT+S |
| Elementinformationen oder -eigenschaften anzeigen | COMMAND+I | ALT+EINGABE |
 | Alle Elemente auswählen | COMMAND+A | STRG+A |
| Mehrere (nicht zusammenhängende) Elemente in einer Liste auswählen | COMMAND gedrückt halten und auf die einzelnen Elemente klicken | STRG gedrückt halten und auf die einzelnen Elemente klicken |
| Sonderzeichen eingeben | OPTION+Zeichentaste | ALT+Zeichentaste|

## <a name="trackpad-shortcuts"></a>Trackpad-Tastenkombinationen

Hinweis: Einige dieser Tastenkombinationen erfordern ein „Präzisionstrackpad“, z. B. die Trackpads auf Surface-Geräten und einigen anderen Laptops von Drittanbietern.

 **Vorgang** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | Mit zwei Fingern vertikal wischen | Mit zwei Fingern vertikal wischen |
| Zoom | Mit zwei Fingern vergrößern oder verkleinern | Mit zwei Fingern vergrößern oder verkleinern |
| Zwischen Ansichten rückwärts und vorwärts wischen | Mit zwei Fingern seitwärts wischen | Mit zwei Fingern seitwärts wischen |
| Virtuelle Arbeitsbereiche wechseln | Mit vier Fingern seitwärts wischen | Mit vier Fingern seitwärts wischen |
| Zurzeit geöffnete Apps anzeigen | Mit vier Fingern aufwärts wischen | Mit drei Fingern aufwärts wischen |
| Zwischen Apps wechseln | NICHT ZUTREFFEND | Langsam mit drei Fingern seitwärts wischen |
| Zum Desktop wechseln | Vier Finger spreizen | Mit drei Finger nach unten wischen |
| Cortana/Aktions-Center öffnen | Zwei Finger von rechts ziehen | Mit drei Fingern tippen |
| Zusätzliche Informationen öffnen | Mit drei Fingern tippen | NICHT ZUTREFFEND |
|Launchpad anzeigen/App starten | Mit vier Fingern drücken | Mit vier Fingern tippen |

Hinweis: Trackpadoptionen können auf beiden Plattformen konfiguriert werden.

## <a name="terminal-and-shell"></a>Terminal und Shell

Windows stellt verschiedene Alternativen zum Mac-Terminalemulator bereit.

1. Die Windows-Befehlszeile

Die Windows-Befehlszeile akzeptiert DOS-Befehle und ist das am häufigsten verwendete Befehlszeilentool unter Windows. So öffnest du sie: Drücke **WINDOWS-TASTE+R**, um das Feld **Ausführen** zu öffnen, gib **cmd** ein und klicke dann auf **OK**. Um eine Administratorbefehlszeile zu öffnen, gib **cmd** ein und drücke dann **STRG+UMSCHALT+EINGABETASTE**.

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) ist eine aufgabenbasierte Befehlszeilenshell und Skriptsprache, die auf .NET basiert. Mit PowerShell können Systemadministratoren und Poweruser schnell Aufgaben automatisieren, die Betriebssysteme verwalten. Mit anderen Worten: Es ist eine sehr leistungsfähige Befehlszeile, die besonders bei Systemadministratoren beliebt ist.

Übrigens ist PowerShell [auch für Mac verfügbar](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. Windows-Subsystem für Linux (WSL)

Mit WSL kannst du eine Linux-Shell innerhalb von Windows ausführen. Dies bedeutet, dass du abhängig von der Auswahl und der spezifischen installierten Linux-Distribution **bash** oder eine andere Shell ausführen kannst. Mithilfe von WSL wird eine Umgebung bereitgestellt, die Mac-Benutzern sehr vertraut ist. Beispielsweise werden Dateien in einem aktuellen Verzeichnis mit **ls** aufgelistet, nicht wie in der Windows-Befehlszeile mit **dir**. Weitere Informationen zum Installieren und Verwenden von WSL findest du in [Windows-Subsystem für Linux: Installationsleitfaden für Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

4. Windows Terminal (Vorschau)

Windows Terminal ist eine Anwendung, die Befehlszeilentools und Shells aus einer Reihe von Quellen kombiniert, einschließlich der herkömmlichen Windows-Befehlszeile, PowerShell und des Windows-Subsystems für Linux. Obwohl sie sich zurzeit noch in der Vorschauphase befindet, enthält sie bereits eine Reihe nützlicher Features wie z. B. Unterstützung mehrerer Registerkarten, geteilte Bereiche, benutzerdefinierte Designs und Stile sowie vollständige Unicodeunterstützung. Windows Terminal kann über den [Microsoft Store unter Windows 10](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) installiert werden.

## <a name="apps-and-utilities"></a>Apps und Hilfsprogramme

 **App** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Einstellungen und Präferenzen | Systemeinstellungen | Settings |
| Task-Manager | Aktivitätsmonitor | Task-Manager |
| Datenträgerformatierung | Datenträgerhilfsprogramm | Datenträgerverwaltung |
| Textbearbeitung | TextEdit | Editor |
| Ereignisanzeige | Konsole | Ereignisanzeige |
| Dateien/Apps suchen | COMMAND+LEERTASTE | WINDOWS-TASTE |
