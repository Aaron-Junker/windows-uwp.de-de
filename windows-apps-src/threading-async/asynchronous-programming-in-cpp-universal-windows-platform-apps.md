---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: Dieser Artikel beschreibt die empfohlene Methode zum Verarbeiten von asynchroner Methoden in Visual C++-komponentenerweiterungen (C++ / CX) mithilfe der Task-Klasse, die in der Concurrency-Namespace in "ppltasks.h" definiert.
title: Asynchrone Programmierung in C++
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, Threads, asynchron, C++
ms.localizationpriority: medium
ms.openlocfilehash: beab78415ab36fc7bc0659af1b3466b2c3601d88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662835"
---
# <a name="asynchronous-programming-in-ccx"></a>Asynchrone Programmierung in C++/CX
> [!NOTE]
> In diesem Thema erhalten Sie Unterstützung bei der Verwaltung Ihrer C++/CX-Anwendung. Es wird jedoch empfohlen, dass Sie [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) für neue Anwendungen nutzen. C++/WinRT ist eine vollständig standardisierte, moderne C++17 Sprachprojektion für Windows-Runtime-(WinRT)-APIs, die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet.

Dieser Artikel beschreibt die empfohlene Methode zum Verarbeiten von asynchroner Methoden in Visual C++-komponentenerweiterungen (C++ / CX) mithilfe der `task` in definierten Klasse die `concurrency` Namespace in "ppltasks.h".

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>Asynchrone UWP-Typen (Universelle Windows-Plattform)
Die Universelle Windows-Plattform (UWP) umfasst ein ausgearbeitetes Modell für den Aufruf asynchroner Methoden und stellt die Typen bereit, die Sie für die Verwendung dieser Methoden benötigen. Wenn Sie mit dem asynchronen Modell der UWP nicht vertraut sind, sollten Sie den Artikel [Asynchrone Programmierung][AsyncProgramming] lesen, bevor Sie mit diesen Erläuterungen fortfahren.

Sie können die asynchronen UWP-APIs zwar direkt in C++ verwenden, aber es ist besser, die [**task class**][task-class] und die zugehörigen Typen und Funktionen zu verwenden. Diese sind im [**concurrency**][concurrencyNamespace] -Namespace enthalten und in `<ppltasks.h>` definiert. **concurrency::task** ist eine Klasse für allgemeine Zwecke. Bei Verwendung des **/ZW**-Compilerschalters, der für UWP-Apps und die entsprechenden Komponenten benötigt wird, werden jedoch die asynchronen Typen der UWP durch die Task-Klasse gekapselt, was Folgendes vereinfacht:

-   Verkettung mehrerer asynchroner und synchroner Vorgänge

-   Behandlung von Ausnahmen in Aufgabenabfolgen

-   Ausführung von Abbrüchen in Aufgabenabfolgen

-   Gewährleistung, dass einzelne Aufgaben im richtigen Threadkontext oder -apartment ausgeführt werden

Dieser Artikel bietet einen allgemeinen Überblick über die Verwendung der **Task**-Klasse mit den asynchronen UWP-APIs. Führen Sie für weitere Dokumentation zu **Aufgabe** und die zugehörigen Methoden, einschließlich [ **erstellen\_Aufgabe**][createTask], finden Sie unter [ Aufgabenparallelität (Concurrency Runtime)][taskParallelism]. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>Verwendung asynchroner Vorgänge mithilfe von Aufgaben
Im folgenden Beispiel wird gezeigt, wie Sie mit der Task-Klasse eine **async**-Methode verwenden, die eine [**IAsyncOperation**][IAsyncOperation]-Schnittstelle zurückgibt und durch deren Vorgang ein Wert erzeugt wird. Die grundlegenden Schritte:

1.  Rufen Sie die `create_task`-Methode auf, und übergeben Sie diese an das **IAsyncOperation^**-Objekt.

2.  Rufen Sie in der Aufgabe die [**task::then**][taskThen]-Memberfunktion auf, und geben Sie einen Lambda-Ausdruck an, der nach Ausführung des asynchronen Vorgangs aufgerufen wird.

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

