---
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Optimieren Ihres XAML-Markups
description: Die Analyse von XAML-Markup zum Erstellen von Objekten im Arbeitsspeicher kann für eine komplexe Benutzeroberfläche viel Zeit in Anspruch nehmen. Hier finden Sie einige Punkte, die Sie zur Optimierung der XAML-Markupanalyse, Ladezeit und Effizienz des Arbeitsspeichers für Ihre App vornehmen können.
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: aacf51cc320a0835f906e91806b686a88907640e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317620"
---
# <a name="optimize-your-xaml-markup"></a>Optimieren Ihres XAML-Markups


Die Analyse von XAML-Markup zum Erstellen von Objekten im Arbeitsspeicher kann für eine komplexe Benutzeroberfläche viel Zeit in Anspruch nehmen. Es folgen einige Vorschläge zur Optimierung der XAML-Markupanalyse, Ladezeit und Effizienz des Arbeitsspeichers Ihrer App.

Beschränken Sie beim Start der App das zu ladende XAML-Markup auf die Elemente, die für die anfängliche Benutzeroberfläche erforderlich sind. Überprüfen Sie das Markup auf der ersten Seite (einschließlich Seitenressourcen), und stellen Sie sicher, dass Sie nicht zusätzliche Elemente laden, die nicht sofort benötigt werden. Diese Elemente können aus einer Vielzahl von Quellen stammen, z. B. aus Ressourcenverzeichnissen, oder es sind Elemente, die anfänglich reduziert sind oder über anderen Elementen dargestellt werden.

Ein möglichst effizienter XAML-Code erfordert Kompromisse. Oft gibt es in einer Situation mehrere Lösungsmöglichkeiten. Wir betrachten hier einige der häufigsten Probleme und stellen Richtlinien bereit, mit deren Hilfe Sie die richtige Entscheidung für Ihre App treffen können.

## <a name="minimize-element-count"></a>Minimieren der Elementanzahl

Obwohl die XAML-Plattform eine große Anzahl von Elementen anzeigen kann, können Sie das Layout und die Darstellung Ihrer App beschleunigen, indem Sie die geringste Anzahl von Elementen verwenden, um das gewünschte Erscheinungsbild zu erreichen.

Das von Ihnen gewählte Layout für Ihre UI-Steuerelemente bedingt die Anzahl der UI-Elemente, die beim Start Ihrer App erstellt werden. Genauere Informationen zur Optimierung des Layouts finden Sie unter [Optimieren des XAML-Layouts](optimize-your-xaml-layout.md).

Die Anzahl der Elemente ist in Datenvorlagen äußerst wichtig, da jedes Element für jedes Datenelement erneut erstellt wird. Informationen zum Reduzieren der Anzahl der Elemente in einer Liste oder einem Raster finden Sie unter *Elementreduzierung pro Element* im Artikel [Optimieren von ListView- und GridView-Benutzeroberfläche](optimize-gridview-and-listview.md).

Hier betrachten wir einige weitere Möglichkeiten, um die Anzahl der Elemente zu verringern, die Ihre App beim Start laden muss.

### <a name="defer-item-creation"></a>Elementerstellung zurückstellen

Wenn Ihr XAML-Markup Elemente enthält, die nicht sofort angezeigt werden müssen, können Sie das Laden dieser Elemente zurückstellen, bis sie benötigt werden. Verzögern Sie beispielsweise die Erstellung nicht sichtbarer Inhalte wie sekundäre Registerkarten in der Benutzeroberfläche. Oder Sie zeigen Elemente in einer Rasteransicht standardmäßig an, geben aber dem Benutzer die Möglichkeit, die Daten stattdessen in einer Liste darzustellen. Sie können das Laden der Liste verzögern, bis sie benötigt wird.

