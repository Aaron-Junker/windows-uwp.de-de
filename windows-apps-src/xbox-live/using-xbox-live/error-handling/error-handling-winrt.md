---
title: Fehlerbehandlung für WinRT-API
description: Erfahren Sie, wie Fehler behandelt werden, wenn Sie einen Aufruf des Xbox Live-Dienst mit der WinRT-APIs vornehmen.
ms.assetid: b4f04798-df91-430e-90f0-83b5a48b9ab2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, Fehlerbehandlung
ms.localizationpriority: medium
ms.openlocfilehash: e72dfa0b6f98284c240cf6af2dde02439d694b48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598635"
---
# <a name="winrt-api-error-handling"></a>Fehlerbehandlung für WinRT-API

In der WinRT-API aufgerufen werden kann in C++ / CX C#, oder Javascript, Fehler werden gemeldet, die mithilfe von Ausnahmen, damit Sie Try/Catch-Blöcke verwendet werden müssen, um Fehler zu behandeln.

Beachten Sie, dass die für asynchrone Aufrufe, werden zwei Try/Catch-Blöcke benötigt wie folgt aus:

```cpp
    try
    {
        auto asyncOp = xboxLiveContext->TitleStorageService->UploadBlobAsync(
            blobMetadata,
            blobBuffer,
            TitleStorageETagMatchCondition::NotUsed,
            DEFAULT_UPLOAD_BLOCK_SIZE
            );

        create_task(asyncOp)
        .then([this, ui]( task<TitleStorageBlobMetadata^> t )
        {
            try
            {
                TitleStorageBlobMetadata^ blobMetadata = t.get();

                ui->Log(L"UploadBlobAsync succeeded");
                PrintBlobMetadata(ui, blobMetadata);

                SaveNewETag(blobMetadata->ETag);
            }
            catch (Platform::Exception^ ex)
            {
                // This could happen if there's a network error or the service returns an error
                ui->Log("The async task of UploadBlobAsync failed with", ex->ToString());
            }
        });
    }
    catch (Platform::Exception^ ex)
    {
        // This could happen if there's invalid args sent to the API
        ui->Log("The API call to UploadBlobAsync failed with", ex->ToString());
    }
```

Die XSAPI Async-Methoden haben sich auf Code aus, der synchron ausgeführt wird, wenn eine Methode aufgerufen wird.  Der synchrone Code funktioniert wie die Eingabeargumente überprüfen und starten alle asynchronen Vorgänge oder Aktionen.  Daher kann selbst der Aufruf der asynchronen Methoden zu einer Ausnahme – führen, obwohl dies für normale Szenarien (d. h. Programmierfehler, nicht Netzwerkfehler) sollte nicht.  Bitte sein. dieser Möglichkeit und dieses Programm bewusste defensiv Eingabe überprüfen und Testen Ihren Code gründlich, während der Entwicklung.  Wichtig werden, ob Sie einen Try/Catch rund um den Aufruf selbst platzieren oder eine auf einer höheren Ebene in der Aufrufliste zu platzieren, haben.

## <a name="help-my-game-crashes-when-i-call-xyz-xbox-service-api"></a>Hilfe! Mein Spiel stürzt ab, wenn ich XYZ Xbox-Dienst-API-Aufrufe

Also Ihr Spiel stürzt ab, und sagt die Aufrufliste, er ist ein Xbox-Dienst-API-Aufruf ausführt, aber?

```cpp
msvcr110.dll!Concurrency::details::_ReportUnobservedException() Line 2455    C++
Social110Release.exe!Concurrency::details::_ExceptionHolder::~_ExceptionHolder() Line 915    C++
Social110Release.exe!Concurrency::details::_Task_impl_base::~_Task_impl_base() Line 1488    C++
Social110Release.exe!Concurrency::details::_Task_impl<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::`scalar deleting destructor'(unsigned int)    C++
Social110Release.exe!Concurrency::task<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::_ContinuationTaskHandle<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64,void,<lambda_8571b6148830c0805feee6ba9e76a692>,std::integral_constant<bool,1>,Concurrency::details::_TypeSelectorNoAsync>::`scalar deleting destructor'(unsigned int)    C++
msvcr110.dll!Concurrency::details::_TaskCollection::_NotifyCompletedChoreAndFree(Concurrency::details::_UnrealizedChore * pChore) Line 1171    C++
msvcr110.dll!Concurrency::details::_UnrealizedChore::_UnstructuredChoreWrapper(Concurrency::details::_UnrealizedChore * pChore) Line 373    C++
msvcr110.dll!Concurrency::details::InternalContextBase::Dispatch(Concurrency::DispatchState * pDispatchState) Line 1716    C++
msvcr110.dll!Concurrency::details::FreeThreadProxy::Dispatch() Line 203    C++
msvcr110.dll!Concurrency::details::ThreadProxy::ThreadProxyMain(void * lpParameter) Line 174    C++
ntdll.dll!RtlUserThreadStart(long (void *) * StartAddress, void * Argument) Line 822    C++
```

