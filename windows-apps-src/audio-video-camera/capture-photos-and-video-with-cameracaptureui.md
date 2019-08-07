---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: In diesem Artikel wird beschrieben, wie Sie mit der cameracaptureui-Klasse Fotos oder Videos mithilfe der in Windows integrierten Kamera-Benutzeroberfläche erfassen.
title: Fotos und Videos mit der Windows-Benutzeroberfläche für integrierte Kameras erfassen
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4fb8132e7b382ee801d83986261bf71d8fef490f
ms.sourcegitcommit: 04a6e60c3b24d6efae0f0e2ada1d66a369471fb3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68830416"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Fotos und Videos mit der Windows-Benutzeroberfläche für integrierte Kameras erfassen



In diesem Artikel wird beschrieben, wie Sie mit der cameracaptureui-Klasse Fotos oder Videos mithilfe der in Windows integrierten Kamera-Benutzeroberfläche erfassen. Diese Funktion ist einfach zu verwenden. Sie ermöglicht es Ihrer APP, ein Foto oder Video mit nur wenigen Codezeilen zu erhalten.

Wenn Sie eine eigene Kamera-UI bereitstellen möchten oder Ihr Szenario eine zuverlässigere Steuerung des Aufnahmevorgangs auf unterster Ebene erfordert, sollten Sie das [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)-Objekt verwenden und Ihre eigene Aufzeichnungsoberfläche implementieren. Weitere Informationen finden Sie unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> Sie sollten die **Webcam** -oder **Mikrofon** -Funktionen nicht in der APP-Manifest-Datei angeben, wenn Ihre APP nur cameracaptureui verwendet. Wenn Sie dies tun, wird Ihre APP in den Datenschutzeinstellungen des Geräts angezeigt, aber selbst wenn der Benutzer den Kamera Zugriff auf Ihre APP verweigert, verhindert dies, dass "cameracaptureui" Medien erfasst. <p>Das liegt daran, dass es sich bei der integrierten Kamera-App von Windows um eine vertrauenswürdige Erstanbieter-App handelt, die erfordert, dass der Benutzer die Foto-, Audio- und Videoaufnahme durch einen Tastendruck initiiert. Bei Verwendung von "cameracaptureui" bei Verwendung von "cameracaptureui" als einziger Foto Erfassungs Mechanismus kann bei Ihrer APP ein Fehler bei der Zertifizierung des Zertifizierungs Kits Microsoft Store für Windows-Anwendungen auftreten.<p>
Sie müssen die Webcam-oder Mikrofon-Funktionen in der APP-Manifest-Datei angeben, wenn Sie mediacapture verwenden, um Audioinhalte, Fotos oder Videos Programm gesteuert zu erfassen.

## <a name="capture-a-photo-with-cameracaptureui"></a>Aufnehmen eines Fotos mit „CameraCaptureUI“