Verwenden Sie das Attribut [x:Load](../xaml-platform/x-load-attribute.md) anstelle der Eigenschaft [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.Visibility), um zu steuern, wann ein Element angezeigt wird. Wenn die Sichtbarkeit eines Elements auf **Collapsed** festgelegt ist, wird es zwar während des Renderns übersprungen, aber die Objektinstanz verbraucht trotzdem Arbeitsspeicher. Durch die Verwendung von x:Load erstellt das Framework die Objektinstanz nur bei Bedarf, sodass der Speicherverbrauch geringer ist. Der Nachteil ist etwas zusätzlicher Speicherverbrauch (ca. 600 Bytes), wenn die UI nicht geladen ist.

> [!NOTE]
> Um Elemente verzögert zu laden, verwenden Sie das Attribut [x:Load](../xaml-platform/x-load-attribute.md) oder [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md). Das Attribut x:Load ist ab Windows 10 Creators Update (Version 1703, SDK-Build 15063) verfügbar. Die Mindestversion, auf die Ihr Visual Studio-Projekt abzielt, muss *Windows 10 Creators Update (10.0, Build 15063)* sein, damit x:Load verwendet werden kann. Für frühere Versionen verwenden Sie x:DeferLoadStrategy.

Die folgenden Beispiele zeigen den Unterschied zwischen der Anzahl von Elementen und der Speicherbelegung, wenn verschiedene Techniken verwendet werden, um UI-Elemente auszublenden. Ein ListView- und ein GridView-Steuerelement mit identischen Elementen werden in das Grundraster einer Seite platziert. Das ListView-Steuerelement ist nicht sichtbar, das GridView-Steuerelement wird angezeigt. Mit dem XAML-Code in jedem dieser Beispiele wird die gleiche Benutzeroberfläche auf dem Bildschirm angezeigt. Wir verwenden die [Tools für Profilerstellung und Leistung](tools-for-profiling-and-performance.md) aus Visual Studio, um Elementanzahl und Speicherbelegung zu überprüfen.

#### <a name="option-1---inefficient"></a>Option 1: Ineffizient

Hier wird das ListView-Steuerelement geladen, aber nicht angezeigt, da seine Breite 0 ist. Das ListView-Steuerelement und jedes seiner untergeordneten Elemente wird in der visuellen Struktur erstellt und in den Speicher geladen.

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <ListView x:Name="List1" Width="0">
        <ListViewItem>Item 1</ListViewItem>
        <ListViewItem>Item 2</ListViewItem>
        <ListViewItem>Item 3</ListViewItem>
        <ListViewItem>Item 4</ListViewItem>
        <ListViewItem>Item 5</ListViewItem>
        <ListViewItem>Item 6</ListViewItem>
        <ListViewItem>Item 7</ListViewItem>
        <ListViewItem>Item 8</ListViewItem>
        <ListViewItem>Item 9</ListViewItem>
        <ListViewItem>Item 10</ListViewItem>
    </ListView>

    <GridView x:Name="Grid1">
        <GridViewItem>Item 1</GridViewItem>
        <GridViewItem>Item 2</GridViewItem>
        <GridViewItem>Item 3</GridViewItem>
        <GridViewItem>Item 4</GridViewItem>
        <GridViewItem>Item 5</GridViewItem>
        <GridViewItem>Item 6</GridViewItem>
        <GridViewItem>Item 7</GridViewItem>
        <GridViewItem>Item 8</GridViewItem>
        <GridViewItem>Item 9</GridViewItem>
        <GridViewItem>Item 10</GridViewItem>
    </GridView>
</Grid>
```

Visuelle Live-Struktur mit dem geladenen ListView-Steuerelement. Die Seite enthält insgesamt 89 Elemente.

![Visuelle Struktur mit Listenansicht](images/visual-tree-1.png)

Das ListView-Steuerelement und seine untergeordneten Elemente werden in den Speicher geladen.

![Visuelle Struktur mit Listenansicht](images/memory-use-1.png)

#### <a name="option-2---better"></a>Option 2: Besser.

Hier wird die Sichtbarkeit des ListView-Steuerelements auf „Collapsed” festgelegt (der restliche XAML-Code ist mit dem Original identisch). Das ListView-Steuerelement wird in der visuellen Struktur erstellt, nicht aber seine untergeordneten Elemente. Allerdings werden sie in den Speicher geladen, und damit ist die Speicherbelegung identisch mit dem vorherigen Beispiel.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed">
```

