---
title: Befehle in universellen Windows-apps-Plattform (UWP)
description: So verwenden Sie die Klassen XamlUICommand und StandardUICommand (zusammen mit der die ICommand-Schnittstelle) freigeben und Verwalten von Befehlen in verschiedenen Steuerelementtypen, unabhängig vom Gerät und Eingabetyp, der verwendet wird.
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 03/11/2019
ms.openlocfilehash: a85a101cd529bf487cbc97b93bb3905f28213c19
ms.sourcegitcommit: 99271798fe53d9768fc52b21366de05268cadcb0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58221416"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>In universellen Windows-Plattform (UWP)-apps, die mit StandardUICommand XamlUICommand und ICommand-Befehle

In diesem Thema beschrieben, Befehle, die in Anwendungen der universellen Windows-Plattform (UWP). Insbesondere die Informationen zur Verwendung erörtert die [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) und [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) (zusammen mit der die ICommand-Schnittstelle) freigeben und Verwalten von Befehlen in verschiedenen Steuerelementtypen, unabhängig davon, Klassen der dem Gerät und dem Eingabetyp verwendet wird.

![Ein Diagramm, die eine häufige Verwendung für einen freigegebenen Befehl darstellt: mehrere UI Flächen mit einer "bevorzugten"-Befehl](images/commanding/generic-commanding.png)

*Freigabe-Befehle für verschiedene Steuerelemente, unabhängig vom Gerät und Eingabetyp*

