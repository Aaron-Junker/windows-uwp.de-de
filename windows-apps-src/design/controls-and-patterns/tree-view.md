---
description: Sie können eine erweiterbare Strukturansicht erstellen, indem Sie die ItemsSource an eine hierarchische Datenquelle binden. Sie können jedoch auch TreeViewNode-Objekte selbst erstellen und verwalten.
title: Strukturansicht
label: Tree view
template: detail.hbs
ms.date: 09/24/2020
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
ms.openlocfilehash: e033c98aa9336c544219786a254cebaedfcd9f29
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750146"
---
# <a name="treeview"></a>TreeView

Das XAML-Steuerelement [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) ermöglicht eine Hierarchieauflistung mit Knoten, die das Aus- und Einblenden von geschachtelten Elementen erlauben. Das Steuerelement kann verwendet werden, um eine Ordnerstruktur oder geschachtelte Beziehungen zwischen Elementen in der Benutzeroberfläche zu veranschaulichen.

Die **TreeView**-APIs unterstützen die folgenden Features:

- Die Schachtelung von N Ebenen
- Auswahl einzelner oder mehrerer Knoten
- Datenbindung an die **ItemsSource**-Eigenschaft für **TreeView** und [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)
- **TreeViewItem** als Stamm der **TreeView**-Elementvorlage
- Beliebige Inhaltstypen in einem **TreeViewItem**
- Drag & Drop zwischen Strukturansichten

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Das Steuerelement **TreeView** ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows-UI-Bibliotheks-APIs:** [TreeView-Klasse](/uwp/api/microsoft.ui.xaml.controls.treeview), [TreeViewNode-Klasse](/uwp/api/microsoft.ui.xaml.controls.treeviewnode), [TreeView.ItemsSource-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource)
>
> **Plattform-APIs:** [TreeView-Klasse](/uwp/api/windows.ui.xaml.controls.treeview), [TreeViewNode-Klasse](/uwp/api/windows.ui.xaml.controls.treeviewnode), [TreeView.ItemsSource-Eigenschaft](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)

> [!TIP]
> In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt: `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using**-Anweisung hinzugefügt: `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

- Verwenden Sie eine **Strukturansicht**, wenn Elemente geschachtelte Elemente haben, und dann, wenn es wichtig ist, die hierarchische Beziehung zwischen Elementen und ihren gleichgestellten Elementen und Knoten zu veranschaulichen.

- Vermeiden Sie die **Strukturansicht**, wenn die geschachtelte Beziehung eines Elements nicht im Vordergrund steht. Für die meisten Drilldown-Szenarien eignet sich eine normale Listenansicht.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/TreeView">die App zu öffnen und TreeView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
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

Sie können eine Strukturansicht erstellen, indem Sie die [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) an eine hierarchische Datenquelle binden. Sie können jedoch auch **TreeViewNode**-Objekte selbst erstellen und verwalten.

Um eine Strukturansicht zu erstellen, verwenden Sie ein [TreeView](/uwp/api/windows.ui.xaml.controls.treeview)-Steuerelement und eine Hierarchie von [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)-Objekten. Du erstellst die Knotenhierarchie, indem du der [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes)-Sammlung des **TreeView**-Steuerelements mindestens einen Stammknoten hinzufügst. Der [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children)-Sammlung der einzelnen **TreeViewNodes** können dann weitere Knoten hinzugefügt werden. Sie können Knoten der Strukturansicht beliebig tief schachteln.

