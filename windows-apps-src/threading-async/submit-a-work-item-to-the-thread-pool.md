---
author: normesta
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: Senden einer Arbeitsaufgabe an den Threadpool
description: Hier erfahren Sie, wie Sie Arbeit in einem separaten Thread erledigen können, indem Sie eine Arbeitsaufgabe an den Threadpool übermitteln.
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Threads, Threadpool
ms.localizationpriority: medium
ms.openlocfilehash: fe73520782b18fb7419807695296bc6487f9c018
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2018
ms.locfileid: "7579926"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>Übermitteln einer Arbeitsaufgabe an den Threadpool

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)

Hier erfahren Sie, wie Sie Arbeit in einem separaten Thread erledigen können, indem Sie eine Arbeitsaufgabe an den Threadpool übermitteln. Somit sorgen Sie dafür, dass die Benutzeroberfläche bei der Erledigung von Arbeit, die viel Zeit in Anspruch nimmt, reaktionsfähig bleibt, und Sie können mehrere Aufgaben parallel bearbeiten.

## <a name="create-and-submit-the-work-item"></a>Erstellen und Senden der Arbeitsaufgabe

Erstellen Sie eine Arbeitsaufgabe, indem Sie [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) aufrufen. Stellen Sie einen Delegaten zur Durchführung der Arbeit bereit (Sie können eine Lambda-Funktion oder Delegatfunktion verwenden). **RunAsync** gibt ein [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)-Objekt zurück. Speichern Sie dieses Objekt, da es im nächsten Schritt verwendet wird.

Drei Versionen von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) sind verfügbar. Damit können Sie optional die Priorität der Arbeitsaufgabe angeben und steuern, ob sie gleichzeitig mit anderen Arbeitsaufgaben ausgeführt wird.

>[!NOTE]
>Verwenden Sie [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) , um Zugriff auf den UI-Thread und den Fortschritt der Arbeitsaufgabe anzuzeigen.

Im folgenden Beispiel werden eine Arbeitsaufgabe erstellt und ein Lambda für die Verarbeitung angegeben:

```csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

```cppwinrt
// The nth prime number to find.
const unsigned int n{ 9999 };

unsigned long nthPrime{ 0 };

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = Windows::System::Threading::ThreadPool::RunAsync([&](Windows::Foundation::IAsyncAction const& workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status() == Windows::Foundation::AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                std::wstringstream updateString;
                updateString << L"Progress to " << n << L"th prime: " << (10 * progress) << std::endl;

                // Update the UI thread with the CoreDispatcher.
                Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
                    Windows::UI::Core::CoreDispatcherPriority::High,
                    Windows::UI::Core::DispatchedHandler([&]()
                {
                    UpdateUI(updateString.str());
                }));
            }
        }
    }
    // Return the nth prime number.
    nthPrime = i;
});
```

```cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
std::shared_ptr<unsigned long> nthPrime = make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
       {
           break;
       }

       // Go to the next number.
       i++;

       // Check for prime.
       bool prime = true;
       for (unsigned int j = 2; j < i; ++j)
       {
           if ((i % j) == 0)
           {
               prime = false;
               break;
           }
       };

       if (prime)
       {
           // Found another prime number.
           primes++;

           // Report progress at every 10 percent.
           unsigned int temp = progress;
           progress = static_cast<unsigned int>(10.f*primes / n);

           if (progress != temp)
           {
               String^ updateString;
               updateString = "Progress to " + n + "th prime: "
                   + (10 * progress).ToString() + "%\n";

               // Update the UI thread with the CoreDispatcher.
               CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                   CoreDispatcherPriority::High,
                   ref new DispatchedHandler([this, updateString]()
               {
                   UpdateUI(updateString);
               }));
           }
       }
   }
   // Return the nth prime number.
   *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

Nach dem Aufruf von [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) wird die Arbeitsaufgabe vom Threadpool in eine Warteschlange eingereiht und ausgeführt, wenn ein Thread verfügbar wird. Arbeitsaufgaben im Threadpool werden asynchron und in einer beliebigen Reihenfolge ausgeführt. Stellen Sie daher sicher, dass Ihre Arbeitsaufgaben über eine unabhängige Funktionsweise funktionieren.

Beachten Sie, dass von der Arbeitsaufgabe die [**IAsyncInfo.Status**](https://msdn.microsoft.com/library/windows/apps/BR206593)-Eigenschaft überprüft und bei einem Abbruch der Arbeitsaufgabe beendet wird.

## <a name="handle-work-item-completion"></a>Behandeln der Vervollständigung der Arbeitsaufgabe

Stellen Sie einen Vervollständigungshandler zur Verfügung, indem Sie die [**IAsyncAction.Completed**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.completed.aspx)-Eigenschaft der Arbeitsaufgabe festlegen. Geben Sie einen Delegaten an (Sie können eine Lambda-Funktion oder Delegatfunktion nutzen), mit dem die Arbeitsaufgabe durchgeführt wird. Verwenden Sie z.B. [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317), um auf den Benutzeroberflächenthread zuzugreifen und das Ergebnis anzuzeigen.

Im folgenden Beispiel wird die Benutzeroberfläche mit dem Ergebnis der in Schritt1 übermittelten Arbeitsaufgabe aktualisiert:

```cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }

    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```

```cppwinrt
m_workItem.Completed([&](Windows::Foundation::IAsyncAction const& asyncInfo, Windows::Foundation::AsyncStatus const& asyncStatus)
{
    if (asyncStatus == Windows::Foundation::AsyncStatus::Canceled)
    {
        return;
    }

    std::wstringstream updateString;
    updateString << std::endl << L"The " << n << L"th prime number is " << nthPrime << std::endl;

    // Update the UI thread with the CoreDispatcher.
    Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
        Windows::UI::Core::CoreDispatcherPriority::High,
        Windows::UI::Core::DispatchedHandler([&]()
    {
        UpdateUI(updateString.str());
    }));
});
```

```c#
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

Beachten Sie, dass vom Vervollständigungshandler überprüft wird, ob die Arbeitsaufgabe abgebrochen wurde, bevor eine Aktualisierung der Benutzeroberfläche bereitgestellt wird.

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

Weitere Informationen finden Sie durch das Herunterladen des Codes in dieser schnellstartanleitung unter dem [Erstellen einer ThreadPool Element Beispiel funktioniert](http://go.microsoft.com/fwlink/p/?LinkID=328569) für Windows8.1, geschrieben und den Quellcode in einer Win\_unap Windows 10-app wiederverwenden.

## <a name="related-topics"></a>Verwandte Themen

* [Senden einer Arbeitsaufgabe an den Threadpool](submit-a-work-item-to-the-thread-pool.md)
* [Bewährte Methoden zum Verwenden des Threadpools](best-practices-for-using-the-thread-pool.md)
* [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md)
 
