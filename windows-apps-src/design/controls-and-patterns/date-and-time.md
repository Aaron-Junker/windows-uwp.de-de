---
description: Mit Datums- und Uhrzeitsteuerelementen können Sie das Datum und die Uhrzeit anzeigen und festlegen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.
title: Richtlinien für Datums- und Uhrzeitsteuerelemente
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 239296cfb2de5b52b9f9ef1498b5c2c22e2be372
ms.sourcegitcommit: 62a6e7b4d35f63c25cedd61c96dfc251ff19c80d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286609"
---
# <a name="calendar-date-and-time-controls"></a>Kalender-, Datums- und Uhrzeitsteuerelemente

 

Datums- und Uhrzeitsteuerelemente bieten Ihnen standardmäßige lokalisierte Methoden, den Benutzern die Anzeige oder Festlegung von Datums-und Uhrzeitwerten in Ihrer App zu ermöglichen. Dieser Artikel enthält Entwurfsrichtlinien und hilft Ihnen beim Auswählen des richtigen Steuerelements.

> **Wichtige APIs:** [CalendarView-Klasse](/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [CalendarDatePicker-Klasse](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker), [DatePicker-Klasse](/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [TimePicker-Klasse](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
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

| Control | Beispiel | BESCHREIBUNG |
| ------- | :-----: | ----------- |
| Kalenderansicht | ![Beispiel für eine Kalenderansicht](images/controls_calendar_monthview_small.png) | Verwenden Sie diese Ansicht, um ein einzelnes Datum oder einen Datumsbereich aus einem immer sichtbaren Kalender auszuwählen. |
| Kalenderdatumsauswahl | ![Screenshot einer Kalenderdatumsauswahl.](images/calendar-date-picker-closed.png) | Verwenden Sie diese Ansicht, um ein einzelnes Datum aus einem kontextbezogenen Kalender auszuwählen. |
| Datumsauswahl | ![Beispiel für eine Datumsauswahl](images/date-picker-closed.png) | Verwenden Sie diese Ansicht, um ein einzelnes bekanntes Datum auszuwählen, wenn kontextbezogene Informationen nicht wichtig sind. |
| Zeitauswahl | ![Beispiel für Zeitauswahl](images/time-picker-closed.png) | Verwenden Sie diese Ansicht zum Auswählen eines einzelnen Uhrzeitwertes. |

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>Kalenderansicht

Mit **CalendarView** können Benutzer einen Kalender anzeigen und mit diesem interagieren. Als Ansichten sind Monat, Jahr und zehn Jahre möglich. Benutzer können ein einzelnes Datum oder einen Datumsbereich auswählen. Es gibt keine Auswahloberfläche, und der Kalender ist stets sichtbar.

Die Kalenderansicht besteht aus drei Ansichten: Monat, Jahr und Jahrzehnt. Standardmäßig ist in der Kalenderansicht zunächst die Monatsansicht geöffnet, Sie können jedoch jede Ansicht als Startansicht angeben.

![Screenshot von drei Kalenderansichten mit einer Monatsansicht, einer Jahresansicht und einer Dekadenansicht.](images/calendar-view-3-views.png)

- Wenn es wichtig ist, das Benutzer gleichzeitig mehrere Tage auswählen können, müssen Sie eine **CalendarView** verwenden.
- Wenn der Benutzern jeweils nur ein Datum auswählen soll und der Kalender nicht permanent sichtbar sein muss, kann möglicherweise ein Steuerelement **CalendarDatePicker** bzw. **DatePicker** verwendet werden.

### <a name="calendar-date-picker"></a>Kalenderdatumsauswahl

**CalendarDatePicker** ist ein Dropdownsteuerelement, das für die Auswahl eines einzelnen Datums aus einer Kalenderansicht optimiert ist, in der kontextbezogene Informationen wie der Wochentag oder die Belegung des Kalenders von Bedeutung sind. Sie können den Kalender bearbeiten, um Kontext hinzuzufügen oder verfügbare Tage einzugrenzen.

Der Einstiegspunkt zeigt Platzhaltertext an, falls kein Datum festgelegt wurde. Andernfalls wird das ausgewählte Datum angezeigt. Wenn Benutzer den Einstiegspunkt auswählen, wird eine Kalenderansicht eingeblendet, damit sie ein Datum auswählen können. Die Kalenderansicht überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „beiseitegeschoben“.

![Screenshot einer Kalenderdatumsauswahl mit einem leeren Textfeld zum Auswählen eines Datums, gefolgt von einem, das aufgefüllt ist, mit darunter liegendem Kalender.](images/calendar-date-picker-2-views.png)

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

### <a name="use-a-date-picker-and-time-picker-together"></a>Gemeinsame Verwendung von Datumsauswahl und Zeitauswahl

In diesem Beispiel wird gezeigt, wie Sie `DatePicker`und `TimePicker` kombinieren können, um Benutzern das Auswählen von Ankunftsdatum und -uhrzeit zu ermöglichen. Sie behandeln das `SelectedDateChanged`-Ereignis und das `SelectedTimeChanged`-Ereignis, um eine einzelne [DateTime](/uwp/api/windows.foundation.datetime)-Instanz namens `arrivalDateTime` zu aktualisieren. Der Benutzer kann die Datums- und Zeitauswahl auch löschen, nachdem sie festgelegt wurden.

:::image type="content" source="images/date-time/date-and-time-picker.png" alt-text="Datumsauswahl, Zeitauswahl, Schaltfläche und Beschriftung.":::

```xaml
<StackPanel>
    <DatePicker x:Name="arrivalDatePicker" Header="Arrival date"
                DayFormat="{}{day.integer} ({dayofweek.abbreviated})"
                SelectedDateChanged="ArrivalDatePicker_SelectedDateChanged"/>
    <StackPanel Orientation="Horizontal">
        <TimePicker x:Name="arrivalTimePicker" Header="Arrival time"
                MinuteIncrement="15"
                SelectedTimeChanged="ArrivalTimePicker_SelectedTimeChanged"/>
        <Button Content="Clear" Click="ClearDateButton_Click"
                VerticalAlignment="Bottom" Height="30" Width="54"/>
    </StackPanel>
    <TextBlock x:Name="arrivalText" Margin="0,12"/>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    DateTime arrivalDateTime;

    public MainPage()
    {
        this.InitializeComponent();

        // Set minimum to the current year and maximum to five years from now.
        arrivalDatePicker.MinYear = DateTimeOffset.Now;
        arrivalDatePicker.MaxYear = DateTimeOffset.Now.AddYears(5);
    }

    private void ArrivalTimePicker_SelectedTimeChanged(TimePicker sender, TimePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalTimePicker.SelectedTime != null)
        {
            arrivalDateTime = new DateTime(arrivalDateTime.Year, arrivalDateTime.Month, arrivalDateTime.Day,
                                           args.NewTime.Value.Hours, args.NewTime.Value.Minutes, args.NewTime.Value.Seconds);
        }
        arrivalText.Text = arrivalDateTime.ToString();
    }

    private void ArrivalDatePicker_SelectedDateChanged(DatePicker sender, DatePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalDatePicker.SelectedDate != null)
        {
            if (VerifyDateIsFuture((DateTimeOffset)arrivalDatePicker.SelectedDate) == true)
            {
                arrivalDateTime = new DateTime(args.NewDate.Value.Year, args.NewDate.Value.Month, args.NewDate.Value.Day,
                                               arrivalDateTime.Hour, arrivalDateTime.Minute, arrivalDateTime.Second);
                arrivalText.Text = arrivalDateTime.ToString();
            }
            else
            {
                arrivalDatePicker.SelectedDate = null;
                arrivalText.Text = "Arrival date must be later than today.";
            }
        }
    }

    private bool VerifyDateIsFuture(DateTimeOffset date)
    {
        if (date > DateTimeOffset.Now)
        {
            return true;
        }
        return false;
    }

    private void ClearDateButton_Click(object sender, RoutedEventArgs e)
    {
        arrivalDateTime = new DateTime();
        arrivalDatePicker.SelectedDate = null;
        arrivalTimePicker.SelectedTime = null;
        arrivalText.Text = string.Empty;
    }
}
```

### <a name="globalization"></a>Globalisierung

Die XAML-Datumssteuerelemente unterstützen jedes der von Windows unterstützten Kalendersysteme. Diese Kalender werden in der [Windows.Globalization.CalendarIdentifiers](/uwp/api/Windows.Globalization.CalendarIdentifiers)-Klasse angegeben. Jedes Steuerelement verwendet den richtigen Kalender für die standardmäßige Sprache Ihrer App. Alternativ können Sie die **CalendarIdentifier**-Eigenschaft festlegen, um ein bestimmtes Kalendersystem zu verwenden.

Das Zeitauswahl-Steuerelement unterstützt jedes der Uhrsysteme, die in der [Windows.Globalization.ClockIdentifiers](/uwp/api/Windows.Globalization.ClockIdentifiers)-Klasse angegeben sind. Sie können für die [ClockIdentifier](/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier)-Eigenschaft festlegen, dass das 12-Stunden- oder 24-Stunden-Uhrzeitformat verwendet werden soll. Die Art der Eigenschaft ist „String“ (Zeichenfolge), Sie müssen jedoch Werte verwenden, die den statischen Zeichenfolgeneigenschaften der ClockIdentifiers-Klasse entsprechen. Dies lauten: TwelveHour (Zeichenfolge „12HourClock“) und TwentyFourHour (Zeichenfolge „24HourClock“). Der Standardwert lautet „12HourClock“

### <a name="datetime-and-calendar-values"></a>Werte für Datum/Uhrzeit und Kalender

Die in den XAML-Datums- und Uhrzeitsteuerelementen verwendeten Datumsobjekte weisen je nach Programmiersprache eine unterschiedliche Darstellung auf.

- C# und Visual Basic verwenden die [System.DateTimeOffset](/dotnet/api/system.datetimeoffset)-Struktur, die Teil von .NET ist. 
- C++ / CX verwendet die [Windows::Foundation::DateTime](/windows/desktop/api/windows.foundation/ns-windows-foundation-datetime)-Struktur. 

Ein verwandtes Konzept ist die Calendar-Klasse, die beeinflusst, wie die Daten im Kontext interpretiert werden. Alle Windows-Runtime-Apps können die [Windows.Globalization.Calendar](/uwp/api/Windows.Globalization.Calendar)-Klasse verwenden. C# und Visual Basic-Apps können auch die [System.Globalization.Calendar](/dotnet/api/system.globalization.calendar)-Klasse verwenden. Diese weist eine sehr ähnliche Funktionalität auf. (Windows-Runtime-Apps können die .NET-Basisklasse Calendar verwenden, nicht jedoch die spezifischen Implementierungen wie z. B. GregorianCalendar.)

.NET unterstützt auch einen Typ mit der Bezeichnung [DateTime](/dotnet/api/system.datetime), der implizit in [DateTimeOffset](/dotnet/api/system.datetimeoffset) konvertierbar ist. Somit könnte ein Typ „DateTime“ in .NET-Code verwendet werden, der zum Festlegen von Werten verwendet wird, bei denen es sich eigentlich um DateTimeOffset handelt. Weitere Informationen zum Unterschied zwischen DateTime und DateTimeOffset finden Sie in den Bemerkungen in der [DateTimeOffset](/dotnet/api/system.datetimeoffset)-Klasse.

> [!NOTE]
> Eigenschaften, die Datumsobjekte verwenden, können nicht als XAML-Attributzeichenfolge festgelegt werden, da der Windows-Runtime-XAML-Parser nicht über eine Konvertierungslogik zum Konvertieren von Zeichenfolgen in Datumsangaben als DateTime/DateTimeOffset-Objekte verfügt. In der Regel legen Sie diese Werte im Code fest. Eine andere Möglichkeit besteht darin, ein Datum zu definieren, das als Datenobjekt oder im Datenkontext verfügbar ist, und anschließend die Eigenschaft als XAML-Attribut festzulegen, das auf einen [\{Binding\}-Markuperweiterung](../../xaml-platform/binding-markup-extension.md)-Ausdruck verweist, der auf das Datum als Daten zugreifen kann.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für XAML-Benutzeroberflächengrundlagen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)
- [Kalenderbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Calendar)
- [Beispiel für Datums- und Uhrzeitformatierung](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/DateTimeFormatting)

## <a name="related-topics"></a>Zugehörige Themen

### <a name="for-developers-xaml"></a>Für Entwickler (XAML)

- [CalendarView-Klasse](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)
- [CalendarDatePicker-Klasse](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)
- [DatePicker-Klasse](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)
- [TimePicker-Klasse](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)
