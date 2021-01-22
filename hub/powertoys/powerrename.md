---
title: PowerToys-PowerToys-Hilfsprogramm für Windows 10
description: Eine Windows-Shellerweiterung für das Massen Umbenennen von Dateien
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 39e06685b6948ed3d3935c69a8b4dafeb9ecc2ea
ms.sourcegitcommit: 8040760f5520bd1732c39aedc68144c4496319df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98691335"
---
# <a name="powerrename-utility"></a>Powerrename-Hilfsprogramm

Powerrename ist ein Tool zum Massen umbenennen, mit dem Sie Folgendes ausführen können:

- Ändern Sie die Dateinamen einer großen Anzahl von Dateien *(ohne Umbenennen aller Dateien denselben Namen)*.
- Führen Sie eine Suche durch, und ersetzen Sie Sie in einem Ziel Abschnitt von Dateinamen.
- Führt eine Umbenennung eines regulären Ausdrucks für mehrere Dateien aus.
- Überprüfen Sie die erwarteten Umbenennungs Ergebnisse in einem Vorschau Fenster, bevor Sie eine Massen Umbenennung abschließen.
- Macht einen Umbenennungs Vorgang rückgängig, nachdem er abgeschlossen wurde.

## <a name="demo"></a>Demo

In dieser Demo werden alle Instanzen des Datei namens "Pampalona" durch "Pamplona" ersetzt. Da alle Dateien eindeutig benannt werden, würde dies eine lange Zeit in Anspruch genommen haben. Powerrename ermöglicht ein einzelnes Massen umbenennen. Beachten Sie, dass der Befehl "rückgängig umbenennen" (STRG + Z) die Möglichkeit ermöglicht, die Änderung rückgängig zu machen.

![Powerrename-Demo](../images/powerrename-demo.gif)

## <a name="powerrename-menu"></a>Menü "powerrename"

Nachdem Sie einige Dateien im Windows-Datei-Explorer ausgewählt haben, klicken Sie mit der rechten Maustaste darauf, und wählen Sie " *powerrename* " (wird nur angezeigt, wenn Sie in PowerToys aktiviert ist). Die Anzahl der Elemente (Dateien), die Sie ausgewählt haben, wird zusammen mit Such-und Ersetzungs Werten, einer Liste von Optionen und einem Vorschaufenster angezeigt, in dem die Ergebnisse der von Ihnen eingegebenen Such-und Ersetzungs Werte angezeigt werden.

![Bildschirm Abbildung von powerrename-Menü](../images/powerrename-menu.png)

### <a name="search-for"></a>Suchen nach

