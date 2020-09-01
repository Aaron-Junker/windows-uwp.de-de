---
ms.assetid: F28162D4-AACC-4EE0-B243-5878F870F87F
description: Erfahren Sie, wie Sie verschiedene Formate von zeitgesteuerten Metadaten nutzen können, die in Mediendateien oder Streams eingebettet werden können.
title: Vom System unterstützte, zeitbasierte Metadaten-Marker
ms.date: 04/18/2017
ms.topic: article
keywords: Windows 10, UWP, Metadaten, Hinweis, Sprache, Kapitel
ms.localizationpriority: medium
ms.openlocfilehash: 4ac8140abc1c93e8e2b249bb6040cef3789f1d6a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175674"
---
# <a name="system-supported-timed-metadata-cues"></a>Vom System unterstützte, zeitbasierte Metadaten-Marker
In diesem Artikel wird beschrieben, wie Sie verschiedene Formate von zeitgesteuerten Metadaten nutzen können, die in Mediendateien oder Streams eingebettet werden können. UWP-Apps können für Ereignisse registriert werden, die während der Wiedergabe von der Medien Pipeline ausgelöst werden, wenn diese metadatenhinweise gefunden werden. Mithilfe der [**datacue**](/uwp/api/Windows.Media.Core.DataCue) -Klasse können Apps ihre eigenen benutzerdefinierten metadatenhinweise implementieren, aber dieser Artikel konzentriert sich auf verschiedene Metadatenstandards, die automatisch von der Medien Pipeline erkannt werden, einschließlich:

* Bildbasierte Untertitel im VOBsub-Format
* Sprach Hinweise, einschließlich Wortgrenzen, Satz Begrenzungen und sprach-Synthese Markup Sprache (SSML)-Lesezeichen
* Kapitel Hinweise
* Erweiterte M3U-Kommentare
* ID3-Tags
* Fragmentierte MP4-emsg-Felder


Dieser Artikel baut auf den Konzepten auf, die im Artikel [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md)erläutert werden, einschließlich der Grundlagen der Arbeit mit den Klassen " [**MediaSource**](/uwp/api/windows.media.core.mediasource)", " [**mediaplaybackitem**](/uwp/api/windows.media.playback.mediaplaybackitem)" und " [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack) " sowie allgemeine Anleitungen zum Verwenden von zeitgesteuerten Metadaten in Ihrer APP.

Die grundlegenden Implementierungs Schritte sind für alle unterschiedlichen Typen von zeitgesteuerten Metadaten identisch, die in diesem Artikel beschrieben werden:

1. Erstellen Sie eine [**MediaSource**](/uwp/api/windows.media.core.mediasource) und dann ein [**mediaplaybackitem-Element**](/uwp/api/windows.media.playback.mediaplaybackitem) für die Wiedergabe des Inhalts.
2. Registrieren Sie sich für das [**mediaplaybackitem. TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) -Ereignis, das auftritt, wenn die untergeordneten Titel des Medien Elements von der Medien Pipeline aufgelöst werden.
3. Registrieren Sie sich für das [**TimedMetadataTrack. cueeingetragen**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) -Ereignis und das [**TimedMetadataTrack. cueexited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) -Ereignis für die zeitgesteuerten metadatenspuren, die Sie verwenden möchten.
4. Aktualisieren Sie die Benutzeroberfläche im **cueeingetragen** -Ereignishandler auf der Grundlage der in den Ereignis Argumenten übergebenen Metadaten. Sie können die Benutzeroberfläche erneut aktualisieren, um den aktuellen Untertitel Text beispielsweise im Ereignis **cueexited** zu entfernen.

