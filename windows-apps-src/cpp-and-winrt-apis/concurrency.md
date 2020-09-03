---
description: Dieses Thema zeigt, wie Sie asynchrone Windows-Runtime-Objekte mit C++/WinRT erstellen und nutzen können.
title: Parallelität und asynchrone Vorgänge mit C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, Parallelität, async, asynchron, Asynchronität
ms.localizationpriority: medium
ms.openlocfilehash: a10962740d3f723a855914595ea02d0688ff9707
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170374"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Parallelität und asynchrone Vorgänge mit C++/WinRT

> [!IMPORTANT]
> In diesem Thema werden die Konzepte der *Coroutinen* und `co_await` vorgestellt, die Sie sowohl in Ihren UI- *als auch* in Ihren Nicht-UI-Anwendungen verwenden sollten. Der Einfachheit halber zeigen die meisten Codebeispiele in diesem Einführungsthema Projekte der **Windows-Konsolenanwendung (C++/WinRT)** . Die späteren Codebeispiele in diesem Thema verwenden zwar Coroutinen, aber der Einfachheit halber verwenden die Konsolenanwendungsbeispiele auch weiterhin den blockierenden **get**-Funktionsaufruf kurz vor dem Beenden, sodass die Anwendung nicht beendet wird, bevor der Druck der Ausgabe abgeschlossen ist. Dies (die blockierende **get**-Funktion aufrufen) werden Sie nicht von einem UI-Thread aus tun. Stattdessen verwenden Sie die `co_await`-Anweisung. Die Techniken, die Sie in Ihren UI-Anwendungen verwenden, werden im Thema [Erweiterte Parallelität und Asynchronie](concurrency-2.md) beschrieben.

In diesem Einführungsthema werden einige der Möglichkeiten beschrieben, wie Sie asynchrone Windows-Runtime-Objekte mit [C++/WinRT](./intro-to-using-cpp-with-winrt.md) erstellen und nutzen können. Lesen Sie nach der Lektüre dieses Themas insbesondere für Techniken, die Sie in Ihren UI-Anwendungen verwenden, auch [Erweiterte Parallelität und Asynchronie](concurrency-2.md).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Asynchrone Vorgänge und „Async“-Funktionen der Windows-Runtime

Jede Windows-Runtime-API, deren Ausführung länger als 50 Millisekunden dauern kann, wird als asynchrone Funktion implementiert (mit einem Namen, der auf „Async” endet). Die Implementierung einer asynchronen Funktion initiiert die Arbeit in einem anderen Thread und gibt sofort ein Objekt zurück, das den asynchronen Vorgang darstellt. Wenn der asynchrone Vorgang abgeschlossen ist, enthält das zurückgegebene Objekt einen aus seiner Ausführung resultierenden Wert. Der Windows-Runtime-Namespace **Windows::Foundation** enthält vier Objekttypen für asynchrone Vorgänge.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1) und
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).

Jeder dieser asynchronen Vorgangstypen wird auf einen entsprechenden Typ im C++/WinRT-Namespace **winrt::Windows::Foundation** projiziert. C++/WinRT enthält außerdem eine interne Await-Adapter-Struktur. Sie verwenden diese Struktur nicht direkt, können dank ihr aber eine `co_await`-Anweisung schreiben, um kooperativ auf das Ergebnis einer Funktion zu warten, die einen dieser asynchronen Vorgangstypen zurückgibt. Zudem können Sie eigene Coroutinen schreiben, die diese Typen zurückgeben.

Ein Beispiel für eine asynchrone Windows-Funktion ist [**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync). Diese Funktion gibt ein asynchrones Vorgangsobjekt vom Typ [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2) zurück.

Sehen wir uns einige Möglichkeiten an (&mdash;zuerst blockierende und dann nicht blockierende&mdash;), wie Sie eine solche API mit C++/WinRT aufrufen können. Nur zur Veranschaulichung der Grundgedanken werden wir in den nächsten Codebeispielen ein Projekt der **Windowskonsolenanwendung (C++/WinRT)** verwenden. Techniken, die für eine UI-Anwendung besser geeignet sind, werden unter [Erweiterte Parallelität und Asynchronie](concurrency-2.md) behandelt.

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

Wie Sie sehen können, verwenden wir im obigen Codebeispiel weiterhin den blockierenden **get**-Funktionsaufruf kurz vor dem Beenden des **main**-Abschnitts. Aber nur, damit die Anwendung nicht beendet wird, bevor der Druck der Ausgabe abgeschlossen ist.

## <a name="asynchronously-return-a-windows-runtime-type"></a>Asynchrone Rückgabe eines Windows-Runtime-Typs

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

Wenn Sie einen Windows-Runtime-Typ asynchron zurückgeben, sollten Sie einen [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1) oder [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2) zurückgeben. Qualifiziert sind alle Erst- oder Drittanbieter-Runtime-Klassen oder Typen, die an oder von eine(r) Windows-Runtime-Funktion übergeben werden können (z. B. `int` oder **winrt::hstring**). Der Compiler gibt einen Fehler vom Typ *WinRT-Typ erforderlich* aus, wenn Sie versuchen, einen dieser asynchronen Vorgangstypen mit einem Nicht-Windows-Runtime-Typ zu verwenden.

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

## <a name="asynchronously-return-a-non-windows-runtime-type"></a>Asynchrone Rückgabe eines Nicht-Windows-Runtime-Typs

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

    co_await DoOtherWorkAsync(); // (this is the first suspension point)...

    // ...accessing value here carries no guarantees of safety.
}
```

In einer Coroutine verläuft die Ausführung synchron bis zum ersten Anhaltepunkt – an diesem wird die Steuerung an den Aufrufer zurückgegeben, und der aufrufende Frame fällt aus dem Gültigkeitsbereich heraus. Bis die Coroutine fortgesetzt wird, kann alles Mögliche mit dem Quellwert passiert sein, auf den ein Verweisparameter verweist. Aus Sicht der Coroutine hat ein Verweisparameter eine unkontrollierte Lebensdauer. Im obigen Beispiel können wir also bis zur `co_await`-Anweisung problemlos auf *value* zugreifen, aber nicht danach. Falls dieser *value* vom Aufrufer zerstört wird, verursacht der anschließende Versuch, in der Coroutine darauf zuzugreifen, eine Beschädigung des Speichers. Wir können *value* nicht sicher an **DoOtherWorkAsync** übergeben, wenn das Risiko besteht, dass diese Funktion dadurch angehalten wird und nach dem Fortsetzen versucht, *value* zu verwenden.

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

Siehe [Starke und schwache Verweise in C++/WinRT](./weak-references.md#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

## <a name="important-apis"></a>Wichtige APIs
* [concurrency::task-Klasse](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction-Schnittstelle](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [IAsyncOperation&lt;TResult&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncoperation-1)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;-Schnittstelle](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [SyndicationClient::RetrieveFeedAsync-Methode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed-Klasse](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>Verwandte Themen
* [Erweiterte Parallelität und Asynchronie](concurrency-2.md)
* [Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT](handle-events.md)
* [C++-Standarddatentypen und C++/WinRT](std-cpp-data-types.md)