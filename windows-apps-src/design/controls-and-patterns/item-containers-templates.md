---
Description: Verwenden Sie Vorlagen, um das Erscheinungsbild der Elemente in ListView- und GridView-Steuerelementen zu ändern.
title: Elementcontainer und Vorlagen
label: Item containers and templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: d8eb818d-b62e-4314-a612-f29142dbd93f
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 761cd9e6d1fc92b4919f701fdd9f8f62078faedf
ms.sourcegitcommit: b8a4b0d5a65da297290b93d73c641df3c135a086
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531663"
---
# <a name="item-containers-and-templates"></a>Elementcontainer und Vorlagen

 

Die **ListView**- und **GridView**-Steuerelemente verwalten, wie ihre Elemente angeordnet werden (horizontal, vertikal, an welcher Stelle der Umbruch in die nächste Zeile erfolgt, usw.), und wie die Benutzer mit den Elementen interagieren, nicht jedoch, wie die einzelnen Elemente auf dem Bildschirm angezeigt werden. Die Visualisierung der Elemente wird von Elementcontainern verwaltet. Wenn Sie einer Listenansicht Elemente hinzufügen, werden diese automatisch in einem Container platziert. Der Standardelementcontainer für ListView ist [ListViewItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem); für GridView ist es [GridViewItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem).

> **Wichtige APIs:** [ListView-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), [GridView-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview), [ListViewItem-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.listviewitem), [GridViewItem-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.gridviewitem), [ItemTemplate-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate), [ItemContainerStyle-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)


> [!NOTE]
> ListView und GridView sind beide von der Klasse [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase) abgeleitet, sodass sie dieselbe Funktionalität haben, Daten jedoch unterschiedlich anzeigen. In diesem Artikel beziehen sich Aussagen zur Listenansicht sowohl auf die ListView- als auch die GridView-Steuerelemente, falls nicht anders angegeben. Möglicherweise werden Klassen wie ListView oder ListViewItem genannt. Das Präfix *List* kann jedoch durch *Grid* für das entsprechende Rastersteuerelement ersetzt werden (GridView oder GridViewItem). 

## <a name="listview-items-and-gridview-items"></a>ListView- und GridView-Elemente
Wie bereits erwähnt, werden ListView-Elemente automatisch im ListViewItem-Container platziert, und GridView-Elemente werden im GridViewItem-Container abgelegt. Diese Elementcontainer sind Steuerelemente, die über eigene integrierte Formatierungen und Interaktionen verfügen, die aber auch hochgradig angepasst werden können. Vor einer Anpassung sollten Sie jedoch sicherstellen, dass Sie sich mit den empfohlenen Formatierungen und Richtlinien für ListViewItem und GridViewItem vertraut machen:

- **ListViewItems** – Die Elemente sind primär textorientiert und in ihrer Form eher länglich. Symbole oder Bilder können links neben dem Text angezeigt werden.
- **GridViewItems** – Die Elemente sind in der Regel eher quadratisch oder zumindest ein nicht sehr längliches Rechteck. Die Elemente sind bildorientiert, und Text kann die Bilder umgeben oder überlagern. 

## <a name="introduction-to-customization"></a>Einführung in die Anpassung
Containersteuerelemente (wie ListViewItem und GridViewItem) bestehen aus zwei wichtigen Komponenten, die zusammen die endgültige visuelle Darstellung für ein Element erstellen, der *Datenvorlage* und der *Steuerelementvorlage*.

- **Datenvorlage**: Sie legen eine [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) für die [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)-Eigenschaft der Listenansicht fest, um anzugeben, wie die einzelnen Datenelemente angezeigt werden.
- **Steuerelementvorlage**: Die Steuerelementvorlage ist die Komponente für die Visualisierung des Elements, für die das Framework verantwortlich ist, beispielsweise optische Zustände. Sie können die [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)-Eigenschaft verwenden, um die Steuerelementvorlage zu ändern. In der Regel tun Sie dies, um die Farben der Listenansicht entsprechend Ihrem Branding zu ändern, oder um zu ändern, wie ausgewählte Elemente angezeigt werden.

Diese Abbildung zeigt, wie die Steuerelementvorlage und die Datenvorlage zusammen die endgültige optische Darstellung für ein Element festlegen.

![Listenansicht – Steuerelement- und Datenvorlagen](images/listview-visual-parts.png)

Nachstehend finden Sie den XAML-Code zur Erstellung dieser Elemente. Die Vorlagen werden zu einem späteren Zeitpunkt erläutert.

