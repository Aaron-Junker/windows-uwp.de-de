---
title: Konvertieren von Datentypen
description: In den folgenden Abschnitten wird beschrieben, wie Direct3D Konvertierungen zwischen Datentypen behandelt.
ms.assetid: B50AB8DE-CAED-465B-B18C-81F3A984B8AC
keywords:
- Konvertieren von Datentypen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 08c6dda8759a6e1452daf7cf0a3cd3e5db9ea1e6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590575"
---
# <a name="data-type-conversion"></a>Konvertieren von Datentypen


In den folgenden Abschnitten wird beschrieben, wie Direct3D Konvertierungen zwischen Datentypen behandelt.

## <a name="span-iddatatypeterminologyspanspan-iddatatypeterminologyspanspan-iddatatypeterminologyspandata-type-terminology"></a><span id="Data_Type_Terminology"></span><span id="data_type_terminology"></span><span id="DATA_TYPE_TERMINOLOGY"></span>Data-Type-Terminologie


Der folgende Begriffe werden nachfolgend zur Beschreibung der verschiedenen Formatkonvertierungen verwendet.

| Begriff  | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SNORM | Normalisierter Ganzzahlwert mit Vorzeichen (Signed Normalized Integer). Bei einem n-Bit-Vielfachen von 2 liegt die höchste Zahl bei 1,0f (der 5-Bit-Wert 01111 entspricht z. B. 1,0f), und der kleinste Wert liegt bei -1,0f (der 5-Bit-Wert 10000 entspricht z. B. -1,0f). Des Weiteren entspricht der zweitkleinste Wert -1,0f (der 5-Bit-Wert 10001 entspricht z. B. -1, 0f). Aus diesem Grund gibt es zwei Ganzahldarstellungen für -1,0f. Für 0,0f und 1,0f gibt es nur eine einzige Darstellung. Dadurch ergibt sich eine Ganzzahldarstellungen für gleichmäßig verteilte Gleitkommawerte im Bereich (-1,0f ... 0,0f), und eine zweite Darstellungen für Zahlen im Bereich (0,0f ... 1,0f). |
| UNORM | Normalisierte Ganzzahl ohne Vorzeichen (Unsigned Normalized Integer). Bei einem n-Bit-Wert stellt der Wert mit allen Bits auf 0 die Zahl 0,0f und der Wert mit allen Bits auf 1 die Zahl 1,0f dar. Es wird eine Folge von gleichmäßig verteilen Gleitkommawerten zwischen 0,0f und 1,0f dargestellt. Ein 2-Bit-UNORM-Wert stellt z. B. 0,0f, 1/3, 2/3 und 1,0f dar.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| SINT  | Ganzzahl mit Vorzeichen (Signed integer). Ganzzahl mit dem Vielfachen von 2. Ein 3-Bit-SINT-Wert stellt z. B. die Ganzzahlen -4, -3, -2, -1, 0, 1, 2 und 3 dar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| UINT  | Ganzzahl ohne Vorzeichen (Unsigned Integer). Ein 3-Bit-UINT-Wert stellt z. B. die Ganzzahlen 0, 1, 2, 3, 4, 5, 6 und 7 dar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| FLOAT | Ein Gleitkommawert in einer von Direct3D definierten Darstellung.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SRGB  | Ähnlich wie bei UNORM stehen hier alle Bits auf 0 für 0,0f und alle Bits auf 1 für 1,0f. Im Gegensatz zu UNORM stellen bei SRGB Ganzzahlen-Bitfolgen zwischen nur 0 und nur 1 einen nicht linearen Verlauf von Gleitkommazahlen zwischen 0,0f und 1,0f dar. Als Farbwerte dargestellt würde dieser nichtlineare Verlauf bei SRGB für einen "durchschnittlichen" Betrachter unter "durchschnittlichen" Betrachtungsbedingungen auf einem "durchschnittlichen" Display als linearer Verlauf von Helligkeitsstufen erscheinen. Ausführliche Informationen finden Sie im SRGB-Standard IEC 61996-2-1 der IEC (International Electrotechnical Commission).                |

 

Die oben genannten Begriffe werden häufig als "Format Name Modifiers" verwendet. Sie beschreiben in diesem Zusammenhang, wie Daten im Speicher angeordnet sind und welche Konvertierung beim Transportpfad (u. U. einschließlich Filterung) aus dem Speicher zu einer Pipelineeinheit wie z. B. einem Shader ausgeführt werden müssen.

