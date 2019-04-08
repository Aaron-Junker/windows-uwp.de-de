---
description: Aktionen, die Sie für eine netzwerkfähige App ausführen müssen.
title: Netzwerkgrundlagen
ms.assetid: 1F47D33B-6F00-4F74-A52D-538851FD38BE
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ffc2f31b52e7913905c7d64ab797b2939cfb313d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651655"
---
# <a name="networking-basics"></a>Netzwerkgrundlagen
Aktionen, die Sie für eine netzwerkfähige App ausführen müssen.

## <a name="capabilities"></a>Funktionen
Um das Netzwerk verwenden zu können, müssen Sie die entsprechenden Funktionselemente zum App-Manifest hinzufügen. Wenn in Ihrem App-Manifest keine Netzwerkfunktion angegeben ist, besitzt Ihre App keine Netzwerkfunktion, und jeder Versuch, eine Verbindung mit dem Netzwerk herzustellen, führt zu einem Fehler.

Im Folgenden finden Sie die am häufigsten verwendeten Netzwerkfunktionen:

| Funktion | Beschreibung |
|------------|-------------|
| **internetClient** | Bietet ausgehenden Zugriff auf das Internet und Netzwerke an öffentlichen Orten wie Flughäfen und Cafés. Die meisten Apps, die Internetzugriff erfordern, sollten diese Funktion verwenden. |
| **internetClientServer** | Bietet der App ein- und ausgehenden Netzwerkzugriff aus dem Internet und aus Netzwerken an öffentlichen Orten wie Flughäfen und Cafés. |
| **privateNetworkClientServer** | Bietet der App eingehenden und ausgehenden Netzwerkzugriff an vertrauenswürdigen Orten des Benutzers (z. B. zu Hause und am Arbeitsplatz). |

In bestimmten Situationen sind möglicherweise weitere Funktionen für Ihre App erforderlich.

| Funktion | Beschreibung |
|------------|-------------|
| **EnterpriseAuthentication** | Ermöglicht einer App, eine Verbindung mit Netzwerkressourcen herzustellen, für die Domänenanmeldeinformationen nötig sind. Bei dieser Funktion muss ein Domänenadministrator die Funktionalität für alle Apps aktivieren. Beispielsweise kann es sich um eine App handeln, die Daten von SharePoint-Servern in einem privaten Intranet abruft. <br/> Mit dieser Funktion können Ihre Anmeldeinformationen für den Zugriff auf Netzwerkressourcen in einem Netzwerk verwendet werden, für das Anmeldeinformationen erforderlich sind. Eine App mit dieser Funktion kann Ihre Identität im Netzwerk annehmen. <br/> Für den Internetzugriff einer App mithilfe eines Authentifizierungsproxys ist diese Funktion nicht erforderlich. |
| **NEAR** | Sie ist für die Nahfeldnäherungskommunikation mit Geräten in geringem Abstand zum Computer erforderlich. Die Nahfeldnäherung kann zum Senden an eine Anwendung oder Verbinden mit einer Anwendung auf einem in der Nähe befindlichen Gerät verwendet werden. <br/> Mit dieser Funktion kann eine App auf das Netzwerk zugreifen, um eine Verbindung mit einem Gerät in geringem Abstand herzustellen und mit der Zustimmen des Benutzers eine Einladung zu senden oder anzunehmen. |
| **Sharedusercertifcates** | Ermöglicht einer App den Zugriff auf Software- und Hardwarezertifikate wie etwa Smartcardzertifikate. Wenn diese Funktion zur Laufzeit aufgerufen wird, muss der Benutzer eine Aktion ausführen (z. B. eine Karte einsetzen oder ein Zertifikat auswählen). <br/> Bei dieser Funktion werden Ihre Software- und Hardwarezertifikate oder eine Smartcard zur Identifikation in der Anwendung verwendet. Diese Funktion kann von Ihrem Arbeitgeber, Ihrer Bank oder Regierungsbehörden zur Identifikation verwendet werden. |

