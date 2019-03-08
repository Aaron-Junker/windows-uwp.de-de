---
title: Schneller Zugriff auf Dateieigenschaften in UWP
description: Stellen Sie schnell eine Liste von Dateien und ihren Eigenschaften über eine Bibliothek in einer UWP-App zusammen.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, Uwp, Datei, Eigenschaften
ms.localizationpriority: medium
ms.openlocfilehash: 5ae884ca5424f50a7a835bc55602b5aa7c54096d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630235"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>Schneller Zugriff auf Dateieigenschaften in UWP 

Erfahren Sie, wie Sie schnell eine Liste von Dateien und ihren Eigenschaften aus einer Bibliothek zusammenstellen und diese Eigenschaften in einer App verwenden können.  

Voraussetzungen 
- **Asynchrone Programmierung für apps der universellen Windows-Plattform (UWP)**   erfahren Sie, wie asynchrone Schreiben von apps in C# oder Visual Basic finden Sie unter [Aufrufen von asynchronen APIs in C# oder Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic).     Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps). 
- **Zugriffsberechtigungen für Bibliotheken**   für den Code in diesen Beispielen ist das **PicturesLibrary** -Funktion, aber den Dateispeicherort möglicherweise eine Funktion oder keine Funktion überhaupt. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). 
- **Einfache Enumeration**    dieses Beispiel verwendet ["queryoptions"](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) einige erweiterte enumerationseigenschaften festlegen. Um mehr darüber zu erfahren, wie Sie eine einfache Liste von Dateien für ein kleineres Verzeichnis erhalten, lesen Sie [Aufzählen und Abfragen von Dateien und Ordnern](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders). 

## <a name="usage"></a>Verwendungszweck  
Viele Apps müssen die Eigenschaften einer Gruppe von Dateien auflisten, müssen aber nicht immer direkt mit den Dateien interagieren. Zum Beispiel, eine Musikanwendung spielt immer nur eine Datei ab (Öffnen), aber sie benötigt die Eigenschaften aller Dateien in einem Ordner, so dass die Anwendung die Song-Warteschlange anzeigen kann. So kann der Benutzer eine gültige Datei zum Abspielen auswählen. 

Die Beispiele auf dieser Seite sollten nicht in Apps verwendet werden, die die Metadaten jeder Datei modifizieren, oder in Apps, die mit allen resultierenden StorageFiles interagieren (über das Lesen ihrer Eigenschaften hinaus). Weitere Informationen finden Sie unter [Aufzählung und Abfrage von Dateien und Ordnern](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders). 

## <a name="enumerate-all-the-pictures-in-a-location"></a>Aufzählung aller Bilder eines Ortes 
In diesem Beispiel führen Sie folgendes aus:
-  Erstellen Sie ein [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions)-Objekt, um anzugeben, dass das Programm die Dateien so schnell wie möglich aufzählen möchte.
-  Holen Sie Dateieigenschaften durch Auslagern von StorageFile-Objekten in das Programm. Durch das Auslagern der Dateien wird der Speicherverbrauch des Programms reduziert und die Reaktion verbessert.

### <a name="creating-the-query"></a>Erstellen der Abfrage 
Um die Abfrage zu erstellen, verwenden wir ein QueryOptions-Objekt, um zu spezifizieren, dass die App daran interessiert ist, nur bestimmte Typen von Bilddateien aufzulisten und mit Windows Information Protection (System.Security.EncryptionOwners) geschützte Dateien herauszufiltern. 

Es ist wichtig, die Eigenschaften einzustellen, auf die die Anwendung über [QueryOptions.SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) zugreifen soll. Wenn die App auf eine Eigenschaft zugreift, die nicht vorgeladen ist, wird sie eine erhebliche Leistungseinbuße erleiden.

[OnlyUseIndexerAndOptimzeForIndexedProperties](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.IndexerOption) weist das System an, die Ergebnisse so schnell wie möglich zurückzuliefern, aber nur die in [SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) angegebenen Eigenschaften zu berücksichtigen. 

### <a name="paging-in-the-results"></a>Paging in den Ergebnissen 
Benutzer können Tausende oder Millionen von Dateien in ihrer Bilderbibliothek haben, so dass der Aufruf von [GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) ihren Rechner überfordern würde, weil es eine StorageFile für jedes Bild erstellt. Dies kann dadurch gelöst werden, dass eine feste Anzahl von StorageFiles auf einmal erstellt wird, diese in das UI verarbeitet werden und der Speicher freigegeben wird. 

In unserem Beispiel tun wir dies, indem wir [StorageFileQueryResult.GetFilesAsync(UInt32 StartIndex, UInt32 maxNumberOfItems)](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) verwenden, um jeweils nur 100 Dateien gleichzeitig zu holen. Das Programm verarbeitet dann die Dateien und erlaubt dem Betriebssystem, den Speicher wieder freizugeben. Diese Technik begrenzen den maximalen Speicher der App und stellt sicher, dass das System reaktionsschnell bleibt. Natürlich müssen Sie die Anzahl der zurückgesendeten Dateien für Ihr Szenario anpassen. Aber um ein reaktionsschnelles Erlebnis für alle Benutzer zu gewährleisten, ist es empfehlenswert, nicht mehr als 500 Dateien auf einmal zu holen.


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
Die resultierenden StorageFile-Dateien enthalten nur die angeforderten Eigenschaften, werden aber 10-mal schneller zurückgegeben als die anderen IndexerOption. Die Anwendung kann weiterhin Zugriff auf Eigenschaften beantragen, die nicht bereits in der Abfrage enthalten sind, aber es gibt eine Leistungseinbuße, um die Datei zu öffnen und diese Eigenschaften abzurufen.  

## <a name="adding-folders-to-libraries"></a>Hinzufügen von Ordnern zu Libraries 
Apps können den Benutzer mittels [StorageLibrary.RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync) auffordern, den Speicherort zum Index hinzuzufügen. Sobald der Speicherort einmal enthalten ist, wird er automatisch indiziert und Apps können diese Technik verwenden, um die Dateien aufzuzählen.
 
## <a name="see-also"></a>Siehe auch
["Queryoptions" API-Referenz](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions)  
[Aufzählen und Abfragen von Dateien und Ordner](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)  
[Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)  
 
 