Diese Aufruflisten sind recht verwirrend.  Parallelität, Parallelität...  Oh ist Microsoft::Xbox::Services::Social::XboxUserProfile Position im Aufrufstapel, daher müssen, die die Ursache sein.  In Wirklichkeit ist dies eine Aufrufliste, die durch einen Aufruf von GetUserProfileAsync für eine ungültige Benutzer-ID für die Xbox generiert  Der Beispielcode, der diese Aufrufliste generiert sieht folgendermaßen aus:

```cpp
    auto pAsyncOp = requester->ProfileService->GetUserProfileAsync("abc123"); //passing invalid Xbox User Id;

    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)
    {
        // Oops, I forgot my exception handling code here.
        // If I don't call resultTask.get() and catch any potential exception it may throw,
        // then PPL will report an unobserved exception.  That unobserved exception will cause your
        // app to crash.
    });
```

Enthält Ihre Aufrufliste Concurrency::_ReportUnobservedException()?  Nehmen Sie einen weiteren Blick auf die oben genannten Aufrufliste an.  Das ist wichtig.  Wenn Ihre Aufrufliste Concurrency:: _ReportUnobservedException() enthält, haben Sie einen Fehler in den spielcode gefunden.

PPL erstellt Aufgaben, die von anderen Aufgaben folgen können.  Im obigen Beispiel create_task() erstellt die Aufgabe, um GetUserProfileAsync() aufrufen und die .then() die nachfolgend aufgeführte Aufgabe.  Diese werden häufig als der Vorgängeraufgabe bezeichnet (zunächst 1) und die Fortsetzung (Sekunden).  Im Beispiel wird die Fortsetzung Fehlerbehandlung fehlt.  Die Runtime beendet die app aus, wenn eine Aufgabe löst eine Ausnahme aus, die Ausnahme wird nicht durch den Task oder eine ihrer Fortsetzungen.

Wenn es sich um Fortsetzungsaufgaben geht, beachten Sie, dass eigentlich zwei verschiedene Arten.  Eine Art, die aufgabenbasierte Fortsetzung, nimmt die vorherige Aufgabe als das Eingabeargument.  Diese Aufgabe wird immer ausgeführt, auch wenn die Vorgängeraufgabe eine Ausnahme auslöst.  Um das Ergebnis der Vorgängeraufgabe abzurufen, müssen Sie .get() für das Argument aufrufen.  Der zweite Wert- Basis, empfängt die Ausgabe der vorherigen Aufgabe direkt – es ist wirklich syntaktisches Bonbon mit Ausnahme einer Sache: wertbasierte Fortsetzungen werden nicht alle ausgeführt, wenn der Vorgänger eine Ausnahme auslöst.

Empfehlung:  Ruft auf Abstürze zu verhindern, verwenden am Ende der fortsetzungskette eine aufgabenbasierte Fortsetzung und setzen Sie alle Concurrency:: task::get() oder Concurrency:: task::wait() in Try/Catch-Blöcken, um Fehler zu behandeln, die aus wiederhergestellt werden können.

Sehen wir uns einige Beispiele:

Aufgabenbasierte Fortsetzung-Beispiel

```cpp
    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)   // Task-based continuation
    {
        try
        {
            XboxUserProfile^ result = resultTask.get();

            // success, do something
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
        }
    });
```

Wertbasierte Fortsetzung-Beispiel

