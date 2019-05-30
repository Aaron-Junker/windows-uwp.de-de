---
title: Auslösen einer Hintergrundaufgabe in Ihrer App
description: Es wird beschrieben, wie Sie eine Hintergrundaufgabe in einer Anwendung auslösen.
ms.date: 07/06/2018
ms.topic: article
keywords: Aufgabe des hintergrundtriggers, Hintergrundtasks
ms.localizationpriority: medium
ms.openlocfilehash: d5d163d36b51e414a403986d1fdd73db7925cc0b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371897"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Auslösen einer Hintergrundaufgabe in Ihrer App

Hier erfahren Sie, wie Sie den [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) zum Aktivieren einer Hintergrundaufgabe in Ihrer App verwenden.

Ein Beispiel für einen Trigger für die Anwendung zu erstellen, finden Sie in diesem [Beispiel](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

In diesem Thema wird davon ausgegangen, dass Sie über eine Hintergrundaufgabe verfügen, die Sie in Ihrer Anwendung aktivieren möchten. Wenn Sie noch keine Hintergrundaufgabe haben, finden Sie unter [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs) ein Beispiel für eine Hintergrundaufgabe. Alternativ können Sie die Schritte in [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb des Prozesses](create-and-register-a-background-task.md) befolgen, um eine zu erstellen.

## <a name="why-use-an-application-trigger"></a>Gründe für die Verwendung eines Anwendungstriggers

Verwenden Sie einen **ApplicationTrigger**, um Code getrennt von der Vordergrund-App auszuführen. Ein **ApplicationTrigger** ist geeignet, wenn Ihre App Aufgaben hat, die im Hintergrund ausgeführt werden müssen, auch wenn der Benutzer die Vordergrund-App schließt. Wenn Hintergrundaufgaben anhalten werden sollen, wenn die App geschlossen wird, oder an den Zustand des Vordergrundprozesses gebunden werden sollten, sollte stattdessen [Extended Execution](run-minimized-with-extended-execution.md) verwendet werden.

## <a name="create-an-application-trigger"></a>Erstellen eines Anwendungstriggers

Erstellen Sie einen neuen [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Sie können ihn in einem Feld speichern, wie dies im folgenden Codeausschnitt erfolgt ist. Dadurch müssen wir später keine neue Instanz erstellen, wenn wir den Trigger signalisieren möchten. Sie können jedoch jede beliebige **ApplicationTrigger**-Instanz verwenden, um den Trigger zu signalisieren.

```csharp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
_AppTrigger = new ApplicationTrigger();
```

```cppwinrt
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
Windows::ApplicationModel::Background::ApplicationTrigger _AppTrigger;
```

```cpp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
ApplicationTrigger ^ _AppTrigger = ref new ApplicationTrigger();
```

## <a name="optional-add-a-condition"></a>(Optional) Hinzufügen einer Bedingung

Sie können eine Hintergrundaufgabenbedingung erstellen, um zu steuern, wann die Aufgabe ausgeführt wird. Eine Bedingung verhindert, dass die Hintergrundaufgabe ausgeführt wird, bis die Bedingung erfüllt ist. Weitere Informationen finden Sie unter [Festlegen von Bedingungen für die Ausführung einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md).

In diesem Beispiel wird die Bedingung festgelegt ist **InternetAvailable** , einmal ausgelöst, der Task nur einmal ausgeführt Zugriff auf das Internet verfügbar ist. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable)
```

Ausführlichere Informationen zu Bedingungen und Arten von Hintergrundtriggern finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Aufrufen von RequestAccessAsync()

Rufen Sie vor dem Registrieren der **ApplicationTrigger**-Hintergrundaufgabe [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) auf, um die Ebene der Hintergrundaktivitäten zu bestimmen, die der Benutzer erlaubt, da der Benutzer Hintergrundaktivitäten für Ihre App evtl. deaktiviert hat. Weitere Informationen zu den Möglichkeiten, die Benutzer haben, um die Einstellungen für Hintergrundaktivitäten zu steuern, finden Sie unter [Optimieren von Hintergrundaktivitäten](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe

Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben sowie die Definition der **RegisterBackgroundTask()** -Methode im folgenden Beispielcode finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

Wenn Sie in Betracht ziehen, einen Anwendungstrigger zur Verlängerung der Lebensdauer des Vordergrundprozesses zu verwenden, sollten Sie stattdessen die Verwendung von [Extended Execution](run-minimized-with-extended-execution.md) in Betracht ziehen. Der Anwendungstrigger dient der Erstellung eines separat gehosteten Prozesses für Aufgaben. Der folgende Codeausschnitt registriert einen Hintergrundtrigger außerhalb von Prozessen.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example application trigger";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example application trigger" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example application trigger";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App problemlos mit Szenarien ohne erfolgreiche Registrierung von Hintergrundaufgaben zurechtkommt. Andernfalls stürzt die App unter Umständen ab, wenn sie so konzipiert ist, dass nach dem Versuch, eine Aufgabe zu registrieren, ein gültiges Registrierungsobjekt vorhanden sein muss.

## <a name="trigger-the-background-task"></a>Auslösen der Hintergrundaufgabe

Bevor Sie die Hintergrundaufgabe auslösen, überprüfen Sie anhand von [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration), ob die Hintergrundaufgabe registriert ist. Ein guter Zeitpunkt, um zu überprüfen, ob alle Ihre Hintergrundaufgaben beim Starten der App registriert werden.

Lösen Sie die Hintergrundaufgabe durch Aufrufen von [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger) aus. Jede **ApplicationTrigger**-Instanz kann verwendet werden.

Beachten Sie, dass **ApplicationTrigger.RequestAsync** nicht von der Hintergrundaufgabe selbst aufgerufen werden kann, oder wenn sich die App im Hintergrundausführungszustand befindet (weitere Informationen zu Anwendungszuständen finden Sie unter [App-Lebenszyklus](app-lifecycle.md)).
[DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) kann zurückgegeben werden, wenn der Benutzer Energie- oder Datenschutzrichtlinien festgelegt hat, die verhindern, dass die App Hintergrundaktivitäten ausführt.
Darüber hinaus kann immer nur ein AppTrigger ausgeführt werden. Wenn Sie versuchen, einen AppTrigger auszuführen, während ein anderer bereits ausgeführt wird, wird die Funktion [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) zurückgegeben.

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Verwalten von Ressourcen für Ihre Hintergrundaufgabe

Um zu ermitteln, ob der Benutzer die Hintergrundaktivität Ihrer App begrenzt hat, verwenden Sie [BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager). Achten Sie auf die Akkunutzung, und führen Sie Ihre App nur dann im Hintergrund aus, wenn dies zum Ausführen einer vom Benutzer gewünschten Aktion notwendig ist. Weitere Informationen zu den Möglichkeiten, die Benutzer haben, um die Einstellungen für Hintergrundaktivitäten zu steuern, finden Sie unter [Optimieren von Hintergrundaktivitäten](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).  

- Speicher: Optimieren Ihrer app-Speicher und Energieverbrauch mithilfe ist-Schlüssel, um sicherzustellen, dass das Betriebssystem Ihrer Hintergrundaufgabe ausgeführt werden kann. Um zu ermitteln, wie viel Arbeitsspeicher Ihre Hintergrundaufgabe verwendet, verwenden Sie die [Speicherverwaltungs-APIs](https://docs.microsoft.com/uwp/api/windows.system.memorymanager). Je mehr Arbeitsspeicher Ihre Hintergrundaufgabe verwendet, desto schwieriger wird es für das Betriebssystem, sie weiter auszuführen, wenn sich eine andere App im Vordergrund befindet. Der Benutzer hat letztendlich die Kontrolle über alle Hintergrundaktivitäten, die Ihre App ausführen kann, und kann die Auswirkungen Ihrer App auf den Akkuverbrauch erkennen.  
- CPU-Zeit: Aufgaben im Hintergrund werden durch die Menge der Nutzung der wanduhrzeit beschränkt, die sie für Trigger vom Typ basiert, erhalten. Vom Anwendungstrigger ausgelöste Hintergrundaufgaben sind auf etwa 10 Minuten beschränkt.

Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Hinweise

Ab Windows 10, ist es nicht mehr erforderlich, für den Benutzer Ihrer app auf dem Sperrbildschirm hinzufügen, um Hintergrundaufgaben nutzen zu können.

Eine Hintergrundaufgabe wird nur mithilfe eines **ApplicationTrigger** ausgeführt, wenn Sie zuerst [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufgerufen haben.

## <a name="related-topics"></a>Verwandte Themen

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Beispiel für einen Hintergrundtask code](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb des Prozesses](create-and-register-an-inproc-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Freigeben von Speicher, wenn die App in den Hintergrund verschoben wird](reduce-memory-usage.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Wie Sie auslösen, anhalten, fortsetzen und hintergrundereignissen in UWP-apps (beim debugging)](https://go.microsoft.com/fwlink/p/?linkid=254345)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Verschieben der angehaltenen App mithilfe der erweiterten Ausführung](run-minimized-with-extended-execution.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
