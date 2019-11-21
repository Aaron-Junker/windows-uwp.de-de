---
title: Einrichten eines Geofence
description: Richten Sie einen Geofence-Bereich in Ihrer App ein, und erfahren Sie, wie Sie Benachrichtigungen im Vordergrund und Hintergrund behandeln.
ms.assetid: A3A46E03-0751-4DBD-A2A1-2323DB09BDBA
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Karten, Standort, Geofence, Benachrichtigungen
ms.localizationpriority: medium
ms.openlocfilehash: 6507b34a1a7f6d8947ce9ec2f05fb883adfc3030
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260366"
---
# <a name="set-up-a-geofence"></a>Einrichten eines Geofence




Richten Sie einen [**Geofence**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)-Bereich in Ihrer App ein, und erfahren Sie, wie Sie Benachrichtigungen im Vordergrund und Hintergrund behandeln.

**Tipp** Wenn Sie weitere Informationen zum Zugreifen auf die Position in Ihrer App benötigen, laden Sie das folgende Beispiel aus den [API-Beispielen für die Universelle Windows-Plattform](https://github.com/Microsoft/Windows-universal-samples) auf GitHub herunter.

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>Aktivieren der Positionsfunktion


1.  Doppelklicken Sie im **Projektmappen-Explorer** auf **package.appxmanifest**, und wählen Sie die Registerkarte **Funktionen** aus.
2.  Überprüfen Sie die Option **Position** in der Liste **Funktionen**. Dadurch wird der Paketmanifestdatei die `Location`-Gerätefunktion hinzugefügt.

```xml
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="set-up-a-geofence"></a>Einrichten eines Geofence


### <a name="step-1-request-access-to-the-users-location"></a>Schritt 1: Anfordern des Zugriffs auf die Position des Benutzers

**Wichtig** Sie müssen über die [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync)-Methode Zugriff auf Positionsdaten des Benutzers anfordern, bevor Sie versuchen, auf die Position des Benutzers zuzugreifen. Sie müssen die **RequestAccessAsync**-Methode aus dem UI-Thread aufrufen, und die App muss sich im Vordergrund befinden. Ihre App kann erst auf Positionsdaten des Benutzers zugreifen, nachdem der Benutzer der App den Zugriff gewährt hat.

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

Die [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync)-Methode fordert den Benutzer auf, den Zugriff auf seinen Standort zu genehmigen. Der Benutzer wird nur einmal (pro App) aufgefordert. Nachdem die Berechtigung erstmalig gewährt oder verweigert wurde, fordert die Methode keine Berechtigung mehr vom Benutzer an. Um das Ändern von Standortberechtigungen nach der Aufforderung für den Benutzer zu vereinfachen, sollten Sie einen Link zu den Standorteinstellungen bereitstellen wie weiter unten in diesem Thema beschrieben.

### <a name="step-2-register-for-changes-in-geofence-state-and-location-permissions"></a>Schritt 2: Registrieren für Änderungen des Geofence-Zustands und für Positionsberechtigungen

In diesem Beispiel wird eine **switch**-Anweisung mit **accessStatus** (aus dem vorherigen Beispiel) verwendet, die nur aktiv ist, wenn der Zugriff auf den Standort eines Benutzers zugelassen wird. Der Code greift (sofern Zugriff auf die Position des Benutzers gewährt wurde) auf die aktuellen Geofences zu und führt eine Registrierung für Geofence-Zustandsänderungen und für Positionsberechtigungsänderungen durch.

**Tipp** Überwachen Sie bei Verwendung eines Geofence-Bereichs Änderungen bezüglich Positionsberechtigungen mithilfe des [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged)-Ereignisses aus der GeofenceMonitor-Klasse anstatt mit dem StatusChanged-Ereignis aus der Geolocator-Klasse. Ein [**GeofenceMonitorStatus**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus)-Element mit dem Status **Disabled** entspricht einem deaktivierten [**PositionStatus**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.PositionStatus)-Element: Beide geben an, dass die App nicht auf Positionsdaten des Benutzers zugreifen darf.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        geofences = GeofenceMonitor.Current.Geofences;

        FillRegisteredGeofenceListBoxWithExistingGeofences();
        FillEventListBoxWithExistingEvents();

        // Register for state change events.
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
        break;


    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access denied.", NotifyType.ErrorMessage);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        break;
}
```

Heben Sie die Registrierung der Ereignislistener auf, wenn der Benutzer die Vordergrund-App verlässt.

```csharp
protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
    GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;

    base.OnNavigatingFrom(e);
}
```

### <a name="step-3-create-the-geofence"></a>Schritt 3: Erstellen des Geofence-Bereichs

Nun können Sie ein [**Geofence**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)-Objekt definieren und einrichten. Je nach Ihren Anforderungen stehen mehrere verschiedene Konstruktorüberladungen zur Auswahl. Geben Sie im einfachsten Geofence-Konstruktor wie hier gezeigt nur [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.id) und [**Geoshape**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.geoshape) an.

```csharp
// Set the fence ID.
string fenceId = "fence1";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set a circular region for the geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle);

```

Sie können den Geofence mit einem der anderen Konstruktoren weiter optimieren. Im nächsten Beispiel legt der Geofence-Konstruktor diese zusätzlichen Parameter fest:

-   [**MonitoredStates**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) - Indicates what geofence events you want to receive notifications for entering the defined region, leaving the defined region, or removal of the geofence.
-   [**SingleUse**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.singleuse) - Removes the geofence once all the states the geofence is being monitored for have been met.
-   [**DwellTime**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) - Indicates how long the user must be in or out of the defined area before the enter/exit events are triggered.
-   [**StartTime**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.starttime) - Indicates when to start monitoring the geofence.
-   [**Duration**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.duration) - Indicates the period for which to monitor the geofence.

```csharp
// Set the fence ID.
string fenceId = "fence2";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set the circular region for geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Remove the geofence after the first trigger.
bool singleUse = true;

// Set the monitored states.
MonitoredGeofenceStates monitoredStates =
                MonitoredGeofenceStates.Entered |
                MonitoredGeofenceStates.Exited |
                MonitoredGeofenceStates.Removed;

// Set how long you need to be in geofence for the enter event to fire.
TimeSpan dwellTime = TimeSpan.FromMinutes(5);

// Set how long the geofence should be active.
TimeSpan duration = TimeSpan.FromDays(1);

// Set up the start time of the geofence.
DateTimeOffset startTime = DateTime.Now;

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle, monitoredStates, singleUse, dwellTime, startTime, duration);
```

Denken Sie nach der Erstellung daran, das neue [**Geofence**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)-Objekt beim Monitor zu registrieren.

```csharp
// Register the geofence
try {
   GeofenceMonitor.Current.Geofences.Add(geofence);
} catch {
   // Handle failure to add geofence
}
```

### <a name="step-4-handle-changes-in-location-permissions"></a>Schritt 4: Behandeln von Änderungen an Positionsberechtigungen

Das [**GeofenceMonitor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitor)-Objekt löst das [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged)-Ereignis aus, um anzugeben, dass sich die Positionseinstellungen des Benutzers geändert haben. Das Ereignis übergibt den entsprechenden Status über die Eigenschaft **sender.Status** des Arguments (vom Typ [**GeofenceMonitorStatus**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus)). Beachten Sie, dass diese Methode nicht vom UI-Thread aufgerufen wird und das [**Dispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)-Objekt die UI-Änderungen aufruft.

```csharp
using Windows.UI.Core;
...
public async void OnGeofenceStatusChanged(GeofenceMonitor sender, object e)
{
   await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
   {
    // Show the location setting message only if the status is disabled.
    LocationDisabledMessage.Visibility = Visibility.Collapsed;

    switch (sender.Status)
    {
     case GeofenceMonitorStatus.Ready:
      _rootPage.NotifyUser("The monitor is ready and active.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.Initializing:
      _rootPage.NotifyUser("The monitor is in the process of initializing.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NoData:
      _rootPage.NotifyUser("There is no data on the status of the monitor.", NotifyType.ErrorMessage);
      break;

     case GeofenceMonitorStatus.Disabled:
      _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

      // Show the message to the user to go to the location settings.
      LocationDisabledMessage.Visibility = Visibility.Visible;
      break;

     case GeofenceMonitorStatus.NotInitialized:
      _rootPage.NotifyUser("The geofence monitor has not been initialized.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NotAvailable:
      _rootPage.NotifyUser("The geofence monitor is not available.", NotifyType.ErrorMessage);
      break;

     default:
      ScenarioOutput_Status.Text = "Unknown";
      _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
      break;
    }
   });
}
```

## <a name="set-up-foreground-notifications"></a>Einrichten von Benachrichtigungen im Vordergrund


Nachdem Ihre Geofence-Bereiche erstellt wurden, müssen Sie die Logik für das Eintreten eines Geofence-Ereignisses hinzufügen. Je nach eingerichteten [**MonitoredStates**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) können Sie in folgenden Fällen ein Ereignis empfangen:

-   Der Benutzer betritt eine Zielregion.
-   Der Benutzer verlässt eine Zielregion.
-   Der Geofence-Bereich ist abgelaufen oder wurde entfernt. Beachten Sie, dass eine Hintergrund-App für ein Entfernungsereignis nicht aktiviert wird.

Sie können direkt in der App auf Ereignisse lauschen, wenn diese ausgeführt wird, oder Sie können eine Registrierung für eine Hintergrundaufgabe vornehmen, damit Sie eine Hintergrundbenachrichtigung erhalten, sobald ein Ereignis eintritt.

### <a name="step-1-register-for-geofence-state-change-events"></a>Schritt 1: Durchführen der Registrierung für Ereignisse zur Änderung des Geofence-Zustands

Damit die App bei der Änderung eines Geofence-Zustands eine Vordergrundbenachrichtigung erhält, müssen Sie einen Ereignishandler registrieren. Dies wird normalerweise eingerichtet, wenn Sie den Geofence-Bereich erstellen.

```csharp
private void Initialize()
{
    // Other initialization logic

    GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
}

```

### <a name="step-2-implement-the-geofence-event-handler"></a>Schritt 2: Implementieren des Geofence-Ereignishandlers

Der nächste Schritt ist die Implementierung der Ereignishandler. Die hier durchzuführende Aktion hängt davon ab, zu welchem Zweck Ihre App den Geofence-Bereich verwendet.

```csharp
public async void OnGeofenceStateChanged(GeofenceMonitor sender, object e)
{
    var reports = sender.ReadReports();

    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        foreach (GeofenceStateChangeReport report in reports)
        {
            GeofenceState state = report.NewState;

            Geofence geofence = report.Geofence;

            if (state == GeofenceState.Removed)
            {
                // Remove the geofence from the geofences collection.
                GeofenceMonitor.Current.Geofences.Remove(geofence);
            }
            else if (state == GeofenceState.Entered)
            {
                // Your app takes action based on the entered event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
            else if (state == GeofenceState.Exited)
            {
                // Your app takes action based on the exited event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
        }
    });
}



```

## <a name="set-up-background-notifications"></a>Einrichten von Benachrichtigungen im Hintergrund


Nachdem Ihre Geofence-Bereiche erstellt wurden, müssen Sie die Logik für das Eintreten eines Geofence-Ereignisses hinzufügen. Je nach eingerichteten [**MonitoredStates**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) können Sie in folgenden Fällen ein Ereignis empfangen:

-   Der Benutzer betritt eine Zielregion.
-   Der Benutzer verlässt eine Zielregion.
-   Der Geofence-Bereich ist abgelaufen oder wurde entfernt. Beachten Sie, dass eine Hintergrund-App für ein Entfernungsereignis nicht aktiviert wird.

So lauschen Sie auf ein Geofence-Ereignis im Hintergrund

-   Deklarieren der Hintergrundaufgabe im App-Manifest
-   Registrieren der Hintergrundaufgabe in Ihrer App Wenn Ihre App Internetzugriff benötigt, z. B. um auf einen Cloud-Dienst zuzugreifen, können Sie dafür ein Kennzeichen festlegen, wenn das Ereignis ausgelöst wird. Sie können mithilfe eines Kennzeichens auch sicherstellen, dass der Benutzer anwesend ist, wenn das Ereignis ausgelöst wird. So können Sie sicher sein, dass der Benutzer die Benachrichtigung erhält.
-   Fordern Sie den Benutzer bei im Vordergrund ausgeführter App auf, der App Positionsberechtigungen zu gewähren.

### <a name="step-1-register-for-geofence-state-change-events"></a>Schritt 1: Durchführen der Registrierung für Ereignisse zur Änderung des Geofence-Zustands

Fügen Sie im App-Manifest auf der Registerkarte **Deklarationen** eine Deklaration für eine Hintergrundaufgabe zum Standort hinzu. Gehen Sie dazu wie folgt vor:

-   Fügen Sie eine Deklaration vom Typ **Hintergrundaufgaben** hinzu.
-   Legen Sie den Eigenschaftenaufgabentyp **Standort** fest.
-   Legen Sie einen Einstiegspunkt für die App fest, der aufgerufen wird, wenn das Ereignis ausgelöst wird.

### <a name="step-2-register-the-background-task"></a>Schritt 2: Registrieren der Hintergrundaufgabe

Mit dem Code in diesem Schritt wird die Geofencing-Hintergrundaufgabe registriert. Beachten Sie, dass bei der Erstellung des Geofence-Bereichs eine Überprüfung auf Positionsberechtigungen durchgeführt wurde.

```csharp
async private void RegisterBackgroundTask(object sender, RoutedEventArgs e)
{
    // Get permission for a background task from the user. If the user has already answered once,
    // this does nothing and the user must manually update their preference via PC Settings.
    BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

    // Regardless of the answer, register the background task. Note that the user can use
    // the Settings app to prevent your app from running background tasks.
    // Create a new background task builder.
    BackgroundTaskBuilder geofenceTaskBuilder = new BackgroundTaskBuilder();

    geofenceTaskBuilder.Name = SampleBackgroundTaskName;
    geofenceTaskBuilder.TaskEntryPoint = SampleBackgroundTaskEntryPoint;

    // Create a new location trigger.
    var trigger = new LocationTrigger(LocationTriggerType.Geofence);

    // Associate the location trigger with the background task builder.
    geofenceTaskBuilder.SetTrigger(trigger);

    // If it is important that there is user presence and/or
    // internet connection when OnCompleted is called
    // the following could be called before calling Register().
    // SystemCondition condition = new SystemCondition(SystemConditionType.UserPresent | SystemConditionType.InternetAvailable);
    // geofenceTaskBuilder.AddCondition(condition);

    // Register the background task.
    geofenceTask = geofenceTaskBuilder.Register();

    // Associate an event handler with the new background task.
    geofenceTask.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);

    BackgroundTaskState.RegisterBackgroundTask(BackgroundTaskState.LocationTriggerBackgroundTaskName);

    switch (backgroundAccessStatus)
    {
    case BackgroundAccessStatus.Unspecified:
    case BackgroundAccessStatus.Denied:
        rootPage.NotifyUser("This app is not allowed to run in the background.", NotifyType.ErrorMessage);
        break;

    }
}


```

### <a name="step-3-handling-the-background-notification"></a>Schritt 3: Behandeln der Hintergrundbenachrichtigung

Die Aktion, die Sie zum Benachrichtigen der Benutzer durchführen, richtet sich nach dem Verwendungszweck der App. Sie können aber z. B. eine Popupbenachrichtigung anzeigen, einen Ton wiedergeben oder eine Live-Kachel aktualisieren. Der Code in diesem Schritt dient zum Behandeln der Benachrichtigung.

```csharp
async private void OnCompleted(IBackgroundTaskRegistration sender, BackgroundTaskCompletedEventArgs e)
{
    if (sender != null)
    {
        // Update the UI with progress reported by the background task.
        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            try
            {
                // If the background task threw an exception, display the exception in
                // the error text box.
                e.CheckResult();

                // Update the UI with the completion status of the background task.
                // The Run method of the background task sets the LocalSettings.
                var settings = ApplicationData.Current.LocalSettings;

                // Get the status.
                if (settings.Values.ContainsKey("Status"))
                {
                    rootPage.NotifyUser(settings.Values["Status"].ToString(), NotifyType.StatusMessage);
                }

                // Do your app work here.

            }
            catch (Exception ex)
            {
                // The background task had an error.
                rootPage.NotifyUser(ex.ToString(), NotifyType.ErrorMessage);
            }
        });
    }
}


```

## <a name="change-the-privacy-settings"></a>Ändern der Datenschutzeinstellungen


Wenn Ihre App gemäß den Standortdatenschutzeinstellungen nicht auf den Standort des Benutzers zugreifen darf, sollten Sie einen praktischen Link zu den **Standortdatenschutzeinstellungen** in der **Einstellungs**-App bereitstellen. In diesem Beispiel wird ein Hyperlink-Steuerelement verwendet, um zum `ms-settings:privacy-location`-URI zu navigieren.

```xml
<!--Set Visibility to Visible when access to the user's location is denied. -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

Alternativ kann Ihre App die [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)-Methode aufrufen, um die **Einstellungs**-App per Code zu starten. Weitere Informationen finden Sie unter [Starten der Einstellungs-App von Windows](https://docs.microsoft.com/windows/uwp/launch-resume/launch-settings-app).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="test-and-debug-your-app"></a>Testen und Debuggen der App


Das Testen und Debuggen von Geofencing-Apps kann eine Herausforderung sein, da diese Apps von der Position des Geräts abhängig sind. Im Folgenden stellen wir verschiedene Methoden zum Testen von im Vordergrund und im Hintergrund ausgeführten Geofencing-Apps vor.

**To debug a geofencing app**

1.  Bewegen Sie das Gerät physisch an eine neue Position.
2.  Sie können das Betreten eines Geofences testen, indem Sie eine Geofence-Region erstellen, die Ihre aktuelle Position enthält. Auf diese Weise befinden Sie sich bereits innerhalb des Geofences, und das Ereignis „Geofence betreten“ wird sofort ausgelöst.
3.  Verwenden Sie den Microsoft Visual Studio-Emulator, um Positionen für das Gerät zu simulieren.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-foreground"></a>Testen und Debuggen einer im Vordergrund ausgeführten Geofencing-App

**To test your geofencing app that is running the foreground**

1.  Erstellen Sie Ihre App in Visual Studio.
2.  Starten Sie Ihre App im Visual Studio-Emulator.
3.  Simulieren Sie mit den Tools verschiedene Positionen innerhalb und außerhalb Ihres Geofence-Bereichs. Damit das Ereignis ausgelöst wird, müssen Sie nach der mit der [**DwellTime**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime)-Eigenschaft festgelegten Verweilzeit ausreichend lange warten. Sie müssen in der Eingabeaufforderung Ihre Zustimmung geben, um der App Berechtigungen zum Verwenden der Position zu gewähren. Weitere Informationen zum Simulieren von Positionen finden Sie unter [Festlegen der simulierten geografischen Position des Geräts](https://msdn.microsoft.com/library/windows/apps/hh441475(v=vs.120).aspx#BKMK_Set_the_simulated_geo_location_of_the_device).
4.  Sie können auch den Emulator verwenden, um die Größe von Umgrenzungen sowie die Verweildauer zu schätzen, die für die Erkennung bei unterschiedlichen Geschwindigkeiten ungefähr notwendig ist.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-background"></a>Testen und Debuggen einer im Hintergrund ausgeführten Geofencing-App

**To test your geofencing app that is running the background**

1.  Erstellen Sie Ihre App in Visual Studio. Beachten Sie, dass Ihre App den Hintergrundaufgabentyp **Position** festlegen muss.
2.  Stellen Sie die App zunächst lokal bereit.
3.  Schließen Sie die lokal ausgeführte App.
4.  Starten Sie Ihre App im Visual Studio-Emulator. Die Geofencingsimulation im Hintergrund wird im Emulator jeweils nur für eine App unterstützt. Starten Sie nicht mehrere Geofencing-Apps im Emulator.
5.  Simulieren Sie im Emulator verschiedene Positionen innerhalb und außerhalb Ihrer Geofence-Region. Damit das Ereignis ausgelöst wird, müssen Sie nach der Verweilzeit ([**DwellTime**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime)) ausreichend lange warten. Sie müssen in der Eingabeaufforderung Ihre Zustimmung geben, um der App Berechtigungen zum Verwenden der Position zu gewähren.
6.  Verwenden Sie Visual Studio, um die Hintergrundaufgabe für die Position auszulösen. Weitere Informationen zum Auslösen von Hintergrundaufgaben in Visual Studio finden Sie unter [So wird’s gemacht: Auslösen von Hintergrundaufgaben](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.120).aspx#BKMK_Trigger_background_tasks).

## <a name="troubleshoot-your-app"></a>Behandeln von App-Problemen


Bevor Ihre App auf Positionsdaten zugreifen kann, muss **Position** auf dem Gerät aktiviert sein. Vergewissern Sie sich in der **Einstellungs**-App, dass die folgenden **Datenschutzeinstellungen für den Standort** aktiviert sind:

-   **Location for this device...** is turned **on** (not applicable in Windows 10 Mobile)
-   Die Einstellung **Position** der Positionsdienste ist **aktiviert**.
-   Ihre App hat unter **Wählen Sie Apps aus, die Ihre Position verwenden dürfen** die Einstellung **Ein**.

## <a name="related-topics"></a>Verwandte Themen

* [UWP-Geolocation-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Design guidelines for geofencing](https://docs.microsoft.com/windows/uwp/maps-and-location/guidelines-for-geofencing)
* [Entwurfsrichtlinien für Apps mit Positionsbestimmung](https://docs.microsoft.com/windows/uwp/maps-and-location/guidelines-and-checklist-for-detecting-location)
