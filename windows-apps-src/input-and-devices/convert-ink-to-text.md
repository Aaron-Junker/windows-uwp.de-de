---
author: Karl-Bridge-Microsoft
Description: Konvertieren Sie letzte Striche mit der Schrifterkennung in Text oder mit der benutzerdefinierten Erkennung in Formen.
title: Erkennen von Windows Ink-Strichen als Text
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, Handschrifterkennung
translationtype: Human Translation
ms.sourcegitcommit: 0f7f54c5c5baccdedfe32bc7c71994e43a93f032
ms.openlocfilehash: c402843cce752a54d8ff7301d892ef8014350a07

---

# <a name="recognize-windows-ink-strokes-as-text"></a>Erkennen von Windows Ink-Strichen als Text

Konvertieren Sie mithilfe der Handschrifterkennung in Windows Ink Freihandstriche in Text.

**Wichtige APIs**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


Die Schrifterkennung ist in die Windows-Freihandplattform integriert und unterstützt einen umfassenden Satz von Gebietsschemas und Sprachen.

Fügen Sie bei allen hier aufgeführten Beispielen die für die Freihandfunktion benötigten Namespaceverweise hinzu. Hierzu gehört „Windows.UI.Input.Inking“.

## <a name="basic-handwriting-recognition"></a>Grundlegende Schrifterkennung


Hier wird veranschaulicht, wie mit dem Schrifterkennungsmodul in Verbindung mit dem standardmäßig installierten Sprachpaket, ein Satz von Freihandstrichen in einer [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse interpretiert wird.

Die Erkennung wird durch den Benutzer initiiert, indem er nach Abschluss des Schreibvorgangs auf eine Schaltfläche klickt.

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

2.  Dann werden einige grundlegende Verhalten für Freihandeingaben festgelegt.

    Die [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft wird für die Interpretation von Eingabedaten von Stift oder Maus als Freihandstriche ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)) konfiguriert. Freihandstriche werden in der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse mit der angegebenen [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050)-Klasse gerendert. Außerdem wird ein Listener für das Click-Ereignis der Schaltfläche „Erkennen“ deklariert.
```    CSharp
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

3.  Zum Schluss wird die grundlegende Schrifterkennung durchgeführt. In diesem Beispiel wird die Schrifterkennung mit dem Click-Ereignishandler der Schaltfläche „Erkennen“ ausgeführt.

    Ein [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) speichert alle Freihandstriche in einem [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)-Objekt. Die Freihandstriche werden durch die [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766)-Eigenschaft von **InkPresenter** verfügbar gemacht und mit der [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499)-Methode abgerufen.
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.
```    CSharp
// Create a manager for the InkRecognizer object
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer =
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
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

    Here's the click handler example, in full.
```    CSharp
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


Für die Schrifterkennung können viele der durch Windows unterstützten Sprachen verwendet werden.

Eine Liste der vom [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478) unterstützten Sprachen finden Sie in der [**InkRecognizer.Name**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkrecognizer.name.aspx)-Eigenschaft.

Ihre App kann den Satz der installierten Schrifterkennungsmodule abfragen und eines davon verwenden, oder der Benutzer wählt die bevorzugte Sprache aus.

**Hinweis**  
Die Benutzer können über **Einstellungen -&gt; Zeit & Sprache** eine Liste der installierten Sprachen anzeigen. Die installierten Sprachen werden unter **Sprachen** aufgeführt.

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
```    CSharp
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
```    CSharp
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
```    CSharp
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
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes =
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
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
```    CSharp
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

## <a name="dynamic-handwriting-recognition"></a>Dynamische Schrifterkennung


In den beiden vorherigen Beispielen musste der Benutzer auf eine Schaltfläche klicken, um die Erkennung zu starten. Wenn die Freihandstricheingabe mit einer einfachen Timing-Funktion kombiniert wird, kann die App auch eine dynamische Erkennung ausführen.

In diesem Beispiel werden die gleichen Einstellungen für Benutzeroberfläche und Freihandstriche verwendet wie im Beispiel für internationale Erkennung oben.

1.  Wie in den vorherigen Beispielen wird [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) so konfiguriert, dass Eingaben von Stift und Maus als Freihandstriche interpretiert werden ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Freihandstriche werden in der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Klasse mit der angegebenen [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050)-Klasse gerendert.

    Anstelle einer Schaltfläche zum Initiieren der Erkennung werden Listener für zwei [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Freihandstrichereignisse ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) und [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)) hinzugefügt. Zudem wird ein einfacher Timer ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)) mit einem [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)-Intervall von einer Sekunde eingerichtet.    
```    CSharp
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

        // Listen for stroke events on the InkPresenter to
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected +=
            inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = new TimeSpan(0, 0, 1);
        recoTimer.Tick += recoTimer_Tick;
    }

    // Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }    
```

2.  Dies sind die Handler für die drei Ereignisse, die im ersten Schritt hinzugefügt wurden.

    [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    Starten Sie den Timer für die Erkennung, wenn der Benutzer die Freihandeingabe beendet, indem er den Stift oder Finger anhebt oder die Maustaste loslässt. Nach einer Sekunde ohne Freihandeingabe wird die Spracherkennung initiiert.

    [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    Wenn ein neuer Freihandstrich vor dem nächsten Tick-Ereignis des Timers beginnt, wird der Timer beendet, da es sich bei dem neuen Freihandstrich wahrscheinlich um die Fortsetzung der vorherigen Handschrifteingabe handelt.

    [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)  
    Rufen Sie die Funktion für die Erkennung nach einer Sekunde ohne Freihandeingaben auf.
```    CSharp
// Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }
```

3.  Abschließend wird die Schrifterkennung auf Grundlage der ausgewählten Erkennung für die Handschrift ausgeführt. In diesem Beispiel wird zum Initiieren der Schrifterkennung der [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)-Ereignishandler einer [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)-Klasse verwendet.

    Ein [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) speichert alle Freihandstriche in einem [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)-Objekt. Die Freihandstriche werden durch die [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766)-Eigenschaft von **InkPresenter** verfügbar gemacht und mit der [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499)-Methode abgerufen.
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
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

    Here's the recognition function, in full.
```    CSharp
// Respond to timer Tick and initiate recognition.
    private async void Recognize_Tick()
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

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

        // Stop the dynamic recognition timer.
        recoTimer.Stop();
    }
```

## <a name="related-articles"></a>Verwandte Artikel

* [Zeichen- und Eingabestiftinteraktionen](pen-and-stylus-interactions.md)

**Beispiele**
* [Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Einfaches Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Komplexes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Malbuchbeispiel](https://aka.ms/cpubsample-coloringbook)
* [Familiennotizbeispiel](https://aka.ms/cpubsample-familynotessample)


 



<!--HONumber=Dec16_HO1-->


