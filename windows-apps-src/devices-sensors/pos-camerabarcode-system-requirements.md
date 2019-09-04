---
title: Kamera-Strichcodescanner-Systemanforderungen
description: Dieser Artikel beschreibt die Anforderungen für die Verwendung der Kamera-Strichcodescanner von einer UWP-App.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 4eed59e302bc34e93d21adef794de02427f2933e
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243270"
---
# <a name="camera-barcode-scanner-system-requirements"></a>Kamera-Strichcodescanner-Systemanforderungen
Ab Windows 10, Version 1803, können Sie Strichcodescanner über ein Standard-Kameraobjektiv von einer universellen Windows-Anwendung lesen.

## <a name="supported-windows-editions"></a>Unterstützte Windows Vista Editionen
- Windows 10 Professional im S Modus
- Windows 10 Professional
- Windows 10 Enterprise
- Windows 10 IOT Core


## <a name="webcam-requirements"></a>Webcam-Systemanforderungen
| Kategorie      | Empfehlung           | Kommentare |
| ------------- | ------------------------ | -------- |
| Fokus         | Autofokus               | Der fixierte Fokus wird jedoch nicht empfohlen |
| Auflösung    | 1920 x 1440 oder höher    | Um Kameras optimal nutzen zu können, wird eine Auflösung von 1920 x 1440 oder höher empfohlen.  Einige niedrigere Auflösungen bei Kameras können Standardstrichcodes lesen, wenn der Strichcode groß genug gedruckt wird. Barcodes mit weniger umfangreichen Elementen benötigen möglicherweise höhere Auflösungen bei einer Kamera. |
|

## <a name="see-also"></a>Siehe auch

### <a name="samples"></a>Proben

- [Beispiel für Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
