---
title: Einführung in den Point of Service
description: Dieser Artikel enthält Informationen zu den ersten Schritten mit den Point of Service Windows-Runtime-APIs.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: f5f19d1337a7ae49f46ab65d8420fedb775eeb2f
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730386"
---
# <a name="getting-started-with-point-of-service"></a>Einführung in den Point of Service

Point of Service, Point of Sale oder Point of Service-Geräte sind Computer Peripheriegeräte, die zum Vereinfachen von Einzelhandels Transaktionen verwendet werden. Beispiele für Point of Service-Geräte sind elektronische Bargeld Register, Barcode Scanner, Magnetstreifenleser und Empfangsdrucker.

Hier lernen Sie die Grundlagen für die Schnittstelle mit Point-of-Service-Geräten kennen, indem Sie die Windows-Runtime Point of Service-APIs verwenden. Wir behandeln die Geräte Enumeration, überprüfen Gerätefunktionen, fordern Geräte und Geräte Freigabe an. Wir verwenden ein Barcode Scanner-Gerät als Beispiel, aber fast alle Anleitungen gelten für alle UWP-kompatiblen Point of Service-Geräte. (Eine Liste der unterstützten Geräte finden Sie [unter Point of Service Device Support](pos-device-support.md)).

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>Suchen und verbinden mit Point of Service-Peripheriegeräten

Bevor ein Point of Service-Gerät von einer App verwendet werden kann, muss es mit dem PC gekoppelt werden, auf dem die app ausgeführt wird. Es gibt mehrere Möglichkeiten, um eine Verbindung mit Point of Service-Geräten herzustellen, entweder Programm gesteuert oder über die app "Einstellungen".

### <a name="connecting-to-devices-by-using-the-settings-app"></a>Herstellen einer Verbindung mit Geräten mithilfe der App "Einstellungen"
Wenn Sie ein Point-of-Service-Gerät wie einen Barcode Scanner an einen PC anschließen, wird es wie jedes andere Gerät angezeigt. Sie finden ihn im Abschnitt **Geräte > Bluetooth & andere Geräte** der App "Einstellungen". Dort können Sie mit einem Point of Service-Gerät koppeln, indem Sie **Bluetooth oder ein anderes Gerät hinzufügen**auswählen.

Einige Point of Service-Geräte werden möglicherweise nicht in der App "Einstellungen" angezeigt, bis Sie Programm gesteuert mithilfe der Point of Service-APIs aufgelistet werden.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>Erhalten eines einzelnen Point-of-Service-Geräts mit "getdefaultasync"
In einem einfachen Anwendungsfall kann es vorkommen, dass nur ein Punkt der Dienst Peripherie mit dem PC verbunden ist, auf dem die app ausgeführt wird, und Sie so schnell wie möglich einrichten möchten. Rufen Sie hierzu das "Standard"-Gerät mit der **getdefaultasync** -Methode ab, wie hier gezeigt.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

Wenn das Standardgerät gefunden wird, kann das abgerufene Geräte Objekt in Anspruch genommen werden. "Claim": ein Gerät ermöglicht einer Anwendung exklusiven Zugriff auf die Anwendung und verhindert so widersprüchliche Befehle aus mehreren Prozessen.

> [!NOTE] 
> Wenn mehr als ein Punkt des Dienst Geräts mit dem PC verbunden ist, gibt **getdefaultasync** das erste gefundene Gerät zurück. Verwenden Sie aus diesem Grund **findallasync** , es sei denn, Sie sind sicher, dass nur ein Punkt des Dienst Geräts für die Anwendung sichtbar ist.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>Auflisten einer Auflistung von Geräten mit findallasync

Wenn eine Verbindung mit mehr als einem Gerät besteht, müssen Sie die Sammlung der **pointfservice** -Geräte Objekte auflisten, um die gewünschte Auflistung zu finden. Der folgende Code erstellt z. b. eine Sammlung aller derzeit verbundenen Barcode Scanner und durchsucht die Auflistung nach einem Scanner mit einem bestimmten Namen.

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

### <a name="scoping-the-device-selection"></a>Einschränken der Geräteauswahl
Beim Herstellen einer Verbindung mit einem Gerät empfiehlt es sich, die Suche auf eine Teilmenge der Dienst Peripherie Punkte zu beschränken, auf die Ihre APP Zugriff hat. Mithilfe der **getdeviceselector** -Methode können Sie den Bereich der Auswahl so festlegen, dass nur Geräte abgerufen werden, die nur durch eine bestimmte Methode (Bluetooth, USB usw.) verbunden sind. Sie können einen Selektor erstellen, der über **Bluetooth**-, **IP-**, **lokale**oder **alle Verbindungstypen**nach Geräten sucht. Dies kann nützlich sein, da die Ermittlung von drahtlos Geräten im Vergleich zur lokalen (verdrahteten) Ermittlung sehr lange dauert. Sie können eine deterministische Wartezeit für die lokale Geräte Verbindung sicherstellen, indem Sie **findallasync** auf **lokale** Verbindungstypen beschränken. Mit diesem Code werden z. b. alle Barcode Scanner abgerufen, auf die über eine lokale Verbindung zugegriffen werden kann. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>Reagieren auf Änderungen der Geräte Verbindung mit devicewatcher

