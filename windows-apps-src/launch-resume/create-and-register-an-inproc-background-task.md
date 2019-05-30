---
title: Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen
description: Erstellen und registrieren Sie eine In-Process-Aufgabe, die im gleichen Prozess wie die Vordergrund-App ausgeführt wird.
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, Uwp, Hintergrundaufgaben
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: f37ffe21795fc68ff72b4e6f1de591c96d2f8b90
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366215"
---
# <a name="create-and-register-an-in-process-background-task"></a>Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen

**Wichtige APIs**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

In diesem Thema wird gezeigt, wie Sie eine Hintergrundaufgabe erstellen und registrieren, die im gleichen Prozess wie Ihre App ausgeführt wird.

In-Process-Hintergrundaufgaben sind einfacher als Out-of-Process-Hintergrundaufgaben zu implementieren. Sie sind jedoch weniger stabil. Wenn der in einer in-Process-Hintergrundaufgabe ausgeführte Code abstürzt, wirkt sich dies auf Ihre App aus. Beachten Sie außerdem, dass [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger), [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) und **IoTStartupTask** nicht mit dem In-Process-Modell verwendet werden können. Zudem ist das Aktivieren einer VoIP-Hintergrundaufgabe innerhalb der Anwendung nicht möglich. Diese Trigger und Aufgaben werden bei Verwendung des Out-of-Process-Hintergrundaufgabenmodells weiterhin unterstützt.

Beachten Sie, dass Hintergrundaktivitäten auch bei Ausführung innerhalb des Vordergrundprozesses der App beendet werden können, wenn Ausführungszeitlimits überschritten werden. Für bestimmte Zwecke ist die Resilienz der Trennung von Arbeiten in eine Hintergrundaufgabe, die in einem separaten Prozess ausgeführt wird, weiterhin hilfreich. Die Trennung von Hintergrundarbeiten als Aufgabe von der Vordergrundanwendung kann die beste Option für Arbeiten sein, die keine Kommunikation mit der Vordergrundanwendung erfordern.

## <a name="fundamentals"></a>Grundlagen

Das In-Process-Modell erweitert den Anwendungslebenszyklus um verbesserte Benachrichtigungen, wenn sich die App im Vordergrund oder im Hintergrund befindet. Zwei neue Ereignisse werden aus dem Anwendungsobjekt für diese Übergänge verfügbar: [**EnteredBackground** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) und [ **LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground). Diese Ereignisse passen je nach Sichtbarkeitszustand der Anwendung in den Anwendungslebenszyklus. Weitere Informationen zu diesen Ereignissen und deren Auswirkungen auf den Anwendungslebenszyklus finden Sie unter [App-Lebenszyklus](app-lifecycle.md).

Auf oberer Ebene behandeln Sie das **EnteredBackground**-Ereignis zum Ausführen von Code bei der Ausführung der App im Hintergrund und das **LeavingBackground**-Ereignis, um zu erfahren, wann Ihre App in den Vordergrund verschoben wurde.

## <a name="register-your-background-task-trigger"></a>Registrieren des Triggers für die Hintergrundaufgabe

