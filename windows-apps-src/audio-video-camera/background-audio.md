---
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: In diesem Artikel wird erläutert, wie die Medienwiedergabe erfolgt, während die App im Hintergrund ausgeführt wird.
title: Wiedergeben von Medien im Hintergrund
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 72687db5bed8303b672ed8ed009108708cb126be
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161184"
---
# <a name="play-media-in-the-background"></a>Wiedergeben von Medien im Hintergrund
In diesem Artikel wird beschrieben, wie Sie Ihre App so konfigurieren, dass die Medienwiedergabe fortgesetzt wird, wenn die App vom Vordergrund in den Hintergrund wechselt. Dies bedeutet, dass die App weiterhin Audio wiedergeben kann, auch nachdem der Benutzer die App minimiert hat, zur Startseite zurückgekehrt ist oder die App auf andere Weise verlassen hat. 

Beispiele für Szenarien mit der Wiedergabe von Hintergrundaudio:

-   **Langzeit-Wiedergabelisten** Der Benutzer ruft kurz eine Vordergrund-App auf, um eine Wiedergabeliste auszuwählen und zu starten, und erwartet anschließend, dass die Wiedergabeliste kontinuierlich im Hintergrund wiedergegeben wird.

-   **Verwenden der Aufgabenumschaltfunktion:** Der Benutzer ruft kurz eine Vordergrund-App auf, um die Audiowiedergabe zu starten, und wechselt mit der Aufgabenumschaltfunktion dann zu einer anderen geöffneten App. Der Benutzer erwartet, dass die Audiowiedergabe im Hintergrund fortgesetzt wird.

Dank der in diesem Artikel beschriebenen Implementierung von Hintergrundaudio kann die App universell auf allen Windows-Geräten, einschließlich Mobile, Desktop und Xbox, ausgeführt werden.

> [!NOTE]
> Der Code in diesem Artikel wurde aus dem UWP-Beispiel für [Hintergrundaudio](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback) übernommen und angepasst.

## <a name="explanation-of-one-process-model"></a>Erläuterung des Einzelprozessmodells
Mit Windows 10, Version 1607, wurde ein neues Einzelprozessmodell eingeführt, das die Aktivierung von Hintergrundaudio erheblich vereinfacht. Zuvor musste Ihre App zusätzlich zur Vordergrund-App einen Hintergrundprozess verwalten und die Statusänderungen zwischen den beiden Prozessen manuell kommunizieren. Beim neuen Modell fügen Sie Ihrem App-Manifest einfach die Hintergrundaudio-Funktion hinzu, damit die Audiowiedergabe automatisch fortgesetzt wird, wenn die App in den Hintergrund wechselt. Durch die beiden neuen App-Lebenszyklusereignisse [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) und [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) weiß Ihre App, wann sie in den Hintergrund wechselt bzw. den Hintergrund wieder verlässt. Beim Übergang der App in den bzw. aus dem Hintergrund können sich die vom System erzwungenen Speicherbeschränkungen ändern. Sie können diese Ereignisse also verwenden, um die aktuelle Arbeitsspeichernutzung zu überprüfen und Ressourcen freizugeben, damit Sie unter dem Grenzwert bleiben.

Durch den Wegfall der komplexen prozessübergreifenden Kommunikation und Zustandsverwaltung ermöglicht Ihnen das neue Modell, Hintergrundaudio mit deutlich geringerem Programmieraufwand viel schneller zu implementieren. Aus Gründen der Abwärtskompatibilität wird das aus zwei Prozessen bestehende Modell in der aktuellen Version jedoch weiterhin unterstützt. Weitere Informationen finden Sie unter [Hintergrundaudio-Modell (Legacy)](legacy-background-media-playback.md).

## <a name="requirements-for-background-audio"></a>Anforderungen für Hintergrundaudio
Ihre App muss die folgenden Anforderungen für die Audiowiedergabe erfüllen, während sie sich im Hintergrund befindet.

* Fügen Sie Ihrem App-Manifest die Funktion **Medienwiedergabe im Hintergrund** wie weiter unten in diesem Artikel beschrieben hinzu.
* Wenn die automatische Integration von **MediaPlayer** in die Steuerelemente für den Systemmedientransport (System Media Transport Controls, SMTC) von der App deaktiviert wird (z. B. durch Festlegen der [**CommandManager.IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled)-Eigenschaft auf FALSE), müssen Sie die manuelle Integration in SMTC implementieren, um die Medienwiedergabe im Hintergrund zu aktivieren. Die manuelle Integration in SMTC ist auch erforderlich, wenn Sie eine andere API als **MediaPlayer** (z. B. [**AudioGraph**](/uwp/api/Windows.Media.Audio.AudioGraph)) verwenden, um die Audiowiedergabe beim Wechsel der App in den Hintergrund fortzusetzen. Die Mindestanforderungen für die SMTC-Integration sind im Abschnitt „Verwenden der Steuerelemente für den Systemmedientransport für Audiowiedergabe im Hintergrund“ von [Manuelle Steuerung der Steuerelemente für den Systemmedientransport](system-media-transport-controls.md) beschrieben.
* Während sich Ihre App im Hintergrund befindet, müssen Sie unterhalb der Grenzwerte für die Speicherauslastung bleiben, die vom System für Hintergrund-Apps festgelegt wurden. Eine Anleitung zur Verwaltung des Arbeitsspeichers, während die App im Hintergrund ausgeführt wird, finden Sie später in diesem Artikel.

