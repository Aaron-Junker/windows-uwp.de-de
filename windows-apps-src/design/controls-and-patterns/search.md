---
Description: Die Suche ist eine der besten Möglichkeiten, um Inhalte in Ihrer App zu finden. In diesem Artikel werden Elemente der Suche sowie Suchbereiche, die Implementierung und Beispiele für die Suche im Kontext behandelt.
title: Suche und „Auf Seite suchen“
ms.assetid: C328FAA3-F6AE-4970-8372-B413F1290C39
label: Search
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: caf0e8e63716f6ba140ef9346257687f0e7293bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631005"
---
# <a name="search-and-find-in-page"></a>Suche und „Auf Seite suchen“

 

Die Suche ist eine der besten Möglichkeiten, um Inhalte in Ihrer App zu finden. In diesem Artikel werden Elemente der Suche sowie Suchbereiche, die Implementierung und Beispiele für die Suche im Kontext behandelt.

> **Wichtige APIs:** [AutoSuggestBox-Klasse](https://msdn.microsoft.com/library/windows/apps/dn633874)

## <a name="elements-of-the-search-experience"></a>Elemente der Suche


**Eingabe.**    Text ist der am häufigsten verwendeten Modus der Sucheingabe und bildet den Schwerpunkt dieses Leitfadens. Andere gängige Eingabemodi sind Spracheingabe und Kamera. Für diese sind jedoch in der Regel eine Verknüpfung mit der Gerätehardware sowie ggf. zusätzliche Steuerelemente oder benutzerdefinierte UI-Elemente in der App erforderlich.

**Keine Eingabe.**    Sobald der Benutzer hat das Eingabefeld aktiviert, aber bevor der Benutzer Text eingegeben wurde, können Sie anzeigen, sogenannte "keine Eingabe Canvas." Die Nulleingabe-Canvas wird häufig in der App-Canvas angezeigt, sodass der Inhalt durch [einen automatischen Vorschlag](auto-suggest-box.md) ersetzt wird, wenn der Benutzer mit der Eingabe der Abfrage beginnt. Für den Nulleingabezustand eignen sich beispielsweise der aktuelle Suchverlauf, populäre Suchabfragen, kontextbezogene Suchvorschläge sowie Hinweise und Tipps.

![Beispiel für Cortana mit Nulleingabe-Canvas](images/search-cortana-example.png)

 

**Fragen Sie die Formulierung/automatisch vorschlagen.**    Abfrage Formulierung NULL eingegebenen Inhalt ersetzt, sobald der Benutzer beginnt, Daten eingegeben. Während der Eingabe einer Abfragezeichenfolge werden dem Benutzer kontinuierlich aktualisierte Abfragevorschläge oder Optionen zur Mehrdeutigkeitsvermeidung angezeigt, um die Eingabe zu beschleunigen und ihn bei der Formulierung einer geeigneten Abfrage zu unterstützen. Dieses Verhalten von Abfragevorschlägen ist in das [Steuerelement für automatische Vorschläge](auto-suggest-box.md) integriert und ermöglicht auch die Symbolanzeige innerhalb der Suche (beispielsweise ein Mikrofon oder ein Commit-Symbol). Jedes andere Verhalten muss von der App behandelt werden.

![example of query/formulation einen automatischen Vorschlag](images/search-autosuggest-example.png)

 

**Legen Sie die Ergebnisse.**    Die Suchergebnisse werden häufig direkt in das Eingabefeld für die Suche. Dies ist zwar nicht zwingend erforderlich, die Gegenüberstellung von Eingabe und Ergebnissen bewahrt jedoch den Kontext und ermöglicht dem Kunden eine direkte Bearbeitung der vorherigen Abfrage oder die Eingabe einer neuen Abfrage. Dieser Zusammenhang kann noch weiter verdeutlicht werden, indem der Hinweistext durch die Abfrage ersetzt wird, auf die der Ergebnissatz zurückzuführen ist.

Eine Möglichkeit für eine wirksame Bearbeitung der vorherigen Abfrage oder die Eingabe einer neuen Abfrage besteht darin, bei Reaktivierung des Felds die vorherige Abfrage hervorzuheben. In diesem Fall ersetzt jede Tastatureingabe die vorherige Zeichenfolge, die Zeichenfolge bleibt jedoch erhalten, sodass der Benutzer einen Cursor platzieren und die vorherige Zeichenfolge bearbeiten oder anhängen kann.

Der Ergebnissatz kann zur bestmöglichen Vermittlung des Inhalts in einem beliebigen Format angezeigt werden: Eine [Listenansicht](lists.md) ist ziemlich flexibel und für die meisten Suchvorgänge geeignet. Eine Rasteransicht eignet sich gut für Bilder oder andere Medien, und eine Karte gibt Aufschluss über räumliche Verteilung.

## <a name="search-scopes"></a>Suchbereiche


Die Suche ist ein gängiges Feature, und die entsprechenden UI-Elemente begegnen dem Benutzer sowohl in der Shell als auch in vielen Apps. Sucheinstiegspunkte werden zwar in der Regel auf ähnliche Weise visualisiert, können aber Zugriff auf verschiedene Ergebnisse bieten – von breit gefächerten (Web- oder Gerätesuche) bis hin zu spezifischen Ergebnissen (Kontaktliste eines Benutzers). Der Sucheinstiegspunkt muss dem gesuchten Inhalt gegenübergestellt werden.

Im Anschluss finden Sie einige gängige Suchbereiche:

**Globale** und **kontextbezogene/Optimierung.**   Suche auf mehrere Quellen mit Cloud- und lokalen Inhalt. Zu den unterschiedlichen Ergebnisse zählen beispielsweise URLs, Dokumente, Medien, Aktionen und Apps.

**Web.**    Finden Sie einen Web-Index. Die Ergebnisse können unter anderem Seiten, Entitäten und Antworten umfassen.

**Meine Produkte sind.**    Suche über Geräte, Cloud, soziale Graphen und vieles mehr. Die Ergebnisse können variieren, sind jedoch durch die Verbindung mit Benutzerkonten eingegrenzt.

Verwenden Sie Hinweistext, um den Suchbereich zu kommunizieren. Dazu gehören:

„Windows und das Web durchsuchen“

„Kontaktliste durchsuchen“

„Postfach durchsuchen“

„Einstellungen durchsuchen“

„Ort suchen“

![Hinweistextbeispiel für die Suche](images/search-windowsandweb.png)

 

Durch eine effektive Vermittlung des Bereichs für den Sucheingabepunkt können Sie sicherstellen, dass die Suchfunktionen die Erwartungen erfüllen, die der Benutzer an sie stellt, und dass er sich nicht über die Suche ärgert.

## <a name="implementation"></a>Implementierung


Bei den meisten Apps empfiehlt sich die Verwendung eines Textfelds als Sucheinstiegspunkt, da es sich hierbei um ein markantes visuelles Element handelt. Darüber hinaus können Sie mit Hinweistext dazu beitragen, dass die Suchfunktion leichter zu finden und der Benutzer über den Suchbereich informiert ist. Wenn es sich bei der Suche eher um eine sekundäre Aktion handelt oder nur wenig Platz zur Verfügung steht, kann das Suchsymbol auch ohne das dazugehörige Eingabefeld als Einstiegspunkt verwendet werden. Achten Sie bei Verwendung eines Symbols darauf, dass genügend Platz für ein modales Suchfeld verfügbar ist, wie in den Beispielen weiter unten zu sehen.

Vor dem Klicken auf das Suchsymbol:

![example of a search icon und collapsed search box](images/search-icon-collapsed.png)

 

Nach dem Klicken auf das Suchsymbol:

![example of a search icon und expunded search box](images/search-icon-expanded.png)

 

Als Einstiegspunkt für die Suche wird immer ein nach rechts geneigtes Lupensymbol verwendet. Dabei ist das Symbol mit dem Hexadezimalzeichencode 0xE0094 (Segoe UI Symbol; üblicherweise mit einem Schriftgrad von 15 Epx) zu verwenden.

Der Sucheinstiegspunkt kann in verschiedenen Bereichen platziert werden und so Aufschluss über Suchbereich und -kontext geben. Suchfunktionen, die übergreifende Ergebnisse für eine Umgebung oder Ergebnisse außerhalb der App sammeln, befinden sich üblicherweise auf der höchsten App-Chromebene. Hierzu zählen beispielsweise globale Befehlsleisten oder Navigationselemente.

Mit zunehmender Eingrenzung oder zunehmendem Kontextbezug des Suchbereichs wird die Platzierung in der Regel direkter mit dem zu suchenden Inhalt assoziiert (beispielsweise auf einer Canvas, bei einer Listenüberschrift oder innerhalb kontextbezogener Befehlsleisten). In jedem Fall muss der Zusammenhang zwischen Sucheingabe und Ergebnissen oder gefilterten Inhalten optisch deutlich gemacht werden.

Bei bildlauffähigen Listen empfiehlt es sich, darauf zu achten, dass die Sucheingabe immer sichtbar ist. Wir empfehlen, die Sucheingabe zu verankern und so zu konfigurieren, dass der Bildlauf im Hintergrund stattfindet.

Die Funktion für Nulleingabe und Abfrageformulierung ist bei kontextbezogenen/eingegrenzten Suchvorgängen, bei denen die Liste in Echtzeit auf der Grundlage der Benutzereingabe gefiltert wird, optional. Hiervon ausgenommen sind Fälle, bei denen möglicherweise Abfrageformatierungsvorschläge verfügbar sind. Hierzu zählen beispielsweise Filteroptionen für den Posteingang (An:&lt;Eingabezeichenfolge&gt;, Von: &lt;Eingabezeichenfolge&gt;, Betreff: &lt;Eingabezeichenfolge&gt; usw.).

## <a name="example"></a>Beispiel


Die Beispiele in diesem Abschnitt zeigen die Suche im Kontext.

Suche als Aktion auf der Symbolleiste von Windows:

![Beispiel für die Suche als Aktion auf der Symbolleiste von Windows](images/search-toolbar-action.png)

 

Suche als Eingabe auf der App-Canvas:

![Beispiel für die Suche auf der App-Canvas](images/search-canvas-contacts.png)

 

Suche in einem Navigationsbereich:

![Beispiel für die Suche in einem Navigationsmenü](images/search-navmenu.png)

 

Inlinesuche eignet sich am besten für seltener verwendete oder stark kontextorientierte Suchvorgänge:

![Beispiel für eine Inlinesuche](images/patterns-search-results-desktop.png)


## <a name="guidelines-for-find-in-page"></a>Richtlinien für die Funktion „Auf Seite suchen“


Die Funktion „Auf Seite suchen“ bietet Benutzern die Möglichkeit, im aktuellen Text nach Übereinstimmungen zu suchen. Dokumentviewer, Leser und Browser sind die typischsten Apps, die die Funktionalität „Auf Seite suchen“ bereitstellen.

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


-   Platzieren Sie eine Befehlsleiste in Ihrer App mit der Funktionalität „Auf Seite suchen“, damit Benutzer Suchvorgänge für Texte auf einer Seite ausführen können. Platzierungsdetails dazu finden Sie im Abschnitt „Beispiele“.

    -   In Apps, die die Funktion „Auf Seite suchen“ zur Verfügung stellen, sollten sich alle notwendigen Steuerelemente in einer Befehlsleiste befinden.
    -   Wenn Ihre App eine große Anzahl an Funktionen bereitstellt, die über "Auf Seite suchen" hinausgehen, können Sie eine **Suchschaltfläche** in der Befehlsleiste oberster Ebene als Einstiegspunkt zu einer weiteren Befehlsleiste Verfügung stellen, die alle Steuerelemente enthält, die zur Funktion "Auf Seite suchen" gehören.
    -   Die Befehlsleiste für „Auf Seite suchen“ sollte während einer Interaktion des Benutzers mit der Bildschirmtastatur sichtbar bleiben. Die Bildschirmtastatur wird angezeigt, wenn ein Benutzer auf das Eingabefeld tippt. Die Befehlsleiste für „Auf Seite suchen“ sollte nach oben rücken, damit sie nicht von der Bildschirmtastatur verdeckt wird.

    -   "Auf Seite suche" sollte während einer Benutzerinteraktion mit der Ansicht verfügbar bleiben. Benutzer müssen mit dem momentan angezeigten Text interagieren, während sie die Funktion "Auf Seite suchen" verwenden. Es sollte den Benutzern beispielsweise möglich sein, in ein Dokument hinein- oder herauszuzoomen oder die Ansicht zu verschieben, um den Text zu lesen. Sobald der Benutzer mit der Verwendung von „Auf Seite suchen“ beginnt, sollte die Befehlsleiste verfügbar bleiben und die Schaltfläche **Schließen** bereitstellen, damit die Funktion "Auf Seite suchen" beendet werden kann.

    -   Aktivieren Sie die Tastenkombination (STRG+F). Implementieren Sie die Tastenkombination STRG+F, um das schnelle Öffnen der Befehlsleiste für „Auf Seite suchen“ zu ermöglichen.

    -   Binden Sie die Basiselemente der Funktion "Auf Seite suchen" ein. Hierbei handelt es sich um die UI-Elemente, die benötigt werden, um „Auf Seite suchen“ zu implementieren.

        -   Eingabefeld
        -   Die Schaltflächen „Zurück“ und „Weiter“
        -   Übereinstimmungszähler
        -   Schließen (nur Desktop)
    -   In der Ansicht sollten Übereinstimmungen hervorgehoben werden, und es sollte ein Bildlauf erfolgen, sodass die nächste Übereinstimmung auf dem Bildschirm angezeigt wird. Die Schaltflächen **Zurück** und **Weiter** sowie Bildlaufleisten oder die direkte Manipulation mittels Fingereingabe ermöglichen den Benutzern die schnelle Navigation innerhalb des Dokuments.

    -   Funktionalität zum Suchen und Ersetzen sollte mit den Basisfunktionen von "Auf Seite suchen" zusammenarbeiten. Sorgen Sie bei Apps, die eine Funktion zum Suchen und Ersetzen bereitstellen, dafür, dass sich diese Funktion und „Auf Seite suchen“ nicht gegenseitig behindern.

-   Fügen Sie einen Übereinstimmungszähler ein, damit der Benutzer weiß, wie viele Textübereinstimmungen auf der Seite vorhanden sind.
-   Aktivieren Sie die Tastenkombination (STRG+F).

## <a name="examples"></a>Beispiele


Stellen Sie eine einfache Möglichkeit zur Verfügung, um das Feature „Auf Seite suchen“ zuzugreifen. In diesem Beispiel auf einer mobilen Benutzeroberfläche wird „Auf Seite suchen“ hinter zwei „Hinzufügen zu…“-Befehlen in einem erweiterbarem Menü angezeigt:

![„Auf Seite suchen“ (Beispiel 1)](images/findinpage-01.png)

 

Nach der Auswahl von „Auf Seite suchen“ gibt der Benutzer einen Suchbegriff ein. Textvorschläge können angezeigt werden, wenn ein Suchbegriff eingegeben wird:

![„Auf Seite suchen“ (Beispiel 2)](images/findinpage-02.png)

 

Wenn keine Textübereinstimmung in der Suche vorliegt, sollte die Textzeichenfolge „Keine Ergebnisse“ im Ergebnisfeld angezeigt werden:

![„Auf Seite suchen“ (Beispiel 3)](images/findinpage-03.png)

 

Wenn eine Textübereinstimmung in der Suche vorliegt, sollte der erste Begriff in einer eindeutigen Farbe hervorgehoben werden, wobei die nachfolgenden Übereinstimmungen in einem dezenteren Ton derselben Farbpalette gehalten werden sollten, wie dies in diesem Beispiel ersichtlich ist:

![„Auf Seite suchen“ (Beispiel 4)](images/findinpage-04.png)

 

„Auf Seite suchen“ weist einen Übereinstimmungszähler auf:

![Beispiel des Suchindikators von „Auf Seite suchen“](images/findinpage-counter.png)




## <a name="implementing-find-in-page"></a>**Find-in-Page-Implementierung**

-   Bei Dokumentviewern, Lesern und Browsern handelt es sich um die wahrscheinlichsten App-Typen für die Bereitstellung von „Auf Seite suchen“, was Benutzern eine vollständige Bildschirmanzeige-/-leseerfahrung ermöglicht.
-   Die Funktion „Auf Seite suchen“ ist sekundär und sollte auf einer Befehlsleiste platziert werden.

Weitere Informationen zum Hinzufügen von Befehlen zur Befehlsleiste finden Sie unter [Befehlsleiste](app-bars.md).

 


## <a name="related-articles"></a>Verwandte Artikel

* [Feld mit automatischen Vorschlägen](auto-suggest-box.md)


 

 
