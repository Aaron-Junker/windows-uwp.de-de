---
Description: Verwenden Sie die API Windows.Globalization.DateTimeFormatting mit benutzerdefinierten Vorlagen und Mustern, Datumsangaben und Uhrzeiten in genau dem Format angezeigt werden sollen.
title: Verwenden von Mustern zum Formatieren von Datums- und Uhrzeitwerten
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 3849ccf0f129b65dc44f549a37859fe38ac71562
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615395"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>Verwenden von Mustern zum Formatieren von Datums- und Uhrzeitwerten

Verwenden Sie Klassen im Namespace [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) mit benutzerdefinierten Vorlagen und Mustern, um Daten und Zeiten im gewünschten Format anzuzeigen.

## <a name="introduction"></a>Einführung

Die Klasse [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) bietet vielfältige Möglichkeiten, um Daten und Uhrzeiten für Sprachen und Regionen rund um die Welt ordnungsgemäß zu formatieren. In den meisten Fällen können Sie Standardformate für Jahr, Monat, Tag usw. verwenden. Oder Sie können eine Formatvorlage an das *FormatTemplate*-Argument des **DateTimeFormatter**-Konstruktors übergeben, beispielsweise „longdate” oder „month day”.

Wenn Sie allerdings noch mehr Kontrolle über Reihenfolge und Format der Komponenten des anzuzeigenden [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) -Objekts haben möchten, können Sie ein Formatmuster an das *formatTemplate*-Argument des Konstruktors übergeben. Ein Formatmuster verwendet eine spezielle Syntax, mit der Sie einzelne Komponenten eines **DateTime**-Objekts (beispielsweise nur den Monatsnamen oder nur den Jahreswert) abrufen können, um sie in einem bestimmten benutzerdefinierten Format anzuzeigen. Darüber hinaus kann das Muster mittels Lokalisierung für andere Sprachen und Regionen angepasst werden.

**Beachten Sie**  nur eine Übersicht über die Formatierungsmuster. Eine ausführlichere Besprechung der Formatvorlagen und Formatmuster finden Sie in der Dokumentation zur [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live)-Klasse im Abschnitt „Anmerkungen“.

## <a name="the-difference-between-format-templates-and-format-patterns"></a>Der Unterschied zwischen Formatvorlagen und Formatmustern

Eine Formatvorlage ist eine kulturunabhängige Formatzeichenfolge. Wenn Sie also einen **DateTimeFormatter** mithilfe einer Formatvorlage erstellen, zeigt der Formatierer Ihre Formatkomponenten in der richtigen Reihenfolge für die aktuelle Sprache an. Im Gegensatz dazu ist ein Formatmuster kulturspezifisch. Wenn Sie einen **DateTimeFormatter** mithilfe eines Formatmusters erstellen, verwendet der Formatierer genau das vorgegebene Muster. Somit ist ein Muster nicht unbedingt für verschiedenen Kulturen anwendbar.

Betrachten Sie den Unterschied anhand des folgenden Beispiels. Wir übergeben eine einfache Formatvorlage (kein Muster) an den **DateTimeFormatter**-Konstruktor. Dies ist die Formatvorlage „month day”.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Hierdurch wird ein Formatierer erstellt, der auf dem Sprach- und Regionswert des aktuellen Kontexts basiert. Die Reihenfolge der Komponenten in einer Formatvorlage spielt keine Rolle. Der Formatierer zeigt sie in der richtigen Reihenfolge für die aktuelle Sprache. Er zeigt also „January 1” für Englisch (USA), „1 janvier“ für Französisch (Frankreich) und „1月1日“ für Japanisch an.

Dagegen ist ein Formatmuster kulturspezifisch. Sehen wir uns das Formatmuster für unsere Formatvorlage für eine.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

Wir erhalten unterschiedliche Ergebnisse, je nach Laufzeitsprache und Region. Unterschiedliche Regionen können unterschiedliche Komponenten in unterschiedlicher Reihenfolge und mit oder ohne zusätzliche Zeichen und Leerzeichen verwenden.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

