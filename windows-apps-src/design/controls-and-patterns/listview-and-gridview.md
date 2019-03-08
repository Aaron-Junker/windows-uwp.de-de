---
Description: Verwenden Sie ListView und GridView-Steuerelemente zum Anzeigen und Bearbeiten von Gruppen von Daten, z.B. ein Katalog von Bildern oder einen Satz von e-Mail-Nachrichten.
title: Listenansicht und Rasteransicht
label: List view and grid view
template: detail.hbs
ms.date: 05/20/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4efbdaef0fbcbe076133be1c491f7ed173c01257
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653575"
---
# <a name="list-view-and-grid-view"></a>Listenansicht und Rasteransicht

Mit den meisten Anwendungen können Sätze von Daten, beispielsweise eine Bildergalerie oder ein Satz von E-Mails, bearbeitet und angezeigt werden. Das XAML-Benutzeroberflächenframework bietet ListView und GridView-Steuerelemente, die das Anzeigen und Bearbeiten von Daten in Ihrer App vereinfachen.  

> **Wichtige APIs**: [ListView-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [GridView-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx), [ItemsSource-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx), [-Elementeigenschaft](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx)

ListView und GridView werden beide von der ListViewBase-Klasse abgeleitet, sodass sie die gleiche Funktionalität haben, Daten jedoch unterschiedlich anzeigen. In diesem Artikel beziehen sich Aussagen zu ListView sowohl auf die ListView- als auch die GridView-Steuerelemente, wenn nicht anders angegeben. Möglicherweise werden Klassen wie ListView oder ListViewItem genannt. Das Präfix „List“ kann jedoch durch „Grid“ für das entsprechende Rastersteuerelement ersetzt werden (GridView oder GridViewItem). 

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

ListView zeigt Daten vertikal in einer einzelnen Spalte an. Sie wird häufig für die Anzeige geordneter Listen von Elementen verwendet, z. B. von E-Mails oder Suchergebnissen. 

![Eine Listenansicht mit gruppierten Daten](images/simple-list-view-phone.png)

GridView zeigt eine Sammlung von Elementen in Zeilen und Spalten an, für die ein horizontaler Bildlauf durchgeführt werden kann. Daten werden horizontal angeordnet, bis die Spalten gefüllt sind. Anschließend wird mit der nächsten Zeile fortgefahren. Die Rasteransicht wird häufig für Visualisierungen verwendet, bei denen die Elemente mehr Platz benötigen, z. B. in einer Fotogalerie. 

![Beispiel einer Inhaltsbibliothek](images/controls_list_contentlibrary.png)

Einen detaillierteren Vergleich und Anleitungen dazu, welches Steuerelement verwendet werden sollte, finden Sie unter [Listen](lists.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ListView">ListView</a> oder <a href="xamlcontrolsgallery:/item/GridView">GridView</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Abrufen der XAML-Steuerelemente Katalog-app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-list-view"></a>Erstellen einer Listenansicht

Die Listenansicht ist ein [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx), sodass sie eine Sammlung von Elementen jeden Typs enthalten kann. In der [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx)-Sammlung müssen Elemente vorhanden sein, bevor auf dem Bildschirm etwas angezeigt werden kann. Elemente für die Ansicht können Sie direkt in die [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx)-Sammlung einfügen oder alternativ die [ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx)-Eigenschaft als Datenquelle festlegen. 

**Wichtig**&nbsp;&nbsp;Sie können Items oder ItemsSource zum Ausfüllen der Liste verwenden, jedoch nicht beide gleichzeitig. Wenn Sie die ItemsSource-Eigenschaft festlegen und dann ein Element in XAML hinzufügen, wird das hinzugefügte Element ignoriert. Wenn Sie die ItemsSource-Eigenschaft festlegen und der Items-Sammlung ein Element in Code hinzufügen, wird eine Ausnahme ausgelöst.

> **Hinweis**&nbsp;&nbsp;Viele Beispiele in diesem Artikel füllen der Einfachheit halber die **Items**-Sammlung direkt aus. Häufiger stammen die Elemente in einer Liste jedoch aus einer dynamischen Quelle, z. B. einer Liste von Büchern aus einer Onlinedatenbank. Verwenden Sie für diesen Zweck die **ItemsSource**-Eigenschaft. 

### <a name="add-items-to-the-items-collection"></a>Hinzufügen von Elementen zur Sammlung Items

Die Elemente können der [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx)-Sammlung mittels XAML oder Code hinzugefügt werden. In der Regel fügen Sie Elemente auf diese Weise hinzu, wenn Sie nur über eine geringe Anzahl von Elementen verfügen, die sich nicht ändern und einfach in XAML definiert werden können, oder wenn die Elemente zur Laufzeit im Code generiert werden. 

Hier sehen Sie eine Listenansicht mit inline in XAML definierten Elementen. Wenn Sie die Elemente in XAML definieren, werden sie der Sammlung Items automatisch hinzugefügt.

**XAML**
```xaml
<ListView x:Name="listView1"> 
   <x:String>Item 1</x:String> 
   <x:String>Item 2</x:String> 
   <x:String>Item 3</x:String> 
   <x:String>Item 4</x:String> 
   <x:String>Item 5</x:String> 
</ListView>  
```

Hier sehen Sie die Listenansicht in Code erstellt. Die resultierende Liste ist mit der zuvor in XAML erstellten Liste identisch.

**C#**
```csharp
// Create a new ListView and add content. 
ListView listView1 = new ListView(); 
listView1.Items.Add("Item 1"); 
listView1.Items.Add("Item 2"); 
listView1.Items.Add("Item 3"); 
listView1.Items.Add("Item 4"); 
listView1.Items.Add("Item 5");
 
// Add the ListView to a parent container in the visual tree. 
stackPanel1.Children.Add(listView1); 
```

ListView sieht wie folgt aus.

![Eine einfache Listenansicht](images/listview-simple.png)

### <a name="set-the-items-source"></a>Festlegen der Quelle von Elementen

In der Regel verwenden Sie eine Listenansicht, um Daten aus Quellen wie einer Datenbank oder dem Internet anzuzeigen. Um eine Datenquelle für die Listenansicht zu verwenden, müssen Sie eine Sammlung von Datenelementen als deren [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx)-Eigenschaft festlegen.

Dies ist die ItemsSource der Listenansicht im Code, direkt auf eine Instanz einer Sammlung festgelegt.

**C#**
```csharp 
// Instead of hard coded items, the data could be pulled 
// asynchronously from a database or the internet.
ObservableCollection<string> listItems = new ObservableCollection<string>();
listItems.Add("Item 1");
listItems.Add("Item 2");
listItems.Add("Item 3");
listItems.Add("Item 4");
listItems.Add("Item 5");

// Create a new list view, add content, 
ListView itemListView = new ListView();
itemListView.ItemsSource = listItems;

// Add the list view to a parent container in the visual tree.
stackPanel1.Children.Add(itemListView);
```

Sie können die ItemsSource-Eigenschaft auch an eine Sammlung in XAML binden. Weitere Informationen zur Datenbindung finden Sie unter [Übersicht über Datenbindung](https://msdn.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).

Hier wird ItemsSource an eine öffentliche Eigenschaft mit dem Namen `Items` gebunden, die die private Datensammlung der Seite verfügbar macht.

**XAML**
```xaml
<ListView x:Name="itemListView" ItemsSource="{x:Bind Items}"/>
```

**C#**
```csharp
private ObservableCollection<string> _items = new ObservableCollection<string>();

public ObservableCollection<string> Items
{
    get { return this._items; }
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Items.Add("Item 1");
    Items.Add("Item 2");
    Items.Add("Item 3");
    Items.Add("Item 4");
    Items.Add("Item 5");
}
```

Wenn Sie in der Listenansicht gruppierte Daten anzeigen müssen, müssen Sie an eine [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) binden. Die CollectionViewSource dient als Proxy für die Sammlungsklasse in XAML und ermöglicht die Unterstützung für Gruppierungen. Weitere Informationen finden Sie unter [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx).

## <a name="data-template"></a>Datenvorlage

Die Datenvorlage eines Elements definiert die Darstellung der Daten. Datenelemente werden in der Listenansicht standardmäßig als Zeichenfolgendarstellung des Datenobjekts angezeigt, an das sie gebunden sind. Sie können die Zeichenfolgendarstellung einer bestimmten Eigenschaft des Datenelements anzeigen, indem Sie den [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) zur Eigenschaft festlegen.

In der Regel möchten Sie jedoch eine ansprechendere Darstellung Ihrer Daten anzeigen. Um genau anzugeben, wie Elemente in der Listenansicht angezeigt werden, müssen Sie ein [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx) erstellen. Der XAML-Code in der DataTemplate definiert das Layout und die Darstellung von Steuerelementen, die zum Anzeigen eines einzelnen Elements verwendet werden. Die Steuerelemente im Layout können an Eigenschaften eines Datenobjekts gebunden werden. Es ist auch möglich, statischen Inhalt intern zu definieren. Weisen Sie das DataTemplate der [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)-Eigenschaft des Listensteuerelements zu.

In diesem Beispiel ist das Datenelement eine einfache Zeichenfolge. Verwenden Sie eine DataTemplate, um auf der linken Seite der Zeichenfolge ein Bild hinzuzufügen und die Zeichenfolge in Aquamarin anzuzeigen.

> **Hinweis**:&nbsp;&nbsp;Wenn Sie die [x:Bind-Markuperweiterung](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) in einem DataTemplate verwenden, müssen Sie den DataType (`x:DataType`) für das DataTemplate angeben.

**XAML**
```XAML
<ListView x:Name="listView1">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="47"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="32" Height="32" 
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Teal" 
                           FontSize="15" Grid.Column="1"/>
            </Grid> 
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

So sehen die Datenelemente aus, wenn sie mit dieser Datenvorlage angezeigt werden.

![Elemente in der Listenansicht mit einer Datenvorlage](images/listview-itemstemplate.png)

Datenvorlagen sind der bevorzugte Weg für die Definition des Aussehens Ihrer Listenansicht. Sie können auch eine erhebliche Auswirkung auf die Leistung haben, wenn in der Liste eine große Anzahl von Elementen angezeigt wird. In diesem Artikel werden für die meisten Beispiele einfache Zeichenfolgendaten verwendet, und es wird keine Datenvorlage angegeben. Weitere Informationen und Beispiele zur Verwendung von Datenvorlagen und Elementcontainern zur Definition des Aussehens von Elementen in Ihrer Liste oder Ihrem Raster finden Sie unter [Elementcontainer und Vorlagen](item-containers-templates.md). 

## <a name="change-the-layout-of-items"></a>Ändern des Layouts von Elementen

Wenn Sie Elemente zu einer Listen- oder Rasteransicht hinzufügen, bricht das Steuerelement automatisch alle Elemente in einem Elementcontainer um und ordnet anschließend alle Elementcontainer an. Die Anordnung dieser Elementcontainer ist vom [ItemsPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) des Steuerelements abhängig.  
- Standardmäßig verwendet **ListView** ein [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx), das eine vertikale Liste wie diese erzeugt.

![Eine einfache Listenansicht](images/listview-simple.png)

- **GridView** verwendet ein [ItemsWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemswrapgrid.aspx), das Elemente horizontal hinzufügt, diese anschließend umbricht und einen vertikalen Bildlauf ausführt, wie hier gezeigt.

![Eine einfache Rasteransicht](images/gridview-simple.png)

Sie können das Layout von Elementen ändern, indem Sie Eigenschaften im Elementpanel anpassen oder das Standardpanel durch ein anderes Panel ersetzen.

> Hinweis&nbsp;&nbsp;Achten Sie darauf, dass Sie keine Virtualisierung deaktivieren, wenn Sie ItemsPanel ändern. Sowohl **ItemsStackPanel** als auch **ItemsWrapGrid** unterstützen Virtualisierung, damit diese sicher verwendet werden können. Wenn Sie ein anderes Panel verwenden, könnten Sie die Virtualisierung deaktivieren und die Leistung der Listenansicht beeinträchtigen. Weitere Informationen finden Sie in den Artikeln zur Listenansicht unter [Leistung](https://msdn.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui). 

In diesem Beispiel wird gezeigt, wie eine **ListView** ihre Elementcontainer in einer horizontalen Liste anordnet, indem die [Orientation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.orientation.aspx)-Eigenschaft von **ItemsStackPanel** geändert wird.
Da die Listenansicht standardmäßig den vertikalen Bildlauf verwendet, müssen Sie auch einige Eigenschaften im internen [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) der Listenansicht anpassen, damit der Bildlauf horizontal durchgeführt wird.
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx) zu **Enabled** oder **Auto**
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx) zu **Auto** 
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx) zu **Disabled** 
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx) zu **Hidden** 

> **Hinweis**&nbsp;&nbsp;Diese Beispiele werden mit unbeschränkter Breite der Listenansicht gezeigt, sodass die horizontalen Bildlaufleisten nicht angezeigt werden. Wenn Sie diesen Code ausführen, können Sie für ListView `Width="180"` festlegen, um die Bildlaufleisten anzuzeigen.

**XAML**
```xaml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

Die resultierende Liste sieht wie folgt aus.

![Eine horizontale Listenansicht](images/listview-horizontal.png)

 Im nächsten Beispiel ordnet **ListView** Elemente in einer Liste an, die vertikal umbrochen wird, indem **ItemsWrapGrid** anstelle von **ItemsStackPanel** verwendet wird. 
 
> **Note**&nbsp;&nbsp;Die Höhe der Listeneinsicht muss eingeschränkt werden, um das Steuerelement zu zwingen, die Container umzubrechen.

**XAML**
```xaml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

Die resultierende Liste sieht wie folgt aus.

![Eine Listenansicht mit Rasterlayout](images/listview-itemswrapgrid.png)

Wenn Sie in Ihrer Listenansicht können gruppierte Daten anzeigen möchten, bestimmt ItemsPanel die Anordnung der Elementgruppen, nicht die Anordnung der einzelnen Elemente. Wenn das horizontale ItemsStackPanel, das zuvor gezeigt wurde, zur Anzeige gruppierter Daten verwendet wird, werden die Gruppen horizontal angeordnet. Die Elemente in den einzelnen Gruppen werden jedoch weiterhin vertikal angeordnet, wie hier gezeigt.

![Eine gruppierte horizontale Listenansicht](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>Auswahl von Elementen und Interaktion

Sie können aus verschiedenen Möglichkeiten wählen, wie Benutzer mit einer Listenansicht interagieren können. Standardmäßig können Benutzer ein einzelnes Element auswählen. Sie können die [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx)-Eigenschaft ändern, um eine Mehrfachauswahl zu ermöglichen oder die Auswahl zu deaktivieren. Sie können die Eigenschaft [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) festlegen, sodass ein Benutzer auf ein Element klickt, um eine Aktion aufzurufen (wie bei einer Schaltfläche), statt das Element auszuwählen.

> **Hinweis**&nbsp;&nbsp;Sowohl ListView als auch GridView verwenden für ihre SelectionMode-Eigenschaften die [ListViewSelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewselectionmode.aspx)-Enumeration. IsItemClickEnabled ist standardmäßig **False**. Daher müssen Sie es lediglich festlegen, um den Klickmodus zu aktivieren.

In der folgenden Tabelle werden die Arten gezeigt, wie Benutzer mit einer Listenansicht interagieren können und wie Sie auf die jeweilige Interaktion reagieren können.

Um diese Interaktion zu ermöglichen: | Verwenden Sie diese Einstellungen: | Behandeln Sie dieses Ereignis: | Verwenden Sie diese Eigenschaft zum Abrufen des ausgewählten Elements:
----------------------------|---------------------|--------------------|--------------------------------------------
Keine Interaktion | [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) = **None**, [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) = **False** | n. a. | n. a. 
Einzelauswahl | SelectionMode = **Single**, IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx), [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx)  
Mehrfachauswahl | SelectionMode = **Multiple**, IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
Erweiterte Auswahl | SelectionMode = **Extended**, IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
Klicken Sie | SelectionMode = **None**, IsItemClickEnabled = **True** | [ItemClick](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.itemclick.aspx) | n. a. 

> **Hinweis**&nbsp;&nbsp;Ab Windows 10 können Sie IsItemClickEnabled aktivieren, um ein ItemClick-Ereignis auszulösen, während SelectionMode ebenfalls auf Single, Multiple oder Extended festgelegt ist. Wenn Sie dies tun, wird zuerst das ItemClick-Ereignis und anschließend das SelectionChanged-Ereignis ausgelöst. In einigen Fällen, wenn Sie beispielsweise zu einer anderen Seite im ItemClick-Ereignishandler navigieren, wird das SelectionChanged-Ereignis nicht ausgelöst, und das Element wird nicht ausgewählt.

Sie können diese Eigenschaften in XAML oder im Code festlegen, wie hier gezeigt.

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>Schreibgeschützt

Sie können die Eigenschaft SelectionMode auf **ListViewSelectionMode.None** festlegen, um die Elementauswahl zu deaktivieren. Dadurch wird das Steuerelement im schreibgeschützten Modus ausgeführt und kann zwar zum Anzeigen von Daten, nicht jedoch für die Interaktion mit ihm verwendet werden. Das Steuerelement selbst ist nicht deaktiviert ist, nur die Elementauswahl ist deaktiviert.

### <a name="single-selection"></a>Einzelauswahl

In der folgenden Tabelle werden die Tastatur-, Maus- und Touchinteraktionen beschrieben, wenn SelectionMode **Single** ist.

Zusatztaste | Interaktion
-------------|------------
Keine | <li>Ein Benutzer kann ein einzelnes Element mit der LEERTASTE, per Mausklick oder durch Tippen auswählen.</li>
STRG | <li>Ein Benutzer kann die Auswahl eines einzelnen Elements mit der LEERTASTE, per Mausklick oder durch Tippen aufheben.</li><li>Mit den Pfeiltasten kann ein Benutzer den Fokus unabhängig von der Auswahl verschieben.</li>

Wenn der SelectionMode auf **Single** eingestellt ist, erhalten Sie das ausgewählte Datenelement über die [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx)-Eigenschaft. Den Index der Sammlung des ausgewählten Elements erhalten Sie mithilfe der [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx)-Eigenschaft. Wenn kein Element ausgewählt ist, ist SelectedItem **null**, und SelectedIndex ist -1. 
 
Wenn Sie versuchen, ein Element festzulegen, das nicht in der **Items**-Sammlung enthalten ist, wird der Vorgang ignoriert, und **SelectedItem** ist **null**. Wenn Sie jedoch versuchen, **SelectedIndex** auf einen Index festzulegen, die außerhalb des Bereichs der **Items** in der Liste ist, wird eine **System.ArgumentException**-Ausnahme ausgelöst. 

### <a name="multiple-selection"></a>Mehrfachauswahl

In der folgenden Tabelle werden die Tastatur-, Maus- und Touchinteraktionen beschrieben, wenn SelectionMode **Multiple** ist.

Zusatztaste | Interaktion
-------------|------------
Keine | <li>Ein Benutzer kann mehrerer Objekte mit der LEERTASTE, per Mausklick oder durch Tippen auswählen, um die Auswahl zum fokussierten Element zu verschieben.</li><li>Mit den Pfeiltasten kann ein Benutzer den Fokus unabhängig von der Auswahl verschieben.</li>
UMSCHALTTASTE | <li>Benutzer können mehrere zusammenhängende Elemente auswählen, indem sie auf das erste Element in der Auswahl und anschließend auf das letzte Element in der Auswahl klicken oder tippen.</li><li>Mit den Pfeiltasten können Benutzer eine zusammenhängende Auswahl beginnend mit dem ausgewählten Element erstellen, wenn die UMSCHALTTASTE gedrückt wird.</li>

### <a name="extended-selection"></a>Erweiterte Auswahl

In der folgenden Tabelle werden die Tastatur-, Maus- und Touchinteraktionen beschrieben, wenn SelectionMode **Extended** ist.

Zusatztaste | Interaktion
-------------|------------
Keine | <li>Das Verhalten ist mit der Auswahl **Single** identisch.</li>
STRG | <li>Ein Benutzer kann mehrerer Objekte mit der LEERTASTE, per Mausklick oder durch Tippen auswählen, um die Auswahl zum fokussierten Element zu verschieben.</li><li>Mit den Pfeiltasten kann ein Benutzer den Fokus unabhängig von der Auswahl verschieben.</li>
UMSCHALTTASTE | <li>Benutzer können mehrere zusammenhängende Elemente auswählen, indem sie auf das erste Element in der Auswahl und anschließend auf das letzte Element in der Auswahl klicken oder tippen.</li><li>Mit den Pfeiltasten können Benutzer eine zusammenhängende Auswahl beginnend mit dem ausgewählten Element erstellen, wenn die UMSCHALTTASTE gedrückt wird.</li>

Wenn der SelectionMode auf **Multiple** oder **Extended** eingestellt ist, erhalten Sie die ausgewählten Datenelemente über die [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)-Eigenschaft. 

Die Eigenschaften **SelectedIndex**, **SelectedItem** und **SelectedItems** sind synchronisiert. Wenn Sie beispielsweise SelectedIndex auf-1 festlegen, wird SelectedItem auf **null** festgelegt und SelectedItems ist leer. Wenn Sie SelectedItem auf **null** festlegen, wird SelectedIndex auf-1 festgelegt und SelectedItems ist leer.

Im Mehrfachauswahlmodus enthält **SelectedItem** das Element, das zuerst ausgewählt wurde, und **Selectedindex** enthält den Index des Elements, das zuerst ausgewählt wurde. 

### <a name="respond-to-selection-changes"></a>Reagieren auf Änderungen der Auswahl

Um auf Änderungen der Auswahl in einer Listenansicht zu reagieren, müssen Sie das [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx)-Event bearbeiten. Im Code des Ereignis-Handlers können Sie die Liste der ausgewählten Elemente über die [SelectionChangedEventArgs.AddedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.addeditems.aspx)-Eigenschaft abrufen. Sie können alle Elemente abrufen, deren Auswahl über die [SelectionChangedEventArgs.RemovedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.removeditems.aspx)-Eigenschaft aufgehoben wurde. Die AddedItems- und RemovedItems-Sammlungen enthalten höchstens 1 Element, wenn der Benutzer keinen Bereich von Elementen auswählt, indem er die UMSCHALTTASTE gedrückt hält.

In diesem Beispiel werden die Behandlung des **SelectionChanged**-Ereignisses und der Zugriff auf die verschiedenen Elementsammlungen gezeigt.

**XAML**
```xaml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>Klickmodus

Sie können eine Listenansicht so ändern, dass die Benutzer auf die Elemente wie auf Schaltflächen klicken, anstatt sie auszuwählen. Das bietet sich z. B. an, wenn Ihre App zu einer neuen Seite navigieren soll, wenn ein Benutzer auf ein Element in der Liste oder im Raster klickt. So aktivieren Sie dieses Verhalten
- Legen Sie **SelectionMode** auf **None** fest.
- Legen Sie **IsItemClickEnabled** auf **true** fest.
- Behandeln Sie das **ItemClick**-Ereignis, um eine Aktion auszuführen, wenn der Benutzer auf ein Element klickt.

Dies ist eine Listenansicht mit Elementen, auf die geklickt werden kann. Der Code im Ereignishandler ItemClick navigiert zu einer neuen Seite.

**XAML**
```xaml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>Programmgesteuerte Auswahl eines Bereichs von Elementen

In manchen Fällen müssen Sie die Elementauswahl einer Listenansicht programmgesteuert bearbeiten. Angenommen, es gibt eine Schaltfläche **Alles auswählen**, um einen Benutzer alle Elemente in einer Liste auswählen zu lassen. In diesem Fall ist es in der Regel nicht sehr effizient, der Sammlung SelectedItems Elemente einzeln hinzuzufügen oder Elemente einzeln aus dieser zu entfernen. Jedes Element führt zu einem SelectionChanged-Ereignis. Wenn Sie direkt mit den Elementen arbeiten und nicht mit Indexwerten, wird die Virtualisierung des betreffenden Elements deaktiviert.

Mittels der Methoden [SelectAll](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectall.aspx), [SelectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectrange.aspx) und [DeselectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.deselectrange.aspx) lässt sich die Auswahl auf effizientere Weise ändern als über die SelectedItems-Eigenschaft. Diese Methoden führen mithilfe von Elementindexbereichen Auswahlen durch oder heben Auswahlen auf. Elemente, die virtualisiert sind, bleiben virtualisiert, da nur der Index verwendet wird. Alle Elemente im angegebenen Bereich werden ausgewählt (bzw. die Auswahl aller Elemente im angegeben Bereich wird aufgehoben), unabhängig vom ursprünglichen Auswahlzustand. Das SelectionChanged-Ereignis tritt nur einmal für jeden Aufruf dieser Methoden auf.

> **Wichtig**&nbsp;&nbsp;Sie sollten diese Methoden nur aufrufen, wenn die SelectionMode-Eigenschaft auf Multiple oder Extended festgelegt ist. Wenn Sie SelectRange aufrufen, wenn der Auswahlmodus Single oder None ist, wird eine Ausnahme ausgelöst.

Verwenden Sie bei der Auswahl von Elementen mit Indexbereichen die [SelectedRanges](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectedranges.aspx)-Eigenschaft, um alle ausgewählten Bereiche in der Liste abzurufen.

Wenn ItemsSource [IItemsRangeInfo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.iitemsrangeinfo.aspx) implementiert, und Sie diese Methoden verwenden, um die Auswahl zu ändern, werden die Eigenschaften **AddedItems** und **RemovedItems** in SelectionChangedEventArgs nicht festgelegt. Das Festlegen dieser Eigenschaften erfordert die Deaktivierung der Virtualisierung des Elementobjekts. Verwenden Sie zum Abrufen der Elemente stattdessen die Eigenschaft **SelectedRanges**.

Sie können alle Elemente in einer Sammlung auswählen, indem Sie die SelectAll-Methode aufrufen. Es gibt jedoch keine entsprechende Methode, um die Auswahl aller Elemente aufzuheben. Sie können die Auswahl aller Elemente aufheben, indem Sie DeselectRange aufrufen und einen [ItemIndexRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.aspx) mit einem Wert für [FirstIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.firstindex.aspx) von 0 und einem Wert für [Length](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.length.aspx) übergeben, der der Anzahl der Elemente in der Sammlung entspricht. 

**XAML**
```xaml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

Informationen zum Ändern der Darstellung ausgewählter Elemente finden Sie unter [Elementcontainer und Vorlagen](item-containers-templates.md).

### <a name="drag-and-drop"></a>Drag & Drop

ListView- und GridView-Steuerelemente unterstützen Drag & Drop für Elemente innerhalb von ihnen und zwischen ihnen und anderen ListView- und GridView-Steuerelementen. Weitere Informationen zur Implementierung des Musters für Drag & Drop finden Sie unter [Drag & Drop](https://msdn.microsoft.com/windows/uwp/design/input/drag-and-drop). 

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [XAML-Beispiel für ListView und GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) – Veranschaulicht die ListView- und GridView-Steuerelementen.
- [XAML-Drag & Drop-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) – Veranschaulicht Drag & Drop mit dem ListView-Steuerelement.
- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Listen](lists.md)
- [Elementcontainer und Vorlagen](item-containers-templates.md)
- [Drag & Drop](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
