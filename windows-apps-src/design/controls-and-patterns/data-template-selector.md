---
Description: Verwenden Sie die Datenvorlagenauswahl, um die Formate ihrer Elemente auf der Grundlage der Elementeigenschaften anzupassen.
title: Datenvorlagenauswahl
label: Data template selection
template: detail.hbs
ms.date: 10/18/2019
ms.topic: article
keywords: Windows 10, UWP
pm-contact: anawish
ms.openlocfilehash: 382e28b38347a4901e781a12637423260c4bd3e3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160384"
---
# <a name="data-template-selection-styling-items-based-on-their-properties"></a>Datenvorlagenauswahl: Formatieren von Elementen basierend auf ihren Eigenschaften

Das angepasste Design von Sammlungssteuerelementen wird von einem [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)-Objekt verwaltet. Datenvorlagen definieren Layout und Format jedes Elements, und legen fest, dass das Markup auf jedes Element in der Sammlung angewendet wird. In diesem Artikel wird beschrieben, wie Sie ein [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)-Objekt verwenden, um unterschiedliche Datenvorlagen auf eine Sammlung anzuwenden und die zu verwendende Datenvorlage basierend auf bestimmten Elementeigenschaften oder -werten ihrer Wahl festzulegen.

> **Wichtige APIs:** [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector), [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)

[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) ist eine Klasse, die eine benutzerdefinierte Vorlagenauswahllogik ermöglicht. Sie können Regeln definieren, die festlegen, welche Datenvorlage für bestimmte Elemente in einer Sammlung verwendet werden soll. Um diese Logik zu implementieren, erstellen Sie eine Unterklasse von DataTemplateSelector im in der CodeBehind-Komponente und definieren die Logik, die bestimmt, welche Datenvorlage für welche Kategorie von Elementen verwendet werden soll (z.B. Elemente eines bestimmten Typs oder Elemente mit einem bestimmten Eigenschaftswert usw.). Sie deklarieren eine Instanz dieser Klasse im Ressourcenabschnitt ihrer XAML-Datei zusammen mit den Definitionen der zu verwendenden Datenvorlagen. Diese Ressourcen werden mit einem `x:Key`-Wert identifiziert, mit dem Sie in ihrer XAML darauf verweisen können.

## <a name="prerequisites"></a>Voraussetzungen

