---
title: Verbinden von Geräten über Remotesitzungen
description: Ermöglichen Sie die gemeinsame Nutzung auf verbundenen Geräten, indem Sie diese in einer Remotesitzung vereinen.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.date: 06/28/2017
ms.topic: article
keywords: Windows 10, UWP, verbundene Geräte, Remote Systeme, Rom, Project Rom
ms.localizationpriority: medium
ms.openlocfilehash: f88f44d26c3a6f4971422074e855ffca53935c7f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164824"
---
# <a name="connect-devices-through-remote-sessions"></a>Verbinden von Geräten über Remotesitzungen

Mit dem Feature "Remote Sitzungen" kann eine APP über eine Sitzung eine Verbindung mit anderen Geräten herstellen, entweder für explizites App-Messaging oder für den Broker Austausch von System verwalteten Daten, wie z. b. **[spatialentitystore](/uwp/api/windows.perception.spatial.spatialentitystore)** für die holografische Freigabe zwischen Windows Holographic-Geräten.

Remote Sitzungen können von jedem beliebigen Windows-Gerät erstellt werden, und ein beliebiges Windows-Gerät kann einen Join anfordern (obwohl Sitzungen nur über eine nur-Einladung-Sichtbarkeit verfügen können), einschließlich der von anderen Benutzern angemeldeten Geräte. Dieser Leitfaden enthält grundlegende Beispielcodes für alle wichtigen Szenarien, in denen Remote Sitzungen verwendet werden. Dieser Code kann in ein vorhandenes App-Projekt integriert und nach Bedarf geändert werden. Eine End-to-End-Implementierung finden Sie in der [Beispiel-App für Quiz Spiele](https://github.com/microsoft/Windows-appsample-remote-system-sessions)).

## <a name="preliminary-setup"></a>Vorläufige Einrichtung

### <a name="add-the-remotesystem-capability"></a>Hinzufügen der Funktionen „remoteSystem“

Damit Ihre App eine App auf einem Remotegerät starten kann, müssen Sie Ihrem App-Paketmanifest die Funktion `remoteSystem` hinzufügen. Sie können den Paket Manifest-Designer verwenden, um ihn hinzuzufügen, indem Sie auf der Registerkarte " **Funktionen** " **Remote System** auswählen, oder Sie können die folgende Zeile manuell der Datei " _Package. appxmanifest_ " des Projekts hinzufügen.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>Benutzer übergreifende Ermittlung auf dem Gerät aktivieren
Remote Sitzungen sind darauf ausgerichtet, mehrere verschiedene Benutzer zu verbinden, sodass für die beteiligten Geräte die Benutzer übergreifende Freigabe aktiviert werden muss. Dies ist eine Systemeinstellung, die mit einer statischen Methode in der **Remotesystem** -Klasse abgefragt werden kann:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

Um diese Einstellung zu ändern, muss der Benutzer die app " **Einstellungen** " öffnen. Im Menü für die gemeinsame Nutzung von **System**Freigaben  >  **Shared experiences**  >  **über Geräte** gibt es eine Dropdown Liste, in der der Benutzer angeben kann, mit welchen Geräten das System gemeinsam genutzt werden kann.

![Seite mit Einstellungen für freigegebene Erfahrungen](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>Erforderliche Namespaces einbeziehen
Um alle Code Ausschnitte in dieser Anleitung verwenden zu können, benötigen Sie die folgenden `using` Anweisungen in der Klassendatei (en).

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>Erstellen einer Remote Sitzung

Um eine Remote Sitzungs Instanz zu erstellen, müssen Sie mit einem **[remotesystemsessioncontroller](/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** -Objekt beginnen. Verwenden Sie das folgende Framework, um eine neue Sitzung zu erstellen und Joinanforderungen von anderen Geräten zu behandeln.

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

### <a name="make-a-remote-session-invite-only"></a>Nur Einladung für eine Remote Sitzung erstellen

Wenn Sie verhindern möchten, dass Ihre Remote Sitzung öffentlich erkennbar ist, können Sie Sie nur einladen. Nur die Geräte, die eine Einladung empfangen, können Joinanforderungen senden. 

Die Prozedur ist größtenteils mit der oben genannten identisch, aber wenn Sie die **[remotesystemsessioncontroller](/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** -Instanz erstellen, übergeben Sie ein konfiguriertes **[remotesystemsessionoptions](/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** -Objekt.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

Zum Senden einer Einladung benötigen Sie einen Verweis auf das empfangende Remote System (über die normale Remote System Ermittlung abgerufen). Übergeben Sie diesen Verweis einfach an die **[sendinvitationasync](/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** -Methode des Sitzungs Objekts. Alle Teilnehmer in einer Sitzung verfügen über einen Verweis auf die Remote Sitzung (siehe nächster Abschnitt), sodass jeder Teilnehmer eine Einladung senden kann.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>Entdecken und beitreten zu einer Remote Sitzung

Der Prozess der Ermittlung von Remote Sitzungen wird von der **[remotesystemsessionwatcher](/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** -Klasse behandelt und ähnelt der Ermittlung einzelner Remote Systeme.

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

Wenn eine **[remotesystemsessioninfo](/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** -Instanz abgerufen wird, kann Sie verwendet werden, um eine joinanforderung an das Gerät auszugeben, das die entsprechende Sitzung steuert. Eine akzeptierte joinanforderung gibt asynchron ein **[remotesystemsessionjoinresult](/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** -Objekt zurück, das einen Verweis auf die verknüpfte Sitzung enthält.

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

Ein Gerät kann gleichzeitig mit mehreren Sitzungen verknüpft werden. Aus diesem Grund kann es wünschenswert sein, die Verbindungsfunktionen von der eigentlichen Interaktion mit den einzelnen Sitzungen zu trennen. Solange ein Verweis auf die **[remotesystemsession](/uwp/api/windows.system.remotesystems.remotesystemsession)** -Instanz in der APP verwaltet wird, kann die Kommunikation über diese Sitzung versucht werden.

## <a name="share-messages-and-data-through-a-remote-session"></a>Freigeben von Nachrichten und Daten über eine Remote Sitzung

### <a name="receive-messages"></a>Empfangen von Nachrichten

Sie können Nachrichten und Daten mit anderen Teilnehmer Geräten in der Sitzung austauschen, indem Sie eine **[remotesystemsessionmessagechannel](/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)** -Instanz verwenden, die einen einzelnen Sitzungs weiten Kommunikationskanal darstellt. Sobald Sie initialisiert ist, beginnt Sie mit dem lauschen auf eingehende Nachrichten.

>[!NOTE]
>Nachrichten müssen nach dem Senden und empfangen serialisiert und aus Byte Arrays deserialisiert werden. Diese Funktion ist in den folgenden Beispielen enthalten, kann aber separat implementiert werden, um eine bessere Code Modularität zu erzielen. Ein Beispiel hierfür finden Sie in der Beispiel- [App](https://github.com/microsoft/Windows-appsample-remote-system-sessions)).

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

Wenn der Kanal eingerichtet wird, ist das Senden einer Nachricht an alle Sitzungsteilnehmer unkompliziert.

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

Damit eine Nachricht nur an bestimmte Teilnehmer gesendet werden kann, müssen Sie zunächst einen Ermittlungsprozess initiieren, um Verweise auf die Remote Systeme zu erhalten, die an der Sitzung teilnehmen. Dies ist vergleichbar mit dem Prozess der Ermittlung von Remote Systemen außerhalb einer Sitzung. Verwenden Sie eine **[remotesystemsessionparticipantwatcher](/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** -Instanz, um die Teilnehmer Geräte einer Sitzung zu suchen.

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

Wenn eine Liste der Verweise auf Sitzungsteilnehmer abgerufen wird, können Sie eine Nachricht an eine beliebige Gruppe von Ihnen senden.

Zum Senden einer Nachricht an einen einzelnen Teilnehmer (idealerweise vom Benutzer auf dem Bildschirm ausgewählt) übergeben Sie einfach den Verweis an eine Methode wie die folgende.

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

Um eine Nachricht an mehrere Teilnehmer zu senden (idealerweise vom Benutzer auf dem Bildschirm ausgewählt), fügen Sie Sie einem Listen Objekt hinzu, und übergeben Sie die Liste an eine Methode wie die folgende.

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

## <a name="related-topics"></a>Zugehörige Themen
* [Verbundene Apps und Geräte (Projekt „Rome”)](connected-apps-and-devices.md)
* [API-Referenz für Remotesysteme](/uwp/api/Windows.System.RemoteSystems)