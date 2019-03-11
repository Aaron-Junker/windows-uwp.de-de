---
title: Echtzeit-Audiommanipulation
description: Informationen Sie zum Bearbeiten und verarbeiten das Spiel Chat 2 erfasste Chat-Audio.
ms.date: 05/10/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Spiele Chat 2, Spiele Chat, Sprachkommunikation, pufferbearbeitung, audio-Bearbeitung
ms.localizationpriority: medium
ms.openlocfilehash: 7746080ea8a9698993a679b425f41442e4a6d943
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643895"
---
# <a name="real-time-audio-manipulation"></a>Echtzeit-Audiommanipulation

Spiele-Chat-2 bietet Entwicklern die Möglichkeit, sich selbst in die audio Chat-Pipeline, um zu überprüfen und Bearbeiten der HomeRuns Chat-Audiodaten einfügen. Dies kann nützlich für die Anwendung, Audioeffekte auf HomeRuns stimmen zurück, die im Spiel interessant sein. Spiele Chat-2 audio Manipulation Pipeline ist-Serverinfrastruktur wird dabei vorausgesetzt mit durch Audiodatenstrom-Objekte, die Audiodaten abgerufen werden können. Im Gegensatz zur Verwendung von Rückrufen, ermöglicht es dieses Modell Entwicklern zu überprüfen oder Bearbeiten von audio, auf den jeweiligen Verarbeitungsthread für sie am einfachsten ist.

Eine kurze exemplarische Vorgehensweise, in Echtzeit audio Manipulation wird im folgenden vorgestellt, die in den folgenden Themen enthält:

1. [Initialisieren die Manipulation von audio-pipeline](#initializing-the-audio-manipulation-pipeline)
2. [Verarbeiten von Änderungen am Ansichtszustand des Audiodatenstroms](#processing-audio-stream-state-changes)
3. [Bearbeiten von vorab Chat-Audio codieren](#manipulating-pre-encode-chat-audio-data)
4. [Bearbeiten von nach dem Chat-Audio decodieren](#manipulating-post-decode-chat-audio-data)
5. [Lebensdauer der Chat-Benutzer](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>Initialisieren die Manipulation von audio-pipeline

Spiel, Chat 2 in der Standardeinstellung wird in Echtzeit-audio-Bearbeitung nicht aktiviert. Um in Echtzeit-audio-Bearbeitung zu aktivieren, die app muss angeben, welche Formulare audio Manipulation soll aktiviert `chat_manager::initialize()` AudioManipulationMode Parameter festlegen.

Derzeit werden audio Manipulation folgendermaßen unterstützt:

* `game_chat_audio_manipulation_mode_flags::none` – Deaktiviert audio Manipulation. Hierbei handelt es sich um die Standardkonfiguration. In diesem Modus fließt unterbrechungsfreien Chat-Audio.
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` – Ermöglicht das vorab zu codieren audio Manipulation. In diesem Modus werden alle Chat-Audiodaten, die von lokalen Benutzern erstellt wurden vor ihrer Codierung ist über die Pipeline audio Manipulation integriert werden. Auch wenn die app nur die Chat-Audiodaten untersuchen und bearbeiten Sie es nicht von ist, ist es immer noch der Zuständigkeit der app um die unveränderte audio-Puffer an Spiel Chat 2 zu übermitteln, damit sie codiert und übertragen werden können.
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` – Ermöglicht das nach dem audio Manipulation decodiert werden. In diesem Modus werden alle Chat Audio empfangen von Remotebenutzern über die Pipeline audio Manipulation eingelesen werden, nach seiner Decodierung durch den Empfänger, aber bevor diese gerendert wird. Auch wenn die app nur die Chat-Audiodaten untersuchen und bearbeiten Sie es nicht von ist, ist es immer noch der Zuständigkeit der app zu mischen und übermitteln das unveränderte audio-Puffer an Spiel Chat 2, sodass sie gerendert werden können.

## <a name="processing-audio-stream-state-changes"></a>Verarbeiten von Änderungen am Ansichtszustand des Audiodatenstroms

Spiel, Chat 2 bietet Updates für den Status des Audiostreams über `game_chat_stream_state_change` Strukturen. Diese Updates speichern Informationen darüber, welche Stream wurde aktualisiert, und wie er aktualisiert wurde. Diese Updates können für abgerufen werden, über Aufrufe der `chat_manager::start_processing_stream_state_changes()` und `chat_manager::finish_processing_stream_state_changes()` Paar von Methoden. Dieses Paar von Methoden enthält alle aktuellen, in der Warteschlange Audiostream Statusupdates als ein Array von `game_chat_stream_state_change` Zeiger zu strukturieren. Apps sollten das Array durchlaufen, und jedes Update entsprechend zu behandeln. Einmal alle verfügbaren `game_chat_stream_state_change` Updates behandelt wurden, dieses Array übergeben werden sollte, zurück zum Spiel Chat 2 bis `chat_manager::finish_processing_stream_state_changes()`. Zum Beispiel:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
```

## <a name="manipulating-pre-encode-chat-audio-data"></a>Bearbeiten von vorab zu codieren Audiodaten Chat

Spiele-Chat-2 bietet Zugriff auf Audiodaten von Chat für lokale Benutzer über vorab zu codieren der `pre_encode_audio_stream` Klasse.

### <a name="stream-lifetime"></a>Stream-Lebensdauer
Wenn ein neuer `pre_encode_audio_stream` bereit für die app verwendet wird, übermittelt es über eine `game_chat_stream_state_change` Struktur mit seinem `state_change_type` Feld festgelegt, um `game_chat_stream_state_change_type::pre_encode_audio_stream_created`. Nachdem diese Zustandsänderung Stream Spiel, Chat 2 zurückgegeben wurde, der Audiostream zur Verfügung stehen für audio Manipulation vorab zu codieren.

Wenn ein vorhandenes `pre_encode_audio_stream` wird nicht verfügbar für audio-Bearbeitung verwenden, die app benachrichtigt werden über eine `game_chat_stream_state_change` Struktur mit seinem `state_change_type` Feld festgelegt, um `game_chat_stream_state_change_type::pre_encode_audio_stream_closed`. Dies ist der app Gelegenheit genutzt, die diesem audio Stream zugeordneten Ressourcen bereinigen. Nachdem diese Zustandsänderung Stream Spiel, Chat 2 zurückgegeben wurde, der Audiostream nicht mehr verfügbar für audio Manipulation vorab zu codieren.

Bei einem geschlossenen `pre_encode_audio_stream` aller Ressourcen zurückgegeben wurde, der Stream zerstört, und die app benachrichtigt werden über eine `game_chat_stream_state_change` Struktur mit seinem `state_change_type` Feld festgelegt, um `game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`. Keinerlei Verweise oder Zeiger auf diesen Datenstrom sollten bereinigt werden. Nachdem diese Zustandsänderung Stream Spiel, Chat 2 zurückgegeben wurde, wird der audio datenstromspeicher ungültig werden.

### <a name="stream-users"></a>Stream-Benutzer
Die Liste der Benutzer, die einem Stream zugeordnete kann überprüft werden, mithilfe von `pre_encode_audio_stream::get_users()`.

### <a name="audio-formats"></a>Audio-Formate
Das Audioformat von den Puffern, die die app von Spiel, Chat 2 ruft kann überprüft werden, mithilfe von `pre_encode_audio_stream::get_pre_processed_format()`. Das vorab verarbeitete Audioformat wird immer mono sein. Die app zu erwarten, Daten, die als 32-Bit-Gleitkommazahlen, 16-Bit-Ganzzahlen und 32-Bit-Ganzzahlen dargestellt zu behandeln.

Die app muss das Spiel Chat 2 des Audioformats manipulierte Puffer, der zum Codieren und mit der Übertragung an sie gesendet werden informieren `pre_encode_audio_stream::set_processed_format()`. Verarbeitete Formate für vorab zu codieren Audio Streams müssen diese Vorbedingungen erfüllen:

* Das Format muss mono sein.
* Das Format muss es sich um 16-Bit-Ganzzahl-PCM-Formaten, 32-Bit-Ganzzahl PCM oder 32-Bit-Gleitkommaoperationen PCM sein.
* Das Format der Abtastrate muss Vorbedingungen basierend auf seiner Plattform entsprechen. Xbox One Zeitraum unterstützt 8kHz, 12kHz, 16kHz und 24kHz Samplingraten. UWP für Xbox One und PCs unterstützt, 8kHz, 12kHz, 16kHz, 24kHz, 32kHz, 44,1 kHz und 48 kHz Samplingraten.

### <a name="retrieving-and-submitting-audio"></a>Abrufen und Senden von Audio
Apps können Abfragen vorab zu codieren Audiodatenströme für die Anzahl der verfügbaren Puffer verarbeiten `pre_encode_audio_stream::get_available_buffer_count()`. Diese Informationen können verwendet werden, wenn die app möchten verzögern audio verarbeitet werden, bevor eine Mindestzahl an Puffern verfügbar sind. Nur 10 Puffer werden in der Warteschlange auf den einzelnen Audiodatenstrom vorab zu codieren und Verzögerungen bei der audio werden Latenz bei der Datenpipeline für audio-Lösung, sodass es wird empfohlen, apps zu leeren ihre Audiodatenströme vorab zu codieren, bevor sie mehr als 4 Puffer in die Warteschlange.

Apps können abrufen, audio-Puffer, aus einer vorab zu codieren, audio Stream mit `pre_encode_audio_stream::get_next_buffer()`. Neue audio-Puffer werden einmal für jede 40ms im Durchschnitt verfügbar sein. Puffer, die von dieser Methode zurückgegebene müssen freigegeben werden, um `pre_encode_audio_stream::return_buffer()` Wenn sie fertig sind, die verwendet wird. Maximal 10 in die Warteschlange eingereiht oder unreturned Puffer können sich auch an einem beliebigen Zeitpunkt für einen Audiostream vorab zu codieren. Sobald dieser Grenzwert erreicht ist, werden neue Puffer anhand des Spielers Audioquelle erfassten erst gelöscht werden, einige der ausstehenden Puffer zurückgegeben werden.

Apps können ihre untersucht und manipulierte Puffer zurück zum Spiel, Chat 2 zum Codieren und mit der Übertragung Senden `pre_encode_audio_stream::submit_buffer()`. Spiel, Chat-2 unterstützt audio Manipulation von in-Place "und" Out durchgeführt, damit an die Puffer gesendet `pre_encode_audio_stream::submit_buffer()` unbedingt müssen nicht die gleichen Puffer aus abgerufen werden `pre_encode_audio_stream::get_next_buffer()`. Privacy/Berechtigungen für diese übermittelten Puffer wird basierend auf den Benutzern, die diesen Datenstrom zugeordnet angewendet werden. Jede 40ms, wird die nächste 40ms audioeingaben aus diesem Stream codiert und übertragen werden. Um audio Unterbrechungen zu vermeiden, sollten die Puffer für Audio, das kontinuierlich beteiligen sollten zu diesem Datenstrom mit konstanter Geschwindigkeit übermittelt werden.

### <a name="stream-contexts"></a>Stream-Kontexten
Verwalten von Apps können benutzerdefinierte Zeigergröße Kontextwerte auf vorab zu codieren Audiodatenströme mit `pre_encode_audio_stream::set_custom_stream_context()` und `pre_encode_audio_stream::custom_stream_context()`. Diese benutzerdefinierte Stream-Kontexte sind hilfreich zum Erstellen von Zuordnungen zwischen spielen Chat 2 des Audiostreams und das temporäre Daten: Metadaten, Spielzustands usw. zu streamen.

### <a name="example"></a>Beispiel
Hier ist ein vereinfachtes Beispiel für End-to-End für die Verwendung Audiostreams im Rahmen der Verarbeitung von einem Audiodaten vorab zu codieren:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            pre_encode_audio_stream* stream = streamStateChanges[streamStateChangeIndex]->pre_encode_audio_stream;
            stream->set_processed_audio_format(...);
            stream->set_custom_stream_context(...);
            HandlePreEncodeAudioStreamCreated(stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            HandlePreEncodeAudioStreamDestroyed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t preEncodeAudioStreamCount;
pre_encode_audio_stream_array preEncodeAudioStreams;
chat_manager::singleton_instance().get_pre_encode_audio_streams(&preEncodeAudioStreamCount, &preEncodeAudioStreams);
for (uint32_t preEncodeAudioStreamIndex = 0; preEncodeAudioStreamIndex < preEncodeAudioStreamCount; ++preEncodeAudioStreamIndex)
{
    pre_encode_audio_stream* stream = preEncodeAudioStreams[preEncodeAudioStreamIndex];
    StreamContext* context = reinterpret_cast<StreamContext*>(stream->custom_stream_context());

    game_chat_audio_format audio_format = stream->get_pre_processed_format();

    uint32_t preProcessedBufferByteCount;
    void* preProcessedBuffer;
    stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);

    while (preProcessedBuffer != nullptr)
    {
        void* processedBuffer = nullptr;
        switch (audio_format.bits_per_sample)
        {
            case 16:
            {
                assert (audio_format.sample_type == game_chat_sample_type::integer);
                processedBuffer = ManipulateChatBuffer<int16_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                break;
            }

            case 32:
            {
                switch (audio_format.sample_type)
                {
                    case game_chat_sample_type::integer:
                    {
                        processedBuffer = ManipulateChatBuffer<int32_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    case game_chat_sample_type::ieee_float:
                    {
                        processedBuffer = ManipulateChatBuffer<float>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    default:
                    {
                        assert(false);
                        break;
                    }
                }
                break;
            }

            default:
            {
                assert(false);
                break;
            }
        }
        // processedBuffer can be the same as preProcessedBuffer (in-place manipulation) or it can be a buffer of
        // memory not managed by Game Chat 2 (out-of-place manipulation).
        stream->submit_buffer(processedBuffer);
        // Only return buffers retrieved from Game Chat 2. Do not return foreign memory to return_buffer.
        stream->return_buffer(preProcessedBuffer);
        stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);
    }
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="manipulating-post-decode-chat-audio-data"></a>Bearbeiten von nach dem Chat-Audiodaten decodieren

Spiele-Chat-2 bietet Zugriff auf Audiodaten von Chat über nach der Decodierung der `post_decode_audio_source_stream` und `post_decode_audio_sink_stream` Klassen, damit Benutzer Audio von Remotebenutzern eindeutig für jeden lokalen Empfänger Chat-Audioformat bearbeiten können.

### <a name="sources-and-sinks"></a>Quellen und senken
Im Gegensatz zu der Pipeline Pre-encode das Modell für Umgang mit nach der Decodierung von Audiodaten aufgeteilt ist für zwei Klassen: `post_decode_audio_source_stream` und `post_decode_audio_sink_stream`. Decodierte Audio von Remotebenutzern abgerufen werden kann, von `post_decode_audio_source_stream` -Objekte, bearbeitet und gesendet, um `post_decode_audio_sink_stream` Objekte für das Rendering. Dadurch wird für die Integration zwischen Spiel Chat 2 nach dem Decodieren, audio-Verarbeitungspipeline und hilfreiche audio-Middleware.

### <a name="stream-lifetime"></a>Stream-Lebensdauer
Wenn ein neuer `post_decode_audio_source_stream` oder `post_decode_audio_sink_stream` bereit für die app verwendet wird, übermittelt es über eine `game_chat_stream_state_change` Struktur mit seinem `state_change_type` Feld festgelegt, um `game_chat_stream_state_change_type::post_decode_audio_source_stream_created` oder `game_chat_stream_state_change_type::post_decode_audio_sink_stream_created`bzw. Nachdem diese Zustandsänderung Stream Spiel, Chat 2 zurückgegeben wurde, der Audiostream zur Verfügung stehen für nach der Decodierung audio Manipulation.

Wenn ein vorhandenes `post_decode_audio_source_stream` oder `post_decode_audio_sink_stream` wird nicht verfügbar für audio-Bearbeitung verwenden, die app benachrichtigt werden über eine `game_chat_stream_state_change` Struktur mit seinem `state_change_type` Feld festgelegt, um `game_chat_stream_state_change_type::post_decode_audio_source_stream_closed` oder `game_chat_stream_state_change_type::post_decode_audio_sink_stream`bzw. Dies ist der app Gelegenheit genutzt, die diesem audio Stream zugeordneten Ressourcen bereinigen. Nachdem diese Zustandsänderung Stream Spiel, Chat 2 zurückgegeben wurde, des Audiodatenstroms nicht mehr verfügbar ist, für die nach der Decodierung audio Manipulation. Quellstreams bedeutet dies, dass keine weiteren Datenpuffer für Bearbeitung in Warteschlangen gestellt werden. Für die Senke Streams bedeutet dies, dass es sich bei, die gesendet wird, dass der Puffer nicht mehr gerendert werden soll.

Bei einem geschlossenen `post_decode_audio_source_stream` oder `post_decode_audio_sink_stream` aller Ressourcen zurückgegeben wurde, der Stream zerstört, und die app benachrichtigt werden über eine `game_chat_stream_state_change` Struktur mit seinem `state_change_type` Feld festgelegt, um `game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed` oder `game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed`, bzw. Keinerlei Verweise oder Zeiger auf diesen Datenstrom sollten bereinigt werden. Nachdem diese Zustandsänderung Stream Spiel, Chat 2 zurückgegeben wurde, wird der audio datenstromspeicher ungültig werden.

### <a name="stream-users"></a>Stream-Benutzer
Die Liste der Remotebenutzer Quelldatenstrom Post-decode zugeordneten kann überprüft werden, mithilfe von `post_decode_audio_source_stream::get_users()`. Die Liste der lokalen Benutzer, die einem Post-decode Senke Stream zugeordnete kann überprüft werden, mithilfe von `post_decode_audio_sink_stream::get_users()`.

### <a name="audio-formats"></a>Audio-Formate
Das Audioformat von den Puffern, die die app von Spiel, Chat 2 ruft kann überprüft werden, mithilfe von `post_decode_audio_source_stream::get_pre_processed_format()`. Das vorab verarbeitete Audioformat wird immer mit mono, 16-Bit-Ganzzahl PCM sein.

Die app muss das Spiel Chat 2 des Audioformats manipulierte Puffer, die für die Verwendung von Rendering an ihn gesendet werden informieren `post_decode_audio_sink_stream::set_processed_format()`. Verarbeitete Formate für nach der Decodierung Audio Senke Streams müssen diese Vorbedingungen erfüllen:

* Das Format muss weniger als 64 Kanäle aufweisen.
* Das Format muss es sich um 16-Bit-Ganzzahl PCM (optimal), 20-Bit-Ganzzahl PCM (in einem 24-Bit-Container), 24-Bit-Ganzzahl PCM, 32-Bit-Ganzzahl PCM oder 32-Bit-Gleitkommaoperationen PCM (bevorzugte Format nach 16-Bit-Ganzzahl, PCM) sein. 
* Das Format der Abtastrate muss zwischen 1000 und 200000 Samples pro Sekunde liegen.

### <a name="retrieving-and-submitting-audio"></a>Abrufen und Senden von Audio
Apps können Abfragen nach der Decodierung Audioquelle Streams für die Anzahl der verfügbaren Puffer verarbeiten `post_decode_audio_source_stream::get_available_buffer_count()`. Diese Informationen können verwendet werden, wenn die app möchten verzögern audio verarbeitet werden, bevor eine Mindestzahl an Puffern verfügbar sind. Nur 10 Puffer werden in der Warteschlange für jedes nach der Decodierung Audioquelle Stream und audio Verzögerungen werden Latenz bei der Datenpipeline für audio-Lösung, sodass es wird empfohlen, apps zu leeren ihre nach der Audiodatenströme decodieren, bevor sie mehr als 4 Puffer in die Warteschlange.

Apps können audio-Puffer aus abrufen, eine nach der Decodierung Audioquelle Stream mit `post_decode_audio_source_stream::get_next_buffer()`. Neue audio-Puffer werden einmal für jede 40ms im Durchschnitt verfügbar sein. Puffer, die von dieser Methode zurückgegebene müssen freigegeben werden, um `post_decode_audio_source_stream::return_buffer()` Wenn sie fertig sind, die verwendet wird. Maximal 10 in die Warteschlange eingereiht oder unreturned Puffer können sich auch an einem beliebigen Zeitpunkt für eine nach der Decodierung audio Quelldatenstrom. Sobald dieser Grenzwert erreicht ist, werden neue entschlüsselte Puffer aus dem remote-Player erst gelöscht werden, einige der ausstehenden Puffer zurückgegeben werden.

Apps können übermitteln ihre untersucht und manipulierte Puffer an Spiel Chat 2 bis decodieren nach dem audio Senke Streams für die Verwendung von Rendering `post_decode_audio_sink_stream::submit_mixed_buffer()`. Spiel, Chat-2 unterstützt audio Manipulation von in-Place "und" Out durchgeführt, damit an die Puffer gesendet `post_decode_audio_sink_stream::submit_mixed_buffer()` unbedingt müssen nicht die gleichen Puffer aus abgerufen werden `post_decode_audio_source_stream::get_next_buffer()`. Jede 40ms, wird die nächste 40ms audioeingaben aus diesem Stream gerendert werden. Um audio Unterbrechungen zu vermeiden, sollten die Puffer für Audio, das kontinuierlich beteiligen sollten zu diesem Datenstrom mit konstanter Geschwindigkeit übermittelt werden.

### <a name="privacy-and-mixing"></a>Datenschutz und die Verwendung von
Aufgrund der Post-decode Pipeline Quelle-Senke Modell ist es der Zuständigkeit der app zum Mischen von der Puffern, die aus abgerufen `post_decode_audio_source_stream` Objekte aus, und Übermitteln von gemischten Puffer `post_decode_audio_sink_stream` Objekte für das Rendering. Dies bedeutet auch, dass es der Zuständigkeit der app zum Ausführen der Mischung ordnungsgemäßen Schutz und die Berechtigungen, die erzwungen wird. Spiel, Chat 2 bietet `post_decode_audio_sink_stream::can_receive_audio_from_source_stream()` um Abfragen für diese Informationen einfach und effizient zu machen.

### <a name="chat-indicators"></a>Chat-Indikatoren

Nach der Decodierung Audio Manipulation wird keine Auswirkungen auf das Indikatorstatus "Chat" für jeden Benutzer. Z. B. wenn ein Remotebenutzer stumm geschaltet wird, das Audio für die app bereitgestellt wechseln, aber des Chat-Indikators für diesen Remotebenutzer wird immer noch stumm geschaltet sind. Wenn ein Remotebenutzer austauscht, deren Audio bereitgestellt werden, aber der Chat-Indikator wird angegeben, sprechen, unabhängig davon, ob die app eine audio Mischung mit Audiodaten von diesem Benutzer bereitstellt. Weitere Informationen zu der Benutzeroberfläche und der Chat-Indikator, finden Sie unter [mithilfe Spiel Chat 2](using-game-chat-2.md#ui). Wenn zusätzliche app-spezifische Einschränkungen verwendet werden, um zu bestimmen, welche Benutzer in einer audio Mischung vorhanden sind, ist es der Zuständigkeit der app die gleichen Einschränkungen zu berücksichtigen, beim Lesen der Chat-Indikatoren, die von Spiel Chat 2 bereitgestellt wird.

### <a name="stream-contexts"></a>Stream-Kontexten
Verwalten von Apps können benutzerdefinierte Zeigergröße Kontextwerte auf Audiodatenströme mit nach der Decodierung der `set_custom_stream_context()` und `custom_stream_context()` Methoden. Diese benutzerdefinierte Stream-Kontexte sind hilfreich zum Erstellen von Zuordnungen zwischen spielen Chat 2 des Audiostreams und das temporäre Daten: Metadaten, Spielzustands usw. zu streamen.

### <a name="example"></a>Beispiel
Hier ist ein vereinfachtes Beispiel für End-to-End für die Verwendung nach der Decodierung Audiostreams im Rahmen der Verarbeitung von einem Audiodaten:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::post_decode_audio_source_stream_created:
        {
            post_decode_audio_source_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_source_stream;
            stream->set_custom_stream_context(...);
            HandlePostDecodeAudioSourceStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_closed:
        {
            HandlePostDecodeAudioSourceStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed:
        {
            HandlePostDecodeAudioSourceStreamDestroyed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_created:
        {
            post_decode_audio_sink_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_sink_stream;
            stream->set_custom_stream_context(...);
            stream->set_processed_format(...);
            HandlePostDecodeAudioSinkStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_closed:
        {
            HandlePostDecodeAudioSinkStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed:
        {
            HandlePostDecodeAudioSinkStreamDestroyed(stream);
            break;
        }

        ...
    }
}

chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t sourceStreamCount;
post_decode_audio_source_stream_array sourceStreams;
chatManager::singleton_instance().get_post_decode_audio_source_streams(&sourceStreamCount, &sourceStreams);

uint32_t sinkStreamCount;
post_decode_audio_sink_stream_array sinkStreams;
chatManager::singleton_instance().get_post_decode_audio_sink_streams(&sinkStreamCount, &sinkStreams);

//
// MixBuffer is a custom type defined as:
// struct MixBuffer
// {
//     uint32_t bufferByteCount;
//     void* buffer;
// };
//
std::vector<std::pair<post_decode_audio_source_stream*, MixBuffer>> cachedSourceBuffers;

for (uint32_t sourceStreamIndex = 0; sourceStreamIndex < sourceStreamCount; ++sourceStreamIndex)
{
    post_decode_audio_source_stream* sourceStream = sourceStreams[sourceStreamIndex];

    MixBuffer mixBuffer;
    sourceStream->get_next_buffer(&mixBuffer.bufferByteCount, &mixBuffer.buffer);
    if (buffer != nullptr)
    {
        // Stash the buffer to return after we are done with mixing. If this program was using audio middleware, now
        // would be an appropriate time to plumb the buffer through the middleware
        cachedSourceBuffer.push_back(std::pair<post_decode_audio_source_stream*, MixBuffer>{sourceStream, mixBuffer});
    }
}

// Loop over each sink stream, perform mixing, and submit
for (uint32_t sinkStreamIndex = 0; sinkStreamIndex < sinkStreamCount; ++sinkStreamIndex)
{
    post_decode_audio_sink_stream* sinkStream = sinkStreams[sinkStreamIndex];

    if (sinkStream->is_open())
    {
        std::vector<std::pair<MixBuffer, float>> buffersToMixForThisStream;

        for (const std::pair<post_decode_audio_source_stream, MixBuffer>& sourceBufferPair : cachedSourceBuffers)
        {
            float volume;
            if (sinkStream->can_receive_audio_from_source_stream(sourceBufferPair.first, &volume))
            {
                buffersToMixForThisStream.push_back(std::pair<MixBuffer, float>{sourceBufferPair.second, volume});
            }
        }

        if (buffersToMixForThisStream.size() > 0)
        {
            uint32_t mixedBufferByteCount;
            uint8_t* mixedBuffer;
            MixPostDecodeBuffers(buffersToMixForThisStream, &mixedBufferByteCount, &mixedBuffer);
            sinkStream->submit_mixed_buffer(mixedBufferByteCount, mixedBuffer);
        }
    }
}

// Return buffers after mix and submission
for (const std::pair<post_decode_audio_source_stream*, MixBuffer>& cachedSourceBuffer : cachedSourceBuffers)
{
    post_decode_audio_source_stream* sourceStream = cachedSourceBuffer.first;
    void* bufferToReturn = cachedSourceBuffer.second.buffer;
    sourceStream->return_buffer(bufferToReturn);
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="chat-user-lifetimes"></a>Lebensdauer der Chat-Benutzer

Aktivieren in Echtzeit audio Bearbeitung wirkt sich auf die Lebensdauer der Chat-Benutzer aus. Wenn `chat_manager::remove_user(chatUserX)` aufgerufen wird, wird die Chat_user Objekt ChatUserX bleibt gültig, bis alle Audiodatenströme, die ChatUserX verweisen zerstört wurden. Stellen Sie sich das folgende Szenario vor:

```cpp
// At somepoint a chat user, chatUserX, leaves the game session.
chat_manager::singleton_instance().remove_user(chatUserX);

// chatUserX is still valid, but to avoid further synchronization, prevent non-audio-stream use of chatUserX
chatUserX = nullptr;

// On the audio processing thread...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        // All of the streams associated with chatUserX will close.
        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            CleanupPreEncodeAudioStreamResources(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

// The next time the app processes stream state changes...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            uint32_t chatUserCount;
            Xs::game_chat_2::chat_user_array chatUsers;
            streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream->get_users(&chatUserCount, &chatUsers);
            assert(chatUserCount != 0);
            for (uint32_t chatUserIndex = 0; chatUserIndex < chatUserCount; ++chatUserIndex)
            {
                // chat_user objects such as chatUserX will still be valid while the destroyed state change is being processed.
                Log(chatUsers[chatUserIndex]->xbox_user_id());
            }
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
// Once the all destroyed state changes have been processed for all streams associated with chatUserX, it's memory will be invalidated.
// Do not call methods on chatUserX, e.g. chatUserX->xbox_user_id()
```
