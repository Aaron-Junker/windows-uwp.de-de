---
title: Erste Schritte mit Point Of Service-Geräten
description: Dieser Artikel enthält Informationen für erste Schritte mit Point Of Service-UWP-Apps.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 518529fecff9f25a83a03847b1623bf9f09a8145
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661155"
---
# <a name="getting-started-with-point-of-service"></a>Erste Schritte mit Point Of Service-Geräten

Point Of Service-, Point Of Sale- oder POS-Geräte sind Peripheriegeräte des Computers für Einzelhandelstransaktionen. Beispiele für POS-Geräte sind elektronische Registrierkassen, Strichcodescanner, Magnetstreifenleser und Belegdrucker.

In dieser Anleitung lernen Sie die Grundlagen der Anbindung an POS-Geräte mithilfe der POS-APIs für die Universelle Windows-Plattform (UWP) kennen. Behandelt werden Themen wie Geräteenumeration, Überprüfen von Gerätefunktionen, Anfordern von Geräten und die gemeinsame Nutzung von Geräten. Wir verwenden einen Strichcodescanner als Beispiel, aber fast alle Erläuterungen in dieser Anleitung gelten für alle UWP-kompatiblen POS-Geräte. (Eine Liste der unterstützten Geräte finden Sie unter [POS-Geräteunterstützung](pos-device-support.md)).

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>Koppeln von POS-Peripheriegeräten

Bevor ein POS-Gerät von einer App verwendet werden kann, muss es mit dem PC gekoppelt werden, auf dem die App ausgeführt wird. Es gibt verschiedene Möglichkeiten, Verbindungen mit POS-Geräten herzustellen, entweder programmgesteuert oder über die App „Einstellungen”.

### <a name="connecting-to-devices-by-using-the-settings-app"></a>Herstellen einer Verbindung mit Geräten mithilfe der App „Einstellungen”
Wenn Sie ein POS-Gerät, z. B. einen Strichcodescanner, an einen PC anschließen, wird es wie jedes anderes Gerät angezeigt. Sie finden es im Abschnitt **Geräte > Bluetooth- & andere Geräte** der App „Einstellungen”. Hier können Sie POS-Geräte koppeln, indem Sie **Bluetooth- oder anderes Gerät hinzufügen** wählen.

Einige POS-Geräte werden möglicherweise erst dann in den Einstellungen angezeigt, nachdem sie mithilfe der POS-APIs programmgesteuert aufgelistet wurden.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>Abrufen eines POS-Geräts mit GetDefaultAsync
In einem einfachen Anwendungsfall haben Sie vielleicht nur ein POS-Peripheriegerät mit dem PC verbunden, auf dem die App ausgeführt wird, und möchten es so schnell wie möglich einrichten. Rufen Sie dazu das Standardgerät „Default” mit der **GetDefaultAsync**-Methode ab, wie hier gezeigt.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

Nachdem dieses Standardgerät gefunden wurde, kann das abgerufene Geräteobjekt beansprucht werden. Eine Anwendung, die ein Gerät „beansprucht”, erhält exklusiven Zugriff darauf. Dadurch werden Konflikte vermieden, die entstehen würden, wenn mehrere Prozesse auf das Gerät zugreifen.

> [!NOTE] 
> Wenn mehrere POS-Geräte mit dem PC verbunden sind, liefert **GetDefaultAsync** das erste gefundene Gerät. Aus diesem Grund sollten Sie **FindAllAsync** verwenden, es sei denn, Sie sind sicher, dass die Anwendung nur ein POS-Gerät erkennt.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>Auflisten von Geräten mit FindAllAsync

Sind mehrere Gerät angeschlossen, müssen Sie die Auflistung **PointOfService** durchsuchen, um das Geräteobjekt zu finden, das Sie beanspruchen möchten. Der folgende Code erstellt z. B. eine Auflistung aller aktuell verbundenen Strichcodescanner und durchsucht die Auflistung dann nach einem Scanner mit einem bestimmten Namen.

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;

