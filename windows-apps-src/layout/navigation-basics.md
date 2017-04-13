---
author: mijacobs
Description: "Die Navigation in UWP-Apps (Apps für die universelle Windows-Plattform) basiert auf einem flexiblen Modell aus Navigationsstrukturen, Navigationselementen und Funktionen auf Systemebene."
title: "Navigationsgrundlagen für UWP-Apps (Windows-Apps)"
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
ms.openlocfilehash: 6397949c4c763db9d406790a6ffcb7f8ad94b7aa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#  <a name="navigation-design-basics-for-uwp-apps"></a>Navigationsdesigngrundlagen für UWP-Apps

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Die Navigation in UWP-Apps (Apps für die universelle Windows-Plattform) basiert auf einem flexiblen Modell aus Navigationsstrukturen, Navigationselementen und Funktionen auf Systemebene. Gemeinsam ermöglichen sie eine Reihe intuitiver Benutzererfahrungen für die Navigation zwischen Apps, Seiten und Inhalten.

In einigen Fällen kann es möglich sein, alle Inhalte und Funktionen der App auf einer einzelnen Seite anzuordnen, ohne dass der Benutzer mehr tun muss, als durch Verschieben in den Inhalten zu navigieren. Die meisten Apps verfügen jedoch in der Regel über mehrere Seiten mit Inhalten und Funktionen, die der Benutzer aufrufen und mit denen er interagieren kann. Wenn eine App mehrere Seiten hat, müssen Sie die geeignete Navigationsfunktionalität bereitstellen.

Damit sie fehlerlos und intuitiv von den Benutzern verwendet werden kann, umfasst die mehrseitige Navigation in UWP-Apps Folgendes (später ausführlich beschrieben):

-   **Die richtige Navigationsstruktur**

    Das Erstellen einer für Benutzer sinnvollen Navigationsstruktur ist für die Entwicklung einer intuitiven Navigationsfunktionalität entscheidend.

-   **Kompatible Navigationselemente**, die die ausgewählte Struktur unterstützen.

    Navigationselemente können dem Benutzer helfen, zum gewünschten Inhalt zu gelangen, und sie können auch anzeigen, wo sich der Benutzer in der App befindet. Allerdings belegen sie auch Platz, der für Inhalte oder Steuerungselemente genutzt werden könnte. Daher ist es wichtig, dass Sie die für Ihre App-Struktur geeigneten Navigationselemente verwenden.

-   **Geeignete Reaktionen auf Navigationsfeatures auf Systemebene (z.B. „Zurück”)**

    Um eine einheitliche intuitive Benutzererfahrung zu bieten, reagieren Sie in vorhersehbarer Weise auf Navigationsfeatures auf Systemebene.

## <a name="build-the-right-navigation-structure"></a>Erstellen der richtigen Navigationsstruktur


Betrachten Sie eine App als eine Sammlung von Seitengruppen, wobei jede Seite einen einzigartigen Satz von Inhalten oder Funktionen enthält. So besitzt eine Foto-App beispielsweise eine Seite für die Aufnahme von Fotos, eine Seite für die Bildbearbeitung und eine andere Seite für die Verwaltung Ihrer Bildbibliothek. Die Anordnung dieser Seiten in Gruppen definiert die Navigationsstruktur der App. Es gibt zwei allgemeine Methoden zum Anordnen einer Gruppe von Seiten:

<table class="uwpd-noborder uwpd-top-aligned-table">
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">In einer Hierarchie</th>
<th align="left">Als Peers</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: center;"><p><img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" /></p></td>
<td style="text-align: center;"><p><img src="images/nav/nav-pages-peer.png" alt="Pages arranged as peers" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align: top">Seiten sind in einer Baumstruktur organisiert. Jede untergeordnete Seite hat nur ein übergeordnetes Element. Ein übergeordnetes Element kann jedoch eine oder mehrere untergeordnete Seiten haben. Um eine untergeordnete Seite aufzurufen, durchlaufen Sie das übergeordnete Element. </td>
<td style="vertical-align: top"> Die Seiten existieren nebeneinander. Sie können in beliebiger Reihenfolge von einer Seite zur nächsten wechseln. </td>
</tr>
</tbody>
</table>

 

