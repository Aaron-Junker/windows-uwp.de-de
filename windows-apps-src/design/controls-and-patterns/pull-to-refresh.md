---
Description: Verwenden Sie das Steuerelement „Aktualisieren durch Ziehen“, um neue Inhalte in eine Liste abzurufen.
title: Aktualisieren durch Ziehen
label: Pull-to-refresh
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: faff0857679d50f6995640bbf9bf0222bb0d2e37
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081612"
---
# <a name="pull-to-refresh"></a>Aktualisierung durch Ziehen

Mithilfe der Aktion „Aktualisieren durch Ziehen“ können Benutzer eine Datenliste per Toucheingabe nach unten ziehen, um weitere Daten abzurufen. Die Aktion „Aktualisieren durch Ziehen“ wird häufig auf Geräten mit Touchscreen verwendet. Sie können die hier gezeigten APIs zum Implementieren von „Aktualisieren durch Ziehen“ in Ihre App verwenden.

> **Wichtige APIs:** [RefreshContainer](/uwp/api/windows.ui.xaml.controls.refreshcontainer), [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)

![GIF zu „Aktualisieren durch Ziehen“](images/Pull-To-Refresh.gif)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie „Aktualisieren durch Ziehen“ für Datenlisten oder -raster, die vom Benutzer regelmäßig aktualisiert werden, vor allem, wenn die App hauptsächlich auf Geräten mit Touchscreen ausgeführt werden soll.

Sie können auch den [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer) verwenden, um eine konsistente Aktualisierungsoption zu erstellen, die auf andere Weise, wie z. B. durch eine Aktualisierungsschaltfläche aufgerufen wird.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/PullToRefresh">die App zu öffnen und PullToRefresh in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="refresh-controls"></a>Aktualisierungssteuerelemente

„Aktualisieren durch Ziehen“ wird durch zwei Steuerelemente aktiviert.

- **RefreshContainer** – Ein ContentControl, das einen Wrapper für das Aktualisieren-durch-Ziehen-Erlebnis bereitstellt. Es verarbeitet die Interaktionen per Toucheingabe und verwaltet den Zustand des internen Visualizers für Aktualisierungen.
- **RefreshVisualizer** – Kapselt die Aktualisierungsvisualisierung, die im nächsten Abschnitt erläutert wird.

Das wichtigste Steuerelement ist **RefreshContainer**, das Sie als Wrapper um den Inhalt platzieren, den der Benutzer abruft, um eine Aktualisierung auszulösen. Da RefreshContainer nur mit Toucheingabe funktioniert, wird empfohlen, dass Sie auch eine Aktualisierungsschaltfläche für Benutzer bereitstellen, die über keinen Touchscreen verfügen. Sie können die Aktualisierungsschaltfläche an einer geeigneten Stelle in der App positionieren, entweder auf einer Befehlsleiste oder in der Nähe der Oberfläche, die aktualisiert wird.

## <a name="refresh-visualization"></a>Visualisierung aktualisieren

Die Visualisierung der Standardaktualisierung ist ein kreisförmiges Fortschritts-Drehfeld, das über den Zeitpunkt einer Aktualisierung und deren Fortschritt informiert, nachdem diese gestartet wurde. Der Aktualisierungs-Visualizer verfügt über fünf Zustände.

 Der Abstand, den der Benutzer benötigt, um eine Liste zum Initiieren einer Aktualisierung nach unten zu ziehen, wird als _Schwellenwert_ bezeichnet. Der Visualizer [Zustand](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.State) wird anhand des Ziehstatus im Zusammenhang mit diesen Schwellenwert bestimmt. Die möglichen Werte sind in der [RefreshVisualizerState](/uwp/api/windows.ui.xaml.controls.refreshvisualizerstate)-Enumeration enthalten.

### <a name="idle"></a>Idle

Der Standardzustand des Visualizers ist **Leerlauf**. Der Benutzer interagiert nicht per Toucheingabe mit dem RefreshContainer, und es wird gerade keine Aktualisierung ausgeführt.

Visuell gibt es keinen Nachweis für den Aktualisierungs-Visualizer.

### <a name="interacting"></a>Interaktion

Wenn der Benutzer die Liste in die von der PullDirection-Eigenschaft angegebene Richtung zieht und sich der Visualizer im Zustand **Interaktion** befindet, bevor der Schwellenwert erreicht wird.