Geben Sie Text oder einen [regulären Ausdruck](https://wikipedia.org/wiki/Regular_expression) ein, um die Dateien in Ihrer Auswahl zu suchen, die die Kriterien für Ihren Eintrag enthalten. Die übereinstimmenden Elemente werden im *Vorschaufenster* angezeigt.

### <a name="replace-with"></a>Ersetzen durch

Geben Sie Text ein, *um den zuvor eingegebenen Wert zu ersetzen* , der den ausgewählten Dateien entspricht. Der ursprüngliche Dateiname und die umbenannte Datei können im *Vorschaufenster* angezeigt werden.

### <a name="options---use-regular-expressions"></a>Optionen: reguläre Ausdrücke verwenden

Wenn dieses Kontrollkästchen aktiviert ist, wird der Suchwert als [regulärer Ausdruck](https://wikipedia.org/wiki/Regular_expression) (Regex) interpretiert. Der Wert Replace kann auch Regex-Variablen enthalten (siehe Beispiele unten).  Wenn diese Option nicht aktiviert ist, wird der Suchwert als Klartext interpretiert, um durch den Text im Feld ersetzen ersetzt zu werden.

### <a name="options---case-sensitive"></a>Optionen: Groß-/Kleinschreibung

Wenn dieses Kontrollkästchen aktiviert ist, stimmt der im Suchfeld angegebene Text nur mit Text in den Elementen überein, wenn der Text dem gleichen Fall entspricht. Die Groß-/Kleinschreibung wird standardmäßig nicht beachtet (keine Unterschiede zwischen Groß-und Kleinbuchstaben erkannt).

### <a name="options---match-all-occurrences"></a>Optionen: alle Vorkommen vergleichen

Wenn dieses Kontrollkästchen aktiviert ist, werden alle Übereinstimmungen von Text im Suchfeld durch den Ersetzungstext ersetzt.  Andernfalls wird nur die erste Instanz der Suche nach Text im Dateinamen ersetzt (von links nach rechts).

Angenommen, der Dateiname `powertoys-powerrename.txt` lautet:

- Suchen nach: `power`
- Umbenennen mit: `super`

Der Wert der umbenannten Datei führt zu folgenden Ergebnissen:

- Alle Vorkommen abgleichen (nicht aktiviert): `supertoys-powerrename.txt`
- Alle Vorkommen vergleichen (aktiviert): `supertoys-superrename.txt`

### <a name="options---exclude-files"></a>Optionen: Ausschließen von Dateien

Dateien werden nicht in den Vorgang eingeschlossen. Nur Ordner werden eingeschlossen.

### <a name="options---exclude-folders"></a>Optionen-Ordner ausschließen

Ordner werden nicht in den Vorgang eingeschlossen. Es werden nur Dateien eingeschlossen.

### <a name="options---exclude-subfolder-items"></a>Optionen-Unterordner Elemente ausschließen

Elemente in Ordnern werden nicht in den Vorgang eingeschlossen. Standardmäßig sind alle Unterordner Elemente enthalten.

### <a name="options---enumerate-items"></a>Optionen-Elemente aufzählen

Fügt einem Dateinamen, die im-Vorgang geändert wurden, ein numerisches Suffix hinzu. Beispiel: `foo.jpg` -> `foo (1).jpg`

### <a name="options---item-name-only"></a>Optionen-nur Element Name

Nur der Dateinamen Teil (nicht die Dateierweiterung) wird durch den Vorgang geändert. Beispiel: `txt.txt` ->  `NewName.txt`

### <a name="options---item-extension-only"></a>Optionen-nur Element Erweiterung

Nur der Datei Erweiterungs Teil (nicht der Dateiname) wird durch den Vorgang geändert. Beispiel: `txt.txt` -> `txt.NewExtension`

## <a name="replace-using-file-creation-date-and-time"></a>Ersetzen Sie dies durch Datum und Uhrzeit der Dateierstellung.

Die Attribute für Erstellungsdatum und-Uhrzeit einer Datei können im Text *ersetzen* durch Eingabe eines Variablen Musters entsprechend der folgenden Tabelle verwendet werden.

Variablenmuster |Erklärung
|:---|:---|
|`$YYYY`|Jahr, das durch eine vollständige vier oder fünf Ziffern dargestellt wird, je nach verwendetem Kalender.
|`$YY`|Jahr, das nur durch die letzten zwei Ziffern dargestellt wird. Für einstellige Jahre wird eine führende Null hinzugefügt.
|`$Y`|Jahr, das nur durch die letzte Ziffer dargestellt wird.
|`$MMMM`|Name des Monats
|`$MMM`|Abgekürzte Name des Monats
|`$MM`|Monat als Ziffern mit führenden Nullen für einstellige Monate.
|`$M`|Monat als Ziffern ohne führende Nullen für einstellige Monate.
|`$DDDD`|Name des Wochentags
|`$DDD`|Abgekürzte Name des Wochentags
|`$DD`|Tag des Monats als Ziffern mit führenden Nullen für Einstellige Tage.
|`$D`|Tag des Monats als Ziffern ohne führende Nullen für Einstellige Tage.
|`$hh`|Stunden mit führenden Nullen für Einstellige Stunden
|`$h`|Stunden ohne führende Nullen für Einstellige Stunden
|`$mm`|Minuten mit führenden Nullen für Einstellige Minuten.
|`$m`|Minuten ohne führende Nullen für Einstellige Minuten.
|`$ss`|Sekunden mit führenden Nullen für Einstellige Sekunden.
|`$s`|Sekunden ohne führende Nullen für Einstellige Sekunden.
|`$fff`|Millisekunden, die durch vollständige drei Ziffern dargestellt werden.
|`$ff`|Millisekunden, die nur durch die ersten beiden Ziffern dargestellt werden.
|`$f`|Millisekunden, die nur durch die erste Ziffer dargestellt werden.

Beispielsweise mit den Dateinamen:

- `powertoys.png`, erstellt am 11/02/2020
- `powertoys-menu.png`, erstellt am 11/03/2020

Geben Sie die Kriterien ein, um die Elemente umzubenennen:

- Suchen nach: `powertoys`
- Umbenennen mit: `$MMM-$DD-$YY-powertoys`

Der Wert der umbenannten Datei führt zu folgenden Ergebnissen:

- `Nov-02-20-powertoys.png`
- `Nov-03-20-powertoys-menu.png`

## <a name="regular-expressions"></a>Reguläre Ausdrücke

Für die meisten Anwendungsfälle ist ein einfaches suchen und ersetzen ausreichend. Es kann jedoch vorkommen, dass komplizierte Umbenennungs Aufgaben zusammenkommen, die mehr Kontrolle erfordern. [Reguläre Ausdrücke](https://wikipedia.org/wiki/Regular_expression) können dabei helfen.

Reguläre Ausdrücke definieren ein Suchmuster für Text. Sie können verwendet werden, um Text zu suchen, zu bearbeiten und zu bearbeiten. Das vom regulären Ausdruck definierte Muster kann für eine bestimmte Zeichenfolge einmal, mehrmals oder überhaupt nicht abgeglichen werden. Powerrename verwendet die [ECMAScript](https://wikipedia.org/wiki/ECMAScript) -Grammatik, die häufig in modernen Programmiersprachen verwendet wird.

Aktivieren Sie das Kontrollkästchen "reguläre Ausdrücke verwenden", um reguläre Ausdrücke zu aktivieren.

**Hinweis:** Sie möchten bei Verwendung regulärer Ausdrücke wahrscheinlich "alle Vorkommen vergleichen" aktivieren.

### <a name="examples-of-regular-expressions"></a>Beispiele für reguläre Ausdrücke

#### <a name="simple-matching-examples"></a>Beispiele für einfache Übereinstimmungen

| Suchen nach       | BESCHREIBUNG                                           |
| ---------------- | ------------- |
| `^`              | Mit dem Anfang des Datei namens vergleichen                   |
| `$`              | Entsprechung für das Ende des Datei namens                         |
| `.*`             | Entsprechung für den gesamten Text im Namen                        |
| `^foo`           | Entsprechung für Text, der mit "foo" beginnt                     |
| `bar$`           | Abgleichen von Text, der auf "Bar" endet                       |
| `^foo.*bar$`     | Entsprechung für Text, der mit "foo" beginnt und mit "Bar" endet |
| `.+?(?=bar)`     | Entsprechung für alles bis "Balken"                          |
| `foo[\s\S]*bar`  | Vergleichen Sie alles zwischen "foo" und "Bar"              |

#### <a name="matching-and-variable-examples"></a>Übereinstimmende und Variable Beispiele

*Wenn Sie die Variablen verwenden, muss die Option "alle Vorkommen vergleichen" aktiviert sein.*

| Suchen nach   | Ersetzen durch    | BESCHREIBUNG                                |
| ------------ | --------------- |--------------------------------------------|
| `(.*).png`   | `foo_$1.png`   | "Foo \_ " dem vorhandenen Dateinamen voranstellen |
| `(.*).png`   | `$1_foo.png`   | Fügt " \_ foo" an den vorhandenen Dateinamen an.  |
| `(.*)`       | `$1.txt`        | Fügt die Erweiterung ". txt" an den vorhandenen Dateinamen an. |
| `(^\w+\.$)|(^\w+$)` | `$2.txt` | Fügt die Erweiterung ". txt" nur an den vorhandenen Dateinamen an, wenn keine Erweiterung vorhanden ist. |

### <a name="additional-resources-for-learning-regular-expressions"></a>Weitere Ressourcen für das Erlernen regulärer Ausdrücke

Es gibt hervorragend verfügbare Beispiele/Cheat Sheets, die Ihnen helfen

[Regex-Tutorial – ein schnelles Spickzettel nach Beispielen](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

[Lernprogramm zu regulären ECMAScript-Ausdrücken](https://o7planning.org/en/12219/ecmascript-regular-expressions-tutorial)

## <a name="file-list-filters"></a>Dateilisten Filter

Filter können in powerrename verwendet werden, um die Ergebnisse der Umbenennung einzugrenzen. Verwenden Sie das *Vorschaufenster* , um die erwarteten Ergebnisse zu überprüfen. Wählen Sie die Spaltenüberschriften zum Wechseln zwischen den Filtern aus.

- **Original**: die erste Spalte im *Vorschaufenster* wird zwischen den folgenden Zyklen durchlaufen:
  - Aktiviert: die Datei ist ausgewählt.
  - Deaktiviert: die Datei ist nicht für die Umbenennung ausgewählt (auch wenn Sie für den in den Suchkriterien eingegebenen Wert geeignet ist).

- Die zweite Spalte in den *Vorschau* Fenstern kann umgeschaltet **werden.**
  - In der Standard Vorschau werden alle ausgewählten Dateien angezeigt, wobei nur Dateien, die mit der *Suche* nach Kriterien übereinstimmen, die den aktualisierten Umbenennungs Wert anzeigen.
  - Wenn Sie die *umbenannte* Kopfzeile auswählen, wird die Vorschau umschalten, sodass nur Dateien angezeigt werden, die umbenannt werden. Andere ausgewählte Dateien aus der ursprünglichen Auswahl werden nicht angezeigt.

![Demo zum PowerToys-powerrename-Filter](../images/powerrename-demo2.gif)
