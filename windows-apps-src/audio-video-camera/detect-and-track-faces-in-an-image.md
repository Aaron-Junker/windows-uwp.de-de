---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: In diesem Thema erfahren Sie, wie mit dem FaceDetector Gesichter in einem Bild erkannt werden. Der FaceTracker ist für die Verfolgung von Gesichtern im zeitlichen Verlauf einer Sequenz von Videoframes optimiert.
title: Erkennen von Gesichtern in Bildern oder Videos
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6232a8003140284956319728e16b3b953aff3177
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362733"
---
# <a name="detect-faces-in-images-or-videos"></a>Erkennen von Gesichtern in Bildern oder Videos



\[Einige Informationen beziehen sich auf Vorabversionen, die vor der kommerziellen Freigabe grundlegend geändert werden können. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.\]

In diesem Thema erfahren Sie, wie mit dem [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) Gesichter in einem Bild erkannt werden. [**Facetracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) ist für die Nachverfolgung von Gesichtern in einer Sequenz von Video Frames optimiert.

Ein alternatives Verfahren der Gesichtsverfolgung mit dem [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) finden Sie unter [Szenenanalyse für die Medienaufnahme](scene-analysis-for-media-capture.md).

Der Code in diesem Artikel wurde aus den Beispielen [Basic Face Detection](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection) und [Basic Face Tracking](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking) übernommen und angepasst. Sie können diese Beispiele herunterladen, um den verwendeten Code im Kontext anzuzeigen oder das Beispiel als Ausgangspunkt für Ihre eigene App zu verwenden.

## <a name="detect-faces-in-a-single-image"></a>Erkennen von Gesichtern in einem einzelnen Bild

Die [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector)-Klasse ermöglicht das Erkennen von einem oder mehreren Gesichtern in einem Standbild.

In diesem Beispiel werden APIs aus den folgenden Namespaces verwendet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetFaceDetectionUsing":::

Deklarieren Sie eine Klassenmembervariable für das [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector)-Objekt und für die Liste der [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace)-Objekte, die im Bild erkannt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetClassVariables1":::

