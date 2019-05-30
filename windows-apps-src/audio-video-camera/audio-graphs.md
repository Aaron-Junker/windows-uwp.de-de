---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: In diesem Artikel wird gezeigt, wie die APIs im Windows.Media.Audio-Namespace zum Erstellen von Audiodiagrammen für Audiorouting sowie Misch- und Verarbeitungsszenarien verwendet werden.
title: Audiodiagramme
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 75066b566fde3f25ea4feb2ed82358b106ffcf7c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359120"
---
# <a name="audio-graphs"></a>Audiodiagramme



In diesem Artikel wird gezeigt, wie die APIs im [**Windows.Media.Audio**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio)-Namespace zum Erstellen von Audiodiagrammen für Audiorouting sowie Misch- und Verarbeitungsszenarien verwendet werden.

Ein *Audiodiagramm* ist ein Satz von miteinander verbundenen Audioknoten, über die Audiodaten fließen. 

- *Audioeingabeknoten* liefern dem Diagramm Audiodaten von Audioeingangsgeräten, Audiodateien oder aus benutzerdefiniertem Code. 

- *Audioausgabeknoten* sind das Ziel von Audiodaten, die vom Diagramm verarbeitet wurden. Audiodaten können außerhalb des Diagramms an Audioausgabegeräte, Audiodateien oder benutzerdefinierten Code weitergeleitet werden. 

- *Submixknoten* kombinieren Audiodaten von mindestens einem Knoten in einer einzigen Ausgabe, die an andere Knoten in dem Diagramm weitergeleitet werden kann. 

Nachdem alle Knoten erstellt und die Verbindungen zwischen ihnen eingerichtet wurden, starten Sie einfach das Audiodiagramm. Daraufhin fließen die Audiodaten von den Eingabeknoten über Submixknoten an die Ausgabeknoten. Dank dieses Modells können Szenarien wie die Aufzeichnung vom Mikrofon eines Geräts in einer Audiodatei, das Wiedergeben von Audiodaten aus einer Datei auf einem Gerätelautsprecher oder das Mischen von Audiodaten aus mehreren Quellen schnell und einfach implementiert werden.

Weitere Szenarien werden durch das Hinzufügen von Audioeffekten zum Audiodiagramm ermöglicht. Jeder Knoten in einem Audiodiagramm kann mit null oder mehr Audioeffekten gefüllt werden, die die Audioverarbeitung für die Audiodaten durchführen, die den Knoten durchlaufen. Es gibt verschiedene integrierte Effekte wie Echo, Equalizer, Begrenzungen und Halleffekt, die mit nur wenigen Codezeilen an einen Audioknoten angefügt werden können. Sie können auch eigene benutzerdefinierte Audioeffekte erstellen, die genau wie die integrierten Effekte funktionieren.

