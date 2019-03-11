---
title: Spiele-Chat-2-Migration
description: Erfahren Sie, wie vorhandener Spiel-Chat-Code zum Spiel, Chat 2 verwenden migriert.
ms.date: 05/02/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Spiele Chat 2, Spiele Chat, Sprachkommunikation
ms.localizationpriority: medium
ms.openlocfilehash: e963210091694a07114f10d5a3dc531a353621df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653585"
---
# <a name="migration-from-game-chat-to-game-chat-2"></a>Migration von Spielen Chat zu spielen Chat 2

In diesem Dokument werden die ähnlichkeiten zwischen Spiel Chat und Spiel, Chat 2 und Migrieren von Spiel-Chat zu Spiel, Chat 2. Daher ist es für Titel, die eine vorhandene Spiel Chat-Implementierung verfügen, das Spiel, Chat 2 migrieren möchten. Wenn Sie bereits über eine Spiel-Chat-Implementierung haben, ist der empfohlene Ausgangspunkt [mithilfe von Spiel-Chat 2](using-game-chat-2.md). 

## <a name="preface"></a>Einleitung

Die ursprüngliche Spiel Chat-API ist eine WinRT-API, die ein Konzept der Chat-Benutzer und Voice-Kanäle unterstützen die Implementierung von in-Game-Voice Chat-Szenarien Xbox Live verfügbar gemacht werden. Der Game-Chat-API basiert auf der Chat-API, die wiederum eine WinRT-API, die ein Konzept der Chat-Benutzer und Voice-Kanäle verfügbar gemacht werden, während eine Low-Level-Verwaltung von Audiogeräten erfordern wird. Spiele-Chat-2 ist der Nachfolger von der ursprünglichen Spiel, Chat und Chat-APIs: Es wurde entworfen, um eine einfachere API für die grundlegende Chat-Szenarien, z. B. Teamkommunikation, während gleichzeitig bietet damit mehr Flexibilität für die erweiterte Chat-Szenarien, z. B. Broadcast-ähnlichen werden die Kommunikation und Echtzeit-audio-Bearbeitung. Sowohl Spiel, Chat und Spiele Chat-2 Geben Sie die gleichen Nische: die APIs stellen eine praktische Methode für die Integration von Xbox Live aktiviert, in-Game-Voice Chat, während der Titel eine Transportschicht bietet, für die Übertragung von Datenpaketen zu und von Remoteinstanzen von Spiel, Chat oder spielen Chatten Sie 2 ein.

Das Spiel Chat-2-API hat viele Vorteile gegenüber der ursprünglichen Spiel chatten und Chat-APIs. Einige Highlights:
* Eine flexible benutzerorientierten-API, die nicht mit dem Modell "Kanal" eingeschränkt ist.
* Bandbreite Verbesserungen durch vermehrte paketfilterung sowohl "Datenquellen" und "Data-Empfänger.
* Eine interne Einschränkung 2 langlebige Threads mit der app konfigurierbare Affinität.
* Eine API, die in C++ und WinRT-Versionen verfügbar.
* Vereinfachte Nutzungsmodell mit einem Muster "funktionieren".
* Arbeitsspeicher-hooks zum Spiel, Chat-2-Zuordnungen mit einer benutzerdefinierten Zuweisung umleiten.
* Identische UWP + exklusive Ressource Anwendung (Zeitraum)-Header für eine praktischer plattformübergreifende Entwicklungsumgebung.

In diesem Dokument werden die ähnlichkeiten zwischen Spiel Chat und Spiel, Chat 2 und wie von Spiel-Chat zu der Game-Chat 2 C++-API zu migrieren. Wenn Sie die Migration von Chat-Spiel auf der Game-Chat 2 WinRT-API interessiert sind, wird vorgeschlagen, die Sie lesen Sie dieses Dokument Informationen zum Spiel Chat Konzepte Spiel, Chat 2 zuzuordnen, und klicken Sie dann finden Sie unter [mithilfe von Spiel-Chat 2 WinRT Projektionen](using-game-chat-2-winrt.md) für die Muster für WinRT spezifisch. Der Beispielcode für das ursprüngliche Spiel Chat in diesem Dokument verwendet C++ / CX.

## <a name="prerequisites"></a>Voraussetzungen

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

Kompilieren von Spiel, Chat 2 ist erforderlich, einschließlich des primären GameChat2.h-Headers. Um ordnungsgemäß zu verknüpfen, muss das Projekt auch GameChat2Impl.h in mindestens einer Kompilierungseinheit enthalten (Allgemeine vorkompilierten Headers wird empfohlen, da diese Funktion stubimplementierungen kurz und leicht für den Compiler an, wie "Inline" generiert werden).