## <a name="important-apis"></a>Wichtige APIs

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) und [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>Übersicht

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

Befehle können direkt über die UI-Interaktionen wie auf eine Schaltfläche oder Auswählen eines Elements aus einem Kontextmenü aufgerufen werden. Sie können auch indirekt über einem Eingabegerät wie z. B. einer Zugriffstaste, Geste, Spracherkennung oder eine Automatisierung/Eingabehilfe aufgerufen werden. Nach dem aufrufen, kann der Befehl dann von einem Steuerelement (Navigieren im Text in einem Bearbeitungssteuerelement), ein Fenster (Rückwärtsnavigation) oder die Anwendung (Beenden) behandelt werden.

Befehle können ausgeführt werden, auf einem bestimmten Kontext in Ihrer app, wie z. B. Text löschen oder eine Aktion rückgängig, oder kontextfreier, z. B. stummschaltung der Audiodatei oder Anpassen der Helligkeit sein.

Die folgende Abbildung zeigt zwei befehlsschnittstellen (eine [CommandBar](app-bars.md) und eine kontextabhängige [CommandBarFlyout](command-bar-flyout.md)), die gemeinsam die gleichen Befehle nutzen.

![Beispiel-Schnittstelle](images/commanding/command-interface-example.png)

## <a name="command-interactions"></a>Befehlsinteraktionen

Aufgrund der Vielzahl von Geräten, Eingabetypen und Benutzeroberflächen, die beeinflussen können, wie ein Befehl aufgerufen wird, wird empfohlen, Ihre Befehle über wie viele Eingabeereignisse Flächen verfügbar machen, wie möglich. Dazu können gehören, eine Kombination von [Wischen](swipe.md), [Menüleiste](menus.md), [CommandBar](app-bars.md), [CommandBarFlyout](command-bar-flyout.md), und traditionelle [ Kontextmenü](menus.md).

**Verwenden Sie für wichtige Befehle Eingabe-spezifische Accelerators.** Eingabe Accelerators können es sich um einen Benutzer, die Aktionen schneller, je nach das Eingabegerät, das sie verwenden.

Hier sind einige allgemeine Eingabe-Accelerators für verschiedene Eingabetypen:

- **Zeiger** -zeigen Sie Schaltflächen, Maus & öffnen
- **Tastatur** -Shortcuts (Tastenkombinationen und Zugriffstasten)
- **Touch** -Wischen
- **Touch** -Pull zum Aktualisieren von Daten

Berücksichtigen Sie die Eingabe- und Benutzerdatentyp Benutzeroberflächen, die die Funktionalität Ihrer Anwendung universell verfügbar zu machen. Auflistungen (insbesondere Benutzer bearbeitet werden) enthalten z. B. in der Regel eine Vielzahl von bestimmte Befehle, die etwas anders, je nach Eingabe Gerät ausgeführt werden.

Die folgende Tabelle zeigt einige typische Sammlung-Befehle und die Möglichkeiten, um diese Befehle zugänglich machen.

| Befehl          | Eingabeartunabhängig | Beschleuniger für die Mauseingabe | Beschleuniger für die Tastatureingabe | Beschleuniger für die Toucheingabe |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Element löschen      | Kontextmenü   | Hoverschaltfläche      | ENTF-Taste              | Löschen per Wischen   |
| Element kennzeichnen        | Kontextmenü   | Hoverschaltfläche      | STRG+UMSCHALT+G         | Kennzeichnen per Wischen     |
| Daten aktualisieren     | Kontextmenü   | Nicht zutreffend               | F5-Taste               | Aktualisierung durch Ziehen   |
| Element als Favorit speichern | Kontextmenü   | Hoverschaltfläche      | F-Taste, STRG+S            | Als Favorit speichern per Wischen |

**Geben Sie immer ein Kontextmenü** wir empfehlen Ihnen, alle relevanten kontextbezogene Befehle, die in einer herkömmlichen Kontextmenü oder CommandBarFlyout, da beide für alle Typen Eingabe unterstützt werden. Z. B. wenn ein Befehl nur während ein Mauszeiger-Bewegungsereignis verfügbar gemacht wird, kann nicht es auf einem Gerät nur Touch verwendet werden.

## <a name="commands-in-uwp-applications"></a>Befehle in UWP-Anwendungen

Es gibt mehrere Möglichkeiten, die Sie freigeben und Verwalten von Eingabeereignisse Erfahrungen in einer UWP-Anwendung. Sie können Ereignishandler für standard-Interaktionen, wie z. B. auf, im Code-Behind (Dies kann je nach Komplexität der Benutzeroberfläche ziemlich ineffizient sein) definieren, können Sie Ereignislistener für Standardinteraktionen an einen freigegebenen Handler binden oder Sie können das Steuerelement binden des Command-Eigenschaft auf eine ICommand-Implementierung, die die Befehlslogik beschreibt.

Um die reichhaltigen und komplexen Benutzeroberflächen für Befehl Flächen effizient und mit minimalen Codeduplizierung bereitzustellen, es wird empfohlen, den Binden von Funktionen, die in diesem Thema beschriebenen Befehl (standard-Ereignisbehandlung finden Sie in den Themen für einzelnes Ereignis).

Um ein Steuerelement auf eine freigegebene Befehl-Ressource zu binden, die ICommand-Schnittstellen selbst implementieren oder Sie können den Befehl entweder erstellen die [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) Basisklasse oder einen der Befehle Plattform von definiert die [ StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) abgeleitete Klasse.

- Die ICommand-Schnittstelle ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) oder [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)) können Sie vollständig angepasste erstellen wiederverwendbarer Befehle über Ihre app.
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) ebenfalls diese Funktion bietet jedoch vereinfacht die Entwicklung durch die Bereitstellung einer Reihe von integrierten Befehlseigenschaften wie z. B. das Verhalten des Befehls, Tastenkombinationen in Visual Studio (Zugriffsschlüssel und Zugriffstaste), Symbol, Bezeichnung und Beschreibung.
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) Dinge noch vereinfacht, indem es Ihnen ermöglicht Ihnen die Wahl zwischen einer Reihe von Befehlen von standard-Plattform mit vordefinierten Eigenschaften.

