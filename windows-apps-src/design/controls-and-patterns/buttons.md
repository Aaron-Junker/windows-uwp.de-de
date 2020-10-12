---
title: Schaltflächen
description: Lernen Sie, wie Sie eine Schaltfläche verwenden, um Benutzern eine Möglichkeit zu geben, Sofortmaßnahmen auszulösen, und lernen Sie spezialisierte Schaltflächen für bestimmte Aufgaben kennen.
label: Buttons
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 38f62483d09b3ec75e9a670ddcd0c27b710d6a38
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829622"
---
# <a name="buttons"></a>Schaltflächen

Eine Schaltfläche ermöglicht dem Benutzer das unmittelbare Auslösen einer Aktion. Einige Schaltflächen sind speziell auf bestimmte Aufgaben wie Navigation, wiederholte Aktionen oder das Präsentieren von Menüs ausgelegt.

![Beispiel für Schaltflächen](images/controls/button.png)

Das [Extensible Application Markup Language (XAML)](../../xaml-platform/xaml-overview.md)-Framework stellt ein standardmäßiges Schaltflächen-Steuerelement sowie verschiedene spezialisierte Schaltflächen-Steuerelemente bereit.

Control | Beschreibung
------- | -----------
[Schaltfläche](/uwp/api/windows.ui.xaml.controls.button) | Eine Schaltfläche, die eine sofortige Aktion auslöst. Kann mit einem [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis oder einer [Command](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)-Bindung verwendet werden.
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Eine Schaltfläche, die im gedrückten Zustand fortlaufend ein [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis auslöst.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Eine Schaltfläche, die wie ein Link formatiert ist und für die Navigation verwendet wird. Weitere Informationen zu Hyperlinks finden unter [Hyperlinks](hyperlinks.md).
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | :::image type="icon" source="images/winui-logo-16x16.png"::: Eine Schaltfläche mit einem Chevron zum Öffnen eines angefügten Flyouts.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | :::image type="icon" source="images/winui-logo-16x16.png"::: Eine Schaltfläche mit zwei Seiten. Eine Seite initiiert eine Aktion, während die andere Seite ein Menü öffnet.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | :::image type="icon" source="images/winui-logo-16x16.png"::: Eine Umschaltfläche mit zwei Seiten. Mit einer Seite wird aktiviert/deaktiviert, über die andere Seite wird ein Menü geöffnet.
[ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton) | Eine Schaltfläche, die ein-oder ausgeschaltet werden kann.

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      :::image type="icon" source="images/winui-logo-64x64.png":::
   :::column-end:::
   :::column span="3":::
      **DropDownButton**, **SplitButton** und **ToggleSplitButton** sind in der Windows-UI-Bibliothek enthalten, einem NuGet-Paket mit neuen Steuerelementen und Benutzeroberflächenfeatures für Windows-Apps. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows-UI-Bibliotheks-APIs:** [DropDownButton-Klasse](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton), [SplitButton-Klasse](/uwp/api/microsoft.ui.xaml.controls.splitbutton), [ToggleSplitButton-Klasse](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)
>
> **Plattform-APIs:** [Click-Ereignis](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), [Command-Eigenschaft](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie ein **Button**-Steuerelement, um dem Benutzer das direkte Auslösen einer Aktion zu ermöglichen, z. B. das Absenden eines Formulars.

[Button](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton)-Steuerelemente sollten nicht verwendet werden, um zu anderen Seiten zu navigieren. Ein **HyperlinkButton** ist für diesen Zweck besser geeignet. Weitere Informationen zu Hyperlinks finden unter [Hyperlinks](hyperlinks.md).

> [!IMPORTANT]
> Für die Navigation in einem Assistenten können die Schaltflächen *Zurück* und *Weiter* verwendet werden. Für andere Arten der Rückwärtsnavigation oder der Navigation zu einer übergeordneten Ebene sollte stattdessen eine [Zurück-Schaltfläche](../basics/navigation-history-and-backwards-navigation.md) verwendet werden.

Verwenden Sie ein **RepeatButton**-Steuerelement, um Benutzern das wiederholte Auslösen einer Aktion zu ermöglichen. Verwenden Sie z. B. ein **RepeatButton**-Steuerelement, um einen Wert in einem Zähler zu erhöhen oder zu verringern.

Verwenden Sie ein **DropDownButton**-Steuerelement, wenn die Schaltfläche über ein Flyout mit weiteren Optionen verfügt. Das Standard-Chevron liefert einen visuellen Hinweis, dass die Schaltfläche ein Flyout enthält.

Verwenden Sie ein **SplitButton**-Steuerelement, wenn der Benutzer in der Lage sein soll, direkt eine Aktion auszulösen oder unabhängig davon unter weiteren Aktionen auszuwählen.

Verwenden Sie ein **ToggleButton**-Steuerelement, wenn der Benutzer in der Lage sein soll, zwischen zwei sich gegenseitig ausschließenden Zuständen sofort zu wechseln, und eine Schaltfläche die beste Lösung für Ihre Anforderungen an die Benutzeroberfläche ist. Wenn die Benutzeroberfläche nicht von einer Schaltfläche profitiert, ist es möglicherweise besser, mit [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [CheckBox](checkbox.md), [RadioButton](radio-button.md), oder [ToggleSwitch](toggles.md) zu arbeiten.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie den <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/Button">die App zu öffnen und die Schaltfläche in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

In diesem Beispiel werden die beiden Schaltflächen **Zulassen** und **Blockieren** in einem Dialogfeld verwendet, über das Zugriff auf den Standort des Benutzers angefordert wird.

![Beispiel für Schaltflächen in einem Dialogfeld](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>Erstellen einer Schaltfläche

Dieses Beispiel zeigt eine Schaltfläche, die auf einen Mausklick reagiert.

Erstellen Sie die Schaltfläche in XAML.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

Sie können die Schaltfläche auch in Code erstellen.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Behandeln Sie das [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>Interaktion mit Schaltflächen

Wenn Sie mit einem Finger oder Stift auf ein **Button**-Steuerelement tippen oder mit der linken Maustaste darauf klicken, löst die Schaltfläche das [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis aus. Bei einer Schaltfläche mit Tastaturfokus wird das **Click**-Ereignis auch durch Drücken der Eingabe- oder Leertaste ausgelöst.

Sie können für **Button**-Objekte generell keine [PointerPressed](/uwp/api/windows.ui.xaml.uielement.pointerpressed)-Ereignisse auf niedriger Ebene verarbeiten, da diese stattdessen mit dem **Click**-Verhalten konfiguriert sind. Weitere Informationen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](../../xaml-platform/events-and-routed-events-overview.md).

Sie können durch Ändern der [ClickMode](/uwp/api/windows.ui.xaml.controls.clickmode)-Eigenschaft festlegen, wie eine Schaltfläche das **Click**-Ereignis auslöst. Der **ClickMode**-Standardwert ist **Release**, Sie können den **ClickMode** einer Schaltfläche jedoch auch auf **Hover** oder **Press** festlegen. Wenn als **ClickMode**-Wert **Hover** festgelegt ist, kann das **Click**-Ereignis nicht über die Tastatur oder durch Berührung ausgelöst werden.


### <a name="button-content"></a>Inhalt von Schaltflächen

**Button** ist ein Inhaltsssteuerelement der [ContentControl](/uwp/api/Windows.UI.Xaml.Controls.ContentControl)-Klasse. Die XAML-Inhaltseigenschaft ist [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content). Damit ist für XAML folgende Syntax möglich: `<Button>A button's content</Button>`. Sie können jedes Objekt als Inhalt der Schaltfläche festlegen. Wenn der Inhalt ein [UIElement](/uwp/api/Windows.UI.Xaml.UIElement)-Objekt ist, wird er in der Schaltfläche gerendert. Wenn es sich beim Inhalt um einen anderen Objekttyp handelt, wird die entsprechende Zeichenfolgendarstellung in der Schaltfläche angezeigt.

Der Inhalt einer Schaltfläche ist für gewöhnlich Text. Wenn Sie diesen Text entwerfen, beachten Sie die folgenden Empfehlungen:

-  Verwenden Sie kurze, spezifische und selbsterklärende Texte, aus denen die Funktion einer Schaltfläche eindeutig hervorgeht. In der Regel umfasst der Schaltflächentext ein einzelnes Wort, und dieses ist ein Verb.

-  Verwenden Sie die standardmäßige Schriftart, es sei denn, Sie müssen entsprechend Ihren Markenrichtlinien eine andere verwenden.

-  Vermeiden Sie für kürzeren Text schmale Befehlsschaltflächen, indem Sie eine Schaltflächen-Mindestbreite von 120 Pixel verwenden.

- Vermeiden Sie für längeren Text breite Befehlsschaltflächen, indem Sie Text auf eine maximale Länge von 26 Zeichen beschränken.

-  Wenn der Textinhalt der Schaltfläche dynamisch ist (d. h. [lokalisiert](../globalizing/globalizing-portal.md) ist), ziehen Sie die Größenänderung der Schaltfläche und das in Betracht, was mit den Steuerelementen um sie herum geschieht.

<table>
<tr>
<td> <b>Folgendes muss geändert werden:</b><br> Schaltflächen mit überlaufendem Text. </td>
<td> <img src="images/button-wraptext.png" alt="Screenshot of two buttons, side by side, with labels that both say: Button with thxt that woul"/> </td>
</tr>
<tr>
<td> <b>Option 1:</b><br> Vergrößern Sie die Breite der Schaltflächen, stapeln Sie Schaltflächen und brechen Sie Text um, wenn er mehr als 26 Zeichen umfasst. </td>
<td> <img src="images/button-wraptext1.png" alt="Screenshot of two buttons with increased width, one over the other, with labels that both say: Button with thxt that would wrap."> </td>
</tr>
<tr>
<td> <b>Option 2:</b><br> Erhöhen Sie die Schaltflächenhöhe, und brechen Sie Text um. </td>
<td> <img src="images/button-wraptext2.png" alt="Screenshot of two buttons with increased height, side by side, with labels that both say: Button with thxt that would wrap."> </td>
</tr>
</table>

Sie können auch die visuellen Elemente anpassen, die die Darstellung der Schaltfläche ausmachen. Sie können beispielsweise den Text durch ein Symbol ersetzen oder zusätzlich zum Text ein Symbol verwenden.

Im folgenden Beispiel wird ein **StackPanel**-Element, das ein Bild und Text enthält, als Inhalt einer Schaltfläche festgelegt.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

Die Schaltfläche sieht wie folgt aus.

![Eine Schaltfläche mit Bild- und Textinhalt](images/button-orange.png)

## <a name="create-a-repeat-button"></a>Erstellen einer Wiederholungsschaltfläche

Ein [RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton)-Steuerelement ist eine Schaltfläche, die [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignisse auslöst, die andauern, solange die Schaltfläche betätigt wird. Legen Sie mit der [Delay](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.delay)-Eigenschaft fest, wie lange das **RepeatButton**-Steuerelement nach dem Betätigen der Schaltfläche wartet, bis die Klickaktion wiederholt wird. Legen Sie mit der [Interval](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.interval)-Eigenschaft das Intervall zwischen Wiederholungen der Klickaktion fest. Die Zeiten beider Eigenschaften werden in Millisekunden angegeben.

Das folgende Beispiel zeigt zwei **RepeatButton**-Steuerelemente. Ihre jeweiligen **Click**-Ereignisse dienen dazu, den Wert in einem Textblock zu erhöhen oder zu verringern.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="create-a-drop-down-button"></a>Erstellen einer Dropdown-Schaltfläche

> **DropDownButton** erfordert die [Windows-UI-Bibliothek](/uwp/toolkits/winui/) oder Windows 10, Version 1809 (SDK 17763) oder höher. Sie können das neueste SDK von [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) herunterladen. Frühere SDKs können Sie von [Windows SDK und Emulator-Archiv](https://developer.microsoft.com/windows/downloads/sdk-archive) herunterladen.

Ein [DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) ist eine Schaltfläche mit einem Chevron als visueller Indikator, mit dem angezeigt wird, dass ein angefügtes Flyout mit weiteren Optionen vorhanden ist. Es weist dasselbe Verhalten wie ein standardmäßiges **Button**-Steuerelement mit einem Flyout auf; lediglich die Darstellung ist anders.

Die Dropdown-Schaltfläche erbt das **Click**-Ereignis, dieses wird jedoch in der Regel nicht verwendet. Stattdessen verwenden Sie die **Flyout**-Eigenschaft, um ein Flyout anzufügen und Aktionen über die Menüoptionen im Flyout aufzurufen. Das Flyout wird beim Klicken auf die Schaltfläche automatisch geöffnet.
Sie müssen die [Placement](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement)-Eigenschaft des Flyouts angeben, um die gewünschte Platzierung in Bezug auf die Schaltfläche sicherzustellen. Der Standardalgorithmus für die Platzierung liefert möglicherweise nicht in allen Situationen die gewünschte Platzierung.

> [!TIP]
> Weitere Informationen zu Flyouts finden Sie unter [Menüs und Kontextmenüs](menus.md).

### <a name="example---drop-down-button"></a>Beispiel für eine Dropdown-Schaltfläche

In diesem Beispiel wird erläutert, wie Sie eine Dropdown-Schaltfläche mit einem Flyout erstellen, das Befehle für die Ausrichtung von Absätzen in einem **RichEditBox**-Steuerelement enthält. (Weitere Informationen und Code finden Sie unter [Rich-Edit-Feld](rich-edit-box.md).)

![Eine Dropdown-Schaltfläche mit den Ausrichtungsbefehlen](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8E4;"/>
    <DropDownButton.Flyout>
        <MenuFlyout Placement="BottomEdgeAlignedLeft">
            <MenuFlyoutItem Text="Left" Icon="AlignLeft" Tag="left"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Center" Icon="AlignCenter" Tag="center"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Right" Icon="AlignRight" Tag="right"
                            Click="AlignmentMenuFlyoutItem_Click"/>
        </MenuFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

```csharp
private void AlignmentMenuFlyoutItem_Click(object sender, RoutedEventArgs e)
{
    var option = ((MenuFlyoutItem)sender).Tag.ToString();

    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the alignment to the selected paragraphs.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (option == "left")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Left;
        }
        else if (option == "center")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Center;
        }
        else if (option == "right")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Right;
        }
    }
}
```

## <a name="create-a-split-button"></a>Erstellen einer unterteilten Schaltfläche

 > [!IMPORTANT]
 > **SplitButton** erfordert die [Windows-UI-Bibliothek](/uwp/toolkits/winui/) oder Windows 10, Version 1809 (SDK 17763) oder höher. Sie können das neueste SDK von [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) herunterladen. Frühere SDKs können Sie von [Windows SDK und Emulator-Archiv](https://developer.microsoft.com/windows/downloads/sdk-archive) herunterladen.

Ein [SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton)-Steuerelement weist zwei Teile auf, die separat aufgerufen werden können. Ein Teil verhält sich wie eine Standardschaltfläche und bewirkt, dass sofort eine Aktion aufgerufen wird. Mit dem anderen Teil wird ein Flyout mit zusätzlichen Optionen aufgerufen, aus denen der Benutzer wählen kann.

> [!NOTE]
> Bei Aufruf per Toucheingabe verhält sich die unterteilte Schaltfläche wie eine Dropdown-Schaltfläche; beide Hälften der Schaltfläche rufen das Flyout auf. Bei anderen Eingabemethoden kann der Benutzer eine Hälfte der Schaltfläche separat aufrufen.

Typisches Verhalten für eine unterteilte Schaltfläche:

- Wenn der Benutzer auf den Teil der Schaltfläche klickt, wird das **Click**-Ereignis verarbeitet, um die derzeit im Dropdown ausgewählte Option aufzurufen.

- Wenn das Dropdown geöffnet ist, wird der Aufruf der Elemente im Dropdown behandelt; dabei wird die ausgewählte Option gewechselt und aufgerufen. Es ist wichtig, dass das Flyout-Element aufgerufen wird, da das **Click**-Ereignis der Schaltfläche bei einer Toucheingabe nicht eintritt.

> [!TIP]
> Es gibt viele Möglichkeiten, Elemente in das Dropdown einzubinden und ihren Aufruf zu behandeln. Wenn Sie eine **ListView** oder **GridView** verwenden, besteht eine Möglichkeit darin, das **SelectionChanged**-Ereignis zu behandeln. Legen Sie in diesem Fall [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) auf **false** fest. Dadurch können Benutzer über Tastatureingaben durch die Optionen navigieren, ohne dass das Element bei jedem Wechseln aufgerufen wird.

### <a name="example---split-button"></a>Beispiel für eine unterteilte Schaltfläche

In diesem Beispiel wird veranschaulicht, wie Sie eine unterteilte Schaltfläche erstellen, über welche die Vordergrundfarbe von ausgewähltem Text in einem **RichEditBox**-Steuerelement geändert wird. (Weitere Informationen und Code finden Sie unter [Rich-Edit-Feld](rich-edit-box.md).)
Das Flyout der unterteilten Schaltfläche verwendet [BottomEdgeAlignedLeft](/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) als Standardwert für seine [Placement](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement)-Eigenschaft. Dieser Wert kann nicht überschrieben werden.

![Unterteilte Schaltfläche zur Auswahl der Vordergrundfarbe](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout">
            <!-- Set SingleSelectionFollowsFocus="False"
                 so that keyboard navigation works correctly. -->
            <GridView ItemsSource="{x:Bind ColorOptions}"
                      SelectionChanged="BrushSelectionChanged"
                      SingleSelectionFollowsFocus="False"
                      SelectedIndex="0" Padding="0">
                <GridView.ItemTemplate>
                    <DataTemplate>
                        <Rectangle Fill="{Binding}" Width="20" Height="20"/>
                    </DataTemplate>
                </GridView.ItemTemplate>
                <GridView.ItemContainerStyle>
                    <Style TargetType="GridViewItem">
                        <Setter Property="Margin" Value="2"/>
                        <Setter Property="MinWidth" Value="0"/>
                        <Setter Property="MinHeight" Value="0"/>
                    </Style>
                </GridView.ItemContainerStyle>
            </GridView>
        </Flyout>
    </SplitButton.Flyout>
</SplitButton>
```

```csharp
public sealed partial class MainPage : Page
{
    // Color options that are bound to the grid in the split button flyout.
    private List<SolidColorBrush> ColorOptions = new List<SolidColorBrush>();
    private SolidColorBrush CurrentColorBrush = null;

    public MainPage()
    {
        this.InitializeComponent();

        // Add color brushes to the collection.
        ColorOptions.Add(new SolidColorBrush(Colors.Black));
        ColorOptions.Add(new SolidColorBrush(Colors.Red));
        ColorOptions.Add(new SolidColorBrush(Colors.Orange));
        ColorOptions.Add(new SolidColorBrush(Colors.Yellow));
        ColorOptions.Add(new SolidColorBrush(Colors.Green));
        ColorOptions.Add(new SolidColorBrush(Colors.Blue));
        ColorOptions.Add(new SolidColorBrush(Colors.Indigo));
        ColorOptions.Add(new SolidColorBrush(Colors.Violet));
        ColorOptions.Add(new SolidColorBrush(Colors.White));
    }

    private void BrushButtonClick(object sender, object e)
    {
        // When the button part of the split button is clicked,
        // apply the selected color.
        ChangeColor();
    }

    private void BrushSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        // When the flyout part of the split button is opened and the user selects
        // an option, set their choice as the current color, apply it, then close the flyout.
        CurrentColorBrush = (SolidColorBrush)e.AddedItems[0];
        SelectedColorBorder.Background = CurrentColorBrush;
        ChangeColor();
        BrushFlyout.Hide();
    }

    private void ChangeColor()
    {
        // Apply the color to the selected text in a RichEditBox.
        Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
        if (selectedText != null)
        {
            Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
            charFormatting.ForegroundColor = CurrentColorBrush.Color;
            selectedText.CharacterFormat = charFormatting;
        }
    }
}
```

## <a name="create-a-toggle-split-button"></a>Erstellen einer unterteilten Umschaltfläche

> [!NOTE]
> **ToggleSplitButton** erfordert die [Windows-UI-Bibliothek](/uwp/toolkits/winui/) oder Windows 10, Version 1809 (SDK 17763) oder höher. Sie können das neueste SDK von [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) herunterladen. Frühere SDKs können Sie von [Windows SDK und Emulator-Archiv](https://developer.microsoft.com/windows/downloads/sdk-archive) herunterladen.

Ein [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton)-Steuerelement weist zwei Teile auf, die separat aufgerufen werden können. Ein Teil verhält sich wie eine Umschaltfläche, mit der eine Option aktiviert oder deaktiviert werden kann. Mit dem anderen Teil wird ein Flyout mit zusätzlichen Optionen aufgerufen, aus denen der Benutzer wählen kann.

Mit einer unterteilten Umschaltfläche wird in der Regel eine Funktion aktiviert bzw. deaktiviert, wenn die Funktion über mehrere Optionen verfügt, unter denen der Benutzer auswählen kann. Sie kann beispielsweise in einem Dokument-Editor verwendet werden, um Listen zu aktivieren oder zu deaktivieren, während über das Dropdown der Stil der Liste ausgewählt wird.

> [!NOTE]
> Beim Aufruf per Toucheingabe verhält sich die unterteilte Umschaltfläche wie eine Dropdown-Schaltfläche. Bei anderen Eingabemethoden kann der Benutzer die beiden Hälften der Schaltfläche umschalten und separat voneinander aufrufen. Beim Aufruf per Toucheingabe rufen beide Hälften der Schaltfläche das Flyout auf. Daher müssen Sie eine Option in den Flyout-Inhalt einbinden, mit der die Schaltfläche aktiviert bzw. deaktiviert wird.


### <a name="differences-with-togglebutton"></a>Unterschiede bei ToggleButton

Im Unterschied zu [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton) weist **ToggleSplitButton** keinen unbestimmten Zustand auf. Deshalb sollten Sie diese Unterschiede beachten:

- **ToggleSplitButton** verfügt weder über eine **IsThreeState**-Eigenschaft noch über das **Indeterminate**-Ereignis.
- Die [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked)-Eigenschaft hat nur einen booleschen Wert, keinen booleschen Wert, der **NULL-Werte zulässt<bool>** .
- **ToggleSplitButton** verfügt nur über das [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged)-Ereignis und nicht über die separaten Ereignisse **Checked** und **Unchecked**.


### <a name="example---toggle-split-button"></a>Beispiel für eine unterteilte Umschaltfläche

Im folgenden Beispiel wird veranschaulicht, wie eine unterteilte Umschaltfläche verwendet werden kann, um die Listenformatierung zu aktivieren bzw. zu deaktivieren und den Stil der Liste in einem **RichEditBox**-Steuerelement zu ändern. (Weitere Informationen und Code finden Sie unter [Rich-Edit-Feld](rich-edit-box.md).)
Das Flyout der unterteilten Umschaltfläche verwendet [BottomEdgeAlignedLeft](/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) als Standardwert für seine [Placement](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement)-Eigenschaft. Dieser Wert kann nicht überschrieben werden.

![Unterteilte Umschaltfläche zum Auswählen von Listenstilen](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout>
            <ListView x:Name="ListStylesListView"
                      SelectionChanged="ListStylesListView_SelectionChanged"
                      SingleSelectionFollowsFocus="False">
                <StackPanel Tag="bullet" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7C8;"/>
                    <TextBlock Text="Bullet" Margin="8,0"/>
                </StackPanel>
                <StackPanel Tag="alpha" Orientation="Horizontal">
                    <TextBlock Text="A" FontSize="24" Margin="2,0"/>
                    <TextBlock Text="Alpha" Margin="8"/>
                </StackPanel>
                <StackPanel Tag="numeric" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xF146;"/>
                    <TextBlock Text="Numeric" Margin="8,0"/>
                </StackPanel>
                <TextBlock Tag="none" Text="None" Margin="28,0"/>
            </ListView>
        </Flyout>
    </ToggleSplitButton.Flyout>
</ToggleSplitButton>
```

```csharp
private void ListStyleButton_IsCheckedChanged(ToggleSplitButton sender, ToggleSplitButtonIsCheckedChangedEventArgs args)
{
    // Use the toggle button to turn the selected list style on or off.
    if (((ToggleSplitButton)sender).IsChecked == true)
    {
        // On. Apply the list style selected in the drop down to the selected text.
        var listStyle = ((FrameworkElement)(ListStylesListView.SelectedItem)).Tag.ToString();
        ApplyListStyle(listStyle);
    }
    else
    {
        // Off. Make the selected text not a list,
        // but don't change the list style selected in the drop down.
        ApplyListStyle("none");
    }
}

private void ListStylesListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var listStyle = ((FrameworkElement)(e.AddedItems[0])).Tag.ToString();

    if (ListButton.IsChecked == true)
    {
        // Toggle button is on. Turn it off...
        if (listStyle == "none")
        {
            ListButton.IsChecked = false;
        }
        else
        {
            // or apply the new selection.
            ApplyListStyle(listStyle);
        }
    }
    else
    {
        // Toggle button is off. Turn it on, which will apply the selection
        // in the IsCheckedChanged event handler.
        ListButton.IsChecked = true;
    }
}

private void ApplyListStyle(string listStyle)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the list style to the selected text.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (listStyle == "none")
        {  
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.None;
        }
        else if (listStyle == "bullet")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Bullet;
        }
        else if (listStyle == "numeric")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Arabic;
        }
        else if (listStyle == "alpha")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.UppercaseEnglishLetter;
        }
        selectedText.ParagraphFormat = paragraphFormatting;
    }
}
```

## <a name="recommendations"></a>Empfehlungen

- Der Zweck und Status einer Schaltfläche müssen für den Benutzer eindeutig sein.

- Wenn mehrere Schaltflächen für die gleiche Entscheidung (z. B. in einem Bestätigungsdialogfeld) vorhanden sind, stellen Sie die Schaltflächen für den Commit in der folgenden Reihenfolge dar, wobei [Ausführen] und [Nicht ausführen] bestimmte Antworten auf die Hauptanweisung sind:
  - OK/[Ausführen]/Ja
    - [Nicht ausführen]/Nein
    - Abbrechen

- Stellen Sie dem Benutzer jeweils nur zwei Schaltflächen zur Verfügung, beispielsweise **Akzeptieren** und **Abbrechen**. Wenn Sie dem Benutzer mehr Aktionen zur Verfügung stellen möchten, sollten Sie [Kontrollkästchen](checkbox.md) oder [Optionsfelder](radio-button.md) verwenden, über die der Benutzer Aktionen mit einer einzelnen Befehlsschaltfläche auslösen kann.

- Für eine Aktion, die auf mehreren Seiten in Ihrer App verfügbar sein muss, sollten Sie die Verwendung einer [unteren App-Leiste](app-bars.md) in Erwägung ziehen, anstatt eine Schaltfläche auf mehreren Seiten zu duplizieren.


### <a name="recommended-single-button-layout"></a>Empfehlungen für Layouts mit einer einzigen Schaltfläche

Wenn in Ihrem Layout nur eine einzige Schaltfläche benötigt wird, sollten Sie diese je nach ihrem Containerkontext entweder linksbündig oder rechtsbündig ausrichten.

  - In Dialogfeldern mit nur einer einzigen Schaltfläche sollte die Schaltfläche **rechtsbündig** ausgerichtet sein. Stellen Sie bei Dialogfeldern mit nur einer einzigen Schaltfläche sicher, dass die Schaltfläche die sichere, nicht destruktive Aktion ausführt. Wenn Sie die Klasse [ContentDialog](./dialogs-and-flyouts/index.md) verwenden und nur eine einzige Schaltfläche definieren, wird diese Schaltfläche automatisch rechtsbündig ausgerichtet.

    ![Schaltfläche in einem Dialogfeld](images/pushbutton_doc_dialog.png)

  - Wird die Schaltfläche als Teil einer Containerbenutzeroberfläche angezeigt (z. B. innerhalb einer Popupbenachrichtigung, eines Flyouts oder eines Listenansicht-Elements), sollten Sie die Schaltfläche innerhalb des Containers **rechtsbündig** ausrichten.

    ![Schaltfläche in einem Container](images/pushbutton_doc_container.png)

  - Auf Seiten mit einer einzigen Schaltfläche (z. B. einer Einstellungsseite mit der Schaltfläche **Anwenden** am unteren Rand) sollten Sie die Schaltfläche **linksbündig** ausrichten. Das gewährleistet, dass die Schaltfläche passend zum übrigen Seiteninhalt ausgerichtet ist.

    ![Schaltfläche auf einer Seite](images/pushbutton_doc_page.png)


## <a name="back-buttons"></a>Zurück-Schaltflächen

Die Zurück-Schaltfläche ist ein durch das System bereitgestelltes Benutzeroberflächenelement, das die Rückwärtsnavigation über den Back-Stapel oder den Navigationsverlauf des Benutzers ermöglicht. Sie müssen keine eigene Zurück-Schaltfläche erstellen, aber unter Umständen ist etwas Aufwand erforderlich, um eine gute Rückwärtsnavigation zu ermöglichen. Weitere Informationen finden Sie im Artikel [Navigationsverlauf und Rückwärtsnavigation für Windows-Apps](../basics/navigation-history-and-backwards-navigation.md).


## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery): In diesem Beispiel werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.


## <a name="related-articles"></a>Verwandte Artikel

- [Button-Klasse](/uwp/api/windows.ui.xaml.controls.button)
- [Optionsfelder](radio-button.md)
- [Kontrollkästchen](checkbox.md)
- [Umschaltfläche](toggles.md)
