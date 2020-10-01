---
title: Befehle in Windows-Apps
description: In diesem Artikel wird beschrieben, wie die XamlUICommand-Klasse und die StandardUICommand-Klasse (zusammen mit der ICommand-Schnittstelle) verwendet werden, um Befehle über verschiedene Steuerelementtypen freizugeben und zu verwalten, und zwar unabhängig vom verwendeten Geräte- und Eingabetyp.
ms.service: ''
ms.topic: overview
ms.date: 09/24/2020
ms.openlocfilehash: 5ef1cb0e78757ee575a0129a60252d1a7404a6d4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217373"
---
# <a name="commanding-in-windows-apps-using-standarduicommand-xamluicommand-and-icommand"></a>Befehle in Windows-Apps, die StandardUICommand, XamlUICommand und ICommand verwenden

In diesem Artikel werden Befehle in Windows-Anwendungen beschrieben. Insbesondere wird erläutert, wie Sie mit der [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand)-Klasse und der [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)-Klasse (zusammen mit der ICommand-Schnittstelle) Befehle über verschiedene Steuerelementtypen hinweg freigeben und verwalten, und zwar unabhängig vom verwendeten Geräte- und Eingabetyp.

![Eine Abbildung zur Veranschaulichung einer gängigen Verwendung eines freigegebenen Befehls: mehrere Benutzeroberflächen mit einem Favorit-Befehl](images/commanding/generic-commanding.png)

*Freigeben von Befehlen über verschiedene Steuerelemente hinweg, unabhängig vom Geräte- und Eingabetyp*

## <a name="important-apis"></a>Wichtige APIs

