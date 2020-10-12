---
description: Verwenden des ListView- und GridView-Steuerelements, um Sätze von Daten anzuzeigen und zu bearbeiten, z. B. eine Bildergalerie oder eine Reihe von E-Mail-Nachrichten
title: Listenansicht und Rasteransicht
label: List view and grid view
template: detail.hbs
ms.date: 11/04/2019
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 26f7e36d09857d37da4a0b4533cc8f65d2789e20
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829649"
---
# <a name="list-view-and-grid-view"></a>Listenansicht und Rasteransicht

Mit den meisten Anwendungen können Sätze von Daten, beispielsweise eine Bildergalerie oder ein Satz von E-Mails, bearbeitet und angezeigt werden. Das XAML-Benutzeroberflächenframework bietet ListView und GridView-Steuerelemente, die das Anzeigen und Bearbeiten von Daten in Ihrer App vereinfachen.  

> **Wichtige APIs:** [ListView-Klasse](/uwp/api/windows.ui.xaml.controls.listview), [GridView-Klasse](/uwp/api/windows.ui.xaml.controls.gridview), [ItemsSource-Eigenschaft](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), [Items-Eigenschaft](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)

> [!NOTE]
> ListView und GridView sind beide von der Klasse [ListViewBase](/uwp/api/windows.ui.xaml.controls.listviewbase) abgeleitet, sodass sie dieselbe Funktionalität haben, Daten jedoch unterschiedlich anzeigen. In diesem Artikel beziehen sich Aussagen zur Listenansicht sowohl auf die ListView- als auch die GridView-Steuerelemente, falls nicht anders angegeben. Möglicherweise werden Klassen wie ListView oder ListViewItem genannt. Das Präfix *List* kann jedoch durch *Grid* für das entsprechende Rastersteuerelement ersetzt werden (GridView oder GridViewItem). 

ListView und GridView bieten viele Vorteile für das Arbeiten mit Sammlungen. Sie sind einerseits leicht zu implementieren und bieten andererseits eine einfache Benutzeroberfläche, Interaktion und Scrolling, während sie trotzdem leicht anpassbar sind. ListView und GridView können an vorhandene dynamische Datenquellen gebunden werden, oder es können hart codierte Daten im eigentlichen XAML-Code bzw. im CodeBehind übergeben werden. 

Diese beiden Steuerelemente eignen sich für viele Anwendungsfälle, funktionieren aber im Allgemeinen am besten mit Sammlungen, in denen alle Elemente die gleiche Grundstruktur und Darstellung sowie das gleiche Interaktionsverhalten aufweisen – d.h., sie sollten beim Anklicken alle die gleiche Aktion ausführen (einen Link öffnen, navigieren usw).


## <a name="differences-between-listview-and-gridview"></a>Unterschiede zwischen ListView und GridView

### <a name="listview"></a>ListView
ListView zeigt Daten vertikal in einer einzelnen Spalte an. ListView eignet sich besser für Elemente, deren Schwerpunkt auf Text liegt, und für Sammlungen, die von oben nach unten gelesen werden sollen (d.h. alphabetisch sortiert sind). Zu den gängigen Anwendungsfällen für ListView gehören Listen von Nachrichten und Suchergebnissen. Für Sammlungen, die in mehreren Spalten oder in einem Tabellenformat angezeigt werden müssen, sollte _nicht_ ListView, sondern stattdessen ein [DataGrid](/windows/communitytoolkit/controls/datagrid)-Steuerelement verwendet werden.

![Eine Listenansicht mit gruppierten Daten](images/listview-grouped-example-resized-final.png)

### <a name="gridview"></a>GridView
GridView zeigt eine Sammlung von Elementen in Zeilen und Spalten an, für die ein horizontaler Bildlauf durchgeführt werden kann. Daten werden horizontal angeordnet, bis die Spalten gefüllt sind. Anschließend wird mit der nächsten Zeile fortgefahren. GridView eignet sich besser für Elemente, deren Schwerpunkt Bilder bilden, und für Sammlungen, die von einer Seite zur anderen gelesen werden können oder nicht in einer bestimmten Reihenfolge sortiert sind. Ein gängiger Anwendungsfall für GridView ist ein Foto- oder Produktkatalog.

![Beispiel einer Inhaltsbibliothek](images/gridview-simple-example-final.png)

