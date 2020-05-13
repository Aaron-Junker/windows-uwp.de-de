---
description: In diesem Artikel erfahren Sie, wie Sie mit audioplaybackconnection über Bluetooth verbundene Remote Geräte für die Wiedergabe von Audiodaten auf dem lokalen Computer aktivieren.
title: Aktivieren der Audiowiedergabe auf Geräten mit Remote Verbindung mit Bluetooth
ms.date: 05/03/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f03fc963e533ff29d49c326611c45437baa14f6c
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234953"
---
# <a name="enable-audio-playback-from-remote-bluetooth-connected-devices"></a>Aktivieren der Audiowiedergabe auf Geräten mit Remote Verbindung mit Bluetooth

In diesem Artikel erfahren Sie, wie Sie mit [audioplaybackconnection](/uwp/api/windows.media.audio.audioplaybackconnection) über Bluetooth verbundene Remote Geräte für die Wiedergabe von Audiodaten auf dem lokalen Computer aktivieren.

Ab Windows 10 können remoteaudioquellen der Version 2004 Audiodaten an Windows-Geräte streamen. so können Szenarien wie z. b. das Konfigurieren eines PCs wie ein Bluetooth-Redner Aussehen und Benutzern das hören von Audiodaten auf Ihrem Telefon ermöglicht werden. Die-Implementierung verwendet die Bluetooth-Komponenten im Betriebssystem, um eingehende Audiodaten zu verarbeiten und Sie auf den audioendpunkten des Systems auf dem System, wie z. b. integrierte PC-Sprecher oder kabelgebundene Kopfhörer, wiederzugeben. Die Aktivierung der zugrunde liegenden Bluetooth-A2DP-Senke wird von apps verwaltet, die für das Endbenutzer Szenario verantwortlich sind, und nicht durch das System.

Die [audioplaybackconnection](/uwp/api/windows.media.audio.audioplaybackconnection) -Klasse wird verwendet, um Verbindungen von einem Remote Gerät zu aktivieren und zu deaktivieren sowie um die Verbindung zu erstellen, sodass die Remote-Audiowiedergabe gestartet werden kann.

## <a name="add-a-user-interface"></a>Hinzufügen einer Benutzeroberfläche

In den Beispielen in diesem Artikel verwenden wir die folgende einfache XAML-Benutzeroberfläche, die das **ListView** -Steuerelement zum Anzeigen verfügbarer Remote Geräte, einen **TextBlock** zum Anzeigen des Verbindungsstatus und drei Schaltflächen zum Aktivieren, deaktivieren und Öffnen von Verbindungen definiert.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml" id="snippet_AudioPlaybackConnectionXAML":::

## <a name="use-devicewatcher-to-monitor-for-remote-devices"></a>Verwenden von devicewatcher zum Überwachen von Remote Geräten

Die [devicewatcher](/uwp/api/windows.devices.enumeration.devicewatcher) -Klasse ermöglicht es Ihnen, verbundene Geräte zu erkennen. Die [audioplaybackconnection. getdeviceselector](/uwp/api/windows.media.audio.audioplaybackconnection.getdeviceselector) -Methode gibt eine Zeichenfolge zurück, die den Geräte-Watcher mitteilt, welche Arten von Geräten überwacht werden sollen. Übergeben Sie diese Zeichenfolge an den **devicewatcher** -Konstruktor. 

Das Ereignis [devicewatcher. Added](/uwp/api/windows.devices.enumeration.devicewatcher.added) wird für jedes Gerät ausgelöst, das verbunden ist, wenn der Geräte-Watcher gestartet wird, sowie für alle Geräte, die verbunden sind, während der Geräte-Watcher ausgeführt wird. Das [devicewatcher. removed](/uwp/api/windows.devices.enumeration.devicewatcher.removed) -Ereignis wird ausgelöst, wenn ein zuvor verbundenes Gerät die Verbindung trennt. 

Wenden Sie sich an [devicewatcher. Start](/uwp/api/windows.devices.enumeration.devicewatcher.start) , um mit der Überwachung auf verbundene Geräte zu beginnen, die Audiowiedergabe Verbindungen unterstützen In diesem Beispiel starten wir den Geräte-Manager, wenn das Haupt **Raster** -Steuerelement in der Benutzeroberfläche geladen wird. Weitere Informationen zur Verwendung von **devicewatcher**finden Sie unter [Aufzählen von Geräten](/windows/uwp/devices-sensors/enumerate-devices).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_MainGridLoaded":::


Im **hinzugefügten** Ereignis des Device Watcher wird jedes ermittelte Gerät durch ein Objekt vom Typ " [de viceinformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) " dargestellt. Fügen Sie jedes erkannte Gerät einer Observable-Auflistung hinzu, die an das **ListView** -Steuerelement in der Benutzeroberfläche gebunden ist.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeclareDevices":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeviceWatcher_Added":::


## <a name="enable-and-release-audio-playback-connections"></a>Aktivieren und Freigeben von Audiowiedergabe-Verbindungen

Vor dem Öffnen einer Verbindung mit einem Gerät muss die Verbindung aktiviert sein. Dadurch wird dem System mitgeteilt, dass eine neue Anwendung vorhanden ist, bei der Audiodaten vom Remote Gerät auf dem PC abgespielt werden sollen, aber die Audiowiedergabe wird erst wieder abgespielt, wenn die Verbindung geöffnet ist. Dies wird in einem späteren Schritt gezeigt.

