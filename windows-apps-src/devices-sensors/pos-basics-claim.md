---
title: PointOfService-Gerät Anspruch und Modell aktivieren
description: Erfahren Sie mehr über die PointOfService-Anspruch, und aktivieren Sie Modell
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 0e7d60c0b612a8067ac4c225dff9da5da428f1a1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639315"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>Point-of-Service-Gerät Anspruch und Modell aktivieren

## <a name="claiming-for-exclusive-use"></a>Für die ausschließliche Verwendung anfordern

Nachdem Sie erfolgreich ein PointOfService-Geräteobjekt erfolgreich erstellt haben, müssen Sie es mithilfe der entsprechenden Beanspruchungsmethode für den Gerätetyp beanspruchen, bevor Sie das Gerät für die Ein- und Ausgabe verwenden können.  Die Beanspruchung gewährt der Anwendung einen exklusiven Zugriff auf viele Funktionen des Geräts, um sicherzustellen, dass eine Anwendung die Verwendung des Geräts durch eine andere Anwendung nicht beeinträchtigt.  Es kann jeweils nur eine Anwendung die exklusive Verwendung eines PointOfService-Geräts beanspruchen. 

> [!Note]
> Die Anspruchs-Aktion noch wird eine exklusive Sperre für ein Gerät, jedoch keine es in einen funktionsfähigen Zustand.  Finden Sie unter [aktivieren-Gerät für e/a-Vorgänge](#enable-device-for-io-operations) für Weitere Informationen.

### <a name="apis-used-to-claim--release"></a>APIs verwendet, um die Anspruch / release

|Gerät|Anspruch | Version | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>Gerät für e/a-Vorgänge aktivieren

Die Anspruchs-Aktion einfach richtet eine exklusive Zugriffsrechte für das Gerät, aber nicht in einen funktionsfähigen Zustand platzieren.  Zum Empfangen von Ereignissen oder keine Ausgabevorgänge ausführen müssen Sie aktivieren, die Geräte mit **EnableAsync**.  Sie können im Gegensatz dazu aufrufen **DisableAsync** beendet das Lauschen auf Ereignisse auf dem Gerät oder das Ausführen der Ausgabe.  Sie können auch **IsEnabled** zum Ermitteln des Status Ihres Geräts.

### <a name="apis-used-enable--disable"></a>Verwendeten APIs aktivieren / deaktivieren

| Gerät | Aktivieren | Deaktivieren | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | Nicht Applicable¹ | Nicht Applicable¹ | Nicht Applicable¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasyc) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ Zeilenanzeige erfordert nicht ausdrücklich das Gerät für e/a-Vorgänge aktivieren.  Aktivieren von erfolgt automatisch durch die PointOfService LineDisplay APIs, die e/a ausführen.

## <a name="code-sample-claim-and-enable"></a>Codebeispiel: Anspruch, und aktivieren

Im folgenden Beispiel ist gezeigt, wie Sie ein Strichcodescanner-Gerät beanspruchen, nachdem Sie erfolgreich ein Strichcodescanner-Objekt erstellt haben.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> Ein Anspruch kann unter den folgenden Umständen verloren gehen:
> 1. Eine andere App hat einen Anspruch auf dasselbe Gerät angefordert, und Ihre App hat kein **RetainDevice** in Reaktion auf das Ereignis **ReleaseDeviceRequested** ausgestellt.  (Weitere Informationen finden Sie im Abschnitt [Anspruchsaushandlung](#Claim-negotiation) weiter unten).
> 2. Ihre App wurde unterbrochen, was dazu geführt hat, dass das Geräteobjekt geschlossen wurde und der Anspruch daher nicht mehr gültig ist. (Weitere Informationen finden Sie unter [Lebenszyklus von Geräteobjekten](pos-basics-deviceobject.md#device-object-lifecycle).)


## <a name="claim-negotiation"></a>Anspruchsaushandlung

Da Windows eine Multitaskingumgebung ist, fordern möglicherweise mehrere Anwendungen auf demselben Computer einen Zugriff auf Peripheriegeräte auf kooperative Weise an.  Die PointOfService-APIs bieten ein Aushandlungsmodell, das mehreren Anwendungen die gemeinsame Verwendung von an den Computer angeschlossenen Peripheriegeräten ermöglicht.

Wenn eine zweite Anwendung auf demselben Computer ein PointOfService-Peripheriegerät beansprucht, das bereits von einer anderen Anwendung in Anspruch genommen wird, wird eine **ReleaseDeviceRequested**-Ereignisbenachrichtigung veröffentlicht. Die Anwendung mit dem aktiven Anspruch muss auf die Ereignisbenachrichtigung reagieren, indem **RetainDevice** aufgerufen wird, wenn die Anwendung das Gerät derzeit verwendet, damit der Anspruch nicht verloren geht. 

Wenn die Anwendung mit dem aktiven Anspruch nicht sofort mit **RetainDevice** antwortet, wird davon ausgegangen, dass die Anwendung unterbrochen wurde oder das Gerät nicht benötigt, und der Anspruch wird widerrufen und an die neue Anwendung übergeben. 

Der erste Schritt ist, um einen Ereignishandler erstellen, das von Werten der **ReleaseDeviceRequested** -Ereignis mit **RetainDevice**.  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

Klicken Sie dann registrieren Sie den Ereignishandler im Zusammenhang mit Ihrem Gerät in Anspruch genommenen

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
```



### <a name="apis-used-for-claim-negotiation"></a>Für die Anspruchsaushandlung verwendete APIs

|Beanspruchtes Gerät|Benachrichtigungsversion| Gerät beibehalten |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
