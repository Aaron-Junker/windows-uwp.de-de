---
title: PointOfService-Geräteobjekte
description: Hier erfahren Sie, wie PointOfService-Geräteobjekte erstellt werden.
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: a2fa7e107d890a5be7c8d27af03289b839ec3c09
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209986"
---
# <a name="pointofservice-device-objects"></a>PointOfService-Geräteobjekte

## <a name="creating-a-device-object"></a>Erstellen eines Geräteobjekts
Nachdem Sie das PointOfService-Gerät, das Sie verwenden möchten, identifiziert haben, entweder über eine neue Aufzählung oder eine gespeicherte Geräte-ID, rufen Sie einfach [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) mit der [**DeviceID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) auf, die Sie programmgesteuert ausgewählt haben oder die der Benutzer ausgewählt hat, um ein neues PointofService-Geräteobjekt zu erstellen.

In diesem Beispiel wird versucht, ein neues BarcodeScanner-Objekt mit FromIdAsync über eine Geräte-ID zu erstellen. Kommt es beim Erstellen des Objekts zu einem Fehler, wird eine Debugmeldung ausgegeben.

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

Wenn Sie ein Geräteobjekt haben, können Sie auf die Methoden, Eigenschaften und Ereignisse des Geräts zugreifen.  

## <a name="device-object-lifecycle"></a>Lebenszyklus des Geräteobjekts
Vor Windows 8 hatten Apps einen einfachen Lebenszyklus. Win32- und .NET-Apps werden entweder ausgeführt oder nicht. Und PointOfService-Peripheriegeräte wurden für gewöhnlich für den gesamten App-Lebenszyklus in Anspruch genommen. Wenn ein Benutzer eine App minimiert oder verlässt, wird sie weiterhin ausgeführt. Dies war in Ordnung, bis tragbare Geräte und die effiziente Energienutzung immer wichtiger wurden.

In Windows 8 wurde mit UWP-Apps ein neues Anwendungsmodell eingeführt. Auf oberer Ebene wurde der neue Zustand „Angehalten“ eingeführt. Kurze Zeit, nachdem der Benutzer eine UWP-App minimiert oder zu einer anderen App wechselt, wird sie angehalten. Dies bedeutet Folgendes: die Threads der App werden angehalten, die App verbleibt im Arbeitsspeicher, es sei denn, das Betriebssystem muss Ressourcen zurückfordern, und alle Geräteobjekte, die PointOfService-Peripheriegeräte darstellen, werden automatisch geschlossen, damit andere Anwendungen auf die Peripheriegeräte zugreifen können. Wenn der Benutzer zur App zurückwechselt, kann diese schnell in einen ausgeführten Zustand wiederhergestellt werden, und auch die Verbindungen mit PointOfService-Peripheriegeräten werden wiederhergestellt, sofern sie zum Fortsetzen noch verfügbar sind.

Sie können erkennen, wenn ein Objekt aus irgendeinem Grund mit einem \<DeviceObject\>geschlossen wird. Der geschlossene Ereignishandler notieren Sie sich die Geräte-ID, um die Verbindung in Zukunft wiederherzustellen.   Alternativ möchten Sie dies ggf. mit einer Benachrichtigung zum Anhalten der App handhaben, um die Geräte-IDs zur Wiederherstellung der Geräteverbindungen bei Benachrichtigung zum Fortsetzen der App zu speichern.  Stellen Sie sicher, dass Sie nicht auf den Ereignis Handlern und den doppelten Aktionen für das Geräte Objekt auf \<DeviceObject-\>doppelklicken. Closed und App Suspend.

> [!TIP]
> Weitere Informationen zum Anwendungslebenszyklus für die Universelle Windows-Plattform (UWP) in Windows 10 finden Sie in den folgenden Themen:
> - [Lebenszyklus der Windows 10 universelle Windows-Plattform-app (UWP)](../launch-resume/app-lifecycle.md)
> - [Behandeln der APP-Aussetzung](../launch-resume/suspend-an-app.md)
> - [Behandeln der App-Fortsetzung](../launch-resume/resume-an-app.md)
