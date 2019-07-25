---
description: Dieses Thema zeigt, wie Sie asynchrone Windows-Runtime-Objekte mit C++/WinRT erstellen und nutzen können.
title: Parallelität und asynchrone Vorgänge mit C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, Parallelität, async, asynchron, Asynchronität
ms.localizationpriority: medium
ms.openlocfilehash: cbabf38f41ae940f5c92944154638eae7016e043
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660098"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Parallelität und asynchrone Vorgänge mit C++/WinRT

In diesem Thema wird beschrieben, wie Sie asynchrone Windows-Runtime-Objekte mit [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) erstellen und nutzen können.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Asynchrone Vorgänge und „Async“-Funktionen der Windows-Runtime

Jede Windows-Runtime-API, deren Ausführung länger als 50 Millisekunden dauern kann, wird als asynchrone Funktion implementiert (mit einem Namen, der auf „Async” endet). Die Implementierung einer asynchronen Funktion initiiert die Arbeit in einem anderen Thread und gibt sofort ein Objekt zurück, das den asynchronen Vorgang darstellt. Wenn der asynchrone Vorgang abgeschlossen ist, enthält das zurückgegebene Objekt einen aus seiner Ausführung resultierenden Wert. Der Windows-Runtime-Namespace **Windows::Foundation** enthält vier Objekttypen für asynchrone Vorgänge.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) und
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Jeder dieser asynchronen Vorgangstypen wird auf einen entsprechenden Typ im C++/WinRT-Namespace **winrt::Windows::Foundation** projiziert. C++/WinRT enthält außerdem eine interne Await-Adapter-Struktur. Sie verwenden diese Struktur nicht direkt, können dank ihr aber eine `co_await`-Anweisung schreiben, um kooperativ auf das Ergebnis einer Funktion zu warten, die einen dieser asynchronen Vorgangstypen zurückgibt. Zudem können Sie eigene Coroutinen schreiben, die diese Typen zurückgeben.

Ein Beispiel für eine asynchrone Windows-Funktion ist [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync). Diese Funktion gibt ein asynchrones Vorgangsobjekt vom Typ [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) zurück. Sehen wir uns einige Möglichkeiten an (&mdash;zuerst blockierende und dann nicht blockierende&mdash;), wie Sie eine solche API mit C++/WinRT aufrufen können.

## <a name="block-the-calling-thread"></a>Blockieren des aufrufenden Threads

Das folgende Codebeispiel empfängt ein asynchrones Vorgangsobjekt von **RetrieveFeedAsync** und ruft **get** für dieses Objekt auf, um den aufrufenden Thread zu blockieren, bis die Ergebnisse des asynchronen Vorgangs vorliegen.

Wenn Sie dieses Beispiel kopieren und direkt in die Hauptquellcodedatei eines Projekts vom Typ **Windows-Konsolenanwendung (C++/WinRT)** einfügen möchten, legen Sie zuerst in den Projekteigenschaften die Option **Vorkompilierte Header nicht verwenden** fest.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

Der Aufruf von **get** lässt sich einfach codieren und eignet sich perfekt für Konsolen-Apps oder Hintergrundthreads, in denen Sie möglicherweise keine Coroutine verwenden möchten. Dieser Aufruf ist jedoch weder parallel noch asynchron und eignet sich daher nicht für einen UI-Thread (dort wird durch den Aufruf außerdem eine Assertion in nicht optimierten Builds ausgelöst). Um zu verhindern, dass Betriebssystemthreads blockiert werden und keine anderen nützlichen Aufgaben ausführen können, benötigen wir eine andere Technik.

## <a name="write-a-coroutine"></a>Schreiben einer Coroutine

C++/WinRT integriert C++ Coroutinen in das Programmiermodell, um eine natürliche Möglichkeit zu bieten, kooperativ auf ein Ergebnis zu warten. Sie können eigene asynchrone Windows-Runtime-Vorgänge erstellen, indem Sie eine Coroutine schreiben. Im folgenden Codebeispiel ist **ProcessFeedAsync** die Coroutine.

> [!NOTE]
> Die **get**-Funktion ist im C++/WinRT-Projektionstyp **Winrt::Windows::Foundation::IAsyncAction** enthalten, sodass Sie die Funktion in jedem C++/WinRT-Projekt aufrufen können. Die Funktion ist nicht als Member der [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)-Schnittstelle aufgeführt, da **get** nicht Teil der Oberfläche der binären Anwendungsschnittstelle (Application Binary Interface, ABI) des eigentlichen Windows-Runtime-Typs **IAsyncAction** ist.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    PrintFeed(syndicationFeed);
}