## <a name="which-collection-control-should-you-use-a-comparison-with-itemsrepeater"></a>Welches Steuerelement sollten Sie verwenden? Ein Vergleich mit ItemsRepeater

ListView und GridView sind Steuerelemente, die sofort einsatzfähig sind und dank eigener integrierter Benutzeroberfläche und Bedienlogik beliebige Sammlungen anzeigen können. Das [ItemsRepeater](./items-repeater.md)-Steuerelement wird ebenfalls zum Anzeigen von Sammlungen verwendet, es wurde aber als Baustein zum Erstellen eines benutzerdefinierten Steuerelements erstellt, das genau Ihren Anforderungen an die Benutzeroberfläche entspricht. Die wichtigsten Unterschiede, die Ihre Wahl des Steuerelements bestimmen sollten, sind unten aufgeführt:

-   ListView und GridView sind funktionsreiche Steuerelemente, die viel bieten und nur wenig Anpassung erfordern. ItemsRepeater ist ein Baustein zum Erstellen Ihres eigenen Layoutsteuerelements und verfügt als solcher nicht über vergleichbare Funktionen und Funktionalität: Sie müssen alle erforderlichen Funktionen oder Interaktionen selbst implementieren.
-   ItemsRepeater sollte verwendet werden, wenn Sie eine in hohem Maß benutzerdefinierte Benutzeroberfläche verwenden, die sich mithilfe von ListView oder GridView nicht herstellen lässt, oder wenn Sie eine Datenquelle verwenden, die für jedes Element ein stark unterschiedliches Verhalten erfordert.


