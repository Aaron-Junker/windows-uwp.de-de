---
title: BC6H-Format
description: Das BC6H-Format ist ein Textur Komprimierungs Format, das für die Unterstützung von HDR-Farbräumen (High Dynamic Range) in Quelldaten konzipiert ist.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- BC6H-Format
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 668a95d92926ddce17d4003f66672ef1e7a3f34e
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105939045"
---
# <a name="bc6h-format"></a>BC6H-Format


Das BC6H-Format ist ein Textur Komprimierungs Format, das für die Unterstützung von HDR-Farbräumen (High Dynamic Range) in Quelldaten konzipiert ist.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgi_format_bc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>Informationen zum BC6H/DXGI- \_ Format \_ BC6H


Das BC6H-Format bietet hochwertige Komprimierung für Bilder, die drei HDR-Farbkanäle verwenden, mit einem 16-Bit-Wert für jeden Farbkanal des Werts (16:16:16). Es gibt keine Unterstützung für einen Alphakanal.

BC6H wird durch die folgenden DXGI- \_ formatenumerationswerte angegeben:

-   **DXGI \_ Format \_ BC6H \_ typlose Formatierung**.
-   **DXGI \_ Format \_ BC6H \_ UF16**. Dieses BC6H-Format verwendet kein Signier Bit in den 16-Bit-Farb Kanal Werten für Gleit Komma Zahlen.
-   **DXGI \_ Format \_ BC6H \_ SF16**. Dieses BC6H-Format verwendet ein Signier Bit in den 16-Bit-Farb Kanal Werten für Gleit Komma Zahlen.

**Hinweis**   Das 16-Bit-Gleit Komma Format für Farbkanäle wird häufig als "halb"-Gleit Komma Format bezeichnet. Dieses Format weist das folgende bitlayout auf:

| Format                      |      Layout                                           |
|-----------------------|-------------------------------------------------|
| UF16 (unsigned float) | 5 Exponent Bits + 11 Mantisse Bits              |
| SF16 (mit Vorzeichen signiert)   | 1 Vorzeichen Bit + 5 Exponent Bits + 10 Mantisse Bits |

 

 

