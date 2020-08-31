---
title: Gleitkommaregeln
description: Direct3D unterstützt mehrere Gleit Komma Darstellungen. Alle Gleit Komma Berechnungen arbeiten unter einer definierten Teilmenge der IEEE 754 32-Bit-Gleit Komma Regeln mit einfacher Genauigkeit.
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- Gleitkommaregeln
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7832d4ad344e425bc479d52e1516ae0c63dff624
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168064"
---
# <a name="span-iddirect3dconceptsfloating-point_rulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>Gleitkommaregeln


Direct3D unterstützt mehrere Gleit Komma Darstellungen. Alle Gleit Komma Berechnungen arbeiten unter einer definierten Teilmenge der IEEE 754 32-Bit-Gleit Komma Regeln mit einfacher Genauigkeit.

## <a name="span-idalpha_32_bitspanspan-idalpha_32_bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>32-Bit-Gleit Komma Regeln


Es gibt zwei Sätze von Regeln: die, die IEEE-754 entsprechen, und die Regeln, die vom Standard abweichen.

### <a name="span-idalpha_754_rulesspanspan-idalpha_754_rulesspanspan-idalpha_754_rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>IEEE-754-Regeln wurden berücksichtigt.

Einige dieser Regeln stellen eine einzelne Option dar, bei der IEEE-754 Optionen bietet.

-   Division durch 0 erzeugt +/-inf, mit Ausnahme von 0/0, was zu Nan führt.
-   das Protokoll von (+/-) 0 erzeugt-inf.  

    das Protokoll eines negativen Werts (mit Ausnahme von-0) erzeugt Nan.
-   Die gegenseitige Quadratwurzel (RSQ) oder die Quadratwurzel (sqrt) einer negativen Zahl erzeugt Nan.  

    Die Ausnahme ist-0; SQRT (-0) erzeugt-0, und RSQ (-0) erzeugt-inf.
-   INF-INF = Nan
-   (+/-) Inf/(+/-) INF = Nan
-   (+/-) INF \* 0 = Nan
-   Nan (beliebiger OP) beliebiger Wert = NaN
-   Die Vergleiche EQ, gt, ge, lt und Le, wenn einer oder beide Operanden NaN ist, wird **false**zurückgegeben.
-   Vergleiche ignorieren das Vorzeichen von 0 (also + 0 ist 0).
-   Der Vergleichs-ne, wenn einer oder beide Operanden NaN ist, wird **true**zurückgegeben.
-   Vergleiche eines beliebigen nicht-NaN-Werts mit +/-INF geben das korrekte Ergebnis zurück.

### <a name="span-idalpha_754_deviationsspanspan-idalpha_754_deviationsspanspan-idalpha_754_deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>Abweichungen oder zusätzliche Anforderungen von IEEE-754-Regeln

-   IEEE-754 erfordert Gleit Komma Vorgänge, um ein Ergebnis zu erhalten, das den nächstgelegenen darstellbaren Wert zu einem unendlich präzisen Ergebnis ist, das auch als "Round-to-Next-even" bezeichnet wird.

    Direct3D 11 und up definieren dieselbe Anforderung wie IEEE-754:32-Bit-Gleit Komma Vorgänge führen zu einem Ergebnis, das sich innerhalb von 0,5 Unit-Last-Place (ULP) mit dem unendlich präzisen Ergebnis befindet. Dies bedeutet, dass Hardware z. b. die Ergebnisse in 32-Bit Abschneiden darf, anstatt Sie auf die nächstgelegene Weise zu durchführen, da dies zu einem Fehler von höchstens 0,5 ULP führen würde. Diese Regel gilt nur für Addition, Subtraktion und Multiplikation.

    Frühere Versionen von Direct3D definieren eine mehrdimensionale Anforderung als IEEE-754:32-Bit-Gleit Komma Vorgänge führen zu einem Ergebnis, das sich innerhalb eines Einheits Last Orts (1 ULP) mit dem unendlich präzisen Ergebnis befindet. Dies bedeutet, dass Hardware z. b. die Ergebnisse in 32-Bit Abschneiden darf, anstatt Sie durch eine roundTo-Next-even-Methode auszuführen, da dies zu einem Fehler bei höchstens einer ULP führen würde.

