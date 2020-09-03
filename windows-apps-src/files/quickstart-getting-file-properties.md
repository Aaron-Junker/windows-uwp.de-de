---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Abrufen von Dateieigenschaften
description: Rufe Eigenschaften&\#8212;oberste Ebene, grundlegend und erweitert&\#8212;für eine Datei ab, die durch ein StorageFile-Objekt dargestellt wird.
ms.date: 12/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1812908319bed81ab776e8e66753f4f881a8728f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156544"
---
# <a name="get-file-properties"></a>Abrufen von Dateieigenschaften

**Wichtige APIs**

-   [**StorageFile.GetBasicPropertiesAsync**](/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

Es werden Eigenschaften – oberste Ebene, grundlegend und erweitert – für eine Datei abgerufen, die durch ein [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Objekt dargestellt wird.

> [!NOTE]
> Ein vollständiges Beispiel findest du im [Beispiel zum Dateizugriff](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess).

## <a name="prerequisites"></a>Voraussetzungen

-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

-   **Zugriffsberechtigungen für den Speicherort**

    Der Code in diesen Beispielen erfordert beispielsweise den Zugriff auf die **picturesLibrary**-Funktion, während Ihr Speicherort einen anderen Zugriffstyp oder keinen Zugriff voraussetzt. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Abrufen von Eigenschaften der obersten Ebene einer Datei

Auf viele Dateieigenschaften der obersten Ebene kann in Form von Membern der [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Klasse zugegriffen werden. Diese Eigenschaften enthalten für eine Datei Attribute, Inhaltstyp, Erstellungsdatum, Anzeigename, Dateityp usw.

> [!NOTE]
> Denke daran, die Funktion **picturesLibrary** zu deklarieren.

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

Viele grundlegende Dateieigenschaften werden abgerufen, indem zuerst die [**StorageFile.GetBasicPropertiesAsync**](/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)-Methode aufgerufen wird. Diese Methode gibt ein [**BasicProperties**](/uwp/api/Windows.Storage.FileProperties.BasicProperties)-Objekt zurück, das Eigenschaften für die Größe des Elements (Datei oder Ordner) und das letzte Änderungsdatum des Elements definiert.

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

Neben den Eigenschaften der obersten Ebene und den grundlegenden Eigenschaften sind dem Inhalt der Datei viele Eigenschaften zugeordnet. Auf diese erweiterten Eigenschaften wird zugegriffen, indem die [**BasicProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync)-Methode aufgerufen wird. (Ein [**BasicProperties**](/uwp/api/Windows.Storage.FileProperties.BasicProperties)-Objekt wird durch den Aufruf einer [**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties)-Eigenschaft abgerufen.) Während auf Dateieigenschaften der obersten Ebene und grundlegende Dateieigenschaften in Form von Eigenschaften einer Klasse zugegriffen werden kann – [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) und **BasicProperties**, werden erweiterte Eigenschaften abgerufen, indem eine [IEnumerable](/dotnet/api/system.collections.ienumerable) -Sammlung von [String](/dotnet/api/system.string)-Objekten mit den Namen der abzurufenden Eigenschaften an die **BasicProperties.RetrievePropertiesAsync**-Methode übergeben wird. Diese Methode gibt dann eine [IDictionary](/dotnet/api/system.collections.idictionary) -Sammlung zurück. Anschließend werden die einzelnen erweiterten Eigenschaften anhand des Namens oder Indexes aus der Sammlung abgerufen.

In diesem Beispiel werden alle Dateien der Bildbibliothek aufgezählt und die Namen der gewünschten Eigenschaften (**DataAccessed** und **FileOwner**) in einem [List](/dotnet/api/system.collections.generic.list-1)-Objekt angegeben. Dieses [List](/dotnet/api/system.collections.generic.list-1)-Objekt wird an [**BasicProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) übergeben, um die Eigenschaften abzurufen. Diese Eigenschaften werden dann anhand des Namens aus dem zurückgegebenen [IDictionary](/dotnet/api/system.collections.idictionary)-Objekt abgerufen.

Eine vollständige Liste der erweiterten Eigenschaften einer Datei findest du im Thema zu den [Windows Core-Eigenschaften](/windows/desktop/properties/core-bumper).

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

 

 