---
title: Behandeln einer abgebrochenen Hintergrundaufgabe
description: Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen, die Abbruchanforderungen erkennt, die Ausführung beendet und den Abbruch mithilfe des beständigen Speichers an die App meldet.
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp, background task
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: c59982c174909a3fb8ab0b21d5dd792969cfeebc
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259476"
---
# <a name="handle-a-cancelled-background-task"></a>Behandeln einer abgebrochenen Hintergrundaufgabe

**Wichtige APIs**

-   [**BackgroundTaskCanceledEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcanceledeventhandler)
-   [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)
-   [**ApplicationData.Current**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current)

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen, die mithilfe des beständigen Speichers Abbruchanforderungen erkennt, die Ausführung beendet und den Abbruch an die App meldet.

This topic assumes you have already created a background task class, including the **Run** method that is used as the background task entry point. Um schnell mit dem Erstellen einer Hintergrundaufgabe zu beginnen, lesen Sie [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md) oder [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md). Ausführlichere Informationen zu Bedingungen und Triggern finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

Dieses Thema gilt auch für Hintergrundaufgaben innerhalb von Prozessen. But instead of the **Run** method, substitute **OnBackgroundActivated**. Hintergrundaufgaben innerhalb von Prozessen benötigen keinen beständigen Speicher, um den Abbruch zu signalisieren, da dieser mithilfe des App-Status übermittelt werden kann, wenn die Hintergrundaufgabe im selben Prozess ausgeführt wird, wie die Vordergrund-App.

## <a name="use-the-oncanceled-method-to-recognize-cancellation-requests"></a>Verwenden der Methode „OnCanceled“ zum Erkennen von Abbruchanforderungen

Schreiben Sie eine Methode für die Behandlung des Abbruchereignisses.

> [!NOTE]
> Für alle Gerätefamilien mit Ausnahme von Desktops können Hintergrundaufgaben beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. If an out of memory exception is not surfaced, or the app doesn't handle it, then the background task will be terminated without warning and without raising the OnCanceled event. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt werden kann.

Eine **OnCanceled**-Methode erstellen Sie wie folgt. Diese Methode ist der Einstiegspunkt, der von der Windows-Runtime aufgerufen wird, wenn eine Abbruchanforderung für die Hintergrundaufgabe erfolgt.

```csharp
private void OnCanceled(
    IBackgroundTaskInstance sender,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(
    IBackgroundTaskInstance^ taskInstance,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

Add a flag variable called **\_CancelRequested** to the background task class. Diese Variable wird verwendet, um anzuzeigen, ob eine Abbruchanforderung erfolgt ist.

```csharp
volatile bool _CancelRequested = false;
```

```cppwinrt
private:
    volatile bool m_cancelRequested;
```

```cpp
private:
    volatile bool CancelRequested;
```

In the **OnCanceled** method you created in step 1, set the flag variable **\_CancelRequested** to **true**.

The full [background task sample]( https://go.microsoft.com/fwlink/p/?linkid=227509) **OnCanceled** method sets **\_CancelRequested** to **true** and writes potentially useful debug output.

```csharp
private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    _cancelRequested = true;

    Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    m_cancelRequested = true;
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    CancelRequested = true;
}
```

In the background task's **Run** method, register the **OnCanceled** event handler method before starting work. Für eine Hintergrundaufgabe innerhalb von Prozessen empfiehlt sich diese Registrierung im Rahmen der Anwendungsinitialisierung. For example, use the following line of code.

```csharp
taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
```

```cppwinrt
taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });
```

```cpp
taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);
```

## <a name="handle-cancellation-by-exiting-your-background-task"></a>Behandeln des Abbruchs durch Beenden der Hintergrundaufgabe

When a cancellation request is received, your method that does background work needs to stop work and exit by recognizing when **\_cancelRequested** is set to **true**. For in-process background tasks, this means returning from the **OnBackgroundActivated** method. For out-of-process background tasks, this means returning from the **Run** method.

Ändern Sie den Code der Hintergrundaufgabenklasse, um die Kennzeichenvariable zu überprüfen, während die Hintergrundaufgabe ausgeführt wird. If **\_cancelRequested** becomes set to true, stop work from continuing.

The [background task sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) includes a check that stops the periodic timer callback if the background task is canceled.

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

> [!NOTE]
> The code sample shown above uses the [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance).[**Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress) property being used to record background task progress. Der Fortschritt wird der App mithilfe der [**BackgroundTaskProgressEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskProgressEventArgs)-Klasse gemeldet.

Modify the **Run** method so that after work has stopped, it records whether the task completed or was cancelled. Dieser Schritt gilt für Hintergrundaufgaben außerhalb von Prozessen, da eine Möglichkeit für die Kommunikation zwischen Prozessen haben müssen, wenn die Hintergrundaufgabe abgebrochen wurde. Für Hintergrundaufgaben innerhalb von Prozessen können Sie den Status einfach mit der Anwendung teilen, um anzugeben, dass die Aufgabe abgebrochen wurde.

The [background task sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) records status in LocalSettings.

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();

    var settings = ApplicationData.Current.LocalSettings;
    var key = _taskInstance.Task.TaskId.ToString();

    // Write to LocalSettings to indicate that this background task ran.
    if (_cancelRequested)
    {
        settings.Values[key] = "Canceled";
    }
    else
    {
        settings.Values[key] = "Completed";
    }
        
    Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
        
    // Indicate that the background task has completed.
    _deferral.Complete();
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();

    // Write to LocalSettings to indicate that this background task ran.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    auto key{ m_taskInstance.Task().Name() };
    settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

    // Indicate that the background task has completed.
    m_deferral.Complete();
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
        
    // Write to LocalSettings to indicate that this background task ran.
    auto settings = ApplicationData::Current->LocalSettings;
    auto key = TaskInstance->Task->Name;
    settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
        
    // Indicate that the background task has completed.
    Deferral->Complete();
}
```

## <a name="remarks"></a>Hinweise

Sie können das [Beispiel zur Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) herunterladen, um diese Codebeispiele im Kontext von Methoden anzuzeigen.

For illustrative purposes, the sample code shows only portions of the **Run** method (and callback timer) from the [background task sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

## <a name="run-method-example"></a>Beispiel der Run-Methode

The complete **Run** method, and timer callback code, from the [background task sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) are shown below for context.

```csharp
// The Run method is the entry point of a background task.
public void Run(IBackgroundTaskInstance taskInstance)
{
    Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");

    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
    var settings = ApplicationData.Current.LocalSettings;
    settings.Values["BackgroundWorkCost"] = cost.ToString();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance;
    _deferral = taskInstance.GetDeferral();
    _taskInstance = taskInstance;

    _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
}

// Simulate the background task activity.
private void PeriodicTimerCallback(ThreadPoolTimer timer)
{
    if ((_cancelRequested == false) && (_progress < 100))
    {
        _progress += 10;
        _taskInstance.Progress = _progress;
    }
    else
    {
        _periodicTimer.Cancel();

        var settings = ApplicationData.Current.LocalSettings;
        var key = _taskInstance.Task.Name;

        // Write to LocalSettings to indicate that this background task ran.
        settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
        Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);

        // Indicate that the background task has completed.
        _deferral.Complete();
    }
}
```

```cppwinrt
void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost{ Windows::ApplicationModel::Background::BackgroundWorkCost::CurrentBackgroundWorkCost() };
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    std::wstring costAsString{ L"Low" };
    if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::Medium) costAsString = L"Medium";
    else if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::High) costAsString = L"High";
    settings.Values().Insert(L"BackgroundWorkCost", winrt::box_value(costAsString));

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    m_deferral = taskInstance.GetDeferral();
    m_taskInstance = taskInstance;

    Windows::Foundation::TimeSpan period{ std::chrono::seconds{1} };
    m_periodicTimer = Windows::System::Threading::ThreadPoolTimer::CreatePeriodicTimer([this](Windows::System::Threading::ThreadPoolTimer timer)
    {
        if (!m_cancelRequested && m_progress < 100)
        {
            m_progress += 10;
            m_taskInstance.Progress(m_progress);
        }
        else
        {
            m_periodicTimer.Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
            auto key{ m_taskInstance.Task().Name() };
            settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

            // Indicate that the background task has completed.
            m_deferral.Complete();
        }
    }, period);
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
    auto settings = ApplicationData::Current->LocalSettings;
    settings->Values->Insert("BackgroundWorkCost", cost.ToString());

    // Associate a cancellation handler with the background task.
    taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    TaskDeferral = taskInstance->GetDeferral();
    TaskInstance = taskInstance;

    auto timerDelegate = [this](ThreadPoolTimer^ timer)
    {
        if ((CancelRequested == false) &&
            (Progress < 100))
        {
            Progress += 10;
            TaskInstance->Progress = Progress;
        }
        else
        {
            PeriodicTimer->Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings = ApplicationData::Current->LocalSettings;
            auto key = TaskInstance->Task->Name;
            settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");

            // Indicate that the background task has completed.
            TaskDeferral->Complete();
        }
    };

    TimeSpan period;
    period.Duration = 1000 * 10000; // 1 second
    PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
}
```

## <a name="related-topics"></a>Verwandte Themen

- [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb des Prozesses](create-and-register-an-inproc-background-task.md)
- [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
- [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
- [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
- [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
- [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
- [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
- [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
- [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
- [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
- [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
- [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
- [How to trigger suspend, resume, and background events in UWP apps (when debugging)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
