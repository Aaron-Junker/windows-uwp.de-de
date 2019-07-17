---
description: Sie können eine erweiterbare Strukturansicht erstellen, indem Sie die ItemsSource an eine hierarchische Datenquelle binden. Sie können jedoch auch TreeViewNode-Objekte selbst erstellen und verwalten.
title: Strukturansicht
label: Tree view
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.custom: RS5, 19H1
ms.openlocfilehash: d9f0396558186008430ccf1454e48f5e2194ee0e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66364002"
---
# <a name="treeview"></a>TreeView

Das XAML-Steuerelement „TreeView“ ermöglicht eine Hierarchieauflistung mit Knoten, die das Aus- und Einblenden von geschachtelten Elementen erlauben. Das Steuerelement kann verwendet werden, um eine Ordnerstruktur oder geschachtelte Beziehungen zwischen Elementen in der Benutzeroberfläche zu veranschaulichen.

Die TreeView-APIs unterstützen die folgenden Features:

- Die Schachtelung von N Ebenen
- Auswahl einzelner oder mehrerer Knoten
- Datenbindung an die ItemsSource-Eigenschaft für TreeView und TreeViewItem
- TreeViewItem als Stamm der TreeView-Elementvorlage
- Beliebige Inhaltstypen in einem TreeViewItem
- Drag & Drop zwischen Strukturansichten

| **Abrufen der Windows-UI-Bibliothek** |
| - |
| Dieses Steuerelement ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für UWP-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

| **Plattform-APIs** | **Windows-UI-Bibliotheks-APIs** |
| - | - |
| [TreeView-Klasse](/uwp/api/windows.ui.xaml.controls.treeview), [TreeViewNode-Klasse](/uwp/api/windows.ui.xaml.controls.treeviewnode), [TreeView.ItemsSource-Eigenschaft](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) | [TreeView-Klasse](/uwp/api/microsoft.ui.xaml.controls.treeview), [TreeViewNode-Klasse](/uwp/api/microsoft.ui.xaml.controls.treeviewnode), [TreeView.ItemsSource-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource) |

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

- Verwenden Sie eine Strukturansicht, wenn Elemente geschachtelte Elemente haben, und dann, wenn es wichtig ist, die hierarchische Beziehung zwischen Elementen und ihren gleichgestellten Elementen und Knoten zu veranschaulichen.

- Vermeiden Sie Strukturansicht, wenn die geschachtelte Beziehung eines Elements nicht im Vordergrund steht. Für die meisten Drilldown-Szenarios eignet sich eine normale ListView

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/TreeView">die App zu öffnen und TreeView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>TreeView-UI

Die Strukturansicht verwendet eine Kombination aus Einzügen und Symbolen, um die geschachtelte Beziehung zwischen übergeordneten Knoten und untergeordneten Knoten darzustellen. Ausgeblendete Knoten verwenden ein Chevron, welches nach rechts zeigt, während eingeblendete Knoten ein Chevron verwenden, das nach unten zeigt.

![Das Chevronsymbol in der Strukturansicht](images/treeview-simple.png)

Sie können ein Symbol in die Datenvorlage für Strukturansichtselemente einschließen, um Knoten darzustellen. Wenn Sie beispielsweise eine Dateisystemhierarchie anzeigen, können Sie Ordnersymbole für die übergeordneten Knoten und Dateisymbole für die untergeordneten Knoten verwenden.

