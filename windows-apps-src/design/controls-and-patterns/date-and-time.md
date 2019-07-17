---
Description: Mit Datums- und Uhrzeitsteuerelementen können Sie das Datum und die Uhrzeit anzeigen und festlegen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.
title: Richtlinien für Datums- und Uhrzeitsteuerelemente
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0c980acc3b9887dac68712bd65de96e8f3a327a5
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319061"
---
# <a name="calendar-date-and-time-controls"></a>Kalender-, Datums- und Uhrzeitsteuerelemente

 

Datums- und Uhrzeitsteuerelemente bieten Ihnen standardmäßige lokalisierte Methoden, den Benutzern die Anzeige oder Festlegung von Datums-und Uhrzeitwerten in Ihrer App zu ermöglichen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.

> **Wichtige APIs:** [CalendarView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [CalendarDatePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker), [DatePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [TimePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)

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

Die XAML-Datumssteuerelemente unterstützen jedes der von Windows unterstützten Kalendersysteme. Diese Kalender werden in der [Windows.Globalization.CalendarIdentifiers](https://docs.microsoft.com/uwp/api/Windows.Globalization.CalendarIdentifiers)-Klasse angegeben. Jedes Steuerelement verwendet den richtigen Kalender für die standardmäßige Sprache Ihrer App. Alternativ können Sie die **CalendarIdentifier**-Eigenschaft festlegen, um ein bestimmtes Kalendersystem zu verwenden.

Das Zeitauswahl-Steuerelement unterstützt jedes der Uhrsysteme, die in der [Windows.Globalization.ClockIdentifiers](https://docs.microsoft.com/uwp/api/Windows.Globalization.ClockIdentifiers)-Klasse angegeben sind. Sie können für die [ClockIdentifier](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier)-Eigenschaft festlegen, dass das 12-Stunden- oder 24-Stunden-Uhrzeitformat verwendet werden soll. Die Art der Eigenschaft ist „String“ (Zeichenfolge), Sie müssen jedoch Werte verwenden, die den statischen Zeichenfolgeneigenschaften der ClockIdentifiers-Klasse entsprechen. Diese sind: TwelveHour (Zeichenfolge „12HourClock“) und TwentyFourHour (Zeichenfolge „24HourClock“). Der Standardwert lautet „12HourClock“


### <a name="datetime-and-calendar-values"></a>Werte für Datum/Uhrzeit und Kalender

Die in den XAML-Datums- und Uhrzeitsteuerelementen verwendeten Datumsobjekte weisen je nach Programmiersprache eine unterschiedliche Darstellung auf. 
- C# und Visual Basic verwenden die [System.DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN)-Struktur, die Teil von .NET ist. 
- C++ / CX verwendet die [Windows::Foundation::DateTime](https://docs.microsoft.com/windows/desktop/api/windows.foundation/ns-windows-foundation-datetime)-Struktur. 

Ein verwandtes Konzept ist die Calendar-Klasse, die beeinflusst, wie die Daten im Kontext interpretiert werden. Alle Windows-Runtime-Apps können die [Windows.Globalization.Calendar](https://docs.microsoft.com/uwp/api/Windows.Globalization.Calendar)-Klasse verwenden. C# und Visual Basic-Apps können auch die [System.Globalization.Calendar](https://docs.microsoft.com/dotnet/api/system.globalization.calendar?redirectedfrom=MSDN)-Klasse verwenden. Diese weist eine sehr ähnliche Funktionalität auf. (Windows-Runtime-Apps können die .NET-Basisklasse Calendar verwenden, nicht jedoch die spezifischen Implementierungen wie z. B. GregorianCalendar.)

.NET unterstützt auch einen Typ mit der Bezeichnung [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime?redirectedfrom=MSDN), der implizit in [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN) konvertierbar ist. Somit könnte ein Typ „DateTime“ in .NET-Code verwendet werden, der zum Festlegen von Werten verwendet wird, bei denen es sich eigentlich um DateTimeOffset handelt. Weitere Informationen zum Unterschied zwischen DateTime und DateTimeOffset finden Sie in den Bemerkungen in der [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN)-Klasse.

> **Note**&nbsp;&nbsp;Eigenschaften, die Datumsobjekte verwenden, können nicht als XAML-Attributzeichenfolge festgelegt werden, da der Windows-Runtime-XAML-Parser nicht über eine Konvertierungslogik zum Konvertieren von Zeichenfolgen in Datumsangaben als DateTime/DateTimeOffset-Objekte verfügt. In der Regel legen Sie diese Werte im Code fest. Eine andere Möglichkeit besteht darin, ein Datum zu definieren, das als Datenobjekt oder im Datenkontext verfügbar ist, und anschließend die Eigenschaft als XAML-Attribut festzulegen, das auf einen [\{Binding\}-Markuperweiterung](../../xaml-platform/binding-markup-extension.md)-Ausdruck verweist, der auf das Datum als Daten zugreifen kann.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen
* [Beispiel für XAML-Benutzeroberflächengrundlagen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)


## <a name="related-topics"></a>Verwandte Themen

**Für Entwickler (XAML)**
- [CalendarView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)
- [CalendarDatePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)
- [DatePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker)
- [TimePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)
