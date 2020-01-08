---
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: In diesem Artikel wird erläutert, wie UWP-Apps vom System initiierte Änderungen der Audiodatenstromebene erkennen und darauf reagieren können
title: Erkennen von und Reagieren auf Audio-Zustandsänderungen
ms.date: 04/03/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c6c5832b479fedc8d2f14e53cdbaccf179358c4d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683893"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>Erkennen von und Reagieren auf Audio-Zustandsänderungen
Ab Windows 10, Version 1803, erkennt Ihre App, wenn das System die Lautstärke der Audioaufnahme Ihrer App oder des Audiodatenstroms reduziert oder stummschaltet. Sie können Benachrichtigungen für die Erfassung und die Wiedergabe von Datenströmen erhalten, für ein bestimmtes Audio-Gerät und eine Audiokategorie oder für eine [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer)-Objekt, das ist Ihrer App für die Medienwiedergabe verwendet. Beispielsweise kann das System die Audiowiedergabe-Ebene reduzieren, wenn ein Alarm klingelt. Das System schaltet Ihre App stumm, wenn sie in den Hintergrund wechselt, falls Ihre App die *BackgroundMediaPlayback*-Funktion im App-Manifest nicht aktiviert hat. 

Das Muster für die Behandlung von Audio-Zustandsänderungen ist für alle unterstützten Audiostreams identisch. Erstellen Sie zuerst eine Instanz der [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor)-Klasse. Im folgenden Beispiel verwendet die App die [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)-Klasse zum Aufnehmen von Audio für Spiele-Chats. Es wird eine Factorymethode aufgerufen, um den Audiostatus-Monitor für den Spiel-Chat-Auidioaufnahmedatenstrom des standardmäßigen Kommunikationsgeräts zugeordnet zu erhalten.  Danach wird ein Handler für das [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged)-Ereignis registriert, das ausgelöst wird, wenn die Audiowiedergabe für die entsprechende Streamkategorie vom System geändert wird.

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

Überprüfen Sie im **Sound Level Changed** -Ereignishandler die Eigenschaft [**Sound Level**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) des an den Handler übergebenen **Audiostatus-itor** -Absenders, um zu bestimmen, welche neue Audioebene für den Stream festgelegt ist. In diesem Beispiel wird die Aufzeichnung von Audio von der App beendet, wenn die Lautstärke stumm geschaltet ist, und fortgesetzt, wenn die volle Lautstärke wieder eingeschaltet wird.

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

Weitere Informationen über die Aufnahme von Audio mit **MediaCapture** finden Sie unter [Allgemeine Foto-, Video- und Audioaufnahme mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

Jede Instanz der [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer)-Klasse verfügt über einen zugeordneten **AudioStateMonitor**, den Sie verwenden können, um festzustellen, ob das System die Lautstärke des aktuell wiedergegebenen Inhalts ändert. Sie können auch entscheiden, Audio-Zustandsänderungen unterschiedlich zu behandeln, je nach Art der wiedergegeben Inhalte. Möglicherweise möchten Sie z. B. die Wiedergabe eines Podcasts anhalten, wenn die Audiowiedergabe leiser ist, und die Wiedergabe fortsetzen, wenn der Inhalt Musik ist. 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

Weitere Informationen zur Verwendung des **MediaPlayer** finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

## <a name="related-topics"></a>Zugehörige Themen