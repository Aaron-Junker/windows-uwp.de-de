---
ms.assetid: ''
description: In diesem Artikel erfahren Sie, wie Sie Videos aus mehreren Quellen simulataneron in einer einzelnen Datei mit mehreren eingebetteten Videospuren erfassen.
title: Erfassen von mehreren Quellen mit „MediaFrameSourceGroup“
ms.date: 09/12/2017
ms.topic: article
keywords: Windows 10, UWP, Erfassung, Video
ms.localizationpriority: medium
ms.openlocfilehash: 6d40e75dd88b84eb5d7244a2ad3ed3d605c17e0b
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362874"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Erfassen von mehreren Quellen mit „MediaFrameSourceGroup“

In diesem Artikel wird gezeigt, wie Sie Videos aus mehreren Quellen gleichzeitig in einer einzelnen Datei mit mehreren eingebetteten Videospuren erfassen. Beginnend mit RS3 können Sie mehrere **[videostreamdescriptor](/uwp/api/windows.media.core.videostreamdescriptor)** -Objekte für ein einzelnes **[mediaencodingprofile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**-Objekt angeben. Dies ermöglicht es Ihnen, mehrere Streams gleichzeitig in einer einzelnen Datei zu codieren. Die in diesem Vorgang codierten Videostreams müssen in einer einzelnen **[mediaframesourcegroup](/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** enthalten sein, die eine Reihe von Kameras auf dem aktuellen Gerät angibt, die gleichzeitig verwendet werden können. 

Informationen zur Verwendung von **[mediaframesourcegroup](/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** mit der **[mediaframereader](/uwp/api/windows.media.capture.frames.mediaframereader)** -Klasse, um Szenarios in Echtzeit-Maschinelles sehen mit mehreren Kameras zu ermöglichen, finden Sie unter [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md).

Der restliche Artikel führt Sie durch die Schritte zum Aufzeichnen von Videos von zwei Farbkameras in eine einzelne Datei mit mehreren Videospuren.

## <a name="find-available-sensor-groups"></a>Suchen nach verfügbaren Sensor Gruppen
Eine **mediaframesourcegroup** stellt eine Auflistung von Frame Quellen dar (in der Regel Kameras), auf die simuliert werden kann. Der Satz verfügbarer Frame Quell Gruppen unterscheidet sich für jedes Gerät. der erste Schritt in diesem Beispiel besteht darin, die Liste der verfügbaren Frame Quell Gruppen zu ermitteln und eine zu suchen, die die für das Szenario erforderlichen Kameras enthält. in diesem Fall sind zwei Farbkameras erforderlich.

Die **[mediaframesourcegroup. findallasync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** -Methode gibt alle auf dem aktuellen Gerät verfügbaren Quell Gruppen zurück. Jede zurückgegebene **mediaframesourcegroup** enthält eine Liste von **[mediaframesourceinfo](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** -Objekten, die jede Frame Quelle in der Gruppe beschreibt. Eine LINQ-Abfrage wird verwendet, um eine Quell Gruppe zu suchen, die zwei Farbkameras enthält, eine auf dem Vordergrund und eine auf der Rückseite. Ein anonymes Objekt, das die ausgewählte **mediaframesourcegroup** und die **mediaframesourceinfo** für jede Farbkamera enthält, wird zurückgegeben. Anstatt die LINQ-Syntax zu verwenden, können Sie stattdessen die einzelnen Gruppen durchlaufen und dann alle **mediaframesourceingefo** -Elemente suchen, um nach einer Gruppe zu suchen, die Ihre Anforderungen erfüllt.

Beachten Sie, dass nicht jedes Gerät eine Quell Gruppe mit zwei Farbkameras enthalten wird. Daher sollten Sie überprüfen, ob eine Quell Gruppe gefunden wurde, bevor Sie versuchen, Video aufzuzeichnen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetMultiRecordFindSensorGroups":::

## <a name="initialize-the-mediacapture-object"></a>Initialisieren des MediaCapture-Objekts
Die **[mediacapture](/uwp/api/windows.media.capture.mediacapture)** -Klasse ist die primäre Klasse, die für die meisten audiovorgänge, Video-und Foto Erfassungs Vorgänge in UWP-Apps verwendet wird. Initialisieren Sie das-Objekt durch Aufrufen von **[initializeasync](/uwp/api/windows.media.capture.mediacapture.InitializeAsync)**, und übergeben Sie ein **[mediacaptureinitializationsettings](/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** -Objekt, das Initialisierungs Parameter enthält. In diesem Beispiel ist die einzige angegebene Einstellung die **[sourcegroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** -Eigenschaft, die auf die **mediaframesourcegroup** festgelegt wird, die im vorherigen Codebeispiel abgerufen wurde.

Informationen zu anderen Vorgängen, die Sie mit **mediacapture** und anderen UWP-App-Features für die Erfassung von Medien ausführen können, finden Sie unter [Kamera](camera.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetMultiRecordInitMediaCapture":::

## <a name="create-a-mediaencodingprofile"></a>Erstellen eines mediaencodingprofile
Die Media **[encodingprofile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** -Klasse teilt der Medien Erfassungs Pipeline mit, wie erfasste Audiodaten und Videos beim Schreiben in eine Datei codiert werden sollen. Für typische Aufzeichnungs-und Transcodierungs Szenarien stellt diese Klasse einen Satz statischer Methoden zum Erstellen allgemeiner Profile bereit, wie z. b. " **[kreateavi](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** " und " **[CreateMp3](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)**". In diesem Beispiel wird ein Codierungs Profil manuell mithilfe eines MPEG4-Containers und der H264 Video-Codierung erstellt. Video Codierungs Einstellungen werden mithilfe eines **[videoencodingproperties](/uwp/api/windows.media.mediaproperties.videoencodingproperties)** -Objekts angegeben. Für jede Farbkamera, die in diesem Szenario verwendet wird, wird ein **videostreamdescriptor** -Objekt konfiguriert. Der Deskriptor wird mit dem **videoencodingproperties** -Objekt erstellt, das die Codierung angibt. Die **[Label](/uwp/api/windows.media.core.videostreamdescriptor.Label)** -Eigenschaft von **videostreamdescriptor** muss auf die ID der Medien Frame Quelle festgelegt werden, die im Stream aufgezeichnet wird. Auf diese Weise weiß die Aufzeichnungs Pipeline, welche Datenstrom Deskriptoren und Codierungs Eigenschaften für die einzelnen Kamera verwendet werden sollen. Die ID der Frame Quelle wird von den **[mediaframesourceinfo](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** -Objekten, die im vorherigen Abschnitt gefunden wurden, bei Auswahl von " **mediaframesourcegroup** " verfügbar gemacht.


Ab Windows 10, Version 1709, können Sie mehrere Codierungs Eigenschaften für ein **mediaencodingprofile** festlegen, indem Sie **[setvideotracks](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)** aufrufen. Sie können die Liste der videostreamdeskriptoren abrufen, indem Sie **[getvideotracks](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)** aufrufen. Beachten Sie Folgendes: Wenn Sie die **[Video](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)** -Eigenschaft festlegen, die einen einzelnen Datenstrom Deskriptor speichert, wird die durch den Aufruf von **setvideotrack** festgelegte deskriptorliste durch eine Liste ersetzt, die den einzelnen Deskriptor enthält, den Sie angegeben haben.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetMultiRecordMediaEncodingProfile":::

### <a name="encode-timed-metadata-in-media-files"></a>Codieren von zeitgesteuerten Metadaten in Mediendateien

Ab Windows 10, Version 1803, können Sie zusätzlich zu Audiodaten und Video zeitgesteuerte Metadaten in eine Mediendatei codieren, für die das Datenformat unterstützt wird. Beispielsweise kann die Datei "GoPro Metadata" (gpmd) in MP4-Dateien gespeichert werden, um den geografischen Standort zu übermitteln, der mit einem Videostream korreliert 

Codierungs Metadaten verwenden ein Muster, das für die Codierung von Audiodaten oder Videos parallel ist. Die [**TimedMetadataEncodingProperties**](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties) -Klasse beschreibt den Typ, den Untertyp und die Codierungs Eigenschaften der Metadaten, wie z. b. **videoencodingproperties** für Video. Der [**TimedMetadataStreamDescriptor**](/uwp/api/windows.media.core.timedmetadatastreamdescriptor) identifiziert einen Metadatenstream, ebenso wie der **videostreamdescriptor** für Videostreams.  

Im folgenden Beispiel wird gezeigt, wie ein **TimedMetadataStreamDescriptor** -Objekt initialisiert wird. Zuerst wird ein **TimedMetadataEncodingProperties** -Objekt erstellt, und der **Untertyp** wird auf eine GUID festgelegt, die den Typ der Metadaten identifiziert, die in den Stream eingeschlossen werden. In diesem Beispiel wird die GUID für die GoPro-Metadaten (gpmd) verwendet. Die [**setformatuserdata**](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) -Methode wird aufgerufen, um Format spezifische Daten festzulegen. Bei MP4-Dateien werden die Format spezifischen Daten im Feld sampledescription (sampledescription Box, STSD) gespeichert. Anschließend wird ein neues **TimedMetadataStreamDescriptor** aus den Codierungs Eigenschaften erstellt. Die Eigenschaften " **Label** " und " **Name** " werden festgelegt, um den zu codierenden Stream zu identifizieren. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetGetStreamDescriptor":::

Aufrufen von [**mediaencodingprofile. SetTimedMetadataTracks**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks) , um den metadatenstreamdeskriptor zum Codierungs Profil hinzuzufügen. Das folgende Beispiel zeigt eine Hilfsmethode, die zwei videostreamdeskriptoren, einen audiostreamdeskriptor und einen zeitgesteuerten metadatenstreamdeskriptor annimmt und ein **mediaencodingprofile** -Objekt zurückgibt, das zum Codieren der Streams verwendet werden kann.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetGetMediaEncodingProfile":::

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Aufzeichnen mit dem Multistream mediaencodingprofile
Der letzte Schritt in diesem Beispiel besteht darin, die Video Erfassung durch Aufrufen von **[startrecordtostoragefileasync](/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)** und durch Übergabe der **storagefile-Datei** , in die das erfasste Medium geschrieben wird, und des im vorherigen Codebeispiel erstellten **mediaencodingprofile** zu initiieren. Nachdem Sie einige Sekunden gewartet haben, wird die Aufzeichnung mit einem Aufrufen von **[stoprecordasync](/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)** beendet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetMultiRecordToFile":::

Wenn der Vorgang abgeschlossen ist, wird eine Videodatei erstellt, die das Video enthält, das von den einzelnen Kameras als separater Stream in der Datei codiert wurde. Informationen zur Wiedergabe von Mediendateien, die mehrere Videospuren enthalten, finden Sie unter [Medienelemente, Wiedergabelisten und Spuren](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Verarbeiten von Medienframes mit „MediaFrameReader“](process-media-frames-with-mediaframereader.md)
* [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md)


 

 