```xaml
<ListView Width="220" SelectionMode="Multiple">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid Background="Yellow">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="54"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="44" Height="44"
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Black"
                           FontSize="14" Grid.Column="1"
                           VerticalAlignment="Center"
                           Padding="0,0,54,0"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Background" Value="LightGreen"/>
        </Style>
    </ListView.ItemContainerStyle>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

> [!IMPORTANT]
> Datenvorlagen und Steuerelementvorlagen werden verwendet, um den Stil für viele andere Steuerelemente als ListView und GridView anzupassen. Hierzu gehören Steuerelemente mit eigenen integrierten Formatierungen wie FlipView und benutzerdefiniert erstellte Steuerelemente wie ItemsRepeater. Obwohl das unten stehende Beispiel für ListView/GridView spezifisch ist, lassen sich die Konzepte auf viele andere Steuerelemente anwenden. 
 
## <a name="prerequisites"></a>Voraussetzungen

- Es wird davon ausgegangen, dass Sie wissen, wie ein Listenansicht-Steuerelement verwendet wird. Weitere Informationen finden Sie in dem Artikel [ListView und GridView](listview-and-gridview.md).
- Weiterhin wird vorausgesetzt, dass Sie die Grundlagen zu Stilen und Vorlagen kennen, auch, wie Sie einen Stil inline oder als Ressource verwenden. Weitere Informationen finden Sie unter [Formatieren von Steuerelementen](xaml-styles.md) und [Steuerelementvorlagen](control-templates.md).

## <a name="the-data"></a>Die Daten

Bevor wir tiefer in die Materie eintauchen, we Datenelemente in einer Listenansicht angezeigt werden, müssen wir die Daten, die angezeigt werden sollen, kennen. In diesem Beispiel erstellen wir einen Datentyp namens `NamedColor`. Es besteht aus einem Farbnamen, einem Farbwert und einem **SolidColorBrush**-Objekt für die Farbe, die als 3 Eigenschaften verfügbar sind, `Name`, `Color` und `Brush`.
 
Anschließend füllen wir eine **Liste** mit einem `NamedColor`-Objekt für jede benannte Farbe in der Klasse [Colors](https://docs.microsoft.com/uwp/api/windows.ui.colors). Die Liste wird als [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) für die Listenansicht festgelegt.

Nachstehend finden Sie den Code, mit dem die Klasse definiert und die `NamedColors`-Liste mit Einträgen gefüllt wird.

**C#**
```csharp
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

namespace ColorsListApp
{
    public sealed partial class MainPage : Page
    {
        // The list of colors won't change after it's populated, so we use List<T>. 
        // If the data can change, we should use an ObservableCollection<T> intead.
        List<NamedColor> NamedColors = new List<NamedColor>();

        public MainPage()
        {
            this.InitializeComponent();

            // Use reflection to get all the properties of the Colors class.
            IEnumerable<PropertyInfo> propertyInfos = typeof(Colors).GetRuntimeProperties();

            // For each property, create a NamedColor with the property name (color name),
            // and property value (color value). Add it the NamedColors list.
            for (int i = 0; i < propertyInfos.Count(); i++)
            {
                NamedColors.Add(new NamedColor(propertyInfos.ElementAt(i).Name,
                                    (Color)propertyInfos.ElementAt(i).GetValue(null)));
            }

            colorsListView.ItemsSource = NamedColors;
        }
    }

    class NamedColor
    {
        public NamedColor(string colorName, Color colorValue)
        {
            Name = colorName;
            Color = colorValue;
        }

        public string Name { get; set; }

        public Color Color { get; set; }

