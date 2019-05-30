---
title: Hinzufügen von Audio zum Marble Maze-Beispiel
description: In diesem Dokument werden die wichtigsten Methoden beschrieben, die Sie berücksichtigen sollten, wenn Sie mit Audio arbeiten. Außerdem erfahren Sie, wie diese Methoden in Marble Maze angewendet werden.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, Audio, Spiele, Beispiel
ms.localizationpriority: medium
ms.openlocfilehash: ba0b4882803d9e6ddf059ae0f431c5bfba7eb227
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369155"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>Hinzufügen von Audio zum Marble Maze-Beispiel

In diesem Dokument werden die wichtigsten Methoden beschrieben, die Sie berücksichtigen sollten, wenn Sie mit Audio arbeiten. Außerdem erfahren Sie, wie diese Methoden in Marble Maze angewendet werden. Marble Maze verwendet [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk), um Audioressourcen aus einer Datei zu laden, und [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal), um Audio zu mischen und wiederzugeben und Effekte auf Audio anzuwenden.

Marble Maze gibt Musik im Hintergrund wieder und verwendet außerdem Spielsounds, die auf Spielereignisse hinweisen, beispielsweise wenn die Murmel an eine Wand prallt. Wichtig ist bei der Implementierung, dass Marble Maze einen Hall- oder Echoeffekt verwendet, um den Klang einer aufprallenden Murmel zu simulieren. Die Implementierung des Halleffekts bewirkt, dass Sie Echos in kleineren Räumen schneller und lauter hören. In größeren Räumen dagegen sind die Echos leiser und nicht so schnell zu hören.

