---
title: Pointservice-geräteanspruch und Aktivieren des Modells
description: Weitere Informationen zum pointservice-Anspruch und zum Aktivieren des Modells
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 0f3fc2b2aa10fedf143c55158e521b2c1cd5b75d
ms.sourcegitcommit: 6fbf645466278c1f014c71f476408fd26c620e01
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816691"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>Point of Service-geräteanspruch und Aktivieren des Modells

## <a name="claiming-for-exclusive-use"></a>Anfordern einer exklusiven Verwendung

Nachdem Sie erfolgreich ein PointOfService-Geräteobjekt erfolgreich erstellt haben, müssen Sie es mithilfe der entsprechenden Beanspruchungsmethode für den Gerätetyp beanspruchen, bevor Sie das Gerät für die Ein- und Ausgabe verwenden können.  Die Beanspruchung gewährt der Anwendung einen exklusiven Zugriff auf viele Funktionen des Geräts, um sicherzustellen, dass eine Anwendung die Verwendung des Geräts durch eine andere Anwendung nicht beeinträchtigt.  Es kann jeweils nur eine Anwendung die exklusive Verwendung eines PointOfService-Geräts beanspruchen. 

> [!Note]
> Mit der Aktion "Anspruch" wird ein Gerät exklusiv gesperrt, aber nicht in einen Betriebszustand versetzt.  Weitere Informationen finden Sie unter [Aktivieren von Geräten für e/a-Vorgänge](#enable-device-for-io-operations) .

### <a name="apis-used-to-claim--release"></a>Zum Anfordern/freigeben verwendete APIs

|Gerät|Anspruch | Version | 
|-|:-|:-|
|BarcodeScanner | [Barcodescanner. claimscannerasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [Claimedbarcodescanner. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [Cashschublade. claimdrawerasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [Claimedcashschublade. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay. claimasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [Claimedinedisplay. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | ["Magneticstripereader. claimreaderasync"](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [Claimedmagneticstripereader. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [Posprinter. claimprinterasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [Claimedposprinter. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>Aktivieren des Geräts für e/a-Vorgänge

Mit der Anspruchs Aktion wird lediglich eine exklusive Berechtigung für das Gerät hergestellt, aber nicht in einen Betriebszustand versetzt.  Zum Empfangen von Ereignissen oder Ausführen von Ausgabe Vorgängen müssen Sie das Gerät mithilfe von **enableasync**aktivieren.  Umgekehrt können Sie " **DisableAsync** " aufrufen, um das Lauschen auf Ereignisse des Geräts zu verhindern oder eine Ausgabe auszuführen.  Sie können auch **isaktivierte** verwenden, um den Status Ihres Geräts zu ermitteln.

### <a name="apis-used-enable--disable"></a>Verwendete APIs aktivieren/deaktivieren

| Gerät | Aktivieren | Deaktivieren | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [Enableasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [Enableasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | Nicht zutreffend ¹ | Nicht zutreffend ¹ | Nicht zutreffend ¹ | 
|ClaimedMagneticStripeReader | [Enableasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [Enableasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ bei der Zeilen Anzeige ist es nicht erforderlich, das Gerät explizit für e/a-Vorgänge zu aktivieren.  Die Aktivierung von erfolgt automatisch durch die PointF-Dienst-LineDisplay-APIs, die e/a-Vorgänge ausführen.

## <a name="code-sample-claim-and-enable"></a>Code Beispiel: Anspruch und Aktivierung

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

Der erste Schritt besteht darin, einen Ereignishandler zu erstellen, der auf das **releasedevicerequtzig** -Ereignis mit **retaindevice**antwortet.  

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

Registrieren Sie anschließend den Ereignishandler in der Zuordnung zu ihrem beanspruchten Gerät.

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
|ClaimedBarcodeScanner | [Releasedevicerequessiert](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [Retaindevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [Releasedevicerequessiert](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [Retaindevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [Releasedevicerequessiert](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [Retaindevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [Releasedevicerequessiert](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [Retaindevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [Releasedevicerequessiert](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [Retaindevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
