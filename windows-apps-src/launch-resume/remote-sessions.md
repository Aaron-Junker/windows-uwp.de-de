---
title: Verbinden von Geräten über Remotesitzungen
description: Ermöglichen Sie die gemeinsame Nutzung auf verbundenen Geräten, indem Sie diese in einer Remotesitzung vereinen.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.date: 06/28/2017
ms.topic: article
keywords: Windows 10, Uwp, verbundene Geräte "," Remotesystemen "," ROM "," Projekt "ROME"
ms.localizationpriority: medium
ms.openlocfilehash: 3dd23603df1f1c3fac151da2aea2f8435b3ee423
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633415"
---
# <a name="connect-devices-through-remote-sessions"></a>Verbinden von Geräten über Remotesitzungen

Die Remotesitzungsfunktion ermöglicht einer App, eine Verbindung mit anderen Geräten über eine Sitzung herzustellen, entweder für explizite App-Nachrichten oder für im Broker gespeicherte ausgetauschte Daten, die vom System verwaltet werden, wie z. B. der **[SpatialEntityStore](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialentitystore)** für ein eine gemeinsame holografische Nutzung von Windows Holographic-Geräten.

Remotesitzungen können von jedem Windows-Gerät erstellt werden, und jedes Windows-Gerät kann einen Beitritt anfordern (obwohl Sitzungen ausschließlich per Einladung aufgerufen werden können), einschließlich Geräte, auf denen andere Benutzer angemeldet sind. Dieses Handbuch enthält einen grundlegenden Beispielcode für alle wichtigen Szenarien, die Remotesitzungen verwenden. Dieser Code kann in ein vorhandenes App-Projekt integriert und nach Bedarf geändert werden. Informationen über eine End-to-End-Implementierung finden Sie unter [Beispiel-App für ein Quizspiel](https://github.com/microsoft/Windows-appsample-remote-system-sessions)).

## <a name="preliminary-setup"></a>Vorläufige Einrichtung

### <a name="add-the-remotesystem-capability"></a>Hinzufügen der Funktionen „remoteSystem“

Damit Ihre App eine App auf einem Remotegerät starten kann, müssen Sie Ihrem App-Paketmanifest die Funktion `remoteSystem` hinzufügen. Sie können den Paketmanifest-Designer verwenden, um die Funktion hinzuzufügen, indem Sie auf der Registerkarte **Funktionen** die Option **Remotesystem** auswählen. Sie können auch der Datei _Package.appxmanifest_ Ihres Projekts die folgende Zeile manuell hinzufügen.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>Aktivieren der benutzerübergreifenden Ermittlung auf dem Gerät
Remotesitzungen ermöglichen das Herstellen einer Verbindung von mehreren verschiedenen Benutzern, daher muss auf den betreffenden Geräten die benutzerübergreifende Freigabe aktiviert sein. Dies ist eine Systemeinstellung, die über eine statische Methode in der **RemoteSystem**-Klasse abgefragt werden kann:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

Um diese Einstellung zu ändern, muss der Benutzer die Anwendung **Einstellungen** öffnen. Im Menü **System** > **Geteilte Umgebungen** > **Auf Geräten freigeben** befindet sich ein Dropdown-Feld, in dem der Benutzer angeben kann, mit welchen Geräten sein System teilen kann.

![Einstellungsseite geteilter Umgebungen](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>Angeben des erforderlichen Namespace
Um alle Codeausschnitte in diesem Handbuch zu verwenden, benötigen Sie die folgenden `using`-Anweisungen in Ihrer Dateien.

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>Erstellen einer Remotesitzung

Um eine Remotesitzungsinstanz zu erstellen, müssen Sie mit einem **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)**-Objekt beginnen. Verwenden Sie das folgende Framework, um eine neue Sitzung zu erstellen und Teilnehmeranfragen von anderen Geräten zu bearbeiten.

```csharp
public async void CreateSession() {
    
    // create a session controller
    RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob’s Minecraft game");
    
    // register the following code to handle the JoinRequested event
    manager.JoinRequested += async (sender, args) => {
        // Get the deferral
        var deferral = args.GetDeferral();
        
        // display the participant (args.JoinRequest.Participant) on UI, giving the 
        // user an opportunity to respond
        // ...
        
        // If the user chooses "accept", accept this remote system as a participant
        args.JoinRequest.Accept();
    };
    
    // create and start the session
    RemoteSystemSessionCreationResult createResult = await manager.CreateSessionAsync();
    
    // handle the creation result
    if (createResult.Status == RemoteSystemSessionCreationStatus.Success) {
        // creation was successful, get a reference to the session
        RemoteSystemSession currentSession = createResult.Session;
        
        // optionally subscribe to the disconnection event
        currentSession.Disconnected += async (sender, args) => {
            // update the UI, using args.Reason
            //...
        };
    
        // Use session (see later section)
        //...
    
    } else if (createResult.Status == RemoteSystemSessionCreationStatus.SessionLimitsExceeded) {
        // creation failed. Optionally update UI to indicate that there are too many sessions in progress
    } else {
        // creation failed for an unknown reason. Optionally update UI
    }
}
```

### <a name="make-a-remote-session-invite-only"></a>Erstellen einer ausschließlich per Einladung aufgerufenen Remotesitzung

Wenn Sie verhindern möchten, dass Ihre Remotesitzung öffentlich sichtbar ist, können Sie diese ausschließlich per Einladung anmelden. Nur die Geräte, die eine Einladung erhalten, können Beitrittsanfragen senden. 

Das Verfahren ist größtenteils identisch wie oben, aber beim Erstellen der **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)**-Instanz übergeben Sie ein konfiguriertes **[RemoteSystemSessionOptions](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)**-Objekt.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