## <a name="span-idfloatingpointconversionspanspan-idfloatingpointconversionspanspan-idfloatingpointconversionspanfloating-point-conversion"></a><span id="Floating_Point_Conversion"></span><span id="floating_point_conversion"></span><span id="FLOATING_POINT_CONVERSION"></span>Floating-Point-Konvertierung


Wenn eine Gleitkomma-Konvertierung zwischen verschiedenen Darstellungen auftritt (inkl. der Konvertierung von einer Nicht-Gleitkommadarstellung) gelten folgende Regeln.

### <a name="span-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanconververting-from-a-higher-range-representation-to-a-lower-range-representation"></a><span id="Conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="CONVERVERTING_FROM_A_HIGHER_RANGE_REPRESENTATION_TO_A_LOWER_RANGE_REPRESENTATION"></span>Conververting aus einer höheren Bereich Darstellung mit Ihrer Darstellung in einem niedrigeren Bereich

-   Führen der Konvertierung zu einem anderen Gleitkommaformat wird auf Null gerundet. Wenn das Ziel ein Ganzzahl- oder eine Festkommaformat ist, wird mathematisch gerundet (round-to-nearest-even, Runden auf den nächsten geraden Wert). Es sei denn, die Beschreibung der Konvertierung gibt explizit ein anderes Rundungsverhalten an (z. B. das kaufmännische Runden (round-to-nearest) bei FLOAT zu SNORM, FLOAT zu UNORM oder FLOAT zu SRGB). Andere Ausnahmen sind die ftoi- und ftou-Shader-Anweisungen, die mit der Rundung auf Null arbeiten (round-to-zero). Die vom Textur-Sampler und Rasterizer verwendeten Konvertierungen von Gleitkommazahl- zu Festkommazahlen arbeiten mit einem festen Stellenwert der letzten Ziffer (Unit-Last-Place).
-   Quellwerte, die größer als der dynamische Bereich des unteren Bereichs des Zielformats sind (z. B. ein großer 32-Bit-Float-Wert, der in ein 16-Bit-Float-RenderTarget geschrieben wird), liegt das Ergebnisse bei dem größten darstellbaren Wert (mit passendem Vorzeichen). Er umfasst NICHT den vorzeichenbehafteten Unendlich-Wert (aufgrund der oben beschriebenen Rundung auf Null).
-   NaN in einem Format für einen größeren Bereich wird zur NaN Darstellung im Format für den kleineren Bereich konvertiert, wenn die NaN Darstellung im Format für den kleineren Bereich vorhanden ist. Verfügt das Format für den niedrigeren Bereich über keine NaN-Darstellung, ist das Ergebnis 0.
-   INF in einem Format für einen größeren Bereich wird, wenn möglich, zu INF im Format für den kleineren Bereich konvertiert. Wenn das Format für den niedrigeren Bereich über keine INF-Darstellung verfügt, wird der Wert in den maximal darstellbaren Wert konvertiert. Wenn im Zielformat vorhanden, wird das Vorzeichen beibehalten.
-   Denorm in einem Format für einen größeren Bereich wird in die Denorm Darstellung des Formats für den kleineren Bereich konvertiert (wenn der Wert im Format für den kleineren Bereich verfügbar ist und die Konvertierung möglich ist – ansonsten lautet das Ergebnis 0). Wenn im Zielformat vorhanden, wird das Vorzeichen-Bit beibehalten.

### <a name="span-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanconverting-from-a-lower-range-representation-to-a-higher-range-representation"></a><span id="Converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="CONVERTING_FROM_A_LOWER_RANGE_REPRESENTATION_TO_A_HIGHER_RANGE_REPRESENTATION"></span>Konvertieren von einer niedrigeren Bereich Darstellung mit Ihrer Darstellung in einem höheren Bereich

