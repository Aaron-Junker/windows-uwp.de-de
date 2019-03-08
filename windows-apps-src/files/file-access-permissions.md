---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Berechtigungen für den Dateizugriff
description: Apps können standardmäßig auf bestimmte Dateisystemspeicherorte zugreifen. Apps können darüber hinaus mithilfe der Dateiauswahl oder über die Deklaration von Funktionen auf weitere Speicherorte zugreifen.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.openlocfilehash: 4845b20ed74642f6fb34ea40dd774c91ae378e7b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630405"
---
# <a name="file-access-permissions"></a>Berechtigungen für den Dateizugriff

Apps der universellen Windows-Plattform (UWP), können standardmäßig auf bestimmte Speicherorte im Dateisystem zugreifen. Apps können darüber hinaus mithilfe der Dateiauswahl oder über die Deklaration von Funktionen auf weitere Speicherorte zugreifen.

## <a name="the-locations-that-all-apps-can-access"></a>Für alle Apps zugängliche Speicherorte

Bei Erstellung einer neuen App können Sie standardmäßig auf folgende Dateisystemspeicherorte zugreifen:

### <a name="application-install-directory"></a>Installationsverzeichnis der Anwendung
Der Ordner, in denen Ihre app auf dem System des Benutzers installiert ist.

Es gibt zwei Hauptmethoden, den Zugriff auf Dateien und Ordner in Ihrer app-Installationsverzeichnis:

1. Sie können auf folgende Weise einen [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) aufrufen, der das Installationsverzeichnis Ihrer App darstellt:

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

    Sie können anschließend mithilfe von [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)-Methoden auf Dateien und Ordner im Verzeichnis zugreifen. Im Beispiel wird dieser **StorageFolder** in der `installDirectory`-Variablen gespeichert. Sie können das [Informationsbeispiel des Anwendungspakets](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) von GitHub herunterladen, um mehr über die Arbeit mit dem App-Paket und dem Installationsverzeichnis zu erfahren.

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

    Nach vollständiger Ausführung von [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) wird ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) zurückgegeben, das die Datei `file.txt` im Installationsverzeichnis der App darstellt (`file` im Beispiel).
    
    Das Präfix „ms-appx:///“ in der URI bezieht sich auf das Installationsverzeichnis der Anwendung. Weitere Informationen zur Verwendung von App-URIs finden Sie unter [Verwenden von URIs zum Verweisen auf Inhalte](https://msdn.microsoft.com/library/windows/apps/hh781215).

Darüber hinaus können Sie im Gegensatz zu anderen Speicherorten auf das Installationsverzeichnis Ihrer App auch direkt mit [Win32 und COM für UWP-Apps (Universelle Windows-Plattform)](https://msdn.microsoft.com/library/windows/apps/br205757) und einigen [Funktionen der C/C++-Standardbibliothek in Microsoft Visual Studio](https://msdn.microsoft.com/library/hh875057.aspx) zugreifen.

Das Installationsverzeichnis der Anwendung ist ein schreibgeschützter Speicherort. Sie können nicht über die Dateiauswahl für das Installationsverzeichnis zugreifen.

### <a name="application-data-locations"></a>Anwendung von Standorten
Die Ordner, in denen Ihre App Daten speichern kann. Diese Ordner (lokal, servergespeichert und temporär) werden nach Installation Ihrer Anwendung erstellt.

Es gibt zwei Hauptmethoden, Dateien und Ordner von Ihrer app von Standorten zugreifen:

1.  Verwenden Sie die [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)-Eigenschaften, um einen Dateiordner abzurufen.

    Sie können zum Beispiel [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) verwenden, um einen [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) abzurufen, der den lokalen Ordner Ihrer Anwendung wie folgt darstellt:
    
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
    
    Wenn Sie auf den servergespeicherten oder temporären Ordner Ihrer Anwendung zugreifen möchten, verwenden Sie stattdessen die [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)- oder [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629)-Eigenschaft.
    
    Nach dem Aufrufen des [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230), der den Dateispeicherort der Anwendung darstellt, können Sie auf Dateien und Ordner im Verzeichnis mithilfe der **StorageFolder**-Methode zugreifen. Im Beispiel werden diese **StorageFolder**-Objekte in der `localFolder`-Variablen gespeichert. Weitere Informationen zum Verwenden der Speicherorte von App-Daten finden Sie unter [ApplicationData-Klasse](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata). Sie können auch das [Beispiel für Anwendungsdaten](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) von GitHub herunterladen.

2. Sie können eine Datei direkt aus Ihrer app lokale Ordner abrufen, mithilfe einer app-URI wie folgt:
    
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
    
    Nach vollständiger Ausführung von [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) wird ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) zurückgegeben, das die Datei `file.txt` im lokalen Ordner der App darstellt (`file` im Beispiel).
    
    Das Präfix „ms-appdata:///local/“ in der URI bezieht sich auf den lokalen Ordner der Anwendung. Für den Zugriff auf Dateien in servergespeicherten oder temporären Ordnern verwenden Sie „ms-appdata:///roaming/“ oder „ms-appdata:///temporary/“. Weitere Informationen zur Verwendung von Anwendungs-URIs finden Sie in [So wird's gemacht: Laden von Dateiressourcen](https://msdn.microsoft.com/library/windows/apps/hh781229).

