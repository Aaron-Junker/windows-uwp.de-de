---
title: Bildschirmaufnahme in Video
description: In diesem Artikel wird beschrieben, wie Sie Frames, die auf dem Bildschirm aufgezeichnet wurden, mit den Windows. Graphics. Capture-APIs in eine Videodatei codieren.
ms.date: 07/28/2020
ms.topic: article
dev_langs:
- csharp
keywords: Windows 10, UWP, Bildschirmaufnahme, Video
ms.localizationpriority: medium
ms.openlocfilehash: d8f70748d025d50d19dbf2cb184ae841cced7f8a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218633"
---
# <a name="screen-capture-to-video"></a>Bildschirmaufnahme in Video

In diesem Artikel wird beschrieben, wie Sie Frames, die auf dem Bildschirm aufgezeichnet wurden, mit den Windows. Graphics. Capture-APIs in eine Videodatei codieren. Weitere Informationen zum Aufzeichnen von Bildschirmen finden Sie unter [screeen Capture](./screen-capture.md).

## <a name="overview-of-the-video-capture-process"></a>Übersicht über den Video Erfassungsprozess
Dieser Artikel enthält eine exemplarische Vorgehensweise für eine Beispiel-APP, die den Inhalt eines Fensters in einer Videodatei aufzeichnet. Obwohl es möglicherweise so aussieht, als ob ein Großteil des Codes erforderlich ist, um dieses Szenario zu implementieren, ist die Struktur auf hoher Ebene einer screenrecorder-APP ziemlich einfach. Der Bildschirm Aufzeichnungsprozess verwendet drei primäre UWP-Features:

- Mit den [Windows. graphicscapture](/uwp/api/windows.graphics.capture) -APIs werden tatsächlich die Pixel vom Bildschirm gepackt. Die [graphicscaptureitem](/uwp/api/windows.graphics.capture.graphicscaptureitem) -Klasse stellt das Fenster oder die Anzeige dar, die aufgezeichnet wird. [Graphicscapturesession](/uwp/api/windows.graphics.capture.graphicscapturesession) wird verwendet, um den Aufzeichnungs Vorgang zu starten und zu unterbinden. Die [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) -Klasse verwaltet einen Puffer von Frames, in die die Bildschirminhalte kopiert werden.
- Die [MediaStreamSource](/uwp/api/windows.media.core.mediastreamsource) -Klasse empfängt die erfassten Frames und generiert einen Videostream.
- Die [mediatranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) -Klasse empfängt den von **MediaStreamSource** erzeugten Stream und codiert Sie in eine Videodatei.

Der in diesem Artikel gezeigte Beispielcode kann in verschiedene Aufgaben eingeteilt werden:

- **Initialisierung** : Dies umfasst das Konfigurieren der oben beschriebenen UWP-Klassen, das Initialisieren der Grafikgeräte Schnittstellen, das Auswählen eines zu erfassenden Fensters und das Einrichten der Codierungs Parameter wie Auflösung und Framerate.
- **Ereignishandler und Threading** : der primäre Treiber der Haupt Erfassungs Schleife ist **MediaStreamSource** , das Frames in regelmäßigen Abständen über das [samplerequessierte](/uwp/api/windows.media.core.mediastreamsource.samplerequested) Ereignis anfordert. In diesem Beispiel werden Ereignisse verwendet, um die Anforderungen für neue Frames zwischen den verschiedenen Komponenten des Beispiels zu koordinieren. Die Synchronisierung ist wichtig, um das gleichzeitige erfassen und Codieren von Frames zuzulassen.
- Das **Kopieren von Frames** -Frames wird aus dem Aufzeichnungs Frame Puffer in eine separate Direct3D-Oberfläche kopiert, die an **MediaStreamSource** übertragen werden kann, damit die Ressource nicht während der Codierung überschrieben wird. Direct3D-APIs werden verwendet, um diesen Kopiervorgang schnell auszuführen. 

