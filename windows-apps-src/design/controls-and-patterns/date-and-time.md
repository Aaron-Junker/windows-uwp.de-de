---
Description: Date and time controls let you view and set the date and time. This article provides design guidelines and helps you pick the right control.
title: Richtlinien für Datums- und Uhrzeitsteuerelemente
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f65ed68db51ea173dfec3c06a9dc81a7f7735afd
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "8894189"
---
# <a name="calendar-date-and-time-controls"></a>Kalender-, Datums- und Uhrzeitsteuerelemente

 

Datums- und Uhrzeitsteuerelemente bieten Ihnen standardmäßige lokalisierte Methoden, den Benutzern die Anzeige oder Festlegung von Datums-und Uhrzeitwerten in Ihrer App zu ermöglichen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.

> **Wichtige APIs:** [Klasse „CalendarView“](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx), [Klasse „CalendarDatePicker“](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx), [Klasse „DatePicker“](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [Klasse „TimePicker“](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/category/DataInput">die App zu öffnen und diese Steuerelemente in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Erwerben Sie den Quellcode (GitHub)</a></li>
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
Uhrzeitauswahl         |![Beispiel für eine Zeitauswahl](images/time-picker-closed.png)|Verwenden Sie diese Ansicht zum Auswählen eines einzelnen Uhrzeitwertes.                                        

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>Kalenderansicht

Mit **CalendarView** können Benutzer einen Kalender anzeigen und mit diesem interagieren. Als Ansichten sind Monat, Jahr und zehn Jahre möglich. Benutzer können ein einzelnes Datum oder einen Datumsbereich auswählen. Es gibt keine Auswahloberfläche, und der Kalender ist immer sichtbar.

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

### <a name="time-picker"></a>Uhrzeitauswahl

**TimePicker** wird zur Auswahl eines einzelnen Uhrzeitwertes für Termine oder Abreisezeiten verwendet. Es handelt sich um eine statische Anzeige, die vom Benutzer oder im Code festgelegt wird, jedoch nicht aktualisiert wird, um die aktuelle Uhrzeit anzuzeigen. 

Der Einstiegspunkt zeigt die ausgewählte Uhrzeit an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit eine Auswahl getroffen werden kann. Die Zeitauswahl überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „verschoben“.

![Beispiel für die Erweiterung der Zeitauswahl](images/controls_timepicker_expand.png)

- Verwenden Sie die Zeitauswahl, um Benutzern die Auswahl eines einzelnen Zeitwerts zu ermöglichen.

## <a name="create-a-date-or-time-control"></a>Erstellen eines Datums oder Uhrzeitsteuerelements

Informationen und Beispiele zu den einzelnen Datums- und Textsteuerelementen finden Sie in den folgenden Artikeln.

- [Kalenderansicht](calendar-view.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Datumsauswahl](date-picker.md)
- [Uhrzeitauswahl](time-picker.md)

### <a name="globalization"></a>Globalisierung

Die XAML-Datumssteuerelemente unterstützen jedes der von Windows unterstützten Kalendersysteme. Diese Kalender werden in der Klasse [Windows.Globalization.CalendarIdentifiers](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendaridentifiers.aspx) angegeben. Jedes Steuerelement verwendet den richtigen Kalender für die standardmäßige Sprache Ihrer App. Alternativ können Sie die **CalendarIdentifier**-Eigenschaft festlegen, um ein bestimmtes Kalendersystem zu verwenden.

Das Zeitauswahl-Steuerelement unterstützt jedes der Uhrsysteme, die in der Klasse [Windows.Globalization.ClockIdentifiers](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.clockidentifiers.aspx) definiert sind. Sie können in der Eigenschaft [ClockIdentifier](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.clockidentifier.aspx) festlegen, ob das 12-Stunden- oder das 24-Stunden-Format verwendet werden soll. Die Art der Eigenschaft ist „String“ (Zeichenfolge), Sie müssen jedoch Werte verwenden, die den statischen Zeichenfolgeneigenschaften der ClockIdentifiers-Klasse entsprechen. Dies lauten: TwelveHour (Zeichenfolge „12HourClock“) und TwentyFourHour (Zeichenfolge „24HourClock“). Der Standardwert lautet „12HourClock“


### <a name="datetime-and-calendar-values"></a>Werte für Datum/Uhrzeit und Kalender

Die in den XAML-Datums- und Uhrzeitsteuerelementen verwendeten Datumsobjekte weisen je nach Programmiersprache eine unterschiedliche Darstellung auf. 
- C# und Visual Basic verwenden die Struktur [System.DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx), die Teil von .NET ist. 
- C++/CX verwendet die Struktur [Windows::Foundation::DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/br205770.aspx). 

Ein verwandtes Konzept ist die Calendar-Klasse, die beeinflusst, wie die Daten im Kontext interpretiert werden. Alle Windows-Runtime-Apps können die Klasse [Windows.Globalization.Calendar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendar.aspx) verwenden. C#- und VisualBasic-Apps können alternativ die Klasse [System.Globalization.Calendar](https://msdn.microsoft.com/library/windows/apps/xaml/system.globalization.calendar.aspx) verwenden, deren Funktionalität sehr ähnlich ist. (Windows-Runtime-Apps können die .NET-Basisklasse Calendar verwenden, nicht jedoch die spezifischen Implementierungen wie z.B. GregorianCalendar.)

.NET unterstützt auch einen Typ namens [DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx), der implizit in [DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) konvertierbar ist. Somit könnte ein Typ „DateTime“ in .NET-Code verwendet werden, der zum Festlegen von Werten verwendet wird, bei denen es sich eigentlich um DateTimeOffset handelt. Weitere Informationen zum Unterschied zwischen „DateTime“ und „DateTimeOffset“ finden Sie in den Bemerkungen zur Klasse [DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx).

> **Hinweis**&nbsp;&nbsp;Eigenschaften, die Datumsobjekte verwenden, können nicht als XAML-Attributzeichenfolge festgelegt werden, da der Windows-Runtime-XAML-Parser nicht über eine Konvertierungslogik zum Konvertieren von Zeichenfolgen in Datumsangaben als DateTime/DateTimeOffset-Objekte verfügt. In der Regel legen Sie diese Werte im Code fest. Eine andere Möglichkeit besteht darin, ein Datum zu definieren, das als Datenobjekt oder im Datenkontext verfügbar ist, und anschließend die Eigenschaft als XAML-Attribut festzulegen, das auf einen [\{Binding\}-Markuperweiterung](../../xaml-platform/binding-markup-extension.md)-Ausdruck verweist, der auf das Datum als Daten zugreifen kann.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen
* [Beispiel für XAML-UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)


## <a name="related-topics"></a>Verwandte Themen

**Für Entwickler (XAML)**
- [CalendarView-Klasse](https://msdn.microsoft.com/library/windows/apps/dn890052)
- [CalendarDatePicker-Klasse](https://msdn.microsoft.com/library/windows/apps/dn950083)
- [DatePicker-Klasse](https://msdn.microsoft.com/library/windows/apps/dn298584)
- [TimePicker-Klasse](https://msdn.microsoft.com/library/windows/apps/dn299280)
