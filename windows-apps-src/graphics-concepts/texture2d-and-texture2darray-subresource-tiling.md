---
title: Unterteilung von Texture2D- und Texture2DArray-Unterressourcen
description: Diese Tabellen zeigen, wie Texture2D und Texture2DArray-unter Ressourcen nebeneinander angeordnet werden.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Unterteilung von Texture2D- und Texture2DArray-Unterressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28152f39983f4831a9efa981efcb85fb65fa0204
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156174"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Unterteilung von Texture2D- und Texture2DArray-Unterressourcen


Diese Tabellen zeigen, wie [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) und [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) -unter Ressourcen nebeneinander angeordnet werden. Die Werte in diesen Tabellen zählen nicht zum Ende der MIP-Verpackung.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Unter Ressourcen mit Multisampling-Anzahl von 1


Diese Tabelle zeigt, wie die unter Ressourcen [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) und [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) mit der Multisampling-Anzahl 1 nebeneinander dargestellt werden.

| Bits/Pixel (1 Stichprobe/Pixel) | Kachel Dimensionen (Pixel, WxH) |
|-----------------------------|-------------------------------|
| 8                           | 256x256                       |
| 16                          | Größe 256x128                       |
| 32                          | 128x128                       |
| 64                          | 128 x 64                        |
| 128                         | 64 x 64                         |
| BC1, 4                       | 512 x 256                       |
| BC2, 3, 5, 6, 7                 | 256x256                       |

 

Die für streamingingressourcen nicht unterstützten Formatbit-Zahlen sind 96 bpp-Formate, Videoformate, DXGI- \_ Format \_ R1 \_ unorm, DXGI- \_ Format \_ R8G8 \_ B8G8 \_ unorm und DXGI- \_ Format \_ R8R8 \_ G8B8 \_ unorm.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Unter Ressourcen mit verschiedenen Multisampling-Zählungen


Diese Tabelle zeigt, wie [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) und [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) -unter Ressourcen mit verschiedenen Multisampling-Zählungen nebeneinander angeordnet werden.

| Bits/Pixel (1 Stichprobe/Pixel) | Kachel Dimensionen (Pixel, WxH) |
|-----------------------------|-------------------------------|
| 1                           | 1x1                           |
| 2                           | 2x1                           |
| 4                           | 2 x 2                           |
| 8                           | 4x2                           |
| 16                          | 4x4                           |

 

Nur die Stichproben Anzahl 1 und 4 sind erforderlich (und zulässig), damit Sie mit Streaming-Ressourcen unterstützt werden. Streaming-Ressourcen unterstützen derzeit nicht 2, 8 und 16, obwohl Sie angezeigt werden.

Implementierungen können für nicht Streaming-Ressourcen Unterstützung von 2, 8 und/oder 16 Sample Multisampling Antialiasing (MSAA)-Modus für nicht Streaming-Ressourcen auswählen, auch wenn Sie von der streamingressource nicht unterstützt werden.

Streaming-Ressourcen mit einer Stichproben Anzahl von mehr als 1 können 128 bpp-Formate nicht verwenden.

Die Einschränkungen für unterstützte Stichproben Anzahl und-Formate sind auf Hardware Inkonsistenzen der Spezifikation zurückzuführen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[So unterteilen Sie den Bereich einer Streamingressource](how-a-streaming-resource-s-area-is-tiled.md)

 

 