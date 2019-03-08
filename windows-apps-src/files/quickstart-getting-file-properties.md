---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Abrufen von Dateieigenschaften
description: Abrufen von Eigenschaften &\#8212; auf der obersten Ebene, grundlegende und erweiterte &\#8212; für eine Datei, die durch eine "storagefile" dargestellte Objekt.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6cde9d8753248614603ee49fb1415ec18ec4669b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596995"
---
# <a name="get-file-properties"></a>Abrufen von Dateieigenschaften

**Wichtige APIs**

-   [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)
-   [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770652)

Es werden Eigenschaften – oberste Ebene, grundlegend und erweitert – für eine Datei abgerufen, die durch ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt dargestellt wird.

> [!NOTE]
> Ein vollständiges Beispiel finden Sie unter den [Beispieldatei für Access](https://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Voraussetzungen

-   **Verstehen Sie die asynchrone Programmierung für apps der universellen Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Zugriffsberechtigungen für den Speicherort**

    Der Code in diesen Beispielen erfordert beispielsweise den Zugriff auf die **picturesLibrary**-Funktion, während Ihr Speicherort einen anderen Zugriffstyp oder keinen Zugriff voraussetzt. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Abrufen von Eigenschaften der obersten Ebene einer Datei

Auf viele Dateieigenschaften der obersten Ebene kann in Form von Membern der [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Klasse zugegriffen werden. Diese Eigenschaften enthalten für eine Datei Attribute, Inhaltstyp, Erstellungsdatum, Anzeigename, Dateityp usw.

> [!NOTE]
> Denken Sie daran, die **picturesLibrary**-Funktion zu deklarieren.

In diesem Beispiel werden alle Dateien der Bildbibliothek aufgezählt, wobei für jede Datei auf einige Eigenschaften der obersten Ebene zugegriffen wird.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>Abrufen von grundlegenden Eigenschaften einer Datei

Viele grundlegende Dateieigenschaften werden abgerufen, indem zuerst die [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)-Methode aufgerufen wird. Diese Methode gibt ein [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113)-Objekt zurück, das Eigenschaften für die Größe des Elements (Datei oder Ordner) und das letzte Änderungsdatum des Elements definiert.

In diesem Beispiel werden alle Dateien der Bildbibliothek aufgezählt, wobei für jede Datei auf einige grundlegende Eigenschaften zugegriffen wird.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>Abrufen von erweiterten Eigenschaften einer Datei

Neben den Eigenschaften der obersten Ebene und den grundlegenden Eigenschaften sind dem Inhalt der Datei viele Eigenschaften zugeordnet. Auf diese erweiterten Eigenschaften wird zugegriffen, indem die [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124)-Methode aufgerufen wird. (Ein [ **BasicProperties** ](https://msdn.microsoft.com/library/windows/apps/br212113) -Objekt abgerufen wird, durch den Aufruf der [ **StorageFile.Properties** ](https://msdn.microsoft.com/library/windows/apps/br227225) Eigenschaft.) Während der obersten Ebene und die grundlegende Eigenschaften wie die Eigenschaften einer Klasse zugegriffen werden –[**"storagefile"** ](https://msdn.microsoft.com/library/windows/apps/br227171) und **BasicProperties**bzw. – werden erweiterte Eigenschaften erhalten durch Übergeben einer ["IEnumerable"](https://go.microsoft.com/fwlink/p/?LinkID=313091) Auflistung von [Zeichenfolge](https://go.microsoft.com/fwlink/p/?LinkID=325032) Objekte, die die Namen der Eigenschaften, die auf abgerufen werden sollen die  **BasicProperties.RetrievePropertiesAsync** Methode. Diese Methode gibt dann eine [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238) -Sammlung zurück. Anschließend werden die einzelnen erweiterten Eigenschaften anhand des Namens oder Indexes aus der Sammlung abgerufen.

In diesem Beispiel werden alle Dateien der Bildbibliothek aufgezählt und die Namen der gewünschten Eigenschaften (**DataAccessed** und **FileOwner**) in einem [List](https://go.microsoft.com/fwlink/p/?LinkID=325246)-Objekt angegeben. Dieses [List](https://go.microsoft.com/fwlink/p/?LinkID=325246)-Objekt wird an [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) übergeben, um die Eigenschaften abzurufen. Diese Eigenschaften werden dann anhand des Namens aus dem zurückgegebenen [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238)-Objekt abgerufen.

Weitere Informationen finden Sie unter der [Windows Core-Eigenschaften](https://msdn.microsoft.com/library/windows/desktop/mt805470) für eine vollständige Liste der erweiterten Eigenschaften einer Datei.

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 