Wählen Sie im Click-Handler für die Schaltfläche **Audiowiedergabe-Verbindung aktivieren** die Geräte-ID aus, die dem aktuell ausgewählten Gerät im **ListView** -Steuerelement zugeordnet ist. In diesem Beispiel wird ein Wörterbuch mit aktivierten **audioplaybackconnection** -Objekten verwaltet. Diese Methode prüft zunächst, ob für das ausgewählte Gerät bereits ein Eintrag im Wörterbuch vorhanden ist. Als nächstes versucht die Methode, eine **audioplaybackconnection** für das ausgewählte Gerät zu erstellen, indem Sie [trycreatefromid](/uwp/api/windows.media.audio.audioplaybackconnection.trycreatefromid) aufruft und die ausgewählte Geräte-ID übergibt. 

Wenn die Verbindung erfolgreich hergestellt wurde, fügen Sie das neue **audioplaybackconnection** -Objekt dem Wörterbuch der APP hinzu, registrieren Sie einen Handler für das [StateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) -Ereignis des Objekts, und starten Sie[startasync](/uwp/api/windows.media.audio.audioplaybackconnection.startasync) , um das System zu benachrichtigen, dass die neue Verbindung aktiviert ist. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeclareConnections":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_EnableAudioPlaybackConnection":::


## <a name="open-the-audio-playback-connection"></a>Öffnen der Verbindung mit der Audiowiedergabe

Im vorherigen Schritt wurde eine Audiowiedergabe Verbindung erstellt, aber Sound wird erst wieder abgespielt, wenn die Verbindung durch Aufrufen von " [Open](/uwp/api/windows.media.audio.audioplaybackconnection.open) " oder " [openasync](/uwp/api/windows.media.audio.audioplaybackconnection.openasync)" geöffnet wurde. Rufen Sie in der Schaltfläche zum **Öffnen der Audiowiedergabe-Verbindung** das aktuell ausgewählte Gerät ab, und verwenden Sie die ID, um die **audioplaybackconnection** aus dem Verbindungs Wörterbuch der APP abzurufen. Warten Sie auf einen **openasync** -aufrufenden, und überprüfen Sie den **Status** Wert des zurückgegebenen [audioplaybackconnectionopenresultstatus](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult) -Objekts, um festzustellen, ob die Verbindung erfolgreich geöffnet wurde, und aktualisieren Sie ggf. das Textfeld Verbindungsstatus.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_OpenAudioPlaybackConnectionButton":::

## <a name="monitor-audio-playback-connection-state"></a>Verbindungsstatus der Audiowiedergabe überwachen

Das [audioplaybackconnection. connectionstatechanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) -Ereignis wird immer dann ausgelöst, wenn sich der Status der Verbindung ändert. In diesem Beispiel aktualisiert der Handler für dieses Ereignis das Textfeld Status. Denken Sie daran, die Benutzeroberfläche innerhalb eines Aufrufers " [Dispatcher. runasync](/uwp/api/windows.ui.core.coredispatcher.runasync) " zu aktualisieren, um sicherzustellen, dass das Update im UI-Thread erfolgt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_ConnectionStateChanged":::

## <a name="release-connections-and-handle-removed-devices"></a>Freigeben von Verbindungen und behandeln entfernter Geräte

Dieses Beispiel stellt eine Verbindungs Schaltfläche für die **releaseaudiowiedergabe** bereit, damit der Benutzer eine Audiowiedergabe Verbindung freigeben kann. Im Handler für dieses Ereignis wird das aktuell ausgewählte Gerät angezeigt, und die Geräte-ID wird verwendet, um die **audioplaybackconnection** im Wörterbuch zu suchen. Geben **Sie verwerfen** ein, um den Verweis freizugeben und alle zugeordneten Ressourcen freizugeben und die Verbindung aus dem Wörterbuch zu entfernen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_ReleaseAudioPlaybackConnectionButton":::

Sie sollten den Fall behandeln, in dem ein Gerät entfernt wird, während eine Verbindung aktiviert oder geöffnet wird. Implementieren Sie zu diesem Zweck einen Handler für das [devicewatcher. entfernten](/uwp/api/windows.devices.enumeration.devicewatcher.removed) -Ereignis von Device Watcher. Zuerst wird die ID des entfernten Geräts verwendet, um das Gerät aus der Observable-Sammlung zu entfernen, die an das **ListView** -Steuerelement der APP gebunden ist. Wenn sich eine Verbindung, die mit diesem Gerät verknüpft ist, im Wörterbuch der APP **befindet, wird verwerfen aufgerufen, um** die zugeordneten Ressourcen freizugeben, und dann wird die Verbindung aus dem Wörterbuch entfernt. All dies erfolgt innerhalb eines Aufrufes in **Dispatcher. runasync** , um sicherzustellen, dass die Aktualisierungen der Benutzeroberfläche im UI-Thread ausgeführt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeviceWatcher_Removed":::

## <a name="related-topics"></a>Zugehörige Themen

[Medienwiedergabe](media-playback.md)


 




