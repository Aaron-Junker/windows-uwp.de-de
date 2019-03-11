---
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: In diesem Artikel wird beschrieben, wie Sie Fotos und Videos mit der MediaCapture-Klasse am einfachsten aufnehmen.
title: Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d0aa227a47fc3940f3fa5c3010eed70f37067935
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618395"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“


In diesem Artikel wird beschrieben, wie Sie Fotos und Videos mit der [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)-Klasse am einfachsten aufnehmen. Die **MediaCapture**-Klasse stellt einen leistungsfähigen Satz von APIs bereit, der eine Steuerung der Aufnahmepipeline auf unterster Ebene sowie fortgeschrittene Aufnahmeszenarien ermöglicht. Dieser Artikel soll Ihnen jedoch helfen, Ihre App schnell und einfach durch grundlegende Medienaufnahmefunktionen zu erweitern. Informationen zu den von **MediaCapture** bereitgestellten Features finden Sie unter [**Kamera**](camera.md).

Wenn Sie einfach ein Foto oder Video aufnehmen möchten, ohne weitere Features für die Medienaufnahme hinzuzufügen, oder wenn Sie keine eigene Kamera-UI erstellen möchten, können Sie die [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI)-Klasse verwenden. So können Sie einfach die in Windows integrierte Kamera-App starten, um die bereits aufgenommene Foto- oder Videodatei zu empfangen. Weitere Informationen finden Sie unter [**Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI**](capture-photos-and-video-with-cameracaptureui.md)

