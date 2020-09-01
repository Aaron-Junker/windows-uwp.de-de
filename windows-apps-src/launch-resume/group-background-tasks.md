---
title: Registrieren von Gruppen-Hintergrundaufgaben
description: Registrieren/Aufheben der Registrierung von Hintergrundaufgaben als Teil einer Gruppe, um diese Registrierungen zu isolieren.
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, Hintergrundaufgabe
ms.openlocfilehash: 61419ac45acd27758e3c874ac4b03510561ccaf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155864"
---
# <a name="group-background-task-registration"></a>Registrieren von Gruppen-Hintergrundaufgaben

**Wichtige APIs**

[Backgroundtaskregistrationgroup-Klasse](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

Hintergrundaufgaben können jetzt in einer Gruppe registriert werden, die Sie als logischen Namespace betrachten können. Durch diese Isolation wird sichergestellt, dass die unterschiedlichen Komponenten einer APP oder unterschiedlicher Bibliotheken nicht die Registrierung der Hintergrundaufgaben der anderen Anwendung beeinträchtigen.

Wenn eine APP und das Framework (oder die Bibliothek) eine Hintergrundaufgabe mit dem gleichen Namen registriert, könnte die APP versehentlich die Hintergrund Task Registrierungen des Frameworks entfernen. App-Autoren können auch versehentlich Framework-und Bibliotheks Hintergrundaufgaben Registrierungen entfernen, da Sie die Registrierung aller registrierten Hintergrundaufgaben mithilfe von " [backgroundtaskregistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)" aufheben können.  Mit Gruppen können Sie Ihre Hintergrundaufgaben Registrierungen isolieren, damit dies nicht der Fall ist.

## <a name="features-of-groups"></a>Features von Gruppen

* Gruppen können durch eine GUID eindeutig identifiziert werden. Sie können auch eine zugeordnete Anzeige Name-Zeichenfolge aufweisen, die beim Debuggen leichter lesbar ist.
* Mehrere Hintergrundaufgaben können in einer Gruppe registriert werden.
* Hintergrundaufgaben, die in einer Gruppe registriert sind, werden nicht in [backgroundtaskregistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)angezeigt. Daher wird die Registrierung von Hintergrundaufgaben, die in einer Gruppe registriert sind, von apps, die derzeit **backgroundtaskregistration. AllTasks** verwenden, nicht versehentlich aufgehoben. Weitere Informationen zum Aufheben der Registrierung aller Hintergrund Trigger, die als Teil einer Gruppe registriert wurden, finden Sie unter Aufheben der Registrierung von [Hintergrund Tasks in einer Gruppe](#unregister-background-tasks-in-a-group) unten.
* Jede Hintergrundaufgaben Registrierung weist eine Group-Eigenschaft auf, um zu bestimmen, welcher Gruppe Sie zugeordnet ist.
* Das Registrieren von in-Process-Hintergrundaufgaben bei einer Gruppe führt dazu, dass die Aktivierung das [backgroundtaskregistrationgroup. backgroundaktivierte](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) Ereignis anstelle von " [Application. onbackgroundaktivi"](/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_)durchläuft.

## <a name="register-a-background-task-in-a-group"></a>Registrieren einer Hintergrundaufgabe in einer Gruppe

Das folgende Beispiel zeigt, wie Sie eine Hintergrundaufgabe (in diesem Beispiel durch eine Zeit Zonen Änderung ausgelöst) als Teil einer Gruppe registrieren.

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

Das folgende Beispiel zeigt, wie Sie die Registrierung von Hintergrundaufgaben aufheben, die als Teil einer Gruppe registriert wurden.
Da Hintergrundaufgaben, die in einer Gruppe registriert sind, nicht in [backgroundtaskregistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks)angezeigt werden, müssen Sie die Gruppen durchlaufen, die für jede Gruppe registrierten Hintergrundaufgaben suchen und deren Registrierung aufheben.

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

## <a name="register-persistent-events"></a>Permanente Ereignisse registrieren

Bei der Verwendung von Hintergrundaufgaben-Registrierungs Gruppen bei Prozess internen Hintergrundaufgaben werden die Hintergrund Aktivierungen an das Ereignis der Gruppe und nicht an das Objekt im Anwendungs-oder coreapplication-Objekt weitergeleitet. Dadurch können mehrere Komponenten in Ihrer APP die Aktivierung verarbeiten, anstatt alle Aktivierungs Codepfade im Anwendungs Objekt zu platzieren. Im folgenden wird gezeigt, wie Sie sich für das im Hintergrund aktivierte Ereignis der Gruppe registrieren. Überprüfen Sie zunächst [backgroundtaskregistration. gettaskgroup](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) , um zu ermitteln, ob die Gruppe bereits registriert wurde. Falls nicht, erstellen Sie eine neue Gruppe mit ihrer ID und dem anzeigen Amen. Registrieren Sie dann einen Ereignishandler für das backgroundaktivierte Ereignis in der Gruppe.

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