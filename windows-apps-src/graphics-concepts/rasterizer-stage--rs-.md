---
title: Rasterizerphase (RS)
description: Der Rasterizer beschneidet Grundtypen, die nicht angezeigt werden, bereitet Grundtypen für die Pixelshaderphase (PS) vor und bestimmt, wie Pixelshader aufgerufen werden.
ms.assetid: 7E80724B-5696-4A99-91AF-49744B5CD3A9
keywords:
- Rasterizerphase (RS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 499d361bddbecef50ee8cdf44b56530a98cfccd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627585"
---
# <a name="rasterizer-rs-stage"></a>Rasterizerphase (RS)


Der Rasterizer beschneidet Grundtypen, die nicht angezeigt werden, bereitet Grundtypen für die [Pixelshaderphase (PS)](pixel-shader-stage--ps-.md) vor und bestimmt, wie Pixelshader aufgerufen werden. Die Rasterizerphase konvertiert Vektorinformationen (bestehend aus Formen oder Grundtypen) in ein Rasterbild (bestehend aus Pixeln) zum Anzeigen von 3D-Grafiken in Echtzeit.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Zweck und Verwendung


Im Verlauf der Rasterung wird jeder Grundtyp in Pixel konvertiert, wobei Pro-Vertex-Werte für jeden Grundtyp interpoliert werden. Bei der Rasterung erfolgt der Beschnitt von Vertices auf das Frustum, eine Division durch Z zum Erhalt der Perspektive, die Zuordnung von Grundtypen zu einem 2D Viewport und das Festlegen, wie der Pixelshader aufgerufen werden soll. Das Verwenden eines Pixelshader ist optional, aber die Rasterizerphase umfasst immer einen Beschnitt, eine perspektivische Division, um die Punkte in homogenen Raum zu transformieren Rasterizer-Stufe immer und ordnet die Scheitelpunkte der Viewport.

Sie können die Rasterung deaktivieren, indem Sie der Pipeline sagen, dass kein Pixelshader vorhanden ist (Einstellen der [Pixelshaderphase (PS)](pixel-shader-stage--ps-.md) auf **NULL** und Deaktivieren von Tiefen- und Schablonentests). Wenn die Rasterung deaktiviert ist, werden damit zusammenhängende Pipelinezähler nicht aktualisiert.

Bei Hardware, die hierarchische Z-Puffer-Optimierungen implementiert, können Sie das Vorabladen des Z-Puffers aktivieren, indem Sie die Pixelshaderphase (PS) auf **NULL** stellen und Tiefen- und Schablonentests aktivieren.

Siehe [Regeln für die Rasterung](rasterization-rules.md).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Eingabe


Vertices (x,y,z,w), die in die Rasterizerphase gelangen, werden als in einem homogenen Clipraum angesehen. In diesem Koordinatenraum zeigt die X-Achse nach rechts, die Y-Achse nach oben und die Z-Achse von der Kamera weg.

Die Rasterizerphase (RS) ist eine feste Funktion und speist sich aus der Streamausgabephase und/oder der vorherigen Pipelinephase, z. B. der [Geometrieshaderphase (GS)](geometry-shader-stage--gs-.md). Wenn die GS nicht verwendet wird, wird die RS aus der [Domänenshaderphase (DS)](domain-shader-stage--ds-.md) gespeist. Wenn auch die DS nicht verwendet wird, speist sich die RS aus der [Vertexshaderphase (VS)](vertex-shader-stage--vs-.md).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Ausgabe


Die Verwendung der Pixelshaderphase (PS) ist optional. Von der Rasterizerphase kann die Ausgabe stattdessen direkt an die [Ausgabemergerphase (OM)](output-merger-stage--om-.md) erfolgen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Rasterization Regeln](rasterization-rules.md)

[Grafikpipeline](graphics-pipeline.md)

 

 




