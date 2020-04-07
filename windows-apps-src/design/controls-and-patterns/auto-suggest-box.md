---
Description: Ein Texteingabefeld, das während der Benutzereingabe Vorschläge anzeigt.
title: Richtlinien für Felder mit automatischen Vorschlägen
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e8a4c347bd2baa51115ecd9315f923e205133a6e
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081274"
---
# <a name="auto-suggest-box"></a>Feld mit automatischen Vorschlägen

Verwenden Sie ein AutoSuggestBox-Element, um eine Liste mit Vorschlägen bereitzustellen, aus der Benutzer während der Eingabe auswählen können.

![Ein Feld mit automatischen Vorschlägen](images/controls/auto-suggest-box-open.png)

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Die Bibliothek „Windows UI“ enthält ab Version 2.2 eine neue Vorlage für dieses Steuerelement, die abgerundete Ecken verwendet. Weitere Informationen finden Sie unter [Eckradius](/windows/uwp/design/style/rounded-corner). „Windows UI“ ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für UWP-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Plattform-APIs:** [Klasse „AutoSuggestBox“](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox), [Ereignis „TextChanged“](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged), [Ereignis „SuggestionChosen“](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen), [Ereignis „QuerySubmitted“](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Wenn Sie ein einfaches, anpassbares Steuerelement verwenden möchten, das die Textsuche mit einer Liste von Vorschlägen ermöglicht, verwenden Sie ein Feld mit automatischen Vorschlägen.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel [Textsteuerelemente](text-controls.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Falls die App <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> installiert ist, klicke <a href="xamlcontrolsgallery:/item/AutoSuggestBox">hier</a>, um die App zu öffnen und „AutoSuggestBox“ in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Ein Feld mit automatischen Vorschlägen in der Groove-Musik-App.

![Ein Feld mit automatischen Vorschlägen in der Groove-Musik-App](images/control-examples/auto-suggest-box-groove.png)

## <a name="anatomy"></a>Aufbau
Der Einstiegspunkt für das Feld mit automatischen Vorschlägen besteht aus einem optionalen Header und einem Feld mit optionalem Hinweistext:

![Beispiel für den Einstiegspunkt für das Steuerelement für automatische Vorschläge](images/controls_autosuggest_entrypoint.png)

Die Ergebnisliste für automatische Vorschläge wird automatisch ausgefüllt, sobald der Benutzer mit der Texteingabe beginnt. Die Ergebnisliste kann oberhalb oder unterhalb des Texteingabefelds angezeigt werden. Eine Schaltfläche „Alles löschen” wird angezeigt:

![Beispiel für das erweiterte Steuerelement für automatische Vorschläge](images/controls_autosuggest_expanded01.png)

## <a name="create-an-auto-suggest-box"></a>Erstellen eines Felds mit automatischen Vorschlägen

Zum Verwenden eines AutoSuggestBox-Elements müssen Sie auf drei Benutzeraktionen reagieren.

- Text geändert: Aktualisieren Sie die Vorschlagsliste, wenn der Benutzer Text eingibt.
- Vorschlag ausgewählt: Aktualisieren Sie das Textfeld, wenn der Benutzer in der Vorschlagsliste einen Vorschlag auswählt.
- Abfrage gesendet: Zeigen Sie die Abfrageergebnisse an, wenn der Benutzer eine Abfrage sendet.

### <a name="text-changed"></a>Text geändert

Das [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textchanged)-Ereignis tritt auf, wenn der Inhalt des Textfelds aktualisiert wird. Verwenden Sie die [Reason](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason)-Eigenschaft für die Ereignisargumente, um zu ermitteln, ob die Änderung aufgrund einer Benutzereingabe erfolgt ist. Wenn der Grund für die Änderung **UserInput** lautet, filtern Sie Ihre Daten basierend auf der Eingabe. Legen Sie die gefilterten Daten anschließend als [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) des AutoSuggestBox-Elements fest, um die Vorschlagsliste zu aktualisieren.

Um zu steuern, wie Elemente in der Vorschlagsliste angezeigt werden, können Sie [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) oder [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) verwenden.

- Wenn Sie den Text einer einzelnen Eigenschaft des Datenelements anzeigen möchten, legen Sie die DisplayMemberPath-Eigenschaft fest, um auszuwählen, welche Eigenschaft Ihres Objekts in der Vorschlagsliste angezeigt werden soll.
- Verwenden Sie zum Definieren einer benutzerdefinierten Darstellung für jedes Element in der Liste die ItemTemplate-Eigenschaft.

### <a name="suggestion-chosen"></a>Vorschlag ausgewählt

Wenn ein Benutzer mit der Tastatur durch die Vorschlagsliste navigiert, müssen Sie den Text im Textfeld entsprechend aktualisieren.

Sie können die [TextMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.textmemberpath)-Eigenschaft festlegen, um auszuwählen, welche Eigenschaft des Datenobjekts im Textfeld angezeigt werden soll. Wenn Sie ein TextMemberPath-Element angeben, wird das Textfeld automatisch aktualisiert. Sie sollten in der Regel den gleichen Wert für DisplayMemberPath and TextMemberPath angeben, damit der Text in der Vorschlagsliste und im Textfeld identisch ist.

Wenn Sie mehr als eine einfache Eigenschaft anzeigen müssen, behandeln Sie das [SuggestionChosen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.suggestionchosen)-Ereignis, um das Textfeld mit benutzerdefiniertem Text basierend auf dem ausgewählten Element zu füllen.

### <a name="query-submitted"></a>Abfrage gesendet

Behandeln Sie das [QuerySubmitted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.querysubmitted) -Ereignis, um eine passende Abfrageaktion für die App durchzuführen und das Ergebnis dem Benutzer anzuzeigen.

Das QuerySubmitted-Ereignis tritt ein, wenn ein Benutzer eine Abfragezeichenfolge absendet. Der Benutzer kann diesen Schritt wie folgt ausführen:
- Mit dem Fokus auf dem Textfeld EINGABETASTE drücken oder auf das Abfragesymbol klicken. Die [ChosenSuggestion](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion)-Eigenschaft für die Ereignisargumente lautet **null**.
- Mit dem Fokus auf der Vorschlagsliste EINGABETASTE drücken oder auf ein Element klicken oder tippen. Die ChosenSuggestion-Eigenschaft für die Ereignisargumente enthält das Element, das in der Liste ausgewählt wurde.

In allen Fällen enthält die [QueryText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext)-Eigenschaft für die Ereignisargumente den Text aus dem Textfeld.

Dies ist ein einfaches AutoSuggestBox-Element mit den erforderlichen Ereignishandlern.

```xaml
<AutoSuggestBox PlaceholderText="Search" QueryIcon="Find" Width="200"
                TextChanged="AutoSuggestBox_TextChanged"
                QuerySubmitted="AutoSuggestBox_QuerySubmitted"
                SuggestionChosen="AutoSuggestBox_SuggestionChosen"/>
```

```csharp
private void AutoSuggestBox_TextChanged(AutoSuggestBox sender, AutoSuggestBoxTextChangedEventArgs args)
{
    // Only get results when it was a user typing,
    // otherwise assume the value got filled in by TextMemberPath
    // or the handler for SuggestionChosen.
    if (args.Reason == AutoSuggestionBoxTextChangeReason.UserInput)
    {
        //Set the ItemsSource to be your filtered dataset
        //sender.ItemsSource = dataset;
    }
}


private void AutoSuggestBox_SuggestionChosen(AutoSuggestBox sender, AutoSuggestBoxSuggestionChosenEventArgs args)
{
    // Set sender.Text. You can use args.SelectedItem to build your text string.
}


private void AutoSuggestBox_QuerySubmitted(AutoSuggestBox sender, AutoSuggestBoxQuerySubmittedEventArgs args)
{
    if (args.ChosenSuggestion != null)
    {
        // User selected an item from the suggestion list, take an action on it here.
    }
    else
    {
        // Use args.QueryText to determine what to do.
    }
}
```

## <a name="use-autosuggestbox-for-search"></a>Verwenden von AutoSuggestBox für die Suche

Verwenden Sie ein AutoSuggestBox-Element, um eine Liste mit Vorschlägen bereitzustellen, aus der Benutzer während der Eingabe auswählen können.

Standardmäßig wird für das Texteingabefeld keine Abfrageschaltfläche angezeigt. Sie können die [QueryIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox.queryicon) -Eigenschaft festlegen, um eine Schaltfläche mit dem angegebenen Symbol rechts vom Textfeld hinzuzufügen. Wenn das AutoSuggestBox-Element wie ein typisches Suchfeld aussehen soll, fügen Sie beispielsweise wie folgt ein Suchsymbol hinzu.

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

Hier ist ein AutoSuggestBox-Element mit einem Suchsymbol dargestellt.

![Beispiel für den Einstiegspunkt für das Steuerelement für automatische Vorschläge](images/controls_autosuggest_entrypoint.png)

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

-   Zeigen Sie die einzeilige Meldung „Keine Ergebnisse” an, wenn Sie das Feld mit automatischen Vorschlägen zum Durchführen von Suchen verwenden und keine Suchergebnisse für den eingegebenen Text vorhanden sind, damit Benutzer wissen, dass ihre Suchanfrage ausgeführt wurde:

    ![Beispiel für ein Feld mit automatischen Vorschlägen ohne Suchergebnisse](images/controls_autosuggest_noresults.png)

<!--
<div class="microsoft-internal-note">
**Globalization and localization checklist**

<table>
<tr>
<th>Vertical spacing</th><td>Use non-Latin characters for vertical spacing to ensure non-Latin scripts will display properly, including numbers.</td>
</tr>
<tr>
<th>Scrolling</th><td>When auto suggest text is selected, user should be able to scroll to end of string.</td>
</tr>
</table>
</div>
-->

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Beispiel für AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Verwandte Artikel

- [Textsteuerelemente](text-controls.md)
- [Rechtschreibprüfung](text-controls.md)
- [Suche](search.md)
- [TextBox-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [StringLength-Eigenschaft](https://docs.microsoft.com/dotnet/api/system.string.length)
