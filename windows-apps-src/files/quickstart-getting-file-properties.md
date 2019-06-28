---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Abrufen von Dateieigenschaften
description: Rufe Eigenschaften&\#8212;oberste Ebene, grundlegend und erweitert&\#8212;für eine Datei ab, die durch ein StorageFile-Objekt dargestellt wird.
ms.date: 12/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 01eda8eefea7e1b3b1102ef154a019e1630e80c2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369315"
---
# <a name="get-file-properties"></a>Abrufen von Dateieigenschaften

**Wichtige APIs**

-   [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

Es werden Eigenschaften – oberste Ebene, grundlegend und erweitert – für eine Datei abgerufen, die durch ein [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt dargestellt wird.

> [!NOTE]
> Ein vollständiges Beispiel findest du im [Beispiel zum Dateizugriff](https://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Voraussetzungen

-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Zugriffsberechtigungen für den Speicherort**

    Der Code in diesen Beispielen erfordert beispielsweise den Zugriff auf die **picturesLibrary**-Funktion, während Ihr Speicherort einen anderen Zugriffstyp oder keinen Zugriff voraussetzt. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Abrufen von Eigenschaften der obersten Ebene einer Datei

Auf viele Dateieigenschaften der obersten Ebene kann in Form von Membern der [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Klasse zugegriffen werden. Diese Eigenschaften enthalten für eine Datei Attribute, Inhaltstyp, Erstellungsdatum, Anzeigename, Dateityp usw.

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

Viele grundlegende Dateieigenschaften werden abgerufen, indem zuerst die [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)-Methode aufgerufen wird. Diese Methode gibt ein [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties)-Objekt zurück, das Eigenschaften für die Größe des Elements (Datei oder Ordner) und das letzte Änderungsdatum des Elements definiert.

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

Neben den Eigenschaften der obersten Ebene und den grundlegenden Eigenschaften sind dem Inhalt der Datei viele Eigenschaften zugeordnet. Auf diese erweiterten Eigenschaften wird zugegriffen, indem die [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync)-Methode aufgerufen wird. (Ein [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties)-Objekt wird durch Aufruf der Eigenschaft [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties) abgerufen.) Während auf Dateieigenschaften der obersten Ebene und grundlegende Dateieigenschaften in Form von Eigenschaften einer Klasse zugegriffen werden kann ([**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) und **BasicProperties**), werden erweiterte Eigenschaften abgerufen, indem eine [IEnumerable](https://go.microsoft.com/fwlink/p/?LinkID=313091)-Sammlung von [String](https://go.microsoft.com/fwlink/p/?LinkID=325032)-Objekten mit den Namen der abzurufenden Eigenschaften an die **BasicProperties.RetrievePropertiesAsync**-Methode übergeben wird. Diese Methode gibt dann eine [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238) -Sammlung zurück. Anschließend werden die einzelnen erweiterten Eigenschaften anhand des Namens oder Indexes aus der Sammlung abgerufen.

In diesem Beispiel werden alle Dateien der Bildbibliothek aufgezählt und die Namen der gewünschten Eigenschaften (**DataAccessed** und **FileOwner**) in einem [List](https://go.microsoft.com/fwlink/p/?LinkID=325246)-Objekt angegeben. Dieses [List](https://go.microsoft.com/fwlink/p/?LinkID=325246)-Objekt wird an [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) übergeben, um die Eigenschaften abzurufen. Diese Eigenschaften werden dann anhand des Namens aus dem zurückgegebenen [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238)-Objekt abgerufen.

Eine vollständige Liste der erweiterten Eigenschaften einer Datei findest du im Thema zu den [Windows Core-Eigenschaften](https://docs.microsoft.com/windows/desktop/properties/core-bumper).

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

 

 
