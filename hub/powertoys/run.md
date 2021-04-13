---
title: PowerToys Run Utility für Windows 10
description: Ein schnell Startprogramm für Poweruser, das einige zusätzliche Features ohne Leistungseinbußen enthält.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d9c67b38f9a7c7729c0f4839a327a2527aa116d
ms.sourcegitcommit: 77af97719a439f5e73a6109b42fd3110bcb2843b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107218137"
---
# <a name="powertoys-run-utility"></a>PowerToys-Hilfsprogramm ausführen

PowerToys Run ist ein schnelles Start Programm für Hauptbenutzer, das einige zusätzliche Features enthält, ohne die Leistung zu beeinträchtigen. Das Programm ist Open Source und für weitere Plug-Ins modular ausgelegt.

Um PowerToys auszuführen, wählen Sie <kbd>alt</kbd> + <kbd>LEERTASTE</kbd> aus, und beginnen Sie mit der Eingabe.

*Wenn diese Verknüpfung nicht Ihren Erwartungen entspricht, machen Sie sich keine Sorgen, Sie ist in den Einstellungen vollständig konfigurierbar.*

![PowerToys führt demoöffnende Apps aus](../images/pt-powerrun-demo.gif)

## <a name="requirements"></a>Requirements (Anforderungen)

- Windows 10, Version 1903 oder höher
- Nach der Installation von muss PowerToys aktiviert und im Hintergrund ausgeführt werden, damit dieses Hilfsprogramm funktioniert.

## <a name="features"></a>Features

Die PowerToys-Run-Funktionen umfassen Folgendes:

- Suchen nach Anwendungen, Ordnern oder Dateien

- Suchen nach ausgelaufenden Prozessen (früher als " [windowwalker](https://github.com/betsegaw/windowwalker/)" bezeichnet)

- Klick Bare Schaltflächen mit Tastenkombinationen (z. b. *Öffnen als Administrator* oder *Öffnen enthaltender Ordner*)

- Aufrufen des Shell-Plug- `>`  ins mithilfe von (öffnet z. b. `> Shell:startup` den Windows-Startordner)

- Ausführen einer einfachen Berechnung mithilfe des Rechners

## <a name="settings"></a>Einstellungen

Die folgenden Test Lauf Optionen sind im Menü PowerToys-Einstellungen verfügbar.

  | **Einstellungen** |**Aktion** |
  | --- | --- |
  | PowerToys-Testlauf öffnen | Tastenkombination zum Öffnen/Ausblenden von PowerToys-Laufwerken definieren |
  | Verknüpfungen im Vollbildmodus ignorieren |  Wenn der Befehl im Vollbildmodus (F11) nicht mit der Verknüpfung ausgeführt wird |
  | Maximale Anzahl von Ergebnissen |  Maximale Anzahl von Ergebnissen, die ohne Scrollen angezeigt werden |
  | Vorherige Abfrage beim Starten löschen | Beim Starten werden vorherige Suchvorgänge nicht hervorgehoben. |
  | Warnung zur Laufwerk Erkennung deaktivieren | Wenn alle Laufwerke nicht indiziert sind, wird die Warnung nicht mehr angezeigt. |

## <a name="keyboard-shortcuts"></a>Tastenkombinationen

  | **Verknüpfungen** | **Aktion** |
  | --- | --- |
  | ALT + LEERTASTE | PowerToys-Testlauf öffnen oder ausblenden |
  | ESC | PowerToys-Run ausblenden |
  | STRG+UMSCHALT+EINGABE | (Gilt nur für Anwendungen) Ausgewählte Anwendung als Administrator öffnen |
  | Strg+Umschalt+E | (Gilt nur für Anwendungen und Dateien) Enthaltenden Ordner in Datei-Explorer öffnen |
  | STRG+C | (Gilt nur für Ordner und Dateien) Pfad für Kopier Pfad |
  | Registerkarte | Navigieren durch die Schaltflächen "Suchergebnis" und "Kontextmenü" |

## <a name="action-keys"></a>Aktionstasten

