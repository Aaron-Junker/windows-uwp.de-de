---
title: Ersten Einstieg in den Kamera Barcode Scanner
description: Verwenden Sie diese Schritt-für-Schritt-Anleitungen und Code Ausschnitte für die ersten Schritte mit einem Kamera Barcode Scanner.
ms.date: 09/02/2019
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: f48d0d8c06a9718479c73ff4523f62ddd2fa0a06
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053740"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Ersten Schritten mit einem Kamera Barcode Scanner

Die hier verwendeten Ausschnitte dienen nur zu Demonstrationszwecken. Ein funktionierendes Beispiel finden Sie im [Beispiel Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner).

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Schritt 1: Hinzufügen von Funktions Deklarationen zum App-Manifest

1. Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2. Registerkarte " **Funktionen** " auswählen
3. Aktivieren Sie die Kontrollkästchen für **Webcam** und **pointservice** .

>[!NOTE]
> Die **Webcam** -Funktion ist erforderlich, damit der Software Decoder Frames von der Kamera zum Decodieren empfängt und eine Vorschau von Ihrer Anwendung bereitstellt.

## <a name="step-2-add-using-directives"></a>Schritt 2: Hinzufügen von using-Direktiven

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>Schritt 3: Definieren der Geräteauswahl

### <a name="option-a-find-all-barcode-scanners"></a>**Option A: alle Barcode Scanner suchen**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**Option B: Bereich für die Geräteauswahl auf Verbindungstyp**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>Schritt 4: Auflisten aller Barcode Scanner

Wenn Sie nicht erwarten, dass sich die Liste der Geräte im Laufe der Lebensdauer Ihrer Anwendung ändert, können Sie eine Momentaufnahme nur einmal mit der [Datei "endviceinformation. findallasync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)" auflisten, aber wenn Sie der Meinung sind, dass sich die Liste der Barcode Scanner während der Lebensdauer Ihrer Anwendung ändern könnte, sollten Sie stattdessen einen [devicewatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) verwenden.  

> [!Important]
> Die Verwendung von getdefaultasync zum Auflisten von pointfservice-Geräten kann zu inkonsistentem Verhalten führen, da es einfach das erste in der-Klasse gefundene Gerät zurückgibt, das sich von Sitzung zu Sitzung ändern kann.

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**Option A: Auflisten einer Momentaufnahme von Barcode Scannern**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Weitere Informationen zur Verwendung von *findallasync*finden Sie unter Auflisten [*einer Momentaufnahme von Geräten*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) .

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**Option B: Auflisten der verfügbaren Barcode Scanner und beobachten der Änderungen an verfügbaren Scannern**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> Weitere Informationen finden Sie unter Auflisten [*und Überwachen von Geräteänderungen*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) und [*devicewatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) .

## <a name="step-5-identify-camera-barcode-scanners"></a>Schritt 5: Identifizieren der Kamera Barcode Scanner

Ein Kamera-Barcode Scanner wird dynamisch erstellt, da Windows die Kamera (n) mit einem Software Decoder an Ihren Computer anfügt.  Jedes Kamera-Decoder-Paar ist ein voll funktionsfähiger Barcode Scanner.

Für jeden Barcode Scanner in der sich ergebenden Geräte Sammlung können Sie die Barcode Scanner und physische Barcode Scanner unterscheiden, indem Sie die Eigenschaft [*Barcodescanner. videodeviceid*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) überprüfen.  Eine videodebug-ID, die nicht NULL ist, gibt an, dass das Barcode Scanner-Objekt aus der Geräte Sammlung ein Kamera-Barcode Scanner ist.  Wenn Sie mehr als einen Kamera Barcode Scanner haben, können Sie eine separate Sammlung erstellen, die physische Barcode Scanner ausschließt.

Kamera Barcode Scanner, die den im Lieferumfang von Windows enthaltenen Decoder verwenden, werden wie folgt identifiziert:

> Microsoft Barcodescanner (*Name der Kamera hier*)

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

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Schritt 6: Anfordern des Kamera Barcode Scanners

Verwenden Sie " [Barcodescanner. claimscannerasync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) ", um den Kamera Barcode Scanner exklusiv zu verwenden.

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

## <a name="step-7-system-provided-preview"></a>Schritt 7: vom System bereitgestellte Vorschau

Eine Kamera Vorschau ist erforderlich, damit der Benutzer die Kamera bei Barcodes erfolgreich verfolgen kann.  Windows bietet eine einfache Kamera Vorschau, mit der ein Dialogfeld für die grundlegende Steuerung des Kamera Barcode Scanners gestartet wird.  Nennen Sie einfach [claimedbarcodescanner. showvideopreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) , um das Dialogfeld zu öffnen, und [claimedbarcodescanner. hidevideopreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) , um es zu schließen, wenn Sie fertig sind.

> [!TIP]
> Weitere Informationen finden Sie in der [hostingvorschau](pos-camerabarcode-hosting-preview.md) zum Hosten der Vorschau für den Kamera Barcode Scanner in Ihrer Anwendung

## <a name="step-8-initiate-scan"></a>Schritt 8: Initiieren der Überprüfung

Sie können den Scanvorgang initiieren, indem Sie [**startsoftwaretriggerasync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)aufrufen.

Abhängig vom Wert von [**isdisabledondatareceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) scannt der Scanner möglicherweise nur einen Barcode und beendet oder scannt fortlaufend, bis Sie [**stopsoftwaretriggerasync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)aufrufen.

Legen Sie den gewünschten Wert von [**isdisabledondatareceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) fest, um das scannerverhalten zu steuern, wenn ein Barcode decodiert wird.

| Wert | BESCHREIBUNG |
| ----- | ----------- |
| True   | Nur einen Barcode Scannen und dann den Vorgang abbrechen |
| Falsch  | Kontinuierliches Scannen von Barcodes ohne Beenden |

## <a name="see-also"></a>Weitere Informationen

### <a name="samples"></a>Proben

- [Beispiel für Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
