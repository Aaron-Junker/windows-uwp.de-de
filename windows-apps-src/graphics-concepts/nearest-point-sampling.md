---
title: Sampling am nächstgelegenen Punkt
description: Apps müssen nicht die Texturfilterung verwenden.
ms.assetid: D7F88320-2C61-47E9-9B92-EC31D48DB079
keywords:
- Sampling am nächstgelegenen Punkt
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 18703a75747e09436e7938e1229dda09cd457fa5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589845"
---
# <a name="span-iddirect3dconceptsnearest-pointsamplingspannearest-point-sampling"></a><span id="direct3dconcepts.nearest-point_sampling"></span>Nächste Point-sampling


Apps müssen nicht die Texturfilterung verwenden. Direct3D kann so eingerichtet werden, dass es die Texel-Adresse berechnet, die häufig nicht auf ganze Zahlen geschätzt wird, und die Farbe des Texels mit der nächstgelegenen ganzzahligen Adresse kopiert. Dieser Prozess wird bezeichnet als *Sampling am nächstgelegenen Punkt*. Sampling am nächstgelegenen Punkt kann eine schnelle und effiziente Möglichkeit zum Verarbeiten von Texturen darstellen, wenn die Größe der Textur der Größe der Abbildung der Primitive auf dem Bildschirm ähnelt. Ist dies nicht der Fall, muss die Textur vergrößert oder verkleinert werden. Das Ergebnis einer mangelnden Übereinstimmung zwischen Texturgröße und der Größe der Abbildung der Primitive kann ein grobes, nicht kantengeglättetes oder verschwommenes Bild sein.

Verwenden Sie Sampling am nächstgelegenen Punkt vorsichtig, da es manchmal Grafikartefakte verursachen kann, wenn die Textur an der Grenze zwischen zwei Texeln gesampelt wird. Diese Grenze ist die Position entlang der Textur (u oder v), an der das gesampelte Texel von einem Texel-zum nächsten übergeht. Wenn Punktsampling verwendet wird, wählt das System ein Probe-Texel oder das andere aus, und das Ergebnis kann sich von einem Texel zum nächsten Texel bei Überschreiten der Grenze abrupt ändern. Dieser Effekt kann als unerwünschte Grafikartefakte in der angezeigten Textur erscheinen. Wenn lineare Filterung verwendet wird, wird das resultierende Texel aus beiden benachbarten Texeln berechnet und fügt sich reibungslos zwischen diese ein, wenn sich der Texturindex über die Grenze bewegt.

Dieser Effekt tritt auf, wenn Sie eine sehr kleine Textur auf ein sehr großes Polygon abbilden: ein Vorgang, der häufig als Vergrößerung bezeichnet wird. Bei der Verwendung einer Textur, die wie ein Schachbrett aussieht, ergibt das Sampling am nächstgelegenen Punkt beispielsweise ein größeres Schachbrett, das ausgeprägte Ränder zeigt. Im Gegensatz dazu führt die lineare Filterung der Textur zu einem Bild, in dem die Farben des Schachbretts reibungslos über das Polygon variieren.

In den meisten Fällen erreichen Anwendungen die besten Ergebnisse durch Vermeiden von Sampling am nächstgelegenen Punkt, wann immer dies möglich ist. Die Mehrzahl der Hardware ist heute für die lineare Filterung optimiert, so dass Ihre Anwendung nicht unter einer Leistungsminderung leiden sollte. Wenn der gewünschte Effekt die Verwendung von Sampling am nächstgelegenen Punkt unbedingt erfordert - z. B. beim Verwenden von Texturen zum Anzeigen lesbarer Textzeichen - sollte Ihre Anwendung extrem vorsichtig sein, um ein Sampling an den Texel-Grenzen zu vermeiden, was zu unerwünschten Effekten führen könnte. Die folgende Abbildung zeigt, wie diese Artefakte aussehen könnten.

![Abbildung eines in sechs Abschnitte unterteilten Feldes mit nicht durchgehenden horizontalen Linien in den beiden Quadraten oben rechts](images/ptrtfct.png)

