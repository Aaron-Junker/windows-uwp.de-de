---
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: Timergesteuertes Übermitteln einer Arbeitsaufgabe
description: Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die nach dem Ablaufen eines Timers ausgeführt wird.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Timer, Threads
ms.localizationpriority: medium
ms.openlocfilehash: 0ee5142997e3d4824152adf6d0b26bccb8a30f7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613305"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>Timergesteuertes Übermitteln einer Arbeitsaufgabe


<b>Wichtige APIs</b>

-   [**Windows.UI.Core namespace**](https://msdn.microsoft.com/library/windows/apps/BR208383)
-   [**Windows.System.Threading-namespace**](https://msdn.microsoft.com/library/windows/apps/BR229642)

Hier erfahren Sie, wie Sie eine Arbeitsaufgabe erstellen, die nach dem Ablaufen eines Timers ausgeführt wird.

## <a name="create-a-single-shot-timer"></a>Erstellen eines einmaligen Timers

Verwenden Sie die [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921)-Methode, um einen Timer für die Arbeitsaufgabe zu erstellen. Stellen Sie eine Lambda-Funktion zum Ausführen der Arbeit bereit, und geben Sie mit dem *delay*-Parameter an, wie lange der Threadpool warten soll, bevor er die Arbeitsaufgabe einem verfügbaren Thread zuweist. Die Verzögerung wird mithilfe einer [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996)-Struktur angegeben.

> **Beachten Sie**  können [ **CoreDispatcher.RunAsync** ](https://msdn.microsoft.com/library/windows/apps/Hh750317) auf die Benutzeroberfläche zugreifen und Anzeigen des Status von der Arbeitsaufgabe.

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

Behandeln Sie den Abbruch und Abschluss der Arbeitsaufgabe ggf. mit einem [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926)-Element. Stellen Sie mithilfe der [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921)-Überladung eine zusätzliche Lambda-Funktion bereit. Diese Funktion wird ausgeführt, wenn der Timer abgebrochen oder die Arbeitsaufgabe abgeschlossen wird.

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

## <a name="cancel-the-timer"></a>Abbrechen des Zeitgebers

Wenn der Timer weiter läuft, die Arbeitsaufgabe aber nicht mehr benötigt wird, rufen Sie [**Cancel**](https://msdn.microsoft.com/library/windows/apps/BR230588) auf. Der Timer wird abgebrochen, und die Arbeitsaufgabe wird nicht an den Threadpool übermittelt.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>Hinweise

UWP (Universelle Windows-Plattform)-Apps können **Thread.Sleep** nicht verwenden, da dies den UI-Thread blockieren kann. Verwenden Sie zum Erstellen einer Arbeitsaufgabe stattdessen einen [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587). Dieser Timer verzögert die von der Arbeitsaufgabe ausgeführte Aufgabe, ohne den UI-Thread zu blockieren.

Ein vollständiges Codebeispiel für Arbeitsaufgaben, Arbeitsaufgaben mit Zeitgeber und regelmäßige Arbeitsaufgaben finden Sie im [Beispiel für den Threadpool](https://go.microsoft.com/fwlink/p/?linkid=255387). Das Codebeispiel wurde ursprünglich für Windows 8.1 geschrieben kann Code jedoch erneut unter Windows 10 verwendet.

Informationen zu Wiederholungstimern finden Sie unter [Erstellen einer regelmäßigen Arbeitsaufgabe](create-a-periodic-work-item.md).

## <a name="related-topics"></a>Verwandte Themen

* [Senden einer Arbeitsaufgabe an den Threadpool](submit-a-work-item-to-the-thread-pool.md)
* [Bewährte Methoden zum Verwenden des Threadpools](best-practices-for-using-the-thread-pool.md)
* [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md)
 

 