- Wenn der Benutzer das Steuerelement loslässt, während es sich in diesem Zustand befindet, kehrt das Steuerelement in den **Leerlauf** zurück.

    ![Aktualisierung durch Ziehen vor dem Schwellenwert](images/ptr-prethreshold.png)

    Das Symbol wird visuell als deaktiviert (Deckkraft von 60 %) angezeigt. Darüber hinaus wird das Symbol mit der Scrollen-Aktion einmal vollständig gedreht.

- Wenn der Benutzer die Liste über den Schwellenwert hinaus zieht, wechselt der Visualizer von **Interaktion** in **Ausstehend**.

    ![Aktualisierung durch Ziehen am Schwellenwert](images/ptr-atthreshold.png)

    Das Symbol wechselt während des Übergangs zu einer Deckkraft von 100 % visuell und pulsiert bis zu einer Größe von 150 % und wieder zurück zu einer Größe von 100 %.

### <a name="pending"></a>Pending

Wenn der Benutzer die Liste über den Schwellenwert hinaus gezogen hat, befindet sich der Visualizer im Zustand **Ausstehend**.

- Wenn der Benutzer die Liste wieder über den Schwellenwert hinaus zieht, ohne sie loszulassen, kehrt sie in den Zustand **Interaktion** zurück.
- Wenn der Benutzer die Liste loslässt, wird eine Aktualisierungsanforderung initiiert und sie wechselt in den Zustand **Aktualisierung**.

![Aktualisierung durch Ziehen nach dem Schwellenwert](images/ptr-postthreshold.png)

Visuell weist das Symbol eine Größe und Deckkraft von 100 % auf. In diesem Zustand bewegt sich das Symbol mit der Aktion „Bildlauf“ weiterhin nach unten, dreht sich jedoch nicht mehr.

### <a name="refreshing"></a>Wird aktualisiert

Wenn der Benutzer den Visualizer in einer über den Schwellenwert hinausgehenden Position freigibt, befindet er sich im Zustand **Aktualisierung**.

Wenn dieser Zustand erreicht ist, wird das **RefreshRequested**-Ereignis ausgelöst. Dies ist das Signal für den Start der Aktualisierung des App-Inhalts. Die Ereignisargumente ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) enthalten das Objekt [Verzögerung](/uwp/api/windows.foundation.deferral), für das Sie ein Handle im Ereignishandler verwenden sollten. Anschließend sollten Sie die Verzögerung als abgeschlossen markieren, wenn der Code zum Ausführen der Aktualisierung abgeschlossen ist.

Wenn die Aktualisierung abgeschlossen ist, kehrt der Visualizer in den Zustand **Leerlauf** zurück.

Visuell kehrt das Symbol zur Schwellenwertposition zurück und dreht sich für die Dauer der Aktualisierung. Diese Drehung dient dazu, den Fortschritt der Aktualisierung anzuzeigen, und wird durch die Animation des eingehenden Inhalts ersetzt.

### <a name="peeking"></a>Einsehen

Wenn der Benutzer von einer Startposition, an der eine Aktualisierung unzulässig ist, in die Aktualisierungsrichtung zieht, wechselt der Visualizer in den Zustand **Einsehen**. Dies geschieht in der Regel auf, wenn der ScrollViewer sich nicht an der Position 0 befindet, wenn der Benutzer den Ziehvorgang startet.

- Wenn der Benutzer das Steuerelement loslässt, während es sich in diesem Zustand befindet, kehrt das Steuerelement in den **Leerlauf** zurück.

## <a name="pull-direction"></a>Ziehrichtung

Der Benutzer zieht eine Liste standardmäßig von oben nach unten, um eine Aktualisierung zu initiieren. Wenn Sie über eine Liste oder ein Raster mit einer anderen Ausrichtung verfügen, sollten Sie die Ziehrichtung des Aktualisierungs-Containers entsprechend ändern.

Die [PullDirection](/uwp/api/windows.ui.xaml.controls.refreshcontainer.PullDirection) Eigenschaft nimmt einen der folgenden [RefreshPullDirection](/uwp/api/windows.ui.xaml.controls.refreshpulldirection) Werte an: **BottomToTop**, **TopToBottom**, **RightToLeft** oder **LeftToRight**.

Wenn Sie die Ziehrichtung ändern, dreht sich die Startposition des Fortschritts-Drehfelds des Visualizers automatisch, sodass der Pfeil in der entsprechenden Position für die Ziehrichtung startet. Bei Bedarf können Sie die [RefreshVisualizer.Orientation](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.Orientation)-Eigenschaft ändern, um das automatische Verhalten außer Kraft zu setzen. In den meisten Fällen empfiehlt es sich, den Standardwert **Auto** beizubehalten.

## <a name="implement-pull-to-refresh"></a>Implementieren von Aktualisierung durch Ziehen

