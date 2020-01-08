---
ms.assetid: 40122343-1FE3-4160-BABE-6A2DD9AF1E8E
title: Optimieren des Dateizugriffs
description: Erstellen Sie UWP-Apps (Universelle Windows-Plattform), die effizient auf das Dateisystem zugreifen und dadurch Leistungsprobleme aufgrund von Datenträgerlatenz und Arbeitsspeicher-/CPU-Zyklen vermeiden.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3114bc7a86f7f7f4d22c69c814735c146352efbd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681951"
---
# <a name="optimize-file-access"></a>Optimieren des Dateizugriffs


Erstellen Sie UWP-Apps (Universelle Windows-Plattform), die effizient auf das Dateisystem zugreifen und dadurch Leistungsprobleme aufgrund von Datenträgerlatenz und Arbeitsspeicher-/CPU-Zyklen vermeiden.

Wenn Sie auf eine große Sammlung von Dateien und auf andere Eigenschaftenwerte als die typischen Name-, FileType- und Path-Eigenschaften zugreifen möchten, empfiehlt es sich, hierzu [**QueryOptions**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) zu erstellen und [**SetPropertyPrefetch**](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) aufzurufen. Mit der **SetPropertyPrefetch**-Methode kann die Leistung von Apps wesentlich gesteigert werden, in denen eine Sammlung von aus dem Dateisystem abgerufenen Elementen angezeigt wird (z. B. eine Sammlung von Bildern). In den nächsten Beispielen werden einige Möglichkeiten veranschaulicht, auf mehrere Dateien zuzugreifen.

Im ersten Beispiel werden mithilfe von [**Windows.Storage.StorageFolder.GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) die Namensinformationen für eine Gruppe von Dateien abgerufen. Dieser Ansatz sorgt für eine gute Leistung, da im Beispiel lediglich auf die name-Eigenschaft zugegriffen wird.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> 
> for (int i = 0; i < files.Count; i++)
> {
>     // do something with the name of each file
>     string fileName = files[i].Name;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) =
>     Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> 
> For i As Integer = 0 To files.Count - 1
>     ' do something with the name of each file
>     Dim fileName As String = files(i).Name
> Next i
> ```

Im zweiten Beispiel wird [**Windows.Storage.StorageFolder.GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) verwendet. Anschließend werden die Bildeigenschaften für die einzelnen Dateien abgerufen. Bei dieser Vorgehensweise ist eine schlechte Leistung zu beobachten.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> for (int i = 0; i < files.Count; i++)
> {
>     ImageProperties imgProps = await files[i].Properties.GetImagePropertiesAsync();
> 
>     // do something with the date the image was taken
>     DateTimeOffset date = imgProps.DateTaken;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) = Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> For i As Integer = 0 To files.Count - 1
>     Dim imgProps As ImageProperties =
>         Await files(i).Properties.GetImagePropertiesAsync()
> 
>     ' do something with the date the image was taken
>     Dim dateTaken As DateTimeOffset = imgProps.DateTaken
> Next i
> ```