-   Es gibt keine Unterstützung für Gleit Komma Ausnahmen, Status Bits oder Traps.
-   Denorms werden bei einem Eingabe-und Ausgabewert einer mathematischen Gleit Komma Operation in die Signierung NULL geleert. Ausnahmen werden für alle e/a-oder Daten Verschiebungs Vorgänge ausgelöst, die die Daten nicht verändern.
-   Zustände, die Gleit Komma Werte enthalten, wie z. b. Viewport mintiefe/maxtiefe oder BorderColor-Werte, können als denorm-Werte angegeben werden und werden möglicherweise nicht geleert, bevor Sie von der Hardware verwendet werden.
-   Bei minimalen oder maximalen Vorgängen werden denorms zum Vergleichen geleert, das Ergebnis kann jedoch nicht mit denorm geleert werden.
-   Die Nan-Eingabe für einen Vorgang erzeugt immer Nan in der Ausgabe. Es ist jedoch nicht erforderlich, das genaue Bitmuster des Nan unverändert zu bleiben (es sei denn, der Vorgang ist eine unformatierte Move-Anweisung, die keine Daten ändert).
-   Bei minimalen oder maximalen Vorgängen, bei denen nur ein Operand NaN ist, wird der andere Operand als Ergebnis zurückgegeben (im Gegensatz zu den zuvor übersuchten Vergleichs Regeln). Dies ist eine IEEE 754r-Regel.

    Die IEEE-754r-Spezifikation für Gleit Komma-und Max-Vorgänge gibt an, dass das Ergebnis der Operation der andere Parameter ist, wenn eine der Eingaben für "min" oder "Max" ein stiller QNAN-Wert ist. Beispiel:

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    Eine Revision der IEEE-754r-Spezifikation hat ein anderes Verhalten für "min" und "Max" übernommen, wenn eine Eingabe ein "Signalisierender"-Wert im Vergleich zu einem QNAN-Wert ist:

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    Im allgemeinen befolgt Direct3D die Standards für Arithmetik: IEEE-754 und IEEE-754r. In diesem Fall haben wir jedoch eine Abweichung.

    Die arithmetischen Regeln in Direct3D 10 und höher machen keine Unterschiede zwischen Stillen und signalisierenden Nan-Werten (QNAN und SNaN). Alle NaN-Werte werden auf dieselbe Weise behandelt. Im Fall von min und Max entspricht das Direct3D-Verhalten für jeden NaN-Wert der Art und Weise, wie QNAN in der IEEE-754r-Definition behandelt wird. (Aus Gründen der Vollständigkeit: Wenn beide Eingaben Nan sind, wird ein beliebiger NaN-Wert zurückgegeben.)

-   Eine weitere IEEE 754r-Regel ist, dass min (-0, + 0) = = min (+ 0,-0) = =-0 und Max (-0, + 0) = = Max (+ 0,-0) = = + 0, das das Vorzeichen berücksichtigt, im Gegensatz zu den Vergleichs Regeln für den Wert signed Zero (wie zuvor gesehen). Direct3D empfiehlt das IEEE 754r-Verhalten hier, erzwingt es jedoch nicht. Es ist zulässig, dass das Ergebnis des Vergleichs von Nullen von der Reihenfolge der Parameter abhängig ist. dabei wird ein Vergleich verwendet, bei dem die Vorzeichen ignoriert werden.
-   x \* 1.0 f ergibt immer x (außer denorm geleert).
-   x/1.0 f ergibt immer x (außer denorm geleert).
-   x +/-0,0 f ergibt immer x (außer denorm geleert). Aber-0 + 0 = + 0.
-   Beim Durchlaufen von Vorgängen (z. b. Mad, DP3) werden Ergebnisse erzeugt, die nicht weniger genau sind als die schlechtesten mögliche serielle Reihenfolge der Auswertung der nicht geschmolzenen Erweiterung des Vorgangs. Die Definition der schlechtesten Reihenfolge ist für die Toleranz keine festgelegte Definition für einen angegebenen Fused-Vorgang. Dies hängt von den jeweiligen Werten der Eingaben ab. Die einzelnen Schritte in der nicht zugezierten Erweiterung sind jeweils als 1 ULP-Toleranz zulässig (oder für Anweisungen Direct3D Aufrufe mit einer weniger Lax-Toleranz als 1 ULP, je weniger Lax-Toleranz zulässig ist).
-   Bei der durchgeführten Ausführung werden die gleichen Nan-Regeln wie nicht-Fused-Vorgänge befolgt.
-   Sqrt und RCP haben eine ULP-Toleranz. Die wechselseitigen und gegenseitigen Square-root-Anweisungen, [**RCP**](/previous-versions/windows/desktop/legacy/hh447205(v=vs.85)) und [**RSQ**](/windows/desktop/direct3dhlsl/rsq--sm4---asm-), weisen eine eigene separate, gelockerte Genauigkeits Anforderung auf.
-   Multiplizieren und Dividieren der einzelnen Vorgänge auf der 32-Bit-Ebene für Gleit Komma Genauigkeit (Genauigkeit zu 0,5 ULP für Multiplikation, 1,0 ULP für die gegenseitige Verwendung). Wenn x/y direkt implementiert wird, müssen die Ergebnisse größer oder gleich der Genauigkeit als eine zweistufige Methode sein.

## <a name="span-iddouble_prec_64_bitspanspan-iddouble_prec_64_bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>64-Bit-Gleit Komma Regeln (doppelte Genauigkeit)


