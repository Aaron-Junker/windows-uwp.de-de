---
ms.assetid: ''
description: In diesem Artikel wird gezeigt, wie Sie die Open Source-Maschinelles Sehen Bibliothek (opencv) mit der mediaframereader-Klasse verwenden.
title: Verwenden von OpenCV mit „MediaFrameReader”
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, opencv
ms.localizationpriority: medium
ms.openlocfilehash: 2128313c48f8d279a23cf63278b3e853c00348e4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175654"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Verwenden der Open Source-Maschinelles Sehen Bibliothek (opencv) mit mediaframereader

In diesem Artikel erfahren Sie, wie Sie die Open Source-Maschinelles Sehen Bibliothek (opencv), eine Native Code Bibliothek, die eine Vielzahl von Bildverarbeitungsalgorithmen bereitstellt, mit der [**mediaframereader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) -Klasse verwenden, die Medien Frames aus mehreren Quellen simulatanäron lesen kann. Der Beispielcode in diesem Artikel führt Sie durch die Erstellung einer einfachen APP, die Frames von einem Farbsensor abruft, jeden Frame mit der OpenCV-Bibliothek bllädt und dann das verarbeitete Bild in einem XAML- **Bild** Steuerelement anzeigt. 

>[!NOTE]
>Opencv. Win. Core und opencv. Win. imgproc werden nicht regelmäßig aktualisiert und bestehen nicht in den Geschäfts Kompatibilitäts Prüfungen. diese Pakete sind daher nur für Experimente gedacht.

Dieser Artikel baut auf dem Inhalt von zwei anderen Artikeln auf:

* [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md) : Dieser Artikel bietet ausführliche Informationen zur Verwendung von **mediaframereader** zum Abrufen von Frames aus einer oder mehreren Medien Frame Quellen und beschreibt ausführlich den größten Teil des Beispielcodes in diesem Artikel. Die Verarbeitung von **Medien Frames mit mediaframereader** bietet insbesondere das Codelisting für eine Hilfsklasse, **framerenderer**, die die Darstellung von Medien Frames in einem XAML- **Bild** Element behandelt. Der Beispielcode in diesem Artikel verwendet auch diese Hilfsklasse.

* [Verarbeiten von Software Bitmaps mit opencv](process-software-bitmaps-with-opencv.md) : in diesem Artikel werden die Schritte zum Erstellen eines systemeigenen Codes Windows-Runtime-Komponente ( **opencvbridge**) erläutert, die die Konvertierung zwischen dem vom **mediaframereader**-Objekt verwendeten **Software Update** und dem von der OpenCV-Bibliothek verwendeten **matentyp** unterstützt. Im Beispielcode in diesem Artikel wird davon ausgegangen, dass Sie die Schritte zum Hinzufügen der **opencvbridge** -Komponente zur UWP-App-Lösung befolgt haben.

Zusätzlich zu diesen Artikeln finden Sie im GitHub-Repository "Windows Universal Samples" eine vollständige End-to-End-Funktionsweise des Szenarios [, das in](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) diesem Artikel beschrieben wird.

