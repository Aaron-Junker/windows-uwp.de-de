---
title: Auslösen einer Hintergrundaufgabe in der App
description: Erfahren Sie, wie Sie mithilfe eines Anwendungs Auslösers einen Hintergrund Task ausführen, den Sie in Ihrer APP aktivieren möchten.
ms.date: 07/06/2018
ms.topic: article
keywords: Hintergrundaufgaben-, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: 446adc9921d2d124fb6e1304a06b70fa72da3d96
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155834"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Auslösen einer Hintergrundaufgabe in der App

Hier erfahren Sie, wie Sie [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) zum Aktivieren einer Hintergrundaufgabe in Ihrer App verwenden.

Ein Beispiel zum Erstellen eines Anwendungs Auslösers finden Sie in diesem [Beispiel](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

In diesem Thema wird davon ausgegangen, dass Sie über eine Hintergrundaufgabe verfügen, die Sie in Ihrer Anwendung aktivieren möchten. Wenn Sie noch nicht über eine Hintergrundaufgabe verfügen, finden Sie eine Beispiel-Hintergrundaufgabe unter [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Oder führen Sie die Schritte unter [Erstellen und Registrieren eines Out-of-Process-Hintergrund Tasks aus](create-and-register-a-background-task.md) , um einen zu erstellen.

## <a name="why-use-an-application-trigger"></a>Verwendung eines Anwendungs Auslösers

Verwenden Sie einen **Application-Auslösers** , um Code in einem separaten Prozess von der Vordergrund-App auszuführen. Ein **Application-Auslösers** ist geeignet, wenn Ihre APP überarbeiten verfügt, die im Hintergrund ausgeführt werden müssen, auch wenn der Benutzer die Vordergrund-APP schließt. Wenn Hintergrundarbeit angehalten werden soll, wenn die app geschlossen wird, oder wenn Sie an den Zustand des Vordergrund Prozesses gebunden werden soll, sollte stattdessen die [Erweiterte Ausführung](run-minimized-with-extended-execution.md) verwendet werden.

## <a name="create-an-application-trigger"></a>Erstellen eines Anwendungs Auslösers

Erstellen Sie einen neuen [Application-Auslösers](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Sie können Sie in einem Feld speichern, wie im folgenden Code Ausschnitt zu sehen. Dies ist der Zweck der einfacheren Verwendung, damit wir später keine neue Instanz erstellen müssen, wenn wir den-Wert signalisieren möchten. Sie können jedoch eine beliebige **applicationauslöst** -Instanz verwenden, um den-Aufruf zu signalisieren.

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

Sie können eine Hintergrundaufgaben Bedingung erstellen, um zu steuern, wann die Aufgabe ausgeführt wird. Eine Bedingung verhindert, dass die Hintergrundaufgabe ausgeführt wird, bis die Bedingung erfüllt ist. Weitere Informationen finden Sie unter [Festlegen von Bedingungen für das Ausführen eines Hintergrund](set-conditions-for-running-a-background-task.md)Tasks.

In diesem Beispiel wird die Bedingung auf **internettavailable** festgelegt, sodass die Aufgabe nach dem auslösen nur dann ausgeführt wird, wenn der Internet Zugriff verfügbar ist. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

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

Ausführlichere Informationen zu Bedingungen und Typen von Hintergrund Triggern finden [Sie unter unterstützen der APP mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Aufrufen von RequestAccessAsync()

Bevor Sie den **applicationlöst** -Hintergrund Task registrieren, müssen Sie [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen, um die Ebene der Hintergrund Aktivität zu ermitteln, die der Benutzer zulässt, da der Benutzer möglicherweise Hintergrund Aktivitäten für Ihre APP deaktiviert hat. Weitere Informationen zu den Möglichkeiten, mit denen Benutzer die Einstellungen für Hintergrund Aktivitäten steuern können, finden Sie unter [Optimieren der Hintergrund Aktivität](../debug-test-perf/optimize-background-activity.md) .

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe

Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben und zum Anzeigen der Definition der **registerbackgroundtask ()** -Methode im folgenden Beispielcode finden Sie unter [Registrieren eines Hintergrund](register-a-background-task.md)Tasks.

Wenn Sie die Lebensdauer Ihres Vordergrund Prozesses mithilfe eines Anwendungs Auslösers verlängern, erwägen Sie stattdessen die Verwendung der [erweiterten Ausführung](run-minimized-with-extended-execution.md) . Der Anwendungs--Auslösung dient zum Erstellen eines separat gehosteten Prozesses, in dem gearbeitet werden soll. Der folgende Code Ausschnitt registriert einen Out-of-Process-Hintergrund-Auslösers.

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

## <a name="trigger-the-background-task"></a>Löst die Hintergrundaufgabe aus.

Vergewissern Sie sich vor dem Ausführen der Hintergrundaufgabe mithilfe von [backgroundtaskregistration](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) , dass die Hintergrundaufgabe registriert ist. Ein guter Zeitpunkt, um zu überprüfen, ob alle Hintergrundaufgaben bei der APP-Start Registrierung registriert sind.

Löst die Hintergrundaufgabe durch Aufrufen von [applicationinsights. requestasync](/uwp/api/windows.applicationmodel.background.applicationtrigger)aus. Jede **Application-auslöserinstanz** wird durchführen.

Beachten Sie, dass " **applicationinsights. requestasync** " nicht von der Hintergrundaufgabe selbst aufgerufen werden kann oder wenn sich die APP im Hintergrund befindet (Weitere Informationen zu Anwendungs Zuständen finden Sie unter [App-Lebenszyklus](app-lifecycle.md) ).
Es kann " [disabledbypolicy](/uwp/api/windows.applicationmodel.background.applicationtriggerresult) " zurückgeben, wenn der Benutzer Energie-oder Datenschutzrichtlinien festgelegt hat, die verhindern, dass die APP Hintergrund Aktivitäten ausführt.
Außerdem kann jeweils nur ein App-Ereignis ausgeführt werden. Wenn Sie versuchen, einen apptriggervorgang auszuführen, während ein anderer bereits ausgeführt wird, gibt die Funktion " [currentlyrunning](/uwp/api/windows.applicationmodel.background.applicationtriggerresult)" zurück.

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Verwalten von Ressourcen für Ihre Hintergrundaufgabe

Um zu ermitteln, ob der Benutzer die Hintergrundaktivität Ihrer App begrenzt hat, verwenden Sie [BackgroundExecutionManager.RequestAccessAsync](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager). Achten Sie auf die Akkunutzung, und führen Sie Ihre App nur dann im Hintergrund aus, wenn dies zum Ausführen einer vom Benutzer gewünschten Aktion notwendig ist. Weitere Informationen zu den Möglichkeiten, mit denen Benutzer die Einstellungen für Hintergrund Aktivitäten steuern können, finden Sie unter [Optimieren der Hintergrund Aktivität](../debug-test-perf/optimize-background-activity.md) .  

- Arbeitsspeicher: das Optimieren des Arbeitsspeichers und des Energieverbrauchs ihrer APP ist entscheidend, um sicherzustellen, dass das Betriebssystem die Durchführung ihrer Hintergrundaufgabe zulässt. Verwenden Sie die [Arbeitsspeicherverwaltungs-APIs](/uwp/api/windows.system.memorymanager) , um anzuzeigen, wie viel Arbeitsspeicher von ihrer Hintergrundaufgabe verwendet wird. Der mehr Arbeitsspeicher, den Ihre Hintergrundaufgabe verwendet, desto schwieriger ist es für das Betriebssystem, die Ausführung aufrechtzuerhalten, wenn sich eine andere APP im Vordergrund befindet. Der Benutzer hat letztendlich die Kontrolle über alle Hintergrundaktivitäten, die Ihre App ausführen kann, und kann die Auswirkungen Ihrer App auf den Akkuverbrauch erkennen.  
- CPU-Zeit: Hintergrundaufgaben werden durch die Menge an Zeit, die auf dem auslösertyp basieren, auf die Zeitspanne beschränkt. Hintergrundaufgaben, die vom Anwendungs Trigger ausgelöst werden, sind auf ungefähr 10 Minuten beschränkt.

Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Bemerkungen

Ab Windows 10 ist es nicht mehr erforderlich, dass der Benutzer die APP dem Sperrbildschirm hinzufügt, um Hintergrundaufgaben zu verwenden.

Ein Hintergrund Task wird nur mithilfe eines **Application-Auslösers** ausgeführt, wenn Sie zuerst " [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) " aufgerufen haben.

## <a name="related-topics"></a>Zugehörige Themen

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Codebeispiel für Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Erstellen und registrieren Sie eine Prozess interne Hintergrundaufgabe](create-and-register-an-inproc-background-task.md).
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Geben Sie Speicher frei, wenn Ihre App in den Hintergrund verschoben wird](reduce-memory-usage.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](/previous-versions/hh974425(v=vs.110))
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Verschieben der angehaltenen App mithilfe der erweiterten Ausführung](run-minimized-with-extended-execution.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)