Diese Standard Aktivierungs Ausdrücke erzwingen, dass PowerToys nur Ziel-Plug-Ins ausgeführt wird.

  | **Aktions Schlüssel** | **Aktion** |
  | --- | --- |
  | `=` | Nur Rechner. Beispiel: `=2+2`. |
  | `?` | Nur Dateisuche. Ein Beispiel `?road` für die Suche `roadmap.txt` . |
  | `.` | Nur installierte Programme. Beispiel `.code` für das Visual Studio Code. Optionen zum Hinzufügen von Parametern zum Start eines Programms finden Sie unter [Programm Parameter](#program-parameters) . |
  | `//` | Nur URLs. Beispiel `//docs.microsoft.com` für den Standardbrowser https://docs.microsoft.com . |
  | `<` | Nur ausführen von Prozessen. Beispiel `<outlook` für die Suche nach allen Prozessen, die Outlook enthalten. |
  | `>` | Nur Shellbefehl. Beispiel `>ping localhost` für eine Ping-Abfrage. |
  | `:` | Nur Registrierungsschlüssel. Beispiel `:hkcu` für die Suche nach dem Registrierungsschlüssel HKEY_CURRENT_USER. |
  | `!` | Nur Windows-Dienste. Beispiel für `!alg` die Suche nach dem anwendungsebenengatewaydienst, der gestartet oder beendet werden soll. |
  | `{` | Visual Studio Code zuvor geöffnete Arbeitsbereiche, Remote Computer (SSH oder codespaces) und Container. Beispiel `{powertoys` für die Suche nach Arbeitsbereichen, die ' PowerToys ' in ihren Pfaden enthalten. Dieses Plug-in ist standardmäßig deaktiviert.

## <a name="system-commands"></a>System Befehle

Die Ausführung von PowerToys ermöglicht eine Reihe von Aktionen auf Systemebene, die ausgeführt werden können.

  | **Aktions Schlüssel**   |   **Aktion** |
  | ------------------ | ---------------------------------------------------------------------------------|
  | `Shutdown` | Fährt den Computer herunter. |
  | `Restart` | Startet den Computer neu. |
  | `Sign Out` | Meldet den aktuellen Benutzer ab. |
  | `Lock` | Sperrt den Computer. |
  | `Sleep` | Computer im Ruhezustand |
  | `Hibernate` | Ruhezustand des Computers |
  | `Empty Recycle Bin` | Leert den Papierkorb. |

## <a name="plugin-manager"></a>Plugin-Manager

Das Menü PowerToys-Lauf Zeit Einstellungen enthält einen Plug-in-Manager, mit dem Sie die verschiedenen derzeit verfügbaren Plug-ins aktivieren bzw. deaktivieren können Wenn Sie die Abschnitte auswählen und erweitern, können Sie die von den einzelnen Plug-ins verwendeten Aktivierungs Ausdrücke anpassen. Außerdem können Sie auswählen, ob ein Plug-in globale Ergebnisse angezeigt wird, und zusätzliche Plug-in-Optionen festlegen, sofern verfügbar. 

## <a name="program-parameters"></a>Programm Parameter

Das PowerToys Run program-Plug-in ermöglicht das Hinzufügen von Programm Argumenten, wenn eine Anwendung gestartet wird. Die Programm Argumente müssen das erwartete Format aufweisen, wie von der Befehlszeilenschnittstelle des Programms definiert.

Wenn Sie z. b. Visual Studio Code starten, können Sie den Ordner angeben, der geöffnet werden soll:

`Visual Studio Code -- C:\myFolder`

Visual Studio Code unterstützt auch eine Reihe von [Befehlszeilen Parametern](https://code.visualstudio.com/docs/editor/command-line), die mit den entsprechenden Argumenten in PowerToys ausgeführt werden können, um beispielsweise den Unterschied zwischen Dateien anzuzeigen:

`Visual Studio Code -d C:\foo.txt C:\bar.txt` 

Wenn die Option "in globales Ergebnis einschließen" des Programm-Plug-ins nicht ausgewählt ist, müssen Sie den Aktivierungs Ausdruck `.` standardmäßig einschließen, um das Verhalten des Plug-ins aufzurufen:

`.Visual Studio Code -- C:\myFolder`

## <a name="monitor-positioning"></a>Überwachen der Positionierung

Wenn mehrere Monitore verwendet werden, kann die Ausführung von PowerToys auf dem gewünschten Monitor gestartet werden, indem das entsprechende Startverhalten im Menü Einstellungen konfiguriert wird. Zu den Optionen gehören das Öffnen für:

- Primärer Monitor
- Überwachen mit dem Mauszeiger
- Überwachen mit Fokus Fenster

![Auswahl von PowerToys-Monitor ausführen](../images/pt-run-monitor.png)

## <a name="windows-search-settings"></a>Windows-Sucheinstellungen

Wenn das Windows Search-Plug-in nicht für alle Laufwerke festgelegt ist, wird die folgende Warnung angezeigt:

![PowerToys-Warnung zum Ausführen des Indexers](../images/pt-run-warning.png)

Sie können die Warnung in den PowerToys-Optionen zum Ausführen des Plug-in-Managers für Windows Search deaktivieren oder die Warnung auswählen, um zu erweitern, welche Laufwerke indiziert werden. Nachdem Sie die Warnung ausgewählt haben, wird das Optionsmenü "Windows 10-Einstellungen Durchsuchen" geöffnet.

![Indizierungs Einstellungen](../images/pt-run-indexing.png)

In diesem Menü "Suchfenster" können Sie folgende Aktionen ausführen:

- Wählen Sie den Modus "Erweitert" aus, um die Indizierung für alle Laufwerke auf Ihrem Windows 10-Computer zu aktivieren.
- Geben Sie die auszuschließenden Ordner Pfade an.
- Wählen Sie die "erweiterten Suchindexer-Einstellungen" (unten in den Menü Optionen) aus, um erweiterte Index Einstellungen festzulegen, Such Speicherorte hinzuzufügen oder zu entfernen, verschlüsselte Dateien zu indizieren usw.

![Erweiterte Indizierungs Einstellungen](../images/pt-run-indexing-advanced.png)

## <a name="known-issues"></a>Bekannte Probleme

Eine Liste aller bekannten Probleme und Vorschläge finden Sie [in den PowerToys-produktrepository-Problemen auf GitHub](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3AProduct-Launcher).

## <a name="attribution"></a>Nung

- [Wox](https://github.com/Wox-launcher/Wox/)

- [Fenster Walker von Beta tadele](https://github.com/betsegaw/windowwalker)
