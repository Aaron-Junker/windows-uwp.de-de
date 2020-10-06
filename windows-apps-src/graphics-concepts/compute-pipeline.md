---
title: Berechnen der Pipeline
description: Die Direct3D-Compute-Pipeline wurde zur Verarbeitung von Berechnungen entwickelt, die meist parallel mit der Grafikpipeline ausgeführt werden können.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 36d1bd524d6e71b0a1aa9477d7a2b7a5f27544aa
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750046"
---
# <a name="compute-pipeline"></a>Berechnen der Pipeline


\[Einige Informationen beziehen sich auf Vorabversionen, die vor der kommerziellen Freigabe grundlegend geändert werden können. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.\]


Die Direct3D-Compute-Pipeline wurde zur Verarbeitung von Berechnungen entwickelt, die meist parallel mit der Grafikpipeline ausgeführt werden können. Es gibt nur wenige Schritte in der computepipeline, bei der Daten von der Eingabe in die Ausgabe über die programmierbare Compute-Shader-Stufe fließen.

## <a name="purpose"></a>Zweck

Wie andere programmierbare [Shader wird die Compute-Shader (CS)-Phase](compute-shader-stage--cs-.md) mit HLSL entworfen und implementiert. Ein Compute-Shader bietet eine hohe Geschwindigkeit für das allgemeine Computing und nutzt die große Anzahl paralleler Prozessoren auf der GPU (Graphics Processing Unit). Der COMPUTE-Shader bietet Funktionen für die Speicherfreigabe und die Thread Synchronisierung, um effektivere parallele Programmiermethoden zu ermöglichen.

## <a name="input"></a>Eingabe

Im Gegensatz zu anderen programmierbaren Shadern ist die Definition der Eingabe abstrakt. Die Eingabe kann ein, zwei oder dreidimensional sein und die Anzahl der Aufrufe des auszuführenden Compute-Shader bestimmen. Es ist möglich, freigegebene Daten für einen Satz von aufrufen zu definieren, der gelesen werden soll.

## <a name="output"></a>Output

Ausgabedaten aus dem Compute-Shader, die stark variieren können, können mit der Grafik Rendering-Pipeline synchronisiert werden, wenn die berechneten Daten benötigt werden.

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


[Direct3D Grafik Lernhandbuch](index.md)

 

 
