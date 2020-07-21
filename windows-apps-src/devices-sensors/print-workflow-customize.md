---
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: Anpassen des Druck-Workflows
description: Erstellen Sie benutzerdefinierte Druck Workflow Umgebungen, um die Anforderungen Ihrer Organisation zu erfüllen.
ms.date: 07/03/2020
ms.topic: article
keywords: Windows 10, UWP, Drucken
ms.localizationpriority: medium
ms.openlocfilehash: 2bcfc5a24ff9202840b59166de625ac619c05670
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493395"
---
# <a name="customize-the-print-workflow"></a>Anpassen des Druck-Workflows

## <a name="overview"></a>Übersicht

Entwickler können die Druck Workflow-Darstellung durch die Verwendung einer Druckworkflow-App anpassen. Beim Drucken von Workflow-apps handelt es sich um UWP-apps, die auf die Funktionen von [Microsoft Store Geräte-Apps (wsdas)](https://docs.microsoft.com/windows-hardware/drivers/devapps/)erweitert werden. Daher ist es hilfreich, sich vor dem Fortfahren mit wsdas vertraut zu machen.

Wie im Fall von wsdas überprüft das System, wenn der Benutzer einer Quell Anwendung einen Text ausgeben und durch das Dialogfeld "Drucken" navigiert, ob eine Workflow-app mit diesem Drucker verknüpft ist. Wenn dies der Fall ist, wird die Druck Workflow-app gestartet (hauptsächlich als Hintergrundaufgabe; Weitere Informationen hierzu finden Sie weiter unten). Eine Workflow-app kann sowohl das Druck Ticket (das XML-Dokument, das die Geräteeinstellungen des Druckers für die aktuelle Druck Aufgabe konfiguriert) als auch den eigentlichen XPS-Inhalt ändern, der gedruckt werden soll. Diese Funktion kann dem Benutzer optional zugänglich gemacht werden, indem eine Benutzeroberfläche in der Mitte des Prozesses gestartet wird. Nachdem die Arbeit ausgeführt wurde, übergibt Sie den Druck Inhalt und das Druck Ticket an den Treiber.

Da Sie Hintergrund-und Vordergrund Komponenten umfasst und funktionell mit anderen apps verbunden ist, kann eine Druck Workflow-app komplizierter zu implementieren sein als andere Kategorien von UWP-apps. Es wird empfohlen, das Workflow- [App-Beispiel](https://github.com/Microsoft/print-oem-samples) zu überprüfen, während Sie dieses Handbuch lesen, um besser zu verstehen, wie die verschiedenen Funktionen implementiert werden können. Einige Features, wie z. b. verschiedene Fehlerüberprüfungen und die Benutzeroberflächen Verwaltung, sind aus Gründen der Einfachheit nicht in diesem Handbuch enthalten.

## <a name="getting-started"></a>Erste Schritte

Die Workflow-app muss den Einstiegspunkt für das Drucksystem angeben, damit Sie zum richtigen Zeitpunkt gestartet werden kann. Dies erfolgt durch Einfügen der folgenden Deklaration in das `Application/Extensions` -Element der Datei " *Package. appxmanifest* " des UWP-Projekts.

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT]
> Es gibt viele Szenarien, in denen die Druckanpassung keine Benutzereingaben erfordert. Aus diesem Grund werden Workflow-apps drucken als Hintergrundaufgaben standardmäßig ausgeführt.

Wenn eine Workflow-app mit der Quell Anwendung verknüpft ist, die den Druckauftrag gestartet hat (Weitere Informationen finden Sie weiter unten in diesem Abschnitt), überprüft das Drucksystem seine Manifest-Dateien auf einen Einstiegspunkt für den Hintergrund Task.

## <a name="do-background-work-on-the-print-ticket"></a>Hintergrundarbeit mit dem Druck Ticket

Der erste Schritt, den das Drucksystem mit der Workflow-app durchführt, ist die Aktivierung der Hintergrundaufgabe (in diesem Fall die- `WfBackgroundTask` Klasse im- `WFBackgroundTasks` Namespace). In der-Methode der Hintergrundaufgabe `Run` sollten Sie die triggerdetails der Aufgabe als **[printworkflowtriggerdetails](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)** -Instanz umwandeln. Dadurch wird die besondere Funktionalität für einen Hintergrund Task für den Druck Workflow bereitgestellt. Sie macht die **[printworkflowsession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)** -Eigenschaft verfügbar, bei der es sich um eine Instanz von **[printworkflowbackgroundsession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)** handelt. Workflow-Sitzungs Klassen drucken: sowohl die Hintergrund-als auch die Vorder Grundsorte: Steuern die sequenziellen Schritte der Druck Workflow-app.

Registrieren Sie dann Handlermethoden für die beiden Ereignisse, die von dieser Sitzungs Klasse erhoben werden. Diese Methoden werden später definiert.

```csharp
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take out a deferral here and complete once all the callbacks are done
    runDeferral = taskInstance.GetDeferral();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // cast the task's trigger details as PrintWorkflowTriggerDetails
    PrintWorkflowTriggerDetails workflowTriggerDetails = taskInstance.TriggerDetails as PrintWorkflowTriggerDetails;

    // Get the session manager, which is unique to this print job
    PrintWorkflowBackgroundSession sessionManager = workflowTriggerDetails.PrintWorkflowSession;

    // add the event handler callback routines
    sessionManager.SetupRequested += OnSetupRequested;
    sessionManager.Submitted += OnXpsOMPrintSubmitted;

    // Allow the event source to start
    // This call blocks until all of the workflow callbacks complete
    sessionManager.Start();
}
```

Wenn die- `Start` Methode aufgerufen wird, gibt der Sitzungs-Manager zuerst das **[setuprequraise](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)** -Ereignis aus. Dieses Ereignis macht allgemeine Informationen über die Druck Aufgabe sowie das Druck Ticket verfügbar. Zu diesem Zeitpunkt kann das Druck Ticket im Hintergrund bearbeitet werden.

```csharp
private void OnSetupRequested(PrintWorkflowBackgroundSession sessionManager, PrintWorkflowBackgroundSetupRequestedEventArgs printTaskSetupArgs) {
    // Take out a deferral here and complete once all the callbacks are done
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get general information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;

    // edit the print ticket
    WorkflowPrintTicket printTicket = printTaskSetupArgs.GetUserPrintTicketAsync();

    // ...
```

Wichtig ist, dass es sich bei der Behandlung von **setuprequors** handelt, dass die APP bestimmt, ob eine Vordergrund Komponente gestartet werden soll. Dies kann abhängig von einer Einstellung sein, die zuvor im lokalen Speicher gespeichert wurde, oder einem Ereignis, das während der Bearbeitung des Druck Tickets aufgetreten ist, oder es kann eine statische Einstellung Ihrer speziellen APP sein.

```csharp
// ...

if (UIrequested) {
    printTaskSetupArgs.SetRequiresUI();

    // Any data that is to be passed to the foreground task must be stored the app's local storage.
    // It should be prefixed with the sourceApplicationName string and the SessionId string, so that
    // it can be identified as pertaining to this workflow app session.
}

// Complete the deferral taken out at the start of OnSetupRequested
setupRequestedDeferral.Complete();
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>Aufgaben im Vordergrund für den Druckauftrag ausführen (optional)

Wenn die **[setrequiresui](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** -Methode aufgerufen wurde, untersucht das Drucksystem die Manifestressource auf den Einstiegspunkt der Vordergrund Anwendung. Das- `Application/Extensions` Element der Datei " *Package. appxmanifest* " muss über die folgenden Zeilen verfügen. Ersetzen Sie den Wert von `EntryPoint` durch den Namen der Vordergrund-app.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" />
```

Als nächstes Ruft das Drucksystem die **onaktivierte** Methode für den angegebenen app-Einstiegspunkt auf. In der **onaktivierten** -Methode der _app.XAML.cs_ -Datei sollte die Workflow-app die Aktivierungs Art überprüfen, um zu überprüfen, ob es sich um eine Workflow Aktivierung handelt. Wenn dies der Fall ist, kann die Workflow-app die Aktivierungs Argumente in ein **[printworkflowuiactivatedeventargs](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)** -Objekt umwandeln, das ein **[printworkflowforegroundsession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** -Objekt als Eigenschaft verfügbar macht. Dieses Objekt enthält, wie das zugehörige Hintergrund Pendant im vorherigen Abschnitt, Ereignisse, die vom Drucksystem ausgelöst werden, und Sie können diesen Handlern zuweisen. In diesem Fall wird die Ereignis Behandlungs Funktion in einer separaten Klasse namens implementiert `WorkflowPage` .

Zuerst in der Datei _app.XAML.cs_ :

```csharp
protected override void OnActivated(IActivatedEventArgs args){

    if (args.Kind == ActivationKind.PrintWorkflowForegroundTask) {

        // the app should instantiate a new UI view so that it can properly handle the case when
        // several print jobs are active at the same time.
        Frame rootFrame = new Frame();
        if (null == Window.Current.Content)
        {
            rootFrame.Navigate(typeof(WorkflowPage));
            Window.Current.Content = rootFrame;
        }

        // Get the main page
        WorkflowPage workflowPage = (WorkflowPage)rootFrame.Content;

        // Make sure the page knows it's handling a foreground task activation
        workflowPage.LaunchType = WorkflowPage.WorkflowPageLaunchType.ForegroundTask;

        // Get the activation arguments
        PrintWorkflowUIActivatedEventArgs printTaskUIEventArgs = args as PrintWorkflowUIActivatedEventArgs;

        // Get the session manager
        PrintWorkflowForegroundSession taskSessionManager = printTaskUIEventArgs.PrintWorkflowSession;

        // Add the callback handlers - these methods are in the workflowPage class
        taskSessionManager.SetupRequested += workflowPage.OnSetupRequested;
        taskSessionManager.XpsDataAvailable += workflowPage.OnXpsDataAvailable;

        // start raising the print workflow events
        taskSessionManager.Start();
    }
}
```

Sobald die Benutzeroberfläche über angefügte Ereignishandler verfügt und die **onaktivierte** Methode beendet wurde, gibt das Drucksystem das **[setuprequoniert](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** -Ereignis aus, das von der Benutzeroberfläche behandelt werden soll. Dieses Ereignis stellt die gleichen Daten bereit, die das Setup Ereignis für die Hintergrundaufgabe bereitgestellt hat, einschließlich der Informationen zum Druckauftrag und zum Druck Ticket, aber ohne die Möglichkeit, den Start zusätzlicher Benutzeroberflächen anzufordern. In der Datei _WorkflowPage.XAML.cs_ :

```csharp
internal void OnSetupRequested(PrintWorkflowForegroundSession sessionManager, PrintWorkflowForegroundSetupRequestedEventArgs printTaskSetupArgs) {
    // If anything asynchronous is going to be done, you need to take out a deferral here,
    // since otherwise the next callback happens once this one exits, which may be premature
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;
    // the following string should be used when storing data that pertains to this workflow session
    // (such as user input data that is meant to change the print content later on)
    string localStorageVariablePrefix = string.Format("{0}::{1}::", sourceApplicationName, sessionID);

    try
    {
        // receive and store user input
        // ...
    }
    catch (Exception ex)
    {
        string errorMessage = ex.Message;
        Debug.WriteLine(errorMessage);
    }
    finally
    {
        // Complete the deferral taken out at the start of OnSetupRequested
        setupRequestedDeferral.Complete();
    }
}
```

Als nächstes gibt das Drucksystem das **[xpsdataavailable](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** -Ereignis für die Benutzeroberfläche aus. Im Handler für dieses Ereignis kann die Workflow-app auf alle Daten zugreifen, die für das Setup Ereignis verfügbar sind, und Sie kann die XPS-Daten zusätzlich direkt lesen, entweder als Stream von Rohdaten Bytes oder als Objektmodell. Der Zugriff auf die XPS-Daten ermöglicht der Benutzeroberfläche die Bereitstellung von Druckvorschau Diensten und das Bereitstellen zusätzlicher Informationen für den Benutzer über die Vorgänge, die von der Workflow-app für die Daten ausgeführt werden.

Als Teil dieses Ereignis Handlers muss die Workflow-app ein Verzögerungs Objekt abrufen, wenn die Interaktion mit dem Benutzer fortgesetzt wird. Ohne eine Verzögerung betrachtet das Drucksystem die Benutzeroberflächen Aufgabe abgeschlossen, wenn der **xpsdataavailable** -Ereignishandler beendet wird oder eine Async-Methode aufruft. Wenn die APP alle erforderlichen Informationen aus der Interaktion des Benutzers mit der Benutzeroberfläche gesammelt hat, sollte Sie den Verzögerungs Vorgang vervollständigen, damit das Drucksystem dann fortfahren kann.

```csharp
internal async void OnXpsDataAvailable(PrintWorkflowForegroundSession sessionManager, PrintWorkflowXpsDataAvailableEventArgs printTaskXpsAvailableEventArgs)
{
    // Take out a deferral
    Deferral xpsDataAvailableDeferral = printTaskXpsAvailableEventArgs.GetDeferral();

    SpoolStreamContent xpsStream = printTaskXpsAvailableEventArgs.Operation.XpsContent.GetSourceSpoolDataAsStreamContent();

    IInputStream inputStream = xpsStream.GetInputSpoolStream();

    using (var inputReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        // Read the XPS data from input stream
        byte[] xpsData = new byte[inputReader.UnconsumedBufferLength];
        while (inputReader.UnconsumedBufferLength > 0)
        {
            inputReader.ReadBytes(xpsData);
            // Do something with the XPS data, e.g. preview
            // ...
        }
    }

    // Complete the deferral taken out at the start of this method
    xpsDataAvailableDeferral.Complete();
}
```

Außerdem bietet die **[printworkflowsubmittedoperation](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** -Instanz, die von den Ereignis Argumenten verfügbar gemacht wird, die Option, den Druckauftrag abzubrechen oder anzugeben, dass der Auftrag erfolgreich ist, aber kein Ausgabe Druckauftrag benötigt wird. Dies erfolgt durch Aufrufen der **[Complete](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** -Methode mit einem **[printworkflowsubmittedstatus](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)** -Wert.

> [!NOTE]
> Wenn die Workflow-app den Druckauftrag abbricht, wird dringend empfohlen, eine Popup Benachrichtigung bereitzustellen, die angibt, warum der Auftrag abgebrochen wurde.

## <a name="do-final-background-work-on-the-print-content"></a>Abschließende Hintergrundarbeit am Druck Inhalt

Nachdem die Benutzeroberfläche die Verzögerung im **printtaskxpsdataavailable** -Ereignis abgeschlossen hat (oder wenn der UI-Schritt umgangen wurde), wird das über **[mittelte](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** Ereignis für die Hintergrundaufgabe ausgelöst. Im-Handler für dieses Ereignis kann die Workflow-app Zugriff auf alle Daten erhalten, die vom **xpsdataavailable** -Ereignis bereitgestellt werden. Anders als bei einem der vorangegangenen Ereignisse bietet gesendete **jedoch** auch *Schreib* Zugriff auf den endgültigen Inhalt des Druckauftrags über eine **[printworkflowtarget](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)** -Instanz.

Das Objekt, das verwendet wird, um die Daten für den endgültigen Druck zu spoolen, hängt davon ab, ob als Rohdaten Strom oder als XPS-Objektmodell auf die Quelldaten zugegriffen wird. Wenn die Workflow-app über einen Bytestream auf die Quelldaten zugreift, wird ein Ausgabe Byte-Stream bereitgestellt, in den die abschließenden Auftragsdaten geschrieben werden. Wenn die Workflow-app über das Objektmodell auf die Quelldaten zugreift, wird ein Dokumentwriter bereitgestellt, um Objekte in den Ausgabe Auftrag zu schreiben. In beiden Fällen sollte die Workflow-app alle Quelldaten lesen, alle erforderlichen Daten ändern und die geänderten Daten in das Ausgabeziel schreiben.

Wenn die Hintergrundaufgabe das Schreiben der Daten beendet hat, sollte Sie für das entsprechende **printworkflowsubmittedoperation** -Objekt **Complete** aufrufen. Sobald die Workflow-app diesen Schritt abgeschlossen hat und der über **mittelte** Ereignishandler beendet wird, wird die Workflow Sitzung geschlossen, und der Benutzer kann den Status des letzten Druckauftrags über die standardmäßigen Druck Dialogfelder überwachen.

## <a name="final-steps"></a>Abschließende Schritte

### <a name="register-the-print-workflow-app-to-the-printer"></a>Registrieren der Druck Workflow-app für den Drucker

Die Workflow-app ist mit einem Drucker verknüpft, der denselben Typ der Metadatendatei Übermittlung wie für wsdas verwendet. Tatsächlich kann eine einzige UWP-Anwendung sowohl als Workflow-app als auch als wsda fungieren, das Funktionen für Druckaufgaben Einstellungen bereitstellt. Befolgen Sie die entsprechenden [wsda-Schritte zum Erstellen der metadatenzuordnung](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-2--create-device-metadata).

Der Unterschied besteht darin, dass wsdas automatisch für den Benutzer aktiviert wird (die APP wird immer gestartet, wenn der Benutzer auf dem zugeordneten Gerät druckt), die Workflow-apps nicht. Sie verfügen über eine separate Richtlinie, die festgelegt werden muss.

### <a name="set-the-workflow-apps-policy"></a>Festlegen der Richtlinie für die Workflow-app

Die Workflow-app-Richtlinie wird von PowerShell-Befehlen auf dem Gerät festgelegt, auf dem die Workflow-app ausgeführt werden soll. Die Befehle Set-Printer, Add-Printer (vorhandener Port) und Add-Printer (neuer WSD-Port) werden so geändert, dass Workflow Richtlinien festgelegt werden können.

* `Disabled`: Workflow-apps werden nicht aktiviert.
* `Uninitialized`: Workflow-apps werden aktiviert, wenn die Workflow-DCA im System installiert ist. Wenn die APP nicht installiert ist, wird der Druckvorgang trotzdem fortgesetzt.
* `Enabled`: Der Workflow Vertrag wird aktiviert, wenn die Workflow-DCA im System installiert ist. Wenn die APP nicht installiert ist, tritt beim Drucken ein Fehler auf.

Mit dem folgenden Befehl wird die Workflow-app auf dem angegebenen Drucker benötigt.

```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy Enabled
```

Ein lokaler Benutzer kann diese Richtlinie auf einem lokalen Drucker ausführen, oder für die Enterprise-Implementierung kann der Drucker Administrator diese Richtlinie auf dem Drucker Server ausführen. Die Richtlinie wird dann mit allen Clientverbindungen synchronisiert. Der Drucker Administrator kann diese Richtlinie immer dann verwenden, wenn ein neuer Drucker hinzugefügt wird.

## <a name="see-also"></a>Weitere Informationen

[Beispiel für Workflow-app](https://github.com/Microsoft/print-oem-samples)

[Windows. Graphics. Printing. Workflow-Namespace](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow)