string selector = BarcodeScanner.GetDeviceSelector();       
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
    if (devInfo.Name.Contains("1200G"))
    {
        Debug.WriteLine(" Found one");
    }
}
```

### <a name="scoping-the-device-selection"></a>Begrenzen der Geräteauswahl
Beim Herstellen einer Verbindung mit einem Gerät können Sie die Suche auf eine Teilmenge der POS-Peripheriegeräte beschränken, auf die Ihre App zugreifen können. Mithilfe der Methode **GetDeviceSelector** beschränken Sie die Auflistung von Geräten auf solche, die nur über eine bestimmte Methode (Bluetooth, USB usw.) verbunden sind. Sie können einen Selektor erstellen, die über folgende Verbindungen nach Geräten sucht: **Bluetooth**, **IP**, **Local** oder **All connection types**. Das kann nützlich sein, da die Suche nach drahtlosen Geräte im Vergleich zur Suche nach lokalen (verkabelten) Geräten lange dauert. Sie können eine bestimmte Wartezeit für die Verbindung mit lokalen Geräten sicherstellen, indem Sie **FindAllAsync** auf **Local**-Verbindungstypen beschränken. Dieser Code ermittelt beispielsweise alle Strichcodescanner, die über eine lokale Verbindung angeschlossen sind. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>Reagieren auf geänderte Geräteverbindungen mit DeviceWatcher

Während Ihre App ausgeführt wird, ist nicht auszuschließen, dass Geräte getrennt, aktualisiert oder neu hinzugefügt werden. Sie können die Klasse **DeviceWatcher** verwenden, um auf gerätebezogene Ereignisse zuzugreifen, sodass Ihre App entsprechend reagieren kann. Hier ein Beispiel zur Verwendung von **DeviceWatcher**. Der Code enthält Methodenstubs, die aufgerufen werden können, wenn ein Gerät hinzugefügt, entfernt oder aktualisiert wird.

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>Überprüfen der Funktionalität eines POS-Geräts
Sogar innerhalb einer Geräteklasse (z. B. der Strichcodescanner) können die Attribute der einzelnen Geräte modellbedingt erheblich variieren. Wenn für Ihre App ein bestimmtes Geräteattribut erforderlich ist, sollten Sie jedes verbundene Gerätobjekt überprüfen, um festzustellen, ob das Attribut unterstützt wird. Beispielsweise könnte es für Ihr Unternehmen erforderlich sein, dass Bezeichnungen mit einem bestimmten Strichcodemuster versehen sind. Im Folgenden wird gezeigt, wie Sie überprüfen können, ob ein verbundener Strichcodescanner eine Symbologie unterstützt. 

> [!NOTE]
> Eine Symbologie ist die Sprache, die zum Codieren von Nachrichten in einem Strichcode verwendet wird.

```Csharp
try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceId);
    if (await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32))
    {
        Debug.WriteLine("Has symbology");
    }
}
catch (Exception ex)
{
    Debug.WriteLine("FromIdAsync() - " + ex.Message);
}
```

### <a name="using-the-devicecapabilities-class"></a>Verwenden der Device.Capabilities-Klasse
Die **Device.Capabilities**-Klasse ist ein Attribut aller POS-Geräteklassen und kann verwendet werden, um allgemeine Informationen zu einzelnen Geräten abzurufen. In diesem Beispiel wird beispielsweise geprüft, ob ein Gerät Statistiken liefert. Wenn das der Fall ist, werden Statistiken für alle unterstützt Typen abgerufen.

```Csharp
try
{
    if (barcodeScanner.Capabilities.IsStatisticsReportingSupported)
    {
        Debug.WriteLine("Statistics reporting is supported");

        string[] statTypes = new string[] {""};
        IBuffer ibuffer = await barcodeScanner.RetrieveStatisticsAsync(statTypes);
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: RetrieveStatisticsAsync() - " + ex.Message);
}
```

## <a name="claiming-a-point-of-service-device"></a>Beanspruchen eines POS-Geräts
Bevor Sie ein POS-Gerät für Ein- oder Ausgaben verwenden können, müssen Sie es „beanspruchen”. Dadurch erhält die Anwendung exklusiven Zugriff auf viele Gerätefunktionen. Dieser Code zeigt, wie Sie einen Strichcodescanner beanspruchen, nachdem Sie das Gerät mithilfe einer der zuvor beschriebenen Methoden gefunden haben.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

### <a name="retaining-the-device"></a>Aufrechthalten des Geräteanspruchs
Wenn Sie ein POS-Gerät über eine Netzwerk- oder Bluetooth-Verbindung verwenden, möchten Sie das Gerät möglicherweise mit anderen Apps im Netzwerk teilen. (Weitere Informationen hierzu finden Sie unter [Geräte freigeben](#sharing-a-device-between-apps).) In anderen Fällen möchten möglicherweise für ausgedehnte Verwendung auf dem Gerät warten. In diesem Beispiel wird veranschaulicht, wie der Anspruch auf einen Strichcodescanner aufrechterhalten wird, wenn eine andere App fordert, das Gerät freizugegeben.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>Eingabe und Ausgabe

Nachdem Sie ein Gerät beansprucht haben, steht der Verwendung kaum noch etwas im Wege. Um Eingaben vom Gerät zu empfangen, müssen Sie es einrichten und einen Delegaten aktivieren, der die Daten übernimmt. Im folgenden Beispiel wird ein Strichcodescanner beansprucht, und es wird die Decodierungseigenschaft festgelegt. Dann erfolgt der Aufruf von **EnableAsync**, um decodierte Eingaben vom Gerät zu aktivieren. Dieser Vorgang variiert je nach Geräteklasse. Informationen zum Einrichten eines Delegaten für andere als Strichcodegeräte finden Sie im entsprechenden [UWP-App-Beispiel](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors).

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
    if (claimedBarcodeScanner != null)
    {
        claimedBarcodeScanner.DataReceived += claimedBarcodeScanner_DataReceived;
        claimedBarcodeScanner.IsDecodeDataEnabled = true;
        await claimedBarcodeScanner.EnableAsync();
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}


void claimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    string symbologyName = BarcodeSymbologies.GetName(args.Report.ScanDataType);
    var scanDataLabelReader = DataReader.FromBuffer(args.Report.ScanDataLabel);
    string barcode = scanDataLabelReader.ReadString(args.Report.ScanDataLabel.Length);
}
```

## <a name="sharing-a-device-between-apps"></a>Teilen eines Geräts zwischen Apps

POS-Geräte werden häufig in Fällen verwendet, in denen mehrere Apps innerhalb eines kurzen Zeitraums darauf zugreifen müssen.  Ein Gerät kann geteilt werden, wenn es mit mehreren Apps entweder lokal verbunden ist (über USB- oder eine andere Kabelverbindung) oder über ein Bluetooth- oder IP-Netzwerk. Je nach den Anforderungen der einzelnen Apps muss ein Prozess möglicherweise seinen Anspruch auf das Gerät aufgeben. Dieser Code gibt den Anspruch auf den Strichcodescanner auf, damit er von einer anderen App beansprucht und verwendet werden kann

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> Sowohl die beanspruchten als auch die nicht beanspruchten POS-Geräteklassen implementieren die [IClosable-Schnittstelle](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable). Wenn ein Gerät über ein Netzwerk oder Bluetooth mit einer App verbunden ist, müssen sowohl die beanspruchten als auch die nicht beanspruchten Objekte freigegeben werden, bevor eine andere App eine Verbindung herstellen kann.

## <a name="see-also"></a>Siehe auch
+ [Barcode Scanner-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Bargeld Drawer-Beispiel]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Beispiel für Zeile anzeigen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Magnetstreifens Reader-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

