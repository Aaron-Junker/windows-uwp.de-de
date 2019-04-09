---
Description: Simulieren und Eingabe von Geräten, z. B. Tastatur, Maus, Touch, Stift und Gamepad in UWP-apps automatisieren.
title: Simulieren der Benutzereingabe über die Eingabeeinfügung
label: Input injection
template: detail.hbs
keywords: Gerät, Digitalisierer, Eingabe, Interaktion, Einfügung
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2699917beed9330dd2044704f19b3c25256c3ca8
ms.sourcegitcommit: 7676d4b4c323e665302c2dfca3c763751a47afa3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58343239"
---
# <a name="simulate-user-input-through-input-injection"></a>Simulieren der Benutzereingabe über die Eingabeeinfügung

Sie können Benutzereingaben über Geräte wie Tastatur, Maus, Touch, Stift und Gamepad in Ihren UWP-Anwendungen simulieren und automatisieren.

> **Wichtige APIs:** [**Windows.UI.Input.Preview.Injection**](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection)

## <a name="overview"></a>Übersicht

Über die Eingabeeinfügung kann Ihre UWP-Anwendung die Eingabe über eine Vielzahl von Eingabegeräten simulieren und diese Eingabe überall hinleiten, z. B. außerhalb des Clientbereichs Ihrer App (selbst zu Apps, die mit Administratorberechtigungen ausgeführt werden, wie etwa der Registrierungs-Editor).

Die Eingabeeinfügung ist nützlich für UWP-Apps und -Tools, die eine Funktionalität mit Bedienungshilfen, Testfunktionen (ad hoc, automatisiert) und Remotezugriff sowie Supportfeatures bereitstellen müssen.

## <a name="setup"></a>Setup

Um die Eingabeeinfügungs-APIs in Ihrer UWP-App verwenden zu können, müssen Sie Folgendes zum App-Manifest hinzufügen:

1. Klicken Sie mit der rechten Maustaste auf die Datei **Package.appxmanifest**, und wählen Sie die Option **Code anzeigen** aus.
1. Fügen Sie Folgendes in den Knoten `Package` ein:
    - `xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"`
    - `IgnorableNamespaces="rescap"`
1. Fügen Sie Folgendes in den Knoten `Capabilities` ein:
    - `<rescap:Capability Name="inputInjectionBrokered" />`

## <a name="duplicate-user-input"></a>Duplizieren der Benutzereingabe

| ![Beispiel für die Touch-Eingabeeinfügung](images/injection/touch-input-injection.gif) | 
|:--:|
| *Touch-Eingabe-Injection-Beispiel* |

In diesem Beispiel wird veranschaulicht, wie Sie die Eingabeeinfügungs-APIs ([Windows.UI.Input.Preview.Injection](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection)) verwenden, um Mauseingabeereignisse in einem Bereich einer App zu überwachen und entsprechende Toucheingabe-Ereignisse in einem anderen Bereich zu simulieren.

