---
title: Seiten- und Scheitelnormalenvektoren
description: Jedes Gesicht in einem Mesh hat einen senkrechten Einheits Vektor. Die Richtung des Vektors wird durch die Reihenfolge bestimmt, in der die Scheitel Punkte definiert werden, und durch die Festlegung, ob das Koordinatensystem von rechts oder Links übergeben wird.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- Seiten- und Scheitelnormalenvektoren
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ef0d3ea5a3bc0f5c4ac6b6b660dc543919d297ec
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168164"
---
# <a name="face-and-vertex-normal-vectors"></a>Seiten- und Scheitelnormalenvektoren


Jedes Gesicht in einem Mesh hat einen senkrechten Einheits Vektor. Die Richtung des Vektors wird durch die Reihenfolge bestimmt, in der die Scheitel Punkte definiert werden, und durch die Festlegung, ob das Koordinatensystem von rechts oder Links übergeben wird.

## <a name="span-idperpendicular_unit_normal_vector_for_a_front_facespanspan-idperpendicular_unit_normal_vector_for_a_front_facespanspan-idperpendicular_unit_normal_vector_for_a_front_facespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>Senkrechter Einheiten normal Vektor für eine Vorderseite


Jedes Gesicht in einem Mesh hat einen senkrechten Einheits Vektor. Die Richtung des Vektors wird durch die Reihenfolge bestimmt, in der die Scheitel Punkte definiert werden, und durch die Festlegung, ob das Koordinatensystem von rechts oder Links übergeben wird. Der Vordergrund zeigt sich von der Vorderseite der Vorderseite Weg. In Direct3D ist nur der Vordergrund einer Fläche sichtbar. Eine Vorderseite ist eine, in der Scheitel Punkte im Uhrzeigersinn definiert werden.

Die folgende Abbildung zeigt einen normalen Vektor für eine Vorderseite:

![ein normaler Vektor für eine Vorderseite](images/nrmlvect.png)

## <a name="span-idculling_back_facesspanspan-idculling_back_facesspanspan-idculling_back_facesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>Wiedergabe von Gesichtern


Jedes Gesicht, das kein Vordergrund ist, ist ein Hintergrund. Direct3D gibt nicht immer wieder Gesichter zurück. backgesichter werden als "culled" bezeichnet. Bei der Hintergrund Erkennung wird das Rendern von Hinterflächen vermieden. Wenn Sie möchten, können Sie den Modus ändern, um die Gesichter zu rendereln. Weitere Informationen finden Sie unter [Status der Status](/windows/desktop/direct3d9/culling-state) Informationen.

## <a name="span-idvertex_unit_normalsspanspan-idvertex_unit_normalsspanspan-idvertex_unit_normalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>Normale für Scheitelpunkt Einheiten


Direct3D verwendet die Vertex-Einheits normale für die Schattierung, Beleuchtung und Texturierung von Gouraud.

In der folgenden Abbildung sind Scheitelpunkt normale dargestellt:

![Scheitelpunkt normale](images/vertnrml.png)

Beim Anwenden von Gouraud-Schattierung auf ein Polygon verwendet Direct3D die Scheitelpunkt normale, um den Winkel zwischen der Lichtquelle und der Oberfläche zu berechnen. Sie berechnet die Farb-und Intensitätswerte für die Scheitel Punkte und interpoliert Sie für jeden Punkt in allen primitiven Oberflächen. Direct3D berechnet den Wert für die helle Intensität mithilfe des Winkels. Je größer der Winkel ist, desto weniger hell wird auf der Oberfläche angezeigt.

## <a name="span-idflat_surfacesspanspan-idflat_surfacesspanspan-idflat_surfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>Flache Oberflächen


Wenn Sie ein Objekt erstellen, das flach ist, legen Sie die Scheitelpunkt normale so fest, dass Sie auf die Oberfläche senkrecht zeigen.

Die folgende Abbildung zeigt eine flache Oberfläche, die aus zwei Dreiecken mit Scheitelpunkt normalen besteht:

![flache Oberfläche, die aus zwei Dreiecken mit Scheitelpunkt normalen besteht](images/flatvert.png)

## <a name="span-idsmooth_shading_on_a_non-flat_objectspanspan-idsmooth_shading_on_a_non-flat_objectspanspan-idsmooth_shading_on_a_non-flat_objectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>Smooth-Schattierung für ein nicht flaches Objekt


