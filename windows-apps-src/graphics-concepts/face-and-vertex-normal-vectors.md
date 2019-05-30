---
title: Seiten- und Scheitelnormalenvektoren
description: Jede Fläche in einem Gitter verfügt über einen normalen Einheitsvektor im rechten Winkel. Die Richtung des Vektors wird von der Reihenfolge bestimmt, in der die Scheitelpunkte definiert sind und ist abhängig davon, ob das Koordinatensystem rechts - oder linkshändig ist.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- Seiten- und Scheitelnormalenvektoren
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7146fe6489d61c641b7104e8c7c25728550c1c54
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370666"
---
# <a name="face-and-vertex-normal-vectors"></a>Seiten- und Scheitelnormalenvektoren


Jede Fläche in einem Gitter verfügt über einen normalen Einheitsvektor im rechten Winkel. Die Richtung des Vektors wird von der Reihenfolge bestimmt, in der die Scheitelpunkte definiert sind und ist abhängig davon, ob das Koordinatensystem rechts - oder linkshändig ist.

## <a name="span-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>Senkrecht Einheit Normalenvektor für eine Vorderseite


Jede Fläche in einem Gitter verfügt über einen normalen Einheitsvektor im rechten Winkel. Die Richtung des Vektors wird von der Reihenfolge bestimmt, in der die Scheitelpunkte definiert sind und ist abhängig davon, ob das Koordinatensystem rechts - oder linkshändig ist. Der Fußpunkt des Normalvektors einer Fläche liegt auf ihrer Vorderseite. In Direct3D ist nur die Vorderseite einer Fläche sichtbar. Eine Vorderseite ist dadurch gekennzeichnet, dass auf ihr die Vertices im Uhrzeigersinn definiert sind.

Die folgende Abbildung zeigt einen Normalvektor für eine Vorderseite:

![ein Normalvektor für eine Vorderseite](images/nrmlvect.png)

## <a name="span-idcullingbackfacesspanspan-idcullingbackfacesspanspan-idcullingbackfacesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>Gesichter Culling zurück


