---
author: mijacobs
Description: "Gute Symbole harmonieren mit der Typografie und der übrigen Gestaltungssprache. Sie verwenden keine Metaphern und geben einfach und schnell nur die erforderlichen Informationen weiter."
title: Symbole
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
ms.openlocfilehash: 07df77f295e6454376b2fd8bcc7f12c9b2956bbc
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="icons-for-uwp-apps"></a>Symbole für UWP-Apps

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Gute Symbole harmonieren mit der Typografie und der übrigen Gestaltungssprache. Sie verwenden keine Metaphern und geben einfach und schnell nur die erforderlichen Informationen weiter. 

## <a name="linear-scaling-size-ramps"></a>Größenabstufungen für lineare Skalierungen 

<table>
    <tr> 
        <td>16px x 16px</td>
        <td>24px x 24px</td>
        <td>32px x 32px</td>
        <td>48px x 48px</td>
    </tr>
    <tr> 
        <td>![Symbole mit 16x16 effektiven Pixeln](images/icons-16x16.png)</td>
        <td>![Symbole mit 24x24 effektiven Pixeln](images/icons-24x24.png)</td>
        <td>![Symbole mit 32x32 effektiven Pixeln](images/icons-32x32.png)</td>
        <td>![Symbole mit 48x48 effektiven Pixeln](images/icons-48x48.png)</td>
    </tr>
</table>

## <a name="common-shapes"></a>Häufige Formen

Grundsätzlich sollten Symbole den vorhandenen Platz mit geringem Abstand möglichst vollständig ausnutzen. Diese Formen sind Ausgangspunkte für die Bestimmung der Größe einfacher Formen. 

![Raster der Größe 32x32px](images/icons-common-shapes.png)

Verwenden Sie die Form, die der Ausrichtung des Symbols entspricht, und berücksichtigen Sie dabei diese grundlegenden Parameter. Symbole müssen die Form nicht unbedingt ausfüllen oder vollständig hinein passen. Sie können nach Bedarf angepasst werden, um ausgewogen zu wirken. 

<table class="uwpd-noborder">
    <tr>
        <td>Kreis<td>
        <td>Quadrat</td>
        <td>Dreieck</td>
    </tr>
    <tr>
        <td>![Ein Kreis](images/icons-common-shapes-examples-1.png)<td>
        <td>![Ein Quadrat](images/icons-common-shapes-examples-2.png)</td>
        <td>![Ein Dreieck ](images/icons-common-shapes-examples-3.png)</td>
    </tr>
        <tr>
        <td>Horizontales Rechteck<td>
        <td colspan="2">Vertikales Rechteck</td>        
        </tr>
    <tr>
        <td>![Ein horizontales Rechteck](images/icons-common-shapes-examples-4.png)<td>
        <td colspan="2">![Ein vertikales Rechteck](images/icons-common-shapes-examples-5.png)</td>
         
    </tr>

</table>

## <a name="angles"></a>Winkel

Neben der Verwendung des gleichen Rasters und der gleichen Linienbreite werden Symbole mit einheitlichen Elementen erstellt. 

Wenn Sie beim Erstellen von Formen nur diese Winkel verwenden, sehen all Ihre Symbole einheitlich aus. Außerdem werden die Symbole dann richtig gerendert. 

Diese Linien können beim Erstellen von Symbolen kombiniert, verknüpft, gedreht und gespiegelt werden. 

<table>
    <tr>
        <td>**1:1**<br/>45°</td>
        <td>**1:2**<br />26,57° (vertikal)<br/>63,43° (horizontal)</td>
        <td>**1:3**<br/>18,43° (vertikal)<br/>71,57° (horizontal)</td>
        <td>**1:4**<br/>14,04° (vertikal)<br/>75,96° (horizontal)</td>
    </tr>
    <tr>
        
        <td>![1:1](images/icons-grid-1-1.png)</td>
        <td>![1:2](images/icons-grid-1-2.png)</td>
        <td>![1:3](images/icons-grid-1-3.png)</td>
        <td>![1:4](images/icons-grid-1-4.png)</td>
    </tr>  
</table>

<p>Dies sind einige Beispiele:</p>

