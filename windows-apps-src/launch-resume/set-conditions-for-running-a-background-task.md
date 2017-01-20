---
author: TylerMSFT
title: "Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe"
description: "Erfahren Sie, wie Bedingungen festgelegt werden, die steuern, wann die Hintergrundaufgabe ausgeführt wird."
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
translationtype: Human Translation
ms.sourcegitcommit: ea862ef33f58b33b70318ddfc1d09d9aca9b3517
ms.openlocfilehash: c83f861f43209c42dff661e3277e1d8a1b67d37c

---

# <a name="set-conditions-for-running-a-background-task"></a>Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Wichtige APIs**

-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
-   [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Erfahren Sie, wie Bedingungen festgelegt werden, die steuern, wann die Hintergrundaufgabe ausgeführt wird.

Einige Hintergrundaufgaben, die durch ein Ereignis ausgelöst werden, müssen zudem noch bestimmte Bedingungen erfüllen, damit sie erfolgreich ausgeführt werden können. Wenn Sie Ihre Hintergrundaufgabe registrieren, können Sie mit [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) eine oder mehrere Bedingungen angeben. Die Bedingung wird geprüft, nachdem der Auslöser aktiviert wurde. Die Hintergrundaufgabe wird in die Warteschlange eingereiht und erst ausgeführt, wenn alle erforderlichen Bedingungen erfüllt sind.

Wenn Sie Bedingungen für Hintergrundaufgaben festlegen, schonen Sie Akku und CPU-Laufzeit, da das unnötige Ausführen von Aufgaben verhindert wird. Wenn Ihre Hintergrundaufgabe z. B. nach einem Timer ausgeführt wird und eine Internetverbindung benötigt, fügen Sie die **InternetAvailable**-Bedingung zum [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) hinzu, bevor Sie die Aufgabe registrieren. So wird verhindert, dass die Aufgabe unnötig Systemressourcen nutzt und Akkulaufzeit beansprucht, da nur die Hintergrundaufgabe ausgeführt wird, wenn der Timer abgelaufen *und* das Internet verfügbar ist.

Sie können auch mehrere Bedingungen kombinieren, indem Sie AddCondition mehrmals für denselben [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) aufrufen. Achten Sie darauf, keine in Konflikt stehenden Bedingungen wie **UserPresent** und **UserNotPresent** hinzuzufügen.

## <a name="create-a-systemcondition-object"></a>Erstellen eines SystemCondition-Objekts

Dieses Thema setzt voraus, dass Sie Ihrer App bereits eine Hintergrundaufgabe zugeordnet haben und dass Ihre App Code enthält, der ein [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Objekt namens **taskBuilder** erstellt.  Wenn Sie zuerst eine Hintergrundaufgabe erstellen müssen, lesen Sie [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md) oder [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md).

Dieses Thema gilt für Hintergrundaufgaben, die in einem Prozess außerhalb von Prozessen ausgeführt werden, sowie für solche, die im gleichen Prozess wie die Vordergrund-App ausgeführt werden.

Erstellen Sie vor dem Hinzufügen der Bedingung ein [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)-Objekt, das die Bedingung darstellt, die zum Ausführen einer Hintergrundaufgabe erfüllt werden muss. Geben Sie im Konstruktor die zu erfüllende Bedingung an, indem Sie einen [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)-Enumerationswert angeben.

Der folgende Code erstellt ein [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)-Objekt, das die Internetverfügbarkeit als Bedingung angibt:

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>Hinzufügen des SystemCondition-Objekts zur Hintergrundaufgabe


Um die Bedingung hinzuzufügen, rufen Sie die [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769)-Methode für das [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Objekt auf, und übergeben Sie das [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)-Objekt an die Methode.

Der folgende Code registriert die InternetAvailable-Bedingung für Hintergrundaufgaben beim TaskBuilder:

> [!div class="tabbedCodeSnippets"]
> ```cs
> taskBuilder.AddCondition(internetCondition);
> ```
> ```cpp
> taskBuilder->AddCondition(internetCondition);
> ```

## <a name="register-your-background-task"></a>Registrieren Sie die Hintergrundaufgabe.


Sie können Ihre Hintergrundaufgabe jetzt mit der [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772)-Methode registrieren. Die Hintergrundaufgabe wird erst gestartet, wenn die angegebene Bedingung erfüllt ist.

Der folgende Code registriert die Aufgabe und speichert das resultierende BackgroundTaskRegistration-Objekt:

> [!div class="tabbedCodeSnippets"]
> ```cs
> BackgroundTaskRegistration task = taskBuilder.Register();
> ```
> ```cpp
> BackgroundTaskRegistration ^ task = taskBuilder->Register();
> ```

> **Hinweis**  Universelle Windows-Apps müssen vor der Registrierung von Hintergrundtrigger-Typen [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen.

Rufen Sie [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) und anschließend [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) auf, wenn die App nach der Aktualisierung gestartet wird, um sicherzustellen, dass Ihre universelle Windows-App nach der Veröffentlichung eines Updates weiterhin ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md).

> **Hinweis**  Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App Szenarien, in denen die Registrierung von Hintergrundaufgaben einen Fehler verursacht, problemlos verarbeitet.

## <a name="place-multiple-conditions-on-your-background-task"></a>Einfügen mehrerer Bedingungen für die Hintergrundaufgabe

Zum Hinzufügen mehrerer Bedingungen ruft Ihre App die [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) -Methode mehrmals auf. Diese Aufrufe müssen stattfinden, bevor die Aufgabenregistrierung wirksam wird.

> **Hinweis**: Achten Sie darauf, einer Hintergrundaufgabe keine in Konflikt stehenden Bedingungen hinzuzufügen.
 

Der folgende Ausschnitt zeigt mehrere Bedingungen im Kontext der Erstellung und Registrierung einer Hintergrundaufgabe:

> [!div class="tabbedCodeSnippets"]
```cs
> //
> // Set up the background task.
> //
>
> TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
>
> var recurringTaskBuilder = new BackgroundTaskBuilder();
>
> recurringTaskBuilder.Name           = "Hourly background task";
> recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder.SetTrigger(hourlyTrigger);
>
> //
> // Begin adding conditions.
> //
>
> SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
>
> recurringTaskBuilder.AddCondition(userCondition);
> recurringTaskBuilder.AddCondition(internetCondition);
>
> //
> // Done adding conditions, now register the background task.
> //
>
> BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```
```cpp
> //
> // Set up the background task.
> //
>
> TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
>
> auto recurringTaskBuilder = ref new BackgroundTaskBuilder();
>
> recurringTaskBuilder->Name           = "Hourly background task";
> recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder->SetTrigger(hourlyTrigger);
>
> //
> // Begin adding conditions.
> //
>
> SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
>
> recurringTaskBuilder->AddCondition(userCondition);
> recurringTaskBuilder->AddCondition(internetCondition);
>
> //
> // Done adding conditions, now register the background task.
> //
>
> BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>Anmerkungen


> **Hinweis**: Wählen Sie die Bedingungen für Ihre Hintergrundaufgabe aus, damit sie nur bei Bedarf ausgeführt wird und nicht dann, wenn sie es nicht sollte. Unter [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) finden Sie Beschreibungen der verschiedenen Bedingungen für Hintergrundaufgaben.

> **Hinweis**: Dieser Artikel ist für Windows 10-Entwickler gedacht, die Apps für die Universelle Windows-Plattform (UWP) schreiben. Informationen zur Entwicklung unter Windows 8.x oder Windows Phone 8.x finden Sie in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

## <a name="related-topics"></a>Verwandte Themen

****

* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen in Windows Store-Apps (beim Debuggen)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Dec16_HO2-->


