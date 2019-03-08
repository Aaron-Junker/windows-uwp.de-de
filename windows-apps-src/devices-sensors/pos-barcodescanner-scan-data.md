---
title: Abrufen und Verstehen von Strichcode-Daten
description: Informationen Sie zum Abrufen und Interpretieren der Barcodedaten, die Sie überprüfen.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ece246ffd369ee21c089598f07b2566424757f55
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605965"
---
# <a name="obtain-and-understand-barcode-data"></a>Abrufen und Verstehen von Strichcode-Daten

Nachdem Sie den Barcodescanner eingerichtet haben, benötigen Sie natürlich eine Möglichkeit zum Verständnis der Daten, die Sie suchen. Beim Scannen eines Barcodes, die ["DataReceived"](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) Ereignis wird ausgelöst. Die [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) sollte dieses Ereignis abonnieren. Die **"DataReceived"** Ereignis übergibt eine [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) -Objekt, das Sie verwenden können, auf die Barcodedaten zugreifen.

## <a name="subscribe-to-the-datareceived-event"></a>Abonnieren des Ereignisses "DataReceived"

Nachdem Sie haben eine **ClaimedBarcodeScanner**, Sie abonnieren das **"DataReceived"** Ereignis:

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

Der Ereignishandler übergeben die **ClaimedBarcodeScanner** und **BarcodeScannerDataReceivedEventArgs** Objekt. Sie können die Barcodedaten zugreifen, mithilfe dieses Objekts [Bericht](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) -Eigenschaft, die vom Typ [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Abrufen der Daten

Nachdem Sie haben die **BarcodeScannerReport**, zugreifen und die Barcodedaten analysiert werden können. **BarcodeScannerReport** verfügt über drei Eigenschaften:

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): Die vollständige, unformatierte Barcodedaten.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): Der decodierte barcodeetiketts, der nicht den Header, Checksum und sonstige Informationen enthält.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): Der Typ des decodierten Barcode-Bezeichnung. Mögliche Werte werden definiert, der [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) Klasse.

Wenn Sie darauf zugreifen möchten **ScanDataLabel** oder **ScanDataType**, müssen Sie zunächst festlegen [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) zu **"true"**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Ruft den Typ der Überprüfung Daten

Abrufen des Typs der decodierte Barcode-Bezeichnung ist ganz einfach&mdash;rufen wir einfach [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) auf **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Die überprüfungsbezeichnung für die Daten zu erhalten

Um die decodierte barcodeetiketts zu erhalten, gibt es einige Dinge, denen Sie berücksichtigen müssen. Nur bestimmte Datentypen codierten Text enthalten, sodass Sie zuerst überprüfen sollten, wenn die Symbologie in eine Zeichenfolge konvertiert werden kann, und konvertieren dann den Puffer, die wir von erhalten **ScanDataLabel** auf UTF-8 codierte Zeichenfolge.

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

### <a name="get-the-raw-scan-data"></a>Rufen Sie die Rohscan-Daten

Rufen Sie die vollständige unformatierten Daten aus den Barcode konvertieren wir einfach den Puffer, die wir von erhalten **ScanData** in eine Zeichenfolge.

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

Diese Daten sind in der Regel im Format wie die Überprüfung übermittelt. Nachrichteninformationen Header- und nachspannsegmente werden entfernt, aber da sie enthalten keine nützlichen Informationen für eine Anwendung, und wahrscheinlich Scanner-spezifische.

Allgemeine Headerinformationen ist ein Präfixzeichen (z. B. ein STX-Zeichen). Allgemeine nachspanninformationen ist ein Abschlusszeichen (z. B. ein ETX oder CR-Zeichen) und ein Block-Check-Zeichen, wenn eine vom Scanner generiert wird.

Diese Eigenschaft sollte ein Symbologie Zeichen enthalten, wenn eine von der Überprüfung zurückgegeben wird (z. B. eine **ein** für UPC-A). Es sollte auch Kontrollkästchen Ziffern enthalten, wenn sie in der Bezeichnung vorhanden sind und von der Überprüfung zurückgegeben. (Beachten Sie, dass sowohl die Symbologie Zeichen als auch die Prüfziffern können möglicherweise nicht vorhanden ist, abhängig von der Konfiguration der Überprüfung. Die Überprüfung wird zurücksenden, wenn vorhanden, jedoch nicht generiert oder berechnen lassen, wenn sie nicht vorhanden sind.)

Einige Waren kann mit einem zusätzlichen Barcode gekennzeichnet werden. Dieser Barcode befindet sich rechts neben der wichtigsten Barcode in der Regel, und es besteht eine zusätzliche zwei oder fünf Zeichen von Informationen aus. Wenn die Überprüfung waren liest, die Haupt- und zusätzliche Barcodes enthält, die ergänzenden Zeichen werden die wichtigsten Zeichen angehängt und das Ergebnis wird als eine Bezeichnung an die Anwendung übermittelt. (Beachten Sie, dass eine Überprüfung möglicherweise eine Konfiguration unterstützt, die aktiviert oder deaktiviert das Lesen des zusätzlichen Codes.)

Einige waren möglicherweise mit mehreren Beschriftungen versehen, bezeichnet gekennzeichnet *mit mehreren Symbolen Bezeichnungen* oder *Ebenen Bezeichnungen*. Diese Barcodes in der Regel vertikal angeordnet werden, und sein, die dasselbe oder ein anderes Symbologie. Wenn die Überprüfung waren, die mehrere Bezeichnungen enthält liest, wird jede Barcode an die Anwendung als eine separate Bezeichnung übermittelt. Dies ist aufgrund des aktuellen Mangels von Standardisierung dieser Barcode Typen erforderlich. Eine kann nicht auf allen Varianten, die basierend auf den einzelnen Barcode-Daten zu bestimmen. Aus diesem Grund muss die Anwendung zu bestimmen, wenn mehrere Bezeichnung Barcodes basierend auf den zurückgegebenen Daten gelesen wurde. (Beachten Sie, dass ein Scanner kann möglicherweise den Lesevorgang für mehrere Bezeichnungen nicht unterstützt.)

Dieser Wert wird festgelegt, vor einem **"DataReceived"** -Ereignis ausgelöst wird, um die Anwendung.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Siehe auch
* [Barcodescanner](pos-barcodescanner.md)
* [ClaimedBarcodeScanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
