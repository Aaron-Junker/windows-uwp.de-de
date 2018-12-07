---
Description: Displays images in a collection, such as photos in an album or items in a product details page, one image at a time.
title: Richtlinien für Flip-Ansicht-Steuerelemente
ms.assetid: A4E05D92-1A0E-4CDD-84B9-92199FF8A8A3
label: Flip view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows10, UWP
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9a7fa4b824e0472971fab61935857670fa5b49ee
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "8799367"
---
# <a name="flip-view"></a>Flip-Ansicht

 

Verwenden Sie eine Flip-Ansicht zum Durchsuchen von Bildern oder anderen Elementen in einer Sammlung, z. B. von Fotos in einem Album oder von Elementen auf einer Seite mit Produktdetails, wobei jeweils ein Bild gescannt wird. Bei Geräten mit Touchscreen erfolgt die Navigation durch die Sammlung mit einer Wischbewegung über ein Element. Bei Verwendung mit einer Maus werden beim Zeigen mit der Maus Navigationsschaltflächen angezeigt. Bei Verwendung einer Tastatur erfolgt die Navigation durch die Sammlung mithilfe der Pfeiltasten.

> **Wichtige APIs**: [FlipView-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx), [ItemsSource-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx), [ItemTemplate-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Die Flip-Ansicht ist zum Durchblättern von Bildern in kleinen bis mittelgroßen Sammlungen (bis zu ungefähr 25 Elementen) am besten geeignet. Beispiele für solche Sammlungen sind Elemente auf einer Seite mit Produktdetails oder Fotos in einem Fotoalbum. Obwohl die Flip-Ansicht für die meisten großen Auflistungen nicht empfohlen wird, kann das Steuerelement für das Anzeigen einzelner Bilder in einem Fotoalbum verwendet werden.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/FlipView">die App zu öffnen und FlipView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Horizontales Browsen, das bei dem Element ganz links beginnt und bei dem dann nach rechts geblättert wird, ist das typische Layout für eine Flip-Ansicht. Dieses Layout eignet sich im Hochformat oder im Querformat auf allen Geräten:

![Beispiel für das horizontale Layout in der Flip-Ansicht](images/controls_flipview_horizonal.jpg)

Eine Flip-Ansicht kann auch vertikal durchsucht werden:

![Beispiel für eine vertikale Flip-Ansicht](images/controls_flipview_vertical.jpg)

## <a name="create-a-flip-view"></a>Erstellen einer Flip-Ansicht

FlipView ist ein [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx), so dass es eine Sammlung von Elementen jeden Typs enthalten kann. Um die Ansicht auszufüllen, fügen Sie der Sammlung [**Items**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) Elemente hinzu, oder legen die Eigenschaft [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) auf eine Datenquelle fest.

Datenelemente werden in der Flip-Ansicht standardmäßig als Zeichenfolgendarstellung des Datenobjekts angezeigt, an das sie gebunden sind. Um genau anzugeben, wie Elemente in der Flip-Ansicht angezeigt werden, erstellen Sie eine [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.datatemplate.aspx), um das Layout der Steuerelemente zu definieren, die für die Anzeige eines einzelnen Elements verwendet werden. Die Steuerelemente im Layout können an Eigenschaften eines Datenobjekts gebunden werden. Es ist auch möglich, den Inhalt intern zu definieren. Sie weisen die DataTemplate der Eigenschaft [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) von FlipView zu.

### <a name="add-items-to-the-items-collection"></a>Hinzufügen von Elementen zur Sammlung Items

Sie können der Sammlung [**Items**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) Elemente per XAML oder Code hinzufügen. In der Regel fügen Sie Elemente auf diese Weise hinzu, wenn Sie nur über eine geringe Anzahl von Elementen verfügen, die sich nicht ändern und einfach in XAML definiert werden können, oder wenn die Elemente zur Laufzeit im Code generiert werden. Hier sehen Sie eine Flip-Ansicht mit inline definierten Elementen.

```xaml
<FlipView x:Name="flipView1">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

```csharp
// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.Items.Add("Item 1");
flipView1.Items.Add("Item 2");

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

Wenn Sie einer Flip-Ansicht Elemente hinzufügen, werden diese automatisch in einem [**FlipViewItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipviewitem.aspx)-Container platziert. Um die Art und Weise zu ändern, wie ein Element angezeigt wird, können Sie dem Elementcontainer einen Stil zuordnen. Legen Sie dazu die [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx)-Eigenschaft fest. 

Wenn Sie die Elemente in XAML definieren, werden sie der Sammlung Items automatisch hinzugefügt.

### <a name="set-the-items-source"></a>Festlegen der Quelle von Elementen

In der Regel verwenden Sie eine Flip-Ansicht, um Daten aus Quellen wie einer Datenbank oder dem Internet anzuzeigen. Um eine Flip-Ansicht aus einer Datenquelle zu füllen, legen Sie deren Eigenschaft [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) auf eine Sammlung mit Datenelementen fest.

Hier ist die ItemsSource der Flip-Ansicht im Code direkt auf eine Instanz einer Sammlung festgelegt.

```csharp
// Data source.
List<String> itemsList = new List<string>();
itemsList.Add("Item 1");
itemsList.Add("Item 2");

// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.ItemsSource = itemsList;
flipView1.SelectionChanged += FlipView_SelectionChanged;

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

Sie können auch die ItemsSource-Eigenschaft an eine Sammlung in XAML binden. Weitere Informationen finden Sie unter [Datenbindung mit XAML](../../data-binding/data-binding-quickstart.md).

Hier wird die ItemsSource an eine [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) mit dem Namen `itemsViewSource` gebunden. 

```xaml
<Page.Resources>
    <!-- Collection of items displayed by this page -->
    <CollectionViewSource x:Name="itemsViewSource" Source="{Binding Items}"/>
</Page.Resources>

...

<FlipView x:Name="itemFlipView" 
          ItemsSource="{Binding Source={StaticResource itemsViewSource}}"/>
```

>**Hinweis**&nbsp;&nbsp;Sie können eine Flip-Ansicht auffüllen, indem Sie entweder der Items-Sammlung Elemente hinzufügen oder die ItemsSource-Eigenschaft festlegen. Die beiden Methoden können jedoch nicht gleichzeitig verwendet werden. Wenn Sie die ItemsSource-Eigenschaft festlegen und dann ein Element in XAML hinzufügen, wird das hinzugefügte Element ignoriert. Wenn Sie die ItemsSource-Eigenschaft festlegen und der Items-Sammlung ein Element in Code hinzufügen, wird eine Ausnahme ausgelöst.

### <a name="specify-the-look-of-the-items"></a>Festlegen der Darstellung der Elemente

Datenelemente werden in der Flip-Ansicht standardmäßig als Zeichenfolgendarstellung des Datenobjekts angezeigt, an das sie gebunden sind. In der Regel möchten Sie eine ansprechendere Darstellung Ihrer Daten anzeigen. Um genau anzugeben, wie Elemente in der Flip-Ansicht angezeigt werden, erstellen Sie eine [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx). Der XAML-Code in der DataTemplate definiert das Layout und die Darstellung von Steuerelementen, die zum Anzeigen eines einzelnen Elements verwendet werden. Die Steuerelemente im Layout können an Eigenschaften eines Datenobjekts gebunden werden. Es ist auch möglich, den Inhalt intern zu definieren. Die DataTemplate ist der Eigenschaft [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) des FlipView-Steuerelements zugeordnet.

In diesem Beispiel wird die ItemTemplate einer FlipView inline definiert. Dem Bild wird eine Überlagerung hinzugefügt, um den Bildnamen anzuzeigen. 

```XAML
<FlipView x:Name="flipView1" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

Hier sehen Sie das durch die Datenvorlage definierte Layout.

Datenvorlage für die Flip-Ansicht.

### <a name="set-the-orientation-of-the-flip-view"></a>Festlegen der Ausrichtung der Flip-Ansicht

Standardmäßig blättert die Flip-Ansicht horizontal. Damit sie vertikal blättert, verwenden Sie ein StackPanel-Element mit vertikaler Ausrichtung als [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) der Flip-Ansicht.

Dieses Beispiel zeigt, wie ein StackPanel-Element mit vertikaler Ausrichtung als ItemsPanel einer FlipView verwendet wird.

```XAML
<FlipView x:Name="flipViewVertical" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    
    <!-- Use a vertical stack panel for vertical flipping. -->
    <FlipView.ItemsPanel>
        <ItemsPanelTemplate>
            <VirtualizingStackPanel Orientation="Vertical"/>
        </ItemsPanelTemplate>
    </FlipView.ItemsPanel>
    
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

So sieht die Flip-Ansicht mit vertikale Ausrichtung aus.

![Beispiel für eine vertikale Flip-Ansicht](images/controls_flipview_vertical.jpg)

## <a name="adding-a-context-indicator"></a>Hinzufügen einer Kontextanzeige

Eine Kontextanzeige in einer Flip-Ansicht stellt einen nützlichen Referenzpunkt dar. Die Punkte in einer standardmäßigen Kontextanzeige sind nicht interaktiv. Wie in diesem Beispiel gezeigt, ist die beste Position in der Regel zentriert und unterhalb des Katalogs:

![Beispiel für eine Seitenanzeige](images/controls_pageindicator.png)

Für größere Sammlungen (10-25 Elemente) kann eine Anzeige mit mehr Kontext, z. B. ein Bildstreifen, nützlich sein. Im Gegensatz zu einer Kontextanzeige, die einfache Punkte verwendet, zeigt jede Miniaturansicht auf dem Bildstreifen eine kleine Version des entsprechenden Bilds und sollte ausgewählt werden können:

![Beispiel für die Kontextanzeige](images/controls_contextindicator.jpg)

Beispielcode, der veranschaulicht, wie Sie einem FlipView-Element eine Kontextanzeige hinzufügen, finden Sie unter [Beispiel für XAML FlipView](http://go.microsoft.com/fwlink/p/?LinkID=311760).

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

-   Flip-Ansichten sind am besten für Sammlungen von bis zu 25 Elementen geeignet.
-   Vermeiden Sie ein Flip-Ansicht-Steuerelement für größere Sammlungen, da das wiederholte Blättern durch die einzelnen Elemente mühsam sein kann. Eine Ausnahme stellen Fotoalben dar, die häufig Hunderte oder Tausende von Bildern enthalten. Fotoalben wechseln nach dem Auswählen eines Fotos in der Rasteransicht fast immer zur Flip-Ansicht. Erwägen Sie für andere große Sammlungen eine [Listen- oder Rasteransicht](lists.md).
-   Für Kontextanzeigen:
    -   Die Reihenfolge der Punkte (oder anderer visueller Kennzeichnungen, die Sie verwenden) funktioniert am besten zentriert und unter einem horizontal verschiebbaren Katalog.
    -   Wenn Sie eine Kontextanzeige in einer vertikal verschiebbaren Galerie anzeigen möchten, funktioniert diese am besten zentriert und rechts neben den Bildern.
    -   Der hervorgehobene Punkt gibt das aktuelle Element an. In der Regel ist der hervorgehobene Punkt weiß, und die anderen Punkte werden grau dargestellt.
    -   Die Anzahl von Punkten kann variieren. Verwenden Sie jedoch nicht so viele Punkte, dass der Benutzer die Orientierung verliert – in der Regel sollten höchstens 10 Punkte angezeigt werden.

## <a name="globalization-and-localization-checklist"></a>Prüfliste für Globalisierung und Lokalisierung

<table>
<tr>
<th>Aspekte der Bidirektionalität</th><td>Verwenden Sie die standardmäßige Spiegelung für Rechts-nach-links-Sprachen. Die Steuerelemente „Zurück“ und „Vorwärts“ sollten auf der Richtung der Schrift beruhen. Bei Rechts-nach-links-Schriften sollte daher die rechte Schaltfläche die Rückwärtsnavigation und die linke Schaltfläche die Vorwärtsnavigation ermöglichen.</td>
</tr>

</table>

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementkatalogs](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Richtlinien für Listen](lists.md)
- [**FlipView-Klasse**](https://msdn.microsoft.com/library/windows/apps/br242678)
