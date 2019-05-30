---
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: In diesem Thema wird das Abrufen eines einzelnen Vorschauframes aus dem Vorschaustream der Medienaufnahme beschrieben.
title: Abrufen eines Vorschauframes
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9963955649b98f226fbb81871b2ac391035ba41a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360884"
---
# <a name="get-a-preview-frame"></a>Abrufen eines Vorschauframes


In diesem Thema wird das Abrufen eines einzelnen Vorschauframes aus dem Vorschaustream der Medienaufnahme beschrieben.

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Voraussetzung für den Code in diesem Artikel ist, dass Ihre App bereits über eine Instanz von MediaCapture verfügt, die ordnungsgemäß initialisiert wurde, und Sie ein [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) mit einem aktiven Videovorschaustream besitzen.

Neben den Namespaces, die für eine einfache Medienaufzeichnung erforderlich sind, erfordert das Aufzeichnen eines Vorschauframes den folgenden Namespace.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

Wenn Sie einen Vorschauframe anfordern, können Sie das Format angeben, in dem Sie den Frame empfangen möchten, indem Sie ein [**VideoFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame)-Objekt im gewünschten Format erstellen. Dieses Beispiel erstellt einen Videoframe mit der gleichen Auflösung wie der Vorschaustream durch Aufrufen von [**VideoDeviceController.GetMediaStreamProperties**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) und Angeben von [**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) zum Anfordern der Eigenschaften für den Vorschaustream. Die Breite und Höhe des Vorschaustreams werden zum Erstellen des neuen Videoframes verwendet.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

Wenn das [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)-Objekt initialisiert ist, und Sie einen aktiven Vorschaustream besitzen, rufen Sie [**GetPreviewFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) auf, um einen Vorschauframe abzurufen. Übergeben Sie den Videoframe, den Sie im letzten Schritt erstellt haben, um das Format des zurückgegebenen Frames anzugeben.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

Rufen Sie eine [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Darstellung des Vorschauframes durch Zugriff auf die [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.softwarebitmap)-Eigenschaft des [**VideoFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame)-Objekts ab. Informationen zum Speichern, Laden und Ändern von Softwarebitmaps finden Sie unter [Imaging](imaging.md).

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Sie können auch eine [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface)-Darstellung des Vorschauframes abrufen, wenn Sie das Bild mit Direct3D-APIs verwenden möchten.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> Die Eigenschaft [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.softwarebitmap) oder die Eigenschaft [**Direct3DSurface**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.direct3dsurface) des zurückgegebenen **VideoFrame** ist möglicherweise Null, abhängig davon, wie Sie **GetPreviewFrameAsync** aufrufen, und von dem Gerät, auf dem Ihre App ausgeführt wird.

> - Wenn Sie die Überladung von [**GetPreviewFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) aufrufen, die ein **VideoFrame**-Argument akzeptiert, dann ist für den zurückgegebenen **VideoFrame** der **SoftwareBitmap**-Wert nicht Null, und die Eigenschaft **Direct3DSurface** ist Null.
> - Wenn Sie auf einem Gerät, das eine Direct3D-Oberfläche verwendet, um den Frame intern darzustellen, die Überladung von [**GetPreviewFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) aufrufen, das keine Argumente besitzt, ist die Eigenschaft **Direct3DSurface** nicht Null und die Eigenschaft **SoftwareBitmap** ist Null.
> - Wenn Sie auf einem Gerät, das keine Direct3D-Oberfläche verwendet, um den Frame intern darzustellen, die Überladung von [**GetPreviewFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) aufrufen, das keine Argumente besitzt, ist die Eigenschaft **SoftwareBitmap** nicht Null und die Eigenschaft **Direct3DSurface** ist Null.

Ihre App sollte stets nach einem Null-Wert suchen, bevor versucht wird, Vorgänge für Objekte auszuführen, die von den Eigenschaften **SoftwareBitmap** oder **Direct3DSurface** zurückgegeben werden.

Wenn Sie den Vorschauframe nicht mehr benötigen, rufen Sie auf jeden Fall die zugehörige [**Close**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.close)-Methode auf (Dispose-Methode in C#), um die vom Frame verwendeten Ressourcen freizugeben. Alternativ können Sie auch das Muster **using** verwenden, durch das das Objekt automatisch entfernt wird.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