Eine typische App verwendet beide Anordnungen, wobei einige Teile als Peers und einige Teile in Hierarchien angeordnet sind.

![App mit einer Hybridstruktur](images/nav/nav-hybridstructure.png.png)

Wann sollten Sie also Seiten in Hierarchien und wann als Peers anordnen? Zur Beantwortung dieser Frage müssen Sie die Anzahl der Seiten in der Gruppe berücksichtigen. Sie müssen feststellen, ob die Seiten in einer bestimmten Reihenfolge durchlaufen werden, und Sie müssen die Beziehung zwischen den Seiten ermitteln. Im Allgemeinen sind flachere Strukturen einfacher zu verstehen und schneller zu navigieren, aber manchmal ist es angemessen, eine tiefe Hierarchie zu verwenden.



<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">Wir empfehlen eine hierarchische Beziehung in folgenden Fällen:
<ul>
<li>Sie erwarten, dass der Benutzer die Seiten in einer bestimmten Reihenfolge durchlaufen wird. Sie ordnen die Hierarchie entsprechend an, um die Reihenfolge zu erzwingen.</li>
<li>Es gibt eine klare Beziehung zwischen einer Seite als übergeordnetem Element und den anderen Seiten in der Gruppe als untergeordneten Elementen.</li>
<li>In der Gruppe gibt es mehr als 7 Seiten.
<p>Wenn eine Gruppe mehr als 7Seiten enthält, wird es für Benutzer möglicherweise schwierig, zu verstehen, inwiefern sich die Seiten unterscheiden oder welche Position sie zurzeit in der Gruppe haben. Wenn Sie davon ausgehen, dass dies kein Problem für Ihre App ist, machen Sie aus den Seiten Peers. Ziehen Sie andernfalls eine hierarchische Struktur in Betracht, um die Seiten in zwei oder mehr kleinere Gruppen zu unterteilen. (Ein Hub-Steuerelement kann Ihnen dabei helfen, die Seiten in Kategorien zu gruppieren.)</p></li>
</ul>
  </div>
  <div class="side-by-side-content-right">Wir empfehlen eine hierarchische Peer-Beziehung in folgenden Fällen:
<ul>
<li>Die Seiten können in beliebiger Reihenfolge angezeigt werden.</li>
<li>Die Seiten sind deutlich voneinander abgegrenzt und verfügen nicht über eine offensichtliche Beziehung zwischen über- und untergeordneten Elementen.</li>
<li><p>Es gibt weniger als acht Seiten in der Gruppe.</p>
<p>Wenn eine Gruppe mehr als 7Seiten enthält, wird es für Benutzer möglicherweise schwierig, zu verstehen, inwiefern sich die Seiten unterscheiden oder welche Position sie zurzeit in der Gruppe haben. Wenn Sie davon ausgehen, dass dies kein Problem für Ihre App ist, machen Sie aus den Seiten Peers. Ziehen Sie andernfalls eine hierarchische Struktur in Betracht, um die Seiten in zwei oder mehr kleinere Gruppen zu unterteilen. (Ein Hub-Steuerelement kann Ihnen dabei helfen, die Seiten in Kategorien zu gruppieren.)</p></li>
</ul>
  </div>
</div>
</div>
 

## <a name="use-the-right-navigation-elements"></a>Verwenden der richtigen Navigationselementen


Navigationselemente können zwei Aufgaben erfüllen: Dadurch kann der Benutzer den gewünschten Inhalt abrufen, und einige Elemente zeigen Benutzern auch an, wo sie sich in der App befinden. Allerdings benötigen diese auch Platz, den die App für Inhalte oder Steuerungselemente nutzen könnte, daher ist es wichtig, dass Sie die für Ihre App-Struktur geeigneten Navigationselemente verwenden.

