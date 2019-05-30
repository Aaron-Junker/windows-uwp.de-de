---
description: Dieses Thema zeigt, wie Sie asynchrone Windows-Runtime-Objekte mit C++/WinRT erstellen und nutzen können.
title: Parallelität und asynchrone Vorgänge mit C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, parallelität, async, asynchron, asynchronität
ms.localizationpriority: medium
ms.openlocfilehash: 5dba97ede63b1bcb85c4ee1807d5558f4c93834a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361208"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Parallelität und asynchrone Vorgänge mit C++/WinRT

In diesem Thema wird gezeigt, in dem Sie beide können, Methoden erstellen, und nutzen asynchrone Windows-Runtime-Objekte mit [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Asynchrone Vorgänge und Windows-Runtime-”Async”-Funktionen

Jede Windows-Runtime-API, die mehr als 50 Millisekunden dauern kann, ist als asynchrone Funktion implementiert (mit einem Namen, der auf „Async” endet). Die Implementierung einer asynchronen Funktion initiiert die Arbeit in einem anderen Thread und kehrt direkt mit einem Objekt zurück, das den asynchronen Vorgang repräsentiert. Wenn der asynchrone Vorgang abgeschlossen ist, enthält das zurückgegebene Objekt einen Wert, der das Ergebnis darstellt. Der **Windows::Foundation**-Windows-Runtime-Namespace enthält vier Typen von Objekten für asynchrone Vorgänge.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_), und
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Jeder dieser Typen für asynchrone Vorgänge wird auf einen entsprechenden Typ im C++/WinRT-Namespace **winrt::Windows::Foundation** projiziert. C++/WinRT enthält außerdem eine interne Await-Adapter-Struktur. Sie verwenden nicht direkt aber Dank dieser Struktur, können Sie schreiben eine `co_await` Anweisung, um das Ergebnis von jeder Funktion, die einen dieser Typen der asynchrone Vorgang zurückgibt, kooperativ zu warten. Und Sie können Ihre eigenen Coroutinen schreiben, die diese Typen zurückgeben.

Ein Beispiel für eine asynchrone Windows-Funktion ist [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), die ein Objekt für einen asynchronen Vorgang vom Typ [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) zurückgibt. Betrachten wir einige Möglichkeiten&mdash;erste blockiert, und klicken Sie dann nicht blockierend&mdash;der Verwendung von C++ / WinRT zum Aufruf einer API, z. B. das.

## <a name="block-the-calling-thread"></a>Den aufrufenden Thread blockieren

Das folgende Codebeispiel erhält ein Objekt für asynchronen Vorgänge von **RetrieveFeedAsync** und ruft **get** für dieses Objekt auf, um den aufrufenden Thread zu blockieren, bis die Ergebnisse des asynchronen Vorgangs vorliegen.

Sollten Sie kopieren und Einfügen von in diesem Beispiel wird direkt in der Codedatei main Quelle des eine **Windows-Konsolenanwendung (C++"/ WinRT")** -Projekt, und klicken Sie dann auf ersten **vorkompilierte Header nicht verwenden** in-Projekt Eigenschaften.

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

Der Aufruf von **get** ermöglicht eine bequeme Codeerstellung und eignet sich perfekt für Konsolenapps oder Hintergrundthreads, in denen Sie möglicherweise keine Coroutine verwenden möchten. Sie ist jedoch weder parallel noch asynchron und folglich eignet sie sich nicht für einen UI-Thread (außerdem wird eine Assertion in nicht optimierten Builds ausgelöst, wenn Sie versuchen, sie dafür zu verwenden). Um Betriebssystem-Threads nicht daran zu hindern, andere nützliche Aufgaben zu erledigen, benötigen wir eine andere Technik.

## <a name="write-a-coroutine"></a>Schreiben einer Coroutine

> [!IMPORTANT]
> Als [ C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20), werden Sie sicher, dass `#include <winrt/coroutine.h>` jedes Mal, wenn Sie erstellen oder nutzen Sie eine Coroutine.

C++/WinRT integriert C++ Coroutinen in das Programmiermodell, um eine natürliche Möglichkeit zu bieten, kooperativ auf ein Ergebnis zu warten. Sie können Ihre eigene asynchronen Windows-Runtime-Vorgänge erzeugen, indem Sie eine Coroutine schreiben. Im folgenden Codebeispiel ist **ProcessFeedAsync** die Coroutine.

