---
Description: Erfahren Sie, wie Sie die leistungsstarke „Visits Tracking”-Funktion (besuchte Standorte) für eine praktischere Standortnachverfolgung verwenden können.
title: Richtlinien für die Verwendung von Visits Tracking
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.date: 05/18/2017
ms.topic: article
keywords: Windows 10, UWP, Karte, Standort, Geovisit, geovisits
ms.localizationpriority: medium
ms.openlocfilehash: db351660722cd13a4e8f14bebb651d60f33d1671
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640775"
---
# <a name="guidelines-for-using-visits-tracking"></a>Richtlinien für die Verwendung von Visits Tracking

Die Funktion Visits optimiert den Prozess der Standortnachverfolgung, um die praktische Erfahrung vieler Apps effizienter zu gestalten. Ein Besuch (Visit) wird als wichtiger geografischer Bereich definiert, den der Benutzer betritt und wieder verlässt. Besuche ähneln [Geofences](guidelines-for-geofencing.md) darin, dass die App benachrichtigt wird, wenn der Benutzer ein bestimmtes Interessengebiet betritt oder verlässt. Dies macht die kontinuierlichen Standortnachverfolgung überflüssig, die die Akkulaufzeit beeinträchtigt. Allerdings werden im Gegensatz zu Geofences die Bereiche eines Besuchs dynamisch auf der Plattform identifiziert und müssen nicht explizit durch einzelne Apps definiert werden. Außerdem wird die Auswahl der Besuche, die eine App aufzeichnet, durch eine einzelne Genauigkeitseinstellung behandelt und nicht durch das Abonnieren einzelner Orte.

## <a name="preliminary-setup"></a>Vorläufige Einrichtung

Bevor Sie fortfahren, stellen Sie sicher, dass Ihre App auf die Position des Geräts zugreifen kann. Sie müssen die `Location`-Funktion im Manifest angeben und die **[Geolocator.RequestAccessAsync](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)**-Methode aufrufen, um sicherzustellen, dass Benutzer der App Standortberechtigungen verleihen. Weitere Informationen hierzu finden Sie unter [Position des Benutzers abrufen](get-location.md). 

Denken Sie daran, Ihrer Klasse einen `Geolocation`-Namespace hinzuzufügen. Dies ist für die einwandfreie Ausführung aller Codeausschnitte in diesem Handbuch erforderlich.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>Überprüfen Sie den neusten Besuch
Die einfachste Möglichkeit, die „Visits Tracking”-Funktion zu verwenden, ist die letzte bekannte Statusänderung abzurufen, die mit dem Besuch verbunden ist. Eine Statusänderung ist ein von der Plattform protokolliertes Ereignis, wobei der Benutzer entweder einen wichtigen Standort betritt/verlässt, seit dem letzten Bericht eine beträchtliche Veränderung geschah oder wenn der Standort des Benutzers verloren geht (Informationen finden Sie in der Enumeration **[VisitStateChange](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.visitstatechange)**). Statusänderungen werden durch **[Geovisit](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisit)**-Instanzen dargestellt. Um die **Geovisit**-Instanz der letzten aufgezeichneten Statusänderung aufzurufen, verwenden Sie einfach die dafür vorgesehene Methode in der **[GeovisitMonitor](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisitmonitor)**-Klasse.

> [!NOTE]
> Das Überprüfen des letzten angemeldeten Besuchs garantiert nicht, dass alle Besuche derzeit vom System nachverfolgt werden. Um Besuche direkt nachzuverfolgen, müssen Sie diese entweder im Vordergrund überwachen oder sich für die Verfolgung von Hintergrundaufgaben anmelden (siehe unten).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Analysieren einer Geovisit-Instanz (optional)
Die folgende Methode konvertiert alle in einer **Geovisit**-Instanz gespeicherten Informationen in eine leicht lesbare Zeichenfolge. Dies kann in allen Szenarien dieses Handbuchs verwendet werden, um Feedback für die gemeldeten Besuche bereitzustellen.

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

## <a name="monitor-visits-in-the-foreground"></a>Besuche im Vordergrund überwachen

