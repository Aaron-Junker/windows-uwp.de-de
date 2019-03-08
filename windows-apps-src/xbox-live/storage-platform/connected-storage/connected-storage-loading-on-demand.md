---
title: Verbundene Speicher laden bei Bedarf
description: Erfahren Sie, wie Speicher verbundene Daten nach Bedarf, anstelle von auf einmal zu laden.
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, verbundenen Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 31c1893f09e6d56682b4e718ee8b905ce72c7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631715"
---
# <a name="connected-storage-loading-on-demand"></a>Verbundene Speicher laden bei Bedarf

`GetSyncOnDemandForUserAsync` können Sie Cloud-gesicherten Daten aus einer verbundenen Speicher "bei Nachfrage" anstatt auf einmal zu laden. Dies kann die Leistung verbessern, über `GetForUserAsync` für Fälle, in dem Datei speichert, besonders groß sind.

## <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>Zum Laden von Daten aus einer verbundenen Speicher nach Bedarf

### <a name="1--call-getsyncondemandforuserasync"></a>1:  Rufen Sie `GetSyncOnDemandForUserAsync`

Dies löst eine partielle Synchronisierung, die eine Liste von Containern und ihren Metadaten aus der Cloud, aber nicht deren Inhalt herunterlädt. Dieser Vorgang erfolgt schnell und unter Umständen Netzwerkverbindung sollte dem Benutzer alle laden Benutzeroberfläche nicht angezeigt.

```cpp
auto op = ConnectedStorageSpace::GetSyncOnDemandForUserAsync(user);
op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
{
    switch (status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        auto storageSpace = operation->GetResults();
        break;
    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        // Present user option: ?Would you like to continue without saving progress??
        if (GetUserInputYesOrNo())
            SetGameState(LoadSaveState::NO_SAVE_MODE);
        else
            SetGameState(LoadSaveState::RETRY_LOAD);
        break;
    }
});
```

```csharp
var users = await Windows.System.User.FindAllAsync();

GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetSyncOnDemandForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Paramaters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
```


### <a name="2--perform-a-container-query-using-getcontainerinfo2async"></a>2:  Führen Sie eine Abfrage mit container `GetContainerInfo2Async`

Dies gibt eine Auflistung von `ContainerInfo2`, die 3 neue Metadatenfelder enthält:

    -   `DisplayName`: Jeder Anzeigename, der Sie geschrieben haben, mithilfe der Überladung von `SubmitUpdatesAsync` , die eine Anzeigezeichenfolge für die Namen als Parameter akzeptiert. Es wird empfohlen, dieses Feld immer verwenden, um einen benutzerfreundlichen Namen zu speichern.
    -   `LastModifiedTime`: Der Zeitpunkt der letzten Änderung dieses Containers. Beachten Sie, dass im Fall von in Konflikt stehende lokale und remote-Zeitstempeln, remote diejenigen verwendet wird.
    -   `NeedsSync`: Ein boolescher gibt an, ob dieser Container Daten enthält, die aus der Cloud heruntergeladen werden muss.

    Verwenden diese zusätzlichen Metadaten, Sie können Anzeigen der wichtiger Informationen zu ihrer Spiele speichert (einschließlich Name, beim letzten Mal verwendet, und gibt an, ob Sie eine auswählen müssen einen Download) ohne einen vollständigen Download aus der Cloud ausführen.

```cpp
auto containerQuery = storageSpace->CreateContainerInfoQuery(nullptr); //return list of containers in ConnectedStorageSpace
auto queryOperation = containerQuery->GetContainerInfo2Async();

queryOperation->Completed = ref new AsyncOperationCompletedHandler<IVectorView<ContainerInfo2>^ >( 
    [=] (IAsyncOperation<IVectorView<ContainerInfo2>^ >^ operation, Windows::Foundation::AsyncStatus status)
    {
        switch (status)
        {
        case Windows::Foundation::AsyncStatus::Completed:
            // get the resulting vector of container info
            auto infoVector = operation->GetResults();
            break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
            // handle error cases
            break;
        }
    });
```

