---
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: In diesem Artikel wird erläutert, wie UWP-apps vom System initiierte Änderungen in audiostreamstufen erkennen und darauf reagieren können.
title: Erkennen von und Reagieren auf Audio-Zustandsänderungen
ms.date: 04/03/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 84dbafa95f0151f8f8774d00ceae4b09b6ec5e88
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170514"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>Erkennen von und Reagieren auf Audio-Zustandsänderungen
Ab Windows 10, Version 1803, kann Ihre APP erkennen, wann die Audioebene eines Audiostreams, der von Ihrer APP verwendet wird, von der APP gesenkt oder nicht erkannt wird. Sie können Benachrichtigungen für Erfassungs-und Rendering-Streams für ein bestimmtes Audiogerät und eine audiokategorie oder für ein [**Media Player**](/uwp/api/Windows.Media.Playback.MediaPlayer) -Objekt empfangen, das Ihre APP für die Medienwiedergabe verwendet. Beispielsweise kann das System die Audiowiedergabe Ebene verringern oder "Enten", wenn ein Alarm klingelt. Das System wird Ihre APP stumm schalten, wenn Sie in den Hintergrund wechselt, wenn Ihre APP die *backgroundmediaplayback* -Funktion nicht im App-Manifest deklariert hat. 

Das Muster für die Handhabung von Änderungen am audiozustand ist für alle unterstützten Audiostreams identisch. Erstellen Sie zunächst eine Instanz der [**audiostatuemonitor**](/uwp/api/windows.media.audio.audiostatemonitor) -Klasse. Im folgenden Beispiel verwendet die APP die [**mediacapture**](/uwp/api/Windows.Media.Capture.MediaCapture) -Klasse, um Audiodaten für Game Chats aufzuzeichnen. Eine Factorymethode wird aufgerufen, um einen audiostatusmonitor zu erhalten, der dem Game Chat-audioerfassungs-Stream des Standard kommunikationsgeräts zugeordnet ist.  Als nächstes wird ein Handler für das [**soundlevelchanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) -Ereignis registriert, das ausgelöst wird, wenn die Audioebene für den zugeordneten Stream vom System geändert wird.

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

Überprüfen Sie im **Sound Level Changed** -Ereignishandler die Eigenschaft [**Sound Level**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) des an den Handler übergebenen **Audiostatus-itor** -Absenders, um zu bestimmen, welche neue Audioebene für den Stream festgelegt ist. In diesem Beispiel stoppt die APP das Erfassen von Audio, wenn die Audioebene stumm geschaltet wird, und setzt die Erfassung fort, wenn die Audioebene zum vollständigen Volume zurückkehrt.

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

Weitere Informationen zum Erfassen von Audiodaten mit **mediacapture**finden Sie unter [Grundlegendes Foto, Video und Audioerfassung mit mediacapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

Jeder Instanz der [**Media Player**](/uwp/api/Windows.Media.Playback.MediaPlayer) -Klasse ist ein **Audiostatus-itor** zugeordnet, mit dem Sie erkennen können, wann das System die Volumeebene des aktuell wiedergegebenen Inhalts ändert. Je nachdem, welche Art von Inhalt wiedergegeben wird, können Sie die Änderungen am audiozustand unterschiedlich behandeln. Beispielsweise können Sie die Wiedergabe eines Podcasts anhalten, wenn die Audiodaten gesenkt werden, aber die Wiedergabe fortsetzen, wenn der Inhalt Musik ist. 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

Weitere Informationen zur Verwendung des **MediaPlayer** finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

## <a name="related-topics"></a>Zugehörige Themen