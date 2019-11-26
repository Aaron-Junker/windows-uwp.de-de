---
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: Dieser Artikel veranschaulicht das Verwenden von MediaFrameReader mit "MediaCapture", um AudioFrames mit Audiodaten aus einer Aufnahmequelle abzurufen.
title: Verarbeiten von Audioframes mit „MediaFrameReader“
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f8d357dfbceafb6cc366b2880956ab3db231047d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74256663"
---
# <a name="process-audio-frames-with-mediaframereader"></a>Verarbeiten von Audioframes mit „MediaFrameReader“

Dieser Artikel veranschaulicht das Verwenden von [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) mit [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) um AudioFrames mit Audiodaten aus einer Aufnahmequelle abzurufen. Weitere Informationen zur Verwendung einer **MediaFrameReader**-Bilddaten wie z. B. Farbe, Infrarot oder Tiefenkamera finden Sie unter [Verarbeiten von Medienframes mit "MediaFrameReader"](process-media-frames-with-mediaframereader.md). Dieser Artikel enthält eine allgemeine Übersicht über das Frame-Reader-Verwendungsmuster und beschreibt einige zusätzlichen Funktionen der **MediaFrameReader**-Klasse, z. B. die Verwendung von **MediaFrameSourceGroup**, um Frames aus mehreren Quellen zur gleichen Zeit abzurufen. 

> [!NOTE] 
> Die in diesem Artikel besprochenen Features sind erst ab Windows 10, Version 1803, verfügbar.

> [!NOTE] 
> Es gibt ein Beispiel für universelle Windows-Apps, in dem die Verwendung von **MediaFrameReader** zum Anzeigen von Frames aus unterschiedlichen Framequellen demonstriert wird, unter anderem Farb-, Tiefen- und Infrarotkameras. Weitere Informationen finden Sie unter [Beispiel für Kameraframes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames).

## <a name="setting-up-your-project"></a>Einrichten Ihres Projekts
Der Prozess zum Erwerb von Audioframes entspricht größtenteils dem Erwerb anderer Arten von Medienframes. Wie bei allen Apps, die **MediaCapture** verwenden, müssen Sie deklarieren, dass Ihre App die *Webcam*-Funktion verwendet. Erst dann können Sie auf Kamerageräte zugreifen. Wenn Ihre App von einem Audiogerät aufzeichnet, müssen Sie auch die *microphone*-Gerätefunktion deklarieren. 

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.



## <a name="select-frame-sources-and-frame-source-groups"></a>Auswählen von Framequellen und Framequellgruppen

Der erste Schritt beim Aufzeichnen von Audio Frames ist das Initialisieren einer [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource), die die Quelle der Audiodaten darstellt, z. B. ein Mikrofon oder ein anderes Gerät für die Audioaufnahme. Zu diesem Zweck müssen Sie eine neue Instanz des [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)-Objekts erstellen. In diesem Beispiel ist die einzige Initialisierungseinstellung für die **MediaCapture** die Einstellung des [**StreamingCaptureMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode), um anzugeben, dass wir des Aufnahmegeräts übertragen möchten. 

Nach dem Aufruf von [**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync), erhalten Sie die Liste der verfügbaren Medienframequellen mit der [**Framequellen**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.framesources)-Eigenschaft. In diesem Beispiel verwendet wir eine Linq-Abfrage, um alle Framequellen auszuwählen, in denen die [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo), die die Framequelle beschreibt, einen [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) von **Audio** hat, und angibt, dass die Medienquelle Audiodaten erzeugt.

Wenn die Abfrage eine oder mehrere Framequellen zurückgibt, können Sie die [**CurrentFormat**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.currentformat)-Eigenschaft überprüfen, um festzustellen, ob die Quelle das gewünschte Audioformat unterstützt – in diesem Beispiel, Float- oder Audiodaten. Überprüfen Sie die [**AudioEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties), um sicherzustellen, dass die gewünschte Audiocodierung von der Quelle unterstützt wird.

[!code-cs[InitAudioFrameSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitAudioFrameSource)]

## <a name="create-and-start-the-mediaframereader"></a>Erstellen und Starten von MediaFrameReader

Erhalten Sie eine neue Instanz des **MediaFrameReader** durch Aufrufen von [**MediaCapture.CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_), und übergeben Sie die **MediaFrameSource** dem im vorherigen Schritt ausgewählten Objekt. Standardmäßig werden Audioframes im gepufferte Modus erhalten, wodurch es weniger wahrscheinlich ist, dass Frames gelöscht werden, obwohl dies weiterhin auftreten kann, wenn Sie nicht schnell genug die Audioframes verarbeiten, und diese den vom System zugewiesenen Speicherpuffer füllen.

