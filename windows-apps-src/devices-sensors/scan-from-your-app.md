---
author: PatrickFarley
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: Scannen aus Ihrer App
description: Erfahren Sie, wie Sie Inhalte über Ihre App mithilfe eines Flachbett-, Einzugs- oder automatisch konfigurierten Scanners scannen können.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f9128056cbb3b9218d164b243948d9dd16af0786
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "6045560"
---
# <a name="scan-from-your-app"></a>Scannen aus Ihrer App


**Wichtige APIs**

-   [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250)
-   [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)
-   [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381)

Erfahren Sie, wie Sie Inhalte über Ihre App mithilfe eines Flachbett-, Einzugs- oder automatisch konfigurierten Scanners scannen können.

**Wichtige**die [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) -APIs sind Teil der desktop [-Gerätefamilie](https://msdn.microsoft.com/library/windows/apps/Dn894631). Apps können diese APIs nur in der Desktopversion von Windows 10 verwenden.

Damit Sie über Ihre App scannen können, müssen Sie zunächst die verfügbaren Scanner auflisten, indem Sie ein neues [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)-Objekt deklarieren und den [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381)-Typ abrufen. Nur Scanner, die lokal mit WIA-Treibern installiert sind, werden in Ihrer App aufgeführt und stehen darin zur Verfügung.

Nachdem Ihre App verfügbare Scanner aufgelistet hat, kann sie die auf dem Scannertyp basierenden automatisch konfigurierten Scaneinstellungen verwenden oder einfach unter Verwendung der verfügbaren Flachbett- oder Einzugsscanquelle scannen. Damit die automatisch konfigurierten Einstellungen verwendet werden können, muss der Scanner mit der automatischen Konfiguration kompatibel sein und darf nicht sowohl über einen Flachbett- als auch über einen Einzugsscanner verfügen. Weitere Informationen finden Sie unter [Automatisch konfigurierter Scan](https://msdn.microsoft.com/library/windows/hardware/Ff539393).

## <a name="enumerate-available-scanners"></a>Aufzählen verfügbarer Scanner

Windows erkennt Scanner nicht automatisch. Sie müssen diesen Schritt ausführen, damit Ihre App mit dem Scanner kommunizieren kann. In diesem Beispiel erfolgt die Scannergeräteaufzählung mit dem [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459)-Namespace.

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

## <a name="scan"></a>Überprüfen

1.  **Abrufen eines ImageScanner-Objekts**

Für jeden [**ImageScannerScanSource**](https://msdn.microsoft.com/library/windows/apps/Dn264238)-Aufzählungstyp (ob **Default**, **AutoConfigured**, **Flatbed** oder **Feeder**) müssen Sie zunächst ein [**ImageScanner**](https://msdn.microsoft.com/library/windows/apps/Dn263806)-Objekt erstellen, indem Sie wie folgt die [**ImageScanner.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.scanners.imagescanner.fromidasync)-Methode aufrufen.

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **Einfach scannen**

Zum Scan mit den Standardeinstellungen ist Ihre App bei der Auswahl eines Scanners vom [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250)-Namespace abhängig und scannt aus dieser Quelle. Es werden keine Scaneinstellungen geändert. Die möglichen Scanner sind „Automatisch konfiguriert“, „Flachbett“ und „Einzug“. Bei dieser Scanart kommt es wahrscheinlich zu einem erfolgreichen Scanvorgang, selbst wenn der Scan von der falschen Quelle aus stattfindet (wie Flachbett anstelle von Einzug).

**Hinweis:** Wenn der Benutzer das Dokument zum Scannen in den Einzug legt, erfolgt der Scanvorgang aus dem Flachbett stattdessen. Wenn der Benutzer versucht, aus einem leeren Einzug zu scannen, generiert der Scanauftrag keine gescannten Dateien.
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **Scannen aus der Quelle „Automatisch konfiguriert“, „Flachbett“ oder „Einzug“**

Ihre App kann den [automatisch konfigurierten Scan](https://msdn.microsoft.com/library/windows/hardware/Ff539393) des Geräts mit den optimalen Scaneinstellungen verwenden. Bei dieser Option kann das Gerät selbst die besten Scaneinstellungen, wie Farbmodus und Scanauflösung, basierend auf dem zu scannenden Inhalt bestimmen. Das Gerät wählt die Scaneinstellungen zur Laufzeit für jeden neuen Scanauftrag.

**Hinweis:** nicht von allen Scannern unterstützt diese Funktion muss die app prüfen, ob der Scanner dieses Feature unterstützt, bevor Sie diese Einstellung verwenden.

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

Benutzer können dynamisch mit der [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/BR207881)-Klasse in beliebige Ordner scannen. Sie müssen aber die Funktion *Bildbibliothek* im Manifest deklarieren, damit Benutzer in diesen Ordner scannen können. Weitere Informationen zu App-Funktionen finden Sie unter [Deklarationen der App-Funktionen](https://msdn.microsoft.com/library/windows/apps/Mt270968).
