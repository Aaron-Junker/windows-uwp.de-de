---
author: stevewhims
description: Um die Netzwerkkommunikation fortzusetzen, während sie sich nicht im Vordergrund befindet, kann eine App Hintergrundaufgaben und entweder Socketbroker- oder Steuerkanaltrigger verwenden.
title: Netzwerkkommunikation im Hintergrund
ms.assetid: 537F8E16-9972-435D-85A5-56D5764D3AC2
ms.author: stwhi
ms.date: 06/14/2018
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 34fad804bb36ad1b4ce92a56772c33318e10faa8
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "5711277"
---
# <a name="network-communications-in-the-background"></a>Netzwerkkommunikation im Hintergrund
Um die Netzwerkkommunikation fortzusetzen, während Sie sich nicht im Vordergrund befindet, kann Ihre app Hintergrundaufgaben und eine der folgenden zwei Optionen verwenden.
- Socketbroker. Wenn Ihre app Sockets für dauerhafte Verbindungen verwendet, wenn sie den Vordergrund verlässt, kann er den Besitz eines Sockets an einen System-socketbroker delegieren. Der Broker dann: Ihre app aktiviert, wenn Datenverkehr, für den Socket eingeht; überträgt den Besitz zurück an Ihre app; und Ihre app verarbeitet dann den eingehenden Datenverkehr.
- Steuerkanaltrigger. 

## <a name="performing-network-operations-in-background-tasks"></a>Netzwerkvorgänge in Hintergrundaufgaben
- Verwenden Sie einen [SocketActivityTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.socketactivitytrigger), um die Hintergrundaufgabe zu aktivieren, wenn ein Paket empfangen wird und Sie eine kurzlebige Aufgabe ausführen müssen. Nach dem Ausführen der Aufgabe sollte die Hintergrundaufgabe beendet werden, um Energie zu sparen.
- Verwenden Sie einen [ControlChannelTrigger](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), um die Hintergrundaufgabe zu aktivieren, wenn ein Paket empfangen wird und Sie eine langlebige Aufgabe ausführen müssen.

**Netzwerkbezogene Bedingungen und flags**

- Fügen Sie die **InternetAvailable**-Bedingung zur Hintergrundaufgabe [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) hinzu, um die Hintergrundaufgabe zu verzögern, bis der Netzwerkstapel ausgeführt wird. Dies spart Energie, da die Hintergrundaufgabe nicht ausgeführt wird, bis das Netzwerk verfügbar ist. Diese Bedingung stellt keine Aktivierung in Echtzeit bereit.

Unabhängig vom verwendeten Auslöser, legen Sie [IsNetworkRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder) für die Hintergrundaufgabe fest, um sicherzustellen, dass das Netzwerk während der Ausführung der Hintergrundaufgabe unterbrechungsfreie ausgeführt wird. Dies weist die Infrastruktur für Hintergrundaufgaben an, die Netzwerkverbindung für die Ausführung der Aufgabe auch dann beizubehalten, wenn sich das Gerät im verbundenen Standbymodus befindet. Wenn Ihre Hintergrundaufgabe keine **IsNetworkRequested**verwendet werden, klicken Sie dann kann Ihre Hintergrundaufgabe nicht Zugriff auf das Netzwerk, wenn sich dieses im verbundenen Standbymodus befindet (z. B. wenn der Bildschirm eines Smartphones ausgeschaltet ist).

## <a name="socket-broker-and-the-socketactivitytrigger"></a>Socketbroker und SocketActivityTrigger
Wenn Ihre App eine [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)-, [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)- oder [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)-Verbindung verwendet, sollten Sie mithilfe von [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) und Socketbroker Benachrichtigungen einrichten, um über eingehenden Datenverkehr für ihre App informiert zu werden, wenn diese im Hintergrund ausgeführt wird.

Damit die App Daten, die auf einem Socket empfangen werden, auch bei Inaktivität empfangen und verarbeiten kann, muss die App beim Start eine einmalige Einrichtung vornehmen und den Socket-Besitz dann an den Socketbroker übertragen, wenn sie in einen nicht aktiven Zustand wechselt.

Bei den Schritten für die einmalige Einrichtung handelt es sich um das Erstellen eines Triggers, das Registrieren einer Hintergrundaufgabe für den Trigger und das Aktivieren des Sockets für den Socketbroker:
  - Erstellen Sie einen **SocketActivityTrigger**, und registrieren Sie eine Hintergrundaufgabe für den Trigger, wobei der TaskEntryPoint-Parameter auf den Code zum Verarbeiten des empfangenen Pakets festgelegt ist.
```csharp
            var socketTaskBuilder = new BackgroundTaskBuilder();
            socketTaskBuilder.Name = _backgroundTaskName;
            socketTaskBuilder.TaskEntryPoint = _backgroundTaskEntryPoint;
            var trigger = new SocketActivityTrigger();
            socketTaskBuilder.SetTrigger(trigger);
            _task = socketTaskBuilder.Register();
```
  - Rufen Sie **EnableTransferOwnership** für den Socket auf, bevor Sie die Bindung für den Socket festlegen.
