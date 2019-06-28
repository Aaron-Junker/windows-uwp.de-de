---
Description: Mit der Datumsauswahl verfügen Sie über eine standardisierte Methode, Benutzern die Möglichkeit zu bieten, einen lokalisierten Datumswert per Touch-, Maus- oder Tastatureingabe auszuwählen.
title: Datumsauswahl
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7e0a7d1732833f8e6fc750f8ee481fa3c1116b50
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66823600"
---
# <a name="date-picker"></a>Datumsauswahl

 

Mit der Datumsauswahl verfügen Sie über eine standardisierte Methode, Benutzern die Möglichkeit zu bieten, einen lokalisierten Datumswert per Touch-, Maus- oder Tastatureingabe auszuwählen. 

> **Wichtige APIs:** [DatePicker-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [Date-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepicker.date)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?
Verwenden Sie eine Datumsauswahl, damit Benutzer ein bekanntes Datum wie etwa einen Geburtstag auswählen können, bei dem der Kalenderkontext unwichtig ist.

Weitere Informationen zur Auswahl des richtigen Datumssteuerelements finden Sie im Artikel über [Datums- und Uhrzeitsteuerelemente](date-and-time.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
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

> **Hinweis**&nbsp;&nbsp;Wichtige Informationen zu Datumswerten finden Sie im Artikel zu Datums- und Uhrzeitsteuerelementen unter [DateTime- und Calendar-Werte](date-and-time.md#datetime-and-calendar-values).

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery): Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Datums- und Uhrzeitsteuerelemente](date-and-time.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Kalenderansicht](calendar-view.md)
- [Uhrzeitauswahl](time-picker.md)
