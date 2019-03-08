---
Description: Verwenden Sie ein Label, um den Benutzer darauf hinzuweisen, was er in ein benachbartes Steuerelement eingeben soll. Sie können auch eine Gruppe verwandter Steuerelemente beschriften oder in der Nähe davon Anweisungstexte anzeigen.
title: Beschriftungen
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4345daf5b879fed7ba9805e4a448c473299031d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654145"
---
# <a name="labels"></a>Beschriftungen

 

Ein Label ist der Name bzw. Titel eines Steuerelements oder einer Gruppe verwandter Steuerelemente.

> **Wichtige APIs**: Header-Eigenschaft, [TextBlock-Klasse](https://msdn.microsoft.com/library/windows/apps/br209652)

In XAML verfügen zahlreiche Steuerelemente über eine integrierte Header-Eigenschaft, die Sie zum Anzeigen der Beschriftung verwenden. Für Steuerelemente ohne Header-Eigenschaft oder zum Beschriften von Steuerelementgruppen können Sie stattdessen ein [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)-Element verwenden.

![Screenshot mit einem standardmäßigen Beschriftungssteuerelement](images/label-standard.png)

## <a name="recommendations"></a>Empfehlungen


-   Verwenden Sie ein Label, um den Benutzer darauf hinzuweisen, was er in ein benachbartes Steuerelement eingeben soll. Sie können auch eine Gruppe verwandter Steuerelemente beschriften oder in der Nähe davon Anweisungstexte anzeigen.
-   Wenn Sie Steuerelemente beschriften, formulieren Sie die Beschriftung als Substantiv oder als präzisen substantivierten Ausdruck und nicht als Satz oder Anweisungstext. Vermeiden Sie die Verwendung von Doppelpunkten oder anderen Satzzeichen.
-   Wenn Sie in einer Bezeichnung Anweisungstext nutzen, können Sie bei der Länge von Textzeichenfolgen großzügiger sein und auch Satzzeichen verwenden.


## <a name="get-the-sample-code"></a>Beispielcode herunterladen
* [Beispiel für XAML UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Verwandte Themen
* [Textsteuerelemente](text-controls.md)
* [TextBox.Header-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [PasswordBox.Header-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [ToggleSwitch.Header-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/br209713)
* [DatePicker.Header-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [TimePicker.Header-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [Slider.Header-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [ComboBox.Header-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [RichEditBox.Header property](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [TextBlock-Klasse](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 




