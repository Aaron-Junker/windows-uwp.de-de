---
title: Ausführen einer Hintergrundaufgabe für einen Timer
description: Hier erfahren Sie, wie Sie eine einmalige Hintergrundaufgabe planen oder eine regelmäßige Hintergrundaufgabe ausführen.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: b0d3c9401ff71475e379b2959a1f0cdc03fe8d8b
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260432"
---
# <a name="run-a-background-task-on-a-timer"></a>Ausführen einer Hintergrundaufgabe für einen Timer

Hier erfahren Sie, wie Sie mit dem [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger) eine einmalige Hintergrundaufgabe planen oder eine regelmäßige Hintergrundaufgabe ausführen.

Unter **Scenario4** im [Hintergrundaktivierungsbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) finden Sie ein Beispiel dafür, wie Sie die zu bestimmten Zeiten ausgelöste Hintergrundaufgabe implementieren, die in diesem Thema beschrieben wird.

In diesem Thema wird davon ausgegangen, dass eine Hintergrundaufgabe regelmäßig oder zu einer bestimmten Uhrzeit ausgeführt werden muss. Wenn Sie noch keine Hintergrundaufgabe haben, finden Sie unter [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs) ein Beispiel für eine Hintergrundaufgabe. Sie können alternativ die Schritte in [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md) oder [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md) befolgen, um eine Hintergrundaufgabe zu erstellen.

## <a name="create-a-time-trigger"></a>Erstellen eines Zeittriggers

Erstellen Sie einen neuen Zeitauslöser ([**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)). Der zweite Parameter (*OneShot*) gibt an, ob die Hintergrundaufgabe einmalig oder regelmäßig ausgeführt wird. Wenn *OneShot* auf „true“ festgelegt ist, gibt der erste Parameter (*FreshnessTime*) an, wie lange mit der Planung der Hintergrundaufgabe gewartet werden soll (in Minuten). Wenn *OneShot* auf „false“ festgelegt ist, gibt *FreshnessTime* die Häufigkeit an, mit der die Hintergrundaufgabe ausgeführt wird.

Der integrierte Timer für auf Desktop- oder Mobilgeräte ausgerichtete UWP-Apps führt Hintergrundaufgaben in einem 15-Minuten-Intervall aus. (Der Timer wird in 15-Minuten-Intervallen ausgeführt, sodass das System nur einmal alle 15 Minuten reaktiviert werden muss, um die Apps zu reaktivieren, die TimerTriggers angefordert haben. Dadurch wird Strom gespart.)

- Wenn *FreshnessTime* auf 15 Minuten und *OneShot* auf „true“ festgelegt ist, wird die Aufgabe einmalig innerhalb der ersten 15 und 30 Minuten nach dem Registrierungszeitpunkt zur Ausführung eingeplant. Wenn sie auf 25 Minuten und *OneShot* auf „true“ festgelegt ist, wird die Aufgabe einmalig innerhalb der ersten 25 und 40 Minuten nach dem Registrierungszeitpunkt zur Ausführung eingeplant.

- Wenn *FreshnessTime* auf 15 Minuten und *OneShot* auf „false“ festgelegt ist, wird die Aufgabe alle 15 Minuten innerhalb der ersten 15 und 30 Minuten nach dem Registrierungszeitpunkt zur Ausführung eingeplant. Wenn sie auf „n” Minuten und *OneShot* auf „false“ festgelegt ist, wird die Aufgabe alle „n” Minuten innerhalb der ersten „n” und „n+15” Minuten nach dem Registrierungszeitpunkt zur Ausführung eingeplant.

> [!NOTE]
> Wenn *freshnesstime* auf weniger als 15 Minuten festgelegt ist, wird beim Versuch, die Hintergrundaufgabe zu registrieren, eine Ausnahme ausgelöst.

Beispielsweise bewirkt dieser auslösen, dass eine Hintergrundaufgabe einmal pro Stunde ausgeführt wird.

```cs
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
```

```cppwinrt
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };
```

```cpp
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
```

## <a name="optional-add-a-condition"></a>(Optional) Hinzufügen einer Bedingung

Sie können eine Hintergrundaufgabenbedingung erstellen, um zu steuern, wann die Aufgabe ausgeführt wird. Eine Bedingung verhindert, dass die Hintergrundaufgabe ausgeführt wird, bis die Bedingung erfüllt ist. Weitere Informationen finden Sie unter [Festlegen von Bedingungen für die Ausführung einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md).

