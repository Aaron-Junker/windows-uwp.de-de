---
title: BC6H-Format
description: Das BC6H-Format ist ein Format für die Texturkomprimierung, das „High Dynamic Range“-Farbräume (HDR) in Quelldaten unterstützt.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- BC6H-Format
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f147f4c30d2a662806df5928fc79178522b9b6a6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633095"
---
# <a name="bc6h-format"></a>BC6H-Format


Das BC6H-Format ist ein Format für die Texturkomprimierung, das „High Dynamic Range“-Farbräume (HDR) in Quelldaten unterstützt.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgiformatbc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>Informationen zu BC6H/DXGI\_FORMAT\_BC6H


Das BC6H-Format bietet eine hochwertige Komprimierung für Bilder, die drei HDR-Farbkanäle mit jeweils 16-Bit-Werten für jeden Farbkanal des Farbwerts (16: 16:16) verwenden. Alphakanäle werden nicht unterstützt.

BC6H wird angegeben, indem die folgenden DXGI\_FORMAT-Enumerationswerte fest:

-   **DXGI\_FORMAT\_BC6H\_TYPELESS**.
-   **DXGI\_FORMAT\_BC6H\_UF16**. Dieses BC6H-Format verwendet keine Bit mit Vorzeichen im 16-Bit-Gleitkommawert des Farbkanals.
-   **DXGI\_FORMAT\_BC6H\_SF16**. Dieses BC6H-Format verwendet eine Bit mit Vorzeichen im 16-Bit-Gleitkommawert des Farbkanals.

**Beachten Sie**    floating-Point-Format für Farbenkanäle der 16-Bit wird häufig als bezeichnet ein "halbe" Gleitkommawert Format. Dieses Format hat das folgende Bit-Layout:
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16 (Gleitkomma ohne Vorzeichen) | 5 Bit Exponent + 11 Bit Mantisse              |
| SF16 (Gleitkomma mit Vorzeichen)   | 1 Bit mit Vorzeichen + 5 Bit Exponent + 10 Bit Mantisse |

 

 