> [!NOTE]
> Die **erhalten** Funktion vorhanden ist, an die C++ / WinRT-Projektionstyp **Winrt::Windows::Foundation::IAsyncAction**, sodass Sie die Funktion in C++ aufrufen können c++ / WinRT-Projekt. Sehen Sie nicht die Funktion als ein Mitglied der [ **IAsyncAction** ](/uwp/api/windows.foundation.iasyncaction) Schnittstelle, da **erhalten** ist nicht Teil der Anwendung anwendungsbinärschnittstelle (ABI) Oberfläche der tatsächliche Windows-Runtime-Typ **IAsyncAction**.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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

Eine Coroutine ist eine Funktion, die angehalten und wieder fortgesetzt werden kann. Wenn die `co_await`-Anweisung In der **ProcessFeedAsync**-Coroutine oben erreicht ist, initiiert die Coroutine asynchron den **RetrieveFeedAsync**-Aufruf und unterbricht sich dann sofort selbst und gibt die Kontrolle an den Aufrufer zurück (was im obigen Beispiel **main** ist). **main** kann dann weiterarbeiten, während der Feed abgerufen und ausgegeben wird. Wenn das erledigt ist (wenn der **RetrieveFeedAsync**-Aufruf abgeschlossen ist), wird die **ProcessFeedAsync**-Coroutine bei der nächsten Anweisung fortgesetzt.

Sie können eine Coroutine in anderen Coroutinen zusammenfassen. Oder Sie können **get** zur Blockierung aufrufen und warten, bis sie abgeschlossen ist (und das Ergebnis erhalten, wenn es eines gibt). Oder Sie können sie an eine andere Programmiersprache übergeben, die Windows-Runtime unterstützt.

Es ist auch möglich, die Completed- und/oder Progress-Ereignisse von asynchronen Aktionen und Vorgängen mit Hilfe von Delegaten zu verarbeiten. Details und Codebeispiele finden Sie unter [Delegattypen für asynchrone Aktionen und Vorgänge](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>Asychrone Rückgabe eines Windows-Runtime-Typs

In diesem nächsten Beispiel verpacken wir einen Aufruf von **RetrieveFeedAsync** für eine bestimmte URI, um uns eine **RetrieveBlogFeedAsync**-Funktion zu holen, die asynchron ein [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed) zurückgibt.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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

Im obigen Beispiel gibt **RetrieveBlogFeedAsync** ein **IAsyncOperationWithProgress** zurück, das sowohl einen Progress- als auch einen Return-Wert hat. Wir können andere Arbeiten durchführen, während **RetrieveBlogFeedAsync** sein Ding macht und den Feed abruft. Dann rufen wir das **get**-Objekt des asynchronen Vorgangs auf, um es zu blockieren, warten, bis es abgeschlossen ist, und erhalten dann die Ergebnisse des Vorgangs.

Wenn Sie einen Windows-Runtime-Typ asynchron zurückgeben, sollten Sie ein [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) oder ein [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) zurückgeben. Alle Erst- oder Drittanbieter-Laufzeitklassen sind qualifiziert, oder ein anderer Typ, der an oder von einer Windows-Runtime-Funktion übergeben werden kann (z. B. `int` oder **winrt::hstring**). Der Compiler hilft Ihnen mit einem „*WinRT-Typ erforderlich*”-Fehler, wenn Sie versuchen, einen dieser Typen für asychrone Vorgänge mit einem Nicht-Windows-Runtime-Typ zu verwenden.

Wenn eine Coroutine nicht mindestens eine `co_await`-Anweisung enthält, benötigt sie mindestens eine `co_return`- oder `co_yield`-Anweisung, um sich als Coroutine zu qualifizieren. Es gibt Fälle, in denen Ihre Coroutine einen Wert zurückgeben kann, ohne dass es zur Asynchronität kommt und ohne dass Kontext blockiert oder gewechselt wird. Hier ist ein solches Beispiel, das (beim zweiten oder nachfolgenden Aufruf) dafür einen Wert zwischenspeichert.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Asychrone Rückgabe eines Nicht-Windows-Runtime-Typs

Wenn Sie asynchron einen Typ zurückgeben, der *kein* Windows-Runtime-Typ ist, sollten Sie ein Parallel Patterns Library (PPL) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class) zurückgeben. Wir empfehlen **concurrency::task**, weil es eine bessere Performance (und später eine bessere Kompatibilität) bietet als **std::future**.

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

