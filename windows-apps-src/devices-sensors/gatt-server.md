---
title: Bluetooth GATT-Server
description: Dieser Artikel bietet eine Übersicht des Bluetooth Generic Attribute- (GATT-) Profilservers für Universelle Windows-Plattform- (UWP-) Apps sowie den Beispielcode für häufige Anwendungsfälle.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 551f8b925ffd56950ba893da7b81fefb4579f558
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635135"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth GATT-Server


**Wichtige APIs**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


Dieser Artikel beschreibt Bluetooth Generic Attribut- (GATT-) Server-APIs für Universelle Windows-Plattform- (UWP-) Apps sowie den Beispielcode für allgemeine GATT-Serveraufgaben: 
- Definieren der unterstützten Dienste
- Veröffentlichen des Servers, damit er von Remote-Clients erkannt werden kann
- Anzeigen der Unterstützung für den Dienst
- Antworten auf Lese- und Schreibanfragen
- Versenden von Benachrichtigungen an abonnierte Clients

## <a name="overview"></a>Übersicht
Windows wird in der Regel als Client ausgeführt. Dennoch gibt es viele Szenarien, in denen Windows auch Bluetooth LE GATT-Serveraufgaben übernehmen muss. In fast allen Szenarien, die IoT-Geräte und plattformübergreifende BLE-Kommunikation betreffen, muss Windows als GATT-Server fungieren. Auch das Versenden von Benachrichtigungen an Wearable-Geräte in der Nähe ist ein typisches Szenario geworden, das diese Technologie erfordert.  
> Achten Sie darauf, dass Ihnen alle Konzepte der [GATT-Client Dokumente](gatt-client.md) klar sind, bevor Sie fortfahren.  

Servervorgänge betreffen vorwiegend den Dienstanbieter und die GattLocalCharacteristic. Diese beiden Klassen bieten Funktionen, die Sie deklarieren, implementieren und eine Hierarchie von Daten auf einem Remotegerät bereitstellen benötigt.

## <a name="define-the-supported-services"></a>Definieren der unterstützten Dienste
Ihre App kann einen oder mehrere Dienste deklarieren, die von Windows veröffentlicht werden. Jeder Dienst wird durch eine UUID eindeutig identifiziert. 

### <a name="attributes-and-uuids"></a>Attribute und UUIDs
Alle Dienste, Merkmale und Deskriptoren werden durch eindeutige 128-Bit-UUIDs definiert.
> Alle Windows-APIs verwenden den Begriff GUID, aber der Bluetooth-Standard definiert diese als UUIDs. Da diese Begriffe für unsere Zwecke austauschbar sind, verwenden wir weiterhin den Begriff UUID. 

Wenn das Attribut nach dem Bluetooth-SIG-Standard definiert ist, hat es auch eine entsprechend kurze 16-Bit-ID (z. B. ist die Akkustand-UUID: 0000**2A19**-0000-1000-8000-00805F9B34FB, die kurze ID ist 0x2A19). Diese Standard-UUIDs können in [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) und [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx) eingesehen werden.

Wenn Ihre App einen eigenen Dienst implementiert, muss eine benutzerdefinierte UUID erzeugt werden. Dazu wählen Sie einfach in Visual Studio die Funktion „Tools -> CreateGuid“ (mit Option 5, um das Format "xxxxxxxx-xxxx -... xxxx" zu erhalten). Mit dieser UUID können Sie neue lokale Dienste, Merkmale oder Deskriptoren deklarieren.

#### <a name="restricted-services"></a>Eingeschränkte Dienste
Folgende Dienste sind vom System reserviert und können derzeit nicht veröffentlicht werden:
1. Geräte-Informationsdienst (DIS)
2. Generischer Attribut-Profildienst (GATT)
3. Generischer Zugriff-Profildienst (GAP)
4. Eingabegerätedienst (HOGP)
5. Abtastparameter-Dienst (SCP)

> Der Versuch, einen blockierten Dienst zu erstellen, gibt bei Aufruf von CreateAsync die Meldung BluetoothError.DisabledByPolicy zurück.

#### <a name="generated-attributes"></a>Generierte Attribute
Folgende Deskriptoren werden vom System automatisch erzeugt, basierend auf den bei Erstellung des Merkmals angegebenen GattLocalCharacteristicParameters:
1. Client-Merkmalkonfiguration (wenn das Merkmal als mitteilbar bzw. meldbar gekennzeichnet ist).
2. Merkmal-Benutzerbeschreibung (wenn die UserDescription-Eigenschaft festgelegt ist). Weitere Informationen dazu finden Sie in der Eigenschaft GattLocalCharacteristicParameters.UserDescription.
3. Merkmals-Format (ein Deskriptor für jedes angegebene Präsentationsformat).  Weitere Informationen dazu finden Sie in der Eigenschaft GattLocalCharacteristicParameters.PresentationFormats.
4. Merkmals-Aggregatsformat (wenn mehr als ein Präsentationsformat angegeben ist).  Weitere Informationen dazu finden Sie in der Eigenschaft GattLocalCharacteristicParameters.See PresentationFormats.
5. Erweiterte Merkmals-Eigenschaften (wenn die Eigenschaft mit dem Erweiterte-Eigenschaften-Bit gekennzeichnet ist).