Um eine Einladung zu senden, müssen Sie einen Verweis auf das Empfänger-Remotesystem (durch die normale Remotesystemermittlung) haben. Übergeben Sie einfach diesen Verweis in die **[SendInvitationAsync](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)**-Methode des Sitzungsobjekts. Alle Teilnehmer in einer Sitzung verfügen über einen Verweis auf die Remotesitzung (siehe nächster Abschnitt), damit alle Teilnehmer eine Einladung senden können.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>Ermitteln und Beitreten einer Remotesitzung

Der Prozess der Ermittlung von Remotesitzungen wird von der **[RemoteSystemSessionWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)**-Klasse behandelt und ähnelt der Ermittlung einzelner Remote-Systeme.

```csharp
public void DiscoverSessions() {
    
    // create a watcher for remote system sessions
    RemoteSystemSessionWatcher sessionWatcher = RemoteSystemSession.CreateWatcher();
    
    // register a handler for the "added" event
    sessionWatcher.Added += async (sender, args) => {
        
        // get a reference to the info about the discovered session
        RemoteSystemSessionInfo sessionInfo = args.SessionInfo;
        
        // Optionally update the UI with the sessionInfo.DisplayName and 
        // sessionInfo.ControllerDisplayName strings. 
        // Save a reference to this RemoteSystemSessionInfo to use when the
        // user selects this session from the UI
        //...
    };
    
    // Begin watching
    sessionWatcher.Start();
}
```

Wenn eine **[RemoteSystemSessionInfo](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)**-Instanz abgerufen wurde, kann damit eine Beitrittsanfrage an das Gerät ausgestellt werden, das die entsprechende Sitzung steuert. Eine akzeptierte Beitrittsanfrage gibt asynchron ein **[RemoteSystemSessionJoinResult](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)**-Objekt zurück, das einen Verweis auf die verbundene Sitzung enthält.

```csharp
public async void JoinSession(RemoteSystemSessionInfo sessionInfo) {

    // issue a join request and wait for result.
    RemoteSystemSessionJoinResult joinResult = await sessionInfo.JoinAsync();
    if (joinResult.Status == RemoteSystemSessionJoinStatus.Success) {
        // Join request was approved

        // RemoteSystemSession instance "currentSession" was declared at class level.
        // Assign the value obtained from the join result.
        currentSession = joinResult.Session;
        
        // note connection and register to handle disconnection event
        bool isConnected = true;
        currentSession.Disconnected += async (sender, args) => {
            isConnected = false;

            // update the UI with args.Reason value
        };
        
        if (isConnected) {
            // optionally use the session here (see next section)
            //...
        }
    } else {
        // Join was unsuccessful.
        // Update the UI, using joinResult.Status value to show cause of failure.
    }
}
```

Ein Gerät kann mehreren Sitzungen gleichzeitig angehören. Aus diesem Grund ist es wünschenswert, die Beitrittsfunktion von der tatsächlichen Interaktion mit jeder Sitzung zu trennen. So lange ein Verweis auf die **[RemoteSystemSession](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession)**-Instanz in der App besteht, kann die Kommunikation über diese Sitzung versucht werden.

## <a name="share-messages-and-data-through-a-remote-session"></a>Teilen von Nachrichten und Daten über eine Remotesitzung

### <a name="receive-messages"></a>Empfangen von Nachrichten

