---
ms.assetid: 12ECEA89-59D2-4BCE-B24C-5A4DD525E0C7
title: Zugriff auf Inhalte in der Heimnetzgruppe
description: Greifen Sie auf Inhalte zu, die sich im Heimnetzgruppenordner des Benutzers befinden, einschließlich Bildern, Musik und Videos.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 422d02b8797bef8ea4f793ba652e9e4e34421a9b
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "8327838"
---
# <a name="accessing-homegroup-content"></a>Zugriff auf Inhalte in der Heimnetzgruppe



**Wichtige APIs**

-   [**Windows.Storage.KnownFolders-Klasse**](https://msdn.microsoft.com/library/windows/apps/br227151)

Greifen Sie auf Inhalte zu, die sich im Heimnetzgruppenordner des Benutzers befinden, einschließlich Bildern, Musik und Videos.

## <a name="prerequisites"></a>Voraussetzungen

-   **Verstehen der asynchronen Programmierung für UWP-Apps (Universelle Windows-Plattform)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Deklaration der App-Funktionen**

    Damit auf Heimnetzgruppeninhalte zugegriffen werden kann, muss auf dem Computer des Benutzers eine Heimnetzgruppe eingerichtet sein, und Ihre App muss mindestens eine der folgenden Funktionen unterstützen: **picturesLibrary**, **musicLibrary** oder **videosLibrary**. Wenn Ihre App auf den Heimnetzgruppenordner zugreift, sieht sie nur die Bibliotheken, die den im Manifest Ihrer App deklarierten Funktionen entsprechen. Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

    > [!NOTE]
    >  Inhalte der Dokumentbibliothek einer Heimnetzgruppe sind für Ihre App nicht sichtbar, unabhängig von den deklarierten Funktionen in ihrem Manifest und den Freigabeeinstellungen des Benutzers.     

-   **So wird's gemacht: Verwenden der Dateiauswahl**

    Normalerweise verwenden Sie die Dateiauswahl, um auf Dateien und Ordner in der Heimnetzgruppe zuzugreifen. Informationen zur Verwendung der Dateiauswahl finden Sie unter [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md).

-   **Datei- und Ordnerabfragen**

    Sie können Abfragen dazu verwenden, Dateien und Ordner in der Heimnetzgruppe aufzulisten. Weitere Informationen zu Datei- und Ordnerabfragen finden Sie unter [Aufzählen und Abfragen von Dateien und Ordnern](quickstart-listing-files-and-folders.md).

## <a name="open-the-file-picker-at-the-homegroup"></a>Öffnen der Dateiauswahl für die Heimnetzgruppe

Führen Sie die folgenden Schritte durch, um eine Instanz der Dateiauswahl zu öffnen, mit der Benutzer Dateien und Ordner in der Heimnetzgruppe auswählen können:

1.  **Erstellen und Anpassen der Dateiauswahl**

    Verwenden Sie [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) zum Erstellen einer Dateiauswahl, und legen Sie dann den [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) der Auswahl auf [**PickerLocationId.HomeGroup**](https://msdn.microsoft.com/library/windows/apps/br207890) fest. Optional können Sie weitere Eigenschaften festlegen, die für Benutzer und App relevant sind. Richtlinien zur Anpassung der Dateiauswahl erhalten Sie in [Richtlinien und Prüfliste für die Dateiauswahl](https://msdn.microsoft.com/library/windows/apps/hh465182).

    In diesem Beispiel wird eine Dateiauswahl erstellt, die in der Heimnetzgruppe geöffnet wird, bei der Dateien jeglichen Typs angezeigt werden und die Dateien als Miniaturbilder erscheinen:
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add("*");
    ```

2.  **Blenden Sie die Dateiauswahl ein und verarbeiten Sie die ausgewählte Datei.**

    Lassen Sie den Benutzer eine Datei auswählen, indem Sie [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) aufrufen, oder mehrere Dateien auswählen, indem Sie [**FileOpenPicker.PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) aufrufen, nachdem Sie die Dateiauswahl erstellt und angepasst haben.

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

1.  **Rufen Sie den Abfrageausdruck vom Benutzer ab.**

    Hier erhalten wir einen Abfrageausdruck, den der Benutzer in ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)-Steuerelement namens `searchQueryTextBox` eingegeben hat:
    ```cs
    string queryTerm = this.searchQueryTextBox.Text;    
    ```

2.  **Legen Sie die Abfrageoptionen und Suchfilter fest.**

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

3.  **Führen Sie die Abfrage aus und verarbeiten Sie die Ergebnisse.**

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

1.  **Stellen Sie eine Sammlung von Heimnetzgruppenbenutzern zusammen.**

    Jeder Ordner auf oberster Ebene in der Heimnetzgruppe repräsentiert einen bestimmten Heimnetzgruppenbenutzer. Um die Sammlung der Heimnetzgruppenbenutzer zu erhalten, rufen Sie also [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227279) auf, um die Heimnetzgruppenordner der obersten Ebene abzurufen.
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFolder> hgFolders =
        await Windows.Storage.KnownFolders.HomeGroup.GetFoldersAsync();    
    ```

2.  **Suchen Sie dann nach dem Ordner des Zielbenutzers, und erstellen Sie eine Dateiabfrage, die auf den Ordner dieses Benutzers eingegrenzt ist.**

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

1.  **Fügen Sie Ihrer App ein „MediaElement“ hinzu.**

    Mithilfe von [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) können Sie Audio- und Videoinhalte in Ihrer App wiedergeben. Weitere Informationen zur Audio- und Videowiedergabe finden Sie unter [Erstellen von benutzerdefinierten Transportsteuerelementen](https://msdn.microsoft.com/library/windows/apps/mt187271) und [Audio, Video und Kamera](https://msdn.microsoft.com/library/windows/apps/mt203788).
    ```HTML
    <Grid x:Name="Output" HorizontalAlignment="Left" VerticalAlignment="Top" Grid.Row="1">
        <MediaElement x:Name="VideoBox" HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0" Width="400" Height="300"/>
    </Grid>    
    ```

2.  **Öffnen Sie eine Dateiauswahl für die Heimnetzgruppe und wenden Sie einen Filter an, der alle Dateien ausschließt, bei denen es sich nicht um Videodateiformate handelt, die Ihre App unterstützt.**

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

3.  **Öffnen Sie die Auswahl des Benutzers Datei für den Lesezugriff, und legen Sie den Dateidatenstrom als Quelle für die** [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)und dann wieder starten der Datei.
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