Weitere Informationen zu ItemsRepeater finden Sie in seinen [Richtlinien](./items-repeater.md) und den Seiten der [API-Dokumentation](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ListView">ListView</a> oder <a href="xamlcontrolsgallery:/item/GridView">GridView</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-listview-or-gridview"></a>Erstellen einer ListView oder GridView

ListView und GridView sind beide [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol)-Typen, sie können also eine Sammlung von Elementen beliebigen Typs enthalten. Eine ListView oder GridView muss über Elemente in ihrer Sammlung [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) verfügen, um irgendetwas auf dem Bildschirm darstellen zu können. Um die Ansicht auszufüllen, können Sie der Sammlung [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) direkt Elemente hinzufügen oder die Eigenschaft [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) auf eine Datenquelle festlegen. 

> [!IMPORTANT]
> Sie können Items oder ItemsSource zum Ausfüllen der Liste verwenden, jedoch nicht beide zugleich. Wenn Sie die ItemsSource-Eigenschaft festlegen und dann ein Element in XAML hinzufügen, wird das hinzugefügte Element ignoriert. Wenn Sie die ItemsSource-Eigenschaft festlegen und der Items-Sammlung ein Element in Code hinzufügen, wird eine Ausnahme ausgelöst.

Viele Beispiele in diesem Artikel füllen der Einfachheit halber die `Items`-Sammlung direkt aus. Häufiger stammen die Elemente in einer Liste jedoch aus einer dynamischen Quelle, z. B. einer Liste von Büchern aus einer Onlinedatenbank. Verwenden Sie für diesen Zweck die `ItemsSource`-Eigenschaft. 

### <a name="add-items-to-a-listview-or-gridview"></a>Hinzufügen von Elementen zu einer ListView oder GridView

Sie können der [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)-Sammlung einer ListView oder GridView Elemente mithilfe von XAML oder Code hinzufügen, was zum gleichen Ergebnis führt. In der Regel fügen Sie Elemente mithilfe von XAML hinzu, wenn Sie nur über eine geringe Anzahl von Elementen verfügen, die sich nicht ändern und einfach definiert werden können, oder wenn die Elemente zur Laufzeit im Code generiert werden. 

<u>Methode 1: Hinzufügen von Elementen zur Items-Sammlung</u>
#### <a name="option-1-add-items-through-xaml"></a>Option 1: Hinzufügen von Elementen mithilfe von XAML
```xml
<!-- No corresponding C# code is needed for this example. -->

<ListView x:Name="Fruits"> 
   <x:String>Apricot</x:String> 
   <x:String>Banana</x:String> 
   <x:String>Cherry</x:String> 
   <x:String>Orange</x:String> 
   <x:String>Strawberry</x:String> 
</ListView>  
```


#### <a name="option-2-add-items-through-c"></a>Option 2: Hinzufügen von Elementen mithilfe von C#

##### <a name="c-code"></a>C#-Code:
```csharp
// Create a new ListView and add content. 
ListView Fruits = new ListView(); 
Fruits.Items.Add("Apricot"); 
Fruits.Items.Add("Banana"); 
Fruits.Items.Add("Cherry"); 
Fruits.Items.Add("Orange"); 
Fruits.Items.Add("Strawberry");
 
// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file).
FruitsPanel.Children.Add(Fruits); 
```

##### <a name="corresponding-xaml-code"></a>Entsprechender XAML-Code:
```xml
<StackPanel Name="FruitsPanel"></StackPanel>
```
Beide oben aufgeführten Optionen führen zu der gleichen ListView, die unten dargestellt ist:

![Screenshot einer einfachen Listenansicht mit einer Liste von Obstsorten.](images/listview-basic-code-example2.png)
<br/>
<u>Methode 2: Hinzufügen von Elementen durch Festlegen der ItemsSource</u>

In der Regel verwenden Sie eine ListView oder GridView, um Daten aus Quellen wie einer Datenbank oder dem Internet anzuzeigen. Um eine ListView/GridView aus einer Datenquelle zu füllen, legen Sie deren Eigenschaft [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) auf eine Sammlung mit Datenelementen fest. Diese Methode funktioniert besser, wenn Ihre ListView oder GridView benutzerdefinierte Klassenobjekte enthalten soll, wie in den Beispielen unten dargestellt.

#### <a name="option-1-set-itemssource-in-c"></a>Option 1: Festlegen von ItemsSource in C#
Dies ist die ItemsSource der Listenansicht im Code, direkt auf eine Instanz einer Sammlung festgelegt. 

##### <a name="c-code"></a>C#-Code:
```csharp 
// Class defintion should be provided within the namespace being used, outside of any other classes.

this.InitializeComponent();

// Instead of adding hard coded items to an ObservableCollection as shown below, 
//the data could be pulled asynchronously from a database or the internet.
ObservableCollection<Contact> Contacts = new ObservableCollection<Contact>();

// Contact objects are created by providing a first name, last name, and company for the Contact constructor.
// They are then added to the ObservableCollection Contacts.
Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));

// Create a new ListView (or GridView) for the UI, add content by setting ItemsSource
ListView ContactsLV = new ListView();
ContactsLV.ItemsSource = Contacts;

// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file)
ContactPanel.Children.Add(ContactsLV);
```

##### <a name="xaml-code"></a>XAML-Code:
```xml
<StackPanel x:Name="ContactPanel"></StackPanel>
```

#### <a name="option-2-set-itemssource-in-xaml"></a>Option 2: Festlegen von ItemsSource in XAML
Sie können die ItemsSource-Eigenschaft auch an eine Sammlung im XAML-Code binden. Hier wird ItemsSource an eine öffentliche Eigenschaft mit dem Namen `Contacts` gebunden, die die private Datensammlung `_contacts` der Seite verfügbar macht.

**XAML**
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}"/>
```

**C#**
```csharp
// Class defintion should be provided within the namespace being used, outside of any other classes.
// These two declarations belong outside of the main page class.
private ObservableCollection<Contact> _contacts = new ObservableCollection<Contact>();

public ObservableCollection<Contact> Contacts
{
    get { return this._contacts; }
}

