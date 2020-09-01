---
title: Rechtecke
description: Während der Direct3D-und Windows-Programmierung werden Objekte auf dem Bildschirm in Bezug auf Begrenzungs Rechtecke bezeichnet.
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- Rechtecke
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a30aa1a2901f109a4f13316024785981023975b8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156254"
---
# <a name="rectangles"></a>Rechtecke

Während der Direct3D-und Windows-Programmierung werden Objekte auf dem Bildschirm in Bezug auf Begrenzungs Rechtecke bezeichnet. Die Seiten eines umgebenden Rechtecks sind immer parallel zu den Seiten des Bildschirms. Daher kann das Rechteck durch zwei Punkte, die obere linke Ecke und die untere rechte Ecke beschrieben werden.

## <a name="span-idbounding_rectanglesspanspan-idbounding_rectanglesspanspan-idbounding_rectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>Begrenzungs Rechtecke


Die meisten Anwendungen verwenden die [**Rect**](/previous-versions/dd162897(v=vs.85)) -Struktur (oder einen typedef-Alias), um Informationen zu einem umgebenden Rechteck zu übertragen, das beim blisten auf den Bildschirm oder beim Ausführen der Treffer Erkennung verwendet werden soll. In C++ hat die **Rect** -Struktur die folgende Definition.

```cpp
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

Im vorherigen Beispiel sind die linken und oberen Elemente die x-und y-Koordinaten der linken oberen Ecke eines umgebenden Rechtecks. Entsprechend bilden die Rechte und unteren Member die Koordinaten der unteren rechten Ecke. In der folgenden Abbildung wird gezeigt, wie diese Werte visualisiert werden können.

![Abbildung des Rechtecks, das durch die Werte Links, oben, rechts und unten begrenzt ist](images/rect.png)

Die Spalte mit Pixeln am rechten Rand und die Zeile der Pixel am unteren Rand sind nicht in der Rect enthalten. Wenn Sie z. b. ein Rect mit den Membern {10, 10, 138, 138} sperren, wird ein Objekt mit einer Breite und Höhe von 128 Pixel ausgegeben.

Aus Gründen der Effizienz, Konsistenz und Benutzerfreundlichkeit können alle Direct3D Presentation-Funktionen mit Rechtecke verwendet werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Koordinatensysteme und Geometrie](coordinate-systems-and-geometry.md)

 

 