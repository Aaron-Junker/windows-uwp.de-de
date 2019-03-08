---
title: Asynchrone C-API-Aufrufmuster
description: Erfahren Sie, die asynchrone Muster für die XSAPI-C-API Aufrufen C-API
ms.date: 06/10/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Developer-Programm,
ms.localizationpriority: medium
ms.openlocfilehash: edc6248a363b844d94c8fa03ab7ce071cc941908
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608005"
---
# <a name="calling-pattern-for-xsapi-flat-c-layer-async-calls"></a>Aufruf-Muster für XSAPI flache C Ebene asynchrone Aufrufe

Ein **asynchrone API** ist eine API, die schnell zurückgegeben, aber beginnt ein **asynchrone Aufgabe** und das Ergebnis wird zurückgegeben, nachdem die Aufgabe abgeschlossen ist.

Traditionell Spiele haben wenig die kontrollieren, über welchem Thread die Ausführung der **asynchrone Aufgabe** und welchem Thread die Ergebnisse zurückgibt, bei Verwendung eine **Beendigungsrückruf**.  Einige Spiele sind so konzipiert, dass ein Abschnitt des Heaps nur von einem einzelnen Thread, dass die Notwendigkeit für die Threadsynchronisierung zu vermeiden berührt wird. Wenn die **Beendigungsrückruf** wird nicht das Spiel von einem anderen Thread aufgerufen-Steuerelemente, Aktualisieren mit dem Ergebnis des gemeinsam genutzten Zustands ein **asynchrone Aufgabe** Threadsynchronisierung benötigen.

Die XSAPI-C-API verfügbar macht, eine neue asynchrone C-API, die Entwickler direkten Threadsteuerung, beim Bereitstellen bietet einer **asynchrone API** aufrufen, z. B. **XblSocialGetSocialRelationshipsAsync()**,  **XblProfileGetUserProfileAsync()** und **XblAchievementsGetAchievementsForTitleIdAsync()**.

Es folgt ein einfaches Beispiel Aufrufen der **XblProfileGetUserProfileAsync** API.

```cpp
AsyncBlock* asyncBlock = new AsyncBlock {};
asyncBlock->queue = asyncQueue;
asyncBlock->context = customDataForCallback;
asyncBlock->callback = [](AsyncBlock* asyncBlock)
{
    XblUserProfile profile;
    if( SUCCEEDED( XblProfileGetUserProfileResult(asyncBlock, &profile) ) )
    {
        printf("Profile retrieved successfully\r\n");
    }
    delete asyncBlock;
};
XblProfileGetUserProfileAsync(asyncBlock, xboxLiveContext, xuid);
```

Um dieses Aufrufmuster zu verstehen, müssen Sie verstehen, wie Sie mit der **AsyncBlock** und **AsyncQueue**.

* Die **AsyncBlock** enthält alle Informationen zu den **asynchrone Aufgabe** und **Beendigungsrückruf**.

* Die **AsyncQueue** können Sie bestimmen, welcher Thread führt die **asynchrone Aufgabe** und welchem Thread Ruft die AsyncBlock des **Beendigungsrückruf**.

## <a name="the-asyncblock"></a>Die **AsyncBlock**

Werfen wir einen Blick auf die **AsyncBlock** im Detail. Es ist eine Struktur, die wie folgt definiert:

```cpp
typedef struct AsyncBlock
{
    AsyncCompletionRoutine* callback;
    void* context;
    async_queue_handle_t queue;
} AsyncBlock;
```

Die **AsyncBlock** enthält:

* *Rückruf* – eine optionale Rückruffunktion, die aufgerufen wird, nachdem der asynchrone Vorgang erfolgt ist.  Wenn Sie einen Rückruf angeben, können Sie warten, für die **AsyncBlock** mit **GetAsyncStatus** und klicken Sie dann die Ergebnisse abzurufen.
* *Kontext* -ermöglicht es Ihnen, Daten an die Rückruffunktion übergeben.
* *Warteschlange* – eine Async_queue_handle_t wird ein Handle Festlegen einer **AsyncQueue**. Wenn dies nicht festgelegt ist, wird eine Standard-Warteschlange verwendet werden.