int main()
{
    winrt::init_apartment();

    auto processOp{ ProcessFeedAsync() };
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

Eine Coroutine ist eine Funktion, die angehalten und fortgesetzt werden kann. Wenn die `co_await`-Anweisung in der obigen **ProcessFeedAsync**-Coroutine erreicht wird, initiiert die Coroutine asynchron den **RetrieveFeedAsync**-Aufruf. Anschließend hält sie sich sofort selbst an und gibt die Steuerung an den Aufrufer zurück (im obigen Beispiel **main**). **main** kann dann weiter ausgeführt werden, während der Feed abgerufen und ausgegeben wird. Danach (wenn der **RetrieveFeedAsync**-Aufruf abgeschlossen ist) wird die **ProcessFeedAsync**-Coroutine bei der nächsten Anweisung fortgesetzt.

Sie können eine Coroutine in anderen Coroutinen zusammenfassen. Alternativ können Sie zum Blockieren **get** aufrufen und auf ihren Abschluss warten (und, sofern vorhanden, das Ergebnis abrufen). Sie können sie auch an eine andere Programmiersprache übergeben, die die Windows-Runtime unterstützt.

Es ist auch möglich, die Completed- und/oder Progress-Ereignisse von asynchronen Aktionen und Vorgängen mit Delegaten zu verarbeiten. Details und Codebeispiele finden Sie unter [Delegattypen für asynchrone Aktionen und Vorgänge](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>Asynchrone Rückgabe eines Windows-Runtime-Typs

Im nächsten Beispiel verpacken wir einen Aufruf von **RetrieveFeedAsync** für einen bestimmten URI, um eine **RetrieveBlogFeedAsync**-Funktion zu erhalten, die asynchron einen [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed) zurückgibt.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> RetrieveBlogFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    return syndicationClient.RetrieveFeedAsync(rssFeedUri);
}

int main()
{
    winrt::init_apartment();

    auto feedOp{ RetrieveBlogFeedAsync() };
    // do other work.
    PrintFeed(feedOp.get());
}
```

Im obigen Beispiel gibt **RetrieveBlogFeedAsync** einen **IAsyncOperationWithProgress** zurück, der sowohl einen Status- als auch einen Rückgabewert enthält. Wir können andere Aufgaben durchführen, während **RetrieveBlogFeedAsync** den Feed abruft. Anschließend rufen wir zum Blockieren **get** für dieses asynchrone Vorgangsobjekt auf, warten auf seinen Abschluss und rufen dann die Ergebnisse des Vorgangs ab.

Wenn Sie einen Windows-Runtime-Typ asynchron zurückgeben, sollten Sie einen [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) oder [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) zurückgeben. Qualifiziert sind alle Erst- oder Drittanbieter-Runtime-Klassen oder Typen, die an oder von eine(r) Windows-Runtime-Funktion übergeben werden können (z. B. `int` oder **winrt::hstring**). Der Compiler gibt einen Fehler vom Typ *WinRT-Typ erforderlich* aus, wenn Sie versuchen, einen dieser asynchronen Vorgangstypen mit einem Nicht-Windows-Runtime-Typ zu verwenden.

Wenn eine Coroutine nicht mindestens eine `co_await`-Anweisung enthält, muss sie mindestens eine `co_return`- oder `co_yield`-Anweisung enthalten, um sich als Coroutine zu qualifizieren. Es gibt Fälle, in denen Ihre Coroutine einen Wert zurückgeben kann, ohne Asynchronität zu verursachen, d. h. ohne den Kontext zu blockieren oder zu wechseln. Hier sehen Sie ein Beispiel, bei dem zu diesem Zweck (beim zweiten oder bei einem nachfolgenden Aufruf) ein Wert zwischengespeichert wird.

```cppwinrt
winrt::hstring m_cache;

IAsyncOperation<winrt::hstring> ReadAsync()
{
    if (m_cache.empty())
    {
        // Asynchronously download and cache the string.
    }
    co_return m_cache;
}
``` 

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Asynchrone Rückgabe eines Nicht-Windows-Runtime-Typs

Wenn Sie asynchron einen Typ zurückgeben, der *kein* Windows-Runtime-Typ ist, sollten Sie einen [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)-PPL-Typ (Parallel Patterns Library) zurückgeben. Wir empfehlen die Verwendung von **concurrency::task**, weil dieser Typ eine bessere Leistung (und später eine bessere Kompatibilität) bietet als **std::future**.

> [!TIP]
> Wenn Sie `<pplawait.h>` einschließen, können Sie **concurrency::task** als Coroutinentyp verwenden.

```cppwinrt
// main.cpp
#include <iostream>
#include <ppltasks.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
        {
            Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
            SyndicationClient syndicationClient;
            SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
            return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
        });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp{ RetrieveFirstTitleAsync() };
    // Do other work here.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="parameter-passing"></a>Parameterübergabe

