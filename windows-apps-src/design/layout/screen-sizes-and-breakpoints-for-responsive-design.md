---
title: Bildschirmgrößen und Haltepunkte für reaktionsfähiges Design
description: Anstelle einer Optimierung deiner Benutzeroberfläche für die vielen Geräte im gesamten Windows 10-Ökosystem wird empfohlen, ein Design für einige Schlüsselbreiten (sogenannte Breakpoints) zu erstellen.
ms.date: 08/30/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 37d0ca71adf43891628a02d60d6873e7934d749b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "79210176"
---
#  <a name="screen-sizes-and-breakpoints"></a>Bildschirmgrößen und Haltepunkte

UWP-Apps können auf einem beliebigen Gerät mit Windows 10 ausgeführt werden – z. B. Smartphones, Tablets, Desktops, TV-Geräten und mehr. Aufgrund der Vielzahl an Geräten im Windows 10-Ökosystem wird anstelle einer Optimierung deiner Benutzeroberfläche für jedes Gerät empfohlen, ein Design für einige Schlüsselbreiten (sogenannte Breakpoints) zu erstellen: 
- Klein (unter 640 Pixel)
- Mittel (641 Pixel bis 1007 Pixel)
- Groß (1008 Pixel und mehr)

> [!TIP]
> Bei der Entwicklung eines Designs für bestimmte Breakpoints solltest du den für deine App verfügbaren Bildschirmbereich (das App-Fenster) berücksichtigen, nicht die Bildschirmgröße. Wenn die App im Vollbildmodus ausgeführt wird, hat das App-Fenster die gleiche Größe wie der Bildschirm, aber wenn sich die App nicht im Vollbildmodus befindet, ist das Fenster kleiner als der Bildschirm.

## <a name="breakpoints"></a>Breakpoints
Diese Tabelle beschreibt die verschiedenen Größenklassen und Breakpoints.

![Breakpoints für ein reaktionsfähiges Design](images/breakpoints/size-classes.svg)

<table>
<thead>
<tr class="header">
<th align="left">Größenklasse</th>
<th align="left">Breakpoints</th>
<th align="left">Normale Bildschirmgröße (diagonal)</th>
<th align="left">-Geräte zu unterstützen</th>
<th align="left">Fenstergrößen</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">Klein</td>
<td style="vertical-align:top;">640 Pixel oder weniger</td>
<td style="vertical-align:top;">4&quot; bis 6&quot;, 20&quot; bis 65&quot;</td>
<td style="vertical-align:top;">Smartphones, TV-Geräte</td>
<td style="vertical-align:top;">320 x 569, 360 x 640, 480 x 854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Medium (Mittel)</td>
<td style="vertical-align:top;">641 Pixel bis 1007 Pixel</td>
<td style="vertical-align:top;">7&quot; bis 12&quot;</td>
<td style="vertical-align:top;">Phablets, Tablets</td>
<td style="vertical-align:top;">960 × 540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Groß</td>
<td style="vertical-align:top;">1008 Pixel oder größer</td>
<td style="vertical-align:top;">13&quot; und größer</td>
<td style="vertical-align:top;">PCs, Laptops, Surface Hubs</td>
<td style="vertical-align:top;">1024 × 640, 1366 × 768, 1920 × 1080</td>
</tr>
</tbody>
</table>

## <a name="why-are-tvs-considered-small"></a>Warum werden TV-Geräte als „klein“ betrachtet? 

Obwohl die meisten TV-Geräte physisch ziemlich groß sind (40 bis 65 Zoll sind üblich) und eine hohe Auflösung aufweisen (HD oder 4k), unterscheidet sich die Entwicklung für ein 1080p-TV-Gerät, das aus 3 Meter Abstand betrachtet wird, von der Entwicklung für einem 1080p-Monitor auf deinem Schreibtisch. Wenn du den Abstand berücksichtigst, entsprechen die 1080 Pixel des TV-Geräts eher einem 540-Pixel-Monitor, der viel näher steht.

