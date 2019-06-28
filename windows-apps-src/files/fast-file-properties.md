---
title: Schneller Zugriff auf Dateieigenschaften in UWP
description: Stellen Sie schnell eine Liste von Dateien und ihren Eigenschaften über eine Bibliothek in einer UWP-App zusammen.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP, Datei, Eigenschaften
ms.localizationpriority: medium
ms.openlocfilehash: 5ae884ca5424f50a7a835bc55602b5aa7c54096d
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "63799613"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>Schneller Zugriff auf Dateieigenschaften in UWP 

Hier erfährst du, wie du schnell eine Liste von Dateien und ihren Eigenschaften aus einer Bibliothek zusammenstellen und diese Eigenschaften in einer App verwenden kannst.  

Voraussetzungen 
- **Asynchrone Programmierung für UWP-Apps (Universelle Windows-Plattform):**  Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic findest du unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps). 
- **Zugriffsberechtigungen für Bibliotheken:**  Der Code in diesen Beispielen erfordert beispielsweise den Zugriff auf die **picturesLibrary**-Funktion, während dein Dateispeicherort einen anderen Zugriffstyp oder keinen Zugriff voraussetzt. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). 
- **Einfache Dateiauflistung:**   In diesem Beispiel werden mit [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) einige erweiterte Auflistungseigenschaften festgelegt. Weitere Informationen dazu, wie du eine einfache Liste von Dateien für ein kleineres Verzeichnis erhältst, findest du unter [Aufzählen und Abfragen von Dateien und Ordnern](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders). 

## <a name="usage"></a>Verwendungszweck  
Viele Apps müssen die Eigenschaften einer Gruppe von Dateien auflisten, müssen aber nicht immer direkt mit den Dateien interagieren. Beispiel: Eine Musik-App spielt (öffnet) immer nur eine Datei ab, sie benötigt aber die Eigenschaften aller Dateien in einem Ordner, damit sie die Songwarteschlange anzeigen oder damit der Benutzer eine gültige Datei für die Wiedergabe auswählen kann. 

Die Beispiele auf dieser Seite sollten nicht in Apps verwendet werden, die die Metadaten jeder Datei modifizieren, oder in Apps, die mit allen resultierenden StorageFiles interagieren (über das Lesen ihrer Eigenschaften hinaus). Weitere Informationen findest du unter [Aufzählen und Abfragen von Dateien und Ordnern](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders). 

## <a name="enumerate-all-the-pictures-in-a-location"></a>Aufzählen aller Bilder an einem Speicherort 
In diesem Beispiel werden die folgenden Schritte ausgeführt:
-  Erstellen eines [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions)-Objekts, um anzugeben, dass die App die Dateien so schnell wie möglich auflisten soll
-  Abrufen von Dateieigenschaften durch Auslagern von StorageFile-Objekten in die App. Durch das Auslagern der Dateien wird der von der App belegte Arbeitsspeicher reduziert und die Reaktion verbessert.

### <a name="creating-the-query"></a>Erstellen der Abfrage 
Zum Erstellen der Abfrage verwenden wir ein QueryOptions-Objekt, um anzugeben, dass die App nur an der Auflistung bestimmter Bilddateitypen interessiert ist, und um Dateien herauszufiltern, die mit Windows Information Protection (System.Security.EncryptionOwners) geschützt sind. 

Es ist wichtig, mit [QueryOptions.SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) die Eigenschaften festzulegen, auf die die App zugreifen wird. Wenn die App auf eine Eigenschaft zugreift, die nicht vorab abgerufen wird, führt dies zu erheblichen Leistungseinbußen.

[OnlyUseIndexerAndOptimzeForIndexedProperties](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.IndexerOption) weist das System an, die Ergebnisse so schnell wie möglich zurückzugeben, aber nur die in [SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) angegebenen Eigenschaften zu berücksichtigen. 

### <a name="paging-in-the-results"></a>Auslagern in den Ergebnissen 
Die Bilderbibliothek von Benutzern kann Tausende oder Millionen von Dateien enthalten. Der Aufruf von [GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) würde daher ihren Rechner überfordern, da für jedes Bild ein StorageFile-Objekt erstellt wird. Dieses Problem kann dadurch behoben werden, dass eine feste Anzahl von StorageFiles-Elementen auf einmal erstellt wird, diese im Kontext der Benutzeroberfläche verarbeitet werden und der Arbeitsspeicher freigegeben wird. 