> [!NOTE]
> Im [UWP-Beispiel für AudioGraph](https://go.microsoft.com/fwlink/?LinkId=619481) wird der in dieser Übersicht erläuterte Code implementiert. Sie können das Beispiel herunterladen, um den Code im Kontext anzuzeigen oder ihn als Ausgangspunkt für Ihre eigene App zu verwenden.

## <a name="choosing-windows-runtime-audiograph-or-xaudio2"></a>Auswählen von Windows-Runtime-AudioGraph oder -XAudio2

Die Audiodiagramm-APIs der Windows-Runtime bieten Funktionen, die auch über die COM-basierten [XAudio2-APIs](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) implementiert werden können. Nachfolgend sind die Features des Audiodiagramm-Frameworks von Windows-Runtime aufgeführt, die von XAudio2 abweichen.

Die Audiodiagramm-APIs von Windows-Runtime

-   sind wesentlich benutzerfreundlicher als XAudio2.
-   können von C# verwendet werden und werden auch für C++ unterstützt.
-   können Audiodateien einschließlich komprimierter Dateiformate direkt verwenden. XAudio2 funktioniert nur auf Audiopuffern und stellt keine Datei-E/A-Funktionen bereit.
-   Die audio mit geringer Latenz-Pipeline können in Windows 10.
-   unterstützen eine automatische Endpunktumschaltung, wenn standardmäßige Endpunktparameter verwendet werden. Wenn der Benutzer beispielsweise vom Lautsprecher eines Geräts zu einem Headset wechselt, werden die Audiodaten automatisch an den neuen Eingang umgeleitet.

## <a name="audiograph-class"></a>AudioGraph-Klasse

Die [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph)-Klasse ist das übergeordnete Element aller Knoten, aus denen das Diagramm besteht. Verwenden Sie dieses Objekt, um Instanzen aller Audioknotentypen zu erstellen. Erstellen Sie eine Instanz der **AudioGraph**-Klasse, indem Sie ein [**AudioGraphSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraphSettings)-Objekt, das Konfigurationseinstellungen für das Diagramm enthält, initialisieren und dann [**AudioGraph.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.) aufrufen. Die zurückgegebene [**CreateAudioGraphResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.CreateAudioGraphResult)-Klasse ermöglicht den Zugriff auf das erstellte Audiodiagramm oder gibt einen Fehler zurück, wenn bei der Erstellung des Audiodiagramms ein Fehler auftritt.

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   Alle audio Knotentypen werden erstellt, mit der\* Methoden der **AudioGraph** Klasse.
-   Die [**AudioGraph.Start**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.start)-Methode bewirkt, dass das Audiodiagramm mit der Verarbeitung der Audiodaten beginnt. Die [**AudioGraph.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.stop)-Methode beendet die Audioverarbeitung. Jeder Knoten im Diagramm kann während der Ausführung des Diagramms unabhängig gestartet und beendet werden. Es sind aber keine Knoten aktiv, wenn das Diagramm beendet wird. [**ResetAllNodes** ](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.resetallnodes) bewirkt, dass alle Knoten im Diagramm, die momentan im audio-Puffer alle Daten verwerfen.
-   Das [**QuantumStarted**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.quantumstarted)-Ereignis tritt auf, wenn das Diagramm die Verarbeitung eines neuen Quantums von Audiodaten beginnt. Das [**QuantumProcessed**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.quantumprocessed)-Ereignis tritt auf, wenn die Verarbeitung eines Quantums abgeschlossen ist.

-   Als einzige [**AudioGraphSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraphSettings)-Eigenschaft ist [**AudioRenderCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Render.AudioRenderCategory) erforderlich. Durch Angabe dieses Werts kann das System die Audiopipeline für die angegebene Kategorie optimieren.
-   Die Quantumgröße des Audiodiagramms bestimmt die Anzahl der Samples, die gleichzeitig verarbeitet werden. Standardmäßig beträgt die Quantumgröße 10 ms basierend auf der Standard-Samplingrate. Wenn Sie eine benutzerdefinierte Quantumgröße durch Festlegen der [**DesiredSamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum)-Eigenschaft angeben, müssen Sie auch die [**QuantumSizeSelectionMode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode)-Eigenschaft auf **ClosestToDesired** festlegen, oder der angegebene Wert wird ignoriert. Wenn dieser Wert verwendet wird, wählt das System eine Quantumgröße aus, die möglich nah an der von Ihnen angegebenen Größe liegt. Um die tatsächliche Quantumgröße zu bestimmen, überprüfen Sie die [**SamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.samplesperquantum)-Eigenschaft der **AudioGraph**-Klasse, nachdem sie erstellt wurde.
-   Wenn Sie das Audiodiagramm nur mit Dateien verwenden möchten und keine Ausgabe an ein Audiogerät planen, wird empfohlen, dass Sie die Standard-Quantumgröße verwenden, indem Sie die [**DesiredSamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum)-Eigenschaft nicht festlegen.
-   Die [**DesiredRenderDeviceAudioProcessing**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing)-Eigenschaft bestimmt Verarbeitungsleistung, die das primäre Darstellungsgerät für die Ausgabe des Audiodiagramms durchführt. Über die **Default**-Einstellung kann das System die Standardaudioverarbeitung für die angegebene Audiowiedergabekategorie verwenden. Durch diese Verarbeitung kann der Sound der Audiodaten auf einigen Geräten wesentlich verbessert werden, insbesondere auf mobilen Geräten mit kleinen Lautsprechern. Durch die **Raw**-Einstellung kann die Leistung durch Minimieren der Signalverarbeitungsleistung verbessert werden. Dies kann jedoch zu einer schlechteren Tonqualität auf einigen Geräten führen.
-   Wenn die [**QuantumSizeSelectionMode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode)-Eigenschaft auf **LowestLatency** festgelegt wird, verwendet das Audiodiagramm für [**DesiredRenderDeviceAudioProcessing**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing) automatisch **Raw**.
- Ab Windows 10, Version 1803, können Sie die Eigenschaft [**AudioGraphSettings.MaxPlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.maxplaybackspeedfactor) auf einen Maximalwert für [**AudioFileInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor), [**AudioFrameInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.playbackspeedfactor) und [**MediaSourceInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceinputnode.playbackspeedfactor) festlegen. Wenn ein Audiodiagramm einen Wiedergabegeschwindigkeitsfaktor höher als 1 unterstützt, muss ihm das System zusätzlichen Speicher zuweisen, um einen ausreichenden Puffer der Audiodaten zu gewährleisten. Daher minimiert das Festlegen der Einstellung **MaxPlaybackSpeedFactor** auf den niedrigsten Wert, der von der App erforderlich ist, den Speicherbedarf Ihrer App. Wenn Ihre App nur Inhalte in normaler Geschwindigkeit wiedergibt, empfiehlt es sich, dass Sie MaxPlaybackSpeedFactor auf 1 festgelegt.
-   Die [**EncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.encodingproperties)-Eigenschaft bestimmt das vom Diagramm verwendete Audioformat. Es werden nur 32-Bit-Float-Formate unterstützt.
-   Die [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice)-Eigenschaft legt das primäre Darstellungsgerät für das Audiodiagramm fest. Wenn Sie diese Eigenschaft nicht festlegen, wird das Standardsystemgerät verwendet. Das primäre Darstellungsgerät wird zur Berechnung der Quantumgrößen für andere Knoten im Diagramm verwendet. Wenn in dem System keine Audiowiedergabegeräte vorhanden sind, tritt bei der Erstellung des Audiodiagramms ein Fehler auf.

Sie können für das Audiodiagramm das Standard-Audiowiedergabegerät festlegen oder die [**Windows.Devices.Enumeration.DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Klasse verwenden, um eine Liste der verfügbaren Audiowiedergabegeräte abzurufen, indem Sie [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) aufrufen und die von [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) zurückgegebene Auswahl des Audiowiedergabegeräts übergeben. Sie können eines der zurückgegebenen **DeviceInformation**-Objekte programmgesteuert auswählen oder die Benutzeroberfläche anzeigen, damit der Benutzer ein Gerät auswählen und dieses dann zum Festlegen der [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice)-Eigenschaft verwenden kann.

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  <a name="device-input-node"></a>Geräteingabeknoten

Eine Geräteingabeknoten liefert Audiodaten von einem an das System angeschlossenen Audioaufnahmegerät wie etwa einem Mikrofon an das Diagramm. Erstellen Sie ein [**DeviceInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceInputNode)-Objekt, das das standardmäßige Audioaufnahmegerät des Systems verwendet, indem Sie die [**CreateDeviceInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync)-Methode aufrufen. Geben Sie eine [**AudioRenderCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Render.AudioRenderCategory)-Enumeration an, damit das System die Audiopipeline für die angegebene Kategorie optimieren kann.

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

Wenn Sie eine bestimmte Audioaufnahmegerät für des Knotens "Eingabe" angeben möchten, können Sie mithilfe der [ **Windows.Devices.Enumeration.DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) Klasse um eine Liste der verfügbaren Audio des Systems zu erhalten. Erfassen von Geräten durch Aufrufen von [ **FindAllAsync** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) und übergeben Sie in der audio Render-Geräteauswahl zurückgegebenes [  **Windows.Media.Devices.MediaDevice.GetAudioCaptureSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiocaptureselector). Sie können eines der zurückgegebenen **DeviceInformation**-Objekte programmgesteuert auswählen oder Benutzeroberfläche anzeigen, damit der Benutzer ein Gerät auswählen und dieses dann an die [**CreateDeviceInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync)-Methode übergeben kann.

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  <a name="device-output-node"></a>Gerätausgabeknoten

Ein Gerätausgabeknoten überträgt Audiodaten von dem Diagramm an ein Audiowiedergabegerät, z. B. Lautsprecher oder ein Headset. Erstellen Sie eine [**DeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode)-Klasse, indem Sie die [**CreateDeviceOutputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceoutputnodeasync)-Methode aufrufen. Der Ausgabeknoten verwendet die [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice)-Eigenschaft des Audiodiagramms.

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  <a name="file-input-node"></a>Dateieingabeknoten

Mit einem Dateieingabeknoten können Sie Daten aus einer Audiodatei in das Diagramm übertragen. Erstellen Sie eine [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode)-Klasse, indem Sie die [**CreateFileInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync)-Methode aufrufen.

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   Dateieingabeknoten unterstützen die Dateiformate MP3, WAV, WMA und M4A.
-   Legen Sie die [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.starttime)-Eigenschaft so fest, dass in der Datei der Zeitoffset angegeben wird, an dem die Wiedergabe beginnen soll. Wenn diese Eigenschaft null ist, wird der Anfang der Datei verwendet. Legen Sie die [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.endtime)-Eigenschaft so fest, dass in der Datei der Zeitoffset angegeben wird, an dem die Wiedergabe enden soll. Wenn diese Eigenschaft null ist, wird das Ende der Datei verwendet. Die Startzeit muss vor der Endzeit liegen, und der Wert für die Endzeit muss kleiner oder gleich der Dauer der Audiodatei sein. Sie können die Richtigkeit anhand des [**Duration**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.duration)-Eigenschaftswerts prüfen.
-   Suchen Sie eine Position in der Audiodatei, indem Sie die [**Seek**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.seek)-Methode aufrufen und den Zeitoffset in der Datei angeben, an den die Wiedergabeposition verschoben werden soll. Der angegebene Wert muss zwischen den Eigenschaften [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.starttime) und [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.endtime) liegen. Die aktuelle Wiedergabeposition des Knotens können Sie mit der schreibgeschützten [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.position)-Eigenschaft abrufen.
-   Aktivieren Sie Schleifen für die Audiodatei, indem Sie die [**LoopCount**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.loopcount)-Eigenschaft festlegen. Wenn diese nicht Null ist, gibt dieser Wert die Anzahl der Wiederholungen der Datei nach der ersten Wiedergabe an. Wenn Sie **LoopCount** beispielsweise auf 1 festlegen, wird die Datei insgesamt zweimal wiedergegeben. Wenn Sie den Wert auf 5 festlegen, wird die Datei insgesamt sechs Mal wiedergegeben. Indem Sie **LoopCount** auf Null setzen, wird die Datei in einer Schleife unbegrenzt wiedergegeben. Um die Schleife zu beenden, setzen Sie den Wert auf 0 fest.
-   Legen Sie zum Anpassen der Geschwindigkeit, mit der die Audiodatei wiedergegeben wird, die [**PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor)-Eigenschaft fest. Der Wert 1 zeigt die ursprüngliche Geschwindigkeit der Datei an. Der Wert 0,5 legt die halbe Geschwindigkeit und der Wert 2 ist die doppelte Geschwindigkeit fest.

##  <a name="mediasource-input-node"></a>MediaSource-Eingabeknoten

Die [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource)-Klasse, die allgemein zum Verweisen auf Medien aus verschiedenen Quellen sowie zum Wiedergeben dieser Medien verwendet wird bietet ein gemeinsames Modell für den Mediendatenzugriff – unabhängig vom zugrunde liegenden Medienformat, was eine Datei auf einem Datenträger, einem Stream oder einer streamenden Netzwerkquelle sein kann. Mit dem [**MediaSourceAudioInputNode](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode)-Knoten können Sie die **MediaSource**-Daten in ein Audiodiagramm umleiten. Erstellen Sie **MediaSourceAudioInputNode** durch Aufrufen von [**CreateMediaSourceAudioInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createmediasourceaudioinputnodeasync#Windows_Media_Audio_AudioGraph_CreateMediaSourceAudioInputNodeAsync_Windows_Media_Core_MediaSource_), und übergeben Sie ein **MediaSource**-Objekt, das die Inhalte, die Sie abspielen möchten, darstellt. Ein [** CreateMediaSourceAudioInputNodeResult](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult) wird zurückgegeben, das Sie verwenden können, um den Status des Vorgangs durch Überprüfen der Eigenschaft [**Status**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.status) zu verwenden. Wenn der Status **Erfolg** ist, erhalten Sie das erstellte **MediaSourceAudioInputNode** durch den Zugriff auf die [**Knoten**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.node)-Eigenschaft. Das folgende Beispiel zeigt die Erstellung eines Knotens aus einem AdaptiveMediaSource-Objekt, das das Streaming von Inhalten über das Netzwerk darstellt. Weitere Informationen zur Verwendung der **MediaSource** finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md). Weitere Informationen zum Streamen von Media-Inhalten über das Internet finden Sie unter [adaptives Streaming](adaptive-streaming.md).

[!code-cs[DeclareMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareMediaSourceInputNode)]

[!code-cs[CreateMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateMediaSourceInputNode)]

Um eine Benachrichtigung zu erhalten, wenn die Wiedergabe das Ende des **MediaSource**-Inhalts erreicht hat, registrieren Sie einen Handler für das [**MediaSourceCompleted**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode.mediasourcecompleted)-Ereignis. 

[!code-cs[RegisterMediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterMediaSourceCompleted)]

[!code-cs[MediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetMediaSourceCompleted)]

Während eine Datei aus Diskis wahrscheinlich immer erfolgreich abgespielt werden kann, kann das Streamen von Medien von einer Netzwerkquelle während der Wiedergabe aufgrund einer Änderung der Netzwerkverbindung oder anderer Probleme fehlschlagen, die außerhalb der Kontrolle des Audiodiagramms liegen. Wenn eine **MediaSource** während der Wiedergabe fehlschlägt, löst das Audiodiagramm ein [**UnrecoverableErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.unrecoverableerroroccurred)-Ereignis aus. Sie können für dieses Ereignis den Handler verwenden, um das Audiodiagramm zu beenden und zu löschen und und das Diagramm erneut zu initialisieren. 

[!code-cs[RegisterUnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterUnrecoverableError)]

[!code-cs[UnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUnrecoverableError)]

##  <a name="file-output-node"></a>Dateiausgabeknoten

Mit einem Dateiausgabeknoten können Sie die Audiodaten aus dem Diagramm in eine Audiodatei umleiten. Erstellen Sie eine [**AudioFileOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileOutputNode)-Klasse, indem Sie die [**CreateFileOutputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileoutputnodeasync)-Methode aufrufen.

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   Dateiausgabeknoten unterstützen die Dateiformate MP3, WAV, WMA und M4A.
-   Um die Verarbeitung des Knotens zu beenden, rufen Sie erst die [**AudioFileOutputNode.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileoutputnode.stop)-Methode und dann die [**AudioFileOutputNode.FinalizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileoutputnode.finalizeasync)-Methode auf. Andernfalls wird eine Ausnahme ausgelöst.

##  <a name="audio-frame-input-node"></a>Audioframe-Eingabeknoten

Mit einem Audioframe-Eingabeknoten können Sie Audiodaten, die Sie in Ihrem eigenen Code generieren, in das Audiodiagramm übertragen. Dies ermöglicht Szenarien wie das Erstellen eines benutzerdefinierten Softwaresynthesizers. Erstellen Sie eine [**AudioFrameInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFrameInputNode)-Klasse, indem Sie die [**CreateFrameInputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createframeinputnode)-Methode aufrufen.

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

Das [**FrameInputNode.QuantumStarted**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.quantumstarted)-Ereignis wird ausgelöst, wenn das Audiodiagramm bereit ist, die Verarbeitung des nächsten Quantums von Audiodaten zu verarbeiten. Sie stellen die benutzerdefinierten generierten Audiodaten vom Handler für dieses Ereignis bereit.

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   Durch das an den **QuantumStarted**-Ereignishandler übergebene [**FrameInputNodeQuantumStartedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.FrameInputNodeQuantumStartedEventArgs)-Objekt wird die [**RequiredSamples**](https://docs.microsoft.com/uwp/api/windows.media.audio.frameinputnodequantumstartedeventargs.requiredsamples)-Eigenschaft verfügbar, die angibt, wie viele Sample das Audiodiagramm füllen muss, damit das Quantum verarbeitet wird.
-   Rufen Sie die [**AudioFrameInputNode.AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe)-Methode auf, um ein mit Audiodaten gefülltes [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame)-Objekt an das Diagramm zu übergeben.
- Es wurde ein neuer Satz von APIs für die Verwendung von **MediaFrameReader** mit Audiodaten in Windows 10, Version 1803 eingeführt. Diese APIs ermöglichen es Ihnen, **AudioFrame**-Objekte von einer Medieframenquelle zu erhalten, die an ein **FrameInputNode** mithilfe der **AddFrame** Methode übergeben werden kann. Weitere Informationen finden Sie unter [Verarbeiten von Audioframes mit MediaFrameReader](process-audio-frames-with-mediaframereader.md).
-   Nachfolgend sehen Sie ein Beispiel einer Implementierung der **GenerateAudioData**-Hilfsmethode.

Zum Auffüllen eines [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame)-Objekts mit Audiodaten benötigen Sie Zugriff auf den zugrunde liegenden Speicherpuffer des Audioframes. Initialisieren Sie zu diesem Zweck die **IMemoryBufferByteAccess**-COM-Schnittstelle, indem Sie dem Namespace den folgenden Code hinzufügen.

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

Der folgende Code zeigt ein Beispiel einer Implementierung der **GenerateAudioData**-Hilfsmethode, die ein [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame)-Objekt erstellt und dieses mit Audiodaten füllt.

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   Da diese Methode auf den Rohdatenpuffer zugreift, der Windows-Runtime-Typen zugrunde liegt, muss diese mithilfe des Schlüsselworts **unsafe** deklariert werden. Außerdem müssen Sie das Projekt in Microsoft Visual Studio so konfigurieren, dass die Kompilierung von unsicherem Code zugelassen wird, indem Sie die Seite **Eigenschaften** des Projekts öffnen, auf die Eigenschaftenseite **Build** klicken und das Kontrollkästchen **Unsicheren Code zulassen** aktivieren.
-   Initialisieren Sie im **Windows.Media**-Namespace eine neue Instanz des [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame)-Objekts, indem Sie die gewünschte Puffergröße an den Konstruktor übergeben. Die Puffergröße ist die Sample-Anzahl multipliziert mit der Größe der einzelnen Sample.
-   Rufen Sie das [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer)-Objekt des Audioframes ab, indem Sie die [**LockBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audioframe.lockbuffer)-Methode aufrufen.
-   Rufen Sie eine Instanz der [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions//mt297505(v=vs.85))-COM-Schnittstelle aus dem Audiopuffer ab, indem Sie [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer.createreference) aufrufen.
-   Rufen Sie einen Zeiger auf die Rohdaten des Audiopuffers ab, indem Sie die [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)-Methode aufrufen und in den Beispieldatentyp der Audiodaten umwandeln.
-   Füllen Sie den Puffer mit Daten, und geben Sie das [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame)-Objekt für die Übermittlung an das Audiodiagramm zurück.

##  <a name="audio-frame-output-node"></a>Audioframe-Ausgabeknoten

Mit einem Audioframe-Ausgabeknoten können Sie eine Audiodatenausgabe aus dem Audiodiagramm mit benutzerdefiniertem Code empfangen und verarbeiten. Ein Beispielszenario hierfür ist das Durchführen einer Signalanalyse für die Audioausgabe. Erstellen Sie ein [**AudioFrameOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFrameOutputNode)-Objekt, indem Sie die [**CreateFrameOutputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createframeoutputnode)-Methode aufrufen.

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

Das [**AudioGraph.QuantumStarted**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph.QuantumStarted)-Ereignis wird ausgelöst, wenn das Audiodiagramm mit der Verarbeitung eines Quantums von Audiodaten beginnt. Sie können innerhalb des Handlers für dieses Ereignis auf die Audiodaten zugreifen. 

> [!NOTE]
> Wenn Sie Audioframes regelmäßig (mit dem Audiodiagramm synchronisiert) abrufen möchten, rufen Sie [AudioFrameOutputNode.GetFrame](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.GetFrame) von innerhalb des synchronen **QuantumStarted**-Ereignishandlers auf. Das **QuantumProcessed**-Ereignis wird nach der Audioverarbeitung durch das Audiomodul asynchron ausgelöst, was bedeutet, dass der Rhythmus möglicherweise unregelmäßig ist. Aus diesem Grund sollten Sie das **QuantumProcessed**-Ereignis nicht für die synchronisierte Verarbeitung von Audioframedaten verwenden.

[!code-cs[SnippetQuantumStartedFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStartedFrameOutput)]

-   Rufen Sie die [**GetFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.getframe)-Methode auf, um ein mit Audiodaten gefülltes [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame)-Objekt mit Audiodaten aus dem Diagramm abzurufen.
-   Nachfolgend sehen Sie ein Beispiel einer Implementierung der **ProcessFrameOutput**-Hilfsmethode.

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   Wie in dem Beispiel oben für den Audioframe-Eingabeknoten müssen Sie die **IMemoryBufferByteAccess**-COM-Schnittstelle deklarieren und das Projekt so konfigurieren, dass unsicherer Code zugelassen wird, damit Sie auf den zugrunde liegenden Audiopuffer zugreifen können.
-   Rufen Sie das [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer)-Objekt des Audioframes ab, indem Sie die [**LockBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audioframe.lockbuffer)-Methode aufrufen.
-   Rufen Sie eine Instanz der **IMemoryBufferByteAccess**-COM-Schnittstelle aus dem Audiopuffer ab, indem Sie [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer.createreference) aufrufen.
-   Rufen Sie einen Zeiger auf die Rohdaten des Audiopuffers ab, indem Sie die **IMemoryBufferByteAccess.GetBuffer**-Methode aufrufen und in den Beispieldatentyp der Audiodaten umwandeln.

## <a name="node-connections-and-submix-nodes"></a>Knotenverbindungen und Submixknoten

Alle Eingabeknotentypen machen die **AddOutgoingConnection**-Methode verfügbar, die die vom Knoten produzierten Audiodaten an den Knoten weiterleitet, der an die Methode übergeben wird. Im folgenden Beispiel wird eine Verbindung zwischen [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) und [**AudioDeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) hergestellt. Dabei handelt es sich um ein einfaches Setup für die Wiedergabe einer Audiodatei über den Lautsprecher des Geräts.

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

Von einem Eingabeknoten können mehrere Verbindungen zu anderen Knoten erstellt werden. Im folgenden Beispiel wird eine Verbindung zwischen [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) und [**AudioFileOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileOutputNode) hinzugefügt. Die Audiodaten aus der Audiodatei werden nun über den Lautsprecher des Geräts wiedergegeben und auch in eine Audiodatei geschrieben.

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

Ausgabeknoten können auch mehrere Verbindungen von anderen Knoten empfangen. Im folgenden Beispiel wird eine Verbindung zwischen den Knoten [**AudioDeviceInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) und [**AudioDeviceOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) hergestellt. Da der Ausgabeknoten Verbindungen vom Dateieingabeknoten und dem Geräteingabeknoten aufweist, enthält die Ausgabe eine Audiomischung aus beiden Quellen. **AddOutgoingConnection** bietet eine Überladung, mit der Sie einen Verstärkungswert für das über die Verbindung laufende Signal angeben können.

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

Obwohl Ausgabeknoten Verbindungen von mehreren Knoten annehmen können, sollten Sie eine Zwischenmischung von Signalen von einem oder mehreren Knoten erstellen, bevor Sie die Mischung an eine Ausgabe übergeben. Angenommen, Sie möchten die Ebene festlegen oder Effekte auf eine Untergruppe der Audiosignale in einem Diagramm anwenden. Verwenden Sie hierfür die [**AudioSubmixNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioSubmixNode)-Klasse. Sie können keine Verbindung zu einem Submixknoten von einer oder mehreren Eingabeknoten oder anderen Submixknoten herstellen. Im folgenden Beispiel wird ein neuer Submixknoten zu [**AudioGraph.CreateSubmixNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createsubmixnode) erstellt. Anschließend werden Verbindungen von einem Dateiausgabeknoten und einem Frameausgabeknoten zum Submixknoten hinzugefügt. Schließlich wird der Submixknoten mit einem Dateiausgabeknoten verbunden.

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## <a name="starting-and-stopping-audio-graph-nodes"></a>Starten und Beenden von Audiodiagrammknoten

Wenn die [**AudioGraph.Start**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.start)-Methode aufgerufen wird, beginnt das Audiodiagramm mit der Verarbeitung der Audiodaten. Jeder Knoten umfasst eine **Start**- und **Stop**-Methode, die bewirken, dass der jeweilige Knoten die Verarbeitung von Daten beginnt oder beendet. Bei Aufruf der [**AudioGraph.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.stop)-Methode wird die gesamte Verarbeitung in allen Knoten unabhängig vom Status der einzelnen Knoten beendet. Der Status der einzelnen Knoten kann jedoch festgelegt werden, während das Audiodiagramm beendet wird. Sie können beispielsweise die **Stop**-Methode in einem einzelnen Knoten aufrufen, während das Diagramm beendet wird. Rufen Sie anschließend die **AudioGraph.Start**-Methode auf, damit der einzelne Knoten angehalten bleibt.

Mit allen Knotentypen wird die **ConsumeInput**-Eigenschaft verfügbar. Diese lest bei Festlegung auf „false“ zu, dass der Knoten die Audioverarbeitung fortsetzt. Gleichzeitig verhinder sie, dass Audiodaten von anderen Knoten verbraucht werden.

Mit allen Knotentypen wird die **Reset** -Methode verfügbar. Sie bewirkt, dass der Knoten alle Audiodaten löscht, die sich aktuell in seinem Puffer befinden.

## <a name="adding-audio-effects"></a>Hinzufügen von Audioeffekten

Mit der Audiodiagramm-API können Sie Audioeffekte zu jedem Knotentyp in einem Diagramm hinzufügen. Ausgabeknoten, Eingabeknoten und Submixknoten können jeweils eine unbegrenzte Anzahl von Audioeffekten aufweisen. Eine Einschränkung erfolgt lediglich durch die Funktionen der Hardware. Im folgenden Beispiel ist das Hinzufügen des integrierten Echoeffekts zu einem Submixknoten dargestellt.

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   Alle Audioeffekte implementieren die [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition)-Schnittstelle. Mit jedem Knoten wird eine **EffectDefinitions**-Eigenschaft verfügbar, welche die Liste der auf diesen Knoten angewendeten Effekte darstellt. Fügen Sie einen Effekt hinzu, indem Sie sein Definitionsobjekt zu der Liste hinzufügen.
-   Es gibt mehrere Effektdefinitionsklassen, die im **Windows.Media.Audio**-Namespace bereitgestellt werden. Dazu gehören:
    -   [**EchoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.EchoEffectDefinition)
    -   [**EqualizerEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.EqualizerEffectDefinition)
    -   [**LimiterEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.LimiterEffectDefinition)
    -   [**ReverbEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.ReverbEffectDefinition)
-   Sie können Ihre eigenen Audioeffekte zum Implementieren von [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) erstellen. Wenden Sie diese anschließend auf einen beliebigen Knoten in einem Audiodiagramm an.
-   Mit jedem Knotentyp wird eine **DisableEffectsByDefinition**-Methode verfügbar, die alle Effekte in der **EffectDefinitions**-Liste des Knotens deaktiviert, die mithilfe der angegebenen Definition hinzugefügt wurden. **EnableEffectsByDefinition** aktiviert die Effekte mit der angegebenen Definition.

## <a name="spatial-audio"></a>Räumliche Audiowiedergabe
Ab Windows 10, Version 1607, unterstützt **AudioGraph** die räumliche Audiowiedergabe. Dabei können Sie eine Position im dreidimensionalen Raum angeben, an der Audiodaten von einem Eingabe- oder Submixknoten ausgegeben werden. Sie können auch eine Form und Richtung für die Audioausgabe angeben, eine Geschwindigkeit festlegen, die für die Dopplerverschiebung der Audiodaten des Knotens verwendet wird, und ein Abklingmodell definieren, das beschreibt, wie Klang mit zunehmender Entfernung gedämpft wird. 

Um einen Emitter zu erstellen, können Sie zunächst eine Form definieren, in der der Sound vom Emitter projiziert wird. Die Klangausbreitung kann kegel- oder kugelförmig sein. Die [**AudioNodeEmitterShape**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitterShape)-Klasse bietet statische Methoden zum Erstellen dieser Formen. Als Nächstes erstellen Sie ein Abklingmodell. Es definiert, wie die Lautstärke des vom Emitter ausgegebenen Sounds mit zunehmender Entfernung vom Listener (Zuhörer) abnimmt. Mit der [**CreateNatural**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitterdecaymodel.createnatural)-Methode wird ein Abklingmodell erstellt. Es emuliert das natürliche Abklingen von Sound anhand eines auf einem Abstandsquadrat basierten Abnahmemodells. Erstellen Sie zuletzt ein [**AudioNodeEmitterSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitterSettings)-Objekt. Dieses Objekt wird derzeit nur zum Aktivieren und Deaktivieren der geschwindigkeitsbasierten Dopplerdämpfung der vom Emitter ausgegebenen Audiodaten verwendet. Rufen Sie den [**AudioNodeEmitter**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.)-Konstruktor auf, und übergeben Sie die gerade erstellten Initialisierungsobjekte. Der Emitter wird standardmäßig am Ursprung positioniert, Sie können seine Position aber auch mit der [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.position)-Eigenschaft festlegen.

> [!NOTE]
> Audioknotenemitter können nur Monoaudiodaten mit einer Abtastrate von 48 kHz verarbeiten. Die Verwendung von Stereoaudiodaten oder Audio mit einer anderen Abtastrate führt zu einer Ausnahme.

Sie weisen den Emitter beim Erstellen einem Audioknoten zu, indem Sie die überladene Erstellungsmethode für den gewünschten Knotentyp verwenden. In diesem Beispiel wird [**CreateFileInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync) verwendet, um einen Dateieingabeknoten aus einer angegebenen Datei und das [**AudioNodeEmitter**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitter)-Objekt zu erstellen, das Sie dem Knoten zuordnen möchten.

[!code-cs[CreateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateEmitter)]

Die [**AudioDeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode)-Klasse, die Audiodaten aus dem Diagramm für den Benutzer ausgibt, verfügt über ein Listener-Objekt, auf das mit der [**Listener**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiodeviceoutputnode.listener)-Eigenschaft zugegriffen wird. Sie gibt die Position, Ausrichtung und Geschwindigkeit des Benutzers im dreidimensionalen Raum an. Die Positionen aller Emitter im Diagramm verhalten sich relativ zur Position und Ausrichtung des Emitterobjekts. Der Listener befindet sich standardmäßig am Ursprung (0,0,0) und ist nach vorne entlang der Z-Achse ausgerichtet. Sie können die Position und Ausrichtung jedoch mit der [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodelistener.position)-Eigenschaft und [**Orientation**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodelistener.orientation)-Eigenschaft festlegen.

[!code-cs[Listener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetListener)]

Sie können die Position, Geschwindigkeit und Richtung von Emittern zur Laufzeit aktualisieren, um die Bewegung einer Audioquelle durch den dreidimensionalen Raum zu simulieren.

[!code-cs[UpdateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateEmitter)]

Sie können auch die Position, Geschwindigkeit und Ausrichtung des Listener-Objekts zur Laufzeit aktualisieren, um die Bewegung des Benutzers durch den dreidimensionalen Raum zu simulieren.

[!code-cs[UpdateListener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateListener)]

Die räumliche Audiowiedergabe wird standardmäßig mit dem HRTF-Algorithmus (Head-relative Transfer Function, kopfbezogene Übertragungsfunktion) von Microsoft berechnet, um die Audiowiedergabe basierend auf der Form, Geschwindigkeit und Position relativ zum Listener zu dämpfen. Sie können die [**SpatialAudioModel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.spatialaudiomodel)-Eigenschaft auf **FoldDown** festlegen, um eine einfache Stereomischmethode zum Simulieren der räumlichen Audiowiedergabe zu verwenden. Diese ist zwar weniger genau, erfordert dafür aber weniger CPU-Leistung und Arbeitsspeicher.

## <a name="see-also"></a>Siehe auch
- [Medienwiedergabe](media-playback.md)
 

 