Das UWP-System effektiver Pixel berücksichtigt automatisch den Betrachtungsabstand. Wenn du eine Größe für ein Steuerelement oder einen Breakpointbereich angibst, verwendest du dabei automatisch „effektive“ Pixel. Wenn du beispielsweise reaktionsfähigen Code für 1080 Pixel und mehr erstellst, wird dieser Code für einen 1080p-Monitor verwendet, nicht jedoch für ein 1080p-TV-Gerät. Denn obwohl ein 1080p-TV-Gerät 1080 physische Pixel besitzt, umfasst es nur 540 effektive Pixel. Dadurch entspricht die Entwicklung für ein TV-Gerät der Entwicklung für ein Smartphone.

## <a name="effective-pixels-and-scale-factor"></a>Effektive Pixel und Skalierungsfaktor

UWP-Apps skalieren deine Benutzeroberfläche automatisch, um sicherzustellen, dass deine Anwendung auf allen Windows 10-Geräten lesbar ist. Windows wählt basierend auf dem DPI-Wert (Punkte pro Zoll) und dem Betrachtungsabstand des Geräts automatisch einen Skalierungsfaktor für jedes Anzeigegerät aus. Benutzer können den Standardwert überschreiben, indem sie zur Einstellungsseite unter **Einstellungen** > **Anzeige** > **Skalierung und Layout** wechseln. 


## <a name="general-recommendations"></a>Allgemeine Empfehlungen

### <a name="small"></a>Klein
- Legen Sie den linken und den rechten Fensterrand auf 12px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.
- Docke die [App-Leisten](../controls-and-patterns/app-bars.md) am unteren Fensterrand an, um bequemer darauf zugreifen zu können.
- Verwenden jeweils eine Spalte/Region.
- Verwenden Sie ein Symbol zum Darstellen der Suche (kein Suchfeld anzeigen).
- Verwenden Sie den [Navigationsbereich](../controls-and-patterns/navigationview.md) im Überlagerungsmodus, um Platz auf dem Bildschirm zu sparen.
- Verwenden Sie für das [Master/Details-Modell](../controls-and-patterns/master-details.md) den gestapelten Darstellungsmodus, um Platz auf dem Bildschirm zu sparen.

### <a name="medium"></a>Medium (Mittel)
- Legen Sie den linken und den rechten Fensterrand auf 24px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.
- Positionieren Sie Elemente wie [App-Leisten](../controls-and-patterns/app-bars.md) am oberen Rand des App-Fensters.
- Verwende bis zu zwei Spalten/Regionen.
- Zeigen Sie das Suchfeld an.
- Legen Sie für [Navigationsleiste](../controls-and-patterns/navigationview.md) den Streifenmodus fest, sodass immer ein schmaler Streifen mit Symbolen angezeigt wird.
- Ziehen Sie weitere Anpassungen für [TV-Umgebungen](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv?redirectedfrom=MSDN) in Erwägung.

### <a name="large"></a>Groß
- Legen Sie den linken und den rechten Fensterrand auf 24px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.
- Positionieren Sie Elemente wie [App-Leisten](../controls-and-patterns/app-bars.md) am oberen Rand des App-Fensters.
- Verwende bis zu drei Spalten/Regionen.
- Zeigen Sie das Suchfeld an.
- Platzieren Sie den [Navigationsbereich](../controls-and-patterns/navigationview.md) im angedockten Modus so, dass er immer angezeigt wird.

>[!TIP] 
> Mit [**Continuum for Phone**](https://docs.microsoft.com/windows-hardware/design/device-experiences/continuum-phone?redirectedfrom=MSDN) können Benutzer kompatible Windows 10-Mobilgeräte mit einem Bildschirm, einer Maus und einer Tastatur verbinden, um ihr Gerät wie einen Laptop zu nutzen. Berücksichtige diese neue Funktionalität bei der Entwicklung für bestimmte Breakpoints – ein Mobiltelefon gehört nicht immer derselben Größenklasse an.


