---
title: Abrufen des Benutzerstandorts
description: Ermitteln Sie den Standort des Benutzers, und reagieren Sie auf Änderungen des Standorts. Der Zugriff auf die Position eines Benutzers wird über die Datenschutzeinstellungen in der Einstellungs-App verwaltet. In diesem Thema wird auch gezeigt, wie Sie überprüfen, ob Ihre App über die Berechtigung zum Zugriff auf den Benutzerstandort verfügt.
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
ms.date: 11/28/2017
ms.topic: article
keywords: Windows 10, UWP, map, Location, Location-Funktion
ms.localizationpriority: medium
ms.openlocfilehash: 79c34af48cf1b2d860d2a170fd642ef05945c15d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158714"
---
# <a name="get-the-users-location"></a>Abrufen des Benutzerstandorts




Ermitteln Sie den Standort des Benutzers, und reagieren Sie auf Änderungen des Standorts. Der Zugriff auf die Position eines Benutzers wird über die Datenschutzeinstellungen in der Einstellungs-App verwaltet. In diesem Thema wird auch gezeigt, wie Sie überprüfen, ob Ihre App über die Berechtigung zum Zugriff auf den Benutzerstandort verfügt.

**Tipp** Um mehr über das Zugreifen auf den Benutzerstandort in Ihrer App zu erfahren, laden Sie das folgende Beispiel aus dem [Repository Beispiele für Universelle Windows-Plattform](https://github.com/Microsoft/Windows-universal-samples) auf GitHub herunter.

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>Aktivieren der Positionsfunktion


1.  Doppelklicken Sie im **Projektmappen-Explorer** auf **package.appxmanifest**, und wählen Sie die Registerkarte **Funktionen** aus.
2.  Aktivieren Sie in der Liste der **Funktionen** das Kontrollkästchen für **Speicherort**. Dadurch wird der Paketmanifestdatei die `location`-Gerätefunktion hinzugefügt.

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="get-the-current-location"></a>Abrufen der aktuellen Position


In diesem Abschnitt erfahren Sie, wie Sie den geografische Standort eines Benutzers über APIs im [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation)-Namespace feststellen.

### <a name="step-1-request-access-to-the-users-location"></a>Schritt 1: Anfordern des Zugriffs auf die Position des Benutzers

Wenn Ihre APP keine grobe Standort Funktion hat (siehe Hinweis), müssen Sie den Zugriff auf den Speicherort des Benutzers mithilfe der [**requestaccessasync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) -Methode anfordern, bevor Sie versuchen, auf den Speicherort zuzugreifen. Sie müssen die **RequestAccessAsync**-Methode aus dem UI-Thread aufrufen, und die App muss sich im Vordergrund ausgeführt werden. Ihre APP kann erst dann auf die Standortinformationen des Benutzers zugreifen, wenn der Benutzer die Berechtigung für Ihre APP erteilt hat.\*

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```



Die [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync)-Methode fordert den Benutzer auf, den Zugriff auf seinen Standort zu genehmigen. Der Benutzer wird nur einmal (pro App) aufgefordert. Nachdem die Berechtigung erstmalig gewährt oder verweigert wurde, fordert die Methode keine Berechtigung mehr vom Benutzer an. Um das Ändern von Standortberechtigungen nach der Aufforderung für den Benutzer zu vereinfachen, sollten Sie einen Link zu den Standorteinstellungen bereitstellen (s. wie weiter unten in diesem Thema).

>Hinweis: mit dem Feature "grobe Speicherort" kann Ihre APP einen absichtlich verfugenden (unpräzisen) Speicherort abrufen, ohne die explizite Berechtigung des Benutzers zu erhalten (der systemweite Schalter muss **jedoch weiterhin aktiviert sein)**. Informationen dazu, wie Sie eine grobe Position in Ihrer APP verwenden, finden Sie in der [**allowfallbacktoallowlesspositionen**](/uwp/api/windows.devices.geolocation.geolocator.allowfallbacktoconsentlesspositions) -Methode in der [**GeoLocator**](/uwp/api/windows.devices.geolocation.geolocator) -Klasse.

### <a name="step-2-get-the-users-location-and-register-for-changes-in-location-permissions"></a>Schritt 2: Abrufen des Benutzerstandorts und Registrieren für Änderungen von Standortberechtigungen

Die [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync)-Methode liest einmalig den aktuellen Standort. Hier wird eine **switch**-Anweisung mit **accessStatus** (aus dem vorherigen Beispiel) verwendet, die nur aktiv ist, wenn der Zugriff auf den Standort des Benutzers zugelassen wird. Wenn der Zugriff auf die Position des Benutzers zugelassen wurde, erstellt der Code ein [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt, führt eine Registrierung für Änderungen von Standortberechtigungen aus und fordert den Standort des Benutzers an.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);

        // If DesiredAccuracy or DesiredAccuracyInMeters are not set (or value is 0), DesiredAccuracy.Default is used.
        Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = _desireAccuracyInMetersValue };

        // Subscribe to the StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        // Carry out the operation.
        Geoposition pos = await geolocator.GetGeopositionAsync();

        UpdateLocationData(pos);
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        UpdateLocationData(null);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        UpdateLocationData(null);
        break;
}
```

### <a name="step-3-handle-changes-in-location-permissions"></a>Schritt 3: Behandeln von Änderungen von Standortberechtigungen

Das [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt löst das [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged)-Ereignis aus, um anzugeben, dass sich die Standorteinstellungen des Benutzers geändert haben. Das Ereignis übergibt den entsprechenden Status über die Eigenschaft **Status** des Arguments (des Typs [**PositionStatus**](/uwp/api/Windows.Devices.Geolocation.PositionStatus). Beachten Sie, dass diese Methode nicht vom UI-Thread aufgerufen wird und das [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)-Objekt die UI-Änderungen aufruft.

```csharp
using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix.
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings.
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear any cached location data.
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```

## <a name="respond-to-location-updates"></a>Reagieren auf Standortupdates


In diesem Abschnitt wird beschrieben, wie Sie mit dem [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged)-Ereignis Standortupdates des Benutzers über einen bestimmten Zeitraum empfangen. Da der Benutzer den Zugriff auf den Standort jederzeit widerrufen kann, ist es wichtig, [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) aufzurufen und das [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged)-Ereignis zu verwenden wie im vorherigen Abschnitt dargestellt.

In diesem Abschnitt wird vorausgesetzt, dass Sie die Standortfunktion bereits aktiviert und [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) im UI-Thread der Vordergrund-App aufgerufen haben.

### <a name="step-1-define-the-report-interval-and-register-for-location-updates"></a>Schritt 1: Definieren des Berichtsintervalls und Registrieren für Standortupdates

In diesem Beispiel wird eine **switch**-Anweisung mit **accessStatus** (aus dem vorherigen Beispiel) verwendet, die nur aktiv ist, wenn der Zugriff auf den Standort eines Benutzers zugelassen wird. Wenn der Zugriff auf den Standort des Benutzers zugelassen wurde, erstellt der Code ein [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt, gibt den Nachverfolgungstyp an und führt eine Registrierung für Standortupdates aus.

Das [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt kann das [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged)-Ereignis basierend auf einer Standortänderung (entfernungsbasierte Nachverfolgung) oder Zeitänderung (zeitraumbasierte Nachverfolgung) auslösen.

-   Für die entfernungsbasierte Nachverfolgung legen Sie die [**MovementThreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold)-Eigenschaft fest.
-   Für die zeitraumbasierte Nachverfolgung legen Sie die [**ReportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval)-Eigenschaft fest.

Wenn keine der beiden Eigenschaften festgelegt wird, wird jede 1 Sekunde ein Standort zurückgegeben (entsprechend `ReportInterval = 1000`). Hier wird ein Berichtsintervall von 2 Sekunden (`ReportInterval = 2000`) verwendet.
```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();

switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        // Create Geolocator and define perodic-based tracking (2 second interval).
        _geolocator = new Geolocator { ReportInterval = 2000 };

        // Subscribe to the PositionChanged event to get location updates.
        _geolocator.PositionChanged += OnPositionChanged;

        // Subscribe to StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        StartTrackingButton.IsEnabled = false;
        StopTrackingButton.IsEnabled = true;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecificed error!", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        break;
}
```

### <a name="step-2-handle-location-updates"></a>Schritt 2: Behandeln von Standortupdates

Das [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt löst das [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged)-Ereignis aus, um anzugeben, dass sich der Benutzerstandort geändert hat bzw. dass Zeit vergangen ist, je nachdem, welche Eigenschaft Sie konfiguriert haben. Dieses Ereignis übergibt den entsprechende Standort über die Eigenschaft **Position** des Arguments (des Typs [**Geoposition**](/uwp/api/Windows.Devices.Geolocation.Geoposition). In diesem Beispiel wird die Methode nicht vom UI-Thread aufgerufen. Die UI-Änderungen werden durch das [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)-Objekt aufgerufen.

```csharp
using Windows.UI.Core;
...
async private void OnPositionChanged(Geolocator sender, PositionChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        UpdateLocationData(e.Position);
    });
}
```

## <a name="change-the-location-privacy-settings"></a>Ändern der Datenschutzeinstellungen für den Standort


Wenn Ihre App gemäß den Standortdatenschutzeinstellungen nicht auf den Standort des Benutzers zugreifen darf, sollten Sie einen praktischen Link zu den **Standortdatenschutzeinstellungen** in der **Einstellungs**-App bereitstellen. In diesem Beispiel wird ein Hyperlink-Steuerelement verwendet, um zum `ms-settings:privacy-location`-URI zu navigieren.

```xml
<!--Set Visibility to Visible when access to location is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

Alternativ kann Ihre App die [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)-Methode aufrufen, um die **Einstellungs**-App per Code zu starten. Weitere Informationen finden Sie unter [Starten der Einstellungs-App von Windows](../launch-resume/launch-settings-app.md).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="troubleshoot-your-app"></a>Behandeln von App-Problemen


Bevor Ihre App auf die Position des Benutzers zugreifen kann, muss **Position** auf dem Gerät aktiviert sein. Vergewissern Sie sich in der **Einstellungs**-App, dass die folgenden **Datenschutzeinstellungen für den Standort** aktiviert sind:

-   **Position dieses Geräts...** ist **aktiviert** (gilt nicht für Windows 10 Mobile)
-   Die Einstellung **Position** der Positionsdienste ist **aktiviert**.
-   Ihre App hat unter **Wählen Sie Apps aus, die Ihre Position verwenden dürfen** die Einstellung **Ein**.

## <a name="related-topics"></a>Zugehörige Themen

* [UWP-Geolocation-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Entwurfsrichtlinien für Geofencing](./guidelines-for-geofencing.md)
* [Entwurfsrichtlinien für Apps mit Positionsbestimmung](./guidelines-and-checklist-for-detecting-location.md)