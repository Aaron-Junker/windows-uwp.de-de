---
description: Erfahren Sie, wie Sie Optionsfelder verwenden, um Benutzern zu ermöglichen, eine Option aus einer Sammlung von zwei oder mehr sich gegenseitig ausschließenden, aber verwandten Optionen auszuwählen.
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
ms.openlocfilehash: 7d09eaefff193a8283fd4bad68528b8976e0b63b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169784"
---
# <a name="radio-buttons"></a>Optionsfelder

Mithilfe von Optionsfeldern können Benutzer eine Option aus einer Sammlung von zwei oder mehr sich gegenseitig ausschließenden, aber verwandten Optionen auswählen. Jede Option wird durch ein Optionsfeld dargestellt.

Im Standardzustand ist in einer RadioButtons-Gruppe kein Optionsfeld ausgewählt. Das heißt, alle Optionsfelder sind deaktiviert. Wenn jedoch ein Optionsfeld ausgewählt wurde, kann der deaktivierte Zustand der Gruppe nicht wiederhergestellt werden.

Das singuläre Verhalten einer Optionsfeldgruppe (RadioButtons) unterscheidet sie von [Kontrollkästchen](checkbox.md), die Mehrfachauswahl und das Aufheben der Auswahl (Deaktivieren) unterstützen.

![Beispiel für eine RadioButtons-Gruppe mit einem aktivierten Optionsfeld.](images/controls/radio-button.png)

## <a name="get-the-windows-ui-library"></a>Abrufen der Windows-UI-Bibliothek

| &nbsp; | &nbsp; |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Das Steuerelement RadioButtons ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows-UI-Bibliothek](/uwp/toolkits/winui/). |

