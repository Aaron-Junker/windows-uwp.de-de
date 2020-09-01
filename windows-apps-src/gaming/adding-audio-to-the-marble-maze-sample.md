---
title: Hinzufügen von Audio zum Marble Maze-Beispiel
description: In diesem Dokument werden die wichtigsten Methoden beschrieben, die Sie berücksichtigen sollten, wenn Sie mit Audio arbeiten. Außerdem erfahren Sie, wie diese Methoden in Marble Maze angewendet werden.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, Audiofunktionen, Spiele, Beispiel
ms.localizationpriority: medium
ms.openlocfilehash: df1be02cd962f086e52609550e9cae65e5280b71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172134"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>Hinzufügen von Audio zum Marble Maze-Beispiel

In diesem Dokument werden die wichtigsten Methoden beschrieben, die Sie berücksichtigen sollten, wenn Sie mit Audio arbeiten. Außerdem erfahren Sie, wie diese Methoden in Marble Maze angewendet werden. Marble Maze verwendet [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) , um Audioressourcen aus Dateien zu laden, und [XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) , um Audiodaten zu kombinieren und wiederzugeben und um Effekte auf Audiodaten anzuwenden.

Marble Maze spielt Musik im Hintergrund und verwendet auch die Verwendung von Spielereignissen, um Spielereignisse anzugeben, z. b. wenn die Marmor auf eine Wand trifft. Wichtig ist bei der Implementierung, dass Marble Maze einen Hall- oder Echoeffekt verwendet, um den Klang einer aufprallenden Murmel zu simulieren. Die Implementierung des Halleffekts bewirkt, dass Sie Echos in kleineren Räumen schneller und lauter hören. In größeren Räumen dagegen sind die Echos leiser und nicht so schnell zu hören.

