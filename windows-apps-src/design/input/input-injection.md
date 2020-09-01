---
Description: Simulieren und automatisieren Sie Eingaben von Geräten wie Tastatur, Maus, Toucheingabe, Stift und Gamepad in Ihren Windows-apps.
title: Simulieren der Benutzereingabe über die Eingabeeinfügung
label: Input injection
template: detail.hbs
keywords: Gerät, Digitalisierer, Eingabe, Interaktion, Injektion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f06414362b6a821233eabfb396ae59001f35c30d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156894"
---
# <a name="simulate-user-input-through-input-injection"></a>Simulieren der Benutzereingabe über die Eingabeeinfügung

Simulieren und automatisieren Sie Benutzereingaben von Geräten wie Tastatur, Maus, Toucheingabe, Stift und Gamepad in Ihren Windows-Anwendungen.

> **Wichtige APIs**: [ **Windows. UI. Input. Preview. Injection**](/uwp/api/windows.ui.input.preview.injection)

## <a name="overview"></a>Übersicht

Die Eingabe Injektion ermöglicht der Windows-Anwendung, Eingaben aus einer Vielzahl von Eingabegeräten zu simulieren und diese Eingabe an eine beliebige Stelle zu leiten, auch außerhalb des Client Bereichs der APP (auch für apps, die mit Administrator Rechten ausgeführt werden, z. b. dem Registrierungs-Editor).

Input Injection eignet sich für Windows-apps und-Tools, die Funktionen bereitstellen müssen, die Barrierefreiheit, Tests (Ad-hoc, automatisiert) sowie RAS-und Support Features enthalten.

## <a name="setup"></a>Einrichten

Wenn Sie die eingabeinjection-APIs in Ihrer Windows-App verwenden möchten, müssen Sie dem App-Manifest Folgendes hinzufügen:

1. Klicken Sie mit der rechten Maustaste auf die Datei **Package. appxmanifest** , und wählen Sie **Code anzeigen**
1. Fügen Sie Folgendes in den- `Package` Knoten ein:
    - `xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"`
    - `IgnorableNamespaces="rescap"`
1. Fügen Sie Folgendes in den- `Capabilities` Knoten ein:
    - `<rescap:Capability Name="inputInjectionBrokered" />`

## <a name="duplicate-user-input"></a>Doppelte Benutzereingaben

| ![Einschleusung der Eingabe Eingabe](images/injection/touch-input-injection.gif) | 
|:--:|
| *Einschleusung der Eingabe Eingabe* |

In diesem Beispiel wird veranschaulicht, wie die eingabeinjection-APIs ([Windows. UI. Input. Preview. Injection](/uwp/api/windows.ui.input.preview.injection)) verwendet werden, um Mauseingabe Ereignisse in einem Bereich einer APP zu überwachen und entsprechende Fingereingabe Ereignisse in einer anderen Region zu simulieren.

**Dieses Beispiel aus [Input Injection Sample (Maus auf berühren)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip) herunterladen**

1. Zuerst richten wir die Benutzeroberfläche ("MainPage. XAML") ein.

    Wir haben zwei Raster Bereiche (eine für die Mauseingabe und eine für die eingefügte Fingereingabe), die jeweils vier Schaltflächen enthalten.
      > [!NOTE] 
      > Dem Raster Hintergrund muss ein Wert zugewiesen werden ( `Transparent` in diesem Fall), andernfalls werden keine Zeiger Ereignisse erkannt.

    Wenn im Eingabebereich beliebige Mausklicks erkannt werden, wird ein entsprechendes Berührungs Ereignis in den eingabeinjektions Bereich eingefügt. Schaltflächen Klicks aus Eingabe einfügen werden im Titelbereich angezeigt.

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

2. Als nächstes initialisieren wir unsere app.
    
    In diesem Code Ausschnitt deklarieren wir unsere globalen Objekte und deklarieren Listener für Zeiger Ereignisse ([AddHandler](/uwp/api/windows.ui.xaml.uielement.addhandler)) im Eingabebereich der Maus, die in den Click-Ereignissen der Schaltfläche als behandelt markiert werden könnten.

    Das [inputinjetor](/uwp/api/windows.ui.input.preview.injection.inputinjector) -Objekt stellt das virtuelle Eingabegerät zum Senden der Eingabedaten dar.

    Im- `ContainerInput_PointerPressed` Handler wird die Funktion für die Berührungs Injektion aufgerufen.

    Im- `ContainerInput_PointerReleased` Handler wird "uninitializetouchinjection" aufgerufen, um das [inputinjetor](/uwp/api/windows.ui.input.preview.injection.inputinjector) -Objekt zu schließen.

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
3. Hier ist die einschleusungs Funktion der Fingereingabe.

    Zuerst wird [TryCreate](/uwp/api/windows.ui.input.preview.injection.inputinjector.trycreate) aufgerufen, um das [inputinjetor](/uwp/api/windows.ui.input.preview.injection.inputinjector) -Objekt zu instanziieren.

    Dann rufen wir [initializetouchinjection](/uwp/api/windows.ui.input.preview.injection.inputinjector.initializetouchinjection) mit einem [injetedinputvisualizationmode](/uwp/api/windows.ui.input.preview.injection.injectedinputvisualizationmode) von auf `Default` .

    Nach dem Berechnen der Einschleusung der Injektion rufen wir " [injetedinputtouchinfo](/uwp/api/windows.ui.input.preview.injection.injectedinputtouchinfo) " auf, um die Liste der einzufügenden Berührungspunkte zu initialisieren (in diesem Beispiel erstellen wir einen Fingerabdruck, der dem Mauseingabe Zeiger entspricht).

    Schließlich wird " [injettouchinput](/uwp/api/windows.ui.input.preview.injection.inputinjector.injecttouchinput) " zweimal aufgerufen, der erste für einen Zeiger nach unten und der zweite für einen Zeiger.

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

4. Schließlich behandeln wir beliebige Schaltflächen- [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase) -Routing Ereignisse im Eingabe einschleusungs Bereich und aktualisieren die Benutzeroberfläche mit dem Namen der Schaltfläche, auf die geklickt wird.

## <a name="see-also"></a>Weitere Informationen

### <a name="topic-samples"></a>Themenbeispiele

- [Eingabe einschleusungs Beispiel (Maus zur Berührung)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip)