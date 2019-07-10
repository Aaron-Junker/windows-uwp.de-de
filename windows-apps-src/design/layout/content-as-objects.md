---
description: ''
title: Inhalte als Objekte
template: detail.hbs
ms.localizationpriority: medium
ms.date: 04/18/2019
ms.openlocfilehash: b73e595454121fa34b536f3672c75a5c554ee0f3
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714028"
---
# <a name="content-as-objects"></a>Inhalte als Objekte

 

Sie können die Tiefe oder z-Reihenfolge von Elementen manipulieren, um eine visuelle Hierarchie zu erstellen, die die Bedienung Ihrer Anwendung erleichtert.  

> Hinweis: In diesem Artikel handelt es sich um einen frühen Entwurf für ein neues Feature von Windows 10 RS2. Featurenamen, Terminologie und Funktionen sind nicht endgültig. 

## <a name="why-visual-hierarchy-is-important"></a>Warum visuelle Hierarchie wichtig ist

Die Benutzer werden ständig mit Anfragen nach ihrer Aufmerksamkeit bombardiert. Jedes Element auf dem Bildschirm bittet darum, betrachtet zu werden, jeder Text will gelesen werden, jeder Button angeklickt werden. Da die visuelle Umgebung immer chaotischer und chaotischer wird, erfordert es mehr Mühe, die Szene zu analysieren und herauszufinden, was vor sich geht.  

Deshalb ist es so wichtig, die Elemente Ihrer Benutzeroberfläche sorgfältig auszuwählen und ein Layout zu erstellen, das eine klare visuelle Hierarchie zwischen Ihren UI-Elementen herstellt. <!-- Every element is competing for the user's attention, and every time you add an element, you add a mental tax to the user. -->

Eine klare visuelle Hierarchie sagt dem Benutzer, welche Elemente am wichtigsten sind und schafft Beziehungen zwischen den Elementen. Mit einer guten visuellen Hierarchie versteht der Benutzer das Layout der Seite auf einen Blick und kann sich auf seine Aufgabe konzentrieren. 

<p></p>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p>Wie schafft man also eine klare visuelle Hierarchie? In früheren Versionen von Windows 10 konnten Sie Leerraum, Position und Typografie verwenden, um eine visuelle Hierarchie zu definieren. </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/flat-layout.png">Ein Flatfile-layout</a>
    
  </div>
</div>
</div>

Mit Windows 10 RS2 haben wir buchstäblich eine weitere Dimension hinzugefügt: die Tiefe. 

<a href="images/content-as-objects/depth-in-layout2.png">Die Tiefe im layout</a>


## <a name="use-depth-to-establish-a-hierarchy"></a>Aufbau einer Hierarchie über Tiefe 

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
     <p>Sie können die Tiefe (z-Reihenfolge) zusammen mit Ihren anderen Designtools (Leerraum, Position, Typografie) verwenden, um eine Hierarchie aufzubauen. Heben Sie Ihre wichtigsten Elemente auf die vorderste Ebene; verwenden Sie die unteren Ebenen, um eine weniger kritische Benutzeroberfläche anzuzeigen. 

    The relative importance of an element can change throughout an experience, so you can bring elements forward as they become more important and backward as they become less important. 
    </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Die Tiefe im layout</a> 
    
  </div>
</div>
</div>

## <a name="how-does-it-work"></a>Wie funktioniert das?
> TODO: Kurze Beschreibung, wie Sie die Z-Reihenfolge der Elemente steuern können. Ist die z-Reihenfolge explizit fest implementiert, oder gibt es ein semantisches Ranking-System? Wie bewegen sich Objekte von einer Ebene zur anderen? Was macht das System automatisch, und worüber müssen sich Designer/Entwickler Gedanken machen? 

## <a name="the-four-layers-of-a-typical-app-layers"></a>Die vier Ebenen einer typischen App

<p>Eine typische App besteht aus 4 Ebenen.</p>
<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Nach Hintergrund</b> dieser Ebene befindet sich hinter der app.  Wenn Elemente auf diese Ebene verschoben werden, empfehlen wir, sie nicht interaktiv zu gestalten. Elemente auf dieser Ebene haben den langsamsten Parallax-Effekt und werden auf das App-Fenster beschnitten. TODO: Werden diese Ebene skaliert? 

<p>Beispiel-Hintergrund-Elemente enthalten Bild hinter dem Inhalt, TODO: Beispiel: TODO: Beispiel:.</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Abgesehen von der Ebene einer App im Hintergrund</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Passive Ebene</b> Dies ist der Ebene von der app, in dem Elemente der Benutzeroberfläche in der Standardeinstellung Leben.  Elemente bewegen sich auf dieser Ebene in Echtzeit (keine Parallax-Effekt). Sie werden auf das App-Fenster beschnitten und auf 100 % skaliert. 

<p>Beispiel-Elemente: Der app Hintergrund, Text, sekundäre Benutzeroberfläche, z. B. die Benutzeroberfläche der app-Navigation.</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Die passive Ebene einer App</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Aufrufe von Aktion</b> diese Ebene ist für die interaktive Elemente, die Sie über den passiven Ebenenelemente priorisieren. Elemente auf dieser Ebene haben einen mittleren Parallax-Effekt und werden auf das App-Fenster beschnitten. TODO: Führen Sie die Elemente auf dieser Ebene Skala oder aufweisen einen Schlagschatten?

<p>Beispiel-Elemente: Listen, Raster, primären Befehle (TODO: Such As...).</p> 
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Die Call-to-Action-Ebene einer App</a>
    
  </div>
</div>
</div>

<p></p>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Hero-Ebene</b> diese Ebene ist für das Element der höchsten Priorität auf dem Bildschirm, zu dem Zeitpunkt.  Elemente auf dieser Ebene können die Grenzen des App-Fensters sprengen, sie können skalieren, und sie erhalten automatisch einen Schlagschatten.

<p>Beispielelemente: fotografische Elemente, das aktuell ausgewählte Element.</p>  
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Die Hero-Ebene einer App</a>
    
  </div>
</div>
</div>



<!--
Depth is meaningful; it establishes visual and interactive hierarchy for users to efficiently complete tasks. Depth orients users in our system. 
-->

## <a name="example-tbd"></a>Beispiel: TBD
> TODO: Zeigen Sie, wie ein häufiges Benutzeroberfläche, um die Z-Reihenfolge verwenden anpassen. Wir sollten Illustrationen und Code zeigen. 

## <a name="download-the-code-samples"></a>Codebeispiele herunterladen
>TODO: Link zu Beispielen, die diese Funktion veranschaulichen. 


## <a name="related-articles"></a>Verwandte Artikel
* [Inhaltsgrundlagen](../basics/content-basics.md)
