---
author: Karl-Bridge-Microsoft
Description: UWP apps that support Windows Ink can serialize and deserialize ink strokes to an Ink Serialized Format (ISF) file. The ISF file is a GIF image with additional metadata for all ink stroke properties and behaviors. Apps that are not ink-enabled, can view the static GIF image, including alpha-channel background transparency.
title: Speichern und Abrufen der Daten zu Windows Ink-Strichen
ms.assetid: C96C9D2F-DB69-4883-9809-4A0DF7CEC506
label: Store and retrieve Windows Ink stroke data
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, ISF, serialisiertes Freihandformat, Benutzerinteraktion, Eingabe
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 946e8421cf21c37c929e7a9f9687117c3d7aca92
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "6183548"
---
# <a name="store-and-retrieve-windows-ink-stroke-data"></a>Speichern und Abrufen der Daten zu Windows Ink-Strichen


UWP-Apps, die Windows Ink unterstützen, können Freihandstriche in eine serialisierte Freihandformatdatei (Ink Serialized Format-Datei, ISF-Datei) serialisieren und aus dieser deserialisieren. Die ISF-Datei ist eine GIF-Bild mit zusätzlichen Metadaten für alle Eigenschaften und Verhaltensweisen von Freihandstrichen. Apps ohne Unterstützung der Freihandeingabe können das statische GIF-Bild, einschließlich Alphakanal-Hintergrundtransparenz, anzeigen.

> [!NOTE]
> Bei ISF handelt es sich um die kompakteste permanente Freihanddarstellung. Sie kann in ein binäres Dokumentformat, z.B. in eine GIF-Datei, eingebettet oder direkt in der Zwischenablage platziert werden.

> **Wichtige APIs**: [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)

## <a name="save-ink-strokes-to-a-file"></a>Speichern von Freihandstrichen in einer Datei

Hier wird veranschaulicht, wie Sie in einem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement gezeichnete Freihandstriche speichern.

