---
title: Direktionalität und Schwerkraft-Animation in Windows-apps
description: Hier finden Sie Informationen zur Verwendung von Bewegungsrichtung, Navigationsrichtung und Schwerkraft in animierten Szenen durch Anzeigen von Beispielen.
label: Directionality and gravity
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 649fb9e2833a425ccad78f5f0bcb69ccb4091bca
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217803"
---
# <a name="directionality-and-gravity"></a>Direktionalität und Schwerkraft

Direktionale Signale helfen dabei, das geistige Modell der Journey zu verfestigen, die ein Benutzer über Erfahrungen hinweg durchführt. Es ist wichtig, dass die Richtung jeder Bewegung sowohl die Kontinuität des Speicherplatzes als auch die Integrität der Objekte im Raum unterstützt.

Die direktionale Bewegung unterliegt den Erzwingung der Kraft haftigkeit Das Anwenden von Erzwingung auf Bewegung stärkt das natürliche Gefühl der Bewegung.

## <a name="examples"></a>Beispiele

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/category/Motion">die App zu öffnen und Bewegung in Aktion zu sehen</a>.</p>
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
Die Richtung der Bewegung entspricht der physischen Bewegung. Ebenso wie in der Natur können-Objekte in jeder Welt Achse-X, Y, Z verschoben werden. So betrachten wir die Bewegung von Objekten auf dem Bildschirm.
Vermeiden Sie unnatürliche Kollisionen, wenn Sie Objekte verschieben. Beachten Sie, dass Objekte von abgeleitet werden und dass Sie immer Konstrukte höherer Ebene unterstützen, die in der Szene verwendet werden können, z. b. Scrollrichtung oder layouthierarchie.
    :::column-end:::
    :::column:::
        ![Richtung rückwärts in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Navigationsrichtung

Die Navigationsrichtung zwischen den Kulissen in der APP ist konzeptionell. Benutzer navigieren vorwärts und rückwärts. Szenen wechseln in den und aus der Ansicht. Diese Konzepte werden mit physischer Bewegung kombiniert, um den Benutzer zu unterstützen.

Wenn die Navigation bewirkt, dass ein Objekt von der vorherigen Szene zur neuen Szene bewegt wird, führt das-Objekt einen einfachen a-to-B-Verschiebe Vorgang auf dem Bildschirm aus. Um sicherzustellen, dass die Bewegung physischer ist, wird die standardmäßige Beschleunigung sowie das Gefühl der Schwerkraft hinzugefügt.

Bei der rückwärts Navigation wird der Verschiebe Vorgang umgekehrt (B-zu-A). Wenn der Benutzer zurück navigiert, wird erwartet, dass er so bald wie möglich in den vorherigen Zustand zurückversetzt wird. Die zeitliche Steuerung ist schneller, direkter und verwendet die Verlangsamung der Verlangsamung.

Hier werden diese Prinzipien angewendet, wenn das ausgewählte Element während der vorwärts-und rückwärts Navigation auf dem Bildschirm bleibt.

![UI-Beispiel für kontinuierliche Bewegung](images/continuous3.gif)

Wenn die Navigation bewirkt, dass die Elemente auf dem Bildschirm ersetzt werden, ist es wichtig, den Speicherort der beendenden Szene und den Ort der neuen Szene anzuzeigen.

Dies hat mehrere Vorteile:

- Dadurch wird das geistige Modell des Benutzers für den Bereich gefestigt.
- Die Dauer der beendenden Szene bietet mehr Zeit für die Vorbereitung von Inhalten, die für die eingehende Szene animiert werden sollen.
- Dies verbessert die wahrgenommene Leistung der app.

Es gibt vier diskrete Navigations Richtungen, die berücksichtigt werden müssen.

:::row:::
    :::column:::
**Vorwärts** Freuen Sie sich, Inhalte in der Szene so zu feiern, dass Sie nicht mit ausgehenden Inhalten in Konflikt stehen. Der Inhalt wird in die Szene verlangsamt.
    :::column-end:::
    :::column:::
        ![Richtung vorwärts in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Vorwärts** Der Inhalt wird schnell beendet. Objekte beschleunigen den Bildschirm.
    :::column-end:::
    :::column:::
        ![Vorwärtsrichtung](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Rückwärts** Identisch mit vorwärts, aber umgekehrt.
    :::column-end:::
    :::column:::
        ![Richtung rückwärts in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Rückwärts** skalieren Identisch mit vorwärts, aber umgekehrt.
    :::column-end:::
    :::column:::
        ![Richtung rückwärts ausgehend](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Ernst

Die Schwerkraft macht Ihre Erfahrungen natürlicher. Objekte, die sich auf der Z-Achse bewegen und nicht durch eine Bildschirm Umgebung an der Szene verankert sind, können durch die Schwerkraft beeinträchtigt werden. Wenn ein Objekt frei von der Szene unterbrochen wird und bevor es die escapegeschwindigkeit erreicht, wird die Schwerkraft auf das Objekt herabgesetzt. dadurch entsteht eine natürlichere Kurve der Objekt Kurve, wenn es verschoben wird.

Die Schwerkraft ist in der Regel eine Manifeste, wenn ein Objekt von einer Szene in eine andere Aus diesem Grund verwendet die verbundene Animation das Konzept der Schwerkraft.

Hier ist ein Element in der obersten Zeile des Rasters von der Schwerkraft betroffen, was bewirkt, dass es leicht absinkt, wenn es seine Position verlässt und in den Vordergrund bewegt wird.

![Richtung rückwärts in](images/continuity-photos.gif)

## <a name="related-articles"></a>Verwandte Artikel

- [Übersicht über die Bewegung](index.md)
- [Timing und Beschleunigung](timing-and-easing.md)
