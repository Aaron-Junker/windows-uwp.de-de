---
title: Unterstützen Ihrer App mit Hintergrundaufgaben
description: In den Themen in diesem Abschnitt wird gezeigt, wie Sie einfachen Code im Hintergrund ausführen, um auf Auslöser zu reagieren.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.date: 08/21/2017
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: ac3a20afc75cc7a6cf3c9f874fa26e1b6387dd89
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171844"
---
# <a name="support-your-app-with-background-tasks"></a>Unterstützen Ihrer App mit Hintergrundaufgaben


In den Themen in diesem Abschnitt wird gezeigt, wie Sie einfachen Code im Hintergrund ausführen, um auf Auslöser zu reagieren. Sie können mit Hintergrundaufgaben Funktionen bereitstellen, wenn Ihre App gerade ausgesetzt ist oder nicht ausgeführt wird. Sie können Hintergrundaufgaben auch für Echtzeitkommunikations-Apps wie VOIP, E-Mail und Sofortnachrichten verwenden.

## <a name="playing-media-in-the-background"></a>Wiedergeben von Medien im Hintergrund

Ab Windows 10 (Version 1607) ist die Wiedergabe von Audio im Hintergrund sehr viel einfacher. Weitere Informationen finden Sie unter [Wiedergeben von Medien im Hintergrund](../audio-video-camera/background-audio.md).

## <a name="in-process-and-out-of-process-background-tasks"></a>Hintergrundaufgaben innerhalb und außerhalb von Prozessen

Es gibt zwei Ansätze für die Implementierung von Hintergrundaufgaben:

* In-Process: die APP und Ihr Hintergrundprozess werden im gleichen Prozess ausgeführt.
* Out-of-Process: die APP und der Hintergrundprozess werden in separaten Prozessen ausgeführt.

Die Unterstützung für Hintergrundaufgaben innerhalb von Prozessen wurde mit Windows 10 (Version 1607) eingeführt, um das Schreiben von Hintergrundaufgaben zu vereinfachen. Es ist jedoch weiterhin möglich, Hintergrundaufgaben außerhalb von Prozessen zu schreiben. Unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md) finden Sie Empfehlungen dazu, wann Sie eine Hintergrundaufgabe innerhalb eines Prozesses und wann außerhalb eines Prozesses schreiben sollten.

Out-of-Process-Hintergrundaufgaben sind stabiler, da der Hintergrundprozess den App-Prozess nicht durchführen kann, wenn etwas schief geht. Die Resilienz kommt jedoch zu einer größeren Komplexität, um die prozessübergreifende Kommunikation zwischen der APP und der Hintergrundaufgabe zu verwalten.

Out-of-Process-Hintergrundaufgaben werden als Lightweight-Klassen implementiert, die die [**ibackgroundtask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) -Schnittstelle implementieren, die das Betriebssystem in einem separaten Prozess (backgroundtaskhost.exe) ausführt. Registrieren Sie eine Hintergrundaufgabe mithilfe der [**backgroundtaskbuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) -Klasse. Der Klassenname wird beim Registrieren der Hintergrundaufgabe zum Angeben des Einstiegspunkts verwendet.

In Windows 10 (Version 1607) können Sie Hintergrundaktivitäten aktivieren, ohne eine Hintergrundaufgabe zu erstellen. Sie können stattdessen den Hintergrund Code direkt innerhalb des Prozesses der Vordergrund Anwendung ausführen.

Erste Schritte für das schnelle Erstellen von Hintergrundaufgaben finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md).

Erste Schritte für das schnelle Erstellen von Hintergrundaufgaben außerhalb von Prozessen finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md).

> [!TIP]
> Ab Windows 10 müssen Sie eine App nicht mehr auf dem Sperrbildschirm platzieren, damit eine Hintergrundaufgabe dafür registriert werden kann.

## <a name="background-tasks-for-system-events"></a>Hintergrundaufgaben für Systemereignisse

Ihre App kann auf Systemereignisse reagieren, indem mit der [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger)-Klasse eine Hintergrundaufgabe registriert wird. Eine APP kann einen der folgenden System Ereignis Trigger verwenden (definiert in [**systemtriggertype**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)).

| Triggername                     | BESCHREIBUNG                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | Das Internet wird verfügbar.                                                                                |
| **NetworkStateChange**           | Eine Netzwerkänderung findet statt, z. B. werden die Kosten oder Verbindungsoptionen geändert.                                              |
| **OnlineIdConnectedStateChange** | Die mit dem Konto verbundene Online-ID wird geändert.                                                                 |
| **SmsReceived**                  | Auf einem installierten mobilen Breitbandgerät geht eine SMS ein.                                         |
| **TimeZoneChange**               | Die Zeitzone auf dem Gerät ändert sich (z. B. wenn das System die Uhrzeit auf die Sommerzeit umstellt). |

