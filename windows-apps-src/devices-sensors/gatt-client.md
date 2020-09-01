---
title: Bluetooth GATT-Client
description: Dieser Artikel bietet eine Übersicht über Bluetooth Generic Attribute profile (GATT) Client for universelle Windows-Plattform-Apps (UWP) sowie Beispielcode für gängige Anwendungsfälle.
ms.date: 06/26/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fd5f2b76af856dd66e2dfd0ee2b3e429199e6a19
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172284"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT-Client

In diesem Artikel wird die Verwendung der Client-APIs für das Bluetooth Generic Attribute (GATT) für universelle Windows-Plattform-Apps (UWP) sowie Beispielcode für gängige GATT-Client Aufgaben veranschaulicht:

- Abfragen von Geräten in der Nähe
- Verbindung mit Gerät herstellen
- Auflisten der unterstützten Dienste und Merkmale des Geräts
- Lese-und Schreibzugriff auf ein Merkmal
- Benachrichtigungen abonnieren, wenn der Merkmal Wert geändert wird

> [!Important]
> Sie müssen die Funktion "Bluetooth" in " *Package. appxmanifest*" deklarieren.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

> **Wichtige APIs**
>
> - [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth)
> - [**Windows. Devices. Bluetooth. generiertributeprofile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>Übersicht

Entwickler können die APIs im [**Windows. Devices. Bluetooth. generiertributeprofile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) -Namespace verwenden, um auf Bluetooth Le-Geräte zuzugreifen. Bluetooth LE-Geräte machen ihre Funktionen über eine Sammlung folgender Elemente verfügbar:

- Dienste
- Merkmale
- Deskriptoren

Dienste definieren den funktionalen Vertrag des Le-Geräts und enthalten eine Sammlung von Merkmalen, die den Dienst definieren. Diese Merkmale wiederum enthalten Deskriptoren, von denen die Merkmale beschrieben werden. Diese drei Begriffe werden generisch als die Attribute eines Geräts bezeichnet.

Die Bluetooth Le GATT-APIs machen anstelle des Zugriffs auf den unformatierten Transport Objekte und Funktionen verfügbar. Die GATT-APIs ermöglichen auch Entwicklern die Arbeit mit Bluetooth Le-Geräten mit der Möglichkeit, die folgenden Aufgaben auszuführen:

- Attribut Ermittlung ausführen
- Lese-und Schreib Attributwerte
- Registrieren eines Rückrufs für das Merkmal "ValueChanged"-Ereignis

Um eine nützliche Implementierung zu erstellen, muss ein Entwickler über vorherige Kenntnisse über die GATT-Dienste und-Eigenschaften verfügen, die von der Anwendung verwendet werden sollen, und um die spezifischen Merkmals Werte zu verarbeiten, sodass die von der API bereitgestellten Binärdaten in nützliche Daten transformiert werden, bevor Sie dem Benutzer präsentiert werden. Die Bluetooth GATT-APIs machen nur die Grundtypen verfügbar, die für die Kommunikation mit einem Bluetooth LE-Gerät erforderlich sind. Zum Interpretieren der Daten muss ein Anwendungsprofil definiert werden, entweder durch ein Bluetooth SIG-Standardprofil oder mit einem benutzerdefinierten Profil, das von einem Geräteanbieter implementiert wird. Ein Profil begründet einen bindenden Vertrag zwischen Anwendung und Gerät darüber, was von den ausgetauschten Daten dargestellt wird und wie sie zu interpretieren sind.

Aus Gründen der Benutzerfreundlichkeit pflegt Bluetooth SIG eine [Liste der öffentlichen Profile](https://www.bluetooth.com/specifications/adopted-specifications#gattspec), die zur Verfügung stehen.

## <a name="query-for-nearby-devices"></a>Abfragen von Geräten in der Nähe

Es gibt zwei Hauptmethoden, um Geräte in der Nähe abzufragen:

- Devicewatcher in Windows. Devices. Enumeration
- Ankündigen in Windows. Devices. Bluetooth. Ankündigungs-Watcher

Die zweite Methode wird in der Ankündigungs Dokumentation ausführlich erläutert, sodass Sie hier nicht ausführlich erläutert wird. die grundlegende Idee besteht jedoch darin, die Bluetooth-Adresse von Geräten [in der Nähe](ble-beacon.md) zu finden, die den jeweiligen Ankündigungs [Filter](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter)erfüllen. Sobald Sie über die Adresse verfügen, können Sie [bluetoothledevice. frombluetoothaddressasync](/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) aufrufen, um einen Verweis auf das Gerät zu erhalten.

Nun zurück zur devicewatcher-Methode. Ein Bluetooth-Le-Gerät ist genau wie jedes andere Gerät in Windows und kann mithilfe der [enumerationsapis](/uwp/api/Windows.Devices.Enumeration)abgefragt werden. Verwenden Sie die [devicewatcher](/uwp/api/windows.devices.enumeration.devicewatcher) -Klasse, und übergeben Sie eine Abfrage Zeichenfolge, die die zu suchenden Geräte angibt:

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```

Nachdem Sie den devicewatcher gestartet haben, erhalten Sie [DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) für jedes Gerät, das die Abfrage im Handler für das [hinzugefügte](/uwp/api/windows.devices.enumeration.devicewatcher.added) Ereignis für die fraglichen Geräte erfüllt. Eine ausführlichere Betrachtung von devicewatcher finden Sie im kompletten Beispiel [auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

## <a name="connecting-to-the-device"></a>Verbindung mit dem Gerät wird hergestellt

Nachdem ein gewünschtes Gerät ermittelt wurde, verwenden Sie das [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) -Objekt, um das Bluetooth Le-Geräte Objekt für das betreffende Gerät zu erhalten:

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```

Wenn Sie allerdings alle Verweise auf ein bluetoothledevice-Objekt für ein Gerät verwerfen (und wenn keine andere APP auf dem System über einen Verweis auf das Gerät verfügt), wird nach einem kurzen Timeout eine automatische Verbindung getrennt.

```csharp
bluetoothLeDevice.Dispose();
```

Wenn die APP erneut auf das Gerät zugreifen muss, wird durch einfaches erneutes Erstellen des Geräte Objekts und durch den Zugriff auf ein Merkmal (im nächsten Abschnitt erläutert) das Betriebssystem zum erneuten Herstellen der Verbindung bei Bedarf zurückgeführt. Wenn sich das Gerät in der Nähe befindet, erhalten Sie Zugriff auf das Gerät. andernfalls wird ein Fehler mit der Fehlermeldung zurückgegeben.  

## <a name="enumerating-supported-services-and-characteristics"></a>Auflisten unterstützter Dienste und Merkmale

Nachdem Sie nun über ein bluetoothledevice-Objekt verfügen, besteht der nächste Schritt darin, herauszufinden, welche Daten das Gerät verfügbar macht. Der erste Schritt besteht darin, Dienste abzufragen:

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```

Nachdem der Dienst von Interesse identifiziert wurde, besteht der nächste Schritt in der Abfrage von Merkmalen.

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```

Das Betriebssystem gibt eine schreibgeschützte Liste von gattobjects-Objekten zurück, für die Sie dann Vorgänge ausführen können.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Ausführen von Lese-/Schreibvorgängen für ein Merkmal

Das Merkmal ist die grundlegende Einheit der GATT-basierten Kommunikation. Sie enthält einen Wert, der ein bestimmtes Datenelement auf dem Gerät darstellt. Beispielsweise verfügt das Merkmal der Akku Ebene über einen Wert, der den Akku Pegel des Geräts darstellt.

Lesen Sie die Merkmal Eigenschaften, um zu bestimmen, welche Vorgänge unterstützt werden:

```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

Wenn Read unterstützt wird, können Sie den Wert lesen:

```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```

Das Schreiben in ein Merkmal folgt einem ähnlichen Muster:

```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```

> **Tipp**: " [DataReader](/uwp/api/windows.storage.streams.datareader) " und " [DataWriter](/uwp/api/windows.storage.streams.datawriter) " sind bei der Arbeit mit den rohpuffern, die Sie aus vielen der Bluetooth-APIs erhalten, nicht funktionsunfähig.

## <a name="subscribing-for-notifications"></a>Abonnieren von Benachrichtigungen

Stellen Sie sicher, dass das Merkmal entweder angeben oder Benachrichtigen unterstützt (überprüfen Sie die Merkmals Eigenschaften, um sicherzustellen).

> **Abgesehen**von: die Angabe ist zuverlässiger als zuverlässiger, da jedes geänderte Ereignis mit einer Bestätigung vom Client Gerät gekoppelt ist. Die Benachrichtigung ist häufiger, da die meisten GATT-Transaktionen die Stromversorgung Verb einsparen und nicht äußerst zuverlässig sein würden. In jedem Fall werden alle auf der Controller Ebene verarbeitet, sodass die APP nicht beteiligt ist. Wir bezeichnen Sie kollektiv als einfach "Benachrichtigungen", aber jetzt wissen Sie.

Vor dem erhalten von Benachrichtigungen sind zwei Dinge zu berücksichtigen:

- In Client Merkmal konfigurationsdeskriptor schreiben (cccd)
- Behandeln des Merkmals. ValueChanged-Ereignisses

Beim Schreiben auf die cccd wird dem Server Gerät mitgeteilt, dass dieser Client jedes Mal wissen möchte, wenn sich ein bestimmter Merkmals Wert ändert. Gehen Sie dazu wie folgt vor:

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```

Nun wird das ValueChanged-Ereignis des GATT-Merkmals immer dann aufgerufen, wenn der Wert auf dem Remote Gerät geändert wird. Der Handler muss nur noch implementiert werden:

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;

...

void Characteristic_ValueChanged(GattCharacteristic sender,
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```