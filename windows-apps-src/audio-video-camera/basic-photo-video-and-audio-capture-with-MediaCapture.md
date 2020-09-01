---
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: In diesem Artikel wird beschrieben, wie Sie Fotos und Videos mit der MediaCapture-Klasse am einfachsten aufnehmen.
title: Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2a1bdb033d9c0d47973c26b28dc357a4000d4099
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161124"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“


In diesem Artikel wird die einfachste Möglichkeit zum Erfassen von Fotos und Videos mithilfe der [**mediacapture**](/uwp/api/Windows.Media.Capture.MediaCapture) -Klasse veranschaulicht. Die **MediaCapture**-Klasse stellt einen leistungsfähigen Satz von APIs bereit, der eine Steuerung der Aufnahmepipeline auf unterster Ebene sowie fortgeschrittene Aufnahmeszenarien ermöglicht. Dieser Artikel soll Ihnen jedoch helfen, Ihre App schnell und einfach durch grundlegende Medienaufnahmefunktionen zu erweitern. Informationen zu den von **MediaCapture** bereitgestellten Features finden Sie unter [**Kamera**](camera.md).

Wenn Sie einfach ein Foto oder Video aufnehmen möchten, ohne weitere Features für die Medienaufnahme hinzuzufügen, oder wenn Sie keine eigene Kamera-UI erstellen möchten, können Sie die [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse verwenden. So können Sie einfach die in Windows integrierte Kamera-App starten, um die bereits aufgenommene Foto- oder Videodatei zu empfangen. Weitere Informationen finden Sie unter [**Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI**](capture-photos-and-video-with-cameracaptureui.md)

Der Code in diesem Artikel wurde aus dem [**Camera Starter Kit**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit) übernommen und angepasst. Sie können das Beispiel herunterladen, um den verwendeten Code im Kontext anzuzeigen oder das Beispiel als Ausgangspunkt für Ihre eigene App zu verwenden.

## <a name="add-capability-declarations-to-the-app-manifest"></a>Hinzufügen von Funktionsdeklarationen zum App-Manifest

Damit Ihre App auf die Kamera eines Geräts zugreifen kann, müssen Sie die Verwendung der *webcam*- und *microphone*-Gerätefunktionen durch Ihre App deklarieren. Wenn Sie aufgenommene Fotos und Videos in der Bild- oder Videobibliothek des Benutzers speichern möchten, müssen Sie auch die Funktionen *picturesLibrary* und *videosLibrary* deklarieren.

**So fügen Sie dem App-Manifest Funktionen hinzu**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie das Kontrollkästchen für die **Webcam** und das Kontrollkästchen für **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.


## <a name="initialize-the-mediacapture-object"></a>Initialisieren des MediaCapture-Objekts
Alle in diesem Artikel beschriebenen Aufnahmemethoden erfordern als ersten Schritt die Initialisierung des [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture)-Objekts. Dazu wird erst der Konstruktor und dann [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) aufgerufen. Da der Zugriff auf das **MediaCapture**-Objekt von mehreren Stellen in der App erfolgt, deklarieren Sie eine Klassenvariable als Container für das Objekt.  Implementieren Sie einen Handler für das [**Failed**](/uwp/api/windows.media.capture.mediacapture.failed)-Ereignis des **MediaCapture**-Objekts, damit Sie bei einem fehlerhaften Aufnahmevorgang benachrichtigt werden.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>Einrichten der Kameravorschau
Fotos, Videos und Audiodateien können auch mit **MediaCapture** aufgezeichnet werden, ohne die Kameravorschau anzuzeigen. Normalerweise ist der Vorschaudatenstrom jedoch erwünscht, damit der Benutzer sehen kann, was er aufzeichnet. Zum Aktivieren einiger **MediaCapture**-Features wie Autofokus, automatische Belichtung und automatischer Weißabgleich ist es außerdem erforderlich, dass der Vorschaudatenstrom aktiv ist. Informationen zum Einrichten der Kameravorschau finden Sie unter [**Anzeigen der Kameravorschau**](simple-camera-preview-access.md).

## <a name="capture-a-photo-to-a-softwarebitmap"></a>Aufnehmen eines Fotos in „SoftwareBitmap“
Die [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Klasse wurde in Windows 10 eingeführt, um über mehrere Features hinweg eine einheitliche Bilddarstellung zu gewährleisten. Wenn Sie ein Foto aufzeichnen und dieses dann direkt in Ihrer App verwenden (z. B. in XAML anzeigen) möchten, sollte Sie es in **SoftwareBitmap** statt in einer Datei aufzeichnen. Sie können das Bild später immer noch auf einem Datenträger speichern.

Nachdem Sie das **MediaCapture**-Objekt initialisiert haben, können Sie ein Foto mithilfe der [**LowLagPhotoCapture**](/uwp/api/Windows.Media.Capture.LowLagPhotoCapture)-Klasse in **SoftwareBitmap** aufzeichnen. Rufen Sie eine Instanz dieser Klasse ab, indem Sie [**PrepareLowLagPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync) aufrufen und ein [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt übergeben, das das zu verwendende Bildformat angibt. Mit [**CreateUncompressed**](/uwp/api/windows.media.mediaproperties.imageencodingproperties.createuncompressed) wird eine nicht komprimierte Codierung mit dem angegebenen Pixelformat erstellt. Nehmen Sie ein Foto durch Aufrufen von [**CaptureAsync**](/uwp/api/windows.media.capture.lowlagphotocapture.captureasync) auf. Dadurch wird ein [**CapturedPhoto**](/uwp/api/Windows.Media.Capture.CapturedPhoto)-Objekt zurückgegeben. Rufen Sie **SoftwareBitmap** ab, indem Sie erst auf die [**Frame**](/uwp/api/windows.media.capture.capturedphoto.frame)-Eigenschaft und dann auf die [**SoftwareBitmap**](/uwp/api/windows.media.capture.capturedframe.softwarebitmap)-Eigenschaft zugreifen.

Sie können auch mehrere Fotos erfassen, indem Sie **CaptureAsync** wiederholt aufrufen. Nachdem die Aufzeichnung abgeschlossen ist, rufen Sie [**FinishAsync**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) auf, um die **LowLagPhotoCapture**-Sitzung zu schließen und die zugeordneten Ressourcen freizugeben. Wenn Sie nach dem Aufrufen von **FinishAsync** mit dem Aufzeichnen von Fotos beginnen möchten, müssen Sie [**PrepareLowLagPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync) erneut aufrufen, um die Aufzeichnungssitzung erneut zu initialisieren, bevor Sie [**CaptureAsync**](/uwp/api/windows.media.capture.lowlagphotocapture.captureasync) aufrufen.

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

Ab Windows, Version 1803, können Sie auf die [**bitmapproperties**](/uwp/api/windows.media.capture.capturedframe.bitmapproperties) -Eigenschaft der **capturedframe** -Klasse zugreifen, die von **captureasync** zurückgegeben wurde, um Metadaten über das erfasste Foto abzurufen. Sie können diese Daten an einen **bitmapcoder** übergeben, um die Metadaten in einer Datei zu speichern. Bisher gab es keine Möglichkeit, auf diese Daten für nicht komprimierte Bildformate zuzugreifen. Sie können auch auf die [**controlvalues**](/uwp/api/windows.media.capture.capturedframe.controlvalues) -Eigenschaft zugreifen, um ein [**capturedframecontrolvalues**](/uwp/api/windows.media.capture.capturedframecontrolvalues) -Objekt abzurufen, das die Steuerelement Werte (z. b. "verfügbar" und "weiß Balance") für den aufgezeichneten Frame beschreibt.

Informationen zur Verwendung von **bitmapcoder** und zum Arbeiten mit dem **softwaremulmap** -Objekt, einschließlich der Anzeige eines auf einer XAML-Seite, finden Sie unter [**erstellen, bearbeiten und Speichern von Bitmapbildern**](imaging.md). 

Weitere Informationen zum Festlegen von Geräte Steuerungs Werten finden Sie unter [Erfassen von Geräte Steuerelementen für Fotos und Videos](capture-device-controls-for-photo-and-video-capture.md).

Ab Windows 10, Version 1803, können Sie die Metadaten (z. b. EXIF-Informationen) für Fotos erhalten, die im unkomprimierten Format aufgezeichnet wurden, indem Sie auf die [**bitmapproperties**](/uwp/api/windows.media.capture.capturedframe.bitmapproperties) -Eigenschaft des **capturedframes** zugreifen, das von **mediacapture**zurückgegeben wurde. In früheren Versionen waren diese Daten nur in der Kopfzeile von Fotos verfügbar, die in einem komprimierten Dateiformat aufgezeichnet wurden. Sie können diese Daten für einen [**bitmapcoder**](/uwp/api/windows.graphics.imaging.bitmapencoder) bereitstellen, wenn Sie eine Bilddatei manuell schreiben. Weitere Informationen zum Codieren von Bitmaps finden Sie unter [erstellen, bearbeiten und Speichern von Bitmap-Bildern](imaging.md).  Sie können auch auf die Frame-Steuerelement Werte zugreifen, wie z. b. "verfügbar" und "Flash Einstellungen", die verwendet werden, wenn das Bild durch Zugriff auf die [**controlvalues**](/uwp/api/windows.media.capture.capturedframe.controlvalues) Weitere Informationen finden Sie unter [Erfassen von Geräte Steuerelementen für Foto-und Videoaufnahmen](capture-device-controls-for-photo-and-video-capture.md).

## <a name="capture-a-photo-to-a-file"></a>Aufnehmen eines Fotos in einer Datei
Bei einer normalen Foto-App wird ein aufgenommenes Foto auf einem Datenträger oder in der Cloud gespeichert. Dabei müssen der Datei Metadaten hinzugefügt werden, z. B. die Ausrichtung des Fotos. Das folgende Beispiel zeigt, wie Sie ein Foto in einer Datei aufnehmen. Sie können später immer noch ein **SoftwareBitmap**-Objekt aus der Bilddatei erstellen. 

Bei dem in diesem Beispiel dargestellten Verfahren wird das Foto in einen In-Memory-Datenstrom aufgenommen und dann vom Datenstrom in eine Datei auf einem Datenträger codiert. Im Beispiel wird [**GetLibraryAsync**](/uwp/api/windows.storage.storagelibrary.getlibraryasync) verwendet, um die Bildbibliothek des Benutzers abzurufen, und dann die [**SaveFolder**](/uwp/api/windows.storage.storagelibrary.savefolder)-Eigenschaft, um einen Referenz-Standardspeicherordner abzurufen. Denken Sie daran, Ihrem App-Manifest die Funktion **Bildbibliothek** hinzuzufügen, um auf diesen Ordner zugreifen zu können. [Mit **CreateFileAsync**](/uwp/api/windows.storage.storagefolder.createfileasync) wird eine neue [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) erstellt, in der das Foto gespeichert wird.

Erstellen Sie eine [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream)-Klasse, und rufen Sie [**CapturePhotoToStreamAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) auf, um ein Foto in den Datenstrom aufzunehmen. Dabei werden der Datenstrom und ein [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties)-Objekt übergeben, in dem das zu verwendende Bildformat angegeben ist. Sie können benutzerdefinierte Codierungseigenschaften erstellen, indem Sie das Objekt selbst initialisieren. Die Klasse stellt jedoch statische Methoden wie [**ImageEncodingProperties.CreateJpeg**](/uwp/api/windows.media.mediaproperties.imageencodingproperties.createjpeg) für allgemeine Codierungsformate bereit. Als Nächstes erstellen Sie einen Dateidatenstrom zur Ausgabedatei, indem Sie [**OpenAsync**](/uwp/api/windows.storage.storagefile.openasync) aufrufen. Erstellen Sie [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder), um das Bild aus dem In-Memory-Datenstrom zu decodieren, und erstellen Sie dann [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder), um das Bild durch Aufrufen von [**CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) in eine Datei zu codieren.

Optional können Sie ein [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet)-Objekt erstellen und dann [**SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) für den Bildcodierer aufrufen, um die Fotometadaten in die Bilddatei aufzunehmen. Weitere Informationen über Codierungseigenschaften finden Sie unter [**Bildmetadaten**](image-metadata.md). Bei den meisten Foto-Apps kommt es darauf an, dass die Geräteausrichtung richtig behandelt wird. Weitere Informationen finden Sie unter [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).

Rufen Sie zuletzt [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) für das Codiererobjekt auf, um das Foto vom In-Memory-Datenstrom in die Datei zu transcodieren.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

Weitere Informationen zum Arbeiten mit Dateien und Ordnern finden Sie unter [**Dateien, Ordner und Bibliotheken**](../files/index.md).

## <a name="capture-a-video"></a>Aufnehmen eines Videos
Mithilfe der [**LowLagMediaRecording**](/uwp/api/Windows.Media.Capture.LowLagMediaRecording)-Klasse fügen Sie Ihrer App schnell eine Videoaufnahme hinzu. Deklarieren Sie zuerst eine Klassenvariable für das Objekt.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

Erstellen Sie als Nächstes ein **StorageFile**-Objekt, in dem das Video gespeichert wird. Um die Videobibliothek des Benutzers wie im Beispiel gezeigt zu speichern, müssen Sie Ihrem App-Manifest die Funktion **Videobibliothek** hinzufügen. Rufen Sie [**PrepareLowLagRecordToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) auf, um die Medienaufzeichnung zu initialisieren. Dabei übergeben Sie die Speicherdatei und ein [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)-Objekt, in dem die Codierung für das Video angegeben wird. Von der Klasse werden statische Methoden wie [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) zum Erstellen allgemeiner Videocodierungsprofile bereitgestellt.

Rufen Sie zuletzt [**StartAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.startasync) auf, um mit der Videoaufnahme zu beginnen.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

Zum Beenden der Videoaufnahme rufen Sie [**StopAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopasync) auf.

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Sie können **StartAsync** und **StopAsync** weiterhin aufrufen, um zusätzliche Videos aufzunehmen. Nachdem die Videoaufnahme abgeschlossen ist, rufen Sie [**FinishAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) auf, um die Aufzeichnungssitzung zu löschen und die zugehörigen Ressourcen zu bereinigen. Nach diesem Aufruf müssen Sie **PrepareLowLagRecordToStorageFileAsync** erneut aufrufen, um die Aufzeichnungssitzung vor dem Aufrufen von **StartAsync** erneut zu initialisieren.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

Bei der Videoaufnahme sollten Sie einen Handler für das [**RecordLimitationExceeded**](/uwp/api/windows.media.capture.mediacapture.recordlimitationexceeded)-Ereignis des **MediaCapture**-Objekts registrieren. Dieses Ereignis wird vom Betriebssystem ausgelöst, wenn Sie den Grenzwert für eine einzelne Aufnahme (derzeit drei Stunden) überschreiten. Schließen Sie die Aufnahme im Ereignishandler ab, indem Sie [**StopAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopasync) aufrufen.

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

### <a name="play-and-edit-captured-video-files"></a>Wiedergabe und Bearbeitung erfasster Videodateien
Wenn Sie ein Video in einer Datei aufgezeichnet haben, möchten Sie die Datei möglicherweise laden und in der Benutzeroberfläche Ihrer APP wiedergeben. Hierfür können Sie das XAML-Steuer **[Element mediaplayerelement](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** und einen zugeordneten **[Media Player](/uwp/api/windows.media.playback.mediaplayer)** verwenden. Informationen zum Wiedergeben von Medien auf einer XAML-Seite finden Sie unter wieder [Gabe von Audiodateien und Videos mit Media Player](play-audio-and-video-with-mediaplayer.md).

Sie können auch ein **[Mediaclip](/uwp/api/windows.media.editing.mediaclip)** -Objekt aus einer Videodatei erstellen, indem Sie **[createfromfileasync](/uwp/api/windows.media.editing.mediaclip.createfromfileasync)** aufrufen.  Eine **[mediacomposition](/uwp/api/windows.media.editing.mediacomposition)** bietet grundlegende Funktionen für die Videobearbeitung wie das Anordnen der Sequenz von **Mediaclip** -Objekten, das kürzen der Videolänge, das Erstellen von Ebenen, das Hinzufügen von Hintergrundmusik und das Anwenden von Weitere Informationen zum Arbeiten mit Medien Kompositionen finden Sie unter [Medien Komposition und Bearbeitung](media-compositions-and-editing.md).

## <a name="pause-and-resume-video-recording"></a>Anhalten und Fortsetzen der Videoaufnahme
Sie können eine Videoaufnahme anhalten und dann ohne Erstellung einer separaten Ausgabedatei fortsetzen, indem Sie [**PauseAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.pauseasync) und dann [**ResumeAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.resumeasync) aufrufen.

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

Ab Windows 10, Version 1607, können Sie eine Videoaufnahme anhalten und den letzten Frame empfangen, der vor dem Anhalten der Aufnahme erfasst wurde. Anschließend können Sie diesen Frame in der Kameravorschau überlagern, damit der Benutzer die Kamera auf den angehaltenen Frame ausrichten kann, bevor die Aufnahme fortgesetzt wird. Durch Aufruf von [**PauseWithResultAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.pausewithresultasync) wird ein [**MediaCapturePauseResult**](/uwp/api/Windows.Media.Capture.MediaCapturePauseResult)-Objekt zurückgegeben. Die [**LastFrame**](/uwp/api/windows.media.capture.mediacapturepauseresult.lastframe)-Eigenschaft ist ein [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame)-Objekt, das den letzten Frame darstellt. Um den Frame in XAML anzuzeigen, rufen Sie die **SoftwareBitmap**-Darstellung des Videoframes ab. Derzeit werden nur Bilder im BGRA8-Format mit prämultipliziertem oder leerem Alphakanal unterstützt. Zum Abrufen des richtigen Formats müssen Sie deshalb [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) aufrufen.  Erstellen Sie ein neues [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Objekt, und rufen Sie [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) auf, um es zu initialisieren. Legen Sie zuletzt die **Source**-Eigenschaft des XAML-Steuerelements [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) fest, um das Bild anzuzeigen. Damit dies funktioniert, muss Ihr Bild auf das **CaptureElement**-Steuerelement ausgerichtet sein und einen Transparenzwert kleiner als 1 aufweisen. Da Sie die Benutzeroberfläche nur im UI-Thread ändern können, müssen Sie den Aufruf in [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) ausführen.

Durch **PauseWithResultAsync** wird auch die Dauer der Videoaufnahme im vorangehenden Segment zurückgegeben, falls Sie die Gesamtaufnahmezeit nachverfolgen müssen.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

Wenn Sie die Aufnahme fortsetzen, können Sie die Quelle des Bilds auf NULL festlegen, um es auszublenden.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

Sie können beim Beenden des Videos auch einen Ergebnisframe abrufen, indem Sie [**StopWithResultAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopwithresultasync) aufrufen.


## <a name="capture-audio"></a>Audioaufnahme 
Sie können Ihrer App schnell eine Audioaufnahme hinzufügen, indem Sie die oben bei der Videoaufnahme beschriebene Vorgehensweise verwenden. Im folgenden Beispiel wird eine **StorageFile** im Anwendungsdatenordner erstellt. Rufen Sie [**PrepareLowLagRecordToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) auf, um die Aufzeichnungssitzung zu initialisieren. Dabei werden die Datei und eine [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)-Klasse übergeben, die im Beispiel von der statischen Methode [**CreateMp3**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3) generiert wird. Um mit der Aufnahme zu beginnen, rufen Sie [**StartAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.startasync) auf.

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


Rufen Sie [**StopAsync**](/uwp/api/windows.media.capture.lowlagphotosequencecapture.stopasync) auf, um die Audioaufnahme zu beenden.

## <a name="related-topics"></a>Zugehörige Themen

* [Kamera](camera.md)  
[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Sie können **StartAsync** und **StopAsync** mehrfach aufrufen, um mehrere Audiodateien aufzuzeichnen. Nachdem die Audioaufnahme abgeschlossen ist, rufen Sie [**FinishAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) auf, um die Aufzeichnungssitzung zu löschen und die zugehörigen Ressourcen zu bereinigen. Nach diesem Aufruf müssen Sie **PrepareLowLagRecordToStorageFileAsync** erneut aufrufen, um die Aufzeichnungssitzung vor dem Aufrufen von **StartAsync** erneut zu initialisieren.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Erkennen und reagieren auf audioleveländerungen durch das System
Ab Windows 10, Version 1803, kann Ihre APP erkennen, wenn das System die Audioebene der Audioerfassung und audiorendering-Streams Ihrer APP senkt oder abtrauert. Beispielsweise kann das System die Datenströme Ihrer APP stumm schalten, wenn Sie in den Hintergrund wechselt. Die [**audiostatuemonitor**](/uwp/api/windows.media.audio.audiostatemonitor) -Klasse ermöglicht es Ihnen, sich für den Empfang eines Ereignisses zu registrieren, wenn das System das Volume eines Audiodatenstroms ändert. Rufen Sie eine Instanz von **audiostatuemonitor** zum Überwachen von audioerfassungs-Streams durch Aufrufen von [**createforcapturemonitoring**](/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring)ab. Rufen Sie mithilfe von [**createforrendermonitoring**](/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring)eine Instanz zum Überwachen von audiorenderingstreams ab. Registrieren Sie einen Handler für das [**Sound levelchanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) -Ereignis jedes Monitors, um benachrichtigt zu werden, wenn die Audiodaten für die entsprechende streamkategorie vom System geändert werden.

[!code-cs[AudioStateMonitorUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateMonitorUsing)]

[!code-cs[AudioStateVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[RegisterAudioStateMonitor](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

Im **soundlevelchanged** -Handler für den Erfassungsdaten Strom können Sie die Eigenschaft " [**Sound Level**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) " des Absenders " **Audiostatus-itor** " überprüfen, um die neue Audioebene zu bestimmen. Beachten Sie, dass ein Erfassungsdaten Strom niemals vom System herabgesetzt oder "abgedockt" werden sollte. Es sollte immer nur stumm geschaltet oder auf das vollständige Volume umgestellt werden. Wenn der Audiodatenstrom stumm geschaltet wird, können Sie eine erfasste Erfassung abbrechen. Wenn der Audiodatenstrom auf dem vollen Volume wieder hergestellt wird, können Sie die Erfassung erneut starten. Im folgenden Beispiel werden einige boolesche Klassen Variablen verwendet, um zu verfolgen, ob die APP derzeit Audiodaten erfasst und ob die Erfassung aufgrund der audiostatusänderung beendet wurde. Diese Variablen werden verwendet, um zu bestimmen, wann es sinnvoll ist, die Audioerfassung Programm gesteuert zu beenden oder zu starten.

[!code-cs[CaptureSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureSoundLevelChanged)]

Im folgenden Codebeispiel wird eine Implementierung des **soundlevelchanged** -Handlers für das audiorendering veranschaulicht. Abhängig von Ihrem App-Szenario und der Art des verwendeten Inhalts können Sie die Audiowiedergabe anhalten, wenn die Audioebene geduckt ist. Weitere Informationen zum Verarbeiten von Sound Ebenen für die Medienwiedergabe finden Sie unter [Abspielen von Audio-und Videoinhalten mit Media Player](play-audio-and-video-with-mediaplayer.md).

[!code-cs[RenderSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRenderSoundLevelChanged)]


* [Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI](capture-photos-and-video-with-cameracaptureui.md)
* [Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“](handle-device-orientation-with-mediacapture.md)
* [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md)
* [Dateien, Ordner und Bibliotheken](../files/index.md)