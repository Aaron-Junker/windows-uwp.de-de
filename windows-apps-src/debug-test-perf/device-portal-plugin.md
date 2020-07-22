---
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: Schreiben eines benutzerdefinierten Plug-Ins für das Geräteportal
description: Erfahre, wie du eine UWP-App schreibst, die mit dem Windows-Geräteportal eine Webseite hostet und Diagnoseinformationen bereitstellt.
ms.date: 07/06/2020
ms.topic: article
keywords: Windows 10, UWP, Geräteportal
ms.localizationpriority: medium
ms.openlocfilehash: b806344fa7e0517caf4d04efaaa605371a200202
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493205"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>Schreiben eines benutzerdefinierten Plug-Ins für das Geräteportal

Erfahre, wie du eine UWP-App schreibst, die mit dem Windows-Geräteportal eine Webseite hostet und Diagnoseinformationen bereitstellt.

Seit dem Windows 10 Creators Update (Version 1703, Build 15063) kannst du die Diagnoseschnittstellen deiner App im Geräteportal hosten. In diesem Artikel werden die drei Aspekte beschrieben, die zum Erstellen eines DevicePortalProvider für deine App erforderlich sind: die [Anwendungspaketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)-Änderungen, das Einrichten der App-Verbindung mit dem [Geräteportal-Dienst](/windows/uwp/debug-test-perf/device-portal) und das Behandeln eingehender Anforderungen.

## <a name="create-a-new-uwp-app-project"></a>Erstellen eines neuen UWP-App-Projekts

