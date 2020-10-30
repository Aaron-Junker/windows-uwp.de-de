---
description: Verwenden Sie die Windows. Globalization. datetimeformatierung-API mit benutzerdefinierten Vorlagen und Mustern, um Datumsangaben und Uhrzeiten in genau dem gewünschten Format anzuzeigen.
title: Verwenden von Mustern zum Formatieren von Datums- und Uhrzeitwerten
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: dbabbcaccd88b187a03c83909bcb38d5f64b30bb
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034313"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>Verwenden von Vorlagen und Mustern zum Formatieren von Datums- und Uhrzeitwerten

Verwenden Sie Klassen im [**Windows. Globalization. datetimeformatierung**](/uwp/api/windows.globalization.datetimeformatting?branch=live) -Namespace mit benutzerdefinierten Vorlagen und Mustern, um Datumsangaben und Uhrzeiten in genau dem gewünschten Format anzuzeigen.

## <a name="introduction"></a>Einführung

Die [**datetimeformatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) -Klasse bietet verschiedene Methoden zum ordnungsgemäßen Formatieren von Datums-und Uhrzeiten für Sprachen und Regionen auf der ganzen Welt. Sie können Standardformate für Jahr, Monat, Tag usw. verwenden. Oder Sie können eine Formatvorlage an das *Format Template* -Argument des **datetimeformatter** -Konstruktors übergeben, z. b. "longDate" oder "Month Day".

Wenn Sie jedoch eine noch größere Kontrolle über die Reihenfolge und das Format der Komponenten des [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) -Objekts wünschen, das Sie anzeigen möchten, können Sie ein Format Muster an das Format *Template* -Argument des Konstruktors übergeben. Ein Format Muster verwendet eine spezielle Syntax, mit der Sie einzelne Komponenten eines **DateTime** -Objekts nur mit &mdash; dem Monatsnamen oder nur dem Year-Wert abrufen können, um &mdash; Sie z. b. in einem beliebigen benutzerdefinierten Format anzuzeigen. Darüber hinaus kann das Muster mittels Lokalisierung für andere Sprachen und Regionen angepasst werden.

**Hinweis**  Dies ist nur eine Übersicht über Format Muster. Eine ausführlichere Besprechung der Formatvorlagen und Formatmuster finden Sie in der Dokumentation zur [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live)-Klasse im Abschnitt „Anmerkungen“.

## <a name="the-difference-between-format-templates-and-format-patterns"></a>Der Unterschied zwischen Formatvorlagen und Format Mustern

Eine Formatvorlage ist eine Kultur unabhängige Format Zeichenfolge. Wenn Sie also einen **datetimeformatter** mithilfe einer Formatvorlage erstellen, zeigt der Formatierer ihre Format Komponenten in der richtigen Reihenfolge für die aktuelle Sprache an. Umgekehrt ist ein Format Muster Kultur spezifisch. Wenn Sie einen **datetimeformatter** mithilfe eines Format Musters erstellen, verwendet der Formatierer das Muster genau wie angegeben. Folglich ist ein Muster nicht in allen Kulturen, die nicht unbedingt notwendig sind.

Sehen wir uns diesen Unterschied mit einem Beispiel an. Wir übergeben eine einfache Formatvorlage (kein Muster) an den **datetimeformatter** -Konstruktor. Dies ist die Formatvorlage "Month Day".

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Hierdurch wird ein Formatierer erstellt, der auf dem Sprach- und Regionswert des aktuellen Kontexts basiert. Die Reihenfolge der Komponenten in einer Formatvorlage spielt keine Rolle. der Formatierer zeigt Sie in der richtigen Reihenfolge der aktuellen Sprache an. Daher wird "1. Januar" für Englisch (USA), "1 janvier" für Französisch (Frankreich) und "1 月 1 日" für Japanisch angezeigt.

Auf der anderen Seite ist ein Format Muster Kultur spezifisch. Wir greifen auf das Format Muster für unsere Formatvorlage zu.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

Dies führt abhängig von der Laufzeit-Sprache und-Region zu unterschiedlichen Ergebnissen. In unterschiedlichen Regionen können unterschiedliche Komponenten, in unterschiedlichen Reihen folgen, mit oder ohne zusätzliche Zeichen und Abstände verwendet werden.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

Im obigen Beispiel wurde eine Kultur unabhängige Format Zeichenfolge zurückgegeben, und wir haben eine kulturspezifische Formatierungs Zeichenfolge zurückgegeben (bei der es sich um eine Funktion der Sprache und der Region handelte, die beim Aufrufen von in Kraft trat `dateFormatter.Patterns` ). Wenn Sie also einen **datetimeformatter** aus einem kulturspezifischen Format Muster erstellen, ist er nur für bestimmte Sprachen/Regionen gültig.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Der obige Formatierer gibt kulturspezifische Werte für die einzelnen Komponenten innerhalb der Klammern zurück {} . Die Reihenfolge der Komponenten in einem Format Muster ist jedoch invariant. Sie erhalten genau das, was Sie Fragen, was möglicherweise in der Kultur angemessen ist. Dieser Formatierer ist für Englisch (USA) gültig, aber nicht für Französisch (Frankreich) und Japanisch.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

