---
title: Arbeiter mit Strichcodescanner-Symbologien
description: Dieser Artikel enthält Informationen über Strichcodescanner-Symbologien.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 690b6b8ee688f62dcae375ed48e07797c921bf43
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637595"
---
# <a name="working-with-symbologies"></a>Arbeiten mit Symbologien
Eine [Strichcodesymbologie](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) ist eine Zuordnung von Daten zu einem bestimmten Strichcodeformat. Einige allgemeine Symbologien enthalten UPC, Code 128, QR-Code und So weiter.  Die universelle Windows-Plattform Barcode-Scanner APIs kann es sich um eine Anwendung zu steuern, wie die Überprüfung dieser Symbologien verarbeitet, ohne die Überprüfung manuell zu konfigurieren. 

## <a name="determine-which-symbologies-are-supported"></a>Bestimmen Sie, welche Symbologien unterstützt werden 
Da die Anwendung unterschiedliche Strichcodescannermodelle verschiedener Hersteller verwenden kann, sollten Sie den Scanner befragen, um die Liste der Symbologien zu ermitteln, die er unterstützt.  Dies kann nützlich sein, wenn die Anwendung eine bestimmte Symbologie erfordert, die möglicherweise nicht von allen Scannern unterstützt wird oder Sie müssen Symbologien aktivieren, die entweder manuell oder programmgesteuert auf dem Scanner deaktiviert wurden.

Sobald Sie ein [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)-Objekt über [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) erhalten haben, rufen Sie [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) auf, um eine Liste der Symbologien abzurufen, die vom Gerät unterstützt werden.

Im folgenden Beispiel ruft eine Liste der unterstützten Symbologien des Barcodescanners ab und zeigt sie in einem Textblock:

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
Um festzustellen, ob die Überprüfung einer bestimmten Symbologie unterstützt, können Sie aufrufen [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_).

Das folgende Beispiel überprüft, wenn der Barcodescanner unterstützt die **Code32** Symbologie:

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Ändern Sie die Symbologien erkannt werden
In einigen Fällen möchten Sie möglicherweise eine Teilmenge Symbologien verwenden, die der Strichcodescanner unterstützt.  Dies ist besonders nützlich, um Symbologien zu blockieren, die Sie nicht in Ihrer Anwendung verwenden möchten. Um sicherzustellen, dass ein Benutzer den richtigen Strichcode scannt, können Sie das Scannen auf UPC oder EAN beim Kauf von Element-SKUs beschränken und das Scannen auf Code 128 beim Kauf von Seriennummern beschränken.

Wenn Sie die Symbologien kennen, die der Scanner unterstützt, können Sie die Symbologien festlegen, die Sie erkennen möchten.  Dies ist möglich, nachdem Sie festgestellt haben eine [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) -Objekt unter Verwendung der [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Rufen Sie [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) auf, um eine bestimmte Anzahl von Symbologien zu aktivieren. Die in der Liste nicht berücksichtigten Symbologien werden deaktiviert.

Im folgenden Beispiel wird die aktiven Symbologien von einem in Anspruch genommenen Barcode-Scanner auf [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) und [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Barcode Symbologie Attribute
Anderen Barcode Symbologien können verschiedene Attribute verfügen, z. B. unterstützen mehrere Länge und übertragen die Prüfziffer an den Host als Teil der Rohdaten decodieren und Digit-Überprüfung überprüfen. Mit der [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) -Klasse, Sie und legen Sie die Attribute für einen bestimmten [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) und barcodesymbologie.

Erhalten Sie die Attribute mit einem bestimmten Symbologie [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_). Der folgende Codeausschnitt Ruft die Attribute der Symbologie Upca für eine **ClaimedBarcodeScanner**.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Wenn Sie abgeschlossen haben, ändern die Attribute und bereit für die sie festlegen, können Sie aufrufen [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync). Diese Methode gibt eine **"bool"**, d.h. **"true"** , wenn die Attribute erfolgreich festgelegt wurden.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Überprüfung von Daten in Data Länge beschränken
Einige Symbologien haben verschiedene Längen z. B. Code 39 oder Code 128.  Barcodes, der diese Symbologien kann in der Nähe mit unterschiedlichen Daten häufig einer bestimmten Länge befinden. Das Festlegen der bestimmten Länge der Daten, die Sie benötigen, kann ungültige Überprüfungen verhindern.

Vor dem Festlegen der Decode-Länge, überprüfen Sie, ob die barcodesymbologie mehrere Länge mit unterstützt [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported). Sobald Sie wissen, dass es unterstützt wird, Sie können festlegen, die [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), vom Typ [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind). Diese Eigenschaft kann einen der folgenden Werte sein:

* **AnyLength**: Decodiert Länge einer beliebigen Anzahl an.
* **Diskrete**: Decodiert die Längen der [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) oder [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) Einzelbyte-Zeichen.
* **Bereich**: Decodieren von Länge zwischen **DecodeLength1** und **DecodeLength2** Einzelbyte-Zeichen. Die Reihenfolge der **DecodeLength1** und **DecodeLength2** ist keine Frage (entweder kann sein höher oder niedriger als die andere).

Außerdem können Sie festlegen, die Werte der **DecodeLength1** und **DecodeLength2** um die Länge der Daten zu steuern, Sie benötigen.

Der folgende Codeausschnitt veranschaulicht das Festlegen der Decode-Länge:

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

### <a name="check-digit-transmission"></a>Digit-Übertragung überprüfen

Ein anderes Attribut, die, das Sie für eine Symbologie festlegen können, ist, gibt an, ob die Prüfziffer an den Host als Teil der Rohdaten gesendet wird. Bevor Sie diese Einstellung aus, stellen Sie sicher, dass es sich bei der Symbologie Überprüfung unterstützt, Ziffer Übertragungen mit [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported). Legen Sie dann, ob Check Ziffer Übertragung aktiviert ist, mit [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled).

Der folgende Codeausschnitt zeigt die Einstellung Kontrollkästchen Ziffer Übertragung:

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

Sie können auch festlegen, ob der Barcode Prüfziffer überprüft werden. Bevor Sie diese Einstellung aus, stellen Sie sicher, dass es sich bei der Symbologie Überprüfung unterstützt, Ziffer-Validierung mit [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported). Legen Sie dann, ob Check Ziffer Validierung aktiviert ist, mit [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled).

Im folgenden Codeausschnitt wird die Einstellung Kontrollkästchen Ziffer-Überprüfung:

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

## <a name="see-also"></a>Siehe auch

* [Barcodescanner](pos-barcodescanner.md)
* [BarcodeSymbologies-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind-Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)