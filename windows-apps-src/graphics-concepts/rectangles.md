---
title: Rechtecke
description: In Direct3D und in der Windows-Programmierung werden Objekte auf dem Bildschirm als umgebende Rechtecke bezeichnet.
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- Rechtecke
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 22aa6da9a26e3bd50fc5ff4fe4272f6da91cdd08
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320995"
---
# <a name="rectangles"></a>Rechtecke

In Direct3D und in der Windows-Programmierung werden Objekte auf dem Bildschirm als umgebende Rechtecke bezeichnet. Die Seiten des umgebenden Rechtecks sind immer parallel zu den Seiten des Bildschirms, damit das Rechteck durch zwei Punkte, der oberen linken Ecke und der unteren rechten Ecke, definiert werden kann.

## <a name="span-idboundingrectanglesspanspan-idboundingrectanglesspanspan-idboundingrectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>Umschließenden Rechtecken


Die meisten Anwendungen verwenden die [**RECT**](https://docs.microsoft.com/previous-versions/dd162897(v=vs.85))-Struktur (oder ein Typedef-Alias) als Träger von Informationen über ein umgebendes Rechteck, die beim Blitten auf den Bildschirm oder bei der Trefferermittlung verwendet werden. In C++ die **RECT** Struktur hat folgende Definition.

```cpp
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

In diesem Beispiel bilden die linken und oberen Elemente die x- und y-Koordinaten für die linke obere Ecke des umgebenden Rechtecks. Entsprechend bilden die rechten und unteren Elemente die Koordinaten der unteren rechten Ecke. Die folgende Abbildung zeigt, wie Sie diese Werte darstellen können.

![Abbildung des durch die linken, oberen, rechten und unteren Werte begrenzten umgebenden Rechtecks](images/rect.png)

Die Pixelspalte am rechten Rand sowie die Pixelzeile am unteren Rand sind nicht im RECT enthalten. Das Sperren eines RECT mit den Elementen {10, 10, 138, 138} ergibt ein Objekt, das 128 Pixel breit und hoch ist.

Für erhöhte Effizienz, Konsistenz und Bedienerfreundlichkeit arbeiten alle Direct3D-Darstellungsfunktionen mit Rechtecken.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Koordinatensysteme und Geometrie](coordinate-systems-and-geometry.md)

 

 




