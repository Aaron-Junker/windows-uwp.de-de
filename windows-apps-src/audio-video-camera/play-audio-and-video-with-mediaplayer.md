---
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: In diesem Artikel erfahren Sie, wie Sie in Ihrer universellen Windows-App mithilfe von „MediaPlayer“ Medien wiedergeben.
title: Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d25cd3f8a963d85f704e32482475827462ddba58
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318264"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“

In diesem Artikel erfahren Sie, wie Sie in Ihrer universellen Windows-App mithilfe der [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse Medien wiedergeben. In Windows 10, Version 1607, wurden wesentliche Verbesserungen an den APIs zur Medienwiedergabe vorgenommen, unter anderem ein vereinfachtes Einzelprozessdesign für die Audiowiedergabe im Hintergrund, automatische Integration in die Steuerelemente für den Systemmedientransport (System Media Transport Controls, SMTC), die Möglichkeit zum Synchronisieren mehrerer Media Player, die Option einer Windows.UI.Composition-Oberfläche und eine anwenderfreundliche Schnittstelle zum Erstellen und Planen von Medienunterbrechungen in Ihren Inhalten. Um diese Verbesserungen optimal zu nutzen, sollten Sie für die Medienwiedergabe anstelle von **MediaElement** die **MediaPlayer**-Klasse verwenden. Das einfache XAML-Steuerelement [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) wurde eingeführt, damit Sie Medieninhalte auf einer XAML-Seite rendern können. Viele der von **MediaElement** bereitgestellten Wiedergabesteuerelemente und Status-APIs sind jetzt über das neue [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession)-Objekt verfügbar. **MediaElement** gewährleistet weiterhin die Abwärtskompatibilität. Dieser Klasse werden jedoch keine weiteren Features hinzugefügt.

Dieser Artikel erläutert die Features von **MediaPlayer**, die von einer typischen App zur Medienwiedergabe verwendet werden. Beachten Sie, dass **MediaPlayer** die [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource)-Klasse als Container für alle Medienelemente verwendet. Diese Klasse ermöglicht Ihnen das Laden und Wiedergeben von Medien aus verschiedenen Quellen – z. B. aus lokalen Dateien, Speicherdatenströmen und Netzwerkquellen – über die gleiche Schnittstelle. Verschiedene Klassen auf höherer Ebene arbeiten ebenfalls mit **MediaSource**, z. B. [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) und [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList). Sie stellen erweiterte Features bereit wie Wiedergabelisten und die Möglichkeit, Medienquellen mit mehreren Audio-, Video- und Metadatentiteln zu verwalten, Weitere Informationen zu **MediaSource** und verwandten APIs finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

> [!NOTE] 
> Windows 10 N- und Windows 10 KN-Editionen enthalten nicht die für die Verwendung von **MediaPlayer** für die Wiedergabe erforderlichen Medienfunktionen. Diese Features können manuell installiert werden. Weitere Informationen finden Sie unter [Media Feature Pack für Windows 10 N- und Windows 10 KN-Editionen](https://support.microsoft.com/en-us/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions).

## <a name="play-a-media-file-with-mediaplayer"></a>Wiedergeben einer Mediendatei mit „MediaPlayer“  
Die grundlegende Medienwiedergabe mit **MediaPlayer** ist sehr einfach zu implementieren. Erstellen Sie zunächst eine neue Instanz der **MediaPlayer**-Klasse. In Ihrer App können mehrere **MediaPlayer**-Instanzen gleichzeitig aktiv sein. Legen Sie dann für die [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source)-Eigenschaft des Players ein Objekt fest, welches die [**IMediaPlaybackSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.IMediaPlaybackSource) implementiert, z. B. eine [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource), ein [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) oder eine [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList). In diesem Beispiel wird ein **MediaSource**-Objekt aus einer Datei im lokalen Speicher der App erstellt. Anschließend wird aus der Quelle ein **MediaPlaybackItem**-Objekt erstellt und der **Source**-Eigenschaft des Players zugewiesen.

Anders als **MediaElement** startet **MediaPlayer** nicht standardmäßig automatisch mit der Wiedergabe. Sie können die Wiedergabe starten, indem Sie [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.play) aufrufen, für die [**AutoPlay**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.autoplay) -Eigenschaft „true“ festlegen oder warten, bis der Benutzer die Wiedergabe mit den integrierten Media-Steuerelementen startet.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Wenn Ihre App die Verwendung eines **MediaPlayers** beendet hat, sollten Sie die Methode[**Close**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.close) aufrufen (**Dispose**-Methode in C#), um die vom Player verwendeten Ressourcen zu bereinigen.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Rendern von Videos in XAML mit MediaPlayerElement
Sie können Medien in einem **MediaPlayer** wiedergeben, ohne sie in XAML anzuzeigen. Viele Medienwiedergabe-Apps versuchen jedoch, die Medien auf einer XAML-Seite zu rendern. Verwenden Sie hierfür das einfache [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelement. Wie mit **MediaElement** können Sie mit **MediaPlayerElement** festlegen, ob die integrierten Transport-Steuerelemente angezeigt werden sollen.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Sie können die **MediaPlayer** -Instanz festlegen, an die das Element gebunden ist, indem Sie [**SetMediaPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) aufrufen.

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

Sie können für das **MediaPlayerElement** auch die Wiedergabequelle festlegen. Das Element erstellt dann automatisch eine neue **MediaPlayer**-Instanz, auf die Sie mithilfe der [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer)-Eigenschaft zugreifen können.

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> Wenn Sie [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) für [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) deaktivieren, indem Sie [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) auf „false“ festlegen, wird die von **MediaPlayerElement** bereitgestellte Verknüpfung zwischen **MediaPlayer** und [**TransportControls**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) getrennt, sodass die integrierten Transportsteuerelemente nicht mehr automatisch die Wiedergabe des Players steuern. Stattdessen müssen Sie Ihre eigenen Steuerelemente zum Steuern des **MediaPlayers** implementieren.

## <a name="common-mediaplayer-tasks"></a>Allgemeine MediaPlayer-Aufgaben
In diesem Abschnitt erfahren Sie, wie Sie verschiedene Features des **MediaPlayers** verwenden.

### <a name="set-the-audio-category"></a>Festlegen der AudioCategory-Eigenschaft
Legen Sie für die [**Audiocategory**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.audiocategory)-Eigenschaft eines **MediaPlayers** einen der Werte der [**MediaPlayerAudioCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayerAudioCategory)-Enumeration fest, um dem System mitzuteilen, welche Art von Medien Sie wiedergeben. Spiele sollten als Kategorie ihrer Musikdatenströme **GameMedia** angeben, sodass die Musik des Spiels automatisch auf stumm geschaltet wird, wenn eine andere Anwendung im Hintergrund Musik wiedergibt. Musik- oder Video-Apps sollten als Kategorien für ihre Datenströme **Media** oder **Movie** angeben, sodass ihnen gegenüber **GameMedia**-Datenströmen Priorität eingeräumt wird.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

### <a name="output-to-a-specific-audio-endpoint"></a>Ausgabe an einen bestimmten Audio-Endpunkt
Die Audioausgabe eines **MediaPlayers** wird standardmäßig zum Standard-Audio-Endpunkt des Systems geleitet. Sie können jedoch auch einen bestimmten Audio-Endpunkt als Ausgabe für den **MediaPlayer** festlegen. Im folgenden Beispiel gibt [**MediaDevice.GetAudioRenderSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) eine Zeichenfolge zur eindeutigen Identifizierung der Audiorendering-Kategorie von Geräten zurück. Als Nächstes wird die [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Methode [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) aufgerufen, um eine Liste aller verfügbaren Geräte des ausgewählten Typs zu erstellen. Sie können programmgesteuert festlegen, welches Gerät Sie verwenden möchten, oder die zurückgegebenen Geräte zu einer [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) hinzufügen, damit der Benutzer ein Gerät auswählen kann.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

Im [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Ereignis für das Geräte-Kombinationsfeld wird die [**AudioDevice**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.audiodevice)-Eigenschaft des **MediaPlayers** auf das ausgewählte Gerät festgelegt, die in der [**Tag**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.tag)-Eigenschaft des **ComboBoxItem** gespeichert wurde.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

### <a name="playback-session"></a>Wiedergabesitzung
Wie zuvor in diesem Artikel beschrieben, wurden viele der von der **MediaElement**-Klasse verfügbar gemachten Funktionen in die [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession)-Klasse verschoben. Dazu gehören Informationen über den Wiedergabestatus des Players, z. B. die aktuelle Wiedergabeposition, ob der Player Medien wiedergibt bzw. angehalten wurde sowie die aktuelle Wiedergabegeschwindigkeit. **MediaPlaybackSession** stellt außerdem einige Ereignisse bereit, um Sie bei Statusänderungen zu benachrichtigen. Dazu gehören der aktuelle Puffer- und Download-Status der wiedergegebenen Inhalte sowie die natürliche Größe und das Seitenverhältnis des aktuell wiedergegebenen Videoinhalts.

Das folgende Beispiel zeigt, wie Sie einen Klickhandler für Schaltflächen implementieren können, der bei der Medienwiedergabe 10 Sekunden überspringt. Zuerst wird das **MediaPlaybackSession**-Objekt für den Player mit der [**PlaybackSession**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.playbacksession)-Eigenschaft abgerufen. Als Nächstes wird die [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position)-Eigenschaft auf die aktuelle Wiedergabeposition plus 10 Sekunden festgelegt.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

Das nächste Beispiel zeigt, wie durch Einstellen der [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate)-Eigenschaft der Sitzung mithilfe einer Schaltfläche zwischen der normalen Wiedergabegeschwindigkeit und zweifacher Geschwindigkeit gewechselt werden kann.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

Ab Windows 10, Version 1803, können Sie die Drehung, mit der ein Video in **MediaPlayer** angezeigt wird, in 90-Grad-Schritten festlegen.

[!code-cs[SetRotation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetRotation)]

### <a name="detect-expected-and-unexpected-buffering"></a>Erkennen der erwarteten und unerwarteten Pufferung
Das im vorherigen Abschnitt beschriebene **MediaPlaybackSession**-Objekt verfügt über zwei Ereignisse, um zu erkennen, wann die aktuell wiedergegebene Mediendatei das Puffern beginnt und endet, **[BufferingStarted](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** und **[BufferingEnded](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)** . Dadurch können Sie die Benutzeroberfläche aktualisieren, um dem Benutzer anzuzeigen, das eine Pufferung durchgeführt wird. Die anfängliche Pufferung wird erwartet, wenn eine Mediendatei zuerst geöffnet wird oder wenn der Benutzer auf ein neues Element in einer Wiedergabeliste wechselt. Eine unerwartete Pufferung kann auftreten, wenn die Geschwindigkeit im Netzwerk beeinträchtigt wird oder wenn beim System für die Inhalte technische Probleme auftreten. Ab RS3 können Sie das **BufferingStarted**-Ereignis verwenden, um zu bestimmen, ob das Puffer-Ereignis erwartet oder nicht erwartet wird und die Wiedergabe unterbrochen wird. Sie können diese Informationen als Telemetriedaten für Ihrem Dienstanbieter-App oder ein Medium verwenden. 

Registrieren Sie Handler für die **BufferingStarted** und **BufferingEnded**-Ereignisse, um Pufferungszustandsbenachrichtigungen zu erhalten.

[!code-cs[RegisterBufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingHandlers)]

Im **BufferingStarted**-Ereignishandler, übergebenen Sie die Ereignisargumente an das **[MediaPlaybackSessionBufferingStartedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** -Objekt, und überprüfen Sie die **[IsPlaybackInterruption](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)** -Eigenschaft. Wenn dieser Wert WAHR ist, ist die Pufferung, die das Ereignis ausgelöst hat, unerwartet und unterbricht die Wiedergabe. Andernfalls ist es das erwartete erste Puffern. 

[!code-cs[BufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetBufferingHandlers)]


### <a name="pinch-and-zoom-video"></a>Zwei-Finger-Zoomen von Video
**MediaPlayer** ermöglicht Ihnen, im Videoinhalt das zu rendernde Quellrechteck festzulegen und so in das Video hineinzuzoomen. Das angegebene Rechteck bezieht sich auf ein normalisiertes Rechteck (0,0,1,1) wobei 0,0 der oberen linken Ecke des Frames entspricht und 1,1 die volle Breite und Höhe des Frames angibt. Um beispielsweise das Zoomrechteck so festzulegen, dass der obere rechte Quadrant des Videos gerendert wird, müssten Sie das Rechteck wie folgt angeben: (.5,0,.5,.5).  Es ist wichtig, die Werte zu überprüfen, um sicherzustellen, dass Ihr Quellrechteck sich innerhalb des normalisierten Rechtecks (0,0,1,1) befindet. Durch den Versuch, einen Wert außerhalb dieses Bereichs festzulegen, wird eine Ausnahme ausgelöst.

Um den Zwei-Finger-Zoom mithilfe von Multitouchbewegungen zu implementieren, müssen Sie zunächst angeben, welche Gesten unterstützt werden sollen. In diesem Beispiel wurden Gesten zum Skalieren und Übersetzen angefordert. Das [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)-Ereignis wird ausgelöst, wenn eine der abonnierten Gesten auftritt. Das [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)-Ereignis wird verwendet, um den Zoom auf den vollständigen Frame zurückzusetzen. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

Deklarieren Sie als Nächstes ein **Rect**-Objekt, welches das aktuelle Zoom-Quellrechteck speichert.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

Der **ManipulationDelta**-Handler passt die Skalierung oder die Übersetzung des Zoom-Rechtecks an. Ist der Deltawert für die Skalierung nicht 1, bedeutet dies, dass der Benutzer eine Zwei-Finger-Zoom-Geste ausgeführt hat. Wenn der Wert größer als 1 ist, muss das Quellrechteck verkleinert werden, um den Inhalt zu vergrößern. Wenn der Wert kleiner als 1 ist, sollte das Quellrechteck vergrößert werden, um den Inhalt zu verkleinern. Vor Festlegen der neuen Skalierungswerte wird das resultierende Rechteck überprüft, um sicherzustellen, dass es vollständig innerhalb der Grenzen von (0,0,1,1) liegt.

Wenn der Skalierungswert 1 ist, wird die Übersetzungsgeste behandelt. Das Rechteck wird wie folgt übersetzt: Anzahl der Pixel in der Geste geteilt durch die Breite und Höhe des Steuerelements. Wieder wird das resultierende Rechteck überprüft, um sicherzustellen, dass es innerhalb der Grenzen von (0,0,1,1) liegt.

Schließlich wird die [**NormalizedSourceRect**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.normalizedsourcerect)-Eigenschaft der **MediaPlaybackSession** auf das neu angepasste Rechteck festgelegt. Dabei wird der Bereich innerhalb des Video-Frames angegeben, der gerendert werden soll.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

Im [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)-Ereignishandler wird das Quellrechteck wieder auf (0,0,1,1) festgelegt, damit der gesamte Videoframe gerendert wird.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]

### <a name="handling-policy-based-playback-degradation"></a>Behandeln von richtlinienbasierten Wiedergabe-Beeinträchtigungen

In einigen Fällen kann das System die Wiedergabe von einem Medienelement beeinträchtigen, wie die Reduzierung der Auflösung (Einschränkung), basierend auf einer Richtlinie statt einem Leistungsproblem. Ein Video kann z. B. durch das System beeinträchtigt werden, wenn das Video einen nicht signierten Treiber verwendet. Rufen Sie [**MediaPlaybackSession.GetOutputDegradationPolicyState**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) auf, um zu bestimmen, ob und warum diese richtlinienbasierte Beeinschränkung erfolgt und warnen Sie den Benutzer oder notieren Sie den Grund für Telemetriezwecke.

Das folgende Beispiel zeigt eine Implementierung eines Handlers für das **MediaPlayer.MediaOpened**-Ereignis, das ausgelöst wird, wenn der Spieler ein neues Medienobjekt öffnet. **GetOutputDegradationPolicyState** wird aufgerufen, wenn **MediaPlayer** an den Handler übergeben wird. Der Wert von [**VideoConstrictionReason**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) gibt die Ursache der Richtlinie an, warum das Video eingeschränkt ist. Wenn der Wert nicht **Keine** ist, protokolliert dieses Beispiel den Grund der Beeinträchtigung zu Telemetriezwecken. Dieses Beispiel zeigt, wie die Bitrate der **AdaptiveMediaSource**, die gerade wiedergegeben wird, auf die niedrigste Bandbreite zum Speichern der Datennutzung festgelegt wird, da das Video eingeschränkt ist und nicht mit hoher Auflösung angezeigt wird. Weitere Informationen über **AdaptiveMediaSource**, finden Sie unter [Adaptives Streaming](adaptive-streaming.md).

[!code-cs[PolicyDegradation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPolicyDegradation)]
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Rendern von Videos auf einer Windows.UI.Composition-Oberfläche mit MediaPlayerSurface
Ab Windows 10, Version 1607, können Sie mit **MediaPlayer** Videos auf einer [**ICompositionSurface**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ICompositionSurface) rendern. Dadurch kann der Player mit den APIs im [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)-Namespace verwendet werden. Das Kompositions-Framework ermöglicht Ihnen, auf der visuellen Ebene zwischen XAML und den DirectX-Grafik-APIs auf niedriger Ebene Grafiken zu verwenden. Dies ermöglicht Szenarien wie das Rendering von Videos in alle XAML-Steuerelemente. Weitere Informationen zur Verwendung der Composition-APIs finden Sie unter [visuelle Ebene](https://docs.microsoft.com/windows/uwp/composition/visual-layer).

Das folgende Beispiel veranschaulicht das Rendern von Inhalten des Videoplayers auf ein [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas)-Steuerelement. Die mediaplayerspezifischen Aufrufe in diesem Beispiel sind [**SetSurfaceSize**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.setsurfacesize) und [**GetSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.getsurface). **SetSurfaceSize** teilt dem System die Größe des Puffers mit, der für das Rendern von Inhalten zugeordnet werden muss. **GetSurface** verwendet einen [**Compositor**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Compositor) als Argument und ruft eine Instanz der [**MediaPlayerSurface**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayerSurface)-Klasse ab. Diese Klasse ermöglicht den Zugriff auf den **MediaPlayer** und den **Compositor**, mit denen die Oberfläche erstellt wird. Die Klasse macht außerdem die Oberfläche selbst über die [**CompositionSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayersurface.compositionsurface)-Eigenschaft verfügbar.

Der restliche Code in diesem Beispiel erstellt ein [**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)-Element, in den das Video gerendert wird, und legt als Größe die Größe des Canvas-Elements fest, das das Visual anzeigt. Als Nächstes wird ein [**CompositionBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) aus der [**MediaPlayerSurface**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayerSurface) erstellt und der [**Brush**](https://docs.microsoft.com/uwp/api/windows.ui.composition.spritevisual.brush)-Eigenschaft des Visuals zugeordnet. Dann wird ein [**ContainerVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) erstellt, und das **SpriteVisual** wird auf der oberen Ebene der visuellen Struktur eingefügt. Schließlich wird [**SetElementChildVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual) aufgerufen, um das Container-Visual dem **Canvas** zuzuordnen.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Synchronisieren von Inhalten zwischen mehreren Playern mit MediaTimelineController
Wie in diesem Artikel bereits erläutert, können in Ihrer App mehrere **MediaPlayer**-Objekte gleichzeitig aktiv sein. Standardmäßig funktioniert jeder von Ihnen erstellte **MediaPlayer** unabhängig. In einigen Szenarien, z. B. beim Synchronisieren einer Kommentarspur mit einem Video, müssen möglicherweise der Status des Players, die Wiedergabeposition und die Wiedergabegeschwindigkeit von mehreren Playern synchronisiert werden. Ab Windows 10, Version 1607, können Sie dieses Verhalten mithilfe der [**MediaTimelineController**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaTimelineController)-Klasse implementieren.

### <a name="implement-playback-controls"></a>Implementieren der Wiedergabe-Steuerelemente
Das folgende Beispiel zeigt, wie Sie mit einem **MediaTimelineController** zwei Instanzen des **MediaPlayers** steuern können. Zuerst werden alle Instanzen des **MediaPlayers** instanziiert und eine Mediendatei als **Source** festgelegt. Als Nächstes wird eine neue **MediaTimelineController**-Klasse erstellt. Bei jedem **MediaPlayer** wird der mit den einzelnen Playern verknüpfte [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) deaktiviert, indem die [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled)-Eigenschaft auf „false“ festgelegt wird. Anschließend wird die [**TimelineController**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.timelinecontroller)-Eigenschaft auf den Zeitachsencontroller festgelegt.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**Achtung** Die [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager)-Klasse stellt eine automatische Integration zwischen **MediaPlayer** und den Steuerelementen für den Systemmedientransport (System Media Transport Controls, SMTC) bereit. Diese automatische Integration kann jedoch nicht für Media Player verwendet werden, die über eine **MediaTimelineController**-Klasse gesteuert werden. Daher müssen Sie vor dem Festlegen des Zeitachsencontrollers des Players den Befehlsmanager des Media Players deaktivieren. Bei unterlassen, führt zu einer Ausnahme mit der folgenden Meldung ausgelöst wird: "Die Anfügen der Datenträger-Zeitachsen-Controller ist aufgrund des aktuellen Status des Objekts blockiert." Weitere Informationen zur Integration des Media Players in die SMTC finden Sie unter [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md). Auch wenn Sie eine **MediaTimelineController**-Klasse verwenden, können Sie die SMTC weiterhin manuell steuern. Weitere Informationen finden Sie unter [Manuelle Steuerung der Steuerelemente für den Systemmedientransport](system-media-transport-controls.md).

Nachdem Sie eine **MediaTimelineController**-Klasse einem oder mehreren Media Player zugewiesen haben, können Sie den Wiedergabestatus mit den vom Controller bereitgestellten Methoden steuern. Im folgenden Beispiel wird [**Start**](https://docs.microsoft.com/uwp/api/windows.media.mediatimelinecontroller.start) aufgerufen, um die Wiedergabe aller zugeordneten Media Player zu Beginn des Mediums zu starten.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

Dieses Beispiel veranschaulicht das Anhalten und Fortsetzen aller zugeordneten Media Player.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

Für den schnellen Vorlauf aller verbundenen Media Player muss die Wiedergabegeschwindigkeit auf einen Wert größer als 1 festgelegt werden.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

Das nächste Beispiel zeigt die Verwendung eines **Slider**-Steuerelements, um die aktuelle Wiedergabeposition des Zeitachsencontrollers in Relation zum Inhalt eines der verbundenen Media Player anzuzeigen. Zunächst wird eine neue **MediaSource** erstellt und ein Handler für das [**OpenOperationCompleted**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.openoperationcompleted)-Ereignis der Medienquelle registriert. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

Der **OpenOperationCompleted**-Handler bietet eine Möglichkeit, um die Dauer des Inhalts der Medienquelle festzustellen. Sobald die Dauer bestimmt ist, wird der Höchstwert des **Slider**-Steuerelements auf die Gesamtzahl der Sekunden des Medienelements festgelegt. Der Wert wird innerhalb eines Aufrufs von [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) festgelegt, um sicherzustellen, dass es im UI-Thread ausgeführt wird.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

Als Nächstes wird ein Handler für das [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.media.mediatimelinecontroller.positionchanged)-Ereignis des Zeitachsencontrollers registriert. Dieses wird in regelmäßigen Abständen durch das System aufgerufen, ungefähr viermal pro Sekunde.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

Im Handler für **PositionChanged** wird der Schiebereglerwert aktualisiert, sodass er die aktuelle Position des Zeitachsencontrollers wiedergibt.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

### <a name="offset-the-playback-position-from-the-timeline-position"></a>Versetzen der Wiedergabeposition in Relation zur Zeitachsenposition
In einigen Fällen soll möglicherweise die Wiedergabeposition eines oder mehrerer mit einem Zeitachsencontroller verknüpften Media Player in Relation zu den anderen Playern versetzt werden. Dafür können Sie die [**TimelineControllerPositionOffset**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.timelinecontrollerpositionoffset)-Eigenschaft des **MediaPlayer**-Objekts festlegen, welches versetzt werden soll. Im folgenden Beispiel wird anhand der jeweiligen Dauer des Inhalts von zwei Media Playern der Minimal- und Maximalwert von zwei Schieberegler-Steuerelementen festgelegt, um die Länge des Elements zu verkürzen bzw. zu verlängern.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

Im [**ValueChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)-Ereignis jedes Schiebereglers wird **TimelineControllerPositionOffset** für jeden Player auf den entsprechenden Wert festgelegt.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Beachten Sie: Wird der Offsetwert eines Players einer negativen Wiedergabeposition zugeordnet, wird der Clip angehalten, bis der Offset den Wert Null erreicht. Anschließend beginnt die Wiedergabe. Wenn der Offsetwert einer Wiedergabeposition zugeordnet ist, welche die Dauer des Medientitels überschreitet, wird entsprechend das letzte Bild angezeigt. Dies entspricht dem Vorgehen, wenn ein einzelner Media Player das Ende der Inhaltswiedergabe erreicht.

## <a name="play-spherical-video-with-mediaplayer"></a>Wiedergeben von sphärischen Videos mit MediaPlayer
Ab Windows 10 Version 1703 unterstützt **MediaPlayer** equirektanguläre Projektionen für kugelförmige Videowiedergabe. **MediaPlayer** unterscheidet sphärische Videoinhalte nicht von anderen und spielt das Video ab, sofern die Video-Codierung unterstützt wird. Falls das sphärische Video einen Metadatentag enthält, der für das Video equirektanguläre Projektion fordert, kann **MediaPlayer** das Video mithilfe dieser Sichtfeld- und Ausrichtungsinformationen wiedergeben. Dies ermöglicht Szenarien wie Virtual-Reality-Videowiedergabe durch Anzeigebrillen oder einfache Bildschwenks innerhalb des sphärischen Videoinhalt durch Maus- oder Tastatureingaben des Benutzers.

Für sphärische Videowiedergaben befolgen Sie die zuvor in diesem Artikel beschriebenen Schritte für die Videowiedergabe. Ein zusätzliche Schritt ist das Registrieren des Handlers für das [**MediaPlayer.MediaOpened**])https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened) -Ereignis. Dieses Ereignis bietet Ihnen die Möglichkeit, Parameter für die sphärische Videowiedergabe zu aktivieren und zu steuern.

[!code-cs[OpenSphericalVideo](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenSphericalVideo)]

Prüfen Sie im **MediaOpened**-Handler zunächst das Frameformat des neu geöffneten Medienelements mithilfe der Eigenschaft [**PlaybackSession.SphericalVideoProjection.FrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat). Wenn dieser Wert [**SphericaVideoFrameFormat.Equirectangular**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat) lautet, kann das System den Videoinhalt automatisch projizieren. Legen Sie zuerst die Eigenschaft [**PlaybackSession.SphericalVideoProjection.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) auf **true** fest. Sie können auch Eigenschaften wie das Sichtfeld und die Ausrichtung anpassen, mit denen MediaPlayer die Videoinhalte projiziert. In diesem Beispiel wurde das Sichtfeld über die [**HorizontalFieldOfViewInDegrees**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees)-Eigenschaft auf einen breiten Wert von 120 Grad festgelegt.

Für sphärische Videoinhalte, die nicht das equirektanguläre Format haben, können Sie mithilfe des MediaPlayer-FrameServer-Modus eigene Projektionsalgorithmen implementieren, die einzelne Frames empfangen und verarbeiten.

[!code-cs[SphericalMediaOpened](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalMediaOpened)]

Der folgende Beispielcode zeigt, wie die Ausrichtung von sphärischen Videos mithilfe der Links- und Rechts-Pfeiltasten angepasst wird.

[!code-cs[SphericalOnKeyDown](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalOnKeyDown)]

Wenn Ihre App Wiedergabelisten für Videos unterstützt, können Sie Elemente für sphärische Videowiedergabe in Ihrer UI identifizieren. Medien-Wiedergabelisten werden ausführlich im Artikel [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md) beschrieben. Das folgende Beispiel zeigt, wie Sie eine neue Wiedergabeliste erstellen, ein Element hinzufügen und einen Handler für das [**MediaPlaybackItem.VideoTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged)-Ereignis registrieren, das auftritt, wenn die Videotitel eines Medienelements aufgelöst werden.

[!code-cs[SphericalList](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalList)]

Die Codierungseigenschaften für alle hinzugefügten Videotitel erhalten Sie im **VideoTracksChanged**-Ereignishandler durch den Aufruf [**VideoTrack.GetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.core.videotrack.GetEncodingProperties). Wenn der Wert für [**SphericalVideoFrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) in den Codierungseigenschaften anders als [**SphericaVideoFrameFormat.None**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat) lautet, enthält der Titel sphärische Videoinhalte, und Sie können die Benutzeroberfläche bei Bedarf aktualisieren.

[!code-cs[SphericalTracksChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalTracksChanged)]

## <a name="use-mediaplayer-in-frame-server-mode"></a>Verwenden des MediaPlayer-FrameServer-Modus
Ab Windows 10 Version 1703 können Sie **MediaPlayer** im FrameServer-Modus ausführen. In diesem Modus rendert **MediaPlayer** die Frames für ein zugehöriges **MediaPlayerElement** nicht automatisch. Stattdessen kopiert Ihre App den aktuellen Frame von **MediaPlayer** in ein Objekt, das eine [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface) implementiert. Dieses Feature ermöglicht vor allem Szenarios, in denen Videoframes von **MediaPlayer** durch Pixel-Shader verarbeitet werden. Nach der Verarbeitung übernimmt Ihre App die Anzeige jedes Frames, um ihn z. B. in einem XAML-[**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image)-Steuerelement anzuzeigen.

Im folgenden Beispiel wird ein neuer **MediaPlayer** initialisiert, um Videoinhalte zu laden. Anschließend wird ein Handler für [**VideoFrameAvailable**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable) registriert. Um den FrameServer-Modus zu aktivieren, wird die **MediaPlayer**-Objekteigenschaft [**IsVideoFrameServerEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) auf **true** festgelegt. Anschließend wird mit dem Aufruf [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play) die Medienwiedergabe gestartet.

[!code-cs[FrameServerInit](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFrameServerInit)]

Das nächste Beispiel zeigt einen Handler für **VideoFrameAvailable**, der durch [Win2D](https://github.com/Microsoft/Win2D) jedem Frame eines Videos einen Weichzeichnereffekt hinzufügt und die verarbeiteten Frames in einem XAML-[Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image)-Steuerelement anzeigt.

Mit jedem Aufruf des **VideoFrameAvailable**-Ereignishandlers wird der Inhalt eines Frames mit der [**CopyFrameToVideoSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface)-Methode in eine [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface) kopiert. Mit dem Aufruf [**CopyFrameToStereoscopicVideoSurfaces**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) können Sie 3D-Inhalte auch in zwei Oberflächen kopieren, um Inhalte für das linke und rechte Auge getrennt zu verarbeiten. Um ein Objekt abzurufen, das **IDirect3DSurface**  implementiert, erstellt dieses Beispiel mithilfe eines [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap)-Objekts eine Win2D **CanvasBitmap**, welche die benötigte Schnittstelle implementiert. Ein **CanvasImageSource**-Win2D-Objekt kann als Quelle für ein neu zu erstellendes **Image**-Steuerelement verwendet werden, das dann als Quelle für das **Bild** dient, welches den Inhalt anzeigt. Als Nächstes wird eine **CanvasDrawingSession** erstellt. Dies wird von Win2D verwendet, um den Weichzeichnereffekt zu rendern.

Nachdem alle erforderlichen Objekte instanziiert sind, wird **CopyFrameToVideoSurface** aufgerufen, um den aktuellen Frame von **MediaPlayer** in die **CanvasBitmap** zu kopieren. Anschließend wird ein Win2D-**GaussianBlurEffect** erstellt, für den **CanvasBitmap** als Quelle festgelegt ist. Zum Schluss wird **CanvasDrawingSession.DrawImage** aufgerufen, um das Quellbild mit angewendetem Weichzeichnereffekt in die **CanvasImageSource** und deren zugeordnetes **Bild**-Steuerelement in der UI zu zeichnen.

[!code-cs[VideoFrameAvailable](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetVideoFrameAvailable)]

Weitere Informationen zu Win2D finden Sie im [Win2D-Repository auf GitHub](https://github.com/Microsoft/Win2D). Um den oben gezeigten Beispielcode zu testen, müssen Sie Ihrem Projekt das Win2D-NuGet-Paket mit folgenden Anweisungen hinzufügen.

**Auf das Win2D-NuGet-Paket Ihrem Projekt Effekt hinzufügen**

1.  Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf Ihr Projekt und wählen Sie **NuGet-Pakete verwalten**.
2.  Wählen Sie oben im Fenster die Registerkarte **Durchsuchen** aus.
3.  Geben Sie im Suchfeld **Win2D** ein.
4.  Wählen Sie **Win2D.uwp** und anschließend im rechten Bereich **Installieren** aus.
5.  Im Dialogfeld **Änderungen überprüfen** wird das zu installierende Paket angezeigt. Klicken Sie auf **OK**.
6.  Akzeptieren Sie die Paketlizenz.

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Erkennen und Reagieren auf Änderungen der Audioebene vom System
Ab Windows 10, Version 1803, erkennt Ihre App, wenn das System die Lautstärke der Audioaufnahme des aktuell wiedergegebenen **MediaPlayer**. Beispielsweise kann das System die Audiowiedergabe-Ebene reduzieren, wenn ein Alarm klingelt. Das System schaltet Ihre App stumm, wenn sie in den Hintergrund wechselt, falls Ihre App die *BackgroundMediaPlayback*-Funktion im App-Manifest nicht aktiviert hat. Mit der [**AudioStateMonitor**](https://docs.microsoft.comuwp/api/windows.media.audio.audiostatemonitor)-Klasse können Sie sich registrieren, um ein Ereignis zu erhalten, wenn das System die Lautstärke eines Audiostreams ändert. Greifen Sie auf die **AudioStateMonitor**-Eigenschaft eines **MediaPlayers** zu und registrieren Sie einen Handler für das [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged)-Ereignis, um informiert zu werden, wenn die Lautstärke für diesen **MediaPlayer** vom System geändert wird.

[!code-cs[RegisterAudioStateMonitor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

Bei der Behandlung des **SoundLevelChanged**-Ereignis, können Sie verschiedene Aktionen ausführen, je nach Art des wiedergegebenen Inhalts. Wenn Sie zurzeit Musik wiedergegeben, empfiehlt es sich die Musik abzuspielen, während das Volume gedämpft ist. Wenn Sie allerdings einen Podcast spielen, empfiehlt sich die Wiedergabe anzuhalten, während die Audiowiedergabe gedämpft ist, damit der Benutzer keinen Inhalt verpasst.

In diesem Beispiel deklariert eine Variable das Nachverfolgen des derzeit wiedergegebenen Inhalts eines Podcasts, vorausgesetzt, dass Sie dies auf den entsprechenden Wert festlegt haben, wenn der Inhalt für den **MediaPlayer** ausgewählt ist. Zudem erstellen wir eine Klassenvariable, um nachzuverfolgen, wann wir die Wiedergabe programmgesteuert anhalten, wenn die Lautstärke geändert wird.

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

Im **SoundLevelChanged**-Ereignis-Handler können Sie die [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel)-Eigenschaft des **AudioStateMonitor**-Senders überprüfen, um die neue Soundebene zu bestimmen. In diesem Beispiel wird überprüft, ob die neue Lautstärke die volle Lautstärke ist, was bedeutet, dass das System das Stummschalten oder Dämpfen der Lautstärke beendet hat, oder wenn die Lautstärke verringert wurde, und ein anderer Inhalt als den des Podcast wiedergegeben wird. Wenn einer der beider Werte "wahr" ist, und der Inhalt programmgesteuert zuvor angehalten wurde, wird die Wiedergabe fortgesetzt. Wenn die neue Lautstärke stumm geschaltet ist oder wenn der aktuelle Inhalt ein Podcast ist und die Lautstärke gesenkt ist, wird die Wiedergabe angehalten, und die Variable wird festgelegt, um nachzuverfolgen, ob die Pause programmgesteuert initiiert wurde.

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

Der Benutzer kann entscheiden, ob er die Wiedergabe anhalten oder fortsetzen möchte, auch wenn die Audiowiedergabe vom System gedämpft ist. Dieses Beispiel zeigt die Ereignishandler für die Wiedergabe und Pause-Tasten. Wenn der Klickhandler der Pause-Schaltfläche angehalten ist, wenn die Wiedergabe bereits programmgesteuert angehalten wurde, aktualisieren wir die Variable, um anzugeben, dass der Benutzer den Inhalt angehalten hat. Wir setzen die Wiedergabe im Klickhandler der Pause-Schaltfläche fort und löschen die Tracking-Variable.

[!code-cs[ButtonUserClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetButtonUserClick)]

## <a name="related-topics"></a>Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Medienobjekte, Wiedergabelisten und Spuren](media-playback-with-mediasource.md)
* [Integrieren Sie in die System-Media-Transport-Steuerelemente](integrate-with-systemmediatransportcontrols.md)
* [Erstellen Sie, Planen Sie und verwalten Sie Datenträger Pausen](create-schedule-and-manage-media-breaks.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md)





 

 




