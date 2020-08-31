---
title: Arbeiten mit Barcode Scanner-Symbologien
description: Verwenden Sie die UWP-Barcode Scanner-APIs, um Barcode Symbologien zu verarbeiten, ohne den Scanner manuell zu konfigurieren.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 96014dfeb0b160d9cb94afef5ba4251b3c8bf8aa
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054380"
---
# <a name="working-with-symbologies"></a>Arbeiten mit Symbologien
Eine [Barcode Symbologie](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) ist die Zuordnung von Daten zu einem bestimmten Barcode Format. Einige gängige Symbologien sind beispielsweise UPC, Code 128, QR-Code usw.  Mit den universelle Windows-Plattform Barcode Scanner-APIs kann eine Anwendung steuern, wie die Überprüfung diese Symbologien verarbeitet, ohne die Überprüfung manuell zu konfigurieren. 

## <a name="determine-which-symbologies-are-supported"></a>Bestimmen, welche Symbologien unterstützt werden 
Da Ihre Anwendung mit verschiedenen Barcode Scanner-Modellen von mehreren Herstellern verwendet werden kann, können Sie den Scanner Abfragen, um die Liste der unterstützten Symbologien zu ermitteln.  Dies kann hilfreich sein, wenn Ihre Anwendung eine bestimmte Symbologie erfordert, die möglicherweise nicht von allen Scannern unterstützt wird, oder wenn Sie Symbologien aktivieren müssen, die auf dem Scanner entweder manuell oder Programm gesteuert deaktiviert wurden.

Wenn Sie über ein [Barcodescanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) -Objekt verfügen, indem Sie " [Barcodescanner. frocenterasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)" verwenden, rufen Sie [getsupportedsymbologiesasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) auf, um eine Liste der vom Gerät unterstützten Symbologien zu erhalten.

Im folgenden Beispiel wird eine Liste der unterstützten Symbologien des Barcode Scanners abgerufen und in einem TextBlock angezeigt:

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>Bestimmen, ob eine bestimmte Symbologie unterstützt wird
Um zu ermitteln, ob der Scanner eine bestimmte Symbologie unterstützt, können Sie [issymbologysupportedasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)aufrufen.

Im folgenden Beispiel wird überprüft, ob der Barcode Scanner die **Code32** -Symbologie unterstützt:

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Ändern der erkannten Symbologien
In einigen Fällen möchten Sie möglicherweise eine Teilmenge der Symbologien verwenden, die der Barcode Scanner unterstützt.  Dies ist besonders nützlich, um Symbologien zu blockieren, die Sie nicht in Ihrer Anwendung verwenden möchten. Um z. b. sicherzustellen, dass ein Benutzer den richtigen Barcode scannt, können Sie die Überprüfung auf UPC oder EAN einschränken, wenn Sie Element-SKUs abrufen und die Überprüfung auf Code 128 einschränken, wenn Sie Seriennummern erwerben.

Wenn Sie die Symbologien kennen, die Ihr Scanner unterstützt, können Sie die Symbologien festlegen, die Sie erkennen möchten.  Dies kann geschehen, nachdem Sie ein [claimedbarcodescanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) -Objekt mithilfe von [claimscannerasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)erstellt haben. Sie können [setactivesymbologiesasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) aufrufen, um einen bestimmten Satz von Symbologien zu aktivieren, während die aus der Liste ausgelassenen nicht deaktiviert sind.

Im folgenden Beispiel werden die aktiven Symbologien eines beanspruchten Barcode Scanners auf [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) und [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex)festgelegt:

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Barcode symbologieattribute
Verschiedene Barcode Symbologien können unterschiedliche Attribute aufweisen, wie z. b. die Unterstützung mehrerer decodierungslängen, das übertragen der Prüfzahl an den Host als Teil der Rohdaten und die Überprüfung der Ziffern Überprüfung. Mit der [barcodesymbologyattribute](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) -Klasse können Sie diese Attribute für eine bestimmte [claimedbarcodescanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) -und Barcode-Symbologie erhalten und festlegen.

Die Attribute einer bestimmten Symbologie können mit " [getsymbologyattributesasync" abgerufen](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)werden. Der folgende Code Ausschnitt Ruft die Attribute der upca-Symbologie für einen **claimedbarcodescanner**ab.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Wenn Sie das Ändern der Attribute abgeschlossen haben und bereit sind, diese festzulegen, können Sie [setsymbologyattributesasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)aufrufen. Diese Methode gibt einen **booleschen**Wert zurück, der **true** ist, wenn die Attribute erfolgreich festgelegt wurden.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Scandaten nach Daten Länge einschränken
Einige Symbologien sind variabler Länge, z. b. Code 39 oder Code 128.  Barcodes dieser Symbologien können sich nahe beieinander befinden, die unterschiedliche Daten enthalten, die häufig eine bestimmte Länge aufweisen. Durch Festlegen der spezifischen Länge der benötigten Daten können ungültige Scans verhindert werden.

Überprüfen Sie vor dem Festlegen der Länge der Decodierung, ob die Barcode Symbologie mehrere Längen mit [isdecodelta engthsupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)unterstützt. Wenn Sie wissen, dass es unterstützt wird, können Sie den Typ " [decodelta](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)Layout" festlegen, der den Typ " [barcodesymbologydecodelta-Kind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)" hat. Diese Eigenschaft kann einen der folgenden Werte aufweisen:

* **Anylength**: Decodieren von Längen einer beliebigen Zahl.
* **Diskret**: Decodieren von Längen von [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) -oder [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) -Einzel Byte Zeichen.
* **Range**: decodieren Längen zwischen **DecodeLength1** -und **DecodeLength2** -Einzel Byte Zeichen. Die Reihenfolge von **DecodeLength1** und **DecodeLength2** ist nicht wichtig (kann entweder höher oder niedriger sein als die andere).

Schließlich können Sie die Werte von **DecodeLength1** und **DecodeLength2** festlegen, um die Länge der benötigten Daten zu steuern.

Der folgende Code Ausschnitt veranschaulicht das Festlegen der Länge der Decodierung:

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>Überprüfen der Ziffern Übertragung

Ein weiteres Attribut, das Sie für eine Symbologie festlegen können, ist, ob die Prüfziffer als Teil der Rohdaten an den Host übertragen wird. Vergewissern Sie sich vor dem festlegen, dass die Symbologie die Überprüfung auf Ziffern mit " [ischeckdigittransmissionsupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)" unterstützt. Legen Sie dann fest, ob die Überprüfung der Überprüfung mit " [ischeckdigittransmissionaktiviert](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)" aktiviert ist.

Der folgende Code Ausschnitt veranschaulicht das Festlegen der Überprüfung der Überprüfung auf Ziffern:

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>Überprüfen der Ziffern Überprüfung

Sie können auch festlegen, ob die Ziffern Überprüfung überprüft werden soll. Vergewissern Sie sich vor dem festlegen, dass die Symbologie die Überprüfung auf Prüfziffern mit " [ischeckdigitvalidationsupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)" unterstützt. Legen Sie dann fest, ob Überprüfungs Ziffern Überprüfung mit " [ischeckdigitvalidationaktivierte](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)" aktiviert ist.

Der folgende Code Ausschnitt veranschaulicht das Festlegen der Prüfziffern Überprüfung:

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Weitere Informationen

* [Barcodescanner](pos-barcodescanner.md)
* [Barcodesymbologien-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [Barcodescanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Claimedbarcodescanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [Barcodesymbologyattribute-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [Barcodesymbologydecodelta-thkind-Enum](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)