---
Description: Erfahren Sie, wie Fluent Motion verwendet direktionalität und die Schwerkraft.
title: Direktionalität und Schwerkraft – Animation in UWP-Apps
label: Directionality and gravity
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ef005ffd018bf52d70067cdfedbb9c98b079bd89
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444208"
---
# <a name="directionality-and-gravity"></a>Direktionalität und Schwerkraft

Gerichtete Bewegungen festigen das mentale Modell der Reise, die ein Benutzer auf einer Oberfläche macht. Es ist wichtig, dass jede Bewegungsrichtung sowohl die Kontinuität des Raumes als auch die Integrität der Objekte im Raum unterstützt.

Eine gerichtete Bewegung unterliegt Kräften wie beispielsweise der Schwerkraft. Die Anwendung von Kräften auf die Bewegung verstärkt ihre die natürliche Anmutung.

## <a name="examples"></a>Beispiele

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/category/Motion">öffnen Sie die app, und Sie während der Übertragung in Aktion sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>Bewegungsrichtung

:::row:::
    :::column:::
        Direction of movement corresponds to physical motion. Just like in nature, objects can move in any world axis - X,Y,Z. This is how we think of the movement of objects on the screen.

        When you move objects, avoid unnatural collisions. Keep in mind where objects come from and go to, and alway support higher level constructs that may be used in the scene, such as scroll direction or layout hierarchy.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Navigationsrichtung

Die Navigationsrichtung zwischen den Szenen in Ihrer App ist konzeptionell. Benutzer navigieren vorwärts oder rückwärts. Szenen erscheinen im Blickfeld oder werden ausgeblendet. Die Kombination dieser Konzepte mit physikalischen Bewegungen dient zur Führung des Benutzers.

Wenn die Navigation ein Objekt von der vorherigen Szene zur neuen Szene bewegt, führt das Objekt auf dem Bildschirm eine einfache Bewegung von A nach B aus. Um sicherzustellen, dass die Bewegung physikalisch real erscheint, werden sowohl der standardmäßige Geschwindigkeitsverlauf als auch die Schwerkraft hinzugefügt.

Für die Rückwärtsnavigation wird die Verschiebung umgekehrt (von B nach A). Wenn der Benutzer zurück navigiert, erwartet er, dass er so schnell wie möglich in den vorherigen Zustand zurückkehrt. Das Timing ist schneller und direkter. Es wird ein sich verlangsamender Geschwindigkeitsverlauf verwendet.

Im Folgenden werden diese Prinzipien angewendet, da das ausgewählte Element während der Vorwärts- und Rückwärtsnavigation auf dem Bildschirm sichtbar bleibt.

![UI-Beispiel für kontinuierliche Bewegung](images/continuous3.gif)

Wenn Elemente auf dem Bildschirm durch die Navigation ersetzt werden, ist es wichtig zu zeigen, wohin die Szene führt und woher die neue Szene kommt.

Dies hat mehrere Vorteile:

- Es festigt das mentale Raummodell des Benutzers.
- Die Dauer der auslaufenden Szene kann genutzt werden, um den Inhalt für die eingehende Szene vorzubereiten.
- De wahrgenommene Leistung der App wird verbessert.

Es sind 4 diskrete Navigationsrichtungen zu berücksichtigen.

:::row:::
    :::column:::
        **Forward-In**

        Celebrate content entering the scene in a manner that does not collide with outgoing content. Content decelerates into the scene.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Forward-Out**

        Content exits quickly. Objects accelerate off screen.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-In**

        Same as Forward-In, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-Out**

        Same as Forward-Out, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Schwerkraft

Schwerkraft lässt die Darstellung natürlicher wirken. Objekte, die sich auf der Z-Achse bewegen und nicht durch ein On-Screen-Angebot in der Szene verankert sind, können von der Schwerkraft beeinflusst werden. Wenn ein Objekt die Szene verlässt, zieht die Schwerkraft das Objekt herunter, bevor es seine Endgeschwindigkeit erreicht hat, und erzeugt so eine natürliche Krümmung der Bahn, auf dem sich das Objekt bewegt.

Schwerkraft wird normalerweise berücksichtigt, wenn ein Objekt von einer Szene zu einer anderen springen muss. Daher verwendet eine verbundene Animationen das Konzept der Schwerkraft.

Im Folgenden wird ein Element aus der oberen Reihe des Gitters durch die Schwerkraft beeinflusst, so dass es leicht fällt, wenn es seinen Platz verlässt und sich nach vorne bewegt.

![Richtung rückwärts hinein](images/continuity-photos.gif)

## <a name="related-articles"></a>Verwandte Artikel

- [Motion-Übersicht](index.md)
- [Zeitsteuerungssystem und vereinfachen der](timing-and-easing.md)