```csharp
           _tcpListener = new StreamSocketListener();

           // Note that EnableTransferOwnership() should be called before bind,
           // so that tcpip keeps required state for the socket to enable connected
           // standby action. Background task Id is taken as a parameter to tie wake pattern
           // to a specific background task.  
           _tcpListener. EnableTransferOwnership(_task.TaskId,SocketActivityConnectedStandbyAction.Wake);
           _tcpListener.ConnectionReceived += OnConnectionReceived;
           await _tcpListener.BindServiceNameAsync("my-service-name");
```

Nachdem der Socket ordnungsgemäß eingerichtet ist, rufen Sie vor Anhalten der App für den Socket **TransferOwnership** auf, um ihn an einen Socketbroker zu übertragen. Der Broker überwacht den Socket und aktiviert die Hintergrundaufgabe, wenn Daten empfangen werden. Im folgenden Beispiel wird die **TransferOwnership**-Hilfsfunktion verwendet, um die Übertragung für **StreamSocketListener**-Sockets auszuführen. (Beachten Sie, dass unterschiedliche Sockettypen jeweils eine eigene **TransferOwnership**-Methode besitzen. Sie müssen daher für die Besitzübertragung des Sockets die richtige Methode verwenden. In der Regel enthält der Code eine überladene **TransferOwnership**-Hilfsfunktion mit einer einzelnen Implementierung für jeden verwendeten Sockettyp, damit der **OnSuspending**-Code gut lesbar bleibt.)

