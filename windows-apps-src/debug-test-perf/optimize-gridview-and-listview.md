---
ms.assetid: 26DF15E8-2C05-4174-A714-7DF2E8273D32
title: Optimieren der ListView- und GridView-Benutzeroberfläche
description: Verbessern Sie die Leistung und Startzeit von ListView und GridView durch UI-Virtualisierung, Elementreduzierung und die progressive Aktualisierung von Elementen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 19749ff80401ccb8a3a4fae0ef6e237f42aeeb40
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166074"
---
# <a name="listview-and-gridview-ui-optimization"></a>Optimieren der ListView- und GridView-Benutzeroberfläche


**Hinweis**   Weitere Informationen findest du in der //build/-Sitzung zum [erheblichen Erhöhen der Leistung bei der Interaktion von Benutzern mit großen Mengen von Daten in GridView und ListView](https://channel9.msdn.com/events/build/2013/3-158).

Verbessern Sie die Leistung und Startzeit von [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) und [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) durch UI-Virtualisierung, Elementreduzierung und die progressive Aktualisierung von Elementen. Weitere Informationen zu Datenvirtualisierungstechniken finden Sie unter [Virtualisierung von ListView- und GridView-Daten](listview-and-gridview-data-optimization.md).

## <a name="two-key-factors-in-collection-performance"></a>Zwei wichtige Faktoren für die Sammlungsleistung

Das Bearbeiten von Sammlungen ist ein gängiges Szenario. Eine Bildanzeige besitzt Sammlungen von Fotos, ein Leser besitzt Sammlungen von Artikeln/Büchern/Geschichten, und eine Shopping-App verfügt über Sammlungen von Produkten. In diesem Thema erfahren Sie, wie Sie vorgehen können, damit Ihre App Sammlungen effizient bearbeiten kann.

Es gibt zwei wichtige Leistungsfaktoren für Sammlungen: einerseits die vom UI-Thread benötige Zeit zum Erstellen von Elementen und andererseits den vom Rohdatensatz und den UI-Elementen zum Rendern dieser Daten verwendeten Arbeitsspeicher.

Für reibungslose Verschiebungen/Bildläufe ist es wichtig, dass der UI-Thread eine effiziente und intelligente Instanziierung, Datenbindung und Anordnung von Elementen vornimmt.

## <a name="ui-virtualization"></a>UI-Virtualisierung

Die Virtualisierung der Benutzeroberfläche ist die wichtigste Verbesserung, die Sie vornehmen können. Dies bedeutet, dass die Benutzeroberflächenelemente, die die Objekte darstellen, bei Bedarf erstellt werden. Für ein an eine Sammlung von 1000 Elementen gebundenes Elementsteuerelement wäre es eine Verschwendung von Ressourcen, die Benutzeroberfläche für alle Elemente gleichzeitig zu erstellen, da sie nicht alle auf einmal angezeigt werden können. [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) und [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) (und andere von [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) abgeleitete Standardsteuerelemente) führen die Virtualisierung der Benutzeroberfläche für dich durch. Wenn Elemente kurz davor sind, per Bildlauf in der Ansicht angezeigt zu werden (einige Seiten davon entfernt), generiert das Framework die Benutzeroberfläche für die Elemente und speichert sie zwischen. Wenn es unwahrscheinlich ist, dass die Elemente erneut angezeigt werden, gibt das Framework den Arbeitsspeicher wieder frei.

Wenn Sie eine benutzerdefinierte ItemsPanel-Vorlage bereitstellen (siehe [**ItemsPanel**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)), ist es wichtig, dass Sie ein Virtualisierungspanel wie [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) oder [**ItemsStackPanel**](/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel) verwenden. Wenn Sie [**VariableSizedWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid), [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) oder [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) verwenden, erhalten Sie keine Virtualisierung. Darüber hinaus werden die folgenden [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)-Ereignisse nur ausgelöst, wenn [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) oder [**ItemsStackPanel**](/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel) verwendet werden: [**ChoosingGroupHeaderContainer**](/uwp/api/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer), [**ChoosingItemContainer**](/uwp/api/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) und [**ContainerContentChanging**](/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging).