Der Code in diesem Artikel wurde aus dem [**Camera Starter Kit**](https://go.microsoft.com/fwlink/?linkid=619479) übernommen und angepasst. Sie können das Beispiel herunterladen, um den verwendeten Code im Kontext anzuzeigen oder das Beispiel als Ausgangspunkt für Ihre eigene App zu verwenden.

## <a name="add-capability-declarations-to-the-app-manifest"></a>Hinzufügen von Funktionsdeklarationen zum App-Manifest

Damit Ihre App auf die Kamera eines Geräts zugreifen kann, müssen Sie die Verwendung der *webcam*- und *microphone*-Gerätefunktionen durch Ihre App deklarieren. Wenn Sie aufgenommene Fotos und Videos in der Bild- oder Videobibliothek des Benutzers speichern möchten, müssen Sie auch die Funktionen *picturesLibrary* und *videosLibrary* deklarieren.

**Hinzufügen von Funktionen in der app-manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.


## <a name="initialize-the-mediacapture-object"></a>Initialisieren des MediaCapture-Objekts
Alle in diesem Artikel beschriebenen Aufnahmemethoden erfordern als ersten Schritt die Initialisierung des [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)-Objekts. Dazu wird erst der Konstruktor und dann [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync) aufgerufen. Da der Zugriff auf das **MediaCapture**-Objekt von mehreren Stellen in der App erfolgt, deklarieren Sie eine Klassenvariable als Container für das Objekt.  Implementieren Sie einen Handler für das [**Failed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.Failed)-Ereignis des **MediaCapture**-Objekts, damit Sie bei einem fehlerhaften Aufnahmevorgang benachrichtigt werden.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>Einrichten der Kameravorschau
Fotos, Videos und Audiodateien können auch mit **MediaCapture** aufgezeichnet werden, ohne die Kameravorschau anzuzeigen. Normalerweise ist der Vorschaudatenstrom jedoch erwünscht, damit der Benutzer sehen kann, was er aufzeichnet. Zum Aktivieren einiger **MediaCapture**-Features wie Autofokus, automatische Belichtung und automatischer Weißabgleich ist es außerdem erforderlich, dass der Vorschaudatenstrom aktiv ist. Informationen zum Einrichten der Kameravorschau finden Sie unter [**Anzeigen der Kameravorschau**](simple-camera-preview-access.md).

## <a name="capture-a-photo-to-a-softwarebitmap"></a>Aufnehmen eines Fotos in „SoftwareBitmap“
Die [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap)-Klasse wurde in Windows 10 eingeführt, um über mehrere Features hinweg eine einheitliche Bilddarstellung zu gewährleisten. Wenn Sie ein Foto aufzeichnen und dieses dann direkt in Ihrer App verwenden (z. B. in XAML anzeigen) möchten, sollte Sie es in **SoftwareBitmap** statt in einer Datei aufzeichnen. Sie können das Bild später immer noch auf einem Datenträger speichern.

Nachdem Sie das **MediaCapture**-Objekt initialisiert haben, können Sie ein Foto mithilfe der [**LowLagPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture)-Klasse in **SoftwareBitmap** aufzeichnen. Rufen Sie eine Instanz dieser Klasse ab, indem Sie [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync) aufrufen und ein [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt übergeben, das das zu verwendende Bildformat angibt. [**CreateUncompressed** ](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateUncompressed) erstellt eine nicht komprimierte Codierung mit der angegebenen Pixelformats. Nehmen Sie ein Foto durch Aufrufen von [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync) auf. Dadurch wird ein [**CapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto)-Objekt zurückgegeben. Rufen Sie **SoftwareBitmap** ab, indem Sie erst auf die [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto.Frame)-Eigenschaft und dann auf die [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap)-Eigenschaft zugreifen.

Sie können auch mehrere Fotos erfassen, indem Sie **CaptureAsync** wiederholt aufrufen. Nachdem die Aufzeichnung abgeschlossen ist, rufen Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) auf, um die **LowLagPhotoCapture**-Sitzung zu schließen und die zugeordneten Ressourcen freizugeben. Wenn Sie nach dem Aufrufen von **FinishAsync** mit dem Aufzeichnen von Fotos beginnen möchten, müssen Sie [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync) erneut aufrufen, um die Aufzeichnungssitzung erneut zu initialisieren, bevor Sie [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync) aufrufen.

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

Beginnend mit Windows, Version 1803, können Sie auf die [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties)-Eigenschaft der **CapturedFrame** -Klasse zugreifen, die von **CaptureAsync** zum Abrufen von Metadaten zu den erfassten Fotos zurückgegeben wird. Sie können diese Daten an einen **BitmapEncoder** übergeben, um die Metadaten in einer Datei zu speichern. Früher gab es keine Möglichkeit, auf diese Daten für nicht komprimierte Bildformate zugreifen. Sie können auch auf die [**ControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.controlvalues)-Eigenschaft zum Abrufen eines [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframecontrolvalues)-Objekts zugreifen, das das Steuerelement wie z. B. die Belichtung und den Weißabgleich der aufgenommenen Frame beschreibt.

Weitere Informationen zum Arbeiten mit dem **BitmapEncoder** und dem **SoftwareBitmap**-Objekt, z. B. wie Sie ein Objekt in einer XAML-Seite anzeigen, finden Sie unter [**Erstellen, Bearbeiten und Speichern von Bitmapbildern**](imaging.md). 

Weitere Informationen zum Festlegen der Steurelementwerte des Aufnahmegeräts finden Sie unter [Steuerelemente des Aufnahmegeräts für Foto- und Videoaufnahmen](capture-device-controls-for-photo-and-video-capture.md).

Ab Windows 10, Version 1803, erhalten Sie die Metadaten wie z. B. EXIF-Informationen für Fotos, die nicht komprimiert erfasst wurden, durch den Zugriff auf die [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties)-Eigenschaft von der **CapturedFrame**, die von **MediaCapture** zurückgegeben wird. In früheren Versionen waren diese Daten nur in der Kopfzeile des Fotos in einem komprimierten Dateiformat erhältlich. Sie können diese Daten für den [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder) bereitstellen, wenn Sie sie manuell in eine Bilddatei schreiben. Weitere Informationen zur Kodierung von Bitmaps finden Sie im Artikel [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md).  Sie können auch auf die Werte des Frame-Steuerelements zugreifen, z. B. die Belichtung und Flash-Einstellungen, die verwendet wurden, als das Image erfasst wurde, indem Sie auf die [**ControlValues**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.capturedframe.controlvalues)-Eigenschaft zugreifen. Weitere Informationen finden Sie unter [Steuerelemente des Aufnahmegeräts für Foto- und Videoaufnahmen](capture-device-controls-for-photo-and-video-capture.md).

## <a name="capture-a-photo-to-a-file"></a>Aufnehmen eines Fotos in einer Datei
Bei einer normalen Foto-App wird ein aufgenommenes Foto auf einem Datenträger oder in der Cloud gespeichert. Dabei müssen der Datei Metadaten hinzugefügt werden, z. B. die Ausrichtung des Fotos. Das folgende Beispiel zeigt, wie Sie ein Foto in einer Datei aufnehmen. Sie können später immer noch ein **SoftwareBitmap**-Objekt aus der Bilddatei erstellen. 

Bei dem in diesem Beispiel dargestellten Verfahren wird das Foto in einen In-Memory-Datenstrom aufgenommen und dann vom Datenstrom in eine Datei auf einem Datenträger codiert. Im Beispiel wird [**GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.GetLibraryAsync) verwendet, um die Bildbibliothek des Benutzers abzurufen, und dann die [**SaveFolder**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.SaveFolder)-Eigenschaft, um einen Referenz-Standardspeicherordner abzurufen. Denken Sie daran, Ihrem App-Manifest die Funktion **Bildbibliothek** hinzuzufügen, um auf diesen Ordner zugreifen zu können. [**CreateFileAsync** ](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFolder.CreateFileAsync) erstellt ein neues [ **"storagefile"** ](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile) , in dem gespeichert werden, des Fotos wird.

Erstellen Sie eine [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream)-Klasse, und rufen Sie [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) auf, um ein Foto in den Datenstrom aufzunehmen. Dabei werden der Datenstrom und ein [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt übergeben, in dem das zu verwendende Bildformat angegeben ist. Sie können benutzerdefinierte Codierungseigenschaften erstellen, indem Sie das Objekt selbst initialisieren. Die Klasse stellt jedoch statische Methoden wie [**ImageEncodingProperties.CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateJpeg) für allgemeine Codierungsformate bereit. Als Nächstes erstellen Sie einen Dateidatenstrom zur Ausgabedatei, indem Sie [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile.OpenAsync) aufrufen. Erstellen Sie [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder), um das Bild aus dem In-Memory-Datenstrom zu decodieren, und erstellen Sie dann [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder), um das Bild durch Aufrufen von [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.CreateForTranscodingAsync) in eine Datei zu codieren.

Optional können Sie ein [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapPropertySet)-Objekt erstellen und dann [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252.aspx) für den Bildcodierer aufrufen, um die Fotometadaten in die Bilddatei aufzunehmen. Weitere Informationen über Codierungseigenschaften finden Sie unter [**Bildmetadaten**](image-metadata.md). Bei den meisten Foto-Apps kommt es darauf an, dass die Geräteausrichtung richtig behandelt wird. Weitere Informationen finden Sie unter [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Rufen Sie zuletzt [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) für das Codiererobjekt auf, um das Foto vom In-Memory-Datenstrom in die Datei zu transcodieren.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

Weitere Informationen zum Arbeiten mit Dateien und Ordnern finden Sie unter [**Dateien, Ordner und Bibliotheken**](https://msdn.microsoft.com/windows/uwp/files/index).

## <a name="capture-a-video"></a>Aufnehmen eines Videos
Mithilfe der [**LowLagMediaRecording**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording)-Klasse fügen Sie Ihrer App schnell eine Videoaufnahme hinzu. Deklarieren Sie zuerst eine Klassenvariable für das Objekt.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

Erstellen Sie als Nächstes ein **StorageFile**-Objekt, in dem das Video gespeichert wird. Um die Videobibliothek des Benutzers wie im Beispiel gezeigt zu speichern, müssen Sie Ihrem App-Manifest die Funktion **Videobibliothek** hinzufügen. Rufen Sie [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) auf, um die Medienaufzeichnung zu initialisieren. Dabei übergeben Sie die Speicherdatei und ein [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile)-Objekt, in dem die Codierung für das Video angegeben wird. Von der Klasse werden statische Methoden wie [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4) zum Erstellen allgemeiner Videocodierungsprofile bereitgestellt.

Rufen Sie zuletzt [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync) auf, um mit der Videoaufnahme zu beginnen.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

Zum Beenden der Videoaufnahme rufen Sie [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync) auf.

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Sie können **StartAsync** und **StopAsync** weiterhin aufrufen, um zusätzliche Videos aufzunehmen. Nachdem die Videoaufnahme abgeschlossen ist, rufen Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) auf, um die Aufzeichnungssitzung zu löschen und die zugehörigen Ressourcen zu bereinigen. Nach diesem Aufruf müssen Sie **PrepareLowLagRecordToStorageFileAsync** erneut aufrufen, um die Aufzeichnungssitzung vor dem Aufrufen von **StartAsync** erneut zu initialisieren.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

Bei der Videoaufnahme sollten Sie einen Handler für das [**RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.RecordLimitationExceeded)-Ereignis des **MediaCapture**-Objekts registrieren. Dieses Ereignis wird vom Betriebssystem ausgelöst, wenn Sie den Grenzwert für eine einzelne Aufnahme (derzeit drei Stunden) überschreiten. Schließen Sie die Aufnahme im Ereignishandler ab, indem Sie [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync) aufrufen.

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

### <a name="play-and-edit-captured-video-files"></a>Wiedergeben und Bearbeiten von aufgenommenen Videodateien
Nachdem Sie ein Video in einer Datei aufgezeichnet haben, empfiehlt es sich, die Datei hochzuladen und in der Benutzeroberfläche Ihrer App wiederzugeben. Sie können dazu das **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** XAML-Steuerelement und einen zugeordneten **[MediaPlayer ](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** verwenden. Weitere Informationen zur Wiedergabe von Medien in einer XAML-Seite finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit MediaPlayer.](play-audio-and-video-with-mediaplayer.md)

Sie können auch ein **[MediaClip](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip)**-Objekt aus einer Videodatei durch Aufrufen von **[Mediaclip ](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync)** erstellen.  Die **[MediaComposition](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition)** bietet grundlegende Videobearbeitungs-Funktionen wie das Anordnen der **MediaClip**-Objekte, Zuschneiden von Videos, Erstellen von Ebenen, Hinzufügen von Musik im Hintergrund und Anwenden von Videoeffekten. Weitere Informationen zu Medienkompositionen finden Sie unter [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md).

## <a name="pause-and-resume-video-recording"></a>Anhalten und Fortsetzen der Videoaufnahme
Sie können eine Videoaufnahme anhalten und dann ohne Erstellung einer separaten Ausgabedatei fortsetzen, indem Sie [**PauseAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseAsync) und dann [**ResumeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.ResumeAsync) aufrufen.

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

Ab Windows 10, Version 1607, können Sie eine Videoaufnahme anhalten und den letzten Frame empfangen, der vor dem Anhalten der Aufnahme erfasst wurde. Anschließend können Sie diesen Frame in der Kameravorschau überlagern, damit der Benutzer die Kamera auf den angehaltenen Frame ausrichten kann, bevor die Aufnahme fortgesetzt wird. Durch Aufruf von [**PauseWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseWithResultAsync) wird ein [**MediaCapturePauseResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult)-Objekt zurückgegeben. Die [**LastFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult.LastFrame)-Eigenschaft ist ein [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.VideoFrame)-Objekt, das den letzten Frame darstellt. Um den Frame in XAML anzuzeigen, rufen Sie die **SoftwareBitmap**-Darstellung des Videoframes ab. Derzeit werden nur Bilder im BGRA8-Format mit prämultipliziertem oder leerem Alphakanal unterstützt. Zum Abrufen des richtigen Formats müssen Sie deshalb [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) aufrufen.  Erstellen Sie ein neues [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Objekt, und rufen Sie [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource.SetBitmapAsync) auf, um es zu initialisieren. Legen Sie zuletzt die **Source**-Eigenschaft des XAML-Steuerelements [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) fest, um das Bild anzuzeigen. Damit dies funktioniert, muss Ihr Bild auf das **CaptureElement**-Steuerelement ausgerichtet sein und einen Transparenzwert kleiner als 1 aufweisen. Da Sie die Benutzeroberfläche nur im UI-Thread ändern können, müssen Sie den Aufruf in [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync) ausführen.

Durch **PauseWithResultAsync** wird auch die Dauer der Videoaufnahme im vorangehenden Segment zurückgegeben, falls Sie die Gesamtaufnahmezeit nachverfolgen müssen.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

Wenn Sie die Aufnahme fortsetzen, können Sie die Quelle des Bilds auf NULL festlegen, um es auszublenden.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

Sie können beim Beenden des Videos auch einen Ergebnisframe abrufen, indem Sie [**StopWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopWithResultAsync) aufrufen.


## <a name="capture-audio"></a>Audioaufnahme 
Sie können Ihrer App schnell eine Audioaufnahme hinzufügen, indem Sie die oben bei der Videoaufnahme beschriebene Vorgehensweise verwenden. Im folgenden Beispiel wird eine **StorageFile** im Anwendungsdatenordner erstellt. Rufen Sie [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) auf, um die Aufzeichnungssitzung zu initialisieren. Dabei werden die Datei und eine [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile)-Klasse übergeben, die im Beispiel von der statischen Methode [**CreateMp3**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp3) generiert wird. Um mit der Aufnahme zu beginnen, rufen Sie [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync) auf.

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


Rufen Sie [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoSequenceCapture.StopAsync) auf, um die Audioaufnahme zu beenden.

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md) [!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Sie können **StartAsync** und **StopAsync** mehrfach aufrufen, um mehrere Audiodateien aufzuzeichnen. Nachdem die Audioaufnahme abgeschlossen ist, rufen Sie [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) auf, um die Aufzeichnungssitzung zu löschen und die zugehörigen Ressourcen zu bereinigen. Nach diesem Aufruf müssen Sie **PrepareLowLagRecordToStorageFileAsync** erneut aufrufen, um die Aufzeichnungssitzung vor dem Aufrufen von **StartAsync** erneut zu initialisieren.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Erkennen und Reagieren auf Änderungen der Audioebene vom System
Ab Windows 10, Version 1803, erkennt Ihre App, wenn das System die Lautstärke der Audioaufnahme Ihrer App oder die Audiowiedergabe reduziert oder stummschaltet. Beispielsweise kann das System das Streamen der App stumm schalten, wenn es in den Hintergrund wechselt. Mit der [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor)-Klasse können Sie sich registrieren, um ein Ereignis zu erhalten, wenn das System die Lautstärke eines Audiostreams ändert. Rufen Sie eine Instanz des **AudioStateMonitor** für die Überwachung des Audio-Datenstroms durch [**CreateForCaptureMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring) auf. Rufen Sie eine Instanz für die Überwachung von Audiowiedergabestreams durch [**CreateForRenderMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring) auf. Registrieren Sie einen Handler für das [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged)-Ereignis jedes Monitors, um benachrichtigt zu werden, wenn die Audiowiedergabe für die entsprechende Streamkategorie vom System geändert wird.

[!code-cs[AudioStateMonitorUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateMonitorUsing)]

[!code-cs[AudioStateVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[RegisterAudioStateMonitor](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

Im **SoundLevelChanged** Handler für den Aufnahmedatenstrom, können Sie die [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel)-Eigenschaft des **AudioStateMonitor**-Senders überprüfen, um die neue Soundebene zu bestimmen. Beachten Sie, dass ein Aufnahmedatenstrom sollte nie vom System gesenkt oder "gedämpft" werden sollte. Es sollte immer nur stummgeschaltet oder auf volle Lautstärke gewechselt werden. Wenn der Audiodatenstrom stumm geschaltet ist, können Sie eine Aufzeichnung in Bearbeitung beenden. Wenn der Audiodatenstrom auf die volle Lautstärke wiederhergestellt wird, können Sie beginnen, erneut aufzuzeichnen. Das folgende Beispiel verwendet einige Variablen der booleschen Klasse, um nachzuverfolgen, ob die App derzeit Audio aufnimmt und ob die Aufnahme aufgrund der Änderung der Audiostatus beendet wurde. Diese Variablen werden verwendet, um zu bestimmen, wann ist es angemessen ist, die Audio-Aufnahme programmgesteuert zu beenden oder zu starten.

[!code-cs[CaptureSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureSoundLevelChanged)]

Im folgenden Codebeispiel wird eine Implementierung des **SoundLevelChanged** Handlers für die Wiedergabe von Audio veranschaulicht. Je nach App-Szenario, und der Art der Inhalte, die Sie abspielen, sollte die Audiowiedergabe angehalten werden, wenn die Lautstärke gedämpft ist. Weitere Informationen zum Behandeln von Soundänderungen auf Systemebene für die Wiedergabe von Medien finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit MediaPlayer](play-audio-and-video-with-mediaplayer.md).

[!code-cs[RenderSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRenderSoundLevelChanged)]


* [Aufzeichnen von Fotos und Videos mit Windows integrierten Kamera-Benutzeroberfläche](capture-photos-and-video-with-cameracaptureui.md)
* [Geräteausrichtung mit MediaCapture behandeln](handle-device-orientation-with-mediacapture.md)
* [Erstellen, bearbeiten und Speichern von Bitmaps](imaging.md)
* [Dateien, Ordner und Bibliotheken](https://msdn.microsoft.com/windows/uwp/files/index)

