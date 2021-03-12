---
title: PowerToys Run Utility für Windows 10
description: Ein schnell Startprogramm für Poweruser, das einige zusätzliche Features ohne Leistungseinbußen enthält.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5b6f86636cb5753d93422a5658e6ae661151d0f9
ms.sourcegitcommit: eb203b55b1332d0ed135abccd50f3fc287f89a5a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103193295"
---
# <a name="powertoys-run-utility"></a>PowerToys-Hilfsprogramm ausführen

PowerToys Run ist ein schnelles Start Programm für Hauptbenutzer, das einige zusätzliche Features enthält, ohne die Leistung zu beeinträchtigen. Das Programm ist Open Source und für weitere Plug-Ins modular ausgelegt.

Um PowerToys auszuführen, wählen Sie <kbd>alt</kbd> + <kbd>LEERTASTE</kbd> aus, und beginnen Sie mit der Eingabe.

*Wenn diese Verknüpfung nicht Ihren Erwartungen entspricht, machen Sie sich keine Sorgen, Sie ist in den Einstellungen vollständig konfigurierbar.*

![PowerToys führt demoöffnende Apps aus](../images/pt-powerrun-demo.gif)

## <a name="requirements"></a>Anforderungen

- Windows 10, Version 1903 oder höher
- Nach der Installation von muss PowerToys aktiviert und im Hintergrund ausgeführt werden, damit dieses Hilfsprogramm funktioniert.

## <a name="features"></a>Features

Die PowerToys-Run-Funktionen umfassen Folgendes:

- Suchen nach Anwendungen, Ordnern oder Dateien

- Suchen nach ausgelaufenden Prozessen (früher als " [windowwalker](https://github.com/betsegaw/windowwalker/)" bezeichnet)

- Klick Bare Schaltflächen mit Tastenkombinationen (z. b. *Öffnen als Administrator* oder *Öffnen enthaltender Ordner*)

- Aufrufen des Shell-Plug- `>`  ins mithilfe von (öffnet z. b. `> Shell:startup` den Windows-Startordner)

- Ausführen einer einfachen Berechnung mithilfe des Rechners

## <a name="settings"></a>Einstellung

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
  | Esc | PowerToys-Run ausblenden |
  | STRG+UMSCHALT+EINGABE | (Gilt nur für Anwendungen) Ausgewählte Anwendung als Administrator öffnen |
  | Strg+Umschalt+E | (Gilt nur für Anwendungen und Dateien) Enthaltenden Ordner in Datei-Explorer öffnen |
  | STRG+C | (Gilt nur für Ordner und Dateien) Pfad für Kopier Pfad |
  | Registerkarte | Navigieren durch die Schaltflächen "Suchergebnis" und "Kontextmenü" |

## <a name="action-key"></a>Aktions Schlüssel

Diese werden erzwingen, dass PowerToys nur als Ziel-Plug-Ins ausgeführt wird.

  | **Aktions Schlüssel** | **Aktion** |
  | --- | --- |
  | `=` | Nur Rechner. Beispiel: `=2+2` |
  | `?` | Nur Dateisuche. Beispiel `?road` für die Suche `roadmap.txt` |
  | `.` | Nur installierte App-Suche. Beispiel `.code` für das Visual Studio Code |
  | `//` | Nur URLs. Beispiel `//docs.microsoft.com` für das Wechseln Ihres Standard Browsers zu https://docs.microsoft.com |
  | `<` | Nur ausführen von Prozessen. Beispiel `<outlook` für die Suche nach allen Prozessen, die Outlook enthalten |
  | `>` | Nur Shellbefehl. Beispiel `>ping localhost` für eine Ping-Abfrage |
  | `:` | Nur Registrierungsschlüssel. Beispiel `:hkcu` für die Suche nach dem Registrierungsschlüssel HKEY_CURRENT_USER |
  | `!` | Nur Windows-Dienste. Beispiel für `!alg` die Suche nach dem anwendungsebenengateway-Dienst, der gestartet oder beendet werden soll |

## <a name="system-commands"></a>System Befehle

Mit PowerToys v 0,31 und on gibt es Aktionen auf Systemebene, die Sie jetzt ausführen können.

  | **Aktions Schlüssel**   |   **Aktion** |
  | ------------------ | ---------------------------------------------------------------------------------|
  | `Shutdown` | Fährt den Computer herunter. |
  | `Restart` | Startet den Computer neu. |
  | `Sign Out` | Meldet den aktuellen Benutzer ab. |
  | `Lock` | Sperrt den Computer. |
  | `Sleep` | Computer im Ruhezustand |
  | `Hibernate` | Ruhezustand des Computers |
  | `Empty Recycle Bin` | Leert den Papierkorb. |

## <a name="indexer-settings"></a>Indexer-Einstellungen

Wenn die Indexer-Einstellungen nicht alle Laufwerke abdecken, wird die folgende Warnung angezeigt:

![PowerToys-Warnung zum Ausführen des Indexers](../images/pt-run-warning.png)

Sie können die Warnung in den PowerToys-Einstellungen deaktivieren oder die Warnung auswählen, um zu erweitern, welche Laufwerke indiziert werden. Nachdem Sie die Warnung ausgewählt haben, werden die Optionen Windows 10-Einstellungen "Windows-Suche" geöffnet.

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
