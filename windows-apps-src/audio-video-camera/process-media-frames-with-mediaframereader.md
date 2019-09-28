---
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: In diesem Artikel wird beschrieben, wie Sie mit „MediaFrameReader“ und „MediaCapture“ Medienframes aus einer oder mehreren verfügbaren Quellen abrufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames).
title: Verarbeiten von Medienframes mit „MediaFrameReader“
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ddd35e0365efcc8c224e717b66f53734af32123d
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339752"
---
# <a name="process-media-frames-with-mediaframereader"></a>Verarbeiten von Medienframes mit „MediaFrameReader“

In diesem Artikel wird beschrieben, wie Sie mit [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) und [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) Medienframes aus einer oder mehreren verfügbaren Quellen abrufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames). Dieses Feature wurde für die Verwendung von Apps entworfen, die Medienframes in Echtzeit verarbeiten, wie beispielsweise Augmented-Reality- und Tiefenkamera-Apps.

Wenn Sie normale Videos oder Fotos aufnehmen möchten, wie mit einer typischen Foto-App, sollten Sie möglicherweise eine der anderen Aufnahmemethoden verwenden, die von [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) unterstützt werden. Eine Liste der verfügbaren Methoden zur Medienaufnahme und Artikel zu ihrer Verwendung finden Sie unter [**Kamera**](camera.md).

> [!NOTE] 
> Die in diesem Artikel besprochenen Features sind erst ab Windows 10, Version 1607, verfügbar.

