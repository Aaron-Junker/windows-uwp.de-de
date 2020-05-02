---
Description: Mit Optionsfeldern können Benutzer eine oder mehrere Optionen auswählen.
title: Richtlinien für Optionsfelder
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1e7fb764f3d21c260080d8846df3c66c65dccdb5
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81123622"
---
# <a name="radio-buttons"></a>Optionsfelder

Mithilfe von Optionsfeldern können Benutzer eine Option aus einer Gruppe von Optionen auswählen. Jede Option wird durch ein Optionsfeld dargestellt, und Benutzer können nur ein Optionsfeld in einer Gruppe von Optionsfeldern auswählen.

(Falls Sie sich über die englische Bezeichnung „Radio Button” wundern: Optionsfelder sind im Englischen nach den Tasten mit voreingestellten Sendern an einem Radio benannt.)

![Optionsfelder](images/controls/radio-button.png)

> **Plattform-APIs:** [RadioButton-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [Checked-Ereignis](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked), [IsChecked-Eigenschaft](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie Optionsfelder, um Benutzern mindestens zwei Optionen bereitzustellen, die sich gegenseitig ausschließen.

![Eine Gruppe von Optionsfeldern](images/radiobutton_basic.png)

Verwenden Sie Optionsfelder, wenn Benutzer alle Optionen sehen müssen, um eine Auswahl treffen zu können. Da Optionsfelder alle Optionen gleichermaßen hervorheben, schenken Benutzer den Optionen möglicherweise mehr Aufmerksamkeit, als eigentlich erforderlich ist. Sie können auch andere Steuerelemente verwenden, es sei denn, die Optionen erfordern zusätzliche Aufmerksamkeit seitens des Benutzers. Verwenden Sie beispielsweise demgegenüber eine [Dropdownliste](lists.md), wenn die standardmäßige Option für die meisten Benutzer in den meisten Situationen empfohlen ist.

![Dropdownliste](images/combo_box_collapsed.png)

Wenn nur zwei sich gegenseitig ausschließende Optionen vorhanden sind, kombinieren Sie sie in einem einzelnen [Kontrollkästchen](checkbox.md) oder [Umschalter](toggles.md). Verwenden Sie beispielsweise ein Kontrollkästchen für "Ich stimme zu" anstelle von zwei Optionsfeldern für "Ich stimme zu" und "Ich stimme nicht zu".

![Zwei Möglichkeiten für die Darstellung einer binären Auswahl](images/radiobutton_vs_checkbox.png)

Wenn der Benutzer mehrere Optionen auswählen kann, verwenden Sie ein [Kontrollkästchen](checkbox.md).

![Auswählen mehrerer Optionen mit Kontrollkästchen](images/checkbox2.png)

Wenn Optionen Zahlen mit festgelegten Schritten (10, 20, 30) sind, verwenden Sie ein [Schieberegler](slider.md)-Steuerelement.

![Schiebereglersteuerelement](images/controls/slider.png)

Wenn mehr als acht Optionen vorhanden sind, verwenden Sie eine [Dropdownliste](lists.md) oder ein [Listenfeld](lists.md).

![Kombinationsfeld](images/combo_box_scroll.png)

Wenn die verfügbaren Optionen auf dem aktuellen Kontext der App basieren oder anderweitig dynamisch variieren können, verwende ein [Listenfeld](lists.md) für die Einfachauswahl.

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

Optionsfelder in den Browsereinstellungen.

![Optionsfelder in den Browsereinstellungen](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>Erstellen eines Optionsfelds

Optionsfelder funktionieren in Gruppen. Es gibt zwei Arten zur Gruppierung von Optionsfeld-Steuerelementen:
- Platzieren Sie sie im gleichen übergeordneten Container.
- Legen Sie die [GroupName](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName)-Eigenschaft für jedes Optionsfeld auf denselben Wert fest.

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

Die Optionsfeldgruppen sehen wie folgt aus.

![Optionsfelder in zwei Gruppen](images/radio-button-groups.png)

Ein Optionsfeld hat zwei Zustände: *aktiviert* und *deaktiviert*. Wenn ein Optionsfeld aktiviert ist, ist die [IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)-Eigenschaft auf **true** festgelegt. Wenn ein Optionsfeld deaktiviert ist, lautet die **IsChecked**-Eigenschaft **false**. Ein Optionsfeld kann durch Klicken auf ein anderes Optionsfeld in derselben Gruppe deaktiviert werden, jedoch nicht durch erneutes Klicken auf das Optionsfeld selbst. Sie können ein Optionsfeld jedoch programmgesteuert durch Festlegen der IsChecked-Eigenschaft auf **false** deaktivieren. Sie können die **IsChecked**-Eigenschaft tatsächlich mit einem booleschen Wert vergleichen, indem Sie den **Wert** der **IsChecked**-Eigenschaft abrufen.

## <a name="recommendations"></a>Empfehlungen

-   Stellen Sie sicher, dass der Zweck und der aktuelle Status einer Gruppe von Optionsfeldern nachvollziehbar ist.
-   Begrenze den Text des Optionsfelds auf eine einzelne Zeile.
-   Wenn der Textinhalt dynamisch ist, bedenken Sie die Größenänderung der Schaltfläche und die visuellen Effekte herum.
-   Verwenden Sie die standardmäßige Schriftart, es sei denn, Sie müssen gemäß Ihren Markenrichtlinien eine andere verwenden.
-   Platzieren Sie keine zwei Optionsfeldgruppen nebeneinander. Wenn sich zwei Optionsfeldgruppen direkt nebeneinander befinden, ist es schwierig, festzustellen, welche Schaltflächen zu welcher Gruppe gehören.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung

Diese Abbildung zeigt die richtige Vorgehensweise zum Platzieren und Anordnen von Optionsfeldern in geeignetem Abstand.

![Gruppe von Optionsfeldern](images/radiobutton-layout.png)

![Abstandsrichtlinien für Optionsfelder](images/radiobutton-redlines.png)

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
