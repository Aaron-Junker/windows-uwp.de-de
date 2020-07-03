---
Description: Mit Optionsfeldern können Benutzer eine oder mehrere Optionen auswählen.
title: Richtlinien für Optionsfelder
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6705c314d9a70f8b6282841a7f8b1df76c6ef880
ms.sourcegitcommit: 6dd6d61c912daab2cc4defe5ba0cf717339f7765
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978402"
---
# <a name="radio-buttons"></a>Optionsfelder

Mithilfe von Optionsfeldern können Benutzer eine Option aus einer Sammlung von zwei oder mehr sich gegenseitig ausschließenden, aber verwandten Optionen auswählen. Jede Option wird durch ein Optionsfeld dargestellt.

Im Standardzustand ist kein Optionsfeld in einer Gruppe ausgewählt. Nachdem ein Benutzer jedoch eine Optionsfeldoption ausgewählt hat, kann er den Gruppenstatus „ohne Auswahl“ nicht wiederhergestellt werden.

Das singuläre Verhalten einer Optionsfeldgruppe unterscheidet sie von [Kontrollkästchen](checkbox.md), die Mehrfachauswahl und das Deaktivieren der Auswahl unterstützen.

![Optionsfelder](images/controls/radio-button.png)

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Das Steuerelement **RadioButtons** ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **APIs der Bibliothek „Windows UI“** [RadioButtons-Klasse](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [SelectionChanged-Ereignis](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged), [SelectedItem-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)
>
> **Plattform-APIs:** [RadioButton-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [Checked-Ereignis](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked), [IsChecked-Eigenschaft](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie Optionsfelder, um Benutzern die Möglichkeit zu bieten, eine Auswahl aus zwei oder mehr Optionen, die sich gegenseitig ausschließen, zu treffen.

![Eine Gruppe von Optionsfeldern](images/radiobutton_basic.png)

Verwenden Sie Optionsfelder, wenn Benutzer alle Optionen sehen müssen, um eine Auswahl treffen zu können. Da Optionsfelder alle Optionen gleichermaßen hervorheben, schenken Benutzer den Optionen möglicherweise mehr Aufmerksamkeit, als eigentlich erforderlich oder wünschenswert ist. Sie können auch andere Steuerelemente verwenden, es sei denn, alle Optionen erfordern die gleiche Aufmerksamkeit seitens des Benutzers. Verwenden Sie beispielsweise demgegenüber ein [Kombinationsfeld](combo-box.md), wenn die standardmäßige Option für die meisten Benutzer und in den meisten Situationen empfohlen ist.

![Eine Dropdownliste, mit der eine Standardoption hervorgehoben wird](images/combo_box_collapsed.png)

Wenn nur zwei sich gegenseitig ausschließende Optionen vorhanden sind, kombinieren Sie sie in einem einzelnen [Kontrollkästchen](checkbox.md) oder [Umschalter](toggles.md). Verwenden Sie beispielsweise ein Kontrollkästchen für "Ich stimme zu" anstelle von zwei Optionsfeldern für "Ich stimme zu" und "Ich stimme nicht zu".

![Ein Kontrollkästchen ist eine gute Alternative zum Darstellen einer binären Auswahl.](images/radiobutton_vs_checkbox.png)

Wenn der Benutzer mehrere Optionen auswählen kann, verwenden Sie ein [Kontrollkästchen](checkbox.md).

![Kontrollkästchen unterstützen die Mehrfachauswahl.](images/checkbox2.png)

Wenn Optionen Zahlen mit festgelegten Schritten (10, 20, 30) sind, verwenden Sie ein [Schieberegler](slider.md)-Steuerelement.

![Ein Schieberegler zum Auswählen von abgestuften Werten](images/controls/slider.png)

Wenn mehr als acht Optionen vorhanden sind, verwenden Sie ein [Kombinations- oder Listenfeld](combo-box.md).

![Ein Listenfeld, das zum Darstellen mehrerer Optionen verwendet wird](images/combo_box_scroll.png)

> [!NOTE]
> Wenn die verfügbaren Optionen auf dem aktuellen Kontext der App basieren oder anderweitig dynamisch variieren können, verwende ein [Listenfeld](combo-box.md#list-boxes) für die Einfachauswahl.

## <a name="radiobuttons-behavior"></a>RadioButtons-Verhalten

Tastaturzugriff und Navigationsverhalten für [RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041)-Gruppen wurden optimiert, um die Barrierefreiheit zu verbessern und Benutzern, die hauptsächlich die Tastatur verwenden, eine schnellere und einfachere Navigation in der Liste der Optionen zu erlauben.

Zusätzlich zu Verbesserungen von Tastenkombinationen und Barrierefreiheit wurde auch das visuelle Standardlayout einzelner Optionsfelder in einer RadioButton-Gruppe durch automatische Einstellungen für Ausrichtung, Abstand und Ränder optimiert. Dadurch entfällt die Notwendigkeit, diese Eigenschaften anzugeben, wie es bei Verwendung eines primitiven Gruppierungssteuerelements wie [StackPanel](../layout/layout-panels.md#stackpanel) oder [Grid](../layout/layout-panels.md#grid) erforderlich wäre.

### <a name="navigating-a-radiobuttons-group"></a>Navigieren in einer RadioButtons-Gruppe

Das RadioButtons-Steuerelement unterstützt zwei Zustände:

- Eine Liste von RadioButton-Steuerelementen, bei denen keine Option ausgewählt/aktiviert ist
- Eine Liste von RadioButton-Steuerelementen, bei denen bereits eine Option ausgewählt/aktiviert ist

In den folgenden beiden Abschnitten werden die Verhaltensweisen des Optionsfeldfokus behandelt.

#### <a name="item-already-selected"></a>Element wurde bereits ausgewählt

Wenn ein Optionsfeld ausgewählt wird und der Benutzer sich über die Registerkartennavigation in die Liste bewegt, erhält das ausgewählte Optionsfeld den Fokus.

|Liste ohne Registerkartenfokus | Liste mit anfänglichem Registerkartenfokus |
|:--:|:--:|
| ![Liste ohne Registerkartenfokus](images/radiobutton-selected-item-no-tab-focus.png) | ![Liste mit anfänglichem Registerkartenfokus](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="no-item-selected"></a>Kein Element ausgewählt

Wenn kein Optionsfeld ausgewählt ist, erhält das erste Optionsfeld in der Liste den Fokus.

> [!NOTE]
> Das Element, das den Registerkartenfokus von der anfänglichen Registerkartennavigation empfängt, wird nicht ausgewählt/aktiviert.

|Liste ohne Registerkartenfokus | Liste mit anfänglichem Registerkartenfokus|
|:--:|:--:|
| ![Liste ohne Registerkartenfokus](images/radiobutton-no-selected-item-no-tab-focus.png) | ![Liste mit anfänglichem Registerkartenfokus](images/radiobutton-no-selected-item-tab-focus.png)|

### <a name="keyboard-navigation"></a>Navigation mithilfe der Tastatur

Wenn Sie über eine einzelne Zeile/Spalte mit Optionsfeldoptionen verfügen und ein Element bereits den Registerkartenfokus erhalten hat, stellen die Pfeiltasten eine „innere Navigation“ zwischen den Elementen im RadioButtons-Steuerelement bereit. Weitere Informationen zum Verhalten bei der Tastaturnavigation finden Sie unter [Tastaturinteraktionen – Navigation](../input/keyboard-interactions.md#navigation).

Wenn die Liste der Optionen in einem RadioButton-Steuerelement (ausschließlich) vertikal angeordnet ist, wird mit den NACH-OBEN/UNTEN-TASTEN zwischen den Elementen navigiert. Die NACH-LINKS/RECHTS-TASTEN haben keine Funktion. Wenn die Liste der Optionen in einem RadioButton-Steuerelement (ausschließlich) horizontal angeordnet ist, wird mit den NACH-OBEN/UNTEN-TASTEN und auch den NACH-LINKS/RECHTS-TASTEN gleichermaßen zwischen den Elementen navigiert.

![Beispiel für die Tastaturnavigation in einer RadioButton-Gruppe mit einer einzigen Spalte/Zeile](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*Beispiel für die Tastaturnavigation in einer RadioButton-Gruppe mit einer einzigen Spalte/Zeile*

#### <a name="navigating-within-multi-columnrow-layouts"></a>Navigieren in Layouts mit mehreren Spalten/Zeilen

Wenn bei spaltenweise absteigender Reihenfolge (Eingabe der Elemente von oben nach unten und von links nach rechts) der Fokus auf dem letzten Element in einer Spalte liegt und die NACH-UNTEN-TASTE gedrückt wird, wird der Fokus zum ersten Element in der nächsten Spalte verlagert. Dieses Verhalten tritt auch umgekehrt auf: Wenn der Fokus auf dem ersten Element in einer Spalte liegt und die NACH-OBEN-TASTE gedrückt wird, wird der Fokus auf das letzte Element in der vorherigen Spalte verschoben.

![Beispiel für die Tastaturnavigation in einer RadioButton-Gruppe mit mehreren Spalten/Zeilen](images/radiobutton-keyboard-navigation-multi-column-row.png)

Wenn bei zeilenweise absteigender Reihenfolge (Eingabe der Elemente von links nach rechts und von oben nach unten) der Fokus auf dem letzten Element in einer Zeile liegt und die NACH-RECHTS-TASTE gedrückt wird, wird der Fokus zum ersten Element in der nächsten Zeile verlagert. Dieses Verhalten tritt auch umgekehrt auf: Wenn der Fokus auf dem ersten Element in einer Zeile liegt und die NACH-LINKS-TASTE gedrückt wird, wird der Fokus auf das letzte Element in der vorherigen Zeile verlagert.

##### <a name="wrapping"></a>Umbruch

Die RadioButtons-Gruppe wird nicht umbrochen. Dies liegt daran, dass bei Verwendung einer Sprachausgabe das Gefühl für Grenzen und eine klare Festlegung von Anfang und Ende verloren geht, wodurch es für Benutzer mit Sehbehinderung schwierig wird, in der Liste zu navigieren. Das RadioButtons-Steuerelement unterstützt auch keine Enumeration, da es eine angemessene Anzahl von Elementen enthalten soll (siehe [Ist dies das richtige Steuerelement?](#is-this-the-right-control)).

## <a name="selection-follows-focus"></a>Auswahl folgt Fokus

Wenn Sie die Tastatur verwenden, um zwischen Elementen in einer RadioButtons-Liste zu navigieren (in der bereits ein Element ausgewählt ist), wird beim Verschieben des Fokus von einem zum anderen Element jeweils das Element, das den Fokus erhält, ausgewählt/aktiviert, und das Element, das zuvor den Fokus besaß, wird abgewählt/deaktiviert.

|Vor der Navigation mithilfe der Tastatur | Nach der Navigation mithilfe der Tastatur|
|:--|:--|
| ![Beispiel für Fokus und Auswahl vor der Tastaturnavigation](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*Beispiel für Fokus und Auswahl vor der Tastaturnavigation* | ![Beispiel für Fokus und Auswahl nach der Tastaturnavigation](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*Beispiel für Fokus und Auswahl nach der Tastaturnavigation, wobei die NACH-UNTEN- oder die NACH-RECHTS-TASTE den Fokus auf das Optionsfeld „3“ verschoben, „3“ aktiviert und „2“ deaktiviert wird.*

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navigation mit Xbox-Gamepad und Xbox-Fernbedienung

Wenn Sie ein Xbox-Gamepad oder eine Xbox-Fernbedienung verwenden, um innerhalb eines RadioButtons-Steuerelements zu navigieren, ist das Verhalten „Auswahl folgt Fokus“ deaktiviert und die „A“-Taste muss gedrückt werden, um das Optionsfeld mit dem Fokus auszuwählen.

## <a name="accessibility-behavior"></a>Barrierefreiheitsverhalten

Die folgende Tabelle zeigt im Detail, wie die Sprachausgabe mit einer Optionsfeldgruppe umgeht und was angesagt wird (dies hängt von den vom Benutzer eingestellten Einstellungen für die Sprachausgabe ab).

| Anfänglicher Fokus | Fokus wechselt zu einem ausgewählten Element |
|:--|:--|
| „Gruppenname“ RadioButton-Sammlung, x von N ausgewählt | RadioButton „Name“ ausgewählt, x von N |
|„Gruppenname“ RadioButton-Sammlung, kein Element ausgewählt| RadioButton „Name“ nicht ausgewählt, x von N <br> *(Bei der Navigation mit UMSCHALT+PFEILTASTEN, was angibt dass die Auswahl nicht dem Fokus folgt)* |

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/RadioButton">die App zu öffnen und RadioButton in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="using-the-winui-radiobuttons-control"></a>Verwenden des WinUI-RadioButtons-Steuerelements

Wenn Sie [WinUI](https://github.com/microsoft/microsoft-ui-xaml) verwenden, empfiehlt es sich, das [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)-Steuerelement zu verwenden.

Das RadioButtons-Steuerelement ist einfach einzurichten und zu verwenden und gewährleistet das richtige und erwartete Verhalten von Tastatur und Sprachausgabe.

Hier wird ein einfaches RadioButtons-Steuerelement mit drei Optionen deklariert.

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```

![Optionsfelder in zwei Gruppen](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>Definieren mehrerer Spalten

Sie können ein mehrspaltiges RadioButton-Steuerelement deklarieren, indem Sie die [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns)-Eigenschaft angeben.

```xaml
<muxc:RadioButtons Header="App Mode" MaxColumns="3">
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
</muxc:RadioButtons>
```

![Optionsfelder in zweispaltigen Gruppen](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>Datenbindung

Das RadioButtons-Steuerelement unterstützt die Datenbindung mithilfe der [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource)-Eigenschaft, wie im folgenden Codeausschnitt gezeigt.

```xaml
<RadioButtons Header="App Mode" ItemsSource="{x:Bind radioButtonItems}" />
```

```c#
public sealed partial class MainPage : Page
{
    public class OptionDataModel
    {
        public string Label;
        public override string ToString()
        {
            return Label;
        }
    }

    List<OptionDataModel> radioButtonItems;

    public MainPage()
    {
        this.InitializeComponent();

        radioButtonItems = new List<OptionDataModel>();
        radioButtonItems.Add(new OptionDataModel() { label = "Item 1" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 2" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 3" });
    }
}
```

## <a name="create-your-own-radio-button-group"></a>Erstellen einer eigenen Optionsfeldgruppe

> [!Important]
> Es empfiehlt sich, das WinUI-RadioButton-Steuerelement zu verwenden, um RadioButton-Elemente zu gruppieren (es sei denn, Sie verwenden eine ältere Version von WinUI).

Optionsfelder funktionieren in Gruppen. Es gibt zwei Arten zur Gruppierung von Optionsfeld-Steuerelementen:

- Platzieren Sie sie im gleichen übergeordneten Container.
- Legen Sie die [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName)-Eigenschaft für jedes Optionsfeld auf denselben Wert fest.

In diesem Beispiel wird die erste Gruppe von Optionsfeldern implizit gruppiert, da die Felder im selben StackPanel-Element enthalten sind. Die zweite Gruppe ist auf zwei StackPanel-Elemente aufgeteilt, damit sie explizit nach GroupName gruppiert werden.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Die folgende Abbildung zeigt, wie diese Optionsfeldgruppe gerendert wird:

![Optionsfelder in zwei Gruppen](images/radio-button-groups.png)

## <a name="radio-button-states"></a>Optionsfeldzustände

Ein Optionsfeld hat zwei Zustände: aktiviert und deaktiviert. Wenn ein Optionsfeld aktiviert ist, ist die [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)-Eigenschaft auf **true** festgelegt. Wenn ein Optionsfeld deaktiviert ist, lautet die **IsChecked**-Eigenschaft **false**. Ein Optionsfeld kann durch Klicken auf ein anderes Optionsfeld in derselben Gruppe deaktiviert werden, jedoch nicht durch erneutes Klicken auf das Optionsfeld selbst. Sie können ein Optionsfeld jedoch programmgesteuert durch Festlegen der IsChecked-Eigenschaft auf **false** deaktivieren.

## <a name="recommendations"></a>Empfehlungen

- Stellen Sie sicher, dass der Zweck und der aktuelle Status einer Gruppe von Optionsfeldern nachvollziehbar ist.
- Begrenze den Text des Optionsfelds auf eine einzelne Zeile.
- Wenn der Textinhalt dynamisch ist, bedenken Sie die Größenänderung der Schaltfläche und die visuellen Effekte herum.
- Verwenden Sie die standardmäßige Schriftart, es sei denn, Sie müssen gemäß Ihren Markenrichtlinien eine andere verwenden.
- Platzieren Sie keine zwei Optionsfeldgruppen nebeneinander. Wenn sich zwei Optionsfeldgruppen direkt nebeneinander befinden, ist es schwierig, festzustellen, welche Schaltflächen zu welcher Gruppe gehören.

### <a name="visuals-to-consider"></a>Zu erwägende visuelle Elemente

Die folgenden Bilder veranschaulichen, wie Sie die Optionsfelder in einer RadioButton-Gruppe am besten anordnen.

![Gruppe von Optionsfeldern](images/radiobutton-layout.png)

![Abstandsrichtlinien für Optionsfelder](images/radiobutton-redline.png)

> [!NOTE]
> Wenn Sie ein WinUI-RadioButton-Steuerelement verwenden, sind Abstand, Ränder und Ausrichtung bereits optimiert.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Zugehörige Themen

### <a name="for-designers"></a>Für Designer

- [Schaltflächen](buttons.md)
- [Umschalter](toggles.md)
- [Kontrollkästchen](checkbox.md)
- [Listen und Kombinationsfelder](lists.md)
- [Schieberegler](slider.md)

### <a name="for-developers-xaml"></a>Für Entwickler (XAML)

- [RadioButton-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
