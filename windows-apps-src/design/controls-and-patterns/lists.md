---
Description: Listen zeigen und ermöglichen die Interaktion mit sammlungsbasierten Inhalten.
title: Sammlungen und Listen
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Collections and Lists
template: detail.hbs
ms.date: 10/08/2019
ms.topic: article
keywords: Windows 10, UWP
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e1167a57da6a3f54cabcc946cfbf7a592f301d2c
ms.sourcegitcommit: 9625f8fb86ff6473ac2851e600bc02e996993660
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72163750"
---
# <a name="collections-and-lists"></a>Sammlungen und Listen

Sammlungen und Listen beziehen sich beide auf die Darstellung mehrerer verwandter Datenelemente, die gemeinsam angezeigt werden. Sammlungen können auf mehrere Arten und durch mehrere unterschiedliche Sammlungssteuerelemente dargestellt werden (können auch als Sammlungsansichten bezeichnet werden). Sammlungssteuerelemente zeigen und ermöglichen Interaktionen mit sammlungsbasierten Inhalten wie einer Liste mit Kontakten, einer Liste mit Daten, einer Sammlung von Bildern usw.  Zu den in diesem Artikel behandelten Steuerelementen gehören:

- Listenansichten, die in erster Linie zum Anzeigen von textlastigen Inhaltssammlungen verwendet werden
- Rasteransichten, die in erster Linie zum Anzeigen von bildlastigen Inhaltssammlungen verwendet werden
- Flip-Ansichten, die in erster Linie zum Anzeigen von bildlastigen Inhaltssammlungen verwendet werden, bei denen exakt ein Element gleichzeitig den Fokus erhalten muss.
- Strukturansichten, die in erster Linie zum Anzeigen von textlastigen Inhaltssammlungen in einer bestimmten Hierarchie verwendet werden.
- ItemsRepeater, wobei es sich um einen anpassbaren Baustein zum Erstellen benutzerdefinierter Sammlungssteuerelemente handelt.


Entwurfsrichtlinien, Funktionen und Beispiele für jedes Steuerelement findest du weiter unten.

Jedes dieser Steuerelemente (mit Ausnahme von ItemsRepeater) bietet integrierte Stile und Interaktionen. Um jedoch die visuelle Darstellung Ihrer Sammlungsansicht und der darin enthaltenen Elemente weiter anzupassen, wird ein [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) verwendet. Ausführliche Informationen zu Datenvorlagen und zum Anpassen der Darstellung einer Sammlungsansicht findest du auf der Seite [Elementcontainer und -vorlagen](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/item-containers-templates).

Jedes dieser Steuerelemente (mit Ausnahme von ItemsRepeater) verfügt außerdem über ein integriertes Verhalten, das die Auswahl einzelner oder mehrerer Elemente ermöglicht. Weitere Informationen findest du unter [Übersicht über Auswahlmodi](selection-modes.md).

