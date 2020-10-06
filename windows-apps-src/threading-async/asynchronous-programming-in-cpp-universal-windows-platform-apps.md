---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: In diesem Artikel wird die empfohlene Vorgehensweise zur Verwendung asynchroner Methoden in Visual C++-Komponentenerweiterungen (C++/CX) mithilfe der Task-Klasse beschrieben, die im Concurrency-Namespace in „ppltasks.h“ definiert wird.
title: Asynchrone Programmierung in C++
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, Threads, Asynchronous, C++
ms.localizationpriority: medium
ms.openlocfilehash: e08a73c7617a5b24af49d5b3665303124e28d257
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750156"
---
# <a name="asynchronous-programming-in-ccx"></a>Asynchrone Programmierung in C++/CX
> [!NOTE]
> Dieses Thema enthält Informationen, die Sie beim Verwalten Ihrer C++/CX-Anwendung unterstützen. Es wird jedoch empfohlen, [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) für neue Anwendungen zu verwenden. C++/WinRT ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet.

In diesem Artikel wird die empfohlene Vorgehensweise zur Verwendung asynchroner Methoden in Visual C++-Komponentenerweiterungen (C++/CX) mithilfe der `task`-Klasse beschrieben, die im `concurrency`-Namespace in „ppltasks.h“ definiert wird.

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>Asynchrone UWP-Typen (Universelle Windows-Plattform)
Die Universelle Windows-Plattform (UWP) umfasst ein ausgearbeitetes Modell für den Aufruf asynchroner Methoden und stellt die Typen bereit, die Sie für die Verwendung dieser Methoden benötigen. Wenn Sie mit dem asynchronen UWP-Modell nicht vertraut sind, lesen Sie die [asynchrone Programmierung][Asyncprogramming] , bevor Sie den Rest dieses Artikels lesen.

Obwohl Sie die asynchronen Windows-Runtime-APIs direkt in C++ nutzen können, ist der bevorzugte Ansatz die Verwendung der [**Task-Klasse**][task-class] und der zugehörigen Typen und Funktionen, die im Parallelitäts [**Namespace**][concurrencyNamespace] enthalten und in definiert sind `<ppltasks.h>` . **concurrency::task** ist eine Klasse für allgemeine Zwecke. Bei Verwendung des **/ZW**-Compilerschalters, der für UWP-Apps und die entsprechenden Komponenten benötigt wird, werden jedoch die asynchronen Typen der UWP durch die Task-Klasse gekapselt, was Folgendes vereinfacht:

-   Verkettung mehrerer asynchroner und synchroner Vorgänge

-   Behandlung von Ausnahmen in Aufgabenabfolgen

-   Ausführung von Abbrüchen in Aufgabenabfolgen

-   Gewährleistung, dass einzelne Aufgaben im richtigen Threadkontext oder -apartment ausgeführt werden

Dieser Artikel bietet einen allgemeinen Überblick über die Verwendung der **Task**-Klasse mit den asynchronen UWP-APIs. Eine ausführlichere Dokumentation zu **Aufgaben** und den zugehörigen Methoden, einschließlich [**Create \_ Task**][createTask], finden Sie unter [Task parallelism (Concurrency Runtime)][taskParallelism]. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>Verwendung asynchroner Vorgänge mithilfe von Aufgaben
Im folgenden Beispiel wird gezeigt, wie die Task-Klasse verwendet wird, um eine asynchrone Methode zu verwenden, **die eine** [**iasyncoperation**][IAsyncOperation] -Schnittstelle zurückgibt und deren Operation einen Wert erzeugt. Im Folgenden finden Sie die grundlegenden Schritte:

1.  Rufen Sie die `create_task`-Methode auf, und übergeben Sie diese an das **IAsyncOperation^**-Objekt.