Hardware-und Anzeigetreiber unterstützen optional Gleit Komma Zahlen mit doppelter Genauigkeit. Um Unterstützung anzugeben, legt der Treiber beim Anrufen von [**ID3D11Device:: checkfeaturesupport**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) mit [**D3D11 \_ Feature \_ **](/windows/desktop/api/d3d11/ne-d3d11-d3d11_feature)Double den Wert **Double precisionfloatshaderops** von [**D3D11 \_ Feature \_ Data \_ Doubles**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_doubles) auf true fest. Der Treiber und die Hardware müssen dann alle Gleit Komma Anweisungen mit doppelter Genauigkeit unterstützen.

Anweisungen mit doppelter Genauigkeit folgen den Anforderungen an das IEEE 754r-Verhalten.

Die Unterstützung für die Generierung von denormalisierten Werten ist für Daten mit doppelter Genauigkeit erforderlich (kein Verhalten beim leeren zu null). Ebenso werden von den Anweisungen keine denormalisierten Daten als signierte NULL-Werte gelesen, Sie berücksichtigen den denorm-Wert.

## <a name="span-idalpha_16_bitspanspan-idalpha_16_bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>16-Bit-Gleit Komma Regeln


Direct3D unterstützt auch 16-Bit-Darstellungen von Gleit Komma Zahlen.

Format:

-   1 Signier Bit (e) an der MSB-Bitposition
-   5 Bits von unparteiischen Exponent (e)
-   10 Bits des Bruchteils (f) mit einem zusätzlichen ausgeblendeten Bit

Ein float16-Wert (v) folgt diesen Regeln:

-   Wenn e = = 31 und f! = 0 ist, dann ist v unabhängig von s
-   Wenn e = = 31 und f = = 0, dann v = (-1) s \* unendlich (Vorzeichen unendlich)
-   Wenn e zwischen 0 und 31 liegt, dann v = (-1) s \* 2 (e-15) \* (1. f)
-   Wenn e = = 0 und f! = 0, dann v = (-1) s \* 2 (e-14) \* (0. f) (Denormalisierte Zahlen)
-   Wenn e = = 0 und f = = 0, dann v = (-1) s \* 0 (signierte null)

32-Bit-Gleit Komma Regeln enthalten auch für 16-Bit-Gleit Komma Zahlen, die für das zuvor beschriebene bitlayout angepasst wurden. Ausnahmen bilden die folgenden Fälle:

-   Genauigkeit: bei unfused-Vorgängen für 16-Bit-Gleit Komma Zahlen wird ein Ergebnis erzeugt, bei dem es sich um den nächstgelegenen darstellbaren Wert eines unendlich präzisen Ergebnisses handelt (abgerundet auf den nächstgelegenen Wert, auch pro IEEE-754, auf 16-Bit-Werte angewendet). 32-Bit-Gleit Komma Regeln entsprechen 1 ULP-Toleranz, 16-Bit-Gleit Komma Regeln entsprechen 0,5 ULP für unfused-Vorgänge und 0,6 ULP für Fused-Vorgänge.
-   16-Bit-Gleit Komma Zahlen behalten denorms bei.

## <a name="span-idalpha_11_bitspanspan-idalpha_11_bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>11-Bit-und 10-Bit-Gleit Komma Regeln


Direct3D unterstützt auch 11-Bit-und 10-Bit-Gleit Komma Formate.

Format:

-   Kein Signier Bit
-   5 Bits von unparteiischen Exponent (e)
-   6 Bits des Bruchteils (f) für ein 11-Bit-Format, 5 Bits von Bruchteil (f) für ein 10-Bit-Format, mit einem zusätzlichen ausgeblendeten Bit in beiden Fällen.

Ein float11/float10-Wert (v) folgt den folgenden Regeln:

-   Wenn e = = 31 und f! = 0 ist, dann ist v Nan.
-   Wenn e = = 31 und f = = 0, dann v = + unendlich
-   Wenn e zwischen 0 und 31 liegt, dann v = 2 (e-15) \* (1. f)
-   Wenn e = = 0 und f! = 0, dann v = \* 2 (e-14) \* (0. f) (Denormalisierte Zahlen)
-   Wenn e = = 0 und f = = 0, dann v = 0 (null)

32-Bit-Gleit Komma Regeln enthalten auch für 11-Bit-und 10-Bit-Gleit Komma Zahlen, die für das zuvor beschriebene bitlayout angepasst wurden. Zu den Ausnahmen zählen:

-   Genauigkeit: 32-Bit-Gleit Komma Regeln entsprechen 0,5 ULP.
-   10/11-Bit-Gleit Komma Zahlen behalten denorms bei.
-   Jeder Vorgang, der zu einer Zahl kleiner als 0 (null) führt, wird an NULL gebunden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Anhänge](appendix.md)

[Ressourcen](/windows/desktop/direct3d11/overviews-direct3d-11-resources)

[Texturen](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 