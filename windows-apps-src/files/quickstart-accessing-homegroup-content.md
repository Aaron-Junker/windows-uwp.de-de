---
ms.assetid: 12ECEA89-59D2-4BCE-B24C-5A4DD525E0C7
title: Zugriff auf Inhalte in der Heimnetzgruppe
description: Greifen Sie auf Inhalte zu, die sich im Heimnetzgruppenordner des Benutzers befinden, einschließlich Bildern, Musik und Videos.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07d94f5b11acfe14bf55392c5cbf2c1b7bcfbeef
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369395"
---
# <a name="accessing-homegroup-content"></a>Zugriff auf Inhalte in der Heimnetzgruppe



**Wichtige APIs**

-   [**Windows.Storage.KnownFolders-Klasse**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)

Greifen Sie auf Inhalte zu, die sich im Heimnetzgruppenordner des Benutzers befinden, einschließlich Bildern, Musik und Videos.

## <a name="prerequisites"></a>Vorraussetzungen

-   **Verstehen Sie die asynchrone Programmierung für apps der universellen Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Deklarationen der App capabilty**

    Damit auf Heimnetzgruppeninhalte zugegriffen werden kann, muss auf dem Computer des Benutzers eine Heimnetzgruppe eingerichtet sein, und Ihre App muss mindestens eine der folgenden Funktionen unterstützen: **picturesLibrary**, **musicLibrary** oder **videosLibrary**. Wenn Ihre App auf den Heimnetzgruppenordner zugreift, sieht sie nur die Bibliotheken, die den im Manifest Ihrer App deklarierten Funktionen entsprechen. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

    > [!NOTE]
    > Inhalt in der Bibliothek Dokumente, einer Heimnetzgruppe nicht zu Ihrer app unabhängig von den Fähigkeiten, die in Ihrer app-Manifest deklariert und unabhängig von den Einstellungen des Benutzers Freigabe angezeigt.     

-   **Grundlegendes zur Verwendung von Datei dateiöffnungs-**

    Normalerweise verwenden Sie die Dateiauswahl, um auf Dateien und Ordner in der Heimnetzgruppe zuzugreifen. Informationen zur Verwendung der Dateiauswahl finden Sie unter [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md).

-   **Verstehen von Datei- und Abfragen**

    Sie können Abfragen dazu verwenden, Dateien und Ordner in der Heimnetzgruppe aufzulisten. Weitere Informationen zu Datei- und Ordnerabfragen finden Sie unter [Aufzählen und Abfragen von Dateien und Ordnern](quickstart-listing-files-and-folders.md).

## <a name="open-the-file-picker-at-the-homegroup"></a>Öffnen der Dateiauswahl für die Heimnetzgruppe

Führen Sie die folgenden Schritte durch, um eine Instanz der Dateiauswahl zu öffnen, mit der Benutzer Dateien und Ordner in der Heimnetzgruppe auswählen können:

1.  **Erstellen und Anpassen der Dateiauswahl**

    Verwenden Sie [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) zum Erstellen einer Dateiauswahl, und legen Sie dann den [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) der Auswahl auf [**PickerLocationId.HomeGroup**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerLocationId) fest. Optional können Sie weitere Eigenschaften festlegen, die für Benutzer und App relevant sind. Richtlinien zur Anpassung der Dateiauswahl erhalten Sie in [Richtlinien und Prüfliste für die Dateiauswahl](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers).

    In diesem Beispiel wird eine Dateiauswahl erstellt, die in der Heimnetzgruppe geöffnet wird, bei der Dateien jeglichen Typs angezeigt werden und die Dateien als Miniaturbilder erscheinen:
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add("*");
    ```

2.  **Zeigen Sie die Dateiauswahl aus, und verarbeiten Sie die ausgewählte Datei.**

    Lassen Sie den Benutzer eine Datei auswählen, indem Sie [**FileOpenPicker.PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) aufrufen, oder mehrere Dateien auswählen, indem Sie [**FileOpenPicker.PickMultipleFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) aufrufen, nachdem Sie die Dateiauswahl erstellt und angepasst haben.

    Dieses Beispiel zeigt eine Dateiauswahl, bei der der Benutzer eine Datei auswählen kann:
    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    if (file != null)
    {
        // Do something with the file.
    }
    else
    {
        // No file returned. Handle the error.
    }   
    ```

## <a name="search-the-homegroup-for-files"></a>Durchsuchen der Heimnetzgruppe nach Dateien

In diesem Beispiel wird erläutert, wie Elemente in der Heimnetzgruppe gefunden werden können, die einem vom Benutzer angegebenen Abfrageausdruck entsprechen.

