---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: In diesem Artikel wird beschrieben, wie Sie mit der [**cameracaptureui**](/uwp/api/windows.media.capture.cameracaptureui) -Klasse Fotos oder Videos mithilfe der in Windows integrierten Kamera-Benutzeroberfläche erfassen.
title: Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 5b5e1369e37fc683a3a09c8f404b1998ee06bdab
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364003"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI

In diesem Artikel wird beschrieben, wie Sie mit der [**cameracaptureui**](/uwp/api/windows.media.capture.cameracaptureui) -Klasse Fotos oder Videos mithilfe der in Windows integrierten Kamera-Benutzeroberfläche erfassen. Diese Funktion ist einfach zu verwenden. Sie ermöglicht es Ihrer APP, ein Foto oder Video mit nur wenigen Codezeilen zu erhalten.

Wenn Sie Ihre eigene Kamera-Benutzeroberfläche bereitstellen möchten oder wenn Ihr Szenario eine stabilere, auf niedriger Ebene erforderliche Kontrolle über den Aufzeichnungs Vorgang erfordert, sollten Sie die [**mediacapture**](/uwp/api/Windows.Media.Capture.MediaCapture) -Klasse verwenden und ihre eigene Erfassungs Oberfläche implementieren. Weitere Informationen finden Sie unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> Sie sollten die Funktionen " **Webcam** " und " **Mikrofon** " nicht in der APP-Manifest-Datei angeben, wenn Ihre APP nur " **cameracaptureui**" verwendet Wenn Sie dies tun, wird Ihre APP in den Datenschutzeinstellungen des Geräts angezeigt, aber selbst wenn der Benutzer den Kamera Zugriff auf Ihre APP verweigert, verhindert dies, dass " **cameracaptureui** " Medien erfasst. <p>Das liegt daran, dass es sich bei der integrierten Kamera-App von Windows um eine vertrauenswürdige Erstanbieter-App handelt, die erfordert, dass der Benutzer die Foto-, Audio- und Videoaufnahme durch einen Tastendruck initiiert. Bei Verwendung von " **cameracaptureui" bei Verwendung von "cameracaptureui** " als einziger Foto Erfassungs Mechanismus kann bei Ihrer APP ein Fehler bei der Zertifizierung des Zertifizierungs Kits Microsoft Store für Windows-Anwendungen auftreten.<p>
Sie müssen die **Webcam** -oder **Mikrofon** -Funktionen in der APP-Manifest-Datei angeben, wenn Sie **mediacapture** verwenden, um Audioinhalte, Fotos oder Videos Programm gesteuert zu erfassen.

## <a name="capture-a-photo-with-cameracaptureui"></a>Aufnehmen eines Fotos mit „CameraCaptureUI“

Um die Kameraaufnahme-UI zu verwenden, schließen Sie den [**Windows.Media.Capture**](/uwp/api/Windows.Media.Capture)-Namespace in Ihr Projekt ein. Um Dateivorgänge mit der zurückgegebenen Bilddatei auszuführen, schließen Sie [**Windows.Storage**](/uwp/api/Windows.Storage) ein.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingCaptureUI":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingCaptureUI":::