In-Process-Hintergrundaktivitäten werden ähnlich wie Out-of-Process-Hintergrundaktivitäten registriert. Alle Hintergrundtrigger werden mit der Registrierung mit [BackgroundTaskBuilder](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder?f=255&MSPPError=-2147217396) gestartet. Der Generator vereinfacht die Registrierung einer Hintergrundaufgabe, indem alle erforderlichen Werte an einem zentralen Ort festlegt werden:

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for in-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> Universelle Windows-Apps müssen jedoch [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen, bevor Hintergrundtrigger-Typen registriert werden.
> Rufen Sie [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) und anschließend [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

In-Process-Hintergrundaktivitäten erfolgen ohne das Festlegen von `TaskEntryPoint.` Bleibt dieser Wert leer, wird der Standardeinstiegspunkt aktiviert, eine neue geschützte Methode für das Application-Objekt namens [OnBackgroundActivated()](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated).

Sobald ein Trigger registriert ist, wird er basierend auf dem in der [SetTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.settrigger)-Methode festgelegten Triggertyp ausgelöst. Im obigen Beispiel wird ein [TimeTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.timetrigger) verwendet, der 15 Minuten nach dem Registrierungszeitpunkt ausgelöst wird.

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>Hinzufügen einer Bedingung, die bestimmt, wann Ihre Aufgabe ausgeführt wird (optional)

Sie können eine Bedingung hinzufügen, die bestimmt, wann Ihre Aufgabe ausgeführt wird, nachdem das Auslöseereignis eintritt. Wenn Sie zum Beispiel möchten, dass die Aufgabe erst ausgeführt wird, wenn der Benutzer anwesend ist, verwenden Sie die Bedingung **UserPresent**. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

Der folgende Beispielcode weist eine Bedingung zu, die die Anwesenheit des Benutzers voraussetzt:

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>Platzieren des Codes der Hintergrundaktivität in „OnBackgroundActivated()“

Fügen Sie Ihren Hintergrund Aktivität-Code in [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated) auf den Hintergrund-Trigger reagieren, wenn es ausgelöst wird. **OnBackgroundActivated** kann wie [IBackgroundTask.Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) behandelt werden. Die Methode verfügt über eine [BackgroundActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.backgroundactivatedeventargs) Parameter, der alles enthält, die die **ausführen** Methode bietet. Z. B. in "App.Xaml.cs":

``` cs
using Windows.ApplicationModel.Background;

...

sealed partial class App : Application
{
  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      DoYourBackgroundWork(taskInstance);  
  }
}
```

Für eine umfangreichere **OnBackgroundActivated** Beispiel finden Sie unter [konvertieren eine app Service in demselben Prozess wie der Host-app ausführen](convert-app-service-in-process.md).

## <a name="handle-background-task-progress-and-completion"></a>Behandeln des Status und Abschlusses von Hintergrundaufgaben

Der Aufgabenstatus und -abschluss kann genauso wie bei Multiprozess-Hintergrundaufgaben überwacht werden (siehe [Überwachen von Status und Durchführung von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)). Wahrscheinlich stellen Sie jedoch fest, dass die Überwachung mithilfe von Variablen zum Nachverfolgen von Status oder Abschluss in Ihrer App einfacher ist. Dies ist einer der Vorteile, wenn der Code der Hintergrundaktivität im selben Prozess wie Ihre App ausgeführt wird.

## <a name="handle-background-task-cancellation"></a>Behandeln des Abbruchs von Hintergrundaufgaben

In-Process-Hintergrundaufgaben werden auf die gleiche Weise abgebrochen wie Out-of-Process-Hintergrundaufgaben (siehe [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)). Beachten Sie, dass der **BackgroundActivated**-Ereignishandler vor dem Abbruch beendet werden muss, oder der gesamte Prozess wird beendet. Wenn die Vordergrund-App beim Abbrechen der Hintergrundaufgabe unerwartet geschlossen wird, überprüfen Sie, ob der Handler vor dem Abbruch beendet wurde.

## <a name="the-manifest"></a>Das Manifest

Im Gegensatz zu Out-of-Process-Hintergrundaufgaben müssen Sie dem Paketmanifest keine Hintergrundaufgabeninformationen hinzufügen, um In-Process-Hintergrundaufgaben auszuführen.

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

Sie sollten jetzt über die Grundlagen verfügen, um eine In-Process-Hintergrundaufgabe zu schreiben.

Eine API-Referenz, konzeptionelle Richtlinien zu Hintergrundaufgaben und ausführlichere Anweisungen zum Schreiben von Apps, die Hintergrundaufgaben verwenden, finden Sie unter den folgenden verwandten Themen:

## <a name="related-topics"></a>Verwandte Themen

**Ausführliche Hintergrundinformationen Aufgabe Anweisungsthemen**

* [Konvertieren Sie eine Out-of-Process-Hintergrundaufgabe in einem in-Process-Hintergrundaufgabe](convert-out-of-process-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Wiedergeben von Medien im Hintergrund](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)

**Hintergrund-Task-Leitfaden**

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Wie Sie auslösen, anhalten, fortsetzen und hintergrundereignissen in UWP-apps (beim debugging)](https://go.microsoft.com/fwlink/p/?linkid=254345)

**Hintergrund Task-API-Referenz**

* [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)
