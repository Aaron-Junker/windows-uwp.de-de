---
Description: Erstellen Sie Apps für die Universelle Windows-Plattform (UWP-Apps), die benutzerdefinierte Interaktionen über Stifte und Tabletstifte unterstützen, einschließlich Freihandeingabe für das Schreiben und Zeichnen wie auf Papier.
title: Stiftinteraktionen und Windows Ink in UWP-Apps
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in UWP apps
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, Handschrifterkennung, Benutzerinteraktion, Eingabe
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 031ac1ebc8d164c99240969ce77813f36a311858
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258298"
---
# <a name="pen-interactions-and-windows-ink-in-uwp-apps"></a>Stiftinteraktionen und Windows Ink in UWP-Apps

![Surface Pen](images/ink/hero-small.png)  
*Surface Pen* (zum Kauf im [Microsoft Store](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b) verfügbar).

## <a name="overview"></a>Übersicht

Optimieren Sie Ihre App für die Universelle Windows-Plattform (UWP-App) für Stifteingaben, um sowohl Standardfunktionalität für [**Zeigergeräte**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.PointerDevice) als auch optimale Windows Ink-Funktionalität für Benutzer bereitzustellen.

> [!NOTE]
> Der Schwerpunkt dieses Themas liegt auf der Windows Ink-Plattform. Informationen zur allgemeinen Behandlung von Zeigereingaben (ähnlich Maus-, Touch- und Touchpadeingaben) finden Sie unter [Behandeln von Zeigereingaben](handle-pointer-input.md).

| Videos |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Using ink in your UWP app* | *Use Windows Pen and Ink to build more engaging enterprise apps* |

In Verbindung mit einem Zeichengerät bietet die Windows Ink-Plattform eine natürliche Möglichkeit, digitale handschriftliche Notizen, Zeichnungen und Anmerkungen zu erstellen. Sie können auf der Plattform Freihanddaten aus einem Eingabedigitalisierungsgerät erfassen, Freihanddaten generieren, verwalten und als letzte Striche auf dem Ausgabegerät rendern sowie über die Schrifterkennung in Text umwandeln.

Ihre App kann nicht nur die grundlegende Position und Bewegung des Stifts aufzeichnen, während der Benutzer schreibt oder zeichnet, sondern auch den variierenden Druck während des gesamten Strichs nachverfolgen und erfassen. Mit diesen Informationen, zusammen mit Einstellungen für Form und Größe der Stiftspitze, Drehung, Freihandfarbe und Zweck (einfache Freihandeingabe, Löschen, Hervorheben und Auswählen), können Sie dem Benutzer ermöglichen, auf ähnliche Weise wie mit einem Stift, Bleistift oder Pinsel auf Papier zu arbeiten.

> [!NOTE]
> Ihre App kann auch Freihandeingaben von anderen zeigerbasierten Geräten, z. B. Touchdigitalisierungs- und Mausgeräte, unterstützen. 

Die Freihandplattform ist sehr flexibel. Je nach Ihren Anforderungen unterstützt sie verschiedene Funktionalitätsgrade.