- [Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) und [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>Übersicht

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

Befehle können direkt über UI-Interaktionen aufgerufen werden, z. B. durch Klicken auf eine Schaltfläche oder Auswählen eines Elements in einem Kontextmenü. Sie können auch indirekt über ein Eingabegerät wie ein Tastaturkürzel, Gesten, Spracherkennung oder ein Automatisierungs-/Barrierefreiheitstool aufgerufen werden. Nach dem Aufrufen kann der Befehl durch ein Steuerelement (Textnavigation in einem Bearbeitungssteuerelement), ein Fenster (Rückwärtsnavigation) oder die Anwendung (Beenden) verarbeitet werden.

Befehle können in einem bestimmten Kontext der App ausgeführt werden (z. B. Löschen von Text oder Rückgängigmachen einer Aktion), oder sie können kontextfrei sein (z. B. Stummschalten von Audio oder Einstellen der Helligkeit).

In der folgenden Abbildung sind zwei Befehlsschnittstellen (eine [CommandBar](app-bars.md) und ein unverankertes, kontextbezogenes [CommandBarFlyout](command-bar-flyout.md)) dargestellt, die einige Befehle gemeinsam nutzen.

![Erweiterte Befehlsleiste](images/control-examples/command-bar-photos.png)<br>*Befehlsleiste*

![Kontextmenü in der Microsoft Fotos-Galerie](images/ContextMenu_example.png)<br>*Kontextmenü in der Microsoft Fotos-Galerie*

## <a name="command-interactions"></a>Befehlsinteraktionen

Aufgrund der Vielzahl von Geräten, Eingabearten und Benutzeroberflächen, die sich auf das Aufrufen eines Befehls auswirken können, empfehlen wir, dass Sie Ihre Befehle über möglichst viele Befehlsschnittstellen verfügbar machen. Hierzu kann eine Kombination aus [Swipe](swipe.md), [MenuBar](menus.md), [CommandBar](app-bars.md), [CommandBarFlyout](command-bar-flyout.md) und dem herkömmlichen [Kontextmenü](menus.md) zählen.

**Verwenden Sie für wichtige Befehle eingabespezifische Beschleuniger.** Mit Eingabebeschleunigern können Benutzer je nach verwendetem Eingabegerät Aktionen schneller ausführen.

Beispiele gängiger Eingabebeschleuniger für diverse Eingabetypen:

- **Zeiger**: Hoverschaltflächen für Maus und Stift
- **Tastatur**: Tastenkombinationen (Zugriffstasten)
- **Toucheingabe**: Wischen
- **Toucheingabe**: Aktualisieren von Daten durch Ziehen

Sie müssen Eingabetyp und Benutzeroberflächen berücksichtigen, um die Funktionen Ihrer Anwendung universell zugänglich zu machen. Sammlungen (insbesondere die, welche durch Benutzer bearbeitet werden können) enthalten beispielsweise im Allgemeinen eine Vielzahl spezifischer Befehle, die je nach Eingabegerät recht unterschiedlich ausgeführt werden.

In der nachstehenden Tabelle sind einige typische Befehle für Sammlungen sowie Möglichkeiten aufgeführt, diese Befehle verfügbar zu machen.

| Befehl          | Eingabeartunabhängig | Beschleuniger für die Mauseingabe | Tastaturkürzel | Beschleuniger für die Toucheingabe |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Element löschen      | Kontextmenü   | Hoverschaltfläche      | ENTF-TASTE              | Löschen per Wischen   |
| Element kennzeichnen        | Kontextmenü   | Hoverschaltfläche      | STRG+UMSCHALT+G         | Kennzeichnen per Wischen     |
| Daten aktualisieren     | Kontextmenü   | NICHT ZUTREFFEND               | F5-TASTE               | Aktualisierung durch Ziehen   |
| Element als Favorit speichern | Kontextmenü   | Hoverschaltfläche      | F-TASTE, STRG+S            | Als Favorit speichern per Wischen |

**Stellen Sie stets ein Kontextmenü bereit** Wir empfehlen, alle relevanten kontextbezogenen Befehle in einem traditionellen Kontextmenü oder in CommandBarFlyout einzuschließen, da beide für sämtliche Eingabetypen unterstützt werden. Wenn ein Befehl beispielsweise nur während eines Mauszeiger-Hoverereignisses verfügbar gemacht wird, kann er nicht auf einem reinen Toucheingabegerät verwendet werden.

## <a name="commands-in-windows-applications"></a>Befehle in Windows-Anwendungen

Es gibt verschiedene Möglichkeiten, Befehle in einer Windows-Anwendung gemeinsam zu nutzen und zu verwalten. Sie können Ereignishandler für Standardinteraktionen wie Click im CodeBehind definieren (dies kann je nach Komplexität der UI recht ineffizient sein), Sie können Ereignislistener für Standardinteraktionen an einen gemeinsam genutzten Handler binden, oder Sie können die Command-Eigenschaft des Steuerelements an eine ICommand-Implementierung binden, die die Befehlslogik beschreibt.

Um umfassende Benutzererfahrungen für alle Befehlsoberflächen effizient und mit minimaler Codeduplizierung bereitzustellen, empfehlen wir, die in diesem Artikel beschriebenen Befehlsbindungsfunktionen zu verwenden (Informationen zum Verarbeiten von Standardereignissen erhalten Sie in den Artikeln zu den einzelnen Ereignissen).

Um ein Steuerelement an eine gemeinsam genutzte Befehlsressource zu binden, können Sie die ICommand-Schnittstellen selbst implementieren, oder Sie können Ihren Befehl in der [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand)-Basisklasse oder anhand eines der Plattformbefehle erstellen, die von der abgeleiteten [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)-Klasse definiert werden.

- Mit der ICommand-Schnittstelle ([Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) oder [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand)) können Sie komplett angepasste, in der gesamten App wiederverwendbare Befehle erstellen.
- [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) bietet diese Möglichkeit auch, vereinfacht jedoch die Entwicklung, indem eine Reihe von integrierten Befehlseigenschaften wie Befehlsverhalten, Tastenkombinationen (Zugriffstasten und Tastaturkürzel), Symbol, Bezeichnung und Beschreibung verfügbar gemacht werden.
- Die Vorgehensweise wird durch [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) noch weiter vereinfacht, denn hiermit können Sie aus einer Reihe von standardmäßigen Plattformbefehlen mit vordefinierten Eigenschaften auswählen.

> [!Important]
> In UWP-Anwendungen sind Befehle je nach gewählter Sprache Implementierungen der [Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand)-Schnittstelle (C++) oder der [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand)-Schnittstelle (C#).

## <a name="command-experiences-using-the-standarduicommand-class"></a>Befehle, die die StandardUICommand-Klasse verwenden

Abgeleitet von [XamlUiCommand](/uwp/api/windows.ui.xaml.input.xamluicommand) (abgeleitet von [Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) für C++ bzw. [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand) für C#) macht die [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)-Klasse eine Reihe von standardmäßigen Plattformbefehlen mit vordefinierten Eigenschaften wie Symbol, Tastaturkürzel und Beschreibung verfügbar.

Ein [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) bietet eine schnelle und konsistente Möglichkeit, gängige Befehle wie `Save` oder `Delete` zu definieren. Dafür müssen Sie lediglich die execute-Funktion und die canExecute-Funktion bereitstellen.

### <a name="example"></a>Beispiel

![Beispiel für StandardUICommand](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

| Code für dieses Beispiel herunterladen |
| -------------------- |
| [Beispiel für UWP-Befehle (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip) |

In diesem Beispiel wird veranschaulicht, wie eine grundlegende [ListView](listview-and-gridview.md) mit einem Befehl „Element löschen“ erweitert wird, der durch die [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)-Klasse implementiert wird, wobei gleichzeitig die Benutzererfahrung für eine Vielzahl von Eingabetypen mit einer [MenuBar](menus.md), einem [Swipe](swipe.md)-Steuerelement, Hoverschaltflächen und einem [Kontextmenü](menus.md) optimiert wird.

> [!NOTE]
> Dieses Beispiel erfordert das Nuget-Paket „Microsoft.UI.Xaml.Controls“, das Teil der [Microsoft Windows-UI-Bibliothek](/uwp/toolkits/winui/) ist.

**XAML:**

Die Beispiel-UI enthält eine [ListView](/uwp/api/windows.ui.xaml.controls.listview) mit fünf Elementen. Der [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) „Löschen“ wird an ein [MenuBarItem](/uwp/api/microsoft.ui.xaml.controls.menubaritem), ein [SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem), ein [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton) und ein [ContextFlyout-Menü](/uwp/api/windows.ui.xaml.uielement.contextflyout) gebunden.

``` xaml
<Page
    x:Class="StandardUICommandSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:StandardUICommandSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="60"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                StandardUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the StandardUICommand class to 
                share a platform command and consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a standard delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1" Padding="10">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True" 
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered" 
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer" >
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem" 
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left" 
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**CodeBehind**

1. Zunächst definieren wir eine `ListItemData`-Klasse, die eine Textzeichenfolge und eine ICommand-Schnittstelle für jedes ListViewItem in unserer ListView enthält.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. In der MainPage-Klasse definieren wir eine Sammlung von `ListItemData`-Objekten für die [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) der [ListView](/uwp/api/windows.ui.xaml.controls.listview)-[ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Anschließend füllen wir sie mit einer anfänglichen Sammlung von fünf Elementen (mit Text und zugeordnetem [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) „Löschen“) auf.

```csharp
/// <summary>
/// ListView item collection.
/// </summary>
ObservableCollection<ListItemData> collection = 
    new ObservableCollection<ListItemData>();

/// <summary>
/// Handler for the layout Grid control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    // Create the standard Delete command.
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(
            new ListItemData {
                Text = "List item " + i.ToString(),
                Command = deleteCommand });
    }
}

/// <summary>
/// Handler for the ListView control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    // Populate the ListView with the item collection.
    listView.ItemsSource = collection;
}
```

3. Anschließend definieren wir den ICommand ExecuteRequested-Handler, in dem wir den Befehl zum Löschen von Elementen implementieren.

``` csharp
/// <summary>
/// Handler for the Delete command.
/// </summary>
/// <param name="sender">Source of the command event</param>
/// <param name="e">Event args for the command event</param>
private void DeleteCommand_ExecuteRequested(
    XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    // If possible, remove specfied item from collection.
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. Schließlich definieren wir Handler für verschiedene ListView-Ereignisse, einschließlich der Ereignisse [PointerEntered](/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](/uwp/api/windows.ui.xaml.uielement.pointerexited) und [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Mit den Ereignishandler für Zeiger wird die Schaltfläche „Löschen“ für jedes Element ein- bzw. ausgeblendet.

```csharp
/// <summary>
/// Handler for the ListView selection changed event.
/// </summary>
/// <param name="sender">Source of the selection changed event</param>
/// <param name="e">Event args for the selection changed event</param>
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

/// <summary>
/// Handler for the pointer entered event.
/// Displays the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer entered event</param>
/// <param name="e">Event args for the pointer entered event</param>
private void ListViewSwipeContainer_PointerEntered(
    object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Mouse || 
        e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(
            sender as Control, "HoverButtonsShown", true);
    }
}

/// <summary>
/// Handler for the pointer exited event.
/// Hides the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer exited event</param>
/// <param name="e">Event args for the pointer exited event</param>

private void ListViewSwipeContainer_PointerExited(
    object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(
        sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-xamluicommand-class"></a>Befehle, die die XamlUICommand-Klasse verwenden

Wenn Sie einen Befehl erstellen müssen, der nicht von der [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)-Klasse definiert wird oder Sie umfassendere Kontrolle über die Darstellung von Befehlen benötigen, wird die [XamlUiCommand](/uwp/api/windows.ui.xaml.input.xamluicommand)-Klasse von der [ICommand](/uwp/api/windows.ui.xaml.input.icommand)-Schnittstelle abgeleitet, wodurch diverse UI-Eigenschaften (wie ein Symbol, eine Bezeichnung, eine Beschreibung und Tastenkombinationen), Methoden und Ereignisse hinzugefügt werden, mit denen die UI und das Verhalten eines benutzerdefinierten Befehls schnell definiert werden können.

Mit [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) können Sie UI über die Steuerelementbindung angeben, z. B. ein Symbol, eine Bezeichnung, eine Beschreibung und Tastenkombinationen (sowohl Zugriffstaste als auch Tastaturkürzel), ohne die einzelnen Eigenschaften festzulegen.

### <a name="example"></a>Beispiel

![Beispiel für XamlUICommand](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

| Code für dieses Beispiel herunterladen |
| -------------------- |
| [Beispiel für UWP-Befehle (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip) |

In diesem Beispiel wird ebenfalls die Funktion „Löschen“ aus dem vorherigen [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)-Beispiel verwendet; es wird jedoch veranschaulicht, wie Sie mit der [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand)-Klasse einen benutzerdefinierten Befehl zum Löschen mit eigenen Werten für Schriftartensymbol, Bezeichnung, Tastaturkürzel und Beschreibung definieren. Wie im [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand)-Beispiel erweitern wir eine grundlegende [ListView](listview-and-gridview.md) mit einem Befehl „Element löschen“, der über die [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand)-Klasse implementiert wird, wobei die Benutzererfahrung für eine Reihe von Eingabetypen mit einer [MenuBar](menus.md), einem [Swipe](swipe.md)-Steuerelement, Hoverschaltflächen und einem [Kontextmenü](menus.md) optimiert wird.

Viele Plattformsteuerelemente verwenden die XamlUICommand-Eigenschaften im Hintergrund, wie auch unser StandardUICommand-Beispiel im vorherigen Abschnitt. 

> [!NOTE]
> Dieses Beispiel erfordert das Nuget-Paket „Microsoft.UI.Xaml.Controls“, das Teil der [Microsoft Windows-UI-Bibliothek](/uwp/toolkits/winui/) ist.

**XAML:**

Die Beispiel-UI enthält eine [ListView](/uwp/api/windows.ui.xaml.controls.listview) mit fünf Elementen. Der benutzerdefinierte [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) „Löschen“ wird an ein [MenuBarItem](/uwp/api/microsoft.ui.xaml.controls.menubaritem), ein [SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem), einen [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton) und ein [ContextFlyout-Menü](/uwp/api/windows.ui.xaml.uielement.contextflyout) gebunden.

``` xaml
<Page
    x:Class="XamlUICommand_Sample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:XamlUICommand_Sample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <XamlUICommand x:Name="CustomXamlUICommand" 
                       ExecuteRequested="DeleteCommand_ExecuteRequested"
                       Description="Custom XamlUICommand" 
                       Label="Custom XamlUICommand">
            <XamlUICommand.IconSource>
                <FontIconSource FontFamily="Wingdings" Glyph="&#x4D;"/>
            </XamlUICommand.IconSource>
            <XamlUICommand.KeyboardAccelerators>
                <KeyboardAccelerator Key="D" Modifiers="Control"/>
            </XamlUICommand.KeyboardAccelerators>
        </XamlUICommand>

        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="70"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
        
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded" Name="MainGrid">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                XamlUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the XamlUICommand class to 
                share a custom command with consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a custom delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" 
                                Command="{StaticResource CustomXamlUICommand}"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True"
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered"
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer">
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem"
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left"       
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**CodeBehind**

1. Zunächst definieren wir eine `ListItemData`-Klasse, die eine Textzeichenfolge und eine ICommand-Schnittstelle für jedes ListViewItem in unserer ListView enthält.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. In der MainPage-Klasse definieren wir eine Sammlung von `ListItemData`-Objekten für die [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) der [ListView](/uwp/api/windows.ui.xaml.controls.listview)-[ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Anschließend füllen wir sie mit einer anfänglichen Sammlung von fünf Elementen auf (mit Text und dem zugehörigen [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand)).

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(
           new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. Anschließend definieren wir den ICommand ExecuteRequested-Handler, in dem wir den Befehl zum Löschen von Elementen implementieren.

``` csharp
private void DeleteCommand_ExecuteRequested(
   XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. Schließlich definieren wir Handler für verschiedene ListView-Ereignisse, einschließlich der Ereignisse [PointerEntered](/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](/uwp/api/windows.ui.xaml.uielement.pointerexited) und [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Mit den Ereignishandler für Zeiger wird die Schaltfläche „Löschen“ für jedes Element ein- bzw. ausgeblendet.

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Mouse || 
        e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-icommand-interface"></a>Befehle, die die ICommand-Schnittstelle verwenden

UWP-Standardsteuerelemente (Schaltfläche, Liste, Auswahl, Kalender, Textvorhersage) stellen die Grundlage für viele gängige Befehle dar. Eine komplette Liste der Steuerelementtypen finden Sie unter [Steuerelemente und Muster für Windows-Apps](index.md).

Am einfachsten lässt sich eine strukturierte Befehlserfahrung durch Definieren einer Implementierung der ICommand-Schnittstelle einrichten ([Windows.UI.Xaml.Input.ICommand](/uwp/api/windows.ui.xaml.input.icommand) für C++ bzw. [System.Windows.Input.ICommand](/dotnet/api/system.windows.input.icommand) für C#).  Diese ICommand-Instanz kann dann an Steuerelemente (z. B. Schaltflächen) gebunden werden.

> [!NOTE]
> In einigen Fällen ist es genau so effizient, eine Methode an das Click-Ereignis und eine Eigenschaft an die IsEnabled-Eigenschaft zu binden.

#### <a name="example"></a>Beispiel

![Beispiel für Befehlsschnittstelle](images/commanding/icommand.gif)

*ICommand-Beispiel*

| Code für dieses Beispiel herunterladen |
| -------------------- |
| [Beispiel für UWP-Befehle (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip) |

In diesem einfachen Beispiel wird veranschaulicht, wie ein einzelner Befehl mit einem Klick auf eine Schaltfläche, dem Eingeben eines Tastaturkürzels und durch Drehen des Mausrads aufgerufen werden kann.

Wir verwenden zwei [ListViews](/uwp/api/windows.ui.xaml.controls.listview), von denen eine mit fünf Elementen aufgefüllt und die andere leer ist, sowie zwei Schaltflächen, bei denen mit einer Elemente aus der ListView links in die ListView rechts und mit der anderen Elemente von rechts nach links verschoben werden. Jede Schaltfläche ist an einen entsprechenden Befehl gebunden (ViewModel.MoveRightCommand bzw. ViewModel.MoveLeftCommand), und sie wird entsprechend der Anzahl der Elemente in der zugehörigen ListView automatisch aktiviert und deaktiviert.

**Im folgenden XAML-Code wird die UI für das Beispiel definiert.**

```xaml
<Page
    x:Class="UICommand1.View.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:vm="using:UICommand1.ViewModel"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <vm:OpacityConverter x:Key="opaque" />
    </Page.Resources>

    <Grid Name="ItemGrid"
          Background="AliceBlue"
          PointerWheelChanged="Page_PointerWheelChanged">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <ListView Grid.Column="0" VerticalAlignment="Center"
                  x:Name="CommandListView" 
                  ItemsSource="{x:Bind Path=ViewModel.ListItemLeft}" 
                  SelectionMode="None" IsItemClickEnabled="False" 
                  HorizontalAlignment="Right">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <Grid Grid.Column="1" Margin="0,0,0,0"
              HorizontalAlignment="Center" 
              VerticalAlignment="Center">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel Grid.Row="1">
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE893;" 
                          Opacity="{x:Bind Path=ViewModel.ListItemLeft.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Add" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Next"/>
                        <TextBlock>Move item right</TextBlock>
                    </StackPanel>
                </Button>
                <Button Name="MoveItemLeftButton" 
                            Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Subtract" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Previous"/>
                        <TextBlock>Move item left</TextBlock>
                    </StackPanel>
                </Button>
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE892;"
                          Opacity="{x:Bind Path=ViewModel.ListItemRight.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
            </StackPanel>
        </Grid>
        <ListView Grid.Column="2" 
                  x:Name="CommandListViewRight" 
                  VerticalAlignment="Center" 
                  IsItemClickEnabled="False" 
                  SelectionMode="None"
                  ItemsSource="{x:Bind Path=ViewModel.ListItemRight}" 
                  HorizontalAlignment="Left">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Dies ist der CodeBehind für die obige UI.**

Im CodeBehind wird eine Verknüpfung mit dem Anzeigemodell hergestellt, das den Befehlscode enthält. Darüber hinaus definieren wir einen Handler für die Eingabe über das Mausrad, der ebenfalls mit dem Befehlscode verknüpft ist.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Controls;
using UICommand1.ViewModel;
using Windows.System;
using Windows.UI.Core;

namespace UICommand1.View
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Reference to our view model.
        public UICommand1ViewModel ViewModel { get; set; }

        // Initialize our view and view model.
        public MainPage()
        {
            this.InitializeComponent();
            ViewModel = new UICommand1ViewModel();
        }

        /// <summary>
        /// Handle mouse wheel input and assign our
        /// commands to appropriate direction of rotation.
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void Page_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            var props = e.GetCurrentPoint(sender as UIElement).Properties;

            // Require CTRL key and accept only vertical mouse wheel movement 
            // to eliminate accidental wheel input.
            if ((Window.Current.CoreWindow.GetKeyState(VirtualKey.Control) != 
                CoreVirtualKeyStates.None) && !props.IsHorizontalMouseWheel)
            {
                bool delta = props.MouseWheelDelta < 0 ? true : false;

                switch (delta)
                {
                    case true:
                        ViewModel.MoveRight();
                        break;
                    case false:
                        ViewModel.MoveLeft();
                        break;
                    default:
                        break;
                }
            }
        }
    }
}
```

**Dies ist der Code aus dem Anzeigemodell.**

In unserem Anzeigemodell definieren wir die Ausführungsdetails für die beiden Befehle in der App, füllen eine ListView auf und stellen einen Deckkraft-Wertkonverter bereit, mit dem einige zusätzliche UI-Elemente entsprechend der Anzahl der Elemente in der jeweiligen ListView ausgeblendet bzw. angezeigt werden.

```csharp
using System;
using System.Collections.ObjectModel;
using System.ComponentModel;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Data;

namespace UICommand1.ViewModel
{
    /// <summary>
    /// UI properties for our list items.
    /// </summary>
    public class ListItemData
    {
        /// <summary>
        /// Gets and sets the list item content string.
        /// </summary>
        public string ListItemText { get; set; }
        /// <summary>
        /// Gets and sets the list item icon.
        /// </summary>
        public Symbol ListItemIcon { get; set; }
    }

    /// <summary>
    /// View Model that sets up a command to handle invoking the move item buttons.
    /// </summary>
    public class UICommand1ViewModel
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand MoveLeftCommand { get; private set; }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand MoveRightCommand { get; private set; }

        // Item collections
        public ObservableCollection<ListItemData> ListItemLeft { get; } = 
           new ObservableCollection<ListItemData>();
        public ObservableCollection<ListItemData> ListItemRight { get; } = 
           new ObservableCollection<ListItemData>();

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            MoveLeftCommand = 
               new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            MoveRightCommand = 
               new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            LoadItems();
        }

        /// <summary>
        ///  Populate our list of items.
        /// </summary>
        public void LoadItems()
        {
            for (var x = 0; x <= 4; x++)
            {
                listItem = new ListItemData();
                listItem.ListItemText = "Item " + (ListItemLeft.Count + 1).ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.Add(listItem);
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return ListItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return ListItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (ListItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                ListItemRight.Add(listItem);
                listItem.ListItemText = "Item " + ListItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.RemoveAt(ListItemLeft.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (ListItemRight.Count > 0)
            {
                listItem = new ListItemData();
                ListItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + ListItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemRight.RemoveAt(ListItemRight.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// Views subscribe to this event to get notified of property updates.
        /// </summary>
        public event PropertyChangedEventHandler PropertyChanged;

        /// <summary>
        /// Notify subscribers of updates to the named property
        /// </summary>
        /// <param name="propertyName">The full, case-sensitive, name of a property.</param>
        protected void NotifyPropertyChanged(string propertyName)
        {
            PropertyChangedEventHandler handler = this.PropertyChanged;
            if (handler != null)
            {
                PropertyChangedEventArgs args = new PropertyChangedEventArgs(propertyName);
                handler(this, args);
            }
        }
    }

    /// <summary>
    /// Convert a collection count to an opacity value of 0.0 or 1.0.
    /// </summary>
    public class OpacityConverter : IValueConverter
    {
        /// <summary>
        /// Converts a collection count to an opacity value of 0.0 or 1.0.
        /// </summary>
        /// <param name="value">The count passed in</param>
        /// <param name="targetType">Ignored.</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns>1.0 if count > 0, otherwise returns 0.0</returns>
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            return ((int)value > 0 ? 1.0 : 0.0);
        }

        /// <summary>
        /// Not used, converter is not intended for two-way binding. 
        /// </summary>
        /// <param name="value">Ignored</param>
        /// <param name="targetType">Ignored</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns></returns>
        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

**Und dies ist schließlich die Implementierung der ICommand-Schnittstelle.**

Hier definieren wir einen Befehl, der die [ICommand](/uwp/api/windows.ui.xaml.input.icommand)-Schnittstelle implementiert und ihre Funktionen einfach an andere Objekte weiterleitet.

```csharp
using System;
using System.Windows.Input;

namespace UICommand1
{
    /// <summary>
    /// A command whose sole purpose is to relay its functionality 
    /// to other objects by invoking delegates. 
    /// The default return value for the CanExecute method is 'true'.
    /// <see cref="RaiseCanExecuteChanged"/> needs to be called whenever
    /// <see cref="CanExecute"/> is expected to return a different value.
    /// </summary>
    public class RelayCommand : ICommand
    {
        private readonly Action _execute;
        private readonly Func<bool> _canExecute;

        /// <summary>
        /// Raised when RaiseCanExecuteChanged is called.
        /// </summary>
        public event EventHandler CanExecuteChanged;

        /// <summary>
        /// Creates a new command that can always execute.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        public RelayCommand(Action execute)
            : this(execute, null)
        {
        }

        /// <summary>
        /// Creates a new command.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        /// <param name="canExecute">The execution status logic.</param>
        public RelayCommand(Action execute, Func<bool> canExecute)
        {
            if (execute == null)
                throw new ArgumentNullException("execute");
            _execute = execute;
            _canExecute = canExecute;
        }

        /// <summary>
        /// Determines whether this <see cref="RelayCommand"/> can execute in its current state.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require 
        /// data to be passed, this object can be set to null.
        /// </param>
        /// <returns>true if this command can be executed; otherwise, false.</returns>
        public bool CanExecute(object parameter)
        {
            return _canExecute == null ? true : _canExecute();
        }

        /// <summary>
        /// Executes the <see cref="RelayCommand"/> on the current command target.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require 
        /// data to be passed, this object can be set to null.
        /// </param>
        public void Execute(object parameter)
        {
            _execute();
        }

        /// <summary>
        /// Method used to raise the <see cref="CanExecuteChanged"/> event
        /// to indicate that the return value of the <see cref="CanExecute"/>
        /// method has changed.
        /// </summary>
        public void RaiseCanExecuteChanged()
        {
            var handler = CanExecuteChanged;
            if (handler != null)
            {
                handler(this, EventArgs.Empty);
            }
        }
    }
}
```

## <a name="summary"></a>Zusammenfassung

Die Universelle Windows-Plattform stellt ein stabiles und flexibles Befehlssystem dar, mit dem Sie Apps erstellen können, in denen Befehle für unterschiedliche Steuerelementtypen, Geräte und Eingabetypen gemeinsam genutzt und verwaltet werden.

Verfolgen Sie beim Erstellen von Befehlen für Ihre Windows-Apps folgende Ansätze:

- Überwachen und Behandeln von Ereignissen in XAML/CodeBehind
- Binden an eine Ereignisbehandlungsmethode wie Click
- Definieren einer eigenen ICommand-Implementierung
- Erstellen von XamlUICommand-Objekten mit eigenen Werten für einen vordefinierten Satz von Eigenschaften
- Erstellen von StandardUICommand-Objekten mit einem Satz von vordefinierten Plattformeigenschaften und -werten

## <a name="next-steps"></a>Nächste Schritte

Ein komplettes Beispiel, das eine Implementierung von [XamlUICommand](/uwp/api/windows.ui.xaml.input.xamluicommand) und [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) veranschaulicht, finden Sie im Beispiel zum [XAML-Steuerelementekatalog](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics).

## <a name="see-also"></a>Siehe auch

[Steuerelemente und Muster für Windows-Apps](index.md)

### <a name="samples"></a>Beispiele

#### <a name="topic-samples"></a>Themenbeispiele

- [Beispiel für UWP-Befehle (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip)
- [Beispiel für UWP-Befehle (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip)
- [Beispiel für UWP-Befehle (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip)

#### <a name="other-samples"></a>Andere Beispiele

- [Universelle Windows-Plattform – Beispiele (C# und C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)
- [XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery)