> [!Important]
> In UWP-Anwendungen sind Befehle Implementierungen der entweder die [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) (C++) oder die [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) (C#) abhängig von der gewählten-Schnittstelle Sprachen-Framework.

## <a name="command-experiences-using-the-standarduicommand-class"></a>Befehls-Erfahrungen, die mit der StandardUICommand-Klasse

Von abgeleiteten [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (abgeleitet von [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) für C++ oder [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) für C#), die [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) Klasse macht eine Reihe von Befehlen von standard-Plattform mit vordefinierten Eigenschaften wie das Symbol, eine Zugriffstaste und Beschreibung.

Ein [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) bietet eine schnelle und einheitliche Möglichkeit, um häufig verwendete Befehle definieren, wie z. B. `Save` oder `Delete`. Müssen Sie lediglich die ausführen und die CanExecute-Funktionen zu bieten.

### <a name="example"></a>Beispiel

![StandardUICommand-Beispiel](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

| Den Code für dieses Beispiel herunterladen |
| -------------------- |
| [Eingabeereignisse UWP-Beispiel (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip) |

In diesem Beispiel erfahren, wie eine grundlegende Verbesserung [ListView](listview-and-gridview.md) mit gelöschten Element Befehl implementiert, über die [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) -Klasse, bei gleichzeitiger Optimierung der benutzerfreundlichkeit für eine Vielzahl von Eingabetypen Verwenden einer [Menüleiste](menus.md), [Wischen](swipe.md) -Steuerelement, zeigen Sie Schaltflächen, und [Kontextmenü](menus.md).

> [!NOTE]
> Dieses Beispiel erfordert das Microsoft.UI.Xaml.Controls-NuGet-Paket, ein Teil der [Microsoft Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).

**Xaml:**

Das Beispiel, das eine Benutzeroberfläche enthält eine [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) mit fünf Elementen. Der Löschvorgang [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) gebunden ist eine [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), und [ ContextFlyout Menü](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

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

**Code-behind**

1. Zuerst definieren wir eine `ListItemData` -Klasse, die eine Textzeichenfolge und ICommand für jede ListViewItem in unserer ListView enthält.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. In der MainPage-Klasse, die wir definieren eine Sammlung von `ListItemData` von Objekten für die [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) von der [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Wir klicken Sie dann mit füllen eine anfangsauflistung von fünf Elemente (mit Text und die zugehörigen [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) löschen).

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

3. Als Nächstes definieren wir den Handler für ICommand ExecuteRequested, in dem wir den Element-Delete-Befehl implementieren.

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

4. Wir definieren Sie schließlich Handler für verschiedene ListView-Ereignisse, einschließlich [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), und [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) Ereignisse. Die Zeiger-Ereignishandler dienen zum Anzeigen oder Ausblenden der Schaltfläche "löschen" für jedes Element.

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

## <a name="command-experiences-using-the-xamluicommand-class"></a>Befehls-Erfahrungen, die mit der XamlUICommand-Klasse

Wenn Sie benötigen, um einen Befehl zu erstellen, die durch nicht definiert ist der [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) -Klasse, oder Sie möchten mehr Kontrolle über die Darstellung des Befehls, der [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) Klasse leitet sich von der [ ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) -Schnittstelle, Hinzufügen von verschiedenen UI Eigenschaften (z. B. ein Symbol, Beschriftung, Beschreibung und Tastenkombinationen in Visual Studio), Methoden und Ereignisse, um schnell die Benutzeroberfläche und das Verhalten eines benutzerdefinierten Befehls zu definieren.

[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) können Sie die Benutzeroberfläche über das Steuerelement binden, wie z. B. ein Symbol angeben, eine Beschreibung der Bezeichnung, und Tastenkombinationen (Zugriffstaste und einer Zugriffstaste), ohne die einzelnen Eigenschaften festzulegen.

### <a name="example"></a>Beispiel

![XamlUICommand-Beispiel](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

| Den Code für dieses Beispiel herunterladen |
| -------------------- |
| [Eingabeereignisse UWP-Beispiel (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip) |

In diesem Beispiel teilt der Delete-Funktion des vorherigen [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) Beispiel, jedoch zeigt wie die [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) Klasse können Sie definieren einen benutzerdefinierten Delete-Befehl mit Ihren eigenen schriftartensymbol Bezeichnung eine Zugriffstaste und Beschreibung. Wie die [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) beispielsweise optimiert, dass Sie eine einfache [ListView](listview-and-gridview.md) mit gelöschten Element Befehl implementiert, über die [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) -Klasse, bei gleichzeitiger Optimierung der Benutzeroberfläche für eine Vielzahl von Eingabetypen verwenden eine [Menüleiste](menus.md), [Wischen](swipe.md) -Steuerelement, zeigen Sie Schaltflächen, und [Kontextmenü](menus.md).

Viele Plattformsteuerelemente verwenden Sie die XamlUICommand-Eigenschaften im Hintergrund werden wie bei unserem StandardUICommand-Beispiel im vorherigen Abschnitt. 

> [!NOTE]
> Dieses Beispiel erfordert das Microsoft.UI.Xaml.Controls-NuGet-Paket, ein Teil der [Microsoft Windows UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).

**Xaml:**

Das Beispiel, das eine Benutzeroberfläche enthält eine [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) mit fünf Elementen. Die benutzerdefinierte [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) löschen gebunden ist eine [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), und [ ContextFlyout Menü](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

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
        <XamlUICommand x:Name="CustomXamlUICommand" ExecuteRequested="DeleteCommand_ExecuteRequested" Description="Custom XamlUICommand" Label="Custom XamlUICommand">
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
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" Command="{StaticResource CustomXamlUICommand}"/>
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

**Code-behind**

1. Zuerst definieren wir eine `ListItemData` -Klasse, die eine Textzeichenfolge und ICommand für jede ListViewItem in unserer ListView enthält.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. In der MainPage-Klasse, die wir definieren eine Sammlung von `ListItemData` von Objekten für die [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) von der [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Wir klicken Sie dann mit füllen eine anfangsauflistung von fünf Elemente (mit Text und die zugehörigen [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)).

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. Als Nächstes definieren wir den Handler für ICommand ExecuteRequested, in dem wir den Element-Delete-Befehl implementieren.

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
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

4. Wir definieren Sie schließlich Handler für verschiedene ListView-Ereignisse, einschließlich [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), und [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) Ereignisse. Die Zeiger-Ereignishandler dienen zum Anzeigen oder Ausblenden der Schaltfläche "löschen" für jedes Element.

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
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-icommand-interface"></a>Befehls-Erfahrungen, die mit die ICommand-Schnittstelle

Standard-UWP-Steuerelemente ("Schaltfläche", "Liste", "Auswahl", "Kalender", "Textvorhersage") bilden die Grundlage für viele allgemeine Befehl Oberflächen. Eine vollständige Liste der Steuerelementtypen, finden Sie unter [Steuerelemente und Muster für die UWP-apps](index.md).

Die einfachste Möglichkeit, eine strukturierte sind ebenfalls unterstützen wird, um eine Implementierung von ICommand-Schnittstelle zu definieren ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) für C++ oder [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)für C#).  Diese ICommand-Instanz kann dann an Steuerelementen wie Schaltflächen gebunden werden.

> [!NOTE]
> In einigen Fällen kann es nur so effizient ist eine Methode an das Click-Ereignis und eine Eigenschaft, die IsEnabled-Eigenschaft gebunden sein.

#### <a name="example"></a>Beispiel

![Beispiel-Schnittstelle](images/commanding/icommand.gif)

*ICommand-Beispiel*

| Den Code für dieses Beispiel herunterladen |
| -------------------- |
| [Eingabeereignisse UWP-Beispiel (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip) |

In diesem einfachen Beispiel zeigen wir wie ein einzelnen Befehl mit einer Schaltfläche aufgerufen werden, kann auf einer Zugriffstaste und Mausrad drehen.

Wir verwenden zwei [ListViews](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), eine mit fünf Elemente und die andere leere und zwei Schaltflächen, eine für das Verschieben von Elementen aus der ListView auf der linken Seite in der ListView auf der rechten Seite, und die andere für das Verschieben von Elementen, von rechts nach links. Jede Schaltfläche gebunden ist, auf einen entsprechenden Befehl (ViewModel.MoveRightCommand und ViewModel.MoveLeftCommand, bzw.), und aktiviert und deaktiviert automatisch auf Grundlage der Anzahl der Elemente in ihre ListView zugeordnet sind.

**Der folgende XAML-Code wird die Benutzeroberfläche für unser Beispiel definiert.**

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

**Hier ist der Code-Behind für die vorherigen Benutzeroberfläche.**

Im Code-Behind verbinden wir zu unserer Ansichtsmodell, das unsere Befehlscode enthält. Darüber hinaus definieren wir einen Handler für die Eingabe in das Mausrad, das auch unsere Befehlscode verbindet.

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

**Hier ist der Code aus unserem Modell anzeigen**

Unser Modell der Sicht ist, in dem wir die Ausführungsdetails für die beiden Befehle in unserer app definieren, füllen eine ListView und geben Sie einen Wertkonverter Deckkraft für ausblenden oder Anzeigen von einige zusätzliche Elemente der Benutzeroberfläche basierend auf der Anzahl der Elemente für die einzelnen ListView.

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
        public ObservableCollection<ListItemData> ListItemLeft { get; } = new ObservableCollection<ListItemData>();
        public ObservableCollection<ListItemData> ListItemRight { get; } = new ObservableCollection<ListItemData>();

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            MoveLeftCommand = new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            MoveRightCommand = new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

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

**Zum Schluss sieht unsere Implementierung die ICommand-Schnittstelle**

Hier definieren wir einen Befehl, der implementiert die [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) Schnittstelle und relays einfach seine Funktionalität auf andere Objekte.

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
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
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
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
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

Die universelle Windows-Plattform bietet eine stabile und flexible Befehlssystem, mit dem Sie apps erstellen, die freigeben und Verwalten von Befehlen für Steuerelementtypen, Geräte und Eingabetypen.

Verwenden Sie die folgenden Vorgehensweisen aus, wenn Befehle für Ihre UWP-apps zu erstellen:

- Überwachen und Behandeln von Ereignissen in XAML/Code-behind
- Binden Sie an eine Ereignisbehandlungsmethode wie z. B. Klicken Sie auf
- Definieren Sie Ihre eigene ICommand-Implementierung
- Erstellen Sie XamlUICommand Objekte durch Ihre eigenen Werte für einen vordefinierten Satz von Eigenschaften
- Erstellen von StandardUICommand-Objekten mit einem Satz von vordefinierten plattformrollen Eigenschaften und Werte

## <a name="next-steps"></a>Nächste Schritte

Für ein vollständiges Beispiel zur Veranschaulichung eine [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) und [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) -Implementierung finden Sie unter den [XAML-Steuerelementsammlungen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) Beispiel.

## <a name="see-also"></a>Siehe auch

[Steuerelemente und Muster für die UWP-apps](index.md)

### <a name="samples"></a>Proben

#### <a name="topic-samples"></a>Themenbeispiele

- [Eingabeereignisse UWP-Beispiel (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip)
- [Eingabeereignisse UWP-Beispiel (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip)
- [Eingabeereignisse UWP-Beispiel (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip)

#### <a name="other-samples"></a>Andere Beispiele

- [Beispiele für universelle Windows-Plattform (C# und C++)](https://go.microsoft.com/fwlink/?linkid=832713)
- [XAML-Steuerelementsammlungen](https://github.com/Microsoft/Xaml-Controls-Gallery)

<!---Some context for the following links goes here
- [link to next logical step for the customer](global-quickstart-template.md)--->

<!--- Required:
In Overview articles, provide at least one next step and no more than three.
Next steps in overview articles will often link to a quickstart.
Use regular links; do not use a blue box link. What you link to will depend on what is really a next step for the customer.
Do not use a "More info section" or a "Resources section" or a "See also section".
--->
