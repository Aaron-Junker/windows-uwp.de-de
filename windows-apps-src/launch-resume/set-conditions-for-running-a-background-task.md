---
title: Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe
description: Erfahren Sie, wie Bedingungen festgelegt werden, die steuern, wann die Hintergrundaufgabe ausgeführt wird.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, Uwp, Hintergrundaufgaben
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 8740829ab95b804afba564110f38116f2c90b416
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650935"
---
# <a name="set-conditions-for-running-a-background-task"></a>Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe

**Wichtige APIs**

- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
- [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Erfahren Sie, wie Bedingungen festgelegt werden, die steuern, wann die Hintergrundaufgabe ausgeführt wird.

Einige Hintergrundaufgaben müssen zudem noch bestimmte Bedingungen erfüllen, damit sie erfolgreich ausgeführt werden können. Wenn Sie Ihre Hintergrundaufgabe registrieren, können Sie mit [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) eine oder mehrere Bedingungen angeben. Die Bedingung wird geprüft, nachdem der Auslöser aktiviert wurde. Die Bedingung wird dann geprüft. Die Hintergrundaufgabe wird in die Warteschlange eingereiht und erst ausgeführt, wenn alle erforderlichen Bedingungen erfüllt sind.

Wenn Sie Bedingungen für Hintergrundaufgaben festlegen, schonen Sie Akku und CPU, da das unnötige Ausführen von Aufgaben verhindert wird. Wenn Ihre Hintergrundaufgabe z. B. nach einem Timer ausgeführt wird und eine Internetverbindung benötigt, fügen Sie die **InternetAvailable**-Bedingung zum [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) hinzu, bevor Sie die Aufgabe registrieren. So wird verhindert, dass die Aufgabe unnötig Systemressourcen nutzt und Akkulaufzeit beansprucht, da nur die Hintergrundaufgabe ausgeführt wird, wenn der Timer abgelaufen *und* das Internet verfügbar ist.

Sie können mehrere Bedingungen kombinieren, indem Sie **AddCondition** mehrmals für denselben [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) aufrufen. Achten Sie darauf, keine in Konflikt stehenden Bedingungen wie **UserPresent** und **UserNotPresent** hinzuzufügen.

## <a name="create-a-systemcondition-object"></a>Erstellen eines SystemCondition-Objekts

Dieses Thema setzt voraus, dass Sie Ihrer App bereits eine Hintergrundaufgabe zugeordnet haben und dass Ihre App Code enthält, der ein [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Objekt namens **taskBuilder** erstellt.  Wenn Sie zuerst eine Hintergrundaufgabe erstellen müssen, lesen Sie [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md) oder [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md).

Dieses Thema gilt für Hintergrundaufgaben, die in einem Prozess außerhalb von Prozessen ausgeführt werden, sowie für solche, die im gleichen Prozess wie die Vordergrund-App ausgeführt werden.

Erstellen Sie vor dem Hinzufügen der Bedingung ein [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)-Objekt, das die Bedingung darstellt, die zum Ausführen einer Hintergrundaufgabe erfüllt werden muss. Geben Sie im Konstruktor die zu erfüllende Bedingung mit einem [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)-Enumerationswert an.

Der folgende Code erstellt ein [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)-Objekt, das die **InternetAvailable**-Bedingung angibt:

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>Hinzufügen des SystemCondition-Objekts zur Hintergrundaufgabe

Um die Bedingung hinzuzufügen, rufen Sie die [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769)-Methode für das [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Objekt auf, und übergeben Sie das [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)-Objekt an die Methode.

Der folgende Code verwendet **TaskBuilder** zum Hinzufügen der **InternetAvailable**-Bedingung.

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>Registrieren Sie die Hintergrundaufgabe.

Sie können Ihre Hintergrundaufgabe jetzt mit der [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772)-Methode registrieren. Die Hintergrundaufgabe wird erst gestartet, wenn die angegebene Bedingung erfüllt ist.

Der folgende Code registriert die Aufgabe und speichert das resultierende BackgroundTaskRegistration-Objekt:

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> Universelle Windows-Apps müssen jedoch [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen, bevor Hintergrundtrigger-Typen registriert werden.

Rufen Sie [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) und anschließend [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

> [!NOTE]
> Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App problemlos mit Szenarien ohne erfolgreiche Registrierung von Hintergrundaufgaben zurechtkommt. Andernfalls stürzt die App unter Umständen ab, wenn sie so konzipiert ist, dass nach dem Versuch, eine Aufgabe zu registrieren, ein gültiges Registrierungsobjekt vorhanden sein muss.

## <a name="place-multiple-conditions-on-your-background-task"></a>Einfügen mehrerer Bedingungen für die Hintergrundaufgabe

Zum Hinzufügen mehrerer Bedingungen ruft Ihre App die [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) -Methode mehrmals auf. Diese Aufrufe müssen stattfinden, bevor die Aufgabenregistrierung wirksam wird.

> [!NOTE]
> Achten Sie darauf, dass Sie nicht in Konflikt stehenden Bedingungen hinzufügen, eine Hintergrundaufgabe.

Der folgende Codeausschnitt zeigt mehrere Bedingungen im Kontext des erstellen und eine Hintergrundaufgabe registrieren.

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>Hinweise

> [!NOTE]
> Wählen Sie die Bedingungen für den Hintergrundtask, sodass sie nur ausgeführt wird, wenn es erforderlich ist, und nicht ausgeführt werden, wenn es sollte nicht. Unter [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) finden Sie Beschreibungen der verschiedenen Bedingungen für Hintergrundaufgaben.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen Sie und registrieren Sie eine Out-of-Process-Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Erstellen Sie und registrieren Sie ein in-Process-Hintergrundaufgabe](create-and-register-an-inproc-background-task.md)
* [Deklarieren Sie Hintergrundtasks im Manifest Anwendung](declare-background-tasks-in-the-application-manifest.md)
* [Eine abgebrochene Hintergrundaufgabe verarbeiten](handle-a-cancelled-background-task.md)
* [Überwachen von Aufgabenstatus Hintergrund und Abschluss](monitor-background-task-progress-and-completion.md)
* [Eine Hintergrundaufgabe registrieren](register-a-background-task.md)
* [Reagieren Sie auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Live-Kachel aus einem Hintergrundtask aktualisiert](update-a-live-tile-from-a-background-task.md)
* [Verwenden Sie einen Wartungstrigger](use-a-maintenance-trigger.md)
* [Führen Sie eine Hintergrundaufgabe über einen timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Eine Hintergrundaufgabe Debuggen](debug-a-background-task.md)
* [Wie Sie auslösen, anhalten, fortsetzen und hintergrundereignissen in UWP-apps (beim debugging)](https://go.microsoft.com/fwlink/p/?linkid=254345)
