---
title: Projektionstransformation
description: Ein Projektionstransformation steuert die internen Elemente der Kamera, z. B. die Auswahl einer Linse für eine Kamera. Dies ist der komplizierteste der drei Transformationstypen.
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- Projektionstransformation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f0806c0aa7a130a080457f4361d17f64451846f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634545"
---
# <a name="projection-transform"></a>Projektionstransformation


Eine *Projektionstransformation* steuert die internen Elemente der Kamera, z. B. die Auswahl eines Objektivs für eine Kamera. Dies ist der komplizierteste der drei Transformationstypen.

Die Projektionsmatrix ist in der Regel eine Skalierung und perspektivische Projektion. Die Projektionstransformation konvertiert das Ansichtsfrustum in eine Quaderform. Da das nähergelegene Ende des Ansichtsfrustrums kleiner als das weiter entfernt liegende Ende, hat dies die Wirkung, dass näher bei der Kamera liegende Objekte erweitert werden. So wird Perspektive auf die Szene angewandt.

Im [Anzeigefrustrum](viewports-and-clipping.md) ist der Abstand zwischen der Kamera und dem Ursprung des Ansichtstransformationsbereichs beliebig als D definiert. Daher sieht die Projektionsmatrix so aus wie in der folgenden Abbildung gezeigt.

![Abbildung der Projektionsmatrix](images/projmat1.png)

Die Ansichtsmatrix verschiebt die Kamera an den Ursprung, indem um -D in die z-Richtung verschoben wird. Die Verschiebungsmatrix entspricht der folgenden Abbildung.

![Abbildung der Verschiebungsmatrix](images/projmat2.png)

Die translationsmatrix durch die Projektionsmatrix Multiplikation (T\*P) können Sie die zusammengesetzte Projektionsmatrix, wie in der folgenden Abbildung dargestellt.

![Abbildung der zusammengesetzten Projektionsmatrix](images/projmat3.png)

Die perspektivische Transformation konvertiert ein Ansichtsfrustum in einen neuen Koordinatenbereich. Beachten Sie, dass das Frustum Quaderform annimmt und dass sich der Ursprung von der oberen rechten Ecke der Szene zur Mitte verschiebt, wie im folgenden Diagramm dargestellt.

![Diagramm, das zeigt, wie die perspektivische Transformation das Ansichtsfrustrum in einen neuen Koordinatenbereich ändert](images/cuboid.png)

In der perspektivischen Transformation sind die Grenzen von X- und Y-Richtung -1 und 1. Die Grenzen der Z-Richtung sind für 0 die vordere Ebene und 1 für die hintere Ebene.

Diese Matrix verschiebt und skaliert Objekte basierend auf einem angegebenen Abstand von der Kamera zur vorderen Clippingebene, berücksichtigt aber nicht das Blickfeld, und die erzeugten Z-Werte für Objekte in der Entfernung können nahezu identisch sein, was Tiefenvergleiche erschwert. Die folgende Matrix behebt diese Probleme und passt Scheitelpunkte an, um das Seitenverhältnis des Viewports zu berücksichtigen. Daher ist sie eine gute Wahl für perspektivische Projektion.

![Abbildung einer Matrix für die perspektivische Projektion](images/prjmatx1.png)

In dieser Matrix ist Zₙ der Z-Wert der vorderen Clippingebene. Die Variablen w, h und Q haben folgende Bedeutung. Beachten Sie, dass Blickfeld<sub>w</sub> und Blickfeldₖ die horizontalen und vertikalen Blickfelder im Bogenmaß darstellen.

![Formeln der Variablenbedeutung](images/prjmatx2.png)

Für Ihre Anwendung ist die Verwendung von Blickfeldwinkeln zum Definieren der X- und Y-Skalierungskoeffizienten möglicherweise nicht so günstig wie der Einsatz der horizontalen und vertikalen Abmessungen des Viewports (im Kamerabereich). Mathematisch verwenden die folgenden beiden Gleichungen für w und h die Abmessungen des Viewports und entsprechen den obigen Gleichungen.

![Gleichungen der Variablenbedeutung für w und h](images/prjmatx3.png)

In diesen Formeln steht Zₙ für die Position der vorderen Clippingebene, und die Variablen V<sub>w</sub> und Vₕ stellen die Breite und Höhe des Viewports im Kamerabereich dar.

Unabhängig von der Formel, für die Sie sich entscheiden, sollten Sie den Zₙ-Wert so groß wie möglich festlegen, da sehr nahe bei der Kamera liegende Z-Werte kaum variieren. Das erschwert Tiefenvergleiche mit 16-Bit-z-Puffern.

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>Eine w-freundliche Projektionsmatrix


Direct3D kann die w-Komponente eines Scheitelpunkts, der durch die Welt-, Ansichts- und Projektionsmatrizen umgewandelt wurde, für tiefenbasierte Berechnungen im Tiefenpuffer oder bei Nebeleffekten einsetzen. Für derartige Berechnungen muss Ihre Projektionsmatrix w so normalisieren, dass es dem Welt-Bereich z entspricht. Wenn Ihre Projektionsmatrix also einen (3,4)-Koeffizienten enthält, der nicht 1 ist, müssen Sie alle Koeffizienten mit dem umgekehrten Wert des (3,4)-Koeffizienten skalieren, um eine korrekte Matrix zu erhalten. Wenn Sie keine kompatible Matrix bereitstellen, werden Nebeleffekte und Tiefenpufferung nicht ordnungsgemäß angewendet.

Die folgende Abbildung zeigt eine nicht kompatible Projektionsmatrix und die gleiche Matrix so skaliert, dass Nebel im Bezug zum Auge ermöglicht wird.

![Abbildungen einer nicht kompatiblen Projektionsmatrix und einer Matrix mit Nebel im Bezug zum Auge](images/eyerlmx.png)

In den vorangehenden Matrizen wird davon ausgegangen, dass alle Variablen ungleich Null sind. Informationen zur w-basierten Tiefenpufferung finden Sie unter [Tiefenpuffer](depth-buffers.md).

Direct3D verwendet die derzeit festgelegte Projektionsmatrix in ihren w-basierten Tiefenberechnungen. Daher müssen Anwendungen eine kompatible Projektionsmatrix festlegen, um die gewünschten w-basierten Funktionen zu erhalten, auch wenn sie Direct3D nicht für Transformationen verwenden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Transformationen](transforms.md)

 

 




