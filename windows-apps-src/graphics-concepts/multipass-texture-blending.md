---
title: Mehrstufige Texturvermischung
description: Direct3D-Apps können durch die Anwendung verschiedener Texturen auf eine Primitive im Laufe von mehreren Berechnungs- und Ausgabedurchläufen zahlreiche Spezialeffekte erzielen.
ms.assetid: FB4D6E3F-4EF5-4D20-BF7E-1008E790E30C
keywords:
- Mehrstufige Texturvermischung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d6b1e8958874ede50a18f2d2446c8f156361210e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589895"
---
# <a name="multipass-texture-blending"></a>Mehrstufige Texturvermischung


Direct3D-Apps können durch die Anwendung verschiedener Texturen auf eine Primitive im Laufe von mehreren Berechnungs- und Ausgabedurchläufen zahlreiche Spezialeffekte erzielen. Der allgemeine Begriff dafür ist *Mehrstufige Texturmischung*. Eine typische Anwendung für die mehrstufige Texturvermischung liegt in der Nachbildung der Effekte von komplexen Beleuchtungs- und Verschattungsmodellen durch die Anwendung mehrerer Farben aus mehreren unterschiedlichen Texturen. Eine solche Anwendung heißt *Lichtzuordnung*. Siehe [Lichtzuordnung mit Texturen](light-mapping-with-textures.md).

**Beachten Sie**    einige Geräte sind mehrere Strukturen, primitive Typen in einem einzelnen Durchlauf anwenden kann. Siehe [Texturvermischung](texture-blending.md).

 

Wenn die Hardware des Benutzers die Vermischung mehrerer Texturen nicht unterstützt, kann Ihre Anwendung die mehrstufige Texturvermischung verwenden, um die gleichen visuellen Effekte zu erzielen. Die Anwendung kann jedoch nicht die Bildwechselfrequenzen erhalten, die bei Verwendung der Vermischung mehrerer Texturen möglich sind.

Durchführen der mehrstufigen Texturvermischung in einer C/C++-Anwendung:

1.  Setzen Sie eine Textur in Texturphase 0.
2.  Wählen Sie die gewünschte Farb- und Alphavermischungsargumente und -abläufe. Die Standardeinstellungen eignen sich hinlänglich für die mehrstufige Texturvermischung.
3.  Berechnen Sie die entsprechenden 3D-Objekte in der Szene und geben Sie diese aus.
4.  Setzen Sie die nächste Textur in Texturphase 0.
5.  Setzen Sie die Berechnungs- und Ausgabezustände, um die Ursprungs- und Zielvermischungsfaktoren nach Bedarf anzupassen. Das System vermischt die neuen Texturen mit den vorhandenen Pixeln in der Ziel-Ausgeben-Oberfläche gemäß dieser Parameter.
6.  Wiederholen Sie die Schritte 3, 4 und 5 mit so vielen Texturen wie gewünscht.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Textur blending](texture-blending.md)

 

 




