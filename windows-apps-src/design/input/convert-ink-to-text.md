---
Description: Verwenden Sie die Handschrifterkennung und die Handschrift Analyse, um Windows-Hand Striche als Text und Formen zu erkennen
title: Erkennen von Windows Ink-Strichen als Text und Formen
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, Handschrifterkennung, Benutzerinteraktion, Eingabe
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d496051a066ffcf9e8df5d4798415e6089cad4d1
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970915"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Erkennen von Windows Ink-Strichen als Text und Formen

Konvertieren von Hand Strichen in Text und Formen mithilfe der in Windows Ink integrierten Erkennungsfunktionen.

> **Wichtige APIs**: [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), [**Windows. UI. Input. Inking**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)

## <a name="free-form-recognition-with-ink-analysis"></a>Frei Form Erkennung mit Ink-Analyse

Hier veranschaulichen wir, wie Sie mit der Windows Ink-Analyse-Engine ([Windows. UI. Input. Inking. Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)) einen Satz von frei Form Strichen in einem [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) als Text oder als Formen klassifizieren, analysieren und erkennen. (Zusätzlich zur Text-und Form Erkennung kann die frei Hand Analyse auch zum Erkennen von Dokumentstruktur, Aufzählungs Listen und generischen Zeichnungen verwendet werden.)

