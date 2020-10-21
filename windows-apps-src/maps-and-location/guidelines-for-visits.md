---
description: Hier erfahren Sie, wie Sie die leistungsstarke Funktion für die Nachverfolgung besuchter Standorte (Visits Tracking) für eine praktischere Standortnachverfolgung verwenden können.
title: Richtlinien für die Verwendung von Visits Tracking
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, map, Location, geovisit, geovisits
ms.localizationpriority: medium
ms.openlocfilehash: bdca33832b4dfadb52dca45c7a060de4f05c500c
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297729"
---
# <a name="guidelines-for-using-visits-tracking"></a>Richtlinien für die Verwendung von Visits Tracking

> [!NOTE]
> [**Mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) -und Map-Dienste erfordern einen Zuordnungs Authentifizierungsschlüssel, der als [**mapservicetoken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)bezeichnet wird. Weitere Informationen zum Abrufen und Festlegen eines Kartenauthentifizierungsschlüssels finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

Die Funktion "Besuche" optimiert den Prozess der Standortüberwachung, um Sie für die praktischen Zwecke vieler apps effizienter zu gestalten. Ein Besuch wird als signifikanter geografischer Bereich definiert, den der Benutzer eingibt und verlässt. Besuche ähneln den [geozäunen](guidelines-for-geofencing.md) insofern, als Sie zulassen, dass die app nur benachrichtigt wird, wenn der Benutzer bestimmte relevante Bereiche eingibt oder verlässt. Dadurch entfällt die Notwendigkeit einer kontinuierlichen Positions Nachverfolgung, die die Akku Lebensdauer ausgleichen kann. Im Unterschied zu geozäunen werden die Bereiche jedoch dynamisch auf Platt Form Ebene identifiziert, und Sie müssen nicht explizit von einzelnen apps definiert werden. Außerdem wird die Auswahl, welche Besuche eine APP verfolgt, von einer einzelnen granularitätseinstellung behandelt, anstatt einzelne Orte zu abonnieren.

## <a name="preliminary-setup"></a>Vorläufige Einrichtung