Registrieren Sie einen Handler für das [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived)-Ereignis, das immer dann vom System ausgelöst wird, wenn ein neuer Frame mit Audiodaten von der Quelle verfügbar ist. Rufen Sie [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync) auf, um die Übernahme des Audioframes zu beginnen. Wenn der Frame-Reader nicht startet, hat der aus dem Aufruf zurückgegebene Statuswert einen anderen Wert als [**Erfolg**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus).

[!code-cs[CreateAudioFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateAudioFrameReader)]

Im **FrameArrived**-Ereignishandler, rufen Sie [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) auf dem **MediaFrameReader**-Objekt auf, das vom Sender an den Handler für den Versuch übergeben wurde, einen Verweis auf die neueste Medienframe abzurufen. Beachten Sie, dass dieses Objekts null sein kann, daher sollten Sie immer eine Überprüfung durchführen, bevor Sie das Objekt verwenden. Die Arten von umschlossenen Medienframes der **MediaFrameReference**, die von **TryAcquireLatestFrame** zurückgegeben werden, hängt von der Art der Framequelle oder Quellen ab, mit der Sie den Frame-Reader zum Abruf konfiguriert haben. Da der Frame-Reader in diesem Beispiel zum Erwerb von Audioframes eingerichtet wurde, erhält er die zugrunde liegende Frame mithilfe der [**AudioMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe)-Eigenschaft. 

In der **ProcessAudioFrame**-Hilfsmethode im folgenden Beispiel wird gezeigt, wie eine [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) abgerufen wird, die Informationen wie z. B. den Zeitstempel der Frame angibt und ob dieser von nicht mit dem **AudioMediaFrame**-Objekt zusammenhängt. Um die Audio-Beispieldaten zu lesen oder verarbeiten, müssen Sie das [**AudioBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer)-Objekt aus dem **AudioMediaFrame**-Objekt erhalten, ein [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference) erstellen und dann die COM-Methode **IMemoryBufferByteAccess::GetBuffer** zum Abrufen der Daten verwenden. Beachten Sie den Hinweis unter dem Code für weitere Informationen zum Zugriff auf systemeigene Puffer.

Das Format der Daten hängt von der Framequelle ab. In diesem Beispiel, bei der Auswahl der Medienframequelle haben wir explizit sicher gestellt, dass die ausgewählte Framequelle einen einzelnen Kanal an Float-Daten verwendet. Der Rest des Beispielcodes veranschaulicht, wie die Anzahl der Dauer und die Beispielanzahl der Audiodaten im Frame bestimmt werden.  

[!code-cs[ProcessAudioFrame](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetProcessAudioFrame)]

> [!NOTE] 
> Damit die Audiodaten ausgeführt werden können, müssen Sie auf einen nativen Speicherpuffer zugreifen. Verwenden Sie zu diesem Zweck die **IMemoryBufferByteAccess**-COM-Schnittstelle, indem Sie die folgende Codeauflistung hinzufügen. Vorgänge im systemeigenen Puffer müssen in einer Methode ausgeführt werden, die das Schlüsselwort **unsicher** verwendet. Außerdem müssen Sie das Kontrollkästchen zum Zulassen von unsicherem Codes in der Registerkarte **Erstellen** des Dialogfelds **Projekt -> Eigenschaften** aktivieren.

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>Weitere Informationen zur Verwendung von MediaFrameReader mit Audiodaten

Können Sie den [**AudioDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.AudioDeviceController), der der Audioframe-Quelle zugeordnet ist, abrufen durch den Zugriff auf die [**MediaFrameSource.Controller**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.controller)-Eigenschaft. Dieses Objekt kann zum Abrufen oder Festlegen der Datenstromeigenschaften des Aufnahmegeräts oder zum Steuern der Aufzeichnungsebene verwendet werden. Das folgende Beispiel schaltet das Audiogerät stumm, sodass Frames weiterhin vom Frame-Reader erworben werden können, aber alle Beispiele den Wert 0 haben.

[!code-cs[AudioDeviceControllerMute](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetAudioDeviceControllerMute)]

Sie können Sie ein [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe)-Objekt verwenden, um die von der Medienframequelle erfassten Audiodaten an ein [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph) zu übergeben. Übergeben Sie den Frame an die [**AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe)-Methode eine [**AudioFrameInputNode**](https://docs.microsoft.com/en-us/uwp/api/windows.media.audio.audioframeinputnode). Weitere Informationen zur Verwendung von Audiodiagrammen, um Audiosignale zu erfassen, verarbeiten und mischen finden Sie unter [Audiodiagramme](audio-graphs.md).

## <a name="related-topics"></a>Verwandte Themen

* [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md)
* [Kamera](camera.md)
* [Einfaches Foto, Video und Audioerfassung mit mediacapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Beispiel für Kamera Frames](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Audiodiagramme](audio-graphs.md)
 