2.  Rufen Sie den Member Function [**Task::**][taskThen] für die Aufgabe auf, und geben Sie einen Lambda-Ausdruck an, der aufgerufen wird, wenn der asynchrone Vorgang abgeschlossen wird.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
using namespace Windows::Devices::Enumeration;
...
void App::TestAsync()
{    
    //Call the *Async method that starts the operation.
    IAsyncOperation<DeviceInformationCollection^>^ deviceOp =
        DeviceInformation::FindAllAsync();

    // Explicit construction. (Not recommended)
    // Pass the IAsyncOperation to a task constructor.
    // task<DeviceInformationCollection^> deviceEnumTask(deviceOp);

    // Recommended:
    auto deviceEnumTask = create_task(deviceOp);

    // Call the task's .then member function, and provide
    // the lambda to be invoked when the async operation completes.
    deviceEnumTask.then( [this] (DeviceInformationCollection^ devices )
    {       
        for(int i = 0; i < devices->Size; i++)
        {
            DeviceInformation^ di = devices->GetAt(i);
            // Do something with di...          
        }       
    }); // end lambda
    // Continue doing work or return...
}
```

Der Task, der von der [**Task:: Then**][taskThen] -Funktion erstellt und zurückgegeben wird, wird als *Fortsetzung*bezeichnet. Das Eingabeargument für den benutzerdefinierten Lambda-Ausdruck ist (in diesem Fall) das Ergebnis der beendeten Aufgabe. Derselbe Wert würde auch mit einem Aufruf von [**IAsyncOperation::GetResults**](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_#Windows_Foundation_IAsyncOperation_1_GetResults) bei direkter Verwendung der **IAsyncOperation**-Schnittstelle abgerufen werden.

Die [**Task:: Then**][taskThen] -Methode wird sofort zurückgegeben, und Ihr Delegat wird erst ausgeführt, wenn die asynchrone Arbeit erfolgreich abgeschlossen wurde. In diesem Beispiel wird die Fortsetzung nicht ausgeführt, wenn der asynchrone Vorgang eine Ausnahme auslöst oder aufgrund einer Abbruchanforderung mit einem Abbruch beendet wird. Weiter unten erfahren Sie, wie Sie Fortsetzungen schreiben, die auch bei einem Abbruch oder Fehler der vorhergehenden Aufgabe ausgeführt werden.

Sie deklarieren die Aufgabenvariable zwar im lokalen Stapel, aber die Lebensdauer wird so verwaltet, dass die Variable erst gelöscht wird, wenn alle zugehörigen Vorgänge abgeschlossen sind und alle Verweise auf die Variable außerhalb des Bereichs liegen – auch dann, wenn der Aufruf der Methode vor Abschluss des Vorgangs beendet wird.

## <a name="creating-a-chain-of-tasks"></a>Erstellen von Aufgabenabfolgen
Bei der asynchronen Programmierung werden in vielen Fällen Abfolgen von Vorgängen definiert, sogenannte *Aufgabenabfolgen*, bei denen eine Fortsetzung immer erst ausgeführt wird, wenn die vorhergehende Aufgabe beendet ist. In manchen Fällen erzeugt die letzte (oder *vorhergehende*) Aufgabe einen Wert, den die Fortsetzung als Eingabe akzeptiert. Mithilfe der [**Task:: Then**][taskThen] -Methode können Sie Task Ketten auf intuitive und unkomplizierte Weise erstellen. die-Methode gibt **eine <T> Aufgabe** zurück, wobei **T** der Rückgabetyp der Lambda-Funktion ist. Eine Aufgabenabfolge kann mehrere Fortsetzungen enthalten: `myTask.then(…).then(…).then(…);`

Aufgabenabfolgen sind insbesondere dann nützlich, wenn eine Fortsetzung einen neuen asynchronen Vorgang erstellt. Solche Aufgaben werden als asynchrone Aufgaben bezeichnet. Das folgende Beispiel zeigt eine Aufgabenabfolge mit zwei Fortsetzungen. Die einleitende Aufgabe ruft der Handle für eine bestehende Datei ab. Nach Abschluss dieses Vorgangs startet die erste Fortsetzung einen neuen asynchronen Vorgang, mit dem die Datei gelöscht wird. Nach Abschluss dieses Vorgangs wird die zweite Fortsetzung ausgeführt, die eine Bestätigungsmeldung ausgibt.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

Das Beispiel oben zeigt vier zentrale Aspekte:

-   Die erste Fortsetzung konvertiert das [**iasyncaction ^**][IAsyncAction] -Objekt in eine **Aufgabe <void> ** und gibt die **Aufgabe**zurück.

-   Die zweite Fortsetzung führt keine Fehlerbehandlung durch und verwendet daher **void** als Eingabe (und nicht **task<void>**). Es handelt sich um eine wertbasierte Fortsetzung.

-   Die zweite Fortsetzung wird erst ausgeführt, wenn der [**deleteasync**][deleteAsync] -Vorgang abgeschlossen ist.

-   Da die zweite Fortsetzung Wert basiert ist, wird die zweite Fortsetzung überhaupt nicht ausgeführt, wenn der Vorgang, der durch den Aufrufen von [**deleteasync**][deleteAsync] gestartet wurde, eine Ausnahme auslöst.

**Hinweis**    Das Erstellen einer Aufgaben Kette ist nur eine der Möglichkeiten, mit der **Task** -Klasse asynchrone Vorgänge zu erstellen. Sie können Vorgänge auch verfassen, indem Sie die Operatoren Join und Choice **&&** und verwenden **||** . Weitere Informationen finden Sie unter [Task parallelism (Concurrency Runtime)][taskParallelism].

## <a name="lambda-function-return-types-and-task-return-types"></a>Rückgabetypen für Lambda-Funktionen und Aufgaben
Bei Aufgabenfortsetzungen ist der Rückgabetyp der Lambda-Funktion von einem **task**-Objekt umschlossen. Wenn die Lambda-Funktion den **double**-Typ zurückgibt, hat die Fortsetzungsaufgabe den **task<double>**-Typ. Das Aufgabenobjekt ist jedoch so konzipiert, dass es keine unnötig geschachtelten Rückgabetypen erzeugt. Wenn eine Lambda-Funktion den **IAsyncOperation<SyndicationFeed^>^**-Typ zurückgibt, gibt die Fortsetzung den **task<SyndicationFeed^>**-Typ und nicht **task<task<SyndicationFeed^>>** oder **task<IAsyncOperation<SyndicationFeed^>^>^** zurück. Dieser Vorgang wird als *asynchrones Entpacken* bezeichnet und sorgt dafür, dass der asynchrone Vorgang in der Fortsetzung vor dem Aufruf der nächsten Fortsetzung beendet wird.

Beachten Sie im vorherigen Beispiel, dass der Task eine **Aufgabe <void> ** zurückgibt, auch wenn der Lambda-Ausdruck ein [**iasyncinfo**][IAsyncInfo] -Objekt zurückgegeben hat. Die folgende Tabelle gibt einen Überblick über die Typkonvertierungen zwischen Lambda-Funktionen und den einschließenden Aufgaben:

| Lambda-Rückgabetyp | `.then`-Rückgabetyp |
| ------------------ | ------------------- |
| TResult                                                | Task<TResult> |
| IAsyncOperation<TResult>^                        | Task<TResult> |
| IAsyncOperationWithProgress<TResult, TProgress>^ | Task<TResult> |
|IAsyncAction^                                           | Task<void>    |
| IAsyncActionWithProgress<TProgress>^             |Task<void>     |
| Task<TResult>                                    |Task<TResult>  |


## <a name="canceling-tasks"></a>Abbrechen von Aufgaben
In vielen Fällen ist es ratsam, Benutzern die Möglichkeit zum Abbrechen eines asynchronen Vorgangs zu geben. Mitunter müssen Sie außerdem einen Vorgang programmgesteuert von außerhalb der Aufgabenabfolge abbrechen. Obwohl jeder \* **Async** -Rückgabetyp über eine [**Cancel**][Iasyncinfocancel] -Methode verfügt, die er von [**iasyncinfo**][IAsyncInfo]erbt, ist es umständlich, ihn für externe Methoden verfügbar zu machen. Die bevorzugte Methode zur Unterstützung von Abbruch in einer Aufgaben Kette besteht darin, eine [**Abbruch \_ Token- \_ Quelle**](/cpp/parallel/concrt/reference/cancellation-token-source-class) zu verwenden, um ein [**Abbruch \_ Token**](/cpp/parallel/concrt/reference/cancellation-token-class)zu erstellen, und das Token dann an den Konstruktor der anfänglichen Aufgabe zu übergeben. Wenn eine asynchrone Aufgabe mit einem Abbruch Token erstellt und [**Abbruch \_ Token \_ Source:: Cancel**](/cpp/parallel/concrt/reference/cancellation-token-source-class?view=vs-2017) aufgerufen wird, ruft der Task automatisch **Abbrechen** für den **iasync \* ** -Vorgang auf und übergibt die Abbruch Anforderung an die zugehörige Fortsetzungs Kette. Im folgenden Pseudocode wird dieses grundlegende Konzept dargestellt.

``` cpp
//Class member:
cancellation_token_source m_fileTaskTokenSource;

