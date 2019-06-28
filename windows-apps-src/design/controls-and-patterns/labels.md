---
Description: Verwenden Sie ein Label, um den Benutzer darauf hinzuweisen, was er in ein benachbartes Steuerelement eingeben soll. Sie können auch eine Gruppe verwandter Steuerelemente beschriften oder in der Nähe davon Anweisungstexte anzeigen.
title: Beschriftungen
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5996fb15c0d7302c7360c2e45613f0da2720d415
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66364650"
---
# <a name="labels"></a>Beschriftungen

 

Ein Label ist der Name bzw. Titel eines Steuerelements oder einer Gruppe verwandter Steuerelemente.

> **Wichtige APIs:** Header-Eigenschaft, [TextBlock-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

In XAML verfügen zahlreiche Steuerelemente über eine integrierte Header-Eigenschaft, die Sie zum Anzeigen der Beschriftung verwenden. Für Steuerelemente ohne Header-Eigenschaft oder zum Beschriften von Steuerelementgruppen können Sie stattdessen ein [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element verwenden.

![Screenshot mit einem standardmäßigen Beschriftungssteuerelement](images/label-standard.png)

## <a name="recommendations"></a>Empfehlungen


-   Verwenden Sie ein Label, um den Benutzer darauf hinzuweisen, was er in ein benachbartes Steuerelement eingeben soll. Sie können auch eine Gruppe verwandter Steuerelemente beschriften oder in der Nähe davon Anweisungstexte anzeigen.
-   Wenn Sie Steuerelemente beschriften, formulieren Sie die Beschriftung als Substantiv oder als präzisen substantivierten Ausdruck und nicht als Satz oder Anweisungstext. Vermeiden Sie die Verwendung von Doppelpunkten oder anderen Satzzeichen.
-   Wenn Sie in einer Bezeichnung Anweisungstext nutzen, können Sie bei der Länge von Textzeichenfolgen großzügiger sein und auch Satzzeichen verwenden.


## <a name="get-the-sample-code"></a>Beispielcode herunterladen
* [Beispiel für XAML-Benutzeroberflächengrundlagen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Verwandte Themen
* [Textsteuerelemente](text-controls.md)
* [TextBox.Header-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.header)
* [PasswordBox.Header-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [ToggleSwitch.Header-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [DatePicker.Header-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [TimePicker.Header-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Slider.Header-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider.header)
* [ComboBox.Header-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.header)
* [RichEditBox.Header-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [TextBlock-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 




