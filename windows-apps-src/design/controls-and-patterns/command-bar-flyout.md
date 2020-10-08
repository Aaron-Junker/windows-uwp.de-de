---
Description: Über Befehlsleisten-Flyouts erhalten Benutzer Inlinezugriff auf die häufigsten Aufgaben in Ihrer App.
title: Befehlsleisten-Flyout
label: Command bar flyout
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a2f6e61373ae343d8d683d6e5f9169cc399f1594
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750546"
---
# <a name="command-bar-flyout"></a>Befehlsleisten-Flyout

Über das Befehlsleisten-Flyout können Sie Benutzern einfachen Zugriff auf häufig verwendete Aufgaben ermöglichen, indem Befehle in einer unverankerten Symbolleiste angezeigt werden, die mit einem Element in der Benutzeroberflächen-Canvas verbunden sind.

![Erweitertes Befehlsleisten-Text-Flyout](images/command-bar-flyout-text-full.png)

Wie [CommandBar](app-bars.md) verfügt auch CommandBarFlyout über die Eigenschaften **PrimaryCommands** und **SecondaryCommands**, über die Sie Befehle hinzufügen können. Sie können Befehle in einer oder beiden Sammlungen platzieren. Wann und wie die primären und sekundären Befehle angezeigt werden, hängt vom Anzeigemodus ab.

Das Befehlsleisten-Flyout verfügt über zwei Anzeigemodi: *Reduziert* und *Erweitert*.

- Im reduzierten Modus werden nur die primären Befehle angezeigt. Wenn das Befehlsleisten-Flyout über primäre und sekundäre Befehle verfügt, wird eine Schaltfläche zum Anzeigen weiterer Befehle angezeigt, die durch Auslassungspunkte \[***\] dargestellt wird. Über diese erhält der Benutzer durch Wechseln in den erweiterten Modus Zugriff auf die sekundären Befehle.
- Im erweiterten Modus werden die primären und die sekundären Befehle angezeigt. (Wenn das Steuerelement nur über sekundäre Elemente verfügt, werden diese ähnlich wie beim Steuerelement MenuFlyout angezeigt.)

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Das Steuerelement **CommandBarFlyout** ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

