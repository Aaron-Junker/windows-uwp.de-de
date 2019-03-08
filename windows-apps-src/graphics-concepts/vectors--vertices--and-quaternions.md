---
title: Vektoren, Scheitelpunkte und Quaternionen
description: In Direct3D beschreiben Scheitelpunkte die Position und Ausrichtung. Jeder Scheitelpunkt in einen Grundtyp wird durch einen Vektor beschrieben, der die Positions-, Farb-, Texturkoordinaten angibt sowie durch einen normalen Vektor, der die Ausrichtung angibt.
ms.assetid: 94EC3D59-43FC-4509-A233-916E9FA8381E
keywords:
- Vektoren, Scheitelpunkte und Quaternionen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8942e53b7372e2e8b3cf4ed05f89b4187bdfc4be
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599865"
---
# <a name="vectors-vertices-and-quaternions"></a>Vektoren, Scheitelpunkte und Quaternionen


In Direct3D beschreiben Scheitelpunkte die Position und Ausrichtung. Jeder Scheitelpunkt in einen Grundtyp wird durch einen Vektor beschrieben, der die Positions-, Farb-, Texturkoordinaten angibt sowie durch einen normalen Vektor, der die Ausrichtung angibt.

Quaternionen hinzufügen, eine vierte Element mit der \[X, y, Z\] Werte, die einen drei-Komponente-Vektor zu definieren. Quaternionen sind eine Alternative zu Matrix-Methoden, die in der Regel für 3D -Drehungen verwendet werden. Ein Quaternion repräsentiert eine Achse im 3D-Raum sowie eine Drehung um diese Achse. Beispielsweise kann ein Quaternion eine (1,1,2)-Achse und eine Drehung um ein Bogenmaß repräsentieren. Quaternionen enthalten wertvolle Informationen, ihr wirklicher Nutzen liegt jedoch in zwei Operationen, die Sie mit ihnen durchführen können: Komposition und Interpolation.

Die Durchführung einer Komposition mit Quaternionen ist vergleichbar mit ihrer Kombinierung. Die Komposition von zwei Quaternionen wird wie in der folgenden Abbildung gezeigt notiert.

![Illustration der Quaternionendrehung](images/quateq.png)

Die Komposition von zwei Quaternionen für eine Geometrie bedeutet die „Drehung der Geometrie um Ache₂ mit Drehung₂ und anschließend um Achse₁ mit Drehung₁". In diesem Fall ist Q eine Drehung um eine einzelne Achse als Ergebnis der Anwendung von q₂ und dann von q₁ auf die Geometrie.

Mit der Quaternioneninterpolation kann eine Anwendung einen reibungslosen und angemessenen Übergangspfad von einer Achse und Ausrichtung zu einer anderen berechnen. Daher bietet eine Interpolation zwischen q₁ und q₂ eine einfache Möglichkeit für Animationen von einer Ausrichtung zu einer anderen.

Wenn Sie Komposition und Interpolation zusammen verwenden, erhalten Sie damit eine einfache Möglichkeit zur Manipulation einer komplex erscheinenden Geometrie. Stellen Sie sich beispielsweise vor, dass Sie eine Geometrie haben, die Sie in einer bestimmten Ausrichtung drehen möchten. Sie wissen, dass Sie sie um r₂ Grad um Achse₂ drehen und anschließend um r₁ Grad um Ache₁ drehen möchten, Sie kennen aber das endgültige Quaternion nicht. Durch Verwendung der Komposition können Sie die zwei Drehungen auf der Geometrie kombinieren, um ein einziges Quaternion als Ergebnis zu erhalten. Anschließend können Sie die Interpolation vom ursprünglichen zum komponierten Quaternion durchführen und so einen glatten Übergang zwischen beiden erreichen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Koordinatensysteme und Geometrie](coordinate-systems-and-geometry.md)

 

 