// Cancel button event handler:
m_fileTaskTokenSource.cancel();

// task chain
auto getFileTask2 = create_task(documentsFolder->GetFileAsync(fileName),
                                m_fileTaskTokenSource.get_token());
//getFileTask2.then ...
```

Wenn ein Task abgebrochen wird, wird die Ausnahme einer [**Aufgabe \_ abgebrochen**][taskCanceled] in der Aufgaben Kette weitergegeben. Wert basierte Fortsetzungen werden einfach nicht ausgeführt, aber aufgabenbasierte Fortsetzungen bewirken, dass die Ausnahme ausgelöst wird, wenn [**Task:: Get**][taskGet] aufgerufen wird. Wenn Sie eine Fehler Behandlungs Fortsetzung haben, stellen Sie sicher, dass die Ausnahme vom Typ " **Aufgabe \_ abgebrochen** " explizit abgefangen wird. (Diese Ausnahme wird nicht von [**Platform::Exception**](/cpp/cppcx/platform-exception-class) abgeleitet.)

Abbruchvorgänge sind kooperativ. Wenn eine Fortsetzung neben dem Aufruf einer UWP-Methode zeitintensive Vorgänge ausführt, ist es wichtig, dass Sie den Status des Abbruchtokens regelmäßig überprüfen und die Ausführung bei einem Abbruch beenden. Nachdem Sie alle in der Fortsetzung zugeordneten Ressourcen bereinigt haben, rufen Sie die [** \_ aktuelle \_ Aufgabe abbrechen**](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) auf, um diese Aufgabe abzubrechen und den Abbruch an alle nachfolgenden Wert basierten Fortsetzungen weiterzugeben. Ein weiteres Beispiel: Sie können Aufgabenabfolgen erstellen, die das Ergebnis eines [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker)-Vorgangs darstellen. Wenn der Benutzer die Schaltfläche **Abbrechen** auswählt, wird die [**iasyncinfo:: Cancel**][Iasyncinfocancel] -Methode nicht aufgerufen. Stattdessen wird der Vorgang erfolgreich ausgeführt, allerdings mit der Rückgabe **nullptr**. Die Fortsetzung kann den Eingabeparameter testen und **die \_ aktuelle \_ Aufgabe abbrechen** aufrufen, wenn die Eingabe **nullptr**ist.

Weitere Informationen finden Sie unter [Abbruch in der PPL](/cpp/parallel/concrt/cancellation-in-the-ppl).

## <a name="handling-errors-in-a-task-chain"></a>Behandeln von Fehlern in Aufgabenabfolgen
Wenn eine Fortsetzung auch dann ausgeführt werden soll, wenn der Vorgänger abgebrochen wurde oder eine Ausnahme ausgelöst hat, führen Sie die Fortsetzung zu einer aufgabenbasierten Fortsetzung aus, indem Sie die Eingabe für die Lambda-Funktion als **Aufgabe <TResult> ** oder **Aufgabe <void> ** angeben, wenn der Lambda-Ausdruck der Vorgänger Aufgabe eine [**iasyncaction ^**][IAsyncAction]zurückgibt.

Zur Behandlung von Fehlern und Abbrüchen in Aufgabenabfolgen müssen jedoch nicht alle Fortsetzungen aufgabenbasiert sein, und Sie müssen nicht alle Vorgänge einschließen, die in einem `try…catch`-Block ausgelöst werden können. Stattdessen können Sie am Ende der Abfolge eine aufgabenbasierte Fortsetzung hinzufügen und alle Fehler dort behandeln. Jede Ausnahme – Dies schließt eine Ausnahme vom Typ " [**Aufgabe \_ abgebrochen**][taskCanceled] " ein – wird die Aufgaben Kette übertragen und alle Wert basierten Fortsetzungen umgehen, sodass Sie Sie in der aufgabenbasierten Fortsetzung der Fehlerbehandlung behandeln können. Das Beispiel oben kann daher mit einer aufgabenbasierten Fortsetzung für die Fehlerbehandlung umgeschrieben werden:

``` cpp
#include <ppltasks.h>
void App::DeleteWithTasksHandleErrors(String^ fileName)
{    
    using namespace Windows::Storage;
    using namespace concurrency;

    StorageFolder^ documentsFolder = KnownFolders::DocumentsLibrary;
    auto getFileTask = create_task(documentsFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample)
    {       
        return storageFileSample->DeleteAsync();
    })

    .then([](task<void> t)
    {

        try
        {
            t.get();
            // .get() didn' t throw, so we succeeded.
            OutputDebugString(L"File deleted.");
        }
        catch (Platform::COMException^ e)
        {
            //Example output: The system cannot find the specified file.
            OutputDebugString(e->Message->Data());
        }

    });
}
```

In einer aufgabenbasierten Fortsetzung wird der Member Function [**Task:: Get**][taskGet] aufgerufen, um die Ergebnisse der Aufgabe zu erhalten. " **Task:: Get** " muss auch dann aufgerufen werden, wenn es sich bei dem Vorgang um eine [**iasyncaction handelt**][IAsyncAction] , die kein Ergebnis erzeugt, da " **Task:: Get** " auch alle Ausnahmen abruft, die zur Aufgabe transportiert wurden. Wenn die Eingabeaufgabe eine Ausnahme speichert, wird diese mit dem Aufruf von **task::get** ausgelöst. Wenn Sie **Task:: Get**nicht aufzurufen oder keine aufgabenbasierte Fortsetzung am Ende der Kette verwenden oder den ausgelösten Ausnahmetyp nicht abfangen, wird eine **nicht beobachtete \_ Task \_ Ausnahme** ausgelöst, wenn alle Verweise auf den Task gelöscht wurden.

Sie sollten nur Ausnahmen abfangen, die Sie auch behandeln können. Wenn die App einen Fehler ausgibt, den Sie nicht beheben können, ist es besser, die App abstürzen zu lassen, als sie mit einem unbekannten Status weiter auszuführen. Außerdem sollten Sie im Allgemeinen nicht versuchen, die ** \_ \_ Ausnahme der nicht beobachteten Aufgabe selbst abzufangen** . Diese Ausnahme ist in erster Linie für Diagnosezwecke gedacht. Wenn eine **nicht beobachtete \_ Task \_ Ausnahme** ausgelöst wird, deutet dies in der Regel auf einen Fehler im Code hin. Die Ursache dafür ist entweder eine Ausnahme, die behandelt werden muss, oder eine nicht behebbare Ausnahme, die durch einen anderen Fehler im Code hervorgerufen wird.

## <a name="managing-the-thread-context"></a>Verwalten des Threadkontexts
Die Benutzeroberfläche von UWP-Apps wird in einem Singlethread-Apartment (STA) ausgeführt. Eine Aufgabe, deren Lambda-Ausdruck entweder [**iasyncaction**][IAsyncAction] oder [**iasyncoperation**][IAsyncOperation] zurückgibt, ist Apartment fähig. Wird die Aufgabe im STA erstellt, werden standardmäßig auch alle zugehörigen Fortsetzungen darin ausgeführt, sofern Sie nichts anderes angeben. Anders gesagt: Die gesamte Aufgabenabfolge erbt die Apartmentfähigkeit von der übergeordneten Aufgabe. Dieses Verhalten vereinfacht die Interaktion mit Benutzeroberflächen-Steuerelementen, auf die ausschließlich über das STA Zugriff besteht.

Beispielsweise können Sie in einer UWP-app in der Member-Funktion einer beliebigen Klasse, die eine XAML-Seite darstellt, ein [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) -Steuerelement aus einer [**Task:: Then**][taskThen] -Methode auffüllen, ohne das [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) -Objekt verwenden zu müssen.

``` cpp
#include <ppltasks.h>
void App::SetFeedText()
{    
    using namespace Windows::Web::Syndication;
    using namespace concurrency;
    String^ url = "http://windowsteamblog.com/windows_phone/b/wmdev/atom.aspx";
    SyndicationClient^ client = ref new SyndicationClient();
    auto feedOp = client->RetrieveFeedAsync(ref new Uri(url));

    create_task(feedOp).then([this]  (SyndicationFeed^ feed)
    {
        m_TextBlock1->Text = feed->Title->Text;
    });
}
```

Wenn ein Task keine [**iasyncaction**][IAsyncAction] oder [**iasyncoperation**][IAsyncOperation]zurückgibt, ist er nicht Apartment abhängig und wird standardmäßig auf dem ersten verfügbaren Hintergrund Thread ausgeführt.

Sie können den Standard Thread Kontext für jede Art von Aufgabe überschreiben, indem Sie die Überladung von [**Task::**][taskThen] verwenden, die einen [**Aufgaben \_ Fortsetzungs \_ Kontext**](/cpp/parallel/concrt/reference/task-continuation-context-class)annimmt. In manchen Fällen ist es zum Beispiel vorteilhaft, die Fortsetzung einer apartmentfähigen Aufgabe für einen Hintergrundthread zu planen. In einem solchen Fall können Sie den [**Task \_ Fortsetzungs \_ Kontext übergeben:: Verwenden Sie \_ beliebig**][useArbitrary] , um die Arbeit der Aufgabe im nächsten verfügbaren Thread in einem Multithreadapartment zu planen. Dadurch wird die Leistung der Fortsetzung verbessert, da die entsprechenden Vorgänge nicht mit anderen Vorgängen im UI-Thread synchronisiert sein müssen.

Im folgenden Beispiel wird veranschaulicht, wann es sinnvoll ist, den [**Task \_ Fortsetzungs \_ Kontext anzugeben:: use \_ beliebig**][useArbitrary] . Außerdem wird gezeigt, wie der standardmäßige Fortsetzungs Kontext zum Synchronisieren von gleichzeitigen Vorgängen für nicht Thread sichere Auflistungen nützlich ist. In diesem Codebeispiel wird eine Schleife für eine Liste mit URLs für RSS-Feeds ausgeführt, und für jede URL wird zum Abrufen der Feeddaten ein asynchroner Vorgang gestartet. Die Reihenfolge, in der die Feeds abgerufen werden, können Sie nicht steuern, was jedoch auch nicht wichtig ist. Mit dem Ende jedes [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.isyndicationclient.retrievefeedasync)-Vorgangs akzeptiert die erste Fortsetzung das [**SyndicationFeed^**](/uwp/api/Windows.Web.Syndication.SyndicationFeed)-Objekt und initialisiert damit ein App-spezifisches `FeedData^`-Objekt. Da jeder dieser Vorgänge unabhängig von den anderen Vorgängen ist, können wir die Dinge möglicherweise beschleunigen, indem wir den **Task \_ Fortsetzungs Kontext angeben: \_ : verwenden \_ ** Sie einen beliebigen Fortsetzungs Kontext. Nach dem Initialisieren jedes `FeedData`-Objekts müssen wir dieses jedoch einem [**Vector**](/cpp/cppcx/platform-collections-vector-class) hinzufügen – und das ist keine threadsichere Auflistung. Daher erstellen wir eine Fortsetzung und geben den [**Task \_ Fortsetzungs \_ Kontext:: use \_ Current**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) an, um sicherzustellen, dass alle Aufrufe zum [**Anfügen**](/uwp/api/windows.foundation.collections.ivector_t_.append) im gleichen Anwendungskontext des Single Thread-Apartment (AStA) auftreten. Da der [**Aufgaben \_ Fortsetzungs \_ Kontext:: use \_ default**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) der Standardkontext ist, müssen wir ihn nicht explizit angeben, aber wir gehen hier aus Gründen der Übersichtlichkeit.

``` cpp
#include <ppltasks.h>
void App::InitDataSource(Vector<Object^>^ feedList, vector<wstring> urls)
{
                using namespace concurrency;
    SyndicationClient^ client = ref new SyndicationClient();

    std::for_each(std::begin(urls), std::end(urls), [=,this] (std::wstring url)
    {
        // Create the async operation. feedOp is an
        // IAsyncOperationWithProgress<SyndicationFeed^, RetrievalProgress>^
        // but we don't handle progress in this example.

        auto feedUri = ref new Uri(ref new String(url.c_str()));
        auto feedOp = client->RetrieveFeedAsync(feedUri);

        // Create the task object and pass it the async operation.
        // SyndicationFeed^ is the type of the return value
        // that the feedOp operation will eventually produce.

        // Then, initialize a FeedData object by using the feed info. Each
        // operation is independent and does not have to happen on the
        // UI thread. Therefore, we specify use_arbitrary.
        create_task(feedOp).then([this]  (SyndicationFeed^ feed) -> FeedData^
        {
            return GetFeedData(feed);
        }, task_continuation_context::use_arbitrary())

        // Append the initialized FeedData object to the list
        // that is the data source for the items collection.
        // This all has to happen on the same thread.
        // By using the use_default context, we can append
        // safely to the Vector without taking an explicit lock.
        .then([feedList] (FeedData^ fd)
        {
            feedList->Append(fd);
            OutputDebugString(fd->Title->Data());
        }, task_continuation_context::use_default())

        // The last continuation serves as an error handler. The
        // call to get() will surface any exceptions that were raised
        // at any point in the task chain.
        .then( [this] (task<void> t)
        {
            try
            {
                t.get();
            }
            catch(Platform::InvalidArgumentException^ e)
            {
                //TODO handle error.
                OutputDebugString(e->Message->Data());
            }
        }); //end task chain

    }); //end std::for_each
}
```

Geschachtelte Aufgaben, bei denen es sich um neu erstellte Aufgaben in einer Fortsetzung handelt, erben nicht die Apartmentfähigkeit der ursprünglichen Aufgabe.

## <a name="handing-progress-updates"></a>Behandeln von Statusaktualisierungen
Methoden mit Unterstützung von [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) oder [**IAsyncActionWithProgress**](/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_) aktualisieren regelmäßig den Status eines laufenden Vorgangs, solange dieser nicht beendet ist. Der Status wird dabei unabhängig von dem Konzept von Aufgaben und Fortsetzungen gemeldet. Sie müssen nur den Delegaten für die [**Progress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)-Eigenschaft des Objekts angeben. Eine typische Verwendung von Delegaten ist die Aktualisierung einer Statusleiste auf der Benutzeroberfläche.

## <a name="related-topics"></a>Zugehörige Themen
* [Erstellen von asynchronen Vorgängen in C++/CX für UWP-apps](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)
* [Visual C++-Programmiersprachenreferenz](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Asynchrone Programmierung][Asyncprogramming]
* [Aufgaben Parallelität (Concurrency Runtime)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: ./asynchronous-programming-universal-windows-platform-apps.md "Asyncprogramming"
[concurrencyNamespace]: /cpp/parallel/concrt/reference/concurrency-namespace "Parallelitäts Namespace"
[createTask]: /cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task "CreateTask"
[deleteAsync]: /uwp/api/Windows.Storage.StorageFile "DeleteAsync"
[IAsyncAction]: /uwp/api/Windows.Foundation.IAsyncAction "IAsyncAction"
[IAsyncOperation]: /uwp/api/windows.foundation.iasyncoperation-1 "IAsyncOperation"
[IAsyncInfo]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfo"
[IAsyncInfoCancel]: /uwp/api/Windows.Foundation.IAsyncInfo "Iasyncinfocancel"
[taskCanceled]: /cpp/parallel/concrt/reference/task-canceled-class "Task abgebrochen"
[task-class]: /cpp/parallel/concrt/reference/task-class#get "Task-Klasse"
[taskGet]: /cpp/parallel/concrt/reference/task-class "Taskget"
[taskParallelism]: /cpp/parallel/concrt/task-parallelism-concurrency-runtime "Aufgaben Parallelität"
[taskThen]: /cpp/parallel/concrt/reference/task-class#then "Taskthen"
[useArbitrary]: /cpp/parallel/concrt/reference/task-continuation-context-class "UseArbitrary"