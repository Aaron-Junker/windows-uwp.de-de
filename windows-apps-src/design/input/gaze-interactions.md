---
title: Interaktionen über Anvisieren
Description: Erfahren Sie, wie Sie Ihre Windows-apps entwerfen und optimieren, um den Benutzern, die sich auf die Überblicks Eingaben von Eye-und Head-Tracker stützen, eine optimale Leistung zu bieten
label: Gaze interactions
template: detail.hbs
keywords: Blick, Augen Verfolgung, Kopf Nachverfolgung, Blickpunkt, Eingabe, Benutzerinteraktion, Barrierefreiheit, Nutzbarkeit
ms.date: 05/01/2018
ms.topic: article
pm-contact: Jake Cohen
dev-contact: Austin Hodges
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c91de7eb0200780b04bad1853cb49caf41a22bc0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172504"
---
# <a name="gaze-interactions-and-eye-tracking-in-windows-apps"></a>Blick Interaktionen und Eye Tracking in Windows-apps

![Helden der Augen Verfolgung](images/gaze/eyecontrolbanner1.png)

Unterstützung für die Nachverfolgung der Ansicht, Aufmerksamkeit und Präsenz eines Benutzers basierend auf der Position und Bewegung ihrer Augen.

> [!NOTE]
> Informationen zu Blick Eingaben in [Windows Mixed Reality](/windows/mixed-reality/)finden Sie unter [Blick](/windows/mixed-reality/gaze).

**Wichtige APIs**: [Windows. Devices. Input. Preview](/uwp/api/windows.devices.input.preview), [gazedevicepreview](/uwp/api/windows.devices.input.preview.gazedevicepreview), [gazepointpreview](/uwp/api/windows.devices.input.preview.gazepointpreview), [gazeinputsourcepreview](/uwp/api/windows.devices.input.preview.gazeinputsourcepreview)

## <a name="overview"></a>Überblick

Die Anzeige von Blick auf die Leistung ist eine leistungsstarke Möglichkeit zur Interaktion und Verwendung von Windows-Anwendungen, die besonders nützlich als Hilfstechnologie für Benutzer mit neuronalen Krankheiten (z. b. als) und anderen Behinderungen mit beeinträchtigtem Muskel-oder Nervenfunktionen sind.

Außerdem bietet die Überblicks Eingabe gleichermaßen überzeugende Möglichkeiten für Spiele (einschließlich Ziel Abruf und-Nachverfolgung) und herkömmlichen Produktivitätsanwendungen, Kiosken und andere interaktive Szenarios, in denen herkömmliche Eingabegeräte (Tastatur, Maus, Fingereingabe) nicht verfügbar sind, oder wenn es hilfreich bzw. hilfreich ist, die Benutzer für andere Aufgaben (z. b. die Aufbewahrung von Einkaufswagen) freizugeben

> [!NOTE]
> Die Unterstützung für die Eye Tracking-Hardware wurde in **Windows 10 Fall Creators Update** zusammen mit der [Eye Control](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control)eingeführt, einem integrierten Feature, mit dem Sie Ihre Augen zum Steuern des Bildschirm Zeigers, zum Eingeben mit der Bildschirmtastatur und zum kommunizieren mit Personen mithilfe von Text-zu-Sprache verwenden können. Eine Reihe von Windows-Runtime-APIs ([Windows. Devices. Input. Preview](/uwp/api/windows.devices.input.preview)) zum Erstellen von Anwendungen, die mit der Überwachungs Hardware interagieren können, finden Sie unter **Windows 10 April 2018 Update (Version 1803, Build 17134)** und neuer.

## <a name="privacy"></a>Datenschutz

Aufgrund der potenziell sensiblen personenbezogenen Daten, die von Augen Verfolgungs Geräten gesammelt wurden, müssen Sie die `gazeInput` Funktion im App-Manifest Ihrer Anwendung deklarieren (siehe folgenden Abschnitt zum **Setup** ). Wenn Sie deklariert wird, werden Benutzer von Windows automatisch mit einem Zustimmungs Dialogfeld (bei der ersten Durchführung der APP) aufgefordert, bei dem der Benutzer die Berechtigung für die APP erteilen muss, mit dem Auge Verfolgungs Gerät zu kommunizieren und auf diese Daten zuzugreifen.

Außerdem müssen Sie, wenn Ihre APP Augen Verfolgungs Daten sammelt, speichert oder überträgt, dies in den Datenschutzbestimmungen Ihrer APP beschreiben und alle anderen Anforderungen für **persönliche Informationen** im [App-Entwickler Vertrag](/legal/windows/agreements/app-developer-agreement) und die Microsoft Store- [Richtlinien](/legal/windows/agreements/store-policies)einhalten.

## <a name="setup"></a>Einrichtung

Wenn Sie die Blick Eingabe-APIs in Ihrer Windows-App verwenden möchten, müssen Sie folgende Schritte ausführen: 

- Geben Sie die `gazeInput` Funktion im App-Manifest an.

    Öffnen Sie die Datei " **Package. appxmanifest** " mit dem Visual Studio-Manifest-Designer, oder fügen Sie die Funktion manuell hinzu, indem Sie **Code anzeigen**auswählen und Folgendes `DeviceCapability` in den `Capabilities` Knoten einfügen:

    ```xaml
    <Capabilities>
       <DeviceCapability Name="gazeInput" />
    </Capabilities>
    ```