Weitere Informationen finden [Sie unter reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md).

## <a name="conditions-for-background-tasks"></a>Bedingungen für Hintergrundaufgaben

Über Bedingungen können Sie steuern, wann die Hintergrundaufgabe ausgeführt wird, auch nachdem sie ausgelöst wurde. Nach dem Auslösen wird die Hintergrundaufgabe erst ausgeführt, wenn alle ihre Bedingungen erfüllt sind. Sie können die folgenden Bedingungen verwenden (dargestellt durch die [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)-Enumeration).

| Bedingungsname           | BESCHREIBUNG                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | Das Internet muss verfügbar sein.   |
| **InternetNotAvailable** | Das Internet darf nicht verfügbar sein. |
| **SessionConnected**     | Die Sitzung muss verbunden sein.    |
| **SessionDisconnected**  | Die Sitzung darf nicht verbunden sein. |
| **UserNotPresent**       | Der Benutzer muss abwesend sein.            |
| **UserPresent**          | Der Benutzer muss anwesend sein.         |

Fügen Sie die **InternetAvailable**-Bedingung zur Hintergrundaufgabe [BackgroundTaskBuilder.AddCondition](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) hinzu, um die Hintergrundaufgabe zu verzögern, bis der Netzwerkstapel ausgeführt wird. Diese Bedingung spart Strom, da die Hintergrundaufgabe erst ausgeführt wird, wenn das Netzwerk verfügbar ist. Dieser Zustand stellt keine Aktivierung in Echtzeit bereit.

Wenn für Ihre Hintergrundaufgabe eine Netzwerk Konnektivität erforderlich ist, legen Sie [isnetworkressitzig](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) fest, um sicherzustellen, dass das Netzwerk während der Ausführung des Hintergrund Tasks weiterhin aktiv bleibt. Dies weist die Infrastruktur für Hintergrundaufgaben an, die Netzwerkverbindung für die Ausführung der Aufgabe auch dann beizubehalten, wenn sich das Gerät im verbundenen Standbymodus befindet. Wenn die Hintergrundaufgabe **isnetworkrequsys**nicht festgelegt hat, kann Ihre Hintergrundaufgabe nicht auf das Netzwerk zugreifen, wenn der verbundene Standbymodus verwendet wird (z. b. wenn der Bildschirm eines Telefons ausgeschaltet ist).  
Weitere Informationen zu Hintergrundaufgaben Bedingungen finden Sie unter [Festlegen von Bedingungen für das Ausführen eines Hintergrund](set-conditions-for-running-a-background-task.md)Tasks.

## <a name="application-manifest-requirements"></a>Anforderungen für das Anwendungsmanifest

Damit Ihre App eine Hintergrundaufgabe registrieren kann, die außerhalb eines Prozesses ausgeführt wird, muss sie im Anwendungsmanifest deklariert werden. Hintergrundaufgaben, die im gleichen Prozess wie ihre Host-App ausgeführt werden, müssen nicht im Anwendungsmanifest deklariert werden. Weitere Informationen finden Sie [unter Deklarieren von Hintergrundaufgaben im Anwendungs Manifest](declare-background-tasks-in-the-application-manifest.md).

## <a name="background-tasks"></a>Hintergrundaufgaben

Die folgenden Echtzeitauslöser können verwendet werden, um einfachen benutzerdefinierten Code im Hintergrund auszuführen:

| Echtzeitauslöser  | BESCHREIBUNG |
|--------------------|-------------|
| **Steuerungs Kanal** | Hintergrundaufgaben können eine Verbindung aufrechterhalten und Nachrichten auf dem Steuerkanal mithilfe des [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) empfangen. Wenn Ihre App ein Socket überwacht, können Sie den Socketbroker statt **ControlChannelTrigger** verwenden. Weitere Informationen zur Verwendung der Socketbroker finden Sie unter [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger). **ControlChannelTrigger** wird unter Windows Phone nicht unterstützt. |
| **Zeitgeber** | Hintergrundaufgaben können in einem Intervall von bis zu 15 Minuten ausgeführt werden, und sie können mithilfe des [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger) auf die Ausführung zu einer bestimmten Zeit festgelegt werden. Weitere Informationen finden Sie im Thema [Ausführen einer Hintergrundaufgabe mit einem Timer](run-a-background-task-on-a-timer-.md). |
| **Pushbenachrichtigung** | Hintergrundaufgaben reagieren auf den [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger), um unformatierte Pushbenachrichtigungen zu empfangen. |

**Hinweis**  

