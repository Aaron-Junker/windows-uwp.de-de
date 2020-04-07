---
Description: Ein Steuerelement eines semantischen Zooms ermöglicht Benutzern, zwischen zwei verschiedenen semantischen Ansichten des gleichen Datensatzes zu zoomen.
title: Semantischer Zoom
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 35613366078531a9e342df0b4314d049fde844e9
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081572"
---
# <a name="semantic-zoom"></a>Semantischer Zoom

 

Mit dem semantischen Zoom können Benutzer zwischen zwei unterschiedlichen Ansichten desselben Inhalts zoomen, um schnell durch große Mengen gruppierter Daten zu navigieren.
 
- Die vergrößerte Ansicht ist die Hauptansicht des Inhalts. Dies ist die Hauptansicht, in der Sie einzelne Datenelemente anzeigen. 
- Die verkleinerte Ansicht zeigt den gleichen Inhalt auf höherer Ebene. In dieser Ansicht werden normalerweise die Gruppenköpfe für gruppierte Daten angezeigt. 

Bei der Anzeige eines Adressbuchs kann der Benutzer beispielsweise schnell zum Buchstaben „W” springen und diesen vergrößern, um die zu dem betreffenden Buchstaben gehörenden Namen anzuzeigen. 

> **Wichtige APIs:** [SemanticZoom-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom), [ListView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [GridView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)

**Features**:

-   Die Größe der verkleinerten Ansicht ist durch die Grenzen des Steuerelements für semantischen Zoom beschränkt.
-   Durch Tippen auf einen Gruppenkopf wird zwischen Ansichten gewechselt. Auch Zusammendrücken kann als Möglichkeit zum Wechseln zwischen den Ansichten aktiviert werden.
-   Aktive Kopfzeilen wechseln zwischen Ansichten.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwende das Steuerelement **SemanticZoom**, wenn du einen gruppierten Datensatz anzeigen musst, der so groß ist, dass er auf einer oder zwei Seiten nicht ganz angezeigt werden kann.

Der semantische Zoom ist nicht mit dem optischen Zoom zu verwechseln. Sie zeigen zwar das gleiche Interaktions- und Grundverhalten (d. h. sie zeigen je nach Zoomfaktor mehr oder weniger Details an), der optische Zoom betrifft jedoch die Größenanpassung für einen Inhaltsbereich oder ein Objekt wie etwa ein Foto. Informationen zu einem Steuerelement, das optisches Zooming durchführt, finden Sie im Artikel über das Steuerelement [ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/SemanticZoom">die App zu öffnen und SemanticZoom in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Fotos-App**

Hier ist ein in der Fotos-App verwendeter semantischer Zoom. Fotos werden nach Monat gruppiert. Durch Auswahl einer Monatskopfzeile in der standardmäßigen Rasteransicht wird die Ansicht der Monatsliste verkleinert, um schneller navigieren zu können.

![In der Fotos-App verwendeter semantischer Zoom](images/control-examples/semantic-zoom-photos.png)

**Adressbuch**

Ein Adressbuch ist ein weiteres Beispiel für einen Datensatz, der sich mit einem Steuerelement für semantisches Zoomen sehr viel einfacher durchblättern lässt. Sie können die verkleinerte Ansicht verwenden, um schnell zu dem gewünschten Buchstaben zu springen (linkes Bild), während die vergrößerte Ansicht die einzelnen Datenelemente zeigt (rechtes Bild).

![Beispiel für semantischen Zoom in einer Kontaktliste](images/semanticzoom-win10.png)

## <a name="create-a-semantic-zoom"></a>Erstellen eines semantischen Zooms

Das Steuerelement **SemanticZoom** verfügt über keine visuelle Darstellung. Es handelt sich um ein Hoststeuerelement, das den Übergang zwischen zwei anderen Steuerelementen steuert, die die Ansichten für Ihre Inhalte bereitstellen, in der Regel die Steuerelemente **ListView** oder **GridView**.  Sie legen die Ansicht-Steuerelemente auf die [ZoomedInView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.semanticzoom.zoomedinview)- und [ZoomedOutView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.semanticzoom.zoomedoutview)-Eigenschaften von SemanticZoom fest.

Sie benötigen für einen semantischen Zoom folgende drei Elemente:
- Eine gruppierte Datenquelle (Gruppen werden durch die GroupStyle-Definition in der vergrößerten Ansicht definiert)
- Eine vergrößerte Ansicht, die Daten auf Elementebene anzeigt
- Eine verkleinerte Ansicht, die die Daten auf Gruppenebene anzeigt

Bevor Sie einen semantischen Zoom verwenden, sollten Sie wissen, wie Sie eine Listenansicht mit gruppierten Daten verwenden. Weitere Informationen finden Sie unter [Listenansicht und Rasteransicht](listview-and-gridview.md). 

> **Hinweis**:&nbsp;&nbsp;Um die vergrößerte und verkleinerte Ansicht des SemanticZoom-Steuerelements zu definieren, können Sie zwei beliebige Steuerelemente verwenden, die die Benutzeroberfläche [ISemanticZoomInformation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ISemanticZoomInformation) implementieren. Das XAML-Framework bietet drei Steuerelemente, die diese Schnittstelle implementieren: ListView, GridView und Hub.
 
 Dieser XAML-Code zeigt die Struktur des SemanticZoom-Steuerelements. Sie weisen weitere Steuerelemente den Eigenschaften „ZoomedInView” und „ZoomedOutView” zu.
 
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed in view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed out view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
Die folgenden Beispiele stammen von der Seite „SemanticZoom” des [Beispiels für XAML-UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics). Sie können das Beispiel herunterladen, um den kompletten Code anzuzeigen, einschließlich der Datenquelle. Dieser semantische Zoom verwendet eine GridView, um die vergrößerte Ansicht und eine ListView für die verkleinerte Ansicht bereitzustellen.
  
**Definieren der vergrößerten Ansicht**

Hier sehen Sie das GridView-Steuerelement für die vergrößerte Ansicht. Die vergrößerte Ansicht sollte die einzelnen Datenelemente in Gruppen anzeigen. Dieses Beispiel zeigt, wie die Elemente in einem Raster mit einem Bild und Text angezeigt werden sollten. 

```xaml
<SemanticZoom.ZoomedInView>
    <GridView ItemsSource="{x:Bind cvsGroups.View}" 
              ScrollViewer.IsHorizontalScrollChainingEnabled="False" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedInTemplate}">
        <GridView.GroupStyle>
            <GroupStyle HeaderTemplate="{StaticResource ZoomedInGroupHeaderTemplate}"/>
        </GridView.GroupStyle>
    </GridView>
</SemanticZoom.ZoomedInView>
```
 
Die Darstellung der Gruppenkopfzeilen wird in der Ressource `ZoomedInGroupHeaderTemplate` definiert. Die Darstellung der Elemente wird in der Ressource `ZoomedInTemplate` definiert. 

```xaml
<DataTemplate x:Key="ZoomedInGroupHeaderTemplate" x:DataType="data:ControlInfoDataGroup">
    <TextBlock Text="{x:Bind Title}" 
               Foreground="{ThemeResource ApplicationForegroundThemeBrush}" 
               Style="{StaticResource SubtitleTextBlockStyle}"/>
</DataTemplate>

<DataTemplate x:Key="ZoomedInTemplate" x:DataType="data:ControlInfoDataItem">
    <StackPanel Orientation="Horizontal" MinWidth="200" Margin="12,6,0,6">
        <Image Source="{x:Bind ImagePath}" Height="80" Width="80"/>
        <StackPanel Margin="20,0,0,0">
            <TextBlock Text="{x:Bind Title}" 
                       Style="{StaticResource BaseTextBlockStyle}"/>
            <TextBlock Text="{x:Bind Subtitle}" 
                       TextWrapping="Wrap" HorizontalAlignment="Left" 
                       Width="300" Style="{StaticResource BodyTextBlockStyle}"/>
        </StackPanel>
    </StackPanel>
</DataTemplate>
```

**Definieren der verkleinerten Ansicht**

Dieser XAML-Code definiert ein ListView-Steuerelement für die verkleinerte Ansicht. Dieses Beispiel zeigt, wie die Gruppenkopfzeilen als Text in einer Liste angezeigt werden sollten.

```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 Die Darstellung wird in der Ressource `ZoomedOutTemplate` definiert.
 
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**Synchronisieren der Ansichten**

Die vergrößerte und verkleinerte Ansicht sollten synchronisiert werden, sodass für den Fall, dass ein Benutzer eine Gruppe in der verkleinerten Ansicht auswählt, die Details dieser Gruppe in der vergrößerten Ansicht angezeigt werden. Sie können eine [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) verwenden oder Code hinzufügen, um die Ansichten zu synchronisieren.

Alle Steuerelemente, die Sie an die gleiche CollectionViewSource binden, verfügen immer über das gleiche aktuelle Element. Wenn beide Ansichten die gleiche CollectionViewSource als Datenquelle verwenden, synchronisiert die CollectionViewSource die Ansichten automatisch. Weitere Informationen finden Sie unter [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource).

Wenn Sie keine CollectionViewSource verwenden, um die Ansichten zu synchronisieren, sollten Sie das [ViewChangeStarted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.semanticzoom.viewchangestarted)-Ereignis behandeln und die Elemente wie hier gezeigt im Ereignishandler synchronisieren.

```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

```csharp
private void SemanticZoom_ViewChangeStarted(object sender, SemanticZoomViewChangedEventArgs e)
{
    if (e.IsSourceZoomedInView == false)
    {
        e.DestinationItem.Item = e.SourceItem.Item;
    }
}
```

## <a name="recommendations"></a>Empfehlungen

-   Stellen Sie bei Verwendung des semantischen Zooms in Ihrer App sicher, dass sich das Elementlayout und die Verschieberichtung nicht basierend auf der Zoomstufe ändern. Layouts und Verschiebeinteraktionen sollten auf allen Zoomstufen konsistent und vorhersehbar sein.
-   Der semantische Zoom ermöglicht Benutzern das schnelle Wechseln zu Inhalten; beschränken Sie daher die Anzahl der Seiten oder Bildschirme in der verkleinerten Ansicht auf drei. Zu viel Verschiebung beeinträchtigt die praktische Nutzung des semantischen Zooms.
-   Vermeiden Sie das Verwenden des semantischen Zooms, um den Umfang der Inhalte zu ändern. So sollte beispielsweise ein Fotoalbum nicht zu einer Ordneransicht im Datei-Explorer wechseln.
-   Verwenden Sie eine Struktur und Semantik, die für die Ansicht wichtig sind.
-   Verwenden Sie Gruppennamen für Elemente in einer gruppierten Auflistung.
-   Verwenden Sie die Sortierreihenfolge für eine nicht gruppierte, jedoch sortierte Sammlung, beispielsweise chronologisch für Datumsangaben oder alphabetisch für Namenslisten.


## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.


## <a name="related-articles"></a>Verwandte Artikel

- [Navigationsdesigngrundlagen](../basics/navigation-basics.md)
- [Listenansicht und Rasteransicht](listview-and-gridview.md)
- [Elementcontainer und Vorlagen](item-containers-templates.md)





