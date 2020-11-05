---
description: Verwenden Sie ein Label, um den Benutzer darauf hinzuweisen, was er in ein benachbartes Steuerelement eingeben soll. Sie können auch eine Gruppe verwandter Steuerelemente beschriften oder in der Nähe davon Anweisungstexte anzeigen.
title: Bezeichnungen
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
ms.openlocfilehash: 9c1dc878280b24424aa4bb20d5a1d6324838b127
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034543"
---
# <a name="labels"></a>Bezeichnungen

 

Ein Label ist der Name bzw. Titel eines Steuerelements oder einer Gruppe verwandter Steuerelemente.

> **Wichtige APIs:** Header-Eigenschaft, [TextBlock-Klasse](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

In XAML verfügen zahlreiche Steuerelemente über eine integrierte Header-Eigenschaft, die Sie zum Anzeigen der Beschriftung verwenden. Für Steuerelemente ohne Header-Eigenschaft oder zum Beschriften von Steuerelementgruppen können Sie stattdessen ein [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element verwenden.

![Screenshot mit einem standardmäßigen Beschriftungssteuerelement](images/label-standard.png)

## <a name="recommendations"></a>Empfehlungen


-   Verwenden Sie ein Label, um den Benutzer darauf hinzuweisen, was er in ein benachbartes Steuerelement eingeben soll. Sie können auch eine Gruppe verwandter Steuerelemente beschriften oder in der Nähe davon Anweisungstexte anzeigen.
-   Wenn Sie Steuerelemente beschriften, formulieren Sie die Beschriftung als Substantiv oder als präzisen substantivierten Ausdruck und nicht als Satz oder Anweisungstext. Vermeiden Sie die Verwendung von Doppelpunkten oder anderen Satzzeichen.
-   Wenn Sie in einer Bezeichnung Anweisungstext nutzen, können Sie bei der Länge von Textzeichenfolgen großzügiger sein und auch Satzzeichen verwenden.


## <a name="get-the-sample-code"></a>Beispielcode herunterladen
* [Beispiel für XAML-Benutzeroberflächengrundlagen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Zugehörige Themen
* [Textsteuerelemente](text-controls.md)
* [TextBox.Header-Eigenschaft](/uwp/api/windows.ui.xaml.controls.textbox.header)
* [PasswordBox.Header-Eigenschaft](/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [ToggleSwitch.Header-Eigenschaft](/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [DatePicker.Header-Eigenschaft](/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [TimePicker.Header-Eigenschaft](/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Slider.Header-Eigenschaft](/uwp/api/windows.ui.xaml.controls.slider.header)
* [ComboBox.Header-Eigenschaft](/uwp/api/windows.ui.xaml.controls.combobox.header)
* [RichEditBox.Header-Eigenschaft](/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [TextBlock-Klasse](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 
