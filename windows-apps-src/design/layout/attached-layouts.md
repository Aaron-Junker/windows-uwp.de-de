---
Description: Sie können eine angefügte Layouts für die Verwendung mit Containern, z. B. das ItemsRepeater Steuerelement definieren.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6ff73b13acb5f5970bb79755b0bf5706fb12545a
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913990"
---
# <a name="attached-layouts"></a>Angefügte Layouts

Ein Container (z. B. Bereich), der die Layout-Logik in ein anderes Objekt delegiert basiert auf dem angefügten Layoutobjekt, das Layoutverhalten für seine untergeordneten Elemente bereitstellen.  Eine angefügte Layout-Modell bietet Flexibilität für eine Anwendung, das Layout der Elemente zur Laufzeit zu ändern oder mehr ganz einfach zwischen verschiedenen Teilen der Benutzeroberfläche (z. B. Elemente in den Zeilen einer Tabelle, die angezeigt werden, innerhalb einer Spalte ausgerichtet werden sollen) von layoutaspekten freigeben.

In diesem Thema geht es um welcher Aufwand erforderlich ist, erstellen Sie eine angefügte Layout (virtualisierenden und nicht virtualisiert), die Konzepte und die Klassen, die Sie benötigen, um zu verstehen, und die vor-und Nachteile Sie müssen bei der Entscheidung zwischen ihnen zu berücksichtigen.

| **Abrufen der Windows-UI-Bibliothek** |
| - |
| Dieses Steuerelement ist Bestandteil der Windows-UI-Bibliothek, eines NuGet-Pakets, die neuen Steuerelemente und Funktionen der Benutzeroberfläche für UWP-apps enthält. Weitere Informationen einschließlich installationsanweisungen finden Sie in der [Übersicht über die Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/). |

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

Ausführen von Layout erfordert, dass für jedes Element zwei Fragen beantwortet werden:

1. Was ***Größe*** dieses Elements werden?

2. Was ist die ***Position*** dieses Elements werden?

Der XAML-Layout-System, das diese Fragen beantwortet, wird kurz behandelt, als Teil der Diskussion der [benutzerdefinierte Bereiche](/windows/uwp/design/layout/custom-panels-overview).

### <a name="containers-and-context"></a>Container und Kontext

Im Prinzip XAMLs [Bereich](/uwp/api/windows.ui.xaml.controls.panel) zwei wichtige Rollen im Framework füllt:

1. Sie können untergeordnete Elemente enthalten und stellt die Verzweigungen in der Struktur von Elementen.
2. Eine bestimmtes Layout-Strategie für die untergeordneten Elemente angewendet.

Aus diesem Grund wird ein Bereich in XAML wurde häufig gleichbedeutend mit dem Layout, aber die technisch gesehen, ist mehr als nur-Layout.

Die [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater) auch verhält sich wie der Bereich, aber im Gegensatz zu den Bereich, es macht Children-Eigenschaft, die programmgesteuert hinzufügen oder Entfernen von "UIElement" untergeordnete Elemente ermöglichen.  Stattdessen werden die Lebensdauer der untergeordneten Elemente automatisch durch das Framework zur Anpassung an eine Auflistung von Datenelementen verwaltet.  Obwohl es nicht von Panel abgeleitet ist, verhält sich und wird vom Framework wie ein Bereich behandelt.

> [!NOTE]
> Die [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) ist ein Container, abgeleitet aus dem Bereich, der seine Logik für die angefügte delegiert [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) Objekt.  LayoutPanel befindet sich im *Vorschau* erhältlich und zurzeit nur in der *Vorabversion* des Pakets WinUI gelöscht.

#### <a name="containers"></a>Container

Im Prinzip [Bereich](/uwp/api/windows.ui.xaml.controls.panel) ist ein Container für Elemente, die auch die Möglichkeit zum Rendern der Pixel für ein [Hintergrund](/uwp/api/windows.ui.xaml.controls.panel.background).  Bereiche bieten eine Möglichkeit, allgemeine Layoutlogik in einem benutzerfreundlichen-Paket zu kapseln.