Im dritten Beispiel werden mit [**QueryOptions**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) Informationen für eine Gruppe von Dateien abgerufen. Diese Vorgehensweise bietet eine wesentlich bessere Leistung als das vorhergehende Beispiel.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> // Set QueryOptions to prefetch our specific properties
> var queryOptions = new Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, null);
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView, 100,
>         ThumbnailOptions.ReturnOnlyIfCached);
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties, 
>        new string[] {"System.Size"});
> 
> StorageFileQueryResult queryResults = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions);
> IReadOnlyList<StorageFile> files = await queryResults.GetFilesAsync();
> 
> foreach (var file in files)
> {
>     ImageProperties imageProperties = await file.Properties.GetImagePropertiesAsync();
> 
>     // Do something with the date the image was taken.
>     DateTimeOffset dateTaken = imageProperties.DateTaken;
> 
>     // Performance gains increase with the number of properties that are accessed.
>     IDictionary<String, object> propertyResults =
>         await file.Properties.RetrievePropertiesAsync(
>               new string[] {"System.Size" });
> 
>     // Get/Set extra properties here
>     var systemSize = propertyResults["System.Size"];
> }
> ```
> ```vb
> ' Set QueryOptions to prefetch our specific properties
> Dim queryOptions = New Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, Nothing)
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView,
>             100, Windows.Storage.FileProperties.ThumbnailOptions.ReturnOnlyIfCached)
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties,
>                                  New String() {"System.Size"})
> 
> Dim queryResults As StorageFileQueryResult = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions)
> Dim files As IReadOnlyList(Of StorageFile) = Await queryResults.GetFilesAsync()
> 
> 
> For Each file In files
>     Dim imageProperties As ImageProperties = Await file.Properties.GetImagePropertiesAsync()
> 
>     ' Do something with the date the image was taken.
>     Dim dateTaken As DateTimeOffset = imageProperties.DateTaken
> 
>     ' Performance gains increase with the number of properties that are accessed.
>     Dim propertyResults As IDictionary(Of String, Object) =
>         Await file.Properties.RetrievePropertiesAsync(New String() {"System.Size"})
> 
>     ' Get/Set extra properties here
>     Dim systemSize = propertyResults("System.Size")
> 
> Next file
> ```
Wenn Sie mehrere Vorgänge an Windows.Storage-Objekten wie `Windows.Storage.ApplicationData.Current.LocalFolder` ausführen, erstellen Sie eine lokale Variable, die auf die Speicherquelle verweist, sodass nicht bei jedem Zugriff erneut Zwischenobjekte erstellt werden müssen.

## <a name="stream-performance-in-c-and-visual-basic"></a>Streamleistung in C# und Visual Basic

### <a name="buffering-between-uwp-and-net-streams"></a>Puffern zwischen UWP- und .NET-Streams

Ein UWP-Stream (wie [**Windows.Storage.Streams.IInputStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IInputStream) oder [**IOutputStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IOutputStream)) kann in zahlreichen Szenarien in .NET-Streams ([**System.IO.Stream**](https://docs.microsoft.com/dotnet/api/system.io.stream)) konvertiert werden. Dies ist beispielsweise hilfreich, wenn Sie eine UWP-App (Universelle Windows-Plattform) erstellen und vorhandenen .NET-Code für Streams im UWP-Dateisystem verwenden möchten. Um dies zu ermöglichen, stellen .NET-APIs für UWP-apps Erweiterungs Methoden bereit, mit denen Sie zwischen .net-und UWP-Streamtypen konvertieren können. Weitere Informationen finden Sie unter [**WindowsRuntimeStreamExtensions**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions).

Beim Konvertieren eines UWP-Streams in einen .NET-Stream wird eigentlich ein Adapter für den zugrundeliegenden UWP-Stream erstellt. Das Aufrufen von Methoden für UWP-Streams ist unter bestimmten Umständen mit einem Laufzeitaufwand verbunden. Dieser kann sich insbesondere in Szenarien mit vielen kleinen und häufig ausgeführten Lese- oder Schreibvorgängen auf die Geschwindigkeit Ihrer App auswirken.

Damit entsprechende Geschwindigkeitseinbußen nach Möglichkeit vermieden werden, verfügen UWP-Streamadapter über einen Datenpuffer. Im folgenden Codebeispiel werden kleine, aufeinander folgende Lesevorgänge unter Verwendung eines UWP-Streamadapters mit Standardpuffergröße veranschaulicht.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFile file = await Windows.Storage.ApplicationData.Current
>     .LocalFolder.GetFileAsync("example.txt");
> Windows.Storage.Streams.IInputStream windowsRuntimeStream = 
>     await file.OpenReadAsync();
> 
> byte[] destinationArray = new byte[8];
> 
> // Create an adapter with the default buffer size.
> using (var managedStream = windowsRuntimeStream.AsStreamForRead())
> {
> 
>     // Read 8 bytes into destinationArray.
>     // A larger block is actually read from the underlying 
>     // windowsRuntimeStream and buffered within the adapter.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> 
>     // Read 8 more bytes into destinationArray.
>     // This call may complete much faster than the first call
>     // because the data is buffered and no call to the 
>     // underlying windowsRuntimeStream needs to be made.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> }
> ```
> ```vb
> Dim file As StorageFile = Await Windows.Storage.ApplicationData.Current -
> .LocalFolder.GetFileAsync("example.txt")
> Dim windowsRuntimeStream As Windows.Storage.Streams.IInputStream =
>     Await file.OpenReadAsync()
> 
> Dim destinationArray() As Byte = New Byte(8) {}
> 
> ' Create an adapter with the default buffer size.
> Dim managedStream As Stream = windowsRuntimeStream.AsStreamForRead()
> Using (managedStream)
> 
>     ' Read 8 bytes into destinationArray.
>     ' A larger block is actually read from the underlying 
>     ' windowsRuntimeStream and buffered within the adapter.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
>     ' Read 8 more bytes into destinationArray.
>     ' This call may complete much faster than the first call
>     ' because the data is buffered and no call to the 
>     ' underlying windowsRuntimeStream needs to be made.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
> End Using
> ```

Das Verhalten des Standardpuffers eignet sich für die meisten Szenarien, in denen ein UWP-Stream in einen .NET-Stream konvertiert werden soll. In einigen Szenarien kann es jedoch wünschenswert sein, das Pufferverhalten zu optimieren, um die Leistung zu erhöhen.

### <a name="working-with-large-data-sets"></a>Arbeiten mit umfangreichen Datensätzen

Beim Lesen oder Schreiben umfangreicher Datensätze können Sie den Durchsatz möglicherweise erhöhen, indem Sie den Puffer für die Erweiterungsmethoden [**AsStreamForRead**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforread?view=dotnet-uwp-10.0), [**AsStreamForWrite**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforwrite?view=dotnet-uwp-10.0) und [**AsStream**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstream?view=dotnet-uwp-10.0) vergrößern. Dadurch enthält der Streamadapter einen größeren internen Puffer. So kann der Parser beim Übergeben eines Streams von einer großen Datei an einen XML-Parser eine Vielzahl von kleinen Lesevorgängen für den Stream ausführen. Große Puffer können dafür sorgen, dass der zugrunde liegende UWP-Stream weniger oft aufgerufen und somit die Leistung gesteigert wird.

