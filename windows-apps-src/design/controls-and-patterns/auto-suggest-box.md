---
Description: A text entry box that provides suggestions as the user types.
title: Richtlinien für Felder mit automatischen Vorschlägen
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b68b3480ac81a445551a070eaf0a1454ff22a9e7
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918686"
---
# <a name="auto-suggest-box"></a>Feld mit automatischen Vorschlägen

Verwenden Sie ein AutoSuggestBox-Element, um eine Liste mit Vorschlägen bereitzustellen, aus der Benutzer bei der Eingabe auswählen können.

> **Wichtige APIs:** [Klasse „AutoSuggestBox“](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx), [Ereignis „TextChanged“](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx), [Ereignis „SuggestionChose“](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx), [Ereignis „QuerySubmitted“](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx)

![Ein Feld mit automatischen Vorschlägen](images/controls/auto-suggest-box-open.png)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Wenn Sie ein einfaches, anpassbares Steuerelement verwenden möchten, das die Textsuche mit einer Liste von Vorschlägen ermöglicht, verwenden Sie ein Feld mit automatischen Vorschlägen.

Weitere Informationen zum Auswählen des passenden Textsteuerelements finden Sie im Artikel über [Textsteuerelemente](text-controls.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/AutoSuggestBox">die App zu öffnen und AutoSuggestBox in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Erwerben Sie den Quellcode (GitHub)</a></li>
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

Das Ereignis [TextChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx) tritt ein, sobald der Inhalt des Textfelds aktualisiert wird. Verwenden Sie die [Reason](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason.aspx)-Eigenschaft für die Ereignisargumente, um zu ermitteln, ob die Änderung aufgrund einer Benutzereingabe erfolgt ist. Wenn der Grund für die Änderung **UserInput** lautet, filtern Sie Ihre Daten basierend auf der Eingabe. Legen Sie die gefilterten Daten anschließend als [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) des AutoSuggestBox-Elements fest, um die Vorschlagsliste zu aktualisieren.

Um zu steuern, wie Elemente in der Vorschlagsliste angezeigt werden, können Sie [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) oder [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) verwenden.

- Wenn Sie den Text einer einzelnen Eigenschaft des Datenelements anzeigen möchten, legen Sie die DisplayMemberPath-Eigenschaft fest, um auszuwählen, welche Eigenschaft Ihres Objekts in der Vorschlagsliste angezeigt werden soll.
- Verwenden Sie zum Definieren einer benutzerdefinierten Darstellung für jedes Element in der Liste die ItemTemplate-Eigenschaft.

### <a name="suggestion-chosen"></a>Vorschlag ausgewählt

Wenn ein Benutzer mit der Tastatur durch die Vorschlagsliste navigiert, müssen Sie den Text im Textfeld entsprechend aktualisieren.

Sie können die [TextMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textmemberpath.aspx)-Eigenschaft festlegen, um auszuwählen, welche Eigenschaft des Datenobjekts im Textfeld angezeigt werden soll. Wenn Sie ein TextMemberPath-Element angeben, wird das Textfeld automatisch aktualisiert. Sie sollten in der Regel den gleichen Wert für DisplayMemberPath and TextMemberPath angeben, damit der Text in der Vorschlagsliste und im Textfeld identisch ist.

Wenn Sie mehr als eine einfache Eigenschaft anzeigen müssen, behandeln Sie das [SuggestionChosen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx)-Ereignis, um das Textfeld mit benutzerdefiniertem Text basierend auf dem ausgewählten Element zu füllen.

### <a name="query-submitted"></a>Abfrage gesendet

Behandeln Sie das [QuerySubmitted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx) -Ereignis, um eine passende Abfrageaktion für die App durchzuführen und das Ergebnis dem Benutzer anzuzeigen.

Das QuerySubmitted-Ereignis tritt ein, wenn ein Benutzer eine Abfragezeichenfolge absendet. Der Benutzer kann diesen Schritt wie folgt ausführen:
- Mit dem Fokus auf dem Textfeld EINGABETASTE drücken oder auf das Abfragesymbol klicken. Die [ChosenSuggestion](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion.aspx)-Eigenschaft für die Ereignisargumente lautet **null**.
- Mit dem Fokus auf der Vorschlagsliste EINGABETASTE drücken oder auf ein Element klicken oder tippen. Die ChosenSuggestion-Eigenschaft für die Ereignisargumente enthält das Element, das in der Liste ausgewählt wurde.

In allen Fällen enthält die [QueryText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext.aspx)-Eigenschaft für die Ereignisargumente den Text aus dem Textfeld.

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

Standardmäßig wird für das Texteingabefeld keine Abfrageschaltfläche angezeigt. Sie können die [QueryIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.queryicon.aspx) -Eigenschaft festlegen, um eine Schaltfläche mit dem angegebenen Symbol rechts vom Textfeld hinzuzufügen. Wenn das AutoSuggestBox-Element wie ein typisches Suchfeld aussehen soll, fügen Sie beispielsweise wie folgt ein Suchsymbol hinzu.

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

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Beispiel für AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Verwandte Artikel

- [Textsteuerelemente](text-controls.md)
- [Rechtschreibprüfung](text-controls.md)
- [Suche](search.md)
- [TextBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br227519)
- [StringLength-Eigenschaft](https://msdn.microsoft.com/library/system.string.length.aspx)
