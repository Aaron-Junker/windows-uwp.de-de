---
ms.assetid: ''
description: Dieser Artikel zeigt Ihnen, wie Sie Videos aus mehreren Quellen gleichzeitig in eine einzige Datei mit mehreren eingebetteten Videospuren aufnehmen können.
title: Erfassen von mehreren Quellen mit MediaFrameSourceGroup
ms.date: 09/12/2017
ms.topic: article
keywords: Windows 10, Uwp, Aufnahme, Video
ms.localizationpriority: medium
ms.openlocfilehash: c474221769bf3aec6e32c80f21386ac1ca2620ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636615"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Erfassen von mehreren Quellen mit MediaFrameSourceGroup

Dieser Artikel zeigt Ihnen, wie Sie Videos aus mehreren Quellen gleichzeitig in eine einzige Datei mit mehreren eingebetteten Videospuren aufnehmen können. Ab RS3 können Sie mehrere **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)**-Objekte für ein einzelnes **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** angeben. Dadurch können Sie mehrere Streams gleichzeitig in eine einzige Datei kodieren. Die Videoströme, die in diesem Vorgang kodiert werden, müssen in einer einzigen **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** enthalten sein, die eine Reihe von Kameras auf dem aktuellen Gerät angibt, die gleichzeitig verwendet werden können. 

Weitere Informationen zur Verwendung von **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** in Verbindung mit der **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)**-Klasse zur Aktivierung von Echtzeit-Szenarien für Computer-Vision, die mehrere Kameras verwenden, finden Sie unter [Verarbeiten von Medienframes mit MediaFrameReader](process-media-frames-with-mediaframereader.md).

Der Rest dieses Artikels wird Sie durch die Schritte der Aufnahme von Videos von zwei Farbkameras zu einer einzigen Datei mit mehreren Videospuren führen.

## <a name="find-available-sensor-groups"></a>Verfügbare Sensorgruppen finden
Eine **MediaFrameSourceGroup** stellt eine Sammlung von Bildquellen, typischerweise Kameras, dar, auf die gleichzeitig zugegriffen werden kann. Der erste Schritt in diesem Beispiel besteht also darin, die Liste der verfügbaren Bildquellengruppen zu erhalten und eine zu finden, die die notwendigen Kameras für das Szenario enthält, das in diesem Fall zwei Farbkameras benötigt.

Die Methode **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** liefert alle auf dem aktuellen Gerät verfügbaren Quellgruppen zurück. Jede zurückgegebene **MediaFrameSourceGroup** hat eine Liste von **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)**-Objekten, die jede Frame-Quelle in der Gruppe beschreibt. Eine Linq-Abfrage wird verwendet, um eine Quellengruppe zu finden, die zwei Farbkameras enthält, eine auf der Vorderseite und eine auf der Rückseite. Für jede Farbkamera wird ein anonymes Objekt zurückgegeben, das die ausgewählte **MediaFrameSourceGroup** und die **MediaFrameSourceInfo** enthält. Anstatt den Linq-Syntax zu verwenden, können Sie jede Gruppe und dann jede **MediaFrameSourceInfo** durchgehen, um nach einer Gruppe zu suchen, die Ihren Anforderungen entspricht.

Beachten Sie, dass nicht jedes Gerät eine Quellgruppe mit zwei Farbkameras enthält. Überprüfen Sie daher, ob eine Quellgruppe gefunden wurde, bevor Sie versuchen, ein Video aufzunehmen.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialisieren des MediaCapture-Objekts
Die **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)**-Klasse ist die primäre Klasse, die für die meisten Audio-, Video- und Foto-Capture-Operationen in UWP-Anwendungen verwendet wird. Initialisieren Sie das Objekt durch Aufruf von **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.InitializeAsync)**, indem Sie ein **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)**-Objekt übergeben, das Initialisierungsparameter enthält. In diesem Beispiel ist die einzige angegebene Einstellung die **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)**-Eigenschaft, die auf die **MediaFrameSourceGroup** gesetzt ist, die im vorherigen Code-Beispiel abgerufen wurde.

Weitere Informationen zu anderen Operationen, die Sie mit **MediaCapture** und anderen UWP-Funktionen für das Capturing von Medien durchführen können, finden Sie unter [Kamera](camera.md).

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>Erstellen eines MediaEncodingProfiles
Die Klasse **[MediaEncodingProfil](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** gibt der Pipeline für die Medienerfassung an, wie Audio- und Video-Captures beim Schreiben in eine Datei kodiert werden sollen. Für typische Capture- und Transcoding-Szenarien bietet diese Klasse eine Reihe statischer Methoden zur Erstellung gemeinsamer Profile, wie **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** und **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)**. Für dieses Beispiel wird manuell ein Kodierungsprofil mit einem Mpeg4-Container und H264-Videokodierung erstellt. Die Videocodierungseinstellungen werden über ein **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)**-Objekt festgelegt. Für jede in diesem Szenario verwendete Farbkamera wird ein **VideoStreamDescriptor**-Objekt konfiguriert. Der Deskriptor wird mit dem **VideoEncodingProperties**-Objekt konstruiert, das die Kodierung angibt. Die **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor.Label)**-Eigenschaft des **VideoStreamDescriptors** muss auf die ID der Medien-Frame-Quelle gesetzt werden, die in den Stream aufgenommen wird. So weiß die Capture-Pipeline, welche Stream-Deskriptor- und Encoding-Eigenschaften für jede Kamera verwendet werden sollten. Die ID der Bildquelle wird von den **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)**-Objekten, die im vorherigen Abschnitt gefunden wurden, angezeigt, wenn eine **MediaFrameSourceGroup** ausgewählt wurde.


