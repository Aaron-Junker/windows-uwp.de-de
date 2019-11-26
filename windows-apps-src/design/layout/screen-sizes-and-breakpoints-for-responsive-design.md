---
title: Bildschirmgrößen und Haltepunkte für reaktionsfähiges Design
description: Anstatt Ihre Benutzeroberfläche für die vielen Geräte im gesamten Windows 10-Ökosystem zu optimieren, empfehlen wir, ein Design für einige Schlüsselbreiten (sogenannte Breakpoints) zu erstellen.
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 37d0ca71adf43891628a02d60d6873e7934d749b
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258191"
---
#  <a name="screen-sizes-and-breakpoints"></a>Bildschirmgrößen und Breakpoints

UWP-Apps können auf jedem Gerät mit Windows 10 ausgeführt werden – z. B. Telefone, Tablets, Desktops, Fernseher und mehr. Bei einer großen Anzahl von Geräte Zielen und Bildschirmgrößen im Windows 10-Ökosystem sollten Sie nicht die Benutzeroberfläche für jedes Gerät optimieren, sondern für einige Schlüsselkategorien (auch als "Breakpoints" bezeichnet) entwerfen: 
- Klein (kleiner als 640 Pixel)
- Mittel (641 Pixel bis 1007 Pixel)
- Groß (1008 Pixel und größer)

> [!TIP]
> Beim Entwerfen für bestimmte Breakpoints sollten Sie den für Ihre App verfügbaren Bildschirmbereich (App-Fenster) berücksichtigen, nicht die Bildschirmgröße. Wenn die App im Vollbildmodus läuft, hat das App-Fenster die gleiche Größe wie der Bildschirm, aber wenn die App nicht im Vollbildmodus ist, ist das Fenster kleiner als der Bildschirm.

## <a name="breakpoints"></a>Breakpoints
Diese Tabelle beschreibt die verschiedenen Größenklassen und Breakpoints.

![Reaktionsfähige Designbreakpoints](images/breakpoints/size-classes.svg)

<table>
<thead>
<tr class="header">
<th align="left">Größenklasse</th>
<th align="left">Breakpoints</th>
<th align="left">Typische Bildschirmgröße (diagonal)</th>
<th align="left">Geräte</th>
<th align="left">Fenstergrößen</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">Klein</td>
<td style="vertical-align:top;">640 Pixel oder weniger</td>
<td style="vertical-align:top;">4&quot; bis 6&quot;; 20&quot; bis 65&quot;</td>
<td style="vertical-align:top;">Smartphones, TV-Geräte</td>
<td style="vertical-align:top;">320 x 569, 360 x 640, 480 x 854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Mittel</td>
<td style="vertical-align:top;">641 Pixel bis 1007 Pixel</td>
<td style="vertical-align:top;">7&quot; bis 12&quot;</td>
<td style="vertical-align:top;">Phablets, Tablets</td>
<td style="vertical-align:top;">960 x 540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Groß</td>
<td style="vertical-align:top;">1008 Pixel oder mehr</td>
<td style="vertical-align:top;">13&quot; und größer</td>
<td style="vertical-align:top;">PCs, Laptops, Surface Hubs</td>
<td style="vertical-align:top;">1024 x 640, 1366 x 768, 1920 x 1080</td>
</tr>
</tbody>
</table>

## <a name="why-are-tvs-considered-small"></a>Warum stehen TV-Geräte unter „klein”? 

Obwohl die meisten TV-Geräte physisch ziemlich groß sind (40 bis 65 Zoll sind üblich) und hohe Auflösungen haben (HD oder 4k), unterscheidet sich das Entwerfen für ein 1080P-TV-Gerät, das Sie aus 3 Meter Abstand betrachten, vom Entwerfen für einem 1080p-Monitor auf Ihrem Schreibtisch. Wenn Sie den Abstand berücksichtigen, entsprechen die 1080-Pixel des TV-Geräts eher einem 540-Pixel-Monitor, der viel näher steht.

