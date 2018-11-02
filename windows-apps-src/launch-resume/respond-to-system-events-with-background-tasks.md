---
author: TylerMSFT
title: Reagieren auf Systemereignisse mit Hintergrundaufgaben
description: Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen können, die auf SystemTrigger-Ereignisse reagiert.
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, Uwp, Hintergrundaufgabe, für die
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: f65751e894d45bab5b39382c7bec43d5f06f18b2
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "5944300"
---
# <a name="respond-to-system-events-with-background-tasks"></a>Reagieren auf Systemereignisse mit Hintergrundaufgaben

**Wichtige APIs**

- [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
- [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen können, die auf [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)-Ereignisse reagiert.

Dieses Thema setzt voraus, dass Sie für Ihre App eine Hintergrundaufgabenklasse geschrieben haben und dass diese Aufgabe als Reaktion auf ein vom System ausgelöstes Ereignis ausgeführt werden muss, z.B. wenn sich die Internetverfügbarkeit ändert oder sich der Benutzer anmeldet. Der Schwerpunkt dieses Themas liegt auf der [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)-Klasse. Weitere Informationen zum Schreiben einer Hintergrundaufgabenklasse finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb des Prozesses](create-and-register-an-inproc-background-task.md) oder [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb des Prozesses](create-and-register-a-background-task.md).

## <a name="create-a-systemtrigger-object"></a>Erstellen eines SystemTrigger-Objekts

Erstellen Sie in Ihrem App-Code ein neues [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)-Objekt. *triggerType* ist der erste Parameter und gibt den Typ des Systemereignis-Triggers zur Aktivierung der Hintergrundaufgabe an. Eine Liste mit Ereignistypen finden Sie unter [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839).

Der zweite Parameter (*OneShot*) gibt an, ob die Hintergrundaufgabe nur einmal ausgeführt wird, wenn das Systemereignis das nächste Mal eintritt, oder ob die Hintergrundaufgabe beim Auslösen des Systemereignisses immer ausgeführt wird, bis die Registrierung der Aufgabe aufgehoben wird.

Der folgende Code gibt an, dass die Hintergrundaufgabe immer ausgeführt wird, wenn das Internet verfügbar wird:

```csharp
SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemTrigger internetTrigger{
    Windows::ApplicationModel::Background::SystemTriggerType::InternetAvailable, false};
```

```cpp
SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
```

## <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe

Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

Der folgende Code registriert die Hintergrundaufgabe für einen Hintergrundprozess, der außerhalb des Prozesses ausgeführt wird. Wenn Sie eine Hintergrundaufgabe aufrufen, die im gleichen Prozess wie die Host-App ausgeführt wird, legen Sie nicht `entrypoint` fest:

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass"; // Namespace name, '.', and the name of the class containing the background task
string taskName   = "Internet-based background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" }; // don't set for in-process background tasks.
std::wstring taskName{ L"Internet-based background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass"; // don't set for in-process background tasks
String ^ taskName   = "Internet-based background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

> [!NOTE]
> Universelle Windows-Plattform-apps müssen [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen, bevor hintergrundtrigger-Typen.

Rufen Sie [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) und anschließend [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

> [!NOTE]
> Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App Szenarien, in denen die Registrierung von Hintergrundaufgaben einen Fehler verursacht, problemlos verarbeitet.
 
## <a name="remarks"></a>Anmerkungen

Laden Sie das [Beispiel zu Hintergrundaufgaben](http://go.microsoft.com/fwlink/p/?LinkId=618666) herunter, um die Registrierung der Hintergrundaufgabe in Aktion zu sehen.

Hintergrundaufgaben können als Reaktion auf die Ereignisse [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) und [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) ausgeführt werden. Dennoch ist das [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md) erforderlich. Vor dem Registrieren einer Hintergrundaufgabe müssen Sie außerdem [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen.

Apps können Hintergrundaufgaben registrieren, die auf die Ereignisse [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) und [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831) reagieren. So können die Apps eine Echtzeitkommunikation mit dem Benutzer bereitstellen, auch wenn die App sich nicht im Vordergrund befindet. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird’s gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in UWP-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)