In diesem Beispiel ist die Bedingung auf **UserPresent** festgelegt, damit die ausgelöste Aufgabe nur ausgeführt wird, wenn der Benutzer aktiv ist. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

```cs
SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
```

```cpp
SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent);
```

Ausführlichere Informationen zu Bedingungen und Arten von Hintergrundtriggern finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Aufrufen von RequestAccessAsync()

Rufen Sie vor dem Registrieren der **ApplicationTrigger**-Hintergrundaufgabe [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) auf, um die Ebene der Hintergrundaktivitäten zu bestimmen, die der Benutzer erlaubt, da der Benutzer Hintergrundaktivitäten für Ihre App evtl. deaktiviert hat. Weitere Informationen zu den Möglichkeiten, die Benutzer haben, um die Einstellungen für Hintergrundaktivitäten zu steuern, finden Sie unter [Optimieren von Hintergrundaktivitäten](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe

Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben sowie die Definition der **RegisterBackgroundTask()** -Methode im folgenden Beispielcode finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

> [!IMPORTANT]
> Legen Sie für Hintergrundaufgaben, die in demselben Prozess wie Ihre APP ausgeführt werden, nicht `entryPoint`fest. Legen Sie für Hintergrundaufgaben, die in einem separaten Prozess von ihrer app ausgeführt werden, `entryPoint` auf den Namespace, "." und den Namen der Klasse fest, die die Implementierung der Hintergrundaufgabe enthält.

```cs
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example hourly background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example hourly background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example hourly background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App problemlos mit Szenarien ohne erfolgreiche Registrierung von Hintergrundaufgaben zurechtkommt. Andernfalls stürzt die App unter Umständen ab, wenn sie so konzipiert ist, dass nach dem Versuch, eine Aufgabe zu registrieren, ein gültiges Registrierungsobjekt vorhanden sein muss.

## <a name="manage-resources-for-your-background-task"></a>Verwalten von Ressourcen für Ihre Hintergrundaufgabe

Um zu ermitteln, ob der Benutzer die Hintergrundaktivität Ihrer App begrenzt hat, verwenden Sie [BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager). Achten Sie auf die Akkunutzung, und führen Sie Ihre App nur dann im Hintergrund aus, wenn dies zum Ausführen einer vom Benutzer gewünschten Aktion notwendig ist. Weitere Informationen zu den Möglichkeiten, die Benutzer haben, um die Einstellungen für Hintergrundaktivitäten zu steuern, finden Sie unter [Optimieren von Hintergrundaktivitäten](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).

- Arbeitsspeicher: Eine Optimierung der Speicherverwendung und des Energieverbrauchs Ihrer App ist wichtig, um sicherzustellen, dass das Betriebssystem die Ausführung Ihrer Hintergrundaufgabe erlaubt. Um zu ermitteln, wie viel Arbeitsspeicher Ihre Hintergrundaufgabe verwendet, verwenden Sie die [Speicherverwaltungs-APIs](https://docs.microsoft.com/uwp/api/windows.system.memorymanager). Je mehr Arbeitsspeicher Ihre Hintergrundaufgabe verwendet, desto schwieriger wird es für das Betriebssystem, sie weiter auszuführen, wenn sich eine andere App im Vordergrund befindet. Der Benutzer hat letztendlich die Kontrolle über alle Hintergrundaktivitäten, die Ihre App ausführen kann, und kann die Auswirkungen Ihrer App auf den Akkuverbrauch erkennen.  
- CPU-Zeit: Hintergrundaufgaben sind durch die Gesamtbetrachtungszeit eingeschränkt, die ihnen basierend auf dem Triggertyp zugeteilt wird.

Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Hinweise

Ab Windows 10 ist es nicht mehr erforderlich, dass der Benutzer die APP dem Sperrbildschirm hinzufügt, um Hintergrundaufgaben zu verwenden.

Eine Hintergrundaufgabe wird nur mithilfe eines **TimeTrigger** ausgeführt, wenn Sie zuerst [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufgerufen haben.

## <a name="related-topics"></a>Verwandte Themen

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Codebeispiel für Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Freigeben von Speicher, wenn die App in den Hintergrund verschoben wird](reduce-memory-usage.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Verschieben der angehaltenen App mithilfe der erweiterten Ausführung](run-minimized-with-extended-execution.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
