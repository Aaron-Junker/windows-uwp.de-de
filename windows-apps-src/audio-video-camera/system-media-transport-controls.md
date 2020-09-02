---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: Mit der SystemMediaTransportControls-Klasse kann Ihre App die Steuerelemente für den Systemmedientransport verwenden, die in Windows integriert sind, und die Metadaten aktualisieren, die die Steuerelemente zu den von der App aktuell wiedergegebenen Medien anzeigen.
title: Manuelle Steuerung der Steuerelemente für den Systemmedientransport
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d592b516db32c2602c8b51d82f3ea56c037e5164
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363783"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>Manuelle Steuerung der Steuerelemente für den Systemmedientransport


Ab Windows 10, Version 1607, werden UWP-Apps, welche die [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse für die Medienwiedergabe verwenden, standardmäßig und automatisch in die Steuerelemente für den Systemmedientransport (System Media Transport Controls, SMTC) integriert. Dies ist für die meisten Szenarien die empfohlene Methode für die Interaktion mit den SMTC. Weitere Informationen zum Anpassen der standardmäßigen SMTC-Integration mit **MediaPlayer** finden Sie unter [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md).

In verschiedenen Szenarien müssen Sie eine manuelle Steuerung der SMTC implementieren. Hierzu gehört die Verwendung einer [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController)-Klasse zum Steuern der Wiedergabe eines oder mehrerer Medienplayer. Ein anderes Szenario ist die Verwendung mehrerer Medienplayer mit nur einer Instanz der SMTC für Ihre App. Bei der Medienwiedergabe mithilfe der [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement)-Klasse müssen Sie die SMTC manuell steuern.

## <a name="set-up-transport-controls"></a>Einrichten von Transportsteuerelementen
Wenn Sie für die Medienwiedergabe **MediaPlayer** verwenden, können Sie eine Instanz der [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls)-Klasse abrufen, indem Sie auf die [**MediaPlayer.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols)-Eigenschaft zugreifen. Wenn Sie die SMTC manuell steuern möchten, sollten Sie die automatische Integration von **MediaPlayer** deaktivieren, indem Sie die [**CommandManager.IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled)-Eigenschaft auf „false“ festlegen.

> [!NOTE] 
> Wenn Sie [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) für [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) deaktivieren, indem Sie [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) auf „false“ festlegen, wird die von **MediaPlayerElement** bereitgestellte Verknüpfung zwischen **MediaPlayer** und [**TransportControls**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) getrennt, sodass die integrierten Transportsteuerelemente nicht mehr automatisch die Wiedergabe des Players steuern. Stattdessen müssen Sie Ihre eigenen Steuerelemente zum Steuern des **MediaPlayers** implementieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetInitSMTCMediaPlayer":::

Sie können auch eine Instanz der [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls)-Klasse abrufen, indem Sie [**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview) aufrufen. Sie müssen das Objekt mit dieser Methode abrufen, wenn Sie für die Medienwiedergabe **MediaElement** verwenden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetInitSMTCMediaElement":::

Legen Sie zum Aktivieren der von Ihrer App verwendeten Schaltflächen die entsprechende „IsEnabled“-Eigenschaft des **SystemMediaTransportControls**-Objekts fest, z. B. [**IsPlayEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled), [**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled), [**IsNextEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isnextenabled) und [**IsPreviousEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispreviousenabled). Eine vollständige Liste der verfügbaren Steuerelemente finden Sie in der Referenzdokumentation zu **SystemMediaTransportControls**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetEnableContols":::

Registrieren Sie einen Handler für das [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed)-Ereignis, um Benachrichtigungen zu empfangen, wenn der Benutzer eine Schaltfläche betätigt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetRegisterButtonPressed":::

## <a name="handle-system-media-transport-controls-button-presses"></a>Behandeln von Klicks auf Schaltflächen von Systemmedientransport-Steuerelementen

Das [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed)-Ereignis wird von den Systemsteuerelementen für den Medientransport ausgelöst, wenn eine der aktivierten Schaltflächen betätigt wird. Die an den Ereignishandler übergebene [**Button**](/uwp/api/windows.media.systemmediatransportcontrolsbuttonpressedeventargs.button)-Eigenschaft der [**SystemMediaTransportControlsButtonPressedEventArgs**](/uwp/api/Windows.Media.SystemMediaTransportControlsButtonPressedEventArgs)-Klasse ist ein Element der [**SystemMediaTransportControlsButton**](/uwp/api/Windows.Media.SystemMediaTransportControlsButton)-Enumeration, die angibt, welche der aktivierten Schaltflächen betätigt wurde.

Zum Aktualisieren von Objekten im UI-Thread des [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed)-Ereignishandlers (z. B. ein [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement)-Objekt) müssen Sie die Aufrufe über den [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) marshallen. Der Grund hierfür ist, dass der **ButtonPressed**-Ereignishandler nicht vom Benutzeroberflächenthread aufgerufen wird und daher eine Ausnahme ausgelöst wird, wenn Sie versuchen, die Benutzeroberfläche direkt zu ändern.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsButtonPressed":::

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>Aktualisieren der Steuerelemente für den Systemmedientransport mit dem aktuellen Medienstatus

Melden Sie der [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls)-Klasse eine Änderung des Medienstatus, damit das System die Steuerelemente entsprechend dem aktuellen Zustand aktualisieren kann. Legen Sie dazu die [**PlaybackStatus**](/uwp/api/windows.media.systemmediatransportcontrols.playbackstatus)-Eigenschaft auf den entsprechenden [**MediaPlaybackStatus**](/uwp/api/Windows.Media.MediaPlaybackStatus)-Wert innerhalb des [**CurrentStateChanged**](/uwp/api/windows.ui.xaml.controls.mediaelement.currentstatechanged)-Ereignisses der [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement)-Klasse fest, die bei Änderungen des Medienstatus ausgelöst wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsStateChange":::

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>Aktualisieren der Steuerelemente für den Systemmedientransport mit Medieninformationen und Miniaturansichten

Verwenden Sie die [**SystemMediaTransportControlsDisplayUpdater**](/uwp/api/Windows.Media.SystemMediaTransportControlsDisplayUpdater)-Klasse, um die von den Transportsteuerelementen angezeigten Medieninfos (z. B. den Songtitel oder das Albumbild) für das aktuell wiedergegebene Medienelement zu aktualisieren. Rufen Sie eine Instanz dieser Klasse mit der [**SystemMediaTransportControls.DisplayUpdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater)-Eigenschaft ab. Für gewöhnliche Szenarien wird empfohlen, die Metadaten mit dem Aufruf der [**CopyFromFileAsync**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.copyfromfileasync)-Methode und in der aktuell wiedergegebenen Mediendatei zu übergeben. Die Anzeigeaktualisierung wird die Metadaten und das Miniaturbild automatisch aus der Datei extrahieren.

Rufen Sie die [**Update**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.update)-Methode auf, damit die Benutzeroberflächen der Steuerelemente für den Systemmedientransport mit den neuen Metadaten und der Miniaturansicht aktualisiert werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsUpdater":::

Wenn es Ihr Szenario erfordert, können Sie die von den Steuerelementen für den Systemmedientransport angezeigten Metadaten manuell aktualisieren, indem Sie die Werte der [**MusicProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.musicproperties)-, [**ImageProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.imageproperties)- oder [**VideoProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.videoproperties)-Objekte festlegen, die von der [**DisplayUpdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater)-Klasse zur Verfügung gestellt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SystemMediaTransportControlsUpdaterManual":::

> [!Note]
> Apps sollten einen Wert für die [systemmediatransportcontrolsdisplayupdater. Type](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.type#Windows_Media_SystemMediaTransportControlsDisplayUpdater_Type
) -Eigenschaft festlegen, auch wenn Sie keine anderen Medien Metadaten bereitstellen, die von den System Media-Transport Steuerelementen angezeigt werden. Dieser Wert hilft dem System dabei, Ihre Medieninhalte ordnungsgemäß zu verarbeiten, einschließlich der Aktivierung des Bildschirmschoners während der Wiedergabe.


## <a name="update-the-system-media-transport-controls-timeline-properties"></a>Aktualisieren der Zeitskalaeigenschaften der Steuerelemente für den Systemmedientransport

Die Steuerelemente für den Systemmedientransport zeigen Informationen über die Zeitskala des aktuell wiedergegebenen Medienelements an, z. B. die aktuelle Wiedergabeposition, die Startzeit und die Endzeit des Medienelements. Erstellen Sie zum Aktualisieren der Zeitskalaeigenschaften der Steuerelemente für den Systemmedientransport ein neues [**SystemMediaTransportControlsTimelineProperties**](/uwp/api/Windows.Media.SystemMediaTransportControlsTimelineProperties)-Objekt. Legen Sie die Eigenschaften des Objekts so fest, dass sich der aktuelle Zustand des wiedergegebenen Medienelements widerspiegelt. Rufen Sie die [**SystemMediaTransportControls.UpdateTimelineProperties**](/uwp/api/windows.media.systemmediatransportcontrols.updatetimelineproperties)-Methode auf, damit die Zeitskala durch die Steuerelemente aktualisiert wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetUpdateTimelineProperties":::

-   Sie müssen einen Wert für die [**StartTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.starttime)-, [**EndTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.endtime)- und [**Position**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested)-Eigenschaften angeben, um eine Zeitskala für das wiedergegebene Element anzuzeigen.

-   [Mit den **MinSeekTime**-						](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) und [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime)-Eigenschaften können Sie den Bereich innerhalb der Zeitskala angeben, den die Benutzer durchsuchen können. Ein typisches Szenario hierfür ist es, den Anbietern Werbepausen in ihren Medien zu ermöglichen.

    Sie müssen die [**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime)- und [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime)-Eigenschaften festlegen, um [**PositionChangeRequest**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) auszulösen.

-   Es wird empfohlen, die Steuerelemente mit der Medienwiedergabe zu synchronisieren, indem diese Eigenschaften etwa alle fünf Sekunden während der Wiedergabe und bei Änderung des Wiedergabestatus aktualisiert werden, z. B. beim Anhalten der Wiedergabe oder bei der Suche nach einer neuen Wiedergabeposition.

## <a name="respond-to-player-property-changes"></a>Reaktion auf Änderungen der Playereigenschaften

Es gibt eine Reihe von Steuerelementeigenschaften für den Systemmedientransport, die sich auf den Zustand des Media Players selbst, und nicht auf den Zustand des wiedergegebenen Medienelements beziehen. Jede dieser Eigenschaften ist einem Ereignis zugeordnet, das ausgelöst wird, wenn der Benutzer das zugeordnete Steuerelement anpasst. Diese Eigenschaften und Ereignisse sind folgende:

| Eigenschaft                                                                  | Ereignis                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmode) | [**AutoRepeatModeChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmodechangerequested) |
| [**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate)     | [**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested)     |
| [**ShuffleEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabled) | [**ShuffleEnabledChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabledchangerequested) |

 
Registrieren Sie zum Behandeln von Benutzerinteraktionen mit einem der folgenden Steuerelemente zunächst einen Handler für das zugeordnete Ereignis.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPlaybackChangedHandler":::

