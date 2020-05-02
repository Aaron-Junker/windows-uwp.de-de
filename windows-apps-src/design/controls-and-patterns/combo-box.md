---
Description: Ein Texteingabefeld, das während der Benutzereingabe Vorschläge anzeigt.
title: Kombinationsfeld und Listenfeld
label: Combo box and list box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 31b3bcc2388a98941fc5e8aa44d18beee53de5c7
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081096"
---
# <a name="combo-box-and-list-box"></a>Kombinationsfeld und Listenfeld

Verwenden Sie ein Kombinationsfeld (auch als Dropdownliste bezeichnet), um eine Liste von Elementen anzuzeigen, in der ein Benutzer eine Auswahl treffen kann. Ein Kombinationsfeld wird zunächst im kompakten Zustand angezeigt und kann erweitert werden, sodass eine Liste der auswählbaren Elemente angezeigt wird. Ein ListBox ähnelt einem Kombinationsfeld, ist aber nicht reduzierbar/besitzt keinen kompakten Zustand. Weitere Informationen zu Listenfeldern findest du am Ende dieses Artikels.

Wenn das Kombinationsfeld geschlossen ist, wird entweder die aktuelle Auswahl angezeigt oder das Feld ist leer, wenn kein Element ausgewählt ist. Wenn der Benutzer das Kombinationsfeld erweitert, wird die Liste der auswählbaren Elemente angezeigt.

![Beispiel für eine Dropdownliste im kompakten Zustand](images/combo_box_collapsed.png)