Für synchrone Funktionen sollten Sie standardmäßig `const&`-Parameter verwenden. Dadurch wird der Aufwand für Kopien vermieden (wozu Referenzzählung gehört, die zu miteinander verbundenen Erhöhungen und Verringerungen führt).

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

Sie können jedoch auf Probleme stoßen, wenn Sie einen Referenzparameter an eine Coroutine übergeben.

```cppwinrt
...
#include <winrt/coroutine.h>
...
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...

    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

In einer Coroutine verläuft die Ausführung bis zum ersten Anhaltepunkt synchron, wobei die Steuerung an den Aufrufer zurückgegeben wird. Wenn die Coroutine fortgesetzt wird, kann alles Mögliche mit dem Quellwert passiert sein, auf den ein Referenzparameter verweist. Aus Sicht der Coroutine hat ein Referenzparameter eine unkontrollierte Lebensdauer. Im obigen Beispiel können wir also problemlos auf *value* bis `co_await` zugreifen, nicht jedoch danach. Wir können *value* nicht problemlos an **DoOtherWorkAsync** übergeben, wenn das Risiko besteht, dass diese Funktion ihrerseits angehalten wird und bei Fortsetzen dann versucht, *value* zu verwenden. Damit die Parameter nach dem Anhalten und Fortsetzen ohne Probleme verwendet werden können, sollten Ihre Coroutinen standardmäßig „pass-by-value“ verwenden, um sicherzustellen, dass die Erfassung nach Wert erfolgt, und um Probleme mit der Lebensdauer zu vermeiden. Fälle, in denen Sie von dieser Vorgehensweise abweichen können, da Sie sicher sind, dass keine Probleme entstehen, sind eher selten.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value);
```

Es ist auch fraglich, ob die Übergabe nach Konstantenwert empfehlenswert ist (es sei denn, Sie möchten den Wert verschieben). Dies hat keine Auswirkungen auf den Quellwert, von dem Sie eine Kopie erstellen, es macht aber die Intention klar und hilft, wenn Sie die Kopie versehentlich ändern.

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