> **Beachten** Sie   Sie beim Festlegen einer Puffergröße, die größer als ungefähr 80 KB ist, vorsichtig sein sollten, da dies eine Fragmentierung im Garbage Collector Heap verursachen kann (siehe [verbessern der Garbage Collection Leistung](improve-garbage-collection-performance.md)). Im folgenden Codebeispiel wird ein verwalteter Datenstromadapter mit einem Puffer mit 81.920 Bytes erstellt.

> [!div class="tabbedCodeSnippets"]
```csharp
// Create a stream adapter with an 80 KB buffer.
Stream managedStream = nativeStream.AsStreamForRead(bufferSize: 81920);
```
```vb
' Create a stream adapter with an 80 KB buffer.
Dim managedStream As Stream = nativeStream.AsStreamForRead(bufferSize:=81920)
```

Die [**Stream.CopyTo**](https://docs.microsoft.com/dotnet/api/system.io.stream.copyto)- und die [**CopyToAsync**](https://docs.microsoft.com/dotnet/api/system.io.stream.copytoasync)-Methode weisen darüber hinaus einen lokalen Puffer für das Kopieren zwischen Datenströmen zu. Analog zur [**AsStreamForRead**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforread?view=dotnet-uwp-10.0)-Erweiterungsmethode kann die Leistung beim Kopieren umfangreicher Streams durch Überschreiben der Standardpuffergröße u. U. verbessert werden. Im folgenden Codebeispiel wird das Ändern der Standardpuffergröße eines **CopyToAsync**-Aufrufs veranschaulicht.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> MemoryStream destination = new MemoryStream();
> // copies the buffer into memory using the default copy buffer
> await managedStream.CopyToAsync(destination);
> 
> // Copy the buffer into memory using a 1 MB copy buffer.
> await managedStream.CopyToAsync(destination, bufferSize: 1024 * 1024);
> ```
> ```vb
> Dim destination As MemoryStream = New MemoryStream()
> ' copies the buffer into memory using the default copy buffer
> Await managedStream.CopyToAsync(destination)
> 
> ' Copy the buffer into memory using a 1 MB copy buffer.
> Await managedStream.CopyToAsync(destination, bufferSize:=1024 * 1024)
> ```

Die Puffergröße im Beispiel beträgt 1 MB. Die empfohlene Puffergröße von 80 KB wird somit deutlich übertroffen. Durch die Verwendung eines entsprechend großen Puffers kann der Durchsatz des Kopiervorgangs bei umfangreichen Datensätzen (von mehreren Hundert Megabyte) verbessert werden. Da dieser Puffer jedoch für den Heap für große Objekte zugewiesen wird, kann die Leistung der Garbage Collection dadurch beeinträchtigt werden. Große Puffergrößen sollten daher nur verwendet werden, wenn die Leistung der App dadurch spürbar verbessert werden kann.

Wenn Sie eine große Anzahl von Streams gleichzeitig verwenden, können Sie den Speichermehraufwand des Puffers verringern oder vermeiden. Sie können einen kleineren Puffer angeben oder den *bufferSize*-Parameter auf 0 festlegen, um den Puffer für den betreffenden Streamadapter vollständig zu deaktivieren. Auch ohne Puffer können Sie bei Lese- und Schreibvorgängen für den verwalteten Stream jedoch eine gute Durchsatzleistung erreichen.

### <a name="performing-latency-sensitive-operations"></a>Durchführen von Vorgängen, bei denen Latenz eine Rolle spielt

Das Vermeiden von Pufferungen kann ebenfalls wünschenswert sein, wenn Lese- und Schreibvorgänge mit geringer Latenz ausgeführt werden sollen, ohne große Blöcke aus dem zugrunde liegenden UWP-Stream zu lesen. Beispielsweise eignen sich Lese- und Schreibvorgänge mit geringer Latenz sehr gut für Netzwerkkommunikationsstreams.

In einer Chat-App können Sie Nachrichten mit einem Stream über eine Netzwerkschnittstelle senden und empfangen. Die Nachrichten sollen dabei unmittelbar nach dem Verfassen und nicht erst dann gesendet werden, wenn der Puffer voll ist. Wenn Sie die Puffergröße beim Aufrufen der Methoden [**AsStreamForRead**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforread?view=dotnet-uwp-10.0), [**AsStreamForWrite**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforwrite?view=dotnet-uwp-10.0) und [**AsStream**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstream?view=dotnet-uwp-10.0) auf 0 festlegen, wird vom resultierenden Adapter kein Puffer zugewiesen. Außerdem wird der zugrunde liegende UWP-Datenstrom von allen Aufrufen direkt bearbeitet.