Die Spiel, Chat-2-Schnittstelle erfordert kein Projekt auswählen, die zwischen dem Kompilieren mit C++ / CX im Vergleich zu herkömmlichen C++; Es kann entweder mit verwendet werden. Die Implementierung auslösen nicht auch Ausnahmen, als Mittel zum nicht schwerwiegenden Fehler, die Berichterstattung, damit Sie es einfach aus Projekten ausnahmefreie nutzen darf, wenn bevorzugt. Die Implementierung, aber löst Ausnahmen als Mittel zum Schwerwiegender Fehler reporting (finden Sie unter [Fehler Modell](#failure-model-and-debugging) Einzelheiten).

## <a name="initialization"></a>Initialisierung

### <a name="game-chat"></a>Spiele-Chat

Interaktion mit der ursprünglichen Spiel Chat erfolgt über die `ChatManager` Klasse. Das folgende Beispiel zeigt, wie Sie erstellen eine `ChatManager` mit Standardparametern-Instanz:

```cpp
auto chatManager = ref new ChatManager();
```

### <a name="game-chat-2"></a>Game Chat 2

Alle Interaktionen mit Chat-2-Spiel erfolgt über Spiel Chat 2 `chat_manager` Singleton. Die Singletoninstanz muss initialisiert werden, bevor jede sinnvolle Interaktion mit der Bibliothek durchgeführt werden kann. Die Singletoninstanz erfordert, dass die maximale Anzahl von gleichzeitigen lokal und remote-Chat-Benutzer bei der Initialisierung angegeben werden. Dies ist da Spiel, Chat 2 Speicher, die proportional zu der erwarteten Anzahl von Benutzern bereits reserviert. Das folgende Beispiel zeigt, wie die Singleton-Instanz initialisiert werden soll, wenn die maximale Anzahl von gleichzeitigen Chat für lokale und remote Benutzer vier bestehen:

```cpp
chat_manager::singleton_instance().initialize(4);
```

Es gibt verschiedene optionale Parameter, die während der Initialisierung ebenfalls angegeben werden können, wie z. B. die Standardeinstellung rendern Volume der remote-Chat-Benutzer und gibt an, ob Spiel Chat 2 automatische Konvertierung von Sprache-zu-Text ausführen soll. Weitere Informationen zu diesen Parametern finden Sie in der Dokumentation der `chat_manager::initialize()`.

## <a name="configuring-users"></a>Konfigurieren von Benutzern

### <a name="game-chat"></a>Spiele-Chat

Hinzufügen von lokale Benutzer auf die ursprünglichen Spiel Chat-API erfolgt über `ChatManager::AddLocalUserToChatChannelAsync()`. Hinzufügen des Benutzers für mehrere Chat-Kanäle erfordert mehrere Aufrufe, jeweils einen anderen Kanal angeben. Das folgende Beispiel zeigt, wie den Benutzer mit der Xuid "MyXuid", um 0 Kanal hinzugefügt wird:

```cpp
auto asyncOperation = chatManager->AddLocalUserToChatChannelAsync(0, L"myXuid");
```

Entfernte Benutzer werden nicht direkt mit der Instanz hinzugefügt. Bei einem Remotegerät in den Titel des Netzwerks angezeigt wird, wird der Titel erstellt ein Objekt, das Remotegerät darstellt und informiert Spiel des neuen Geräts per Chat `ChatManager::HandleNewRemoteConsole()`. Der Titel muss auch eine Methode, vergleichen Sie die Objekte, die Remotegeräte Spiel sprechen darstellen, durch die Implementierung bieten `CompareUniqueConsoleIdentifiersHandler`. Das folgende Beispiel zeigt, wie dieser Delegat Spiel zu sprechen, vorausgesetzt, `Platform::String` Objekte dienen zur Darstellung von Remotegeräten und ein neues Gerät Zeichenfolge dargestellten `L"1"` wurde mit dem Titel gehörigen Netzwerk verknüpft:

```cpp
auto token = chatManager->OnCompareUniqueConsoleIdentifiers +=
    ref new CompareUniqueConsoleIdentifiersHandler(
        [this](Platform::Object^ obj1, Platform::Object^ obj2)
    {
        return (static_cast<Platform::String^>(obj1)->Equals(static_cast<Platform::String^>(obj2)));
    });

...

Platform::String^ newDeviceIdentifier = ref new Platform::String(L"1");
chatManager->HandleNewRemoteConsole(newDeviceIdentifier);
```

Sobald Spiel Chat über dieses Gerät informiert wurde, es-Pakete, die Informationen zu Benutzern, lokalen generiert und diese Pakete auf den Titel für die Übertragung an die Spiel-Chat-Instanz auf dem Remotegerät bereitstellen. Auf ähnliche Weise generiert die Spiel-Chat-Instanz auf dem Remotegerät Pakete, die die Informationen zu Benutzern, auf dem remote-Gerät, mit dem der Titel mit der lokalen Instanz von Spiel Chat übertragen wird. Wenn die lokale Instanz Pakete, die die Informationen zu neuen Remotebenutzern empfängt, werden die neuen remote-Benutzer der lokalen Instanz der Liste der Benutzer über hinzugefügt `ChatManager::GetUsers()`.

Entfernen von Benutzern aus der Game-Chat-Instanz wird ausgeführt, über ähnliche Aufrufe `ChatManager::RemoveLocalUserFromChatChannelAsync()` und `ChatManager::RemoveRemoteConsoleAsync()`

### <a name="game-chat-2"></a>Game Chat 2

Hinzufügen von lokalen Benutzern zu Spiel, Chat 2 erfolgt synchron über `chat_manager::add_local_user()`. In diesem Beispiel wird Benutzer A darstellen, einen lokalen Benutzer mit Benutzer-Id für Xbox `L"myLocalXboxUserId"`:

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(L"myLocalXboxUserId");
```

Beachten Sie, dass der Benutzer nicht hinzugefügt, um einen bestimmten Kanal - Spiel, Chat 2 ein Konzept der "kommunikationsbeziehungen" statt Kanälen verwendet, um die steuern, ob Benutzer hören und sprechen zu können. Die Methode zum Konfigurieren von kommunikationsbeziehungen wird weiter unten in diesem Abschnitt behoben.

Ähnlich wie lokale Benutzer, Remotebenutzer werden synchron im lokalen Spiel Chat 2-Instanz hinzugefügt. Remotebenutzer müssen gleichzeitig Bezeichner zugeordnet sein, die verwendet wird, auf das Remotegerät darstellen. Spiel, Chat 2 bezieht sich auf eine Instanz der Ausführung der app auf einem Remotegerät als "Endpunkt". In diesem Beispiel werden Benutzer B einen Benutzer mit Benutzer-Id für Xbox `L"remoteXboxUserId"` für einen Endpunkt, dargestellt durch die Ganzzahl `1`.

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(L"remoteXboxUserId", 1);
```

Sobald die Benutzer mit der Instanz hinzugefügt wurden, sollten Sie die "kommunikationsbeziehung" zwischen jeden Remotebenutzer und jede lokale Benutzer konfigurieren. In diesem Beispiel nehmen wir an, dass Benutzer A und Benutzer B befinden sich auf das gleiche Team und die bidirektionale Kommunikation zulässig ist. `c_communicationRelationshiSendAndReceiveAll` in GameChat2.h bidirektionale Kommunikation darstellen wird eine Konstante definiert werden. Die Beziehung kann mit konfiguriert werden:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

Schließlich nehmen Sie an, dass Benutzer B das Spiel verlassen hat und aus der lokalen Spiel, Chat-Instanz entfernt werden sollen. Die erfolgt synchron mit dem folgenden Aufruf:

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

Finden Sie unter [mithilfe von Spiel-Chat 2: Konfigurieren von Benutzern](using-game-chat-2.md#configuring-users) Ausführlichere Beispiele oder den Verweis für die einzelnen API-Methoden für Weitere Informationen.

## <a name="processing-data"></a>Datenverarbeitung

### <a name="game-chat"></a>Spiele-Chat

Spiel, Chat besitzt keine eigene Transportschicht; Dies muss von der app angegeben werden. Ausgehende Pakete erfolgt durch Abonnieren der `OnOutgoingChatPacketReady` Ereignis und überprüfen die Argumente, um die Paket-Ziel und die Transport-Anforderungen zu bestimmen. Das folgende Beispiel zeigt, wie Sie das Ereignis abonnieren und die Argumente Weiterleiten der `HandleOutgoingPacket()` Methode implementiert, die durch den Titel:

```cpp
auto token = chatManager->OnOutgoingChatPacketReady +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::ChatPacketEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::ChatPacketEventArgs^ args)
    {
        this->HandleOutgoingPacket(args);
    });
```

Eingehende Pakete werden gesendet, zum Spiel Chat über `ChatManager::ProcessingIncomingChatMessage()`. Der Puffer rohpakete und die remote-Geräte-ID müssen angegeben werden. Das folgende Beispiel zeigt, wie Sie ein Paket zu übermitteln, die in der lokalen gespeichert sind `packetBuffer` und Geräte-ID, die in der lokalen Variablen gespeichert `remoteIdentifier`.

```cpp
Platform::String^ remoteIdentifier = /* The identifier associated with the device that generated this packet */
Windows::Storage::Streams::IBuffer^ packetBuffer = /* The incoming chat packet */

chatManager->ProcessIncomingChatMessage(packetBuffer, remoteIdentifier);
```

### <a name="game-chat-2"></a>Game Chat 2

Auf ähnliche Weise Spiel, Chat 2 eine eigene Transportebene keine; Dies muss von der app angegeben werden. Ausgehende Pakete werden von der app reguläre und häufige Aufrufe verarbeitet die `chat_manager::start_processing_data_frames()` und `chat_manager::finish_processing_data_frames()` Paar von Methoden. Diese Methoden sind wie Spiel, Chat 2 für ausgehende Daten an die app bereitstellt. Sie sind dazu konzipiert, schnell ausgeführt werden, dass sie häufig auf einem dedizierten Thread für die Netzwerke abgefragt werden können. Dies bietet eine bequeme Möglichkeit zum Abrufen von Daten für alle in der Warteschlange, ohne sich Gedanken Netzwerk zeitliche Steuerung oder die Komplexität von Multithread-Rückruf nicht vorhersagbar sind.

Wenn `chat_manager::start_processing_data_frames()` aufgerufen wird, alle Daten in der Warteschlange werden gemeldet, in ein Array von `game_chat_data_frame` Zeiger zu strukturieren. Apps sollten das Array durchlaufen, überprüfen das Ziel "Endpunkte" und Verwenden der app-Netzwerkebene, die Daten an die entsprechenden remote-app-Instanzen bereitzustellen. Wenn alle fertig gestellt das `game_chat_data_frame` Strukturen, das Array übergeben werden zurück zum Chatten Spiel-2, um die Ressourcen freigeben, indem `chat_manager:finish_processing_data_frames()`. Zum Beispiel:

```cpp
uint32_t dataFrameCount;
game_chat_data_frame_array dataFrames;
chat_manager::singleton_instance().start_processing_data_frames(&dataFrameCount, &dataFrames);
for (uint32_t dataFrameIndex = 0; dataFrameIndex < dataFrameCount; ++dataFrameIndex)
{
    game_chat_data_frame const* dataFrame = dataFrames[dataFrameIndex];
    HandleOutgoingDataFrame(
        dataFrame->packet_byte_count,
        dataFrame->packet_buffer,
        dataFrame->target_endpoint_identifier_count,
        dataFrame->target_endpoint_identifiers,
        dataFrame->transport_requirement
        );
}
chat_manager::singleton_instance().finish_processing_data_frames(dataFrames);
```

Die Datenrahmen werden verarbeitet, desto häufiger, desto niedriger ist die audio Wartezeit für den Endbenutzer offensichtlich. Das Audio wird in 40 ms-Datenrahmen vereinigt; Dies ist der vorgeschlagene Abrufzeitraum.

Eingehende Daten werden gesendet, Spiel Chat 2 über `chat_manager::processing_incoming_data()`. Der Datenpuffer und die remote-Endpunkt-ID müssen angegeben werden. Das folgende Beispiel zeigt, wie Sie ein Datenpaket übermitteln, die in der lokalen Variablen gespeichert ist `dataFrame` und die remote-Endpunkt-ID, die in der lokalen Variablen gespeichert `remoteEndpointIdentifier`:

```cpp
uin64_t remoteEndpointIdentier = /* The identifier associated with the endpoint that generated this packet */
uint32_t dataFrameSize = /* The size of the incoming data frame, in bytes */
uint8_t* dataFrame = /* A pointer to the buffer containing the incoming data */

chatManager::singleton_instance().process_incoming_data(remoteEndpointIdentifier, dataFrameSize, dataFrame);
```

## <a name="processing-events"></a>Verarbeitung von Ereignissen

### <a name="game-chat"></a>Spiele-Chat

Spiel, Chat verwendet ein Ereignismodell, um die app zu informieren, wenn die Aktionen von Interesse – wie z. B. den Empfang von SMS oder die Änderung der Zugriff auf eine benutzereinstellung tritt auf. Die app muss abonnieren und implementieren Sie einen Handler für jedes Ereignis von Interesse sind. Dieses Beispiel zeigt, wie Sie abonnieren das `OnTextMessageReceived` Ereignis, und leiten Sie die Argumente für die `OnTextChatReceived()` oder `OnTranscribedChatReceived()` Methoden, die von der Anwendung implementiert:

```cpp
auto token = chatManager->OnTextMessageReceived +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^ args)
    {
        if (args->ChatTextMessageType != ChatTextMessageType::TranscribedSpeechMessage)
        {
            this->OnTextChatReceived(args);
        }
        else
        {
            this->OnTranscribedChatReceived(args);
        }
    });
```

### <a name="game-chat-2"></a>Game Chat 2

Spiel, Chat 2 bietet Updates für die app ein, z. B. der empfangenen SMS-Nachrichten, über die app reguläre und häufige Aufrufe der `chat_manager::start_processing_state_changes()` und `chat_manager::finish_processing_state_changes()` Paar von Methoden. Sie sind dazu konzipiert, schnell ausgeführt werden, sodass sie jedes Einzelbild Grafiken in der UI-Rendering-Schleife aufgerufen werden können. Dadurch wird einen praktischer Ort für alle in der Warteschlange stehende Änderungen abrufen, ohne sich Gedanken Netzwerk zeitliche Steuerung oder die Komplexität von Multithread-Rückruf nicht vorhersagbar sind.

Wenn `chat_manager::start_processing_state_changes()` wird aufgerufen, werden alle in der Warteschlange Updates in ein Array von gemeldet `game_chat_state_change` Zeiger zu strukturieren. Apps sollten das Array durchlaufen, überprüfen die grundlegende Struktur für die spezifischeren Typ, wandeln die grundlegende Struktur mit der entsprechenden Weitere detaillierte geben, und klicken Sie dann das Update nach Bedarf zu behandeln. Wenn fertig gestellt, mit allen `game_chat_state_change` Objekte, die derzeit verfügbaren, dieses Array übergeben werden zurück zum Chatten Spiel-2, um die Ressourcen freigeben, indem `chat_manager::finish_processing_state_changes()`. Zum Beispiel:

```cpp
uint32_t stateChangeCount;
game_chat_state_change_array gameChatStateChanges;
chat_manager::singleton_instance().start_processing_state_changes(&stateChangeCount, &gameChatStateChanges);

for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; ++stateChangeIndex)
{
    switch (gameChatStateChanges[stateChangeIndex]->state_change_type)
    {
        case game_chat_state_change_type::text_chat_received:
        {
            HandleTextChatReceived(static_cast<const game_chat_text_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        case Xs::game_chat_2::game_chat_state_change_type::transcribed_chat_received:
        {
            HandleTranscribedChatReceived(static_cast<const Xs::game_chat_2::game_chat_transcribed_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_state_changes(gameChatStateChanges);
```

Da `chat_manager::remove_user()` erklärt für ein User-Objekt zugeordneten Arbeitsspeicher sofort ungültig und Änderungen des Ansichtszustands enthält möglicherweise Verweise auf Benutzerobjekte `chat_manager::remove_user()` darf nicht aufgerufen werden, während der Verarbeitung von Änderungen des Ansichtszustands.

## <a name="text-chat"></a>Text-chat

### <a name="game-chat"></a>Spiele-Chat

Zum Senden von Text-Chat mit dem Spiel Chat, `GameChatUser::GenerateTextMessage()` kann verwendet werden. Das folgende Beispiel zeigt, wie eine Chat-Textnachricht mit einem lokalen Chat-Benutzer, der durch dargestellt senden die `chatUser` Variable:

```cpp
chatUser->GenerateTextMessage(L"Hello", false);
```

Der zweite booleschen Parameter steuert-Konvertierung Sprachwiedergabe von Text. Weitere Informationen finden Sie unter [Barrierefreiheit](#accessibility). Spiel, Chat generiert dann eine Chat-Paket, die diese Nachricht enthält. Der SMS über Remoteinstanzen von Spiel-Chat benachrichtigt werden die `OnTextMessageReceived` Ereignis.

### <a name="game-chat-2"></a>Game Chat 2

Zum Versenden von Text-Chat mit dem Spiel Chat 2 verwenden `chat_user::chat_user_local::send_chat_text()`. Im folgenden Beispiel wird gezeigt, wie zum Senden einer Chat-Text-Nachricht mit einem lokalen Chat-Benutzer, der durch dargestellt die `chatUser` Variable:

```cpp
chatUser->local()->send_chat_text(L"Hello");
```

Spiel, Chat 2 generiert einen Datenrahmen, der diese Nachricht enthält. die Zielendpunkte für den Datenrahmen werden die Anforderungen für Benutzer, die zum Empfangen von Text von den lokalen Benutzer konfiguriert wurden. Wenn die Daten von der entfernten Endpunkten verarbeitet werden, die Nachricht zur Verfügung gestellt über eine `game_chat_text_chat_received_state_change`. Wie bei Voice Chat, werden die Einschränkungen für Berechtigungen und Datenschutz für Textchat eingehalten. Wenn ein Paar der Benutzer Textchat zulassen konfiguriert wurden, aber die Rechte oder des Datenschutzes Einschränkungen nicht, dass die Kommunikation zulassen, wird die Nachricht automatisch gelöscht werden.

Unterstützung der Texteingabe-Chat und Anzeige ist erforderlich, für den Zugriff auf (finden Sie unter [Barrierefreiheit](#accessibility) Weitere Details).

## <a name="accessibility"></a>Bedienungshilfen

Unterstützung für Text-Chat-Eingabe und Anzeige ist erforderlich, für Spiel, Chat und Chat-2-Spiel. Texteingabe ist erforderlich, da, sogar auf Plattformen oder Spiele Genres, die weit verbreitete physische Tastatur verwenden, in der Vergangenheit aufwiesen, Benutzer das System, um die Sprachwiedergabe von Text hilfstechnologien verwenden konfigurieren können. Anzeigen von Text ist ebenso erforderlich, weil Benutzer konfigurieren, dass das System, um die Sprache-zu-Text verwenden können. Spiel-Chat und Chat-2-Spiel bereitstellen Methoden zum Erkennen von und der Rücksichtnahme auf Einstellungen für die Barrierefreiheit des Benutzers; Sie möchten möglicherweise bedingt Text Mechanismen, die basierend auf diesen Einstellungen zu aktivieren.

### <a name="text-to-speech---game-chat"></a>Sprachsynthese-Game-Chat

Wenn ein Benutzer aktiviert, Speech hat `GameChatUser::HasRequestedSynthesizedAudio()` gibt "true" zurück. Wenn dieser Zustand erkannt wird, `GameChatUser::GenerateTextMessage()` generiert außerdem-Sprach-Audio, die in den Audiostream des lokalen Benutzers eingefügt wird. Das folgende Beispiel zeigt, wie eine Chat-Textnachricht mit einem lokalen Benutzer, der durch dargestellt senden die `chatUser` Variable:

```cpp
chatUser->GenerateTextMessage(L"Hello", true);
```

Der zweite booleschen Parameter wird verwendet, um die app die Sprache-zu-Text-Einstellung außer Kraft setzen kann: "True" gibt an, dass das Spiel Chat-Sprach-Audio generieren soll, wenn die Einstellungen der Benutzer Text-Sprach-aktiviert wurde während 'False', der angibt, Spiel Chat sollte-Sprach-Audio nie aus der Nachricht generiert werden.

### <a name="text-to-speech---game-chat-2"></a>Sprachsynthese-Spiel, Chat 2

Wenn ein Benutzer aktiviert, Speech hat `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 'true' zurück. Wenn dieser Zustand erkannt wird, muss die app eine Methode der Texteingabe bereitzustellen. Nachdem Sie die Texteingabe, die von einem physischen oder virtuellen Tastatur bereitgestellt haben, übergeben Sie die Zeichenfolge, die die `chat_user::chat_user_local::synthesize_text_to_speech()` Methode. Spiel, Chat 2 erkennt und basierte auf der Zeichenfolge und die Einstellungen der Benutzer zugegriffen werden kann, Voice Audiodaten zu synthetisieren. Zum Beispiel:

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

Das Audio synthetisiert, die im Rahmen dieses Vorgangs wird für alle Benutzer, die zum Empfangen von Audiodaten von dieses lokalen Benutzers konfiguriert wurden, übertragen werden. Wenn `chat_user::chat_user_local::synthesize_text_to_speech()` wird aufgerufen, für einen Benutzer, die keine Sprachwiedergabe von Text aktiviert Spiel, Chat 2 ist keine weitere Aktion durchführen.

### <a name="speech-to-text---game-chat"></a>Sprache-zu-Text - Game-Chat

Wenn ein Benutzer die Sprache-zu-Text-aktiviert ist, hat `GameChatUser::HasRequestSynthesizedAudio()` 'true' zurück. Wenn dieser Zustand erkannt wird, spielen Chat automatisch das Audio des Audio für jeden Remotebenutzer zu transkribieren und verfügbar zu machen, über die `OnTextMessageReceived` Ereignis. Wenn die `OnTextMessageReceived` -Ereignis ausgelöst wird, aufgrund der Empfang einer Nachricht Aufzeichnung, die `TextMessageReceivedEventArgs` gibt Nachrichtentyp `ChatTextMessageType::TranscribedSpeechMessage`.

### <a name="speech-to-text---game-chat-2"></a>Sprache-zu-Text - Spiel, Chat 2

Wenn ein Benutzer die Sprache-zu-Text-aktiviert ist, hat `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` gibt "true" zurück. Wenn dieser Zustand erkannt wird, muss die app angeben, dass nach Möglichkeit transkribierte Chatnachrichten Benutzeroberfläche zugeordnete vorbereitet werden. Spiel, Chat 2 automatisch jeden Remotebenutzer Audio transkribieren und verfügbar zu machen, über eine `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` eine Xbox One Klasse wurde speziell für ein einfaches Rendering von Text mit in-Game-Chat mit dem Schwerpunkt auf hilfstechnologien Sprache-zu-Text bereitstellen.

Finden Sie unter [mithilfe von Spiel-Chat 2 – Überlegungen zur Leistung von Sprache-zu-Text](using-game-chat-2.md#speech-to-text-performance-considerations) für Weitere Informationen zu Überlegungen zur Leistung von Sprache-zu-Text.

## <a name="ui"></a>UI

Es wird empfohlen, dass an einer beliebigen Stelle Player, besonders in einer Liste von Gamertags wie z. B. eine spielstandsanzeige, angezeigt werden, dass auch stumm geschaltet/Vorträge Symbole als Feedback für den Benutzer angezeigt werden. Spiel, Chat 2 führt einen zusammengeführte Indikator, der ein einfacher bestimmen die entsprechenden Elemente der Benutzeroberfläche anzuzeigenden ermöglicht.

### <a name="game-chat"></a>Spiele-Chat

Ein `GameChatUser` verfügt über drei Eigenschaften, die häufig, beim Ermitteln der entsprechenden Elemente der Benutzeroberfläche überprüft werden - `GameChatUser::TalkingMode`, `GameChatUser::IsMuted`, und `GameChatUser::RestrictionMode`. Das folgende Beispiel zeigt eine mögliche Heuristik zur Bestimmung der ein bestimmtes Symbol Konstanten erfragen zuweisen eine `iconToShow` befehlsshellvariable eine `GameChatUser` Objekt verweist die Variable "ChatUser".

```cpp
if (chatUser->RestrictionMode != None)
{
    if (!chatUser->IsMuted)
    {
        if (chatUser->TalkingMode != NotTalking)
        {
            iconToShow = Icon_ActiveSpeaker;
        }
        else
        {
            iconToShow = Icon_InactiveSpeaker;
        }
    }
    else
    {
        iconToShow = Icon_MutedSpeaker;
    }
}
else
{
    iconToShow = Icon_RestrictedSpeaker;
}
```

### <a name="game-chat-2"></a>Game Chat 2

Spiele Chat 2 verfügt über eine zusammengeführte `game_chat_user_chat_indicator` verwendet, um den Status aktueller, sofortige Chat für einen Player darstellen. Dieser Wert wird abgerufen, durch den Aufruf `chat_user::chat_indicator()`. Das folgende Beispiel zeigt das Abrufen der Indikatorwert für den ein `chat_user` Objekt verweist die Variable "ChatUser", um zu bestimmen, einen bestimmtes Symbol Konstantenwert zuweisen zu einer Variablen "IconToShow":

```cpp
switch (chatUser->chat_indicator())
{
   case game_chat_user_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::local_microphone_muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

## <a name="muting"></a>Stummschaltung

## <a name="game-chat"></a>Spiele-Chat

Stummschaltung oder wird einen Benutzer im Spiel Chat erfolgt durch einen Aufruf von `ChatManager::MuteUserFromAllChannels()` oder `ChatManager::UnMuteUIserFromAllChannels()`bzw. Der Status eines Benutzers zum Stummschalten abgerufen werden, indem Sie überprüfen `GameChatUser::IsMuted` oder `GameChatUser::IsLocalUserMuted`.

## <a name="game-chat-2"></a>Game Chat 2

Die `chat_user::chat_user_local::set_microphone_muted()` Methode kann verwendet werden, um den Status eines lokalen Benutzers Mikrofon stumm schalten auszuschalten. Wenn das Mikrofon stumm geschaltet sind, werden ohne Audio, Mikrofon, erfasst. Wenn der Benutzer auf ein freigegebenes Gerät, z. B. Kinect, gilt der zum Stummschalten Status für alle Benutzer.

Die `chat_user::chat_user_local::microphone_muted()` Methode kann verwendet werden, um das Abrufen des Status eines lokalen Benutzers Mikrofon stumm schalten. Diese Methode gibt nur wieder, ob in der Software über einen Aufruf des lokalen Benutzers Mikrofon stummgeschaltet ist `chat_user::chat_user_local::set_microphone_muted()`; diese Methode berücksichtigt nicht die eine Hardware-stummschaltung gesteuert werden, z. B. über eine Schaltfläche des Benutzers Kopfhörer. Es gibt keine Methode zum Abrufen von den Zustand der Hardware zum Stummschalten der audio-Gerät eines Benutzers über die Spiel-Chat-2.

Die `chat_user::chat_user_local::set_remote_user_muted()` Methode kann verwendet werden, um den Status eines Remotebenutzers in Bezug auf einen bestimmten lokalen Benutzer zum Stummschalten auszuschalten. Wenn der Remotebenutzer stumm geschaltet wird, nicht des lokalen Benutzers Audio hören oder vom Remotebenutzer entgegen SMS empfangen.

## <a name="bad-reputation-auto-mute"></a>In dem Ruf Auto-Stummschalten

In der Regel werden Remotebenutzern unmuted beginnen. Spiel Chat und Spiel, Chat 2 haben eine Funktion "in dem Ruf Auto-Ton aus". Dies bedeutet, dass Benutzer in einem Zustand stumm geschaltet sind beginnen werden, wenn (1) der Remotebenutzer nicht Freunde mit einem lokalen Benutzer und (2) remote User has ein Flag in dem Ruf kann. Spiel, Chat 2 liefert Feedback aus, wenn ein Benutzer aufgrund dieses Features stumm geschaltet wird. Finden Sie unter [mithilfe von Spiel-Chat 2 – in dem Ruf Auto-stummschaltung](using-game-chat-2.md#bad-reputation-auto-mute) für Weitere Informationen.

## <a name="privilege-and-privacy"></a>Berechtigungen und Datenschutz

Spiel-Chat und Chat-2-Spiel erzwingen Xbox Live Berechtigungen und Datenschutz Einschränkungen über alle Kanal oder Beziehungen, die von der app verwaltet. Spiel, Chat 2 bietet zudem Diagnoseinformationen, um zu bestimmen, genau wie die Einschränkung, beeinträchtigt die Richtung des Audio (z. B., ob Audio eine audio Einschränkung eine unidirektionale oder bidirektionale ist).

### <a name="game-chat"></a>Spiele-Chat

Spiele-Chat verfügbar gemacht werden Informationen zu Berechtigungen und Datenschutz über die `RestrictionMode` Eigenschaft. Es kann durch Überprüfen abgerufen werden `GameChatUser::RestrictionMode`.

### <a name="game-chat-2"></a>Game Chat 2

Spiel, Chat 2 führt Berechtigungen und Datenschutz Einschränkung Suchvorgänge aus, wenn ein Benutzer zuerst hinzugefügt wird; des Benutzers `chat_user::chat_indicator()` gibt stets `game_chat_user_chat_indicator::silent` bis zum Abschluss dieser Vorgänge. Wenn einer Rechte oder des Datenschutzes der Einschränkung, die Benutzer auf die Kommunikation mit einem Benutzer auswirken `chat_user::chat_indicator()` zurück `game_chat_user_chat_indicator::platform_restricted`. Kommunikation von plattformbeschränkungen gelten für sowohl Text als auch Voice Chat. Es werden nie eine Instanz, in denen Textchat wird durch eine plattformeinschränkung blockiert Voice Chat ist jedoch nicht, oder umgekehrt.

`chat_user::chat_user_local::get_effective_communication_relationship()` kann verwendet werden, um Hilfe zu unterscheiden, wenn Benutzer aufgrund von unvollständigen Berechtigungen und Datenschutz Vorgänge kommunizieren können. Gibt die vom Spiel, Chat 2 erzwungen wird, in Form von kommunikationsbeziehung `game_chat_communication_relationship_flags` und die Ursache die Beziehung entspricht möglicherweise nicht auf die konfigurierten Beziehung in Form von `game_chat_communication_relationship_adjuster`. Angenommen, die Suchvorgänge noch ausgeführt wird, werden die `game_chat_communication_relationship_adjuster` werden `game_chat_communication_relationship_adjuster::intializing`. Diese Methode wird erwartet, bei der Entwicklung und debugging-Szenarien verwendet werden. Es sollte nicht zum beeinflussen der Benutzeroberfläche verwendet werden (siehe [UI](#UI)).

## <a name="cleanup"></a>Bereinigen

Das ursprüngliche Spiel Chat `ChatManager` ist eine WinRT-Runtime-Klasse – eine verweisgezählte Schnittstelle. Daher es verfügbaren Speicherressourcen werden freigegeben, wenn der letzte Verweiszähler auf 0 (null) fällt und bereinigt. Interaktion mit dem Spiel Chat 2 C++-API erfolgt über eine Singleton-Instanz; Wenn die app nicht mehr Kommunikation über das Spiel, Chat 2 benötigt, Sie sollten Aufrufen `chat_manager::cleanup()`. Dies ermöglicht das Spiel, Chat 2 sofort alle Ressourcen frei, die zugeordnet wurden, um die Kommunikation verwalten. Ausführliche Informationen zum Spiel Chat 2 WinRT-API und ressourcenverwaltung, finden Sie unter [mithilfe von Spiel-Chat 2 WinRT Projektionen](using-game-chat-2-winrt.md#cleanup).

## <a name="failure-model-and-debugging"></a>Fehler-Modell und das Debuggen

Das ursprüngliche Spiel Chat ist eine WinRT-API. Daher hat es über Ausnahmen Fehler gemeldet. Nicht schwerwiegende Fehler oder Warnungen werden durch eine optionale Rückruf gemeldet. Spiele Chat 2 im C++-API löst keine Ausnahmen als Mittel zum nicht schwerwiegenden Fehler, die Berichterstattung, damit Sie es einfach aus Projekten ausnahmefreie nutzen darf, wenn bevorzugt. Spiel, Chat 2, allerdings löst Ausnahmen aus, zu schwerwiegenden Fehlern zu informieren. Diese Fehler sind ein Ergebnis von API-Missbrauch besser geschützt, wie z. B. das Hinzufügen eines Benutzers mit der Game-Chat-Instanz vor dem Initialisieren der Instanz oder den Zugriff auf ein Objekt, nachdem es von der Game-Chat-Instanz entfernt wurde. Diese Fehler früh in der Entwicklung abgefangen werden sollen, und es können korrigiert werden, indem Sie das Muster für die Interaktion mit dem Spiel, Chat 2 ändern. Tritt ein Fehler auf, wird an den Debugger ein Hinweis auf die Ursache des Fehlers ausgegeben, bevor die Ausnahme ausgelöst wird. Spiel Chat 2 die WinRT-API basiert auf der WinRT-Muster der Berichterstattung über Ausnahmen Fehler.

## <a name="how-to-configure-popular-scenarios"></a>Gängige Szenarien konfigurieren

Finden Sie unter [mithilfe von Spiel-Chat 2: gängige Szenarien konfigurieren](using-game-chat-2.md#how-to-configure-popular-scenarios) Beispiele für gängige Szenarien wie z. B. die Push-an-Talk, Teams und Broadcast-ähnlichen Kommunikationsszenarien zu konfigurieren.

## <a name="pre-encode-and-post-decode-audio-manipulation"></a>Vorab zu codieren und Decodieren nach der Bearbeitung von Audiodateien

Das ursprüngliche Spiel Chat für audio Bearbeitung zulässig, durch das Gewähren des Zugriffs auf unformatierte Mikrofon Audio über den `OnPreEncodeAudioBuffer` und `OnPostDecodeAudioBuffers` Ereignisse. Spiel, Chat 2 macht diese Funktion über ein Modell abrufen. Weitere Informationen finden Sie unter [in Echtzeit audio Manipulation](real-time-audio-manipulation.md).
