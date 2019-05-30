---
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: Speichern einer Datei mit einer Auswahl
description: Mithilfe von FileSavePicker können Benutzer den Namen und Speicherort zum Speichern einer Datei durch die App angeben.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c1480c17d97cb8b5e227cc44b384f13095bfd469
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369251"
---
# <a name="save-a-file-with-a-picker"></a>Speichern einer Datei mit einer Auswahl

**Wichtige APIs**

-   [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)
-   [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)

Mithilfe von [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) können Benutzer den Namen und Speicherort zum Speichern einer Datei durch die App angeben.

> [!NOTE]
> Ein vollständiges Beispiel finden Sie unter den [Beispieldatei mit Auswahl](https://go.microsoft.com/fwlink/p/?linkid=619994).

 

## <a name="prerequisites"></a>Vorraussetzungen


-   **Verstehen Sie die asynchrone Programmierung für apps der universellen Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Zugriffsberechtigungen für den Speicherort**

    Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## <a name="filesavepicker-step-by-step"></a>FileSavePicker: Schritt für Schritt

Verwenden Sie [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker), damit Benutzer den Namen, Typ und Speicherort von zu speichernden Dateien angeben können. Erstellen Sie ein Dateiauswahlobjekt, passen und zeigen Sie es an, und speichern Sie dann Daten über das zurückgegebene [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt, das die ausgewählte Datei darstellt.

1.  **Erstellen und Anpassen der FileSavePicker**

    ```cs
    var savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";
    ```

Legen Sie Eigenschaften für das Dateiauswahlobjekt fest, die für Ihre Benutzer und Ihre App relevant sind. In diesem Beispiel werden drei Eigenschaften festgelegt: [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation), [ **FileTypeChoices** ](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices) und [ **SuggestedFileName**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename).
     
- Da der Benutzer in unserem Fall ein Dokument oder eine Textdatei speichert, wird im Beispiel [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation) auf den lokalen Ordner der App mithilfe von [**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) festgelegt. Legen Sie [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) auf einen Speicherort fest, der dem Typ der gespeicherten Datei entspricht, z. B. Musik, Bilder, Videos oder Dokumente. Der Benutzer kann vom Ausgangspfad aus zu anderen Speicherorten navigieren.

- Da wir sicherstellen möchten, dass unsere App die Datei nach dem Speichern öffnen kann, verwenden wir im Beispiel [**FileTypeChoices**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices) für die Angabe der vom Beispiel unterstützten Dateitypen (Microsoft Word-Dokumente und Textdateien). Stellen Sie sicher, dass alle von Ihnen angegebenen Dateitypen von Ihrer App unterstützt werden. Die Benutzer können ihre Datei unter einem beliebigen der von Ihnen angegebenen Dateitypen speichern. Sie können auch den Dateityp ändern, indem sie einen anderen von Ihnen angegebenen Dateityp auswählen. Der erste Dateityp in der Liste ist standardmäßig ausgewählt. Legen Sie zum Steuern dieses Verhaltens die [**DefaultFileExtension**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.defaultfileextension)-Eigenschaft fest.

    > [!NOTE]
    > Die Dateiauswahl verwendet auch den Typ der aktuell ausgewählten Datei, welche Dateien, die angezeigt wird, filtern, sodass nur die Dateitypen, die die ausgewählten Dateitypen entsprechen, die dem Benutzer angezeigt werden.

- Um dem Benutzer einige Eingaben zu ersparen, legt das Beispiel einen [**SuggestedFileName**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename) fest. Verwenden Sie als vorgeschlagenen Dateinamen einen Namen, der für die gespeicherte Datei relevant ist. Beispielsweise können Sie wie in Word den vorhandenen Dateinamen vorschlagen, sofern vorhanden. Sie können auch die erste Zeile eines Dokuments vorschlagen, wenn der Benutzer eine noch nicht benannte Datei speichert.

> [!NOTE]
> [**FileSavePicker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) Objekte anzuzeigen, die Auswahl mithilfe der [ **PickerViewMode.List** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerViewMode) sichtmodus befinden.

2.  **Die FileSavePicker anzeigen und speichern in der ausgewählten Datei**

    Zeigen Sie die Dateiauswahl durch Aufrufen von [**PickSaveFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.picksavefileasync) an. Nachdem die Benutzer den Namen, Dateityp und Speicherort angegeben und bestätigt haben, dass die Datei gespeichert wird, gibt **PickSaveFileAsync** ein [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt zurück, das die gespeicherte Datei darstellt. Sie können diese Datei erfassen und verarbeiten, da sie jetzt über Lese- und Schreibzugriff verfügen.

    ```cs
    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
        if (file != null)
        {
            // Prevent updates to the remote version of the file until
            // we finish making changes and call CompleteUpdatesAsync.
            Windows.Storage.CachedFileManager.DeferUpdates(file);
            // write to file
            await Windows.Storage.FileIO.WriteTextAsync(file, file.Name);
            // Let Windows know that we're finished changing the file so
            // the other app can update the remote version of the file.
            // Completing updates may require Windows to ask for user input.
            Windows.Storage.Provider.FileUpdateStatus status =
                await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
            if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
            {
                this.textBlock.Text = "File " + file.Name + " was saved.";
            }
            else
            {
                this.textBlock.Text = "File " + file.Name + " couldn't be saved.";
            }
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
    ```

Im Beispiel wird überprüft, ob die Datei gültig ist. Danach wird der eigene Dateiname in die Datei geschrieben. Weitere Informationen finden Sie unter [Erstellen, Lesen und Schreiben einer Datei](quickstart-reading-and-writing-files.md)

> [!TIP]
> Sie sollten immer überprüfen, dass die gespeicherte Datei aus, um sicherzustellen, dass er gültig ist, bevor Sie andere Verarbeitungsschritte auszuführen. Anschließend können Sie Inhalte dem Zweck der App entsprechend in der Datei speichern und das Verhalten für den Fall festlegen, dass die ausgewählte Datei nicht gültig ist.
