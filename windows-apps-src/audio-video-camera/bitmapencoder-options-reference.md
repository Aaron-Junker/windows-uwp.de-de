---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: In diesem Artikel sind die Codierungsoptionen aufgeführt, die mit BitmapEncoder verwendet werden können.
title: Referenz zu BitmapEncoder-Optionen
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07f5c6ef180cb4abe90a705e73be8d99ecbd2ca7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603185"
---
# <a name="bitmapencoder-options-reference"></a>Referenz zu BitmapEncoder-Optionen


In diesem Artikel sind die Codierungsoptionen aufgeführt, die mit [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) verwendet werden können. Eine Codierungsoption wird durch ihren Namen (eine Zeichenfolge) und einen Wert mit einem bestimmten Datentyp definiert ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)). Weitere Informationen zur Verwendung von Bildern finden Sie unter [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md).

| Name                    | PropertyType | Verwendungshinweise                                                                                        | Gültige Formate |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | Gültige Werte von 0 bis 1,0 Höhere Werte bedeuten höhere Qualität                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | Gültige Werte von 0 bis 1,0 Höhere Werte bedeuten ein effizienteres und langsameres Komprimierungsverfahren | TIFF          |
| Lossless                | boolean      | Wenn dieser Wert auf „true“ festgelegt ist, wird die Option „ImageQuality“ ignoriert.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Gibt an, ob der Interlacemodus für das Bild verwendet wird                                                                    | PNG           |
| FilterOption            | uint8        | Verwenden Sie die [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389)-Enumeration.                                | PNG           |
| TiffCompressionMethod   | uint8        | Verwenden Sie die [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399)-Enumeration.                    | TIFF          |
| Luminance               | uint32Array  | Ein Array mit 64 Elementen, das die Quantifizierungskonstanten für die Leuchtdichte enthält                               | JPEG          |
| Chrominance             | uint32Array  | Ein Array mit 64 Elementen, das die Quantifizierungskonstanten für die Chrominanz enthält                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Verwenden Sie die [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386)-Enumeration                    | JPEG          |
| SuppressApp0            | boolean      | Gibt an, ob die Erstellung eines App0-Metadatenblocks unterdrückt wird                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Gibt an, ob die Codierung als Version 5 des BMP-Formats erfolgen soll, die Alphawerte unterstützt.                                         | BMP           |

 

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md)
* [Unterstützte Codecs](supported-codecs.md)

 