**Laden Sie dieses Beispiel unter [Speichern und Laden von Freihandstrichen aus einer ISF-Datei (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip) herunter.**

1.  Zuerst wird die Benutzeroberfläche eingerichtet.

    Die Benutzeroberfläche enthält die Schaltflächen „Speichern“, „Laden“ und „Löschen“ sowie den [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  Dann werden einige grundlegende Verhalten für Freihandeingaben festgelegt.

    Der [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) ist so konfiguriert, dass die Eingabedaten von Stift und Maus als Freihandstriche ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)) interpretiert werden, und es werden Listener für die Klickereignisse der Schaltflächen deklariert.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  Schließlich werden die Freihanddaten im Click-Ereignishandler der Schaltfläche **Speichern** gespeichert.

    Mit einem [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) kann der Benutzer die Datei und den Speicherort der Freihanddaten auswählen.

    Wenn eine Datei ausgewählt ist, wird ein auf [**ReadWrite**](https://msdn.microsoft.com/library/windows/apps/br241731) festgelegter [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241635)-Datenstrom geöffnet.

    Anschließend wird [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/br242114) aufgerufen, um die vom [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) verwalteten Freihandstriche in den Datenstrom zu serialisieren.

```csharp
// Save ink data to a file.
    private async void btnSave_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Strokes present on ink canvas.
        if (currentStrokes.Count > 0)
        {
            // Let users choose their ink file using a file picker.
            // Initialize the picker.
            Windows.Storage.Pickers.FileSavePicker savePicker = 
                new Windows.Storage.Pickers.FileSavePicker();
            savePicker.SuggestedStartLocation = 
                Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
            savePicker.FileTypeChoices.Add(
                "GIF with embedded ISF", 
                new List<string>() { ".gif" });
            savePicker.DefaultFileExtension = ".gif";
            savePicker.SuggestedFileName = "InkSample";

            // Show the file picker.
            Windows.Storage.StorageFile file = 
                await savePicker.PickSaveFileAsync();
            // When chosen, picker returns a reference to the selected file.
            if (file != null)
            {
                // Prevent updates to the file until updates are 
                // finalized with call to CompleteUpdatesAsync.
                Windows.Storage.CachedFileManager.DeferUpdates(file);
                // Open a file stream for writing.
                IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
                // Write the ink strokes to the output stream.
                using (IOutputStream outputStream = stream.GetOutputStreamAt(0))
                {
                    await inkCanvas.InkPresenter.StrokeContainer.SaveAsync(outputStream);
                    await outputStream.FlushAsync();
                }
                stream.Dispose();

                // Finalize write so other apps can update file.
                Windows.Storage.Provider.FileUpdateStatus status =
                    await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);

                if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
                {
                    // File saved.
                }
                else
                {
                    // File couldn't be saved.
                }
            }
            // User selects Cancel and picker returns null.
            else
            {
                // Operation cancelled.
            }
        }
    }
```

> [!NOTE]
> Zum Speichern von Freihanddaten wird nur das Format GIF unterstützt. Aus Gründen der Abwärtskompatibilität werden allerdings von der [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607)-Methode (im nächsten Abschnitt veranschaulicht) weitere Formate unterstützt.

## <a name="load-ink-strokes-from-a-file"></a>Laden von Freihanddaten aus einer Datei

Hier wird veranschaulicht, wie Freihandstriche aus einer Datei geladen und in einem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement gerendert werden.

**Laden Sie dieses Beispiel unter [Speichern und Laden von Freihandstrichen aus einer ISF-Datei (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip) herunter.**

1.  Zuerst wird die Benutzeroberfläche eingerichtet.

    Die Benutzeroberfläche enthält die Schaltflächen „Speichern“, „Laden“ und „Löschen“ sowie den [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  Dann werden einige grundlegende Verhalten für Freihandeingaben festgelegt.

    Der [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) ist so konfiguriert, dass die Eingabedaten von Stift und Maus als Freihandstriche ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)) interpretiert werden, und es werden Listener für die Klickereignisse der Schaltflächen deklariert.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  Schließlich werden die Freihanddaten im Click-Ereignishandler der Schaltfläche **Laden** geladen.

    Mit einem [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) kann der Benutzer die Datei und den Speicherort auswählen, aus der bzw. dem die gespeicherten Freihanddaten abgerufen werden.

    Wenn eine Datei ausgewählt ist, wird ein auf [**Read**](https://msdn.microsoft.com/library/windows/apps/br241635) festgelegter [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)-Datenstrom geöffnet.

    Anschließend wird [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607) aufgerufen, um die gespeicherten Freihandstriche zu lesen, zu serialisieren und in den [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) zu laden. Das Laden der Striche in den **InkStrokeContainer** bewirkt, dass sie sofort vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) auf dem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) gerendert werden.

    > [!NOTE]
    > Alle vorhandenen Striche im InkStrokeContainer werden gelöscht, bevor neue Striche geladen werden.

``` csharp
// Load ink data from a file.
private async void btnLoad_Click(object sender, RoutedEventArgs e)
{
    // Let users choose their ink file using a file picker.
    // Initialize the picker.
    Windows.Storage.Pickers.FileOpenPicker openPicker =
        new Windows.Storage.Pickers.FileOpenPicker();
    openPicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    openPicker.FileTypeFilter.Add(".gif");
    // Show the file picker.
    Windows.Storage.StorageFile file = await openPicker.PickSingleFileAsync();
    // User selects a file and picker returns a reference to the selected file.
    if (file != null)
    {
        // Open a file stream for reading.
        IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        // Read from file.
        using (var inputStream = stream.GetInputStreamAt(0))
        {
            await inkCanvas.InkPresenter.StrokeContainer.LoadAsync(inputStream);
        }
        stream.Dispose();
    }
    // User selects Cancel and picker returns null.
    else
    {
        // Operation cancelled.
    }
}
```

> [!NOTE]
> Zum Speichern von Freihanddaten wird nur das Format GIF unterstützt. Aus Gründen der Abwärtskompatibilität werden allerdings von der [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607)-Methode die folgenden Formate unterstützt.

