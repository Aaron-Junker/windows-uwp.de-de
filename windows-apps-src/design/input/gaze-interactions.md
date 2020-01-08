---
title: Interaktionen über Anvisieren
Description: Erfahren Sie, wie Sie Ihre UWP-apps entwerfen und optimieren, um den Benutzern, die sich auf die Überblicks Eingaben von Eye-und Head-Tracker stützen, eine optimale Leistung zu bieten
label: Gaze interactions
template: detail.hbs
keywords: Anvisieren, Eye Tracking, Head Tracking, Anvisierungspunkt, Eingabe, Benutzerinteraktion, Bedienungshilfen, Benutzerfreundlichkeit
ms.date: 05/01/2018
ms.topic: article
pm-contact: Jake Cohen
dev-contact: Austin Hodges
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6176bdce1a725c1024af9f4ecf0c37cabb0f5376
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684235"
---
# <a name="gaze-interactions-and-eye-tracking-in-uwp-apps"></a>Interaktionen via Blick und Eye Tracking in UWP-Apps

![Eye Tracking Hero](images/gaze/eyecontrolbanner1.png)

Bieten Sie Unterstützung für die Verfolgung des Blicks, der Aufmerksamkeit und der Anwesenheit eines Benutzers basierend auf der Position und den Bewegungen seiner Augen.

