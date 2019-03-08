---
title: Ansichten
description: Der Begriff \ 0034;Ansicht \ 0034; wird für \ 0034;Daten im erforderlichen Format \ 0034; verwendet. Eine Konstantenpufferansicht (CBV) beispielweise, sind ordnungsgemäß formatierte, konstante Pufferdaten. Dieser Abschnitt beschreibt die gängigsten und hilfreichsten Ansichten.
ms.assetid: 0C7FB99F-7391-472F-BA53-576888DFC171
keywords:
- Ansichten
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: df106a400a7ba8c8f94aa6dd35325aabacd36eca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663295"
---
# <a name="views"></a>Ansichten


Der Begriff „Ansicht“ wird für „Daten im erforderlichen Format“ verwendet. Eine Konstantenpufferansicht (CBV) beispielweise, sind ordnungsgemäß formatierte, konstante Pufferdaten. Dieser Abschnitt beschreibt die gängigsten und hilfreichsten Ansichten.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="constant-buffer-view--cbv-.md">Konstantenpuffer anzeigen (CBV-)</a></p></td>
<td align="left"><p>Konstantenpuffer enthalten Konstantendaten für Shader. Der Nutzen ist, dass die Daten erhalten bleiben und jeder GPU-Shader darauf zugreifen kann, bis eine Änderung der Daten erforderlich wird.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-buffer-view--vbv-.md">Puffer vertexansicht (VBV) und Puffer Indexansicht (IBV)</a></p></td>
<td align="left"><p>Ein Scheitelpunktpuffer enthält Daten für eine Liste von Scheitelpunkten. Die Daten für jeden Scheitelpunkt können Position, Farbe, Normalvektor, Texturkoordinaten u. dgl. beinhalten. Ein Indexpuffer enthält Ganzzahlindizes (Offsets) für einen Scheitelpunktpuffer und wird verwendet, um ein Objekt, das aus einem Teil der Liste aller Scheitelpunkte besteht, zu definieren und zu rendern.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="shader-resource-view--srv-.md">Shaderressourcenansicht (SRV) und Unsortierte Zugriffsansicht (UAV)</a></p></td>
<td align="left"><p>Shaderressourcenansichten verpacken Texturen in der Regel in einem Format, sodass die Shader darauf zugreifen können. Eine unsortierte Zugriffsansicht bietet ähnlichen Funktionen, ermöglicht aber das Lesen und Schreiben der Textur (oder einer anderen Ressource) in beliebiger Reihenfolge.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="sampler.md">Sampler</a></p></td>
<td align="left"><p>Mit Sampling wird der Prozess des Lesens von Eingabewerten aus einer Textur oder einer anderen Ressource bezeichnet. Ein &quot;Sampler&quot; ist ein beliebiges Objekt, das aus Ressourcen liest.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-target-view--rtv-.md">(RTV) Ansicht des Renderziels</a></p></td>
<td align="left"><p>Renderziele ermöglichen das Rendern einer Szene, die auf dem Bildschirm gerendert werden soll, in einem temporären Zwischenpuffer statt im Hintergrundpuffer. Dieses Feature ermöglicht die Verwendung der komplexen Szene, die z. B. als eine Spiegelungstextur oder für andere Zwecke in der Grafikpipeline, oder vielleicht zum Hinzufügen von Pixelshader-Effekten zu der Szene vor dem Rendern gerendert werden kann.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="depth-stencil-view--dsv-.md">Ansicht der tiefenschablone (DSV)</a></p></td>
<td align="left"><p>Eine Tiefenschablonenansicht stellt das Format und den Puffer zur Speicherung von Tiefen- und Schabloneninformationen bereit. Der Tiefenpuffer dient dazu, das Zeichnen von Pixeln zu vermeiden, die von einem näher liegenden Objekt verdeckt werden und so für den Betrachter nicht sichtbar wären. Der Schablonenpuffer kann zur Aussortierung aller Zeichnungsaktivitäten außerhalb einer definierten Form genutzt werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="stream-output-view--sov-.md">Stream-Ausgabe anzeigen (SOV)</a></p></td>
<td align="left"><p>Streamausgabeansichten ermöglichen, dass die Vertexinformationen von den Vertex-, Tessellation- und Hüllen-Shadern zur weiteren Verwendung zurück in die Anwendung gestreamt werden. Zum Beispiel könnte ein Objekt, das durch diese Shader verzerrt wurde, in die Anwendung zurückgeschrieben werden, um eine genauere Eingabe für eine Physik- oder eine andere Engine bereitzustellen. In der Praxis sind aber Streamausgabeansichten ein selten verwendetes Feature der Grafikpipeline.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rasterizer-ordered-view--rov-.md">Des Rasterizers sortiert anzeigen (ROV)</a></p></td>
<td align="left"><p>Mit rasterizergesteuerten Ansichten können einige der Einschränkungen von Tiefenpuffern behandelt werden, insbesondere wenn mehrere Texturen mit Transparenz alle für dieselben Pixel gelten.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Schulungsleitfaden für Direct3D-Grafiken](index.md)

 

 




