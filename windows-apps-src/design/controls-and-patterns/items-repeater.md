---
description: ItemsRepeater ist ein einfaches Steuerelement zum Generieren und Darstellen einer Sammlung von Elementen.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 43434a0354a39ae37798e959a9eb919465989dba
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034573"
---
# <a name="itemsrepeater"></a>ItemsRepeater

Verwenden Sie ein [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)-Steuerelement, um benutzerdefinierte Sammlungsoberflächen mit einem flexiblen Layoutsystem, benutzerdefinierten Ansichten und Virtualisierung zu erstellen.

Im Gegensatz zu [ListView](/uwp/api/windows.ui.xaml.controls.listview) stellt [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) keine umfassende Benutzeroberfläche bereit– „ItemsRepeater“ hat keine Standardbenutzeroberfläche und stellt keine Richtlinien hinsichtlich Fokus, Auswahl, oder Benutzerinteraktion bereit. Stattdessen ist das Steuerelement ein Baustein, den du dazu verwenden kannst, deine eigenen einzigartigen sammlungsbasierten Oberflächen und benutzerdefinierten Steuerelemente zu erstellen. Weil es keine integrierte Richtlinien hat, können Sie Richtlinien zuordnen, um die von Ihnen geforderte Benutzeroberfläche zu erstellen. Beispielsweise können Sie das zu verwendende Layout, die Tastaturunterstützungsrichtlinie, die Auswahlrichtlinie usw. definieren.