Weitere Informationen finden Sie unter [Standard-Arrays und -Vektoren](std-cpp-data-types.md#standard-arrays-and-vectors). Hier wird beschrieben, wie Sie einen Standardvektor in einem asynchronen Aufrufer übergeben.

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Auslagern von Aufgaben an den Windows-Threadpool

Eine Coroutine ist eine Funktion wie jede andere ein Aufrufer blockiert wird, bis eine Funktion Ausführung zurückgegeben. Die erste Gelegenheit für eine Coroutine zurückzugebenden ist die erste `co_await`, `co_return`, oder `co_yield`.

Also vor dem Ausführen rechnergebundene Aufgaben in einer Coroutine, müssen Sie die Ausführung an den Aufrufer zurückgeben (das heißt, führen Sie einen Unterbrechungspunkt), damit der Aufrufer nicht blockiert ist. Wenn Sie noch nicht geschehen, das Durchführen von `co_await`- Verbindung, die einen anderen Vorgang, Sie können `co_await` der [ **winrt::resume_background** ](/uwp/cpp-ref-for-winrt/resume-background) Funktion. Dadurch wird die Steuerung an den Aufrufer zurückgegeben, und unmittelbar danach wird die Ausführung auf einem Threadpoolthread fortgesetzt.

Der in der Implementierung verwendete Threadpool wird auf niedriger Ebene ausgeführt [Windows-Threadpool](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api), sodass er optimal effizient ist.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

Dieses Szenario erweitert das vorherige. Sie lagern einige Aufgaben an den Threadpool aus, möchten dann aber den Fortschritt auf der Benutzeroberfläche anzeigen.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

Der obige Code löst eine [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread)-Ausnahme aus, da ein **TextBlock** von dem Thread, der ihn erstellt hat (UI-Thread), aktualisiert werden muss. Eine Lösung besteht darin, den Threadkontext zu erfassen, in dem unsere Coroutine ursprünglich aufgerufen wurde. Instanziieren zu diesem Zweck eine [ **winrt::apartment_context** ](/uwp/cpp-ref-for-winrt/apartment-context) Objekt, im Hintergrund Arbeit, und klicken Sie dann `co_await` der **Apartment_context** So wechseln Sie zurück an die aufrufende Kontext.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

Solange die oben genannte Coroutine vom UI-Thread aufgerufen wird, der den **TextBlock** erstellt hat, funktioniert dieses Verfahren. Es wird einige Fälle in Ihrer App geben, in denen Sie dessen sicher sind.

Für eine allgemeine Lösung zum Aktualisieren der Benutzeroberfläche, die Fälle behandelt, in dem Sie den aufrufenden Thread nicht sicher sind, können Sie `co_await` der [ **winrt::resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) Funktion, die in einer bestimmten wechseln Vordergrundthread. Im folgenden Codebeispiel geben Sie den Vordergrundthread durch Übergabe des Dispatcherobjekts an, das mit dem **TextBlock** verknüpft ist (durch Zugriff auf die zugehörige [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)-Eigenschaft). Die Implementierung von **winrt::resume_foreground** ruft [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) für dieses Dispatcherobjekt auf, um die Aufgaben auszuführen, die danach in der Coroutine folgen.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Fortsetzen, und wechseln in einer Coroutine Ausführungskontexte

Im Allgemeinen können nach einen Unterbrechungspunkt in einer Coroutine, der ursprüngliche Thread der Ausführung möglicherweise verschwinden, Wiederaufnahme von kann auf einen beliebigen Thread auftreten (das heißt, kann einen beliebigen Thread Aufrufen der **abgeschlossen** Methode für den asynchronen Vorgang).

Jedoch möglich, wenn Sie `co_await` alle der vier Windows-Runtime asynchronen Vorgang (**IAsyncXxx**) ist dann C++ / WinRT erfasst an dem Punkt im aufrufenden Kontext Sie `co_await`. Und es wird sichergestellt, dass Sie immer noch auf dieses Kontexts sind, wenn die Fortsetzung fortgesetzt wird. C++ / WinRT ist dies überprüfen, ob Sie bereits im aufrufenden Kontext sind und, falls nicht, dann zu diesem wechseln. Würden Sie in einem Singlethread-Apartment (STA)-Thread vor dem `co_await`, werden Sie auch auf demselben Knoten anschließend; würden Sie in einem Multithread-Apartment (MTA)-Thread vor dem `co_await`, und Sie anschließend auf einem sein werden.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

Sie können dieses Verhalten abhängig ist, weil C++ / WinRT stellt Code aus, um diese Typen Windows-Runtime asynchronen Vorgang, um die sprachunterstützung für C++-Coroutine anpassen (diesen Codeabschnitten heißen Wait-Adapter). Die restlichen awaitable-Typen in C++ / WinRT, sind einfache Wrapper für Thread-Pools und/oder Hilfsprogramme; So führen sie auf den Threadpool.

```cppwinrt
...
#include <winrt/coroutine.h>
...
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

Wenn Sie `co_await` eine andere Art&mdash;auch in C++ / WinRT Coroutine Implementierung&mdash;einer anderen Bibliothek stellt der Adapter, und Sie müssen verstehen, was tun, diese Adapter im Hinblick auf die Wiederaufnahme und Kontexte.

Um Kontextwechsel auf ein Minimum zu halten, können Sie einige der Techniken verwenden, die wir bereits, in diesem Thema gesehen haben. Sehen wir uns einige Abbildungen der Änderungen. In Pseudocode nächsten Beispiel gezeigt, den Überblick über einen Ereignishandler, der Aufrufe von einer Windows-Runtime-API zum Laden eines Bilds, fällt in einem Hintergrundthread ausgeführt, um das Image zu verarbeiten und sendet Sie an den UI-Thread zur Anzeige des Bilds in der Benutzeroberfläche zurück.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

Für dieses Szenario ist ein wenig Ineffiency rund um den Aufruf **StorageFile::OpenAsync**. Besteht ein Bedarf Kontextwechsel zu einem Hintergrund thread (sodass der Handler für die Ausführung an den Aufrufer zurückgeben kann), bei Wiederaufnahme nach dem die C + c++ / WinRT stellt den Kontext des UI-Threads. Aber in diesem Fall ist es nicht erforderlich, werden im UI-Thread, bis wir sind dabei, die die Benutzeroberfläche zu aktualisieren. Je mehr Windows-Runtime-APIs Wir rufen *vor* unseren Aufruf von **winrt::resume_background**, die nicht mehr benötigte hin und her Kontextwechsel wir anfallen. Die Lösung besteht nicht darin Aufrufen *alle* Windows-Runtime-APIs vorher. Alle nach dem Verschieben der **winrt::resume_background**.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

"Await"-Adapter, wenn Sie möchten etwas schwieriger ist und Sie Ihren eigenen schreiben können. Wenn Sie möchten z. B. eine `co_await` auf dem gleichen Thread fortgesetzt werden, die auf die asynchrone Aktion abgeschlossen ist (also, es gibt keine Kontextwechsel), wird Sie durch das Schreiben von beginnen können "await"-Adapter wie die unten aufgeführten.

> [!NOTE]
> Im folgenden Codebeispiel wird nur zu Lernzwecken bereitgestellt wird. Es ist, die Ihnen den Einstieg erleichtern Verständnis wie "await" verwenden. Wenn Sie dieses Verfahren in Ihrer eigenen Codebasis, verwenden möchten, und es wird empfohlen, die Sie entwickeln und Testen Ihre eigenen "await" Adapter Struct(s). Sie können z. B. schreiben **Complete_on_any**, **Complete_on_current**, und **complete_on(dispatcher)** . Berücksichtigen Sie auch, sodass sie Vorlagen, die verwenden der **IAsyncXxx** Typ als einen Vorlagenparameter.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

Informationen zum Verwenden der **Nicht_wechseln** "await"-Adapter, zunächst müssen Sie wissen, bei der C++ Compiler findet eine `co_await` Ausdruck, Funktionen, die aufgerufen sucht **Await_ready**, **"await_suspend"** , und **"await_resume"** . C++ / WinRT-Bibliothek bietet diese Funktionen, damit Sie sinnvolle Verhalten standardmäßig wie folgt abrufen.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Verwenden der **Nicht_wechseln** "await"-Adapter, ändern Sie den Typ der, die einfach `co_await` Ausdruck aus **IAsyncXxx** zu **Nicht_wechseln**, wie folgt aus.

```cppwinrt
...
#include <winrt/coroutine.h>
...
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

Klicken Sie dann für die drei suchen, statt **Await_xxx** Funktionen entsprechen **IAsyncXxx**, C++ sucht der Compiler für Funktionen, die mit übereinstimmen **Nicht_wechseln**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Abbrechen von einem asynchrone Vorgang und Abbruch-Rückrufe

Die Windows-Runtime-Funktionen für die asynchrone Programmierung können Sie eine in-Flight asynchrone Aktion oder Operation abzubrechen. Es folgt ein Beispiel, das Aufrufe [ **StorageFolder::GetFilesAsync** ](/uwp/api/windows.storage.storagefolder.getfilesasync) zum Abrufen einer möglicherweise großen Auflistung von Dateien und speichert die resultierende asynchrone Vorgang in einen Datenmember. Der Benutzer hat die Möglichkeit, den Vorgang abzubrechen.

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/coroutine.h>
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

Für die Implementierung-Seite des Abbruchs beginnen wir mit einem einfachen Beispiel.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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

Wenn Sie das obige Beispiel ausführen, sehen Sie **ImplicitCancellationAsync** Drucken eine Nachricht pro Sekunde für drei Sekunden, nach der Zeit, die er automatisch beendet wird. daher wird abgebrochen. Dies funktioniert, da auf Auftreten einer `co_await` Ausdruck eine Coroutine wird überprüft, ob er abgebrochen wurde. Falls Ja, wird verkürzt dann es; und wenn dies nicht der Fall, es hält dann wie gewohnt.

Abbruch, natürlich möglich, solange die Coroutine angehalten wird. Nur, wenn die Coroutine fortgesetzt wird, oder eine andere erreicht `co_await`, überprüft er, für den Abbruch. Das Problem ist möglicherweise zu grob-umfassendere Latenz bei der Reaktion auf einen Abbruch.

Eine weitere Option ist explizit für den Abbruch von innerhalb der Coroutine abrufen. Aktualisieren Sie im Beispiel oben durch den Code in der Liste unten aus. In diesem Beispiel neue **ExplicitCancellationAsync** Ruft ab, das von zurückgegebene Objekt der [ **winrt::get_cancellation_token** ](/uwp/cpp-ref-for-winrt/get-cancellation-token) Funktion, und wird verwendet, um in regelmäßigen Abständen Überprüfen Sie, ob die Coroutine wurde abgebrochen. Solange er nicht abgebrochen wird, wird die Coroutine auf unbestimmte Zeit Schleifen; Nachdem sie abgebrochen wird, beendet die Schleife und die Funktion normal. Das Ergebnis ist identisch, da diese explizit erfolgt der vorherigen Beispiel, aber hier beenden, und unter Kontrolle.

```cppwinrt
...
#include <winrt/coroutine.h>
...
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

Warten auf **winrt::get_cancellation_token** Ruft ein Abbruchtoken, die über Kenntnisse der **IAsyncAction** , die die Coroutine in Ihrem Auftrag erzeugt. Sie können den Funktionsaufruf-Operator auf dieses Token verwenden, zum Abfragen des abbruchstatus&mdash;im Wesentlichen für den Abbruch abrufen. Wenn Sie einen rechnergebundenen Vorgang ausführen, oder eine umfangreiche Sammlung durchlaufen, ist dies eine sinnvolle Methode.

### <a name="register-a-cancellation-callback"></a>Registrieren eines Rückrufs Abbruch

Die Windows-Runtime-Abbruch nicht automatisch auf andere asynchrone Objekte übertragen werden. Aber&mdash;eingeführt in Version 10.0.17763.0 (Windows 10, Version 1809) des Windows SDK&mdash;können Sie einen abbruchsrückruf registrieren. Dies ist eine Präemptive Hook, nach dem Abbruch weitergegeben werden kann und macht es möglich, vorhandene Parallelitätsbibliotheken integrieren.

In diesem Codebeispiel weiter **NestedCoroutineAsync** funktioniert das, aber es enthält keine speziellen Abbruchlogik. **CancellationPropagatorAsync** ist im Wesentlichen ein Wrapper für die geschachtelte Coroutine, leitet der Wrapper Abbruch vorsorglich weiter.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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

**CancellationPropagatorAsync** registriert eine Lambda-Funktion für die eigene abbruchsrückruf, und klicken Sie dann "awaits" (angehalten) bis zum Abschluss der geschachtelten Arbeit. Wenn oder **CancellationPropagatorAsync** wird abgebrochen, es verteilt die Kündigung der geschachtelten Coroutine. Es gibt keine Notwendigkeit für den Abruf von Abbruch. noch ist Abbruch auf unbestimmte Zeit blockiert. Dieser Mechanismus ist flexibel genug für die Verwendung mit einer Coroutine oder die Parallelität-Bibliothek, die nichts c++ bekannt ist c++ / WinRT.

## <a name="reporting-progress"></a>Statusmeldung

Wenn Ihre Coroutine entweder zurückgibt [ **IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_), oder [ **IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), und klicken Sie dann abgerufen werden können die zurückgegebenes Objekt der [ **winrt::get_progress_token** ](/uwp/cpp-ref-for-winrt/get-progress-token) Funktion, und verwenden dieses Zugriffstokens zum Melden des Fortschritts an einem Handler ausgeführt. Dies ist ein Codebeispiel:

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/coroutine.h>
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
> Es ist nicht korrekt implementiert mehrere *Abschlusshandler* für eine asynchrone Aktion oder Operation. Sie können entweder einen einzelnen Delegaten für das abgeschlossene Ereignis verwenden, oder Sie können `co_await` es. Wenn Sie beides haben, misslingt die zweite. Entweder eine der folgenden Arten von Handlern für Abschluss eignet sich; nicht beide für das gleiche Async-Objekt.

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

Weitere Informationen über den Abschluss Handler, finden Sie unter [Delegattypen für asynchrone Aktionen und Vorgänge](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Auslösen und vergessen

In einigen Fällen haben, dass eine Aufgabe, die gleichzeitig mit anderen Aufgaben ausgeführt werden kann, und Sie müssen nicht warten, bis die Aufgabe abgeschlossen (keine andere Verarbeitung hängt es), Sie brauchen auch einen Wert zurückgeben. In diesem Fall können Sie den Task auszulösen und vergessen es. Sie erreichen, die durch Schreiben einer Coroutine, deren Rückgabetyp ist [ **winrt::fire_and_forget** ](/uwp/cpp-ref-for-winrt/fire-and-forget) (statt in einem asynchronen Vorgang die Windows-Runtime Typen oder **Concurrency:: Task**).

```cppwinrt
// main.cpp
#include <winrt/coroutine.h>
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

## <a name="important-apis"></a>Wichtige APIs
* [Concurrency:: Task-Klasse](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction-Schnittstelle](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; Schnittstelle](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; Schnittstelle](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; interface](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync-Methode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed-Klasse](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Verwandte Themen
* [Behandeln von Ereignissen mithilfe von Delegaten in C++ / WinRT](handle-events.md)
* [C++-Standarddatentypen und C++/WinRT](std-cpp-data-types.md)
