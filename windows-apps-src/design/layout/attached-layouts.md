---
Description: Sie können angefügte Layouts für die Verwendung mit Containern definieren, etwa dem ItemsRepeater-Steuerelement.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dc23e86f85c5db3dd10c5cec152047be387d4513
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282293"
---
# <a name="attached-layouts"></a>Angefügte Layouts

Ein Container (z. B. ein Panel), der seine Layoutlogik an ein anderes Objekt delegiert, baut auf dem angefügten Layoutobjekt auf, um das Layoutverhalten für seine untergeordneten Elemente bereitzustellen.  Ein angefügtes Layoutmodell gibt einer Anwendung die Flexibilität, das Layout von Elementen zur Laufzeit zu ändern oder Layoutaspekte auf einfache Weise zwischen verschiedenen Teilen der Benutzeroberfläche zu teilen (beispielsweise Elemente in den Zeilen einer Tabelle, die innerhalb einer Spalte ausgerichtet dargestellt sind).

In diesem Thema befassen wir uns damit, was das Erstellen eines angefügten Layouts (virtualisierend und nicht virtualisierend) beinhaltet, welche Konzepte und Klassen Sie kennen müssen und welche Vor- und Nachteile Sie bei der Entscheidung zwischen Ihnen berücksichtigen müssen.

| **Abrufen der Windows-UI-Bibliothek** |
| - |
| Dieses Steuerelement ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für UWP-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Wichtige APIs:**

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [Layout](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (Vorschau)

## <a name="key-concepts"></a>Wichtige Konzepte

Zum Durchführen des Layouts müssen für jedes Element zwei Fragen beantwortet werden:

1. Welche ***Größe*** soll das Element aufweisen?

2. Welche ***Position*** soll das Element haben?

Das XAML-Layoutsystem, das Antworten auf diese Fragen gibt, wird kurz im Rahmen der Erörterung von [benutzerdefinierten Panels](/windows/uwp/design/layout/custom-panels-overview) behandelt.

### <a name="containers-and-context"></a>Container und Kontext

Konzeptionell füllt ein XAML-[Panel](/uwp/api/windows.ui.xaml.controls.panel) zwei wichtige Rollen in diesem Framework aus:

1. Es kann untergeordnete Elemente enthalten und es führt Verzweigung in die Baumstruktur der Elemente ein.
2. Es wendet auf die untergeordneten Elemente eine bestimmte Layoutstrategie an.

Aus diesem Grund wurde ein Panel in XAML häufig als Synonym für Layout verwendet, technisch erledigt es aber mehr.

Der [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater) verhält sich ebenso wie ein Panel, im Gegensatz zu Panel macht er aber keine Eigenschaft „Children“ verfügbar, die es ermöglicht, untergeordnete UIElements hinzuzufügen oder zu entfernen.  Stattdessen wird die Lebensdauer seiner untergeordneten Elemente automatisch vom Framework so verwaltet, dass sie einer Sammlung von Datenelementen entspricht.  Zwar ist er nicht von Panel abgeleitet, er verhält sich aber wie ein Panel und wird vom Framework auch so behandelt.

> [!NOTE]
> Das [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) ist ein Container, der von Panel abgeleitet ist und seine Logik an das angefügte [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout)-Objekt delegiert.  LayoutPanel ist eine *Vorschauversion* und zurzeit nur in den *Vorabversions*-Drops des WinUI-Pakets erhältlich.

#### <a name="containers"></a>Container

Konzeptionell ist [Panel](/uwp/api/windows.ui.xaml.controls.panel) ein Container von Elementen, der außerdem über die Fähigkeit verfügt, Pixel für einen [Background](/uwp/api/windows.ui.xaml.controls.panel.background) zu rendern.  Panels stellen eine Möglichkeit dar, allgemeine Layoutlogik in einem leicht verwendbaren Paket zu kapseln.

Das Konzept des **angefügten Layouts** verdeutlicht die Unterscheidung zwischen den zwei Rollen von Container und Layout.  Wenn der Container seine Layoutlogik an ein anderes Objekt delegiert, bezeichnen wir dieses Objekt als das angefügte Layout, wie im Codeausschnitt unten zu sehen. Container, die von [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement) erben, wie etwa das LayoutPanel, machen die allgemeinen Eigenschaften (beispielsweise Höhe und Breite), die die Eingabe für den XAML-Layoutvorgang bilden, automatisch verfügbar.

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

Während des Layoutprozesses stützt sich der Container auf das angefügte *UniformGridLayout*, um seine untergeordneten Elemente zu bemessen und anzuordnen.

#### <a name="per-container-state"></a>Pro-Container-Zustand

Innerhalb eines angefügten Layouts kann eine einzelne Instanz des Layoutobjekts *vielen* Containern zugeordnet sein, wie im Codeausschnitt unten zu sehen. Daher darf sie nicht vom Hostcontainer abhängig sein oder direkt auf diesen verweisen.  Beispiel:

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

Für diese Situation muss *ExampleLayout* den Zustand, den es in seiner Layoutberechnung verwendet, und den Speicherort dieses Zustands sorgfältig abwägen, um einen gegenseitigen Einfluss auf das Layout der Elemente in einem Panel zu vermeiden.  Dies wäre analog zu einem benutzerdefinierten Panel, dessen MeasureOverride- und ArrangeOverride-Logik von den Werten seiner *statischen* Eigenschaften abhängt.

#### <a name="layoutcontext"></a>LayoutContext

Es ist der Zweck von [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext), diese Herausforderungen zu bewältigen.  Er bietet dem angefügten Layout die Möglichkeit, mit dem Hostcontainer zu interagieren, etwa durch Abrufen von untergeordneten Elementen, ohne eine direkte Abhängigkeit zwischen beiden entstehen zu lassen. Der Kontext ermöglicht es dem Layout außerdem, jeden beliebigen für es erforderlichen Zustand zu speichern, der möglicherweise mit den untergeordneten Elementen des Containers zusammenhängt.

Einfache, nicht virtualisierende Layouts müssen oftmals überhaupt keinen Zustand bewahren, wodurch sich das Problem nicht stellt. Bei einem komplexeren Layout, wie etwa einem Grid (Raster), wird man sich jedoch eher dafür entscheiden, den Zustand zwischen dem Measure- und dem Arrange-Aufruf beizubehalten, um die Neuberechnung eines Werts zu vermeiden.

Beim Virtualisieren von Layouts muss *oftmals* der Zustand zwischen Measure und Arrange beibehalten werden, ebenso zwischen iterativen Layoutdurchläufen.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Pro-Container-Zustand: Initialisieren und Aufheben der Initialisierung

Wenn ein Layout an einen Container angefügt wird, wird dessen [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)-Methode aufgerufen und bietet die Möglichkeit, ein Objekt zum Speichern des Zustands zu initialisieren.

Ähnlich dazu wird die [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)-Methode aufgerufen, wenn das Layout von einem Container entfernt wird.  Dadurch erhält das Layout die Möglichkeit, alle Zustandsinformationen zu löschen, die es dem betreffenden Container zugeordnet hatte.

Das Zustandsobjekt des Layouts kann mithilfe der [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate)-Eigenschaft im Kontext zusammen mit dem Container gespeichert und aus diesem abgerufen werden.

### <a name="ui-virtualization"></a>UI-Virtualisierung

UI-Virtualisierung bedeutet das Verzögern der Erstellung eines UI-Objekts bis zu dem Zeitpunkt, _da es benötigt wird_.  Das stellt eine Leistungsoptimierung dar.  In Szenarien ohne Bildlauf kann die Bestimmung _wann es erforderlich ist_ auf einer beliebigen Anzahl App-spezifischer Aspekte basieren.  In diesen Fällen sollte für Apps die Verwendung von [x:Load](../../xaml-platform/x-load-attribute.md) in Erwägung gezogen werden. Dafür ist in Ihrem Layout keine besondere Behandlung erforderlich.

In auf Bildlauf basierenden Szenarien, etwa einer Liste, hängt die Bestimmung _wann es erforderlich ist_ oftmals davon ab, „ob es für einen Benutzer sichtbar ist“, was stark davon abhängt, wo etwas im Layoutprozess platziert wurde. Hier sind besondere Überlegungen erforderlich.  Dieses Szenario bildet einen Schwerpunkt dieses Dokuments.

> [!NOTE]
> Zwar wird es in diesem Dokument nicht behandelt, jedoch können die gleichen Funktionen, die UI-Virtualisierung in Szenarien mit Bildlauf ermöglichen, in Szenarien ohne Bildlauf angewendet werden.  Als Beispiel eignet sich ein datengestütztes ToolBar-Steuerelement, das die Lebensdauer der Befehle verwaltet, die es darstellt, und auf Änderungen am verfügbaren Platz reagiert, indem es Elemente zwischen einem sichtbaren Bereich und einem Überlaufmenü recycelt/verschiebt.

## <a name="getting-started"></a>Erste Schritte

Entscheiden Sie zunächst, ob das Layout, das Sie erstellen müssen, UI-Virtualisierung unterstützen soll.

**Ein paar Dinge, die Sie beachten müssen...**

1. Nicht virtualisierende Layouts sind einfacher zu erstellen. Wenn die Anzahl der Elemente immer klein ist, wird die Erstellung eines nicht virtualisierenden Layouts empfohlen.
2. Die Plattform bietet eine Reihe angefügter Layouts, die zusammen mit [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) und [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) funktionieren und allgemeine Anforderungen abdecken.  Machen Sie sich mit diesen vertraut, bevor Sie sich für die Definition eines benutzerdefinierten Layouts entscheiden.
3. Mit virtualisierenden Layouts geht im Vergleich mit nicht virtualisierenden Layouts immer ein gewisser Mehraufwand an Kosten/Komplexität einher.  Als Daumenregel lässt sich davon ausgehen, dass wenn die untergeordneten Elemente, die vom Layout verwaltet werden müssen, in einen Bereich passen, der die dreifache Fläche des Viewports aufweist, ein virtualisierendes Layout möglicherweise keinen großen Gewinn bringt. Die dreifache Größe wird detaillierter weiter unten in diesem Dokument erläutert, sie hängt aber mit der prinzipbedingten Asynchronität des Bildlaufs unter Windows und deren Einfluss auf die Virtualisierung zusammen.

> [!TIP]
> Als Bezugspunkt kann angesehen werden, dass die Standardeinstellungen für [ListView](/uwp/api/windows.ui.xaml.controls.listview) (und [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) vorsehen, dass Recycling erst beginnt, wenn die Anzahl der Elemente ausreichend ist um die dreifache Größe des aktuellen Viewports zu füllen.

**Auswählen Ihres Basistyps**

![Hierarchie für angefügtes Layout](images/xaml-attached-layout-hierarchy.png)

Der Basistyp [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) weist zwei abgeleitete Typen auf, die als Ausgangspunkt für das Erstellen eines angefügten Layouts dienen:

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Nicht virtualisierendes Layout

Die Vorgehensweise beim Erstellen eines nicht virtualisierenden Layouts sollte allen Benutzern vertraut sein, die ein [benutzerdefiniertes Panel](/windows/uwp/design/layout/custom-panels-overview) erstellt haben.  Es gelten dieselben Konzepte.  Der Hauptunterschied besteht darin, dass ein [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) verwendet wird, um auf die [untergeordnete](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) Sammlung zuzugreifen, und das Layout hat die Möglichkeit, den Zustand zu speichern.

1. Leiten Sie vom Basistyp [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (anstelle von Panel) ab.
2. *(Optional)* Definieren Sie Abhängigkeitseigenschaften, deren Änderung bewirkt, dass das Layout für ungültig erklärt wird.
3. _(**Neu**/Optional)_ Initialisieren Sie jedes für das Layout erforderliche Zustandsobjekt als Teil von [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Speichern Sie es im Hostcontainer, indem Sie den im Kontext bereitgestellten [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) verwenden.
4. Überschreiben Sie [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride), und rufen Sie die [Measure](/uwp/api/windows.ui.xaml.uielement.measure)-Methode für alle untergeordneten Elemente auf.
5. Überschreiben Sie [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride), und rufen Sie die [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange)-Methode für alle untergeordneten Elemente auf.
6. *(**Neu**/Optional)* Löschen Sie als Teil von [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) jeden gespeicherten Zustand.

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Beispiel: Ein einfaches Stapellayout (Elemente verschiedener Größe)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

Hier sehen Sie ein sehr einfaches, nicht virtualisierendes Stapellayout aus Elementen verschiedener Größe. Es fehlen ihm alle Eigenschaften, mit denen das Verhalten des Layouts angepasst werden könnte. Die Implementierung unten veranschaulicht, wie das Layout auf dem vom Container bereitgestellten Kontextobjekt aufbaut, um dies zu erreichen:

1. Abrufen der Anzahl untergeordneter Elemente und
2. Zugreifen auf jedes untergeordnete Element anhand des Index.

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>Virtualisierende Layouts

Die allgemeinen Schritte sind bei einem virtualisierenden Layout ähnlich wie bei einem nicht virtualisierenden Layout.  Die Komplexität liegt größtenteils in der Bestimmung, welche Elemente innerhalb des Viewports liegen und realisiert werden sollten.

1. Leiten Sie vom Basistyp [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) ab.
2. (Optional) Definieren Sie Abhängigkeitseigenschaften, deren Änderung bewirkt, dass das Layout für ungültig erklärt wird.
3. Initialisieren Sie jedes für das Layout erforderliche Zustandsobjekt als Teil von [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Speichern Sie es im Hostcontainer, indem Sie den im Kontext bereitgestellten [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) verwenden.
4. Überschreiben Sie [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride), und rufen Sie die [Measure](/uwp/api/windows.ui.xaml.uielement.measure)-Methode für jedes untergeordnete Element auf, das realisiert werden soll.
   1. Die [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)-Methode wird verwendet, um ein UIElement abzurufen, das vom Framework vorbereitet wurde (beispielsweise durch Anwenden der Datenbindungen).
5. Überschreiben Sie [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride), und rufen Sie die [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange)-Methode für jedes realisierte Element auf.
6. (Optional) Löschen Sie als Teil von [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) jeden gespeicherten Zustand.

> [!TIP]
> Der Wert, der von [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) zurückgegeben wird, wird als Größe des virtualisierten Inhalts verwendet.

Beim Erstellen eines virtualisierenden Layouts sind zwei allgemeine Ansätze zu bedenken.  Ob Sie sich für den einen oder den anderen entscheiden, hängt zum großen Teil davon ab, „wie Sie die Größe eines Elements bestimmen“.  Wenn es ausreicht, den Index eines Elements im Dataset zu kennen oder die Daten selbst letztlich dessen Größe bestimmen, sehen wir es als **datenabhängig** an.  Diese lassen sich einfacher erstellen.  Wenn jedoch die einzige Möglichkeit, die Größe für ein Element zu bestimmen, darin besteht, es zu erstellen und auf der Benutzeroberfläche abzumessen, bezeichnen wir es als **inhaltsabhängig**.  Diese sind komplexer.

### <a name="the-layout-process"></a>Der Layoutprozess

Unabhängig davon, ob Sie ein daten- oder inhaltsabhängiges Layout erstellen, es ist wichtig, den Layoutprozess und die Rolle des asynchronen Bildlaufs unter Windows zu verstehen.

Eine (übermäßig) vereinfachte Darstellung der Schritte, die vom Framework von dessen Start bis zum Anzeigen der Benutzeroberfläche auf dem Bildschirm ausgeführt werden, bieten die folgenden Punkte:

1. Das Markup wird analysiert.

2. Es wird eine Elementstruktur erstellt.

3. Es wird ein Layoutdurchlauf ausgeführt.

4. Es wird ein Renderdurchlauf ausgeführt.

Mit UI-Virtualisierung wird das Erstellen der Elemente, das normalerweise in Schritt 2 erfolgen würde, verzögert oder frühzeitig beendet, sobald festgestellt wurde, das ausreichend Inhalt erstellt wurde, um den Viewport zu füllen. Ein virtualisierender Container (z. B. ItemsRepeater) greift auf sein angefügtes Layout zurück, um diesen Prozess zu steuern. Er stellt dem angefügten Layout einen [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) zur Verfügung, der die zusätzlichen Informationen enthält, die für ein virtualisierendes Layout erforderlich sind.

**Das RealizationRect (d. h. Viewport)**

Der Bildlauf unter Windows erfolgt asynchron zum UI-Thread. Er wird nicht durch das Layout des Frameworks gesteuert.  Vielmehr erfolgen Interaktion und Bewegung im Kompositionsmodul des Systems. Der Vorteil dieses Ansatzes besteht darin, dass das Schwenken von Inhalten immer bei 60 B/s erfolgen kann.  Die Herausforderung besteht jedoch darin, dass der „Viewport“, wie ihn das Layout wahrnimmt, im Vergleich zu dem, was tatsächlich auf dem Bildschirm sichtbar ist, etwas veraltet sein kann. Wenn ein Benutzer schnell scrollt, kann er die Geschwindigkeit des UI-Threads beim Generieren neuer Inhalte übertreffen und „ins Schwarze schwenken“. Aus diesem Grund ist es für ein virtualisierendes Layout oftmals erforderlich, einen zusätzlichen Puffer vorbereiteter Elemente zu erstellen, der dafür ausreicht, einen größeren Bereich als den Viewport zu füllen. Dann werden dem Benutzer beim Scrollen unter stärkerer Auslastung des Systems trotzdem Inhalte angezeigt.

![Realisierungrechteck](images/xaml-attached-layout-realizationrect.png)

Da die Elementerstellung teuer ist, statten virtualisierende Container (z. B. [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) das angefügte Layout zu Anfang mit einem [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) aus, das mit dem Viewport übereinstimmt. In Leerlaufzeiten kann der Container den Puffer vorbereiteter Inhalte durch wiederholtes Aufrufen des Layouts auffüllen, indem er ein zunehmend größeres Realisierungsrechteck verwendet. Dieses Verhalten stellt eine Leistungsoptimierung dar, die versucht, ein Gleichgewicht zwischen einer schnellen Startzeit und einem guten Schwenkverhalten zu bieten. Die maximale Größe des Puffers, der vom ItemsRepeater generiert wird, wird durch dessen Eigenschaften [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) und [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) gesteuert.

**Wiederverwenden von Elementen (Recycling)**

Vom Layout wird erwartet, dass es bei jeder Ausführung die Elemente dimensioniert und positioniert, um das [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) zu füllen. Standardmäßig recycelt das [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) am Ende jedes Layoutdurchlaufs alle nicht verwendeten Elemente.

Der [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext), der dem Layout als Teil von [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) und [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) übergeben wird, stellt die zusätzlichen Informationen bereit, die von einem virtualisierenden Layout benötigt werden. Dies sind einige der besonders häufig verwendeten Fähigkeiten eines virtualisierenden Layoutkontexts:

1. Abfragen der Anzahl der Elemente in den Daten ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. Abrufen eines bestimmten Elements mithilfe der [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat)-Methode.
3. Abrufen eines [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect), das den Viewport und einen Puffer darstellt, die vom Layout mit realisierten Elementen gefüllt werden sollen.
4. Anfordern des UIElement für ein bestimmtes Element mithilfe der [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)-Methode.

Das Anfordern eines Elements für einen bestimmten Index bewirkt, dass das betreffende Element für diesen Layoutdurchlauf als „in Verwendung“ gekennzeichnet wird. Wenn das Element noch nicht vorhanden ist, wird es realisiert und automatisch für die Verwendung vorbereitet (beispielsweise durch Ausweiten der in einer DataTemplate definierten UI-Baumstruktur, Verarbeiten aller Datenbindungen usw.).  Andernfalls wird es aus einem Pool vorhandener Instanzen abgerufen.

Am Ende jedes Messdurchlaufs wird jedes vorhandene realisierte Element, das nicht als „in Verwendung“ gekennzeichnet wurde, automatisch als für die Wiederverwendung verfügbar angesehen, es sei denn, beim Abrufen des Elements mithilfe der [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)-Methode wurde die Option [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) verwendet. Das Framework verschiebt es automatisch in einen Recyclingpool und macht es verfügbar. Es kann in der Folge für die Verwendung durch einen anderen Container abgerufen werden. Das Framework versucht, dies nach Möglichkeit zu vermeiden, da bei der Neu-Unterordnung eines Elements Kosten anfallen.

Wenn ein virtualisierendes Layout zu Beginn jeder Messung weiß, welche Elemente nicht mehr in das Realisierungsrechteck fallen, kann es deren Wiederverwendung optimieren. Anstatt auf das Standardverhalten des Frameworks zu vertrauen. Das Layout kann Elemente mithilfe der [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement)-Methode präemptiv in den Recyclingpool verschieben.  Das Aufrufen dieser Methode vor dem Anfordern neuer Elemente führt dazu, dass diese Elemente vorhanden sind, wenn das Layout später eine [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)-Anforderung für einen Index ausgibt, der noch keinem Element zugeordnet ist.

Der VirtualizingLayoutContext bietet zwei zusätzliche Eigenschaften, die für Layoutautoren vorgesehen sind, die ein inhaltsabhängiges Layout erstellen. Diese werden weiter unten ausführlicher erörtert.

1. Ein [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex), der eine optionale _Eingabe_ für ein Layout darstellt.
2. Ein [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin), der eine optionale _Ausgabe_ des Layouts darstellt.

## <a name="data-dependent-virtualizing-layouts"></a>Datenabhängige virtualisierende Layouts

Ein virtualisierendes Layout ist einfacher zu erstellen, wenn Sie wissen, welche Größe jedes Element haben sollte, ohne die anzuzeigenden Inhalte messen zu müssen.  Im Rahmen dieses Dokuments bezeichnen wir diese Kategorie virtualisierender Layouts einfach als **Datenlayouts**, da sie normalerweise eine Untersuchung der Daten mit sich bringen.  Auf der Grundlage der Daten kann eine App eine visuelle Darstellung mit bekannter Größe auswählen – möglicherweise, weil diese Teil der Daten ist oder zuvor im Entwurf festgelegt wurde.

Das allgemeine Vorgehen beim Layout ist Folgendes:

1. Die Größe und Position Elements werden berechnet.
2. Im Rahmen von [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. Das [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) wird verwendet, um zu bestimmen, welche Elemente innerhalb des Viewports angezeigt werden sollen.
   2. Mithilfe der Methode [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) wird das UIElement abgerufen, das das Element darstellen soll.
   3. Per [Measure](/uwp/api/windows.ui.xaml.uielement.measure) wird das UIElement mit der vorberechneten Größe gemessen.
3. Im Rahmen von [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) wird mithilfe von [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) jedes realisierte UIElement an der vorab berechneten Position angeordnet.

> [!NOTE]
> Ein Datenlayoutansatz ist häufig inkompatibel mit _Datenvirtualisierung_.  Insbesondere, wenn die einzigen Daten, die in den Arbeitsspeicher geladen werden, diejenigen sind, die für den vom Benutzer sichtbaren Bereich erforderlich sind.  Datenvirtualisierung bezieht sich nicht auf ein verzögertes oder inkrementelles Laden von Daten, wenn ein Benutzer einen Bildlauf nach unten durchführt, wobei die genannten Daten resident bleiben.  Es bezieht sich vielmehr auf den Fall, dass Elemente aus dem Arbeitsspeicher freigegeben werden, wenn sie beim Bildlauf den sichtbaren Bereich verlassen.  Ein Datenlayout, das jedes Datenelement als Teil eines Datenlayouts untersucht, würde die erwartungsgemäße Funktion der Datenvirtualisierung verhindern.  Eine Ausnahme ist ein Layout wie das UniformGridLayout, bei dem angenommen wird, dass alles die gleiche Größe hat.

> [!TIP]
> Wenn Sie ein benutzerdefiniertes Steuerelement für eine Steuerelementbibliothek erstellen, die von anderen in einer großen Bandbreite von Situationen verwendet wird, ist ein Datenlayout möglicherweise keine Option für Sie.

### <a name="example-xbox-activity-feed-layout"></a>Beispiel: Xbox-Aktivitätsfeed-Layout

Die Benutzeroberfläche für den Xbox-Aktivitätsfeed verwendet ein sich wiederholendes Muster bei dem jede Zeile eine breite Kachel, gefolgt von zwei schmalen Kacheln, aufweist und das in der folgenden Zeile umgekehrt wird. Bei diesem Layout stellt die Größe jedes Elements eine Funktion der Elementposition im DataSet und der bekannten Größe der Kacheln (breit i. Vgl. mit schmal) dar.

![Xbox-Aktivitätsfeed](images/xaml-attached-layout-activityfeedscreenshot.png)

Der Code unten durchläuft schrittweise dieses potenzielle Beispiel für eine benutzerdefinierte virtualisierende Benutzeroberfläche, um den allgemeinen Ansatz zu veranschaulichen, den Sie für ein **Datenlayout** wählen können.

<table>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> in Aktion in diesem Beispiellayout zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>Implementierung

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(Optional) Verwalten der Zuordnung von Elementen zu UIElement

Standardmäßig unterhält der [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) eine Zuordnung zwischen den realisierten Elementen und dem Index in der Datenquelle, die sie darstellen.  Ein Layout kann diese Zuordnung auf Wunsch selbst verwalten, indem es beim Abrufen eines Elements mithilfe der [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat)-Methode immer die Option [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) anfordert, wodurch das Standardverhalten zum Recycling verhindert wird.  Dies kann für ein Layout beispielsweise dann sinnvoll sein, wenn es nur in Verbindung mit einem auf eine Richtung eingeschränkten Bildlauf verwendet wird und die berücksichtigten Elemente stets aufeinander folgen (d. h. die Kenntnis des Index des ersten und des letzten Elements ausreichend ist, um alle zu realisierenden Elemente zu kennen).

#### <a name="example-xbox-activity-feed-measure"></a>Beispiel: Xbox-Aktivitätsfeed: Messung

Der Codeausschnitt unten zeigt die zusätzliche Logik, die MeasureOverride im vorherigen Beispiel hinzugefügt werden könnte, um die Zuordnung zu verwalten.

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>Inhaltsabhängige virtualisierende Layouts

Wenn Sie den UI-Inhalt für ein Element zuerst messen müssen, um seine genaue Größe zu ermitteln, handelt es sich um ein **inhaltsabhängiges Layout**.  Sie können sich dies auch als ein Layout vorstellen, bei dem jedes Element seine eigene Größe festlegen muss, statt dass das Layout dem Element seine Größe zuweist. Virtualisierende Layouts dieser Kategorie sind anspruchsvoller.

> [!NOTE]
> Inhaltsabhängige Layouts stören die Datenvirtualisierung nicht (oder sollten es zumindest nicht tun).

### <a name="estimations"></a>Schätzungen

Inhaltsabhängige Layouts bauen auf Schätzungen auf, um sowohl die Größe von nicht realisiertem Inhalt als auch die Position des realisierten Inhalts zu erraten. Da sich diese Schätzungen ändern, verschieben sich ständig die Positionen der realisierten Inhalte innerhalb des Scrollbereichs. Dies kann zu einer sehr frustrierenden und holprigen Benutzererfahrung führen, wenn es nicht abgemildert wird. Die potenziellen Probleme und Abhilfen werden hier erörtert.

> [!NOTE]
> Bei Datenlayouts, die jedes Element berücksichtigen und die genaue Größe und Position aller Elemente kennen, seien sie realisiert oder nicht, können diese Probleme völlig vermieden werden.

**Scrollverankerung**

XAML bietet einen Mechanismus zum Abmildern plötzlicher Verschiebungen des Viewports, indem die Bildlaufsteuerelemente durch Implementieren der [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)-Schnittstelle [Scrollverankerung](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) unterstützen. Wenn der Benutzer den Inhalt bearbeitet, wählt das Bildlaufsteuerelement fortlaufend ein Element aus der Menge der Kandidaten aus, für die Nachverfolgung festgelegt wurde. Wenn sich die Position des Ankerelements während des Layoutprozesses verschiebt, verschiebt das Bildlaufsteuerelement automatisch seinen Viewport, um den Viewport beizubehalten.

Der Wert des [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex), der dem Layout übergeben wird, kann das aktuell ausgewählte und vom Bildlaufsteuerelement gewählte Ankerelement widerspiegeln. Alternativ, wenn ein Entwickler explizit anfordert, dass ein Element für einen Index mit der [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement)-Methode für den [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) realisiert wird, wird der betreffende Index im nächsten Layoutdurchlauf als [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) übergeben. Dadurch kann sich das Layout auf das wahrscheinliche Szenario vorbereiten, dass ein Entwickler ein Element realisiert und nachfolgend mithilfe der [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview)-Methode anfordert, dass das Element in der Ansicht platziert wird.

Der [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) ist der Index für das Element in der Datenquelle, die ein inhaltsabhängiges Layout zuerst positionieren sollte, wenn es die Position seiner Elemente untersucht. Er sollte als Ausgangspunkt für die Positionierung anderer realisierter Elemente dienen.

**Auswirkung auf Scrollleisten**

Wenn die Schätzungen des Layouts stark variieren, möglicherweise aufgrund großer Abweichungen in der Größe der Inhalte, kann die Position des Scrollleisten-Ziehpunkts trotz Scrollverankerung hin und her springen.  Dies kann sich für einen Benutzer holprig anfühlen, wenn der Ziehpunkt die Position seines Mauszeigers beim Ziehen nicht zu erfassen scheint.

Je genauer das Layout in seinen Schätzungen sein kann, desto weniger wahrscheinlich ist es, dass ein Benutzer ein Springen des Scrollleisten-Ziehpunkts bemerkt.

### <a name="layout-corrections"></a>Layoutkorrekturen

Ein inhaltsabhängiges Layout sollte darauf vorbereitet sein, seine Schätzung mit der Realität abzugleichen.  Wenn der Benutzer beispielsweise an den Anfang des Inhalts scrollt und das Layout das allererste Element realisiert, stellt es möglicherweise fest, dass die vorhergesagte Position relativ zu dem Element, von dem die Vorhersage ausging, zu seiner Darstellung an einer anderen Position als dem Ursprung (x:0, y:0) führen würde. Wenn dieser Fall eintritt, kann das Layout die [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin)-Eigenschaft verwenden, um die berechnete Position als neuen Layoutursprung festzulegen.  Das Endergebnis ist ähnlich wie bei der Scrollverankerung, bei der der Viewport des Bildlaufsteuerelements automatisch angepasst wird, um der Position des Inhalts gemäß den Angaben des Layouts Rechnung zu tragen.

![Korrigieren von LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Getrennte Viewports

Die von der [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)-Methode des Layouts zurückgegebene Größe stellt die beste Schätzung der Größe des Inhalts dar, die sich mit jedem nachfolgenden Layout ändern kann.  Während ein Benutzer scrollt, wird das Layout fortlaufend erneut mit einem aktualisierten [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) bewertet.

Wenn ein Benutzer den Scrollleisten-Ziehpunkt sehr schnell zieht, ist es aus der Perspektive des Layouts möglich, dass der Viewport scheinbar große Sprünge macht, bei denen die vorherige Position sich nicht mit der nun aktuellen Position überschneidet.  Dies hat seinen Grund in der prinzipbedingten Asynchronität des Bildlaufs. Es ist ferner möglich, dass eine App, die das Layout nutzt, die Darstellung eines aktuell nicht realisierten Elements anfordert, das der Schätzung nach außerhalb des vom Layout nachverfolgten aktuellen Bereichs liegt.

Wenn das Layout feststellt, dass seine Schätzung falsch ist und/oder eine unerwartete Verschiebung des Viewports feststellt, muss es seine Ausgangsposition neu ausrichten.  Die virtualisierenden Layouts, die als Teil der XAML-Steuerelemente zur Verfügung stehen, wurden als inhaltsabhängige Layouts entwickelt, da diese der Art der dargestellten Inhalte weniger Einschränkungen auferlegen.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Beispiel: Einfaches virtualisierendes Stapellayout für Elemente variabler Größe

Im folgenden Beispiel wird ein einfaches Stapellayout für Elemente variabler Größen dargestellt, für das Folgendes gilt:

* es unterstützt UI-Virtualisierung
* es verwendet Schätzungen, um die Größe der nicht realisierten Elemente zu raten
* es kann potenziell diskontinuierliche Verschiebungen des Viewports erkennen und
* es wendet Layoutkorrekturen an, um diese Verschiebungen zu berücksichtigen.

**Syntax: Markup**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**Codebehind: Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**Code: VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
