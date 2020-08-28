---
title: Verwenden eines Software-Triggers
description: Erfahren Sie, wie Sie den Barcode Scanvorgang mithilfe eines asynchronen Software Auslösers Programm gesteuert steuern.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: ee61a428115522717f8ff57ef7a0fc04f19aae41
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043382"
---
# <a name="use-a-software-trigger"></a>Verwenden eines Software-Triggers

Es kann nützlich sein, den Scanvorgang von Software zu steuern, wenn Sie einen Barcode Scanner im Präsentationsmodus verwenden oder wenn der Scanner nicht über einen physischen, z. b. einen kamerabasierten Barcode Scanner verfügt. Sie können den Scanvorgang initiieren, indem Sie [startsoftwaretriggerasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)aufrufen.

Abhängig vom Wert von [isdisabledondatareceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) scannt der Scanner möglicherweise nur einen Barcode und beendet oder scannt fortlaufend, bis Sie [stopsoftwaretriggerasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)aufrufen.

Legen Sie den gewünschten Wert von [isdisabledondatareceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) fest, um das scannerverhalten zu steuern, wenn ein Barcode decodiert wird.

| Wert | Beschreibung |
| ----- | ----------- |
| True   | Nur einen Barcode Scannen und dann den Vorgang abbrechen |
| Falsch  | Kontinuierliches Scannen von Barcodes ohne Beenden |


> [!Important]
> Vergewissern Sie sich, dass der Barcode Scanner die Verwendung des Software Auslösers unterstützt, indem Sie zunächst die Eigenschaft [issoftwaretriggersupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)überprüfen.

Im folgenden Beispiel wird gezeigt, wie die Überprüfung mithilfe eines Software Auslösers initiiert wird, der die Überprüfung nach dem Scannen eines Barcodes beendet:

```cs
private void SoftwareTrigger(BarcodeScanner barcodeScanner, ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    if (barcodeScanner.Capabilities.IsSoftwareTriggerSupported)
    {
        claimedBarcodeScanner.IsDisabledOnDataReceived = true;
        await claimedBarcodeScanner.StartSoftwareTriggerAsync();
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]