---
title: PowerToys fancyzones-Hilfsprogramm für Windows 10
description: Ein Fenster-Manager-Hilfsprogramm zum Anordnen und Ausrichten von Fenstern in effiziente Layouts
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4e85193aca4ffda4f863cca7321ca870a08694a3
ms.sourcegitcommit: 77af97719a439f5e73a6109b42fd3110bcb2843b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107219011"
---
# <a name="fancyzones-utility"></a>Fancyzones (Hilfsprogramm)

Fancyzones ist ein Fenster-Manager-Hilfsprogramm zum Anordnen und Ausrichten von Fenstern in effiziente Layouts, um die Geschwindigkeit des Workflows zu verbessern und Layouts schnell wiederherzustellen. Mit fancyzones kann der Benutzer eine Reihe von Fenster Standorten für einen Desktop definieren, die als Drag-Ziele für Windows dienen.  Wenn der Benutzer ein Fenster in eine Zone zieht, wird die Größe des Fensters geändert und neu positioniert, um diese Zone auszufüllen.  

![Screenshot von FancyZones](../images/pt-fancy-zones2.png)

## <a name="getting-started"></a>Erste Schritte

### <a name="enable"></a>Aktivieren

Um mit der Verwendung von fancyzones zu beginnen, müssen Sie das Hilfsprogramm in den PowerToys-Einstellungen aktivieren und dann die Benutzeroberfläche des fancyzones-Editors aufrufen.  

### <a name="launch-zones-editor"></a>Startzonen-Editor

