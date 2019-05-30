---
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: In diesem Artikel wird erläutert, wie die Medienwiedergabe erfolgt, während die App im Hintergrund ausgeführt wird.
title: Wiedergeben von Medien im Hintergrund
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 19b3aa80bee643087a0aa92f714349004f6ec1c1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359160"
---
# <a name="play-media-in-the-background"></a>Wiedergeben von Medien im Hintergrund
In diesem Artikel wird beschrieben, wie Sie Ihre App so konfigurieren, dass die Medienwiedergabe fortgesetzt wird, wenn die App vom Vordergrund in den Hintergrund wechselt. Dies bedeutet, dass die App weiterhin Audio wiedergeben kann, auch nachdem der Benutzer die App minimiert hat, zur Startseite zurückgekehrt ist oder die App auf andere Weise verlassen hat. 

Beispiele für Szenarien mit der Wiedergabe von Hintergrundaudio:

-   **Lang andauernde Wiedergabelisten:** Der Benutzer wird kurz eine Vordergrund-app auswählen und Starten einer Wiedergabeliste, wonach erwartet, dass der Benutzer die Wiedergabeliste aus, um die Wiedergabe im Hintergrund fortgesetzt.

-   **Verwenden die programmumschaltung:** Der Benutzer kurz setzt eine Vordergrund-app um die Wiedergabe von audio, dann wechselt zu einer anderen app, die eine Verbindung mithilfe der programmumschaltung öffnen. Der Benutzer erwartet, dass die Audiowiedergabe im Hintergrund fortgesetzt wird.

Dank der in diesem Artikel beschriebenen Implementierung von Hintergrundaudio kann die App universell auf allen Windows-Geräten, einschließlich Mobile, Desktop und Xbox, ausgeführt werden.