        public SolidColorBrush Brush
        {
            get { return new SolidColorBrush(Color); }
        }
    }
}
```

## <a name="data-template"></a>Datenvorlage

Sie geben eine Datenvorlage an, um der Listenansicht mitzuteilen, wie das Datenelement angezeigt werden soll. 

Datenelemente werden in der Listenansicht standardmäßig als Zeichenfolgendarstellung des Datenobjekts angezeigt, an das sie gebunden sind. Wenn Sie die „NamedColors“-Daten in einer Listenansicht anzeigen, ohne der Listenansicht mitzuteilen, wie sie aussehen sollten, wird der Inhalt so angezeigt, wie von der **ToString**-Methode zurückgegeben. Dies sieht dann so aus.

**XAML**
```xaml
<ListView x:Name="colorsListView"/>
```

![Listenansicht mit einer Zeichenfolgendarstellung der Elemente](images/listview-no-template.png)

Sie können die Zeichenfolgendarstellung einer bestimmten Eigenschaft des Datenelements anzeigen, indem Sie den [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) zur Eigenschaft festlegen. Hier legen Sie „DisplayMemberPath“ auf den Wert für die `Name`-Eigenschaft des `NamedColor`-Elements fest.

**XAML**
```xaml
<ListView x:Name="colorsListView" DisplayMemberPath="Name" />
```

Die Listenansicht zeigt jetzt die Elemente nach Namen sortiert an, wie in dieser hier dargestellt. Das ist bereits sinnvoller, aber diese Darstellung ist nicht sehr interessant und bewirkt, dass viele Informationen ausgeblendet bleiben.

![Listenansicht mit einer Zeichenfolgendarstellung einer Element-Eigenschaft](images/listview-display-member-path.png)

In der Regel möchten Sie eine ansprechendere Darstellung Ihrer Daten anzeigen. Um genau anzugeben, wie Elemente in der Listenansicht angezeigt werden, erstellen Sie eine [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). Der XAML-Code in der DataTemplate definiert das Layout und die Darstellung von Steuerelementen, die zum Anzeigen eines einzelnen Elements verwendet werden. Die Steuerelemente im Layout können an Eigenschaften eines Datenobjekts gebunden werden. Es ist auch möglich, statischen Inhalt intern zu definieren. Sie weisen DataTemplate der [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)-Eigenschaft des Listensteuerelements zu.

> [!IMPORTANT]
> Sie können **ItemTemplate** und **DisplayMemberPath** nicht gleichzeitig verwenden. Wenn beide Eigenschaften festgelegt sind, wird eine Ausnahmebedingung ausgelöst.

Hier definieren Sie ein DataTemplate-Objekt, durch das [Rectangle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.rectangle) in der Farbe des Elements zusammen mit dem Farbnamen und den RGB-Werten angezeigt wird. 

> [!NOTE]
> Wenn Sie die [x:Bind-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) in DataTemplate verwenden, müssen Sie DataType (`x:DataType`) für DataTemplate angeben.

**XAML**
```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:NamedColor">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition MinWidth="54"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Grid.RowDefinitions>
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Rectangle Width="44" Height="44" Fill="{x:Bind Brush}" Grid.RowSpan="2"/>
                <TextBlock Text="{x:Bind Name}" Grid.Column="1" Grid.ColumnSpan="4"/>
                <TextBlock Text="{x:Bind Color.R}" Grid.Column="1" Grid.Row="1" Foreground="Red"/>
                <TextBlock Text="{x:Bind Color.G}" Grid.Column="2" Grid.Row="1" Foreground="Green"/>
                <TextBlock Text="{x:Bind Color.B}" Grid.Column="3" Grid.Row="1" Foreground="Blue"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

So sehen die Datenelemente aus, wenn sie mit dieser Datenvorlage angezeigt werden.

![Elemente in der Listenansicht mit einer Datenvorlage](images/listview-data-template-0.png)