Vom Konzept her können Sie sich [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) als einen datengesteuerten Bereich statt als ein vollständiges Steuerelement wie „ListView“ vorstellen. Sie geben eine Sammlung von anzuzeigenden Datenelementen, eine Elementvorlage, über die ein Benutzeroberflächenelement für jedes Datenelement generiert wird, und ein Layout an, das die Größe und die Position der Elemente bestimmt. „ItemsRepeater“ erstellt dann anhand der Datenquelle untergeordnete Elemente und zeigt diese entsprechend der Elementvorlage und dem Layout an. Die angezeigten Elemente müssen nicht homogen sein, da „ItemsRepeater“ Inhalte, die den Datenelementen entsprechen, anhand von Kriterien laden kann, die Sie in einer Datenvorlagenauswahl angeben.

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Das Steuerelement **ItemsRepeater** ist in der Bibliothek „Windows UI“ enthalten, einem NuGet-Paket mit neuen Steuerelementen und Benutzeroberflächenfeatures für Windows-Apps. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **APIs der Bibliothek „Windows UI“** [ItemsRepeater-Klasse](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
>
> **Plattform-APIs:** [ScrollViewer-Klasse](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie ein [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)-Steuerelement, um eine benutzerdefinierte Anzeige für eine Sammlung von Daten zu erstellen. Obwohl es verwendet werden kann, einen einfachen Satz von Elementen darzustellen, werden Sie es häufig wahrscheinlich als das Anzeigeelement in der Vorlage eines benutzerdefinierten Steuerelements verwenden.

Wenn Sie ein sofort einsatzbereites Steuerelement benötigen, um bei minimaler Anpassung Daten in einer Liste oder einem Raster anzuzeigen, sollten Sie ein [ListView](/uwp/api/windows.ui.xaml.controls.listview)- oder [GridView](/uwp/api/windows.ui.xaml.controls.gridview)-Steuerelement verwenden.

„ItemsRepeater“ hat keine integrierte Sammlung für Elemente (Items-Sammlung). Wenn Sie statt einer Bindung an eine separate Datenquelle direkt eine Items-Sammlung bereitstellen müssen, benötigen Sie wahrscheinlich eine Oberfläche mit eingeschränkterer Richtlinie und sollten [ListView](/uwp/api/windows.ui.xaml.controls.listview) oder [GridView](/uwp/api/windows.ui.xaml.controls.gridview) verwenden.

Sowohl [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) als auch „ItemsRepeater“ ermöglichen anpassbare Sammlungsoberflächen, aber im Gegensatz zu „ItemsControl“ unterstützt „ItemsRepeater“ das Virtualisieren von Benutzeroberflächenlayouts. Es empfiehlt sich, „ItemsRepeater“ statt „ItemsControl“ zu verwenden, egal, ob nur einige Datenelemente dargestellt werden sollen oder ob ein benutzerdefiniertes Sammlungssteuerelement erstellt werden soll.

> [!NOTE]
> Haben Sie eine Situation, für die Sie der Meinung sind, dass „ItemsControl“ Ihren Anforderungen entspricht, „ItemsRepeater“ dagegen nicht, bitten wir Sie, uns dies als Feedback zum [GitHub-Projekt der Windows-UI-Bibliothek](https://github.com/Microsoft/microsoft-ui-xaml/issues) wissen zu lassen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>Scrollen mit „ItemsRepeater“

[**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) ist nicht aus [**Control**](/uwp/api/windows.ui.xaml.controls.control) abgeleitet und hat somit keine Steuerelementvorlage. Aus diesem Grund enthält „ItemsRepeater“ kein integriertes Scrollen, wie dies für „ListView“ oder andere Sammlungssteuerelemente der Fall ist.

Wenn Sie ein **ItemsRepeater** -Steuerelement verwenden, sollten Sie Scrollfunktionalität bereitstellen, indem Sie es in ein [**ScrollViewer**](/uwp/api/windows.ui.xaml.controls.scrollviewer)-Steuerelement einbinden.

> [!NOTE]
> Wird Ihre App unter früheren Versionen von Windows ausgeführt – Versionen, die *vor* Windows 10, Version 1809, veröffentlicht wurden –, müssen Sie das **ScrollViewer** -Steuerelement im [**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost)-Steuerelement hosten. 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> Wird Ihre App nur unter Windows 10 ab Version 1809 ausgeführt, besteht keine Notwendigkeit, [**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost) zu verwenden.
>
> Vor Windows 10, Version 1809, ist in **ScrollViewer** die Schnittstelle [**IScrollAnchorProvider**](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) nicht implementiert, die für **ItemsRepeater** benötigt wird.  Das **ItemsRepeaterScrollHost** -Steuerelement ermöglicht das Koordinieren von **ItemsRepeater** und **ScrollViewer** unter früheren Versionen, um die sichtbaren Positionen von Elementen, die der Benutzer anzeigt, ordnungsgemäß beizubehalten.  Andernfalls kann es passieren, dass die Elemente verschoben werden oder plötzlich verschwinden, wenn die Elemente in der Liste geändert werden oder wenn die Größe der App geändert wird.

## <a name="create-an-itemsrepeater"></a>Erstellen eines „ItemsRepeater“-Steuerelements

Um ein [**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)-Steuerelement verwenden zu können, müssen ihm die anzuzeigenden Daten zuordnen, indem Sie die **ItemsSource** -Eigenschaft festlegen. Danach teilen Sie ihm mit, wie die Elemente angezeigt werden sollen, indem Sie die [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)-Eigenschaft festlegen.

### <a name="itemssource"></a>ItemsSource

Um die Ansicht aufzufüllen, legen Sie die [**ItemsSource**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource)-Eigenschaft auf eine Sammlung von Datenelementen fest. Hier wird die **ItemsSource** -Eigenschaft im Code direkt auf eine Instanz einer Sammlung festgelegt.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

Sie können die **ItemsSource** -Eigenschaft auch an eine Sammlung in XAML binden. Weitere Informationen zur Datenbindung finden Sie unter [Übersicht über Datenbindung](../../data-binding/data-binding-quickstart.md).


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
Um anzugeben, wie ein Datenelement visualisiert wird, legen Sie die [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)-Eigenschaft auf eine [**DataTemplate**](/uwp/api/windows.ui.xaml.datatemplate)- oder [ **DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector)-Instanz fest, die Sie definiert haben. Die Datenvorlage definiert, wie die Daten visualisiert werden. Standardmäßig wird das Element in der Ansicht mit einer **TextBlock** -Instanz angezeigt, für die die Zeichenfolgendarstellung des Datenobjekts verwendet wird.

In der Regel möchten Sie jedoch eine ansprechendere Darstellung Ihrer Daten anzeigen, indem Sie eine Vorlage verwenden, in der das Layout und das Aussehen der Steuerelemente definiert sind, in denen Sie ein einzelnes Elements anzeigen. Die Steuerelemente, die Sie in der Vorlage verwenden, können an die Eigenschaften eines Datenobjekts gebunden sein oder statischen Inhalt haben, der intern definiert ist.

#### <a name="datatemplate"></a>DataTemplate
In diesem Beispiel ist das Datenobjekt eine einfache Zeichenfolge. Die **DataTemplate** -Instanz enthält ein Bild links neben dem Text und legt für die **TextBlock** -Instanz fest, dass die Zeichenfolge in Blaugrün angezeigt werden soll.

> [!NOTE]
> Wenn Sie die [x:Bind-Markuperweiterung](../../xaml-platform/x-bind-markup-extension.md) in **DataTemplate** verwenden, müssen Sie „DataType“ (`x:DataType`) für „DataTemplate“ angeben.

```xaml
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
```

Nachstehend sind die Elemente so dargestellt, wie sie mit dieser **DataTemplate** -Instanz angezeigt würden.

![Elemente, die mit einer Datenvorlage angezeigt werden](images/listview-itemstemplate.png)

Die Anzahl der Objekte, die in der **DataTemplate** -Instanz für ein Element verwendet werden, kann einen erheblichen Einfluss auf die Leistung haben, wenn in Ihrer Ansicht sehr viele Elemente angezeigt werden. Weitere Informationen sowie Beispiele zur Verwendung von **DataTemplate** -Instanzen, um das Aussehen von Elementen in Ihrer Liste zu definieren, finden Sie unter [Elementcontainer und Vorlagen](item-containers-templates.md).

> [!TIP]
> Wenn Sie der Einfachheit halber die Vorlage intern deklarieren möchten, statt dass sie als statische Ressource referenziert wird, können Sie die **DataTemplate** - oder **DataTemplateSelector** -Instanz als direktes untergeordnetes Element des **ItemsRepeater** -Steuerelements angeben.  Sie wird als der Wert der **ItemTemplate** -Eigenschaft zugewiesen. Folgendes ist beispielsweise gültig:
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> Im Gegensatz zu **ListView** und anderen Sammlungssteuerelementen umhüllt **ItemsRepeater** die Elemente aus einer **DataTemplate** -Instanz nicht mit einem zusätzlichen Elementcontainer, der Standardrichtlinien enthält, etwa Ränder, Auffüllung, visuelle Auswahlkomponenten oder ein Zeiger über einem visuellen Zustand. Stattdessen präsentiert **ItemsRepeater** nur den Inhalt, der in der **DataTemplate** -Instanz definiert ist. Wenn Sie möchten, dass Ihre Elemente genau so aussehen wie ein Listenansichtselement, können Sie explizit einen Container, z. B. **ListViewItem** , in Ihre Datenvorlage einfügen. **ItemsRepeater** zeigt die visuellen **ListViewItem** -Komponenten an, verwendet aber nicht automatisch weitere Funktionalität, etwa Auswahl oder Anzeigen des Multiauswahl-Kontrollkästchen.
>
> Analog können Sie, wenn Ihre Datensammlung eine Sammlung von Steuerelementen ist, z. B. **Button** (`List<Button>`), eine **ContentPresenter** -Instanz in Ihrer **DataTemplate** -Instanz anordnen, um das Steuerelement anzuzeigen.

#### <a name="datatemplateselector"></a>DataTemplateSelector

Die Elemente, die Sie in der Ansicht anzeigen, müssen nicht denselben Typ haben. Sie können die [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)-Eigenschaft mit einer [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector)-Instanz bereitstellen, um anhand von Kriterien, die Sie angegeben haben, verschiedene **DataTemplate** -Instanzen auszuwählen.

In diesem Beispiel wird davon ausgegangen, dass eine **DataTemplateSelector** -Instanz definiert ist, in der zwischen zwei unterschiedlichen **DataTemplate** -Instanzen entschieden wird, um ein „Large“- und ein „Small“-Element darzustellen.

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

Wenn Sie eine **DataTemplateSelector** -Instanz definieren, die mit **ItemsRepeater** verwendet werden soll, müssen Sie nur eine Überschreibung für die [**SelectTemplateCore(Object)**](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_)-Methode implementieren. Weitere Informationen und Beispiele finden Sie unter [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Für die Art und Weise, wie Elemente in komplexeren Szenarien erstellt werden, können Sie alternativ zu **DataTemplate** -Instanzen Ihre eigene [**Windows.UI.Xaml.Controls.IElementFactory**](/uwp/api/windows.ui.xaml.controls.ielementfactory)-Instanz implementieren, um diese als **ItemTemplate** -Instanz zu verwenden.  Sie ist für das Generieren von Inhalt verantwortlich, wenn dieser angefordert wird.

## <a name="configure-the-data-source"></a>Konfigurieren der Datenquelle

Verwenden Sie die [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource)-Eigenschaft, um die Sammlung anzugeben, mit der der Inhalt von Elementen generiert werden soll. Sie können die „ItemsSource“-Eigenschaft auf jeden Typ festlegen, der **IEnumerable** implementiert. Zusätzliche Sammlungsschnittstellen, die durch Ihre Datenquelle implementiert sind, bestimmen, welche Funktionalität für die „ItemsRepeater“-Instanz verfügbar ist, um mit Ihren Daten zu interagieren.

In dieser Liste sind die verfügbaren Schnittstellen aufgeführt und ist beschrieben, wann die jeweilige Schnittstelle in Betracht kommt.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - Kann für kleine statische Datasets verwendet werden.

    Für die Datenquelle muss mindestens die „IEnumerable / IIterable“-Schnittstelle implementiert werden. Ist dies alles, was unterstützt wird, durchläuft das Steuerelement alle Inhalte einmal, um eine Kopie zu erstellen, die es verwenden kann, um über einen Indexwert auf Elemente zuzugreifen.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - Kann für statische schreibgeschützte Datasets verwendet werden.

    Ermöglicht dem Steuerelement, über den Index auf Elemente zuzugreifen, und vermeidet redundante interne Kopien.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - Kann für statische Datasets verwendet werden.

    Ermöglicht es dem Steuerelement, über den Index auf Elemente zuzugreifen, und vermeidet redundante interne Kopien.

    **Warnung** : [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) muss implementiert sein, damit Änderungen an der Liste oder dem Vektor in die Benutzeroberfläche übernommen werden.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - Empfohlen, um Änderungsbenachrichtigungen zu unterstützen.

    Ermöglicht es dem Steuerelement, auf Änderungen in der Datenquelle zu überwachen und diese Änderungen in die Benutzeroberfläche zu übernehmen.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - Unterstützt Änderungsbenachrichtigung.

    Hiermit wird es dem Steuerelement, wie bei der **INotifyCollectionChanged** -Schnittstelle, ermöglicht, auf Änderungen in der Datenquelle zu überwachen und zu reagieren.

    **Warnung** : „Windows.Foundation.IObservableVector\<T>“ unterstützt keine Aktion zum Verschieben (Move-Aktion). Dies kann dazu führen, dass die Benutzeroberfläche für ein Element ihren visuellen Zustand verliert.  Beispiel: Ein Element, das derzeit ausgewählt ist oder den Fokus hat, verliert den Fokus verliert und ist nicht mehr ausgewählt, wenn das Verschieben durch ein Entfernen (Remove) gefolgt von einem Hinzufügen (Add) erfolgt.

    „Platform.Collections.Vector\<T>“ verwendet „IObservableVector\<T>“ und hat dieselbe Einschränkung. Muss eine Move-Aktion unterstützt werden, verwenden Sie die **INotifyCollectionChanged** -Schnittstelle.  Die .NET-Klasse „ObservableCollection\<T>“ verwendet „ **INotifyCollectionChanged** “.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - Wird verwendet, wenn jedem Element ein eindeutiger Bezeichner (ID) zugeordnet werden kann.  Empfohlen, wenn „Reset“ als Aktion für Sammlungsänderungen verwendet wird.

    Ermöglicht es dem Steuerelement, sehr effizient die vorhandene Benutzeroberfläche wiederherstellen zu können, nachdem eine harte Aktion für Zurücksetzen (Reset-Aktion) im Rahmen eines **INotifyCollectionChanged** - oder **IObservableVector** -Ereignisses empfangen wurde. Nachdem das Steuerelement eine „Reset“-Aktion empfangen hat, verwendet es die bereitgestellte eindeutige ID, um die aktuellen Daten mit Elementen zu verknüpfen, die es bereits erstellt hat. Ohne die Zuordnung von Schlüssel zu Index müsste das Steuerelement davon ausgehen, dass es die Benutzeroberfläche für die Daten von Grund auf neu erstellen muss.

Die oben aufgeführten Schnittstellen zeigen, mit Ausnahme von „IKeyIndexMapping“, in „ItemsRepeater“ das gleiche Verhalten wie in „ListView“ und „GridView“.


Die folgenden Schnittstellen für eine Elementequelle (ItemsSource) ermöglichen spezielle Funktionalität in „ListView“- und „GridView“-Steuerelementen, wirken sich derzeit aber nicht auf ein „ItemsRepeater“-Steuerelement aus:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> Wir legen Wert auf Ihre Meinung! Teilen Sie uns Ihre Meinung zu dem [GitHub-Projekt der Windows-UI-Bibliothek](https://github.com/Microsoft/microsoft-ui-xaml/issues) mit. Erwägen Sie, Ihre Gedanken zu vorhandenen Vorschlägen wie [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374) hinzuzufügen: Add incremental loading support for ItemsRepeater (Unterstützung für inkrementelles Laden zu ItemsRepeater hinzufügen).

Ein alternativer Ansatz zum inkrementellen Laden von Daten, während der Benutzer nach oben oder unten scrollt, besteht darin, die Position des Viewports von „ScrollViewer“ zu beobachten und mehr Daten zu laden, wenn sich der Viewport der Erstreckung nähert.

```xaml
<ScrollViewer ViewChanged="ScrollViewer_ViewChanged">
    <ItemsRepeater ItemsSource="{x:Bind MyItemsSource}" .../>
</ScrollViewer>
```

```csharp
private async void ScrollViewer_ViewChanged(object sender, ScrollViewerViewChangedEventArgs e)
{
    if (!e.IsIntermediate)
    {
        var scroller = (ScrollViewer)sender;
        var distanceToEnd = scroller.ExtentHeight - (scroller.VerticalOffset + scroller.ViewportHeight);

        // trigger if within 2 viewports of the end
        if (distanceToEnd <= 2.0 * scroller.ViewportHeight
                && MyItemsSource.HasMore && !itemsSource.Busy)
        {
            // show an indeterminate progress UI
            myLoadingIndicator.Visibility = Visibility.Visible;

            await MyItemsSource.LoadMoreItemsAsync(/*DataFetchSize*/);

            loadingIndicator.Visibility = Visibility.Collapsed;
        }
    }
}
```

## <a name="change-the-layout-of-items"></a>Ändern des Layouts von Elementen

Elemente, die von [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) angezeigt werden, werden von einem [Layout](/uwp/api/microsoft.ui.xaml.controls.layout)-Objekt angeordnet, das die Größen und Positionierungen seiner untergeordneten Elemente verwaltet. Wird ein „Layout“-Objekt mit einem „ItemsRepeater“-Steuerelement verwendet, ermöglicht es UI-Virtualisierung. Die bereitgestellten Layouts sind [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) und [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout). Standardmäßig verwendet „ItemsRepeater“ ein „StackLayout“ mit vertikaler Ausrichtung.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) ordnet Elemente in einer einzelnen Zeile an, die Sie horizontal oder vertikal ausrichten können.

Sie können die [Spacing](/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing)-Eigenschaft festlegen, um den Abstand zwischen Elementen anzupassen. „Spacing“ wird in der Richtung angewendet, die durch die Ausrichtung ([Orientation](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation)) des Layouts angegeben ist.

![Abstand für StackLayout](images/stack-layout.png)

In diesem Beispiel wird gezeigt, wie die „ItemsRepeater.Layout“-Eigenschaft auf ein Stapellayout (StackLayout) mit horizontaler Ausrichtung und einem jeweiligen Abstand von 8 Pixeln festgelegt wird.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

[UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) positioniert Elemente der Reihe nach in einem Umbruchlayout. Elemente werden in Reihenfolge von links nach rechts angeordnet, wenn die [Orientation](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation)-Eigenschaft auf **Horizontal** , und von oben nach unten, wenn „Orientation“ auf **Vertical** festgelegt ist. Jedes Element hat dieselbe Größe.

![Einheitliches Rasterlayout](images/uniform-grid-layout.png)

Die Anzahl der Elemente in jeder Zeile eines horizontalen Layouts wird durch die Mindestbreite eines Elements beeinflusst. Die Anzahl der Elemente in jeder Spalte eines vertikalen Layouts wird durch die Mindesthöhe eines Elements beeinflusst.

- Sie können explizit eine zu verwendende Mindestgröße bereitstellen, indem Sie die Eigenschaften [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) und [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth) festlegen.
- Wenn Sie keine Mindestgröße angeben, wird die gemessene Größe des ersten Elements als die Mindestgröße pro Element angenommen.

Sie können für das Layout auch einen Mindestabstand, der zwischen Zeilen und Spalten eingefügt werden soll, angeben, indem Sie die Eigenschaften [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) und [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing) festlegen.

![Einheitliche Größe und einheitlicher Abstand für Raster](images/uniform-grid-sizing-spacing.png)

Nachdem die Anzahl der Elemente in einer Zeile oder Spalte anhand von Mindestgröße und Mindestabstand des Elements bestimmt wurde, kann nach dem letzten Element in der Zeile oder Spalte ungenutzter Platz vorhanden sein (wie in der vorherigen Abbildung dargestellt). Sie können angeben, ob zusätzlicher Platz ignoriert wird, verwendet wird, um die Größe jedes Elements zu erhöhen, oder verwendet wird, um zusätzlichen Abstand zwischen den Elementen zu erstellen. Dies wird mit den Eigenschaften [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) und [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) gesteuert.

Sie können die [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch)-Eigenschaft festlegen, um anzugeben, wie die Elementgröße erhöht werden soll, um den ungenutzten Platz zu füllen.

In dieser Liste sind die verfügbaren Werte aufgeführt. Für die Definitionen wird für **Orientation** der Wert **Horizontal** als Standardwert angenommen.

- **None** : Zusätzlicher Platz am Ende der Zeile wird nicht verwendet. Dies ist der Standardwert.
- **Fill** : Den Elementen wird zusätzliche Breite zugewiesen, um den verfügbaren Platz vollständig zu nutzen (Höhe bei vertikalem Layout).
- **Uniform** : Den Elementen wird zusätzliche Breite zugewiesen, um den verfügbaren Platz vollständig zu nutzen, sowie zusätzliche Höhe, um das Seitenverhältnis beizubehalten (Höhe und Breite werden bei vertikaler Anordnung getauscht).

Diese Abbildung zeigen die Auswirkungen der **ItemsStretch** -Werte in einem horizontalen Layout.

![Einheitliches Dehnen von Rasterelementen](images/uniform-grid-item-stretch.png)

Ist **ItemsStretch** auf **None** festgelegt, können Sie durch Festlegen der [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification)-Eigenschaft angeben, wie zusätzlicher Platz dazu verwendet werden soll, die Elemente auszurichten.

In dieser Liste sind die verfügbaren Werte aufgeführt. Für die Definitionen wird für **Orientation** der Wert **Horizontal** als Standardwert angenommen.

- **Start** : Elemente werden am Anfang der Zeile ausgerichtet. Zusätzlicher Platz am Ende der Zeile wird nicht verwendet. Dies ist der Standardwert.
- **Center** : Elemente werden in der Mitte der Zeile ausgerichtet. Zusätzlicher Platz wird gleichmäßig auf den Anfang und das Ende der Zeile verteilt.
- **End** : Elemente werden am Ende der Zeile ausgerichtet. Zusätzlicher Platz am Anfang der Zeile wird nicht verwendet.
- **SpaceAround** : Elemente werden gleichmäßig verteilt. Vor und nach jedem Element wird gleich großer Platz hinzugefügt.
- **SpaceBetween** : Elemente werden gleichmäßig verteilt. Zwischen je zwei Elementen wird gleich großer Platz hinzugefügt. Weder am Anfang noch am Ende der Zeile wird Platz hinzugefügt.
- **SpaceEvenly** : Elemente werden gleichmäßig verteilt, wobei gleich viel Platz sowohl zwischen je zwei Elementen als auch am Anfang und am Ende der Zeile hinzugefügt wird.

Diese Abbildung zeigt die Auswirkungen der **ItemsStretch** -Werte in einem vertikalen Layout (angewendet auf Spalten statt auf Zeilen).

![Einheitliche Ausrichtung von Rasterlementen](images/uniform-grid-item-justification.png)

> [!TIP]
> Die **ItemsStretch** -Eigenschaft wirkt sich auf den _Bemessen_ -Schritt eines Layouts aus. Die **ItemsJustification** -Eigenschaft wirkt sich auf den _Anordnen_ -Schritt eines Layouts aus.

In diesem Beispiel wird veranschaulicht, wie die **ItemsRepeater.Layout** -Eigenschaft, auf ein einheitliches Rasterlayout ( **UniformGridLayout** ) festgelegt wird.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                    ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:UniformGridLayout MinItemWidth="200"
                                MinColumnSpacing="28"
                                ItemsJustification="SpaceAround"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

## <a name="lifecycle-events"></a>Lebenszyklusereignisse

Wenn Sie Elemente in einem [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)-Steuerelement hosten, müssen Sie möglicherweise, wenn ein Element angezeigt oder dessen Anzeige beendet wird, eine Aktion auslösen, etwa Starten eines asynchronen Downloads von bestimmten Inhalten, Verknüpfen des Elements mit einem Mechanismus zum Nachverfolgen der Auswahl oder Beenden einer Hintergrundaufgabe.

In einem virtualisierenden Steuerelement können Sie sich nicht auf Geladen/Entladen-Ereignisse (Loaded/Unloaded) verlassen, weil das Element möglicherweise nicht aus dem visuellen Livebaum entfernt wird, wenn es erneut verwendet wird. Stattdessen werden andere Ereignisse bereitgestellt, um den Lebenszyklus von Elementen zu verwalten. In dieser schematischen Darstellung ist der Lebenszyklus eines Elements in einem „ItemsRepeater“-Steuerelement gezeigt und wird veranschaulicht, wann die zugehörigen Ereignisse ausgelöst werden.

![Schematische Darstellung von Lebenszyklusereignissen](images/items-repeater-lifecycle.png)

- [**ElementPrepared**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) tritt jedes Mal auf, wenn ein Element für die Verwendung vorbereitet wird. Dieses Ereignis tritt sowohl für ein neu erstelltes Element als auch für ein Element auf, das bereits vorhanden ist und aus der Wiederverwendungswarteschlange erneut verwendet wird.
- [**ElementClearing**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) tritt jedes Mal unmittelbar auf, nachdem ein Element an die Wiederverwendungswarteschlange gesendet wurde, beispielsweise wenn es aus dem Bereich der realisierten Elemente herausgefallen ist.
- [**ElementIndexChanged**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) tritt für jedes realisierte Benutzeroberflächenelement (UIElement) auf, bei dem sich der Index des Elements, dem es entspricht, geändert hat. Wenn beispielsweise ein anderes Element in der Datenquelle hinzugefügt oder entfernt wird, empfängt der Index für Elemente, die in der Reihenfolge später kommen, dieses Ereignis.

In diesem Beispiel wird gezeigt, wie Sie diese Ereignisse dazu verwenden könnten, einen Auswahldienst anzufügen, um die Elementauswahl in einem benutzerdefinierten Steuerelement nachzuverfolgen, in dem „ItemsRepeater“ zum Anzeigen von Elementen verwendet wird.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<UserControl ...>
    ...
    <ScrollViewer>
        <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                            ItemTemplate="{StaticResource MyTemplate}"
                            ElementPrepared="OnElementPrepared"
                            ElementIndexChanged="OnElementIndexChanged"
                            ElementClearing="OnElementClearing">
        </muxc:ItemsRepeater>
    </ScrollViewer>
    ...
</UserControl>
```

```csharp
interface ISelectable
{
    int SelectionIndex { get; set; }
    void UnregisterSelectionModel(SelectionModel selectionModel);
    void RegisterSelectionModel(SelectionModel selectionModel);
}

private void OnElementPrepared(ItemsRepeater sender, ElementPreparedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Wire up this item to recognize a 'select' and listen for programmatic
        // changes to the selection model to know when to update its visual state.
        selectable.SelectionIndex = args.Index;
        selectable.RegisterSelectionModel(this.SelectionModel);
    }
}

private void OnElementIndexChanged(ItemsRepeater sender, ElementIndexChangedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Sync the ID we use to notify the selection model when the item
        // we represent has changed location in the data source.
        selectable.SelectionIndex = args.NewIndex;
    }
}

private void OnElementClearing(ItemsRepeater sender, ElementClearingEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Disconnect handlers to recognize a 'select' and stop
        // listening for programmatic changes to the selection model.
        selectable.UnregisterSelectionModel(this.SelectionModel);
        selectable.SelectionIndex = -1;
    }
}
```

## <a name="sorting-filtering-and-resetting-the-data"></a>Sortieren, Filtern und Zurücksetzen der Daten

Wenn Sie eine Aktion wie Filtern oder Sortieren eines Datasets ausführen, besteht Ihre herkömmliche Vorgehensweise möglicherweise darin, den früheren Satz von Daten mit den neuen Daten zu vergleichen und dann granulare Änderungsbenachrichtigungen über [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged) auszugeben. Häufig ist es jedoch einfacher, die alten Daten vollständig durch die neuen Daten zu ersetzen und mit der [Reset](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction)-Aktion eine Änderungsbenachrichtigung für die Sammlung auszulösen.

In der Regel veranlasst ein Zurücksetzen ein Steuerelement dazu, vorhandene untergeordnete Elemente freizugeben und neu zu beginnen, wobei die Benutzeroberfläche ab dem Anfang an Scrollposition 0 erstellt wird, weil das Steuerelement nicht genau weiß, wie sich die Daten während des Zurücksetzens geändert haben.

Unterstützt die als Elementequelle (ItemsSource) zugewiesene Sammlung jedoch eindeutige Bezeichner, indem die [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)-Schnittstelle implementiert ist, kann „ItemsRepeater“ schnell Folgendes ermitteln:

- Wiederverwendbare „UIElement“-Objekte für Daten, die vor und nach dem Zurücksetzen vorhanden waren
- Zuvor sichtbare Elemente, die entfernt wurden
- Hinzugefügte neue Elemente, die sichtbar sind

Dadurch wird verhindert, dass „ItemsRepeater“ erneut bei der Scrollposition 0 beginnt. Außerdem wird „ItemsRepeater“ hiermit in die Lage versetzt, die „UIElement“-Objekte für Daten schnell wiederherzustellen, die beim Zurücksetzen nicht geändert wurden, wodurch eine bessere Leistung erzielt wird.

In diesem Beispiel wird gezeigt, wie eine Liste von Elementen in einem vertikalen Stapel angezeigt wird, wobei _MyItemsSource_ eine benutzerdefinierte Datenquelle ist, die eine zugrunde liegende Liste von Elementen umschließt. Sie macht eine _Data_ -Eigenschaft verfügbar, die dazu verwendet werden kann, eine neue Quelle zuzuweisen, die als Elementequelle zu verwenden ist, wodurch dann ein Zurücksetzen ausgelöst wird.

```xaml
<ScrollViewer x:Name="sv">
    <ItemsRepeater x:Name="repeater"
                ItemsSource="{x:Bind MyItemsSource}"
                ItemTemplate="{StaticResource MyTemplate}">
       <ItemsRepeater.Layout>
           <StackLayout ItemSpacing="8"/>
       </ItemsRepeater.Layout>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Similar to an ItemsControl, a developer sets the ItemsRepeater's ItemsSource.
    // Here we provide our custom source that supports unique IDs which enables
    // ItemsRepeater to be smart about handling resets from the data.
    // Unique IDs also make it easy to do things apply sorting/filtering
    // without impacting any state (i.e. selection).
    MyItemsSource myItemsSource = new MyItemsSource(data);

    repeater.ItemsSource = myItemsSource;

    // ...

    // We can sort/filter the data using whatever mechanism makes the
    // most sense (LINQ, database query, etc.) and then reassign
    // it, which in our implementation triggers a reset.
    myItemsSource.Data = someNewData;
}

