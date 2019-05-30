---
Description: ItemsRepeater ist ein Lightweight-Steuerelement zum Generieren und eine Auflistung von Elementen vorhanden.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 93a81501b524826484111419899675fbb99b86fa
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364757"
---
# <a name="itemsrepeater"></a>ItemsRepeater

Verwenden einer [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) zum Erstellen von benutzerdefinierten Sammlung Benutzererlebnis mit ein flexibles Layout-System, benutzerdefinierte Ansichten und Virtualisierung.

Im Gegensatz zu [ListView](/uwp/api/windows.ui.xaml.controls.listview), [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) bietet eine umfassende Benutzeroberfläche – keine er besitzt keinen Standard-Benutzeroberfläche und bietet keine Richtlinie aus, um den Fokus, Auswahl, oder Interaktion des Benutzers. Stattdessen ist es ein Baustein, den Sie verwenden können, um eigene eindeutige Auflistung basierende Funktionen und benutzerdefinierte Steuerelemente zu erstellen. Wenn Sie keine integrierte Richtlinie hat, können Sie zum Anfügen der Richtlinie, um die Benutzeroberfläche zu erstellen, die Sie benötigen. Beispielsweise können Sie das Layout zu verwenden, die keyboarding Richtlinie, die Richtlinie zur Serverauswahl usw. definieren.

Sie können sich vorstellen [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) konzeptionell als einen Bereich für datengesteuerte und nicht als vollständige wie ListView-Steuerelement. Sie geben eine Auflistung von Datenelementen, die angezeigt werden, eine Elementvorlage, die ein Element der Benutzeroberfläche für jedes Datenelement generiert und ein Layout, das bestimmt, wie die Elemente angepasst und positioniert werden. Klicken Sie dann ItemsRepeater untergeordneter Elemente anhand der Datenquelle erstellt, und zeigt sie laut die Item-Vorlage und das Layout. Die angezeigten Elemente müssen nicht homogen sein, da es sich bei Inhalt zur Darstellung der Datenelemente, die basierend auf Kriterien, die Sie in eine Datenvorlagenauswahl angeben ItemsRepeater geladen werden kann.