Das Viewport-Konzept ist für die UI-Virtualisierung entscheidend, da das Framework die Elemente erstellen muss, die voraussichtlich angezeigt werden. Allgemein gilt: Der Viewport einer [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl)-Klasse entspricht dem Umfang des logischen Steuerelements. So entspricht beispielsweise der Viewport einer [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)-Klasse der Breite und Höhe des **ListView**-Elements. Einige Panels gewähren untergeordneten Elementen unbegrenzten Speicherplatz. Beispiele hierfür sind [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) und ein [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)-Objekt mit Zeilen oder Spalten mit automatischer Größenanpassung. Wird ein virtualisiertes **ItemsControl**-Objekt in einem dieser Panel platziert, nimmt es ausreichend Platz ein, um alle Elemente anzuzeigen, wodurch die Virtualisierung vereitelt wird. Stellen Sie die Virtualisierung wieder her, indem Sie die Breite und Höhe für das **ItemsControl**-Objekt festlegen.

## <a name="element-reduction-per-item"></a>Elementreduzierung pro Element

Halten Sie die Anzahl von Benutzeroberflächenelementen, die zum Rendern Ihrer Elemente verwendet werden, vertretbar gering.

Wenn ein Elementsteuerelement erstmalig angezeigt wird, werden alle zum Rendern eines mit Elementen gefüllten Viewports erforderlichen Elemente erstellt. Während sich die Elemente dem Viewport nähern, aktualisiert das Framework zudem die Benutzeroberflächenelemente in zwischengespeicherten Elementvorlagen mit den gebundenen Datenobjekten. Die Minimierung der Komplexität des Markups in Vorlagen zahlt sich hinsichtlich des Arbeitsspeichers und des Zeitaufwands für den UI-Thread aus, da die Reaktionsfähigkeit insbesondere beim Schwenken/Bildlauf verbessert wird. Die betreffenden Vorlagen sind die Elementvorlage (siehe [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)) und die Steuerelementvorlage eines [**ListViewItem**](/uwp/api/windows.ui.xaml.controls.listviewitem)- oder [**GridViewItem**](/uwp/api/windows.ui.xaml.controls.gridviewitem)-Objekts (die Elementsteuerelementvorlage oder [**ItemContainerStyle**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)). Der Vorteil auch einer geringen Reduzierung der Elementanzahl wird durch die Anzahl der angezeigten Elemente multipliziert.

Beispiele zur Elementreduzierung finden Sie unter [Optimieren des XAML-Markups](optimize-xaml-loading.md).

Die standardmäßigen Steuerelementvorlagen für [**ListViewItem**](/uwp/api/windows.ui.xaml.controls.listviewitem) und [**GridViewItem**](/uwp/api/windows.ui.xaml.controls.gridviewitem) enthalten ein [**ListViewItemPresenter**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter)-Element. Dieser Presenter ist ein einzelnes optimiertes Element, das komplexe visuelle Elemente für Fokus, Auswahl und andere visuelle Zustände anzeigt. Wenn Sie bereits über benutzerdefinierte Elementsteuerelementvorlagen verfügen ([**ItemContainerStyle**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)) oder zukünftig eine Kopie einer Elementsteuerelementvorlage bearbeiten, wird empfohlen, dass Sie ein **ListViewItemPresenter**-Element verwenden, da dieses Element in der Mehrzahl der Fälle ein optimales Gleichgewicht zwischen Leistung und Anpassbarkeit bietet. Sie können den Presenter anpassen, indem Sie dessen Eigenschaften festlegen. Im folgenden Markup-Beispiel wird z. B. das standardmäßig beim Auswählen eines Elements angezeigte Häkchen entfernt und die Hintergrundfarbe des ausgewählten Elements in Orange geändert.

```xml
...
<ListView>
 ...
 <ListView.ItemContainerStyle>
 <Style TargetType="ListViewItem">
 <Setter Property="Template">
 <Setter.Value>
 <ControlTemplate TargetType="ListViewItem">
 <ListViewItemPresenter SelectionCheckMarkVisualEnabled="False" SelectedBackground="Orange"/>
 </ControlTemplate>
 </Setter.Value>
 </Setter>
 </Style>
 </ListView.ItemContainerStyle>
</ListView>
<!-- ... -->
```

