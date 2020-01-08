---
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: Schreiben eines benutzerdefinierten Plug-Ins für das Device Portal
description: Hier erfahren Sie, wie Sie eine UWP-App schreiben, die mit dem Windows Device Portal eine Webseite hostet und Diagnoseinformationen bereitstellt.
ms.date: 03/24/2017
ms.topic: article
keywords: Windows 10, UWP, Geräte Portal
ms.localizationpriority: medium
ms.openlocfilehash: 4881fe961979243849728d3f835c449e0f71f4b4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683843"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>Schreiben eines benutzerdefinierten Plug-Ins für das Geräteportal

Hier erfahren Sie, wie Sie eine UWP-App schreiben, die mit dem Windows Device Portal eine Webseite hostet und Diagnoseinformationen bereitstellt.

Ab dem Creators Update können Sie im Device Portal die Diagnoseschnittstellen Ihrer App hosten. Dieser Artikel beschreibt die drei Teile, die zum Erstellen eines DevicePortalProvider für Ihre App erforderlich sind – die Appxmanifest-Änderungen, das Einrichten der App-Verbindung zum Device Portal-Dienst und das Behandeln einer eingehenden Anforderung. Eine Beispiel-App wird auch bereitgestellt, um den Einstieg zu erleichtern (in Kürze verfügbar). 

## <a name="create-a-new-uwp-app-project"></a>Erstellen eines neuen UWP-App-Projekts
In dieser Anleitung erstellen wir der Einfachheit halber alles in einer Projektmappe.