1.  **Rufen Sie den Abfrageausdruck, aus der Benutzer.**

    Hier erhalten wir einen Abfrageausdruck, den der Benutzer in ein [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Steuerelement namens `searchQueryTextBox` eingegeben hat:
    ```cs
    string queryTerm = this.searchQueryTextBox.Text;    
    ```

2.  **Legen Sie die Abfrageoptionen und Suchfilter.**

    Abfrageoptionen bestimmen, wie die Suchergebnisse sortiert werden, während durch den Suchfilter festgelegt wird, welche Elemente in den Suchergebnissen erscheinen.

    In diesem Beispiel werden Abfrageoptionen festgelegt, die die Suchergebnisse nach Relevanz und dem Änderungsdatum sortieren. Beim Suchfilter handelt es sich um den Abfrageausdruck, den der Benutzer im vorherigen Schritt eingegeben hat:
    ```cs
    Windows.Storage.Search.QueryOptions queryOptions =
            new Windows.Storage.Search.QueryOptions
                (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
    queryOptions.UserSearchFilter = queryTerm.Text;
    Windows.Storage.Search.StorageFileQueryResult queryResults =
            Windows.Storage.KnownFolders.HomeGroup.CreateFileQueryWithOptions(queryOptions);    
    ```

3.  **Die Abfrage auszuführen und die Ergebnisse zu verarbeiten.**

    Im folgenden Beispiel wird die Suchabfrage in der Heimnetzgruppe ausgeführt, und die Namen aller passenden Dateien werden in einer Liste mit Zeichenfolgen gespeichert.
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
        await queryResults.GetFilesAsync();

    if (files.Count > 0)
    {
        outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
        foreach (Windows.Storage.StorageFile file in files)
        {
            outputString += file.Name + "\n";
        }
    }    
    ```


## <a name="search-the-homegroup-for-a-particular-users-shared-files"></a>Durchsuchen der Heimnetzgruppe nach den freigegebenen Dateien eines bestimmten Benutzers

In diesem Abschnitt erfahren Sie, wie Sie nach Heimnetzgruppendateien suchen, die von einem bestimmten Benutzer freigegeben wurden.

1.  **Ruft eine Auflistung der heimnetzgruppenbenutzer.**

    Jeder Ordner auf oberster Ebene in der Heimnetzgruppe repräsentiert einen bestimmten Heimnetzgruppenbenutzer. Um die Sammlung der Heimnetzgruppenbenutzer zu erhalten, rufen Sie also [**GetFoldersAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfoldersasync) auf, um die Heimnetzgruppenordner der obersten Ebene abzurufen.
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFolder> hgFolders =
        await Windows.Storage.KnownFolders.HomeGroup.GetFoldersAsync();    
    ```

2.  **Suchen Sie nach dem Zielbenutzer-Ordner, und erstellen Sie dann eine Datei-Abfrage, die auf den Ordner des Benutzers begrenzt.**

    Das folgende Beispiel geht die abgerufenen Ordner durch, um den Ordner des Zielbenutzers zu finden. Anschließend legt es die Abfrageoptionen so fest, dass alle Dateien im Ordner gefunden werden, primär nach Relevanz und zweitrangig nach dem Änderungsdatum sortiert. Das Beispiel generiert eine Zeichenfolge, die die Anzahl der gefundenen Dateien zusammen mit den Namen der Dateien ausgibt.
    ```cs
    bool userFound = false;
    foreach (Windows.Storage.StorageFolder folder in hgFolders)
    {
        if (folder.DisplayName == targetUserName)
        {
            // Found the target user's folder, now find all files in the folder.
            userFound = true;
            Windows.Storage.Search.QueryOptions queryOptions =
                new Windows.Storage.Search.QueryOptions
                    (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
            queryOptions.UserSearchFilter = "*";
            Windows.Storage.Search.StorageFileQueryResult queryResults =
                folder.CreateFileQueryWithOptions(queryOptions);
            System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
                await queryResults.GetFilesAsync();

            if (files.Count > 0)
            {
                string outputString = "Searched for files belonging to " + targetUserName + "'\n";
                outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
                foreach (Windows.Storage.StorageFile file in files)
                {
                    outputString += file.Name + "\n";
                }
            }
        }
    }    
    ```

## <a name="stream-video-from-the-homegroup"></a>Streamen von Videos aus der Heimnetzgruppe

Gehen Sie wie folgt vor, um Videoinhalte aus der Heimnetzgruppe zu streamen:

1.  **Fügen Sie ein MediaElement in Ihre app ein.**

    Mithilfe von [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) können Sie Audio- und Videoinhalte in Ihrer App wiedergeben. Weitere Informationen zur Audio- und Videowiedergabe finden Sie unter [Erstellen von benutzerdefinierten Transportsteuerelementen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls) und [Audio, Video und Kamera](https://docs.microsoft.com/windows/uwp/audio-video-camera/index).
    ```HTML
    <Grid x:Name="Output" HorizontalAlignment="Left" VerticalAlignment="Top" Grid.Row="1">
        <MediaElement x:Name="VideoBox" HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0" Width="400" Height="300"/>
    </Grid>    
    ```

2.  **Öffnen Sie eine Dateiauswahl an der Heimnetzgruppe und Anwenden eines Filters mit Videodateien in den Formaten, die Ihrer app unterstützt.**

    In diesem Beispiel werden MP4- und WMV-Dateien für die Dateiauswahl festgelegt.
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add(".mp4");
    picker.FileTypeFilter.Add(".wmv");
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();   
    ```

3.  **Öffnen Sie die Auswahl des Benutzers-Datei für den Lesezugriff, und legen Sie den Datei-Stream als Quelle für die** [ **MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement), und klicken Sie dann die Datei nicht wiedergeben.
    ```cs
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        VideoBox.SetSource(stream, file.ContentType);
        VideoBox.Stop();
        VideoBox.Play();
    }
    else
    {
        // No file selected. Handle the error here.
    }    
    ```
