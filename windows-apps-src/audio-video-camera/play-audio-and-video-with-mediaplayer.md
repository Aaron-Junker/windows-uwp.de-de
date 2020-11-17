---
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: In diesem Artikel erfahren Sie, wie Sie in Ihrer universellen Windows-App mithilfe von „MediaPlayer“ Medien wiedergeben.
title: Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 166a498ba7323869fe60f7d3392b93ac501dd331
ms.sourcegitcommit: 75e1f49be211e8b4b3e825978d67625776f992f5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94691548"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“

In diesem Artikel erfahren Sie, wie Sie in Ihrer universellen Windows-App mithilfe der [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse Medien wiedergeben. Mit Windows 10, Version 1607, wurden bedeutende Verbesserungen an den Medienwiedergabe-APIs vorgenommen, darunter ein vereinfachtes Einzel Prozessdesign für Hintergrund-Audiodaten, die automatische Integration in die System Media Transport-Steuerelemente (System Media Transport Controls, SMTC), die Möglichkeit zum Synchronisieren mehrerer Medien Player, die Möglichkeit zum renderingvideo Frames auf eine Windows. UI Um diese Verbesserungen optimal zu nutzen, sollten Sie für die Medienwiedergabe anstelle von **MediaElement** die **MediaPlayer**-Klasse verwenden. Das einfache XAML-Steuerelement [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) wurde eingeführt, damit Sie Medieninhalte auf einer XAML-Seite rendern können. Viele der von **MediaElement** bereitgestellten Wiedergabesteuerelemente und Status-APIs sind jetzt über das neue [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession)-Objekt verfügbar. **MediaElement** gewährleistet weiterhin die Abwärtskompatibilität. Dieser Klasse werden jedoch keine weiteren Features hinzugefügt.

Dieser Artikel erläutert die Features von **MediaPlayer**, die von einer typischen App zur Medienwiedergabe verwendet werden. Beachten Sie, dass **MediaPlayer** die [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource)-Klasse als Container für alle Medienelemente verwendet. Diese Klasse ermöglicht Ihnen das Laden und Wiedergeben von Medien aus verschiedenen Quellen – z. B. aus lokalen Dateien, Speicherdatenströmen und Netzwerkquellen – über die gleiche Schnittstelle. Verschiedene Klassen auf höherer Ebene arbeiten ebenfalls mit **MediaSource**, z. B. [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) und [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList). Sie stellen erweiterte Features bereit wie Wiedergabelisten und die Möglichkeit, Medienquellen mit mehreren Audio-, Video- und Metadatentiteln zu verwalten, Weitere Informationen zu **MediaSource** und verwandten APIs finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

> [!NOTE] 
> Die Editionen Windows 10 N und Windows 10 kn enthalten nicht die Medien Features, die für die Verwendung von **Media Player** für die Wiedergabe erforderlich sind. Diese Features können manuell installiert werden. Weitere Informationen finden Sie unter [Media Feature Pack für die Editionen Windows 10 N und Windows 10 kn](https://support.microsoft.com/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions).

## <a name="play-a-media-file-with-mediaplayer"></a>Wiedergeben einer Mediendatei mit „MediaPlayer“  
Die grundlegende Medienwiedergabe mit **MediaPlayer** ist sehr einfach zu implementieren. Erstellen Sie zunächst eine neue Instanz der **MediaPlayer**-Klasse. In Ihrer App können mehrere **MediaPlayer**-Instanzen gleichzeitig aktiv sein. Legen Sie dann für die [**Source**](/uwp/api/windows.media.playback.mediaplayer.source)-Eigenschaft des Players ein Objekt fest, welches die [**IMediaPlaybackSource**](/uwp/api/Windows.Media.Playback.IMediaPlaybackSource) implementiert, z. B. eine [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource), ein [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) oder eine [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList). In diesem Beispiel wird ein **MediaSource**-Objekt aus einer Datei im lokalen Speicher der App erstellt. Anschließend wird aus der Quelle ein **MediaPlaybackItem**-Objekt erstellt und der **Source**-Eigenschaft des Players zugewiesen.

Anders als **MediaElement** startet **MediaPlayer** nicht standardmäßig automatisch mit der Wiedergabe. Sie können die Wiedergabe starten, indem Sie [**Play**](/uwp/api/windows.media.playback.mediaplayer.play) aufrufen, für die [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) -Eigenschaft „true“ festlegen oder warten, bis der Benutzer die Wiedergabe mit den integrierten Media-Steuerelementen startet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSimpleFilePlayback":::

Wenn Ihre App die Verwendung eines **MediaPlayers** beendet hat, sollten Sie die Methode [**Close**](/uwp/api/windows.media.playback.mediaplayer.close) aufrufen (**Dispose**-Methode in C#), um die vom Player verwendeten Ressourcen zu bereinigen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetCloseMediaPlayer":::

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Rendern von Videos in XAML mit MediaPlayerElement
Sie können Medien in einem **MediaPlayer** wiedergeben, ohne sie in XAML anzuzeigen. Viele Medienwiedergabe-Apps versuchen jedoch, die Medien auf einer XAML-Seite zu rendern. Verwenden Sie hierfür das einfache [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelement. Wie mit **MediaElement** können Sie mit **MediaPlayerElement** festlegen, ob die integrierten Transport-Steuerelemente angezeigt werden sollen.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElementXAML":::

Sie können die **MediaPlayer** -Instanz festlegen, an die das Element gebunden ist, indem Sie [**SetMediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetMediaPlayer":::

Sie können für das **MediaPlayerElement** auch die Wiedergabequelle festlegen. Das Element erstellt dann automatisch eine neue **MediaPlayer**-Instanz, auf die Sie mithilfe der [**MediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer)-Eigenschaft zugreifen können.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetGetPlayerFromElement":::

> [!NOTE] 
> Wenn Sie [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) für [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) deaktivieren, indem Sie [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) auf „false“ festlegen, wird die von **MediaPlayerElement** bereitgestellte Verknüpfung zwischen **MediaPlayer** und [**TransportControls**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) getrennt, sodass die integrierten Transportsteuerelemente nicht mehr automatisch die Wiedergabe des Players steuern. Stattdessen müssen Sie Ihre eigenen Steuerelemente zum Steuern des **MediaPlayers** implementieren.

## <a name="common-mediaplayer-tasks"></a>Allgemeine MediaPlayer-Aufgaben
In diesem Abschnitt erfahren Sie, wie Sie verschiedene Features des **MediaPlayers** verwenden.

### <a name="set-the-audio-category"></a>Festlegen der AudioCategory-Eigenschaft
Legen Sie für die [**Audiocategory**](/uwp/api/windows.media.playback.mediaplayer.audiocategory)-Eigenschaft eines **MediaPlayers** einen der Werte der [**MediaPlayerAudioCategory**](/uwp/api/Windows.Media.Playback.MediaPlayerAudioCategory)-Enumeration fest, um dem System mitzuteilen, welche Art von Medien Sie wiedergeben. Spiele sollten als Kategorie ihrer Musikdatenströme **GameMedia** angeben, sodass die Musik des Spiels automatisch auf stumm geschaltet wird, wenn eine andere Anwendung im Hintergrund Musik wiedergibt. Musik- oder Video-Apps sollten als Kategorien für ihre Datenströme **Media** oder **Movie** angeben, sodass ihnen gegenüber **GameMedia**-Datenströmen Priorität eingeräumt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetAudioCategory":::

### <a name="output-to-a-specific-audio-endpoint"></a>Ausgabe an einen bestimmten Audio-Endpunkt
Die Audioausgabe eines **MediaPlayers** wird standardmäßig zum Standard-Audio-Endpunkt des Systems geleitet. Sie können jedoch auch einen bestimmten Audio-Endpunkt als Ausgabe für den **MediaPlayer** festlegen. Im folgenden Beispiel gibt [**MediaDevice.GetAudioRenderSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) eine Zeichenfolge zur eindeutigen Identifizierung der Audiorendering-Kategorie von Geräten zurück. Als Nächstes wird die [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Methode [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) aufgerufen, um eine Liste aller verfügbaren Geräte des ausgewählten Typs zu erstellen. Sie können programmgesteuert festlegen, welches Gerät Sie verwenden möchten, oder die zurückgegebenen Geräte zu einer [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) hinzufügen, damit der Benutzer ein Gerät auswählen kann.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetAudioEndpointEnumerate":::

Im [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Ereignis für das Geräte-Kombinationsfeld wird die [**AudioDevice**](/uwp/api/windows.media.playback.mediaplayer.audiodevice)-Eigenschaft des **MediaPlayers** auf das ausgewählte Gerät festgelegt, die in der [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag)-Eigenschaft des **ComboBoxItem** gespeichert wurde.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetAudioEndpontSelectionChanged":::

### <a name="playback-session"></a>Wiedergabesitzung
Wie zuvor in diesem Artikel beschrieben, wurden viele der von der **MediaElement**-Klasse verfügbar gemachten Funktionen in die [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession)-Klasse verschoben. Dazu gehören Informationen über den Wiedergabestatus des Players, z. B. die aktuelle Wiedergabeposition, ob der Player Medien wiedergibt bzw. angehalten wurde sowie die aktuelle Wiedergabegeschwindigkeit. **MediaPlaybackSession** stellt außerdem einige Ereignisse bereit, um Sie bei Statusänderungen zu benachrichtigen. Dazu gehören der aktuelle Puffer- und Download-Status der wiedergegebenen Inhalte sowie die natürliche Größe und das Seitenverhältnis des aktuell wiedergegebenen Videoinhalts.

Das folgende Beispiel zeigt, wie Sie einen Klickhandler für Schaltflächen implementieren können, der bei der Medienwiedergabe 10 Sekunden überspringt. Zuerst wird das **MediaPlaybackSession**-Objekt für den Player mit der [**PlaybackSession**](/uwp/api/windows.media.playback.mediaplayer.playbacksession)-Eigenschaft abgerufen. Als Nächstes wird die [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.position)-Eigenschaft auf die aktuelle Wiedergabeposition plus 10 Sekunden festgelegt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSkipForwardClick":::

Das nächste Beispiel zeigt, wie durch Einstellen der [**PlaybackRate**](/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate)-Eigenschaft der Sitzung mithilfe einer Schaltfläche zwischen der normalen Wiedergabegeschwindigkeit und zweifacher Geschwindigkeit gewechselt werden kann.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSpeedChecked":::

Ab Windows 10, Version 1803, können Sie die Drehung festlegen, mit der das Video in **Media Player** in Schritten von 90 Grad dargestellt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetRotation":::

### <a name="detect-expected-and-unexpected-buffering"></a>Erwartete und unerwartete Pufferung erkennen
Das im vorherigen Abschnitt beschriebene **mediaplaybacksession** -Objekt enthält zwei Ereignisse, die erkennen, wann die aktuell wiedergegebene Mediendatei beginnt und endet, **[BufferingStarted](/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** und **[BufferingEnded](/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)**. Auf diese Weise können Sie die Benutzeroberfläche aktualisieren, um den Benutzer anzuzeigen, dass eine Pufferung stattfindet. Die anfängliche Pufferung wird erwartet, wenn eine Mediendatei zum ersten Mal geöffnet wird oder wenn der Benutzer zu einem neuen Element in einer Wiedergabeliste wechselt. Unerwartete Pufferung kann auftreten, wenn die Netzwerkgeschwindigkeit beeinträchtigt wird oder wenn das Inhalts Verwaltungssystem, das den Inhalt bereitstellt, technische Probleme hat. Beginnend mit RS3 können Sie mit dem **BufferingStarted** -Ereignis ermitteln, ob das Puffer Ereignis erwartet wird, oder ob es unerwartet ist, und die Wiedergabe wird unterbrochen. Sie können diese Informationen als Telemetriedaten für Ihre APP oder den Media Delivery Service verwenden. 

Registrieren von Handlern für die **BufferingStarted** -und **BufferingEnded** -Ereignisse, um Puffer Zustands Benachrichtigungen zu empfangen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterBufferingHandlers":::

Wandeln Sie im **BufferingStarted** -Ereignishandler die an das-Ereignis übergebenen Ereignis Argumente in ein **[mediaplaybacksessionbufferingstartedeventargs](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** -Objekt um, und überprüfen Sie die **[isplaybackresolution](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)** -Eigenschaft. Wenn dieser Wert true ist, ist die Pufferung, die das Ereignis ausgelöst hat, unerwartet und unterbricht die Wiedergabe. Andernfalls wird die anfängliche Pufferung erwartet. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetBufferingHandlers":::


### <a name="pinch-and-zoom-video"></a>Zwei-Finger-Zoomen von Video
**MediaPlayer** ermöglicht Ihnen, im Videoinhalt das zu rendernde Quellrechteck festzulegen und so in das Video hineinzuzoomen. Das angegebene Rechteck bezieht sich auf ein normalisiertes Rechteck (0,0,1,1) wobei 0,0 der oberen linken Ecke des Frames entspricht und 1,1 die volle Breite und Höhe des Frames angibt. Um beispielsweise das Zoomrechteck so festzulegen, dass der obere rechte Quadrant des Videos gerendert wird, müssten Sie das Rechteck wie folgt angeben: (.5,0,.5,.5).  Es ist wichtig, die Werte zu überprüfen, um sicherzustellen, dass Ihr Quellrechteck sich innerhalb des normalisierten Rechtecks (0,0,1,1) befindet. Durch den Versuch, einen Wert außerhalb dieses Bereichs festzulegen, wird eine Ausnahme ausgelöst.

Um den Zwei-Finger-Zoom mithilfe von Multitouchbewegungen zu implementieren, müssen Sie zunächst angeben, welche Gesten unterstützt werden sollen. In diesem Beispiel wurden Gesten zum Skalieren und Übersetzen angefordert. Das [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)-Ereignis wird ausgelöst, wenn eine der abonnierten Gesten auftritt. Das [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)-Ereignis wird verwendet, um den Zoom auf den vollständigen Frame zurückzusetzen. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterPinchZoomEvents":::

Deklarieren Sie als Nächstes ein **Rect**-Objekt, welches das aktuelle Zoom-Quellrechteck speichert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareSourceRect":::

Der **ManipulationDelta**-Handler passt die Skalierung oder die Übersetzung des Zoom-Rechtecks an. Ist der Deltawert für die Skalierung nicht 1, bedeutet dies, dass der Benutzer eine Zwei-Finger-Zoom-Geste ausgeführt hat. Wenn der Wert größer als 1 ist, muss das Quellrechteck verkleinert werden, um den Inhalt zu vergrößern. Wenn der Wert kleiner als 1 ist, sollte das Quell Rechteck vergrößert werden, um verkleinert zu werden. Vor dem Festlegen der neuen Skalierungs Werte wird das resultierende Rechteck geprüft, um sicherzustellen, dass es vollständig innerhalb der Grenzwerte (0, 0, 1, 1) liegt.

Wenn der Skalierungswert 1 ist, wird die Übersetzungsgeste behandelt. Das Rechteck wird wie folgt übersetzt: Anzahl der Pixel in der Geste geteilt durch die Breite und Höhe des Steuerelements. Wieder wird das resultierende Rechteck überprüft, um sicherzustellen, dass es innerhalb der Grenzen von (0,0,1,1) liegt.

Schließlich wird die [**NormalizedSourceRect**](/uwp/api/windows.media.playback.mediaplaybacksession.normalizedsourcerect)-Eigenschaft der **MediaPlaybackSession** auf das neu angepasste Rechteck festgelegt. Dabei wird der Bereich innerhalb des Video-Frames angegeben, der gerendert werden soll.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetManipulationDelta":::

Im [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)-Ereignishandler wird das Quellrechteck wieder auf (0,0,1,1) festgelegt, damit der gesamte Videoframe gerendert wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetDoubleTapped":::

**Hinweis** In diesem Abschnitt werden Berührungs Eingaben beschrieben. Touchpad sendet Zeiger Ereignisse und sendet keine Bearbeitungs Ereignisse.

### <a name="handling-policy-based-playback-degradation"></a>Behandeln der Beeinträchtigung der Richtlinien basierten Wiedergabe

In einigen Fällen kann das System die Wiedergabe eines Medien Elements beeinträchtigen, wie z. b. das Reduzieren der Auflösung (durch strierung), basierend auf einer Richtlinie und nicht mit einem Leistungsproblem. Beispielsweise kann das Video durch das System beeinträchtigt werden, wenn es mit einem nicht signierten Videotreiber wiedergegeben wird. Sie können [**mediaplaybacksession. getoutputdegradationpolicystate**](/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) aufrufen, um zu bestimmen, ob und warum diese Richtlinien basierte einer auftritt, und den Benutzer zu benachrichtigen oder den Grund für Telemetriezwecke aufzuzeichnen.

Das folgende Beispiel zeigt eine Implementierung eines Handlers für das **Media Player. mediageöffnete** -Ereignis, das ausgelöst wird, wenn der Spieler ein neues Medien Element öffnet. **Getoutputdegradationpolicystate** wird für den **Media Player** aufgerufen, der an den-Handler weitergeleitet wird. Der Wert von [**videomenstrictionreason**](/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) gibt den Grund für die Ursache des Videos an. Wenn der Wert nicht " **None**" ist, protokolliert dieses Beispiel den Grund für die Herabstufung der Telemetrie. Außerdem wird in diesem Beispiel gezeigt, wie die Bitrate der **adaptivemediasource** , die zurzeit wiedergegeben wird, auf die niedrigste Bandbreite festgelegt wird, um die Datennutzung zu sparen, da das Video verstrichen ist und trotzdem nicht mit hoher Auflösung angezeigt wird. Weitere Informationen zur Verwendung von **adaptivemediasource** finden Sie unter [Adaptive Streaming](adaptive-streaming.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetPolicyDegradation":::
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Rendern von Videos auf einer Windows.UI.Composition-Oberfläche mit MediaPlayerSurface
Ab Windows 10, Version 1607, können Sie mit **MediaPlayer** Videos auf einer [**ICompositionSurface**](/uwp/api/Windows.UI.Composition.ICompositionSurface) rendern. Dadurch kann der Player mit den APIs im [**Windows.UI.Composition**](/uwp/api/Windows.UI.Composition)-Namespace verwendet werden. Das Kompositions-Framework ermöglicht Ihnen, auf der visuellen Ebene zwischen XAML und den DirectX-Grafik-APIs auf niedriger Ebene Grafiken zu verwenden. Dies ermöglicht Szenarien wie das Rendering von Videos in alle XAML-Steuerelemente. Weitere Informationen zur Verwendung der Composition-APIs finden Sie unter [visuelle Ebene](../composition/visual-layer.md).

Das folgende Beispiel veranschaulicht das Rendern von Inhalten des Videoplayers auf ein [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas)-Steuerelement. Die mediaplayerspezifischen Aufrufe in diesem Beispiel sind [**SetSurfaceSize**](/uwp/api/windows.media.playback.mediaplayer.setsurfacesize) und [**GetSurface**](/uwp/api/windows.media.playback.mediaplayer.getsurface). **SetSurfaceSize** teilt dem System die Größe des Puffers mit, der für das Rendern von Inhalten zugeordnet werden muss. **GetSurface** verwendet einen [**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) als Argument und ruft eine Instanz der [**MediaPlayerSurface**](/uwp/api/Windows.Media.Playback.MediaPlayerSurface)-Klasse ab. Diese Klasse ermöglicht den Zugriff auf den **MediaPlayer** und den **Compositor**, mit denen die Oberfläche erstellt wird. Die Klasse macht außerdem die Oberfläche selbst über die [**CompositionSurface**](/uwp/api/windows.media.playback.mediaplayersurface.compositionsurface)-Eigenschaft verfügbar.

Der restliche Code in diesem Beispiel erstellt ein [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual)-Element, in den das Video gerendert wird, und legt als Größe die Größe des Canvas-Elements fest, das das Visual anzeigt. Als Nächstes wird ein [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) aus der [**MediaPlayerSurface**](/uwp/api/Windows.Media.Playback.MediaPlayerSurface) erstellt und der [**Brush**](/uwp/api/windows.ui.composition.spritevisual.brush)-Eigenschaft des Visuals zugeordnet. Dann wird ein [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) erstellt, und das **SpriteVisual** wird auf der oberen Ebene der visuellen Struktur eingefügt. Schließlich wird [**SetElementChildVisual**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual) aufgerufen, um das Container-Visual dem **Canvas** zuzuordnen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetCompositor":::
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Synchronisieren von Inhalten zwischen mehreren Playern mit MediaTimelineController
Wie in diesem Artikel bereits erläutert, können in Ihrer App mehrere **MediaPlayer**-Objekte gleichzeitig aktiv sein. Standardmäßig funktioniert jeder von Ihnen erstellte **MediaPlayer** unabhängig. In einigen Szenarien, z. B. beim Synchronisieren einer Kommentarspur mit einem Video, müssen möglicherweise der Status des Players, die Wiedergabeposition und die Wiedergabegeschwindigkeit von mehreren Playern synchronisiert werden. Ab Windows 10, Version 1607, können Sie dieses Verhalten mithilfe der [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController)-Klasse implementieren.

### <a name="implement-playback-controls"></a>Implementieren der Wiedergabe-Steuerelemente
Das folgende Beispiel zeigt, wie Sie mit einem **MediaTimelineController** zwei Instanzen des **MediaPlayers** steuern können. Zuerst werden alle Instanzen des **MediaPlayers** instanziiert und eine Mediendatei als **Source** festgelegt. Als Nächstes wird eine neue **MediaTimelineController**-Klasse erstellt. Bei jedem **MediaPlayer** wird der mit den einzelnen Playern verknüpfte [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) deaktiviert, indem die [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled)-Eigenschaft auf „false“ festgelegt wird. Und dann wird die [**timelinecontroller**](/uwp/api/windows.media.playback.mediaplayer.timelinecontroller) -Eigenschaft auf das Timeline Controller-Objekt festgelegt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaTimelineController":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetTimelineController":::

**Achtung** Die [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager)-Klasse stellt eine automatische Integration zwischen **MediaPlayer** und den Steuerelementen für den Systemmedientransport (System Media Transport Controls, SMTC) bereit. Diese automatische Integration kann jedoch nicht für Media Player verwendet werden, die über eine **MediaTimelineController**-Klasse gesteuert werden. Daher müssen Sie vor dem Festlegen des Zeitachsencontrollers des Players den Befehlsmanager des Media Players deaktivieren. Andernfalls wird eine Ausnahme mit der Benachrichtigung ausgelöst, dass das Anfügen des Medienzeitachsencontrollers im aktuellen Objektzustand blockiert wird. Weitere Informationen zur Integration des Media Players in die SMTC finden Sie unter [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md). Auch wenn Sie eine **MediaTimelineController**-Klasse verwenden, können Sie die SMTC weiterhin manuell steuern. Weitere Informationen finden Sie unter [Manuelle Steuerung der Steuerelemente für den Systemmedientransport](system-media-transport-controls.md).

Nachdem Sie eine **MediaTimelineController**-Klasse einem oder mehreren Media Player zugewiesen haben, können Sie den Wiedergabestatus mit den vom Controller bereitgestellten Methoden steuern. Im folgenden Beispiel wird [**Start**](/uwp/api/windows.media.mediatimelinecontroller.start) aufgerufen, um die Wiedergabe aller zugeordneten Media Player zu Beginn des Mediums zu starten.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetPlayButtonClick":::

Dieses Beispiel veranschaulicht das Anhalten und Fortsetzen aller zugeordneten Media Player.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetPauseButtonClick":::

Für den schnellen Vorlauf aller verbundenen Media Player muss die Wiedergabegeschwindigkeit auf einen Wert größer als 1 festgelegt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetFastForwardButtonClick":::

Das nächste Beispiel zeigt die Verwendung eines **Slider**-Steuerelements, um die aktuelle Wiedergabeposition des Zeitachsencontrollers in Relation zum Inhalt eines der verbundenen Media Player anzuzeigen. Zunächst wird eine neue **MediaSource** erstellt und ein Handler für das [**OpenOperationCompleted**](/uwp/api/windows.media.core.mediasource.openoperationcompleted)-Ereignis der Medienquelle registriert. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetCreateSourceWithOpenCompleted":::

Der **OpenOperationCompleted**-Handler bietet eine Möglichkeit, um die Dauer des Inhalts der Medienquelle festzustellen. Sobald die Dauer bestimmt ist, wird der Höchstwert des **Slider**-Steuerelements auf die Gesamtzahl der Sekunden des Medienelements festgelegt. Der Wert wird innerhalb eines Aufrufs von [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) festgelegt, um sicherzustellen, dass es im UI-Thread ausgeführt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareDuration":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetOpenCompleted":::

Als Nächstes wird ein Handler für das [**PositionChanged**](/uwp/api/windows.media.mediatimelinecontroller.positionchanged)-Ereignis des Zeitachsencontrollers registriert. Dieses wird in regelmäßigen Abständen durch das System aufgerufen, ungefähr viermal pro Sekunde.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterPositionChanged":::

Im Handler für **PositionChanged** wird der Schiebereglerwert aktualisiert, sodass er die aktuelle Position des Zeitachsencontrollers wiedergibt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetPositionChanged":::

### <a name="offset-the-playback-position-from-the-timeline-position"></a>Versetzen der Wiedergabeposition in Relation zur Zeitachsenposition
In einigen Fällen soll möglicherweise die Wiedergabeposition eines oder mehrerer mit einem Zeitachsencontroller verknüpften Media Player in Relation zu den anderen Playern versetzt werden. Dafür können Sie die [**TimelineControllerPositionOffset**](/uwp/api/windows.media.playback.mediaplayer.timelinecontrollerpositionoffset)-Eigenschaft des **MediaPlayer**-Objekts festlegen, welches versetzt werden soll. Im folgenden Beispiel wird anhand der jeweiligen Dauer des Inhalts von zwei Media Playern der Minimal- und Maximalwert von zwei Schieberegler-Steuerelementen festgelegt, um die Länge des Elements zu verkürzen bzw. zu verlängern.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetOffsetSliders":::

Im [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)-Ereignis jedes Schiebereglers wird **TimelineControllerPositionOffset** für jeden Player auf den entsprechenden Wert festgelegt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetTimelineOffset":::

Beachten Sie: Wird der Offsetwert eines Players einer negativen Wiedergabeposition zugeordnet, wird der Clip angehalten, bis der Offset den Wert Null erreicht. Anschließend beginnt die Wiedergabe. Wenn der Offsetwert einer Wiedergabeposition zugeordnet ist, welche die Dauer des Medientitels überschreitet, wird entsprechend das letzte Bild angezeigt. Dies entspricht dem Vorgehen, wenn ein einzelner Media Player das Ende der Inhaltswiedergabe erreicht.

## <a name="play-spherical-video-with-mediaplayer"></a>Abspielen von sphärischen Videos mit Media Player
Ab Windows 10, Version 1703, unterstützt **Media Player** die equirecht eckige Projektion für die Wiedergabe von kugelförmigen Videos. Der Inhalt von kugelförmigen Videos unterscheidet sich nicht von regulärem, flatvideo, da **Media Player** das Video so lange Rendering wird, wie die Videocodierung unterstützt wird. Bei einem sphärischen Video, das ein Metadatentag enthält, das angibt, dass das Video eine equirecht eckige Projektion verwendet, kann **Media Player** das Video mithilfe eines angegebenen Felds von Ansicht und Anzeige Ausrichtung Rendering. Dies ermöglicht Szenarien wie die Videowiedergabe von Virtual Reality mit einer am Anfang eingebundenen Anzeige oder die einfache Navigation innerhalb von sphärischen Videoinhalten mithilfe der Maus-oder Tastatureingaben.

Zum Wiedergeben von sphärischen Videos führen Sie die Schritte zum Wiedergeben von Videoinhalten aus, die zuvor in diesem Artikel beschrieben wurden. Der einzige zusätzliche Schritt ist die Registrierung eines Handlers für das [**Media Player. mediageöffnete**](/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened) -Ereignis. Dieses Ereignis bietet Ihnen die Möglichkeit, die Parameter für die sphärischen Videowiedergabe zu aktivieren und zu steuern.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetOpenSphericalVideo":::

Überprüfen Sie im **mediageöffneten** -Handler zuerst das Frame Format des neu geöffneten Medien Elements, indem Sie die Eigenschaft [**playbacksession. sphericalvideoprojection. frameformat**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat) überprüfen. Wenn dieser Wert [**sphericavideoframeformat. equirecht eckig**](/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat)ist, kann das System den Videoinhalt automatisch projizieren. Legen Sie zunächst die Eigenschaft [**playbacksession. sphericalvideoprojection. isaktiviauf**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) **true** fest. Sie können auch Eigenschaften anpassen, z. b. die Ansichts Ausrichtung und das Sichtfeld, die der Media Player zum Projizieren des Video Inhalts verwendet. In diesem Beispiel wird das Feld der Ansicht auf einen Breitenwert von 120 Grad festgelegt, indem die [**horizontalfieldofviewindegrees**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees) -Eigenschaft festgelegt wird.

Wenn der Videoinhalt kugelförmig ist, aber in einem anderen Format als equirecht eckig vorliegt, können Sie mit dem Frame Server Modus von Media Player einen eigenen Projektions Algorithmus implementieren, um einzelne Frames zu empfangen und zu verarbeiten.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSphericalMediaOpened":::

Der folgende Beispielcode veranschaulicht, wie die Ausrichtung der kugelförmigen Videoansicht mithilfe der nach-links-und nach-rechts-Taste angepasst wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSphericalOnKeyDown":::

Wenn Ihre APP Wiedergabelisten von Videos unterstützt, möchten Sie möglicherweise Wiedergabe Elemente identifizieren, die das sphärischen Video in der Benutzeroberfläche enthalten. Medienwiedergabe Listen werden im Artikel [Medienelemente, Wiedergabelisten und Spuren](media-playback-with-mediasource.md)ausführlich erläutert. Das folgende Beispiel zeigt das Erstellen einer neuen Wiedergabeliste, das Hinzufügen eines Elements und das Registrieren eines Handlers für das [**mediaplaybackitem. videotrackschangi-**](/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged) Ereignis, das auftritt, wenn die Videotitel für ein Medien Element aufgelöst werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSphericalList":::

Rufen Sie im **videotrackschge** -Ereignishandler die Codierungs Eigenschaften für alle hinzugefügten Videotitel durch Aufrufen von [**Videotrack. getencodingproperties**](/uwp/api/windows.media.core.videotrack.GetEncodingProperties)ab. Wenn die [**sphericalvideoframeformat**](/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) -Eigenschaft der Codierungs Eigenschaften ein anderer Wert als [**sphericavideoframeformat. None**](/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat)ist, enthält die Videospur das sphärischen Video, und Sie können die Benutzeroberfläche entsprechend aktualisieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSphericalTracksChanged":::

## <a name="use-mediaplayer-in-frame-server-mode"></a>Verwenden von Media Player im Frame Server Modus
Ab Windows 10, Version 1703, können Sie **Media Player** im Frame Server Modus verwenden. In diesem Modus renbt **Media Player** keine Frames automatisch zu einem zugeordneten **mediaplayerelement**. Stattdessen kopiert Ihre APP den aktuellen Frame vom **Media Player** in ein Objekt, das [**IDirect3DSurface**](/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface)implementiert. Das primäre Szenario, das diese Funktion ermöglicht, besteht in der Verwendung von Pixel-Shadern, um von **Media Player** bereitgestellte Video Frames zu verarbeiten Ihre APP ist für die Anzeige der einzelnen Frames nach der Verarbeitung zuständig, z. b. durch Anzeigen des Frames in einem XAML- [**Bild**](/uwp/api/windows.ui.xaml.controls.image) -Steuerelement.

Im folgenden Beispiel wird ein neuer **Media Player** initialisiert, und Videoinhalt wird geladen. Als nächstes wird ein Handler für [**videoframeavailable**](/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable) registriert. Der Frame Server Modus wird aktiviert, indem die [**isvideoframeserveraktivierte**](/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) Eigenschaft des **Media Player** -Objekts auf " **true**" festgelegt wird. Zum Schluss wird die Medienwiedergabe mit einem [**Play**](/uwp/api/windows.media.playback.mediaplayer.Play)-Befehl gestartet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetFrameServerInit":::

Das nächste Beispiel zeigt einen Handler für **videoframeavailable** , der [Win2D](https://github.com/Microsoft/Win2D) verwendet, um jedem Frame eines Videos einen einfachen Weichzeichnereffekt hinzuzufügen, und dann die verarbeiteten Frames in einem XAML- [Bild](/uwp/api/windows.ui.xaml.controls.image) Steuerelement anzeigt.

Wenn der **videoframeavailable** -Handler aufgerufen wird, wird die [**copyframetovideosurface**](/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface) -Methode verwendet, um den Inhalt des Frames in ein [**IDirect3DSurface**](/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface)zu kopieren. Sie können auch [**copyframedestereoscopicvideo-Oberflächen**](/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) verwenden, um 3D-Inhalte in zwei Oberflächen zu kopieren, um den linken und den rechten Inhalt separat zu verarbeiten. Um ein Objekt zu erhalten, das implementiert **IDirect3DSurface**  in diesem Beispiel wird eine [**softwardaemmap**](/uwp/api/windows.graphics.imaging.softwarebitmap) erstellt. Anschließend wird dieses Objekt verwendet, um eine Win2D **canvasbitmap** zu erstellen, die die erforderliche Schnittstelle implementiert. Eine **canvasimagesource** ist ein Win2D-Objekt, das als Quelle für ein **Image** -Steuerelement verwendet werden kann, sodass ein neues erstellt und als Quelle für das **Bild** festgelegt wird, in dem der Inhalt angezeigt wird. Als nächstes wird eine **canvasdrawingsession** erstellt. Diese wird von Win2D verwendet, um den Weichzeichnereffekt zu erzeugen.

Nachdem alle erforderlichen Objekte instanziiert wurden, wird **copyframeumvideosurface** aufgerufen, das den aktuellen Frame aus **Media Player** in die **canvasbitmap** kopiert. Als nächstes wird ein Win2D **gausianblureffect** erstellt, wobei die **canvasbitmap** als Quelle des Vorgangs festgelegt ist. Schließlich wird " **canvasdrawingsession. DrawImage** " aufgerufen, um das Quell Bild, bei dem der weich Zieh Effekt angewendet wurde, in " **canvasimagesource** " zu zeichnen, das dem **Image** -Steuerelement zugeordnet ist, was dazu führt, dass es in der Benutzeroberfläche gezeichnet wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetVideoFrameAvailable":::

Weitere Informationen zu Win2D finden Sie im [Win2D GitHub-Repository](https://github.com/Microsoft/Win2D). Wenn Sie den oben gezeigten Beispielcode ausprobieren möchten, müssen Sie das nuget-Paket Win2D mit den folgenden Anweisungen zu Ihrem Projekt hinzufügen.

**So fügen Sie das Win2D-NuGet-Paket zum Effektprojekt hinzu**

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2.  Wählen Sie oben im Fenster die Registerkarte **Durchsuchen** aus.
3.  Geben Sie im Suchfeld **Win2D** ein.
4.  Wählen Sie **Win2D.uwp** und anschließend im rechten Bereich **Installieren** aus.
5.  Im Dialogfeld **Änderungen überprüfen** wird das zu installierende Paket angezeigt. Klicken Sie auf **OK**.
6.  Akzeptieren Sie die Paketlizenz.

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Erkennen und reagieren auf audioleveländerungen durch das System
Ab Windows 10, Version 1803, kann Ihre APP erkennen, wenn das System die Audioebene eines gerade wiedergegebenen **Media Player**-oder-Mutes absinkt. Beispielsweise kann das System die Audiowiedergabe Ebene verringern oder "Enten", wenn ein Alarm klingelt. Das System wird Ihre APP stumm schalten, wenn Sie in den Hintergrund wechselt, wenn Ihre APP die *backgroundmediaplayback* -Funktion nicht im App-Manifest deklariert hat. Die [**audiostatuemonitor**](/uwp/api/windows.media.audio.audiostatemonitor) -Klasse ermöglicht es Ihnen, sich für den Empfang eines Ereignisses zu registrieren, wenn das System das Volume eines Audiodatenstroms ändert. Greifen Sie auf die **audiostatuemonitor** -Eigenschaft eines **Media Player** zu, und registrieren Sie einen Handler für das [**soundlevelchanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) -Ereignis, um benachrichtigt zu werden, wenn die Audioebene für den **Media Player** vom System geändert wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterAudioStateMonitor":::

Wenn Sie das **Sound levelchanged** -Ereignis verarbeiten, können Sie je nach Art der wiedergegebenen Inhalte verschiedene Aktionen ausführen. Wenn Sie derzeit Musik spielen, sollten Sie die Musik wiedergeben lassen, während das Volume geduckt ist. Wenn Sie jedoch einen Podcast spielen, möchten Sie die Wiedergabe wahrscheinlich anhalten, während das Audiogerät geduckt wird, damit der Benutzer den Inhalt nicht verpasst.

In diesem Beispiel wird eine Variable deklariert, um zu verfolgen, ob es sich bei dem aktuell wiedergegebenen Inhalt um einen Podcast handelt. es wird davon ausgegangen, dass Sie diesen Wert beim Auswählen des **Media Player**-Inhalts auf den entsprechenden Wert festlegen Außerdem wird eine Klassen Variable erstellt, die nachverfolgt werden soll, wenn die Wiedergabe beim Ändern der Audioebene Programm gesteuert angehalten wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetAudioStateVars":::

Überprüfen Sie im Ereignishandler von **soundlevelchanged** die Eigenschaft [**Sound Level**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) des Absenders **Audiostatus-itor** , um die neue Audioebene zu bestimmen. In diesem Beispiel wird überprüft, ob die neue Audioebene ein vollständiges Volume ist, d. h., das System hat das Volume beendet oder das Volume duckt, oder ob die Audiostufe gesenkt wurde, aber nicht-Podcast-Inhalte wieder gibt. Wenn eine der beiden Optionen true ist und der Inhalt zuvor Programm gesteuert angehalten wurde, wird die Wiedergabe fortgesetzt. Wenn die neue Audioebene stumm geschaltet wird oder wenn es sich bei dem aktuellen Inhalt um einen Podcast handelt und die Audioebene niedrig ist, wird die Wiedergabe angehalten, und die Variable wird festgelegt, um zu verfolgen, ob die Pause Programm gesteuert initiiert wurde.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSoundLevelChanged":::

Der Benutzer kann festlegen, dass die Wiedergabe angehalten oder fortgesetzt werden soll, auch wenn das Audiogerät vom System geduckt ist. In diesem Beispiel werden Ereignishandler für eine Wiedergabe-und eine Pause-Schaltfläche angezeigt. Klicken Sie in der Schaltfläche Anhalten auf angehalten, wenn die Wiedergabe bereits Programm gesteuert angehalten wurde, und aktualisieren Sie die Variable, um anzugeben, dass der Inhalt vom Benutzer angehalten wurde. Im Click-Handler der Wiedergabe Taste setzen wir die Wiedergabe fort und löschen die nach Verfolgungs Variable.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetButtonUserClick":::

## <a name="related-topics"></a>Zugehörige Themen
* [Medienwiedergabe](media-playback.md)
* [Medienelemente, Wiedergabelisten und Spuren](media-playback-with-mediasource.md)
* [Integrieren in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md)
* [Erstellen, Planen und Verwalten von Medienunterbrechungen](create-schedule-and-manage-media-breaks.md)
* [Wiedergeben von Medien im Hintergrund](background-audio.md)





 

 