- [Verwenden und Erstellen eines Sammlungssteuerelements, z.B. „ListView“ oder „GridView“.](listview-and-gridview.md)
- [Anpassen des Aussehens Ihrer Elemente mithilfe von „DataTemplate“.](item-containers-templates.md#data-template)

## <a name="when-not-to-use-a-datatemplateselector"></a>Wann „DataTemplateSelector“ nicht verwendet werden sollte

Im Allgemeinen sollten Sie nicht jedem Element in einem ListView- oder GridView-Steuerelement ein vollkommen anderes Layout/Format zuweisen. Dies wäre eine schlechte Verwendung der Datenvorlagenauswahl und wirkt sich negativ auf die Leistung aus.

Bestimmte Elemente der visuellen Darstellung eines Listenelements können mithilfe von nur einer Datenvorlage gesteuert werden, indem bestimmte Eigenschaften gebunden werden. Beispielsweise können Elemente jeweils ein unterschiedliches Symbol aufweisen, indem sie an eine Symbolquelleigenschaft in der Datenvorlage gebunden werden und jedem Element ein anderer Wert für diese Symbolquelleigenschaft zugewiesen wird. Dadurch wird eine bessere Leistung erzielt als bei Verwendung von „DataTemplateSelector“.

## <a name="when-to-use-a-datatemplateselector"></a>Wann „DataTemplateSelector“ verwendet werden sollte

Sie sollten ein [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)-Objekt erstellen, wenn Sie mehrere Datenvorlagen in einem Sammlungssteuerelement verwenden möchten. Ein `DataTemplateSelector`-Objekt bietet Ihnen die Flexibilität, bestimmte Elemente hervorzuheben, und dabei ein ähnliches Layout beizubehalten. Es gibt viele Anwendungsfälle, in denen ein DataTemplateSelector-Objekt hilfreich ist, und einige Szenarien, in denen es besser ist, das verwendete Steuerelement und die verwendete Strategie zu überdenken.

Sammlungssteuerelemente sind in der Regel an eine Sammlung von Elementen des gleichen Typs gebunden. Auch wenn Elemente vom gleichen Typ sind, können sie möglicherweise unterschiedliche Werte für bestimmte Eigenschaften aufweisen oder unterschiedliche Bedeutungen darstellen. Bestimmte Elemente sind können wichtiger als andere sein, oder ein Element ist möglicherweise besonders wichtig oder unterscheidet sich von den anderen und muss visuell hervorgehoben werden. In diesen Fällen ist ein DataTemplateSelector-Objekt sehr hilfreich.

Sie können auch eine Bindung an eine Sammlung herstellen, die verschiedene Elementtypen enthält. Die gebundene Sammlung kann eine Mischung aus Zeichenfolgen, ganzen Zahlen, benutzerdefinierten Klassenobjekten usw. aufweisen. Das macht das DataTemplateSelector-Objekt besonders nützlich, da es basierend auf dem Objekttyp eines Elements verschiedene Datenvorlagen zuweisen kann.

Es folgen einige Beispiele für mögliche Verwendungszwecke einer Datenvorlagenauswahl:

- Darstellung verschiedener Ebenen von Mitarbeitern in einer ListView: für jeden Mitarbeitertyp bzw. jede Mitarbeiterebene ist zur besseren Unterscheidung ggf. ein andersfarbiger Hintergrund erforderlich.
- Darstellung von Verkaufsartikeln in einem Produktkatalog, in dem ein GridView-Objekt verwendet wird: ein Verkaufsartikel benötigt möglicherweise einen roten Hintergrund oder eine Schrift in einer anderen Farbe, um sie gegenüber Artikeln mit regulären Preisen hervorzuheben.
- Darstellung von Gewinner/Top-Fotos in einer Fotogalerie mithilfe von FlipView.
- Die Notwendigkeit, negative/positive Zahlen in einem ListView-Objekt unterschiedlich darzustellen, oder kurze Zeichenfolgen/lange Zeichenfolgen.

## <a name="create-a-datatemplateselector"></a>Erstellen eines DataTemplateSelector-Objekts

Wenn Sie eine Datenvorlagenauswahl erstellen, definieren Sie die Vorlagenauswahllogik in Ihrem Code und die Datenvorlagen in ihrer XAML-Datei.

### <a name="code-behind-component"></a>CodeBehind-Komponente

Wenn Sie eine Datenvorlagenauswahl verwenden möchten, erstellen Sie zuerst eine Unterklasse von [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) (eine Klasse, die davon abgeleitet ist) in der CodeBehind-Komponente. In der Klasse deklarieren Sie jede Vorlage als Eigenschaft der Klasse. Dann überschreiben Sie die [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore)-Methode, um Ihre eigene Vorlagenauswahllogik hinzuzufügen.

Im folgenden finden Sie ein Beispiel für eine einfache `DataTemplateSelector`-Unterklasse mit dem Namen `MyDataTemplateSelector`.

```csharp
public class MyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate Normal { get; set; }
    public DataTemplate Accent { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        if ((int)item % 2 == 0)
        {
            return Normal;
        }
        else
        {
            return Accent;
        }
    }
}
```

Die `MyDataTemplateSelector`-Klasse wird von der `DataTemplateSelector`-Klasse abgeleitet und definiert zuerst zwei [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)-Objekte: `Normal` und `Accent`. Die sind zunächst leere Deklarationen, werden jedoch in der XAML-Datei mit Markup „ausgefüllt“.

Die `SelectTemplateCore`-Methode übernimmt ein Item-Objekt (d.h. jedes Sammlungselement) und wird mit Regeln überschrieben, für angeben, welches `DataTemplate`-Objekt unter welchen Umständen zurückgegeben werden soll. Wenn das Element in diesem Fall eine ungerade Zahl ist, empfängt es die `Accent`-Datenvorlage. Wenn es sich hingegen um eine gerade Zahl handelt, wird die `Normal`-Datenvorlage empfangen.

### <a name="xaml-component"></a>XAML-Komponente

Zweitens müssen Sie eine Instanz dieser neuen `MyDataTemplateSelector`-Klasse im Ressourcenabschnitt der XAML-Datei erstellen. Alle Ressourcen erfordern ein `x:Key`-Objekt, mit dem sie an die `ItemTemplateSelector`-Eigenschaft des Sammlungssteuerelements gebunden werden (in einem späteren Schritt). Außerdem erstellen Sie zwei Instanzen von `DataTemplate`-Objekten und definieren deren Layout im Ressourcenabschnitt. Diese Datenvorlagen weisen Sie den Eigenschaften `Accent` und `Normal` zu, die Sie in der `MyDataTemplateSelector`-Klasse deklariert haben.

Im folgenden finden Sie ein Beispiel für die XAML-Ressourcen und das erforderliche Markup:

```xaml
<Page.Resources>

<DataTemplate x:Key="NormalItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemChromeLowColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<DataTemplate x:Key="AccentItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemAccentColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<l:MyDataTemplateSelector x:Key="MyDataTemplateSelector"
    Normal="{StaticResource NormalItemTemplate}"
    Accent="{StaticResource AccentItemTemplate}"/>

</Page.Resources>
```

Wie Sie oben sehen können, werden die beiden Datenvorlagen `Normal` und `Accent` definiert. Beide zeigen Elemente als Schaltflächen an, aber die `Accent`-Datenvorlage verwendet einen Akzentfarbpinsel für den Hintergrund, während für die `Normal`-Datenvorlage ein grauer Farbpinsel (`SystemChromeLowColor`) verwendet wird. Diese beiden Datenvorlagen werden dann den DataTemplate-Objekten `Normal` und `Accent` zugewiesen, die Attribute der in der C#-Code-Behind-Komponente erstellten MyDataTemplateSelector-Klasse sind.

Der letzte Schritt besteht darin, das `DataTemplateSelector`-Objekt an die `ItemTemplateSelector`-Eigenschaft des Sammlungssteuerelements zu binden (in diesem Fall ein „ListView“-Objekt). Dies ersetzt die Notwendigkeit, der `ItemTemplate`-Eigenschaft einen Wert zuzuweisen. 

```xaml
<ListView x:Name = "TestListView"
          ItemsSource = "{x:Bind NumbersList}"
          ItemTemplateSelector = "{StaticResource MyDataTemplateSelector}">
</ListView>
```

Nachdem der Code kompiliert wurde, wird jedes Sammlungselement über die überschriebene `SelectTemplateCore`-Methode in `MyDataTemplateSelector` ausgeführt und mit dem entsprechenden DataTemplate-Objekt gerendert.

> [!IMPORTANT]
> Wenn Sie `DataTemplateSelector` mit einem [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)-Objekt verwenden, binden Sie das `DataTemplateSelector`-Objekt an die `ItemTemplate`-Eigenschaft. Das `ItemsRepeater`-Objekt weist keine `ItemTemplateSelector`-Eigenschaft auf.

## <a name="datatemplateselector-performance-considerations"></a>Überlegungen zur Leistung von DataTemplateSelector

Wenn Sie ein ListView- oder GridView-Objekt mit einer großen Datensammlung verwenden, kann die Leistung beim Scrollen und Schwenken ein Problem darstellen. Um die Leistung großer Sammlungen zu gewährleisten, können Sie einige Schritte ausführen, um die Leistung Ihrer Datenvorlagen zu verbessern. Diese werden in [Optimieren der ListView- und GridView-Benutzeroberfläche](../../debug-test-perf/optimize-gridview-and-listview.md) ausführlich beschrieben.

- _Elementreduzierung pro Element_: Begrenzen Sie die Anzahl der Oberflächenelemente in einer Datenvorlage auf ein vernünftiges Minimum.
- Containerwiederverwendung mit heterogenen Sammlungen
  - Verwenden des _ChoosingItemContainer-Ereignisses_: Dieses Ereignis ist eine leistungsstarke Möglichkeit zur Verwendung unterschiedlicher Datenvorlagen für verschiedene Elemente. Um eine optimale Leistung zu erzielen, sollten Sie das Zwischenspeichern optimieren und Datenvorlagen für Ihre spezifischen Daten auswählen.
  - Verwenden Sie eine _Elementvorlagenauswahl_: Eine Elementvorlagenauswahl (`DataTemplateSelector`) sollte in einigen Fällen aufgrund der Auswirkung auf die Leistung vermieden werden.