Sie können Nachrichten und Daten mit anderen teilnehmenden Geräten in der Sitzung mithilfe einer **[RemoteSystemSessionMessageChannel](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)**-Instanz austauschen, den einzigen Kommunikationskanal für die ganze Sitzung. Sobald dieser initialisiert wird, wartet er auf eingehende Nachrichten.

>[!NOTE]
>Nachrichten müssen beim Senden und Empfangen aus Bytearrays serialisiert und deserialisiert werden. Diese Funktion ist in den folgenden Beispielen enthalten, sie kann jedoch separat für eine bessere Code-Modularität implementiert werden. Ein Beispiel dafür finden Sie in der [Beispiel-App](https://github.com/microsoft/Windows-appsample-remote-system-sessions).

```csharp
public async void StartReceivingMessages() {
    
    // Initialize. The channel name must be known by all participant devices 
    // that will communicate over it.
    RemoteSystemSessionMessageChannel messageChannel = new RemoteSystemSessionMessageChannel(currentSession, 
        "Everyone in Bob's Minecraft game", 
        RemoteSystemSessionMessageChannelReliability.Reliable);
    
    // write the handler for incoming messages on this channel
    messageChannel.ValueSetReceived += async (sender, args) => {
        
        // Update UI: a message was received from the participant args.Sender
        
        // Deserialize the message 
        // (this app must know what key to use and what object type the value is expected to be)
        ValueSet receivedMessage = args.Message;
        object rawData = receivedMessage["appKey"]);
        object value = new ExpectedType(); // this must be whatever type is expected

        using (var stream = new MemoryStream((byte[])rawData)) {
            value = new DataContractJsonSerializer(value.GetType()).ReadObject(stream);
        }
        
        // do something with the "value" object
        //...
    };
}
```

### <a name="send-messages"></a>Senden von Nachrichten

Wenn der Kanal eingerichtet ist, ist das Senden einer Nachricht an alle Sitzungsteilnehmer einfach.

```csharp
public async void SendMessageToAllParticipantsAsync(RemoteSystemSessionMessageChannel messageChannel, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }
    
    // Send message to all participants. Ordering is not guaranteed.
    await messageChannel.BroadcastValueSetAsync(message);
}
```

Um eine Nachricht nur an bestimmte Teilnehmer zu senden, müssen Sie zunächst einen Ermittlungsprozess starten, um Verweise auf die Remotesysteme zu erhalten, die an der Sitzung teilnehmen. Dies ist vergleichbar mit dem Prozess der Ermittlung von Remotesystemen außerhalb einer Sitzung. Verwenden Sie die **[RemoteSystemSessionParticipantWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)**-Instanz, um das Gerät des Teilnehmers an einer Sitzung zu finden

```csharp
public void WatchForParticipants() {
    // "currentSession" is a reference to a RemoteSystemSession.
    RemoteSystemSessionParticipantWatcher watcher = currentSession.CreateParticipantWatcher();

    watcher.Added += (sender, participant) => {
        // save a reference to "participant"
        // optionally update UI
    };   

    watcher.Removed += (sender, participant) => {
        // remove reference to "participant"
        // optionally update UI
    };

    watcher.EnumerationCompleted += (sender, args) => {
        // Apps can delay data model render up until this point if they wish.
    };

    // Begin watching for session participants
    watcher.Start();
}
```

Beim Abrufen der Verweisliste der Sitzungsteilnehmer können Sie eine Nachricht an alle senden.

Um eine Nachricht an einen einzelnen Teilnehmer zu senden (im Idealfall wird dieser auf dem Bildschirm vom Benutzer ausgewählt), übergeben Sie einfach den Verweis an eine Methode, die folgendermaßen aussieht:

```csharp
public async void SendMessageToParticipantAsync(RemoteSystemSessionMessageChannel messageChannel, RemoteSystemSessionParticipant participant, object value) {
    
    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to the participant
    await messageChannel.SendValueSetAsync(message,participant);
}
```

Um eine Nachricht an mehrere Teilnehmer zu senden (im Idealfall wird dieser auf dem Bildschirm vom Benutzer ausgewählt), fügen Sie diese einem Listenobjekt hinzu und übergeben Sie die Liste an eine Methode, die folgendermaßen aussieht:

```csharp
public async void SendMessageToListAsync(RemoteSystemSessionMessageChannel messageChannel, IReadOnlyList<RemoteSystemSessionParticipant> myTeam, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to specific participants. Ordering is not guaranteed.
    await messageChannel.SendValueSetToParticipantsAsync(message, myTeam);   
}
```

## <a name="related-topics"></a>Verwandte Themen
* [Verbundene apps und Geräten (Projekt "ROME")](connected-apps-and-devices.md)
* [Remote-Systemen-API-Referenz](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