Im obigen Beispiel haben wir eine kulturunabhängige Formatzeichenfolge eingegeben und eine kulturspezifische Formatzeichenfolge erhalten (die eine Funktion der Sprache und Region war, die beim Aufruf von `dateFormatter.Patterns` gültig waren). Wenn Sie also einen **DateTimeFormatter** aus einem kulturspezifischen Formatmuster erstellen, ist er nur für bestimmte Sprachen/Regionen gültig.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Der Formatierer, der oben genannten gibt kulturspezifischer Werte für die einzelnen Komponenten innerhalb der Klammern {}. Aber die Reihenfolge der Komponenten in einem Formatmuster ist unveränderlich. Sie erhalten also stets genau das, was Sie anfordern, und das ist unter Umständen nicht immer für die jeweilige Kultur geeignet. Diese Formatierer gilt für Englisch (USA), aber nicht für Französisch (Frankreich) oder Japanisch.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

Zudem kann ein Muster, das heute korrekt ist, in Zukunft falsch sein. Länder und Regionen können Änderungen an ihren Kalendersystemen vornehmen, die eine Anpassung einer Formatvorlage erfordern. Windows aktualisiert die Ausgabe von Formatierern basierend auf Formatvorlagen, um solchen Änderungen Rechnung zu tragen Daher sollten Sie die Mustersyntax nur unter einer oder mehreren dieser Bedingungen verwenden:

-   Sie benötigen keine bestimmte Ausgabe für ein Format.
-   Das Format muss keinen bestimmten kulturspezifischen Standard erfüllen.
-   Das Muster soll bewusst für alle Kulturen unveränderlich sein.
-   Sie beabsichtigen, die Musterformat-Zeichenfolge selbst zu lokalisieren.

Hier eine Zusammenfassung der Unterschiede zwischen Formatvorlagen und Formatmustern:

**Format-Vorlagen, z.B. "Monatstag"**

-   Eine abstrahierte Darstellung des [DateTime](/uwp/api/windows.foundation.datetime?branch=live)-Formats, die Werte für Tag , Monat usw. in einer bestimmten Reihenfolge enthält.
-   Gibt für alle von Windows unterstützten Sprach- und Regionswerte stets ein gültiges Standardformat zurück.
-   Liefert stets eine entsprechend der Kultur formatierte Zeichenfolge für die angegebene Sprache und Region.
-   Nicht alle Kombinationen der Komponenten sind gültig. Beispielsweise ist „dayofweek day ” ungültig.

**Formatierungsmuster, z. B. "{month.full} {day.integer}"**

-   Eine Zeichenfolge mit explizit festgelegter Reihenfolge, die den vollständigen Monatsnamen, gefolgt von einem Leerzeichen, gefolgt von einer ganze Zahl (für den Tag) angibt, und zwar in dieser Reihenfolge oder gemäß dem angegebenen spezifischen Formatmuster.
-   Entspricht möglicherweise nicht dem gültigen Standardformat für jedes Sprach-Region-Paar.
-   Ist nicht in jedem Fall der Kultur angemessen.
-   Es kann eine beliebige Kombination von Komponenten in einer beliebigen Reihenfolge angegeben werden.

## <a name="examples"></a>Beispiele

Angenommen, Sie möchten den aktuellen Monat und den aktuellen Tag zusammen mit der aktuellen Uhrzeit in einen bestimmten Format anzeigen. Sie möchten also z. B., dass für US-Benutzer Folgendes angezeigt wird:

``` syntax
June 25 | 1:38 PM
```

Die Datumskomponente entspricht der Formatvorlage „month day“, die Uhrzeitkomponente der Formatvorlage „hour minute“. Sie können also Formatierungsprogramme für die relevante Datum und Uhrzeit-Format Vorlagen erstellen und verkettet Sie dann die Ausgabe, die zusammen mit der eine lokalisierbare Zeichenfolge.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` eine Ressourcen-ID ist auf eine lokalisierbare Ressource in einer Ressourcendatei (.resw) verwiesen werden. Für den Standardsprachen Englisch (Vereinigte Staaten), dies festgelegt auf einen Wert von "{0} | {1}"sowie einen Kommentar, der angibt, dass"{0}"ist das Datum und"{1}"ist die Zeit an. Somit können Übersetzer die Formatelemente wie gewünscht anpassen. Sie können beispielsweise die Reihenfolge der Komponenten ändern, falls in bestimmten Sprachen oder Regionen die Uhrzeit vor dem Datum stehen soll. Oder Sie können „|“ durch ein anderes Trennzeichen ersetzen.

Eine andere Möglichkeit zum Implementieren dieses Beispiels besteht darin, die beiden Formatierer nach ihren Formatmustern abzufragen, diese miteinander zu verketten und dann aus dem resultierenden Formatmuster einen dritten Formatierer zu erstellen.

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

* [Datums- und Zeitformat-Beispiel](https://go.microsoft.com/fwlink/p/?LinkId=231618)
