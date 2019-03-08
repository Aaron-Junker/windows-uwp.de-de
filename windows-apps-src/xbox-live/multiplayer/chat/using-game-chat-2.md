---
title: Spiele-Chat 2 verwenden
description: Erfahren Sie, wie Xbox Live-Game-Chat 2 zu verwenden, um Ihr Spiel Sprachkommunikation hinzugefügt.
ms.date: 03/20/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Spiele Chat 2, Spiele Chat, Sprachkommunikation
ms.localizationpriority: medium
ms.openlocfilehash: 43fbd7cec037df735686aa60bc6cd57217875e33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639255"
---
# <a name="using-game-chat-2-c"></a>Verwenden von Spiele-Chat-2 (C++)

Dies ist eine kurze exemplarische Vorgehensweise zur Verwendung von C++-API-Spiel Chat 2. Entwickler, die auf der Game-Chat 2 bis spielen C# sollte [verwenden Spiel Chat-2-WinRT-Projektionen](using-game-chat-2-winrt.md).

1. [Erforderliche Komponenten](#prerequisites)
2. [Initialisierung](#initialization)
3. [Konfigurieren von Benutzern](#configuring-users)
4. [Verarbeiten von Datenrahmen.](#processing-data-frames)
5. [Verarbeiten von Änderungen des Ansichtszustands](#processing-state-changes)
6. [Text-chat](#text-chat)
7. [Bedienungshilfen](#accessibility)
8. [UI](#ui)
9. [Stummschaltung](#muting)
10. [In dem Ruf Auto-Stummschalten](#bad-reputation-auto-mute)
11. [Berechtigungen und Datenschutz](#privilege-and-privacy)
12. [Bereinigen](#cleanup)
13. [Fehler-Modell](#failure-model)
14. [Gängige Szenarien konfigurieren](#how-to-configure-popular-scenarios)

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

Die Spiel, Chat-2-Schnittstelle erfordert kein Projekt auswählen, die zwischen dem Kompilieren mit C++ / CX im Vergleich zu herkömmlichen C++; Es kann entweder mit verwendet werden. Die Implementierung auslösen nicht auch Ausnahmen, als Mittel zum nicht schwerwiegenden Fehler, die Berichterstattung, damit Sie es einfach aus Projekten ausnahmefreie nutzen darf, wenn bevorzugt. Die Implementierung, aber löst Ausnahmen als Mittel zum Schwerwiegender Fehler reporting (finden Sie unter [Fehler Modell](#failure) Einzelheiten).

## <a name="initialization"></a>Initialisierung

Sie beginnen mit der Bibliothek interagieren, initialisieren Sie die Spiel-Chat-2-Singleton-Instanz mit Parametern, die für die Lebensdauer des Singletons Initialisierung gelten.

```cpp
chat_manager::singleton_instance().initialize(...);
```

## <a name="configuring-users"></a>Konfigurieren von Benutzern

Sobald die Instanz initialisiert wird, müssen Sie die lokalen Benutzer mit der Game-Chat-2-Instanz hinzufügen. In diesem Beispiel stellt der Benutzer ein lokalen Benutzer dar.

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(<user_a_xuid>);
```

Sie müssen auch hinzufügen, die remote-Benutzer und dem Bezeichner, die zum Darstellen von remote "Endpoint" verwendet werden, in der der Benutzer ist. "Endpunkt" ist eine Instanz der app auf einem Remotegerät ausgeführt wird. In diesem Beispiel ist User B Endpunkt X, Benutzer C und D befinden sich auf Endpunkt y Endpunkt X wird die ID "1" nach dem Zufallsprinzip zugewiesen, und Endpunkt Y Bezeichner "2" nach dem Zufallsprinzip zugewiesen wird. Informieren Sie Spiel Chat 2 der Remotebenutzer mit diesen aufrufen:

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(<user_b_xuid>, 1);
chat_user* chatUserC = chat_manager::singleton_instance().add_remote_user(<user_c_xuid>, 2);
chat_user* chatUserD = chat_manager::singleton_instance().add_remote_user(<user_d_xuid>, 2);
```

Nachdem Sie die kommunikationsbeziehung zwischen jeden Remotebenutzer und dem lokalen Benutzer konfigurieren. In diesem Beispiel nehmen wir an, dass Benutzer A und Benutzer B befinden sich auf das gleiche Team und die bidirektionale Kommunikation zulässig ist. `c_communicationRelationshipSendAndReceiveAll` in GameChat2.h bidirektionale Kommunikation darstellen wird eine Konstante definiert werden. Festlegen des benutzerbeziehung an Benutzer B mit:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

Nehmen wir nun an, dass Benutzer C und D "Zuschauer" und die Berechtigung für Benutzer A anhören, aber nicht sprechen. `c_communicationRelationshipSendAll` in GameChat2.h unidirektionale Kommunikation darstellen wird eine Konstante definiert werden. Legen Sie die Beziehungen mit:

```cpp
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

Wenn zu einem beliebigen Zeitpunkt Remotebenutzer, die die Singleton-Instanz hinzugefügt wurden, aber noch nicht so konfiguriert, dass die Kommunikation mit den lokalen Benutzern - vorhanden sind, ist das kein Problem! Dies kann in Szenarien erwartet werden, in dem Benutzer werden Teams bestimmen oder gesehen Kanäle können beliebig ändern. Spiel, Chat 2 werden nur Informationen (z. B. Beziehungen für Datenschutz und die Zuverlässigkeit der) für Benutzer zwischengespeichert werden, die auf die Instanz, daher ist es nützlich, um das Spiel Chat 2 alle möglichen Benutzer zu informieren, auch wenn für alle lokalen Benutzer zu einem bestimmten Zeitpunkt sprechen kann nicht hinzugefügt wurden.

Schließlich nehmen Sie an, dass Benutzer D das Spiel verlassen hat und aus der lokalen Spiel, Chat-2-Instanz entfernt werden sollen. Kann mit dem folgenden Aufruf erfolgen:

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

Aufrufen von `chat_manager::remove_user()` möglicherweise das User-Objekt ungültig. Bei Verwendung von [in Echtzeit audio Manipulation](real-time-audio-manipulation.md), finden Sie in der [Chat Benutzer Lebensdauer](real-time-audio-manipulation.md#chat-user-lifetimes) Dokumentation für Weitere Informationen zu erhalten. Das Benutzerobjekt wird hingegen sofort ungültig Wenn `chat_manager::remove_user()` aufgerufen wird. Eine subtile Einschränkung auf, wenn Benutzer entfernt werden können, finden Sie im [Verarbeitung von Änderungen des Ansichtszustands](#state).

## <a name="processing-data-frames"></a>Verarbeiten von Datenrahmen.

Spiel, Chat 2 besitzt keine eigene Transportschicht; Dies muss von der app angegeben werden. Dieses Plug-in wird von der app reguläre und häufige Aufrufe verwaltet die `chat_manager::start_processing_data_frames()` und `chat_manager::finish_processing_data_frames()` Paar von Methoden. Diese Methoden sind wie Spiel, Chat 2 für ausgehende Daten an die app bereitstellt. Sie sind dazu konzipiert, schnell ausgeführt werden, dass sie häufig auf einem dedizierten Thread für die Netzwerke abgefragt werden können. Dies bietet eine bequeme Möglichkeit zum Abrufen von Daten für alle in der Warteschlange, ohne sich Gedanken Netzwerk zeitliche Steuerung oder die Komplexität von Multithread-Rückruf nicht vorhersagbar sind.

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

## <a name="processing-state-changes"></a>Verarbeiten von Änderungen des Ansichtszustands

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

Um Textchat senden zu können, verwenden `chat_user::chat_user_local::send_chat_text()`. Zum Beispiel:

```cpp
chatUserA->local()->send_chat_text(L"Hello");
```

Spiel, Chat 2 generiert einen Datenrahmen, der diese Nachricht enthält. die Zielendpunkte für den Datenrahmen werden die Anforderungen für Benutzer, die zum Empfangen von Text von den lokalen Benutzer konfiguriert wurden. Wenn die Daten von der entfernten Endpunkten verarbeitet werden, die Nachricht zur Verfügung gestellt über eine `game_chat_text_chat_received_state_change`. Wie bei Voice Chat, werden die Einschränkungen für Berechtigungen und Datenschutz für Textchat eingehalten. Wenn ein Paar der Benutzer Textchat zulassen konfiguriert wurden, aber die Rechte oder des Datenschutzes Einschränkungen nicht, dass die Kommunikation zulassen, wird die Nachricht gelöscht werden.

Unterstützung der Texteingabe-Chat und Anzeige ist erforderlich, für den Zugriff auf (finden Sie unter [Barrierefreiheit](#access) Weitere Details).

## <a name="accessibility"></a>Bedienungshilfen

Unterstützung für Text-Chat-Eingabe und Anzeige ist erforderlich. Texteingabe ist erforderlich, da, sogar auf Plattformen oder Spiele Genres, die weit verbreitete physische Tastatur verwenden, in der Vergangenheit aufwiesen, Benutzer das System, um die Sprachwiedergabe von Text hilfstechnologien verwenden konfigurieren können. Anzeigen von Text ist ebenso erforderlich, weil Benutzer konfigurieren, dass das System, um die Sprache-zu-Text verwenden können. Diese Einstellungen können auf lokale Benutzer erkannt werden, durch den Aufruf der `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` und `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` Methoden, und Sie möchten die Text-Mechanismen bedingt zu aktivieren.

### <a name="text-to-speech"></a>Text-zu-Sprache

Wenn ein Benutzer aktiviert, Speech hat `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` 'true' zurück. Wenn dieser Zustand erkannt wird, muss die app eine Methode der Texteingabe bereitzustellen. Nachdem Sie die Texteingabe, die von einem physischen oder virtuellen Tastatur bereitgestellt haben, übergeben Sie die Zeichenfolge, die die `chat_user::chat_user_local::synthesize_text_to_speech()` Methode. Spiel, Chat 2 erkennt und basierte auf der Zeichenfolge und die Einstellungen der Benutzer zugegriffen werden kann, Voice Audiodaten zu synthetisieren. Zum Beispiel:

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

Das Audio synthetisiert, die im Rahmen dieses Vorgangs wird für alle Benutzer, die zum Empfangen von Audiodaten von dieses lokalen Benutzers konfiguriert wurden, übertragen werden. Wenn `chat_user::chat_user_local::synthesize_text_to_speech()` wird aufgerufen, für einen Benutzer, die keine Sprachwiedergabe von Text aktiviert Spiel, Chat 2 ist keine weitere Aktion durchführen.

### <a name="speech-to-text"></a>Sprache-zu-text

Wenn ein Benutzer die Sprache-zu-Text-aktiviert ist, hat `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` gibt "true" zurück. Wenn dieser Zustand erkannt wird, muss die app angeben, dass nach Möglichkeit transkribierte Chatnachrichten Benutzeroberfläche zugeordnete vorbereitet werden. Spiel, Chat 2 automatisch jeden Remotebenutzer Audio transkribieren und verfügbar zu machen, über eine `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` eine Xbox One Klasse wurde speziell für ein einfaches Rendering von Text mit in-Game-Chat mit dem Schwerpunkt auf hilfstechnologien Sprache-zu-Text bereitstellen.

### <a name="speech-to-text-performance-considerations"></a>Überlegungen zur Leistung von Sprache-zu-text

Wenn Sprache-zu-Text aktiviert ist, initiiert die Game-Chat-2-Instanz auf jedem Gerät remote eine Web-Socket-Verbindung mit dem Endpunkt der spracherkennungs-Dienste. Jedem Spiel, Chat-2-Remoteclient lädt Audio für die Spracherkennung Services-Endpunkte über diesem Websocket; die spracherkennungs-Dienst-Endpunkte gibt gelegentlich eine Aufzeichnung-Meldung zurück, mit dem Remotegerät. Das Remotegerät sendet dann die Aufzeichnung-Nachricht (d. h. eine Textnachricht) auf dem lokalen Gerät, in dem die Nachricht nach Möglichkeit transkribierte Spiel, Chat 2 an die app erhält zum Rendern.

Daher ist die primäre Leistungseinbußen von Sprache-zu-Text-Netzwerkverwendung an. Der Netzwerkdatenverkehr wird das codierte Audio hochladen. Der Websocket hochlädt, Audio, die bereits im Pfad "normal" Voice Chat Spiel Chat 2 codiert wurde; die app verfügt über die Kontrolle über die Bitrate über `chat_manager::set_audio_encoding_type_and_bitrate`.

## <a name="ui"></a>UI

Es wird empfohlen, dass an einer beliebigen Stelle Player, besonders in einer Liste von Gamertags wie z. B. eine spielstandsanzeige, angezeigt werden, dass auch stumm geschaltet/Vorträge Symbole als Feedback für den Benutzer angezeigt werden. Dies erfolgt durch Aufrufen von `chat_user::chat_indicator()` zum Abrufen einer `game_chat_user_chat_indicator` , die den aktuellen, sofortigen Status Chat für die betreffende Spieler darstellt. Das folgende Beispiel zeigt das Abrufen der Indikatorwert für den ein `chat_user` Objekt verweist die Variable "ChatUserA", um zu bestimmen, einen bestimmtes Symbol Konstantenwert zuweisen zu einer Variablen "IconToShow":

```cpp
switch (chatUserA->chat_indicator())
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

Der gemeldete Wert ist vom `xim_player::chat_indicator()` wird erwartet, dass Sie häufig als Player starten und beenden zu sprechen, z. B. ändern. Es soll Abrufen jedes UI-Frames daher-apps zu unterstützen.

## <a name="muting"></a>Stummschaltung

Die `chat_user::chat_user_local::set_microphone_muted()` Methode kann verwendet werden, um den Status eines lokalen Benutzers Mikrofon stumm schalten auszuschalten. Wenn das Mikrofon stumm geschaltet sind, werden ohne Audio, Mikrofon, erfasst. Wenn der Benutzer auf ein freigegebenes Gerät, z. B. Kinect, gilt der zum Stummschalten Status für alle Benutzer.

Die `chat_user::chat_user_local::microphone_muted()` Methode kann verwendet werden, um das Abrufen des Status eines lokalen Benutzers Mikrofon stumm schalten. Diese Methode gibt nur wieder, ob in der Software über einen Aufruf des lokalen Benutzers Mikrofon stummgeschaltet ist `chat_user::chat_user_local::set_microphone_muted()`; diese Methode berücksichtigt nicht die eine Hardware-stummschaltung gesteuert werden, z. B. über eine Schaltfläche des Benutzers Kopfhörer. Es gibt keine Methode zum Abrufen von den Zustand der Hardware zum Stummschalten der audio-Gerät eines Benutzers über die Spiel-Chat-2.

Die `chat_user::chat_user_local::set_remote_user_muted()` Methode kann verwendet werden, um den Status eines Remotebenutzers in Bezug auf einen bestimmten lokalen Benutzer zum Stummschalten auszuschalten. Wenn der Remotebenutzer stumm geschaltet wird, nicht des lokalen Benutzers Audio hören oder vom Remotebenutzer entgegen SMS empfangen.

## <a name="bad-reputation-auto-mute"></a>In dem Ruf Auto-Stummschalten

In der Regel werden Remotebenutzern unmuted beginnen. Spiel, Chat 2 startet die Benutzer in einem Zustand stumm geschaltet sind, wenn (1) der Remotebenutzer nicht Freunde mit dem lokalen Benutzer und (2) remote User has ein Ruf-Flag kann. Wenn Benutzer aufgrund von diesem Vorgang stumm geschaltet sind `chat_user::chat_indicator()` zurück `game_chat_user_chat_indicator::reputation_restricted`. Dieser Zustand wird von der erste Aufruf überschrieben `chat_user::chat_user_local::set_remote_user_muted()` , die den entfernten Benutzer als dem Zielbenutzer enthält.

## <a name="privilege-and-privacy"></a>Berechtigungen und Datenschutz

Zusätzlich zu den kommunikationsbeziehung konfiguriert, indem das Spiel erzwingt Spiel, Chat 2 Einschränkungen für Berechtigungen und Datenschutz. Spiel, Chat 2 führt Berechtigungen und Datenschutz Einschränkung Suchvorgänge aus, wenn ein Benutzer zuerst hinzugefügt wird; des Benutzers `chat_user::chat_indicator()` gibt stets `game_chat_user_chat_indicator::silent` bis zum Abschluss dieser Vorgänge. Wenn einer Rechte oder des Datenschutzes der Einschränkung, die Benutzer auf die Kommunikation mit einem Benutzer auswirken `chat_user::chat_indicator()` zurück `game_chat_user_chat_indicator::platform_restricted`. Kommunikation von plattformbeschränkungen gelten für sowohl Text als auch Voice Chat. Es werden nie eine Instanz, in denen Textchat wird durch eine plattformeinschränkung blockiert Voice Chat ist jedoch nicht, oder umgekehrt.

`chat_user::chat_user_local::get_effective_communication_relationship()` kann verwendet werden, um Hilfe zu unterscheiden, wenn Benutzer aufgrund von unvollständigen Berechtigungen und Datenschutz Vorgänge kommunizieren können. Gibt die vom Spiel, Chat 2 erzwungen wird, in Form von kommunikationsbeziehung `game_chat_communication_relationship_flags` und die Ursache die Beziehung entspricht möglicherweise nicht auf die konfigurierten Beziehung in Form von `game_chat_communication_relationship_adjuster`. Angenommen, die Suchvorgänge noch ausgeführt wird, werden die `game_chat_communication_relationship_adjuster` werden `game_chat_communication_relationship_adjuster::intializing`. Diese Methode wird erwartet, bei der Entwicklung und debugging-Szenarien verwendet werden. Es sollte nicht zum beeinflussen der Benutzeroberfläche verwendet werden (siehe [UI](#UI)).

## <a name="cleanup"></a>Bereinigen

Wenn die app nicht mehr Kommunikation über das Spiel, Chat 2 benötigt, Sie sollten Aufrufen `chat_manager::cleanup()`. Dadurch wird ein Spiel Chat 2 Ressourcen freigeben, die zugeordnet wurden, um die Kommunikation zu verwalten.

## <a name="failure-model"></a>Fehler-Modell

Die Spiel-Chat-2-Implementierung löst keine Ausnahmen als Mittel zum nicht schwerwiegenden Fehler, die Berichterstattung, damit Sie es einfach aus Projekten ausnahmefreie nutzen darf, wenn bevorzugt. Spiel, Chat 2, allerdings löst Ausnahmen aus, zu schwerwiegenden Fehlern zu informieren. Diese Fehler sind ein Ergebnis von API-Missbrauch besser geschützt, wie z. B. das Hinzufügen eines Benutzers mit der Game-Chat-Instanz vor dem Initialisieren der Instanz oder den Zugriff auf ein Objekt, nachdem es von der Game-Chat-2-Instanz entfernt wurde. Diese Fehler früh in der Entwicklung abgefangen werden sollen, und es können korrigiert werden, indem Sie das Muster für die Interaktion mit dem Spiel, Chat 2 ändern. Tritt ein Fehler auf, wird an den Debugger ein Hinweis auf die Ursache des Fehlers ausgegeben, bevor die Ausnahme ausgelöst wird.

## <a name="how-to-configure-popular-scenarios"></a>Gängige Szenarien konfigurieren

### <a name="push-to-talk"></a>Push-an-talk

Push-an-Talk mit implementiert werden sollte `chat_user::chat_user_local::set_microphone_muted()`. Rufen Sie `set_microphone_muted(false)` zu Sprache und `set_microphone_muted(true)` um es zu beschränken. Diese Methode bietet die niedrigste Latenz-Antwort vom Spiel, Chat 2.

### <a name="teams"></a>Teams

Nehmen wir an, dass Benutzer A und Benutzer B auf Blue Team, und Benutzer C und D für Benutzer, die auf Team Rot sind. Jeder Benutzer ist in der app eine eindeutige Instanz.

Für Benutzer A-Gerät:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
chatUserA->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserA->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

Auf Benutzer B-Gerät:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipSendAndReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

Auf den Benutzer C-Gerät:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAndReceiveAll);
```

Auf die Benutzer-D-Gerät:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAndReceiveAll);
```

### <a name="broadcast"></a>Übertragung

Nehmen wir an, dass Benutzer A ist das führende Aufträge erteilen und Benutzer B, C und D nur überwachen können. Jeder Spieler befindet sich auf eine eindeutige Geräte.

Für Benutzer A-Gerät:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

Auf Benutzer B-Gerät:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

Auf den Benutzer C-Gerät:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

Auf die Benutzer-D-Gerät:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
```