Visuelle Live-Struktur mit dem reduzierten ListView-Steuerelement. Die Seite enthält insgesamt 46 Elemente.

![Visuelle Struktur mit reduzierter Listenansicht](images/visual-tree-2.png)

Das ListView-Steuerelement und seine untergeordneten Elemente werden in den Speicher geladen.

![Visuelle Struktur mit Listenansicht](images/memory-use-1.png)

#### <a name="option-3---most-efficient"></a>Option 3: Am effizientesten.

Hier ist für das ListView-Steuerelement das Attribut x:Load auf **False** festgelegt (der restliche XAML-Code ist mit dem Original identisch). Das ListView-Steuerelement wird nicht in der visuellen Struktur erstellt oder beim Start in den Speicher geladen.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed" x:Load="False">
```

Visuelle Live-Struktur mit dem nicht geladenen ListView-Steuerelement. Die Seite enthält insgesamt 45 Elemente.

![Visuelle Struktur mit dem nicht geladenen ListView-Steuerelement.](images/visual-tree-3.png)

Das ListView-Steuerelement und seine untergeordneten Elemente werden nicht in den Speicher geladen.

![Visuelle Struktur mit Listenansicht](images/memory-use-3.png)

> [!NOTE]
> Anzahl der Elemente und Speicherbelegung sind in diesen Beispielen sehr gering und sollen nur das Konzept veranschaulichen. In diesen Beispielen ist der Aufwand für die Verwendung von x:Load größer als die Speicherplatzersparnis, sodass die App davon nicht profitieren würde. Sie sollten mit den Profilerstellungstools untersuchen, ob Ihre App von einem verzögerten Laden profitieren würde.

### <a name="use-layout-panel-properties"></a>Verwenden von Layoutpaneleigenschaften

Layoutpanel verfügen über die Eigenschaft [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.background), sodass zum Einfärben kein [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) vor dem Panel erforderlich ist.

**Inefficient**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid>
    <Rectangle Fill="Black"/>
</Grid>
```

**Efficient**

```xaml
<Grid Background="Black"/>
```

Layoutpanel verfügen auch über integrierte Rahmeneigenschaften, sodass Sie kein [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border)-Element um ein Layoutpanel platzieren müssen Beispiele und weitere Informationen finden Sie unter [Optimieren des XAML-Layouts](optimize-your-xaml-layout.md).

### <a name="use-images-in-place-of-vector-based-elements"></a>Verwenden von Bildern anstelle von vektorbasierten Elementen

Wenn Sie das gleiche vektorbasierte Element häufig wiederverwenden, kann es effizienter sein, stattdessen ein [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image)-Element zu verwenden. Vektorbasierte Elemente können aufwendiger sein, da die CPU jedes einzelne Element separat erstellen muss. Die Bilddatei muss hingegen nur einmal decodiert werden.

## <a name="optimize-resources-and-resource-dictionaries"></a>Optimieren von Ressourcen und Ressourcenwörterbücher

Verwenden Sie möglichst [Ressourcenwörterbücher](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md), um Ressourcen, auf die an mehreren Stellen in Ihrer App verwiesen werden soll, auf einer Art globalen Ebene zu speichern. Beispielsweise Stile, Pinsel, Vorlagen usw.

Wir haben [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) dahingehend optimiert, dass nur angeforderte Ressourcen instanziiert werden. Aber es gibt Situationen, die Sie vermeiden sollten, damit Ressourcen nicht unnötig instanziiert werden.

### <a name="resources-with-xname"></a>Ressource mit „x:Name“