Die zwei Quadrate oben rechts in der Gruppe scheinen anders als ihre Nachbarn, wobei diagonale Versätze durch sie hindurchlaufen. Um solche Grafikartefakte zu vermeiden, müssen Sie mit Direct3D-Textur-Sampling-Regeln für die Filterung am nächstgelegenen Punkt vertraut sein. Direct3D ordnet eine Gleitkomma Texturkoordinate zwischen \[0,0; 1,0\] (0,0 bis 1,0, inklusive) in einen ganzzahligen Texel Space-Wert im Bereich von \[ - 0,5, n - 0,5\], wobei n die Anzahl von texeln, die in einer angegebenen die Dimension für die Textur. Der resultierende Texturindex wird auf die nächste ganze Zahl gerundet. Diese Zuordnung kann Sampling-Ungenauigkeiten an Texel-Grenzen einführen.

Ein einfaches Beispiel: Stellen Sie sich eine Anwendung vor, die Polygone mit dem Wrap-Textur-Adressierungsmodus berechnet und ausgibt. Mithilfe der Zuordnung von Direct3D ordnet der u-Texturindex, wie im folgenden Diagramm für eine Textur mit einer Breite von 4 Texeln veranschaulicht, zu.

![Diagramm der Texturkoordinaten 0,0 und 1,0 an der Grenze zwischen Texeln](images/ptsmpprb.png)

Die Texturkoordinaten 0,0 und 1,0 für diese Abbildung sind genau an der Grenze zwischen Texeln. Mithilfe der Methode, die mit dem ordnet Direct3D Werte, die Textur koordiniert reichen von \[ : 0,5, 4: 0,5\], wobei 4 die Breite der Textur ist. Für diesen Fall ist das aufgenommene Texel das Texel 0 für einen Texturindex von 1,0. Wenn die Texturkoordinate jedoch lediglich leicht unter 1,0 lag, würde das aufgenommene Texel das Texel n sein, und nicht das Texel 0.

Das bedeutet, dass eine Vergrößerung einer kleinen Textur unter Verwendung von Texturkoordinaten von genau 0,0 und 1,0 mit Filterung am nächstgelegenen Punkt auf einem mit dem Bildschirmbereich ausgerichteten Dreieck zu Pixeln führt, für die die Texturzuordnung an der Grenze zwischen Texeln gesampelt wird. Eventuelle Ungenauigkeiten bei der Berechnung von Texturkoordinaten, wie geringfügig sie auch sein mögen, führen zu Artefakten entlang der Bereiche des ausgegebenen Bildes, die den Texel-Kanten der Texturzuordnung entsprechen.

Die Zuordnung von Gleitkomma-Texturkoordinaten zu ganzzahligen Texeln mit absoluter Genauigkeit ist schwierig, erfordert eine lange Rechenzeit und ist in der Regel nicht erforderlich. Die meisten Hardwareimplementierungen verwenden einen iterativen Ansatz zum Berechnen der Texturkoordinaten an jedem Pixelstandort in einem Dreieck. Iterative Ansätze neigen dazu, diese Ungenauigkeiten auszublenden, da die Fehler während der Iteration gleichmäßig auflaufen.

Der Direct3D-Referenz-Rasterizer verwendet einen direkten Bewertungsansatz zum Berechnen der Texturindizes an jedem Pixelstandort. Eine direkte Bewertung unterscheidet sich vom iterativen Ansatz dahingehend, dass alle Ungenauigkeiten im Vorgang eine zufälligere Verteilung der Fehler aufweisen. Im Ergebnis können die an den Grenzen auftretenden Samplingfehler deutlicher bemerkbar werden, da der Referenz-Rasterizer diesen Vorgang nicht mit absoluter Genauigkeit durchführt.

Der beste Ansatz besteht in der Verwendung der Filterung am nächstgelegenen Punkt nur bei Bedarf. Wenn Sie diese verwenden müssen, empfiehlt es sich die Texturkoordinaten geringfügig von den Grenzpositionen zu versetzen, um Artefakte zu vermeiden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturfilterung](texture-filtering.md)

 

 




