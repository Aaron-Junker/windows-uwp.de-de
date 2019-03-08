---
title: Swapchains
description: Eine Swapchain ist eine Sammlung von Puffern, die zum Anzeigen von Frames für den Benutzer verwendet werden.
ms.assetid: A38E8BB7-1E77-4D93-B321-D3572A80D5DD
keywords:
- Swapchains
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 486eb4adc1151bac1bf6a04a8f54b67530b426a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607345"
---
# <a name="swap-chains"></a>Swapchains


Eine Swapchain ist eine Sammlung von Puffern, die zum Anzeigen von Frames für den Benutzer verwendet werden. Bei jeder Darstellung eines neuen Frames für eine Anzeige durch eine Anwendung wird der erste Puffer der Swapchain zum Anzeigepuffer. Dieser Prozess heißt *Swapping* bzw. *Flipping*.

Eine Grafikkarte enthält einen Zeiger auf eine als Frontpuffer bezeichnete Oberfläche, die das auf dem Monitor anzuzeigende Bild darstellt. Wenn der Monitor aktualisiert wird, sendet die Grafikkarte den Inhalt des Frontpuffers zur Anzeige an dem Monitor. Dies führt aber beim Rendern von Grafiken in Echtzeit zu dem Problem des „Abreißens”. Im Wesentlichen besteht das Problem darin, dass die Aktualisierungsrate des Monitors im Vergleich zum Rest des Computers sehr langsam ist. Übliche Aktualisierungsraten liegen zwischen 60 Hz (60 Mal pro Sekunde) und 100 Hz.

Wenn Ihre Anwendung den Frontpuffer aktualisiert, während der Monitor sich mitten in einer Aktualisierung befindet, wird das angezeigte Bild in der Mitte abgerissen. Die obere Hälfte des Bildschirms enthält das alte Bild und die untere Hälfte das neue Bild. Dieses Problem wird als *Abreißen* bezeichnet.

## <a name="span-idavoidingtearingspanspan-idavoidingtearingspanspan-idavoidingtearingspanavoiding-tearing"></a><span id="Avoiding_tearing"></span><span id="avoiding_tearing"></span><span id="AVOIDING_TEARING"></span>Vermeiden, löschen


Direct3D implementiert zwei Optionen, um ein Abreißen zu vermeiden:

-   Eine Option, um nur Aktualisierungen des Monitors bei der vertikalen Synchronisierung zuzulassen. Ein Monitor aktualisiert das Bild in der Regel durch einen Lichtpunkt, der horizontal in Zickzacklinien von links oben nach rechts unten verläuft. Wenn der Lichtpunkt unten angekommen ist, setzt der Monitor den Lichtpunkt wieder zurück nach links oben, sodass der Vorgang erneut beginnen kann.

    Diese Neukalibrierung wird vertikale Synchronisierung genannt. Bei einer vertikalen Synchronisierung zeichnet der Monitor nichts, daher werden Aktualisierungen des Frontpuffers erst angezeigt, wenn der Monitor erneut zu zeichnen beginnt. Die vertikale Synchronisierung ist relativ langsam. Allerdings nicht langsam genug, um eine komplexe Szenen beim Warten zu rendern. Um ein Abreißen zu vermeiden und komplexe Szenen zu rendern, wird ein Prozess benötigt, der als Hintergrundpufferung bezeichnet wird.

-   Eine Option, um die als Hintergrundpufferung bezeichnete Technik zu verwenden. Hintergrundpufferung ist der Prozess des Zeichnens einer Szene auf eine Offscreenoberfläche, die Hintergrundpuffer genannt wird. Jede Oberfläche mit Ausnahme des Frontpuffers wird als Offscreenoberfläche bezeichnet, weil diese nie direkt vom Monitor angezeigt werden.

    Mit einem Hintergrundpuffer hat eine Anwendung die Möglichkeit, eine Szene zu rendern, wenn das System sich im Leerlauf befindet (d. h. keine Windows-Meldungen warten), ohne dass die Aktualisierungsrate des Monitors berücksichtigt werden muss. Hintergrundpufferung führt eine zusätzliche Schwierigkeit ein, nämlich die Frage, wie und wann der Hintergrundpuffer in den Frontpuffer verschoben werden soll.

## <a name="span-idsurfaceflippingspanspan-idsurfaceflippingspanspan-idsurfaceflippingspansurface-flipping"></a><span id="Surface_flipping"></span><span id="surface_flipping"></span><span id="SURFACE_FLIPPING"></span>Oberfläche kippen


Der Prozess des Verschiebens des Hintergrundpuffers in den Frontpuffer wird „Kippen der Oberfläche” genannt. Da die Grafikkarte einfach einen Zeiger auf eine Oberfläche verwendet, um den Frontpuffer darzustellen, ist eine Änderung des Zeigers alles, was erforderlich ist, um den Hintergrundpuffer auf den Frontpuffer festzulegen. Wenn eine Anwendung Direct3D auffordert, den Hintergrundpuffer in den Frontpuffer zu verschieben, vertauscht Direct3D einfach die beiden Oberflächenzeiger. Das Ergebnis ist, dass der Hintergrundpuffer jetzt der neue Frontpuffer ist und der alte Frontpuffer der neue Hintergrundpuffer.

Das Kippen der Oberfläche wird immer aufgerufen, wenn eine Anwendung das Direct3D-Gerät auffordert, den Hintergrundpuffer darzustellen. Direct3D kann aber auch so eingerichtet werden, dass die Anforderungen in eine Warteschlange gestellt werden, bis eine vertikale Synchronisierung erfolgt. Diese Option wird als das Darstellungsintervall des Direct3D-Geräts bezeichnet. Die Daten in dem neuen Hintergrundpuffer sind möglicherweise nicht wiederverwendbar. Das hängt davon ab, wie eine Anwendung die Behandlung des Kippens der Oberfläche durch Direct3D festlegt.

Das Kippen der Oberfläche ist in Multimedia-, Animations- und Spielesoftware wichtig. Es entspricht der Art und Weise, wie Sie Animationen mit einem Notizblock (Daumenkino) ausführen können. Auf jeder Seite ändert der Zeichner die Figuren etwas, sodass beim schnellen Durchblättern der Seiten die Zeichnung animiert erscheint.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Geräte](devices.md)

 

 