Erstellen Sie in Microsoft Visual Studio 2019 ein neues UWP-App-Projekt. Wechseln Sie zu Datei > Neues > Projekt, und wählen Sie leere app (Windows C#Universal) für aus, und klicken Sie dann auf Weiter. Im Dialogfeld Neues Projekt konfigurieren. Nennen Sie das Projekt "deviceportalprovider", und klicken Sie dann auf erstellen. Diese App soll den App-Dienst enthalten. Stellen Sie sicher, dass Sie "Windows 10 Creators Update" auswählen (10,0; Build 15063) "zur Unterstützung von.  Möglicherweise müssen Sie Visual Studio aktualisieren oder das neue SDK installieren – Details dazu finden Sie [hier](https://blogs.windows.com/buildingapps/2017/04/05/updating-tooling-windows-10-creators-update/). 

## <a name="add-the-deviceportalprovider-extension-to-your-packageappxmanifest-file"></a>Hinzufügen der devicePortalProvider-Erweiterung zur package.appxmanifest-Datei
Sie müssen der *package.appxmanifest*-Datei Code hinzufügen, damit Sie Ihre App als Device Portal-Plug-In nutzen können. Fügen Sie zunächst die folgenden Namespace-Definitionen am Beginn der Datei ein. Fügen Sie auch das `IgnorableNamespaces`-Attribut hinzu.

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

Um zu deklarieren, dass die App ein Device Portal-Anbieter ist, müssen Sie einen App-Dienst und eine neue Device Portal-Anbietererweiterung, die den Dienst verwendet, hinzufügen. Fügen Sie dem `Extensions`-Element unter `Application` die Erweiterungen windows.appService und windows.devicePortalProvider hinzu. Stellen Sie sicher, dass die `AppServiceName`-Attribute in allen Erweiterungen übereinstimmen. Damit wird dem Device Portal-Dienst mitgeteilt, dass dieser App-Dienst zur Behandlung von Anforderungen für den Ereignishandler-Namespace gestartet werden kann. 

```xml
...   
<Application 
    Id="App" 
    Executable="$targetnametoken$.exe"
    EntryPoint="DevicePortalProvider.App">
    ...
    <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MySampleProvider.SampleProvider">
            <uap:AppService Name="com.sampleProvider.wdp" />
        </uap:Extension>
        <uap4:Extension Category="windows.devicePortalProvider">
            <uap4:DevicePortalProvider 
                DisplayName="My Device Portal Provider Sample App" 
                AppServiceName="com.sampleProvider.wdp" 
                HandlerRoute="/MyNamespace/api/" />
        </uap4:Extension>
    </Extensions>
</Application>
...
```

Das `HandlerRoute`-Attribut verweist auf den REST-Namespace, der von Ihrer App in Anspruch genommen wird. Alle HTTP-Anforderungen für diesen Namespace (implizit gefolgt von einem Platzhalter), die der Device Portal-Dienst empfängt, werden zur Behandlung an Ihre App gesendet. In diesem Fall wird jede erfolgreich authentifizierte HTTP-Anforderung für `<ip_address>/MyNamespace/api/*` an Ihre App gesendet. Konflikte zwischen Handler-Routen werden anhand einer Überprüfung nach dem Prinzip „die längste gewinnt“ aufgelöst: die Route mit der größten Übereinstimmung mit den Anforderungen wird ausgewählt. Eine Anforderung für „/MyNamespace/api/foo“ wird also z. B. dem Anbieter „/MyNamespace/api“ anstatt „/MyNamespace“ zugeordnet.  

Damit dies ausgeführt werden kann, sind zwei neue Funktionen erforderlich. Diese Funktionen müssen ebenfalls der *package.appxmanifest*-Datei hinzugefügt werden.

```xml
...
<Capabilities>
    ...
    <Capability Name="privateNetworkClientServer" />
    <rescap:Capability Name="devicePortalProvider" />
</Capabilities>
...
```

> [!NOTE]
> Die Funktion „devicePortalProvider” ist eingeschränkt („rescap”), das heißt, Sie müssen zuerst die Zustimmung vom Store erhalten, bevor Sie Ihre App dort veröffentlichen können. Dies hindert Sie jedoch nicht daran, Ihre App lokal per Querladen zu testen. Weitere Informationen zu eingeschränkten Funktionen finden Sie unter [Deklarationen von App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

## <a name="set-up-your-background-task-and-winrt-component"></a>Einrichten der Hintergrundaufgabe und der WinRT-Komponente
Um die Device Portal-Verbindung einzurichten, muss Ihre App eine App-Dienstverbindung aus dem Device Portal-Dienst mit der in Ihrer App ausgeführten Instanz des Device Portal herstellen. Fügen Sie zu diesem Zweck Ihrer Anwendung eine neue WinRT-Komponente mit einer Klasse hinzu, die [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) implementiert.

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

Stellen Sie sicher, dass der Name mit dem Namespace und dem Klassennamen übereinstimmt, die von AppService EntryPoint („MySampleProvider.SampleProvider”) eingerichtet wurden. Wenn Sie die erste Anforderung an Ihren Device Portal-Anbieter richten, speichert das Device Portal die Anforderung, startet die Hintergrundaufgabe Ihrer App, ruft die Methode **Run** auf und übergibt eine [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance). Damit richtet Ihre App dann eine [**DevicePortalConnection**](https://docs.microsoft.com/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection)-Instanz ein.

```csharp
// Implement background task handler with a DevicePortalConnection
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take a deferral to allow the background task to continue executing 
    this.taskDeferral = taskInstance.GetDeferral();
    taskInstance.Canceled += TaskInstance_Canceled;

    // Create a DevicePortal client from an AppServiceConnection 
    var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    var appServiceConnection = details.AppServiceConnection;
    this.devicePortalConnection = DevicePortalConnection.GetForAppServiceConnection(appServiceConnection);

    // Add Closed, RequestReceived handlers 
    devicePortalConnection.Closed += DevicePortalConnection_Closed;
    devicePortalConnection.RequestReceived += DevicePortalConnection_RequestReceived;
}
```

Es gibt zwei Ereignisse, die von der APP behandelt werden müssen, um die Anforderungs Verarbeitungs Schleife abzuschließen: **Closed**, für den Fall, dass der Geräte Portal Dienst heruntergefahren wird und Anforderer [**empfangen**](https://docs.microsoft.com/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs)wird, der eingehende HTTP-Anforderungen durchsucht und die Hauptfunktionalität des Geräte Portal Anbieters bereitstellt. 

## <a name="handle-the-requestreceived-event"></a>Behandeln des „RequestReceived“-Ereignisses
Das **RequestReceived**-Ereignis wird einmal für jede HTTP-Anforderung ausgelöst, die für die angegebene Handler-Route Ihres Plug-Ins erfolgt. Die Anforderungsbehandlungsschleife für Device Portal-Anbieter ähnelt der in NodeJS Express: die Anforderungs- und Antwortobjekte werden zusammen mit dem Ereignis bereitgestellt, und der Handler reagiert, indem er das Antwortobjekt ausfüllt. In Device Portal-Anbietern verwenden das **RequestReceived**-Ereignis und seine Handler [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/windows.web.http.httprequestmessage)- und [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/windows.web.http.httpresponsemessage)-Objekte.   

```csharp
// Sample RequestReceived echo handler: respond with an HTML page including the query and some additional process information. 
private void DevicePortalConnection_RequestReceived(DevicePortalConnection sender, DevicePortalConnectionRequestReceivedEventArgs args)
{
    var req = args.RequestMessage;
    var res = args.ResponseMessage;

    if (req.RequestUri.AbsolutePath.EndsWith("/echo"))
    {
        // construct an html response message
        string con = "<h1>" + req.RequestUri.AbsoluteUri + "</h1><br/>";
        var proc = Windows.System.Diagnostics.ProcessDiagnosticInfo.GetForCurrentProcess();
        con += String.Format("This process is consuming {0} bytes (Working Set)<br/>", proc.MemoryUsage.GetReport().WorkingSetSizeInBytes);
        con += String.Format("The process PID is {0}<br/>", proc.ProcessId);
        con += String.Format("The executable filename is {0}", proc.ExecutableFileName);
        res.Content = new HttpStringContent(con);
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
        res.StatusCode = HttpStatusCode.Ok;            
    }
    //...
}
```

In diesem Beispiel für einen Anforderungshandler rufen wir zunächst die Anforderungs- und Antwortobjekte aus dem *Args*-Parameter ab, erstellen dann eine Zeichenfolge mit der Anforderungs-URL und einige weitere HTML-Formatierungen. Dies wird in das Antwortobjekt als [**HttpStringContent**](https://docs.microsoft.com/uwp/api/windows.web.http.httpstringcontent)-Instanz eingefügt. Andere [**IHttpContent**](https://docs.microsoft.com/uwp/api/windows.web.http.ihttpcontent)-Klassen, z. B. diejenigen für „String” und „Buffer”, sind ebenfalls zulässig.

Die Antwort wird dann als HTTP-Antwort festgelegt und der Statuscode 200 (OK) zugeordnet. In dem Browser, der den ursprünglichen Aufruf vorgenommen hat, sollte dies wie erwartet gerendert werden. Wenn der **RequestReceived**-Ereignishandler zurückkehrt, wird die Antwortnachricht automatisch an den Benutzer-Agent zurückgegeben: es ist keine zusätzliche „send“-Methode erforderlich.

![Antwortnachricht des Device Portal](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>Bereitstellen von statischem Inhalt
Statischer Inhalt kann direkt aus einem Ordner in Ihrem Paket bereitgestellt werden. Dadurch können Sie Ihrem Anbieter sehr einfach eine Benutzeroberfläche hinzufügen.  Die einfachste Möglichkeit zur Bereitstellung von statischem Inhalt ist, einen Inhaltsordner in Ihrem Projekt zu erstellen, der einer URL zugeordnet werden kann.

![Statischer Inhaltsordner im Device Portal](images/device-portal/plugin-static-content.png)
 
Fügen Sie dann Ihrem **RequestReceived**-Ereignishandler einen Routenereignishandler hinzu, der statische Inhaltsrouten erkennt und eine Anforderung entsprechend zuordnet.  

```csharp
if (req.RequestUri.LocalPath.ToLower().Contains("/www/")) {
    var filePath = req.RequestUri.AbsolutePath.Replace('/', '\\').ToLower();
    filePath = filePath.Replace("\\backgroundprovider", "")
    try {
        var fileStream = Windows.ApplicationModel.Package.Current.InstalledLocation.OpenStreamForReadAsync(filePath).GetAwaiter().GetResult();
        res.StatusCode = HttpStatusCode.Ok;
        res.Content = new HttpStreamContent(fileStream.AsInputStream());
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    } catch(FileNotFoundException e) {
        string con = String.Format("<h1>{0} - not found</h1>\r\n", filePath);
        con += "Exception: " + e.ToString();
        res.Content = new HttpStringContent(con);
        res.StatusCode = HttpStatusCode.NotFound;
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    }
}
```
Stellen Sie sicher, dass alle Dateien in dem Inhaltsordner als „Inhalt“ gekennzeichnet sind und im Eigenschaftenmenü von Visual Studio auf „Kopieren, wenn neuer“ oder „Immer kopieren“ festgelegt sind.  Damit stellen Sie sicher, dass die Dateien sich bei der Bereitstellung in Ihrem AppX-Paket befinden.  

![Konfigurieren des Kopierens statischer Inhaltsdateien](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>Verwenden vorhandener Device Portal-Ressourcen und APIs
Statischer, von einem Device Portal-Anbieter bereitgestellter Inhalt wird auf demselben Port wie der Kerndienst des Device Portal bereitgestellt.  Dies bedeutet, dass Sie mit einfachen `<link>`- und `<script>`-Tags im HTML-Code auf das im Device Portal vorhandene JS und CSS verweisen können. Im Allgemeinen empfehlen wir die Verwendung von *rest.js*, das alle Kern-REST-APIs des Device Portal in einem geeigneten webbRest-Objekt einschließt, und die *common.css*-Datei, mit der Sie Ihren Inhalt entsprechend der Device Portal-Benutzeroberfläche formatieren können. Ein Beispiel hierzu finden Sie auf der *index.html*-Seite in der Beispiel-App. Hier werden mit *rest.js* der Name des Geräts und die ausgeführten Prozesse von Device Portal abgerufen. 

![Ausgabe des Device Portal-Plug-Ins](images/device-portal/plugin-output.png)
 
Wichtig: Bei Verwendung der HttpPost/DeleteExpect200-Methoden für webbRest erfolgt die [CSRF-Behandlung](https://docs.microsoft.com/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) automatisch, wodurch Ihre Webseite zustandsverändernde REST-APIs aufrufen kann.  

> [!NOTE] 
> Der statische, im Device Portal enthaltene Inhalt kann Änderungen unterworfen sein. APIs werden im Allgemeinen nicht häufig geändert, aber es kann vorkommen, insbesondere können die Dateien *common.js* und *controls.js* Änderungen unterworfen sein. Daher sollte Ihr Anbieter diese Dateien nicht verwenden. 

## <a name="debugging-the-device-portal-connection"></a>Debuggen der Device Portal-Verbindung
Um die Hintergrundaufgabe zu debuggen, müssen Sie die Art und Weise ändern, wie Visual Studio Ihren Code ausführt. Führen Sie die folgenden Schritte für das Debuggen einer App-Dienstverbindung aus, um zu überprüfen, wie Ihr Anbieter die HTTP-Anforderungen behandelt:

1.  Wählen Sie im Debuggermenü die DevicePortalProvider-Eigenschaften aus. 
2.  Aktivieren Sie auf der Registerkarte „Debuggen“ im Abschnitt „Startaktion“ das Kontrollkästchen „Eigenen Code zunächst nicht starten, sondern debuggen“.  
![Versetzen des Plug-Ins in den Debugmodus](images/device-portal/plugin-debug-mode.png)
3.  Setzen Sie in der RequestReceived-Handlerfunktion einen Haltepunkt.
![Haltepunkt beim requestempfangenden Handler](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> Stellen Sie sicher, dass die Build-Architektur genau mit der Architektur des Ziels übereinstimmt. Wenn Sie einen 64-Bit-PC verwenden, müssen Sie einen AMD64-Build bereitstellen. 
4.  Drücken Sie F5, um Ihre App bereitzustellen.
5.  Deaktivieren Sie das Device Portal, und aktivieren Sie es wieder, damit es Ihre App findet (nur erforderlich, wenn Sie Ihr App-Manifest ändern – ansonsten können Sie einfach erneut bereitstellen und diesen Schritt überspringen). 
6.  Öffnen Sie in Ihrem Browser den Namespace des Anbieters, und der Haltepunkt sollte erreicht werden.

## <a name="related-topics"></a>Zugehörige Themen
* [Übersicht über das Windows-Geräte Portal](device-portal.md)
* [Erstellen und Verwenden eines App-Diensts](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)