Darüber hinaus können Sie, im Gegensatz zu anderen Speicherorten, auf Dateien in den App-Datenspeicherorten auch mit [Win32 und COM für UWP-Apps (Universelle Windows-Plattform)](https://msdn.microsoft.com/library/windows/apps/br205757) und einigen Funktionen der C/C++-Standardbibliothek in Microsoft Visual Studio zugreifen.

Sie können nicht die lokale roaming oder temporären Ordner über die Dateiauswahl zugreifen.

### <a name="removable-devices"></a>Wechselmedien
Darüber hinaus kann Ihre App standardmäßig auf einige der Dateien auf verbundenen Geräten zugreifen. Diese Möglichkeit besteht, wenn Ihre App die [Erweiterung für die automatische Wiedergabe](https://msdn.microsoft.com/library/windows/apps/xaml/hh464906.aspx#autoplay) verwendet, die automatisch gestartet wird, sobald Benutzer ein Gerät, wie eine Kamera oder einen USB-Speicherstick, an Ihr System anschließen. Die Dateien, auf welche Ihre App zugreifen kann, sind auf bestimmte Dateitypen begrenzt, die über Deklarationen von Dateitypzuordnungen in Ihrem App-Manifest festgelegt sind.

Sie können natürlich auch auf Dateien und Ordner auf einem Wechselmedium mithilfe der Dateiauswahl zugreifen (unter Verwendung von [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) und [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)) und den Benutzer die Dateien und Ordner auswählen lassen, auf welche Ihre App Zugriff haben soll. Weitere Informationen zur Verwendung der Dateiauswahl finden Sie unter [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md).

> [!NOTE]
> Weitere Informationen zum Zugriff auf eine SD-Karte oder andere Wechselgeräte finden Sie unter [Zugreifen auf die SD-Karte](access-the-sd-card.md).

## <a name="locations-that-uwp-apps-can-access"></a>Standorte, die UWP-apps zugreifen können
### <a name="users-downloads-folder"></a>Ordner "Downloads" des Benutzers

Der Ordner, in dem heruntergeladene Dateien standardmäßig gespeichert werden.

Standardmäßig kann Ihre Anwendung nur auf Dateien und Ordner im Ordner "Downloads" des Benutzers zugreifen, die von Ihrer App erstellt wurden. Sie können jedoch durch Aufrufen der Dateiauswahl auf weitere Dateien und Ordner im Downloadordner des Benutzers zugreifen ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) oder [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)), sodass Benutzer zu Dateien oder Ordner navigieren und diese auswählen können und Ihre Anwendung Zugriff auf diese erhält.

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

    [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[ **CreateFileAsync** ](https://msdn.microsoft.com/library/windows/apps/hh996761) ist überladen, sodass Sie angeben können, was das System tun sollte Wenn bereits eine vorhandene Datei im Ordner "Downloads" mit dem gleichen Namen vorhanden ist. Nach vollständiger Ausführung dieser Methoden wird ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) zurückgegeben, das die erstellte Datei darstellt. Diese Datei wird im Beispiel `newFile` genannt.

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

    [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[ **CreateFolderAsync** ](https://msdn.microsoft.com/library/windows/apps/hh996763) ist überladen, sodass Sie angeben können, was das System tun sollte, wenn bereits ein vorhandener Unterordner im Ordner "Downloads" mit dem gleichen Namen vorhanden ist. Nach vollständiger Ausführung dieser Methoden wird ein [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) zurückgegeben, der den erstellten Unterordner darstellt. Diese Datei wird im Beispiel `newFolder` genannt.

Wenn Sie eine Datei oder einen Ordner im Downloadordner erstellen, empfehlen wir, die Datei oder den Ordner der [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) Ihrer App hinzuzufügen, sodass zukünftig leicht auf dieses Element zugegriffen werden kann.

## <a name="accessing-additional-locations"></a>Zugriff auf zusätzliche Speicherorte

Zusätzlich zu den Standardspeicherorten kann eine App durch das Deklarieren von Funktionen im App-Manifest (siehe [Deklaration der App-Funktionen](https://msdn.microsoft.com/library/windows/apps/mt270968)) oder durch Aufrufen der Dateiauswahl auf zusätzliche Dateien und Ordner zugreifen, um den Benutzer Dateien und Ordner auswählen zu lassen, auf welche die App Zugriff haben soll (siehe [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md)).

App, die die [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)-Erweiterung deklarieren, haben Dateisystemberechtigungen für das Verzeichnis, aus dem sie im Konsolenfenster gestartet werden, und darunter liegende Verzeichnisse.

In der folgenden Tabelle sind weitere Speicherorte aufgeführt, auf die Sie durch Deklarieren von Funktionen und Verwenden der zugehörigen [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)-APIs zugreifen können:

| Pfad | Funktion | Windows.Storage-API |
|----------|------------|---------------------|
| Alle Dateien, auf die der Benutzer Zugriff hat. Beispiel: Dokumente, Bilder, Fotos, Downloads, Desktop, OneDrive usw. | broadFileSystemAccess<br><br>Dies ist eine eingeschränkte Funktion. Zugriff-Zuordnungsvorgänge kann in **Einstellungen** > **Datenschutz** > **Dateisystem**. Da Benutzer gewähren oder verweigern die Berechtigung von einem beliebigen Zeitpunkt im können **Einstellungen**, sollten Sie sicherstellen, dass es sich bei Ihrer app auf diese Änderungen stabil ist. Wenn Sie feststellen, dass Ihre app nicht zugreifen kann, können Sie den Benutzer auffordern, die Einstellung zu ändern, indem ein Link zu der [Dateisystemzugriff für Windows 10 und Datenschutz](https://privacy.microsoft.com/en-US/windows-10-file-system-access-and-privacy) Artikel. Beachten Sie, dass der Benutzer muss die app schließen, schalten Sie die Einstellung, und die app neu starten. Wenn sie die Einstellung umzuschalten, während die app ausgeführt wird, hält die Plattform Ihrer app, damit können Sie den Status speichern und dann beenden Sie die app erzwungen, um die neue Einstellung zu übernehmen. Im April 2018 Update ist die Standardeinstellung für die Berechtigung. Im Update vom Oktober 2018 ist standardmäßig deaktiviert.<br /><br />Wenn Sie eine App an den Store übermitteln, die diese Funktion deklariert, müssen Sie zusätzliche Beschreibungen dazu bereitstellen, warum die App diese Funktion benötigt und wie sie diese verwenden wird.<br>Diese Funktion ist für APIs in der [ **Windows.Storage** ](https://msdn.microsoft.com/library/windows/apps/BR227346) Namespace. Finden Sie unter den **Beispiel** Abschnitt am Ende dieses Artikels ein Beispiel für Informationen zum Aktivieren dieser Funktion in Ihrer app. | n. a. |
| Dokumente | DocumentsLibrary <br><br>Hinweis: Sie müssen eine Dateitypzuordnung hinzufügen, in Ihrer app-Manifest, in denen bestimmte Dateitypen deklariert, die Ihre app an diesem Standort zugreifen können. <br><br>Verwenden Sie diese Funktion, wenn Ihre App:<br>- Den plattformübergreifenden Offlinezugriff auf bestimmte OneDrive-Inhalte mit gültigen OneDrive-URLs oder Ressourcen-IDs ermöglicht.<br>– Speichert geöffneten Dateien auf OneDrive des Benutzers, während automatisch offline | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| Musik     | MusicLibrary <br>Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| Bilder  | PicturesLibrary<br> Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| Videos    | VideosLibrary<br>Weitere Informationen finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| Wechselmedien  | RemovableDevices <br><br>Hinweis: Sie müssen Ihrem App-Manifest Dateitypzuordnungen hinzufügen, die bestimmte Dateitypen deklarieren, auf die Ihre App an diesem Speicherort Zugriff hat. <br><br>Weitere Informationen finden Sie unter [Zugreifen auf die SD-Karte](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| Bibliotheken der Heimnetzgruppen  | Mindestens eine der folgenden Funktionen ist erforderlich. <br>– MusicLibrary <br>– PicturesLibrary <br>– VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| Medienservergeräte (DLNA) | Mindestens eine der folgenden Funktionen ist erforderlich. <br>– MusicLibrary <br>– PicturesLibrary <br>– VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) |
| Universal Naming Convention (UNC)-Ordner | Eine Kombination der folgenden Funktionen ist erforderlich. <br><br>Die Funktion für Heim- oder Arbeitsplatznetzwerke: <br>– PrivateNetworkClientServer <br><br>Zudem mindestens eine Funktion zu Internet und öffentlichen Netzwerken: <br>– InternetClient <br>– InternetClientServer <br><br>Und gegebenenfalls die Funktion für Domänenanmeldeinformationen:<br>– EnterpriseAuthentication <br><br>Hinweis: Sie müssen eine Dateitypzuordnung hinzufügen, in Ihrer app-Manifest, in denen bestimmte Dateitypen deklariert, die Ihre app an diesem Standort zugreifen können. | Abrufen eines Ordners mithilfe von: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>Abrufen einer Datei mithilfe von: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |

**Beispiel**

In diesem Beispiel wird die eingeschränkte `broadFileSystemAccess`-Funktion hinzugefügt. Zusätzlich zur Angabe der Funktion muss der `rescap`-Namespace muss hinzugefügt werden und wird auch zu `IgnorableNamespaces` hinzugefügt:

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp uap5 rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> Eine vollständige Liste der App-Funktionen finden Sie unter [Deklarationen von App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).