// ...


public class MyItemsSource : IReadOnlyList<ItemBase>, IKeyIndexMapping, INotifyCollectionChanged
{
    private IList<ItemBase> _data;

    public MyItemsSource(IEnumerable<ItemBase> data)
    {
        if (data == null) throw new ArgumentNullException();

        this._data = data.ToList();
    }

    public IList<ItemBase> Data
    {
        get { return _data; }
        set
        {
            _data = value;

            // Instead of tossing out existing elements and re-creating them,
            // ItemsRepeater will reuse the existing elements and match them up
            // with the data again.
            this.CollectionChanged?.Invoke(
                this,
                new NotifyCollectionChangedEventArgs(NotifyCollectionChangedAction.Reset));
        }
    }

    #region IReadOnlyList<T>

    public ItemBase this[int index] => this.Data != null
        ? this.Data[index]
        : throw new IndexOutOfRangeException();

    public int Count => this.Data != null ? this.Data.Count : 0;
    public IEnumerator<ItemBase> GetEnumerator() => this.Data.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => this.GetEnumerator();

    #endregion

    #region INotifyCollectionChanged

    public event NotifyCollectionChangedEventHandler CollectionChanged;

    #endregion

    #region IKeyIndexMapping

    private int lastRequestedIndex = IndexNotFound;
    private const int IndexNotFound = -1;