### <a name="peer-to-peer-navigation-elements"></a>Peer-to-Peer-Navigationselemente

Peer-zu-Peer-Navigationselemente ermöglichen die Navigation zwischen Seiten auf der gleichen Ebene in der gleichen Unterstruktur.

![Peer-to-Peer-Navigation](images/nav/nav-lateralmovement.png)

Bei der Peer-to-Peer-Navigation empfehlen wir die Nutzung von Registerkarten oder eines Navigationsbereichs.

<table>
<thead>
<tr class="header">
<th align="left">Navigationselement</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">[Registerkarten und Pivot](../controls-and-patterns/tabs-pivot.md)
<p><img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></p></td>
<td style="vertical-align:top;">Zeigt eine dauerhafte Liste mit Links zu Seiten auf derselben Ebene an.
<p>Verwenden Sie in folgenden Fällen Registerkarten/Pivots:</p>
<ul>
<li><p>Es sind 2 bis 5 Seiten vorhanden.</p>
<p>(Sie können Registerkarten/Pivots bei mehr als fünf Seiten verwenden, es kann aber schwierig sein, alle Registerkarten/Pivots auf dem Bildschirm anzuzeigen.)</p></li>
<li>Sie erwarten, dass Benutzer häufig zwischen Seiten wechseln werden.</li>
</ul>
<p>Dieser Entwurf für eine App, die Restaurants findet, verwendet Registerkarten/Pivots:</p>
<p><img src="images/food-truck-finder/uap-foodtruck-tabletphone-sbs-sm-400.png" alt="Example of an app using tabs/pivots pattern" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;">[Navigationsbereich](../controls-and-patterns/nav-pane.md)
<p><img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></p></td>
<td style="vertical-align:top;">Zeigt eine Liste mit Links zu den übergeordneten Seiten an.
<p>Verwenden Sie in folgenden Fällen einen Navigationsbereich:</p>
<ul>
<li>Sie erwarten nicht, dass Benutzer häufig zwischen Seiten wechseln werden.</li>
<li>Sie möchten Platz sparen, wobei Sie eine Verlangsamung der Navigationsvorgänge in Kauf nehmen.</li>
<li>Die Seiten befinden sich auf der obersten Ebene.</li>
</ul>
<p>Dieser Entwurf einer Smart Home-App enthält einen Navigationsbereich:</p>
<p><img src="images/smart-home/uap-smarthome-tabletphone-sbs-sm-400.png" alt="Example of an app that uses a nav pane pattern" /></p>
<p></p></td>
</tr>
</tbody>
</table>

 

Wenn die Navigationsstruktur über mehrere Ebenen verfügt, empfehlen wir, dass Peer-to-Peer-Navigationselemente nur mit den Peers innerhalb der aktuellen Unterstruktur verknüpft sind. Beachten Sie die folgende Abbildung, die eine Navigationsstruktur mit drei Ebenen zeigt:

![App mit zwei Unterstrukturen](images/nav/nav-subtrees.png)
-   Auf Ebene 1 sollte das Peer-to-Peer-Navigationselement Zugriff auf die Seiten A, B, C und D ermöglichen.
-   Auf Ebene 2 sollten die Peer-to-Peer Navigationselemente für die A2-Seiten nur mit den anderen A2-Seiten verknüpft werden. Sie sollten nicht mit Seiten auf Ebene 2 in der C-Unterstruktur verknüpft sein.

![App mit zwei Unterstrukturen](images/nav/nav-subtrees2.png)

### <a name="hierarchical-navigation-elements"></a>Hierarchische Navigationselemente

Hierarchische Navigationselemente ermöglichen die Navigation zwischen einer übergeordneten Seite und den untergeordneten Seiten.

![Hierarchische Navigation](images/nav/nav-verticalmovement.png)

