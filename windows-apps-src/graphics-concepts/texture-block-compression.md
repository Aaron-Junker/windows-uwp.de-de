---
title: Texturblockkomprimierung
description: Die Unterstützung der Blockkomprimierung (BC) für Texturen wurde in Direct3D 11 erweitert, um BC6H- und BC7-Algorithmen anzubieten.
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- Texturblockkomprimierung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8d88ad4db56b40490f2fc4d034b1569eb0850ba
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370924"
---
# <a name="texture-block-compression"></a>Texturblockkomprimierung


Die Unterstützung der Blockkomprimierung (BC) für Texturen wurde in Direct3D 11 erweitert, um BC6H- und BC7-Algorithmen anzubieten. BC6H unterstützt HDR-Farb-Quelldaten und BC7 bietet überdurchschnittliche Qualität bei der Komprimierung mit weniger Artefakte für Standard-RGB-Quelldaten.

Weitere Informationen zur Unterstützung des Blockkomprimierungsalgorithmus vor Direct3D 11, einschließlich der Unterstützung für die Formate BC1 bis BC5 Formate, finden Sie unter [Blockkomprimierung (Direct3D 10)](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-block-compression).

**Hinweis zur Dateiformate:  ** Die Formate BC6H und BC7 Textur Komprimierung verwenden das DDS-Dateiformat zum Speichern der texturdaten für die komprimierte. Weitere Informationen finden Sie in der [Programmieranleitung für DDS](https://docs.microsoft.com/windows/desktop/direct3ddds/dx-graphics-dds-pguide).

## <a name="span-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>In Direct3D unterstützt verschiedener Komprimierungsformate blockieren 11


| Quelldaten                                  | Erforderliche Mindestauflösung für die Datenkomprimierung                              | Empfohlenes Format | Minimaler unterstützte Featureebene |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| Drei-Kanal-Farbe mit Alphakanal       | Drei Farbkanäle (5 Bit:6 Bit:5 Bit) mit 0 oder 1 Bit Alpha  | BU1                | Direct3D 9.1                    |
| Drei-Kanal-Farbe mit Alphakanal       | Drei Farbkanäle (5 Bit:6 Bit:5 Bit) mit 4 Bit Alpha         | BU2                | Direct3D 9.1                    |
| Drei-Kanal-Farbe mit Alphakanal       | Drei Farbkanäle (5 Bit:6 Bit:5 Bit) mit 8 Bit Alpha          | BU3                | Direct3D 9.1                    |
| Ein-Kanal-Farbe                            | Ein Farbkanal (8 Bit)                                                | BC4                | Direct3D 10                     |
| Zwei-Kanal-Farbe                            | Zwei Farbkanäle (8 Bit:8 Bit)                                        | BC5                | Direct3D 10                     |
| Drei-Kanal-HDR- (High Dynamic Range) Farbe | Drei Farbkanäle (16 Bits: 16 Bits: 16 Bits) in "halbe" Gleitkommazahl\* | BC6H               | Direct3D 11                     |
| Drei-Kanal-Farbe, Alphakanal optional  | Drei Farbkanäle (4 bis 7 Bit pro Kanal) mit 0 bis 8 Bit für Alpha  | BC7                | Direct3D 11                     |

 

\*"Halbe" floating Point ist ein 16-Bit-Wert, der besteht aus einem optionalen Vorzeichenbit, 5 etwas parteiisch, Exponent, und eine 10 oder 11-Bit-Mantisse.
## <a name="span-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1, BC2 und B3-Formaten


Die Formate BC1, BC2 und BC3 entsprechen den Direct3D 9 DXTn-Texturkomprimierungsformaten und sind mit den entsprechenden Direct3D 10-Formaten BC1, BC2 und BC3 identisch. Unterstützung für diese drei Formate ist erforderlich, von allen Funktionsebenen (D3D\_FEATURE\_Ebene\_9\_1, D3D\_FEATURE\_Ebene\_9\_2, D3D\_ FEATURE\_Ebene\_9\_3, D3D\_FEATURE\_Ebene\_10\_0 (null) D3D\_FEATURE\_Ebene\_10 \_1 und D3D\_FEATURE\_Ebene\_11\_0).

| Blockkomprimierungsformat | DXGI-Format                                                                           | Äquivalentes Direct3D 9-Format                               | Byte pro 4 x 4-Pixelblock |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BU1                      | DXGI\_FORMAT\_BC1\_UNORM, DXGI\_FORMAT\_BC1\_UNORM\_SRGB, DXGI\_FORMAT\_BC1\_TYPELESS | D3DFMT\_DXT1, FourCC="DXT1"                                | 8                         |
| BU2                      | DXGI\_FORMAT\_BC2\_UNORM, DXGI\_FORMAT\_BC2\_UNORM\_SRGB, DXGI\_FORMAT\_BC2\_TYPELESS | D3DFMT\_DXT2\*, FourCC="DXT2", D3DFMT\_DXT3, FourCC="DXT3" | 16                        |
| BU3                      | DXGI\_FORMAT\_BC3\_UNORM, DXGI\_FORMAT\_BC3\_UNORM\_SRGB, DXGI\_FORMAT\_BC3\_TYPELESS | D3DFMT\_DXT4\*, FourCC="DXT4", D3DFMT\_DXT5, FourCC="DXT5" | 16                        |

 

\*Diese Komprimierungsschemas (für DXT2 und für DXT4) stellen keinen Unterschied zwischen der Direct3D 9 vorab multipliziertes alpha-Formate und die standard-alpha-Formate. Diese Unterscheidung muss zum Zeitpunkt des Renderns von den programmierbaren Shadern berücksichtigt werden.

## <a name="span-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4 "und" BC5-Formate


| Blockkomprimierungsformat | DXGI-Format                                                                     | Äquivalentes Direct3D 9-Format | Byte pro 4 x 4-Pixelblock |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI\_FORMAT\_BC4\_UNORM, DXGI\_FORMAT\_BC4\_SNORM, DXGI\_FORMAT\_BC4\_TYPELESS | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI\_FORMAT\_BC5\_UNORM, DXGI\_FORMAT\_BC5\_SNORM, DXGI\_FORMAT\_BC5\_TYPELESS | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6hformatspanspan-idbc6hformatspanspan-idbc6hformatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H-Format


Ausführlichere Informationen über dieses Format finden Sie in der Dokumentation zum [BC6H-Format](https://docs.microsoft.com/windows/desktop/direct3d11/bc6h-format).

| Blockkomprimierungsformat | DXGI-Format                                                                      | Äquivalentes Direct3D 9-Format | Byte pro 4 x 4-Pixelblock |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI\_FORMAT\_BC6H\_UF16, DXGI\_FORMAT\_BC6H\_SF16, DXGI\_FORMAT\_BC6H\_TYPELESS | Nicht zutreffend                          | 16                        |

 

Das Format BC6H kann für jeden 4x4-Pixel-Block unterschiedliche Kodierungsmodi auswählen. Es sind insgesamt 14 verschiedene Kodierungsmodi verfügbar, jeder davon mit unterschiedlichen Verteilungen der resultierenden visuellen Qualität und der angezeigten Textur. Die Modusauswahl ermöglicht die schnelle Dekodierung durch die Hardware mit der ausgewählten Qualitätsstufe bzw. angepasst an den jeweiligen Quellinhalt; andererseits erhöht sich dadurch die Komplexität des Suchbereichs erheblich.

## <a name="span-idbc7formatspanspan-idbc7formatspanspan-idbc7formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>"Bc7" nicht-Format


Ausführlichere Informationen über dieses Format finden Sie in der Dokumentation zum [BC7-Format](https://docs.microsoft.com/windows/desktop/direct3d11/bc7-format).

| Blockkomprimierungsformat | DXGI-Format                                                                           | Äquivalentes Direct3D 9-Format | Byte pro 4 x 4-Pixelblock |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI\_FORMAT\_BC7\_UNORM, DXGI\_FORMAT\_BC7\_UNORM\_SRGB, DXGI\_FORMAT\_BC7\_TYPELESS | Nicht zutreffend                          | 16                        |

 

Das Format BC7 kann für jeden 4x4-Pixel-Block unterschiedliche Kodierungsmodi auswählen. Es sind insgesamt 8 verschiedene Kodierungsmodi verfügbar, jeder davon mit unterschiedlichen Verteilungen der resultierenden visuellen Qualität und der angezeigten Textur. Die Modusauswahl ermöglicht die schnelle Dekodierung durch die Hardware mit der ausgewählten Qualitätsstufe bzw. angepasst an den jeweiligen Quellinhalt; andererseits erhöht sich dadurch die Komplexität des Suchbereichs erheblich.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Anhänge](appendix.md)

[Texturen](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 