**Laden Sie dieses Beispiel von [Injection-Beispiel für Eingabe (Maus, touch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip)**

1. Zunächst richten wir die Benutzeroberfläche ein (MainPage.xaml).

    Wir verfügen über zwei Rasterbereiche (einen für die Mauseingabe und einen für die eingefügte Toucheingabe), jeweils mit vier Schaltflächen.
       > [!NOTE] The Grid background must be assigned a value (`Transparent`, in this case), otherwise pointer events are not detected.

    Wenn im Eingabebereich Mausklicks erkannt werden, wird ein entsprechendes Touchereignis in den Eingabeeinfügungsbereich eingefügt. Klicks auf Schaltflächen von der Eingabeeinfügung werden im Titelbereich gemeldet.

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel Grid.Row="0"
                    Margin="10">
            <TextBlock Style="{ThemeResource TitleTextBlockStyle}" 
                       Name="titleText"
                       Text="Touch input injection"
                       HorizontalTextAlignment="Center" />
            <TextBlock Style="{ThemeResource BodyTextBlockStyle}"
                       Name="statusText"
                       HorizontalTextAlignment="Center" />
        </StackPanel>
        <Grid HorizontalAlignment="Center"
                        Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Column="0" 
                       Grid.Row="0" 
                       Style="{ThemeResource CaptionTextBlockStyle}"
                       Text="User mouse input area"/>
            <!-- Background must be set to something, otherwise pointer events are not detected. -->
            <Grid Name="ContainerInput" 
                  Grid.Column="0" 
                  Grid.Row="1"
                  HorizontalAlignment="Stretch" 
                  Background="Transparent" 
                  BorderBrush="Green" 
                  BorderThickness="2" 
                  MinHeight="100" MinWidth="300" 
                  Margin="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Name="B1" 
                        Grid.Column="0" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B1" />
                <Button Name="B2" 
                        Grid.Column="1" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B2" />
                <Button Name="B3" 
                        Grid.Column="2" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B3" />
                <Button Name="B4" 
                        Grid.Column="3" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B4" />
            </Grid>
            <TextBlock Grid.Column="1" 
                       Grid.Row="0"                         
                       Style="{ThemeResource CaptionTextBlockStyle}"
                       Text="Injected touch input area"/>
            <Grid Name="ContainerInject"
                  Grid.Column="1"  
                  Grid.Row="1"
                  HorizontalAlignment="Stretch"
                  BorderBrush="Red" 
                  BorderThickness="2" 
                  MinHeight="100" MinWidth="300" 
                  Margin="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Name="B1i" Click="Button_Click_Injected"
                        Content="B1i"
                        Grid.Column="0" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B2i" Click="Button_Click_Injected"
                        Content="B2i"
                        Grid.Column="1" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B3i" Click="Button_Click_Injected"
                        Content="B3i"
                        Grid.Column="2" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B4i" Click="Button_Click_Injected"
                        Content="B4i"
                        Grid.Column="3" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
            </Grid>
        </Grid>
    </Grid>
    ```

2. Als Nächstes initialisieren wir unsere App.
    
    In diesem Codeausschnitt deklarieren wir unsere globalen Objekte und Listener für Zeigerereignisse ([AddHandler](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)) innerhalb des Mauseingabebereichs, die in den Schaltflächenklickereignissen möglicherweise als verarbeitet gekennzeichnet sind.

    Das [InputInjector](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection.inputinjector)-Objekt stellt das virtuelle Eingabegerät zum Senden der eingegebenen Daten dar.

    Im Handler `ContainerInput_PointerPressed` rufen wir die Toucheinfügungsfunktion auf.

    Im Handler `ContainerInput_PointerReleased` rufen wir UninitializeTouchInjection auf, um das [InputInjector](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection.inputinjector)-Objekt zu beenden.

    ```csharp
    public sealed partial class MainPage : Page
    {
        /// <summary>
        /// The virtual input device.
        /// </summary>
        InputInjector _inputInjector;

        /// <summary>
        /// Initialize the app, set the window size, 
        /// and add pointer input handlers for the container.
        /// </summary>
        public MainPage()
        {
            this.InitializeComponent();

            ApplicationView.PreferredLaunchViewSize =
                new Size(600, 200);
            ApplicationView.PreferredLaunchWindowingMode =
                ApplicationViewWindowingMode.PreferredLaunchViewSize;

            // Button handles PointerPressed/PointerReleased in 
            // the Tapped routed event, but we need the container Grid 
            // to handle them also. Add a handler for both 
            // PointerPressedEvent and PointerReleasedEvent on the input Grid 
            // and set handledEventsToo to true.
            ContainerInput.AddHandler(PointerPressedEvent,
                new PointerEventHandler(ContainerInput_PointerPressed), true);
            ContainerInput.AddHandler(PointerReleasedEvent,
                new PointerEventHandler(ContainerInput_PointerReleased), true);
        }

        /// <summary>
        /// PointerReleased handler for all pointer conclusion events.
        /// PointerPressed and PointerReleased events do not always occur 
        /// in pairs, so your app should listen for and handle any event that 
        /// might conclude a pointer down (such as PointerExited, PointerCanceled, 
        /// and PointerCaptureLost).  
        /// </summary>
        /// <param name="sender">Source of the click event</param>
        /// <param name="e">Event args for the button click routed event</param>
        private void ContainerInput_PointerReleased(
            object sender, PointerRoutedEventArgs e)
        {
            // Prevent most handlers along the event route from handling event again.
            e.Handled = true;

            // Shut down the virtual input device.
            _inputInjector.UninitializeTouchInjection();
        }

        /// <summary>
        /// PointerPressed handler.
        /// PointerPressed and PointerReleased events do not always occur 
        /// in pairs. Your app should listen for and handle any event that 
        /// might conclude a pointer down (such as PointerExited, 
        /// PointerCanceled, and PointerCaptureLost).  
        /// </summary>
        /// <param name="sender">Source of the click event</param>
        /// <param name="e">Event args for the button click routed event</param>
        private void ContainerInput_PointerPressed(
            object sender, PointerRoutedEventArgs e)
        {
            // Prevent most handlers along the event route from 
            // handling the same event again.
            e.Handled = true;

            InjectTouchForMouse(e.GetCurrentPoint(ContainerInput));

        }
        ...
    }
    ```
3. Dies ist die Toucheingabeeinfügungsfunktion.

    Zuerst rufen wir [TryCreate](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection.inputinjector.trycreate) auf, um das [InputInjector](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection.inputinjector)-Objekt zu instanziieren.

    Anschließend rufen wir [InitializeTouchInjection](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection.inputinjector.initializetouchinjection) mit dem [InjectedInputVisualizationMode](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection.injectedinputvisualizationmode) von `Default` auf.

    Nach der Berechnung des Einfügungspunktes rufen wir [InjectedInputTouchInfo](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection.injectedinputtouchinfo) auf, um die Liste der Touchpunkte zum Einfügen zu initialisieren (in diesem Beispiel erstellen wir einen Touchpunkt entsprechend dem Mauseingabezeiger).

    Abschließend rufen wir zweimal [InjectTouchInput](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.injection.inputinjector.injecttouchinput) auf, einmal für einen Zeiger nach unten und einmal für einen Zeiger nach oben.

    ```csharp
    /// <summary>
    /// Inject touch input on injection target corresponding 
    /// to mouse click on input target.
    /// </summary>
    /// <param name="pointerPoint">The mouse click pointer.</param>
    private void InjectTouchForMouse(PointerPoint pointerPoint)
    {
        // Create the touch injection object.
        _inputInjector = InputInjector.TryCreate();

        if (_inputInjector != null)
        {
            _inputInjector.InitializeTouchInjection(
                InjectedInputVisualizationMode.Default);

            // Create a unique pointer ID for the injected touch pointer.
            // Multiple input pointers would require more robust handling.
            uint pointerId = pointerPoint.PointerId + 1;

            // Get the bounding rectangle of the app window.
            Rect appBounds =
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

            // Get the top left screen coordinates of the app window rect.
            Point appBoundsTopLeft = new Point(appBounds.Left, appBounds.Top);

            // Get a reference to the input injection area.
            GeneralTransform injectArea =
                ContainerInject.TransformToVisual(Window.Current.Content);

            // Get the top left screen coordinates of the input injection area.
            Point injectAreaTopLeft = injectArea.TransformPoint(new Point(0, 0));

            // Get the screen coordinates (relative to the input area) 
            // of the input pointer.
            int pointerPointX = (int)pointerPoint.Position.X;
            int pointerPointY = (int)pointerPoint.Position.Y;

            // Create the point for input injection and calculate its screen location.
            Point injectionPoint =
                new Point(
                    appBoundsTopLeft.X + injectAreaTopLeft.X + pointerPointX,
                    appBoundsTopLeft.Y + injectAreaTopLeft.Y + pointerPointY);

            // Create a touch data point for pointer down.
            // Each element in the touch data list represents a single touch contact. 
            // For this example, we're mirroring a single mouse pointer.
            List<InjectedInputTouchInfo> touchData =
                new List<InjectedInputTouchInfo>
                {
                    new InjectedInputTouchInfo
                    {
                        Contact = new InjectedInputRectangle
                        {
                            Left = 30, Top = 30, Bottom = 30, Right = 30
                        },
                        PointerInfo = new InjectedInputPointerInfo
                        {
                            PointerId = pointerId,
                            PointerOptions =
                            InjectedInputPointerOptions.PointerDown |
                            InjectedInputPointerOptions.InContact |
                            InjectedInputPointerOptions.New,
                            TimeOffsetInMilliseconds = 0,
                            PixelLocation = new InjectedInputPoint
                            {
                                PositionX = (int)injectionPoint.X ,
                                PositionY = (int)injectionPoint.Y
                            }
                    },
                    Pressure = 1.0,
                    TouchParameters =
                        InjectedInputTouchParameters.Pressure |
                        InjectedInputTouchParameters.Contact
                }
            };

            // Inject the touch input. 
            _inputInjector.InjectTouchInput(touchData);

            // Create a touch data point for pointer up.
            touchData = new List<InjectedInputTouchInfo>
            {
                new InjectedInputTouchInfo
                {
                    PointerInfo = new InjectedInputPointerInfo
                    {
                        PointerId = pointerId,
                        PointerOptions = InjectedInputPointerOptions.PointerUp
                    }
                }
            };

            // Inject the touch input. 
            _inputInjector.InjectTouchInput(touchData);
        }
    }
    ```

4. Schließlich verarbeiten wir alle [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase)-Weiterleitungsereignisse im Eingabeeinfügungsbereich und aktualisieren die Benutzeroberfläche mit dem Namen der angeklickten Schaltfläche.

## <a name="see-also"></a>Siehe auch

### <a name="topic-samples"></a>Themenbeispiele

- [Eingabe-Injection-Beispiel (Maus, touch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip)