Das Konzept der **angefügt Layout** macht den Unterschied zwischen den beiden Rollen des Containers und das Layout klarer.  Wenn der Container die Layout-Logik in ein anderes Objekt delegiert würden wir das Objekt das angefügte Layout aufrufen, wie im folgenden Codeausschnitt zu sehen. Container, die von erben ["FrameworkElement"](/uwp/api/windows.ui.xaml.frameworkelement), z. B. das LayoutPanel, automatisch verfügbar zu machen die allgemeinen Eigenschaften, die Eingabe XAMLs-Layout-Prozess (z. B. Höhe und Breite) bereitstellen.

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

Während des Prozesses Layout der Container basiert, auf die angefügte *UniformGridLayout* zu messen und seine untergeordneten Elemente anzuordnen.

#### <a name="per-container-state"></a>Pro Container-Status

Mit einem angefügten Layout, eine einzelne Instanz des Objekts Layout kann zugeordnet werden *viele* -Container wie im folgenden Codeausschnitt; aus diesem Grund Es dürfen keine hängen oder direkt auf den Hostcontainer zu verweisen.  Zum Beispiel:

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

In dieser Situation *ExampleLayout* muss sorgfältig bedacht werden den Status, die sie in seiner layoutberechnung und dem Speicherort, Status verwendet, um die Auswirkungen auf das Layout für Elemente in einem Bereich mit den anderen zu vermeiden.  Es wäre analog zu der ein benutzerdefiniertes Panel, deren MeasureOverride- und ArrangeOverride-Logik, die auf den Werten abhängig ist, dessen *statische* Eigenschaften.

#### <a name="layoutcontext"></a>LayoutContext

Der Zweck der [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) ist für den Umgang mit diesen Herausforderungen.  Es bietet die angefügten Layout die Fähigkeit, mit dem Hostcontainer, wie das Abrufen von untergeordneten Elementen ohne direkte Abhängigkeit zwischen den beiden interagieren. Der Kontext ermöglicht auch das Layout, um alle dafür erforderlichen Zustand zu speichern, die von untergeordneten Elementen des Containers bezogen werden kann.

Einfache, nicht virtualisiert Layouts ist oftmals nicht erforderlich, zu einem beliebigen Zustand, sodass es nicht verwalten. Ein komplexeres Layout, wie z. B. Raster kann jedoch zum Beibehalten des Status zwischen dem Measure, und ordnen Sie aufrufen, um zu vermeiden, durch das Neuberechnen eines Werts auswählen.

Virtualisieren von Layouts *häufig* müssen einige Zustand zwischen sowohl für die Measure beibehalten und auch zwischen iterative layoutdurchläufe anordnen.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Initialisieren und Deinitialisierung pro Container-Status

Wenn ein Layout mit einem Container angefügt ist seine [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) Methode wird aufgerufen, und bietet die Möglichkeit, ein Objekt zum Speichern von Status zu initialisieren.

Auf ähnliche Weise, wenn das Layout entfernt wird aus einem Container, der [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) aufgerufene Methode.  Dadurch werden dem Layout Gelegenheit zum Bereinigen von einem beliebigen Zustand, die sie mit dem betreffenden Container verbunden hätten.

Das Layout des Zustandsobjekt, das mit gespeichert und abgerufen werden, aus dem Container mit werden die [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) Eigenschaft im Kontext.

### <a name="ui-virtualization"></a>UI-Virtualisierung

UI-Virtualisierung bedeutet, dass die Erstellung eines UI-Objekts, bis zu verzögern _Wenn es benötigt_.  Es ist eine leistungsoptimierung.  Für Szenarien ohne Bildlauf zum Bestimmen der _bei Bedarf_ basiert möglicherweise auf eine beliebige Anzahl der Aufgaben, die app-spezifisch sind.  In diesen Fällen apps sollten mit den [X: Load](../../xaml-platform/x-load-attribute.md). Es erfordert keine spezielle Behandlung im Layout.

In Szenarien wie z. B. eine Liste durchführen eines Bildlaufs basierende bestimmen _bei Bedarf_ basiert häufig auf "werden angezeigt, zu einem Benutzer" die hängt in hohem Maß, in dem sie, während der Layoutprozess abgelegt wurde und erfordert besondere Überlegungen.  Dieses Szenario ist ein Schwerpunkt für dieses Dokument.