Erstelle in Microsoft Visual Studio ein neues UWP-App-Projekt. Wechsele zu **Datei > Neu > Projekt**, wähle **Leere App (Universelles Windows: C#)** aus, und klicke dann auf **Weiter**. **Konfiguriere dein neues Projekt** im Dialogfeld. Benenne das Projekt mit „DevicePortalProvider“, und klicke dann auf **Erstellen**. Diese App enthält später den App-Dienst. Möglicherweise musst du Visual Studio aktualisieren oder das aktuelle [Windows SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) installieren.

## <a name="add-the-deviceportalprovider-extension-to-your-application-package-manifest"></a>Hinzufügen der devicePortalProvider-Erweiterung zum Anwendungspaketmanifest

Du musst in der Datei *package.appxmanifest* Code hinzufügen, damit deine App als Geräteportal-Plug-In genutzt werden kann. Füge zunächst die folgenden Namespacedefinitionen am Beginn der Datei ein. Füge sie auch dem `IgnorableNamespaces`-Attribut hinzu.

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

Um zu deklarieren, dass die App ein Geräteportal-Anbieter ist, musst du einen App-Dienst und eine neue Geräteportal-Anbietererweiterung erstellen, die den Dienst verwendet. Füge dem `Extensions`-Element unter `Application` die Erweiterungen windows.appService und windows.devicePortalProvider hinzu. Stelle sicher, dass die `AppServiceName`-Attribute in allen Erweiterungen übereinstimmen. Damit wird dem Geräteportal-Dienst mitgeteilt, dass dieser App-Dienst zur Behandlung von Anforderungen an den Handlernamespace gestartet werden kann. 

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

Das `HandlerRoute`-Attribut verweist auf den REST-Namespace, der von deiner App beansprucht wird. Alle HTTP-Anforderungen für diesen Namespace (implizit gefolgt von einem Platzhalter), die der Geräteportal-Dienst empfängt, werden zur Verarbeitung an deine App gesendet. In diesem Fall wird jede erfolgreich authentifizierte HTTP-Anforderung für `<ip_address>/MyNamespace/api/*` an deine App gesendet. Konflikte zwischen Handlerrouten werden anhand einer Überprüfung nach dem Prinzip „die längste gewinnt“ aufgelöst: Die Route mit der größten Übereinstimmung mit den Anforderungen wird ausgewählt. Eine Anforderung für „/MyNamespace/api/foo“ wird also z. B. dem Anbieter „/MyNamespace/api“ und nicht „/MyNamespace“ zugeordnet.  

Dazu sind zwei neue Funktionen erforderlich. Diese Funktionen müssen auch der Datei *package.appxmanifest* hinzugefügt werden.

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
> Die Funktion „devicePortalProvider“ ist eingeschränkt („rescap“), das heißt, du musst zuerst die Zustimmung vom Store erhalten, bevor du deine App dort veröffentlichen kannst. Dies hindert dich jedoch nicht daran, deine App lokal per Querladen zu testen. Weitere Informationen zu eingeschränkten Funktionen findest du unter [Deklarationen von App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

## <a name="set-up-your-background-task-and-winrt-component"></a>Einrichten der Hintergrundaufgabe und der WinRT-Komponente
Um die Geräteportal-Verbindung einzurichten, muss deine App eine App-Dienstverbindung vom Geräteportal-Dienst mit der in deiner App ausgeführten Instanz des Geräteportals herstellen. Füge deiner Anwendung zu diesem Zweck eine neue WinRT-Komponente mit einer Klasse hinzu, die [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) implementiert.

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

Stelle sicher, dass der Name mit dem Namespace und dem Klassennamen übereinstimmt, die von AppService EntryPoint („MySampleProvider.SampleProvider“) eingerichtet wurden. Wenn du die erste Anforderung an deinen Geräteportal-Anbieter richtest, speichert das Geräteportal die Anforderung, startet die Hintergrundaufgabe deiner App, ruft die Methode **Run** auf und übergibt eine [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance). Damit richtet deine App dann eine [**DevicePortalConnection**](https://docs.microsoft.com/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection)-Instanz ein.

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

Es gibt zwei Ereignisse, die von der App behandelt werden müssen, um die Anforderungsverarbeitungsschleife abzuschließen: **Closed**, wenn der Geräteportal-Dienst heruntergefahren wird, und [**RequestReceived**](https://docs.microsoft.com/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs), bei dem eingehende HTTP-Anforderungen angezeigt und die Hauptfunktionen des Geräteportal-Anbieters bereitgestellt werden. 

## <a name="handle-the-requestreceived-event"></a>Behandeln des RequestReceived-Ereignisses
Das **RequestReceived**-Ereignis wird einmal für jede HTTP-Anforderung ausgelöst, die für die angegebene Handlerroute deines Plug-Ins erfolgt. Die Anforderungsbehandlungsschleife für Geräteportal-Anbieter ähnelt der in NodeJS Express: Die Anforderungs- und Antwortobjekte werden zusammen mit dem Ereignis bereitgestellt, und der Handler antwortet, indem er das Antwortobjekt ausfüllt. Bei Geräteportal-Anbietern verwenden das **RequestReceived**-Ereignis und seine Handler [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/windows.web.http.httprequestmessage)- und [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/windows.web.http.httpresponsemessage)-Objekte.   

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

In diesem Beispiel für einen Anforderungshandler rufen wir zunächst die Anforderungs- und Antwortobjekte aus dem Parameter *args* ab, erstellen dann eine Zeichenfolge mit der Anforderungs-URL und einige weitere HTML-Formatierungen. Dies wird als [**HttpStringContent**](https://docs.microsoft.com/uwp/api/windows.web.http.httpstringcontent)-Instanz in das Antwortobjekt eingefügt. Andere [**IHttpContent**](https://docs.microsoft.com/uwp/api/windows.web.http.ihttpcontent)-Klassen, z. B. diejenigen für „String“ und „Buffer“, sind ebenfalls zulässig.

Die Antwort wird dann als HTTP-Antwort festgelegt und dem Statuscode 200 (OK) zugeordnet. Sie sollte in dem Browser, der den ursprünglichen Aufruf vorgenommen hat, wie erwartet gerendert werden. Wenn der **RequestReceived**-Ereignishandler fertig ist, wird die Antwortnachricht automatisch an den Benutzer-Agent zurückgegeben: Es ist keine zusätzliche send-Methode erforderlich.

![Antwortnachricht des Geräteportals](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>Bereitstellen von statischem Inhalt
Statischer Inhalt kann direkt aus einem Ordner in deinem Paket bereitgestellt werden. Dadurch kannst du deinem Anbieter sehr einfach eine Benutzeroberfläche hinzufügen.  Die einfachste Möglichkeit zur Bereitstellung von statischem Inhalt ist, einen Inhaltsordner in deinem Projekt zu erstellen, dem eine URL zugeordnet werden kann.

![Statischer Inhaltsordner im Geräteportal](images/device-portal/plugin-static-content.png)
 
Füge dann deinem **RequestReceived**-Ereignishandler einen Routenhandler hinzu, der statische Inhaltsrouten erkennt und eine Anforderung entsprechend zuordnet.  

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
Stelle sicher, dass alle Dateien in dem Inhaltsordner auch als „Inhalt“ gekennzeichnet und im Eigenschaftenmenü von Visual Studio auf „Kopieren, wenn neuer“ oder „Immer kopieren“ festgelegt sind.  Damit stellst du sicher, dass die Dateien sich bei der Bereitstellung in deinem AppX-Paket befinden.  

![Konfigurieren des Kopierens von Dateien mit statischem Inhalt](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>Verwenden vorhandener Geräteportal-Ressourcen und -APIs
Statischer Inhalt, wird von einem Geräteportal-Anbieter an demselben Port wie der Kerndienst des Geräteportals verarbeitet.  Dies bedeutet, dass du mit einfachen `<link>`- und `<script>`-Tags im HTML-Code auf vorhandene JS- und CSS-Inhalte im Geräteportal verweisen kannst. Im Allgemeinen empfehlen wir die Verwendung von *rest.js*, das alle Kern-REST-APIs des Geräteportals in einem geeigneten webbRest-Objekt umschließt, und der Datei *common.css*, mit der du deinen Inhalt entsprechend der Benutzeroberfläche des Geräteportals formatieren kannst. Ein Beispiel hierzu findest du im Beispiel auf der Seite *index.html*. Hier werden mit *rest.js* der Name des Geräts und die ausgeführten Prozesse vom Geräteportal abgerufen. 

![Ausgabe des Geräteportal-Plug-Ins](images/device-portal/plugin-output.png)
 
Wichtig: Bei Verwendung der HttpPost-/DeleteExpect200-Methoden für webbRest erfolgt die [CSRF-Behandlung](https://docs.microsoft.com/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) automatisch, sodass deine Webseite zustandsverändernde REST-APIs aufrufen kann.  

> [!NOTE] 
> Der statische Inhalt im Geräteportal kann Breaking Changes unterliegen. APIs werden im Allgemeinen nicht sehr häufig geändert, aber es kann vorkommen – insbesondere in den Dateien *common.js* und *controls.js*. Daher sollte dein Anbieter diese Dateien nicht verwenden. 

## <a name="debugging-the-device-portal-connection"></a>Debuggen der Geräteportal-Verbindung
Um deine Hintergrundaufgabe zu debuggen, musst du die Art und Weise ändern, wie Visual Studio deinen Code ausführt. Führe die folgenden Schritte für das Debuggen einer App-Dienstverbindung aus, um zu überprüfen, wie dein Anbieter die HTTP-Anforderungen behandelt:

1.  Wähle im Menü „Debuggen“ die DevicePortalProvider-Eigenschaften aus. 
2.  Aktiviere auf der Registerkarte „Debuggen“ im Abschnitt „Startaktion“ das Kontrollkästchen „Eigenen Code zunächst nicht starten, sondern debuggen“.  
![Versetzen des Plug-Ins in den Debugmodus](images/device-portal/plugin-debug-mode.png)
3.  Platziere einen Haltepunkt in der RequestReceived-Handlerfunktion.
![Haltepunkt beim requestreceived-Handler](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> Stelle sicher, dass die Buildarchitektur genau mit der Zielarchitektur übereinstimmt. Wenn du einen 64-Bit-PC verwendest, musst du die Bereitstellung auch mit einem AMD64-Build durchführen. 
4.  Drücke F5, um deine App bereitzustellen.
5.  Deaktiviere das Geräteportal, und aktiviere es erneut, damit es deine App findet (dies ist nur bei Änderungen am App-Manifest erforderlich – andernfalls kannst du einfach eine erneute Bereitstellung durchführen und diesen Schritt überspringen). 
6.  Öffne in deinen Browser den Namespace des Anbieters. Der Haltepunkt sollte nun erreicht werden.

## <a name="related-topics"></a>Zugehörige Themen
* [Übersicht über das Windows-Geräteportal](device-portal.md)
* [Erstellen und Verwenden eines App-Diensts](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)


