---
title: Arbeiten mit Dateien
description: Hier erfahren Sie, wie Sie in der Universellen Windows-Plattform mit Dateien arbeiten.
ms.date: 05/01/2018
ms.topic: article
keywords: Erste Schritte, UWP, Windows 10, Lernpfad, Dateien, Datei-E/A, Datei lesen, Datei schreiben, Datei erstellen, Text schreiben, Text lesen
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5480638e201dca8a5eb5363d7a5944422c626f67
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366889"
---
# <a name="work-with-files"></a>Arbeiten mit Dateien

In diesem Thema erfahren Sie alles, was Sie wissen müssen, um Dateien aus einer UWP-App (Universelle Windows-Plattform zu lesen und in eine solche App zu schreiben. Es werden die wichtigsten APIs und Typen vorgestellt, und Sie finden hier Links, die Ihnen dabei helfen, noch mehr zu erfahren.

Dies ist kein Lernprogramm. Wenn Sie ein Lernprogramm wünschen, lesen Sie [Erstellen, Schreiben und Lesen einer Datei](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files). Hier wird gezeigt, wie Sie eine Datei erstellen, lesen und schreiben und wie Sie Puffer und Streams verwenden. Unter Umständen sind Sie auch am [Beispiel zum Dateizugriff](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) interessiert. Hier wird gezeigt, wie Sie eine Datei erstellen, lesen, schreiben, kopieren und löschen, wie Sie Dateieigenschaften abrufen und wie Sie sich eine Datei oder einen Ordner merken, damit Ihre App problemlos erneut darauf zugreifen kann.

Wir schauen uns den Code zum Schreiben und Lesen von Text aus einer Datei an. Ferner besprechen wir, wie Sie auf die lokalen, Roaming- und temporären Ordner der App zugreifen.

## <a name="what-do-you-need-to-know"></a>Wissenswertes

Dies sind die wichtigsten Elemente, die Sie kennen müssen, um Text aus einer Datei zu lesen bzw. in eine Datei zu schreiben:

- [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) stellt eine Datei dar. Diese Klasse enthält Eigenschaften, die Informationen über die Datei bieten, sowie Methoden zum Erstellen, Öffnen, Kopieren, Löschen und Umbenennen von Dateien.
Unter Umständen sind Sie mit dem Umgang mit Zeichenfolgenpfaden vertraut. Es gibt einige UWP-APIs, die einen Zeichenfolgenpfad verwenden, öfter aber verwenden Sie eine **StorageFile** (Speicherdatei) zur Darstellung einer Datei, da einige Dateien, mit denen Sie in UWP arbeiten, keinen Pfad bzw. möglicherweise einen eher umständlichen Pfad haben. Verwenden Sie [StorageFile.GetFileFromPathAsync()](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync), um einen Zeichenfolgenpfad in eine **StorageFile** zu konvertieren. 

- Die [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio)-Klasse bietet eine einfache Möglichkeit zum Lesen und Schreiben von Text. Diese Klasse kann auch einen Array von Bytes oder den Inhalt eines Puffers lesen/schreiben. Diese Klasse ist der [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio)-Klasse sehr ähnlich. Der Hauptunterschied besteht darin, dass anstelle eines Zeichenfolgenpfads wie bei **PathIO** eine **StorageFile** verwendet wird.
- [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) steht für einen Ordner (Verzeichnis). Diese Klasse verfügt über Methoden zum Erstellen von Dateien, zum Abfragen des Inhalts eines Ordners, zum Erstellen, Umbenennen und Löschen von Ordnern sowie Eigenschaften, die Informationen zu einem Ordner bereitstellen. 

Dies sind gängige Methoden zum Abrufen eines **StorageFolder**:

- [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) – Erlaubt es dem Benutzer, zu dem Ordner zu navigieren, den er verwenden möchte.
- [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) – Stellt den **StorageFolder** speziell für einen der Ordner bereit, die für die App lokal vorliegen, wie der lokale, Roaming- und temporäre Ordner.
- [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) – Stellt den **StorageFolder** für bekannte Bibliotheken wie Musik- oder Bildbibliotheken bereit.

## <a name="write-text-to-a-file"></a>Schreiben von Text in eine Datei

 In dieser Einführung konzentrieren wir uns auf ein einfaches Szenario: das Lesen und Schreiben von Text. Betrachten wir zunächst Code, der die **FileIO**-Klasse verwendet, um Text in eine Datei zu schreiben.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

Zunächst ermitteln wir, wo die Datei gespeichert werden soll. `Windows.Storage.ApplicationData.Current.LocalFolder` bietet Zugriff auf den Ordner für lokale Daten, die für Ihre app erstellt wird, wenn sie installiert ist. Unter [Zugreifen auf das Dateisystem](#access-the-file-system) finden Sie ausführliche Informationen zu den Ordnern, auf die Ihre App zugreifen kann.

Anschließend verwenden wir **StorageFolder**, um die Datei zu erstellen (oder zu öffnen, wenn sie bereits vorhanden ist).

Die **FileIO**-Klasse bietet eine bequeme Möglichkeit, um Text in die Datei zu schreiben. `FileIO.WriteTextAsync()` ersetzt den gesamten Inhalt der Datei mit dem angegebenen Text. `FileIO.AppendLinesAsync()` Fügt eine Auflistung von Zeichenfolgen in die Datei – eine Zeichenfolge pro Zeile schreiben.

## <a name="read-text-from-a-file"></a>Lesen von Text aus einer Datei

Wie beim Schreiben einer Datei beginnt das Lesen einer Datei durch die Angabe, wo sich die Datei befindet. Wir verwenden den gleichen Speicherort wie im obigen Beispiel. Anschließend wir verwenden die **FileIO** Klasse, um ihren Inhalt zu lesen.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Sie können auch jede Zeile der Datei in den einzelnen Zeichenfolgen in einer Sammlung mit lesen. `IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);`

## <a name="access-the-file-system"></a>Zugreifen auf das Dateisystem

In der UWP-Plattform ist der Ordnerzugriff eingeschränkt, um die Integrität und Vertraulichkeit der Daten des Benutzers sicherzustellen. 

### <a name="app-folders"></a>App-Ordner

Wenn eine UWP-app installiert ist, werden mehrere Ordner unter c:\users erstellt\<Benutzername > \AppData\Local\Packages\<app-Paket-ID > \, unter anderem der app lokale, roaming und temporäre Dateien zu speichern. Die App muss keine Funktionen zum Zugreifen auf diese Ordner deklarieren, und diese Ordner sind auch nicht von anderen Apps zugänglich. Diese Ordner werden ebenfalls entfernt, wenn die App deinstalliert wird.

Dies sind einige der App-Ordner, die Sie häufig verwenden:

- **LocalState**: Für Daten, die lokal auf dem aktuellen Gerät. Wenn das Gerät gesichert wird, werden die Daten in diesem Verzeichnis in einem Sicherungsabbild in OneDrive gespeichert. Wenn der Benutzer das Gerät zurücksetzt oder austauscht, werden die Daten wiederhergestellt. Greifen Sie auf diesen Ordner zu über `Windows.Storage.ApplicationData.Current.LocalFolder.` Speichern Sie lokale Daten, die nicht in OneDrive gesichert werden sollen, im Ordner **LocalCacheFolder**, auf den Sie mit `Windows.Storage.ApplicationData.Current.LocalCacheFolder` zugreifen können.

- **RoamingState**: Für Daten, die auf allen Geräten repliziert werden soll, in dem die app installiert ist. Windows schränkt die Menge der Daten für das Roaming ein, speichern Sie hier also nur Benutzereinstellungen und kleine Dateien. Greifen Sie auf den Roamingordner mit `Windows.Storage.ApplicationData.Current.RoamingFolder` zu.

- **TempState**: Für Daten, die gelöscht werden können, wenn die app nicht ausgeführt wird. Greifen Sie auf diesen Ordner mit `Windows.Storage.ApplicationData.Current.TemporaryFolder` zu.

### <a name="access-the-rest-of-the-file-system"></a>Zugreifen auf das restliche Dateisystem

Eine UWP-App muss ihre Absicht deklarieren, auf eine bestimmte Benutzerbibliothek zuzugreifen, indem sie die entsprechende Funktion zu ihrem Manifest hinzufügt. Der Benutzer erhält bei Installation der App dann eine Aufforderung, um sicherzustellen, dass der Zugriff auf die angegebene Bibliothek autorisiert ist. Ist dies nicht der Fall, wird die App nicht installiert. Es gibt Funktionen für den Zugriff auf die Bilder, Videos und Musikbibliotheken. Eine vollständige Liste finden Sie unter [Deklaration der App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations). Zum Abrufen eines **StorageFolder** für diese Bibliotheken verwenden Sie die Klasse [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders).

#### <a name="documents-library"></a>Dokumentbibliothek

Es gibt zwar eine Funktion für den Zugriff auf die Dokumentbibliothek des Benutzers, diese Funktion ist aber eingeschränkt, das heißt, dass eine App, die sie deklariert, vom Microsoft Store zurückgewiesen wird, es sei denn Sie folgen einem Prozess, um eine spezielle Genehmigung einzuholen. Sie ist nicht für die allgemeine Verwendung vorgesehen. Verwenden Sie stattdessen die Datei- oder Ordnerauswahl (siehe [Öffnen von Dateien und Ordnern mit einer Auswahl](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) und [Speichern einer Datei mit einer Auswahl](https://docs.microsoft.com/windows/uwp/files/quickstart-save-a-file-with-a-picker)), die es dem Benutzer ermöglicht, zu dem Ordner oder der Datei zu navigieren. Wenn der Benutzer zu einem Ordner oder einer Datei navigiert, hat er implizit die Berechtigung für den Zugriff auf die App, und das System lässt den Zugriff zu.

#### <a name="general-access"></a>Allgemeiner Zugriff

Alternativ kann Ihre App auch die eingeschränkte [broadFileSystem](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)-Funktion im Manifest deklarieren, die ebenfalls eine Microsoft Store-Genehmigung erfordert. Die App kann dann auf jede Datei zugreifen, auf die der Benutzer Zugriff hat, ohne dass das Eingreifen einer Datei- oder Ordnerauswahl erforderlich ist.

Eine umfassende Liste der Speicherorte, auf die Apps zugreifen können, finden Sie unter [Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

## <a name="useful-apis-and-docs"></a>Nützliche APIs und Dokumente

Nachfolgend finden Sie einen kurzen Überblick über die APIs und andere nützliche Dokumentation, die Ihnen beim Arbeiten mit Dateien und Ordnern helfen.

### <a name="useful-apis"></a>Nützliche APIs

| API | Beschreibung |
|------|---------------|
|  [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) | Enthält Informationen über die Datei sowie Methoden zum Erstellen, Öffnen, Kopieren, Löschen und Umbenennen von Dateien. |
| [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) | Enthält Informationen zu dem Ordner, Methoden zum Erstellen von Dateien und Methoden zum Erstellen, Umbenennen und Löschen von Ordnern. |
| [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) |  Bietet eine einfache Möglichkeit zum Lesen und Schreiben von Text. Diese Klasse kann auch einen Array von Bytes oder den Inhalt eines Puffers lesen/schreiben. |
| [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) | Bietet eine einfache Möglichkeit zum Lesen/Schreiben von Text von/aus einer Datei bei einem Zeichenfolgenpfad für die Datei. Diese Klasse kann auch einen Array von Bytes oder den Inhalt eines Puffers lesen/schreiben. |
| [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) & [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) |  Zum Lesen und Schreiben von Puffern, Bytes, ganzen Zahlen, GUIDs, Zeitspannen usw. aus/in einen Stream. |
| [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) | Bietet Zugriff auf die Ordner, die für die App erstellt werden, z. B. den lokalen Ordner, Roamingordner und temporären Ordner. |
| [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) |  Ermöglicht es dem Benutzer, einen Ordner auszuwählen, und gibt dafür einen **StorageFolder** zurück. So erhalten Sie Zugriff auf Speicherorte, auf die die App standardmäßig nicht zugreifen kann. |
| [Windows.Storage.Pickers.FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker) | Ermöglicht es dem Benutzer, eine Datei auszuwählen und zu öffnen; gibt dafür eine **StorageFile** zurück. So erhalten Sie Zugriff auf eine Datei, auf die die App standardmäßig nicht zugreifen kann. |
| [Windows.Storage.Pickers.FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) | Ermöglicht es dem Benutzer, den Dateinamen, die Erweiterung und den Speicherort für eine Datei auszuwählen. Gibt eine **StorageFile**  zurück. So speichern Sie eine Datei an einem Speicherort, auf den die App nicht standardmäßig zugreifen kann. |
|  [Windows.Storage.Streams-namespace](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Behandelt das Lesen und Schreiben von Streams. Schauen Sie sich insbesondere die Klassen [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) und [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) an, die Puffer, Bytes, ganze Zahlen, GUIDs, Zeitspannen usw. lesen und schreiben. |

### <a name="useful-docs"></a>Nützliche Dokumente

| Thema | Beschreibung |
|-------|----------------|
| [Windows.Storage-Namespace](https://docs.microsoft.com/uwp/api/windows.storage) | API-Referenzdokumente |
| [Dateien, Ordner und Bibliotheken](https://docs.microsoft.com/windows/uwp/files/) | Konzeptionelle Dokumente |
| [Erstellen, Schreiben und Lesen einer Datei](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) | Erläutert das Erstellen, Lesen und Schreiben von Text, binären Daten und Streams. |
| [Erste Schritte das Speichern von app-Daten lokal](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) | Zusätzlich zu den bewährte Methoden zum Speichern lokaler Daten wird hier der Zweck der Ordner LocalSettings und LocalCache erörtert. |
| [Erste Schritte mit App-Datenroaming](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) | Eine zweiteilige Serie über die Verwendung von Roaming-App-Daten. |
| [Richtlinien für das roaming von Anwendungsdaten](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Befolgen Sie diese Richtlinien für das Daten-Roaming, wenn Sie Ihre App entwerfen. |
| [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Bietet eine Übersicht über die verschiedenen App-Datenspeicher wie z. B. die lokalen, Roaming- und temporären Ordner. Unter [Roamingdaten](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#roaming-data) finden Sie Richtlinien und weitere Informationen zum Schreiben von Daten, die via Roaming zwischen Geräten übertragen werden. |
| [Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) | Informationen darüber, auf welche Dateisystemspeicherorte Ihre App zugreifen kann. |
| [Öffnen von Dateien und Ordnern mit einer Auswahl](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) | Zeigt, wie Sie auf Dateien und Ordner zugreifen, indem Sie Benutzer über eine Auswahl-UI entscheiden lassen. |
| [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Typen, die zum Lesen und Schreiben von Streams verwendet werden. |
| [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries) | Erörtert, wie Sie Ordner aus Bibliotheken entfernen, die Liste der Ordner in einer Bibliothek abrufen und gespeicherte Fotos, Musik und Videos untersuchen. |

## <a name="useful-code-samples"></a>Nützliche Codebeispiele

| Codebeispiel | Beschreibung |
|-----------------|---------------|
| [Anwendungsbeispiel für Daten](https://code.msdn.microsoft.com/windowsapps/ApplicationData-sample-fb043eb2) | Zeigt, wie Sie die Anwendungsdaten-APIs verwenden können, um spezifische Daten für jeden Benutzer zu speichern und abzurufen. |
| [Beispiel zum Dateizugriff](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | Veranschaulicht das Erstellen, Lesen, Schreiben, Kopieren und Löschen einer Datei. |
| [Beispiel zur Dateiauswahl](https://code.msdn.microsoft.com/windowsapps/File-picker-sample-9f294cba) | Zeigt, wie auf Dateien und Ordner zugegriffen werden kann, indem der Benutzer diese über die Benutzeroberfläche auswählen darf, und wie eine Datei so gespeichert wird, dass der Benutzer den Namen, Dateityp und Speicherort der Datei angeben kann. |
| [JSON-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) | Veranschaulicht das Codieren und Decodieren von JSON-Objekten (JavaScript Object Notation), Arrays, Zeichenfolgen, Zahlen und booleschen Werten mit dem [Windows.Data.Json-Namespace](https://docs.microsoft.com/uwp/api/Windows.Data.Json). |
| [Weitere Codebeispiele](https://developer.microsoft.com//windows/samples) | Wählen Sie **Dateien, Ordner und Bibliotheken** in der Dropdownliste mit den Kategorien. |
