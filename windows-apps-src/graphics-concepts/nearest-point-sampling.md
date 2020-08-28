---
title: Sampling am nächstgelegenen Punkt
description: Erfahren Sie, wie Sie die nächste Punkt Stichprobe in Direct3D als Alternative zur Textur Filterung für die Verarbeitung von Texturen in einer Anwendung verwenden.
ms.assetid: D7F88320-2C61-47E9-9B92-EC31D48DB079
keywords:
- Sampling am nächstgelegenen Punkt
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 090c148e05e664ffe0b027fe9af7eb69c11b22a9
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043492"
---
# <a name="span-iddirect3dconceptsnearest-point_samplingspannearest-point-sampling"></a><span id="direct3dconcepts.nearest-point_sampling"></span>Sampling am nächstgelegenen Punkt


Anwendungen müssen die Textur Filterung nicht verwenden. Direct3D kann so festgelegt werden, dass die textabzahl berechnet wird, die häufig nicht Ganzzahlen ergibt, und die Farbe des texms mit der nächstgelegenen ganzzahligen Adresse kopiert. Dieser Vorgang wird als " *nächster Punkt Sampling*" bezeichnet. Die Stichprobenentnahme am nächsten Punkt kann so aussehen, als ob die Größe der Textur der Größe des primitiven Bilds auf dem Bildschirm ähnelt. Andernfalls muss die Textur vergrößert oder minimiert werden. Das Ergebnis von nicht übereinstimmenden Textur Größen mit der primitiven Bildgröße kann ein segmentiert, ein Alias oder ein verschwommites Bild sein.

Verwenden Sie die Stichprobenentnahme für das nächste Punkt sorgfältig, da es mitunter grafische Artefakte verursachen kann, wenn die Textur an der Grenze zwischen zwei texeln gemessen wird. Diese Grenze ist die Position entlang der Textur (u oder v), an der der abgetastete Texturtyp von einem texturdown zum nächsten übergeht. Wenn die Punkt Stichproben Erstellung verwendet wird, wählt das System ein Beispiel textexaus, und das Ergebnis kann sich bei Überschreitung der Grenze plötzlich von einem texseto den nächsten texsetyp ändern. Dieser Effekt kann in der angezeigten Textur als unerwünschte grafische Artefakte angezeigt werden. Wenn eine lineare Filterung verwendet wird, wird das resultierende Texel aus benachbarten texeln berechnet und nahtlos zwischen Ihnen gemischt, wenn der Textur Index durch die Grenze bewegt wird.

Dieser Effekt kann bei der Zuordnung einer sehr kleinen Textur zu einem sehr großen Polygon angezeigt werden: ein Vorgang, der häufig als Vergrößerungs Vorgang bezeichnet wird. Wenn Sie z. b. eine Textur verwenden, die wie ein checkboard aussieht, führt die Stichprobenentnahme am nächsten Punkt zu einem größeren checkboard, das unterschiedliche Ränder anzeigt. Im Gegensatz dazu führt die lineare Textur Filterung zu einem Bild, bei dem die Farben des Prüf Boards über das gesamte Polygon hinweg reibungslos abweichen.

In den meisten Fällen erhalten Anwendungen die besten Ergebnisse, indem Sie nach Möglichkeit das nächste Punkt Beispiel vermeiden. Der Großteil der Hardware ist heute für die lineare Filterung optimiert, sodass Ihre Anwendung nicht beeinträchtigt wird. Wenn die gewünschte Auswirkung wirklich die Verwendung der nächsten Punkt Stichprobe erfordert, z. b. bei der Verwendung von Texturen zum Anzeigen lesbarer Textzeichen, sollte Ihre Anwendung sehr vorsichtig sein, um die Stichprobenentnahme an den Textzeile zu vermeiden, was zu unerwünschten Effekten führen kann. In der folgenden Abbildung wird gezeigt, wie diese Artefakte aussehen können.

![Abbildung eines sechsstufigen Felds mit nicht kontinuierlichen horizontalen Linien in den beiden oberen rechten Quadraten](images/ptrtfct.png)