>**Windows-UI-Bibliotheks-APIs:** [CommandBarFlyout-Klasse](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout-Klasse](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)
>
>**Plattform-APIs:** [CommandBarFlyout-Klasse](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout-Klasse](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [AppBarButton-Klasse](/uwp/api/windows.ui.xaml.controls.appbarbutton), [AppBarToggleButton-Klasse](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [AppBarSeparator-Klasse](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>
> Für CommandBarFlyout ist Windows 10 Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher oder die [Windows-UI-Bibliothek](/uwp/toolkits/winui/) erforderlich.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie das Steuerelement CommandBarFlyout, um eine Sammlung von Befehlen, z. B. Schaltflächen und Menüelemente, im Kontext eines Elements in der App-Canvas für den Benutzer anzuzeigen.

Mit TextCommandBarFlyout werden Textbefehle in den Steuerelementen TextBox, TextBlock, RichEditBox, RichTextBlock und PasswordBox angezeigt. Die Befehle werden entsprechend der aktuellen Textauswahl automatisch konfiguriert. Verwenden Sie CommandBarFlyout, um die Standardtextbefehle in Textsteuerelementen zu ersetzen.

Um Kontextbefehle für Listenelemente anzuzeigen, befolgen Sie die Anweisungen unter [Kontextbefehle in Sammlungen und Listen](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout im Vergleich zu MenuFlyout

Zum Anzeigen von Befehlen in einem Kontextmenü können Sie CommandBarFlyout oder MenuFlyout verwenden. Die Verwendung von CommandBarFlyout wird empfohlen, da dieses Steuerelement mehr Funktionen als MenuFlyout umfasst. Sie können CommandBarFlyout nur mit sekundären Befehlen verwenden, um das Verhalten und Erscheinungsbild eines MenuFlyout-Steuerelements zu erhalten. Sie können aber auch das vollständige Befehlsleisten-Flyout mit primären und sekundären Befehlen verwenden.

> Entsprechende Informationen finden Sie unter [Flyouts](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menüs und Kontextmenüs](menus.md) und [Befehlsleisten](app-bars.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CommandBarFlyout">die App zu öffnen und CommandBarFlyout in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proaktiver und reaktiver Aufruf

Normalerweise gibt es zwei Möglichkeiten zum Aufrufen eines Flyouts oder Menüs, das einem Element in der Benutzeroberflächen-Canvas zugeordnet ist: _proaktiver Aufruf_ und _reaktiver Aufruf_.

Beim proaktiven Aufruf werden die Befehle automatisch angezeigt, wenn der Benutzer mit dem Element interagiert, dem die Befehle zugeordnet sind. Beispielsweise werden Befehle zur Textformatierung angezeigt, wenn der Benutzer Text in einem Textfeld auswählt. In diesem Fall erhält das Befehlsleisten-Flyout nicht den Fokus. Stattdessen werden relevante Befehle für das Element angezeigt, mit dem der Benutzer interagiert. Wenn der Benutzer die Befehle nicht verwendet, werden sie verworfen.

Beim reaktiven Aufruf werden die Befehle als Reaktion auf eine explizite Aktion des Benutzers zum Anfordern der Befehle angezeigt, z. B. Klicken mit der rechten Maustaste. Dies entspricht dem herkömmlichen Konzept eines [Kontextmenüs](menus.md).

Sie können CommandBarFlyout auf die eine oder andere Weise oder sogar mit einer Kombination aus beiden verwenden.

## <a name="create-a-command-bar-flyout"></a>Erstellen eines Befehlsleisten-Flyouts

Dieses Beispiel zeigt, wie ein Befehlsleisten-Flyout erstellt und wie es proaktiv sowie reaktiv verwendet wird. Wenn Sie auf das Bild tippen, wird das Flyout im reduzierten Modus angezeigt. Als Kontextmenü wird das Flyout im erweiterten Modus angezeigt. In beiden Fällen kann der Benutzer das geöffnete Flyout erweitern oder reduzieren.

![Beispiel für ein reduziertes Befehlsleisten-Flyout](images/command-bar-flyout-img-collapsed.png)

> _Reduziertes Befehlsleisten-Flyout_

![Beispiel für ein erweitertes Befehlsleisten-Flyout](images/command-bar-flyout-img-expanded.png)

> _Erweitertes Befehlsleisten-Flyout_

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

### <a name="show-commands-proactively"></a>Proaktives Anzeigen von Befehlen

Wenn Sie Kontextbefehle proaktiv anzeigen, sollten standardmäßig nur die primären Befehle angezeigt werden (das Befehlsleisten-Flyout sollte reduziert sein). Platzieren Sie die wichtigsten Befehle in der Sammlung der primären Befehle und zusätzliche Befehle, die herkömmlicherweise in einem Kontextmenü aufgenommen werden, in der Sammlung der sekundären Befehle.

Zum proaktiven Anzeigen von Befehlen wird normalerweise das [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)- oder [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped)-Ereignis verarbeitet, um das Befehlsleisten-Flyout anzuzeigen. Legen Sie [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) für das Flyout auf **Transient** oder **TransientWithDismissOnPointerMoveAway** fest, um das Flyout im reduzierten Modus ohne Fokus zu öffnen.

Ab Windows 10 Insider Preview umfassen Textsteuerelemente eine **SelectionFlyout**-Eigenschaft. Wenn Sie dieser Eigenschaft ein Flyout zuweisen, wird es automatisch angezeigt, wenn Text ausgewählt wird.

### <a name="show-commands-reactively"></a>Reaktives Anzeigen von Befehlen

Wenn Sie Kontextbefehle reaktiv als Kontextmenü anzeigen, werden standardmäßig die sekundären Befehle angezeigt (das Befehlsleisten-Flyout sollte erweitert sein). In diesem Fall können im Befehlsleisten-Flyout primäre und sekundäre Befehle oder nur sekundäre Befehle angezeigt werden.

Zum Anzeigen von Befehlen in einem Kontextmenü weisen Sie das Flyout normalerweise der [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout)-Eigenschaft eines Benutzeroberflächenelements zu. Auf diese Weise erfolgt das Öffnen des Flyouts über das Element, und Sie müssen keine weiteren Schritte ausführen.

Wenn Sie die Anzeige des Flyouts selbst festlegen möchten (z. B. für ein [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped)-Ereignis), legen Sie [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) für das Flyout auf **Standard** fest, um das Flyout im erweiterten Modus zu öffnen und den Fokus auf das Flyout zu übertragen.

> [!TIP]
> Weitere Informationen zu Optionen beim Anzeigen eines Flyouts und zum Steuern der Platzierung des Flyouts finden Sie unter [Flyouts](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Befehle und Inhalt

Das Steuerelement CommandBarFlyout umfasst 2 Eigenschaften, mit denen Sie Befehle und Inhalte hinzufügen können: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) und [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Befehlsleistenelemente werden standardmäßig der **PrimaryCommands**-Sammlung hinzugefügt. Diese Befehle werden auf der Befehlsleiste angezeigt und sind im reduzierten und im erweiterten Modus sichtbar. Im Unterschied zu CommandBar werden primäre Befehle nicht automatisch bei den sekundären Befehlen übernommen und werden möglicherweise gekürzt.

Sie können Befehle auch der **SecondaryCommands**-Sammlung hinzufügen. Sekundäre Befehle werden im Menübereich des Steuerelements angezeigt und sind nur im erweiterten Modus sichtbar.

### <a name="app-bar-buttons"></a>App-Leistenschaltflächen

Sie können PrimaryCommands und SecondaryCommands direkt mit Steuerelementen vom Typ [AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton), [AppBarToggleButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) und [AppBarSeparator](/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) füllen.

Die Steuerelemente für die App-Leistenschaltfläche zeichnen sich durch ein Symbol und eine Textbeschriftung aus. Diese Steuerelemente sind für die Verwendung auf Befehlsleisten optimiert, und ihr Erscheinungsbild verändert sich abhängig davon, ob das Steuerelement auf der Befehlsleiste oder im Überlaufmenü angezeigt wird.

- Die als primäre Befehle verwendeten App-Leistenschaltflächen werden auf der Befehlsleiste nur mit ihrem Symbol angezeigt, die Textbeschriftung wird nicht angezeigt. Es wird empfohlen, eine QuickInfo zu verwenden, um wie hier veranschaulicht eine Textbeschreibung des Befehls anzuzeigen.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Die als sekundäre Befehle verwendeten App-Leistenschaltflächen werden mit der Beschriftung und dem Symbol im Menü angezeigt.

### <a name="other-content"></a>Andere Inhalte

Sie können einem Befehlsleisten-Flyout weitere Befehle hinzufügen, indem Sie diese in einem AppBarElementContainer umschließen. Dadurch können Sie Steuerelemente wie [DropDownButton](buttons.md) oder [SplitButton](buttons.md) oder Container wie [StackPanel](buttons.md) hinzufügen und eine komplexere Benutzeroberfläche erstellen.

Damit ein Element der Sammlung der primären oder der sekundären Befehle eines Befehlsleisten-Flyouts hinzugefügt werden kann, muss es die [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement)-Schnittstelle implementieren. AppBarElementContainer ist ein Wrapper, der diese Schnittstelle implementiert, sodass Sie einer Befehlsleiste ein Element hinzufügen können, auch wenn das Element selbst die Schnittstelle nicht implementiert.

Hier werden mithilfe von AppBarElementContainer einem Befehlsleisten-Flyout zusätzliche Elemente hinzugefügt. Den primären Befehlen wird ein SplitButton hinzugefügt, um die Auswahl von Farben zu ermöglichen. Den sekundären Befehlen wird ein StackPanel hinzugefügt, um ein komplexeres Layout für Zoomsteuerelemente zu ermöglichen.

> [!TIP]
> Standardmäßig werden die für die App-Canvas entworfenen Elemente in einer Befehlsleiste möglicherweise nicht richtig dargestellt. Wenn Sie ein Element mithilfe von AppBarElementContainer hinzufügen, müssen Sie einige Schritte ausführen, damit das Element mit anderen Elementen der Befehlsleiste übereinstimmt:
>
> - Überschreiben Sie die Standardpinsel mit [einfacher Formatierung](./xaml-styles.md#lightweight-styling), um den Hintergrund und den Rahmen des Elements an die App-Leistenschaltflächen anzupassen.
> - Passen Sie die Größe und die Position des Elements an.
> - Umschließen Sie Symbole in einer Viewbox mit einer Breite und Höhe von 16 Pixel.

> [!NOTE]
> In diesem Beispiel wird nur die Benutzeroberfläche des Befehlsleisten-Flyouts angezeigt, die angezeigten Befehle werden nicht implementiert. Weitere Informationen zum Implementieren der Befehle finden Sie unter [Schaltflächen](buttons.md) und [Befehlsdesigngrundlagen](../basics/commanding-basics.md).

![Befehlsleisten-Flyout mit einer unterteilten Schaltfläche](images/command-bar-flyout-split-button.png)

> _Reduziertes Befehlsleisten-Flyout mit einer geöffneten unterteilten Schaltfläche_

![Befehlsleisten-Flyout mit komplexer Benutzeroberfläche](images/command-bar-flyout-custom-ui.png)

> _Erweitertes Befehlsleisten-Flyout mit Benutzeroberfläche für benutzerdefinierten Zoom im Menü_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Erstellen eines Kontextmenüs mit nur sekundären Befehlen

Anstelle eines MenuFlyout können Sie ein CommandBarFlyout, das nur sekundäre Befehle enthält, als [Kontextmenü](menus.md) verwenden.

![Befehlsleisten-Flyout mit nur sekundären Befehlen](images/command-bar-flyout-context-menu.png)

> _Befehlsleisten-Flyout als Kontextmenü_

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

Sie können ein CommandBarFlyout auch mit DropDownButton verwenden, um ein Standardmenü zu erstellen.

![Befehlsleisten-Flyout mit einem Dropdown-Schaltflächenmenü](images/command-bar-flyout-dropdown.png)

> _Dropdown-Schaltflächenmenü in einem Befehlsleisten-Flyout_

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

## <a name="command-bar-flyouts-for-text-controls"></a>Befehlsleisten-Flyouts für Textsteuerelemente

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) ist ein spezielles Befehlsleisten-Flyout, das Befehle zur Bearbeitung von Text enthält. Jedes Textsteuerelement zeigt TextCommandBarFlyout automatisch als Kontextmenü (Klick mit der rechten Maustaste) oder bei Auswahl von Text an. Das Befehlsleisten-Text-Flyout passt sich an die Textauswahl an, sodass nur relevante Befehle angezeigt werden.

![Reduziertes Befehlsleisten-Text-Flyout](images/command-bar-flyout-text-selection.png)

> _Befehlsleisten-Text-Flyout bei der Textauswahl_

![Erweitertes Befehlsleisten-Text-Flyout](images/command-bar-flyout-text-full.png)

> _Erweitertes Befehlsleisten-Text-Flyout_


### <a name="available-commands"></a>Verfügbare Befehle

In der folgenden Tabelle sind die in TextCommandBarFlyout enthaltenen Befehle aufgeführt. Zudem ist angegeben, wann sie angezeigt werden.

| Befehl | Wird angezeigt ... |
| ------- | -------- |
| Fett | wenn das Steuerelement nicht schreibgeschützt ist (nur RichEditBox). |
| Kursiv | wenn das Steuerelement nicht schreibgeschützt ist (nur RichEditBox). |
| Underline | wenn das Steuerelement nicht schreibgeschützt ist (nur RichEditBox). |
| Korrektur | wenn IsSpellCheckEnabled auf **true** festgelegt und Text mit Rechtschreibfehlern ausgewählt ist. |
| Ausschneiden | wenn das Steuerelement nicht schreibgeschützt ist und Text ausgewählt ist. |
| Kopieren | wenn Text ausgewählt ist. |
| Einfügen | wenn das Steuerelement nicht schreibgeschützt ist und die Zwischenablage Inhalte enthält. |
| Rückgängig machen | wenn eine Aktion vorhanden ist, die rückgängig gemacht werden kann. |
| Alle auswählen | wenn Text ausgewählt werden kann. |

### <a name="custom-text-command-bar-flyouts"></a>Benutzerdefinierte Befehlsleisten-Text-Flyouts

TextCommandBarFlyout kann nicht angepasst werden und wird von jedem Textsteuerelement automatisch verwaltet. Sie können jedoch das Standard-TextCommandBarFlyout durch benutzerdefinierte Befehle ersetzen.

- Um das standardmäßig bei der Textauswahl angezeigte TextCommandBarFlyout zu ersetzen, können Sie ein benutzerdefiniertes CommandBarFlyout (oder einen anderen Flyout-Typ) erstellen und der **SelectionFlyout**-Eigenschaft zuweisen. Wenn Sie SelectionFlyout auf **null** festlegen, werden bei der Textauswahl keine Befehle angezeigt.
- Um das standardmäßig als Kontextmenü angezeigte TextCommandBarFlyout zu ersetzen, weisen Sie der **ContextFlyout**-Eigenschaft eines Textsteuerelements ein benutzerdefiniertes CommandBarFlyout (oder einen anderen Flyout-Typ) zu. Wenn Sie ContextFlyout auf **null** festlegen, wird das Menü-Flyout, das in vorherigen Versionen des Textsteuerelements angezeigt wird, anstelle von TextCommandBarFlyout angezeigt.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Beispiel für XAML-Befehle](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>Verwandte Artikel

- [Grundlagen des Befehlsdesigns für Windows-Apps](../basics/commanding-basics.md)
- [CommandBar-Klasse](/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