| Format                    | Beschreibung |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InkSerializedFormat       | Gibt Freihandeingaben an, die mithilfe des ISF beibehalten werden. Dabei handelt es sich um die kompakteste permanente Freihanddarstellung. Sie kann in ein binäres Dokumentformat eingebettet oder direkt in der Zwischenablage platziert werden.                                                                                                                                                                                                         |
| Base64InkSerializedFormat | Gibt Freihandeingaben an, die durch Codieren von ISF als Base64-Datenstrom beibehalten werden. Dieses Format wird bereitgestellt, damit Freihandeingaben direkt in einer XML-Datei oder HTML-Datei codiert werden können.                                                                                                                                                                                                                                                |
| Gif                       | Dieses Format definiert Freihandeingaben, die mithilfe einer GIF-Datei beibehalten werden, die ISF in Form von Metadaten enthält, die in die Datei eingebettet sind. So ist es möglich, Freihandeingaben in Anwendungen anzuzeigen, die nicht über die Funktion zur Freihandeingabe verfügen, während die Eingaben beim Wechsel zu einer Anwendung mit Freihandeingabe in vollem Umfang erhalten bleiben. Das Format ermöglicht nicht nur die Verwendung in Anwendungen mit und ohne Freihandfunktion, sondern ist bestens geeignet für den Transport von Freihandinhalt innerhalb einer HTML-Datei. |
| Base64Gif                 | Dieses Format definiert Freihandeingaben, die mithilfe einer Base64-codierten verstärkten GIF-Datei beibehalten werden. Dieses Format wird bereitgestellt, wenn Freihandeingaben direkt in einer XML- oder HTML-Datei codiert werden müssen, um sie später in ein Bild zu konvertieren. Dieses Format kann beispielsweise in einem XML-Format verwendet werden, das erstellt wurde, um alle Freihandinformationen zu speichern, und das verwendet wird, um HTML über XSLT (Extensible Stylesheet Language Transformations) zu erstellen. 

## <a name="copy-and-paste-ink-strokes-with-the-clipboard"></a>Kopieren und Einfügen von Freihandstrichen mit der Zwischenablage

Hier wird veranschaulicht, wie Sie mit der Zwischenablage Freihandstriche zwischen Apps übertragen.

Damit die Zwischenablagefunktion unterstützt wird, müssen für die integrierten Befehle zum Ausschneiden und Kopieren von [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)-Inhalten ein oder mehrere Freihandstriche ausgewählt sein.

In diesem Beispiel wird die Strichauswahl aktiviert, wenn die Eingabe mit einer Zeichenstift-Drucktaste (oder der rechten Maustaste) geändert wird. Ein vollständiges Beispiel für das Implementieren der Strichauswahl finden Sie unter „Weitergabe der Eingabe für die erweiterte Verarbeitung“ in [Zeichen- und Eingabestiftinteraktionen](pen-and-stylus-interactions.md).

**Laden Sie dieses Beispiel unter [Speichern und Laden von Freihandstrichen aus der Zwischenablage](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip) herunter.**

1.  Zuerst wird die Benutzeroberfläche eingerichtet.

    Die Benutzeroberfläche enthält die Schaltflächen „Ausschneiden“, „Kopiere“, „Einfügen“ und „Löschen“ sowie den [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) und einen Auswahlzeichenbereich.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="tbHeader" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnCut" 
                    Content="Cut" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnCopy" 
                    Content="Copy" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnPaste" 
                    Content="Paste" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="20,0,10,0"/>
        </StackPanel>
        <Grid x:Name="gridCanvas" Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  Dann werden einige grundlegende Verhalten für Freihandeingaben festgelegt.

    Die [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft wird für die Interpretation von Eingabedaten von Stift oder Maus als Freihandstriche ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)) konfiguriert. Hier werden außerdem Listener für die Click-Ereignisse der Schaltflächen sowie Zeiger- und Strichereignisse für die Auswahlfunktion deklariert.

    Ein vollständiges Beispiel für das Implementieren der Strichauswahl finden Sie unter „Weitergabe der Eingabe für die erweiterte Verarbeitung“ in [Zeichen- und Eingabestiftinteraktionen](pen-and-stylus-interactions.md).
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to cut ink strokes.
        btnCut.Click += btnCut_Click;
        // Listen for button click to copy ink strokes.
        btnCopy.Click += btnCopy_Click;
        // Listen for button click to paste ink strokes.
        btnPaste.Click += btnPaste_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;

        // By default, the InkPresenter processes input modified by 
        // a secondary affordance (pen barrel button, right mouse 
        // button, or similar) as ink.
        // To pass through modified input to the app for custom processing 
        // on the app UI thread instead of the background ink thread, set 
        // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
        inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
            InkInputRightDragAction.LeaveUnprocessed;

        // Listen for unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
            UnprocessedInput_PointerPressed;
        inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
            UnprocessedInput_PointerMoved;
        inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
            UnprocessedInput_PointerReleased;

        // Listen for new ink or erase strokes to clean up selection UI.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            StrokeInput_StrokeStarted;
        inkCanvas.InkPresenter.StrokesErased +=
            InkPresenter_StrokesErased;
    }
