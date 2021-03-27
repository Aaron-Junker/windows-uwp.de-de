---
description: Erstellen Sie Windows-apps, die benutzerdefinierte Interaktionen von Stift-und Tablettstiftgeräten unterstützen, einschließlich digitaler Hand Eingaben für das natürliche schreiben und zeichnen.
title: Stiftinteraktionen und Windows Ink in Windows-Apps
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in Windows apps
template: detail.hbs
keywords: Windows Ink, Windows-Freihandeingabe, DirectInk, InkPresenter, InkCanvas, Handschrifterkennung, Benutzerinteraktion, Eingabe
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 231145a5f8a9b44b4dc6060a6b02d55e007704e4
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619586"
---
# <a name="pen-interactions-and-windows-ink-in-windows-apps"></a>Stiftinteraktionen und Windows Ink in Windows-Apps

![Das herabbild des Oberflächen Stifts.](images/ink/hero-small.png)  
*Surface-Stift* (zum Kauf im [Microsoft Store](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b) verfügbar).

## <a name="overview"></a>Übersicht

Optimieren Sie Ihre Windows-App für Stift Eingaben, um sowohl Standard [**Zeiger-Geräte**](/uwp/api/Windows.Devices.Input.PointerDevice) Funktionen als auch die beste Windows-Handschrift für Ihre Benutzer bereitzustellen.

> [!NOTE]
> Der Schwerpunkt dieses Themas liegt auf der Windows Ink-Plattform. Informationen zur allgemeinen Behandlung von Zeigereingaben (ähnlich Maus-, Touch- und Touchpadeingaben) finden Sie unter [Behandeln von Zeigereingaben](handle-pointer-input.md).

:::row:::
   :::column:::
      <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe>

      *Verwenden von frei Hand Eingaben in Ihrer Windows-App*
   :::column-end:::
   :::column:::
      <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe>

      *Verwenden von Windows Pen und Ink zum Erstellen von stärker interaktiven Unternehmens-Apps*
   :::column-end:::
:::row-end:::

In Verbindung mit einem Zeichengerät bietet die Windows Ink-Plattform eine natürliche Möglichkeit, digitale handschriftliche Notizen, Zeichnungen und Anmerkungen zu erstellen. Sie können auf der Plattform Freihanddaten aus einem Eingabedigitalisierungsgerät erfassen, Freihanddaten generieren, verwalten und als letzte Striche auf dem Ausgabegerät rendern sowie über die Schrifterkennung in Text umwandeln.

Ihre App kann nicht nur die grundlegende Position und Bewegung des Stifts aufzeichnen, während der Benutzer schreibt oder zeichnet, sondern auch den variierenden Druck während des gesamten Strichs nachverfolgen und erfassen. Mit diesen Informationen, zusammen mit Einstellungen für Form und Größe der Stiftspitze, Drehung, Freihandfarbe und Zweck (einfache Freihandeingabe, Löschen, Hervorheben und Auswählen), können Sie dem Benutzer ermöglichen, auf ähnliche Weise wie mit einem Stift, Bleistift oder Pinsel auf Papier zu arbeiten.

> [!NOTE]
> Ihre App kann auch Freihandeingaben von anderen zeigerbasierten Geräten, z. B. Touchdigitalisierungs- und Mausgeräte, unterstützen. 

Die Freihandplattform ist sehr flexibel. Je nach Ihren Anforderungen unterstützt sie verschiedene Funktionalitätsgrade.

