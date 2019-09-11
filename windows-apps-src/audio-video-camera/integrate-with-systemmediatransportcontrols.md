---
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: In diesem Artikel wird erläutert, wie Sie mit den Steuerelementen für den Systemmedientransport arbeiten.
title: Integration in die Steuerelemente für den Systemmedientransport
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d2c8e05d2b01b110085ed82c19cecd251c9c6971
ms.sourcegitcommit: c95915f8a13736705eab74951a12b2cf528ea612
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876242"
---
# <a name="integrate-with-the-system-media-transport-controls"></a>Integration in die Steuerelemente für den Systemmedientransport

In diesem Artikel wird erläutert, wie Sie mit den Steuerelementen für den Systemmedientransport (System Media Transport Controls, SMTC) arbeiten. SMTC ist eine Reihe von Steuerelementen, die auf allen Windows 10-Geräten verfügbar ist. Sie bieten den Benutzern eine einheitliche Methode zum Steuern der Wiedergabe von Medien für alle ausgeführten Apps, die Sound über [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) ausgeben.

Ein vollständiges Beispiel, das die Integration in SMTC veranschaulicht, finden Sie in dem [Beispiel für die Steuerelemente für den Systemmedientransport auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls).
                    
## <a name="automatic-integration-with-smtc"></a>Automatische Integration in SMTC
Ab Windows 10, Version 1607, sind UWP-Apps, die die [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse für die Medienwiedergabe verwenden, automatisch in SMTC integriert. Erstellen Sie einfach eine neue Instanz von **MediaPlayer** , und weisen Sie der [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source)-Eigenschaft ein [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource)-, [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem)- oder [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList)-Objekt zu. Dem Benutzer wird der Name Ihrer App in SMTC angezeigt, und er kann mit den Steuerelementen für den Systemmedientransport durch seine Wiedergabelisten navigieren und Titel wiedergeben und anhalten. 

Ihre App kann mehrere **MediaPlayer** Objekte auf einmal erstellen und verwenden. Für jede aktive **MediaPlayer**-Instanz in Ihrer App wird in SMTC eine separaten Registerkarte erstellt, sodass der Benutzer zwischen dem aktiven MediaPlayer und den Instanzen in anderen ausgeführten Apps wechseln kann. Die Steuerelemente bedienen den jeweils ausgewählten MediaPlayer.

Weitere Informationen zur Verwendung von **MediaPlayer** in Ihrer App, u. a. zum Einbinden an ein [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)-Objekt in der XAML-Seite finden Sie unter [Audio-und Videowiedergabe mit MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

Weitere Informationen zum Arbeiten mit **MediaSource**-, **MediaPlaybackItem**- und **MediaPlaybackList**-Objekten finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

## <a name="add-metadata-to-be-displayed-by-the-smtc"></a>Hinzufügen von Metadaten, die von SMTC angezeigt werden sollen
Wenn Sie Metadaten, die für die Medienobjekte in SMTC angezeigt werden, hinzufügen oder ändern möchten z. B. dass ein Video oder Songtitel angezeigt wird, müssen Sie die Anzeigeeigenschaften für das **MediaPlaybackItem**, das Ihr Medienelement darstellt, aktualisieren. Rufen Sie zunächst einen Verweis auf das [**MediaItemDisplayProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaItemDisplayProperties)-Objekt ab, indem Sie [**GetDisplayProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties) aufrufen. Legen Sie dann den Medientyp für das Objekt mit der [**Type**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.type)-Eigenschaft als Musik oder Video fest. Anschließend können Sie Werte in die Felder für die [**MusicProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties)- bzw. [**VideoProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties)-Eigenschaften einfügen, je nachdem, welchen Medientyp Sie angegeben haben. Zum Schluss aktualisieren Sie die Metadaten für das Medienobjekt, indem Sie [**ApplyDisplayProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties) aufrufen.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]


> [!Note]
> Apps sollten einen Wert für die [**Type**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) -Eigenschaft festlegen, auch wenn Sie keine anderen Medien Metadaten bereitstellen, die von den System Media-Transport Steuerelementen angezeigt werden. Dieser Wert hilft dem System dabei, Ihre Medieninhalte ordnungsgemäß zu verarbeiten, einschließlich der Aktivierung des Bildschirmschoners während der Wiedergabe.


## <a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>Verwenden Sie CommandManager, um die Standard-SMTC-Befehle zu ändern oder zu überschreiben.
Ihre App kann das Verhalten der SMTC-Steuerelemente in der [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager)-Klasse ändern oder vollständig überschreiben. Sie können für jede Instanz der **MediaPlayer**-Klasse eine Befehlsmanagerinstanz abrufen, indem Sie auf die [**CommandManager**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.commandmanager)-Eigenschaft zugreifen.

Für jeden Befehl, beispielsweise den Befehl *Next*, der standardmäßig zum nächsten Element in einer **MediaPlaybackList**-Liste springt, veröffentlicht der Befehlsmanager empfangene Ereignisse (z. B. [**NextReceived**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextreceived)) und ein Objekt, das Verhalten des Befehls verwaltet (z. B. [**NextBehavior**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextbehavior)). 

In dem folgenden Beispiel wird ein Handler für das **NextReceived** Ereignis und für das [**IsEnabledChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged)-Ereignis von **NextBehavior** registriert.

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

Das folgende Beispiel illustriert ein Anwendungsszenario, in dem die App den Befehl *Next* deaktiviert, nachdem der Benutzer nacheinander auf fünf Elemente in der Playlist geklickt hat, weil vielleicht ein Benutzereingriff erforderlich ist, bevor weiter Inhalte wiedergegeben werden. Jedes Mal, wenn das **NextReceived**-Ereignis ausgelöst wird, wird ein Zähler erhöht. Wenn der Zähler die angegebene Zahl erreicht, wird die [**EnablingRule**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.enablingrule)-Eigenschaft für den *Next*-Befehl auf [**Never**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaCommandEnablingRule) festgelegt, was den Befehl deaktiviert. 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

Sie können den Befehl auch auf **Always** festlegen, was bedeutet, dass der Befehl auch dann aktiviert ist, wenn bei dem *Next*-Befehl in dem Beispiel keine Elemente mehr in der Playlist vorhanden sind. Alternativ können Sie den Befehl auf **Auto** festlegen. In diesem Fall bestimmt das System auf der Grundlage des aktuell wiedergegebenen Inhalts, ob der Befehl aktiviert werden soll.

Bei dem oben beschriebenen Anwendungsszenario sollte die App zu einem bestimmten Zeitpunkt den *Next*-Befehl wieder aktivieren. Dies erfolgt, indem **EnablingRule** auf **Auto** festgelegt wird.

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

Da Ihre App möglicherweise eine eigene UI zum Steuern der Videowiedergabe hat, wenn sie sich im Vordergrund befindet, können Sie mithilfe der [**IsEnabledChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged)-Ereignisse die eigene Benutzeroberfläche entsprechend den Steuerelemente für den Systemmedientransport aktualisieren, sobald Befehle aktiviert oder deaktiviert werden, indem Sie auf [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabled) von [**MediaPlaybackCommandManagerCommandBehavior**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior) zugreifen, das an den Handler übergeben wird.

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

In einigen Fällen gibt es Gründe, das Verhalten eines Befehls in den Steuerelementen für den Systemmedientransport vollständig zu überschreiben. Das folgende Beispiel illustriert ein Anwendungsszenario, in dem eine App die Befehle *Next* und *Previous* verwendet, um zwischen Internetradiosendern zu wechseln statt zwischen den Titeln der aktuellen Playlist. Wie im vorherigen Beispiel wird ein Handler registriert, wenn ein Befehl empfangen wird. In diesem Fall ist dies das [**PreviousReceived**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.previousreceived)-Ereignis.

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

In dem **PreviousReceived**-Handler wird zuerst ein [**Deferral**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Deferral)-Wert abgerufen, indem die Funktion [**GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.getdeferral) von [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs) aufgerufen wird, das an den Handler übergeben wurde. Dies weist das System an, entsprechend der Verzögerung zu warten, und erst dann den Befehl auszuführen. Dies ist sehr wichtig, wenn Sie beabsichtigen, in dem Ereignishandler asynchrone Aufrufe auszuführen. An dieser Stelle wird im Beispiel eine angepasste Methode aufgerufen, die einen **MediaPlaybackItem**-Wert zurückgibt, der den vorherigen Radiosender darstellt.

Als Nächstes wird die [**Handled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.handled)-Eigenschaft überprüft, um sicherzustellen, dass das Ereignis nicht bereits von einem anderen Handler verarbeitet wird. Wenn dies nicht der Fall ist, wird die Eigenschaft **Handled** auf „true“ festgelegt. Auf diese Weise können die Steuerelemente für den Systemmedientransport und auch alle anderen abonnierten Handler wissen, dass sie keine weiteren Aktionen zum Ausführen dieses Befehls auslösen sollen, da dieser bereits verarbeitet wird. Anschließend wird die neue Quelle für die Medienwiedergabe festgelegt und der Player gestartet.

Schließlich wird für das Deferral-Objekt die Funktion [**Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete) aufgerufen, um dem System mitzuteilen, dass die Verarbeitung des Befehls abgeschlossen wurde.

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                
## <a name="manual-control-of-the-smtc"></a>Manuelle Steuerung von SMTC
Wie weiter oben in diesem Artikel bereits erwähnt, erkennt der SMTC automatisch die Informationen für jede einzelne Instanz von **MediaPlayer**, die Ihre App erstellt, und zeigt diese an. Wenn Sie mehrere Instanzen von **MediaPlayer** verwenden möchten, SMTC jedoch noch einen einzelnen Eintrag für Ihre App bereitstellt, müssen Sie das Verhalten von SMTC manuell steuern anstatt sich auf die automatische Integration zu verlassen. Auch wenn Sie [**MediaTimelineController**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaTimelineController) verwenden, um mindestens ein MediaPlayer zu steuern, müssen Sie die SMTC-Integration manuell durchführen. Dies gilt außerdem, wenn Ihre App ein anderes API als **MediaPlayer** verwendet, um Medien wiederzugeben, z. B. die [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph)-Klasse. Auch in diesem Fall müssen Sie die SMTC-Integration manuell durchführen, damit der Benutzer Ihre App mit den Steuerelementen für den Systemmedientransport bedienen kann. Informationen zur manuellen Bedienung der Steuerelemente für den Systemmedientransport finden Sie unter [Manuelle Steuerung der Steuerelemente für den Systemmedientransport](system-media-transport-controls.md).



## <a name="related-topics"></a>Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Wiedergabe von Audiodateien und Videos mit Media Player](play-audio-and-video-with-mediaplayer.md)
* [Manuelles Steuern der System Media-Transport Steuerelemente](system-media-transport-controls.md)
* [Beispiel für das tranportieren von System Medien auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 




