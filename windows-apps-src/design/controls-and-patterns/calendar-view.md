---
Description: In einer Kalenderansicht können Benutzer einen Kalender anzeigen und damit interagieren. Als Ansichten sind Monat, Jahr und zehn Jahre möglich.
title: Kalenderansicht
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9214ec2f2a1e352f94205675f2200920b8a00bd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654515"
---
# <a name="calendar-view"></a>Kalenderansicht

In einer Kalenderansicht können Benutzer einen Kalender anzeigen und damit interagieren. Als Ansichten sind Monat, Jahr und zehn Jahre möglich. Benutzer können ein einzelnes Datum oder einen Datumsbereich auswählen. Es gibt keine Auswahloberfläche, und der Kalender ist stets sichtbar. 

> **Wichtige APIs**:  [CalendarView Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx), [SelectedDatesChanged-Ereignis](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddateschanged.aspx)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?
Benutzer können in einer Kalenderansicht in einem stets sichtbaren Kalender ein einzelnes Datum oder einen Datumsbereich auswählen.

Wenn es wichtig ist, das Benutzer gleichzeitig mehrere Tage auswählen können, verwenden Sie eine Kalenderansicht. Sollen Benutzer jeweils nur ein Datum auswählen, ohne dass permanent ein Kalender sichtbar ist, bietet sich möglicherweise ein Steuerelement für die [Kalenderdatumsauswahl](calendar-date-picker.md) bzw. die [Datumsauswahl](date-picker.md) an.

Weitere Informationen zur Auswahl des passenden Steuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CalendarView">die App zu öffnen und CalendarView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Abrufen der XAML-Steuerelemente Katalog-app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Die Kalenderansicht besteht aus drei Ansichten: Monat, Jahr und Jahrzehnt. Standardmäßig wird beim Aufrufen des Kalenders die Monatsansicht angezeigt. Über die Eigenschaft [DisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.displaymode.aspx) können Sie die gewünschte Startansicht festlegen.

![Die drei Ansichten einer Kalenderansicht](images/calendar-view-3-views.png)

Wenn Benutzer in der Kopfzeile der Monatsansicht klicken, wird die Jahresansicht angezeigt. Durch Klicken auf die Kopfzeile der Jahresansicht wird die 10-Jahres-Ansicht aufgerufen. Indem Benutzer ein Jahr in der 10-Jahres-Ansicht auswählen, kehren sie zur Jahresansicht zurück. Durch Auswahl eines Monats in der Jahresansicht kehren sie zur Monatsansicht zurück. Mit den beiden Pfeilen neben der Kopfzeile navigieren sie einen Monat, ein Jahr bzw. zehn Jahre vor oder zurück. 

## <a name="create-a-calendar-view"></a>Erstellen einer Kalenderansicht

Dieses Beispiel zeigt, wie Sie eine einfache Kalenderansicht erstellen.

```xaml
<CalendarView/>
```

Die fertige Kalenderansicht sieht wie folgt aus:

![Beispiel für eine Kalenderansicht](images/controls_calendar_monthview.png)

### <a name="selecting-dates"></a>Auswählen von Tagen

Die Eigenschaft [SelectionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) ist standardmäßig auf **Single** gesetzt. Hiermit können Benutzer ein einzelnes Datum im Kalender auswählen. Um die Datumsauswahl zu deaktivieren, setzen Sie die „SelectionMode“-Eigenschaft auf **None**. 

Damit Benutzer mehrere Tage auswählen können, setzen Sie die „SelectionMode“-Eigenschaft auf **Multiple**. Sie können auch programmgesteuert mehrere Tage auswählen, indem Sie der Sammlung [SelectedDates](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddates.aspx) wie unten dargestellt Objekte des Typs [DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx)/[DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) hinzufügen:

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

Benutzer können die Auswahl eines Datums aufheben, indem sie im Kalenderraster auf das Datum klicken oder tippen.

Über das Ereignis [SelectedDatesChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddateschanged.aspx) können Sie sich über Änderungen an der Sammlung [SelectedDates](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selecteddates.aspx) benachrichtigen lassen.

> [!NOTE]
> Wichtige Informationen zu Datumswerten finden Sie im Artikel zu Datums- und Uhrzeitsteuerelementen unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values).

### <a name="customizing-the-calendar-views-appearance"></a>Anpassen des Erscheinungsbilds der Kalenderansicht

