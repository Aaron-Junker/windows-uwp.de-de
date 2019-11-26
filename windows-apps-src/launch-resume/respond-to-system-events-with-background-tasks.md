---
title: Reagieren auf Systemereignisse mit Hintergrundaufgaben
description: Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen können, die auf SystemTrigger-Ereignisse reagiert.
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 4b704a83fbcf948f2c9377334831ca8948fc0e1a
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260447"
---
# <a name="respond-to-system-events-with-background-tasks"></a>Reagieren auf Systemereignisse mit Hintergrundaufgaben

**Wichtige APIs**

- [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
- [**System Trigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger)

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen können, die auf [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)-Ereignisse reagiert.

Dieses Thema setzt voraus, dass Sie für Ihre App eine Hintergrundaufgabenklasse geschrieben haben und dass diese Aufgabe als Reaktion auf ein vom System ausgelöstes Ereignis ausgeführt werden muss, z. B. wenn sich die Internetverfügbarkeit ändert oder sich der Benutzer anmeldet. Der Schwerpunkt dieses Themas liegt auf der [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)-Klasse. Weitere Informationen zum Schreiben einer Hintergrundaufgabenklasse finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb des Prozesses](create-and-register-an-inproc-background-task.md) oder [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb des Prozesses](create-and-register-a-background-task.md).

## <a name="create-a-systemtrigger-object"></a>Erstellen eines SystemTrigger-Objekts

Erstellen Sie in Ihrem App-Code ein neues [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger)-Objekt. *triggerType* ist der erste Parameter und gibt den Typ des Systemereignis-Triggers zur Aktivierung der Hintergrundaufgabe an. Eine Liste mit Ereignistypen finden Sie unter [**SystemTriggerType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType).

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
> Universelle Windows-Plattform-apps müssen [**requestaccessasync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen, bevor Sie einen der Hintergrund Auslösertypen registrieren.

Rufen Sie [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) und anschließend [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

> [!NOTE]
> Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App problemlos mit Szenarien ohne erfolgreiche Registrierung von Hintergrundaufgaben zurechtkommt. Andernfalls stürzt die App unter Umständen ab, wenn sie so konzipiert ist, dass nach dem Versuch, eine Aufgabe zu registrieren, ein gültiges Registrierungsobjekt vorhanden sein muss.
 
## <a name="remarks"></a>Hinweise

Laden Sie das [Beispiel zu Hintergrundaufgaben](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) herunter, um die Registrierung der Hintergrundaufgabe in Aktion zu sehen.

Hintergrundaufgaben können als Reaktion auf die Ereignisse [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) und [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) ausgeführt werden. Dennoch ist das [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md) erforderlich. Vor dem Registrieren einer Hintergrundaufgabe müssen Sie außerdem [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen.

Apps können Hintergrundaufgaben registrieren, die auf die Ereignisse [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger), [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) und [**NetworkOperatorNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.NetworkOperatorNotificationTrigger) reagieren. So können die Apps eine Echtzeitkommunikation mit dem Benutzer bereitstellen, auch wenn die App sich nicht im Vordergrund befindet. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

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
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
