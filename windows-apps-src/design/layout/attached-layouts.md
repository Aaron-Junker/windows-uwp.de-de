---
Description: Sie können ein angefügtes Layout zur Verwendung mit Containern definieren, wie z. b. dem itemsrepeater-Steuerelement.
title: Attachedlayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dc23e86f85c5db3dd10c5cec152047be387d4513
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282293"
---
# <a name="attached-layouts"></a>Angefügte Layouts

Ein Container (z. b. Panel), der seine Layoutlogik an ein anderes Objekt delegiert, stützt das angefügte Layoutobjekt, um das Layoutverhalten für seine untergeordneten Elemente bereitzustellen.  Ein angefügtes Layoutmodell bietet Flexibilität für eine Anwendung, um das Layout von Elementen zur Laufzeit zu ändern oder um Aspekte des Layouts auf einfache Weise zwischen verschiedenen Teilen der Benutzeroberfläche zu teilen (z. b. Elemente in den Zeilen einer Tabelle, die in einer Spalte ausgerichtet sind).

In diesem Thema wird erläutert, was mit der Erstellung eines angefügten Layouts (virtualisieren und nicht virtualisieren), den Konzepten und Klassen, die Sie verstehen müssen, und den vor-und Nachteile, die Sie berücksichtigen müssen, bei der Entscheidung zwischen Ihnen zu berücksichtigen ist.

| **Abrufen der Windows-UI-Bibliothek** |
| - |
| Dieses Steuerelement ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für UWP-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Wichtige APIs:**

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [Layout](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [Nonvirtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [Virtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [Layoutcontext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [Nonvirtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [Virtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (Vorschau)

## <a name="key-concepts"></a>Wichtige Konzepte

Zum Durchführen des Layouts müssen für jedes Element zwei Fragen beantwortet werden:

1. Welche ***Größe*** hat dieses Element?

2. Wie lautet die ***Position*** dieses Elements?

Das Layoutsystem von XAML, das diese Fragen beantwortet, wird kurz im Rahmen der Erörterung von [benutzerdefinierten Panels](/windows/uwp/design/layout/custom-panels-overview)behandelt.

### <a name="containers-and-context"></a>Container und Kontext

Konzeptionell füllt das [Panel](/uwp/api/windows.ui.xaml.controls.panel) von XAML zwei wichtige Rollen im Framework aus:

1. Sie kann untergeordnete Elemente enthalten und stellt Verzweigungen in der Struktur von Elementen vor.
2. Es wendet eine bestimmte layoutstrategie auf diese untergeordneten Elemente an.

Aus diesem Grund ist ein Panel in XAML oft gleichbedeutend mit dem Layout, aber technisch gesehen bedeutet dies mehr als nur das Layout.

Der [itemsrepeater](/windows/uwp/design/controls-and-patterns/items-repeater) verhält sich auch wie Panel, aber im Gegensatz zum Bereich macht er keine Children-Eigenschaft verfügbar, die das programmgesteuerte Hinzufügen oder Entfernen von UIElement-untergeordneten Elementen ermöglicht.  Stattdessen wird die Lebensdauer der untergeordneten Elemente automatisch vom Framework verwaltet, damit Sie einer Auflistung von Datenelementen entspricht.  Obwohl Sie nicht vom Panel abgeleitet ist, verhält sie sich und wird vom Framework wie einem Panel behandelt.

> [!NOTE]
> Das [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) ist ein vom Panel abgeleiteter Container, der seine Logik an das angefügte [Layoutobjekt](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) delegiert.  Layoutpanel befindet sich in der *Vorschau* Phase und ist zurzeit nur in der *vorab* Version des WinUI-Pakets verfügbar.

#### <a name="containers"></a>Container

Konzeptionell ist [Panel](/uwp/api/windows.ui.xaml.controls.panel) ein Container von Elementen, der auch die Möglichkeit hat, Pixel für einen [Hintergrund](/uwp/api/windows.ui.xaml.controls.panel.background)zu Rendering.  Panels bieten eine Möglichkeit, gängige Layoutlogik in einem leicht zu verwendenden Paket zu kapseln.

Das Konzept des **angefügten Layouts** macht den Unterschied zwischen den beiden Rollen von Container und Layout klarer.  Wenn der Container seine Layoutlogik an ein anderes Objekt delegiert, wird dieses Objekt wie im folgenden Code Ausschnitt gezeigt als angefügtes Layout bezeichnet. Container, die von [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)erben (z. b. Layoutpanel), machen automatisch die allgemeinen Eigenschaften verfügbar, die Eingaben für den XAML-Layoutprozess bereitstellen (z. b. Height und Width).

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

Während des Layoutprozesses basiert der Container auf dem angeschlossenen *uniforgridlayout* , um seine untergeordneten Elemente zu messen und anzuordnen.

#### <a name="per-container-state"></a>Pro-Container-Status

Mit einem angefügten Layout kann eine einzelne Instanz des Layoutobjekte mit *vielen* Containern wie im folgenden Code Ausschnitt verknüpft werden. Daher darf Sie nicht vom Host Container abhängig sein oder ihn direkt referenzieren.  Zum Beispiel:

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

In *dieser Situation muss* das Unternehmen, das in der Layoutberechnung verwendet wird, und der Speicherort, an dem der Zustand gespeichert wird, sorgfältig in Erwägung gezogen werden, um das Layout für Elemente in einem Panel mit dem anderen zu beeinträchtigen  Dies wäre analog zu einem benutzerdefinierten Bereich, dessen messreoverride-und ArrangeOverride-Logik von den Werten der *statischen* Eigenschaften abhängig ist.

#### <a name="layoutcontext"></a>Layoutcontext

Der Zweck von [layoutcontext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) besteht darin, diese Herausforderungen zu bewältigen.  Das angefügte Layout bietet die Möglichkeit, mit dem Host Container zu interagieren, z. b. das Abrufen von untergeordneten Elementen, ohne eine direkte Abhängigkeit zwischen beiden zu erstellen. Der Kontext ermöglicht es dem Layout auch, jeden beliebigen Zustand zu speichern, der möglicherweise mit den untergeordneten Elementen des Containers verknüpft ist.

Einfache, nicht virtualisierende Layouts müssen häufig keinen Zustand aufrechterhalten, sodass es nicht zu Problemen wird. Ein komplexeres Layout, z. b. Raster, kann jedoch den Zustand zwischen dem Measure und dem Anordnungs Ansichts Zustand beibehalten, um das erneute Berechnen eines Werts zu vermeiden.

Bei der Virtualisierung von Layouts müssen *häufig* einige Zustände zwischen Measure und anordnen sowie zwischen iterativen layoutdurchläufen beibehalten werden.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Initialisieren und Aufheben der Initialisierung pro Container-Status

Wenn ein Layout an einen Container angefügt wird, wird seine [initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) -Methode aufgerufen und bietet die Möglichkeit, ein Objekt zum Speichern des Zustands zu initialisieren.

Wenn das Layout aus einem Container entfernt wird, wird auch die [uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) -Methode aufgerufen.  Dadurch erhält das Layout die Möglichkeit, jeden Zustand zu bereinigen, der diesem Container zugeordnet ist.

Das Zustands Objekt des Layouts kann mit der [layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) -Eigenschaft im Kontext gespeichert und aus dem Container abgerufen werden.

### <a name="ui-virtualization"></a>UI-Virtualisierung

Die UI-Virtualisierung bedeutet, dass das Erstellen eines UI-Objekts verzögert wird, bis _es benötigt_wird.  Es handelt sich um eine Leistungsoptimierung.  Für Szenarios ohne Bildlauf, die bestimmen, _wann erforderlich_ ist, können Sie auf einer beliebigen Anzahl von apps basieren, die APP-spezifisch sind.  In diesen Fällen sollten Apps die Verwendung von [x:Load](../../xaml-platform/x-load-attribute.md)in Erwägung gezogen werden. In Ihrem Layout ist keine besondere Handhabung erforderlich.

In Bild Lauf basierten Szenarios, wie z. b. einer Liste, hängt die Bestimmung, _bei Bedarf_ , häufig davon ab, dass Sie für einen Benutzer sichtbar ist. Dies hängt stark davon ab, wo es während des Layoutprozesses platziert wurde, und erfordert besondere Überlegungen.  Dieses Szenario ist der Schwerpunkt dieses Dokuments.

> [!NOTE]
> Obwohl es in diesem Dokument nicht behandelt wird, können die gleichen Funktionen, die die UI-Virtualisierung in scrollszenarien ermöglichen, in Szenarios ohne Bildlauf angewendet werden.  Beispielsweise ein datengesteuerte Symbolleisten-Steuerelement, das die Lebensdauer der Befehle verwaltet, die es anzeigt, und auf Änderungen des verfügbaren Speicherplatzes reagiert, indem Elemente zwischen einem sichtbaren Bereich und einem Überlauf Menü verschoben werden.

## <a name="getting-started"></a>Erste Schritte

Entscheiden Sie zunächst, ob das Layout, das Sie erstellen müssen, die UI-Virtualisierung unterstützen soll.

**Einige Dinge, die Sie beachten müssen...**

1. Nicht virtualisierende Layouts sind einfacher zu erstellen. Wenn die Anzahl der Elemente immer klein ist, wird die Erstellung eines nicht virtualisierenden Layouts empfohlen.
2. Die Plattform bietet eine Reihe angefügter Layouts, die mit [itemsrepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) und [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) zusammenarbeiten, um allgemeine Anforderungen abzudecken.  Machen Sie sich mit diesen vertraut, bevor Sie entscheiden, ein benutzerdefiniertes Layout zu definieren.
3. Das Virtualisieren von Layouts hat im Vergleich zu einem nicht virtualisierenden Layout immer zusätzliche CPU-und Arbeitsspeicher Kosten/-Komplexität/mehr Aufwand.  Als allgemeine Faustregel gilt: Wenn die untergeordneten Elemente, die das Layout verwalten muss, wahrscheinlich in einen Bereich passen, der 3X die Größe des Viewports ist, kommt es möglicherweise nicht viel zu einem virtualisierunglayout. Die 3X-Größe wird weiter unten in diesem Dokument ausführlicher erläutert. Dies ist jedoch auf die asynchrone Art des Bildlaufs unter Windows und deren Auswirkung auf die Virtualisierung zurückzuführen.

> [!TIP]
> Als Bezugspunkt gilt: die Standardeinstellungen für [ListView](/uwp/api/windows.ui.xaml.controls.listview) (und [itemsrepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)), dass die Wiederverwendung nicht beginnt, bis die Anzahl der Elemente ausreicht, um die 3X-Größe des aktuellen Viewports zu füllen.

**Auswählen des Basistyps**

![angefügte layoutanarchie](images/xaml-attached-layout-hierarchy.png)

Der [basislayouttyp](/uwp/api/microsoft.ui.xaml.controls.layout) verfügt über zwei abgeleitete Typen, die als Ausgangspunkt für die Erstellung eines angefügten Layouts fungieren:

1. [Nonvirtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [Virtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Nicht virtualisierungslayout

Der Ansatz für das Erstellen eines nicht virtualisierenden Layouts sollte allen Benutzern vertraut sein, die einen [benutzerdefinierten](/windows/uwp/design/layout/custom-panels-overview)Bereich erstellt haben.  Es gelten dieselben Konzepte.  Der Hauptunterschied besteht darin, dass ein [nonvirtualizinglayoutcontext-Objekt](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) verwendet wird, um [auf die Auflistung](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) der untergeordneten Elemente zuzugreifen, und das Layout kann den Zustand speichern.

1. Leiten Sie vom Basistyp [nonvirtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (anstelle von Panel) ab.
2. *(Optional)* Definieren Sie Abhängigkeits Eigenschaften, die bei einer Änderung das Layout für ungültig erklären.
3. _(**Neue**/optional)_ Initialisieren Sie alle Zustands Objekte, die vom Layout als Teil von [initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)benötigt werden. Stash Sie mit dem Host Container, indem Sie den [layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) verwenden, der mit dem Kontext bereitgestellt wird.
4. Überschreiben Sie die [measreoverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) -Methode, und nennen Sie die [Measure](/uwp/api/windows.ui.xaml.uielement.measure) -Methode für alle untergeordneten Elemente
5. Überschreiben der [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) und aufzurufen der [Anordnungs](/uwp/api/windows.ui.xaml.uielement.arrange) Methode für alle untergeordneten Elemente.
6. *(**Neue**/optional)* Bereinigen Sie einen gespeicherten Zustand als Teil von " [uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)".

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Beispiel: Ein einfaches Stapel Layout (Elemente variabler Größen)

![Mystacklayout](images/xaml-attached-layout-mystacklayout.png)

Im folgenden finden Sie ein sehr einfaches nicht Virtualisierungsstapel Layout von Elementen mit unterschiedlichen Größen. Es sind keine Eigenschaften zum Anpassen des Layoutverhaltens vorhanden. Die folgende Implementierung veranschaulicht, wie das Layout auf dem vom Container bereitgestellten Kontext Objekt basiert:

1. Anzahl der untergeordneten Elemente und
2. Greifen Sie über den Index auf jedes untergeordnete Element zu.

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

## <a name="virtualizing-layouts"></a>Virtualisieren von Layouts

Ähnlich wie bei einem nicht virtualisierenden Layout sind die Grundschritte für ein virtualisierungslayout identisch.  Die Komplexität liegt größtenteils darin, zu bestimmen, welche Elemente innerhalb des Viewports liegen und sollte realisiert werden.

1. Leiten Sie vom Basistyp [virtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)ab.
2. Optionale Definieren Sie die Abhängigkeits Eigenschaften, die bei einer Änderung das Layout für ungültig erklären.
3. Initialisieren Sie alle Zustands Objekte, die vom Layout als Teil von [initializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore)benötigt werden. Stash Sie mit dem Host Container, indem Sie den [layoutstate](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) verwenden, der mit dem Kontext bereitgestellt wird.
4. Überschreiben Sie die [measreoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) -Methode, und wenden Sie die [Measure](/uwp/api/windows.ui.xaml.uielement.measure) -Methode für jedes untergeordnete Element an, das
   1. Die [getorkreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) -Methode wird verwendet, um ein UIElement abzurufen, das vom Framework vorbereitet wurde (z. b. angewendete Daten Bindungen).
5. Überschreiben Sie die [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) -Methode, und wenden Sie die [Anordnungs](/uwp/api/windows.ui.xaml.uielement.arrange) Methode für jedes erkannte unter
6. Optionale Bereinigen Sie einen gespeicherten Zustand als Teil von " [uninitializeforcontextcore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore)".

> [!TIP]
> Der von der " [messreoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) " zurückgegebene Wert wird als Größe des virtualisierten Inhalts verwendet.

Beim Erstellen eines virtualisierenden Layouts sollten Sie zwei allgemeine Ansätze beachten.  Ob Sie eine oder die andere auswählen, hängt größtenteils davon ab, wie die Größe eines Elements bestimmt wird.  Wenn es ausreichend ist, den Index eines Elements im DataSet zu erkennen, oder die Daten selbst die endgültige Größe festgelegt haben, sollten wir Sie als **Daten abhängig**vorstellen.  Diese können einfacher erstellt werden.  Wenn jedoch die Größe für ein Element nur von der Erstellung und Messung der Benutzeroberfläche bestimmt werden soll, wird die Benutzeroberfläche als **Inhalts abhängig**angezeigt.  Diese sind komplexer.

### <a name="the-layout-process"></a>Der Layoutprozess

Unabhängig davon, ob Sie ein Daten-oder Inhalts abhängiges Layout erstellen, ist es wichtig, den Layoutprozess und die Auswirkung des asynchronen Bildlaufs von Windows zu verstehen.

Eine (über) vereinfachte Ansicht der Schritte, die vom Framework gestartet werden, um die Benutzeroberfläche auf dem Bildschirm anzuzeigen, ist Folgendes:

1. Das Markup wird analysiert.

2. Generiert eine Struktur von Elementen.

3. Führt eine layoutdurchführung aus.

4. Führt einen renderdurchlauf aus.

Bei der benutzeroberflächenvirtualisierung wird das Erstellen der Elemente, die normalerweise in Schritt 2 ausgeführt werden, verzögert oder vorzeitig beendet, sobald festgestellt wurde, dass genügend Inhalte zum Ausfüllen des Viewports erstellt wurden. Ein virtualisierender Container (z. b. itemsrepeater) ist für das angefügte Layout festgelegt, um diesen Prozess zu steuern. Das angefügte Layout wird mit einem [virtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) bereitgestellt, der die zusätzlichen Informationen, die für ein virtualisierungslayout erforderlich sind, enthält.

**Der realizationrect (z. b. Viewport)**

Das Scrollen unter Windows erfolgt asynchron mit dem UI-Thread. Er wird nicht durch das Layout des Frameworks gesteuert.  Stattdessen erfolgt die Interaktion und Bewegung im System-Compositor. Der Vorteil dieses Ansatzes besteht darin, dass das Schwenken von Inhalten immer mit 60 fps erfolgen kann.  Die Herausforderung ist jedoch, dass der "Viewport", wie im Layout dargestellt, möglicherweise etwas veraltet ist, relativ zu dem, was auf dem Bildschirm tatsächlich sichtbar ist. Wenn ein Benutzer schnell einen Bildlauf durchführt, kann dies die Geschwindigkeit des UI-Threads übersteigen, um neuen Inhalt zu generieren, und "Pan to Black". Aus diesem Grund ist es häufig erforderlich, dass ein virtualisierungslayout einen zusätzlichen Puffer mit vorbereiteten Elementen generiert, um einen Bereich auszufüllen, der größer als der Viewport ist. Wenn beim Scrollen unter schwerer Ladevorgang angezeigt wird, wird der Benutzer weiterhin Inhalte angezeigt.

![Realisierung Rect](images/xaml-attached-layout-realizationrect.png)

Da die Element Erstellung kostspielig ist, wird das angefügte Layout durch Virtualisieren von Containern (z. b. [itemsrepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) anfänglich mit einem " [realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) " bereitgestellt, das mit dem Viewport übereinstimmt. Während der Leerlaufzeit kann der Container den Puffer vorbereiteter Inhalte durch wiederholte Aufrufe des Layouts vergrößern, indem er eine immer größere ausführungsrect verwendet. Dieses Verhalten ist eine Leistungsoptimierung, die versucht, ein Gleichgewicht zwischen der schnellen Startzeit und einem guten Schwenk Verhalten zu erzielen. Die maximale Puffergröße, die der itemsrepeater generiert, wird durch die Eigenschaften [verticalcachelength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) und [horizontalcachelength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) gesteuert.

**Wieder verwenden von Elementen (Wiederverwendung)**

Beim Layout wird erwartet, dass die Elemente so groß werden, dass Sie jedes Mal, wenn Sie ausgeführt wird, die Größe der [realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) ausfüllen. Standardmäßig werden alle nicht verwendeten Elemente von [virtualizinglayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) am Ende jedes Layoutdurchlaufs wieder verwendet.

Der [virtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) , der als Teil von " [Mess reoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) " und " [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) " an das Layout übergeben wird, bietet die zusätzlichen Informationen, die ein virtualisierungslayout benötigt. Zu den am häufigsten verwendeten Dingen gehören die folgenden Möglichkeiten:

1. Fragen Sie die Anzahl der Elemente in den Daten ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)) ab.
2. Rufen Sie mithilfe der [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) -Methode ein bestimmtes Element ab.
3. Abrufen eines [realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) , das den Viewport und Puffer darstellt, den das Layout mit den erkannten Elementen füllen soll.
4. Fordern Sie das UIElement-Element für ein bestimmtes Element mit der [getorkreateelement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) -Methode an.

Das Anfordern eines Elements für einen bestimmten Index bewirkt, dass dieses Element für diesen Durchlauf des Layouts als "in Verwendung" markiert wird. Wenn das Element nicht bereits vorhanden ist, wird es erkannt und automatisch zur Verwendung vorbereitet (z. b. zum Auffüllen der UI-Struktur, die in einem DataTemplate definiert ist, zum Verarbeiten von Daten Bindungen usw.).  Andernfalls wird Sie aus einem Pool vorhandener Instanzen abgerufen.

Am Ende jedes Measures werden alle vorhandenen, erkannten Elemente, die nicht als "in Gebrauch" gekennzeichnet waren, automatisch als wieder verwendend angesehen, es sei denn, die Option " [suppressautorecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) " wurde verwendet, als das Element über die [ Getorkreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) -Methode. Das Framework verschiebt es automatisch in einen Papierkorb und stellt es zur Verfügung. Sie kann anschließend für die Verwendung durch einen anderen Container abgerufen werden. Das Framework versucht, dies zu vermeiden, wenn dies möglich ist, da bei der erneuten Verarbeitung eines Elements einige Kosten anfallen.

Wenn ein virtualisierungslayout am Anfang jedes Measures weiß, welche Elemente nicht mehr in die Erkenntnis Rect fallen, kann die Wiederverwendung optimiert werden. Anstatt auf das Standardverhalten des Frameworks zu vertrauen. Das Layout kann Elemente mithilfe der Methode " [recycleelement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) " vorab in den Papierkorb verschieben.  Wenn diese Methode aufgerufen wird, bevor neue Elemente angefordert werden, werden diese vorhandenen Elemente verfügbar, wenn das Layout später eine [getorcreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) -Anforderung für einen Index ausgibt, der nicht bereits einem Element zugeordnet ist.

Der virtualizinglayoutcontext bietet zwei zusätzliche Eigenschaften, die für Layoutautoren entworfen wurden, die ein Inhalts abhängiges Layout erstellen. Sie werden später ausführlicher erläutert.

1. Ein [Empfehlungs Empfehlungs Index](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) , der eine optionale _Eingabe_ für das Layout bereitstellt.
2. Ein [layoutorigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) , bei dem es sich um eine optionale _Ausgabe_ des Layouts handelt.

## <a name="data-dependent-virtualizing-layouts"></a>Daten abhängige virtualisierungslayouts

Ein virtualisierungslayout ist einfacher, wenn Sie wissen, welche Größe jedes Element aufweisen sollte, ohne dass der anzuzeigende Inhalt gemessen werden muss.  In diesem Dokument bezeichnen wir einfach diese Kategorie der virtualisierungslayouts als **Daten Layouts** , da Sie in der Regel die Daten überprüfen.  Basierend auf den Daten kann eine APP eine visuelle Darstellung mit einer bekannten Größe auswählen. Dies ist möglicherweise darauf zurückzuführen, dass Ihr Teil der Daten oder zuvor vom Entwurf bestimmt wurde.

Der allgemeine Ansatz ist das Layout für Folgendes:

1. Berechnen Sie eine Größe und Position jedes Elements.
2. Als Teil von " [Mess reoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride)":
   1. Verwenden Sie das Element " [realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) ", um zu bestimmen, welche Elemente im Viewport angezeigt werden sollen.
   2. Rufen Sie das UIElement ab, das das Element mit der [getorkreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) -Methode darstellen soll.
   3. [Messen](/uwp/api/windows.ui.xaml.uielement.measure) Sie das UIElement mit der vorab berechneten Größe.
3. [Ordnen](/uwp/api/windows.ui.xaml.uielement.arrange) Sie als Teil von [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride)jedes erkannte UIElement mit der Voraus berechneten Position an.

> [!NOTE]
> Ein datenlayoutansatz ist häufig mit der _Datenvirtualisierung_nicht kompatibel.  Insbesondere, wenn die einzigen Daten, die in den Arbeitsspeicher geladen werden, die Daten sind, die für den Benutzer sichtbar sind.  Die Datenvirtualisierung verweist nicht auf ein verzögertes oder inkrementelles Laden von Daten, wenn ein Benutzer einen Bildlauf nach unten durchführt  Vielmehr bezieht sich dies auf den Fall, dass Elemente aus dem Arbeitsspeicher freigegeben werden, wenn Sie aus der Ansicht heraus scrollen.  Wenn ein Datenlayout vorhanden ist, bei dem jedes Datenelement als Teil eines Daten Layouts überprüft wird, würde die Datenvirtualisierung nicht wie erwartet funktionieren.  Eine Ausnahme ist ein Layout wie das uniforgridlayout, bei dem davon ausgegangen wird, dass alles die gleiche Größe hat.

> [!TIP]
> Wenn Sie ein benutzerdefiniertes Steuerelement für eine Steuerelement Bibliothek erstellen, die von anderen in einer Vielzahl von Situationen verwendet wird, ist ein Datenlayout möglicherweise keine Option für Sie.

### <a name="example-xbox-activity-feed-layout"></a>Beispiel: Layout des Xbox-Aktivitäts Feeds

Die Benutzeroberfläche für den Xbox-Aktivitäts Feed verwendet ein sich wiederholendes Muster, bei dem jede Zeile über eine breite Kachel verfügt, gefolgt von zwei schmalen Kacheln, die in der nachfolgenden Zeile invertiert werden. In diesem Layout ist die Größe für jedes Element eine Funktion der Position des Elements im DataSet und der bekannten Größe für die Kacheln (Wide vs Narrow).

![Xbox-Aktivitäts Feed](images/xaml-attached-layout-activityfeedscreenshot.png)

Der folgende Code zeigt, was eine benutzerdefinierte virtualisierungsschnittstelle für den Aktivitäts Feed sein könnte, um den allgemeinen Ansatz zu veranschaulichen, den Sie für ein **Datenlayout**benötigen.

<table>
<td>
    <p>Wenn Sie die Katalog-App für <strong style="font-weight: semi-bold">XAML</strong> -Steuerelemente installiert haben, klicken Sie hier, um die APP zu öffnen, und sehen Sie sich das <a href="xamlcontrolsgallery:/item/ItemsRepeater">itemsrepeater</a> in Aktion mit diesem Beispiel Layout an.</p>
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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>Optionale Verwalten des Elements mit der UIElement-Zuordnung

Standardmäßig verwaltet [virtualizinglayoutcontext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) eine Zuordnung zwischen den erkannten Elementen und dem Index in der Datenquelle, die Sie darstellen.  Ein Layout kann diese Zuordnung selbst verwalten, indem beim Abrufen eines Elements über die [getorkreateelementat](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) -Methode immer die Option zum [suppressautorecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) angefordert wird, die das Standardverhalten für die automatische Wiederverwendung verhindert.  Dies kann sich z. b. als Layout erweisen, wenn es nur dann verwendet wird, wenn der Bildlauf auf eine Richtung beschränkt ist und die Elemente, die es berücksichtigt, immer zusammenhängend sind (d. h. das wissen, dass der Index des ersten und des letzten Elements genug ist, um alle Elemente zu kennen, die eine lisiert).

#### <a name="example-xbox-activity-feed-measure"></a>Beispiel: Xbox-Aktivitäts Feed-Measure

Der folgende Code Ausschnitt zeigt die zusätzliche Logik, die der Messung reoverride im vorherigen Beispiel hinzugefügt werden konnte, um die Zuordnung zu verwalten.

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

## <a name="content-dependent-virtualizing-layouts"></a>Inhalts abhängige virtualisierungslayouts

Wenn Sie den Inhalt der Benutzeroberfläche für ein Element zuerst messen müssen, um seine genaue Größe zu ermitteln, ist es ein **Inhalts abhängiges Layout**.  Sie können sich das Layout auch als Layout vorstellen, bei dem jedes Element seine Größe selbst überschreiten muss, anstatt das Layout, das dem Element seine Größe mitteilt. Das Virtualisieren von Layouts, die in diese Kategorie fallen, ist stärker betroffen.

> [!NOTE]
> Inhalts abhängige Layouts sollten die Datenvirtualisierung nicht stören.

### <a name="estimations"></a>Schätzungen

Inhalts abhängige Layouts basieren auf der Schätzung, um sowohl die Größe des nicht erkannten Inhalts als auch die Position des realisierten Inhalts zu erraten. Wenn sich diese Schätzungen ändern, verschiebt der erkannte Inhalt regelmäßig Positionen innerhalb des Bild lauffähigen Bereichs. Dies kann zu einer sehr frustrierenden und jarringbenutzerbenutzerleistung führen, wenn diese nicht verringert werden. Die möglichen Probleme und entschärfungen werden hier erläutert.

> [!NOTE]
> Daten Layouts, die jedes Element berücksichtigen und die genaue Größe aller Elemente ermitteln, realisiert werden, und ihre Positionen können diese Probleme vollständig vermeiden.

**Scrollverankerung**

XAML bietet einen Mechanismus, um plötzliche viewportverschiebungen zu verringern, indem scrollsteuerelemente die [scrollverankerung](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) durch Implementieren der [iscrollanchorpovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) -Schnittstelle unterstützen. Wenn der Benutzer den Inhalt ändert, wählt das scrollsteuer Element kontinuierlich ein Element aus der Gruppe von Kandidaten aus, die für die Nachverfolgung angemeldet wurden. Wenn sich die Position des Anker Elements während des Layouts verschiebt, verschiebt das scrollsteuerelement seinen Viewport automatisch, um den Viewport beizubehalten.

Der für das Layout angegebene Wert von " [Empfehlungs](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) Name" kann das aktuell ausgewählte Anker Element widerspiegeln, das von dem scrollsteuerelement ausgewählt wurde. Wenn ein Entwickler explizit anfordert, dass ein Element für einen Index mit der [getorkreateelement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) -Methode für das [itemsrepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)-Element realisiert werden soll, wird dieser Index beim nächsten Layoutdurchlauf als [Empfehlungs Index](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) angegeben. Dadurch kann das Layout für das wahrscheinliche Szenario vorbereitet werden, in dem ein Entwickler ein Element erkennt, und anschließend die Anzeige über die [startbringintoview](/uwp/api/windows.ui.xaml.uielement.startbringintoview) -Methode anfordern.

[Empfehlungs Index Index](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) ist der Index für das Element in der Datenquelle, das von einem Inhalts abhängigen Layout zuerst positioniert werden soll, wenn die Position der Elemente geschätzt wird. Er sollte als Ausgangspunkt für die Positionierung anderer der erkannten Elemente dienen.

**Auswirkung auf Scrollleisten**

Auch bei der Bild Lauf Verankerung, wenn die Schätzungen des Layouts stark variieren, vielleicht aufgrund bedeutender Variationen der Inhalts Größe, wird die Position des Zieh Punkts für die Scrollleiste möglicherweise angezeigt.  Dies kann für einen Benutzer der Fall sein, wenn der Ziehpunkt nicht angezeigt wird, um die Position des Mauszeigers beim Ziehen zu verfolgen.

Umso genauer kann das Layout in seinen Schätzungen sein, und desto wahrscheinlicher ist es, dass ein Benutzer den Ziehpunkt der Scrollleiste einspringt.

### <a name="layout-corrections"></a>Layoutkorrekturen

Ein Inhalts abhängiges Layout sollte darauf vorbereitet sein, seine Schätzung mit der Realität zu rationalisieren.  Wenn z. b. der Benutzer einen Bildlauf zum Anfang des Inhalts durchführt und das Layout das erste Element erkennt, kann es vorkommen, dass die erwartete Position des Elements relativ zu dem Element, von dem es gestartet wurde, an einer anderen Stelle als der Ursprung von (x:0 , y:0). Wenn dies auftritt, kann das Layout die [layoutorigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) -Eigenschaft verwenden, um die Position festzulegen, die als neuer layoutursprung berechnet wird.  Das Ergebnis ist vergleichbar mit der scrollverankerung, bei der der Viewport des scrollsteuer Elements automatisch so angepasst wird, dass die vom Layout gemeldete Position des Inhalts angepasst wird.

![Korrigieren von layoutorigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Getrennte Viewports

Die von der [Mess reoverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) -Methode des Layouts zurückgegebene Größe stellt die beste Schätzung bei der Größe des Inhalts dar, die sich mit jedem aufeinander folgenden Layout ändern kann.  Wenn ein Benutzer einen Bildlauf durchführt, wird das Layout ständig mit einem aktualisierten [realizationrect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect)erneut ausgewertet.

Wenn ein Benutzer den Ziehpunkt sehr schnell zieht, dann ist es möglich, dass der Viewport von der Perspektive des Layouts aus angezeigt wird, um große Sprünge zu erstellen, bei denen sich die vorherige Position nicht mit der aktuellen Position überschneidet.  Dies liegt an der asynchronen Art des Bildlaufs. Es ist auch möglich, dass eine APP, die das Layout beansprucht, anfordert, dass ein Element für ein Element, das derzeit nicht realisiert ist, angezeigt wird, und dass es sich außerhalb des aktuellen Bereichs befindet, der vom Layout nachverfolgt wird.

Wenn das Layout feststellt, dass der Schätzwert nicht korrekt ist und/oder eine unerwartete viewportverschiebung feststellt, muss er seine Anfangsposition neu ausrichten.  Die virtualisierungslayouts, die als Teil der XAML-Steuerelemente ausgeliefert werden, werden als Inhalts abhängige Layouts entwickelt, da Sie weniger Einschränkungen hinsichtlich der Art des angezeigten Inhalts haben.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Beispiel: Einfaches virtualisieren des Stapel Layouts für Elemente variabler Größen

Im folgenden Beispiel wird ein einfaches Stapel Layout für Elemente variabler Größen veranschaulicht:

* unterstützt die UI-Virtualisierung.
* verwendet Schätzungen, um die Größe der nicht erkannten Elemente zu erraten.
* Beachten Sie die möglichen diskontinuierlichen Ansichts Verschiebungen, und
* wendet Layoutkorrekturen an, um diese Verschiebungen zu berücksichtigen.

**Syntax: Markup @ no__t-0

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

**codebehind: Main. cs @ no__t-0

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

**code: Virtualizingstacklayout. cs @ no__t-0

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
