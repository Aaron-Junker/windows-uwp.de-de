---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: Erstellen einer regelmäßigen Arbeitsaufgabe
description: Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die regelmäßig wiederholt wird.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP, regelmäßige Arbeitsaufgabe, Threading, Timer
ms.localizationpriority: medium
ms.openlocfilehash: 92142bcf084b6504e4c694ca33d2dc8532f1acca
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "8214722"
---
# <a name="create-a-periodic-work-item"></a>Erstellen einer regelmäßigen Arbeitsaufgabe


** Wichtige APIs **

-   [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915)
-   [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587)

Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die regelmäßig wiederholt wird.

## <a name="create-the-periodic-work-item"></a>Erstellen der regelmäßigen Arbeitsaufgabe

Verwenden Sie die [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915)-Methode, um eine regelmäßige Arbeitsaufgabe zu erstellen. Stellen Sie eine Lambda-Funktion zum Ausführen der Arbeit bereit, und geben Sie mit dem *period*-Parameter das Intervall zwischen den Übermittlungen an. Das Intervall wird anhand einer [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996)-Struktur angegeben. Nach jedem Verstreichen des Intervalls wird die Arbeitsaufgabe erneut gesendet. Stellen Sie daher sicher, dass es lang genug ist, um die Arbeit auszuführen.

[**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.createtimer.aspx) gibt ein [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587)-Objekt zurück. Speichern Sie das Objekt für den Fall, dass der Timer abgebrochen werden muss.

> **Hinweis:** vermeiden Sie es, einen Wert 0 (null) (oder ein Wert kleiner als eine Millisekunde) für das Intervall. Andernfalls verhält sich der regelmäßige Timer wie ein einmaliger Timer.

> **Hinweis:** können Sie [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) verwenden, um auf die Benutzeroberfläche zuzugreifen und den Fortschritt der Arbeitsaufgabe anzuzeigen.

Das folgende Beispiel erstellt eine Arbeitsaufgabe, die alle 60Sekunden ausgeführt wird:

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

Bei Bedarf können Sie den Abbruch des regelmäßigen Timers mit einem [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926)-Element verarbeiten. Stellen Sie mithilfe der [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915)-Überladung eine zusätzliche Lambda-Funktion bereit, die den Abbruch der regelmäßigen Arbeitsaufgabe behandelt.

Das folgende Beispiel erstellt eine regelmäßige Arbeitsaufgabe, die alle 60Sekunden wiederholt wird, und stellt außerdem einen Abbruchhandler bereit:

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

## <a name="cancel-the-timer"></a>Abbrechen des Timers

Rufen Sie ggf. die [**Cancel**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.cancel.aspx)-Methode auf, um die Wiederholung der regelmäßigen Arbeitsaufgabe zu beenden. Falls die Arbeitsaufgabe beim Abbruch des regelmäßigen Timers ausgeführt wird, kann sie noch abgeschlossen werden. Das [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926)-Element (sofern verwendet) wird aufgerufen, wenn alle Instanzen der regelmäßigen Arbeitsaufgabe abgeschlossen wurden.

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
 
