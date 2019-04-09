---
Description: Die Kalenderdatumsauswahl ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums in einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind.
title: Kalenderdatumsauswahl
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c899334b43353fbc69c3080cfd329df0ef9e0797
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913970"
---
# <a name="calendar-date-picker"></a>Kalenderdatumsauswahl

 

Die Kalenderdatumsauswahl ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums in einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind. Sie können den Kalender bearbeiten, um Kontext hinzuzufügen oder verfügbare Tage einzugrenzen.

> **Wichtige APIs:** [CalendarDatePicker Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx), [Datum Eigenschaft](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx), [DateChanged-Ereignis](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?
Benutzer können in einer **Kalenderdatumsauswahl** in einer kontextbezogenen Kalenderansicht ein einzelnes Datum auswählen. Dies eignet sich beispielsweise, um Termine oder Abflugzeiten einzutragen.

Damit Benutzer ein bekanntes Datum wie etwa einen Geburtstag eintragen können, bei dem der Kalenderkontext unwichtig ist, könnte eine [Datumsauswahl](date-picker.md) von Vorteil sein.

Weitere Informationen zur Auswahl des passenden Steuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CalendarDatePicker">die App zu öffnen und CalendarDatePicker in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Der Einstiegspunkt zeigt Platzhaltertext an, falls kein Datum festgelegt wurde. Andernfalls wird das ausgewählte Datum angezeigt. Wenn Benutzer den Einstiegspunkt auswählen, wird eine Kalenderansicht eingeblendet, damit sie ein Datum auswählen können. Die Kalenderansicht überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „beiseitegeschoben“.

![Beispiel für eine Kalenderdatumsauswahl](images/calendar-date-picker-2-views.png)

## <a name="create-a-date-picker"></a>Erstellen einer Datumsauswahl

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

Die fertige Kalenderdatumsauswahl sieht wie folgt aus:

![Beispiel für eine Kalenderdatumsauswahl](images/calendar-date-picker-closed.png)

Die Kalenderdatumsauswahl verfügt über eine interne Klasse [CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) zur Auswahl eines Datums. In CalendarDatePicker ist eine Teilmenge von CalendarView-Eigenschaften, wie [IsTodayHighlighted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted.aspx) und [FirstDayOfWeek](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.firstdayofweek.aspx) vorhanden. Diese werden an die interne „CalendarView“-Klasse weitergeleitet, damit Sie sie ändern können. 

Sie können jedoch über die Eigenschaft [SelectionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) der internen Klasse „CalendarView“ keine Mehrfachauswahl definieren. Falls Benutzer mehrere Tage auswählen sollen oder permanent ein Kalender sichtbar sein muss, bietet sich möglicherweise anstelle der Kalenderdatumsauswahl eine Kalenderansicht an. Weitere Informationen zum Ändern der Kalenderanzeige finden Sie im Artikel zur [Kalenderansicht](calendar-view.md).

### <a name="selecting-dates"></a>Auswählen von Tagen

Verwenden Sie die Eigenschaft [Date](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx), um das ausgewählte Datum abzurufen oder festzulegen. Der Standardwert der „Date“-Eigenschaft lautet **null**. Wenn Benutzer ein Datum in der Kalenderansicht auswählen, wird diese Eigenschaft aktualisiert. Benutzer können die Auswahl eines Datums aufheben, indem sie in der Kalenderansicht auf das Datum klicken. 

Sie können das Datum im Code wie folgt festlegen.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Wenn Sie das Datum in Code festlegen, wird der Wert durch die Eigenschaften [MinDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.mindate.aspx) und [MaxDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.maxdate.aspx) beschränkt.
- Wenn **Date** vor **MinDate** liegt, wird der Wert auf **MinDate** gesetzt.
- Wenn **Date** nach **MaxDate** liegt, wird der Wert auf **MaxDate** gesetzt.

Über das Ereignis [DateChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx) können Sie sich bei Änderungen am Wert „Date“ benachrichtigen lassen.

> [!NOTE]
> Wichtige Informationen zu Datumswerten finden Sie im Artikel zu Datums- und Uhrzeitsteuerelementen unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values).

### <a name="setting-a-header-and-placeholder-text"></a>Festlegen von Kopfzeilen- und Platzhaltertext

Sie können der Kalenderdatumsauswahl eine [Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.header.aspx)-Eigenschaft (oder eine Beschriftung) und eine [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.placeholdertext.aspx)-Eigenschaft (oder ein Wasserzeichen) hinzufügen, um Benutzern einen Hinweis bezüglich des Verwendungszwecks zu geben. Um das Erscheinungsbild der Überschrift anzupassen, können Sie anstelle der Header-Eigenschaft die [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.headertemplate.aspx)-Eigenschaft festlegen.

Als Standardtext für den Platzhalter wird „Datum auswählen“ angezeigt. Sie können diesen Text entfernen, indem Sie die PlaceholderText-Eigenschaft auf eine leere Zeichenfolge festlegen. Alternativ können Sie wie hier gezeigt einen benutzerdefinierten Text eingeben.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderansicht](calendar-view.md)
- [Datumsauswahl](date-picker.md)
- [Zeitauswahl](time-picker.md)
