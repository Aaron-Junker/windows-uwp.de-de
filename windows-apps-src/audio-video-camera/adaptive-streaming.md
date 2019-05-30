---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: In diesem Artikel wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming einer App für die Universelle Windows-Plattform (UWP) hinzufügen können. Dieses Feature unterstützt derzeit die Wiedergabe von Inhalten über HTTP Live Streaming (HLS) und Dynamic Streaming over HTTP (DASH).
title: Adaptives Streaming
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9ee2f8fd670da6bf843962e6b4dcac7a4b0c516d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359135"
---
# <a name="adaptive-streaming"></a>Adaptives Streaming


In diesem Artikel wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming einer App für die Universelle Windows-Plattform (UWP) hinzufügen können. Dieses Feature unterstützt die Wiedergabe von Inhalten über HTTP Live Streaming (HLS) und Dynamic Streaming over HTTP (DASH). Ab Windows 10, Version 1803, wird Smooth Streaming von **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** unterstützt.

Eine Liste mit unterstützten HLS-Protokolltags finden Sie unter [Unterstützung von HLS-Tags](hls-tag-support.md). 

Eine Liste der unterstützten DASH-Profile finden Sie unter [DASH-Profilunterstützung](dash-profile-support.md). 

> [!NOTE] 
> Der Code in diesem Artikel wurde aus dem [Beispiel für adaptives Streaming](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) für UWP übernommen und angepasst.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>Einfaches adaptives Streaming mit MediaPlayer und MediaPlayerElement

