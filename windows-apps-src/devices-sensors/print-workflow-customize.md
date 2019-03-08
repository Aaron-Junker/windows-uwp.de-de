---
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: Anpassen des Druck-Workflows
description: Erstellen Sie angepasste Druck-Workflows entsprechend den Anforderungen Ihrer Organisation.
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, Uwp, Drucken
ms.localizationpriority: medium
ms.openlocfilehash: 96e308793e60c0367c712fb93a5d25a056397568
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653235"
---
# <a name="customize-the-print-workflow"></a>Anpassen des Druck-Workflows

## <a name="overview"></a>Übersicht
Entwickler können den Druck-Workflow durch die Verwendung einer Druck-Workflow-App individuell anpassen. Druck-Workflow-Apps sind UWP-Apps, die die Funktionalität von [Microsoft Store Apps (WSDAs)](https://docs.microsoft.com/windows-hardware/drivers/devapps/) erweitern. Daher ist es hilfreich, sich mit WSDAs vertraut zu machen. 

Ähnlich wie bei WSDAs prüft das System, ob eine Workflow-App mit dem Drucker verknüpft ist, wenn der Benutzer einer Quellanwendung etwas ausdruckt und durch den Druckdialog navigiert. Ist dies der Fall, startet die Druck-Workflow-App (hauptsächlich als Hintergrundaufgabe; mehr dazu weiter unten). Eine Workflow-App ist in der Lage, sowohl das Druckticket (das XML-Dokument, das die Druckereinstellungen für den aktuellen Druckauftrag konfiguriert) als auch den tatsächlich zu druckenden XPS-Inhalt zu ändern. Diese Funktionalität kann optional dem Benutzer zur Verfügung gestellt werden, indem im Prozess eine Benutzeroberfläche gestartet wird. Nach ihrer Arbeit gibt sie den Druckinhalt und das Druckticket an den Treiber weiter.

Da es sich um Hintergrund- und Vordergrundkomponenten handelt und der Prozess funktional mit anderen Apps gekoppelt ist, kann eine Druck-Workflow-App für den Druck komplizierter zu implementieren sein als andere Kategorien von UWP-Apps. Es wird empfohlen, sich das [Workflow-App-Beispiel](https://github.com/Microsoft/print-oem-samples) anzusehen, während Sie dieses Handbuch lesen, um besser zu verstehen, wie die verschiedenen Funktionen implementiert werden können. Einige Funktionen, wie z. B. verschiedene Fehlerprüfungen und die Verwaltung der Benutzeroberfläche, fehlen zur Vereinfachung in diesem Handbuch.

## <a name="getting-started"></a>Erste Schritte

Die Workflow-App muss dem Drucksystem den Einstiegspunkt der Workflow-App angeben, damit sie zum richtigen Zeitpunkt gestartet werden kann. Dies geschieht durch Einfügen der folgenden Deklaration in das `Application/Extensions`-Element der *package.appxmanifest*-Datei des UWP-Projekts. 

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT] 
> Es gibt viele Szenarien, in denen das Anpassen des Druck keine Benutzereingaben erfordert. Aus diesem Grund werden standardmäßig Workflow-Apps für den Druck als Hintergrundaufgaben ausgeführt.

Wenn eine Workflow-App mit der Quellanwendung verknüpft ist, die den Druckauftrag gestartet hat (siehe dazu weiter unten), prüft das Drucksystem seine Manifestdateien auf einen Einstiegspunkt für die Hintergrundaufgabe.

## <a name="do-background-work-on-the-print-ticket"></a>Hintergrundarbeiten am Druckticket durchführen

Das erste, was das Drucksystem mit der Workflow-App macht, ist die Aktivierung der Hintergrundaufgabe (in diesem Fall die Klasse `WfBackgroundTask` im Namenspace `WFBackgroundTasks`). In der `Run`-Methode der Hintergrundaufgabe sollten Sie die Trigger-Details des als **[PrintWorkflowTriggerDetails](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)**-Instanz definieren. Damit steht Ihnen die spezielle Funktionalität für eine Hintergrundaufgabe eines Druck-Workflows zur Verfügung. Sie stellt die **[PrintWorkflowSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)**-Eigenschaft bereit, die eine Instanz von **[PrintWorkFlowBackgroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)** ist. Die Session-Klassen des Druck-Workflows – sowohl die Varianten für den Hintergrund als auch für den Vordergrund – steuern die Ablaufschritte der Druck-Workflow-App. 

Registrieren Sie dann Handler-Methoden für die beiden Ereignisse, die diese Session-Klasse auslösen wird. Diese Methoden werden Sie später definieren.

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

Wenn die `Start`-Methode aufgerufen wird, löst der Sitzungsmanager das Ereignis **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)** zuerst aus. Dieses Ereignis zeigt allgemeine Informationen über den Druckauftrag sowie das Druckticket an. In dieser Phase kann das Druckticket im Hintergrund bearbeitet werden. 

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