```cpp
    create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
        // if the task didn't complete successfully, you'd better have a task-based
        // continuation at the end of the continuation chain or the app will crash.
    })
    .then( [this] (task<void> previousTask) // Task-based continuation
    {
        try
        {
            // DO NOT IGNORE THIS THINKING IT'S NOT IMPORTANT.

            // call concurrency::task::get and handle any unobserved exception
            // so the application doesn't crash.
            previousTask.get();

            // success, continue
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
            // By handling this exception, you ensure your application will not
            // crash when calling Xbox Service APIs
        }
    });
```

Es gibt eine dritte Lösung – wertbasierten Fortsetzungen vollständig zu verwenden, aber .get() oder .wait() in einem anderen Thread aufrufen und die Ausnahme vorhanden.  Hier ist ein einfaches Beispiel:

```cpp
    auto getProfileTask = create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
    });
    // Note the lack of a task-based continuation with error handling at the end

    // You may call .get() or .wait() on a value-based only chain, but
    // must ensure you surround the call in a try/catch block and handle errors
    try
    {
        getProfileTask.get();     // or getProfileTask.wait();
    }
    catch (Platform::Exception^ ex)
    {
        // concurrency::task::get threw an exception
        // safely handle the error here
        // By handling this exception, you ensure your application will not
        // crash when calling Xbox Service APIs
    }
```

**Wenn Sie nicht PPL verwenden** Wenn AsyncOperationCompletionHandler oder AsyncActionCompletionHandler Sie anstelle von PPL verwenden, haben Sie auch eine Fehlerbehandlung, die bei einer fehlerhaften, Durchführung Abstürze verursachen.  Unten ist ein Beispiel, das zeigt, wie Fehler behandeln

```cpp
    try
    {
        // Example is making a service call with an invalid XboxUserId which will result in an error.
        // The completion handler properly detects the error and does not crash the app.
        requester->ProfileService->GetUserProfileAsync("abc123")->Completed
            = ref new AsyncOperationCompletedHandler<XboxUserProfile^>([=] (IAsyncOperation<XboxUserProfile^>^ operation, Windows::Foundation::AsyncStatus status)
        {
            if( status == Windows::Foundation::AsyncStatus::Completed)
            {
                // Always check the AsyncStatus before calling GetResults().
                // If status is not AsyncStatus::Completed, calls to operation->GetResults()
                // may throw an exception.
                // You can also surround this call in a try/catch block for added safety.

                XboxUserProfile^ result = operation->GetResults();

                // success, do something with the result
            }
            else if( status == Windows::Foundation::AsyncStatus::Error )
            {
                // Failed
            }
        });
    }
    catch ( Platform::COMException^ ex )
    {
        // What is this try/catch block for?
        //
        // Xbox Service APIs do have some code that runs synchronously and errors need
        // to be safely handled.  In this example, if “” was passed instead of “abc123”,
        // then an invalid argument exception would be thrown when calling GetUserProfileAsync
    // See the next section for more a more detailed explanation.
        //
        // Note: this catch block will NOT catch exceptions thrown within the completion handler.
    }
```

**GetNextAsync() und Ausnahmen** verwenden die Paging-APIs?  Alle AchievementResult, LeaderboardResult, InventoryItemResult und TitleStorageBlobMetadataResult Objekte enthalten eine GetNextAsync()-Methode, um die nächste Seite mit Ergebnissen anzufordern.  Es ist ein Sonderfall, wenn keine weiteren Daten verfügbar ist, die Ausnahme wird ausgelöst, wenn GetNextAsync() aufrufen.  Diese Ausnahme wird während der synchronen Ausführung GetNextAsync() ausgelöst.  In diesem Fall löst die Methode GetNextAsync INET_E_DATA_NOT_AVAILABLE (0x800C0007) aus.

Empfehlung: Stellen Sie sicher, dass Sie umschließen GetNextAsync() Methode in einem Try/Catch-Block aufruft und die INET_E_DATA_NOT_AVAILABLE Situation ordnungsgemäß zu behandeln.

```cpp
    try
    {
        // AchievementResult^ LastResult

        // Get next page of achievement results
        if(LastResult != nullptr)
        {
            auto getNextPage = LastResult->GetNextAsync(10);

            // create_task( getNextPage ) ...
        }
    }
    catch (Platform::Exception^ ex)
    {
        if (ex->HResult == INET_E_DATA_NOT_AVAILABLE)
        {
                // we hit the end of the achievements
        }
        else
        {
            // failed for unexpected reason
        }
    }
```