Verwenden Sie das [x:Key-Attribut](../xaml-platform/x-key-attribute.md), um Ihre Ressourcen zu referenzieren. Ressourcen mit dem [x:Name-Attribut](../xaml-platform/x-name-attribute.md) profitieren nicht von der Plattformoptimierung, sondern werden sofort bei der Erstellung von ResourceDictionary instanziiert. Der Grund: „x:Name“ teilt der Plattform mit, dass Ihre App Feldzugriff auf diese Ressource benötigt. Daher muss die Plattform etwas erstellen, für das ein Verweis eingerichtet werden kann.

### <a name="resourcedictionary-in-a-usercontrol"></a>ResourceDictionary in einem UserControl-Element

Ein innerhalb von [UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) definiertes ResourceDictionary führt zu Leistungseinbußen. Die Plattform erstellt für jede Instanz von UserControl eine Kopie von ResourceDictionary. Bei einem häufig verwendeten Benutzersteuerelement empfiehlt es sich daher, ResourceDictionary aus UserControl zu entfernen und auf der Seitenebene zu platzieren.

### <a name="resource-and-resourcedictionary-scope"></a>Ressourcen und ResourceDictionary-Bereich

Falls eine Seite auf ein Steuerelement oder auf eine Ressource in einer anderen Datei verweist, wird auch diese Datei vom Framework analysiert.

Da _InitialPage.xaml_ in diesem Beispiel eine Ressource von _ExampleResourceDictionary.xaml_ verwendet, muss die gesamte Datei _ExampleResourceDictionary.xaml_ beim Start analysiert werden.

**InitialPage.xaml.**

```xaml
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml.**

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
        are used in the app, but are not needed during startup.-->
</ResourceDictionary>
```

Wenn Sie eine Ressource in der gesamten App auf vielen Seiten verwenden, ist es eine bewährte Methode, diese in der Datei _App.xaml_ zu speichern, um eine Duplizierung zu vermeiden. Aber _App.xaml_ wird beim Starten der App analysiert. Daher sollte jede Ressource, die nur auf einer Seite verwendet wird (es sei denn, diese Seite ist die Ausgangsseite), unter den lokalen Ressourcen der Seite gespeichert werden. In diesem Beispiel enthält die Datei _App.xaml_ Ressourcen, die nur von einer Seite verwendet werden (bei der es sich nicht um die Ausgangsseite handelt). Dadurch wird die Startzeit der App unnötigerweise erhöht.

**App.xaml**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Application ...>
     <Application.Resources>
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/>
    </Application.Resources>
</Application>
```

**InitialPage.xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <StackPanel>
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/>
    </StackPanel>
</Page>
```

**SecondPage.xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.SecondPage" ...>
    <StackPanel>
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" />
    </StackPanel>
</Page>
```

Um dieses Beispiel effizienter zu gestalten, verschieben Sie `SecondPageTextBrush` in _SecondPage.xaml_ und `ThirdPageTextBrush` in _ThirdPage.xaml_. `InitialPageTextBrush` verbleiben im _"App.xaml"_ Anwendungsressourcen in jedem Fall beim Starten der app analysiert werden müssen.

### <a name="consolidate-multiple-brushes-that-look-the-same-into-one-resource"></a>Zusammenfassen mehrerer gleichartiger Pinsel in einer Ressource

Die XAML-Plattform speichert allgemein verwendete Objekte zwischen, um eine möglichst häufige Wiederverwendung zu ermöglichen. XAML kann jedoch nicht ohne Weiteres feststellen, ob es sich bei einem Pinsel, der in zwei unterschiedlichen Markupkomponenten deklariert ist, um denselben Pinsel handelt. Dieses Beispiel verwendet [SolidColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) zur Veranschaulichung, aber die Wahrscheinlichkeit und Bedeutung von [GradientBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientBrush) ist höher. Prüfen Sie auch auf Pinsel, die vordefinierte Farben verwenden – beispielsweise bezeichnen `"Orange"` und `"#FFFFA500"` dieselbe Farbe.

**Ineffizient.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page ... >
    <StackPanel>
        <TextBlock>
            <TextBlock.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </TextBlock.Foreground>
        </TextBox>
        <Button Content="Submit">
            <Button.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </Button.Foreground>
        </Button>
    </StackPanel>
</Page>
```