Die Kalenderansicht besteht sowohl aus XAML-Elementen, die in der „ControlTemplate“-Vorlage definiert werden, als auch aus visuellen Elementen, die direkt durch das Steuerelement gerendert werden. 
- Die in der Steuerelementevorlage definierten XAML-Elemente beinhalten den Rahmen des Steuerelements, die Kopfzeile, Vor- und Zurück-Schaltflächen sowie DayOfWeek-Elemente. Sie können die Elemente stilistisch wie XAML-Steuerelemente anpassen und als neue Vorlagen speichern. 
- Das Kalenderraster besteht aus Objekten des Typs [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx). Diese Elemente lassen sich weder stilistisch anpassen noch als neue Vorlage speichern. Sie können jedoch mithilfe verschiedener Eigenschaften ihr Erscheinungsbild ändern.

Dieses Diagramm veranschaulicht, aus welchen Elementen sich die Monatsansicht des Kalenders zusammensetzt. Weitere Informationen finden Sie in den Anmerkungen zur Klasse [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx).

![Die Elemente einer Monatsansicht des Kalenders](images/calendar-view-month-elements.png)

In dieser Tabelle sind die Eigenschaften aufgeführt, mit denen Sie das Erscheinungsbild von Kalenderelementen ändern können.

Element | Eigenschaften
--------|-----------
DayOfWeek | [DayOfWeekFormat](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayofweekformat.aspx)  
CalendarItem | [CalendarItemBackground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritembackground.aspx), [CalendarItemBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemborderbrush.aspx), [CalendarItemBorderThickness](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemborderthickness.aspx), [CalendarItemForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendaritemforeground.aspx)  
DayItem | [DayItemFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontfamily.aspx), [DayItemFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontsize.aspx), [DayItemFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontstyle.aspx), [DayItemFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.dayitemfontweight.aspx), [HorizontalDayItemAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment.aspx), [VerticalDayItemAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.verticaldayitemalignment.aspx), [CalendarViewDayItemStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle.aspx)  
MonthYearItem (in der Jahres- und 10-Jahres-Ansicht äquivalent zu DayItem) | [MonthYearItemFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily.aspx), [MonthYearItemFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontsize.aspx), [MonthYearItemFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle.aspx), [MonthYearItemFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.monthyearitemfontweight.aspx)  
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily.aspx), [FirstOfMonthLabelFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize.aspx), [FirstOfMonthLabelFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle.aspx), [FirstOfMonthLabelFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight.aspx), [HorizontalFirstOfMonthLabelAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment.aspx), [VerticalFirstOfMonthLabelAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment.aspx), [IsGroupLabelVisible](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.isgrouplabelvisible.aspx)  
FirstofYearDecadeLabel (in der Jahres- und 10-Jahres-Ansicht äquivalent zu FirstOfMonthLabel) | [FirstOfYearDecadeLabelFontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily.aspx), [FirstOfYearDecadeLabelFontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize.aspx), [FirstOfYearDecadeLabelFontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle.aspx), [FirstOfYearDecadeLabelFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight.aspx)  
Visual State-Rahmen | [FocusBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.focusborderbrush.aspx), [HoverBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.hoverborderbrush.aspx), [PressedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.pressedborderbrush.aspx), [SelectedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedborderbrush.aspx), [SelectedForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedforeground.aspx), [SelectedHoverBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush.aspx), [SelectedPressedBorderBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush.aspx)  
OutofScope | [IsOutOfScopeEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.isoutofscopeenabled.aspx), [OutOfScopeBackground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.outofscopebackground.aspx), [OutOfScopeForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.outofscopeforeground.aspx)  
Today | [IsTodayHighlighted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.istodayhighlighted.aspx), [TodayFontWeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.todayfontweight.aspx), [TodayForeground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.todayforeground.aspx)  

 Standardmäßig werden in der Monatsansicht sechs Wochen angezeigt. Die Anzahl der angezeigten Wochen können Sie über die Eigenschaft [NumberOfWeeksInView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.numberofweeksinview.aspx) anpassen. Sie haben die Möglichkeit, zwischen zwei und acht Wochen anzuzeigen.

Die Jahres- und 10-Jahres-Ansicht werden standardmäßig in einem 4x4-Raster angezeigt. Wenn Sie die Zeilen- oder Spaltenanzahl ändern möchten, rufen Sie [SetYearDecadeDisplayDimensions](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions.aspx) mit der gewünschten Anzahl von Zeilen und Spalten auf. Dadurch ändert sich das Raster der Jahres- und 10-Jahres-Ansicht.

Hier wurde für die Jahres- und 10-Jahres-Ansicht ein 3x4-Raster festgelegt.

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

Als frühestes Datum wird im Kalender standardmäßig das Datum vor 100 Jahren angezeigt, als spätestes Datum das Datum in 100 Jahren. Sie können das früheste und das späteste Datum im Kalender über die Eigenschaft [MinDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.mindate.aspx) bzw. die Eigenschaft [MaxDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.maxdate.aspx) ändern.

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>Aktualisieren von Kalendertagelementen

