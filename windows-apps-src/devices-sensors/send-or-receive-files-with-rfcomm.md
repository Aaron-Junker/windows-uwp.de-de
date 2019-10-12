---
ms.assetid: 5B3A6326-15EE-4618-AA8C-F1C7FB5232FB
title: Bluetooth RFCOMM
description: Dieser Artikel enthält eine Übersicht über Bluetooth RFCOMM in UWP-Apps (Universelle Windows-Plattform) sowie Beispielcode zum Senden oder Empfangen einer Datei.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: f38adc3de17c699c7a19bc28d201c6a78c000688
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281810"
---
# <a name="bluetooth-rfcomm"></a>Bluetooth RFCOMM

**Wichtige APIs**

-   [**Windows. Devices. Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
-   [**Windows. Devices. Bluetooth. RFCOMM**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm)

Dieser Artikel enthält eine Übersicht über Bluetooth RFCOMM in UWP-Apps (Universelle Windows-Plattform) sowie Beispielcode zum Senden oder Empfangen einer Datei.

## <a name="overview"></a>Übersicht

Die APIs im [**Windows.Devices.Bluetooth.Rfcomm**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm)-Namespace setzen auf vorhandenen Mustern für „Windows.Devices“ auf, einschließlich [**enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) und [**instantiation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Portable.StorageDevice). Beim Lesen und Schreiben von Daten werden die Vorteile von [**etablierten Datenstrommustern**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) und Objekten in [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) genutzt. SDP-Attribute (Service Discovery Protocol) besitzen einen Wert und einen erwarteten Typ. Einige gängige Geräte verfügen jedoch über fehlerhafte Implementierungen von SDP-Attributen, bei denen der Wert nicht dem erwarteten Typ entspricht. Darüber hinaus sind für viele Verwendungen von RFCOMM zusätzliche SDP-Attribute überhaupt nicht erforderlich. Daher bietet diese API Zugriff auf die nicht analysierten SDP-Daten, aus denen Entwickler die erforderlichen Informationen abrufen können.

Die RFCOMM-APIs stützen sich auf das Konzept der Dienst-IDs (Service Identifiers, SIDs). Obwohl es sich bei einer SID lediglich um eine 128-Bit-GUID handelt, wird sie im Allgemeinen als 16-Bit-Ganzzahl oder als 32-Bit-Ganzzahl angegeben. Die RFCOMM-API bietet einen Wrapper für SIDs, der deren Angabe und Verwendung als 128-Bit-GUIDs und als 32-Bit-Ganzzahlen zulässt, jedoch keine 16-Bit-Ganzzahlen bietet. Dies stellt für die API kein Problem dar, da in den Sprachen automatisch eine Erweiterung auf eine 32-Bit-Ganzzahl erfolgt und die ID immer noch ordnungsgemäß generiert werden kann.

Apps können mehrere Schritte umfassende Gerätevorgänge als Hintergrundaufgabe ausführen, sodass sie bis zum Abschluss ausgeführt werden können, selbst wenn die App in den Hintergrund verschoben und ausgesetzt wird. Dies ermöglicht zuverlässige Gerätewartungen, z. B. Änderungen an dauerhaften Einstellungen oder Firmware sowie Synchronisierung von Inhalten, ohne dass der Benutzer in der Zwischenzeit eine Statusanzeige beobachten muss. Verwenden Sie den [**DeviceServicingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger) für Gerätewartungen und den [**DeviceUseTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) für die Synchronisierung von Inhalten. Beachten Sie Folgendes: Diese Hintergrundaufgaben können den Zeitraum begrenzen, in dem die App im Hintergrund ausgeführt werden kann, und sie sollen nicht den zeitlich unbefristeten Betrieb oder eine unendliche Synchronisierung ermöglichen.

Ein vollständiges Codebeispiel mit Details über den RFCOMM-Vorgang finden Sie im [**Bluetooth Rfcomm Chat-Beispiel**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothRfcommChat) oder auf Github.  

## <a name="send-a-file-as-a-client"></a>Senden einer Datei als Client

Beim Senden einer Datei besteht das einfachste App-Szenario darin, auf Basis eines gewünschten Dienstes eine Verbindung mit einem gekoppelten Gerät herzustellen. Dies umfasst die folgenden Schritte:

-   Verwenden Sie die **rfcommdeviceservice. getdeviceselector @ no__t-1-** Funktionen, um eine AQS-Abfrage zu generieren, die zum Auflisten von gekoppelten Geräte Instanzen des gewünschten Diensts verwendet werden kann.
-   Wählen Sie ein aufgelistetes Gerät aus, erstellen Sie einen [**RfcommDeviceService**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm.RfcommDeviceService), und lesen Sie bei Bedarf die SDP-Attribute (mit [**established data helpers**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) zum Analysieren der Attributdaten).
-   Erstellen Sie einen Socket, und verwenden die Eigenschaften [**RfcommDeviceService.ConnectionHostName**](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionhostname) und [**RfcommDeviceService.ConnectionServiceName**](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionservicename) für [**StreamSocket.ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) und für den Remotegerätedienst mit den entsprechenden Parametern.
-   Folgen die etablierten Datenstrommuster, um Datenblöcke der Datei auszulesen und sie über [**StreamSocket.OutputStream**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.outputstream) an das Gerät zu senden.

```csharp
Windows.Devices.Bluetooth.Rfcomm.RfcommDeviceService _service;
Windows.Networking.Sockets.StreamSocket _socket;

async void Initialize()
{
    // Enumerate devices with the object push service
    var services =
        await Windows.Devices.Enumeration.DeviceInformation.FindAllAsync(
            RfcommDeviceService.GetDeviceSelector(
                RfcommServiceId.ObexObjectPush));

    if (services.Count > 0)
    {
        // Initialize the target Bluetooth BR device
        var service = await RfcommDeviceService.FromIdAsync(services[0].Id);

        // Check that the service meets this App's minimum requirement
        if (SupportsProtection(service) && IsCompatibleVersion(service))
        {
            _service = service;

            // Create a socket and connect to the target
            _socket = new StreamSocket();
            await _socket.ConnectAsync(
                _service.ConnectionHostName,
                _service.ConnectionServiceName,
                SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can wait for
            // the user to take some action, for example, click a button to send a
            // file to the device, which could invoke the Picker and then
            // send the picked file. The transfer itself would use the
            // Sockets API and not the Rfcomm API, and so is omitted here for
            // brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(RfcommDeviceService service)
{
    switch (service.ProtectionLevel)
    {
    case SocketProtectionLevel.PlainSocket:
        if ((service.MaxProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionWithAuthentication)
            || (service.MaxProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionAllowNullAuthentication))
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel.BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel.BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService service)
{
    var attributes = await service.GetSdpRawAttributesAsync(
        BluetothCacheMode.Uncached);
    var attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    var reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute's type
    byte attributeType = reader.ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader.ReadUInt32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Bluetooth.Rfcomm.h>
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService m_service{ nullptr };
Windows::Networking::Sockets::StreamSocket m_socket;

Windows::Foundation::IAsyncAction Initialize()
{
    // Enumerate devices with the object push service.
    Windows::Devices::Enumeration::DeviceInformationCollection services{
        co_await Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService::GetDeviceSelector(
                Windows::Devices::Bluetooth::Rfcomm::RfcommServiceId::ObexObjectPush())) };

    if (services.Size() > 0)
    {
        // Initialize the target Bluetooth BR device.
        Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService service{
            co_await Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService::FromIdAsync(services.GetAt(0).Id()) };

        // Check that the service meets this App's minimum
        // requirement
        if (SupportsProtection(service)
            && co_await IsCompatibleVersion(service))
        {
            m_service = service;

            // Create a socket and connect to the target
            co_await m_socket.ConnectAsync(
                m_service.ConnectionHostName(),
                m_service.ConnectionServiceName(),
                Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can
            // wait for the user to take some action, for example, click
            // a button to send a file to the device, which could
            // invoke the Picker and then send the picked file.
            // The transfer itself would use the Sockets API and
            // not the Rfcomm API, and so is omitted here for
            //brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService const& service)
{
    switch (service.ProtectionLevel())
    {
    case Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket:
        if ((service.MaxProtectionLevel() == Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionWithAuthentication)
            || (service.MaxProtectionLevel() == Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication))
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This application relies on CRC32 checking available in version 2.0 of the service.
const uint32_t SERVICE_VERSION_ATTRIBUTE_ID{ 0x0300 };
const byte SERVICE_VERSION_ATTRIBUTE_TYPE{ 0x0A }; // UINT32.
const uint32_t MINIMUM_SERVICE_VERSION{ 200 };

Windows::Foundation::IAsyncOperation<bool> IsCompatibleVersion(Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService const& service)
{
    auto attributes{
        co_await service.GetSdpRawAttributesAsync(Windows::Devices::Bluetooth::BluetoothCacheMode::Uncached) };

    auto attribute{ attributes.Lookup(SERVICE_VERSION_ATTRIBUTE_ID) };
    auto reader{ Windows::Storage::Streams::DataReader::FromBuffer(attribute) };

    // The first byte contains the attribute's type.
    byte attributeType{ reader.ReadByte() };
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint32_t version{ reader.ReadUInt32() };
        co_return (version >= MINIMUM_SERVICE_VERSION);
    }
}
...
```

```cpp
Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService^ _service;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Enumerate devices with the object push service
    create_task(
        Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            RfcommDeviceService::GetDeviceSelector(
                RfcommServiceId::ObexObjectPush)))
    .then([](DeviceInformationCollection^ services)
    {
        if (services->Size > 0)
        {
            // Initialize the target Bluetooth BR device
            create_task(RfcommDeviceService::FromIdAsync(services[0]->Id))
            .then([](RfcommDeviceService^ service)
            {
                // Check that the service meets this App's minimum
                // requirement
                if (SupportsProtection(service)
                    && IsCompatibleVersion(service))
                {
                    _service = service;

                    // Create a socket and connect to the target
                    _socket = ref new StreamSocket();
                    create_task(_socket->ConnectAsync(
                        _service->ConnectionHostName,
                        _service->ConnectionServiceName,
                        SocketProtectionLevel
                            ::BluetoothEncryptionAllowNullAuthentication)
                    .then([](void)
                    {
                        // The socket is connected. At this point the App can
                        // wait for the user to take some action, for example, click
                        // a button to send a file to the device, which could
                        // invoke the Picker and then send the picked file.
                        // The transfer itself would use the Sockets API and
                        // not the Rfcomm API, and so is omitted here for
                        //brevity.
                    });
                }
            });
        }
    });
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(RfcommDeviceService^ service)
{
    switch (service->ProtectionLevel)
    {
    case SocketProtectionLevel->PlainSocket:
        if ((service->MaxProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionWithAuthentication)
            || (service->MaxProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication))
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService^ service)
{
    auto attributes = await service->GetSdpRawAttributesAsync(
        BluetoothCacheMode::Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute's type
    byte attributeType = reader->ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader->ReadUInt32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

## <a name="receive-file-as-a-server"></a>Empfangen einer Datei als Server

Ein weiteres allgemeines RFCOMM-App-Szenario besteht darin, einen Dienst auf einem PC zu hosten und ihn anderen Geräten zur Verfügung zu stellen.

-   Erstellen eines [**RfcommServiceProvider**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider) zum Ankündigen des gewünschten Diensts.
-   Legen Sie die SDP-Attribute nach Bedarf fest (mit [**verbreiteten Datenhilfsfunktionen**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) zum Erstellen der Attributdaten), und beginnen Sie damit, die SDP-Datensätze anzukündigen, damit sie andere Geräte abrufen können.
-   Zur Herstellung einer Verbindung zu einem Clientgerät erstellen Sie einen Socketlistener, um mit dem Überwachen eingehender Verbindungsanforderungen zu beginnen.
-   Sobald eine Verbindung hergestellt ist, können Sie das verbundene Socket zur späteren Verarbeitung speichern.
-   Folgen Sie etablierten Datenstrommustern, um Datenblöcke aus dem InputStream des Sockets zu lesen und in einer Datei zu speichern.

Um einen RFCOMM-Dienst im Hintergrund beizubehalten, verwenden Sie [**RfcommConnectionTrigger**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.rfcommconnectiontrigger). Die Hintergrundaufgabe wird bei der Verbindungsherstellung mit dem Dienst ausgelöst. Der Entwickler erhält einen Handle für den Socket in der Hintergrundaufgabe. Die Hintergrundaufgabe wird wird lange ausgeführt und so lange beibehalten, wie der Socket verwendet wird.    

```csharp
Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider _provider;

async void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    _provider = await Windows.Devices.Bluetooth.
        RfcommServiceProvider.CreateAsync(RfcommServiceId.ObexObjectPush);

    // Create a listener for this service and start listening
    StreamSocketListener listener = new StreamSocketListener();
    listener.ConnectionReceived += OnConnectionReceived;
    await listener.BindServiceNameAsync(
        _provider.ServiceId.AsString(),
        SocketProtectionLevel
            .BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes(_provider);
    _provider.StartAdvertising(listener);
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider provider)
{
    auto writer = new Windows.Storage.Streams.DataWriter();

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer.WriteUInt32(SERVICE_VERSION);

    auto data = writer.DetachBuffer();
    provider.SdpRawAttributes.Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener listener,
    StreamSocketListenerConnectionReceivedEventArgs args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider.StopAdvertising();
    await listener.Close();
    _socket = args.Socket;

    // The client socket is connected. At this point the App can wait for
    // the user to take some action, for example, click a button to receive a file
    // from the device, which could invoke the Picker and then save the
    // received file to the picked location. The transfer itself would use
    // the Sockets API and not the Rfcomm API, and so is omitted here for
    // brevity.
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Bluetooth.Rfcomm.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider m_provider{ nullptr };
Windows::Networking::Sockets::StreamSocket m_socket;

Windows::Foundation::IAsyncAction Initialize()
{
    // Initialize the provider for the hosted RFCOMM service.
    auto provider{ co_await Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider::CreateAsync(
        Windows::Devices::Bluetooth::Rfcomm::RfcommServiceId::ObexObjectPush()) };

    m_provider = provider;

    // Create a listener for this service and start listening.
    Windows::Networking::Sockets::StreamSocketListener listener;
    listener.ConnectionReceived({ this, &MainPage::OnConnectionReceived });

    co_await listener.BindServiceNameAsync(m_provider.ServiceId().AsString(),
        Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes();
    m_provider.StartAdvertising(listener);
}

const uint32_t SERVICE_VERSION_ATTRIBUTE_ID{ 0x0300 };
const byte SERVICE_VERSION_ATTRIBUTE_TYPE{ 0x0A };   // UINT32.
const uint32_t SERVICE_VERSION{ 200 };

void InitializeServiceSdpAttributes()
{
    Windows::Storage::Streams::DataWriter writer;

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer.WriteUInt32(SERVICE_VERSION);

    auto data{ writer.DetachBuffer() };
    m_provider.SdpRawAttributes().Insert(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    Windows::Networking::Sockets::StreamSocketListener const& listener,
    Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs const& args)
{
    // Stop advertising/listening so that we're only serving one client
    m_provider.StopAdvertising();
    listener.Close();
    m_socket = args.Socket();

    // The client socket is connected. At this point the application can wait for
    // the user to take some action, for example, click a button to receive a
    // file from the device, which could invoke the Picker and then save
    // the received file to the picked location. The transfer itself
    // would use the Sockets API and not the Rfcomm API, and so is
    // omitted here for brevity.
}
...
```

```cpp
Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider^ _provider;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    create_task(Windows::Devices::Bluetooth.
        RfcommServiceProvider::CreateAsync(
            RfcommServiceId::ObexObjectPush))
    .then([](RfcommServiceProvider^ provider) -> task<void> {
        _provider = provider;

        // Create a listener for this service and start listening
        auto listener = ref new StreamSocketListener();
        listener->ConnectionReceived += ref new TypedEventHandler<
                StreamSocketListener^,
                StreamSocketListenerConnectionReceivedEventArgs^>
           (&OnConnectionReceived);
        return create_task(listener->BindServiceNameAsync(
            _provider->ServiceId->AsString(),
            SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication));
    }).then([listener](void) {
        // Set the SDP attributes and start advertising
        InitializeServiceSdpAttributes(_provider);
        _provider->StartAdvertising(listener);
    });
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider^ provider)
{
    auto writer = ref new Windows::Storage::Streams::DataWriter();

    // First write the attribute type
    writer->WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer->WriteUInt32(SERVICE_VERSION);

    auto data = writer->DetachBuffer();
    provider->SdpRawAttributes->Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener^ listener,
    StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider->StopAdvertising();
    create_task(listener->Close())
    .then([args](void) {
        _socket = args->Socket;

        // The client socket is connected. At this point the App can wait for
        // the user to take some action, for example, click a button to receive a
        // file from the device, which could invoke the Picker and then save
        // the received file to the picked location. The transfer itself
        // would use the Sockets API and not the Rfcomm API, and so is
        // omitted here for brevity.
    });
}
```
