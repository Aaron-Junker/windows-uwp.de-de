---
title: Übersicht über Transformationen
description: Matrix-Transformationen kümmern sich um einen Großteil der einfachen Mathematik von 3D-Grafiken.
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1b6be8ee8aa67196581907087d99e0324d741a00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640145"
---
# <a name="transform-overview"></a>Übersicht über Transformationen


Matrix-Transformationen kümmern sich um einen Großteil der einfachen Mathematik von 3D-Grafiken.

Die Geometrie-Pipeline verwendet Scheitelpunkte als Eingabe. Die Transform-Engine wendet die Welt-, Ansichts- und Projektionstransformationen auf die Scheitelpunkte an, beschneidet das Ergebnis und übergibt alles an den Rasterizer.

| Transformation und Bereich                           | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Modellkoordinaten im Modellbereich              | Am Anfang der Pipeline werden die Scheitelpunkte eines Modells relativ zu einem lokalen Koordinatensystems deklariert. Dies ist ein lokaler Ursprung und eine Ausrichtung. Diese Ausrichtung der Koordinaten wird häufig als *Modellbereich* bezeichnet. Einzelne Koordinaten werden als *Modellkoordinaten* bezeichnet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Welttransformation in den Weltbereich              | Die erste Phase der Geometrie-Pipeline transformiert die Scheitelpunkte eines Modells aus ihrem lokalen Koordinatensystem in ein Koordinatensystem, das von allen Objekten in einer Szene verwendet wird. Der Prozess der Neuorientierung der Scheitelpunkte wird als [Welttransformation](world-transform.md) bezeichnet; dies ist die Konvertierung vom Modellbereich zu einer neuen Orientierung, die als *Weltbereich* bezeichnet wird. Jeder Scheitelpunkt im Weltbereich wird durch *Weltkoordinaten* angegeben.                                                                                                                                                                                                                                                                                                                           |
| Ansichtstransformation zum Ansichtsbereich (Kamerabereich) | Im nächsten Schritt werden die Scheitelpunkte, die Ihre 3D-Welt beschreiben, hinsichtlich der Kamera ausgerichtet. Dies bedeutet, dass Ihre Anwendung einen Ansichtspunkt für die Szene auswählt, und dass die Weltbereichskoordinaten um die Kameraansicht neu angeordnet und gedreht werden, wodurch der Weltbereich zum *Ansichtsbereich* (bzw. *Kamerabereich*) wird. Dies ist die [Ansichtstransformation](view-transform.md), die Konvertierung vom Ansichtsbereich zum Weltbereich.                                                                                                                                                                                                                                                                                                                        |
| Transformation der Projektion zum Projektionsbereich    | Der nächste Schritt ist die [Projektionstransformation](projection-transform.md), die Konvertierung vom Ansichtsbereich zum Projektionsbereich. In diesem Teil der Pipeline werden Objekte in der Regel in Bezug auf ihre Entfernung vom Betrachter skaliert, um einer Szene die Illusion von Tiefe zu geben. Nahe Objekte erscheinen dadurch größer als entfernte Objekte. Der Einfachheit halber bezeichnet diese Dokumentation den Bereich mit den Scheitelpunkten nach der Projektionstransformation als *Projektionsbereich*. Einige Grafikbücher bezeichnen den Projektionsbereich möglicherweise als *postperspektivischen homogenen Bereich*. Nicht alle Projektionstransformationen skalieren Objekte in einer Szene nach der Größe. Eine solche Projektion wird manchmal als *affine* oder *orthogonale Projektion* bezeichnet. |
| Zuschneiden im Bildschirmbereich                      | Im letzten Teil der Pipeline werden alle Scheitelpunkte, die nicht auf dem Bildschirm zu sehen sind, entfernt, so dass der Rasterizer keine Zeit für die Berechnung der Farben und der Schattierungen für Dinge aufwenden muss, die niemals zu sehen sind. Dieser Vorgang wird als *Zuschneiden* bezeichnet. Nach dem Zuschneiden werden die verbleibenden Scheitelpunkte nach den Viewportparametern skaliert und in Bildschirmkoordinaten umgewandelt. Die resultieRendern Scheitelpunkte, die bei der Rasterisierung der Szene auf dem Bildschirm zu sehen sind, befinden sich im *Bildschirmbereich*.                                                                                                                                                                                                                                                    |

 