Es sind ca. 25 Eigenschaften mit selbstbeschreibenden Namen wie [**SelectionCheckMarkVisualEnabled**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled) und [**SelectedBackground**](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedbackground) verfügbar. Sollten sich die Presentertypen für Ihren Zweck als nicht ausreichend anpassbar erweisen, können Sie stattdessen eine Kopie der `ListViewItemExpanded` - oder `GridViewItemExpanded` -Steuerelementvorlage bearbeiten. Diese befinden sich in `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml`. Beachten Sie, dass die Verwendung dieser Vorlagen bedeutet, dass Sie für die gesteigerte Anpassbarkeit einen Teil der Leistung einbüßen.

<span id="update-items-incrementally"/>

## <a name="update-listview-and-gridview-items-progressively"></a>Progressives Aktualisieren von ListView- und GridView-Elementen

Wenn Sie die Datenvirtualisierung verwenden, können Sie für [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) und [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) eine hohe Reaktionsfähigkeit erhalten. Konfigurieren Sie das Steuerelement zu diesem Zweck so, dass für die noch zu ladenden Elemente temporäre UI-Elemente gerendert werden. Die temporären Elemente werden dann beim Laden der Daten schrittweise durch die eigentlichen UI-Elemente ersetzt.

Egal, von wo die Daten geladen werden (lokaler Datenträger, Netzwerk oder Cloud), kann ein Benutzer ein [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)- oder [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView)-Element so schnell schwenken/durchlaufen, dass es nicht möglich ist, alle Elemente originalgetreu zu rendern, während gleichzeitig eine flüssige Verschiebung bzw. ein gleichmäßiger Bildlauf gewährleistet wird. Um die Gleichmäßigkeit der Verschiebung bzw. des Bildlaufs zu erhalten, können Sie zusätzlich zur Verwendung von Platzhaltern das Element in mehreren Phasen rendern.

Ein Beispiel für diese Verfahren findet sich häufig bei Fotoanzeige-Apps: Auch wenn nicht alle Bilder geladen und angezeigt wurden, kann der Benutzer dennoch Verschiebungen/Bildläufe vornehmen und mit der Sammlung interagieren. Für ein „Filmelement“ können Sie z. B. in der ersten Phase den Titel anzeigen, während die Altersfreigabe in der zweiten und ein Bild des Posters in der dritten Phase angezeigt werden. Der Benutzer sieht die wichtigsten Daten zu den einzelnen Elementen so früh wie möglich, d. h., es können sofort Maßnahmen ergriffen werden. Anschließend werden die weniger wichtigen Informationen ausgefüllt. Hier folgen die Plattformfeatures, mit denen Sie diese Verfahren implementieren können.

### <a name="placeholders"></a>Platzhalter

Das Feature der temporären Platzhalter für visuelle Elemente ist standardmäßig aktiviert und wird über die [**ShowsScrollingPlaceholders**](/uwp/api/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders)-Eigenschaft gesteuert. Bei der schnellen Verschiebung bzw. einem schnellen Bildlauf bietet dieses Feature dem Benutzer einen visuellen Hinweis, dass es weitere anzuzeigende Elemente gibt, während gleichzeitig die Gleichmäßigkeit beibehalten wird. Bei Verwendung eines der folgenden Verfahren können Sie **ShowsScrollingPlaceholders** auf „false“ festlegen, wenn die Platzhalter vom System gerendert werden sollen.

**Progressive Aktualisierung von Datenvorlagen mithilfe von x:Phase**

Im Folgenden wird beschrieben, wie Sie das [x:Phase-Attribut](../xaml-platform/x-phase-attribute.md) mit [{x:Bind}](../xaml-platform/x-bind-markup-extension.md)-Bindungen verwenden, um die progressive Aktualisierung von Datenvorlagen zu implementieren.

1.  So sieht die Bindungsquelle aus (dies ist die Datenquelle, die zum Binden verwendet wird).

    ```csharp
    namespace LotsOfItems
    {
        public class ExampleItem
        {
            public string Title { get; set; }
            public string Subtitle { get; set; }
            public string Description { get; set; }
        }

        public class ExampleItemViewModel
        {
            private ObservableCollection<ExampleItem> exampleItems = new ObservableCollection<ExampleItem>();
            public ObservableCollection<ExampleItem> ExampleItems { get { return this.exampleItems; } }

            public ExampleItemViewModel()
            {
                for (int i = 1; i < 150000; i++)
                {
                    this.exampleItems.Add(new ExampleItem(){
                        Title = "Title: " + i.ToString(),
                        Subtitle = "Sub: " + i.ToString(),
                        Description = "Desc: " + i.ToString()
                    });
                }
            }
        }
    }
    ```
