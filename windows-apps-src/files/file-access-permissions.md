---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Berechtigungen für den Dateizugriff
description: Apps können standardmäßig auf bestimmte Dateisystemspeicherorte zugreifen. Apps können darüber hinaus mithilfe der Dateiauswahl oder über die Deklaration von Funktionen auf weitere Speicherorte zugreifen.
ms.date: 09/10/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.custom: contperf-fy21q1
ms.openlocfilehash: fa37bc45ae85456aff177417a3875d724b0ebd54
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860253"
---
# <a name="file-access-permissions"></a>Berechtigungen für den Dateizugriff

UWP-Apps (Universelle Windows Plattform) können standardmäßig auf bestimmte Dateisystemspeicherorte zugreifen. Apps können darüber hinaus mithilfe der Dateiauswahl oder über die Deklaration von Funktionen auf weitere Speicherorte zugreifen.

## <a name="locations-that-all-apps-can-access"></a>Für alle Apps zugängliche Speicherorte

Bei Erstellung einer neuen App können Sie standardmäßig auf folgende Dateisystemspeicherorte zugreifen:

### <a name="application-install-directory"></a>Installationsverzeichnis der Anwendung
Der Ordner, in dem Ihre App im System des Benutzers installiert ist.

Es gibt im Wesentlichen zwei Möglichkeiten für den Zugriff auf Dateien und Ordner im Installationsverzeichnis Ihrer App:

1. Sie können auf folgende Weise einen [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) aufrufen, der das Installationsverzeichnis Ihrer App darstellt:

    ```csharp
    Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
    ```
    
    ```javascript
    var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
    ```
    
    ```cppwinrt
    #include <winrt/Windows.Storage.h>
    ...
    Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
    ```
    
    ```cpp
    Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
    ```

    Sie können anschließend mithilfe von [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder)-Methoden auf Dateien und Ordner im Verzeichnis zugreifen. Im Beispiel wird dieser **StorageFolder** in der `installDirectory`-Variablen gespeichert. Im [Informationsbeispiel zum App-Paket](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) auf GitHub erfahren Sie mehr über die Arbeit mit Ihrem App-Paket und dem Installationsverzeichnis.

2. Sie können eine Datei direkt aus dem Installationsverzeichnis Ihrer Anwendung mithilfe der Anwendungs-URI wie folgt aufrufen:

    ```csharp
    using Windows.Storage;            
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFile file{
            co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
        };
        // Process file
    }
    ```
    
    ```cpp
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```

    Nach vollständiger Ausführung von [**GetFileFromApplicationUriAsync**](/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) wird ein [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) zurückgegeben, das die Datei `file.txt` im Installationsverzeichnis der App darstellt (`file` im Beispiel).
    
    Das Präfix „ms-appx:///“ in der URI bezieht sich auf das Installationsverzeichnis der Anwendung. Weitere Informationen zur Verwendung von App-URIs finden Sie unter [Verwenden von URIs zum Verweisen auf Inhalte](/previous-versions/windows/apps/hh781215(v=win.10)).

Darüber hinaus können Sie im Gegensatz zu anderen Speicherorten auf das Installationsverzeichnis Ihrer App auch direkt mit [Win32 und COM für UWP-Apps (Universelle Windows-Plattform)](/uwp/win32-and-com/win32-and-com-for-uwp-apps) und einigen [Funktionen der C/C++-Standardbibliothek in Microsoft Visual Studio](/cpp/cpp/c-cpp-language-and-standard-libraries) zugreifen.

Das Installationsverzeichnis der Anwendung ist ein schreibgeschützter Speicherort. Sie können über die Dateiauswahl nicht auf das Installationsverzeichnis zugreifen.

### <a name="application-data-locations"></a>Speicherorte von Anwendungsdaten
Die Ordner, in denen Ihre App Daten speichern kann. Diese Ordner (lokal, servergespeichert und temporär) werden nach Installation Ihrer Anwendung erstellt.

Es gibt im Wesentlichen zwei Möglichkeiten, auf Dateien und Ordner in den Dateispeicherorten Ihrer App zuzugreifen:

1.  Verwenden Sie die [**ApplicationData**](/uwp/api/Windows.Storage.ApplicationData)-Eigenschaften, um einen Dateiordner abzurufen.

    Sie können zum Beispiel [**ApplicationData**](/uwp/api/Windows.Storage.ApplicationData).[**LocalFolder**](/uwp/api/windows.storage.applicationdata.localfolder) verwenden, um einen [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) abzurufen, der den lokalen Ordner Ihrer Anwendung wie folgt darstellt:
    
    ```csharp
    using Windows.Storage;
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    ```
    
    ```javascript
    var localFolder = Windows.Storage.ApplicationData.current.localFolder;
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{
        Windows::Storage::ApplicationData::Current().LocalFolder()
    };
    ```
    
    ```cpp
    using namespace Windows::Storage;
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    ```
    
    Wenn Sie auf den servergespeicherten oder temporären Ordner Ihrer Anwendung zugreifen möchten, verwenden Sie stattdessen die [**RoamingFolder**](/uwp/api/windows.storage.applicationdata.roamingfolder)- oder [**TemporaryFolder**](/uwp/api/windows.storage.applicationdata.temporaryfolder)-Eigenschaft.
    
    Nach dem Aufrufen des [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder), der den Dateispeicherort der Anwendung darstellt, können Sie auf Dateien und Ordner im Verzeichnis mithilfe der **StorageFolder**-Methode zugreifen. Im Beispiel werden diese **StorageFolder**-Objekte in der `localFolder`-Variablen gespeichert. Weitere Informationen zum Verwenden der Speicherorte von App-Daten finden Sie auf der Seite [ApplicationData-Klasse](/uwp/api/windows.storage.applicationdata). Sie können auch das [Beispiel für Anwendungsdaten](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) von GitHub herunterladen.

2. Sie können eine Datei direkt aus dem lokalen Ordner Ihrer App mithilfe einer App-URI wie folgt aufrufen:
    
    ```csharp
    using Windows.Storage;
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```
    
    Nach vollständiger Ausführung von [**GetFileFromApplicationUriAsync**](/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) wird ein [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) zurückgegeben, das die Datei `file.txt` im lokalen Ordner der App darstellt (`file` im Beispiel).
    
    Das Präfix „ms-appdata:///local/“ in der URI bezieht sich auf den lokalen Ordner der Anwendung. Für den Zugriff auf Dateien in servergespeicherten oder temporären Ordnern verwenden Sie „ms-appdata:///roaming/“ oder „ms-appdata:///temporary/“. Weitere Informationen zur Verwendung von Anwendungs-URIs finden Sie in [So wird's gemacht: Laden von Dateiressourcen](/previous-versions/windows/apps/hh781229(v=win.10)).

Darüber hinaus können Sie, im Gegensatz zu anderen Speicherorten, auf Dateien in den App-Datenspeicherorten auch mit [Win32 und COM für UWP-Apps (Universelle Windows-Plattform)](/uwp/win32-and-com/win32-and-com-for-uwp-apps) und einigen Funktionen der C/C++-Standardbibliothek in Microsoft Visual Studio zugreifen.

Sie können über die Dateiauswahl nicht auf lokale, servergespeicherte oder temporäre Ordner zugreifen.

### <a name="removable-devices"></a>Wechselmedien

Darüber hinaus kann Ihre App standardmäßig auf einige der Dateien auf verbundenen Geräten zugreifen. Diese Möglichkeit besteht, wenn Ihre App die [Erweiterung für die automatische Wiedergabe](/previous-versions/windows/apps/hh464906(v=win.10)) verwendet, die automatisch gestartet wird, sobald Benutzer ein Gerät, wie eine Kamera oder einen USB-Speicherstick, an Ihr System anschließen. Die Dateien, auf welche Ihre App zugreifen kann, sind auf bestimmte Dateitypen begrenzt, die über Deklarationen von Dateitypzuordnungen in Ihrem App-Manifest festgelegt sind.

Sie können natürlich auch auf Dateien und Ordner auf einem Wechselmedium mithilfe der Dateiauswahl zugreifen (unter Verwendung von [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) und [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker)) und den Benutzer die Dateien und Ordner auswählen lassen, auf welche Ihre App Zugriff haben soll. Weitere Informationen zur Verwendung der Dateiauswahl finden Sie unter [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md).

> [!NOTE]
> Weitere Informationen zum Zugriff auf eine SD-Karte oder andere Wechselgeräte finden Sie unter [Zugreifen auf die SD-Karte](access-the-sd-card.md).

## <a name="locations-that-uwp-apps-can-access"></a>Speicherorte, auf die UWP-Apps zugreifen können

### <a name="users-downloads-folder"></a>Ordner „Downloads“ des Benutzers

Der Ordner, in dem heruntergeladene Dateien standardmäßig gespeichert werden.

Standardmäßig kann Ihre Anwendung nur auf Dateien und Ordner im Ordner "Downloads" des Benutzers zugreifen, die von Ihrer App erstellt wurden. Sie können jedoch durch Aufrufen der Dateiauswahl auf weitere Dateien und Ordner im Downloadordner des Benutzers zugreifen ([**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) oder [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker)), sodass Benutzer zu Dateien oder Ordner navigieren und diese auswählen können und Ihre Anwendung Zugriff auf diese erhält.

