---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: In diesem Artikel wird gezeigt, wie die APIs im Windows.Media.Audio-Namespace zum Erstellen von Audiodiagrammen für Audiorouting sowie Misch- und Verarbeitungsszenarien verwendet werden.
title: Audiodiagramme
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 31889f94eaa489bdb6955b578c0ad4b18af6b606
ms.sourcegitcommit: dacbb7eef2cfffd7a8639e3a24ebda7b4eefae38
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2021
ms.locfileid: "105616783"
---
# <a name="audio-graphs"></a>Audiodiagramme



In diesem Artikel wird beschrieben, wie Sie die APIs im [**Windows. Media. audionamespace**](/uwp/api/Windows.Media.Audio) verwenden, um audiodiagramme für Audiorouting, Mischung und Verarbeitungs Szenarien zu erstellen.

Bei einem *audiodiagramm* handelt es sich um einen Satz vernetzter Audioknoten, über die Audiodaten fließen. 

- *Audioeingabeknoten* stellen Audiodaten aus audioeingabegeräten, Audiodateien oder aus benutzerdefiniertem Code für das Diagramm bereit. lat
- *Audioausgabeknoten* sind das Ziel von Audiodaten, die vom Diagramm verarbeitet wurden. Audiodaten können außerhalb des Diagramms an Audioausgabegeräte, Audiodateien oder benutzerdefinierten Code weitergeleitet werden. 

- *Submixknoten* kombinieren Audiodaten von mindestens einem Knoten in einer einzigen Ausgabe, die an andere Knoten in dem Diagramm weitergeleitet werden kann. 

Nachdem alle Knoten erstellt und die Verbindungen zwischen ihnen eingerichtet wurden, starten Sie einfach das Audiodiagramm. Daraufhin fließen die Audiodaten von den Eingabeknoten über Submixknoten an die Ausgabeknoten. Dank dieses Modells können Szenarien wie die Aufzeichnung vom Mikrofon eines Geräts in einer Audiodatei, das Wiedergeben von Audiodaten aus einer Datei auf einem Gerätelautsprecher oder das Mischen von Audiodaten aus mehreren Quellen schnell und einfach implementiert werden.

Weitere Szenarien werden durch das Hinzufügen von Audioeffekten zum Audiodiagramm ermöglicht. Jeder Knoten in einem Audiodiagramm kann mit null oder mehr Audioeffekten gefüllt werden, die die Audioverarbeitung für die Audiodaten durchführen, die den Knoten durchlaufen. Es gibt verschiedene integrierte Effekte wie Echo, Equalizer, Begrenzungen und Halleffekt, die mit nur wenigen Codezeilen an einen Audioknoten angefügt werden können. Sie können auch eigene benutzerdefinierte Audioeffekte erstellen, die genau wie die integrierten Effekte funktionieren.

> [!NOTE]
> Im [UWP-Beispiel für AudioGraph](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AudioCreation) wird der in dieser Übersicht erläuterte Code implementiert. Sie können das Beispiel herunterladen, um den Code im Kontext anzuzeigen oder ihn als Ausgangspunkt für Ihre eigene App zu verwenden.

## <a name="choosing-windows-runtime-audiograph-or-xaudio2"></a>Auswählen von Windows-Runtime-AudioGraph oder -XAudio2

Die Audiodiagramm-APIs der Windows-Runtime bieten Funktionen, die auch über die COM-basierten [XAudio2-APIs](/windows/desktop/xaudio2/xaudio2-apis-portal) implementiert werden können. Nachfolgend sind die Features des Audiodiagramm-Frameworks von Windows-Runtime aufgeführt, die von XAudio2 abweichen.

Die Audiodiagramm-APIs von Windows-Runtime

-   sind wesentlich benutzerfreundlicher als XAudio2.
-   können von C# verwendet werden und werden auch für C++ unterstützt.
-   können Audiodateien einschließlich komprimierter Dateiformate direkt verwenden. XAudio2 funktioniert nur auf Audiopuffern und stellt keine Datei-E/A-Funktionen bereit.
-   können die Audiopipeline mit geringer Latenzzeit in Windows 10 verwenden.
-   unterstützen eine automatische Endpunktumschaltung, wenn standardmäßige Endpunktparameter verwendet werden. Wenn der Benutzer beispielsweise vom Lautsprecher eines Geräts zu einem Headset wechselt, werden die Audiodaten automatisch an den neuen Eingang umgeleitet.

## <a name="audiograph-class"></a>AudioGraph-Klasse

Die [**AudioGraph**](/uwp/api/Windows.Media.Audio.AudioGraph)-Klasse ist das übergeordnete Element aller Knoten, aus denen das Diagramm besteht. Verwenden Sie dieses Objekt, um Instanzen aller Audioknotentypen zu erstellen. Erstellen Sie eine Instanz der **AudioGraph**-Klasse, indem Sie ein [**AudioGraphSettings**](/uwp/api/Windows.Media.Audio.AudioGraphSettings)-Objekt, das Konfigurationseinstellungen für das Diagramm enthält, initialisieren und dann [**AudioGraph.CreateAsync**](/uwp/api/windows.media.audio.audiograph.createasync) aufrufen. Die zurückgegebene [**CreateAudioGraphResult**](/uwp/api/Windows.Media.Audio.CreateAudioGraphResult)-Klasse ermöglicht den Zugriff auf das erstellte Audiodiagramm oder gibt einen Fehler zurück, wenn bei der Erstellung des Audiodiagramms ein Fehler auftritt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareAudioGraph":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetInitAudioGraph":::

