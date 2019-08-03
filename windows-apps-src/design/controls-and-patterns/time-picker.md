---
Description: Mit der Zeitauswahl verfügen Sie über eine standardmäßige Methode, mit der die Benutzer einen Zeitwert per Touch-, Maus- oder Tastatureingabe auswählen können.
title: Zeitauswahl
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.date: 05/08/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e8be583778ccbf47c61466033c58c784c4df4395
ms.sourcegitcommit: e0ae346eadda864dcad1453cd1644668549e66e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603418"
---
# <a name="time-picker"></a>Zeitauswahl
 

Mit der Zeitauswahl verfügen Sie über eine standardmäßige Methode, mit der die Benutzer einen Zeitwert per Touch-, Maus- oder Tastatureingabe auswählen können. 

> **Wichtige APIs:** [TimePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker), [Time-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.time)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?
Verwenden Sie die Zeitauswahl, um Benutzern die Auswahl eines einzelnen Zeitwerts zu ermöglichen.

Weitere Informationen zur Auswahl des passenden Steuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
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

> [!NOTE]
> Wichtige Informationen zu Uhrzeit- und Datumswerten finden Sie unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values) im Artikel *Steuerelemente für Datum und Uhrzeit*.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Verwandte Themen

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Kalenderansicht](calendar-view.md)
- [Datumsauswahl](date-picker.md)
