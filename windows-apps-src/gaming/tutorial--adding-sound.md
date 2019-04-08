---
title: Hinzufügen von Sound
description: Entwickeln Sie eine einfache sound-Engine mit XAudio2-APIs können Spiele Musik wiedergeben und Soundeffekten.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Sound
ms.localizationpriority: medium
ms.openlocfilehash: 8d5a976ef65bee5efc3329afc98bf198d094b037
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589935"
---
# <a name="add-sound"></a>Hinzufügen von Sound

In diesem Thema erstellen wir eine einfache sound-Engine mit [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) APIs. Wenn Sie noch nicht mit sind __XAudio2__, wir haben eine kurze Einführung unter enthalten [Audio-Konzepte](#audio-concepts).

>[!Note]
>Wenn Sie den neuesten Code für dieses Beispiel noch nicht heruntergeladen haben, wechseln Sie zu [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Ziel

Hinzufügen von Sounds in der Beispiel-Spiel verwenden [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction).

## <a name="define-the-audio-engine"></a>Definieren Sie das audio-Modul

Im beispielhaften Spiel werden die Audioobjekte und -verhaltensweisen in drei Dateien definiert:

* __[Audio.h](#audioh)/.cpp__: Definiert die __Audio__ -Objekt, das enthält die __XAudio2__ Ressourcen für die Soundwiedergabe. Außerdem definiert sie die Methode zum Anhalten und Fortsetzen der Audiowiedergabe, wenn das Spiel angehalten oder deaktiviert wurde.
* __[MediaReader.h](#mediareaderh)/.cpp__: Definiert die Methoden zum Lesen von audio WAV-Dateien aus dem lokalen Speicher.
* __[SoundEffect.h](#soundeffecth)/.cpp__: Definiert ein Objekt im Spiel Soundwiedergabe.

## <a name="overview"></a>Übersicht

Es gibt drei Hauptkomponenten in Einrichtung für die Audiowiedergabe in Ihr Spiel.

1. [Erstellen Sie und initialisieren Sie die audio-Ressourcen](#create-and-initialize-the-audio-resources)
2. [Audio-Datei laden](#load-audio-file)
3. [Zuordnen des Sound-Objekt](#associate-sound-to-object)

Sie sind alle definiert, der [Simple3DGame::Initialize](#simple3dgameinitialize-method) Methode. Lassen Sie uns zunächst untersuchen diese Methode, und klicken Sie dann weitere Details in den Abschnitten im Detail.

Nach dem einrichten, erfahren wir, wie die Auswirkungen der Sound wiedergeben auslösen. Weitere Informationen finden Sie unter [der Klang wiedergegeben](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize-Methode

In __Simple3DGame::Initialize__, wobei __m\_Controller__ und __m\_Renderer__ werden initialisiert, wir richten Sie das audio-Modul und herunterladen Möchten Sie die Sounds wiedergeben.

 * Erstellen Sie __m\_AudioController__, dies ist eine Instanz von der [Audio](#audioh) Klasse.
 * Erstellen von audio erforderlichen Ressourcen mithilfe der [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) Methode. Hier wird zwei __XAudio2__ Objekte &mdash; eine Musik-Engine-Objekt und ein sound-Engine-Objekt und eine Perfektion Stimme für jede von ihnen erstellt wurden. Die Musik-Engine-Objekt kann verwendet werden, um Hintergrundmusik für Ihr Spiel zu spielen. Soundeffekte in Ihr Spiel zu spielen, kann die sound-Engine verwendet werden. Weitere Informationen finden Sie unter [erstellen und initialisieren Sie die audio Ressourcen](#create-and-initialize-the-audio-resources).
 * Erstellen Sie __MediaReader__, dies ist eine Instanz von [MediaReader](#mediareaderh) Klasse. [MediaReader](#mediareaderh), dies ist eine Hilfsklasse für die [SoundEffect](#soundeffecth) -Klasse Lesevorgänge kleine Audio synchron aus dem Speicherort der Dateien und sound Daten als ein Bytearray zurückgibt.
 * Verwendung [MediaReader::LoadMedia](#mediareaderloadmedia-method) Audiodateien aus seinem Speicherort zu laden, und erstellen eine __TargetHitSound__ Variable zum Speichern der geladenen WAV-sound-Daten. Weitere Informationen finden Sie unter [Load Audiodatei](#load-audio-file). 

Soundeffekte sind das spielobjekt zugeordnet. Daher tritt ein Konflikt mit diesem Spiel-Objekt, löst er den Soundeffekt wiedergegeben werden soll. In diesem Spiel Beispiel haben wir Soundeffekte, die für die Ammo (was wir verwenden dafür Ziele mit) und für das Ziel. 
    
* In der __"gameobject"__ Klasse, gibt es eine __HitSound__ -Eigenschaft, die zum Zuordnen des Soundeffekts auf das Objekt verwendet wird.
* Erstellen Sie eine neue Instanz der dem [SoundEffect](#soundeffecth) Klasse, und initialisieren Sie es. Während der Initialisierung wird eine Stimme Quelle für den Soundeffekt erstellt. 
* Diese Klasse spielt einen Sound mithilfe von Perfektion Sprachnotizen bereitgestellt, die von der [Audio](#audioh) Klasse. Sounddaten auslesen Speicherort mithilfe der [MediaReader](#mediareaderh) Klasse. Weitere Informationen finden Sie unter [Sound-Objekt zuordnen](#associate-sound-to-object).

>[!Note]
>Der tatsächliche Trigger den Sound wiedergegeben wird durch das Verschieben und Konflikte zwischen diesen Spielobjekte bestimmt. Der Aufruf tatsächlich diese Sounds wiedergegeben werden daher in definiert die [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) Methode. Weitere Informationen finden Sie unter [der Klang wiedergegeben](#play-the-sound).

```cpp
void Simple3DGame::Initialize(
    _In_ MoveLookController^ controller,
    _In_ GameRenderer^ renderer
    )
{
    // ...
    // Create a new Audio class object.
    m_audioController = ref new Audio;

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController->CreateDeviceIndependentResources();

    m_ammo = std::vector<Sphere^>(GameConstants::MaxAmmo);

    // ..
    // Create a media reader which is used to read audio files from its file location.
    MediaReader^ mediaReader = ref new MediaReader;
    auto targetHitSound = mediaReader->LoadMedia("Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        // ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(ref new SoundEffect());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            targetHitSound
            );
        // ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader->LoadMedia("Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = ref new Sphere;
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(ref new SoundEffect());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    // ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Erstellen Sie und initialisieren Sie die audio-Ressourcen

* Verwendung [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212), eine XAudio2-API, um zwei neue XAudio2-Objekte erstellt, die die – Musik und Sound-Wirkung-Engines zu definieren. Diese Methode gibt einen Zeiger auf des Objekts des [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) -Schnittstelle, die alle Audiomodul verwaltet angibt, das Audio processing Thread, der Voice-Diagramm und mehr.
* Nachdem die Engines instanziiert wurden, verwenden Sie [IXAudio2::CreateMasteringVoice](https://msdn.microsoft.com/library/windows/desktop/hh405048) eine Perfektion Stimme für die einzelnen Objekte der sound-Engine zu erstellen.

Weitere Informationen finden Sie unter [Vorgehensweise: Initialisieren Sie XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx).

### <a name="audiocreatedeviceindependentresources-method"></a>Audio::CreateDeviceIndependentResources-Methode

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Audio-Datei laden

In diesem Beispiel Spielen ist in der Code zum Lesen von audio-Format-Dateien definiert [MediaReader.h](#mediareaderh)/cpp__.  Um eine codierte WAV-Audiodatei zu lesen, rufen Sie [MediaReader::LoadMedia](#mediareaderloadmedia-method), und der Dateiname der die WAV-Datei als Eingabeparameter übergeben.

### <a name="mediareaderloadmedia-method"></a>MediaReader::LoadMedia-Methode

Diese Methode verwendet die [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197)-APIs, um die WAV-Audiodatei als Pulse Code Modulation (PCM)-Puffer einzulesen.

#### <a name="set-up-the-source-reader"></a>Der Quellreader einrichten

1. Verwendung [MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110) , erstellen Sie einen Mediensatz quellreader ([IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655)).
2. Verwendung [MFCreateMediaType](https://msdn.microsoft.com/library/windows/desktop/ms693861) erstellen Sie einen Medientyp ([IMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms704850)) Objekt (_MediaType_). Es stellt eine Beschreibung der Media-Format dar. 
3. Angeben, die die _MediaType_der Ausgabe decodiert wird PCM-Audio, d.h. ein Audio eingeben, die __XAudio2__ verwenden können.
4. Legt die entschlüsselte Ausgabe für der quellreader durch Aufrufen von Medientyp [IMFSourceReader::SetCurrentMediaType](https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx).

Weitere Informationen dazu, warum wir der Quellreader finden Sie unter [Quellreader](https://msdn.microsoft.com/library/windows/desktop/dd940436.aspx).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Beschreiben Sie das Datenformat des Audiostreams

1. Verwendung [IMFSourceReader::GetCurrentMediaType](https://msdn.microsoft.com/library/windows/desktop/dd374660) beim Abrufen des aktuellen Medien-Typs für den Stream.
2. Verwenden [IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms702177) Konvertieren der aktuellen audio Medientyp, um eine [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) zu Puffern, verwenden die Ergebnisse des Vorgangs weiter oben als Eingabe. Diese Struktur gibt an, das Datenformat des Audiostreams Wave, das verwendet wird, nach dem Audio geladen wird. 

Die __WAVEFORMATEX__ Format zum Beschreiben des PCM-Puffers verwendet werden kann. Verglichen mit der [WAVEFORMATEXTENSIBLE](https://msdn.microsoft.com/library/windows/hardware/ff538802) Struktur kann nur eine Teilmenge von Audio-Wave-Format beschrieben verwendet werden. Weitere Informationen zu den Unterschieden zwischen __WAVEFORMATEX__ und __WAVEFORMATEXTENSIBLE__, finden Sie unter [Extensible Wave-Format-Deskriptoren](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Lesen des Audiodatenstroms

1.  Erhalten Sie die Dauer in Sekunden, des Audiostreams durch Aufrufen von [IMFSourceReader::GetPresentationAttribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) und dann die Dauer in Bytes konvertiert.
2.  Lesen die Audiodatei im als Datenstrom durch Aufrufen von [IMFSourceReader::ReadSample](https://msdn.microsoft.com/library/windows/desktop/dd374665). __ReadSample__ liest das nächste Beispiel aus der Media-Quelle.
3.  Verwendung [IMFSample::ConvertToContiguousBuffer](https://msdn.microsoft.com/library/windows/desktop/ms698917.aspx) , den Inhalt des Puffers audioabtastwert kopieren (_Beispiel_) in ein Array (_MediaBuffer_).

```cpp
Platform::Array<byte>^ MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );
    
    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.Get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
        Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), &outputMediaType)
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(static_cast<uint32>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        //...
        // Read audio data.
    }

    return fileData;
}
```
## <a name="associate-sound-to-object"></a>Zuordnen des Sound-Objekt

Zuordnen von Sounds mit dem Objekt findet statt, wenn das Spiel initialisiert werden, in der [Simple3DGame::Initialize](#simple3dgameinitialize-method) Methode.

Zusammenfassung:
* In der __"gameobject"__ Klasse, gibt es eine __HitSound__ -Eigenschaft, die zum Zuordnen des Soundeffekts auf das Objekt verwendet wird.
* Erstellen Sie eine neue Instanz der dem [SoundEffect](#soundeffecth) Klasse Objekt und das spielobjekt zuzuordnen. Diese Klasse spielt einen sound mithilfe __XAudio2__ APIs.  Er verwendet eine Perfektion Stimme gebotenen die [Audio](#audioh) Klasse. Der sound Daten gelesen werden können, aus Speicherort mithilfe der [MediaReader](#mediareaderh) Klasse.

[SoundEffect::Initialize](#soundeffectinitialize-method) dient zur Initialisierung der __SoundEffect__ -Instanz mit der folgenden Eingabeparameter: Zeiger auf den sound-Engine-Objekt (IXAudio2-Objekte, erstellt die [Audio:: "Createdeviceindependentresources"](#audiocreatedeviceindependentresources-method) Methode), Zeiger auf das Formatieren der WAV-Datei mit __MediaReader::GetOutputWaveFormatEx__, und die Daten geladen, mit [MediaReader::LoadMedia ](#mediareaderloadmedia-method) Methode. Während der Initialisierung wird die Quelle Stimme für den Soundeffekt auch erstellt werden.

### <a name="soundeffectinitialize-method"></a>SoundEffect::Initialize-Methode

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Der Klang wiedergegeben

Trigger Soundeffekte in definiert [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) Methode, da es sich handelt, in dem Verschieben der Objekte aktualisiert werden und Konflikte zwischen Objekten bestimmt ist.

Da die Interaktion zwischen Objekten erheblich, je nachdem das Spiel, unterscheidet sich sind wir nicht die Dynamik von hier die Spielobjekte zu diskutieren. Wenn Sie auf ihre Implementierung zu verstehen möchten, wechseln Sie zu [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) Methode.

Im Prinzip, tritt ein Konflikt auf, es wird ausgelöst, den Soundeffekt durch Aufrufen von Spielen **SoundEffect::PlaySound**. Diese Methode beendet alle Soundeffekte, die in die Warteschlange von der in-Memory-Puffer mit den gewünschten solide Daten und ist aktuell wiedergegebenen. Quelle verwendet, um das Volume festzulegen, solide Daten senden und die Wiedergabe zu starten.

### <a name="soundeffectplaysound-method"></a>SoundEffect::PlaySound-Methode

* Verwendet das Quellobjekt für den Voice **m\_SourceVoice** zu die Wiedergabe von sound Datenpuffer **m\_SoundData**
* Erstellt eine [XAUDIO2\_Puffer](https://msdn.microsoft.com/library/windows/desktop/ee419228), um die es enthält einen Verweis auf den sound Datenpuffer, und sendet sie durch einen Aufruf von [IXAudio2SourceVoice::SubmitSourceBuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473). 
* Mit den sound Daten in einer Warteschlange und **SoundEffect::PlaySound** beginnt durch Aufrufen von wiedergeben [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471).

```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics-Methode

Die __Simple3DGame::UpdateDynamics__ Methode übernimmt die Interaktion und ein Konflikt zwischen Spielobjekte. Wenn Objekte in Konflikt stehen (oder intersect-), löst er den zugehörigen Soundeffekt spielen.

```cpp
void Simple3DGame::UpdateDynamics()
{
    // ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
    if (m_ammoCount > 1)
    {
       // ...
       // Check collision between instances One and Two.
       // ...
       if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
       {
           // The two ammo are intersecting.
           // ...
           // Start playing the sounds for the impact between the two balls.
              m_ammo[one]->PlaySound(impact, m_player->Position());
              m_ammo[two]->PlaySound(impact, m_player->Position());

       }
    }
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.
    if (impact > 0.0f)
    {
        // ...
        // Play the sound associated with the Ammo hitting something.
           m_ammo[one]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark it as hit and
            // play the sound associated with the impact.
             m_objects[i]->Hit(true);
             m_objects[i]->HitTime(timeTotal);
             m_totalHits++;

             m_objects[i]->PlaySound(impact, m_player->Position());
        }
        // ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
    // Apply gravity and check for collision against enclosing volume.
    // ...
    if (position.z < limit)
    {
        // The ammo instance hit the a wall in the min Z direction.
        // Align the ammo instance to the wall, invert the Z component of the velocity and
        // play the impact sound.
           position.z = limit;
           m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
           velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
    }
    // ...
#pragma endregion
}
```
## <a name="next-steps"></a>Nächste Schritte

Wir haben die UWP Framework, Grafiken, Steuerelemente, Benutzeroberfläche und Audio für ein Spiel für Windows 10 behandelt. Der nächste Teil dieses Lernprogramms, [erweitert das Beispiel](tutorial-resources.md), wird erläutert, andere Optionen, die verwendet werden können, wenn Sie ein Spiel zu entwickeln.

## <a name="audio-concepts"></a>Audio-Konzepte

Verwenden Sie für Windows 10-Spiele-Entwicklung XAudio2 Version 2.9 ein. Diese Version ist im Lieferumfang Windows 10. Weitere Informationen finden Sie unter [XAudio2 Versionen](https://msdn.microsoft.com/library/windows/desktop/ee415802.aspx).

__AudioX2__ ist eine Low-Level-API, die signalverarbeitung, und Kombinieren von Foundation bereitstellt. Weitere Informationen finden Sie unter [XAudio2-Schlüsselkonzepte](https://msdn.microsoft.com/library/windows/desktop/ee415764.aspx).

### <a name="xaudio2-voices"></a>XAudio2 stimmen

Es gibt drei Arten von XAudio2-Voice-Objekten: Quelle, Submix und stimmen verwenden. Stimmen werden die XAudio2-Objekte zu verarbeiten, bearbeiten und zum Wiedergeben der Audiodaten verwenden. 
* Quellstimmen verarbeiten die vom Client bereitgestellten Audiodaten. 
* Quell- und Submixstimmen senden ihre Ausgabe an mindestens eine Submix- oder Masterstimme. 
* Submix- und Masterstimmen mischen die Audiodaten aller Stimmen, von denen sie Daten erhalten, und verarbeiten das Ergebnis. 
* Mastering stimmen empfangen von Daten aus der Datenquelle stimmen und Submix stimmen, und sendet diese Daten an die audio-Hardware.

Weitere Informationen finden Sie unter [XAudio2 stimmen](/windows/desktop/xaudio2/xaudio2-voices).

### <a name="audio-graph"></a>Audiograph

Audiograph ist eine Sammlung von [XAudio2 stimmen](/windows/desktop/xaudio2/xaudio2-voices). Audio auf einer Seite von einem audiograph in Quelle stimmen beginnt, durchläuft optional eine oder mehrere Submix stimmen, und endet mit einer Perfektion Stimme. Ein audiograph enthält eine Stimme Quelle für jede Sound wiedergeben, NULL oder mehr Submix stimmen, und eine Perfektion Stimme an. Die einfachste audiograph sowie die Mindestanforderung für die keine Geräusche in XAudio2 ist eine einzelne Quelle Stimme direkt an eine Perfektion Stimme ausgeben. Weitere Informationen finden Sie unter [Audio Diagramme](https://msdn.microsoft.com/library/windows/desktop/ee415739.aspx).

### <a name="additional-reading"></a>Zusätzliches Lesematerial

* [So wird es gemacht: Initialisieren Sie XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)
* [So wird es gemacht: Laden von Dateien von Audiodaten in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [So wird es gemacht: Wiedergeben von Sound mit XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415787.aspx)

## <a name="key-audio-h-files"></a>Wichtige audio .h-Dateien

### <a name="audioh"></a>Audio.h

```cpp
// Audio:
// This class uses XAudio2 to provide sound output.  It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    // ...
};
```
### <a name="mediareaderh"></a>MediaReader.h

```cpp
// MediaReader:
// This is a helper class for the SoundEffect class.  It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// byte array.

ref class MediaReader
{
internal:
    MediaReader();

    Platform::Array<byte>^          LoadMedia(_In_ Platform::String^ filename);
    WAVEFORMATEX*                   GetOutputWaveFormatEx();

protected private:
    Windows::Storage::StorageFolder^ m_installedLocation;
    Platform::String^               m_installedLocationPath;
    WAVEFORMATEX                    m_waveFormat;
};
```
### <a name="soundeffecth"></a>SoundEffect.h

```cpp
// SoundEffect:
// This class plays a sound using XAudio2.  It uses a mastering voice provided
// from the Audio class.  The sound data can be read from disk using the MediaReader
// class.

ref class SoundEffect
{
internal:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected private:
    //...

};
```


