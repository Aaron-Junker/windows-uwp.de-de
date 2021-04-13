---
description: Mit der Zeitauswahl verfügen Sie über eine standardmäßige Methode, mit der die Benutzer einen Zeitwert per Touch-, Maus- oder Tastatureingabe auswählen können.
title: Zeitauswahl
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c3d5391e3d738e7de81362a5fd0e42dc6d007dd
ms.sourcegitcommit: cc871be2508f52509b6a947fe879aeec360d0fd2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106270237"
---
# <a name="time-picker"></a>Zeitauswahl
 
Mit der Zeitauswahl verfügen Sie über eine standardmäßige Methode, mit der die Benutzer einen Zeitwert per Touch-, Maus- oder Tastatureingabe auswählen können.

![Beispiel für Zeitauswahl](images/time-picker-closed.png)

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

> **Plattform-APIs**: [TimePicker-Klasse](/uwp/api/Windows.UI.Xaml.Controls.TimePicker), [SelectedTime-Eigenschaft](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?
Verwenden Sie die Zeitauswahl, um Benutzern die Auswahl eines einzelnen Zeitwerts zu ermöglichen.

Weitere Informationen zur Auswahl des passenden Steuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/TimePicker">die App zu öffnen und TimePicker in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Der Einstiegspunkt zeigt die ausgewählte Uhrzeit an, und wenn der Benutzer den Einstiegspunkt auswählt, wird eine Auswahloberfläche von der Bildschirmmitte aus vertikal erweitert, damit er eine Auswahl treffen kann. Die Zeitauswahl überlagert andere Elemente der Benutzeroberfläche. Die anderen Elemente der Benutzeroberfläche werden dadurch jedoch nicht „verschoben“.

![Beispiel für die Erweiterung der Zeitauswahl](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>Erstellen einer Zeitauswahl

Dieses Beispiel zeigt, wie Sie ein einfaches Zeitauswahl-Steuerelement mit einer Kopfzeile erstellen.

```xaml
<TimePicker x:Name="arrivalTimePicker" Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

Die fertige Zeitauswahl sieht wie folgt aus:

![Beispiel für Zeitauswahl](images/time-picker-closed.png)

### <a name="formatting-the-time-picker"></a>Formatieren der Zeitauswahl

Standardmäßig zeigt die Zeitauswahl eine Uhr im 12-Stunden-Format mit einer am/pm-Auswahl an. Sie können die [ClockIdentifier](/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier)-Eigenschaft auf „24HourClock“ festlegen, um stattdessen eine Uhr im 24-Stunden-Format anzuzeigen.

```xaml
<TimePicker Header="12HourClock" SelectedTime="14:30"/>
<TimePicker Header="24HourClock" SelectedTime="14:30" ClockIdentifier="24HourClock"/>
```

:::image type="content" source="images/date-time/time-picker-clocks.png" alt-text="Zeitauswahl mit einer 12-Stunden-Uhr und Auswahl mit einer 24-Stunden-Uhr.":::

Sie können die [MinuteIncrement](/uwp/api/windows.ui.xaml.controls.timepicker.minuteincrement)-Eigenschaft so festlegen, dass die in der Minuten-Auswahl angezeigten Zeitschritte angegeben werden. „15“ gibt beispielsweise an, dass das `TimePicker`-Minuten-Steuerelement nur die Optionen „00“, „15“, „30“ und „45“ anzeigt.

```xaml
<TimePicker MinuteIncrement="15"/>
```

:::image type="content" source="images/date-time/time-picker-minute-increment.png" alt-text="Zeitauswahl, die 15-Minutenschritte anzeigt.":::

### <a name="time-values"></a>Uhrzeitwerte

Das Steuerelement für die Zeitauswahl verfügt sowohl über [Time](/uwp/api/windows.ui.xaml.controls.timepicker.time)/[TimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.timechanged)- als auch [SelectedTime](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime)/[SelectedTimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtimechanged)-APIs. Der Unterschied zwischen diesen besteht darin, dass `Time` keine NULL-Werte zulässt, während `SelectedTime` auf NULL festgelegt werden kann.

Der Wert von `SelectedTime` wird verwendet, um die Zeitauswahl aufzufüllen, und lautet standardmäßig `null`. Wenn `SelectedTime` den Wert `null` aufweist, wird die `Time`-Eigenschaft auf eine [TimeSpan](/dotnet/api/system.timespan?view=dotnet-uwp-10.0&preserve-view=true) von 0 festgelegt. Andernfalls wird der `Time`-Wert mit dem `SelectedTime`-Wert synchronisiert. Wenn `SelectedTime` den Wert `null` aufweist, ist die Auswahl „nicht festgelegt“ und zeigt die Feldnamen anstelle einer Uhrzeit an.

:::image type="content" source="images/date-time/time-picker-no-selected-time.png" alt-text="Zeitauswahl, bei der keine Uhrzeit ausgewählt ist.":::

#### <a name="initializing-a-time-value"></a>Initialisieren eines Uhrzeitwerts

Im Code können Sie die Uhrzeiteigenschaften mit einem Wert vom Typ `TimeSpan` initialisieren:

```csharp
TimePicker timePicker = new TimePicker
{
    SelectedTime = new TimeSpan(14, 15, 00) // Seconds are ignored.
};
```

Sie können den Uhrzeitwert als Attribut in XAML festlegen. Dies ist wahrscheinlich am einfachsten, wenn Sie das `TimePicker`-Objekt bereits in XAML deklarieren und keine Bindungen für den Uhrzeitwert verwenden. Verwenden Sie eine Zeichenfolge im Format *Hh:Mm*, wobei *Hh* die Stunden (zwischen 0 und 23) und *Mm* die Minuten (zwischen 0 und 59) angibt.

```xaml
<TimePicker SelectedTime="14:15"/>
```

> [!NOTE]
> Wichtige Informationen zu Uhrzeit- und Datumswerten finden Sie unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values) im Artikel *Steuerelemente für Datum und Uhrzeit*.

### <a name="using-the-time-values"></a>Verwenden der Uhrzeitwerte

Wenn Sie den Uhrzeitwert in Ihrer App verwenden möchten, können Sie eine Datenbindung an die [SelectedTime](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime)- oder [Time](/uwp/api/windows.ui.xaml.controls.timepicker.time)-Eigenschaft verwenden, die Uhrzeiteigenschaften direkt in Ihrem Code verwenden oder das [SelectedTimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtimechanged)- oder [TimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.timechanged)-Ereignis behandeln.

> Ein Beispiel für die gemeinsame Verwendung von `DatePicker` und `TimePicker` zum Aktualisieren eines einzelnen `DateTime`-Werts finden Sie unter [Kalender-, Datums- und Uhrzeitsteuerelemente – Gemeinsame Verwendung von Datumsauswahl und Zeitauswahl](/windows/uwp/design/controls-and-patterns/date-and-time#use-a-date-picker-and-time-picker-together).

Hier wird die `SelectedTime`-Eigenschaft verwendet, um die ausgewählte Uhrzeit mit der aktuellen Uhrzeit zu vergleichen.

Beachten Sie Folgendes: Da die `SelectedTime`-Eigenschaft NULL-Werte zulässt, müssen Sie diese explizit wie folgt in `DateTime` umwandeln: `DateTime myTime = (DateTime)(DateTime.Today + checkTimePicker.SelectedTime);`. Die `Time`-Eigenschaft kann jedoch wie folgt ohne eine Umwandlung verwendet werden: `DateTime myTime = DateTime.Today + checkTimePicker.Time;`.

:::image type="content" source="images/date-time/time-picker-check.png" alt-text="Zeitauswahl, Schaltfläche und Beschriftung.":::

```xaml
<StackPanel>
    <TimePicker x:Name="checkTimePicker"/>
    <Button Content="Check time" Click="{x:Bind CheckTime}"/>
    <TextBlock x:Name="resultText"/>
</StackPanel>
```

```csharp
private void CheckTime()
{
    // Using the Time property.
    // DateTime myTime = DateTime.Today + checkTimePicker.Time;
    // Using the SelectedTime property (nullable requires cast to DateTime).
    DateTime myTime = (DateTime)(DateTime.Today + checkTimePicker.SelectedTime);
    if (DateTime.Now >= myTime)
    {
        resultText.Text = "Your selected time has already past.";
    }
    else
    {
        string hrs = (myTime - DateTime.Now).Hours.ToString();
        string mins = (myTime - DateTime.Now).Minutes.ToString();
        resultText.Text = string.Format("Your selected time is {0} hours, {1} minutes from now.", hrs, mins);
    }
}
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Zugehörige Themen

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Kalenderansicht](calendar-view.md)
- [Datumsauswahl](date-picker.md)
