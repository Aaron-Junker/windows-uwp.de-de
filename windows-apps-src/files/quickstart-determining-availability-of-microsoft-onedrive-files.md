---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Ermitteln der Verfügbarkeit von Microsoft OneDrive-Dateien
description: Ermitteln Sie mithilfe der StorageFile.IsAvailable-Eigenschaft, ob eine Microsoft OneDrive-Datei verfügbar ist.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: effb28fa453ec884152dbc404245f00f4893ef5a
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369427"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Ermitteln der Verfügbarkeit von Microsoft OneDrive-Dateien


**Wichtige APIs**

-   [**FileIO-Klasse**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
-   [**StorageFile-Klasse**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)
-   [**StorageFile.IsAvailable-Eigenschaft**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)

Ermitteln Sie mithilfe der [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)-Eigenschaft, ob eine Microsoft OneDrive-Datei verfügbar ist.

## <a name="prerequisites"></a>Voraussetzungen

-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Deklarationen von App-Funktionen**

    Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## <a name="using-the-storagefileisavailable-property"></a>Verwenden der StorageFile.IsAvailable-Eigenschaft

Benutzer können OneDrive-Dateien als „Offline verfügbar“ (Standardeinstellung) oder „Nur online verfügbar“ kennzeichnen. Diese Funktion bietet Benutzern die Möglichkeit, große Dateien (z. B. Bilder und Videos) in ihren OneDrive-Speicher zu verschieben, als nur online verfügbar zu kennzeichnen und so Speicherplatz auf der Festplatte zu sparen (lokal wird nur eine Datei mit Metadaten gespeichert).

Mithilfe von [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) wird ermittelt, ob eine Datei zurzeit verfügbar ist. Die folgende Tabelle zeigt den Wert der **StorageFile.IsAvailable**-Eigenschaft in verschiedenen Szenarien.

| Dateityp                              | Online | Getaktetes Netzwerk        | Offline |
|-------------------------------------------|--------|------------------------|---------|
| Lokale Datei                                | True   | True                   | True    |
| Als "Offline verfügbar" gekennzeichnete OneDrive-Datei | True   | True                   | True    |
| Als "Nur online verfügbar" gekennzeichnete OneDrive-Datei       | True   | Basierend auf Benutzereinstellungen | False   |
| Netzwerkdatei                              | True   | Basierend auf Benutzereinstellungen | False   |

 

Die folgenden Schritte zeigen, wie festgestellt wird, ob eine Datei momentan verfügbar ist.

1.  Deklarieren Sie eine für die Bibliothek, auf die Sie zugreifen möchten, geeignete Funktion.
2.  Schließen Sie den [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage)-Namespace ein. Dieser Namespace enthält die Typen zum Verwalten von Dateien, Ordnern und Anwendungseinstellungen. Außerdem enthält er den erforderlichen [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Typ.
3.  Beschaffen Sie ein [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt für die gewünschte Datei bzw. die gewünschten Dateien. Wenn Sie eine Bibliothek aufzählen, können Sie zur Durchführung dieses Schritts die [**StorageFolder.CreateFileQuery**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfilequery)-Methode und dann die [**GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync)-Methode des sich ergebenden [**StorageFileQueryResult**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.StorageFileQueryResult)-Objekts aufrufen. Die **GetFilesAsync**-Methode gibt eine [IReadOnlyList](https://go.microsoft.com/fwlink/p/?LinkId=324970)-Sammlung mit **StorageFile**-Objekten zurück.
4.  Nachdem Sie den Zugriff auf ein [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt eingerichtet haben, das die gewünschten Dateien darstellt, spiegelt der Wert der [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)-Eigenschaft wider, ob die Datei verfügbar ist.

Die folgende generische Methode veranschaulicht, wie Sie einen beliebigen Ordner aufzählen und die Sammlung mit [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekten für diesen Ordner zurückgeben. Die aufrufende Methode durchläuft dann die zurückgegebene Sammlung und verweist für jede Datei auf die [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)-Eigenschaft.

```cs
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```
