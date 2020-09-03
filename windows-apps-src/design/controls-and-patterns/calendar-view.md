---
Description: In einer Kalenderansicht können Benutzer einen Kalender anzeigen und damit interagieren. Als Ansichten sind Monat, Jahr und zehn Jahre möglich.
title: Kalenderansicht
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9c4c1dfa6bf4a92b7f8da177b749dfd861d93338
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173554"
---
# <a name="calendar-view"></a>Kalenderansicht

In einer Kalenderansicht können Benutzer einen Kalender anzeigen und damit interagieren. Als Ansichten sind Monat, Jahr und zehn Jahre möglich. Benutzer können ein einzelnes Datum oder einen Datumsbereich auswählen. Es gibt keine Auswahloberfläche, und der Kalender ist stets sichtbar.

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Die Bibliothek „Windows UI“ enthält ab Version 2.2 eine neue Vorlage für dieses Steuerelement, die abgerundete Ecken verwendet. Weitere Informationen finden Sie unter [Eckradius](../style/rounded-corner.md). „WinUI“ ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Plattform-APIs:**  [CalendarView-Klasse](/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [SelectedDatesChanged-Ereignis](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?
Benutzer können in einer Kalenderansicht in einem stets sichtbaren Kalender ein einzelnes Datum oder einen Datumsbereich auswählen.

Wenn es wichtig ist, das Benutzer gleichzeitig mehrere Tage auswählen können, verwenden Sie eine Kalenderansicht. Sollen Benutzer jeweils nur ein Datum auswählen, ohne dass permanent ein Kalender sichtbar ist, bietet sich möglicherweise ein Steuerelement für die [Kalenderdatumsauswahl](calendar-date-picker.md) bzw. die [Datumsauswahl](date-picker.md) an.

Weitere Informationen zur Auswahl des passenden Steuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CalendarView">die App zu öffnen und CalendarView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Die Kalenderansicht besteht aus drei Ansichten: Monat, Jahr und Jahrzehnt. Standardmäßig wird beim Aufrufen des Kalenders die Monatsansicht angezeigt. Sie können mit der [DisplayMode](/uwp/api/windows.ui.xaml.controls.calendarview.displaymode)-Eigenschaft eine Standardansicht festlegen.

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

Standardmäßig ist die [SelectionMode](/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode)-Eigenschaft auf **Single** festgelegt. Hiermit können Benutzer ein einzelnes Datum im Kalender auswählen. Um die Datumsauswahl zu deaktivieren, setzen Sie die „SelectionMode“-Eigenschaft auf **None**.

Damit Benutzer mehrere Tage auswählen können, setzen Sie die „SelectionMode“-Eigenschaft auf **Multiple**. Sie können mehrere Tage programmatisch auswählen, indem Sie der [SelectedDates](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates)-Sammlung wie folgt [DateTime](/dotnet/api/system.datetime)/[DateTimeOffset](/dotnet/api/system.datetimeoffset)-Objekte hinzufügen:

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

Benutzer können die Auswahl eines Datums aufheben, indem sie im Kalenderraster auf das Datum klicken oder tippen.

Mithilfe des [SelectedDatesChanged](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged)-Ereignisses können Sie sich bei Änderungen an der [SelectedDates](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates)-Sammlung benachrichtigen lassen.

> [!NOTE]
> Wichtige Informationen zu Datumswerten finden Sie im Artikel zu Datums- und Uhrzeitsteuerelementen unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values).

### <a name="customizing-the-calendar-views-appearance"></a>Anpassen des Erscheinungsbilds der Kalenderansicht

Die Kalenderansicht besteht sowohl aus XAML-Elementen, die in der „ControlTemplate“-Vorlage definiert werden, als auch aus visuellen Elementen, die direkt durch das Steuerelement gerendert werden.
- Die in der Steuerelementevorlage definierten XAML-Elemente beinhalten den Rahmen des Steuerelements, die Kopfzeile, Vor- und Zurück-Schaltflächen sowie DayOfWeek-Elemente. Sie können die Elemente stilistisch wie XAML-Steuerelemente anpassen und als neue Vorlagen speichern.
- Das Kalenderraster besteht aus [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem)-Objekten. Diese Elemente lassen sich weder stilistisch anpassen noch als neue Vorlage speichern. Sie können jedoch mithilfe verschiedener Eigenschaften ihr Erscheinungsbild ändern.

Dieses Diagramm veranschaulicht, aus welchen Elementen sich die Monatsansicht des Kalenders zusammensetzt. Weitere Informationen finden Sie in den Anmerkungen zur [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem)-Klasse.

![Die Elemente einer Monatsansicht des Kalenders](images/calendar-view-month-elements.png)

In dieser Tabelle sind die Eigenschaften aufgeführt, mit denen Sie das Erscheinungsbild von Kalenderelementen ändern können.

Element | Eigenschaften
--------|-----------
DayOfWeek | [DayOfWeekFormat](/uwp/api/windows.ui.xaml.controls.calendarview.dayofweekformat)
CalendarItem | [CalendarItemBackground](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritembackground), [CalendarItemBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderbrush), [CalendarItemBorderThickness](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderthickness), [CalendarItemForeground](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemforeground)
DayItem | [DayItemFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontfamily), [DayItemFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontsize), [DayItemFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontstyle), [DayItemFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontweight), [HorizontalDayItemAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment), [VerticalDayItemAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.verticaldayitemalignment), [CalendarViewDayItemStyle](/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle)
MonthYearItem (in der Jahres- und 10-Jahres-Ansicht äquivalent zu DayItem) | [MonthYearItemFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily), [MonthYearItemFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontsize), [MonthYearItemFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle), [MonthYearItemFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontweight)
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily), [FirstOfMonthLabelFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize), [FirstOfMonthLabelFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle), [FirstOfMonthLabelFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight), [HorizontalFirstOfMonthLabelAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment), [VerticalFirstOfMonthLabelAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment), [IsGroupLabelVisible](/uwp/api/windows.ui.xaml.controls.calendarview.isgrouplabelvisible)
FirstofYearDecadeLabel (in der Jahres- und 10-Jahres-Ansicht äquivalent zu FirstOfMonthLabel) | [FirstOfYearDecadeLabelFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily), [FirstOfYearDecadeLabelFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize), [FirstOfYearDecadeLabelFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle), [FirstOfYearDecadeLabelFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight)
Visual State-Rahmen | [FocusBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.focusborderbrush), [HoverBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.hoverborderbrush), [PressedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.pressedborderbrush), [SelectedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedborderbrush), [SelectedForeground](/uwp/api/windows.ui.xaml.controls.calendarview.selectedforeground), [SelectedHoverBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush), [SelectedPressedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush)
OutofScope | [IsOutOfScopeEnabled](/uwp/api/windows.ui.xaml.controls.calendarview.isoutofscopeenabled), [OutOfScopeBackground](/uwp/api/windows.ui.xaml.controls.calendarview.outofscopebackground), [OutOfScopeForeground](/uwp/api/windows.ui.xaml.controls.calendarview.outofscopeforeground)
Heute | [IsTodayHighlighted](/uwp/api/windows.ui.xaml.controls.calendarview.istodayhighlighted), [TodayFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.todayfontweight), [TodayForeground](/uwp/api/windows.ui.xaml.controls.calendarview.todayforeground)

 Standardmäßig werden in der Monatsansicht sechs Wochen angezeigt. Die Anzahl der angezeigten Wochen können Sie mit der [NumberOfWeeksInView](/uwp/api/windows.ui.xaml.controls.calendarview.numberofweeksinview)-Eigenschaft ändern. Sie haben die Möglichkeit, zwischen zwei und acht Wochen anzuzeigen.