Das UWP-System effektiver Pixel berücksichtigt den Betrachtungsabstand automatisch. Wenn Sie eine Größe für ein Steuerelement oder einen Breakpointbereich angeben, verwenden Sie dabei automatisch „effektive” Pixel. Wenn Sie beispielsweise einen reaktionsfähigen Code für 1080 Pixel und mehr erstellen, wird ein 1080-Monitor diesen Code verwenden, ein 1080p-Fernsehgerät jedoch nicht, denn obwohl ein 1080p-Fernsehgerät 1080 physische Pixel besitzt, hat es nur 540 effektive Pixel. Dadurch entspricht das Entwerfen für ein Fernsehgerät dem Entwerfen für ein Smartphone.

## <a name="effective-pixels-and-scale-factor"></a>Effektive Pixel und Skalierungsfaktor

UWP-Apps skalieren Ihre Benutzeroberfläche automatisch, um sicherzustellen, dass Ihre Anwendung auf allen Windows 10-Geräten lesbar ist. Windows wählt automatisch einen Skalierungsfaktor für jede Anzeige aus, basierend auf dem DPI-Wert (Punkte pro Zoll) und dem Betrachtungsabstand des Geräts. Benutzer können den Standardwert überschreiben, indem Sie auf **Einstellungen** > **Anzeige** > **Skalierung und Layout**-Einstellungsseite wechseln. 


## <a name="general-recommendations"></a>Allgemeine Empfehlungen

### <a name="small"></a>Klein
- Legen Sie den linken und den rechten Fensterrand auf 12px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.
- Docken Sie [App-Leisten](../controls-and-patterns/app-bars.md) für bessere Erreichbarkeit am unteren Fensterrand an.
- Verwenden Sie jeweils eine Spalte/Region.
- Verwenden Sie ein Symbol zum Darstellen der Suche (kein Suchfeld anzeigen).
- Verwenden Sie den [Navigationsbereich](../controls-and-patterns/navigationview.md) im Überlagerungsmodus, um Platz auf dem Bildschirm zu sparen.
- Verwenden Sie für das [Master/Details-Modell](../controls-and-patterns/master-details.md) den gestapelten Darstellungsmodus, um Platz auf dem Bildschirm zu sparen.

### <a name="medium"></a>Mittel
- Legen Sie den linken und den rechten Fensterrand auf 24px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.
- Positionieren Sie Elemente wie [App-Leisten](../controls-and-patterns/app-bars.md) am oberen Rand des App-Fensters.
- Verwenden Sie bis zu zwei Spalten/Regionen.
- Zeigen Sie das Suchfeld an.
- Legen Sie für [Navigationsleiste](../controls-and-patterns/navigationview.md) den Streifenmodus fest, sodass immer ein schmaler Streifen mit Symbolen angezeigt wird.
- Ziehen Sie weitere Anpassungen für [TV-Umgebungen](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv?redirectedfrom=MSDN) in Erwägung.

### <a name="large"></a>Groß
- Legen Sie den linken und den rechten Fensterrand auf 24px fest, um eine visuelle Trennung zwischen dem linken und dem rechten Rand des App-Fensters zu erzielen.
- Positionieren Sie Elemente wie [App-Leisten](../controls-and-patterns/app-bars.md) am oberen Rand des App-Fensters.
- Verwenden Sie bis zu drei Spalten/Regionen.
- Zeigen Sie das Suchfeld an.
- Platzieren Sie den [Navigationsbereich](../controls-and-patterns/navigationview.md) im angedockten Modus so, dass er immer angezeigt wird.

>[!TIP] 
> Mit [**Continuum für Smartphones**](https://docs.microsoft.com/windows-hardware/design/device-experiences/continuum-phone?redirectedfrom=MSDN)können Benutzer kompatible Windows 10 Mobile-Geräte mit einem Monitor, einer Maus und Tastatur verbinden, damit ihre Telefone wie Laptops funktionieren. Berücksichtigen Sie diese neue Funktion beim Entwerfen für bestimmte Breakpoints – ein Mobiltelefon bleibt nicht immer in einer Klasse mit geringer Größe.


