---
title: Ansichtstransformation
description: Eine Ansichtstransformation lokalisiert den Betrachter im Weltbereich und transformiert dazu Scheitelpunkte in den Kamerabereich.
ms.assetid: DA4C2051-4C28-4ABF-8C06-241C8CB87F2F
keywords:
- Ansichtstransformation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63660883f327547a82eac4a3accec475995a651a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593585"
---
# <a name="view-transform"></a>Ansichtstransformation


Eine *Ansichtstransformation* lokalisiert den Betrachter im Weltbereich und transformiert dazu Scheitelpunkte in den Kamerabereich. Im Kamerabereich befindet sich die Kamera, bzw. der Betrachter, am Ursprungspunkt und blickt in die positive z-Richtung. Die Ansichtsmatrix verschiebt die Objekte in der Welt um die Position einer Kamera - den Ursprung des Kamerabereichs - und ihre Ausrichtung. Direct3D verwendet ein linkshändiges Koordinatensystem, z ist daher in einer Szene positiv.

Es gibt viele Möglichkeiten, um eine Ansichtsmatrix erstellen. In allen Fällen hat die Kamera eine logische Position und Ausrichtung im Weltbereich, die als Ausgangspunkt verwendet wird, um eine Ansichtsmatrix zu erstellen, die mit den Modellen in einer Szene angewendet wird. Die Ansichtsmatrix übersetzt und dreht Objekte, um sie im Kamerabereich zu platzieren, in dem sich die Kamera am Ursprung befindet. Eine Möglichkeit zur Erstellung einer Ansichtsmatrix ist die Kombination einer Übersetzungsmatrix mit Drehungsmatrizen für jede Achse. Bei diesem Ansatz gilt die folgende allgemeine Matrixgleichung für allgemeine.

![Gleichung der Ansichtstransformation](images/viewtran.png)

In dieser Formel ist V die zu erstellende Ansichtsmatrix, T ist eine Übersetzungsmatrix, die Objekte in der Welt neu positioniert und Rₓ bis R<sub>z</sub> sind Drehungsmatrizen, die Objekte um die x-, y- und z-Achse drehen. Die Übersetzungs- und Drehungsmatrizen basieren auf der logischen Position und Orientierung der Kamera im Weltbereich. Wenn die logische Position der Kamera in der Welt &lt;10,20,100&gt; ist, ist das Ziel der Übersetzungsmatrix die Verschiebung von Objekten um -10 Einheiten entlang der x-Achse, um -20 Einheiten entlang der y-Achse sowie um -100 Einheiten entlang der z-Achse. Die Drehungsmatrizen in der Formel basieren auf der Ausrichtung der Kamera, d. h. darauf, wie weit die Achsen des Kamerabereichs durch Drehung von der Übereinstimmung mit dem Weltbereich entfernt sind. Zum Beispiel: Wenn die erwähnte Kamera direkt nach unten zeigt, befindet sich ihre z-Achse 90 Grad (pi/2 Bogenmaß) außerhalb der Übereinstimmung mit der z-Achse des Weltbereichs, wie in der folgenden Abbildung illustriert.

![Illustration des Ansichtsbereichs der Kamera im Vergleich zum Weltbereich](images/camtop.png)

Die Drehungsmatrizen wenden Drehungen gleicher (jedoch entgegengesetzter) Größe auf die Modelle in der Szene an. Die Ansichtsmatrix für diese Kamera enthält eine Drehung um -90 Grad um die x-Achse. Die Drehungsmatrix wird mit der Übersetzungsmatrix kombiniert, um eine Ansichtsmatrix zu schaffen, die die Position und die Ausrichtung der Objekte in der Szene so anpasst, dass ihr oberer Rand zur Kamera weist; dadurch entsteht der Eindruck, dass sich die Kamera über dem Modell befindet.

## <a name="span-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspansetting-up-a-view-matrix"></a><span id="Setting_Up_a_View_Matrix"></span><span id="setting_up_a_view_matrix"></span><span id="SETTING_UP_A_VIEW_MATRIX"></span>Das Einrichten einer Matrix anzeigen


Direct3D verwendet die Welt- und Ansichtsmatrizen, um mehrere interne Datenstrukturen zu konfigurieren. Jedes Mal, wenn Sie eine neue Welt- oder Ansichtsmatrix festlegen, berechnet das System die zugehörigen internen Strukturen. Die Einrichtung dieser Matrizen nimmt oft viel Rechenzeit in Anspruch. Sie können die Anzahl der erforderlichen Berechnungen minimieren, indem Sie die Welt- und die Ansichtsmatrizen zu einer Welt-Ansichtsmatrix verketten, die Sie als die Weltmatrix festlegen, und dann für die Ansichtsmatrix die Identitätsmatrix verwenden. Bewahren Sie zwischengespeicherte Kopien der einzelnen Welt- und Ansichtsmatrizen auf, damit Sie die Weltmatrix bei Bedarf ändern, verketten und zurücksetzen können.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Transformationen](transforms.md)

 

 