Eine App überträgt den Besitz eines Sockets an einen Socketbroker und übergibt die ID für die Hintergrundaufgabe mit der entsprechenden Methode:
-   Einer der [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn804256)-Methoden für einen [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319).
-   Einer der [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn781433)-Methoden für einen [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882).
-   Einer der [**TransferOwnership**](https://msdn.microsoft.com/library/windows/apps/dn804407)-Methoden für einen [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906).

```csharp

// declare int _transferOwnershipCount as a field.

private void TransferOwnership(StreamSocketListener tcpListener)
{
    await tcpListener.CancelIOAsync();

    var dataWriter = new DataWriter();
    ++_transferOwnershipCount;
    dataWriter.WriteInt32(transferOwnershipCount);
    var context = new SocketActivityContext(dataWriter.DetachBuffer());
    tcpListener.TransferOwnership(_socketId, context);
}

private void OnSuspending(object sender, SuspendingEventArgs e)
{
    var deferral = e.SuspendingOperation.GetDeferral();

    TransferOwnership(_tcpListener);
    deferral.Complete();
}
```
Im Ereignishandler für die Hintergrundaufgabe:
   -  Rufen Sie zunächst eine Hintergrundaufgabenverzögerung ab, damit Sie das Ereignis mit asynchronen Methoden behandeln können.
```csharp
var deferral = taskInstance.GetDeferral();
```
   -  Als Nächstes extrahieren Sie die SocketActivityTriggerDetails aus den Ereignisargumenten und suchen den Grund für das Auslösen des Ereignisses:
```csharp
var details = taskInstance.TriggerDetails as SocketActivityTriggerDetails;
    var socketInformation = details.SocketInformation;
    switch (details.Reason)
```
   -   Wenn das Ereignis aufgrund der Socket-Aktivität ausgelöst wurde, erstellen Sie einen DataReader für den Socket, laden den Reader asynchron und verwenden die Daten entsprechend der Logik der App. Beachten Sie, dass Sie den Besitz des Sockets wieder an Socketbroker zurückgeben müssen, um bei weiterer Socket-Aktivität erneut benachrichtigt zu werden.

   Im folgenden Beispiel wird der vom Socket empfangene Text in einem Popup angezeigt.

```csharp
case SocketActivityTriggerReason.SocketActivity:
            var socket = socketInformation.StreamSocket;
            DataReader reader = new DataReader(socket.InputStream);
            reader.InputStreamOptions = InputStreamOptions.Partial;
            await reader.LoadAsync(250);
            var dataString = reader.ReadString(reader.UnconsumedBufferLength);
            ShowToast(dataString);
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   Wenn das Ereignis ausgelöst wurde, weil ein Keep-Alive-Zeitgeber abgelaufen ist, sollte der Code Daten über den Socket senden, damit der Socket geöffnet bleibt und der Keep-Alive-Zeitgeber neu gestartet wird. Dabei ist es wiederum wichtig, den Besitz des Sockets zurück an den Socketbroker zu übertragen, um weitere Ereignisbenachrichtigungen zu erhalten:

```csharp
case SocketActivityTriggerReason.KeepAliveTimerExpired:
            socket = socketInformation.StreamSocket;
            DataWriter writer = new DataWriter(socket.OutputStream);
            writer.WriteBytes(Encoding.UTF8.GetBytes("Keep alive"));
            await writer.StoreAsync();
            writer.DetachStream();
            writer.Dispose();
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   Wenn das Ereignis durch das Schließen des Sockets ausgelöst wurde, richten Sie den Socket erneut ein und stellen sicher, anschließend den Besitz des neuen Sockets an den Socketbroker zu übertragen. In diesem Beispiel werden der Hostname und -port in den lokalen Einstellungen gespeichert, damit sie zum Einrichten einer neuen Socketverbindung verwendet werden können:

```csharp
case SocketActivityTriggerReason.SocketClosed:
            socket = new StreamSocket();
            socket.EnableTransferOwnership(taskInstance.Task.TaskId, SocketActivityConnectedStandbyAction.Wake);
            if (ApplicationData.Current.LocalSettings.Values["hostname"] == null)
            {
                break;
            }
            var hostname = (String)ApplicationData.Current.LocalSettings.Values["hostname"];
            var port = (String)ApplicationData.Current.LocalSettings.Values["port"];
            await socket.ConnectAsync(new HostName(hostname), port);
            socket.TransferOwnership(socketId);
            break;
```

   -   Denken Sie daran, die Verzögerung abzuschließen, sobald die Verarbeitung der Ereignisbenachrichtigung beendet wurde:

```csharp
  deferral.Complete();
```

Ein vollständiges Beispiel zur Verwendung des [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) und des Socketbroker finden Sie im [Beispiel für einen SocketActivityStreamSocket](http://go.microsoft.com/fwlink/p/?LinkId=620606). Die Initialisierung des Sockets wird in „Scenario1\_Connect.xaml.cs“ und die Implementierung der Hintergrundaufgabe in „SocketActivityTask.cs“ ausgeführt.

Wahrscheinlich wird Ihnen auffallen, dass im Beispiel beim Erstellen eines neuen Sockets oder beim Aufrufen eines vorhandenen Sockets **TransferOwnership** aufgerufen wird und dazu nicht der in diesem Thema beschriebene **OnSuspending**-Ereignishandler verwendet wird. Dies liegt daran, dass in diesem Beispiel schwerpunktmäßig der [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) veranschaulicht werden soll und der Socket während der Ausführung für keine weiteren Aktivitäten verwendet wird. In der Regel wird Ihre App komplexer sein, sodass Sie mit **OnSuspending** bestimmen sollten, wann **TransferOwnership** aufgerufen wird.

## <a name="control-channel-triggers"></a>Steuerkanaltrigger
Stellen Sie zunächst sicher, dass Steuerkanaltrigger korrekt verwendet werden. Wenn Sie [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)oder [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) Verbindungen verwenden, empfehlen wir, dass Sie [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)verwenden. Sie können Steuerkanaltrigger für **StreamSocket** verwenden, wobei diese jedoch mehr Ressourcen belegen und möglicherweise nicht im verbundenen Standbymodus funktionieren.

Wenn Sie WebSockets, [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151), [**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)oder [**Windows.Web.Http.HttpClient**](/uwp/api/windows.web.http.httpclient)verwenden, müssen Sie [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)verwenden.

## <a name="controlchanneltrigger-with-websockets"></a>ControlChannelTrigger mit WebSockets
Bei Verwendung von [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) sind einige wichtige Punkte zu berücksichtigen. Bei Verwendung von **MessageWebSocket** oder **StreamWebSocket** mit **ControlChannelTrigger** sollten Sie transportspezifische Verwendungsmuster und bewährte Methoden nutzen. Diese Aspekte beeinflussen, wie Anforderungen für den Paketempfang über **StreamWebSocket** verarbeitet werden. Anforderungen für den Paketempfang über **MessageWebSocket** sind davon nicht betroffen.

Beachten Sie bei Verwendung von [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) die folgenden Verwendungsmuster und bewährten Methoden:

-   Ein ausstehender Socketempfang muss immer bereitgestellt bleiben. Das ist erforderlich, damit die Pushbenachrichtigungsaufgaben auftreten können.
-   Das WebSocket-Protokoll definiert ein Standardmodell für Keep-Alive-Meldungen. Die [**WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531)-Klasse kann vom Client initiierte WebSocket-Protokoll-Keep-Alive-Meldungen an den Server senden. Die **WebSocketKeepAlive**-Klasse muss von der App als „TaskEntryPoint“ für einen „KeepAliveTrigger“ registriert werden.

Die Verarbeitung von Anforderungen für den Paketempfang über [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) wird durch eine Reihe besonderer Faktoren beeinflusst. Insbesondere gilt, dass die App bei Verwendung von **StreamWebSocket** mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) für Lesevorgänge statt des **await**-Modells in C# und VB.NET oder Aufgaben in C++ ein asynchrones Rohmuster verwenden muss. Ein asynchrones Rohmuster wird im folgenden Codebeispiel weiter unten in diesem Abschnitt veranschaulicht.

Bei Verwendung eines asynchronen Rohmusters kann Windows die [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode für die Hintergrundaufgabe für [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) mit der Rückgabe des Empfangsabschlussrückrufs synchronisieren. Die **Run**-Methode wird nach Rückgabe des Abschlussrückrufs aufgerufen. Dies stellt sicher, dass die App die Daten/Fehler vor Aufruf der **Run**-Methode empfängt.

Beachten Sie, dass die App einen weiteren Lesevorgang bereitstellen muss, bevor sie die Steuerung vom Abschlussrückruf zurückgibt. Berücksichtigen Sie außerdem, dass [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) nicht direkt mit dem [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)- oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Transport verwendet werden kann, da hierdurch die oben beschriebene Synchronisierung unterbrochen wird. Die direkte Verwendung der [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135)-Methode zusätzlich zum Transport wird nicht unterstützt. Stattdessen kann die von der [**IInputStream.ReadAsync**](https://msdn.microsoft.com/library/windows/apps/br241719)-Methode für die [**StreamWebSocket.InputStream**](https://msdn.microsoft.com/library/windows/apps/br226936)-Eigenschaft zurückgegebene [**IBuffer**](https://msdn.microsoft.com/library/windows/apps/br241656)-Schnittstelle später zur weiteren Verarbeitung an die [**DataReader.FromBuffer**](https://msdn.microsoft.com/library/windows/apps/br208133)-Methode übergeben werden.

Im folgenden Beispiel wird ein asynchrones Rohmuster zur Verarbeitung von Lesevorgängen für [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) verwendet.

```csharp
void PostSocketRead(int length)
{
    try
    {
        var readBuf = new Windows.Storage.Streams.Buffer((uint)length);
        var readOp = socket.InputStream.ReadAsync(readBuf, (uint)length, InputStreamOptions.Partial);
        readOp.Completed = (IAsyncOperationWithProgress<IBuffer, uint>
            asyncAction, AsyncStatus asyncStatus) =>
        {
            switch (asyncStatus)
            {
                case AsyncStatus.Completed:
                case AsyncStatus.Error:
                    try
                    {
                        // GetResults in AsyncStatus::Error is called as it throws a user friendly error string.
                        IBuffer localBuf = asyncAction.GetResults();
                        uint bytesRead = localBuf.Length;
                        readPacket = DataReader.FromBuffer(localBuf);
                        OnDataReadCompletion(bytesRead, readPacket);
                    }
                    catch (Exception exp)
                    {
                        Diag.DebugPrint("Read operation failed:  " + exp.Message);
                    }
                    break;
                case AsyncStatus.Canceled:

                    // Read is not cancelled in this sample.
                    break;
           }
       };
   }
   catch (Exception exp)
   {
       Diag.DebugPrint("failed to post a read failed with error:  " + exp.Message);
   }
}
```

Der Handler für den Abschluss des Lesevorgangs wird auf jeden Fall ausgelöst, bevor die [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode für die Hintergrundaufgabe für [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) aufgerufen wird. Windows hält die interne Synchronisierung an, um auf die Rückgabe einer App vom Rückruf des Lesevorgangabschlusses zu warten. Meist kann die App die Daten oder den Fehler für [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) im Rückruf des Lesevorgangsabschlusses in kurzer Zeit verarbeiten. Die Meldung als solche wird im Kontext der **IBackgroundTask.Run**-Methode verarbeitet. Im Beispiel unten wird dieser Aspekt anhand einer Meldungswarteschlange veranschaulicht, in die der Handler für den Lesevorgangsabschluss die Meldung einfügt, die später von der Hintergrundaufgabe verarbeitet wird.

Im folgenden Beispiel wird der Handler für den Lesevorgangsabschluss zusammen mit einem asynchronen Rohmuster zur Verarbeitung von Lesevorgängen für [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) verwendet.

```csharp
public void OnDataReadCompletion(uint bytesRead, DataReader readPacket)
{
    if (readPacket == null)
    {
        Diag.DebugPrint("DataReader is null");

        // Ideally when read completion returns error,
        // apps should be resilient and try to
        // recover if there is an error by posting another recv
        // after creating a new transport, if required.
        return;
    }
    uint buffLen = readPacket.UnconsumedBufferLength;
    Diag.DebugPrint("bytesRead: " + bytesRead + ", unconsumedbufflength: " + buffLen);

    // check if buffLen is 0 and treat that as fatal error.
    if (buffLen == 0)
    {
        Diag.DebugPrint("Received zero bytes from the socket. Server must have closed the connection.");
        Diag.DebugPrint("Try disconnecting and reconnecting to the server");
        return;
    }

    // Perform minimal processing in the completion
    string message = readPacket.ReadString(buffLen);
    Diag.DebugPrint("Received Buffer : " + message);

    // Enqueue the message received to a queue that the push notify
    // task will pick up.
    AppContext.messageQueue.Enqueue(message);

    // Post another receive to ensure future push notifications.
    PostSocketRead(MAX_BUFFER_LENGTH);
}
```

Ein weiteres Detail für WebSockets ist der Keep-Alive-Handler. Das WebSocket-Protokoll definiert ein Standardmodell für Keep-Alive-Meldungen.

Bei Verwendung der Klasse [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) muss eine [**WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531)-Klasseninstanz als [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774) für KeepAliveTrigger registriert werden. Dies ist erforderlich, damit die App fortgesetzt werden kann und regelmäßig Keep-Alive-Meldungen an den Server (Remoteendpunkt) gesendet werden können. Dieser Vorgang muss sowohl im App-Code für die Hintergrundregistrierung als auch im Paketmanifest enthalten sein.

Der Aufgabeneinstiegspunkt für [**Windows.Sockets.WebSocketKeepAlive**](https://msdn.microsoft.com/library/windows/apps/hh701531) muss an zwei Stellen angegeben werden:

-   Beim Erstellen des „KeepAliveTrigger“-Triggers im Quellcode (siehe Beispiel unten).
-   Im App-Paketmanifest für die Keepalive-Hintergrundaufgabendeklaration.

Im folgenden Beispiel werden eine Netzwerktrigger-Benachrichtigung und ein Keep-Alive-Trigger unter dem &lt;Application&gt;-Element in einem App-Manifest hinzugefügt.

```xml
  <Extensions>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Background.PushNotifyTask">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Windows.Networking.Sockets.WebSocketKeepAlive">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
  </Extensions>
```

Besondere Sorgfalt ist geboten, wenn eine App eine **await**-Anweisung im Kontext einer [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)-Klasse und eines asynchronen Vorgangs für eine [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-, [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)- oder [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-Klasse verwendet. Ein **Task&lt;bool&gt;**-Objekt kann verwendet werden, um eine **ControlChannelTrigger**-Klasse für Pushbenachrichtigungen und WebSocket-Keep-Alive-Meldungen in der **StreamWebSocket**-Klasse zu registrieren und die Transportverbindung herzustellen. Im Rahmen der Registrierung wird der **StreamWebSocket**-Transport als Transport für **ControlChannelTrigger** festgelegt, und es wird ein Lesevorgang bereitgestellt. **Task.Result** blockiert den aktuellen Thread, bis alle Schritte der Aufgabe ausgeführt und Anweisungen im Nachrichtentext zurückgegeben wurden. Die Aufgabe ist erst gelöst, wenn die Methode „true“ oder „false“ zurückgibt. So wird die vollständige Ausführung der Methode sichergestellt. Die **Task**-Klasse kann mehrere **await** -Anweisungen erhalten, die durch die **Task**-Klasse geschützt werden. Diese Vorgehensweise sollte bei Nutzung eines **StreamWebSocket**- oder **MessageWebSocket**-Transports mit dem **ControlChannelTrigger**-Objekt verwendet werden. Für Vorgänge, deren Durchführung u. U. längere Zeit in Anspruch nimmt, (z. B. ein typischer asynchroner Lesevorgang) sollte die App das zuvor erläuterte asynchrone Rohmuster verwenden.

Im folgenden Beispiel wird die [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)-Klasse für Pushbenachrichtigungen registriert. Die Registrierung von WebSocket-Keep-Alive-Meldungen erfolgt über die [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Klasse.

```csharp
private bool RegisterWithControlChannelTrigger(string serverUri)
{
    // Make sure the objects are created in a system thread
    // Demonstrate the core registration path
    // Wait for the entire operation to complete before returning from this method.
    // The transport setup routine can be triggered by user control, by network state change
    // or by keepalive task
    Task<bool> registerTask = RegisterWithCCTHelper(serverUri);
    return registerTask.Result;
}

async Task<bool> RegisterWithCCTHelper(string serverUri)
{
    bool result = false;
    socket = new StreamWebSocket();

    // Specify the keepalive interval expected by the server for this app
    // in order of minutes.
    const int serverKeepAliveInterval = 30;

    // Specify the channelId string to differentiate this
    // channel instance from any other channel instance.
    // When background task fires, the channel object is provided
    // as context and the channel id can be used to adapt the behavior
    // of the app as required.
    const string channelId = "channelOne";

    // For websockets, the system does the keepalive on behalf of the app
    // But the app still needs to specify this well known keepalive task.
    // This should be done here in the background registration as well
    // as in the package manifest.
    const string WebSocketKeepAliveTask = "Windows.Networking.Sockets.WebSocketKeepAlive";

    // Try creating the controlchanneltrigger if this has not been already
    // created and stored in the property bag.
    ControlChannelTriggerStatus status;

    // Create the ControlChannelTrigger object and request a hardware slot for this app.
    // If the app is not on LockScreen, then the ControlChannelTrigger constructor will
    // fail right away.
    try
    {
        channel = new ControlChannelTrigger(channelId, serverKeepAliveInterval,
                                   ControlChannelTriggerResourceType.RequestHardwareSlot);
    }
    catch (UnauthorizedAccessException exp)
    {
        Diag.DebugPrint("Is the app on lockscreen? " + exp.Message);
        return result;
    }

    Uri serverUriInstance;
    try
    {
        serverUriInstance = new Uri(serverUri);
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Error creating URI: " + exp.Message);
        return result;
    }

    // Register the apps background task with the trigger for keepalive.
    var keepAliveBuilder = new BackgroundTaskBuilder();
    keepAliveBuilder.Name = "KeepaliveTaskForChannelOne";
    keepAliveBuilder.TaskEntryPoint = WebSocketKeepAliveTask;
    keepAliveBuilder.SetTrigger(channel.KeepAliveTrigger);
    keepAliveBuilder.Register();

    // Register the apps background task with the trigger for push notification task.
    var pushNotifyBuilder = new BackgroundTaskBuilder();
    pushNotifyBuilder.Name = "PushNotificationTaskForChannelOne";
    pushNotifyBuilder.TaskEntryPoint = "Background.PushNotifyTask";
    pushNotifyBuilder.SetTrigger(channel.PushNotificationTrigger);
    pushNotifyBuilder.Register();

    // Tie the transport method to the ControlChannelTrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the ControlChannelTrigger object can be reused to plug in a new transport by
    // calling UsingTransport API again.
    try
    {
        channel.UsingTransport(socket);

        // Connect the socket
        //
        // If connect fails or times out it will throw exception.
        // ConnectAsync can also fail if hardware slot was requested
        // but none are available
        await socket.ConnectAsync(serverUriInstance);

        // Call WaitForPushEnabled API to make sure the TCP connection has
        // been established, which will mean that the OS will have allocated
        // any hardware slot for this TCP connection.
        //
        // In this sample, the ControlChannelTrigger object was created by
        // explicitly requesting a hardware slot.
        //
        // On systems that without connected standby, if app requests hardware slot as above,
        // the system will fallback to a software slot automatically.
        //
        // On systems that support connected standby,, if no hardware slot is available, then app
        // can request a software slot by re-creating the ControlChannelTrigger object.
        status = channel.WaitForPushEnabled();
        if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
            && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
        {
            throw new Exception(string.Format("Neither hardware nor software slot could be allocated. ChannelStatus is {0}", status.ToString()));
        }

        // Store the objects created in the property bag for later use.
        CoreApplication.Properties.Remove(channel.ControlChannelTriggerId);

        var appContext = new AppContext(this, socket, channel, channel.ControlChannelTriggerId);
        ((IDictionary<string, object>)CoreApplication.Properties).Add(channel.ControlChannelTriggerId, appContext);
        result = true;

        // Almost done. Post a read since we are using streamwebsocket
        // to allow push notifications to be received.
        PostSocketRead(MAX_BUFFER_LENGTH);
    }
    catch (Exception exp)
    {
         Diag.DebugPrint("RegisterWithCCTHelper Task failed with: " + exp.Message);

         // Exceptions may be thrown for example if the application has not
         // registered the background task class id for using real time communications
         // broker in the package manifest.
    }
    return result
}
```

Weitere Informationen zur Verwendung von [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) finden Sie im [Beispiel für einen ControlChannelTrigger-StreamSocket](http://go.microsoft.com/fwlink/p/?linkid=251232).

## <a name="controlchanneltrigger-with-httpclient"></a>ControlChannelTrigger mit HttpClient
Bei Verwendung von [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) müssen einige besondere Punkte berücksichtigt werden. Bei Verwendung von [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) mit **ControlChannelTrigger** sollten Sie sich an einige transportspezifische Verwendungsmuster und bewährte Methoden halten. Diese Aspekte beeinflussen, wie Anforderungen für den Paketempfang in der [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Klasse verarbeitet werden.

**Hinweis:** [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) mit SSL wird derzeit nicht unterstützt unter Verwendung des netzwerktriggerfeatures und [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032).
 
Beachten Sie bei Verwendung von [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) die folgenden Verwendungsmuster und bewährten Methoden:

-   Die App muss möglicherweise verschiedene Eigenschaften und Header für das [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)- oder [HttpClientHandler](http://go.microsoft.com/fwlink/p/?linkid=241638)-Objekt im [System.Net.Http](http://go.microsoft.com/fwlink/p/?linkid=227894)-Namespace festlegen, bevor die Anforderung an den entsprechenden URI gesendet wird.
-   Eine App muss ggf. zu Beginn eine Anforderung senden, um den Transport zu testen und richtig einzurichten, bevor der [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Transport zur Verwendung mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) erstellt wird. Nachdem die App festgestellt hat, dass der Transport richtig eingerichtet ist, kann ein [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Objekt als Transportobjekt zur Verwendung mit dem **ControlChannelTrigger**-Objekt konfiguriert werden. Dieser Vorgang soll verhindern, dass die über den Transport hergestellte Verbindung in manchen Szenarien abgebrochen wird. Bei Verwendung von SSL mit einem Zertifikat kann für eine App die Anzeige eines Dialogfelds für die Eingabe einer PIN oder – bei mehreren Zertifikaten – für die Auswahl eines Zertifikats erforderlich sein. Proxyauthentifizierung und Serverauthentifizierung können ebenfalls erforderlich sein. Wenn die Proxy- oder Serverauthentifizierung abläuft, wird die Verbindung möglicherweise geschlossen. Eine Möglichkeit, wie eine App mit dem Problem des Authentifizierungsablaufs umgehen kann, besteht in der Verwendung eines Zeitgebers. Wenn eine HTTP-Umleitung erforderlich ist, ist nicht sichergestellt, dass die zweite Verbindung verlässlich hergestellt werden kann. Eine anfängliche Testanforderung stellt sicher, dass die App die neueste umgeleitete URL verwenden kann, bevor das [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Objekt als Transport mit dem **ControlChannelTrigger**-Objekt verwendet wird.

Im Unterschied zu anderen Netzwerktransporten kann das [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Objekt nicht direkt an die [**UsingTransport**](https://msdn.microsoft.com/library/windows/apps/hh701175)-Methode des [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)-Objekts übergeben werden. Sie müssen vielmehr ein [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153)-Objekt speziell für die Verwendung mit dem [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Objekt und **ControlChannelTrigger** erstellen. Das [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153)-Objekt wird mithilfe der [RtcRequestFactory.Create](http://go.microsoft.com/fwlink/p/?linkid=259154)-Methode erstellt. Das erstellte [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153)-Objekt wird dann an die **UsingTransport**-Methode übergeben.

Das folgende Beispiel zeigt, wie Sie ein [HttpRequestMessage](http://go.microsoft.com/fwlink/p/?linkid=259153)-Objekt für die Verwendung mit dem [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Objekt und [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) erstellen können.

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

public HttpRequestMessage httpRequest;
public HttpClient httpClient;
public HttpRequestMessage httpRequest;
public ControlChannelTrigger channel;
public Uri serverUri;

private void SetupHttpRequestAndSendToHttpServer()
{
    try
    {
        // For HTTP based transports that use the RTC broker, whenever we send next request, we will abort the earlier
        // outstanding http request and start new one.
        // For example in case when http server is taking longer to reply, and keep alive trigger is fired in-between
        // then keep alive task will abort outstanding http request and start a new request which should be finished
        // before next keep alive task is triggered.
        if (httpRequest != null)
        {
            httpRequest.Dispose();
        }

        httpRequest = RtcRequestFactory.Create(HttpMethod.Get, serverUri);

        SendHttpRequest();
    }
        catch (Exception e)
    {
        Diag.DebugPrint("Connect failed with: " + e.ToString());
        throw;
    }
}
```

Für die Behandlung von Anforderungen zum Senden von HTTP-Anforderungen über [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) zur Initiierung des Antwortempfangs müssen einige besondere Punkte berücksichtigt werden. Insbesondere muss die App bei Verwendung von [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) Sendevorgänge mithilfe einer Aufgabe statt des **await**-Modells behandeln.

Bei Verwendung von [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) erfolgt keine Synchronisierung der [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode der Hintergrundaufgabe für [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) mit der Rückgabe des Empfangsabschlussrückrufs. Die App kann daher nur das „HttpResponseMessage“-Blockierverfahren für die **Run**-Methode verwenden und auf den Empfang der vollständigen Antwort warten.

Die Verwendung von [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) unterscheidet sich deutlich von den [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)-, [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)- oder [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)-Transporten. Der [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Empfangsrückruf wird ab dem [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637)-Code mithilfe einer Aufgabe an die App übermittelt. Dies impliziert, dass die **ControlChannelTrigger**-Pushbenachrichtigungsaufgabe ausgelöst wird, sobald die Daten oder der Fehler an die App übermittelt werden. Im nachfolgenden Beispiel speichert der Code die von der [HttpClient.SendAsync](http://go.microsoft.com/fwlink/p/?linkid=241637)-Methode zurückgegebene „responseTask“ im globalen Speicher. Die Pushbenachrichtigungsaufgabe übernimmt diese dann für die Inline-Verarbeitung.

Das folgende Beispiel zeigt, wie Sie Sendeanforderungen für [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) bei Verwendung mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) behandeln.

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

private void SendHttpRequest()
{
    if (httpRequest == null)
    {
        throw new Exception("HttpRequest object is null");
    }

    // Tie the transport method to the controlchanneltrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the controlchanneltrigger object can be reused to plugin a new transport by
    // calling UsingTransport API again.
    channel.UsingTransport(httpRequest);

    // Call the SendAsync function to kick start the TCP connection establishment
    // process for this http request.
    Task<HttpResponseMessage> httpResponseTask = httpClient.SendAsync(httpRequest);

    // Call WaitForPushEnabled API to make sure the TCP connection has been established,
    // which will mean that the OS will have allocated any hardware slot for this TCP connection.
    ControlChannelTriggerStatus status = channel.WaitForPushEnabled();
    Diag.DebugPrint("WaitForPushEnabled() completed with status: " + status);
    if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
        && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
    {
        throw new Exception("Hardware/Software slot not allocated");
    }

    // The HttpClient receive callback is delivered via a Task to the app.
    // The notification task will fire as soon as the data or error is dispatched
    // Enqueue the responseTask returned by httpClient.sendAsync
    // into a queue that the push notify task will pick up and process inline.
    AppContext.messageQueue.Enqueue(httpResponseTask);
}
```

Das folgende Beispiel zeigt, wie Sie empfangene Antworten für [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) bei Verwendung mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) lesen.

```csharp
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

public string ReadResponse(Task<HttpResponseMessage> httpResponseTask)
{
    string message = null;
    try
    {
        if (httpResponseTask.IsCanceled || httpResponseTask.IsFaulted)
        {
            Diag.DebugPrint("Task is cancelled or has failed");
            return message;
        }
        // We' ll wait until we got the whole response.
        // This is the only supported scenario for HttpClient for ControlChannelTrigger.
        HttpResponseMessage httpResponse = httpResponseTask.Result;
        if (httpResponse == null || httpResponse.Content == null)
        {
            Diag.DebugPrint("Cannot read from httpresponse, as either httpResponse or its content is null. try to reset connection.");
        }
        else
        {
            // This is likely being processed in the context of a background task and so
            // synchronously read the Content' s results inline so that the Toast can be shown.
            // before we exit the Run method.
            message = httpResponse.Content.ReadAsStringAsync().Result;
        }
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Failed to read from httpresponse with error:  " + exp.ToString());
    }
    return message;
}
```

Weitere Informationen zur Verwendung von [HttpClient](http://go.microsoft.com/fwlink/p/?linkid=241637) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) finden Sie im [Beispiel für einen ControlChannelTrigger-HttpClient](http://go.microsoft.com/fwlink/p/?linkid=258323).

## <a name="controlchanneltrigger-with-ixmlhttprequest2"></a>ControlChannelTrigger mit IXMLHttpRequest2
Bei Verwendung von [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) müssen einige besondere Punkte berücksichtigt werden. Bei Verwendung von **IXMLHTTPRequest2** mit **ControlChannelTrigger** sollten Sie sich an einige transportspezifische Verwendungsmuster und bewährte Methoden halten. Die Verwendung von **ControlChannelTrigger** wirkt sich nicht auf die Behandlung von Anforderungen zum Senden oder Empfangen von HTTP-Anforderungen über **IXMLHTTPRequest2** aus.

Verwendungsmuster und bewährte Methoden für die Verwendung von [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)

-   Ein als Transport verwendetes [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151)-Objekt ist für nur eine Anforderung/Antwort gültig. Bei Verwendung mit dem [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)-Objekt sollte das **ControlChannelTrigger**-Objekt einmalig erstellt und eingerichtet werden. Rufen Sie anschließend jedes Mal die [**UsingTransport**](https://msdn.microsoft.com/library/windows/apps/hh701175)-Methode auf, und ordnen Sie jedes Mal ein neues **IXMLHTTPRequest2**-Objekt zu. Die App sollte vor der Bereitstellung eines neuen **IXMLHTTPRequest2**-Objekts das vorherige **IXMLHTTPRequest2**-Objekt löschen, damit sie die zugeordneten Ressourcenbeschränkungen nicht überschreitet.
-   Die App muss ggf. die Methoden [**SetProperty**](https://msdn.microsoft.com/library/windows/desktop/hh831167) und [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/desktop/hh831168) aufrufen, um den HTTP-Transport vor dem Aufruf der [**Send**](https://msdn.microsoft.com/library/windows/desktop/hh831164)-Methode einzurichten.
-   Die App muss zu Beginn gegebenenfalls eine [**Send**](https://msdn.microsoft.com/library/windows/desktop/hh831164)-Anforderung senden, um den Transport zu testen und richtig einzurichten, bevor der Transport zur Verwendung mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) erstellt wird. Nachdem die App festgestellt hat, dass der Transport richtig eingerichtet ist, kann das [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151)-Objekt als Transportobjekt zur Verwendung mit **ControlChannelTrigger** konfiguriert werden. Dieser Vorgang soll verhindern, dass die über den Transport hergestellte Verbindung in manchen Szenarien abgebrochen wird. Bei Verwendung von SSL mit einem Zertifikat kann für eine App die Anzeige eines Dialogfelds für die Eingabe einer PIN oder – bei mehreren Zertifikaten – für die Auswahl eines Zertifikats erforderlich sein. Proxyauthentifizierung und Serverauthentifizierung können ebenfalls erforderlich sein. Wenn die Proxy- oder Serverauthentifizierung abläuft, wird die Verbindung möglicherweise geschlossen. Eine Möglichkeit, wie eine App mit dem Problem des Authentifizierungsablaufs umgehen kann, besteht in der Verwendung eines Zeitgebers. Wenn eine HTTP-Umleitung erforderlich ist, ist nicht sichergestellt, dass die zweite Verbindung verlässlich hergestellt werden kann. Eine anfängliche Testanforderung stellt sicher, dass die App die neueste umgeleitete URL verwenden kann, bevor das **IXMLHTTPRequest2**-Objekt als Transport mit dem **ControlChannelTrigger**-Objekt verwendet wird.

Weitere Informationen zur Verwendung von [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151) mit [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) finden Sie im [Beispiel für ControlChannelTrigger mit IXMLHTTPRequest2](http://go.microsoft.com/fwlink/p/?linkid=258538).

## <a name="important-apis"></a>Wichtige APIs
* [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)
* [ControlChannelTrigger](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)
