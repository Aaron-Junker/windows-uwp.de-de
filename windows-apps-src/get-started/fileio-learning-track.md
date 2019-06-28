---
title: Arbeiten mit Dateien
description: Hier erfährst du, wie du auf der universellen Windows-Plattform mit Dateien arbeitest.
ms.date: 05/01/2018
ms.topic: article
keywords: Erste Schritte, UWP, Windows 10, Lernpfad, Dateien, Datei-E/A, Datei lesen, Datei schreiben, Datei erstellen, Text schreiben, Text lesen
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5480638e201dca8a5eb5363d7a5944422c626f67
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66366889"
---
# <a name="work-with-files"></a>Arbeiten mit Dateien

In diesem Thema erfährst du alles, was du wissen musst, um Dateien aus einer UWP-App (Universelle Windows-Plattform) zu lesen bzw. in eine UWP-App zu schreiben. Hier findest du grundlegende Informationen zu den wichtigsten APIs und Typen sowie Links zu weiterführenden Informationen.

Dieser Artikel ist kein Tutorial. Ein Tutorial findest du unter [Erstellen, Schreiben und Lesen einer Datei](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files). Darin wird gezeigt, wie du eine Datei erstellst, liest und schreibst und wie du Puffer und Streams verwendest. Bei Interesse kannst du dir auch aus [Beispiel zum Dateizugriff](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) ansehen. Dort erfährst du, wie du eine Datei erstellst, liest, schreibst, kopierst und löschst. Außerdem erfährst du, wie du Dateieigenschaften abrufst und eine Datei oder einen Ordner speicherst, sodass deine App mühelos erneut darauf zugreifen kann.

Wir sehen uns Code zum Schreiben und Lesen von Text in einer Datei an, und du erfährst, wie du auf den lokalen und temporären Ordner sowie auf den Roamingordner der App zugreifst.

## <a name="what-do-you-need-to-know"></a>Wissenswertes

Im Anschluss findest du die wichtigsten Typen, die du im Zusammenhang mit dem Lesen und Schreiben von Text aus einer bzw. in eine Datei kennen musst:

- [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) stellt eine Datei dar. Diese Klasse enthält Eigenschaften, die Informationen zur Datei liefern, sowie Methoden zum Erstellen, Öffnen, Kopieren, Löschen und Umbenennen von Dateien.
Möglicherweise bist du es gewohnt, mit Zeichenfolgenpfaden zu arbeiten. Es gibt zwar einige UWP-APIs, die einen Zeichenfolgenpfad verwenden, in der Regel wird zur Darstellung einer Datei jedoch ein **StorageFile**-Element verwendet, da einige Dateien, mit denen du in UWP arbeitest, unter Umständen keinen oder einen eher unhandlichen Pfad haben. Verwende [StorageFile.GetFileFromPathAsync()](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync), um einen Zeichenfolgenpfad in ein **StorageFile**-Element zu konvertieren. 

- Mit der Klasse [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) kannst du ganz einfach Text lesen und schreiben. Sie kann auch ein Bytearray oder den Inhalt eines Puffers lesen/schreiben. Diese Klasse ist der Klasse [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) sehr ähnlich. Der Hauptunterschied besteht darin, dass anstelle eines Zeichenfolgenpfads wie bei **PathIO** ein **StorageFile**-Element verwendet wird.
- [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) stellt einen Ordner (Verzeichnis) dar. Diese Klasse verfügt über Methoden zum Erstellen von Dateien, zum Abfragen des Inhalts eines Ordners und zum Erstellen, Umbenennen und Löschen von Ordnern sowie über Eigenschaften, die Informationen zu einem Ordner bereitstellen. 

Gängige Methoden zum Abrufen eines **StorageFolder**-Elements sind:

- [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker): Ermöglicht es dem Benutzer, zum gewünschten Ordner zu navigieren.
- [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current): Stellt das spezifische **StorageFolder**-Element für einen der lokalen Ordner der App bereit (etwa den lokalen oder temporären Ordner oder den Roamingordner).
- [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders): Stellt das **StorageFolder**-Element für bekannte Bibliotheken wie Musik- oder Bildbibliotheken bereit.