Die Gesichtserkennung arbeitet mit einem [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekt, das auf vielfältige Weise erstellt werden kann. In diesem Beispiel wird ein [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) verwendet, um dem Benutzer das Auswählen einer Bilddatei zu ermöglichen, in der Gesichter erkannt werden. Weitere Informationen zum Arbeiten mit Softwarebitmaps finden Sie unter [Bildverarbeitung](imaging.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetPicker":::

Verwenden Sie die [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)-Klasse, um die Bilddatei als **SoftwareBitmap** zu decodieren. Mit einem kleineren Bild erfolgt die Gesichtserkennung schneller. Daher empfiehlt es sich, das Quellbild auf eine geringere Größe zu skalieren. Dies kann während der Decodierung erfolgen, indem ein [**BitmapTransform**](/uwp/api/Windows.Graphics.Imaging.BitmapTransform)-Objekt erstellt wird, die Eigenschaften [**ScaledWidth**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledwidth) und [**ScaledHeight**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledheight) festgelegt werden und das Objekt an den Aufruf von [**GetSoftwareBitmapAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) übergeben wird, der die decodierte und skalierte **SoftwareBitmap** zurückgibt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetDecode":::

In der aktuellen Version unterstützt die **FaceDetector**-Klasse nur Bilder im Pixelformat Gray8 oder Nv12. Die **SoftwareBitmap**-Klasse stellt die [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert)-Methode bereit, die eine Bitmap in ein anderes Format umwandelt. In diesem Beispiel wird das Quellbild in das Pixelformat Gray8 umgewandelt, sofern es nicht bereits dieses Format aufweist. Sie können ggf. mithilfe der Methoden [**GetSupportedBitmapPixelFormats**](/uwp/api/windows.media.faceanalysis.facedetector.getsupportedbitmappixelformats) und [**IsBitmapPixelFormatSupported**](/uwp/api/windows.media.faceanalysis.facedetector.isbitmappixelformatsupported) zur Laufzeit bestimmen, ob ein Pixelformat unterstützt wird, für den Fall, dass der Satz der unterstützten Formate in zukünftigen Versionen erweitert wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetFormat":::

Instanziieren Sie das **FaceDetector**-Objekt, indem Sie [**CreateAsync**](/uwp/api/windows.media.faceanalysis.facedetector.createasync) und dann [**DetectFacesAsync**](/uwp/api/windows.media.faceanalysis.facedetector.detectfacesasync) aufrufen, und übergeben Sie die Bitmap, die auf eine geeignete Größe skaliert und in ein unterstütztes Pixelformat konvertiert wurde. Diese Methode gibt eine Liste von [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace)-Objekten zurück. **ShowDetectedFaces** ist eine Hilfsmethode (unten dargestellt), die Quadrate um die Gesichter im Bild zeichnet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetDetect":::

Stellen Sie sicher, dass die Objekte gelöscht werden, die während der Gesichtserkennung erstellt wurden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetDispose":::

Um das Bild anzuzeigen und Rahmen um die erkannten Gesichter zu zeichnen, fügen Sie der XAML-Seite ein [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas)-Element hinzu.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml" id="SnippetCanvas":::

Definieren Sie Membervariablen, um die zu zeichnenden Quadrate zu formatieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetClassVariables2":::

In der **ShowDetectedFaces**-Hilfsmethode wird ein neuer [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) erstellt, und die Quelle wird auf eine [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) festgelegt, die aus der das Quellbild darstellenden **SoftwareBitmap** erstellt wird. Der Hintergrund des XAML **Canvas**-Steuerelements wird auf den Bildpinsel festgelegt.

Wenn die Liste der an die Hilfsmethode übergebenen Gesichter nicht leer ist, durchlaufen Sie die einzelnen Gesichter in der Liste, und bestimmen Sie mithilfe der [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox)-Eigenschaft der [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace)-Klasse die Position und Größe des Rechtecks im Bild, das das Gesicht enthält. Da die Größe des **Canvas**-Steuerelements sehr wahrscheinlich nicht mit der Größe des Quellbilds übereinstimmt, sollten Sie die X- und Y-Koordinate sowie die Breite und Höhe der **FaceBox** mit einem Skalierungswert multiplizieren, der das Verhältnis der Quellbildgröße zur tatsächlichen Größe des **Canvas**-Steuerelements angibt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetShowDetectedFaces":::

## <a name="track-faces-in-a-sequence-of-frames"></a>Verfolgen von Gesichtern in einer Framesequenz

Für die Gesichtserkennung in einem Video ist es effizienter, die [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker)-Klasse anstelle der [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector)-Klasse zu verwenden. Die Implementierungsschritte sind jedoch sehr ähnlich. Der **FaceTracker** optimiert den Erkennungsprozess anhand von Informationen über bereits verarbeitete Frames.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetFaceTrackingUsing":::

Deklarieren Sie eine Klassenvariable für das **FaceTracker**-Objekt. In diesem Beispiel wird ein [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) verwendet, um die Gesichtsverfolgung für ein definiertes Intervall zu initiieren. Mit einem [SemaphoreSlim](/dotnet/api/system.threading.semaphoreslim) wird sichergestellt, dass immer nur ein Gesichtsverfolgungsvorgang ausgeführt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetClassVariables3":::

Erstellen Sie zum Initialisieren des Gesichtsverfolgungsvorgangs ein neues **FaceTracker**-Objekt, indem Sie [**CreateAsync**](/uwp/api/windows.media.faceanalysis.facetracker.createasync) aufrufen. Initialisieren Sie das gewünschte Timerintervall, und erstellen Sie dann den Timer. Immer wenn das angegebene Intervall verstrichen ist, wird die **ProcessCurrentVideoFrame**-Hilfsmethode aufgerufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetTrackingInit":::

Die **ProcessCurrentVideoFrame**-Hilfsmethode wird vom Timer asynchron aufgerufen. Deshalb ruft die Methode zunächst die **Wait**-Methode des Semaphors auf, um festzustellen, ob derzeit ein Verfolgungsvorgang ausgeführt wird. Wenn dies der Fall ist, wird die Methode beendet, ohne dass versucht wird, Gesichter zu erkennen. Am Ende dieser Methode wird die **Release**-Methode des Semaphors aufgerufen, sodass mit dem nachfolgenden Aufruf von **ProcessCurrentVideoFrame** fortgefahren werden kann.

Die [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) Klasse arbeitet mit [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame)-Objekten. Es gibt mehrere Möglichkeiten, ein **VideoFrame** zu erhalten, z. B. durch Aufzeichnen eines Vorschauframes aus einem derzeit ausgeführten [MediaCapture](./index.md)-Objekt oder durch Implementieren der [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicaudioeffect.processframe)-Methode von [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect). In diesem Beispiel wird die nicht definierte Hilfsmethode **GetLatestFrame**, die einen Videoframe zurückgibt, als Platzhalter für diesen Vorgang verwendet. Informationen zum Abrufen von Videoframes aus dem Vorschaudatenstrom eines derzeit ausgeführten Medienaufnahmegeräts finden Sie unter [Abrufen eines Vorschauframes](get-a-preview-frame.md).

**FaceDetector** unterstützt wie **FaceTracker** eine begrenzte Anzahl von Pixelformaten. In diesem Beispiel wird die Gesichtserkennung abgebrochen, wenn der bereitgestellte Frame nicht das Format Nv12 aufweist.

Rufen Sie [**ProcessNextFrameAsync**](/uwp/api/windows.media.faceanalysis.facetracker.processnextframeasync) auf, um eine Liste von [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace)-Objekten, die die Gesichter im Frame darstellen, abzurufen. Sobald Sie über die Liste der Gesichter verfügen, können Sie sie (auf die gleiche Weise wie weiter oben für die Gesichtserkennung beschrieben) anzeigen. Beachten Sie, dass Sie alle Aktualisierungen der Benutzeroberfläche innerhalb eines Aufrufs von [**coredispatcher. runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync)ausführen müssen, da die Hilfsmethode für die Gesichts Verfolgung nicht im UI-Thread aufgerufen wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetProcessCurrentVideoFrame":::

## <a name="related-topics"></a>Verwandte Themen

* [Szenenanalyse für die Medienaufnahme](scene-analysis-for-media-capture.md)
* [Beispiel „Basic Face Detection“](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection)
* [Beispiel „Basic Face Tracking“](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking)
* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Medienwiedergabe](media-playback.md)
