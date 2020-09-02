---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: Erstellen ein regelmäßiges Arbeitselement
description: Erfahren Sie, wie Sie ein Arbeits Element erstellen, das regelmäßig wiederholt wird, indem Sie die Methode "-Methode" der universelle Windows-Plattform (UWP) threadpooltimer-API verwenden.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, periodisches Arbeits Element, Threading, Timer
ms.localizationpriority: medium
ms.openlocfilehash: e1b50858b2c7e3ce4cd60f9401cedb75eb950c7d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304442"
---
# <a name="create-a-periodic-work-item"></a>Erstellen ein regelmäßiges Arbeitselement


<b>Wichtige APIs</b>

-   [**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)
-   [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer)

Hier erfahren Sie, wie Sie ein Arbeitselement erstellen, die regelmäßig wiederholt wird.

## <a name="create-the-periodic-work-item"></a>Erstellen das regelmäßige Arbeitselement

Verwenden Sie die [**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)-Methode, um eine regelmäßige Arbeitsaufgabe zu erstellen. Stellen Sie eine Lambda-Funktion zum Ausführen der Arbeit bereit, und geben Sie mit dem *period*-Parameter das Intervall zwischen den Übermittlungen an. Das Intervall wird anhand einer [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan)-Struktur angegeben. Nach jedem Verstreichen des Intervalls wird das Arbeitselement erneut gesendet. Stellen Sie daher sicher, dass es lang genug ist, um die Arbeit auszuführen.

[**CreateTimer**](/uwp/api/windows.system.threading.threadpooltimer.createtimer) gibt ein [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer)-Objekt zurück. Speichern Sie das Objekt für den Fall, dass der Timer abgebrochen werden muss.

> **Hinweis**    Vermeiden Sie den Wert 0 (null) (oder einen Wert kleiner als eine Millisekunde) für das Intervall. Andernfalls verhält sich der regelmäßige Timer wie ein einmaliger Timer.

> **Hinweis**    Sie können " [**coredispatcher. runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync) " verwenden, um auf die Benutzeroberfläche zuzugreifen und den Fortschritt des Arbeits Elements anzuzeigen.

Das folgende Beispiel erstellt ein Arbeitselement, die alle 60 Sekunden ausgeführt wird:

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

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>Behandeln des Abbruchs des regelmäßigen Arbeitselement (optional)

Bei Bedarf können Sie den Abbruch des regelmäßigen Timers mit einem [**TimerDestroyedHandler**](/uwp/api/windows.system.threading.timerdestroyedhandler)-Element verarbeiten. Stellen Sie mithilfe der [**CreatePeriodicTimer**](/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)-Überladung eine zusätzliche Lambda-Funktion bereit, die den Abbruch der regelmäßigen Arbeitsaufgabe behandelt.

Das folgende Beispiel erstellt ein regelmäßiges Arbeitselement, die alle 60 Sekunden wiederholt wird, und stellt außerdem einen Abbruchhandler bereit:

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

Rufen Sie ggf. die [**Cancel**](/uwp/api/windows.system.threading.threadpooltimer.cancel)-Methode auf, um die Wiederholung der regelmäßigen Arbeitsaufgabe zu beenden. Falls das Arbeitselement beim Abbruch des regelmäßigen Timers ausgeführt wird, kann sie noch abgeschlossen werden. Das [**TimerDestroyedHandler**](/uwp/api/windows.system.threading.timerdestroyedhandler)-Element (sofern verwendet) wird aufgerufen, wenn alle Instanzen der regelmäßigen Arbeitsaufgabe abgeschlossen wurden.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>Bemerkungen

Informationen zu einmaligen Timern finden Sie unter [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md).

## <a name="related-topics"></a>Zugehörige Themen

* [Senden ein Arbeitselement an den Threadpool](submit-a-work-item-to-the-thread-pool.md)
* [Bewährte Methoden zum Verwenden des Threadpools](best-practices-for-using-the-thread-pool.md)
* [Timergesteuertes Übermitteln einer Arbeitsaufgabe](use-a-timer-to-submit-a-work-item.md)
 