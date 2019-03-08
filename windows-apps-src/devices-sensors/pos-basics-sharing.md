---
title: PointOfService gemeinsame Nutzung von Geräten
description: PointOfService-Peripheriegeräte für andere Benutzer freigeben
ms.date: 06/14/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618945"
---
# <a name="pointofservice-device-sharing"></a>PointOfService gemeinsame Nutzung von Geräten

Erfahren Sie mehr über das Netzwerk oder verbundene Bluetooth-Peripheriegeräte mit anderen Computern in einer Umgebung gemeinsam nutzen, in denen mehrere PCs freigegebene Peripheriegeräte anstelle von dedizierten, auf jedem Computer angeschlossenen Peripheriegeräten abhängen.

## <a name="device-sharing"></a>Gemeinsame Nutzung von Geräten

Netzwerk- und Bluetooth-verbundenen PointOfService-Peripheriegeräte normalerweise verwendet werden, in einer Umgebung Wheere sind mehrere Client-Geräte die gleichen Peripheriegeräten im Laufe des Tages freigeben.  In einer Umgebung mit hoher Auslastung Einzelhandels- oder Food-Services hat eine Verzögerung bei der die Möglichkeit, dass ein Clientgerät zum Anfügen an ein Peripheriegerät Auswirkungen auf die Effizienz, die in der eine Zuordnung eine Transaktion mit dem Kunden zu schließen und zur nächsten fortfahren kann. In einem schnellen Service Restaurant-Szenario, in denen ein Receipt-Drucker als eine Küche Drucker verwendet wird, um die Details der Bestellung eines Kunden in der Küche für die datenvorbereitung übertragen, werden mehrere Client-Geräte, die die Bestellungen von Kunden.  Sobald der Auftrag abgeschlossen ist. sollte jedem Clientgerät Lage Anspruch der freigegebenen Drucker und die Reihenfolge für die Küche sofort zu drucken.

In diesen Umgebungen ist es wichtig für die Anwendung vollständig **dispose** des Geräts Objekts, sodass eine andere auf demselben Gerät beanspruchen kann.

Freigeben von einem PosPrinter am Ende eine "using"-Block

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


Von einem PosPrinter verworfen durch explizites Aufrufen von Dispose()

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>API-Methoden 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
