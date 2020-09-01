---
title: Bluetooth-GATT-Server
description: Dieser Artikel bietet eine Übersicht über Bluetooth Generic Attribute profile (GATT) Server for universelle Windows-Plattform-Apps (UWP) sowie Beispielcode für gängige Anwendungsfälle.
ms.date: 06/26/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b4cf26d4f4fe5fa33f9f214da32263031188c5f0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172254"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth-GATT-Server

In diesem Artikel werden die Server-APIs für Bluetooth Generic Attribute (GATT) für universelle Windows-Plattform-Apps (UWP) sowie Beispielcode für gängige GATT-Server Aufgaben veranschaulicht:

- Definieren der unterstützten Dienste
- Veröffentlichen des Servers, damit er von Remote Clients erkannt werden kann
- Unterstützung für Dienst ankündigen
- Antworten auf Lese-und Schreib Anforderungen
- Senden von Benachrichtigungen an abonnierte Clients

> [!Important]
> Sie müssen die Funktion "Bluetooth" in " *Package. appxmanifest*" deklarieren.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

**Wichtige APIs**

- [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth)
- [**Windows. Devices. Bluetooth. generiertributeprofile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>Übersicht

Windows funktioniert in der Regel in der Client Rolle. Dennoch entstehen viele Szenarien, die erfordern, dass Windows auch als Bluetooth Le GATT-Server fungiert. Fast alle Szenarien für IOT-Geräte, zusammen mit der meisten plattformübergreifenden ble-Kommunikation, erfordern Windows als GATT-Server. Außerdem ist das Senden von Benachrichtigungen an nahe gelegene, tragbaren Geräte ein gängiges Szenario, das auch diese Technologie erfordert.  

Server Vorgänge werden um den Dienstanbieter und das gattlocalmerkmal herum gedreht. Diese beiden Klassen bieten die Funktionalität, die erforderlich ist, um eine Hierarchie von Daten auf einem Remote Gerät zu deklarieren, zu implementieren und verfügbar zu machen.

## <a name="define-the-supported-services"></a>Definieren der unterstützten Dienste
Ihre APP kann einen oder mehrere Dienste deklarieren, die von Windows veröffentlicht werden. Jeder Dienst wird durch eine UUID eindeutig identifiziert.

### <a name="attributes-and-uuids"></a>Attribute und UUIDs
Jeder Dienst, jedes Merkmal und jeder Deskriptor wird durch seine eigene eindeutige 128-Bit-UUID definiert.
> Die Windows-APIs verwenden alle den Begriff GUID, aber der Bluetooth-Standard definiert Sie als UUIDs. Zu diesem Zweck sind diese beiden Begriffe austauschbar, sodass wir weiterhin den Begriff uuid verwenden werden. 

Wenn das Attribut "Standard" und durch das von der Bluetooth-Definition definiert definiert ist, verfügt es auch über eine entsprechende 16-Bit-kurzkennung (z. b. ist der Akku Pegel-UUID 0000**2A19**-0000-1000-8000-00805f 9b34fb und die kurze ID 0x2a19). Diese standardmäßigen UUIDs finden Sie in der Datei " [gattserviceuuids](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) " und " [gattcharakteristicuuids](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids)".

Wenn Ihre APP ihren eigenen benutzerdefinierten Dienst implementiert, muss eine benutzerdefinierte UUID generiert werden. Dies kann in Visual Studio problemlos über die Tools-> "kreateguid" durchgeführt werden (verwenden Sie Option 5 zum Aufrufen im "xxxxxxxx-xxxx-... xxxx "Format). Diese UUID kann jetzt verwendet werden, um neue lokale Dienste, Merkmale oder Deskriptoren zu deklarieren.

#### <a name="restricted-services"></a>Eingeschränkte Dienste
Die folgenden Dienste sind vom System reserviert und können zurzeit nicht veröffentlicht werden:
1. Geräte Informationsdienst (Device Information Service, DIS)
2. Generischer Attribut Profil Dienst (GATT)
3. Generischer Zugriffs Profil Dienst (GAP)
4. Eingabegeräte-Dienst (gehostet)
5. Überprüfungs Parameter Dienst (SCP)

> Der Versuch, einen blockierten Dienst zu erstellen, führt dazu, dass bluedeotherror. disabledbypolicy vom Aufrufen von "anateasync" zurückgegeben wird.

#### <a name="generated-attributes"></a>Generierte Attribute
Die folgenden Deskriptoren werden vom System automatisch generiert, basierend auf den bei der Erstellung des Merkmalen bereitgestellten gattlocalcharakteristicpara Metern:
1. Konfiguration von Client Merkmalen (wenn das Merkmal als fest stellbar oder benachrichtigbar gekennzeichnet ist).
2. Merkmals Benutzerbeschreibung (wenn die userdescription-Eigenschaft festgelegt ist). Weitere Informationen finden Sie unter der Eigenschaft "gattlocalcharakteristicparameters. userdescription".
3. Merkmal Format (ein Deskriptor für jedes angegebene Präsentationsformat).  Weitere Informationen finden Sie unter der Eigenschaft "gattlocalcharakteristicparameters. presentationformats".
4. Charakteristisches Aggregat Format (wenn mehr als ein Darstellungsformat angegeben ist).  Gattlocalcharakteristicparameters. Weitere Informationen finden Sie unter presentationformats-Eigenschaft.
5. Merkmale erweiterter Eigenschaften (wenn das Merkmal mit dem Feld "Erweiterte Eigenschaften" gekennzeichnet ist).

> Der Wert des Deskriptors für erweiterte Eigenschaften wird über die Eigenschaften "reliablewrite" und "Beschreib tableauxiliaries" bestimmt.

> Der Versuch, einen reservierten Deskriptor zu erstellen, führt zu einer Ausnahme.

> Beachten Sie, dass Broadcast zurzeit nicht unterstützt wird.  Das Angeben der Broadcast-Eigenschaft "gattbezeichsticproperty" führt zu einer Ausnahme.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>Erstellen der Hierarchie von Diensten und Merkmalen
Der gattserviceprovider wird verwendet, um die primäre Dienst Definition zu erstellen und ankündigen.  Jeder Dienst benötigt sein eigenes Serviceprovider-Objekt, das eine GUID annimmt: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Primäre Dienste sind die oberste Ebene der GATT-Struktur. Primäre Dienste enthalten Merkmale und andere Dienste (als "enthaltene" oder sekundäre Dienste bezeichnet). 

Füllen Sie nun den Dienst mit den erforderlichen Merkmalen und Deskriptoren:

```csharp
GattLocalCharacteristicResult characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid1, ReadParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_readCharacteristic = characteristicResult.Characteristic;
_readCharacteristic.ReadRequested += ReadCharacteristic_ReadRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid2, WriteParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_writeCharacteristic = characteristicResult.Characteristic;
_writeCharacteristic.WriteRequested += WriteCharacteristic_WriteRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid3, NotifyParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_notifyCharacteristic = characteristicResult.Characteristic;
_notifyCharacteristic.SubscribedClientsChanged += SubscribedClientsChanged;
```
Wie oben gezeigt, ist dies auch ein guter Ort zum Deklarieren von Ereignis Handlern für die Vorgänge, die von den einzelnen Merkmalen unterstützt werden.  Um ordnungsgemäß auf Anforderungen zu reagieren, muss eine APP definiert und einen Ereignishandler für jeden Anforderungstyp festlegen, der vom Attribut unterstützt wird.  Wenn Sie einen Handler nicht registrieren, führt dies dazu, dass die Anforderung sofort mit *unlikelyerror* vom System abgeschlossen wird.

### <a name="constant-characteristics"></a>Konstante Merkmale
Manchmal gibt es Merkmals Werte, die während der Lebensdauer der APP nicht geändert werden. In diesem Fall empfiehlt es sich, ein konstantes Merkmal zu deklarieren, um eine unnötige App-Aktivierung zu verhindern: 

```csharp
byte[] value = new byte[] {0x21};
var constantParameters = new GattLocalCharacteristicParameters
{
    CharacteristicProperties = (GattCharacteristicProperties.Read),
    StaticValue = value.AsBuffer(),
    ReadProtectionLevel = GattProtectionLevel.Plain,
};

var characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid4, constantParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
```
## <a name="publish-the-service"></a>Veröffentlichen des Dienstanbieter
Nachdem der Dienst vollständig definiert wurde, besteht der nächste Schritt im Veröffentlichen der Unterstützung für den Dienst. Dadurch wird das Betriebssystem darüber informiert, dass der Dienst zurückgegeben werden sollte, wenn Remote Geräte eine Dienst Ermittlung ausführen.  Sie müssen zwei Eigenschaften festlegen: isdiscoverable und isconnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **Isdiscoverable**: gibt den anzeigen Amen für Remote Geräte in der Ankündigung an, sodass das Gerät erkennbar ist.
- **Isconnectable**: gibt eine Verbindungs fähige Ankündigung an, die in der Peripherie Rolle verwendet werden soll.

> Wenn ein Dienst sichtbar und Verbindungs fähig ist, fügt das System dem Ankündigungs Paket die Dienst-UUID hinzu.  Das Ankündigungs Paket enthält nur 31 bytes, und eine 128-Bit-UUID dauert 16 von Ihnen!

> Beachten Sie Folgendes: Wenn ein Dienst im Vordergrund veröffentlicht wird, muss eine Anwendung eine Stopp Werbung aufzurufen, wenn die Anwendung angehalten wird.

## <a name="respond-to-read-and-write-requests"></a>Antworten auf Lese-und Schreib Anforderungen
Wie oben beim Deklarieren der erforderlichen Merkmale gezeigt, haben die gattlocalcharacteristics drei Arten von Ereignissen: "schreibgeschützt", "beschreibunden" und "abonniert".

### <a name="read"></a>Lesen
Wenn ein Remote Gerät versucht, einen Wert aus einem Merkmal zu lesen (und es sich dabei nicht um einen konstanten Wert handelt), wird das Ereignis "Schreib angefordert" aufgerufen. Das Merkmal, auf dem der Lesevorgang aufgerufen wurde, und args (mit Informationen über das Remote Gerät) werden an den-Delegaten weitergegeben: 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ... 

async void ReadCharacteristic_ReadRequested(GattLocalCharacteristic sender, GattReadRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    // Our familiar friend - DataWriter.
    var writer = new DataWriter();
    // populate writer w/ some data. 
    // ... 

    var request = await args.GetRequestAsync();
    request.RespondWithValue(writer.DetachBuffer());
    
    deferral.Complete();
}
``` 

### <a name="write"></a>Write
Wenn ein Remote Gerät versucht, einen Wert in ein Merkmal zu schreiben, wird das Ereignis "Write Event Event" mit Details zum Remote Gerät, das Merkmal, in das geschrieben werden soll, und den Wert selbst aufgerufen: 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ...

async void WriteCharacteristic_WriteRequested(GattLocalCharacteristic sender, GattWriteRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    var request = await args.GetRequestAsync();
    var reader = DataReader.FromBuffer(request.Value);
    // Parse data as necessary. 

    if (request.Option == GattWriteOption.WriteWithResponse)
    {
        request.Respond();
    }
    
    deferral.Complete();
}
```
Es gibt zwei Arten von Schreibvorgängen: und ohne Antwort. Verwenden Sie die Option "uriwrite-Option" (eine Eigenschaft für das "gattschreiterequest"-Objekt), um herauszufinden, welche Art von Schreibvorgang das Remote Gerät ausführt. 

## <a name="send-notifications-to-subscribed-clients"></a>Senden von Benachrichtigungen an abonnierte Clients
Bei den meisten GATT-Server Vorgängen führen Benachrichtigungen die entscheidende Funktion aus, um Daten auf die Remote Geräte zu übertragen. In manchen Fällen möchten Sie alle abonnierten Clients benachrichtigen, aber Sie können auch andere Geräte auswählen, an die der neue Wert gesendet werden soll: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Wenn ein neues Gerät Benachrichtigungen abonniert, wird das abonnierte Ereignis aufgerufen: 

```csharp
characteristic.SubscribedClientsChanged += SubscribedClientsChanged;
// ...

void _notifyCharacteristic_SubscribedClientsChanged(GattLocalCharacteristic sender, object args)
{
    List<GattSubscribedClient> clients = sender.SubscribedClients;
    // Diff the new list of clients from a previously saved one 
    // to get which device has subscribed for notifications. 

    // You can also just validate that the list of clients is expected for this app.  
}

```
> Beachten Sie, dass eine Anwendung die maximale Benachrichtigungs Größe für einen bestimmten Client mit der maxnotificationsize-Eigenschaft erhalten kann.  Alle Daten, die größer als die maximale Größe sind, werden vom System abgeschnitten.