Um einer Liste die Funktionalität „Aktualisierung durch Ziehen“ hinzuzufügen, müssen nur wenige Schritte ausgeführt werden.

1. Umschließen Sie Ihre Liste in einem **RefreshContainer**-Steuerelement.
1. Behandeln Sie das **RefreshRequested**-Ereignis, um Ihre Inhalte zu aktualisieren.
1. Initiieren Sie optional eine Aktualisierung durch Aufrufen von **RequestRefresh** (z. B. durch Klicken auf eine Schaltfläche).

> [!NOTE]
> Sie können einen eigenständigen RefreshVisualizer instanziieren. Es empfiehlt sich jedoch, Ihre Inhalte in einem RefreshContainer zu umschließen und den von der RefreshContainer.Visualizer-Eigenschaft bereitgestellten RefreshVisualizer auch für Szenarien ohne Touchscreen zu verwenden. In diesem Artikel wird davon ausgegangen, dass der Visualizer immer aus dem Aktualisierungs-Container abgerufen wird.

> Verwenden Sie außerdem der Einfachheit halber die RequestRefresh- und RefreshRequested-Elemente des Containers. `refreshContainer.RequestRefresh()` entspricht `refreshContainer.Visualizer.RequestRefresh()` und löst sowohl das RefreshContainer.RefreshRequested-Ereignis als auch die RefreshVisualizer.RefreshRequested-Ereignisse aus.

### <a name="request-a-refresh"></a>Anfordern einer Aktualisierung

Der Aktualisierungs-Container verarbeitet Touchinteraktionen, damit Benutzer Inhalte per Toucheingabe aktualisieren können. Es wird empfohlen, dass Sie andere Angebote für Schnittstellen bereitstellen, die keine Toucheingabe unterstützen, z. B. eine Aktualisierungsschaltfläche oder Sprachsteuerung.

Rufen Sie zum Initiieren einer Aktualisierung die [RequestRefresh](/uwp/api/windows.ui.xaml.controls.refreshcontainer.RequestRefresh)-Methode auf.

```csharp
// See the Examples section for the full code.
private void RefreshButtonClick(object sender, RoutedEventArgs e)
{
    RefreshContainer.RequestRefresh();
}
```

Beim Aufruf von RequestRefresh wechselt der Visualizer-Zustand direkt von **Leerlauf** zu **Aktualisierung**.

### <a name="handle-a-refresh-request"></a>Verarbeiten einer Aktualisierungsanforderung

Behandeln Sie das RefreshRequested-Ereignis, um aktualisierte Inhalte abzurufen. Im Ereignishandler benötigen Sie Code, der für Ihre App spezifisch ist, um die aktualisierten Inhalte abzurufen.

Die Ereignisargumente ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) enthalten ein [Verzögerung](/uwp/api/windows.foundation.deferral)-Objekt. Rufen Sie im Ereignishandler einen Handle für die Verzögerung ab. Markieren Sie die Verzögerung anschließend als abgeschlossen, wenn der Code zum Ausführen der Aktualisierung abgeschlossen ist.

```csharp
// See the Examples section for the full code.
private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
{
    // Respond to a request by performing a refresh and using the deferral object.
    using (var RefreshCompletionDeferral = args.GetDeferral())
    {
        // Do some async operation to refresh the content

         await FetchAndInsertItemsAsync(3);

        // The 'using' statement ensures the deferral is marked as complete.
        // Otherwise, you'd call
        // RefreshCompletionDeferral.Complete();
        // RefreshCompletionDeferral.Dispose();
    }
}
```

### <a name="respond-to-state-changes"></a>Reagieren auf Zustandsänderungen

Sie können bei Bedarf auf Änderungen des Visualizer-Zustands reagieren. Um mehrere Aktualisierungsanforderungen zu verhindern, können Sie eine Aktualisierungsschaltfläche während der Aktualisierung des Visualizers deaktivieren.

```csharp
// See the Examples section for the full code.
private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
{
    // Respond to visualizer state changes.
    // Disable the refresh button if the visualizer is refreshing.
    if (args.NewState == RefreshVisualizerState.Refreshing)
    {
        RefreshButton.IsEnabled = false;
    }
    else
    {
        RefreshButton.IsEnabled = true;
    }
}
```

## <a name="examples"></a>Beispiele

### <a name="using-a-scrollviewer-in-a-refreshcontainer"></a>Verwenden von ScrollViewer in einem RefreshContainer
> [!NOTE]
> Der Inhalt eines RefreshContainer muss ein scrollbares Steuerelement sein, z.B. ScrollViewer, GridView, ListView usw. Das Festlegen des Inhalts auf ein Steuerelement wie Grid führt zu einem nicht definierten Verhalten.

