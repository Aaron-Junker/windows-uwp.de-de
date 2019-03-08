---
title: Texturen mit Alphakanälen
description: Es gibt zwei Möglichkeiten, Texturabbildungen mit komplexer Transparenz zu codieren.
ms.assetid: 768A774A-4F21-4DDE-B863-14211DA92926
keywords:
- Texturen mit Alphakanälen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88d150383d2be219e7f382e0e690771acbc9d2ee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651475"
---
# <a name="textures-with-alpha-channels"></a>Texturen mit Alphakanälen


Es gibt zwei Möglichkeiten, Texturabbildungen mit komplexer Transparenz zu codieren. In jedem Fall geht ein Block, der die Transparenz beschreibt, dem bereits beschriebenen 64-Bit-Block voraus. Die Transparenz wird entweder als 4 x 4-Bitmap mit 4 Bits pro Pixel (explizite Codierung) oder mit weniger Bits und linearer Interpolation dargestellt, die mit der verwendeten Codierung von Farben vergleichbar ist.

Die Transparenzblock und der Farbblock sind wie in der folgenden Tabelle gezeigt angeordnet.

| Word-Adresse | 64-Bit-Block                      |
|--------------|-----------------------------------|
| 3:0          | Transparenzblock                |
| 7:4          | Zuvor beschriebener 64-Bit-Block |

 

## <a name="span-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanexplicit-texture-encoding"></a><span id="Explicit-Texture-Encoding"></span><span id="explicit-texture-encoding"></span><span id="EXPLICIT-TEXTURE-ENCODING"></span>Explizite Textur-Codierung


Für die explizite Texturkodierung (Format BC2) werden die Alphakomponenten der Texel, die die Transparenz beschreiben, in einer 4 x 4-Bitmap mit 4 Bits pro Texel kodiert. Diese vier Bits kann können auf verschiedene Weise gewonnen werden, etwa durch Dithering oder durch Verwendung der vier wichtigsten Bits der Alphadaten. Wie auch immer sie produziert werden, so sind sie, ohne irgendeine Art von Interpolation.

Das folgende Diagramm zeigt einen 64-Bit-Transparenzblock.

![Diagramm eines 64-Bit-Transparenzblocks](images/colors4.png)

**Beachten Sie**    Direct3D-Komprimierungsmethode verwendet die vier höchstwertigen Bits.

 

Die folgenden Tabellen illustrieren, wie die Alpha-Informationen für jedes 16-Bit-Wort im Speicher repräsentiert sind.

Layout für Wort 0:

| Bits          | Alpha      |
|---------------|------------|
| 3:0 (LSB\*)   | \[0\]\[0\] |
| 7:4           | \[0\]\[1\] |
| 11:8          | \[0\]\[2\] |
| 15:12 (MSB\*) | \[0\]\[3\] |

 

\*unwichtigste Bit, höchstwertigen bit (MSB)

Layout für Wort 1:

| Bits        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[1\]\[0\] |
| 7:4         | \[1\]\[1\] |
| 11:8        | \[1\]\[2\] |
| 15:12 (MSB) | \[1\]\[3\] |

 

Layout für Wort 2:

| Bits        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[2\]\[0\] |
| 7:4         | \[2\]\[1\] |
| 11:8        | \[2\]\[2\] |
| 15:12 (MSB) | \[2\]\[3\] |

 

Layout für Wort 3:

| Bits        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[3\]\[0\] |
| 7:4         | \[3\]\[1\] |
| 11:8        | \[3\]\[2\] |
| 15:12 (MSB) | \[3\]\[3\] |

 

Der Farbvergleich, der in BC1 verwendet wird, um festzustellen, ob der Texel transparent ist, wird in diesem Format nicht verwendet. Es wird angenommen, dass die Farbdaten ohne den Farbvergleich immer wie im 4-Farben-Modus behandelt werden.