> [!NOTE]
> Obwohl in diesem Dokument nicht behandelt, konnten die gleichen Funktionen, die die UI-Virtualisierung Durchführen eines Bildlaufs Szenarien ermöglichen in Szenarien ohne Bildlauf angewendet werden.  Z. B. eine datengesteuerte Symbolleisten-Steuerelement, das die Lebensdauer der Befehle verwaltet werden, er bietet und reagiert auf Änderungen in den verfügbaren Speicherplatz, indem die Wiederverwendung / Verschieben von Elementen zwischen einer sichtbaren Bereich und ein Überlaufmenü.

## <a name="getting-started"></a>Erste Schritte

Zunächst entscheiden Sie, ob das Layout, die, das Sie erstellen müssen, die UI-Virtualisierung unterstützen soll.

**Einige Dinge zu bedenken...**

1. Layouts nicht virtualisiert sind einfacher zu erstellen. Wenn die Anzahl der Elemente immer kleine werden wird empfohlen, dann erstellen ein Layout nicht virtualisiert.
2. Die Plattform bietet eine Reihe von angefügten Layouts, mit denen Arbeit mit der [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) und [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) muss häufig zu behandeln.  Machen Sie sich mit den bevor Sie sich entscheiden, müssen Sie ein benutzerdefiniertes Layout zu definieren.
3. Virtualisierenden Layouts verfügen immer über einige zusätzliche CPU und Arbeitsspeicher/Komplexität/Kostenaufwand im Vergleich zu einem Layout nicht virtualisiert.  Wie wahrscheinlich eine allgemeine Faustregel, wenn die untergeordneten Elemente des Layouts verwalten muss in einem Bereich passt, die 3 x die Größe des Viewports, wird möglicherweise nicht Ihr Zuwachs vornehmlich aus einem virtualisierenden Layout. Die Größe 3 X wird weiter unten in diesem Dokument ausführlicher erläutert, ist jedoch aufgrund der asynchronen Natur des Bildlaufs auf Windows und seine Auswirkungen auf die Virtualisierung.

