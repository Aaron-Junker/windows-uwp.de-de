---
author: drewbatgit
ms.assetid: F28162D4-AACC-4EE0-B243-5878F870F87F
description: Behandlung vom System unterstützter Metadaten-Marker während der Medienwiedergabe
title: Vom System unterstützte, zeitbasierte Metadaten-Marker
ms.author: drewbat
ms.date: 04/18/2017
ms.topic: article
keywords: Windows10, Uwp, Metadaten, Marker, Spracherkennung, Kapitel
ms.localizationpriority: medium
ms.openlocfilehash: 1e97c913764db24c68ce7becdba0fc283e1a3b73
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "5930618"
---
# <a name="system-supported-timed-metadata-cues"></a>Vom System unterstützte, zeitbasierte Metadaten-Marker
Dieser Artikel beschreibt, wie verschiedene Formate zeitbasierter Metadaten genutzt werden, die in Mediendateien oder -streams eingebettet sein können. UWP-Apps können Ereignisse registrieren, die von der Medienpipeline während der Wiedergabe ausgelöst werden, wenn diese Metadaten-Marker erkannt werden. Mithilfe der [**DataCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.DataCue)-Klasse können Apps eigene, benutzerdefinierte Metadaten-Marker implementieren; dieser Artikel behandelt mehrere Metadatenstandards, die von der Medienpipeline automatisch erkannt werden, z. B.:

* Bildbasierte Untertitel im VobSub-Format
* Spracherkennungs-Marker wie Wort-, Satzgrenzen und Lesezeichen der Speech Synthesis Markup Language (SSML)
* Kapitelmarker
* Erweiterte M3U-Kommentare
* ID3-Tags
* Fragmentierte MP4-Emsg-Felder


Dieser Artikel setzt die Konzepte des Artikels [Media-Elemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md) fort, liefert Grundlagen für die Arbeit mit den Klassen [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource), [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) und [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) sowie allgemeine Richtlinien für die Verwendung zeitgesteuerter Metadaten in Ihrer App.

Die grundlegenden Implementierungsschritte sind für alle in diesem Artikel beschriebenen zeitgesteuerten Metadatentypen gleich:

1. Erstellen Sie für den Inhalt, der wiedergegeben werden soll, eine [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) und dann ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem).
2. Registrieren Sie das [**MediaPlaybackItem.TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged)-Ereignis, das eintritt, wenn untergeordnete Spuren des Medienelements durch die Medienpipeline aufgelöst werden.
3. Wenn Sie zeitgesteuerte Metadatenspuren verwenden möchten, registrieren Sie dafür die Ereignisse [**TimedMetadataTrack.CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und [**TimedMetadataTrack.CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited).
4. Aktualisieren Sie im **CueEntered**-Ereignishandler die Benutzeroberfläche anhand der Metadaten, die in den Ereignisargumenten übergeben werden. Sie können die UI erneut im **CueExited**-Ereignis aktualisieren, z.B. um aktuelle Untertiteltexte zu entfernen.

Dieser Artikel zeigt die Behandlung jedes Metadatentyps als eindeutiges Szenario; Sie können jedoch mithilfe von gemeinsam genutztem Code verschiedene Metadatentypen behandeln (oder ignorieren). Die [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind)-Eigenschaft des [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Objekts kann während des Vorgangs an mehreren Punkten geprüft werden. So können Sie z. B. das **CueEntered**-Ereignis für Metadatenspuren mit dem Wert **TimedMetadataKind.ImageSubtitle** registrieren, jedoch nicht für Spuren mit dem Wert **TimedMetadataKind.Speech**. Oder Sie können für alle Metadatenspurtypen einen Handler registrieren, um anhand des **TimedMetadataKind**-Werts im **CueEntered**-Ereignishandler festzustellen, wie auf den Marker reagiert werden soll.

## <a name="image-based-subtitles"></a>Bildbasierte Untertitel
Ab Windows10 Version 1703 können UWP-Apps externe bildbasierte Untertitel im VobSub-Format unterstützen. Um dieses Feature zu nutzen, erstellen Sie ein [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)-Objekt für den Medieninhalt, der bildbasierte Untertitel enthält. Dann erstellen Sie ein [**TimedTextSource**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource)-Objekt, indem Sie mit dem Aufruf von [**CreateFromUriWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromUriWithIndex) oder [**CreateFromStreamWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromStreamWithIndex) den URI für die .sub- und .idx-Dateien übergeben, welche die Untertitel-Bild- und Zeitdaten enthalten. Fügen Sie die **TimedTextSource** Ihrer **MediaSource** und der [**ExternalTimedTextSources**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.ExternalTimedTextSources)-Sammlung Ihrer Quelle hinzu. Erstellen Sie ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[ImageSubtitleLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleLoadContent)]

Registrieren Sie mithilfe des im vorherigen Schritterstellten **MediaPlaybackItem**-Objekts die Metadaten-Ereignisse für die bildbasierten Untertitel. Dieses Beispiel verwendet die Hilfsmethode **RegisterMetadataHandlerForImageSubtitles**, um die Ereignisse zu registrieren. Mithilfe eines Lambda-Ausdrucks implementieren wir einen Handler für das [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged)-Ereignis, das auftritt, wenn das System die Änderung einer dem **MediaPlaybackItem** zugeordneten Metadatenspuren bemerkt. Falls Metadatenspuren schon verfügbar sind, weil das Wiedergabeelement zu Beginn aufgelöst wurde, durchsuchen Sie außer dem **TimedMetadataTracksChanged**-Ereignishandler alle verfügbaren Metadatenspuren und rufen **RegisterMetadataHandlerForImageSubtitles** auf.

[!code-cs[ImageSubtitleTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleTracksChanged)]

Nach der Registrierung der Metadaten-Ereignisse für bildbasierte Untertitel wird das **MediaItem** für die Wiedergabe einem [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) als [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) zugewiesen.

[!code-cs[ImageSubtitlePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitlePlay)]

Rufen Sie in der **RegisterMetadataHandlerForImageSubtitles**-Hilfsmethode eine Instanz der [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Klasse ab, indem Sie es in der **TimedMetadataTracks**-Sammlung des **MediaPlaybackItem** indizieren. Registrieren Sie die Ereignisse [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Anschließend müssen Sie [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) in der **TimedMetadataTracks**-Sammlung des Wiedergabeelements aufrufen, um das System anzuweisen, für dieses Wiedergabeelement Metadaten-Marker-Ereignisse an die App zu übergeben.

[!code-cs[RegisterMetadataHandlerForImageSubtitles](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForImageSubtitles)]

Prüfen Sie im Handler für das **CueEntered**-Ereignis die [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind)-Eigenschaft des im Ereignishandler übergebenen [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Objekts, um festzustellen, ob die Metadaten bildbasierte Untertitel betreffen. Dies ist notwendig, wenn Sie für mehrere Metadatentypen den gleichen Datenmarker-Ereignishandler verwenden. Falls die zugeordnete Metadatenspur vom Typ **TimedMetadataKind.ImageSubtitle** ist, übertragen Sie den in der **Cue**-Eigenschaft enthaltenen Datenmarker der [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) auf einen [**ImageCue**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue). Die [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.SoftwareBitmap)-Eigenschaft der **ImageCue** enthält eine [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap)-Darstellung des Untertitel-Bildes. Erstellen Sie eine [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource) und rufen Sie [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.SetBitmapAsync) auf, um das Bild einem XAML-[**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image)-Steuerelement zuzuweisen. Die Eigenschaften [**Extent**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Extent) und [**Position**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Position) der **ImageCue** liefern Informationen über die Größe und Position des Untertitel-Bildes.

[!code-cs[ImageSubtitleCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleCueEntered)]

## <a name="speech-cues"></a>Spracherkennungs-Marker
Ab Windows10 Version 1703 können UWP-Apps Handler für empfangene Wort- und Satzgrenzen sowie für Lesezeichen der Speech Synthesis Markup Language (SSML) in wiedergegebenen Medien registrieren. Durch diese Ereignisse können Sie Audiodatenstroms wiedergeben, die mit der [**SpeechSynthesizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer)-Klasse generiert wurden, und Ihre Benutzeroberfläche aktualisieren, um z.B. aktuell wiedergegebenen Text anzuzeigen.

Das Beispiel in diesem Abschnittverwendet eine Klassenmembervariable, um eine Zeichenfolge zu speichern, die synthetisiert und wiedergegeben wird.

[!code-cs[SpeechInputText](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechInputText)]

Erstellen Sie eine neue Instanz der **SpeechSynthesizer**-Klasse. Legen Sie die Optionen [**IncludeWordBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeWordBoundaryMetadata) und [**IncludeSentenceBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeSentenceBoundaryMetadata) für den Synthesizer auf **true** fest, um die Metadaten in den generierten Datenstrom einzubinden. Durch den Aufruf [**SynthesizeTextToStreamAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer.SynthesizeTextToStreamAsync) erzeugen Sie einen Datenstrom, der die synthetisierte Spracherkennung und zugehörige Metadaten enthält. Erstellen Sie aus dem synthetisierten Datenstrom eine [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) und ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem).

[!code-cs[SynthesizeSpeech](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSynthesizeSpeech)]

Registrieren Sie die Spracherkennungs-Metadaten-Ereignisse mithilfe des **MediaPlaybackItem**-Objekts. Dieses Beispiel verwendet die Hilfsmethode **RegisterMetadataHandlerForSpeech**, um die Ereignisse zu registrieren. Mithilfe eines Lambda-Ausdrucks implementieren wir einen Handler für das [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged)-Ereignis, das auftritt, wenn das System die Änderung einer dem **MediaPlaybackItem** zugeordneten Metadatenspuren bemerkt.  Falls Metadatenspuren schon verfügbar sind, weil das Wiedergabeelement zu Beginn aufgelöst wurde, durchsuchen Sie außer dem **TimedMetadataTracksChanged**-Ereignishandler alle verfügbaren Metadatenspuren und rufen **RegisterMetadataHandlerForSpeech** auf.

[!code-cs[SpeechTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechTracksChanged)]

Nach der Registrierung der Spracherkennungs-Metadaten-Ereignisse wird das **MediaItem** für die Wiedergabe einem [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) als [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) zugewiesen.

[!code-cs[SpeechPlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechPlay)]

Rufen Sie in der **RegisterMetadataHandlerForSpeech**-Hilfsmethode eine Instanz der [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Klasse ab, indem Sie es in der **TimedMetadataTracks**-Sammlung des **MediaPlaybackItem** indizieren. Registrieren Sie die Ereignisse [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Anschließend müssen Sie [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) in der **TimedMetadataTracks**-Sammlung des Wiedergabeelements aufrufen, um das System anzuweisen, für dieses Wiedergabeelement Metadaten-Marker-Ereignisse an die App zu übergeben.

[!code-cs[RegisterMetadataHandlerForWords](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForWords)]

Prüfen Sie im Handler für das **CueEntered**-Ereignis die [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind)-Eigenschaft des im Ereignishandler übergebenen [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Objekts, um festzustellen, ob die Metadaten sprachbasierte Untertitel betreffen. Dies ist notwendig, wenn Sie für mehrere Metadatentypen den gleichen Datenmarker-Ereignishandler verwenden. Falls die zugeordnete Metadatenspur vom Typ **TimedMetadataKind.Speech** ist, übertragen Sie den in der **Cue**-Eigenschaft enthaltenen Datenmarker der [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) auf einen [**SpeechCue**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue). Der Typ der in Metadatenspuren enthaltenen Spracherkennungs-Marker wird durch die **Label**-Eigenschaft bestimmt. Mögliche Werte dieser Eigenschaft sind „SpeechWord“ für Wortgrenzen, „SpeechSentence” für Satzgrenzen oder „SpeechBookmark“ für SSML-Lesezeichen. In diesem Beispiel prüfen wir den Wert „SpeechWord”; ist dieser Wert vorhanden, wird durch die Eigenschaften [**StartPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.StartPositionInInput) und [**EndPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.EndPositionInInput) der **SpeechCue** die Position des aktuell wiedergegebenen Wortes im eingegebenen Text ermittelt. Dieses Beispiel übergibt jedes Wort einfach an die Debugausgabe.

[!code-cs[SpeechWordCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechWordCueEntered)]

## <a name="chapter-cues"></a>Kapitelmarker
Ab Windows10 Version 1703 können UWP-Apps Marker registrieren, um die Kapitel eines Medienelements zuzuordnen. Um dieses Feature zu nutzen, erstellen Sie für den Medieninhalt zuerst ein [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)-Objekt und dann ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[ChapterCueLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueLoadContent)]

Registrieren Sie mithilfe des im vorherigen Schritterstellten **MediaPlaybackItem**-Objekts die Metadaten-Ereignisse für Kapitel. Dieses Beispiel verwendet die Hilfsmethode **RegisterMetadataHandlerForChapterCues**, um die Ereignisse zu registrieren. Mithilfe eines Lambda-Ausdrucks implementieren wir einen Handler für das [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged)-Ereignis, das auftritt, wenn das System die Änderung einer dem **MediaPlaybackItem** zugeordneten Metadatenspuren bemerkt. Falls Metadatenspuren schon verfügbar sind, weil das Wiedergabeelement zu Beginn aufgelöst wurde, durchsuchen Sie außer dem **TimedMetadataTracksChanged**-Ereignishandler alle verfügbaren Metadatenspuren und rufen **RegisterMetadataHandlerForChapterCues** auf.

[!code-cs[ChapterCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueTracksChanged)]

Nach der Registrierung der Kapitel-Metadaten-Ereignisse wird das **MediaItem** für die Wiedergabe einem [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) als [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) zugewiesen.

[!code-cs[ChapterCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCuePlay)]

Rufen Sie in der **RegisterMetadataHandlerForChapterCues**-Hilfsmethode eine Instanz der [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Klasse ab, indem Sie es in der **TimedMetadataTracks**-Sammlung des **MediaPlaybackItem** indizieren. Registrieren Sie die Ereignisse [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Anschließend müssen Sie [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) in der **TimedMetadataTracks**-Sammlung des Wiedergabeelements aufrufen, um das System anzuweisen, für dieses Wiedergabeelement Metadaten-Marker-Ereignisse an die App zu übergeben.

[!code-cs[RegisterMetadataHandlerForChapterCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForChapterCues)]

Prüfen Sie im Handler für das **CueEntered**-Ereignis die [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind)-Eigenschaft des im Ereignishandler übergebenen [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Objekts, um festzustellen, ob die Metadaten Kapitel betreffen. Dies ist notwendig, wenn Sie für mehrere Metadatentypen den gleichen Datenmarker-Ereignishandler verwenden. Falls die zugeordnete Metadatenspur vom Typ **TimedMetadataKind.Chapter** ist, übertragen Sie den in der **Cue**-Eigenschaft enthaltenen Datenmarker der [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) auf einen [**ChapterCue**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue). Die [**Title**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.Title)-Eigenschaft des **ChapterCue** enthält den Titel des Kapitels, das in der Wiedergabe aktuell erreicht wurde.

[!code-cs[ChapterCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueEntered)]

### <a name="seek-to-the-next-chapter-using-chapter-cues"></a>Suchen des nächsten Kapitelsdurch Kapitelmarker
Außer Benachrichtigungen zu erhalten, wenn sich das aktuelle Kapiteleines Wiedergabeelements ändert, können Sie durch Kapitelmarker auch zum nächsten Kapitelinnerhalb des Wiedergabeelements springen. Die folgende Beispielmethode akzeptiert die Argumente [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) sowie für das aktuell wiedergegebene Medienelement ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem). In der [**TimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracks)-Sammlung wird gesucht, ob einer der Titel die Eigenschaft [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) des Werts [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) von **TimedMetadataKind.Chapter** aufweist.  Wird eine Kapitelspur gefunden, durchsucht die Methode alle Marker in der [**Cues**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.Cues)-Sammlung des Titels nach dem ersten Marker, dessen [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.StartTime)-Wert größer ist als die aktuelle [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.Position) in der MediaPlayer-Wiedergabe. Sobald der korrekte Marker gefunden ist, werden die aktuelle Wiedergabeposition und die Kapitel auf der Benutzeroberfläche aktualisiert.

[!code-cs[GoToNextChapter](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetGoToNextChapter)]

## <a name="extended-m3u-comments"></a>Erweiterte M3U-Kommentare
Ab Windows10 Version 1703 können UWP-Apps Marker registrieren, die Kommentaren in erweiterten M3U-Manifestdateien zugeordnet sind. Dieses Beispiel verwendet [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource), um Medieninhalte wiederzugeben. Weitere Informationen finden Sie unter [Adaptives Streaming](adaptive-streaming.md). Erstellen Sie für den Inhalt eine **AdaptiveMediaSource** durch den Aufruf von [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) oder [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Erstellen Sie zuerst ein [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)-Objekt durch den Aufruf [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) und dann ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

Registrieren Sie die M3U-Metadaten-Ereignisse mithilfe des im vorherigen Schritterstellten **MediaPlaybackItem**-Objekts. Dieses Beispiel verwendet die Hilfsmethode **RegisterMetadataHandlerForEXTM3UCues**, um die Ereignisse zu registrieren. Mithilfe eines Lambda-Ausdrucks implementieren wir einen Handler für das [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged)-Ereignis, das auftritt, wenn das System die Änderung einer dem **MediaPlaybackItem** zugeordneten Metadatenspuren bemerkt. Falls Metadatenspuren schon verfügbar sind, weil das Wiedergabeelement zu Beginn aufgelöst wurde, durchsuchen Sie außer dem **TimedMetadataTracksChanged**-Ereignishandler alle verfügbaren Metadatenspuren und rufen **RegisterMetadataHandlerForEXTM3UCues** auf.

[!code-cs[EXTM3UCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueTracksChanged)]

Nach der Registrierung der M3U-Metadaten-Ereignisse wird das **MediaItem** für die Wiedergabe einem [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) als [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) zugewiesen.

[!code-cs[EXTM3UCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCuePlay)]

Rufen Sie in der **RegisterMetadataHandlerForEXTM3UCues**-Hilfsmethode eine Instanz der [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Klasse ab, indem Sie es in der **TimedMetadataTracks**-Sammlung des **MediaPlaybackItem** indizieren. Prüfen Sie die Dispatchtyp-Eigenschaft der Metadatenspur; diese weist den Wert „EXTM3U“ auf, wenn die Spur M3U-Kommentare darstellt. Registrieren Sie die Ereignisse [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Anschließend müssen Sie [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) in der **TimedMetadataTracks**-Sammlung des Wiedergabeelements aufrufen, um das System anzuweisen, für dieses Wiedergabeelement Metadaten-Marker-Ereignisse an die App zu übergeben.

[!code-cs[RegisterMetadataHandlerForEXTM3UCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEXTM3UCues)]

Im Handler für das **CueEntered**-Ereignis übertragen Sie die in der **Cues**-Eigenschaft des [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) enthaltenen Datenmarker auf einen [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue).  Stellen Sie sicher, dass die Eigenschaften **DataCue** und [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) des Markers nicht „Null” lauten. Erweiterte EWU-Kommentare werden als Zeichenfolgen bereitgestellt in der Form UTF-16, little endian, null-beendet. Erstellen Sie zum Lesen der Markerdaten einen neuen **DataReader** durch den Aufruf [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer). Legen Sie die [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding)-Eigenschaft des Readers auf [**Utf16LE**](https://docs.microsoft.com/uwp/api/windows.storage.streams.unicodeencoding) fest, damit die Daten im richtigen Format gelesen werden. Um die Daten zu lesen, müssen Sie [**ReadString**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.ReadString) aufrufen, dann die Hälfte des **Data**-Feldes ausfüllen und, da jedes Zeichen zwei Byte groß ist, eines abziehen, um das letzte Null-Zeichen zu entfernen. In diesem Beispiel wird der M3U-Kommentar einfach in die Debugausgabe geschrieben.

[!code-cs[EXTM3UCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueEntered)]

## <a name="id3-tags"></a>ID3-Tags
Ab Windows10 Version 1703 können UWP-Apps Marker registrieren, die ID3-Tags im Inhalt von Http Live Streaming (HLS) zugeordnet sind. Dieses Beispiel verwendet [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource), um Medieninhalte wiederzugeben. Weitere Informationen finden Sie unter [Adaptives Streaming](adaptive-streaming.md). Erstellen Sie für den Inhalt eine **AdaptiveMediaSource** durch den Aufruf von [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) oder [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Erstellen Sie zuerst ein [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)-Objekt durch den Aufruf [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) und dann ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

Registrieren Sie die ID3-Tag-Ereignisse mithilfe des im vorherigen Schritterstellten **MediaPlaybackItem**-Objekts. Dieses Beispiel verwendet die Hilfsmethode **RegisterMetadataHandlerForID3Cues**, um die Ereignisse zu registrieren. Mithilfe eines Lambda-Ausdrucks implementieren wir einen Handler für das [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged)-Ereignis, das auftritt, wenn das System die Änderung einer dem **MediaPlaybackItem** zugeordneten Metadatenspuren bemerkt. Falls Metadatenspuren schon verfügbar sind, weil das Wiedergabeelement zu Beginn aufgelöst wurde, durchsuchen Sie außer dem **TimedMetadataTracksChanged**-Ereignishandler alle verfügbaren Metadatenspuren und rufen **RegisterMetadataHandlerForID3Cues** auf.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Nach der Registrierung der ID3-Metadaten-Ereignisse wird das **MediaItem** für die Wiedergabe einem [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) als [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) zugewiesen.

[!code-cs[ID3CuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CuePlay)]


Rufen Sie in der **RegisterMetadataHandlerForID3Cues**-Hilfsmethode eine Instanz der [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Klasse ab, indem Sie es in der **TimedMetadataTracks**-Sammlung des **MediaPlaybackItem** indizieren. Prüfen Sie die Dispatchtyp-Eigenschaft der Metadatenspur; diese enthalten die GUID-Zeichenfolge „15260DFFFF49443320FF49443320000F”, wenn die Spur ID3-Tags darstellt. Registrieren Sie die Ereignisse [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Anschließend müssen Sie [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) in der **TimedMetadataTracks**-Sammlung des Wiedergabeelements aufrufen, um das System anzuweisen, für dieses Wiedergabeelement Metadaten-Marker-Ereignisse an die App zu übergeben.

[!code-cs[RegisterMetadataHandlerForID3Cues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForID3Cues)]

Im Handler für das **CueEntered**-Ereignis übertragen Sie die in der **Cues**-Eigenschaft des [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) enthaltenen Datenmarker auf einen [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue).  Stellen Sie sicher, dass die Eigenschaften **DataCue** und [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) des Markers nicht „Null” lauten. Erweiterte EWU-Kommentare werden im Transportstream als unformatierte Bytes bereitgestellt (siehe [http://id3.org/id3v2.4.0-structure](http://id3.org/id3v2.4.0-structure)). Erstellen Sie zum Lesen der Markerdaten einen neuen **DataReader** durch den Aufruf [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer).  In diesem Beispiel werden die Kopfzeilenwerte des ID3-Tags aus den Markerdaten gelesen und in die Debugausgabe geschrieben.

[!code-cs[ID3CueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CueEntered)]

## <a name="fragmented-mp4-emsg-boxes"></a>Fragmentierte MP4-Emsg-Felder
Ab Windows10 Version 1703 können UWP-Apps Marker registrieren, die Emsg-Feldern in fragmentierten MP4-Streams zugeordnet sind. Diesen Metadatentyp können z. B. Inhaltsanbieter verwenden, um Clientanwendungen die Wiedergabe von Anzeigen in Live-Streaming-Inhalten zu signalisieren. Dieses Beispiel verwendet [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource), um Medieninhalte wiederzugeben. Weitere Informationen finden Sie unter [Adaptives Streaming](adaptive-streaming.md). Erstellen Sie für den Inhalt eine **AdaptiveMediaSource** durch den Aufruf von [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) oder [**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Erstellen Sie zuerst ein [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)-Objekt durch den Aufruf [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) und dann ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[EmsgLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgLoadContent)]

Registrieren Sie die Emsg-Feld-Ereignisse mithilfe des im vorherigen Schritterstellten **MediaPlaybackItem**-Objekts. Dieses Beispiel verwendet die Hilfsmethode **RegisterMetadataHandlerForEmsgCues**, um die Ereignisse zu registrieren. Mithilfe eines Lambda-Ausdrucks implementieren wir einen Handler für das [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged)-Ereignis, das auftritt, wenn das System die Änderung einer dem **MediaPlaybackItem** zugeordneten Metadatenspuren bemerkt. Falls Metadatenspuren schon verfügbar sind, weil das Wiedergabeelement zu Beginn aufgelöst wurde, durchsuchen Sie außer dem **TimedMetadataTracksChanged**-Ereignishandler alle verfügbaren Metadatenspuren und rufen **RegisterMetadataHandlerForEmsgCues** auf.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Nach der Registrierung der Emsg-Feld-Ereignisse wird das **MediaItem** für die Wiedergabe einem [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) als [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) zugewiesen.

[!code-cs[EmsgCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCuePlay)]


Rufen Sie in der **RegisterMetadataHandlerForEmsgCues**-Hilfsmethode eine Instanz der [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack)-Klasse ab, indem Sie es in der **TimedMetadataTracks**-Sammlung des **MediaPlaybackItem** indizieren. Prüfen Sie die Dispatchtyp-Eigenschaft der Metadatenspur; diese enthält den Wert „emsg:mp4”, wenn die Spur Emsg-Felder darstellt. Registrieren Sie die Ereignisse [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited). Anschließend müssen Sie [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) in der **TimedMetadataTracks**-Sammlung des Wiedergabeelements aufrufen, um das System anzuweisen, für dieses Wiedergabeelement Metadaten-Marker-Ereignisse an die App zu übergeben.


[!code-cs[RegisterMetadataHandlerForEmsgCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEmsgCues)]


Im Handler für das **CueEntered**-Ereignis übertragen Sie die in der **Cues**-Eigenschaft des [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) enthaltenen Datenmarker auf einen [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue).  Stellen Sie sicher, dass das **DataCue**-Objekt nicht „Null“ lautet. Die Eigenschaften des Emsg-Feldes werden von der Medienpipeline in der [**Properties**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Properties)-Sammlung als benutzerdefinierte Eigenschaften des Datenmarker-Objekts bereitgestellt. In diesem Beispiel wird versucht, mehrere verschiedene Eigenschaftswerte mithilfe der **[TryGetValue](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset.trygetvalue)**-Methode zu extrahieren. Wenn diese Methode „Null“ zurückgibt, ist die angeforderte Eigenschaft nicht im Emsg-Feld vorhanden; stattdessen wird ein Standardwert festgelegt.

Der nächste Teil des Beispiels zeigt, wie die Wiedergabe einer Anzeige ausgelöst wird, weil die im vorherigen Schrittermittelte Eigenschaft *Scheme_id_uri* den Wert „urn: scte:scte35:2013:xml” aufweist (siehe [http://dashif.org/identifiers/event-schemes/](http://dashif.org/identifiers/event-schemes/)). Beachten Sie, dass der Standard empfiehlt, das Emsg mehrmals redundant zu senden; dieses Beispiel führt daher eine Liste bereits verarbeiteter Emsg-IDs, damit nur neue Nachrichten verarbeitet werden. Erstellen Sie vor dem Lesen der Markerdaten einen neuen **DataReader** durch den Aufruf [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer) und legen Sie durch die [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding)-Eigenschaft UTF-8 als Codierung fest. In diesem Beispiel wird die Meldungsnutzlast in die Debugausgabe geschrieben. Echte Apps verwenden diese Nutzlastdaten, um die Wiedergabe von Anzeigen zu planen.

[!code-cs[EmsgCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCueEntered)]


## <a name="related-topics"></a>Verwandte Themen

* [Medienwiedergabe](media-playback.md)
* [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md)


 