In unserem Beispiel verwenden wir dazu [StorageFileQueryResult.GetFilesAsync(UInt32 StartIndex, UInt32 maxNumberOfItems)](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync), um jeweils nur 100 Dateien gleichzeitig abzurufen. Die App verarbeitet dann die Dateien und erlaubt dem Betriebssystem, den Arbeitsspeicher anschließend wieder freizugeben. Mit dieser Methode kann der von der App verwendete maximale Arbeitsspeicher begrenzt und sichergestellt werden, dass das System reaktionsschnell bleibt. Natürlich musst du die Anzahl der zurückgesendeten Dateien für dein Szenario anpassen. Aber um eine schnelle Reaktionszeit für alle Benutzer zu gewährleisten, ist es empfehlenswert, nicht mehr als 500 Dateien auf einmal abzurufen.


**Beispiel**  
```csharp
StorageFolder folderToEnumerate = KnownFolders.PicturesLibrary; 
// Check if the folder is indexed before doing anything. 
IndexedState folderIndexedState = await folderToEnumerate.GetIndexedStateAsync(); 
if (folderIndexedState == IndexedState.NotIndexed || folderIndexedState == IndexedState.Unknown) 
{ 
    // Only possible in indexed directories.  
    return; 
} 
 
QueryOptions picturesQuery = new QueryOptions() 
{ 
    FolderDepth = FolderDepth.Deep, 
    // Filter out all files that have WIP enabled
    ApplicationSearchFilter = "System.Security.EncryptionOwners:[]", 
    IndexerOption = IndexerOption.OnlyUseIndexerAndOptimizeForIndexedProperties 
}; 

picturesQuery.FileTypeFilter.Add(".jpg"); 
string[] otherProperties = new string[] 
{ 
    SystemProperties.GPS.LatitudeDecimal, 
    SystemProperties.GPS.LongitudeDecimal 
}; 
 
picturesQuery.SetPropertyPrefetch(PropertyPrefetchOptions.BasicProperties | PropertyPrefetchOptions.ImageProperties, 
                                    otherProperties); 
SortEntry sortOrder = new SortEntry() 
{ 
    AscendingOrder = true, 
    PropertyName = "System.FileName" // FileName property is used as an example. Any property can be used here.  
}; 
picturesQuery.SortOrder.Add(sortOrder); 
 
// Create the query and get the results 
uint index = 0; 
const uint stepSize = 100; 
if (!folderToEnumerate.AreQueryOptionsSupported(picturesQuery)) 
{ 
    log("Querying for a sort order is not supported in this location"); 
    picturesQuery.SortOrder.Clear(); 
} 
StorageFileQueryResult queryResult = folderToEnumerate.CreateFileQueryWithOptions(picturesQuery); 
IReadOnlyList<StorageFile> images = await queryResult.GetFilesAsync(index, stepSize); 
while (images.Count != 0 || index < 10000) 
{ 
    foreach (StorageFile file in images) 
    { 
        // With the OnlyUseIndexerAndOptimizeForIndexedProperties set, this won't  
        // be async. It will run synchronously. 
        var imageProps = await file.Properties.GetImagePropertiesAsync(); 
 
        // Build the UI 
        log(String.Format("{0} at {1}, {2}", 
                    file.Path, 
                    imageProps.Latitude, 
                    imageProps.Longitude)); 
    } 
    index += stepSize; 
    images = await queryResult.GetFilesAsync(index, stepSize); 
} 
```

### <a name="results"></a>Ergebnisse 
Die resultierenden StorageFile-Dateien enthalten nur die angeforderten Eigenschaften, werden aber zehnmal schneller zurückgegeben als die anderen IndexerOptions-Elemente. Die App kann weiterhin Zugriff auf Eigenschaften anfordern, die nicht bereits in der Abfrage enthalten sind. Dies führt beim Öffnen der Datei und Abrufen dieser Eigenschaften jedoch zu Leistungseinbußen.  

## <a name="adding-folders-to-libraries"></a>Hinzufügen von Ordnern zu Bibliotheken 
Apps können den Benutzer mittels [StorageLibrary.RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync) auffordern, den Speicherort zum Index hinzuzufügen. Ist der Speicherort erst einmal enthalten, wird er automatisch indiziert, und Apps können diese Technik zum Auflisten der Dateien verwenden.
 
## <a name="see-also"></a>Weitere Informationen
[QueryOptions-API-Referenz](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions)  
[Aufzählen und Abfragen von Dateien und Ordnern](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)  
[Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)  
 
 