> [!IMPORTANT]
> Der Inhalt von ListViewItems wird standardmäßig linksbündig ausgerichtet, d. h. ihre [HorizontalContentAlignmentProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment#Windows_UI_Xaml_Controls_Control_HorizontalContentAlignment) ist auf „Links“ festgelegt. Wenn Sie über mehrere Elemente innerhalb eines ListViewItems verfügen, die horizontal aneinander angrenzen, z. B. horizontal gestapelte Elemente oder Elemente, die in derselben Rasterzeile platziert sind, werden diese alle linksbündig ausgerichtet und nur durch ihre definierten Ränder voneinander getrennt. 
<br/><br/> Damit Elemente so verteilt werden, dass sie den gesamten Raum eines ListItems füllen, müssen Sie die „HorizontalContentAlignmentProperty“ auf [Stretch ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.horizontalalignment) (Strecken) festlegen, indem Sie innerhalb Ihrer ListView einen [Setter ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.setter) verwenden:

```xaml
<ListView.ItemContainerStyle>
    <Style TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>
</ListView.ItemContainerStyle>
```


Möglicherweise möchten Sie die Daten in einem GridView anzuzeigen. Hier sehen Sie eine andere Datenvorlage, die die Daten besser geeignet für ein Rasterlayout anzeigt. Dieses Mal ist die Datenvorlage als Ressource definiert, nicht in dem XAML-Code für das GridView-Objekt.


**XAML**
```xaml
<Page.Resources>
    <DataTemplate x:Key="namedColorItemGridTemplate" x:DataType="local:NamedColor">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="96"/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
    
            <Rectangle Width="96" Height="96" Fill="{x:Bind Brush}" Grid.ColumnSpan="3" />
            <!-- Name -->
            <Border Background="#AAFFFFFF" Grid.ColumnSpan="3" Height="40" VerticalAlignment="Top">
                <TextBlock Text="{x:Bind Name}" TextWrapping="Wrap" Margin="4,0,0,0"/>
            </Border>
            <!-- RGB -->
            <Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
            <TextBlock Text="{x:Bind Color.R}" Foreground="Red"
                   Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.G}" Foreground="Green"
                   Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
                   Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
            <!-- HEX -->
            <Border Background="Gray" Grid.Row="2" Grid.ColumnSpan="3">
                <TextBlock Text="{x:Bind Color}" Foreground="White" Margin="4,0,0,0"/>
            </Border>
        </Grid>
    </DataTemplate>
</Page.Resources>

...

<GridView x:Name="colorsGridView" 
          ItemTemplate="{StaticResource namedColorItemGridTemplate}"/>
```

Wenn die Daten mithilfe dieser Datenvorlage in einem Raster angezeigt werden, sieht dies wie folgt aus.

![Elemente in der Rasteransicht mit einer Datenvorlage](images/gridview-data-template.png)

### <a name="performance-considerations"></a>Leistungsaspekte

Datenvorlagen sind der bevorzugte Weg für die Definition des Aussehens Ihrer Listenansicht. Sie können auch eine erhebliche Auswirkung auf die Leistung haben, wenn in der Liste eine große Anzahl von Elementen angezeigt wird. 

Bei einer Datenvorlage wird für jedes Element in der Listenansicht eine Instanz von jedem XAML-Elements erstellt. Die Rastervorlage im vorherigen Beispiel hat beispielsweise 10 XAML-Elemente (1 Raster, 1 Rechteck, 3 Rahmen, 5 Textblöcke). Bei einer GridView, die auf dem Bildschirm mithilfe dieser Datenvorlage 20 Elemente anzeigt, werden mindestens 200 Elemente (20 * 10 = 200) erstellt. Wenn Sie die Anzahl der Elemente in einer Datenvorlage verringern, kann die Gesamtanzahl der Elemente, die für die Listenansicht erstellt werden müssen, erheblich reduziert werden. Weitere Informationen finden Sie unter [ListView und GridView-UI Optimierung:  Reduzierung der Anzahl der Element pro Element](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview).

 Betrachten Sie diesen Ausschnitt der Rasterdatenvorlage. Sehen wir uns an, wodurch wir die Anzahl der Elemente verringern können.

**XAML**
```xaml
<!-- RGB -->
<Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
<TextBlock Text="{x:Bind Color.R}" Foreground="Red"
           Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.G}" Foreground="Green"
           Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
           Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
```

 - Erstens: Das Layout verwendet nur ein Raster. Sie verwenden ein einspaltiges Raster und platzieren diese 3 Textblöcke in ein „StackPanel“-Objekt, aber in einer Datenvorlage, die in vielfältigen Varianten erstellt wird, sollten Sie nach Möglichkeiten suchen, die Einbettung von Layoutpanels in andere Layoutpanele zu vermeiden.
 - Zweitens: Sie können ein „Border“-Steuerelement verwenden, mit dem ein Hintergrund gerendert wird, ohne dass wirklich Elemente innerhalb des „Border“-Elements positioniert werden. Ein „Border“-Element kann nur ein untergeordnetes Element haben, daher benötigen Sie ein zusätzliches Layoutpanel für die 3 „TextBlock“-Elemente in dem „Border“-Element in dem XAML-Code. Da die „TextBlock“-Elemente nicht dem „Border“-Element untergeordnet werden, benötigen Sie kein Panel, um diese Textblöcke aufzunehmen.
 - Schließlich können Sie die Textblöcke innerhalb eines StackPanel-Elements platzieren und die Rahmeneigenschaften an dem StackPanel-Element festlegen statt über eine explizites „Border“-Element. Das „Border“-Element ist jedoch ein weniger Ressourcen beanspruchendes Steuerelement als das StackPanel-Element. Damit wirkt es sich weniger nachteilig auf die Systemleistung aus, wenn das Element häufig neu gerendert wird.

### <a name="using-different-layouts-for-different-items"></a>Verwenden verschiedener Layouts für unterschiedliche Elemente


## <a name="control-template"></a>Steuerelementvorlage
Eine Steuerelementvorlage für ein Element enthält die optischen Elemente zur Anzeige des Zustands wie Auswahl, Draufzeigen und Fokus. Diese visuellen Elemente werden über oder unter der Datenvorlage gerendert. Hier ist eine Liste mit häufig verwendeten visuellen Standardelementen, wie sie von der ListView-Steuerelementvorlage gezeichnet werden.

- Draufzeigen: Eine hellgraues Rechteck, das unter der Datenvorlage gezeichnet wird.  
- Auswahl: Eine hellblaues Rechteck, das unter der Datenvorlage gezeichnet wird. 
- Tastaturfokus: Ein schwarzweiß gepunkteter Rahmen, der über der Datenvorlage gezeichnet wird. 

![Visuelle Elemente zum Anzeigen des Zustands in Listenansichten](images/listview-state-visuals.png)

Die Listenansicht kombiniert die Elemente aus der Datenvorlage und aus der Steuerelementvorlage, um die endgültigen visuellen Elemente zu erstellen, die auf dem Bildschirm gerendert werden. Hier werden die visuellen Elemente für den Zustand im Kontext einer Listenansicht angezeigt.

![Listenansicht mit Elementen in unterschiedlichen Zuständen](images/listview-states.png)

### <a name="listviewitempresenter"></a>ListViewItemPresenter

Wie bereits zuvor bei den Datenvorlagen erwähnt kann die Anzahl der für die einzelnen Elemente erstellten XAML-Elemente XAML erhebliche Auswirkungen auf die Leistung einer Listenansicht haben. Da die Datenvorlage und die Steuerelementvorlage kombiniert werden, um die einzelnen Elemente anzuzeigen, stellen auch die Elemente aus beiden Vorlagen einen Faktor bei der Bestimmung der tatsächlichen Anzahl der für die Anzeige benötigten Elemente dar.

Die Steuerelemente „ListView“ und „GridView“ sind dahingegehend optimiert, dass die Anzahl der pro Element erstellten XAML-Elemente reduziert wird. Die visuellen Elemente für **ListViewItem** werden über den [ListViewItemPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) erstellt. Dies ist ein spezielles XAML-Element, das komplexe visuelle Objekte für Fokus, Auswahl und andere visuelle Zustände anzeigt, ohne dass zahlreiche UIElements verwaltet werden müssen.
 
> [!NOTE]
> In UWP-Apps für Windows 10 verwendet sowohl **ListViewItem** als auch **GridViewItem** den **ListViewItemPresenter**. „GridViewItemPresenter“ ist veraltet und sollte nicht mehr verwendet werden. ListViewItem und GridViewItem werden standardmäßig unterschiedlich angezeigt, weil sie für ListViewItemPresenter unterschiedliche Eigenschaftswerte festlegen.)