Wichtig ist, dass die App bei der Verarbeitung von **SetupRequested** bestimmt, ob sie eine Vordergrundkomponente startet. Dies kann von einer Einstellung abhängen, die zuvor lokal gespeichert wurde, oder von einem Ereignis, das während der Bearbeitung des Drucktickets aufgetreten ist, oder von einer statischen Einstellung Ihrer speziellen App.

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
}
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>Vordergrundarbeit für den Druckjob verarbeiten (optional)

Wurde die Methode **[SetRequiresUI](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** aufgerufen, dann überprüft das Drucksystem die Manifest-Datei für den Einstiegspunkt in die Vordergrund-Anwendung. Das `Application/Extensions`-Element der Datei *package.appxmanifest* muss die folgenden Zeilen haben. Ersetzen Sie den Wert von `EntryPoint` durch den Namen der Vordergrund-App.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" /> 
```

Als nächstes ruft das Drucksystem die **OnActivated**-Methode für den angegebenen App-Einsprungspunkt auf. In der **OnActivated**-Methode ihrer _App. xaml.cs_-Datei sollte die Workflow-Anwendung die Aktivierungsart prüfen, um zu überprüfen, ob es sich um eine Workflow-Aktivierung handelt. Wenn ja, kann die Workflow-Anwendung die Aktivierungsargumente in ein **[PrintWorkflowUIActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)**-Objekt umwandeln, das ein **[PrintWorkflowForegroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)**-Objekt als Eigenschaft ausgibt. Dieses Objekt enthält, wie sein Gegenstück im vorherigen Abschnitt, Ereignisse, die vom Drucksystem ausgelöst werden, und Sie können diesen Handler zuordnen. In diesem Fall wird die Ereignisbehandlungsfunktionalität in einer eigenen Klasse namens `WorkflowPage` implementiert.

Zuerst in der Datei _App.xaml.cs_:

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

Sobald das UI-Ereignis-Handler angehängt und die **OnActivated**-Methode beendet hat, löst das Drucksystem das **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)**-Ereignis für das zu behandelnde UI aus. Dieses Ereignis liefert die gleichen Daten wie das Setup-Ereignis der Hintergrundaufgabe, einschließlich der Druckauftragsinfo und des Druckticketdokuments, jedoch ohne die Möglichkeit, den Start eine zusätzlichen UIs anzufordern. In der Datei _WorkflowPage.xaml.cs_:

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

Als nächstes wird das Drucksystem das **[XpsDataAvailable](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)**-Ereignis für das UI auslösen. Im Handler für dieses Ereignis kann die Workflow-App auf alle dem Setup-Ereignis zur Verfügung stehenden Daten zugreifen und zusätzlich die XPS-Daten direkt lesen, entweder als Raw-Byte-Strom oder als Objektmodell. Der Zugriff auf die XPS-Daten ermöglicht es dem UI, Druckvorschau-Dienste zur Verfügung zu stellen und dem Benutzer zusätzliche Informationen über die Operationen zur Verfügung zu stellen, die die Workflow-App für die Daten ausführen wird. 

Im Rahmen dieses Ereignis-Handlers muss die Workflow-App ein Deferral-Objekt erhalten, wenn sie weiterhin mit dem Benutzer interagieren soll. Ohne eine Verzögerung betrachtet das Drucksystem den UI-Task als erledigt, wenn der **XpsDataAvailable**-Ereignis-Handler beendet wird oder eine asynchrone Methode aufgerufen wird. Wenn die App alle erforderlichen Informationen aus der Interaktion des Benutzers mit dem UI gesammelt hat, sollte sie die Verzögerung abschließen, damit das Drucksystem fortfahren kann.

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

Zusätzlich bietet die Instanz **[PrintWorkflowSubmittedOperation](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)**, die durch die Ereignisargumente bereitgestellt wird, die Option, den Druckauftrag abzubrechen oder anzuzeigen, dass der Auftrag erfolgreich ist, aber kein Ausgabedruckauftrag erforderlich ist. Dazu wird die **[Complete](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)**-Methode mit dem Wert **[PrintWorkflowSubmittedStatus](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)** aufgerufen.

> [!NOTE]
> Wenn die Workflow-App den Druckauftrag abbricht, empfiehlt es sich dringend, eine Popup-Benachrichtigung zu senden, aus der hervorgeht, warum der Druckauftrag abgebrochen wurde. 


## <a name="do-final-background-work-on-the-print-content"></a>Abschließende Hintergrundarbeiten am Druckinhalt durchführen

Nachdem das UI die Verzögerung des Ereignisses **PrintTaskXpsDataAvailable** abgeschlossen hat (oder wenn der Schritt des UIs umgangen wurde), löst das Drucksystem das Ereignis **[Submitted](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** für die Hintergrundaufgabe aus. Im Handler für dieses Ereignis kann die Workflow-App auf alle Daten zugreifen, die das Ereignis **XpsDataAvailable** zur Verfügung stellt. Im Gegensatz zu den vorherigen Ereignissen bietet **Submitted** jedoch auch *Schreibzugriff* auf den Inhalt des endgültigen Druckauftrags über eine **[PrintWorkflowTarget](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)**-Instanz. 

Das Objekt, mit dem die Daten für den endgültigen Druck gespoolt werden, hängt davon ab, ob auf die Quelldaten als Raw-Bity-Strom oder als XPS-Objektmodell zugegriffen wird. Wenn die Workflow-App über einen Byte-Stream auf die Quelldaten zugreift, wird ein Ausgabe-Byte-Stream bereitgestellt, in den die endgültigen Auftragsdaten geschrieben werden. Wenn die Workflow-App über das Objektmodell auf die Quelldaten zugreift, wird ein Document-Writer bereitgestellt, um Objekte in den Ausgabeauftrag zu schreiben. In jedem Fall sollte die Workflow-App alle Quelldaten lesen, die erforderlichen Daten modifizieren und die geänderten Daten in das Ausgabeziel schreiben.

Wenn die Hintergrundaufgabe das Schreiben der Daten beendet hat, sollte sie **Complete** für das entsprechende **PrintWorkflowSubmittedOperation**-Objekt aufrufen. Sobald die Workflow-App diesen Schritt abgeschlossen hat und der Eventhandler **Submitted** beendet wurde, wird die Workflow-Sitzung geschlossen und der Benutzer kann den Status des endgültigen Druckauftrags über die Standard-Druckdialoge überwachen.

## <a name="final-steps"></a>Letzte Schritte

### <a name="register-the-print-workflow-app-to-the-printer"></a>Druck-Workflow-App beim Drucker registrieren

Ihre Workflow-App ist mit einem Drucker verbunden, der die gleiche Art der Übermittlung von Metadaten wie bei WSDAs verwendet. Tatsächlich kann eine einzelne UWP-Anwendung sowohl als Workflow-App als auch als WSDA fungieren, die Druckaufgabeeinstellungen ermöglicht. Führen Sie die entsprechenden [WSDA-Schritte zum Anlegen der Metadaten-Zuordnung](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-2--create-device-metadata) aus.

Der Unterschied besteht darin, dass WSDAs zwar automatisch für den Benutzer aktiviert werden (die App wird immer gestartet, wenn der Benutzer über das zugehörige Gerät druckt), Workflow-Apps jedoch nicht. Sie haben eine eigene Richtlinie, die festgelegt werden muss.

### <a name="set-the-workflow-apps-policy"></a>Festlegen der Richtlinien für die Workflow-App
Die Workflow-App-Richtlinie wird durch Powershell-Befehle auf dem Gerät festgelegt, das die Workflow-App ausführen soll. Die Befehle Set-Printer, Add-Printer (bestehender Port) und Add-Printer (neuer WSD-Port) werden modifiziert, um das Festen von Workflow-Richtlinien zu ermöglichen. 
* `Disabled`: Workflow-apps werden nicht aktiviert werden.
* `Uninitialized`: Workflowanwendungen werden aktiviert, wenn der Workflow DCA im System installiert ist. Wenn die App nicht installiert ist, wird trotzdem gedruckt. 
* `Enabled`: Workflow-Vertrag wird aktiviert werden, wenn der Workflow DCA im System installiert ist. Wenn die App nicht installiert ist, schlägt das Drucken fehl. 

Mit dem folgenden Befehl wird die Workflow-App auf dem angegebenen Drucker als erforderlich festgelegt.
```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy On
```

Ein lokaler Benutzer kann diese Richtlinie auf einem lokalen Drucker ausführen, oder der Druckeradministrator kann diese Richtlinie auf dem Druckerserver ausführen. Die Richtlinie wird dann mit allen Client-Verbindungen synchronisiert. Der Druckeradministrator kann diese Richtlinie immer dann verwenden, wenn ein neuer Drucker hinzugefügt wird.

## <a name="see-also"></a>Siehe auch

[Workflow-app-Beispiel](https://github.com/Microsoft/print-oem-samples)

[Windows.Graphics.Printing.Workflow-namespace](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow)