- Ein Windows-kompatibles Auge Verfolgungs Gerät, das mit dem System verbunden ist (entweder integriert oder Peripherie) und aktiviert ist.

    Eine Liste der unterstützten Überwachungsgeräte finden [Sie unter Get Started with Eye Control in Windows 10](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control#supported-devices ) .

## <a name="basic-eye-tracking"></a>Grundlegende Augen Verfolgung

In diesem Beispiel veranschaulichen wir, wie Sie den Benutzer Blick in einer Windows-App nachverfolgen und eine Zeit Steuerungsfunktion mit grundlegenden Treffer Tests verwenden, um anzugeben, wie gut Sie den Fokus auf ein bestimmtes Element bringen können.

Eine kleine Ellipse wird verwendet, um anzuzeigen, wo sich der Blickpunkt innerhalb des Viewports der Anwendung befindet, während eine [radialprogressbar](/windows/communitytoolkit/controls/radialprogressbar) aus dem [Windows Community Toolkit](/windows/communitytoolkit/) nach dem Zufallsprinzip in den Zeichenbereich eingefügt wird. Wenn der Fokus Fokus auf der Statusanzeige festgestellt wird, wird ein Timer gestartet, und die Statusleiste wird nach dem Zufallsprinzip in den Zeichenbereich verschoben, wenn die Statusanzeige 100% erreicht.

![Beispiel Überwachung mit Timer](images/gaze/gaze-input-timed2.gif)

*Beispiel Überwachung mit Timer*

**Herunterladen dieses Beispiels aus dem [Beispiel Eingabe Beispiel (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)**

1. Zuerst richten wir die Benutzeroberfläche ("MainPage. XAML") ein.

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

2. Als nächstes initialisieren wir unsere app.

    In diesem Code Ausschnitt deklarieren wir unsere globalen Objekte und setzen das [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) -Seiten Ereignis außer Kraft, um unseren Blick auf die [Geräte](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview) Überwachung und das [OnNavigatedFrom](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) -Seiten Ereignis zu überschreiben, um den Überwachungs [Geräte-Watcher](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview)anzuhalten.

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

3. Als Nächstes fügen wir unsere "Do Device Watcher"-Methoden hinzu. 
    
    In `StartGazeDeviceWatcher` rufen wir " [devatewatcher](/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.createwatcher) " auf und deklarieren die Ereignislistener für das Überwachungs [DeviceUpdated](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.updated)Gerät ([deviceadded](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.added), Debug-und [deviceremoved](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.removed)).

    In `DeviceAdded` wird der Status des Geräts für die Augen Verfolgung überprüft. Wenn ein funktionierendes Gerät aktiviert ist, wird die Anzahl der Geräte erhöht und die Nachverfolgung des Blicks aktiviert. Weitere Informationen finden Sie im nächsten Schritt.

    In `DeviceUpdated` aktivieren wir auch die Nachverfolgung von Augenblicken, wenn dieses Ereignis ausgelöst wird, wenn ein Gerät neu gestartet wird.

    In `DeviceRemoved` dekrelieren wir den Geräte Zählers und entfernen die Geräte Ereignishandler.

    In `StopGazeDeviceWatcher` wird der Blick auf Device Watcher heruntergefahren. 

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

4. Hier überprüfen wir, ob das Gerät in `IsSupportedDevice` geeignet ist, und wenn dies der Fall ist, versuchen Sie, die Blick Verfolgung in zu aktivieren `TryEnableGazeTrackingAsync` .

    In `TryEnableGazeTrackingAsync` deklarieren wir die Ereignishandler für den Blick und rufen " [gazeinputsourcepreview. getforcurrentview ()](/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) " auf, um einen Verweis auf die Eingabe Quelle zu erhalten (Dies muss im UI-Thread aufgerufen werden. Weitere Informationen finden Sie unter [Keep the UI Thread Reaktions](../../debug-test-perf/keep-the-ui-thread-responsive.md)fähig).

    > [!NOTE]
    > Sie sollten " [gazeinputsourcepreview. getforcurrentview ()](/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) " nur aufrufen, wenn ein kompatibles Überwachungsgerät verbunden ist und von Ihrer Anwendung benötigt wird. Andernfalls ist das Zustimmungs Dialogfeld unnötig.

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

5. Als nächstes richten wir unsere Ereignishandler für den Blick ein.

    Wir zeigen die Ellipse der Gaze-Nachverfolgung `GazeEntered` in `GazeExited` bzw. aus.

    In `GazeMoved` verschieben wir unsere Ellipse für die Sichtüberwachung basierend auf der vom [CurrentPoint-Wert von "CurrentPoint](/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs.currentpoint) " des " [gazeenteredprevieweventargs](/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs)" bereitgestellten " [pipeposition](/uwp/api/windows.devices.input.preview.gazepointpreview.eyegazeposition) " Außerdem wird der Timer-Fokus Zeit Geber auf der [radialprogressbar](/windows/communitytoolkit/controls/radialprogressbar)verwaltet, die die Neupositionierung der Statusanzeige auslöst. Weitere Informationen finden Sie im nächsten Schritt.

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
6. Im folgenden finden Sie die Methoden, die zum Verwalten des Fokus Zeit Gebers für den Blick auf diese APP verwendet werden.

    `DoesElementContainPoint` überprüft, ob sich der Mauszeiger über der Statusleiste befindet. Wenn dies der Fall ist, wird der Timer des zeitakts gestartet, und die Statusanzeige wird bei jedem Tick-Timer erhöht.

    `SetGazeTargetLocation` legt die Anfangsposition der Statusanzeige fest, und wenn die Statusleiste abgeschlossen ist (abhängig vom Timer für den Blickpunkt), wird die Statusanzeige an einen zufälligen Speicherort verschoben.

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

## <a name="see-also"></a>Weitere Informationen

### <a name="resources"></a>Ressourcen

- [Windows Community Toolkit-Blick Bibliothek](/windows/communitytoolkit/gaze/gazeinteractionlibrary)

### <a name="topic-samples"></a>Themenbeispiele

- [Blick Beispiel (Standard) (c#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)