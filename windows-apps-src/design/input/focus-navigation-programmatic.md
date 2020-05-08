---
Description: Erfahren Sie, wie Sie die Fokus Navigation mit Tastatur-, Gamepad-und Barrierefreiheits Tools in einer Windows-app Programm gesteuert verwalten.
title: Programmgesteuerte Fokus Navigation mit Tools für Tastatur, Gamepad und Barrierefreiheit
label: Programmatic focus navigation
keywords: Tastatur, Spiele Controller, Remote Steuerung, Navigation, Navigations Strategie, Eingabe, Benutzerinteraktion, Barrierefreiheit, Nutzbarkeit
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6b66363588ddad01b05ccc9cc6b3b7912fa21594
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970145"
---
# <a name="programmatic-focus-navigation"></a>Programmgesteuerte Fokusnavigation

![Tastatur, Remote und D-Pad](images/dpad-remote/dpad-remote-keyboard.png)

Wenn Sie den Fokus in der Windows-Anwendung Programm gesteuert verschieben möchten, können Sie entweder die [Focus Manager. trymovefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) -Methode oder die [findnextelements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) -Methode verwenden.

[Tryfovefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) versucht, den Fokus vom Element mit dem Fokus auf das nächste Fokussier Bare Element in der angegebenen Richtung zu ändern, während [findnextelement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) das Element (als [DependencyObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject)) abruft, das den Fokus auf der Grundlage der angegebenen Navigationsrichtung empfängt (nur direktionale Navigation), kann nicht zum Emulieren der Registerkarten Navigation verwendet werden.

> [!NOTE]
> Wir empfehlen die Verwendung der [findnextelement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) -Methode anstelle von [findnextfocus-Element](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) , da findnextfoctelableelement ein UIElement-Element abruft, das NULL zurückgibt, wenn das nächste Fokus verwendbare Element kein UIElement (z. b. ein Hyperlink-Objekt) ist. 

## <a name="find-a-focus-candidate-within-a-scope"></a>Suchen eines Fokus Kandidaten innerhalb eines Bereichs

Sie können das Fokus Navigationsverhalten sowohl für [trymuvefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) als auch für [findnextelements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)anpassen, indem Sie in einer bestimmten UI-Struktur nach dem nächsten Fokus Kandidaten suchen oder bestimmte Elemente nicht berücksichtigen.

In diesem Beispiel wird ein tictactoe-Spiel verwendet, um die Methoden [trymuvefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) und [findnextelements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) zu veranschaulichen.

```xaml
<StackPanel Orientation="Horizontal"
                VerticalAlignment="Center"
                HorizontalAlignment="Center" >
    <Button Content="Start Game" />
    <Button Content="Undo Movement" />
    <Grid x:Name="TicTacToeGrid" KeyDown="OnKeyDown">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="0" 
            x:Name="Cell00" />
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="0" 
            x:Name="Cell10"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="0" 
            x:Name="Cell20"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="1" 
            x:Name="Cell01"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="1" 
            x:Name="Cell11"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="1" 
            x:Name="Cell21"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="2" 
            x:Name="Cell02"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="2" 
            x:Name="Cell22"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="2" 
            x:Name="Cell32"/>
    </Grid>
</StackPanel>
```

```csharp
private void OnKeyDown(object sender, KeyRoutedEventArgs e)
{
    DependencyObject candidate = null;

    var options = new FindNextElementOptions ()
    {
        SearchRoot = TicTacToeGrid,
        XYFocusNavigationStrategyOverride = XYFocusNavigationStrategyOverride.Projection
    };

    switch (e.Key)
    {
        case Windows.System.VirtualKey.Up:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Up, options);
            break;
        case Windows.System.VirtualKey.Down:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Down, options);
            break;
        case Windows.System.VirtualKey.Left:
            candidate = FocusManager.FindNextElement(
                FocusNavigationDirection.Left, options);
            break;
        case Windows.System.VirtualKey.Right:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Right, options);
            break;
    }
    // Also consider whether candidate is a Hyperlink, WebView, or TextBlock.
    if (candidate != null && candidate is Control)
    {
        (candidate as Control).Focus(FocusState.Keyboard);
    }
}
```

