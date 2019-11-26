---
Description: Wenden Sie bei der Verwendung von Geofencing in Ihrer App die folgenden bewährten Methoden an.
title: Richtlinien für Geofencing-Apps
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Karte, Standort, Ort, Geofencing
ms.localizationpriority: medium
ms.openlocfilehash: 6b1f328d45e626e1c7eb633165aad3671f1645e5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260383"
---
# <a name="guidelines-for-geofencing-apps"></a>Richtlinien für Geofencing-Apps




**Wichtige APIs**

-   [**Geofence-Klasse (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)
-   [**GeoLocator-Klasse (XAML)** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)

Halten Sie sich bei der Verwendung von [**Geofencing**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing) in Ihrer App an die folgenden bewährten Methoden.

## <a name="recommendations"></a>Empfehlungen


-   Falls die App beim Eintreten eines [**Geofence**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)-Ereignisses Internetzugriff benötigt, überprüfen Sie den Internetzugriff, bevor Sie den Geofence erstellen.
    -   Falls die App gerade nicht über Internetzugriff verfügt, können Sie Benutzer zum Herstellen einer Internetverbindung auffordern, bevor Sie den Geofence einrichten.
    -   Falls kein Internetzugriff möglich ist, vermeiden Sie den Energieverbrauch, der für die Überprüfungen des Geofencing-Standorts anfällt.
-   Stellen Sie die Relevanz der Geofencing-Benachrichtigungen sicher, indem Sie den Zeitstempel und den aktuellen Standort prüfen, wenn ein Geofence-Ereignis eine Änderung für einen [**Entered**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState)- oder **Exited**-Zustand anzeigt. Weitere Informationen finden Sie weiter unten im Abschnitt **Überprüfen des Zeitstempels und aktuellen Standorts**.
Weitere Informationen unter (#timestamp).
-   Ausnahmen für Fälle erstellen, wenn ein Gerät keinen Zugriff auf Standortinformationen hat, und ggf. den Benutzer benachrichtigen. Standortinformationen können aus verschiedenen Gründen nicht verfügbar sein, z. B. wenn die Berechtigungen deaktiviert sind, das Gerät keine GPS-Funkeinheit enthält, das GPS-Signal blockiert wird oder das WLAN-Signal nicht stark genug ist.
-   Es ist im Allgemeinen nicht erforderlich, die Überwachung auf Geofence-Ereignisse sowohl im Vordergrund als auch im Hintergrund durchzuführen. Wenn für Ihre App jedoch eine Überwachung auf Geofence-Ereignisse im Vordergrund und im Hintergrund erforderlich ist, empfehlen wir Folgendes:

    -   Rufen Sie die [**ReadReports**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports)-Methode auf, um zu prüfen, ob ein Ereignis eingetreten ist.
    -   Heben Sie die Registrierung des Ereignis-Listeners im Vordergrund jeweils auf, wenn die App für Benutzer nicht sichtbar ist, und registrieren Sie den Listener wieder, wenn die App wieder sichtbar ist.

    Codebeispiele und weitere Informationen finden Sie unter [Listener im Hintergrund und Vordergrund](#background-and-foreground-listeners).

-   Verwenden Sie pro App maximal 1000 Geofence-Bereiche. Das System unterstützt zwar grundsätzlich mehrere tausend Geofence-Bereiche pro App, durch die Beschränkung auf maximal 1000 Geofence-App können Sie jedoch die App-Leistung verbessern und den Speicherbedarf der App verringern.
-   Erstellen Sie keine Geofence-Bereiche mit einem Radius von weniger als 50 Metern. Falls Ihre App Geofence-Bereiche mit kleinem Radius erfordert, empfehlen Sie Benutzern, die App auf einem Gerät mit GPS-Funkeinheit zu verwenden, um bestmögliche Ergebnisse zu erzielen.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung

### <a name="checking-the-time-stamp-and-current-location"></a>Überprüfen des Zeitstempels und des aktuellen Standorts

Wenn ein Ereignis eine Änderung für einen [**Entered**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState)- oder **Exited**-Zustand anzeigt, sollten Sie sowohl den Zeitstempel des Ereignisses als auch Ihren aktuellen Standort überprüfen. Wann das Ereignis letztendlich vom Benutzer verarbeitet wird, hängt von verschiedenen Faktoren ab, z. B. das Fehlen von Ressourcen zum Starten einer Hintergrundaufgabe, das Übersehen der Benachrichtigung durch den Benutzer oder das Gerät befindet sich im Standbymodus (unter Windows). Beispielsweise kann es zu folgendem Ablauf kommen:

-   Von der App wird ein Geofence erstellt und auf enter- und exit-Ereignisse überwacht.
-   Der Benutzer bewegt sich mit dem Gerät in den Geofence-Bereich, sodass ein enter-Ereignis ausgelöst wird.
-   Von der App wird eine Benachrichtigung an den Benutzer gesendet, dass er sich jetzt innerhalb des Geofence-Bereichs befindet.
-   Der Benutzer war beschäftigt und sieht die Benachrichtigung erst 10 Minuten später.
-   Während dieser zehnminütigen Verzögerung hat sich der Benutzer wieder aus dem Geofence-Bereich herausbewegt.

Anhand des Zeitstempels können Sie erkennen, dass die Aktion in der Vergangenheit erfolgt ist. Am aktuellen Standort kann abgelesen werden, dass sich der Benutzer wieder außerhalb des Geofence-Bereichs befindet. Je nach Funktion der App kann es ratsam sein, dieses Ereignis herauszufiltern.

### <a name="background-and-foreground-listeners"></a>Listener im Hintergrund und Vordergrund

Im Allgemeinen muss eine App nicht gleichzeitig im Vordergrund und Hintergrund eine Überwachung auf [**Geofence**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)-Ereignisse durchführen. Die sauberste Vorgehensweise zur Behandlung eines Falls, in dem beide erforderlich sind, ist die Behandlung der Benachrichtigungen durch die Hintergrundaufgabe. Wenn Sie Geofencing-Listener sowohl im Vordergrund als auch im Hintergrund einrichten, kann nicht garantiert werden, welcher zuerst ausgelöst wird. Daher müssen Sie immer per Aufruf der [**ReadReports**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports)-Methode ermitteln, ob ein Ereignis eingetreten ist.

Wenn Sie Geofencing-Listener sowohl im Vordergrund als auch im Hintergrund eingerichtet haben, heben Sie die Registrierung für den Ereignislistener im Vordergrund jeweils auf, wenn die App für Benutzer nicht sichtbar ist, und registrieren Sie die App wieder, sobald sie wieder sichtbar ist. Unten ist Beispielcode angegeben, mit dem die Registrierung für das visibility-Ereignis durchgeführt wird.

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

Wenn sich die Sichtbarkeit ändert, können Sie die Ereignishandler im Vordergrund wie hier gezeigt aktivieren oder deaktivieren.

```csharp
private void OnVisibilityChanged(CoreWindow sender, VisibilityChangedEventArgs args)
{
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (args.Visible)
    {
        // register for foreground events
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
    }
    else
    {
        // unregister foreground events (let background capture events)
        GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;
    }
}
```

```javascript
function onVisibilityChanged() {
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (document.msVisibilityState === "visible") {
        // register for foreground events
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("statuschanged", onGeofenceStatusChanged);
    } else {
        // unregister foreground events (let background capture events)
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("statuschanged", onGeofenceStatusChanged);
    }
}
```

### <a name="sizing-your-geofences"></a>Festlegen der Größe von Geofence-Bereichen

Per GPS können sehr genaue Informationen zum Standort bereitgestellt werden, aber für das Geofencing können auch WLAN- oder andere Standortsensoren genutzt werden, um die aktuelle Position von Benutzern zu ermitteln. Diese anderen Methoden können sich jedoch auf die Größe der Geofence-Bereiche auswirken, die Sie erstellen können. Wenn die Genauigkeit nicht ausreicht, ist das Erstellen von kleinen Geofence-Bereichen nicht sinnvoll. Allgemein empfiehlt es sich, nur Geofence-Bereiche mit einem Mindestradius von 50 Metern zu erstellen. Bedenken Sie außerdem, dass Geofence-Hintergrundaufgaben unter Windows nur periodisch ausgeführt werden. Bei Verwendung eines kleinen Geofence-Bereichs besteht das Risiko, dass Sie ein [**Enter**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState)- oder ein **Exit**-Ereignis verpassen.

Falls Ihre App Geofence-Bereiche mit kleinem Radius erfordert, empfehlen Sie Benutzern, die App auf einem Gerät mit GPS-Funkeinheit zu verwenden, um bestmögliche Ergebnisse zu erzielen.

## <a name="related-topics"></a>Verwandte Themen


* [Einrichten von Geofence-Bereichen](https://docs.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)
* [Abrufen der aktuellen Position](https://docs.microsoft.com/windows/uwp/maps-and-location/get-location)
* [Beispiel für UWP-Speicherort (Geolokation)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 
