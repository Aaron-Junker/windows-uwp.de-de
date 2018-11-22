---
author: TerryWarwick
title: Erste Schrittemit dem Kamera-Strichcodescanner
description: Umgang mit dem Kamera-Strichcodescanner
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 12aabff66fc116f510dced78aa56f3df5f84c850
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "7569408"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Erste Schrittemit dem Kamera-Strichcodescanner
## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Schritt 1: Hinzufügen von Funktionsdeklarationen zum App-Manifest
1. Öffnen Sie in MicrosoftVisual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2. Wählen Sie die Registerkarte **Funktionen** aus
3. Aktivieren Sie die Kontrollkästchen für **Webcam** und **PointOfService** 

>[!NOTE] 
> Die **Webcam**-Funktion ist erforderlich, um für den Softwaredecoder von der Kamera Frames zum Decodieren zu empfangen, sowie eine Vorschau von Ihrer Anwendung bereitzustellen

## <a name="step-2-add-using-directives"></a>Schritt2: Hinzufügen von using-Direktiven

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```
## <a name="step-3-define-your-device-selector"></a>Schritt3: Definieren der Geräteauswahl

### **<a name="option-a-find-all-barcode-scanners"></a>Option A: Anzeigen aller Strichcodescanner**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();       
```

### **<a name="option-b-scoping-device-selector-to-connection-type"></a>Option B: Begrenzen der Geräteauswahl für den Verbindungstyp**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-barcode-scanners"></a>Schritt4: Aufzählen der Strichcodescanner
Wenn Sie nicht erwarten, dass sich die Liste der Geräte über die Lebensdauer der Anwendung ändert, können Sie eine Momentaufnahme einmal mit *FindAllAsync* auflisten. Wenn Sie allerdings glauben, dass sich die Liste der Strichcodescanner während der Lebensdauer der Anwendung ändern wird, verwenden Sie stattdessen einen *DeviceWatcher*.  

> [!Important] 
> Das Verwenden von GetDefaultAsync PointOfService-Geräten kann zu inkonsistentem Verhalten führen, da nur das erste Gerät in der Klasse zurückgegeben wird und sich dies von Sitzung zu Sitzung ändern kann.

### **<a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>Option A: Enumerieren einer Momentaufnahme von Strichcodescannern**
```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Unter [*Enumerieren einer Momentaufnahme von Geräten*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) finden Sie weitere Informationen zur Verwendung von *FindAllAsync*.

### **<a name="option-b-enumerate-and-watch-for-changes-in-available-barcode-scanners"></a>Option B: Enumerieren und Überwachen von Änderungen in verfügbaren Strichcodescannern**
```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);

// TODO: Add Event Listeners and Handlers
```
> [!TIP]
> Weitere Informationen finden Sie unter [*Enumerieren und Überwachen von Änderungen in Geräten*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) und [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).

## <a name="step-5-identify-camera-barcode-scanners"></a>Schritt5: Identifizieren von Kamera-Strichcodescannern
Ein Kamera-Strichcodescanner wird dynamisch erstellt, wenn Windows die Kamera(s) koppelt, die mit einen Software-Decoder an den Computer angeschlossen sind.  Jede Kamera – gekoppelter Decoder ist ein voll funktionsfähiger Strichcodescanner.

Für jeden Strichcodescanner in der resultieRendern Gerätesammlung, können Sie festlegen, welche Kamera-Strichcodescanner im Vergleich zu physischen Strichcodescannern sind, indem Sie die [*BarcodeScanner.VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId)-Eigenschaft aktivieren.  Eine Nicht-NULL-VideoDeviceID gibt an, dass das Strichcodescannerobjekt aus Ihrer Gerätesammlung ein Kamera-Strichcodescanner ist.  Wenn Sie mehrere Kamera-Strichcodescanner haben, können Sie eine separate Sammlungen erstellen, die physische Strichcodescanner ausschließt.. 

Kamera-Strichcodescanner, die den Decoder verwenden, der mit Windows geliefert wird, sehen folgendes: 

> Microsoft-BarcodeScanner (*Name der Kamera hier*)

Wenn Ihre Kamera im Gehäuse des Computers integriert ist, kann der Name sich zwischen *vorne* und *hinten* unterscheiden, wenn Sie über mehrere Kameras verfügen.  In Zukunft sind möglicherweise zusätzliche Softwaredecoder verfügbar, die ein anderes Benennungsschema haben.

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Schritt6: Anfordern des Kamera-Strichcodescanners 
Verwenden Sie [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync), um eine exklusive Verwendung des Kamera-Strichcodescanners zu erhalten.

## <a name="step-7-system-provided-preview"></a>Schritt7: Vom System bereitgestellte Vorschau
Eine Kameravorschau ist für den Benutzer notwendig, um die Kamera auf Barcodes zu richten.  Windows bietet eine einfache Kameravorschau, die ein Dialogfeld startet, das die einfache Steuerung des Kamera-Strichcodescanners ermöglicht.  Rufen Sie einfach [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) auf, um das Dialogfeld zu öffnen und [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) zu schließen, wenn Sie fertig sind.

> [!TIP]
> Unter See [Hosting-Vorschau](pos-camerabarcode-hosting-preview.md) erfahren Sie, wie Sie die Vorschau für Kamera-Strichcodescanner in Ihrer Anwendung hosten.

## <a name="step-8-initiate-scan"></a>Schritt 8: Initialisieren scan 
Sie können den Überprüfungsvorgang initiieren, indem Sie [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) aufrufen.  
Je nach Wert des [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) kann der Scanner nur einen Strichcode scannen und diesen beenden oder kontinuierlich bis zum Aufruf von [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync) scannen.

Legen Sie den gewünschten Wert der [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) fest, um das Verhalten des Scanners zu steuern, wenn ein Strichcode decodiert wird.

| Wert | Beschreibung |
| ----- | ----------- |
| Wahr   | Nur einen Barcode scannen und dann beenden |
| False  | Scannen Sie Barcodes ohne Unterbrechung |