Das BC6H-Format kann für die Textur Ressourcen [Texture2D](/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (einschließlich Arrays), Texture3D oder texturecube (einschließlich Arrays) verwendet werden. Ebenso gilt dieses Format für alle mit diesen Ressourcen verknüpften MIP-Map-Oberflächen.

BC6H verwendet eine fester Blockgröße von 16 Bytes (128 Bits) und eine fixierte Kachel Größe von 4 x 4 texeln. Wie bei früheren BC-Formaten werden Textur Bilder, die größer als die unterstützte Kachel Größe (4X4) sind, mithilfe mehrerer Blöcke komprimiert. Diese Adressierungs Identität gilt auch für dreidimensionale Bilder, MIP-Maps, Cubemaps und Textur Arrays. Alle Bildkacheln müssen das gleiche Format aufweisen.

Einschränkungen im BC6H-Format:

-   BC6H unterstützt die Floating-Point-Denormalisierung, unterstützt jedoch nicht inf (unendlich) und NaN (keine Zahl). Die Ausnahme ist der signierte Modus von BC6H (DXGI \_ Format \_ BC6H \_ SF16), der-INF unterstützt (minus unendlich). Diese Unterstützung für-INF ist lediglich ein Element des Formats selbst und wird nicht ausdrücklich von Encodern für dieses Format unterstützt. Wenn ein Encoder in der Regel inf-(positive oder negative) oder NaN-Eingabedaten erkennt, sollte der Encoder diese Daten in den maximal zulässigen nicht-INF-Darstellungs Wert konvertieren und Nan vor der Komprimierung zu 0 zuordnen.
-   BC6H unterstützt keinen Alphakanal.
-   Der BC6H-Decoder führt die Komprimierung aus, bevor die Textur Filterung durchführt.
-   Die BC6H-Komprimierung muss etwas genau sein. Das heißt, dass die Hardware Ergebnisse zurückgeben muss, die mit dem in dieser Dokumentation beschriebenen Decoder identisch sind.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>BC6H-Implementierung


Ein BC6H-Block besteht aus modusbits, komprimierten Endpunkten, komprimierten Indizes und einem optionalen Partitions Index. Dieses Format gibt 14 verschiedene Modi an.

Eine Endpunktfarbe wird als RGB-Dreieck gespeichert. BC6H definiert eine Palette von Farben in einer ungefähren Linie für eine Reihe definierter Farb Endpunkte. Abhängig vom Modus kann eine Kachel auch in zwei Bereiche aufgeteilt oder als eine einzelne Region behandelt werden, in der eine Kachel mit zwei Regionen über einen separaten Satz von Farb Endpunkten für jede Region verfügt. BC6H speichert einen Palettenindex pro Texel.

Im Fall von zwei Regionen gibt es 32 mögliche Partitionen.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>Decodieren des BC6H-Formats


Der folgende Pseudo Code zeigt die Schritte zum Dekomprimieren des Pixels bei (x, y), wenn der 16-Byte-BC6H-Block angegeben wird.

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

Die folgende Tabelle enthält die Bitanzahl und die Werte für jedes der 14 möglichen Formate für BC6H-Blöcke.

| Mode | Partitions Indizes | Partition | Farb Endpunkte                  | Modusbits      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 Bits           | 5 Bits    | 75 Bits (10,555, 10,555, 10,555) | 2 Bits (00)    |
| 2    | 46 Bits           | 5 Bits    | 75 Bits (7666, 7666, 7666)       | 2 Bits (01)    |
| 3    | 46 Bits           | 5 Bits    | 72 Bits (11,555, 11,444, 11,444) | 5 Bits (00010) |
| 4    | 46 Bits           | 5 Bits    | 72 Bits (11,444, 11,555, 11,444) | 5 Bits (00110) |
| 5    | 46 Bits           | 5 Bits    | 72 Bits (11,444, 11,444, 11,555) | 5 Bits (01010) |
| 6    | 46 Bits           | 5 Bits    | 72 Bits (9555, 9555, 9555)       | 5 Bits (01110) |
| 7    | 46 Bits           | 5 Bits    | 72 Bits (8666, 8555, 8555)       | 5 Bits (10010) |
| 8    | 46 Bits           | 5 Bits    | 72 Bits (8555, 8666, 8555)       | 5 Bits (10110) |
| 9    | 46 Bits           | 5 Bits    | 72 Bits (8555, 8555, 8666)       | 5 Bits (11010) |
| 10   | 46 Bits           | 5 Bits    | 72 Bits (6666, 6666, 6666)       | 5 Bits (11110) |
| 11   | 63 Bits           | 0 Bits    | 60 Bits (10,10, 10,10, 10,10)    | 5 Bits (00011) |
| 12   | 63 Bits           | 0 Bits    | 60 Bits (11,9, 11,9, 11,9)       | 5 Bits (00111) |
| 13   | 63 Bits           | 0 Bits    | 60 Bits (12,8, 12,8, 12,8)       | 5 Bits (01011) |
| 14   | 63 Bits           | 0 Bits    | 60 Bits (16,4, 16,4, 16,4)       | 5 Bits (01111) |

 

Jedes Format in dieser Tabelle kann durch die modusbits eindeutig identifiziert werden. Die ersten zehn Modi werden für Kacheln mit zwei Regionen verwendet, und das Bitfeld des Modus kann entweder zwei oder fünf Bits lang sein. Diese Blöcke verfügen auch über Felder für die komprimierten Farb Endpunkte (72 oder 75 Bits), die Partition (5 Bits) und die Partitions Indizes (46 Bits).

Für die komprimierten Farb Endpunkte notieren die Werte in der obigen Tabelle die Genauigkeit der gespeicherten RGB-Endpunkte und die Anzahl der Bits, die für die einzelnen Farbwerte verwendet werden. Beispielsweise gibt Modus 3 eine farbend Punktgenauigkeit von 11 und die Anzahl der Bits an, die zum Speichern der Delta Werte der transformierten Endpunkte für die Farben rot, blau und grün (5, 4 bzw. 4) verwendet werden. Der Modus 10 verwendet keine Delta Komprimierung und speichert stattdessen alle vier Farb Endpunkte explizit.

Die letzten vier Block Modi werden für Kacheln der einzelnen Regionen verwendet, wobei das Feld Modus 5 Bits ist. Diese Blöcke verfügen über Felder für die Endpunkte (60 Bits) und die komprimierten Indizes (63 Bits). Der Modus 11 (z. b. Modus 10) verwendet keine Delta Komprimierung, sondern speichert beide Farb Endpunkte explizit.

Die Modi 10011, 10111, 11011 und 11111 (nicht angezeigt) sind reserviert. Verwenden Sie diese nicht im Encoder. Wenn die Hardware Blöcke mit einem dieser Modi überschritten werden, muss der resultierende dekomprimierte Block alle Nullen in allen Kanälen mit Ausnahme des Alphakanals enthalten.

Bei BC6H muss der Alphakanal unabhängig vom Modus immer 1,0 zurückgeben.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>BC6H-Partitions Satz

Es gibt 32 mögliche Partitions Sätze für eine Kachel mit zwei Regionen, die in der folgenden Tabelle definiert sind. Jeder 4X4-Block stellt eine einzelne Form dar.

![Tabelle von bc6h-Partitions Sätzen](images/bc6h-partition-sets.png)

In dieser Tabelle mit Partitions Sätzen ist der fett formatierten und unterstrichene Eintrag der Speicherort des fixupindex für die Teilmenge 1 (die mit einem geringeren Bit angegeben wird). Der fixupindex für Teilmenge 0 ist immer Index 0, da die Partitionierung immer so angeordnet ist, dass Index 0 immer in der Teilmenge 0 ist. Die Partitions Reihenfolge wechselt von oben links nach unten rechts, von links nach rechts und von oben nach unten.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>BC6H-komprimiertes Endpunkt Format


![Bitfelder für bc6h-komprimierte Endpunkt Formate](images/bc6h-headers-med.png)

Diese Tabelle zeigt die Bitfelder für die komprimierten Endpunkte als Funktion des Endpunkt Formats an, wobei jede Spalte eine Codierung und jede Zeile, die ein Bitfeld angibt, angibt. Bei diesem Ansatz werden 82 Bits für Kacheln mit zwei Regionen und 65 Bits für Kacheln der einzelnen Regionen benötigt. Beispielsweise sind die ersten 5 Bits für die oben genannte 1-Region \[ 16 4- \] Codierung (insbesondere die Spalte ganz rechts) Bits m \[ 4:0 \] , die nächsten 10 Bits sind Bits RW \[ 9:0 \] , usw. mit den letzten 6 Bits, die BW 10:15 enthalten \[ \] .

Die Feldnamen in der obigen Tabelle sind wie folgt definiert:

| Feld | Variable          |
|-------|-------------------|
| m     | Modus              |
| d     | Shape-Index       |
| RW    | Endpt \[ 0 \] . Ein \[ 0\] |
| RX    | Endpt \[ 0 \] . B \[ 0\] |
| Wahn    | Endpt \[ 1 \] . Ein \[ 0\] |
| RZ    | Endpt \[ 1 \] . B \[ 0\] |
| GW    | Endpt \[ 0 \] . A \[ 1\] |
| GX    | Endpt \[ 0 \] . B \[ 1\] |
| schlank    | Endpt \[ 1 \] . A \[ 1\] |
| gz    | Endpt \[ 1 \] . B \[ 1\] |
| BW    | Endpt \[ 0 \] . A \[ 2\] |
| BX    | Endpt \[ 0 \] . B \[ 2\] |
| by    | Endpt \[ 1 \] . A \[ 2\] |
| BZ    | Endpt \[ 1 \] . B \[ 2\] |

 

"Endpt \[ i" \] , wobei "0" oder "1" ist, bezieht sich auf den bzw. den jeweils ersten Satz von Endpunkten.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>Signieren der Erweiterung für Endpunkt Werte


