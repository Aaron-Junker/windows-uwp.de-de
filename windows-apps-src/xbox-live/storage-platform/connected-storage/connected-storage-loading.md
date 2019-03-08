---
title: Verwenden Sie zum Laden von Daten verbundenen Speicher
description: Erfahren Sie, wie verbundene Speicher zu verwenden, um Daten zu laden.
ms.assetid: c660a456-fe7d-453a-ae7b-9ecaa2ba0a15
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, verbundenen Speicher
ms.localizationpriority: medium
ms.openlocfilehash: a2f7498e8063e290dc506de72b34d2c77d29b26e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637135"
---
# <a name="use-connected-storage-to-load-data"></a>Verwenden Sie zum Laden von Daten verbundenen Speicher

Daten werden asynchron gelesen mithilfe der `ReadAsync` oder `GetAsync` Speichermethode verbunden.

### <a name="to-load-data-from-connected-storage"></a>Zum Laden von Daten aus dem Speicher verbunden

1.  Abrufen einer `ConnectedStorageSpace` für den Benutzer durch Aufrufen von `GetForUserAsync`.

    Im Beispiel xdk-Version der zurückgegebenen `ConnectedStorageSpace` hinzugefügt wird zu einer Zuordnung zum Aktivieren der einfachen Verwaltung von `ConnectedStorageSpace` Objekte für mehrere Benutzer.

2.  Erstellen Sie eine `ConnectedStorageContainer` durch Aufrufen von `CreateContainer` auf die `ConnectedStorageSpace`.
3.  Rufen Sie die Daten durch den Aufruf `ReadAsync` oder `GetAsync` auf die `ConnectedStorageContainer`. `ReadAsync` erfordert, dass Sie für die Übergabe in einem Puffer beim `GetAsync` weist neue Puffer zum Speichern der Daten, die gelesen wird.

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

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status);

// Load data from a fixed container/blob name into an IBuffer
void Load(Windows::Storage::Streams::IBuffer^ buffer)
{

    auto reads = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
    reads->Insert("data", buffer);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(gCurrentUser->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Save Data is Loading

    auto op = container->ReadAsync(reads->GetView());

    op->Completed = ref new AsyncActionCompletedHandler(OnLoadCompleted);
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status)
{
    switch (action->Status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        //Successful load logic here.
        break;

    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        //Fail logic here
        break;

    default:
        //all other possible values of action->status are also failures, alternate fail logic here. 
        break;
    }
}
```

Finden Sie, dass der xdk-Version verbunden Storage-APIs in der xdk-Version CHM-Datei unter dem Pfad dokumentiert: **Xbox eine xdk-Version >>-API-Referenz >> Plattform-API-Referenz >> System-API-Referenz >> Windows.Xbox.Storage**.
Darüber hinaus sind die APIs xdk-Version auf dokumentiert die [developer.microsoft.com Site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
Der Link zum xdk-Version-APIs benötigen Sie ein Microsoft-Account(MSA), die für Xbox-Entwickler Kit(XDK) Zugriff aktiviert wurde.
Windows.Xbox.Storage ist der Name des Namespaces verbundenen Speicher für Xbox One-Konsolen.

## <a name="c-uwp-sample"></a>C#UWP-Beispiel

Während andere APIs xdk-Version-Spiele und UWP-apps verwenden können, ist die xdk-Version-API sehr genau die UWP-API-nachgebildet. Zum Laden von Daten müssen Sie weiterhin die gleichen grundlegenden Schritte befolgen, und notieren Sie sich einige Namensänderungen Namespace- und Klassennamen gleichzeitig. Anstatt den Namespace `Windows::Xbox::Storage` verwenden Sie `Windows.Gaming.XboxLive.Storage`. Die Klasse `ConnectedStorageSpace`, entspricht der `GameSaveProvider`. Die Klasse `ConnectedStorageContainer` entspricht `GameSaveContainer`. Genauere Beschreibung dieser Änderungen in den verbunden Speicherabschnitt [Portieren von Xbox Live-Code aus xdk-Version für die UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 0;
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
    //throw new Exception("Game Save Provider Initialization failed");;
}

//Now you have a GameSaveProvider
//Next you need to call CreateContainer to get a GameSaveContainer

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName);
//Parameter
//string name (name of the GameSaveContainer Created)

//form an array of strings containing the blob names you would like to read.
string[] blobsToRead = new string[] { c_saveBlobName };

// GetAsync allocates a new Dictionary to hold the retrieved data. You can also use ReadAsync
// to provide your own preallocated Dictionary.
GameSaveBlobGetResult result = await gameSaveContainer.GetAsync(blobsToRead);

int loadedData = 0;

//Check status to make sure data was read from the container
if(result.Status == GameSaveErrorStatus.Ok)
{
    //prepare a buffer to receive blob
    IBuffer loadedBuffer;

    //retrieve the named blob from the GetAsync result, place it in loaded buffer.
    result.Value.TryGetValue(c_saveBlobName, out loadedBuffer);

    if(loadedBuffer == null)
    {

        throw new Exception(String.Format("Didn't find expected blob \"{0}\" in the loaded data.", c_saveBlobName));

    }
    DataReader reader = DataReader.FromBuffer(loadedBuffer);
    loadedData = reader.ReadInt32();
}
```

Verbindung von Storage-APIs für UWP-apps in dokumentiert sind die [Xbox Live-API-Referenz](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Um ein weiteres Beispiel zu sehen, verwendet der Speicher verbunden, sehen Sie sich, die [Projekt Xbox Live-API, Beispiele Spiel speichern](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).