- Sie können im Downloadordner des Benutzers eine Datei wie folgt erstellen:

    ```csharp
    using Windows.Storage;
    StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
    ```
    
    ```javascript
    Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
        function(newFile) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile newFile{
        co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
    createFileTask.then([](StorageFile^ newFile)
    {
        // Process file
    });
    ```

    [**DownloadsFolder**](/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFileAsync**](/uwp/api/windows.storage.downloadsfolder.createfileasync) ist überladen, sodass Sie festlegen können, was das System im Falle einer bereits vorhandenen gleichnamigen Datei im Ordner „Downloads“ des Benutzers tun sollte. Nach vollständiger Ausführung dieser Methoden wird ein [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) zurückgegeben, das die erstellte Datei darstellt. Diese Datei wird im Beispiel `newFile` genannt.

- Sie können im Downloadordner des Benutzers wie folgt einen Unterordner erstellen:

    ```csharp
    using Windows.Storage;
    StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
    ```
    
    ```javascript
    Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
        function(newFolder) {
            // Process folder
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder newFolder{
        co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
    };
    // Process folder
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
    createFolderTask.then([](StorageFolder^ newFolder)
    {
        // Process folder
    });
    ```

    [**DownloadsFolder**](/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFolderAsync**](/uwp/api/windows.storage.downloadsfolder.createfolderasync) ist überladen, sodass Sie festlegen können, was das System im Falle eines bereits vorhandenen gleichnamigen Unterordners im Ordner „Downloads“ des Benutzers tun sollte. Nach vollständiger Ausführung dieser Methoden wird ein [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) zurückgegeben, der den erstellten Unterordner darstellt. Diese Datei wird im Beispiel `newFolder` genannt.

## <a name="accessing-additional-locations"></a>Zugriff auf zusätzliche Speicherorte

Zusätzlich zu den Standardspeicherorten kann eine App durch das [Deklarieren von Funktionen im App-Manifest](../packaging/app-capability-declarations.md) oder durch [Aufrufen der Dateiauswahl](quickstart-using-file-and-folder-pickers.md) auf zusätzliche Dateien und Ordner zugreifen, um den Benutzer Dateien und Ordner auswählen zu lassen, auf die die App Zugriff haben soll.

Apps, die die Erweiterung [AppExecutionAlias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) deklarieren, haben Dateisystemberechtigungen für das Verzeichnis, aus dem sie im Konsolenfenster gestartet werden, und darunter liegende Verzeichnisse.

### <a name="retaining-access-to-files-and-folders"></a>Beibehalten des Zugriffs auf Dateien und Ordner

Wenn Ihre App eine Datei oder einen Ordner über eine Auswahl, eine Dateiaktivierung, einen Drag-and-Drop-Vorgang usw. abruft, hat sie nur Zugriff auf diese Datei oder diesen Ordner, bis die App beendet wird. Wenn Sie in Zukunft automatisch auf die Datei oder den Ordner zugreifen möchten, können Sie die Datei bzw. den Ordner zur [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) hinzufügen, damit Ihre App in Zukunft problemlos auf dieses Element zugreifen kann. Sie können auch die [**MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) verwenden, um auf einfache Weise eine Liste der zuletzt verwendeten Dateien zu verwalten.

### <a name="capabilities-for-accessing-other-locations"></a>Funktionen für den Zugriff auf andere Speicherorte

In der folgenden Tabelle sind weitere Speicherorte aufgeführt, auf die Sie durch Deklarieren von einer oder mehreren Funktionen und Verwenden der zugehörigen [**Windows.Storage**](/uwp/api/Windows.Storage)-APIs zugreifen können.

