---
Description: Befehlsleiste Flyouts Benutzern Zugriff gewähren, Inline zu Ihrer app am häufigsten verwendeten Aufgaben.
title: Befehlsleisten-Flyout
label: Command bar flyout
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d5774b5301f7e8ce0616df72cfbf4fc81d0d0cf7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363251"
---
# <a name="command-bar-flyout"></a>Befehlsleisten-Flyout

Das Befehl-Flyout-Leiste können Sie die Benutzern einfachen Zugriff auf allgemeine Aufgaben führt Befehle in einer unverankerte Symbolleiste, die im Zusammenhang mit der ein Element im Zeichenbereich Benutzeroberfläche bieten.

![Eine erweiterte Text Befehlsleiste flyout](images/command-bar-flyout-header.png)

> CommandBarFlyout erfordert Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, oder die [Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).

> - **Plattform-APIs**: [CommandBarFlyout Klasse](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout Klasse](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [AppBarButton Klasse](/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton Klasse](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [ AppBarSeparator-Klasse](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Windows-APIs für UI-Bibliothek**: [CommandBarFlyout Klasse](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout-Klasse](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

Wie [CommandBar](app-bars.md), CommandBarFlyout hat **PrimaryCommands** und **SecondaryCommands** Eigenschaften Befehle hinzufügen können. Sie können Befehle in der Auflistung oder beiden platzieren. Wann und wie die primäre und sekundäre Befehle angezeigt werden, hängt von den Anzeigemodus ab.

Flyout Befehlsleiste verfügt über zwei Anzeigemodi: *reduziert* und *erweitert*.

- Im reduzierten Modus werden nur die primären Befehle angezeigt. Verfügt Ihr Befehlsleiste Flyout primäre und sekundäre Befehle verwenden, wird eine Schaltfläche "Weitere Informationen", die durch ein Auslassungszeichen dargestellt wird \[• • • anzuzeigen\], wird angezeigt. Dadurch kann der Benutzer Zugriff auf die sekundäre Befehle zu erhalten, durch den Übergang in den erweiterten Modus.
- Im erweiterten Modus werden die primären und sekundären Befehle angezeigt. (Wenn das Steuerelement nur sekundäre Elemente enthält, werden sie ähnlich wie für das Steuerelement MenuFlyout angezeigt.)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie das CommandBarFlyout-Steuerelement, um eine Auflistung von Befehlen für dem Benutzer, z. B. Schaltflächen und Menüelemente, im Kontext eines Elements auf der Canvas app anzuzeigen.

Die TextCommandBarFlyout zeigt Textbefehlen im Textfeld, TextBlock, RichEditBox, RichTextBlock und PasswordBox-Steuerelemente. Die Befehle sind automatisch entsprechend für die aktuelle Textauswahl konfiguriert. Verwenden Sie eine CommandBarFlyout, um die Standard-Text-Befehle auf Textsteuerelemente zu ersetzen.

Kontextbezogene anzuzeigenden Befehle für Listenelemente Anleitung im [kontextbezogene Befehle für Auflistungen und Listen](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout Vs MenuFlyout

Um Befehle in einem Kontextmenü anzuzeigen, können Sie CommandBarFlyout oder MenuFlyout verwenden. CommandBarFlyout wird empfohlen, da sie mehr Funktionen als MenuFlyout bereitstellt. Sie können CommandBarFlyout mit nur sekundäre Befehle verwenden, um das Verhalten zu erhalten und von einem MenuFlyout, oder verwenden Sie das Flyout der vollständige Befehlsleiste mit Befehlen für den primären und sekundären.

> Verwandte Informationen finden Sie unter [Flyouts](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menüs und Kontextmenüs](menus.md), und [der Befehlsleisten](app-bars.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CommandBarFlyout">öffnen Sie die app, und finden Sie unter den CommandBarFlyout in Aktion</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proaktive und reaktive Aufruf

Es gibt in der Regel zwei Möglichkeiten zum Aufrufen einer Flyout oder einem Menü, das mit einem auf dem UI-Canvas-Element zugeordnet ist: _proaktive Aufruf_ und _reaktive Aufruf_.

In proaktive aufrufen werden Befehle automatisch angezeigt, wenn der Benutzer mit dem Element interagiert, die die Befehle zugeordnet sind. Beispielsweise können Text Formatierungsbefehle angezeigt, wenn der Benutzer Text in einem Textfeld auswählt. In diesem Fall nimmt das Befehl-Flyout-Leiste nicht den Fokus. Stattdessen stellt er relevanten Befehle in der Nähe das Element, dem mit der Benutzer interagiert. Wenn der Benutzer mit den Befehlen interagieren nicht, werden sie verworfen.

Reaktive Aufruf werden Befehle als Reaktion auf eine explizite Benutzeraktion angezeigt, die Befehle anfordern; Beispiel: eine mit der rechten Maustaste. Dies entspricht dem herkömmlichen Konzept einer [Kontextmenü](menus.md).

Sie können die CommandBarFlyout in Weise oder sogar eine Mischung aus beiden verwenden.

## <a name="create-a-command-bar-flyout"></a>Erstellen Sie einen Befehlsleiste flyout

Dieses Beispiel zeigt, wie ein Befehlsleiste Flyout erstellen und verwenden sie proaktiv und reaktiv. Wenn das Bild angetippt wird, wird das Flyout im reduzierten Modus angezeigt. Wenn als Kontextmenü angezeigt wird, wird das Flyout in den erweiterten Modus angezeigt. In beiden Fällen kann der Benutzer erweitern oder reduzieren das Flyout aus, nachdem es geöffnet wurde.

![Beispiel für einen reduzierten Befehlsleiste flyout](images/command-bar-flyout-img-collapsed.png)

> _Eine reduzierte Befehlsleiste flyout_

![Beispiel für einen erweiterten Befehlsleiste flyout](images/command-bar-flyout-img-expanded.png)

> _Einen erweiterten Befehlsleiste flyout_

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Select all"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>Zeigen Sie proaktiv Befehle

Wenn Sie proaktiv kontextbezogene Befehle, die angezeigt werden, soll nur die primären Befehle werden standardmäßig angezeigt werden (der Befehlsleiste Flyout sollten reduziert werden.). Platzieren Sie die wichtigsten Befehle in der Auflistung der primären Befehle, und klicken Sie auf zusätzliche Befehle, die normalerweise in einem Kontextmenü in der Auflistung sekundäre Befehle aufgenommen werden würden.

Um Befehle proaktiv zu veranschaulichen, behandeln Sie in der Regel die [klicken Sie auf](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) oder [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) Ereignis, um das Befehl-Balken-Flyout anzeigen. Festlegen des Flyouts [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) zu **vorübergehende** oder **TransientWithDismissOnPointerMoveAway** das Flyout im reduzierten Modus zu öffnen, ohne den Fokus.

Ab Windows 10 Insider Preview können die Textsteuerelemente haben eine **SelectionFlyout** Eigenschaft. Wenn Sie diese Eigenschaft ein Flyout zuweisen, wird es automatisch angezeigt, wenn Text markiert ist.

### <a name="show-commands-reactively"></a>Befehle reaktiv anzeigen

Wenn Sie kontextbezogene Befehle, die reaktiv als Kontextmenü angezeigt werden, sind die sekundäre Befehle standardmäßig angezeigt (der Befehlsleiste Flyout sollten erweitert). In diesem Fall möglicherweise das Befehl-Leiste Flyout primäre und sekundäre Befehle oder nur sekundäre Befehle.

Um Befehle in einem Kontextmenü anzuzeigen, weisen Sie in der Regel das Flyout zum Abfragen der [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) Eigenschaft eines Benutzeroberflächenelements. Auf diese Weise öffnen das Flyout erfolgt durch das Element, und Sie müssen nichts weiter tun.

Verarbeitet das Flyout mit sich selbst (z. B. auf eine [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) Ereignis), legen Sie des Flyouts [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) zu **Standard** auf das Flyout in den erweiterten Modus zu öffnen und Geben Sie ihm den Fokus.

> [!TIP]
> Weitere Informationen zu Optionen, die beim Anzeigen eines Flyouts und zur Steuerung der Platzierung des Flyouts, finden Sie unter [Flyouts](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Befehle und Inhalt

Das Steuerelement CommandBarFlyout hat 2 – Eigenschaften, die Sie verwenden können, um Befehle und Inhalt hinzuzufügen: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) und [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Befehlsleistenelemente werden standardmäßig der **PrimaryCommands**-Sammlung hinzugefügt. Diese Befehle werden in der Befehlsleiste angezeigt und sind in beiden Modi zu- bzw. aufgeklappt sichtbar. Im Gegensatz zu CommandBar primären Befehle nicht automatisch einem Überlauf auf die sekundäre Befehle und möglicherweise abgeschnitten.

Sie können auch Befehle zum Hinzufügen der **SecondaryCommands** Auflistung. Sekundäre Befehle in der Menü-Teil des Steuerelements angezeigt werden und sind nur in den erweiterten Modus sichtbar.

### <a name="app-bar-buttons"></a>App-Leistenschaltflächen

Sie können die füllen PrimaryCommands und SecondaryCommands direkt mit [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton), und [AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) Steuerelemente.

Die Steuerelemente für die App-Leistenschaltfläche zeichnen sich durch ein Symbol und eine Textbeschriftung aus. Diese Steuerelemente sind für die Verwendung in einer Befehlsleiste optimiert, und ihre Darstellung ändert sich je nachdem, ob das Steuerelement in der Befehlsleiste oder Überlaufmenü angezeigt werden.

- App-Leiste Schaltflächen verwendet, wie die primären Befehle sind in der Befehlsleiste mit nur deren Symbol angezeigt, die Bezeichnung wird nicht angezeigt. Es wird empfohlen, eine QuickInfo verwenden, um eine textbeschreibung des Befehls angezeigt wie hier gezeigt.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- App-Leiste-Schaltflächen, die als sekundäre Befehle werden im Menü mit der Bezeichnung und den Symbol sichtbar angezeigt.

### <a name="other-content"></a>Andere Inhalte

Sie können ein Befehlsleiste Flyout andere Steuerelemente hinzufügen, indem Sie sie in einem AppBarElementContainer umschließen. Dadurch können Sie das Hinzufügen der Steuerelemente wie [DropDownButton](buttons.md) oder [SplitButton](buttons.md), oder Hinzufügen von Containern wie [StackPanel](buttons.md) zum Erstellen komplexer Benutzeroberfläche.

Damit Sie den primären oder sekundären Befehl Sammlungen der Flyout-Befehlsleiste hinzugefügt werden, muss ein Element implementieren die [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) Schnittstelle. AppBarElementContainer ist ein Wrapper, der diese Schnittstelle implementiert, sodass Sie ein Element mit einer Befehlsleiste hinzufügen können, auch wenn es nicht die Schnittstelle selbst implementiert.

Hier ist ein AppBarElementContainer verwendet, um einen Befehlsleiste Flyout zusätzliche Elemente hinzuzufügen. Auswahl von Farben ermöglicht die primären Befehle wird ein SplitButton hinzugefügt. Ein StackPanel wird die sekundäre Befehle aus, um ein komplexeres Layout für die Zoomsteuerelemente ermöglichen hinzugefügt.

> [!TIP]
> In der Standardeinstellung entwickelt wurden, für die app-Canvas sieht möglicherweise nicht rechts in einer Befehlsleiste. Wenn Sie ein Element mithilfe von AppBarElementContainer hinzufügen, gibt es einige Schritte, die Sie ausführen sollten, um das Element, das andere Element auf der Befehl übereinstimmen zu machen:
>
> - Überschreiben Sie die Standardpinsel mit [einfache Stile](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling) des Elements Hintergrund und Rahmen, die Schaltflächen auf der app entsprechen.
> - Passen Sie die Größe und Position des Elements.
> - Umschließen Sie Symbole mit einer Viewbox mit einer Breite und Höhe des 16px.

> [!NOTE]
> Dieses Beispiel zeigt nur den Befehlsleiste Flyout-Benutzeroberfläche es nicht die Befehle implementiert, die angezeigt werden. Weitere Informationen über die Befehle implementiert, finden Sie unter [Schaltflächen](buttons.md) und [Befehl Entwurf Grundlagen](../basics/commanding-basics.md).

![Ein Befehlsleiste Flyout mit einer unterteilten Schaltfläche](images/command-bar-flyout-split-button.png)

> _Eine reduzierte Befehlsleiste Flyout mit einer geöffneten SplitButton_

![Ein Befehlsleiste Flyout mit komplexen Benutzeroberfläche](images/command-bar-flyout-custom-ui.png)

> _Einen erweiterten Befehlsleiste Flyouts vergrößern/verkleinern Benutzeroberfläche im Menü_


```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Alignment controls -->
    <AppBarElementContainer>
        <SplitButton ToolTipService.ToolTip="Alignment">
            <SplitButton.Resources>
                <!-- Override default brushes to make the SplitButton 
                     match other command bar elements. -->
                <Style TargetType="SplitButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="SplitButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrush" Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </SplitButton.Resources>
            <SplitButton.Content>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="AlignLeft"/>
                </Viewbox>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Icon="AlignLeft" Text="Align left"/>
                    <MenuFlyoutItem Icon="AlignCenter" Text="Center"/>
                    <MenuFlyoutItem Icon="AlignRight" Text="Align right"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Alignment controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <!-- Override default brushes to make the Buttons 
                     match other command bar elements. -->
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
                <Style TargetType="Button">
                    <Setter Property="Height" Value="40"/>
                    <Setter Property="Width" Value="40"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,-4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="76"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="Zoom"/>
                </Viewbox>
                <TextBlock Text="Zoom" Margin="10,0,0,0" Grid.Column="1"/>
                <StackPanel Orientation="Horizontal" Grid.Column="2">
                    <Button ToolTipService.ToolTip="Zoom out">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomOut"/>
                        </Viewbox>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button ToolTipService.ToolTip="Zoom in">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomIn"/>
                        </Viewbox>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all" Icon="SelectAll"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Erstellen Sie ein Kontextmenü mit nur sekundäre Befehle

Können Sie eine CommandBarFlyout mit nur sekundäre Befehle als eine [Kontextmenü](menus.md), anstelle einer MenuFlyout.

![Ein Befehlsleiste Flyout mit nur sekundäre Befehle](images/command-bar-flyout-context-menu.png)

> _Befehlsleiste Flyout als Kontextmenü_

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarButton Label="Save" Icon="Save"/>
                <AppBarButton Label="Print" Icon="Print"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

Sie können auch eine CommandBarFlyout mit einem DropDownButton verwenden, an um ein Standardmenü zu erstellen.

![Ein Befehlsleiste Flyout mit als ein Dropdown-im Schaltflächenmenü](images/command-bar-flyout-dropdown.png)

> _Ein Dropdown-Menü der Schaltfläche in ein Flyout der Befehl-Leiste_

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Placeholder"/>
    <AppBarElementContainer>
        <DropDownButton Content="Mail">
            <DropDownButton.Resources>
                <!-- Override default brushes to make the DropDownButton 
                     match other command bar elements. -->
                <Style TargetType="DropDownButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>

                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </DropDownButton.Resources>
            <DropDownButton.Flyout>
                <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
                    <CommandBarFlyout.SecondaryCommands>
                        <AppBarButton Icon="MailReply" Label="Reply"/>
                        <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
                        <AppBarButton Icon="MailForward" Label="Forward"/>
                    </CommandBarFlyout.SecondaryCommands>
                </CommandBarFlyout>
            </DropDownButton.Flyout>
        </DropDownButton>
    </AppBarElementContainer>
    <AppBarButton Icon="Placeholder"/>
    <AppBarButton Icon="Placeholder"/>
</CommandBarFlyout>
```

## <a name="command-bar-flyouts-for-text-controls"></a>Befehlsleiste Flyouts für Textsteuerelemente

Die [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) ist eine spezielle Befehlsleiste Flyout, das Befehle zum Bearbeiten von Text enthält. Jeder Text-Steuerelement wird automatisch die TextCommandBarFlyout, als Kontextmenü (Rechtsklick), oder wenn Text markiert ist. Das Text-Befehlsleiste Flyout passt sich an die Textauswahl um nur die relevanten Befehle anzuzeigen.

![Ein reduzierten Text Befehlsleiste flyout](images/command-bar-flyout-text-selection.png)

> _Ein Text-Befehlsleiste Flyout auf ausgewählten text_

![Eine erweiterte Text Befehlsleiste flyout](images/command-bar-flyout-text-full.png)

> _Eine erweiterte Text Befehlsleiste flyout_


### <a name="available-commands"></a>Verfügbare Befehle

Diese Tabelle zeigt die Befehle in einem TextCommandBarFlyout, und wenn sie angezeigt werden.

| Befehl | Angezeigt... |
| ------- | -------- |
| Bold | Wenn das Steuerelement nicht schreibgeschützten (RichEditBox nur) ist. |
| Italic | Wenn das Steuerelement nicht schreibgeschützten (RichEditBox nur) ist. |
| Underline | Wenn das Steuerelement nicht schreibgeschützten (RichEditBox nur) ist. |
| Korrekturhilfen | Wenn IsSpellCheckEnabled ist **"true"** und falsch geschriebenen Text ausgewählt ist. |
| Ausschneiden | Wenn das Steuerelement ist nicht nur-Lese und Text ausgewählt ist. |
| Kopieren | Wenn Text markiert ist. |
| Einfügen | Wenn das Steuerelement ist nicht schreibgeschützt, und die Zwischenablage der Inhalt ist. |
| Rückgängig machen | Wenn eine Aktion, die rückgängig gemacht werden kann. |
| Alle auswählen | Wenn der Text ausgewählt werden können. |

### <a name="custom-text-command-bar-flyouts"></a>Benutzerdefinierten Text Befehlsleiste flyouts

TextCommandBarFlyout kann nicht angepasst werden, und von jeder Text-Steuerelement automatisch verwaltet wird. Allerdings können Sie die Standardeinstellung TextCommandBarFlyout mit benutzerdefinierten Befehlen ersetzen.

- Um die Standardeinstellung TextCommandBarFlyout zu ersetzen, die auf ausgewählten Text angezeigt wird, können Sie erstellen eine benutzerdefinierte CommandBarFlyout (oder ein anderer Typ "Flyout") und weisen sie Sie der **SelectionFlyout** Eigenschaft. Wenn Sie auf SelectionFlyout **null**, werden keine Befehle auf der Auswahl angezeigt.
- Um die Standardeinstellung TextCommandBarFlyout zu ersetzen, die als im Kontextmenü angezeigt wird, weisen Sie eine benutzerdefinierte CommandBarFlyout (oder ein anderer Typ "Flyout") zu den **ContextFlyout** Eigenschaft in einem Textsteuerelement. Wenn Sie auf ContextFlyout **null**, das im menüflyout angezeigt Steuerelement wird in früheren Versionen des Texts anstelle der TextCommandBarFlyout angezeigt.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Befehle der XAML-Beispiel](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Verwandte Artikel

- [Befehlsdesigngrundlagen für UWP-Apps](../basics/commanding-basics.md)
- [CommandBar-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
