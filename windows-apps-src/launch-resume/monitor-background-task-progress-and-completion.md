---
title: Überwachen des Status und Abschlusses von Hintergrundaufgaben
description: Hier erfahren Sie, wie Ihre App den von einer Hintergrundaufgabe gemeldeten Status und Abschluss erkennt.
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 9c6fb40d64d0826a5cbbfa7c97694e6650df9dbe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172974"
---
# <a name="monitor-background-task-progress-and-completion"></a>Überwachen des Status und Abschlusses von Hintergrundaufgaben

**Wichtige APIs**

- [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)
- [**BackgroundTaskProgressEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskprogresseventhandler)
- [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Erfahren Sie, wie Ihre App einen von einer ausgeführten Out-of-Process-Hintergrundaufgabe gemeldeten Status und Abschluss erkennt. (Für In-Process-Hintergrundaufgaben können Sie freigegebene Variablen festlegen, um Status und Abschluss anzugeben.)

Der Status und Abschluss von Hintergrundaufgaben kann durch App-Code überwacht werden. Hierzu abonniert die App Ereignisse der Hintergrundaufgabe(n), die sie im System registriert hat.

- In diesem Thema wird vorausgesetzt, dass Sie über eine App verfügen, die Hintergrundaufgaben registriert. Um schnell mit dem Erstellen einer Hintergrundaufgabe zu beginnen, lesen Sie [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md) oder [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md). Ausführlichere Informationen zu Bedingungen und Triggern finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## <a name="create-an-event-handler-to-handle-completed-background-tasks"></a>Erstellen Sie einen Ereignishandler zum Behandeln abgeschlossener Hintergrundaufgaben.

### <a name="step-1"></a>Schritt 1
Erstellen Sie eine Ereignishandlerfunktion zum Behandeln abgeschlossener Hintergrundaufgaben. Dieser Code muss einem bestimmten Speicherbedarf folgen, der ein [**ibackgroundtaskregistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) -Objekt und ein [**backgroundtaskcompletedeventargs**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskCompletedEventArgs) -Objekt annimmt.

Verwenden Sie den folgenden Speicherbedarf für die Ereignishandler-Methode " **onabgeschlossene** Hintergrundaufgabe".

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    // TODO: Add code that deals with background task completion.
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task completion.
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    // TODO: Add code that deals with background task completion.
};
```

### <a name="step-2"></a>Schritt 2
Fügen Sie dem Ereignishandler Code zum Behandeln des Abschlusses der Hintergrundaufgabe hinzu.

Das [Beispiel für eine Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) aktualisiert z. B. die Benutzeroberfläche.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    UpdateUI();
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    UpdateUI();
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{    
    UpdateUI();
};
```

## <a name="create-an-event-handler-function-to-handle-background-task-progress"></a>Erstellen einer Ereignishandlerfunktion zum Behandeln des Hintergrundaufgabenfortschritts

### <a name="step-1"></a>Schritt 1
Erstellen Sie eine Ereignishandlerfunktion zum Behandeln abgeschlossener Hintergrundaufgaben. Dieser Code muss einem bestimmten Profil entsprechen und ein [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration)-Objekt sowie ein [**BackgroundTaskProgressEventArgs**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskProgressEventArgs)-Objekt enthalten:

Verwenden Sie das folgende Profil für die Ereignishandlermethode für die OnProgress-Hintergrundaufgabe:

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    // TODO: Add code that deals with background task progress.
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task progress.
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    // TODO: Add code that deals with background task progress.
};
```

### <a name="step-2"></a>Schritt 2
Fügen Sie dem Ereignishandler Code zum Behandeln des Abschlusses der Hintergrundaufgabe hinzu.

So aktualisiert beispielsweise das [Beispiel für eine Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) die Benutzeroberfläche mit dem Fortschrittsstatus, der mithilfe des *args*-Parameters übergeben wird:

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    var progress = "Progress: " + args.Progress + "%";
    BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    UpdateUI();
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
{
    winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    auto progress = "Progress: " + args->Progress + "%";
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
};
```

## <a name="register-the-event-handler-functions-with-new-and-existing-background-tasks"></a>Registrieren der Ereignishandlerfunktionen mit neuen und vorhandenen Hintergrundaufgaben

### <a name="step-1"></a>Schritt 1
Wenn die App erstmals eine Hintergrundaufgabe registriert, muss sie sich für den Empfang von Status- und Abschlussupdates registrieren, falls die Aufgabe ausgeführt wird, während die App im Vordergrund noch aktiv ist.

So ruft beispielsweise das [Beispiel für eine Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) für jede Hintergrundaufgabe, die es registriert, die folgende Funktion auf:

```csharp
private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
{
    task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
}
```

```cppwinrt
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(Windows::ApplicationModel::Background::IBackgroundTaskRegistration const& task)
{
    auto progress{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
    {
        winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    } };

    task.Progress(progress);

    auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
    {
        UpdateUI();
    } };

    task.Completed(completed);
}
```

```cpp
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)
{
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        auto progress = "Progress: " + args->Progress + "%";
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    };

    task->Progress += ref new BackgroundTaskProgressEventHandler(progress);

    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {
        UpdateUI();
    };

    task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
}
```

### <a name="step-2"></a>Schritt 2
Wenn die App startet oder zu einer neuen Seite navigiert, für die der Hintergrundaufgabenstatus relevant ist, muss sie eine Liste mit den derzeit registrierten Hintergrundaufgaben abrufen und diese den Status- und Abschlussereignishandlerfunktionen zuordnen. Die Liste mit den derzeit von der Anwendung registrierten Hintergrundaufgaben ist in der [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration).[**AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks)-Eigenschaft gespeichert.

So verwendet beispielsweise das [Beispiel für eine Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) den folgenden Code, um Ereignishandler anzufügen, wenn zur Seite „SampleBackgroundTask“ navigiert wird:

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    foreach (var task in BackgroundTaskRegistration.AllTasks)
    {
        if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value);
            BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);
        }
    }

    UpdateUI();
}
```

```cppwinrt
void SampleBackgroundTask::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& /* e */)
{
    // A pointer back to the main page. This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    m_rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

    for (auto const& task : allTasks)
    {
        if (task.Value().Name() == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value());
            break;
        }
    }

    UpdateUI();
}
```

```cpp
void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
{
    // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto iter = BackgroundTaskRegistration::AllTasks->First();
    auto hascur = iter->HasCurrent;
    while (hascur)
    {
        auto cur = iter->Current->Value;

        if (cur->Name == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(cur);
            break;
        }

        hascur = iter->MoveNext();
    }

    UpdateUI();
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und registrieren Sie eine Prozess interne Hintergrundaufgabe](create-and-register-an-inproc-background-task.md).
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](/previous-versions/hh974425(v=vs.110))