| Standort | Funktion | Windows.Storage-API |
|----------|------------|---------------------|
| Alle Dateien, auf die der Benutzer Zugriff hat. Beispiel: Dokumente, Bilder, Fotos, Downloads, Desktop, OneDrive usw. | **broadFileSystemAccess**<br><br>Dies ist eine eingeschränkte Funktion. Der Zugriff kann in **Einstellungen** > **Datenschutz** > **Dateisystem** konfiguriert werden. Weil Benutzer die Berechtigung in **Einstellungen** jederzeit gewähren oder verweigern können, sollten Sie sicherstellen, dass Ihre App auf diese Änderungen flexibel reagiert. Wenn Sie feststellen, dass Ihre App keinen Zugriff hat, können Sie den Benutzer auffordern, die Einstellung zu ändern, indem Sie ihm einen Link zum Artikel [Dateisystemzugriff unter Windows 10 und Datenschutz](https://support.microsoft.com/help/4468237/windows-10-file-system-access-and-privacy-microsoft-privacy) bereitstellen. Beachten Sie, dass der Benutzer die App schließen, die Einstellung umschalten und dann die App neu starten muss. Wird die Einstellung umgeschaltet, während Ihre App ausgeführt wird, hält die Plattform die App an, damit Sie den Status speichern können, und erzwingt dann das Beenden der App, um die neue Einstellung anzuwenden. Im Update vom April 2018 lautet die Standardeinstellung für die Berechtigung „Ein“. Im Update vom Oktober 2018 lautet sie „Aus“.<br /><br />Wenn Sie eine App an den Store übermitteln, die diese Funktion deklariert, müssen Sie zusätzliche Beschreibungen dazu bereitstellen, warum die App diese Funktion benötigt und wie sie sie verwenden wird.<br/><br/>Diese Funktion funktioniert für APIs im Namespace [**Windows.Storage**](/uwp/api/Windows.Storage). Im Abschnitt **Beispiel** am Ende dieses Artikels finden Sie ein Beispiel zum Aktivieren dieser Funktion in Ihrer App.<br/><br/>**Hinweis:** Diese Funktion wird auf Xbox nicht unterstützt. | – |
| Dokumente | **documentsLibrary**<br><br>Hinweis: Sie müssen Ihrem App-Manifest Dateitypzuordnungen hinzufügen, die bestimmte Dateitypen deklarieren, auf die Ihre App an diesem Speicherort Zugriff hat. <br><br>Verwenden Sie diese Funktion, wenn Ihre App:<br>- Den plattformübergreifenden Offlinezugriff auf bestimmte OneDrive-Inhalte mit gültigen OneDrive-URLs oder Ressourcen-IDs ermöglicht.<br>– Geöffnete Dateien im Offlinezustand automatisch im OneDrive des Benutzers speichert. | [KnownFolders.DocumentsLibrary](/uwp/api/windows.storage.knownfolders.documentslibrary) |
| Musik     | **musicLibrary** <br>Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](/uwp/api/windows.storage.knownfolders.musiclibrary) |    
| Bilder  | **picturesLibrary**<br> Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](/uwp/api/windows.storage.knownfolders.pictureslibrary) |  
| Videos    | **videosLibrary**<br>Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](/uwp/api/windows.storage.knownfolders.videoslibrary) |   
| Wechselmedien  | **removableStorage**  <br><br>Hinweis: Sie müssen Ihrem App-Manifest Dateitypzuordnungen hinzufügen, die bestimmte Dateitypen deklarieren, auf die Ihre App an diesem Speicherort Zugriff hat. <br><br>Weitere Informationen finden Sie unter [Zugreifen auf die SD-Karte](access-the-sd-card.md). | [KnownFolders.RemovableDevices](/uwp/api/windows.storage.knownfolders.removabledevices) |  
| Bibliotheken der Heimnetzgruppen  | Mindestens eine der folgenden Funktionen ist erforderlich. <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.HomeGroup](/uwp/api/windows.storage.knownfolders.homegroup) |      
| Medienservergeräte (DLNA) | Mindestens eine der folgenden Funktionen ist erforderlich. <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.MediaServerDevices](/uwp/api/windows.storage.knownfolders.mediaserverdevices) |
| Universal Naming Convention (UNC)-Ordner | Eine Kombination der folgenden Funktionen ist erforderlich. <br><br>Die Funktion für Heim- oder Arbeitsplatznetzwerke: <br>- **privateNetworkClientServer** <br><br>Zudem mindestens eine Funktion zu Internet und öffentlichen Netzwerken: <br>- **internetClient** <br>- **internetClientServer** <br><br>Und gegebenenfalls die Funktion für Domänenanmeldeinformationen:<br>- **enterpriseAuthentication** <br><br>**Hinweis:** Sie müssen Ihrem App-Manifest Dateitypzuordnungen hinzufügen, die bestimmte Dateitypen deklarieren, auf die Ihre App an diesem Speicherort zugreifen kann. | Abrufen eines Ordners mithilfe von: <br>[StorageFolder.GetFolderFromPathAsync](/uwp/api/windows.storage.storagefolder.getfolderfrompathasync) <br><br>Abrufen einer Datei mithilfe von: <br>[StorageFile.GetFileFromPathAsync](/uwp/api/windows.storage.storagefile.getfilefrompathasync) |

### <a name="example"></a>Beispiel

In diesem Beispiel wird die eingeschränkte **broadFileSystemAccess**-Funktion hinzugefügt. Zusätzlich zur Angabe der Funktion muss der Namespace `rescap` hinzugefügt werden und wird auch zu `IgnorableNamespaces` hinzugefügt.

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> Eine vollständige Liste der App-Funktionen finden Sie unter [Deklarationen von App-Funktionen](../packaging/app-capability-declarations.md).