## <a name="write-text-to-a-file"></a>Schreiben von Text in eine Datei

 In dieser Einführung konzentrieren wir uns auf ein einfaches Szenario: das Lesen und Schreiben von Text. Dazu sehen wir uns zunächst Code an, in dem die Klasse **FileIO** verwendet wird, um Text in eine Datei zu schreiben.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

Als Erstes geben wir an, wo sich die Datei befindet. `Windows.Storage.ApplicationData.Current.LocalFolder` ermöglicht den Zugriff auf den Ordner mit lokalen Daten, der bei der Installation deiner App erstellt wird. Ausführliche Informationen zu den Ordnern, auf die deine App zugreifen kann, findest du unter [Zugreifen auf das Dateisystem](#access-the-file-system).

Anschließend verwenden wir **StorageFolder**, um die Datei zu erstellen (oder zu öffnen, falls sie bereits vorhanden ist).

Mithilfe der Klasse **FileIO** kannst du mühelos Text in die Datei schreiben. `FileIO.WriteTextAsync()` ersetzt den gesamten Inhalt der Datei durch den angegebenen Text. `FileIO.AppendLinesAsync()` fügt eine Sammlung von Zeichenfolgen an die Datei an. Dabei wird jeweils eine Zeichenfolge pro Zeile geschrieben.

## <a name="read-text-from-a-file"></a>Lesen von Text aus einer Datei

Das Lesen einer Datei beginnt genau wie das Schreiben einer Datei mit der Angabe, wo sich die Datei befindet. Wir verwenden den gleichen Speicherort wie im obigen Beispiel. Anschließend verwenden wir die Klasse **FileIO**, um den Inhalt zu lesen.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Du kannst auch `IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);` verwenden, um die Zeilen der Datei jeweils in einzelne Zeichenfolgen in einer Sammlung zu lesen.

## <a name="access-the-file-system"></a>Zugreifen auf das Dateisystem

Auf der UWP-Plattform ist der Ordnerzugriff eingeschränkt, um die Integrität und Vertraulichkeit der Daten des Benutzers zu gewährleisten. 

### <a name="app-folders"></a>App-Ordner

Bei der Installation einer UWP-App werden unter „C:\Benutzer\<Benutzername>\AppData\Local\Packages\<Bezeichner des App-Pakets>\“ mehrere Ordner erstellt, in denen unter anderem die lokalen und temporären Dateien sowie die Roamingdateien der App gespeichert werden. Die App muss für den Zugriff auf diese Ordner keinerlei Funktionen deklarieren, und andere Apps können nicht auf diese Ordner zugreifen. Bei der Deinstallation der App werden diese Ordner ebenfalls entfernt.

Im Anschluss folgen einige der häufig verwendeten App-Ordner:

- **LocalState**: Für lokale Daten des aktuellen Geräts. Wenn das Gerät gesichert wird, werden die Daten in diesem Verzeichnis in einem Sicherungsabbild in OneDrive gespeichert. Wenn der Benutzer das Gerät zurücksetzt oder austauscht, werden die Daten wiederhergestellt. Auf diesen Ordner kannst du mit Folgendem zugreifen `Windows.Storage.ApplicationData.Current.LocalFolder.` Speichere lokale Daten, die nicht auf OneDrive gesichert werden sollen, im Ordner **LocalCacheFolder**. Auf diesen Ordner kannst du mit `Windows.Storage.ApplicationData.Current.LocalCacheFolder` zugreifen.

- **RoamingState**: Für Daten, die auf allen Geräten repliziert werden sollen, auf denen die App installiert ist. Die Menge an Roamingdaten wird von Windows eingeschränkt. Speichere hier also nur Benutzereinstellungen und kleine Dateien. Auf den Roamingordner kannst du mit `Windows.Storage.ApplicationData.Current.RoamingFolder` zugreifen.

- **TempState**: Für Daten, die gelöscht werden können, wenn die App nicht ausgeführt wird. Auf diesen Ordner kannst du mit `Windows.Storage.ApplicationData.Current.TemporaryFolder` zugreifen.

### <a name="access-the-rest-of-the-file-system"></a>Zugreifen auf das restliche Dateisystem

Eine UWP-App muss deklarieren, dass sie auf eine bestimmte Benutzerbibliothek zugreifen möchte. Hierzu muss ihrem Manifest die entsprechende Funktion hinzugefügt werden. Der Benutzer muss dann bei der Installation der App den Zugriff auf die angegebene Bibliothek autorisieren. Andernfalls wird die App nicht installiert. Es stehen Funktionen für den Zugriff auf die Bild-, Video- und Musikbibliothek zur Verfügung. Eine vollständige Liste findest du unter [Deklarationen von App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations). Verwende die Klasse [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders), um ein **StorageFolder**-Element für diese Bibliotheken abzurufen.

#### <a name="documents-library"></a>Dokumentbibliothek

Es gibt zwar eine Funktion für den Zugriff auf die Dokumentbibliothek des Benutzers, diese Funktion ist jedoch eingeschränkt. Das bedeutet, dass für eine App, die sie deklariert, eine spezielle Genehmigung eingeholt werden muss, damit sie vom Microsoft Store akzeptiert wird. Sie ist nicht für die allgemeine Verwendung vorgesehen. Verwende stattdessen die Datei- oder Ordnerauswahl (siehe [Öffnen von Dateien und Ordnern mit einer Auswahl](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) und [Speichern einer Datei mit einer Auswahl](https://docs.microsoft.com/windows/uwp/files/quickstart-save-a-file-with-a-picker)). Sie ermöglicht es dem Benutzer, zu dem Ordner oder der Datei zu navigieren. Wenn der Benutzer zu einem Ordner oder einer Datei navigiert, erteilt er der App dadurch implizit eine Zugriffsberechtigung für den Ordner oder die Datei, und das System lässt den Zugriff zu.

#### <a name="general-access"></a>Allgemeiner Zugriff

Alternativ kann deine App auch die eingeschränkte Funktion [broadFileSystem](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) im Manifest deklarieren. Diese erfordert ebenfalls eine Microsoft Store-Genehmigung. Die App kann dann auf jede Datei zugreifen, auf die der Benutzer Zugriff hat – ganz ohne Datei- oder Ordnerauswahl.

Eine umfassende Liste der Speicherorte, auf die Apps zugreifen können, findest du unter [Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

## <a name="useful-apis-and-docs"></a>Nützliche APIs und Dokumente

Hier findest du einen kurzen Überblick über die APIs und andere nützliche Dokumentationen, die dir den Einstieg in die Verwendung von Dateien und Ordnern erleichtern.

### <a name="useful-apis"></a>Nützliche APIs

| API | Beschreibung |
|------|---------------|
|  [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) | Stellt Informationen zur Datei sowie Methoden zum Erstellen, Öffnen, Kopieren, Löschen und Umbenennen von Dateien bereit. |
| [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) | Stellt Informationen zum Ordner sowie Methoden zum Erstellen von Dateien und Methoden zum Erstellen, Umbenennen und Löschen von Ordnern bereit. |
| [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) |  Bietet eine einfache Möglichkeit zum Lesen und Schreiben von Text. Diese Klasse kann auch ein Bytearray oder den Inhalt eines Puffers lesen/schreiben. |
| [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) | Bietet eine einfache Möglichkeit zum Lesen und Schreiben von Text aus einer bzw. in eine Datei unter Angabe eines Zeichenfolgenpfads für die Datei. Diese Klasse kann auch ein Bytearray oder den Inhalt eines Puffers lesen/schreiben. |
| [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) & [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) |  Ermöglicht das Lesen und Schreiben von Puffern, Bytes, ganzen Zahlen, GUIDs, Zeitspannen und Ähnlichem aus einem bzw. in einen Stream. |
| [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) | Ermöglicht den Zugriff auf die Ordner, die für die App erstellt wurden – beispielsweise auf den lokalen Ordner, den Roamingordner und den Ordner für temporäre Dateien. |
| [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) |  Ermöglicht es dem Benutzer, einen Ordner auszuwählen, und gibt dafür ein **StorageFolder**-Element zurück. So erhältst du Zugriff auf Speicherorte, auf die die App standardmäßig nicht zugreifen kann. |
| [Windows.Storage.Pickers.FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker) | Ermöglicht es dem Benutzer, eine Datei auszuwählen und zu öffnen, und gibt dafür ein **StorageFile**-Element zurück. So erhältst du Zugriff auf eine Datei, auf die die App standardmäßig nicht zugreifen kann. |
| [Windows.Storage.Pickers.FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) | Ermöglicht es dem Benutzer, den Dateinamen, die Erweiterung und den Speicherort für eine Datei auszuwählen. Gibt ein **StorageFile**-Element zurück. So kannst du eine Datei an einem Speicherort speichern, auf den die App standardmäßig keinen Zugriff hat. |
|  [Windows.Storage.Streams-Namespace](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Ermöglicht das Lesen und Schreiben von Streams. Sieh dir insbesondere die Klassen [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) und [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) zum Lesen und Schreiben von Puffern, Bytes, ganzen Zahlen, GUIDs, Zeitspannen und Ähnlichem an. |

### <a name="useful-docs"></a>Nützliche Dokumentation

| Thema | Beschreibung |
|-------|----------------|
| [Windows.Storage Namespace](https://docs.microsoft.com/uwp/api/windows.storage) | API-Referenzdokumentation |
| [Dateien, Ordner und Bibliotheken](https://docs.microsoft.com/windows/uwp/files/) | Konzeptdokumentation |
| [Erstellen, Schreiben und Lesen einer Datei](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) | Behandelt das Erstellen, Lesen und Schreiben von Text, binären Daten und Streams. |
| [Erste Schritte zum lokalen Speichern von App-Daten](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) | Neben den bewährten Methoden zum Speichern lokaler Daten wird hier auch der Zweck der Ordner „LocalSettings“ und „LocalCache“ erläutert. |
| [Erste Schritte mit Roaming-App-Daten](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) | Eine zweiteilige Reihe zur Verwendung von Roaming-App-Daten. |
| [Richtlinien für das Roaming von Anwendungsdaten](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Berücksichtige diese Richtlinien für das Datenroaming bei der Entwicklung deiner App. |
| [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Bietet eine Übersicht über die verschiedenen App-Datenspeicher (etwa über den lokalen und temporären Ordner und den Roamingordner). Der Abschnitt [Roamingdaten](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#roaming-data) enthält Richtlinien und weitere Informationen zum Schreiben von Daten, die mittels Roaming zwischen Geräten übertragen werden. |
| [Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) | Informationen zu den Speicherorten des Dateisystems, auf die deine App zugreifen kann. |
| [Öffnen von Dateien und Ordnern mit einer Auswahl](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) | Hier erfährst du, wie du auf Dateien und Ordner zugreifst, indem du dem Benutzer eine Auswahlbenutzeroberfläche zur Verfügung stellst. |
| [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Typen zum Lesen und Schreiben von Streams. |
| [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries) | Hier erfährst du, wie du Ordner aus Bibliotheken entfernst, die Liste der Ordner in einer Bibliothek abrufst und gespeicherte Fotos, Musik und Videos erkennst. |

## <a name="useful-code-samples"></a>Nützliche Codebeispiele

| Codebeispiel | Beschreibung |
|-----------------|---------------|
| [Beispiel für Anwendungsdaten](https://code.msdn.microsoft.com/windowsapps/ApplicationData-sample-fb043eb2) | Zeigt, wie du mithilfe der Anwendungsdaten-APIs benutzerspezifische Daten speichern und abrufen kannst. |
| [Beispiel zum Dateizugriff](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | Veranschaulicht das Erstellen, Lesen, Schreiben, Kopieren und Löschen einer Datei. |
| [Beispiel zur Dateiauswahl](https://code.msdn.microsoft.com/windowsapps/File-picker-sample-9f294cba) | Zeigt, wie auf Dateien und Ordner zugegriffen werden kann, indem dem Benutzer die Möglichkeit gegeben wird, diese über die Benutzeroberfläche auszuwählen. Außerdem erfährst du hier, wie du eine Datei so speicherst, dass der Benutzer den Namen, Dateityp und Speicherort der zu speichernden Datei angeben kann. |
| [JSON-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) | Veranschaulicht das Codieren und Decodieren von JSON-Objekten (JavaScript Object Notation), Arrays, Zeichenfolgen, Zahlen und booleschen Werten mithilfe des [Windows.Data.Json-Namespace](https://docs.microsoft.com/uwp/api/Windows.Data.Json). |
| [Weitere Codebeispiele](https://developer.microsoft.com//windows/samples) | Wähle in der Dropdownliste mit den Kategorien die Option **Dateien, Ordner und Bibliotheken** aus. |
