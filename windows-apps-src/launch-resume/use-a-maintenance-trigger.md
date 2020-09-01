---
title: Verwenden eines Wartungsauslösers
description: Hier erfahren Sie, wie Sie die MaintenanceTrigger-Klasse zum Ausführen von einfachem Code im Hintergrund verwenden, während das Gerät eingesteckt ist.
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: c390280e084cd8557633f591b1686a0550f6a656
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155764"
---
# <a name="use-a-maintenance-trigger"></a>Verwenden eines Wartungsauslösers

**Wichtige APIs**

- [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger)
- [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
- [**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition)

Erfahren Sie, wie Sie mit der [**Maintenance-**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) Klasse im Hintergrund Lightweight-Code ausführen, während das Gerät angeschlossen ist.

## <a name="create-a-maintenance-trigger-object"></a>Erstellen eines MaintenanceTrigger-Objekts

In diesem Beispiel wird davon ausgegangen, dass Sie über einfachen Code verfügen, den Sie im Hintergrund ausführen können, um Ihre App zu optimieren, wenn das Gerät eingesteckt ist. Der Schwerpunkt dieses Themas liegt auf der [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger)-Klasse, die der [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)-Klasse ähnelt.

Weitere Informationen zum Schreiben einer Hintergrundaufgabenklasse finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb des Prozesses](create-and-register-an-inproc-background-task.md) oder [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb des Prozesses](create-and-register-a-background-task.md).

Erstellen Sie ein neues [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger)-Objekt. Der zweite Parameter *OneShot* gibt an, ob die Wartungsaufgabe nur einmal oder regelmäßig ausgeführt wird. Wenn *OneShot* auf „true“ festgelegt ist, gibt der erste Parameter (*FreshnessTime*) an, wie lange mit der Planung der Hintergrundaufgabe gewartet werden soll (in Minuten). Wenn *OneShot* auf „false“ festgelegt ist, gibt *FreshnessTime* an, wie oft die Hintergrundaufgabe ausgeführt wird.

> [!NOTE]
> Wenn *freshnesstime* auf weniger als 15 Minuten festgelegt ist, wird beim Versuch, die Hintergrundaufgabe zu registrieren, eine Ausnahme ausgelöst.

Dieser Beispielcode erstellt einen-Triggerwert, der einmal pro Stunde ausgeführt wird.

```csharp
uint waitIntervalMinutes = 60;
MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
```

```cppwinrt
uint32_t waitIntervalMinutes{ 60 };
Windows::ApplicationModel::Background::MaintenanceTrigger taskTrigger{ waitIntervalMinutes, false };
```

```cpp
unsigned int waitIntervalMinutes = 60;
MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
```

## <a name="optional-add-a-condition"></a>(Optional) Hinzufügen einer Bedingung

- Erstellen Sie bei Bedarf eine Hintergrundaufgabenbedingung, um zu steuern, wann die Aufgabe ausgeführt wird. Eine Bedingung sorgt dafür, dass die Hintergrundaufgabe erst ausgeführt wird, wenn die Bedingung erfüllt ist. Weitere Informationen finden Sie unter [Festlegen von Bedingungen für die Ausführung einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md).

In diesem Beispiel wird die Bedingung auf **InternetAvailable** festgelegt, damit die Wartung ausgeführt wird, wenn eine Internetverbindung verfügbar ist (oder wird). Eine Liste mit möglichen Hintergrundaufgabenbedingungen finden Sie unter [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

Der folgende Code fügt dem Hintergrundaufgaben-Generator eine Bedingung hinzu:

```csharp
SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition exampleCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="register-the-background-task"></a>Registrieren der Hintergrundaufgabe

- Registrieren Sie die Hintergrundaufgabe, indem Sie die Funktion zum Registrieren der Hintergrundaufgabe aufrufen. Weitere Informationen zum Registrieren von Hintergrundaufgaben finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

Der folgende Code registriert die Wartungsaufgabe. Beachten Sie, dass davon ausgegangen wird, dass die Hintergrundaufgaben in einem von Ihrer App getrennten Prozess ausgeführt wird, da `entryPoint` angegeben wird. Wenn die Hintergrundaufgabe in demselben Prozess wie Ihre App ausgeführt wird, wird `entryPoint` nicht angegeben.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Maintenance background task example";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Maintenance background task example" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Maintenance background task example";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

> [!NOTE]
> Für alle Gerätefamilien mit Ausnahme von Desktops können Hintergrundaufgaben beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. Wenn eine Ausnahme über wenig Arbeitsspeicher nicht angezeigt oder von der App nicht behandelt wird, wird die Hintergrundaufgabe ohne Warnung und ohne Auslösen des OnCanceled-Ereignisses beendet. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt werden kann.

> [!NOTE]
> Universelle Windows-Plattform-apps müssen [**requestaccessasync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen, bevor Sie einen der Hintergrund Auslösertypen registrieren.

Rufen Sie [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) und anschließend [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates der App weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

> [!NOTE]
> Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App problemlos mit Szenarien ohne erfolgreiche Registrierung von Hintergrundaufgaben zurechtkommt. Andernfalls stürzt die App unter Umständen ab, wenn sie so konzipiert ist, dass nach dem Versuch, eine Aufgabe zu registrieren, ein gültiges Registrierungsobjekt vorhanden sein muss.

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und registrieren Sie eine Prozess interne Hintergrundaufgabe](create-and-register-an-inproc-background-task.md).
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](/previous-versions/hh974425(v=vs.110))