-   NaN in einem Format für einen kleineren Bereich wird zur NaN Darstellung im Format für den größeren Bereich konvertiert, wenn der Wert im Format für den größeren Bereich vorhanden ist. Wenn das Format für den größeren Bereich keine NaN-Darstellung hat, wird der Wert in 0 konvertiert.
-   INFO in einem Format für einen kleineren Bereich wird zur INF-Darstellung im Format für den größeren Bereich konvertiert, wenn der Wert im Format für den größeren Bereich vorhanden ist. Wenn das höhere Format nicht über eine INF-Darstellung verfügt, erfolgt eine Konvertierung auf den maximalen Wert dargestellt werden kann (max.\_"float" in diesem Format). Wenn im Zielformat vorhanden, wird das Vorzeichen beibehalten.
-   Denorm in einem Format für einen kleineren Bereich wird, wenn möglich, in die normalisierte Darstellung des Formats für den größeren Bereich konvertiert. Andernfalls wird zu einer Denorm-Darstellung im Format für den größeren Bereich konvertiert, solange die Denorm-Darstellung vorhanden ist. Ist dies nicht möglich, da das Format für den größeren Bereich keine Denorm-Darstellung hat, wird zu 0 konvertiert. Wenn im Zielformat vorhanden, wird das Vorzeichen beibehalten. Beachten Sie, dass die 32-Bit-Gleitkommawerten als Format ohne eine Denorm-Darstellung zählen (da Denorms bei Operationen für 32-Bit-Gleitkommawerten auf eine vorzeichenbehaftete 0 gesetzt werden).

## <a name="span-idintegerconversionspanspan-idintegerconversionspanspan-idintegerconversionspaninteger-conversion"></a><span id="Integer_Conversion"></span><span id="integer_conversion"></span><span id="INTEGER_CONVERSION"></span>Ganzzahlige Konvertierung


Die folgende Tabelle beschreibt die Konvertierung von verschiedenen oben beschriebenen Darstellungen zu anderen Darstellungen. Nur Konvertierungen, die tatsächlich in Direct3D auftreten, werden aufgeführt.