Um ein Foto aufzunehmen, erstellen Sie ein neues [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Objekt. Mithilfe der Eigenschaft " [**photosettings**](/uwp/api/windows.media.capture.cameracaptureui.photosettings) " des Objekts können Sie Eigenschaften für das zurückgegebene Foto angeben, z. b. das Bildformat des Fotos. Standardmäßig unterstützt die Kamera Fassungs Benutzeroberfläche das Ausschneiden des Fotos, bevor es zurückgegeben wird. Dies kann mit der [**allowcropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) -Eigenschaft deaktiviert werden. In diesem Beispiel wird [**CroppedSizeInPixels**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) festgelegt, um anzufordern, dass das zurückgegebene Bild 200 x 200 Pixel hat.

> [!NOTE]
> Das Zuschneiden von Bildern in " **cameracaptureui** " wird für Geräte in der Produktfamilie für mobile Geräte nicht unterstützt. Der Wert der [**AllowCropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping)-Eigenschaft wird ignoriert, wenn Ihre App auf diesen Geräten ausgeführt wird.

Rufen Sie [**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) auf, und geben Sie [**CameraCaptureUIMode.Photo**](/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) an, um festzulegen, dass ein Foto aufgenommen werden soll. Die Methode gibt eine [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Instanz mit dem Bild zurück, wenn die Aufnahme erfolgreich ist. Wenn der Benutzer die Aufnahme abbricht, ist das zurückgegebene Objekt „null“.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCapturePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCapturePhoto":::

Die **StorageFile** mit dem aufgenommenen Foto erhält einen dynamisch generierten Namen und wird im lokalen Ordner der App gespeichert. Um die aufgezeichneten Fotos besser zu organisieren, können Sie die Datei in einen anderen Ordner verschieben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCopyAndDeletePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCopyAndDeletePhoto":::

Zur Verwendung des Fotos in der App kann die Erstellung eines [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekts hilfreich sein, das mit unterschiedlichen Funktionen für universelle Windows-Apps genutzt werden kann.

Fügen Sie zunächst den [**Windows. Graphics. Imaging**](/uwp/api/Windows.Graphics.Imaging) -Namespace in Ihr Projekt ein.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmap":::

Rufen Sie [**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) auf, um einen Stream aus der Bilddatei abzurufen. Rufen Sie [**BitmapDecoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) auf, um einen Bitmap-Decoder für den Stream abzurufen. Anschließend können Sie [**getsoftwarebitmap**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) aufrufen, um eine Darstellung des Bilds in der **Software** zu erhalten.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSoftwareBitmap":::

Um das Bild in der Benutzeroberfläche anzuzeigen, deklarieren Sie ein [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image)-Steuerelement auf der XAML-Seite.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetImageControl":::
:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.xaml" id="SnippetImageControl":::

Um die Software-Bitmap in der XAML-Seite zu verwenden, schließen Sie den [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging)-Namespace in Ihr Projekt ein.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmapSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmapSource":::

Das **Image** -Steuerelement erfordert, dass die Bildquelle das Format BGRA8 mit einem prämultiplizierten Alpha oder ohne Alpha hat. Rufen Sie die statische Methode [**softwarebitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) auf, um eine neue Software Bitmap mit dem gewünschten Format zu erstellen. Erstellen Sie als nächstes ein neues [**softwarebitmapsource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) -Objekt, und nennen Sie es [**setbitmapasync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) , um die Software Bitmap der Quelle zuzuweisen. Legen Sie abschließend die [**Source**](/uwp/api/windows.ui.xaml.controls.image.source)-Eigenschaft des **Image**-Steuerelements fest, um das aufgenommene Foto in der Benutzeroberfläche anzuzeigen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSetImageSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSetImageSource":::

## <a name="capture-a-video-with-cameracaptureui"></a>Aufnehmen eines Videos mit CameraCaptureUI

Um ein Video aufzunehmen, erstellen Sie ein neues [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Objekt. Mithilfe der [**videosettings**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) -Eigenschaft des Objekts können Sie Eigenschaften für das zurückgegebene Video angeben, z. b. das Format des Videos.

Aufrufen von [**capturefileasync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) und Angeben von [**Video**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) zum Erfassen eines Videos. Die Methode gibt eine [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Instanz mit dem Video zurück, wenn die Aufnahme erfolgreich ist. Wenn Sie die Erfassung abbrechen, ist das zurückgegebene Objekt NULL.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCaptureVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCaptureVideo":::

Wofür Sie die aufgenommene Videodatei verwenden, ist von dem Szenario für Ihre App abhängig. Im weiteren Verlauf dieses Artikels wird gezeigt, wie Sie schnell eine Medienkomposition aus einem oder mehreren aufgenommenen Videos erstellen und diese in der Benutzeroberfläche anzeigen.

Fügen Sie zunächst ein [**mediaplayerelement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) -Steuerelement hinzu, in dem die Videokomposition auf der XAML-Seite angezeigt wird.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetMediaElement":::

Wenn die Videodatei von der Kameraaufnahme-Benutzeroberfläche zurückgegeben wird, erstellen Sie eine neue [**MediaSource**](/uwp/api/windows.media.core.mediasource) durch Aufrufen von **[createfromstoragefile](/uwp/api/windows.media.core.mediasource.createfromstoragefile)**. Ruft die **[Play](/uwp/api/windows.media.playback.mediaplayer.Play)** -Methode des **[Media Player](/uwp/api/windows.media.playback.mediaplayer)** -Standard Elements auf, das dem **mediaplayerelement** zugeordnet ist, um das Video wiederzugeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetPlayVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetPlayVideo":::

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI)