2.  Im Folgenden sehen Sie das Markup, das `DeferMainPage.xaml` enthält. Die Rasteransicht enthält eine Elementvorlage mit Elementen, die an die Eigenschaften **Title**, **Subtitle** und **Description** der **MyItem**-Klasse gebunden sind. Beachten Sie, dass für **X: Phase** der Standardwert 0 ist. Hier werden die Elemente anfänglich nur mit dem sichtbaren Titel gerendert. Anschließend erhält der Untertitel eine Datenbindung, der für alle Elemente usw. sichtbar gemacht wird, bis alle Phasen verarbeitet wurden.
    ```xml
    <Page
        x:Class="LotsOfItems.DeferMainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Text="{x:Bind Subtitle}" x:Phase="1"/>
                            <TextBlock Text="{x:Bind Description}" x:Phase="2"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```

3.  Wenn Sie die App jetzt ausführen und eine schnelle Verschiebung bzw. einen schnellen Bildlauf in der Rasteransicht durchführen, werden Sie erkennen, dass jedes neue Element auf dem Bildschirm angezeigt wird. Zunächst wird es als dunkelgraues Rechteck dargestellt (dank der [**ShowsScrollingPlaceholders**](/uwp/api/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders)-Eigenschaft mit dem Standardwert **true**), dann folgen Titel, Untertitel und abschließend die Beschreibung.

**Progressive Aktualisierung von Datenvorlagen mithilfe von ContainerContentChanging**

Die allgemeine Strategie für das [**ContainerContentChanging**](/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging)-Ereignis ist die Verwendung von **Opacity**, um Elemente auszublenden, die nicht sofort sichtbar sein müssen. Wenn Elemente wiederverwendet werden, behalten sie ihre alten Werte, daher möchten wir diese Elemente ausblenden, bis wir diese Werte über das neue Datenelement aktualisiert haben. Wir verwenden die **Phase**-Eigenschaft für die Ereignisargumente, um zu bestimmen, welche Elemente aktualisiert und angezeigt werden sollen. Wenn weitere Phasen erforderlich sind, registrieren wir einen Rückruf.

1.  Wir verwenden dieselbe Bindungsquelle wie für **x:Phase**.

2.  Im Folgenden sehen Sie das Markup, das `MainPage.xaml` enthält. Die Rasteransicht deklariert einen Handler für ihr [**ContainerContentChanging**](/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging)-Ereignis. Zudem enthält sie eine Elementvorlage mit Elementen, die zum Anzeigen der Eigenschaften **Title**, **Subtitle** und **Description** der **MyItem**-Klasse verwendet werden. Damit Sie mit **ContainerContentChanging** die maximalen Leistungsvorteile erreichen, werden im Markup keine Bindungen verwendet. Stattdessen werden die Werte programmgesteuert zugewiesen. Eine Ausnahme stellt hier das Element zum Anzeigen des Titels dar, das wir in Phase 0 erwarten.
    ```xml
    <Page
        x:Class="LotsOfItems.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}" ContainerContentChanging="GridView_ContainerContentChanging">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Opacity="0"/>
                            <TextBlock Opacity="0"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```
