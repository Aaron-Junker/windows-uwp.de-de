---
title: Bluetooth-Werbung
description: Dieser Abschnitt enthält Artikel zur Integration von Bluetooth Low Energy (LE)-Ankündigungen in Apps für die Universelle Windows-Plattform (UWP) mithilfe der AdvertisementWatcher- and AdvertisementPublisher-APIs.
ms.date: 06/26/2020
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: ff10bbc0-03a7-492c-b5fe-c5b9ce8ca32e
ms.localizationpriority: medium
ms.openlocfilehash: 319038f8922aac2686dab6836f94ead5018effdf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159744"
---
# <a name="bluetooth-le-advertisements"></a>Bluetooth LE-Ankündigungen


**Wichtige APIs**

-   [**Windows.Devices.Bluetooth.Advertisement**](/uwp/api/windows.devices.bluetooth.advertisement)

Dieser Artikel enthält eine Übersicht über Bluetooth Low Energy (LE)-Ankündigungsbeacons für Apps für die Universelle Windows-Plattform (UWP).  

> [!Important]
> Sie müssen die Funktion "Bluetooth" in " *Package. appxmanifest*" deklarieren.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="overview"></a>Übersicht

Es gibt zwei Hauptfunktionen, die ein Entwickler mithilfe der LE-Ankündigungs-APIs ausführen kann:

-   [Ankündigungsüberwachung](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher): Überwachung auf Beacons in der Nähe und deren Filterung auf der Basis von Nutzlast oder Nähe.  
-   [Ankündigung Publisher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher): eine Nutzlast für Windows definieren, um im Auftrag eines Entwicklers zu werben.  

Ein vollständiges Codebeispiel finden Sie unter [Beispiel für Bluetooth-Ankündigungen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothAdvertisement) auf Github.

## <a name="basic-setup"></a>Grundlegende Einrichtung

Um Bluetooth LE-Grundfunktionalität in einer App für die Universelle Windows-Plattform zu verwenden, müssen Sie die Bluetooth-Funktion in „Package.appxmanifest“ aktivieren.

1. Öffnen Sie „Package.appxmanifest“.
2. Navigieren Sie zur Registerkarte „Funktionen“.
3. Suchen Sie Bluetooth in der Liste auf der linken Seite, und aktivieren Sie das Kontrollkästchen daneben.

## <a name="publishing-advertisements"></a>Veröffentlichen von Ankündigungen

Bluetooth LE-Ankündigungen ermöglichen Ihrem Gerät, konstant eine spezifische Nutzlast („Ankündigung“ genannt) zu signalisieren. Diese Ankündigung kann von jedem Bluetooth LE-kompatiblen Gerät in der Nähe erkannt werden, wenn es für den Empfang dieser spezifischen Ankündigung eingerichtet wurde.

> **Hinweis**: für den Benutzer Datenschutz ist die Lebensdauer ihrer Ankündigung an die Ihrer APP gebunden. Sie können einen BluetoothLEAdvertisementPublisher erstellen und in einer Hintergrundaufgabe „Start“ aufrufen, um die Ankündigung im Hintergrund auszuführen. Weitere Informationen zu Hintergrundaufgaben finden Sie unter [Starten, Fortsetzen und Hintergrundaufgaben](../launch-resume/index.md).

### <a name="basic-publishing"></a>Grundlegende Veröffentlichung

Es gibt viele verschiedene Arten, einer Ankündigung Daten hinzufügen. Dieses Beispiel zeigt ein häufig angewendetes Verfahren zum Erstellen einer unternehmensspezifischen Ankündigung. 

Erstellen Sie zunächst den Ankündigungsherausgeber, der steuert, ob das Gerät eine spezifische Ankündigung signalisiert oder nicht.

```csharp
BluetoothLEAdvertisementPublisher publisher = new BluetoothLEAdvertisementPublisher();
```

Erstellen Sie anschließend einen benutzerdefinierten Datenabschnitt. In diesem Beispiel wird ein nicht zugewiesener **CompanyId**-Wert *0xFFFE* verwendet und der Ankündigung der Text *Hello World* hinzugefügt. 

```csharp
// Add custom data to the advertisement
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

var writer = new DataWriter();
writer.WriteString("Hello World");

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
manufacturerData.Data = writer.DetachBuffer();

// Add the manufacturer data to the advertisement publisher:
publisher.Advertisement.ManufacturerData.Add(manufacturerData);
```

Da der Herausgeber nun erstellt und eingerichtet wurde, können Sie **Start** aufrufen, um die Ankündigung zu beginnen.