Die Aufgabe, die von der [**task::then**][taskThen]-Funktion erstellt und zurückgegeben wird, wird als *Fortsetzung* bezeichnet. Das Eingabeargument für den benutzerdefinierten Lambda-Ausdruck ist (in diesem Fall) das Ergebnis der beendeten Aufgabe. Derselbe Wert würde auch mit einem Aufruf von [**IAsyncOperation::GetResults**](https://msdn.microsoft.com/library/windows/apps/br206600) bei direkter Verwendung der **IAsyncOperation**-Schnittstelle abgerufen werden.

Der Aufruf der [**task::then**][taskThen]-Methode wird sofort beendet. Der Delegat wird erst ausgeführt, nachdem die asynchronen Vorgänge erfolgreich beendet wurden. In diesem Beispiel wird die Fortsetzung nicht ausgeführt, wenn der asynchrone Vorgang eine Ausnahme auslöst oder aufgrund einer Abbruchanforderung mit einem Abbruch beendet wird. Weiter unten erfahren Sie, wie Sie Fortsetzungen schreiben, die auch bei einem Abbruch oder Fehler der vorhergehenden Aufgabe ausgeführt werden.

Sie deklarieren die Aufgabenvariable zwar im lokalen Stapel, aber die Lebensdauer wird so verwaltet, dass die Variable erst gelöscht wird, wenn alle zugehörigen Vorgänge abgeschlossen sind und alle Verweise auf die Variable außerhalb des Bereichs liegen – auch dann, wenn der Aufruf der Methode vor Abschluss des Vorgangs beendet wird.

## <a name="creating-a-chain-of-tasks"></a>Erstellen von Aufgabenabfolgen
Bei der asynchronen Programmierung werden in vielen Fällen Abfolgen von Vorgängen definiert, sogenannte *Aufgabenabfolgen*, bei denen eine Fortsetzung immer erst ausgeführt wird, wenn die vorhergehende Aufgabe beendet ist. In manchen Fällen erzeugt die letzte (oder *vorhergehende*) Aufgabe einen Wert, den die Fortsetzung als Eingabe akzeptiert. Mithilfe der [**task::then**][taskThen]-Methode können Sie intuitiv und ganz einfach Aufgabenabfolgen erstellen. Die Methode gibt ein Element vom Typ **task<T>** zurück, wobei **T** der Rückgabetyp der Lambda-Funktion ist. Eine Aufgabenabfolge kann mehrere Fortsetzungen enthalten: `myTask.then(…).then(…).then(…);`

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

-   Mit der ersten Fortsetzung wird das [**IAsyncAction^**][IAsyncAction]-Objekt in ein Element vom Typ **task<void>** konvertiert und das Element vom Typ **task** zurückgegeben.

-   Die zweite Fortsetzung führt keine Fehlerbehandlung durch und verwendet daher **void** als Eingabe (und nicht **task<void>**). Es handelt sich um eine wertbasierte Fortsetzung.

-   Die zweite Fortsetzung wird erst ausgeführt, wenn der [**DeleteAsync**][deleteAsync]-Vorgang beendet ist.

-   Da die zweite Fortsetzung auf einem Wert basiert, wird sie nicht ausgeführt, wenn der mit dem Aufruf [**DeleteAsync**][deleteAsync] gestartete Vorgang eine Ausnahme auslöst.

**Beachten Sie**  erstellen eine Task-Kette ist nur eine der Möglichkeiten zum Verwenden der **Aufgabe** Klasse, um asynchrone Vorgänge zu erstellen. Sie können Vorgänge auch mit den Verknüpfungs- und Auswahloperatoren **&&** und **||** erstellen. Weitere Informationen finden Sie unter [Task-Parallelität (Konkurrenz-Runtime)][taskParallelism].

## <a name="lambda-function-return-types-and-task-return-types"></a>Rückgabetypen für Lambda-Funktionen und Aufgaben
Bei Aufgabenfortsetzungen ist der Rückgabetyp der Lambda-Funktion von einem **task**-Objekt umschlossen. Wenn die Lambda-Funktion den **double**-Typ zurückgibt, hat die Fortsetzungsaufgabe den **task<double>**-Typ. Das Aufgabenobjekt ist jedoch so konzipiert, dass es keine unnötig geschachtelten Rückgabetypen erzeugt. Wenn eine Lambda-Funktion den **IAsyncOperation<SyndicationFeed^>^**-Typ zurückgibt, gibt die Fortsetzung den **task<SyndicationFeed^>**-Typ und nicht **task<task<SyndicationFeed^>>** oder **task<IAsyncOperation<SyndicationFeed^>^>^** zurück. Dieser Vorgang wird als *asynchrones Entpacken* bezeichnet und sorgt dafür, dass der asynchrone Vorgang in der Fortsetzung vor dem Aufruf der nächsten Fortsetzung beendet wird.

Beachten Sie im vorhergehenden Beispiel, dass die Aufgabe den **task<void>**-Typ zurückgibt, obwohl die zugehörige Lambda-Funktion ein [**IAsyncInfo**][IAsyncInfo]-Objekt zurückgegeben hat. Die folgende Tabelle gibt einen Überblick über die Typkonvertierungen zwischen Lambda-Funktionen und den einschließenden Aufgaben:

| | |
|--------------------------------------------------------|---------------------|
| Lambda-Rückgabetyp                                     | `.then`-Rückgabetyp |
| TResult                                                | task<TResult> |
| IAsyncOperation<TResult>^                        | task<TResult> |
| IAsyncOperationWithProgress<TResult, TProgress>^ | task<TResult> |
|IAsyncAction^                                           | task<void>    |
| IAsyncActionWithProgress<TProgress>^             |task<void>     |
| task<TResult>                                    |task<TResult>  |


## <a name="canceling-tasks"></a>Abbrechen von Aufgaben
In vielen Fällen ist es ratsam, Benutzern die Möglichkeit zum Abbrechen eines asynchronen Vorgangs zu geben. Mitunter müssen Sie außerdem einen Vorgang programmgesteuert von außerhalb der Aufgabenabfolge abbrechen. Obwohl jede \* **Async** Typ verfügt über eine [ **Abbrechen** ] [ IAsyncInfoCancel] -Methode, die es erbt [  **IAsyncInfo**][IAsyncInfo], es ist ganz einfach, sie außerhalb von Methoden verfügbar zu machen. Die bevorzugte Methode zur Unterstützung des Abbruchs in einer aufgabenkette ist die Verwendung einer [ **Abbruch\_token\_Quelle** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh749985.aspx) zum Erstellen einer [ **Abbruch \_token**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749975.aspx), und klicken Sie dann das Token an den Konstruktor des ersten Tasks übergeben. Wenn eine asynchrone Aufgabe mit einem Abbruchtoken erstellt wird und [ **Abbruch\_token\_source::cancel** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh750076.aspx) aufgerufen wird, die Aufgabe automatisch Aufrufe  **Abbrechen** auf die **IAsync\***  Vorgang aus und übergibt den Abbruch anzufordern, nach dessen fortsetzungskette unten. Im folgenden Pseudocode wird dieses grundlegende Konzept dargestellt.

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

Wenn eine Aufgabe abgebrochen wird, eine [ **Aufgabe\_abgebrochen** ] [ taskCanceled] Ausnahme nach unten der aufgabenkette übermittelt wird. Wertbasierte Fortsetzungen werden nicht ausgeführt. Aufgabenbasierte Fortsetzungen dagegen lösen beim Aufruf von [**task::get**][taskGet] die Ausnahme aus. Wenn Sie eine fehlerbehandlungsfortsetzung haben, stellen Sie sicher, dass es entdeckt die **Aufgabe\_abgebrochen** Ausnahme explizit. (Diese Ausnahme wird nicht von [**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755825.aspx) abgeleitet.)

Abbruchvorgänge sind kooperativ. Wenn eine Fortsetzung neben dem Aufruf einer UWP-Methode zeitintensive Vorgänge ausführt, ist es wichtig, dass Sie den Status des Abbruchtokens regelmäßig überprüfen und die Ausführung bei einem Abbruch beenden. Nachdem Sie alle Ressourcen, die in der Fortsetzung zugeordnet wurden bereinigen, rufen [ **Abbrechen\_aktuelle\_Aufgabe** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh749945.aspx) um diese Aufgabe abbrechen, und geben den Abbruch zu einem wertbasierte Fortsetzungen, die folgen. Ein weiteres Beispiel: Sie können Aufgabenabfolgen erstellen, die das Ergebnis eines [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/BR207871)-Vorgangs darstellen. Wählt der Benutzer die Schaltfläche **Abbrechen**, wird die [**IAsyncInfo::Cancel**][IAsyncInfoCancel]-Methode nicht aufgerufen. Stattdessen wird der Vorgang erfolgreich ausgeführt, allerdings mit der Rückgabe **nullptr**. Die Fortsetzung kann testen, die Eingabeparameter und den Aufruf **Abbrechen\_aktuelle\_Aufgabe** ist die Eingabe **"nullptr"**.

Weitere Informationen finden Sie unter [Abbruch in der PPL](https://msdn.microsoft.com/library/windows/apps/xaml/dd984117.aspx).

## <a name="handling-errors-in-a-task-chain"></a>Behandeln von Fehlern in Aufgabenabfolgen
Wenn eine Fortsetzung auch dann ausgeführt werden soll, wenn die vorhergehende Aufgabe abgebrochen wurde oder eine Ausnahme ausgelöst hat, müssen Sie die Fortsetzung als aufgabenbasierte Fortsetzung anlegen. Geben Sie dafür die Eingabe für die entsprechende Lambda-Funktion als **task<TResult>** oder **task<void>** an, wenn die Lambda-Funktion der vorhergehenden Aufgabe den [**IAsyncAction^**][IAsyncAction]-Typ zurückgibt.

Zur Behandlung von Fehlern und Abbrüchen in Aufgabenabfolgen müssen jedoch nicht alle Fortsetzungen aufgabenbasiert sein, und Sie müssen nicht alle Vorgänge einschließen, die in einem `try…catch`-Block ausgelöst werden können. Stattdessen können Sie am Ende der Abfolge eine aufgabenbasierte Fortsetzung hinzufügen und alle Fehler dort behandeln. Jede Ausnahme – dazu gehören eine [ **Aufgabe\_abgebrochen** ] [ taskCanceled] Ausnahme – der aufgabenkette weitergegeben wird, und alle wertbasierten Fortsetzungen zu umgehen, damit Sie können in der Fehlerbehandlung aufgabenbasierte Fortsetzung behandelt werden. Das Beispiel oben kann daher mit einer aufgabenbasierten Fortsetzung für die Fehlerbehandlung umgeschrieben werden:

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

Bei aufgabenbasierten Fortsetzungen rufen wir die [**task::get**][taskGet]-Memberfunktion auf, um die Aufgabenergebnisse abzurufen. Dabei muss weiterhin **task::get** aufgerufen werden. Dies gilt auch bei ergebnislosen [**IAsyncAction**][IAsyncAction]-Vorgängen, da **task::get** auch alle Ausnahmen abruft, die an die Aufgabe weitergegeben wurden. Wenn die Eingabeaufgabe eine Ausnahme speichert, wird diese mit dem Aufruf von **task::get** ausgelöst. Wenn Sie nicht aufrufen **Task:: Get**, oder verwenden Sie nicht am Ende der Kette eine aufgabenbasierte Fortsetzung, oder fangen Sie nicht den Typ der Ausnahme, die ausgelöst wurde, ab ein **nicht überwachte\_Aufgabe\_-Ausnahme** wird ausgelöst, wenn alle Verweise auf die Aufgabe gelöscht wurden.

Sie sollten nur Ausnahmen abfangen, die Sie auch behandeln können. Wenn die App einen Fehler ausgibt, den Sie nicht beheben können, ist es besser, die App abstürzen zu lassen, als sie mit einem unbekannten Status weiter auszuführen. Darüber hinaus in der Regel versuchen Sie nicht zum Abfangen der **nicht überwachte\_Aufgabe\_Ausnahme** selbst. Diese Ausnahme ist in erster Linie für Diagnosezwecke gedacht. Wenn **nicht überwachte\_Aufgabe\_Ausnahme** wird ausgelöst, in der Regel wird einen Fehler im Code. Die Ursache dafür ist entweder eine Ausnahme, die behandelt werden muss, oder eine nicht behebbare Ausnahme, die durch einen anderen Fehler im Code hervorgerufen wird.

## <a name="managing-the-thread-context"></a>Verwalten des Threadkontexts
Die Benutzeroberfläche von UWP-Apps wird in einem Singlethread-Apartment (STA) ausgeführt. Aufgaben, deren Lambda-Ausdruck [**IAsyncAction**][IAsyncAction] oder [**IAsyncOperation**][IAsyncOperation] zurückgibt, sind apartmentfähig. Wird die Aufgabe im STA erstellt, werden standardmäßig auch alle zugehörigen Fortsetzungen darin ausgeführt, sofern Sie nichts anderes angeben. Anders gesagt: Die gesamte Aufgabenabfolge erbt die Apartmentfähigkeit von der übergeordneten Aufgabe. Dieses Verhalten vereinfacht die Interaktion mit Benutzeroberflächen-Steuerelementen, auf die ausschließlich über das STA Zugriff besteht.

Beispielsweise können Sie in einer UWP-App in der Memberfunktion jeder Klasse, die eine XAML-Seite darstellt, ein [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) -Steuerelement innerhalb einer [**task::then**][taskThen] -Methode auffüllen, ohne dafür das [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211)-Objekt zu verwenden.

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

Gibt die Aufgabe nicht [**IAsyncAction**][IAsyncAction] oder [**IAsyncOperation**][IAsyncOperation] zurück, ist sie nicht apartmentfähig, und ihre Fortsetzungen werden standardmäßig im ersten verfügbaren Hintergrundthread ausgeführt.

Sie können den Kontext des Threads Standardwert für beide Arten von Tasks überschreiben, indem Sie mithilfe der Überladung von [ **Task:: Then** ] [ taskThen] , akzeptiert eine [ **Aufgabe \_Fortsetzung\_Kontext**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749968.aspx). In manchen Fällen ist es zum Beispiel vorteilhaft, die Fortsetzung einer apartmentfähigen Aufgabe für einen Hintergrundthread zu planen. Sie können in diesem Fall übergeben [ **Aufgabe\_Fortsetzung\_context::use\_beliebige** ] [ useArbitrary] zum Planen der Arbeit von der Aufgabe auf der nächsten verfügbaren Thread in einem Multithread-Apartment. Dadurch wird die Leistung der Fortsetzung verbessert, da die entsprechenden Vorgänge nicht mit anderen Vorgängen im UI-Thread synchronisiert sein müssen.

Im folgende Beispiel wird veranschaulicht, wann ist nützlich, geben Sie die [ **Aufgabe\_Fortsetzung\_context::use\_beliebige** ] [ useArbitrary] Option und auch zeigt, wie der standardmäßige Fortsetzungskontext für die Synchronisierung gleichzeitige Vorgänge auf nicht threadsichere Auflistungen. In diesem Codebeispiel wird eine Schleife für eine Liste mit URLs für RSS-Feeds ausgeführt, und für jede URL wird zum Abrufen der Feeddaten ein asynchroner Vorgang gestartet. Die Reihenfolge, in der die Feeds abgerufen werden, können Sie nicht steuern, was jedoch auch nicht wichtig ist. Mit dem Ende jedes [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR210642)-Vorgangs akzeptiert die erste Fortsetzung das [**SyndicationFeed^**](https://msdn.microsoft.com/library/windows/apps/BR243485)-Objekt und initialisiert damit ein App-spezifisches `FeedData^`-Objekt. Da jede dieser Operationen unabhängig von den anderen ist, es kann möglicherweise beschleunigt werden durch Angabe der **Aufgabe\_Fortsetzung\_context::use\_beliebige** Fortsetzungskontext . Nach dem Initialisieren jedes `FeedData`-Objekts müssen wir dieses jedoch einem [**Vector**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx) hinzufügen – und das ist keine threadsichere Auflistung. Aus diesem Grund erstellen wir eine Fortsetzung und geben Sie [ **Aufgabe\_Fortsetzung\_context::use\_aktuelle** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) um sicherzustellen, dass alle Aufrufe von [ **Append** ](https://msdn.microsoft.com/library/windows/apps/BR206632) in demselben Kontext Application Single-Threaded Apartment (ASTA) auftreten. Da [ **Aufgabe\_Fortsetzung\_context::use\_Standard** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) Standardkontext ist, müssen wir nicht explizit anzugeben, aber dies erfolgt hier zum sake der Klarheit.

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
Methoden mit Unterstützung von [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx) oder [**IAsyncActionWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx) aktualisieren regelmäßig den Status eines laufenden Vorgangs, solange dieser nicht beendet ist. Der Status wird dabei unabhängig von dem Konzept von Aufgaben und Fortsetzungen gemeldet. Sie müssen nur den Delegaten für die [**Progress**](https://msdn.microsoft.com/library/windows/apps/br206594)-Eigenschaft des Objekts angeben. Eine typische Verwendung von Delegaten ist die Aktualisierung einer Statusleiste auf der Benutzeroberfläche.

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen von asynchronen Vorgängen in C++ / CX für UWP-apps](https://msdn.microsoft.com/library/hh750082)
* [Sprachreferenz zu Visual C++](https://msdn.microsoft.com/library/windows/apps/hh699871.aspx)
* [Asynchrone Programmierung][AsyncProgramming]
* [Aufgabenparallelität (Concurrency Runtime)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: <https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps> "AsyncProgramming"
[concurrencyNamespace]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace> "Concurrency-Namespace"
[createTask]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task> "CreateTask"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199> "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx> "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598> "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587> "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel> "IAsyncInfoCancel"
[taskCanceled]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-canceled-class> "TaskCancelled"
[task-class]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#get> "Task-Klasse"
[taskGet]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750017.aspx> "TaskGet"
[taskParallelism]: <https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime> "Aufgabenparallelität"
[taskThen]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#then> "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750036.aspx> "UseArbitrary"
