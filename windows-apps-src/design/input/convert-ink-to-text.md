---
Description: Nutzen Sie die Handschrifterkennung und Freihandeingabenanalyse, um Windows Ink-Striche als Text und Formen zu erkennen.
title: Erkennen von Windows Ink-Strichen als Text und Formen
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, Handschrifterkennung, Benutzerinteraktion, Eingabe
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9bdd122f438cc9584b5e1eff2236c625adea9c2b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633745"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Erkennen von Windows Ink-Strichen als Text und Formen

Konvertieren Sie mithilfe der integrierten Erkennungsfunktionen in Windows Ink Freihandstriche in Text und Form.

> **Wichtige APIs**: [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="free-form-recognition-with-ink-analysis"></a>Freiformerkennung mit der Freihandeingabenanalyse

Hier wird veranschaulicht, wie Sie das Windows Ink-Analysemodul ([Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)) zur Klassifizierung, Analyse und Erkennung von Freiformstrichen in einer [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse als Text oder Formen verwenden können. (Zusätzlich zur Text- und Formenerkennung können Sie mithilfe der Freihandeingabenanalyse die Dokumentstruktur, Aufzählungen und generische Zeichnungen erkennen.)

> [!NOTE]
> Informationen zu einfachen, einzeilige, Nur-Text-Szenarien wie der Formulareingabe finden Sie später unter [Eingeschränkte Schrifterkennung](#constrained-handwriting-recognition).

In diesem Beispiel wird die Erkennung durch den Benutzer initiiert, wenn er nach Abschluss des Zeichnens auf eine Schaltfläche klickt.

**Laden Sie dieses Beispiel von [Ink Analysis-Beispiel (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)**

1.  Zunächst richten wir die Benutzeroberfläche ein (MainPage.xaml). 

    Die Benutzeroberfläche bietet die Schaltfläche „Erkennen“, eine [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)- und eine [**Canvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)-Standardkomponente. Bei Betätigen der Schaltfläche „Erkennen“ werden alle Freihandstriche im Freihandeingabe-Zeichenbereich analysiert. Wenn Formen und Texte erkannt werden, werden sie im Standardzeichenbereich gezeichnet. Die ursprünglichen Freihandstriche werden anschließend aus dem Freihandeingabe-Zeichenbereich gelöscht.
```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                        Text="Basic ink analysis sample" 
                        Style="{ThemeResource HeaderTextBlockStyle}" 
                        Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid x:Name="drawingCanvas" Grid.Row="1">

            <!-- The canvas where we render the replacement text and shapes. -->
            <Canvas x:Name="recognitionCanvas" />
            <!-- The canvas for ink input. -->
            <InkCanvas x:Name="inkCanvas" />

        </Grid>
    </Grid>
```
2. Fügen Sie in der CodeBehind-Datei der Benutzeroberfläche (MainPage.xaml.cs) die für die Freihandfunktion und die Freihandeingabenanalyse benötigten Namespaceverweistypen hinzu:
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)
    - [Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes)

3. Anschließend geben wir die globalen Variablen an:
```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
```
4.  Dann werden einige grundlegende Verhaltensweisen für Freihandeingaben festgelegt:
    - Die [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft wird für die Interpretation von Eingabedaten von Stift, Maus und Toucheingabe als Freihandstriche ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)) konfiguriert. 
    - Freihandstriche werden in der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse mit der angegebenen [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050)-Klasse gerendert. 
    - Außerdem wird ein Listener für das Click-Ereignis der Schaltfläche „Erkennen“ deklariert.
```csharp
/// <summary>
/// Initialize the UI page.
/// </summary>
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen | 
            Windows.UI.Core.CoreInputDeviceTypes.Touch;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += RecognizeStrokes_Click;
    }
```
5.  In diesem Beispiel wird die Freihandeingabenanalyse mit dem Click-Ereignishandler der Schaltfläche „Erkennen“ ausgeführt.
    - Rufen Sie zuerst [**GetStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) im [**StrokeContainer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) von [**InkCanvas.InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) auf, um alle aktuell erfassten Freihandstriche abzurufen.
    - Wenn Freihandstriche vorhanden sind, übergeben Sie sie in einem Aufruf an [**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) von InkAnalyzer.
    - Wir versuchen, Zeichnungen und Text zu erkennen, Sie können allerdings auch die Methode [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) verwenden, um anzugeben, ob Sie nur am Text (einschließlich der Dokumentstruktur und Aufzählungen) oder nur an den Zeichnungen (einschließlich der Formenerkennung) interessiert sind.
    - Rufen Sie [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) auf, um die Freihandeingabenanalyse zu initiieren und [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult) abzurufen.
    - Wenn unter [**Status**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) die Option **Aktualisiert** zurückgegeben wird, rufen Sie [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) für [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) und [**InkAnalysisNodeKind.InkDrawing**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) auf.
    - Durchlaufen Sie beide Sätze der Knotentypen und zeichnen Sie den entsprechenden Text oder die Form in den Erkennungszeichenbereich (unter dem Freihandeingabe-Zeichenbereich).
    - Löschen Sie dann die erkannten Knoten aus dem InkAnalyzer und die entsprechenden Freihandstriche aus dem Freihandeingabe-Zeichenbereich.
```csharp
/// <summary>
/// The "Analyze" button click handler.
/// Ink recognition is performed here.
/// </summary>
/// <param name="sender">Source of the click event</param>
/// <param name="e">Event args for the button click routed event</param>
private async void RecognizeStrokes_Click(object sender, RoutedEventArgs e)
    {
        inkStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (inkStrokes.Count > 0)
        {
            inkAnalyzer.AddDataForStrokes(inkStrokes);

            // In this example, we try to recognizing both 
            // writing and drawing, so the platform default 
            // of "InkAnalysisStrokeKind.Auto" is used.
            // If you're only interested in a specific type of recognition,
            // such as writing or drawing, you can constrain recognition 
            // using the SetStrokDataKind method as follows:
            // foreach (var stroke in strokesText)
            // {
            //     analyzerText.SetStrokeDataKind(
            //      stroke.Id, InkAnalysisStrokeKind.Writing);
            // }
            // This can improve both efficiency and recognition results.
            inkAnalysisResults = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (inkAnalysisResults.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes = 
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Draw primary recognized text on recognitionCanvas 
                // (for this example, we ignore alternatives), and delete 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    // Draw a TextBlock object on the recognitionCanvas.
                    DrawText(node.RecognizedText, node.BoundingRect);

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke = 
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();

                // Find all strokes that are recognized as a drawing and 
                // create a corresponding ink analysis InkDrawing node.
                var inkdrawingNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkDrawing);
                // Iterate through each InkDrawing node.
                // Draw recognized shapes on recognitionCanvas and
                // delete ink analysis data and recognized strokes.
                foreach (InkAnalysisInkDrawing node in inkdrawingNodes)
                {
                    if (node.DrawingKind == InkAnalysisDrawingKind.Drawing)
                    {
                        // Catch and process unsupported shapes (lines and so on) here.
                    }
                    // Process generalized shapes here (ellipses and polygons).
                    else
                    {
                        // Draw an Ellipse object on the recognitionCanvas (circle is a specialized ellipse).
                        if (node.DrawingKind == InkAnalysisDrawingKind.Circle || node.DrawingKind == InkAnalysisDrawingKind.Ellipse)
                        {
                            DrawEllipse(node);
                        }
                        // Draw a Polygon object on the recognitionCanvas.
                        else
                        {
                            DrawPolygon(node);
                        }
                        foreach (var strokeId in node.GetStrokeIds())
                        {
                            var stroke = inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                            stroke.Selected = true;
                        }
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
    }
```
6. Im Folgenden wird die Funktion zum Zeichnen eines TextBlock-Elements im Erkennungszeichenbereich beschrieben. Wir verwenden das umschließende Rechteck des Strichs zugeordneten Freihandeingaben auf der Leinwand Freihandeingaben, legen Sie die Position und Schriftartgröße des Textblocks.
```csharp
/// <summary>
/// Draw ink recognition text string on the recognitionCanvas.
/// </summary>
/// <param name="recognizedText">The string returned by text recognition.</param>
/// <param name="boundingRect">The bounding rect of the original ink writing.</param>
private void DrawText(string recognizedText, Rect boundingRect)
{
    TextBlock text = new TextBlock();
    Canvas.SetTop(text, boundingRect.Top);
    Canvas.SetLeft(text, boundingRect.Left);

    text.Text = recognizedText;
    text.FontSize = boundingRect.Height;

    recognitionCanvas.Children.Add(text);
}
```
7. Im Folgenden werden die Funktionen zum Zeichnen von Ellipsen und Polygonen im Erkennungszeichenbereich beschrieben. Wir verwenden das umschließende Rechteck des Strichs zugeordneten Freihandeingaben auf der Leinwand Freihandeingaben auf die Position und den Schriftgrad des Shapes festlegen.
```csharp
    // Draw an ellipse on the recognitionCanvas.
    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        var points = shape.Points;
        Ellipse ellipse = new Ellipse();

        ellipse.Width = shape.BoundingRect.Width;
        ellipse.Height = shape.BoundingRect.Height;

        Canvas.SetTop(ellipse, shape.BoundingRect.Top);
        Canvas.SetLeft(ellipse, shape.BoundingRect.Left);

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        ellipse.Stroke = brush;
        ellipse.StrokeThickness = 2;
        recognitionCanvas.Children.Add(ellipse);
    }

    // Draw a polygon on the recognitionCanvas.
    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        List<Point> points = new List<Point>(shape.Points);
        Polygon polygon = new Polygon();

        foreach (Point point in points)
        {
            polygon.Points.Add(point);
        }

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        polygon.Stroke = brush;
        polygon.StrokeThickness = 2;
        recognitionCanvas.Children.Add(polygon);
    }
```

Nachfolgend finden Sie das Beispiel in Aktion:

| Vor der Analyse | Nach der Analyse |
| --- | --- |
| ![Vor der Analyse](images/ink/ink-analysis-raw2-small.png) | ![Nach der Analyse](images/ink/ink-analysis-analyzed2-small.png) |

---
## <a name="constrained-handwriting-recognition"></a>Eingeschränkte Schrifterkennung

Im vorherigen Abschnitt ([Freiformerkennung mit der Freihandeingabenanalyse](#free-form-recognition-with-ink-analysis)) haben wir erklärt, wie Sie [Ink-Analyse-APIs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis) verwenden, um beliebige Freihandstriche in einem InkCanvas-Bereich zu analysieren und zu erkennen.

In diesem Abschnitt wird veranschaulicht, wie mit dem Windows Ink-Schrifterkennungsmodul (nicht der Freihandeingabenanalyse) ein Satz von Freihandstrichen in einer [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse in Text interpretiert wird (je nach standardmäßig installiertem Sprachpaket).

> [!NOTE]
> Die grundlegende Schrifterkennung, so wie in diesem Abschnitt beschrieben, eignet sich insbesondere für einfache einzeilige Nur-Text-Eingabeszenarien wie Formulareingaben. Umfangreichere Recognition-Szenarios, die Analyse und Auswertung der Struktur eines Dokuments, Auflisten von Elementen, Formen und Zeichnungen (zusätzlich zur Erkennung von sichtbarem Text) einschließen, finden Sie im vorherigen Abschnitt: [Freiform-Erkennung mit handschrifterkennung](#free-form-recognition-with-ink-analysis).

In diesem Beispiel wird die Erkennung durch den Benutzer initiiert, wenn er nach Abschluss des Schreibens auf eine Schaltfläche klickt.

**Laden Sie dieses Beispiel von [Freihandeingaben Handschrift Recognition-Beispiel](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)**

1.  Zuerst wird die Benutzeroberfläche eingerichtet.

    Die Benutzeroberfläche umfasst die Schaltfläche „Erkennen“, die [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse und einen Bereich zum Anzeigen der Ergebnisse der Erkennung.    

    ```    XAML
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="HeaderPanel"
                        Orientation="Horizontal"
                        Grid.Row="0">
                <TextBlock x:Name="Header"
                        Text="Basic ink recognition sample"
                        Style="{ThemeResource HeaderTextBlockStyle}"
                        Margin="10,0,0,0" />
                <Button x:Name="recognize"
                        Content="Recognize"
                        Margin="50,0,10,0"/>
            </StackPanel>
            <Grid Grid.Row="1">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                <InkCanvas x:Name="inkCanvas"
                        Grid.Row="0"/>
                <TextBlock x:Name="recognitionResult"
                        Grid.Row="1"
                        Margin="50,0,10,0"/>
            </Grid>
        </Grid>
    ```

2. In diesem Beispiel müssen Sie zunächst die für die Freihandfunktion benötigten Namespaceverweistypen hinzufügen:
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)


3.  Dann werden einige grundlegende Verhalten für Freihandeingaben festgelegt.

    Die [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft wird für die Interpretation von Eingabedaten von Stift oder Maus als Freihandstriche ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)) konfiguriert. Freihandstriche werden in der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse mit der angegebenen [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050)-Klasse gerendert. Außerdem wird ein Listener für das Click-Ereignis der Schaltfläche „Erkennen“ deklariert.

    ```csharp
    public MainPage()
        {
            this.InitializeComponent();

            // Set supported inking device types.
            inkCanvas.InkPresenter.InputDeviceTypes =
                Windows.UI.Core.CoreInputDeviceTypes.Mouse |
                Windows.UI.Core.CoreInputDeviceTypes.Pen;

            // Set initial ink stroke attributes.
            InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
            drawingAttributes.Color = Windows.UI.Colors.Black;
            drawingAttributes.IgnorePressure = false;
            drawingAttributes.FitToCurve = true;
            inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

            // Listen for button click to initiate recognition.
            recognize.Click += Recognize_Click;
        }
    ```

4.  Zum Schluss wird die grundlegende Schrifterkennung durchgeführt. In diesem Beispiel wird die Schrifterkennung mit dem Click-Ereignishandler der Schaltfläche „Erkennen“ ausgeführt.

    Ein [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) speichert alle Freihandstriche in einem [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)-Objekt. Die Freihandstriche werden durch die [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766)-Eigenschaft von **InkPresenter** verfügbar gemacht und mit der [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499)-Methode abgerufen.

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    Eine [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479)-Klasse wird erstellt, um den Schrifterkennungsprozess zu verwalten.

    ```csharp
    // Create a manager for the InkRecognizer object
        // used in handwriting recognition.
        InkRecognizerContainer inkRecognizerContainer =
            new InkRecognizerContainer();
    ```

    [**RecognizeAsync** ](https://msdn.microsoft.com/library/windows/apps/br208446) wird aufgerufen, um einen Satz von abrufen [ **InkRecognitionResult** ](https://msdn.microsoft.com/library/windows/apps/br208464) Objekte.

    Ergebnisse werden für jedes Wort, das von erkannt wurde, erzeugt ein [ **InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
        IReadOnlyList<InkRecognitionResult> recognitionResults =
            await inkRecognizerContainer.RecognizeAsync(
                inkCanvas.InkPresenter.StrokeContainer,
                InkRecognitionTarget.All);
    ```

    Jede [ **InkRecognitionResult** ](https://msdn.microsoft.com/library/windows/apps/br208464) Objekt enthält einen Satz von Kandidaten für Text. Das oberste Element in dieser Liste wird durch die erkennungs-Engine als beste Übereinstimmung, gefolgt von die verbliebenen Kandidaten nach sinkender vertrauen betrachtet.

    Wird durchlaufen, jede [ **InkRecognitionResult** ](https://msdn.microsoft.com/library/windows/apps/br208464) und kompilieren Sie die Liste der Kandidaten. Anschließend werden die Kandidaten angezeigt, und die [ **InkStrokeContainer** ](https://msdn.microsoft.com/library/windows/apps/br208492) deaktiviert ist (die löscht auch die [ **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

    ```csharp
    string str = "Recognition result\n";
        // Iterate through the recognition results.
        foreach (var result in recognitionResults)
        {
            // Get all recognition candidates from each recognition result.
            IReadOnlyList<string> candidates = result.GetTextCandidates();
            str += "Candidates: " + candidates.Count.ToString() + "\n";
            foreach (string candidate in candidates)
            {
                str += candidate + " ";
            }
        }
        // Display the recognition candidates.
        recognitionResult.Text = str;
        // Clear the ink canvas once recognition is complete.
        inkCanvas.InkPresenter.StrokeContainer.Clear();
    ```

    Hier ist die vollständige Beispiel Handler auf.

    ```csharp
    // Handle button click to initiate recognition.
        private async void Recognize_Click(object sender, RoutedEventArgs e)
        {
            // Get all strokes on the InkCanvas.
            IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

            // Ensure an ink stroke is present.
            if (currentStrokes.Count > 0)
            {
                // Create a manager for the InkRecognizer object
                // used in handwriting recognition.
                InkRecognizerContainer inkRecognizerContainer =
                    new InkRecognizerContainer();

                // inkRecognizerContainer is null if a recognition engine is not available.
                if (!(inkRecognizerContainer == null))
                {
                    // Recognize all ink strokes on the ink canvas.
                    IReadOnlyList<InkRecognitionResult> recognitionResults =
                        await inkRecognizerContainer.RecognizeAsync(
                            inkCanvas.InkPresenter.StrokeContainer,
                            InkRecognitionTarget.All);
                    // Process and display the recognition results.
                    if (recognitionResults.Count > 0)
                    {
                        string str = "Recognition result\n";
                        // Iterate through the recognition results.
                        foreach (var result in recognitionResults)
                        {
                            // Get all recognition candidates from each recognition result.
                            IReadOnlyList<string> candidates = result.GetTextCandidates();
                            str += "Candidates: " + candidates.Count.ToString() + "\n";
                            foreach (string candidate in candidates)
                            {
                                str += candidate + " ";
                            }
                        }
                        // Display the recognition candidates.
                        recognitionResult.Text = str;
                        // Clear the ink canvas once recognition is complete.
                        inkCanvas.InkPresenter.StrokeContainer.Clear();
                    }
                    else
                    {
                        recognitionResult.Text = "No recognition results.";
                    }
                }
                else
                {
                    Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                    await messageDialog.ShowAsync();
                }
            }
            else
            {
                recognitionResult.Text = "No ink strokes to recognize.";
            }
        }
    ```

## <a name="international-recognition"></a>Internationale Erkennung

Das Handschrifterkennungstool ist in die Windows-Freihandplattform integriert und umfasst einen Satz von Gebietsschemas und Sprachen, die von Windows unterstützt werden.

Im Abschnitt der Eigenschaften [**InkRecognizer.Name**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkrecognizer.name.aspx) finden Sie eine Liste der von [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478) unterstützten Sprachen.

Ihre App kann den Satz der installierten Schrifterkennungsmodule abfragen und eines davon verwenden, oder der Benutzer wählt die bevorzugte Sprache aus.

**Beachten Sie**    Benutzern können eine Liste der installierten Sprachen angezeigt, indem Sie zu **Einstellungen –&gt; Zeit & Sprache**. Die installierten Sprachen werden unter **Sprachen** aufgeführt.

So installieren Sie ein neues Sprachpaket und aktivieren die Schrifterkennung für die Sprache

1.  Öffnen Sie **Einstellungen &gt; Zeit & Sprache &gt; Region & Sprache**.
2.  Wählen Sie **Sprache hinzufügen** aus.
3.  Wählen Sie eine Sprache aus der Liste und dann die Regionsversion aus. Die Sprache wird jetzt auf der Seite **Region & Sprache** aufgeführt.
4.  Klicken Sie auf die Sprache, und wählen Sie **Optionen** aus.
5.  Laden Sie auf der Seite **Sprachoptionen** das **Schrifterkennungsmodul** herunter. (Sie können auch das vollständige Sprachpaket, das Spracherkennungsmodul und das Tastaturlayout hier herunterladen.)

 

Hier wird gezeigt, wie auf Grundlage der ausgewählten Erkennung mit dem Schrifterkennungsmodul eine Reihe von Freihandstrichen in einer [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse interpretiert werden.

Die Erkennung wird durch den Benutzer initiiert, indem er nach Abschluss des Schreibvorgangs auf eine Schaltfläche klickt.

1.  Zuerst wird die Benutzeroberfläche eingerichtet.

    Die Benutzeroberfläche umfasst die Schaltfläche „Erkennen“, ein Kombinationsfeld mit allen installierten Schrifterkennungen, den [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) und einen Bereich zum Anzeigen der Ergebnisse der Erkennung.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Advanced international ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <ComboBox x:Name="comboInstalledRecognizers"
                     Margin="50,0,10,0">
                <ComboBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="{Binding Name}" />
                        </StackPanel>
                    </DataTemplate>
                </ComboBox.ItemTemplate>
            </ComboBox>
            <Button x:Name="buttonRecognize"
                    Content="Recognize"
                    IsEnabled="False"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  Dann werden einige grundlegende Verhalten für Freihandeingaben festgelegt.

    Die [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft wird für die Interpretation von Eingabedaten von Stift oder Maus als Freihandstriche ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)) konfiguriert. Freihandstriche werden in der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse mit der angegebenen [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050)-Klasse gerendert.

    Zum Ausfüllen des Kombinationsfelds für die Erkennung mit einer Liste der installierten Schrifterkennungen wird eine `InitializeRecognizerList`-Funktion aufgerufen.

    Außerdem werden Listener für das Click-Ereignis auf die Schaltfläche „Erkennen“ und das Ereignis der geänderten Auswahl im Kombinationsfeld für die Erkennung deklariert.
```csharp
 public MainPage()
     {
         this.InitializeComponent();

         // Set supported inking device types.
         inkCanvas.InkPresenter.InputDeviceTypes =
             Windows.UI.Core.CoreInputDeviceTypes.Mouse |
             Windows.UI.Core.CoreInputDeviceTypes.Pen;

         // Set initial ink stroke attributes.
         InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
         drawingAttributes.Color = Windows.UI.Colors.Black;
         drawingAttributes.IgnorePressure = false;
         drawingAttributes.FitToCurve = true;
         inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

         // Populate the recognizer combo box with installed recognizers.
         InitializeRecognizerList();

         // Listen for combo box selection.
         comboInstalledRecognizers.SelectionChanged +=
             comboInstalledRecognizers_SelectionChanged;

         // Listen for button click to initiate recognition.
         buttonRecognize.Click += Recognize_Click;
     }
```

3.  Das Kombinationsfeld für die Erkennung wird mit einer Liste der installierten Schrifterkennungen aufgefüllt.

    Eine [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479)-Klasse wird erstellt, um den Schrifterkennungsprozess zu verwalten. Rufen Sie mit diesem Objekt [**GetRecognizers**](https://msdn.microsoft.com/library/windows/apps/br208480). Rufen Sie dann die Liste der installierten Erkennungen ab, um das Kombinationsfeld für die Erkennung aufzufüllen.
```csharp
// Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers =
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
```

4.  Aktualisieren Sie die Handschrifterkennung, wenn die Auswahl im Kombinationsfeld für die Erkennung geändert wird.

    Rufen Sie mit [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) die [**SetDefaultRecognizer**](https://msdn.microsoft.com/library/windows/apps/hh920328)-Methode auf Grundlage der ausgewählten Erkennung im Kombinationsfeld für die Erkennung auf.
```csharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  Abschließend wird die Schrifterkennung auf Grundlage der ausgewählten Erkennung für die Handschrift ausgeführt. In diesem Beispiel wird die Schrifterkennung mit dem Click-Ereignishandler der Schaltfläche „Erkennen“ ausgeführt.

    Ein [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) speichert alle Freihandstriche in einem [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)-Objekt. Die Freihandstriche werden durch die [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766)-Eigenschaft von **InkPresenter** verfügbar gemacht und mit der [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499)-Methode abgerufen.
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes =
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates =
            result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
    
```csharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates =
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog =
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <a name="dynamic-recognition"></a>Dynamische Erkennung

Während der Benutzer bei den beiden obigen Beispielen auf eine Schaltfläche zum Starten der Spracherkennung drücken müssen, können Sie die dynamische Erkennung mithilfe der Freihandstricheingabe mit einer einfachen Timing-Funktion ausführen.

In diesem Beispiel werden die gleichen Einstellungen für Benutzeroberfläche und Freihandstriche verwendet wie im Beispiel für internationale Erkennung oben.

1. Diese globalen Objekte ([InkAnalyzer](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [InkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult), [DispatcherTimer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer)) werden in der gesamten App verwendet.    
```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
```

2.  Anstelle einer Schaltfläche zum Initiieren der Erkennung werden Listener für zwei [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Freihandstrichereignisse ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) und [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)) hinzugefügt. Zudem wird ein einfacher Timer ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)) mit einem [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)-Intervall von einer Sekunde eingerichtet.    
```csharp
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        inkAnalyzer = new InkAnalyzer();

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = TimeSpan.FromSeconds(1);
        recoTimer.Tick += recoTimer_TickAsync;
    }
```

3.  Anschließend definieren wir die Handler für die InkPresenter-Ereignisse, die wir im ersten Schritt deklariert haben (wir überschreiben ebenfalls das [**OnNavigatingFrom**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom)-Ereignis der Seite, um den Timer zu verwalten).

    - [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    Fügen Sie dem InkAnalyzer Freihandstriche hinzu ([**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) und starten Sie den Timer für die Erkennung, wenn der Benutzer die Freihandeingabe beendet, indem er den Stift oder Finger anhebt oder die Maustaste loslässt. Nach einer Sekunde ohne Freihandeingabe wird die Spracherkennung initiiert.  

        Verwenden Sie die Methode [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind), um anzugeben, ob Sie nur am Text (einschließlich der Dokumentstruktur und Aufzählungen) oder nur an den Zeichnungen (einschließlich der Formenerkennung) interessiert sind.

    - [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    Wenn ein neuer Freihandstrich vor dem nächsten Tick-Ereignis des Timers beginnt, wird der Timer beendet, da es sich bei dem neuen Freihandstrich wahrscheinlich um die Fortsetzung der vorherigen Handschrifteingabe handelt.
```csharp
    // Handler for the InkPresenter StrokeStarted event.
    // Don't perform analysis while a stroke is in progress.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }    
    // Handler for the InkPresenter StrokesCollected event.
    // Stop the timer and add the collected strokes to the InkAnalyzer.
    // Start the recognition timer when the user stops inking (by 
    // lifting their pen or finger, or releasing the mouse button).
    // If ink input is not detected after one second, initiate recognition.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Stop();
        // If you're only interested in a specific type of recognition,
        // such as writing or drawing, you can constrain recognition 
        // using the SetStrokDataKind method, which can improve both 
        // efficiency and recognition results.
        // In this example, "InkAnalysisStrokeKind.Writing" is used.
        foreach (var stroke in args.Strokes)
        {
            inkAnalyzer.AddDataForStroke(stroke);
            inkAnalyzer.SetStrokeDataKind(stroke.Id, InkAnalysisStrokeKind.Writing);
        }
        recoTimer.Start();
    }    
    // Override the Page OnNavigatingFrom event handler to 
    // stop our timer if user leaves page.
    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        recoTimer.Stop();
    } 
```

4.  Zum Schluss wird die Schrifterkennung durchgeführt. In diesem Beispiel wird zum Initiieren der Schrifterkennung der [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)-Ereignishandler einer [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)-Klasse verwendet.
    - Rufen Sie [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) auf, um die Freihandeingabenanalyse zu initiieren und [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult) abzurufen.
    - Wenn der [**Status**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) den Zustand **Aktualisiert** zurückgibt, rufen Sie [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) für Knotentypen von [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) auf.
    - Durchlaufen Sie alle Knoten und zeigen Sie den erkannten Text an.
    - Löschen Sie dann die erkannten Knoten aus dem InkAnalyzer und die entsprechenden Freihandstriche aus dem Freihandeingabe-Zeichenbereich.
```csharp
    private async void recoTimer_TickAsync(object sender, object e)
    {
        recoTimer.Stop();
        if (!inkAnalyzer.IsAnalyzing)
        {
            InkAnalysisResult result = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (result.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Display the primary recognized text (for this example, 
                // we ignore alternatives), and then delete the 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    string recognizedText = node.RecognizedText;
                    // Display the recognition candidates.
                    recognitionResult.Text = recognizedText;

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke =
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
        else
        {
            // Ink analyzer is busy. Wait a while and try again.
            recoTimer.Start();
        }
    }
```

## <a name="related-articles"></a>Verwandte Artikel

* [Zeichen- und Eingabestiftinteraktionen](pen-and-stylus-interactions.md)

**Beispiele für Thema**
* [Beispiel "Ink Analysis" (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [Beispiel von Freihandeingaben Handschrift Recognition (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

**Weitere Beispiele**
* [Beispiel für einfache Freihandeingaben (C#/C++)](https://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Komplexe Freihand-Beispiel (C++)](https://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Freihand-Beispiel (JavaScript)](https://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Einstiegstutorial: Unterstützung von Freihandeingaben in Ihre UWP-app](https://aka.ms/appsample-ink)
* [Codefarben Book-Beispiel](https://aka.ms/cpubsample-coloringbook)
* [Anmerkungen zu dieser Version Familie-Beispiel](https://aka.ms/cpubsample-familynotessample)


 
