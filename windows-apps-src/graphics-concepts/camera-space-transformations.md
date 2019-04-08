---
title: Kameraraumtransformationen
description: Eckpunkte im Kameraraum werden durch das Wandeln der Objekteckpunkte mit der Weltansichtsmatrix berechnet.
ms.assetid: 86EDEB95-8348-4FAA-897F-25251B32B076
keywords:
- Kameraraumtransformationen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b35fb71e51044ee6be6ed90001e3b5614c8cb45
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655735"
---
# <a name="camera-space-transformations"></a>Kameraraumtransformationen


Eckpunkte im Kameraraum werden durch das Wandeln der Objekteckpunkte mit der Weltansichtsmatrix berechnet.

V = V \* WvMatrix

Vertexspezifische Normalen im Kameraraum werden berechnet, indem die Normalen des Objekts mit der inversen Umsetzung der globalen Ansichtsmatrix transformiert werden. Eine globale Ansichtsmatrix kann entweder orthogonal sein oder nicht.

N = N \* (wvMatrix⁻¹)<sup>T</sup>

Die Matrixinversion und der Matrixaustausch werden auf einer 4 x 4-Matrix ausgeführt. Die Multiplikation kombiniert den normalen Teil mit dem 3 x 3-Teil der resultierenden 4 x 4-Matrix.

Wenn der Renderstatus so festgelegt ist, dass die Normalen normalisiert werden, werden Scheitelnormalenvektoren wie folgt nach der Transformation auf den Kameraraum normalisiert:

N = norm(N)

Die Beleuchtungsposition im Kameraraum wird durch die Transformation der Position der Lichtquelle mit der Ansichtsmatrix berechnet.

Lₚ = Lₚ \* vMatrix

Die Richtung des Lichts im Kameraraum für gerichtetes Licht wird durch die Multiplikation der Richtung der Lichtquellen mit der Ansichtsmatrix berechnet, normalisiert und das Resultat wird negiert.

L<sub>Dir</sub> = - Norm (L<sub>Dir</sub> \* WvMatrix)

Für ein punktuelles Licht und ein Spotlight wird die Richtung der Lichtquelle wie folgt berechnet:

L<sub>Dir</sub> = Norm (V \* Lₚ), wobei die Parameter in der folgenden Tabelle definiert werden.

| Parameter       | Standardwert | Typ                                          | Beschreibung                                               |
|-----------------|---------------|-----------------------------------------------|-----------------------------------------------------------|
| L<sub>dir</sub> | n. a.           | 3D-Vektor (X-, Y- und Z-Gleitkommawerte) | Richtungsvektor vom Objekt-Vertex bis zur Lichtquelle          |
| V               | n. a.           | 3D-Vektor (X-, Y- und Z-Gleitkommawerte) | Vertexposition im Kameraraum                           |
| wvMatrix        | Identität      | 4 x 4-Matrix der Gleitkommawerte           | Zusammengesetzte Matrix mit globaler und Ansichtstransformation |
| N               | n. a.           | 3D-Vektor (X-, Y- und Z-Gleitkommawerte) | Vertexnormale                                             |
| Lₚ              | n. a.           | 3D-Vektor (X-, Y- und Z-Gleitkommawerte) | Position der Lichtquelle im Kameraraum                            |
| vMatrix         | Identität      | 4 x 4-Matrix der Gleitkommawerte           | Matrix mit Ansichtstransformation                      |

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Mathematik der Beleuchtung](mathematics-of-lighting.md)

 

 




