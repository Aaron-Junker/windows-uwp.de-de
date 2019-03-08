---
title: Raumtransformationen
description: Eine Welttransformation ändert die Koordinaten vom Modellbereich, in dem Scheitelpunkte relativ zum lokalen Ursprung definiert sind, zum Weltbereich.
ms.assetid: 767B032C-69D0-4583-8FEB-247F4C41E31D
keywords:
- Raumtransformationen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: af694ed32d845e518f4189f75309f1f371743f90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662415"
---
# <a name="world-transform"></a>Raumtransformationen


Eine Welttransformation ändert die Koordinaten vom Modellbereich, in dem Scheitelpunkte relativ zum lokalen Ursprung definiert sind, zum Weltbereich. Im Weltbereich werden Scheitelpunkte relativ zu einem Ursprung definiert, der allen Objekten in einer Szene gemeinsam ist. Die Welttransformation platziert ein Modell in der Welt.

## <span id="What_Is_a_World_Transform"></span><span id="what_is_a_world_transform"></span><span id="WHAT_IS_A_WORLD_TRANSFORM"></span>


Das folgende Diagramm zeigt die Beziehung zwischen dem Weltkoordinatensystem und einem lokalen Koordinatensystem des Modells.

![Diagramm zur Beziehung zwischen Weltkoordinaten und lokalen Koordinaten](images/worldcrd.png)

Welttransformationen können eine beliebige Kombination von Übersetzungen, Drehungen und Skalierungen enthalten.

## <a name="span-idsettingupaworldmatrixxmlspansetting-up-a-world-matrix"></a><span id="SETTING_UP_A_WORLD_MATRIX.XML"></span>Eine Weltmatrix einrichten


Sie erstellen, wie bei allen anderen Transformationen auch, die Welt, indem Sie eine Reihe von Matrizen zu einer einzelnen Matrix konkatenieren, die die Summe der Effekte der einzelnen Matrizen enthält. Im einfachste Fall befindet sich ein Modell am Ursprung der Welt, und seine lokalen Koordinatenachsen sind genauso ausgerichtet wie der Raum der Welt. In diesem Fall ist die Weltmatrix die Identitätsmatrix. In der Regel ist jedoch die Weltmatrix eine Kombination aus einer Übersetzung in den Raum der Welt und möglicherweise einer oder ggf. mehreren Drehungen des Modell.

Direct3D verwendet die Welt- und Ansichtsmatrizen, die Sie einrichten, um mehrere interne Datenstrukturen zu konfigurieren. Jedes Mal, wenn Sie eine neue Welt- oder Ansichtsmatrix festlegen, berechnet das System die zugehörigen internen Strukturen. Diese Matrizen festzulegen – z. B. mehrere Tausend Male pro Frame ist rechenintensiv und zeitaufwändig. Sie können die Anzahl der erforderlichen Berechnungen minimieren, indem Sie die Welt- und die Ansichtsmatrizen zu einer Welt-Ansichtsmatrix verketten, die Sie als die Weltmatrix festlegen, und dann für die Ansichtsmatrix die Identitätsmatrix verwenden. Halten Sie zwischengespeicherte Kopien der einzelnen Welt- und Ansichtsmatrizen an, damit Sie die Weltmatrix bei Bedarf ändern, verketten und zurücksetzen können.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Transformationen](transforms.md)

 

 




