---
title: Kamera-Barcode Scanner-Symbologien
description: Kamera-Barcode-Scanner unterstützte Symbologien
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 481d10f2fea076f45124a3c75819dfe6494300bf
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448400"
---
# <a name="symbologies"></a>Symbologien

Dieses Thema enthält Beispiel-Barcodes für jede der Symbologien, die von dem Software Barcode Decoder unterstützt werden, der im Lieferumfang von Windows 10 enthalten ist, einschließlich: UPC/EAN, Code 39, Code 128, Interleaved 2 von 5, DataBar omnidirektional, DataBar gestapelt, QR-Code und GS1DWCode.

Windows 10 nutzt eine Standard-Lens-Kamera in Kombination mit einem Software Decoder, um einen Barcode Scanner zu generieren. Dieser Artikel bezieht sich auf die Symbologien, die vom Software Decoder unterstützt werden. Weitere Symbologien werden möglicherweise von dedizierten Barcode Scanner-Geräten unterstützt, die über integrierte Hardware Decoder verfügen, wenden Sie sich an Ihren Barcode Scanner, um weitere Informationen zu erhalten. Die aufgeführten Symbologien werden in allen Editionen von Windows 10 Build 17134 oder höher unterstützt, sofern nicht anders angegeben.

Verwenden Sie [getsupportedsymbologiesasync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync) , um die spezifischen Symbologien zu ermitteln, die von einem Barcode Scanner unterstützt werden.

> [!NOTE]
> Der in Windows 10 integrierte Software Decoder wird von [*Digimarc Corporation*](https://www.digimarc.com/)bereitgestellt.

## <a name="1d-symbologies"></a>1D-Symbologien

### <a name="code-39"></a>Code 39
![Beispiel Barcode-Code 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>Code 128
![Beispiel Barcode-Code 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>DataBar omnidirektional
![Beispiel Barcode-DataBar omnidirektional](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>Gestapeltes DataBar
![Beispiel für Barcode-DataBar gestapelt](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![Beispiel Barcode-EAN-8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![Barcode-Beispiel für den Barcode](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>Interleaved 2 von 5
![Barcode-Interleaved 2 von 5](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![Beispiel Barcode-UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![Beispiel Barcode-UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>2D-Symbologien
### <a name="qr-code"></a>QR-Code
![Sample Barcode-QR-Code](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>Digitales Wasserzeichen
### <a name="gs1-dwcode"></a>GS1-dwcode

Scannen Sie das Image eines Pakets unten mit Ihrer Kamera Barcode Scanner-Anwendung, um GS1DWCode in Aktion zu sehen.  Das Bild wird mit upca 856107006854 codiert.  http://www.digimarc.comWeitere Informationen zu GS1DWCode-Funktionen finden Sie unter.

![Sample Barcode-GS1DWCode](images/pos/Rice-Box-V7.jpg)

## <a name="see-also"></a>Siehe auch

### <a name="samples"></a>Beispiele

- [Beispiel für Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