Verwenden Sie [findnextelementoptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions) , um die Identifizierung von Fokus Kandidaten weiter anzupassen. Dieses Objekt stellt die folgenden Eigenschaften bereit:

- [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) -Bereich der Suche nach Fokus Navigations Kandidaten für die untergeordneten Elemente dieses DependencyObject. NULL zeigt an, dass die Suche aus dem Stamm der visuellen Struktur gestartet werden soll.

> [!Important] 
> Wenn eine oder mehrere Transformationen auf die Nachfolger von **SearchRoot** angewendet werden, die Sie außerhalb des direktionalen Bereichs platzieren, werden diese Elemente weiterhin als Kandidaten angesehen.

- [Exclusionrect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) -Fokus Navigations Kandidaten werden mithilfe eines "fiktiven" umgebenden Rechtecks identifiziert, bei dem alle überlappenden Objekte aus dem Navigations Fokus ausgeschlossen werden. Dieses Rechteck wird nur für Berechnungen verwendet und nie der visuellen Struktur hinzugefügt.
- [Hintrect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) -Fokus Navigations Kandidaten werden mithilfe eines "fiktiven" umgebenden Rechtecks identifiziert, das die Elemente identifiziert, die den Fokus am wahrscheinlichsten erhalten. Dieses Rechteck wird nur für Berechnungen verwendet und nie der visuellen Struktur hinzugefügt.
- [Xyfocus navigationstrategyoverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride) : die Fokus Navigations Strategie, mit der das beste Kandidaten Element identifiziert wird, das den Fokus erhält.

In der folgenden Abbildung werden einige dieser Konzepte veranschaulicht. 

Wenn Element B den Fokus besitzt, identifiziert findnextelement I als Fokus Kandidat bei der Navigation nach rechts. Die Gründe hierfür sind:
- Aufgrund des [hintrect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) in einer ist der Start Verweis a und nicht B.
- C ist kein Kandidat, da myPanel als [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) angegeben wurde.
- F ist kein Kandidat, da der [exclusionrect Sie überlappt](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) .

![Benutzerdefiniertes Fokus Navigationsverhalten mithilfe von Navigations hinweisen](images/keyboard/navigation-hints.png)

*Benutzerdefiniertes Fokus Navigationsverhalten mithilfe von Navigations hinweisen*

## <a name="navigation-focus-events"></a>Navigations Fokus Ereignisse

### <a name="nofocuscandidatefound-event"></a>Nofocus candidatefound-Ereignis

Das [UIElement. nofocdingcandidatefound](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) -Ereignis wird ausgelöst, wenn die Tab-Taste oder die Pfeiltaste gedrückt werden und kein Fokus Kandidat in der angegebenen Richtung vorhanden ist. Dieses Ereignis wird nicht für [trymuvefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)ausgelöst.

Da es sich hierbei um ein Routing Ereignis handelt, wird es vom Fokus Element nach oben durch aufeinander folgende übergeordnete Objekte in den Stamm der Objektstruktur hochskalieren. Auf diese Weise können Sie das Ereignis nach Bedarf behandeln.

<a name="split-view-code-sample"></a>