// This method should be defined within your main page class.
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
    Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
    Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));
}
```

Beide oben aufgeführte Optionen führen zu der gleichen ListView, die unten dargestellt ist. Die ListView stellt nur die Zeichenfolgendarstellung der einzelnen Elemente dar, da wir keine Datenvorlage bereitgestellt haben.

![Eine einfache Listenansicht mit festgelegter ItemsSource](images/listview-basic-code-example-final.png)

> [!IMPORTANT]
> Ohne definierte Datenvorlage werden benutzerdefinierte Klassenobjekte in der ListView lediglich mit ihrem Zeichenfolgenwert dargestellt, wenn für sie eine [ToString()](/uwp/api/windows.foundation.istringable.tostring)-Methode definiert wurde.

 Der nächste Abschnitt befasst sich im Detail mit der ordnungsgemäßen visuellen Darstellung einfacher und benutzerdefinierter Klassenelemente in einer ListView oder GridView.

Weitere Informationen zur Datenbindung finden Sie unter [Übersicht über Datenbindung](../../data-binding/data-binding-quickstart.md).

> [!NOTE]
> Wenn Sie gruppierte Daten in der Listenansicht anzeigen müssen, müssen Sie an eine [CollectionViewSource](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) binden. Die CollectionViewSource dient als Proxy für die Sammlungsklasse in XAML und ermöglicht die Unterstützung für Gruppierungen. Weitere Informationen finden Sie unter [CollectionViewSource](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource).

## <a name="customizing-the-look-of-items-with-a-datatemplate"></a>Anpassen der Darstellung Elementen mit einer Datenvorlage

Eine Datenvorlage in einer ListView oder GridView definiert, wie die Elemente/Daten visualisiert werden. Datenelemente werden in der ListView standardmäßig als Zeichenfolgendarstellung des Datenobjekts angezeigt, an das sie gebunden sind. Sie können die Zeichenfolgendarstellung einer bestimmten Eigenschaft des Datenelements anzeigen, indem Sie den [DisplayMemberPath](/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) zur Eigenschaft festlegen.

In der Regel möchten Sie jedoch eine ansprechendere Darstellung Ihrer Daten anzeigen. Um genau anzugeben, wie Elemente in der ListView/GridView angezeigt werden, erstellen Sie eine [DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate). Der XAML-Code in der DataTemplate definiert das Layout und die Darstellung von Steuerelementen, die zum Anzeigen eines einzelnen Elements verwendet werden. Die Steuerelemente im Layout können an Eigenschaften eines Datenobjekts gebunden werden. Es ist auch möglich, statischen Inhalt intern zu definieren. 

> [!NOTE]
> Wenn Sie die [x:Bind-Markuperweiterung](../../xaml-platform/x-bind-markup-extension.md) in DataTemplate verwenden, müssen Sie den Datentyp (`x:DataType`) für „DataTemplate“ angeben.

#### <a name="simple-listview-data-template"></a>Einfache ListView-Datenvorlage
In diesem Beispiel ist das Datenelement eine einfache Zeichenfolge. Eine DataTemplate wird inline innerhalb der ListView-Definition definiert, um links neben der Zeichenfolge ein Bild einzufügen und die Zeichenfolge blaugrün anzuzeigen. Dies ist die gleiche ListView, unter Verwendung von Methode 1 und Option 1 (oben angeführt) erstellt.

**XAML**
```XML
<!--No corresponding C# code is needed for this example.-->
<ListView x:Name="FruitsList">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="x:String">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="47"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <Image Source="Assets/placeholder.png" Width="32" Height="32"
                                HorizontalAlignment="Left" VerticalAlignment="Center"/>
                            <TextBlock Text="{x:Bind}" Foreground="Teal" FontSize="14" 
                                Grid.Column="1" VerticalAlignment="Center"/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
                <x:String>Apricot</x:String>
                <x:String>Banana</x:String>
                <x:String>Cherry</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </ListView>

