---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: In diesem Artikel wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming einer App für die Universelle Windows-Plattform (UWP) hinzufügen können. Dieses Feature unterstützt derzeit die Wiedergabe von Inhalten über HTTP Live Streaming (HLS) und Dynamic Streaming over HTTP (DASH).
title: Adaptives Streaming
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0ecfe0b8e48a0810614cad6fde91d9a429956bf9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161234"
---
# <a name="adaptive-streaming"></a>Adaptives Streaming


In diesem Artikel wird beschrieben, wie Sie die Wiedergabe von Multimediainhalten für adaptives Streaming einer App für die Universelle Windows-Plattform (UWP) hinzufügen können. Diese Funktion unterstützt die Wiedergabe von http Live Streaming (HLS) und Dynamic Streaming over HTTP (Dash)-Inhalt. Ab Windows 10, Version 1803, wird Smooth Streaming von  **[adaptivemediasource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** unterstützt.

Eine Liste mit unterstützten HLS-Protokolltags finden Sie unter [Unterstützung von HLS-Tags](hls-tag-support.md). 

Eine Liste der unterstützten Dash-Profile finden Sie [unter Dash-Profil Unterstützung](dash-profile-support.md). 

> [!NOTE] 
> Der Code in diesem Artikel wurde aus dem [Beispiel für adaptives Streaming](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) für UWP übernommen und angepasst.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>Einfaches adaptives Streaming mit MediaPlayer und MediaPlayerElement

Erstellen Sie ein **Uri**-Objekt, das auf eine DASH- oder HLS-Manifestdatei verweist, um Medien für adaptives Streaming in einer UWP-App wiederzugeben. Erstellen Sie eine Instanz der [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse. Rufen Sie [**MediaSource.CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) auf, um ein neues **MediaSource**-Objekt zu erstellen, und legen Sie es dann auf die [**Source**](/uwp/api/windows.media.playback.mediaplayer.source)-Eigenschaft von **MediaPlayer** fest. Rufen Sie [**Play**](/uwp/api/windows.media.playback.mediaplayer.play) auf, um die Wiedergabe des Medieninhalts zu starten.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

Im obigen Beispiel werden die Audiodaten des Medieninhalts wiedergegeben, aber der Inhalt wird nicht automatisch auf Ihrer Benutzeroberfläche gerendert. Für die meisten Apps, mit denen Videoinhalte wiedergegeben werden, wird der Inhalt auf einer XAML-Seite gerendert.  Fügen Sie der XAML-Seite hierzu ein [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelement hinzu.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Rufen Sie [**MediaSource.CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) auf, um ein **MediaSource**-Element über den URI einer DASH- oder HLS-Manifestdatei zu erstellen. Legen Sie anschließend die [**Source**](/uwp/api/windows.ui.xaml.controls.mediaelement.sourceproperty)-Eigenschaft von **MediaPlayerElement** fest. **MediaPlayerElement** erstellt automatisch eine neues **MediaPlayer**-Objekt für den Inhalt. Sie können **Play** für das **MediaPlayer**-Objekt aufrufen, um die Wiedergabe des Inhalts zu starten.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> Ab Windows 10, Version 1607, wird die Verwendung der **MediaPlayer**-Klasse zum Wiedergeben von Medienelementen empfohlen. **MediaPlayerElement** ist ein einfaches XAML-Steuerelement, das zum Rendern des Inhalts eines **MediaPlayer**-Objekts auf einer XAML-Seite verwendet wird. Das **MediaElement**-Steuerelement wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. Weitere Informationen zur Verwendung von **MediaPlayer** und **MediaPlayerElement** zum Wiedergeben von Medieninhalten finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md). Informationen zur Verwendung von **MediaSource** und dazugehörigen APIs für die Arbeit mit Medieninhalten finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

## <a name="adaptive-streaming-with-adaptivemediasource"></a>Adaptives Streaming mit AdaptiveMediaSource

Verwenden Sie das **[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)**-Objekt, wenn Ihre App erweiterte adaptive Streamingfunktionen erfordert, z. B. das Bereitstellen von benutzerdefinierten HTTP-Headern, die Überwachung der aktuellen Download- und Wiedergabe-Bitraten oder das Anpassen der Verhältnisse, die bestimmen, wann das System Bitraten des adaptiven Datenstroms wechselt.

Die adaptiven Streaming-APIs sind im [**Windows.Media.Streaming.Adaptive**](/uwp/api/Windows.Media.Streaming.Adaptive)-Namespace zu finden. In den Beispielen in diesem Artikel werden APIs aus den folgenden Namespaces verwendet.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>Initialisieren Sie eine adaptivemediasource aus einem URI.

Initialisieren Sie **AdaptiveMediaSource** mit dem URI einer adaptiven Streaming-Manifestdatei durch Aufrufen von [**CreateFromUriAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync). Der von dieser Methode zurückgegebene [**AdaptiveMediaSourceCreationStatus**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceCreationStatus)-Wert teilt Ihnen mit, ob die Medienquelle erfolgreich erstellt wurde. Wenn dies der Fall ist, können Sie das Objekt als Streamquelle für **Media Player** festlegen, indem Sie ein **MediaSource** -Objekt durch Aufrufen von  [**MediaSource. createfromadaptivemediasource**](/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource)erstellen und es dann der [**Source**](/uwp/api/windows.media.playback.mediaplayer.Source) -Eigenschaft von Media Player zuweisen. In diesem Beispiel wird die [**AvailableBitrates**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.availablebitrates)-Eigenschaft abgefragt, um die maximal unterstützte Bitrate für diesen Datenstrom zu ermitteln. Anschließend wird dieser Wert als ursprüngliche Bitrate festgelegt. In diesem Beispiel werden auch Handler für die verschiedenen **adaptivemediasource** -Ereignisse registriert, die später in diesem Artikel erläutert werden.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>Initialisieren einer adaptivemediasource mithilfe von httpclient

Wenn Sie benutzerdefinierte HTTP-Header für das Abrufen der Manifestdatei festlegen müssen, können Sie ein [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)-Objekt erstellen, die gewünschten Header festlegen und das Objekt an die Überladung von **CreateFromUriAsync** übergeben.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

Das [**DownloadRequested**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.downloadrequested)-Ereignis wird ausgelöst, wenn das System eine Ressource vom Server abruft. Die an den Ereignishandler übergebene [**AdaptiveMediaSourceDownloadRequestedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadRequestedEventArgs) macht Eigenschaften verfügbar, die Informationen über die angeforderte Ressource wie Typ und URI der Ressource bereitstellen.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>Ändern der Eigenschaften von Ressourcenanforderungen mithilfe des downloadangeforderten Ereignisses

Sie können mit dem **DownloadRequested**-Ereignishandler die Ressourcenanforderung durch Aktualisieren der Eigenschaften des von den Ereignisargumenten bereitgestellten [**AdaptiveMediaSourceDownloadResult**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadResult)-Objekts ändern. Im folgenden Beispiel wird der URI, aus dem die Ressource abgerufen wird, durch die Aktualisierung der [**ResourceUri**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.resourceuri)-Eigenschaften des Ergebnisobjekts geändert. Sie können auch den Byte Bereichs Offset und die Länge für Medien Segmente neu schreiben oder, wie im folgenden Beispiel gezeigt, den Ressourcen-URI ändern, um die vollständige Ressource herunterzuladen und den Byte Bereichs Offset und die Länge auf NULL festzulegen.

Sie können den Inhalt der angeforderten Ressource durch Festlegen der [**Buffer**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.buffer)- oder [**InputStream**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.inputstream)-Eigenschaften des Ergebnisobjekts überschreiben. Im folgenden Beispiel wird der Inhalt der Manifestressource durch Festlegen der **Buffer**-Eigenschaft ersetzt. Wenn Sie die Ressourcenanforderung mit Daten aktualisieren, die asynchron abgerufen werden, z. B. durch Abrufen von Daten von einem Remoteserver oder asynchrone Benutzerauthentifizierung, müssen Sie [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequestedeventargs.getdeferral) aufrufen, um eine Verzögerung abzurufen. Anschließend bei Abschluss des Vorgangs rufen Sie dann [**Complete**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequesteddeferral.complete) auf, um dem System zu signalisieren, dass der Downloadanforderungsvorgang fortgesetzt werden kann.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>Verwenden von Bitrate-Ereignissen zum Verwalten und reagieren auf Bitrate-Änderungen

Das **AdaptiveMediaSource**-Objekt stellt Ereignisse bereit, mit denen Sie reagieren können, wenn sich die Download- oder Wiedergabe-Bitraten ändern. In diesem Beispiel werden die aktuellen Bitraten einfach auf der Benutzeroberfläche aktualisiert. Sie können die Verhältnisse ändern, die bestimmen, wann das System Bitraten des adaptiven Datenstroms wechselt. Weitere Informationen finden Sie unter der [**AdvancedSettings**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.advancedsettings)-Eigenschaft.

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>Behandeln von Download Vervollständigungs-und Fehlerereignissen
Das **adaptivemediasource** -Objekt löst das [**DownloadFailed**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) -Ereignis aus, wenn das Herunterladen einer angeforderten Ressource fehlschlägt. Sie können dieses Ereignis verwenden, um die Benutzeroberfläche als Reaktion auf den Fehler zu aktualisieren. Sie können auch das-Ereignis verwenden, um statistische Informationen über den Downloadvorgang und den Fehler zu protokollieren. 

Das an den Ereignishandler übergebenen [**adaptivemediasourcedownloadfailedeventargs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) -Objekt enthält Metadaten über den Fehler beim Herunterladen der Ressource, z. b. den Ressourcentyp, den URI der Ressource und die Position innerhalb des Streams, in der der Fehler aufgetreten ist. Die [**RequestId**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) erhält einen vom System generierten eindeutigen Bezeichner für die Anforderung, der zum Korrelieren von Statusinformationen zu einer einzelnen Anforderung über mehrere Ereignisse verwendet werden kann.

Die [**Statistics**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) -Eigenschaft gibt ein [**adaptivemediasourcedownloadstatistics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) -Objekt zurück, das ausführliche Informationen über die Anzahl der zum Zeitpunkt des Ereignisses empfangenen Bytes und die zeitliche Steuerung der verschiedenen Meilensteine im Downloadvorgang bereitstellt. Sie können diese Informationen protokollieren, um Leistungs-Probleme in ihrer adaptiven streamingimplementierung zu identifizieren.

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


Das  [**downloadabgeschlossene**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) -Ereignis tritt auf, wenn ein Ressourcen Download abgeschlossen ist, und es werden ähnliche Daten an das **DownloadFailed** -Ereignis ausgegeben. Erneut wird ein **RequestId-** Element für das korrelieren von Ereignissen für eine einzelne Anforderung bereitgestellt. Außerdem wird ein **adaptivemediasourcedownloadstatistics** -Objekt bereitgestellt, um die Protokollierung von Download Statistiken zu aktivieren.

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>Sammeln von adaptiven streamingtelemetriedaten mit adaptivemediasourcediagnostics
Die **adaptivemediasource macht** eine [**Diagnose**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource) Eigenschaft verfügbar, die ein [**adaptivemediasourcediagnostics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics) -Objekt zurückgibt. Verwenden Sie dieses Objekt, um sich für das [**diagnosticavailable**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable) -Ereignis zu registrieren. Dieses Ereignis ist für die Verwendung bei der telemetrieerfassung vorgesehen und sollte nicht zum Ändern des App-Verhaltens zur Laufzeit verwendet werden. Dieses Diagnose Ereignis wird aus vielen verschiedenen Gründen ausgelöst. Überprüfen Sie die [**diagnostictype**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) -Eigenschaft des [**adaptivemediasourcediagnosticavailableeventargs**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) -Objekts, das an das-Ereignis übergeben wird, um zu bestimmen, warum das Ereignis ausgelöst wurde. Mögliche Ursachen sind Fehler beim Zugriff auf die angeforderte Ressource und Fehler beim Parsen der Streaming-Manifest-Datei. Eine Liste der Situationen, in denen ein Diagnose Ereignis auftreten kann, finden Sie unter [**adaptivemediasourcediagnostictype**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype). Wie die Argumente anderer adaptiver streamingereignisse bietet **adaptivemediasourcediagnosticavailableeventargs** eine **RequestId-** Eigenschaft für das korrelieren von Anforderungs Informationen zwischen verschiedenen Ereignissen.

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Zurückstellen der Bindung von adaptiver Streaminginhalt für Elemente in einer Wiedergabeliste mithilfe von "-"
Mit [**der Klasse**](/uwp/api/Windows.Media.Core.MediaBinder) "die Klasse" können Sie die Bindung von Medieninhalten in einer [**mediaplaybacklist**](/uwp/api/Windows.Media.Playback.MediaPlaybackList)verzögern. Ab Windows 10, Version 1703, können Sie eine [**adaptivemediasource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) als gebundenen Inhalt bereitstellen. Der Prozess für die verzögerte Bindung einer adaptiven Medienquelle ist größtenteils mit der Bindung anderer Medientypen identisch, die unter [Medienelemente, Wiedergabelisten und Spuren](media-playback-with-mediasource.md)beschrieben werden. 

Erstellen Sie eine **-Instanz,** und legen Sie eine APP-definierte Tokenzeichenfolge fest, um den zu gebundenen Inhalt zu identifizieren, und registrieren Sie sich für das [**Token**](/uwp/api/Windows.Media.Core.MediaBinder.Token) [**Bindungs**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) Ereignis. Erstellen Sie eine **MediaSource** aus dem **Binder** , indem Sie [**MediaSource. createfrommediabinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder)aufrufen. Erstellen Sie dann ein **mediaplaybackitem-Element** aus der **MediaSource** , und fügen Sie es der Wiedergabeliste hinzu.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

Verwenden Sie im **Bindungs** Ereignishandler die Tokenzeichenfolge, um den Inhalt zu identifizieren, der gebunden werden soll, und erstellen Sie dann die Adaptive Medienquelle, indem Sie eine der über Ladungen von **[createfromstreamasync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** oder **[createfromuriasync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)** aufrufen. Da es sich hierbei um asynchrone Methoden handelt, sollten Sie zuerst die Methode "-Methode [**. getdeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) " aufrufen, um das System anzuweisen, auf den Abschluss des Vorgangs zu warten, bevor Sie fortfahren.  Legen Sie die Adaptive Medienquelle als gebundenen Inhalt fest, indem Sie **[setadaptivemediasource](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)** aufrufen. Zum Schluss wird " [**Deferral. Complete**](/uwp/api/windows.foundation.deferral.Complete) " aufgerufen, nachdem der Vorgang beendet wurde, um das System anzuweisen, fortzufahren.

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

Wenn Sie Ereignishandler für die gebundene Adaptive Medienquelle registrieren möchten, können Sie dies im Handler für [**das Ereignis "**](/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) " in der Datei " **mediaplaybacklist**" durchführen. Die [**currentmediaplaybackitemchangedebug-args. netwitem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) -Eigenschaft enthält die neue, derzeit in der Liste wiedergegebene **mediaplaybackitem** . Sie erhalten eine Instanz von **adaptivemediasource** , die das neue Element darstellt, indem Sie auf die [**Source**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) -Eigenschaft von **mediaplaybackitem** und dann auf die [**adaptivemediasource**](/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) -Eigenschaft der Medienquelle zugreifen. Diese Eigenschaft ist NULL, wenn es sich bei dem neuen Wiedergabe Element nicht um eine **adaptivemediasource**handelt. Daher sollten Sie auf NULL testen, bevor Sie versuchen, Handler für die Ereignisse eines beliebigen Objekts zu registrieren.

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>Zugehörige Themen
* [Medienwiedergabe](media-playback.md)
* [Unterstützung von HLS-Tags](hls-tag-support.md) 
* [Unterstützung für Strich profile](dash-profile-support.md) 
* [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md) 