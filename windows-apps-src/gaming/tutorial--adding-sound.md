---
title: Hinzufügen von Sound
description: Entwickeln Sie eine einfache Sound-Engine mithilfe von XAudio2-APIs, um Spiele Musik und Soundeffekte wiederzugeben.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Sound
ms.localizationpriority: medium
ms.openlocfilehash: 04a9ea70914be3c60826df8753eca2ad1c30f19d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175184"
---
# <a name="add-sound"></a>Hinzufügen von Sound

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

In diesem Thema erstellen wir mithilfe von [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction) -APIs eine einfache Sound-Engine. Wenn Sie noch nicht mit __XAudio2__vertraut sind, haben wir eine kurze Einführung in [audiokonzepte](#audio-concepts)integriert.

>[!Note]
>Wenn Sie den neuesten Spiel Code für dieses Beispiel nicht heruntergeladen haben, besuchen Sie das [Direct3D-Beispiel Spiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel ist Teil einer großen Auflistung von UWP-Funktions Beispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter herunterladen [der UWP-Beispiele von GitHub](../get-started/get-app-samples.md).

## <a name="objective"></a>Ziel

Fügen Sie mit [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)Sounds in das Beispiel Spiel ein.

## <a name="define-the-audio-engine"></a>Definieren der Audioengine

Im Beispiel Spiel sind die Audioobjekte und-Verhaltensweisen in drei Dateien definiert:

* __[Audio. h](#audioh)/.cpp__: definiert das __Audioobjekt__ , das die __XAudio2__ -Ressourcen für die Audiowiedergabe enthält. Außerdem definiert sie die Methode zum Anhalten und Fortsetzen der Audiowiedergabe, wenn das Spiel angehalten oder deaktiviert wurde.
* __ [Mediareader. h](#mediareaderh)/.cpp__: definiert die Methoden zum Lesen von AudioWav-Dateien aus lokalem Speicher.
* __ [Sounerffect. h](#soundeffecth)/.cpp__: definiert ein Objekt für die in-Game-Soundwiedergabe.

## <a name="overview"></a>Übersicht

Es gibt drei Hauptbestandteile bei der Einrichtung der Audiowiedergabe in Ihrem Spiel.

1. [Erstellen und Initialisieren der Audioressourcen](#create-and-initialize-the-audio-resources)
2. [Audiodatei laden](#load-audio-file)
3. [Dem Objekt einen Sound zuordnen](#associate-sound-to-object)

Sie sind alle in der [Simple3DGame:: Initialize](#simple3dgameinitialize-method) -Methode definiert. Betrachten wir diese Methode zunächst und ausführlichere Informationen zu den einzelnen Abschnitten.

Nach dem Einrichten von erfahren Sie, wie Sie die Soundeffekte für die Wiedergabe auslöst. Weitere Informationen finden Sie unter [Abspielen des Sounds](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame:: Initialize-Methode

In __Simple3DGame:: Initialize__, wobei __m \_ Controller__ und __m- \_ Renderer__ ebenfalls initialisiert werden, richten wir die Audioengine ein und stellen Sie für die Wiedergabe von Sounds bereit.

 * Erstellen Sie __m \_ Audiocontroller__, bei dem es sich um eine Instanz der [audioklasse](#audioh) handelt.
 * Erstellen Sie die benötigten Audioressourcen mithilfe der Methode " [Audio:: createdevicindependentresources](#audiocreatedeviceindependentresources-method) ". Hier sind zwei __XAudio2__ &mdash; -Objekte, die ein Musik-Engine-Objekt und ein Sound-Engine-Objekt und eine Mastering-Stimme für jedes dieser Objekte erstellt wurden. Das Music-Engine-Objekt kann verwendet werden, um Hintergrundmusik für Ihr Spiel wiederzugeben. Die Sound-Engine kann verwendet werden, um Soundeffekte in Ihrem Spiel zu spielen. Weitere Informationen finden Sie unter [Erstellen und Initialisieren der Audioressourcen](#create-and-initialize-the-audio-resources).
 * Erstellen Sie __Mediareader__, bei dem es sich um eine Instanz der [Mediareader](#mediareaderh) -Klasse handelt. [Mediareader](#mediareaderh), eine Hilfsklasse für die [SoundEffect](#soundeffecth) -Klasse, liest kleine Audiodateien synchron aus dem Datei Speicherort und gibt Audiodaten als Bytearray zurück.
 * Verwenden Sie [Mediareader:: loadmedia](#mediareaderloadmedia-method) , um Sounddateien aus ihrem Speicherort zu laden und eine __targethitsound__ -Variable zu erstellen, die die geladenen WAV-Audiodaten enthält. Weitere Informationen finden Sie unter [Laden der Audiodatei](#load-audio-file). 

Sound Effekte werden dem Spielobjekt zugeordnet. Wenn also eine Kollision mit dem Spielobjekt auftritt, wird der Soundeffekt ausgelöst, der wiedergegeben wird. In diesem Beispiel Spiel haben wir Soundeffekte für den Ammo (was wir zum Erreichen von Zielen verwenden) und für das Ziel. 
    
* In der __gameobject__ -Klasse gibt es eine __hitsound__ -Eigenschaft, die verwendet wird, um den Soundeffekt dem-Objekt zuzuordnen.
* Erstellen Sie eine neue Instanz der [SoundEffect](#soundeffecth) -Klasse, und initialisieren Sie Sie. Während der Initialisierung wird eine Quell Stimme für den Soundeffekt erstellt. 
* Diese Klasse spielt einen Sound mithilfe einer von der [Audio-](#audioh) Klasse bereitgestellten Mastering-Stimme ab. Audiodaten werden mithilfe der [Mediareader](#mediareaderh) -Klasse aus dem Datei Speicherort gelesen. Weitere Informationen finden Sie unter [Zuordnen von Sound zu Objekten](#associate-sound-to-object).

>[!Note]
>Der tatsächliche-Erton zum Wiedergeben des Sounds wird durch die Bewegung und die Kollision dieser Spielobjekte bestimmt. Daher wird der-Befehl, der diese Sounds tatsächlich wieder gibt, in der [Simple3DGame:: updatedynamics](#simple3dgameupdatedynamics-method) -Methode definiert. Weitere Informationen finden Sie unter [Abspielen des Sounds](#play-the-sound).

```cppwinrt
void Simple3DGame::Initialize(
    _In_ std::shared_ptr<MoveLookController> const& controller,
    _In_ std::shared_ptr<GameRenderer> const& renderer
    )
{
    // The following member is defined in the header file:
    // Audio m_audioController;

    ...

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController.CreateDeviceIndependentResources();

    m_ammo.resize(GameConstants::MaxAmmo);

    ...

    // Create a media reader which is used to read audio files from its file location.
    MediaReader mediaReader;
    auto targetHitSoundX = mediaReader.LoadMedia(L"Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(std::make_shared<SoundEffect>());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            targetHitSoundX
            );
        ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader.LoadMedia(L"Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = std::make_shared<Sphere>();
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(std::make_shared<SoundEffect>());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Erstellen und Initialisieren der Audioressourcen

* Verwenden Sie [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), eine XAudio2-API, um zwei neue XAudio2-Objekte zu erstellen, die die Musik-und Soundeffekt-Engines definieren. Diese Methode gibt einen Zeiger auf die [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) -Schnittstelle des Objekts zurück, die alle audiomodulzustände, den Audioverarbeitungs Thread, das sprach Diagramm und mehr verwaltet.
* Nachdem die Module instanziiert wurden, verwenden Sie [IXAudio2:: foratemasteringvoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) , um für jedes der Sound-Engine-Objekte eine Mastering-Stimme zu erstellen.

Weitere Informationen finden Sie unter Gewusst [wie: Initialisieren von XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2).

### <a name="audiocreatedeviceindependentresources-method"></a>Audiomethode: createdevicindependentresources-Methode

```cppwinrt
void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    winrt::check_hresult(
        XAudio2Create(m_musicEngine.put(), flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    winrt::check_hresult(
        XAudio2Create(m_soundEffectEngine.put(), flags)
        );

    winrt::check_hresult(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Audiodatei laden

Im Beispiel Spiel wird der Code zum Lesen von audioformatdateien in " [Mediareader. h](#mediareaderh)/cpp__" definiert.  Um eine codierte WAV-Audiodatei zu lesen, wenden Sie [Mediareader:: loadmedia](#mediareaderloadmedia-method)an, und übergeben Sie den Dateinamen der. wav als Eingabeparameter.

### <a name="mediareaderloadmedia-method"></a>Mediareader:: loadmedia-Methode

Diese Methode verwendet die [Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk)-APIs, um die WAV-Audiodatei als Pulse Code Modulation (PCM)-Puffer einzulesen.

#### <a name="set-up-the-source-reader"></a>Einrichten des Quell Readers

1. Verwenden Sie [mfkreatesourcereaderfromurl](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) zum Erstellen eines Medienquellen Readers ([imfsourcereader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)).
2. Verwenden Sie [mfkreatemediatype](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) zum Erstellen eines Medientyp Objekts ([imfmediatype](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) (_mediaType_). Sie stellt eine Beschreibung eines Medien Formats dar. 
3. Geben Sie an, dass die decodierte Ausgabe des _mediaType_PCM-Audiodaten ist, bei dem es sich um einen Audiotyp handelt, den __XAudio2__ verwenden kann
4. Legt den decodierten Ausgabe Medientyp für den Quell Leser fest, indem [imfsourcereader:: setcurrentmediatype](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)aufgerufen wird.

Weitere Informationen zur Verwendung des Quell Readers finden Sie unter [Quelle Reader](/windows/desktop/medfound/source-reader).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Beschreiben des Datenformats des Audiodatenstroms

1. Verwenden Sie [imfsourcereader:: getcurrentmediatype](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) , um den aktuellen Medientyp für den Stream zu erhalten.
2. Verwenden Sie [imfmediatype:: mfkreatewaveformatexfrommfmediatype](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) , um den aktuellen audiomedientyp in einen [WaveFormatEx](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) -Puffer zu konvertieren. verwenden Sie dazu die Ergebnisse des vorherigen Vorgangs als Eingabe. Diese Struktur gibt das Datenformat des Wave-Audiostreams an, der nach dem Laden der Audiodatei verwendet wird. 

Das __WaveFormatEx__ -Format kann verwendet werden, um den PCM-Puffer zu beschreiben. Im Vergleich zur [WAVEFORMATEXTENSIBLE](/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) -Struktur kann Sie nur verwendet werden, um eine Teilmenge von AudioWave-Formaten zu beschreiben. Weitere Informationen zu den Unterschieden zwischen __WaveFormatEx__ und __WAVEFORMATEXTENSIBLE__finden Sie unter [erweiterbare Wave-Format-Deskriptoren](/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Lesen des Audiostreams

1.  Rufen Sie die Dauer (in Sekunden) des Audiostreams durch Aufrufen von [imfsourcereader:: getpresentationattribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) ab, und konvertieren Sie dann die Dauer in Bytes.
2.  Lesen Sie die Audiodatei in als Stream, indem Sie [imfsourcereader:: infosample](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample)aufrufen. "Read __Sample__ " liest das nächste Beispiel aus der Medienquelle.
3.  Verwenden Sie [imfsample:: convertumcontiguousbuffer](/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) , um den Inhalt des audiobeispielpuffers (_Sample_) in ein Array (_mediabuffer_) zu kopieren.

```cppwinrt
std::vector<byte> MediaReader::LoadMedia(_In_ winrt::hstring const& filename)
{
    winrt::check_hresult(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    winrt::com_ptr<IMFSourceReader> reader;
    winrt::check_hresult(
        MFCreateSourceReaderFromURL(
        (m_installedLocationPath + filename).c_str(),
            nullptr,
            reader.put()
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    winrt::com_ptr<IMFMediaType> mediaType;
    winrt::check_hresult(
        MFCreateMediaType(mediaType.put())
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    winrt::check_hresult(
        reader->SetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
    winrt::com_ptr<IMFMediaType> outputMediaType;
    winrt::check_hresult(
        reader->GetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), outputMediaType.put())
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    winrt::check_hresult(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    winrt::check_hresult(
        reader->GetPresentationAttribute(static_cast<uint32_t>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    std::vector<byte> fileData(maxStreamLengthInBytes);

    winrt::com_ptr<IMFSample> sample;
    winrt::com_ptr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        // Read audio data.
        ...
    }

    return fileData;
}
```

## <a name="associate-sound-to-object"></a>Dem Objekt einen Sound zuordnen

Das Zuordnen von Sounds zum Objekt findet statt, wenn das Spiel in der [Simple3DGame:: Initialize](#simple3dgameinitialize-method) -Methode initialisiert wird.

Zusammenfassung:
* In der __gameobject__ -Klasse gibt es eine __hitsound__ -Eigenschaft, die verwendet wird, um den Soundeffekt dem-Objekt zuzuordnen.
* Erstellen Sie eine neue Instanz des [SoundEffect](#soundeffecth) -Klassen Objekts, und ordnen Sie es dem Game-Objekt zu. Diese Klasse spielt einen Sound mithilfe von __XAudio2__ -APIs.  Er verwendet eine von der [audioklasse](#audioh) bereitgestellte Mastering-Stimme. Die Audiodaten können mit der [Mediareader](#mediareaderh) -Klasse aus dem Datei Speicherort gelesen werden.

[SoundEffect:: Initialize](#soundeffectinitialize-method) wird verwendet, um die __SoundEffect__ -Instanz mit den folgenden Eingabe Parametern zu initialisieren: Zeiger auf das Sound-Engine-Objekt (IXAudio2-Objekte, das in der [Audio:: createdeviceindependentresources-](#audiocreatedeviceindependentresources-method) Methode erstellt wurde), Zeiger auf das Format der WAV-Datei mithilfe von __Mediareader:: getoutputwaveformatex__und die mit der [Mediareader:: loadmedia](#mediareaderloadmedia-method) -Methode geladenen Audiodaten. Während der Initialisierung wird die Quell Stimme für den Soundeffekt ebenfalls erstellt.

### <a name="soundeffectinitialize-method"></a>SoundEffect:: Initialize-Methode

```cppwinrt
void SoundEffect::Initialize(
    _In_ IXAudio2* masteringEngine,
    _In_ WAVEFORMATEX* sourceFormat,
    _In_ std::vector<byte> const& soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    winrt::check_hresult(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Sound abspielen

Trigger zur Wiedergabe von Soundeffekten werden in der [Simple3DGame:: updatedynamics](#simple3dgameupdatedynamics-method) -Methode definiert, da hier die Verschiebung der Objekte aktualisiert und die Konflikte zwischen Objekten bestimmt werden.

Da die Interaktion zwischen Objekten stark abweicht, wird die Dynamik der Spielobjekte in Abhängigkeit von dem Spiel hier nicht erörtert. Wenn Sie sich mit der Implementierung vertraut machen möchten, rufen Sie die [Simple3DGame:: updatedynamics](#simple3dgameupdatedynamics-method) -Methode auf.

Wenn eine Kollision auftritt, löst sie grundsätzlich den Soundeffekt aus, der durch Aufrufen von **SoundEffect::P laysound**wiedergegeben wird. Mit dieser Methode werden alle derzeit wiedergegebenen Soundeffekte angehalten, und der Speicher interne Puffer wird mit den gewünschten Audiodaten in die Warteschlange eingereiht. Er verwendet die Quell Stimme, um das Volume festzulegen, Audiodaten zu senden und die Wiedergabe zu starten.

### <a name="soundeffectplaysound-method"></a>SoundEffect::P laysound-Methode

* Verwendet das Quell sprach Objekt **m \_ sourcevoice** , um die Wiedergabe des Sound Daten Puffers **m \_ Sound Data** zu starten.
* Erstellt einen [XAUDIO2- \_ Puffer](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer), mit dem ein Verweis auf den Sound Datenpuffer bereitstellt und dann mit einem Aufrufen von [IXAudio2SourceVoice:: submitsourcebuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)übermittelt wird. 
* Wenn sich die Audiodaten in der Warteschlange befinden, beginnt **SoundEffect::P laysound** durch Aufrufen von [IXAudio2SourceVoice:: Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start)wiederzugeben.

```cppwinrt
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = { 0 };

    if (!m_audioAvailable)
    {
        // Audio is not available so just return.
        return;
    }

    // Interrupt sound effect if it is currently playing.
    winrt::check_hresult(
        m_sourceVoice->Stop()
        );
    winrt::check_hresult(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue the memory buffer for playback and start the voice.
    buffer.AudioBytes = (UINT32)m_soundData.size();
    buffer.pAudioData = m_soundData.data();
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    winrt::check_hresult(
        m_sourceVoice->SetVolume(volume)
        );
    winrt::check_hresult(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    winrt::check_hresult(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame:: updatedynamics-Methode

Die __Simple3DGame:: updatedynamics__ -Methode kümmert sich um die Interaktion und den Konflikt Zwischenspiel Objekten. Wenn Objekte Konflikte (oder überschneiden), wird der zugehörige Soundeffekt ausgelöst.

```cppwinrt
void Simple3DGame::UpdateDynamics()
{
    ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
if (m_ammoCount > 1)
{
    ...
    // Check collision between instances One and Two.
    ...
    if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
    {
        // The two ammo are intersecting.
        ...
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
        ...
        // Play the sound associated with the Ammo hitting something.
        m_objects[i]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark
            // it as hit and play the sound associated with the impact.
            m_objects[i]->Hit(true);
            m_objects[i]->HitTime(timeTotal);
            m_totalHits++;

            m_objects[i]->PlaySound(impact, m_player->Position());
        }
        ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
            // Apply gravity and check for collision against enclosing volume.
            ...
                if (position.z < limit)
                {
                    // The ammo instance hit the a wall in the min Z direction.
                    // Align the ammo instance to the wall, invert the Z component of the velocity and
                    // play the impact sound.
                    position.z = limit;
                    m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
                    velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
                }
                ...
#pragma endregion
}
```

## <a name="next-steps"></a>Nächste Schritte

Wir haben das UWP-Framework, Grafiken, Steuerelemente, die Benutzeroberfläche und das Audiomaterial eines Windows 10-Spiels abgedeckt. Im nächsten Teil dieses Tutorials, [Erweitern des Beispiel Spiels](tutorial-resources.md), werden weitere Optionen erläutert, die bei der Entwicklung eines Spiels verwendet werden können.

## <a name="audio-concepts"></a>Audiokonzepte

Verwenden Sie für die Entwicklung von Windows 10-spielen die XAudio2-Version 2,9. Diese Version ist mit Windows 10 ausgeliefert. Weitere Informationen finden Sie unter [XAudio2-Versionen](/windows/desktop/xaudio2/xaudio2-versions).

__AudioX2__ ist eine API auf niedriger Ebene, die Signalverarbeitung und Vermischung von Foundation bereitstellt. Weitere Informationen finden Sie unter [XAudio2 Key Concepts](/windows/desktop/xaudio2/xaudio2-key-concepts).

### <a name="xaudio2-voices"></a>XAudio2 Stimmen

Es gibt drei Arten von XAudio2 Voice-Objekten: Quelle, teilmischung und Mastering Stimmen. Stimmen sind die Objekte, die XAudio2 zur Verarbeitung, Bearbeitung und Wiedergabe von Audiodaten verwendet werden. 
* Quellstimmen verarbeiten die vom Client bereitgestellten Audiodaten. 
* Quell- und Submixstimmen senden ihre Ausgabe an mindestens eine Submix- oder Masterstimme. 
* Submix- und Masterstimmen mischen die Audiodaten aller Stimmen, von denen sie Daten erhalten, und verarbeiten das Ergebnis. 
* Durch das beherrschen von Stimmen erhalten Sie Daten aus Quell Stimmen und Teil Mischungs Stimmen und sendet diese Daten an die Audiohardware.

Weitere Informationen finden Sie unter [XAudio2 Stimmen](/windows/desktop/xaudio2/xaudio2-voices).

### <a name="audio-graph"></a>Audiodiagramm

Das audiodiagramm ist eine Sammlung von [XAudio2 Stimmen](/windows/desktop/xaudio2/xaudio2-voices). Audiodaten werden an einer Seite eines audiodiagramms in den Quell Stimmen gestartet. optional wird eine oder mehrere Teil Mischungs Stimmen durchlaufen, und eine Stimme an. Ein audiodiagramm enthält eine Quell Stimme für jeden aktuell wiedergegebenen Sound, 0 (null) oder mehr Teil Mischungs Stimmen und eine Stimme. Das einfachste audiodiagramm und die Mindestanforderungen für die Durchführung eines Rauschens in XAudio2 ist eine einzelne Quelle, die direkt auf eine Mastering-Stimme aussteht. Weitere Informationen finden Sie in den [audiodiagrammen](/windows/desktop/xaudio2/audio-graphs).

### <a name="additional-reading"></a>Zusätzliche Lektüre

* [So wird's gemacht: Initialisieren von XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [So wird's gemacht: Laden von Datendateien in XAudio2](/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [So wird's gemacht: Wiedergeben von Ton mit XAudio2](/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>Key-Audiodateien

### <a name="audioh"></a>Audio.h

```cppwinrt
// Audio:
// This class uses XAudio2 to provide sound output. It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.

class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

private:
    ...
};
```

### <a name="mediareaderh"></a>Mediareader. h

```cppwinrt
// MediaReader:
// This is a helper class for the SoundEffect class. It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// vector of bytes.

class MediaReader
{
public:
    MediaReader();

    std::vector<byte> LoadMedia(_In_ winrt::hstring const& filename);
    WAVEFORMATEX* GetOutputWaveFormatEx();

private:
    winrt::Windows::Storage::StorageFolder  m_installedLocation{ nullptr };
    winrt::hstring                          m_installedLocationPath;
    WAVEFORMATEX                            m_waveFormat;
};
```

### <a name="soundeffecth"></a>SoundEffect.h

```cppwinrt
// SoundEffect:
// This class plays a sound using XAudio2. It uses a mastering voice provided
// from the Audio class. The sound data can be read from disk using the MediaReader
// class.

class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2* masteringEngine,
        _In_ WAVEFORMATEX* sourceFormat,
        _In_ std::vector<byte> const& soundData
        );

    void PlaySound(_In_ float volume);

private:
    ...
};
```