Wenn Ihre APP ausgeführt wird, werden Geräte manchmal getrennt oder aktualisiert, oder es müssen neue Geräte hinzugefügt werden. Sie können die **devicewatcher** -Klasse verwenden, um auf gerätebezogene Ereignisse zuzugreifen, sodass Ihre APP entsprechend reagieren kann. Hier ist ein Beispiel für die Verwendung von **devicewatcher**mit Methodenstub, die aufgerufen werden, wenn ein Gerät hinzugefügt, entfernt oder aktualisiert wird.

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

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>Überprüfen der Funktionen eines Point-of-Service-Geräts
Auch innerhalb einer Geräteklasse, wie z. b. Barcode Scanner, variieren die Attribute jedes Geräts zwischen Modellen möglicherweise erheblich. Wenn Ihre APP ein bestimmtes Geräte Attribut erfordert, müssen Sie möglicherweise jedes verbundene Geräte Objekt überprüfen, um zu bestimmen, ob das Attribut unterstützt wird. Beispielsweise erfordert Ihr Unternehmen möglicherweise, dass Bezeichnungen mit einem bestimmten Barcodedruck Muster erstellt werden. Hier sehen Sie, wie Sie überprüfen können, ob ein verbundener Barcode Scanner eine Symbologie unterstützt. 

> [!NOTE]
> Eine Symbologie ist die sprach Zuordnung, die ein Barcode verwendet, um Nachrichten zu codieren.

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

### <a name="using-the-devicecapabilities-class"></a>Verwenden der Device. Funktionalitäten-Klasse
Die **Device. Funktionalitäten** -Klasse ist ein Attribut aller Geräteklassen des Dienst Punkts und kann verwendet werden, um allgemeine Informationen zu den einzelnen Geräten zu erhalten. In diesem Beispiel wird z. b. ermittelt, ob ein Gerät Statistikberichte unterstützt, und wenn dies der Fall ist, werden Statistiken für alle unterstützten Typen abgerufen.

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

## <a name="claiming-a-point-of-service-device"></a>Anfordern eines Punkts des Dienst Geräts
Bevor Sie ein Point-of-Service-Gerät für aktive Eingaben oder Ausgaben verwenden können, müssen Sie es anfordern, um der Anwendung exklusiven Zugriff auf viele ihrer Funktionen zu gewähren. Dieser Code zeigt, wie Sie ein Barcode Scanner-Gerät anfordern, nachdem Sie das Gerät mithilfe einer der zuvor beschriebenen Methoden gefunden haben.

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

### <a name="retaining-the-device"></a>Beibehalten des Geräts
Wenn Sie ein Point-of-Service-Gerät über eine Netzwerk-oder Bluetooth-Verbindung verwenden, möchten Sie das Gerät möglicherweise für andere apps im Netzwerk freigeben. (Weitere Informationen hierzu finden Sie unter Freigeben von [Geräten](#sharing-a-device-between-apps).) In anderen Fällen empfiehlt es sich, für die längere Verwendung auf dem Gerät zu halten. Dieses Beispiel zeigt, wie ein beforderter Barcode Scanner beibehalten wird, nachdem eine andere APP angefordert hat, dass das Gerät freigegeben werden muss.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>Eingabe und Ausgabe

Nachdem Sie ein Gerät beansprucht haben, sind Sie fast bereit, es zu verwenden. Zum Empfangen von Eingaben vom Gerät müssen Sie einen Delegaten einrichten und aktivieren, um Daten zu empfangen. Im folgenden Beispiel wird ein Barcode Scanner-Gerät beansprucht, dessen Decodierungs Eigenschaft festgelegt und anschließend **enableasync** aufgerufen, um decodierte Eingaben vom Gerät zu aktivieren. Dieser Prozess unterscheidet sich zwischen den Geräteklassen. eine Anleitung zum Einrichten eines Delegaten für nicht-Barcode-Geräte finden Sie im entsprechenden [UWP-App-Beispiel](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors).

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

## <a name="sharing-a-device-between-apps"></a>Freigeben eines Geräts zwischen Apps

Point of Service-Geräte werden häufig in Fällen verwendet, in denen mehr als eine app in einem kurzen Zeitraum darauf zugreifen muss.  Ein Gerät kann gemeinsam genutzt werden, wenn eine Verbindung mit mehreren apps lokal (USB-oder andere Kabelverbindung) oder über ein Bluetooth-oder IP-Netzwerk besteht. Abhängig von den Anforderungen der einzelnen apps muss ein Prozess möglicherweise seinen Anspruch auf dem Gerät verwerfen. Mit diesem Code wird das beanspruchte Barcode Scanner-Gerät freigegeben, sodass andere apps es beanspruchen und verwenden können.

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> Sowohl der beanspruchte als auch der nicht beanspruchte Punkt der Dienst Geräteklassen implementieren die [iclosable-Schnittstelle](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable). Wenn ein Gerät über ein Netzwerk oder Bluetooth mit einer App verbunden ist, müssen sowohl die beanspruchten als auch die nicht beanspruchten Objekte verworfen werden, bevor eine andere App eine Verbindung herstellen kann.

## <a name="see-also"></a>Siehe auch
+ [Beispiel für Barcode Scanner](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Beispiel für Bargeld laden]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Beispiel für eine Zeilen Anzeige](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Beispiel für Magnet Stripe Reader](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Posprinter-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

