---
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: Scannen aus Ihrer App
description: Erfahren Sie, wie Sie Inhalte über Ihre App mithilfe eines Flachbett-, Einzugs- oder automatisch konfigurierten Scanners scannen können.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1a314d0acdc3df1e0b53b1d78445b6ab1b71bf92
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369747"
---
# <a name="scan-from-your-app"></a>Scannen aus Ihrer App


**Wichtige APIs**

-   [**Windows.Devices.Scanners**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners)
-   [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)
-   [**Geräteklasse**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass)

Erfahren Sie, wie Sie Inhalte über Ihre App mithilfe eines Flachbett-, Einzugs- oder automatisch konfigurierten Scanners scannen können.

**Wichtige**  der [ **Windows.Devices.Scanners** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners) APIs sind Teil des Desktops [Gerätefamilie](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide). Apps können diese APIs nur für die desktop-Version von Windows 10 verwenden.

Damit Sie über Ihre App scannen können, müssen Sie zunächst die verfügbaren Scanner auflisten, indem Sie ein neues [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt deklarieren und den [**DeviceClass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass)-Typ abrufen. Nur Scanner, die lokal mit WIA-Treibern installiert sind, werden in Ihrer App aufgeführt und stehen darin zur Verfügung.

Nachdem Ihre App verfügbare Scanner aufgelistet hat, kann sie die auf dem Scannertyp basierenden automatisch konfigurierten Scaneinstellungen verwenden oder einfach unter Verwendung der verfügbaren Flachbett- oder Einzugsscanquelle scannen. Damit die automatisch konfigurierten Einstellungen verwendet werden können, muss der Scanner mit der automatischen Konfiguration kompatibel sein und darf nicht sowohl über einen Flachbett- als auch über einen Einzugsscanner verfügen. Weitere Informationen finden Sie unter [Automatisch konfigurierter Scan](https://docs.microsoft.com/windows-hardware/drivers/image/auto-configured-scanning).

## <a name="enumerate-available-scanners"></a>Aufzählen verfügbarer Scanner

Windows erkennt Scanner nicht automatisch. Sie müssen diesen Schritt ausführen, damit Ihre App mit dem Scanner kommunizieren kann. In diesem Beispiel erfolgt die Scannergeräteaufzählung mit dem [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)-Namespace.

1.  Fügen Sie zunächst diese using-Anweisungen zu Ihrer Klassendefinitionsdatei hinzu.

``` csharp
    using Windows.Devices.Enumeration;
    using Windows.Devices.Scanners;
```

2.  Implementieren Sie dann die Geräteüberwachung, um mit der Scanneraufzählung zu beginnen. Weitere Informationen finden Sie unter [Aufzählen von Geräten](enumerate-devices.md).

```csharp
    void InitDeviceWatcher()
    {
       // Create a Device Watcher class for type Image Scanner for enumerating scanners
       scannerWatcher = DeviceInformation.CreateWatcher(DeviceClass.ImageScanner);

       scannerWatcher.Added += OnScannerAdded;
       scannerWatcher.Removed += OnScannerRemoved;
       scannerWatcher.EnumerationCompleted += OnScannerEnumerationComplete;
    }
```

3.  Erstellen Sie einen Ereignishandler für den Fall, dass ein Scanner hinzugefügt wird.

```csharp
    private async void OnScannerAdded(DeviceWatcher sender,  DeviceInformation deviceInfo)
    {
       await
       MainPage.Current.Dispatcher.RunAsync(
             Windows.UI.Core.CoreDispatcherPriority.Normal,
             () =>
             {
                MainPage.Current.NotifyUser(String.Format("Scanner with device id {0} has been added", deviceInfo.Id), NotifyType.StatusMessage);

                // search the device list for a device with a matching device id
                ScannerDataItem match = FindInList(deviceInfo.Id);

                // If we found a match then mark it as verified and return
                if (match != null)
                {
                   match.Matched = true;
                   return;
                }

                // Add the new element to the end of the list of devices
                AppendToList(deviceInfo);
             }
       );
    }
```

## <a name="scan"></a>Scan

1.  **Abrufen eines Objekts ImageScanner**

Für jeden [**ImageScannerScanSource**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners.ImageScannerScanSource)-Aufzählungstyp (ob **Default**, **AutoConfigured**, **Flatbed** oder **Feeder**) müssen Sie zunächst ein [**ImageScanner**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners.ImageScanner)-Objekt erstellen, indem Sie wie folgt die [**ImageScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.scanners.imagescanner.fromidasync)-Methode aufrufen.

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **Nur Überprüfung**

Zum Scan mit den Standardeinstellungen ist Ihre App bei der Auswahl eines Scanners vom [**Windows.Devices.Scanners**](https://docs.microsoft.com/uwp/api/Windows.Devices.Scanners)-Namespace abhängig und scannt aus dieser Quelle. Es werden keine Scaneinstellungen geändert. Die möglichen Scanner sind „Automatisch konfiguriert“, „Flachbett“ und „Einzug“. Bei dieser Scanart kommt es wahrscheinlich zu einem erfolgreichen Scanvorgang, selbst wenn der Scan von der falschen Quelle aus stattfindet (wie Flachbett anstelle von Einzug).

**Beachten Sie**  , wenn der Benutzer das Dokument überprüfen im Einzug platziert, der Scan wird Scannen aus dem Flachbett stattdessen. Wenn der Benutzer versucht, aus einem leeren Einzug zu scannen, generiert der Scanauftrag keine gescannten Dateien.
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **Überprüfung von automatisch konfiguriert, Flachbett oder Feeder Quelle**

Ihre App kann den [automatisch konfigurierten Scan](https://docs.microsoft.com/windows-hardware/drivers/image/auto-configured-scanning) des Geräts mit den optimalen Scaneinstellungen verwenden. Bei dieser Option kann das Gerät selbst die besten Scaneinstellungen, wie Farbmodus und Scanauflösung, basierend auf dem zu scannenden Inhalt bestimmen. Das Gerät wählt die Scaneinstellungen zur Laufzeit für jeden neuen Scanauftrag.

**Beachten Sie**  nicht alle Scanner unterstützen dieses Feature, damit die app überprüft werden muss, wenn die Überprüfung dieses Feature unterstützt wird, bevor Sie diese Einstellung verwenden.

In diesem Beispiel prüft die App zunächst, ob der Scanner die automatische Konfiguration unterstützt, und startet dann den Scanvorgang. Um einen Flachbett- oder Einzugsscanner anzugeben, ersetzen Sie einfach **AutoConfigured** durch **Flatbed** oder **Feeder**.

```csharp
    if (myScanner.IsScanSourceSupported(ImageScannerScanSource.AutoConfigured))
    {
        ...
        // Scan API call to start scanning with Auto-Configured settings.
        var result = await myScanner.ScanFilesToFolderAsync(
            ImageScannerScanSource.AutoConfigured, folder).AsTask(cancellationToken.Token, progress);
        ...
    }
```

## <a name="preview-the-scan"></a>Scanvorschau

Sie können Code hinzufügen, um eine Scanvorschau vor dem Scan in einen Ordner anzuzeigen. Im folgenden Beispiel prüft die App, ob der **Flatbed**-Scanner die Vorschau unterstützt, und zeigt dann die Scanvorschau an.

```csharp
if (myScanner.IsPreviewSupported(ImageScannerScanSource.Flatbed))
{
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
                // Scan API call to get preview from the flatbed.
                var result = await myScanner.ScanPreviewToStreamAsync(
                    ImageScannerScanSource.Flatbed, stream);
```

## <a name="cancel-the-scan"></a>Abbrechen des Scans

Sie können wie folgt zulassen, dass Benutzer den Scanauftrag während der Ausführung abbrechen.

```csharp
void CancelScanning()
{
    if (ModelDataContext.ScenarioRunning)
    {
        if (cancellationToken != null)
        {
            cancellationToken.Cancel();
        }                
        DisplayImage.Source = null;
        ModelDataContext.ScenarioRunning = false;
        ModelDataContext.ClearFileList();
    }
}
```

## <a name="scan-with-progress"></a>Scannen mit Fortschritt

1.  Erstellen eines **System.Threading.CancellationTokenSource**-Objekts.

```csharp
cancellationToken = new CancellationTokenSource();
```

2.  Richten Sie den Fortschritts-Ereignishandler ein, und rufen Sie den Scanfortschritt ab.

```csharp
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
    var progress = new Progress<UInt32>(ScanProgress);
```

## <a name="scanning-to-the-pictures-library"></a>Scannen in die Bildbibliothek

Benutzer können dynamisch mit der [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)-Klasse in beliebige Ordner scannen. Sie müssen aber die Funktion *Bildbibliothek* im Manifest deklarieren, damit Benutzer in diesen Ordner scannen können. Weitere Informationen zu App-Funktionen finden Sie unter [Deklarationen der App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).