Bevor Sie fortfahren, stellen Sie sicher, dass Ihre APP auf den Speicherort des Geräts zugreifen kann. Sie müssen die `Location` Funktion im Manifest deklarieren und die **[GeoLocator. requestaccessasync](/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** -Methode aufrufen, um sicherzustellen, dass Benutzer den App-Standort Berechtigungen erteilt werden. Weitere Informationen zur Vorgehensweise finden Sie unter [Get the User es Location](get-location.md) . 

Denken Sie daran, den `Geolocation` Namespace zu ihrer Klasse hinzuzufügen. Dies wird benötigt, damit alle Code Ausschnitte in diesem Handbuch funktionieren.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>Überprüfen Sie den aktuellen Besuch.
Die einfachste Möglichkeit, die Funktion zum Nachverfolgen von besuchen zu verwenden, ist das Abrufen der letzten bekannten Änderungs Zustandsänderung. Eine Statusänderung ist ein Platt Form protokolliertes Ereignis, bei dem der Benutzer einen Speicherort der Bedeutung eingibt oder verlässt, da seit dem letzten Bericht bedeutende Verschiebungen vorhanden sind oder der Speicherort des Benutzers verloren geht (siehe **[visitstatechange](/uwp/api/windows.devices.geolocation.visitstatechange)** Enum). Zustandsänderungen werden durch **[geovisit](/uwp/api/windows.devices.geolocation.geovisit)** -Instanzen dargestellt. Um die **geovisit** -Instanz für die zuletzt aufgezeichnete Zustandsänderung abzurufen, verwenden Sie einfach die angegebene Methode in der **[geovisitmonitor](/uwp/api/windows.devices.geolocation.geovisitmonitor)** -Klasse.

> [!NOTE]
> Durch das Überprüfen des letzten angemeldeten Besuchs ist nicht sichergestellt, dass die Besuche aktuell vom System nachverfolgt werden. Um Besuche zu verfolgen, während Sie auftreten, müssen Sie Sie entweder im Vordergrund überwachen oder für die Hintergrund Verfolgung registrieren (siehe nachfolgende Abschnitte).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Analysieren einer geovisit-Instanz (optional)
Mit der folgenden Methode werden alle in einer **geovisit** -Instanz gespeicherten Informationen in eine leicht lesbare Zeichenfolge konvertiert. Sie kann in jedem der Szenarien in dieser Anleitung verwendet werden, um Feedback für die gemeldeten Besuche bereitzustellen.

```csharp
private string ParseGeovisit(Geovisit visit){
    string visitString = null;

    // Use a DateTimeFormatter object to process the timestamp. The following
    // format configuration will give an intuitive representation of the date/time
    Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatterLongTime;
    
    formatterLongTime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(
        "{hour.integer}:{minute.integer(2)}:{second.integer(2)}", new[] { "en-US" }, "US", 
        Windows.Globalization.CalendarIdentifiers.Gregorian, 
        Windows.Globalization.ClockIdentifiers.TwentyFourHour);
    
    // use this formatter to convert the timestamp to a string, and add to "visitString"
    visitString = formatterLongTime.Format(visit.Timestamp);

    // Next, add on the state change type value
    visitString += " " + visit.StateChange.ToString();

    // Next, add the position information (if any is provided; this will be null if 
    // the reported event was "TrackingLost")
    if (visit.Position != null) {
        visitString += " (" +
        visit.Position.Coordinate.Point.Position.Latitude.ToString() + "," +
        visit.Position.Coordinate.Point.Position.Longitude.ToString() + 
        ")";
    }

    return visitString;
}
```

## <a name="monitor-visits-in-the-foreground"></a>Überwachen von besuchen im Vordergrund

Die im vorherigen Abschnitt verwendete **geovisitmonitor** -Klasse behandelt außerdem das Szenario der Überwachung von Zustandsänderungen über einen bestimmten Zeitraum. Dies können Sie erreichen, indem Sie diese Klasse instanziieren, eine Handlermethode für das zugehörige-Ereignis registrieren und die- `Start` Methode aufrufen.

```csharp
// this GeovisitMonitor instance will belong to the class scope
GeovisitMonitor monitor;

public void RegisterForVisits() {

    // Create and initialize a new monitor instance.
    monitor = new GeovisitMonitor();
    
    // Attach a handler to receive state change notifications.
    monitor.VisitStateChanged += OnVisitStateChanged;
    
    // Calling the start method will start Visits tracking for a specified scope:
    // For higher granularity such as venue/building level changes, choose Venue.
    // For lower granularity in the range of zipcode level changes, choose City.
    monitor.Start(VisitMonitoringScope.Venue);
}
```

In diesem Beispiel verarbeitet die- `OnVisitStateChanged` Methode eingehende Besuchsberichte. Die entsprechende **geovisit** -Instanz wird durch den Ereignis Parameter übergeben.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
Wenn die APP das Überwachen von Änderungen im Zusammenhang mit dem Änderungs Status abgeschlossen hat, sollte Sie den Monitor beenden und die Registrierung der Ereignishandler aufheben. Dies sollte auch immer dann erfolgen, wenn die APP angehalten oder geschlossen wird.

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>Überwachen von besuchen im Hintergrund

Sie können auch die Überwachung von Überwachungen in einer Hintergrundaufgabe implementieren, damit die besuchte Aktivität auch dann auf dem Gerät verarbeitet werden kann, wenn Ihre APP nicht geöffnet ist. Dies ist die empfohlene Methode, da Sie vielseitiger und energieeffizienter ist. 

In diesem Handbuch wird das Modell in [Erstellen und Registrieren eines Out-of-Process-Hintergrund](../launch-resume/create-and-register-a-background-task.md)Tasks verwendet, in dem die Haupt Anwendungs Dateien in einem Projekt und die Hintergrundaufgaben Datei in einem separaten Projekt in derselben Projekt Mappe gespeichert sind. Wenn Sie noch nicht mit der Implementierung von Hintergrundaufgaben vertraut sind, wird empfohlen, dass Sie diese Anleitung in erster Linie befolgen, indem Sie die unten aufgeführten Schritte durchführen, um eine Hintergrundaufgabe für die Hintergrundverarbeitung zu erstellen

> [!NOTE]
> In den folgenden Code Ausschnitten fehlen einige wichtige Funktionen, wie z. b. Fehlerbehandlung und lokaler Speicher, aus Gründen der Einfachheit. Eine robuste Implementierung der Verarbeitung von Hintergrund besuchen finden Sie in der [Beispiel-App](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation).


Stellen Sie zunächst sicher, dass Ihre APP Berechtigungen für Hintergrundaufgaben deklariert hat. `Application/Extensions`Fügen Sie im-Element der Datei " *Package. appxmanifest* " die folgende Erweiterung hinzu (fügen Sie ein Element hinzu, `Extensions` Falls noch nicht vorhanden).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

Fügen Sie als nächstes in der Definition der Hintergrundaufgaben Klasse den folgenden Code ein. Die- `Run` Methode dieser Hintergrundaufgabe übergibt einfach die Details des Auslösers (die die Besuchs Informationen enthalten) in eine separate Methode.

```csharp
using Windows.ApplicationModel.Background;

namespace Tasks {
    
    public sealed class VisitBackgroundTask : IBackgroundTask {
        
        public void Run(IBackgroundTaskInstance taskInstance) {
            
            // get a deferral
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
            
            // this task's trigger will be a Geovisit trigger
            GeovisitTriggerDetails triggerDetails = taskInstance.TriggerDetails as GeovisitTriggerDetails;

            // Handle Visit reports
            GetVisitReports(triggerDetails);         

            finally {
                deferral.Complete();
            }
        }        
    }
}
```

Definieren Sie die `GetVisitReports` Methode irgendwo in derselben Klasse.

```csharp
private void GetVisitReports(GeovisitTriggerDetails triggerDetails) {

    // Read reports from the triggerDetails. This populates the "reports" variable 
    // with all of the Geovisit instances that have been logged since the previous
    // report reading.
    IReadOnlyList<Geovisit> reports = triggerDetails.ReadReports();

    foreach (Geovisit report in reports) {
        // Using the properties of "visit", parse out the time that the state 
        // change was recorded, the device's location when the change was recorded,
        // and the type of state change.
    }

    // Note: depending on the intent of the app, you many wish to store the
    // reports in the app's local storage so they can be retrieved the next time 
    // the app is opened in the foreground.
}
```

Als nächstes müssen Sie im Hauptprojekt Ihrer APP die Registrierung dieser Hintergrundaufgabe ausführen. Erstellen Sie eine Registrierungsmethode, die von einer Benutzeraktion aufgerufen werden kann oder jedes Mal aufgerufen wird, wenn die-Klasse aktiviert wird.

```csharp
// a reference to this registration should be declared at the class level
private IBackgroundTaskRegistration visitTask = null;

// The app must call this method at some point to allow future use of 
// the background task. 
private async void RegisterBackgroundTask(object sender, RoutedEventArgs e) {
    
    string taskName = "MyVisitTask";
    string taskEntryPoint = "Tasks.VisitBackgroundTask";

    // First check whether the task in question is already registered
    foreach (var task in BackgroundTaskRegistration.AllTasks) {
        if (task.Value.Name == taskName) {
            // if a task is found with the name of this app's background task, then
            // return and do not attempt to register this task
            return;
        }
    }
    
    // Attempt to register the background task.
    try {
        // Get permission for a background task from the user. If the user has 
        // already responded once, this does nothing and the user must manually 
        // update their preference via Settings.
        BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

        switch (backgroundAccessStatus) {
            case BackgroundAccessStatus.AlwaysAllowed:
            case BackgroundAccessStatus.AllowedSubjectToSystemPolicy:
                // BackgroundTask is allowed
                break;

            default:
                // notify user that background tasks are disabled for this app
                //...
                break;
        }

        // Create a new background task builder
        BackgroundTaskBuilder visitTaskBuilder = new BackgroundTaskBuilder();

        visitTaskBuilder.Name = exampleTaskName;
        visitTaskBuilder.TaskEntryPoint = taskEntryPoint;

        // Create a new Visit trigger
        var trigger = new GeovisitTrigger();

        // Set the desired monitoring scope.
        // For higher granularity such as venue/building level changes, choose Venue.
        // For lower granularity in the range of zipcode level changes, choose City. 
        trigger.MonitoringScope = VisitMonitoringScope.Venue; 

        // Associate the trigger with the background task builder
        visitTaskBuilder.SetTrigger(trigger);

        // Register the background task
        visitTask = visitTaskBuilder.Register();      
    }
    catch (Exception ex) {
        // notify user that the task failed to register, using ex.ToString()
    }
}
```

Dadurch wird festgelegt, dass eine Hintergrundaufgaben Klasse, `VisitBackgroundTask` die im-Namespace aufgerufen `Tasks` wird, mit dem `location` auslösertyp funktioniert. 

Ihre APP sollte nun in der Lage sein, die Hintergrundaufgabe für die Besuchs Behandlung zu registrieren. diese Aufgabe sollte aktiviert werden, wenn das Gerät eine Besuchs bezogene Zustandsänderung protokolliert. Sie müssen die Logik in der Hintergrundaufgaben Klasse ausfüllen, um zu bestimmen, was mit diesen Zustands Änderungs Informationen zu tun ist.

## <a name="related-topics"></a>Zugehörige Themen
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](../launch-resume/create-and-register-a-background-task.md)
* [Abrufen der Position eines Benutzers](get-location.md)
* [Windows. Devices. Geolokation-Namespace](/uwp/api/windows.devices.geolocation)