Anhand dieses Beispiels wird veranschaulicht, wie Sie „Aktualisierung durch Ziehen“ mit einer Bildlaufanzeige verwenden.

```xaml
<RefreshContainer>
    <ScrollViewer VerticalScrollMode="Enabled"
                  VerticalScrollBarVisibility="Auto"
                  HorizontalScrollBarVisibility="Auto">
 
        <!-- Scrollviewer content -->

    </ScrollViewer>
</RefreshContainer>
```

### <a name="adding-pull-to-refresh-to-a-listview"></a>Hinzufügen von „Aktualisierung durch Ziehen“ zu einer ListView

Anhand dieses Beispiels wird veranschaulicht, wie Sie „Aktualisierung durch Ziehen“ mit einer Listenansicht verwenden.

```xaml
<StackPanel Margin="0,40" Width="280">
    <CommandBar OverflowButtonVisibility="Collapsed">
        <AppBarButton x:Name="RefreshButton" Click="RefreshButtonClick"
                      Icon="Refresh" Label="Refresh"/>
        <CommandBar.Content>
            <TextBlock Text="List of items" 
                       Style="{StaticResource TitleTextBlockStyle}"
                       Margin="12,8"/>
        </CommandBar.Content>
    </CommandBar>

    <RefreshContainer x:Name="RefreshContainer">
        <ListView x:Name="ListView1" Height="400">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <Grid Height="80">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <TextBlock Text="{x:Bind Path=Header}"
                                   Style="{StaticResource SubtitleTextBlockStyle}"
                                   Grid.Row="0"/>
                        <TextBlock Text="{x:Bind Path=Date}"
                                   Style="{StaticResource CaptionTextBlockStyle}"
                                   Grid.Row="1"/>
                        <TextBlock Text="{x:Bind Path=Body}"
                                   Style="{StaticResource BodyTextBlockStyle}"
                                   Grid.Row="2"
                                   Margin="0,4,0,0" />
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RefreshContainer>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<ListItemData> Items { get; set; } 
        = new ObservableCollection<ListItemData>();

    public MainPage()
    {
        this.InitializeComponent();

        Loaded += MainPage_Loaded;
        ListView1.ItemsSource = Items;
    }

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        Loaded -= MainPage_Loaded;
        RefreshContainer.RefreshRequested += RefreshContainer_RefreshRequested;
        RefreshContainer.Visualizer.RefreshStateChanged += Visualizer_RefreshStateChanged;

        // Add some initial content to the list.
        await FetchAndInsertItemsAsync(2);
    }

    private void RefreshButtonClick(object sender, RoutedEventArgs e)
    {
        RefreshContainer.RequestRefresh();
    }

    private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
    {
        // Respond to a request by performing a refresh and using the deferral object.
        using (var RefreshCompletionDeferral = args.GetDeferral())
        {
            // Do some async operation to refresh the content

            await FetchAndInsertItemsAsync(3);

            // The 'using' statement ensures the deferral is marked as complete.
            // Otherwise, you'd call
            // RefreshCompletionDeferral.Complete();
            // RefreshCompletionDeferral.Dispose();
        }
    }

    private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
    {
        // Respond to visualizer state changes.
        // Disable the refresh button if the visualizer is refreshing.
        if (args.NewState == RefreshVisualizerState.Refreshing)
        {
            RefreshButton.IsEnabled = false;
        }
        else
        {
            RefreshButton.IsEnabled = true;
        }
    }

    // App specific code to get fresh data.
    private async Task FetchAndInsertItemsAsync(int updateCount)
    {
        for (int i = 0; i < updateCount; ++i)
        {
            // Simulate delay while we go fetch new items.
            await Task.Delay(1000);
            Items.Insert(0, GetNextItem());
        }
    }

    private ListItemData GetNextItem()
    {
        return new ListItemData()
        {
            Header = "Header " + DateTime.Now.Second.ToString(),
            Date = DateTime.Now.ToLongDateString(),
            Body = DateTime.Now.ToLongTimeString()
        };
    }
}

public class ListItemData
{
    public string Header { get; set; }
    public string Date { get; set; }
    public string Body { get; set; }
}
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Interaktionen per Toucheingabe](../input/touch-interactions.md)
- [Listenansicht und Rasteransicht](listview-and-gridview.md)
- [Elementcontainer und Vorlagen](item-containers-templates.md)
- [Ausdrucksanimationen](../../composition/composition-animation.md)