```csharp
GameSaveContainerInfoQuery infoQuery = gameSaveProvider.createContainerInfoQuery();
GameSaveContainerInfoGetResult containerInfoResult = await infoQuery.GetContainerInfoAsync();
var containerInfoList;

if(containerInfoResult.Status == GameSaveErrorStatus.Ok)
{
    containerInfoList = containerInfoResult.value;
}

// Use the containerInfoList to inform further actions or display container data to user. 
```

### <a name="3--trigger-a-sync"></a>3:  Auslösen einer Synchronisierung

Eine verbundene Speicher Synce wird durch das Aufrufen einer der folgenden vorhandenen verbundenen Speicher-API ausgelöst werden:

**C++**

    -   BlobInfoQueryResult::GetBlobInfoAsync
    -   BlobInfoQueryResult::GetItemCountAsync
    -   ConnectedStorageContainer::GetAsync
    -   ConnectedStorageContainer::ReadAsync
    -   ConnectedStorageSpace::DeleteContainerAsync

**C#**

    -   GameSaveBlobInfoQuery.GetBlobInfoAsync
    -   GameSaveBlobInfoQuery.GetItemCountAsync
    -   GameSaveContainer.GetAsync
    -   GameSaveContainer.ReadAsync
    -   GameSaveProvider.DeleteContainerAsync

Dadurch wird den Benutzer die üblichen Sync-Benutzeroberfläche und die Statusleiste angezeigt wird, wie Daten aus ihren ausgewählten Container heruntergeladen werden. Beachten Sie, dass nur Daten aus den ausgewählten Container synchronisiert werden. Daten aus anderen Containern werden nicht heruntergeladen werden.

Beim Aufrufen dieser API im Kontext einer on Demand-Synchronisierung, können diese Vorgänge alle der folgenden neuen Fehlercodes erzeugt:

-   `ConnectedStorageErrorStatus::ContainerSyncFailed`(`GameSaveErrorStatus.ContainerSyncFailed` auf UWP C# API): Dieser Fehler weist darauf hin, dass der Vorgang fehlgeschlagen ist und der Container nicht mit der Cloud synchronisiert werden konnte. Die wahrscheinlichste Ursache ist der Benutzer die netzwerkbedingungen, verursacht die Synchronisierung nicht erfolgreich. Sie sollten versuchen, nachdem sie ihr Netzwerk behoben haben oder sie können einen Container verwenden, sind keine, Synchronisierung. Die Benutzeroberfläche sollte es sich um eine dieser Optionen ermöglichen. Kein Dialogfeld "Wiederholen" ist erforderlich, da sie bereits die System-Benutzeroberflächendialogfeld "Wiederholen" gesehen haben, werden.

-   `ConnectedStorageErrorStatus::ContainerNotInSync`(`GameSaveErrorStatus.ContainerNotInSync` auf UWP C# API): Dieser Fehler weist darauf hin, dass der Titel des Versuch zum Schreiben in einen Container unsynchronisierte. Aufrufen von `ConnectedStorageContainer::SubmitUpdatesAsync`(`GameSaveContainer.SubmitUpdatesAsync` auf UWP C# API) in einem Container mit dem NeedsSync-Flag auf True festgelegt ist nicht zulässig. Sie müssen zuerst einen Container für eine Synchronisierung auslösen, vor dem Schreiben auf die sie lesen. Wenn Sie einen Container ohne Lesen von ihm schreiben, ist es wahrscheinlich der Titel, einen Fehler enthält, in dem Status der User beeinträchtigt werden könnte.

Dieses Verhalten ist anders, wenn ein Benutzer offline spielt. Während offline, es gibt keinen Hinweis darauf, ob der Container synchronisiert werden, daher wird der Benutzer zu einem späteren Zeitpunkt etwaige Konflikte lösen. In diesem Fall weiß das System jedoch, dass der Benutzer muss zu synchronisieren, damit sie nicht kann, erhalten in einem fehlerhaften Zustand mithilfe eines veralteten Containers (obwohl, wenn sie wünschen, können sie neu des Titels starten und Spielen sie offline).

### <a name="4--use-the-rest-of-the-connected-storage-api-as-normal"></a>4:  Verwenden Sie die restlichen die verbundenen Speicher-API wie gewohnt

Verbundene speicherverhaltens bleibt unverändert, wenn bedarfsgesteuert synchronisieren.
