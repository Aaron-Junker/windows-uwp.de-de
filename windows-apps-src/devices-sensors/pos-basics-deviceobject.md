---
title: Punkt Dienst-Geräte Objekte
description: Erfahren Sie, wie Sie ein pointfservice-Geräte Objekt erstellen und Informationen zum Lebenszyklus von Geräte Objekten im UWP-Anwendungsmodell (universelle Windows-Plattform) erhalten.
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 4c9a5008756831eed9819a3b323d167dcc4b2744
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043402"
---
# <a name="pointofservice-device-objects"></a>Punkt Dienst-Geräte Objekte

## <a name="creating-a-device-object"></a>Erstellen eines Geräteobjekts

Nachdem Sie das zu verwendende pointfservice-Gerät durch eine neue Enumeration oder eine gespeicherte DeviceID identifiziert haben, rufen Sie einfach [**fromittlerasync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) mit der[**DeviceID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) auf, die Sie Programm gesteuert ausgewählt haben, oder der Benutzer hat sich für die Erstellung eines neuen Point of Service-Geräte Objekts entschieden.

In diesem Beispiel wird versucht, ein neues Barcodescanner-Objekt mit fromittel Async mithilfe einer DeviceID zu erstellen. Wenn beim Erstellen des Objekts ein Fehler aufgetreten ist, wird eine Debugmeldung geschrieben.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use and enable it to exchange data
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

Sobald Sie über ein Geräte Objekt verfügen, können Sie auf die Methoden, Eigenschaften und Ereignisse des Geräts zugreifen.  

## <a name="device-object-lifecycle"></a>Lebenszyklus von Geräte Objekten

Vor Windows 8 hatten Apps einen einfachen Lebenszyklus. Win32-und .net-apps werden entweder ausgeführt oder werden nicht ausgeführt, und pointservice-Peripheriegeräte wurden in der Regel für den gesamten App-Lebenszyklus beansprucht. Wenn ein Benutzer eine App minimiert oder verlässt, wird sie weiterhin ausgeführt. Dies war in Ordnung, bis tragbare Geräte und die effiziente Energienutzung immer wichtiger wurden.

Mit Windows 8 wurde ein neues Anwendungsmodell mit UWP-apps eingeführt. Auf oberer Ebene wurde der neue Zustand „Angehalten“ eingeführt. Eine UWP-APP wird kurz nach dem minimieren oder wechseln zu einer anderen APP angehalten. Dies bedeutet, dass die Threads der App beendet werden. die APP wird im Arbeitsspeicher belassen, es sei denn, das Betriebssystem muss Ressourcen freigeben, und alle Geräte Objekte, die pointarfservice-Peripheriegeräte darstellen, werden automatisch geschlossen, damit andere Anwendungen auf die Peripheriegeräte zugreifen können. Wenn der Benutzer zur APP zurück wechselt, kann er schnell in einen laufenden Zustand wieder hergestellt werden, und es kann sein, dass er nach dem Fortsetzen von pointfservice-Peripheriegeräten wieder hergestellt wird.

Sie können erkennen, wenn ein Objekt aus irgendeinem Grund mit einem geschlossen wird \<DeviceObject\> . Der geschlossene Ereignishandler notieren Sie sich die Geräte-ID, um die Verbindung in Zukunft wiederherzustellen.   Alternativ dazu können Sie dies auch in einer APP unterbrechen, um die Geräte-IDs zum erneuten Einrichten der Geräte Verbindungen bei der Benachrichtigung über die APP-Wiederaufnahme zu speichern.  Stellen Sie sicher, dass Sie nicht auf den Ereignis Handlern und den doppelten Aktionen für das Geräte Objekt für beides doppelklicken \<DeviceObject\> . Closed und App Suspend.

> [!TIP]
> Weitere Informationen zum Lebenszyklus von Windows 10 universelle Windows-Plattform (UWP)-Anwendungen finden Sie in den folgenden Themen:
> - [Lebenszyklus der Windows 10 universelle Windows-Plattform-app (UWP)](../launch-resume/app-lifecycle.md)
> - [Behandeln der APP-Aussetzung](../launch-resume/suspend-an-app.md)
> - [Behandeln der App-Fortsetzung](../launch-resume/resume-an-app.md)