Sofern nicht anders angegeben, werden bei Ganzzahlen alle Konvertierungen zu und aus Ganzzahl-Darstellungen zu den unten beschriebenen Gleitkomma-Darstellung exakt durchgeführt.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Quelldatentyp</th>
<th align="left">Zieldatentyp</th>
<th align="left">Konvertierungsregel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">SNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>Bei einem n-Bit-Ganzzahlwert für einen Bereich mit Vorzeichen [-1,0f, 1,0f] wird die Konvertierung in Gleitkommazahlen wie folgt durchgeführt.</p>
<ul>
<li>Der kleinste Wert entspricht -1,0f. (der 5-Bit-Wert 10000 entspricht z. B. -1,0f).</li>
<li>Alle anderen Werte werden in einen Gleitkommawert konvertiert (c). Das Ergebnis lautet = c * (1,0f / (2⁽ⁿ⁻¹⁾-1)). Der 5-Bit-Wert 10001 wird Z. B. zu -15,0f konvertiert, und dann durch 15,0f geteilt. Dies bringt als Ergebnis -1,0f.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SNORM</td>
<td align="left"><p>Bei einer Gleitkommazahl findet die Konvertierung zu einem n-Bit-Ganzzahlwert für einen Bereich mit Vorzeichen [-1,0f, 1,0f] wie folgt statt.</p>
<ul>
<li>Der Startwert ist c.</li>
<li>Wenn c ein NaN ist, lautet das Ergebnis 0.</li>
<li>Wenn c &gt; 1,0f ist (einschließlich INF), wird es das Ergebnis auf 1,0f gesetzt.</li>
<li>Wenn c &lt; -1,0f ist (einschließlich -INF), wird es das Ergebnis auf -1,0f gesetzt.</li>
<li>Konvertieren von Gleitkomma-Skalierung zu Ganzzahl-Skalierung: c = c * (2ⁿ⁻¹-1).</li>
<li>Die Konvertieren zu einem Ganzzahlwert erfolgt folgendermaßen:
<ul>
<li>Wenn c &gt;= 0 dann c = c + 0,5f, andernfalls, c = c - 0,5f.</li>
<li>Der dezimalen Nachkommastellen werden verworfen. Die verbleibenden (ganzzahlige) Wert wird direkt in einen Ganzzahlwert konvertiert.</li>
</ul></li>
</ul>
<p>Diese Konvertierung lässt eine Toleranz von D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_Unit-Last-Place Unit-Last-Place (für die Ganzzahl) zu. Das bedeutet, dass nach der Konvertierung eines Gleitkommawertes in einen Ganzzahlwert jeder Wert innerhalb von D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place eines darstellbaren Zielformatwertes für den Wert gültig ist. Die zusätzliche Dateninvertierbarkeitsanforderung stellt sicher, dass die Konvertierung nicht über den Bereich abnehmend ist und alle Ausgabewerte abdeckbar sind. (In den hier aufgeführten Konstanten müssen die Stellen <em>xx</em> durch die Direct3D-Version ersetzt werden – beispielsweise 10, 11 oder 12.)</p></td>
</tr>
<tr class="odd">
<td align="left">UNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>Der n-Bit-Startwert wird zu einer Gleitkommazahl konvertiert (0,0f, 1,0f, 2,0f etc.) und dann durch (2ⁿ-1) geteilt.</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">UNORM</td>
<td align="left"><p>Der Startwert ist c.</p>
<ul>
<li>Wenn c ein NaN ist, lautet das Ergebnis 0.</li>
<li>Wenn c &gt; 1,0f ist (einschließlich INF), wird es das Ergebnis auf 1,0f gesetzt.</li>
<li>Wenn c &lt; 0,0f ist (einschließlich -INF) wird das Ergebnis auf 0,0f gesetzt.</li>
<li>Konvertieren von Gleitkomma-Skalierung zu Ganzzahl-Skalierung: c = c * (2ⁿ-1).</li>
<li>Konvertieren zu Ganzzahl
<ul>
<li>c = c + 0,5f.</li>
<li>Der dezimalen Nachkommastellen werden verworfen. Die verbleibenden (ganzzahlige) Wert wird direkt in einen Ganzzahlwert konvertiert.</li>
</ul></li>
</ul>
<p>Diese Konvertierung lässt eine Toleranz von D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place (für die Ganzzahl) zu. Das bedeutet, dass nach der Konvertierung eines Gleitkommawertes in einen Ganzzahlwert jeder Wert innerhalb von D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place eines darstellbaren Zielformatwertes für den Wert gültig ist. Die zusätzliche Dateninvertierbarkeitsanforderung stellt sicher, dass die Konvertierung nicht über den Bereich abnehmend ist und alle Ausgabewerte abdeckbar sind.</p></td>
</tr>
<tr class="odd">
<td align="left">SRGB</td>
<td align="left">FLOAT</td>
<td align="left"><p>Die folgende Konvertierung ist die ideale SRGB zu FLOAT Konvertierung.</p>
<ul>
<li>Der n-Bit-Startwert wird zu einer Gleitkommazahl konvertiert (0,0f, 1,0f, 2,0f etc.); das Ergebnis ist c.</li>
<li>c = c * (1,0f / (2ⁿ-1))</li>
<li>Wenn (c &lt; = D3D<em>xx</em>_SRGB_TO_FLOAT_THRESHOLD) dann: Ergebnis = c / D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_1, andernfalls: Ergebnis = ((c + D3D<em>xx</em>_SRGB_TO_FLOAT_OFFSET)/D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_2)D3D<em>xx</em>_SRGB_TO_FLOAT_EXPONENT</li>
</ul>
<p>Diese Konvertierung lässt eine Toleranz von D3D<em>xx</em>_SRGB_TO_FLOAT_TOLERANCE_IN_ULP Unit-Last-Place (für SRGB) zu.</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SRGB</td>
<td align="left"><p>Die folgende Konvertierung ist die ideale FLOAT -&gt; SRGB Konvertierung.</p>
<p>Die SRGB-Zielfarbkomponente hat n Bits:</p>
<ul>
<li>Der Startwert ist c.</li>
<li>Wenn c ein NaN ist, lautet das Ergebnis 0.</li>
<li>Wenn c &gt; 1,0f ist (einschließlich INF), wird es das Ergebnis auf 1,0f gesetzt.</li>
<li>Wenn c &lt; 0,0f ist (einschließlich -INF) wird das Ergebnis auf 0,0f gesetzt.</li>
<li>Wenn (c &lt;= D3D<em>xx</em>_FLOAT_TO_SRGB_THRESHOLD) dann: c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_1 * c, ansonsten: c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_2 * c(D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_NUMERATOR/D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_DENOMINATOR) - D3D<em>xx</em>_FLOAT_TO_SRGB_OFFSET</li>
<li>Konvertieren von Gleitkomma-Skalierung zu Ganzzahl-Skalierung: c = c * (2ⁿ-1).</li>
<li>Konvertieren zu Ganzzahl:
<ul>
<li>c = c + 0,5f.</li>
<li>Der dezimalen Nachkommastellen werden verworfen. Die verbleibenden (ganzzahlige) Wert wird direkt in einen Ganzzahlwert konvertiert.</li>
</ul></li>
</ul>
<p>Diese Konvertierung lässt eine Toleranz von D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place (für die Ganzzahl) zu. Das bedeutet, dass nach der Konvertierung eines Gleitkommawertes in einen Ganzzahlwert jeder Wert innerhalb von D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place eines darstellbaren Zielformatwertes für den Wert gültig ist. Die zusätzliche Dateninvertierbarkeitsanforderung stellt sicher, dass die Konvertierung nicht über den Bereich abnehmend ist und alle Ausgabewerte abdeckbar sind.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">SINT mit mehr Bits</td>
<td align="left"><p>Zum Konvertieren von SINT zu SINT mit mehr Bits wird das hochwertigste Bit (Most Significant Bit, MSB) der Startzahl unter &quot;Beibehaltung des Vorzeichens&quot; auf die Anzahl Bits im Zielformat erweitert.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">SINT mit mehr Bits</td>
<td align="left"><p>Zum Konvertieren von UINT zu SINT mit mehr Bits wird die Zahl in die Bits mit der geringsten Wertigkeit (Least Significant Bits, LSBs) kopiert und die zusätzlichen Bits mit höchster Wertigkeit werden mit 0 aufgefüllt.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">UINT mit mehr Bits</td>
<td align="left"><p>SINT in UINT mit mehr Bits konvertiert: Wenn Sie negativ ist, wird der Wert auf 0 festgelegt. Andernfalls wird die Zahl in die hochwertigsten Bits des Zielformates kopiert und die zusätzlichen Bits mit geringer Wertigkeit werden mit 0 aufgefüllt.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">UINT mit mehr Bits</td>
<td align="left"><p>Zum Konvertieren von UINT zu UINT mit mehr Bits wird die Zahl in die hochwertigsten Bits des Zielformats kopiert. Die Bits mit niedriger Wertigkeit werden mit 0 aufgefüllt.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT oder UINT</td>
<td align="left">SINT oder UNIT mit weniger oder gleich vielen Bits</td>
<td align="left"><p>Zum Konvertieren von SINT oder UINT zu SINT oder UNIT mit weniger oder gleich vielen Bits (und/oder Vorzeichenänderung) wird der Startwert einfach auf die Bereich des Zielformates beschränkt.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanfixed-point-integer-conversion"></a><span id="Fixed_Point_Integer_Conversion"></span><span id="fixed_point_integer_conversion"></span><span id="FIXED_POINT_INTEGER_CONVERSION"></span>Festen Punkt ganzzahlige Konvertierung


