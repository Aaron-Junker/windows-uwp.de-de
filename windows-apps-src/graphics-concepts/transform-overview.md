---
title: Übersicht über Transformationen
description: Matrix Transformationen verarbeiten eine Menge der Low-Level-Mathematik von 3D-Grafiken.
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0f8efd1984ae8a726870bd8e7aaa3960baf91218
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156214"
---
# <a name="transform-overview"></a>Übersicht über Transformationen

Matrix Transformationen verarbeiten eine Menge der Low-Level-Mathematik von 3D-Grafiken.

Die Geometrie Pipeline nimmt Vertices als Eingabe. Die Transformations-Engine wendet die Transformationen "World", "View" und "Projection" auf die Scheitel Punkte an, schneidet das Ergebnis ab und übergibt alles an den Rasterizer.

| Transformieren und Leerzeichen                           | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Modell Koordinaten im Modellraum              | Am Anfang der Pipeline werden die Vertices eines Modells relativ zu einem lokalen Koordinatensystem deklariert. Dies ist ein lokaler Ursprung und eine Ausrichtung. Diese Ausrichtung von Koordinaten wird häufig als *Modellraum*bezeichnet. Einzelne Koordinaten werden als *Modell Koordinaten*bezeichnet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Weltweiten Transformations Raum              | In der ersten Phase der Geometrie Pipeline werden die Scheitel Punkte eines Modells von Ihrem lokalen Koordinatensystem in ein Koordinatensystem transformiert, das von allen Objekten in einer Szene verwendet wird. Der Prozess der Umgestaltung der Scheitel Punkte wird als [Welt Transformation](world-transform.md)bezeichnet, die vom Modellbereich in eine neue Ausrichtung namens *World Space*konvertiert. Jeder Scheitelpunkt im Raum wird mit *Weltkoordinaten*deklariert.                                                                                                                                                                                                                                                                                                                           |
| Transformation in Anzeigebereich anzeigen (Kamerabereich) | In der nächsten Phase sind die Scheitel Punkte, die Ihre 3D-Welt beschreiben, in Bezug auf eine Kamera orientiert. Das heißt, Ihre Anwendung wählt eine Sicht Ansicht für die Szene aus, und die Raumkoordinaten der Welt werden verschoben und um die Ansicht der Kamera gedreht. Dadurch wird der Raum in den *Ansichts Raum* (auch als *Kamera Raum*bezeichnet) verwandelt. Dabei handelt es sich um die [Ansichts Transformation](view-transform.md), die aus dem Welt Raum in den Anzeigebereich konvertiert.                                                                                                                                                                                                                                                                                                                        |
| Projektions Transformation in Projektionsfläche    | Die nächste Phase ist die [Projektions Transformation](projection-transform.md), die aus dem Ansichts Raum in den Projektions Bereich konvertiert. In diesem Teil der Pipeline werden Objekte in der Regel auf die Entfernung des Viewers skaliert, um die Illusion von Tiefe an eine Szene zu vermitteln. Close-Objekte werden so erstellt, dass Sie größer als entfernte Objekte werden. Der Einfachheit halber bezieht sich diese Dokumentation auf den Raum, in dem Scheitel Punkte nach der Projektions Transformation als *Projektionsraum*vorhanden sind. In einigen Grafik Büchern wird der Projektionsraum möglicherweise als *homogener Perspektiven Raum*bezeichnet. Nicht alle Projektions Transformationen Skalieren die Größe von Objekten in einer Szene. Eine Projektion wie diese wird mitunter als *affine* oder *orthogonale Projektion*bezeichnet. |
| Clipping im Bildschirmbereich                      | Im letzten Teil der Pipeline werden alle Scheitel Punkte, die auf dem Bildschirm nicht sichtbar sind, entfernt, sodass der Raster nicht mehr die Zeit zum Berechnen der Farben und Schattierung für etwas hat, das nie angezeigt wird. Dieser Vorgang wird als *Clipping*bezeichnet. Nach dem Clipping werden die verbleibenden Scheitel Punkte entsprechend den viewportparametern skaliert und in Bildschirm Koordinaten konvertiert. Die resultierenden Scheitel Punkte, die auf dem Bildschirm angezeigt werden, wenn die Szene rasteriert ist, sind im *Bildschirmbereich*vorhanden.                                                                                                                                                                                                                                                    |

 

