---
title: Pointservice-geräteanspruch und Aktivieren des Modells
description: Verwenden Sie den Point of Service-geräteanspruch, und aktivieren Sie APIs, um Geräte zu beanspruchen und für e/a-Vorgänge zu aktivieren.
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 391acd1f4acf0620a87e77f3c540c3dd0faa6def
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165404"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>Point of Service-geräteanspruch und Aktivieren des Modells

## <a name="claiming-for-exclusive-use"></a>Anfordern einer exklusiven Verwendung

Nachdem Sie erfolgreich ein pointservice-Geräte Objekt erstellt haben, müssen Sie es mithilfe der entsprechenden Anspruchs Methode für den Gerätetyp anfordern, bevor Sie das Gerät für die Eingabe oder Ausgabe verwenden können.  Der-Anspruch gewährt der Anwendung exklusiven Zugriff auf viele der Funktionen des Geräts, um sicherzustellen, dass die Verwendung des Geräts durch eine andere Anwendung nicht beeinträchtigt wird.  Nur eine Anwendung kann für eine ausschließliche Verwendung ein pointservice-Gerät beanspruchen. 

> [!Note]
> Mit der Aktion "Anspruch" wird ein Gerät exklusiv gesperrt, aber nicht in einen Betriebszustand versetzt.  Weitere Informationen finden Sie unter [Aktivieren von Geräten für e/a-Vorgänge](#enable-device-for-io-operations) .

### <a name="apis-used-to-claim--release"></a>Zum Anfordern/freigeben verwendete APIs

|Sicherungsmedium|Anspruch | Freigabe | 
|-|:-|:-|
|BarcodeScanner | [Barcodescanner. claimscannerasync](/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [Claimedbarcodescanner. Close](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [Cashschublade. claimdrawerasync](/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [Claimedcashschublade. Close](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay. claimasync](/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [Claimedinedisplay. Close](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|"Magneticstripereader" | ["Magneticstripereader. claimreaderasync"](/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [Claimedmagneticstripereader. Close](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|Posprinter | [Posprinter. claimprinterasync](/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [Claimedposprinter. Close](/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>Aktivieren des Geräts für e/a-Vorgänge

Mit der Anspruchs Aktion wird lediglich eine exklusive Berechtigung für das Gerät hergestellt, aber nicht in einen Betriebszustand versetzt.  Zum Empfangen von Ereignissen oder Ausführen von Ausgabe Vorgängen müssen Sie das Gerät mithilfe von **enableasync**aktivieren.  Umgekehrt können Sie " **DisableAsync** " aufrufen, um das Lauschen auf Ereignisse des Geräts zu verhindern oder eine Ausgabe auszuführen.  Sie können auch **isaktivierte** verwenden, um den Status Ihres Geräts zu ermitteln.

### <a name="apis-used-enable--disable"></a>Verwendete APIs aktivieren/deaktivieren

| Sicherungsmedium | Aktivieren | Disable | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [Enableasync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [Enableasync](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | Nicht zutreffend ¹ | Nicht zutreffend ¹ | Nicht zutreffend ¹ | 
|ClaimedMagneticStripeReader | [Enableasync](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [Enableasync](/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ bei der Zeilen Anzeige ist es nicht erforderlich, das Gerät explizit für e/a-Vorgänge zu aktivieren.  Die Aktivierung von erfolgt automatisch durch die PointF-Dienst-LineDisplay-APIs, die e/a-Vorgänge ausführen.

## <a name="code-sample-claim-and-enable"></a>Code Beispiel: Anspruch und Aktivierung

In diesem Beispiel wird gezeigt, wie Sie ein Barcode Scanner-Gerät anfordern, nachdem Sie erfolgreich ein Barcode Scanner-Objekt erstellt haben.

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
> Ein Anspruch kann in den folgenden Situationen verloren gehen:
> 1. Eine andere APP hat einen Anspruch desselben Geräts angefordert, und Ihre APP hat kein **retaindevice** als Reaktion auf das **releasedevicerequway-Ereignis** ausgegeben.  (Weitere Informationen finden Sie weiter unten in der [Anspruchs Verhandlung](#claim-negotiation) .)
> 2. Ihre APP wurde angehalten, was dazu geführt hat, dass das Geräte Objekt geschlossen wurde und der Anspruch daher nicht mehr gültig ist. (Weitere Informationen finden Sie unter [Lebenszyklus von Geräte Objekten](pos-basics-deviceobject.md#device-object-lifecycle) .)


## <a name="claim-negotiation"></a>Anspruchs Aushandlung

Da Windows eine Multitasking-Umgebung ist, ist es möglich, dass mehrere Anwendungen auf demselben Computer auf kooperative Weise auf Peripheriegeräte zugreifen müssen.  Die pointfservice-APIs bieten ein Aushandlungs Modell, mit dem mehrere Anwendungen Peripheriegeräte freigeben können, die mit dem Computer verbunden sind.

Wenn eine zweite Anwendung auf dem gleichen Computer einen Anspruch für eine "pointofservice"-Peripherie anfordert, die bereits von einer anderen Anwendung beansprucht wird, wird eine **releasedevicerequpher** -Ereignis Benachrichtigung veröffentlicht. Die Anwendung mit dem aktiven Anspruch muss auf die Ereignis Benachrichtigung reagieren, indem Sie **retaindevice** aufrufen, wenn die Anwendung zurzeit das Gerät verwendet, um den Verlust des Anspruchs zu vermeiden. 

Wenn die Anwendung mit dem aktiven Anspruch nicht sofort mit **retaindevice** antwortet, wird davon ausgegangen, dass die Anwendung angehalten wurde oder das Gerät nicht benötigt und der Anspruch gesperrt und an die neue Anwendung übergeben wird. 

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



### <a name="apis-used-for-claim-negotiation"></a>Für die Anspruchs Aushandlung verwendete APIs

|Angeforderter Gerät|Releasebenachrichtigung| Gerät beibehalten |
|-|:-|:-|
|ClaimedBarcodeScanner | [Releasedevicerequessiert](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [Retaindevice](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [Releasedevicerequessiert](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [Retaindevice](/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [Releasedevicerequessiert](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [Retaindevice](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [Releasedevicerequessiert](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [Retaindevice](/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [Releasedevicerequessiert](/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [Retaindevice](/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|