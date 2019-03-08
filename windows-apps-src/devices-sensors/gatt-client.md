---
title: Bluetooth GATT-Client
description: Dieser Artikel enthält eine Übersicht der Bluetooth Generic Attribute-Profile (GATT) für Universelle Windows-Plattform (UWP)-Apps sowie den Beispielcode für drei häufige Anwendungsfälle.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3ae656b473a4dd5999588057b0ec970645703eec
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635085"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT-Client


**Wichtige APIs**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Dieser Artikel erläutert die Verwendung der Bluetooth Generic Attribut- (GATT-) Client-APIs für Universelle Windows-Plattform- (UWP-) Apps sowie den Beispielcode für allgemeine GATT-Client-Aufgaben:
- Abfrage für Geräte in der Nähe
- Mit Gerät verbinden
- Auflisten der unterstützten Dienste und Merkmale des Geräts
- Lesen und Schreiben eines Merkmals
- Benachrichtigungen für geänderte Merkmale abonnieren

## <a name="overview"></a>Übersicht
Entwickler können mit den APIs im [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)-Namensraum auf Bluetooth LE-Geräte zugreifen. Bluetooth LE-Geräte machen ihre Funktionen über eine Sammlung folgender Elemente verfügbar:

-   Dienste
-   Merkmale
-   Deskriptoren

Dienste definieren den Funktionsvertrag des LE-Geräts und enthalten eine Sammlung von Merkmalen, die den Dienst definieren. Diese Merkmale wiederum enthalten Deskriptoren, von denen die Merkmale beschrieben werden. Diese 3 Begriffe werden in der Regel als die Attribute des Geräts bezeichnet.

Die Bluetooth LE GATT-APIs machen Objekte und Funktionen verfügbar, statt Zugriff auf den Rohtransport zu bieten. Die GATT-APIs ermöglichen Entwicklern zudem mit Bluetooth LE-Geräten zu arbeiten, um folgende Aufgaben auszuführen:

-   Attributermittlung ausführen
-   Lesen und Schreiben von Attributwerten
-   Registrieren eines Rückrufs für das ValueChanged-Ereignis des Merkmals

Um eine sinnvolle Implementierung herzustellen, muss der Entwickler Vorkenntnisse über die GATT-Dienste und -Merkmale besitzen, welche die Anwendung nutzen soll, und die spezifischen Merkmalswerte so verarbeiten, dass die von der API bereitgestellten Binärdaten vor der Präsentation in für den Benutzer nützliche Daten umgewandelt werden. Die Bluetooth GATT-APIs machen nur die Grundtypen verfügbar, die für die Kommunikation mit einem Bluetooth LE-Gerät erforderlich sind. Zum Interpretieren der Daten muss ein Anwendungsprofil definiert werden, entweder durch ein Bluetooth SIG-Standardprofil oder mit einem benutzerdefinierten Profil, das von einem Geräteanbieter implementiert wird. Ein Profil begründet einen bindenden Vertrag zwischen Anwendung und Gerät darüber, was von den ausgetauschten Daten dargestellt wird und wie sie zu interpretieren sind.