Das BC6H-Format kann für Texturressourcen wie [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277) (einschließlich Arrays), Texture3D oder TextureCube (einschließlich Arrays verwendet werden. Das Format gilt ebenfalls für alle Mip-Map-Oberflächen, die mit diesen Ressourcen verbunden sind.

BC6H verwendet eine feste Blockgröße von 16 Byte (128 Bit) und eine feste Kachelgröße von 4 × 4-Texel. Genau wie mit vorherigen BC-Formaten werden Texturbilder, die größer als die unterstützte Kachelgröße (4 × 4) sind, durch die Verwendung mehrerer Blöcke komprimiert. Diese Adressierungsidentität gilt auch für dreidimensionale Bilder, MIP-Maps, Cube-Zuordnungen und Texturarrays. Alle Bildkacheln müssen das gleiche Format aufweisen.

Einschränkungen des BC6H-Formats:

-   BC6H unterstützt die Denormalisierung der Gleitkommazahl. INF (unendlich) und NaN (keine Zahl) werden allerdings nicht unterstützt. Die Ausnahme ist das signierte BC6H (DXGI\_FORMAT\_BC6H\_SF16), die -INF (minus unendlich) unterstützt. Diese Unterstützung für -INF ist lediglich ein Artefakt des Formats selbst und wird nicht speziell von Encodern für dieses Format unterstützt. Wenn ein Encoder auf INF- (positiv oder negativ) oder NaN-Eingabedaten trifft, sollte der Encoder diese Daten auf den Wert der maximal zulässigen Nicht-INF-Darstellung konvertieren und NaN vor der Komprimierung „0” zuordnen.
-   Alphakanäle werden nicht von BC6H unterstützt.
-   Der BC6H-Decoder führt vor der Texturfilterung eine Dekomprimierung durch.
-   Die BC6H-Dekomprimierung muss die Bit präzise wiedergeben. Das heißt, die Hardware muss Ergebnisse zurückgeben, die mit dem in dieser Dokumentation beschriebenen Decoder übereinstimmen.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>BC6H-Implementierung


Ein BC6H-Block besteht aus Modus-Bit, komprimierten Endpunkten, komprimierten Indizes und einem optionalen Partitionsindex. Dieses Format legt 14 verschiedenen Modi fest.

Die Farbe eines Endpunkts wird als eine RGB-Dreiergruppe gespeichert. BC6H definiert eine Farbpalette auf einer ungefähren Linie für eine Anzahl von definierten Farbendpunkten. Je nach gewähltem Modus kann eine Kachel darüber hinaus in zwei Regionen unterteilt oder als Einzelregion behandelt werden. Dabei verfügt eine Kachel mit zwei Regionen über eine separate Reihe von Farben für den Endpunkte jeder Region. BC6H speichert einen Farbpaletten-Index pro Texel.

Im Fall von zwei Regionen sind 32 Partitionen möglich.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>Das Format BC6H-Decodierung


Der folgende Pseudocode zeigt die Schritte zur Dekomprimierung der Pixel (x, y) bei einem 16-Byte BC6H-Block an.

``` syntax
decompress_bc6h(x, y, block)
{
    mode = extract_mode(block);
    endpoints;
    index;
    
    if(mode.type == ONE)
    {
        endpoints = extract_compressed_endpoints(mode, block);
        index = extract_index_ONE(x, y, block);
    }
    else //mode.type == TWO
    {
        partition = extract_partition(block);
        region = get_region(partition, x, y);
        endpoints = extract_compressed_endpoints(mode, region, block);
        index = extract_index_TWO(x, y, partition, block);
    }
    
    unquantize(endpoints);
    color = interpolate(index, endpoints);
    finish_unquantize(color);
}
```

Die folgende Tabelle enthält die Anzahl und Werte der Bit für jedes der 14 möglichen Formate für BC6H-Blöcke.

| Modus | Partitionsindizes | Partition | Farbendpunkte                  | Modus-Bit      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 Bit           | 5 Bit    | 75 Bit (10.555, 10.555, 10.555) | 2 Bit (00)    |
| 2    | 46 Bit           | 5 Bit    | 75 Bit (7666, 7666, 7666)       | 2 Bit (01)    |
| 3    | 46 Bit           | 5 Bit    | 72 Bit (11.555, 11.444, 11.444) | 5 Bit (00010) |
| 4    | 46 Bit           | 5 Bit    | 72 Bit (11.444, 11.555, 11.444) | 5 Bit (00110) |
| 5    | 46 Bit           | 5 Bit    | 72 Bit (11.444, 11.444, 11.555) | 5 Bit (01010) |
| 6    | 46 Bit           | 5 Bit    | 72 Bit (9555, 9555, 9555)       | 5 Bit (01110) |
| 7    | 46 Bit           | 5 Bit    | 72 Bit (8666, 8555, 8555)       | 5 Bit (10010) |
| 8    | 46 Bit           | 5 Bit    | 72 Bit (8555, 8666, 8555)       | 5 Bit (10110) |
| 9    | 46 Bit           | 5 Bit    | 72 Bit (8555, 8555, 8666)       | 5 Bit (11010) |
| 10   | 46 Bit           | 5 Bit    | 72 Bit (6666, 6666, 6666)       | 5 Bit (11110) |
| 11   | 63 Bit           | 0 Bit    | 60 Bit (10.10, 10.10, 10.10)    | 5 Bit (00011) |
| 12   | 63 Bit           | 0 Bit    | 60 Bit (11.9, 11.9, 11.9)       | 5 Bit (00111) |
| 13   | 63 Bit           | 0 Bit    | 60 Bit (12.8, 12.8, 12.8)       | 5 Bit (01011) |
| 14   | 63 Bit           | 0 Bit    | 60 Bit (16.4, 16.4, 16.4)       | 5 Bit (01111) |

 

Jedes Format dieser Tabelle kann durch Modus-Bits eindeutig identifiziert werden. Die ersten zehn Modi werden für Kacheln mit zwei Regionen verwendet, wobei das Modus-Bitfeld entweder 2 oder 5 Bit lang sein kann. Diese Blöcke haben ebenfalls Felder für die komprimierten Farbendpunkte (72 oder 75 Bit), die Partition (5 Bit) und die Partitionsindizes (46 Bit).

Für die komprimierten Farbendpunkte geben die Werte der obigen Tabelle die Genauigkeit der gespeicherten RGB-Endpunkte und die verwendete Anzahl der Bit für jeden Farbwert an. Modus 3 gibt z. B. einen Endpunkt der Farbe der Präzisionsebene 11 an sowie die Anzahl der Bit, die zur Speicherung der Deltawerte der transformierten Farbendpunkte Rot, Blau und Grün (je 5, 4 und 4) verwendet werden. Modus 10 verwendet keine Delta-Komprimierung und speichert stattdessen explizit alle vier Farbendpunkte.

Die letzten vier Textblock-Modi werden für Kacheln mit einer Region verwendet, wobei das Modusfeld 5 Bit ist. Diese Blöcke verfügen über 5 Felder für die Endpunkte (60 Bit) und die komprimierten Indizes (63 Bit). Modus 11 (wie auch Modus 10) verwendet keine Delta-Komprimierung und speichert stattdessen explizit beide Farbendpunkte.

Modi 10011, 10111, 11011 und 11111 (nicht dargestellt) sind reserviert. Verwenden Sie diese nicht in Ihrem Encoder. Wenn die Hardware in einem dieser angegebenen Modi ausgeführt wird, darf der resultierende dekomprimierte Block nur Nullen in allen Kanälen - mit Ausnahme des Alphakanals - enthalten.

Für BC6H muss der Alphakanal - unabhängig vom Modus - immer 1,0 zurückgeben.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>BC6H Partitionsgruppe

Es gibt 32 mögliche Partitionen für eine Kachel mit zwei Regionen. Diese werden in der folgenden Tabelle definiert. Jeder 4 × 4-Block stellt eine einzelne Form dar.

![Tabelle mit Sätzen von BC6H-Partitionen](images/bc6h-partition-sets.png)

In dieser Tabelle mit Sätzen von Partitionen ist der fett formatierte und unterstrichene Eintrag der Speicherort des korrigierten Indexes für die Teilmenge 1 (diese wird mit einer Bit weniger angegeben). Der korrigierte Index der Teilmenge 0 ist immer 0, da die Partitionierung immer so angeordnet ist, dass der Index 0 sich immer in der Teilmenge 0 befindet. Die Partitionsreihenfolge wird von oben links nach unten rechts und von links nach rechts und dann von oben nach unten ausgeführt.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>Komprimierte Endpunktformat BC6H


![Bitfelder für komprimierte BC6H-Endpunktformate](images/bc6h-headers-med.png)

Diese Tabelle zeigt die Bitfelder für die komprimierten Endpunkte als Funktion des Endpunktformats an, wobei jede Spalte eine Codierung und jede Zeile ein Bitfeld angibt. Dieser Ansatz nimmt 82 Bit für Kacheln mit zwei Regionen und 65 Bit für Kacheln mit einer Region in Anspruch. Als Beispiel die ersten 5 Bits für die eine Region \[16 4\] oben (insbesondere die Spalte ganz rechts)-Codierung werden Bits m\[4:0\], die nächsten 10 Bits sind Bits-RW-Medien\[9:0\]usw. mit den letzten 6 Bits mit bw\[10:15\].

Die Feldnamen in der obigen Tabelle sind wie folgt definiert:

| Feld | Variable          |
|-------|-------------------|
| m     | mode              |
| d     | shape index       |
| rw    | Endpt\[0\]. Ein\[0\] |
| rx    | Endpt\[0\]. B\[0\] |
| ry    | Endpt\[1\]. Ein\[0\] |
| rz    | Endpt\[1\]. B\[0\] |
| gw    | Endpt\[0\]. Ein\[1\] |
| gx    | Endpt\[0\]. B\[1\] |
| gy    | Endpt\[1\]. Ein\[1\] |
| gz    | Endpt\[1\]. B\[1\] |
| bw    | Endpt\[0\]. Ein\[2\] |
| bx    | Endpt\[0\]. B\[2\] |
| by    | Endpt\[1\]. Ein\[2\] |
| bz    | Endpt\[1\]. B\[2\] |

 

Endpt\[ich\], wobei entweder 0 oder 1, "i" bezieht sich auf die 0. oder 1. Gruppe von Endpunkten bzw.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>SSO-Erweiterung für die endpunktwerte


Bei Kacheln mit zwei Regionen gibt es vier Endpunktwerte, für die eine Sign-Erweiterung möglich ist. Endpt\[0\]. A ist nur dann, wenn das Format einer signierten Format ist signiert die anderen Endpunkte werden signiert, nur dann, wenn der Endpunkt transformiert wurde, oder wenn das Format einer signierten Format ist. Der folgende Code veranschaulicht den Algorithmus für das Erweitern der Vorzeichen für Endpunktwerte mit zwei Regionen.

``` syntax
static void sign_extend_two_region(Pattern &p, IntEndpts endpts[NREGIONS_TWO])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
      if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
      if (p.transformed || BC6H::FORMAT == SIGNED_F16)
      {
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
        endpts[1].A[i] = SIGN_EXTEND(endpts[1].A[i], p.chan[i].delta[1]);
        endpts[1].B[i] = SIGN_EXTEND(endpts[1].B[i], p.chan[i].delta[2]);
      }
    }
}
```

Für Kacheln, die eine Region, das Verhalten ist identisch, nur mit Endpt\[1\] entfernt.

``` syntax
static void sign_extend_one_region(Pattern &p, IntEndpts endpts[NREGIONS_ONE])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
    if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
    if (p.transformed || BC6H::FORMAT == SIGNED_F16) 
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
    }
}
```

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>Inversion für endpunktwerte transformieren


Für Kacheln, die zwei Regionen, die Transformation wird die Umkehrung des Unterschieds codieren, Hinzufügen von den Basiswert zur Endpt\[0\]. Ein auf die drei anderen Einträge für einen Zeitraum von 9 Vorgänge hinzufügen. In der folgenden Abbildung wird der Basiswert als „A0” dargestellt und weist die höchste Gleitkommapräzision auf. "A1", "B0" und "B1" sind alle vom Anchor-Wert berechnete Deltas. Diese Deltawerte werden mit geringerer Präzision dargestellt. (A0 entspricht Endpt\[0\]. A, B0 entspricht Endpt\[0\]. B A1 entspricht Endpt\[1\]. Ein, und B1 entspricht Endpt\[1\]. B.)

![Berechnung der Inversionstransformation der Endpunktwerte](images/bc6h-transform-inverse.png)

Für Kacheln mit einer Region gibt es nur eine Delta-Abweichung und daher nur 3 hinzugefügte Vorgänge.

Das Dekomprimierungsprogramm muss sicherstellen, dass die Genauigkeit der Endpt nicht nicht, dass die Ergebnisse der inversen umwandeln ausreicht,\[0\]. ein. Im Falle eines Überlaufs müssen die durch die Inversionstransformation enthaltenen Werte innerhalb der gleichen Anzahl von Bits umbrochen werden. Wenn die Genauigkeit von A0 „p” Bit ist, ist der Transformationsalgorithmus wie folgt:

`B0 = (B0 + A0) & ((1 << p) - 1)`

Für Formate mit Vorzeichen müssen die Ergebnisse der Delta-Berechnung ebenfalls eine Zeichenerweiterung enthalten. Wenn der Zeichenerweiterungsvorgang die Erweiterung beider Anzeichen berücksichtigt, wobei 0 positiv und 1 negativ ist, übernimmt die Zeichenerweiterung von 0 den vorherigen „Clamp”. Außerdem muss nach dem vorigen „Clamp” nur der Wert 1 (negativ) mit einem Zeichen erweitert werden.

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>Unquantization Farbe Endpunkte


Angesichts der nicht komprimierten Endpunkte ist der nächste Schritt die Ausführung einer anfänglichen Entquantisierung der Farbendpunkte. Dies umfasst drei Schritte:

-   Entquantisierung der Farbpaletten
-   Interpolation der Paletten
-   Abschließen der Entquantisierung

Das Trennen des Entquantisierungsprozesses in zwei Schritte (Entquantisieren der Farbpalette vor der Interpolation und endgültiges Entquantisieren der Farbpalette nach der Interpolation) reduziert die Anzahl der Multiplikationsvorgänge im Vergleich zu einem vollständigen Entquantisierungsprozess vor der Interpolation der Palette.

Der folgende Code veranschaulicht das Verfahren zum Abrufen von Schätzungen der ursprünglichen 16-Bit-Farbwerte und die anschließende Nutzung der angegebenen Gewichtungswerte, um der Palette 6 weitere Farbwerte hinzuzufügen. Der gleiche Vorgang wird auf jedem Kanal durchgeführt.

``` syntax
int aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
int aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

// c1, c2: endpoints of a component
void generate_palette_unquantized(UINT8 uNumIndices, int c1, int c2, int prec, UINT16 palette[NINDICES])
{
    int* aWeights;
    if(uNumIndices == 8)
        aWeights = aWeight3;
    else  // uNumIndices == 16
        aWeights = aWeight4;

    int a = unquantize(c1, prec); 
    int b = unquantize(c2, prec);

    // interpolate
    for(int i = 0; i < uNumIndices; ++i)
        palette[i] = finish_unquantize((a * (64 - aWeights[i]) + b * aWeights[i] + 32) >> 6);
}
```

Im nächsten Codebeispiel wird der Interpolationsprozess veranschaulicht, mit folgender Erläuterung:

-   Da der vollständige Bereich der Farbwerte der Funktion **Entquantisierung** (siehe unten) zwischen -32768 und 65535 liegt, wird der Interpolator mithilfe der Arithmetik-Berechnungen von 17-Bit mit Vorzeichen implementiert.
-   Nach der Interpolation, die Werte übergeben werden, um die **Fertig stellen\_unquantize** -Funktion (im dritten Beispiel in diesem Abschnitt beschrieben), die die endgültige Skalierung gilt.
-   Alle Hardware Dekomprimierprogramme müssen mithilfe dieser Funktion die Bit präzise wiedergeben.

``` syntax
int unquantize(int comp, int uBitsPerComp)
{
    int unq, s = 0;
    switch(BC6H::FORMAT)
    {
    case UNSIGNED_F16:
        if(uBitsPerComp >= 15)
            unq = comp;
        else if(comp == 0)
            unq = 0;
        else if(comp == ((1 << uBitsPerComp) - 1))
            unq = 0xFFFF;
        else
            unq = ((comp << 16) + 0x8000) >> uBitsPerComp;
        break;

    case SIGNED_F16:
        if(uBitsPerComp >= 16)
            unq = comp;
        else
        {
            if(comp < 0)
            {
                s = 1;
                comp = -comp;
            }

            if(comp == 0)
                unq = 0;
            else if(comp >= ((1 << (uBitsPerComp - 1)) - 1))
                unq = 0x7FFF;
            else
                unq = ((comp << 15) + 0x4000) >> (uBitsPerComp-1);

            if(s)
                unq = -unq;
        }
        break;
    }
    return unq;
}
```

**Fertig stellen\_unquantize** wird aufgerufen, nachdem die Palette Interpolation. Die Funktion **Entquantisierung** verschiebt die Skalierung von 31/32 für signierte und von 31/64 für nicht-signierte Zeichen. Dieses Verhalten ist erforderlich, um den Endwert in einem gültigen Halb-Bereich zu erhalten (-0x7BFF ~ 0x7BFF), nachdem die Interpolation der Palette abgeschlossen ist, um die Anzahl der erforderlichen Multiplikationen zu verringern. **Fertig stellen\_unquantize** gilt die letzte Skalierung und gibt eine **unsigned short** -Wert, der in neu interpretiert ruft **Hälfte**.

``` syntax
unsigned short finish_unquantize(int comp)
{
    if(BC6H::FORMAT == UNSIGNED_F16)
    {
        comp = (comp * 31) >> 6;                                         // scale the magnitude by 31/64
        return (unsigned short) comp;
    }
    else // (BC6H::FORMAT == SIGNED_F16)
    {
        comp = (comp < 0) ? -(((-comp) * 31) >> 5) : (comp * 31) >> 5;   // scale the magnitude by 31/32
        int s = 0;
        if(comp < 0)
        {
            s = 0x8000;
            comp = -comp;
        }
        return (unsigned short) (s | comp);
    }
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Block-texturkomprimierung](texture-block-compression.md)

 

 




