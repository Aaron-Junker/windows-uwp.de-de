---
title: Verwenden Sie Spiele Chat 2 WinRT Projektionen
description: Erfahren Sie, wie Xbox Live-Game-Chat 2 mit WinRT-Projektionen zu verwenden, um Ihr Spiel Sprachkommunikation hinzugefügt.
ms.date: 04/11/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Spiele Chat 2, Spiele Chat, Sprachkommunikation
ms.localizationpriority: medium
ms.openlocfilehash: 1cb0578151d4262d61f5fbc078bebab721fb3bfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639165"
---
# <a name="using-game-chat-2-winrt-projections"></a>Spiele-Chat-2 (Projektionen WinRT) verwenden

Dies ist eine kurze exemplarische Vorgehensweise zur Verwendung der Game-Chat 2 C# API. Spieleentwickler Spiel, Chat 2 über C++ zugreifen möchten, sollten finden Sie unter [mithilfe von Spiel-Chat 2](using-game-chat-2.md).

## <a name="prerequisites-a-nameprereq"></a>Erforderliche Komponenten <a name="prereq">

Um das Spiel, Chat 2 nutzen zu können, müssen Sie enthalten die [Microsoft.Xbox.Services.GameChat2 Nuget-Paket](https://www.nuget.org/packages/Microsoft.Xbox.Game.Chat.2.WinRT.UWP/).

Bevor Sie die Codierung mit Spiel, Chat 2 beginnen, müssen Sie Ihrer app AppXManifest um deklarieren die Gerätefunktion "Mikrofon" konfiguriert haben. AppXManifest-Funktionen werden in den jeweiligen Abschnitten der Plattformdokumentation ausführlicher beschrieben; Der folgende Codeausschnitt zeigt der Knoten "Mikrofon" Gerät-Funktion, der unter dem Knoten Paket/Funktionen vorhanden sind, sollten dies sonst bei Chat blockiert werden:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="initialization-a-nameinit"></a>Initialisierung <a name="init">

Sie beginnen mit der Bibliothek interagieren, durch die Instanziierung einer `GameChat2ChatManager` Objekt mit der maximalen Anzahl von gleichzeitigen Chat-Benutzern erwartet wird, die Instanz hinzugefügt werden. Zum Ändern dieses Werts, der `GameChat2ChatManager` Objekt muss gelöscht und neu erstellt, mit dem gewünschten Wert. Sie möglicherweise nur `GameChat2ChatManager` zu einem Zeitpunkt instanziiert.

```cs
GameChat2ChatManager myGameChat2ChatManager = new GameChat2ChatManager(<maxChatUserCount>);
```

Die `GameChat2ChatManager` Objekt verfügt über zusätzliche, optionale Eigenschaften, die jederzeit in konfiguriert werden können. Im folgenden Codebeispiel wird davon ausgegangen, dass die Variablen ein Wert zugewiesen werden werden, bevor sie, zum Festlegen der Eigenschaften verwendet werden auf der `myGameChat2ChatManager` Objekt.

```cs
GameChat2SpeechToTextConversionMode mySpeechToTextConversionMode;
GameChat2SharedDeviceCommunicationRelationshipResolutionMode mySharedDeviceResolutionMode;
GameChat2CommunicationRelationship myDefaultLocalToRemoteCommunicationRelationship;
float myDefaultAudioRenderVolume;
GameChat2AudioEncodingTypeAndBitrate myAudioEncodingTypeAndBitRate;

...

myGameChat2ChatManager.SpeechToTextConversionMode = mySpeechToTextConversionMode;
myGameChat2ChatManager.SharedDeviceResolutionMode = mySharedDeviceResolutionMode;
myGameChat2ChatManager.DefaultLocalToRemoteCommunicationRelationship = myDefaultLocalToRemoteCommunicationRelationship;
myGameChat2ChatManager.DefaultAudioRenderVolume = myDefaultAudioRenderVolume;
myGameChat2ChatManager.AudioEncodingTypeAndBitRate = myAudioEncodingTypeAndBitRate;
```

## <a name="configuring-users-a-nameconfig"></a>Konfigurieren von Benutzern <a name="config">

Sobald die Instanz initialisiert wird, müssen Sie die lokalen Benutzer mit der GC2-Instanz hinzufügen. In diesem Beispiel stellt der Benutzer ein lokalen Benutzer dar.

```cs
string userAXuid;

...

IGameChat2ChatUser userA = myGameChat2ChatManager.AddLocalUser(userAXuid);
```

Fügen Sie anschließend die remote-Benutzer und Identifers, die zum Darstellen von remote "Endpunkte" verwendet werden, die jeden Remotebenutzer auf. "Endpunkte" werden durch die Objekte im Besitz der app, die implementieren dargestellt die `IGameChat2Endpoint` Schnittstelle. Das folgende Beispiel zeigt eine beispielimplementierung der `MyEndpoint` Klasse, die implementiert die `IGameChat2Endpoint`.

```cs
class MyEndpoint : IGameChat2Endpoint
{
    private uint endpointIdentifier;
    public MyEndpoint(uint identifier)
    {
        endpointIdentifier = identifier;
    }

    // Implementing IsSameEndpointAs, the only method on the IGameChat2Endpoint interface
    public bool IsSameEndpointAs(IGameChat2Endpoint gameChat2Endpoint)
    {
        return endpointIdentifier == ((MyEndpoint)gameChat2Endpoint).endpointIdentifier;
    }
}
```

Das folgende Codebeispiel zeigt sind Benutzer B, C und D Remotebenutzer, die hinzugefügt wird. Benutzer B auf einem Remotegerät ist und Benutzer C und D auf einem anderen remote-Gerät. In diesem Beispiel wird davon ausgegangen, dass die Variablen festgelegt werden und dass `myGameChat2ChatManager` ist eine Instanz der `GameChat2ChatManager` "MyEndpoint" auf eine Klasse ist, implementiert und `IGameChat2Endpoint`.

```cs
string userBXuid;
string userCXuid;
string userDXuid;
MyEndpoint myRemoteEndpointOne;
MyEndpoint myRemoteEndpointTwo;

...

IGameChat2ChatUser remoteUserB = myGameChat2ChatManager.AddRemoteUser(userBXuid, myRemoteEndpointOne);
IGameChat2ChatUser remoteUserC = myGameChat2ChatManager.AddRemoteUser(userCXuid, myRemoteEndpointTwo);
IGameChat2ChatUser remoteUserD = myGameChat2ChatManager.AddRemoteUser(userDXuid, myRemoteEndpointTwo);
```

Nachdem Sie die kommunikationsbeziehung zwischen jeden Remotebenutzer und dem lokalen Benutzer konfigurieren. In diesem Beispiel nehmen wir an, dass Benutzer A und Benutzer B befinden sich auf das gleiche Team und die bidirektionale Kommunikation zulässig ist. `GameChat2CommunicationRelationship.SendAndReceiveAll` wird definiert, um die bidirektionale Kommunikation darstellen. Festlegen des benutzerbeziehung an Benutzer B mit:

```cs
GameChat2ChatUserLocal localUserA = userA as GameChat2ChatUserLocal;
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

Nehmen wir nun an, dass Benutzer C und D "Zuschauer" und die Berechtigung für Benutzer A anhören, aber nicht sprechen. `GameChat2CommunicationRelationship.ReceiveAll` wird definiert diese unidirektionale Kommunikation. Legen Sie die Beziehungen mit:

```cs
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.ReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.ReceiveAll);
```

Wenn zu einem beliebigen Zeitpunkt stehen Remotebenutzer, die hinzugefügt wurden die `GameChat2ChatManager` -Instanz, aber noch nicht konfiguriert wurde, für die Kommunikation mit den lokalen Benutzern – das ist kein Problem! Dies kann in Szenarien erwartet werden, in dem Benutzer werden Teams bestimmen oder gesehen Kanäle können beliebig ändern. Spiel, Chat 2 werden nur Informationen (z. B. Beziehungen für Datenschutz und die Zuverlässigkeit der) für Benutzer zwischengespeichert werden, die auf die Instanz, daher ist es nützlich, um das Spiel Chat 2 alle möglichen Benutzer zu informieren, auch wenn für alle lokalen Benutzer zu einem bestimmten Zeitpunkt sprechen kann nicht hinzugefügt wurden.

Schließlich nehmen Sie an, dass Benutzer D das Spiel verlassen hat und aus der lokalen Spiel, Chat-2-Instanz entfernt werden sollen. Kann mit dem folgenden Aufruf erfolgen:

```cs
remoteUserD.Remove();
```

Das Benutzerobjekt wird sofort ungültig Wenn `Remove()` aufgerufen wird. Der letzte Zustand des Benutzers wird zwischengespeichert, wenn es entfernt wird. Wird aufgerufen, nachdem das User-Objekt für ungültig erklärt wird nur zu Informationszwecken Methoden werden der Status eines Benutzers wider, bevor es entfernt wurde. Andere Methoden lösen einen Fehler beim Aufruf.

## <a name="processing-data-frames-a-namedata"></a>Verarbeiten von Datenrahmen. <a name="data">

Spiel, Chat 2 besitzt keine eigene Transportschicht; Dies muss von der app angegeben werden. Dieses Plug-in wird von der app reguläre und häufige Aufrufe verwaltet die `GameChat2ChatManager.GetDataFrames()`. Diese Methode ist, wie Spiel, Chat 2 für ausgehende Daten an die app bereitstellt. Es soll schnell ausgeführt werden, dass es auf einem dedizierten Thread für die Netzwerke häufig abgefragt werden kann. Dies bietet eine bequeme Möglichkeit zum Abrufen von Daten für alle in der Warteschlange, ohne sich Gedanken Netzwerk zeitliche Steuerung oder die Komplexität von Multithread-Rückruf nicht vorhersagbar sind.

Wenn `GameChat2ChatManager.GetDataFrames()` aufgerufen wird, alle Daten in der Warteschlange werden gemeldet, in einer Liste von `IGameChat2DataFrame` Objekte. Apps sollten Durchlaufen der Liste aus, überprüfen Sie die `TargetEndpointIdentifiers`, und verwenden Sie Netzwerkschicht der app, um die Daten an die entsprechenden remote-app-Instanzen zu übermitteln. In diesem Beispiel `HandleOutgoingDataFrame` ist eine Funktion, die die Daten, in sendet der `Buffer` für jeden Benutzer auf jeder "Endpoint" angegeben, der `TargetEndpointIdentifiers` gemäß der `TransportRequirement`.

```cs
IReadOnlyList<IGameChat2DataFrame> frames = myGameChat2ChatManager.GetDataFrames();
foreach (IGameChat2DataFrame dataFrame in frames)
{
    HandleOutgoingDataFrame(
        dataFrame.Buffer,
        dataFrame.TargetEndpointIdentifiers,
        dataFrame.TransportRequirement
        );
}
```

Die Datenrahmen werden verarbeitet, desto häufiger, desto niedriger ist die audio Wartezeit für den Endbenutzer offensichtlich. Das Audio wird in 40 ms-Datenrahmen vereinigt; Dies ist der vorgeschlagene Abrufzeitraum.

## <a name="processing-state-changes-a-namestate"></a>Verarbeiten von Änderungen des Ansichtszustands <a name="state">

Spiel, Chat 2 bietet Updates für die app ein, z. B. der empfangenen SMS-Nachrichten, über die app reguläre und häufige Aufrufe der `GameChat2ChatManager.GetStateChanges()` Methode. Es wurde entwickelt, um schnell ausgeführt werden, sodass einzelbildweise Grafiken in der UI-Rendering-Schleife aufgerufen werden kann. Dadurch wird einen praktischer Ort für alle in der Warteschlange stehende Änderungen abrufen, ohne sich Gedanken Netzwerk zeitliche Steuerung oder die Komplexität von Multithread-Rückruf nicht vorhersagbar sind.

Wenn `GameChat2ChatManager.GetStateChanges()` wird aufgerufen, werden alle in der Warteschlange Updates in einer Liste von gemeldet `IGameChat2StateChange` Objekte. Apps sollten Folgendes beachten:
1. Die Liste durchlaufen
2. Überprüfen Sie die grundlegende Struktur für die spezifischeren Typ
3. Wandeln Sie die grundlegende Struktur auf den entsprechenden Typ ausführlichere
4. Behandeln Sie das Update nach Bedarf. 

```cs
IReadOnlyList<IGameChat2StateChange> stateChanges = myGameChat2ChatManager.GetStateChanges();
foreach (IGameChat2StateChange stateChange in stateChanges)
{
    switch (stateChange.Type)
    {
        case GameChat2StateChangeType.CommunicationRelationshipAdjusterChanged:
        {
            MyHandleCommunicationRelationshipAdjusterChanged(stateChange as GameChat2CommunicationRelationshipAdjusterChangedStateChange);
            break;
        }
        case GameChat2StateChangeType.TextChatReceived:
        {
            MyHandleTextChatReceived(stateChange as GameChat2TextChatReceivedStateChange);
            break;
        }

        ...
    }
}
```

## <a name="text-chat-a-nametext"></a>Text-chat <a name="text">

Um Textchat senden zu können, verwenden `GameChat2ChatUserLocal.SendChatText()`. Zum Beispiel:

```cs
localUserA.SendChatText("Hello");
```

Spiel, Chat 2 generiert einen Datenrahmen, der diese Nachricht enthält. die Zielendpunkte für den Datenrahmen werden die Anforderungen für Benutzer, die zum Empfangen von Text von den lokalen Benutzer konfiguriert wurden. Wenn die Daten von der entfernten Endpunkten verarbeitet werden, die Nachricht zur Verfügung gestellt über eine `GameChat2TextChatReceivedStateChange`. Wie bei Voice Chat, werden die Einschränkungen für Berechtigungen und Datenschutz für Textchat eingehalten. Wenn ein Paar der Benutzer Textchat zulassen konfiguriert wurden, aber die Rechte oder des Datenschutzes Einschränkungen nicht, dass die Kommunikation zulassen, wird die Nachricht gelöscht werden.

Unterstützung der Texteingabe-Chat und Anzeige ist erforderlich, für den Zugriff auf (finden Sie unter [Barrierefreiheit](#access) Weitere Details).

## <a name="accessibility-a-nameaccess"></a>Barrierefreiheit <a name="access">

Unterstützung für Text-Chat-Eingabe und Anzeige ist erforderlich. Texteingabe ist erforderlich, da, sogar auf Plattformen oder Spiele Genres, die weit verbreitete physische Tastatur verwenden, in der Vergangenheit aufwiesen, Benutzer das System, um die Sprachwiedergabe von Text hilfstechnologien verwenden konfigurieren können. Anzeigen von Text ist ebenso erforderlich, weil Benutzer konfigurieren, dass das System, um die Sprache-zu-Text verwenden können. Diese Einstellungen können auf lokale Benutzer erkannt werden, indem Sie überprüfen die `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` und `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` Eigenschaften, und Sie möchten die Text-Mechanismen bedingt zu aktivieren.

### <a name="text-to-speech"></a>Text-zu-Sprache

Wenn ein Benutzer aktiviert, Speech hat `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` wird "true" sein. Wenn dieser Zustand erkannt wird, muss die app eine Methode der Texteingabe bereitzustellen. Nachdem Sie die Texteingabe, die von einem physischen oder virtuellen Tastatur bereitgestellt haben, übergeben Sie die Zeichenfolge, die die `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` Methode. Spiel, Chat 2 erkennt und basierte auf der Zeichenfolge und die Einstellungen der Benutzer zugegriffen werden kann, Voice Audiodaten zu synthetisieren. Zum Beispiel:

```cs
localUserA.SynthesizeTextToSpeech("Hello");
```

Das Audio synthetisiert, die im Rahmen dieses Vorgangs wird für alle Benutzer, die zum Empfangen von Audiodaten von dieses lokalen Benutzers konfiguriert wurden, übertragen werden. Wenn `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` wird aufgerufen, für einen Benutzer, die keine Sprachwiedergabe von Text aktiviert Spiel, Chat 2 ist keine weitere Aktion durchführen.

### <a name="speech-to-text"></a>Sprache-zu-text

Wenn ein Benutzer die Sprache-zu-Text-aktiviert ist, hat `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` gilt. Wenn dieser Zustand erkannt wird, muss die app angeben, dass nach Möglichkeit transkribierte Chatnachrichten Benutzeroberfläche zugeordnete vorbereitet werden. GC automatisch jeden Remotebenutzer Audio transkribieren und machen Sie es über eine `GameChat2TranscribedChatReceivedStateChange`.

### <a name="speech-to-text-performance-considerations"></a>Überlegungen zur Leistung von Sprache-zu-text

Wenn Sprache-zu-Text aktiviert ist, initiiert die Game-Chat-2-Instanz auf jedem Gerät remote eine Web-Socket-Verbindung mit dem Endpunkt der spracherkennungs-Dienste. Jedem Spiel, Chat-2-Remoteclient lädt Audio für die Spracherkennung Services-Endpunkte über diesem Websocket; die spracherkennungs-Dienst-Endpunkte gibt gelegentlich eine Aufzeichnung-Meldung zurück, mit dem Remotegerät. Das Remotegerät sendet dann die Aufzeichnung-Nachricht (d. h. eine Textnachricht) auf dem lokalen Gerät, in dem die Nachricht nach Möglichkeit transkribierte Spiel, Chat 2 an die app erhält zum Rendern.

Daher ist die primäre Leistungseinbußen von Sprache-zu-Text-Netzwerkverwendung an. Der Netzwerkdatenverkehr wird das codierte Audio hochladen. Der Websocket hochlädt, Audio, die bereits im Pfad "normal" Voice Chat Spiel Chat 2 codiert wurde; die app verfügt über die Kontrolle über die Bitrate über `GameChat2ChatManager.AudioEncodingTypeAndBitrate`.

## <a name="ui-a-nameui"></a>UI <a name="UI">

Es wird empfohlen, dass an einer beliebigen Stelle Player, besonders in einer Liste von Gamertags wie z. B. eine spielstandsanzeige, angezeigt werden, dass auch stumm geschaltet/Vorträge Symbole als Feedback für den Benutzer angezeigt werden. Die `IGameChat2ChatUser.ChatIndicator` Eigenschaft stellt den Status aktuellen, sofortigen Chat dar, für die der Spieler. Das folgende Beispiel zeigt das Abrufen der Indikatorwert für den ein `IGameChat2ChatUser` Objekt verweist die Variable "UserA", um zu bestimmen, einen bestimmtes Symbol Konstantenwert zuweisen zu einer Variablen "IconToShow":

```cs
switch (userA.ChatIndicator)
{
    case GameChat2UserChatIndicator.Silent:
    {
        iconToShow = Icon_InactiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.Talking:
    {
        iconToShow = Icon_ActiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.LocalMicrophoneMuted:
    {
        iconToShow = Icon_MutedSpeaker;
        break;
    }
    
    ...
}
```

Der Wert des `IGameChat2ChatUser.ChatIndicator` wird erwartet, dass Sie häufig als Player starten und beenden zu sprechen, z. B. ändern. Es soll Abrufen jedes UI-Frames daher-apps zu unterstützen.

## <a name="muting-a-namemute"></a>Stummschaltung <a name="mute">

Die `GameChat2ChatUserLocal.MicrophoneMuted` Eigenschaft kann verwendet werden, um den Status eines lokalen Benutzers Mikrofon stumm schalten auszuschalten. Wenn das Mikrofon stumm geschaltet sind, werden ohne Audio, Mikrofon, erfasst. Wenn der Benutzer auf ein freigegebenes Gerät, z. B. Kinect, gilt der zum Stummschalten Status für alle Benutzer. Diese Methode wird ein Hardware-Steuerelement zum Stummschalten (z. B. eine Schaltfläche auf des Benutzers Kopfhörer) nicht angezeigt. Es gibt keine Methode zum Abrufen von den Zustand der Hardware zum Stummschalten der audio-Gerät eines Benutzers über die Spiel-Chat-2.

Die `GameChat2ChatUserLocal.SetRemoteUserMuted()` Methode kann verwendet werden, um den Status eines Remotebenutzers in Bezug auf einen bestimmten lokalen Benutzer zum Stummschalten auszuschalten. Wenn der Remotebenutzer stumm geschaltet wird, nicht des lokalen Benutzers Audio hören oder vom Remotebenutzer entgegen SMS empfangen.

## <a name="bad-reputation-auto-mute-a-nameautomute"></a>In dem Ruf Auto-Stummschalten <a name="automute">

In der Regel werden Remotebenutzern unmuted beginnen. Spiel, Chat 2 startet die Benutzer in einem Zustand stumm geschaltet sind, wenn (1) der Remotebenutzer nicht Freunde mit dem lokalen Benutzer und (2) remote User has ein Ruf-Flag kann. Wenn Benutzer aufgrund von diesem Vorgang stumm geschaltet sind `IGameChat2ChatUser.ChatIndicator` zurück `GameChat2UserChatIndicator.ReputationRestricted`. Dieser Zustand wird von der erste Aufruf überschrieben `GameChat2ChatUserLocal.SetRemoteUserMuted()` , die den entfernten Benutzer als dem Zielbenutzer enthält.

## <a name="privilege-and-privacy-a-namepriv"></a>Berechtigungen und Datenschutz <a name="priv">

Zusätzlich zu den kommunikationsbeziehung konfiguriert, indem das Spiel erzwingt Spiel, Chat 2 Einschränkungen für Berechtigungen und Datenschutz. Spiel, Chat 2 führt Berechtigungen und Datenschutz Einschränkung Suchvorgänge aus, wenn ein Benutzer zuerst hinzugefügt wird; des Benutzers `IGameChat2ChatUser.ChatIndicator` gibt stets `GameChat2UserChatIndicator.Silent` bis zum Abschluss dieser Vorgänge. Wenn eine Rechte oder des Datenschutzes geräteeinschränkungen, die Benutzer die Kommunikation mit einem Benutzer betroffen ist `IGameChat2ChatUser.ChatIndicator` zurück `GameChat2UserChatIndicator.PlatformRestricted`. Kommunikation von plattformbeschränkungen gelten für sowohl Text als auch Voice Chat. Es werden nie eine Instanz, in denen Textchat wird durch eine plattformeinschränkung blockiert Voice Chat ist jedoch nicht, oder umgekehrt.

`GameChat2ChatUserLocal.GetEffectiveCommunicationRelationship()` kann verwendet werden, um Hilfe zu unterscheiden, wenn Benutzer aufgrund von unvollständigen Berechtigungen und Datenschutz Vorgänge kommunizieren können. Gibt die vom Spiel, Chat 2 erzwungen wird, in Form von kommunikationsbeziehung `GameChat2CommunicationRelationship` und die Ursache die Beziehung entspricht möglicherweise nicht auf die konfigurierten Beziehung in Form von `GameChat2CommunicationRelationshipAdjuster`. Angenommen, die Suchvorgänge noch ausgeführt wird, werden die `GameChat2CommunicationRelationshipAdjuster` werden `GameChat2CommunicationRelationshipAdjuster.Initializing`. Diese Methode wird erwartet, bei der Entwicklung und debugging-Szenarien verwendet werden. Es sollte nicht zum beeinflussen der Benutzeroberfläche verwendet werden (siehe [UI](#UI)).

## <a name="cleanup-a-namecleanup"></a>Bereinigen <a name="cleanup">

Wenn die app nicht mehr Kommunikation über das Spiel, Chat 2 benötigt, Sie sollten Aufrufen `GameChat2ChatManager.Dispose()`. Dadurch wird ein Spiel Chat 2 Ressourcen freigeben, die zugeordnet wurden, um die Kommunikation zu verwalten.

## <a name="how-to-configure-popular-scenarios"></a>Gängige Szenarien konfigurieren

### <a name="push-to-talk"></a>Push-an-talk

Push-an-Talk mit implementiert werden sollte `GameChat2ChatUserLocal.MicrophoneMuted`. Legen Sie `MicrophoneMuted` auf "false", um Sprache zu ermöglichen und `MicrophoneMuted` auf true festlegen, um es zu beschränken. Diese Änderung der Eigenschaft stellt die niedrigste Latenz-Antwort vom Spiel, Chat 2 bereit.

### <a name="teams"></a>Teams

Nehmen wir an, dass Benutzer A und Benutzer B auf Blue Team, und Benutzer C und D für Benutzer, die auf Team Rot sind. Jeder Benutzer ist in der app eine eindeutige Instanz.

Für Benutzer A-Gerät:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

Auf Benutzer B-Gerät:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

Auf den Benutzer C-Gerät:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

Auf die Benutzer-D-Gerät:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

### <a name="broadcast"></a>Übertragung

Nehmen wir an, dass Benutzer A ist das führende Aufträge erteilen und Benutzer B, C und D nur überwachen können. Jeder Spieler befindet sich auf eine eindeutige Geräte.

Für Benutzer A-Gerät:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAll);
```

Auf Benutzer B-Gerät:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

Auf den Benutzer C-Gerät:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

Auf die Benutzer-D-Gerät:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
```