<table>
<thead>
<tr class="header">
<th align="left">Navigationselement</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">[Hub](../controls-and-patterns/hub.md)
<p><img src="images/higsecone-hub-thumb.png" alt="Hub" /></p></td>
<td align="left">Ein Hub ist eine spezielle Art von Navigationssteuerelement, das eine Vorschau/Übersicht der untergeordneten Seiten bereitstellt. Im Gegensatz zum Navigationsbereich oder den Registerkarten ermöglicht es die Navigation zu diesen untergeordneten Seiten über in die Seite selbst eingebettete Links und Abschnittsüberschriften.
<p>Verwenden Sie in folgenden Fällen einen Hub:</p>
<ul>
<li>Sie gehen davon aus, dass Benutzer einen Teil des Inhalts der untergeordneten Seiten anzeigen möchten, ohne zu jeder einzeln navigieren zu müssen.</li>
</ul>
<p>Hubs fördern das Erkennen und Erforschen, weshalb sie ideal für Medien, Newsreader und Shopping-Apps geeignet sind.</p>
<p></p></td>
</tr>

<tr class="even">
<td style="vertical-align:top;">[Master/Details](../controls-and-patterns/master-details.md)
<p><img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></p></td>
<td align="left">Zeigt eine Liste (Masteransicht) der Elementübersichten an. Durch Auswahl eines Elements wird die entsprechende Elementseite im Detailbereich angezeigt.
<p>Verwenden Sie in folgenden Fällen das Master/Details-Element:</p>
<ul>
<li>Sie erwarten, dass Benutzer häufig zwischen untergeordneten Elementen wechseln werden.</li>
<li>Sie möchten es dem Benutzer ermöglichen, Vorgänge auf hoher Ebene, z. B. Löschen oder Sortieren, für einzelne Elemente oder Gruppen von Elementen durchzuführen, und Sie möchten es dem Benutzer ermöglichen, Details für jedes Element anzuzeigen oder zu aktualisieren.</li>
</ul>
<p>Master/Details-Elemente eignen sich gut für E-Mail-Posteingänge, Kontaktlisten und die Dateneingabe.</p>
<p>Dieser Entwurf für eine Aktien-App verwendet ein Master/Details-Muster:</p>
<p><img src="images/stock-tracker/uap-finance-tabletphone-sbs-sm.png" alt="Example of a stock trading app that has a master/details pattern" /></p></td>
</tr>
</tbody>
</table>

 

### <a name="historical-navigation-elements"></a>Historische Navigationselemente

<table>
<thead>
<tr class="header">
<th align="left">Navigationselement</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">[Zurück](navigation-history-and-backwards-navigation.md)</td>
<td style="vertical-align:top;">Der Benutzer kann den Navigationsverlauf innerhalb einer App und, abhängig vom Gerät, von App zu App zurückverfolgen. Weitere Informationen finden Sie im Artikel [Navigationsverlauf und Rückwärtsnavigation](navigation-history-and-backwards-navigation.md).</td>
</tr>
</tbody>
</table>

 

### <a name="content-level-navigation-elements"></a>Navigationselemente auf Inhaltsebene

<table>
<thead>
<tr class="header">
<th align="left">Navigationselement</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">Hyperlinks und Schaltflächen</td>
<td style="vertical-align:top;">Im Inhalt eingebettete Navigationselemente werden im Inhalt einer Seite angezeigt. Im Gegensatz zu anderen Navigationselementen, die für alle Gruppen oder Unterstrukturen der Seite konsistent sein sollten, sind im Inhalt eingebettete Navigationselemente auf jeder Seite einzigartig.</td>
</tr>
</tbody>
</table>

 

### <a name="combining-navigation-elements"></a>Kombinieren von Navigationselementen

Sie können Navigationselemente kombinieren, um eine für Ihre App geeignete Navigationserfahrung zu erstellen. Ihre App kann beispielsweise einen Navigationsbereich verwenden, um auf Seiten auf oberster Ebene und Registerkarten auf Seiten der zweiten Ebene zuzugreifen.






 