> [!NOTE] 
> Es gibt ein Beispiel für universelle Windows-Apps, in dem die Verwendung von **MediaFrameReader** zum Anzeigen von Frames aus unterschiedlichen Framequellen demonstriert wird, unter anderem Farb-, Tiefen- und Infrarotkameras. Weitere Informationen finden Sie unter [Beispiel für Kameraframes](https://go.microsoft.com/fwlink/?LinkId=823230).

> [!NOTE] 
> Es wurde ein neuer Satz von APIs für die Verwendung von **MediaFrameReader** mit Audiodaten in Windows 10, Version 1803 eingeführt. Weitere Informationen finden Sie unter [Verarbeiten von Audioframes mit MediaFrameReader](process-audio-frames-with-mediaframereader.md).


## <a name="setting-up-your-project"></a>Einrichten Ihres Projekts
Wie bei allen Apps, die **MediaCapture** verwenden, müssen Sie deklarieren, dass Ihre App die *Webcam*-Funktion verwendet. Erst dann können Sie auf Kamerageräte zugreifen. Wenn Ihre App von einem Audiogerät aufzeichnet, müssen Sie auch die *microphone*-Gerätefunktion deklarieren. 

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.

Im Beispielcode in diesem Artikel werden neben den in der Standard-Projektvorlage enthaltenen APIs auch APIs aus den folgenden Namespaces verwendet.

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## <a name="select-frame-sources-and-frame-source-groups"></a>Auswählen von Framequellen und Framequellgruppen
Viele Apps, die Medienframes verarbeiten, müssen Frames aus mehreren Quellen gleichzeitig abrufen, z. B. die Farb- und Tiefenkameras eines Geräts. Das [**mediaframesourcegroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) -Objekt stellt einen Satz von Medien Frame Quellen dar, die gleichzeitig verwendet werden können. Rufen Sie die statische Methode [**MediaFrameSourceGroup.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) auf, um eine Liste aller vom aktuellen Gerät unterstützten Gruppen von Framequellen abzurufen.

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

Sie können auch einen [**devicewatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) mit [**DeviceInformation. devatewatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) erstellen und den Wert, der von [**mediaframesourcegroup. getdeviceselector**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) zurückgegeben wird, um Benachrichtigungen zu empfangen, wenn die verfügbaren Frame Quell Gruppen auf dem Gerät vorhanden sind. Änderungen, z. b. Wenn eine externe Kamera angeschlossen ist. Weitere Informationen finden Sie unter [**Auflisten von Geräten**](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices).

Eine [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) verfügt über eine Sammlung von [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo)-Objekten, die in der Gruppe enthaltene Framequellen beschreiben. Nach dem Abrufen der auf dem Gerät verfügbaren Framequellgruppen können Sie die Gruppe auswählen, die die für Sie relevanten Framequellen verfügbar macht.

Das folgende Beispiel zeigt die einfachste Möglichkeit zum Auswählen einer Framequellgruppe. Vom Code werden dann einfach Schleifen durch alle verfügbaren Gruppen und anschließend durch die einzelnen Elemente in der [**SourceInfos**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.sourceinfos)-Sammlung durchgeführt. Für jede **MediaFrameSourceInfo** wird geprüft, ob sie die gewünschten Funktionen unterstützt. In diesem Fall wird die [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype)-Eigenschaft auf den Wert [**VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) überprüft. D. h. das Gerät stellt einen Videovorschau-Stream bereit, und die [**SourceKind**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.sourcekind)-Eigenschaft wird auf den Wert [**Farbe**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceKind) überprüft, um anzuzeigen, dass die Quelle Farbframes bereitstellt.

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

Diese Methode zur Identifizierung der gewünschten Framequellgruppe und Framequellen ist für einfache Fälle geeignet. Wenn Sie jedoch anhand komplexerer Kriterien Framequellen auswählen möchten, wird dieses Verfahren unpraktisch. Eine weitere Methode ist die Auswahl mithilfe von Linq-Syntax und anonymen Objekten. Im folgenden Beispiel wird die Erweiterungsmethode **Select** verwendet, um die **MediaFrameSourceGroup**-Objekte in der *FrameSourceGroups*-Liste in ein anonymes Objekt mit zwei Feldern umzuwandeln:*sourceGroup*, welches die Gruppe selbst darstellt, und *colorSourceInfo*, welches die Farbframequelle in der Gruppe repräsentiert. Für das Feld *colorSourceInfo* wird das Ergebnis der **FirstOrDefault**-Methode festgelegt, die das erste Objekt auswählt, für dessen bereitgestelltes Prädikat die Auflösung „true“ ist. In diesem Fall ist das Prädikat „true“, wenn der Datenstromtyp **VideoPreview** und die Art der Quelle **Color** ist und sich die Kamera an der Vorderseite des Geräts befindet.

Aus der Liste der von der oben beschriebenen Abfrage zurückgegebenen anonymen Objekte werden mit der **Where**-Erweiterungsmethode nur die Objekte ausgewählt, in denen das Feld *colorSourceInfo* nicht NULL ist. Schließlich wird **FirstOrDefault** aufgerufen, um das erste Element in der Liste auszuwählen.

Nun können Sie mithilfe der Felder des ausgewählten Objekts die Verweise auf das ausgewählte **MediaFrameSourceGroup**- und das **MediaFrameSourceInfo**-Objekt abrufen, die die Farbkamera darstellen. Diese werden später verwendet, um das **MediaCapture**-Objekt zu initialisieren und einen **MediaFrameReader** für die ausgewählte Quelle zu erstellen. Abschließend sollten Sie testen, ob Quellgruppe NULL ist. Dies würde bedeuten, dass das aktuelle Gerät nicht über Ihre angeforderten Aufnahmequellen verfügt.

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

Im folgenden Beispiel wird mit einer ähnlichen Methode wie der oben beschriebenen eine Quellgruppe mit Farb-, Tiefen- und Infrarotkameras ausgewählt.

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

> [!NOTE]
> Ab Windows 10, Version 1803, können Sie die [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile)-Klasse auswählen, um eine Medienframequelle mit den gewünschten Funktionen zu erstellen. Weitere Informationen finden Sie im Abschnitt **Use video profiles to select a frame source** weiter unten in diesem Artikel.


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>Initialisieren des MediaCapture-Objekts zum Verwenden der ausgewählten Framequellgruppe
Im nächsten Schritt wird das **MediaCapture**-Objekt initialisiert, um die im vorherigen Schritt ausgewählte Framequellgruppe zu verwenden.

Das **MediaCapture**-Objekt wird üblicherweise an mehreren Stellen in Ihrer App verwendet. Sie sollten daher eine Klassenmembervariable deklarieren, um es aufzunehmen.

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Erstellen Sie eine Instanz des **MediaCapture**-Objekts, indem Sie den Konstruktor aufrufen. Erstellen Sie als nächstes ein [**mediacaptureinitializationsettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings) -Objekt, das zum Initialisieren des **mediacapture** -Objekts verwendet wird. In diesem Beispiel werden die folgenden Einstellungen verwendet:

* [**Sourcegroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sourcegroup) : Hiermit wird dem System mitgeteilt, welche Quell Gruppe zum erhalten von Frames verwendet werden soll. Bedenken Sie, dass die Quellgruppe einen Satz von Medienframequellen definiert, die gleichzeitig verwendet werden können.
* [**Sharingmode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sharingmode) : Hiermit wird dem System mitgeteilt, ob Sie die ausschließliche Kontrolle über die Erfassungs Quell Geräte benötigen. Bei Festlegung auf [**ExclusiveControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode) können Sie die Einstellungen für das Aufnahmegerät, z. B. das Format der von ihm erzeugten Frames, ändern. Wenn jedoch eine andere App bereits über die exklusive Steuerung verfügt, wird der Versuch Ihrer App, das Medienaufnahmegerät zu initialisieren, fehlschlagen. Bei Festlegung auf [**SharedReadOnly**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode) können Sie Frames von den Framequellen empfangen, auch wenn diese gerade von einer anderen App verwendet werden. Sie können jedoch nicht die Einstellungen der Geräte ändern.
* [**Memorypreference**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.memorypreference) : Wenn Sie die [**CPU**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference)angeben, verwendet das System den CPU-Arbeitsspeicher, der garantiert, dass Sie beim Eintreffen von Frames als [**softwarebitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) -Objekte verfügbar sind. Wenn Sie [**Auto**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference) angeben, wählt das System dynamisch den optimalen Speicherort zum Speichern der Frames aus. Wenn das System die Verwendung des GPU-Speichers auswählt, werden die Medienframes als [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface)-Objekt und nicht als **SoftwareBitmap**-Objekt übermittelt.
* [**Streamingcapturemode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) : Legen Sie diese Einstellung auf " [**Video**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.StreamingCaptureMode) " fest, um anzugeben, dass Audiodaten nicht gestreamt werden müssen.

Rufen Sie [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) auf, um **MediaCapture** mit ihren gewünschten Einstellungen zu initialisieren. Achten Sie darauf, den Aufruf in einem *Try*-Block auszuführen, falls die Initialisierung fehlschlägt.

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-the-preferred-format-for-the-frame-source"></a>Festlegen des bevorzugten Formats für die Framequelle
Zum Einstellen des bevorzugten Formats einer Framequelle benötigen Sie ein [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource)-Objekt, das die Quelle darstellt. Sie erhalten dieses Objekt, indem Sie auf das [**Frames**](https://docs.microsoft.com/previous-versions/windows/apps/phone/jj207578(v=win.10))-Verzeichnis des initialisierten **MediaCapture** -Objekts zugreifen und dabei den Bezeichner der gewünschten Framequelle angeben. Aus diesem Grund wurde das [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo)-Objekt bei der Auswahl einer Framequellgruppe gespeichert.

Die [**MediaFrameSource.SupportedFormats**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats)-Eigenschaft enthält eine Liste von [**MediaFrameFormat**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameFormat)-Objekten, die die unterstützten Formate der Framequelle beschreiben. Verwenden Sie die Linq-Erweiterungsmethode **Where**, um basierend auf den gewünschten Eigenschaften ein Format auszuwählen. In diesem Beispiel wird ein Format mit einer Breite von 1.080 Pixeln ausgewählt, welches Frames im 32-Bit-RGB-Format bereitstellen kann. Die **FirstOrDefault**-Erweiterungsmethode wählt den ersten Eintrag in der Liste aus. Ist das ausgewählte Format NULL, wird das angeforderte Format nicht von der Framequelle unterstützt. Wenn das Format unterstützt wird, können Sie [**SetFormatAsync**](https://docs.microsoft.com/windows/uwp/develop/) aufrufen, um die Verwendung dieses Formats durch die Quelle anzufordern.

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## <a name="create-a-frame-reader-for-the-frame-source"></a>Erstellen eines Frame-Readers für die Framequelle
Verwenden Sie zum Empfangen von Frames für die Medienframequelle einen [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader).

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

Instanziieren Sie den Frame-Reader, indem Sie auf Ihrem initialisierten **MediaCapture**-Objekt [**CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync) aufrufen. Das erste Argument dieser Methode ist die Framequelle, von der Frames empfangen werden sollen. Sie können für jede gewünschte Framequelle einen separaten Frame-Reader erstellen. Das zweite Argument gibt dem System das Ausgabeformat vor, in dem die Frames übermittelt werden sollen. So entfällt das Konvertieren der übermittelten Frames. Beachten Sie, dass eine Ausnahme ausgelöst wird, wenn Sie ein von der Framequelle nicht unterstütztes Format festlegen. Stellen Sie daher sicher, dass dieser Wert in der [**SupportedFormats**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats)-Sammlung enthalten ist.  

Registrieren Sie nach dem Erstellen des Frame-Readers einen Handler für das [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived)-Ereignis, das immer dann ausgelöst wird, wenn ein neuer Frame von der Quelle verfügbar ist.

Rufen Sie [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync) auf, um dem System zu befehlen, mit dem Lesen von Frames aus der Quelle zu beginnen.

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## <a name="handle-the-frame-arrived-event"></a>Behandeln des „FrameArrived“-Ereignisses
Das [**MediaFrameReader.FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived)-Ereignis wird ausgelöst, wenn ein neuer Frame verfügbar ist. Sie können entweder jeden empfangenen Frame verarbeiten oder Frames nur bei Bedarf verwenden. Der Frame-Reader löst das Ereignis in seinem eigenen Thread aus. Daher müssen Sie möglicherweise Synchronisierungslogik implementieren, damit Sie nicht aus mehreren Threads auf dieselben Daten zugreifen. In diesem Abschnitt wird das Synchronisieren von Zeichenfarbframes zu einem Image-Steuerelement in einer XAML-Seite veranschaulicht. Dieses Szenario behandelt die zusätzliche Synchronisierungseinschränkung, die erfordert, dass alle Updates für XAML-Steuerelemente im UI-Thread ausgeführt werden.

Als erster Schritt beim Anzeigen von Frames in XAML wird ein Bild-Steuerelement erstellt. 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

Deklarieren Sie auf der CodeBehind-Seite eine Klassenmembervariable vom Typ **SoftwareBitmap**. Diese wird als Hintergrundpuffer verwendet, in den alle eingehenden Bilder kopiert werden. Beachten Sie, dass nicht die Bilddaten selbst kopiert werden, sondern nur die Objektverweise. Deklarieren Sie außerdem einen booleschen Wert, um nachzuverfolgen, ob der UI-Vorgang aktuell ausgeführt wird.

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

Da Frames als **SoftwareBitmap**-Objekte übermittelt werden, müssen Sie ein [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Objekt erstellen, mit dem Sie eine **SoftwareBitmap** als Quelle für ein XAML-**Steuerelement** verwenden können. Sie sollten die Bildquelle in Ihrem Code festlegen, bevor Sie den Frame-Reader starten.

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

Nun wird der **FrameArrived**-Ereignishandler implementiert. Wenn der Handler aufgerufen wird, enthält der *sender*-Parameter einen Verweis auf das **MediaFrameReader**-Objekt, welches das Ereignis ausgelöst hat. Rufen Sie [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) für dieses Objekt auf, um zu versuchen, den aktuellen Frame abzurufen. Wie der Name schon sagt, schlägt das Zurückgeben eines Frames über **TryAcquireLatestFrame** möglicherweise fehl. Testen Sie daher beim Zugreifen auf die VideoMediaFrame- und die SoftwareBitmap-Eigenschaften, ob diese auf NULL festgelegt sind. In diesem Beispiel wird der Operator mit NULL-Bedingung ? verwendet, um auf die **SoftwareBitmap** zurückzugreifen. Anschließend wird geprüft, ob das abgerufene Objekt auf NULL festgelegt ist.

Das **Image**-Steuerelement kann nur Bilder in BRGA8 Format anzeigen, die entweder vormultipliziert sind oder kein Alpha aufweisen. Weist der eingehende Frame nicht dieses Format auf, wird mit der statischen Methode [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.convert) die Software-Bitmap in das richtige Format konvertiert.

Als Nächstes wird mit der [**Interlocked.Exchange**](https://docs.microsoft.com/dotnet/api/system.threading.interlocked.exchange#System_Threading_Interlocked_Exchange__1___0____0_)-Methode der Verweis auf die eingehende Bitmap mit der Hintergrundpuffer-Bitmap ausgetauscht. Bei dieser Methode werden die Verweise in einer threadsicheren atomischen Operation ausgetauscht. Nach dem Austausch wird das alte Hintergrundpufferbild – jetzt in der *softwareBitmap*-Variablen – gelöscht, um seine Ressourcen zu bereinigen.

Als Nächstes wird mit dem mit dem **Image**-Element verknüpften [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) eine Aufgabe erstellt, die durch Aufrufen von [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) im UI-Thread ausgeführt wird. Da die asynchronen Aufgaben in dieser Aufgabe ausgeführt werden, wird der an **RunAsync** weitergegebene Lambda-Ausdruck mit dem *async*- Schlagwort deklariert.

Innerhalb der Aufgabe wird die *_taskRunning*-Variable überprüft, um sicherzustellen, dass zu jedem Zeitpunkt nur eine Instanz der Aufgabe ausgeführt wird. Wird die Aufgabe nicht bereits ausgeführt, wird *_taskRunning* auf „true“ festgelegt, um zu verhindern, dass die Aufgabe erneut ausgeführt wird. **Interlocked.Exchange** wird zum Kopieren aus dem Hintergrundpuffer in eine temporäre **SoftwareBitmap** in einer *while*-Schleife aufgerufen, bis das Hintergrundpufferbild NULL ist. Bei jedem Auffüllen der temporären Bitmap wird die **Source**-Eigenschaft des **Bildes** in eine **SoftwareBitmapSource** umgewandelt. Anschließend wird [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) aufgerufen, um die Bildquelle festzulegen.

Schließlich wird die *_taskRunning*-Variable wieder auf „false“ festgelegt, damit die Aufgabe beim nächsten Aufrufen des Handlers erneut ausgeführt werden kann.

> [!NOTE] 
> Wenn Sie auf das [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.videomediaframe.softwarebitmap)- oder [**Direct3DSurface**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.videomediaframe.direct3dsurface)-Objekt zugreifen, das von der [**VideoMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.videomediaframe)-Eigenschaft eines [**MediaFrameReference**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference) bereitgestellt wird, erstellt das System einen starken Verweis auf dieses Objekt. Das bedeutet, dass sie nicht entfernt werden, wenn Sie [**Dispose**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.close) auf dem **MediaFrameReference** aufrufen, der das Objekt enthält. Sie müssen die **Dispose**-Methode von **SoftwareBitmap** oder **Direct3DSurface** explizit direkt aufrufen, damit die Objekte sofort entfernt werden. Andernfalls wird der Garbage Collector den Speicher für diese Objekte schließlich freigeben. Sie wissen jedoch nicht, wann dies sein wird. Wenn die Anzahl der zugeteilten Bitmaps oder Oberflächen die maximale, vom System zugelassene Menge überschreitet, wird der Fluss neuer Frames beendet. Sie können abgerufene Frames mithilfe der [**SoftwareBitmap.Copy**](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.imaging.softwarebitmap.copy)-Methode kopieren und anschließend wieder die ursprünglichen Frames veröffentlichen, um diese Einschränkung zu umgehen. Wenn Sie **MediaFrameReader** mithilfe der Überladung [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype, Windows.Graphics.Imaging.BitmapSize outputSize)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) oder [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_) erstellen, sind die zurückgegebenen Frames Kopien der ursprünglichen Framedaten. Diese verursachen daher kein Anhalten des Frame-Erwerbs, wenn Sie beibehalten werden. 


[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen
Achten Sie darauf, nach dem Lesen der Frames den Medienframe-Reader zu beenden, indem Sie [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.stopasync) aufrufen, die Registrierung des **FrameArrived**-Handlers aufheben und das **MediaCapture**-Objekt löschen.

[!code-cs[Cleanup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

Weitere Informationen zum Bereinigen von Medienaufnahmeobjekten bei angehaltener Anwendung finden Sie unter [**Anzeigen der Kameravorschau**](simple-camera-preview-access.md).

## <a name="the-framerenderer-helper-class"></a>Die FrameRenderer-Hilfsprogrammklasse
Das Universal Windows-[Beispiel für Kameraframes](https://go.microsoft.com/fwlink/?LinkId=823230) stellt eine Hilfsprogrammklasse zum einfachen Anzeigen der Frames aus Farb-, Infrarot- und Tiefenquellen in Ihrer App bereit. In der Regel sollen Tiefen- und Infrarotdaten nicht nur auf dem Bildschirm angezeigt werden. Dennoch ist diese Hilfsprogrammklasse ein hilfreiches Tool zum Veranschaulichen der Frame-Reader-Funktion und zum Debuggen Ihrer eigenen Frame-Reader-Implementierung.

Die **FrameRenderer**-Hilfsprogrammklasse implementiert die folgenden Methoden.

* **FrameRenderer**-Konstruktor – Der Konstruktor initialisiert die Verwendung des übergebenen XAML-[**Bild**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)-Elements durch die Hilfsprogrammklasse zum Anzeigen von Medienframes.
* **ProcessFrame** – Bei dieser Methode wird ein durch eine [**MediaFrameReference**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference) repräsentierter Medienframe in dem in den Konstruktor eingereichten **Bild**-Element angezeigt. Üblicherweise sollten Sie diese Methode aus Ihrem [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived)-Ereignishandler aufrufen, wenn der von [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) zurückgegebene Frame übergeben wird.
* **ConvertToDisplayableImage** – Bei dieser Methode wird das Format des Medienframes überprüft und ggf. in ein anzeigbares Format umgewandelt. Bei Farbbildern wird also überprüft, ob als Farbformat BGRA8 festgelegt ist und der Bitmap-Alphamodus prämultipliziert wird. Bei Tiefen- oder Infrarotframes wird jede Scanzeile verarbeitet, um die Tiefen- oder Infrarotwerte mithilfe der ebenfalls im Beispiel enthaltenen und unten aufgeführten **PsuedoColorHelper**-Klasse in einen Pseudo-Farbgradienten umzuwandeln

> [!NOTE] 
> Zum Ändern von Pixeln in **SoftwareBitmap**-Bildern müssen Sie auf einen nativen Speicherpuffer zugreifen. Zu diesem Zweck müssen Sie die in der untenstehenden Codeauflistung enthaltene IMemoryBufferByteAccess COM-Schnittstelle verwenden und die Projekteigenschaften aktualisieren, um die Kompilierung von unsicherem Code zuzulassen. Weitere Informationen finden Sie unter [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md).

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>Verwenden von MultiSourceMediaFrameReader zum Abrufen zeitkorrelierter Frames aus mehreren Quellen
Ab Windows 10, Version 1607, können Sie mit [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) zeitkorrelierte Frames aus mehreren Quellen empfangen. Diese API vereinfacht im Vergleich mit der [**DepthCorrelatedCoordinateMapper**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper)-Klasse die Verarbeitung von Frames aus mehreren Quellen, die zeitlich eng aufeinanderfolgend erfasst wurden. Eine Einschränkung bei der Verwendung dieser neuen Methode ist, das Ereignisse, die eine Frameankunft signalisieren, nur mit der Geschwindigkeit der langsamsten Erfassungsquelle ausgelöst werden. Zusätzliche Frames aus schneller Quellen werden gelöscht. Und da das System erwartet, dass Frames aus verschiedenen Quellen unterschiedlich schnell eintreffen, kann es nicht automatisch erkennen, wenn eine Quelle das Generieren von Frames endgültig beendet hat. Der Beispielcode in diesem Abschnitt zeigt, wie Sie mit einem Ereignis Ihre eigene Timeoutlogik erstellen. Der Aufruf erfolgt, wenn korrelierte Frames nicht in einem von der App festgelegten Zeitlimit eintreffen.

Die Schritte für die Verwendung von [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) ähneln den Schritten für die zuvor in diesem Artikel beschriebene Verwendung von [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader). In diesem Beispiel wird eine Farbquelle und eine Tiefenquelle verwendet. Deklarieren Sie einige Zeichenfolgenvariablen, um die Frame-IDs der Medienquelle zu speichern, die verwendet werden, um Frames aus jeder Quelle auswählen. Deklarieren Sie dann ein [**ManualResetEventSlim**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim), eine [**CancellationTokenSource**](https://docs.microsoft.com/dotnet/api/system.threading.cancellationtokensource) sowie einen [**EventHandler**](https://docs.microsoft.com/dotnet/api/system.eventhandler), der zur Implementierung von Timeoutlogik für das Beispiel verwendet wird. 

[!code-cs[MultiFrameDeclarations](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameDeclarations)]

Ermitteln Sie mithilfe der in diesem Artikel beschriebenen Techniken eine [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup), welche die Farb- und Tiefenquellen enthält, die für dieses Beispielszenario erforderlich sind. Nach Auswahl der gewünschten Framequellengruppe rufen Sie die [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) für jede Framequelle ab.

[!code-cs[SelectColorAndDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColorAndDepth)]

Erstellen und Initialisieren Sie ein **MediaCapture**-Objekt, und übergeben Sie die gewählte Framequellengruppe in den Initialisierungseinstellungen.

[!code-cs[MultiFrameInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameInitMediaCapture)]

Nach der Initialisierung des **MediaCapture**-Objekts rufen Sie die [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource)-Objekte für die Farb- und Tiefenkameras ab. Speichern Sie die ID für jede Quelle, sodass Sie den eingehenden Frame für die entsprechende Quelle auswählen können.

[!code-cs[GetColorAndDepthSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetColorAndDepthSource)]

Erstellen und initialisieren Sie den **MultiSourceMediaFrameReader** durch Aufrufen von [**CreateMultiSourceFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync). Übergeben Sie dabei ein Array mit Framequellen für den Reader. Registrieren Sie einen Ereignishandler für das Ereignis [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived). Dieses Beispiel erstellt eine Instanz der zuvor in diesem Artikel beschriebenen **FrameRenderer**-Hilfsklasse, die zum Rendern von Frames für ein **Image**-Steuerelement dient. Starten Sie die den Framereader durch Aufrufen von [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync).

Registrieren Sie einen Ereignishandler für das zuvor in diesem Beispiel deklarierte Ereignis **CorellationFailed**. Dieses Ereignis signalisiert, wenn eine der verwendeten Medienframequellen die Produktion von Frames beendet. Rufen Sie schließlich mit [**Task.Run**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run_System_Action_) die Timeout-Hilfsmethode **NotifyAboutCorrelationFailure** in einem separaten Thread auf. Die Implementierung dieser Methode wird später in diesem Artikel gezeigt.

[!code-cs[InitMultiFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMultiFrameReader)]

Das Ereignis **FrameArrived** wird jedes Mal ausgelöst, wenn ein neuer Frame von einer der Medienframequellen verfügbar ist, die vom **MultiSourceMediaFrameReader** verwaltet werden. Dies bedeutet, dass das Ereignis mit der Geschwindigkeit der langsamsten Medienquelle ausgelöst wird. Wenn eine Quelle mehrere Frames in der Zeit produziert, in der eine langsamere Quelle einen Frame fertigstellt, werden die zusätzlichen Frames aus der schnellen Quelle gelöscht. 

Rufen Sie die dem Ereignis zugeordnete [**MultiSourceMediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference) durch Aufrufen von [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame) ab. Rufen Sie die jeder Medienframequelle zugeordnete **MediaFrameReference** durch Aufrufen von [**TryGetFrameReferenceBySourceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid) ab. Übergeben Sie dabei die ID-Zeichenfolgen, die Sie gespeichert haben, als der Framereader initialisiert wurde.

Rufen Sie die [**Set**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim.set#System_Threading_ManualResetEventSlim_Set)-Methode des **ManualResetEventSlim**-Objekts auf, um zu signalisieren, dass Frames angekommen sind. Dieses Ereignis wird in der **NotifyCorrelationFailure**-Methode überprüft, die in einem separaten Thread ausgeführt wird. 

Schließlich verarbeiten Sie die zeitkorrelierten Medienframes. Dieses Beispiel zeigt einfach den Frame aus der Tiefenquelle an.

[!code-cs[MultiFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameArrived)]

Die Hilfsmethode **NotifyCorrelationFailure** wurde nach dem Start des Framereaders in einem separaten Thread ausgeführt. Überprüfen Sie in dieser Methode, ob das Ereignis des Frameempfangs signalisiert wurde. Beachten Sie, dass dieses Ereignis im **FrameArrived**-Handler ausgelöst wird, sobald ein Satz korrelierter Frames ankommt. Wenn das Ereignis nicht innerhalb einer von der App definierten Zeit ausgelöst wurde (5 Sekunden sind ein angemessener Wert), und der Task nicht mit der **CancellationToken**-Methode abgebrochen wurde, hat wahrscheinlich eine der Medienframequellen das Lesen von Frames beendet. In diesem Fall sollten Sie in der Regel den Framereader schließen, indem Sie das **CorrelationFailed**-Ereignis auslösen. Im Handler für dieses Ereignis können Sie den Framereader beenden und die zugeordnet Ressourcen bereinigen (wie zuvor in diesem Artikel gezeigt).

[!code-cs[NotifyCorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetNotifyCorrelationFailure)]

[!code-cs[CorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCorrelationFailure)]

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>Verwenden Sie den gepufferte Frame-Erwerb-Modus, um die Reihenfolge der erfassten Frames beizubehalten
Ab Windows 10, Version 1709, können Sie die **[AcquisitionMode](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** -Eigenschaft eines **MediaFrameReader** oder **MultiSourceMediaFrameReader** auf **Gepuffert** festlegen, um die Reihenfolge der Bilder in Ihre App von der Framequelle beizubehalten.

[!code-cs[SetBufferedFrameAcquisitionMode](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSetBufferedFrameAcquisitionMode)]

Im Standard-Erwerbsmodus, **Echtzeit**, wenn mehrere Frames von der Quelle erworben werden, während Ihre App noch das **FrameArrived**-Ereignis für einen vorherigen Frame verarbeitet, sendet das System Ihrer App die kürzlich erworbene Frame and andere zusätzliche Frames, die im Puffer warten. Dadurch erhält Ihre App jederzeit alle aktuellsten verfügbaren Frames. Dies ist in der Regel der nützlichste Modus für Computer-Vision Anwendungen in Echtzeit. 

Im **gepufferten** Erwerbsmodus behält das System alle Frames im Puffer und liefert diese an Ihre App durch ein **FrameArrived**-Ereignis in der Reihenfolge, in der es empfangen wurde. Beachten Sie in diesem Modus, dass, wenn der Puffer des Systems für diese Frames voll wird, das System den Erwerb neuer Frames bis zum Abschluss des **FrameArrived**-Ereignisses für vorherige Frames beendet, um mehr Platz im Puffer zu schaffen.

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>Verwenden Sie zum Anzeigen von Frames im MediaPlayerElement die MediaSource
Ab Windows, Version 1709, können Sie die vom **MediaFrameReader** erworbenen Frames direkt in einem **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** -Steuerelement auf der XAML-Seite anzeigen. Dies geschieht mithilfe der **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** , um ein **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** -Objekt zu erstellen, das direkt vom **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** Zusammenhang mit einem **MediaPlayerElement** verwendet werden kann. Weitere Informationen zur Arbeit mit **MediaPlayer** und **MediaPlayerElement** finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md).

Die folgenden Codebeispiele zeigen Sie eine einfache Implementierung, die Frames von einer nach vorne gerichteten und nach hinten gerichteten Kamera gleichzeitig in einer XAML-Seite anzeigt.

Fügen Sie zuerst der XAML-Seite zwei **MediaPlayerElement**-Steuerelement hinzu.

[!code-xml[MediaPlayerElement1XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement1XAML)]

[!code-xml[MediaPlayerElement2XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement2XAML)]

Verwenden die danach das im vorherigen Abschnitte in diesem Artikel gezeigte Verfahren, wählen Sie eine **MediaFrameSourceGroup**, die **MediaFrameSourceInfo**-Objekte für Farbkameras auf der Vorder- und Rückseite enthält. Beachten Sie, dass der **MediaPlayer** nicht automatisch Frames aus schwarzweiß Formaten wie z. B. Tiefe- oder IR-Daten, in Farbdaten umwandelt. Das Verwenden anderer Sensortypen kann zu unerwarteten Ergebnissen führen. 

[!code-cs[MediaSourceSelectGroup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceSelectGroup)]

Initialisieren des **MediaCapture**-Objekts zum Verwenden der ausgewählten **MediaFrameSourceGroup**.

[!code-cs[MediaSourceInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceInitMediaCapture)]

Rufen Sie schließlich **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** zum Erstellen einer **MediaSource** für jeden Frame mithilfe der **[ID](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** -Eigenschaft des zugeordneten **MediaFrameSourceInfo**-Objekts zum Auswählen einer der Framequellen in der **MediaCapture** des Objekts **[FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** -Sammlung auf. Initialisieren Sie ein neues **MediaPlayer**-Objekt, und weisen Sie ihn ein **MediaPlayerElement** durch Aufrufen von **[SetMediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)** zu. Anschließend können Sie die **[Source](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source)** -Eigenschaft auf das neu erstellte **MediaSource**-Objekt festlegen.

[!code-cs[MediaSourceMediaPlayer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceMediaPlayer)]

## <a name="use-video-profiles-to-select-a-frame-source"></a>Verwenden Sie Videoprofile zum Auswählen einer Framequelle

Eine Kameraprofil, dargestellt durch ein [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile)-Objekt, stellt eine Reihe von Funktionen von einem bestimmten Aufnahmegerät wie z. B. Frameraten, Auflösungen oder erweiterte Features wie die HDR-Aufnahme dar. Ein Aufnahmegerät unterstützt möglicherweise mehrere Profile, was Ihnen ermöglicht, die für Ihr Szenario optimierte Aufnahme auszuwählen. Ab Windows 10, Version 1803, können Sie die **MediaCaptureVideoProfile**-Klasse auswählen, um eine Medienframequelle mit den gewünschten Funktionen zu erstellen, bevor Sie ein **MediaCapture**-Objekt erstellen. Die folgende Beispielmethode sucht ein Video-Profil, das HDR mit Wide Color Gamut (WCG) unterstützt und gibt ein **MediaCaptureInitializationSettings**-Objekt zurück, das verwendet werden kann, um **MediaCapture** zu initialisieren, um das ausgewählte Gerät und Profil zu verwenden.

Erhalten Sie die Liste der verfügbaren Framequellgruppen von Medien auf dem Gerät durch Aufrufen von [**MediaFrameSourceGroup.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync). Durchlaufen Sie die einzelnen Gruppe, und rufen Sie [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) für eine Liste aller Video-Profile für die aktuelle Quellgruppe auf, die das angegebene Profil unterstützen, in diesem Fall HDR mit WCG Foto. Wenn ein Profil, das den Kriterien entspricht, gefunden wird, erstellen Sie ein neues **MediaCaptureInitializationSettings**-Objekt, und legen Sie das **VideoProfile** auf das ausgewählte Profil fest und **VideoDeviceId** auf die **ID**-Eigenschaft der aktuellen Framequellgruppe von Medien.

[!code-cs[GetSettingsWithProfile](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetSettingsWithProfile)]

Weitere Informationen zur Verwendung von Kameraprofilen finden Sie unter [Kameraprofile](camera-profiles.md).

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Einfaches Foto, Video und Audioerfassung mit mediacapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Beispiel für Kamera Frames](https://go.microsoft.com/fwlink/?LinkId=823230)
 

 




