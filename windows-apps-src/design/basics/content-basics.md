---
Description: Hier erhalten Sie eine Übersicht über gängige Seitenmuster und UI-Elemente für die Darstellung von Inhalten in Ihrer UWP-App.
title: Grundlagen des Inhaltsdesigns für UWP-Apps (Universelle Windows-Plattform)
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.date: 12/01/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0e5b46223ea7dfa5e43b5a50a04a4c3f31307d49
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "63796275"
---
# <a name="content-design-basics-for-uwp-apps"></a>Grundlagen des Inhaltsdesigns für UWP-Apps

Der Hauptzweck einer App besteht darin, den Zugriff auf Inhalte zu gewähren. Da es Apps für viele verschiedene Zwecke gibt, gibt es Inhalte in vielen Formen: In einer Fotobearbeitungs-App ist das Foto der Inhalt; in einer Reiseanwendung sind es Karten und Informationen über Reiseziele, und so weiter. 

Dieser Artikel gibt einen Überblick darüber, wie Sie Inhalte in Ihrer App präsentieren können. Wir beschreiben gängige Seitenmuster und UI-Elemente, die Sie zur Darstellung Ihrer Inhalte verwenden können, egal in welcher Form.

## <a name="common-page-patterns"></a>Allgemeine Seitenmuster

Viele Apps verwenden einige oder alle dieser gängigen Seitenmuster, um verschiedene Arten von Inhalten anzuzeigen. Sie können diese Muster kombinieren, um sie für den Inhalt Ihrer App zu optimieren.

### <a name="landing"></a>Angebotsseite (Landing-Page)

![Angebotsseite (Landing-Page)](images/content-basics/hero-screen.png)

Angebotsseiten werden auch Hero-Screens genannt und werden häufig auf der obersten Ebene einer App-Erfahrung angezeigt. Die große Oberfläche dient als Bühne, um Inhalte hervorzuheben, die Benutzer ansehen und nutzen möchten.

### <a name="collections"></a>Sammlungen

![Galerie](images/content-basics/gridview.png)

Über Sammlungen können Benutzer Gruppen von Inhalten oder Daten suchen. Die [Rasteransicht](../controls-and-patterns/item-templates-gridview.md) ist eine gute Option für Fotos oder medienzentrierte Inhalte, und die [Listenansicht](../controls-and-patterns/item-templates-listview.md) ist eine gute Option für textlastige Inhalte oder Daten.


### <a name="masterdetail"></a>Master/Detail

![Master Detail](images/content-basics/master-detail.png)

Das [Master/Detail](../controls-and-patterns/master-details.md)-Modell besteht aus einer Listenansicht (Master) und einer Inhaltsansicht (Detail). Beide Bereiche sind festgelegt und bieten vertikales Scrollen. Zwischen dem Listenelement und der Inhaltsansicht besteht ein klarer Zusammenhang: Das Element in der Master-Ansicht ist markiert und die Detailansicht wird entsprechend aktualisiert. Zusätzlich zur Navigation in der Detailansicht können Elemente in der Master-Ansicht hinzugefügt und entfernt werden.

### <a name="details"></a>Details

![Mehrere Ansichten](images/multi-view.png)

Wenn Benutzer den gesuchten Inhalt finden, sollten Sie eine eigene Seite zum Anzeigen von Inhalten erstellen, damit die Benutzer die Seite ohne Ablenkungen betrachten können. Wenn möglich, [erstellen Sie eine Vollbildansicht](../layout/show-multiple-views.md), die den Inhalt auf den gesamten Bildschirm erweitert und alle anderen UI-Elemente ausblendet. 

Um sich an Änderungen in der Bildschirmgröße anzupassen, sollten Sie auch ein [dynamisches Design](design-and-ui-intro.md) erstellen, das UI-Elemente entsprechend ausblendet bzw. einblendet.

### <a name="forms"></a>Formulare
![Formular](images/content-basics/forms.png)

Ein [Formular](../controls-and-patterns/forms.md) ist eine Gruppe von Steuerelementen, die Daten von Benutzern sammeln und übermitteln. Die meisten, wenn nicht alle Apps, verwenden eine Art Formular für Einstellungsseiten, Anmeldeportale, Feedback-Hubs, Kontoerstellung oder andere Zwecke. 

## <a name="common-content-elements"></a>Grundlegende Inhaltselemente

Um diese Seitenmuster zu erstellen, müssen Sie eine Kombination aus einzelnen Inhaltselementen verwenden. Hier sind einige UI-Elemente, die häufig zur Anzeige von Inhalten verwendet werden. (Eine vollständige Liste der UI-Elemente finden Sie unter [Steuerelemente und Muster](../controls-and-patterns/index.md).

<div class="mx-responsive-img">
<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Kategorie</th>
<th align="left">Elemente</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Audio und Video<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">Steuerelemente für Medienwiedergabe und -transport</a></td>
<td align="left">Gibt Audio- und Videoinhalte wieder.</td>
</tr>
<tr class="even">
<td align="left">Bild-Viewer<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">Flip-Ansicht</a>, <a href="../controls-and-patterns/images-imagebrushes.md">Bild</a></td>
<td align="left">Zeigt Bilder an. Die „Flip-Ansicht“ zeigt Bilder nacheinander in einer Sammlung an, wie etwa Fotos in einem Album oder Elemente auf einer Produktdetailseite.</td>
</tr>
<tr class="odd">
<td align="left">Sammlungen <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">Listenansicht und Rasteransicht</a></td>
<td align="left">Stellt Elemente in einer interaktiven Liste oder einem Raster dar. Verwenden Sie diese Elemente, um Benutzern die Auswahl eines Films aus einer Liste mit Neuerscheinungen oder die Verwaltung von Inventar zu ermöglichen.</td>
</tr>
<tr class="even">
<td align="left">Text und Texteingabe <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">Textblock</a>, <a href="../controls-and-patterns/text-box.md">Textfeld</a>, <a href="../controls-and-patterns/rich-edit-box.md">Rich-Edit-Feld</a></p>
</td>
<td align="left">Zeigt Text an. Einige Elemente ermöglichen dem Benutzer das Bearbeiten von Text. Weitere Informationen finden Sie unter <a href="../controls-and-patterns/text-controls.md">Textsteuerelemente</a>.
<p>Richtlinien zur Anzeige von Text finden Sie unter <a href="../style/typography.md">Typografie</a>.</p>
</td>
</tr>
<tr class="odd">
<td align="left">Karten<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">Zeigt eine grafische oder fotorealistische Karte der Erde an.</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">Stellt HTML-Inhalte dar.</td>
</tr>
</tbody>
</table>
</div>