Für synchrone Funktionen sollten Sie standardmäßig `const&`-Parameter verwenden. Dadurch wird der Mehraufwand für Kopien vermieden (einschließlich Verweiszählung, die zu miteinander verbundenen Erhöhungen und Verringerungen führt).

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

Es können jedoch Probleme auftreten, wenn Sie einen Verweisparameter an eine Coroutine übergeben.

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...

    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

In einer Coroutine verläuft die Ausführung bis zum ersten Anhaltepunkt, an dem die Steuerung an den Aufrufer zurückgegeben wird, synchron. Bis die Coroutine fortgesetzt wird, kann alles Mögliche mit dem Quellwert passiert sein, auf den ein Verweisparameter verweist. Aus Sicht der Coroutine hat ein Verweisparameter eine unkontrollierte Lebensdauer. Im obigen Beispiel können wir also bis zur `co_await`-Anweisung problemlos auf *value* zugreifen, aber nicht danach. Falls dieser *value* vom Aufrufer zerstört wird, verursacht der anschließende Versuch, in der Coroutine darauf zuzugreifen, eine Beschädigung des Speichers. Wir können *value* nicht sicher an **DoOtherWorkAsync** übergeben, wenn das Risiko besteht, dass diese Funktion dadurch angehalten wird und nach dem Fortsetzen versucht, *value* zu verwenden.

Damit die Parameter nach dem Anhalten und Fortsetzen ohne Probleme verwendet werden können, sollten Ihre Coroutinen standardmäßig als Wert zu übergebende Parameter verwenden. Dadurch stellen Sie sicher, dass die Erfassung nach Wert erfolgt und keine Probleme mit der Lebensdauer auftreten. Die Fälle, in denen Sie sicher sind, dass keine Probleme auftreten, und Sie daher von dieser Vorgehensweise abweichen können, sind eher selten.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value); // not const&
```

Die Übergabe nach Wert erfordert, dass das Argument einfach zu verschieben oder zu kopieren ist, und dies ist in der Regel bei intelligenten Zeigern der Fall.

Es ist auch fraglich, ob die Übergabe nach Konstantenwert empfehlenswert ist (es sei denn, Sie möchten den Wert verschieben). Diese Vorgehensweise hat keine Auswirkungen auf den Quellwert, von dem Sie eine Kopie erstellen, macht aber die Absicht klar und hilft Ihnen, wenn Sie die Kopie versehentlich ändern.

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

Unter [Standard-Arrays und -Vektoren](std-cpp-data-types.md#standard-arrays-and-vectors) erfahren Sie, wie Sie einen Standardvektor an einen asynchronen Aufgerufenen übergeben.

Wenn die Signatur der Coroutine nicht geändert werden kann, die Implementierung jedoch geändert werden kann, können Sie vor dem ersten `co_await` eine lokale Kopie erstellen.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_value = value;
    // It's ok to access both safe_value and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_value here (not value).
}
```

Wenn das Kopieren von `Param` zu aufwendig ist, extrahieren Sie einfach die erforderlichen Teile vor dem ersten `co_await`.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_data = value.data;
    // It's ok to access safe_data, value.data, and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_data here (not value.data, nor value).
}
```

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Sicherer Zugriff auf den *this*-Zeiger in einer Klassenmember-Coroutine

Siehe [Starke und schwache Verweise in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

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
using namespace std::chrono;
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

Das erste Argument (*sender*) bleibt unbenannt, weil es nie verwendet wird. Aus diesem Grund können wir es problemlos als Verweis belassen. Beachten Sie jedoch, dass *args* als Wert übergeben wird. Siehe oben den Abschnitt [Parameterübergabe](#parameter-passing).

## <a name="important-apis"></a>Wichtige APIs
* [concurrency::task-Klasse](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction-Schnittstelle](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync-Methode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed-Klasse](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Verwandte Themen
* [Verarbeiten von Ereignissen über Delegaten in C++/WinRT](handle-events.md)
* [C++-Standarddatentypen und C++/WinRT](std-cpp-data-types.md)