> _Kombinationsfeld im kompakten Zustand mit einer Überschrift._

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Die Bibliothek „Windows UI“ enthält ab Version 2.2 eine neue Vorlage für dieses Steuerelement, die abgerundete Ecken verwendet. Weitere Informationen finden Sie unter [Eckradius](/windows/uwp/design/style/rounded-corner). „Windows UI“ ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für UWP-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Plattform-APIs:** [ComboBox-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [IsEditable-Eigenschaft](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [Text-Eigenschaft](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [TextSubmitted-Ereignis](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

- Mit einer Dropdownliste können Benutzer einen einzelnen Wert aus einer Reihe von Elementen auswählen, die mit einzelnen Textzeilen angemessen dargestellt werden können.
- Verwenden Sie eine Liste oder eine Rasteransicht anstelle eines Kombinationsfelds, um Elemente anzuzeigen, die mehrere Textzeilen oder Bilder enthalten.
- Wenn weniger als fünf Elemente vorhanden sind, können Sie stattdessen die Verwendung von [Optionsfeldern](radio-button.md) (wenn nur ein Element ausgewählt werden kann) oder [Kontrollkästchen](checkbox.md) (wenn mehrere Elemente ausgewählt werden können) in Betracht ziehen.
- Verwenden Sie ein Kombinationsfeld, wenn die Auswahlelemente für den Fluss der App weniger wichtig sind. Wenn für die Mehrzahl der Benutzer in der Mehrzahl der Situationen die Standardoption empfohlen wird, kann die Anzeige aller Elemente in einer Listenansicht mehr Aufmerksamkeit auf die Optionen ziehen als nötig. Sie können Platz sparen und Ablenkungen reduzieren, indem Sie ein Kombinationsfeld verwenden.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ComboBox">die App zu öffnen und ComboBox in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Ein Kombinationsfeld im kompakten Zustand kann eine Kopfzeile anzeigen.

![Beispiel für eine Dropdownliste im kompakten Zustand](images/combo_box_collapsed.png)

Obwohl Kombinationsfelder erweitert werden, um längere Zeichenfolgen zu unterstützen, sollten Sie extrem lange Zeichenfolgen vermeiden, die nur schwer lesbar sind.

![Beispiel einer Dropdownliste mit einer langen Textzeichenfolge](images/combo_box_listitemstate.png)

Wenn die Liste in einem Kombinationsfeld lang genug ist, wird eine Bildlaufleiste angezeigt. Gruppieren Sie die Elemente in der Liste logisch.

![Beispiel einer Bildlaufleiste in einer Dropdownliste](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>Erstellen eines Kombinationsfelds

Das Kombinationsfeld füllen Sie, indem Sie der Sammlung [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) direkt Objekte hinzufügen oder die [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)-Eigenschaft an eine Datenquelle binden. Elemente, die ComboBox hinzugefügt werden, sind in [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem)-Containern umschlossen.

Nachfolgend sehen Sie ein einfaches Kombinationsfeld mit hinzugefügten Elementen in XAML.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

Im folgenden Beispiel wird die Bindung eines Kombinationsfelds an eine Sammlung von FontFamily-Objekten veranschaulicht.

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>Elementauswahl

Wie ListView und GridView ist auch ComboBox von [Selector](/uwp/api/windows.ui.xaml.controls.primitives.selector) abgeleitet, sodass du die Auswahl eines Benutzers auf die gleiche Standardmethode abrufen kannst.

Sie können das ausgewählte Element des Kombinationsfelds mithilfe der [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)-Eigenschaft und den Index des ausgewählten Elements mithilfe der [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)-Eigenschaft abrufen oder festlegen.

Um den Wert einer bestimmten Eigenschaft des ausgewählten Datenelements abzurufen, können Sie die [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue)-Eigenschaft verwenden. Legen Sie in diesem Fall [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) fest, um anzugeben, für welche Eigenschaft des ausgewählten Elements der Wert abgerufen werden soll.

> [!TIP]
> Wenn Sie SelectedItem oder SelectedIndex festlegen, um die Standardauswahl anzugeben, wird eine Ausnahme ausgegeben, wenn die Eigenschaft festgelegt wird, bevor die Sammlung Items des Kombinationsfelds gefüllt wird. Sofern Sie die Elemente nicht in XAML definieren, empfiehlt es sich, das Ereignis „Loaded“ des Kombinationsfelds zu verarbeiten und SelectedItem oder SelectedIndex im Ereignishandler „Loaded“ festzulegen.

Sie können eine Bindung an diese Eigenschaften in XAML erstellen oder das Ereignis [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) verarbeiten, um auf Änderungen der Auswahl zu reagieren.

Im Ereignishandlercode können Sie das ausgewählte Element von der [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems)-Eigenschaft abrufen. Das zuvor ausgewählte Element (falls vorhanden) können Sie von der [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems)-Eigenschaft abrufen. Die AddedItems- und RemovedItems-Sammlung enthält jeweils nur ein Element, da in Kombinationsfeldern keine Mehrfachauswahl möglich ist.

Im folgenden Beispiel werden die Verarbeitung des Ereignisses SelectionChanged und die Bindung an das ausgewählte Element veranschaulicht.

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>SelectionChanged und Tastaturnavigation

Das Ereignis SelectionChanged tritt standardmäßig ein, wenn ein Benutzer auf ein Element in der Liste klickt oder tippt oder die EINGABETASTE drückt, um die Auswahl zu übernehmen, und das Kombinationsfeld geschlossen wird. Die Auswahl ändert sich nicht, wenn der Benutzer mit den Pfeiltasten der Tastatur in der geöffneten Liste des Kombinationsfelds navigiert.

Um ein Kombinationsfeld zu erstellen, das live aktualisiert wird, wenn der Benutzer mit den Pfeiltasten in der geöffneten Liste navigiert (z. B. in einer Dropdownliste für die Schriftartauswahl), müssen Sie [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) auf [Always](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger) festlegen. Dies bewirkt, dass das Ereignis SelectionChanged eintritt, wenn der Fokus auf ein anderes Element in der geöffneten Liste verschoben wird.

#### <a name="selected-item-behavior-change"></a>Änderung des Verhaltens von ausgewählten Elementen

In Windows 10 Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher wird das Verhalten von ausgewählten Elementen aktualisiert, sodass bearbeitbare Kombinationsfelder unterstützt werden.

Vor SDK 17763 musste der Wert der SelectedItem-Eigenschaft (und somit auch SelectedValue und SelectedIndex) in der Items-Sammlung des Kombinationsfelds enthalten sein. Die Einstellung `colorComboBox.SelectedItem = "Pink"` im vorherigen Beispiel ergibt Folgendes:

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

In SDK 17763 und höher muss der Wert der SelectedItem-Eigenschaft (und somit auch SelectedValue und SelectedIndex) nicht in der Items-Sammlung des Kombinationsfelds enthalten sein. Die Einstellung `colorComboBox.SelectedItem = "Pink"` im vorherigen Beispiel ergibt Folgendes:

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>Textsuche

Kombinationsfelder unterstützen automatisch die Suche in ihren Sammlungen. Wenn ein Benutzer über eine physische Tastatur Zeichen eingibt, während sich der Fokus auf einem geöffneten oder geschlossenen Kombinationsfeld befindet, werden Vorschläge angezeigt, die der vom Benutzer eingegebenen Zeichenfolge entsprechen. Diese Funktionalität ist besonders bei der Navigation durch eine lange Liste nützlich. Beispielsweise können Benutzer bei der Interaktion mit einer Dropdownliste, die verschiedene Bundesstaaten enthält, die Taste „w“ drücken, um „Washington“ anzuzeigen und schnell auswählen zu können. Bei der Textsuche wird nicht zwischen Groß- und Kleinschreibung unterschieden.

Sie können die [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled)-Eigenschaft auf **false** festlegen, um diese Funktion zu deaktivieren.

## <a name="make-a-combo-box-editable"></a>Bearbeitbarkeit eines Kombinationsfelds

> [!IMPORTANT]
> Für diese Funktion ist Windows 10 Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher erforderlich.

Standardmäßig kann der Benutzer in einem Kombinationsfeld eine Option in einer vordefinierten Liste von Optionen auswählen. In manchen Fällen enthält die Liste jedoch nur eine Teilmenge der gültigen Werte, und der Benutzer soll andere Werte eingeben können, die nicht aufgeführt sind. Dazu können Sie das Kombinationsfeld bearbeitbar machen.

Um ein Kombinationsfeld bearbeitbar zu machen, legen Sie die [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)-Eigenschaft auf **true** fest. Dann muss das Ereignis [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) so verarbeitet werden, dass es mit dem vom Benutzer eingegebenen Wert verwendet wird.

Standardmäßig wird der Wert SelectedItem aktualisiert, wenn benutzerdefinierter Text übernommen wird. Sie können dieses Verhalten überschreiben, indem Sie **Handled** in den TextSubmitted-Ereignisargumenten auf **true** festlegen. Wenn das Ereignis als „handled“ markiert ist, wird im Kombinationsfeld nach dem Ereignis keine weitere Aktion ausgeführt, und der Bearbeitungszustand des Kombinationsfelds wird beibehalten. SelectedItem wird nicht aktualisiert.

Im folgenden Beispiel ist ein einfaches bearbeitbares Kombinationsfeld dargestellt. Die Liste enthält einfache Zeichenfolgen, und alle vom Benutzer eingegebenen Werte werden wie eingegeben verwendet.

Über die Auswahl der zuletzt verwendeten Namen können benutzerdefinierte Zeichenfolgen eingegeben werden. Die Liste RecentlyUsedNames enthält einige Werte, die der Benutzer auswählen kann. Er kann jedoch auch einen neuen benutzerdefinierten Wert eingeben. Die CurrentName-Eigenschaft gibt den aktuell eingegebenen Namen an.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Übermittelter Text

Das Ereignis [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) kann so verarbeitet werden, dass es mit dem vom Benutzer eingegebenen Wert verwendet wird. Im Ereignishandler überprüfen Sie normalerweise, ob der vom Benutzer eingegebene Wert gültig ist, und verwenden dann den Wert in der App. Je nach Fall kannst du den Wert auch zur späteren Verwendung der Liste mit Optionen des Kombinationsfelds hinzufügen.

Das Ereignis TextSubmitted tritt ein, wenn folgende Bedingungen erfüllt sind:

- Die IsEditable-Eigenschaft ist auf **true** festgelegt.
- Der Benutzer gibt Text ein, der mit keinem vorhandenen Eintrag in der Liste des Kombinationsfelds übereinstimmt.
- Der Benutzer drückt die EINGABETASTE oder verschiebt den Fokus weg vom Kombinationsfeld.

Das Ereignis TextSubmitted tritt nicht ein, wenn der Benutzer Text eingibt und dann in der Liste nach oben oder nach unten navigiert.

### <a name="sample---validate-input-and-use-locally"></a>Beispiel: Überprüfen der Eingabe und lokale Verwendung

In diesem Beispiel enthält die Auswahl für den Schriftgrad eine Gruppe von Werten, die der Schriftgradabstufung entsprechen. Der Benutzer kann jedoch Schriftgrade eingeben, die nicht in der Liste enthalten sind.

Wenn der Benutzer einen Wert hinzufügt, der nicht in der Liste enthalten ist, wird der Schriftgrad aktualisiert, der Wert wird jedoch der Liste der Schriftgrade nicht hinzugefügt.

Wenn der neu eingegebene Wert ungültig ist, verwenden Sie SelectedValue, um die Text-Eigenschaft auf den letzten bekannten gültigen Wert zurückzusetzen.

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app's font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn't update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>Beispiel: Überprüfen der Eingabe und Hinzufügen zur Liste

In diesem Beispiel enthält eine Auswahl der bevorzugten Farben die am häufigsten verwendeten bevorzugten Farben (Rot, Blau, Grün und Orange). Der Benutzer kann jedoch eine bevorzugte Farbe hinzufügen, die nicht in der Liste enthalten ist. Wenn der Benutzer eine gültige Farbe hinzufügt (z. B. Rosa), wird die neu eingegebene Farbe der Liste hinzugefügt und als aktive bevorzugte Farbe festgelegt.

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn't update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Schränken Sie den Textinhalt von Kombinationsfeldelementen auf eine einzelne Zeile ein.
- Sortieren Sie die Elemente in einem Kombinationsfeld in der logischsten Reihenfolge. Gruppieren Sie verwandte Optionen, und platzieren Sie die am häufigsten verwendeten Optionen oben in der Liste. Sortieren Sie Namen in alphabetischer Reihenfolge, Nummern in numerischer Reihenfolge und Datumsangaben in chronologischer Reihenfolge.

## <a name="list-boxes"></a>Listenfelder

In einem Listenfeld kann der Benutzer ein einzelnes Element oder mehrere Elemente aus einer Auflistung auswählen. Listenfelder ähneln Dropdownlisten, abgesehen davon, dass Listenfelder immer geöffnet sind; für ein Listenfeld gibt es keinen kompakten Zustand (nicht erweitert). Elemente in einem Listenfeld können gescrollt werden, wenn der Platz nicht ausreicht, um alle Elemente anzuzeigen.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

- Ein Listenfeld kann nützlich sein, wenn Elemente in der Liste so relevant sind, dass sie auffälliger dargestellt werden sollten, und genügend Platz auf dem Bildschirm zum Anzeigen der vollständigen Liste vorhanden ist.
- Ein Listenfeld sollte den Benutzer bei einer wichtigen Entscheidung bzw. Auswahl auf alle Alternativen aufmerksam machen. Im Gegensatz dazu lenkt eine Dropdownliste die Aufmerksamkeit des Benutzers zunächst auf das ausgewählte Element.
- Vermeiden Sie die Verwendung eines Listenfelds in folgenden Fällen:
    - Es gibt eine sehr kleine Anzahl von Elementen für die Liste. Ein Einzelauswahl-Listenfeld, das immer zwei identische Optionen enthält, sollte besser in Form von [Optionsfeldern](radio-button.md) dargestellt werden. Ziehen Sie die Verwendung von Optionsfeldern ebenfalls in Erwägung, wenn die Liste drei oder vier statische Elemente enthält.
    - Das Listenfeld ist eine Einzelauswahl und enthält immer die gleichen zwei Optionen, wobei eine als „nicht die andere Option“ impliziert werden kann (beispielsweise „Ein“ und „Aus“). Verwenden Sie ein einzelnes Kontrollkästchen oder einen Umschalter.
    - Die Anzahl von Elementen ist sehr hoch. Bessere Optionen für lange Listen sind Rasteransicht und Listenansicht. Für sehr lange Listen mit gruppierten Daten wir der semantische Zoom bevorzugt.
    - Bei den Elementen handelt es sich um zusammenhängende numerische Werte. Wenn dies der Fall ist, sollten Sie einen [Schieberegler](slider.md) in Erwägung ziehen.
    - Die Auswahlelemente sind im Fluss Ihrer App von sekundärer Bedeutung, oder für die meisten Benutzer wird in den meisten Situationen die Standardoption empfohlen. Verwenden Sie stattdessen eine Dropdownliste.

### <a name="recommendations"></a>Empfehlungen

- Der ideale Bereich von Elementen in einem Listenfeld beträgt 3 bis 9.
- Ein Listenfeld funktioniert gut, wenn die Elemente darin dynamisch variieren können.
- Legen Sie die Größe eines Listenfelds möglichst so fest, dass für die Liste der zugehörigen Elemente keine Verschiebung und kein Scrollen erforderlich sind.
- Stellen Sie sicher, dass der Zweck des Listenfelds und die momentan ausgewählten Elemente klar sind.
- Reservieren Sie visuelle Effekte und Animationen für das Feedback für die Fingereingabe und für den ausgewählten Zustand von Elementen.
- Beschränken Sie den Text von Listenfeldelementen auf eine einzelne Zeile. Wenn es sich bei den Elementen um visuelle Objekte handelt, können Sie die Größe anpassen. Wenn ein Element mehrere Textzeilen oder Bilder enthält, verwenden Sie stattdessen eine Raster- oder Listenansicht.
- Verwenden Sie die standardmäßige Schriftart, sofern Sie gemäß Ihren Markenrichtlinien keine andere verwenden müssen.
- Verwenden Sie ein Listenfeld nicht zum Ausführen von Befehlen oder zum dynamischen Anzeigen oder Ausblenden anderer Steuerelemente.

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
