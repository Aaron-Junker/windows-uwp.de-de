---
Description: Ein Texteingabefeld, das während der Benutzereingabe Vorschläge anzeigt.
title: Kombinationsfeld (Dropdownliste)
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 21a6c698fa0e07587e2c25ae827dc6654a8ced9d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618625"
---
# <a name="combo-box"></a>Kombinationsfeld

Verwenden Sie ein Kombinationsfeld (auch bekannt als eine Dropdown-Liste), um eine Liste von Elementen vor, die ein Benutzer auswählen kann. Ein Kombinationsfeld in einem compact Zustand beginnt, und es wird erweitert, um eine Liste der auswählbaren Elemente anzuzeigen.

Wenn das Kombinationsfeld geschlossen wird, sie zeigt die aktuelle Auswahl oder ist leer, wenn es kein Element ausgewählt ist. Wenn der Benutzer im Kombinationsfeld erweitert, wird die Liste der auswählbaren Elemente angezeigt.

> **Wichtige APIs**: [ComboBox-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [IsEditable-Eigenschaft](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [Texteigenschaft](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [TextSubmitted-Ereignis](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

Ein Kombinationsfeld im compact Zustand mit einem Header.

![Beispiel für eine Dropdownliste im kompakten Zustand](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

- Mit einer Dropdownliste können Benutzer einen einzelnen Wert aus einer Reihe von Elementen auswählen, die mit einzelnen Textzeilen angemessen dargestellt werden können.
- Verwenden Sie eine Liste oder eine Rasteransicht anstelle eines Kombinationsfelds, um Elemente anzuzeigen, die mehrere Textzeilen oder Bilder enthalten.
- Wenn weniger als fünf Elemente vorhanden sind, können Sie stattdessen die Verwendung von [Optionsfeldern](radio-button.md) (wenn nur ein Element ausgewählt werden kann) oder [Kontrollkästchen](checkbox.md) (wenn mehrere Elemente ausgewählt werden können) in Betracht ziehen.
- Verwenden Sie ein Kombinationsfeld, wenn die Auswahlelemente für den Fluss der App weniger wichtig sind. Wenn für die Mehrzahl der Benutzer in der Mehrzahl der Situationen die Standardoption empfohlen wird, kann die Anzeige aller Elemente in einer Listenansicht mehr Aufmerksamkeit auf die Optionen ziehen als nötig. Sie können Platz sparen und Ablenkungen reduzieren, indem Sie ein Kombinationsfeld verwenden.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ComboBox">öffnen Sie die app, und sehen Sie das Kombinationsfeld in Aktion</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Abrufen der XAML-Steuerelemente Katalog-app (Microsoft Store)</a></li>
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

## <a name="create-a-combo-box"></a>Erstellen Sie ein Kombinationsfeld

Sie im Kombinationsfeld auffüllen, indem Objekte direkt hinzufügen der [Elemente](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) Sammlung oder durch die Bindung der [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) Eigenschaft mit einer Datenquelle. Elemente hinzugefügt werden, an das Kombinationsfeld umschlossen sind [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) Container.

Hier ist ein einfaches Kombinationsfeld mit Elementen, die in XAML hinzugefügt.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

Das folgende Beispiel zeigt ein Kombinationsfeld an eine Auflistung von FontFamily-Objekten binden.

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

Z. B. ListView und GridView, stammt aus "ComboBox" [Selektor](/uwp/api/windows.ui.xaml.controls.primitives.selector), damit Sie die Auswahl des Benutzers auf die gleiche standard Weise nutzen können.

Können Sie abrufen oder des Kombinationsfelds ausgewählte Element festlegen, indem die [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) -Eigenschaft "und" Get "oder" Set-der Index des ausgewählten Elements mithilfe der [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) Eigenschaft.

Um den Wert einer bestimmten Eigenschaft für das ausgewählte Datenelement zu erhalten, können Sie die [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) Eigenschaft. In diesem Fall legen Sie die [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) an welche Eigenschaft für das ausgewählte Element, dessen Wert abgerufen.

> [!TIP]
> Wenn Sie SelectedItem "oder" SelectedIndex ", um anzugeben, die Standardauswahl festlegen, tritt eine Ausnahme auf, wenn die Eigenschaft festgelegt ist, bevor die Elementauflistung des Kombinationsfeld-Feld gefüllt wird. Wenn Sie die Elemente in XAML definieren, empfiehlt es sich um das Kombinationsfeld Feld Loaded-Ereignis zu behandeln und SelectedItem oder SelectedIndex in das Loaded-Ereignis-Handler festlegen.

Sie können auf diese Eigenschaften in XAML zu binden oder Verarbeiten der [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) Event auf Änderungen der Auswahl reagiert.

In den Ereignisdaten Handlercode, erhalten Sie das ausgewählte Element aus der [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) Eigenschaft. Sie können die zuvor ausgewählten Elements (sofern vorhanden) abrufen, aus der [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) Eigenschaft. Über die Sammlungen AddedItems und RemovedItems enthalten nur 1 Element aus, da im Kombinationsfeld Mehrfachauswahl nicht unterstützt.

In diesem Beispiel wird veranschaulicht, wie das SelectionChanged-Ereignis behandeln, und Binden an das ausgewählte Element.

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

Standardmäßig tritt auf, das SelectionChanged-Ereignis, wenn ein Benutzer klickt, tippt oder drückt, geben Sie auf ein Element in der Liste aus, um ihre Auswahl zu übernehmen, und das Kombinationsfeld wird geschlossen. Auswahl ändern nicht, wenn der Benutzer die öffnen Kombinationsfeldliste mit den Pfeiltasten der Tastatur navigiert.

Ein Kombinationsfeld, "live-Updates" im Feld während der Benutzer die Öffnen der Liste mit den Pfeiltasten (z. B. eine Schriftart Auswahl Dropdown-Liste) navigiert legen fest, um [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) zu [immer](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger). Dies bewirkt, dass das SelectionChanged-Ereignis auftreten, wenn es sich bei fokusänderungen in ein anderes Element in der Liste öffnen.

#### <a name="selected-item-behavior-change"></a>Ausgewähltes Element verhaltensänderung

In Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, wird das Verhalten der ausgewählten Elemente aktualisiert, um bearbeitbare Kombinationsfelder zu unterstützen.

Vor dem SDK 17763, den Wert der SelectedItem-Eigenschaft (und somit SelectedValue und SelectedIndex) war erforderlich, um in das Kombinationsfeld Items-Auflistung sein. Im vorherigen Beispiel, Festlegen von `colorComboBox.SelectedItem = "Pink"` führt:

- SelectedItem = Null
- SelectedValue = Null
- SelectedIndex = -1

Im SDK 17763 und höhere Versionen den Wert der SelectedItem-Eigenschaft (und somit SelectedValue und SelectedIndex) muss nicht in die Auflistung der Elemente des Kombinationsfelds sein. Im vorherigen Beispiel, Festlegen von `colorComboBox.SelectedItem = "Pink"` führt:

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>Textsuche

Kombinationsfelder unterstützen automatisch die Suche in ihren Sammlungen. Wenn ein Benutzer über eine physische Tastatur Zeichen eingibt, während sich der Fokus auf einem geöffneten oder geschlossenen Kombinationsfeld befindet, werden Vorschläge angezeigt, die der vom Benutzer eingegebenen Zeichenfolge entsprechen. Diese Funktionalität ist besonders bei der Navigation durch eine lange Liste nützlich. Beispielsweise können bei der Interaktion mit einer Dropdown-Liste mit einer Liste von Staaten, Benutzer der "w" "Washington" in der Ansicht für die schnelle Auswahl erscheinen Taste. Die Textsuche wird nicht beachtet.

Sie können festlegen, die [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) Eigenschaft **"false"** wird diese Funktion deaktiviert.

## <a name="make-a-combo-box-editable"></a>Stellen Sie das Kombinationsfeld bearbeitet werden

> [!IMPORTANT]
> Dieses Feature erfordert Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher.

Wird standardmäßig ein Kombinationsfeld ermöglicht dem Benutzer aus einer vordefinierten Liste von Optionen auswählen. Es gibt jedoch Fälle, in dem die Liste enthält nur eine Teilmenge der gültigen Werte und der Benutzer sollte in der Lage, andere Werte eingeben, die nicht aufgelistet sind. Um dies zu unterstützen, können Sie im Kombinationsfeld bearbeitbar zu machen.

Damit ein Kombinationsfeld bearbeitet werden kann, legen Sie die [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) Eigenschaft **"true"**. Behandeln Sie dann die [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) Ereignis, um die Arbeit mit der vom Benutzer eingegebene Wert.

Standardmäßig wird der SelectedItem-Wert aktualisiert, wenn der Benutzer die benutzerdefinierten Text ein Commit ausgeführt. Sie können dieses Verhalten überschreiben, indem Sie festlegen **behandelt** zu **"true"** in den Ereignis-Args TextSubmitted. Wenn das Ereignis als behandelt markiert ist, wird das Kombinationsfeld werden keine weiteren Maßnahmen nach dem Ereignis und verbleibt im Zustand "Bearbeiten". SelectedItem wird nicht aktualisiert werden.

Dieses Beispiel zeigt ein einfaches bearbeitbare Kombinationsfeld. Die Liste enthält einfache Zeichenfolgen, und alle vom Benutzer eingegebene Wert wird verwendet, wie Sie eingegeben wurden.

Eine Auswahl "zuletzt verwendet Namen" kann die Benutzer, die benutzerdefinierte Zeichenfolgen eingeben. Die Liste "RecentlyUsedNames" enthält einige Werte, aus denen der Benutzer auswählen kann, aber Benutzer können auch einen neuen, angepasste Wert hinzufügen. Die Eigenschaft "CurrentName" stellt den gerade eingegebenen Namen.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Text übermittelt

Sie können behandeln die [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) Ereignis, um die Arbeit mit der vom Benutzer eingegebene Wert. Im Ereignisprotokoll Handler auf, Sie in der Regel überprüft, dass der vom Benutzer eingegebene Wert ungültig ist, verwenden Sie den Wert in Ihrer app an. Je nach Situation können Sie auch den Wert des Kombinationsfelds Liste mit den Optionen für die zukünftige Verwendung hinzufügen.

Die TextSubmitted-Ereignis tritt auf, wenn diese Bedingungen erfüllt sind:

- Die IsEditable-Eigenschaft ist **"true"**
- Der Benutzer gibt Text an, die einen vorhandenen Eintrag in der Liste des Kombinationsfelds nicht übereinstimmen
- Der Benutzer drückt die EINGABETASTE, oder aus dem Kombinationsfeld den Fokus.

Das Ereignis TextSubmitted tritt nicht auf, wenn der Benutzer Text eingibt und anschließend in der Liste nach oben oder unten wechselt.

### <a name="sample---validate-input-and-use-locally"></a>Beispiel: -Eingabe überprüfen und verwenden Sie lokal

In diesem Beispiel eine Größe Schriftartauswahl enthält einen Satz von Werten, die die Schriftart Größe Ramp entsprechen, aber der Benutzer möglicherweise Schriftgrade, die nicht in der Liste eingeben.

Wenn der Benutzer einen Wert hinzufügt, die nicht in der Liste, die Schriftart Größe Updates, aber der Wert wird nicht zur Liste der Schriftgrade Ihren Bedürfnissen entsprechend hinzugefügt.

Wenn der neu eingegebene Wert ungültig ist, die SelectedValue können Sie die Text-Eigenschaft mit dem letzten Zurücksetzen bezeichnet guten Wert.

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
        // Update the app’s font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>Beispiel: Überprüfen von Eingaben und zur Liste hinzufügen

Hier eine "Lieblingsfarbe-Auswahl" enthält, die am häufigsten verwendeten bevorzugten Farben (Rot, Blau, Grün, Orange), aber der Benutzer kann eine bevorzugte Farbe, die nicht in der Liste eingeben. Wenn der Benutzer eine gültige Farbe (z. B. Folgendes ein: rosa) hinzufügt, wird die neu eingegebene Farbe zur Liste hinzugefügt und als aktive "bevorzugten Color" festgelegt.

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
        // Mark the event as handled so the framework doesn’t update the selected item.
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

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [AutoSuggestBox-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Verwandte Artikel

- [Textsteuerelemente](text-controls.md)
- [Rechtschreibprüfung](text-controls.md)
- [Suche](search.md)
- [TextBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length-Eigenschaft](https://msdn.microsoft.com/library/system.string.length.aspx)
