---
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: Dateien, Ordner und Bibliotheken
description: Hier erhalten Sie Informationen zum Lesen und Schreiben von App-Einstellungen, zur Datei- und Ordnerauswahl und zu speziellen Speicherorten im Sandkasten, wie z. B. Video-/Musikbibliothek.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 15e6ebd45d4f7069e09169a437476b0f785ea959
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259598"
---
 # <a name="files-folders-and-libraries"></a>Dateien, Ordner und Bibliotheken


Verwenden Sie die APIs im [Windows.Storage](https://docs.microsoft.com/uwp/api/Windows.Storage), [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) und [Windows.Storage.Pickers](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers)-Namespace, um Text und andere Datenformate in Dateien zu lesen und zu schreiben sowie Dateien und Ordner zu verwalten. In diesem Abschnitt erhalten Sie Informationen zum Lesen und Schreiben von App-Einstellungen, zur Datei- und Ordnerauswahl und zu speziellen Speicherorten im Sandkasten, z. B. die Video-/Musikbibliothek.

| Thema | Beschreibung  |
|-------|--------------|
| [Aufzählen und Abfragen von Dateien und Ordnern](quickstart-listing-files-and-folders.md) | Greifen Sie auf Dateien und Ordner zu, die sich in einem Ordner, in einer Bibliothek, auf einem Gerät oder an einer Netzwerkadresse befinden. Sie können auch durch Erstellen von Datei- und Ordnerabfragen Dateien und Ordner an bestimmten Speicherorten abrufen. |
| [Erstellen, Schreiben und Lesen einer Datei](quickstart-reading-and-writing-files.md) | Lesen und Schreiben Sie eine Datei mithilfe eines [StorageFile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekts. |
| [Bewährte Methoden zum Schreiben in Dateien](best-practices-for-writing-to-files.md) | Hier finden Sie bewährte Methoden für verschiedene Dateischreibvorgänge mit den Klassen [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) und [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio). |
| [Abrufen von Dateieigenschaften](quickstart-getting-file-properties.md) | Es werden Eigenschaften – oberste Ebene, grundlegend und erweitert – für eine Datei abgerufen, die durch ein [StorageFile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt dargestellt wird. |
| [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md) | Greifen Sie auf Dateien und Ordner zu, indem Sie Benutzern die Interaktion mit einer Auswahl ermöglichen. Sie können über [FolderPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker) auf einen Ordner zugreifen. |
| [Speichern einer Datei mit einer Auswahl](quickstart-save-a-file-with-a-picker.md) | Mithilfe von [FileSavePicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) können Benutzer den Namen und Speicherort zum Speichern einer Datei durch die App angeben. |
| [Zugriff auf Inhalte in der Heimnetzgruppe](quickstart-accessing-homegroup-content.md) | Greifen Sie auf Inhalte zu, die sich im Heimnetzgruppenordner des Benutzers befinden, einschließlich Bildern, Musik und Videos. |
| [Ermitteln der Verfügbarkeit von Microsoft OneDrive-Dateien](quickstart-determining-availability-of-microsoft-onedrive-files.md) | Ermitteln Sie mithilfe der [StorageFile.IsAvailable](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)-Eigenschaft, ob eine Microsoft OneDrive-Datei verfügbar ist. |
| [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | Fügen Sie vorhandene Musik-, Bilder- oder Video-Ordner den entsprechenden Bibliotheken hinzu. Sie können auch Ordner aus Bibliotheken entfernen, die Liste der Ordner in einer Bibliothek abrufen und gespeicherte Fotos, Musik und Videos untersuchen. |
| [Nachverfolgen kürzlich verwendeter Dateien und Ordner](how-to-track-recently-used-files-and-folders.md) | Sie können Dateien nachverfolgen, auf die häufig zugegriffen wird, indem Sie sie der Liste mit den zuletzt verwendeten Elementen (MRU) der App hinzufügen. Die Plattform verwaltet die MRU für Sie. Dabei werden Elemente nach der Zeit und dem Ort des letzten Zugriffs sortiert und die ältesten Elemente entfernt, wenn das Limit von 25 Elementen erreicht ist. Alle Apps besitzen eine eigene MRU. |
| [Nachverfolgen von Dateisystemänderungen im Hintergrund](change-tracking-filesystem.md) | Verfolgen Sie Änderungen am Dateisystem nach, selbst wenn die App nicht ausgeführt wird.|
| [Zugreifen auf die SD-Karte](access-the-sd-card.md) | Sie können weniger wichtige Daten auf einer optionalen microSD-Karte speichern und auf diese zugreifen. Dies gilt besonders für kostengünstige Geräte, die nur über einen begrenzten internen Speicher verfügen. |
| [Berechtigungen für den Dateizugriff](file-access-permissions.md) | Apps können standardmäßig auf bestimmte Dateisystemspeicherorte zugreifen. Apps können darüber hinaus mithilfe der Dateiauswahl oder über die Deklaration von Funktionen auf weitere Speicherorte zugreifen. |
| [Schneller Zugriff auf Dateieigenschaften in UWP](fast-file-properties.md) | Stellen Sie schnell eine Liste von Dateien und ihren Eigenschaften über eine Bibliothek in einer UWP-App zusammen. |

## <a name="related-samples"></a>Verwandte Beispiele
[Beispiel für Ordnerenumeration](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FolderEnumeration)

[Beispiel zum Dateizugriff](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)

[Beispiel zur Dateiauswahl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker)
 

 
