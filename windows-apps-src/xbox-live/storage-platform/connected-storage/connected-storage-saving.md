---
title: Verwenden Sie verbundene Speicher zum Speichern von Daten
description: Erfahren Sie, wie verbundene Speicher zum Speichern von Daten verwenden.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, verbundenen Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 4140e3bbe0f0ab229e3637008e01892f4179292e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617615"
---
# <a name="use-connected-storage-to-save-data"></a>Verwenden Sie verbundene Speicher zum Speichern von Daten


Datenspeicherung wird asynchron durch das Erstellen einer `ConnectedStorageContainer` in einer `ConnectedStorageSpace` für einen Benutzer und dem Aufrufen der `SubmitUpdatesAsync` Methode für den Container.

> [!IMPORTANT]
> Datenabhängigkeiten im verbundenen Storage-Container sind unsicher. Z. B. möglicherweise von einem Container in die Cloud hochladen beenden, während eine andere für den Upload in der Warteschlange bleiben kann. Wenn der Benutzer in einer anderen Konsole verschoben werden soll, können der Synchronisierungsvorgang den ersten Container synchronisiert und in der zweiten Konsole zugegriffen werden, ohne den ersten Container vorhanden.

## <a name="to-save-data-to-connected-storage"></a>Zum Speichern von Daten in den Speicher verbunden

1.  Abrufen einer `ConnectedStorageSpace` Objekt für den Benutzer durch Aufrufen von `GetForUserAsync`.

    Im Beispiel xdk-Version der zurückgegebenen `ConnectedStorageSpace` Objekt hinzugefügt wird zu einer Zuordnung zum Aktivieren der einfachen Verwaltung von `ConnectedStorageSpace` Objekte für mehrere Benutzer.

2.  Erstellen Sie eine `ConnectedStorageContainer` Objekt durch Aufrufen von `CreateContainer` auf die `ConnectedStorageSpace` Objekt.
3.  Rufen Sie `SubmitUpdatesAsync` auf die `ConnectedStorageContainer` mit, die Sie speichern Daten-Blob als spielen die `blobsToWrite` Parameter.

## <a name="c-xdk-sample"></a>Beispiel für C++ xdk-Version

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() {return true;};

User^ gCurrentUser;
IBuffer^ WrapRawBuffer( void* ptr, size_t size );

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

enum Color { RED, BLUE };
enum EngineSize { BIG, SMALL };

struct CarData
{
    Color color;
    bool hasWheels;
    bool hasFancyRims;
    EngineSize engineSize;
};


const int MAX_CARS = 10;

struct SaveData
{
    CarData cars[MAX_CARS];
    int numCars;
    int currentCar;
    int cash;
};

SaveData gMySaveData;

void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user);

bool ItIsTimeToSaveACheckpoint() {return true;};

void Update()
{
    if (ItIsTimeToSaveACheckpoint())
        SaveCheckpoint(WrapRawBuffer(&gMySaveData,sizeof(SaveData)),gCurrentUser);
}


void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user)
{
     auto storageSpace = gConnectedStorageSpaceForUsers->Lookup( user->Id );

     auto container = storageSpace->CreateContainer("Saves/Checkpoint");

     auto updates = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
     updates->Insert("data", buffer);

     auto op = container->SubmitUpdatesAsync(updates->GetView(), nullptr);

     //Save is happening here asynchronously

     op->Completed = ref new AsyncActionCompletedHandler(
               [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
                   //Save function has completed
                   //This area can be filled with further post save logic.
     });
}
```

Finden Sie, dass der xdk-Version verbunden Storage-APIs in der xdk-Version CHM-Datei unter dem Pfad dokumentiert: **Xbox eine xdk-Version >>-API-Referenz >> Plattform-API-Referenz >> System-API-Referenz >> Windows.Xbox.Storage**.
Darüber hinaus sind die APIs xdk-Version auf dokumentiert die [developer.microsoft.com Site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
Der Link zum xdk-Version-APIs benötigen Sie ein Microsoft-Account(MSA), die für Xbox-Entwickler Kit(XDK) Zugriff aktiviert wurde.
Windows.Xbox.Storage ist der Name des Namespaces verbundenen Speicher für Xbox One-Konsolen.

## <a name="c-uwp-sample"></a>C#UWP-Beispiel

Während andere APIs xdk-Version-Spiele und UWP-apps verwenden können, ist die xdk-Version-API sehr genau die UWP-API-nachgebildet. Zum Speichern von Daten müssen Sie weiterhin die gleichen grundlegenden Schritte befolgen, und notieren Sie sich einige Namensänderungen Namespace- und Klassennamen gleichzeitig. Anstatt den Namespace `Windows::Xbox::Storage` verwenden Sie `Windows.Gaming.XboxLive.Storage`. Die Klasse `ConnectedStorageSpace`, entspricht der `GameSaveProvider`. Die Klasse `ConnectedStorageContainer` entspricht `GameSaveContainer`. Genauere Beschreibung dieser Änderungen in den verbunden Speicherabschnitt [Portieren von Xbox Live-Code aus xdk-Version für die UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

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
//string name

// To store a value in the container, it needs to be written into a buffer, then stored with
// a blob name in a Dictionary.

DataWriter writer = new DataWriter();

writer.WriteInt32(intData); //some number you want to save, in this case 23.

IBuffer dataBuffer = writer.DetachBuffer();

var blobsToWrite = new Dictionary<string, IBuffer>();

blobsToWrite.Add(c_saveBlobName, dataBuffer);

GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(blobsToWrite, null, c_saveContainerDisplayName);
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName
```

Verbindung von Storage-APIs für UWP-apps in dokumentiert sind die [Xbox Live-API-Referenz](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Um ein weiteres Beispiel zu sehen, verwendet der Speicher verbunden, sehen Sie sich, die [Projekt Xbox Live-API, Beispiele Spiel speichern](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).