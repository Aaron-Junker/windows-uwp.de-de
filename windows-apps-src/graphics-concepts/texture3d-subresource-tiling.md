---
title: Unterteilung von Texture3D-Unterressourcen
description: Sehen Sie sich eine Tabelle an, die zeigt, wie Texture3D-unter Berichte auf der Grundlage der Bits pro Pixel der Texture2D-tiult angezeigt werden.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Unterteilung von Texture3D-Unterressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ee4ff5c87022f9fd303b1331665a2551704cb93
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167944"
---
# <a name="texture3d-subresource-tiling"></a>Unterteilung von Texture3D-Unterressourcen


Diese Tabelle zeigt, wie [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) -unter Ressourcen nebeneinander angeordnet werden. Die Werte in dieser Tabelle zählen nicht zum Ende der MIP-Verpackung.

In dieser Tabelle wird das [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) -tichen berücksichtigt, und die x/y-Dimensionen werden um 4 dividiert, und es werden 16 Ebenen hinzugefügt. Alle Kacheln für die erste Ebene (2D-Ebene der Kacheln, die die ersten 16 Ebenen der Tiefe definieren) werden vor den nachfolgenden Ebenen angezeigt.

**Hinweis**  [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) die Unterstützung von Streamingressourcen wird in der anfänglichen Implementierung von Streamingressourcen nicht verfügbar gemacht, die gewünschten Kachel Formen werden hier jedoch für die Unterstützung in einer zukünftigen Version aufgeführt.

 

| Bits/Pixel (1 Stichprobe/Pixel) | Kachel Dimensionen (Pixel, WxHxD) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1, 4                       | 128 x 64x16                       |
| BC2, 3, 5, 6, 7                 | 64x64x16                        |

 

Die für streamingingressourcen nicht unterstützten Formatbit-Zahlen sind 96 bpp-Formate, Videoformate, DXGI- \_ Format \_ R1 \_ unorm, DXGI- \_ Format \_ R8G8 \_ B8G8 \_ unorm und DXGI- \_ Format \_ R8R8 \_ G8B8 \_ unorm.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[So unterteilen Sie den Bereich einer Streamingressource](how-a-streaming-resource-s-area-is-tiled.md)

 

 