Die Jahres- und 10-Jahres-Ansicht werden standardmäßig in einem 4x4-Raster angezeigt. Um die Zeilen- oder Spaltenanzahl zu ändern, rufen Sie [SetYearDecadeDisplayDimensions](/uwp/api/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions) mit der gewünschten Anzahl von Zeilen und Spalten auf. Dadurch ändert sich das Raster der Jahres- und 10-Jahres-Ansicht.

Hier wurde für die Jahres- und 10-Jahres-Ansicht ein 3x4-Raster festgelegt.

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

Als frühestes Datum wird im Kalender standardmäßig das Datum vor 100 Jahren angezeigt, als spätestes Datum das Datum in 100 Jahren. Sie können das früheste und späteste Datum des Kalenders mit den Eigenschaften [MinDate](/uwp/api/windows.ui.xaml.controls.calendarview.mindate) und [MaxDate](/uwp/api/windows.ui.xaml.controls.calendarview.maxdate) ändern.

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>Aktualisieren von Kalendertagelementen

Jeder Tag im Kalender wird durch ein [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem)-Objekt repräsentiert. Um auf ein individuelles Tagelement zuzugreifen und dessen Eigenschaften und Methoden zu verwenden, nutzen Sie das [CalendarViewDayItemChanging](/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging)-Ereignis und die Item-Eigenschaft der Ereignisargumente, um auf das CalendarViewDayItem-Objekt zuzugreifen.

Wenn ein Tag in der Kalenderansicht nicht auswählbar sein soll, legen Sie dessen [CalendarViewDayItem.IsBlackout](/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.isblackout)-Eigenschaft auf **True** fest.

Durch Aufrufen der [CalendarViewDayItem.SetDensityColors](/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.setdensitycolors)-Methode können Sie Kontextinformationen zur Dichte von Ereignissen an einem Tag anzeigen. Sie können pro Tag zwischen 0 und 10 Dichtebalken in individuellen Farben festlegen.

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

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Datumsauswahl](date-picker.md)
- [Uhrzeitauswahl](time-picker.md)