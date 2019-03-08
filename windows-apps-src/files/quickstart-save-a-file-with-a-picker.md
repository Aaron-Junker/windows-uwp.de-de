---
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: Speichern einer Datei mit einer Auswahl
description: Mithilfe von FileSavePicker können Benutzer den Namen und Speicherort zum Speichern einer Datei durch die App angeben.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4c61a34b983b0faaedc509b68fd4225ea0859a7d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660525"
---
# <a name="save-a-file-with-a-picker"></a>Speichern einer Datei mit einer Auswahl

**Wichtige APIs**

-   [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)
-   [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)

Mithilfe von [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) können Benutzer den Namen und Speicherort zum Speichern einer Datei durch die App angeben.

> [!NOTE]
> Ein vollständiges Beispiel finden Sie unter den [Beispieldatei mit Auswahl](https://go.microsoft.com/fwlink/p/?linkid=619994).

 

## <a name="prerequisites"></a>Voraussetzungen


-   **Verstehen Sie die asynchrone Programmierung für apps der universellen Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Zugriffsberechtigungen für den Speicherort**

    Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

## <a name="filesavepicker-step-by-step"></a>FileSavePicker: Schritt für Schritt

Verwenden Sie [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871), damit Benutzer den Namen, Typ und Speicherort von zu speichernden Dateien angeben können. Erstellen Sie ein Dateiauswahlobjekt, passen und zeigen Sie es an, und speichern Sie dann Daten über das zurückgegebene [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt, das die ausgewählte Datei darstellt.

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

Legen Sie Eigenschaften für das Dateiauswahlobjekt fest, die für Ihre Benutzer und Ihre App relevant sind. In diesem Beispiel werden drei Eigenschaften festgelegt: [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880), [ **FileTypeChoices** ](https://msdn.microsoft.com/library/windows/apps/br207875) und [ **SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878).
     
- Da der Benutzer in unserem Fall ein Dokument oder eine Textdatei speichert, wird im Beispiel [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880) auf den lokalen Ordner der App mithilfe von [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) festgelegt. Legen Sie [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) auf einen Speicherort fest, der dem Typ der gespeicherten Datei entspricht, z. B. Musik, Bilder, Videos oder Dokumente. Der Benutzer kann vom Ausgangspfad aus zu anderen Speicherorten navigieren.

- Da wir sicherstellen möchten, dass unsere App die Datei nach dem Speichern öffnen kann, verwenden wir im Beispiel [**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875) für die Angabe der vom Beispiel unterstützten Dateitypen (Microsoft Word-Dokumente und Textdateien). Stellen Sie sicher, dass alle von Ihnen angegebenen Dateitypen von Ihrer App unterstützt werden. Die Benutzer können ihre Datei unter einem beliebigen der von Ihnen angegebenen Dateitypen speichern. Sie können auch den Dateityp ändern, indem sie einen anderen von Ihnen angegebenen Dateityp auswählen. Der erste Dateityp in der Liste ist standardmäßig ausgewählt. Legen Sie zum Steuern dieses Verhaltens die [**DefaultFileExtension**](https://msdn.microsoft.com/library/windows/apps/br207873)-Eigenschaft fest.

    > [!NOTE]
    > Die Dateiauswahl verwendet auch den Typ der aktuell ausgewählten Datei, welche Dateien, die angezeigt wird, filtern, sodass nur die Dateitypen, die die ausgewählten Dateitypen entsprechen, die dem Benutzer angezeigt werden.

- Um dem Benutzer einige Eingaben zu ersparen, legt das Beispiel einen [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878) fest. Verwenden Sie als vorgeschlagenen Dateinamen einen Namen, der für die gespeicherte Datei relevant ist. Beispielsweise können Sie wie in Word den vorhandenen Dateinamen vorschlagen, sofern vorhanden. Sie können auch die erste Zeile eines Dokuments vorschlagen, wenn der Benutzer eine noch nicht benannte Datei speichert.

> [!NOTE]
> [**FileSavePicker** ](https://msdn.microsoft.com/library/windows/apps/br207871) Objekte anzuzeigen, die Auswahl mithilfe der [ **PickerViewMode.List** ](https://msdn.microsoft.com/library/windows/apps/br207891) sichtmodus befinden.

2.  **Die FileSavePicker anzeigen und speichern in der ausgewählten Datei**

    Zeigen Sie die Dateiauswahl durch Aufrufen von [**PickSaveFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207876) an. Nachdem die Benutzer den Namen, Dateityp und Speicherort angegeben und bestätigt haben, dass die Datei gespeichert wird, gibt **PickSaveFileAsync** ein [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)-Objekt zurück, das die gespeicherte Datei darstellt. Sie können diese Datei erfassen und verarbeiten, da sie jetzt über Lese- und Schreibzugriff verfügen.

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