> [!NOTE]
> Grundlegende, einzeilige, nur-Text-Szenarios wie z. b. Formular Eingaben finden Sie weiter unten in diesem Thema unter [Eingeschränkte Handschrifterkennung](#constrained-handwriting-recognition) .

In diesem Beispiel wird die Erkennung initiiert, wenn der Benutzer auf eine Schaltfläche klickt, um anzugeben, dass das Zeichnen abgeschlossen ist.

**Herunterladen dieses Beispiels aus dem [Ink-Analyse Beispiel (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)**

1. Zuerst richten wir die Benutzeroberfläche ("MainPage. XAML") ein. 

   Die Benutzeroberfläche enthält die Schaltfläche "erkennen", eine [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)und eine Standard [**Canvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas). Wenn die Schaltfläche "erkennen" gedrückt wird, werden alle frei Hand Eingaben auf dem frei Handzeichen Bereich analysiert und (sofern erkannt) die entsprechenden Formen und Texte werden auf der Standard Canvas gezeichnet. Die ursprünglichen Hand Eingaben werden dann aus dem frei Handzeichen Bereich gelöscht.

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

2. Fügen Sie in der Code Behind-Datei der Benutzeroberfläche (MainPage.XAML.cs) die Namespace-Typverweise hinzu, die für unsere frei Hand-und frei Hand Analysefunktion erforderlich sind
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)
    - [Windows. UI. Input. Inking. Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes)

3. Anschließend geben wir unsere globalen Variablen an:

   ```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
   ```

4. Als nächstes legen wir einige grundlegende Freihand-Eingabe Verhalten fest:
    - Der [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) ist so konfiguriert, dass die Eingabedaten von Stift, Maus und Fingereingabe als frei Hand Striche interpretiert werden ([**inputenvicetypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). 
    - Freihandstriche werden in der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Klasse mit der angegebenen [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class)-Klasse gerendert. 
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

5. In diesem Beispiel führen wir die Handschrift Analyse im Click-Ereignishandler der Schaltfläche "erkennen" aus.
    - Aufrufen Sie zuerst [**getstrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) im [**strokecontainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) von [**InkCanvas. InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) , um die Auflistung aller aktuellen Freihand Striche abzurufen.
    - Wenn Freihand Striche vorhanden sind, übergeben Sie Sie an den [**adddataforstrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) -Befehl von InkAnalyzer.
    - Wir versuchen, sowohl Zeichnungen als auch Text zu erkennen, Sie können jedoch die [**setstrokedatakind**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) -Methode verwenden, um anzugeben, ob Sie nur an Text (einschließlich Dokumentstruktur und Aufzählungs Listen) interessiert sind oder nur in Zeichnungen (einschließlich Formen Erkennung).
    - Aufrufen von [**analyzeasync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) , um die frei Hand Analyse zu initiieren und das [**inkanalysisresult**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)abzurufen.
    - Wenn [**Status den Status**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) **aktualisiert**zurückgibt, müssen Sie [**FindNodes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) sowohl für [**inkanalysisnodekind. InkWord**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) als auch für [**inkanalysisnodekind. InkDrawing**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind)aufzurufen.
    - Iterieren Sie beide Sätze von Knoten Typen, und zeichnen Sie den entsprechenden Text oder die entsprechende Form auf dem Erkennungszeichen Bereich (unterhalb des frei Handzeichen Bereichs).
    - Löschen Sie abschließend die erkannten Knoten aus dem InkAnalyzer und die entsprechenden Handschrift Striche aus dem frei Handzeichen Bereich.

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

6. Hier ist die Funktion zum Zeichnen eines TextBlock in unserem Erkennungszeichen Bereich. Wir verwenden das umgebende Rechteck des zugeordneten frei Hand Strichs im frei Handzeichen Bereich, um die Position und den Schrift Grad des TextBlock festzulegen.

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

7. Hier sind die Funktionen zum Zeichnen von Ellipsen und Polygonen in unserem Erkennungszeichen Bereich aufgeführt. Wir verwenden das umgebende Rechteck des zugeordneten frei Hand Strichs im frei Handzeichen Bereich, um die Position und den Schrift Grad der Formen festzulegen.

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

Hier ist dieses Beispiel in Aktion:

| Vor der Analyse | Nach der Analyse |
| --- | --- |
| ![Vor der Analyse](images/ink/ink-analysis-raw2-small.png) | ![Nach der Analyse](images/ink/ink-analysis-analyzed2-small.png) |

## <a name="constrained-handwriting-recognition"></a>Eingeschränkte Handschrifterkennung

Im vorherigen Abschnitt ([frei Form Erkennung bei der frei](#free-form-recognition-with-ink-analysis)Hand Eingabe Analyse) haben wir gezeigt, wie die frei Hand Eingabe- [APIs](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis) verwendet werden, um willkürliche Hand Striche in einem InkCanvas-Bereich zu analysieren und zu erkennen.

In diesem Abschnitt wird veranschaulicht, wie die Windows-Handschrift Erkennungs-Engine (keine frei Hand Analyse) verwendet wird, um einen Satz von Strichen in einem [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) in Text umzuwandeln (basierend auf dem installierten Standard Sprachpaket).

> [!NOTE]
> Die in diesem Abschnitt gezeigte grundlegende Handschrifterkennung eignet sich am besten für einzeilige, Texteingabe Szenarien wie Formular Eingaben. Informationen zu komplexeren Erkennungs Szenarien, die Analyse und Interpretation der Dokumentstruktur, Listenelemente, Formen und Zeichnungen enthalten (zusätzlich zur Texterkennung), finden Sie im vorherigen Abschnitt: [frei Form Erkennung mit Ink-Analyse](#free-form-recognition-with-ink-analysis).

In diesem Beispiel wird die Erkennung initiiert, wenn der Benutzer auf eine Schaltfläche klickt, um anzugeben, dass der Schreibvorgang abgeschlossen ist.

**Dieses Beispiel aus dem [Handschrift Erkennungs Beispiel](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip) herunterladen**

1. Zuerst wird die Benutzeroberfläche eingerichtet.

   Die Benutzeroberfläche umfasst die Schaltfläche „Erkennen“, die [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Klasse und einen Bereich zum Anzeigen der Ergebnisse der Erkennung.    

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

2. In diesem Beispiel müssen Sie zuerst die Namespace-Typverweise hinzufügen, die für die Ink-Funktionalität erforderlich sind:
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)

3. Dann werden einige grundlegende Verhalten für Freihandeingaben festgelegt.

    Die [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Eigenschaft wird für die Interpretation von Eingabedaten von Stift oder Maus als Freihandstriche ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)) konfiguriert. Freihandstriche werden in der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Klasse mit der angegebenen [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class)-Klasse gerendert. Außerdem wird ein Listener für das Click-Ereignis der Schaltfläche „Erkennen“ deklariert.

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

4. Zum Schluss wird die grundlegende Schrifterkennung durchgeführt. In diesem Beispiel wird die Schrifterkennung mit dem Click-Ereignishandler der Schaltfläche „Erkennen“ ausgeführt.

   - Ein [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) speichert alle Freihandstriche in einem [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer)-Objekt. Die Freihandstriche werden durch die [**StrokeContainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer)-Eigenschaft von **InkPresenter** verfügbar gemacht und mit der [**GetStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes)-Methode abgerufen. 

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - Eine [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer)-Klasse wird erstellt, um den Schrifterkennungsprozess zu verwalten.

    ```csharp
    // Create a manager for the InkRecognizer object
        // used in handwriting recognition.
        InkRecognizerContainer inkRecognizerContainer =
            new InkRecognizerContainer();
    ```

    - " [**Erkennzeasync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) " wird aufgerufen, um einen Satz von [**inkrecognitionresult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) -Objekten abzurufen. Erkennungsergebnisse werden für jedes Wort erzeugt, das von einem [**InkRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer)erkannt wird.

    ```csharp
    // Recognize all ink strokes on the ink canvas.
        IReadOnlyList<InkRecognitionResult> recognitionResults =
            await inkRecognizerContainer.RecognizeAsync(
                inkCanvas.InkPresenter.StrokeContainer,
                InkRecognitionTarget.All);
    ```

    - Jedes [**inkrecognitionresult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) -Objekt enthält einen Satz von Text Kandidaten. Das oberste Element in dieser Liste wird von der Erkennungs-Engine als die beste Entsprechung betrachtet, gefolgt von den verbleibenden Kandidaten in der Reihenfolge der Zuverlässigkeit.

       Wir durchlaufen jedes [**inkrecognitionresult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) und kompilieren die Liste der Kandidaten. Die Kandidaten werden dann angezeigt, und der [**inkstrokecontainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) wird gelöscht (wodurch auch der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)gelöscht wird).

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

    - Hier ist das Beispiel für den Click-Handler, vollständig.

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

Die in die Windows Ink-Plattform integrierte Handschrifterkennung umfasst eine umfangreiche Teilmenge der von Windows unterstützten Gebiets Schemas und Sprachen.

Eine Liste der Sprachen, die von [**InkRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer) unterstützt werden, finden Sie im Thema [**InkRecognizer.Name**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizer.name) -Eigenschaft.

Ihre APP kann den Satz installierter Handschrifterkennungs-Engines Abfragen und eine dieser Anwendungen verwenden oder einen Benutzer für die bevorzugte Sprache auswählen.

**Hinweis**    Benutzer können eine Liste der installierten Sprachen anzeigen, indem Sie zu **Einstellungen&gt; -Zeit & Sprache**wechseln. Die installierten Sprachen werden unter **Sprachen** aufgeführt.

So installieren Sie ein neues Sprachpaket und aktivieren die Schrifterkennung für die Sprache

1. Öffnen Sie **Einstellungen &gt; Zeit & Sprache &gt; Region & Sprache**.
2. Wählen Sie **Sprache hinzufügen** aus.
3. Wählen Sie eine Sprache aus der Liste und dann die Regionsversion aus. Die Sprache wird jetzt auf der Seite **Region & Sprache** aufgeführt.
4. Klicken Sie auf die Sprache, und wählen Sie **Optionen** aus.
5. Laden Sie auf der Seite **Sprachoptionen** das **Schrifterkennungsmodul** herunter. (Sie können auch das vollständige Sprachpaket, das Spracherkennungsmodul und das Tastaturlayout hier herunterladen.)

Hier wird gezeigt, wie auf Grundlage der ausgewählten Erkennung mit dem Schrifterkennungsmodul eine Reihe von Freihandstrichen in einer [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Klasse interpretiert werden.

Die Erkennung wird durch den Benutzer initiiert, indem er nach Abschluss des Schreibvorgangs auf eine Schaltfläche klickt.

1. Zuerst wird die Benutzeroberfläche eingerichtet.

   Die Benutzeroberfläche umfasst die Schaltfläche „Erkennen“, ein Kombinationsfeld mit allen installierten Schrifterkennungen, den [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) und einen Bereich zum Anzeigen der Ergebnisse der Erkennung.

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

2. Dann werden einige grundlegende Verhalten für Freihandeingaben festgelegt.

   Die [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Eigenschaft wird für die Interpretation von Eingabedaten von Stift oder Maus als Freihandstriche ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)) konfiguriert. Freihandstriche werden in der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Klasse mit der angegebenen [**InkDrawingAttributes**](https://docs.microsoft.com/windows/desktop/tablet/inkdrawingattributes-class)-Klasse gerendert.

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

3. Das Kombinationsfeld für die Erkennung wird mit einer Liste der installierten Schrifterkennungen aufgefüllt.

   Eine [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer)-Klasse wird erstellt, um den Schrifterkennungsprozess zu verwalten. Rufen Sie mit diesem Objekt [**GetRecognizers**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizercontainer.getrecognizers). Rufen Sie dann die Liste der installierten Erkennungen ab, um das Kombinationsfeld für die Erkennung aufzufüllen.

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
    
4. Aktualisieren Sie die Handschrifterkennung, wenn die Auswahl im Kombinationsfeld für die Erkennung geändert wird.

   Rufen Sie mit [**InkRecognizerContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) die [**SetDefaultRecognizer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkrecognizercontainer.setdefaultrecognizer)-Methode auf Grundlage der ausgewählten Erkennung im Kombinationsfeld für die Erkennung auf.

    ```csharp
    // Handle recognizer change.
        private void comboInstalledRecognizers_SelectionChanged(
            object sender, SelectionChangedEventArgs e)
        {
            inkRecognizerContainer.SetDefaultRecognizer(
                (InkRecognizer)comboInstalledRecognizers.SelectedItem);
        }
    ```

5. Abschließend wird die Schrifterkennung auf Grundlage der ausgewählten Erkennung für die Handschrift ausgeführt. In diesem Beispiel wird die Schrifterkennung mit dem Click-Ereignishandler der Schaltfläche „Erkennen“ ausgeführt.

   - Ein [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) speichert alle Freihandstriche in einem [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer)-Objekt. Die Freihandstriche werden durch die [**StrokeContainer**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer)-Eigenschaft von **InkPresenter** verfügbar gemacht und mit der [**GetStrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes)-Methode abgerufen.

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - " [**Erkennzeasync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) " wird aufgerufen, um einen Satz von [**inkrecognitionresult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) -Objekten abzurufen.

      Erkennungsergebnisse werden für jedes Wort erzeugt, das von einem [**InkRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognizer)erkannt wird.

    ```csharp
    // Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
    ```

    - Jedes [**inkrecognitionresult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) -Objekt enthält einen Satz von Text Kandidaten. Das oberste Element in dieser Liste wird von der Erkennungs-Engine als die beste Entsprechung betrachtet, gefolgt von den verbleibenden Kandidaten in der Reihenfolge der Zuverlässigkeit.

       Wir durchlaufen jedes [**inkrecognitionresult**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) und kompilieren die Liste der Kandidaten. Die Kandidaten werden dann angezeigt, und der [**inkstrokecontainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) wird gelöscht (wodurch auch der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)gelöscht wird).

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

    - Hier ist das Beispiel für den Click-Handler, vollständig.

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

In den vorherigen beiden Beispielen ist es jedoch erforderlich, dass der Benutzer eine Schaltfläche zum Starten der Erkennung drückt. Sie können auch dynamische Erkennung mithilfe von Strich Eingaben durchführen, die mit einer einfachen Zeit Steuerungsfunktion gekoppelt sind.

In diesem Beispiel werden die gleichen Einstellungen für Benutzeroberfläche und Freihandstriche verwendet wie im Beispiel für internationale Erkennung oben.

1. Diese globalen Objekte ("[InkAnalyzer](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer)", " [inkstroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke)", " [inkanalysisresult](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)", " [DispatcherTimer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer)") werden in der gesamten App verwendet.

    ```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
    ```

2. Anstelle einer Schaltfläche zum Initiieren der Erkennung werden Listener für zwei [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Freihandstrichereignisse ([**StrokesCollected**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected) und [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)) hinzugefügt. Zudem wird ein einfacher Timer ([**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer)) mit einem [**Tick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer.tick)-Intervall von einer Sekunde eingerichtet.

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

3. Anschließend werden die Handler für die InkPresenter-Ereignisse definiert, die im ersten Schritt deklariert wurden (wir überschreiben auch das [**OnNavigatingFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) -Seiten Ereignis, um den Timer zu verwalten).

    - [**StrokesCollected**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected)  
    Fügen Sie dem InkAnalyzer Ink-Striche ([**adddataforstrokes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) hinzu, und starten Sie den Erkennungs Timer, wenn der Benutzer die Eingabe beendet (indem Sie den Stift oder Finger heben oder die Maustaste loslassen). Nach einer Sekunde ohne Freihandeingabe wird die Spracherkennung initiiert.  

        Verwenden Sie die [**setstrokedatakind**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) -Methode, um anzugeben, ob Sie nur an Text (einschließlich Dokumentstruktur-AMD-Aufzählungs Listen) oder nur in Zeichnungen (inlcuding-Form Erkennung) interessiert sind.

    - [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)  
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

4. Zum Schluss führen wir die Handschrifterkennung aus. In diesem Beispiel wird zum Initiieren der Schrifterkennung der [**Tick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer.tick)-Ereignishandler einer [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer)-Klasse verwendet.
    - Aufrufen von [**analyzeasync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) , um die frei Hand Analyse zu initiieren und das [**inkanalysisresult**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)abzurufen.
    - Wenn [**der Status**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) "aktualisiert" den Status " **aktualisiert**" zurückgibt, wird [**FindNodes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) für Knoten Typen von " [**inkanalysisnodekind. InkWord**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind)" aufgerufen.
    - Iterieren Sie die Knoten, und zeigen Sie den erkannten Text an.
    - Löschen Sie abschließend die erkannten Knoten aus dem InkAnalyzer und die entsprechenden Handschrift Striche aus dem frei Handzeichen Bereich.

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

- [Zeichen- und Eingabestiftinteraktionen](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Themenbeispiele

- [Ink-Analyse-Beispiel (Standard) (c#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
- [Handschrift Erkennungs Beispiel (c#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

### <a name="other-samples"></a>Weitere Beispiele

- [Simple Ink Sample (c#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Beispiel für Complex Ink (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink-Beispiel (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Tutorial zu den ersten Schritten: Unterstützung von frei Hand Eingaben in Ihrer Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Malbuchbeispiel](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Familiennotizbeispiel](https://github.com/Microsoft/Windows-appsample-familynotes)