```

3.  Nachdem die Unterstützung für die Strichauswahl hinzugefügt wurde, wird schließlich die Zwischenablagefunktion in den Click-Ereignishandlern der Schaltflächen **Ausschneiden**, **Kopieren** und **Einfügen** implementiert.

    Für die Schaltfläche „Ausschneiden“ wird zunächst [**CopySelectedToClipboard**](https://msdn.microsoft.com/library/windows/apps/br244232) für den [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) des [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) aufgerufen.

    Anschließend wird [**DeleteSelected**](https://msdn.microsoft.com/library/windows/apps/br244233) aufgerufen, um die Striche aus dem Freihandeingabe-Zeichenbereich zu entfernen.

    Schließlich werden alle Auswahlstriche aus dem Auswahlzeichenbereich gelöscht.
    
```csharp
private void btnCut_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
        ClearSelection();
    }
```
```csharp
// Clean up selection UI.
    private void ClearSelection()
    {
        var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        foreach (var stroke in strokes)
        {
            stroke.Selected = false;
        }
        ClearDrawnBoundingRect();
    }

    private void ClearDrawnBoundingRect()
    {
        if (selectionCanvas.Children.Any())
        {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
        }
    }
```

Zum Kopieren wird einfach [**CopySelectedToClipboard**](https://msdn.microsoft.com/library/windows/apps/br244232) für den [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) des [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) aufgerufen.


```csharp
private void btnCopy_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
    }
```

Zum Einfügen wird [**CanPasteFromClipboard**](https://msdn.microsoft.com/library/windows/apps/br208495) aufgerufen, um sicherzustellen, dass der Inhalt in der Zwischenablage in den Freihandeingabe-Zeichenbereich eingefügt werden kann.

Ist dies der Fall, wird [**PasteFromClipboard**](https://msdn.microsoft.com/library/windows/apps/br208503) aufgerufen, um die Freihandstriche aus der Zwischenablage in den [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) des [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) einzufügen, der die Striche dann auf dem Zeichenbereich für die Freihandeingabe rendert.

```csharp
private void btnPaste_Click(object sender, RoutedEventArgs e)
    {
        if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
        {
            inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                new Point(0, 0));
        }
        else
        {
            // Cannot paste from clipboard.
        }
    }
```

## <a name="related-articles"></a>Verwandte Artikel

* [Zeichen- und Eingabestiftinteraktionen](pen-and-stylus-interactions.md)

**Themenbeispiele**
* [Speichern und Laden von Freihandstrichen aus einer ISF-Datei (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Speichern und Laden von Freihandstrichen aus der Zwischenablage](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)

**Andere Beispiele**
* [Einfaches Freihandbeispiel (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Komplexes Freihandbeispiel (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Freihandbeispiel (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Lernprogramm „Erste Schritte:” Unterstützen von Freihandeingaben in Ihrer UWP-App](https://aka.ms/appsample-ink)
* [Malbuchbeispiel](https://aka.ms/cpubsample-coloringbook)
* [Familiennotizbeispiel](https://aka.ms/cpubsample-familynotessample)