    // When UniqueIDs are supported, the ItemsRepeater caches the unique ID for each item
    // with the matching UIElement that represents the item.  When a reset occurs the
    // ItemsRepeater pairs up the already generated UIElements with items in the data
    // source.
    // ItemsRepeater uses IndexForUniqueId after a reset to probe the data and identify
    // the new index of an item to use as the anchor.  If that item no
    // longer exists in the data source it may try using another cached unique ID until
    // either a match is found or it determines that all the previously visible items
    // no longer exist.
    public int IndexForUniqueId(string uniqueId)
    {
        // We'll try to increase our odds of finding a match sooner by starting from the
        // position that we know was last requested and search forward.
        var start = lastRequestedIndex;
        for (int i = start; i < this.Count; i++)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        // Then try searching backward.
        start = Math.Min(this.Count - 1, lastRequestedIndex);
        for (int i = start; i >= 0; i--)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        return IndexNotFound;
    }

    public string UniqueIdForIndex(int index)
    {
        var key = this[index].PrimaryKey;
        lastRequestedIndex = index;
        return key;
    }

    #endregion
}

```

## <a name="create-a-custom-collection-control"></a>Erstellen eines benutzerdefinierten Sammlungssteuerelements

Sie können [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) verwenden, um ein benutzerdefiniertes Sammlungssteuerelement vollständig mit seinem eigenen Steuerungstyp zu erstellen, um jedes Element darzustellen.

> [!NOTE]
> Dies ist vergleichbar mit dem Verwenden von **ItemsControl** , aber statt von **ItemsControl** abzuleiten und eine **ItemsPresenter** -Instanz in die Steuerelementvorlage zu setzen, nehmen Sie das Ableiten von **Control** vor, und fügen Sie ein **ItemsRepeater** -Steuerelement in die Steuerelementvorlage ein. Das benutzerdefinierte Sammlungssteuerelement „hat ein“ **ItemsRepeater** -Steuerelement im Vergleich zu „ist ein“ **ItemsControl** -Steuerelement. Dies impliziert, Sie müssen auch explizit auswählen, welche Eigenschaften verfügbar zu machen sind, anstatt festzulegen, welche geerbten Eigenschaften nicht unterstützt werden sollen.

In diesem Beispiel wird gezeigt, wie ein [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)-Steuerelement in der Vorlage eines benutzerdefinierten Steuerelements namens _MediaCollectionView_ angeordnet wird und wie dessen Eigenschaften verfügbar gemacht werden.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Style TargetType="local:MediaCollectionView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="local:MediaCollectionView">
                <Border
                    Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer">
                        <muxc:ItemsRepeater x:Name="ItemsRepeater"
                                            ItemsSource="{TemplateBinding ItemsSource}"
                                            ItemTemplate="{TemplateBinding ItemTemplate}"
                                            Layout="{TemplateBinding Layout}"
                                            TabFocusNavigation="{TemplateBinding TabFocusNavigation}"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

```csharp
public sealed class MediaCollectionView : Control
{
    public object ItemsSource
    {
        get { return (object)GetValue(ItemsSourceProperty); }
        set { SetValue(ItemsSourceProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemsSource.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemsSourceProperty =
        DependencyProperty.Register(nameof(ItemsSource), typeof(object), typeof(MediaCollectionView), new PropertyMetadata(0));

    public DataTemplate ItemTemplate
    {
        get { return (DataTemplate)GetValue(ItemTemplateProperty); }
        set { SetValue(ItemTemplateProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemTemplate.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemTemplateProperty =
        DependencyProperty.Register(nameof(ItemTemplate), typeof(DataTemplate), typeof(MediaCollectionView), new PropertyMetadata(0));

    public Layout Layout
    {
        get { return (Layout)GetValue(LayoutProperty); }
        set { SetValue(LayoutProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Layout.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty LayoutProperty =
        DependencyProperty.Register(nameof(Layout), typeof(Layout), typeof(MediaCollectionView), new PropertyMetadata(0));

    public MediaCollectionView()
    {
        this.DefaultStyleKey = typeof(MediaCollectionView);
    }
}
```

## <a name="display-grouped-items"></a>Anzeigen von gruppierten Elementen

Sie können ein [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)-Steuerelement in der [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)-Eigenschaft eines anderen ItemsRepeater-Steuerelements schachteln, um geschachtelte Virtualisierungslayouts zu erstellen. Das Framework nutzt Ressourcen effizient, indem es unnötige Verwendung (Realisierung) von Elementen minimiert, die nicht sichtbar sind oder sich in der Nähe des aktuellen Viewports befinden.

In diesem Beispiel wird veranschaulicht, wie Sie eine Liste von gruppierten Elementen in einem vertikalen Stapel anzeigen können. Das äußere „ItemsRepeater“-Steuerelement generiert jede Gruppe. In der Vorlage für jede Gruppe generiert ein anderes „ItemsRepeater“-Steuerelement die Elemente .

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->

<Page.Resources>
    <muxc:StackLayout x:Key="MyGroupLayout"/>
    <muxc:StackLayout x:Key="MyItemLayout" Orientation="Horizontal"/>
</Page.Resources>

<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          <TextBlock Text="{x:Bind AppTitle}"/>
          <!-- Items -->
          <muxc:ItemsRepeater ItemsSource="{x:Bind Notifications}"
                              Layout="{StaticResource MyItemLayout}"
                              ItemTemplate="{StaticResource MyTemplate}"/>
          <!-- Footer -->
          <Button Content="{x:Bind FooterText}"/>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```
Die folgende Abbildung zeigt das grundlegende Layout, das auf Grundlage des obigen Beispiels erstellt wurde.

![Geschachteltes Layouts mit „ItemsRepeater“-Steuerelement](images/items-repeater-nested-layout.png)

Das nächste Beispiel zeigt ein Layout für eine App, in der es verschiedene Kategorien gibt, die mit der Benutzereinstellung geändert werden können und als horizontal scrollende Listen angezeigt werden. Das Layout dieses Beispiels wird auch in der obigen Abbildung dargestellt.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
          <ScrollViewer HorizontalScrollMode="Enabled"
                                          VerticalScrollMode="Disabled"
                                          HorizontalScrollBarVisibility="Auto" >
            <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                                Background="Orange">
              <muxc:ItemsRepeater.ItemTemplate>
                <DataTemplate x:DataType="local:CategoryItem">
                  <Grid Margin="10"
                        Height="60" Width="120"
                        Background="LightBlue">
                    <TextBlock Text="{x:Bind Name}"
                               Style="{StaticResource SubtitleTextBlockStyle}"
                               Margin="4"/>
                  </Grid>
                </DataTemplate>
              </muxc:ItemsRepeater.ItemTemplate>
              <muxc:ItemsRepeater.Layout>
                <muxc:StackLayout Orientation="Horizontal"/>
              </muxc:ItemsRepeater.Layout>
            </muxc:ItemsRepeater>
          </ScrollViewer>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

## <a name="bringing-an-element-into-view"></a>Bringen eines Elements in die Ansicht

Das XAML-Framework verwaltet bereits das Bringen eines Frameworkelements (FrameworkElement) in die Ansicht, wenn es entweder den 1) Tastaturfokus erhält oder den 2) Sprachausgabefokus erhält. Es kann weitere Fälle geben, in denen Sie ein Element explizit in die Ansicht bringen müssen. So beispielsweise als Antwort auf eine Benutzeraktion oder zum Wiederherstellen des Zustands der Benutzeroberfläche nach einer Seitennavigation.

Für ein Bringen eines virtualisierten Elements in die Ansicht sind folgende Schritt erforderlich:
1. Realisieren eines „UIElement“-Objekts für ein Element
2. Ausführen des Layouts, um sicherzustellen, dass das Element eine gültige Position hat
3. Initiieren einer Anforderung, um das realisierte Element in die Anzeige zu bringen

Im folgenden Beispiel werden diese Schritte als Bestandteil der Wiederherstellung der Scrollposition eines Elements in einer flachen vertikalen Liste nach einer Seitennavigation veranschaulicht. Bei hierarchischen Daten mit geschachtelten „ItemsRepeater“-Steuerelementen ist die Vorgehensweise im Wesentlichen identisch, muss aber auf jeder Ebene der Hierarchie ausgeführt werden.

```xaml
<ScrollViewer x:Name="scrollviewer">
  <ItemsRepeater x:Name="repeater" .../>
</ScrollViewer>
```

```csharp
public class MyPage : Page
{
    // ...

     protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

        // retrieve saved offset + index(es) of the tracked element and then bring it into view.
        // ... 
        
        var element = repeater.GetOrCreateElement(index);

        // ensure the item is given a valid position
        element.UpdateLayout();

        element.StartBringIntoView(new BringIntoViewOptions()
        {
            VerticalOffset = relativeVerticalOffset
        });
    }

    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        base.OnNavigatingFrom(e);

        // retrieve and save the relative offset and index(es) of the scrollviewer's current anchor element ...
        var anchor = this.scrollviewer.CurrentAnchor;
        var index = this.repeater.GetElementIndex(anchor);
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualSize.X, anchor.ActualSize.Y));
        relativeVerticalOffset = this.scrollviewer.VerticalOffset - anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>Aktivieren von Barrierefreiheit

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) stellt keine Standardbenutzeroberfläche für Barrierefreiheit bereit. Die Dokumentation zu [Benutzerfreundlichkeit in Windows-Apps](../usability/index.md) bietet eine Fülle von Informationen dazu, wie Sie sicherstellen können, dass Ihre App eine inklusive Benutzeroberfläche bereitstellt. Wenn Sie die „ItemsRepeater“-Klasse verwenden, um ein benutzerdefiniertes Steuerelement zu erstellen, sollten Sie die Dokumentation zu [Benutzerdefinierte Automatisierungspeers](../accessibility/custom-automation-peers.md) lesen.

### <a name="keyboarding"></a>Tastaturunterstützung
Die Mindesttastaturunterstützung für Fokusbewegung, die „ItemsRepeater“ bietet, basiert auf der zu XAML gehörenden [direktionalen 2D-Navigation für die Tastatur](../input/focus-navigation.md#2d-directional-navigation-for-keyboard).

![Richtungsnavigation (direktionale Navigation)](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

Der [XYFocusKeyboardNavigation-Modus](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) von „ItemsRepeater“ ist standardmäßig aktiviert ( _Enabled_ ). Je nach gewünschter Oberfläche bietet es sich an, Unterstützung für allgemeine [Tastaturinteraktionen](../input/keyboard-interactions.md), etwa „Pos1“ (Home), „Ende“ (End), „Bild auf“ (PageUp) und „Bild ab“ (PageDown), hinzuzufügen.

„ItemsRepeater“ stellt nicht automatisch sicher, dass die Standardaktivierreihenfolge für die enthaltenen Elemente (ob virtualisiert oder nicht) mit der Reihenfolge identisch ist, in der die Elemente in den Daten vorliegen. Standardmäßig ist die [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation)-Eigenschaft von „ItemsRepeater“ auf [Once](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) anstelle der üblichen Standardeinstellung von _Local_ festgelegt.

> [!NOTE]
> „ItemsRepeater“ merkt sich nicht automatisch das letzte fokussierte Element.  Dies bedeutet, dass ein Benutzer, wenn er UMSCHALT+TAB verwendet, möglicherweise zum letzten realisierten Element gelangt.

### <a name="announcing-item-_x_-of-_y_-in-screen-readers"></a>Ankündigen von „Element _X_ von _Y_ “ in Sprachausgaben

Sie müssen das Festlegen der entsprechenden Automatisierungseigenschaften verwalten, z. B. die Werte für **PositionInSet** und **SizeOfSet** , und sicherstellen, dass sie auf dem neuesten Stand bleiben, wenn Elemente hinzugefügt, verschoben, entfernt usw. werden.

In einigen benutzerdefinierten Layouts gibt es möglicherweise keine offensichtliche Abfolge für die visuelle Reihenfolge.  Benutzer erwarten zumindest, dass die Werte für die Eigenschaften „PositionInSet“ und „SizeOfSet“, die von Sprachausgaben verwendet werden, der Reihenfolge entsprechen, in der die Elemente in den Daten stehen (versetzt um 1, um der natürlichen Zählweise gegenüber 0-Basierung zu entsprechen).

Dies lässt sich am besten erreichen, indem im Automatisierungspeer für das Elementsteuerelement die Methoden [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) und [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) implementiert werden und die Position des Elements im Dataset, das durch das Steuerelement dargestellt wird, mitgeteilt wird. Der Wert wird nur zur Laufzeit berechnet, wenn über eine Hilfstechnologie auf ihn zugegriffen wird. Somit ist es kein Problem, ihn auf dem neuesten Stand zu halten. Der Wert entspricht der Datenreihenfolge.

In diesem Beispiel wird gezeigt, wie Sie dies umsetzen können, wenn Sie ein benutzerdefiniertes Steuerelement namens _CardControl_ präsentieren.

```xaml
<ScrollViewer >
    <ItemsRepeater x:Name="repeater" ItemsSource="{x:Bind MyItemsSource}">
       <ItemsRepeater.ItemTemplate>
           <DataTemplate x:DataType="local:CardViewModel">
               <local:CardControl Item="{x:Bind}"/>
           </DataTemplate>
       </ItemsRepeater.ItemTemplate>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
internal sealed class CardControl : CardControlBase
{
    protected override AutomationPeer OnCreateAutomationPeer() => new CardControlAutomationPeer(this);

    private sealed class CardControlAutomationPeer : FrameworkElementAutomationPeer
    {
        private readonly CardControl owner;

        public CardControlAutomationPeer(CardControl owner) : base(owner) => this.owner = owner;

        protected override int GetPositionInSetCore()
          => ((ItemsRepeater)owner.Parent)?.GetElementIndex(this.owner) + 1 ?? base.GetPositionInSetCore();

        protected override int GetSizeOfSetCore()
          => ((ItemsRepeater)owner.Parent)?.ItemsSourceView?.Count ?? base.GetSizeOfSetCore();
    }
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [Listen](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
