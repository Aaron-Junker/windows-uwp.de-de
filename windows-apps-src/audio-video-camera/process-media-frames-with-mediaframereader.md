---
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: In diesem Artikel wird beschrieben, wie Sie mit „MediaFrameReader“ und „MediaCapture“ Medienframes aus einer oder mehreren verfügbaren Quellen abrufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames).
title: Verarbeiten von Medienframes mit „MediaFrameReader“
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b53103f0d0c67bd18b71ac94812f4cef53ca8ac0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363813"
---
# <a name="process-media-frames-with-mediaframereader"></a>Verarbeiten von Medienframes mit „MediaFrameReader“

In diesem Artikel wird beschrieben, wie Sie mit [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) und [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) Medienframes aus einer oder mehreren verfügbaren Quellen abrufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames). Dieses Feature wurde für die Verwendung von Apps entworfen, die Medienframes in Echtzeit verarbeiten, wie beispielsweise Augmented-Reality- und Tiefenkamera-Apps.

Wenn Sie normale Videos oder Fotos aufnehmen möchten, wie mit einer typischen Foto-App, sollten Sie möglicherweise eine der anderen Aufnahmemethoden verwenden, die von [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) unterstützt werden. Eine Liste der verfügbaren Methoden zur Medienaufnahme und Artikel zu ihrer Verwendung finden Sie unter [**Kamera**](camera.md).

> [!NOTE] 
> Die in diesem Artikel besprochenen Features sind erst ab Windows 10, Version 1607, verfügbar.

> [!NOTE] 
> Es gibt ein Beispiel für universelle Windows-Apps, in dem die Verwendung von **MediaFrameReader** zum Anzeigen von Frames aus unterschiedlichen Framequellen demonstriert wird, unter anderem Farb-, Tiefen- und Infrarotkameras. Weitere Informationen finden Sie unter [Beispiel für Kameraframes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames).

> [!NOTE] 
> Ein neuer Satz von APIs für die Verwendung von **mediaframereader** mit Audiodaten wurde in Windows 10, Version 1803, eingeführt. Weitere Informationen finden Sie unter [Verarbeiten von Audioframes mit mediaframereader](process-audio-frames-with-mediaframereader.md).


## <a name="setting-up-your-project"></a>Einrichten Ihres Projekts
Wie bei allen Apps, die **MediaCapture** verwenden, müssen Sie deklarieren, dass Ihre App die *Webcam*-Funktion verwendet. Erst dann können Sie auf Kamerageräte zugreifen. Wenn Ihre App von einem Audiogerät aufzeichnet, müssen Sie auch die *microphone*-Gerätefunktion deklarieren. 

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie das Kontrollkästchen für die **Webcam** und das Kontrollkästchen für **Mikrofon**.
4.  Für den Zugriff auf die Bibliothek „Bilder und Videos“ aktivieren Sie die Kontrollkästchen für **Bildbibliothek** und **Videobibliothek**.

Im Beispielcode in diesem Artikel werden neben den in der Standard-Projektvorlage enthaltenen APIs auch APIs aus den folgenden Namespaces verwendet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFramesUsing":::

## <a name="select-frame-sources-and-frame-source-groups"></a>Auswählen von Framequellen und Framequellgruppen
Viele Apps, die Medienframes verarbeiten, müssen Frames aus mehreren Quellen gleichzeitig abrufen, z. B. die Farb- und Tiefenkameras eines Geräts. Das [**mediaframesourcegroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) -Objekt stellt einen Satz von Medien Frame Quellen dar, die gleichzeitig verwendet werden können. Rufen Sie die statische Methode [**MediaFrameSourceGroup.FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) auf, um eine Liste aller vom aktuellen Gerät unterstützten Gruppen von Framequellen abzurufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFindAllAsync":::

Sie können auch einen [**devicewatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) mit [**DeviceInformation. kreatewatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) erstellen und den Wert, der von [**mediaframesourcegroup. getdeviceselector**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) zurückgegeben wird, um Benachrichtigungen zu empfangen, wenn sich die verfügbaren Frame Quell Gruppen auf dem Gerät ändern, z. b. Wenn eine externe Kamera angeschlossen ist. Weitere Informationen finden Sie unter [**Auflisten von Geräten**](../devices-sensors/enumerate-devices.md).

Eine [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) verfügt über eine Sammlung von [**MediaFrameSourceInfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo)-Objekten, die in der Gruppe enthaltene Framequellen beschreiben. Nach dem Abrufen der auf dem Gerät verfügbaren Framequellgruppen können Sie die Gruppe auswählen, die die für Sie relevanten Framequellen verfügbar macht.