```

So sehen die Datenelemente aus, wenn sie mit dieser Datenvorlage in einer ListView angezeigt werden:

![Elemente in der ListView mit einer Datenvorlage](images/listview-w-datatemplate1-final.png)

#### <a name="listview-data-template-for-custom-class-objects"></a>ListView-Datenvorlage für benutzerdefinierte Klassenobjekte
In diesem Beispiel ist das Datenelement ein Kontaktobjekt. Eine DataTemplate wird inline innerhalb der ListView-Definition definiert, um links neben dem Kontaktnamen und der Firma das Bild des Kontakts einzufügen. Die ListView wurde mithilfe von Methode 2 und Option 2 erstellt, die oben angeführt sind.
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Contact">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Grid.RowSpan="2" Source="Assets/grey-placeholder.png" Width="32"
                    Height="32" HorizontalAlignment="Center" VerticalAlignment="Center"></Image>
                <TextBlock Grid.Column="1" Text="{x:Bind Name}" Margin="12,6,0,0" 
                    Style="{ThemeResource BaseTextBlockStyle}"/>
                <TextBlock  Grid.Column="1" Grid.Row="1" Text="{x:Bind Company}" Margin="12,0,0,6" 
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

So sehen die Datenelemente aus, wenn sie mithilfe dieser Datenvorlage in einer ListView angezeigt werden:

![Benutzerdefinierte Klassenelemente in der ListView mit einer Datenvorlage](images/listview-customclass-datatemplate-final.png)

Datenvorlagen sind der bevorzugte Weg für die Definition des Aussehens Ihrer ListView. Sie können auch eine erhebliche Auswirkung auf die Leistung haben, wenn Ihre Liste eine große Anzahl von Elementen enthält.  

Ihre Datenvorlage kann innerhalb der ListView/GridView-Definition inline (oben dargestellt) oder separat in einem Resources-Abschnitt definiert werden. Wenn sie außerhalb der eigentlichen ListView/GridView definiert wird, muss die DataTemplate mit einem [x:Key](../../xaml-platform/x-key-attribute.md)-Attribut versehen und mithilfe dieses Schlüssels der [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)-Eigenschaft der ListView oder GridView zugewiesen werden.

Weitere Informationen und Beispiele zur Verwendung von Datenvorlagen und Elementcontainern zur Definition des Aussehens von Elementen in Ihrer Liste oder Ihrem Raster finden Sie unter [Elementcontainer und Vorlagen](item-containers-templates.md). 

## <a name="change-the-layout-of-items"></a>Ändern des Layouts von Elementen

Wenn Sie Elemente zu einer ListView oder GridView hinzufügen, bricht das Steuerelement automatisch alle Elemente in einem Elementcontainer um und ordnet anschließend alle Elementcontainer an. Die Anordnung dieser Elementcontainer ist von [ItemsPanel](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) des Steuerelements abhängig.  
- Standardmäßig verwendet **ListView** ein [ItemsStackPanel](/uwp/api/windows.ui.xaml.controls.itemsstackpanel), das eine vertikale Liste wie diese erzeugt.

![Screenshot einer einfachen Listenansicht mit einer Liste mit Elementen.](images/listview-simple.png)

- **GridView** verwendet ein [ItemsWrapGrid](/uwp/api/windows.ui.xaml.controls.itemswrapgrid), das Elemente horizontal hinzufügt, diese anschließend umbricht und einen vertikalen Bildlauf ausführt, wie hier gezeigt.

![Eine einfache Rasteransicht](images/gridview-simple.png)

Sie können das Layout von Elementen ändern, indem Sie Eigenschaften im Elementpanel anpassen oder das Standardpanel durch ein anderes Panel ersetzen.

> [!NOTE]
> Achten Sie darauf, dass Sie Virtualisierung nicht deaktivieren, wenn Sie ItemsPanel ändern. Sowohl **ItemsStackPanel** als auch **ItemsWrapGrid** unterstützen Virtualisierung, damit diese sicher verwendet werden können. Wenn Sie ein anderes Panel verwenden, könnten Sie die Virtualisierung deaktivieren und die Leistung der Listenansicht beeinträchtigen. Weitere Informationen finden Sie in den Artikeln zur Listenansicht unter [Leistung](../../debug-test-perf/performance-and-xaml-ui.md). 

In diesem Beispiel wird gezeigt, wie eine **ListView** ihre Elementcontainer in einer horizontalen Liste anordnet, indem die [Orientation](/uwp/api/windows.ui.xaml.controls.itemsstackpanel.orientation)-Eigenschaft von **ItemsStackPanel** geändert wird.
Da die Listenansicht standardmäßig den vertikalen Bildlauf verwendet, müssen Sie auch einige Eigenschaften im internen [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer) der Listenansicht anpassen, damit der Bildlauf horizontal durchgeführt wird.
- [ScrollViewer.HorizontalScrollMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) zu **Enabled** oder **Auto**
- [ScrollViewer.HorizontalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) zu **Auto** 
- [ScrollViewer.VerticalScrollMode](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) zu **Disabled** 
- [ScrollViewer.VerticalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) zu **Hidden** 

> [!IMPORTANT]
> Diese Beispiele werden mit unbeschränkter Breite der Listenansicht gezeigt, sodass die horizontalen Bildlaufleisten nicht angezeigt werden. Wenn Sie diesen Code ausführen, können Sie für ListView `Width="180"` festlegen, um die Bildlaufleisten anzuzeigen.

**XAML**
```xml
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
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

Die resultierende Liste sieht wie folgt aus.

![Eine horizontale Listenansicht](images/listview-horizontal2-final.png)

 Im nächsten Beispiel ordnet **ListView** Elemente in einer Liste an, die vertikal umbrochen wird, indem **ItemsWrapGrid** anstelle von **ItemsStackPanel** verwendet wird. 
 
