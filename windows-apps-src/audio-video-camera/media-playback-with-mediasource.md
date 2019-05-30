---
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: In diesem Artikel wird erläutert, wie Sie die MediaSource-Klasse verwenden, die allgemein zum Verweisen auf Medien aus verschiedenen Quellen (etwa lokale Dateien oder Remotedateien) sowie zum Wiedergeben dieser Medien verwendet wird und ein gemeinsames Modell für den Mediendatenzugriff verfügbar macht – unabhängig vom zugrunde liegenden Medienformat.
title: Medienelemente, Wiedergabelisten und Titel
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 472e163344c8cc2fdea3dd639383bb1dac84a2f4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361589"
---
# <a name="media-items-playlists-and-tracks"></a>Medienelemente, Wiedergabelisten und Titel


 In diesem Artikel wird erläutert, wie Sie die [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource)-Klasse verwenden, die allgemein zum Verweisen auf Medien aus verschiedenen Quellen (etwa lokale Dateien oder Remotedateien) sowie zum Wiedergeben dieser Medien verwendet wird und ein gemeinsames Modell für den Mediendatenzugriff verfügbar macht – unabhängig vom zugrunde liegenden Medienformat. Die [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem)-Klasse erweitert den Funktionsumfang von **MediaSource** und ermöglicht die Verwaltung und Auswahl mehrerer Audio-, Video- und Metadatentitel in einem Medienelement. [**MediaPlaybackList** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList) können Sie Listen der Wiedergabe von Medien für eine oder mehrere Wiedergabe-Elemente erstellen.


## <a name="create-and-play-a-mediasource"></a>Erstellen und Wiedergeben eines MediaSource-Elements

Erstellen Sie eine neue Instanz von **MediaSource**, indem Sie eine der von der Klasse verfügbar gemachten Factory-Methoden aufrufen:

-   [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromadaptivemediasource)
-   [**CreateFromIMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromimediasource)
-   [**CreateFromMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediastreamsource)
-   [**CreateFromMseStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommsestreamsource)
-   [**CreateFromStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)
-   [**CreateFromStream**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstream)
-   [**CreateFromStreamReference**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstreamreference)
-   [**CreateFromUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromuri)
-   [**CreateFromDownloadOperation**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

Nach dem Erstellen einer **MediaSource** können Sie diese mit einer [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) durch Festlegen der [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source)-Eigenschaft abspielen. Ab Windows 10, Version 1607, können Sie einen **MediaPlayer** einem [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) durch Aufrufen von [**SetMediaPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) zuweisen, um die Media Player-Inhalte in einer XAML-Seite zu rendern. Dies ist die bevorzugte Methode gegenüber der Verwendung von **MediaElement**. Weitere Informationen zur Verwendung des **MediaPlayer** finden Sie unter [**Wiedergeben von Audio- und Videoinhalten mit MediaPlayer**](play-audio-and-video-with-mediaplayer.md).

Das folgende Beispiel zeigt die Wiedergabe einer vom Benutzer ausgewählten Mediendatei in einem **MediaPlayer** mit **MediaSource**.

Für dieses Szenario müssen die Namespaces [**Windows.Media.Core**](https://docs.microsoft.com/uwp/api/Windows.Media.Core) und [**Windows.Media.Playback**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) eingeschlossen werden.

[!code-cs[Using](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetUsing)]

Deklarieren Sie eine Membervariable vom Typ **MediaSource**. Für die Beispiele in diesem Artikel wird die Medienquelle als Klassenmember deklariert, um den Zugriff über mehrere Orte zu ermöglichen.

[!code-cs[DeclareMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

Deklarieren Sie eine Variable zum Speichern des **MediaPlayer** Objekts. Wenn die Medieninhalte in XAML gerendert werden sollen, fügen Sie der Seite ein **MediaPlayerElement**-Steuerelements hinzu.

[!code-cs[DeclareMediaPlayer](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-xml[MediaPlayerElement](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMediaPlayerElement)]

Verwenden Sie ein [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)-Element, um dem Benutzer das Auswählen einer wiederzugebenden Mediendatei zu ermöglichen. Initialisieren Sie mit dem von der [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync)-Auswahlmethode zurückgegebenen [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt durch Aufrufen von [**MediaSource.CreateFromStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile) ein neues Medienobjekt. Legen Sie abschließend durch Aufrufen der [**SetPlaybackSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.setplaybacksource)-Methode die Medienquelle als Wiedergabequelle für das **MediaElement**-Element fest.

[!code-cs[PlayMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

Standardmäßig beginnt **MediaPlayer** nicht automatisch mit der Wiedergabe, wenn die Medienquelle festgelegt ist. Sie können die Wiedergabe manuell durch Aufrufen von [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.play) wiedergeben.

[!code-cs[Play](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

Sie können auch die [**AutoPlay**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.autoplay)-Eigenschaft des **MediaPlayer** auf „ true“ festlegen, um dem Player mitzuteilen, dass er mit der Wiedergabe beginnen soll, sobald die Medienquelle festgelegt ist.

[!code-cs[AutoPlay](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAutoPlay)]

### <a name="create-a-mediasource-from-a-downloadoperation"></a>Erstellen einer MediaSource aus einer DownloadOperation
Ab Windows, Version 1803, können Sie ein **MediaSource**-Objekt von einer **DownloadOperation** erstellen.

[!code-cs[CreateMediaSourceFromDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetCreateMediaSourceFromDownload)]

Beachten Sie, dass Sie zwar eine **MediaSource** von einem Download erstellen können ohne ihn zu starten oder seine **IsRandomAccessRequired**-Eigenschaft auf "true" festzulegen, allerdings müssen beide Schritte vor dem Hinzufügen der **MediaSource**  zum **MediaPlayer** oder **MediaPlayerElement** für die Wiedergabe ausgeführt werden.

[!code-cs[StartDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetStartDownload)]


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>Behandeln mehrerer Audio-, Video- und Metadatentitel mit „MediaPlaybackItem“

Die Wiedergabe mithilfe eines [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource)-Elements ist praktisch, da es allgemein die Wiedergabe von Medien aus unterschiedlichen Quellen ermöglicht. Bei Erstellung eines [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) aus der **MediaSource** stehen dagegen stärker erweiterte Verhaltensweisen zur Verfügung. Unter anderem können Sie damit auf mehrere Audio-, Video- und Datentitel eines Medienelements zugreifen und die Titel verwalten.

Deklarieren Sie eine Variable zum Speichern Ihres **MediaPlaybackItem**-Elements.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

Erstellen Sie ein **MediaPlaybackItem**-Element, indem Sie den Konstruktor aufrufen und ein initialisiertes **MediaSource**-Objekt übergeben.

Wenn Ihre App mehrere Audio-, Video- oder Datentitel in einem Medienwiedergabeelement unterstützt, registrieren Sie Ereignishandler für das Ereignis [**AudioTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged), [**VideoTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged) oder [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged).

Legen Sie abschließend die Wiedergabequelle des **MediaElement**- oder **MediaPlayer**-Elements auf Ihr **MediaPlaybackItem**-Element fest.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

> [!NOTE] 
> Ein **MediaSource**-Element kann immer nur einem einzelnen **MediaPlaybackItem**-Element zugeordnet werden. Wenn Sie nach der Erstellung eines quellenbasierten **MediaPlaybackItem** -Elements versuchen, auf der Grundlage der gleichen Quelle ein weiteres Wiedergabeelement zu erstellen, tritt ein Fehler auf. Außerdem gilt: Nach der Erstellung eines auf einer Medienquelle basierenden **MediaPlaybackItem**-Elements können Sie das **MediaSource**-Objekt nicht direkt als Quelle für ein **MediaPlayer**-Element festlegen, sondern müssen stattdessen das **MediaPlaybackItem**-Element verwenden.

Das [**VideoTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged)-Ereignis wird ausgelöst, wenn ein **MediaPlaybackItem**-Element mit mehreren Videotiteln als Wiedergabequelle zugewiesen wurde, und kann erneut ausgelöst werden, wenn sich die Liste mit den Videotiteln für das Element ändert. Der Handler für dieses Ereignis ermöglicht die Aktualisierung der Benutzeroberfläche, damit der Benutzer zwischen den verfügbaren Titeln wechseln kann. In diesem Beispiel wird zum Anzeigen der verfügbaren Videotitel ein [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)-Element verwendet.

[!code-xml[VideoComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetVideoComboBox)]

Durchlaufen Sie im **VideoTracksChanged**-Handler alle Titel, die in der [**VideoTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.videotracks)-Liste des Wiedergabeelements enthalten sind. Für die einzelnen Titel wird jeweils ein neues [**ComboBoxItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBoxItem)-Element erstellt. Falls der Titel noch nicht beschriftet ist, wird auf der Grundlage des Titelindex eine Beschriftung generiert. Die [**Tag**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.tag)-Eigenschaft des Kombinationsfeldelements wird zur späteren Identifizierung auf den Titelindex festgelegt. Danach wird das Element dem Kombinationsfeld hinzugefügt. Beachten Sie, dass diese Vorgänge innerhalb eines [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows)-Aufrufs ausgeführt werden, da alle UI-Änderungen im UI-Thread erfolgen müssen und dieses Ereignis in einem anderen Thread ausgelöst wird.

[!code-cs[VideoTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

Im [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Handler für das Kombinationsfeld wird der Titelindex aus der **Tag**-Eigenschaft des ausgewählten Elements abgerufen. Das Festlegen der [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackvideotracklist.selectedindex)-Eigenschaft der [**VideoTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.videotracks)-Liste des Medienwiedergabeelements bewirkt, dass das **MediaElement**- oder **MediaPlayer**-Element beim aktiven Videotitel zum angegebenen Index wechselt.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

Die Verwaltung von Medienelementen mit mehreren Audiotiteln funktioniert genau wie bei den Videotiteln. Behandeln Sie das [**AudioTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged)-Ereignis, um Ihre Benutzeroberfläche mit den Audiotiteln aus der [**AudioTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks)-Liste des Wiedergabeelements zu aktualisieren. Wenn der Benutzer einen Audiotitel auswählt, legen Sie die [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackaudiotracklist.selectedindex)-Eigenschaft der **AudioTracks**-Liste fest, damit das **MediaElement**- oder **MediaPlayer**-Element beim aktiven Audiotitel zum angegebenen Index wechselt.

[!code-xml[AudioComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

Neben Audio und Video kann ein **MediaPlaybackItem**-Objekt auch [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.TimedMetadataTrack)-Objekte enthalten. Ein zeitgesteuerter Metadatentitel kann Untertitel, Beschriftungstext oder benutzerdefinierte proprietäre Daten Ihrer App enthalten. Ein zeitgesteuerter Metadatentitel enthält eine Liste mit Markern. Diese werden durch Objekte dargestellt, die von [**IMediaCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.IMediaCue) erben (etwa [**DataCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.DataCue) oder [**TimedTextCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.TimedTextCue)). Jeder Marker hat eine Anfangszeit und eine Dauer. Diese bestimmen den Aktivierungszeitpunkt und die Aktivierungsdauer des jeweiligen Markers.

Die zeitgesteuerten Metadatentitel für ein Medienelement können ähnlich wie Audio- und Videotitel durch Behandeln des [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged)-Ereignisses eines **MediaPlaybackItem**-Elements ermittelt werden. Bei zeitgesteuerten Metadatentiteln möchte der Benutzer aber unter Umständen mehrere Metadatentitel gleichzeitig aktivieren. Außerdem empfiehlt sich je nach App-Szenario unter Umständen eine automatische Aktivierung oder Deaktivierung von Metadatentiteln. Dieses Beispiel fügt zur Veranschaulichung ein [**ToggleButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton)-Element für die einzelnen Metadatentitel in einem Medienelement hinzu, sodass der Benutzer den jeweiligen Titel aktivieren und deaktivieren kann. Die **Tag**-Eigenschaft der einzelnen Schaltflächen wird auf den Index des zugehörigen Metadatentitels festgelegt, sodass sie bei Betätigung der Schaltfläche identifiziert werden können.

[!code-xml[MetaStackPanel](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

Da gleichzeitig mehrere Metadatentitel aktiv sein können, legen Sie nicht einfach den aktiven Index für die Metadatentitelliste fest. Rufen Sie stattdessen die [**SetPresentationMode**](https://docs.microsoft.com/previous-versions/windows/dn986977(v=win.10))-Methode des **MediaPlaybackItem**-Objekts auf. Übergeben Sie dabei den Index des umzuschaltenden Titels, und geben Sie dann einen Wert aus der [**TimedMetadataTrackPresentationMode**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.TimedMetadataTrackPresentationMode)-Enumeration an. Der gewählte Darstellungsmodus hängt von der Implementierung Ihrer App ab. In diesem Beispiel wird der Metadatentitel bei der Aktivierung auf **PlatformPresented** festgelegt. Bei textbasierten Titeln werden dadurch automatisch die Textmarker des Titels angezeigt. Wenn die Umschaltfläche deaktiviert wird, wird der Darstellungsmodus auf **Disabled** festgelegt, sodass kein Text angezeigt wird und keine Marker-Ereignisse ausgelöst werden. Marker-Ereignisse werden weiter unten in diesem Artikel behandelt.

[!code-cs[ToggleChecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

Während der Verarbeitung der Metadatentitel können Sie auf den Satz von Markern im Titel zugreifen, indem Sie auf die [**Cues**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.cues) oder [**ActiveCues**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.activecues)-Eigenschaften zugreifen. Sie können dies tun, um die Benutzeroberfläche zu aktualisieren, damit die Markerspeicherorte für ein Medienelement angezeigt werden.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>Behandeln von nicht unterstützten Codecs und unbekannten Fehler beim Öffnen von Medienelementen
Ab Windows 10, Version 1607, können Sie überprüfen, ob der Codec zur Wiedergabe eines Medienelements auf dem Gerät unterstützt oder teilweise unterstützt wird, auf dem Ihre App ausgeführt wird. Überprüfen Sie zunächst im Ereignishandler für **MediaPlaybackItem**-Ereignisse mit geänderten Spuren, wie [**AudioTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged), ob sich bei der Änderung des Titels um das Einfügen eines neuen Titels handelt. Wenn dies der Fall ist, können Sie einen Verweis auf den Titel abrufen, der eingefügt wird, indem Sie den Index verwenden, der in den **IVectorChangedEventArgs.Index**-Parameter mit der entsprechenden Titelsammlung des **MediaPlaybackItem**-Parameter übergeben wurde, z. B. die [**AudioTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks)-Auflistung.

Wenn Sie einen Verweis auf den Titel eingefügt haben, überprüfen Sie den [**DecoderStatus**](https://docs.microsoft.com/uwp/api/windows.media.core.audiotracksupportinfo.decoderstatus) der [**SupportInfo**](https://docs.microsoft.com/uwp/api/windows.media.core.audiotrack.supportinfo)-Eigenschaft des Titels. Wenn der Wert [**FullySupported**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaDecoderStatus) ist, dann ist der für die Wiedergabe des Titels erforderliche Codec auf dem Gerät vorhanden. Wenn der Wert [**Degraded**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaDecoderStatus) ist, kann der Titel vom System wiedergegeben werden, die Wiedergabe wird jedoch zu einem gewissen Grad verschlechtert. Beispielsweise kann ein 5.1-Audiotitel auch als 2-Kanal-Stereo wiedergegeben werden. Wenn dies der Fall ist, sollten Sie die UI aktualisieren, um den Benutzer über die Beeinträchtigung zu informieren. Wenn der Wert [**UnsupportedSubtype**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaDecoderStatus) oder [**UnsupportedEncoderProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaDecoderStatus) ist, kann der Titel überhaupt nicht mit den aktuell auf dem Gerät vorhandenen Codecs wiedergegeben werden. Sie sollten den Benutzer warnen und die Wiedergabe des Elements überspringen oder UI-Elemente implementieren, um dem Benutzer den Download des richtigen Codec zu ermöglichen. Die [**GetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.core.audiotrack.getencodingproperties)-Methode des Titels kann verwendet werden, um den erforderlichen Codec für die Wiedergabe zu ermitteln.

Sie können schließlich eine Registrierung für das [**OpenFailed**](https://docs.microsoft.com/uwp/api/windows.media.core.audiotrack.openfailed)-Ereignis des Titels ausführen, das ausgelöst wird, wenn der Titel auf dem Gerät unterstützt wird, jedoch aufgrund eines unbekannten Fehlers in der Pipeline nicht geöffnet werden konnte.

[!code-cs[AudioTracksChanged_CodecCheck](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged_CodecCheck)]

Im [**OpenFailed**](https://docs.microsoft.com/uwp/api/windows.media.core.audiotrack.openfailed)-Ereignishandler können Sie überprüfen, ob der **MediaSource**-Status unbekannt ist. Wenn dies der Fall ist, können Sie programmgesteuert einen anderen Titel auswählen oder dem Benutzer die Auswahl eines anderen Titels bzw. die Aufgabe der Wiedergabe ermöglichen.

[!code-cs[OpenFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetOpenFailed)]

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>Festlegen der Anzeigeeigenschaften, die für den Medientransportsteuerelemente des Systems verwendet werden
Ab Windows 10, Version 1607, werden Medien, die in einem [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) wiedergegeben werden, standardmäßig automatisch in System Media Transport Controls (SMTC) integriert. Sie können die Metadaten angeben, die SMTC anzeigt, indem Sie die Anzeigeeigenschaften für ein **MediaPlaybackItem** aktualisieren. Rufen Sie ein Objekt ab, das die Anzeigeeigenschaften für ein Element darstellt, indem Sie [**GetDisplayProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties) aufrufen. Legen Sie fest, ob das Wiedergabeelement Musik oder Video ist, indem Sie die [**Type**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.type)-Eigenschaft festlegen. Legen Sie anschließend die Eigenschaften von [**VideoProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties) oder [**MusicProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) des Objekts fest. Rufen Sie [**ApplyDisplayProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties) auf, um die Eigenschaften des Elements auf die von Ihnen angegebenen Werte zu aktualisieren. In der Regel ruft eine App die Anzeigewerte dynamisch aus einem Webdienst ab. Das folgende Beispiel zeigt diesen Vorgang jedoch anhand hartcodierter Werte.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="add-external-timed-text-with-timedtextsource"></a>Hinzufügen von externem zeitgesteuerten Text mit „TimedTextSource“

Gelegentlich liegen unter Umständen externe Dateien mit zeitgesteuertem Text für ein Medienelement vor – beispielsweise Dateien mit Untertiteln für verschiedene Gebietsschemas. Mit der [**TimedTextSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.TimedTextSource)-Klasse können Sie externe zeitgesteuerte Textdateien aus einem Datenstrom oder URI laden.

In diesem Beispiel wird eine **Dictionary**-Sammlung zum Speichern einer Quellenliste mit zeitgesteuertem Text für das Medienelement verwendet. Dabei fungieren der Quell-URI und das **TimedTextSource**-Objekt als Schlüssel-Wert-Paar zum Identifizieren der aufgelösten Titel.

[!code-cs[TimedTextSourceMap](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

Rufen Sie [**CreateFromUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromuri) auf, um für jede externe Datei mit zeitgesteuertem Text ein neues **TimedTextSource**-Element zu erstellen. Fügen Sie dem **Dictionary**-Element einen Eintrag für die Quelle mit zeitgesteuertem Text hinzu. Fügen Sie einen Handler für das [**TimedTextSource.Resolved**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.resolved)-Ereignis hinzu, um einen möglichen Fehler beim Laden des Elements zu behandeln oder nach dem erfolgreichen Laden des Elements weitere Eigenschaften festzulegen.

Registrieren Sie alle Ihre **TimedTextSource**-Objekte beim **MediaSource**-Element, indem Sie sie der [**ExternalTimedTextSources**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.externaltimedtextsources)-Sammlung hinzufügen. Beachten Sie, dass externe Quellen mit zeitgesteuertem Text nicht dem auf der Grundlage der Quelle erstellten **MediaSource**-Element, sondern direkt dem **MediaPlaybackItem**-Element hinzugefügt werden. Registrieren und behandeln Sie das **TimedMetadataTracksChanged**-Ereignis wie zuvor in diesem Artikel beschrieben, um Ihre UI mit den externen Texttiteln zu aktualisieren.

[!code-cs[TimedTextSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

Ermitteln Sie im Handler für das [**TimedTextSource.Resolved**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.resolved)-Ereignis anhand der **Error**-Eigenschaft des an den Handler übergebenen [**TimedTextSourceResolveResultEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.TimedTextSourceResolveResultEventArgs)-Elements, ob beim Laden der zeitgesteuerten Textdaten ein Fehler aufgetreten ist. Wurde das Element erfolgreich aufgelöst, können Sie mit diesem Handler zusätzliche Eigenschaften des aufgelösten Titels aktualisieren. In diesem Beispiel wird auf der Grundlage des zuvor im **Dictionary**-Element gespeicherten URI für jede Spur eine Beschriftung hinzufügt.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## <a name="add-additional-metadata-tracks"></a>Hinzufügen zusätzlicher Metadatentitel

Sie können im Code dynamisch benutzerdefinierte Metadatentitel erstellen und sie einer Medienquelle zuordnen. Die erstellten Titel können Untertitel, Beschriftungstext oder proprietäre App-Daten enthalten.

Erstellen Sie ein neues [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.TimedMetadataTrack)-Element. Rufen Sie hierzu den Konstruktor auf, und geben Sie eine ID, die Sprachen-ID und einen Wert aus der [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.TimedMetadataKind)-Enumeration an. Registrieren Sie Handler für die Ereignisse [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.cueentered) und [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.cueexited). Diese Ereignisse werden ausgelöst, wenn die Startzeit für einen Marker erreicht wurde und wenn die Dauer für einen Marker abgelaufen ist.

Erstellen Sie ein neues, geeignetes Marker-Objekt für die Art des erstellten Metadatentitels, und legen Sie ID, Startzeit und Dauer für den Titel fest. Dieses Beispiel erstellt eine Datenspur. Es wird also eine Reihe von [**DataCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.DataCue)-Objekten erstellt, und für die einzelnen Marker wird jeweils ein Puffer mit App-spezifischen Daten bereitgestellt. Fügen Sie den neuen Titel der [**ExternalTimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.externaltimedmetadatatracks)-Sammlung des **MediaSource**-Objekts hinzu, um ihn zu registrieren.

Ab Windows 10 Version 1703 wird die **DataCue.Properties**-Eigenschaft als [**PropertySet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset) angezeigt, mit der Sie benutzerdefinierte Eigenschaften in Schlüssel-/Datenpaaren speichern können, die in **CueEntered**- und **CueExited**-Ereignissen abgerufen werden können.  

[!code-cs[AddDataTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

Das **CueEntered**-Ereignis wird ausgelöst, wenn die Startzeit eines Markers erreicht wurde und der zugehörige Titel den Präsentationsmodus **ApplicationPresented**, **Hidden** oder **PlatformPresented** hat. Marker-Ereignisse werden nicht für Metadatentitel ausgelöst, wenn der Präsentationsmodus für den Titel **Disabled** ist. Dieses Beispiel gibt einfach die benutzerdefinierten, dem Marker zugeordneten Daten im Debugfenster aus.

[!code-cs[DataCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

Dieses Beispiel fügt einen benutzerdefinierten Texttitel hinzu. Hierzu wird beim Erstellen des Titels **TimedMetadataKind.Caption** angegeben. Außerdem werden dem Titel mithilfe von [**TimedTextCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.TimedTextCue)-Objekten Marker hinzugefügt.

[!code-cs[AddTextTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>Wiedergeben einer Liste mit Medienelementen mit „MediaPlaybackList“

Mithilfe des [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList)-Elements können Sie eine Wiedergabeliste mit Medienelementen (dargestellt durch **MediaPlaybackItem**-Objekte) erstellen.

**Beachten Sie**  Elemente in einem [ **MediaPlaybackList** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList) werden mit lückenloses Wiedergabe gerendert. Das System verwendet die in MP3- oder AAC-codierten Dateien bereitgestellten Metadaten, um die für die lückenlose Wiedergabe erforderliche Verzögerungs- oder Auffüllkorrektur (Delay/Padding) zu ermitteln. Werden diese Metadaten von den MP3- oder AAC-codierten Dateien nicht bereitgestellt, ermittelt das System Verzögerungen und Auffüllungen heuristisch. Bei verlustfreien Formaten wie PCM, FLAC oder ALAC ergreift das System keine Maßnahme, da diese Encoder keine Verzögerungen oder Auffüllungen verursachen.

Deklarieren Sie zunächst eine Variable zum Speichern Ihres **MediaPlaybackList**-Elements.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

Erstellen Sie mithilfe des weiter oben beschriebenen Verfahrens für jedes Medienelement, das Sie Ihrer Liste hinzufügen möchten, ein **MediaPlaybackItem**-Element. Initialisieren Sie Ihr **MediaPlaybackList**-Objekt, und fügen Sie die Medienwiedergabeelemente hinzu. Registrieren Sie einen Handler für das [**CurrentItemChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.currentitemchanged)-Ereignis. Mit diesem Ereignis können Sie Ihre UI mit dem derzeit wiedergegebenen Medienelement aktualisieren. Sie können auch die Ereignisse [ItemOpened](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened) und [ItemFailed](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed) registrieren, die jeweils ausgelöst werden, wenn Elemente in der Liste erfolgreich geöffnet wurden bzw. nicht geöffnet werden können.

Ab Windows 10 Version 1703 können Sie angeben, welche maximale Anzahl von **MediaPlaybackItem**-Objekten in der **MediaPlaybackList** das System geöffnet lassen soll, nachdem sie durch Festlegen der [MaxPlayedItemsToKeepOpen](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen)-Eigenschaft wiedergegeben wurden. Wird ein **MediaPlaybackItem** geöffnet gelassen, kann die Wiedergabe sofort starten, wenn der Benutzer zu dem Element wechselt, da es nicht erneut geladen werden muss. Da sich der Speicherverbrauch Ihrer App erhöht, wenn Elemente geöffnet bleiben, sollten Sie beim Festlegen dieses Werts das Verhältnis zwischen Reaktionsfähigkeit und Speicherverbrauch berücksichtigen. 

Um die Wiedergabe Ihrer Liste zu aktivieren, legen Sie als Wiedergabequelle Ihres **MediaPlayer** Ihre **MediaPlaybackList** fest.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

Aktualisieren Sie im **CurrentItemChanged**-Ereignishandler Ihre UI mit dem derzeit wiedergegebenen Element. Dieses kann mithilfe der [**NewItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.newitem)-Eigenschaft des an das Ereignis übergebenen [**CurrentMediaPlaybackItemChangedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.CurrentMediaPlaybackItemChangedEventArgs)-Objekts abgerufen werden. Zur Erinnerung: Wenn Sie die UI auf der Grundlage dieses Ereignisses aktualisieren, müssen Sie dies innerhalb eines Aufrufs von [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows) tun, damit die Aktualisierungen im UI-Thread vorgenommen werden.

Ab Windows 10 Version 1703 können Sie durch den Wert der [CurrentMediaPlaybackItemChangedEventArgs.Reason](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason)-Eigenschaft den Grund für die Änderung des Elements feststellen, z. B. weil die App programmgesteuert gewechselt, das zuvor wiedergegebene Element beendet oder einen Fehler festgestellt hat.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]


Rufen Sie [**MovePrevious**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.moveprevious) oder [**MoveNext**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.movenext) auf, um bei der Media Player-Wiedergabe zum vorherigen oder nächsten Element Ihres **MediaPlaybackList**-Elements zu wechseln.

[!code-cs[PrevButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNextButton)]

Geben Sie durch Festlegen der [**ShuffleEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.shuffleenabled)-Eigenschaft an, ob der Media Player die Elemente in Ihrer Liste in zufälliger Reihenfolge wiedergeben soll.

[!code-cs[ShuffleButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetShuffleButton)]

Geben Sie durch Festlegen der [**AutoRepeatEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.autorepeatenabled)-Eigenschaft an, ob der Media Player die Wiedergabe Ihrer Liste automatisch wiederholen soll.

[!code-cs[RepeatButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRepeatButton)]


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>Behandeln des Ausfalls von Medienelementen in einer Wiedergabeliste
Das [**ItemFailed**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.itemfailed)-Ereignis wird ausgelöst, wenn ein Element in der Liste nicht geöffnet wird. Die [**ErrorCode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitemerror.errorcode)-Eigenschaft des [**MediaPlaybackItemError**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItemError)-Objekts, das an den Ereignishandler übergeben wird, listet die genaue Ursache des Fehlers auf, wenn möglich, einschließlich Netzwerkfehlern, Decodierungsfehlern oder Verschlüsselungsfehlern.

[!code-cs[ItemFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetItemFailed)]

### <a name="disable-playback-of-items-in-a-playback-list"></a>Deaktivieren der Wiedergabe von Elementen in Wiedergabelisten
Ab Windows 10 Version 1703 können Sie die Wiedergabe eines oder mehrerer Elemente in einer **MediaPlaybackItemList** deaktivieren, indem Sie die [IsDisabledInPlaybackList](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList)-Eigenschaft eines [MediaPlaybackItem](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) auf „false“ festlegen. 

Eine typische Anwendung dieses Features ist in Apps, die Internet-Musikstreams wiedergeben. Die App kann Änderungen im Netzwerkverbindungsstatus des Geräts überwachen, um die Wiedergabe unvollständig heruntergeladener Elemente zu deaktivieren. Im folgenden Beispiel wird ein Handler für das [NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged)-Ereignis registriert.

[!code-cs[RegisterNetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRegisterNetworkStatusChanged)]

Überprüfen Sie im **NetworkStatusChanged**-Handler, ob [GetInternetConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) den Wert Null zurückgibt, wenn das Netzwerk nicht verbunden ist. Falls ja, wiederholen Sie die Wiedergabe aller Elemente in der Liste; wenn der Wert [TotalDownloadProgress](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) eines Elements kleiner als 1 ist, das Element also nicht vollständig heruntergeladen wurde, deaktivieren Sie es. Wenn die Netzwerkverbindung aktiviert ist, wiederholen Sie die Wiedergabe der Liste und aktivieren Sie jedes Element.

[!code-cs[NetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNetworkStatusChanged)]

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Verzögern der Bindung von Medieninhalten in einer Wiedergabeliste mit MediaBinder
In den vorherigen Beispielen wurde aus einer Datei, URL oder Datenstrom zuerst eine **MediaSource** erstellt, dann ein **MediaPlaybackItem** erstellt und einer **MediaPlaybackList** hinzugefügt. In manchen Fällen, z. B. wenn Benutzer für angezeigte Inhalte bezahlen müssen, können Sie ggf. das Abrufen des Inhalts einer **MediaSource** verzögern, bis das Element in der Liste für die Wiedergabe bereit ist. Um dieses Szenario zu implementieren, erstellen Sie eine Instanz der [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder)-Klasse. Legen Sie die [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token)-Eigenschaft auf eine von der App definierte Zeichenfolge fest, die den Inhalt identifiziert, dessen Abruf Sie verzögern möchten, und registrieren Sie einen Handler für das [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding)-Ereignis. Anschließend erstellen Sie eine **MediaSource** aus dem **Binder** durch den Aufruf [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder). Nun erstellen Sie ein **MediaPlaybackItem** aus der **MediaSource**, das Sie der Wiedergabeliste hinzufügen.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

Wenn das System bestimmt, dass der von **MediaBinder** zugeordnete Inhalt abgerufen werden soll, wird das **Binding**-Ereignis ausgelöst. Im Handler für dieses Ereignis können Sie die **MediaBinder**-Instanz von [**MediaBindingEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs) abrufen, die dem Ereignis übergeben wurde. Rufen Sie die für die **Token**-Eigenschaft angegebene Zeichenfolge ab, um festzustellen, welche Inhalte abgerufen werden sollen. Mit **MediaBindingEventArgs** können verschiedene Darstellungen für gebundene Inhalte festgelegt werden, z. B. [**SetStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile), [**SetStream**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstream), [**SetStreamReference**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference) und [**SetUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.seturi). 

[!code-cs[BinderBinding](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBinding)]

Denken Sie daran, für asynchrone Vorgänge im **Binding**-Ereignishandler die Methode [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) aufzurufen, z. B. für Webanforderungen, damit das System vor der Fortsetzung den Abschluss des Vorgangs abwartet. Wenn der Vorgang abgeschlossen ist, rufen Sie [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete), um das System fortzusetzen.

Ab Windows 10 Version 1703 können Sie eine [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) als gebundenen Inhalt bereitstellen, indem Sie [**SetAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource) aufrufen. Weitere Informationen über adaptives Streaming für Ihre App finden Sie unter [Adaptives Streaming](adaptive-streaming.md).



## <a name="related-topics"></a>Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Abspielen von Audio- und Videodateien mit MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrieren Sie in die System-Media-Transport-Steuerelemente](integrate-with-systemmediatransportcontrols.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md)