> [!NOTE]
> Informationen zur Eingabe via Anvisieren in [Windows Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/) finden Sie unter [Anvisieren](https://docs.microsoft.com/windows/mixed-reality/gaze).

**Wichtige APIs**: [Windows.Devices.Input.Preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview), [GazeDevicePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicepreview), [GazePointPreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview), [GazeInputSourcePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview)

## <a name="overview"></a>Übersicht

Die Eingabe über Anvisieren ist eine tolle Möglichkeit, mit Windows- und UWP-Anwendungen zu interagieren. Sie eignet sich insbesondere als Hilfstechnologe für Benutzer mit neuromuskulären Erkrankungen (z. B. ALS) und anderen Einschränkungen, die mit einer beeinträchtigten Muskel- oder Nervenfunktion einhergehen.

Darüber hinaus bietet die Eingabe über Anvisieren gleichermaßen attraktive Möglichkeiten für Spiele (z. B. Zielbestimmung und Nachverfolgung) und herkömmliche Produktivitätsanwendungen, Kiosks und andere interaktiven Szenarien, bei denen herkömmliche Eingabegeräte (z. B. Tastatur, Maus, Touch) nicht verfügbar sind oder es hilfreich wäre, dass der Benutzer die Hände frei hat für andere Aufgaben (z. B. zum Halten von Einkaufstüten).

> [!NOTE]
> Die Unterstützung für Eye Tracking-Hardware wurde mit dem **Windows 10 Fall Creators Update** zusammen mit der [Augensteuerung](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control) eingeführt, einem integrierten Feature, mit dem Sie Ihre Augen verwenden können, um den Bildschirmzeiger zu steuern, Text über die Bildschirmtastatur einzugeben und mit Personen über Text-zu-Sprache zu kommunizieren. Ein Satz von UWP-APIs ([Windows. Devices. Input. Preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview)) zum Erstellen von Anwendungen, die mit der Eye Tracking-Hardware interagieren können, ist mit dem **Windows 10-Update vom April 2018 (Version 1803, Build 17134)** und neueren Versionen verfügbar.

## <a name="privacy"></a>„Datenschutz”

Aufgrund der potenziell vertraulichen persönlichen Daten, die von Eye Tracking-Geräten gesammelt werden, müssen Sie die `gazeInput`-Funktion im App-Manifest Ihrer UWP-Anwendung deklarieren (siehe den folgenden Abschnitt **Setup**). Sofern deklariert, fordert Windows die Benutzer (bei erstmaliger Ausführung der App) automatisch über ein Dialogfeld auf, ihre Zustimmung zu geben, dass die App mit dem Eye Tracking-Gerät kommuniziert und auf diese Daten zugreift.

Wenn Ihre App darüber hinaus Eye Tracking-Daten erfasst, speichert oder überträgt, müssen Sie dies in den Datenschutzbestimmungen für die App darlegen und alle sonstigen Anforderungen an **personenbezogene Informationen** in der [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) und den [Microsoft Store-Richtlinien ](https://docs.microsoft.com/legal/windows/agreements/store-policies) erfüllen.

## <a name="setup"></a>Setup

Um die APIs für die Eingabe über Anvisieren in Ihrer UWP-App zu verwenden, müssen Sie Folgendes tun: 

- Geben Sie die `gazeInput`-Funktion im Appmanifest an.

    Öffnen Sie die Datei **Package.appxmanifest** mit dem Manifest-Designer von Visual Studio, oder fügen Sie die Funktion manuell hinzu, indem Sie **Code anzeigen** auswählen und die folgende `DeviceCapability` in den Knoten `Capabilities` einfügen:

    ```xaml
    <Capabilities>
       <DeviceCapability Name="gazeInput" />
    </Capabilities>
    ```

- Ein Windows-kompatibles Eye Tracking-Gerät (entweder integriertes Gerät oder Peripheriegerät) muss an Ihr System angeschlossen und eingeschaltet sein.

    Unter [Erste Schritte mit der Augensteuerung in Windows 10](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control#supported-devices ) finden Sie eine Liste der unterstützten Eye Tracking-Geräte.

## <a name="basic-eye-tracking"></a>Grundlegendes Eye Tracking

In diesem Beispiel wird veranschaulicht, wie Sie den Blick der Benutzer in einer UWP-Anwendung verfolgen und eine Timing-Funktion mit grundlegenden Treffertests verwenden, um anzugeben, wie gut sie ihren Blick auf ein bestimmtes Element fokussieren können.

Eine kleine Ellipse wird verwendet, um anzuzeigen, wo der Anvisierungspunkt sich im Viewport der Anwendung befindet, während eine [RadialProgressBar](https://docs.microsoft.com/windows/communitytoolkit/controls/radialprogressbar) aus dem [Windows-Community Toolkit](https://docs.microsoft.com/windows/communitytoolkit/) nach dem Zufallsprinzip im Zeichenbereich platziert wird. Wenn der Anvisierungspunkt auf der Fortschrittsleiste erkannt wird, wird ein Timer gestartet und die Fortschrittsleiste wird im Zeichenbereich nach dem Zufallsprinzip verschoben, wenn sie 100 % erreicht.

![Blickverfolgung mit Timer-Beispiel](images/gaze/gaze-input-timed2.gif)

*Beispiel Überwachung mit Timer*

**Herunterladen dieses Beispiels aus dem [Beispiel Eingabe Beispiel (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)**

1. Zunächst richten wir die Benutzeroberfläche ein (MainPage.xaml).

    ```xaml
    <Page
        x:Class="gazeinput.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:gazeinput"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"    
        mc:Ignorable="d">
    
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Grid x:Name="containerGrid">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <StackPanel x:Name="HeaderPanel" 
                        Orientation="Horizontal" 
                        Grid.Row="0">
                    <StackPanel.Transitions>
                        <TransitionCollection>
                            <AddDeleteThemeTransition/>
                        </TransitionCollection>
                    </StackPanel.Transitions>
                    <TextBlock x:Name="Header" 
                           Text="Gaze tracking sample" 
                           Style="{ThemeResource HeaderTextBlockStyle}" 
                           Margin="10,0,0,0" />
                    <TextBlock x:Name="TrackerCounterLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="Number of trackers: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerCounter"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="0" 
                           Margin="10,0,0,0"/>
                    <TextBlock x:Name="TrackerStateLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="State: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerState"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="n/a" 
                           Margin="10,0,0,0"/>
                </StackPanel>
                <Canvas x:Name="gazePositionCanvas" Grid.Row="1">
                    <controls:RadialProgressBar
                        x:Name="GazeRadialProgressBar" 
                        Value="0"
                        Foreground="Blue" 
                        Background="White"
                        Thickness="4"
                        Minimum="0"
                        Maximum="100"
                        Width="100"
                        Height="100"
                        Outline="Gray"
                        Visibility="Collapsed"/>
                    <Ellipse 
                        x:Name="eyeGazePositionEllipse"
                        Width="20" Height="20"
                        Fill="Blue" 
                        Opacity="0.5" 
                        Visibility="Collapsed">
                    </Ellipse>
                </Canvas>
            </Grid>
        </Grid>
    </Page>
    ```

2. Als Nächstes initialisieren wir unsere App.

    In diesem Codeausschnitt deklarieren wir unsere globalen Objekte und überschreiben das Seitenereignis [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto), um unseren [Gaze Device Watcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview) und das Seitenereignis [OnNavigatedFrom](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) zu starten, um unseren [Gaze Device Watcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview) zu beenden.

    ```csharp
    using System;
    using Windows.Devices.Input.Preview;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml;
    using Windows.Foundation;
    using System.Collections.Generic;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;
    
    namespace gazeinput
    {
        public sealed partial class MainPage : Page
        {
            /// <summary>
            /// Reference to the user's eyes and head as detected
            /// by the eye-tracking device.
            /// </summary>
            private GazeInputSourcePreview gazeInputSource;
    
            /// <summary>
            /// Dynamic store of eye-tracking devices.
            /// </summary>
            /// <remarks>
            /// Receives event notifications when a device is added, removed, 
            /// or updated after the initial enumeration.
            /// </remarks>
            private GazeDeviceWatcherPreview gazeDeviceWatcher;
    
            /// <summary>
            /// Eye-tracking device counter.
            /// </summary>
            private int deviceCounter = 0;
    
            /// <summary>
            /// Timer for gaze focus on RadialProgressBar.
            /// </summary>
            DispatcherTimer timerGaze = new DispatcherTimer();
    
            /// <summary>
            /// Tracker used to prevent gaze timer restarts.
            /// </summary>
            bool timerStarted = false;
    
            /// <summary>
            /// Initialize the app.
            /// </summary>
            public MainPage()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Override of OnNavigatedTo page event starts GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedTo event</param>
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                // Start listening for device events on navigation to eye-tracking page.
                StartGazeDeviceWatcher();
            }
    
            /// <summary>
            /// Override of OnNavigatedFrom page event stops GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedFrom event</param>
            protected override void OnNavigatedFrom(NavigationEventArgs e)
            {
                // Stop listening for device events on navigation from eye-tracking page.
                StopGazeDeviceWatcher();
            }
        }
    }
    ```

3. Als Nächstes fügen wir unsere Gaze Device Watcher-Methoden hinzu. 
    
    In `StartGazeDeviceWatcher` rufen wir [CreateWatcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.createwatcher) auf und deklarieren die Ereignislistener des Eye Tracking-Geräts ([DeviceAdded](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.added), [DeviceUpdated](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.updated) und [DeviceRemoved](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.removed)).

    In `DeviceAdded` überprüfen wir den Status des Eye Tracking-Geräts. Bei einem geeigneten Gerät erhöhen wir die Gerätezahl und aktivieren die Blickverfolgung. Einzelheiten erfahren Sie im nächsten Schritt.

    In `DeviceUpdated` aktivieren wir außerdem die Blickverfolgung, da dieses Ereignis ausgelöst wird, wenn ein Gerät neu kalibriert wird.

    In `DeviceRemoved` verringern wir die Gerätezahl und entfernen die Gerätereignishandler.

    In `StopGazeDeviceWatcher` beenden wir den Gaze Device Watcher. 

```csharp
    /// <summary>
    /// Start gaze watcher and declare watcher event handlers.
    /// </summary>
    private void StartGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher == null)
        {
            gazeDeviceWatcher = GazeInputSourcePreview.CreateWatcher();
            gazeDeviceWatcher.Added += this.DeviceAdded;
            gazeDeviceWatcher.Updated += this.DeviceUpdated;
            gazeDeviceWatcher.Removed += this.DeviceRemoved;
            gazeDeviceWatcher.Start();
        }
    }

    /// <summary>
    /// Shut down gaze watcher and stop listening for events.
    /// </summary>
    private void StopGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher != null)
        {
            gazeDeviceWatcher.Stop();
            gazeDeviceWatcher.Added -= this.DeviceAdded;
            gazeDeviceWatcher.Updated -= this.DeviceUpdated;
            gazeDeviceWatcher.Removed -= this.DeviceRemoved;
            gazeDeviceWatcher = null;
        }
    }

    /// <summary>
    /// Eye-tracking device connected (added, or available when watcher is initialized).
    /// </summary>
    /// <param name="sender">Source of the device added event</param>
    /// <param name="e">Event args for the device added event</param>
    private void DeviceAdded(GazeDeviceWatcherPreview source, 
        GazeDeviceWatcherAddedPreviewEventArgs args)
    {
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter++;
            TrackerCounter.Text = deviceCounter.ToString();
        }
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Initial device state might be uncalibrated, 
    /// but device was subsequently calibrated.
    /// </summary>
    /// <param name="sender">Source of the device updated event</param>
    /// <param name="e">Event args for the device updated event</param>
    private void DeviceUpdated(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherUpdatedPreviewEventArgs args)
    {
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Handles disconnection of eye-tracking devices.
    /// </summary>
    /// <param name="sender">Source of the device removed event</param>
    /// <param name="e">Event args for the device removed event</param>
    private void DeviceRemoved(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherRemovedPreviewEventArgs args)
    {
        // Decrement gaze device counter and remove event handlers.
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter--;
            TrackerCounter.Text = deviceCounter.ToString();

            if (deviceCounter == 0)
            {
                gazeInputSource.GazeEntered -= this.GazeEntered;
                gazeInputSource.GazeMoved -= this.GazeMoved;
                gazeInputSource.GazeExited -= this.GazeExited;
            }
        }
    }
```

4. Hier wird überprüft, ob das Gerät in `IsSupportedDevice` funktionsbereit ist. Ist dies der Fall, versuchen Sie, die Blickverfolgung in `TryEnableGazeTrackingAsync` zu aktivieren.

    In `TryEnableGazeTrackingAsync` deklarieren wir die Blickereignishandler und rufen [GazeInputSourcePreview.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) auf, um einen Verweis zur Eingabequelle zu erhalten (Aufruf muss über den UI-Thread erfolgen, siehe [Aufrechterhalten der Reaktionsfähigkeit des UI-Threads](https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive)).

    > [!NOTE]
    > Sie sollten [GazeInputSourcePreview.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) nur aufrufen, wenn ein kompatibles Eye Tracking-Gerät angeschlossen ist und für Ihre Anwendung erforderlich ist. Andernfalls ist das Zustimmungsdialogfeld nicht erforderlich.

```csharp
    /// <summary>
    /// Initialize gaze tracking.
    /// </summary>
    /// <param name="gazeDevice"></param>
    private async void TryEnableGazeTrackingAsync(GazeDevicePreview gazeDevice)
    {
        // If eye-tracking device is ready, declare event handlers and start tracking.
        if (IsSupportedDevice(gazeDevice))
        {
            timerGaze.Interval = new TimeSpan(0, 0, 0, 0, 20);
            timerGaze.Tick += TimerGaze_Tick;

            SetGazeTargetLocation();

            // This must be called from the UI thread.
            gazeInputSource = GazeInputSourcePreview.GetForCurrentView();

            gazeInputSource.GazeEntered += GazeEntered;
            gazeInputSource.GazeMoved += GazeMoved;
            gazeInputSource.GazeExited += GazeExited;
        }
        // Notify if device calibration required.
        else if (gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.UserCalibrationNeeded ||
                    gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.ScreenSetupNeeded)
        {
            // Device isn't calibrated, so invoke the calibration handler.
            System.Diagnostics.Debug.WriteLine(
                "Your device needs to calibrate. Please wait for it to finish.");
            await gazeDevice.RequestCalibrationAsync();
        }
        // Notify if device calibration underway.
        else if (gazeDevice.ConfigurationState == 
            GazeDeviceConfigurationStatePreview.Configuring)
        {
            // Device is currently undergoing calibration.  
            // A device update is sent when calibration complete.
            System.Diagnostics.Debug.WriteLine(
                "Your device is being configured. Please wait for it to finish"); 
        }
        // Device is not viable.
        else if (gazeDevice.ConfigurationState == GazeDeviceConfigurationStatePreview.Unknown)
        {
            // Notify if device is in unknown state.  
            // Reconfigure/recalbirate the device.  
            System.Diagnostics.Debug.WriteLine(
                "Your device is not ready. Please set up your device or reconfigure it."); 
        }
    }

    /// <summary>
    /// Check if eye-tracking device is viable.
    /// </summary>
    /// <param name="gazeDevice">Reference to eye-tracking device.</param>
    /// <returns>True, if device is viable; otherwise, false.</returns>
    private bool IsSupportedDevice(GazeDevicePreview gazeDevice)
    {
        TrackerState.Text = gazeDevice.ConfigurationState.ToString();
        return (gazeDevice.CanTrackEyes &&
                    gazeDevice.ConfigurationState == 
                    GazeDeviceConfigurationStatePreview.Ready);
    }
```

5. Als Nächstes richten wir unsere Blickereignishandler ein.

    Wir blenden die Blickverfolgungsellipse in `GazeEntered` und `GazeExited` ein bzw. aus.

    In `GazeMoved` verschieben wir unsere Blickverfolgungsellipse basierend auf der [EyeGazePosition](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview.eyegazeposition), die vom [CurrentPoint](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs.currentpoint) der [GazeEnteredPreviewEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs) bereitgestellt wird. Wir verwalten auch den Blickfokus-Timer in der [RadialProgressBar](https://docs.microsoft.com/windows/communitytoolkit/controls/radialprogressbar), der die Neupositionierung der Fortschrittsleiste auslöst. Einzelheiten erfahren Sie im nächsten Schritt.

    ```csharp
    /// <summary>
    /// GazeEntered handler.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void GazeEntered(
        GazeInputSourcePreview sender, 
        GazeEnteredPreviewEventArgs args)
    {
        // Show ellipse representing gaze point.
        eyeGazePositionEllipse.Visibility = Visibility.Visible;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeExited handler.
    /// Call DisplayRequest.RequestRelease to conclude the 
    /// RequestActive called in GazeEntered.
    /// </summary>
    /// <param name="sender">Source of the gaze exited event</param>
    /// <param name="e">Event args for the gaze exited event</param>
    private void GazeExited(
        GazeInputSourcePreview sender, 
        GazeExitedPreviewEventArgs args)
    {
        // Hide gaze tracking ellipse.
        eyeGazePositionEllipse.Visibility = Visibility.Collapsed;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeMoved handler translates the ellipse on the canvas to reflect gaze point.
    /// </summary>
    /// <param name="sender">Source of the gaze moved event</param>
    /// <param name="e">Event args for the gaze moved event</param>
    private void GazeMoved(GazeInputSourcePreview sender, GazeMovedPreviewEventArgs args)
    {
        // Update the position of the ellipse corresponding to gaze point.
        if (args.CurrentPoint.EyeGazePosition != null)
        {
            double gazePointX = args.CurrentPoint.EyeGazePosition.Value.X;
            double gazePointY = args.CurrentPoint.EyeGazePosition.Value.Y;

            double ellipseLeft = 
                gazePointX - 
                (eyeGazePositionEllipse.Width / 2.0f);
            double ellipseTop = 
                gazePointY - 
                (eyeGazePositionEllipse.Height / 2.0f) - 
                (int)Header.ActualHeight;

            // Translate transform for moving gaze ellipse.
            TranslateTransform translateEllipse = new TranslateTransform
            {
                X = ellipseLeft,
                Y = ellipseTop
            };

            eyeGazePositionEllipse.RenderTransform = translateEllipse;

            // The gaze point screen location.
            Point gazePoint = new Point(gazePointX, gazePointY);

            // Basic hit test to determine if gaze point is on progress bar.
            bool hitRadialProgressBar = 
                DoesElementContainPoint(
                    gazePoint, 
                    GazeRadialProgressBar.Name, 
                    GazeRadialProgressBar); 

            // Use progress bar thickness for visual feedback.
            if (hitRadialProgressBar)
            {
                GazeRadialProgressBar.Thickness = 10;
            }
            else
            {
                GazeRadialProgressBar.Thickness = 4;
            }

            // Mark the event handled.
            args.Handled = true;
        }
    }
    ```
6. Hier folgen schließlich die Methoden zum Verwalten des Blickfokus-Timers für diese App.

    `DoesElementContainPoint` überprüft, ob sich der Mauszeiger über der Statusleiste befindet. Wenn ja, wird der Timer gestartet, und die Statusanzeige wird bei jedem Zeitgebertakt erhöht.

    `SetGazeTargetLocation` legt die Anfangsposition der Statusanzeige fest und verschiebt die Statusanzeige, wenn die Statusanzeige abgeschlossen ist (je nach Fokus Zeit Geber), an einen zufälligen Speicherort.

    ```csharp
    /// <summary>
    /// Return whether the gaze point is over the progress bar.
    /// </summary>
    /// <param name="gazePoint">The gaze point screen location</param>
    /// <param name="elementName">The progress bar name</param>
    /// <param name="uiElement">The progress bar UI element</param>
    /// <returns></returns>
    private bool DoesElementContainPoint(
        Point gazePoint, string elementName, UIElement uiElement)
    {
        // Use entire visual tree of progress bar.
        IEnumerable<UIElement> elementStack = 
            VisualTreeHelper.FindElementsInHostCoordinates(gazePoint, uiElement, true);
        foreach (UIElement item in elementStack)
        {
            //Cast to FrameworkElement and get element name.
            if (item is FrameworkElement feItem)
            {
                if (feItem.Name.Equals(elementName))
                {
                    if (!timerStarted)
                    {
                        // Start gaze timer if gaze over element.
                        timerGaze.Start();
                        timerStarted = true;
                    }
                    return true;
                }
            }
        }

        // Stop gaze timer and reset progress bar if gaze leaves element.
        timerGaze.Stop();
        GazeRadialProgressBar.Value = 0;
        timerStarted = false;
        return false;
    }

    /// <summary>
    /// Tick handler for gaze focus timer.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void TimerGaze_Tick(object sender, object e)
    {
        // Increment progress bar.
        GazeRadialProgressBar.Value += 1;

        // If progress bar reaches maximum value, reset and relocate.
        if (GazeRadialProgressBar.Value == 100)
        {
            SetGazeTargetLocation();
        }
    }

    /// <summary>
    /// Set/reset the screen location of the progress bar.
    /// </summary>
    private void SetGazeTargetLocation()
    {
        // Ensure the gaze timer restarts on new progress bar location.
        timerGaze.Stop();
        timerStarted = false;

        // Get the bounding rectangle of the app window.
        Rect appBounds = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

        // Translate transform for moving progress bar.
        TranslateTransform translateTarget = new TranslateTransform();

        // Calculate random location within gaze canvas.
            Random random = new Random();
            int randomX = 
                random.Next(
                    0, 
                    (int)appBounds.Width - (int)GazeRadialProgressBar.Width);
            int randomY = 
                random.Next(
                    0, 
                    (int)appBounds.Height - (int)GazeRadialProgressBar.Height - (int)Header.ActualHeight);

        translateTarget.X = randomX;
        translateTarget.Y = randomY;

        GazeRadialProgressBar.RenderTransform = translateTarget;

        // Show progress bar.
        GazeRadialProgressBar.Visibility = Visibility.Visible;
        GazeRadialProgressBar.Value = 0;
    }
    ```

## <a name="see-also"></a>Weitere Informationen:

### <a name="resources"></a>Ressourcen

- [Windows Community Toolkit-Blick Bibliothek](https://docs.microsoft.com/windows/communitytoolkit/gaze/gazeinteractionlibrary)

### <a name="topic-samples"></a>Themenbeispiele

- [Blick Beispiel (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)