3.  Abschließend folgt hier die Implementierung des [**ContainerContentChanging**](/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging)-Ereignishandlers. Dieser Code zeigt auch, wie eine Eigenschaft vom Typ **RecordingViewModel** der **MainPage**-Klasse hinzugefügt wird, um die Klasse der Bindungsquelle über die Klasse verfügbar zu machen, die unsere Markupseite darstellt. Solange keine [{Binding}](../xaml-platform/binding-markup-extension.md)-Bindungen in Ihrer Datenvorlage vorliegen, markieren Sie das Ereignisargumentobjekt in der ersten Phase des Handlers als behandelt, um dem Element einen Hinweis zu geben, dass es keinen Datenkontext festlegen muss.
    ```csharp
    namespace LotsOfItems
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                this.ViewModel = new ExampleItemViewModel();
            }

            public ExampleItemViewModel ViewModel { get; set; }

            // Display each item incrementally to improve performance.
            private void GridView_ContainerContentChanging(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 0)
                {
                    throw new System.Exception("We should be in phase 0, but we are not.");
                }

                // It's phase 0, so this item's title will already be bound and displayed.

                args.RegisterUpdateCallback(this.ShowSubtitle);

                args.Handled = true;
            }

            private void ShowSubtitle(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 1)
                {
                    throw new System.Exception("We should be in phase 1, but we are not.");
                }

                // It's phase 1, so show this item's subtitle.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[1] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Subtitle;
                textBlock.Opacity = 1;

                args.RegisterUpdateCallback(this.ShowDescription);
            }

            private void ShowDescription(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 2)
                {
                    throw new System.Exception("We should be in phase 2, but we are not.");
                }

                // It's phase 2, so show this item's description.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[2] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Description;
                textBlock.Opacity = 1;
            }
        }
    }
    ```

4.  Wenn Sie die App jetzt ausführen und schnell schwenken oder einen schnellen Bildlauf in der Rasteransicht durchführen, sehen Sie dasselbe Verhalten wie für **x:Phase**.

## <a name="container-recycling-with-heterogeneous-collections"></a>Containerwiederverwendung mit heterogenen Sammlungen

In einigen Anwendungen benötigen Sie verschiedene Benutzeroberflächen für unterschiedliche Elementtypen innerhalb einer Sammlung. Dies kann zu Situationen führen, in denen die Virtualisierungspanels die visuellen Elemente, die zur Elementanzeige dienen, nicht wiederverwenden können. Wenn die visuellen Elemente eines Element beim Schwenken neu erstellt werden müssen, gehen viele Leistungsvorteile der Virtualisierung verloren. Mit etwas Planung können Virtualisierungspanels jedoch die Elemente wiederverwenden. Entwicklern stehen je nach Szenario zwei Optionen zur Verfügung: das [**ChoosingItemContainer**](/uwp/api/windows.ui.xaml.controls.listviewbase.choosingitemcontainer)-Ereignis oder ein Elementvorlagenselektor. Der Ansatz mit **ChoosingItemContainer** bietet eine bessere Leistung.

**Das ChoosingItemContainer-Ereignis**

