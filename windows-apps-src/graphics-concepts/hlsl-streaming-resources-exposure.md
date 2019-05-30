---
title: Belichtung von HLSL-Streamingressourcen
description: Zur Unterstützung von Streamingressourcen in Shader Model 5 ist eine spezielle Microsoft High Level Shader Language (HLSL)-Syntax erforderlich.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- Belichtung von HLSL-Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: db8a368f6cd9e0b6d38fb16d81dbc31a0f8a615f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370601"
---
# <a name="hlsl-streaming-resources-exposure"></a>Belichtung von HLSL-Streamingressourcen


Eine spezielle Syntax für die Microsoft High Level Shader Language (HLSL) ist für die Unterstützung von Streaming Ressourcen in [Shadermodell 5](https://docs.microsoft.com/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5) erforderlich.

Die HLSL-Syntax für Shader Model 5 ist nur auf Geräten zulässig, die Streamingressourcen unterstützen. Jede relevante HLSL-Methode für das Streaming von Ressourcen in der folgenden Tabelle akzeptiert einen (feedback) oder zwei (clamp und feedback in dieser Reihenfolge) zusätzliche optionale Parameter. Aufbau einer **Sample**-Methode:

**Beispiel (Sampler, Speicherort \[, Offset \[, Clamp \[, Feedback\] \] \])**

Ein Beispiel für eine **Sample**-Methode ist [**Texture2D.Sample(S,float,int,float,uint)** ](https://docs.microsoft.com/windows/desktop/direct3dhlsl/t2darray-sample-s-float-int-float-uint-).

Die Parameter offset, clamp und feedback sind optional. Sie müssen alle optionalen Parameter bis zu dem angeben, den Sie benötigen, was mit den C++- Regeln für Standardfunktionsargumente übereinstimmt. Wenn zum Beispiel der Feedbackstatus erforderlich ist, müssen Sie sowohl den offset- als auch den clamp-Parameter explizit für **Sample** bereitstellen, obwohl diese Parameter möglicherweise logisch nicht benötigt werden.

Der Parameter clamp ist ein skalarer Float-Wert. Mit clamp=0.0f geben Sie an, dass der Clampvorgang nicht durchgeführt wird.

Der Parameter feedback ist eine **Uint**-Variable, die Sie der internen Funktion [**CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped) bereitstellen können, mit der Speicherzugriff angefordert wird. Sie müssen den Parameter feedback nicht ändern oder interpretieren – der Compiler stellt keine erweiterten Analyse- oder Diagnosemöglichkeiten bereit, um festzustellen, ob Sie den Wert geändert haben.

So lautet die Syntax für [**CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped):

**"bool" CheckAccessFullyMapped (in (Uint FeedbackVar);**

[**CheckAccessFullyMapped** ](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped) interpretiert den Wert der *FeedbackVar* und gibt true, wenn alle Daten, die auf die zugegriffen wird wurde in der Ressource zugeordnet ist, andernfalls **CheckAccessFullyMapped**"false" zurückgibt.

Wenn der Parameter clamp oder feedback vorhanden ist, gibt der Compiler eine Variante der Grundanweisung aus. Beispielsweise generiert sample für eine Streamingressource die Anweisung `sample_cl_s`.

Ist weder clamp noch feedback angegeben, gibt der Compiler die Grundanweisung aus, sodass sich das aktuelle Verhalten nicht ändert.

Der clamp-Wert 0.0f gibt an, dass kein Clamp-Vorgang ausgeführt wird. Daher kann der Treibercompiler die Anweisung weiter an die Zielarchitektur anpassen. Ist feedback ein NULL-Register in einer Anweisung, wird das Feedback nicht verwendet. Daher kann der Treibercompiler die Anweisung weiter an die Zielarchitektur anpassen.

Wenn der HLSL-Compiler erkennt, dass clamp den Wert 0.0f hat und feedback nicht verwendet wird, gibt der Compiler die entsprechende Grundanweisung aus (z. B. `sample` statt `sample_cl_s`).

Besteht ein Streamingressourcenzugriff aus mehreren Bytecodeanweisungen, beispielsweise für strukturierte Ressourcen, aggregiert der Compiler einzelne feedback-Werte mit der OR-Operation, um den endgültigen feedback-Wert zu erzeugen. Dadurch ergibt sich für einen solchen komplexen Zugriff ein einzelner feedback-Wert.

In der folgende Tabelle sind die HLSL-Methoden zusammengefasst, die geändert wurden, um feedback und/oder clamp zu unterstützen. Sie alle arbeiten mit strukturierten und Nicht-Streamingressourcen aller Dimensionen. Nicht-Streamingressourcen scheinen immer vollständig zugeordnet zu sein.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5-objects">HLSL-Objekte</a> </th>
<th align="left">Intrinsische Methoden mit feedback-Option – (*) verfügt auch über die clamp-Option</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Gather</p>
<p>GatherRed</p>
<p>GatherGreen</p>
<p>GatherBlue</p>
<p>GatherAlpha</p>
<p>GatherCmp</p>
<p>GatherCmpRed</p>
<p>GatherCmpGreen</p>
<p>GatherCmpBlue</p>
<p>GatherCmpAlpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>[RW]Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Sample*</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>Texture2DMS</p>
<p>[RW]Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>[RW]Texture3D</p>
<p>[RW]Buffer</p>
<p>[RW]ByteAddressBuffer</p>
<p>[RW]StructuredBuffer</p></td>
<td align="left">Laden</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Pipeline-Zugriff auf Ressourcen streaming](pipeline-access-to-streaming-resources.md)

 

 