> [!NOTE]
> Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](https://go.microsoft.com/fwlink/?LinkId=624011).

Hier sind einige der wichtigsten in diesem Dokument erörterten Punkte für das Arbeiten mit Audio in Ihrem Spiel:

- Ziehen Sie die Verwendung von Media Foundation zum Decodieren von Audioressourcen und XAudio2 zum Wiedergeben von Audio in Betracht. Wenn Sie aber bereits einen Mechanismus zum Laden von Audioressourcen haben, der in einer UWP-App funktioniert, können Sie diesen Mechanismus verwenden.

- Ein Audiodiagramm enthält eine Quellstimme für jeden aktiven Sound, null oder mehr Submixstimmen und eine Masterstimme. Quellstimmen können in Submixstimmen und/oder die Masterstimme eingespeist werden. Submixstimmen können in andere Submixstimmen oder die Masterstimme eingespeist werden.

- Wenn die Dateien für die Hintergrundmusik groß sind, können Sie die Musik in kleinere Puffer streamen, damit weniger Arbeitsspeicher verbraucht wird.

- Halten Sie gegebenenfalls die Audiowiedergabe an, wenn die App nicht mehr den Fokus hat, nicht mehr sichtbar ist oder angehalten wird. Setzen Sie die Wiedergabe fort, wenn Ihre App wieder den Fokus erhält, wieder sichtbar wird oder fortgesetzt wird.

- Legen Sie Audiokategorien fest, die den Rollen der einzelnen Sounds entsprechen. Angenommen, Sie in der Regel verwenden **AudioCategory\_GameMedia** für Spiele hintergrundaudiofunktion und **AudioCategory\_GameEffects** für Soundeffekten.

- Behandeln Sie Geräteänderungen, einschließlich Kopfhörern, in dem Sie alle Audioressourcen und -schnittstellen freigeben und erneut erstellen.

- Komprimieren Sie gegebenenfalls Audiodateien, wenn Sie den Speicherplatz und die Streamingkosten minimieren müssen. Anderenfalls können Sie die Audiodateien unkomprimiert lassen, damit sie schneller geladen werden.

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>Einführung in XAudio2 und Microsoft Media Foundation

XAudio2 ist eine Low-Level-Audiobibliothek für Windows, die speziell Audio in Spielen unterstützt. Sie enthält eine digitale Signalverarbeitung (Digital Signal Processing, DSP) und ein Audiodiagrammmodul für Spiele. Als Erweiterung der Vorgänger DirectSound und XAudio unterstützt XAudio2 Computertrends wie zum Beispiel SIMD-Gleitkommaarchitekturen und HD-Audio. Außerdem werden die komplexeren Soundverarbeitungsanforderungen aktueller Spiele unterstützt.

Im [Dokument zu den wichtigsten Konzepten von XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts) werden die wichtigsten Konzepte für die Verwendung von XAudio2 erläutert. Die Konzepte im Kurzüberblick:

- Die [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Schnittstelle bildet den Kern des XAudio2-Moduls. Marble Maze verwendet diese Schnittstelle, um Stimmen zu erstellen und Benachrichtigungen zu empfangen, wenn sich das Ausgabegerät ändert oder Fehler auftreten.

- Eine **Stimme** verarbeitet Audiodaten, passt sie an und gibt sie wieder.

- Eine **Quellstimme** ist eine Sammlung von Audiokanälen (Mono, 5.1 usw.) und stellt einen Audiodatenstream dar. In XAudio2 ist eine Quellstimme der Ausgangspunkt der Audioverarbeitung. Normalerweise werden die Sounddaten aus einer externen Quelle wie beispielsweise einer Datei oder einem Netzwerk geladen und an eine Quellstimme gesendet. In Marble Maze werden Sounddaten mithilfe von [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) aus Dateien geladen. Media Foundation wird weiter unten in diesem Dokument vorgestellt.

- Eine **Submixstimme** verarbeitet Audiodaten. Bei dieser Verarbeitung kann zum Beispiel der Audiostream geändert werden oder mehrere Streams werden zu einem Stream kombiniert. Marble Maze verwendet Submixe, um den Halleffekt zu erstellen.

- Eine **Masterstimme** kombiniert Daten aus Quell- und Submixstimmen und sendet Daten an die Audiohardware.

- Ein **Audiodiagramm** enthält eine Quellstimme für jeden aktiven Sound, null oder mehrere Submixstimmen und nur eine Masterstimme .

- Der Clientcode wird über einen **Rückruf** informiert, dass in einer Stimme oder in einem Modulobjekt ein Ereignis aufgetreten ist. Mithilfe von Rückrufen können Sie den Arbeitsspeicher wiederverwenden, wenn XAudio2 mit einem Puffer fertig ist, reagieren, wenn sich das Audiogerät ändert (z. B. wenn Sie Kopfhörer anschließen oder trennen) usw. Im Abschnitt [Behandeln von Kopfhörern und Geräteänderungen](#handling-headphones-and-device-changes) weiter unten in diesem Dokument erfahren Sie, wie Marble Maze diesen Mechanismus für die Behandlung von Geräteänderungen verwendet.

Marble Maze verwendet zwei Audiomodule (mit anderen Worten: zwei [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Objekte) zum Verarbeiten von Audio. Ein Modul verarbeitet die Hintergrundmusik und das andere verarbeitet Spielsounds.

Außerdem muss Marble Maze für jedes Modul eine Masterstimme erstellen. Denken Sie daran, dass ein Mastermodul Audiostreams in einem Stream kombiniert und diesen Stream an die Audiohardware sendet. Der Stream für die Hintergrundmusik, eine Quellstimme, gibt Daten an eine Masterstimme und an zwei Submixstimmen aus. Die Submixstimmen führen den Halleffekt aus.

Media Foundation ist eine Multimediabibliothek, die viele Audio- und Videoformate unterstützt. XAudio2 und Media Foundation ergänzen sich gegenseitig. Marble Maze verwendet Media Foundation zum Laden von Audioressourcen aus Dateien und XAudio2 zum Wiedergeben von Audio. Sie müssen Media Foundation nicht verwenden, um Audioressourcen zu laden. Wenn Sie bereits einen Mechanismus zum Laden von Audioressourcen haben, der in einer UWP-App funktioniert, verwenden Sie diesen Mechanismus. [Audio, Video und Kamera](../audio-video-camera/index.md) beschreibt verschiedene Methoden zur Implementierung von Audio in einer UWP-App.

Weitere Informationen zu XAudio2 finden Sie im [Programmierhandbuch](https://docs.microsoft.com/windows/desktop/xaudio2/programming-guide). Weitere Informationen zu Media Foundation finden Sie unter [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk).

## <a name="initializing-audio-resources"></a>Initialisieren von Audioressourcen

Marble Maze verwendet eine WMA-Datei (Windows Media Audio) für die Hintergrundmusik und WAV-Dateien für Spielsounds. Diese Formate werden von Media Foundation unterstützt. Obwohl das WAV-Dateiformat nativ von XAudio2 unterstützt wird, muss das Dateiformat von einem Spiel manuell analysiert werden, um die entsprechenden XAudio2-Datenstrukturen auszufüllen. Marble Maze verwendet Media Foundation, um die Arbeit mit WAV-Dateien zu erleichtern. Die vollständige Liste der von Media Foundation unterstützten Medienformate finden Sie unter [Unterstützte Medienformate in Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/supported-media-formats-in-media-foundation). Marble Maze verwendet keine getrennten Entwurfszeit- und Laufzeit-Audioformate und keine Unterstützung für XAudio2-ADPCM-Komprimierung. Weitere Informationen zur ADPCM-Komprimierung in XAudio2 finden Sie in der Übersicht über [ADPCM](https://docs.microsoft.com/windows/desktop/xaudio2/adpcm-overview).

Die **Audio::CreateResources**-Methode, die von **MarbleMazeMain::LoadDeferredResources** aufgerufen wird, lädt die Audiostreams aus den Dateien, initialisiert die XAudio2-Modulobjekte und erstellt die Quell-, Submix- und Masterstimmen.

### <a name="creating-the-xaudio2-engines"></a>Erstellen der XAudio2-Module

Bedenken Sie, dass Marble Maze für jedes verwendete Audiomodul ein [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Objekt erstellt. Rufen Sie die [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create)-Methode auf, um ein Audiomodul zu erstellen. Das folgende Beispiel zeigt, wie Marble Maze das Audiomodul für die Verarbeitung der Hintergrundmusik erstellt.

```cpp
// In Audio.h
class Audio
{
private:
    IXAudio2*                   m_musicEngine;
// ...
}

// In Audio.cpp
void Audio::CreateResources()
{
    try
    {
        // ...
        DX::ThrowIfFailed(
            XAudio2Create(&m_musicEngine)
            );
        // ...
    }
    // ...
}
```

Einen ähnlichen Schritt führt Marble Maze aus, um das Audiomodul zu erstellen, das die Spielsounds wiedergibt.

Zwischen der Arbeit mit der [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Schnittstelle in einer UWP-App und der Arbeit mit einer Desktop-App gibt es zwei Unterschiede. Erstens ist es nicht erforderlich, [CoInitializeEx](https://docs.microsoft.com/windows/desktop/api/combaseapi/nf-combaseapi-coinitializeex) vor dem Aufrufen von [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) aufzurufen. Außerdem unterstützt **IXAudio2** die Geräteaufzählung nicht mehr. Weitere Informationen zum Aufzählen von Audiogeräten finden Sie unter [Enumerieren von Geräten](https://docs.microsoft.com/previous-versions/windows/apps/hh464977(v=win.10)).

### <a name="creating-the-mastering-voices"></a>Erstellen der Masterstimmen

Das folgende Beispiel zeigt, wie die **Audio::CreateResources**-Methode mithilfe der [IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice)-Methode die Masterstimme für die Hintergrundmusik erstellt. In diesem Beispiel **m\_MusicMasteringVoice** ist ein [IXAudio2MasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2masteringvoice) Objekt. Wir verwenden zwei Eingabekanäle, um die Logik für den Halleffekt zu vereinfachen. 

Wir geben 48000 als Eingabebeispielrate an. Diese Abtastrate wurde ausgewählt, da sie ein ausgewogenes Verhältnis von Audioqualität und Umfang der erforderlichen CPU-Verarbeitung darstellt. Eine höhere Abtastrate würde mehr CPU-Verarbeitung erfordern, ohne eine merkliche Qualitätssteigerung zu bewirken. 

Wir geben danach **AudioCategory\_GameMedia** als Audiostreamkategorie an, damit Benutzer bei der Verwendung des Spiels Musik aus einer anderen Anwendung hören können. Bei eine Musik-app wiedergegeben wird, schaltet Windows alle stimmen zurück, die vom erstellten stumm die **AudioCategory\_GameMedia** Option. Der Benutzer weiterhin hört Gameplay klingt, da sie von erstellt werden die **AudioCategory\_GameEffects** Option. Weitere Informationen über audio Kategorien finden Sie unter [AUDIO\_STREAM\_Kategorie](https://docs.microsoft.com/windows/desktop/api/audiosessiontypes/ne-audiosessiontypes-_audio_stream_category).

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice,
        2,
        48000,
        0,
        nullptr,
        nullptr,
        AudioCategory_GameMedia
        )
);
```

Die **Audio::CreateResources** Methode führt einen ähnlichen Schritt, um Perfektion Stimme für die Sounds Spiel zu erstellen, mit dem Unterschied, dass angegeben **AudioCategory\_GameEffects** für die  *StreamCategory* -Parameter, der der Standardwert ist.

### <a name="creating-the-reverb-effect"></a>Erstellen des Halleffekts

Sie können für jede Stimme mit XAudio2 Effektsequenzen erstellen, die Audio verarbeiten. Eine solche Sequenz wird als Effektkette bezeichnet. Verwenden Sie Effektketten, wenn Sie mindestens einen Effekt auf eine Stimme anwenden möchten. Effektketten können destruktiv sein, d. h., jeder Effekt in der Kette kann den Audiopuffer überschreiben. Diese Eigenschaft ist wichtig, da XAudio2 nicht garantiert, dass Ausgabepuffer lautlos initialisiert werden. Effektobjekte werden in XAudio2 durch plattformübergreifende Audioverarbeitungsobjekte (Audio Processing Object, XAPO) dargestellt. Weitere Informationen zu XAPO finden Sie in der [XAPO-Übersicht](https://docs.microsoft.com/windows/desktop/xaudio2/xapo-overview).

Führen Sie beim Erstellen einer Effektkette die folgenden Schritte aus:

1. Erstellen Sie das Effektobjekt.

2. Auffüllen einer [XAUDIO2\_Auswirkung\_Deskriptor](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) Struktur mit Daten von Auswirkungen.

3. Auffüllen einer [XAUDIO2\_Auswirkung\_Kette](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) Struktur mit Daten.

4. Wenden Sie die Effektkette auf eine Stimme an.

5. Füllen Sie eine Effektparameterstruktur auf und wenden Sie sie auf den Effekt an.

6. Deaktivieren oder aktivieren Sie den Effekt nach Bedarf.

Die **Audio**-Klasse definiert die **CreateReverb**-Methode, um die Effektkette zu erstellen, die den Halleffekt implementiert. Diese Methode ruft die [XAudio2CreateReverb](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/nf-xaudio2fx-xaudio2createreverb)-Methode auf, um ein **ComPtr&lt;IUnknown&gt;** -Objekt, **soundEffectXAPO** zu erstellen, das als Submixstimme für den Halleffekt fungiert.

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

Die [XAUDIO2\_Auswirkung\_Deskriptor](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) Struktur enthält Informationen über eine XAPO für die Verwendung in einer Kette Auswirkungen, z. B. die vorgegebene Anzahl von Ausgabekanälen. Die **Audio::CreateReverb** -Methode erstellt eine **XAUDIO2\_Auswirkung\_Deskriptor** -Objekt, **SoundEffectdescriptor**, d. h. Legen Sie auf der deaktiviert, Status, verwendet zwei Ausgabekanäle und Verweise **SoundEffectXAPO** für den Halleffekt. **soundEffectdescriptor** beginnt mit dem deaktivierten Zustand, da das Spiel Parameter festlegen muss, bevor der Effekt mit dem Ändern der Spielsounds beginnt. Marble Maze verwendet zwei Ausgabekanäle, um die Logik für den Halleffekt zu vereinfachen.

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

Wenn Ihre Effektkette mehrere Effekte enthält, benötigen Sie für jeden Effekt ein Objekt. Die [XAUDIO2\_Auswirkung\_Kette](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) Struktur enthält das Array von [XAUDIO2\_AUSWIRKUNGEN\_Deskriptor](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) Objekte, die in den Effekt einbezogen. Das folgende Beispiel zeigt, wie die **Audio::CreateReverb**-Methode den einen Effekt für die Implementierung des Halleffekts angibt.

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

Die **Audio::CreateReverb**-Methode ruft die [IXAudio2::CreateSubmixVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsubmixvoice)-Methode auf, um die Submixstimme für den Effekt zu erstellen. Wird die [XAUDIO2\_Auswirkung\_Kette](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) Objekt **SoundEffectChain**, für die *pEffectChain* Parameter, um die Auswirkungen zu verknüpfen die Kette mit der Stimme. Außerdem gibt Marble Maze zwei Ausgabekanäle und eine Abtastrate von 48 Kilohertz an.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> Verwenden Sie die [IXAudio2Voice::SetEffectChain](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectchain)-Methode, wenn Sie eine vorhandene Effektkette an eine vorhandene Submixstimme anfügen oder die aktuelle Effektkette ersetzen möchten.

Die **Audio::CreateReverb**-Methode ruft [IXAudio2Voice::SetEffectParameters](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectparameters) auf, um zusätzliche dem Effekt zugeordnete Parameter festzulegen. Diese Methode akzeptiert eine für den Effekt spezifische Parameterstruktur. Ein [XAUDIO2FX\_HALL\_Parameter](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters) Objekt **M_reverbParametersSmall**, enthält die Parameter wirksam für Hall, initialisiert wird, der  **Audio::Initialize** Methode, da jeder "Hall" die gleichen Parameter verwendet. Das folgende Beispiel zeigt, wie die **Audio::Initialize**-Methode die Hallparameter für Nahfeld-Halleffekte initialisiert.

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

Dieses Beispiel verwendet die Standardwerte für die meisten Hallparameter, legt aber **DisableLateField** auf TRUE fest, um den Nahfeld-Halleffekt anzugeben, **EarlyDiffusion** auf 4, um flache Oberflächen in der Nähe zu simulieren, und **LateDiffusion** auf 15, um sehr diffuse entfernte Oberflächen zu simulieren. Flache Oberflächen in der Nähe verursachen Echos, die Sie schneller und lauter hören. Diffuse entfernte Oberflächen verursachen leisere und nicht so schnell hörbare Echos. Sie können mit den Halleffektwerten experimentieren, um in Ihrem Spiel den gewünschten Effekt zu erzielen, oder die **ReverbConvertI3DL2ToNative**-Methode verwenden, um I3DL2-Parameter (Interactive 3D Audio Rendering Guidelines Level 2.0) nach Branchenstandard zu verwenden.

Das folgende Beispiel zeigt, wie **Audio::CreateReverb** die Hallparameter festlegt. **NewSubmix** ist ein [IXAudio2SubmixVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2submixvoice)**-Objekt. **Parameter** ist ein [XAUDIO2FX\_HALL\_Parameter](https://docs.microsoft.com/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)* Objekt.

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

Die **Audio::CreateReverb**-Methode aktiviert zum Schluss den Effekt mithilfe von [IXAudio2Voice::EnableEffect](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-enableeffect) fest, wenn das **enableEffect**-Kennzeichen festgelegt ist. Es legt ebenfalls die Lautstärke mit [IXAudio2Voice::SetVolume](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setvolume) und unter Verwendung der Matrix [IXAudio2Voice::SetOutputMatrix](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setoutputmatrix) fest. Mit diesem Teil wird die volle Lautstärke (1.0) festgelegt und dann die Lautstärkematrix so angegeben, dass die linke und die rechte Eingabe sowie der linke und der rechte Ausgabelautsprecher stumm sind. Dies geschieht, da später anderer Code zwischen den beiden Halleffekten überblendet (und dabei den Übergang von der Nähe zu einer Wand zu einem großen Raum simuliert) oder bei Bedarf beide Halleffekte stummschaltet. Wenn die Stummschaltung des Hallpfads später aufgehoben wird, legt das Spiel die Matrix {1.0f, 0.0f, 0.0f, 1.0f} fest, um die linke Hallausgabe an die linke Eingabe der Masterstimme und die rechte Hallausgabe an die rechte Eingabe der Masterstimme weiterzuleiten.

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

Marble Maze ruft die **Audio::CreateReverb**-Methode viermal auf: zweimal für die Hintergrundmusik und zweimal für die Spielsounds. Das folgende Beispiel zeigt, wie Marble Maze die **CreateReverb**-Methode für die Hintergrundmusik aufruft.

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

Eine Liste der möglichen Quellen für Effekte, die Sie mit XAudio2 verwenden können, finden Sie unter [XAudio2-Audioeffekte](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-audio-effects).

### <a name="loading-audio-data-from-file"></a>Laden von Audiodaten aus einer Datei

Marble Maze definiert die **MediaStreamer**-Klasse, die Media Foundation zum Laden von Audioressourcen aus den Dateien verwendet. Marble Maze verwendet zum Laden jeder Audiodatei ein **MediaStreamer**-Objekt.

Marble Maze ruft die **MediaStreamer::Initialize**-Methode auf, um die einzelnen Audiostreams zu initialisieren. So ruft die **Audio::CreateResources**-Methode **MediaStreamer::Initialize** auf, um den Audiostream für die Hintergrundmusik zu initialisieren:

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

Die **MediaStreamer::Initialize**-Methode ruft zunächst die [MFStartup](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfstartup)-Funktion auf, um Media Foundation zu initialisieren. **MF_VERSION** ist eine im **mfapi.h** definierte Makro, die wir als die empfohlene Version von Media Foundation verwenden.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

**MediaStreamer::Initialize** ruft dann [MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) auf, um ein [IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)-Objekt zu erstellen. Ein **IMFSourceReader**-Objekt, **m_reader**, liest Mediendaten aus der Datei, die mit **url** angegeben wird.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

Dann erstellt die **MediaStreamer::Initialize**-Methode ein [IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)-Objekt mithilfe von [MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype), um das Format des Audiostreams zu beschreiben. Ein Audioformat hat zwei Typen: einen Haupttyp und einen Untertyp. Der Haupttyp definiert das allgemeine Format der Medien, zum Beispiel Video, Audio, Skript usw. Der Untertyp definiert das Format, zum Beispiel PCM, ADPCM oder WMA.

Die **MediaStreamer::Initialize** -Methode verwendet die [IMFAttributes::SetGUID](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setguid) Methode, um den Haupttyp anzugeben ([MF_MT_MAJOR_TYPE](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-major-type-attribute)) als Audiodaten ( **MFMediaType\_Audio**) und der kleinere Typ ([MF_MT_SUBTYPE](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-subtype-attribute)) als unkomprimierte PCM-Audio (**MFAudioFormat\_PCM**). **MF_MT_MAJOR_TYPE** und **MF_MT_SUBTYPE** sind [Media Foundation-Attribute](https://docs.microsoft.com/windows/desktop/medfound/media-foundation-attributes). **MFMediaType_Audio** und **MFAudioFormat_PCM** sind Typ- und Untertyp-GUIDs; Weitere Informationen finden Sie unter [Audio-Medientypen](https://docs.microsoft.com/windows/desktop/medfound/audio-media-types). Die [IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)-Methode ordnet den Medientyp dem StreamReader zu.

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

DX::ThrowIfFailed(
    MFCreateMediaType(&mediaType)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
    );

DX::ThrowIfFailed(
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

Die **MediaStreamer::Initialize**-Methode ruft dann das vollständige Ausgabemedienformat von Media Foundation mithilfe von [IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) ab und ruft die [MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype)-Funktion auf, um den Media Foundation-Audiomedientyp in eine [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)-Struktur zu konvertieren. Die **WAVEFORMATEX**-Struktur definiert das Format von Waveform-Audiodaten. Marble Maze erstellt mithilfe dieser Struktur die Quellstimmen und wendet den Tiefpassfilter auf den Sound für das Rollen der Murmel an.

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> [!IMPORTANT]
> Die [MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype)-Methode verwendet **CoTaskMemAlloc** zum Zuordnen des [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)-Objekts. Rufen Sie daher unbedingt **CoTaskMemFree** auf, wenn Sie dieses Objekt nicht mehr verwenden.

 

Die **MediaStreamer::Initialize** Methode abgeschlossen ist, durch die die Länge des Datenstroms, der Berechnung **m\_MaxStreamLengthInBytes**, in Bytes. Dazu ruft sie die [IMFSourceReader::IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute)-Methode auf, um die Dauer des Audiostreams in Einheiten von jeweils 100 Nanosekunden abzurufen, konvertiert die Dauer in Abschnitte und multipliziert dann diese Zahl mit der durchschnittlichen Datenübertragungsrate in Bytes pro Sekunde. Diesen Wert verwendet Marble Maze später, um den Puffer zuzuordnen, in dem sich die einzelnen Spielsounds befinden.

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->
        GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );

// duration is in 100ns units; convert to seconds, and round up
// to the nearest whole byte.
ULONGLONG duration = var.uhVal.QuadPart;
m_maxStreamLengthInBytes =
    static_cast<unsigned int>(
        ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000)
        / 10000000
        );
```

### <a name="creating-the-source-voices"></a>Erstellen der Quellstimmen

Marble Maze erstellt XAudio2-Quellstimmen für die Wiedergabe der einzelnen Spielsounds und der Musik in Quellstimmen. Die **Audio**-Klasse definiert ein [IXAudio2SourceVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2sourcevoice)-Objekt für die Hintergrundmusik und ein Array mit **SoundEffectData**-Objekten für die Spielsounds. Die **SoundEffectData**-Struktur enthält das **IXAudio2SourceVoice**-Objekt für einen Effekt und definiert außerdem andere Daten im Zusammenhang mit Effekten, beispielsweise den Audiopuffer. **Audio.h** definiert die **SoundEvent**-Enumeration. Mithilfe dieser Enumeration identifiziert Marble Maze die einzelnen Sounds im Spiel. Außerdem verwendet die **Audio**-Klasse diese Enumeration zum Indizieren des Arrays mit **SoundEffectData**-Objekten.

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

Die folgende Tabelle zeigt die Beziehung zwischen den einzelnen Werten, die Datei, in der sich die zugeordneten Sounddaten befinden, und eine kurze Beschreibung der einzelnen Sounds. Die Dateien befinden sich in der  **\\Media\\Audio** Ordner.

| SoundEvent-Wert  | Dateiname      | Beschreibung                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | Wird wiedergegeben, wenn die Murmel rollt.                              |
| FallingEvent      | MarbleFall.wav | Wird wiedergegeben, wenn die Murmel aus dem Labyrinth fällt.               |
| CollisionEvent    | MarbleHit.wav  | Wird wiedergegeben, wenn die Murmel mit dem Labyrinth kollidiert.           |
| CheckpointEvent   | Checkpoint.wav | Wird wiedergegeben, wenn die Murmel über einen Prüfpunkt rollt.         |
| MenuChangeEvent   | MenuChange.wav | Wird wiedergegeben, wenn der Benutzer das aktuelle Menüelement ändert. |
| MenuSelectedEvent | MenuSelect.wav | Wird wiedergegeben, wenn der Benutzer ein Menüelement auswählt.           |

 

Das folgende Beispiel zeigt, wie die **Audio::CreateResources**-Methode die Quellstimme für die Hintergrundmusik erstellt. Die [XAUDIO2\_senden\_Deskriptor](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_send_descriptor) Struktur definiert die Stimme des Ziel-Ziel von einer anderen Sprache und gibt an, ob ein Filter verwendet werden soll. Marble Maze ruft die **Audio::SetSoundEffectFilter**-Methode auf, um mit den Filtern den Sound der rollenden Murmel zu ändern. Die [XAUDIO2\_VOICE\_sendet](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_voice_sends) Struktur definiert den Satz der stimmen zurück, die zum Empfangen von Daten aus einer einzigen Ausgabezeile Stimme. Marble Maze sendet Daten von der Quellstimme an die Masterstimme (für den direkten oder unveränderten Teil eines wiedergegebenen Sounds) und an die zwei Submixstimmen, die den mit einem Effekt versehenen oder hallenden Teil eines wiedergegebenen Sounds implementieren.

Die [IXAudio2::CreateSourceVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsourcevoice)-Methode erstellt und konfiguriert eine Quellstimme. Sie akzeptiert eine [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)-Struktur, die das Format der an die Stimme gesendeten Audiopuffer definiert. Wie bereits erwähnt verwendet Marble Maze das PCM-Format.

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_musicMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_musicReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_musicReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;
WAVEFORMATEX& waveFormat = m_musicStreamer.GetOutputWaveFormatEx();

DX::ThrowIfFailed(
    m_musicEngine->CreateSourceVoice(&m_musicSourceVoice, &waveFormat, 0, 1.0f, &m_voiceContext, &sends, nullptr)
    );

DX::ThrowIfFailed(
    m_musicMasteringVoice->SetVolume(0.4f)
    );
```

## <a name="playing-background-music"></a>Wiedergeben von Hintergrundmusik


Eine Quellstimme wird im angehaltenen Zustand erstellt. Marble Maze startet die Hintergrundmusik in der Spielschleife. Beim ersten Aufruf von **MarbleMazeMain::Update** wird **Audio::Start** aufgerufen, um die Hintergrundmusik zu starten.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

Die **Audio::Start**-Methode ruft [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) auf, um die Verarbeitung der Quellstimme für die Hintergrundmusik zu starten.

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

Die Quellstimme gibt diese Audiodaten an die nächste Phase des Audiodiagramms weiter. Im Fall von Marble Maze enthält die nächste Phase zwei Submixstimmen, die die beiden Halleffekte auf die Audiodaten anwenden. Eine Submixstimme wendet einen nah klingenden, spät hörbaren Halleffekt an, die zweite wendet einen entfernt klingenden, spät hörbaren Halleffekt an.

In welchem Umfang die einzelnen Submixstimmen zur endgültigen Mischung beitragen, hängt von der Größe und Form des Raums ab. Der nah klingende Halleffekt trägt mehr bei, wenn sich die Murmel in der Nähe einer Wand oder in einem kleinen Raum befindet, und der spät hörbare Halleffekt trägt mehr bei, wenn sich die Murmel in einem großen Raum befindet. Durch diese Vorgehensweise entsteht ein realistischerer Echoeffekt, wenn sich die Murmel durch das Labyrinth bewegt. Weitere Informationen zur Implementierung dieses Effekts in Marble Maze finden Sie im Marble Maze-Quellcode unter **Audio::SetRoomSize** und **Physics::CalculateCurrentRoomSize**.

> [!NOTE]
> In einem Spiel mit relativ gleichen Raumgrößen können Sie ein einfacheres Hallmodell verwenden. Zum Beispiel können Sie eine Halleinstellung für alle Räume verwenden oder für jeden Raum eine vordefinierte Halleinstellung erstellen.

Die **Audio::CreateResources**-Methode verwendet Media Foundation zum Laden der Hintergrundmusik. An dieser Stelle hat die Quellstimme aber noch keine Audiodaten, mit denen sie arbeiten kann. Darüber hinaus muss die Quellstimme regelmäßig mit Daten aktualisiert werden, damit die Wiedergabe der Hintergrundmusik, die in einer Schleife wiedergegeben wird, fortgesetzt wird.

Damit die Quellstimme immer mit Daten gefüllt ist, aktualisiert die Spielschleife die Audiopuffer in jedem Frame. Die **MarbleMazeMain::Render**-Methode ruft **Audio::Render** auf, um den Audiopuffer der Hintergrundmusik zu verarbeiten. Die **Audio** -Klasse definiert ein Array von drei audio-Puffer **m\_AudioBuffers**. Jeder Puffer enthält 64 KB (65536 Byte) an Daten. Die Schleife liest Daten aus dem Media Foundation-Objekt und schreibt diese Daten in die Quellstimme, bis die Warteschlange drei Puffer enthält.

> [!CAUTION]
> Obwohl Marble Maze einen 64 KB großen Puffer für Musikdaten verwendet, müssen Sie möglicherweise einen größeren oder kleineren Puffer verwenden. Die Größe hängt von den Anforderungen Ihres Spiels ab.

```cpp
// This sample processes audio buffers during the render cycle of the application.
// As long as the sample maintains a high-enough frame rate, this approach should
// not glitch audio. In game code, it is best for audio buffers to be processed
// on a separate thread that is not synced to the main render loop of the game.
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer],
                STREAMING_BUFFER_SIZE,
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch (...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

Die Schleife ist auch zuständig, wenn das Media Foundation-Objekt das Ende des Streams erreicht. In diesem Fall ruft sie die [IMFSourceReader::SetCurrentPosition](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentposition)-Methode auf, um die Position der Audioquelle zurückzusetzen.

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

Um audio Schleifen für ein einzelner Puffer (oder für einen gesamten Sound, die vollständig in den Arbeitsspeicher geladen ist) zu implementieren, Sie können festlegen, die [XAUDIO2_BUFFER](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer):: loopcount ab Feld **XAUDIO2\_Schleife\_INFINITE** Wenn Sie den Sound zu initialisieren. Marble Maze verwendet diese Technik, um den Rollsound der Murmel wiederzugeben.

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

Für die Hintergrundmusik verwaltet Marble Maze aber die Puffer direkt, sodass es die Menge des verwendeten Arbeitsspeichers besser steuern kann. Wenn Ihre Musikdateien zu groß sind, können Sie die Musikdaten in kleinere Puffer streamen. Dadurch können Sie die Größe des Arbeitsspeichers besser auf die Verarbeitung und das Streamen von Audiodaten im Spiel abstimmen.

> [!TIP]
> Wenn Ihr Spiel eine niedrige oder veränderliche Framerate hat, kann die Verarbeitung von Audio im Hauptthread zu unerwarteten Pausen oder zu Knacken im Audio führen. Das kommt daher, dass dem Audiomodul nicht genug gepufferte Audiodaten zur Verfügung stehen. Wenn dieses Problem bei Ihrem Spiel zu erwarten ist, denken Sie darüber nach, Audio in einem getrennten Thread zu verarbeiten, in dem kein Rendering ausgeführt wird. Dieser Ansatz ist besonders hilfreich auf Computern mit mehreren Prozessoren, da Ihr Spiel die Prozessoren verwenden kann, die sich im Leerlauf befinden.

## <a name="reacting-to-game-events"></a>Reagieren auf Spielereignisse

Die **Audio**-Klasse stellt Methoden wie **PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch** und **SetSoundEffectFilter** bereit, damit vom Spiel gesteuert werden kann, wann Sounds wiedergegeben und beendet werden. Außerdem werden damit Soundeigenschaften wie Lautstärke und Tonhöhe gesteuert. Falls die Murmel beispielsweise aus dem Labyrinth herausfällt, ruft **MarbleMazeMain::Update** die **Audio::PlaySoundEffect**-Methode auf, um den **FallingEvent**-Sound wiederzugeben.

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

Die **Audio::PlaySoundEffect**-Methode ruft die [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start)-Methode auf, um die Wiedergabe des Sounds zu starten. Wenn die **IXAudio2SourceVoice::Start**-Methode bereits aufgerufen wurde, wird sie nicht erneut gestartet. Anschließend führt **Audio::PlaySoundEffect** eine benutzerdefinierte Logik für bestimmte Sounds aus.

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError)
    {
        // If there's an error, then we'll recreate the engine on the next
        // render pass.
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();

    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up,
    // and allow up to two collisions to be queued up. 
    if (sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};

        soundEffect->m_soundEffectSourceVoice->
            GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);

        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click
        // right away.
        // Note that stopping and then flushing could cause a glitch due to the
        // waveform not being at a zero-crossing, but due to the nature of the 
        // sound (fast and 'clicky'), we don't mind.
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();

            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);

            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

Für andere Sounds als den Rollsound ruft die **Audio::PlaySoundEffect**-Methode [IXAudio2SourceVoice::GetState](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-getstate) auf, um die Anzahl der Puffer zu ermitteln, die die Quellstimme wiedergibt. Sie ruft [IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer) auf, um die Audiodaten für den Sound der Eingabewarteschlange der Stimme hinzuzufügen, wenn keine Puffer aktiv sind. Mit der **Audio::PlaySoundEffect**-Methode kann der Kollisionssound auch zweimal hintereinander wiedergegeben werden. Dies ist beispielsweise der Fall, wenn die Murmel mit einer Ecke des Labyrinths kollidiert.

Wie bereits beschrieben, die Audio-Klasse verwendet die **XAUDIO2\_Schleife\_UNENDLICHE** zu kennzeichnen, wenn er den Sound für das parallele Ereignis initialisiert. Der Sound wird in einer Schleife wiedergegeben, wenn **Audio::PlaySoundEffect** erstmals für dieses Ereignis aufgerufen wird. Um die Wiedergabelogik für den Rollsound zu vereinfachen, schaltet Marble Maze den Sound stumm, anstatt ihn anzuhalten. Wenn sich die Geschwindigkeit der Murmel ändert, werden Tonhöhe und Lautstärke des Sounds geändert, damit er realistischer klingt. Unten wird gezeigt, wie die **MarbleMazeMain::Update**-Methode die Tonhöhe und Lautstärke der Murmel aktualisiert, wenn sich die Geschwindigkeit ändert, und wie sie beim Anhalten der Murmel den Sound stummschaltet, indem sie die Lautstärke auf Null festlegt.

```cpp
// Play the roll sound only if the marble is actually rolling.
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly
    // ramp up the low-pass filter the faster we go.
    // We also reduce the Q-value of the filter, starting with a
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent,
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## <a name="reacting-to-suspend-and-resume-events"></a>Reagieren auf Anhalte- und Fortsetzungsereignisse

In [Marble Maze-Anwendungsstruktur](marble-maze-application-structure.md) wird beschrieben, wie Marble Maze Anhalten und Fortsetzen unterstützt. Wenn das Spiel angehalten wird, wird auch die Audiowiedergabe angehalten. Wenn das Spiel fortgesetzt wird, wird die Audiowiedergabe an der Stelle fortgesetzt, an der sie unterbrochen wurde. Damit orientieren wir uns an der bewährten Methode, keine Ressourcen zu verwenden, die nicht benötigt werden.

Wenn das Spiel angehalten wird, wird die **Audio::SuspendAudio**-Methode aufgerufen. Diese Methode ruft die [IXAudio2::StopEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine)-Methode auf, um die gesamte Audiowiedergabe anzuhalten. Obwohl **IXAudio2::StopEngine** sofort die gesamte Audioausgabe anhält, bleiben das Audiodiagramm und die Effektparameter erhalten (z. B. der Halleffekt, der beim Aufprallen der Murmel angewendet wird).

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }

    m_isAudioStarted = false;
}
```

Wenn das Spiel fortgesetzt wird, wird die **Audio::ResumeAudio**-Methode aufgerufen. Diese Methode verwendet die [IXAudio2::StartEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-startengine)-Methode, um die Audiowiedergabe neu zu starten. Da beim Aufruf von [IXAudio2::StopEngine](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) das Audiodiagramm und die Effektparameter erhalten bleiben, wird die Audioausgabe an der Stelle fortgesetzt, an der sie unterbrochen wurde.

```cpp
// Restarts the audio streams. A call to this method must match a previous call
// to SuspendAudio. This method causes audio to continue where it left off.
// If there is a problem with the restart, the m_engineExperiencedCriticalError
// flag is set. The next call to Render will recreate all the resources and
// reset the audio pipeline.
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## <a name="handling-headphones-and-device-changes"></a>Behandeln von Kopfhörern und Geräteänderungen

Marble Maze behandelt Fehler des XAudio2-Moduls wie zum Beispiel eine Änderung des Audiogeräts mithilfe von Modulrückrufen. Zu einer Geräteänderung kommt es meist, wenn der Benutzer des Spiels die Kopfhörer anschließt oder trennt. Sie sollten den Modulrückruf implementieren, der Geräteänderungen behandelt. Ansonsten wird die Soundwiedergabe im Spiel bis zum Neustart angehalten, wenn der Benutzer Kopfhörer anschließt oder entfernt.

**Audio.h** definiert die **AudioEngineCallbacks**-Klasse. Diese Klasse implementiert die [IXAudio2EngineCallback](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback)-Schnittstelle.

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private:
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

Über die [IXAudio2EngineCallback](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback)-Schnittstelle kann Ihr Code benachrichtigt werden, wenn Audioverarbeitungsereignisse auftreten und im Modul ein schwerwiegender Fehler auftritt. Um sich für Rückrufe zu registrieren, ruft Marble Maze nach der Erstellung des [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Objekts für das Musikmodul die [IXAudio2::RegisterForCallbacks](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-registerforcallbacks)-Methode in **Audio::CreateResources** auf.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze muss nicht benachrichtigt werden, wenn die Audioverarbeitung gestartet oder beendet wird. Daher implementiert es die Methoden [IXAudio2EngineCallback::OnProcessingPassStart](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassstart) und [IXAudio2EngineCallback::OnProcessingPassEnd](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassend) so, dass sie keine Aktion ausführen. Für die [IXAudio2EngineCallback::OnCriticalError](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-oncriticalerror) -Methode, Marble Maze-Ruft die **SetEngineExperiencedCriticalError** -Methode, die festlegt der **m\_ EngineExperiencedCriticalError** Flag.

```cpp
// Audio.cpp

// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// Audio.h (Audio class)

// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

Wenn ein schwerwiegender Fehler auftritt, wird die Audioverarbeitung beendet, und alle weiteren Aufrufe an XAudio2 schlagen fehl. Um diese Situation zu beheben, müssen Sie die XAudio2-Instanz freigeben und eine neue erstellen. Die **Audio::Render** Methode, die über die spielschleife jedes Bild aufgerufen wird, überprüft zuerst die **m\_EngineExperiencedCriticalError** Flag. Wenn das Kennzeichen festgelegt ist, löscht die Methode das Kennzeichen, gibt die aktuelle XAudio2-Instanz frei, initialisiert Ressourcen und startet dann die Hintergrundmusik.

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

Marble Maze verwendet auch die **m\_EngineExperiencedCriticalError** Flag zum Schutz vor XAudio2 aufrufen, wenn keine audio-Gerät verfügbar ist. Wenn dieses Kennzeichen festgelegt ist, verarbeitet die **MarbleMazeMain::Update**-Methode zum Beispiel kein Audio für Roll- oder Kollisionsereignisse. Die app versucht, die dem Audiomodul einzelbildweise reparieren, wenn dies erforderlich ist; allerdings die **m\_EngineExperiencedCriticalError** Flag kann immer festgelegt werden, wenn der Computer verfügt nicht über ein audio-Gerät oder den Kopfhörer getrennt sind und keine anderen verfügbaren audio-Gerät.

> [!CAUTION]
> Sie sollten grundsätzlich im Hauptteil eines Modulrückrufs keine Blockierungsvorgänge ausführen. Diese können zu Leistungsproblemen führen. Marble Maze legt im **OnCriticalError**-Rückruf ein Kennzeichen fest und behandelt später den Fehler in der normalen Audioverarbeitungsphase. Weitere Informationen zu XAudio2-Rückrufen finden Sie unter [XAudio2-Rückrufe](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-callbacks).

## <a name="conclusion"></a>Schlussbemerkung

Hier endet das Spielebeispiel für Marble Maze! Obwohl es sich um ein relativ einfaches Spiel handelt, enthält es viele wichtige Teile, die in jedem UWP-DirectX-Spiel enthalten sind, und ist ein gutes Beispiel, wenn Sie Ihr eigenes Spiel erstellen möchten.

Nachdem Sie fertig sind, versuchen Sie mit dem Quellcode zu spielen und sehen Sie, was passiert. Oder lesen Sie [Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-uwp-directx-game.md), ein anderes UWP-DirectX-Beispielspiel.

Möchten Sie mit DirectX fortfahren? Dann sehen Sie sich unsere Leitfäden auf [DirectX-Programmierung](directx-programming.md) an.

Wenn Sie die Entwicklung von Spielen auf UWP im Allgemeinen interessiert sind, finden Sie in der Dokumentation zur [Spieleprogrammierung](index.md) weitere Informationen.

## <a name="related-topics"></a>Verwandte Themen

* [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)