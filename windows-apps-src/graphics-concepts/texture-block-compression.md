---
title: Texturblockkomprimierung
description: Block Komprimierungs Unterstützung (BC) für Texturen wurde in Direct3D 11 erweitert, um die BC6H-und bzw bc7-Algorithmen einzubeziehen.
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- Texturblockkomprimierung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06c132f23d22dc688f82ff7976bcb93108f2ee1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158814"
---
# <a name="texture-block-compression"></a>Texturblockkomprimierung


Block Komprimierungs Unterstützung (BC) für Texturen wurde in Direct3D 11 erweitert, um die BC6H-und bzw bc7-Algorithmen einzubeziehen. BC6H unterstützt Datenquellen Daten mit hoher dynamischer Bereichs Farbe, und bzw bc7 bietet eine bessere Qualitäts Komprimierung mit weniger Artefakten für Standard-RGB-Quelldaten.

Spezifischere Informationen zur Unterstützung von Block Komprimierungs Algorithmen vor Direct3D 11, einschließlich der Unterstützung für die BC1 bis BC5-Formate, finden Sie unter [Block Komprimierung (Direct3D 10)](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-block-compression).

**Beachten Sie die folgenden Dateiformate:  ** Die Textur Komprimierungs Formate BC6H und bzw bc7 verwenden das DDS-Dateiformat zum Speichern der komprimierten Textur Daten. Weitere Informationen finden Sie im [Programmier Handbuch für DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) .

## <a name="span-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>In Direct3D 11 unterstützte Block Komprimierungs Formate


| Quelldaten                                  | Mindestens erforderliche Daten Komprimierungs Auflösung                              | Empfohlenes Format | Unterstützte Mindestfunktionsebene |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| Drei-Kanal-Farbe mit Alphakanal       | Drei Farbkanäle (5 Bits: 6 Bits: 5 Bits) mit 0 oder 1 Bit (s) von Alpha  | BC1                | Direct3D 9,1                    |
| Drei-Kanal-Farbe mit Alphakanal       | Drei Farbkanäle (5 Bits: 6 Bits: 5 Bits) mit 4 Bits Alpha         | BU2                | Direct3D 9,1                    |
| Drei-Kanal-Farbe mit Alphakanal       | Drei Farbkanäle (5 Bits: 6 Bits: 5 Bits) mit 8 Bits von Alpha          | BC3                | Direct3D 9,1                    |
| One-Channel-Farbe                            | Ein Farbkanal (8 Bits)                                                | BC4                | Direct3D 10                     |
| Farbe mit zwei Kanälen                            | Zwei Farbkanäle (8 Bits: 8 Bits)                                        | BC5                | Direct3D 10                     |
| Breite des dynamischen Bereichs mit drei Kanälen (HDR) | Drei Farbkanäle (16 Bits: 16 Bits: 16 Bits) im "halben" Gleit Komma\* | BC6H               | Direct3D 11                     |
| Drei-Kanal-Farbe, Alphakanal optional  | Drei Farbkanäle (4 bis 7 Bits pro Kanal) mit 0 bis 8 Bits von Alpha  | Bzw bc7                | Direct3D 11                     |

 

\*"Half" Floating Point ist ein 16-Bit-Wert, der aus einem optionalen Signier Bit, einem 5-Bit-Exponenten Exponenten und einer 10-oder 11-Bit-Mantisse besteht.
## <a name="span-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1-, BC2-und B3-Formate


Die BC1-, BC2-und BC3-Formate entsprechen den Direct3D 9 DXTn-Textur Komprimierungs Formaten und sind identisch mit den entsprechenden Formaten Direct3D 10 BC1, BC2 und BC3. Die Unterstützung für diese drei Formate ist für alle featureebenen erforderlich (D3D \_ Feature \_ Level \_ 9 \_ 1, D3D \_ Feature \_ Level \_ 9 \_ 2, D3D \_ Feature \_ Level \_ 9 \_ 3, D3D \_ Feature \_ Level \_ 10 \_ 0, D3D \_ Feature \_ Level \_ 10 \_ 1 und D3D \_ Feature \_ Level \_ 11 \_ 0).

