---
author: TerryWarwick
title: Arbeiter mit Strichcodescanner-Symbologien
description: Dieser Artikel enthält Informationen über Strichcodescanner-Symbologien.
ms.author: jken
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 91e09fd881ea5ce2b5ae6482148cdbb730c7ef17
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "5684840"
---
# <a name="working-with-symbologies"></a>Arbeiten mit Symbologien
Eine [Strichcodesymbologie](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) ist eine Zuordnung von Daten zu einem bestimmten Strichcodeformat. Einige allgemeinen Symbologien enthalten UPC, Code 128, QR-Code und So weiter.  Der universelle Windows-Plattform-Strichcodescanner-APIs ermöglichen einer Anwendung zu steuern, wie der Scanner die Symbologien verarbeitet, ohne manuell den Scanner konfigurieren. 

## <a name="determine-which-symbologies-are-supported"></a>Bestimmen Sie, welche Symbologien unterstützt werden 
Da die Anwendung unterschiedliche Strichcodescannermodelle verschiedener Hersteller verwenden kann, sollten Sie den Scanner befragen, um die Liste der Symbologien zu ermitteln, die er unterstützt.  Dies kann nützlich sein, wenn die Anwendung eine bestimmte Symbologie erfordert, die möglicherweise nicht von allen Scannern unterstützt wird oder Sie müssen Symbologien aktivieren, die entweder manuell oder programmgesteuert auf dem Scanner deaktiviert wurden.

Sobald Sie ein [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)-Objekt über [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) erhalten haben, rufen Sie [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) auf, um eine Liste der Symbologien abzurufen, die vom Gerät unterstützt werden.

Das folgende Beispiel ruft eine Liste der unterstützten Symbologien von der Strichcodescanner ab, und zeigt diese in einem Textblock:

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

## <a name="determine-if-a-specific-symbology-is-supported"></a>Bestimmen Sie, ob eine bestimmte Symbologie unterstützt wird
Um festzustellen, ob der Scanner eine bestimmte Symbologie unterstützt, können Sie [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)aufrufen.

Im folgenden Beispiel wird überprüft, ob der Strichcodescanner die **Code32** Symbologie unterstützt:

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Ändern Sie, welche Symbologien erkannt werden
In einigen Fällen möchten Sie möglicherweise eine Teilmenge Symbologien verwenden, die der Strichcodescanner unterstützt.  Dies ist besonders nützlich, um Symbologien zu blockieren, die Sie nicht in Ihrer Anwendung verwenden möchten. Um sicherzustellen, dass ein Benutzer den richtigen Strichcode scannt, können Sie das Scannen auf UPC oder EAN beim Kauf von Element-SKUs beschränken und das Scannen auf Code 128 beim Kauf von Seriennummern beschränken.

Wenn Sie die Symbologien kennen, die der Scanner unterstützt, können Sie die Symbologien festlegen, die Sie erkennen möchten.  Dies kann erfolgen, wenn Sie ein [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) -Objekt mit [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)eingerichtet haben. Rufen Sie [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) auf, um eine bestimmte Anzahl von Symbologien zu aktivieren. Die in der Liste nicht berücksichtigten Symbologien werden deaktiviert.

Im folgenden Beispiel wird die aktiven Symbologien von einem Anspruch Strichcodescanner [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) und [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Barcode-symbologieattribute
Verschiedene barcodesymbologien können verschiedene Attribute verfügen, z. B. unterstützen mehrere decodieren Länge, übertragen die Prüfziffer an den Host als Teil der Rohdaten, und überprüfen Sie Ziffer Validierung. Mit der [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) -Klasse können Sie abrufen und diese Attribute für einen bestimmten [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) und Strichcode-Symbologie festlegen.

Sie können die Attribute der eine bestimmte Symbologie mit [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)abrufen. Der folgende Codeausschnitt Ruft die Attribute der Upca Symbologie für eine **ClaimedBarcodeScanner**ab.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Wenn Sie die Änderung der Attribute abgeschlossen haben und nun diese festlegen können, können Sie [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)aufrufen. Diese Methode gibt **ein boolescher Wert**, das ist **"true"** , wenn die Attribute erfolgreich festgelegt wurden.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Einschränken von gescannten Daten nach Datenlänge
Einige Symbologien haben verschiedene Längen z.B. Code 39 oder Code 128.  Strichcodes mit dieser Symbologien können sich nahe beieinander, enthält verschiedene Daten einer bestimmten Länge befinden. Das Festlegen der bestimmten Länge der Daten, die Sie benötigen, kann ungültige Überprüfungen verhindern.

Vor dem Festlegen der Decodierung Länge, überprüfen Sie, ob der Strichcode-Symbologie mehrere Länge mit [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)unterstützt. Wenn Sie wissen, dass es unterstützt wird, können Sie die [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), die vom Typ [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)festlegen. Diese Eigenschaft kann eines der folgenden Werte sein:

* **AnyLength**: Länge einer beliebigen Anzahl zu decodieren.
* **Diskret**: Längen von [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) oder [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) Single-Byte-Zeichen zu decodieren.
* **Bereich**: Länge zwischen **DecodeLength1** und **DecodeLength2** Single-Byte-Zeichen zu decodieren. Die Reihenfolge der **DecodeLength1** und **DecodeLength2** keine Rolle (entweder kann höher oder niedriger als die andere sein).

Schließlich können Sie festlegen, die Werte **DecodeLength1** und **DecodeLength2** zum Steuern der Länge der Daten, die Sie benötigen.

Der folgende Codeausschnitt veranschaulicht das Festlegen der Decodierung Länge:

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

### <a name="check-digit-transmission"></a>Aktivieren Sie das Kontrollkästchen Ziffer Übertragung

Ein weiteres Attribut, die, das Sie für eine Symbologie festlegen können, ist gibt an, ob die Prüfziffer als Teil der unformatierten Daten an den Host übermittelt werden. Bevor Sie diese Einstellung, stellen Sie sicher, dass die Symbologie, aktivieren Sie das Kontrollkästchen unterstützt Ziffer Datenübertragung mit [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported). Legen Sie anschließend, ob Kontrollkästchen Ziffer Übertragung mit [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)aktiviert ist.

Der folgende Codeausschnitt veranschaulicht die Einstellung Kontrollkästchen Ziffer Übertragung:

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

### <a name="check-digit-validation"></a>Überprüfen Sie Ziffer Validierung

Sie können auch festlegen, ob die Barcode-Prüfziffer überprüft werden. Bevor Sie diese Einstellung, stellen Sie sicher, dass die Symbologie, aktivieren Sie das Kontrollkästchen unterstützt Ziffer Validierung mit [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported). Legen Sie anschließend, ob Kontrollkästchen Ziffer Validierung mit [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)aktiviert ist.

Der folgende Codeausschnitt veranschaulicht die Einstellung Kontrollkästchen Ziffer Überprüfung:

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

## <a name="see-also"></a>Weitere Informationen:

* [Strichcodescanner](pos-barcodescanner.md)
* [BarcodeSymbologies-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind-Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)