---
Description: Die Kalenderdatumsauswahl ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums in einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind.
title: Kalenderdatumsauswahl
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2bc62a70b3dc52440e88652cd2d1eec3d01f97cc
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968995"
---
# <a name="calendar-date-picker"></a>Kalenderdatumsauswahl

Die Kalenderdatumsauswahl ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums in einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind. Sie können den Kalender bearbeiten, um Kontext hinzuzufügen oder verfügbare Tage einzugrenzen.

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Die Bibliothek „Windows UI“ enthält ab Version 2.2 eine neue Vorlage für dieses Steuerelement, die abgerundete Ecken verwendet. Weitere Informationen finden Sie unter [Eckradius](/windows/uwp/design/style/rounded-corner). WinUI ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Plattform-APIs:** [CalendarDatePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker), [Date-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.date), [DateChanged-Ereignis](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.datechanged)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Benutzer können in einer **Kalenderdatumsauswahl** in einer kontextbezogenen Kalenderansicht ein einzelnes Datum auswählen. Dies eignet sich beispielsweise, um Termine oder Abflugzeiten einzutragen.

Damit Benutzer ein bekanntes Datum wie etwa einen Geburtstag eintragen können, bei dem der Kalenderkontext unwichtig ist, könnte eine [Datumsauswahl](date-picker.md) von Vorteil sein.

Weitere Informationen zur Auswahl des passenden Steuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CalendarDatePicker">die App zu öffnen und CalendarDatePicker in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
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

Die Kalenderdatumsauswahl verfügt über eine interne [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)-Klasse, um ein Datum auszuwählen. In CalendarDatePicker ist eine Teilmenge von CalendarView-Eigenschaften wie [IsTodayHighlighted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted) und [FirstDayOfWeek](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.firstdayofweek) vorhanden. Diese werden an die interne CalendarView-Klasse weitergeleitet, damit Sie sie ändern können. 

Sie können jedoch die [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode)-Eigenschaft der internen CalendarView-Klasse nicht ändern und somit keine Mehrfachauswahl zulassen. Falls Benutzer mehrere Tage auswählen sollen oder permanent ein Kalender sichtbar sein muss, bietet sich möglicherweise anstelle der Kalenderdatumsauswahl eine Kalenderansicht an. Weitere Informationen zum Ändern der Kalenderanzeige finden Sie im Artikel zur [Kalenderansicht](calendar-view.md).

### <a name="selecting-dates"></a>Auswählen von Tagen

Verwenden Sie die [Date](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.date)-Eigenschaft, um das ausgewählte Datum abzurufen oder festzulegen. Der Standardwert der „Date“-Eigenschaft lautet **null**. Wenn Benutzer ein Datum in der Kalenderansicht auswählen, wird diese Eigenschaft aktualisiert. Benutzer können die Auswahl eines Datums aufheben, indem sie in der Kalenderansicht auf das Datum klicken. 

Sie können das Datum im Code wie folgt festlegen.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Wenn Sie das Datum in Code festlegen, wird der Wert durch die Eigenschaften [MinDate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.mindate) und [MaxDate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.maxdate) beschränkt.
- Wenn **Date** vor **MinDate** liegt, wird der Wert auf **MinDate** gesetzt.
- Wenn **Date** nach **MaxDate** liegt, wird der Wert auf **MaxDate** gesetzt.

Mithilfe des [DateChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.datechanged)-Ereignisses können Sie sich bei Änderungen am Date-Wert benachrichtigen lassen.

> [!NOTE]
> Wichtige Informationen zu Datumswerten finden Sie im Artikel zu Datums- und Uhrzeitsteuerelementen unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values).

### <a name="setting-a-header-and-placeholder-text"></a>Festlegen von Kopfzeilen- und Platzhaltertext

Sie können der Kalenderdatumsauswahl eine [Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.header)-Eigenschaft (oder eine Beschriftung) und eine [PlaceholderText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.placeholdertext)-Eigenschaft (oder ein Wasserzeichen) hinzufügen, um Benutzern einen Hinweis zum Verwendungszweck zu geben. Um das Erscheinungsbild der Überschrift anzupassen, können Sie anstelle der Header-Eigenschaft die [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker.headertemplate)-Eigenschaft festlegen.

Als Standardtext für den Platzhalter wird „Datum auswählen“ angezeigt. Sie können diesen Text entfernen, indem Sie die PlaceholderText-Eigenschaft auf eine leere Zeichenfolge festlegen. Alternativ können Sie wie hier gezeigt einen benutzerdefinierten Text eingeben.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderansicht](calendar-view.md)
- [Datumsauswahl](date-picker.md)
- [Uhrzeitauswahl](time-picker.md)
