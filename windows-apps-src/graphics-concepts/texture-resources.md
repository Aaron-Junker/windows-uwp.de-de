---
title: Texturressourcen
description: Erfahren Sie mehr über das Rendering mit Direct3D-Textur Ressourcen und darüber, wie Sie mehrere Textur Mischungen mithilfe von Textur Stufen unterstützen.
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- Texturressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d82a5525601c98812d6aab97f5f5d4399ceddc91
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156184"
---
# <a name="texture-resources"></a>Texturressourcen


Texturen sind ein Ressourcentyp, der zum Rendern verwendet wird.

## <a name="span-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>Rendering mit Textur Ressourcen


Direct3D unterstützt mehrere Textur-Mischungen durch das Konzept von Textur Stufen. Jede Textur Phase enthält eine Textur und Vorgänge, die für die Textur ausgeführt werden können. Die Texturen in den Textur Stufen bilden den Satz der aktuellen Texturen. Siehe [Textur Mischung](texture-blending.md). Der Zustand jeder Textur wird in der Textur Phase gekapselt.

Die Anwendung kann auch die Textur Perspektive und die Textur Filter Zustände festlegen. Siehe [Textur Filterung](texture-filtering.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturen](textures.md)

 

 