Transformationen werden verwendet, um die Objektgeometrie von einem Koordinatenbereich zu einem anderen zu konvertieren. Direct3D verwendet Matrizen für die Durchführung von 3D-Transformationen. Matrizen erstellen-3D-Transformationen. Sie können Matrizen kombinieren, um eine einzelne Matrix zu erstellen, die dann mehrere Transformationen umfasst.

Sie können Koordinaten zwischen Modellbereich, Weltbereich und Ansichtsbereich transformieren.

-   [Welttransformation](world-transform.md) - Konvertiert vom Modellbereich zum Weltbereich.
-   [Ansichtstransformation](view-transform.md) - Konvertiert vom Weltbereich zum Ansichtsbereich.
-   [Projektionstransformation](projection-transform.md) -Konvertiert vom Ansichtsbereich zum Projektionsbereich.

## <a name="span-idmatrixtransformsspanspan-idmatrixtransformsspanspan-idmatrixtransformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>Matrixtransformationen


In Anwendungen, die mit 3D-Grafiken arbeiten, können Sie mithilfe geometrischer Transformationen Folgendes tun:

-   Ausdrücken des Orts eines Objekts im Verhältnis zu einem anderen Objekt.
-   Drehen und Dimensionieren von Objekten.
-   Ändern von Ansichtspositionen, -richtungen und -perspektiven.

