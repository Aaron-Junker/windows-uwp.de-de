---
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: In diesem Artikel erfahren Sie, wie Sie mit mediaframereader mit mediacapture Audioframes mit Audiodaten aus einer aufzeichnungsquelle erhalten.
title: Verarbeiten von Audioframes mit „MediaFrameReader“
ms.date: 04/18/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1d1d4f7e24caf50db41851e237a832301df75cd0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163654"
---
# <a name="process-audio-frames-with-mediaframereader"></a>Verarbeiten von Audioframes mit „MediaFrameReader“

In diesem Artikel erfahren Sie, wie Sie mithilfe von [**mediaframereader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) mit [**mediacapture**](/uwp/api/Windows.Media.Capture.MediaCapture) Audiodaten aus einer Medien Frame Quelle erhalten. Informationen zur Verwendung von **mediaframereader** zum Aufrufen von Bilddaten, z. b. von einer Farb-, Infrarot-oder tiefenkamera, finden Sie unter [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md). Dieser Artikel bietet eine allgemeine Übersicht über das Frame Reader-Verwendungs Muster und erläutert einige zusätzliche Features der **mediaframereader** -Klasse, z. b. die Verwendung von **mediaframesourcegroup** , um Frames aus mehreren Quellen gleichzeitig abzurufen. 

> [!NOTE] 
> Die in diesem Artikel erläuterten Features sind nur ab Windows 10, Version 1803, verfügbar.

> [!NOTE] 
> Es gibt ein Beispiel für universelle Windows-Apps, in dem die Verwendung von **MediaFrameReader** zum Anzeigen von Frames aus unterschiedlichen Framequellen demonstriert wird, unter anderem Farb-, Tiefen- und Infrarotkameras. Weitere Informationen finden Sie unter [Beispiel für Kameraframes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames).

## <a name="setting-up-your-project"></a>Einrichten Ihres Projekts
Der Vorgang zum Abrufen von Audioframes ist größtenteils identisch mit dem Erwerb von anderen Medien Frame Typen. Wie bei allen Apps, die **MediaCapture** verwenden, müssen Sie deklarieren, dass Ihre App die *Webcam*-Funktion verwendet. Erst dann können Sie auf Kamerageräte zugreifen. Wenn Ihre App von einem Audiogerät aufzeichnet, müssen Sie auch die *microphone*-Gerätefunktion deklarieren. 

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie das Kontrollkästchen für die **Webcam** und das Kontrollkästchen für **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.



## <a name="select-frame-sources-and-frame-source-groups"></a>Auswählen von Framequellen und Framequellgruppen

Der erste Schritt bei der Erfassung von Audioframes besteht darin, eine [**mediaframesource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) zu initialisieren, die die Quelle der Audiodaten darstellt, z. b. ein Mikrofon oder ein anderes audioerfassungs Gerät. Zu diesem Zweck müssen Sie eine neue Instanz des [**mediacapture**](/uwp/api/Windows.Media.Capture.MediaCapture) -Objekts erstellen. In diesem Beispiel ist die einzige Initialisierungs Einstellung für das **mediacapture** das Festlegen von [**streamingcapturemode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) , um anzugeben, dass Audiodaten aus dem Erfassungsgerät gestreamt werden sollen. 

Nachdem Sie [**MediaCapture.Initializeasync**](/uwp/api/windows.media.capture.mediacapture.initializeasync)aufgerufen haben, können Sie die Liste der zugänglichen Medien Frame Quellen mit der [**framesources**](/uwp/api/windows.media.capture.mediacapture.framesources) -Eigenschaft abrufen. In diesem Beispiel wird eine LINQ-Abfrage verwendet, um alle Frame Quellen auszuwählen, bei denen das [**mediaframesourceinfo**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo) -Element, das die Frame Quelle beschreibt, einen  [**MediaStreamType**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) von **Audiodaten**aufweist, was angibt, dass die Medienquelle Audiodaten erzeugt

Wenn die Abfrage mindestens eine Frame Quelle zurückgibt, können Sie die [**currentFormat**](/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) -Eigenschaft überprüfen, um festzustellen, ob die Quelle das gewünschte Audioformat unterstützt (in diesem Beispiel: float-Audiodaten). Überprüfen Sie die [**audioencodingproperties**](/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) , um sicherzustellen, dass die gewünschte Audiocodierung von der Quelle unterstützt wird.

[!code-cs[InitAudioFrameSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitAudioFrameSource)]

## <a name="create-and-start-the-mediaframereader"></a>Erstellen und starten von "mediaframereader"

Rufen Sie eine neue Instanz von **mediaframereader** durch Aufrufen von [**mediacapture. createframereaderasync**](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_)ab, und übergeben Sie das im vorherigen Schritt ausgewählte **mediaframesource** -Objekt. Standardmäßig werden Audioframes im gepufferten Modus abgerufen, sodass es weniger wahrscheinlich ist, dass Frames gelöscht werden. Dies kann jedoch weiterhin vorkommen, wenn Sie keine Audioframes schnell genug verarbeiten und den belegten Speicherpuffer des Systems auffüllen.