-   Alle audioknotentypen werden mithilfe der Create- \* Methoden der **audiograph** -Klasse erstellt.
-   Die [**AudioGraph.Start**](/uwp/api/windows.media.audio.audiograph.start)-Methode bewirkt, dass das Audiodiagramm mit der Verarbeitung der Audiodaten beginnt. Die [**AudioGraph.Stop**](/uwp/api/windows.media.audio.audiograph.stop)-Methode beendet die Audioverarbeitung. Jeder Knoten im Diagramm kann während der Ausführung des Diagramms unabhängig gestartet und beendet werden. Es sind aber keine Knoten aktiv, wenn das Diagramm beendet wird. [**ResetAllNodes**](/uwp/api/windows.media.audio.audiograph.resetallnodes) bewirkt, dass alle Knoten im Diagramm alle Daten löschen, die sich derzeit in ihren Audiopuffern befinden.
-   Das [**QuantumStarted**](/uwp/api/windows.media.audio.audiograph.quantumstarted)-Ereignis tritt auf, wenn das Diagramm die Verarbeitung eines neuen Quantums von Audiodaten beginnt. Das [**QuantumProcessed**](/uwp/api/windows.media.audio.audiograph.quantumprocessed)-Ereignis tritt auf, wenn die Verarbeitung eines Quantums abgeschlossen ist.

-   Als einzige [**AudioGraphSettings**](/uwp/api/Windows.Media.Audio.AudioGraphSettings)-Eigenschaft ist [**AudioRenderCategory**](/uwp/api/Windows.Media.Render.AudioRenderCategory) erforderlich. Durch Angabe dieses Werts kann das System die Audiopipeline für die angegebene Kategorie optimieren.
-   Die Quantumgröße des Audiodiagramms bestimmt die Anzahl der Samples, die gleichzeitig verarbeitet werden. Standardmäßig beträgt die Quantumgröße 10 ms basierend auf der Standard-Samplingrate. Wenn Sie eine benutzerdefinierte Quantumgröße durch Festlegen der [**DesiredSamplesPerQuantum**](/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum)-Eigenschaft angeben, müssen Sie auch die [**QuantumSizeSelectionMode**](/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode)-Eigenschaft auf **ClosestToDesired** festlegen, oder der angegebene Wert wird ignoriert. Wenn dieser Wert verwendet wird, wählt das System eine Quantumgröße aus, die möglich nah an der von Ihnen angegebenen Größe liegt. Um die tatsächliche Quantumgröße zu bestimmen, überprüfen Sie die [**SamplesPerQuantum**](/uwp/api/windows.media.audio.audiograph.samplesperquantum)-Eigenschaft der **AudioGraph**-Klasse, nachdem sie erstellt wurde.
-   Wenn Sie das Audiodiagramm nur mit Dateien verwenden möchten und keine Ausgabe an ein Audiogerät planen, wird empfohlen, dass Sie die Standard-Quantumgröße verwenden, indem Sie die [**DesiredSamplesPerQuantum**](/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum)-Eigenschaft nicht festlegen.
-   Die [**DesiredRenderDeviceAudioProcessing**](/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing)-Eigenschaft bestimmt Verarbeitungsleistung, die das primäre Darstellungsgerät für die Ausgabe des Audiodiagramms durchführt. Über die **Default**-Einstellung kann das System die Standardaudioverarbeitung für die angegebene Audiowiedergabekategorie verwenden. Durch diese Verarbeitung kann der Sound der Audiodaten auf einigen Geräten wesentlich verbessert werden, insbesondere auf mobilen Geräten mit kleinen Lautsprechern. Durch die **Raw**-Einstellung kann die Leistung durch Minimieren der Signalverarbeitungsleistung verbessert werden. Dies kann jedoch zu einer schlechteren Tonqualität auf einigen Geräten führen.
-   Wenn für " [**quantumsizeselectionmode**](/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) " der Wert " **lowestlatency**" festgelegt ist, wird das audiodiagramm **automatisch für "** [**desiredrenderdeviceaudioprocessing**](/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing)" verwendet.
- Ab Windows 10, Version 1803, können Sie die [**audiographsettings. maxplaybackspeedfactor**](/uwp/api/windows.media.audio.audiographsettings.maxplaybackspeedfactor) -Eigenschaft festlegen, um einen maximalen Wert festzulegen, der für die Eigenschaften [**audiofileinputnode. playbackspeedfactor**](/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor), [**audioframeinputnode. playbackspeedfactor**](/uwp/api/windows.media.audio.audioframeinputnode.playbackspeedfactor)und [**mediasourceinputnode. playbackspeedfactor**](/uwp/api/windows.media.audio.mediasourceinputnode.playbackspeedfactor) verwendet wird. Wenn ein audiodiagramm einen mehr als 1 Wiedergabe Geschwindigkeitsfaktor unterstützt, muss das System zusätzlichen Arbeitsspeicher zuordnen, um einen ausreichenden Puffer an Audiodaten zu erhalten. Aus diesem Grund wird durch Festlegen von **maxplaybackspeedfactor** auf den niedrigsten von Ihrer APP benötigten Wert der Arbeitsspeicher Verbrauch der APP reduziert. Wenn Ihre APP nur Inhalte mit normaler Geschwindigkeit wieder gibt, empfiehlt es sich, maxplaybackspeedfactor auf 1 festzulegen.
-   Die [**EncodingProperties**](/uwp/api/windows.media.audio.audiographsettings.encodingproperties)-Eigenschaft bestimmt das vom Diagramm verwendete Audioformat. Es werden nur 32-Bit-Float-Formate unterstützt.
-   Die [**PrimaryRenderDevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice)-Eigenschaft legt das primäre Darstellungsgerät für das Audiodiagramm fest. Wenn Sie diese Eigenschaft nicht festlegen, wird das Standardsystemgerät verwendet. Das primäre Darstellungsgerät wird zur Berechnung der Quantumgrößen für andere Knoten im Diagramm verwendet. Wenn in dem System keine Audiowiedergabegeräte vorhanden sind, tritt bei der Erstellung des Audiodiagramms ein Fehler auf.

