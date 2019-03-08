---
ms.assetid: ''
description: In diesem Artikel wird erläutert, wie Sie die Open Source Computer Vision-Bibliothek (OpenCV) mit der MediaFrameReader-Klasse verwenden.
title: Verwenden von OpenCV mit „MediaFrameReader”
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, openCV
ms.localizationpriority: medium
ms.openlocfilehash: d72a8d3fcaf337973f585ab19370140cd80f3826
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640175"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Verwenden Sie die Open Source Computer Vision-Bibliothek (OpenCV) mit MediaFrameReader

In diesem Artikel wird erläutert, wie Sie die Open Source Computer Vision-Bibliothek (OpenCV), eine systemeigene Code-Bibliothek, die eine Vielzahl von Algorithmen für die Bildverarbeitung bietet, mit der [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader)-Klasse verwenden, die Medien aus mehreren Quellen gleichzeitig lesen kann. Der Beispielcode in diesem Artikel führt Sie durch die Erstellung einer einfachen App, die Frames von einem Farbsensor erhält, jede Frame mithilfe der OpenCV-Bibliothek verwischt und dann das verarbeitete Bild in einem XAML-**Image** Steuerelement anzeigt. 

>[!NOTE]
>OpenCV.Win.Core und OpenCV.Win. ImgProc werden nicht regelmäßig aktualisiert, werden jedoch weiterhin für das Erstellen von OpenCVHelper empfohlen, wie auf dieser Seite beschrieben.

Dieser Artikel baut auf dem Inhalt von zwei anderen Artikeln auf:

* [Verarbeiten von Medienframes mit "MediaFrameReader"](process-media-frames-with-mediaframereader.md) – dieser Artikel enthält ausführliche Informationen zur Verwendung von **MediaFrameReader** zum Abrufen von Frames aus einer oder mehreren Medienframequellen und beschreibt ausführlich die meisten Beispielcodes in diesem Artikel. Vor allem der Artikel **Verarbeiten von Medienframes mit „MediaFrameReader“** enthält den Code für eine Hilfsklasse **FrameRenderer**, die die Darstellung von Medienframes in einem XAML-**Bild**element behandelt. Der Beispielcode in diesem Artikel verwendet ebenfalls diese Hilfsklasse.

* [Verarbeiten von Softwarebitmaps mit OpenCV](process-software-bitmaps-with-opencv.md) – dieser Artikel führt Sie durch die Erstellung einer mit systemeigenem Code erstellten Komponente für Windows-Runtime, **OpenCVBridge**. Dies hilft beim Konvertieren zwischen einem **SoftwareBitmap**-Objekt, das von **MediaFrameReader** verwendet wird und dem **Mat**-Typ, der von der OpenCV-Bibliothek verwendet wird. Im Beispielcode in diesem Artikel wird vorausgesetzt, dass Sie die Schritte zum Hinzufügen der **OpenCVBridge**-Komponente zu Ihrer UWP-App-Lösung befolgt haben.