Jeder Tag im Kalender wird durch ein Objekt des Typs [CalendarViewDayItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.aspx) dargestellt. Um auf ein individuelles Tagelement zuzugreifen und dessen Eigenschaften und Methoden zu verwenden, nutzen Sie das [CalendarViewDayItemChanging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging.aspx)-Ereignis und die „Item“-Eigenschaft der Ereignisargumente, um auf das „CalendarViewDayItem“-Element zuzugreifen.

Wenn ein Tag in der Kalenderansicht nicht auswählbar sein soll, setzen Sie seine Eigenschaft [CalendarViewDayItem.IsBlackout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarviewdayitem.isblackout.aspx) auf **true**. 

Durch Aufrufen der Methode [CalendarViewDayItem.SetDensityColors](https://msdn.microsoft.com/library/windows/apps/xaml/dn890067.aspx) können Sie Kontextinformationen zur Termindichte des betreffenden Tages anzeigen. Sie können pro Tag zwischen 0 und 10 Dichtebalken in individuellen Farben festlegen. 

Ein Kalender enthält unter anderem folgende Tagelemente. Tag 1 und 2 sind schwarz angezeigt und damit nicht buchbar. Für Tag 2, 3 und 4 wurden verschiedene Dichtebalken festgelegt.

![Kalendertage mit Dichtebalken](images/calendar-view-density-bars.png)

### <a name="phased-rendering"></a>Phasen-Rendering

Eine Kalenderansicht kann zahlreiche „CalendarViewDayItem“-Objekte enthalten. Damit die Benutzeroberfläche reaktionsfähig bleibt und Benutzer fließend durch den Kalender navigieren können, unterstützt die Kalenderansicht das Phasen-Rendering. Sie können damit die Verarbeitung eines Tagelements in Phasen unterteilen. Wenn ein Tag aus der Ansicht verschoben wird, bevor alle Phasen abgeschlossen sind, wird keine Zeit mehr zum Verarbeiten oder Rendern dieses Elements aufgewendet.

In diesem Beispiel sehen Sie das Phasen-Rendering einer Kalenderansicht zur Terminplanung. 
- In Phase 0 wird das Standardtagelement gerendert. 
- In Phase 1 geben Sie an, welche Tage nicht buchbar sind. Hierzu zählen vergangene Tage, Sonntage und Tage, die bereits vollständig ausgebucht sind. 
- In Phase 2 prüfen Sie jeden für den Tag gebuchten Termin. Bestätigte Termine werden durch einen grünen Dichtebalken und vorbehaltliche Termine durch einen blauen Dichtebalken gekennzeichnet. 

Die `Bookings`-Klasse in diesem Beispiel stammt aus einer fiktiven Terminierungs-App und wird nicht angezeigt.

```xaml
<CalendarView CalendarViewDayItemChanging="CalendarView_CalendarViewDayItemChanging"/>
```

```csharp
private void CalendarView_CalendarViewDayItemChanging(CalendarView sender, 
                                   CalendarViewDayItemChangingEventArgs args)
{
    // Render basic day items.
    if (args.Phase == 0)
    {
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set blackout dates.
    else if (args.Phase == 1)
    {   
        // Blackout dates in the past, Sundays, and dates that are fully booked.
        if (args.Item.Date < DateTimeOffset.Now ||
            args.Item.Date.DayOfWeek == DayOfWeek.Sunday ||
            Bookings.HasOpenings(args.Item.Date) == false)
        {
            args.Item.IsBlackout = true;
        }
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set density bars.
    else if (args.Phase == 2)
    {
        // Avoid unnecessary processing.
        // You don't need to set bars on past dates or Sundays.
        if (args.Item.Date > DateTimeOffset.Now &&
            args.Item.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            // Get bookings for the date being rendered.
            var currentBookings = Bookings.GetBookings(args.Item.Date);

            List<Color> densityColors = new List<Color>();
            // Set a density bar color for each of the days bookings.
            // It's assumed that there can't be more than 10 bookings in a day. Otherwise,
            // further processing is needed to fit within the max of 10 density bars.
            foreach (booking in currentBookings)
            {
                if (booking.IsConfirmed == true)
                {
                    densityColors.Add(Colors.Green);
                }
                else
                {
                    densityColors.Add(Colors.Blue);
                }
            }
            args.Item.SetDensityColors(densityColors);
        }
    }
}
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Datumsauswahl](date-picker.md)
- [Uhrzeitauswahl](time-picker.md)
