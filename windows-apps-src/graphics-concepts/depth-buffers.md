---
title: Tiefenpuffer
description: Ein Tiefenpuffer oder Z-Puffer speichert Tiefeninformationen. Diese steuern, welche Bereiche von Polygonen dargestellt werden.
ms.assetid: BE83A1D7-D43D-4013-8817-EFD2B24DC58E
keywords:
- Tiefenpuffer
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 279e650532505467f3c0dbabf3814618b893aedb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637025"
---
# <a name="depth-buffers"></a>Tiefenpuffer


Ein *Tiefenpuffer* oder *Z-Puffer* speichert Tiefeninformationen. Diese steuern, welche Bereiche von Polygonen dargestellt werden.

## <a name="span-idoverviewspanspan-idoverviewspanspan-idoverviewspanoverview"></a><span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>Übersicht über die


Ein Tiefenpuffer oder auch Z-Puffer bzw. W-Puffer ist ein Element des Geräts, das Tiefeninformationen für die Nutzung durch Direct3D speichert. Beim Rendern einer Szene auf eine Zieloberfläche durch Direct3D kann der Speicher für den einer Oberfläche zugeordneten Tiefenpuffer als Arbeitsbereich zur Bestimmung der Überlagerung der gerasterten Pixel von Polygonen genutzt werden. Direct3D verwendet eine Direct3D-Oberfläche außerhalb des Bildschirms als Ziel zum Schreiben der endgültigen Farbwerte. Die Tiefenpufferoberfläche, die der Renderzieloberfläche zugeordnet ist, wird verwendet, um Tiefeninformationen zu speichern. Diese zeigen Direct3D, wie tief jedes sichtbare Pixel in der Szene angeordnet ist.

Wenn eine Szene mit aktivierter Tiefenpufferung gerastert wird, wird jeder Punkt auf der Renderingoberfläche getestet. Bei den Werten im Tiefenpuffer kann es sich um die Z-Koordinate des Punkts oder dessen homogene W-Koordinate (aus der X, Y, Z, W-Position des Punktes im Projektionsbereich) handeln. Ein Tiefenpuffer, der Z-Werte verwendet, wird häufig als Z-Puffer bezeichnet. Ein Puffer, der W-Werte verwendet, wird W-Puffer genannt. Jede Art von Tiefenpuffer hat Vor- und Nachteile. Diese werden später erläutert.

Am Anfang des Tests, wird der Wert für die Tiefe im Tiefenpuffer auf den größten Wert für die Szene festgelegt. Der Farbwert auf der Renderingoberfläche wird entweder auf den Wert der Hintergrundfarbe oder den Farbwert der Hintergrundtextur an diesem Punkt festgelegt. Jedes Polygon in der Szene wird getestet. So wird festgestellt, ob er sich mit der aktuellen Koordinate (x, y) auf der Renderingoberfläche überschneidet.

Wenn er sich überschneidet, wird der Tiefenwert (die Z-Koordinate in einem Z-Puffer oder die W-Koordinate in einem W-Puffer) am aktuellen Punkt getestet, um festzustellen, ob dieser kleiner als der im Tiefenpuffer gespeicherte Tiefenwert ist. Wenn der Tiefenwert des Polygons kleiner ist, wird dieser im Tiefenpuffer gespeichert. Der Farbwert des Polygons wird in den aktuellen Punkt auf der Renderingoberfläche geschrieben. Wenn der Tiefenwert des Polygons an diesem Punkt größer ist, wird die nächste Polygon in der Liste getestet. Dieser Prozess wird im folgenden Diagramm dargestellt.

![Diagramm zum Testen von Tiefenwerten](images/zbuffer.png)

## <a name="span-idbufferingtechniquesspanspan-idbufferingtechniquesspanspan-idbufferingtechniquesspanbuffering-techniques"></a><span id="Buffering_techniques"></span><span id="buffering_techniques"></span><span id="BUFFERING_TECHNIQUES"></span>Pufferung Techniken