Jede Fläche, die keine Vorderseite ist, ist eine Rückseite. Direct3D unterstützt Rückseitenentfernung, d.°h. Rückseiten werden nicht immer gerendert. Bei der Rückseitenentfernung werden die Rückseiten nicht gerendert. Sie können den Cullingmodus ändern, um Rückseiten doch zu rendern. Weitere Informationen finden Sie unter [Culling-Status](https://docs.microsoft.com/windows/desktop/direct3d9/culling-state).

## <a name="span-idvertexunitnormalsspanspan-idvertexunitnormalsspanspan-idvertexunitnormalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>Flächenspezifische-Einheit


Direct3D verwendet die Vertexeinheitsnormalen für Gouraud-Schattierung, Beleuchtung und Textureffekte.

Die folgende Abbildung zeigt die Vertexnormalen:

![Vertexnormale](images/vertnrml.png)

Beim Anwenden von Gouraud-Schattierung auf ein Polygon verwendet Direct3D die Vertexnormalen, um den Winkel zwischen Lichtquelle und Oberfläche zu berechnen. Die Farb- und Intensitätswerte für die Vertices werden berechnet und für jeden Punkt auf allen Grundformoberflächen interpoliert. Direct3D berechnet den Lichtintensitätswert mithilfe des Winkels. Je größer der Winkel, desto weniger fällt auf die Oberfläche.

## <a name="span-idflatsurfacesspanspan-idflatsurfacesspanspan-idflatsurfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>Flache Flächen


Wenn Sie ein flaches Objekt erstellen, legen Sie die Vertexnormalen so fest, das sie im rechten Winkel zur Oberfläche stehen.

Die folgende Abbildung zeigt eine flache Oberfläche, die aus zwei Dreiecken mit Vertexnormalen besteht:

![Flache Oberfläche, bestehend aus zwei Dreiecken mit Vertexnormalen](images/flatvert.png)

## <a name="span-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>Sanfter Übergang auf eine nicht-flache-Objekt


Wahrscheinlich ist Ihr Objekt jedoch nicht plan, sondern besteht aus Dreieckstrips, die nicht koplanar sind. Eine einfache Methode, um übergangslose Schattierung über alle Dreiecke im Strip zu erzielen, besteht darin, zuerst den Oberflächennormalvektor für jede polygonale Fläche zu berechnen, welcher der Vertex zugeordnet ist. Die Vertexnormale kann so festgelegt werden, dass sie mit jeder Oberflächennormalen den gleichen Winkel bildet. Diese Methode ist aber für komplexe Grundformen möglicherweise nicht effizient genug.

Diese Methode wird im folgende Diagramm veranschaulicht, in dem zwei Oberflächen, S1 und S2, von oben gesehen dargestellt sind. Die Normalvektoren für S1 und S2 sind blau. Der Vertexnormalvektor ist rot. Der Winkel, den der Vertexnormalvektor mit der Oberflächenormalen von S1 bildet, ist identisch mit den Winkel zwischen der Vertexnormalen und der Flächenormalen von S2. Wenn diese beiden Oberflächen beleuchtet und mit einer Gouraud-Schattierung versehen werden, ergibt das einen übergangslos schattierten und abgerundeten Rand zwischen ihnen.

Die folgende Illustration zeigt zwei Oberflächen (S1 und S2) mit ihren Normalvektoren und Vertexnormalenvektoren:

![Zwei Oberflächen (s1 und s2) mit ihren Normalvektoren und Vertexnormalenvektoren](images/gvert.png)

Wenn die Vertexnormale in Richtung einer der Flächen zeigt, der sie zugeordnet ist, wird die Lichtintensität für Punkte auf der Fläche erhöht oder vermindert, je nach dem mit der Lichtquelle gebildeten Winkel. In der folgenden Abbildung wird ein Beispiel gezeigt. Diese Ränder der Oberflächen liegen aneinander. Die Vertexnormale zeigt in Richtung S1, sodass sie mit der Lichtquelle einen Winkel bildet, der kleiner ist, als es die identischen Winkel mit den Oberflächennormalen wären.

Die folgende Abbildung zeigt zwei Oberflächen (S1 und S2) mit einem Vertexnormalvektor, der sich zu eine Fläche neigt:

![Zwei Oberflächen (s1 und s2) mit einem Vertexnormalvektor, der sich zu einer Fläche neigt](images/gvert2.png)

## <a name="span-idsharpedgesspanspan-idsharpedgesspanspan-idsharpedgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>Scharfe Kanten


Sie können Gouraud-Schattierung verwenden, um einige Objekte in einer 3D-Szene mit scharfe Kanten darzustellen. Duplizieren Sie hierzu die Vertexnormalvektoren für jede Flächenüberschneidung, an der eine scharfe Kante erforderlich ist.

Die folgende Illustration zeigt duplizierte Vertexnormalevektoren an scharfen Kanten:

![Duplizierte Vertexnormalvektoren an scharfen Kanten](images/shade1.png)

Wenn Sie die DrawPrimitive-Methoden zum Rendern der Szene verwenden, definieren Sie das Objekt mit den scharfen Kanten als Dreiecksliste und nicht als Dreiecksstrip. Wenn Sie ein Objekt als Dreiecksstrip definieren, behandelt Direct3D es als ein einziges Polygon, das aus mehreren dreieckigen Flächen besteht. Gouraud-Schattierung wird sowohl auf jeder Fläche des Polygons als auch zwischen benachbarten Flächen angewendet.

Das Ergebnis ist stufenlos von Fläche zu Fläche schattiertes Objekt. Da eine Dreiecksliste ein Polygon ist, das aus einer Reihe unabhängiger dreieckiger Flächen besteht, wendet Direct3D die Gouraud-Schattierung auf jeder Fläche des Polygons an. Jedoch nicht flächenübergreifend. Wenn zwei oder mehr Dreiecke einer Liste nebeneinander liegen, wird eine scharfe Kante zwischen ihnen angezeigt.

Eine weitere Alternative ist, flache Schattierung zum Rendern von Objekten mit scharfen Kanten zu verwenden. Dies ist rechnerisch die effizienteste Methode, kann aber dazu führen, dass solche Objekte in der Szene nicht so realistisch gerendert werden wie die Objekte mit der Gouraud-Schattierung.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Koordinatensysteme und Geometrie](coordinate-systems-and-geometry.md)

 

 




