---
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: Senden einer Arbeitsaufgabe mithilfe eines Timers
description: Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die nach dem Ablaufen eines Zeitgebers ausgeführt wird.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Timer, Threads
ms.localizationpriority: medium
ms.openlocfilehash: 7bd870858bbccffa07b082384ae6ddea987b67f2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258930"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>Senden einer Arbeitsaufgabe mithilfe eines Timers


<b>Wichtige APIs</b>

-   [**Windows.UI.Core namespace**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)
-   [**Windows.System.Threading namespace**](https://docs.microsoft.com/uwp/api/Windows.System.Threading)

Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die nach dem Ablaufen eines Zeitgebers ausgeführt wird.

## <a name="create-a-single-shot-timer"></a>Erstellen eines einmaligen Timers

Verwenden Sie die [**CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer)-Methode, um einen Timer für die Arbeitsaufgabe zu erstellen. Stellen Sie eine Lambda-Funktion zum Ausführen der Arbeit bereit, und geben Sie mit dem *delay*-Parameter an, wie lange der Threadpool warten soll, bevor er die Arbeitsaufgabe einem verfügbaren Thread zuweist. Die Verzögerung wird mithilfe einer [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan)-Struktur angegeben.

> **Note**  You can use [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) to access the UI and show progress from the work item.

Im folgenden Beispiel wird eine Arbeitsaufgabe erstellt, die in drei Minuten ausgeführt wird:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, delay);
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     ExampleUIUpdateMethod("Timer completed.");
>
>                 }));
>
>         }), delay);
> ```

## <a name="provide-a-completion-handler"></a>Bereitstellen eines Abschlusshandlers

Behandeln Sie den Abbruch und Abschluss der Arbeitsaufgabe ggf. mit einem [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler)-Element. Stellen Sie mithilfe der [**CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer)-Überladung eine zusätzliche Lambda-Funktion bereit. Diese Funktion wird ausgeführt, wenn der Timer abgebrochen oder die Arbeitsaufgabe abgeschlossen wird.

Das folgende Beispiel erstellt einen Zeitgeber, der die Arbeitsaufgabe sendet, und ruft eine Methode auf, wenn die Arbeitsaufgabe abgeschlossen oder der Zeitgeber abgebrochen wird:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> bool completed = false;
>
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>                 CoreDispatcherPriority.High,
>                 () =>
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 });
>
>         completed = true;
>     },
>     delay,
>     (source) =>
>     {
>         //
>         // TODO: Handle work cancellation/completion.
>         //
>
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>                 if (completed)
>                 {
>                     // Timer completed.
>                 }
>                 else
>                 {
>                     // Timer cancelled.
>                 }
>
>             });
>     });
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> completed = false;
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Work
>             //
>
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>             completed = true;
>
>         }),
>         delay,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle work cancellation/completion.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // Update the UI thread by using the UI core dispatcher.
>                     //
>
>                     if (completed)
>                     {
>                         // Timer completed.
>                     }
>                     else
>                     {
>                         // Timer cancelled.
>                     }
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>Abbrechen des Timers

Wenn der Timer weiter läuft, die Arbeitsaufgabe aber nicht mehr benötigt wird, rufen Sie [**Cancel**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.cancel) auf. Der Timer wird abgebrochen, und die Arbeitsaufgabe wird nicht an den Threadpool übermittelt.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>Hinweise

UWP (Universelle Windows-Plattform)-Apps können **Thread.Sleep** nicht verwenden, da dies den UI-Thread blockieren kann. Verwenden Sie zum Erstellen einer Arbeitsaufgabe stattdessen einen [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer). Dieser Timer verzögert die von der Arbeitsaufgabe ausgeführte Aufgabe, ohne den UI-Thread zu blockieren.

Ein vollständiges Codebeispiel für Arbeitsaufgaben, Arbeitsaufgaben mit Zeitgeber und regelmäßige Arbeitsaufgaben finden Sie im [Beispiel für den Threadpool](https://code.msdn.microsoft.com/windowsapps/Pool-Sample-5aa60454). The code sample was originally written for Windows 8.1 but the code can be re-used in Windows 10.

Informationen zu Wiederholungstimern finden Sie unter [Erstellen einer regelmäßigen Arbeitsaufgabe](create-a-periodic-work-item.md).

## <a name="related-topics"></a>Verwandte Themen

* [Senden einer Arbeitsaufgabe an den Threadpool](submit-a-work-item-to-the-thread-pool.md)
* [Bewährte Methoden zum Verwenden des Threadpools](best-practices-for-using-the-thread-pool.md)
* [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md)
 

 
