---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: Dieser Artikel beschreibt, wie Sie die „CameraCaptureUI“-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden.
title: Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 35eed8310b406a960334c90d6c359c0313b2660c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358898"
---
# <a name="capture-photos-and-video-with-windows-built-in-camera-ui"></a>Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI



Dieser Artikel beschreibt, wie Sie die „CameraCaptureUI“-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden. Dieses Feature ist benutzerfreundlich und ermöglicht, dass die App ein vom Benutzer aufgenommenes Fotos oder Video mit nur wenigen Codezeilen abruft.

Wenn Sie eine eigene Kamera-UI bereitstellen möchten oder Ihr Szenario eine zuverlässigere Steuerung des Aufnahmevorgangs auf unterster Ebene erfordert, sollten Sie das [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)-Objekt verwenden und Ihre eigene Aufzeichnungsoberfläche implementieren. Weitere Informationen finden Sie unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> Geben Sie nicht die **Webcam** oder **Mikrofon** Funktionen in Ihrer app-Manifestdatei, wenn Ihre app nur "cameracaptureui" verwendet. Andernfalls wird Ihre App in den Datenschutzeinstellungen für die Kamera angezeigt. Aber auch wenn der Benutzer den Kamerazugriff auf Ihre App verweigert, wird dadurch nicht verhindert, dass „CameraCaptureUI“ Medien aufzeichnet. Das liegt daran, dass es sich bei der integrierten Kamera-App von Windows um eine vertrauenswürdige Erstanbieter-App handelt, die erfordert, dass der Benutzer die Foto-, Audio- und Videoaufnahme durch einen Tastendruck initiiert. Ihre app kann fehlschlagen, wenn an den Store übermittelt werden soll, wenn Sie auf die Webkamera oder Mikrofon-Funktionen angeben, bei Verwendung von "cameracaptureui" als Ihre einzige Foto aufzeichnungsmechanismus Zertifizierungskit für Windows-Anwendung-Zertifizierung.
> Sie müssen die Webcam- oder Mikrofonfunktion in Ihrem App-Manifest angeben, wenn Sie „MediaCapture“ für die programmgesteuerte Audio-, Foto- oder Videoaufnahme verwenden.

## <a name="capture-a-photo-with-cameracaptureui"></a>Aufnehmen eines Fotos mit „CameraCaptureUI“

Um die Kameraaufnahme-UI zu verwenden, schließen Sie den [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture)-Namespace in Ihr Projekt ein. Um Dateivorgänge mit der zurückgegebenen Bilddatei auszuführen, schließen Sie [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) ein.

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Um ein Foto aufzunehmen, erstellen Sie ein neues [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Objekt. Durch Verwendung der [**PhotoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings)-Eigenschaft des Objekts können Sie Eigenschaften für das zurückgegebene Foto angeben, z. B. das Bildformat des Fotos. Standardmäßig kann der Benutzer über die Kameraaufnahme-UI das Foto zuschneiden, bevor es zurückgegeben wird. Dies kann aber mit der [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping)-Eigenschaft deaktiviert werden. In diesem Beispiel wird [**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) festgelegt, um anzufordern, dass das zurückgegebene Bild 200 x 200 Pixel hat.

> [!NOTE]
> Das Zuschneiden von Bildern in **CameraCaptureUI** wird für Geräte in der Mobilgerätefamilie nicht unterstützt. Der Wert der [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping)-Eigenschaft wird ignoriert, wenn Ihre App auf diesen Geräten ausgeführt wird.

Rufen Sie [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.) auf, und geben Sie [**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) an, um festzulegen, dass ein Foto aufgenommen werden soll. Die Methode gibt eine [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Instanz mit dem Bild zurück, wenn die Aufnahme erfolgreich ist. Wenn der Benutzer die Aufnahme abbricht, ist das zurückgegebene Objekt „null“.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

Die **StorageFile** mit dem aufgenommenen Foto erhält einen dynamisch generierten Namen und wird im lokalen Ordner der App gespeichert. Es kann ratsam sein, die Datei in einen anderen Ordner zu verschieben, um die aufgenommenen Fotos besser zu organisieren.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Zur Verwendung des Fotos in der App kann die Erstellung eines [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekts hilfreich sein, das mit unterschiedlichen Funktionen für universelle Windows-Apps genutzt werden kann.

Sie sollten zunächst den [**Windows.Graphics.Imaging**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging)-Namespace in Ihr Projekt einschließen.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Rufen Sie [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) auf, um einen Stream aus der Bilddatei abzurufen. Rufen Sie [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) auf, um einen Bitmap-Decoder für den Stream abzurufen. Rufen Sie dann [**GetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) auf, um eine **SoftwareBitmap**-Darstellung des Bilds abzurufen.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Um das Bild in der Benutzeroberfläche anzuzeigen, deklarieren Sie ein [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)-Steuerelement auf der XAML-Seite.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Um die Software-Bitmap in der XAML-Seite zu verwenden, schließen Sie den [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging)-Namespace in Ihr Projekt ein.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

Das **Image**-Steuerelement erfordert, dass die Bildquelle das BGRA8-Format mit vormultipliziertem oder gar keinem Alphaformat aufweist. Rufen Sie daher die statische Methode [**SoftwareBitmap.Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows) auf, um eine neue Software-Bitmap mit dem gewünschten Format zu erstellen. Erstellen Sie als Nächstes ein neues [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Objekt, und rufen Sie [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) auf, um die Software-Bitmap der Quelle zuzuweisen. Legen Sie abschließend die [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source)-Eigenschaft des **Image**-Steuerelements fest, um das aufgenommene Foto in der Benutzeroberfläche anzuzeigen.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Aufnehmen eines Videos mit CameraCaptureUI

Um ein Video aufzunehmen, erstellen Sie ein neues [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Objekt. Durch Verwendung der [**VideoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings)-Eigenschaft des Objekts können Sie Eigenschaften für das zurückgegebene Video angeben, z. B. das Format des Videos.

Rufen Sie [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.) auf, und geben Sie [**Video**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) an, um festzulegen, dass ein Video aufgenommen werden soll. Die Methode gibt eine [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Instanz mit dem Video zurück, wenn die Aufnahme erfolgreich ist. Wenn der Benutzer die Aufnahme abbricht, ist das zurückgegebene Objekt „null“.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Wofür Sie die aufgenommene Videodatei verwenden, ist von dem Szenario für Ihre App abhängig. Im weiteren Verlauf dieses Artikels wird gezeigt, wie Sie schnell eine Medienkomposition aus einem oder mehreren aufgenommenen Videos erstellen und diese in der Benutzeroberfläche anzeigen.

Fügen Sie zunächst der XAML-Seite ein [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelement hinzu, in dem die Videokomposition angezeigt werden soll.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


Erstellen Sie mit der von der Kameraaufnahme-UI zurückgegebenen Videodatei eine neue [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource), indem Sie **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** aufrufen. Rufen Sie zur Wiedergabe des Videos die **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** -Methode aus dem Standard- **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** auf, der dem **MediaPlayerElement** zugeordnet ist.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




