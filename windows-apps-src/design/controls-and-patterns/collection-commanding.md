---
description: Hier wird beschrieben, wie Sie mithilfe von Kontextbefehlen derartige Aktionen so implementieren können, dass bei allen Eingabearten die jeweils bestmögliche Benutzererfahrung gewährleistet ist.
title: Kontextbefehle
ms.assetid: ''
label: Contextual commanding in collections
template: detail.hbs
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8138256dbde751768e5d9a707ffc1ad23ace7494
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173524"
---
# <a name="contextual-commanding-for-collections-and-lists"></a>Kontextbefehle für Sammlungen und Listen



Viele Apps enthalten Sammlungen von Inhalten in Form von Listen, Rastern und Strukturen, auf die der Benutzer Aktionen anwenden kann. Beispielsweise kann er Elemente löschen, umbenennen, kennzeichnen oder aktualisieren. In diesem Artikel wird beschrieben, wie Sie mithilfe von Kontextbefehlen derartige Aktionen so implementieren können, dass bei allen Eingabearten die jeweils bestmögliche Benutzererfahrung gewährleistet ist.  

> **Wichtige APIs:** [ICommand-Schnittstelle](/uwp/api/Windows.UI.Xaml.Input.ICommand), [UIElement.ContextFlyout-Eigenschaft](/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout), [INotifyPropertyChanged-Schnittstelle](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![Ausführen des Befehls „Als Favorit speichern“ mittels verschiedener Eingabearten](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>Erstellen von Befehlen für alle Eingabearten

Benutzer können zur Interaktion mit Windows-Apps eine [Vielzahl unterschiedlicher Geräte und Eingabearten](../devices/index.md) verwenden. Daher sollte Ihre App Befehle sowohl über von der Eingabeart unabhängige Kontextmenüs als auch über eingabeartspezifische Beschleuniger verfügbar machen. Wenn Sie beide Optionen integrieren, können Benutzer Befehle schnell auf Inhalte anwenden, unabhängig von Eingabeart und Gerätetyp.

In der Tabelle unten sind einige typische Befehle für Sammlungen aufgeführt sowie Möglichkeiten, diese Befehle verfügbar zu machen. 

| Befehl          | Eingabeartunabhängig | Beschleuniger für die Mauseingabe | Tastaturkürzel | Beschleuniger für die Toucheingabe |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Element löschen      | Kontextmenü   | Hoverschaltfläche      | ENTF-TASTE              | Löschen per Wischen   |
| Element kennzeichnen        | Kontextmenü   | Hoverschaltfläche      | STRG+UMSCHALT+G         | Kennzeichnen per Wischen     |
| Daten aktualisieren     | Kontextmenü   | NICHT ZUTREFFEND               | F5-TASTE               | Aktualisierung durch Ziehen   |
| Element als Favorit speichern | Kontextmenü   | Hoverschaltfläche      | F-TASTE, STRG+S            | Als Favorit speichern per Wischen |


* **Grundsätzlich sollten Sie sämtliche Befehle für ein Element im [Kontextmenü](menus.md) des Elements verfügbar machen.** Kontextmenüs sind für den Benutzer bei jeder Eingabeart verfügbar und sollten alle Kontextbefehle enthalten, die er ausführen darf.

* **Für häufig verwendete Befehle empfiehlt sich die Implementierung von Eingabebeschleunigern.** Eingabebeschleuniger basieren auf dem jeweiligen Eingabegerät und erlauben es dem Benutzer, Aktionen schnell auszuführen. Zu den Eingabebeschleunigern gehören:
    - Aktionsausführung per Wischen (Beschleuniger für die Toucheingabe)
    - Datenaktualisierung per Ziehen (Beschleuniger für die Toucheingabe)
    - Tastenkombinationen (Beschleuniger für die Tastatureingabe)
    - Zugriffstasten (Beschleuniger für die Tastatureingabe)
    - Hoverschaltflächen für Maus und Stift (Beschleuniger für die Eingabe per Zeigegerät)

> [!NOTE]
> Benutzer sollten immer auf sämtliche Befehle zugreifen können, ganz gleich, welches Gerät sie verwenden. Wenn Sie die Befehle Ihrer App beispielsweise nur in Form von Hoverschaltflächen für eine beschleunigte Eingabe über Zeigegeräte verfügbar machen, haben Benutzer von Touchsystemen keinen Zugriff auf sie. Implementieren Sie zumindest ein Kontextmenü, in dem alle Befehle verfügbar sind.  

## <a name="example-the-podcastobject-data-model"></a>Beispiel: Beispiel: Datenmodell „PodcastObject“

Zur Verdeutlichung unserer Empfehlungen für die Befehlsimplementierung erstellen wir im Rahmen dieses Artikels eine Liste von Podcasts für eine Podcast-App. Der Beispielcode demonstriert, wie Sie es Benutzern ermöglichen können, bestimmte Podcasts aus der Liste als Favoriten zu speichern.

Hier die Definition des Podcast-Objekts, mit dem Sie in diesem Beispiel arbeiten: 

```csharp
public class PodcastObject : INotifyPropertyChanged
{
    // The title of the podcast
    public String Title { get; set; }

    // The podcast's description
    public String Description { get; set; }

    // Describes if the user has set this podcast as a favorite
    public bool IsFavorite
    {
        get
        {
            return _isFavorite;
        }
        set
        {
            _isFavorite = value;
            OnPropertyChanged("IsFavorite");
        }
    }
    private bool _isFavorite = false;

    public event PropertyChangedEventHandler PropertyChanged;

    private void OnPropertyChanged(String property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }
}
```

Beachten Sie: Das PodcastObject-Objekt implementiert die [INotifyPropertyChanged](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)-Schnittstelle, um reagieren zu können, sobald der Benutzer die IsFavorite-Eigenschaft ändert.

## <a name="defining-commands-with-the-icommand-interface"></a>Definieren von Befehlen mit der ICommand-Schnittstelle

Über die [ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand)-Schnittstelle können Sie Befehle definieren, die für mehrere Eingabearten verfügbar sind. Ein Beispiel: Statt den Code eines Löschbefehls in identischer Form in zwei unterschiedliche Ereignishandler zu schreiben (einmal in einen Handler für das Drücken der ENTF-TASTE und einmal in einen Handler für das Rechtsklicken auf „Löschen“ in einem Kontextmenü), können Sie Ihre Löschlogik einmalig als Schnittstelle des Typs [ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand) implementieren und anschließend für verschiedene Eingabearten verfügbar machen.

Dazu müssen Sie die ICommand-Schnittstelle definieren, die die Aktion „Als Favorit speichern“ darstellt. In diesem Beispiel verwenden Sie die [Execute](/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute)-Methode des Befehls, um einen Podcast als Favorit zu speichern. Der betreffende Podcast wird über den Parameter des Befehls an die Execute-Methode übergeben. Dieser Parameter kann mithilfe der CommandParameter-Eigenschaft gebunden werden.

```csharp
public class FavoriteCommand: ICommand
{
    public event EventHandler CanExecuteChanged;

    public bool CanExecute(object parameter)
    {
        return true;
    }
    public void Execute(object parameter)
    {
        // Perform the logic to "favorite" an item.
        (parameter as PodcastObject).IsFavorite = true;
    }
}
```

Wenn Sie einen Befehl für mehrere Sammlungen und Elemente verwenden möchten, können Sie ihn als Ressource auf der Seite oder in der App speichern.

```xaml
<Application.Resources>
    <local:FavoriteCommand x:Key="favoriteCommand" />
</Application.Resources>
```

Zur Ausführung des Befehls rufen Sie seine [Execute](/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute)-Methode auf.

```csharp
// Favorite the item using the defined command
var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
favoriteCommand.Execute(PodcastObject);
```


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>Erstellen eines UserControl-Steuerelements zur Reaktion auf verschiedene Eingabearten

Wenn Sie eine Liste von Elementen implementieren und jedes dieser Elemente auf verschiedene Eingabearten reagieren können soll, können Sie zur Vereinfachung des Codes jeweils ein [UserControl](/uwp/api/Windows.UI.Xaml.Controls.UserControl)-Steuerelement für die einzelnen Elemente definieren. In diesem Steuerelement wiederum definieren Sie das Kontextmenü und die Ereignishandler des jeweiligen Elements. 

So erstellen Sie ein UserControl-Steuerelement in Visual Studio:
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt. Es wird ein Kontextmenü angezeigt.
2. Klicken Sie auf **Hinzufügen > Neues Element...** <br />Das Dialogfeld **Neues Element hinzufügen** wird angezeigt. 
3. Wählen Sie aus der Liste der Elemente die Option „UserControl“ aus. Geben Sie dem Element einen Namen, und klicken Sie auf **Hinzufügen**. Visual Studio generiert nun einen UserControl-Stub. 

In unserem Podcast-Beispiel werden alle Podcasts in einer Liste angezeigt. In dieser Liste gibt es verschiedene Möglichkeiten, wie der Benutzer einen Podcast als Favorit speichern kann. Konkret kann der Benutzer folgende Aktionen ausführen, um einen Podcast als Favorit zu speichern:
- Aufrufen eines Kontextmenüs
- Drücken einer Tastenkombination
- Anzeigen einer Hoverschaltfläche
- Durchführen einer Wischgeste

Um diese Verhaltensweisen zu kapseln und den Befehl „FavoriteCommand“ anwenden zu können, erstellen Sie nun ein neues [UserControl](/uwp/api/Windows.UI.Xaml.Controls.UserControl)-Steuerelement namens „PodcastUserControl“, das einen Podcast in der Liste darstellt.

PodcastUserControl zeigt die Felder des PodcastObject-Objekts als TextBlock-Objekte an und kann auf verschiedene Benutzerinteraktionen reagieren. Im weiteren Verlauf des Artikels wird das PodcastUserControl-Steuerelement referenziert und erweitert.

**PodcastUserControl.xaml**
```xaml
<UserControl
    x:Class="ContextCommanding.PodcastUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    IsTabStop="True" UseSystemFocusVisuals="True"
    >
    <Grid Margin="12,0,12,0">
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**PodcastUserControl.xaml.cs**
```csharp
public sealed partial class PodcastUserControl : UserControl
{
    public static readonly DependencyProperty PodcastObjectProperty =
        DependencyProperty.Register(
            "PodcastObject",
            typeof(PodcastObject),
            typeof(PodcastUserControl),
            new PropertyMetadata(null));

    public PodcastObject PodcastObject
    {
        get { return (PodcastObject)GetValue(PodcastObjectProperty); }
        set { SetValue(PodcastObjectProperty, value); }
    }

    public PodcastUserControl()
    {
        this.InitializeComponent();

        // TODO: We will add event handlers here.
    }
}
```

Beachten Sie: PodcastUserControl enthält einen Verweis auf PodcastObject in Gestalt von DependencyProperty. So lassen sich Objekte des Typs PodcastObject an das PodcastUserControl-Steuerelement binden.

Nun erstellen Sie einige Objekte des Typs PodcastObject und binden diese Objekte an ein ListView-Element, um eine Podcast-Liste zu erstellen. Die PodcastUserControl-Objekte beschreiben die Visualisierung der PodcastObject-Objekte und werden daher über die ItemTemplate-Eigenschaft von ListView festgelegt.

**MainPage.xaml**
```xaml
<ListView x:Name="ListOfPodcasts"
            ItemsSource="{x:Bind podcasts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:PodcastObject">
            <local:PodcastUserControl PodcastObject="{x:Bind Mode=OneWay}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <!-- The PodcastUserControl will entirely fill the ListView item and handle tabbing within itself. -->
        <Style TargetType="ListViewItem" BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="Padding" Value="0"/>
            <Setter Property="IsTabStop" Value="False"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

## <a name="creating-context-menus"></a>Erstellen von Kontextmenüs

Kontextmenüs zeigen bei Aufruf durch den Benutzer eine Liste von Befehlen oder Optionen an. Sie stellen Kontextbefehle für das ihnen zugeordnete Element bereit und sind in der Regel für sekundäre Aktionen reserviert, die nur für dieses Element verfügbar sind.

![Anzeigen eines Kontextmenüs für ein Element](images/ContextualCommand_RightClick.png)

Benutzer können Kontextmenüs über die folgenden „Kontextaktionen“ aufrufen:

| Eingabe    | Kontextaktion                          |
| -------- | --------------------------------------- |
| Maus    | Rechtsklick                             |
| Tastatur | UMSCHALT+F10, Menütaste                  |
| Touch    | Langes Drücken auf das Element                      |
| Stift      | Drücken der Drucktaste, langes Drücken auf das Element |
| Gamepad  | Menü-Taste                             |

**Da Kontextmenüs über jede Eingabeart geöffnet werden können, sollten sie sämtliche Kontextbefehle enthalten, die für das jeweilige Listenelement verfügbar sind.**

### <a name="contextflyout"></a>ContextFlyout

Über die in der UIElement-Klasse definierte [ContextFlyout-Eigenschaft](/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) können Sie ganz einfach ein Kontextmenü erstellen, das bei allen Eingabearten funktioniert. Über die MenuFlyout-Klasse stellen Sie ein Flyout bereit, das Ihr Kontextmenü darstellt. Sobald der Benutzer eine der oben aufgeführten „Kontextaktionen“ durchführt, wird das dem Element zugeordnete MenuFlyout-Objekt angezeigt.

In diesem Beispiel fügen Sie „PodcastUserControl“ eine ContextFlyout-Eigenschaft hinzu. Das in der ContextFlyout-Eigenschaft definierte MenuFlyout-Objekt enthält ein einziges Element, über das sich Podcasts als Favorit speichern lassen. Beachten Sie: MenuFlyoutItem verwendet den oben definierten favoriteCommand-Befehl, wobei CommandParameter an PodcastObject gebunden ist.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" />
        </MenuFlyout>
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <!-- ... -->
    </Grid>
</UserControl>

```

Sie können auch das [ContextRequested-Ereignis](/uwp/api/Windows.UI.Xaml.UIElement.ContextRequested) verwenden, um auf Kontextaktionen zu reagieren. Das ContextRequested-Ereignis wird nicht ausgelöst, wenn die ContextFlyout-Eigenschaft definiert wurde.

## <a name="creating-input-accelerators"></a>Erstellen von Eingabebeschleunigern

Für jedes Element in einer Sammlung sollte ein Kontextmenü mit allen verfügbaren Kontextbefehlen implementiert sein. Vielleicht möchten Sie jedoch, dass Benutzer eine kleinere Gruppe häufig verwendeter Befehle schnell ausführen können. Beispiel: In einer E-Mail-App gibt es die sekundären Befehle „Antworten“, „Archivieren“, „In Ordner verschieben“, „Kennzeichnen“ und „Löschen“. Am häufigsten verwendet werden jedoch die Befehle „Löschen“ und „Kennzeichnen“. Sobald Sie wissen, welche Befehle am häufigsten verwendet werden, können Sie eingabeartbasierte Beschleuniger implementieren, damit Ihre Benutzer diese Befehle einfacher ausführen können.

In unserer Podcast-App ist der Befehl „Als Favorit speichern“ einer der am häufigsten ausgeführten Befehle.

### <a name="keyboard-accelerators"></a>Tastaturkürzel

#### <a name="shortcuts-and-direct-key-handling"></a>Behandeln von Tastenkombinationen und Einzeltasten

![Aktionsausführung per STRG oder F-TASTE](images/ContextualCommand_Keyboard.png)

Je nach Art des Inhalts können Sie bestimmte Tastenkombinationen für die Ausführung einer Aktion festlegen. In einer E-Mail-App beispielsweise könnten sich ausgewählte E-Mails über die ENTF-TASTE löschen lassen. In einer Podcast-App könnte die Tastenkombination STRG+S oder eine der F-TASTEN einen Podcast als Favorit zur späteren Wiedergabe speichern. Obwohl einige Befehle allgemein bekannte Standardtasten bzw. Standardtastenkombinationen haben (z. B. ENTF zum Löschen), sind die Tasten und Tastenkombinationen anderer Befehle jeweils App-spezifisch oder domänenspezifisch. Verwenden Sie falls möglich allgemein bekannte Tasten und Tastenkombinationen, oder zeigen Sie für den Benutzer eine kurze QuickInfo mit der Taste oder Tastenkombination für den jeweiligen Befehl an.

Über das [KeyDown](/uwp/api/Windows.UI.Xaml.UIElement.KeyDownEvent)-Ereignis kann Ihre App auf einen Tastendruck des Benutzers reagieren. In der Regel gehen die Benutzer davon aus, dass die App beim ersten Tastendruck reagiert, nicht erst, wenn sie die Taste wieder loslassen.

In diesem Beispiel zeigen wir Ihnen, wie Sie den KeyDown-Handler zu PodcastUserControl hinzufügen können, damit ein Podcast als Favorit gespeichert wird, wenn der Benutzer STRG+S oder eine der F-TASTEN drückt. Dabei verwenden Sie denselben Befehl wie zuvor.

**PodcastUserControl.xaml.cs**
```csharp
// Respond to the F and Ctrl+S keys to favorite the focused item.
protected override void OnKeyDown(KeyRoutedEventArgs e)
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    var isCtrlPressed = (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down || (ctrlState & CoreVirtualKeyStates.Locked) == CoreVirtualKeyStates.Locked;

    if (e.Key == Windows.System.VirtualKey.F || (e.Key == Windows.System.VirtualKey.S && isCtrlPressed))
    {
        // Favorite the item using the defined command
        var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
        favoriteCommand.Execute(PodcastObject);
    }
}
```

### <a name="mouse-accelerators"></a>Beschleuniger für die Mauseingabe

![Anzeigen einer Schaltfläche durch Platzieren des Mauszeigers auf einem Element](images/ContextualCommand_HovertoReveal.png)

Benutzer sind vertraut mit per Rechtsklick aufrufbaren Kontextmenüs. Möglicherweise möchten Sie jedoch, dass sie häufig verwendete Befehle mit nur einem einzigen Mausklick ausführen können. Zu diesem Zweck können Sie dem Zeichenbereich Ihres Sammlungselements dedizierte Schaltflächen hinzufügen. Damit Ihre Benutzer schnell mithilfe der Maus Aktionen durchführen können, die Benutzeroberfläche aber dennoch übersichtlich bleibt, können Sie festlegen, dass diese Schaltflächen nur angezeigt werden, wenn der Benutzer den Zeiger auf einem bestimmten Listenelement platziert.

In unserem Beispiel wird der Befehl „Als Favorit speichern“ durch eine Schaltfläche dargestellt, die direkt in PodcastUserControl definiert ist. Beachten Sie: Die Schaltfläche in diesem Beispiel verwendet denselben Befehl wie zuvor, FavoriteCommand. Die Sichtbarkeit der Schaltfläche können Sie über VisualStateManager steuern. Diese Klasse schaltet zwischen den verschiedenen visuellen Zuständen um, sobald der Zeiger auf dem Steuerelement platziert wird oder vom Steuerelement entfernt wird.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <!-- ... -->
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="HoveringStates">
                <VisualState x:Name="HoverButtonsShown">
                    <VisualState.Setters>
                        <Setter Target="hoverArea.Visibility" Value="Visible" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="HoverButtonsHidden" />
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
        <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
            <AppBarButton Icon="OutlineStar" Label="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" VerticalAlignment="Stretch"  />
        </Grid>
    </Grid>
</UserControl>
```

Die Hoverschaltflächen sollten angezeigt bzw. ausgeblendet werden, sobald der Mauszeiger auf dem Element platziert oder von ihm entfernt wird. Zur Reaktion auf Mausereignisse können Sie die Ereignisse [PointerEntered](/uwp/api/Windows.UI.Xaml.UIElement.PointerEnteredEvent) und [PointerExited](/uwp/api/Windows.UI.Xaml.UIElement.PointerExitedEvent) in PodcastUserControl verwenden.

**PodcastUserControl.xaml.cs**
```csharp
protected override void OnPointerEntered(PointerRoutedEventArgs e)
{
    base.OnPointerEntered(e);

    // Only show hover buttons when the user is using mouse or pen.
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(this, "HoverButtonsShown", true);
    }
}

protected override void OnPointerExited(PointerRoutedEventArgs e)
{
    base.OnPointerExited(e);

    VisualStateManager.GoToState(this, "HoverButtonsHidden", true);
}
```

Die im Hoverzustand angezeigten Schaltflächen sind nur bei der Eingabe über Zeigegeräte verfügbar. Da diese Schaltflächen ausschließlich für die Eingabe über Zeigegeräte zur Verfügung stehen, möchten Sie zur Optimierung der Zeigereingabe möglicherweise den Abstand um das jeweilige Schaltflächensymbol minimieren oder vollständig entfernen. Wenn Sie sich dazu entscheiden, muss die Schaltfläche mindestens 20 × 20 Pixel groß sein, damit sie mit Stift und Maus noch gut bedienbar ist.

### <a name="touch-accelerators"></a>Beschleuniger für die Toucheingabe

#### <a name="swipe"></a>Swipe

![Aufrufen eines Befehls per Wischgeste](images/ContextualCommand_Swipe.png)

Wischgestenbasierte Befehle sind ein Beschleuniger für die Toucheingabe. Sie ermöglichen es Benutzern von Touchgeräten, häufig verwendete sekundäre Aktionen per Touchgeste auszuführen. Mithilfe von Wischgesten können Benutzer auf Touchgeräten schnell und natürlich mit Inhalten interagieren. So können sie beispielsweise gängige Aktionen wie „Löschen per Wischen“ oder „Aufrufen per Wischen“ durchführen. Weitere Informationen finden Sie im Artikel zum Thema [Wischgestenbasierte Befehle](swipe.md).

Um das Wischen in Ihrer Sammlung zu integrieren, benötigen Sie zwei Komponenten: SwipeItems zum Hosten der Befehle sowie ein SwipeControl als Wrapper des Elements, der die Interaktion per Wischgesten ermöglicht.

SwipeItems kann als Ressource in PodcastUserControl definiert werden. In diesem Beispiel enthält SwipeItems einen Befehl, über den sich ein Element als Favorit speichern lässt.

```xaml
<UserControl.Resources>
    <SymbolIconSource x:Key="FavoriteIcon" Symbol="Favorite"/>
    <SwipeItems x:Key="RevealOtherCommands" Mode="Reveal">
        <SwipeItem IconSource="{StaticResource FavoriteIcon}" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
    </SwipeItems>
</UserControl.Resources>
```

SwipeControl fungiert als Wrapper für das Element und erlaubt es dem Benutzer, per Wischgesten mit ihm zu interagieren. Beachten Sie, dass SwipeControl in seiner RightItems-Eigenschaft einen Verweis auf SwipeItems enthält. Das Element „Als Favorit speichern“ wird angezeigt, wenn der Benutzer von rechts nach links wischt.

```xaml
<SwipeControl x:Name="swipeContainer" RightItems="{StaticResource RevealOtherCommands}">
   <!-- The visual state groups moved from the Grid to the SwipeControl, since the SwipeControl wraps the Grid. -->
   <VisualStateManager.VisualStateGroups>
       <VisualStateGroup x:Name="HoveringStates">
           <VisualState x:Name="HoverButtonsShown">
               <VisualState.Setters>
                   <Setter Target="hoverArea.Visibility" Value="Visible" />
               </VisualState.Setters>
           </VisualState>
           <VisualState x:Name="HoverButtonsHidden" />
       </VisualStateGroup>
   </VisualStateManager.VisualStateGroups>
   <Grid Margin="12,0,12,0">
       <Grid.ColumnDefinitions>
           <ColumnDefinition Width="*" />
           <ColumnDefinition Width="Auto" />
       </Grid.ColumnDefinitions>
       <StackPanel>
           <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
       </StackPanel>
       <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
           <AppBarButton Icon="OutlineStar" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" LabelPosition="Collapsed" VerticalAlignment="Stretch"  />
       </Grid>
   </Grid>
</SwipeControl>
```

Sobald der Benutzer über das Display wischt, um den Befehl „Als Favorit speichern“ aufzurufen, wird die Invoked-Methode aufgerufen.

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>Aktualisierung durch Ziehen

Mithilfe der Aktion „Aktualisieren durch Ziehen“ können Benutzer eine Datensammlung per Touchgeste nach unten ziehen, um weitere Daten abzurufen. Weitere Informationen finden Sie im Artikel zum Thema [Aktualisieren durch Ziehen](pull-to-refresh.md).

### <a name="pen-accelerators"></a>Beschleuniger für die Stifteingabe

Die Eingabe per Stift ist ebenso präzise wie die Eingabe per Zeigegerät. Mit stiftbasierten Beschleunigern können Benutzer gängige Aktionen wie beispielsweise das Öffnen von Kontextmenüs durchführen. Zum Öffnen eines Kontextmenüs kann der Benutzer mit gedrückter Drucktaste auf das Display tippen oder alternativ lange auf den Inhalt drücken. Er kann den Stift auch über Inhalten platzieren, um ähnlich wie mit der Maus Details zur Benutzeroberfläche (z. B. QuickInfos) aufzurufen oder sekundäre Hoveraktionen anzuzeigen.

Wie Sie Ihre App für die Stifteingabe optimieren können, erfahren Sie im Artikel zum Thema [Interaktion per Eingabestift](../input/pen-and-stylus-interactions.md).


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

* Stellen Sie sicher, dass Ihre Benutzer auf sämtliche Befehle zugreifen können, und zwar über alle Typen von Windows-Geräten.
* Integrieren Sie ein Kontextmenü, das alle für ein Sammlungselement verfügbaren Befehle bereitstellt. 
* Implementieren Sie Eingabebeschleuniger für häufig verwendete Befehle. 
* Verwenden Sie zur Implementierung von Befehlen die [ICommand-Schnittstelle](/uwp/api/Windows.UI.Xaml.Input.ICommand). 

## <a name="related-topics"></a>Zugehörige Themen
* [ICommand-Schnittstelle](/uwp/api/Windows.UI.Xaml.Input.ICommand)
* [Menüs und Kontextmenüs](menus.md)
* [Wischen](swipe.md)
* [Aktualisierung durch Ziehen](pull-to-refresh.md)
* [Interaktion per Eingabestift](../input/pen-and-stylus-interactions.md)
* [App-Optimierung für Gamepads und Xbox](../devices/designing-for-tv.md)