### <a name="about-the-direct3d-apis"></a>Informationen zu den Direct3D-APIs
Wie bereits erwähnt, ist das Kopieren der einzelnen aufgezeichneten Frames wahrscheinlich der komplexeste Teil der in diesem Artikel gezeigten Implementierung. Auf niedriger Ebene wird dieser Vorgang mithilfe von Direct3D durchgeführt. In diesem Beispiel verwenden wir die [sharpdx](http://sharpdx.org/) -Bibliothek, um die Direct3D-Vorgänge aus c# auszuführen. Diese Bibliothek wird nicht mehr offiziell unterstützt, aber Sie wurde ausgewählt, da die Leistung bei Kopier Vorgängen auf niedriger Ebene für dieses Szenario gut geeignet ist. Wir haben versucht, die Direct3D-Vorgänge so diskret wie möglich zu halten, damit Sie Ihren eigenen Code oder andere Bibliotheken für diese Aufgaben leichter ersetzen können.

## <a name="setting-up-your-project"></a>Einrichten Ihres Projekts
Der Beispielcode in dieser exemplarischen Vorgehensweise wurde mithilfe der c#-Projektvorlage **leere app (universelle Windows-APP)** in Visual Studio 2019 erstellt. Um die **Windows. Graphics. Capture** -APIs in Ihrer APP zu verwenden, müssen Sie die **Grafik Aufzeichnungs** Funktion in die Datei "Package. appxmanifest" für Ihr Projekt einschließen. In diesem Beispiel werden generierte Videodateien in der Video Bibliothek auf dem Gerät gespeichert. Um auf diesen Ordner zuzugreifen, müssen Sie die Funktion für die **Video Bibliothek** einschließen.

Wählen Sie zum Installieren des sharpdx-nuget-Pakets in Visual Studio **nuget-Pakete verwalten**aus. Suchen Sie auf der Registerkarte Durchsuchen nach dem Paket "sharpdx. Direct3D11", und klicken Sie auf **Installieren**.

Beachten Sie, dass der Code in der exemplarischen Vorgehensweise, um die Größe der Code Auflistungen in diesem Artikel zu reduzieren, explizite Namespace Verweise und die Deklaration von MainPage-Klassenmember-Variablen, die mit dem führenden Unterstrich "_" benannt sind, unterbricht.

## <a name="setup-for-encoding"></a>Setup für Codierung

Mit der in diesem Abschnitt beschriebenen **setupcoding** -Methode werden einige der Hauptobjekte initialisiert, die zum Erfassen und Codieren von Video Frames und zum Einrichten der Codierungs Parameter für erfasste Videos verwendet werden. Diese Methode kann Programm gesteuert oder als Reaktion auf eine Benutzerinteraktion aufgerufen werden, z. b. ein Klick auf die Schaltfläche. Das Codelisting für **setupcoding** wird unten nach den Beschreibungen der Initialisierungs Schritte angezeigt.

- **Überprüfen Sie die Erfassungs Unterstützung.** Bevor Sie mit dem Erfassungsprozess beginnen, müssen Sie [graphicscapturesession. IsSupported](/uwp/api/windows.graphics.capture.graphicscapturesession.issupported) aufrufen, um sicherzustellen, dass die Bildschirmaufnahme Funktion auf dem aktuellen Gerät unterstützt wird.

- **Initialisieren Sie Direct3D-Schnittstellen.** In diesem Beispiel wird Direct3D verwendet, um die vom Bildschirm aufgezeichneten Pixel in eine Textur zu kopieren, die als Videoframe codiert ist. Weiter unten in diesem Artikel werden die Hilfsmethoden verwendet, die zum Initialisieren der Direct3D-Schnittstellen, **CreateD3DDevice** und " **kreatesharpdxdevice**" verwendet werden.

- **Initialisieren Sie ein graphicscaptureitem-Element.** Ein [graphicscaptureitem](/uwp/api/windows.graphics.capture.graphicscaptureitem) -Element stellt ein Element auf dem Bildschirm dar, das aufgezeichnet wird, entweder ein Fenster oder den gesamten Bildschirm. Ermöglicht es dem Benutzer, ein Element auszuwählen, das erfasst werden soll, indem ein [graphicscapturepicker](/uwp/api/windows.graphics.capture.graphicscapturepicker) erstellt und [picksingleitemasync](/uwp/api/windows.graphics.capture.graphicscapturepicker.picksingleitemasync)aufgerufen wird.

- **Erstellen Sie eine Kompositions Textur.** Erstellen Sie eine Textur Ressource und eine zugeordnete renderzielansicht, die verwendet wird, um die einzelnen Video Frames zu kopieren. Diese Textur kann erst erstellt werden, nachdem das **graphicscaptureitem** -Element erstellt und die zugehörigen Dimensionen bekannt sind. Weitere Informationen zur Verwendung dieser Kompositions Textur finden Sie in der Beschreibung von **waitfornewframe** . Die Hilfsmethode zum Erstellen dieser Textur wird auch weiter unten in diesem Artikel gezeigt.

- **Erstellen Sie ein mediaencodingprofile-und videostreamdescriptor-Element.** Eine Instanz der [MediaStreamSource](/uwp/api/windows.media.core.mediastreamsource) -Klasse übernimmt Bilder, die auf dem Bildschirm aufgezeichnet werden, und codiert Sie in einen Videostream. Anschließend wird der Videostream von der [mediatranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) -Klasse in eine Videodatei transcodiert. Ein [videostreamdecriptor](/uwp/api/windows.media.core.videostreamdescriptor) stellt Codierungs Parameter (z. b. Auflösung und Framerate) für **MediaStreamSource**bereit. Die Videodatei Codierungs Parameter für **mediatranscoder** werden mit einem [mediaencodingprofile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)angegeben. Beachten Sie, dass die für die Videocodierung verwendete Größe nicht mit der Größe des erfassten Fensters identisch sein muss. um dieses Beispiel einfach zu halten, sind die Codierungs Einstellungen jedoch hart codiert, um die tatsächlichen Dimensionen des Erfassungs Elements zu verwenden.

- **Erstellen Sie die Objekte MediaStreamSource und mediatranscoder.** Wie bereits erwähnt, codiert das **MediaStreamSource** -Objekt einzelne Frames in einen Videostream. Nennen Sie den Konstruktor für diese Klasse, und übergeben Sie das im vorherigen Schritt erstellte **mediaencodingprofile** . Legen Sie die Pufferzeit auf NULL fest, und registrieren Sie die Handler für das [Start](/uwp/api/windows.media.core.mediastreamsource.starting) -und das [samplerequessierte](/uwp/api/windows.media.core.mediastreamsource.samplerequested) -Ereignis, das weiter unten in diesem Artikel dargestellt wird. Erstellen Sie als nächstes eine neue Instanz der **mediatranscoder** -Klasse, und aktivieren Sie die Hardwarebeschleunigung.

- **Erstellen einer Ausgabedatei** Der letzte Schritt in dieser Methode besteht darin, eine Datei zu erstellen, in die das Video transcodiert wird. In diesem Beispiel erstellen wir einfach eine eindeutig benannte Datei im Ordner "Videos Library" auf dem Gerät. Beachten Sie, dass Ihre APP für den Zugriff auf diesen Ordner die "Videos Library"-Funktion im App-Manifest angeben muss. Nachdem die Datei erstellt wurde, öffnen Sie Sie für Lese-und Schreibvorgänge, und übergeben Sie den resultierenden Stream an die **encodeasync** -Methode, die als nächstes angezeigt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SetupEncoding":::

## <a name="start-encoding"></a>Codierung starten
Nachdem die Hauptobjekte initialisiert wurden, wird die **encodeasync** -Methode implementiert, um den Aufzeichnungs Vorgang zu starten. Diese Methode prüft zunächst, ob Sie nicht bereits aufzeichnen, und wenn nicht, ruft Sie die Hilfsmethode **startcapture** auf, um mit dem Erfassen von Frames auf dem Bildschirm zu beginnen. Diese Methode wird weiter unten in diesem Artikel gezeigt. Als nächstes wird [preparemediastreamsourcetranscodeasync](/uwp/api/windows.media.transcoding.mediatranscoder.preparemediastreamsourcetranscodeasync) aufgerufen, um den **mediatranscoder** zum transcodieren des Videodaten Stroms bereitzustellen, der vom **MediaStreamSource** -Objekt in den ausgabedateistream erstellt wurde. dabei wird das Codierungs Profil verwendet, das im vorherigen Abschnitt erstellt wurde. Nachdem der Transcoder vorbereitet wurde, können Sie [transcodeasync](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) aufrufen, um die Transcodierung zu starten. Weitere Informationen zur Verwendung von **mediatranscoder**finden Sie unter [transcodieren von Mediendateien](./transcode-media-files.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_EncodeAsync":::

## <a name="handle-mediastreamsource-events"></a>Verarbeiten von MediaStreamSource-Ereignissen
Das **MediaStreamSource** -Objekt übernimmt Frames, die auf dem Bildschirm aufgezeichnet werden, und transformiert Sie in einen Videostream, der mithilfe von **mediatranscoder**in einer Datei gespeichert werden kann. Die Frames werden mithilfe von Handlern für die Ereignisse des Objekts an die **MediaStreamSource** übergeben.

Das [samplerequraised](/uwp/api/windows.media.core.mediastreamsource.samplerequested) -Ereignis wird ausgelöst, wenn **MediaStreamSource** für einen neuen Videoframe bereit ist. Nachdem Sie sichergestellt haben, dass wir gerade aufzeichnen, wird die Hilfsmethode **waitfornewframe** aufgerufen, um einen neuen Frame, der vom Bildschirm aufgezeichnet wird, zu erhalten. Diese Methode, die weiter unten in diesem Artikel gezeigt wird, gibt ein [ID3D11Surface](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) -Objekt zurück, das den aufgezeichneten Frame enthält. In diesem Beispiel wrappen wir die **IDirect3DSurface** -Schnittstelle in einer Hilfsklasse, die auch die Systemzeit speichert, zu der der Frame aufgezeichnet wurde. Sowohl die Frame-als auch die Systemzeit werden an die [MediaStreamSample. CreateFromDirect3D11Surface](/uwp/api/windows.media.core.mediastreamsample.createfromdirect3d11surface) Factory-Methode und das resultierende [MediaStreamSample](/uwp/api/windows.media.core.mediastreamsample) -Objekt auf die [mediastreamsourcesamplerequest. Sample](/uwp/api/windows.media.core.mediastreamsourcesamplerequest.sample) -Eigenschaft von [mediastreamsourcesamplerequestedeventargs](/uwp/api/windows.media.core.mediastreamsourcesamplerequestedeventargs)festgelegt. Auf diese Weise wird der erfasste Frame für **MediaStreamSource**bereitgestellt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceSampleRequested":::

Im-Handler für das [Start](/uwp/api/windows.media.core.mediastreamsource.starting) Ereignis rufen wir **waitfornewframe**auf, übergeben jedoch nur die Systemzeit, in der der Frame aufgezeichnet wurde, an die [mediastreamsourcestartingrequest. setactualstartposition](/uwp/api/windows.media.core.mediastreamsourcestartingrequest.setactualstartposition) -Methode, die von **MediaStreamSource** verwendet wird, um die zeitliche Steuerung der nachfolgenden Frames ordnungsgemäß zu codieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceStarting":::

## <a name="start-capturing"></a>Erfassung starten
Die in diesem Schritt gezeigte **startcapture** -Methode wird von der in einem vorherigen Schritt gezeigten Hilfsmethode **encodeasync** aufgerufen. Zuerst initialisiert diese Methode eine Reihe von Ereignis Objekten, die zum Steuern des Ablaufs des Aufzeichnungs Vorgangs verwendet werden.

- **_multithread** ist eine Hilfsklasse, die das **multithreadobjekt** der sharpdx-Bibliothek umwickelt, das verwendet wird, um sicherzustellen, dass keine anderen Threads auf die sharpdx-Textur zugreifen, während Sie kopiert werden.
- **_frameEvent** wird verwendet, um zu signalisieren, dass ein neuer Frame aufgezeichnet wurde und an **MediaStreamSource** übermittelt werden kann.
- **_closedEvent** signalisiert, dass die Aufzeichnung beendet wurde und dass wir nicht auf neue Frames warten sollten.

Das Frame Ereignis und das geschlossene Ereignis werden einem Array hinzugefügt, sodass wir auf eines dieser Elemente in der Erfassungs Schleife warten können.

Der Rest der **startcapture** -Methode richtet die Windows. Graphics. Capture-APIs ein, die die tatsächliche Bildschirm Erfassung durchführen. Zuerst wird ein Ereignis für das **captureitem. Closed** -Ereignis registriert. Als nächstes wird ein [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) erstellt, mit dem mehrere erfasste Frames gleichzeitig gepuffert werden können. Die Methode " [foratefreethread"](/uwp/api/windows.graphics.capture.direct3d11captureframepool.createfreethreaded) wird zum Erstellen des Frame Pools verwendet, sodass das [framekam](/uwp/api/windows.graphics.capture.direct3d11captureframepool.framearrived) -Ereignis für den eigenen Workerthread des Pools aufgerufen wird und nicht für den Haupt Thread der app. Als nächstes wird ein Handler für das **framekam** -Ereignis registriert. Zum Schluss wird eine [graphicscapturesession](/uwp/api/windows.graphics.capture.graphicscapturesession) für das ausgewählte **captureitem** erstellt, und die Erfassung von Frames wird durch Aufrufen von " [startcapture](/uwp/api/windows.graphics.capture.graphicscapturesession)" initiiert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_StartCapture":::

## <a name="handle-graphics-capture-events"></a>Behandeln von Grafik Aufzeichnungs Ereignissen
Im vorherigen Schritt haben wir zwei Handler für Grafik Aufzeichnungs Ereignisse registriert und einige Ereignisse eingerichtet, um die Verwaltung des Ablaufs der Erfassungs Schleife zu unterstützen.

Das **framekam** -Ereignis wird ausgelöst, wenn für **Direct3D11CaptureFramePool** ein neuer erfasster Frame verfügbar ist. Im Handler für dieses Ereignis wird [trygetnextframe](/uwp/api/windows.graphics.capture.direct3d11captureframepool.trygetnextframe) für den Absender aufgerufen, um den nächsten aufgezeichneten Frame abzurufen. Nachdem der Frame abgerufen wurde, legen wir die **_frameEvent** so fest, dass die Erfassungs Schleife weiß, dass ein neuer Frame verfügbar ist.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnFrameArrived":::

Im **Closed** -Ereignishandler signalisieren wir die **_closedEvent** , damit die Aufzeichnungs Schleife weiß, wann Sie beendet werden soll.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnClosed":::

## <a name="wait-for-new-frames"></a>Auf neue Frames warten
Die in diesem Abschnitt beschriebene Hilfsmethode " **waitfornewframe** " ist der Ort, an dem die Erfassungs Schleife stark ausgelastet ist. Beachten Sie, dass diese Methode vom **onmediastreamsourcesamplerequtry-** Ereignishandler aufgerufen wird, wenn **MediaStreamSource** bereit ist, um dem Videostream einen neuen Frame hinzuzufügen. Auf hoher Ebene kopiert diese Funktion einfach jeden Bildschirm, auf den der Bildschirm aufgezeichnet wird, von einer Direct3D-Oberfläche in eine andere, sodass Sie an die **MediaStreamSource** für die Codierung geleitet werden kann, während ein neuer Frame aufgezeichnet wird. In diesem Beispiel wird die sharpdx-Bibliothek verwendet, um den eigentlichen Kopiervorgang auszuführen.

Bevor auf einen neuen Frame gewartet wird, gibt die Methode einen beliebigen vorherigen Frame aus, der in der Klassen Variablen gespeichert ist, **_currentFrame**und setzt die **_frameEvent**zurück. Dann wartet die Methode darauf, dass entweder die **_frameEvent** oder die **_closedEvent** signalisiert werden. Wenn das Closed-Ereignis festgelegt ist, ruft die APP eine Hilfsmethode auf, um die Erfassungs Ressourcen zu bereinigen. Diese Methode wird weiter unten in diesem Artikel gezeigt.

Wenn das Frame-Ereignis festgelegt ist, wissen wir, dass der im vorherigen Schritt definierte **framekam** -Ereignishandler aufgerufen wurde, und wir beginnen mit dem Kopieren der aufgezeichneten Frame Daten in eine Direct3D 11-Oberfläche, die an die **MediaStreamSource**-Schnittstelle weitergeleitet wird.

In diesem Beispiel wird die Hilfsklasse " **surfacewithinfo**" verwendet, die es uns einfach ermöglicht, den Videoframe und die Systemzeit des Frames, beides von **MediaStreamSource** , als einzelnes Objekt zu übergeben. Der erste Schritt des Frame Kopiervorgangs besteht darin, diese Klasse zu instanziieren und die Systemzeit festzulegen.

Die nächsten Schritte sind der Teil dieses Beispiels, der speziell auf die sharpdx-Bibliothek basiert. Die hier verwendeten Hilfsfunktionen werden am Ende dieses Artikels definiert. Zuerst verwenden wir das **multithreadlock** , um sicherzustellen, dass keine anderen Threads auf den Video Frame Puffer zugreifen, während wir die Kopie erstellen. Als Nächstes rufen wir die Hilfsmethode **CreateSharpDXTexture2D** auf, um ein sharpdx **Texture2D** -Objekt aus dem Videoframe zu erstellen. Dies ist die Quell Textur für den Kopiervorgang. 

Im nächsten Schritt kopieren wir aus dem im vorherigen Schritt erstellten **Texture2D** -Objekt in die Kompositionsstruktur, die wir zuvor in diesem Prozess erstellt haben. Diese Kompositions Textur fungiert als Auslagerungs Puffer, sodass der Codierungsprozess in den Pixeln ausgeführt werden kann, während der nächste Frame aufgezeichnet wird. Um die Kopie auszuführen, löschen wir die renderzielansicht, die der Kompositions Textur zugeordnet ist, und definieren dann den Bereich innerhalb der Textur, den wir kopieren möchten (in diesem Fall die gesamte Textur). Anschließend wird **copysubresourceregion** aufgerufen, um die Pixel tatsächlich in die Kompositions Textur zu kopieren.

Wir erstellen eine Kopie der Textur Beschreibung, die beim Erstellen der Ziel Textur verwendet werden soll. die Beschreibung wird jedoch geändert, wobei " **bindflags** " auf **renderTarget** festgelegt wird, sodass die neue Textur Schreibzugriff hat. Wenn Sie " **cpuaccessflags** " auf " **None** " festlegen, kann das System den Kopiervorgang optimieren. Die Textur Beschreibung wird zum Erstellen einer neuen Textur Ressource verwendet, und die Kompositions Textur Ressource wird mithilfe eines Aufrufes **copyresource**in diese neue Ressource kopiert. Schließlich wird **CreateDirect3DSurfaceFromSharpDXTexture** aufgerufen, um das **IDirect3DSurface** -Objekt zu erstellen, das von dieser Methode zurückgegeben wird.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_WaitForNewFrame":::

## <a name="stop-capture-and-clean-up-resources"></a>Erfassung und Bereinigung von Ressourcen Abbrechen
Die Methode zum **Abbrechen** bietet eine Möglichkeit, den Aufzeichnungs Vorgang zu unterbinden. Ihre APP kann diese Programm gesteuert oder als Reaktion auf eine Benutzerinteraktion (z. b. einen Klick auf die Schaltfläche) abrufen. Diese Methode legt einfach den **_closedEvent**fest. Die in den vorherigen Schritten definierte **waitfornewframe** -Methode sucht nach diesem Ereignis und fährt, falls festgelegt, den Aufzeichnungs Vorgang herunter.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Stop":::

Die **Bereinigungs** Methode wird verwendet, um die Ressourcen, die während des Kopiervorgangs erstellt wurden, ordnungsgemäß zu verwerfen. Dies schließt Folgendes ein:

- Das von der Erfassungs Sitzung verwendete **Direct3D11CaptureFramePool** -Objekt.
- **Graphicscapturesession** und **graphicscaptureitem**
- Die Direct3D-und sharpdx-Geräte
- Die im Kopiervorgang verwendete sharpdx-Textur-und renderzielansicht.
- Die **Direct3D11CaptureFrame** , die zum Speichern des aktuellen Frames verwendet werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Cleanup":::

## <a name="helper-wrapper-classes"></a>Wrapper Klassen für Helper
Die folgenden Hilfsklassen wurden definiert, um den Beispielcode in diesem Artikel zu unterstützen.

Die **multithreadlock** -Hilfsklasse umschließt die sharpdx- **multithreadklasse** , mit der sichergestellt wird, dass andere Threads beim Kopieren nicht auf die Textur Ressourcen zugreifen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_MultithreadLock":::

" **Surfacewithinfo** " wird verwendet, um eine **IDirect3DSurface** mit einer **systemrelativetime** zuzuordnen, die den aufgezeichneten Frame und die erfasste Zeit darstellt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SurfaceWithInfo":::

## <a name="direct3d-and-sharpdx-helper-apis"></a>Direct3D-und sharpdx-Hilfsobjekte
Die folgenden hilfsapis werden definiert, um die Erstellung von Direct3D-und sharpdx-Ressourcen zu abstrahieren. Eine ausführliche Erläuterung dieser Technologien ist nicht Gegenstand dieses Artikels, aber der Code wird hier bereitgestellt, damit Sie den in der exemplarischen Vorgehensweise gezeigten Beispielcode implementieren können.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/Direct3D11Helpers.cs" id="snippet_Direct3D11Helpers":::

## <a name="see-also"></a>Weitere Informationen

* [Windows. Graphics. Capture-Namespace](/uwp/api/windows.graphics.capture)
* [Bildschirmaufnahme](screen-capture.md)