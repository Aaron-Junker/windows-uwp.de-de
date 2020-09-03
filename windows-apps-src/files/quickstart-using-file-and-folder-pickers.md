---
ms.assetid: F87DBE2F-77DB-4573-8172-29E11ABEFD34
title: Öffnen von Dateien und Ordnern mit einer Auswahl
description: Greifen Sie auf Dateien und Ordner zu, indem Sie Benutzern die Interaktion mit einer Auswahl ermöglichen. Mithilfe der FileOpenPicker- und der FileSavePicker-Klasse können Sie auf Dateien und mithilfe der FolderPicker-Klasse auf einen Ordner zugreifen.
ms.date: 12/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8a438c5c0fa0e3dbe932c393291afe05b28b3e4a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175424"
---
# <a name="open-files-and-folders-with-a-picker"></a>Öffnen von Dateien und Ordnern mit einer Auswahl

**Wichtige APIs**

-   [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)
-   [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker)
-   [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)

Greifen Sie auf Dateien und Ordner zu, indem Sie Benutzern die Interaktion mit einer Auswahl ermöglichen. Mithilfe der Klassen [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) und [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) können Sie auf Dateien und mithilfe der Klasse [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker) auf einen Ordner zugreifen.

> [!NOTE]
> Ein vollständiges Beispiel findest du im [Beispiel für die Dateiauswahl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker).

## <a name="prerequisites"></a>Voraussetzungen


-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

-   **Zugriffsberechtigungen für den Speicherort**

    Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## <a name="file-picker-ui"></a>Dateiauswahl – Benutzeroberfläche


Eine Dateiauswahl zeigt Informationen für die Orientierung der Benutzer an und stellt eine einheitliche Benutzererfahrung beim Öffnen oder Speichern von Dateien bereit.

Diese Informationen umfassen:

-   Den aktuellen Speicherort
-   Die Elemente, die der Benutzer ausgewählt hat
-   Eine Struktur mit Speicherorten, die der Benutzer durchsuchen kann. Diese Speicherorte umfassen Dateisystemspeicherorte, wie z. B. Musik- oder Downloadordner, sowie Apps, die den Dateiauswahlvertrag (z. B. Kamera, Fotos und Microsoft OneDrive) implementieren.

Eine E-Mail-App zeigt möglicherweise eine Dateiauswahl an, damit der Benutzer Anlagen auswählen kann.

![Eine Dateiauswahl mit zwei zu öffnenden Dateien.](images/picker-multifile-600px.png)

## <a name="how-pickers-work"></a>Funktionsweise der Auswahl


Mithilfe einer Auswahl kann Ihre App Zugriff auf Dateien und Ordner auf dem System des Benutzers erlangen und diese durchsuchen und speichern. Ihre App erhält diese Auswahl als [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)- und [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder)-Objekte, die Sie dann verarbeiten können.