Sie können einen beliebigen Punkt (x, y, z) mithilfe einer 4x4-Matrix in einen anderen Punkt (x', y', z') transformieren, wie die folgende Gleichung zeigt.

![Gleichung für die Transformation eines Punktes in einen anderen Punkt](images/matmult.png)

Führen Sie die folgenden Gleichungen für (x, y, z) und die Matrix durch, um den Punkt (x', y', z') zu erhalten.

![Gleichungen für den neuen Punkt](images/matexpnd.png)

Die am häufigsten verwendeten Transformationen sind Übersetzung, Drehung und Skalierung. Sie können die Matrizen, die diese Effekte produzieren, zu einer einzelnen Matrix kombinieren, um mehrere Transformationen gleichzeitig zu berechnen. Beispielsweise können Sie eine einzelne Matrix zum Übersetzen und Drehen eine Reihe von Punkten erstellen.

Matrizen werden in der Reihenfolge Zeile-Spalte geschrieben. Die folgende Matrix repräsentiert, in mathematischer Notation, eine Matrix, die Scheitelpunkte entlang der einzelnen Achsen gleichmäßig skaliert (einheitliche Skalierung).

![Gleichung einer Matrix für die einheitliche Skalierung](images/matrix.png)

In C++ deklariert Direct3D Matrizen als zweidimensionale Arrays mit einer Matrix-Struktur. Das folgende Beispiel demonstriert die Initialisierung einer [**D3DMATRIX**](https://msdn.microsoft.com/library/windows/desktop/bb172573)-Struktur zur Funktion als einheitliche Skalierungsmatrix (Skalierungsfaktor „s“.

```
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>Übersetzen


Die folgende Gleichung übersetzt den Punkt (x, y, z) zu einem neuen Punkt (x', y', z').

![Gleichung einer Übersetzungsmatrix für einen neuen Punkt](images/transl8.png)

Sie können eine Übersetzungsmatrix manuell in C++ erstellen. Das folgende Beispiel zeigt den Quellcode für eine Funktion, die eine Matrix zum Übersetzen von Scheitelpunkten erstellt.

```
D3DXMATRIX Translate(const float dx, const float dy, const float dz) {
    D3DXMATRIX ret;

    D3DXMatrixIdentity(&ret);
    ret(3, 0) = dx;
    ret(3, 1) = dy;
    ret(3, 2) = dz;
    return ret;
}    // End of Translate
```

## <a name="span-idscalespanspan-idscalespanspan-idscalespanscale"></a><span id="Scale"></span><span id="scale"></span><span id="SCALE"></span>Skalieren


Die folgende Gleichung skaliert den Punkt (x, y, z), um beliebige Werte in der x-, y- und z-Richtung zu einem neuen Punkt (x', y', z').

![Gleichung für eine Skalierungsmatrix für einen neuen Punkt](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>Drehen


Die hier beschriebenen Transformationen sind für linkshändige Koordinatensysteme gedacht und unterscheiden sich daher möglicherweise von Transformationsmatrizen, die Sie anderswo gesehen haben könnten.

Die folgende Gleichung dreht den Punkt (x, y, z) um die x-Achse und erzeugt so einen neuen Punkt (x', y', z').

![Gleichung einer x-Drehungsmatrix für einen neuen Punkt](images/matxrot.png)

Die folgende Gleichung dreht den Punkt um die y-Achse.

![Gleichung einer y-Drehungsmatrix für einen neuen Punkt](images/matyrot.png)

Die folgende Gleichung dreht den Punkt um die z-Achse.

![Gleichung einer z-Drehungsmatrix für einen neuen Punkt](images/matzrot.png)

In diesen Beispielmatrizen steht der griechische Buchstabe Theta für den Drehwinkel (in Bogenmaß). Winkel werden im Uhrzeigersinn entlang der Drehachse zum Ursprung gemessen.

Der folgende Code zeigt eine Funktion für die Drehung um die x-Achse.

```
    // Inputs are a pointer to a matrix (pOut) and an angle in radians.
    float sin, cos;
    sincosf(angle, &sin, &cos);  // Determine sin and cos of angle

    pOut->_11 = 1.0f; pOut->_12 =  0.0f;   pOut->_13 = 0.0f; pOut->_14 = 0.0f;
    pOut->_21 = 0.0f; pOut->_22 =  cos;    pOut->_23 = sin;  pOut->_24 = 0.0f;
    pOut->_31 = 0.0f; pOut->_32 = -sin;    pOut->_33 = cos;  pOut->_34 = 0.0f;
    pOut->_41 = 0.0f; pOut->_42 =  0.0f;   pOut->_43 = 0.0f; pOut->_44 = 1.0f;

    return pOut;
}
```

## <a name="span-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>Verketten von Matrizen


Ein Vorteil der Verwendung von Matrizen ist, dass die Effekte von zwei oder mehr Matrizen kombiniert werden können, indem Sie sie multiplizieren. Dies bedeutet, dass Sie nicht zwei Matrizen verwenden müssen, um ein Modell zu drehen und dann zu einem bestimmten Ort zu übersetzen. Stattdessen multiplizieren Sie die Drehungs- und Übersetzungsmatrizen und erzeugen so eine kombinierte Matrix, die alle diese Effekte enthält. Dieser Vorgang wird als Matrixverkettung bezeichnet und kann mit der folgenden Gleichung geschrieben werden.

![Gleichung für die Matrixverkettung](images/matrxcat.png)

In dieser Gleichung ist C die kombinierte Matrix, die erstellt wird, und M₁ bis Mₙ sind die einzelnen Matrizen. In den meisten Fällen werden nur zwei oder drei Matrizen verkettet, es gibt aber keine Beschränkung.

Die Reihenfolge, in der die Matrixmultiplikation durchgeführt wird, ist entscheidend. Die oben aufgeführte Formel gibt die Links-nach-Rechts-Regel für die Matrixverkettung wieder. Dies bedeutet, dass die sichtbaren Effekte der Matrizen, aus denen Sie eine kombinierte Matrix erstellen, von links nach rechts auftreten. Das folgende Beispiel zeigt eine typische Weltmatrix. Stellen Sie sich vor, dass Sie die Weltmatrix für eine klischeehafte fliegende Untertasse erstellen. Möglicherweise soll diese sich um ihren Mittelpunkt drehen – die y-Achse des Modellbereichs – und an einen anderen Ort in Ihrer Szene übersetzt werden. Um diesen Effekt zu erzielen, erstellen Sie zuerst eine Drehungsmatrix und multiplizieren diese dann mit einer Übersetzungsmatrix, wie in der folgenden Gleichung gezeigt.

![Gleichung einer auf einer Drehungsmatrix und einer Übersetzungsmatrix basieRendern Rotation](images/wrldexpl.png)

In dieser Formel ist R<sub>y</sub> eine Matrix für die Drehung um die y-Achse, und T<sub>w</sub> ist eine Übersetzung zu einer Position in Weltkoordinaten.

Die Reihenfolge, in der Sie die Matrizen multiplizieren, ist wichtig, da die Matrix-Multiplikation (anders als die Multiplikation zweier skalarer Werte) nicht kommutativ ist. Die Multiplikation der Matrizen in umgekehrter Reihenfolge führt zu einem visuellen Effekt, bei dem die fliegende Untertasse an ihre Weltbereichsposition übersetzt wird und dann um den Weltursprungspunkt rotiert.

Unabhängig davon, welche Art von Matrix Sie erstellen: Beachten Sie immer die Links-nach-Rechts-Regel, um sicherzustellen, dass Sie die gewünschten Effekte erzielen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Transformationen](transforms.md)

 

 