**Windows-UI-Bibliotheks-APIs:** 
* [RadioButtons-Klasse](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
* [SelectionChanged-Ereignis](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
* [SelectedItem-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)
* [SelectedIndex-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)

**Plattform-APIs:** 
* [RadioButton-Klasse](/uwp/api/Windows.UI.Xaml.Controls.RadioButton)
* [Checked-Ereignis](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)
* [IsChecked-Eigenschaft](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie Optionsfelder, um Benutzern zu gestatten, eine Auswahl aus zwei oder mehr Optionen, die sich gegenseitig ausschließen, zu treffen.

![Eine RadioButtons-Gruppe mit einem aktivierten Optionsfeld.](images/radiobutton_basic.png)

Verwenden Sie Optionsfelder, wenn Benutzer alle Optionen sehen müssen, bevor sie eine Auswahl treffen. Optionsfelder heben alle Optionen gleichermaßen hervor, was bedeutet, dass manche Optionen möglicherweise mehr Aufmerksamkeit auf sich ziehen, als eigentlich erforderlich oder wünschenswert ist. 

Wenn nicht alle Optionen dieselbe Aufmerksamkeit benötigen, erwägen Sie die Verwendung anderer Steuerelemente. Um beispielsweise eine einzelne beste Option für die meisten Benutzer und in den meisten Situationen zu empfehlen, verwenden Sie ein [Kombinationsfeld](combo-box.md), um diese beste Option als Standardoption anzuzeigen.

![Ein Kombinationsfeld mit angezeigter Standardoption.](images/combo_box_collapsed.png)

Wenn es nur zwei Optionen gibt, die sich zudem gegenseitig ausschließen, kombinieren Sie sie in einem einzigen [Kontrollkästchen](checkbox.md) oder [Umschalter](toggles.md)-Steuerelement. Verwenden Sie beispielsweise ein einzelnes Kontrollkästchen für „Ich stimme zu“ anstelle von zwei Optionsfeldern für „Ich stimme zu“ und „Ich stimme nicht zu“.

![Ein Kontrollkästchen ist eine gute Alternative zum Anbieten einer binären Auswahl.](images/radiobutton_vs_checkbox.png)

Wenn Benutzer mehrere Optionen auswählen können, verwenden Sie [Kontrollkästchen](checkbox.md).

![Kontrollkästchen unterstützen Mehrfachauswahl](images/checkbox2.png)

Wenn die Optionen des Benutzers in einem Wertebereich liegen (z. B. *10, 20, 30, ... 100*), verwenden Sie ein [Schieberegler](slider.md)-Steuerelement.

![Ein Schieberegler-Steuerelement, das einen Wert aus einem Wertebereich anzeigt.](images/controls/slider.png)

Wenn acht oder mehr Optionen vorhanden sind, verwenden Sie ein [Kombinationsfeld](combo-box.md).

![Ein Listenfeld, das mehrere Optionen anzeigt.](images/combo_box_scroll.png)

> [!NOTE]
> Wenn die verfügbaren Optionen auf dem aktuellen Kontext einer App basieren oder anderweitig dynamisch variieren können, verwenden Sie ein Listensteuerelement.

## <a name="radiobuttons-behavior"></a>RadioButtons-Verhalten

Das Tastaturzugriffs- und -navigationsverhalten wurden in der [RadioButton-Klasse](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041) optimiert. Diese Verbesserungen helfen sowohl Barrierefreiheits- als auch Tastaturbenutzern dabei, sich schneller und einfacher in der Liste der Optionen zu bewegen.

Zusätzlich zu diesen Verbesserungen wurde auch das visuelle Standardlayout einzelner Optionsfelder in einer RadioButtons-Gruppe durch automatische Einstellungen für Ausrichtung, Abstand und Ränder optimiert. Durch diese Optimierung entfällt die Notwendigkeit, diese Eigenschaften anzugeben, wie es bei Verwendung eines primitiven Gruppierungssteuerelements wie [StackPanel](../layout/layout-panels.md#stackpanel) oder [Grid](../layout/layout-panels.md#grid) erforderlich wäre.

### <a name="navigating-a-radiobuttons-group"></a>Navigieren in einer RadioButtons-Gruppe

Das RadioButtons-Steuerelement unterstützt zwei Zustände:

- Kein Optionsfeld ist ausgewählt.
- Ein Optionsfeld ist ausgewählt.

In den folgenden beiden Abschnitten werden die Verhaltensweisen des Optionsfeldfokus behandelt.

#### <a name="no-radio-button-is-selected"></a>Kein Optionsfeld ist ausgewählt.

Wenn kein Optionsfeld ausgewählt ist, erhält das erste Optionsfeld in der Liste den Fokus.

> [!NOTE]
> Das Element, das den TAB-TASTEN-Fokus durch die erste Navigation per TAB-TASTE empfängt, wird nicht ausgewählt.

|Liste ohne Registerkartenfokus | Liste mit anfänglichem Registerkartenfokus|
|:--:|:--:|
| ![Liste ohne Registerkartenfokus](images/radiobutton-no-selected-item-no-tab-focus.png) | ![Liste mit anfänglichem Registerkartenfokus](images/radiobutton-no-selected-item-tab-focus.png)|

#### <a name="one-radio-button-is-selected"></a>Ein Optionsfeld ist ausgewählt.

Wenn ein Optionsfeld ausgewählt ist, und der Benutzer wechselt mittels TAB-TASTE in die Liste, erhält das ausgewählte Optionsfeld den Fokus.

|Liste ohne Registerkartenfokus | Liste mit anfänglichem Registerkartenfokus |
|:--:|:--:|
| ![Liste ohne Registerkartenfokus](images/radiobutton-selected-item-no-tab-focus.png) | ![Liste mit anfänglichem Registerkartenfokus](images/radiobutton-selected-item-tab-focus.png)|


### <a name="keyboard-navigation"></a>Navigation mithilfe der Tastatur

Wenn Benutzer über eine einzelne Zeile oder Spalte mit Optionsfeldoptionen verfügen und ein Element bereits den TAB-TASTEN-Fokus erhalten hat, können sie die Pfeiltasten für die „innere Navigation“ zwischen den Elementen innerhalb des RadioButtons-Steuerelements verwenden. Weitere Informationen zum Tastaturnavigationsverhalten finden Sie unter [Tastaturinteraktionen – Navigation](../input/keyboard-interactions.md#navigation).

Wenn die Liste der Optionen in einem RadioButtons-Steuerelement nur vertikal angeordnet ist, wird mit den NACH-OBEN-/NACH-UNTEN-TASTEN zwischen den Elementen navigiert, während die NACH-LINKS-/NACH-RECHTS-TASTEN keine Funktion haben. Wenn die Liste der Optionen in einem RadioButtons-Steuerelement jedoch nur horizontal angeordnet ist, wird mit den NACH-OBEN-/NACH-UNTEN-TASTEN wie auch mit den NACH-LINKS-/NACH-RECHTS-TASTEN gleichermaßen zwischen den Elementen navigiert.

![Beispiel für die Tastaturnavigation in einer RadioButtons-Gruppe mit einer einzigen Spalte oder Zeile.](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*Beispiel für die Tastaturnavigation in einer RadioButtons-Gruppe mit einer einzigen Spalte oder Zeile.*

#### <a name="navigating-within-multi-column-or-multi-row-layouts"></a>Navigieren in Layouts mit mehreren Spalten oder Zeilen

Bei der spaltenweisen Reihenfolge wechselt der Fokus von oben nach unten und von links nach rechts. Wenn der Fokus auf dem letzten Element in einer Spalte liegt und die NACH-UNTEN-TASTE gedrückt wird, wird der Fokus auf das erste Element in der nächsten Spalte verschoben. Dieses selbe Verhalten tritt auch umgekehrt auf: Wenn der Fokus auf dem ersten Element in einer Spalte liegt und die NACH-OBEN-TASTE gedrückt wird, wird der Fokus auf das letzte Element in der vorherigen Spalte verlagert.

![Beispiel für die Tastaturnavigation in einer RadioButtons-Gruppe mit mehreren Spalten oder Zeilen.](images/radiobutton-keyboard-navigation-multi-column-row.png)

Wenn bei zeilenweise absteigender Reihenfolge (Eingabe der Elemente von links nach rechts und von oben nach unten) der Fokus auf dem letzten Element in einer Zeile liegt und die NACH-RECHTS-TASTE gedrückt wird, wird der Fokus zum ersten Element in der nächsten Zeile verlagert. Dieses selbe Verhalten tritt auch umgekehrt auf: Wenn der Fokus auf dem ersten Element in einer Zeile liegt und die NACH-LINKS-TASTE gedrückt wird, wird der Fokus auf das letzte Element in der vorherigen Zeile verlagert.

Weitere Informationen finden Sie unter [Tastaturinteraktionen](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items).

##### <a name="wrapping"></a>Umbruch

Die RadioButtons-Gruppe wird nicht umbrochen. Dies liegt daran, dass bei Verwendung einer Sprachausgabe das Gefühl für Grenzen und eine klare Festlegung von Anfang und Ende verloren geht, wodurch es für Benutzer mit Sehbehinderung schwierig wird, in der Liste zu navigieren. Das RadioButtons-Steuerelement unterstützt auch keine Enumeration, da das Steuerelement eine angemessene Anzahl von Elementen enthalten soll (siehe [Ist dies das richtige Steuerelement?](#is-this-the-right-control)).

## <a name="selection-follows-focus"></a>Auswahl folgt Fokus

Wenn ein Benutzer die Tastatur verwendet, um zwischen Elementen in einer RadioButtons-Liste zu navigieren, in der bereits ein Element ausgewählt ist, wird beim Verschieben des Fokus von einem zum anderen Element jeweils das Element, das den Fokus erhält, ausgewählt, und das Element, das zuvor den Fokus besaß, wird deaktiviert.

|Vor der Navigation mithilfe der Tastatur | Nach der Navigation mithilfe der Tastatur|
|:--|:--|
| ![Beispiel für Fokus und Auswahl vor der Tastaturnavigation](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*Beispiel für Fokus und Auswahl vor der Tastaturnavigation* | ![Beispiel für Fokus und Auswahl nach der Tastaturnavigation](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*Beispiel für Fokus und Auswahl nach der Tastaturnavigation, wobei die NACH-UNTEN- oder NACH-RECHTS-TASTE den Fokus zu Optionsfeld 3 verschiebt, dieses auswählt und Optionsfeld 2 deaktiviert.* |

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navigation mit Xbox-Gamepad und Xbox-Fernbedienung

Wenn ein Benutzer ein Xbox-Gamepad oder eine Xbox-Fernbedienung verwendet, um zwischen Optionsfeldern zu navigieren, ist das Verhalten „Auswahl folgt Fokus“ deaktiviert, und der Benutzer muss die „A“-Taste drücken, um das Optionsfeld mit dem Fokus auszuwählen.

## <a name="accessibility-behavior"></a>Barrierefreiheitsverhalten

In der folgenden Tabelle wird beschrieben, wie die Sprachausgabe eine RadioButtons-Gruppe behandelt und was angekündigt wird. Dieses Verhalten hängt davon ab, wie ein Benutzer die Detaileinstellungen der Sprachausgabe festgelegt hat.

| Anfänglicher Fokus | Fokus wechselt zu einem ausgewählten Element |
|:--|:--|
| Die RadioButton-Sammlung „Gruppenname“ hat den Fokus, und Element x von N Elementen ist ausgewählt. | Wenn das Optionsfeld (RadioButton) „Name“ ausgewählt ist, hat Element x den Fokus. |
| Die RadioButton-Sammlung „Gruppenname“ hat den Fokus, und kein Element ist ausgewählt.| Wenn das Optionsfeld (RadioButton) „Name“ nicht ausgewählt ist, hat Element x den Fokus. <br> Wenn der Benutzer UMSCHALT+Pfeiltasten verwendet, folgt die Auswahl nicht dem Fokus. |

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="The XAML Controls Gallery app icon"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, <a href="xamlcontrolsgallery:/item/RadioButton">öffnen Sie sie, um das RadioButtons-Steuerelement in Aktion zu sehen</a>.</p>
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

Im folgenden Code deklarieren Sie ein einfaches RadioButtons-Steuerelement mit drei Optionen:

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```
Die folgende Abbildung zeigt das Ergebnis:

![Optionsfelder in zwei Gruppen](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>Definieren mehrerer Spalten

Sie können ein mehrspaltiges RadioButtons-Steuerelement deklarieren, indem Sie die [MaxColumns-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns) angeben.

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

![Optionsfelder in zwei dreispaltigen Gruppen.](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>Datenbindung

Das RadioButtons-Steuerelement unterstützt die Datenbindung, die die [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource)-Eigenschaft verwendet, wie im folgenden Codeausschnitt gezeigt.

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

## <a name="create-your-own-radiobuttons-group"></a>Erstellen einer eigenen Optionsfeldgruppe (RadioButtons)

> [!Important]
> Außer wenn sie eine ältere Version von WinUI verwenden, empfiehlt es sich, das WinUI-RadioButtons-Steuerelement zu verwenden, um RadioButton-Elemente zu gruppieren.

Optionsfelder funktionieren in Gruppen. Sie können Optionsfelder auf zwei Arten gruppieren:

- Platzieren Sie sie im gleichen übergeordneten Container.
- Legen Sie die [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName)-Eigenschaft für jedes Optionsfeld auf denselben Wert fest.

In diesem Beispiel wird die erste Gruppe von Optionsfeldern implizit gruppiert, da die Felder im selben StackPanel-Element enthalten sind. Die zweite Gruppe wird auf zwei StackPanel-Elemente aufgeteilt, damit sie explizit nach „GroupName“ gruppiert werden.

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

Die folgende Abbildung zeigt, wie diese Optionsfeldgruppe (RadioButtons) gerendert wird:

![Optionsfelder in zwei Gruppen](images/radio-button-groups.png)

## <a name="radio-button-states"></a>Optionsfeldzustände

Ein Optionsfeld hat zwei Zustände: „aktiviert“ und „deaktiviert“. Wenn ein Optionsfeld aktiviert ist, ist die [IsChecked-Eigenschaft](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) auf `true` festgelegt. Wenn ein Optionsfeld deaktiviert ist, lautet seine „IsChecked“-Eigenschaft `false`. Ein Optionsfeld kann deaktiviert werden, wenn ein Benutzer ein anderes Optionsfeld in derselben Gruppe auswählt, doch es kann nicht durch erneutes Auswählen deaktiviert werden. Sie können ein Optionsfeld jedoch programmgesteuert durch Festlegen der „IsChecked“-Eigenschaft auf `false` deaktivieren.

## <a name="recommendations"></a>Empfehlungen

- Stellen Sie sicher, dass der Zweck und der aktuelle Status einer Gruppe von Optionsfeldern explizit ist.
- Begrenzen Sie die Textbezeichnung des Optionsfelds auf eine einzelne Zeile.
- Wenn die Textbezeichnung dynamisch ist, bedenken Sie, wie sich die Größe des Optionsfelds automatisch ändert sowie die Auswirkungen auf umliegende visuelle Elemente.
- Verwenden Sie die standardmäßige Schriftart, es sei denn, Sie müssen gemäß Ihren Markenrichtlinien eine andere verwenden.
- Platzieren Sie keine zwei Optionsfeldgruppen (RadioButtons) nebeneinander. Wenn sich zwei Optionsfeldgruppen direkt nebeneinander befinden, kann es für Benutzer schwierig sein, festzustellen, welche Optionsfelder zu welcher Gruppe gehören.

### <a name="visuals-to-consider"></a>Zu erwägende visuelle Elemente

Die folgenden Bilder veranschaulichen, wie Sie die Optionsfelder in einer RadioButtons-Gruppe am besten anordnen.

![Bild mit einer Gruppe von Optionsfeldern, vertikal angeordnet.](images/radiobutton-layout.png)

![Bild mit angezeigten Abstandsführungslinien für Optionsfelder.](images/radiobutton-redline.png)

> [!NOTE]
> Wenn Sie ein WinUI-RadioButtons-Steuerelement verwenden, sind Abstand, Ränder und Ausrichtung bereits optimiert.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- Informationen, wie Sie alle XAML-Steuerelemente in ein interaktives Format bekommen, finden Sie im [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery). 

## <a name="related-topics"></a>Zugehörige Themen

### <a name="for-designers"></a>Für Designer

- [Schaltflächen](buttons.md)
- [Umschalter](toggles.md)
- [Kontrollkästchen](checkbox.md)
- [Listen und Kombinationsfelder](lists.md)
- [Schieberegler](slider.md)

### <a name="for-developers-xaml"></a>Für Entwickler (XAML)

- [RadioButton-Klasse](/uwp/api/windows.ui.xaml.controls.radiobutton)