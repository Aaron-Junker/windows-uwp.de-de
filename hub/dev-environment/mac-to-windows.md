---
title: Wechseln von Mac (Unix) zu Windows
description: Eine Anleitung, die dich beim Übergang von einer Mac-Entwicklungsumgebung (Unix) zu einer Windows-Entwicklungsumgebung unterstützt, einschließlich Tastenkombinationszuordnung und einer kurzen Übersicht über die Unterschiede zwischen Mac und Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac zu Windows, Tastenkombinationszuordnung, Wechsel von UNIX zu Windows, Umstellung von Mac auf Windows, Unterstützung des Wechsels von MacBook zu Surface, Verwendung von Windows für einen Macintosh-Benutzer, Wechsel von Macintosh zu Windows, Unterstützung der Änderung von Entwicklungsumgebungen, Mac OS X zu Windows, Unterstützung des Wechsels von Mac zu PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 782f0d50c8b1f577f8530628b76b3df75673c083
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254587"
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

## <a name="command-line-shells-and-terminals"></a>Befehlszeilenshells und Terminals

Windows unterstützt mehrere Befehlszeilenshells und Terminals, die manchmal etwas anders funktionieren als die BASH-Shell des Mac und Terminalemulator-Apps wie Terminal und iTerm.

### <a name="windows-shells"></a>Windows-Shells

Windows verfügt über zwei primäre Befehlszeilenshells:

1. **[PowerShell](/powershell/scripting/overview?view=powershell-7)** : PowerShell ist ein plattformübergreifendes Framework zur Aufgabenautomatisierung und Konfigurationsverwaltung, das aus einer Befehlszeilenshell und einer Skriptsprache auf .NET-Basis besteht. Mit PowerShell können Administratoren, Entwickler und Power-User Aufgaben zur Verwaltung komplexer Prozesse und verschiedener Aspekte der Umgebung und des Betriebssystems, auf dem dieses ausgeführt wird, schnell steuern und automatisieren. PowerShell ist [ vollständig open-source](https://github.com/powershell/powershell), und weil es plattformübergreifend ist, auch [für Mac und Linux verfügbar](/powershell/scripting/install/installing-powershell?view=powershell-7).

    **Mac-und Linux-BASH-Shell-Benutzer**: PowerShell unterstützt auch viele Befehlsaliase, mit denen Sie bereits vertraut sind. Beispiel:
    - Inhalt des aktuellen Verzeichnisses auflisten mit `ls`
    - Dateien verschieben mit `mv`
    - In ein neues Verzeichnis verschieben mit `cd <path>`

    Einige Befehle und Argumente sind in PowerShell und BASH unterschiedlich. Weitere Informationen erhalten Sie, indem Sie [`get-help`](/powershell/scripting/learn/ps101/02-help-system?view=powershell-7) in PowerShell eingeben oder sich die [Kompatibilitätsaliase](/powershell/scripting/samples/appendix-1---compatibility-aliases?view=powershell-7) in der Dokumentation ansehen.

    Um PowerShell als Administrator auszuführen, geben Sie „PowerShell“ im Windows-Startmenü ein und wählen dann „Als Administrator ausführen“ aus.

2. **Windows-Befehlszeile (Cmd)** : Windows stellt nach wie vor die herkömmliche Eingabeaufforderung bereit (und Konsole – siehe unten), die Kompatibilität mit aktuellen und älteren MS-DOS-kompatiblen Befehlen und Batchdateien bietet. Cmd ist nützlich beim Ausführen von vorhandenen/älteren Batchdateien oder Befehlszeilenoperationen, aber im Allgemeinen wird Benutzern empfohlen, PowerShell zu erlernen und zu verwenden, da Cmd zwar noch unterstützt aber in Zukunft keine Verbesserungen oder neuen Features mehr erhalten wird.

### <a name="linux-shells"></a>Linux-Shells

Windows-Subsystem für Linux (WSL) kann jetzt installiert werden, um die Ausführung einer Linux-Shell innerhalb von Windows zu unterstützen. Das bedeutet, dass Sie **bash** direkt in Windows integriert ausführen können, ganz gleich, für welche Linux-Verteilung Sie sich entscheiden. Mithilfe von WSL wird eine Umgebung bereitgestellt, die Mac-Benutzern sehr vertraut ist. Beispielsweise werden Dateien in einem aktuellen Verzeichnis mit **ls** aufgelistet, nicht wie in der traditionellen Windows-Befehlsshell mit **dir**. Weitere Informationen zum Installieren und Verwenden von WSL findest du in [Windows-Subsystem für Linux: Installationsleitfaden für Windows 10](/windows/wsl/install-win10). Linux-Verteilungen, die unter Windows mit WSL installiert werden können, enthalten:

1. [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
2. [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
3. [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
4. [openSUSE Leap 15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
5. [SUSE Linux Enterprise Server 15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)

Um nur einige zu nennen. Weitere Informationen finden Sie in der [WSL-Installationsdokumentation](/windows/wsl/install-win10#install-your-linux-distribution-of-choice). Die Installation können Sie direkt aus den [Microsoft Store](https://www.microsoft.com/search/shop/apps?q=linux&category=Developer+tools) durchführen.

## <a name="windows-terminals"></a>Windows-Terminals

Zusätzlich zu vielen Angeboten von Drittanbietern stellt Microsoft zwei „Terminals“ zur Verfügung: GUI-Anwendungen, die Zugriff auf Befehlszeilenshells und Anwendungen bieten.

1. **[Windows-Terminal](/windows/terminal/)** : Windows-Terminal ist eine neue, moderne, hochgradig konfigurierbare Befehlszeilen-Terminalanwendung, die eine sehr hohe Leistung, eine Befehlszeilen-Benutzeroberfläche mit niedriger Latenz, mehrere Registerkarten, geteilte Fensterbereiche, benutzerdefinierte Designs und Stile, mehrere „Profile“ für verschiedene Shells oder Befehlszeilen-Apps sowie beträchtliche Möglichkeiten zur Konfiguration und Personalisierung zahlreicher Aspekte Ihrer Befehlszeilen-Benutzeroberfläche bietet.

    Sie können Windows-Terminal verwenden, um Registerkarten zu öffnen, die mit PowerShell, WSL-Shells (wie Ubuntu oder Debian), der traditionellen Windows-Eingabeaufforderung oder jeder anderen Befehlszeilen-App (z. B. SSH, Azure CLI, Git Bash) verbunden sind.

2. **[Konsole](/windows/console/)** : Unter Mac und Linux starten Benutzer normalerweise ihre bevorzugte Terminalanwendung, die dann die Standardshell des Benutzers (z. B. BASH) erstellt und sich mit ihr verbindet.

    Aufgrund einer Laune der Geschichte starten Windows-Benutzer jedoch traditionell ihre Shell, und Windows startet automatisch eine GUI-Konsolenanwendung und verbindet sie.

    Zwar kann man Shells auch direkt starten und die veraltete Windows-Konsole verwenden, aber es wird dringend empfohlen, stattdessen Windows-Terminal zu installieren und zu verwenden, um die beste, schnellste und produktivste Befehlszeilenumgebung zu erhalten.

## <a name="apps-and-utilities"></a>Apps und Hilfsprogramme

 **App** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Einstellungen und Präferenzen | Systemeinstellungen | Settings |
| Task-Manager | Aktivitätsmonitor | Task-Manager |
| Datenträgerformatierung | Datenträgerhilfsprogramm | Datenträgerverwaltung |
| Textbearbeitung | TextEdit | Editor |
| Ereignisanzeige | Konsole | Ereignisanzeige |
| Dateien/Apps suchen | COMMAND+LEERTASTE | WINDOWS-TASTE |