Das folgende Beispiel zeigt die einfachste Möglichkeit zum Auswählen einer Framequellgruppe. Vom Code werden dann einfach Schleifen durch alle verfügbaren Gruppen und anschließend durch die einzelnen Elemente in der [**SourceInfos**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.sourceinfos)-Sammlung durchgeführt. Für jede **MediaFrameSourceInfo** wird geprüft, ob sie die gewünschten Funktionen unterstützt. In diesem Fall wird die [**MediaStreamType**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype)-Eigenschaft auf den Wert [**VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) überprüft. D. h. das Gerät stellt einen Videovorschau-Stream bereit, und die [**SourceKind**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.sourcekind)-Eigenschaft wird auf den Wert [**Farbe**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceKind) überprüft, um anzuzeigen, dass die Quelle Farbframes bereitstellt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSimpleSelect":::

Diese Methode zur Identifizierung der gewünschten Framequellgruppe und Framequellen ist für einfache Fälle geeignet. Wenn Sie jedoch anhand komplexerer Kriterien Framequellen auswählen möchten, wird dieses Verfahren unpraktisch. Eine weitere Methode ist die Auswahl mithilfe von Linq-Syntax und anonymen Objekten. Im folgenden Beispiel wird die Erweiterungsmethode **Select** verwendet, um die **MediaFrameSourceGroup**-Objekte in der *FrameSourceGroups*-Liste in ein anonymes Objekt mit zwei Feldern umzuwandeln:*sourceGroup*, welches die Gruppe selbst darstellt, und *colorSourceInfo*, welches die Farbframequelle in der Gruppe repräsentiert. Für das Feld *colorSourceInfo* wird das Ergebnis der **FirstOrDefault**-Methode festgelegt, die das erste Objekt auswählt, für dessen bereitgestelltes Prädikat die Auflösung „true“ ist. In diesem Fall ist das Prädikat „true“, wenn der Datenstromtyp **VideoPreview** und die Art der Quelle **Color** ist und sich die Kamera an der Vorderseite des Geräts befindet.

Aus der Liste der von der oben beschriebenen Abfrage zurückgegebenen anonymen Objekte werden mit der **Where**-Erweiterungsmethode nur die Objekte ausgewählt, in denen das Feld *colorSourceInfo* nicht NULL ist. Schließlich wird **FirstOrDefault** aufgerufen, um das erste Element in der Liste auszuwählen.

Nun können Sie mithilfe der Felder des ausgewählten Objekts die Verweise auf das ausgewählte **MediaFrameSourceGroup**- und das **MediaFrameSourceInfo**-Objekt abrufen, die die Farbkamera darstellen. Diese werden später verwendet, um das **MediaCapture**-Objekt zu initialisieren und einen **MediaFrameReader** für die ausgewählte Quelle zu erstellen. Abschließend sollten Sie testen, ob Quellgruppe NULL ist. Dies würde bedeuten, dass das aktuelle Gerät nicht über Ihre angeforderten Aufnahmequellen verfügt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSelectColor":::

Im folgenden Beispiel wird mit einer ähnlichen Methode wie der oben beschriebenen eine Quellgruppe mit Farb-, Tiefen- und Infrarotkameras ausgewählt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetColorInfraredDepth":::

> [!NOTE]
> Ab Windows 10, Version 1803, können Sie die [**mediacapturevideoprofile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) -Klasse verwenden, um eine Medien Frame Quelle mit einem Satz gewünschter Funktionen auszuwählen. Weitere Informationen finden Sie im Abschnitt **Verwenden von Video Profilen zum Auswählen einer Frame Quelle weiter** unten in diesem Artikel.


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>Initialisieren des MediaCapture-Objekts zum Verwenden der ausgewählten Framequellgruppe
Im nächsten Schritt wird das **MediaCapture**-Objekt initialisiert, um die im vorherigen Schritt ausgewählte Framequellgruppe zu verwenden.

Das **MediaCapture**-Objekt wird üblicherweise an mehreren Stellen in Ihrer App verwendet. Sie sollten daher eine Klassenmembervariable deklarieren, um es aufzunehmen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareMediaCapture":::

Erstellen Sie eine Instanz des **MediaCapture**-Objekts, indem Sie den Konstruktor aufrufen. Erstellen Sie als nächstes ein [**mediacaptureinitializationsettings**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings) -Objekt, das zum Initialisieren des **mediacapture** -Objekts verwendet wird. In diesem Beispiel werden die folgenden Einstellungen verwendet:

* [**SourceGroup**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sourcegroup) – Damit wird dem System mitgeteilt, welche Quellgruppe zum Abrufen von Frames verwendet wird. Bedenken Sie, dass die Quellgruppe einen Satz von Medienframequellen definiert, die gleichzeitig verwendet werden können.
* [**SharingMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sharingmode) – Damit wird dem System mitgeteilt, ob exklusive Steuerung der Aufnahmequellgeräte erforderlich ist. Bei Festlegung auf [**ExclusiveControl**](/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode) können Sie die Einstellungen für das Aufnahmegerät, z. B. das Format der von ihm erzeugten Frames, ändern. Wenn jedoch eine andere App bereits über die exklusive Steuerung verfügt, wird der Versuch Ihrer App, das Medienaufnahmegerät zu initialisieren, fehlschlagen. Bei Festlegung auf [**SharedReadOnly**](/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode) können Sie Frames von den Framequellen empfangen, auch wenn diese gerade von einer anderen App verwendet werden. Sie können jedoch nicht die Einstellungen der Geräte ändern.
* [**MemoryPreference**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.memorypreference) – Wenn Sie [**CPU**](/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference) angeben, verwendet das Gerät den CPU-Speicher. Damit wird garantiert, dass eintreffende Frames als [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekte verfügbar sind. Wenn Sie [**Auto**](/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference) angeben, wählt das System dynamisch den optimalen Speicherort zum Speichern der Frames aus. Wenn das System die Verwendung des GPU-Speichers auswählt, werden die Medienframes als [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface)-Objekt und nicht als **SoftwareBitmap**-Objekt übermittelt.
* [**StreamingCaptureMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) – Legen Sie [**Video**](/uwp/api/Windows.Media.Capture.StreamingCaptureMode) fest, um anzugeben, dass das Streamen von Audio nicht erforderlich ist.

Rufen Sie [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) auf, um **MediaCapture** mit ihren gewünschten Einstellungen zu initialisieren. Achten Sie darauf, den Aufruf in einem *Try*-Block auszuführen, falls die Initialisierung fehlschlägt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitMediaCapture":::

## <a name="set-the-preferred-format-for-the-frame-source"></a>Festlegen des bevorzugten Formats für die Framequelle
Zum Einstellen des bevorzugten Formats einer Framequelle benötigen Sie ein [**MediaFrameSource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource)-Objekt, das die Quelle darstellt. Sie erhalten dieses Objekt, indem Sie auf das [**Frames**](/previous-versions/windows/apps/phone/jj207578(v=win.10))-Verzeichnis des initialisierten **MediaCapture** -Objekts zugreifen und dabei den Bezeichner der gewünschten Framequelle angeben. Aus diesem Grund wurde das [**MediaFrameSourceInfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo)-Objekt bei der Auswahl einer Framequellgruppe gespeichert.

Die [**MediaFrameSource.SupportedFormats**](/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats)-Eigenschaft enthält eine Liste von [**MediaFrameFormat**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameFormat)-Objekten, die die unterstützten Formate der Framequelle beschreiben. Verwenden Sie die Linq-Erweiterungsmethode **Where**, um basierend auf den gewünschten Eigenschaften ein Format auszuwählen. In diesem Beispiel wird ein Format mit einer Breite von 1.080 Pixeln ausgewählt, welches Frames im 32-Bit-RGB-Format bereitstellen kann. Die **FirstOrDefault**-Erweiterungsmethode wählt den ersten Eintrag in der Liste aus. Ist das ausgewählte Format NULL, wird das angeforderte Format nicht von der Framequelle unterstützt. Wenn das Format unterstützt wird, können Sie [**SetFormatAsync**](../develop/index.md) aufrufen, um die Verwendung dieses Formats durch die Quelle anzufordern.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetPreferredFormat":::

## <a name="create-a-frame-reader-for-the-frame-source"></a>Erstellen eines Frame-Readers für die Framequelle
Verwenden Sie zum Empfangen von Frames für die Medienframequelle einen [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareMediaFrameReader":::

Instanziieren Sie den Frame-Reader, indem Sie auf Ihrem initialisierten **MediaCapture**-Objekt [**CreateFrameReaderAsync**](/uwp/api/windows.media.capture.mediacapture.createframereaderasync) aufrufen. Das erste Argument dieser Methode ist die Framequelle, von der Frames empfangen werden sollen. Sie können für jede gewünschte Framequelle einen separaten Frame-Reader erstellen. Das zweite Argument gibt dem System das Ausgabeformat vor, in dem die Frames übermittelt werden sollen. So entfällt das Konvertieren der übermittelten Frames. Beachten Sie, dass eine Ausnahme ausgelöst wird, wenn Sie ein von der Framequelle nicht unterstütztes Format festlegen. Stellen Sie daher sicher, dass dieser Wert in der [**SupportedFormats**](/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats)-Sammlung enthalten ist.  

Registrieren Sie nach dem Erstellen des Frame-Readers einen Handler für das [**FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived)-Ereignis, das immer dann ausgelöst wird, wenn ein neuer Frame von der Quelle verfügbar ist.

Rufen Sie [**StartAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.startasync) auf, um dem System zu befehlen, mit dem Lesen von Frames aus der Quelle zu beginnen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCreateFrameReader":::

## <a name="handle-the-frame-arrived-event"></a>Behandeln des „FrameArrived“-Ereignisses
Das [**MediaFrameReader.FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived)-Ereignis wird ausgelöst, wenn ein neuer Frame verfügbar ist. Sie können entweder jeden empfangenen Frame verarbeiten oder Frames nur bei Bedarf verwenden. Der Frame-Reader löst das Ereignis in seinem eigenen Thread aus. Daher müssen Sie möglicherweise Synchronisierungslogik implementieren, damit Sie nicht aus mehreren Threads auf dieselben Daten zugreifen. In diesem Abschnitt wird das Synchronisieren von Zeichenfarbframes zu einem Image-Steuerelement in einer XAML-Seite veranschaulicht. Dieses Szenario behandelt die zusätzliche Synchronisierungseinschränkung, die erfordert, dass alle Updates für XAML-Steuerelemente im UI-Thread ausgeführt werden.

Als erster Schritt beim Anzeigen von Frames in XAML wird ein Bild-Steuerelement erstellt. 

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetImageElementXAML":::

Deklarieren Sie auf der CodeBehind-Seite eine Klassenmembervariable vom Typ **SoftwareBitmap**. Diese wird als Hintergrundpuffer verwendet, in den alle eingehenden Bilder kopiert werden. Beachten Sie, dass nicht die Bilddaten selbst kopiert werden, sondern nur die Objektverweise. Deklarieren Sie außerdem einen booleschen Wert, um nachzuverfolgen, ob der UI-Vorgang aktuell ausgeführt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareBackBuffer":::

Da Frames als **SoftwareBitmap**-Objekte übermittelt werden, müssen Sie ein [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)-Objekt erstellen, mit dem Sie eine **SoftwareBitmap** als Quelle für ein XAML-**Steuerelement** verwenden können. Sie sollten die Bildquelle in Ihrem Code festlegen, bevor Sie den Frame-Reader starten.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetImageElementSource":::

Nun wird der **FrameArrived**-Ereignishandler implementiert. Wenn der Handler aufgerufen wird, enthält der *sender*-Parameter einen Verweis auf das **MediaFrameReader**-Objekt, welches das Ereignis ausgelöst hat. Rufen Sie [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) für dieses Objekt auf, um zu versuchen, den aktuellen Frame abzurufen. Wie der Name schon sagt, schlägt das Zurückgeben eines Frames über **TryAcquireLatestFrame** möglicherweise fehl. Testen Sie daher beim Zugreifen auf die VideoMediaFrame- und die SoftwareBitmap-Eigenschaften, ob diese auf NULL festgelegt sind. In diesem Beispiel wird der Operator mit NULL-Bedingung ? verwendet, um auf die **SoftwareBitmap** zurückzugreifen. Anschließend wird geprüft, ob das abgerufene Objekt auf NULL festgelegt ist.

Das **Image**-Steuerelement kann nur Bilder in BRGA8 Format anzeigen, die entweder vormultipliziert sind oder kein Alpha aufweisen. Weist der eingehende Frame nicht dieses Format auf, wird mit der statischen Methode [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) die Software-Bitmap in das richtige Format konvertiert.

Als Nächstes wird mit der [**Interlocked.Exchange**](/dotnet/api/system.threading.interlocked.exchange#System_Threading_Interlocked_Exchange__1___0____0_)-Methode der Verweis auf die eingehende Bitmap mit der Hintergrundpuffer-Bitmap ausgetauscht. Bei dieser Methode werden die Verweise in einer threadsicheren atomischen Operation ausgetauscht. Nach dem Austausch wird das alte Hintergrundpufferbild – jetzt in der *softwareBitmap*-Variablen – gelöscht, um seine Ressourcen zu bereinigen.

Als Nächstes wird mit dem mit dem **Image**-Element verknüpften [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) eine Aufgabe erstellt, die durch Aufrufen von [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) im UI-Thread ausgeführt wird. Da die asynchronen Aufgaben in dieser Aufgabe ausgeführt werden, wird der an **RunAsync** weitergegebene Lambda-Ausdruck mit dem *async*- Schlagwort deklariert.

Innerhalb der Aufgabe wird die *_taskRunning*-Variable überprüft, um sicherzustellen, dass zu jedem Zeitpunkt nur eine Instanz der Aufgabe ausgeführt wird. Wird die Aufgabe nicht bereits ausgeführt, wird *_taskRunning* auf „true“ festgelegt, um zu verhindern, dass die Aufgabe erneut ausgeführt wird. **Interlocked.Exchange** wird zum Kopieren aus dem Hintergrundpuffer in eine temporäre **SoftwareBitmap** in einer *while*-Schleife aufgerufen, bis das Hintergrundpufferbild NULL ist. Bei jedem Auffüllen der temporären Bitmap wird die **Source**-Eigenschaft des **Bildes** in eine **SoftwareBitmapSource** umgewandelt. Anschließend wird [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) aufgerufen, um die Bildquelle festzulegen.

Schließlich wird die *_taskRunning*-Variable wieder auf „false“ festgelegt, damit die Aufgabe beim nächsten Aufrufen des Handlers erneut ausgeführt werden kann.

> [!NOTE] 
> Wenn Sie auf das [**SoftwareBitmap**](/uwp/api/windows.media.capture.frames.videomediaframe.softwarebitmap)- oder [**Direct3DSurface**](/uwp/api/windows.media.capture.frames.videomediaframe.direct3dsurface)-Objekt zugreifen, das von der [**VideoMediaFrame**](/uwp/api/windows.media.capture.frames.mediaframereference.videomediaframe)-Eigenschaft eines [**MediaFrameReference**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference) bereitgestellt wird, erstellt das System einen starken Verweis auf dieses Objekt. Das bedeutet, dass sie nicht entfernt werden, wenn Sie [**Dispose**](/uwp/api/windows.media.capture.frames.mediaframereference.close) auf dem **MediaFrameReference** aufrufen, der das Objekt enthält. Sie müssen die **Dispose**-Methode von **SoftwareBitmap** oder **Direct3DSurface** explizit direkt aufrufen, damit die Objekte sofort entfernt werden. Andernfalls wird der Garbage Collector den Speicher für diese Objekte schließlich freigeben. Sie wissen jedoch nicht, wann dies sein wird. Wenn die Anzahl der zugeteilten Bitmaps oder Oberflächen die maximale, vom System zugelassene Menge überschreitet, wird der Fluss neuer Frames beendet. Sie können abgerufene Frames mithilfe der [**softwarebitmap. Copy**](/uwp/api/windows.graphics.imaging.softwarebitmap.copy) -Methode kopieren und dann die ursprünglichen Frames freigeben, um diese Einschränkung zu überwinden. Wenn Sie den **mediaframereader** mithilfe der überladenen Datei "" erstellen, erstellen Sie auch "" mit der-Überladung "" "", "" "". [Windows. Graphics. Imaging. bitmapsize outputsize)](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) oder [kreateframereaderasync (Windows. Media. Capture. Frames. mediaframesource InputSource, System. String outputsubtype)](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_). die zurückgegebenen Frames sind Kopien der ursprünglichen Frame Daten und bewirken somit nicht, dass die Rahmen Erfassung angehalten wird, wenn Sie beibehalten werden. 


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFrameArrived":::

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen
Achten Sie darauf, nach dem Lesen der Frames den Medienframe-Reader zu beenden, indem Sie [**StopAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.stopasync) aufrufen, die Registrierung des **FrameArrived**-Handlers aufheben und das **MediaCapture**-Objekt löschen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCleanup":::

Weitere Informationen zum Bereinigen von Medienaufnahmeobjekten bei angehaltener Anwendung finden Sie unter [**Anzeigen der Kameravorschau**](simple-camera-preview-access.md).

## <a name="the-framerenderer-helper-class"></a>Die FrameRenderer-Hilfsprogrammklasse
Das Universal Windows-[Beispiel für Kameraframes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames) stellt eine Hilfsprogrammklasse zum einfachen Anzeigen der Frames aus Farb-, Infrarot- und Tiefenquellen in Ihrer App bereit. In der Regel sollen Tiefen- und Infrarotdaten nicht nur auf dem Bildschirm angezeigt werden. Dennoch ist diese Hilfsprogrammklasse ein hilfreiches Tool zum Veranschaulichen der Frame-Reader-Funktion und zum Debuggen Ihrer eigenen Frame-Reader-Implementierung.

Die **FrameRenderer**-Hilfsprogrammklasse implementiert die folgenden Methoden.

* **FrameRenderer**-Konstruktor – Der Konstruktor initialisiert die Verwendung des übergebenen XAML-[**Bild**](/uwp/api/Windows.UI.Xaml.Controls.Image)-Elements durch die Hilfsprogrammklasse zum Anzeigen von Medienframes.
* **ProcessFrame** – Bei dieser Methode wird ein durch eine [**MediaFrameReference**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference) repräsentierter Medienframe in dem in den Konstruktor eingereichten **Bild**-Element angezeigt. Üblicherweise sollten Sie diese Methode aus Ihrem [**FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived)-Ereignishandler aufrufen, wenn der von [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) zurückgegebene Frame übergeben wird.
* **ConvertToDisplayableImage** – Bei dieser Methode wird das Format des Medienframes überprüft und ggf. in ein anzeigbares Format umgewandelt. Bei Farbbildern wird also überprüft, ob als Farbformat BGRA8 festgelegt ist und der Bitmap-Alphamodus prämultipliziert wird. Bei Tiefen- oder Infrarotframes wird jede Scanzeile verarbeitet, um die Tiefen- oder Infrarotwerte mithilfe der ebenfalls im Beispiel enthaltenen und unten aufgeführten **PsuedoColorHelper**-Klasse in einen Pseudo-Farbgradienten umzuwandeln

> [!NOTE] 
> Zum Ändern von Pixeln in **SoftwareBitmap**-Bildern müssen Sie auf einen nativen Speicherpuffer zugreifen. Zu diesem Zweck müssen Sie die in der untenstehenden Codeauflistung enthaltene IMemoryBufferByteAccess COM-Schnittstelle verwenden und die Projekteigenschaften aktualisieren, um die Kompilierung von unsicherem Code zuzulassen. Weitere Informationen finden Sie unter [Erstellen, Bearbeiten und Speichern von Bitmapbildern](imaging.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetIMemoryBufferByteAccess":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetFrameRenderer":::

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>Verwenden von multisourcemediaframereader zum erhalten von Zeit korellen Frames aus mehreren Quellen
Ab Windows 10, Version 1607, können Sie [**multisourcemediaframereader**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader) verwenden, um Zeit KORE Frames aus mehreren Quellen zu empfangen. Diese API vereinfacht die Verarbeitung, die Frames aus mehreren Quellen erfordert, die in unmittelbarer Temporaler Nähe, z. b. der Verwendung der [**depthcorrelatedcoordinatemapper**](/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper) -Klasse, erstellt wurden. Eine Einschränkung bei der Verwendung dieser neuen Methode besteht darin, dass Frame-empfangene Ereignisse nur mit der Geschwindigkeit der langsamsten Erfassungs Quelle ausgelöst werden. Zusätzliche Frames aus schnelleren Quellen werden gelöscht. Da das System erwartet, dass Frames aus unterschiedlichen Quellen mit unterschiedlichen Raten ankommen, erkennt es außerdem nicht automatisch, ob eine Quelle das Erstellen von Frames beendet hat. Der Beispielcode in diesem Abschnitt zeigt, wie ein Ereignis verwendet wird, um eine eigene Timeout Logik zu erstellen, die aufgerufen wird, wenn korrelierte Frames nicht innerhalb eines von der APP definierten Zeitlimits ankommen.

Die Schritte für die Verwendung von [**multisourcemediaframereader**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader) ähneln den Schritten für die Verwendung von " [**mediaframereader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) ", die weiter oben in diesem Artikel beschrieben wurden. In diesem Beispiel werden eine Farb Quelle und eine tiefen Quelle verwendet. Deklarieren Sie einige Zeichen folgen Variablen zum Speichern der Medien Frame-Quell-IDs, die zum Auswählen von Frames aus den einzelnen Quellen verwendet werden. Deklarieren Sie als nächstes einen [**manualrestiteventslim**](/dotnet/api/system.threading.manualreseteventslim), eine [**cancellationdekensource**](/dotnet/api/system.threading.cancellationtokensource)und einen [**EventHandler**](/dotnet/api/system.eventhandler) , der verwendet wird, um für das Beispiel eine Timeout Logik zu implementieren. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameDeclarations":::

Fragen Sie mithilfe der zuvor in diesem Artikel beschriebenen Verfahren nach einer [**mediaframesourcegroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) , die die für dieses Beispielszenario erforderlichen Farb-und tiefen Quellen enthält. Nachdem Sie die gewünschte Frame Quell Gruppe ausgewählt haben, können Sie die [**mediaframesourceinfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) für jede Frame Quelle erhalten.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSelectColorAndDepth":::

Erstellen und initialisieren Sie ein **mediacapture** -Objekt, und übergeben Sie die ausgewählte Frame Quell Gruppe in den Initialisierungs Einstellungen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameInitMediaCapture":::

Nachdem Sie das **mediacapture** -Objekt initialisiert haben, rufen Sie [**mediaframesource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) -Objekte für die Farb-und tiefen Kameras ab. Speichern Sie die ID für jede Quelle, damit Sie den ankommenden Frame für die entsprechende Quelle auswählen können.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetColorAndDepthSource":::

Erstellen und initialisieren Sie **multisourcemediaframereader** , indem Sie [**createmultisourceframereaderasync**](/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync) aufrufen und ein Array von Frame Quellen übergeben, das der Reader verwendet. Registrieren Sie einen Ereignishandler für das [**framekam**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived) -Ereignis. In diesem Beispiel wird eine Instanz der **framerenderer** -Hilfsklasse erstellt, die weiter oben in diesem Artikel beschrieben wurde, um Frames für ein **Bild** Steuerelement zu Renderen Starten Sie den Frame Reader durch Aufrufen von [**startasync**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync).

Registrieren Sie einen Ereignishandler für das **corellationfailed** -Ereignis, das zuvor im Beispiel deklariert wurde. Dieses Ereignis wird signalisiert, wenn eine der verwendeten Medien Frame-Quellen die Erstellung von Frames beendet. Zum Schluss wird [**Task. Run**](/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run_System_Action_) aufgerufen, um die Timeout-Hilfsmethode **notifyaboutcorrelationfailure**in einem separaten Thread aufzurufen. Die Implementierung dieser Methode wird weiter unten in diesem Artikel gezeigt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitMultiFrameReader":::

Das Ereignis **framekam** wird immer dann ausgelöst, wenn ein neuer Frame aus allen Medien Frame Quellen verfügbar ist, die von **multisourcemediaframereader**verwaltet werden. Dies bedeutet, dass das Ereignis für die Kadenz der langsamsten Medienquelle ausgelöst wird. Wenn eine Quelle mehrere Frames erstellt, wenn eine langsamere Quelle einen Frame erzeugt, werden die zusätzlichen Frames aus der schnellen Quelle gelöscht. 

Rufen Sie die [**multisourcemediaframereferenzierung**](/uwp/api/windows.media.capture.frames.multisourcemediaframereference) ab, die dem Ereignis zugeordnet ist, indem Sie [**tryacquirelatestframe**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame)aufrufen. Rufen Sie die jedem Medien Frame-Quelle zugeordneten **mediaframereferenzierungsart** ab, indem Sie [**trygetframereferencebysourceid**](/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid)aufrufen und dabei die ID-Zeichen folgen übergeben, die beim Initialisieren des Frame Readers gespeichert wurden.

Aufrufen der [**Set**](/dotnet/api/system.threading.manualreseteventslim.set#System_Threading_ManualResetEventSlim_Set) -Methode des **ManualResetEventSlim** -Objekts, um zu signalisieren, dass Frames eingetroffen sind. Dieses Ereignis wird in der **notifycorrelationfailure** -Methode, die in einem separaten Thread ausgeführt wird, überprüft. 

Führen Sie schließlich eine beliebige Verarbeitung der Zeit korrelierten Medien Frames aus. In diesem Beispiel wird einfach der Rahmen aus der tiefen Quelle angezeigt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameArrived":::

Die **notifycorrelationfailure** -Hilfsmethode wurde nach dem Start des Frame Readers in einem separaten Thread ausgeführt. Überprüfen Sie in dieser Methode, ob das empfangene Frame-Ereignis signalisiert wurde. Beachten Sie, dass wir im **framearrive** -Handler dieses Ereignis immer dann festlegen, wenn ein Satz korrelierter Frames eingeht. Wenn das Ereignis für eine APP-definierte Zeitspanne nicht signalisiert wurde-5 Sekunden ist ein angemessener Wert, und die Aufgabe wurde nicht mit dem **CancellationToken**abgebrochen, dann ist es wahrscheinlich, dass eine der Medien Frame Quellen das Lesen von Frames beendet hat. In diesem Fall möchten Sie in der Regel den Frame Reader Herunterfahren, um das von der APP definierte **correlationfailed** -Ereignis zu erhöhen. Im Handler für dieses Ereignis können Sie den Frame Reader abbrechen und seine zugehörigen Ressourcen bereinigen, wie zuvor in diesem Artikel gezeigt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetNotifyCorrelationFailure":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCorrelationFailure":::

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>Verwenden des gepufferten Frame-Erfassungs Modus zum Beibehalten der Sequenz von abgerufenen Frames
Ab Windows 10, Version 1709, können Sie die **[acquisitionmode](/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** -Eigenschaft eines **mediaframereader** oder **multisourcemediaframereader** auf **Buffered** festlegen, um die Reihenfolge der Frames beizubehalten, die von der Frame Quelle an Ihre APP weitergeleitet werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSetBufferedFrameAcquisitionMode":::

Wenn im Standard Erwerbs Modus in **Echtzeit**mehrere Frames aus der Quelle abgerufen werden, während Ihre APP das Ereignis **framekam** für einen vorherigen Frame weiterhin verarbeitet, sendet das System Ihrer APP den zuletzt abgerufenen Frame, und es werden zusätzliche Frames gelöscht, die im Puffer warten. Dadurch erhält Ihre APP jederzeit den neuesten verfügbaren Frame. Dies ist in der Regel der nützlichste Modus für die Echtzeit-Maschinelles Sehen-Anwendungen. 

Im **gepufferten** Erwerbs Modus behält das System alle Frames im Puffer bei und stellt Sie über das **framekam** -Ereignis in der empfangenen Reihenfolge für Ihre APP bereit. Beachten Sie, dass das System in diesem Modus den Abruf neuer Frames beendet, bis die APP das **framekam** -Ereignis für vorherige Frames abgeschlossen hat, sodass mehr Speicherplatz im Puffer freigegeben wird.

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>Verwenden von MediaSource zum Anzeigen von Frames in einem mediaplayerelement
Beginnend mit Windows, Version 1709, können Sie Frames, die von einem **mediaframereader** abgerufen wurden, direkt in einem **[mediaplayerelement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** -Steuerelement auf der XAML-Seite anzeigen. Dies wird erreicht, indem **[MediaSource. kreatefrommediaframesource](/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** zum Erstellen eines **[MediaSource](/uwp/api/windows.media.core.mediasource)** -Objekts verwendet wird, das direkt von einem **[Media Player](/uwp/api/windows.media.playback.mediaplayer)** verwendet werden kann, der einem **mediaplayerelement**zugeordnet ist. Ausführliche Informationen zum Arbeiten mit **Media Player** und **mediaplayerelement**finden Sie unter wieder [Gabe von Audiodateien und Videos mit Media Player](play-audio-and-video-with-mediaplayer.md).

Die folgenden Codebeispiele veranschaulichen eine einfache Implementierung, die die Frames von einer Front-und Back-Kamera gleichzeitig auf einer XAML-Seite anzeigt.

Fügen Sie zunächst der XAML-Seite zwei **mediaplayerelement** -Steuerelemente hinzu.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetMediaPlayerElement1XAML":::

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetMediaPlayerElement2XAML":::

Wählen Sie anschließend unter Verwendung der in den vorherigen Abschnitten in diesem Artikel gezeigten Techniken eine **mediaframesourcegroup** aus, die **mediaframesourceinfo** -Objekte für Farbkameras im Vorder-und Hintergrundbereich enthält. Beachten Sie, dass **Media Player** keine Frames aus nicht-Farbformaten, z. b. tiefen-oder Infrarot Daten, in Farbdaten konvertiert. Die Verwendung anderer Sensortypen kann zu unerwarteten Ergebnissen führen. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceSelectGroup":::

Initialisieren Sie das **mediacapture** -Objekt, um die ausgewählte **mediaframesourcegroup**zu verwenden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceInitMediaCapture":::

Rufen Sie schließlich **[MediaSource. kreatefrommediaframesource](/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** auf, um eine **MediaSource** für jede Frame Quelle zu erstellen, indem Sie die **[ID](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** -Eigenschaft des zugeordneten **mediaframesourceinfo** -Objekts verwenden, um eine der Frame Quellen in der **[framesources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** -Auflistung des **mediacapture** -Objekts auszuwählen. Initialisieren Sie ein neues **Media Player** -Objekt, und weisen Sie es einem **mediaplayerelement** zu, indem Sie **[setmediaplayer](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)** aufrufen. Legen Sie dann die **[Source](/uwp/api/windows.media.playback.mediaplayer.Source)** -Eigenschaft auf das neu erstellte **MediaSource** -Objekt fest.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceMediaPlayer":::

## <a name="use-video-profiles-to-select-a-frame-source"></a>Verwenden von Video Profilen zum Auswählen einer Frame Quelle

Ein Kamera Profil, das von einem [**mediacapturevideoprofile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) -Objekt dargestellt wird, stellt eine Reihe von Funktionen dar, die ein bestimmtes Erfassungsgerät bereitstellt, z. b. Frameraten, Auflösungen oder erweiterte Features wie z. b. HDR Capture. Ein Erfassungsgerät kann mehrere Profile unterstützen, sodass Sie das für das Aufzeichnungs Szenario optimierte auswählen können. Ab Windows 10, Version 1803, können Sie **mediacapturevideoprofile** verwenden, um eine Medien Frame Quelle mit bestimmten Funktionen auszuwählen, bevor das **mediacapture** -Objekt initialisiert wird. Die folgende Beispiel Methode sucht nach einem Video Profil, das HDR mit Wide Color Gamut (WCG) unterstützt, und gibt ein **mediacaptureinitializationsettings** -Objekt zurück, das zum Initialisieren von **mediacapture** verwendet werden kann, um das ausgewählte Gerät und Profil zu verwenden.

Zum Abrufen einer Liste aller Medien Frame-Quell Gruppen, die auf dem aktuellen Gerät verfügbar sind, müssen Sie zunächst " [**mediaframesourcegroup. findallasync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) " aufrufen. Durchlaufen Sie jede Quell Gruppe, und wenden Sie [**mediacapture. findknownvideoprofiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) an, um eine Liste aller Video Profile für die aktuelle Quell Gruppe zu erhalten, die das angegebene Profil unterstützen, in diesem Fall HDR mit WCG Photo. Wenn ein Profil gefunden wird, das die Kriterien erfüllt, erstellen Sie ein neues **mediacaptureinitializationsettings** -Objekt, und legen Sie die **Videoprofile-Datei** auf das Profil auswählen und **videodeviceid** auf die **ID** -Eigenschaft der aktuellen Medien Frame-Quell Gruppe fest.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetSettingsWithProfile":::

Weitere Informationen zur Verwendung von Kameraprofilen finden Sie unter [Kameraprofile](camera-profiles.md).

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Beispiel für Kameraframes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
 

 