Hier wird gezeigt, wie ein Raster eine [SPLITVIEW](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.splitview) öffnet, wenn der Benutzer versucht, den Fokus auf die linke Seite des Fokus baren Steuer Elements zu verschieben (siehe [Entwerfen für Xbox und TV](../devices/designing-for-tv.md#navigation-pane)).

```xaml
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">
...
</Grid>
```

```csharp
private void OnNoFocusCandidateFound (
    UIElement sender, NoFocusCandidateFoundEventArgs args)
{
    if(args.NavigationDirection == FocusNavigationDirection.Left)
    {
        if(args.InputDevice == FocusInputDeviceKind.Keyboard ||
        args.InputDevice == FocusInputDeviceKind.GameController )
            {
                OpenSplitPaneView();
            }
        args.Handled = true;
    }
}
```

### <a name="gotfocus-and-lostfocus-events"></a>Ereignisse von "GotFocus" und "LostFocus"
Die [UIElement. GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) -und [UIElement. LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) -Ereignisse werden ausgelöst, wenn ein Element den Fokus erhält oder den Fokus verliert. Dieses Ereignis wird nicht für [trymuvefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)ausgelöst.

Da es sich hierbei um Routing Ereignisse handelt, werden Sie vom Fokus Element nach oben durch nachfolgende übergeordnete Objekte in den Stamm der Objektstruktur hochskalieren. Auf diese Weise können Sie das Ereignis nach Bedarf behandeln.

### <a name="gettingfocus-and-losingfocus-events"></a>Gettingfocus-und losingfocus-Ereignisse

Die [UIElement. gettingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) -und [UIElement. losingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) -Ereignisse werden vor den entsprechenden [UIElement. GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) -und [UIElement. LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) -Ereignissen ausgelöst. 

Da es sich hierbei um Routing Ereignisse handelt, werden Sie vom Fokus Element nach oben durch nachfolgende übergeordnete Objekte in den Stamm der Objektstruktur hochskalieren. Wenn dies geschieht, bevor eine schwerpunktänderung stattfindet, können Sie die Fokus Änderung umleiten oder Abbrechen.

[Gettingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) und [losingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) sind synchrone Ereignisse, sodass der Fokus nicht verschoben wird, während diese Ereignisse blindeln. Allerdings sind " [GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) " und " [LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) " asynchrone Ereignisse, was bedeutet, dass keine Garantie vorhanden ist, dass der Fokus vor der Ausführung des Handlers nicht erneut verschoben wird.

Wenn der Fokus durch einen Aufrufen von [Control. Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)bewegt wird, wird [gettingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) während des Aufrufes ausgelöst, während der- [Aufrufpunkt](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) ausgelöst wird.

Das Fokus Navigations Ziel kann während der [gettingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) -und [losingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) -Ereignisse (vor dem Verschieben des Fokus) durch die [gettingfoculokargs. newfocucements](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement) -Eigenschaft geändert werden. Auch wenn das Ziel geändert wird, wird das Ereignis immer noch Blasen und das Ziel kann erneut geändert werden.

Um Probleme mit dem erneuten eintreten zu vermeiden, wird eine Ausnahme ausgelöst, wenn Sie versuchen, den Fokus zu verschieben (mithilfe von [trymovefocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) oder [Control. Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)), während diese Ereignisse blindeln.

Diese Ereignisse werden unabhängig von der Ursache für den Fokus verschoben (einschließlich Navigation in der Registerkarte, direktionale Navigation und programmgesteuerte Navigation).

Hier ist die Ausführungsreihenfolge für die Fokus Ereignisse:

1.  [Losingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) Wenn der Fokus auf das verlorene Fokus Element zurückgesetzt wird oder [trycancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) erfolgreich ist, werden keine weiteren Ereignisse ausgelöst.
2.  [Gettingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) Wenn der Fokus auf das verlorene Fokus Element zurückgesetzt wird oder [trycancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) erfolgreich ist, werden keine weiteren Ereignisse ausgelöst.
3.  [LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

In der folgenden Abbildung wird gezeigt, wie der xyfocus bei der Umstellung von einer auf die Rechte Seite von "B4" als Kandidat wählt. B4 löst dann das gettingfocus-Ereignis aus, bei dem die ListView die Möglichkeit hat, den Fokus auf B3 neu zuzuweisen.

![Ändern des Fokus Navigations Ziels für das gettingfocus-Ereignis](images/keyboard/focus-events.png)

*Ändern des Fokus Navigations Ziels für das gettingfocus-Ereignis*

Hier wird gezeigt, wie das [gettingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) -Ereignis behandelt und der Fokus umgeleitet wird.

```XAML
<StackPanel Orientation="Horizontal">
    <Button Content="A" />
    <ListView x:Name="MyListView" SelectedIndex="2" GettingFocus="OnGettingFocus">
        <ListViewItem>LV1</ListViewItem>
        <ListViewItem>LV2</ListViewItem>
        <ListViewItem>LV3</ListViewItem>
        <ListViewItem>LV4</ListViewItem>
        <ListViewItem>LV5</ListViewItem>
    </ListView>
</StackPanel>
```

```csharp
private void OnGettingFocus(UIElement sender, GettingFocusEventArgs args)
{
    //Redirect the focus only when the focus comes from outside of the ListView.
    // move the focus to the selected item
    if (MyListView.SelectedIndex != -1 && 
        IsNotAChildOf(MyListView, args.OldFocusedElement))
    {
        var selectedContainer = 
            MyListView.ContainerFromItem(MyListView.SelectedItem);
        if (args.FocusState == 
            FocusState.Keyboard && 
            args.NewFocusedElement != selectedContainer)
        {
            args.TryRedirect(
                MyListView.ContainerFromItem(MyListView.SelectedItem));
            args.Handled = true;
        }
    }
}
```

Hier wird gezeigt, wie das [losingfocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) -Ereignis für eine [Befehlsleiste](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) behandelt und der Fokus festgelegt wird, wenn das Menü geschlossen wird.

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
     <AppBarButton Icon="Back" Label="Back" />
     <AppBarButton Icon="Stop" Label="Stop" />
     <AppBarButton Icon="Play" Label="Play" />
     <AppBarButton Icon="Forward" Label="Forward" />

     <CommandBar.SecondaryCommands>
         <AppBarButton Icon="Like" Label="Like" />
         <AppBarButton Icon="Share" Label="Share" />
     </CommandBar.SecondaryCommands>
 </CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (MyCommandBar.IsOpen == true && 
        IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
    {
        if (args.TryCancel())
        {
            args.Handled = true;
        }
    }
}
```

## <a name="find-the-first-and-last-focusable-element"></a>Suchen des ersten und letzten Fokus nutzbaren Elements

Die [focemanager. findfirstfocfeableelement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) -Methode und die [liegt Manager. findlastfocingableelement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) -Methode bewegen den Fokus auf das erste oder letzte Fokussier Bare Element innerhalb des Gültigkeits Bereichs eines Objekts (die Elementstruktur eines [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) oder die Text Struktur eines [Textelement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement)). Der Bereich wird im-Befehl angegeben (wenn das-Argument NULL ist, ist der Gültigkeitsbereich das aktuelle Fenster).

Wenn im Gültigkeitsbereich keine Fokus Kandidaten identifiziert werden können, wird NULL zurückgegeben.

Hier wird gezeigt, wie angegeben wird, dass die Schaltflächen einer Befehlsleiste über ein umschließtes direktionales Verhalten verfügen (siehe [Tastatur Interaktionen](keyboard-interactions.md#popup-ui)).

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
    <AppBarButton Icon="Back" Label="Back" />
    <AppBarButton Icon="Stop" Label="Stop" />
    <AppBarButton Icon="Play" Label="Play" />
    <AppBarButton Icon="Forward" Label="Forward" />

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" />
        <AppBarButton Icon="ReShare" Label="Share" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (IsNotAChildOf(MyCommandBar, args.NewFocussedElement))
    {
        DependencyObject candidate = null;
        if (args.Direction == FocusNavigationDirection.Left)
        {
            candidate = FocusManager.FindLastFocusableElement(MyCommandBar);
        }
        else if (args.Direction == FocusNavigationDirection.Right)
        {
            candidate = FocusManager.FindFirstFocusableElement(MyCommandBar);
        }
        if (candidate != null)
        {
            args.NewFocusedElement = candidate;
            args.Handled = true;
        }
    }
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [Fokus Navigation für Tools der Tastatur, Gamepad, Remote Steuerung und Barrierefreiheit](focus-navigation.md)
- [Tastatur Interaktionen](keyboard-interactions.md)
- [Barrierefreiheit der Tastaturnavigation](../accessibility/keyboard-accessibility.md)