> [!NOTE]
> Der Code in diesem Artikel wurde aus dem UWP-Beispiel für [Hintergrundaudio](https://go.microsoft.com/fwlink/p/?LinkId=800141) übernommen und angepasst.

## <a name="explanation-of-one-process-model"></a>Erläuterung des Einzelprozessmodells
Mit Windows 10, Version 1607, wurde ein neues Einzelprozessmodell eingeführt, das die Aktivierung von Hintergrundaudio erheblich vereinfacht. Zuvor musste Ihre App zusätzlich zur Vordergrund-App einen Hintergrundprozess verwalten und die Statusänderungen zwischen den beiden Prozessen manuell kommunizieren. Beim neuen Modell fügen Sie Ihrem App-Manifest einfach die Hintergrundaudio-Funktion hinzu, damit die Audiowiedergabe automatisch fortgesetzt wird, wenn die App in den Hintergrund wechselt. Durch die beiden neuen App-Lebenszyklusereignisse [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) und [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) weiß Ihre App, wann sie in den Hintergrund wechselt bzw. den Hintergrund wieder verlässt. Beim Übergang der App in den bzw. aus dem Hintergrund können sich die vom System erzwungenen Speicherbeschränkungen ändern. Sie können diese Ereignisse also verwenden, um die aktuelle Arbeitsspeichernutzung zu überprüfen und Ressourcen freizugeben, damit Sie unter dem Grenzwert bleiben.

Durch den Wegfall der komplexen prozessübergreifenden Kommunikation und Zustandsverwaltung ermöglicht Ihnen das neue Modell, Hintergrundaudio mit deutlich geringerem Programmieraufwand viel schneller zu implementieren. Aus Gründen der Abwärtskompatibilität wird das aus zwei Prozessen bestehende Modell in der aktuellen Version jedoch weiterhin unterstützt. Weitere Informationen finden Sie unter [Hintergrundaudio-Modell (Legacy)](legacy-background-media-playback.md).

## <a name="requirements-for-background-audio"></a>Anforderungen für Hintergrundaudio
Ihre App muss die folgenden Anforderungen für die Audiowiedergabe erfüllen, während sie sich im Hintergrund befindet.

* Fügen Sie Ihrem App-Manifest die Funktion **Medienwiedergabe im Hintergrund** wie weiter unten in diesem Artikel beschrieben hinzu.
* Wenn die automatische Integration von **MediaPlayer** in die Steuerelemente für den Systemmedientransport (System Media Transport Controls, SMTC) von der App deaktiviert wird (z. B. durch Festlegen der [**CommandManager.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled)-Eigenschaft auf FALSE), müssen Sie die manuelle Integration in SMTC implementieren, um die Medienwiedergabe im Hintergrund zu aktivieren. Die manuelle Integration in SMTC ist auch erforderlich, wenn Sie eine andere API als **MediaPlayer** (z. B. [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph)) verwenden, um die Audiowiedergabe beim Wechsel der App in den Hintergrund fortzusetzen. Die Mindestanforderungen für die SMTC-Integration sind im Abschnitt „Verwenden der Steuerelemente für den Systemmedientransport für Audiowiedergabe im Hintergrund“ von [Manuelle Steuerung der Steuerelemente für den Systemmedientransport](system-media-transport-controls.md) beschrieben.
* Während sich Ihre App im Hintergrund befindet, müssen Sie unterhalb der Grenzwerte für die Speicherauslastung bleiben, die vom System für Hintergrund-Apps festgelegt wurden. Eine Anleitung zur Verwaltung des Arbeitsspeichers, während die App im Hintergrund ausgeführt wird, finden Sie später in diesem Artikel.

## <a name="background-media-playback-manifest-capability"></a>Manifestfunktion „Medienwiedergabe im Hintergrund“
Wenn Sie Hintergrundaudio aktivieren möchten, müssen Sie der App-Manifestdatei „Package.appxmanifest“ die Funktion „Medienwiedergabe im Hintergrund“ hinzufügen. 

**Hinzufügen von Funktionen in der app-Manifest, das mit dem manifest-designer**

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
Wenn Ihre App vom Vordergrund in den Hintergrund wechselt, wird das [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground)-Ereignis ausgelöst. Wechselt die App dann wieder in den Vordergrund, wird das [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)-Ereignis ausgelöst. Da es sich dabei um App-Lebenszyklusereignisse handelt, sollten Sie bei der App-Erstellung Handler für diese Ereignisse registrieren. In der Standardprojektvorlage wird der Handler dem **App**-Klassenkonstruktor in „App.xaml.cs“ hinzugefügt. 

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

Erstellen Sie eine Variable, um nachzuverfolgen, ob die App momentan im Hintergrund ausgeführt wird.

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

Wenn das [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground)-Ereignis ausgelöst wird, legen Sie die Nachverfolgungsvariable fest, um anzugeben, dass die App momentan im Hintergrund ausgeführt wird. Langzeitaufgaben sollten im **EnteredBackground**-Ereignis nicht ausgeführt werden, da der Übergang in den Hintergrund dem Benutzer dadurch langsam erscheinen könnte.

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

Im [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)-Ereignishandler sollten Sie die Nachverfolgungsvariable festlegen, um anzugeben, dass die App nicht mehr im Hintergrund ausgeführt wird.

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

### <a name="memory-management-requirements"></a>Anforderungen an die Speicherverwaltung
Den wichtigsten Teil beim Übergang zwischen Vorder- und Hintergrund stellt die Verwaltung des von der App genutzten Speichers dar. Da die Ausführung im Hintergrund die Speicherressourcen verringert, die der App vom System gewährt werden, sollten Sie die App auch für das [**AppMemoryUsageIncreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased)-Ereignis und das [**AppMemoryUsageLimitChanging**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging)-Ereignis registrieren. Wenn diese Ereignisse ausgelöst werden, sollten Sie die aktuelle Speicherbelegung und den aktuellen Grenzwert Ihrer App überprüfen und die Speichernutzung ggf. reduzieren. Informationen dazu, wie Sie die Speichernutzung während der Ausführung im Hintergrund reduzieren, finden Sie unter [Geben Sie Speicher frei, wenn Ihre App in den Hintergrund verschoben wird](../launch-resume/reduce-memory-usage.md).

## <a name="network-availability-for-background-media-apps"></a>Netzwerkverfügbarkeit für im Hintergrund ausgeführte Medien-Apps
Alle netzwerkfähigen Medienquellen, die nicht auf der Basis eines Datenstroms oder einer Datei erstellt wurden, behalten beim Abrufen von Remoteinhalten eine aktive Netzwerkverbindung bei. Andernfalls wird die Verbindung nicht beibehalten. [**MediaStreamSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaStreamSource), insbesondere dazu, abhängig von der Anwendung, melden den richtigen Bereich für die gepufferte ordnungsgemäß auf die Plattform mit [ **SetBufferedRange**](https://docs.microsoft.com/uwp/api/windows.media.core.mediastreamsource.setbufferedrange). Nachdem der gesamte Inhalt vollständig gepuffert wurde, wird die Netzwerkverbindung nicht mehr für die App reserviert.

Beim Durchführen von Netzwerkaufrufen, die im Hintergrund ausgeführt werden, wenn kein Mediendownload stattfindet, müssen Sie diese in eine entsprechende Aufgabe, wie [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) oder [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger), einschließen. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## <a name="related-topics"></a>Verwandte Themen
* [Medienwiedergabe](media-playback.md)
* [Abspielen von Audio- und Videodateien mit MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrieren Sie in das Medium für die Transport-Steuerelemente](integrate-with-systemmediatransportcontrols.md)
* [Hintergrund-Audio-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 