Transformationen werden verwendet, um die Objekt Geometrie von einem Koordinaten Bereich in einen anderen zu konvertieren. Direct3D verwendet Matrizen, um 3D-Transformationen auszuführen. Matrizen erstellen 3D-Transformationen. Matrizen können kombiniert werden, um eine einzelne Matrix zu entwickeln, die mehrere Transformationen umfasst.

Sie können Koordinaten zwischen Modellraum, Welt Raum und Anzeigebereich transformieren.

-   [World Transform](world-transform.md) : wandelt vom Modellbereich in den Raum um.
-   [View Transform](view-transform.md) : wandelt from World Space in View Space um.
-   [Projektions Transformation](projection-transform.md) : konvertiert von Sichtbereich in Projektionsfläche.

## <a name="span-idmatrix_transformsspanspan-idmatrix_transformsspanspan-idmatrix_transformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>Matrix Transformationen


In Anwendungen, die mit 3D-Grafiken funktionieren, können Sie mithilfe geometrischer Transformationen folgende Aktionen ausführen:

-   Gibt den Speicherort eines Objekts relativ zu einem anderen Objekt an.
-   Drehen und Größe von Objekten.
-   Ändern der Anzeige von Positionen, Richtungen und Perspektiven.

Sie können beliebige Punkte (x, y, z) mit einer 4 x 4-Matrix in einen anderen Punkt (x ", y", z ") umwandeln, wie in der folgenden Gleichung gezeigt.

![Gleichung der Umwandlung eines Punkts in einen anderen Punkt](images/matmult.png)

Führen Sie die folgenden Gleichungen für (x, y, z) und die Matrix aus, um den Punkt (x ', y ', z ') zu schaffen.

![Gleichungen für den neuen Punkt](images/matexpnd.png)

Die gängigsten Transformationen sind Übersetzung, Drehung und Skalierung. Sie können die Matrizen, die diese Effekte erzeugen, in einer einzelnen Matrix kombinieren, um mehrere Transformationen gleichzeitig zu berechnen. Beispielsweise können Sie eine einzelne Matrix erstellen, um eine Reihe von Punkten zu übersetzen und zu drehen.

Matrizen werden in der Reihenfolge der Zeilen Spalten geschrieben. Eine Matrix, die Scheitel Punkte entlang jeder Achse gleichmäßig skaliert, die als einheitliche Skalierung bezeichnet wird, wird durch die folgende Matrix mithilfe der mathematischen Notation dargestellt.

![Gleichung einer Matrix für die einheitliche Skalierung](images/matrix.png)

In C++ deklariert Direct3D Matrizen als zweidimensionales Array mit einer Matrixstruktur. Im folgenden Beispiel wird gezeigt, wie eine [**D3DMATRIX**](/windows/desktop/direct3d9/d3dmatrix) -Struktur initialisiert wird, die als einheitliche Skalierungs Matrix (Skalierungsfaktor ' s ') fungiert.

```cpp
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>Übersetzen


Mit der folgenden Gleichung wird der Punkt (x, y, z) in einen neuen Punkt (x ", y", z ") übersetzt.

![Gleichung einer Übersetzungs Matrix für einen neuen Punkt](images/transl8.png)

Sie können eine Übersetzungs Matrix in C++ manuell erstellen. Das folgende Beispiel zeigt den Quellcode für eine Funktion, die eine Matrix erstellt, um Vertices zu übersetzen.

```cpp
D3DXMATRIX Translate(const float dx, const float dy, const float dz) {
    D3DXMATRIX ret;

    D3DXMatrixIdentity(&ret);
    ret(3, 0) = dx;
    ret(3, 1) = dy;
    ret(3, 2) = dz;
    return ret;
}    // End of Translate
```

## <a name="span-idscalespanspan-idscalespanspan-idscalespanscale"></a><span id="Scale"></span><span id="scale"></span><span id="SCALE"></span>Migen


Die folgende Gleichung skaliert den Punkt (x, y, z) um beliebige Werte in der x-, y-und z-Richtung zu einem neuen Punkt (x ', y ', z ').

![Gleichung einer Skalierungs Matrix für einen neuen Punkt](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>Controller


Die hier beschriebenen Transformationen gelten für Links gesteuerte Koordinatensysteme und können sich daher von Transformations Matrizen unterscheiden, die Sie an anderer Stelle gesehen haben.

Die folgende Gleichung dreht den Punkt (x, y, z) um die x-Achse und erzeugt einen neuen Punkt (x ', y ', z ').

![Gleichung einer x-Rotations Matrix für einen neuen Punkt](images/matxrot.png)

Die folgende Gleichung dreht den Punkt um die y-Achse.

![Gleichung einer y-Rotations Matrix für einen neuen Punkt](images/matyrot.png)

Die folgende Gleichung dreht den Punkt um die z-Achse.

![Gleichung einer z-Rotations Matrix für einen neuen Punkt](images/matzrot.png)

In diesen Beispiel Matrizen steht der griechische Buchstabe der TA für den Drehwinkel im Bogenmaße. Winkel werden im Uhrzeigersinn gemessen, wenn Sie entlang der Drehungs Achse zum Ursprung suchen.

Der folgende Code zeigt eine Funktion, die die Drehung über die X-Achse behandelt.

```cpp
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

## <a name="span-idconcatenating_matricesspanspan-idconcatenating_matricesspanspan-idconcatenating_matricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>Verketten von Matrizen


Ein Vorteil der Verwendung von Matrizen besteht darin, dass Sie die Auswirkungen von zwei oder mehr Matrizen kombinieren können, indem Sie diese multiplizieren. Dies bedeutet, dass Sie keine zwei Matrizen anwenden müssen, um ein Modell zu drehen und es dann an einen Speicherort zu übersetzen. Stattdessen multiplizieren Sie die Drehung und die Übersetzungs Matrizen, um eine zusammengesetzte Matrix zu erzeugen, die all ihre Auswirkungen enthält. Dieser Prozess, der als Matrix Verkettung bezeichnet wird, kann mit der folgenden Gleichung geschrieben werden.

![Gleichung der Matrix Verkettung](images/matrxcat.png)

In dieser Gleichung ist C die zusammengesetzte Matrix, die erstellt wird, und M ₁ bis MN sind die einzelnen Matrizen. In den meisten Fällen werden nur zwei oder drei Matrizen verkettet, aber es gibt keine Begrenzung.

Die Reihenfolge, in der die Matrix Multiplikation durchgeführt wird, ist entscheidend. Die vorangehende Formel reflektiert die Regel von links nach rechts der Matrix Verkettung. Das heißt, die sichtbaren Auswirkungen der Matrizen, die Sie zum Erstellen einer zusammengesetzten Matrix verwenden, treten in der Reihenfolge von links nach rechts auf. Im folgenden Beispiel wird eine typische Weltmatrix gezeigt. Stellen Sie sich vor, dass Sie die Weltmatrix für einen Stereo typischen Flugtyp erstellen. Vielleicht möchten Sie den fliegenden Schlauch um seine Mitte drehen, die y-Achse des Modell Raums, und ihn an eine andere Stelle in Ihrer Szene übersetzen. Um diesen Effekt zu erreichen, erstellen Sie zunächst eine Rotations Matrix und multiplizieren Sie dann mit einer Übersetzungs Matrix, wie in der folgenden Gleichung gezeigt.

![Gleichung der Drehung basierend auf einer Rotations Matrix und einer Übersetzungs Matrix](images/wrldexpl.png)

In dieser Formel ist R<sub>y</sub> eine Matrix für die Drehung der y-Achse, und T<sub>w</sub> ist eine Übersetzung an eine Position in globalen Koordinaten.

Die Reihenfolge, in der Sie die Matrizen multiplizieren, ist wichtig, da im Gegensatz zu multiplizieren zweier Skalarwerte die Matrix Multiplikation nicht commutativ ist. Das Multiplizieren der Matrizen in umgekehrter Reihenfolge hat den visuellen Effekt, dass der fliegende Unterbereich in seine Welt Raum Position übersetzt und dann um den Ursprung der Welt gedreht wird.

Unabhängig von der Art der Matrix, die Sie erstellen, merken Sie sich die Regel von links nach rechts, um sicherzustellen, dass Sie die erwarteten Auswirkungen erzielen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Transformationen](transforms.md)

 

 