Erstellen Sie ein **Uri**-Objekt, das auf eine DASH- oder HLS-Manifestdatei verweist, um Medien für adaptives Streaming in einer UWP-App wiederzugeben. Erstellen Sie eine Instanz der [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse. Rufen Sie [**MediaSource.CreateFromUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromuri) auf, um ein neues **MediaSource**-Objekt zu erstellen, und legen Sie es dann auf die [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source)-Eigenschaft von **MediaPlayer** fest. Rufen Sie [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.play) auf, um die Wiedergabe des Medieninhalts zu starten.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

Im obigen Beispiel werden die Audiodaten des Medieninhalts wiedergegeben, aber der Inhalt wird nicht automatisch auf Ihrer Benutzeroberfläche gerendert. Für die meisten Apps, mit denen Videoinhalte wiedergegeben werden, wird der Inhalt auf einer XAML-Seite gerendert.  Fügen Sie der XAML-Seite hierzu ein [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelement hinzu.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Rufen Sie [**MediaSource.CreateFromUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromuri) auf, um ein **MediaSource**-Element über den URI einer DASH- oder HLS-Manifestdatei zu erstellen. Legen Sie anschließend die [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.sourceproperty)-Eigenschaft von **MediaPlayerElement** fest. **MediaPlayerElement** erstellt automatisch eine neues **MediaPlayer**-Objekt für den Inhalt. Sie können **Play** für das **MediaPlayer**-Objekt aufrufen, um die Wiedergabe des Inhalts zu starten.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> Ab Windows 10, Version 1607, wird die Verwendung der **MediaPlayer**-Klasse zum Wiedergeben von Medienelementen empfohlen. **MediaPlayerElement** ist ein einfaches XAML-Steuerelement, das zum Rendern des Inhalts eines **MediaPlayer**-Objekts auf einer XAML-Seite verwendet wird. Das **MediaElement**-Steuerelement wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. Weitere Informationen zur Verwendung von **MediaPlayer** und **MediaPlayerElement** zum Wiedergeben von Medieninhalten finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md). Informationen zur Verwendung von **MediaSource** und dazugehörigen APIs für die Arbeit mit Medieninhalten finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

## <a name="adaptive-streaming-with-adaptivemediasource"></a>Adaptives Streaming mit AdaptiveMediaSource

Verwenden Sie das **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** -Objekt, wenn Ihre App erweiterte adaptive Streamingfunktionen erfordert, z. B. das Bereitstellen von benutzerdefinierten HTTP-Headern, die Überwachung der aktuellen Download- und Wiedergabe-Bitraten oder das Anpassen der Verhältnisse, die bestimmen, wann das System Bitraten des adaptiven Datenstroms wechselt.

Die adaptiven Streaming-APIs sind im [**Windows.Media.Streaming.Adaptive**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive)-Namespace zu finden. Die Beispiele in diesem Artikel verwenden APIs aus den folgenden Namespaces.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>Initialisieren eines AdaptiveMediaSource-Objekts mit einem URI

Initialisieren Sie **AdaptiveMediaSource** mit dem URI einer adaptiven Streaming-Manifestdatei durch Aufrufen von [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync). Der von dieser Methode zurückgegebene [**AdaptiveMediaSourceCreationStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceCreationStatus)-Wert teilt Ihnen mit, ob die Medienquelle erfolgreich erstellt wurde. In diesem Fall können Sie das Objekt als Datenstromquelle für Ihren **MediaPlayer** durch Erstellen eines **MediaSource**-Objekts festlegen, indem Sie [**MediaSource.CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource) aufrufen und dann der [**Quelle**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source)-Eigenschaft des Media-Players zuweisen. In diesem Beispiel wird die [**AvailableBitrates**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.availablebitrates)-Eigenschaft abgefragt, um die maximal unterstützte Bitrate für diesen Datenstrom zu ermitteln. Anschließend wird dieser Wert als ursprüngliche Bitrate festgelegt. In diesem Beispiel werden auch Handler für mehrere **AdaptiveMediaSource**-Ereignisse registriert, die weiter unten in diesem Artikel erläutert werden.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>Initialisieren eines AdaptiveMediaSource-Objekts mit HttpClient

Wenn Sie benutzerdefinierte HTTP-Header für das Abrufen der Manifestdatei festlegen müssen, können Sie ein [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Objekt erstellen, die gewünschten Header festlegen und das Objekt an die Überladung von **CreateFromUriAsync** übergeben.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

Das [**DownloadRequested**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.downloadrequested)-Ereignis wird ausgelöst, wenn das System eine Ressource vom Server abruft. Die an den Ereignishandler übergebene [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadRequestedEventArgs) macht Eigenschaften verfügbar, die Informationen über die angeforderte Ressource wie Typ und URI der Ressource bereitstellen.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>Ändern der Eigenschaften der Ressourcenanforderung mit dem DownloadRequested-Ereignis

Sie können mit dem **DownloadRequested**-Ereignishandler die Ressourcenanforderung durch Aktualisieren der Eigenschaften des von den Ereignisargumenten bereitgestellten [**AdaptiveMediaSourceDownloadResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadResult)-Objekts ändern. Im folgenden Beispiel wird der URI, aus dem die Ressource abgerufen wird, durch die Aktualisierung der [**ResourceUri**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.resourceuri)-Eigenschaften des Ergebnisobjekts geändert. Sie können auch den Bytebereichsoffset und die Länge für Mediensegmente umschreiben oder, wie im folgenden Beispiel gezeigt, den Ressourcen-URI ändern, um die vollständige Ressource herunterzuladen und das Bytebereichsoffset und die Länge auf null festzulegen.

Sie können den Inhalt der angeforderten Ressource durch Festlegen der [**Buffer**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.buffer)- oder [**InputStream**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.inputstream)-Eigenschaften des Ergebnisobjekts überschreiben. Im folgenden Beispiel wird der Inhalt der Manifestressource durch Festlegen der **Buffer**-Eigenschaft ersetzt. Wenn Sie die Ressourcenanforderung mit Daten aktualisieren, die asynchron abgerufen werden, z. B. durch Abrufen von Daten von einem Remoteserver oder asynchrone Benutzerauthentifizierung, müssen Sie [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequestedeventargs.getdeferral) aufrufen, um eine Verzögerung abzurufen. Anschließend bei Abschluss des Vorgangs rufen Sie dann [**Complete**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequesteddeferral.complete) auf, um dem System zu signalisieren, dass der Downloadanforderungsvorgang fortgesetzt werden kann.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>Verwenden von Bitratenereignissen zum Verwalten von und Reagieren auf Bitratenänderungen

Das **AdaptiveMediaSource**-Objekt stellt Ereignisse bereit, mit denen Sie reagieren können, wenn sich die Download- oder Wiedergabe-Bitraten ändern. In diesem Beispiel werden die aktuellen Bitraten einfach auf der Benutzeroberfläche aktualisiert. Sie können die Verhältnisse ändern, die bestimmen, wann das System Bitraten des adaptiven Datenstroms wechselt. Weitere Informationen finden Sie unter der [**AdvancedSettings**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.advancedsettings)-Eigenschaft.

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>Behandeln von Downloadabschluss und Fehlerereignissen
Das **AdaptiveMediaSource**-Objekt löst das [**DownloadFailed**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed)-Ereignis aus, wenn der Download einer angeforderten Ressource fehlschlägt. Sie können mit diesem Ereignis die UI als Reaktion auf den Fehler aktualisieren. Sie können das Ereignis auch verwenden, um statistische Informationen über den Downloadvorgang und den Fehler zu protokollieren. 

Das an den Ereignishandler übergebene [**AdaptiveMediaSourceDownloadFailedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs)-Objekt enthält Metadaten über den Fehler beim Herunterladen der Ressource, z. B. den Ressourcentyp, den URI der Ressource und die Position im Stream, an der der Fehler aufgetreten ist. Die [**RequestId**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId)-Eigenschaft erhält einen vom System generierter eindeutigen Bezeichner für die Anforderung, der zum Korrelieren von Statusinformationen über eine einzelne Anforderung für mehrere Ereignisse verwendet werden kann.

Die [**Statistics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics)-Eigenschaft gibt ein [**AdaptiveMediaSourceDownloadStatistics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics)-Objekt zurück, das ausführliche Informationen über die Anzahl der zum Zeitpunkt des Ereignisses empfangenen Bytes und über den zeitlichen Ablauf der verschiedenen Meilensteine im Downloadvorgang enthält. Sie können diese Informationen protokollieren, um Leistungsprobleme in Ihrer Implementierung des adaptiven Streaming zu identifizieren.

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


Das  [**DownloadCompleted**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted)-Ereignis tritt auf, wenn ein Ressourcendownload abgeschlossen ist und für das **DownloadFailed**-Ereignis ähnliche Daten bereitstellt. Es wird wieder eine **RequestId** bereitgestellt, um Ereignisse für eine einzige Anforderung zu korrelieren. Darüber hinaus wird ein **AdaptiveMediaSourceDownloadStatistics**-Objekt bereitgestellt, um das Protokollieren der Downloadstatistiken zu aktivieren.

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>Erfassen von adaptiven Streaming-Telemetriedaten mit AdaptiveMediaSourceDiagnostics
Das **AdaptiveMediaSource**-Objekt stell die [**Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)-Eigenschaft bereit, die ein [**AdaptiveMediaSourceDiagnostics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics)-Objekt zurückgibt. Verwenden Sie dieses Objekt, um eine Registrierung für das [**DiagnosticAvailable**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable)-Ereignis auszuführen. Dieses Ereignis ist für die Telemetriesammlung vorgesehen und sollte nicht verwendet werden, um das Verhalten der App zur Laufzeit zu ändern. Dieses Diagnoseereignis wird aus vielen verschiedenen Gründen ausgelöst. Überprüfen Sie die [**DiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType)-Eigenschaft des an das Ereignis übergebenen [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs)-Objekts, um zu ermitteln, ob das Ereignis ausgelöst wurde. Zu den möglichen Gründen zählen Fehler beim Zugriff auf die angeforderte Ressource und Fehler beim Analysieren der Streaming-Manifestdatei. Eine Liste der Situationen, in denen ein Diagnoseereignis ausgelöst werden kann, finden Sie unter [**AdaptiveMediaSourceDiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype). Wie die Argumente für andere adaptive Streaming-Ereignisse stellt auch **AdaptiveMediaSourceDiagnosticAvailableEventArgs** eine **RequestId**-Eigenschaft zum Korrelieren von Anforderungsinformationen zwischen verschiedenen Ereignissen bereit.

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Verzögern der Bindung adaptiver Streaming-Inhalte für Elemente in einer Wiedergabeliste mit MediaBinder
Die [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder)-Klasse ermöglicht das Verzögern der Bindung von Medieninhalten in einer [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList). Ab Windows 10, Version 1703, können Sie eine [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) als gebundenen Inhalt angeben. Der Prozess für die verzögerte Bindung einer adaptiven Medienquelle entspricht größtenteils dem Binden anderer Medientypen, was in [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md) beschrieben ist. 

Erstellen Sie eine **MediaBinder**-Instanz, legen Sie eine App-definierte [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token)-Zeichenfolge fest, um den zu bindenden Inhalt zu identifizieren, und führen Sie eine Registrierung für das [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding)-Ereignis durch. Erstellen Sie eine **MediaSource** aus dem **Binder**, indem Sie [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder) aufrufen. Erstellen Sie anschließend ein **MediaPlaybackItem** aus der **MediaSource**, und fügen Sie es der Wiedergabeliste hinzu.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

Verwenden Sie im **Bindung**-Ereignishandler die Tokenzeichenfolge zum Identifizieren des zu bindenden Inhalts, und erstellen Sie dann die adaptive Medienquelle, indem Sie eine der Überladungen von **[CreateFromStreamAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** oder **[CreateFromUriAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)** aufrufen. Da es sich dabei um asynchrone Methoden handelt, sollten Sie zuerst die [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral)-Methode aufrufen, um das System anzuweisen, auf den Abschluss des Vorgangs zu warten.  Legen Sie die adaptive Medienquelle als gebundenen Inhalt fest, indem Sie **[SetAdaptiveMediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)** aufrufen. Rufen Sie nach Abschluss des Vorgangs [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) auf, um dem System mitzuteilen, dass es fortsetzen kann.

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

Wenn Sie Ereignishandler für die gebundene adaptive Medienquelle registrieren möchten, können Sie dies im Handler für das [**CurrentItemChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged)-Ereignis der **MediaPlaybackList** durchführen. Die [**CurrentMediaPlaybackItemChangedEventArgs.NewItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem)-Eigenschaft enthält das neue, aktuell wiedergegebene **MediaPlaybackItem** in der Liste. Rufen Sie eine Instanz der **AdaptiveMediaSource** ab, die das neue Element darstellt, indem Sie auf die [**Source**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source)-Eigenschaft des **MediaPlaybackItem** und dann auf die [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) Eigenschaft der Medienquelle zugreifen. Diese Eigenschaft ist null, wenn das neue Wiedergabeelement kein **AdaptiveMediaSource** ist. Sie sollten daher auf null testen, bevor Sie versuchen, Handler für die Ereignisse des Objekts zu registrieren.

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Unterstützung für HLS-Tags](hls-tag-support.md) 
* [Unterstützung für Dash-profile](dash-profile-support.md) 
* [Abspielen von Audio- und Videodateien mit MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md) 