Richtlinien für die Benutzeroberfläche von Windows Ink finden Sie unter [Inking controls](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Komponenten der Windows Ink-Plattform

| Komponente | Beschreibung |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | A XAML UI platform control that, by default, receives and displays all input from a pen as either an ink stroke or an erase stroke.<br/>Weitere Informationen zur Verwendung von InkCanvas finden Sie unter [Erkennen von Windows Ink-Strichen als Text](convert-ink-to-text.md) und [Speichern und Abrufen der Daten von Windows Ink-Strichen](save-and-load-ink.md). |
| [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Ein CodeBehind-Objekt, das zusammen mit einem [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement instanziiert wird (über die [**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Eigenschaft verfügbar gemacht). Dieses Objekt stellt alle Standardfreihandfunktionen bereit, die vom **InkCanvas**-Steuerelement zur Verfügung gestellt werden, sowie einen umfassenden Satz von APIs für zusätzliche Anpassung und Personalisierung.<br/>Weitere Informationen zur Verwendung von InkPresenter finden Sie unter [Erkennen von Windows Ink-Strichen als Text](convert-ink-to-text.md) und [Speichern und Abrufen der Daten von Windows Ink-Strichen](save-and-load-ink.md). |
| [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) | A XAML UI platform control containing a customizable and extensible collection of buttons that activate ink-related features in an associated [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas).<br/>Weitere Informationen zur Verwendung von InkToolbar finden Sie unter [Add an InkToolbar to a Universal Windows Platform (UWP) inking app](ink-toolbar.md). |
| [**IInkD2DRenderer**](https://docs.microsoft.com/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) | Ermöglicht das Rendern von Freihandstrichen im angegebenen Direct2D-Gerätekontext einer universellen Windows-App statt im standardmäßigen [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement. Dies ermöglicht die umfassende Anpassung der Freihandfunktionen.<br/>Weitere Informationen finden Sie in diesem [komplexen Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk). |

## <a name="basic-inking-with-inkcanvas"></a>Einfaches Freihandzeichnen mit „InkCanvas“

Um einfaches Freihandzeichen hinzuzufügen, platzieren Sie einfach ein [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-UWP-Steuerelement auf der entsprechenden Seite in Ihrer App.

Das [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement unterstützt standardmäßig nur Freihandeingaben mit einem Stift. Die Eingabe wird entweder als Strich mit den Standardeinstellungen für Farbe und Stärke gerendert (ein schwarzer Kugelschreiber mit einer Stärke von 2 Pixeln) oder als Strichradierer behandelt (wenn die Eingabe von einer mit einer Löschschaltfläche geänderten Radiergummi- oder Stiftspitze stammt).

> [!NOTE]
> Falls keine Radiergummispitze bzw. -schaltfläche vorhanden ist, kann InkCanvas so konfiguriert werden, dass Eingaben mit der Stiftspitze wie Radierstriche behandelt werden.

In diesem Beispiel überlagert ein [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement ein Hintergrundbild.

> [!NOTE]
> An InkCanvas has default [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) and [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width) properties of zero, unless it is the child of an element that automatically sizes its child elements, such as [StackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel) or [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) controls.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Diese Serie von Bildern zeigt, wie die Stifteingabe von diesem [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement gerendert wird.

| ![Der leere „InkCanvas“ mit einem Hintergrundbild](images/ink_basic_1_small.png) | ![Der „InkCanvas“ mit letzten Strichen](images/ink_basic_2_small.png) | ![Der „InkCanvas“ mit einem ausradierten Strich](images/ink_basic_3_small.png) |
| --- | --- | ---|
| Der leere [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) mit einem Hintergrundbild | Der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) mit letzten Strichen | Der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) mit einem ausradierten Strich (beachten Sie, dass jeweils der gesamte Strich und nicht nur auf einen Teil davon ausradiert wird). |

Die vom [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement unterstützte Freihandfunktion wird von einem CodeBehind-Objekt mit dem Namen [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) bereitgestellt.

Für die einfache Freihandeingabe müssen Sie sich nicht mit dem [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)-Objekt befassen. Wenn Sie jedoch das Freihandverhalten des [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements anpassen und konfigurieren möchten, müssen Sie auf das entsprechende **InkPresenter**-Objekt zugreifen.

## <a name="basic-customization-with-inkpresenter"></a>Einfache Anpassung mit „InkPresenter“

Für jedes [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement wird ein [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)-Objekt instanziiert.

> [!NOTE]
> Das [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)-Objekt kann nicht direkt instanziiert werden. Stattdessen erfolgt der Zugriff darauf über die [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Eigenschaft des [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements. 

Das [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)-Objekt stellt nicht nur das gesamte Standard-Freihandeingabeverhalten des entsprechenden [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements bereit, sondern bietet auch einen umfassenden Satz von APIs für die zusätzliche Strichanpassung und eine differenzierte Verwaltung des Stifts (standardmäßig und verändert). Hierzu zählen Stricheigenschaften, unterstützte Eingabegerätetypen und die Möglichkeit festzulegen, ob die Eingabe vom Objekt verarbeitet oder zur Verarbeitung an die App übergeben wird.

> [!NOTE]
> Standardmäßige Freihandeingabe (entweder Stiftspitze oder Radiererspitze/-schaltfläche) wird mit einem sekundären Hardware-Angebot nicht geändert, wie z. B. eine Zeichenstift-Drucktaste, rechte Maustaste oder einem ähnlichen Mechanismus. 

Standardmäßig werden Freihandstriche für nur die Stifteingabe unterstützt. Hier konfigurieren wir [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter), damit Eingabedaten von Stift und Maus als letzte Striche interpretiert werden. Außerdem legen wir einige anfängliche Freihandstrichattribute fest, die zum Rendern von Strichen mit dem [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement verwendet werden.

Legen Sie zum Aktivieren der Maus- und Touch-Freihandeingabe die [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes)-Eigenschaft des [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter) auf die Kombination aus den gewünschten [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes)-Werten fest.

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
}
```

Attribute für letzte Striche können dynamisch entsprechend den Benutzereinstellungen oder App-Anforderungen festgelegt werden.

Hier kann der Benutzer aus einer Liste von Freihandfarben auswählen.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink customization sample"
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Anschließend behandeln wir Änderungen an der ausgewählten Farbe und aktualisieren die Attribute für letzte Striche entsprechend.

```csharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes =
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

Diese Bilder zeigen, wie die Stifteingabe vom [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt verarbeitet und angepasst wird.

| ![InkCanvas mit standardmäßigen schwarzen Freihandstrichen](images/ink-basic-custom-1-small.png) | ![InkCanvas mit vom Benutzer ausgewählten roten Freihandstrichen](images/ink-basic-custom-2-small.png) |
| --- | --- |
| Das [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement mit standardmäßigen schwarzen letzten Strichen | Das [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement mit vom Benutzer ausgewählten roten letzten Strichen | 

Um zusätzlich zur Freihandeingabe und zum Löschen weitere Funktionen wie etwa die Strichauswahl bereitzustellen, muss die App bestimmte Eingaben identifizieren, die vom [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt ohne Verarbeitung zur Behandlung an die App weitergegeben werden.

## <a name="pass-through-input-for-advanced-processing"></a>Weitergabe der Eingabe für die erweiterte Verarbeitung

In der Standardeinstellung verarbeitet [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) sämtliche Eingaben als letzten Strich oder ausradierten Strich, einschließlich der Eingabe, die durch ein sekundäres Hardwareangebot wie einer Zeichenstift-Drucktaste, einer rechten Maustaste oder ähnlichem geändert wird. Wenn Benutzer diese sekundären Angebote verwenden, erwarten sie jedoch in der Regel zusätzliche Funktionalität oder ein geändertes Verhalten.

In einigen Fällen müssen Sie möglicherweise basierend auf der Benutzerauswahl auf der App-UI auch zusätzliche Freihandfunktionen für Stifte ohne sekundäre Angebote (Funktionalität, die in der Regel nicht der Stiftspitze zugeordnet ist), andere Eingabegerätetypen oder ein auf irgendeine Weise geändertes Verhalten zur Verfügung stellen.

Das [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt kann zu diesem Zweck so konfiguriert werden, dass bestimmte Eingaben unverarbeitet bleiben. Diese unverarbeiteten Eingaben werden dann zur Verarbeitung an die App weitergegeben.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>Beispiel: Verwendung unverarbeiteter Eingaben zum Implementieren der Strichauswahl 

Die Windows Ink-Plattform bietet keine integrierte Unterstützung für die Aktionen, die geänderte Eingaben erfordern, z. B. die Strichauswahl. Zur Unterstützung derartiger Features müssen Sie eine benutzerdefinierte Lösung in Ihren Apps bereitstellen. 

Im folgenden Codebeispiel (der gesamte Code befindet sich in den Dateien MainPage.xaml und MainPage.xaml.cs) werden die Schritte zum Aktivieren der Strichauswahl gezeigt, wenn die Eingabe mit einer Zeichenstift-Drucktaste (oder der rechten Maustaste) geändert wird.

1.  Zunächst richten wir in „MainPage.xaml“ die Benutzeroberfläche ein.

    Hier fügen wir einen Zeichenbereich (unterhalb des [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements) zum Zeichnen der Strichauswahl hinzu. Durch die Verwendung einer eigenen Ebene zum Zeichnen der Strichauswahl bleiben das **InkCanvas**-Steuerelement und dessen Inhalt unverändert.

    ![Das leere InkCanvas-Steuerelement mit einem darunterliegenden Auswahlzeichenbereich](images/ink-unprocessed-1-small.png)

      ```xaml
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
          </Grid.RowDefinitions>
          <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header"
              Text="Advanced ink customization sample"
              VerticalAlignment="Center"
              Style="{ThemeResource HeaderTextBlockStyle}"
              Margin="10,0,0,0" />
          </StackPanel>
          <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
          </Grid>
        </Grid>
      ```

2.  In „MainPage.xaml.cs“ deklarieren wir eine Reihe von globalen Variablen zum Speichern von Verweisen auf Aspekte der Auswahl-UI. Dies gilt insbesondere für den Auswahllassostrich und das umgebende Rechteck, das die ausgewählten Striche hervorhebt.

      ```csharp
        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;
      ```

3.  Anschließend konfigurieren wir das [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt so, dass es Eingabedaten von Stift und Maus als Freihandstriche interpretiert. Zudem legen wir anfängliche Freihandstrichattribute zum Rendern von Freihandstrichen mit dem [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement fest.

    Vor allem geben wir mithilfe der [**InputProcessingConfiguration**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputprocessingconfiguration)-Eigenschaft des [InkPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekts an, dass alle geänderten Eingaben von der App verarbeitet werden sollen. Geänderte Eingaben geben wir an, indem wir **InputProcessingConfiguration.RightDragAction** den Wert [**InkInputRightDragAction.LeaveUnprocessed**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkInputRightDragAction) zuweisen. Wenn dieser Wert festgelegt wird, übergibt [InkPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) eine Reihe von Zeigerereignissen zur Verarbeitung an die [InkUnprocessedInput](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput)-Klasse.

    Wir weisen Listener für die nicht verarbeiteten Ereignisse [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved) und [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) zu, die vom [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt weitergegeben werden. Sämtliche Auswahlfunktionalität wird in den Handlern für diese Ereignisse implementiert.

    Schließlich weisen wir Listener für die Ereignisse [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) und [**StrokesErased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) des [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekts zu. Wir verwenden die Handler für diese Ereignisse, um die Auswahl-UI zu bereinigen, wenn ein neuer Strich begonnen oder ein vorhandener Strich ausradiert wird.

    ![InkCanvas mit standardmäßigen schwarzen Freihandstrichen](images/ink-unprocessed-2-small.png)

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

4.  Anschließend definieren wir Handler für die nicht verarbeiteten Ereignisse [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved) und [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased), die vom [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt weitergegeben werden.

    Sämtliche Auswahlfunktionalität, einschließlich des Lassostrichs und des umgebenden Rechtecks, wird in diesen Handlern implementiert.

    ![Auswahllasso](images/ink-unprocessed-3-small.png)

      ```csharp
        // Handle unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
          InkUnprocessedInput sender, PointerEventArgs args)
        {
          // Initialize a selection lasso.
          lasso = new Polyline()
          {
              Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
              StrokeThickness = 1,
              StrokeDashArray = new DoubleCollection() { 5, 2 },
              };

              lasso.Points.Add(args.CurrentPoint.RawPosition);

              selectionCanvas.Children.Add(lasso);
          }

          private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
          }

          private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add the final point to the Polyline object and
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
              inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

            DrawBoundingRect();
          }
      ```

5.  Um den PointerReleased-Ereignishandler abzuschließen, löschen wir sämtlichen Inhalt (den Lassostrich) aus der Auswahlebene und zeichnen dann ein einzelnes umgebendes Rechteck um die letzten Striche, die sich im Lassobereich befinden.

    ![Das umgebende Auswahlrechteck](images/ink-unprocessed-4-small.png)

      ```csharp
        // Draw a bounding rectangle, on the selection canvas, encompassing
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
          // Clear all existing content from the selection canvas.
          selectionCanvas.Children.Clear();

          // Draw a bounding rectangle only if there are ink strokes
          // within the lasso area.
          if (!((boundingRect.Width == 0) ||
            (boundingRect.Height == 0) ||
            boundingRect.IsEmpty))
            {
              var rectangle = new Rectangle()
              {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                  StrokeThickness = 1,
                  StrokeDashArray = new DoubleCollection() { 5, 2 },
                  Width = boundingRect.Width,
                  Height = boundingRect.Height
              };

              Canvas.SetLeft(rectangle, boundingRect.X);
              Canvas.SetTop(rectangle, boundingRect.Y);

              selectionCanvas.Children.Add(rectangle);
            }
          }
      ```

6.  Schließlich definieren wir Handler für die InkPresenter-Ereignisse [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) und [**StrokesErased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased).

    Diese beiden rufen einfach die gleiche Bereinigungsfunktion auf, um bei jeder Erkennung eines neuen Strichs die aktuelle Auswahl zu löschen.

      ```csharp
        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
          InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
          ClearSelection();
        }

        private void InkPresenter_StrokesErased(
          InkPresenter sender, InkStrokesErasedEventArgs args)
        {
          ClearSelection();
        }
      ```

7.  Dies ist die Funktion zum Entfernen der gesamten Auswahl-UI aus dem Auswahlzeichenbereich, wenn ein neuer Strich begonnen oder ein vorhandener Strich ausradiert wird.

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

## <a name="custom-ink-rendering"></a>Benutzerdefiniertes Rendern von Freihandeingaben

Standardmäßig werden Freihandeingaben in einem Hintergrundthread mit geringer Wartezeit verarbeitet und im Verlauf des Zeichnens „nass“ gerendert. Wenn der Strich abgeschlossen ist (der Stift oder Finger wurde angehoben oder die Maustaste losgelassen), wird er im UI-Thread verarbeitet und auf der [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Ebene „trocken“ gerendert (über dem Anwendungsinhalt, wo er die nasse Freihandeingabe ersetzt).

Sie können dieses Standardverhalten überschreiben und die Freihandfunktionen durch benutzerdefiniertes Trocknen der Freihandeingabestriche vollständig steuern. Das Standardverhalten ist zwar für die meisten Anwendungen in der Regel ausreichend, aber es gibt einige Fälle, in denen möglicherweise ein benutzerdefiniertes Trocknen erforderlich ist. Dazu gehören:
- Eine effizientere Verwaltung großer und komplexer Sammlungen von Freihandstrichen
- Effizientere Unterstützung für Schwenken und Zoomen in großen Freihandzeichenbereichen
- Überlappung von Freihand- und anderen Objekte, z. B. Formen oder Text, bei gleichzeitiger Aufrechterhaltung der Z-Reihenfolge 
- Synchrones Trocknen und Umwandeln von Freihandeingaben in eine DirectX-Form (z. B. eine gerade Linie oder Form, die im Anwendungsinhalt gerastert und integriert ist, statt als separate **InkCanvas**-Ebene).

Benutzerdefiniertes Trocknen erfordert anstelle des standardmäßigen [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements ein [**IInkD2DRenderer**](https://docs.microsoft.com/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer)-Objekt, um die Freihandeingabe zu verwalten und im Direct2D-Gerätekontext der universellen Windows-App zu rendern.

Eine App erstellt durch Aufruf von [**ActivateCustomDrying**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.activatecustomdrying) (vor dem Laden des [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements) ein [**InkSynchronizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkSynchronizer)-Objekt, um zu definieren, wie ein letzter Strich trocken in einer [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)- oder [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource)-Klasse gerendert wird. 

Sowohl [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) als auch [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) bieten eine gemeinsame DirectX-Oberfläche, auf die Ihre App zeichnen und Inhalte für Ihre Anwendung verfassen kann, obwohl VSIS eine virtuelle Oberfläche bereitstellt, die größer als der Bildschirm ist, um ein leistungsfähiges Schwenken und Zoomen zu ermöglichen. Da visuelle Aktualisierungen an diesen Oberflächen beim Rendern von Freihandeingaben in beiden mit dem XAML-UI-Thread synchronisiert werden, kann die nasse Freihandeingabe gleichzeitig aus InkCanvas entfernt werden. 

Sie können Freihandeingaben auch an ein [SwapChainPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel) benutzerdefiniert trocknen, aber die Synchronisierung mit dem UI-Thread kann nicht garantiert werden, und es kommt möglicherweise zu einer Verzögerung zwischen dem Rendern der Freihandeingabe in Ihrem SwapChainPanel und dem Entfernen der Freihandeingabe aus InkCanvas.

Ein vollständiges Beispiel für diese Funktion finden Sie unter [Komplexes Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk).

> [!NOTE]
> Benutzerdefiniertes Trocknen und die [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> Wenn Ihre App das Standard-Renderverhalten für Freihandeingaben von [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) mit einer benutzerdefinierten Trockenimplementierung außer Kraft setzt, sind die gerenderten Freihandstriche für die InkToolbar nicht mehr verfügbar und die integrierten Löschbefehle der InkToolbar funktionieren nicht wie erwartet. Damit Sie Löschfunktionen bereitstellen können, müssen Sie alle Zeigerereignisse verarbeiten, für jeden Strich einen Treffertest ausführen und den integrierten Befehl „Freihand vollständig löschen“ überschreiben.

## <a name="other-articles-in-this-section"></a>Andere Artikel in diesem Abschnitt

| Thema | Beschreibung |
| --- | --- |
| [Recognize ink strokes](convert-ink-to-text.md) | Konvertieren Sie letzte Striche mit der Schrifterkennung in Text oder mit der benutzerdefinierten Erkennung in Formen. |
| [Store and retrieve ink strokes](save-and-load-ink.md) | Speichern Sie Freihandstrichdaten mithilfe eingebetteter serialisierter Freihandformat-Metadaten (Ink Serialized Format, ISF) in einer GIF-Datei (Graphics Interchange Format). |
| [Add an InkToolbar to a UWP inking app](ink-toolbar.md) | Fügen Sie einer App für die Freihandeingabe in der universellen Windows-Plattform (UWP) eine Standard-InkToolbar hinzu. Fügen Sie der InkToolbar einen anpassbaren Stift hinzu und binden Sie diesen an eine benutzerdefinierte Definition für den Stift. |

## <a name="related-articles"></a>Verwandte Artikel

* [Get started: Support ink in your UWP app](../../get-started/ink-walkthrough.md)
* [Behandeln von Zeigereingaben](handle-pointer-input.md)
* [Identifizieren von Eingabegeräten](identify-input-devices.md)

**APIs**

* [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)
* [**Windows.UI.Input.Inking**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)
* [**Windows.UI.Input.Inking.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.Core)

**Beispiele**
* [Get Started Tutorial: Support ink in your UWP app](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [Simple ink sample (C#/C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [Complex ink sample (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Ink sample (JavaScript)](https://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Coloring book sample](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Family notes sample](https://github.com/Microsoft/Windows-appsample-familynotes)
* [Basic input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Low latency input sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Archivbeispiele**
* [Input: Device capabilities sample](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Input: XAML user input events sample](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [XAML scrolling, panning, and zooming sample](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Input: Gestures and manipulations with GestureRecognizer](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