Außerdem ist ein Muster, das heute richtig ist, in Zukunft möglicherweise nicht korrekt. Länder oder Regionen können Ihre Kalendersysteme ändern, wodurch eine Formatvorlage geändert wird. Windows aktualisiert die Ausgabe der Formatierer auf der Grundlage von Formatvorlagen, um solche Änderungen zu ermöglichen. Daher sollten Sie nur die Muster Syntax unter einer oder mehreren dieser Bedingungen verwenden.

-   Sie benötigen keine bestimmte Ausgabe für ein Format.
-   Das Format muss keinen bestimmten kulturspezifischen Standard erfüllen.
-   Das Muster soll bewusst für alle Kulturen unveränderlich sein.
-   Sie beabsichtigen, die tatsächliche Format Muster Zeichenfolge zu lokalisieren.

Im folgenden finden Sie eine Zusammenfassung der Unterscheidung zwischen Formatvorlagen und Format Mustern.

**Format Vorlagen, z. b. "Month Day"**

-   Abstrahierte Darstellung eines [DateTime](/uwp/api/windows.foundation.datetime?branch=live) -Formats, das Werte für den Monat, den Tag usw. enthält, in beliebiger Reihenfolge.
-   Gibt für alle von Windows unterstützten Sprach- und Regionswerte stets ein gültiges Standardformat zurück.
-   Liefert stets eine entsprechend der Kultur formatierte Zeichenfolge für die angegebene Sprache und Region.
-   Nicht alle Kombinationen von Komponenten sind gültig. Beispielsweise ist "DayOfWeek Day" nicht gültig.

**Format Muster, z. b. "{Month. Full} {Day. Integer}"**

-   Explizit geordnete Zeichenfolge, die den vollständigen Monatsnamen, gefolgt von einem Leerzeichen, gefolgt von der ganzzahligen Ganzzahl, in dieser Reihenfolge oder einem bestimmten, von Ihnen angegebenen Format Muster ausdrückt.
-   Entspricht möglicherweise nicht dem gültigen Standardformat für jedes Sprach-Region-Paar.
-   Ist nicht in jedem Fall der Kultur angemessen.
-   Eine beliebige Kombination von Komponenten kann in beliebiger Reihenfolge angegeben werden.

## <a name="examples"></a>Beispiele

Angenommen, Sie möchten den aktuellen Monat und den aktuellen Tag zusammen mit der aktuellen Uhrzeit in einen bestimmten Format anzeigen. Sie möchten also z. B., dass für US-Benutzer Folgendes angezeigt wird:

``` syntax
June 25 | 1:38 PM
```

Der Datums Teil entspricht der Formatvorlage "Month Day", und der Uhrzeit Teil entspricht der Formatvorlage "Hour-Minute". Daher können Sie Formatierungs Programme für die relevanten Formatvorlagen für Datum und Uhrzeit erstellen und dann Ihre Ausgabe mit einer lokalisierbaren Format Zeichenfolge verketten.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` ein Ressourcen Bezeichner, der sich auf eine lokalisierbare Ressource in einer Ressourcen Datei (. resw) bezieht. Für eine Standardsprache in englischer Sprache (USA) wird dies auf den Wert " {0} | {1} " und einen Kommentar festgelegt, der angibt, dass " {0} " das Datum und " {1} " die Uhrzeit ist. Auf diese Weise können Konvertierer die Format Elemente nach Bedarf anpassen. Sie können z. b. die Reihenfolge der Elemente ändern, wenn Sie in einer Sprache oder Region in einer anderen Sprache oder Region der Zeit vorangestellt ist. Oder Sie können „|“ durch ein anderes Trennzeichen ersetzen.

Eine andere Möglichkeit zur Implementierung dieses Beispiels besteht darin, die beiden Formatierungs Programme für Ihre Format Muster abzufragen, diese miteinander zu verketten und dann ein drittes Formatierungs Programm aus dem resultierenden Format Muster zu erstellen.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

string dateFormatterPattern = dateFormatter.Patterns[0];
string timeFormatterPattern = timeFormatter.Patterns[0];

string pattern = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), dateFormatterPattern, timeFormatterPattern);

var patternFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(pattern);

string output = patternFormatter.Format(System.DateTime.Now);
```

## <a name="important-apis"></a>Wichtige APIs

* [Windows.Globalization.DateTimeFormatting](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTime](/uwp/api/windows.foundation.datetime?branch=live)

## <a name="related-topics"></a>Verwandte Themen

* [Beispiel für Datums- und Uhrzeitformatierung](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Date%20and%20time%20formatting%20sample%20(Windows%208))