Definieren Sie den Pinsel als Ressource, um die Duplizierung zu beheben. Wenn Steuerelemente auf anderen Seiten denselben Pinsel verwenden, verschieben Sie diese in die Datei _App.xaml_.

**Effiziente.**

```xaml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>

    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## <a name="minimize-overdrawing"></a>Minimieren der Überzeichnung

Eine Überzeichnung tritt dort auf, wo mehrere Objekte dieselben Bildschirmpixel beanspruchen. Beachten Sie, dass Sie manchmal einen Kompromiss zwischen dieser Richtlinie und dem Wunsch zur Minimierung der Elementanzahl eingehen müssen.

Verwenden Sie [**DebugSettings.IsOverdrawHeatMapEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.debugsettings.isoverdrawheatmapenabled) als visuelle Diagnose. Möglicherweise erkennen Sie, dass Objekte dargestellt werden, von denen Sie sich nicht wussten, dass sie in der Szene auftreten.

### <a name="transparent-or-hidden-elements"></a>Transparente oder ausgeblendeten Elemente

Wenn ein Element nicht angezeigt wird, da es transparent ist oder von anderen Elementen verdeckt wird, löschen Sie es einfach, wenn es nicht zum Layout beiträgt. Wenn das Element nicht in seinem visuellen Anfangszustand, aber in anderen visuellen Zuständen sichtbar ist, können Sie den Zustand mit „x:Load“ steuern oder [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) auf den Wert **Collapsed** für das Element selbst festlegen und in den entsprechenden Zuständen in **Visible** ändern. Es gibt Ausnahmen von dieser Heuristik: In der Regel wird der Wert, den eine Eigenschaft in den meisten visuellen Zuständen aufweist, am besten lokal für das Element festgelegt.

### <a name="composite-elements"></a>Zusammengesetzte Elemente

Verwenden Sie ein Verbundelement, anstatt mehrere Elemente übereinander zu stapeln, um einen Effekt zu erzielen. In diesem Beispiel ist das Ergebnis eine zweifarbige Form, wobei die obere Hälfte schwarz (vom Hintergrund von [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)) und die untere Hälfte grau ist (vom halbtransparenten weißen [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Objekt, das mit einer Alpha-Überblendung über den schwarzen Hintergrund von **Grid** gelegt wird). Hier werden 150 % der Pixel gefüllt, die erforderlich sind, um das Ergebnis zu erzielen.

**Ineffizient.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Black">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/>
</Grid>
```

**Effiziente.**

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Fill="Black"/>
    <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
</Grid>
```

### <a name="layout-panels"></a>Layoutpanels

Ein Layoutpanel kann zwei Zwecke verfolgen: einem Bereich eine Farbe zuzuweisen und untergeordnete Elemente darzustellen. Wenn ein Element, das sich in der Z-Ordnung weiter hinten befindet, einem Bereich bereits eine Farbe zuweist, dann muss ein darüber befindliches Layoutpanel diesem Bereich keine Farbe zuweisen. Stattdessen kann es sich darauf konzentrieren, seine untergeordneten Elemente zu gestalten. Hier sehen Sie ein Beispiel.

**Ineffizient.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid Background="Blue"/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

**Effiziente.**

```xaml
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

Wenn für das [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) Treffertests durchgeführt werden müssen, legen Sie für den Hintergrund einen transparenten Wert fest.

### <a name="borders"></a>Umrandung

Verwenden Sie ein [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border)-Element, um einen Rahmen um ein Objekt zu zeichnen. In diesem Beispiel wird ein [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) als provisorischer Rahmen für eine [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) verwendet. Allerdings werden alle Pixel in der Mitte der Zelle überzeichnet.

**Ineffizient.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Blue" Width="300" Height="45">
    <Grid.RowDefinitions>
        <RowDefinition Height="5"/>
        <RowDefinition/>
        <RowDefinition Height="5"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="5"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="5"/>
    </Grid.ColumnDefinitions>
    <TextBox Grid.Row="1" Grid.Column="1"></TextBox>
