---
title: BC7-Format
description: Das BC7-Format ist ein Format für die Texturkomprimierung, das für die hochwertige Komprimierung von RGB- und RGBA-Daten verwendet wird.
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- BC7-Format
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 25ee9960e55100eaf743d73d7007e9bcf51342ee
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370724"
---
# <a name="bc7-format"></a>BC7-Format


Das BC7-Format ist ein Format für die Texturkomprimierung, das für die hochwertige Komprimierung von RGB- und RGBA-Daten verwendet wird.

Informationen zu den Blockmodi des BC7-Formats finden Sie unter [Verweis auf den BC7-Format-Modus](https://docs.microsoft.com/windows/desktop/direct3d11/bc7-format-mode-reference).

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgiformatbc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>Informationen zu "bc7" nicht/DXGI\_FORMAT\_"bc7" nicht


"Bc7" nicht angegeben ist, indem Sie die folgenden DXGI\_FORMAT-Enumerationswerte fest:

-   **DXGI\_FORMAT\_BC7\_TYPELESS**.
-   **DXGI\_FORMAT\_BC7\_UNORM**.
-   **DXGI\_FORMAT\_BC7\_UNORM\_SRGB**.

Das BC7-Format kann für Texturressourcen wie [Texture2D](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (einschließlich Arrays), Texture3D oder TextureCube (einschließlich Arrays) verwendet werden. Das Format gilt ebenfalls für alle Mip-Map-Oberflächen, die mit diesen Ressourcen verbunden sind.

BC7 verwendet eine feste Blockgröße von 16 Byte (128 Bit) und eine feste Kachelgröße von 4 × 4-Texel. Genau wie mit vorherigen BC-Formaten werden Texturbilder, die größer als die unterstützte Kachelgröße (4 × 4) sind, durch die Verwendung mehrerer Blöcke komprimiert. Diese Adressierungsidentität gilt auch für dreidimensionale Bilder, MIP-Maps, Cube-Zuordnungen und Texturarrays. Alle Bildkacheln müssen das gleiche Format aufweisen.

BC7 komprimiert sowohl Drei-Kanal (RGB) und Vier-Kanal (RGBA) Datenimages mit Festpunkt. Normalerweise sind die Quelldaten 8 Bit pro Farbkomponente (Kanal), obwohl das Format Quelldaten mit höheren Bits pro Farbkomponenten codieren kann. Alle Bildkacheln müssen das gleiche Format aufweisen.

Der BC7-Decoder führt eine Dekomprimierung aus, bevor die Texturfilterung angewendet wird.

Die Hardware für die BC7-Dekomprimierung muss die Bit präzise wiedergeben. Das heißt, sie muss Ergebnisse zurückgeben, die mit dem in dieser Dokumentation beschriebenen Decoder übereinstimmen.

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>"Bc7" nicht-Implementierung


Eine BC7-Implementierung kann einen von 8 Modi bestimmen, inklusive den Modus des Bit mit dem unerheblichsten Wert des 16 Byte-Blocks (128-Bit). Der Modus wird mit Null oder mehr Bit mit einem Wert von 0 codiert, gefolgt von einer 1.

Ein BC7-Block kann mehrere Endpunktpaare enthalten. Der Satz von Indizes, die einem Endpunktpaar entsprechen, kann als „Teilmenge” bezeichnet werden. In einigen Blockmodi wird die Darstellung der Endpunkte auch in einem Format codiert, das als „RBGP” bezeichnet wird, wobei das Bit „P” das gemeinsame Bit mit dem unerheblichsten Wert für die Farbkomponente des Endpunkts darstellt. Wenn beispielsweise die Darstellung des Endpunkts für das Format „RGB 5.5.5.1” ist, wird der Endpunkt als ein RGB-Wert von 6.6.6 interpretiert, wobei der Zustand des P-Bits das Bit mit dem unerheblichsten Wert jeder Komponente darstellt. Wenn die Darstellung für das Format für Quelldaten mit einem Alpha-Kanal „RGBAP 5.5.5.5.1” ist, wird der Endpunkt als RGBA 6.6.6.6 interpretiert. Je nach Blockmodus können Sie das gemeinsame Bit mit dem unerheblichsten Wert entweder für beide Endpunkten einer Teilmenge einzeln (2 P-Bit pro Teilmenge) oder zwischen Endpunkten einer Teilmenge gemeinsam (1 P-Bit pro Teilmenge) angeben.

Ein BC7-Block, der die Alpha-Komponente nicht explizit codiert, besteht aus Modus-Bit, Partition-Bit, komprimierten Endpunkten, komprimierten Indizes und einem optionalen P-Bit. Die Endpunkte in den Blöcken haben eine reine RGB-Darstellung und die Alpha-Komponente wird als 1,0 für alle Texel in den Quelldaten decodiert.

Ein BC7-Block, der gemeinsame Farb- und Alphawertkomponenten hat, besteht aus Modus-Bit, komprimierten Endpunkten, komprimierten Indizes und optionalen Partition-Bit und einem P-Bit. In diese Blöcken werden die Endpunktfarben im RGBA-Format dargestellt und die Alpha-Komponentenwerte werden zusammen mit den Werten der Farben interpoliert.

Ein BC7-Block, der separate Farb- und Alpha-Komponenten hat, besteht aus Modus-Bit, Rotations-Bit, komprimierten Endpunkten, komprimierten Indizes und einem optionalen Index-Selektor-Bit. Diese Blöcke haben einen effektiven RGB-Vektor \[R, G, B\] und eine skalare alpha-Kanal \[ein\] separat codierte.

Die folgende Tabelle enthält die Komponenten der einzelnen Blocktypen.

| BC7-Block enthält...     | Modus-Bit | Rotations-Bit | Index-Selektor- | Partition-Bit | Komprimierte Endpunkte | P-Bit    | Komprimierte Indizes |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| Nur Farbkomponenten     | Erforderlich  | Nicht zutreffend           | Nicht zutreffend                | Erforderlich       | Erforderlich             | Optional | Erforderlich           |
| Farb- + Alphawert kombiniert    | Erforderlich  | Nicht zutreffend           | Nicht zutreffend                | Optional       | Erforderlich             | Optional | Erforderlich           |
| Separater Farb- und Alphawert | Erforderlich  | Erforderlich      | Optional           | Nicht zutreffend            | Erforderlich             | Nicht zutreffend      | Erforderlich           |

 

BC7 definiert eine Farbpalette auf einer ungefähren Linie zwischen zwei Endpunkten. Der Moduswert bestimmt die Anzahl der interpolierten Endpunktpaare pro Block. BC7 speichert einen Farbpaletten-Index pro Texel.

Für jede Teilmenge an Indizes, die einem Endpunktpaar entsprechen, legt der Encoder den Zustand eines Bit der komprimierten Indexdaten für diese Teilmenge fest. Dazu wird eine Reihenfolge der Endpunkte ausgewählt, mit der der Index für den angegebenen "korrigierten" Index das wichtigste Bit auf 0 festlegen kann. Dieses wird anschließend verworfen und es wird ein Bit pro Teilmenge gespeichert. Für Block-Modi mit nur einer einzigen Teilmenge ist der korrigierte Index immer der Index 0.

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>Das Format "bc7" nicht-Decodierung


Der folgende Pseudocode zeigt die Schritte zur Dekomprimierung der Pixel (x, y) bei einem 16-Byte BC7-Block an.

``` syntax
decompress_bc7(x, y, block)
{
    mode = extract_mode(block);
    
    //decode partition data from explicit partition bits
    subset_index = 0;
    num_subsets = 1;
    
    if (mode.type == 0 OR == 1 OR == 2 OR == 3 OR == 7)
    {
        num_subsets = get_num_subsets(mode.type);
        partition_set_id = extract_partition_set_id(mode, block);
        subset_index = get_partition_index(num_subsets, partition_set_id, x, y);
    }
    
    //extract raw, compressed endpoint bits
    UINT8 endpoint_array[num_subsets][4] = extract_endpoints(mode, block);
    
    //decode endpoint color and alpha for each subset
    fully_decode_endpoints(endpoint_array, mode, block);
    
    //endpoints are now complete.
    UINT8 endpoint_start[4] = endpoint_array[2 * subset_index];
    UINT8 endpoint_end[4]   = endpoint_array[2 * subset_index + 1];
        
    //Determine the palette index for this pixel
    alpha_index     = get_alpha_index(block, mode, x, y);
    alpha_bitcount  = get_alpha_bitcount(block, mode);
    color_index     = get_color_index(block, mode, x, y);
    color_bitcount  = get_color_bitcount(block, mode);

    //determine output
    UINT8 output[4];
    output.rgb = interpolate(endpoint_start.rgb, endpoint_end.rgb, color_index, color_bitcount);
    output.a   = interpolate(endpoint_start.a,   endpoint_end.a,   alpha_index, alpha_bitcount);
    
    if (mode.type == 4 OR == 5)
    {
        //Decode the 2 color rotation bits as follows:
        // 00 – Block format is Scalar(A) Vector(RGB) - no swapping
        // 01 – Block format is Scalar(R) Vector(AGB) - swap A and R
        // 10 – Block format is Scalar(G) Vector(RAB) - swap A and G
        // 11 - Block format is Scalar(B) Vector(RGA) - swap A and B
        rotation = extract_rot_bits(mode, block);
        output = swap_channels(output, rotation);
    }
    
}
```

Der folgende Pseudocode zeigt die Schritte an, um die Farbe eines Endpunkts und die Alpha-Komponenten für jede Teilmenge eines 16-Byte BC7-Blocks vollständig zu decodieren.

``` syntax
fully_decode_endpoints(endpoint_array, mode, block)
{
    //first handle modes that have P-bits
    if (mode.type == 0 OR == 1 OR == 3 OR == 6 OR == 7)
    {
        for each endpoint i
        {
            //component-wise left-shift
            endpoint_array[i].rgba = endpoint_array[i].rgba << 1;
        }
        
        //if P-bit is shared
        if (mode.type == 1) 
        {
            pbit_zero = extract_pbit_zero(mode, block);
            pbit_one = extract_pbit_one(mode, block);
            
            //rgb component-wise insert pbits
            endpoint_array[0].rgb |= pbit_zero;
            endpoint_array[1].rgb |= pbit_zero;
            endpoint_array[2].rgb |= pbit_one;
            endpoint_array[3].rgb |= pbit_one;  
        }
        else //unique P-bit per endpoint
        {  
            pbit_array = extract_pbit_array(mode, block);
            for each endpoint i
            {
                endpoint_array[i].rgba |= pbit_array[i];
            }
        }
    }

    for each endpoint i
    {
        // Color_component_precision & alpha_component_precision includes pbit
        // left shift endpoint components so that their MSB lies in bit 7
        endpoint_array[i].rgb = endpoint_array[i].rgb << (8 - color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a << (8 - alpha_component_precision(mode));

        // Replicate each component's MSB into the LSBs revealed by the left-shift operation above
        endpoint_array[i].rgb = endpoint_array[i].rgb | (endpoint_array[i].rgb >> color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a | (endpoint_array[i].a >> alpha_component_precision(mode));
    }
        
    //If this mode does not explicitly define the alpha component
    //set alpha equal to 1.0
    if (mode.type == 0 OR == 1 OR == 2 OR == 3)
    {
        for each endpoint i
        {
            endpoint_array[i].a = 255; //i.e. alpha = 1.0f
        }
    }
}
```

Um jede interpolierte Komponente für jede Teilmenge zu generieren, verwenden Sie folgenden Algorithmus: „c” ist die generierende Komponente, „e0” ist die Komponente von Endpunkt 0 der Teilmenge und „e1” ist die entsprechende Komponente von Endpunkt 1 dieser Teilmenge.

``` syntax
UINT16 aWeight2[] = {0, 21, 43, 64};
UINT16 aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
UINT16 aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

UINT8 interpolate(UINT8 e0, UINT8 e1, UINT8 index, UINT8 indexprecision)
{
    if(indexprecision == 2)
        return (UINT8) (((64 - aWeights2[index])*UINT16(e0) + aWeights2[index]*UINT16(e1) + 32) >> 6);
    else if(indexprecision == 3)
        return (UINT8) (((64 - aWeights3[index])*UINT16(e0) + aWeights3[index]*UINT16(e1) + 32) >> 6);
    else // indexprecision == 4
        return (UINT8) (((64 - aWeights4[index])*UINT16(e0) + aWeights4[index]*UINT16(e1) + 32) >> 6);
}
```

Der folgende Pseudocode veranschaulicht, wie Sie Indizes und die Bit-Anzahl an Farb- und Alphawertkomponenten extrahieren. Blöcke mit separaten Farb- und Alphawerten verfügen auch über zwei Sätze an Indexdaten: eine für den Vektor-Kanal und eine für den skalaren Kanal. Für Modus 4 sind diese Indizes unterschiedlich breit (2 oder 3 Bit) und ein 1-Bit-Selektor gibt an, ob die Vektor- oder skalaren Daten die 3-Bit-Indizes verwenden. (Das Extrahieren der Anzahl an Alpha-Bits ähnelt dem Extrahieren der Anzahl an Farb-Bits mit einem umgekehrtem Verhalten, das auf dem **IdxMode**-Bit basiert.)

``` syntax
bitcount get_color_bitcount(block, mode)
{
    if (mode.type == 0 OR == 1)
        return 3;
    
    if (mode.type == 2 OR == 3 OR == 5 OR == 7)
        return 2;
    
    if (mode.type == 6)
        return 4;
        
    //The only remaining case is Mode 4 with 1-bit index selector
    idxMode = extract_idxMode(block);
    if (idxMode == 0)
        return 2;
    else
        return 3;
}
```

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>Referenz zu überwachungsprotokollformaten im Modus "bc7" nicht


Dieser Abschnitt enthält eine Liste der 8 Block-Modi und Bit-Zuordnungen für BC7-Blöcke im Texturkomprimierungsformat.

Die Farben für jede Teilmenge in einem Block werden durch zwei explizite Endpunktfarben und ein Set an interpolierten Farben zwischen den beiden dargestellt. Je nach den der Genauigkeit des Block-Indexes kann jede Teilmenge über 4, 8 oder 16 mögliche Farben verfügen.

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>0-Modus

BC7-Modus 0 weist folgende Merkmale auf:

-   Nur Farbkomponenten (kein Alpha)
-   3 Teilmengen pro Block
-   RGBP 4.4.4.1-Endpunkte mit einem eindeutigen P-Bit pro Endpunkt
-   3-Bit-Indizes
-   16 Partitionen

![Modus 0-Bit-Layout](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>Modus 1

BC7-Modus 1 weist folgende Merkmale auf:

-   Nur Farbkomponenten (kein Alpha)
-   2 Teilmengen pro Block
-   RGBP 6.6.6.1 Endpunkte mit einem gemeinsamen P-Bit pro Teilmenge)
-   3-Bit-Indizes
-   64 Partitionen

![Modus 1-Bit-Layout](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>Modus 2

BC7-Modus 2 weist folgende Merkmale auf:

-   Nur Farbkomponenten (kein Alpha)
-   3 Teilmengen pro Block
-   RGB 5.5.5-Endpunkte
-   2-Bit-Indizes
-   64 Partitionen

![Modus 2-Bit-Layout](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>Modus 3

BC7-Modus 3 weist folgende Merkmale auf:

-   Nur Farbkomponenten (kein Alpha)
-   2 Teilmengen pro Block
-   RGBP 7.7.7.1 Endpunkte mit einem eindeutigen P-Bit pro Teilmenge)
-   2-Bit-Indizes
-   64 Partitionen

![Modus 3-Bit-Layout](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>Modus 4

BC7-Modus 4 weist folgende Merkmale auf:

-   Farbkomponenten mit separater Alpha-Komponente
-   1 Teilmenge pro Block
-   RGB 5.5.5-Farbendpunkte
-   6-Bit-Alpha-Endpunkte
-   16 x 2-Bit-Indizes
-   16 x 3-Bit-Indizes
-   Rotation der 2-Bit-Komponente
-   1-Bit-Index-Selektor (unabhängig davon, ob 2- oder 3-Bit-Indizes verwendet werden)

![Modus 4-Bit-Layout](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>Modus 5

BC7-Modus 5 weist folgende Merkmale auf:

-   Farbkomponenten mit separater Alpha-Komponente
-   1 Teilmenge pro Block
-   RGB 7.7.7-Farbendpunkte
-   6-Bit-Alpha-Endpunkte
-   16 x 2-Bit-Farbindizes
-   16 x 2-Bit-Alpha-Indizes
-   Rotation der 2-Bit-Komponente

![Modus 5-Bit-Layout](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>6-Modus

BC7-Modus 6 weist folgende Merkmale auf:

-   Kombinierte Farb- und Alpha-Komponenten
-   Eine Teilmenge pro Block
-   RGBAP 7.7.7.7.1-Farb- (und Alpha)-Endpunkte (eindeutiger P-Bit pro Endpunkt)
-   16 x 3-Bit-Indizes

![Modus 6-Bit-Layout](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>7-Modus

BC7-Modus 7 weist folgende Merkmale auf:

-   Kombinierte Farb- und Alpha-Komponenten
-   2 Teilmengen pro Block
-   RGBAP 5.5.5.5.1-Farb- (und Alpha)-Endpunkte (eindeutiger P-Bit pro Endpunkt)
-   2-Bit-Indizes
-   64 Partitionen

![Modus 7-Bit-Layout](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>"Hinweise"

Modus 8 (das Bit mit dem unerheblichsten Wert ist auf 0 x 00 Byte festgelegt) ist reserviert. Verwenden Sie es nicht für den Encoder. Wenn Sie diesen Modus an die Hardware übergeben, wird ein initialisierter Block mit nur Nullen zurückgegeben.

In BC7 können Sie die Alpha-Komponente auf eine der folgenden Weisen codieren:

-   Blocktypen ohne explizite Alpha-Komponenten-Codierung. In diesen Blöcken enthalten die Farbendpunkte eine reine RGB-Codierung, wobei die Alpha-Komponente auf 1,0 für alle Texel decodiert ist.
-   Blocktypen mit kombinierten Farb- und Alpha-Komponenten. Die Endpunkt-Farbwerte in diesen Blöcken werden im RGBA-Format angegeben und die Werte der Alpha-Komponente werden zusammen mit den Farbwerten interpoliert.
-   Blocktypen mit separaten Farb- und Alpha-Komponenten. Die Farb- und Alphawerte dieser Blöcke werden separat angegeben und enthalten jeweils ihr eigenes Set an Indizes. Sie haben daher eine effektive Vektor und einen skalaren Kanal, der separat codiert, wobei der Vektor der Farbkanäle häufig angibt \[R, G, B\] und der Skalar gibt den alpha-Kanal \[ein\]. Zur Unterstützung dieses Ansatzes wird ein eigenes 2-Bitfeld in der Codierung bereitgestellt, das die Spezifikation der separaten Kanal-Codierung als skalaren Wert zulässt. Daher kann der Block eine der folgenden vier verschiedenen Darstellungen dieser Alpha-Codierung haben (wie durch das 2-Bitfeld angezeigt):
    -   RGB|A: separater Alpha-Kanal
    -   AGB|R: „roter” separater Alpha-Kanal
    -   RAB|G: „grüner” separater Alpha-Kanal
    -   RGA|B: „blauer” separater Alpha-Kanal

    Der Decoder ordnet die Kanalreihenfolge nach der Decodierung wieder auf RGBA an, damit das interne Blockformat für den Entwickler unsichtbar ist. Schwarze Kanäle mit eigenen Farb- und Alpha-Komponenten verfügen auch über zwei Sätze an Indexdaten: einen für den Satz an Vektor-Kanälen und einen für den skalaren Kanal. (Im Fall von Modus 4, der diese Indizes werden mit unterschiedlichen Breiten und \[2 oder 3 Bits\]. Modus 4 enthält auch einen 1-Bit-Selektor, der angibt, ob der Vektor- oder der skalare Kanal die 3-Bit-Indizes verwendet.)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Block-texturkomprimierung](texture-block-compression.md)

 

 




