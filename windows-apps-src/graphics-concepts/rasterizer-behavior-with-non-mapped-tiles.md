---
title: Rasterizerverhalten bei nicht zugeordneten Kacheln
description: Erfahren Sie mehr über das Verhalten von "depthstencilview (DSV)" und "rendertargetview" (RTV) Raster mit nicht zugeordneten Kacheln.
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords:
- Rasterizerverhalten bei nicht zugeordneten Kacheln
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f8085d8d29a86c0c5da82f6cb98c57c037b81beb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171884"
---
# <a name="span-iddirect3dconceptsrasterizer_behavior_with_non-mapped_tilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>Rasterizerverhalten bei nicht zugeordneten Kacheln


In diesem Abschnitt wird das Verhalten des Rasterizers mit nicht zugeordneten Kacheln beschrieben.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>Depthstencilview


Das Verhalten von Lese-und Schreibvorgängen in der tiefen Schablone ist abhängig von der Hardwareunterstützung. Eine Aufschlüsselung der Anforderungen finden Sie unter Allgemeines Lese-und Schreibverhalten für [Streamingressourcen-Funktionsebenen](streaming-resources-features-tiers.md).

Hier ist das ideale Verhalten:

Wenn eine Kachel nicht in der depthstencilview zugeordnet ist, ist der Rückgabewert aus der Lesetiefe 0 (null), der dann in alle Vorgänge eingefügt wird, die für den Wert der tiefen Lesevorgänge konfiguriert sind. Schreibvorgänge in die Kachel "fehlende tiefe" werden gelöscht. Diese ideale Definition für die Schreib Behandlung ist für [Ebene 2](tier-2.md)nicht erforderlich. Schreibvorgänge in nicht zugeordnete Kacheln können in einem Cache landen, den nachfolgende Lesevorgänge aufnehmen können.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>Rendertargetview


Das Verhalten von Lese-und Schreibvorgängen der renderzielansicht (RTV) hängt von der Hardwareunterstützung ab. Eine Aufschlüsselung der Anforderungen finden Sie unter Allgemeines Lese-und Schreibverhalten für [Streamingressourcen-Funktionsebenen](streaming-resources-features-tiers.md).

Bei allen Implementierungen können unterschiedliche, gleichzeitig gebundene rtvs (und DSV) verschiedene Bereiche zugeordnet werden, die nicht zugeordnet sind und unterschiedliche Größen Oberflächen Formate aufweisen können (Dies bedeutet verschiedene Kachel Formen).

Hier ist das ideale Verhalten:

Lesevorgänge aus rtvs geben 0 in fehlenden Kacheln zurück, und Schreibvorgänge werden gelöscht. Diese ideale Definition für die Schreib Behandlung ist für [Ebene 2](tier-2.md)nicht erforderlich. Schreibvorgänge in nicht zugeordnete Kacheln können in einem Cache landen, den nachfolgende Lesevorgänge aufnehmen können.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Pipelinezugriff auf Streamingressourcen](pipeline-access-to-streaming-resources.md)

 

 




