---
Description: Erfahren Sie, wie Sie die Fokus Navigation Programm gesteuert mit den Tools Tastatur, Gamepad und Barrierefreiheit in einer UWP-App verwalten.
title: Programmgesteuerte Fokusnavigation mit Tastatur, Gamepad und Bedienungshilfen
label: Programmatic focus navigation
keywords: Tastatur, Gamecontroller, Fernbedienung, Navigation, Navigationsstrategie, Eingabe, Benutzerinteraktion, Bedienungshilfen, Verwendbarkeit
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d919a86a44110d5b3b444fdf47d41f31637ccb6b
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684499"
---
# <a name="programmatic-focus-navigation"></a>Programmgesteuerte Fokusnavigation

![Tastatur, Fernbedienung und Steuerkreuz](images/dpad-remote/dpad-remote-keyboard.png)

Verwenden Sie die [FocusManager.TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_)-Methode oder die [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)-Methode, um den Fokus in Ihrer UWP-Anwendung programmgesteuert zu verlagern.

[TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) versucht, den Fokus von dem Element mit Fokus auf das nächste fokussierbare Element in der angegebenen Richtung zu ändern, während [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) das Element abruft (als [DependencyObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject)), das den Fokus basierend auf der angegebenen Navigationsrichtung erhalten wird (nur direktionale Navigation, kann nicht verwendet werden, um die TAB-Navigation zu emulieren).

> [!NOTE]
> Wir empfehlen die Verwendung der [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)-Methode anstelle von [FindNextFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_), da FindNextFocusableElement ein UIElement abruft, das Null zurückgibt, wenn das nächste fokussierbare Element kein UIElement ist (z. B. ein Hyperlink-Objekt). 

## <a name="find-a-focus-candidate-within-a-scope"></a>Suche nach einem Fokuskandidaten in einem Bereich

Sie können das Verhalten der Fokusnavigation für [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) und [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) anpassen, einschließlich der Suche nach dem nächsten Fokuskandidaten in einer bestimmten UI-Struktur oder dem Ausschließen bestimmter Elemente, sodass diese nicht in Betracht gezogen werden.

In diesem Beispiel wird ein TicTacToe-Spiel verwendet, um die Methoden [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) und [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) zu veranschaulichen.

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

Verwenden Sie [FindNextElementOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions), um weiter anzupassen, wie Fokuskandidaten identifiziert werden. Dieses -Objekt stellt die folgenden Eigenschaften bereit:

- [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot): Legen Sie den Bereich für die Suche nach Fokusnavigationskandidaten auf die untergeordneten Elemente dieses DependencyObject fest. Null gibt an, dass die Suche im Stamm der visuellen Struktur beginnt.

> [!Important] 
> Wenn eine oder mehrere Transformationen auf die Nachfolgerelemente von **SearchRoot** angewendet werden, die sie außerhalb des direktionalen Bereichs platzieren, werden diese Elemente weiterhin als Kandidaten betrachtet.

- [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect): Fokusnavigationskandidaten werden mit einem „fiktiven“ umgebenden Rechteck identifiziert, und dabei werden alle überlappenden Objekte aus dem Navigationsfokus ausgeschlossen. Dieses Rechteck dient nur zur Berechnung und wird nicht der visuellen Struktur hinzugefügt.
- [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect): Fokusnavigationskandidaten werden mit einem „fiktiven“ umgebenden Rechteck identifiziert, das Elemente ermittelt, die am ehesten den Fokus erhalten. Dieses Rechteck dient nur zur Berechnung und wird nicht der visuellen Struktur hinzugefügt.
- [XYFocusNavigationStrategyOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride): Navigationsstrategie, die verwendet wird, um das beste Kandidatenelement zum Verlagern des Fokus zu identifizieren.

In dieser folgenden Abbildung sind einige dieser Konzepte veranschaulicht. 

Wenn der Fokus auf Element B liegt, identifiziert FindNextElement bei der Navigation nach rechts I als Fokuskandidaten. Dafür gibt es folgende Gründe:
- Da [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) bei A liegt, ist die erste Referenz A, nicht B.
- C ist kein Kandidat, da MyPanel als [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) angegeben wurde.
- F ist kein Kandidat, da eine Überlappung mit [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) vorliegt.

![Benutzerdefiniertes Fokusnavigationsverhalten mithilfe von Navigationshinweisen](images/keyboard/navigation-hints.png)

*Benutzerdefiniertes Fokus Navigationsverhalten mithilfe von Navigations hinweisen*

## <a name="navigation-focus-events"></a>Navigationsfokusereignisse

### <a name="nofocuscandidatefound-event"></a>NoFocusCandidateFound-Ereignis

Das Ereignis [UIElement.NoFocusCandidateFound](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) wird ausgelöst, wenn die TAB-Taste oder die Pfeiltasten gedrückt werden und kein Fokuskandidat in der angegebenen Richtung vorhanden ist. Dieses Ereignis wird nicht für [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) ausgelöst.

Da dies ein Routingereignis ist, erfolgt Bubbling vom fokussierten Element über die einzelnen übergeordneten Objekte zum Stamm der Objektstruktur nach oben. So können Sie das Ereignis verarbeiten, sofern angebracht.