## <a name="background-media-playback-manifest-capability"></a>Manifestfunktion „Medienwiedergabe im Hintergrund“
Wenn Sie Hintergrundaudio aktivieren möchten, müssen Sie der App-Manifestdatei „Package.appxmanifest“ die Funktion „Medienwiedergabe im Hintergrund“ hinzufügen. 

**So fügen Sie dem App-Manifest mithilfe des Manifest-Designers Funktionen hinzu**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie das Kontrollkästchen **Medienwiedergabe im Hintergrund**.

Wenn Sie die Funktion festlegen möchten, indem Sie die XML-Datei des App-Manifests manuell bearbeiten, müssen Sie zunächst sicherstellen, dass das Namespacepräfix *uap3* im **Package**-Element definiert ist. Wenn dies nicht der Fall ist, fügen Sie es gemäß den folgenden Schritten hinzu.
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

Als Nächstes fügen Sie dem **Capabilities**-Element die *backgroundMediaPlayback*-Funktion hinzu:
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

## <a name="handle-transitioning-between-foreground-and-background"></a>Verarbeiten der Übergänge zwischen Vordergrund und Hintergrund
Wenn Ihre App vom Vordergrund in den Hintergrund wechselt, wird das [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground)-Ereignis ausgelöst. Wechselt die App dann wieder in den Vordergrund, wird das [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)-Ereignis ausgelöst. Da es sich dabei um App-Lebenszyklusereignisse handelt, sollten Sie bei der App-Erstellung Handler für diese Ereignisse registrieren. In der Standardprojektvorlage wird der Handler dem **App**-Klassenkonstruktor in „App.xaml.cs“ hinzugefügt. 

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

Erstellen Sie eine Variable, um nachzuverfolgen, ob die App momentan im Hintergrund ausgeführt wird.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

Wenn das [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground)-Ereignis ausgelöst wird, legen Sie die Nachverfolgungsvariable fest, um anzugeben, dass die App momentan im Hintergrund ausgeführt wird. Langzeitaufgaben sollten im **EnteredBackground**-Ereignis nicht ausgeführt werden, da der Übergang in den Hintergrund dem Benutzer dadurch langsam erscheinen könnte.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

Im [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)-Ereignishandler sollten Sie die Nachverfolgungsvariable festlegen, um anzugeben, dass die App nicht mehr im Hintergrund ausgeführt wird.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

### <a name="memory-management-requirements"></a>Anforderungen an die Speicherverwaltung
Den wichtigsten Teil beim Übergang zwischen Vorder- und Hintergrund stellt die Verwaltung des von der App genutzten Speichers dar. Da die Ausführung im Hintergrund die Speicherressourcen verringert, die der App vom System gewährt werden, sollten Sie die App auch für das [**AppMemoryUsageIncreased**](/uwp/api/windows.system.memorymanager.appmemoryusageincreased)-Ereignis und das [**AppMemoryUsageLimitChanging**](/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging)-Ereignis registrieren. Wenn diese Ereignisse ausgelöst werden, sollten Sie die aktuelle Speicherbelegung und den aktuellen Grenzwert Ihrer App überprüfen und die Speichernutzung ggf. reduzieren. Informationen dazu, wie Sie die Speichernutzung während der Ausführung im Hintergrund reduzieren, finden Sie unter [Geben Sie Speicher frei, wenn Ihre App in den Hintergrund verschoben wird](../launch-resume/reduce-memory-usage.md).

## <a name="network-availability-for-background-media-apps"></a>Netzwerkverfügbarkeit für im Hintergrund ausgeführte Medien-Apps
Alle netzwerkfähigen Medienquellen, die nicht auf der Basis eines Datenstroms oder einer Datei erstellt wurden, behalten beim Abrufen von Remoteinhalten eine aktive Netzwerkverbindung bei. Andernfalls wird die Verbindung nicht beibehalten. [Insbesondere bei **MediaStreamSource**](/uwp/api/Windows.Media.Core.MediaStreamSource) kommt es darauf an, dass die Anwendung den richtigen Pufferbereich mithilfe von [**SetBufferedRange**](/uwp/api/windows.media.core.mediastreamsource.setbufferedrange) an die Plattform übergibt. Nachdem der gesamte Inhalt vollständig gepuffert wurde, wird die Netzwerkverbindung nicht mehr für die App reserviert.

Wenn Sie Netzwerk Aufrufe durchführen müssen, die im Hintergrund auftreten, wenn Medien nicht heruntergeladen werden, müssen diese in einer entsprechenden Aufgabe wie [**Maintenance-**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) oder [**time-Auslösers**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)umschließt werden. Weitere Informationen finden [Sie unter unterstützen der APP mit Hintergrundaufgaben](../launch-resume/support-your-app-with-background-tasks.md).

## <a name="related-topics"></a>Zugehörige Themen
* [Medienwiedergabe](media-playback.md)
* [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md)
* [Integration in die Steuerelemente für den Systemmedientransport](integrate-with-systemmediatransportcontrols.md)
* [Hintergrundaudio-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 