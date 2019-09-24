---
description: Weitere erweiterte Szenarien mit Parallelität und Asynchronie in C++/WinRT.
title: Erweiterte Parallelität und Asynchronie mit C++/WinRT
ms.date: 07/23/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, Parallelität, async, asynchron, Asynchronität
ms.localizationpriority: medium
ms.openlocfilehash: 1170b8e1291afd166f210feb291b644d1c7ed546
ms.sourcegitcommit: e5a154c7b6c1b236943738febdb17a4815853de5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71164827"
---
# <a name="more-advanced-concurrency-and-asynchrony-with-cwinrt"></a>Erweiterte Parallelität und Asynchronie mit C++/WinRT

In diesem Thema werden erweiterte Szenarien mit Parallelität und Asynchronie in C++/WinRT beschrieben.

Um eine Einführung zu diesem Thema zu erhalten, lesen Sie zunächst [Parallelität und asynchrone Vorgänge](concurrency.md).

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Auslagern von Aufgaben an den Windows-Threadpool

Eine Coroutine ist insofern eine Funktion wie jede andere, dass ein Aufrufer blockiert wird, bis eine Funktion die Ausführung an sie zurückgibt. Die erste Rückgabe einer Coroutine erfolgt bei der ersten `co_await`-, `co_return`- oder `co_yield`-Anweisung.

Bevor Sie rechnergebundene Aufgaben in einer Coroutine ausführen, müssen Sie die Ausführung an den Aufrufer zurückgeben (d. h. einen Anhaltepunkt einfügen), damit der Aufrufer nicht blockiert wird. Sofern Sie dazu noch keine `co_await`-Anweisung für einen anderen Vorgang verwenden, können Sie `co_await` für die [**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background)-Funktion ausführen. Dadurch wird die Steuerung an den Aufrufer zurückgegeben, und unmittelbar danach wird die Ausführung in einem Thread aus dem Threadpool fortgesetzt.

Der in der Implementierung verwendete Threadpool ist der [Windows-Threadpool](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api) auf niedriger Ebene und daher optimal effizient.

```cppwinrt
IAsyncOperation<uint32_t> DoWorkOnThreadPoolAsync()
{
    co_await winrt::resume_background(); // Return control; resume on thread pool.

    uint32_t result;
    for (uint32_t y = 0; y < height; ++y)
    for (uint32_t x = 0; x < width; ++x)
    {
        // Do compute-bound work here.
    }
    co_return result;
}
```

## <a name="programming-with-thread-affinity-in-mind"></a>Programmieren mit Threadaffinität

Dieses Szenario erweitert das vorherige. Sie lagern einige Aufgaben an den Threadpool aus, möchten dann aber den Status auf der Benutzeroberfläche anzeigen.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

Der obige Code löst eine [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread)-Ausnahme aus, da ein **TextBlock** von dem Thread aktualisiert werden muss, von dem er erstellt wurde (UI-Thread). Eine Lösung besteht darin, den Threadkontext in dem Kontext zu erfassen, in dem unsere Coroutine ursprünglich aufgerufen wurde. Instanziieren Sie zu diesem Zweck ein [**winrt::apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context)-Objekt für die Arbeit im Hintergrund, und führen Sie dann `co_await` für den **apartment_context** aus, um zum aufrufenden Kontext zurückzukehren.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

Solange die oben genannte Coroutine vom UI-Thread aufgerufen wird, der den **TextBlock** erstellt hat, funktioniert diese Technik. Es wird viele Fälle in Ihrer App geben, in denen Sie sicher sind, dass der Aufruf durch den UI-Thread erfolgt.

Eine allgemeinere Lösung zum Aktualisieren der Benutzeroberfläche für Fälle, in denen Sie den aufrufenden Thread nicht kennen, besteht darin, `co_await` für die [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)-Funktion zu verwenden, um zu einem bestimmten Vordergrundthread zu wechseln. Im folgenden Codebeispiel geben Sie den Vordergrundthread an, indem Sie das dem **TextBlock** zugeordnete Dispatcherobjekt übergeben (durch Zugriff auf die zugehörige [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)-Eigenschaft). Die Implementierung von **winrt::resume_foreground** ruft [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) für dieses Dispatcherobjekt auf, um die anschließenden Aufgaben in der Coroutine auszuführen.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Ausführungskontexte, Fortsetzen und Wechseln in einer Coroutine

Vereinfacht ausgedrückt, kann nach einem Anhaltepunkt in einer Coroutine der ursprüngliche Ausführungsthread nicht mehr vorhanden sein und die Wiederaufnahme in einem beliebigen Thread erfolgen (d. h. jeder Thread kann die **Completed**-Methode für den asynchronen Vorgang aufrufen).

Wenn Sie `co_await` aber für einen der vier asynchronen Windows-Runtime-Vorgangstypen (**IAsyncXxx**) verwenden, erfasst C++/WinRT den aufrufenden Kontext zum Zeitpunkt der `co_await`-Anweisung. Außerdem wird sichergestellt, dass dieser Kontext beim Fortsetzen beibehalten wird. C++/WinRT überprüft dazu, ob Sie sich bereits im aufrufenden Kontext befinden, und wechselt zu diesem Kontext, wenn dies nicht der Fall ist. Wenn vor der `co_await`-Anweisung ein STA-Thread (Singlethread-Apartment) aktiv ist, ist dieser auch nach dem Aufruf aktiv. Ist vor der `co_await`-Anweisung ein MTA-Thread (Multithread-Apartment) aktiv, ist dies danach ebenfalls der Fall.

```cppwinrt
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

Sie können sich auf dieses Verhalten verlassen, da C++/WinRT Code zum Anpassen der asynchronen Windows-Runtime-Vorgangstypen für die C++-Sprachunterstützung für Coroutinen bereitstellt (diese Codeabschnitte werden als Wait-Adapter bezeichnet). Die übrigen Awaitable-Typen in C++/WinRT sind lediglich Wrapper und/oder Hilfstypen für den Threadpool und werden daher in diesem abgeschlossen.

```cppwinrt
using namespace std::chrono_literals;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

Wenn Sie `co_await` für einen anderen Typ verwenden – auch innerhalb einer C++/WinRT-Coroutinenimplementierung –, stellt eine andere Bibliothek die Adapter bereit. In diesem Fall müssen Sie wissen, wie sich diese Adapter im Hinblick auf die Wiederaufnahme und Kontexte verhalten.

Um Kontextwechsel auf ein Minimum zu begrenzen, können Sie die hier bereits vorgestellten Techniken verwenden. Im Folgenden sehen wir uns einige Beispiele dazu an. Dieses Pseudocodebeispiel zeigt die Gliederung eines Ereignishandlers, der eine Windows-Runtime-API aufruft, um ein Bild zu laden, das Bild zur Verarbeitung an einen Hintergrundthread übergibt und dann zum UI-Thread zurückkehrt, um das Bild auf der Benutzeroberfläche anzuzeigen.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    // Call StorageFile::OpenAsync to load an image file.

    // The call to OpenAsync occurred on a background thread, but C++/WinRT has restored us to the UI thread by this point.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

Bei diesem Szenario ist der Aufruf von **StorageFile::OpenAsync** etwas ineffizient. Bei der Wiederaufnahme, nach der C++/WinRT den Kontext des UI-Threads wiederherstellt, ist ein Kontextwechsel zu einem Hintergrundthread erforderlich (damit der Handler die Ausführung an den Aufrufer zurückgeben kann). In diesem Fall muss der UI-Thread aber erst aktiv sein, wenn Sie die Benutzeroberfläche aktualisieren möchten. Je mehr Windows-Runtime-APIs wir *vor* dem Aufruf von **winrt::resume_background** aufrufen, desto mehr unnötige Kontextwechsel finden statt. Die Lösung besteht darin, vor diesem Aufruf *keine* Windows-Runtime-APIs aufzurufen. Führen Sie diese Aufrufe alle nach dem Aufruf von **winrt::resume_background** aus.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Call StorageFile::OpenAsync to load an image file.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

Falls Sie eine anspruchsvollere Lösung wünschen, können Sie eigene Await-Adapter schreiben. Wenn Sie eine `co_await`-Anweisung beispielsweise in dem Thread fortsetzen möchten, in dem die asynchrone Aktion abgeschlossen wird (d. h. ohne Kontextwechsel), könnten Sie wie unten gezeigt Await-Adapter schreiben.

> [!NOTE]
> Das folgende Codebeispiel dient nur zur Veranschaulichung. Es zeigt Ihnen, wie Await-Adapter funktionieren. Wenn Sie diese Technik in Ihrer eigenen Codebasis verwenden möchten, empfehlen wir, eigene Await-Adapterstrukturen zu entwickeln und zu testen. Sie könnten beispielsweise **complete_on_any**, **complete_on_current** und **complete_on(dispatcher)** schreiben. Erwägen Sie auch, die Adapter als Vorlagen zu erstellen, die den **IAsyncXxx**-Typ als Vorlagenparameter akzeptieren.

```cppwinrt
struct no_switch
{
    no_switch(Windows::Foundation::IAsyncAction const& async) : m_async(async)
    {
    }

    bool await_ready() const
    {
        return m_async.Status() == Windows::Foundation::AsyncStatus::Completed;
    }

    void await_suspend(std::experimental::coroutine_handle<> handle) const
    {
        m_async.Completed([handle](Windows::Foundation::IAsyncAction const& /* asyncInfo */, Windows::Foundation::AsyncStatus const& /* asyncStatus */)
        {
            handle();
        });
    }

    auto await_resume() const
    {
        return m_async.GetResults();
    }

private:
    Windows::Foundation::IAsyncAction const& m_async;
};
```

Wichtig zu wissen ist bei der Verwendung von **no_switch**-Await-Adaptern, dass der C++-Compiler nach Funktionen namens **await_ready**, **await_suspend** und **await_resume** sucht, wenn er auf einen `co_await`-Ausdruck trifft. Da die C++/WinRT-Bibliothek diese Funktionen bereitstellt, erhalten Sie standardmäßig ein geeignetes Verhalten wie dieses.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Ändern Sie zur Verwendung der **no_switch**-Await-Adapter einfach wie hier gezeigt den Typ dieses `co_await`-Ausdrucks von **IAsyncXxx** in **no_switch**.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

Anstatt nach den drei **await_xxx**-Funktionen zu suchen, die **IAsyncXxx** entsprechen, sucht der C++-Compiler dann nach Funktionen, die mit **no_switch** übereinstimmen.

## <a name="a-deeper-dive-into-winrtresume_foreground"></a>Ausführlichere Erläuterung von **winrt::resume_foreground**

Ab [C++/WinRT 2.0](/windows/uwp/cpp-and-winrt-apis/newsnews#news-and-changes-in-cwinrt-20) wird die [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)-Funktion auch angehalten, wenn sie über den Verteilerthread aufgerufen wird (in früheren Versionen konnte dies in einigen Szenarien Deadlocks zur Folge haben, da die Funktion nur angehalten wurde, wenn sie nicht bereits im Verteilerthread vorhanden war).

Mit dem aktuellen Verhalten können Sie sich darauf verlassen, dass der Stapel entladen wird und ein erneutes Einreihen in die Warteschlange erfolgt. Dies ist für die Systemstabilität wichtig, vor allem in systemnahem Systemcode. Die letzte Codeliste im Abschnitt [Programmieren mit Threadaffinität](#programming-with-thread-affinity-in-mind) oben zeigt, wie eine komplexe Berechnung in einem Hintergrundthread durchgeführt und anschließend zum entsprechenden UI-Thread gewechselt wird, um die Benutzeroberfläche zu aktualisieren.

So sieht **winrt::resume_foreground** intern aus.

```cppwinrt
auto resume_foreground(...) noexcept
{
    struct awaitable
    {
        bool await_ready() const
        {
            return false; // Queue without waiting.
            // return m_dispatcher.HasThreadAccess(); // The C++/WinRT 1.0 implementation.
        }
        void await_resume() const {}
        void await_suspend(coroutine_handle<> handle) const { ... }
    };
    return awaitable{ ... };
};
```

Das aktuelle Verhalten im Vergleich zum vorherigen Verhalten entspricht dem Unterschied zwischen [**PostMessage**](/windows/win32/api/winuser/nf-winuser-postmessagew) und [**SendMessage**](/windows/win32/api/winuser/nf-winuser-sendmessage) in der Win32-Anwendungsentwicklung. **PostMessage** fügt die Aufgabe in die Warteschlange ein und entlädt dann den Stapel, ohne auf den Abschluss der Aufgabe zu warten. Das Entladen des Stapels kann von wesentlicher Bedeutung sein.

Die **winrt::resume_foreground**-Funktion hat zunächst nur den [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) (an ein [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) gebunden) unterstützt, der vor Windows 10 eingeführt wurde. Wir haben seitdem einen flexibleren und effizienteren Verteiler eingeführt: [ **DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Sie können eine **DispatcherQueue** für eigene Zwecke erstellen. Sehen Sie sich diese einfache Konsolenanwendung an.

```cppwinrt
using namespace Windows::System;

winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    auto controller{ DispatcherQueueController::CreateOnDedicatedThread() };
    RunAsync(controller.DispatcherQueue());
    getchar();
}
```

Im obigen Beispiel wird eine Warteschlange (in einem Controller) in einem privaten Thread erstellt und dann der Controller an die Coroutine übergeben. Die Coroutine kann die Warteschlange verwenden, um auf den privaten Thread zu warten (ihn anzuhalten und fortsetzen). Eine weitere häufige Verwendung von **DispatcherQueue** ist das Erstellen einer Warteschlange im aktuellen UI-Thread für eine herkömmliche Desktop- oder Win32-App.

```cppwinrt
DispatcherQueueController CreateDispatcherQueueController()
{
    DispatcherQueueOptions options
    {
        sizeof(DispatcherQueueOptions),
        DQTYPE_THREAD_CURRENT,
        DQTAT_COM_STA
    };
 
    ABI::Windows::System::IDispatcherQueueController* ptr{};
    winrt::check_hresult(CreateDispatcherQueueController(options, &ptr));
    return { ptr, take_ownership_from_abi };
}
```

Dies veranschaulicht, wie Sie Win32-Funktionen aufrufen und in Ihre C++/WinRT-Projekte einbinden können, indem Sie einfach die [**CreateDispatcherQueueController**](/windows/win32/api/dispatcherqueue/nf-dispatcherqueue-createdispatcherqueuecontroller)-Funktion im Win32-Stil aufrufen, um den Controller zu erstellen, und dann den Besitz des resultierenden Warteschlangencontrollers an den Aufrufer als WinRT-Objekt übertragen. So können Sie außerdem die effiziente und nahtlose Verwendung von Warteschlangen für vorhandene Win32-Anwendungen im Petzold-Stil unterstützen.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    Window window;
    auto controller{ CreateDispatcherQueueController() };
    RunAsync(controller.DispatcherQueue());
    MSG message;
 
    while (GetMessage(&message, nullptr, 0, 0))
    {
        DispatchMessage(&message);
    }
}
```

Im obigen Beispiel wird in der einfachen **main**-Funktion zunächst ein Fenster erstellt. Sie können sich vorstellen, dass hierdurch eine Window-Klasse registriert und [**CreateWindow**](/windows/win32/api/winuser/nf-winuser-createwindoww) aufgerufen wird, um das Desktopfenster der obersten Ebene zu erstellen. Anschließend wird die **CreateDispatcherQueueController**-Funktion aufgerufen, um den Warteschlangencontroller zu erstellen, bevor eine Coroutine mit der Verteilerwarteschlange aufgerufen wird, die im Besitz des Controllers ist. Dann wechselt die Ausführung zu einem herkömmlichen Nachrichtensystem, in dem die Coroutine selbstverständlich in diesem Thread wiederaufgenommen wird. Anschließend können Sie zu den Coroutinen für den asynchronen oder nachrichtenbasierten Workflow in der Anwendung zurückkehren.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ... // Begin on the calling thread...
 
    co_await winrt::resume_foreground(queue);
 
    ... // ...resume on the dispatcher thread.
}
```

Durch den Aufruf von **winrt::resume_foreground** wird der Stapel immer *in die Warteschlange gestellt* und dann entladen. Optional können Sie auch die Wiederaufnahmepriorität festlegen.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await winrt::resume_foreground(queue, DispatcherQueuePriority::High);
 
    ...
}
```

Oder die Standardreihenfolge für Warteschlangen verwenden.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await queue;
 
    ...
}
```

Oder in diesem Fall das Herunterfahren der Warteschlange erkennen und dies ordnungsgemäß durchführen.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    if (co_await queue)
    {
        ... // Resume on dispatcher thread.
    }
    else
    {
        ... // Still on calling thread.
    }
}
```

Der Ausdruck `co_await` gibt `true` zurück, was bedeutet, dass die Wiederaufnahme im Verteilerthread erfolgt. Das heißt, dass die Verarbeitung des Stapels in der Warteschlange erfolgreich war. Umgekehrt wird `false` zurückgegeben, um anzugeben, dass die Ausführung im aufrufenden Thread verbleibt, da der Controller der Warteschlange heruntergefahren wird und keine Warteschlangenanforderungen mehr verarbeitet.

Sie haben also sehr viele Möglichkeiten, wenn Sie C++/WinRT mit Coroutinen kombinieren, insbesondere bei der herkömmlichen Desktopanwendungsentwicklung im Petzold-Stil.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Abbrechen eines asynchronen Vorgangs und Abbruchrückrufe

Die Funktionen der Windows-Runtime für die asynchrone Programmierung ermöglichen es Ihnen, ausgeführte asynchrone Aktionen oder Vorgänge abzubrechen. Im folgenden Beispiel wird [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) aufgerufen, um eine möglicherweise große Sammlung von Dateien abzurufen, und das resultierende asynchrone Vorgangsobjekt in einem Datenmember gespeichert. Der Benutzer hat die Möglichkeit, den Vorgang abzubrechen.

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.Search.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace Windows::UI::Xaml;
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable /* sender */, RoutedEventArgs /* args */)
    {
        workButton().Content(winrt::box_value(L"Working..."));

        // Enable the Pictures Library capability in the app manifest file.
        StorageFolder picturesLibrary{ KnownFolders::PicturesLibrary() };

        m_async = picturesLibrary.GetFilesAsync(CommonFileQuery::OrderByDate, 0, 1000);

        IVectorView<StorageFile> filesInFolder{ co_await m_async };

        workButton().Content(box_value(L"Done!"));

        // Process the files in some way.
    }

    void OnCancel(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        if (m_async.Status() != AsyncStatus::Completed)
        {
            m_async.Cancel();
            workButton().Content(winrt::box_value(L"Canceled"));
        }
    }

private:
    IAsyncOperation<::IVectorView<StorageFile>> m_async;
};
...
```

Für die Implementierung des Abbruchs beginnen wir mit einem einfachen Beispiel.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction ImplicitCancellationAsync()
{
    while (true)
    {
        std::cout << "ImplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto implicit_cancellation{ ImplicitCancellationAsync() };
    co_await 3s;
    implicit_cancellation.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

Wenn Sie das obige Beispiel ausführen, werden Sie sehen, dass **ImplicitCancellationAsync** drei Sekunden lang eine Meldung pro Sekunde ausgibt und danach aufgrund des Abbruchs automatisch beendet wird. Dies funktioniert, weil eine Coroutine überprüft, ob sie abgebrochen wurde, wenn sie auf einen `co_await`-Ausdruck trifft. Wenn dies der Fall ist, wird sie sofort beendet. Andernfalls wird sie wie gewohnt angehalten.

Der Abbruch kann natürlich erfolgen, während die Coroutine angehalten ist. Die Coroutine überprüft nur dann, ob sie abgebrochen wurde, wenn sie fortgesetzt wird oder auf einen weiteren `co_await`-Ausdruck trifft. Das Problem ist möglicherweise eine zu lange Wartezeit bei der Reaktion auf den Abbruch.

Eine weitere Option besteht daher darin, den Abbruch explizit innerhalb der Coroutine abzufragen. Aktualisieren Sie das obige Beispiel mit dem unten aufgeführten Code. **ExplicitCancellationAsync** ruft in diesem neuen Beispiel das von der [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token)-Funktion zurückgegebene Objekt ab und verwendet es, um in regelmäßigen Abständen zu überprüfen, ob die Coroutine abgebrochen wurde. Solange sie nicht abgebrochen wird, wird die Coroutine unbegrenzt in einer Schleife ausgeführt. Wenn sie abgebrochen wird, werden die Schleife und die Funktion normal beendet. Das Ergebnis ist mit dem im vorherigen Beispiel identisch, hier erfolgt die Beendigung jedoch explizit und kontrolliert.

```cppwinrt
IAsyncAction ExplicitCancellationAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };

    while (!cancellation_token())
    {
        std::cout << "ExplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto explicit_cancellation{ ExplicitCancellationAsync() };
    co_await 3s;
    explicit_cancellation.Cancel();
}
...
```

Beim Warten auf **winrt::get_cancellation_token** wird ein Abbruchtoken abgerufen, wobei die **IAsyncAction**, die die Coroutine für Sie erzeugt, bekannt ist. Sie können den Funktionsaufrufoperator für dieses Token verwenden, um den Abbruchstatus bzw. Abbruch abzufragen. Wenn Sie einen rechnergebundenen Vorgang ausführen oder eine umfangreiche Sammlung durchlaufen, ist diese Technik sinnvoll.

### <a name="register-a-cancellation-callback"></a>Registrieren eines Abbruchrückrufs

Der Abbruch der Windows-Runtime wird nicht automatisch an andere asynchrone Objekte übertragen. Seit &mdash;Version 10.0.17763.0 (Windows 10, Version 1809) des Windows SDK&mdash; können Sie jedoch einen Abbruchrückruf registrieren. Dies ist ein präemptiver Hook, der die Weitergabe des Abbruchs und die Integration in vorhandene Parallelitätsbibliotheken ermöglicht.

Im nächsten Codebeispiel funktioniert **NestedCoroutineAsync**, enthält aber keine speziellen Abbruchlogik. **CancellationPropagatorAsync** ist im Wesentlichen ein Wrapper für die geschachtelte Coroutine, der den Abbruch präemptiv weiterleitet.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction NestedCoroutineAsync()
{
    while (true)
    {
        std::cout << "NestedCoroutineAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction CancellationPropagatorAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };
    auto nested_coroutine{ NestedCoroutineAsync() };

    cancellation_token.callback([=]
    {
        nested_coroutine.Cancel();
    });

    co_await nested_coroutine;
}

IAsyncAction MainCoroutineAsync()
{
    auto cancellation_propagator{ CancellationPropagatorAsync() };
    co_await 3s;
    cancellation_propagator.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

**CancellationPropagatorAsync** registriert eine Lambda-Funktion für seinen eigenen Abbruchrückruf und wartet dann (wird angehalten), bis die geschachtelte Coroutine abgeschlossen ist. Wenn **CancellationPropagatorAsync** abgebrochen wird, gibt er den Abbruch an die geschachtelte Coroutine weiter. Es ist weder erforderlich, den Abbruch abzufragen, noch wird der Abbruch auf unbestimmte Zeit blockiert. Dieser Mechanismus ist flexibel genug, um ihn zusammen mit einer nicht für C++/WinRT konzipierten Coroutine oder Parallelitätsbibliothek zu verwenden.

## <a name="reporting-progress"></a>Melden des Status

Wenn Ihre Coroutine [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_) oder [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) zurückgibt, können Sie das von der [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token)-Funktion zurückgegebene Objekt abrufen und damit den Status an einen Statushandler zurückmelden. Hier sehen Sie ein Codebeispiel.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncOperationWithProgress<double, double> CalcPiTo5DPs()
{
    auto progress{ co_await winrt::get_progress_token() };

    co_await 1s;
    double pi_so_far{ 3.1 };
    progress(0.2);

    co_await 1s;
    pi_so_far += 4.e-2;
    progress(0.4);

    co_await 1s;
    pi_so_far += 1.e-3;
    progress(0.6);

    co_await 1s;
    pi_so_far += 5.e-4;
    progress(0.8);

    co_await 1s;
    pi_so_far += 9.e-5;
    progress(1.0);
    co_return pi_so_far;
}

IAsyncAction DoMath()
{
    auto async_op_with_progress{ CalcPiTo5DPs() };
    async_op_with_progress.Progress([](auto const& /* sender */, double progress)
    {
        std::wcout << L"CalcPiTo5DPs() reports progress: " << progress << std::endl;
    });
    double pi{ co_await async_op_with_progress };
    std::wcout << L"CalcPiTo5DPs() is complete !" << std::endl;
    std::wcout << L"Pi is approx.: " << pi << std::endl;
}

int main()
{
    winrt::init_apartment();
    DoMath().get();
}
```

> [!NOTE]
> Implementieren Sie für asynchrone Aktionen oder Vorgänge nicht mehrere *Abschlusshandler*. Du kannst entweder einen einzelnen Delegaten für das Abschlussereignis verwenden oder `co_await` dafür ausführen. Wenn Sie beide Abschlusshandler nutzen, schlägt der zweite fehl. Für ein asynchrones Objekt können Sie einen der beiden folgenden Abschlusshandlertypen verwenden, jedoch nicht beide.

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
async_op_with_progress.Completed([](auto const& sender, AsyncStatus /* status */)
{
    double pi{ sender.GetResults() };
});
```

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
double pi{ co_await async_op_with_progress };
```

Weitere Informationen zu Abschlusshandlern finden Sie unter [Delegattypen für asynchrone Aktionen und Vorgänge](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Fire and Forget (Auslösen und Vergessen)

Es kann vorkommen, dass eine Aufgabe gleichzeitig mit anderen Aufgaben ausgeführt werden kann, und Sie weder auf ihren Abschluss warten (keine anderen Aufgaben sind davon abhängig) noch einen Wert zurückgeben müssen. In diesem Fall können Sie die Aufgabe „auslösen und vergessen“. Dazu können Sie eine Coroutine mit dem Rückgabetyp [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) schreiben (anstelle eines der asynchronen Windows-Runtime-Vorgangstypen oder **concurrency::task**).

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace std::chrono_literals;

winrt::fire_and_forget CompleteInFiveSeconds()
{
    co_await 5s;
}

int main()
{
    winrt::init_apartment();
    CompleteInFiveSeconds();
    // Do other work here.
}
```

**winrt::fire_and_forget** ist auch als Rückgabetyp des Ereignishandlers hilfreich, wenn Sie asynchrone Vorgänge in ihm ausführen müssen. Hier ist ein Beispiel (siehe auch [Starke und schwache Verweise in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)).

```cppwinrt
winrt::fire_and_forget MyClass::MyMediaBinder_OnBinding(MediaBinder const&, MediaBindingEventArgs args)
{
    auto lifetime{ get_strong() }; // Prevent *this* from prematurely being destructed.
    auto ensure_completion{ unique_deferral(args.GetDeferral()) }; // Take a deferral, and ensure that we complete it.

    auto file{ co_await StorageFile::GetFileFromApplicationUriAsync(Uri(L"ms-appx:///video_file.mp4")) };
    args.SetStorageFile(file);

    // The destructor of unique_deferral completes the deferral here.
}
```

Das erste Argument (*sender*) bleibt unbenannt, weil es nie verwendet wird. Aus diesem Grund können wir es problemlos als Verweis belassen. Beachten Sie jedoch, dass *args* als Wert übergeben wird. Siehe oben den Abschnitt [Parameterübergabe](concurrency.md#parameter-passing).

## <a name="awaiting-a-kernel-handle"></a>Warten auf ein Kernelhandle

C++/WinRT stellt eine **resume_on_signal**-Klasse bereit, mit der ein Prozess ausgesetzt werden kann, bis ein Kernelereignis signalisiert wird. Sie müssen sicherstellen, dass das Handle gültig bleibt, bis `co_await resume_on_signal(h)` wieder zurückgegeben wird. **resume_on_signal** selbst kann das nicht ausführen, da das Handle bereits verloren gegangen sein kann, bevor **resume_on_signal** startet – wie in diesem ersten Beispiel zu sehen.

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

Das eingehende **HANDLE** ist nur so lange gültig, bis die Funktion zurückgegeben wird, und diese Funktion (bei der es sich um eine Coroutine handelt) wird am ersten Anhaltepunkt zurückgegeben (in diesem Fall das erste Auftreten von `co_await`). Beim Warten auf **DoWorkAsync** ist die Steuerung wieder beim Aufrufer, der aufrufende Frame ist nicht mehr im Gültigkeitsbereich, und es ist nicht mehr klar, ob das Handle noch gültig ist, wenn die Coroutine fortgesetzt wird.

Technisch gesehen, erhält die Coroutine ihre Parameter ordnungsgemäß über einen Wert (siehe [Parameterübergabe](concurrency.md#parameter-passing) weiter oben). In diesem Fall müssen wir jedoch einen Schritt weiter gehen, um den *Grundgedanken* dieser Anweisung zu befolgen und sie nicht einfach nur abzuarbeiten. Wir müssen zusammen mit dem Handle einen starken Verweis (bzw. den Besitz) übergeben. Gehen Sie dazu wie folgt vor:

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* not valid here.
}
```

Die Übergabe von [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) über einen Wert sorgt für Besitzsemantik, wodurch sichergestellt ist, dass das Kernelhandle für die gesamten Lebensdauer der Coroutine gültig bleibt.

So könnte diese Coroutine aufgerufen werden.

```cppwinrt
namespace
{
    winrt::handle duplicate(winrt::handle const& other, DWORD access)
    {
        winrt::handle result;
        if (other)
        {
            winrt::check_bool(::DuplicateHandle(::GetCurrentProcess(),
                other.get(), ::GetCurrentProcess(), result.put(), access, FALSE, 0));
        }
        return result;
    }

    winrt::handle make_manual_reset_event(bool initialState = false)
    {
        winrt::handle event{ ::CreateEvent(nullptr, true, initialState, nullptr) };
        winrt::check_bool(static_cast<bool>(event));
        return event;
    }
}

IAsyncAction SampleCaller()
{
    handle event{ make_manual_reset_event() };
    auto async{ Async(duplicate(event)) };

    ::SetEvent(event.get());
    event.close(); // Our handle is closed, but Async still has a valid handle.

    co_await async; // Will wake up when *event* is signaled.
}
```

## <a name="asynchronous-timeouts-made-easy"></a>Einfache Verwendung von asynchronen Timeouts

In C++/WinRT haben C++-Coroutinen große Bedeutung. Sie haben eine wesentliche Auswirkung auf das Schreiben von Parallelitätscode. In diesem Abschnitt werden Fälle erläutert, in denen die Details der Asynchronie nicht wichtig sind und es nur auf das unmittelbare Ergebnis ankommt. Aus diesem Grund verfügt die C++/WinRT-Implementierung der [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)-Windows-Runtime-Schnittstelle für asynchrone Vorgänge über eine **get**-Funktion, die der von **std::function** bereitgestellten Funktionalität ähnelt.

```cppwinrt
using namespace winrt::Windows::Foundation;
int main()
{
    IAsyncAction async = ...
    async.get();
    puts("Done!");
}
```

Die **get**-Funktion wird auf unbegrenzte Zeit blockiert, bis das asynchrone Objekt abgeschlossen wurde. Asynchrone Objekte sind tendenziell sehr kurzlebig, sodass dies häufig alles ist, was Sie benötigen.

Es gibt jedoch Fälle, in denen dies nicht ausreicht, und Sie müssen den Wartevorgang verwerfen, nachdem einige Zeit vergangen ist. Dank der von der Windows-Runtime bereitgestellten Bausteine war es immer möglich, diesen Code zu schreiben. Durch das Bereitstellen der **wait_for**-Funktion wurde dies jetzt jedoch durch C++/WinRT wesentlich vereinfacht. Sie ist auch in **IAsyncAction** implementiert und ähnelt ebenfalls der von **std::function** bereitgestellten Funktionalität.

```cppwinrt
using namespace std::chrono_literals;
int main()
{
    IAsyncAction async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        puts("done");
    }
}
```

> [!NOTE]
> **wait_for** verwendet **std::chrono::chrono::duration** an der Schnittstelle, ist aber auf einen kleineren Bereich beschränkt als das, was **std::chrono::duration** bietet (etwa 49,7 Tage).

Die **wait_for**-Funktion im nächsten Beispiel wartet ca. fünf Sekunden lang und überprüft dann, ob der Vorgang abgeschlossen wurde. Wenn der Vergleich positiv ausgefallen ist, wissen Sie, dass das asynchrone Objekt erfolgreich abgeschlossen wurde, und Sie sind fertig. Wenn Sie auf ein Ergebnis warten, können Sie einfach die **get**-Funktion aufrufen, um das Ergebnis abzurufen.

```cppwinrt
int main()
{
    IAsyncOperation<int> async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        printf("result %d\n", async.get());
    }
}
```

Da das asynchrone Objekt inzwischen abgeschlossen ist, gibt **get** das Ergebnis sofort ohne weitere Wartezeit zurück. Wie Sie sehen können, gibt **wait_for** den Status des asynchronen Objekts zurück. Daher können Sie die Funktion für eine präzisere Steuerung verwenden, wie im folgenden Beispiel.

```cppwinrt
switch (async.wait_for(5s))
{
case AsyncStatus::Completed:
    printf("result %d\n", async.get());
    break;
case AsyncStatus::Canceled:
    puts("canceled");
    break;
case AsyncStatus::Error:
    puts("failed");
    break;
case AsyncStatus::Started:
    puts("still running");
    break;
}
```

- Beachten Sie, dass **AsyncStatus::Completed** den erfolgreichen Abschluss des asynchronen Objekts zurückgibt, und Sie können mit **get** ggf. das Ergebnis abrufen.
- **AsyncStatus::Canceled** bedeutet, dass das asynchrone Objekt abgebrochen wurde. Da ein Abbruch in der Regel vom Aufrufer angefordert wird, wird dieser Status selten behandelt. In der Regel wird ein abgebrochenes asynchrones Objekt einfach verworfen.
- **Asyncstatus:: Error** bedeutet, dass bei dem asynchronen Objekt ein Fehler aufgetreten ist. Sie können ggf. mithilfe von **get** die Ausnahme erneut auslösen.
- **AsyncStatus::Started** bedeutet, dass das asynchrone Objekt noch ausgeführt wird. Das asynchrone Windows-Runtime-Muster lässt nicht mehrere Wartevorgänge zu. Deshalb können Sie **wait_for** nicht in einer Schleife aufrufen. Wenn bei der Wartezeit tatsächlich ein Timeout aufgetreten ist, stehen Ihnen einige Optionen zur Verfügung. Sie können das Objekt verwerfen, oder Sie können seinen Status abfragen, bevor Sie ggf. durch den Aufruf von **get** ein Ergebnis abrufen. Es empfiehlt sich jedoch, das Objekt an dieser Stelle zu verwerfen.

## <a name="important-apis"></a>Wichtige APIs
* [IAsyncAction-Schnittstelle](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync-Methode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground)

## <a name="related-topics"></a>Verwandte Themen
* [Parallelität und asynchrone Vorgänge](concurrency.md)
* [Verarbeiten von Ereignissen über Delegaten in C++/WinRT](handle-events.md)