<a name="split-view-code-sample"></a>

Das folgende Beispiel zeigt, wie ein Raster die geteilte Ansicht [SplitView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.splitview) öffnet, wenn der Benutzer versucht, den Fokus auf die ganz linke Seite des fokussierbaren Steuerelements zu verlagern (siehe [Entwerfen für Xbox und Fernsehgeräte](../devices/designing-for-tv.md#navigation-pane)).

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

### <a name="gotfocus-and-lostfocus-events"></a>GotFocus- und LostFocus-Ereignisse
Die Ereignisse [UIElement.GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) und [UIElement.LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) werden ausgelöst, wenn ein Element den Fokus erhält bzw. verliert. Dieses Ereignis wird nicht für [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) ausgelöst.

Da es sich hier um Routingereignisse handelt, erfolgt ein Bubbling vom fokussierten Element über die einzelnen übergeordneten Objekte zum Stamm der Objektstruktur nach oben. So können Sie das Ereignis verarbeiten, sofern angebracht.

### <a name="gettingfocus-and-losingfocus-events"></a>GettingFocus- und LosingFocus-Ereignisse

Die Ereignisse [UIElement.GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) und [UIElement.LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) werden vor den entsprechenden [UIElement.GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)- und [UIElement.LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)-Ereignissen ausgelöst. 

Da es sich hier um Routingereignisse handelt, erfolgt ein Bubbling vom fokussierten Element über die einzelnen übergeordneten Objekte zum Stamm der Objektstruktur nach oben. Da dies vor einer Änderung des Fokus stattfindet, können Sie die Fokusänderung umleiten oder abbrechen.

[GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) und [LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) sind synchrone Ereignisse, was heißt, dass der Fokus nicht verlagert wird, während das Bubbling für diese Ereignisse erfolgt. Da [GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) und [LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) asynchrone Ereignisse sind, kann jedoch nicht garantiert werden, dass der Fokus vor der Ausführung des Handlers nicht erneut verlagert wird.

Wenn der Fokus über einen Aufruf an [Control.Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_) verlagert wird, wird [GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) während des Aufrufs ausgelöst, während [GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) nach dem Aufruf ausgelöst wird.

Das Ziel der Fokusnavigation kann während der Ereignisse [GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) und [LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) (vor der Verlagerung des Fokus) über die [GettingFocusEventArgs.NewFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement) geändert werden. Selbst wenn das Ziel geändert wird, erfolgt weiterhin ein Ereignis-Bubbling, und das Ziel kann erneut geändert werden.

Um Eintrittsvarianzprobleme zu vermeiden, wird eine Ausnahme ausgegeben, wenn Sie versuchen, den Fokus zu verlagern (mit [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) oder [Control.Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)), während für diese Ereignisse das Bubbling erfolgt.

Diese Ereignisse werden unabhängig vom Grund der Fokusverlagerung ausgelöst (z. B. TAB-Navigation, direktionale Navigation und programmgesteuerte Navigation).

Fokusereignisse werden in der folgenden Reihenfolge ausgeführt:

1.  [LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) Wenn der Fokus auf das Fokuselement zurückgesetzt wird, das den Fokus verloren hat, oder [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) erfolgreich ist, werden keine weiteren Ereignisse ausgelöst.
2.  [GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) Wenn der Fokus auf das Fokuselement zurückgesetzt wird, das den Fokus verloren hat, oder [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) erfolgreich ist, werden keine weiteren Ereignisse ausgelöst.
3.  [LostFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

Die folgende Abbildung zeigt, wie beim Verschieben nach rechts von A XYFocus B4 als Kandidat auswählt. B4 löst dann das GettingFocus-Ereignis aus, wobei ListView die Möglichkeit hat, den Fokus erneut zu B3 zuzuweisen.

![Ändern des Fokusnavigationsziels bei einem GettingFocus-Ereignis](images/keyboard/focus-events.png)

*Ändern des Fokus Navigations Ziels für das gettingfocus-Ereignis*

Im Folgenden erfahren Sie, wie Sie das [GettingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus)-Ereignis verarbeiten und den Fokus umleiten.

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

Im Folgenden erfahren Sie, wie Sie das [LosingFocus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus)-Ereignis für [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) verarbeiten und den Fokus festlegen, wenn das Menü geschlossen wird.

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

## <a name="find-the-first-and-last-focusable-element"></a>Suchen des ersten und letzten fokussierbaren Elements

Die Methoden [FocusManager.FindFirstFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) und [FocusManager.FindLastFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) ermöglichen das Verlagern des Fokus auf das erste oder letzte fokussierbare Element im Bereich eines Objekts ([UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)-Elementstruktur oder [TextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement)-Textstruktur). Der Bereich wird im Aufruf angegeben (wenn das Argument null ist, ist der Bereich das aktuelle Fenster).

Wenn keine Fokuskandidaten im Bereich identifiziert werden können, wird Null zurückgegeben.

Im Folgenden wird veranschaulicht, wie Sie angeben, dass die Schaltflächen einer CommandBar ein direktionales Verhalten (Umbruch) aufweisen (siehe [Tastaturinteraktionen](keyboard-interactions.md#popup-ui)).

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