---
title: Hilfe beim Wechsel von Mac (Unix) zu Windows
description: Eine Anleitung, die Sie beim Übergang von einem Mac (Unix) zu einer Windows-Entwicklungsumgebung unterstützt, einschließlich Tastenkombinationen und einer kurzen Übersicht über die Unterschiede zwischen Mac und Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac zu Windows, Tastenkombination für Tastenkombinationen, Wechsel von UNIX zu Windows, Übergang von Mac zu Windows, Unterstützung der Umstellung von MacBook auf die Oberfläche, Verwendung von Windows für einen Macintosh-Benutzer, Wechsel von Macintosh zu Windows, Hilfe beim Ändern von Entwicklungsumgebungen, Mac OS X zu Windows, Hilfe Wechsel von Mac zu PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: f72e688417726d3c3193d831dc886f6a33ec98f8
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315324"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Leitfaden zum Ändern der Entwicklungsumgebung von Mac zu Windows

Die folgenden Tipps und Steuerelement äquivalente sollten Sie bei der Umstellung zwischen einer Mac-und Windows-Entwicklungsumgebung (oder einer WSL/Linux) unterstützen.

Bei der APP-Entwicklung wäre das nächste Äquivalent zu Xcode [Visual Studio](https://visualstudio.microsoft.com). Es gibt auch eine Version von [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/), wenn Sie jemals wieder zurückkehren müssen. Für die plattformübergreifende Code Bearbeitung (und eine große Anzahl von Plug-ins) ist [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) die häufigste Wahl.

## <a name="keyboard-shortcuts"></a>Tastenkombinationen

| **Vorgang** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Kopieren | Befehl + C | CTR + C |
| Ausschneiden | Befehl + X | CTR + X |
| Einfügen | Befehl + V | CTR + V |
| Rückgängig machen | Befehl + Z | STRG+Z |
| Speichern | Befehl + S | STRG+S |
| Öffnen | Befehl + O | STRG+O |
| Computer sperren | Befehl + Steuerung + Q | CTR + L |
| Desktop anzeigen | Befehl + F3 | STRG + D |
| Minimieren von Fenstern | Windows-Taste + M | BEFEHL + M |
| Suchen | Befehl + Leertaste | Windows-Taste |
| Aktives Fenster schließen | Befehl + W | Steuerelement + W |
| Aktuellen Task wechseln | Befehl + Tab | ALT+TAB |
| Bildschirm speichern (Screenshot) | Befehl + Umschalt + 3 | Windows + UMSCHALT + S |
| Fenster speichern | Befehl + Umschalt + 4 | Windows + UMSCHALT + S |
| Element Informationen oder Eigenschaften anzeigen | Befehl + I | ALT + EINGABETASTE |
 | Alle Elemente auswählen | Befehl + A | STRG+A |
| Wählen Sie mehr als ein Element in einer Liste aus (nicht zusammenhängend). | , Und klicken Sie dann auf die einzelnen Elemente. | Steuerelement, und klicken Sie dann auf jedes Element |
| Sonderzeichen eingeben | Option + Zeichen Taste | ALT + Zeichen Taste|

## <a name="trackpad-shortcuts"></a>Trackpad-Tastenkombinationen

Hinweis: Einige dieser Tastenkombinationen erfordern eine "Genauigkeits Trackpad", z. b. die Trackpad-Geräte auf Surface-Geräten und andere Laptops von Drittanbietern.

 **Vorgang** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | Vertikaler Schwenken mit zwei Fingern | Vertikaler Schwenken mit zwei Fingern |
| Zoom | Zwei Finger ein-und ausgehend | Zwei Finger ein-und ausgehend |
| Rückwärts-und Vorwärtsbewegung zwischen Ansichten | Zwei Finger seitwärts schwenken | Zwei Finger seitwärts schwenken |
| Virtuelle Arbeitsbereiche wechseln | Vier Finger seitwärts schwenken | Vier Finger seitwärts schwenken |
| Zurzeit geöffnete apps anzeigen | Vier Finger nach oben schwenken | Drei Finger nach oben schwenken |
| Wechseln zwischen Apps | Nicht zutreffend | Langsamste drei Finger seitwärts schwenken |
| Zum Desktop wechseln | Verteilen von vier Fingern | Drei Finger schwenken nach unten |
| Cortana/Aktions Center öffnen | Zwei Finger Folie von rechts | Drei Finger Tippen |
| Zusätzliche Informationen öffnen | Drei Finger Tippen | Nicht zutreffend |
|Launchpad anzeigen/app starten | Mit vier Fingern | Tippen Sie mit vier Fingern |

Hinweis: Trackpad-Optionen können auf beiden Plattformen konfiguriert werden.

## <a name="terminal-and-shell"></a>Terminal und Shell

Windows stellt verschiedene Alternativen zum Terminal Emulator des Macs bereit.

1. Die Windows-Befehlszeile

Die Windows-Befehlszeile akzeptiert DOS-Befehle und ist das am häufigsten verwendete Befehlszeilen Tool unter Windows. So öffnen Sie es: Drücken Sie **Windows + R** , um das Feld **Ausführen** zu öffnen, geben Sie **cmd** ein, und klicken Sie dann auf **OK**. Um eine Administrator Befehlszeile zu öffnen, geben Sie **cmd** ein, und drücken **Sie STRG + UMSCHALT + Eingabe**Taste. 

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) ist eine "PowerShell ist eine aufgabenbasierte Befehlszeilenshell und Skriptsprache, die auf .NET basiert. Mit PowerShell können Systemadministratoren und Power-Benutzer schnell Aufgaben automatisieren, die Betriebssysteme verwalten. Dies bedeutet, dass es sich um eine sehr leistungsfähige Befehlszeile handelt, die von Systemadministratoren besonders beliebt ist.

Übrigens ist PowerShell [auch für Mac verfügbar](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. Windows-Subsystem für Linux (WSL)

Mit WSL können Sie eine Linux-Shell innerhalb von Windows ausführen. Dies bedeutet, dass Sie *bash** oder eine andere Shell ausführen können, abhängig von der Auswahl und der spezifischen installierten Linux-Distribution. Mithilfe von WSL wird die Art der Umgebung bereitgestellt, die den Mac-Benutzern am meisten vertraut ist. Beispielsweise können Sie die Dateien in einem aktuellen Verzeichnis und **nicht in der** Windows- Befehlszeile auflisten. Weitere Informationen zum Installieren und Verwenden von WSL finden Sie im [Windows-Subsystem für Linux-Installationshandbuch für Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

## <a name="apps-and-utilities"></a>Apps und Hilfsprogramme

 **App** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Einstellungen und Einstellungen | System Einstellungen | Einstellungen |
| Task-Manager | Aktivitäts Monitor | Task-Manager |
| Datenträger Formatierung | Datenträger Dienstprogramm | Datenträgerverwaltung |
| Text Bearbeitung | TextEdit | Editor |
| Ereignisanzeige | Console | Ereignisanzeige |
| Suchen nach Dateien/apps | Befehl + Leertaste | Windows-Taste |