Um mit der schnellen Entwicklung zu beginnen, können Sie die OpenCV-Bibliothek mithilfe von nuget-Paketen in ein UWP-App-Projekt einschließen. diese Pakete bestehen jedoch möglicherweise nicht im certficiationsprozess der APP, wenn Sie Ihre APP an den Store übermitteln Informationen zum Entwickeln mit opencv finden Sie unter [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>Implementieren der systemeigenen Windows-Runtime-Komponente von opencvhelper
Führen Sie die Schritte in [Verarbeiten von Software Bitmaps mit opencv](process-software-bitmaps-with-opencv.md) aus, um die opencv Helper Windows-Runtime-Komponente zu erstellen und der UWP-App-Lösung einen Verweis auf das Komponenten Projekt hinzuzufügen.

## <a name="find-available-frame-source-groups"></a>Suchen nach verfügbaren Frame Quell Gruppen
Zunächst müssen Sie eine Medien Frame-Quell Gruppe suchen, aus der Medien Frames abgerufen werden. Rufen Sie die Liste der verfügbaren Quell Gruppen auf dem aktuellen Gerät durch Aufrufen von **[mediaframesourcegroup. findallasync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** ab. Wählen Sie dann die Quell Gruppen aus, die die für das App-Szenario erforderlichen Sensortypen bereitstellen. In diesem Beispiel benötigen wir einfach eine Quell Gruppe, die Frames von einer RGB-Kamera bereitstellt.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialisieren des MediaCapture-Objekts
Als nächstes müssen Sie das **mediacapture** -Objekt initialisieren, um die im vorherigen Schritt ausgewählte Frame-Quell Gruppe zu verwenden, indem Sie die **[sourcegroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** -Eigenschaft von **mediacaptureinitializationsettings**festlegen.

> [!NOTE] 
> Das von der opencvhelper-Komponente verwendete Verfahren, das in [Verarbeiten von Software Bitmaps mit opencv](process-software-bitmaps-with-opencv.md)ausführlich beschrieben wird, erfordert, dass sich die Bilddaten im CPU-Speicher und nicht im GPU-Arbeitsspeicher befinden. Daher sollten Sie " **memorypreference. CPU** " für das Feld " **[memorypreference](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** " von " **mediacaptureinitializationsettings**" angeben.

Nachdem das **mediacapture** -Objekt initialisiert wurde, erhalten Sie einen Verweis auf die RGB-Frame Quelle, indem Sie auf die **[mediacapture. framesources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** -Eigenschaft zugreifen.

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>Initialisieren von mediaframereader
Erstellen Sie als nächstes einen [**mediaframereader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) für die RGB-Frame Quelle, die Sie im vorherigen Schritt abgerufen haben. Um eine gute Framerate beizubehalten, können Sie Frames mit einer niedrigeren Auflösung als der Auflösung des Sensors verarbeiten. In diesem Beispiel wird das optionale **[bitmapsize](/uwp/api/windows.graphics.imaging.bitmapsize)** -Argument der **[mediacapture. kreateframereaderasync](/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** -Methode bereitgestellt, um anzufordern, dass die Größe der Frames, die vom Frame Reader bereitgestellt werden, auf 640 x 480 Pixel geändert werden.

Registrieren Sie nach dem Erstellen des Frame Readers einen Handler für das **[framekam](/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** -Ereignis. Erstellen Sie dann ein neues **[softwarebitmapsource](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** -Objekt, das von der **framerenderer** -Hilfsklasse zum Darstellen des verarbeiteten Bilds verwendet wird. Anschließend wird der Konstruktor für den **framerenderer**aufgerufen. Initialisieren Sie die Instanz der **opencvhelper** -Klasse, die in der Windows-Runtime Komponente von opencvbridge definiert ist. Diese Hilfsklasse wird im **frameeach** -Handler zum Verarbeiten der einzelnen Frames verwendet. Starten Sie abschließend den Frame Reader, indem Sie **[startasync](/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)** aufrufen.

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>Behandeln des framekam-Ereignisses
Das **framekam** -Ereignis wird ausgelöst, wenn ein neuer Frame vom Frame Reader verfügbar ist. Wenden Sie **[tryacquirelatestframe](/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** an, um den Frame abzurufen, sofern dieser vorhanden ist. Holen Sie sich die **softwarebitmap** aus dem **[mediaframereferenzierungsverzeichnis](/uwp/api/windows.media.capture.frames.mediaframereference)**. Beachten Sie, dass die **cvhelper** -Klasse, die in diesem Beispiel verwendet wird, Bilder benötigt, um das BRGA8-Pixel-Format mit prämultipliziertem Alpha Wenn der an das Ereignis umgegebene Frame ein anderes Format aufweist, konvertieren Sie die **softwarframemap** in das richtige Format. Erstellen Sie als nächstes eine **softwarframemap** , die als Ziel für den weichzeichenvorgang verwendet werden soll. Die Eigenschaften des Quell Bilds werden als Argumente für den Konstruktor verwendet, um eine Bitmap mit überein stimmendem Format zu erstellen. Um den Frame zu verarbeiten, wenden Sie die Methode " **Blur** " der Hilfsklasse Übergeben Sie schließlich das Ausgabe Bild des weichzeichenvorgangs in **presentsoftwarebitmap**, die Methode der **framerenderer** -Hilfsklasse, die das Bild im XAML- **Bild** Steuerelement anzeigt, mit dem es initialisiert wurde.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>Zugehörige Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Verarbeiten von Medienframes mit „MediaFrameReader“](process-media-frames-with-mediaframereader.md)
* [Verarbeiten von Software Bitmaps mit opencv](process-software-bitmaps-with-opencv.md)
* [Beispiel für Kameraframes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Kamera Frames + opencv-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 