Die Auswahl verwendet eine einzige, einheitliche Oberfläche, über die der Benutzer Dateien und Ordner im Dateisystem oder in anderen Apps auswählen kann. Mit Dateien, die in anderen Apps ausgewählt werden, verhält es sich genauso wie mit Dateien im Dateisystem: Sie werden als [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Objekte zurückgegeben. Im Allgemeinen kann Ihre App genauso mit ihnen arbeiten wie mit anderen Objekten. Andere Apps stellen Dateien bereit, indem sie an Verträgen für die Dateiauswahl teilnehmen. Wenn Ihre App Dateien, einen Speicherort oder Dateiupdates für andere Apps bereitstellen soll, finden Sie Informationen dazu unter [Integration mit Verträgen für die Dateiauswahl](/previous-versions/windows/apps/hh465192(v=win.10)).

Beispielsweise können Sie die Dateiauswahl in Ihrer App aufrufen und dem Benutzer so ermöglichen, eine Datei zu öffnen. Dadurch wird Ihre App zur aufrufenden App. Die Dateiauswahl interagiert mit dem System und/oder anderen Apps, damit der Benutzer zu einer Datei navigieren und diese auswählen kann. Wählt der Benutzer eine Datei aus, gibt die Dateiauswahl diese Datei an Ihre App zurück. Dies ist der Prozess für einen Fall, in dem ein Benutzer eine Datei in einer bereitstellenden App wie etwa OneDrive auswählt.

![Ein Diagramm, das zeigt, wie eine App eine zu öffnende Datei aus einer anderen App erhält. Die Dateiauswahl fungiert hier als Schnittstelle zwischen den beiden Apps.](images/app-to-app-diagram-600px.png)

## <a name="pick-a-single-file-complete-code-listing"></a>Auswählen einer einzelnen Datei: vollständige Codeauflistung


```cs
var picker = new Windows.Storage.Pickers.FileOpenPicker();
picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
picker.FileTypeFilter.Add(".jpg");
picker.FileTypeFilter.Add(".jpeg");
picker.FileTypeFilter.Add(".png");

Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
if (file != null)
{
    // Application now has read/write access to the picked file
    this.textBlock.Text = "Picked photo: " + file.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

## <a name="pick-a-single-file-step-by-step"></a>Auswählen einer einzelnen Datei: Schritt für Schritt


Das Verwenden der Dateiauswahl umfasst das Erstellen und Anpassen eines Dateiauswahlobjekts und Einblenden der Dateiauswahl, sodass der Benutzer ein oder mehrere Elemente auswählen kann.

1.  **Erstellen und Anpassen eines FileOpenPicker-Objekts**

    ```cs
    var picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
    picker.FileTypeFilter.Add(".jpg");
    picker.FileTypeFilter.Add(".jpeg");
    picker.FileTypeFilter.Add(".png");
    ```
    Legen Sie Eigenschaften für das Dateiauswahlobjekt fest, die für Ihre Benutzer und Ihre App relevant sind.

    Dieses Beispiel erstellt eine ansprechende visuelle Darstellung von Bildern an einem praktischen Ort, aus denen der Benutzer wählen kann, indem drei Eigenschaften festgelegt werden: [**ViewMode**](/uwp/api/windows.storage.pickers.fileopenpicker.viewmode), [**SuggestedStartLocation**](/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) und [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter).

    -   Wenn [**ViewMode**](/uwp/api/windows.storage.pickers.fileopenpicker.viewmode) auf den **Thumbnail**-Enumerationswert [**PickerViewMode**](/uwp/api/Windows.Storage.Pickers.PickerViewMode) festgelegt wird, entsteht eine ansprechende visuelle Darstellung, da Miniaturbilder als Darstellung für die Dateien in der Dateiauswahl erscheinen. Dies gilt für die Auswahl visueller Dateien wie Bilder oder Videos. Verwenden Sie andernfalls [**PickerViewMode.List**](/uwp/api/Windows.Storage.Pickers.PickerViewMode). Eine hypothetische E-Mail-App mit **Bild oder Video anfügen**- und **Dokument anfügen**-Funktionen würde den **ViewMode** auf die entsprechende Funktion vor dem Anzeigen der Dateiauswahl festlegen.

    -   Wenn [**SuggestedStartLocation**](/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) mithilfe von [**PickerLocationId.PicturesLibrary**](/uwp/api/Windows.Storage.Pickers.PickerLocationId) auf Bilder festgelegt wird, beginnt der Benutzer in einem Pfad, der mit hoher Wahrscheinlichkeit Bilder enthält. Legen Sie **SuggestedStartLocation** auf einen Speicherort fest, der dem Typ der ausgewählten Datei entspricht, z. B. Musik, Bilder, Videos oder Dokumente. Der Benutzer kann vom Ausgangspfad aus zu anderen Speicherorten navigieren.

    -   Mit [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) zum Angeben von Dateitypen wählt der Benutzer weiterhin relevante Dateien aus. Um ältere Dateitypen im **FileTypeFilter** durch neue Einträgen zu ersetzen, verwenden Sie [**ReplaceAll**](/uwp/api/windows.storage.pickers.fileextensionvector.replaceall) anstelle von [**Add**](/uwp/api/windows.storage.pickers.fileextensionvector.append).

2.  **Anzeigen von FileOpenPicker**

    - **So wählst du eine einzelne Datei aus**

        ```cs
        Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
        if (file != null)
        {
            // Application now has read/write access to the picked file
            this.textBlock.Text = "Picked photo: " + file.Name;
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
        ```

    - **So wählst du mehrere Dateien aus**  

        ```cs
        var files = await picker.PickMultipleFilesAsync();
        if (files.Count > 0)
        {
            StringBuilder output = new StringBuilder("Picked files:\n");
    
            // Application now has read/write access to the picked file(s)
            foreach (Windows.Storage.StorageFile file in files)
            {
                output.Append(file.Name + "\n");
            }
            this.textBlock.Text = output.ToString();
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
        ```

## <a name="pick-a-folder-complete-code-listing"></a>Auswählen eines Ordners: vollständige Codeauflistung


```cs
var folderPicker = new Windows.Storage.Pickers.FolderPicker();
folderPicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.Desktop;
folderPicker.FileTypeFilter.Add("*");

Windows.Storage.StorageFolder folder = await folderPicker.PickSingleFolderAsync();
if (folder != null)
{
    // Application now has read/write access to all contents in the picked folder
    // (including other sub-folder contents)
    Windows.Storage.AccessCache.StorageApplicationPermissions.
    FutureAccessList.AddOrReplace("PickedFolderToken", folder);
    this.textBlock.Text = "Picked folder: " + folder.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

> [!TIP]
> Füge die Datei oder den Ordner zur Verbesserung der Nachverfolgbarkeit zu [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) oder zu [**MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) der App hinzu, wenn deine App über eine Dateiauswahl auf eine Datei oder einen Ordner zugreift. Weitere Informationen zur Verwendung dieser Listen finden Sie in [So wird's gemacht: Nachverfolgen kürzlich verwendeter Dateien und Ordner](how-to-track-recently-used-files-and-folders.md).