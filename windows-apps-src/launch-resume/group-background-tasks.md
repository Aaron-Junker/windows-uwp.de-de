---
title: Registrieren von Gruppen-Hintergrundaufgaben
description: Registrieren/Aufheben der Registrierung von Hintergrundaufgaben als Teil einer Gruppe, um diese Registrierungen zu isolieren.
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, Hintergrundaufgaben
ms.openlocfilehash: a70c814e5e35359746076c5418d1f1d973e61773
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623855"
---
# <a name="group-background-task-registration"></a>Registrieren von Gruppen-Hintergrundaufgaben

**Wichtige APIs**

[BackgroundTaskRegistrationGroup-Klasse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

Hintergrundaufgaben können jetzt in einer Gruppe registriert werden, die Sie sich als logischen Namensraum vorstellen können. Durch diese Isolation kann sichergestellt werden, dass sich die einzelnen Komponenten einer App bzw. die verschiedenen Bibliotheken nicht gegenseitig bei der Registrierung von Hintergrundaufgaben beeinträchtigen.

Wenn eine App und das von ihr verwendete Framework (bzw. die Bibliothek) eine gleichlautende Hintergrundaufgabe registrieren, könnte die App versehentlich die Registrierung der Hintergrundaufgabe des Frameworks entfernt. Zudem könnten App-Autoren versehentlich die Registrierungen von Hintergrundaufgaben des Frameworks oder der Bibliothek entfernen, da diese mittels [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) die Registrierung alle registrierten Hintergrundaufgaben aufheben könnten.  Mithilfe von Gruppen können Sie die Registrierung Ihrer Hintergrundaufgaben isolieren, um dies zu verhindern.

## <a name="features-of-groups"></a>Funktionen von Gruppen

* Gruppen können durch eine GUID eindeutig identifiziert werden. Ihnen kann zudem auch ein Anzeigenamen zugeordnet sein, der beim Debugging für eine bessere Lesbarkeit sorgt.
* In einer Gruppe können mehrere Hintergrundaufgaben registriert werden.
* In einer Gruppe registrierte Hintergrundaufgaben werden unter [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) nicht angezeigt. Somit können Apps, die derzeit mittels **BackgroundTaskRegistration.AllTasks** die Registrierung ihrer Aufgaben aufheben, die Registrierung von Hintergrundaufgaben, die in einer Gruppe registriert sind, nicht versehentlich aufheben. Finden Sie unter [Aufheben der Registrierung Hintergrundaufgaben in einer Gruppe](#unregister-background-tasks-in-a-group) unten, um Informationen zum Aufheben der Registrierung alle Hintergrund-Trigger, die als Teil einer Gruppe registriert wurden.
* Jeder Registrierung einer Hintergrundaufgabe umfasst eine Gruppen-Eigenschaft, die bestimmt, welcher Gruppe sie zugeordnet ist.
* Registrieren von In-Process-Hintergrundaufgaben mit einer Gruppe führt dazu, dass die Aktivierung über [BackgroundTaskRegistrationGroup.BackgroundActivated](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) Ereignis anstelle von [Application.OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_).

## <a name="register-a-background-task-in-a-group"></a>Registrieren einer Hintergrundaufgabe in einer Gruppe

Nachfolgend ein Beispiel für die Registrierung einer Hintergrundaufgabe (hier ausgelöst durch eine Änderung der Zeitzone) als Teil einer Gruppe.

```csharp
private const string groupFriendlyName = "myGroup";
private const string groupId = "3F2504E0-4F89-41D3-9A0C-0305E82C3301";
private const string myTaskName = "My Background Trigger";

public static void RegisterBackgroundTaskInGroup()
{
   BackgroundTaskRegistrationGroup group = BackgroundTaskRegistration.GetTaskGroup(groupId);
   bool isTaskRegistered = false;

   // See if this task already belongs to a group
   if (group != null)
   {
       foreach (var taskKeyValue in group.AllTasks)
       {
           if (taskKeyValue.Value.Name == myTaskName)
           {
               isTaskRegistered = true;
               break;
           }
       }
   }

   // If the background task is not in a group, register it
   if (!isTaskRegistered)
   {
       if (group == null)
       {
           group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
       }

       var builder = new BackgroundTaskBuilder();
       builder.Name = myTaskName;
       builder.TaskGroup = group; // we specify the group, here
       builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));

       // Because builder.TaskEntryPoint is not specified, OnBackgroundActivated() will be raised when the background task is triggered
       BackgroundTaskRegistration task = builder.Register();
   }
}
```

## <a name="unregister-background-tasks-in-a-group"></a>Aufheben der Registrierung von Hintergrundaufgaben in einer Gruppe

Nachfolgend ein Beispiel für die Aufhebung der Registrierung von Hintergrundaufgaben, die als Teil einer Gruppe registriert wurden.
Da in einer Gruppe registrierte Hintergrundaufgaben nicht in [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks) angezeigt werden, müssen Sie die Gruppen wiederholt durchlaufen, um die für jede Gruppe registrierten Hintergrundaufgaben zu finden und deren Registrierung aufzuheben.

```csharp
private static void UnRegisterAllTasks()
{
    // Unregister tasks that are part of a group
    foreach (var groupKeyValue in BackgroundTaskRegistration.AllTaskGroups)
    {
        foreach (var groupedTask in groupKeyValue.Value.AllTasks)
        {
            groupedTask.Value.Unregister(true); // passing true to cancel currently running instances of this background task
        }
    }

    // Unregister tasks that aren't part of a group
    foreach(var taskKeyValue in BackgroundTaskRegistration.AllTasks)
    {
        taskKeyValue.Value.Unregister(true); // passing true to cancel currently running instances of this background task
    }
}
```

## <a name="register-persistent-events"></a>Dauerhafte Ereignisse registrieren

Wenn Sie Gruppen für die Registrierung der Hintergrundaufgaben von In-Process-Hintergrundaufgaben verwenden, werden die Hintergrundaktivierungen anstatt an das Anwendungs- bzw. CoreApplication-Objekt an das Gruppenereignis gerichtet. Dadurch kann die Aktivierung Ihrer App von verschiedenen Komponenten übernommen werden, da die Pfade sämtlicher Aktivierungscodes nicht im App-Objekt platziert werden müssen. Nachfolgend ein Beispiel für die Registrierung von im Hintergrund aktivierten Ereignissen einer Gruppe. Überprüfen Sie zunächst die [BackgroundTaskRegistration.GetTaskGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup), um zu ermitteln, ob die Gruppe bereits registriert wurde. Ist dies nicht der Fall, erstellen Sie eine neue Gruppe mit Ihrer ID und einem Anzeigenamen. Registrieren Sie anschließend in dieser Gruppe einen Ereignis-Handler für das Ereignis BackgroundActivated.

```csharp
void RegisterPersistentEvent()
{
    var group = BackgroundTaskRegistration.GetTaskGroup(groupId);
    if (group == null)
    {
        group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
    }

    group.BackgroundActivated += MyEventHandler;
}
```