## <a name="communicating-when-your-app-is-not-in-the-foreground"></a>Kommunikation, wenn Ihre App nicht im Vordergrund ausgeführt wird
[Unterstützen Ihrer App mit Hintergrundaufgaben](https://msdn.microsoft.com/library/windows/apps/mt299103) enthält allgemeine Informationen zur Verwendung von Hintergrundaufgaben, um Aufgaben auszuführen, während sich Ihre App nicht im Vordergrund befindet. Genauer gesagt muss Ihr Code besondere Schritte vornehmen, damit eine Benachrichtigung erfolgt, wenn es sich dabei nicht um die aktuelle App im Vordergrund handelt und diese Daten über das Netzwerk empfängt. Sie Kanal-Trigger-Steuerelement in Windows 8 zu diesem Zweck verwendet, und sie werden weiterhin in Windows 10 unterstützt. Vollständige Informationen zur Verwendung der Steuerkanalauslöser finden Sie [**hier**](https://msdn.microsoft.com/library/windows/apps/hh701032). Eine neue Technologie in Windows 10 bietet eine bessere Funktionen mit weniger Aufwand für einige Szenarien, z. B. Push-fähigen Streamsockets: der Socket-Broker und der Socket-aktivitätstrigger.

Wenn die App [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) oder [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) verwendet, kann sie die Beteiligung eines offenen Sockets an einen vom System bereitgestellten Socketbroker übertragen und sie dann im Vordergrund belassen oder sogar beenden. Wenn eine Verbindung zum übertragenen Socket hergestellt oder Datenverkehr auf diesem Socket empfangen wird, wird Ihre App oder die festgelegten Hintergrundaufgabe aktiviert. Wenn Ihre App nicht ausgeführt wird, wird sie gestartet. Der Socketbroker benachrichtigt Ihre App mittels [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) darüber, dass neuer Datenverkehr eingegangen ist. Ihre App gibt den Socket aus dem Socketbroker frei und verarbeitet den Datenverkehr auf dem Socket. Das heißt, Ihre App beansprucht deutlich weniger Systemressourcen, wenn es aktiv keinen Netzwerkdatenverkehr verarbeitet.

Der Socketbroker soll den Steuerkanal-Auslöser ersetzen, in dem er anwendbar ist, da er die gleiche Funktionalität bietet, jedoch weniger Einschränkungen besitzt und weniger Speicherbedarf benötigt. Der Socketbroker kann von Sperrbildschirm-Apps verwendet werden und wird auf Telefonen und anderen Geräten auf die gleiche Weise verwendet. Apps müssen nicht ausgeführt werden, wenn Datenverkehr eingeht, damit sie vom Socketbroker aktiviert werden. Der Socketbroker unterstützt darüber hinaus die Erkennung auf TCP-Sockets. Dies ist bei Steuerkanalauslösern nicht der Fall.

### <a name="choosing-a-network-trigger"></a>Wählen eines Netzwerkauslösers
Es gibt einige Szenarien, in denen beide Auslöserarten geeignet sind. Beachten Sie bei der Auswahl der Auslöserart der App Folgendes:

-   Bei Verwendung von [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), [**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) oder [System.Net.Http.HttpClientHandler](https://go.microsoft.com/fwlink/p/?linkid=241638) müssen Sie [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) verwenden.
-   Bei Verwendung pushfähiger **StreamSockets** können Sie Steuerkanaltrigger verwenden, [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) sind jedoch vorzuziehen. In letzterem Fall kann das System Arbeitsspeicher freigeben und den Stromverbrauch verringern, wenn die Verbindung nicht aktiv verwendet wird.
-   Wenn Sie den Speicherbedarf Ihrer App während der aktiven Verarbeitung von Netzwerkanforderungen minimieren möchten, sollten Sie nach Möglichkeit [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) verwenden.
-   Wenn Sie möchten, dass Ihre App Daten empfängt, während sich das System im verbundenen Standbymodus befindet, verwenden Sie [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009).

Ausführliche Informationen und Beispiele zur Verwendung des Socketbrokers finden Sie unter [Netzwerkkommunikation im Hintergrund](network-communications-in-the-background.md).

## <a name="secured-connections"></a>Sichere Verbindungen
Secure Sockets Layer (SSL) und das aktuellere Transport Layer Security (TLS) sind Verschlüsselungsprotokolle für die Authentifizierung und Verschlüsselung der Kommunikation in Netzwerken. Diese Protokolle dienen dazu, das Mitverfolgen und Manipulieren von Daten zu verhindern, die im Netzwerk gesendet und empfangen werden. Für den Protokollaustausch wird dabei ein Client-Server-Modell verwendet. Diese Protokolle verwenden zudem digitale Zertifikate und Zertifizierungsstellen, um den jeweiligen Server zu identifizieren.

### <a name="creating-secure-socket-connections"></a>Erstellen von sicheren Socketverbindungen
Ein [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Objekt kann für die Verwendung von SSL/TLS für die Kommunikation zwischen dem Client und dem Server konfiguriert werden. Diese Unterstützung für SSL/TLS beschränkt sich auf die Verwendung des **StreamSocket**-Objekts als Client in der SSL/TLS-Aushandlung. Sie können SSL/TLS nicht mit dem von einer [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)-Klasse erstellten **StreamSocket**-Objekt verwenden, wenn eingehende Kommunikation empfangen wird, da SSL/TLS-Aushandlung als Server nicht von der **StreamSocket**-Klasse implementiert wird.

Es gibt zwei Möglichkeiten, eine [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Verbindung mit SSL/TLS zu sichern:

-   [**ConnectAsync** ](https://msdn.microsoft.com/library/windows/apps/hh701504) : Stellen Sie die erste Verbindung mit einem Netzwerkdienst und sofort für die gesamte Kommunikation mit SSL/TLS-Aushandlung.
-   [**UpgradeToSslAsync** ](https://msdn.microsoft.com/library/windows/apps/br226922) -verbinden Sie sich zunächst mit einem Netzwerkdienst ohne Verschlüsselung. Die App kann Daten senden oder empfangen. Dann wird die Verbindung für die Verwendung von SSL/TLS für jede weitere Verbindung hochgestuft.

SocketProtectionLevel gibt die gewünschte Socket-Schutzebene an, mit der die App eingerichtet oder die Verbindung aktualisiert werden soll. Die letztendliche Schutzebene der hergestellten Verbindung wird jedoch im Aushandlungsprozess zwischen beiden Endpunkten der Verbindung festgelegt. Dies kann zu einer niedrigeren Schutzebene führen, die sichererer als die von Ihnen angegebene Ebene ist, wenn der andere Endpunkt eine niedrigere Ebene erfordert. 

 Nachdem der asynchrone Vorgang erfolgreich abgeschlossen wurde, können Sie die erforderliche Schutzebene abrufen, die im Aufruf von [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) oder [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) über die [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868)-Eigenschaften verwendet werden. Dies entspricht jedoch nicht der tatsächlichen Schutzebene, die in der Verbindung verwendet wird.

> [!NOTE]
> Ihr Code sollte nicht implizit von der Verwendung einer bestimmten Schutzebene oder der Annahme abhängig sein, dass standardmäßig eine bestimmte Sicherheitsstufe verwendet wird. Die Sicherheitslandschaft befindet sich in einem steten Wandel, und Protokolle und Standardschutzebenen ändern sich mit der Zeit, um die Verwendung von Protokollen mit bekannten Schwachstellen zu vermeiden. Die Standardwerte können je nach Konfiguration einzelner Computer oder abhängig von der installierten Software oder angewendeten Patches variieren. Wenn Ihre App von der Verwendung einer bestimmten Sicherheitsstufe abhängt, müssen Sie diese Stufe explizit angeben und dann durch eine Überprüfung sicherstellen, dass sie tatsächlich für die hergestellte Verbindung verwendet wird.

### <a name="use-connectasync"></a>Verwenden von ConnectAsync
[**ConnectAsync** ](https://msdn.microsoft.com/library/windows/apps/hh701504) dienen zum Herstellen der anfangsverbindung mit einem Netzwerkdienst, und klicken Sie dann sofort für die gesamte Kommunikation mit SSL/TLS-Aushandlung. Es gibt zwei **ConnectAsync**-Methoden, die das Übergeben eines *protectionLevel*-Parameters unterstützen:

-   [**ConnectAsync (EndpointPair, SocketProtectionLevel)** ](https://msdn.microsoft.com/library/windows/apps/hh701511) -startet einen asynchronen Vorgang auf einem [ **StreamSocket** ](https://msdn.microsoft.com/library/windows/apps/br226882) Objekt, das mit einem Remotenetzwerk-Ziel verbinden angegebene als ein [ **EndpointPair** ](https://msdn.microsoft.com/library/windows/apps/hh700953) Objekt und ein [ **SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880).
-   [**ConnectAsync (HostName, String, SocketProtectionLevel)** ](https://msdn.microsoft.com/library/windows/apps/br226916) -startet einen asynchronen Vorgang auf einem [ **StreamSocket** ](https://msdn.microsoft.com/library/windows/apps/br226882) Objekt, das mit einem remote-Ziel verbinden durch einen remote-Hostnamen, ein remotedienstname angegeben und ein [ **SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880).

Wenn der *protectionLevel*-Parameter beim Aufruf einer der obigen [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504)-Methoden auf **Windows.Networking.Sockets.SocketProtectionLevel.Ssl** festgelegt ist, muss mit [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) die Verwendung von SSL/TLS für die Verschlüsselung sichergestellt werden. Dieser Wert erfordert eine Verschlüsselung, wobei keine NULL-Verschlüsselung zulässig ist.

Der übliche Ablauf ist bei beiden [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504)-Methoden gleich.

-   Erstellen Sie ein [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Objekt.
-   Wenn eine erweiterte Option für den Socket erforderlich ist, rufen Sie mit der [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917)-Eigenschaft die [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893)-Instanz ab, die einem [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Objekt zugeordnet ist. Legen Sie eine Eigenschaft für **StreamSocketControl** fest.
-   Rufen Sie eine der obigen [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504)-Methoden auf, um einen Vorgang zu starten, bei dem eine Verbindung mit einem Remoteziel hergestellt und sofort mit der Aushandlung von SSL/TLS begonnen wird.
-   Die tatsächlich mithilfe von [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) ausgehandelte SSL-Schlüsselstärke kann durch Abruf der [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868)-Eigenschaft bestimmt werden, nachdem der asynchrone Vorgang erfolgreich abgeschlossen wurde.

Das folgende Beispiel erstellt ein [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Objekt und versucht, eine Verbindung mit dem Netzwerkdienst herzustellen und sofort die Verwendung von SSL/TLS auszuhandeln. Bei einer erfolgreichen Aushandlung wird jede Netzwerkkommunikation zwischen dem Client und dem Netzwerkserver verschlüsselt, bei der das **StreamSocket**-Objekt verwendet wird.

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
     
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "https";
    
    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 
    
    // Try to connect to contoso using HTTPS (port 443)
    try {

        // Call ConnectAsync method with SSL
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.Ssl);

        NotifyUser("Connected");
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }
        
        NotifyUser("Connect failed with error: " + exception.Message);
        // Could retry the connection, but for this simple example
        // just close the socket.
        
        clientSocket.Dispose();
        clientSocket = null; 
    }
           
    // Add code to send and receive data using the clientSocket
    // and then close the clientSocket
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>

using namespace winrt;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages.

    // Try to connect to the server using HTTPS and SSL (port 443).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::Tls12);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }
    // Add code to send and receive data using the clientSocket,
    // then set the clientSocket to nullptr when done to close it.
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    HostName^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "https";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to the server using HTTPS and SSL (port 443)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::SSL)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
            
            clientSocket.Close();
            clientSocket = null;
        }
    });
    // Add code to send and receive data using the clientSocket
    // Then close the clientSocket when done
```

### <a name="use-upgradetosslasync"></a>Verwenden von UpgradeToSslAsync
Wenn der Code [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) verwendet, stellt er zuerst eine Verbindung mit einem Netzwerkdienst ohne Verschlüsselung her. Die App kann ein gewisse Datenmenge senden oder empfangen, bevor sie die Verbindung für jegliche weitere Kommunikation auf die Verwendung von SSL/TLS heraufgestuft.

Die [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922)-Methode besitzt zwei Parameter. Der *protectionLevel*-Parameter gibt den gewünschten Schutzgrad an. Der *validationHostName*-Parameter ist der Hostname des Remotenetzwerkziels, der bei der Höherstufung auf SSL für die Validierung verwendet wird. In der Regel entspricht *validationHostName* dem Hostnamen, mit dem die App die erste Verbindung hergestellt hat. Wenn der *protectionLevel*-Parameter beim Aufrufen von **UpgradeToSslAsync** auf **Windows.System.Socket.SocketProtectionLevel.Ssl** festgelegt ist, muss [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) für die weitere Kommunikation über den Socket die SSL/TLS-Verschlüsselung verwenden. Dieser Wert erfordert eine Verschlüsselung, wobei keine NULL-Verschlüsselung zulässig ist.

Der übliche Ablauf bei dieser [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922)-Methode lautet wie folgt:

-   Erstellen Sie ein [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Objekt.
-   Wenn eine erweiterte Option für den Socket erforderlich ist, rufen Sie mit der [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917)-Eigenschaft die [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893)-Instanz ab, die einem [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Objekt zugeordnet ist. Legen Sie eine Eigenschaft für **StreamSocketControl** fest.
-   Senden Sie jetzt alle Daten, die ggf. unverschlüsselt gesendet oder empfangen werden müssen.
-   Rufen Sie die [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922)-Methode auf, um einen Vorgang zum Höherstufen der Verbindung auf SSL/TLS zu starten.
-   Die tatsächlich mithilfe von [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) ausgehandelte SSL-Schlüsselstärke kann durch Abruf der [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868)-Eigenschaft bestimmt werden, nachdem der asynchrone Vorgang erfolgreich abgeschlossen wurde.

Das folgende Beispiel erstellt ein [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Objekt, versucht, eine Verbindung mit dem Netzwerkdienst herzustellen, sendet einige erste Daten und handelt dann die Verwendung von SSL/TLS aus. Bei einer erfolgreichen Aushandlung wird jede Netzwerkkommunikation zwischen dem Client und dem Netzwerkserver verschlüsselt, bei der **StreamSocket** verwendet wird.

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
 
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    try {
        // Call ConnectAsync method with a plain socket
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.PlainSocket);

        NotifyUser("Connected");

    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Connect failed with error: " + exception.Message, NotifyType.ErrorMessage);
        // Could retry the connection, but for this simple example
        // just close the socket.

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }

    // Now try to send some data
    DataWriter writer = new DataWriter(clientSocket.OutputStream);
    string hello = "Hello, World! ☺ ";
    Int32 len = (int) writer.MeasureString(hello); // Gets the UTF-8 string length.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser("Client: sending hello");

    try {
        // Call StoreAsync method to store the hello message
        await writer.StoreAsync();

        NotifyUser("Client: sent data");

        writer.DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
    }
    catch (Exception exception) {
        NotifyUser("Store failed with error: " + exception.Message);
        // Could retry the store, but for this simple example
            // just close the socket.

            clientSocket.Dispose();
            clientSocket = null; 
            return;
    }

    // Now upgrade the client to use SSL
    try {
        // Try to upgrade to SSL
        await clientSocket.UpgradeToSslAsync(SocketProtectionLevel.Ssl, serverHost);

        NotifyUser("Client: upgrade to SSL completed");
           
        // Add code to send and receive data 
        // The close clientSocket when done
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt;
using namespace Windows::Storage::Streams;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages. 

    // Try to connect to the server using HTTP (port 80).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }

    // Now, try to send some data.
    DataWriter writer{ clientSocket.OutputStream() };
    winrt::hstring hello{ L"Hello, World! ☺ " };
    uint32_t len{ writer.MeasureString(hello) }; // Gets the size of the string, in bytes.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser(L"Client: sending hello");

    try
    {
        co_await writer.StoreAsync();
        NotifyUser(L"Client: sent hello");

        writer.DetachStream(); // Detach the stream when you want to continue using it; otherwise, the DataWriter destructor closes it.
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Store failed with error: " + exception.message());
        // We could retry the store operation. But, for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }

    // Now, upgrade the client to use SSL.
    try
    {
        co_await clientSocket.UpgradeToSslAsync(Windows::Networking::Sockets::SocketProtectionLevel::Tls12, serverHost);
        NotifyUser(L"Client: upgrade to SSL completed");

        // Add code to send and receive data using the clientSocket,
        // then set the clientSocket to nullptr when done to close it.
    }
    catch (winrt::hresult_error const& exception)
    {
        // If this is an unknown status, then the error is fatal and retry will likely fail.
        Windows::Networking::Sockets::SocketErrorStatus socketErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(exception.to_abi()) };
        if (socketErrorStatus == Windows::Networking::Sockets::SocketErrorStatus::Unknown)
        {
            throw;
        }

        NotifyUser(L"Upgrade to SSL failed with error: " + exception.message());
        // We could retry the store operation. But for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;
using Windows::Storage::Streams;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    Hostname^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
 
            clientSocket->Close();
            clientSocket = null;
        }
    });
       
    // Now try to send some data
    DataWriter^ writer = new ref DataWriter(clientSocket.OutputStream);
    String hello = "Hello, World! ☺ ";
    Int32 len = (int) writer->MeasureString(hello); // Gets the UTF-8 string length.
    writer->writeInt32(len);
    writer->writeString(hello);
    NotifyUser("Client: sending hello");

    task<void>(writer->StoreAsync()).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

            NotifyUser("Client: sent hello");

            writer->DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
       }
       catch (Exception^ exception) {
               NotifyUser("Store failed with error: " + exception->Message);
               // Could retry the store, but for this simple example
               // just close the socket.
 
               clientSocket->Close();
               clientSocket = null;
               return
       }
    });

    // Now upgrade the client to use SSL
    task<void>(clientSocket->UpgradeToSslAsync(clientSocket.SocketProtectionLevel.Ssl, serverHost)).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

           NotifyUser("Client: upgrade to SSL completed");
           
           // Add code to send and receive data 
           // Then close clientSocket when done
        }
        catch (Exception^ exception) {
            // If this is an unknown status it means that the error is fatal and retry will likely fail.
            if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
                throw;
            }

            NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

            clientSocket->Close();
            clientSocket = null; 
            return;
        }
    });
```

### <a name="creating-secure-websocket-connections"></a>Erstellen von sicheren WebSocket-Verbindungen
WebSocket-Verbindungen können wie herkömmliche Socketverbindungen mit TLS (Transport Layer Security)/SSL (Secure Sockets Layer) verschlüsselt werden, wenn Sie die Features [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) und [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) für eine UWP-App verwenden. In den meisten Fällen empfiehlt sich die Verwendung einer sicheren WebSocket-Verbindung. Dadurch ist es wahrscheinlicher, dass die Verbindung funktioniert, da andernfalls viele Proxys unverschlüsselte WebSocket-Verbindungen ablehnen.

Beispiele für das Erstellen einer sicheren WebSocket-Verbindung mit einem Netzwerkdienst bzw. für das Schützen einer WebSocket-Verbindung mit einem Netzwerkdienst finden Sie unter [So wird’s gemacht: Schützen von WebSocket-Verbindungen mit TLS/SSL](https://msdn.microsoft.com/library/windows/apps/xaml/hh994399).

Ein Server benötigt möglicherweise zusätzlich zur TLS/SSL-Verschlüsselung einen **Sec-WebSocket-Protocol**-Headerwert, um den ersten Handshake auszuführen. Dieser Wert, der von der [**StreamWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701514)-Eigenschaft und der [**MessageWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701358)-Eigenschaft dargestellt wird, gibt die Protokollversion der Verbindung an und ermöglicht es dem Server, den Handshake zum Öffnen der Verbindung und die anschließend ausgetauschten Daten korrekt zu interpretieren. Anhand dieser Protokollinformationen kann die Verbindung jederzeit geschlossen werden, wenn der Server die eingehenden Daten nicht auf sichere Weise interpretieren kann.

Wenn die erste Anforderung vom Client nicht diesen Wert enthält oder einen Wert bereitstellt, der vom Server nicht erwartet wird, tritt ein WebSocket-Handshakefehler auf, und der Server sendet den erwarteten Wert an den Client.

## <a name="authentication"></a>Authentication
So werden beim Herstellen einer Verbindung über das Netzwerk die Authentifizierungsanmeldeinformationen bereitgestellt.

### <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>Bereitstellen eines Clientzertifikats mit der StreamSocket-Klasse
Die [**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Klasse unterstützt die Verwendung von SSL/TLS zum Authentifizieren des Servers, mit dem die App kommuniziert. In bestimmten Fällen muss auch die App selbst mit einem TLS-Clientzertifikat am Server authentifiziert werden. In Windows 10, können Sie ein Clientzertifikat bereitstellen, auf die [ **StreamSocket.Control** ](https://msdn.microsoft.com/library/windows/apps/br226893) Objekt (Dies muss festgelegt werden, bevor die TLS-Handshake gestartet wird). Wenn der Server das Clientzertifikat anfordert, reagiert Windows mit dem bereitgestellten Zertifikat.

Hier ist ein Codeausschnitt, in dem die Implementierung dazu dargestellt wird:

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

### <a name="providing-authentication-credentials-to-a-web-service"></a>Bereitstellen von Anmeldeinformationen zum Authentifizieren für einen Webdienst
Die Netzwerk-APIs, die Apps die Interaktion mit sicheren Webdiensten ermöglichen, stellen jeweils eigene Methoden zum Initialisieren eines Clients oder Festlegen eines Anforderungsheaders mit Anmeldeinformationen für die Server- und Proxyauthentifizierung bereit. Jede Methode wird mit einem [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061)-Objekt festgelegt, das einen Benutzernamen, ein Kennwort und die Ressource angibt, für die die Anmeldeinformationen verwendet werden. In der folgenden Tabelle werden diese APIs zugeordnet:

| **WebSockets** | [**MessageWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226848) |
|-------------------------|----------------------------------------------------------------------------------------------------------|
|  | [**MessageWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226847) |
|  | [**StreamWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226928) |
|  | [**StreamWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226927) |
| **Hintergrundübertragung** | [**BackgroundDownloader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701076) |
|  | [**BackgroundDownloader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701068) |
|  | [**BackgroundUploader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701184) |
|  | [**BackgroundUploader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701178) |
| **Syndication** | [**SyndicationClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702355) |
|  | [**SyndicationClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) |
|  | [**SyndicationClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) |
| **AtomPub** | [**AtomPubClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702262) |
|  | [**AtomPubClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243428) |
|  | [**AtomPubClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243423) |

## <a name="handling-network-exceptions"></a>Behandeln von Netzwerkausnahmen
In den meisten Bereichen der Programmierung ist eine Ausnahme auf ein erhebliches Problem oder einen Fehler zurückzuführen, dass durch einige Fehler im Programm verursacht wurde. In der Netzwerkprogrammierung gibt es eine zusätzliche Quelle für Ausnahmen: das Netzwerk selbst und der Art der Netzwerkkommunikation. Die Netzwerkkommunikation sind grundsätzlich nicht zuverlässig und anfällig für unerwartete Fehler. Für jede der Möglichkeiten, mit der Ihre App das Netzwerke verwendet, müssen Sie einige Statusinformationen verwalten; und der App-Code muss Netzwerkausnahmen behandeln, indem er diese Statusinformationen aktualisiert und die entsprechende Logik für Ihre App erneut initialisiert, um Kommunikationsfehler wiederherzustellen oder zu wiederholen.

Wenn Universelle Windows-Apps eine Ausnahme auslösen, kann Ihr Ausnahmehandler detailliertere Informationen zur Ursache der Ausnahme abrufen, um die Ausnahme besser verstehen und entsprechende Entscheidungen treffen zu können.

Jede Programmiersprache unterstützt eine Methode, mit der auf diese detaillierteren Informationen zugegriffen werden kann. Eine Ausnahme wird als **HRESULT**-Wert in Universellen Windows-Apps projiziert. Die *Winerror.h*-Includedatei enthält eine sehr umfangreiche Liste möglicher **HRESULT**-Werte, die Netzwerkfehler beinhaltet.

Die Netzwerk-APIs unterstützen verschiedene Methoden zum Abrufen der detaillierten Informationen zur Ursache der Ausnahme.

-   Einige APIs bieten eine Hilfsmethode, die den **HRESULT**-Wert der Ausnahme in einen Enumerationswert konvertiert.
-   Andere APIs bieten eine Methode zum Abrufen des tatsächlichen **HRESULT**-Werts.

## <a name="related-topics"></a>Verwandte Themen
* [Netzwerk-API-Verbesserungen in Windows 10](https://blogs.windows.com/buildingapps/2015/07/02/networking-api-improvements-in-windows-10/)
