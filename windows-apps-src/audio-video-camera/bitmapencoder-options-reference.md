---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: In diesem Artikel sind die Codierungsoptionen aufgeführt, die mit BitmapEncoder verwendet werden können.
title: Referenz zu BitmapEncoder-Optionen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 212f405c943354618a4ae2bf6f2ab9c4925e7085
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161084"
---
# <a name="bitmapencoder-options-reference"></a>Referenz zu BitmapEncoder-Optionen


In diesem Artikel werden die Codierungsoptionen aufgeführt, die mit [**bitmapcoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder)verwendet werden können. Eine Codierungsoption wird durch ihren Namen (eine Zeichenfolge) und einen Wert mit einem bestimmten Datentyp definiert ([**Windows.Foundation.PropertyType**](/uwp/api/Windows.Foundation.PropertyType)). Weitere Informationen zur Verwendung von Bildern finden Sie unter [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md).

| Name                    | PropertyType | Hinweise zur Verwendung                                                                                        | Gültige Formate |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | Gültige Werte von 0 bis 1,0 Höhere Werte bedeuten höhere Qualität                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | Gültige Werte von 0 bis 1,0 Höhere Werte bedeuten ein effizienteres und langsameres Komprimierungsverfahren | TIFF          |
| Lossless                | boolean      | Wenn dieser Wert auf „true“ festgelegt ist, wird die Option „ImageQuality“ ignoriert.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Gibt an, ob der Interlacemodus für das Bild verwendet wird                                                                    | PNG           |
| FilterOption            | uint8        | Verwenden der [**pngfiltermode**](/uwp/api/Windows.Graphics.Imaging.PngFilterMode) -Enumeration                                | PNG           |
| TiffCompressionMethod   | uint8        | Verwenden Sie die [**TiffCompressionMode**](/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode)-Enumeration.                    | TIFF          |
| Luminance               | uint32Array  | Ein Array mit 64 Elementen, das die Quantifizierungskonstanten für die Leuchtdichte enthält                               | JPEG          |
| Chrominance             | uint32Array  | Ein Array mit 64 Elementen, das die Quantifizierungskonstanten für die Chrominanz enthält                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Verwenden Sie die [**JpegSubsamplingMode**](/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode)-Enumeration                    | JPEG          |
| SuppressApp0            | boolean      | Gibt an, ob die Erstellung eines App0-Metadatenblocks unterdrückt wird                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Gibt an, ob die Codierung als Version 5 des BMP-Formats erfolgen soll, die Alphawerte unterstützt.                                         | BMP           |

 

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md)
* [Unterstützte Codecs](supported-codecs.md)

 