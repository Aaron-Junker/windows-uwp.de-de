---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: In diesem Artikel sind die Codierungsoptionen aufgeführt, die mit BitmapEncoder verwendet werden können.
title: Referenz zu BitmapEncoder-Optionen
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 34e2ac1531b496f076abbe8e23ffa1c0cb318373
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359052"
---
# <a name="bitmapencoder-options-reference"></a>Referenz zu BitmapEncoder-Optionen


In diesem Artikel sind die Codierungsoptionen aufgeführt, die mit [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) verwendet werden können. Eine Codierungsoption wird durch ihren Namen (eine Zeichenfolge) und einen Wert mit einem bestimmten Datentyp definiert ([**Windows.Foundation.PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType)). Weitere Informationen zur Verwendung von Bildern finden Sie unter [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md).

| Name                    | PropertyType | Verwendungshinweise                                                                                        | Gültige Formate |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | Gültige Werte von 0 bis 1,0 Höhere Werte bedeuten höhere Qualität                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | Gültige Werte von 0 bis 1,0 Höhere Werte bedeuten ein effizienteres und langsameres Komprimierungsverfahren | TIFF          |
| Lossless                | boolean      | Wenn dieser Wert auf „true“ festgelegt ist, wird die Option „ImageQuality“ ignoriert.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Gibt an, ob der Interlacemodus für das Bild verwendet wird                                                                    | PNG           |
| FilterOption            | uint8        | Verwenden Sie die [**PngFilterMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.PngFilterMode)-Enumeration.                                | PNG           |
| TiffCompressionMethod   | uint8        | Verwenden Sie die [**TiffCompressionMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode)-Enumeration.                    | TIFF          |
| Luminance               | uint32Array  | Ein Array mit 64 Elementen, das die Quantifizierungskonstanten für die Leuchtdichte enthält                               | JPEG          |
| Chrominance             | uint32Array  | Ein Array mit 64 Elementen, das die Quantifizierungskonstanten für die Chrominanz enthält                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Verwenden Sie die [**JpegSubsamplingMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode)-Enumeration                    | JPEG          |
| SuppressApp0            | boolean      | Gibt an, ob die Erstellung eines App0-Metadatenblocks unterdrückt wird                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Gibt an, ob die Codierung als Version 5 des BMP-Formats erfolgen soll, die Alphawerte unterstützt.                                         | BMP           |

 

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md)
* [Unterstützte Codecs](supported-codecs.md)

 