Die beiden Quadrate in der oberen rechten Ecke der Gruppe werden unterschiedlich angezeigt, und es werden diagonal Offsets durch Sie ausgeführt. Um Grafik Artefakte wie diese zu vermeiden, müssen Sie mit den Direct3D-Textur-Samplings-Regeln für die Next-Point-Filterung vertraut sein. Direct3D ordnet eine Gleit Komma-Textur Koordinate im \[ Bereich von 0,0, 1,0 \] (0,0 bis 1,0, inklusiv) einem ganzzahligen Texel-Leerraum zu, \[ der von-0,5, n-0,5 \] , wobei n die Anzahl der texeln in einer gegebenen Dimension in der Textur ist. Der resultierende Textur Index wird auf die nächste ganze Zahl gerundet. Diese Zuordnung kann zu Stichproben Ungenauigkeiten bei Texel-Grenzen führen.

Stellen Sie sich bei einem einfachen Beispiel eine Anwendung vor, die Polygone mit dem Wrap-Textur Adressierungs Modus rendert. Mit der von Direct3D verwendeten Zuordnung wird der u-Textur Index wie im folgenden Diagramm dargestellt für eine Textur mit einer Breite von 4 texeln zugeordnet.

![Diagramm der Texturkoordinaten 0,0 und 1,0 an der Grenze zwischen texeln](images/ptsmpprb.png)

Die Texturkoordinaten 0,0 und 1,0 für diese Abbildung liegen genau an der Grenze zwischen texeln. Mithilfe der Methode, mit der Direct3D-Werte zuordnet, reichen die Texturkoordinaten von \[ -0,5, 4-0,5 \] , wobei 4 die Breite der Textur ist. In diesem Fall handelt es sich bei dem abgetasteten textext um den 0 textext für einen Textur Index von 1,0. Wenn die Textur Koordinate jedoch nur geringfügig kleiner als 1,0 ist, wäre der abgetastete textext der n Texel anstelle des 0-Texels.

Dies impliziert, dass eine kleine Textur mit Texturkoordinaten von exakt 0,0 und 1,0 mit der nächstgelegenen Filterung auf einem auf einem Bildschirmrand ausgerichteten Dreieck in Pixel vergrößert wird, für die die Textur Map an der Grenze zwischen Texels gemessen wird. Alle Ungenauigkeiten bei der Berechnung von Texturkoordinaten, die jedoch klein sind, führen zu Artefakten in den Bereichen im gerenderten Bild, die den texrändern der Textur Karte entsprechen.

Die Durchführung dieser Zuordnung von Gleit Komma Texturkoordinaten zu ganzzahligen texeln mit perfekter Genauigkeit ist schwierig, Rechenzeit aufwändig und in der Regel nicht erforderlich. Die meisten Hardware Implementierungen verwenden einen iterativen Ansatz zum Berechnen von Texturkoordinaten an jeder Pixelposition innerhalb eines Dreiecks. Iterative Ansätze Blenden tendenziell diese Ungenauigkeiten aus, da die Fehler gleichmäßig während der Iteration kumuliert werden.

Der Direct3D Reference Raster verwendet einen direkt Auswertungs Ansatz zum Berechnen von Textur Indizes an jedem Pixel Speicherort. Die direkte Auswertung unterscheidet sich vom iterativen Ansatz dahin, dass jede Ungenauigkeit im Vorgang eine eher zufällige Fehlerverteilung aufweist. Das Ergebnis ist, dass die an den Grenzen auftretenden Stichprobenfehler deutlicher erkennbar sein können, da der Verweis Raster diesen Vorgang nicht mit der perfekten Genauigkeit ausführt.

Der beste Ansatz besteht darin, die nächstgelegene Punkt Filterung nur bei Bedarf zu verwenden. Wenn Sie es verwenden müssen, empfiehlt es sich, die Texturkoordinaten leicht von den Begrenzungs Positionen zu versetzen, um Artefakte zu vermeiden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturfilterung](texture-filtering.md)

 

 




