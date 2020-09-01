---
title: Ausführen einer Hintergrundaufgabe für einen Timer
description: Hier erfahren Sie, wie Sie eine einmalige Hintergrundaufgabe planen oder eine regelmäßige Hintergrundaufgabe ausführen.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: 7f9bf2ad18402976a7e089c2b2273e473819af1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175134"
---
# <a name="run-a-background-task-on-a-timer"></a>Ausführen einer Hintergrundaufgabe für einen Timer

Erfahren Sie, wie Sie mit dem [**time-**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger) Vorgang eine einmalige Hintergrundaufgabe planen oder eine periodische Hintergrundaufgabe ausführen.

Ein Beispiel für die Implementierung der in diesem Thema beschriebenen Hintergrundaufgabe finden Sie unter **Scenario4** im Beispiel für die [Hintergrund Aktivierung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) .

In diesem Thema wird davon ausgegangen, dass Sie über einen Hintergrund Task verfügen, der regelmäßig oder zu einem bestimmten Zeitpunkt ausgeführt werden muss. Wenn Sie noch nicht über eine Hintergrundaufgabe verfügen, finden Sie eine Beispiel-Hintergrundaufgabe unter [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Oder führen Sie die Schritte unter [Erstellen und Registrieren eines in-Process-Hintergrund](create-and-register-an-inproc-background-task.md) Tasks oder [Erstellen und Registrieren eines Out-of-Process-Hintergrund Tasks aus](create-and-register-a-background-task.md) , um einen zu erstellen.

## <a name="create-a-time-trigger"></a>Erstellen eines Zeitauslösers

Erstellen Sie einen neuen Zeitauslöser ([**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)). Der zweite Parameter (*OneShot*) gibt an, ob die Hintergrundaufgabe einmalig oder regelmäßig ausgeführt wird. Wenn *OneShot* auf „true“ festgelegt ist, gibt der erste Parameter (*FreshnessTime*) an, wie lange mit der Planung der Hintergrundaufgabe gewartet werden soll (in Minuten). Wenn *OneShot* auf „false“ festgelegt ist, gibt *FreshnessTime* die Häufigkeit an, mit der die Hintergrundaufgabe ausgeführt wird.

Der integrierte Timer für auf Desktop- oder Mobilgeräte ausgerichtete UWP-Apps führt Hintergrundaufgaben in einem 15-Minuten-Intervall aus. (Der Timer wird in 15-Minuten-Intervallen ausgeführt, sodass das System nur einmal alle 15 Minuten aktiviert werden muss, um die apps zu aktivieren, die timertriggers angefordert haben, wodurch Energie gespart wird.)

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

Sie können eine Hintergrundaufgaben Bedingung erstellen, um zu steuern, wann die Aufgabe ausgeführt wird. Eine Bedingung verhindert, dass die Hintergrundaufgabe ausgeführt wird, bis die Bedingung erfüllt ist. Weitere Informationen finden Sie unter [Festlegen von Bedingungen für das Ausführen eines Hintergrund](set-conditions-for-running-a-background-task.md)Tasks.

In diesem Beispiel ist die Bedingung auf **UserPresent** festgelegt, damit die ausgelöste Aufgabe nur ausgeführt wird, wenn der Benutzer aktiv ist. Eine Liste mit möglichen Bedingungen finden Sie in [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

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

Ausführlichere Informationen zu Bedingungen und Typen von Hintergrund Triggern finden [Sie unter unterstützen der APP mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Aufrufen von RequestAccessAsync()

Bevor Sie den **applicationlöst** -Hintergrund Task registrieren, müssen Sie [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen, um die Ebene der Hintergrund Aktivität zu ermitteln, die der Benutzer zulässt, da der Benutzer möglicherweise Hintergrund Aktivitäten für Ihre APP deaktiviert hat. Weitere Informationen zu den Möglichkeiten, mit denen Benutzer die Einstellungen für Hintergrund Aktivitäten steuern können, finden Sie unter [Optimieren der Hintergrund Aktivität](../debug-test-perf/optimize-background-activity.md) .

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe

Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben und zum Anzeigen der Definition der **registerbackgroundtask ()** -Methode im folgenden Beispielcode finden Sie unter [Registrieren eines Hintergrund](register-a-background-task.md)Tasks.

> [!IMPORTANT]
> Legen Sie für Hintergrundaufgaben, die in demselben Prozess wie Ihre APP ausgeführt werden, nicht fest `entryPoint` . Für Hintergrundaufgaben, die in einem separaten Prozess von ihrer app ausgeführt werden, legen Sie `entryPoint` auf den Namespace, "." und den Namen der Klasse fest, die die Implementierung der Hintergrundaufgabe enthält.

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

Um zu ermitteln, ob der Benutzer die Hintergrundaktivität Ihrer App begrenzt hat, verwenden Sie [BackgroundExecutionManager.RequestAccessAsync](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager). Achten Sie auf die Akkunutzung, und führen Sie Ihre App nur dann im Hintergrund aus, wenn dies zum Ausführen einer vom Benutzer gewünschten Aktion notwendig ist. Weitere Informationen zu den Möglichkeiten, mit denen Benutzer die Einstellungen für Hintergrund Aktivitäten steuern können, finden Sie unter [Optimieren der Hintergrund Aktivität](../debug-test-perf/optimize-background-activity.md) .

- Arbeitsspeicher: das Optimieren des Arbeitsspeichers und des Energieverbrauchs ihrer APP ist entscheidend, um sicherzustellen, dass das Betriebssystem die Durchführung ihrer Hintergrundaufgabe zulässt. Verwenden Sie die [Arbeitsspeicherverwaltungs-APIs](/uwp/api/windows.system.memorymanager) , um anzuzeigen, wie viel Arbeitsspeicher von ihrer Hintergrundaufgabe verwendet wird. Der mehr Arbeitsspeicher, den Ihre Hintergrundaufgabe verwendet, desto schwieriger ist es für das Betriebssystem, die Ausführung aufrechtzuerhalten, wenn sich eine andere APP im Vordergrund befindet. Der Benutzer hat letztendlich die Kontrolle über alle Hintergrundaktivitäten, die Ihre App ausführen kann, und kann die Auswirkungen Ihrer App auf den Akkuverbrauch erkennen.  
- CPU-Zeit: Hintergrundaufgaben werden durch die Menge an Zeit, die auf dem auslösertyp basieren, auf die Zeitspanne beschränkt.

Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Bemerkungen

Ab Windows 10 ist es nicht mehr erforderlich, dass der Benutzer die APP dem Sperrbildschirm hinzufügt, um Hintergrundaufgaben zu verwenden.

Ein Hintergrund Task wird nur mithilfe eines **time-Auslösers** ausgeführt, wenn Sie zuerst [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufgerufen haben.

## <a name="related-topics"></a>Zugehörige Themen

* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Codebeispiel für Hintergrundaufgabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md)
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