Für Kacheln mit zwei Regionen gibt es vier Endpunkt Werte, für die das Vorzeichen erweitert werden kann. Endpt \[ 0 \] . Ein wird nur signiert, wenn das Format ein signiertes Format ist. die anderen Endpunkte werden nur signiert, wenn der Endpunkt transformiert wurde, oder, wenn das Format ein signiertes Format ist. Der folgende Code veranschaulicht den Algorithmus zum Erweitern des Vorzeichen von Endpunkt Werten mit zwei Regionen.

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

Bei Kacheln der einzelnen Regionen ist das Verhalten identisch, nur wenn Endpt \[ 1 \] entfernt wurde.

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

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>Inversion für Endpunkt Werte transformieren


Bei Kacheln mit zwei Regionen wendet die Transformation die Umkehrung der Differenz Codierung an und fügt den Basiswert bei Endpt \[ 0 hinzu \] . Eine zu den drei anderen Einträgen für insgesamt 9 Add-Vorgänge. In der folgenden Abbildung wird der Basiswert als "a0" dargestellt und verfügt über die höchste Gleit Komma Genauigkeit. "A1", "B0" und "B1" sind alle Deltas, die aus dem Anker Wert berechnet werden, und diese Delta Werte werden mit niedrigerer Genauigkeit dargestellt. (A0 entspricht Endpt \[ 0 \] . A, B0 entspricht Endpt \[ 0 \] . B, a1 entspricht Endpt \[ 1 \] . A und B1 entsprechen Endpt \[ 1 \] . B.)

