---
title: Belichtung von HLSL-Streamingressourcen
description: Zur Unterstützung von Streamingressourcen in Shader-Modell 5 ist eine bestimmte Microsoft High Level Shader Language (HLSL)-Syntax erforderlich.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- Belichtung von HLSL-Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: decb0ff2d791417326a70b06c0fdd6a25ea2d119
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173084"
---
# <a name="hlsl-streaming-resources-exposure"></a>Belichtung von HLSL-Streamingressourcen


Zur Unterstützung von Streamingressourcen in [Shader-Modell 5](/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5)ist eine bestimmte Microsoft High Level Shader Language (HLSL)-Syntax erforderlich.

Die HLSL-Syntax für Shader Model 5 ist nur auf Geräten mit Unterstützung für Streamingressourcen zulässig. Jede relevante HLSL-Methode für das Streamen von Ressourcen in der folgenden Tabelle akzeptiert entweder ein (Feedback) oder zwei (in dieser Reihenfolge einspannen und Feedback) zusätzliche optionale Parameter. Beispielsweise ist eine **Beispiel** Methode wie folgt:

**Sample (Sampler, Location \[ , Offset \[ , Clamp \[ , Feedback \] \] \] )**

Ein Beispiel für eine **Beispiel Methode** ist [**Texture2D. Sample (S, float, int, float, uint)**](/windows/desktop/direct3dhlsl/t2darray-sample-s-float-int-float-uint-).

Die Offset-, Clamp-und Feedback-Parameter sind optional. Sie müssen alle optionalen Parameter angeben, die Sie benötigen. Dies entspricht den C++-Regeln für Standard Funktionsargumente. Wenn der Feedback Status z. b. erforderlich ist, müssen sowohl Offset-als auch Klammer Parameter explizit für **Stichproben**bereitgestellt werden, auch wenn Sie möglicherweise nicht logisch benötigt werden.

Der Klammer Parameter ist ein skalarer float-Wert. Der Literalwert von Clamp = 0,0 f gibt an, dass der Klammer Vorgang nicht ausgeführt wird.

Der Feedback Parameter ist eine **uint** -Variable, die Sie der systeminternen [**checkaccessfullymapping**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) -Funktion für den Speicherzugriff zur Abfrage bereitstellen können. Der Wert des Parameters "Feedback" darf nicht geändert oder interpretiert werden. der Compiler bietet jedoch keine erweiterte Analyse und Diagnose, um zu ermitteln, ob Sie den Wert geändert haben.

Dies ist die Syntax von [**checkaccessfullymapping**](/windows/desktop/direct3dhlsl/checkaccessfullymapped):

**bool checkaccessfullymapping (in uint feedbackvar);**

[**Checkaccessfullymapping**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) interpretiert den Wert von *feedbackvar* und gibt true zurück, wenn alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurden. Andernfalls gibt **checkaccessfullymapping** den Wert false zurück.

Wenn entweder der Parameter "Clamp" oder "Feedback" vorhanden ist, gibt der Compiler eine Variante der Grund Anweisung aus. Beispielsweise generiert Sample of a Streaming Resource die- `sample_cl_s` Anweisung.

Wenn weder "Clamp" noch "Feedback" angegeben ist, gibt der Compiler die grundlegende Anweisung aus, sodass es keine Änderung des aktuellen Verhaltens gibt.

Der Klammer Wert von 0,0 f gibt an, dass keine Klammer ausgeführt wird. Daher kann der Treiber Compiler die Anweisung weiter an die Zielhardware anpassen. Wenn Feedback ein NULL-Register in einer Anweisung ist, wird das Feedback nicht verwendet. Daher kann der Treiber Compiler die Anweisung weiter an die Zielarchitektur anpassen.

Wenn der HLSL-Compiler diese Klammer als 0,0 f ausgibt und Feedback nicht verwendet wird, gibt der Compiler die entsprechende Grund Anweisung aus (z `sample` . b `sample_cl_s` . anstelle von).

Wenn ein Zugriff auf Streaming-Ressourcen aus mehreren einzelnen Anweisungen für den Byte Code besteht (z. b. bei strukturierten Ressourcen), aggregiert der Compiler einzelne Feedback Werte über den Vorgang oder, um den endgültigen Feedback Wert zu erhalten. Aus diesem Grund wird ein einzelner Feedback Wert für einen solchen komplexen Zugriff angezeigt.

Dies ist die Zusammenfassungs Tabelle der HLSL-Methoden, die zur Unterstützung von Feedback und/oder der Klammer geändert werden. Diese Vorgänge funktionieren bei Kacheln und nicht-Streaming-Ressourcen aller Dimensionen. Nicht Streaming-Ressourcen scheinen immer vollständig zugeordnet zu sein.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5-objects">HLSL-Objekte</a> </th>
<th align="left">Intrinsische Methoden mit der Option "Feedback" (*)-auch die Option "Klemme"</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>RW Texture2D</p>
<p>RW Texture2DArray</p>
<p>Texturecube</p>
<p>Texturecubearray</p></td>
<td align="left"><p>Informieren</p>
<p>Gatherred</p>
<p>Gathergrün</p>
<p>Gatherblue</p>
<p>Gatheralpha</p>
<p>Gathercmp</p>
<p>Gathercmpred</p>
<p>Gathercmpgreen</p>
<p>Gathercmpblue</p>
<p>Gathercmpalpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>RW Texture1D</p>
<p>RW Texture1DArray</p>
<p>RW Texture2D</p>
<p>RW Texture2DArray</p>
<p>RW Texture3D</p>
<p>Texturecube</p>
<p>Texturecubearray</p></td>
<td align="left"><p>Blutprobe</p>
<p>Samplebias *</p>
<p>Samplecmp *</p>
<p>Samplecmplevelzero</p>
<p>Samplegrad *</p>
<p>Samplelevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>RW Texture1D</p>
<p>RW Texture1DArray</p>
<p>RW Texture2D</p>
<p>Texture2DMS</p>
<p>RW Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>RW Texture3D</p>
<p>RW Ert</p>
<p>RW Byteaddressbuffer</p>
<p>RW Structuredbuffer</p></td>
<td align="left">Laden</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Pipelinezugriff auf Streamingressourcen](pipeline-access-to-streaming-resources.md)

 

 