> Der Wert des Erweiterte-Eigenschaften-Deskriptors wird durch die Merkmals-Eigenschaften ReliableWrites und WritableAuxiliaries bestimmt.

> Der Versuch, einen reservierten Deskriptor zu erstellen, führt zu einer Ausnahme.

> Beachten Sie, dass Broadcast derzeit nicht unterstützt wird.  Die Angabe von Broadcast GattCharacteristicProperty führt zu einer Ausnahme.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>Um die Hierarchie der Dienste und Merkmale erstellen
Der GattServiceProvider dient zum Erstellen und Anzeigen der Root-Primärdienst-Definition.  Jeder Dienst erfordert ein eigenes ServiceProvider-Objekt, das eine GUID akzeptiert: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Primärdienste sind die oberste Ebene in der GATT-Struktur. Primärdienste enthalten Merkmale sowie andere (sogenannte 'enthaltene' oder sekundäre) Dienste. 

Tragen Sie nun die für den Dienst erforderlichen Merkmale und Deskriptoren ein:

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
Wie oben gezeigt, ist dies auch der geeignete Ort, um Ereignishandler für die Vorgänge zu deklarieren, die von den Merkmalen unterstützt werden.  Um auf Anfragen korrekt zu reagieren, muss die App einen Ereignishandler für jeden Anforderungstyp definieren und festlegen, der vom Attribut unterstützt wird.  Wird kein Handler registriert, wird die Anfrage sofort mit der Meldung *UnlikelyError* vom System geschlossen.

### <a name="constant-characteristics"></a>Konstante Merkmale
Es gibt mitunter auch Merkmalswerte, die sich während der ganzen Lebensdauer einer App nicht ändern. In diesem Fall sollte ein konstantes Merkmal deklariert werden, um unnötige App-Aktivierungen zu vermeiden: 

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
## <a name="publish-the-service"></a>Den Dienst veröffentlichen
Sobald der Dienst vollständig definiert ist, ist der nächste Schritt, die Unterstützung für den Dienst zu veröffentlichen. Dies teilt dem Betriebssystem mit, den Dienst auszugeben, wenn Remotegeräte eine Dienstermittlung durchführen.  Sie müssen die beiden Eigenschaften IsDiscoverable und IsConnectable festlegen:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: Kündigt den Anzeigenamen für externe Geräte in der Ankündigung, sodass das Gerät erkennbar.
- **IsConnectable**:  Kündigt eine verbindungsfähige Ankündigung für die Verwendung in peripheren Rolle an.

> Wenn ein Dienst sowohl erkennbar und verbindbar ist, fügt das System die Dienst-UUID zum Ankündigungspaket hinzu.  Das Ankündigungspaket umfasst nur 31 Byte, von denen 16 auf die 128-Bit-UUID entfallen!

> Beachten Sie: Wird ein Dienst im Vordergrund veröffentlicht, muss eine Anwendung, die angehalten wird, den Aufruf „StopAdvertising“ verwenden.

## <a name="respond-to-read-and-write-requests"></a>Antworten auf Lese- und Schreibanfragen
Wie oben bei der Deklaration von benötigten Eigenschaften gezeigt, haben GattLocalCharacteristics 3 Ereignistypen: ReadRequested, WriteRequested und SubscribedClientsChanged.

### <a name="read"></a>Read
Wenn ein Remotegerät den Wert eines Merkmals zu lesen versucht (der kein konstanter Wert ist), wird das Ereignis ReadRequested aufgerufen. Das Merkmal, für das Lesen sowie Argumente (die Informationen über das Remotegerät enthalten) aufgerufen wurden, wird an den Delegaten übergeben: 

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

### <a name="write"></a>Schreiben
Wenn ein Remotegerät einen Wert in ein Merkmal zu schreiben versucht, wird das Ereignis WriteRequested aufgerufen, das Details über das Remotegerät, das zu schreibende Merkmal und den Wert enthält: 

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
Es gibt 2 Arten von Schreibvorgängen: mit und ohne Antwort. Verwenden Sie GattWriteOption (eine Eigenschaft des GattWriteRequest-Objekts), um festzustellen, welchen Schreibvorgang das Remotegerät ausführt. 

## <a name="send-notifications-to-subscribed-clients"></a>Versenden von Benachrichtigungen an abonnierte Clients
Die wichtigste und häufigste Funktion eines GATT-Servers sind Benachrichtigungen, die Daten an Remotegeräte versenden. In manchen Fällen möchten Sie alle abonnierten Clients benachrichtigen, in anderen Fällen die Geräte auswählen, an welche der neue Wert gesendet werden soll: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Wenn ein neues Gerät Benachrichtigungen abonniert, wird das Ereignis SubscribedClientsChanged aufgerufen: 

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
> Beachten Sie, dass eine Anwendung die maximale Größe von Benachrichtigungen für einen Client mit der Eigenschaft MaxNotificationSize abrufen kann.  Alle Daten, die den Maximalwert überschreiten, werden vom System gekürzt.
