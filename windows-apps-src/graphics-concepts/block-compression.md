---
title: Blockkomprimierung
description: Blockkomprimierung ist ein verlustbehaftetes Texturkomprimierungsverfahren zum Reduzieren der Texturgröße und des Speicherbedarfs und zur Steigerung der Leistung. Eine blockkomprimierte Textur kann kleiner als eine Textur mit 32 Bit pro Farbe sein.
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- Blockkomprimierung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2ac7f0785894849cffe09cd902f459015f1f7b6b
ms.sourcegitcommit: ea15237291ae3ade0bf22e38bd292c3a23947a03
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2019
ms.locfileid: "66377323"
---
# <a name="block-compression"></a>Blockkomprimierung

Blockkomprimierung ist ein verlustbehaftetes Texturkomprimierungsverfahren zum Reduzieren der Texturgröße und des Speicherbedarfs und zur Steigerung der Leistung. Eine blockkomprimierte Textur kann kleiner als eine Textur mit 32 Bit pro Farbe sein.

Blockkomprimierung ist eine Texturkomprimierungstechnik zum Reduzieren der Texturgröße. Im Vergleich zu einer Textur mit 32 Bit pro Farbe kann eine blockkomprimierte Textur bis zu 75 Prozent kleiner sein. Anwendungen sehen generell eine Leistungssteigerung beim Verwenden der Blockkomprimierung durch einen verringerten Speicherbedarf.

Obwohl sie verlustbehaftet ist, funktioniert die Blockkomprimierung und wird für alle Texturen empfohlen, die von der Pipeline transformiert und gefiltert werden. Texturen, die dem Bildschirm direkt zugeordnet werden (UI-Elemente wie Symbole und Text) eignen sich nicht für die Komprimierung, da Artefakte deutlich wahrnehmbar sind.

Eine blockkomprimierte Textur muss als eine Potenz von 4 in allen Dimensionen erstellt werden und darf nicht als Ausgabe der Pipeline verwendet werden.

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>Blockieren Komprimierung funktioniert wie

Die Blockkomprimierung ist eine Methode zum Verringern des zum Speichern von Farbdaten benötigten Arbeitsspeicherbedarfs. Durch das Speichern von Farben in deren ursprünglichen Größe und von Farben mit einem Codierungsschema, können Sie den zum Speichern des Bildes benötigten Arbeitsspeicherbedarf erheblich reduzieren. Da die Hardware automatisch komprimierte Daten entschlüsselt, besteht keine Leistungseinbuße für die Verwendung komprimierter Texturen.

Betrachten Sie die folgenden zwei Beispiele, um zu sehen, wie die Komprimierung funktioniert. Das erste Beispiel zeigt den erforderlichen Speicherbedarf beim Speichern von nicht komprimierten Daten. Das zweite Beispiel zeigt den erforderlichen Speicherbedarf beim Speichern von komprimierten Daten.