![Das Chevron- und Ordnersymbol zusammen in einer Strukturansicht](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>Erstellen einer Strukturansicht

Sie können eine Strukturansicht erstellen, indem Sie die [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) an eine hierarchische Datenquelle binden. Sie können jedoch auch TreeViewNode-Objekte selbst erstellen und verwalten.

Um eine Strukturansicht zu erstellen, verwenden Sie ein [TreeView](/uwp/api/windows.ui.xaml.controls.treeview)-Steuerelement und eine Hierarchie von [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)-Objekten. Sie erstellen die Knotenhierarchie, indem Sie der [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes)-Sammlung des TreeView-Steuerelements mindestens einen Stammknoten hinzufügen. Der Children-Sammlung der einzelnen TreeViewNodes können dann weitere Knoten hinzugefügt werden. Sie können Knoten der Strukturansicht beliebig tief schachteln.

Sie können eine hierarchische Datenquelle an die [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)-Eigenschaft binden, um den Inhalt der Strukturansicht bereitzustellen, ebenso wie bei der ItemsSource der ListView. Verwenden Sie auf die gleiche Weise [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (und den optionalen [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)), um eine DataTemplate bereitzustellen, die das Element rendert.

> [!IMPORTANT]
> ItemsSource und die zugehörigen APIs erfordern Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher, oder die [Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).
>
> ItemsSource ist ein alternativer Mechanismus zu TreeView.RootNodes für das Ablegen von Inhalt im TreeView-Steuerelement. ItemsSource und RootNodes können nicht gleichzeitig festgelegt werden. Wenn Sie ItemsSource verwenden, werden Knoten für Sie erstellt, und Sie können aus der TreeView.RootNodes-Eigenschaft darauf zugreifen.

Im Folgenden finden Sie ein Beispiel für eine einfache in XAML deklarierte Strukturansicht. Sie fügen die Knoten in der Regel im Code hinzu, aber hier wird die XAML-Hierarchie angezeigt, da sie beim Visualisieren der Erstellung der Knotenhierarchie hilfreich sein kann.

```xaml
<TreeView>
    <TreeView.RootNodes>
        <TreeViewNode Content="Flavors" IsExpanded="True">
            <TreeViewNode.Children>
                <TreeViewNode Content="Vanilla"/>
                <TreeViewNode Content="Strawberry"/>
                <TreeViewNode Content="Chocolate"/>
            </TreeViewNode.Children>
        </TreeViewNode>
    </TreeView.RootNodes>
</TreeView>
```

In den meisten Fällen zeigt die Strukturansicht Daten aus einer Datenquelle an. Daher deklarieren Sie das Steuerelement der Stammstrukturansicht in der Regel in XAML, fügen die TreeViewNode-Objekte jedoch im Code oder per Datenbindung hinzu.

### <a name="bind-to-a-hierarchical-data-source"></a>Binden an eine hierarchische Datenquelle

Zum Erstellen einer Strukturansicht mit Datenbindung legen Sie eine hierarchische Sammlung auf die TreeView.ItemsSource-Eigenschaft fest. Legen Sie anschließend in der ItemTemplate die Sammlung der untergeordneten Elemente auf die TreeViewItem.ItemsSource-Eigenschaft fest.

```xaml
<TreeView ItemsSource="{x:Bind DataSource}">
    <TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <TreeViewItem ItemsSource="{x:Bind Children}"
                          Content="{x:Bind Name}"/>
        </DataTemplate>
    </TreeView.ItemTemplate>
</TreeView>
```

Den vollständigen Code finden Sie unter _Strukturansicht mit Datenbindung_ im Abschnitt „Beispiele“.

#### <a name="items-and-item-containers"></a>Elemente und Elementcontainer

Wenn Sie TreeView.ItemsSource verwenden, sind diese APIs verfügbar, um den Knoten bzw. das Datenelement aus dem Container (und umgekehrt) abzurufen.

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | Ruft das Datenelement für den angegebenen TreeViewItem-Container ab. |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | Ruft den TreeViewItem-Container für das angegebene Datenelement ab. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | Ruft den TreeViewNode für den angegebenen TreeViewItem-Container ab. |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | Ruft den TreeViewItem-Container für den angegebenen TreeViewNode ab. |

### <a name="manage-tree-view-nodes"></a>Verwalten von Strukturansichtsknoten

Diese Strukturansicht ist identisch mit der zuvor in XAML erstellten Strukturansicht, die Knoten wurden jedoch im Code erstellt.

```xaml
<TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    TreeViewNode rootNode = new TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New TreeViewNode With {.Content = "Vanilla"})
        .Add(New TreeViewNode With {.Content = "Strawberry"})
        .Add(New TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

Diese APIs sind für die Verwaltung der Datenhierarchie der Strukturansicht verfügbar.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Eine Strukturansicht kann einen oder mehrere Stammknoten aufweisen. Fügen Sie der RootNodes-Sammlung ein TreeViewNode-Objekt hinzu, um einen Stammknoten zu erstellen. Das **übergeordnete Element** eines Stammknotens hat immer den Wert **null**. Die **Tiefe** eines Stammknotens beträgt 0. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Fügen Sie der Children-Sammlung eines übergeordneten Knotens TreeViewNode-Objekte hinzu, um eine Knotenhierarchie zu erstellen. Ein Knoten ist das **übergeordnete Element** aller Knoten in seiner **Children**-Sammlung. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | Ist **true**, wenn der Knoten untergeordnete Elemente erkannt hat. **false** gibt einen leeren Ordner oder ein Element an. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Verwenden Sie diese Eigenschaft, wenn Knoten beim Erweitern aufgefüllt werden. Weitere Informationen finden Sie unter _Auffüllen eines Knotens, wenn er erweitert wird_ weiter unten in diesem Artikel. |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Gibt die Entfernung eines untergeordneten Knotens vom Stammknoten an. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Ruft den TreeViewNode ab, der die **Children**-Sammlung besitzt, zu der dieser Knoten gehört. |

Die Strukturansicht verwendet die Eigenschaften **HasChildren** und **HasUnrealizedChildren**, um zu ermitteln, ob das Symbol „Erweitern/Reduzieren“ angezeigt wird. Wenn eine der Eigenschaften den Wert **true** hat, wird das Symbol angezeigt, andernfalls nicht.

## <a name="tree-view-node-content"></a>Inhalt des Knotens der Strukturansicht

Sie können das Datenelement, das ein Knoten in der Strukturansicht darstellt, in dessen [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content)-Eigenschaft speichern.

In den vorherigen Beispielen war der Inhalt ein einfacher Zeichenfolgenwert. Hier stellt ein Knoten der Strukturansicht den Ordner „Pictures“ des Benutzers dar. Daher ist die Bildbibliothek [StorageFolder](/uwp/api/windows.storage.storagefolder) der Content-Eigenschaft des Knotens zugewiesen.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
TreeViewNode pictureNode = new TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New TreeViewNode With {.Content = picturesFolder}
```

Sie können eine [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) angeben, um festzulegen, wie das Datenelement in der Strukturansicht angezeigt wird.

> [!NOTE]
> In Windows 10, Version 1803, müssen Sie Vorlage für das TreeView-Steuerelement umschreiben und eine benutzerdefinierte ItemTemplate festlegen, falls der Inhalt keine Zeichenfolge ist. Weitere Informationen finden Sie im vollständigen Beispiel am Ende dieses Artikels. Legen Sie in späteren Versionen die [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)-Eigenschaft fest.

### <a name="item-container-style"></a>Elementcontainerstil

Egal, ob Sie ItemsSource oder RootNodes verwenden, bei den tatsächlichen Elementen zum Anzeigen der einzelnen Knoten – als „Container“ bezeichnet – handelt es sich um ein [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)-Objekt. Sie können den Container mit der ItemContainerStyle-Eigenschaft oder der ItemContainerStyleSelector-Eigenschaft der TreeView formatieren.

### <a name="item-template-selectors"></a>Elementvorlagenselektor

Sie können je nach Elementtyp eine andere DataTemplate für die Strukturansichtselemente festlegen. In einer Datei-Explorer-App können Sie beispielsweise eine Datenvorlage für Ordner und eine andere für Dateien verwenden.

![Ordner und Dateien, für die unterschiedliche Datenvorlagen verwendet werden](images/treeview-icons.png)

Hier ist ein Beispiel für das Erstellen und Verwenden eines Elementvorlagenselektors.

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <TreeView ItemsSource="{x:Bind DataSource}"
              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

## <a name="interacting-with-a-tree-view"></a>Interagieren mit einer Strukturansicht

Sie können eine Strukturansicht konfigurieren, damit Benutzer auf verschiedene Weise mit ihr interagieren können:

- Knoten erweitern oder reduzieren
- Einfach- oder Mehrfachauswahlelemente
- Element durch Klicken aufrufen

### <a name="expandcollapse"></a>Erweitern/Reduzieren

Jeder Strukturknoten, der über untergeordnete Elemente verfügt, kann durch Klicken auf das Erweitern/Reduzieren-Symbol erweitert oder reduziert werden. Knoten lassen sich auch programmgesteuert erweitern oder reduzieren und reagieren, wenn der Zustand eines Knotens geändert wird.

#### <a name="expandcollapse-a-node-programmatically"></a>Programmgesteuertes Erweitern/Reduzieren von Knoten

Es gibt zwei Methoden, den Knoten einer Strukturansicht im Code zu erweitern oder zu reduzieren.

- Die [TreeView](/uwp/api/windows.ui.xaml.controls.treeview)-Klasse verfügt über die [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse)-Methode und die [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand)-Methode. Wenn Sie diese Methoden aufrufen, übergeben Sie den TreeViewNode, den Sie erweitern oder reduzieren möchten.

- Jeder [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) verfügt über die [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded)-Eigenschaft. Mit dieser Eigenschaft können Sie den Zustand eines Knotens überprüfen; Sie können sie auch so festlegen, dass sein Zustand geändert wird. Diese Eigenschaft lässt sich auch in XAML festlegen, um den Anfangszustand eines Knotens festzulegen.

### <a name="fill-a-node-when-its-expanding"></a>Auffüllen eines Knotens, wenn er erweitert wird

Sie müssen möglicherweise eine große Anzahl von Knoten in der Strukturansicht anzeigen, oder Sie wissen möglicherweise nicht vorab, wie viele Knoten diese aufweisen wird. Das TreeView-Steuerelement ist nicht virtualisiert. Daher können Sie Ressourcen verwalten, indem Sie jeden Knoten beim Erweitern auffüllen und die untergeordneten Knoten beim Reduzieren entfernen.

Behandeln Sie das [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand)-Ereignis, und verwenden Sie die [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren)-Eigenschaft, um einem Knoten untergeordnete Elemente hinzuzufügen, wenn er erweitert wird. Die HasUnrealizedChildren-Eigenschaft gibt an, ob der Knoten aufgefüllt werden muss oder ob dessen Children-Sammlung bereits aufgefüllt wurde. Es ist wichtig, dass dieser Wert nicht durch den TreeViewNode festgelegt, sondern von Ihnen im App-Code verwaltet wird.

Im Folgenden finden Sie ein Beispiel für diese verwendeten APIs. Der vollständige Beispielcode ist am Ende dieses Artikels aufgeführt, einschließlich der Implementierung von „FillTreeNode”.

```csharp
private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

Es ist nicht erforderlich, Sie können jedoch auch das [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed)-Ereignis behandeln und die untergeordneten Knoten entfernen, wenn der übergeordnete Knoten geschlossen wird. Dies kann wichtig sein, wenn die Strukturansicht viele Knoten aufweist oder die Knotendaten viele Ressourcen nutzen. Bedenken Sie potenzielle Auswirkungen auf die Leistung durch das Ausfüllen eines Knotens bei jedem Öffnen im Vergleich gegenüber der Beibehaltung der untergeordneten Elemente in einem geschlossenen Knoten. Welche Option jeweils geeignet ist, hängt von Ihrer App ab.

Nachfolgend finden Sie ein Beispiel für einen Handler für das Collapsed-Ereignis.

```csharp
private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>Aufrufen eines Elements

Ein Benutzer kann eine Aktion (behandelt das Element wie eine Schaltfläche) aufrufen, statt das Element auszuwählen. Sie behandeln das [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked)-Ereignis, um auf diese Benutzerinteraktion zu reagieren.

> [!NOTE]
> Im Gegensatz zur ListView, die über die [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled)-Eigenschaft verfügt, ist das Aufrufen eines Elements für die Strukturansicht immer aktiviert. Sie können weiterhin auswählen, ob das Ereignis behandelt werden soll oder nicht.

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)-Klasse**

Die ItemInvoked-Ereignisargumente ermöglichen Ihnen den Zugriff auf das aufgerufene Element. Die [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem)-Eigenschaft verfügt über den Knoten, der aufgerufen wurde. Sie können eine Umwandlung in TreeViewNode durchführen, um das Datenelement aus der TreeViewNode.Content-Eigenschaft abzurufen.

Nachfolgend finden Sie ein Beispiel für einen ItemInvoked-Ereignishandler. Das Datenelement ist ein [IStorageItem](/uwp/api/windows.storage.istorageitem), und in diesem Beispiel werden lediglich einige Informationen über die Datei und die Struktur angezeigt. Wenn der Knoten ein Ordnerknoten ist, wird der Knoten gleichzeitig erweitert bzw. reduziert. Andernfalls wird der Knoten nur dann erweitert oder reduziert, wenn auf die Chevron-Schaltfläche geklickt wird.

```csharp
private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>Elementauswahl

Das TreeView-Steuerelement unterstützt sowohl die Einzelauswahl als auch die Mehrfachauswahl. Die Auswahl von Knoten ist standardmäßig deaktiviert, Sie können die [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode)-Eigenschaft jedoch festlegen, um die Auswahl von Knoten zu ermöglichen. Die [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode)-Werte sind **Keine**, **Einzelne** und **Mehrere**.

#### <a name="multiple-selection"></a>Mehrfachauswahl

Wenn die Mehrfachauswahl aktiviert ist, wird ein Kontrollkästchen neben jedem Knoten einer Strukturansicht angezeigt, und ausgewählte Elemente werden hervorgehoben. Ein Benutzer kann ein Element mithilfe eines Kontrollkästchens aktivieren oder deaktivieren. Das Element kann weiterhin aufgerufen werden, indem darauf geklickt wird.

Beim Aktivieren bzw. Deaktivieren eines übergeordneten Knotens werden alle darunter befindlichen untergeordneten Knoten aktiviert bzw. deaktiviert. Wenn einige (jedoch nicht alle) untergeordnete Elemente unter einem übergeordneten Knoten ausgewählt werden, wird das Kontrollkästchen für den übergeordneten Knoten als undefiniert angezeigt (gefüllt mit einem schwarzen Kästchen).

![Mehrfachauswahl in einer Strukturansicht](images/treeview-selection.png)

Ausgewählte Knoten werden der [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes)-Sammlung der Strukturansicht hinzugefügt. Sie können die [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall)-Methode aufrufen, um alle Knoten in einer Strukturansicht auszuwählen.

> [!NOTE]
> Wenn Sie **SelectAll** aufrufen, werden alle erkannten Knoten ungeachtet des SelectionMode ausgewählt. Um ein einheitliches Benutzererlebnis zu ermöglichen, sollten Sie SelectAll nur dann aufrufen, wenn SelectionMode auf **Mehrere** festgelegt ist.

#### <a name="selection-and-realizedunrealized-nodes"></a>Auswahl und erkannte/nicht erkannte Knoten

Wenn die Strukturansicht nicht erkannte Knoten aufweist, werden sie bei der Auswahl nicht berücksichtigt. Beachten Sie bei der Auswahl mit nicht erkannten Knoten Folgendes.

- Wenn ein Benutzer einen übergeordneten Knoten auswählt, werden auch alle erkannten untergeordneten Elemente ausgewählt, die sich unter diesem übergeordneten Element befinden. Wenn alle untergeordneten Knoten ausgewählt werden, wird der übergeordnete Knoten ebenfalls ausgewählt.
- Bei der SelectAll-Methode werden der SelectedNodes-Sammlung nur erkannte Knoten hinzugefügt.
- Wenn ein übergeordneter Knoten mit nicht erkannten untergeordneten Elementen ausgewählt wird, werden die untergeordneten Elemente ausgewählt, sobald sie erkannt werden.

## <a name="code-examples"></a>Codebeispiele

### <a name="tree-view-using-xaml"></a>Strukturansicht mit XAML

Dieses Beispiel zeigt, wie in XAML eine einfache Struktur für eine Strukturansicht erstellt wird. Die Strukturansicht zeigt in Kategorien angeordnete Eissorten und Garnierungen, die der Benutzer auswählen kann. Die Mehrfachauswahl ist aktiviert, und wenn der Benutzer auf eine Schaltfläche klickt, werden die SelectedItems in der Haupt-App-UI angezeigt.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView x:Name="DessertTree" SelectionMode="Multiple">
                <TreeView.RootNodes>
                    <TreeViewNode Content="Flavors" IsExpanded="True">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Vanilla"/>
                            <TreeViewNode Content="Strawberry"/>
                            <TreeViewNode Content="Chocolate"/>
                        </TreeViewNode.Children>
                    </TreeViewNode>

                    <TreeViewNode Content="Toppings">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Candy">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Chocolate"/>
                                    <TreeViewNode Content="Mint"/>
                                    <TreeViewNode Content="Sprinkles"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Fruits">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Mango"/>
                                    <TreeViewNode Content="Peach"/>
                                    <TreeViewNode Content="Kiwi"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Berries">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Strawberry"/>
                                    <TreeViewNode Content="Blueberry"/>
                                    <TreeViewNode Content="Blackberry"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                        </TreeViewNode.Children>
                    </TreeViewNode>
                </TreeView.RootNodes>
            </TreeView>
        </SplitView.Pane>

        <StackPanel Grid.Column="1" Margin="12,0">
            <Button Content="Select all" Click="SelectAllButton_Click"/>
            <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
            <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
            <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="ToppingList"/>
        </StackPanel>
    </SplitView>
</Grid>
```

```csharp
private void OrderButton_Click(object sender, RoutedEventArgs e)
{
    FlavorList.Text = string.Empty;
    ToppingList.Text = string.Empty;

    foreach (TreeViewNode node in DessertTree.SelectedNodes)
    {
        if (node.Parent.Content?.ToString() == "Flavors")
        {
            FlavorList.Text += node.Content + "; ";
        }
        else if (node.HasChildren == false)
        {
            ToppingList.Text += node.Content + "; ";
        }
    }
}

private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (DessertTree.SelectionMode == TreeViewSelectionMode.Multiple)
    {
        DessertTree.SelectAll();
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>Strukturansicht mit Datenbindung

In diesem Beispiel wird veranschaulicht, wie Sie dieselbe Strukturansicht wie im vorherigen Beispiel erstellen. Die Datenhierarchie wird jedoch nicht in XAML erstellt. Stattdessen werden die Daten im Code erstellt und an die ItemsSource-Eigenschaft der Strukturansicht gebunden. (Die im vorigen Beispiel gezeigten Schaltflächen-Ereignishandler gelten auch für dieses Beispiel.)

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView Name="DessertTree"
                      SelectionMode="Multiple"
                      ItemsSource="{x:Bind DataSource}">
                <TreeView.ItemTemplate>
                    <DataTemplate x:DataType="local:Item">
                        <TreeViewItem ItemsSource="{x:Bind Children}"
                                      Content="{x:Bind Name}"/>
                    </DataTemplate>
                </TreeView.ItemTemplate>
            </TreeView>
        </SplitView.Pane>

        <StackPanel Grid.Column="1" Margin="12,0">
            <Button Content="Select all" Click="SelectAllButton_Click"/>
            <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
            <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
            <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="ToppingList"/>
        </StackPanel>
    </SplitView>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

    public MainPage()
    {
        this.InitializeComponent();
        DataSource = GetDessertData();
    }

    private ObservableCollection<Item> GetDessertData()
    {
        var list = new ObservableCollection<Item>();
        Item flavorsCategory = new Item()
        {
            Name = "Flavors",
            Children =
            {
                new Item() { Name = "Vanilla" },
                new Item() { Name = "Strawberry" },
                new Item() { Name = "Chocolate" }
            }
        };
        Item toppingsCategory = new Item()
        {
            Name = "Toppings",
            Children =
            {
                new Item()
                {
                    Name = "Candy",
                    Children =
                    {
                        new Item() { Name = "Chocolate" },
                        new Item() { Name = "Mint" },
                        new Item() { Name = "Sprinkles" }
                    }
                },
                new Item()
                {
                    Name = "Fruits",
                    Children =
                    {
                        new Item() { Name = "Mango" },
                        new Item() { Name = "Peach" },
                        new Item() { Name = "Kiwi" }
                    }
                },
                new Item()
                {
                    Name = "Berries",
                    Children =
                    {
                        new Item() { Name = "Strawberry" },
                        new Item() { Name = "Blueberry" },
                        new Item() { Name = "Blackberry" }
                    }
                }
            }
        };

        list.Add(flavorsCategory);
        list.Add(toppingsCategory);
        return list;
    }

    // Button event handlers...
}

public class Item
{
    public string Name { get; set; }
    public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

    public override string ToString()
    {
        return Name;
    }
}
```

### <a name="pictures-and-music-library-tree-view"></a>Strukturansicht für Bild- und Musikbibliotheken

Dieses Beispiel zeigt, wie eine Strukturansicht erstellt wird, die Inhalt und Struktur der Bild- und Musikbibliotheken von Benutzern anzeigt. Da die Anzahl der Elemente vorab nicht bekannt sein kann, wird jeder Knoten beim Erweitern aufgefüllt und beim Reduzieren geleert.

Eine benutzerdefinierte Elementvorlage wird verwendet, um die Datenelemente vom Typ [IStorageItem](/uwp/api/windows.storage.istorageitem) anzuzeigen.

> [!IMPORTANT]
> Für den Code in diesem Beispiel sind die Funktionen PicturesLibrary und MusicLibrary erforderlich. Weitere Informationen zum Dateizugriff finden Sie unter [Berechtigungen für den Dateizugriff](../../files/file-access-permissions.md), [Aufzählen und Abfragen von Dateien und Ordnern](../../files/quickstart-listing-files-and-folders.md) und [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewApp1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewApp1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock
                    Text="{Binding Content.DisplayName}"
                    HorizontalAlignment="Left"
                    VerticalAlignment="Center"
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <Style TargetType="TreeView">
            <Setter Property="IsTabStop" Value="False" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="TreeView">
                        <TreeViewList x:Name="ListControl"
                                      ItemTemplate="{StaticResource TreeViewItemDataTemplate}"
                                      ItemContainerStyle="{StaticResource TreeViewItemStyle}"
                                      CanDragItems="True"
                                      AllowDrop="True"
                                      CanReorderItems="True">
                            <TreeViewList.ItemContainerTransitions>
                                <TransitionCollection>
                                    <ContentThemeTransition />
                                    <ReorderThemeTransition />
                                    <EntranceThemeTransition IsStaggeringEnabled="False" />
                                </TransitionCollection>
                            </TreeViewList.ItemContainerTransitions>
                        </TreeViewList>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();
    InitializeTreeView();
}

private void InitializeTreeView()
{
    // A TreeView can have more than 1 root node. The Pictures library
    // and the Music library will each be a root node in the tree.
    // Get Pictures library.
    StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
    TreeViewNode pictureNode = new TreeViewNode();
    pictureNode.Content = picturesFolder;
    pictureNode.IsExpanded = true;
    pictureNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(pictureNode);
    FillTreeNode(pictureNode);

    // Get Music library.
    StorageFolder musicFolder = KnownFolders.MusicLibrary;
    TreeViewNode musicNode = new TreeViewNode();
    musicNode.Content = musicFolder;
    musicNode.IsExpanded = true;
    musicNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(musicNode);
    FillTreeNode(musicNode);
}

private async void FillTreeNode(TreeViewNode node)
{
    // Get the contents of the folder represented by the current tree node.
    // Add each item as a new child node of the node that's being expanded.

    // Only process the node if it's a folder and has unrealized children.
    StorageFolder folder = null;
    if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
    {
        folder = node.Content as StorageFolder;
    }
    else
    {
        // The node isn't a folder, or it's already been filled.
        return;
    }

    IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

    if (itemsList.Count == 0)
    {
        // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
        // that the chevron appears, but don't try to process children that aren't there.
        return;
    }

    foreach (var item in itemsList)
    {
        var newNode = new TreeViewNode();
        newNode.Content = item;

        if (item is StorageFolder)
        {
            // If the item is a folder, set HasUnrealizedChildren to true. 
            // This makes the collapsed chevron show up.
            newNode.HasUnrealizedChildren = true;
        }
        else
        {
            // Item is StorageFile. No processing needed for this scenario.
        }
        node.Children.Add(newNode);
    }
    // Children were just added to this node, so set HasUnrealizedChildren to false.
    node.HasUnrealizedChildren = false;
}

private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}