> **Wichtige APIs:** [ListView class](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [GridView class](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView), [FlipView class](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flipview), [TreeView class](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview), [ItemsRepeater class](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)

> <div id="main">
> <strong>Windows 10 Fall Creators Update – Abweichende Funktionsweise</strong>
> </div>
> Beim Schwenken/Bildlauf in der Liste der UWP-Apps wird jetzt standardmäßig anstelle des Ausführens der Auswahl ein aktiver Stift verwendet (z. B. Toucheingabe, Touchpad und passiver Stift).
> Wenn Ihre App vom vorherigen Verhalten abhängig ist, können Sie die Stift-Bildlaufaktionen außer Kraft setzen und auf das vorherige Verhalten zurückzusetzen. Weitere Details finden Sie im API-Referenzthema für die <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer">ScrollViewer-Klasse</a>.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn du die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert hast, kannst du die Steuerelemente <a href="xamlcontrolsgallery:/item/ListView">ListView</a>, <a href="xamlcontrolsgallery:/item/GridView">GridView</a>, <a href="xamlcontrolsgallery:/item/FlipView">FlipView</a>, <a href="xamlcontrolsgallery:/item/TreeView">TreeView</a> und <a href="xamlcontrolsgalley:/item/ItemsRepeater">ItemsRepeater</a> in Aktion sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="list-views"></a>Listenansichten

Listenansichten stellen textintensive Elemente dar, normalerweise in einem einspaltigen, vertikal gestapelten Layout. Mit ihnen kannst du Elemente kategorisieren und Gruppenüberschriften zuweisen, Elemente ziehen und ablegen, Inhalt zusammenstellen und Elemente neu anordnen.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Mit einer Listenansicht können Sie:

- Anzeigen einer Sammlung, die in erster Linie aus textbasierten Elementen besteht, wobei alle Elemente das selbe visuelle und Interaktionsverhalten aufweisen sollten.
- Darstellen einer einzelnen oder kategorisierten Inhaltssammlung.
- Aufnehmen einer Vielzahl von Anwendungsfällen, einschließlich der folgenden gängigen:
    - Erstellen einer Liste von Nachrichten oder Nachrichtenprotokollen.
    - Erstellen einer Kontaktliste.
    - Den Masterbereich mit dem [Master-/Detailmuster](master-details.md) erstellen. Ein Master-/Detailmuster wird häufig in E-Mail-Apps verwendet, in denen ein Bereich (der Masterbereich) eine Liste auswählbarer Elemente enthält, während im anderen eine detaillierte Ansicht des ausgewählten Elements enthalten ist.
    

### <a name="examples"></a>Beispiele

Im Folgenden findest du eine einfache Listenansicht, in der eine Kontaktliste angezeigt wird und in der die Datenelemente alphabetisch gruppiert sind. Die Gruppenkopfzeilen (in diesem Beispiel jeder Buchstabe des Alphabets) können auch so angepasst werden, dass sie „fixiert“ (sticky) bleiben, damit sie beim Scrollen immer am oberen Rand der ListView angezeigt werden.

![Eine Listenansicht mit gruppierten Daten](images/listview-grouped-example-resized-final.png)

Dies ist eine ListView, die invertiert wurde, um ein Protokoll mit Meldungen anzuzeigen, wobei die neuesten Meldungen ganz unten angezeigt werden. Bei einer invertierten ListView werden Elemente am unteren Bildschirmrand mit einer integrierten Animation angezeigt.

![Invertierte Listenansicht](images/listview-inverted-2.png)

### <a name="related-articles"></a>Verwandte Artikel
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">Listenansicht und Rasteransicht</a></p></td>
<td align="left"><p>Lernen Sie die Grundlagen der Verwendung einer Listen- oder Rasteransicht in Ihrer App kennen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Elementcontainer und Vorlagen</a></p></td>
<td align="left"><p>Die Elemente, die du in einer Listen- oder Rasteransicht anzeigst, können eine wichtige Rolle für das Gesamtdesign deiner App spielen. Lass deine App gut aussehen , indem du die Darstellung deiner Sammlungselemente durch Ändern von Steuerelementvorlagen und Datenvorlagen anpasst.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">Elementvorlagen für Listenansicht</a></p></td>
<td align="left"><p>Verwenden Sie diese Beispiel-Elementvorlagen für eine ListView, um das Erscheinungsbild gängiger App-Typen abzurufen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">Invertierte Listen</a></p></td>
<td align="left"><p>Bei invertierten Listen werden neue Elemente am Ende hinzugefügt, z. B. bei einer Chat-App. Befolge die Anleitung dieses Artikels, um in deiner App eine invertierte Liste zu verwenden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">Aktualisieren durch Ziehen</a></p></td>
<td align="left"><p>Dank des Mechanismus „Aktualisieren durch Ziehen“ können Benutzer aktuelle Daten in einer Liste durch das Ausführen einer Ziehbewegung von oben nach unten auf der Liste abrufen. Verwende diesen Artikel, um „Aktualisieren durch Ziehen“ in deiner Listenansicht zu implementieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Geschachtelte UI</a></p></td>
<td align="left"><p>Eine geschachtelte UI ist eine Benutzeroberfläche (User Interface, UI) mit geschachtelten Steuerelementen, die in einem Container eingeschlossen sind, die ein Benutzer ebenfalls aktivieren kann. Beispielsweise gibt es möglicherweise ein Listenansichtelement, das eine Schaltfläche enthält. Benutzer können das Listenelement auswählen oder die Schaltfläche verwenden, die darin geschachtelt ist. Befolgen Sie diese bewährten Methoden, um eine optimal geschachtelte Benutzeroberfläche (UI) für Ihre Benutzer bereitzustellen.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>Rasteransichten

Rasteransichten eignen sich zum Anordnen und Durchsuchen bildbasierter Inhaltssammlungen. Ein Rasteransichtslayout wird vertikal gescrollt und horizontal bewegt. Elemente befinden sich in einem umbrochenen Layout und werden von links nach rechts und anschließend von oben nach unten in Leserichtung angezeigt.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwende eine Rasteransicht für Folgendes:

- Anzeigen einer Inhaltssammlung, in der der Fokuspunkt jedes Elements ein Bild ist und jedes Element das selbe visuelle und Interaktionsverhalten aufweisen sollte.
- Inhaltsbibliotheken anzeigen.
- Die zwei Inhaltsansichten formatieren, die dem [semantischen Zoom](semantic-zoom.md) zugeordnet sind.
- Aufnehmen einer Vielzahl von Anwendungsfällen, einschließlich der folgenden gängigen:
    - Storefront-artige Benutzeroberfläche (d. h. Durchsuchen von Apps, Liedern, Produkten)
    - Interaktive Fotobibliotheken

### <a name="examples"></a>Beispiele

Dieses Beispiel zeigt ein typisches Rasteransichtslayout, in diesem Fall zum Durchsuchen von Apps. Metadaten für Rasteransichtselemente sind in der Regel auf wenige Textzeilen und eine Bewertung des Elements beschränkt.

![Beispiel eines Rasteransichtslayouts](images/controls_gridview_example02.png)

Eine Rasteransicht eignet sich ideal für eine Inhaltsbibliothek, die häufig verwendet wird, um Medien wie Bilder und Videos darzustellen. In einer Inhaltsbibliothek erwarten Benutzer, dass sie auf ein Element tippen können, um eine Aktion aufzurufen.

![Beispiel einer Inhaltsbibliothek](images/gridview-simple-example-final.png)

### <a name="related-articles"></a>Verwandte Artikel
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">Listenansicht und Rasteransicht</a></p></td>
<td align="left"><p>Lernen Sie die Grundlagen der Verwendung einer Listen- oder Rasteransicht in Ihrer App kennen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Elementcontainer und Vorlagen</a></p></td>
<td align="left"><p>Die Elemente, die du in einer Listen- oder Rasteransicht anzeigst, können eine wichtige Rolle für das Gesamtdesign deiner App spielen. Lass deine App gut aussehen , indem du die Darstellung deiner Sammlungselemente durch Ändern von Steuerelementvorlagen und Datenvorlagen anpasst.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">Elementvorlagen für Rasteransicht</a></p></td>
<td align="left"><p>Verwenden Sie diese Beispiel-Elementvorlagen für eine GridView, um das Erscheinungsbild gängiger App-Typen abzurufen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Geschachtelte UI</a></p></td>
<td align="left"><p>Eine geschachtelte UI ist eine Benutzeroberfläche (User Interface, UI) mit geschachtelten Steuerelementen, die in einem Container eingeschlossen sind, die ein Benutzer ebenfalls aktivieren kann. Beispielsweise gibt es möglicherweise ein Rasteransichtselement, das eine Schaltfläche enthält. Benutzer können das Rasterelement auswählen oder die Schaltfläche verwenden, die darin geschachtelt ist. Befolgen Sie diese bewährten Methoden, um eine optimal geschachtelte Benutzeroberfläche (UI) für Ihre Benutzer bereitzustellen.</p></td>
</tr>
</tbody>
</table>

## <a name="flip-views"></a>Flip-Ansichten

Flip-Ansichten eignen sich zum Durchsuchen bildbasierter Inhaltssammlungen, insbesondere dann, wenn die gewünschte Erfahrung so ist, dass immer nur ein Bild sichtbar ist. Eine Flip-Ansicht ermöglicht dem Benutzer sich durch die Sammlungselemente zu bewegen bzw. diese „durchzublättern“ (vertikal oder horizontal), wobei jedes Element einzeln nach der Benutzerinteraktion angezeigt wird.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwende eine Flip-Ansicht für Folgendes:

- Anzeigen einer kleinen bis mittelgroßen Sammlung (weniger als 25 Elemente), wobei die Sammlung aus Bildern besteht, die nur wenige oder gar keine Metadaten besitzen.
- Einzelnes Anzeigen von Elementen, wobei dem Endbenutzer das Durchblättern der Elemente in der selbst gewählten Geschwindigkeit gestattet wird.
- Aufnehmen einer Vielzahl von Anwendungsfällen, einschließlich der folgenden gängigen:
    - Fotokataloge
    - Produktkataloge oder -präsentationen

### <a name="examples"></a>Beispiele

In den folgenden beiden Beispielen wird eine FlipView gezeigt, die horizontal bzw. vertikal durchblätterbar ist.

![Horizontale Flip-Ansicht](images/controls_flipview_horizonal.jpg)

![Vertikale Flip-Ansicht](images/controls_flipview_vertical.jpg)

### <a name="related-articles"></a>Verwandte Artikel
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="flipview.md">Flip-Ansicht</a></p></td>
<td align="left"><p>Erfahre die Grundlagen der Verwendung einer Flip-Ansicht in deiner App sowie, wie du das Aussehen deiner Elemente innerhalb einer Flip-Ansicht anpasst.</p></td>
</tr>
</tbody>
</table>

## <a name="tree-views"></a>Strukturansichten

Strukturansichten sind zum Anzeigen von textbasierten Sammlungen geeignet, die über eine wichtige Hierarchie verfügen, die angezeigt werden muss. Strukturansichtselemente sind reduzierbar/erweiterbar, werden in einer visuellen Hierarchie angezeigt, können mit Symbolen ergänzt werden und können zwischen Strukturansichten gezogen und abgelegt werden. Strukturansichten ermöglichen eine Schachtelung auf N Ebenen.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwende eine Strukturansicht für Folgendes:

- Anzeigen einer Sammlung geschachtelter Elemente, deren Kontext und Bedeutung von einer Hierarchie oder einer bestimmten Organisationskette abhängig sind.
- Aufnehmen einer Vielzahl von Anwendungsfällen, einschließlich der folgenden gängigen:
    - Datei-Browser
    - Unternehmensorganigramm

### <a name="examples"></a>Beispiele

Im Folgenden findest du ein Beispiel für eine Strukturansicht, die einen Datei-Explorer darstellt und viele verschiedene, geschachtelte Elemente anzeigt, die von Symbolen ergänzt werden.

![Strukturansicht mit Symbolen](images/treeview-icons.png)

### <a name="related-articles"></a>Verwandte Artikel
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="tree-view.md">Strukturansicht</a></p></td>
<td align="left"><p>Erfahre die Grundlagen der Verwendung einer Strukturansicht in deiner App sowie, wie du das Aussehen und Interaktionsverhalten deiner Elemente innerhalb einer Strukturansicht anpasst.</p></td>
</tr>
</tbody>
</table>

## <a name="itemsrepeater"></a>ItemsRepeater

ItemsRepeater unterscheidet sich von den übrigen Sammlungssteuerelementen, die auf dieser Seite angezeigt werden, da es keine vorgefertigten Stile oder Interaktionen bereitstellt, d. h., wenn es einfach auf einer Seite platziert wird, ohne Eigenschaften zu definieren. ItemsRepeater ist eher ein Baustein, den du als Entwickler verwenden kannst, um ein eigenes benutzerdefiniertes Sammlungssteuerelement zu erstellen, insbesondere eins, das nicht mithilfe der anderen in diesem Artikel aufgeführten Steuerelemente erstellt werden kann. ItemsRepeater ist ein datengesteuerter Hochleistungsbereich, der exakt auf deine Anforderungen zugeschnitten werden kann.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwende in folgenden Fällen ein „ItemsRepeater“-Steuerelement:

- Du hast eine konkrete Vorstellung von einer Benutzeroberfläche und Benutzererfahrung, die nicht mithilfe vorhandener Sammlungssteuerelemente erstellt werden können.
- Du verfügst über eine bestehende Datenquelle für deine Elemente (z. B. aus dem Internet abgerufene Daten, eine Datenbank oder eine bereits vorhandene Sammlung in deiner CodeBehind).

### <a name="examples"></a>Beispiele

Die folgenden drei Beispiele sind alle ItemsRepeater-Steuerelemente, die an dieselbe Datenquelle (eine Sammlung von Zahlen) gebunden sind. Die Sammlung von Zahlen wird auf drei Arten dargestellt, wobei jedes der unten dargestellten ItemsRepeater-Steuerelemente unter Verwendung eines anderen benutzerdefinierten [Layouts](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layout) und einer anderen benutzerdefinierten [ItemTemplate](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate?view=winui-2.2) benutzt wurde.

![ItemsRepeater mit horizontalen Balken](images/itemsrepeater-1.png)
![ItemsRepeater mit vertikalen Balken](images/itemsrepeater-2.png)
![ItemsRepeater mit kreisförmiger Darstellung](images/itemsrepeater-3.png)

### <a name="related-articles"></a>Verwandte Artikel
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="items-repeater.md">ItemsRepeater</a></p></td>
<td align="left"><p>Erfahre mehr über die Grundlagen der Verwendung eines ItemsRepeater-Steuerelements in deiner App und wie du alle erforderlichen Interaktionen und visuellen Komponenten für deine Sammlungsansicht implementieren kannst.</p></td>
</tr>
</tbody>
</table>


## <a name="globalization-and-localization-checklist"></a>Prüfliste für Globalisierung und Lokalisierung

<table>
<tr>
<th>Umbruch</th><td>Zwei Zeilen für die Listenbeschriftung zulassen.</td>
</tr>
<tr>
<th>Horizontale Erweiterung</th><td>Stelle sicher, dass die Felder sich an Texte unterschiedlicher Längen anpassen können und stets scrollfähig sind.</td>
</tr>
<tr>
<th>Vertikaler Abstand</th><td>Verwende nicht lateinische Zeichen für vertikalen Abstand, um sicherzustellen, dass nicht lateinische Schriften richtig angezeigt werden.</td>
</tr>
</table>

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

**Entwurfs- und UX-Richtlinien**
- [Master/Details](master-details.md)
- [Navigationsbereich](navigationview.md)
- [Semantischer Zoom](semantic-zoom.md)
- [Drag & Drop](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [Miniaturbilder](../../files/thumbnails.md)

**API-Referenz**
- [ListView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [ComboBox-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)
- [ListBox-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