Stellen Sie im Handler für das Ereignis zunächst sicher, dass der angeforderte Wert innerhalb eines gültigen und erwarteten Bereichs liegt. Wenn dies der Fall ist, legen Sie die entsprechende Eigenschaft für die [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement)-Klasse fest und anschließend die entsprechende Eigenschaft für das [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls)-Objekt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetPlaybackChangedHandler":::

-   Sie müssen einen Anfangswert für die Eigenschaft festlegen, damit eines dieser Ereignisse der Playereigenschaft ausgelöst wird. [**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested) wird beispielsweise erst ausgelöst, wenn ein Wert für die [**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate)-Eigenschaft mindestens ein Mal festgelegt wurde.

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>Verwenden der Steuerelemente für den Systemmedientransport für Audiowiedergabe im Hintergrund

Wenn Sie die automatische SMTC-Integration von **MediaPlayer** nicht verwenden, müssen Sie die SMTC manuell integrieren, um Hintergrundaudio zu aktivieren. Sie sollten für Ihre App mindestens die Schaltflächen „Wiedergeben“ und „Anhalten“ aktivieren, indem Sie [**IsPlayEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled) und [**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled) auf „true“ festlegen. Außerdem muss die App das [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed)-Ereignis behandeln. Wenn Ihre App diese Anforderungen nicht erfüllt, wird die Audiowiedergabe bei Verschieben der App in den Hintergrund beendet.

Apps, in denen das neue Einzelprozessmodell für Hintergrundaudio verwendet wird, sollten eine Instanz der [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls)-Klasse abrufen, indem sie [**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview) aufrufen. Apps, die das vorherige Modell mit zwei Prozessen für die Audiowiedergabe im Hintergrund verwenden, müssen [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) verwenden, um über den Hintergrundprozess Zugriff auf die SMTC zu erhalten.

Weitere Informationen zur Audiowiedergabe im Hintergrund finden Sie unter [Wiedergeben von Medien im Hintergrund](background-audio.md).

## <a name="related-topics"></a>Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md) 
* [Beispiel für den Systemmedientransport](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 