- [Das Speichern von nicht komprimierten Daten](#storing-uncompressed-data)
- [Das Speichern komprimierter Daten](#storing-compressed-data)

### <a name="span-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>Das Speichern von nicht komprimierten Daten

Die folgende Abbildung zeigt eine nicht komprimierte 4 × 4-Textur. Angenommen, jede Farbe enthält eine einzelne Farbkomponente (z. B. rot) und wird in einem Byte Arbeitsspeicher gespeichert.

![eine nicht komprimierte 4 x 4-Textur](images/d3d10-block-compress-1.png)

Die nicht komprimierten Daten werden im Arbeitsspeicher nacheinander verteilt und erfordern 16 Byte, wie in der folgenden Abbildung dargestellt.

![nicht komprimierte Daten im sequenziellen Arbeitsspeicher](images/d3d10-block-compress-2.png)

### <a name="span-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>Das Speichern komprimierter Daten

Da Sie jetzt gesehen haben, wie viel Arbeitsspeicher ein nicht komprimiertes Bild verwendet, betrachten wir einmal, wie viel Arbeitsspeicher ein komprimiertes Bild erfordert. Das [BC1](#bc1)-Komprimierungsformat speichert 2 Farben (jeweils 1 Byte) und 16 3-Bit-Indizes (48 Bit oder 6 Bytes), die zum Interpolieren der ursprünglichen Farben in der Textur verwendet werden, wie in der folgenden Abbildung dargestellt.

![das BC1-Komprimierungsformat](images/d3d10-block-compress-3.png)

Der insgesamt benötigte Speicherplatz für die komprimierten Daten ist 8 Byte, also eine Arbeitsspeichereinsparungen von 50 Prozent im Gegensatz zum nicht komprimierten Beispiel. Die Einsparungen sind dann noch größer, wenn mehr als eine Farbkomponente verwendet wird.

Die durch Blockkomprimierung erhaltenen wesentlichen Arbeitsspeichereinsparungen können zu einer Leistungssteigerung führen. Diese Leistung geht zu Lasten der Bildqualität (aufgrund der Farbinterpolation); die geringere Qualität ist jedoch häufig nicht erkennbar.

Im nächste Abschnitt wird gezeigt, wie Direct3D das Verwenden der Blockkomprimierung in einer Anwendung ermöglicht.

## <a name="span-idusingblockcompressionspanspan-idusingblockcompressionspanspan-idusingblockcompressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>Verwenden die blockkomprimierung

Eine blockkomprimierte Textur kann genau wie eine nicht komprimierte Textur erstellt werden, außer dass Sie dabei ein blockkomprimiertes Format angeben.

Erstellen Sie dann eine Ansicht, um die Textur an die Pipeline zu binden. Da eine blockkomprimierte Textur nur als Eingabe für eine Shaderphase verwendet werden kann, müssen Sie eine Shaderressourcenansicht erstellen.

Verwenden Sie eine blockkomprimierte Textur genau wie eine nicht komprimierte Textur. Wenn Ihre Anwendung einen Speicherzeiger für blockkomprimierte Daten erhält, müssen Sie den Speicherabstand in einer Mipmap berücksichtigen, durch die sich die deklarierte Größe von der tatsächlichen Größe unterscheidet.

- [Virtuelle Größe, die im Vergleich zur physischen Größe](#virtual-size-versus-physical-size)

### <a name="span-idvirtualsizespanspan-idvirtualsizespanspan-idvirtualsizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>Virtuelle Größe, die im Vergleich zur physischen Größe

Wenn Sie einen App-Code besitzen, der einen Speicherzeiger zur Anzeige des Speichers einer blockkomprimierten Textur besitzt, gibt es einen wichtigen Aspekt, der eventuell eine Änderung im Anwendungscode erfordert. Eine blockkomprimierte Textur muss als eine Potenz von 4 in allen Dimensionen erstellt werden, da die Algorithmen für die Blockkomprimierung auf 4 x 4-Texel-Blöcken funktionieren. Das ist ein Problem für eine Mipmap, da deren anfängliche Dimensionen zwar durch 4 teilbar sind, doch deren unterteilten Ebenen nicht. Das folgende Diagramm zeigt die Differenz zwischen der virtuellen (deklarierten) und der physischen (aktuellen) Größe aller Mipmap-Ebenen an.

![Nicht komprimiert und komprimierte Mipmap-Ebenen](images/d3d10-block-compress-pad.png)

Auf der linken Seite des Diagramms sehen Sie die Mipmap-Ebenengrößen, die für eine nicht komprimierte 60 x 40 Textur generiert werden. Die Größe der obersten Ebene stammt vom API-Aufruf, der die Textur generiert. Jeder nachfolgende Ebene ist halb so groß wie die vorherige Ebene. Für eine nicht komprimierte Textur besteht kein Unterschied zwischen der virtuellen (deklarierten) und der physischen (tatsächlichen) Größe.

Auf der rechten Seite des Diagramms sehen Sie die Mipmap-Ebenengrößen, die für die gleiche 60 x 40 Textur durch Komprimierung generiert werden. Beachten Sie, dass die zweiten und dritten Ebenen einen Arbeitsspeicherabstand aufweisen, um die Größen auf jeder Ebene als ein Vierfaches festzulegen. Dies ist erforderlich, damit die Algorithmen für 4 × 4-Texel-Blöcke verwendet werden können. Dies ist besonders bemerkbar, wenn Sie Mipmap-Ebenen in Betracht ziehen, die kleiner als 4 × 4; die Größe dieser sehr kleine Mipmap-Ebenen wird aufgerundet auf die nächste Stufe der 4 Wenn Texturspeicher zugeordnet wird.

Sampling-Hardware verwendet die virtuelle Größe. Wenn die Textur ermittelt wird, wird der Speicherabstand ignoriert. Für Mipmap-Ebenen, die kleiner als 4 × 4 sind, werden nur die ersten vier Texel für eine 2 x 2-Karte und nur der erste Texel für einen 1 x 1-Block verwendet. Es gibt jedoch keine API-Struktur, die die physische Größe (einschließlich des Arbeitsspeicherabstands) anzeigt.

Zusammenfassung: Seien Sie bei der Verwendung von ausgerichteten Speicherblöcken vorsichtig, wenn Sie Regionen kopieren, die blockkomprimierte Daten enthalten. Um dies in einer Anwendung mit Speicherzeiger zu verwenden, stellen Sie sicher, dass der Zeiger den Neigungswinkel der Oberfläche verwendet, um die Größe des physischen Speichers zu berücksichtigen.

## <a name="span-idcompressionalgorithmsspanspan-idcompressionalgorithmsspanspan-idcompressionalgorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>Komprimierungsalgorithmen

Blockkomprimierungstechniken in Direct3D teilen nicht komprimierte Texturdaten in 4 × 4 Blöcke auf, komprimieren jeden Block und speichern anschließend die Daten. Aus diesem Grund müssen Texturen, die komprimiert werden, Texturdimensionen aufweisen, die eine Potenz von 4 sind.

![Blockkomprimierung](images/d3d10-compression-1.png)

Die obige Abbildung zeigt eine in Texel-Blöcke aufgeteilte Textur. Der erste Block zeigt das Layout der 16 Texel mit der Bezeichnung „a-p”, doch jeder Block verfügt über die gleiche Organisation der Daten.

Direct3D implementiert mehrere Komprimierungsmethoden. Jede Methode implementiert einen unterschiedlichen Kompromiss zwischen der Anzahl an gespeicherten Komponenten und der Anzahl an Bit pro Komponente und dem erforderlichen Arbeitsspeicher. Verwenden Sie die Tabelle zur Auswahl des Formats, das am besten mit der Art von Daten und der Datenauflösung funktioniert, die Ihrer Anwendung entspricht.

| Datenquelle                     | Auflösung der Datenkomprimierung (in Bit) | Wählen Sie dieses Komprimierungsformat |
|---------------------------------|---------------------------------------|--------------------------------|
| Farb- und Alphawert mit drei Komponenten | Farbe (5:6:5), Alpha (1) oder kein Alpha  | [BC1](#bc1)                    |
| Farb- und Alphawert mit drei Komponenten | Farbe (5:6:5), Alpha (4)              | [BC2](#bc2)                    |
| Farb- und Alphawert mit drei Komponenten | Farbe (5:6:5), Alpha (8)              | [BC3](#bc3)                    |
| Farbe mit einer Komponente             | Eine Komponente (8)                     | [BC4](#bc4)                    |
| Farbe mit zwei Komponenten             | Zwei Komponenten (8:8)                  | [BC5](#bc5)                    |

- [BC1](#bc1)
- [BC2](#bc2)
- [BC3](#bc3)
- [BC4](#bc4)
- [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

Der erste Block-Komprimierungsformat (BC1) verwenden (entweder DXGI\_FORMAT\_BC1\_TYPELESS, DXGI\_FORMAT\_BC1\_UNORM oder DXGI\_BC1\_UNORM \_SRGB) zum Speichern von drei Komponenten Farbdaten mit einem 5:6:5-Farbe (5 Bits, die roten, grünen 6 Bits, 5 Bits Blau). Dies gilt auch, wenn die Daten ebenfalls 1 Bit Alpha enthalten. Bei einer 4 × 4-Textur mit dem größtmöglichen Datenformat reduziert das Format BC1 den erforderlichen Speicherplatz von 48 Byte (16 Farben × 3-Komponenten/Farben × 1 Byte/Komponente) auf 8 Byte Arbeitsspeicher.

Der Algorithmus funktioniert auf 4 × 4-Texelblöcken. Anstatt zu speichern, 16 Farben, speichert der Algorithmus 2 Verweis Farben (Farbe\_0 und Farbe\_(1) und 16 2-Bit-Farben-Indizes (Blöcke a – p), wie im folgenden Diagramm dargestellt.

![das Layout für eine BC1-Komprimierung](images/d3d10-compression-bc1.png)

Die Farbindizes (a-p) werden verwendet, um die ursprünglichen Farben aus einer Farbtabelle anzuzeigen. Die Farbtabelle enthält 4 Farben. Die ersten beiden Farben: Farbe\_0 und Farbe\_1 – sind die minimalen und maximalen Farben. Die anderen beiden Farben, Farbe\_2 und Farbe\_3, sind fortgeschrittene Farben, die bei der linearen Interpolation berechnet.

```cpp
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

Den vier Farben werden 2-Bit-Indexwerte zugewiesen, die in den Blöcke a-p gespeichert werden.

```cpp
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

Anschließend werden die Farben in den Blöcken a-p mit den vier Farben der Tabelle verglichen und der Index für die ähnlichste Farbe wird in den 2-Bit-Blöcken gespeichert.

Dieser Algorithmus eignet sich für ebenfalls für Daten, die 1 Bit Alpha enthalten. Der einzige Unterschied ist die entsprechende Farbe\_3 wird auf 0 festgelegt (womit eine transparente Farbe) sowie Farbe\_2 ist eine lineare Mischung der Farbe\_0 und Farbe\_1.

```cpp
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

Verwenden Sie das BC2-Format (entweder DXGI\_FORMAT\_BC2\_TYPELESS, DXGI\_FORMAT\_BC2\_UNORM oder DXGI\_BC2\_UNORM\_SRGB), Speichern von Daten, die Farb- und Alphawerte Daten mit niedriger Kohärenz enthält (verwenden Sie [BC3](#bc3) für hoch kohärente Alphadaten). Das BC2-Format speichert RGB-Daten als eine „5:6:5” Farbe (5 Bit Rot, 6 Bit Grün, 5 Bit Blau) und Alpha als einen separaten 4-Bit-Wert. Bei einer 4 × 4-Textur mit dem größtmöglichen Datenformat reduziert diese Komprimierungstechnik den erforderlichen Speicherplatz von 64 Byte (16 Farben × 4-Komponenten/Farben × 1 Byte/Komponente) auf 16 Byte Arbeitsspeicher.

Das BC2-Format speichert Farben mit der gleichen Anzahl von Bit und Daten-Layout wie das [BC1](#bc1)-Format. BC2 erfordert allerdings 64-Bit zusätzlichen Speicher zum Speichern der Alpha-Daten, wie im folgenden Diagramm dargestellt.

![das Layout für eine BC2-Komprimierung](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

Verwenden Sie das BC3-Format (entweder DXGI\_FORMAT\_BC3\_TYPELESS, DXGI\_FORMAT\_BC3\_UNORM oder DXGI\_BC3\_UNORM\_SRGB), hoch kohärente Farbdaten speichern (verwenden Sie [BC2](#bc2) mit weniger kohärente Alphadaten). Die BC3-Format speichert Farbdaten mit „5:6:5” Farbe ( Bit Rot, 6 Bit Grün, 5 Bit Blau) und Alpha-Daten mit einem Byte. Bei einer 4 × 4-Textur mit dem größtmöglichen Datenformat reduziert diese Komprimierungstechnik den erforderlichen Speicherplatz von 64 Byte (16 Farben × 4-Komponenten/Farben × 1 Byte/Komponente) auf 16 Byte Arbeitsspeicher.

Die BC3-Format speichert Farben mit der gleichen Anzahl von Bit und Daten-Layout wie das [BC1](#bc1)-Format. BC3 erfordert allerdings 64-Bit zusätzlichen Speicher zum Speichern der Alpha-Daten, wie im folgenden Diagramm dargestellt. Das BC3-Format verarbeitet Alpha, indem es zwei Referenzdaten speichert und die Werte zwischen ihnen interpoliert (ähnlich wie BC1 RGB-Farbe speichert).

Der Algorithmus funktioniert auf 4 × 4-Texelblöcken. Anstelle von 16 alpha-Werte zu speichern, speichert der Algorithmus 2 Verweis Alphas (Alpha\_0 und Alpha\_1) und 16 3-Bit-Farben-Indizes (Alpha über p), wie im folgenden Diagramm dargestellt.

![das Layout für eine BC3-Komprimierung](images/d3d10-compression-bc3.png)

Das BC3-Format verwendet die Alpha-Indizes (a-p), um die ursprünglichen Farben in einer Suchtabelle zu finden, die 8 Werte enthält. Die ersten beiden Werte – Alpha\_0 und Alpha\_1 – sind die minimalen und maximalen Werte; die anderen sechs Zwischenwerte werden mithilfe von linearer Interpolation berechnet.

Der Algorithmus bestimmt die Anzahl der interpolierten Alphawerte anhand von zwei Alpha-Referenzen. Wenn alpha\_0 ist größer als Alpha\_1 und BC3 interpoliert 6 alpha-Werte; anderenfalls es 4 interpoliert. Wenn BC3 nur 4 Alphawerte interpoliert, werden zwei zusätzliche Alphawerte festgelegt (0 für vollständig transparent und 255 für vollständig undurchsichtig). BC3 komprimiert die Alphawerte in 4 × 4-Texel-Bereiche, indem der Bit-Code gespeichert wird, der dem interpolierten Alpha-Wert entspricht, der dem ursprünglichen Alpha-Wert für ein bestimmtes Texel am ehesten entspricht.

```cpp
if( alpha_0 > alpha_1 )
{
  // 6 interpolated alpha values.
  alpha_2 = 6/7*alpha_0 + 1/7*alpha_1; // bit code 010
  alpha_3 = 5/7*alpha_0 + 2/7*alpha_1; // bit code 011
  alpha_4 = 4/7*alpha_0 + 3/7*alpha_1; // bit code 100
  alpha_5 = 3/7*alpha_0 + 4/7*alpha_1; // bit code 101
  alpha_6 = 2/7*alpha_0 + 5/7*alpha_1; // bit code 110
  alpha_7 = 1/7*alpha_0 + 6/7*alpha_1; // bit code 111
}
else
{
  // 4 interpolated alpha values.
  alpha_2 = 4/5*alpha_0 + 1/5*alpha_1; // bit code 010
  alpha_3 = 3/5*alpha_0 + 2/5*alpha_1; // bit code 011
  alpha_4 = 2/5*alpha_0 + 3/5*alpha_1; // bit code 100
  alpha_5 = 1/5*alpha_0 + 4/5*alpha_1; // bit code 101
  alpha_6 = 0;                         // bit code 110
  alpha_7 = 255;                       // bit code 111
}
```

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>BC4

Verwenden Sie das BC4-Format, um Einzelkomponent-Farbdaten mit 8 Bit pro Farbe zu speichern. Aufgrund der erhöhten Genauigkeit (im Vergleich zu [BC1](#bc1)), BC4 eignet sich ideal zum Speichern von Gleitkommazahlen in den Bereich der \[0 bis 1\] mithilfe der DXGI\_FORMAT\_BC4\_UNORM-Format und \[-1 bis + 1\] mithilfe der DXGI\_FORMAT\_BC4\_SNORM-Format. Bei einer 4 × 4-Textur mit dem größtmöglichen Datenformat reduziert diese Komprimierungstechnik den erforderlichen Speicherplatz von 16 Byte (16 Farben × 1-Komponenten/Farben × 1 Byte/Komponente) auf 8 Byte.

Der Algorithmus funktioniert auf 4 × 4-Texelblöcken. Anstatt zu speichern, 16 Farben, speichert der Algorithmus 2 Verweis Farben (rote\_0 und Rot\_1) und 16 3-Bit-Farben-Indizes (rote durch rote p), wie im folgenden Diagramm dargestellt.

![das Layout für eine BC4-Komprimierung](images/d3d10-compression-bc4.png)

Der Algorithmus verwendet die 3-Bit-Indizes um Farben aus einer Farbtabelle mit 8 Farben herauszufinden. Die ersten beiden Farben – Rot\_0 und Rot\_1 – sind die minimalen und maximalen Farben. Der Algorithmus berechnet die restlichen Farben mithilfe der linearen Interpolation.

Der Algorithmus bestimmt die Anzahl der interpolierten Farbwerte anhand von zwei Referenzwerten. Wenn Rot\_0 ist größer als Rot\_1, und klicken Sie dann auf BC4 interpoliert 6 Farbwerte; anderenfalls es 4 interpoliert. Wenn BC4 nur 4 Farbwerte interpoliert, werden zwei zusätzliche Farbwerte festgelegt (0,0f ist vollständig transparent und 1,0f ist vollständig undurchsichtig). BC4 komprimiert die Alphawerte in 4 × 4-Texel-Bereiche, indem der Bit-Code gespeichert wird, der dem interpolierten Alpha-Wert entspricht, der mit dem ursprünglichen Alpha-Wert für ein bestimmtes Texel am ehesten übereinstimmt.

- [BC4\_UNORM](#bc4-unorm)
- [BC4\_SNORM](#bc4-snorm)

### <a name="span-idbc4unormspanspan-idbc4unormspanspan-idbc4-unormspanbc4unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>BC4\_UNORM

Die Interpolation bei Daten mit einer einzelnen Komponente erfolgt wie im folgenden Codebeispiel dargestellt.

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

Den Referenzfarben sind 3-Bit-Indizes zugewiesen (000 – 111, da 8 Werte vorhanden sind), die während der Komprimierung in Blöcken von „Rot a” bis „Rot p” gespeichert werden.

### <a name="span-idbc4snormspanspan-idbc4snormspanspan-idbc4-snormspanbc4snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>BC4\_SNORM

Das DXGI\_FORMAT\_BC4\_SNORM ist identisch, mit dem Unterschied, dass die Daten im Bereich der SNORM codiert ist und wenn eine 4-Farbwerte werden interpoliert. Die Interpolation bei Daten mit einer einzelnen Komponente erfolgt wie im folgenden Codebeispiel dargestellt.

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

Den Referenzfarben sind 3-Bit-Indizes zugewiesen (000 – 111, da 8 Werte vorhanden sind), die während der Komprimierung in Blöcken von „Rot a” bis „Rot p” gespeichert werden.

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

Verwenden Sie das BC5-Format, um Zweikomponent-Farbdaten mit 8 Bit pro Farbe zu speichern. Aufgrund der erhöhten Genauigkeit (im Vergleich zu [BC1](#bc1)), BC5 eignet sich ideal zum Speichern von Gleitkommazahlen in den Bereich der \[0 bis 1\] mithilfe der DXGI\_FORMAT\_BC5\_UNORM-Format und \[-1 bis + 1\] mithilfe der DXGI\_FORMAT\_BC5\_SNORM-Format. Bei einer 4 × 4-Textur mit dem größtmöglichen Datenformat reduziert diese Komprimierungstechnik den erforderlichen Speicherplatz von 32 Byte (16 Farben × 2-Komponenten/Farben × 1 Byte/Komponente) auf 16 Byte.

- [BC5\_UNORM](#bc5-unorm)
- [BC5\_SNORM](#bc5-snorm)

Der Algorithmus funktioniert auf 4 × 4-Texelblöcken. Anstatt zu speichern, 16 Farben für beide Komponenten, speichert der Algorithmus 2 Verweis Farben für jede Komponente (rote\_0, red\_1, Grün\_0 und grünen\_1) und 16 3-Bit-Farben-Indizes für jede Komponente (rote ein durch rote p und grünen über grüne p), wie im folgenden Diagramm dargestellt.

![das Layout für eine BC5-Komprimierung](images/d3d10-compression-bc5.png)

Der Algorithmus verwendet die 3-Bit-Indizes um Farben aus einer Farbtabelle mit 8 Farben herauszufinden. Die ersten beiden Farben – Rot\_0 und Rot\_1 (oder grüner\_0 und grünen\_1), sind die minimalen und maximalen Farben. Der Algorithmus berechnet die restlichen Farben mithilfe der linearen Interpolation.

Der Algorithmus bestimmt die Anzahl der interpolierten Farbwerte anhand von zwei Referenzwerten. Wenn Rot\_0 ist größer als Rot\_1, und klicken Sie dann auf BC5 interpoliert 6 Farbwerte; anderenfalls es 4 interpoliert. Wenn BC5 nur 4 Farbwerte interpoliert, legt es die verbleibenden zwei Farbwerte auf 0,0f und 1,0f fest.

### <a name="span-idbc5unormspanspan-idbc5unormspanspan-idbc5-unormspanbc5unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5\_UNORM

Die Interpolation bei Daten mit einer einzelnen Komponente erfolgt wie im folgenden Codebeispiel dargestellt. Die Berechnungen für die grünen Komponenten sind ähnlich.

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

Den Referenzfarben sind 3-Bit-Indizes zugewiesen (000 – 111, da 8 Werte vorhanden sind), die während der Komprimierung in Blöcken von „Rot a” bis „Rot p” gespeichert werden.

### <a name="span-idbc5snormspanspan-idbc5snormspanspan-idbc5-snormspanbc5snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5\_SNORM

Das DXGI\_FORMAT\_BC5\_SNORM ist identisch, außer dass die Daten im Bereich der SNORM codiert ist und wenn 4 Datenwerte interpoliert werden, die zwei zusätzlichen Werte - 1. 0f und 1. 0f sind. Die Interpolation bei Daten mit einer einzelnen Komponente erfolgt wie im folgenden Codebeispiel dargestellt. Die Berechnungen für die grünen Komponenten sind ähnlich.

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

Den Referenzfarben sind 3-Bit-Indizes zugewiesen (000 – 111, da 8 Werte vorhanden sind), die während der Komprimierung in Blöcken von „Rot a” bis „Rot p” gespeichert werden.

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>Formatkonvertierung

Direct3D ermöglicht Kopien zwischen vorstrukturierten, typisierten Texturen und blockkomprimierten Texturen für die gleiche Bit-Breite.

Sie können Ressourcen zwischen verschiedenen Formattypen kopieren. Diese Art von Kopiervorgang führt eine Formatkonvertierung durch, die Ressourcendaten als einen neuen Formattyp neu interpretiert. Betrachten Sie in diesem Beispiel den Unterschied zwischen neu interpretierten Daten mit der Funktionsweise einer typischen Art der Konvertierung:

```cpp
FLOAT32 f = 1.0f;
UINT32 u;
```

Um „f” als Typ „u” neu zu interpretieren, verwenden Sie [memcpy](https://docs.microsoft.com/cpp/c-runtime-library/reference/memcpy-wmemcpy):

```cpp
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

In der vorherigen neuen Interpretation ändert sich der zugrunde liegende Wert der Daten nicht. [memcpy](https://docs.microsoft.com/cpp/c-runtime-library/reference/memcpy-wmemcpy) interpretiert den Float-Wert als Ganzzahl ohne Vorzeichen.

Verwenden Sie zum Ausführen der typischen Art der Konvertierung foglende Zuweisung:

```cpp
u = f; // 'u' becomes 1.
```

In der vorherigen Konvertierung ändert sich der zugrunde liegende Wert der Daten.

Die folgende Tabelle enthält die zulässigen Quell- und Zielformate, die Sie in dieser neu interpretierten Art der Formatkonvertierung verwenden können. Sie müssen die Werte ordnungsgemäß codieren, damit die Neuinterpretation wie gewünscht ausgeführt werden kann.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Bit-Breite</th>
<th align="left">Nicht komprimierte Ressource</th>
<th align="left">Blockkomprimierte Ressource</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">32</td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p>
<p>DXGI_FORMAT_R32_SINT</p></td>
<td align="left">DXGI_FORMAT_R9G9B9E5_SHAREDEXP</td>
</tr>
<tr class="even">
<td align="left">64</td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UINT</p>
<p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<p>DXGI_FORMAT_R32G32_UINT</p>
<p>DXGI_FORMAT_R32G32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen

[Komprimierte des texturressourcen](compressed-texture-resources.md)