> [!TIP]
> Als Ausgangspunkt des Verweises, die Standardeinstellungen für die [ListView](/uwp/api/windows.ui.xaml.controls.listview) (und [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) sind, dass die Wiederverwendung erst beginnen, nicht die Anzahl der Elemente aus, um 3 x die Größe des aktuellen Viewports gefüllt werden.

**Wählen Sie Ihr Basistyp**

![angefügte Layout-Hierarchie](images/xaml-attached-layout-hierarchy.png)

Die Basis [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) Typ verfügt über zwei abgeleitete Typen, die als Startpunkt für das Erstellen ein Layout für die angefügten dienen:

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Nicht virtualisiert Layout

Die Vorgehensweise zum Erstellen eines Layouts nicht virtualisiert vertraut für jeden, der Erstellung einer [benutzerdefinierte Panel](/windows/uwp/design/layout/custom-panels-overview).  Die gleichen Konzepte gelten.  Der Hauptunterschied besteht darin, die eine [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) wird verwendet, für den Zugriff auf die [untergeordneten](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) Auflistung und Layout, den Status speichern können.

1. Der Basistyp abgeleitet [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (anstelle von Bereich).
2. *(Optional)*  Abhängigkeitseigenschaften definieren, die bei geändert, wird das Layout ungültig.
3. _(**Neu**/Optional)_ initialisieren alle Zustandsobjekt, das das Layout erforderlichen als Teil der [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Stash es mit dem Hostcontainer mithilfe der [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) bereitgestellt, mit dem Kontext.
4. Überschreiben der [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) , und rufen Sie die [Measure](/uwp/api/windows.ui.xaml.uielement.measure) Methode für alle untergeordneten Elemente.
5. Überschreiben der [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) , und rufen Sie die [anordnen](/uwp/api/windows.ui.xaml.uielement.arrange) Methode für alle untergeordneten Elemente.
6. *(**Neu**/Optional)* bereinigen gespeicherte Zustände als Teil der [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Beispiel: Eine einfache Stapellayout (ausgibt, deren Höhe der Elemente)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

Hier ist ein sehr einfaches-Virtualisierung von Stack Layout von Elementen mit unterschiedlicher Größe. Es fehlen alle Eigenschaften, um das Layout Verhalten anzupassen. Die nachfolgende Implementierung veranschaulicht, wie das Layout der Context-Objekt, das vom Container zum bereitgestellten abhängt:

1. Rufen Sie die Anzahl der untergeordneten Elemente und
2. Zugriff auf jedes untergeordnete Element anhand des Indexes.

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

Ähnlich wie bei einem Layout nicht virtualisiert werden sind die allgemeinen Schritte für ein virtualisierenden Layout identisch.  Die Komplexität ist größtenteils in bestimmen, welche Elemente innerhalb des Viewports fällt und realisiert werden soll.

1. Der Basistyp abgeleitet [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout).
2. (Optional) Definieren Sie Ihre Abhängigkeitseigenschaften bei geändert, wird das Layout ungültig.
3. Initialisieren Sie alle Zustandsobjekt, das als Teil des durch das Layout erforderlich ist der [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Stash es mit dem Hostcontainer mithilfe der [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) bereitgestellt, mit dem Kontext.
4. Überschreiben der [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) , und rufen Sie die [Measure](/uwp/api/windows.ui.xaml.uielement.measure) -Methode für jedes untergeordnete Element, das realisiert werden soll.
   1. Die [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) Methode wird verwendet, um eine "UIElement" abzurufen, die durch das Framework (z. B. datenbindungen angewendet) vorbereitet wurde.
5. Überschreiben der [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) , und rufen Sie die [anordnen](/uwp/api/windows.ui.xaml.uielement.arrange) -Methode für jedes erkannte untergeordnete Element.
6. (Optional) Bereinigen Sie alle einen gespeicherten Status aufweist als Teil der [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

> [!TIP]
> Der Rückgabewert von der [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) als die Größe des virtualisierten Inhalts verwendet wird.

Es gibt zwei allgemeine Vorgehensweisen beim Erstellen eines Layouts virtualisierenden zu berücksichtigen.  Angibt, ob wählen Sie eines dieser Zuordnungsverfahren größtenteils hängt von "wie Sie die Größe eines Elements bestimmt".  Wenn die genug sein, um den Index eines Elements in das Dataset oder die Daten selbst kennen ihre letztendliche Größe bestimmt ist, würden wir betrachten es **datenabhängige**.  Dies sind einfacher zu erstellen.  Wenn Sie jedoch die einzige Möglichkeit zum Bestimmen der Größe für ein Element erstellt wird und die Benutzeroberfläche zu messen, und klicken Sie dann wir es also ist **Inhalt abhängiges**.  Diese sind komplexer.

### <a name="the-layout-process"></a>Der Layoutvorgang

Ob Sie eine Daten- oder Inhalt abhängiges Layout erstellen, ist es wichtig zu verstehen, der Layoutprozess und die Auswirkungen des Bildlaufs an asynchronen Windows.

Eine (Failover) vereinfachte Ansicht der Schritte, die durch das Start-Framework für die Anzeige der Benutzeroberfläche auf dem Bildschirm darstellt:

1. Das Markup analysiert.

2. Generiert eine Struktur von Elementen an.

3. Führt eine Layoutphase an.

4. Führt eine Renderings aus.

Mit UI-Virtualisierung, Erstellen von Elementen, die normalerweise ausgeführt werden würden in Schritt 2 wird verzögert, oder hat vorzeitig beendet, sobald die wurde festgestellt, dass ausreichend der Inhalt erstellt wurde zum Ausfüllen des Viewports. Virtualisierender Container (z. B. ItemsRepeater) verzögert das angefügte Layout, um diesen Prozess zu steuern. Es bietet die angefügten Layout mit einer [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) , die die zusätzliche Informationen, die ein virtualisierenden Layout angezeigt.

**Die RealizationRect (d. h. Viewport)**

Scrollen auf Windows erfolgt asynchron an den UI-Thread. Es wird nicht durch die Framework Layout gesteuert.  Stattdessen die Interaktion und die datenverschiebung tritt auf, in dem System Compositor. Der Vorteil dieses Ansatzes ist, dass Inhalt Schwenken mit 60 fps immer ausgeführt werden kann.  Die Herausforderung ist jedoch, dass "Viewport", wie Sie sehen das Layout, möglicherweise etwas veraltet ist relativ zum was tatsächlich auf dem Bildschirm angezeigt wird. Wenn ein Benutzer schnell einen Bildlauf durchführt, können sie die Geschwindigkeit des UI-Threads zum Generieren von neuen Inhalten und "Schwenken auf Schwarz festgelegt," überholt haben. Aus diesem Grund ist es häufig erforderlich, für ein virtualisierenden Layout einen zusätzlichen Puffer von vorbereiteten Elemente, die ausreichen, um einen größeren Bereich als der Viewport ausfüllen zu generieren. Wenn bei größerer Auslastung während des Bildlaufs des Benutzers weiterhin mit Inhalt angezeigt wird.

![Realisierung rect](images/xaml-attached-layout-realizationrect.png)

Virtualisieren von Containern, da elementerstellung teuer ist, (z. B. [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) erhalten Sie zunächst die angefügten Layout mit einem [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) entspricht den Viewport. Leerlaufzeit Anwachsen des Containers den Puffer mit vorbereiteten Inhalt durch wiederholte Aufrufe an das Layout mit einem immer größeren Realisierung Rect Dieses Verhalten ist eine leistungsoptimierung, die versucht, einen Mittelweg zwischen Schnellstart und schwenkansicht benutzerfreundlichkeit. Die maximale Puffergröße, die die ItemsRepeater generieren wird gesteuert, indem die [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) und [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) Eigenschaften.

**Erneut mithilfe von Elementen, die (Wiederverwenden von)**

Das Layout wird erwartet, dass die Größe und position der Elemente zum Ausfüllen der [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) jedes Mal ausgeführt. In der Standardeinstellung die [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) wird wiederverwendet, alle nicht verwendeten Elemente am Ende jeder Layoutdurchlauf.

Die [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) , an das Layout übergeben wird, als Teil der [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) und [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) erhalten Sie zusätzliche Informationen ein virtualisierenden Layout muss. Einige der am häufigsten verwendeten Dinge bereitgestellte sind die Möglichkeit zum:

1. Die Anzahl der Elemente in den Daten ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. Abrufen einen bestimmten, mit der [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) Methode.
3. Abrufen einer [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) , das den Anzeigebereich darstellt und als Puffer, der das Layout mit ausfüllen soll realisiert Elemente.
4. Fordern Sie das "UIElement" für ein bestimmtes Element mit dem [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) Methode.

Anfordern eines Elements für einen bestimmten Index wird dieses Element als "verwendet" markiert werden, übergeben Sie das Layout verursachen. Wenn das Element nicht bereits vorhanden ist, wird dann erkannt und automatisch vorbereitet, für die Verwendung (z. B. jedoch die Benutzeroberflächenautomatisierungs-Struktur, die in einem DataTemplate, Verarbeitung, Datenbindung, usw. definiert.).  Andernfalls wird sie aus einem Pool mit vorhandenen Instanzen abgerufen werden.

Am Ende der einzelnen Maßübergabe, alle vorhandenen erkannte Element, das nicht "in Verwendung" markiert war gilt automatisch für die erneute Verwendung zur Verfügung, wenn die Option zum [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) wurde verwendet, wenn das Element über abgerufen wurden die [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) Methode. Das Framework automatisch verschiebt ihn in einem Pool für die Wiederverwendung und macht sie verfügbar. Sie können anschließend für die Verwendung durch einen anderen Container abgerufen werden. Das Framework versucht, die dies nach Möglichkeit vermeiden, da einige Kosten im Zusammenhang mit neuzuordnung ein Element vorhanden ist.

Wenn ein virtualisierenden Layout am Anfang jedes Measure kennt die Elemente nicht mehr in der Erkenntnis Rect fallen werden können dann sie die erneute Verwendung optimieren. Statt auf die Framework Standardverhalten. Das Layout kann bereits im Vorfeld Elemente in den Papierkorb Pool verschieben, indem Sie mit der [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) Methode.  Diese Methode aufrufen, bevor Sie neue Elemente anfordern bewirkt, dass die vorhandenen Elemente verfügbar sein, wenn Sie das Layout später Probleme mit einem [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) Anforderung für einen Index, der nicht bereits einem Element zugeordnet ist.

Die VirtualizingLayoutContext bietet zwei zusätzliche Eigenschaften für Layoutautoren Erstellen eines Layouts Inhalt abhängig. Sie werden später ausführlicher beschrieben.

1. Ein [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) , bietet ein optionales _Eingabe_ Layout.
2. Ein [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) , ein optionales _Ausgabe_ des Layouts.

## <a name="data-dependent-virtualizing-layouts"></a>Datenabhängige virtualisierenden Layouts

Ein virtualisierenden Layout ist einfacher, wenn Sie wissen, was die Größe jedes Elements ohne messen Sie den Inhalt, der angezeigt werden soll.  In diesem Dokument werden wir einfach in dieser Kategorie des Layouts als Virtualisierung bezeichnen **Datenlayouts** , da sie in der Regel umfassen die Daten untersucht werden.  Anhand der Daten eine app eine visuelle Darstellung mit einer bekannten Größe – z. B. auswählen kann, da der Teil der Daten oder zuvor entwurfsbedingt ermittelt wurde.

Der allgemeine Ansatz ist für das Layout auf:

1. Berechnen Sie eine Größe und Position eines jeden Elements ein.
2. Als Teil der [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. Verwenden der [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) um zu bestimmen, welche Elemente innerhalb des Viewports angezeigt werden soll.
   2. Rufen Sie das "UIElement", die das Element mit darstellen, sollten die [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) Methode.
   3. [Measure](/uwp/api/windows.ui.xaml.uielement.measure) das "UIElement" mit der im Voraus berechnete Größe.
3. Als Teil der [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride), [anordnen](/uwp/api/windows.ui.xaml.uielement.arrange) jedes "UIElement" mit der im Voraus berechnete Position realisiert.

> [!NOTE]
> Ein Daten-Layout-Ansatz ist häufig nicht kompatibel mit _Datenvirtualisierung_.  Insbesondere, in dem nur die Daten in den Arbeitsspeicher geladen, Daten, die erforderlich sind, zum Ausfüllen, was für den Benutzer sichtbar ist vorhanden ist.  Data Virtualization ist nicht auf lazy oder inkrementellen Laden von Daten bezieht, wie ein Benutzer einen Bildlauf nach unten, wo die Daten erhält vom residenten bleibt.  Es ist vielmehr auf verweisen, wenn Elemente aus dem Speicher freigegeben werden, da sie außerhalb der Ansicht gescrollt werden.  Müssen Sie ein Datenlayout, die jedem Datenelement überprüft als Teil eines Datenlayouts Datenvirtualisierung ordnungsgemäß verhindern würden, wie erwartet.  Eine Ausnahme ist ein Layout wie die UniformGridLayout davon aus, alles, was die gleiche Größe hat.

> [!TIP]
> Wenn Sie ein benutzerdefiniertes Steuerelement für eine Steuerelementbibliothek erstellen, die von anderen Benutzern in einer Vielzahl von Situationen verwendet werden, wird möglicherweise ein Datenlayout keine Option für Sie.

### <a name="example-xbox-activity-feed-layout"></a>Beispiel: Xbox-Aktivitätsfeed layout

Die Benutzeroberfläche für die Xbox-Aktivitätsfeed verwendet ein sich wiederholendes Muster, in dem jede Zeile eine Breite Kachel, gefolgt von zwei schmalen Kacheln, die umgekehrt wird in der nachfolgenden Zeile hat. In diesem Layout wird die Größe für jedes Element eine Funktion von der Position des Elements in das DataSet und die bekannte Größe für die Kacheln (Breite Vs einzuschränken).

![Aktivitätsfeed Xbox](images/xaml-attached-layout-activityfeedscreenshot.png)

Der Code folgt die Beschreibung, über welche eine benutzerdefinierte Virtualisierung der Benutzeroberfläche für die Aktivität mit dem feed kann sein, um zu veranschaulichen, werden die allgemeinen Ansatz Sie dauern für eine **Datenlayout**.

<table>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um die app öffnen und finden Sie unter der <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> in Aktion, die mit diesem Beispiel-Layout.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Erwerben Sie den Quellcode (GitHub)</a></li>
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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(Optional) Verwalten das Element, das "UIElement"-Zuordnung

In der Standardeinstellung die [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) verwaltet eine Zuordnung zwischen den realisierten Elementen und den Index in der Datenquelle, die sie darstellen.  Ein Layout können diese Zuordnung selbst anfordern immer die Option zum Verwalten von [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) beim Abrufen eines Elements über die [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) -Methode die verhindert, der Standardwert dass automatische Wiederverwendung Verhalten.  Ein Layout kann dazu z. B. entscheiden, wenn es nur verwendet wird, beim Durchführen eines Bildlaufs auf eine Richtung beschränkt ist und die Elemente, die er berücksichtigt werden immer in der zusammenhängenden (d. h. in dem wissen, dass der Index des ersten und letzten Elements ist ausreichend, um alle Elemente kennen, die Rea werden soll Lized).

#### <a name="example-xbox-activity-feed-measure"></a>Beispiel: Xbox-Aktivitätsfeed measure

Der folgende Codeausschnitt zeigt die zusätzliche Logik, die der MeasureOverride im vorherigen Beispiel zum Verwalten der Zuordnung hinzugefügt werden konnte.

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

## <a name="content-dependent-virtualizing-layouts"></a>Inhalt abhängiges virtualisierenden Layouts

Wenn Sie müssen zuerst messen, die Inhalte der Benutzeroberfläche für ein Element aus, um die genaue Größe zu ermitteln einer **Inhalt abhängiges Layout**.  Sie können auch davon vorstellen, als ein Layout, in dem jedes Element seine Größe selbst festlegen muss, anstatt das Layout an, dass dem Element die Größe. Virtualisierenden Layouts, die in diese Kategorie fallen, sind komplizierter.

> [!NOTE]
> Keine Inhalte abhängiges Layouts (sollte nicht) Datenvirtualisierung unterbrechen.

### <a name="estimations"></a>Schätzungen

Inhalt abhängiges Layouts nutzen Schätzung, sowohl die Größe des Inhalts von realisierten als auch die Position des realisierten Inhalts zu erraten. Wenn diese Schätzungen ändern, verursacht dies die realisierten Inhalte regelmäßig Positionen innerhalb des bildlauffähigen Bereichs verschoben. Dies kann zu einer sehr frustrierend und bedürfen der Benutzeroberfläche führen, wenn dies nicht verringert. Hier werden die möglichen Probleme und Lösungen erläutert.

> [!NOTE]
> Datenlayouts, die jedes Element berücksichtigen und kennen die genaue Größe aller Elemente, die erkannt oder nicht, und ihre Positionen können diese Probleme vollständig vermeiden.

**Führen Sie einen Bildlauf zu verankern**

XAML bietet einen Mechanismus zum plötzlichen Viewport-Schichten zu verringern, indem Sie mit Bildlauf Steuerelemente unterstützen [Scroll Verankern](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) durch die Implementierung der [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) Schnittstelle. Wie der Benutzer den Inhalt bearbeitet werden, wählt das Scrollen Steuerelement kontinuierlich ein Element aus der Gruppe der Kandidaten, die erfolgter Anmeldung nachverfolgt werden. Verschiebt die Position des Ankerelements während des Layouts verschiebt sich dann das Scroll-Steuerelement automatisch des Viewports, um den Viewport zu gewährleisten.

Der Wert des der [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) bereitgestellt, um das Layout der dienstsynchronisierung vorliegen, aktuell ausgewählten Anchor-Element, das von der bildlaufsteuerelement ausgewählt. Alternativ, wenn ein Entwickler fordert explizit an, dass ein Element für einen Index mit realisiert werden die [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) Methode für die [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), diesen Index zugewiesen ist, als die [ RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) auf der nächsten Layoutdurchlauf. Das Layout auf die wahrscheinliches Szenario vorbereitet sein, dass ein Entwickler erkennt, ein Element dass, und anschließend fordert an, dass es in die Ansicht über die eingebunden werden dadurch die [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) Methode.

Die [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) ist der Index für das Element in der Datenquelle, das ein Layout Inhalt abhängiges zuerst positioniert werden sollte, um die Position der Elemente zu ermitteln. Sie sollten als Ausgangspunkt für die Positionierung anderer realisierten Elementen dienen.

**Auswirkungen auf die Bildlaufleisten**

Trotz führen Sie einen Bildlauf zu verankern, wenn das Layout des Schätzungen häufig verwenden, möglicherweise aufgrund von erhebliche abweichungen bei der variieren kann die Größe des Inhalts, und klicken Sie dann auf die Position des Ziehpunkts für die Bildlaufleiste angezeigt, um zu wechseln.  Dies kann für einen Benutzer irritierend sein, wenn das Thumb-Steuerelement nicht mit dem Mauszeiger auf die Position nachzuverfolgen, wenn sie sie ziehen können.

Desto genauer kann das Layout in seine Schätzungen werden, und klicken Sie dann die weniger wahrscheinlich, dass ein Benutzer wird die springen Bildlaufleiste angezeigt.

### <a name="layout-corrections"></a>Layout-Korrekturen

Ein Layout Inhalt abhängiges sollten darauf vorbereitet sein, auf die Schätzung Abklingen zu rationalisieren.  Wenn der Benutzer scrollt, um z. B. am Anfang des Inhalts und das Layout erkennt, dass das erste Element sind, finden es möglicherweise, dass die Position des Elements erwartete Bezug auf das Element, in dem sie gestartet haben, als der Ursprung (x-: 0 an einer beliebigen Stelle angezeigt werden würde Y:0). In diesem Fall können das Layout der [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) Eigenschaft, um die Position, die er berechnet als das neue Layout Ursprung festzulegen.  Das Ergebnis ähnelt der Scrollen verankern, in dem die bildlaufsteuerelement Viewport automatisch-Konto für den Inhalt der Position angepasst wird, wie das Layout gemeldet.

![Korrigieren Sie die LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Nicht verbundene Viewports

Die Größe aus des Layouts des zurückgegebenen [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) Methode darstellt, die bestmögliche Schätzung der Größe des Inhalts der mit jedem nachfolgenden Layout ändern kann.  Wenn ein Benutzer einen Bildlauf durchführt wird das Layout mit einem aktualisierten ständig neu ausgewertet werden [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect).

Wenn ein Benutzer das Thumb-Steuerelement sehr schnell zieht springt es möglich, für den Viewport, aus der Perspektive des Layouts, angezeigt werden, zu groß werden lassen, in denen die vorherige Position die aktuelle Position nicht überlappt.  Dies ist aufgrund der asynchronen Natur des Bildlaufs an. Es ist auch möglich, für eine app, die verwendet wird, das Layout, um anzufordern, dass ein Element in die Ansicht für ein Element eingebunden werden, die derzeit nicht realisiert und schätzungsweise um außerhalb des aktuellen Bereichs, der nachverfolgt werden, indem Sie das Layout zu erstellen.

Wenn das Layout wird ermittelt, die Schätzung nicht zutrifft und/oder eine unerwartete Viewport-Verschiebung sieht, muss sie die Position des ersten Basiswerte.  Die virtualisierenden Layouts, die im Rahmen der XAML-Steuerelemente geliefert werden als Inhalt abhängiges Layouts entwickelt, wenn sie weniger Einschränkungen auf die Art des Inhalts platzieren, die angezeigt wird.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Beispiel: Einfache virtualisierenden Stapellayout für Elemente mit variabler Größe

Das folgende Beispiel veranschaulicht eine einfache Stapellayout zum variabler Größe, die Elemente:

* unterstützt die Virtualisierung der Benutzeroberfläche
* Schätzungen, die verwendet wird, um die Größe von realisierten Elementen zu erraten,
* erkennt mögliche diskontinuierlich Viewport-Schichten und
* Wendet Layout Korrekturen vor, um diese Schichten zu berücksichtigen.

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