Um das Aussehen der Elementcontainer zu ändern, verwenden Sie die [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)-Eigenschaft und stellen Sie einen [Stil](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style) bereit, indem Sie für den zugehörigen [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.targettype) als Wert **ListViewItem** oder **GridViewItem** festlegen.

In diesem Beispiel fügen Sie dem ListViewItem Abstände hinzu, um die Elemente in der Liste deutlicher voneinander zu trennen.

```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <!-- DataTemplate XAML shown in previous ListView example -->
    </ListView.ItemTemplate>

    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Padding" Value="0,4"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

Jetzt sieht die Listenansicht mit etwas mehr Platz zwischen den Elementen so aus.

![Elemente in der Listenansicht mit Abständen](images/listview-data-template-1.png)

In dem Standardstil für ListViewItem ist die **ContentMargin**-Eigenschaft über ein [TemplateBinding](https://docs.microsoft.com/windows/uwp/xaml-platform/templatebinding-markup-extension) mit der **Padding**-Eigenschaft (`<ListViewItemPresenter ContentMargin="{TemplateBinding Padding}"/>`) des ListViewItem-Objekts verknüpft. Wenn wir die „Padding“-Eigenschaft festlegen, wird dieser Wert eigentlich an die „ContentMargin“-Eigenschaft des „ListViewItemPresenter“ übergeben.

Um andere ListViewItemPresenter-Eigenschaften zu ändern, die nicht per Vorlage an ListViewItems-Eigenschaften gebunden sind, müssen Sie die Vorlage für ListViewItem mit einem neuen ListViewItemPresenter umschreiben, dessen Eigenschaften Sie dann ändern können. 

> [!NOTE]
> Mit den Standardformaten von ListViewItem und GridViewItem werden zahlreiche Eigenschaften für ListViewItemPresenter festgelegt. Es wird empfohlen, mit einer Kopie des Standardstils zu beginnen und nur die Eigenschaften zu ändern, bei denen dies erforderlich ist. Andernfalls werden die visuellen Elemente wahrscheinlich nicht so wie angezeigt, die Sie es erwarten, da einige Eigenschaften nicht richtig festgelegt sein werden.

**So erstellen Sie eine Kopie der Standardvorlage in Visual Studio**
 
1. Öffnen Sie das Dokumentgliederungsfenster (**Ansicht > Andere Fenster > Dokumentgliederung**).
2. Wählen Sie das Listen- oder Rasterelement aus, das Sie ändern möchten. In diesem Beispiel ändern Sie das `colorsGridView`-Element.
3. Klicken mit der rechten Maustaste, und wählen Sie **Weitere Vorlagen bearbeiten > Erzeugten Elementcontainer bearbeiten (ItemContainerStyle) > Kopie bearbeiten**.
    ![Visual Studio-Editor](images/listview-itemcontainerstyle-vs.png)
4. Geben Sie im Dialogfeld „Stilressource erstellen“ einen Namen für den Stil ein. In diesem Beispiel verwenden Sie `colorsGridViewItemStyle`.
    ![Visual Studio-Dialogfeld „Stilressource erstellen“ (images/listview-style-resource-vs.png)

Anschließend wird Ihrer App eine Kopie des Standardstils als Ressource hinzugefügt, und die **GridView.ItemContainerStyle**-Eigenschaft wird auf diese Ressource festgelegt, wie in diesem XAML-Code dargestellt. 

```xaml
<Style x:Key="colorsGridViewItemStyle" TargetType="GridViewItem">
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="Background" Value="Transparent"/>
    <Setter Property="Foreground" Value="{ThemeResource SystemControlForegroundBaseHighBrush}"/>
    <Setter Property="TabNavigation" Value="Local"/>
    <Setter Property="IsHoldingEnabled" Value="True"/>
    <Setter Property="HorizontalContentAlignment" Value="Center"/>
    <Setter Property="VerticalContentAlignment" Value="Center"/>
    <Setter Property="Margin" Value="0,0,4,4"/>
    <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
    <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="GridViewItem">
                <ListViewItemPresenter 
                    CheckBrush="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                    ContentMargin="{TemplateBinding Padding}" 
                    CheckMode="Overlay" 
                    ContentTransitions="{TemplateBinding ContentTransitions}" 
                    CheckBoxBrush="{ThemeResource SystemControlBackgroundChromeMediumBrush}" 
                    DragForeground="{ThemeResource ListViewItemDragForegroundThemeBrush}" 
                    DragOpacity="{ThemeResource ListViewItemDragThemeOpacity}" 
                    DragBackground="{ThemeResource ListViewItemDragBackgroundThemeBrush}" 
                    DisabledOpacity="{ThemeResource ListViewItemDisabledThemeOpacity}" 
                    FocusBorderBrush="{ThemeResource SystemControlForegroundAltHighBrush}" 
                    FocusSecondaryBorderBrush="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}" 
                    PointerOverForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    PressedBackground="{ThemeResource SystemControlHighlightListMediumBrush}" 
                    PlaceholderBackground="{ThemeResource ListViewItemPlaceholderBackgroundThemeBrush}" 
                    PointerOverBackground="{ThemeResource SystemControlHighlightListLowBrush}" 
                    ReorderHintOffset="{ThemeResource GridViewItemReorderHintThemeOffset}" 
                    SelectedPressedBackground="{ThemeResource SystemControlHighlightListAccentHighBrush}" 
                    SelectionCheckMarkVisualEnabled="True" 
                    SelectedForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    SelectedPointerOverBackground="{ThemeResource SystemControlHighlightListAccentMediumBrush}" 
                    SelectedBackground="{ThemeResource SystemControlHighlightAccentBrush}" 
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"/>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>