Ab Windows 10, Version 1709, können Sie mehrere Encoding-Eigenschaften auf einem **MediaEncodingProfil** einstellen, indem Sie **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)** aufrufen. Sie können die Liste der Videostream-Deskriptoren abrufen, indem Sie **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)** aufrufen. Beachten Sie, dass beim Festlegen der **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)**-Eigenschaft, die einen einzelnen Stream-Deskriptor speichert, die von Ihnen über das Aufrufen von **SetVideoTracks** festgelegt Deskriptor-Liste durch eine Liste mit dem einzelnen angegebenen Deskriptor ersetzt wird.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

### <a name="encode-timed-metadata-in-media-files"></a>Codieren von zeitgesteuerten Metadaten in Mediendateien

Ab Windows 10, Version 1803, können Sie neben Audio und Video ebenfalls Metadaten in einer Mediendatei codieren, dessen Datenformat unterstützt wird. GoPro-Metadaten (Gpmd) können z. B. in MP4-Dateien gespeichert werden, um den geografischen Standort, der mit einem Videostream korrelierte ist, zu vermitteln. 

Die Codierung von Metadaten verwendet ein Muster, das parallel zur Codierung von Audio oder Video ist. Die [**TimedMetadataEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties)-Klasse beschreibt den Typ, Untertyp und die Codierungseigenschaften der Metadaten, wie **VideoEncodingProperties** für Videos. Der [**TimedMetadataStreamDescriptor**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) identifiziert einen Metadatenstream, wie **VideoStreamDescriptor** für Videostreams.  

Das folgende Beispiel zeigt, wie Sie ein **TimedMetadataStreamDescriptor**-Objekt initialisieren. Zuerst wird ein **TimedMetadataEncodingProperties**-Objekt erstellt, und der **Untertyp** auf GUID festgelegt, der den Typ der Metadaten identifiziert, die in den Datenstrom aufgenommen werden. Dieses Beispiel verwendet GUID für GoPro-Metadaten (Gpmd). Die [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata)-Methode wird aufgerufen, um formatspezifische Daten festzulegen. Für MP4-Dateien werden die formatspezifischen Daten im Feld SampleDescription (Stsd) gespeichert. Als Nächstes wird ein neuer **TimedMetadataStreamDescriptor** aus den Codierungseigenschaften erstellt. Die Eigenschaften **Bezeichnung** und **Name** werden festgelegt, um den Stream, der codiert werden muss, zu identifizieren. 

[!code-cs[GetStreamDescriptor](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetStreamDescriptor)]

Rufen Sie [ **MediaEncodingProfile.SetTimedMetadataTracks** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks) den Metadaten-Stream-Deskriptor für das encoding-Profil hinzufügen. Das folgende Beispiel zeigt eine Hilfsmethode, die zwei Videostream-Deskriptoren akzeptiert, einen Audiodatenstrom-Deskriptor, und einen zeitgesteuerten Metadatenstream-Deskriptor und ein **MediaEncodingProfile** zurückgibt, das zum Codieren von Streams verwendet werden kann.

[!code-cs[GetMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Aufzeichnung mit dem Multi-Stream-MediaEncodingProfil
Der letzte Schritt in diesem Beispiel ist es, die Videoaufnahme zu initiieren, indem Sie **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)** aufrufen, **StorageFile** übergeben, das in die das aufgenommene Medium geschrieben wird, und das **MediaEncodingProfil**, das im vorherigen Code-Beispiel erstellt wurde. Nach einigen Sekunden Wartezeit wird die Aufnahme mit einem Anruf bei **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)** gestoppt.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

Wenn der Vorgang abgeschlossen ist, wurde eine Videodatei erstellt, die das von jeder Kamera aufgenommene Video als separaten Stream in der Datei enthält. Informationen zur Wiedergabe von Mediendateien mit mehreren Videospuren finden Sie unter [Medienelemente, Wiedergabelisten und Tracks](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Verarbeiten von Medien-Frames mit MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Medienobjekte, Wiedergabelisten und Spuren](media-playback-with-mediasource.md)


 

 




