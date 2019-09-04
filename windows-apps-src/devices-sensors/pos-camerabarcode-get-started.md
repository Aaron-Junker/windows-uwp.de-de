---
title: Erste Schritte mit dem Kamera-Strichcodescanner
description: Umgang mit dem Kamera-Strichcodescanner
ms.date: 09/02/2019
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: b35ff6b183a6344fbc8da6b44a6cb81ea695a1c9
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243293"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Erste Schritte mit dem Kamera-Strichcodescanner

Die hier verwendeten Ausschnitte dienen nur zu Demonstrationszwecken. Ein funktionierendes Beispiel finden Sie im [Beispiel Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner).

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Schritt 1: Hinzufügen von Funktions Deklarationen zum App-Manifest

1. Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2. Wählen Sie die Registerkarte **Funktionen** aus
3. Aktivieren Sie die Kontrollkästchen für **Webcam** und **PointOfService**

>[!NOTE]
> Die **Webcam**-Funktion ist erforderlich, um für den Softwaredecoder von der Kamera Frames zum Decodieren zu empfangen, sowie eine Vorschau von Ihrer Anwendung bereitzustellen

## <a name="step-2-add-using-directives"></a>Schritt 2: Add using-Direktiven

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>Schritt 3: Definieren der Geräteauswahl

### <a name="option-a-find-all-barcode-scanners"></a>**Option A: Alle Barcode Scanner suchen**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**Option B: Bereich für Geräteauswahl zum Verbindungstyp**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>Schritt 4: Alle Barcode Scanner aufzählen

Wenn Sie nicht erwarten, dass sich die Liste der Geräte im Laufe der Lebensdauer Ihrer Anwendung ändert, können Sie eine Momentaufnahme nur einmal mit der [Datei "endviceinformation. findallasync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)" auflisten, aber wenn Sie der Meinung sind, dass sich die Liste der Barcode Scanner während der Lebensdauer Ihres Anwendung Sie sollten stattdessen einen [devicewatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) verwenden.  

> [!Important]
> Das Verwenden von GetDefaultAsync PointOfService-Geräten kann zu inkonsistentem Verhalten führen, da nur das erste Gerät in der Klasse zurückgegeben wird und sich dies von Sitzung zu Sitzung ändern kann.

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**Option A: Auflisten einer Momentaufnahme von Barcode Scannern**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Unter [*Enumerieren einer Momentaufnahme von Geräten*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) finden Sie weitere Informationen zur Verwendung von *FindAllAsync*.

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**Option B: Auflisten der verfügbaren Barcode Scanner und Überwachen von Änderungen an verfügbaren Scannern**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> Weitere Informationen finden Sie unter [*Enumerieren und Überwachen von Änderungen in Geräten*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) und [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).

## <a name="step-5-identify-camera-barcode-scanners"></a>Schritt 5: Kamera Barcode Scanner identifizieren

Ein Kamera-Strichcodescanner wird dynamisch erstellt, wenn Windows die Kamera(s) koppelt, die mit einen Software-Decoder an den Computer angeschlossen sind.  Jede Kamera – gekoppelter Decoder ist ein voll funktionsfähiger Strichcodescanner.

Für jeden Barcode Scanner in der sich ergebenden Geräte Sammlung können Sie die Barcode Scanner und physische Barcode Scanner unterscheiden, indem Sie die Eigenschaft [*Barcodescanner. videodeviceid*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) überprüfen.  Eine videodebug-ID, die nicht NULL ist, gibt an, dass das Barcode Scanner-Objekt aus der Geräte Sammlung ein Kamera-Barcode Scanner ist.  Wenn Sie mehr als einen Kamera Barcode Scanner haben, können Sie eine separate Sammlung erstellen, die physische Barcode Scanner ausschließt.

Kamera Barcode Scanner, die den im Lieferumfang von Windows enthaltenen Decoder verwenden, werden wie folgt identifiziert:

> Microsoft-BarcodeScanner (*Name der Kamera hier*)

Wenn Sie über mehr als eine Kamera verfügen und diese in das Chassis Ihres Computers integriert sind, kann der Name zwischen der *Vorder* -und der *Rückseite* unterscheiden.

> [!NOTE]
> In Zukunft können zusätzliche Software Decoder mit unterschiedlichen Benennungs Schemas freigegeben werden.

Wenn devicewatcher startet (Schritt 4), listet er alle verbundenen Geräte auf. Hier fügen wir die verfügbaren Scanner zu einer Barcode Scanner-Sammlung hinzu und binden die Auflistung an ein ListBox-Steuerfeld.

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

Wenn der SelectedIndex des ListBox-Elements geändert wird (das erste Element ist standardmäßig im vorherigen Code Ausschnitt ausgewählt), werden die Geräteinformationen abgefragt.

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Schritt 6: Kamera Barcode Scanner anfordern

Verwenden Sie [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync), um eine exklusive Verwendung des Kamera-Strichcodescanners zu erhalten.

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>Schritt 7: Vom System bereitgestellte Vorschau

Eine Kameravorschau ist für den Benutzer notwendig, um die Kamera auf Barcodes zu richten.  Windows bietet eine einfache Kamera Vorschau, mit der ein Dialogfeld für die grundlegende Steuerung des Kamera Barcode Scanners gestartet wird.  Rufen Sie einfach [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) auf, um das Dialogfeld zu öffnen und [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) zu schließen, wenn Sie fertig sind.

> [!TIP]
> Unter See [Hosting-Vorschau](pos-camerabarcode-hosting-preview.md) erfahren Sie, wie Sie die Vorschau für Kamera-Strichcodescanner in Ihrer Anwendung hosten.

## <a name="step-8-initiate-scan"></a>Schritt 8: Überprüfung initiieren

Sie können den Überprüfungsvorgang initiieren, indem Sie [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) aufrufen.

Abhängig vom Wert von [**isdisabledondatareceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) scannt der Scanner möglicherweise nur einen Barcode und beendet oder scannt fortlaufend, bis Sie [**stopsoftwaretriggerasync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)aufrufen.

Legen Sie den gewünschten Wert der [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) fest, um das Verhalten des Scanners zu steuern, wenn ein Strichcode decodiert wird.

| Wert | Beschreibung |
| ----- | ----------- |
| True   | Nur einen Barcode scannen und dann beenden |
| False  | Scannen Sie Barcodes ohne Unterbrechung |

## <a name="see-also"></a>Siehe auch

### <a name="samples"></a>Proben

- [Beispiel für Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