Erstellen Sie eine neue AsyncBlock im Heap für jede asynchrone API, die Sie aufrufen.  Die AsyncBlock muss aktiv ist, bis der Rückruf bei Abschluss der AsyncBlock wird aufgerufen, und klicken Sie dann es gelöscht werden kann.

> [!IMPORTANT]
> Ein **AsyncBlock** muss bleiben im Arbeitsspeicher, bis die **asynchrone Aufgabe** abgeschlossen ist. Wenn sie dynamisch zugewiesen wird, kann gelöscht werden innerhalb der AsyncBlock des **Beendigungsrückruf**.

### <a name="waiting-for-asynchronous-task"></a>Warten auf **asynchrone Aufgabe**

Sie können feststellen, ein **asynchrone Aufgabe** ist, führen eine Reihe von Möglichkeiten:

* die AsyncBlocks **Beendigungsrückruf** aufgerufen wird
* Rufen Sie **GetAsyncStatus** mit "true" wartet, bis der Vorgang abgeschlossen ist.
* Legen Sie eine WaitEvent in die **AsyncBlock** und warten Sie, bis das Ereignis signalisiert wird,

Mit **GetAsyncStatus** und WaitEvent, die **asynchrone Aufgabe** gilt als abgeschlossen, nach der AsyncBlock des **Beendigungsrückruf** führt jedoch die AsyncBlocks **Beendigungsrückruf** ist optional.

Sobald die **asynchrone Aufgabe** ist abgeschlossen ist, erhalten Sie die Ergebnisse.

### <a name="getting-the-result-of-the-asynchronous-task"></a>Das Ergebnis der erste der **asynchrone Aufgabe**

Zum Abrufen des Ergebnisses wird die meisten **asynchrone API** Funktionen verfügen über eine entsprechende \[Name der Funktion\]führen Sie die Funktion, um das Ergebnis des asynchronen Aufrufs zu erhalten. In unserem Beispielcode **XblProfileGetUserProfileAsync** verfügt über eine entsprechende **XblProfileGetUserProfileResult** Funktion. Sie können diese Funktion verwenden, um das Ergebnis der Funktion zurückgegeben, und entsprechend agiert.  Finden Sie in der Dokumentation der einzelnen **asynchrone API** -Funktion für ausführliche Informationen zum Abrufen der Ergebnisse.

## <a name="the-asyncqueue"></a>Die **AsyncQueue**

Die **AsyncQueue** können Sie bestimmen, welcher Thread führt die **asynchrone Aufgabe** und welchem Thread Ruft die AsyncBlock des **Beendigungsrückruf**.

Sie können steuern, welcher Thread diesen Vorgang durch Festlegen von führt eine *Dispatch-Modus*. Es gibt drei Dispatch-Modi zur Verfügung:

* *Manuelle* – die manuelle Warteschlange werden nicht automatisch verteilt.  Es ist Aufgabe des Entwicklers, die sie für jeden Thread verteilen gewünschten. Dies kann verwendet werden, die Arbeit oder der Rückruffunktion neben einem asynchronen Aufruf an einen bestimmten Thread zuweisen.  Dies wird im folgenden ausführlicher erläutert.

* *Threadpool* – sendet, die Verwendung eines Threadpools.  Der Threadpool ruft die Aufrufe im parallelen, erstellen einen Aufruf aus der Warteschlange wiederum führen, wie der Threadpool-Threads verfügbar sind.  Dies ist die Vertrauenswürdigkeit verwenden, sondern bietet die geringste Menge an steuern, in dem Thread verwendet wird.

* *Feste Thread* – sendet, die mithilfe von QueueUserAPC auf den Thread, der die Async-Warteschlange erstellt. Bei einer Benutzermodus-APC in der Warteschlange befindet, wird der Thread nicht umgeleitet, die APC-Funktion aufrufen, es sei denn, es in einem alertable Zustand befindet. Ein Thread wechselt in einen alertable Zustand mithilfe von **SleepEx**, **SignalObjectAndWait**, **WaitForSingleObjectEx**, **WaitForMultipleObjectsEx**, oder **MsgWaitForMultipleObjectsEx** zum Ausführen eines Wartevorgangs alertable