Registrieren Sie einen Handler für das [**mediaframereader. framekam**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) -Ereignis, das vom System ausgelöst wird, wenn ein neuer Rahmen der Audiodaten verfügbar ist. Aufrufen von [**startasync**](/uwp/api/windows.media.capture.frames.mediaframereader.startasync) , um mit dem Erwerb von Audioframes zu beginnen. Wenn der Frame Reader nicht gestartet werden kann, hat der vom-Befehl zurückgegebene Statuswert einen anderen Wert als [**Success**](/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus).

[!code-cs[CreateAudioFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateAudioFrameReader)]

Rufen Sie im **framekam** -Ereignishandler [**tryacquirelatestframe**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) für das **mediaframereader** -Objekt auf, das als Absender an den-Handler weitergegeben wurde, um einen Verweis auf den letzten Medien Frame abzurufen. Beachten Sie, dass dieses Objekt NULL sein kann, daher sollten Sie immer überprüfen, bevor Sie das-Objekt verwenden. Der Typ des Medien Frames, der in der von **tryacquirelatestframe** zurückgegebenen **mediaframereferenzierung** umschließt ist, hängt davon ab, welche Art von Frame-Quelle oder welche Quellen Sie für den Frame Reader konfiguriert haben. Da der Frame Reader in diesem Beispiel zum Abrufen von Audioframes eingerichtet wurde, ruft er den zugrunde liegenden Frame mithilfe der [**audiomediaframe**](/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe) -Eigenschaft ab. 

Diese **processaudioframe** -Hilfsmethode im folgenden Beispiel zeigt, wie Sie einen [**audioframe**](/uwp/api/windows.media.audioframe) erhalten, der Informationen wie z. b. den Zeitstempel des Frames und die diskontinuierliche Verwendung des **audiomediaframe** -Objekts bereitstellt. Um die audiobeispieldaten zu lesen oder zu verarbeiten, müssen Sie das [**audiobuffer**](/uwp/api/windows.media.audiobuffer) -Objekt aus dem **audiomediaframe** -Objekt abrufen, einen [**imemorybufferreference**](/uwp/api/windows.foundation.imemorybufferreference)erstellen und dann die com-Methode **imemorybufferbyteaccess:: GetBuffer** aufrufen, um die Daten abzurufen. Weitere Informationen zum Zugreifen auf systemeigene Puffer finden Sie im Hinweis unter dem Codelisting.

Das Format der Daten hängt von der Frame Quelle ab. In diesem Beispiel haben wir bei der Auswahl einer Medien Frame Quelle explizit sichergestellt, dass die ausgewählte Frame Quelle einen einzelnen Kanal von float-Daten verwendet hat. Der Rest des Beispielcodes zeigt, wie die Dauer und die Anzahl der Stichproben für die Audiodaten im Frame bestimmt werden.  

[!code-cs[ProcessAudioFrame](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetProcessAudioFrame)]

> [!NOTE] 
> Um mit den Audiodaten arbeiten zu können, müssen Sie auf einen systemeigenen Speicherpuffer zugreifen. Zu diesem Zweck müssen Sie die **imemorybufferbyteaccess** -com-Schnittstelle verwenden, indem Sie das unten stehende Codelisting einschließen. Vorgänge für den systemeigenen Puffer müssen in einer Methode ausgeführt werden, die das **unsichere** Schlüsselwort verwendet. Sie müssen auch das Kontrollkästchen aktivieren, um unsicheren Code auf der Registerkarte **Build** des Dialog Felds **Eigenschaften für Projekt >** zuzulassen.

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>Weitere Informationen zur Verwendung von "mediaframereader" mit Audiodaten

Sie können den [**audioabvicecontroller**](/uwp/api/Windows.Media.Devices.AudioDeviceController) abrufen, der der audioframe-Quelle zugeordnet ist, indem Sie auf die [**mediaframesource. Controller**](/uwp/api/windows.media.capture.frames.mediaframesource.controller) -Eigenschaft zugreifen. Dieses Objekt kann verwendet werden, um die streameigenschaften des Aufzeichnungs Geräts oder die Erfassungs Ebene zu steuern. Im folgenden Beispiel wird ein Hashwert für das Audiogerät angezeigt, sodass Frames weiterhin vom Frame Reader abgerufen werden, aber alle-Beispiele haben den Wert 0.

[!code-cs[AudioDeviceControllerMute](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetAudioDeviceControllerMute)]

Sie können ein [**audioframe**](/uwp/api/windows.media.audioframe) -Objekt verwenden, um von einer Medien Frame Quelle erfasste Audiodaten in einem [**audiograph**](/uwp/api/windows.media.audio.audiograph)zu übergeben. Übergeben Sie den Frame an die [**addframe**](/uwp/api/windows.media.audio.audioframeinputnode.addframe) -Methode eines [**audioframeinputnode**](/uwp/api/windows.media.audio.audioframeinputnode). Weitere Informationen zur Verwendung von audiodiagrammen, um Audiosignale zu erfassen, zu verarbeiten und zu mischen, finden Sie unter [audiodiagramme](audio-graphs.md).

## <a name="related-topics"></a>Zugehörige Themen

* [Verarbeiten von Medienframes mit „MediaFrameReader“](process-media-frames-with-mediaframereader.md)
* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Beispiel für Kameraframes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Audiodiagramme](audio-graphs.md)
 