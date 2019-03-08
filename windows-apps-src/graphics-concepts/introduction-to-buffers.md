---
title: Einführung zu Puffern
description: Eine Pufferressource ist eine Sammlung vollständig typisierter Daten, die zu Elementen gruppiert werden.
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- Einführung zu Puffern
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: deeae0cc66a7e75da2e44c0d2aba2a9ed459b824
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602575"
---
# <a name="introduction-to-buffers"></a>Einführung zu Puffern


Eine Pufferressource ist eine Sammlung vollständig typisierter Daten, die zu Elementen gruppiert werden. Puffer speichern Daten, wie z. B. Texturkoordinaten in einem *Scheitelpunktpuffer*, Indizes in einem *Indexpuffer*, Shader-Konstantendaten in einem *Konstantenpuffer*, Positionsvektoren, normale Vektoren oder den Gerätezustand.

Ein Pufferelement besteht aus ein bis vier Komponenten. Pufferelemente können gepackte Datenwerten (wie R8G8B8A8-Oberflächenwerte), einzelne 8-Bit-Ganzahlen oder vier 32-Bit-Gleitkommawerte enthalten.

Ein Puffer wird als eine unstrukturierte Ressource erstellt. Ein Puffer kann, da unstrukturiert, keine Mipmap-Ebenen enthalten, nicht beim Lesen gefiltert werden und unterstützt kein Multisampling.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Puffer-Typen


Folgende Pufferressourcentypen werden von Direct3D 11 unterstützt.

-   [Vertex-Puffer](#vertex-buffer)
-   [Indexpuffer](#index-buffer)
-   [Konstantenpuffer](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Vertex-Puffer

Ein Vertexpuffer enthält Scheitelpunktdaten, die zur Definition Ihrer Geometrie verwendet werden. Zu den Scheitelpunktdaten gehören Positionskoordinaten, Farbdaten, Texturkoordinatendaten, Normale-Daten usw.

Das einfachste Beispiel eines Vertexpuffers ist einer, der nur Positionsdaten enthält. Dieser kann dargestellt werden, und sieht aus wie in folgender Abbildung.

![Abbildung eines Vertexpuffers, der Positionsdaten enthält](images/d3d10-resources-single-element-vb2.png)

Häufiger enthält ein Vertexpuffer aber alle erforderlichen Daten, um 3D-Scheitelpunkte vollständig dazustellen. Ein Beispiel dazu könnte ein Vertexpuffer sein, der eine Position pro Scheitelpunkt sowie Normale und Texturkoordinaten enthält. Diese Daten sind in der Regel als Sätze der Elemente pro Scheitelpunkt organisiert, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Vertexpuffers, der Positionsdaten, Normale und Texturdaten enthält](images/d3d10-vertex-buffer-element.png)

Dieser Vertexpuffer enthält Daten pro Scheitelpunkt; jeder Scheitelpunkt speichert drei Elemente (Positionsdaten, Normale und Texturkoordinaten). Position und Normale werden normalerweise mit drei 32-Bit-Gleitkommawerten und die Texturkoordinaten mit zwei 32-Bit-Gleitkommawerten angegeben.

Um Daten aus einem Vertexpuffer abzurufen, müssen Sie den gewünschten Scheitelpunkt und folgende zusätzliche Pufferparameter kennen:

-   Offset - Die Anzahl von Bytes vom Anfang des Puffers bis zu den Daten für den ersten Scheitelpunkt.
-   BaseVertexLocation - Die Anzahl der Bytes vom Offset bis zum ersten Scheitelpunkt, den der entsprechende Zeichnen-Aufruf verwendet.

Bevor Sie einen Vertexpuffer erstellen, müssen Sie das Layout definieren. Nachdem Sie das Eingabe-Layout-Objekt erstellt haben, binden Sie es an die [Eingabeassemblerphase (IA)](input-assembler-stage--ia-.md).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Indexpuffer

Indexpuffer enthalten Ganzzahl-Offsets in Vertexpuffern und werden zum effizienteren Rendern der Grundtypen verwendet. Ein Indexpuffer enthält eine aufeinanderfolgende Reihe von 16-Bit- oder 32-Bit-Indizes; jeder Index wird verwendet, um einen Scheitelpunkt im Vertexpuffer zu kennzeichnen. Ein Indexpuffer kann dargestellt werden, wie in der folgenden Abbildung.

![Abbildung eines Indexpuffers](images/d3d10-index-buffer.png)

Die aufeinanderfolgenden Indizes in einem Indexpuffer werden mit den folgenden Parametern lokalisiert:

-   Offset - Die Anzahl an Bytes vom Anfang des Puffers bis zum ersten Index.
-   StartIndexLocation - Gibt das erste Element des Indexpuffers aus der Basisadresse und dem Offset an. Die Startposition stellt den ersten Index dar, der gerendert werden kann.
-   IndexCount - Die Anzahl der Indizes, die gerendert werden kann.

Index pufferanfangs = Index Puffer Basisadresse + Offsets (Bytes) + StartIndexLocation \* ElementSize (Bytes).

In dieser Rechnung gibt ElementSize die Größe jedes Indexpufferelements an, das entweder zwei oder vier Bytes groß ist.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Konstantenpuffer

Ein Konstantenpuffer ermöglicht Ihnen, Shader-Konstantendaten effizient an die Pipeline zu liefern. Sie können einen Konstantenpuffer verwenden, um die Ergebnisse der Streamausgabephase zu speichern. Vom Konzept her sieht ein Konstantenpuffer wie ein Vertexpuffer mit einem Element aus, wie in der folgenden Abbildung dargestellt.

![Abbildung eines Shader-Konstantenpuffers](images/d3d10-shader-resource-buffer.png)

Jedes Element speichert eine Konstante mit ein bis vier Komponenten, die durch das Format der gespeicherten Daten bestimmt wird.

Ein Konstantenpuffer kann nur ein einziges Bindungsflag verwenden, das nicht mit einem anderen Bindungsflag kombiniert werden kann.

Verwenden Sie eine HLSL-Ladefunktion, um einen Shader-Konstantenpuffer aus einen Shader auszulesen. Jede Shader-Phase ermöglicht bis zu 15 Shader-Konstantenpuffer; Jeder Puffer kann bis zu 4096 Konstanten vorhalten.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Vertex- und Indexpuffer](vertex-and-index-buffers.md)

 

 