<table>
    <tr>
        <td>![Beispiel für einen 1:1-Winkel](images/icons-angles-examples-1.png)</td>
        <td>![Beispiel für einen 1:2-Winkel](images/icons-angles-examples-2.png)</td>
        <td>![Beispiel für einen 1:3-Winkel](images/icons-angles-examples-3.png)</td>
        <td>![Beispiel für einen 1:4-Winkel](images/icons-angles-examples-4.png)</td>
    </tr>
</table>

## <a name="curves"></a>Kurven

Kurvenlinien entstehen aus Abschnitten eines ganzen Kreises und sollten nur dann verzerrt werden, wenn dies für die Verankerung am Pixelraster erforderlich ist. 

<table>
    <tr>
        <td>1/4-Kreis</td>
        <td>1/8-Kreis</td>
    </tr>
    <tr>
        <td>![1/4-Kreis](images/icons-curves-14circle.png)</td>
        <td>![1/8-Kreis](images/icons-curves-18circle.png)</td>
    </tr>
    <tr>
        <td>![Beispiel für einen 1/4-Kreis](images/icons-curves-examples-1.png)</td>
        <td>![Beispiel für einen 1/8-Kreis](images/icons-curves-examples-2.png)</td>
    </tr>    
</table>

## <a name="geometric-construction"></a>Geometrische Konstruktion

Wir empfehlen, beim Erstellen von Symbolen ausschließlich rein geometrische Formen zu verwenden.

![Gitarrensymbol mit geometrischer Überlagerung ](images/icons-geometric-construction.png)

## <a name="filled-shapes"></a>Gefüllte Formen 

Bei Bedarf können Symbole gefüllte Formen enthalten, aber sie sollten bei einer Rastergröße von 32×32px maximal 4px groß sein. Gefüllte Kreise dürfen maximal 6×6px groß sein. 

![Füllung mit 5pxx8px ](images/icons-filled-shapes.png)

## <a name="badges"></a>Signale

Ein „Signal“ ist ein allgemeiner Begriff zur Beschreibung eines Elements, das einem Symbol hinzugefügt wird, das nicht in das Ausgangselement des Symbols integriert werden soll. Diese enthalten in der Regel andere Informationen über das Symbol wie Status oder Aktion. Andere gängige Begriffe sind beispielsweise Überlagerung, Anmerkung oder Modifizierer. 

![Statussignal ](images/icons-badge-status.png)

![Aktionssignal ](images/icons-badge-action.png)

Statussignale nutzen ein ausgefülltes, farbiges Objekt, das sich auf dem Symbol befindet, während Aktionssignale im gleichen monochromen Stil und mit der gleichen Linienstärke in das Symbol integriert sind.

<table>
<tr>
    <td>Häufig verwendete Statussignale</td>
    <td>Häufig verwendete Aktionssignale</td>
</tr>
<tr>
    <td>![Statussignal ](images/icons-badge-common-states-1.png)</td>
    <td>![Aktionssignal ](images/icons-badge-common-states-2.png)</td>
</tr>
</table>
<p></p>

### <a name="badge-color"></a>Signalfarbe 

Farbsignale sollten nur zur Angabe des Status eines Symbols verwendet werden. Die Farben für Statussignale übermitteln dem Benutzer bestimmte emotionale Botschaften. 

<table>
<tr><td>Grün: #128B44</td><td>Blau: #2C71B9</td><td>Gelb: #FDC214</td></tr>
<tr><td>Positiv: fertig, abgeschlossen </td><td>Neutral: Hilfe, Benachrichtigung </td><td>Warnend: Hinweis, Warnung </td></tr>
<tr><td>![Grüner Status](images/icons-color-inbadging-1.png)</td><td>![Blauer Status](images/icons-color-inbadging-2.png)</td>
<td>![Gelber Status](images/icons-color-inbadging-3.png)</td></tr>
</table>
<p></p>

### <a name="badge-position"></a>Signalposition

Die Standardposition für jeden Status bzw. jede Aktion ist unten rechts. Verwenden Sie die anderen Positionen nur dann, wenn das Design die Standardposition nicht zulässt. 

### <a name="badge-sizing"></a>Signalgröße

Signale sollten bei einem Raster der Größe 32×32px 10 bis 18px umfassen. 

## <a name="related-articles"></a>Verwandte Artikel

* [Richtlinien für die Ressourcen für Kacheln und Symbole](../controls-and-patterns/tiles-and-notifications-app-assets.md)