Sie können eine hierarchische Datenquelle an die [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)-Eigenschaft binden, um den Inhalt der Strukturansicht bereitzustellen, ebenso wie bei der **ItemsSource** der [ListView](/uwp/api/windows.ui.xaml.controls.listview). Verwenden Sie auf die gleiche Weise [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (und den optionalen [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)), um eine [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) bereitzustellen, die das Element rendert.

> [!IMPORTANT]
> **ItemsSource** und die zugehörigen APIs erfordern Windows 10, Version 1809 ([SDK 17763 oder höher](https://developer.microsoft.com/windows/downloads/windows-10-sdk)), oder die [Windows-UI-Bibliothek](/uwp/toolkits/winui/).
>
> **ItemsSource** ist ein alternativer Mechanismus zu **TreeView.RootNodes** für das Ablegen von Inhalt im **TreeView**-Steuerelement. **ItemsSource** und **RootNodes** können nicht gleichzeitig festgelegt werden. Wenn Sie **ItemsSource** verwenden, werden Knoten für Sie erstellt, und Sie können aus der **TreeView.RootNodes**-Eigenschaft darauf zugreifen.

Im Folgenden finden Sie ein Beispiel für eine einfache in XAML deklarierte Strukturansicht. Sie fügen die Knoten in der Regel im Code hinzu, aber hier wird die XAML-Hierarchie angezeigt, da sie beim Visualisieren der Erstellung der Knotenhierarchie hilfreich sein kann.

```xaml
<muxc:TreeView>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
                           IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

In den meisten Fällen zeigt die Strukturansicht Daten aus einer Datenquelle an. Daher deklarieren Sie das **TreeView**-Steuerelement in der Regel in XAML, fügen die **TreeViewNode**-Objekte jedoch im Code oder per Datenbindung hinzu.

### <a name="bind-to-a-hierarchical-data-source"></a>Binden an eine hierarchische Datenquelle

Zum Erstellen einer Strukturansicht mit Datenbindung legen Sie eine hierarchische Sammlung auf die **TreeView.ItemsSource**-Eigenschaft fest. Legen Sie anschließend in der **ItemTemplate** die Sammlung der untergeordneten Elemente auf die **TreeViewItem.ItemsSource**-Eigenschaft fest.

```xaml
<muxc:TreeView ItemsSource="{x:Bind DataSource}">
    <muxc:TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <muxc:TreeViewItem ItemsSource="{x:Bind Children}"
                               Content="{x:Bind Name}"/>
        </DataTemplate>
    </muxc:TreeView.ItemTemplate>
</muxc:TreeView>
```

Den vollständigen Code finden Sie unter [Strukturansicht mit Datenbindung](#tree-view-using-data-binding).

#### <a name="items-and-item-containers"></a>Elemente und Elementcontainer

Wenn Sie **TreeView.ItemsSource** verwenden, sind diese APIs verfügbar, um den Knoten bzw. das Datenelement aus dem Container (und umgekehrt) abzurufen.

| [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) | BESCHREIBUNG |
| ------------------------------------------------------------------ | ----------- |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | Ruft das Datenelement für den angegebenen **TreeViewItem**-Container ab. |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | Ruft den **TreeViewItem**-Container für das angegebene Datenelement ab. |

| [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) | BESCHREIBUNG |
| -------------------------------------------------------------- | ----------- |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | Ruft den **TreeViewNode** für den angegebenen **TreeViewItem**-Container ab. |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | Ruft den **TreeViewItem**-Container für den angegebenen **TreeViewNode** ab. |

### <a name="manage-tree-view-nodes"></a>Verwalten von Strukturansichtsknoten

Diese Strukturansicht ist identisch mit der zuvor in XAML erstellten Strukturansicht, die Knoten wurden jedoch im Code erstellt.

```xaml
<muxc:TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    muxc.TreeViewNode rootNode = new muxc.TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New muxc.TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New muxc.TreeViewNode With {.Content = "Vanilla"})
        .Add(New muxc.TreeViewNode With {.Content = "Strawberry"})
        .Add(New muxc.TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

Diese APIs sind für die Verwaltung der Datenhierarchie der Strukturansicht verfügbar.

| [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) | BESCHREIBUNG |
| ------------------------------------------------------ | ----------- |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Eine Strukturansicht kann einen oder mehrere Stammknoten aufweisen. Fügen Sie der **RootNodes**-Sammlung ein **TreeViewNode**-Objekt hinzu, um einen Stammknoten zu erstellen. Das **übergeordnete Element** eines Stammknotens hat immer den Wert **null**. Die **Tiefe** eines Stammknotens beträgt 0. |

| [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) | BESCHREIBUNG |
| -------------------------------------------------------------- | ----------- |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Fügen Sie der **Children**-Sammlung eines übergeordneten Knotens **TreeViewNode**-Objekte hinzu, um eine Knotenhierarchie zu erstellen. Ein Knoten ist das **übergeordnete Element** aller Knoten in seiner **Children**-Sammlung. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | Ist **true**, wenn der Knoten untergeordnete Elemente erkannt hat. **false** gibt einen leeren Ordner oder ein Element an. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Verwenden Sie diese Eigenschaft, wenn Knoten beim Erweitern aufgefüllt werden. Weitere Informationen finden Sie unter [Auffüllen eines Knotens, wenn er erweitert wird](#fill-a-node-when-its-expanding) weiter unten in diesem Artikel. |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Gibt die Entfernung eines untergeordneten Knotens vom Stammknoten an. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Ruft den **TreeViewNode** ab, der die **Children**-Sammlung besitzt, zu der dieser Knoten gehört. |

Die Strukturansicht verwendet die Eigenschaften **HasChildren** und **HasUnrealizedChildren**, um zu ermitteln, ob das Symbol „Erweitern/Reduzieren“ angezeigt wird. Wenn eine der Eigenschaften den Wert **true** hat, wird das Symbol angezeigt, andernfalls nicht.

## <a name="tree-view-node-content"></a>Inhalt des Knotens der Strukturansicht

Sie können das Datenelement, das ein Knoten in der Strukturansicht darstellt, in dessen [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content)-Eigenschaft speichern.

In den vorherigen Beispielen war der Inhalt ein einfacher Zeichenfolgenwert. Hier stellt ein Knoten der Strukturansicht den Ordner **Pictures** des Benutzers dar. Daher ist die Bildbibliothek [StorageFolder](/uwp/api/windows.storage.storagefolder) der **Content**-Eigenschaft des Knotens zugewiesen.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New muxc.TreeViewNode With {.Content = picturesFolder}
```

> [!NOTE]
> Für den Zugriff auf den Ordner **Pictures** müssen Sie im App-Manifest die Funktion **Bildbibliothek** angeben. Weitere Informationen finden Sie unter [Deklarationen von App-Funktionen](../../packaging/app-capability-declarations.md).

Sie können eine [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) angeben, um festzulegen, wie das Datenelement in der Strukturansicht angezeigt wird.

> [!NOTE]
> In Windows 10, Version 1803, müssen Sie die Vorlage für das **TreeView**-Steuerelement umschreiben und eine benutzerdefinierte **ItemTemplate** festlegen, falls der Inhalt keine Zeichenfolge ist. Legen Sie in späteren Versionen die **ItemTemplate**-Eigenschaft fest. Weitere Informationen finden Sie unter [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate).

### <a name="item-container-style"></a>Elementcontainerstil

Egal, ob Sie **ItemsSource** oder **RootNodes** verwenden, bei den tatsächlichen Elementen zum Anzeigen der einzelnen Knoten – als „Container“ bezeichnet – handelt es sich um ein [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)-Objekt. Sie können die **TreeViewItem**-Eigenschaften ändern, um den Container mit der [ItemContainerStyle](/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyle)-Eigenschaft oder [ItemContainerStyleSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyleselector)-Eigenschaft der **TreeView** zu formatieren.

In diesem Beispiel wird gezeigt, wie die erweiterten/reduzierten Glyphen in orangefarbene +/-Zeichen geändert werden. In der **TreeViewItem**-Standardvorlage wird für Glyphen die Schriftart `Segoe MDL2 Assets` verwendet. Sie können die **Setter.Value**-Eigenschaft festlegen, indem Sie den Unicode-Zeichenwert in dem von XAML verwendeten Format angeben, z. B.: `Value="&#xE948;"`.

```xaml
<muxc:TreeView>
    <muxc:TreeView.ItemContainerStyle>
        <Style TargetType="muxc:TreeViewItem">
            <Setter Property="CollapsedGlyph" Value="&#xE948;"/>
            <Setter Property="ExpandedGlyph" Value="&#xE949;"/>
            <Setter Property="GlyphBrush" Value="DarkOrange"/>
        </Style>
    </muxc:TreeView.ItemContainerStyle>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
               IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

### <a name="item-template-selectors"></a>Elementvorlagenselektor

Standardmäßig wird in der [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) für jeden Knoten die Zeichenfolgendarstellung des Datenelements angezeigt. Sie können durch Festlegen der [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)-Eigenschaft ändern, was für alle Knoten angezeigt wird. Oder Sie können mit einem [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplateselector) eine andere [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) für die Strukturansichtselemente auf Grundlage des Elementtyps oder anderer von Ihnen angegebener Kriterien auswählen.

In einer Datei-Explorer-App können Sie beispielsweise eine Datenvorlage für Ordner und eine andere für Dateien verwenden.

![Ordner und Dateien, für die unterschiedliche Datenvorlagen verwendet werden](images/treeview-icons.png)

Hier ist ein Beispiel für das Erstellen und Verwenden eines Elementvorlagenselektors.  Weitere Informationen finden Sie unter der [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)-Klasse.

> [!NOTE]
> Dieser Code ist Teil eines größeren Beispiels und kann nicht isoliert ausgeführt werden. Das vollständige Beispiel, einschließlich des Codes zum Definieren von `ExplorerItem`, finden Sie im [Repository „Xaml-Controls-Gallery“](https://github.com/microsoft/Xaml-Controls-Gallery) auf GitHub. [TreeViewPage.xaml](https://github.com/microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml) und [TreeViewPage.xaml.cs](https://github.com/Microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml.cs) enthalten den relevanten Code.

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{x:Bind Name}"/>
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <muxc:TreeView
        ItemsSource="{x:Bind DataSource}"
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

Der Typ des an die [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore)-Methode übergebenen Objekts hängt davon ab, ob Sie die Strukturansicht durch Festlegen der **ItemsSource**-Eigenschaft erstellen oder zu diesem Zweck **TreeViewNode**-Objekte selbst erstellen und verwalten.

- Wenn **ItemsSource** festgelegt ist, ist das Objekt vom Typ des Datenelements. Im vorherigen Beispiel war das Objekt ein `ExplorerItem`, sodass es nach einer einfachen Umwandlung in `ExplorerItem`: `var explorerItem = (ExplorerItem)item;` verwendet werden konnte.
- Wenn **ItemsSource** nicht festgelegt ist und Sie die Strukturansichtsknoten selbst verwalten, ist das an **SelectTemplateCore** übergebene Objekt ein **TreeViewNode**. In diesem Fall können Sie das Datenelement aus der [TreeViewNode.Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content)-Eigenschaft abrufen.

Dies ist ein Datenvorlagenselektor aus dem später gezeigten Beispiel für eine [Strukturansicht für Bild- und Musikbibliotheken](#pictures-and-music-library-tree-view). Die **SelectTemplateCore**-Methode empfängt einen **TreeViewNode**, dessen Inhalt ein [StorageFolder](/uwp/api/windows.storage.storagefolder) oder eine [StorageFile](/uwp/api/windows.storage.storagefile) sein kann. Basierend auf dem Inhalt kann eine Standardvorlage oder eine spezielle Vorlage für den Ordner „Musik“, den Ordner „Bilder“, für Musikdateien oder Bilddateien zurückgegeben werden.

```csharp
protected override DataTemplate SelectTemplateCore(object item)
{
    var node = (TreeViewNode)item;
    if (node.Content is StorageFolder)
    {
        var content = node.Content as StorageFolder;
        if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
        if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
    }
    else if (node.Content is StorageFile)
    {
        var content = node.Content as StorageFile;
        if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
        if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;
    }
    return DefaultTemplate;
}
```

```vb
Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
    Dim node = CType(item, muxc.TreeViewNode)

    If TypeOf node.Content Is StorageFolder Then
        Dim content = TryCast(node.Content, StorageFolder)
        If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
        If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
    ElseIf TypeOf node.Content Is StorageFile Then
        Dim content = TryCast(node.Content, StorageFile)
        If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
        If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
    End If

    Return DefaultTemplate
End Function
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

- Die [TreeView](/uwp/api/windows.ui.xaml.controls.treeview)-Klasse verfügt über die [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse)-Methode und die [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand)-Methode. Wenn Sie diese Methoden aufrufen, übergeben Sie den **TreeViewNode**, den Sie erweitern oder reduzieren möchten.

- Jeder [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) verfügt über die [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded)-Eigenschaft. Mit dieser Eigenschaft können Sie den Zustand eines Knotens überprüfen; Sie können sie auch so festlegen, dass sein Zustand geändert wird. Diese Eigenschaft lässt sich auch in XAML festlegen, um den Anfangszustand eines Knotens festzulegen.

### <a name="fill-a-node-when-its-expanding"></a>Auffüllen eines Knotens, wenn er erweitert wird

Sie müssen möglicherweise eine große Anzahl von Knoten in der Strukturansicht anzeigen, oder Sie wissen möglicherweise nicht vorab, wie viele Knoten diese aufweisen wird. Das **TreeView**-Steuerelement ist nicht virtualisiert. Daher können Sie Ressourcen verwalten, indem Sie jeden Knoten beim Erweitern auffüllen und die untergeordneten Knoten beim Reduzieren entfernen.

Behandeln Sie das [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand)-Ereignis, und verwenden Sie die [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren)-Eigenschaft, um einem Knoten untergeordnete Elemente hinzuzufügen, wenn er erweitert wird. Die **HasUnrealizedChildren**-Eigenschaft gibt an, ob der Knoten aufgefüllt werden muss oder ob dessen **Children**-Sammlung bereits aufgefüllt wurde. Es ist wichtig, dass dieser Wert nicht durch den **TreeViewNode** festgelegt, sondern von Ihnen im App-Code verwaltet wird.

Im Folgenden finden Sie ein Beispiel für diese verwendeten APIs. Der vollständige Beispielcode ist am Ende dieses Artikels aufgeführt, einschließlich der Implementierung von **FillTreeNode**.

```csharp
private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

Es ist nicht erforderlich, Sie können jedoch auch das [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed)-Ereignis behandeln und die untergeordneten Knoten entfernen, wenn der übergeordnete Knoten geschlossen wird. Dies kann wichtig sein, wenn die Strukturansicht viele Knoten aufweist oder die Knotendaten viele Ressourcen nutzen. Bedenken Sie potenzielle Auswirkungen auf die Leistung durch das Ausfüllen eines Knotens bei jedem Öffnen im Vergleich gegenüber der Beibehaltung der untergeordneten Elemente in einem geschlossenen Knoten. Welche Option jeweils geeignet ist, hängt von Ihrer App ab.

Nachfolgend finden Sie ein Beispiel für einen Handler für das **Collapsed**-Ereignis.

```csharp
private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>Aufrufen eines Elements

Ein Benutzer kann eine Aktion (behandelt das Element wie eine Schaltfläche) aufrufen, statt das Element auszuwählen. Sie behandeln das [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked)-Ereignis, um auf diese Benutzerinteraktion zu reagieren.

> [!NOTE]
> Im Gegensatz zur **ListView**, die über die [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled)-Eigenschaft verfügt, ist das Aufrufen eines Elements für die Strukturansicht immer aktiviert. Sie können weiterhin auswählen, ob das Ereignis behandelt werden soll oder nicht.

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)-Klasse**

Die **ItemInvoked**-Ereignisargumente ermöglichen Ihnen den Zugriff auf das aufgerufene Element. Die [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem)-Eigenschaft verfügt über den Knoten, der aufgerufen wurde. Sie können eine Umwandlung in **TreeViewNode** durchführen, um das Datenelement aus der **TreeViewNode.Content**-Eigenschaft abzurufen.

Nachfolgend finden Sie ein Beispiel für einen **ItemInvoked**-Ereignishandler. Das Datenelement ist ein [IStorageItem](/uwp/api/windows.storage.istorageitem), und in diesem Beispiel werden lediglich einige Informationen über die Datei und die Struktur angezeigt. Wenn der Knoten ein Ordnerknoten ist, wird der Knoten gleichzeitig erweitert bzw. reduziert. Andernfalls wird der Knoten nur dann erweitert oder reduziert, wenn auf die Chevron-Schaltfläche geklickt wird.

```csharp
private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as muxc.TreeViewNode;
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
Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
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

Das **TreeView**-Steuerelement unterstützt sowohl die Einzelauswahl als auch die Mehrfachauswahl. Die Auswahl von Knoten ist standardmäßig deaktiviert, Sie können die [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode)-Eigenschaft jedoch festlegen, um die Auswahl von Knoten zu ermöglichen. Die [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode)-Werte sind **Keine**, **Einzelne** und **Mehrere**.

#### <a name="multiple-selection"></a>Mehrfachauswahl

Wenn die Mehrfachauswahl aktiviert ist, wird ein Kontrollkästchen neben jedem Knoten einer Strukturansicht angezeigt, und ausgewählte Elemente werden hervorgehoben. Ein Benutzer kann ein Element mithilfe eines Kontrollkästchens aktivieren oder deaktivieren. Das Element kann weiterhin aufgerufen werden, indem darauf geklickt wird.

Beim Aktivieren bzw. Deaktivieren eines übergeordneten Knotens werden alle darunter befindlichen untergeordneten Knoten aktiviert bzw. deaktiviert. Wenn einige (jedoch nicht alle) untergeordnete Elemente unter einem übergeordneten Knoten ausgewählt werden, wird das Kontrollkästchen für den übergeordneten Knoten mit dem Status „Undefiniert“ angezeigt.

![Mehrfachauswahl in einer Strukturansicht](images/treeview-selection.png)

Ausgewählte Knoten werden der [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes)-Sammlung der Strukturansicht hinzugefügt. Sie können die [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall)-Methode aufrufen, um alle Knoten in einer Strukturansicht auszuwählen.

> [!NOTE]
> Wenn Sie **SelectAll** aufrufen, werden alle erkannten Knoten ungeachtet des **SelectionMode** ausgewählt. Um ein einheitliches Benutzererlebnis zu ermöglichen, sollten Sie **SelectAll** nur dann aufrufen, wenn **SelectionMode** auf **Multiple** festgelegt ist.

#### <a name="selection-and-realizedunrealized-nodes"></a>Auswahl und erkannte/nicht erkannte Knoten

Wenn die Strukturansicht nicht erkannte Knoten aufweist, werden sie bei der Auswahl nicht berücksichtigt. Beachten Sie bei der Auswahl mit nicht erkannten Knoten Folgendes.

- Wenn ein Benutzer einen übergeordneten Knoten auswählt, werden auch alle erkannten untergeordneten Elemente ausgewählt, die sich unter diesem übergeordneten Element befinden. Wenn alle untergeordneten Knoten ausgewählt werden, wird der übergeordnete Knoten ebenfalls ausgewählt.
- Bei der **SelectAll**-Methode werden der **SelectedNodes**-Sammlung nur erkannte Knoten hinzugefügt.
- Wenn ein übergeordneter Knoten mit nicht erkannten untergeordneten Elementen ausgewählt wird, werden die untergeordneten Elemente ausgewählt, sobald sie erkannt werden.

#### <a name="selecteditemselecteditems"></a>SelectedItem/SelectedItems

Beginnend mit WinUI 2.2 verfügt TreeView über die Eigenschaften [SelectedItem-](/uwp/api/microsoft.ui.xaml.controls.treeview.selecteditem) und [SelectedItems](/uwp/api/microsoft.ui.xaml.controls.treeview.selecteditems). Sie können diese Eigenschaften verwenden, um den Inhalt ausgewählter Knoten direkt abzurufen. Wenn die Mehrfachauswahl aktiviert ist, enthält „SelectedItem“ das erste Element in der SelectedItems-Sammlung.

## <a name="code-examples"></a>Codebeispiele

Die folgenden Codebeispiele veranschaulichen verschiedene Features des TreeView-Steuerelements.

### <a name="tree-view-using-xaml"></a>Strukturansicht mit XAML

Dieses Beispiel zeigt, wie in XAML eine einfache Struktur für eine Strukturansicht erstellt wird. Die Strukturansicht zeigt in Kategorien angeordnete Eissorten und Garnierungen, die der Benutzer auswählen kann. Die Mehrfachauswahl ist aktiviert, und wenn der Benutzer auf eine Schaltfläche klickt, werden die ausgewählten Elemente in der Haupt-App-UI angezeigt.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView x:Name="DessertTree" SelectionMode="Multiple">
                    <muxc:TreeView.RootNodes>
                        <muxc:TreeViewNode Content="Flavors" IsExpanded="True">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Vanilla"/>
                                <muxc:TreeViewNode Content="Strawberry"/>
                                <muxc:TreeViewNode Content="Chocolate"/>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>

                        <muxc:TreeViewNode Content="Toppings">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Candy">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Chocolate"/>
                                        <muxc:TreeViewNode Content="Mint"/>
                                        <muxc:TreeViewNode Content="Sprinkles"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Fruits">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Mango"/>
                                        <muxc:TreeViewNode Content="Peach"/>
                                        <muxc:TreeViewNode Content="Kiwi"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Berries">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Strawberry"/>
                                        <muxc:TreeViewNode Content="Blueberry"/>
                                        <muxc:TreeViewNode Content="Blackberry"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>
                    </muxc:TreeView.RootNodes>
                </muxc:TreeView>
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
</Page>
```

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
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
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As muxc.TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = muxc.TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>Strukturansicht mit Datenbindung

In diesem Beispiel wird veranschaulicht, wie Sie dieselbe Strukturansicht wie im vorherigen Beispiel erstellen. Die Datenhierarchie wird jedoch nicht in XAML erstellt. Stattdessen werden die Daten im Code erstellt und an die **ItemsSource**-Eigenschaft der Strukturansicht gebunden. (Die im vorigen Beispiel gezeigten Schaltflächen-Ereignishandler gelten auch für dieses Beispiel.)

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:local="using:TreeViewTest"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView Name="DessertTree"
                                      SelectionMode="Multiple"
                                      ItemsSource="{x:Bind DataSource}">
                    <muxc:TreeView.ItemTemplate>
                        <DataTemplate x:DataType="local:Item">
                            <muxc:TreeViewItem
                                ItemsSource="{x:Bind Children}"
                                Content="{x:Bind Name}"/>
                        </DataTemplate>
                    </muxc:TreeView.ItemTemplate>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all"
                        Click="SelectAllButton_Click"/>
                <Button Content="Create order"
                        Click="OrderButton_Click"
                        Margin="0,12"/>
                <TextBlock Text="Your flavor selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>

</Page>
```

```csharp
using System.Collections.ObjectModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
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

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
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
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
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
}

```

### <a name="pictures-and-music-library-tree-view"></a>Strukturansicht für Bild- und Musikbibliotheken

Dieses Beispiel zeigt, wie eine Strukturansicht erstellt wird, die Inhalt und Struktur der **Bild**- und **Musikbibliotheken** von Benutzern anzeigt. Da die Anzahl der Elemente vorab nicht bekannt sein kann, wird jeder Knoten beim Erweitern aufgefüllt und beim Reduzieren geleert.

Eine benutzerdefinierte Elementvorlage wird verwendet, um die Datenelemente vom Typ [IStorageItem](/uwp/api/windows.storage.istorageitem) anzuzeigen.

> [!IMPORTANT]
> Für den Code in diesem Beispiel sind die Funktionen **picturesLibrary** und **musicLibrary** erforderlich. Weitere Informationen zum Dateizugriff finden Sie unter [Berechtigungen für den Dateizugriff](../../files/file-access-permissions.md), [Aufzählen und Abfragen von Dateien und Ordnern](../../files/quickstart-listing-files-and-folders.md) und [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewTest"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:storage="using:Windows.Storage"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <Grid Height="44">
                <TextBlock Text="{x:Bind ((storage:IStorageItem)Content).Name}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <DataTemplate x:Key="MusicItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Audio" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFile)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB9F;"
                          Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFile)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="MusicFolderDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="MusicInfo" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFolder)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureFolderDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Pictures" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFolder)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            DefaultTemplate="{StaticResource TreeViewItemDataTemplate}"
            MusicItemTemplate="{StaticResource MusicItemDataTemplate}"
            MusicFolderTemplate="{StaticResource MusicFolderDataTemplate}"
            PictureItemTemplate="{StaticResource PictureItemDataTemplate}"
            PictureFolderTemplate="{StaticResource PictureFolderDataTemplate}"/>
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
                    <muxc:TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"
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
using System;
using System.Collections.Generic;
using Windows.Storage;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
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
            muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
            pictureNode.Content = picturesFolder;
            pictureNode.IsExpanded = true;
            pictureNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(pictureNode);
            FillTreeNode(pictureNode);

            // Get Music library.
            StorageFolder musicFolder = KnownFolders.MusicLibrary;
            muxc.TreeViewNode musicNode = new muxc.TreeViewNode();
            musicNode.Content = musicFolder;
            musicNode.IsExpanded = true;
            musicNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(musicNode);
            FillTreeNode(musicNode);
        }

        private async void FillTreeNode(muxc.TreeViewNode node)
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
                var newNode = new muxc.TreeViewNode();
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

        private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
        {
            if (args.Node.HasUnrealizedChildren)
            {
                FillTreeNode(args.Node);
            }
        }

        private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
        {
            args.Node.Children.Clear();
            args.Node.HasUnrealizedChildren = true;
        }

        private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
        {
            var node = args.InvokedItem as muxc.TreeViewNode;

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
    }

    public class ExplorerItemTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate MusicItemTemplate { get; set; }
        public DataTemplate PictureItemTemplate { get; set; }
        public DataTemplate MusicFolderTemplate { get; set; }
        public DataTemplate PictureFolderTemplate { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            var node = (muxc.TreeViewNode)item;

            if (node.Content is StorageFolder)
            {
                var content = node.Content as StorageFolder;
                if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
                if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
            }
            else if (node.Content is StorageFile)
            {
                var content = node.Content as StorageFile;
                if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
                if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;

            }
            return DefaultTemplate;
        }
    }
}
```

```vb
Public NotInheritable Class MainPage
    Inherits Page

    Public Sub New()
        InitializeComponent()
        InitializeTreeView()
    End Sub

    Private Sub InitializeTreeView()
        ' A TreeView can have more than 1 root node. The Pictures library
        ' and the Music library will each be a root node in the tree.
        ' Get Pictures library.
        Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
        Dim pictureNode As New muxc.TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(pictureNode)
        FillTreeNode(pictureNode)

        ' Get Music library.
        Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
        Dim musicNode As New muxc.TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(musicNode)
        FillTreeNode(musicNode)
    End Sub

    Private Async Sub FillTreeNode(node As muxc.TreeViewNode)
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
            Dim newNode As New muxc.TreeViewNode With {
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

    Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
        If args.Node.HasUnrealizedChildren Then
            FillTreeNode(args.Node)
        End If
    End Sub

    Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
        args.Node.Children.Clear()
        args.Node.HasUnrealizedChildren = True
    End Sub

    Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
        Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
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

End Class

Public Class ExplorerItemTemplateSelector
    Inherits DataTemplateSelector

    Public Property DefaultTemplate As DataTemplate
    Public Property MusicItemTemplate As DataTemplate
    Public Property PictureItemTemplate As DataTemplate
    Public Property MusicFolderTemplate As DataTemplate
    Public Property PictureFolderTemplate As DataTemplate

    Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
        Dim node = CType(item, muxc.TreeViewNode)

        If TypeOf node.Content Is StorageFolder Then
            Dim content = TryCast(node.Content, StorageFolder)
            If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
            If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
        ElseIf TypeOf node.Content Is StorageFile Then
            Dim content = TryCast(node.Content, StorageFile)
            If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
            If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
        End If

        Return DefaultTemplate
    End Function
End Class
```

### <a name="drag-and-drop-items-between-tree-views"></a>Ziehen und Ablegen von Elementen zwischen Strukturansichten

Im folgenden Beispiel wird veranschaulicht, wie zwei Strukturansichten erstellt werden, deren Elemente zwischen den Ansichten gezogen und abgelegt werden können. Wenn ein Element in die andere Strukturansicht gezogen wird, wird es am Ende der Liste hinzugefügt. Die Elemente können jedoch innerhalb einer Strukturansicht neu angeordnet werden. In diesem Beispiel werden auch Strukturansichten mit einem Stammknoten berücksichtigt.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <TreeView x:Name="treeView1"
                  AllowDrop="True"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>
        <TreeView x:Name="treeView2"
                  AllowDrop="True"
                  Grid.Column="1"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>

    </Grid>

</Page>
```

```csharp
using System;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private TreeViewNode deletedItem;
        private TreeView sourceTreeView;

        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            TreeViewNode parentNode1 = new TreeViewNode() { Content = "tv1" };
            TreeViewNode parentNode2 = new TreeViewNode() { Content = "tv2" };

            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FirstChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1SecondChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1ThirdChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FourthChild" });
            parentNode1.IsExpanded = true;
            treeView1.RootNodes.Add(parentNode1);

            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2FirstChild" });
            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2SecondChild" });
            parentNode2.IsExpanded = true;
            treeView2.RootNodes.Add(parentNode2);
        }

        private void TreeView_DragOver(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                e.AcceptedOperation = DataPackageOperation.Move;
            }
        }

        private async void TreeView_Drop(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                string text = await e.DataView.GetTextAsync();
                TreeView destinationTreeView = sender as TreeView;

                if (destinationTreeView.RootNodes != null)
                {
                    TreeViewNode newNode = new TreeViewNode() { Content = text };
                    destinationTreeView.RootNodes[0].Children.Add(newNode);
                    deletedItem = newNode;
                }
            }
        }

        private void TreeView_DragItemsStarting(TreeView sender, TreeViewDragItemsStartingEventArgs args)
        {
            if (args.Items.Count == 1)
            {
                args.Data.RequestedOperation = DataPackageOperation.Move;
                sourceTreeView = sender;

                foreach (var item in args.Items)
                {
                    args.Data.SetText(item.ToString());
                }
            }
        }

        private void TreeView_DragItemsCompleted(TreeView sender, TreeViewDragItemsCompletedEventArgs args)
        {
            var children = sourceTreeView.RootNodes[0].Children;

            if (deletedItem != null)
            {
                for (int i = 0; i < children.Count; i++)
                {
                    if (children[i].Content.ToString() == deletedItem.Content.ToString())
                    {
                        children.RemoveAt(i);
                        break;
                    }
                }
            }

            sourceTreeView = null;
            deletedItem = null;
        }
    }
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [TreeView-Klasse](/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView-Klasse](/uwp/api/windows.ui.xaml.controls.listview)
- [ListView und GridView](listview-and-gridview.md)
