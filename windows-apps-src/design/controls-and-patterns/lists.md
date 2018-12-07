---
Description: Lists display and enable interaction with collection-based content.
title: Listen
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Lists
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d8ea08cd02314fb566680d8f5b249eaf735b977a
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "8780805"
---
# <a name="lists"></a>Listen

Listen zeigen und ermöglichen die Interaktion mit sammlungsbasierten Inhalten. Zu den in diesem Artikel behandelten vier Listenmustern gehören:

-   Listenansichten, die in erster Linie zum Anzeigen von textlastigen Inhaltssammlungen verwendet werden
-   Rasteransichten, die in erster Linie zum Anzeigen von bildlastigen Inhaltssammlungen verwendet werden
-   Dropdownlisten, aus denen Benutzer ein Element aus einer erweiterten Liste auswählen können
-   Listenfelder, in denen Benutzer ein einzelnes Element oder mehrere Elemente aus einem Feld auswählen können, in dem gescrollt werden kann

Für jedes Listenmuster sind Entwurfsrichtlinien, Features und Beispiele aufgeführt.

> **Wichtige APIs**: [ListView-Klasse](https://msdn.microsoft.com/library/windows/apps/br242878), [GridView-Klasse](https://msdn.microsoft.com/library/windows/apps/br242705), [ComboBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br209348)


> <div id="main">
> <strong>Windows10 Fall Creators Update – Änderung des Verhaltens</strong>
> </div>
> Beim Schwenken/Bildlauf in der Liste der UWP-Apps wird jetzt standardmäßig anstelle des Ausführens der Auswahl ein aktiver Stift verwendet (z.B. Toucheingabe, Touchpad und passiver Stift).
> Wenn Ihre App vom vorherigen Verhalten abhängig ist, können Sie die Stift-Bildlaufaktionen außer Kraft setzen und auf das vorherige Verhalten zurückzusetzen. Finden Sie unter das [ScrollViewer-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) der API-Referenzthema für Details.

## <a name="list-views"></a>Listenansichten

Mit Listenansichten können Sie Elemente kategorisieren und Gruppenüberschriften zuweisen, Elemente per Drag & Drop verschieben, Inhalt überprüfen und Elemente neu anordnen.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Mit einer Listenansicht können Sie:

-   Inhaltssammlungen anzeigen, die in erster Linie aus Text bestehen.
-   Durch eine einzelne oder kategorisierte Inhaltssammlung navigieren.
-   Den Masterbereich mit dem [Master-/Detailmuster](master-details.md) erstellen. Ein Master-/Detailmuster wird häufig in E-Mail-Apps verwendet, in denen ein Bereich (der Masterbereich) eine Liste auswählbarer Elemente enthält, während im anderen eine detaillierte Ansicht des ausgewählten Elements enthalten ist.

### <a name="examples"></a>Beispiele

Dies ist eine einfache Listenansicht mit gruppierten Daten auf einem Telefon.

![Eine Listenansicht mit gruppierten Daten](images/simple-list-view-phone.png)

### <a name="recommendations"></a>Empfehlungen

-   Elemente in einer Liste sollten das gleiche Verhalten aufweisen.
-   Wenn Ihre Liste in Gruppen unterteilt ist, verwenden Sie den [semantischen Zoom](semantic-zoom.md), mit dem Benutzern die Navigation in gruppierten Inhalten erleichtert wird.

### <a name="list-view-articles"></a>Artikel zur Listenansicht
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
<td align="left"><p>Die Elemente, die Sie in einer Liste oder Raster anzeigen, können eine wichtige Rolle für das Gesamtdesign Ihrer App spielen. Ändern Sie Steuerelementvorlagen und Datenvorlagen, um das Erscheinungsbild der Elemente zu definieren, damit Ihre App ansprechend aussieht.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">Elementvorlagen für Listenansicht</a></p></td>
<td align="left"><p>Verwenden Sie diese Beispiel-Elementvorlagen für eine ListView, um das Erscheinungsbild gängiger App-Typen abzurufen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">Invertierte Listen</a></p></td>
<td align="left"><p>Bei invertierten Listen werden neue Elemente am Ende hinzugefügt, z.B. bei einer Chat-App. Befolgen Sie diese Richtlinien, um in Ihrer App eine invertierte Liste zu verwenden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">Aktualisieren durch Ziehen</a></p></td>
<td align="left"><p>Dank des Musters „Aktualisieren durch Ziehen“ können Benutzer aktuelle Daten in einer Liste durch das Ausführen einer Ziehbewegung von oben nach unten auf der Liste abrufen. Verwenden Sie diese Anleitung, um „Aktualisieren durch Ziehen“ in der Listenansicht zu implementieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Geschachtelte UI</a></p></td>
<td align="left"><p>Eine geschachtelte UI ist eine Benutzeroberfläche (User Interface, UI) mit geschachtelten Steuerelementen, die in einem Container eingeschlossen sind, die ein Benutzer ebenfalls aktivieren kann. Beispielsweise gibt es möglicherweise ein Listenansichtelement, das eine Schaltfläche enthält. Benutzer können das Listenelement auswählen oder die Schaltfläche verwenden, die darin geschachtelt ist. Befolgen Sie diese bewährten Methoden, um eine optimal geschachtelte Benutzeroberfläche (UI) für Ihre Benutzer bereitzustellen.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>Rasteransichten

Rasteransichten eignen sich zum Anordnen und Durchsuchen bildbasierter Inhaltssammlungen. Ein Rasteransichtslayout wird vertikal gescrollt und horizontal bewegt. Elemente werden von links nach rechts und anschließend von oben nach unten in Leserichtung angeordnet.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Mit einer Listenansicht können Sie:

-   Eine Inhaltssammlung anzeigen, die in erster Linie aus Bildern besteht.
-   Inhaltsbibliotheken anzeigen.
-   Die zwei Inhaltsansichten formatieren, die dem [semantischen Zoom](semantic-zoom.md) zugeordnet sind.

### <a name="examples"></a>Beispiele

Dieses Beispiel zeigt ein typisches Rasteransichtslayout, in diesem Fall zum Durchsuchen von Apps. Metadaten für Rasteransichtselemente sind in der Regel auf wenige Textzeilen und eine Bewertung des Elements beschränkt.

![Beispiel eines Rasteransichtslayouts](images/controls_gridview_example02.png)

Eine Rasteransicht eignet sich ideal für eine Inhaltsbibliothek, die häufig verwendet wird, um Medien wie Bilder und Videos darzustellen. In einer Inhaltsbibliothek erwarten Benutzer, dass sie auf ein Element tippen können, um eine Aktion aufzurufen.

![Beispiel einer Inhaltsbibliothek](images/controls_list_contentlibrary.png)

### <a name="recommendations"></a>Empfehlungen

-   Elemente in einer Liste sollten das gleiche Verhalten aufweisen.
-   Wenn Ihre Liste in Gruppen unterteilt ist, verwenden Sie den [semantischen Zoom](semantic-zoom.md), mit dem Benutzern die Navigation in gruppierten Inhalten erleichtert wird.

### <a name="grid-view-articles"></a>Artikel zur Rasteransicht
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
<td align="left"><p>Die Elemente, die Sie in einer Liste oder Raster anzeigen, können eine wichtige Rolle für das Gesamtdesign Ihrer App spielen. Ändern Sie Steuerelementvorlagen und Datenvorlagen, um das Erscheinungsbild der Elemente zu definieren, damit Ihre App gut aussieht.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">Elementvorlagen für Rasteransicht</a></p></td>
<td align="left"><p>Verwenden Sie diese Beispiel-Elementvorlagen für eine GridView, um das Erscheinungsbild gängiger App-Typen abzurufen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Geschachtelte UI</a></p></td>
<td align="left"><p>Eine geschachtelte UI ist eine Benutzeroberfläche (User Interface, UI) mit geschachtelten Steuerelementen, die in einem Container eingeschlossen sind, die ein Benutzer ebenfalls aktivieren kann. Beispielsweise gibt es möglicherweise ein Listenansichtelement, das eine Schaltfläche enthält. Benutzer können das Listenelement auswählen oder die Schaltfläche verwenden, die darin geschachtelt ist. Befolgen Sie diese bewährten Methoden, um eine optimal geschachtelte Benutzeroberfläche (UI) für Ihre Benutzer bereitzustellen.</p></td>
</tr>
</tbody>
</table>

## <a name="drop-down-lists"></a>Dropdownlisten

Dropdownlisten, auch als Kombinationsfelder bezeichnet, werden in einem kompakten Zustand gestartet und erweitert, um eine Liste mit auswählbaren Elementen anzuzeigen. Das ausgewählte Element ist stets sichtbar. Nicht sichtbare Elemente können eingeblendet werden, wenn der Benutzer auf das Kombinationsfeld tippt, um es zu erweitern.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

-   Mit einer Dropdownliste können Benutzer einen einzelnen Wert aus einer Reihe von Elementen auswählen, die mit einzelnen Textzeilen angemessen dargestellt werden können.
-   Verwenden Sie eine Liste oder eine Rasteransicht anstelle eines Kombinationsfelds, um Elemente anzuzeigen, die mehrere Textzeilen oder Bilder enthalten.
-   Wenn weniger als fünf Elemente vorhanden sind, können Sie stattdessen die Verwendung von [Optionsfeldern](radio-button.md) (wenn nur ein Element ausgewählt werden kann) oder [Kontrollkästchen](checkbox.md) (wenn mehrere Elemente ausgewählt werden können) in Betracht ziehen.
-   Verwenden Sie ein Kombinationsfeld, wenn die Auswahlelemente für den Fluss der App weniger wichtig sind. Wenn für die Mehrzahl der Benutzer in der Mehrzahl der Situationen die Standardoption empfohlen wird, kann die Anzeige aller Elemente in einer Listenansicht mehr Aufmerksamkeit auf die Optionen ziehen als nötig. Sie können Platz sparen und Ablenkungen reduzieren, indem Sie ein Kombinationsfeld verwenden.

### <a name="examples"></a>Beispiele

Ein Kombinationsfeld im kompakten Zustand kann eine Kopfzeile anzeigen.

![Beispiel für eine Dropdownliste im kompakten Zustand](images/combo_box_collapsed.png)

Obwohl Kombinationsfelder erweitert werden, um längere Zeichenfolgen zu unterstützen, sollten Sie extrem lange Zeichenfolgen vermeiden, die nur schwer lesbar sind.

![Beispiel einer Dropdownliste mit einer langen Textzeichenfolge](images/combo_box_listitemstate.png)

Wenn die Liste in einem Kombinationsfeld lang genug ist, wird eine Bildlaufleiste angezeigt. Gruppieren Sie die Elemente in der Liste logisch.

![Beispiel einer Bildlaufleiste in einer Dropdownliste](images/combo_box_scroll.png)

### <a name="recommendations"></a>Empfehlungen

-   Schränken Sie den Textinhalt von Kombinationsfeldelementen auf eine einzelne Zeile ein.
-   Sortieren Sie die Elemente in einem Kombinationsfeld in der logischsten Reihenfolge. Gruppieren Sie verwandte Optionen, und platzieren Sie die am häufigsten verwendeten Optionen oben in der Liste. Sortieren Sie Namen in alphabetischer Reihenfolge, Zahlen in numerischer Reihenfolge und Datumsangaben in chronologischer Reihenfolge.
-   Um ein Kombinationsfeld zu erstellen, das live aktualisiert wird, wenn der Benutzer die Pfeiltasten (z.B. eine Dropdown-Liste für die Schriftauswahl) verwendet, müssen Sie SelectionChangedTrigger auf „Immer“ einstellen.  

### <a name="text-search"></a>Textsuche

Kombinationsfelder unterstützen automatisch die Suche in ihren Sammlungen. Wenn ein Benutzer über eine physische Tastatur Zeichen eingibt, während sich der Fokus auf einem geöffneten oder geschlossenen Kombinationsfeld befindet, werden Vorschläge angezeigt, die der vom Benutzer eingegebenen Zeichenfolge entsprechen. Diese Funktionalität ist besonders bei der Navigation durch eine lange Liste nützlich. Beispielsweise können Benutzer bei der Interaktion mit einer Dropdownliste, die eine Liste von Bundesstaaten enthält, die Taste „w“ drücken, um „Washington“ anzuzeigen und diesen Bundesstaat schnell auswählen zu können.


## <a name="list-boxes"></a>Listenfelder

In einem Listenfeld kann der Benutzer ein einzelnes Element oder mehrere Elemente aus einer Auflistung auswählen. Listenfelder ähneln Dropdownlisten, abgesehen davon, dass Listenfelder immer geöffnet sind; für ein Listenfeld gibt es keinen kompakten Zustand (nicht erweitert). Elemente in einem Listenfeld können gescrollt werden, wenn der Platz nicht ausreicht, um alle Elemente anzuzeigen.

### <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

-   Ein Listenfeld kann nützlich sein, wenn Elemente in der Liste so relevant sind, dass sie auffälliger dargestellt werden sollten, und genügend Platz auf dem Bildschirm zum Anzeigen der vollständigen Liste vorhanden ist.
-   Ein Listenfeld sollte den Benutzer bei einer wichtigen Entscheidung bzw. Auswahl auf alle Alternativen aufmerksam machen. Im Gegensatz dazu lenkt eine Dropdownliste die Aufmerksamkeit des Benutzers zunächst auf das ausgewählte Element.
-   Vermeiden Sie die Verwendung eines Listenfelds in folgenden Fällen:
    -   Es gibt eine sehr kleine Anzahl von Elementen für die Liste. Ein Einzelauswahl-Listenfeld, das immer zwei identische Optionen enthält, sollte besser in Form von [Optionsfeldern](radio-button.md) dargestellt werden. Ziehen Sie die Verwendung von Optionsfeldern ebenfalls in Erwägung, wenn die Liste drei oder vier statische Elemente enthält.
    -   Das Listenfeld ist eine Einzelauswahl und enthält immer die gleichen zwei Optionen, wobei eine als „nicht die andere Option“ impliziert werden kann (beispielsweise „Ein“ und „Aus“). Verwenden Sie ein einzelnes Kontrollkästchen oder einen Umschalter.
    -   Die Anzahl von Elementen ist sehr hoch. Bessere Optionen für lange Listen sind Rasteransicht und Listenansicht. Für sehr lange Listen mit gruppierten Daten wir der semantische Zoom bevorzugt.
    -   Bei den Elementen handelt es sich um zusammenhängende numerische Werte. Wenn dies der Fall ist, sollten Sie einen [Schieberegler](slider.md) in Erwägung ziehen.
    -   Die Auswahlelemente sind im Fluss Ihrer App von sekundärer Bedeutung, oder für die meisten Benutzer wird in den meisten Situationen die Standardoption empfohlen. Verwenden Sie stattdessen eine Dropdownliste.

### <a name="recommendations"></a>Empfehlungen

-   Der ideale Bereich von Elementen in einem Listenfeld beträgt 3 bis 9.
-   Ein Listenfeld funktioniert gut, wenn die Elemente darin dynamisch variieren können.
-   Legen Sie die Größe eines Listenfelds möglichst so fest, dass für die Liste der zugehörigen Elemente keine Verschiebung und kein Scrollen erforderlich sind.
-   Stellen Sie sicher, dass der Zweck des Listenfelds und die momentan ausgewählten Elemente klar sind.
-   Reservieren Sie visuelle Effekte und Animationen für das Feedback für die Fingereingabe und für den ausgewählten Zustand von Elementen.
-   Beschränken Sie den Text von Listenfeldelementen auf eine einzelne Zeile. Wenn es sich bei den Elementen um visuelle Objekte handelt, können Sie die Größe anpassen. Wenn ein Element mehrere Textzeilen oder Bilder enthält, verwenden Sie stattdessen eine Raster- oder Listenansicht.
-   Verwenden Sie die standardmäßige Schriftart, sofern Sie gemäß Ihren Markenrichtlinien keine andere verwenden müssen.
-   Verwenden Sie ein Listenfeld nicht zum Ausführen von Befehlen oder zum dynamischen Anzeigen oder Ausblenden anderer Steuerelemente.

## <a name="selection-mode"></a>Auswahlmodus

Mit dem Auswahlmodus können Benutzer ein einzelnes oder mehrere Elemente auswählen und dafür Aktionen vornehmen. Er kann über ein Kontextmenü aufgerufen werden, indem Sie bei gedrückter STRG- oder UMSCHALTTASTE auf ein Element klicken oder in einer Fotogalerieansicht bei einem Element auf ein Ziel zeigen. Wenn der Auswahlmodus aktiviert ist, werden Kontrollkästchen neben jedem Listenelement angezeigt, und Aktionen können am oberen oder unteren Bildschirmrand angezeigt werden.

Es gibt drei verschiedene Auswahlmodi:

-   Einzeln: Dabei kann der Benutzer jeweils nur ein Element auswählen.
-   Mehrfach: Der Benutzer kann mehrere Elemente ohne Modifizierer auswählen.
-   Erweitert: Dabei kann der Benutzer mit Zusatztasten mehrere Elemente auswählen, z.B. durch Gedrückthalten der UMSCHALTTASTE.

Durch Tippen auf ein Element wird es ausgewählt. Das Tippen auf die Aktion auf der Befehlsleiste wirkt sich auf alle ausgewählten Elemente aus. Wenn kein Element ausgewählt ist, sind die Aktionen auf der Befehlsleiste mit Ausnahme von „Alle auswählen“ in der Regel inaktiv.

Im Auswahlmodus funktioniert das einfache Ausblenden nicht. Wenn Sie außerhalb des Frames tippen, in dem der Auswahlmodus aktiv ist, wird der Modus nicht beendet. Dadurch wird die unbeabsichtigte Deaktivierung des Modus verhindert. Per Klick auf die Zurück-Schaltfläche wird der Mehrfachauswahlmodus beendet.

Wenn eine Aktion ausgewählt wurde, wird eine visuelle Bestätigung angezeigt. Ziehen Sie die Anzeige eines Bestätigungsdialogfelds für bestimmte Aktionen in Erwägung, insbesondere bei destruktiven Aktionen wie Löschen.

Der Auswahlmodus ist auf die Seite beschränkt, auf der er aktiv ist. Er wirkt sich nicht auf Elemente außerhalb dieser Seite aus.

Der Einstiegspunkt für den Auswahlmodus sollte neben dem Inhalt platziert werden, auf den er sich auswirkt.

Empfehlungen für die Befehlsleiste finden Sie unter [Richtlinien für Befehlsleisten](app-bars.md).

## <a name="globalization-and-localization-checklist"></a>Prüfliste für Globalisierung und Lokalisierung

<table>
<tr>
<th>Umbruch</th><td>Zwei Zeilen für die Listenbeschriftung zulassen.</td>
</tr>
<tr>
<th>Horizontale Erweiterung</th><td>Stellen Sie sicher, dass die Felder sich an Texte unterschiedlicher Längen anpassen können und stets ein Bildlauf möglich ist.</td>
</tr>
<tr>
<th>Vertikaler Abstand</th><td>Verwenden Sie nicht lateinische Zeichen für vertikalen Abstand, um sicherzustellen, dass nicht lateinische Schriften richtig angezeigt werden.</td>
</tr>
</table>


## <a name="related-articles"></a>Verwandte Artikel

- [Hub](hub.md)
- [Master/Details](master-details.md)
- [Navigationsbereich](navigationview.md)
- [Semantischer Zoom](semantic-zoom.md)
- [Drag&Drop](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [Miniaturbilder](../../files/thumbnails.md)

**Für Entwickler**
- [ListView-Klasse](https://msdn.microsoft.com/library/windows/apps/br242878)
- [GridView-Klasse](https://msdn.microsoft.com/library/windows/apps/br242705)
- [ComboBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br209348)
- [ListBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br242868)