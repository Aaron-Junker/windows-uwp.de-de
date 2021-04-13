---
description: Mit der Datumsauswahl verfügen Sie über eine standardisierte Methode, Benutzern die Möglichkeit zu bieten, einen lokalisierten Datumswert per Touch-, Maus- oder Tastatureingabe auszuwählen.
title: Datumsauswahl
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 51fa98908fa4a2c1c026ce5a4c924b8f800aee8b
ms.sourcegitcommit: 62a6e7b4d35f63c25cedd61c96dfc251ff19c80d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286599"
---
# <a name="date-picker"></a>Datumsauswahl

Mit der Datumsauswahl verfügen Sie über eine standardisierte Methode, Benutzern die Möglichkeit zu bieten, einen lokalisierten Datumswert per Touch-, Maus- oder Tastatureingabe auszuwählen.

![Beispiel für eine Datumsauswahl](images/date-picker-closed.png)

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Die Bibliothek „Windows UI“ enthält ab Version 2.2 eine neue Vorlage für dieses Steuerelement, die abgerundete Ecken verwendet. Weitere Informationen finden Sie unter [Eckradius](../style/rounded-corner.md). „WinUI“ ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Plattform-APIs:** [DatePicker-Klasse](/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [SelectedDate-Eigenschaft](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie eine Datumsauswahl, damit Benutzer ein bekanntes Datum wie etwa einen Geburtstag auswählen können, bei dem der Kalenderkontext unwichtig ist.

Wenn der Kontext eines Kalenders wichtig ist, sollten Sie die Verwendung einer [Kalenderdatumsauswahl](calendar-date-picker.md) oder einer [Kalenderansicht](calendar-view.md) in Erwägung ziehen.

Weitere Informationen zur Auswahl des richtigen Datumssteuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/DatePicker">die App zu öffnen und DatePicker in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Der Einstiegspunkt zeigt das ausgewählte Datum an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit eine Auswahl getroffen werden kann. Die Datumsauswahl überlagert andere Elemente der Benutzeroberfläche; die anderen Elemente der Benutzeroberfläche werden jedoch nicht „beiseitegeschoben“.

![Beispiel für die Erweiterung der Datumsauswahl](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>Erstellen einer Datumsauswahl

Dieses Beispiel zeigt, wie Sie eine einfache Datumsauswahl mit einer Kopfzeile erstellen.

```xaml
<DatePicker x:Name="birthDatePicker" Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

Die fertige Datumsauswahl sieht wie folgt aus:

![Beispiel für eine Datumsauswahl](images/date-picker-closed.png)

### <a name="formatting-the-date-picker"></a>Formatieren der Datumsauswahl

Standardmäßig zeigt die Datumsauswahl den Tag, den Monat und das Jahr an. Wenn in Ihrem Szenario für die Datumsauswahl nicht alle Felder erforderlich sind, können Sie diejenigen ausblenden, die Sie nicht benötigen. Wenn Sie ein Feld ausblenden möchten, legen Sie die zugehörige *Visible*-Eigenschaft für das Feld auf `false` fest: [DayVisible](/uwp/api/windows.ui.xaml.controls.datepicker.dayvisible), [MonthVisible](/uwp/api/windows.ui.xaml.controls.datepicker.monthvisible) oder [YearVisible](/uwp/api/windows.ui.xaml.controls.datepicker.yearvisible).

Hier ist nur das Jahr erforderlich, daher sind die Felder „Tag“ und „Monat“ ausgeblendet.

```xaml
<DatePicker x:Name="yearDatePicker" Header="In what year was Microsoft founded?" 
            MonthVisible="False" DayVisible="False"/>
```

:::image type="content" source="images/date-time/date-picker-year-only.png" alt-text="Datumsauswahl, bei der die Felder „Tag“ und „Monat“ ausgeblendet sind.":::

Der Zeichenfolgeninhalt jedes `ComboBox` in der `DatePicker` wird von einem [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting.datetimeformatter) erstellt. Anhand einer Zeichenfolge, die entweder eine *Formatvorlage* oder ein *Formatmuster* ist, informieren Sie `DateTimeFormatter`, wie der Datumswert formatiert werden soll. Weitere Informationen finden Sie in den Eigenschaften [DayFormat](/uwp/api/windows.ui.xaml.controls.datepicker.dayformat), [MonthFormat](/uwp/api/windows.ui.xaml.controls.datepicker.monthformat) und [YearFormat](/uwp/api/windows.ui.xaml.controls.datepicker.yearformat).

Hier wird ein *Formatmuster* verwendet, um den Monat als Ganzzahl und Abkürzung anzuzeigen. Sie können dem Formatierungsmuster Literalzeichenfolgen hinzufügen, z. B. Klammern um das Monatskürzel: `({month.abbreviated})`.

```xaml
<DatePicker MonthFormat="{}{month.integer(2)} ({month.abbreviated})" DayVisible="False"/>
```

:::image type="content" source="images/date-time/date-picker-day-hidden.png" alt-text="Datumsauswahl, bei der das Feld für den Tag ausgeblendet ist.":::

### <a name="date-values"></a>Datumswerte

Das Steuerelement für die Datumsauswahl verfügt sowohl über die [Date](/uwp/api/windows.ui.xaml.controls.datepicker.date)/[DateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.datechanged)- als auch [SelectedDate](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate)/[SelectedDateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddatechanged)-APIs. Der Unterschied zwischen diesen besteht darin, dass `Date` keine NULL-Werte zulässt, während `SelectedDate` auf NULL festgelegt werden kann.

Der Wert von `SelectedDate` wird verwendet, um die Datumsauswahl aufzufüllen, und lautet standardmäßig `null`. Wenn `SelectedDate` den Wert `null` aufweist, wird die `Date`-Eigenschaft auf 12/31/1600 festgelegt. Andernfalls wird der `Date`-Wert mit dem `SelectedDate`-Wert synchronisiert. Wenn `SelectedDate` den Wert `null` aufweist, ist die Auswahl „nicht festgelegt“ und zeigt die Feldnamen anstelle eines Datums an.

:::image type="content" source="images/date-time/date-picker-no-selected-date.png" alt-text="Eine Datumsauswahl ohne ausgewähltes Datum.":::

Sie können die Eigenschaften [MinYear](/uwp/api/windows.ui.xaml.controls.datepicker.minyear) und [MaxYear](/uwp/api/windows.ui.xaml.controls.datepicker.maxyear) festlegen, um die Datumswerte in der Auswahl einzuschränken. Standardmäßig ist `MinYear` auf 100 Jahre vor dem aktuellen Datum und `MaxYear` auf 100 Jahre nach dem aktuellen Datum festgelegt.

Wenn Sie nur `MinYear` oder `MaxYear` festlegen, müssen Sie sicherstellen, dass durch das festgelegte Datum und den Standardwert des anderen Datums ein gültiger Datumsbereich gebildet wird, andernfalls ist im Auswahlfeld kein Datum zur Auswahl verfügbar. Wenn Sie beispielsweise nur `yearDatePicker.MaxYear = new DateTimeOffset(new DateTime(900, 1, 1));` festlegen, wird ein ungültiger Datumsbereich mit dem Standardwert `MinYear` erstellt.

#### <a name="initializing-a-date-value"></a>Initialisieren eines Datumswerts

Die Datumseigenschaften können nicht als XAML-Attributzeichenfolge festgelegt werden, da der Windows-Runtime-XAML-Parser nicht über eine Konvertierungslogik zum Konvertieren von Zeichenfolgen in Datumsangaben als [DateTime](/uwp/api/windows.foundation.datetime) / [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true)-Objekte verfügt. Hier finden Sie einige Vorschläge, wie diese Objekte im Code definiert und auf ein anderes als das aktuelle Datum festgelegt werden können.

- [DateTime](/uwp/api/windows.foundation.datetime): Instanziieren Sie ein [Windows.Globalization.Calendar](/uwp/api/windows.globalization.calendar)-Objekt (es wird mit dem aktuellen Datum initialisiert). Legen Sie [Year](/uwp/api/windows.globalization.calendar.year) fest, oder rufen Sie [AddYears](/uwp/api/windows.globalization.calendar.addyears) auf, um das Datum anzupassen. Rufen Sie dann [Calendar.GetDateTime](/uwp/api/windows.globalization.calendar.getdatetime) auf, und verwenden Sie den zurückgegebenen `DateTime`-Wert, um die Datumseigenschaft festzulegen.
- [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true): Rufen Sie den Konstruktor auf. Verwenden Sie für den inneren [System.DateTime](/dotnet/api/system.datetime?view=dotnet-uwp-10.0&preserve-view=true)-Wert die Konstruktorsignatur. Oder erstellen Sie einen standardmäßigen [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true)-Wert (der mit dem aktuellen Datum initialisiert wird), und rufen Sie [AddYears](/dotnet/api/system.datetimeoffset.addyears?view=dotnet-uwp-10.0&preserve-view=true) auf.

Eine andere Möglichkeit besteht darin, ein Datum zu definieren, das als Datenobjekt oder im Datenkontext verfügbar ist, und anschließend die Datumseigenschaft als XAML-Attribut festzulegen, das auf eine [{Binding}-Markuperweiterung](/windows/uwp/xaml-platform/binding-markup-extension) verweist, die auf das Datum als Daten zugreifen kann.

> [!NOTE]
> Wichtige Informationen zu Datumswerten finden Sie im Artikel zu Datums- und Uhrzeitsteuerelementen unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values).

In diesem Beispiel wird veranschaulicht, wie die Eigenschaften `SelectedDate`, `MinYear` und `MaxYear` für verschiedene `DatePicker`-Steuerelemente festgelegt werden.

```xaml
<DatePicker x:Name="yearDatePicker" MonthVisible="False" DayVisible="False"/>
<DatePicker x:Name="arrivalDatePicker" Header="Arrival date"/>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Set minimum year to 1900 and maximum year to 1999.
    yearDatePicker.SelectedDate = new DateTimeOffset(new DateTime(1950, 1, 1));
    yearDatePicker.MinYear = new DateTimeOffset(new DateTime(1900, 1, 1));
    // Using a different DateTimeOffset constructor.
    yearDatePicker.MaxYear = new DateTimeOffset(1999, 12, 31, 0, 0, 0, new TimeSpan());

    // Set minimum to the current year and maximum to five years from now.
    arrivalDatePicker.MinYear = DateTimeOffset.Now;
    arrivalDatePicker.MaxYear = DateTimeOffset.Now.AddYears(5);
}
```

### <a name="using-the-date-values"></a>Verwenden der Datumswerte

Um den Datumswert in Ihrer App zu verwenden, nutzen Sie typischerweise eine Datenbindung an die [SelectedDate](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate)-Eigenschaft oder behandeln das Ereignis [SelectedDateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddatechanged).

> Ein Beispiel für die gemeinsame Verwendung von `DatePicker` und `TimePicker` zum Aktualisieren eines einzelnen `DateTime`-Werts finden Sie unter [Kalender-, Datums- und Uhrzeitsteuerelemente – Gemeinsame Verwendung von Datumsauswahl und Zeitauswahl](/windows/uwp/design/controls-and-patterns/date-and-time#use-a-date-picker-and-time-picker-together).

Hier verwenden Sie eine `DatePicker`, um einem Benutzer zu ermöglichen, sein Ankunftsdatum auszuwählen. Sie behandeln das `SelectedDateChanged`-Ereignis, um eine [DateTime](/uwp/api/windows.foundation.datetime)-Instanz namens `arrivalDateTime` zu aktualisieren.

```xaml
<StackPanel>
    <DatePicker x:Name="arrivalDatePicker" Header="Arrival date"
                DayFormat="{}{day.integer} ({dayofweek.abbreviated})"
                SelectedDateChanged="arrivalDatePicker_SelectedDateChanged"/>
    <Button Content="Clear" Click="ClearDateButton_Click"/>
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

    private void arrivalDatePicker_SelectedDateChanged(DatePicker sender, DatePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalDatePicker.SelectedDate != null)
        {
            arrivalDateTime = new DateTime(args.NewDate.Value.Year, args.NewDate.Value.Month, args.NewDate.Value.Day);
        }
        arrivalText.Text = arrivalDateTime.ToString();
    }

    private void ClearDateButton_Click(object sender, RoutedEventArgs e)
    {
        arrivalDatePicker.SelectedDate = null;
        arrivalText.Text = string.Empty;
    }
}
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Kalenderansicht](calendar-view.md)
- [Uhrzeitauswahl](time-picker.md)
