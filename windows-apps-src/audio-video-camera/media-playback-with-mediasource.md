---
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: In diesem Artikel wird erläutert, wie Sie die MediaSource-Klasse verwenden, die allgemein zum Verweisen auf Medien aus verschiedenen Quellen (etwa lokale Dateien oder Remotedateien) sowie zum Wiedergeben dieser Medien verwendet wird und ein gemeinsames Modell für den Mediendatenzugriff verfügbar macht – unabhängig vom zugrunde liegenden Medienformat.
title: Medienelemente, Wiedergabelisten und Titel
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f1f428bb8beb4bb933387a77e5a74819016a4c64
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363883"
---
# <a name="media-items-playlists-and-tracks"></a>Medienelemente, Wiedergabelisten und Titel


 In diesem Artikel wird erläutert, wie Sie die [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource)-Klasse verwenden, die allgemein zum Verweisen auf Medien aus verschiedenen Quellen (etwa lokale Dateien oder Remotedateien) sowie zum Wiedergeben dieser Medien verwendet wird und ein gemeinsames Modell für den Mediendatenzugriff verfügbar macht – unabhängig vom zugrunde liegenden Medienformat. Die [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem)-Klasse erweitert den Funktionsumfang von **MediaSource** und ermöglicht die Verwaltung und Auswahl mehrerer Audio-, Video- und Metadatentitel in einem Medienelement. [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) ermöglicht die Erstellung von Wiedergabelisten auf der Grundlage von Medienelementen.


## <a name="create-and-play-a-mediasource"></a>Erstellen und Wiedergeben eines MediaSource-Elements

Erstellen Sie eine neue Instanz von **MediaSource**, indem Sie eine der von der Klasse verfügbar gemachten Factory-Methoden aufrufen:

-   [**CreateFromAdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.createfromadaptivemediasource)
-   [**CreateFromIMediaSource**](/uwp/api/windows.media.core.mediasource.createfromimediasource)
-   [**CreateFromMediaStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource)
-   [**CreateFromMseStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommsestreamsource)
-   [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile)
-   [**CreateFromStream**](/uwp/api/windows.media.core.mediasource.createfromstream)
-   [**CreateFromStreamReference**](/uwp/api/windows.media.core.mediasource.createfromstreamreference)
-   [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri)
-   [**"Kreatefromdownloadoperation"**](/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

Nach dem Erstellen einer **MediaSource** können Sie diese mit einer [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) durch Festlegen der [**Source**](/uwp/api/windows.media.playback.mediaplayer.source)-Eigenschaft abspielen. Ab Windows 10, Version 1607, können Sie einen **MediaPlayer** einem [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) durch Aufrufen von [**SetMediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) zuweisen, um die Media Player-Inhalte in einer XAML-Seite zu rendern. Dies ist die bevorzugte Methode gegenüber der Verwendung von **MediaElement**. Weitere Informationen zur Verwendung des **MediaPlayer** finden Sie unter [**Wiedergeben von Audio- und Videoinhalten mit MediaPlayer**](play-audio-and-video-with-mediaplayer.md).

Das folgende Beispiel zeigt die Wiedergabe einer vom Benutzer ausgewählten Mediendatei in einem **MediaPlayer** mit **MediaSource**.

Für dieses Szenario müssen die Namespaces [**Windows.Media.Core**](/uwp/api/Windows.Media.Core) und [**Windows.Media.Playback**](/uwp/api/Windows.Media.Playback) eingeschlossen werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetUsing":::

Deklarieren Sie eine Membervariable vom Typ **MediaSource**. Für die Beispiele in diesem Artikel wird die Medienquelle als Klassenmember deklariert, um den Zugriff über mehrere Orte zu ermöglichen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaSource":::

Deklarieren Sie eine Variable zum Speichern des **MediaPlayer** Objekts. Wenn die Medieninhalte in XAML gerendert werden sollen, fügen Sie der Seite ein **MediaPlayerElement**-Steuerelements hinzu.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlayer":::

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElement":::

Verwenden Sie ein [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)-Element, um dem Benutzer das Auswählen einer wiederzugebenden Mediendatei zu ermöglichen. Initialisieren Sie mit dem von der [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync)-Auswahlmethode zurückgegebenen [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Objekt durch Aufrufen von [**MediaSource.CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile) ein neues Medienobjekt. Legen Sie abschließend durch Aufrufen der [**SetPlaybackSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.setplaybacksource)-Methode die Medienquelle als Wiedergabequelle für das **MediaElement**-Element fest.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaSource":::

Standardmäßig beginnt **MediaPlayer** nicht automatisch mit der Wiedergabe, wenn die Medienquelle festgelegt ist. Sie können die Wiedergabe manuell durch Aufrufen von [**Play**](/uwp/api/windows.media.playback.mediaplayer.play) wiedergeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlay":::

Sie können auch die [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay)-Eigenschaft des **MediaPlayer** auf „ true“ festlegen, um dem Player mitzuteilen, dass er mit der Wiedergabe beginnen soll, sobald die Medienquelle festgelegt ist.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAutoPlay":::

### <a name="create-a-mediasource-from-a-downloadoperation"></a>Erstellen einer MediaSource aus einem Downloadvorgang
Ab Windows, Version 1803, können Sie ein **MediaSource** -Objekt aus einem **Downloadvorgang**erstellen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetCreateMediaSourceFromDownload":::

Obwohl Sie eine **MediaSource** aus einem Download erstellen können, ohne Sie zu starten oder die **israndomaccessrequired** -Eigenschaft auf "true" festzulegen, müssen Sie beide Aktionen ausführen, bevor Sie versuchen, die **MediaSource** an **Media Player** oder **mediaplayerelement** für die Wiedergabe anzufügen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetStartDownload":::


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>Behandeln mehrerer Audio-, Video- und Metadatentitel mit „MediaPlaybackItem“

Die Wiedergabe mithilfe eines [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource)-Elements ist praktisch, da es allgemein die Wiedergabe von Medien aus unterschiedlichen Quellen ermöglicht. Bei Erstellung eines [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) aus der **MediaSource** stehen dagegen stärker erweiterte Verhaltensweisen zur Verfügung. Unter anderem können Sie damit auf mehrere Audio-, Video- und Datentitel eines Medienelements zugreifen und die Titel verwalten.

Deklarieren Sie eine Variable zum Speichern Ihres **MediaPlaybackItem**-Elements.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackItem":::

Erstellen Sie ein **MediaPlaybackItem**-Element, indem Sie den Konstruktor aufrufen und ein initialisiertes **MediaSource**-Objekt übergeben.

Wenn Ihre App mehrere Audio-, Video- oder Datentitel in einem Medienwiedergabeelement unterstützt, registrieren Sie Ereignishandler für das Ereignis [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged), [**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged) oder [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged).

Legen Sie abschließend die Wiedergabequelle des **MediaElement**- oder **MediaPlayer**-Elements auf Ihr **MediaPlaybackItem**-Element fest.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackItem":::

> [!NOTE] 
> Ein **MediaSource**-Element kann immer nur einem einzelnen **MediaPlaybackItem**-Element zugeordnet werden. Wenn Sie nach der Erstellung eines quellenbasierten **MediaPlaybackItem** -Elements versuchen, auf der Grundlage der gleichen Quelle ein weiteres Wiedergabeelement zu erstellen, tritt ein Fehler auf. Außerdem gilt: Nach der Erstellung eines auf einer Medienquelle basierenden **MediaPlaybackItem**-Elements können Sie das **MediaSource**-Objekt nicht direkt als Quelle für ein **MediaPlayer**-Element festlegen, sondern müssen stattdessen das **MediaPlaybackItem**-Element verwenden.

Das [**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged)-Ereignis wird ausgelöst, wenn ein **MediaPlaybackItem**-Element mit mehreren Videotiteln als Wiedergabequelle zugewiesen wurde, und kann erneut ausgelöst werden, wenn sich die Liste mit den Videotiteln für das Element ändert. Der Handler für dieses Ereignis ermöglicht die Aktualisierung der Benutzeroberfläche, damit der Benutzer zwischen den verfügbaren Titeln wechseln kann. In diesem Beispiel wird zum Anzeigen der verfügbaren Videotitel ein [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)-Element verwendet.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetVideoComboBox":::

Durchlaufen Sie im **VideoTracksChanged**-Handler alle Titel, die in der [**VideoTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks)-Liste des Wiedergabeelements enthalten sind. Für die einzelnen Titel wird jeweils ein neues [**ComboBoxItem**](/uwp/api/Windows.UI.Xaml.Controls.ComboBoxItem)-Element erstellt. Falls der Titel noch nicht beschriftet ist, wird auf der Grundlage des Titelindex eine Beschriftung generiert. Die [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag)-Eigenschaft des Kombinationsfeldelements wird zur späteren Identifizierung auf den Titelindex festgelegt. Danach wird das Element dem Kombinationsfeld hinzugefügt. Beachten Sie, dass diese Vorgänge innerhalb eines [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)-Aufrufs ausgeführt werden, da alle UI-Änderungen im UI-Thread erfolgen müssen und dieses Ereignis in einem anderen Thread ausgelöst wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksChanged":::

Im [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Handler für das Kombinationsfeld wird der Titelindex aus der **Tag**-Eigenschaft des ausgewählten Elements abgerufen. Das Festlegen der [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackvideotracklist.selectedindex)-Eigenschaft der [**VideoTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks)-Liste des Medienwiedergabeelements bewirkt, dass das **MediaElement**- oder **MediaPlayer**-Element beim aktiven Videotitel zum angegebenen Index wechselt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksSelectionChanged":::

Die Verwaltung von Medienelementen mit mehreren Audiotiteln funktioniert genau wie bei den Videotiteln. Behandeln Sie das [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged)-Ereignis, um Ihre Benutzeroberfläche mit den Audiotiteln aus der [**AudioTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks)-Liste des Wiedergabeelements zu aktualisieren. Wenn der Benutzer einen Audiotitel auswählt, legen Sie die [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackaudiotracklist.selectedindex)-Eigenschaft der **AudioTracks**-Liste fest, damit das **MediaElement**- oder **MediaPlayer**-Element beim aktiven Audiotitel zum angegebenen Index wechselt.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetAudioComboBox":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksSelectionChanged":::

Neben Audio und Video kann ein **MediaPlaybackItem**-Objekt auch [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack)-Objekte enthalten. Ein zeitgesteuerter Metadatentitel kann Untertitel, Beschriftungstext oder benutzerdefinierte proprietäre Daten Ihrer App enthalten. Ein zeitgesteuerter Metadatentitel enthält eine Liste mit Markern. Diese werden durch Objekte dargestellt, die von [**IMediaCue**](/uwp/api/Windows.Media.Core.IMediaCue) erben (etwa [**DataCue**](/uwp/api/Windows.Media.Core.DataCue) oder [**TimedTextCue**](/uwp/api/Windows.Media.Core.TimedTextCue)). Jeder Marker hat eine Anfangszeit und eine Dauer. Diese bestimmen den Aktivierungszeitpunkt und die Aktivierungsdauer des jeweiligen Markers.

Die zeitgesteuerten Metadatentitel für ein Medienelement können ähnlich wie Audio- und Videotitel durch Behandeln des [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged)-Ereignisses eines **MediaPlaybackItem**-Elements ermittelt werden. Bei zeitgesteuerten Metadatentiteln möchte der Benutzer aber unter Umständen mehrere Metadatentitel gleichzeitig aktivieren. Außerdem empfiehlt sich je nach App-Szenario unter Umständen eine automatische Aktivierung oder Deaktivierung von Metadatentiteln. Zu Veranschaulichung wird in diesem Beispiel ein " [**degglebutton**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton) " für jeden metadatentitel in einem Medien Element hinzugefügt, um dem Benutzer zu ermöglichen, den Track zu aktivieren und zu deaktivieren. Die **Tag** -Eigenschaft jeder Schaltfläche wird auf den Index des zugeordneten metadatentitels festgelegt, sodass Sie identifiziert werden kann, wenn die Schaltfläche ein-und ausgeschaltet wird.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMetaStackPanel":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedMetadataTrackschanged":::

Da gleichzeitig mehrere Metadatentitel aktiv sein können, legen Sie nicht einfach den aktiven Index für die Metadatentitelliste fest. Rufen Sie stattdessen die [**SetPresentationMode**](/previous-versions/windows/dn986977(v=win.10))-Methode des **MediaPlaybackItem**-Objekts auf. Übergeben Sie dabei den Index des umzuschaltenden Titels, und geben Sie dann einen Wert aus der [**TimedMetadataTrackPresentationMode**](/uwp/api/Windows.Media.Playback.TimedMetadataTrackPresentationMode)-Enumeration an. Der gewählte Darstellungsmodus hängt von der Implementierung Ihrer App ab. In diesem Beispiel wird der Metadatentitel bei der Aktivierung auf **PlatformPresented** festgelegt. Bei textbasierten Spuren bedeutet dies, dass das System automatisch die Text Hinweise in der Spur anzeigt. Wenn die UMSCHALT Fläche ausgeschaltet wird, wird der Präsentationsmodus auf **deaktiviert**festgelegt. Dies bedeutet, dass kein Text angezeigt wird und keine Hinweis Ereignisse ausgelöst werden. Marker-Ereignisse werden weiter unten in diesem Artikel behandelt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleChecked":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleUnchecked":::

Während der Verarbeitung der Metadatentitel können Sie auf den Satz von Markern im Titel zugreifen, indem Sie auf die [**Cues**](/uwp/api/windows.media.core.timedmetadatatrack.cues) oder [**ActiveCues**](/uwp/api/windows.media.core.timedmetadatatrack.activecues)-Eigenschaften zugreifen. Sie können dies tun, um die Benutzeroberfläche zu aktualisieren, damit die Markerspeicherorte für ein Medienelement angezeigt werden.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>Behandeln von nicht unterstützten Codecs und unbekannten Fehler beim Öffnen von Medienelementen
Ab Windows 10, Version 1607, können Sie überprüfen, ob der Codec zur Wiedergabe eines Medienelements auf dem Gerät unterstützt oder teilweise unterstützt wird, auf dem Ihre App ausgeführt wird. Überprüfen Sie im Ereignishandler für **mediaplaybackitem** -geänderte Ereignisse wie [**audiotrackschge**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged)zunächst, ob die nach Verfolgungs Änderung eine Einfügung eines neuen Titels ist. Wenn dies der Fall ist, können Sie mithilfe des Indexes, der im **ivectorchangedebug. Index** -Parameter übergeben wird, einen Verweis auf den Track-Befehl erhalten, der mit der entsprechenden Track-Auflistung des **mediaplaybackitem** -Parameters übergeben wird, z. b. die [**Audiotracks**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) -Auflistung.

Wenn Sie einen Verweis auf den Titel eingefügt haben, überprüfen Sie den [**DecoderStatus**](/uwp/api/windows.media.core.audiotracksupportinfo.decoderstatus) der [**SupportInfo**](/uwp/api/windows.media.core.audiotrack.supportinfo)-Eigenschaft des Titels. Wenn der Wert [**FullySupported**](/uwp/api/Windows.Media.Core.MediaDecoderStatus) ist, dann ist der für die Wiedergabe des Titels erforderliche Codec auf dem Gerät vorhanden. Wenn der Wert [**Degraded**](/uwp/api/Windows.Media.Core.MediaDecoderStatus) ist, kann der Titel vom System wiedergegeben werden, die Wiedergabe wird jedoch zu einem gewissen Grad verschlechtert. Beispielsweise kann ein 5.1-Audiotitel auch als 2-Kanal-Stereo wiedergegeben werden. Wenn dies der Fall ist, sollten Sie die UI aktualisieren, um den Benutzer über die Beeinträchtigung zu informieren. Wenn der Wert [**UnsupportedSubtype**](/uwp/api/Windows.Media.Core.MediaDecoderStatus) oder [**UnsupportedEncoderProperties**](/uwp/api/Windows.Media.Core.MediaDecoderStatus) ist, kann der Titel überhaupt nicht mit den aktuell auf dem Gerät vorhandenen Codecs wiedergegeben werden. Sie sollten den Benutzer warnen und die Wiedergabe des Elements überspringen oder UI-Elemente implementieren, um dem Benutzer den Download des richtigen Codec zu ermöglichen. Die [**GetEncodingProperties**](/uwp/api/windows.media.core.audiotrack.getencodingproperties)-Methode des Titels kann verwendet werden, um den erforderlichen Codec für die Wiedergabe zu ermitteln.

Sie können schließlich eine Registrierung für das [**OpenFailed**](/uwp/api/windows.media.core.audiotrack.openfailed)-Ereignis des Titels ausführen, das ausgelöst wird, wenn der Titel auf dem Gerät unterstützt wird, jedoch aufgrund eines unbekannten Fehlers in der Pipeline nicht geöffnet werden konnte.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged_CodecCheck":::

Im [**OpenFailed**](/uwp/api/windows.media.core.audiotrack.openfailed)-Ereignishandler können Sie überprüfen, ob der **MediaSource**-Status unbekannt ist. Wenn dies der Fall ist, können Sie programmgesteuert einen anderen Titel auswählen oder dem Benutzer die Auswahl eines anderen Titels bzw. die Aufgabe der Wiedergabe ermöglichen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetOpenFailed":::

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>Festlegen der Anzeigeeigenschaften, die für den Medientransportsteuerelemente des Systems verwendet werden
Ab Windows 10, Version 1607, werden Medien, die in einem [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) wiedergegeben werden, standardmäßig automatisch in System Media Transport Controls (SMTC) integriert. Sie können die Metadaten angeben, die SMTC anzeigt, indem Sie die Anzeigeeigenschaften für ein **MediaPlaybackItem** aktualisieren. Rufen Sie ein Objekt ab, das die Anzeigeeigenschaften für ein Element darstellt, indem Sie [**GetDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties) aufrufen. Legen Sie fest, ob das Wiedergabeelement Musik oder Video ist, indem Sie die [**Type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type)-Eigenschaft festlegen. Legen Sie anschließend die Eigenschaften von [**VideoProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties) oder [**MusicProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) des Objekts fest. Rufen Sie [**ApplyDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties) auf, um die Eigenschaften des Elements auf die von Ihnen angegebenen Werte zu aktualisieren. In der Regel ruft eine App die Anzeigewerte dynamisch aus einem Webdienst ab. Das folgende Beispiel zeigt diesen Vorgang jedoch anhand hartcodierter Werte.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetVideoProperties":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetMusicProperties":::

## <a name="add-external-timed-text-with-timedtextsource"></a>Hinzufügen von externem zeitgesteuerten Text mit „TimedTextSource“

Gelegentlich liegen unter Umständen externe Dateien mit zeitgesteuertem Text für ein Medienelement vor – beispielsweise Dateien mit Untertiteln für verschiedene Gebietsschemas. Mit der [**TimedTextSource**](/uwp/api/Windows.Media.Core.TimedTextSource)-Klasse können Sie externe zeitgesteuerte Textdateien aus einem Datenstrom oder URI laden.

In diesem Beispiel wird eine **Dictionary**-Sammlung zum Speichern einer Quellenliste mit zeitgesteuertem Text für das Medienelement verwendet. Dabei fungieren der Quell-URI und das **TimedTextSource**-Objekt als Schlüssel-Wert-Paar zum Identifizieren der aufgelösten Titel.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceMap":::

Rufen Sie [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) auf, um für jede externe Datei mit zeitgesteuertem Text ein neues **TimedTextSource**-Element zu erstellen. Fügen Sie dem **Dictionary**-Element einen Eintrag für die Quelle mit zeitgesteuertem Text hinzu. Fügen Sie einen Handler für das [**TimedTextSource.Resolved**](/uwp/api/windows.media.core.timedtextsource.resolved)-Ereignis hinzu, um einen möglichen Fehler beim Laden des Elements zu behandeln oder nach dem erfolgreichen Laden des Elements weitere Eigenschaften festzulegen.

Registrieren Sie alle Ihre **TimedTextSource**-Objekte beim **MediaSource**-Element, indem Sie sie der [**ExternalTimedTextSources**](/uwp/api/windows.media.core.mediasource.externaltimedtextsources)-Sammlung hinzufügen. Beachten Sie, dass externe Quellen mit zeitgesteuertem Text nicht dem auf der Grundlage der Quelle erstellten **MediaSource**-Element, sondern direkt dem **MediaPlaybackItem**-Element hinzugefügt werden. Registrieren und behandeln Sie das **TimedMetadataTracksChanged**-Ereignis wie zuvor in diesem Artikel beschrieben, um Ihre UI mit den externen Texttiteln zu aktualisieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSource":::

Ermitteln Sie im Handler für das [**TimedTextSource.Resolved**](/uwp/api/windows.media.core.timedtextsource.resolved)-Ereignis anhand der **Error**-Eigenschaft des an den Handler übergebenen [**TimedTextSourceResolveResultEventArgs**](/uwp/api/Windows.Media.Core.TimedTextSourceResolveResultEventArgs)-Elements, ob beim Laden der zeitgesteuerten Textdaten ein Fehler aufgetreten ist. Wenn das Element erfolgreich aufgelöst wurde, können Sie mit diesem Handler zusätzliche Eigenschaften des aufgelösten Titels aktualisieren. In diesem Beispiel wird eine Bezeichnung für jede Spur basierend auf dem zuvor im **Wörterbuch**gespeicherten URI hinzugefügt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceResolved":::

## <a name="add-additional-metadata-tracks"></a>Hinzufügen zusätzlicher Metadatentitel

Sie können im Code dynamisch benutzerdefinierte Metadatentitel erstellen und sie einer Medienquelle zuordnen. Die erstellten Titel können Untertitel, Beschriftungstext oder proprietäre App-Daten enthalten.

Erstellen Sie ein neues [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack)-Element. Rufen Sie hierzu den Konstruktor auf, und geben Sie eine ID, die Sprachen-ID und einen Wert aus der [**TimedMetadataKind**](/uwp/api/Windows.Media.Core.TimedMetadataKind)-Enumeration an. Registrieren Sie Handler für die Ereignisse [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.cueentered) und [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.cueexited). Diese Ereignisse werden ausgelöst, wenn die Startzeit für einen Marker erreicht wurde und wenn die Dauer für einen Marker abgelaufen ist.

Erstellen Sie ein neues Hinweis Objekt, das für den Typ der von Ihnen erstellten metadatenverfolgung geeignet ist, und legen Sie die ID, die Startzeit und die Dauer für die Nachverfolgung fest. In diesem Beispiel wird ein Daten Track erstellt, sodass ein Satz von [**datacue**](/uwp/api/Windows.Media.Core.DataCue) -Objekten generiert wird und ein Puffer, der APP-spezifische Daten enthält, für jeden Hinweis bereitgestellt wird. Fügen Sie den neuen Titel der [**ExternalTimedMetadataTracks**](/uwp/api/windows.media.core.mediasource.externaltimedmetadatatracks)-Sammlung des **MediaSource**-Objekts hinzu, um ihn zu registrieren.

Ab Windows 10, Version 1703, macht die **datacue. Properties** -Eigenschaft ein [**PropertySet**](/uwp/api/windows.foundation.collections.propertyset) verfügbar, mit dem Sie benutzerdefinierte Eigenschaften in Schlüssel/Daten-Paaren speichern können, die in den Ereignissen " **cueeingetragen** " und " **cueexited** " abgerufen werden können.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddDataTrack":::

Das **CueEntered**-Ereignis wird ausgelöst, wenn die Startzeit eines Markers erreicht wurde und der zugehörige Titel den Präsentationsmodus **ApplicationPresented**, **Hidden** oder **PlatformPresented** hat. Marker-Ereignisse werden nicht für Metadatentitel ausgelöst, wenn der Präsentationsmodus für den Titel **Disabled** ist. Dieses Beispiel gibt einfach die benutzerdefinierten, dem Marker zugeordneten Daten im Debugfenster aus.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDataCueEntered":::

Dieses Beispiel fügt einen benutzerdefinierten Texttitel hinzu. Hierzu wird beim Erstellen des Titels **TimedMetadataKind.Caption** angegeben. Außerdem werden dem Titel mithilfe von [**TimedTextCue**](/uwp/api/Windows.Media.Core.TimedTextCue)-Objekten Marker hinzugefügt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddTextTrack":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTextCueEntered":::

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>Wiedergeben einer Liste mit Medienelementen mit „MediaPlaybackList“

Mithilfe des [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList)-Elements können Sie eine Wiedergabeliste mit Medienelementen (dargestellt durch **MediaPlaybackItem**-Objekte) erstellen.

**Hinweis**    Elemente in einer [**mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) werden mithilfe der Wiedergabe losen Wiedergabe gerendert. Das System verwendet die in MP3- oder AAC-codierten Dateien bereitgestellten Metadaten, um die für die lückenlose Wiedergabe erforderliche Verzögerungs- oder Auffüllkorrektur (Delay/Padding) zu ermitteln. Werden diese Metadaten von den MP3- oder AAC-codierten Dateien nicht bereitgestellt, ermittelt das System Verzögerungen und Auffüllungen heuristisch. Bei verlustfreien Formaten wie PCM, FLAC oder ALAC ergreift das System keine Maßnahme, da diese Encoder keine Verzögerungen oder Auffüllungen verursachen.

Deklarieren Sie zunächst eine Variable zum Speichern Ihres **MediaPlaybackList**-Elements.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackList":::

Erstellen Sie mithilfe des weiter oben beschriebenen Verfahrens für jedes Medienelement, das Sie Ihrer Liste hinzufügen möchten, ein **MediaPlaybackItem**-Element. Initialisieren Sie Ihr **MediaPlaybackList**-Objekt, und fügen Sie die Medienwiedergabeelemente hinzu. Registrieren Sie einen Handler für das [**CurrentItemChanged**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemchanged)-Ereignis. Mit diesem Ereignis können Sie Ihre UI mit dem derzeit wiedergegebenen Medienelement aktualisieren. Sie können sich auch für das [itemgeöffnete](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened) -Ereignis registrieren, das ausgelöst wird, wenn ein Element in der Liste erfolgreich geöffnet wird, und das [itemfailed](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed) -Ereignis, das ausgelöst wird, wenn ein Element in der Liste nicht geöffnet werden kann.

Ab Windows 10, Version 1703, können Sie die maximale Anzahl von **mediaplaybackitem** -Objekten in der **mediaplaybacklist** angeben, die das System nach der Wiedergabe geöffnet bleibt, indem Sie die [maxplayeditemstokeepopen](/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen) -Eigenschaft festlegen. Wenn ein **mediaplaybackitem** geöffnet bleibt, kann die Wiedergabe des Elements sofort gestartet werden, wenn der Benutzer zu diesem Element wechselt, da das Element nicht erneut geladen werden muss. Die Offenlegung von Elementen erhöht aber auch die Arbeitsspeicher Nutzung Ihrer APP. Daher sollten Sie beim Festlegen dieses Werts das Gleichgewicht zwischen Reaktionsfähigkeit und Speicherauslastung berücksichtigen. 

Um die Wiedergabe Ihrer Liste zu aktivieren, legen Sie die Wiedergabe Quelle von **Media Player** auf Ihre **mediaplaybacklist**fest.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackList":::

Aktualisieren Sie im **CurrentItemChanged**-Ereignishandler Ihre UI mit dem derzeit wiedergegebenen Element. Dieses kann mithilfe der [**NewItem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.newitem)-Eigenschaft des an das Ereignis übergebenen [**CurrentMediaPlaybackItemChangedEventArgs**](/uwp/api/Windows.Media.Playback.CurrentMediaPlaybackItemChangedEventArgs)-Objekts abgerufen werden. Zur Erinnerung: Wenn Sie die UI auf der Grundlage dieses Ereignisses aktualisieren, müssen Sie dies innerhalb eines Aufrufs von [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) tun, damit die Aktualisierungen im UI-Thread vorgenommen werden.

Ab Windows 10, Version 1703, können Sie die [currentmediaplaybackitemchangedeventargs. Reason](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) -Eigenschaft überprüfen, um einen Wert abzurufen, der den Grund für das Ändern des Elements angibt, z. b. das programmgesteuerte wechseln von Elementen, das bereits spielende Element, das das Ende erreicht hat, oder einen Fehler, der auftritt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetMediaPlaybackListItemChanged":::


Rufen Sie [**MovePrevious**](/uwp/api/windows.media.playback.mediaplaybacklist.moveprevious) oder [**MoveNext**](/uwp/api/windows.media.playback.mediaplaybacklist.movenext) auf, um bei der Media Player-Wiedergabe zum vorherigen oder nächsten Element Ihres **MediaPlaybackList**-Elements zu wechseln.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPrevButton":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNextButton":::

Geben Sie durch Festlegen der [**ShuffleEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.shuffleenabled)-Eigenschaft an, ob der Media Player die Elemente in Ihrer Liste in zufälliger Reihenfolge wiedergeben soll.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetShuffleButton":::

Geben Sie durch Festlegen der [**AutoRepeatEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.autorepeatenabled)-Eigenschaft an, ob der Media Player die Wiedergabe Ihrer Liste automatisch wiederholen soll.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRepeatButton":::


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>Behandeln des Ausfalls von Medienelementen in einer Wiedergabeliste
Das [**ItemFailed**](/uwp/api/windows.media.playback.mediaplaybacklist.itemfailed)-Ereignis wird ausgelöst, wenn ein Element in der Liste nicht geöffnet wird. Die [**ErrorCode**](/uwp/api/windows.media.playback.mediaplaybackitemerror.errorcode)-Eigenschaft des [**MediaPlaybackItemError**](/uwp/api/Windows.Media.Playback.MediaPlaybackItemError)-Objekts, das an den Ereignishandler übergeben wird, listet die genaue Ursache des Fehlers auf, wenn möglich, einschließlich Netzwerkfehlern, Decodierungsfehlern oder Verschlüsselungsfehlern.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetItemFailed":::

### <a name="disable-playback-of-items-in-a-playback-list"></a>Wiedergabe von Elementen in einer Wiedergabeliste deaktivieren
Ab Windows 10, Version 1703, können Sie die Wiedergabe von einem oder mehreren Elementen in einer **mediaplaybackitemlist** deaktivieren, indem Sie die [isdisabledinplaybacklist](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) -Eigenschaft von [mediaplaybackitem](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) auf false festlegen. 

Ein typisches Szenario für dieses Feature ist für apps, die Musik abspielen, die aus dem Internet gestreamt werden. Die APP kann auf Änderungen im Netzwerk Verbindungsstatus des Geräts lauschen und die Wiedergabe von Elementen deaktivieren, die nicht vollständig heruntergeladen wurden. Im folgenden Beispiel wird ein Handler für das [NetworkInformation. networkstatuschge](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged) -Ereignis registriert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterNetworkStatusChanged":::

Überprüfen Sie im Handler für **networkstatuschge**, ob [getinternetconnectionprofile](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) NULL zurückgibt, was darauf hinweist, dass das Netzwerk nicht verbunden ist. Wenn dies der Fall ist, durchlaufen Sie alle Elemente in der Wiedergabeliste, und wenn [totaldownloadprogress](/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) für das Element kleiner als 1 ist, was bedeutet, dass das Element nicht vollständig heruntergeladen wurde, deaktivieren Sie das Element. Wenn die Netzwerkverbindung aktiviert ist, durchlaufen Sie alle Elemente in der Wiedergabeliste, und aktivieren Sie jedes Element.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNetworkStatusChanged":::

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Zurückstellen der Bindung von Medieninhalten für Elemente in einer Wiedergabeliste mithilfe von "-"
In den vorherigen Beispielen wird eine **MediaSource** aus einer Datei, einer URL oder einem Stream erstellt, nach der ein **mediaplaybackitem** erstellt und einer **mediaplaybacklist**hinzugefügt wird. In einigen Szenarien, z. b. wenn dem Benutzer die Anzeige von Inhalten in Rechnung gestellt wird, können Sie das Abrufen des Inhalts einer **MediaSource** verzögern, bis das Element in der Wiedergabeliste für die Wiedergabe bereit ist. Um dieses Szenario zu implementieren, erstellen Sie eine Instanz der Klasse " [**mediabinder**](/uwp/api/Windows.Media.Core.MediaBinder) ". Legen Sie die Eigenschaft [**Token**](/uwp/api/Windows.Media.Core.MediaBinder.Token) auf eine APP-definierte Zeichenfolge fest, die den Inhalt identifiziert, für den Sie den Abruf verzögern möchten, und registrieren Sie dann einen Handler für das [**Bindungs**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) Ereignis. Erstellen Sie als nächstes eine **MediaSource** aus dem **Binder** , indem Sie [**MediaSource. createfrommediabinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder)aufrufen. Erstellen Sie dann ein **mediaplaybackitem-Element** aus der **MediaSource** , und fügen Sie es wie gewohnt der Wiedergabeliste hinzu.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetInitMediaBinder":::

Wenn das System feststellt, dass der dem **mediabinder** zugeordnete Inhalt abgerufen werden muss, wird das **Bindungs** Ereignis aufgerufen. Im-Handler für dieses Ereignis können Sie die **-Instanz von** "" [**in das-**](/uwp/api/windows.media.core.mediabindingeventargs) Ereignis übertragen. Rufen Sie die Zeichenfolge ab, die Sie für die **tokeneigenschaft** angegeben haben, und verwenden Sie Sie, um zu bestimmen, welcher Inhalt Die Klasse "- **indingeventargs** " stellt Methoden zum Festlegen des gebundenen Inhalts in mehreren unterschiedlichen Darstellungen bereit, einschließlich " [**setstoragefile**](/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile)", " [**setStream**](/uwp/api/windows.media.core.mediabindingeventargs.setstream)", " [**setstreamreference**](/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference)" und " [**setURI**](/uwp/api/windows.media.core.mediabindingeventargs.seturi)". 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetBinderBinding":::

Beachten Sie Folgendes: Wenn Sie asynchrone Vorgänge (z. b. Webanforderungen) im **Bindungs** Ereignishandler ausführen, sollten Sie die Methode "-Methode" vom Typ "-Methode" aufrufen, [**um das System**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) anzuweisen, auf den Abschluss des Vorgangs zu warten, bevor Sie fortfahren. " [**Deferral. Complete**](/uwp/api/windows.foundation.deferral.Complete) " aufzurufen, nachdem der Vorgang beendet wurde, um das System anzuweisen, fortzufahren.

Ab Windows 10, Version 1703, können Sie eine [**adaptivemediasource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) als gebundenen Inhalt angeben, indem Sie [**setadaptivemediasource**](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)aufrufen. Weitere Informationen zur Verwendung von adaptiver Streaming in ihrer App finden Sie unter [Adaptive Streaming](adaptive-streaming.md).



## <a name="related-topics"></a>Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md)
* [Integrieren in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md)
