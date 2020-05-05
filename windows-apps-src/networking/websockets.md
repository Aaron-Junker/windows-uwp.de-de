---
description: WebSockets stellt einen Mechanismus für die schnelle und sichere bidirektionale Kommunikation zwischen einem Client und einem Server über das Web mithilfe von HTTP(S) bereit und unterstützt sowohl UTF-8-Nachrichten als auch binäre Nachrichten.
title: WebSockets
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, netzwerk, websocket, messagewebsocket, streamwebsocket
ms.localizationpriority: medium
ms.openlocfilehash: 221ab5e0647fe95e8d715fc320ba2b9c1bee2dfe
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684961"
---
# <a name="websockets"></a>WebSockets
WebSockets stellt einen Mechanismus für die schnelle und sichere bidirektionale Kommunikation zwischen einem Client und einem Server über das Web mithilfe von HTTP(S) bereit und unterstützt sowohl UTF-8-Nachrichten als auch binäre Nachrichten.

Mit dem [WebSocket-Protokoll](https://tools.ietf.org/html/rfc6455) werden Daten unmittelbar über eine Vollduplex-Einzelsocketverbindung übertragen, wodurch Nachrichten von beiden Endpunkten in Echtzeit gesendet und empfangen werden können. WebSockets eignen sich ideal für den Einsatz in Multiplayer-Spielen (einschließlich Echtzeit- und rundenbasierter Spiele), Sofortbenachrichtigungen aus sozialen Netzwerken oder die aktuelle Anzeige von Aktien- oder Wetterdaten einschließen, sowie für Apps, für die die sichere und schnelle Datenübertragung erforderlich ist.

Zum Herstellen einer WebSocket-Verbindung wird zwischen dem Client und dem Server ein spezieller HTTP-basierter Handshake ausgetauscht. Wenn dies erfolgreich war, wird das Anwendungsschichtprotokoll mithilfe der zuvor hergestellten TCP-Verbindung von HTTP auf WebSockets „aktualisiert“. Sobald dies erfolgt ist, ist HTTP nicht mehr relevant. Daten können jederzeit mithilfe des WebSocket-Protokolls von beiden Endpunkten gesendet und empfangen werden, bis die WebSocket-Verbindung geschlossen wird.

**Hinweis** Ein Client kann WebSockets nur zum Übertragen von Daten verwenden, wenn der Server auch das WebSocket-Protokoll verwendet. Wenn der Server WebSockets nicht unterstützt, dann müssen Sie eine andere Datenübertragungsmethode verwenden.

Die Universelle Windows-Plattform (UWP) bietet Unterstützung für die Client- und Serververwendung von WebSockets. Der [**Windows.Networking.Sockets**](/uwp/api/windows.networking.sockets)-Namespace definiert zwei WebSocket-Klassen für die Verwendung durch Clients&mdash;[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) und [**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket). Hier sehen Sie einen Vergleich dieser beiden WebSocket-Klassen.

| [MessageWebSocket](/uwp/api/windows.networking.sockets.messagewebsocket) | [StreamWebSocket](/uwp/api/windows.networking.sockets.streamwebsocket) |
| - | - |
| Eine gesamte WebSocket-Nachricht wird in einem einzigen Vorgang gelesen/geschrieben. | Abschnitte einer Nachricht können bei jedem Lesevorgang gelesen werden. |
| Geeignet, wenn Nachrichten nicht sehr groß sind. | Geeignet, wenn sehr große Dateien (z. B. Fotos oder Videos) übertragen werden. |
| Unterstützt UTF-8-Nachrichten und binäre Nachrichten. | Unterstützt nur binäre Nachrichten. |
| Ähnlich wie ein [UDP- oder Datagrammsocket](sockets.md#build-a-basic-udp-socket-client-and-server) (in dem Sinne, dass es für häufige, kleine Nachrichten bestimmt ist), jedoch mit der Zuverlässigkeit von TCP, Garantien für die Paketreihenfolge und Überlastungssteuerung. | Ähnlich wie ein [TCP- oder Streamsocket](sockets.md#build-a-basic-tcp-socket-client-and-server). |

## <a name="secure-your-connection-with-tlsssl"></a>Sichern Ihrer Verbindung mit TLS/SSL
In den meisten Fällen sollten Sie eine sichere WebSocket-Verbindung verwenden, damit Ihre gesendeten und empfangenen Daten verschlüsselt sind. Dadurch ist es wahrscheinlicher, dass die Verbindung funktioniert, da andernfalls viele Vermittler wie beispielsweise Firewalls und Proxys unverschlüsselte WebSocket-Verbindungen ablehnen. Das [WebSocket-Protokoll](https://tools.ietf.org/html/rfc6455#section-3) definiert die folgenden zwei URI-Schemas.

| URI-Schema | Zweck |
| - | - |
| wss: | Wird für sichere Verbindungen verwendet, die verschlüsselt werden sollten. |
| ws: | Wird für unverschlüsselte Verbindungen verwendet. |

Verwenden Sie zum Sichern Ihrer WebSocket-Verbindung das URI-Schema `wss:`. Hier ist ein Beispiel.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    var webSocket = new Windows.Networking.Sockets.MessageWebSocket();
    await webSocket.ConnectAsync(new Uri("wss://www.contoso.com/mywebservice"));
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml::Navigation;
...
IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
{
    Windows::Networking::Sockets::MessageWebSocket webSocket;
    co_await webSocket.ConnectAsync(Uri{ L"wss://www.contoso.com/mywebservice" });
}
```

## <a name="use-messagewebsocket-to-connect"></a>Verwenden von MessageWebSocket zum Herstellen einer Verbindung
Mit [**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) kann eine gesamte WebSocket-Nachricht in einem einzigen Vorgang gelesen/geschrieben werden. Folglich ist es geeignet, wenn Nachrichten nicht sehr groß sind. Die Klasse unterstützt UTF-8- und binäre Nachrichten.

Der nachstehende Beispielcode verwendet den WebSocket.org-Echoserver – einen Dienst, der Nachrichten zurück an den Absender sendet.

```csharp
private Windows.Networking.Sockets.MessageWebSocket messageWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.messageWebSocket = new Windows.Networking.Sockets.MessageWebSocket();

    // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
    this.messageWebSocket.Control.MessageType = Windows.Networking.Sockets.SocketMessageType.Utf8;

    this.messageWebSocket.MessageReceived += WebSocket_MessageReceived;
    this.messageWebSocket.Closed += WebSocket_Closed;

    try
    {
        Task connectTask = this.messageWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask();
        connectTask.ContinueWith(_ => this.SendMessageUsingMessageWebSocketAsync("Hello, World!"));
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}

private async Task SendMessageUsingMessageWebSocketAsync(string message)
{
    using (var dataWriter = new DataWriter(this.messageWebSocket.OutputStream))
    {
        dataWriter.WriteString(message);
        await dataWriter.StoreAsync();
        dataWriter.DetachStream();
    }
    Debug.WriteLine("Sending message using MessageWebSocket: " + message);
}

private void WebSocket_MessageReceived(Windows.Networking.Sockets.MessageWebSocket sender, Windows.Networking.Sockets.MessageWebSocketMessageReceivedEventArgs args)
{
    try
    {
        using (DataReader dataReader = args.GetDataReader())
        {
            dataReader.UnicodeEncoding = Windows.Storage.Streams.UnicodeEncoding.Utf8;
            string message = dataReader.ReadString(dataReader.UnconsumedBufferLength);
            Debug.WriteLine("Message received from MessageWebSocket: " + message);
            this.messageWebSocket.Dispose();
        }
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}

private void WebSocket_Closed(Windows.Networking.Sockets.IWebSocket sender, Windows.Networking.Sockets.WebSocketClosedEventArgs args)
{
    Debug.WriteLine("WebSocket_Closed; Code: " + args.Code + ", Reason: \"" + args.Reason + "\"");
    // Add additional code here to handle the WebSocket being closed.
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket m_messageWebSocket;
    winrt::event_token m_messageReceivedEventToken;
    winrt::event_token m_closedEventToken;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
        m_messageWebSocket.Control().MessageType(Windows::Networking::Sockets::SocketMessageType::Utf8);

        m_messageReceivedEventToken = m_messageWebSocket.MessageReceived({ this, &MessageWebSocketPage::OnWebSocketMessageReceived });
        m_closedEventToken = m_messageWebSocket.Closed({ this, &MessageWebSocketPage::OnWebSocketClosed });

        try
        {
            co_await m_messageWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
            SendMessageUsingMessageWebSocketAsync(L"Hello, World!");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

private:
    IAsyncAction SendMessageUsingMessageWebSocketAsync(std::wstring message)
    {
        DataWriter dataWriter{ m_messageWebSocket.OutputStream() };
        dataWriter.WriteString(message);

        co_await dataWriter.StoreAsync();
        dataWriter.DetachStream();
        std::wstringstream wstringstream;
        wstringstream << L"Sending message using MessageWebSocket: " << message.c_str() << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
    }

    void OnWebSocketMessageReceived(Windows::Networking::Sockets::MessageWebSocket const& /* sender */, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs const& args)
    {
        try
        {
            DataReader dataReader{ args.GetDataReader() };

            dataReader.UnicodeEncoding(Windows::Storage::Streams::UnicodeEncoding::Utf8);
            auto message = dataReader.ReadString(dataReader.UnconsumedBufferLength());
            std::wstringstream wstringstream;
            wstringstream << L"Message received from MessageWebSocket: " << message.c_str() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            m_messageWebSocket.Close(1000, L"");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    void OnWebSocketClosed(Windows::Networking::Sockets::IWebSocket const& /* sender */, Windows::Networking::Sockets::WebSocketClosedEventArgs const& args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args.Code() << ", Reason: \"" << args.Reason().c_str() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket^ messageWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->messageWebSocket = ref new Windows::Networking::Sockets::MessageWebSocket();

        // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
        this->messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;

        this->messageWebSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::MessageWebSocket^, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs^>(this, &MessageWebSocketPage::WebSocket_MessageReceived);
        this->messageWebSocket->Closed += ref new TypedEventHandler<Windows::Networking::Sockets::IWebSocket^, Windows::Networking::Sockets::WebSocketClosedEventArgs^>(this, &MessageWebSocketPage::WebSocket_Closed);

        try
        {
            auto connectTask = Concurrency::create_task(this->messageWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));
            connectTask.then([this] { this->SendMessageUsingMessageWebSocketAsync(L"Hello, World!"); });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

private:
    void SendMessageUsingMessageWebSocketAsync(Platform::String^ message)
    {
        auto dataWriter = ref new DataWriter(this->messageWebSocket->OutputStream);
        dataWriter->WriteString(message);

        Concurrency::create_task(dataWriter->StoreAsync()).then(
            [=](unsigned int)
        {
            dataWriter->DetachStream();
            std::wstringstream wstringstream;
            wstringstream << L"Sending message using MessageWebSocket: " << message->Data() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
        });
    }

    void WebSocket_MessageReceived(Windows::Networking::Sockets::MessageWebSocket^ sender, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs^ args)
    {
        try
        {
            DataReader^ dataReader = args->GetDataReader();

            dataReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
            Platform::String^ message = dataReader->ReadString(dataReader->UnconsumedBufferLength);
            std::wstringstream wstringstream;
            wstringstream << L"Message received from MessageWebSocket: " << message->Data() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            this->messageWebSocket->Close(1000, L"");
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void WebSocket_Closed(Windows::Networking::Sockets::IWebSocket^ sender, Windows::Networking::Sockets::WebSocketClosedEventArgs^ args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args->Code << ", Reason: \"" << args->Reason->Data() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

### <a name="handle-the-messagewebsocketmessagereceived-and-messagewebsocketclosed-events"></a>Behandeln der Ereignisse „MessageWebSocket.MessageReceived” und „MessageWebSocket.Closed”
Wie im obigen Beispiel gezeigt, sollten Sie vor dem Herstellen einer Verbindung und Senden von Daten mit einem **MessageWebSocket** die Ereignisse [**MessageWebSocket.MessageReceived**](/uwp/api/windows.networking.sockets.messagewebsocket.MessageReceived) und [**MessageWebSocket.Closed**](/uwp/api/windows.networking.sockets.messagewebsocket.Closed) abonnieren.
 
**MessageReceived** wird ausgelöst, wenn Daten empfangen werden. Der Zugriff auf die Daten erfolgt über [**MessageWebSocketMessageReceivedEventArgs**](/uwp/api/windows.networking.sockets.messagewebsocketmessagereceivedeventargs). **Closed** wird ausgelöst, wenn der Client oder Server den Socket schließt.
 
### <a name="send-data-on-a-messagewebsocket"></a>Senden von Daten an einem MessageWebSocket
Sobald eine Verbindung hergestellt ist, können Sie Daten an den Server senden. Verwenden Sie hierzu die [**MessageWebSocket.OutputStream**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket.OutputStream)-Eigenschaft und einen [**DataWriter**](/uwp/api/windows.storage.streams.datawriter), um die Daten zu schreiben. 

**Hinweis** Der **DataWriter** übernimmt den Besitz am Ausgabestream. Wenn der **DataWriter** den gültigen Bereich verlässt, gibt der **DataWriter** den Ausgabestream frei, wenn dieser an ihn angefügt ist. Danach schlagen alle nachfolgenden Versuche, den Ausgabestream zu verwenden, mit einem HRESULT-Wert 0x80000013 fehl. Sie können jedoch [**DataWriter.DetachStream**](/uwp/api/windows.storage.streams.datawriter.DetachStream) aufrufen, um den Ausgabestream vom **DataWriter** zu trennen und den Besitz am Stream dem **MessageWebSocket** zurückzugeben.

## <a name="use-streamwebsocket-to-connect"></a>Verwenden von StreamWebSocket zum Herstellen einer Verbindung
[**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket) ermöglicht, dass bei jedem Lesevorgang Abschnitte einer Nachricht gelesen werden. Folglich ist es geeignet, wenn sehr große Dateien (z. B. Fotos oder Videos) übertragen werden. Die Klasse unterstützt nur binäre Nachrichten.

Der nachstehende Beispielcode verwendet den WebSocket.org-Echoserver – einen Dienst, der Nachrichten zurück an den Absender sendet.

```csharp
private Windows.Networking.Sockets.StreamWebSocket streamWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.streamWebSocket = new Windows.Networking.Sockets.StreamWebSocket();

    this.streamWebSocket.Closed += WebSocket_Closed;

    try
    {
        Task connectTask = this.streamWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask();

        connectTask.ContinueWith(_ =>
        {
            Task.Run(() => this.ReceiveMessageUsingStreamWebSocket());
            Task.Run(() => this.SendMessageUsingStreamWebSocket(new byte[] { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 }));
        });
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private async void ReceiveMessageUsingStreamWebSocket()
{
    try
    {
        using (var dataReader = new DataReader(this.streamWebSocket.InputStream))
        {
            dataReader.InputStreamOptions = InputStreamOptions.Partial;
            await dataReader.LoadAsync(256);
            byte[] message = new byte[dataReader.UnconsumedBufferLength];
            dataReader.ReadBytes(message);
            Debug.WriteLine("Data received from StreamWebSocket: " + message.Length + " bytes");
        }
        this.streamWebSocket.Dispose();
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private async void SendMessageUsingStreamWebSocket(byte[] message)
{
    try
    {
        using (var dataWriter = new DataWriter(this.streamWebSocket.OutputStream))
        {
            dataWriter.WriteBytes(message);
            await dataWriter.StoreAsync();
            dataWriter.DetachStream();
        }
        Debug.WriteLine("Sending data using StreamWebSocket: " + message.Length.ToString() + " bytes");
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private void WebSocket_Closed(Windows.Networking.Sockets.IWebSocket sender, Windows.Networking.Sockets.WebSocketClosedEventArgs args)
{
    Debug.WriteLine("WebSocket_Closed; Code: " + args.Code + ", Reason: \"" + args.Reason + "\"");
    // Add additional code here to handle the WebSocket being closed.
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamWebSocket m_streamWebSocket;
    winrt::event_token m_closedEventToken;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        m_closedEventToken = m_streamWebSocket.Closed({ this, &StreamWebSocketPage::OnWebSocketClosed });

        try
        {
            co_await m_streamWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
            ReceiveMessageUsingStreamWebSocket();
            SendMessageUsingStreamWebSocket({ 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 });
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

private:
    IAsyncAction SendMessageUsingStreamWebSocket(std::vector< byte > message)
    {
        try
        {
            DataWriter dataWriter{ m_streamWebSocket.OutputStream() };
            dataWriter.WriteBytes(message);

            co_await dataWriter.StoreAsync();
            dataWriter.DetachStream();
            std::wstringstream wstringstream;
            wstringstream << L"Sending data using StreamWebSocket: " << message.size() << L" bytes" << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    IAsyncAction ReceiveMessageUsingStreamWebSocket()
    {
        try
        {
            DataReader dataReader{ m_streamWebSocket.InputStream() };
            dataReader.InputStreamOptions(InputStreamOptions::Partial);

            unsigned int bytesLoaded = co_await dataReader.LoadAsync(256);
            std::vector< byte > message(bytesLoaded);
            dataReader.ReadBytes(message);
            std::wstringstream wstringstream;
            wstringstream << L"Data received from StreamWebSocket: " << message.size() << " bytes" << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            m_streamWebSocket.Close(1000, L"");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    void OnWebSocketClosed(Windows::Networking::Sockets::IWebSocket const&, Windows::Networking::Sockets::WebSocketClosedEventArgs const& args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args.Code() << ", Reason: \"" << args.Reason().c_str() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamWebSocket^ streamWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->streamWebSocket = ref new Windows::Networking::Sockets::StreamWebSocket();

        this->streamWebSocket->Closed += ref new TypedEventHandler<Windows::Networking::Sockets::IWebSocket^, Windows::Networking::Sockets::WebSocketClosedEventArgs^>(this, &StreamWebSocketPage::WebSocket_Closed);

        try
        {
            auto connectTask = Concurrency::create_task(this->streamWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));

            connectTask.then(
                [=]
            {
                this->ReceiveMessageUsingStreamWebSocket();
                this->SendMessageUsingStreamWebSocket(ref new Platform::Array< byte >{ 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

private:
    void SendMessageUsingStreamWebSocket(const Platform::Array< byte >^ message)
    {
        try
        {
            auto dataWriter = ref new DataWriter(this->streamWebSocket->OutputStream);
            dataWriter->WriteBytes(message);

            Concurrency::create_task(dataWriter->StoreAsync()).then(
                [=](Concurrency::task< unsigned int >) // task< unsigned int > instead of unsigned int in order to handle any exceptions thrown in StoreAsync().
            {
                dataWriter->DetachStream();
                std::wstringstream wstringstream;
                wstringstream << L"Sending data using StreamWebSocket: " << message->Length << L" bytes" << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void ReceiveMessageUsingStreamWebSocket()
    {
        try
        {
            DataReader^ dataReader = ref new DataReader(this->streamWebSocket->InputStream);
            dataReader->InputStreamOptions = InputStreamOptions::Partial;

            Concurrency::create_task(dataReader->LoadAsync(256)).then(
                [=](unsigned int bytesLoaded)
            {
                auto message = ref new Platform::Array< byte >(bytesLoaded);
                dataReader->ReadBytes(message);
                std::wstringstream wstringstream;
                wstringstream << L"Data received from StreamWebSocket: " << message->Length << " bytes" << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
                this->streamWebSocket->Close(1000, L"");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void WebSocket_Closed(Windows::Networking::Sockets::IWebSocket^ sender, Windows::Networking::Sockets::WebSocketClosedEventArgs^ args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args->Code << ", Reason: \"" << args->Reason->Data() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

### <a name="handle-the-streamwebsocketclosed-event"></a>Behandeln des „StreamWebSocket.Closed“-Ereignisses
Vor dem Herstellen einer Verbindung und dem Senden von Daten mit einem **StreamWebSocket** sollten Sie das [**StreamWebSocket.Closed**](/uwp/api/windows.networking.sockets.streamwebsocket.Closed)-Ereignis abonnieren. **Closed** wird ausgelöst, wenn der Client oder Server den Socket schließt.
 
### <a name="send-data-on-a-streamwebsocket"></a>Senden von Daten an einem StreamWebSocket
Sobald eine Verbindung hergestellt ist, können Sie Daten an den Server senden. Verwenden Sie hierzu die [**StreamWebSocket.OutputStream**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket.OutputStream)-Eigenschaft und einen [**DataWriter**](/uwp/api/windows.storage.streams.datawriter), um die Daten zu schreiben.

**Hinweis** Wenn Sie mehr Daten auf demselben Socket schreiben möchten, müssen Sie [**DataWriter.DetachStream**](/uwp/api/windows.storage.streams.datawriter.DetachStream) aufrufen, um den Ausgabestream vom **DataWriter** zu trennen, bevor der **DataWriter** den gültigen Bereich verlässt. Dadurch wird der Besitz am Datenstrom an den **MessageWebSocket** zurückgegeben.

### <a name="receive-data-on-a-streamwebsocket"></a>Empfangen von Daten an einem StreamWebSocket
Verwenden Sie die [**StreamWebSocket.InputStream**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket.InputStream)-Eigenschaft und einen [**DataReader**](/uwp/api/windows.storage.streams.datareader), um die Daten zu lesen.

## <a name="advanced-options-for-messagewebsocket-and-streamwebsocket"></a>Erweiterte Optionen für „MessageWebSocket“ und „StreamWebSocket“
Vor dem Herstellen einer Verbindung können Sie erweiterte Optionen für einen Socket festlegen, indem Sie die Eigenschaften für entweder [**MessageWebSocketControl**](/uwp/api/windows.networking.sockets.messagewebsocketcontrol) oder [**StreamWebSocketControl**](/uwp/api/windows.networking.sockets.streamwebsocketcontrol) festlegen. Sie greifen auf eine Instanz dieser Klassen aus dem Socketobjekt selbst entweder über dessen [**MessageWebSocket.Control**](/uwp/api/windows.networking.sockets.messagewebsocket.control)- oder [**StreamWebSocket.Control**](/uwp/api/windows.networking.sockets.streamwebsocket.control)-Eigenschaft zu, je nach Bedarf.

Hier sehen Sie ein Beispiel zur Verwendung von **StreamWebSocket**. Das gleiche Muster gilt für **MessageWebSocket**.

```csharp
var streamWebSocket = new Windows.Networking.Sockets.StreamWebSocket();

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket.Control.NoDelay = false;

await streamWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org"));
```

```cppwinrt
Windows::Networking::Sockets::StreamWebSocket streamWebSocket;

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket.Control().NoDelay(false);

auto connectAsyncAction = streamWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
```

```cpp
auto streamWebSocket = ref new Windows::Networking::Sockets::StreamWebSocket();

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket->Control->NoDelay = false;

auto connectTask = Concurrency::create_task(streamWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));
```

**Hinweis** Versuchen Sie nicht, eine Steuerelementeigenschaft zu ändern, *nachdem* Sie **ConnectAsync** aufgerufen haben. Die einzige Ausnahme von dieser Regel ist [MessageWebSocketControl.MessageType](/uwp/api/windows.networking.sockets.messagewebsocketcontrol.MessageType).

## <a name="websocket-information-classes"></a>WebSocket-Informationsklassen
[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) und [**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket) besitzen jeweils eine entsprechende Klasse, die zusätzliche Informationen über das Objekt bereitstellt.

[**MessageWebSocketInformation**](/uwp/api/windows.networking.sockets.messagewebsocketinformation) enthält Informationen zu einem **MessageWebSocket**, und Sie rufen eine seiner Instanzen mit der [**MessageWebSocket.Information**](/uwp/api/windows.networking.sockets.messagewebsocket.Information)-Eigenschaft ab.

[**StreamWebSocketInformation**](/uwp/api/Windows.Networking.Sockets.StreamWebSocketInformation) enthält Informationen zu einem **StreamWebSocket**, und Sie rufen eine seiner Instanzen mit der [**StreamWebSocket.Information**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket.Information)-Eigenschaft ab.

Beachten Sie, dass die Eigenschaften für diese Informationsklassen schreibgeschützt sind, Sie können Sie jedoch verwenden, um Informationen zu einem beliebigen Zeitpunkt während der Lebensdauer eines Websocket-Objekts abzurufen.

## <a name="handling-exceptions"></a>Behandeln von Ausnahmen
Ein Fehler in einem [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket)-Vorgang oder [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket)-Vorgang wird als **HRESULT**-Wert zurückgegeben. Sie können diesen **HRESULT**-Wert der [**WebSocketError.GetStatus**](/uwp/api/windows.networking.sockets.websocketerror.getstatus)-Methode übergeben, um ihn in einen [**WebErrorStatus**](/uwp/api/Windows.Web.WebErrorStatus)-Enumerationswert zu konvertieren.

Die meisten **WebErrorStatus**-Enumerationswerte entsprechen einem vom systemeigenen HTTP-Clientvorgang zurückgegebenen Fehler. Ihre App kann **WebErrorStatus**-Enumerationswerte einschalten, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Bei Parameterprüfungsfehlern können Sie den **HRESULT**-Wert aus der Ausnahme verwenden, um ausführlichere Informationen zum Fehler zu erhalten. Mögliche **HRESULT**-Werte sind in `Winerror.h` aufgelistet; dies finden Sie in Ihrer SDK-Installation (z. B. im Ordner `C:\Program Files (x86)\Windows Kits\10\Include\<VERSION>\shared`). Für die meisten Parameterüberprüfungsfehler wird der **HRESULT**-Wert **E_INVALIDARG** zurückgegeben.

## <a name="setting-timeouts-on-websocket-operations"></a>Festlegen von Timeouts für WebSocket-Vorgänge
**MessageWebSocket** und **StreamWebSocket** verwenden einen internen Systemdienst, um WebSocket-Clientanforderungen zu senden und Antworten von einem Server zu empfangen. Der standardmäßige Timeoutwert, der für einen WebSocket-Verbindungsvorgang verwendet wird, beträgt 60 Sekunden. Wenn der HTTP-Server, der WebSockets unterstützt, nicht auf die WebSocket-Verbindungsanforderung antwortet oder antworten kann (er ist vorübergehend nicht verfügbar oder durch einen Netzwerkausfall blockiert), wartet der interne Systemdienst die standardmäßig festgelegten 60 Sekunden ab, bevor ein Fehler zurückgegeben wird. Dieser Fehler führt dazu, dass eine Ausnahme in der WebSocket **ConnectAsync**-Methode ausgelöst wird. Das Standardtimeout, das für Sende- und Empfangsvorgänge nach dem Herstellen einer WebSocket-Verbindung verwendet wird, beträgt 30 Sekunden.

Falls die Namensabfrage für einen HTTP-Servernamen im URI mehrere IP-Adressen für den Namen zurückgibt, testet der interne Systemdienst bis zu fünf IP-Adressen für die Website. Dabei wird jeweils das Standardtimeout von 60 Sekunden eingehalten, bevor ein Fehler auftritt. Folglich ist es möglich, dass Ihre App mehrere Minuten lang versucht, sich mit mehreren IP-Adressen zu verbinden, bevor eine Ausnahme behandelt wird. Dieses Verhalten könnte für Benutzer den Anschein erwecken, als ob die App nicht mehr reagiert. 

Damit Ihre App besser reagiert und diese Probleme minimiert werden, können Sie für Verbindungsanforderungen ein kürzeres Timeout festlegen. Ein Timeout wird für **MessageWebSocket** und **StreamWebSocket** auf ähnliche Weise festgelegt.

```csharp
private Windows.Networking.Sockets.MessageWebSocket messageWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.messageWebSocket = new Windows.Networking.Sockets.MessageWebSocket();

    try
    {
        var cancellationTokenSource = new CancellationTokenSource();
        var connectTask = this.messageWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask(cancellationTokenSource.Token);

        // Cancel connectTask after 5 seconds.
        cancellationTokenSource.CancelAfter(TimeSpan.FromMilliseconds(5000));

        connectTask.ContinueWith((antecedent) =>
        {
            if (antecedent.Status == TaskStatus.RanToCompletion)
            {
                // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                // Add additional code here to use the MessageWebSocket.
            }
            else
            {
                // connectTask timed out, or faulted.
            }
        });
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket m_messageWebSocket;

    IAsyncAction TimeoutAsync()
    {
        // Return control to the caller, and resume again to complete the async action after the timeout period.
        // 5 seconds, in this example.
        co_await(std::chrono::seconds{ 5 });
    }

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        try
        {
            // Return control to the caller, and then immediately resume on a thread pool thread.
            co_await winrt::resume_background();

            auto connectAsyncAction = m_messageWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });

            TimeoutAsync().Completed([connectAsyncAction](IAsyncAction const& sender, AsyncStatus const)
            {
                // TimeoutAsync completes after the timeout period. After that period, it's safe
                // to cancel the ConnectAsync action even if it has already completed.
                connectAsyncAction.Cancel();
            });

            try
            {
                // Block until the ConnectAsync action completes or is canceled.
                connectAsyncAction.get();
            }
            catch (winrt::hresult_error const& ex)
            {
                std::wstringstream wstringstream;
                wstringstream << L"ConnectAsync threw an exception: " << ex.message().c_str() << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
            }

            if (connectAsyncAction.Status() == AsyncStatus::Completed)
            {
                // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                // Add additional code here to use the MessageWebSocket.
            }
            else
            {
                // connectTask did not run to completion.
            }
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }
```

```cpp
#include <agents.h>
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket^ messageWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->messageWebSocket = ref new Windows::Networking::Sockets::MessageWebSocket();

        try
        {
            Concurrency::cancellation_token_source cancellationTokenSource;
            Concurrency::cancellation_token cancellationToken = cancellationTokenSource.get_token();

            auto connectTask = Concurrency::create_task(this->messageWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")), cancellationToken);

            // This continuation task returns true should connectTask run to completion.
            Concurrency::task< bool > taskRanToCompletion = connectTask.then([](void)
            {
                return true;
            });

            // This task returns false after the specified timeout. 5 seconds, in this example.
            Concurrency::task< bool > taskTimedout = Concurrency::create_task([]() -> bool
            {
                Concurrency::task_completion_event< void > taskCompletionEvent;

                // A call object that sets the task completion event.
                auto call = std::make_shared< Concurrency::call< int > >([taskCompletionEvent](int)
                {
                    taskCompletionEvent.set();
                });

                // A non-repeating timer that calls the call object when the timer fires.
                auto nonRepeatingTimer = std::make_shared< Concurrency::timer < int > >(5000, 0, call.get(), false);
                nonRepeatingTimer->start();

                // A task that completes after the completion event is set.
                Concurrency::task< void > taskWaitForCompletionEvent(taskCompletionEvent);

                return taskWaitForCompletionEvent.then([]() {return false; }).get();
            });

            (taskRanToCompletion || taskTimedout).then([this, cancellationTokenSource](bool connectTaskRanToCompletion)
            {
                if (connectTaskRanToCompletion)
                {
                    // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                    // Add additional code here to use the MessageWebSocket.
                }
                else
                {
                    // taskTimedout ran to completion, so we should cancel connectTask via the cancellation_token_source.
                    cancellationTokenSource.cancel();
                }
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }
```

## <a name="important-apis"></a>Wichtige APIs
* [DataReader](/uwp/api/Windows.Storage.Streams.DataReader)
* [DataWriter](/uwp/api/Windows.Storage.Streams.DataWriter)
* [DataWriter.DetachStream](/uwp/api/windows.storage.streams.datawriter.DetachStream)
* [MessageWebSocket](/uwp/api/windows.networking.sockets.messagewebsocket)
* [MessageWebSocket.Closed](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.Closed)
* [MessageWebSocket.ConnectAsync](/uwp/api/windows.networking.sockets.messagewebsocket.connectasync)
* [MessageWebSocket.Control](/uwp/api/windows.networking.sockets.messagewebsocket.control)
* [MessageWebSocket.Information](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.Information)
* [MessageWebSocket.MessageReceived](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.MessageReceived)
* [MessageWebSocket.OutputStream](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket.OutputStream)
* [MessageWebSocketControl](/uwp/api/Windows.Networking.Sockets.MessageWebSocketControl)
* [MessageWebSocketControl.MessageType](/uwp/api/Windows.Networking.Sockets.MessageWebSocketControl.MessageType)
* [MessageWebSocketInformation](/uwp/api/Windows.Networking.Sockets.MessageWebSocketInformation)
* [MessageWebSocketMessageReceivedEventArgs](/uwp/api/Windows.Networking.Sockets.MessageWebSocketMessageReceivedEventArgs)
* [SocketMessageType](/uwp/api/windows.networking.sockets.socketmessagetype)
* [StreamWebSocket](/uwp/api/Windows.Networking.Sockets.StreamWebSocket)
* [StreamWebSocket.Closed](/uwp/api/Windows.Networking.Sockets.StreamWebSocket.Closed)
* [StreamSocket.ConnectAsync](/uwp/api/windows.networking.sockets.streamsocket.connectasync)
* [StreamWebSocket.Control](/uwp/api/windows.networking.sockets.streamwebsocket.control)
* [StreamWebSocket.Information](/uwp/api/windows.networking.sockets.streamwebsocket.Information)
* [StreamWebSocket.InputStream](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket.InputStream)
* [StreamWebSocket.OutputStream](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket.OutputStream)
* [StreamWebSocketControl](/uwp/api/Windows.Networking.Sockets.StreamWebSocketControl)
* [StreamWebSocketInformation](/uwp/api/Windows.Networking.Sockets.StreamWebSocketInformation)
* [WebErrorStatus](/uwp/api/Windows.Web.WebErrorStatus) 
* [WebSocketError.GetStatus](/uwp/api/windows.networking.sockets.websocketerror.getstatus)
* [Windows.Networking.Sockets](/uwp/api/Windows.Networking.Sockets)

## <a name="related-topics"></a>Verwandte Themen
* [WebSocket-Protokoll](https://tools.ietf.org/html/rfc6455)
* [Sockets](sockets.md)

## <a name="samples"></a>Beispiele
* [WebSocket-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebSocket)