[**ChoosingItemContainer**](/uwp/api/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) ist ein Ereignis, mit dem du ein Element (**ListViewItem**/**GridViewItem**) für [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)/[**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) bereitstellen kannst, wenn beim Starten oder Wiederverwenden ein neues Element benötigt wird. Sie können einen Container basierend auf dem Typ des Datenelements erstellen, das der Container anzeigen soll (im folgenden Beispiel dargestellt). **ChoosingItemContainer** ist die leistungsfähigere Möglichkeit zur Verwendung unterschiedlicher Datenvorlagen für verschiedene Elemente. Die Containerzwischenspeicherung kann mit **ChoosingItemContainer** erzielt werden. Wenn Sie beispielsweise fünf verschiedene Vorlagen besitzen, wobei eine Vorlage um eine Größenordnung häufiger als die anderen verwendet wird, können Sie mit ChoosingItemContainer nicht nur Elemente im benötigten Verhältnis erstellen, sondern auch eine passende Anzahl von Elementen im Zwischenspeicher speichern, die dann zur Wiederverwendung zur Verfügung stehen. [**ChoosingGroupHeaderContainer**](/uwp/api/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer) bietet die gleiche Funktionalität für Gruppenheader.

```csharp
// Example shows how to use ChoosingItemContainer to return the correct
// DataTemplate when one is available. This example shows how to return different 
// data templates based on the type of FileItem. Available ListViewItems are kept
// in two separate lists based on the type of DataTemplate needed.
private void ListView_ChoosingItemContainer
    (ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    // Determines type of FileItem from the item passed in.
    bool special = args.Item is DifferentFileItem;

    // Uses the Tag property to keep track of whether a particular ListViewItem's 
    // datatemplate should be a simple or a special one.
    string tag = special ? "specialFiles" : "simpleFiles";

    // Based on the type of datatemplate needed return the correct list of 
    // ListViewItems, this could have also been handled with a hash table. These 
    // two lists are being used to keep track of ItemContainers that can be reused.
    List<UIElement> relevantStorage = special ? specialFileItemTrees : simpleFileItemTrees;

    // args.ItemContainer is used to indicate whether the ListView is proposing an 
    // ItemContainer (ListViewItem) to use. If args.Itemcontainer, then there was a 
    // recycled ItemContainer available to be reused.
    if (args.ItemContainer != null)
    {
        // The Tag is being used to determine whether this is a special file or 
        // a simple file.
        if (args.ItemContainer.Tag.Equals(tag))
        {
            // Great: the system suggested a container that is actually going to 
            // work well.
        }
        else
        {
            // the ItemContainer's datatemplate does not match the needed 
            // datatemplate.
            args.ItemContainer = null;
        }
    }

    if (args.ItemContainer == null)
    {
        // see if we can fetch from the correct list.
        if (relevantStorage.Count > 0)
        {
            args.ItemContainer = relevantStorage[0] as SelectorItem;
        }
        else
        {
            // there aren't any (recycled) ItemContainers available. So a new one 
            // needs to be created.
            ListViewItem item = new ListViewItem();
            item.ContentTemplate = this.Resources[tag] as DataTemplate;
            item.Tag = tag;
            args.ItemContainer = item;
        }
    }
}
```

**Elementvorlagenselektor**

Ein Elementvorlagenselektor ([**DataTemplateSelector**](/uwp/api/Windows.UI.Xaml.Controls.DataTemplateSelector)) ermöglicht es einer App, zur Laufzeit eine andere Elementvorlage basierend auf dem Typ des angezeigten Datenelements zurückzugeben. Dadurch wird die Entwicklung zwar produktiver, es erschwert jedoch die UI-Virtualisierung, da nicht jede Elementvorlage für jedes Datenelement wiederverwendet werden kann.

Wenn ein Element (**ListViewItem**/**GridViewItem**) wiederverwendet wird, muss das Framework entscheiden, ob die zur Verwendung in der Wiederverwendungswarteschlange (einem Zwischenspeicher von Elementen, die zurzeit nicht zur Datenanzeige verwendet werden) verfügbaren Elemente über eine Elementvorlage verfügen, die der gewünschten Vorlage für das aktuelle Datenelement entspricht. Wenn die Wiederverwendungswarteschlange keine Elemente mit der passenden Elementvorlage enthält, wird ein neues Element erstellt, und die passende Elementvorlage wird dafür instanziiert. Enthält die Wiederverwendungswarteschlange ein Element mit der passenden Elementvorlage, wird dieses Element aus der Wiederverwendungswarteschlange entfernt und für das aktuelle Datenelement verwendet. Ein Elementvorlagenselektor eignet sich in Situationen, in denen nur eine geringe Anzahl von Elementvorlagen verwendet wird und eine flache Verteilung in der Sammlung der Elemente vorliegt, die unterschiedliche Elementvorlagen verwenden.

Bei einer ungleichmäßigen Verteilung von Elementen, die unterschiedliche Elementvorlagen verwenden, müssen während des Schwenkens wahrscheinlich neue Elementvorlagen erstellt werden, was viele der Vorteile der Virtualisierung zunichtemacht. Zudem berücksichtigt ein Elementvorlagenselektor nur fünf mögliche Kandidaten beim Auswerten, ob ein bestimmter Container für das aktuelle Datenelement wiederverwendet werden kann. Daher sollten Sie sorgfältig überlegen, ob Ihre Daten für die Verwendung eines Elementvorlagenselektors geeignet sind, bevor Sie ihn in Ihrer App verwenden. Wenn Ihre Sammlung überwiegend homogen ist, gibt der Selektor in den meisten Fällen (möglicherweise immer) denselben Typ zurück. Seien Sie sich jedoch bewusst, welche Folgen diese seltenen Homogenitätsausnahmen für Sie haben, und überlegen Sie, ob die Verwendung von [**ChoosingItemContainer**](/uwp/api/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) (oder zwei Elementsteuerelementen) nicht vorzuziehen wäre.

 