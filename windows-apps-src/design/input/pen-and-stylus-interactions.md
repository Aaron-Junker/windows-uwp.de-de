---
author: Karl-Bridge-Microsoft
Description: Build Universal Windows Platform (UWP) apps that support custom interactions from pen and stylus devices, including digital ink for natural writing and drawing experiences.
title: Stiftinteraktionen und Windows Ink in UWP-Apps
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in UWP apps
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, Handschrifterkennung, Benutzerinteraktion, Eingabe
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3d04ea3506fc909b115ba9aab397ded9e4464479
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "5822456"
---
# <a name="pen-interactions-and-windows-ink-in-uwp-apps"></a>Stiftinteraktionen und Windows Ink in UWP-Apps

![Surface Pen](images/ink/hero-small.png)  
*Surface Pen* (zum Kauf im [Microsoft Store](https://aka.ms/purchasesurfacepen) verfügbar).

## <a name="overview"></a>Übersicht

Optimieren Sie Ihre App für die Universelle Windows-Plattform (UWP-App) für Stifteingaben, um sowohl Standardfunktionalität für [**Zeigergeräte**](https://msdn.microsoft.com/library/windows/apps/br225633) als auch optimale Windows Ink-Funktionalität für Benutzer bereitzustellen.

> [!NOTE]
> Der Schwerpunkt dieses Themas liegt auf der Windows Ink-Plattform. Informationen zur allgemeinen Behandlung von Zeigereingaben (ähnlich Maus-, Touch- und Touchpadeingaben) finden Sie unter [Behandeln von Zeigereingaben](handle-pointer-input.md).

| Videos |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Verwenden von Ink in Ihrer UWP-App* | *Verwenden von Windows Pen und Ink zum Erstellen von stärker interaktiven enterpriseapps* |

In Verbindung mit einem Zeichengerät bietet die Windows Ink-Plattform eine natürliche Möglichkeit, digitale handschriftliche Notizen, Zeichnungen und Anmerkungen zu erstellen. Sie können auf der Plattform Freihanddaten aus einem Eingabedigitalisierungsgerät erfassen, Freihanddaten generieren, verwalten und als letzte Striche auf dem Ausgabegerät rendern sowie über die Schrifterkennung in Text umwandeln.

Ihre App kann nicht nur die grundlegende Position und Bewegung des Stifts aufzeichnen, während der Benutzer schreibt oder zeichnet, sondern auch den variierenden Druck während des gesamten Strichs nachverfolgen und erfassen. Mit diesen Informationen, zusammen mit Einstellungen für Form und Größe der Stiftspitze, Drehung, Freihandfarbe und Zweck (einfache Freihandeingabe, Löschen, Hervorheben und Auswählen), können Sie dem Benutzer ermöglichen, auf ähnliche Weise wie mit einem Stift, Bleistift oder Pinsel auf Papier zu arbeiten.

> [!NOTE]
> Ihre App kann auch Freihandeingaben von anderen zeigerbasierten Geräten, z.B. Touchdigitalisierungs- und Mausgeräte, unterstützen. 

Die Freihandplattform ist sehr flexibel. Je nach Ihren Anforderungen unterstützt sie verschiedene Funktionalitätsgrade.

Richtlinien für die Benutzeroberfläche von WindowsInk finden Sie unter [Inking controls](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Komponenten der WindowsInk-Plattform

| Komponente | Beschreibung |
| --- | --- |
| [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) | Ein XAMLUI Plattform-Steuerelement, das in der Standardeinstellung empfängt und zeigt alle Eingaben von einem Stift als letzten Strich oder ausradierten Strich.<br/>Weitere Informationen zur Verwendung von InkCanvas finden Sie unter [Erkennen von Windows Ink-Strichen als Text](convert-ink-to-text.md) und [Speichern und Abrufen der Daten von Windows Ink-Strichen](save-and-load-ink.md). |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | Ein CodeBehind-Objekt, das zusammen mit einem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement instanziiert wird (über die [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft verfügbar gemacht). Dieses Objekt stellt alle Standardfreihandfunktionen bereit, die vom **InkCanvas**-Steuerelement zur Verfügung gestellt werden, sowie einen umfassenden Satz von APIs für zusätzliche Anpassung und Personalisierung.<br/>Weitere Informationen zur Verwendung von InkPresenter finden Sie unter [Erkennen von Windows Ink-Strichen als Text](convert-ink-to-text.md) und [Speichern und Abrufen der Daten von Windows Ink-Strichen](save-and-load-ink.md). |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | Ein XAMLUI-plattformsteuerelement, enthält eine anpassbare und erweiterbare Sammlung von Schaltflächen, die Freihand-Features in einem verknüpften [**InkCanvas-Steuerelement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)aktivieren.<br/>Weitere Informationen zur Verwendung von InkToolbar finden Sie unter [Add an InkToolbar to a Universal Windows Platform (UWP) inking app](ink-toolbar.md). |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) | Ermöglicht das Rendern von Freihandstrichen im angegebenen Direct2D-Gerätekontext einer universellen Windows-App statt im standardmäßigen [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement. Dies ermöglicht die umfassende Anpassung der Freihandfunktionen.<br/>Weitere Informationen finden Sie in diesem [komplexen Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620314). |

## <a name="basic-inking-with-inkcanvas"></a>Einfaches Freihandzeichnen mit „InkCanvas“

Um einfaches Freihandzeichen hinzuzufügen, platzieren Sie einfach ein [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-UWP-Steuerelement auf der entsprechenden Seite in Ihrer App.

Das [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement unterstützt standardmäßig nur Freihandeingaben mit einem Stift. Die Eingabe wird entweder als Strich mit den Standardeinstellungen für Farbe und Stärke gerendert (ein schwarzer Kugelschreiber mit einer Stärke von 2Pixeln) oder als Strichradierer behandelt (wenn die Eingabe von einer mit einer Löschschaltfläche geänderten Radiergummi- oder Stiftspitze stammt).

> [!NOTE]
> Falls keine Radiergummispitze bzw. -schaltfläche vorhanden ist, kann InkCanvas so konfiguriert werden, dass Eingaben mit der Stiftspitze wie Radierstriche behandelt werden.

In diesem Beispiel überlagert ein [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement ein Hintergrundbild.

> [!NOTE]
> Ein InkCanvas verfügt standardmäßig über [**Höhe.**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) und [**Breite-**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width)-Eigenschaften von null, sofern es sich um ein untergeordnetes Element eines Elements handelt, das die Größe seiner untergeordneten Elemente automatisch festlegt. 

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

Diese Serie von Bildern zeigt, wie die Stifteingabe von diesem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement gerendert wird.

| ![Der leere „InkCanvas“ mit einem Hintergrundbild](images/ink_basic_1_small.png) | ![Der „InkCanvas“ mit letzten Strichen](images/ink_basic_2_small.png) | ![Der „InkCanvas“ mit einem ausradierten Strich](images/ink_basic_3_small.png) |
| --- | --- | ---|
| Der leere [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) mit einem Hintergrundbild | Der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) mit letzten Strichen | Der [**InkCanvas** ](https://msdn.microsoft.com/library/windows/apps/dn858535) mit einem ausradierten Strich (beachten Sie, dass jeweils der gesamte Strich und nicht nur auf einen Teil davon ausradiert wird). |

Die vom [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement unterstützte Freihandfunktion wird von einem CodeBehind-Objekt mit dem Namen [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) bereitgestellt.

Für die einfache Freihandeingabe müssen Sie sich nicht mit dem [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Objekt befassen. Wenn Sie jedoch das Freihandverhalten des [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements anpassen und konfigurieren möchten, müssen Sie auf das entsprechende **InkPresenter**-Objekt zugreifen.

## <a name="basic-customization-with-inkpresenter"></a>Einfache Anpassung mit „InkPresenter“

Für jedes [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement wird ein [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Objekt instanziiert.

> [!NOTE]
> Das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Objekt kann nicht direkt instanziiert werden. Stattdessen erfolgt der Zugriff darauf über die [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Eigenschaft des [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements. 

Das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)-Objekt stellt nicht nur das gesamte Standard-Freihandeingabeverhalten des entsprechenden [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements bereit, sondern bietet auch einen umfassenden Satz von APIs für die zusätzliche Strichanpassung und eine differenzierte Verwaltung des Stifts (standardmäßig und verändert). Hierzu zählen Stricheigenschaften, unterstützte Eingabegerätetypen und die Möglichkeit festzulegen, ob die Eingabe vom Objekt verarbeitet oder zur Verarbeitung an die App übergeben wird.

> [!NOTE]
> Standardmäßige Freihandeingabe (entweder Stiftspitze oder Radiererspitze/-schaltfläche) wird mit einem sekundären Hardware-Angebot nicht geändert, wie z.B. eine Zeichenstift-Drucktaste, rechte Maustaste oder einem ähnlichen Mechanismus. 

Standardmäßig werden Freihandstriche für nur die Stifteingabe unterstützt. Hier konfigurieren wir [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081), damit Eingabedaten von Stift und Maus als letzte Striche interpretiert werden. Außerdem legen wir einige anfängliche Freihandstrichattribute fest, die zum Rendern von Strichen mit dem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement verwendet werden.

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

Diese Bilder zeigen, wie die Stifteingabe vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt verarbeitet und angepasst wird.

| ![InkCanvas mit standardmäßigen schwarzen Freihandstrichen](images/ink-basic-custom-1-small.png) | ![InkCanvas mit vom Benutzer ausgewählten roten Freihandstrichen](images/ink-basic-custom-2-small.png) |
| --- | --- |
| Das [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement mit standardmäßigen schwarzen letzten Strichen | Das [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement mit vom Benutzer ausgewählten roten letzten Strichen | 

Um zusätzlich zur Freihandeingabe und zum Löschen weitere Funktionen wie etwa die Strichauswahl bereitzustellen, muss die App bestimmte Eingaben identifizieren, die vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt ohne Verarbeitung zur Behandlung an die App weitergegeben werden.

## <a name="pass-through-input-for-advanced-processing"></a>Weitergabe der Eingabe für die erweiterte Verarbeitung

In der Standardeinstellung verarbeitet [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) sämtliche Eingaben als letzten Strich oder ausradierten Strich, einschließlich der Eingabe, die durch ein sekundäres Hardwareangebot wie einer Zeichenstift-Drucktaste, einer rechten Maustaste oder ähnlichem geändert wird. Wenn Benutzer diese sekundären Angebote verwenden, erwarten sie jedoch in der Regel zusätzliche Funktionalität oder ein geändertes Verhalten.

In einigen Fällen müssen Sie möglicherweise basierend auf der Benutzerauswahl auf der App-UI auch zusätzliche Freihandfunktionen für Stifte ohne sekundäre Angebote (Funktionalität, die in der Regel nicht der Stiftspitze zugeordnet ist), andere Eingabegerätetypen oder ein auf irgendeine Weise geändertes Verhalten zur Verfügung stellen.

Das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt kann zu diesem Zweck so konfiguriert werden, dass bestimmte Eingaben unverarbeitet bleiben. Diese unverarbeiteten Eingaben werden dann zur Verarbeitung an die App weitergegeben.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>Beispiel: Verwendung unverarbeiteter Eingaben zum Implementieren der Strichauswahl 

Die Windows Ink-Plattform bietet keine integrierte Unterstützung für die Aktionen, die geänderte Eingaben erfordern, z.B. die Strichauswahl. Zur Unterstützung derartiger Features müssen Sie eine benutzerdefinierte Lösung in Ihren Apps bereitstellen. 

Im folgenden Codebeispiel (der gesamte Code befindet sich in den Dateien MainPage.xaml und MainPage.xaml.cs) werden die Schritte zum Aktivieren der Strichauswahl gezeigt, wenn die Eingabe mit einer Zeichenstift-Drucktaste (oder der rechten Maustaste) geändert wird.

1.  Zunächst richten wir in „MainPage.xaml“ die Benutzeroberfläche ein.

    Hier fügen wir einen Zeichenbereich (unterhalb des [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements) zum Zeichnen der Strichauswahl hinzu. Durch die Verwendung einer eigenen Ebene zum Zeichnen der Strichauswahl bleiben das **InkCanvas**-Steuerelement und dessen Inhalt unverändert.

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

3.  Anschließend konfigurieren wir das [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt so, dass es Eingabedaten von Stift und Maus als Freihandstriche interpretiert. Zudem legen wir anfängliche Freihandstrichattribute zum Rendern von Freihandstrichen mit dem [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelement fest.

    Vor allem geben wir mithilfe der [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764)-Eigenschaft des [InkPresenter](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekts an, dass alle geänderten Eingaben von der App verarbeitet werden sollen. Geänderte Eingaben geben wir an, indem wir **InputProcessingConfiguration.RightDragAction** den Wert [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760) zuweisen. Wenn dieser Wert festgelegt wird, übergibt [InkPresenter](https://msdn.microsoft.com/library/windows/apps/dn899081) eine Reihe von Zeigerereignissen zur Verarbeitung an die [InkUnprocessedInput](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput)-Klasse.

    Wir weisen Listener für die nicht verarbeiteten Ereignisse [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) und [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) zu, die vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt weitergegeben werden. Sämtliche Auswahlfunktionalität wird in den Handlern für diese Ereignisse implementiert.

    Schließlich weisen wir Listener für die Ereignisse [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) und [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) des [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekts zu. Wir verwenden die Handler für diese Ereignisse, um die Auswahl-UI zu bereinigen, wenn ein neuer Strich begonnen oder ein vorhandener Strich ausradiert wird.

    ![InkCanvas mit standardmäßigen schwarzen letzten Strichen](images/ink-unprocessed-2-small.png)

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

4.  Anschließend definieren wir Handler für die nicht verarbeiteten Ereignisse [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) und [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713), die vom [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)-Objekt weitergegeben werden.

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

6.  Schließlich definieren wir Handler für die InkPresenter-Ereignisse [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) und [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767).

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

Standardmäßig werden Freihandeingaben in einem Hintergrundthread mit geringer Wartezeit verarbeitet und im Verlauf des Zeichnens „nass“ gerendert. Wenn der Strich abgeschlossen ist (der Stift oder Finger wurde angehoben oder die Maustaste losgelassen), wird er im UI-Thread verarbeitet und auf der [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Ebene „trocken“ gerendert (über dem Anwendungsinhalt, wo er die nasse Freihandeingabe ersetzt).

Sie können dieses Standardverhalten überschreiben und die Freihandfunktionen durch benutzerdefiniertes Trocknen der Freihandeingabestriche vollständig steuern. Das Standardverhalten ist zwar für die meisten Anwendungen in der Regel ausreichend, aber es gibt einige Fälle, in denen möglicherweise ein benutzerdefiniertes Trocknen erforderlich ist. Dazu gehören:
- Eine effizientere Verwaltung großer und komplexer Sammlungen von Freihandstrichen
- Effizientere Unterstützung für Schwenken und Zoomen in großen Freihandzeichenbereichen
- Überlappung von Freihand- und anderen Objekte, z.B. Formen oder Text, bei gleichzeitiger Aufrechterhaltung der Z-Reihenfolge 
- Synchrones Trocknen und Umwandeln von Freihandeingaben in eine DirectX-Form (z.B. eine gerade Linie oder Form, die im Anwendungsinhalt gerastert und integriert ist, statt als separate **InkCanvas**-Ebene).

Benutzerdefiniertes Trocknen erfordert anstelle des standardmäßigen [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements ein [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263)-Objekt, um die Freihandeingabe zu verwalten und im Direct2D-Gerätekontext der universellen Windows-App zu rendern.

Eine App erstellt durch Aufruf von [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012) (vor dem Laden des [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)-Steuerelements) ein [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979)-Objekt, um zu definieren, wie ein letzter Strich trocken in einer [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)- oder [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource)-Klasse gerendert wird. 

Sowohl [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) als auch [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) bieten eine gemeinsame DirectX-Oberfläche, auf die Ihre App zeichnen und Inhalte für Ihre Anwendung verfassen kann, obwohl VSIS eine virtuelle Oberfläche bereitstellt, die größer als der Bildschirm ist, um ein leistungsfähiges Schwenken und Zoomen zu ermöglichen. Da visuelle Aktualisierungen an diesen Oberflächen beim Rendern von Freihandeingaben in beiden mit dem XAML-UI-Thread synchronisiert werden, kann die nasse Freihandeingabe gleichzeitig aus InkCanvas entfernt werden. 

Sie können Freihandeingaben auch an ein [SwapChainPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel) benutzerdefiniert trocknen, aber die Synchronisierung mit dem UI-Thread kann nicht garantiert werden, und es kommt möglicherweise zu einer Verzögerung zwischen dem Rendern der Freihandeingabe in Ihrem SwapChainPanel und dem Entfernen der Freihandeingabe aus InkCanvas.

Ein vollständiges Beispiel für diese Funktion finden Sie unter [Komplexes Freihandbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620314).

> [!NOTE]
> Benutzerdefiniertes Trocknen und die [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> Wenn Ihre App das Standard-Renderverhalten für Freihandeingaben von [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) mit einer benutzerdefinierten Trockenimplementierung außer Kraft setzt, sind die gerenderten Freihandstriche für die InkToolbar nicht mehr verfügbar und die integrierten Löschbefehle der InkToolbar funktionieren nicht wie erwartet. Damit Sie Löschfunktionen bereitstellen können, müssen Sie alle Zeigerereignisse verarbeiten, für jeden Strich einen Treffertest ausführen und den integrierten Befehl „Freihand vollständig löschen“ außer Kraft setzen.

## <a name="other-articles-in-this-section"></a>Andere Artikel in diesem Abschnitt

| Thema | Beschreibung |
| --- | --- |
| [Erkennen von Freihandstrichen](convert-ink-to-text.md) | Konvertieren Sie letzte Striche mit der Schrifterkennung in Text oder mit der benutzerdefinierten Erkennung in Formen. |
| [Speichern und Abrufen von letzten Strichen](save-and-load-ink.md) | Speichern Sie Freihandstrichdaten mithilfe eingebetteter serialisierter Freihandformat-Metadaten (Ink Serialized Format, ISF) in einer GIF-Datei (Graphics Interchange Format). |
| [Hinzufügen eines InkToolbar-Elements zu einer UWP-App für die Freihandeingabe](ink-toolbar.md) | Fügen Sie einer App für die Freihandeingabe in der universellen Windows-Plattform (UWP) eine Standard-InkToolbar hinzu. Fügen Sie der InkToolbar einen anpassbaren Stift hinzu und binden Sie diesen an eine benutzerdefinierte Definition für den Stift. |

## <a name="related-articles"></a>Verwandte Artikel

* [Erste Schritte: Unterstützen von Freihandeingaben in Ihrer UWP-App](../../get-started/ink-walkthrough.md)
* [Behandeln von Zeigereingaben](handle-pointer-input.md)
* [Identifizieren von Eingabegeräten](identify-input-devices.md)

**APIs**

* [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
* [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
* [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)

**Beispiele**
* [Lernprogramm „Erste Schritte:” Unterstützen von Freihandeingaben in Ihrer UWP-App](https://aka.ms/appsample-ink)
* [Einfaches Freihandbeispiel (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Komplexes Freihandbeispiel (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Freihandbeispiel (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Malbuchbeispiel](https://aka.ms/cpubsample-coloringbook)
* [Familiennotizbeispiel](https://aka.ms/cpubsample-familynotessample)
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Beispiel für Eingabe mit niedriger Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Beispiel für XAML-Bildlauf, -Verschiebung und -Zoom](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Gesten und Manipulationen mit GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
