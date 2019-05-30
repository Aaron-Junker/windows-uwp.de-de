---
Description: Mit Datums- und Uhrzeitsteuerelementen können Sie das Datum und die Uhrzeit anzeigen und festlegen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.
title: Richtlinien für Datums- und Uhrzeitsteuerelemente
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 54da5ea68dad0bbd2c7f00ae5fc4128f14d1ce43
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362880"
---
# <a name="calendar-date-and-time-controls"></a>Kalender-, Datums- und Uhrzeitsteuerelemente

 

Datums- und Uhrzeitsteuerelemente bieten Ihnen standardmäßige lokalisierte Methoden, den Benutzern die Anzeige oder Festlegung von Datums-und Uhrzeitwerten in Ihrer App zu ermöglichen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.

> **Wichtige APIs:** [CalendarView Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [CalendarDatePicker Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker), ["DatePicker"-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [TimePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/category/DataInput">die App zu öffnen und diese Steuerelemente in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="which-date-or-time-control-should-you-use"></a>Welches Datums- bzw. Uhrzeitsteuerelement sollten Sie verwenden?

Es stehen vier Datums- und Uhrzeitsteuerelemente zur Auswahl. Welches Steuerelement Sie verwenden, hängt vom jeweiligen Szenario ab. Die Informationen in diesem Abschnitt sollen Ihnen dabei helfen, das richtige Steuerelement für Ihre App auszuwählen.

&nbsp;|&nbsp;|&nbsp;                                                                                                                      
--------------------|-------|-------------------------------------------------------------------------------------------------------------------------------
Kalenderansicht       |![Beispiel für eine Kalenderansicht](images/controls_calendar_monthview_small.png)|Verwenden Sie diese Ansicht, um ein einzelnes Datum oder einen Datumsbereich aus einem immer sichtbaren Kalender auszuwählen.                   
Kalenderdatumsauswahl|![Beispiel für eine Kalenderdatumsauswahl](images/calendar-date-picker-closed.png)|Verwenden Sie diese Ansicht, um ein einzelnes Datum aus einem kontextbezogenen Kalender auszuwählen. 
Datumsauswahl         |![Beispiel für eine Datumsauswahl](images/date-picker-closed.png)|Verwenden Sie diese Ansicht, um ein einzelnes bekanntes Datum auszuwählen, wenn kontextbezogene Informationen nicht wichtig sind.
Zeitauswahl         |![Beispiel für Zeitauswahl](images/time-picker-closed.png)|Verwenden Sie diese Ansicht zum Auswählen eines einzelnen Uhrzeitwertes.                                        

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>Kalenderansicht

Mit **CalendarView** können Benutzer einen Kalender anzeigen und mit diesem interagieren. Als Ansichten sind Monat, Jahr und zehn Jahre möglich. Benutzer können ein einzelnes Datum oder einen Datumsbereich auswählen. Es gibt keine Auswahloberfläche, und der Kalender ist stets sichtbar.

Die Kalenderansicht besteht aus drei Ansichten: Monat, Jahr und Jahrzehnt. Standardmäßig ist in der Kalenderansicht zunächst die Monatsansicht geöffnet, Sie können jedoch jede Ansicht als Startansicht angeben.

![Beispiel für eine Kalenderdatumsauswahl](images/calendar-view-3-views.png)

- Wenn es wichtig ist, das Benutzer gleichzeitig mehrere Tage auswählen können, müssen Sie eine **CalendarView** verwenden.
- Wenn der Benutzern jeweils nur ein Datum auswählen soll und der Kalender nicht permanent sichtbar sein muss, kann möglicherweise ein Steuerelement **CalendarDatePicker** bzw. **DatePicker** verwendet werden.

### <a name="calendar-date-picker"></a>Kalenderdatumsauswahl

**CalendarDatePicker** ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums aus einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind. Sie können den Kalender bearbeiten, um Kontext hinzuzufügen oder verfügbare Tage einzugrenzen.

Der Einstiegspunkt zeigt Platzhaltertext an, falls kein Datum festgelegt wurde. Andernfalls wird das ausgewählte Datum angezeigt. Wenn Benutzer den Einstiegspunkt auswählen, wird eine Kalenderansicht eingeblendet, damit sie ein Datum auswählen können. Die Kalenderansicht überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „beiseitegeschoben“.

![Beispiel für eine Kalenderdatumsauswahl](images/calendar-date-picker-2-views.png)

- Verwenden Sie eine Kalenderdatumsauswahl, um beispielsweise einen Termin oder ein Abreisedatum auszuwählen. 

### <a name="date-picker"></a>Datumsauswahl

Das **DatePicker**-Steuerelement bietet eine standardisierte Methode zum Auswählen eines bestimmten Datums. 

Der Einstiegspunkt zeigt das ausgewählte Datum an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit eine Auswahl getroffen werden kann. Die Datumsauswahl überlagert andere Elemente der Benutzeroberfläche; die anderen Elemente der Benutzeroberfläche werden jedoch nicht „beiseitegeschoben“.

![Beispiel für die Erweiterung der Datumsauswahl](images/controls_datepicker_expand.png)

- Verwenden Sie eine Datumsauswahl, damit Benutzer ein bekanntes Datum wie etwa einen Geburtstag auswählen können, bei dem der Kalenderkontext unwichtig ist.

### <a name="time-picker"></a>Zeitauswahl

**TimePicker** wird zur Auswahl eines einzelnen Uhrzeitwertes für Termine oder Abreisezeiten verwendet. Es handelt sich um eine statische Anzeige, die vom Benutzer oder im Code festgelegt wird, jedoch nicht aktualisiert wird, um die aktuelle Uhrzeit anzuzeigen. 

Der Einstiegspunkt zeigt die ausgewählte Uhrzeit an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit er eine Auswahl treffen kann. Die Zeitauswahl überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „verschoben“.

![Beispiel für die Erweiterung der Zeitauswahl](images/controls_timepicker_expand.png)

- Verwenden Sie die Zeitauswahl, um Benutzern die Auswahl eines einzelnen Zeitwerts zu ermöglichen.

## <a name="create-a-date-or-time-control"></a>Erstellen eines Datums oder Uhrzeitsteuerelements

Informationen und Beispiele zu den einzelnen Datums- und Textsteuerelementen finden Sie in den folgenden Artikeln.

- [Kalenderansicht](calendar-view.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Datumsauswahl](date-picker.md)
- [Zeitauswahl](time-picker.md)

### <a name="globalization"></a>Globalisierung

Die XAML-Datumssteuerelemente unterstützen jedes der von Windows unterstützten Kalendersysteme. Diese Kalender werden in der Klasse [Windows.Globalization.CalendarIdentifiers](https://docs.microsoft.com/uwp/api/Windows.Globalization.CalendarIdentifiers) angegeben. Jedes Steuerelement verwendet den richtigen Kalender für die standardmäßige Sprache Ihrer App. Alternativ können Sie die **CalendarIdentifier**-Eigenschaft festlegen, um ein bestimmtes Kalendersystem zu verwenden.

Das Zeitauswahl-Steuerelement unterstützt jedes der Uhrsysteme, die in der Klasse [Windows.Globalization.ClockIdentifiers](https://docs.microsoft.com/uwp/api/Windows.Globalization.ClockIdentifiers) definiert sind. Sie können in der Eigenschaft [ClockIdentifier](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier) festlegen, ob das 12-Stunden- oder das 24-Stunden-Format verwendet werden soll. Die Art der Eigenschaft ist „String“ (Zeichenfolge), Sie müssen jedoch Werte verwenden, die den statischen Zeichenfolgeneigenschaften der ClockIdentifiers-Klasse entsprechen. Diese lauten wie folgt: TwelveHour (die Zeichenfolge "" 12hourclock "") und TwentyFourHour (die Zeichenfolge "" 24hourclock "). Der Standardwert lautet „12HourClock“


### <a name="datetime-and-calendar-values"></a>Werte für Datum/Uhrzeit und Kalender

Die in den XAML-Datums- und Uhrzeitsteuerelementen verwendeten Datumsobjekte weisen je nach Programmiersprache eine unterschiedliche Darstellung auf. 
- C# und Visual Basic verwenden die Struktur [System.DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN), die Teil von .NET ist. 
- C++/CX verwendet die Struktur [Windows::Foundation::DateTime](https://docs.microsoft.com/windows/desktop/api/windows.foundation/ns-windows-foundation-datetime). 

Ein verwandtes Konzept ist die Calendar-Klasse, die beeinflusst, wie die Daten im Kontext interpretiert werden. Alle Windows-Runtime-Apps können die Klasse [Windows.Globalization.Calendar](https://docs.microsoft.com/uwp/api/Windows.Globalization.Calendar) verwenden. C#- und Visual Basic-Apps können alternativ die Klasse [System.Globalization.Calendar](https://docs.microsoft.com/dotnet/api/system.globalization.calendar?redirectedfrom=MSDN) verwenden, deren Funktionalität sehr ähnlich ist. (Windows-Runtime-Apps können die .NET-Basisklasse Calendar verwenden, nicht jedoch die spezifischen Implementierungen wie z. B. GregorianCalendar.)

.NET unterstützt auch einen Typ namens [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime?redirectedfrom=MSDN), der implizit in [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN) konvertierbar ist. Somit könnte ein Typ „DateTime“ in .NET-Code verwendet werden, der zum Festlegen von Werten verwendet wird, bei denen es sich eigentlich um DateTimeOffset handelt. Weitere Informationen zum Unterschied zwischen „DateTime“ und „DateTimeOffset“ finden Sie in den Bemerkungen zur Klasse [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN).

> **Hinweis**&nbsp;&nbsp;Eigenschaften, die Datumsobjekte verwenden, können nicht als XAML-Attributzeichenfolge festgelegt werden, da der Windows-Runtime-XAML-Parser nicht über eine Konvertierungslogik zum Konvertieren von Zeichenfolgen in Datumsangaben als DateTime/DateTimeOffset-Objekte verfügt. In der Regel legen Sie diese Werte im Code fest. Eine andere mögliche Methode ist, um ein Datum zu definieren, die als ein Objekt oder in den Datenkontext verfügbar ist, und legen Sie die Eigenschaft als ein XAML-Attribut, das verweist auf eine [ \{Bindung\} Markuperweiterung](../../xaml-platform/binding-markup-extension.md) Ausdruck die können das Datum als Daten zugreifen.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen
* [Beispiel für XAML UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)


## <a name="related-topics"></a>Verwandte Themen

**Für Entwickler (XAML)**
- [CalendarView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)
- [CalendarDatePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)
- [DatePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker)
- [TimePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)