| Block Komprimierungs Format | DXGI-Format                                                                           | Direct3D 9 entsprechendes Format                               | Bytes pro 4X4-Pixel Block |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI- \_ Format \_ BC1 \_ unorm, DXGI- \_ Format \_ BC1 \_ unorm \_ sRGB, DXGI- \_ Format \_ BC1 \_ typlose | D3DFMT \_ DXT1, FourCC = "DXT1"                                | 8                         |
| BU2                      | DXGI- \_ Format \_ BC2 \_ unorm, DXGI- \_ Format \_ BC2 \_ unorm \_ sRGB, DXGI- \_ Format \_ BC2 \_ typlose | D3DFMT \_ DXT2 \* , FourCC = "DXT2", D3DFMT \_ DXT3, FourCC = "DXT3" | 16                        |
| BC3                      | DXGI- \_ Format \_ BC3 \_ unorm, DXGI- \_ Format \_ BC3 \_ unorm \_ sRGB, DXGI- \_ Format \_ BC3 \_ typlose | D3DFMT \_ DXT4 \* , FourCC = "DXT4", D3DFMT \_ DXT5, FourCC = "DXT5" | 16                        |

 

\*Diese Komprimierungs Schemas (DXT2 und DXT4) unterscheiden sich nicht zwischen den vorab multiplizierten Alpha Formaten Direct3D 9 und den Alpha Formaten Standard. Dieser Unterschied muss von den programmierbaren Shadern zur Rendering-Zeit behandelt werden.

## <a name="span-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4-und BC5-Formate


| Block Komprimierungs Format | DXGI-Format                                                                     | Direct3D 9 entsprechendes Format | Bytes pro 4X4-Pixel Block |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI- \_ Format \_ BC4 \_ unorm, DXGI- \_ Format \_ BC4 \_ snorm, DXGI- \_ Format \_ BC4 \_ typlose | FourCC = "ATI1"                | 8                         |
| BC5                      | DXGI- \_ Format \_ BC5 \_ unorm, DXGI- \_ Format \_ BC5 \_ snorm, DXGI- \_ Format \_ BC5 \_ typlose | FourCC = "ati2"                | 16                        |

 

## <a name="span-idbc6h_formatspanspan-idbc6h_formatspanspan-idbc6h_formatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H-Format


Ausführlichere Informationen zu diesem Format finden Sie in der Dokumentation zum [BC6H-Format](/windows/desktop/direct3d11/bc6h-format) .

| Block Komprimierungs Format | DXGI-Format                                                                      | Direct3D 9 entsprechendes Format | Bytes pro 4X4-Pixel Block |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI- \_ Format \_ BC6H \_ UF16, DXGI- \_ Format \_ BC6H \_ SF16, DXGI- \_ Format \_ BC6H \_ typlose | Nicht zutreffend                          | 16                        |

 

Das BC6H-Format kann für jeden 4 x 4-Pixel Block verschiedene Codierungs Modi auswählen. Es sind insgesamt 14 verschiedene Codierungs Modi verfügbar, von denen jedes in der resultierenden visuellen Qualität der angezeigten Textur etwas andere Kompromisse hat. Die Auswahl der Modi ermöglicht eine schnelle Decodierung durch die Hardware, bei der die Qualitätsstufe entsprechend dem Quell Inhalt ausgewählt oder angepasst wird, aber auch die Komplexität des Suchraums erheblich erhöht.

## <a name="span-idbc7_formatspanspan-idbc7_formatspanspan-idbc7_formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>Bzw bc7-Format


Ausführlichere Informationen zu diesem Format finden Sie in der Dokumentation zum [bzw bc7-Format](/windows/desktop/direct3d11/bc7-format) .

| Block Komprimierungs Format | DXGI-Format                                                                           | Direct3D 9 entsprechendes Format | Bytes pro 4X4-Pixel Block |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| Bzw bc7                      | DXGI- \_ Format \_ bzw bc7 \_ unorm, DXGI- \_ Format \_ bzw bc7 \_ unorm \_ sRGB, DXGI- \_ Format \_ bzw bc7 \_ typlose | Nicht zutreffend                          | 16                        |

 

Das bzw bc7-Format kann für jeden 4 x 4-Pixel Block verschiedene Codierungs Modi auswählen. Es sind insgesamt 8 verschiedene Codierungs Modi verfügbar, von denen jede in der resultierenden visuellen Qualität der angezeigten Textur etwas abweicht. Die Auswahl der Modi ermöglicht eine schnelle Decodierung durch die Hardware, bei der die Qualitätsstufe entsprechend dem Quell Inhalt ausgewählt oder angepasst wird, aber auch die Komplexität des Suchraums erheblich erhöht.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Anhänge](appendix.md)

[Texturen](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 