Sie können für das Audiodiagramm das Standard-Audiowiedergabegerät festlegen oder die [**Windows.Devices.Enumeration.DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Klasse verwenden, um eine Liste der verfügbaren Audiowiedergabegeräte abzurufen, indem Sie [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) aufrufen und die von [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) zurückgegebene Auswahl des Audiowiedergabegeräts übergeben. Sie können eines der zurückgegebenen **DeviceInformation**-Objekte programmgesteuert auswählen oder die Benutzeroberfläche anzeigen, damit der Benutzer ein Gerät auswählen und dieses dann zum Festlegen der [**PrimaryRenderDevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice)-Eigenschaft verwenden kann.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetEnumerateAudioRenderDevices":::

##  <a name="device-input-node"></a>Geräteingabeknoten

Eine Geräteingabeknoten liefert Audiodaten von einem an das System angeschlossenen Audioaufnahmegerät wie etwa einem Mikrofon an das Diagramm. Erstellen Sie ein [**DeviceInputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceInputNode)-Objekt, das das standardmäßige Audioaufnahmegerät des Systems verwendet, indem Sie die [**CreateDeviceInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync)-Methode aufrufen. Geben Sie eine [**AudioRenderCategory**](/uwp/api/Windows.Media.Render.AudioRenderCategory)-Enumeration an, damit das System die Audiopipeline für die angegebene Kategorie optimieren kann.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateDeviceInputNode":::

Wenn Sie ein bestimmtes audioerfassungs Gerät für den Geräte Eingabe Knoten angeben möchten, können Sie mit der [**Windows. Devices. Enumeration. DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) -Klasse eine Liste der verfügbaren audioerfassungs Geräte des Systems abrufen, indem Sie [**findallasync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) aufrufen und die von [**Windows. Media. Devices. mediadevice. getaudiocaptureselector**](/uwp/api/windows.media.devices.mediadevice.getaudiocaptureselector)zurückgegebene Audiowiedergabe-Geräteauswahl übergeben Sie können eines der zurückgegebenen **DeviceInformation** -Objekte Programm gesteuert auswählen oder die Benutzeroberfläche anzeigen, um dem Benutzer zu gestatten, ein Gerät auszuwählen und es dann an [**createdeviceinputnodeasync**](/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync)zu übergeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetEnumerateAudioCaptureDevices":::

##  <a name="device-output-node"></a>Gerätausgabeknoten

Ein Gerätausgabeknoten überträgt Audiodaten von dem Diagramm an ein Audiowiedergabegerät, z. B. Lautsprecher oder ein Headset. Erstellen Sie eine [**DeviceOutputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode)-Klasse, indem Sie die [**CreateDeviceOutputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createdeviceoutputnodeasync)-Methode aufrufen. Der Ausgabeknoten verwendet die [**PrimaryRenderDevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice)-Eigenschaft des Audiodiagramms.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceOutputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateDeviceOutputNode":::

##  <a name="file-input-node"></a>Dateieingabeknoten

Mit einem Dateieingabeknoten können Sie Daten aus einer Audiodatei in das Diagramm übertragen. Erstellen Sie eine [**AudioFileInputNode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode)-Klasse, indem Sie die [**CreateFileInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync)-Methode aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFileInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFileInputNode":::

-   Dateieingabeknoten unterstützen die Dateiformate MP3, WAV, WMA und M4A.
-   Legen Sie die [**StartTime**](/uwp/api/windows.media.audio.audiofileinputnode.starttime)-Eigenschaft so fest, dass in der Datei der Zeitoffset angegeben wird, an dem die Wiedergabe beginnen soll. Wenn diese Eigenschaft null ist, wird der Anfang der Datei verwendet. Legen Sie die [**EndTime**](/uwp/api/windows.media.audio.audiofileinputnode.endtime)-Eigenschaft so fest, dass in der Datei der Zeitoffset angegeben wird, an dem die Wiedergabe enden soll. Wenn diese Eigenschaft null ist, wird das Ende der Datei verwendet. Die Startzeit muss vor der Endzeit liegen, und der Wert für die Endzeit muss kleiner oder gleich der Dauer der Audiodatei sein. Sie können die Richtigkeit anhand des [**Duration**](/uwp/api/windows.media.audio.audiofileinputnode.duration)-Eigenschaftswerts prüfen.
-   Suchen Sie eine Position in der Audiodatei, indem Sie die [**Seek**](/uwp/api/windows.media.audio.audiofileinputnode.seek)-Methode aufrufen und den Zeitoffset in der Datei angeben, an den die Wiedergabeposition verschoben werden soll. Der angegebene Wert muss zwischen den Eigenschaften [**StartTime**](/uwp/api/windows.media.audio.audiofileinputnode.starttime) und [**EndTime**](/uwp/api/windows.media.audio.audiofileinputnode.endtime) liegen. Die aktuelle Wiedergabeposition des Knotens können Sie mit der schreibgeschützten [**Position**](/uwp/api/windows.media.audio.audiofileinputnode.position)-Eigenschaft abrufen.
-   Aktivieren Sie Schleifen für die Audiodatei, indem Sie die [**LoopCount**](/uwp/api/windows.media.audio.audiofileinputnode.loopcount)-Eigenschaft festlegen. Wenn diese nicht Null ist, gibt dieser Wert die Anzahl der Wiederholungen der Datei nach der ersten Wiedergabe an. Wenn Sie **LoopCount** beispielsweise auf 1 festlegen, wird die Datei insgesamt zweimal wiedergegeben. Wenn Sie den Wert auf 5 festlegen, wird die Datei insgesamt sechs Mal wiedergegeben. Indem Sie **LoopCount** auf Null setzen, wird die Datei in einer Schleife unbegrenzt wiedergegeben. Um die Schleife zu beenden, setzen Sie den Wert auf 0 fest.
-   Legen Sie zum Anpassen der Geschwindigkeit, mit der die Audiodatei wiedergegeben wird, die [**PlaybackSpeedFactor**](/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor)-Eigenschaft fest. Der Wert 1 zeigt die ursprüngliche Geschwindigkeit der Datei an. Der Wert 0,5 legt die halbe Geschwindigkeit und der Wert 2 ist die doppelte Geschwindigkeit fest.

##  <a name="mediasource-input-node"></a>MediaSource-Eingabe Knoten

Die [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) -Klasse bietet eine gängige Methode zum Referenzieren von Medien aus verschiedenen Quellen und bietet ein gängiges Modell für den Zugriff auf Mediendaten, unabhängig vom zugrunde liegenden Medienformat, bei dem es sich um eine Datei auf einem Datenträger, einem Stream oder einer adaptiven streamingnetzwerkquelle Mit einem [* * mediasourceaudioinputnode](/uwp/api/windows.media.audio.mediasourceaudioinputnode) -Knoten können Sie Audiodaten von einer **MediaSource** in das audiodiagramm leiten. Erstellen Sie einen **mediasourceaudioinputnode** durch Aufrufen von [**createmediasourceaudioinputnodeasync**](/uwp/api/windows.media.audio.audiograph.createmediasourceaudioinputnodeasync#Windows_Media_Audio_AudioGraph_CreateMediaSourceAudioInputNodeAsync_Windows_Media_Core_MediaSource_), und übergeben Sie ein **MediaSource** -Objekt, das den Inhalt darstellt, den Sie wiedergeben möchten. Ein [* * ermittelungsmediasourceaudioinputnoderesult](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult) wird zurückgegeben, mit dem Sie den Status des Vorgangs ermitteln können, indem Sie die Eigenschaft [**Status**](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.status) überprüfen. Wenn der Status **erfolgreich** ist, können Sie den erstellten **mediasourceaudioinputnode** durchzugreifen auf die [**Node**](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.node) -Eigenschaft erhalten. Das folgende Beispiel zeigt die Erstellung eines Knotens aus einem adaptivemediasource-Objekt, das Content Streaming über das Netzwerk darstellt. Weitere Informationen zum Arbeiten mit **MediaSource** finden Sie unter [Medienelemente, Wiedergabelisten und Spuren](media-playback-with-mediasource.md). Weitere Informationen zum Streamen von Medieninhalten über das Internet finden Sie unter [Adaptive Streaming](adaptive-streaming.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareMediaSourceInputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateMediaSourceInputNode":::

Um eine Benachrichtigung zu erhalten, wenn die Wiedergabe das Ende des **MediaSource** -Inhalts erreicht hat, registrieren Sie einen Handler für das [**mediasourceabgeschlossene**](/uwp/api/windows.media.audio.mediasourceaudioinputnode.mediasourcecompleted) -Ereignis. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetRegisterMediaSourceCompleted":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetMediaSourceCompleted":::

Obwohl die Wiedergabe einer Datei von diskis wahrscheinlich immer erfolgreich abgeschlossen wird, kann es sein, dass Medien, die von einer Netzwerkquelle gestreamt werden, während der Wiedergabe aufgrund einer Änderung an der Netzwerkverbindung oder anderen Problemen außerhalb der Kontrolle des audiodiagramms fehlschlagen. Wenn eine **MediaSource** während der Wiedergabe nicht mehr verfügbar ist, wird das [**unrecoverableerroreingetreten**](/uwp/api/windows.media.audio.audiograph.unrecoverableerroroccurred) -Ereignis vom audiograph aufgerufen. Sie können den Handler für dieses Ereignis verwenden, um das audiodiagramm zu beenden und zu verwerfen und dann das Diagramm erneut zu initialisieren. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetRegisterUnrecoverableError":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUnrecoverableError":::

##  <a name="file-output-node"></a>Dateiausgabeknoten

Mit einem Dateiausgabeknoten können Sie die Audiodaten aus dem Diagramm in eine Audiodatei umleiten. Erstellen Sie eine [**AudioFileOutputNode**](/uwp/api/Windows.Media.Audio.AudioFileOutputNode)-Klasse, indem Sie die [**CreateFileOutputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createfileoutputnodeasync)-Methode aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFileOutputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFileOutputNode":::

-   Dateiausgabeknoten unterstützen die Dateiformate MP3, WAV, WMA und M4A.
-   Um die Verarbeitung des Knotens zu beenden, rufen Sie erst die [**AudioFileOutputNode.Stop**](/uwp/api/windows.media.audio.audiofileoutputnode.stop)-Methode und dann die [**AudioFileOutputNode.FinalizeAsync**](/uwp/api/windows.media.audio.audiofileoutputnode.finalizeasync)-Methode auf. Andernfalls wird eine Ausnahme ausgelöst.

##  <a name="audio-frame-input-node"></a>Audioframe-Eingabeknoten

Mit einem Audioframe-Eingabeknoten können Sie Audiodaten, die Sie in Ihrem eigenen Code generieren, in das Audiodiagramm übertragen. Dies ermöglicht Szenarien wie das Erstellen eines benutzerdefinierten Softwaresynthesizers. Erstellen Sie einen [**audioframeinputnode**](/uwp/api/Windows.Media.Audio.AudioFrameInputNode) durch Aufrufen von [**createframeinputnode**](/uwp/api/windows.media.audio.audiograph.createframeinputnode).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFrameInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFrameInputNode":::

Das [**FrameInputNode.QuantumStarted**](/uwp/api/windows.media.audio.audioframeinputnode.quantumstarted)-Ereignis wird ausgelöst, wenn das Audiodiagramm bereit ist, die Verarbeitung des nächsten Quantums von Audiodaten zu verarbeiten. Sie stellen die benutzerdefinierten generierten Audiodaten vom Handler für dieses Ereignis bereit.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetQuantumStarted":::

-   Durch das an den **QuantumStarted**-Ereignishandler übergebene [**FrameInputNodeQuantumStartedEventArgs**](/uwp/api/Windows.Media.Audio.FrameInputNodeQuantumStartedEventArgs)-Objekt wird die [**RequiredSamples**](/uwp/api/windows.media.audio.frameinputnodequantumstartedeventargs.requiredsamples)-Eigenschaft verfügbar, die angibt, wie viele Sample das Audiodiagramm füllen muss, damit das Quantum verarbeitet wird.
-   Rufen Sie die [**AudioFrameInputNode.AddFrame**](/uwp/api/windows.media.audio.audioframeinputnode.addframe)-Methode auf, um ein mit Audiodaten gefülltes [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame)-Objekt an das Diagramm zu übergeben.
- Ein neuer Satz von APIs für die Verwendung von **mediaframereader** mit Audiodaten wurde in Windows 10, Version 1803, eingeführt. Diese APIs ermöglichen Ihnen das Abrufen von **audioframe** -Objekten aus einer Medien Frame-Quelle, die mithilfe der **addframe** -Methode an einen **frameinputnode** übermittelt werden kann. Weitere Informationen finden Sie unter [Verarbeiten von Audioframes mit mediaframereader](process-audio-frames-with-mediaframereader.md).
-   Nachfolgend sehen Sie ein Beispiel einer Implementierung der **GenerateAudioData**-Hilfsmethode.

Zum Auffüllen eines [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame)-Objekts mit Audiodaten benötigen Sie Zugriff auf den zugrunde liegenden Speicherpuffer des Audioframes. Initialisieren Sie zu diesem Zweck die **IMemoryBufferByteAccess**-COM-Schnittstelle, indem Sie dem Namespace den folgenden Code hinzufügen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetComImportIMemoryBufferByteAccess":::

Der folgende Code zeigt ein Beispiel einer Implementierung der **GenerateAudioData**-Hilfsmethode, die ein [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame)-Objekt erstellt und dieses mit Audiodaten füllt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetGenerateAudioData":::

-   Da diese Methode auf den Rohdatenpuffer zugreift, der Windows-Runtime-Typen zugrunde liegt, muss diese mithilfe des Schlüsselworts **unsafe** deklariert werden. Außerdem müssen Sie das Projekt in Microsoft Visual Studio so konfigurieren, dass die Kompilierung von unsicherem Code zugelassen wird, indem Sie die Seite **Eigenschaften** des Projekts öffnen, auf die Eigenschaftenseite **Build** klicken und das Kontrollkästchen **Unsicheren Code zulassen** aktivieren.
-   Initialisieren Sie im **Windows.Media**-Namespace eine neue Instanz des [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame)-Objekts, indem Sie die gewünschte Puffergröße an den Konstruktor übergeben. Die Puffergröße ist die Sample-Anzahl multipliziert mit der Größe der einzelnen Sample.
-   Rufen Sie das [**AudioBuffer**](/uwp/api/Windows.Media.AudioBuffer)-Objekt des Audioframes ab, indem Sie die [**LockBuffer**](/uwp/api/windows.media.audioframe.lockbuffer)-Methode aufrufen.
-   Rufen Sie eine Instanz der [**imemorybufferbyteaccess**](/previous-versions/mt297505(v=vs.85)) -com-Schnittstelle aus dem Audiopuffer ab, indem Sie [**createreferenzieren**](/uwp/api/windows.media.audiobuffer.createreference).
-   Rufen Sie einen Zeiger auf die Rohdaten des Audiopuffers ab, indem Sie die [**IMemoryBufferByteAccess.GetBuffer**](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)-Methode aufrufen und in den Beispieldatentyp der Audiodaten umwandeln.
-   Füllen Sie den Puffer mit Daten, und geben Sie das [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame)-Objekt für die Übermittlung an das Audiodiagramm zurück.

##  <a name="audio-frame-output-node"></a>Audioframe-Ausgabeknoten

Mit einem Audioframe-Ausgabeknoten können Sie eine Audiodatenausgabe aus dem Audiodiagramm mit benutzerdefiniertem Code empfangen und verarbeiten. Ein Beispielszenario hierfür ist das Durchführen einer Signalanalyse für die Audioausgabe. Erstellen Sie ein [**AudioFrameOutputNode**](/uwp/api/Windows.Media.Audio.AudioFrameOutputNode)-Objekt, indem Sie die [**CreateFrameOutputNode**](/uwp/api/windows.media.audio.audiograph.createframeoutputnode)-Methode aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFrameOutputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFrameOutputNode":::

Das [**audiograph. quantumstarted**](/uwp/api/Windows.Media.Audio.AudioGraph.QuantumStarted) -Ereignis wird ausgelöst, wenn das audiodiagramm mit der Verarbeitung eines Quantums von Audiodaten begonnen hat. Sie können innerhalb des Handlers für dieses Ereignis auf die Audiodaten zugreifen. 

> [!NOTE]
> Wenn Sie Audioframes in einem regulären Rhythmus abrufen möchten, das mit dem audiodiagramm synchronisiert ist, rufen Sie [audioframeoutputnode. GetFrame](/uwp/api/windows.media.audio.audioframeoutputnode.GetFrame) aus dem synchronen **quantumstarted** -Ereignishandler auf. Das **quantumschlag** -Ereignis wird asynchron ausgelöst, nachdem die Audioengine die Audioverarbeitung abgeschlossen hat, was bedeutet, dass der Rhythmus unregelmäßig sein kann. Daher sollte das **quantumprocessing** -Ereignis nicht für die synchronisierte Verarbeitung von audioframe-Daten verwendet werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetQuantumStartedFrameOutput":::

-   Rufen Sie die [**GetFrame**](/uwp/api/windows.media.audio.audioframeoutputnode.getframe)-Methode auf, um ein mit Audiodaten gefülltes [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame)-Objekt mit Audiodaten aus dem Diagramm abzurufen.
-   Nachfolgend sehen Sie ein Beispiel einer Implementierung der **ProcessFrameOutput**-Hilfsmethode.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetProcessFrameOutput":::

-   Wie in dem Beispiel oben für den Audioframe-Eingabeknoten müssen Sie die **IMemoryBufferByteAccess**-COM-Schnittstelle deklarieren und das Projekt so konfigurieren, dass unsicherer Code zugelassen wird, damit Sie auf den zugrunde liegenden Audiopuffer zugreifen können.
-   Rufen Sie das [**AudioBuffer**](/uwp/api/Windows.Media.AudioBuffer)-Objekt des Audioframes ab, indem Sie die [**LockBuffer**](/uwp/api/windows.media.audioframe.lockbuffer)-Methode aufrufen.
-   Rufen Sie eine Instanz der **imemorybufferbyteaccess** -com-Schnittstelle aus dem Audiopuffer ab, indem Sie [**createreferenzieren**](/uwp/api/windows.media.audiobuffer.createreference).
-   Rufen Sie einen Zeiger auf die Rohdaten des Audiopuffers ab, indem Sie die **IMemoryBufferByteAccess.GetBuffer**-Methode aufrufen und in den Beispieldatentyp der Audiodaten umwandeln.

## <a name="node-connections-and-submix-nodes"></a>Knotenverbindungen und Submixknoten

Alle Eingabeknotentypen machen die **AddOutgoingConnection**-Methode verfügbar, die die vom Knoten produzierten Audiodaten an den Knoten weiterleitet, der an die Methode übergeben wird. Im folgenden Beispiel wird eine Verbindung zwischen [**AudioFileInputNode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode) und [**AudioDeviceOutputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) hergestellt. Dabei handelt es sich um ein einfaches Setup für die Wiedergabe einer Audiodatei über den Lautsprecher des Geräts.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection1":::

Von einem Eingabeknoten können mehrere Verbindungen zu anderen Knoten erstellt werden. Im folgenden Beispiel wird eine Verbindung zwischen [**AudioFileInputNode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode) und [**AudioFileOutputNode**](/uwp/api/Windows.Media.Audio.AudioFileOutputNode) hinzugefügt. Die Audiodaten aus der Audiodatei werden nun über den Lautsprecher des Geräts wiedergegeben und auch in eine Audiodatei geschrieben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection2":::

Ausgabeknoten können auch mehrere Verbindungen von anderen Knoten empfangen. Im folgenden Beispiel wird eine Verbindung zwischen den Knoten [**AudioDeviceInputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) und [**AudioDeviceOutput**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) hergestellt. Da der Ausgabeknoten Verbindungen vom Dateieingabeknoten und dem Geräteingabeknoten aufweist, enthält die Ausgabe eine Audiomischung aus beiden Quellen. **AddOutgoingConnection** bietet eine Überladung, mit der Sie einen Verstärkungswert für das über die Verbindung laufende Signal angeben können.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection3":::

Obwohl Ausgabeknoten Verbindungen von mehreren Knoten annehmen können, sollten Sie eine Zwischenmischung von Signalen von einem oder mehreren Knoten erstellen, bevor Sie die Mischung an eine Ausgabe übergeben. Angenommen, Sie möchten die Ebene festlegen oder Effekte auf eine Untergruppe der Audiosignale in einem Diagramm anwenden. Verwenden Sie hierfür die [**AudioSubmixNode**](/uwp/api/Windows.Media.Audio.AudioSubmixNode)-Klasse. Sie können keine Verbindung zu einem Submixknoten von einer oder mehreren Eingabeknoten oder anderen Submixknoten herstellen. Im folgenden Beispiel wird ein neuer Submixknoten zu [**AudioGraph.CreateSubmixNode**](/uwp/api/windows.media.audio.audiograph.createsubmixnode) erstellt. Anschließend werden Verbindungen von einem Dateiausgabeknoten und einem Frameausgabeknoten zum Submixknoten hinzugefügt. Schließlich wird der Submixknoten mit einem Dateiausgabeknoten verbunden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateSubmixNode":::

## <a name="starting-and-stopping-audio-graph-nodes"></a>Starten und Beenden von Audiodiagrammknoten

Wenn die [**AudioGraph.Start**](/uwp/api/windows.media.audio.audiograph.start)-Methode aufgerufen wird, beginnt das Audiodiagramm mit der Verarbeitung der Audiodaten. Jeder Knoten umfasst eine **Start**- und **Stop**-Methode, die bewirken, dass der jeweilige Knoten die Verarbeitung von Daten beginnt oder beendet. Bei Aufruf der [**AudioGraph.Stop**](/uwp/api/windows.media.audio.audiograph.stop)-Methode wird die gesamte Verarbeitung in allen Knoten unabhängig vom Status der einzelnen Knoten beendet. Der Status der einzelnen Knoten kann jedoch festgelegt werden, während das Audiodiagramm beendet wird. Sie können beispielsweise die **Stop**-Methode in einem einzelnen Knoten aufrufen, während das Diagramm beendet wird. Rufen Sie anschließend die **AudioGraph.Start**-Methode auf, damit der einzelne Knoten angehalten bleibt.

Mit allen Knotentypen wird die **ConsumeInput**-Eigenschaft verfügbar. Diese lest bei Festlegung auf „false“ zu, dass der Knoten die Audioverarbeitung fortsetzt. Gleichzeitig verhinder sie, dass Audiodaten von anderen Knoten verbraucht werden.

Mit allen Knotentypen wird die **Reset** -Methode verfügbar. Sie bewirkt, dass der Knoten alle Audiodaten löscht, die sich aktuell in seinem Puffer befinden.

## <a name="adding-audio-effects"></a>Hinzufügen von Audioeffekten

Mit der Audiodiagramm-API können Sie Audioeffekte zu jedem Knotentyp in einem Diagramm hinzufügen. Ausgabeknoten, Eingabeknoten und Submixknoten können jeweils eine unbegrenzte Anzahl von Audioeffekten aufweisen. Eine Einschränkung erfolgt lediglich durch die Funktionen der Hardware. Im folgenden Beispiel ist das Hinzufügen des integrierten Echoeffekts zu einem Submixknoten dargestellt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddEffect":::

-   Alle Audioeffekte implementieren die [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition)-Schnittstelle. Mit jedem Knoten wird eine **EffectDefinitions**-Eigenschaft verfügbar, welche die Liste der auf diesen Knoten angewendeten Effekte darstellt. Fügen Sie einen Effekt hinzu, indem Sie sein Definitionsobjekt zu der Liste hinzufügen.
-   Es gibt mehrere Effektdefinitionsklassen, die im **Windows.Media.Audio**-Namespace bereitgestellt werden. Dazu gehören:
    -   [**EchoEffectDefinition**](/uwp/api/Windows.Media.Audio.EchoEffectDefinition)
    -   [**EqualizerEffectDefinition**](/uwp/api/Windows.Media.Audio.EqualizerEffectDefinition)
    -   [**LimiterEffectDefinition**](/uwp/api/Windows.Media.Audio.LimiterEffectDefinition)
    -   [**ReverbEffectDefinition**](/uwp/api/Windows.Media.Audio.ReverbEffectDefinition)
-   Sie können Ihre eigenen Audioeffekte zum Implementieren von [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) erstellen. Wenden Sie diese anschließend auf einen beliebigen Knoten in einem Audiodiagramm an.
-   Mit jedem Knotentyp wird eine **DisableEffectsByDefinition**-Methode verfügbar, die alle Effekte in der **EffectDefinitions**-Liste des Knotens deaktiviert, die mithilfe der angegebenen Definition hinzugefügt wurden. **EnableEffectsByDefinition** aktiviert die Effekte mit der angegebenen Definition.

## <a name="spatial-audio"></a>Räumliche Audiowiedergabe
Ab Windows 10, Version 1607, unterstützt **AudioGraph** die räumliche Audiowiedergabe. Dabei können Sie eine Position im dreidimensionalen Raum angeben, an der Audiodaten von einem Eingabe- oder Submixknoten ausgegeben werden. Sie können auch eine Form und Richtung für die Audioausgabe angeben, eine Geschwindigkeit festlegen, die für die Dopplerverschiebung der Audiodaten des Knotens verwendet wird, und ein Abklingmodell definieren, das beschreibt, wie Klang mit zunehmender Entfernung gedämpft wird. 

Um einen Emitter zu erstellen, können Sie zunächst eine Form definieren, in der der Sound vom Emitter projiziert wird. Die Klangausbreitung kann kegel- oder kugelförmig sein. Die [**AudioNodeEmitterShape**](/uwp/api/Windows.Media.Audio.AudioNodeEmitterShape)-Klasse bietet statische Methoden zum Erstellen dieser Formen. Als Nächstes erstellen Sie ein Abklingmodell. Es definiert, wie die Lautstärke des vom Emitter ausgegebenen Sounds mit zunehmender Entfernung vom Listener (Zuhörer) abnimmt. Mit der [**CreateNatural**](/uwp/api/windows.media.audio.audionodeemitterdecaymodel.createnatural)-Methode wird ein Abklingmodell erstellt. Es emuliert das natürliche Abklingen von Sound anhand eines auf einem Abstandsquadrat basierten Abnahmemodells. Erstellen Sie zuletzt ein [**AudioNodeEmitterSettings**](/uwp/api/Windows.Media.Audio.AudioNodeEmitterSettings)-Objekt. Dieses Objekt wird derzeit nur zum Aktivieren und Deaktivieren der geschwindigkeitsbasierten Dopplerdämpfung der vom Emitter ausgegebenen Audiodaten verwendet. Rufen Sie den [**AudioNodeEmitter**](/uwp/api/windows.media.audio.audionodeemitter.-ctor)-Konstruktor auf, und übergeben Sie die gerade erstellten Initialisierungsobjekte. Der Emitter wird standardmäßig am Ursprung positioniert, Sie können seine Position aber auch mit der [**Position**](/uwp/api/windows.media.audio.audionodeemitter.position)-Eigenschaft festlegen.

> [!NOTE]
> Audioknotenemitter können nur Monoaudiodaten mit einer Abtastrate von 48 kHz verarbeiten. Die Verwendung von Stereoaudiodaten oder Audio mit einer anderen Abtastrate führt zu einer Ausnahme.

Sie weisen den Emitter beim Erstellen einem Audioknoten zu, indem Sie die überladene Erstellungsmethode für den gewünschten Knotentyp verwenden. In diesem Beispiel wird [**CreateFileInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync) verwendet, um einen Dateieingabeknoten aus einer angegebenen Datei und das [**AudioNodeEmitter**](/uwp/api/Windows.Media.Audio.AudioNodeEmitter)-Objekt zu erstellen, das Sie dem Knoten zuordnen möchten.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateEmitter":::

Die [**AudioDeviceOutputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode)-Klasse, die Audiodaten aus dem Diagramm für den Benutzer ausgibt, verfügt über ein Listener-Objekt, auf das mit der [**Listener**](/uwp/api/windows.media.audio.audiodeviceoutputnode.listener)-Eigenschaft zugegriffen wird. Sie gibt die Position, Ausrichtung und Geschwindigkeit des Benutzers im dreidimensionalen Raum an. Die Positionen aller Emitter im Diagramm sind relativ zur Position und Ausrichtung des listenerobjekts. Der Listener befindet sich standardmäßig am Ursprung (0,0,0) und ist nach vorne entlang der Z-Achse ausgerichtet. Sie können die Position und Ausrichtung jedoch mit der [**Position**](/uwp/api/windows.media.audio.audionodelistener.position)-Eigenschaft und [**Orientation**](/uwp/api/windows.media.audio.audionodelistener.orientation)-Eigenschaft festlegen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetListener":::

Sie können die Position, Geschwindigkeit und Richtung von Emittern zur Laufzeit aktualisieren, um die Bewegung einer Audioquelle durch den dreidimensionalen Raum zu simulieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUpdateEmitter":::

Sie können auch die Position, Geschwindigkeit und Ausrichtung des Listener-Objekts zur Laufzeit aktualisieren, um die Bewegung des Benutzers durch den dreidimensionalen Raum zu simulieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUpdateListener":::

Die räumliche Audiowiedergabe wird standardmäßig mit dem HRTF-Algorithmus (Head-relative Transfer Function, kopfbezogene Übertragungsfunktion) von Microsoft berechnet, um die Audiowiedergabe basierend auf der Form, Geschwindigkeit und Position relativ zum Listener zu dämpfen. Sie können die [**SpatialAudioModel**](/uwp/api/windows.media.audio.audionodeemitter.spatialaudiomodel)-Eigenschaft auf **FoldDown** festlegen, um eine einfache Stereomischmethode zum Simulieren der räumlichen Audiowiedergabe zu verwenden. Diese ist zwar weniger genau, erfordert dafür aber weniger CPU-Leistung und Arbeitsspeicher.

## <a name="see-also"></a>Siehe auch
- [Medienwiedergabe](media-playback.md)
 

 