## <a name="span-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanthree-bit-linear-alpha-interpolation"></a><span id="Three-Bit-Linear-Alpha-Interpolation"></span><span id="three-bit-linear-alpha-interpolation"></span><span id="THREE-BIT-LINEAR-ALPHA-INTERPOLATION"></span>3-Bit-linearen alpha-interpolation


Die Kodierung der Transparenz für das Format BC3 basiert auf einem Konzept, das der für Farbe verwendeten linearen Kodierung ähnelt. Zwei 8-Bit-Alphawerte und eine 4 x 4-Bitmap mit drei Bits pro Pixel sind in den ersten acht Bytes des Blocks gespeichert. Die repräsentativen Alphawerte werden zum Interpolieren der Alpha-Zwischenwerte verwendet. Die Art und Weise der beiden Alphawerte bietet weitere Informationen. Wenn alpha\_0 ist größer als Alpha\_1, und dann sechs intermediate alpha-Werte werden durch die Interpolation erstellt. Andernfalls werden vier Alpha-Zwischenwerte zwischen den angegebenen Alpha-Extremen interpoliert. Die zwei zusätzlichen impliziten Alphawerte sind 0 (vollständig transparent) und 255 (vollständig opak).

Das folgende Codebeispiel veranschaulicht diesen Algorithmus.

```
// 8-alpha or 6-alpha block?    
if (alpha_0 > alpha_1) {    
    // 8-alpha block:  derive the other six alphas.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (6 * alpha_0 + 1 * alpha_1 + 3) / 7;    // bit code 010
    alpha_3 = (5 * alpha_0 + 2 * alpha_1 + 3) / 7;    // bit code 011
    alpha_4 = (4 * alpha_0 + 3 * alpha_1 + 3) / 7;    // bit code 100
    alpha_5 = (3 * alpha_0 + 4 * alpha_1 + 3) / 7;    // bit code 101
    alpha_6 = (2 * alpha_0 + 5 * alpha_1 + 3) / 7;    // bit code 110
    alpha_7 = (1 * alpha_0 + 6 * alpha_1 + 3) / 7;    // bit code 111  
}    
else {  
    // 6-alpha block.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (4 * alpha_0 + 1 * alpha_1 + 2) / 5;    // Bit code 010
    alpha_3 = (3 * alpha_0 + 2 * alpha_1 + 2) / 5;    // Bit code 011
    alpha_4 = (2 * alpha_0 + 3 * alpha_1 + 2) / 5;    // Bit code 100
    alpha_5 = (1 * alpha_0 + 4 * alpha_1 + 2) / 5;    // Bit code 101
    alpha_6 = 0;                                      // Bit code 110
    alpha_7 = 255;                                    // Bit code 111
}
```

Das Speicherlayout des Alphablocks ist wie folgt:

| Byte | Alpha                                                          |
|------|----------------------------------------------------------------|
| 0    | Alpha\_0                                                       |
| 1    | Alpha\_1                                                       |
| 2    | \[0\]\[2\] (2 MSBs), \[0\]\[1\], \[0\]\[0\]                    |
| 3    | \[1\]\[1\] (1 MSB), \[1\]\[0\], \[0\]\[3\], \[0\]\[2\] (1 LSB) |
| 4    | \[1\]\[3\], \[1\]\[2\], \[1\]\[1\] (2 LSBs)                    |
| 5    | \[2\]\[2\] (2 MSBs), \[2\]\[1\], \[2\]\[0\]                    |
| 6    | \[3\]\[1\] (1 MSB), \[3\]\[0\], \[2\]\[3\], \[2\]\[2\] (1 LSB) |
| 7    | \[3\]\[3\], \[3\]\[2\], \[3\]\[1\] (2 LSBs)                    |

 

Der Farbvergleich, der in BC1 verwendet wird, um festzustellen, ob der Texel transparent ist, wird in diesen Formaten nicht verwendet. Es wird angenommen, dass die Farbdaten ohne den Farbvergleich immer wie im 4-Farben-Modus behandelt werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Komprimierte des texturressourcen](compressed-texture-resources.md)

 

 




