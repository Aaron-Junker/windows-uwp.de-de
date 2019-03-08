---
title: Verwenden Sie zum Löschen von Daten verbundenen Speicher
description: Erfahren Sie, wie verbundene Speicher verwenden, um BLOBs und Container löschen.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, verbundenen Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 756de46d05cdbf64d85491b4e8c6f783122f2356
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601315"
---
# <a name="use-connected-storage-to-delete-data"></a>Verwenden Sie zum Löschen von Daten verbundenen Speicher

Daten-Blobs werden asynchron gelöscht, durch das Erstellen einer `ConnectedStorageContainer` in eine `ConnectedStorageSpace` für einen Benutzer und dem Aufrufen der `SubmitUpdatesAsync` Methode für den Container, der eine Liste von Zeichenfolgen, die die benannten Blobs für den Parameter BlobsToDelete gelöscht werden soll.

Datencontainer werden asynchron gelöscht, durch das Erstellen einer `ConnectedStorageContainer` und das Aufrufen der `DeleteContainerAsync` Methode.

## <a name="to-delete-blob-data-from-connected-storage"></a>So löschen Sie die Blob-Daten aus dem Speicher verbunden

1.  Abrufen einer `ConnectedStorageSpace` Objekt für den Benutzer durch Aufrufen von `GetForUserAsync`.

    Im Beispiel xdk-Version der zurückgegebenen `ConnectedStorageSpace` Objekt hinzugefügt wird zu einer Zuordnung zum Aktivieren der einfachen Verwaltung von `ConnectedStorageSpace` Objekte für mehrere Benutzer.

2.  Erstellen Sie eine `ConnectedStorageContainer` Objekt durch Aufrufen von `CreateContainer` auf die `ConnectedStorageSpace` Objekt.
3.  Rufen Sie `SubmitUpdatesAsync` auf die `ConnectedStorageContainer` Objekt.

## <a name="to-delete-a-container-from-connected-storage"></a>Zum Löschen eines Containers aus dem Speicher verbunden

1.  Abrufen einer `ConnectedStorageSpace` Objekt für den Benutzer durch Aufrufen von `GetForUserAsync`.

    Im Beispiel xdk-Version der zurückgegebenen `ConnectedStorageSpace` Objekt hinzugefügt wird zu einer Zuordnung zum Aktivieren der einfachen Verwaltung von `ConnectedStorageSpace` Objekte für mehrere Benutzer.

2.  Rufen Sie die `DeleteContainerAsync` Methode der ConnectedStorageSpace Methoden.

## <a name="c-xdk-sample"></a>Beispiel für C++ xdk-Version
```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() { return true; };

User^ gCurrentUser;
IBuffer^ WrapRawBuffer(void* ptr, size_t size);

// Acquire a Connected Storage space for a user. A Connected Storage space is required to manipulate Connected Storage Data.
void PrepareConnectedStorage(User^ user)
{
  auto op = ConnectedStorageSpace::GetForUserAsync(user);
  op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
    {
      switch(status)
      {
        case Windows::Foundation::AsyncStatus::Completed:
          gConnectedStorageSpaceForUsers->Insert(user->Id, operation->GetResults());
          break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
          // Present user option: ?Would you like to continue without saving progress??
          if( GetUserInputYesOrNo() )
            //If the users opts yes, continue in offline mode
          else
            //If the users opts no, retry.
          break;
      }
    });
}

// Delete data from a fixed container/blob name into an IBuffer
void DeleteBlob(User^ user)
{

    //Create a list of blob names to delete
    std::vector<Platform::String^> blobsToDelete;

    //Add the blob name to your Generic Iterable
    blobsToDelete.push_back(L"someBlobName");

    auto blobNames = ref new Platform::Collections::VectorView<Platform::String^>(blobsToDelete);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Delete is occurring asynchronously at this point.

    auto op = container->SubmitUpdatesAsync(nullptr, blobNames);

    op->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });
}

void DeleteContainer(User^ user)
{
    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);

    auto deleteOperation = storageSpace->DeleteContainerAsync("Saves/Checkpoint");

    deleteOperation->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });

}
```

Finden Sie, dass der xdk-Version verbunden Storage-APIs in der xdk-Version CHM-Datei unter dem Pfad dokumentiert: **Xbox eine xdk-Version >>-API-Referenz >> Plattform-API-Referenz >> System-API-Referenz >> Windows.Xbox.Storage**.
Darüber hinaus sind die APIs xdk-Version auf dokumentiert die [developer.microsoft.com Site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
Der Link zum xdk-Version-APIs benötigen Sie ein Microsoft-Account(MSA), die für Xbox-Entwickler Kit(XDK) Zugriff aktiviert wurde.
Windows.Xbox.Storage ist der Name des Namespaces verbundenen Speicher für Xbox One-Konsolen.


## <a name="c-uwp-sample"></a>C#UWP-Beispiel

Während andere APIs xdk-Version-Spiele und UWP-apps verwenden können, ist die xdk-Version-API sehr genau die UWP-API-nachgebildet. Zum Löschen von Daten müssen Sie weiterhin die gleichen grundlegenden Schritte befolgen, und notieren Sie sich einige Namensänderungen Namespace- und Klassennamen gleichzeitig. Anstatt den Namespace `Windows::Xbox::Storage` verwenden Sie `Windows.Gaming.XboxLive.Storage`. Die Klasse `ConnectedStorageSpace`, entspricht der `GameSaveProvider`. Die Klasse `ConnectedStorageContainer` entspricht `GameSaveContainer`. Genauere Beschreibung dieser Änderungen in den verbunden Speicherabschnitt [Portieren von Xbox Live-Code aus xdk-Version für die UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 23;
const string c_saveBlobName = "Jersey";
const string c_saveContainerDisplayName = "GameSave";
const string c_saveContainerName = "GameSaveContainer";
GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetForUserAsync(users[0], context.AppConfig.ServiceConfigurationId);
//Parameters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
else
{
    return;
    //throw new Exception("Game Save Provider Initialization failed");
}

//Now you have a GameSaveProvider (formerly ConnectedStorageSpace)
//Next you need to call CreateContainer to get a GameSaveContainer (formerly ConnectedStorageContainer)

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName); // this will create a new named game save container with the name = to the input name
//Parameter
//string name (name of container to be created) 

//DELETE A BLOB
//form an array of strings containing the blob names you would like to delete.
string[] blobsToDelete = new string[] { c_saveBlobName };

//Call SubmitUpdatesAsync with the dictionary of the names of blobs to delete as the second parameter.
GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(null, blobsToDelete, c_saveContainerDisplayName);
//Parameters
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName

//Check status of the operation to ensure successful delete.
if(gameSaveOperationResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}

//DELETE A SAVE CONTAINER
GameSaveOperationResult deleteContainerResult = await gameSaveProvider.DeleteContainerAsync(c_saveContainerName);
//Parameter
//string name (name of container to be deleted)

//Check status of the operation to ensure successful delete.
if(deleteContainerResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}
```

Verbindung von Storage-APIs für UWP-apps in dokumentiert sind die [Xbox Live-API-Referenz](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Um ein weiteres Beispiel zu sehen, verwendet der Speicher verbunden, sehen Sie sich, die [Projekt Xbox Live-API, Beispiele Spiel speichern](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).