Um die Kameraaufnahme-UI zu verwenden, schließen Sie den [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture)-Namespace in Ihr Projekt ein. Um Dateivorgänge mit der zurückgegebenen Bilddatei auszuführen, schließen Sie [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) ein.

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Um ein Foto aufzunehmen, erstellen Sie ein neues [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Objekt. Mithilfe der Eigenschaft " [**photosettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings) " des Objekts können Sie Eigenschaften für das zurückgegebene Foto angeben, z. b. das Bildformat des Fotos. Standardmäßig unterstützt die Kamera Fassungs Benutzeroberfläche das Ausschneiden des Fotos, bevor es zurückgegeben wird. Dies kann mit der [**allowcropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) -Eigenschaft deaktiviert werden. In diesem Beispiel wird [**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) festgelegt, um anzufordern, dass das zurückgegebene Bild 200 x 200 Pixel hat.

> [!NOTE]
> Das Zuschneiden von Bildern in " **cameracaptureui** " wird für Geräte in der mobilen Gerätefamilie nicht unterstützt. Der Wert der [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping)-Eigenschaft wird ignoriert, wenn Ihre App auf diesen Geräten ausgeführt wird.

Rufen Sie [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) auf, und geben Sie [**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) an, um festzulegen, dass ein Foto aufgenommen werden soll. Die Methode gibt eine [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Instanz mit dem Bild zurück, wenn die Aufnahme erfolgreich ist. Wenn der Benutzer die Aufnahme abbricht, ist das zurückgegebene Objekt „null“.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

Die **StorageFile** mit dem aufgenommenen Foto erhält einen dynamisch generierten Namen und wird im lokalen Ordner der App gespeichert. Um die aufgezeichneten Fotos besser zu organisieren, können Sie die Datei in einen anderen Ordner verschieben.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Zur Verwendung des Fotos in der App kann die Erstellung eines [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekts hilfreich sein, das mit unterschiedlichen Funktionen für universelle Windows-Apps genutzt werden kann.

Zuerst sollten Sie den [**Windows. Graphics. Imaging**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) -Namespace in Ihr Projekt einschließen.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Rufen Sie [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) auf, um einen Stream aus der Bilddatei abzurufen. Rufen Sie [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) auf, um einen Bitmap-Decoder für den Stream abzurufen. Anschließend können Sie [**getsoftwarebitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) aufrufen, um eine Darstellung des Bilds in der **Software** zu erhalten.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Um das Bild in der Benutzeroberfläche anzuzeigen, deklarieren Sie ein [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)-Steuerelement auf der XAML-Seite.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Um die Software-Bitmap in der XAML-Seite zu verwenden, schließen Sie den [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging)-Namespace in Ihr Projekt ein.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

Das **Image** -Steuerelement erfordert, dass die Bildquelle das Format BGRA8 mit einem prämultiplizierten Alpha oder ohne Alpha hat. Rufen Sie die statische Methode [**softwarebitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) auf, um eine neue Software Bitmap mit dem gewünschten Format zu erstellen. Erstellen Sie als nächstes ein neues [**softwarebitmapsource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) -Objekt, und nennen Sie es [**setbitmapasync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) , um die Software Bitmap der Quelle zuzuweisen. Legen Sie abschließend die [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source)-Eigenschaft des **Image**-Steuerelements fest, um das aufgenommene Foto in der Benutzeroberfläche anzuzeigen.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Aufnehmen eines Videos mit CameraCaptureUI

Um ein Video aufzunehmen, erstellen Sie ein neues [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Objekt. Mithilfe der [**videosettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) -Eigenschaft des Objekts können Sie Eigenschaften für das zurückgegebene Video angeben, z. b. das Format des Videos.

Aufrufen von [**capturefileasync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) und Angeben von [**Video**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) zum Erfassen eines Videos. Die Methode gibt eine [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Instanz mit dem Video zurück, wenn die Aufnahme erfolgreich ist. Wenn Sie die Erfassung abbrechen, ist das zurückgegebene Objekt NULL.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Wofür Sie die aufgenommene Videodatei verwenden, ist von dem Szenario für Ihre App abhängig. Im weiteren Verlauf dieses Artikels wird gezeigt, wie Sie schnell eine Medienkomposition aus einem oder mehreren aufgenommenen Videos erstellen und diese in der Benutzeroberfläche anzeigen.

Fügen Sie zunächst ein [**mediaplayerelement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) -Steuerelement hinzu, in dem die Videokomposition auf der XAML-Seite angezeigt wird.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


Wenn die Videodatei von der Kameraaufnahme-Benutzeroberfläche zurückgegeben wird, erstellen Sie eine neue [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) durch Aufrufen von **[createfromstoragefile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** . Rufen Sie zur Wiedergabe des Videos die **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** -Methode aus dem Standard- **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** auf, der dem **MediaPlayerElement** zugeordnet ist.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Einfaches Foto, Video und Audioerfassung mit mediacapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