</Grid>
```

**Effiziente.**

```xaml
 <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
     <TextBox/>
</Border>
```

### <a name="margins"></a>Ränder

Achten Sie auf die Ränder. Zwei benachbarte Elemente überlappen sich (möglicherweise versehentlich), wenn negative Ränder in die Rendergrenzen hineinreichen und eine Überzeichnung verursachen.

### <a name="cache-static-content"></a>Zwischenspeichern statischer Inhalte

Eine andere Quelle für die Überzeichnung ist eine Form, die aus vielen überlappenden Elementen besteht. Wenn Sie [CacheMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CacheMode) für die [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)-Klasse mit der Verbundform auf **BitmapCache** festlegen, rendert die Plattform das Element für eine Bitmap einmalig und verwendet dann diese Bitmap für jeden Frame (anstelle der Überzeichnung).

**Ineffizient.**

```xaml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![Venn-Diagramm mit drei farbigen Kreisen](images/solidvenn.png)

Das obige Bild ist das Ergebnis, aber hier folgt eine Karte der überzeichneten Bereiche. Ein dunkleres Rot deutet auf ein höheres Maß an Überzeichnung hin.

![Venn-Diagramm mit Überschneidungsbereichen](images/translucentvenn.png)

**Effiziente.**

```xaml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

Beachten Sie die Verwendung von [CacheMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CacheMode). Verwenden Sie dieses Verfahren nicht, wenn eine der Teilformen animiert ist, da der Bitmapcache wahrscheinlich bei jedem Frame neu generiert werden muss, was dem Zweck widerspricht.

## <a name="use-xbf2"></a>Verwendung von XBF2

Bei XBF2 handelt es sich um eine binäre Darstellung des XAML-Markups, die sämtliche Nachteile der Textanalyse zur Laufzeit vermeidet. Darüber hinaus optimiert sie den Binärcode für das Laden und die Strukturerstellung und ermöglicht die Verwendung von Fast-Path für XAML-Typen, um die Effizienz bei der Heap- und Objekterstellung (VSM, ResourceDictionary, Stile usw.) zu verbessern. Dank vollständiger Abbildung im Speicher entsteht beim Laden und Lesen einer XAML-Seite keinerlei Heap-Bedarf. Des Weiteren verringert sich der Datenträgerbedarf gespeicherter XAML-Seiten in einem APPX-Element. XBF2 zeichnet sich durch eine kompaktere Darstellung aus und kann den Datenträgerbedarf im Vergleich zu XAML/XBF1-Dateien um bis zu 50 Prozent reduzieren. So wurde bei der integrierten Foto-App etwa eine Reduzierung um rund 60 Prozent erreicht – von ehemals rund 1 MB (XBF1-Ressourcen) auf ca. 400 KB (XBF2-Ressourcen). Darüber hinaus wurden für Apps auch Verbesserungen bei CPU (15 bis 20 Prozent) und Win32-Heap (10 bis 15 Prozent) verzeichnet.

Integrierte XAML-Steuerelemente und vom Framework bereitgestellte Wörterbücher sind bereits vollständig XBF2-fähig. Achten Sie bei Ihrer eigenen App darauf, dass Ihre Projektdatei mindestens „TargetPlatformVersion 8.2“ deklariert.

Wenn Sie wissen möchten, ob Sie über XBF2 verfügen, öffnen Sie Ihre App in einem Binär-Editor. Falls das 12. und 13. Byte „00 02“ ist, haben Sie XBF2.

## <a name="related-articles"></a>Verwandte Artikel

- [Bewährte Methoden für die Leistung Ihrer app starten](best-practices-for-your-app-s-startup-performance.md)
- [Optimieren des XAML-Layouts](optimize-your-xaml-layout.md)
- [Optimieren der ListView- und GridView-Benutzeroberfläche](optimize-gridview-and-listview.md)
- [Tools zur profilerstellung und Leistung](tools-for-profiling-and-performance.md)
