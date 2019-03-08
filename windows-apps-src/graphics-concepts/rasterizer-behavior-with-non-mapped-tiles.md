---
title: Rasterizerverhalten bei nicht zugeordneten Kacheln
description: Dieser Abschnitt beschreibt Rasterizerverhalten bei nicht zugeordneten Kacheln.
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords:
- Rasterizerverhalten bei nicht zugeordneten Kacheln
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e3089444820f990644526eaafb7f2ef9007fa70a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631885"
---
# <a name="span-iddirect3dconceptsrasterizerbehaviorwithnon-mappedtilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>Verhalten des Rasterizers, mit Kacheln, die nicht zugeordnete


Dieser Abschnitt beschreibt Rasterizerverhalten bei nicht zugeordneten Kacheln.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


Verhalten der DSV-Lese- und Schreibvorgänge (Depth Stencil View) hängt von der Ebene der Hardwareunterstützung ab. Eine Aufschlüsselung der Anforderungen finden Sie im Thema zum allgemeinen Lese- und Schreibverhalten unter [Ebenen der Features von Streamingressourcen](streaming-resources-features-tiers.md).

Hier ist das ideale Verhalten:

Wenn eine Kachel in der DepthStencilView-Ansicht nicht zugeordnet wird, ist der Rückgabewert aus dem Lesen der Tiefe 0, der dann in die Vorgänge, die für den Tiefenlesewert konfiguriert sind, übergeben wird. Schreibvorgänge in die fehlende Tiefenkachel werden gelöscht. Diese ideale Definition für die Behandlung von Schreibvorgängen ist für [Ebene 2](tier-2.md) nicht erforderlich. Schreibvorgänge an nicht zugeordnete Kacheln können zu einem Cache führen, den nachfolgende Lesevorgänge aufnehmen können.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


Verhalten der RTV-Lese- und Schreibvorgänge (Render Target View, Renderzielansicht) hängt von der Ebene der Hardwareunterstützung ab. Eine Aufschlüsselung der Anforderungen finden Sie im Thema zum allgemeinen Lese- und Schreibverhalten unter [Ebenen der Features von Streamingressourcen](streaming-resources-features-tiers.md).

Für alle Implementierungen können verschiedene RTVs (und DSVs), die gleichzeitig gebunden werden, verschiedene zugeordnete und nicht zugeordnete Bereiche sowie Oberflächenformate unterschiedlicher Größen (was unterschiedliche Kachelformen bedeutet) aufweisen.

Hier ist das ideale Verhalten:

Lesevorgänge aus RTVs geben 0 für fehlende Kacheln zurück, und Schreibvorgänge werden gelöscht. Diese ideale Definition für die Behandlung von Schreibvorgängen ist für [Ebene 2](tier-2.md) nicht erforderlich. Schreibvorgänge an nicht zugeordnete Kacheln können zu einem Cache führen, den nachfolgende Lesevorgänge aufnehmen können.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Pipeline-Zugriff auf Ressourcen streaming](pipeline-access-to-streaming-resources.md)

 

 