> [!IMPORTANT]
> Die Höhe der Listenansicht muss eingeschränkt werden, um das Steuerelement zu zwingen, die Container umzubrechen.

**XAML**
```xml
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
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

Die resultierende Liste sieht wie folgt aus.

![Eine Listenansicht mit Rasterlayout](images/listview-itemswrapgrid2-final.png)

Wenn Sie in Ihrer Listenansicht können gruppierte Daten anzeigen möchten, bestimmt ItemsPanel die Anordnung der Elementgruppen, nicht die Anordnung der einzelnen Elemente. Wenn das horizontale ItemsStackPanel, das zuvor gezeigt wurde, zur Anzeige gruppierter Daten verwendet wird, werden die Gruppen horizontal angeordnet. Die Elemente in den einzelnen Gruppen werden jedoch weiterhin vertikal angeordnet, wie hier gezeigt.

![Eine gruppierte horizontale Listenansicht](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>Auswahl von Elementen und Interaktion

Sie können aus verschiedenen Möglichkeiten wählen, wie Benutzer mit einer Listenansicht interagieren können. Standardmäßig können Benutzer ein einzelnes Element auswählen. Sie können die Eigenschaft [SelectionMode](/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) ändern, um eine Mehrfachauswahl zu ermöglichen oder die Auswahl zu deaktivieren. Sie können die Eigenschaft [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) festlegen, sodass ein Benutzer auf ein Element klickt, um eine Aktion aufzurufen (wie bei einer Schaltfläche), statt das Element auszuwählen.

> **Hinweis**&nbsp;&nbsp;Sowohl ListView als auch GridView verwenden die Enumeration [ListViewSelectionMode](/uwp/api/windows.ui.xaml.controls.listviewselectionmode) für ihre SelectionMode-Eigenschaften. IsItemClickEnabled ist standardmäßig **False**. Daher müssen Sie es lediglich festlegen, um den Klickmodus zu aktivieren.

In der folgenden Tabelle werden die Arten gezeigt, wie Benutzer mit einer Listenansicht interagieren können und wie Sie auf die jeweilige Interaktion reagieren können.

Um diese Interaktion zu ermöglichen: | Verwenden Sie diese Einstellungen: | Behandeln Sie dieses Ereignis: | Verwenden Sie diese Eigenschaft zum Abrufen des ausgewählten Elements:
----------------------------|---------------------|--------------------|--------------------------------------------
Keine Interaktion | [SelectionMode](/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) = **None**, [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) = **False** | NICHT ZUTREFFEND | NICHT ZUTREFFEND 
Einzelauswahl | SelectionMode = **Single**, IsItemClickEnabled = **False** | [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem), [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)  
Mehrfachauswahl | SelectionMode = **Multiple**, IsItemClickEnabled = **False** | [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
Erweiterte Auswahl | SelectionMode = **Extended**, IsItemClickEnabled = **False** | [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
Klicken Sie im Menüband auf | SelectionMode = **None**, IsItemClickEnabled = **True** | [ItemClick](/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) | NICHT ZUTREFFEND 

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

Wenn SelectionMode **Single** ist, erhalten Sie das ausgewählte Datenelement aus der [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)-Eigenschaft. Sie erhalten den Index in der Sammlung des ausgewählten Elements mithilfe der [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)-Eigenschaft. Wenn kein Element ausgewählt ist, ist SelectedItem **null**, und SelectedIndex ist -1. 
 
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

Wenn SelectionMode **Multiple** oder **Extended** ist, können Sie die ausgewählten Datenelemente aus der [SelectedItems](/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)-Eigenschaft abrufen. 

Die Eigenschaften **SelectedIndex**, **SelectedItem** und **SelectedItems** sind synchronisiert. Wenn Sie beispielsweise SelectedIndex auf-1 festlegen, wird SelectedItem auf **null** festgelegt und SelectedItems ist leer. Wenn Sie SelectedItem auf **null** festlegen, wird SelectedIndex auf-1 festgelegt und SelectedItems ist leer.

Im Mehrfachauswahlmodus enthält **SelectedItem** das Element, das zuerst ausgewählt wurde, und **Selectedindex** enthält den Index des Elements, das zuerst ausgewählt wurde. 

### <a name="respond-to-selection-changes"></a>Reagieren auf Änderungen der Auswahl

Um auf Änderungen der Auswahl in einer Listenansicht zu reagieren, behandeln Sie das Ereignis [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Im Ereignishandlercode können Sie die Liste mit den ausgewählten Elementen aus der [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems)-Eigenschaft abrufen. Sie können alle Elemente abrufen, deren Auswahl aus der [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems)-Eigenschaft aufgehoben wurde. Die AddedItems- und RemovedItems-Sammlungen enthalten höchstens 1 Element, wenn der Benutzer keinen Bereich von Elementen auswählt, indem er die UMSCHALTTASTE gedrückt hält.

In diesem Beispiel werden die Behandlung des **SelectionChanged**-Ereignisses und der Zugriff auf die verschiedenen Elementsammlungen gezeigt.

**XAML**
```xml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
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
```xml
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

Die Methoden [SelectAll](/uwp/api/windows.ui.xaml.controls.listviewbase.selectall), [SelectRange](/uwp/api/windows.ui.xaml.controls.listviewbase.selectrange) und [DeselectRange](/uwp/api/windows.ui.xaml.controls.listviewbase.deselectrange) stellen eine effizientere Möglichkeit dar, die Auswahl zu ändern, als die Verwendung der Eigenschaft SelectedItems. Diese Methoden führen mithilfe von Elementindexbereichen Auswahlen durch oder heben Auswahlen auf. Elemente, die virtualisiert sind, bleiben virtualisiert, da nur der Index verwendet wird. Alle Elemente im angegebenen Bereich werden ausgewählt (bzw. die Auswahl aller Elemente im angegeben Bereich wird aufgehoben), unabhängig vom ursprünglichen Auswahlzustand. Das SelectionChanged-Ereignis tritt nur einmal für jeden Aufruf dieser Methoden auf.

> [!IMPORTANT]
> Sie sollten diese Methoden nur aufrufen, wenn die SelectionMode-Eigenschaft auf Multiple oder Extended festgelegt ist. Wenn Sie SelectRange aufrufen, wenn der Auswahlmodus Single oder None ist, wird eine Ausnahme ausgelöst.

Verwenden Sie bei der Auswahl von Elementen mit Indexbereichen die [SelectedRanges](/uwp/api/windows.ui.xaml.controls.listviewbase.selectedranges)-Eigenschaft, um alle ausgewählten Bereiche in der Liste abzurufen.

Wenn ItemsSource [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo) implementiert, und Sie diese Methoden verwenden, um die Auswahl zu ändern, werden die Eigenschaften **AddedItems** und **RemovedItems** in SelectionChangedEventArgs nicht festgelegt. Das Festlegen dieser Eigenschaften erfordert die Deaktivierung der Virtualisierung des Elementobjekts. Verwenden Sie zum Abrufen der Elemente stattdessen die Eigenschaft **SelectedRanges**.

Sie können alle Elemente in einer Sammlung auswählen, indem Sie die SelectAll-Methode aufrufen. Es gibt jedoch keine entsprechende Methode, um die Auswahl aller Elemente aufzuheben. Sie können die Auswahl aller Elemente aufheben, indem Sie DeselectRange aufrufen und einen [ItemIndexRange](/uwp/api/windows.ui.xaml.data.itemindexrange) mit einem Wert für [FirstIndex](/uwp/api/windows.ui.xaml.data.itemindexrange.firstindex) von 0 und einem Wert für [Length](/uwp/api/windows.ui.xaml.data.itemindexrange.length) übergeben, der der Anzahl der Elemente in der Sammlung entspricht. Dies wird im Beispiel unten dargestellt, zusammen mit einer Option zum Auswählen aller Elemente.

**XAML**
```xml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
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

### <a name="drag-and-drop"></a>Drag &amp; Drop

ListView- und GridView-Steuerelemente unterstützen Drag & Drop für Elemente innerhalb von ihnen und zwischen ihnen und anderen ListView- und GridView-Steuerelementen. Weitere Informationen zur Implementierung des Musters für Drag & Drop finden Sie unter [Drag & Drop](../input/drag-and-drop.md).

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [XAML-Beispiel für ListView und GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) – Veranschaulicht die ListView- und GridView-Steuerelementen.
- [XAML-Drag & Drop-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) – Veranschaulicht Drag & Drop mit dem ListView-Steuerelement.
- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Listen](lists.md)
- [Elementcontainer und Vorlagen](item-containers-templates.md)
- [Drag & Drop](../input/drag-and-drop.md)