Starten Sie den Zonen-Editor mithilfe der Schaltfläche im Menü PowerToys-Einstellungen oder durch Drücken von <kbd>Win</kbd> + <kbd>`</kbd> (Beachten Sie, dass diese Verknüpfung im Dialogfeld "Einstellungen" geändert werden kann).  

![Benutzeroberfläche für fancyzones-Einstellungen](../images/pt-fancyzones-settings.png) 

### <a name="elevated-permission-admin-apps"></a>Administrator-apps mit erhöhten Berechtigungen

Wenn Sie über Anwendungen verfügen, die mit erhöhten Rechten ausgeführt werden, führen Sie den Administrator Modus aus, lesen Sie [PowerToys und führen Sie als Administrator](administrator.md) aus.

## <a name="choose-your-layout-layout-editor"></a>Layout auswählen (Layouteditor)

Beim ersten Start zeigt der Zonen-Editor eine Liste von Layouts an, die anhand der Anzahl von Fenstern auf dem Monitor angepasst werden können. Wenn Sie ein Layout auswählen, wird eine Vorschau dieses Layouts auf dem Monitor angezeigt. Das ausgewählte Layout wird automatisch angewendet.  

![Bildschirm Abbildung der fancyzones-Auswahl](../images/pt-fancyzones-picker.png)

Wenn mehrere Anzeigen verwendet werden, erkennt der Editor die verfügbaren Monitore und zeigt Sie an, zwischen denen der Benutzer wählen kann. Der ausgewählte Monitor ist dann das Ziel des ausgewählten Layouts.

![Fancyzones-Auswahl mehrerer Monitore](../images/pt-fancyzones-multimon.png)

### <a name="space-around-zones"></a>Leerraum um Zonen

Mit der UMSCHALT **Fläche Raum um Zonen anzeigen** können Sie die Sortierreihenfolge oder den Rand jedes fancyzone-Fensters festlegen. Im Feld **Leerraum um Zonen** können Sie einen benutzerdefinierten Wert für die Breite des Rahmens festlegen.

Mithilfe der **Entfernung von angrenzenden Zonen** können Sie einen benutzerdefinierten Wert für den Leerraum zwischen den Fenstern "fancyzone" festlegen, bis Sie zusammengeführt werden, oder bevor beide hervorgehoben werden, sodass Sie zusammengeführt werden können.

Aktivieren und deaktivieren Sie den Zonen-Editor, und deaktivieren Sie das Feld **Leerzeichen um Zonen anzeigen** , nachdem Sie die Werte geändert haben, um den neuen Wert anzuzeigen.

![Bildschirm Abbildung von "fancyzones-Raum um Zonen"](../images/pt-fancyzones-spacearound.png)

### <a name="creating-a-custom-layout"></a>Erstellen eines benutzerdefinierten Layouts

Der Zonen-Editor unterstützt auch das Erstellen und Speichern von benutzerdefinierten Layouts. Klicken Sie unten rechts auf die Schaltfläche **Neues Layout erstellen** .
  
Es gibt zwei Möglichkeiten, benutzerdefinierte Zonen Layouts zu erstellen: **Raster** Layout und **Canvas** -Layout. Diese können auch als subtraktive und Additive Modelle angesehen werden.  

Das subtraktive **Raster** Modell beginnt mit einem drei spaltigen Raster und ermöglicht das Erstellen von Zonen, indem Zonen aufgeteilt und gemergt werden, sodass die Größe des Bundstegs zwischen den Zonen nach Belieben geändert wird.

Zum Zusammenführen von zwei Zonen aktivieren und halten Sie die linke Maustaste gedrückt, und ziehen Sie die Maus, bis eine zweite Zone ausgewählt ist, und geben Sie dann die Schaltfläche ein, und es wird ein Popup Menü angezeigt.

![Tabellen-Editor-Modus von fancyzones](../images/pt-fancyzones-grideditor.png)

Das Additive **Canvas** -Modell beginnt mit einem leeren Layout und unterstützt das Hinzufügen von Zonen, die ähnlich wie Windows gezogen und geändert werden können.

![Fenster-Editor-Modus "fancyzones"](../images/pt-fancyzones-canvaseditor.png)

### <a name="quickly-changing-between-layouts"></a>Schnelles Wechseln zwischen Layouts

Mit einem benutzerdefinierten Layout kann dieses Layout für einen benutzerdefinierten Hotkey konfiguriert werden, um es schnell auf den gewünschten Desktop anzuwenden. Der Hotkey kann festgelegt werden, indem das Menü Bearbeiten des benutzerdefinierten Layouts geöffnet wird. Sobald das benutzerdefinierte Layout festgelegt ist, kann es durch Drücken der Bindung angewendet werden `Win ⊞ + Ctrl + Alt + [hotkey]` . Das Layout kann auch durch Drücken der Hotkey-Taste angewendet werden, wenn ein Fenster gezogen wird.

In der folgenden Demo beginnen wir mit einer Standardvorlage, die auf den Bildschirm angewendet wird, und zwei benutzerdefinierten Layouts, für die wir Hotkeys zuweisen. Anschließend wird die `Win ⊞ + Ctrl + Alt + [hotkey]` Bindung verwendet, um das erste benutzerdefinierte Layout anzuwenden und ein Fenster zu binden. Zum Schluss wird das zweite benutzerdefinierte Layout angewendet, während ein Fenster gezogen und das Fenster an das Fenster gebunden wird.

![Fancyzones-Quick-Swap Layouts](../images/pt-fancyzones-quickswap.gif) 

## <a name="snapping-a-window-to-two-or-more-zones"></a>Ausrichten eines Fensters an zwei oder mehr Zonen

Wenn zwei Zonen nebeneinander liegen, kann ein Fenster an die Summe des Bereichs ausgerichtet werden (abgerundet auf das minimale Rechteck, das beide enthält). Wenn sich der Mauszeiger in der Nähe der allgemeinen Kante von zwei Zonen befindet, werden beide Zonen gleichzeitig aktiviert, sodass Sie das Fenster in beide Zonen ablegen können.  

Es ist auch möglich, eine beliebige Anzahl von Zonen zu aktivieren: ziehen Sie zunächst das Fenster, bis eine Zone aktiviert ist, und halten Sie die Taste gedrückt, `Control` während Sie das Fenster ziehen, um mehrere Zonen auszuwählen.

Wenn Sie ein Fenster mithilfe der Tastatur auf mehrere Zonen ausrichten möchten, aktivieren Sie zuerst die beiden Optionen `Override Windows Snap hotkeys (Win+Arrow) to move between zones` und `Move windows based on their position` . Nachdem Sie ein Fenster an eine Zone angeschlossen haben, können Sie mit <kbd>Win</kbd>  +  <kbd>Control</kbd>  +  <kbd>alt</kbd> + Pfeile das Fenster auf mehrere Zonen erweitern.

![Bildschirm Abbildung der zwei Zonen Aktivierung](../images/pt-fancyzones-twozones.png)

## <a name="shortcut-keys"></a>Tastenkombinationen

| Tastenkombination      | Aktion |
| ----------- | ----------- |
| Win ⊞ + \` | Mit der Taste + Graviszeichen (⊞ + \` ) von Windows wird der Editor gestartet (diese Verknüpfung kann im Dialogfeld "Einstellungen" bearbeitet werden). |
| Win ⊞ + nach-links/nach-rechts-Taste | Fokus Fenster zwischen Zonen verschieben (nur `Override Windows Snap hotkeys` , wenn die Einstellung aktiviert ist, in diesem Fall werden nur die `Windows ⊞ key + Left Arrow` und `Windows key ⊞ + Right Arrow` überschrieben, während die `Win ⊞ + Up Arrow` und `Win ⊞ + Down Arrow` weiterhin wie gewohnt funktionieren) |

Mit fancyzones wird Windows 10 nicht außer Kraft gesetzt `Win ⊞ + Shift + Arrow` , um schnell ein Fenster zu einem angrenzenden Monitor zu verschieben.

## <a name="settings"></a>Einstellungen

| Einstellung | Beschreibung |
| --------- | ------------- |
| Hotkey für den Zonen-Editor konfigurieren | Wenn Sie den Standard-Hotkey ändern möchten, klicken Sie auf das Textfeld (es ist nicht erforderlich, den Text auszuwählen oder zu löschen), und drücken Sie dann auf der Tastatur die gewünschte Tastenkombination. |
| Umschalttaste gedrückt halten, um Zonen beim Ziehen zu aktivieren | Schaltet zwischen dem automatischen Andock Modus und der UMSCHALTTASTE, wobei das Andocken während eines Drag-und manuellen Snap-Ins deaktiviert wird, wobei beim Drücken der UMSCHALTTASTE während eines Zieh Vorgangs Andocken aktiviert ist. |
| Verwenden einer nicht primären Maustaste zum Umschalten der Zonen Aktivierung | Wenn diese Option aktiviert ist, wird durch Klicken auf eine nicht primäre Maustaste die Zonen Aktivierung gewechselt. |
| Windows-Snap-in-Hotkeys überschreiben (Win ⊞ + Pfeil), um zwischen Zonen zu wechseln | Wenn diese Option auf on und fancyzones ausgeführt wird, überschreibt Sie zwei Windows-Snap-Keys: `Win ⊞ + Left Arrow` und `Win ⊞ + Right Arrow` |
| Verschieben von Fenstern basierend auf ihrer Position | Ermöglicht die Verwendung von Win ⊞ + nach oben/nach unten/rechts/nach-links-Pfeilen zum Ausrichten eines Fensters auf der Grundlage seiner Position relativ zum Zonen Layout. |
| Verschieben von Fenstern zwischen Zonen auf allen Monitoren | Wenn diese Option deaktiviert ist, wird durch das Ausrichten mit Win ⊞ + Pfeil das Fenster durch die Zonen auf dem aktuellen Monitor gezogen, wenn aktiviert ist, wird das Fenster durch alle Zonen auf allen Monitoren gezogen. |
| Wenn sich die Bildschirmauflösung ändert, behalten Sie Windows in ihren Zonen bei. | Wenn diese Einstellung aktiviert ist, wird die Größe der Bildschirmauflösung geändert, wenn diese Einstellung aktiviert ist. |
| Bei Änderungen am zonenlayout entspricht Windows, das einer Zone zugewiesen ist, neuen Größe/Positionen. | Wenn diese Option aktiviert ist, ändern sich die Größe und Position von Fenstern in das neue Zonen Layout, indem Sie den Speicherort der vorherigen Zonennummer jedes Fensters beibehalten. |
| Verschieben neu erstellter Fenster in die letzte bekannte Zone | Automatisches Verschieben eines neu geöffneten Fensters in den letzten zonenspeicherort, in dem sich die Anwendung befand |
| Verschieben neu erstellter Fenster auf den aktuellen aktiven Monitor [experimentell] | Wenn diese Option aktiviert ist und das Verschieben von neu erstellten Fenstern in die letzte bekannte Zone deaktiviert ist, oder wenn die Anwendung nicht über eine letzte bekannte Zone verfügt, wird die Anwendung auf dem aktuellen aktiven Monitor beibehalten. |
| Wiederherstellen der ursprünglichen Größe von Fenstern beim Aufheben der Wiederherstellung | Wenn diese Option aktiviert ist, wird die Größe des Fensters beim Aufheben der Fenstergröße wieder hergestellt. |
| Wenn Sie den Editor in einer Umgebung mit mehreren Monitoren starten, folgen Sie dem Mauszeiger und dem Fokus. | Wenn diese Option aktiviert ist, startet der Editor-Hotkey den Editor auf dem Monitor, auf dem sich der Mauszeiger befindet. wenn diese Option deaktiviert ist, startet der Editor-Hotkey den Editor auf dem Monitor, auf dem das aktuelle aktive Fenster ist.  |
| Beim Ziehen eines Fensters Zonen auf allen Monitoren anzeigen | Standardmäßig zeigt fancyzones nur die Zonen an, die auf dem aktuellen Monitor verfügbar sind. diese Funktion hat möglicherweise Auswirkungen auf die Leistung, wenn aktiviert. |
| Zonen übergreifende überwachen zulassen (alle Monitore müssen über die gleiche DPI-Skalierung verfügen) | Mit dieser Option können alle verbundenen Monitore als ein großer Bildschirm behandelt werden. Um ordnungsgemäß zu funktionieren, muss der gleiche dpi-Skalierungsfaktor von allen Monitoren verwendet werden. |
| Ziehen des gezogenen Fensters transparent | Wenn die Zonen aktiviert sind, wird das gezogene Fenster transparent gemacht, um die Sichtbarkeit der Zonen zu verbessern. |
| Farbe für Zonen Hervorhebung (Standard #008CFF) | Die Farbe, zu der eine Zone wird, wenn Sie beim Ziehen des Fensters das aktive Ablage Ziel ist. |
| Inaktive Zonen Farbe (Standard #F5FCFF) | Die Farbe, die Zonen werden, wenn Sie während eines Fenster Zieh Vorgangs nicht aktiv sind. |
| Zonen Rahmenfarbe (Standard #FFFFFF) | Die Farbe des Rahmens von aktiven und inaktiven Zonen. |
| Zonen Deckkraft (%) (Standard 50%) | Der Prozentsatz der Deckkraft aktiver und inaktiver Zonen |
| Ausschließen von Anwendungen vom ausrichten in Zonen | Fügen Sie den Anwendungsnamen oder einen Teil des Namens (eine pro Zeile) hinzu (z. b. wird das Hinzufügen von `Notepad` sowohl `Notepad.exe` mit als auch mit übereinstimmen `Notepad++.exe` `Notepad.exe` `.exe` ). |

![Bildschirm Abbildung von "fancyzones" unten](../images/pt-fancyzones-settings2.png)