Richtlinien für die Benutzeroberfläche von Windows Ink finden Sie unter [Inking controls](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Komponenten der Windows Ink-Plattform

| Komponente | BESCHREIBUNG |
| --- | --- |
| [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | Ein XAML-UI-Plattformsteuerelement, das standardmäßig alle Eingaben von einem Stift als Freihandstriche oder Löschen von Freihandstrichen empfängt und anzeigt.<br/>Weitere Informationen zur Verwendung von InkCanvas finden Sie unter [Erkennen von Windows Ink-Strichen als Text](convert-ink-to-text.md) und [Speichern und Abrufen der Daten von Windows Ink-Strichen](save-and-load-ink.md). |
| [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Ein CodeBehind-Objekt, das zusammen mit einem [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement instanziiert wird (über die [**InkCanvas.InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Eigenschaft verfügbar gemacht). Dieses Objekt stellt alle Standardfreihandfunktionen bereit, die vom **InkCanvas**-Steuerelement zur Verfügung gestellt werden, sowie einen umfassenden Satz von APIs für zusätzliche Anpassung und Personalisierung.<br/>Weitere Informationen zur Verwendung von InkPresenter finden Sie unter [Erkennen von Windows Ink-Strichen als Text](convert-ink-to-text.md) und [Speichern und Abrufen der Daten von Windows Ink-Strichen](save-and-load-ink.md). |
| [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) | Ein XAML-UI-Platt Form Steuerelement, das eine anpassbare und erweiterbare Auflistung von Schaltflächen enthält, die frei Hand Funktionen in einem zugeordneten [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)aktivieren.<br/>Weitere Informationen zur Verwendung von inktoolbar finden [Sie unter Hinzufügen einer inktoolbar zu einer Windows-App zum Freihand-App](ink-toolbar.md). |
| [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) | Ermöglicht das Rendern von Freihandstrichen im angegebenen Direct2D-Gerätekontext einer universellen Windows-App statt im standardmäßigen [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement. Dies ermöglicht die umfassende Anpassung der Freihandfunktionen.<br/>Weitere Informationen finden Sie in diesem [komplexen Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk). |

## <a name="basic-inking-with-inkcanvas"></a>Einfaches Freihandzeichnen mit „InkCanvas“

Um grundlegende Funktionen für die Einbindung hinzuzufügen, platzieren Sie einfach ein [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) -UWP-Platt Form Steuerelement auf der entsprechenden Seite in Ihrer APP.

Das [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement unterstützt standardmäßig nur Freihandeingaben mit einem Stift. Die Eingabe wird entweder als Strich mit den Standardeinstellungen für Farbe und Stärke gerendert (ein schwarzer Kugelschreiber mit einer Stärke von 2 Pixeln) oder als Strichradierer behandelt (wenn die Eingabe von einer mit einer Löschschaltfläche geänderten Radiergummi- oder Stiftspitze stammt).

> [!NOTE]
> Falls keine Radiergummispitze bzw. -schaltfläche vorhanden ist, kann InkCanvas so konfiguriert werden, dass Eingaben mit der Stiftspitze wie Radierstriche behandelt werden.

In diesem Beispiel überlagert ein [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement ein Hintergrundbild.

> [!NOTE]
> Ein InkCanvas-Steuerelement verfügt über die Standardeigenschaften für [**Höhe**](/uwp/api/windows.ui.xaml.frameworkelement.Height) und [**Breite**](/uwp/api/windows.ui.xaml.frameworkelement.Width) von 0 (null), es sei denn, es handelt sich um ein untergeordnetes [](/uwp/api/windows.ui.xaml.controls.grid) Element eines Elements, das seine untergeordneten Elemente [, wie z](/uwp/api/windows.ui.xaml.controls.stackpanel) . b

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

Diese Serie von Bildern zeigt, wie die Stifteingabe von diesem [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement gerendert wird.

| ![Screenshot des leeren InkCanvas mit einem Hintergrundbild.](images/ink_basic_1_small.png) | ![Screenshot des InkCanvas mit Ink-Strichen.](images/ink_basic_2_small.png) | ![Screenshot des InkCanvas mit einem gelöschten Strich.](images/ink_basic_3_small.png) |
| --- | --- | ---|
| Der leere [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) mit einem Hintergrundbild. | Der [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) mit Freihand Strichen. | Der [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) mit einem ausradierten Strich (beachten Sie, dass jeweils der gesamte Strich und nicht nur auf einen Teil davon ausradiert wird). |

Die vom [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement unterstützte Freihandfunktion wird von einem CodeBehind-Objekt mit dem Namen [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) bereitgestellt.

Für die einfache Freihandeingabe müssen Sie sich nicht mit [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) befassen. Wenn Sie jedoch das Freihandverhalten des [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements anpassen und konfigurieren möchten, müssen Sie auf das entsprechende **InkPresenter**-Objekt zugreifen.

## <a name="basic-customization-with-inkpresenter"></a>Einfache Anpassung mit „InkPresenter“

Für jedes [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement wird ein [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)-Objekt instanziiert.

> [!NOTE]
> Das [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)-Objekt kann nicht direkt instanziiert werden. Stattdessen erfolgt der Zugriff über die [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) -Eigenschaft von [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas). 

Neben dem Bereitstellen sämtlicher Standardverhalten für die Erfassung des entsprechenden [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) -Steuer Elements bietet [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) einen umfassenden Satz von APIs für die zusätzliche Hubanpassung und eine präzisere Verwaltung der Stift Eingabe (Standard und geändert). Dies umfasst Stroke-Eigenschaften, unterstützte Eingabegeräte Typen und ob Eingaben vom-Objekt verarbeitet oder zur Verarbeitung an die APP übermittelt werden.

> [!NOTE]
> Standard-frei Hand Eingaben (von der Stift-Tip-oder radiererschaltfläche) werden nicht mit einem sekundären hardwarevirtualisierungsverfahren, wie z. b. einem Stift-oder einem ähnlichen Mechanismus, geändert. 

Standardmäßig wird Ink nur für Stift Eingaben unterstützt. Hier konfigurieren wir [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter), damit Eingabedaten von Stift und Maus als letzte Striche interpretiert werden. Wir legen außerdem einige anfängliche Freihand Strich Attribute fest, die zum Rendern von Strichen im [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)verwendet werden.

Legen Sie die [**inputdebug-YPES**](/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) -Eigenschaft von [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) auf die Kombination der gewünschten [**coreinputabvicetypes**](/uwp/api/windows.ui.core.coreinputdevicetypes) -Werte fest, um die Erfassung von Maus-und Berührungs Eingaben zu aktivieren.

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

Diese Bilder zeigen, wie die Stifteingabe vom [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt verarbeitet und angepasst wird.

:::row:::
   :::column span="":::
      ![Screenshot, der den InkCanvas mit standardmäßigen schwarzen Hand Strichen anzeigt.](images/ink-basic-custom-1-small.png)
   :::column-end:::
   :::column span="":::
      Der [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) mit standardmäßigen schwarzen Hand Strichen.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      ![Screenshot des InkCanvas mit vom Benutzer ausgewählten roten Hand Strichen.](images/ink-basic-custom-2-small.png)
   :::column-end:::
   :::column span="":::
      Der [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) , bei dem vom Benutzer ausgewählte Rote Hand Striche angezeigt werden.
   :::column-end:::
:::row-end:::

Um zusätzlich zur Freihandeingabe und zum Löschen weitere Funktionen wie etwa die Strichauswahl bereitzustellen, muss die App bestimmte Eingaben identifizieren, die vom [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt ohne Verarbeitung zur Behandlung an die App weitergegeben werden.

## <a name="pass-through-input-for-advanced-processing"></a>Weitergabe der Eingabe für die erweiterte Verarbeitung

Standardmäßig verarbeitet [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) alle Eingaben entweder als frei Hand Strich oder als Lösch Strich, einschließlich der Eingabe, die durch ein sekundäres Hardware Verfahren wie z. b. eine Stift Taste, eine Rechte Maustaste oder ähnliches geändert wird. Benutzer erwarten jedoch in der Regel einige zusätzliche Funktionen oder verändertes Verhalten bei diesen sekundären Kosten.

In einigen Fällen müssen Sie möglicherweise zusätzliche Funktionen für Stifte ohne sekundäre Kosten (Funktionen, die normalerweise nicht mit der Stift Info verknüpft sind), andere Eingabegeräte Typen oder ein verändertes Verhalten auf der Grundlage einer Benutzer Auswahl in der Benutzeroberfläche Ihrer app verfügbar machen.

Das [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt kann zu diesem Zweck so konfiguriert werden, dass bestimmte Eingaben unverarbeitet bleiben. Diese unverarbeiteten Eingaben werden dann zur Verarbeitung an die App weitergegeben.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>Beispiel: Verwenden der nicht verarbeiteten Eingabe zum Implementieren der Strich Auswahl 

Die Windows Ink-Plattform bietet keine integrierte Unterstützung für Aktionen, für die geänderte Eingaben erforderlich sind, wie z. b. die Strich Auswahl. Um Funktionen wie diese zu unterstützen, müssen Sie eine benutzerdefinierte Lösung in ihren apps bereitstellen. 

Im folgenden Codebeispiel (sämtlicher Code finden Sie in den Dateien MainPage. XAML und MainPage. XAML. cs), wie Sie die Strich Auswahl aktivieren, wenn die Eingabe mit einer Stift Taste (oder mit der rechten Maustaste) geändert wird.

1.  Zunächst richten wir in „MainPage.xaml“ die Benutzeroberfläche ein.

    Hier fügen wir einen Zeichenbereich (unterhalb des [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements) zum Zeichnen der Strichauswahl hinzu. Durch die Verwendung einer eigenen Ebene zum Zeichnen der Strichauswahl bleiben das **InkCanvas**-Steuerelement und dessen Inhalt unverändert.

    ![Screenshot des leeren InkCanvas mit einem zugrunde liegenden Auswahl Zeichenbereich.](images/ink-unprocessed-1-small.png)

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

3.  Anschließend konfigurieren wir das [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekt so, dass es Eingabedaten von Stift und Maus als Freihandstriche interpretiert. Zudem legen wir anfängliche Freihandstrichattribute zum Rendern von Freihandstrichen mit dem [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement fest.

    Vor allem geben wir mithilfe der [**InputProcessingConfiguration**](/uwp/api/windows.ui.input.inking.inkpresenter.inputprocessingconfiguration)-Eigenschaft von [InkPresenter](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) an, dass alle geänderten Eingaben von der App verarbeitet werden sollen. Geänderte Eingaben werden angegeben, indem **inputprocessingconfiguration. rightdragaction** der Wert [**inkinpuzghtdragaction. leaveunprocessing**](/uwp/api/Windows.UI.Input.Inking.InkInputRightDragAction)zugewiesen wird. Wenn dieser Wert festgelegt ist, übergibt [InkPresenter](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) an die [inkunprocessedinput](/uwp/api/windows.ui.input.inking.inkunprocessedinput) -Klasse, eine Reihe von Zeiger Ereignissen, die Sie behandeln können.

    Wir weisen Listener für die nicht verarbeiteten Ereignisse " [**pointerpressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed)", " [**pointerverschobe**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved)" und " [**pointerreleased**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) " zu, die von [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)weitergeleitet werden. Sämtliche Auswahlfunktionalität wird in den Handlern für diese Ereignisse implementiert.

    Schließlich weisen wir Listener für die Ereignisse [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) und [**StrokesErased**](/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) des [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)-Objekts zu. Wir verwenden die Handler für diese Ereignisse, um die Auswahl-UI zu bereinigen, wenn ein neuer Strich begonnen oder ein vorhandener Strich ausradiert wird.

    ![Screenshot der Beispiel-App mit der Fortschritt Enden frei Hand Eingabe, die den InkCanvas mit standardmäßigen schwarzen Hand Strichen anzeigt](images/ink-unprocessed-2-small.png)

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

4.  Anschließend definieren wir Handler für die nicht verarbeiteten Ereignisse " [**pointerpressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed)", " [**pointerverschoben**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved)" und " [**pointerreleased**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) ", die von " [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)" weitergegeben werden.

    Sämtliche Auswahlfunktionalität, einschließlich des Lassostrichs und des umgebenden Rechtecks, wird in diesen Handlern implementiert.

    ![Screenshot der Auswahl-Lasso.](images/ink-unprocessed-3-small.png)

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

    ![Screenshot der Begrenzungs-Rect-Auswahl.](images/ink-unprocessed-4-small.png)

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

6.  Schließlich definieren wir Handler für die InkPresenter-Ereignisse [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) und [**StrokesErased**](/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased).

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

Standardmäßig wird die frei Hand Eingabe in einem Hintergrund Thread mit niedriger Latenz verarbeitet und in Bearbeitung gerendert oder "nass", wenn Sie gezeichnet wird. Wenn der Strich abgeschlossen ist (der Stift oder Finger wurde angehoben oder die Maustaste losgelassen), wird er im UI-Thread verarbeitet und auf der [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Ebene „trocken“ gerendert (über dem Anwendungsinhalt, wo er die nasse Freihandeingabe ersetzt).

Sie können dieses Standardverhalten außer Kraft setzen und die Einbindung vollständig durch "benutzerdefiniertes trocknen" der nassen Hand Striche steuern. Das Standardverhalten ist zwar in der Regel für die meisten Anwendungen ausreichend, es gibt jedoch einige Fälle, in denen eine benutzerdefinierte Trocknung erforderlich ist. dazu gehören:
- Effizientere Verwaltung großer oder komplexer Sammlungen von Hand Strichen
- Effizientere Unterstützung von schwenken und Zoomen für große Freihand-kanvasen
- Austausch von frei Hand Eingaben und anderen Objekten, z. b. Formen oder Text, während die z-Reihenfolge beibehalten wird 
- Das synchrone Trocknen und Umlagern von frei Hand Eingaben in eine DirectX-Form (z. b. eine gerade Linie oder Form, die in Anwendungs Inhalte und nicht als separate **InkCanvas** -Schicht integriert und integriert wird).

Benutzerdefiniertes Trocknen erfordert anstelle des standardmäßigen [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements ein [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer)-Objekt, um die Freihandeingabe zu verwalten und im Direct2D-Gerätekontext der universellen Windows-App zu rendern.

Eine App erstellt durch Aufruf von [**ActivateCustomDrying**](/uwp/api/windows.ui.input.inking.inkpresenter.activatecustomdrying) (vor dem Laden des [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelements) ein [**InkSynchronizer**](/uwp/api/Windows.UI.Input.Inking.InkSynchronizer)-Objekt, um zu definieren, wie ein letzter Strich trocken in einer [**SurfaceImageSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)- oder [**VirtualSurfaceImageSource**](/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource)-Klasse gerendert wird. 

Sowohl " [**surfaceimagesource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) " als auch " [**virtualsurfaceimagesource**](/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) " bieten eine freigegebene DirectX-Oberfläche für Ihre APP, die in den Inhalt Ihrer Anwendung hineingezogen und zusammengesetzt werden kann. VSIs bietet jedoch eine virtuelle Oberfläche, die größer ist als der Bildschirm für leistungsstarke schwenken und Zoomen. Da visuelle Updates für diese Oberflächen mit dem XAML-UI-Thread synchronisiert werden, kann das nasse frei Handzeichen gleichzeitig aus dem InkCanvas entfernt werden, wenn freigegeben wird. 

Sie können auch benutzerdefinierte trocken Hand Eingaben an einen "Swap"- [Austausch einfügen](/uwp/api/windows.ui.xaml.controls.swapchainpanel), die Synchronisierung mit dem UI-Thread ist jedoch nicht gewährleistet, und es kann eine Verzögerung zwischen dem Rendern der frei Hand Eingabe in Ihrem "Swap" und "Ink" aus der InkCanvas auftreten.

Ein vollständiges Beispiel für diese Funktionalität finden Sie im Beispiel [Complex Ink](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk).

> [!NOTE]
> Benutzerdefiniertes Trocknen und die [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> Wenn Ihre App das Standard-Renderverhalten für Freihandeingaben von [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) mit einer benutzerdefinierten Trockenimplementierung außer Kraft setzt, sind die gerenderten Freihandstriche für die InkToolbar nicht mehr verfügbar und die integrierten Löschbefehle der InkToolbar funktionieren nicht wie erwartet. Damit Sie Löschfunktionen bereitstellen können, müssen Sie alle Zeigerereignisse verarbeiten, für jeden Strich einen Treffertest ausführen und den integrierten Befehl „Freihand vollständig löschen“ außer Kraft setzen.

## <a name="other-articles-in-this-section"></a>Andere Artikel in diesem Abschnitt

| Thema | BESCHREIBUNG |
| --- | --- |
| [Erkennen von Freihandstrichen](convert-ink-to-text.md) | Konvertieren Sie letzte Striche mit der Schrifterkennung in Text oder mit der benutzerdefinierten Erkennung in Formen. |
| [Speichern und Abrufen von Freihandstrichen](save-and-load-ink.md) | Speichern Sie Freihandstrichdaten mithilfe eingebetteter serialisierter Freihandformat-Metadaten (Ink Serialized Format, ISF) in einer GIF-Datei (Graphics Interchange Format). |
| [Hinzufügen einer inktoolbar zu einer Windows Freihand-App](ink-toolbar.md) | Fügen Sie eine Standard-inktoolbar zu einer Windows-App zum Einbinden der APP hinzu, fügen Sie der inktoolbar eine benutzerdefinierte Stift Schaltfläche hinzu, und binden Sie die benutzerdefinierte Stift Schaltfläche an eine benutzerdefinierte |

## <a name="related-articles"></a>Verwandte Artikel

- [Einstieg: Unterstützung von frei Hand Eingaben in Ihrer Windows-App](./ink-walkthrough.md)
- [Behandeln von Zeigereingaben](handle-pointer-input.md)
- [Identifizieren von Eingabegeräten](identify-input-devices.md)

### <a name="apis"></a>APIs

- [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)
- [**Windows.UI.Input.Inking**](/uwp/api/Windows.UI.Input.Inking)
- [**Windows. UI. Input. Inking. Core**](/uwp/api/Windows.UI.Input.Inking.Core)

### <a name="samples"></a>Beispiele

- [Tutorial zu den ersten Schritten: Unterstützung von frei Hand Eingaben in Ihrer Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Simple Ink Sample (c#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Beispiel für Complex Ink (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink-Beispiel (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Malbuchbeispiel](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Familiennotizbeispiel](https://github.com/Microsoft/Windows-appsample-familynotes)
- [Einfaches Eingabebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabebeispiel mit geringer Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für Gerätefunktionen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Eingabe: Gesten und Manipulationen mit GestureRecognizer](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
