---
title: Berechnen der Pipeline
description: Die Direct3D-Berechnungs-Pipeline wurde entwickelt, um Berechnungen zu erledigen, die meist parallel mit der Grafik-Pipeline ausgeführt werden können.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 911546f1c2973a79aea4b597a47352149a4e4210
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651115"
---
# <a name="compute-pipeline"></a>Berechnen der Pipeline


\[Einige Informationen beziehen sich auf einer Vorabversion eines Produkts die vor der markteinführung grundlegend geändert werden kann. Microsoft übernimmt keine Garantien, ausdrücklich oder konkludent, in Bezug auf die hier bereitgestellten Informationen.\]


Die Direct3D-Berechnungs-Pipeline wurde entwickelt, um Berechnungen zu erledigen, die meist parallel mit der Grafik-Pipeline ausgeführt werden können. Die Compute-Pipeline enthält nur wenige Schritte, bei der Daten zwischen Eingabe und Ausgabe über von Eingaben über die programmierbare Computeshaderphase fließen.

| | |
|-|-|
|Zweck|Wie andere programmierbare Shader wurde auch die [Computeshaderphase (CS)](compute-shader-stage--cs-.md) mit HLSL entwickelt und implementiert. Ein Computeshader bietet schnelle, allgemeine Berechnungen und nutzt die große Anzahl von parallelen Prozessoren auf dem Grafikprozessor (GPU). Der Computeshader bietet die Freigabe des Arbeitsspeichers und Threadsynchronisierung, um effektivere parallele Programmiermethoden zu ermöglichen.|
|Input|Im Gegensatz zu anderen programmierbaren Shadern ist die Definition der Eingabe abstrakt. Die Eingabe kann ein-, zwei- oder dreidimensionaler Natur sein, um die Anzahl der auszufühRendern Aufrufe des Computeshaders festzulegen. Es ist möglich, gemeinsam genutzte Daten für einen Satz zu lesender Aufrufe zu definieren.|
|Ausgabe|Ausgabedaten des Computeshaders können die stark variiert und mit der Grafikrenderingpipeline synchronisiert werden, wenn die berechneten Daten erforderlich sind.|
| | |




<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Schulungsleitfaden für Direct3D-Grafiken](index.md)

 

 