Anstelle von Flat-Objekten ist es wahrscheinlicher, dass das Objekt aus Dreiecks Streifen besteht und die Dreiecke nicht Coplanar sind. Eine einfache Möglichkeit, eine nahtlose Schattierung über alle Dreiecke des Streifens hinweg zu erreichen, besteht darin, zuerst den Oberflächen normal Vektor für jedes polygonale Gesicht zu berechnen, dem der Scheitelpunkt zugeordnet ist. Der Scheitelpunkt normal kann so festgelegt werden, dass er für jede Oberflächen normale den gleichen Winkel hat. Diese Methode ist jedoch möglicherweise nicht effizient genug für komplexe primitive.

Diese Methode wird anhand des folgenden Diagramms veranschaulicht, das zwei Oberflächen anzeigt: S1 und S2: Edge-on von oben. Die normalen Vektoren für S1 und S2 sind blau dargestellt. Der Vertex-normal Vektor wird rot dargestellt. Der Winkel, den der normal Vektor des Scheitel Punkts mit der Oberflächen Norm S1 herstellt, ist derselbe wie der Winkel zwischen der Scheitelpunkt-normal und der Oberflächen Norm von S2. Wenn diese beiden Oberflächen mit der Gouraud-Schattierung beleuchtet und schattiert werden, ist das Ergebnis ein reibungslos schattierter, nahtlos Abgerundeter Rand zwischen Ihnen.

Die folgende Abbildung zeigt zwei Oberflächen (S1 und S2) und ihre normalen Vektoren und den Scheitelpunkt normal Vektor:

![zwei Oberflächen (S1 und S2) und ihre normalen Vektoren und Vertex-normal Vektor](images/gvert.png)

Wenn sich der Scheitelpunkt normal zu einem der Gesichter bewegt, mit denen er verknüpft ist, bewirkt dies, dass die helle Intensität für Punkte auf dieser Oberfläche vergrößert oder verringert wird, je nachdem, wie sich die Lichtquelle ergibt. Das folgende Diagramm zeigt ein Beispiel. Diese Oberflächen werden als Rand angezeigt. Der Scheitelpunkt normal ist in Richtung S1 und bewirkt, dass er einen kleineren Winkel mit der Lichtquelle hat, als wenn der Scheitelpunkt normal mit den Oberflächen Normals gleich groß ist.

In der folgenden Abbildung werden zwei Oberflächen (S1 und S2) mit einem normalen Scheitelpunkt Vektor angezeigt, der sich auf ein Gesicht stützt:

![zwei Oberflächen (S1 und S2) mit einem Vertex-normal Vektor, der sich auf ein Gesicht bewegt](images/gvert2.png)

## <a name="span-idsharp_edgesspanspan-idsharp_edgesspanspan-idsharp_edgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>Spitzen Ränder


Mithilfe von Gouraud-Schattierung können Sie einige Objekte in einer 3D-Szene mit Spitzen Kanten anzeigen. Zu diesem Zweck duplizieren Sie die Scheitel Punkte des Scheitel Punkts bei jeder Schnittmenge von Gesichtern, bei der ein starker Rand erforderlich ist.

Die folgende Abbildung zeigt doppelte Scheitelpunkt normale Scheitel Punkte bei Spitzen Kanten:

![doppelte Vertex-normal Vektoren bei Spitzen Kanten](images/shade1.png)

Wenn Sie die drawprimitive-Methoden zum renderingihrer Szene verwenden, definieren Sie das Objekt mit Spitzen Rändern als Dreiecks Liste und nicht als Dreiecks Streifen. Wenn Sie ein Objekt als Dreiecks Streifen definieren, behandelt Direct3D es als ein einzelnes Polygon, das aus mehreren dreieckigen Flächen besteht. Die Schattierung von Gouraud wird sowohl über jedes Gesicht des Polygons als auch zwischen angrenzenden Gesichtern angewendet.

Das Ergebnis ist ein Objekt, das nahtlos von Gesicht zu Gesicht schattiert wird. Da eine Dreiecks Liste ein Polygon ist, das aus einer Reihe nicht zusammenhängender Dreiecks Flächen besteht, wendet Direct3D die Gouraud-Schattierung auf jedes Gesicht des Polygons an. Sie wird jedoch nicht von Gesicht zu Gesicht angewendet. Wenn zwei oder mehr Dreiecke einer Dreiecks Liste nebeneinander liegen, scheinen Sie einen Spitzen Rand dazwischen zu haben.

Eine andere Alternative besteht darin, beim Rendern von Objekten mit Spitzen Kanten zu einer flachen Schattierung zu wechseln. Dies ist rechnerisch die effizienteste Methode, kann jedoch zu Objekten in der Szene führen, die nicht so realistisch gerendert werden wie die Objekte, bei denen es sich um eine Gouraud-Schattierung handelt.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Koordinatensysteme und Geometrie](coordinate-systems-and-geometry.md)

 

 