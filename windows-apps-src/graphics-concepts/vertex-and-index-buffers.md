---
title: Vertex- und Indexpuffer
description: Scheitelpunktpuffer sind Speicherpuffer, die Scheitelpunktdaten enthalten. Scheitelpunkte in einem Scheitelpunktpuffer werden verarbeitet, um Transformation, Beleuchtung und Zuschneiden auszuführen.
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords:
- Vertex- und Indexpuffer
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d2ea6ce4060957ade5dd1007389be51176440f04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593575"
---
# <a name="vertex-and-index-buffers"></a>Vertex- und Indexpuffer


*Scheitelpunktpuffer* sind Speicherpuffer, die Scheitelpunktdaten enthalten. Scheitelpunkte in einem Scheitelpunktpuffer werden verarbeitet, um Transformation, Beleuchtung und Zuschneiden auszuführen. *Indexpuffer* sind Speicherpuffer mit Indexdaten, die Ganzzahl-Offsets in Vertexpuffer darstellen, und zum Rendern von Grundtypen verwendet werden.

Scheitelpunktpuffer können Scheitelpunkte beliebiger Art enthalten, die gerendert werden können – transformiert oder nicht transformiert, beleuchtet oder nicht beleuchtet. Sie können die Scheitelpunkte in einem Scheitelpunktpuffer verarbeiten, um Vorgänge wie Transformation oder Beleuchtung durchzuführen, oder um Zuschneidemarkierungen zu generieren. Die Transformation wird immer ausgeführt.

Die Flexibilität der Scheitelpunktpuffer macht sie zu idealen Phasenpunkten für die Wiederverwendung einer transformierten Geometrie. Sie können etwa einen einzelnen Scheitelpunktpuffer erstellen, die Scheitelpunkte darin transformieren, beleuchten und zuschneiden und dann das Modell in der Szene so oft wie nötig rendern, ohne es erneut zu transformieren, selbst mit überlappenden Renderingzustandsänderungen. Dies ist nützlich beim Rendern von Modellen, die mehrere Texturen verwenden: Die Geometrie wird nur einmal transformiert, und anschließend können Teile davon nach Bedarf gerendert werden, in Überlappung mit den erforderlichen Texturänderungen. Renderingzustandsänderungen, die nach der Verarbeitung von Scheitelpunkten vorgenommen wurden, werden bei der nächsten Verarbeitung der Scheitelpunkte wirksam.

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
<td align="left"><p><a href="introduction-to-buffers.md">Einführung in Puffer</a></p></td>
<td align="left"><p>Eine Pufferressource ist eine Sammlung vollständig typisierter Daten, die zu Elementen gruppiert werden. Puffer speichern Daten, wie z. B. Texturkoordinaten in einem <em>Scheitelpunktpuffer</em>, Indizes in einem <em>Indexpuffer</em>, Shader-Konstantendaten in einem <em>Konstantenpuffer</em>, Positionsvektoren, normale Vektoren oder den Gerätezustand.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="index-buffers.md">Indexpuffer</a></p></td>
<td align="left"><p><em>Indexpuffer</em> sind Speicherpuffer mit Indexdaten, die Ganzzahl-Offsets in Vertexpuffer darstellen, und zum Rendern von Grundtypen verwendet werden.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Schulungsleitfaden für Direct3D-Grafiken](index.md)

 

 




