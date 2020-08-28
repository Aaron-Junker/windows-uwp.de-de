---
title: Abrufen und Verstehen von Strichcode-Daten
description: Erfahren Sie, wie Sie Daten von einem Barcode Scanner in einem barcodescannerreport-Objekt abrufen und dessen Format und Inhalt verstehen.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5d2ab873774ce8b48252b82ce6cf7521165554d4
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043442"
---
# <a name="obtain-and-understand-barcode-data"></a>Abrufen und Verstehen von Strichcode-Daten

Nachdem Sie den Barcode Scanner eingerichtet haben, benötigen Sie natürlich eine Möglichkeit, die Daten zu verstehen, die Sie scannen. Wenn Sie einen Barcode Scannen, wird das [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) -Ereignis ausgelöst. Der [claimedbarcodescanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) sollte dieses Ereignis abonnieren. Das **DataReceived** -Ereignis übergibt ein [barcodescannerdatareceivedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) -Objekt, das Sie verwenden können, um auf die Barcode Daten zuzugreifen.

## <a name="subscribe-to-the-datareceived-event"></a>DataReceived-Ereignis abonnieren

Wenn Sie über ein **claimedbarcodescanner**verfügen, abonnieren Sie das **DataReceived** -Ereignis:

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

Dem Ereignishandler werden das **claimedbarcodescanner** -und das **barcodescannerdatareceivedeventargs** -Objekt übermittelt. Sie können über die [Berichts](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) Eigenschaft des-Objekts, die vom Typ " [barcodescannerreport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)" ist, auf die Barcode Daten zugreifen.

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Abrufen von Daten

Sobald Sie über den **barcodescannerreport**verfügen, können Sie auf die Barcode Daten zugreifen und diese analysieren. **Barcodescannerreport** hat drei Eigenschaften:

* [Scandata](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): die vollständigen, Rohdaten des Barcodes.
* [Scandatalabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): die decodierte Barcode Bezeichnung, die den Header, die Prüfsumme und andere sonstige Informationen nicht enthält.
* [Scandatatype](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): der Typ der decodierten Barcode Bezeichnung. Mögliche Werte sind in der [barcodesymbologien](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) -Klasse definiert.

Wenn Sie entweder auf " **scandatalabel** " oder " **scandatatype**" zugreifen möchten, müssen Sie " [isdecodedataaktivierte](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) " zuerst auf " **true**" festlegen.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Abrufen des Scan Datentyps

Das Abrufen des decodierten Barcode Bezeichnungs Typs ist recht trivial &mdash; . wir nennen einfach [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) für " **scandatatype**".

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Abrufen der Bezeichnung für Scan Daten

Um die decodierte Barcode Bezeichnung zu erhalten, gibt es einige Dinge, die Sie beachten müssen. Nur bestimmte Datentypen enthalten codierten Text. Daher sollten Sie zunächst überprüfen, ob die Symbologie in eine Zeichenfolge konvertiert werden kann, und dann den Puffer, den wir von " **scandatalabel** " erhalten, in eine codierte UTF-8-Zeichenfolge konvertieren.

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>Abrufen der Rohdaten der Überprüfung

Um die vollständigen Rohdaten des Barcodes zu erhalten, konvertieren wir einfach den Puffer, den wir von " **scandata** " erhalten, in eine Zeichenfolge.

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

Diese Daten sind im Allgemeinen im Format, das vom Scanner bereitgestellt wird. Nachrichten Header-und nach Spann Informationen werden jedoch entfernt, da Sie keine nützlichen Informationen für eine Anwendung enthalten und wahrscheinlich Scanner-spezifisch sind.

Allgemeine Header Informationen sind ein Präfix Zeichen (z. b. ein STX-Zeichen). Allgemeine Nachspann Informationen sind ein Abschluss Zeichen (z. b. ein ETX-oder CR-Zeichen) und ein Block Häkchen, wenn eine vom Scanner generiert wird.

Diese Eigenschaft sollte ein symbologiezeichen enthalten, wenn eine von der Überprüfung zurückgegeben wird (z. b **. ein-Objekt für UPC** -a). Sie sollte auch Prüfziffern enthalten, wenn Sie in der Bezeichnung vorhanden sind und von der Überprüfung zurückgegeben werden. (Beachten Sie, dass je nach Scannerkonfiguration sowohl symbologiezeichen als auch Prüfziffern vorhanden sein können. Der Scanner gibt Sie zurück, falls vorhanden, generiert oder berechnet Sie jedoch nicht, wenn Sie nicht vorhanden sind.)

Einige waren können mit einem ergänzenden Barcode gekennzeichnet werden. Dieser Barcode wird in der Regel rechts vom Haupt Barcode platziert und besteht aus zusätzlichen zwei oder fünf Zeichen. Wenn der Scanner waren liest, die sowohl die Haupt-als auch die ergänzenden Barcodes enthalten, werden die zusätzlichen Zeichen an die Haupt Zeichen angehängt, und das Ergebnis wird als eine Bezeichnung an die Anwendung übermittelt. (Beachten Sie, dass ein Scanner eine Konfiguration unterstützen kann, die das Lesen zusätzlicher Codes aktiviert oder deaktiviert.)

Einige waren können mit mehreren Bezeichnungen gekennzeichnet werden, die manchmal auch als mehrfach *Symbol Bezeichnungen oder mehr* *stufige Bezeichnungen*bezeichnet werden. Diese Barcodes sind in der Regel vertikal angeordnet und können dieselbe oder eine andere Symbologie aufweisen. Wenn der Scanner waren, die mehrere Bezeichnungen enthalten, liest, wird jeder Barcode als separate Bezeichnung an die Anwendung übermittelt. Dies ist aufgrund der aktuellen fehlenden Standardisierung dieser Barcode Typen erforderlich. Eine kann nicht alle Variationen basierend auf den einzelnen Barcode Daten bestimmen. Aus diesem Grund muss die Anwendung bestimmen, ob ein Code mit mehreren Bezeichnungen auf der Grundlage der zurückgegebenen Daten gelesen wurde. (Beachten Sie, dass ein Scanner das Lesen mehrerer Bezeichnungen möglicherweise unterstützt.)

Dieser Wert wird festgelegt, bevor ein **DataReceived** -Ereignis für die Anwendung ausgelöst wird.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Weitere Informationen
* [Barcodescanner](pos-barcodescanner.md)
* [Claimedbarcodescanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [Barcodescannerdatareceivedeventargs-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [Barcodescannerreport-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [Barcodesymbologien-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