private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}

private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}

private void RefreshButton_Click(object sender, RoutedEventArgs e)
{
    sampleTreeView.RootNodes.Clear();
    InitializeTreeView();
}
```

```vb
Public Sub New()
    InitializeComponent()
    InitializeTreeView()
End Sub

Private Sub InitializeTreeView()
    ' A TreeView can have more than 1 root node. The Pictures library
    ' and the Music library will each be a root node in the tree.
    ' Get Pictures library.
    Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
    Dim pictureNode As New TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(pictureNode)
    FillTreeNode(pictureNode)

    ' Get Music library.
    Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
    Dim musicNode As New TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(musicNode)
    FillTreeNode(musicNode)
End Sub

Private Async Sub FillTreeNode(node As TreeViewNode)
    ' Get the contents of the folder represented by the current tree node.
    ' Add each item as a new child node of the node that's being expanded.

    ' Only process the node if it's a folder and has unrealized children.
    Dim folder As StorageFolder = Nothing
    If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
        folder = TryCast(node.Content, StorageFolder)
    Else
        ' The node isn't a folder, or it's already been filled.
        Return
    End If

    Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
    If itemsList.Count = 0 Then
        ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
        ' that the chevron appears, but don't try to process children that aren't there.
        Return
    End If

    For Each item In itemsList
        Dim newNode As New TreeViewNode With {
            .Content = item
        }
        If TypeOf item Is StorageFolder Then
            ' If the item is a folder, set HasUnrealizedChildren to True.
            ' This makes the collapsed chevron show up.
            newNode.HasUnrealizedChildren = True
        Else
            ' Item is StorageFile. No processing needed for this scenario.
        End If
        node.Children.Add(newNode)
    Next

    ' Children were just added to this node, so set HasUnrealizedChildren to False.
    node.HasUnrealizedChildren = False
End Sub

Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub

Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub

Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub

Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
    sampleTreeView.RootNodes.Clear()
    InitializeTreeView()
End Sub
```

## <a name="related-articles"></a>Verwandte Artikel

- [TreeView-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [ListView und GridView](listview-and-gridview.md)