```csharp
publisher.Start();
```

## <a name="watching-for-advertisements"></a>Überwachen auf Ankündigungen

### <a name="basic-watching"></a>Grundlegende Überwachung

Der folgende Code zeigt, wie Sie eine Bluetooth LE-Ankündigungsüberwachung erstellen, einen Rückruf einrichten und mit der Überwachung auf alle LE-Ankündigungen beginnen.

```csharp
BluetoothLEAdvertisementWatcher watcher = new BluetoothLEAdvertisementWatcher();
watcher.Received += OnAdvertisementReceived;
watcher.Start();
``` 

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // Do whatever you want with the advertisement
}
```

#### <a name="active-scanning"></a>Aktives Scannen
Um Scanantwortankündigungen ebenfalls zu erhalten, legen Sie nach dem Erstellen der Überwachung Folgendes fest. Beachten Sie, dass dies mehr Energie verbraucht und in Hintergrundmodi nicht verfügbar ist.

```csharp
watcher.ScanningMode = BluetoothLEScanningMode.Active;
```

### <a name="watching-for-a-specific-advertisement-pattern"></a>Überwachung auf ein spezifisches Ankündigungsmuster

Manchmal möchten Sie eine spezifische Ankündigung erhalten. In diesem Fall überwachen Sie auf eine Ankündigung, die eine Nutzlast mit einem angenommenen Unternehmen (als 0xFFFE identifiziert) und die Zeichenfolge *Hello World* in der Ankündigung enthält. Dies kann mit dem Beispiel für die grundlegende Veröffentlichung kombiniert werden, sodass ein Windows-Computer die Ankündigung sendet und ein anderer auf diese Ankündigung überwacht. Achten Sie darauf, diesen Ankündigungsfilter festzulegen, bevor Sie die Überwachung starten.

```csharp
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
var writer = new DataWriter();
writer.WriteString("Hello World");
manufacturerData.Data = writer.DetachBuffer();

watcher.AdvertisementFilter.Advertisement.ManufacturerData.Add(manufacturerData);
```

### <a name="watching-for-a-nearby-advertisement"></a>Überwachung auf eine Ankündigung in der Nähe

Manchmal möchten Sie die Überwachung nur auslösen, wenn das Gerät, das die Ankündigung sendet, in Reichweite ist. Sie können einen eigenen Bereich definieren. Sie müssen lediglich beachten, dass die Werte auf zwischen 0 und -128 gekürzt werden. 

```csharp
// Set the in-range threshold to -70dBm. This means advertisements with RSSI >= -70dBm 
// will start to be considered "in-range" (callbacks will start in this range).
watcher.SignalStrengthFilter.InRangeThresholdInDBm = -70;

// Set the out-of-range threshold to -75dBm (give some buffer). Used in conjunction 
// with OutOfRangeTimeout to determine when an advertisement is no longer 
// considered "in-range".
watcher.SignalStrengthFilter.OutOfRangeThresholdInDBm = -75;

// Set the out-of-range timeout to be 2 seconds. Used in conjunction with 
// OutOfRangeThresholdInDBm to determine when an advertisement is no longer 
// considered "in-range"
watcher.SignalStrengthFilter.OutOfRangeTimeout = TimeSpan.FromMilliseconds(2000);
```

### <a name="gauging-distance"></a>Messen der Entfernung

Wenn der Rückruf Ihrer Bluetooth LE-Überwachung ausgelöst wird, enthalten die „EventArgs“ einen RSSI-Wert, der Sie über die Stärke des empfangenen Signals informiert (d. h., wie stark das Bluetooth-Signal ist).

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // The received signal strength indicator (RSSI)
    Int16 rssi = eventArgs.RawSignalStrengthInDBm;
}
```

Dieses kann zwar in eine ungefähre Entfernung übersetzt werden, sollte jedoch nicht zum Messen der echten Entfernung verwendet werden, da jeder Sender unterschiedlich ist. Unterschiedliche Umweltbedingungen können das Messen der Entfernung erschweren (wie Wände, Sendergehäuse oder sogar die Luftfeuchtigkeit).

Eine Alternative zur Beurteilung der reinen Entfernung besteht im Definieren von „Buckets“. Sender tendieren dazu, 0 – 50 dBm zu melden, wenn sie sehr nahe sind,-50 – -90 dBm, wenn sie sich in einem mittleren Abstand befinden, und weniger als -90 dBm, wenn sie weit entfernt sind. Tests sind die beste Möglichkeit, diese Buckets für Ihre App festzulegen.