In diesem Artikel wird die Behandlung der einzelnen Metadatentypen als eindeutiges Szenario dargestellt, aber es ist möglich, unterschiedliche Metadatentypen mithilfe von größtenteils frei gegebenem Code zu verarbeiten (oder zu ignorieren). Sie können die [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) -Eigenschaft des [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Objekts an mehreren Punkten im Prozess überprüfen. Sie können sich z. b. für das **cueeingetragen** -Ereignis für metadatenspuren registrieren, die den Wert **TimedMetadataKind. imageunter Titel**aufweisen, aber nicht für Titel mit dem Wert **TimedMetadataKind. Speech**. Stattdessen können Sie einen Handler für alle metadatentracktypen registrieren und dann den **TimedMetadataKind** -Wert innerhalb des **cueeingetragen** -Handlers überprüfen, um zu bestimmen, welche Aktion als Reaktion auf den Hinweis ausgeführt werden soll.

## <a name="image-based-subtitles"></a>Bildbasierte Untertitel
Ab Windows 10, Version 1703, können UWP-apps externe bildbasierte Untertitel im VOBsub-Format unterstützen. Um dieses Feature zu verwenden, erstellen Sie zunächst ein [**MediaSource**](/uwp/api/windows.media.core.mediasource) -Objekt für den Medieninhalt, für den Bild-Untertitel angezeigt werden. Erstellen Sie als nächstes ein [**timedtextsource**](/uwp/api/windows.media.core.timedtextsource) -Objekt, indem Sie [**createfromuriwithindex**](/uwp/api/windows.media.core.timedtextsource.CreateFromUriWithIndex) oder [**createfromstreamwithindex**](/uwp/api/windows.media.core.timedtextsource.CreateFromStreamWithIndex)aufrufen. übergeben Sie dabei den URI der Sub-Datei, die die Untertitel Bilddaten enthält, und die IDX-Datei, die die Zeit Steuerungsinformationen für die Untertitel enthält. Fügen Sie der **MediaSource** die **timedtextsource** hinzu, indem Sie Sie der [**externaltimedtextsources**](/uwp/api/windows.media.core.mediasource.ExternalTimedTextSources) -Auflistung der Quelle hinzufügen. Erstellen Sie ein [**mediaplaybackitem-Element**](/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[ImageSubtitleLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleLoadContent)]

Registrieren Sie sich mit dem im vorherigen Schritt erstellten **mediaplaybackitem** -Objekt für die Metadatenereignisse des Bild Untertitels. In diesem Beispiel wird eine Hilfsmethode, **RegisterMetadataHandlerForImageSubtitles**, verwendet, um sich für die-Ereignisse zu registrieren. Ein Lambda-Ausdruck wird zum Implementieren eines Handlers für das [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) -Ereignis verwendet, das auftritt, wenn das System eine Änderung in den mit einem **mediaplaybackitem**verknüpften metadatenspuren erkennt. In einigen Fällen sind die metadatenspuren möglicherweise verfügbar, wenn das Wiedergabe Element anfänglich aufgelöst wird. Daher werden außerhalb des **TimedMetadataTracksChanged** -Handlers auch die verfügbaren metadatenspuren durchlaufen und **RegisterMetadataHandlerForImageSubtitles**aufgerufen.

[!code-cs[ImageSubtitleTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleTracksChanged)]

Nach der Registrierung für die Metadatenereignisse des Image-Untertitels wird das **mediaitem** einem [**Media Player**](/uwp/api/windows.media.playback.mediaplayer) für die Wiedergabe in einem [**mediaplayerelement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)zugewiesen.

[!code-cs[ImageSubtitlePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitlePlay)]

In der **RegisterMetadataHandlerForImageSubtitles** -Hilfsmethode erhalten Sie eine Instanz der [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Klasse, indem Sie Sie in die **TimedMetadataTracks** -Auflistung von **mediaplaybackitem**indizieren. Registrieren Sie sich für das Ereignis [**cueeingetragen**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und das Ereignis [**cueexited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Anschließend müssen Sie [**setpresentationmode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) für die **TimedMetadataTracks** -Auflistung des Wiedergabe Elements aufrufen, um das System anzuweisen, dass die APP metadatenandereignisse für dieses Wiedergabe Element empfangen möchte.

[!code-cs[RegisterMetadataHandlerForImageSubtitles](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForImageSubtitles)]

Im Handler für das **cueinto** -Ereignis können Sie die [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) -Eigenschaft des [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Objekts überprüfen, das an den Handler übergebenen wird, um zu überprüfen, ob die Metadaten für Bild-Untertitel vorgesehen sind. Dies ist erforderlich, wenn Sie für mehrere Metadatentypen denselben Daten Ereignishandler verwenden. Wenn der zugeordnete metadatentitel vom Typ **TimedMetadataKind. imageunter Titel**ist, wandeln Sie den Daten Hinweis, der in **der Eigenschaft "** Hinweis" von [**mediacueeventargs**](/uwp/api/windows.media.core.mediacueeventargs) enthalten ist, in einen [**imagecue**](/uwp/api/windows.media.core.imagecue)um. Die [**softwarebitmap**](/uwp/api/windows.media.core.imagecue.SoftwareBitmap) -Eigenschaft von **imagecue** enthält eine [**softwarebitmap**](/uwp/api/windows.graphics.imaging.softwarebitmap) -Darstellung des Untertitel Bilds. Erstellen Sie eine [**softwarebitmapsource**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource) , und rufen Sie [**setbitmapasync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.SetBitmapAsync) auf, um das Bild einem XAML- [**Bild**](/uwp/api/windows.ui.xaml.controls.image) Steuerelement zuzuweisen. Die [**Block-und**](/uwp/api/windows.media.core.imagecue.Extent) [**Positions**](/uwp/api/windows.media.core.imagecue.Position) Eigenschaften von **imagecue** enthalten Informationen über die Größe und Position des Untertitel Bilds.

[!code-cs[ImageSubtitleCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleCueEntered)]

## <a name="speech-cues"></a>Sprach Hinweise
Ab Windows 10, Version 1703, können sich UWP-apps registrieren, um Ereignisse als Reaktion auf Wortgrenzen, Satz Begrenzungen und Sprachsynthese-Markup Sprache (SSML)-Lesezeichen in wiedergegebenen Medien zu empfangen. Dies ermöglicht es Ihnen, mit der [**Sprachsynthesizer**](/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) -Klasse generierte Audiostreams wiederzugeben und die Benutzeroberfläche auf der Grundlage dieser Ereignisse zu aktualisieren, z. b. zum Anzeigen des Texts des gerade Wiedergabe Worts oder Satzes.

Im Beispiel in diesem Abschnitt wird eine Klassenmember-Variable verwendet, um eine Text Zeichenfolge zu speichern, die synthetisiert und wiedergegeben wird.

[!code-cs[SpeechInputText](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechInputText)]

Erstellen Sie eine neue Instanz der sprechenden **Synthesizer** -Klasse. Legen Sie die [**includewordboundarymetadata**](/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeWordBoundaryMetadata) -und [**includesentenceboundarymetadata**](/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeSentenceBoundaryMetadata) -Optionen für den Synthesizer auf **true** fest, um anzugeben, dass die Metadaten in den generierten Mediendaten Strom eingeschlossen werden sollen. Rufen Sie [**synzetextdestreamasync**](/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer.SynthesizeTextToStreamAsync) auf, um einen Stream zu generieren, der die synthetisierte Sprache und die zugehörigen Metadaten enthält. Erstellen Sie eine [**MediaSource**](/uwp/api/windows.media.core.mediasource) und ein [**mediaplaybackitem**](/uwp/api/windows.media.playback.mediaplaybackitem) aus dem synthetisierten Stream.

[!code-cs[SynthesizeSpeech](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSynthesizeSpeech)]

Registrieren Sie sich für die sprachmetadatenereignisse mithilfe des **mediaplaybackitem** -Objekts. In diesem Beispiel wird eine Hilfsmethode, **RegisterMetadataHandlerForSpeech**, verwendet, um sich für die-Ereignisse zu registrieren. Ein Lambda-Ausdruck wird zum Implementieren eines Handlers für das [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) -Ereignis verwendet, das auftritt, wenn das System eine Änderung in den mit einem **mediaplaybackitem**verknüpften metadatenspuren erkennt.  In einigen Fällen sind die metadatenspuren möglicherweise verfügbar, wenn das Wiedergabe Element anfänglich aufgelöst wird. Daher werden außerhalb des **TimedMetadataTracksChanged** -Handlers auch die verfügbaren metadatenspuren durchlaufen und **RegisterMetadataHandlerForSpeech**aufgerufen.

[!code-cs[SpeechTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechTracksChanged)]

Nach der Registrierung für die sprachmetadatenereignisse wird das **mediaitem** einem [**Media Player**](/uwp/api/windows.media.playback.mediaplayer) für die Wiedergabe in einem [**mediaplayerelement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)zugewiesen.

[!code-cs[SpeechPlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechPlay)]

In der **RegisterMetadataHandlerForSpeech** -Hilfsmethode erhalten Sie eine Instanz der [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Klasse, indem Sie Sie in die **TimedMetadataTracks** -Auflistung von **mediaplaybackitem**indizieren. Registrieren Sie sich für das Ereignis [**cueeingetragen**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und das Ereignis [**cueexited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Anschließend müssen Sie [**setpresentationmode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) für die **TimedMetadataTracks** -Auflistung des Wiedergabe Elements aufrufen, um das System anzuweisen, dass die APP metadatenandereignisse für dieses Wiedergabe Element empfangen möchte.

[!code-cs[RegisterMetadataHandlerForWords](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForWords)]

Im Handler für das **cueinto** -Ereignis können Sie die [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) -Eigenschaft des [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Objekts überprüfen, das an den Handler übergebenen wird, um zu überprüfen, ob die Metadaten Sprache sind. Dies ist erforderlich, wenn Sie für mehrere Metadatentypen denselben Daten Ereignishandler verwenden. Wenn der zugeordnete metadatentitel vom Typ **TimedMetadataKind. Speech**ist, wandeln Sie den Daten Hinweis, der in **der Eigenschaft "** Hinweis" von [**mediacueeventargs**](/uwp/api/windows.media.core.mediacueeventargs) enthalten ist, [**in einen**](/uwp/api/windows.media.core.speechcue)sprechenden Hinweis um. Für sprach Hinweise wird der in der metadatenspur enthaltene Typ von sprach Hinweis festgelegt, indem die **Label** -Eigenschaft überprüft wird. Der Wert dieser Eigenschaft ist "Sprech Wort Wort" für Wortgrenzen, "sprach Satz" für Satz Begrenzungen oder "sprechende Lesezeichen" für SSML-Lesezeichen. In diesem Beispiel überprüfen wir den Wert "Redner Wort". Wenn dieser Wert gefunden wird **, werden die** [**startpositionininput**](/uwp/api/windows.media.core.speechcue.StartPositionInInput) -Eigenschaft und die [**endpositionininput**](/uwp/api/windows.media.core.speechcue.EndPositionInInput) -Eigenschaft des sprechenden Punkts verwendet, um die Position innerhalb des Eingabe Texts des zurzeit wiedergegebenen Worts zu bestimmen. In diesem Beispiel wird jedes Wort einfach an die Debugausgabe ausgegeben.

[!code-cs[SpeechWordCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechWordCueEntered)]

## <a name="chapter-cues"></a>Kapitel Hinweise
Ab Windows 10, Version 1703, können UWP-Apps für Cues registrieren, die den Kapiteln innerhalb eines Medien Elements entsprechen. Um dieses Feature zu verwenden, erstellen Sie ein [**MediaSource**](/uwp/api/windows.media.core.mediasource) -Objekt für den Medieninhalt, und erstellen Sie dann ein [**mediaplaybackitem**](/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[ChapterCueLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueLoadContent)]

Registrieren Sie sich für die Kapitel Metadatenereignisse mithilfe des **mediaplaybackitem** -Objekts, das im vorherigen Schritt erstellt wurde. In diesem Beispiel wird eine Hilfsmethode, **RegisterMetadataHandlerForChapterCues**, verwendet, um sich für die-Ereignisse zu registrieren. Ein Lambda-Ausdruck wird zum Implementieren eines Handlers für das [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) -Ereignis verwendet, das auftritt, wenn das System eine Änderung in den mit einem **mediaplaybackitem**verknüpften metadatenspuren erkennt. In einigen Fällen sind die metadatenspuren möglicherweise verfügbar, wenn das Wiedergabe Element anfänglich aufgelöst wird. Daher werden außerhalb des **TimedMetadataTracksChanged** -Handlers auch die verfügbaren metadatenspuren durchlaufen und **RegisterMetadataHandlerForChapterCues**aufgerufen.

[!code-cs[ChapterCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueTracksChanged)]

Nach der Registrierung für die Kapitel Metadatenereignisse wird das **mediaitem** einem [**Media Player**](/uwp/api/windows.media.playback.mediaplayer) für die Wiedergabe in einem [**mediaplayerelement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)zugewiesen.

[!code-cs[ChapterCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCuePlay)]

In der **RegisterMetadataHandlerForChapterCues** -Hilfsmethode erhalten Sie eine Instanz der [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Klasse, indem Sie Sie in die **TimedMetadataTracks** -Auflistung von **mediaplaybackitem**indizieren. Registrieren Sie sich für das Ereignis [**cueeingetragen**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und das Ereignis [**cueexited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Anschließend müssen Sie [**setpresentationmode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) für die **TimedMetadataTracks** -Auflistung des Wiedergabe Elements aufrufen, um das System anzuweisen, dass die APP metadatenandereignisse für dieses Wiedergabe Element empfangen möchte.

[!code-cs[RegisterMetadataHandlerForChapterCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForChapterCues)]

Im-Handler für das **cueinto** -Ereignis können Sie die [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) -Eigenschaft des [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Objekts überprüfen, das an den-Handler übergebenen wird, um zu überprüfen, ob die Metadaten für Kapitel Hinweise vorgesehen sind. Dies ist erforderlich, wenn Sie für mehrere Metadatentypen denselben Daten Ereignishandler verwenden. Wenn der zugeordnete metadatentitel vom Typ **TimedMetadataKind. Chapter**ist, wandeln Sie den Daten Hinweis, der in **der Eigenschaft "** Hinweis" von [**mediacueeventargs**](/uwp/api/windows.media.core.mediacueeventargs) enthalten ist, in einen " [**chaptercue**](/uwp/api/windows.media.core.chaptercue)" um. Die [**Title**](/uwp/api/windows.media.core.chaptercue.Title) -Eigenschaft von " **chaptercue** " enthält den Titel des Kapitels, das gerade bei der Wiedergabe erreicht wurde.

[!code-cs[ChapterCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueEntered)]

### <a name="seek-to-the-next-chapter-using-chapter-cues"></a>Suchen Sie im nächsten Kapitel mithilfe der Kapitel Hinweise.
Zusätzlich zum Empfang von Benachrichtigungen, wenn sich das aktuelle Kapitel in einem Spielelement ändert, können Sie auch Kapitel Hinweise verwenden, um das nächste Kapitel innerhalb eines Spiel Elements zu suchen. Die unten gezeigte Beispiel Methode hat einen [**Media Player**](/uwp/api/windows.media.playback.mediaplayer) als Argument und ein [**mediaplaybackitem-Element**](/uwp/api/windows.media.playback.mediaplaybackitem) , das das aktuell wiedergegebene Medien Element darstellt. Die [**TimedMetadataTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracks) -Auflistung wird durchsucht, um festzustellen, ob eine der Spuren [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) Eigenschaften des [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Werts von **TimedMetadataKind. Chapter**hat.  Wenn ein Kapiteltitel gefunden wird, durchläuft die-Methode jeden Hinweis in der [**Cues**](/uwp/api/windows.media.core.timedmetadatatrack.Cues) -Auflistung des Titels, um den ersten Hinweis zu finden, dessen [**StartTime**](/uwp/api/windows.media.core.chaptercue.StartTime) größer ist als die aktuelle [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.Position) der Wiedergabe Sitzung des Media Players. Sobald der richtige Hinweis gefunden wurde, wird die Position der Wiedergabe Sitzung aktualisiert, und der Kapiteltitel wird in der Benutzeroberfläche aktualisiert.

[!code-cs[GoToNextChapter](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetGoToNextChapter)]

## <a name="extended-m3u-comments"></a>Erweiterte M3U-Kommentare
Ab Windows 10, Version 1703, können UWP-Apps für Cues registrieren, die Kommentaren in einer erweiterten M3U Manifest-Datei entsprechen. Dieses Beispiel verwendet [**adaptivemediasource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) , um den Medieninhalt wiederzugeben. Weitere Informationen finden Sie unter [Adaptive Streaming](adaptive-streaming.md). Erstellen Sie eine **adaptivemediasource** für den Inhalt, indem Sie [**createfromuriasync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) oder [**createfromstreamasync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)aufrufen. Erstellen Sie ein  [**MediaSource**](/uwp/api/windows.media.core.mediasource) -Objekt durch Aufrufen von [**createfromadaptivemediasource**](/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) , und erstellen Sie dann ein [**mediaplaybackitem**](/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

Registrieren Sie sich für die M3U Metadata-Ereignisse mithilfe des im vorherigen Schritt erstellten **mediaplaybackitem** -Objekts. In diesem Beispiel wird eine Hilfsmethode, **RegisterMetadataHandlerForEXTM3UCues**, verwendet, um sich für die-Ereignisse zu registrieren. Ein Lambda-Ausdruck wird zum Implementieren eines Handlers für das [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) -Ereignis verwendet, das auftritt, wenn das System eine Änderung in den mit einem **mediaplaybackitem**verknüpften metadatenspuren erkennt. In einigen Fällen sind die metadatenspuren möglicherweise verfügbar, wenn das Wiedergabe Element anfänglich aufgelöst wird. Daher werden außerhalb des **TimedMetadataTracksChanged** -Handlers auch die verfügbaren metadatenspuren durchlaufen und **RegisterMetadataHandlerForEXTM3UCues**aufgerufen.

[!code-cs[EXTM3UCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueTracksChanged)]

Nach der Registrierung für die M3U Metadata-Ereignisse wird das **mediaitem** einem [**Media Player**](/uwp/api/windows.media.playback.mediaplayer) für die Wiedergabe in einem [**mediaplayerelement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)zugewiesen.

[!code-cs[EXTM3UCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCuePlay)]

In der **RegisterMetadataHandlerForEXTM3UCues** -Hilfsmethode erhalten Sie eine Instanz der [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Klasse, indem Sie Sie in die **TimedMetadataTracks** -Auflistung von **mediaplaybackitem**indizieren. Überprüfen Sie die dispatchType-Eigenschaft des metadatentitels, die den Wert "EXTM3U" hat, wenn die Nachverfolgung M3U-Kommentare darstellt. Registrieren Sie sich für das Ereignis [**cueeingetragen**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und das Ereignis [**cueexited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Anschließend müssen Sie [**setpresentationmode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) für die **TimedMetadataTracks** -Auflistung des Wiedergabe Elements aufrufen, um das System anzuweisen, dass die APP metadatenandereignisse für dieses Wiedergabe Element empfangen möchte.

[!code-cs[RegisterMetadataHandlerForEXTM3UCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEXTM3UCues)]

Wandeln Sie im Handler für das **cuecast** -Ereignis den in **der Eigenschaft "** Hinweis" von [**mediacueeventargs**](/uwp/api/windows.media.core.mediacueeventargs) enthaltenen Daten Hinweis in einen [**datacue**](/uwp/api/windows.media.core.datacue)um.  Stellen Sie sicher, dass der **datacue** und die [**Data**](/uwp/api/windows.media.core.datacue.Data) -Eigenschaft des-Hinweis nicht NULL sind. Erweiterte Emu-Kommentare werden in Form von UTF-16-, Little-Endian-und null-terminierten Zeichen folgen bereitgestellt. Erstellen Sie einen neuen **DataReader** , um die Daten zu lesen, indem Sie [**DataReader. frombuffer**](/uwp/api/windows.storage.streams.datareader.FromBuffer)aufrufen. Legen Sie die [**UnicodeEncoding**](/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) -Eigenschaft des Readers auf [**Utf16LE**](/uwp/api/windows.storage.streams.unicodeencoding) fest, um die Daten im richtigen Format zu lesen. Rufen Sie die Daten [**Zeichenfolge**](/uwp/api/windows.storage.streams.datareader.ReadString) auf, um die Daten zu lesen, und geben Sie die Hälfte der Länge des **Daten** Felds an, da jedes Zeichen zwei Bytes groß ist, und ziehen Sie es ein, um das nachfolgende NULL Zeichen zu entfernen. In diesem Beispiel wird der M3U-Kommentar einfach in die Debugausgabe geschrieben.

[!code-cs[EXTM3UCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueEntered)]

## <a name="id3-tags"></a>ID3-Tags
Ab Windows 10, Version 1703, können sich UWP-Apps für Cues registrieren, die in HTTP-Live Streaming-Inhalten (HLS) mit ID3-Tags übereinstimmen. Dieses Beispiel verwendet [**adaptivemediasource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) , um den Medieninhalt wiederzugeben. Weitere Informationen finden Sie unter [Adaptive Streaming](adaptive-streaming.md). Erstellen Sie eine **adaptivemediasource** für den Inhalt, indem Sie [**createfromuriasync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) oder [**createfromstreamasync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)aufrufen. Erstellen Sie ein  [**MediaSource**](/uwp/api/windows.media.core.mediasource) -Objekt durch Aufrufen von [**createfromadaptivemediasource**](/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) , und erstellen Sie dann ein [**mediaplaybackitem**](/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

Registrieren Sie sich für die ID3-Tagereignisse mithilfe des **mediaplaybackitem** -Objekts, das im vorherigen Schritt erstellt wurde. In diesem Beispiel wird eine Hilfsmethode, **RegisterMetadataHandlerForID3Cues**, verwendet, um sich für die-Ereignisse zu registrieren. Ein Lambda-Ausdruck wird zum Implementieren eines Handlers für das [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) -Ereignis verwendet, das auftritt, wenn das System eine Änderung in den mit einem **mediaplaybackitem**verknüpften metadatenspuren erkennt. In einigen Fällen sind die metadatenspuren möglicherweise verfügbar, wenn das Wiedergabe Element anfänglich aufgelöst wird. Daher werden außerhalb des **TimedMetadataTracksChanged** -Handlers auch die verfügbaren metadatenspuren durchlaufen und **RegisterMetadataHandlerForID3Cues**aufgerufen.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Nach der Registrierung für die ID3-Metadatenereignisse wird das **mediaitem** einem [**Media Player**](/uwp/api/windows.media.playback.mediaplayer) für die Wiedergabe in einem [**mediaplayerelement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)zugewiesen.

[!code-cs[ID3CuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CuePlay)]


In der **RegisterMetadataHandlerForID3Cues** -Hilfsmethode erhalten Sie eine Instanz der [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Klasse, indem Sie Sie in die **TimedMetadataTracks** -Auflistung von **mediaplaybackitem**indizieren. Überprüfen Sie die dispatchType-Eigenschaft des metadatentitels, die einen Wert enthält, der die GUID-Zeichenfolge "15260dffff49443320ff49443320000f" enthält, wenn der Track ID3-Tags darstellt. Registrieren Sie sich für das Ereignis [**cueeingetragen**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und das Ereignis [**cueexited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Anschließend müssen Sie [**setpresentationmode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) für die **TimedMetadataTracks** -Auflistung des Wiedergabe Elements aufrufen, um das System anzuweisen, dass die APP metadatenandereignisse für dieses Wiedergabe Element empfangen möchte.

[!code-cs[RegisterMetadataHandlerForID3Cues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForID3Cues)]

Wandeln Sie im Handler für das **cuecast** -Ereignis den in **der Eigenschaft "** Hinweis" von [**mediacueeventargs**](/uwp/api/windows.media.core.mediacueeventargs) enthaltenen Daten Hinweis in einen [**datacue**](/uwp/api/windows.media.core.datacue)um.  Stellen Sie sicher, dass der **datacue** und die [**Data**](/uwp/api/windows.media.core.datacue.Data) -Eigenschaft des-Hinweis nicht NULL sind. Erweiterte Emu-Kommentare werden in der Form Rohdaten Bytes im Transportstream bereitgestellt (siehe [http://id3.org/id3v2.4.0-structure](https://id3.org/id3v2.4.0-structure) ). Erstellen Sie einen neuen **DataReader** , um die Daten zu lesen, indem Sie [**DataReader. frombuffer**](/uwp/api/windows.storage.streams.datareader.FromBuffer)aufrufen.  In diesem Beispiel werden die Header Werte aus dem ID3-Tag aus den Hinweis Daten gelesen und in die Debugausgabe geschrieben.

[!code-cs[ID3CueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CueEntered)]

## <a name="fragmented-mp4-emsg-boxes"></a>Fragmentierte MP4-emsg-Felder
Ab Windows 10, Version 1703, können sich UWP-Apps für Cues registrieren, die emsg-Feldern in fragmentierten MP4-Streams entsprechen. Ein Beispiel für die Verwendung dieser metadatenart besteht darin, dass Inhaltsanbieter Client Anwendungen signalisieren, während Live Streaming-Inhalt eine Werbeeinblendungen durchführen. Dieses Beispiel verwendet [**adaptivemediasource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) , um den Medieninhalt wiederzugeben. Weitere Informationen finden Sie unter [Adaptive Streaming](adaptive-streaming.md). Erstellen Sie eine **adaptivemediasource** für den Inhalt, indem Sie [**createfromuriasync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) oder [**createfromstreamasync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)aufrufen. Erstellen Sie ein  [**MediaSource**](/uwp/api/windows.media.core.mediasource) -Objekt durch Aufrufen von [**createfromadaptivemediasource**](/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) , und erstellen Sie dann ein [**mediaplaybackitem**](/uwp/api/windows.media.playback.mediaplaybackitem) aus der **MediaSource**.

[!code-cs[EmsgLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgLoadContent)]

Registrieren Sie sich für die emsg Box-Ereignisse mithilfe des **mediaplaybackitem** -Objekts, das im vorherigen Schritt erstellt wurde. In diesem Beispiel wird eine Hilfsmethode, **RegisterMetadataHandlerForEmsgCues**, verwendet, um sich für die-Ereignisse zu registrieren. Ein Lambda-Ausdruck wird zum Implementieren eines Handlers für das [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) -Ereignis verwendet, das auftritt, wenn das System eine Änderung in den mit einem **mediaplaybackitem**verknüpften metadatenspuren erkennt. In einigen Fällen sind die metadatenspuren möglicherweise verfügbar, wenn das Wiedergabe Element anfänglich aufgelöst wird. Daher werden außerhalb des **TimedMetadataTracksChanged** -Handlers auch die verfügbaren metadatenspuren durchlaufen und **RegisterMetadataHandlerForEmsgCues**aufgerufen.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Nach der Registrierung für die emsg Box-Metadatenereignisse wird das **mediaitem** einem [**Media Player**](/uwp/api/windows.media.playback.mediaplayer) für die Wiedergabe in einem [**mediaplayerelement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)zugewiesen.

[!code-cs[EmsgCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCuePlay)]


In der **RegisterMetadataHandlerForEmsgCues** -Hilfsmethode erhalten Sie eine Instanz der [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) -Klasse, indem Sie Sie in die **TimedMetadataTracks** -Auflistung von **mediaplaybackitem**indizieren. Überprüfen Sie die dispatchType-Eigenschaft des metadatentitels, die den Wert "emsg: MP4" hat, wenn die Nachverfolgung emsg-Felder darstellt. Registrieren Sie sich für das Ereignis [**cueeingetragen**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) und das Ereignis [**cueexited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Anschließend müssen Sie [**setpresentationmode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) für die **TimedMetadataTracks** -Auflistung des Wiedergabe Elements aufrufen, um das System anzuweisen, dass die APP metadatenandereignisse für dieses Wiedergabe Element empfangen möchte.


[!code-cs[RegisterMetadataHandlerForEmsgCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEmsgCues)]


Wandeln Sie im Handler für das **cuecast** -Ereignis den in **der Eigenschaft "** Hinweis" von [**mediacueeventargs**](/uwp/api/windows.media.core.mediacueeventargs) enthaltenen Daten Hinweis in einen [**datacue**](/uwp/api/windows.media.core.datacue)um.  Stellen Sie sicher, dass das **datacue** -Objekt nicht NULL ist. Die Eigenschaften der emsg-Box werden von der Medien Pipeline als benutzerdefinierte Eigenschaften in der [**Properties**](/uwp/api/windows.media.core.datacue.Properties) -Auflistung des datacue-Objekts bereitgestellt. In diesem Beispiel wird versucht, mithilfe der **[TryGetValue](/uwp/api/windows.foundation.collections.propertyset.trygetvalue)** -Methode mehrere verschiedene Eigenschaftswerte zu extrahieren. Wenn diese Methode NULL zurückgibt, bedeutet dies, dass die angeforderte Eigenschaften nicht im Feld emsg vorhanden ist, daher wird stattdessen ein Standardwert festgelegt.

Der nächste Teil des Beispiels veranschaulicht das Szenario, in dem die AD-Wiedergabe ausgelöst wird. Dies ist der Fall, wenn die *scheme_id_uri* -Eigenschaft, die im vorherigen Schritt abgerufen wurde, den Wert "urn: SCTE: scte35:2013: XML" hat (siehe [http://dashif.org/identifiers/event-schemes/](https://dashif.org/identifiers/event-schemes/) ). Beachten Sie, dass der Standard empfiehlt, diese emsg mehrmals für Redundanz zu senden, sodass in diesem Beispiel eine Liste der bereits verarbeiteten emsg-IDs verwaltet wird und nur neue Nachrichten verarbeitet werden. Erstellen Sie einen neuen **DataReader** , um die Hinweis Daten zu lesen, indem Sie [**DataReader. frombuffer**](/uwp/api/windows.storage.streams.datareader.FromBuffer) aufrufen und die Codierung auf UTF-8 festlegen. Legen Sie hierzu die [**UnicodeEncoding**](/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) -Eigenschaft fest, und lesen Sie dann die Daten. In diesem Beispiel wird die Nachrichten Nutzlast in die Debugausgabe geschrieben. Eine echte App verwendet die Nutzlastdaten, um die Wiedergabe einer Werbe einelle zu planen.

[!code-cs[EmsgCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCueEntered)]


## <a name="related-topics"></a>Zugehörige Themen

* [Medienwiedergabe](media-playback.md)
* [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md)


 