Wenn Sie zusätzlich zu diesem Artikel ein vollständiges, funtionsfähiges End-to-End-Beispiel des in diesem Artikel beschriebenen Szenarios anzeigen und herunterladen möchten, finden Sie dies unter [Kamera-Frames + OpenCV – Beispiel](https://go.microsoft.com/fwlink/?linkid=854003) im GitHub-Repository für Beispiele für die Universelle Windows-Plattform.

Informationen zum Einstieg schnell entwickeln, können Sie die OpenCV-Bibliothek in eine UWP-app-Projekt einschließen, mithilfe von NuGet-Pakete, aber diese Pakete möglicherweise nicht den app-Prozess Certficication übergeben, wenn Sie Ihre app auf den Store übermitteln, daher wird empfohlen, dass Sie die OpenCV herunterladen Bibliothek Quellcode und die Binärdateien vor dem übermitteln der app erstellen. Informationen zum Entwickeln mit OpenCV finden Sie unter [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>Implementieren der systemeigenem OpenCVHelper-Komponente für Windows-Runtime
Führen Sie die Schritte unter [Verarbeiten von Softwarebitmaps mit OpenCV](process-software-bitmaps-with-opencv.md) durch, um die OpenCV Helferkomponente für Windows-Runtime zu erstellen und der UWP-App-Lösung einen Verweis auf das Komponentenprojekt hinzuzufügen.

## <a name="find-available-frame-source-groups"></a>Suchen nach verfügbaren Framequellgruppen
Zunächst müssen Sie eine Framequellgruppe von Medien finden, aus der die Medienframes abgerufen werden. Erhalten Sie die Liste der verfügbaren Gruppen auf dem aktuellen Gerät durch Aufrufen von **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)**. Wählen Sie anschließend die Quellgruppen aus, die die für Ihr App-Szenario erforderlichen Sensortypen enthalten. In diesem Beispiel benötigen wir lediglich eine Quellgruppe, die Frames von einer RGB-Kamera bereitstellt.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialisieren des MediaCapture-Objekts
Initialisieren Sie anschließend das **MediaCapture**-Objekt durch Festlegen der Eigenschaften von **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** der **MediaCaptureInitializationSettings**, um die im vorherigen Schritt ausgewählte Framequellgruppe zu verwenden.

> [!NOTE] 
> Die Technik, die von der OpenCV-Helferkomponente verwendet wird und die in [Verarbeiten von Softwarebitmaps mit OpenCV](process-software-bitmaps-with-opencv.md) ausführlich beschrieben ist, erfordert, dass sich die Bilddaten im CPU-Speicher und nicht im GPU-Speicher befinden. Sie sollten daher **MemoryPreference.CPU** für die **[MemoryPreference](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** im Feld der **MediaCaptureInitializationSettings** angeben.

Nachdem das **MediaCapture**-Objet initialisiert wurde, rufen Sie einen Verweis auf die RGB-Framequelle durch den Zugriff auf die **[MediaCapture.FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)**-Eigenschaft auf.

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>Initialisieren des MediaFrameReader
Als Nächstes erstellen Sie einen [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) für die RGB-Framequelle, die in den vorherigen Schritten abgerufen wurde. Um eine gute Framerate zu gewährleisten, sollten Sie Frames verarbeiten, die eine niedrigere Auflösung als die Auflösung des Sensors haben. Dieses Beispiel bietet das optionale **[BitmapSize](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapsize)**-Argument für die **[MediaCapture.CreateFrameReaderAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync)**-Methode, um anzufordern, dass die vom Frame-Reader bereitgestellten Frames auf 640 x 480 Pixel geändert werden.

Nach dem Erstellen des Frame-Readers, registrieren Sie einen Handler für das **[FrameArrived](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)**-Ereignis. Erstellen Sie anschließend ein neues **[SoftwareBitmapSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)**-Objekt, das die **FrameRenderer**-Hilfsklasse verwendet, um das verarbeitete Bild darzustellen. Rufen Sie dann den Konstruktor für den **FrameRenderer** auf. Initialisieren Sie die Instanzen der **OpenCVHelper**-Klasse, die in der OpenCVBridge Komponente für Windows-Runtime definiert ist. Diese Hilfsklasse wird im **FrameArrived**-Ereignishandler verwendet, um jeden Frame zu verarbeiten. Starten Sie anschließend die den Framereader durch Aufrufen von **[StartAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)**.

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>Behandeln des „FrameArrived“-Ereignisses
Das **FrameArrived**-Ereignis wird ausgelöst, wenn ein neuer Frame von FrameReader verfügbar ist. Rufen Sie **[TryAcquireLatestFrame](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** auf, um das Frame zu erhalten, wenn es vorhanden ist. Rufen Sie **SoftwareBitmap** aus **[MediaFrameReference](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference)** auf. Beachten Sie, dass die **CVHelper**-Klasse, die in diesem Beispiel verwendet wird, Bilder im BRGA8-Format mit vormultipliziertem Alpha erfordert. Wenn das in das Ereignis übergebene Frame ein anderes Format hat, konvertieren Sie die **SoftwareBitmap** in das richtige Format. Erstellen Sie als Nächstes ein **SoftwareBitmap**, um als Ziel für den Weichzeichnervorgang zu verwenden. Die Quell-Bildeigenschaften werden als Argumente für den Konstruktor verwendet, um eine Bitmap mit übereinstimmendem Format erstellen. Rufen Sie die **Weichzeichner**-Methode der Hilfsklasse auf, um den Frame zu verarbeiten. Übergeben Sie abschließend das Bild des Weichzeichnervorgangs in **PresentSoftwareBitmap**, die Methode der **FrameRenderer**-Hilfsklasse, die das Bild im XAML-**Bild**-Steuerelement anzeigt, mit dem es initialisiert wurde.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Verarbeiten von Medien-Frames mit MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Verarbeiten von Software-Bitmaps mit OpenCV](process-software-bitmaps-with-opencv.md)
* [Beispiel für Camera-frames](https://go.microsoft.com/fwlink/?LinkId=823230)
* [Kamera-Frames und OpenCV-Beispiele](https://go.microsoft.com/fwlink/?linkid=854003)
 

 