...

<GridView x:Name="colorsGridView" ItemContainerStyle="{StaticResource colorsGridViewItemStyle}"/>
```

Jetzt können Sie Eigenschaften für den ListViewItemPresenter ändern, so dass Sie die Kontrollkästchen für Auswahl, die Positionierung der Elemente und die Pinselfarben für die visuelle Zustände ändern können. 

#### <a name="inline-and-overlay-selection-visuals"></a>Inline und Overlay Auswahlelemente

ListView und GridView weisen, je nach Steuerelement und [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode), auf unterschiedliche Weise darauf hin, welche Elemente ausgewählt sind. Weitere Informationen zur Auswahl der Listenansicht finden Sie unter [ListView und GridView](listview-and-gridview.md). 

Wenn **SelectionMode** auf **Multiple** festgelegt ist, wird in der Steuerelementvorlage für das Element ein Kontrollkästchen zur Auswahl angezeigt. Sie können im Mehrfachauswahlmodus das Kontrollkästchen zur Auswahl über die [SelectionCheckMarkVisualEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled)-Eigenschaft deaktivieren. Diese Eigenschaft wird jedoch in den anderen Auswahlmodi ignoriert, daher können Sie das Kontrollkästchen im erweiterten oder im Einzelauswahlmodus nicht aktivieren.

Sie können über die [CheckMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode)-Eigenschaft festlegen, ob das Kontrollkästchen im Inlineformat oder im Overlayformat angezeigt wird.

- **Inline**: Bei diesem Stil wird das Kontrollkästchen links neben dem Inhalt angezeigt. Der Hintergrund des Elementcontainers wird farbig hervorgehoben, um darauf hinzuweisen, dass es ausgewählt ist. Dies ist der Standardstil für ListView.
- **Overlay** (Überlagerung): Bei diesem Stil wird das Kontrollkästchen über dem Inhalt angezeigt. Es wird nur der Rahmen des Elementcontainers farbig hervorgehoben, um darauf hinzuweisen, dass es ausgewählt ist. Dies ist der Standardstil für GridView.

Die folgende Tabelle zeigt die visuellen Standardelemente, mit denen auf eine Auswahl hingewiesen wird.

SelectionMode:&nbsp;&nbsp; | Einzelauswahl/erweitert | Mehrfachauswahl
---------------|-----------------|---------
Inline | ![Einfache oder erweiterte Inlineauswahl](images/listview-single-selection.png) | ![Inline-Mehrfachauswahl](images/listview-multi-selection.png)
Überlagerung | ![Einfache oder erweiterte Auswahl von Überlagerungen](images/gridview-single-selection.png) | ![Mehrfachauswahl von Überlagerungen](images/gridview-multi-selection.png)

> [!NOTE]
> In diesem und den folgenden Beispielen werden einfache Zeichenfolgen-Datenelemente ohne Datenvorlagen angezeigt, um zu zeigen, wie sich die visuellen Elemente der Steuerelementvorlage auswirken.

Es gibt auch verschiedene Pinseleigenschaften, mit denen die Farben des Kontrollkästchens geändert werden können. Wir sehen uns nun diese und weitere Pinseleigenschaften an.

#### <a name="brushes"></a>Pinsel 

Viele Eigenschaften legen den Pinsel für die verschiedenen visuellen Zustände fest. Möglicherweise möchten Sie diese anpassen, beispielsweise, um Ihre App mit Ihrem Branding zu versehen. 

Diese Tabelle enthält häufig verwendete visuelle Zustände und Auswahlzustände für ListViewItem, sowie die Pinsel zum Rendern der visuellen Elemente für die einzelnen Zustände. Die Bilder zeigen die Effekte der Pinsel auf die visuellen Inline- und Überlagerungsstile für die Auswahl.

> [!NOTE]
> In dieser Tabelle sind die geänderten Farbwerte für die Pinsel hartcodierte benannte Farben, und die Farben werden ausgewählt, um deutlicher zu machen, an welchen Stellen in der Vorlage sie angewendet werden. Diese sind nicht die Standardfarben für die visuellen Zustände. Wenn Sie die Standardfarben in der App ändern, sollten Sie Pinselressourcen verwenden, um die Farbwerte entsprechend der Standardvorlage zu ändern.

Zustand/Pinselname | Inlineformat | Overlaystil
------------|--------------|--------------
<b>Normal</b><ul><li><b>CheckBoxBrush="Red"</b></li></ul> | ![Inlineelementauswahl (normal)](images/listview-item-normal.png) | ![Overlayelementauswahl (normal)](images/gridview-item-normal.png)
<b>PointerOver</b><ul><li><b>PointerOverForeground="DarkOrange"</b></li><li><b>PointerOverBackground="MistyRose"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![Inline-Elementauswahlzeiger über](images/listview-item-pointerover.png) | ![Overlyelementauswahl (Hover)](images/gridview-item-pointerover.png)
<b>Gedrückt</b><ul><li><b>PressedBackground="LightCyan"</b></li><li>PointerOverForeground="DarkOrange"</li><li>CheckBoxBrush="Red"</li></ul> | ![Inlineelementauswahl (gedrückt)](images/listview-item-pressed.png) | ![Overlayelementauswahl (gedrückt)](images/gridview-item-pressed.png)
<b>Ausgewählt</b><ul><li><b>SelectedForeground="Navy"</b></li><li><b>SelectedBackground="Khaki"</b></li><li><b>CheckBrush="Green"</b></li><li>CheckBoxBrush="Red" (nur Inline)</li></ul> | ![Inlineelementauswahl aktiviert](images/listview-item-selected.png) | ![Overlayelementauswahl aktiviert](images/gridview-item-selected.png)
<b>PointerOverSelected</b><ul><li><b>SelectedPointerOverBackground="Lavender"</b></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (nur Overlay)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (nur Inline)</li></ul> | ![Inline-Element Auswahlzeiger über ausgewählt](images/listview-item-pointeroverselected.png) | ![Overlayelementauswahl (Hover)](images/gridview-item-pointeroverselected.png)
<b>PressedSelected</b><ul><li><b>SelectedPressedBackground="MediumTurquoise"</b></li></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (nur Overlay)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (nur Inline)</li></ul> | ![Inlineelementauswahl (markiert)](images/listview-item-pressedselected.png) | ![Overlayelementauswahl (markiert)](images/gridview-item-pressedselected.png)
<b>Im Vordergrund</b><ul><li><b>FocusBorderBrush="Crimson"</b></li><li><b>FocusSecondaryBorderBrush="Gold"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![Inlineelementauswahl (Vordergrund)](images/listview-item-focused.png) | ![Overlayelementauswahl (Vordergrund)](images/gridview-item-focused.png)

ListViewItemPresenter hat noch andere Pinseleigenschaften für Datenplatzhalter und Drag&Drop-Zustände. Wenn Sie in der Listenansicht inkrementelles Laden oder Drag&Drop verwenden, sollten Sie überlegen, ob Sie auch diese zusätzlichen Pinseleigenschaften ändern müssen. Informationen finden Sie in der ListViewItemPresenter-Klasse mit einer vollständigen Liste der Eigenschaften, die Sie ändern können. 

### <a name="expanded-xaml-item-templates"></a>Erweiterte XAML-Elementvorlagen

Wenn Sie weitere Änderungen vornehmen müssen als es von den **ListViewItemPresenter** Eigenschaften erlaubt ist – Wenn Sie die Position des Kontrollkästchens ändern müssen z. B. - können Sie die *ListViewItemExpanded* oder *GridViewItemExpanded* Vorlagen verwenden. Diese Vorlagen sind in den Standardstilen in „generic.xaml“ enthalten. Sie basieren auf dem standardmäßigen XAML-Konstruktionsschema für alle visuellen Elemente der einzelnen UIElements.

Wie bereits erwähnt, hat die Anzahl von UIElements in einer Elementvorlage erhebliche Auswirkungen auf die Leistung der Listenansicht. Wenn Sie ListViewItemPresenter durch erweiterte XAML-Vorlagen ersetzen, erhöht sich die Anzahl der Elemente erheblich. Wenn in der Listenansicht eine große Anzahl von Elementen angezeigt wird oder die Leistung von Bedeutung ist, wird davon abgeraten.

> [!NOTE]
> **ListViewItemPresenter** wird nur unterstützt, wenn [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) der Listenansicht [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid) oder [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel) entspricht. Wenn Sie die ItemsPanel ändern und [VariableSizedWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid), [WrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.wrapgrid) oder [StackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel) verwenden, wird die Elementvorlage in der erweiterten XAML-Vorlage automatisch gewechselt. Weitere Informationen finden Sie unter [Optimieren der ListView- und GridView-Benutzeroberfläche](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview).

Um eine erweiterte XAML-Vorlage anzupassen, müssen Sie eine Kopie von Ihrer App erstellen und die **ItemContainerStyle**-Eigenschaft auf diese Kopie festlegen.

**So kopieren Sie die erweiterte Vorlage**
1. Legen Sie die ItemContainerStyle-Eigenschaft für Ihren ListView oder GridView wie hier dargestellt fest.
    ```xaml
    <ListView ItemContainerStyle="{StaticResource ListViewItemExpanded}"/>
    <GridView ItemContainerStyle="{StaticResource GridViewItemExpanded}"/>
    ```
2. Erweitern Sie im Eigenschaftenbereich von Visual Studio den Abschnitt „Verschiedenes“, und suchen Sie die „ItemContainerStyle“-Eigenschaft. (Stellen Sie sicher, dass ListView oder GridView ausgewählt ist.)
3. Klicken Sie auf den Eigenschaftenmarker für die ItemContainerStyle-Eigenschaft. (Das ist das kleine Feld neben dem Textfeld. Es wird grün angezeigt, um darauf hinzuweisen, dass eine StaticResource festgelegt ist.) Das Eigenschaftsmenü wird geöffnet.
4. Wählen Sie im Eigenschaftsmenü die Option **In neue Ressource konvertieren** aus. 
    
    ![Visual Studio-Eigenschaftenmenü](images/listview-convert-resource-vs.png)
5. Geben Sie im Dialogfeld „Stilressource erstellen“ einen Namen für die Ressource ein, und klicken Sie dann auf „OK“.

Eine Kopie der erweiterten Vorlage von generic.xaml wird in Ihrer App erstellt, die Sie bei Bedarf ändern können.


## <a name="related-articles"></a>Verwandte Artikel

- [Listen](lists.md)
- [ListView und GridView](listview-and-gridview.md)