| **Abrufen der Windows-UI-Bibliothek** |
| - |
| Dieses Steuerelement ist Bestandteil der Windows-UI-Bibliothek, eines NuGet-Pakets, die neuen Steuerelemente und Funktionen der Benutzeroberfläche für UWP-apps enthält. Weitere Informationen einschließlich installationsanweisungen finden Sie in der [Übersicht über die Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **Wichtige APIs:** [ItemsRepeater Klasse](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), [ScrollViewer-Klasse](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden einer [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) benutzerdefinierte zeigt für Auflistungen von Daten zu erstellen. Während es verwendet werden kann, die einen grundlegenden Satz von Elementen vorhanden, können Sie häufig als das Anzeigeelement in der Vorlage ein benutzerdefiniertes Steuerelement verwenden.

Wenn Sie ein Out-of-the-Box-Steuerelement zum Anzeigen von Daten in einer Liste oder Raster mit minimalem Anpassungsaufwand benötigen, sollten Sie verwenden eine [ListView](/uwp/api/windows.ui.xaml.controls.listview) oder [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

ItemsRepeater muss sich nicht auf eine integrierte Elementsammlung aus. Wenn Sie eine Items-Auflistung direkt, anstatt zu einer anderen Datenquelle, Bindung bereitstellen müssen Sie sind eine weitere High-Policy-Erfahrung schädlicheren und sollten [ListView](/uwp/api/windows.ui.xaml.controls.listview) oder [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) und ItemsRepeater beide ermöglichen Sie die Sammlung, aber ItemsRepeater unterstützt virtualisierenden UI-Layouts, während das ItemsControl-Element nicht der Fall ist. Es wird empfohlen, anstelle von ItemsRepeater ItemsControl-Objekt gibt an, ob die nur für einige Elemente aus Daten darstellen oder Erstellen eines benutzerdefinierten Sammlung-Steuerelements.

> [!NOTE]
> Wenn Sie eine Situation haben, in dem Sie der Meinung sind ItemsControl Ihren Anforderungen entspricht und keine ItemsRepeater, bitte Ihr Feedback auf die [Windows UI-Bibliothek-GitHub-Projekt](https://github.com/Microsoft/microsoft-ui-xaml/issues) uns wissen zu lassen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um die app öffnen und finden Sie unter den <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> in Aktion.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>Durchführen eines Bildlaufs mit ItemsRepeater

[**ItemsRepeater** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) nicht von abgeleitet [ **Steuerelement**](/uwp/api/windows.ui.xaml.controls.control), sodass es eine Steuerelementvorlage enthält. Aus diesem Grund Es enthält keine integrierten, wie ein ListView einen Bildlauf aus, oder führen Sie andere Steuerelemente für die Attributsammlung.

Bei Verwendung einer **ItemsRepeater**, sollten Sie Bildlauffunktionen bereitstellen, durch das wrapping in eine [ **ScrollViewer** ](/uwp/api/windows.ui.xaml.controls.scrollviewer) Steuerelement.

> [!NOTE]
> Wenn Ihre app, in früheren Versionen von Windows ausgeführt wird - veröffentlicht die *vor* Windows 10, Version 1809 - auch müssen Sie zum Hosten der **ScrollViewer** innerhalb der [  **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost). 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> Wenn die app nur auf neueren Versionen von Windows 10, Version 1809 und höher – ausgeführt, und klicken Sie dann keine Notwendigkeit besteht, verwenden Sie die [ **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost).
>
> Vor Windows 10, Version 1809, **ScrollViewer** wurde nicht implementiert die [ **IScrollAnchorProvider** ](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) Schnittstelle, die die **ItemsRepeater**benötigt.  Die **ItemsRepeaterScrollHost** ermöglicht die **ItemsRepeater** zur Koordinierung mit **ScrollViewer** unter früheren Versionen der sichtbaren Stelle Elemente ordnungsgemäß beibehalten der Benutzer wird angezeigt.  Andernfalls können die Elemente angezeigt, zu verschieben oder plötzlich verschwinden, wenn die Elemente in der Liste geändert werden, oder die app angepasst wird.

## <a name="create-an-itemsrepeater"></a>Erstellen Sie eine ItemsRepeater

Verwenden einer [ **ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), müssen Sie es durch Festlegen der anzuzeigenden Daten erteilen der **ItemsSource** Eigenschaft. Weisen Sie es dann, wie zum Anzeigen von Elementen durch Festlegen der [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) Eigenschaft.

### <a name="itemssource"></a>ItemsSource

Legen Sie zum Auffüllen der Ansicht der [ **ItemsSource** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) Eigenschaft, um eine Auflistung von Datenelementen. Hier ist die **ItemsSource** im Code direkt an eine Instanz einer Sammlung festgelegt ist.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

Sie können auch eine Bindung die **ItemsSource** Eigenschaft zu einer Sammlung in XAML. Weitere Informationen zur Datenbindung finden Sie unter [Übersicht über Datenbindung](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
Um anzugeben, wie ein Datenelement sichtbar gemacht wird, legen die [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) Eigenschaft, um eine [ **DataTemplate** ](/uwp/api/windows.ui.xaml.datatemplate) oder [  **DataTemplateSelector** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector) Sie definiert haben. Die Datenvorlage definiert, wie die Daten sichtbar gemacht werden. Standardmäßig wird das Element in der Ansicht mit einem **TextBlock** verwendet der die Zeichenfolgendarstellung des Objekts.

Allerdings möchten Sie in der Regel eine mehr überzeugende Präsentation von Daten mithilfe einer Vorlage, die definiert, das Layout und die Darstellung von ein oder mehrere Steuerelemente, die Sie zum Anzeigen eines einzelnen Elements verwenden angezeigt. Der Steuerelemente, die Sie in der Vorlage verwendet die Eigenschaften des Datenobjekts gebunden werden können, oder Inline mit statischen Inhalt definiert haben.

#### <a name="datatemplate"></a>DataTemplate-Element
In diesem Beispiel ist das Datenobjekt, das eine einfache Zeichenfolge. Die **DataTemplate** enthält ein Bild auf der linken Seite den Text und die Stile der **TextBlock** zur Anzeige der Zeichenfolge in einer Blaugrün-Farbe.

> [!NOTE]
> Bei Verwendung der [X: Bind-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) in einem **DataTemplate**, müssen Sie den Datentyp angeben (`x:DataType`) auf dem DataTemplate.

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

Hier ist wie die Elemente angezeigt werden würde, wenn Sie mit diesem angezeigt **DataTemplate**.

![Elemente, die mit einer Datenvorlage angezeigt](images/listview-itemstemplate.png)

Die Anzahl der Elemente in verwendet die **DataTemplate** für ein Element einen erheblichen Einfluss auf die Leistung haben kann, wenn die Ansicht eine große Anzahl von Elementen angezeigt. Weitere Informationen und Beispiele zum Verwenden von **DataTemplate**s, um das Erscheinungsbild von Elementen in der Liste zu definieren, finden Sie unter [Element Container und Vorlagen](item-containers-templates.md).

> [!TIP]
> Zur Vereinfachung, wenn Sie die Vorlagen-Inline deklarieren möchten, statt als statische Ressource auf die verwiesen wird, können Sie angeben, die **DataTemplate** oder **DataTemplateSelector** als direkt untergeordnetes Element der **ItemsRepeater**.  Zugewiesen als Wert für die **ItemTemplate** Eigenschaft. Dies ist z. B. gültig:
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> Im Gegensatz zu **ListView** und andere Steuerelemente Auflistung der **ItemsRepeater** umschließen Sie nicht die Elemente aus einer **DataTemplate** zusätzliche Elementcontainers enthält die Standardrichtlinie wie z. B. Ränder, Auffüllung, visuellen Elemente der Auswahl oder ein Zeiger über den visuellen Zustand. Stattdessen **ItemsRepeater** zeigt nur, was in definiert ist die **DataTemplate**. Wenn Sie Elemente wie ein Listenansichtselement haben möchten, Sie können explizit einschließen ein Containers, wie z. B. **ListViewItem**, in der Datenvorlage. **ItemsRepeater** zeigt die **ListViewItem** visuelle Objekte, jedoch nicht automatisch verwenden, die von anderen Funktionen, z. B. Auswahl oder das Kontrollkästchen mit mehreren Optionen angezeigt.
>
> Auf ähnliche Weise ist Datenerfassung eine Auflistung der Steuerelemente, wie **Schaltfläche** (`List<Button>`), können Sie legen eine **ContentPresenter-Element** in Ihre **DataTemplate-Element** zu Anzeigen des Steuerelements an.

#### <a name="datatemplateselector"></a>DataTemplateSelector

Die Elemente, die Sie in der Ansicht anzeigen müssen nicht vom gleichen Typ sein. Sie können angeben, die [ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) Eigenschaft mit einer [ **DataTemplateSelector** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector) auf verschiedenen  **DataTemplate**basierend auf von Ihnen angegebenen Kriterien.

In diesem Beispiel geht davon aus eine **DataTemplateSelector** definiert wurde, die zwischen zwei unterschiedlichen entscheidet **DataTemplate**s, um ein Element für große und kleine darstellen.

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

Beim Definieren einer **DataTemplateSelector** mit **ItemsRepeater** müssen Sie nur implementieren Sie eine Außerkraftsetzung für die [ **SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_) Methode. Weitere Informationen und Beispiele finden Sie unter [ **DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Eine Alternative zum **DataTemplate**s zum Verwalten der Erstellung von Elementen in weiter fortgeschrittenen Szenarien zur Implementierung einer eigenen ist [ **Windows.UI.Xaml.Controls.IElementFactory** ](/uwp/api/windows.ui.xaml.controls.ielementfactory)als verwenden die **ItemTemplate**.  Es wird zum Generieren von Inhalten, die bei der Anforderung verantwortlich sein.

## <a name="configure-the-data-source"></a>Konfigurieren der Datenquelle

Verwenden Sie die [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) -Eigenschaft an die Auflistung, die zum Generieren des Inhalts von Elementen verwenden. Sie können die ItemsSource festlegen, auf einen beliebigen Typ, der implementiert **"IEnumerable"** . Zusätzliche Sammlungs-Schnittstellen implementiert, indem Sie die Datenquelle ermitteln, welche Funktionen für die ItemsRepeater für die Interaktion mit Ihren Daten verfügbar sind.

Diese Liste zeigt die verfügbaren Schnittstellen und wann Sie jeweils in Betracht.

- ["IEnumerable"](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - Kann bei kleinen, statischen Datasets verwendet werden.

    Die Datenquelle muss mindestens die "IEnumerable" implementieren / IIterable-Schnittstelle. Wenn dies alles ist, die unterstützt wird dann durchläuft das Steuerelement alles, was Sie einmal eine Kopie zu erstellen, die sie verwenden können, den Zugriff auf Elemente, die über einen Indexwert.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - Kann für statische, schreibgeschützte Datasets verwendet werden.

    Ermöglicht das Steuerelement, das Zugreifen auf Elemente nach Index, und vermeidet die redundante interne Kopie.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - Kann für statische Datasets verwendet werden.

    Ermöglicht das Steuerelement, das Zugreifen auf Elemente nach Index, und vermeidet die redundante interne Kopie.

    **Warnung**: In der Liste/eines Vektors geändert werden, ohne die Implementierung der [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) wird nicht in der Benutzeroberfläche widergespiegelt werden.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - Empfohlen, die änderungsbenachrichtigungen unterstützen.

    Aktiviert das Steuerelement zu überwachen und reagieren auf Änderungen in der Datenquelle aus, und diese Änderungen in der Benutzeroberfläche.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - Unterstützt die änderungsbenachrichtigung

    Wie die **INotifyCollectionChanged** -Schnittstelle, ermöglicht dies das Steuerelement zum Überwachen und reagieren auf Änderungen in der Datenquelle.

    **Warnung**: Die Windows.Foundation.IObservableVector\<T > unterstützt keine Aktion "Verschieben". Dadurch kann die Benutzeroberfläche für ein Element der visuellen Zustand zu verlieren.  Z. B. ein Element, das derzeit ausgewählt ist, bzw. den Fokus besitzt, in denen erfolgt des Verschiebens von "Entfernen" gefolgt von einem "hinzufügen" den Fokus verliert und nicht mehr ausgewählt werden.

    Die Platform.Collections.Vector\<T > verwendet IObservableVector\<T > und verfügt über dieselbe Einschränkung. Wenn Sie Unterstützung für eine Aktion "Verschieben" erforderlich ist, Sie dann verwenden die **INotifyCollectionChanged** Schnittstelle.  Der .NET ObservableCollection\<T >-Klasse verwendet **INotifyCollectionChanged**.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - Wenn ein eindeutiger Bezeichner jedes Element zugeordnet werden können.  Empfohlen bei Verwendung von "Reset" als Aktion zum Ändern der Auflistung.

    Aktiviert das Steuerelement die vorhandene Benutzeroberfläche sehr effizient wiederherstellen, nach dem Empfang einer schwer 'Zurücksetzen'-Aktion im Rahmen einer **INotifyCollectionChanged** oder **IObservableVector** Ereignis. Nach dem Empfang einer Zurücksetzen wird das Steuerelement die bereitgestellte eindeutige ID verwenden, um die aktuellen Daten mit Elementen zuzuordnen, die sie bereits erstellt haben. Ohne den Schlüssel für die Zuordnung des Index müsste das Steuerelement wird davon ausgegangen, dass es im Erstellen einer Benutzeroberfläche für die Daten von Grund auf beginnen soll.

Die Schnittstellen, die oben aufgeführten, als IKeyIndexMapping, geben das gleiche Verhalten in ItemsRepeater, wie in ListView und GridView.


Die folgenden Schnittstellen auf ItemsSource spezielle Funktionen in der ListView und GridView-Steuerelemente zu aktivieren, aber zurzeit keine Auswirkungen auf eine ItemsRepeater:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> Wir legen Wert auf Ihre Meinung! Teilen Sie uns Ihre Meinung der [Windows UI-Bibliothek-GitHub-Projekt](https://github.com/Microsoft/microsoft-ui-xaml/issues). Erwägen Sie Ihre Meinung über vorhandene Vorschläge wie z. B. [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374): Inkrementelles Laden-Unterstützung für ItemsRepeater hinzugefügt.

Ein alternativer Ansatz, um die Daten inkrementell zu laden, wie der Bildlauf nach oben oder unten ist beobachten Sie die Position des Viewports für das ScrollViewer-Element, und Laden weitere Daten werden während der Viewport der Block erreicht.

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

Elemente angezeigt, indem Sie die [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) angeordnet sind, nach einem [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) Objekt, das die Größe und Positionierung der untergeordneten Elemente verwaltet. Bei Verwendung mit einem ItemsRepeater ermöglicht das Layoutobjekt UI-Virtualisierung. Die bereitgestellten Layouts sind [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) und [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout). Standardmäßig verwendet ItemsRepeater einem StackLayout mit vertikaler Ausrichtung an.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) ordnet Elemente in einer einzelnen Zeile, die Sie horizontal oder vertikal ausrichten können.

Sie können festlegen, die [Abstand](/en-us/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) Eigenschaft, um den Abstand zwischen Elementen anpassen. Abstand wird angewendet, in die Richtung des Layouts des [Ausrichtung](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation).

![Stack-Layoutabstände](images/stack-layout.png)

Dieses Beispiel zeigt, wie Sie die ItemsRepeater.Layout-Eigenschaft auf einem StackLayout mit horizontaler Ausrichtung und Abstände 8-Pixel festgelegt wird.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

Die [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) Elemente nacheinander in einem Layout Wrapping positioniert. Elemente in der Reihenfolge von links nach rechts angeordnet sind bei der [Ausrichtung](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) ist **horizontale**, und oben-nach-unten, wenn die Ausrichtung ist angeordnet **vertikale**. Jedes Element ist gleich groß.

![Symbole mit einheitlicher Layout Rasterweite](images/uniform-grid-layout.png)

Die Anzahl der Elemente in jeder Zeile des einem horizontalen Layout wird durch die Breite des Elements mit minimalen beeinflusst. Die Anzahl der Elemente in jeder Spalte von einem vertikalen Layout wird durch die Höhe des Elements mit minimalen beeinflusst.

- Sie können explizit angeben, eine Mindestgröße für die Verwendung durch Festlegen der [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) und [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth) Eigenschaften.
- Wenn Sie keine Mindestgröße angeben, gilt die gemessene Größe des ersten Elements die minimale Größe pro Element.

Sie können auch festlegen minimalen Abstand für das Layout zwischen Zeilen und Spalten enthalten die [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) und [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing) Eigenschaften.

![Einheitliche Raster größenanpassung und Abstand](images/uniform-grid-sizing-spacing.png)

Nachdem Sie die Anzahl, wenn Elemente in einer Zeile oder Spalte festgelegt wurden auf die minimale Größe des Elements und Abstand basieren, gibt es möglicherweise nicht verwendeter Speicherplatz nach dem letzten Element in der Zeile oder Spalte (wie in der vorherigen Abbildung dargestellt). Sie können angeben, ob zusätzlicher Leerraum wird ignoriert, verwendet, um die Größe der einzelnen Elemente zu erhöhen oder verwendet, um zusätzlichen Abstand zwischen den Elementen zu erstellen. Dies wird gesteuert, indem die [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) und [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) Eigenschaften.

Sie können festlegen, die [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) Eigenschaft, um anzugeben, wie die Elementgröße erhöht wird, um den nicht verwendeten Speicherplatz zu füllen.

Diese Liste zeigt die verfügbaren Werte. Die Definitionen davon aus, der Standardwert **Ausrichtung** von **horizontale**.

- **Keine**: Zusätzliche Leerzeichen am Ende der Zeile nicht verwendete bleibt. Dies ist die Standardeinstellung.
- **Geben Sie**: Die Elemente werden zusätzliche Breite mit den verfügbaren Speicherplatz (Höhe, wenn vertikale) angegeben.
- **Uniform**: Die Elemente sind erhält zusätzliche Breite für die Verwendung der verfügbare Platz und zusätzliche Höhe um das Seitenverhältnis beibehalten (Höhe und Breite werden gewechselt wenn vertikale).

Diese Abbildung zeigt die Auswirkungen der **ItemsStretch** Werte in einem horizontalen Layout.

![Stretch für einheitliche Grid-Element](images/uniform-grid-item-stretch.png)

Wenn **ItemsStretch** ist **keine**, Sie können festlegen, die [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) Eigenschaft, um anzugeben, wie zusätzlicher Speicherplatz verwendet wird, die Elemente ausrichten.

Diese Liste zeigt die verfügbaren Werte. Die Definitionen davon aus, der Standardwert **Ausrichtung** von **horizontale**.

- **Start**: Elemente werden mit dem Anfang der Zeile ausgerichtet. Zusätzliche Leerzeichen am Ende der Zeile nicht verwendete bleibt. Dies ist die Standardeinstellung.
- **Center**: Elemente werden in der Mitte der Zeile ausgerichtet. Zusätzlicher Leerraum wird am Anfang und Ende der Zeile gleichmäßig verteilt.
- **End**: Elemente werden am Ende der Zeile ausgerichtet. Zusätzlicher Leerraum wird am Anfang der Zeile nicht verwendete belassen.
- **SpaceAround**: Elemente werden gleichmäßig verteilt. Gleich viel Speicherplatz wird vor und nach jedem Element hinzugefügt.
- **SpaceBetween**: Elemente werden gleichmäßig verteilt. Gleich viel Speicherplatz wird zwischen jedem Element hinzugefügt. Es wird kein Leerraum am Anfang und Ende der Zeile hinzugefügt.
- **SpaceEvenly**: Elemente werden mit gleich viel Platz zwischen den einzelnen Elementen sowohl am Anfang und Ende der Zeile gleichmäßig verteilt.

Diese Abbildung zeigt die Auswirkungen der **ItemsStretch** Werte in einem vertikalen Layout (angewendet auf Spalten anstelle von Zeilen).

![Eine Begründung Uniform Grid-Element](images/uniform-grid-item-justification.png)

> [!TIP]
> Die **ItemsStretch** Eigenschaft wirkt sich auf die _Measure_ Layout übergeben. Die **ItemsJustification** Eigenschaft wirkt sich auf die _anordnen_ Layout übergeben.

Dieses Beispiel veranschaulicht die legen Sie die **ItemsRepeater.Layout** Eigenschaft, um eine **UniformGridLayout**.

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

## <a name="lifecycle-events"></a>Ereignisse des Lebenszyklus

Wenn Sie Elemente in Hosten einer [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), müssen Sie möglicherweise Maßnahmen ergreifen, wenn ein Element angezeigt wird oder beendet wird, angezeigt werden, z. B. wie die Startzeit für eines asynchronen Downloads der Inhalte, verknüpfen Sie das Element einen Mechanismus zum Nachverfolgen der Auswahl oder Beenden Sie eine Hintergrundaufgabe.

In einem Steuerelement virtualisierenden können nicht Sie auf Laden/Entladen Ereignisse verlassen, da das Element nicht aus der visuellen echtzeitstruktur entfernt werden kann, wenn es erneuert wurde. Stattdessen werden andere Ereignisse bereitgestellt, um den Lebenszyklus von Elementen zu verwalten. Dieses Diagramm zeigt den Lebenszyklus eines Elements in einer ItemsRepeater, und wenn die verwandten Ereignisse ausgelöst werden.

![Diagramm zum Lebenszyklus-Ereignis](images/items-repeater-lifecycle.png)

- [**ElementPrepared** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) tritt jedes Mal ein Element für die Verwendung bereitgestellt wird. Dies tritt auf, für sowohl eine neu erstellte Element als auch ein Element, das bereits vorhanden ist und aus der Warteschlange für die Wiederverwendung erneut verwendet wird.
- [**ElementClearing** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) tritt auf, unmittelbar jedes Mal ein Element in die Warteschlange Wiederverwendung, z. B. Wenn sie außerhalb des Bereichs von liegt gesendet wurde Elemente realisiert.
- [**ElementIndexChanged** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) tritt auf, für jedes "UIElement" erzielt, in dem der Index des Elements dar geändert hat. Z. B. wenn ein anderes Element hinzugefügt oder entfernt werden, in der Datenquelle, der Index für Elemente, die in der Sortierung Folgen dieses Ereignis empfangen.

Dieses Beispiel zeigt, wie Sie diese Ereignisse verwenden können, zum Anfügen eines Diensts benutzerdefinierte Auswahl, um die Auswahl von Elementen in einem benutzerdefinierten Steuerelement nachzuverfolgen, die ItemsRepeater verwendet wird, um Elemente anzuzeigen.

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

Beim Ausführen von Aktionen wie das Filtern oder sortieren das DataSet Sie üblicherweise möglicherweise besitzen den früheren Satz von Daten in die neuen Daten verglichen, und über eine präzise änderungsbenachrichtigungen ausgegeben [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged). Allerdings ist es oft einfacher, vollständig ersetzen Sie die alten Daten mit den neuen Daten und zum Auslösen einer abfragebenachrichtigung mit Auflistung ändern der [zurücksetzen](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction) Aktion stattdessen.

In der Regel führt dazu, dass ein Zurücksetzen ein Steuerelements, das Freigeben von vorhandenen untergeordneten Elemente und das Starten über die Benutzeroberfläche von den beginnend an der Bildlaufposition 0 erstellen, wie er kennt genau wie die Daten während des Zurücksetzens geändert hat.

Wenn die Auflistung als zugewiesen unterstützt die ItemsSource eindeutige Bezeichner durch die Implementierung der [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping) Schnittstelle, und klicken Sie dann die ItemsRepeater schnell ermitteln:

- wiederverwendbare UIElements für Daten, die vor und nach dem Zurücksetzen vorhanden waren
- zuvor sichtbare Elemente, die entfernt wurden
- neue Elemente hinzugefügt werden, die angezeigt werden

Dadurch wird die ItemsRepeater zu vermeiden, die Bildlaufposition 0 zu beginnen. Außerdem können sie schnell Wiederherstellen der UIElements für Daten, die in dem Zurücksetzen nicht geändert haben, wodurch eine bessere Leistung.

Dieses Beispiel zeigt, wie eine Liste von Elementen in einem vertikalen Stapel angezeigt, in denen _MyItemsSource_ ist eine benutzerdefinierte Datenquelle, die eine zugrunde liegende Liste von Elementen umschließt. Macht eine _Daten_ -Eigenschaft, die verwendet werden kann, um als Quelle Elemente zu verwenden, die zurücksetzen ein dann löst eine neue Liste neu zuweisen.

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

## <a name="create-a-custom-collection-control"></a>Erstellen eines benutzerdefinierten Sammlung-Steuerelements

Sie können die [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) So erstellen Sie eine benutzerdefinierte Sammlung-Steuerelement mit seinen eigenen Typ des Steuerelements, das jedes Element vorhanden.

> [!NOTE]
> Dies ist vergleichbar mit der Verwendung **ItemsControl**, jedoch nicht von **ItemsControl-Element** und eine **ItemsPresenter** in der Steuerelementvorlage, die Sie von ableiten **Steuerelement** und fügen Sie eine **ItemsRepeater** in der Steuerelementvorlage. Das benutzerdefinierte Steuerelement "verfügt über eine" **ItemsRepeater** im Vergleich zu "ist ein" **ItemsControl**. Das bedeutet, Sie müssen auch explizit auswählen, welche Eigenschaften verfügbar machen, anstatt die geerbte Eigenschaften nicht unterstützt.

In diesem Beispiel wird gezeigt, wie zum Platzieren einer [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) in der Vorlage ein benutzerdefiniertes Steuerelement mit dem Namen _MediaCollectionView_ und seine Eigenschaften verfügbar machen.

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

## <a name="display-grouped-items"></a>Gruppiert Elemente anzeigen

Können geschachtelt werden ein [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) in die [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) von einem anderen ItemsRepeater Erstellung geschachtelter Virtualisierung Layouts. Das Framework veranlasst effiziente Verwendung von Ressourcen durch Minimierung der unnötigen Realisierung von Elementen, die nicht sichtbar sind oder in der Nähe der aktuellen Viewport.

Dieses Beispiel zeigt, wie Sie eine Liste der gruppierten Elemente in einem vertikalen Stapel anzeigen können. Die äußere ItemsRepeater generiert jede Gruppe. In der Vorlage für jede Gruppe wird eine andere ItemsRepeater Elemente generiert.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          TextBlock Text="{x:Bind AppTitle}"/>
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

Dieses Beispiel zeigt ein Layout für eine app mit verschiedenen Kategorien, die mit der benutzereinstellung ändern können und werden als horizontal Scrollen Listen angezeigt, wie hier gezeigt.

![Geschachtelte Layouts mit Elementen repeater](images/items-repeater-nested-layout.png)

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

## <a name="bringing-an-element-into-view"></a>Schalten ein Element in der Ansicht

Der XAML-Framework behandelt bereits ein "FrameworkElement" in der Ansicht zu bringen, wenn sie entweder 1) erhält den Tastaturfokus oder (2) Wenn Sie die Sprachausgabe den Fokus erhält. Möglicherweise anderen Fällen Sie explizit ein Element in der Ansicht erscheinen müssen. Z. B. in der Antwort auf eine Benutzeraktion oder um den Zustand der Benutzeroberfläche nach einer Seitennavigation wiederherzustellen.

Schalten eine virtualisierte Element in der Ansicht umfasst Folgendes:
1. Beachten Sie ein UIElement für ein Element
2. Führen Sie das Layout, um sicherzustellen, dass das Element eine gültige Position hat.
3. Initiiert eine Anforderung an das erkannte Element sichtbar zu machen.

Das folgende Beispiel zeigt die folgenden Schritte aus, als Teil der Wiederherstellung der Bildlaufposition eines Elements in einer flachen, vertikale Liste nach einer Seitennavigation. Im Fall von hierarchischen Daten geschachtelte ItemsRepeaters der Ansatz ist im Wesentlichen identisch, jedoch muss auf jeder Ebene der Hierarchie ausgeführt werden.

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
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualWidth, anchor.ActualHeight));
        relativeVerticalOffset = this.sv.VerticalOffset – anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>Aktivieren der Barrierefreiheit

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) bietet keine Standardbenutzeroberfläche für den Zugriff auf. Die Dokumentation zu [Benutzerfreundlichkeit für UWP-apps](/windows/uwp/design/usability) bietet eine Fülle von Informationen, die Ihnen sicherzustellen, dass Ihre app bietet eine Benutzeroberfläche für die inklusive. Wenn Sie die ItemsRepeater zum Erstellen eines benutzerdefinierten Steuerelements verwenden, müssen Sie auf die Dokumentation auf anzuzeigen [Automatisierungspeers von benutzerdefinierten](/windows/uwp/design/accessibility/custom-automation-peers).

### <a name="keyboarding"></a>Tastatureingabe
Die minimale keyboarding-Unterstützung für das Verschieben von konzentrieren, die ItemsRepeater bietet basiert darauf, dass die XAML [2D Richtungsnavigation für Keyboarding](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard).

![Richtung](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

Die ItemsRepeaters [XYFocusKeyboardNavigation Modus](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) ist _aktiviert_ standardmäßig. Je nach den gewünschten Erfahrung, erwägen Sie Unterstützung für allgemeine [Tastenkombinationen](/windows/uwp/design/input/keyboard-interactions) wie Start, Ende, PageUp und Bild-ab.

Die ItemsRepeater gewährleistet automatisch, dass die Standardreihenfolge für die Registerkarte für die enthaltenen Elemente (ob oder nicht virtualisiert) die gleiche Reihenfolge entspricht, die die Elemente in den Daten angegeben werden. Hat standardmäßig die ItemsRepeater seine [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) -Eigenschaftensatz auf [einmal](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) anstelle der standardmäßigen allgemeine _lokalen_.

> [!NOTE]
> Die ItemsRepeater merkt sich nicht automatisch auf das letzte fokussierte Element aus.  Dies bedeutet, dass die Umschalt + Tab, bis zum letzten genommen werden können, Element realisiert, wenn ein Benutzer verwendet.

### <a name="announcing-item-x-of-y-in-screen-readers"></a>Ankündigung von "Element _X_ von _Y_" in der Sprachausgabe

Müssen Sie verwalten, Festlegen der entsprechenden Automatisierungseigenschaften, z. B. die Werte für **PositionInSet** und **SizeOfSet**, und stellen Sie sicher, sie auf dem neuesten Stand bleiben, wenn Elemente hinzugefügt werden, usw. verschoben, entfernt.

In einigen benutzerdefinierten Layouts möglicherweise nicht auf der visuellen Reihenfolge eine offensichtliche Sequenz.  Benutzer erwarten minimal an, dass die Werte für die PositionInSet und SizeOfSet-Eigenschaft, die von Bildschirmsprachausgaben verwendet die Reihenfolge entspricht, die die Elemente in den Daten (Offset von 1 fest, um gezählt zu werden, 0-basierte natürliche übereinstimmen) angezeigt werden.

Die beste Möglichkeit, dies zu erreichen ist, dass des Automatisierungspeers für die Element-Steuerelement implementieren die [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) und [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) Methoden und die Position des Elements im DataSet-Bericht durch das Steuerelement dargestellt. Der Wert wird nur zur Laufzeit beim Zugriff über eine hilfstechnologie berechnet, und es auf dem neuesten Stand zu halten, wird nicht. Der Wert entspricht der Reihenfolge der Daten.

Dieses Beispiel zeigt, wie dies möglich wäre bei der Präsentation eines benutzerdefinierten Steuerelements namens _CardControl_.

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