Obwohl die meisten Anwendungen dieses Feature nicht verwenden, ist es möglich, den von Direct3D zur Bestimmung der in den Tiefenpuffer und in die Renderzieloberfläche zu schreibenden Werte verwendeten Vergleich zu ändern. Auf bestimmter Hardware ist es möglich, das die Änderung der Vergleichsfunktion das Testen von hierarchischen Z-Werten deaktiviert.

Fast alle im Markt verfügbaren Hardwarebeschleuniger unterstützten die Z-Pufferung. Die Z-Pufferung ist daher heute der am häufigsten verwendeten Tiefenpuffer-Typ. Trotz ihrer breitflächigen Nutzung haben Z-Puffer Nachteile. Aufgrund der erforderlichen Berechnungen tendieren die generierten Z-Werte in einem Z-Puffer zu einer ungleichmäßigen Verteilung auf den Z-Puffer-Bereich (in der Regel zwischen 0,0 und 1,0 – einschließlich dieser beiden Randwerte).

Besonders das Verhältnis zwischen den entfernten und nahen Clippingebenen wirkt sich stark auf die ungleichmäßige Verteilung der Z-Werte aus. Mit einem Distanzverhältnis von 100 zwischen entfernter Ebene und naher Ebene wird 90 Prozent des Pufferbereichs für die ersten 10 Prozent des Tiefenbereichs der Szene genutzt. Standardanwendungen im Unterhaltungsbereich oder visuelle Simulationen mit Szenen in Außenbereichen erfordern häufig ein Verhältnis zwischen entfernter und naher Ebene, das zwischen 1.000 und 10.000 liegt. Bei einem Verhältnis von 1.000 wird 98 Prozent des Bereichs für die erste 2 Prozent des Tiefenbereichs genutzt. Bei höheren Verhältnissen wird die Verteilung noch schlechter. Dies kann zu versteckten Oberflächenartefakten bei entfernten Objekten führen – insbesondere bei der Verwendung von 16-Bit-Tiefenpuffern (die am häufigsten unterstützten Bittiefe).

Bei einem W-basierten Tiefenpuffer ist die Verteilung zwischen den nahen und entfernten Clippingebenen hingegen häufig gleichmäßiger als bei einem Z-Puffer. Der wichtigste Vorteil ist, dass das Entfernungsverhältnis zwischen den entfernten und nahen Clippingebenen kein Problem mehr darstellt. Anwendungen können so mit großen Maximalbereichen arbeiten und erhalten trotzdem eine relativ präzise Tiefenpufferung für nahe Punkte. Ein W-basierter Tiefenpuffer ist jedoch auch nicht perfekt. Manchmal kann es zu versteckten Oberflächenartefakten bei nahen Objekten kommen. Ein weiterer Nachteil des W-gepufferten Ansatzes liegt in der Hardwareunterstützung: Die W-Pufferung wird nicht so weitflächig unterstützt wie die Z-Pufferung.

Bei einem Z-Puffer ist außerdem Mehraufwand beim Rendern erforderlich. Bei Z-Puffern können verschiedene Techniken zur Optimierung des Renderings eingesetzt werden. Anwendungen können die Leistung bei der Verwendung von Z-Puffern und Texturen verbessern, indem Sie dafür sorgen, dass die Szenen von vorne nach hinten gerendert werden. Texturierte, Z-gepuffert Primitiven werden auf Basis von Scan-Linien gegen den Z-Puffer vorgetestet. Wenn eine Scan-Linie von einem zuvor gerenderten Polygon verdeckt wird, verwirft das System diese schnell und effizient. Z-Puffer können die Leistung verbessern. Die Technik ist dann besonders hilfreich, wenn in einer eine Szene Pixel mehrmals gezeichnet werden. Der exakte Leistungsgewinn kann nur schwer berechnet werden. Häufig können Sie jedoch einen Näherungswert bestimmen. Wenn ein Pixel weniger als zweimal gezeichnet wird, erreichen Sie mit der Deaktivierung der Z Pufferung und dem Rendern der Szene von vorne nach hinten die beste Leistung.

Die tatsächlichen Interpretation eines Tiefenwerts ist je nach Renderer unterschiedlich.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Tiefe und Schablone Puffer](depth-and-stencil-buffers.md)

 

 