Universelle Windows-Apps müssen jedoch [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen, bevor Hintergrundtrigger-Typen registriert werden.

Rufen Sie [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) und anschließend [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

**Grenzwerte für die Anzahl der auslöserinstanzen:** Es gibt Grenzwerte für die Anzahl der Instanzen einiger Trigger, die eine APP registrieren kann. Eine APP kann nur "   [applicationtrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)", " [mediaprocessingtrigger](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) " und " [deviceusertrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) " einmal pro Instanz der APP registrieren. Wenn eine APP diesen Grenzwert überschreitet, löst die Registrierung eine Ausnahme aus.

## <a name="system-event-triggers"></a>Systemereignistrigger

Die [**SystemTriggerType**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)-Enumeration stellt die folgenden Systemereignistrigger dar:

| Triggername            | BESCHREIBUNG                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | Die Hintergrundaufgabe wird ausgelöst, wenn der Benutzer anwesend ist.   |
| **UserAway**            | Die Hintergrundaufgabe wird ausgelöst, wenn der Benutzer abwesend ist.    |
| **ControlChannelReset** | Die Hintergrundaufgabe wird ausgelöst, wenn ein Steuerkanal zurückgesetzt wird. |
| **SessionConnected**    | Die Hintergrundaufgabe wird ausgelöst, wenn die Sitzung eine Verbindung herstellt.   |

   
Das folgende Systemereignis löst ein Signal aus, wenn der Benutzer eine App auf den oder aus dem Sperrbildschirm verschoben hat.

| Triggername                     | BESCHREIBUNG                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | Dem Sperrbildschirm wird eine App-Kachel hinzugefügt.     |
| **LockScreenApplicationRemoved** | Eine App-Kachel wird vom Sperrbildschirm entfernt. |

 
## <a name="background-task-resource-constraints"></a>Ressourcenbeschränkungen für Hintergrundaufgaben

Hintergrundaufgaben sind einfach. Beschränken Sie die Ausführung von Hintergrundaufgaben auf ein Minimum, um die beste Benutzererfahrung für Vordergrund-Apps und Akkulaufzeit sicherzustellen. Dies wird erzwungen, indem auf Hintergrundaufgaben Ressourcenbeschränkungen angewendet werden.

Hintergrundaufgaben sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt.

### <a name="memory-constraints"></a>Arbeitsspeicherbeschränkungen

Aufgrund der Ressourcenbeschränkungen für Geräte mit wenig Arbeitsspeicher kann für Hintergrundaufgaben ein Arbeitsspeicherlimit gelten. Dieses gibt die maximale Menge von Arbeitsspeicher an, der von der Hintergrundaufgabe verwendet werden kann. Wenn die Hintergrundaufgabe einen Vorgang ausführt, der dieses Limit überschreiten würde, tritt ein Fehler und ggf. eine Ausnahme über zu wenig Arbeitsspeicher auf, die von der Aufgabe behandelt werden kann. Wenn die Ausnahme über wenig Arbeitsspeicher von der Aufgabe nicht behandelt wird oder eine solche Ausnahme je nach Vorgang überhaupt nicht generiert wird, wird die Aufgabe sofort beendet.  

Sie können die [**MemoryManager**](/uwp/api/Windows.System.MemoryManager)-APIs verwenden, um durch Abfrage von aktueller Speicherauslastung und Limit die Obergrenze (falls vorhanden) zu ermitteln und die fortlaufende Speichernutzung der Hintergrundaufgabe zu überwachen.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>Limit pro Gerät für Apps mit Hintergrundaufgaben für Geräte mit wenig Arbeitsspeicher

Geräte mit beschränktem Arbeitsspeicher haben ein Limit für die Anzahl von Apps, die gleichzeitig auf einem Gerät installierbar sind und Hintergrundaufgaben nutzen können. Wenn diese Zahl überschritten wird, tritt für den Aufruf von [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync), der zum Registrieren aller Hintergrundaufgaben erforderlich ist, ein Fehler auf.

### <a name="battery-saver"></a>Stromsparmodus

Solange Sie die App nicht davon befreien, bei aktiviertem Stromsparmodus Hintergrundaufgaben auszuführen und Pushbenachrichtigungen zu empfangen, verhindert der Stromsparmodus (falls aktiviert) die Ausführung von Hintergrundaufgaben, falls das Gerät nicht mit einer externen Stromquelle verbunden ist und der Akku eine angegebene Restmenge unterschreitet. Sie können Hintergrundaufgaben aber weiterhin registrieren.

Informationen zu Unternehmens-apps und apps, die nicht im Microsoft Store veröffentlicht werden, finden Sie unter [Ausführen im Hintergrund auf unbestimmte Zeit](run-in-the-background-indefinetly.md) , um zu erfahren, wie Sie eine Hintergrundaufgabe oder eine erweiterte Ausführungs Sitzung auf unbestimmte Zeit im Hintergrund ausführen.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>Die Ressourcen für Hintergrundaufgabe erlauben die Kommunikation in Echtzeit.

Um zu verhindern, dass Ressourcenkontingente die Echtzeitkommunikation stören, erhalten Hintergrundaufgaben, die den [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) und den [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) verwenden, garantierte Ressourcenkontingente (CPU) für jede ausgeführte Aufgabe. Die Ressourcenkontingente sind wie oben erwähnt und bleiben für diese Hintergrundaufgaben unverändert.

Ihre App benötigt keine anderen Funktionen, um die garantierten Ressourcenkontingente für [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)- und [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)-Hintergrundaufgaben zu erhalten. Das System behandelt sie immer als kritische Hintergrundaufgaben.

## <a name="maintenance-trigger"></a>Wartungsauslöser

Wartungsaufgaben werden nur ausgeführt, wenn das Gerät an die Stromversorgung angeschlossen ist. Weitere Informationen finden Sie unter [Verwenden eines Wartungs Auslösers](use-a-maintenance-trigger.md).

## <a name="background-tasks-for-sensors-and-devices"></a>Hintergrundaufgaben für Sensoren und Geräte

Ihre App kann über eine Hintergrundaufgabe mit der [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger)-Klasse auf Sensoren und Peripheriegeräte zugreifen. Dieser Auslöser ist für zeitaufwändige Vorgänge wie Datensynchronisierung oder Überwachung geeignet. Im Gegensatz zu Aufgaben für Systemereignisse kann eine **DeviceUseTrigger**-Aufgabe nur ausgelöst werden, wenn Ihre App im Vordergrund ausgeführt wird, und es können keine Bedingungen festgelegt werden.

> [!IMPORTANT]
> **DeviceUseTrigger** und **DeviceServicingTrigger** können nicht für prozessinterne Hintergrundaufgaben verwendet werden.

Einige kritische Gerätevorgänge (wie etwa zeitaufwändige Firmwareupdates) können mithilfe von [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) nicht durchgeführt werden. Diese Vorgänge können nur auf dem PC und nur von einer privilegierten App durchgeführt werden, für die [**DeviceServicingTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger) verwendet wird. Eine *privilegierte App* ist eine App, die vom Gerätehersteller dafür autorisiert wurde, diese Vorgänge auszuführen. Mithilfe von Metadaten wird angegeben, welche App, falls zutreffend, als privilegierte App für ein Gerät festgelegt wurde. Weitere Informationen finden Sie unter [Geräte Synchronisierung und Update für Microsoft Store Geräte-apps](/windows-hardware/drivers/devapps/device-sync-and-update-for-uwp-device-apps) .

## <a name="managing-background-tasks"></a>Verwalten von Hintergrundaufgaben

Hintergrundaufgaben können mit Ereignissen und lokalem Speicher Fortschritt, Beendigung und Abbruch an die App melden. Eine App kann außerdem die von einer Hintergrundaufgabe ausgelösten Ausnahmen auffangen und die Registrierung von Hintergrundaufgaben während eines App-Updates verwalten. Weitere Informationen finden Sie unter:

[Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)  
[Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)

Überprüfen Sie die Registrierung Ihrer Hintergrundaufgabe während des App-Starts. Stellen Sie sicher, dass die nicht gruppierten Hintergrundaufgaben ihrer app in backgroundtaskbuilder. AllTasks vorhanden sind. Registrieren Sie diejenigen, die nicht vorhanden sind, erneut. Heben Sie die Registrierung aller Aufgaben auf, die nicht mehr benötigt werden. Dadurch wird sichergestellt, dass alle Registrierungen von Hintergrundaufgaben bei jedem Start der APP auf dem neuesten Stand sind.

## <a name="related-topics"></a>Zugehörige Themen

**Konzeptionelle Richtlinien für Multitasking in Windows 10**

* [Launching, resuming, and multitasking](index.md)

**Ratschläge für zugehörige Hintergrundaufgaben**

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Zugreifen auf Sensoren und Geräte von einer Hintergrundaufgabe aus](access-sensors-and-devices-from-a-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Konvertieren einer Out-of-Process-Hintergrundaufgabe in eine In-Process-Hintergrundaufgabe](convert-out-of-process-background-task.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Registrieren von Gruppen-Hintergrundaufgaben](group-background-tasks.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Wiedergeben von Medien im Hintergrund](../audio-video-camera/background-audio.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Ausführen einer Hintergrundaufgabe beim Aktualisieren der UWP-App](run-a-background-task-during-updatetask.md)
* [Unbegrenzte Ausführung im Hintergrund](run-in-the-background-indefinetly.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Auslösen einer Hintergrundaufgabe in der App](trigger-background-task-from-app.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)