---
title: Unterteilung von Texture2D- und Texture2DArray-Unterressourcen
description: Diese Tabellen zeigen die Unterteilung von Texture2D- und Texture2DArray-Unterressourcen.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Unterteilung von Texture2D- und Texture2DArray-Unterressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2e193ab7bce31c1f13cb40f04902922c6ff21056
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370916"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Unterteilung von Texture2D- und Texture2DArray-Unterressourcen


Diese Tabellen zeigen die Unterteilung von [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d)- und [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray)-Unterressourcen. Die Werte in dieser Tabelle beziehen keine Tail-MIP-Verpackung mit ein.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Unterressourcen mit 1 für Multisampling


Die folgende Tabelle zeigt die Unterteilung von [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d)- und [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray)-Unterressourcen mit Multisampling (1).

| Bits/Pixel (1 Beispiel/Pixel) | Kachelabmessungen (Pixel, B x H) |
|-----------------------------|-------------------------------|
| 8                           | 256 x 256                       |
| 16                          | 256 x 128                       |
| 32                          | 128 x 128                       |
| 64                          | 128 x 64                        |
| 128                         | 64 x 64                         |
| BC1, 4                       | 512 x 256                       |
| BC2, 3, 5, 6, 7                 | 256 x 256                       |

 

Format-Bit-Anzahl für das streaming von Ressourcen nicht unterstützt werden, 96 Bit pro Pixel Formate Videoformate, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM, und DXGI\_FORMAT\_R8R8\_G8B8\_UNORM.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Unterressourcen mit verschiedenen für Multisampling


Die folgende Tabelle zeigt die Unterteilung von [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d)- und [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray)-Unterressourcen mit verschiedenen Multisampling-Anzahlen.

| Bits/Pixel (1 Beispiel/Pixel) | Kachelabmessungen (Pixel, B x H) |
|-----------------------------|-------------------------------|
| 1                           | 1 x 1                           |
| 2                           | 2 x 1                           |
| 4                           | 2 x 2                           |
| 8                           | 4 x 2                           |
| 16                          | 4 x 4                           |

 

Es sind ausschließlich Beispielanzahlen von 1 und 4 erforderlich (und zulässig), um mit Streamingressourcen unterstützt zu werden. Anzahlen von 2, 8 und 16 werden derzeit von Streamingressourcen nicht unterstützt, auch wenn sie angezeigt werden.

In Implementierungen kann der Multiple Sample Antialiasing-Modus (MSAA) mit 2, 8 und/oder 16 Beispielen für Nicht-Streamingressourcen unterstützt werden, auch wenn dies von der Streamingressource nicht unterstützt wird.

128-BpP-Formate können von Streamingressourcen mit Beispielanzahlen von mehr als 1 nicht verwendet werden.

Die Einschränkungen bei der Anzahl unterstützter Beispiele und Formate gehen auf Hardwareinkonsistenzen in der Spezifikation zurück.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Wie ein streaming Ressourcenbereich gekachelt wird](how-a-streaming-resource-s-area-is-tiled.md)

 

 