Aus Gründen der Benutzerfreundlichkeit pflegt Bluetooth SIG eine [Liste der öffentlichen Profile](https://www.bluetooth.com/specifications/adopted-specifications#gattspec), die zur Verfügung stehen.

## <a name="query-for-nearby-devices"></a>Abfrage für Geräte in der Nähe
Es gibt zwei Hauptmethoden zur Abfrage von Geräten in der Nähe:
- DeviceWatcher in Windows.Devices.Enumeration
- AdvertisementWatcher in Windows.Devices.Bluetooth.Advertisement

Die zweite Methode wird in der [Anzeigen](ble-beacon.md)-Dokumentation ausführlich erläutert, daher nicht an dieser Stelle; das grundlegende Ziel ist jedoch, Bluetooth-Adressen von Geräten in der Nähe zu finden, die den speziellen [Anzeigenfilter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx) erfüllen. Wenn Sie die Adresse haben, können Sie [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) aufrufen, um einen Verweis auf das Gerät zu erhalten. 

Zurück zu der DeviceWatcher-Methode. Bluetooth LE-Geräte funktionieren genau wie alle anderen Geräte in Windows und können mithilfe von [Enumerations-APIs](https://msdn.microsoft.com/library/windows/apps/BR225459) abgefragt werden. Verwenden Sie die [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher)-Klasse, und übergeben Sie eine Abfragezeichenfolge für die gesuchten Geräte: 

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
Nachdem Sie DeviceWatcher gestartet haben, erhalten Sie [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) für jedes Gerät, das die Abfrage im Handler für das [Added](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added)-Ereignis der betreffenden Geräte erfüllt. Eine ausführlichere Beschreibung von DeviceWatcher finden Sie im vollständigen Beispiel [auf Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing). 

## <a name="connecting-to-the-device"></a>Verbinden mit dem Gerät
Sobald ein gewünschtes Gerät erkannt wird, verwenden Sie die [DeviceInformation.ID](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id), um das Bluetooth LE Device-Objekt für das Gerät abzurufen: 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
Andererseits führt das Löschen aller Verweise auf das BluetoothLEDevice-Objekt eines Geräts dazu (sofern keine andere App im System auf das Gerät verweist), das die Verbindung nach einem kurzen Zeitlimit automatisch getrennt wird. 

```csharp
bluetoothLeDevice.Dispose();
```
Wenn die App erneut auf das Gerät zugreifen muss, können das Geräteobjekt und der Zugriff auf eine Eigenschaft (siehe nächster Abschnitt) einfach erneuert werden, damit das Betriebssystem die Verbindung wiederherstellt. Wenn es in Reichweite ist, erhalten Sie Zugriff auf das Gerät; andernfalls gibt es einen DeviceUnreachable-Fehler zurück.  

## <a name="enumerating-supported-services-and-characteristics"></a>Auflisten unterstützter Dienste und Merkmale
Sobald Sie ein BluetoothLEDevice-Objekt haben, ist der nächste Schritt, die vom Gerät angezeigten Daten zu erkennen. Der erste Schritt dazu ist die Abfrage der Dienste: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Nachdem der gesuchte Dienst identifiziert wurde, ist der nächste Schritt die Abfrage der Merkmale. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
Das Betriebssystem gibt eine ReadOnly-Liste der GattCharacteristic-Objekte zurück, mit denen Sie Vorgänge durchführen können.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Lese-/Schreibvorgänge auf einem Merkmal

Das Merkmal ist die grundlegende Einheit der GATT-basierten Kommunikation. Es enthält einen Wert, der ein eindeutiges Datenelement auf dem Gerät darstellt. Zum Beispiel enthält das Akkustand-Merkmal einen Wert, der den Ladestand des Geräteakkus angibt.

Lesen Sie die Merkmaleigenschaften, um festzustellen, welche Vorgänge unterstützt werden:
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

Wenn Lesen unterstützt wird, können Sie den Wert lesen: 
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
// WriteByte used for simplicity. Other commmon functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattReadResult result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result.Status == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```
> **Tipp**: Lehnen Sie sich mit der Verwendung von [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) und [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx). Diese Funktionalität wird unentbehrlich für die Arbeit mit den unformatierten Puffern, die viele Bluetooth-APIs liefern. 
## <a name="subscribing-for-notifications"></a>Abonnieren von Benachrichtigungen

Achten Sie darauf, ob das Merkmal die Funktionen „Indicate“ oder „Notify“ unterstützt (Eigenschaften des Merkmals prüfen). 

> **Reserviert**: Anzugeben ist mehr als zuverlässig betrachtet, da jedes Ereignis Wert geändert, die mit einer Bestätigung vom Clientgerät gekoppelt ist. „Notify“ ist weiter verbreitet, da die meisten GATT-Transaktionen eher Energie sparen als äußerst zuverlässig zu sein. In jedem Fall wird all dies auf Controllerebene behandelt, sodass die App nicht beteiligt wird. Dies zur Information; zusammenfassend nennen wir diese nur „Benachrichtigungen“. 

Bevor Sie Benachrichtigungen erhalten, sind zwei Dinge zu erledigen:
- Schreiben in den Clientmerkmals-Konfigurationsdeskriptor (Client Characteristic Configuration Descriptor, CCCD)
- Behandeln des Characteristic.ValueChanged-Ereignisses

Das Schreiben in den CCCD teilt dem Server mit, dass dieser Client immer erfahren möchte, wenn sich der Wert dieses Merkmals ändert. Gehen Sie dazu wie folgt vor: 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
Das GattCharacteristic-ValueChanged-Ereignis wird jedes Mal aufgerufen, wenn der Wert auf dem Remotegerät geändert wird. Sie müssen nur noch den Handler implementieren: 

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;
// ... 

void Characteristic_ValueChanged(GattCharacteristic sender, 
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```