![Berechnung der Endpunkt Werte der Transformations Umkehrung](images/bc6h-transform-inverse.png)

Für Kacheln der 1-Region gibt es nur einen Delta Offset und somit nur drei Add-Vorgänge.

Der-Debug muss sicherstellen, dass die Ergebnisse der umgekehrten Transformation nicht die Genauigkeit von Endpt \[ 0 \] . a überlaufen. Im Fall eines Überlaufs müssen die Werte, die sich aus der umgekehrten Transformation ergeben, innerhalb der gleichen Anzahl von Bits umschlossen werden. Wenn die Genauigkeit von a0 "p" ist, lautet der Transformations Algorithmus wie folgt:

`B0 = (B0 + A0) & ((1 << p) - 1)`

Bei signierten Formaten müssen die Ergebnisse der Delta Berechnung ebenfalls mit Vorzeichen erweitert werden. Wenn beim Vorgang zum Signieren der Signierung beide Zeichen erweitert werden, wobei 0 positiv und 1 negativ ist, wird die oben genannte Klammer durch die Signierungs Erweiterung 0 übernommen. Gleichwertig, nach der obigen Klammer, muss nur der Wert 1 (negativ) mit Extended signiert werden.

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>Unquantifization von Farb Endpunkten


Bei den nicht komprimierten Endpunkten ist der nächste Schritt das Ausführen einer anfänglichen unquantisierung der Farb Endpunkte. Dies umfasst drei Schritte:

-   Eine unquantisierung der Farbpaletten.
-   Interpolations der Paletten
-   Beendigung der nicht quantifialisierung

Wenn Sie den unquantisierungsprozess in zwei Teile aufteilen (vor Interpolations-und abschließende unquantisierung nach der interpolung), wird die Anzahl der Multiplikations Vorgänge verringert, die im Vergleich zu einem vollständigen unquantisierungsprozess vor der paletteninterpolung erforderlich sind.

Der folgende Code veranschaulicht den Prozess zum Abrufen von Schätzungen der ursprünglichen 16-Bit-Farbwerte und verwendet dann die angegebenen Gewichtungswerte, um der Palette 6 zusätzliche Farbwerte hinzuzufügen. Der gleiche Vorgang wird für jeden Kanal ausgeführt.

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

Im nächsten Codebeispiel wird der Interpolations Prozess mit den folgenden Beobachtungen veranschaulicht:

-   Da der vollständige Bereich von Farbwerten für die **unquantifize** -Funktion (unten) zwischen-32768 und 65535 liegt, wird der interpolators mit einer 17-Bit-Arithmetik mit Vorzeichen implementiert.
-   Nach der interpolung werden die Werte an die Funktion zur Fertigstellung von **\_ unquantifize** (im dritten Beispiel in diesem Abschnitt beschrieben) weitergegeben, die die endgültige Skalierung anwendet.
-   Alle Hardware-Dekompressoren müssen mit diesen Funktionen bitgenaue Ergebnisse zurückgeben.

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

**" \_ unquantize beenden** " wird nach der paletteninterpolung aufgerufen. Die Funktion " **unquantifize** " verschiebt die Skalierung um 31/32 für "signiert", 31/64 für "unsigned". Dieses Verhalten ist erforderlich, um den endgültigen Wert in einen gültigen halben Bereich (-0x7bff ~ 0x7bff) zu bringen, nachdem die paletteninterpolung abgeschlossen ist, um die Anzahl der erforderlichen Multiplikationen zu verringern. **" \_ unquantize abschließen** " wendet die endgültige Skalierung an und gibt einen **kurzen Wert ohne** Vorzeichen zurück, der in eine **Hälfte** reinterpretiert wird.

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


[Textur Block Komprimierung](texture-block-compression.md)

 

 