> [!NOTE]
> Den diesem Dokument entsprechenden Beispielcode finden Sie im [DirectX Marble Maze-Spielbeispiel](https://github.com/microsoft/Windows-appsample-marble-maze).

Hier sind einige der wichtigsten in diesem Dokument erörterten Punkte für das Arbeiten mit Audio in Ihrem Spiel:

- Ziehen Sie die Verwendung von Media Foundation zum Decodieren von Audioressourcen und XAudio2 zum Wiedergeben von Audio in Betracht. Wenn Sie aber bereits einen Mechanismus zum Laden von Audioressourcen haben, der in einer UWP-App funktioniert, können Sie diesen Mechanismus verwenden.

- Ein Audiodiagramm enthält eine Quellstimme für jeden aktiven Sound, null oder mehr Submixstimmen und eine Masterstimme. Quellstimmen können in Submixstimmen und/oder die Masterstimme eingespeist werden. Submixstimmen können in andere Submixstimmen oder die Masterstimme eingespeist werden.

- Wenn die Dateien für die Hintergrundmusik groß sind, können Sie die Musik in kleinere Puffer streamen, damit weniger Arbeitsspeicher verbraucht wird.

- Halten Sie gegebenenfalls die Audiowiedergabe an, wenn die App nicht mehr den Fokus hat, nicht mehr sichtbar ist oder angehalten wird. Setzen Sie die Wiedergabe fort, wenn Ihre App wieder den Fokus erhält, wieder sichtbar wird oder fortgesetzt wird.

- Legen Sie Audiokategorien fest, die den Rollen der einzelnen Sounds entsprechen. Beispielsweise verwenden Sie in der Regel **audiocategory \_ gameMedia** für Game Background Audio und **audiocategory \_ gameeffects** für Soundeffekte.

- Behandeln Sie Geräteänderungen, einschließlich Kopfhörern, in dem Sie alle Audioressourcen und -schnittstellen freigeben und erneut erstellen.

- Komprimieren Sie gegebenenfalls Audiodateien, wenn Sie den Speicherplatz und die Streamingkosten minimieren müssen. Anderenfalls können Sie die Audiodateien unkomprimiert lassen, damit sie schneller geladen werden.

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>Einführung in XAudio2 und Microsoft Media Foundation

XAudio2 ist eine Low-Level-Audiobibliothek für Windows, die speziell Audio in Spielen unterstützt. Sie enthält eine digitale Signalverarbeitung (Digital Signal Processing, DSP) und ein Audiodiagrammmodul für Spiele. Als Erweiterung der Vorgänger DirectSound und XAudio unterstützt XAudio2 Computertrends wie zum Beispiel SIMD-Gleitkommaarchitekturen und HD-Audio. Außerdem werden die komplexeren Soundverarbeitungsanforderungen aktueller Spiele unterstützt.

Im [Dokument zu den wichtigsten Konzepten von XAudio2](/windows/desktop/xaudio2/xaudio2-key-concepts) werden die wichtigsten Konzepte für die Verwendung von XAudio2 erläutert. Die Konzepte im Kurzüberblick:

- Die [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Schnittstelle bildet den Kern des XAudio2-Moduls. Marble Maze verwendet diese Schnittstelle, um Stimmen zu erstellen und Benachrichtigungen zu empfangen, wenn sich das Ausgabegerät ändert oder Fehler auftreten.

- Eine **Stimme** verarbeitet, passt Audiodaten an und gibt Sie wieder.

- Eine **Quell Stimme** ist eine Sammlung von Audiokanälen (Mono, 5,1 usw.) und stellt einen Stream von Audiodaten dar. In XAudio2 ist eine Quellstimme der Ausgangspunkt der Audioverarbeitung. Normalerweise werden die Sounddaten aus einer externen Quelle wie beispielsweise einer Datei oder einem Netzwerk geladen und an eine Quellstimme gesendet. In Marble Maze werden Sounddaten mithilfe von [Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) aus Dateien geladen. Media Foundation wird weiter unten in diesem Dokument vorgestellt.

- Eine **unter Mischungs Sprache** verarbeitet Audiodaten. Bei dieser Verarbeitung kann zum Beispiel der Audiostream geändert werden oder mehrere Streams werden zu einem Stream kombiniert. Marble Maze verwendet Submixe, um den Halleffekt zu erstellen.

- Eine **Mastering Voice** kombiniert Daten aus Quell-und teilmischungs Stimmen und sendet diese Daten an die Audiohardware.

- Ein **audiodiagramm** enthält eine Quell Stimme für jeden aktiven Sound, 0 (null) oder mehr teilmischungs Stimmen und nur eine Stimme.

- Ein **Rückruf** informiert den Client Code darüber, dass ein Ereignis in einer Stimme oder in einem Engine-Objekt aufgetreten ist. Mithilfe von Rückrufen können Sie den Arbeitsspeicher wiederverwenden, wenn XAudio2 mit einem Puffer fertig ist, reagieren, wenn sich das Audiogerät ändert (z. B. wenn Sie Kopfhörer anschließen oder trennen) usw. Die [Verarbeitung von Kopf-und Geräteänderungen](#handling-headphones-and-device-changes) weiter unten in diesem Dokument erläutert, wie Marble Maze diesen Mechanismus zum Behandeln von Geräteänderungen verwendet.

Marble Maze verwendet zwei Audiomodule (mit anderen Worten: zwei [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Objekte) zum Verarbeiten von Audio. Die Hintergrundmusik wird von einem Modul verarbeitet, und die andere Engine verarbeitet renderingsounds.

Außerdem muss Marble Maze für jedes Modul eine Masterstimme erstellen. Denken Sie daran, dass ein Mastermodul Audiostreams in einem Stream kombiniert und diesen Stream an die Audiohardware sendet. Der Stream für die Hintergrundmusik, eine Quellstimme, gibt Daten an eine Masterstimme und an zwei Submixstimmen aus. Die Submixstimmen führen den Halleffekt aus.

Media Foundation ist eine Multimediabibliothek, die viele Audio- und Videoformate unterstützt. XAudio2 und Media Foundation ergänzen sich gegenseitig. Marble Maze verwendet Media Foundation, um audioassets aus Dateien zu laden, und verwendet XAudio2, um Audioinhalte wiederzugeben. Sie müssen Media Foundation nicht verwenden, um Audioressourcen zu laden. Wenn Sie bereits einen Mechanismus zum Laden von Audioressourcen haben, der in einer UWP-App funktioniert, verwenden Sie diesen Mechanismus. [Audiodatei, Video und Kamera](../audio-video-camera/index.md) erläutert verschiedene Möglichkeiten der Implementierung von Audiodaten in einer UWP-app.

Weitere Informationen zu XAudio2 finden Sie im [Programmierhandbuch](/windows/desktop/xaudio2/programming-guide). Weitere Informationen zu Media Foundation finden Sie unter [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk).

## <a name="initializing-audio-resources"></a>Initialisieren von Audioressourcen

Bei Marble Mazes wird eine Windows Media Audio Datei (. WMA) für die Hintergrundmusik und WAV-Dateien (WAV-Dateien) für die Verwendung von Spiel Geräuschen verwendet. Diese Formate werden von Media Foundation unterstützt. Obwohl das WAV-Dateiformat nativ von XAudio2 unterstützt wird, muss das Dateiformat von einem Spiel manuell analysiert werden, um die entsprechenden XAudio2-Datenstrukturen auszufüllen. Marble Maze verwendet Media Foundation, um die Arbeit mit WAV-Dateien zu erleichtern. Die vollständige Liste der von Media Foundation unterstützten Medienformate finden Sie unter [Unterstützte Medienformate in Media Foundation](/windows/desktop/medfound/supported-media-formats-in-media-foundation). Marble Maze verwendet keine getrennten Entwurfszeit- und Laufzeit-Audioformate und keine Unterstützung für XAudio2-ADPCM-Komprimierung. Weitere Informationen zur ADPCM-Komprimierung in XAudio2 finden Sie in der Übersicht über [ADPCM](/windows/desktop/xaudio2/adpcm-overview).

Die Methode " **Audio:: samateresources** ", die von **marblemazemain:: loaddeferredresources**aufgerufen wird, lädt die Audiodatenströme aus den Dateien, initialisiert die Objekte der XAudio2-Engine und erstellt die Quelle, die teilmischung und die Mastering-Stimmen.

### <a name="creating-the-xaudio2-engines"></a>Erstellen der XAudio2-Module

Bedenken Sie, dass Marble Maze für jedes verwendete Audiomodul ein [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Objekt erstellt. Um eine Audioengine zu erstellen, rufen Sie die [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) -Methode auf. Das folgende Beispiel zeigt, wie Marble Maze das Audiomodul für die Verarbeitung der Hintergrundmusik erstellt.

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

Marble Maze führt einen ähnlichen Schritt aus, um das Audiomodul zu erstellen, das Spiele Sounds wieder gibt.

Zwischen der Arbeit mit der [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)-Schnittstelle in einer UWP-App und der Arbeit mit einer Desktop-App gibt es zwei Unterschiede. Erstens ist es nicht erforderlich, [CoInitializeEx](/windows/desktop/api/combaseapi/nf-combaseapi-coinitializeex) vor dem Aufrufen von [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) aufzurufen. Außerdem unterstützt **IXAudio2** die Geräteaufzählung nicht mehr. Weitere Informationen zum Aufzählen von Audiogeräten finden Sie unter [Enumerieren von Geräten](/previous-versions/windows/apps/hh464977(v=win.10)).

### <a name="creating-the-mastering-voices"></a>Erstellen der Masterstimmen

Im folgenden Beispiel wird gezeigt, wie die **audiomethode:: up-** Methode die "Mastering"-Stimme für die Hintergrundmusik mithilfe der [IXAudio2::](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) -Methode erstellt. In diesem Beispiel ist **m \_ musicmasteringvoice** ein [IXAudio2MasteringVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2masteringvoice) -Objekt. Wir geben zwei Eingabe Kanäle an: Dies vereinfacht die Logik für den Hall Effekt. 

Wir geben 48000 als Eingabe Stichproben Rate an. Diese Abtastrate wurde ausgewählt, da sie ein ausgewogenes Verhältnis von Audioqualität und Umfang der erforderlichen CPU-Verarbeitung darstellt. Eine höhere Abtastrate würde mehr CPU-Verarbeitung erfordern, ohne eine merkliche Qualitätssteigerung zu bewirken. 

Zum Schluss geben wir **AudioCategory_GameMedia** als audiostreamkategorie an, damit Benutzer Musik von einer anderen Anwendung lauschen können, während Sie das Spiel spielen. Wenn eine Musik-app abgespielt wird, werden von Windows alle Stimmen, die von der Option **audiocategory \_ gameMedia** erstellt werden, von Windows abgleichen. Der Benutzer hört weiterhin gamesounds, weil Sie von der Option " **audiocategory \_ gameeffects** " erstellt werden. Weitere Informationen zu audiokategorien finden Sie unter [ \_ \_ audiostreamkategorie](/windows/desktop/api/audiosessiontypes/ne-audiosessiontypes-_audio_stream_category).

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

Die **Audio:: samateresources-** Methode führt einen ähnlichen Schritt aus, um die Mastering-Stimme für die Wiedergabe Klänge zu erstellen, mit der Ausnahme, dass Sie **audiocategory- \_ gameeffects** für den Parameter " *streamcategory* " angibt. Dies ist die Standardeinstellung.

### <a name="creating-the-reverb-effect"></a>Erstellen des Halleffekts

Sie können für jede Stimme mit XAudio2 Effektsequenzen erstellen, die Audio verarbeiten. Eine solche Sequenz wird als Effektkette bezeichnet. Verwenden Sie Effektketten, wenn Sie mindestens einen Effekt auf eine Stimme anwenden möchten. Effektketten können destruktiv sein, d. h., jeder Effekt in der Kette kann den Audiopuffer überschreiben. Diese Eigenschaft ist wichtig, da XAudio2 nicht garantiert, dass Ausgabepuffer lautlos initialisiert werden. Effektobjekte werden in XAudio2 durch plattformübergreifende Audioverarbeitungsobjekte (Audio Processing Object, XAPO) dargestellt. Weitere Informationen zu XAPO finden Sie in der [XAPO-Übersicht](/windows/desktop/xaudio2/xapo-overview).

Führen Sie beim Erstellen einer Effektkette die folgenden Schritte aus:

1. Erstellen Sie das Effektobjekt.

2. Füllt eine [XAUDIO2 \_ Effect- \_ deskriptorstruktur](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) mit Effekt Daten auf.

3. Füllt eine [XAUDIO2- \_ Effekt \_ Ketten](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) Struktur mit Daten auf.

4. Wenden Sie die Effektkette auf eine Stimme an.

5. Füllen Sie eine Effektparameterstruktur auf und wenden Sie sie auf den Effekt an.

6. Deaktivieren oder aktivieren Sie den Effekt nach Bedarf.

Die **Audio**-Klasse definiert die **CreateReverb**-Methode, um die Effektkette zu erstellen, die den Halleffekt implementiert. Diese Methode ruft die [XAudio2CreateReverb](/windows/desktop/api/xaudio2fx/nf-xaudio2fx-xaudio2createreverb) -Methode auf, um ein ** &lt; IUnknown &gt; -comptr** -Objekt ( **soundeffectxapo**) zu erstellen, das als unter Mischungs Stimme für den Hall Effekt fungiert.

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

Die [XAUDIO2 \_ Effect- \_ deskriptorstruktur](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) enthält Informationen über ein xapo zur Verwendung in einer Effekt Kette, z. b. die Ziel Anzahl der Ausgabekanäle. Die **audiomethode:: samatereverb-** Methode erstellt ein **XAUDIO2 \_ Effect \_ Descriptor** -Objekt, **soundeffectdescriptor**, das auf den deaktivierten Zustand festgelegt ist, zwei Ausgabekanäle verwendet und auf **soundeffectxapo** für den Hall Effekt verweist. **soundeffectdescriptor** startet im deaktivierten Zustand, da das Spielparameter festlegen muss, bevor die Auswirkungen der Änderung von Spiel Geräuschen beginnen. Marble Maze verwendet zwei Ausgabekanäle, um die Logik für den Halleffekt zu vereinfachen.

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

Wenn Ihre Effektkette mehrere Effekte enthält, benötigen Sie für jeden Effekt ein Objekt. Die [XAUDIO2 \_ Effect \_ ](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) -Struktur enthält das Array von [XAUDIO2 \_ Effect- \_ deskriptorobjekten](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) , die am Effekt beteiligt sind. Das folgende Beispiel zeigt, wie die **Audio::CreateReverb**-Methode den einen Effekt für die Implementierung des Halleffekts angibt.

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

Die **Audio::CreateReverb**-Methode ruft die [IXAudio2::CreateSubmixVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsubmixvoice)-Methode auf, um die Submixstimme für den Effekt zu erstellen. Er gibt das [XAUDIO2- \_ Effekt \_ Ketten](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) Objekt **sounde ffectchain**für den Parameter " *Peer-ectchain* " an, um die Effekt Kette der Stimme zuzuordnen. Außerdem gibt Marble Maze zwei Ausgabekanäle und eine Abtastrate von 48 Kilohertz an.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> Wenn Sie eine vorhandene Wirkungskette an eine vorhandene teilmischungs-Stimme anfügen oder die aktuelle Wirkungskette ersetzen möchten, verwenden Sie die [IXAudio2Voice::](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectchain) *-Methode.

Die **audiomethode:: samatereverb-** Methode ruft [IXAudio2Voice:: seteffectparameters](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectparameters) auf, um zusätzliche Parameter festzulegen, die mit der Auswirkung verknüpft sind. Diese Methode akzeptiert eine für den Effekt spezifische Parameterstruktur. Ein XAUDIO2FX- **m_reverbParametersSmall**- [ \_ \_ Parameter](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters) Objekt, das die Effektparameter für den Hall enthält, wird in der Methode " **Audio:: Initialize** " initialisiert, da jeder "Reverb"-Effekt die gleichen Parameter gemeinsam nutzt. Das folgende Beispiel zeigt, wie die **Audio::Initialize**-Methode die Hallparameter für Nahfeld-Halleffekte initialisiert.

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

Dieses Beispiel verwendet die Standardwerte für die meisten Hallparameter, legt aber **DisableLateField** auf TRUE fest, um den Nahfeld-Halleffekt anzugeben, **EarlyDiffusion** auf 4, um flache Oberflächen in der Nähe zu simulieren, und **LateDiffusion** auf 15, um sehr diffuse entfernte Oberflächen zu simulieren. Flache Oberflächen in der Nähe verursachen Echos, die Sie schneller und lauter hören. Diffuse entfernte Oberflächen verursachen leisere und nicht so schnell hörbare Echos. Sie können mit den Hall-Werten experimentieren, um den gewünschten Effekt in Ihrem Spiel zu erzielen, oder Sie können die **ReverbConvertI3DL2ToNative** -Methode verwenden, um branchenübliche I3DL2 (interaktive 3D-audiorenderführungs-Richtlinien Level 2,0) zu verwenden

Das folgende Beispiel zeigt, wie **Audio::CreateReverb** die Hallparameter festlegt. **newsubmix** ist ein [IXAudio2SubmixVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2submixvoice)* *-Objekt. **Parameter** ist ein [XAUDIO2FX \_ - \_ Parameter](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)Objekt *.

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

Die **audiomethode:: samatereverb-** Methode wird beendet, indem der Effekt mithilfe von [IXAudio2Voice:: enableeffect](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-enableeffect) aktiviert wird, wenn das **enableeffect** -Flag festgelegt ist. Außerdem wird das Volume mit [IXAudio2Voice:: SetVolume](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setvolume) und der Ausgabe Matrix mithilfe von [IXAudio2Voice:: setoutputmatrix](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setoutputmatrix)festgelegt. Mit diesem Teil wird die volle Lautstärke (1.0) festgelegt und dann die Lautstärkematrix so angegeben, dass die linke und die rechte Eingabe sowie der linke und der rechte Ausgabelautsprecher stumm sind. Dies geschieht, da später anderer Code zwischen den beiden Halleffekten überblendet (und dabei den Übergang von der Nähe zu einer Wand zu einem großen Raum simuliert) oder bei Bedarf beide Halleffekte stummschaltet. Wenn die Stummschaltung des Hallpfads später aufgehoben wird, legt das Spiel die Matrix {1.0f, 0.0f, 0.0f, 1.0f} fest, um die linke Hallausgabe an die linke Eingabe der Masterstimme und die rechte Hallausgabe an die rechte Eingabe der Masterstimme weiterzuleiten.

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

In Marble Maze wird die **Audio:: up-** Methode viermal aufgerufen: zweimal für die Hintergrundmusik und zwei Mal für die Wiedergabe Sounds. Das folgende Beispiel zeigt, wie Marble Maze die **CreateReverb**-Methode für die Hintergrundmusik aufruft.

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

Eine Liste der möglichen Quellen für Effekte, die Sie mit XAudio2 verwenden können, finden Sie unter [XAudio2-Audioeffekte](/windows/desktop/xaudio2/xaudio2-audio-effects).

### <a name="loading-audio-data-from-file"></a>Laden von Audiodaten aus einer Datei

Marble Maze definiert die **Mediastreamer** -Klasse, die Media Foundation verwendet, um Audioressourcen aus Dateien zu laden. Marble Maze verwendet zum Laden jeder Audiodatei ein **MediaStreamer**-Objekt.

Marble Maze ruft die **MediaStreamer::Initialize**-Methode auf, um die einzelnen Audiostreams zu initialisieren. So ruft die **Audio::CreateResources**-Methode **MediaStreamer::Initialize** auf, um den Audiostream für die Hintergrundmusik zu initialisieren:

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

Die **Mediastreamer:: Initialisieren** -Methode beginnt mit dem Aufruf der [MFStartup](/windows/desktop/api/mfapi/nf-mfapi-mfstartup) -Methode, um Media Foundation zu initialisieren. **MF_VERSION** ist ein in " **mfapi. h**" definiertes Makro, das als zu verwendende Version von Media Foundation angegeben wird.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

**MediaStreamer::Initialize** ruft dann [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) auf, um ein [IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)-Objekt zu erstellen. Ein **IMF sourcereader** -Objekt, **m_reader**, liest Mediendaten aus der durch **URL**angegebenen Datei.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

Die **Mediastreamer:: Initialize** -Methode erstellt dann ein [imfmediatype](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype) -Objekt mithilfe von [mfkreatemediatype](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) , um das Format des Audiostreams zu beschreiben. Ein Audioformat hat zwei Typen: einen Haupttyp und einen Untertyp. Der Haupttyp definiert das allgemeine Format der Medien, zum Beispiel Video, Audio, Skript usw. Der Untertyp definiert das Format, zum Beispiel PCM, ADPCM oder WMA.

Die **Mediastreamer:: Initialize** -Methode verwendet die [imfattributes:: SetGuid](/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setguid) -Methode, um den Haupttyp ([MF_MT_MAJOR_TYPE](/windows/desktop/medfound/mf-mt-major-type-attribute)) als Audio (**mfmediatype \_ -Audio**) und den nebentyp ([MF_MT_SUBTYPE](/windows/desktop/medfound/mf-mt-subtype-attribute)) als unkomprimiertes PCM-Audio (**mfaudioformat \_ PCM**) anzugeben. **MF_MT_MAJOR_TYPE** und **MF_MT_SUBTYPE** sind [Media Foundation Attribute](/windows/desktop/medfound/media-foundation-attributes). **MFMediaType_Audio** und **MFAudioFormat_PCM** sind Typ-und Untertyp-GUIDs. Weitere Informationen finden Sie unter [audiomedientypen](/windows/desktop/medfound/audio-media-types) . Die [IMFSourceReader::SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)-Methode ordnet den Medientyp dem StreamReader zu.

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

Die **Mediastreamer:: Initialize** -Methode ruft dann das vollständige Ausgabemedien Format aus Media Foundation mithilfe von [imfsourcereader:: getcurrentmediatype](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) ab und ruft die [mfkreatewaveformatexfrommfmediatype](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) -Methode auf, um den Media Foundation audiomedientyp in eine [WaveFormatEx](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) -Struktur zu konvertieren. Die **WAVEFORMATEX**-Struktur definiert das Format von Waveform-Audiodaten. Marble Maze erstellt mithilfe dieser Struktur die Quellstimmen und wendet den Tiefpassfilter auf den Sound für das Rollen der Murmel an.

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
> Die [mfkreatewaveformatexfrommfmediatype](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) -Methode verwendet **CoTaskMemAlloc** , um das [WaveFormatEx](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) -Objekt zuzuordnen. Rufen Sie daher unbedingt **CoTaskMemFree** auf, wenn Sie dieses Objekt nicht mehr verwenden.

 

Die **Mediastreamer:: Initialize** -Methode wird beendet, indem die Länge des Streams ( **m \_ maxstreamlengthinbytes**) in Bytes berechnet wird. Zu diesem Zweck ruft Sie die [imfsourcereader:: getpresentationattribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) -Methode auf, um die Dauer des Audiostreams in 100-Nanosecond-Einheiten zu erhalten, konvertiert die Dauer in Abschnitte und multipliziert mit der durchschnittlichen Datenübertragungsrate in Bytes pro Sekunde. Das Marble Maze verwendet diesen Wert später, um den Puffer zuzuordnen, der die einzelnen Spiel Klänge enthält.

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

Marble Maze erstellt XAudio2-Quellstimmen für die Wiedergabe der einzelnen Spielsounds und der Musik in Quellstimmen. Die **Audio-** Klasse definiert ein [IXAudio2SourceVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2sourcevoice) -Objekt für die Hintergrundmusik und ein Array von **soundeffectdata** -Objekten, die die Wiedergabe Sounds enthalten. Die **SoundEffectData**-Struktur enthält das **IXAudio2SourceVoice**-Objekt für einen Effekt und definiert außerdem andere Daten im Zusammenhang mit Effekten, beispielsweise den Audiopuffer. " **Audio. h** " definiert die **sounentvent** -Enumeration. Marble Maze verwendet diese Enumeration, um die einzelnen Spiel Klänge zu identifizieren. Die **audioklasse** verwendet diese Enumeration auch, um das Array von **soundeffectdata** -Objekten zu indizieren.

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

Die folgende Tabelle zeigt die Beziehung zwischen den einzelnen Werten, die Datei, in der sich die zugeordneten Sounddaten befinden, und eine kurze Beschreibung der einzelnen Sounds. Die Audiodateien befinden sich im Ordner " ** \\ Media \\ Audiodatei** ".

| SoundEvent-Wert  | Dateiname      | BESCHREIBUNG                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | Wird wiedergegeben, wenn die Murmel rollt.                              |
| FallingEvent      | MarbleFall.wav | Wird wiedergegeben, wenn die Murmel aus dem Labyrinth fällt.               |
| CollisionEvent    | MarbleHit.wav  | Wird wiedergegeben, wenn die Murmel mit dem Labyrinth kollidiert.           |
| CheckpointEvent   | Checkpoint.wav | Wird wiedergegeben, wenn die Murmel über einen Prüfpunkt rollt.         |
| MenuChangeEvent   | MenuChange.wav | Wird abgespielt, wenn der Benutzer das aktuelle Menü Element ändert. |
| MenuSelectedEvent | MenuSelect.wav | Wird abgespielt, wenn der Benutzer ein Menü Element auswählt.           |

 

Das folgende Beispiel zeigt, wie die **Audio::CreateResources**-Methode die Quellstimme für die Hintergrundmusik erstellt. Die [XAUDIO2- \_ Sende \_ deskriptorstruktur](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_send_descriptor) definiert die Zielziel-Stimme von einer anderen Stimme und gibt an, ob ein Filter verwendet werden soll. Marble Maze Ruft die **Audio:: setsoundeffectfilter-** Methode auf, um die Filter zum Ändern des Sounds der Kugel beim Rollup zu verwenden. Die [XAUDIO2 \_ Voice \_ ](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_voice_sends) Send-Struktur definiert den Satz von Stimmen, mit dem Daten aus einer einzelnen Ausgabesprache empfangen werden. Marble Maze sendet Daten von der Quellstimme an die Masterstimme (für den direkten oder unveränderten Teil eines wiedergegebenen Sounds) und an die zwei Submixstimmen, die den mit einem Effekt versehenen oder hallenden Teil eines wiedergegebenen Sounds implementieren.

Die [IXAudio2::CreateSourceVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsourcevoice)-Methode erstellt und konfiguriert eine Quellstimme. Sie akzeptiert eine [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)-Struktur, die das Format der an die Stimme gesendeten Audiopuffer definiert. Wie bereits erwähnt verwendet Marble Maze das PCM-Format.

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


Eine Quellstimme wird im angehaltenen Zustand erstellt. Marble Maze startet die Hintergrundmusik in der Spielschleife. Beim ersten Aufruf von **marblemazemain:: Update** wird " **Audio:: Start** " aufgerufen, um die Hintergrundmusik zu starten.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

Die **Audio::Start**-Methode ruft [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) auf, um die Verarbeitung der Quellstimme für die Hintergrundmusik zu starten.

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
> In einem Spiel, in dem die meisten Raumgrößen relativ identisch sind, können Sie ein basieres Reverb-Modell verwenden. Zum Beispiel können Sie eine Halleinstellung für alle Räume verwenden oder für jeden Raum eine vordefinierte Halleinstellung erstellen.

Die **Audio::CreateResources**-Methode verwendet Media Foundation zum Laden der Hintergrundmusik. An dieser Stelle hat die Quellstimme aber noch keine Audiodaten, mit denen sie arbeiten kann. Darüber hinaus muss die Quellstimme regelmäßig mit Daten aktualisiert werden, damit die Wiedergabe der Hintergrundmusik, die in einer Schleife wiedergegeben wird, fortgesetzt wird.

Damit die Quellstimme immer mit Daten gefüllt ist, aktualisiert die Spielschleife die Audiopuffer in jedem Frame. Mit der **marblemazemain:: Rendering** -Methode wird " **Audio:: Rendering** " aufgerufen, um den Audiopuffer der Hintergrundmusik zu verarbeiten. Die **audioklasse** definiert ein Array mit drei audiopuffern, **m \_ audiobuffers**. Jeder Puffer enthält 64 KB (65536 Byte) an Daten. Die Schleife liest Daten aus dem Media Foundation-Objekt und schreibt diese Daten in die Quellstimme, bis die Warteschlange drei Puffer enthält.

> [!CAUTION]
> Obwohl Marble Maze einen Puffer mit einer Größe von 64 KB zum Speichern von Musikdaten verwendet, müssen Sie möglicherweise einen größeren oder kleineren Puffer verwenden. Die Größe hängt von den Anforderungen Ihres Spiels ab.

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

Die Schleife ist auch zuständig, wenn das Media Foundation-Objekt das Ende des Streams erreicht. In diesem Fall ruft Sie die [imfsourcereader:: setcurrentposition](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentposition) -Methode auf, um die Position der Audioquelle zurückzusetzen.

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

Um audioschleifen für einen einzelnen Puffer (oder für einen gesamten Sound, der vollständig in den Arbeitsspeicher geladen wurde) zu implementieren, können Sie das [XAUDIO2_BUFFER](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer):: loopCount-Feld auf **XAUDIO2- \_ Schleife \_ unendlich** festlegen, wenn Sie den Sound initialisieren. Marble Maze verwendet diese Technik, um den Rollsound der Murmel wiederzugeben.

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

Für die Hintergrundmusik verwaltet Marble Maze aber die Puffer direkt, sodass es die Menge des verwendeten Arbeitsspeichers besser steuern kann. Wenn Ihre Musikdateien zu groß sind, können Sie die Musikdaten in kleinere Puffer streamen. Dadurch können Sie die Größe des Arbeitsspeichers besser auf die Verarbeitung und das Streamen von Audiodaten im Spiel abstimmen.

> [!TIP]
> Wenn Ihr Spiel eine niedrige oder abweichende Framerate aufweist, kann die Verarbeitung von Audiodaten im Haupt Thread unerwartete Pausen oder POPs im Audioformat verursachen, da die Audioengine nicht über genügend gepufferte Audiodaten verfügt, mit denen Sie arbeiten können. Wenn dieses Problem bei Ihrem Spiel zu erwarten ist, denken Sie darüber nach, Audio in einem getrennten Thread zu verarbeiten, in dem kein Rendering ausgeführt wird. Dieser Ansatz ist besonders hilfreich auf Computern mit mehreren Prozessoren, da Ihr Spiel die Prozessoren verwenden kann, die sich im Leerlauf befinden.

## <a name="reacting-to-game-events"></a>Reagieren auf Spielereignisse

Die **audioklasse** stellt Methoden wie **playsoundeffect**bereit. " **issounenffectstarted**", " **stopsoundebug**", " **setsoundebug**", " **setsoundebug**" und " **setsoundebug** ", damit das Spiel steuern kann, wann Sounds abgespielt und angehalten werden, und um Sound Eigenschaften wie Volume und Tonhöhe zu steuern. Wenn z. b. der Marmor aus dem Maze entfernt wird, ruft **marblemazemain:: Update** die **Audio::P laysoundeffect-** Methode auf, um den **fallingevent** -Sound wiederzugeben.

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

Die **Audio::PlaySoundEffect**-Methode ruft die [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start)-Methode auf, um die Wiedergabe des Sounds zu starten. Wenn die **IXAudio2SourceVoice::Start**-Methode bereits aufgerufen wurde, wird sie nicht erneut gestartet. Anschließend führt **Audio::PlaySoundEffect** eine benutzerdefinierte Logik für bestimmte Sounds aus.

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

Für andere Sounds als den Rollsound ruft die **Audio::PlaySoundEffect**-Methode [IXAudio2SourceVoice::GetState](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-getstate) auf, um die Anzahl der Puffer zu ermitteln, die die Quellstimme wiedergibt. Sie ruft [IXAudio2SourceVoice::SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer) auf, um die Audiodaten für den Sound der Eingabewarteschlange der Stimme hinzuzufügen, wenn keine Puffer aktiv sind. Mit der **Audio::PlaySoundEffect**-Methode kann der Kollisionssound auch zweimal hintereinander wiedergegeben werden. Dies ist beispielsweise der Fall, wenn die Murmel mit einer Ecke des Labyrinths kollidiert.

Wie bereits beschrieben, verwendet die audioklasse das Flag " **XAUDIO2 \_ Loop \_ Infinite** ", wenn Sie den Sound für das parallele Ereignis initialisiert. Der Sound wird in einer Schleife wiedergegeben, wenn **Audio::PlaySoundEffect** erstmals für dieses Ereignis aufgerufen wird. Um die Wiedergabelogik für den Rollsound zu vereinfachen, schaltet Marble Maze den Sound stumm, anstatt ihn anzuhalten. Wenn sich die Geschwindigkeit der Murmel ändert, werden Tonhöhe und Lautstärke des Sounds geändert, damit er realistischer klingt. Im folgenden Beispiel wird gezeigt, wie die **marblemazemain:: Update** -Methode die Tonhöhe und das Volume des Marmors aktualisiert, wenn die Geschwindigkeit geändert wird und wie Sie den Sound abhört, indem das Volume auf NULL festgelegt wird, wenn der Marmor anhält.

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

In der [Marble Maze-Anwendungs Struktur](marble-maze-application-structure.md) wird beschrieben, wie Marble Maze Suspend und Resume unterstützt. Wenn das Spiel angehalten wird, wird auch die Audiowiedergabe angehalten. Wenn das Spiel fortgesetzt wird, wird die Audiowiedergabe an der Stelle fortgesetzt, an der sie unterbrochen wurde. Damit orientieren wir uns an der bewährten Methode, keine Ressourcen zu verwenden, die nicht benötigt werden.

Wenn das Spiel angehalten wird, wird die **Audio::SuspendAudio**-Methode aufgerufen. Diese Methode ruft die [IXAudio2::StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine)-Methode auf, um die gesamte Audiowiedergabe anzuhalten. Obwohl **IXAudio2::StopEngine** sofort die gesamte Audioausgabe anhält, bleiben das Audiodiagramm und die Effektparameter erhalten (z. B. der Halleffekt, der beim Aufprallen der Murmel angewendet wird).

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

Wenn das Spiel fortgesetzt wird, wird die **Audio::ResumeAudio**-Methode aufgerufen. Diese Methode verwendet die [IXAudio2::StartEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-startengine)-Methode, um die Audiowiedergabe neu zu starten. Da beim Aufruf von [IXAudio2::StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) das Audiodiagramm und die Effektparameter erhalten bleiben, wird die Audioausgabe an der Stelle fortgesetzt, an der sie unterbrochen wurde.

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

Marble Maze verwendet Engine-Rückrufe, um XAudio2-Engine-Fehler zu behandeln, z. b. wenn das Audiogerät geändert wird. Zu einer Geräteänderung kommt es meist, wenn der Benutzer des Spiels die Kopfhörer anschließt oder trennt. Sie sollten den Modulrückruf implementieren, der Geräteänderungen behandelt. Ansonsten wird die Soundwiedergabe im Spiel bis zum Neustart angehalten, wenn der Benutzer Kopfhörer anschließt oder entfernt.

**Audio. h** definiert die **audioenginecallbacks** -Klasse. Diese Klasse implementiert die [IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback)-Schnittstelle.

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

Über die [IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback)-Schnittstelle kann Ihr Code benachrichtigt werden, wenn Audioverarbeitungsereignisse auftreten und im Modul ein schwerwiegender Fehler auftritt. Um sich für Rückrufe zu registrieren, ruft Marble Maze die [IXAudio2:: registerforcallbacks](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-registerforcallbacks) -Methode in " **Audio:: forateresources**" auf, nachdem das [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) -Objekt für die Musik-Engine erstellt wurde.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze muss nicht benachrichtigt werden, wenn die Audioverarbeitung gestartet oder beendet wird. Daher implementiert es die Methoden [IXAudio2EngineCallback::OnProcessingPassStart](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassstart) und [IXAudio2EngineCallback::OnProcessingPassEnd](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassend) so, dass sie keine Aktion ausführen. Bei der [IXAudio2EngineCallback:: oncriticalerror](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-oncriticalerror) -Methode ruft Marble Maze die **setengineexperiercedcriticalerror** -Methode auf, die das **m \_ engineexperitencedcriticalerror** -Flag festlegt.

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

Wenn ein schwerwiegender Fehler auftritt, wird die Audioverarbeitung beendet, und alle weiteren Aufrufe an XAudio2 schlagen fehl. Um diese Situation zu beheben, müssen Sie die XAudio2-Instanz freigeben und eine neue erstellen. Die **audiomethode: Rendermethode** , die von der Spiel Schleife jedes Frames aufgerufen wird, überprüft zuerst das **m \_ engineexperidercedcriticalerror** -Flag. Wenn das Kennzeichen festgelegt ist, löscht die Methode das Kennzeichen, gibt die aktuelle XAudio2-Instanz frei, initialisiert Ressourcen und startet dann die Hintergrundmusik.

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

Marble Maze verwendet auch das **m \_ engineexperiencedcriticalerror** -Flag zum Schutz vor dem Aufrufen von XAudio2, wenn kein Audiogerät verfügbar ist. Die **marblemazemain:: Update** -Methode verarbeitet z. b. keine Audiodaten für parallele oder kollisionsereignisse, wenn dieses Flag festgelegt ist. Die APP versucht, die Audioengine jedes Frame zu reparieren, wenn dies erforderlich ist. Das **m \_ engineexperiencedcriticalerror** -Flag kann jedoch immer festgelegt werden, wenn der Computer nicht über ein Audiogerät verfügt oder der Kopfhörer nicht getrennt ist und kein anderes verfügbares Audiogerät vorhanden ist.

> [!CAUTION]
> Führen Sie als Regel keine blockierenden Vorgänge im Text eines Engine-Rückrufs aus. Diese können zu Leistungsproblemen führen. Marble Maze legt im **OnCriticalError**-Rückruf ein Kennzeichen fest und behandelt später den Fehler in der normalen Audioverarbeitungsphase. Weitere Informationen zu XAudio2-Rückrufen finden Sie unter [XAudio2-Rückrufe](/windows/desktop/xaudio2/xaudio2-callbacks).

## <a name="conclusion"></a>Zusammenfassung

Das ist das Marmor Maze-Spielbeispiel. Obwohl es sich um ein relativ einfaches Spiel handelt, enthält es viele wichtige Teile, die in jedem UWP DirectX-Spiel zu finden sind, und es ist ein gutes Beispiel, das Sie beim Erstellen eines eigenen Spiels beachten sollten.

Nachdem Sie den Vorgang abgeschlossen haben, versuchen Sie es mit dem Quellcode, und sehen Sie sich an, was passiert. Weitere Informationen finden Sie unter [Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-uwp-directx-game.md), einem anderen UWP DirectX-Spielbeispiel.

Sind Sie bereit, mit DirectX fortzufahren? Sehen Sie sich die Anleitungen unter [DirectX-Programmierung](directx-programming.md)an.

Wenn Sie an der Entwicklung von Spielen für die UWP im allgemeinen interessiert sind, lesen Sie die Dokumentation unter [Spielprogrammierung](index.md).

## <a name="related-topics"></a>Zugehörige Themen

* [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)