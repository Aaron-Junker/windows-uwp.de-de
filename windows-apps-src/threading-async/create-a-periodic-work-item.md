---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: Erstellen einer regelmäßigen Arbeitsaufgabe
description: Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die regelmäßig wiederholt wird.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, regelmäßige Arbeitsaufgabe, Threading, Timer
ms.localizationpriority: medium
ms.openlocfilehash: cf3a5817e459c7089eafb8f2c38d58b0e8eef03c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371562"
---
# <a name="create-a-periodic-work-item"></a>Erstellen einer regelmäßigen Arbeitsaufgabe


<b>Wichtige APIs</b>

-   [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)
-   [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer)

Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die regelmäßig wiederholt wird.

## <a name="create-the-periodic-work-item"></a>Erstellen der regelmäßigen Arbeitsaufgabe

Verwenden Sie die [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)-Methode, um eine regelmäßige Arbeitsaufgabe zu erstellen. Stellen Sie eine Lambda-Funktion zum Ausführen der Arbeit bereit, und geben Sie mit dem *period*-Parameter das Intervall zwischen den Übermittlungen an. Das Intervall wird anhand einer [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan)-Struktur angegeben. Nach jedem Verstreichen des Intervalls wird die Arbeitsaufgabe erneut gesendet. Stellen Sie daher sicher, dass es lang genug ist, um die Arbeit auszuführen.

[**CreateTimer** ](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) gibt eine [ **ThreadPoolTimer** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) Objekt. Speichern Sie das Objekt für den Fall, dass der Timer abgebrochen werden muss.

> **Beachten Sie**  vermeiden Sie dabei den Wert 0 (null) (oder ein Wert kleiner als eine Millisekunde) für das Intervall. Andernfalls verhält sich der regelmäßige Timer wie ein einmaliger Timer.

> **Beachten Sie**  können [ **CoreDispatcher.RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows) auf die Benutzeroberfläche zugreifen und Anzeigen des Status von der Arbeitsaufgabe.

Das folgende Beispiel erstellt eine Arbeitsaufgabe, die alle 60 Sekunden ausgeführt wird:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> TimeSpan period = TimeSpan.FromSeconds(60);
>
> ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, period);
> ```
> ``` cpp
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
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
>                 }));
>
>         }), period);
> ```

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>Behandeln des Abbruchs der regelmäßigen Arbeitsaufgabe (optional)

Bei Bedarf können Sie den Abbruch des regelmäßigen Timers mit einem [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler)-Element verarbeiten. Stellen Sie mithilfe der [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)-Überladung eine zusätzliche Lambda-Funktion bereit, die den Abbruch der regelmäßigen Arbeitsaufgabe behandelt.

Das folgende Beispiel erstellt eine regelmäßige Arbeitsaufgabe, die alle 60 Sekunden wiederholt wird, und stellt außerdem einen Abbruchhandler bereit:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> using Windows.System.Threading;
>
>     TimeSpan period = TimeSpan.FromSeconds(60);
>
>     ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>     },
>     period,
>     (source) =>
>     {
>         //
>         // TODO: Handle periodic timer cancellation.
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher->RunAsync(CoreDispatcherPriority.High,
>             ()=>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //                 
>
>                 // Periodic timer cancelled.
>
>             }));
>     });
> ```
> ``` cpp
> using namespace Windows::System::Threading;
> using namespace Windows::UI::Core;
>
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
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
>                 }));
>
>         }),
>         period,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle periodic timer cancellation.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     // Periodic timer cancelled.
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>Abbrechen des Zeitgebers

Rufen Sie ggf. die [**Cancel**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.cancel)-Methode auf, um die Wiederholung der regelmäßigen Arbeitsaufgabe zu beenden. Falls die Arbeitsaufgabe beim Abbruch des regelmäßigen Timers ausgeführt wird, kann sie noch abgeschlossen werden. Das [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler)-Element (sofern verwendet) wird aufgerufen, wenn alle Instanzen der regelmäßigen Arbeitsaufgabe abgeschlossen wurden.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>Hinweise

Informationen zu einmaligen Timern finden Sie unter [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md).

## <a name="related-topics"></a>Verwandte Themen

* [Senden einer Arbeitsaufgabe an den Threadpool](submit-a-work-item-to-the-thread-pool.md)
* [Bewährte Methoden zum Verwenden des Threadpools](best-practices-for-using-the-thread-pool.md)
* [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md)
 