Die **GeovisitMonitor**-Klasse, die im vorherigen Abschnitt verwendet wurde, sorgt auch für das Szenario der Überwachung der Statusänderung in einem gewissen Zeitraum. Verwenden Sie hierzu die Instanziierung der Klasse, die Registrierung einer Handlermethode für das Ereignis und das Aufrufen der `Start`-Methode.

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

In diesem Beispiel übernimmt die `OnVisitStateChanged`-Methode eingehende Bericht über Besuche. Das entsprechende **Geovisit**-Instanz wird über den Ereignisparameter übergeben.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
Wenn die App mit der Überwachung der Statusänderungen, die mit dem Besuch verbunden sind, fertig ist, sollte die Überwachung beendet werden und die Registrierung des Ereignishandlers aufgehoben werden. Dies sollte auch dann durchgeführt werden, wenn die App angehalten oder geschlossen wird.

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>Besuche im Hintergrund überwachen

Sie können ebenfalls die Überwachung der Besuche als Hintergrundaufgabe implementieren, damit die mit dem Besuch verbundene Aktivitäten auf dem Gerät auch dann verarbeitet werden kann, wenn Ihre App nicht geöffnet ist. Diese Methode wird empfohlen, da sie vielseitiger und energieeffizienter ist. 

Dieses Handbuch verwendet das unter [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb des Prozesses](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) beschriebene Modell, in dem die Hauptanwendungsdateien sich in einem Projekt befindet und die Hintergrundaufgabendatei sich in einem separaten Projekt in der gleichen Lösung befindet. Wenn Sie nicht mit dem Implementieren von Hintergrundaufgaben vertraut sind, empfiehlt es sich, dass Sie dieser Anleitung vorwiegend folgen und die erforderlichen Ersetzungen durchführen, um eine Aufgabe als Hintergrundaufgabe zu erstellen, die die Besuche behandelt.

> [!NOTE]
> In den folgenden Codeausschnitten wurden einige wichtige Funktionen wie z. B. die Fehlerbehandlung und das lokale Speichern aus Gründen der Einfachheit weggelassen. Eine robuste Implementierung der Behandlung der Besuche im Hintergrund finden Sie in der [Beispiel-App](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation).


Stellen Sie zunächst sicher, dass Ihre App die Berechtigung für Hintergrundaufgaben deklariert hat. Fügen Sie dem `Application/Extensions`-Element Ihrer *"Package.appxmanifest"*-Datei die folgende Erweiterung hinzu (fügen Sie ein `Extensions`-Element hinzu, wenn es nicht bereits existiert).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

Fügen Sie als Nächstes in der Definition der Hintergrundaufgabenklasse folgenden Code hinzu. Die `Run`-Methode dieser Hintergrundaufgabe übergibt die Triggerdetails (die die Besuchsinformationen enthalten) ganz einfach an eine separate Methode.

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

Definieren Sie die `GetVisitReports`-Methode an einer beliebigen Stelle in dieser Klasse.

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

Sie müssen als Nächstes im Hauptprojekt Ihrer App die Registrierung der Hintergrundaufgabe ausführen. Erstellen Sie eine Registrierungsmethode, die durch eine Benutzeraktion aufgerufen werden kann, oder jedes Mal aufgerufen wird, wenn die Klasse aktiviert wird.

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

Dadurch wird festgelegt, dass eine Hintergrundaufgabenklasse mit dem Namen `VisitBackgroundTask` im Namespace `Tasks` eine Aktion mit dem `location`-Triggertyp durchführt. 

Ihre App sollte nun das Registrieren der Hintergrundaufgabe für Besuche behandeln können, und diese Aufgabe sollte aktiviert werden, sobald das Gerät eine Änderung im Zusammenhang mit einem Besuch protokolliert. Sie müssen die Logik in Ihrer Hintergrundaufgabenklasse ausfüllen, welche Aktion mit dieser Zustandsänderungsinformation ausgeführt werden soll.

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
* [Abrufen der Position eines Benutzers](get-location.md)
* [Windows.Devices.Geolocation-namespace](https://docs.microsoft.com/uwp/api/windows.devices.geolocation)