Festkomma-Ganzzahlwerte sind einfach nur Ganzzahlwerte einer bestimmten Bitlänge, die an einem festen Punkt ein Komma haben.

Der häufigste "Integer"-Datentyp ist ein Sonderfall eines Festkomma-Ganzzahlwerts mit einem Dezimalkomma am Ende der Zahl.

Ganzzahl-Festkomma-Darstellungen haben die Form i,f (wobei i die Anzahl der ganzzahligen Bits und f die Anzahl der Nachkommastellenbits ist). 16,8 bedeutet beispielsweise, dass 16-Bit für die Ganzzahl und 8 Bits für die Nachkommastellen verwendet werden. Der ganzzahlige Teil wird, zumindest in der hier verwendeten Definition, in als Vielfaches von 2 gespeichert (kann jedoch genauso für Ganzzahlen ohne Vorzeichen definiert werden). Die Nachkommastellen werden ohne Vorzeichen gespeichert. Der Nachkommastellen-Teil ist immer der positive Teil zwischen den zwei direkt benachbarten Ganzzahlwerten ab dem kleineren Wert.

Additionen und Subtraktionen mit Ganzzahl-Festkommawerten werden einfach über die normale Ganzzahlarithmetik und ohne Berücksichtigung der Position der Kommastelle durchgeführt. Das Addieren von 1 zur einem 16,8 Ganzzahl-Festkommawert ist gleichbedeutend mit dem Addieren von 256. Denn die Dezimalstelle befindet sich 8 Positionen vom Bit mit der geringsten entfernt. Andere Operationen (beispielsweise Multiplikationen) können ebenfalls ganz einfach per Ganzzahlarithmetik durchgeführt werden (sofern die Auswirkung auf die festen Dezimalstellen berücksichtigt werden). Die Multiplikation von zwei 16,8 Ganzahlen über eine Ganzzahlmultiplikation führt Beispielsweise zu einem 32,16 Ergebnis.

Im Rahmen von Direct3D werden Festkomma-Ganzzahl-Darstellungen auf zwei Arten verwendet.

-   Die Vertex-Positionen nach dem Clipping im Rasterizer werden über Festkommazahlen gerastert, um präzise über den RenderTarget-Bereich verteilt zu werden. Viele Rasterizer-Operationen (u. A. Face-Culling) arbeiten mit gerasterten Festkommapositionen. Andere Operationen (z. B. die Attributinterpolatoreinrichtung) arbeiten mit Positionen, die von den Festkommapositionen wieder zurück zu Gleitkommawerten konvertiert wurden.
-   Texturkoordinaten für Sampling-Operationen werden auf Festkommawerte gerastert (nach der Skalierung entsprechend der Texturgröße), um bei der Auswahl von Filter-Tap-Positionen/-Gewichtungen präzise über den Texturraum verteilt zu werden Gewichtungswerte werden vor der Durchführung der tatsächlichen Filterarithmetik wieder in Gleitkommazahl konvertiert.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Quelldatentyp</th>
<th align="left">Zieldatentyp</th>
<th align="left">Konvertierungsregel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">FLOAT</td>
<td align="left">Festkomma-Ganzzahl</td>
<td align="left"><p>Im folgenden wird das grundsätzliche Verfahren zum Konvertieren einer Gleitkommazahl zu einer Festkomma-Ganzzahl i,f dargestellt. Wobei i die Anzahl der Ganzzahl-Bits (mit vorzeichen) und f die Anzahl der Nachkommastellen-Bits darstellt.</p>
<ul>
<li>Berechnung von FixedMin = -2⁽ⁱ⁻¹⁾</li>
<li>Berechnung von FixedMax = 2⁽ⁱ⁻¹⁾ - 2<sup>(-f)</sup></li>
<li>Wenn n ein NaN ist, Ergebnis = 0; wenn n ein +Inf ist, Ergebnis = FixedMax*2<sup>f</sup>; wenn n ein -Inf ist, Ergebnis = FixedMin*2<sup>f</sup></li>
<li>Wenn n &gt;= FixedMax, Ergebnis = Fixedmax*2<sup>f</sup>; wenn n &lt;= FixedMin, Ergebnis = FixedMin*2<sup>f</sup></li>
<li>Andernfalls berechnen von n * 2<sup>f</sup> und Konvertierung in Ganzzahl.</li>
</ul>
<p>Implementierungen dürfen statt dem Wert n * 2<sup>f</sup> mit unendlicher Genauigkeit nach dem letzten Schritt oben eine Toleranz von D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place für das Ganzzahlergebnis aufweisen.</p></td>
</tr>
<tr class="even">
<td align="left">Festkomma-Ganzzahl</td>
<td align="left">FLOAT</td>
<td align="left"><p>Gehen wir davon aus, dass eine bestimmte Festkommadarstellung in einen Gleitkommawert konvertiert wird, der nicht mehr als 24 Bit Informationen speichern kann und bei dem nicht mehr 23 Bits für die Nachkommakomponente reserviert sind. Nehmen wir an, die Festkommazahl fxp liegt in der Form i,f (i = Ganzahl-Bits, f = Nachkomma-Bits) vor. Die Konvertierung zu einem Gleitkommawert lässt sich in Pseudocode folgendermaßen darstellen.</p>
<p>float Ergebnis = (float)(fxp &gt;&gt; f) + // Ganzzahl</p>
((Float) (Fxp &amp; (2<sup>f</sup> - 1)) / (2<sup>f</sup>)); / / extract Bruch</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Anhänge](appendix.md)

 

 