Zum Erstellen eines neuen **AsyncQueue** aufrufen, müssen Sie **CreateAsyncQueue**.

```cpp
STDAPI CreateAsyncQueue(
    _In_ AsyncQueueDispatchMode workDispatchMode,
    _In_ AsyncQueueDispatchMode completionDispatchMode,
    _Out_ async_queue_handle_t* queue);
```

AsyncQueueDispatchMode enthält, in dem die drei Dispatch-Modi, die zuvor eingeführt:

```cpp
typedef enum AsyncQueueDispatchMode
{
    /// <summary>
    /// Async callbacks are invoked manually by DispatchAsyncQueue
    /// </summary>
    AsyncQueueDispatchMode_Manual,

    /// <summary>
    /// Async callbacks are queued to the thread that created the queue
    /// and will be processed when the thread is alertable.
    /// </summary>
    AsyncQueueDispatchMode_FixedThread,

    /// <summary>
    /// Async callbacks are queued to the system thread pool and will
    /// be processed in order by the threadpool.
    /// </summary>
    AsyncQueueDispatchMode_ThreadPool

} AsyncQueueDispatchMode;
```

**WorkDispatchMode** bestimmt die Dispatch-Modus für den Thread, der die asynchrone Arbeit, behandelt zwar **CompletionDispatchMode** bestimmt die Dispatch-Modus für den Thread, der den Abschluss der asynchronen behandelt. der Vorgang.

Sie können auch aufrufen **CreateSharedAsyncQueue** zum Erstellen einer **AsyncQueue** mit dem gleichen Warteschlangentyp durch eine ID für die Warteschlange angeben.

```cpp
STDAPI CreateSharedAsyncQueue(
    _In_ uint32_t id,
    _In_ AsyncQueueDispatchMode workerMode,
    _In_ AsyncQueueDispatchMode completionMode,
    _Out_ async_queue_handle_t* aQueue);
```

> [!NOTE]
> Wenn bereits eine Warteschlange mit dieser ID und Dispatch-Modus vorhanden ist, wird darauf verwiesen werden.  Andernfalls wird eine neue Warteschlange erstellt werden.

Nach dem Erstellen Ihrer **AsyncQueue** einfach hinzufügen, damit die **AsyncBlock** zum threading für Ihre Arbeit und den Abschluss Funktionen steuern.
Wenn Sie fertig sind mit den **AsyncQueue**, normalerweise Wenn das Spiel beendet wird, Sie können es schließen mit **CloseAsyncQueue**:

```cpp
STDAPI_(void) CloseAsyncQueue(
    _In_ async_queue_handle_t aQueue);
```

### <a name="manually-dispatching-an-asyncqueue"></a>Verteilen manuell eine **AsyncQueue**

Wenn Sie für die manuelle Warteschlange Dispatch-Modus verwendet ein **AsyncQueue** Geschäfts-, Schul- oder Abschluss-Warteschlange, müssen Sie manuell verteilen.
Angenommen, die eine **AsyncQueue** wurde erstellt, in dem sowohl für die Warteschlange als auch für die Warteschlange für die Vervollständigung manuell verteilt festgelegt sind wie folgt:

```cpp
CreateAsyncQueue(
    AsyncQueueDispatchMode_Manual,
    AsyncQueueDispatchMode_Manual,
    &queue);
```

Um die Arbeit zu senden, der zugewiesen wurde **AsyncQueueDispatchMode_Manual** müssen Sie sie verteilen die **DispatchAsyncQueue** Funktion.

```cpp
STDAPI_(bool) DispatchAsyncQueue(
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type,
    _In_ uint32_t timeoutInMs);
```

* *Warteschlange* – welche Warteschlange zum Verteilen von Arbeit auf
* *Typ* – eine Instanz der **AsyncQueueCallbackType** Enumeration
* *TimeoutInMs* -eine uint32_t für das Timeout in Millisekunden.

Es gibt zwei Rückruf Arten von definiert die **AsyncQueueCallbackType** Enumeration:

```cpp
typedef enum AsyncQueueCallbackType
{
    /// <summary>
    /// Async work callbacks
    /// </summary>
    AsyncQueueCallbackType_Work,

    /// <summary>
    /// Completion callbacks after work is done
    /// </summary>
    AsyncQueueCallbackType_Completion
} AsyncQueueCallbackType;
```

### <a name="when-to-call-dispatchasyncqueue"></a>Wann aufzurufen **DispatchAsyncQueue**

Um zu überprüfen, wenn die Warteschlange ein neues empfangen hat Element Sie erreichen **AddAsyncQueueCallbackSubmitted** , legen Sie einen Ereignishandler einem Ereignis können Ihren Code, wissen, dass entweder Geschäfts-, Schul- oder vervollständigungen verteilt werden können.

```cpp
STDAPI AddAsyncQueueCallbackSubmitted(
    _In_ async_queue_handle_t queue,
    _In_opt_ void* context,
    _In_ AsyncQueueCallbackSubmitted* callback,
    _Out_ uint32_t* token);
```

**AddAsyncQueueCallbackSubmitted** enthält die folgenden Parameter:

* *Warteschlange* -der asynchrone Warteschlange übermitteln Sie den Rückruf für.
* *Kontext* -ein Zeiger auf Daten, die an den Submit-Rückruf übergeben werden sollen.
* *Rückruf* -Funktion, die aufgerufen wird, wenn Sie ein neuer Rückruf in die Warteschlange gesendet wird.
* *Token* -ein Token, die in einem späteren Aufruf verwendet werden, **RemoveAsynceCallbackSubmitted** um den Rückruf zu entfernen.

Hier ist z. B. ein Aufruf von **AddAsyncQueueCallbackSubmitted**:

`AddAsyncQueueCallbackSubmitted(queue, nullptr, HandleAsyncQueueCallback, &m_callbackToken);`

Das entsprechende **AsyncQueueCallbackSubmitted** Rückruf kann wie folgt implementiert werden:

```cpp
void CALLBACK HandleAsyncQueueCallback(
    _In_ void* context,
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type)
{
    switch (type)
    {
    case AsyncQueueCallbackType::AsyncQueueCallbackType_Work:
        ReleaseSemaphore(g_workReadyHandle, 1, nullptr);
        break;
    }
}
```

Und dann in einem Hintergrundthread können Sie Lauschen für diese Semaphor reaktiviert, und rufen Sie **DispatchAsyncQueue**.

```cpp
DWORD WINAPI BackgroundWorkThreadProc(LPVOID lpParam)
{
    HANDLE hEvents[2] =
    {
        g_workReadyHandle.get(),
        g_stopRequestedHandle.get()
    };

    async_queue_handle_t queue = static_cast<async_queue_handle_t>(lpParam);

    bool stop = false;
    while (!stop)
    {
        DWORD dwResult = WaitForMultipleObjectsEx(2, hEvents, false, INFINITE, false);
        switch (dwResult)
        {
        case WAIT_OBJECT_0: 
            // Background work is ready to be dispatched
            DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);
            break;

        case WAIT_OBJECT_0 + 1:
        default:
            stop = true;
            break;
        }
    }

    CloseAsyncQueue(queue);
    return 0;
}
```

Es empfiehlt sich, verwenden mit Win32-Semaphorobjekt implementieren.  Wenn stattdessen Sie mithilfe eines Win32-Event-Objekts implementieren, um sicherzustellen, dass Sie müssen verpassen Sie keine Ereignisse mit Code wie z.B.:

```cpp
    case WAIT_OBJECT_0: 
        // Background work is ready to be dispatched
        DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);        
        
        if (!IsAsyncQueueEmpty(queue, AsyncQueueCallbackType_Work))
        {
            // If there's more pending work, then set the event to process them
            SetEvent(g_workReadyHandle.get());
        }
        break;
```


Sehen Sie ein Beispiel für die bewährten Methoden für Async-Integration auf [soziale AsyncIntegration.cpp der C#-Beispiel](https://github.com/Microsoft/xbox-live-api/blob/master/InProgressSamples/Social/Xbox/C/AsyncIntegration.cpp)

