---
title: Erstellen und Verwenden eines App-Diensts
description: Hier erfahren Sie, wie Sie eine App für die Universelle Windows-Plattform (UWP) erstellen, die Dienste für andere UWP-Apps bereitstellen kann, und wie Sie diese Dienste nutzen.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: Kommunikation zwischen APP und APP, prozessübergreifende Kommunikation, IPC, Hintergrund Messaging, Hintergrund Kommunikation, APP-zu-app, App Service
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd67c8a94ae7fc46956f5a6f7c3358e0e28965d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158824"
---
# <a name="create-and-consume-an-app-service"></a>Erstellen und Verwenden eines App-Diensts

App Services sind UWP-apps, die Dienste für andere UWP-apps bereitstellen. Sie entsprechen den Webdiensten auf einem Gerät. Ein App Service wird als Hintergrundaufgabe in der Host-app ausgeführt und kann den Dienst für andere apps bereitstellen. Beispielsweise kann ein App Service einen Barcode-Überprüfungs Dienst bereitstellen, der von anderen Anwendungen verwendet werden kann. Oder eine Unternehmens Suite von apps hat eine gängige Rechtschreibprüfung App Service, die für die anderen apps in der Suite verfügbar ist.  Mit App Services können Sie Dienste ohne Benutzeroberfläche erstellen, die apps auf demselben Gerät und ab Windows 10, Version 1607, auf Remote Geräten abrufen können.

Ab Windows 10, Version 1607, können Sie App-Dienste erstellen, die im gleichen Prozess wie die Vordergrund-App ausgeführt werden. Dieser Artikel konzentriert sich auf das Erstellen und Verwenden eines App Service, der in einem separaten Hintergrundprozess ausgeführt wird. Weitere Informationen zum Ausführen eines App Service in demselben Prozess wie der Anbieter finden [Sie unter Konvertieren eines App Service, um ihn in demselben Prozess wie](convert-app-service-in-process.md) die zugehörige Host-App auszuführen.

Ein Beispiel für ein App Service-Code finden Sie unter [universelle Windows-Plattform-App-Beispiele (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Erstellen eines neuen App-Dienstanbieterprojekts

In dieser Anleitung erstellen wir der Einfachheit halber alles in einer Projektmappe.

1. Erstellen Sie in Visual Studio 2015 oder höher ein neues UWP-App-Projekt, und nennen Sie es **appserviceprovider**.
    1. **Datei > neues > Projekt auswählen...** 
    2. Wählen Sie im Dialogfeld **Neues Projekt erstellen** die Option **leere app (universelle Windows-APP) c#** aus. Dabei handelt es sich um die APP, mit der der APP Service anderen UWP-Apps zur Verfügung gestellt wird.
    3. Klicken Sie auf **weiter**, benennen Sie das Projekt **appserviceprovider**, wählen Sie einen Speicherort für das Projekt aus, und klicken Sie dann auf **Erstellen**.

2. Wählen Sie mindestens **10.0.14393**aus, wenn Sie aufgefordert werden, ein **Ziel** und eine **Mindestversion** für das Projekt auszuwählen. Wenn Sie das neue **supportsmultipleinhaltungen** -Attribut verwenden möchten, müssen Sie Visual Studio 2017 oder Visual Studio 2019 und Ziel **10.0.15063** (**Windows 10 Creators Update**) oder höher verwenden.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Hinzufügen einer APP Service-Erweiterung zu "Package. appxmanifest"

Öffnen Sie im Projekt " **appserviceprovider** " die Datei " **Package. appxmanifest** " in einem Text-Editor: 

1. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste darauf. 
2. Wählen Sie **Öffnen mit**aus. 
3. Wählen Sie den **XML-Editor (Text)** aus. 

Fügen Sie die folgende `AppService` Erweiterung im- `<Application>` Element hinzu. Durch dieses Beispiel wird der `com.microsoft.inventory`-Dienst angekündigt, und die App wird als App-Dienstanbieter identifiziert. Der eigentliche Dienst wird als Hintergrundaufgabe implementiert. Das App Service-Projekt macht den Dienst für andere apps verfügbar. Wir empfehlen, einen umgekehrten Domänennamen für den Dienstnamen zu verwenden.

Beachten Sie, dass das `xmlns:uap4` Namespace Präfix und das- `uap4:SupportsMultipleInstances` Attribut nur gültig sind, wenn Sie Windows SDK Version 10.0.15063 oder höher abzielen. Sie können Sie problemlos entfernen, wenn Sie auf ältere SDK-Versionen abzielen.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

Das- `Category` Attribut identifiziert diese Anwendung als App Service-Anbieter.

Das- `EntryPoint` Attribut identifiziert die qualifizierte Namespace Klasse, die den Dienst implementiert, den wir als nächstes implementieren.

Das- `SupportsMultipleInstances` Attribut gibt an, dass jedes Mal, wenn der APP Service aufgerufen wird, dass er in einem neuen Prozess ausgeführt werden soll. Dies ist nicht erforderlich, ist jedoch für Sie verfügbar, wenn Sie diese Funktionalität benötigen und das 10.0.15063 SDK (**Windows 10 Creators Update**) oder höher als Zielversion verwendet. Außerdem sollte dem-Namespace vorangestellt werden `uap4` .

## <a name="create-the-app-service"></a>Erstellen des App-Diensts

1.  Ein App Service kann als Hintergrundaufgabe implementiert werden. Dadurch kann eine Vordergrund Anwendung einen app Service in einer anderen Anwendung aufrufen. Zum Erstellen eines App Service als Hintergrundaufgabe fügen Sie der Projekt**Mappe (Datei &gt; Add &gt; New Project**) einen neuen Windows-Runtime Component-Projekt mit dem Namen **myappservice**hinzu. Wählen Sie im Dialogfeld **Neues Projekt hinzufügen** die Option **installiert > Visual c#-> Windows-Runtime Komponente (universelle Windows-Komponente)** aus.
2.  Fügen Sie im **appserviceprovider** -Projekt einen Projekt-zu-Projekt-Verweis auf das neue Projekt **myappservice** hinzu (Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **appserviceprovider** > Projekt Mappe "Verweis Projekte **Hinzufügen**"  >  **Reference**  >  **Projects**  >  **Solution**, und wählen Sie **myappservice**  >  **OK**aus.) Dieser Schritt ist wichtig, da der APP Service nicht zur Laufzeit eine Verbindung herstellt, wenn Sie den Verweis nicht hinzufügen.
3.  Fügen Sie im Projekt **myappservice** die folgenden **using** -Anweisungen am Anfang von **Class1.cs**hinzu:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Benennen Sie **Class1.cs** in **Inventory.cs**um, und ersetzen Sie den Stubcode für **Class1** durch eine neue Hintergrundaufgaben Klasse mit dem Namen **Inventory**:

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    In dieser Klasse wird der App-Dienst ausgeführt.

    " **Run** " wird aufgerufen, wenn die Hintergrundaufgabe erstellt wird. Da Hintergrundaufgaben nach Abschluss der **Ausführung** beendet werden, nimmt der Code eine Verzögerung vor, damit die Hintergrundaufgabe die Anforderungen verarbeiten kann. Ein App Service, der als Hintergrundaufgabe implementiert wird, bleibt ungefähr 30 Sekunden nach dem Empfang eines Aufrufs erhalten, es sei denn, er wird innerhalb dieses Zeitfensters erneut aufgerufen, oder es wird eine Verzögerung durchgeführt. Wenn der APP-Dienst im selben Prozess wie der Aufrufer implementiert wird, ist die Lebensdauer des App Service an die Lebensdauer des Aufrufers gebunden.

    Die Lebensdauer von App Service hängt vom Aufrufer ab:

    * Wenn sich der Aufrufer im Vordergrund befindet, ist die APP Service-Lebensdauer mit der des Aufrufers identisch.
    * Wenn sich der Aufrufer im Hintergrund befindet, erhält der APP Service 30 Sekunden für die Durchführung. Das Abnehmen einer Verzögerung bietet eine zusätzliche Zeit in 5 Sekunden.

    " **Ontaskabgeb Rochen** " wird aufgerufen, wenn der Task abgebrochen wird. Der Task wird abgebrochen, wenn die Client-App [appserviceconnetction](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection)freigibt, die Client-App angehalten wird, das Betriebssystem heruntergefahren oder heruntergefahren wird oder das Betriebssystem nicht mehr über genügend Ressourcen verfügt, um den Task auszuführen.

## <a name="write-the-code-for-the-app-service"></a>Schreiben des Codes für den App-Dienst

**Onrequestempfangenen** ist der Ort, an dem der Code für den App Service eingeht. Ersetzen Sie den Stub **onrequestempfangenen** in der **Inventory.cs** von **myappservice**durch den Code aus diesem Beispiel. Dieser Code ruft einen Index für ein Bestandselement ab und übergibt ihn zusammen mit einer Befehlszeichenfolge an den Dienst, um den Namen und Preis des angegebenen Bestandselements abzurufen. Fügen Sie für Ihre eigenen Projekte Fehler Behandlungs Code hinzu.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if (inventoryIndex.HasValue &&
        inventoryIndex.Value >= 0 &&
        inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

Beachten Sie, dass **onrequestempfang\async** ist, da in diesem Beispiel ein erwarteter Methodenaufrufe an [sendresponseasync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) erfolgt. **async**

Eine Verzögerung wird vorgenommen, sodass der Dienst **Async** -Methoden im **onrequestempfangenden** Handler verwenden kann. Dadurch wird sichergestellt, dass der **onrequestempfangene** Vorgang nicht abgeschlossen wird, bis die Verarbeitung der Nachricht abgeschlossen ist.  [Sendresponseasync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) sendet das Ergebnis an den Aufrufer. **SendResponseAsync** signalisiert nicht den Abschluss des Aufrufs. Es ist der Abschluss der Verzögerung, die [sendmessageasync](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) signalisiert, dass **onrequestempfangenen** abgeschlossen ist. Der **Send Response Async** -aufrufsvorgang wird in einem try/Complete-Block umschließt, da Sie den Verzögerungs Vorgang auch dann ausführen müssen, wenn **sendresponanasync** eine Ausnahme auslöst.

App-Dienste verwenden [valueset](/uwp/api/Windows.Foundation.Collections.ValueSet) -Objekte zum Austauschen von Informationen. Die Größe der Daten, die übergeben werden können, ist nur durch die Systemressourcen begrenzt. Es gibt keine vordefinierten Schlüssel zur Verwendung in Ihrem **ValueSet**. Sie müssen bestimmen, welche Schlüsselwerte Sie zum Definieren des Protokolls für Ihren App-Dienst verwenden. Dieses Protokoll muss beim Schreiben des Aufrufers berücksichtigt werden. In diesem Beispiel haben wir einen Schlüssel namens mit einem Wert gewählt, der `Command` angibt, ob der APP Service den Namen des Inventur Elements oder seinen Preis angeben soll. Der Index des Inventur namens wird unter dem `ID` Schlüssel gespeichert. Der Rückgabewert wird unter dem `Result` Schlüssel gespeichert.

An den Aufrufer wird eine [AppServiceClosedStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus)-Enumeration zurückgegeben, um anzugeben, ob der Aufruf des App-Diensts erfolgreich war. Ein Beispiel dafür, wie der APP Service-Aufrufe fehlschlagen könnte, ist, wenn das Betriebssystem den Dienst Endpunkt abbricht, da seine Ressourcen überschritten wurden. Sie können über [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) zusätzliche Fehlerinformationen zurückgeben. In diesem Beispiel verwenden wir einen Schlüssel mit dem Namen `Status` , um ausführlichere Fehlerinformationen an den Aufrufer zurückzugeben.

Der Aufruf von [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) gibt [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) an den Aufrufer zurück.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Bereitstellen der Dienstanbieter-App und Abrufen des Paketfamiliennamens

Der APP Service-Anbieter muss bereitgestellt werden, bevor er von einem Client aufgerufen werden kann. Sie können es bereitstellen, indem Sie **>** Projekt Mappe in Visual Studio bereitstellen auswählen.

Sie benötigen auch den Paket Familiennamen des App Service-Anbieters, um ihn aufzurufen. Sie können dies erreichen, indem Sie die Datei " **Package. appxmanifest** " des **appserviceprovider** -Projekts in der Designer Ansicht öffnen (Doppelklicken Sie in der **Projektmappen-Explorer**). Wählen Sie die Registerkarte **Verpacken** aus, kopieren Sie den Wert neben **paketfamilienname**, und fügen Sie ihn wie in Editor ein.

## <a name="write-a-client-to-call-the-app-service"></a>Schreiben eines Clients zum Aufrufen des App-Diensts

1.  Fügen Sie der Projekt Mappe ein neues leeres Windows Universal-App-Projekt mit der **Datei &gt; Add &gt; New Project**hinzu. Wählen Sie im Dialogfeld **Neues Projekt hinzufügen** die Option **installiert > Visual c# > leere app (universelle Windows-APP)** , und nennen Sie Sie " **ClientApp**".

2.  Fügen Sie im **ClientApp** -Projekt am Anfang von **MainPage.XAML.cs**die folgende **using** -Anweisung hinzu:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Fügen Sie " **MainPage. XAML**" ein Textfeld namens " **TextBox** " und eine Schaltfläche hinzu.

4.  Fügen Sie einen Click-Handler für die Schaltfläche mit dem Namen **Button_Click**hinzu, und fügen Sie der Signatur des Schaltflächen Handlers das Schlüsselwort **Async** hinzu.

5. Ersetzen Sie den Stub des Klickhandlers für die Schaltfläche durch den folgenden Code. Vergessen Sie nicht die `inventoryService`-Felddeklaration.
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
               return;
           }
       }

       // Call the service.
       int idx = int.Parse(textBox.Text);
       var message = new ValueSet();
       message.Add("Command", "Item");
       message.Add("ID", idx);
       AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
       string result = "";

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data  that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result = response.Message["Result"] as string;
           }
       }

       message.Clear();
       message.Add("Command", "Price");
       message.Add("ID", idx);
       response = await this.inventoryService.SendMessageAsync(message);

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result += " : Price = " + response.Message["Result"] as string;
           }
       }

       textBox.Text = result;
   }
   ```
    
    Ersetzen Sie den Paket Familiennamen in der Zeile `this.inventoryService.PackageFamilyName = "Replace with the package family name";` durch den Paket Familiennamen des **appserviceprovider** -Projekts, das Sie oben in bereitstellen [der Dienst-App abgerufen haben, und rufen Sie den Paket Familiennamen](#deploy-the-service-app-and-get-the-package-family-name)ab.

    > [!NOTE]
    > Stellen Sie sicher, dass Sie das Zeichenfolgenliteralzeichen einfügen, anstatt es in eine Variable einzufügen. Wenn Sie eine Variable verwenden, funktioniert Sie nicht.

    Der Code richtet zunächst eine Verbindung mit dem App-Dienst ein. Die Verbindung bleibt geöffnet, bis Sie den Löschvorgang durchführt `this.inventoryService` . Der APP Service-Name muss `AppService` mit dem Attribut des Elements, das `Name` Sie der Datei " **Package. appxmanifest** " des **appserviceprovider** -Projekts hinzugefügt haben, identisch sein. In diesem Beispiel lautet er `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Ein [valueset](/uwp/api/Windows.Foundation.Collections.ValueSet) `message` mit dem Namen wird erstellt, um den Befehl anzugeben, den wir an den App Service senden möchten. Der Beispiel-App-Dienst erwartet einen Befehl, der angibt, welche der beiden Aktionen ausgeführt werden soll. Wir erhalten den Index aus dem Textfeld in der Client-App und wenden den Dienst dann mit dem `Item` Befehl an, um die Beschreibung des Elements abzurufen. Anschließend führen wir den-Befehl mit dem `Price` -Befehl aus, um den Preis des Elements abzurufen. Der Text der Schaltfläche wird auf das Ergebnis festgelegt.

    Da [appserviceresponsstatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) nur angibt, ob das Betriebssystem den Aufruf von App Service verbinden konnte, überprüfen wir den `Status` Schlüssel in dem von App Service empfangenen [valueset](/uwp/api/Windows.Foundation.Collections.ValueSet) , um sicherzustellen, dass er die Anforderung erfüllen konnte.

6. Legen Sie das **ClientApp** -Projekt als Startprojekt fest (Klicken Sie mit der rechten Maustaste in der **Projektmappen-Explorer**  >  **als Startprojekt festgelegt**), und führen Sie die Projekt Mappe aus. Geben Sie 1 in das Textfeld ein, und klicken Sie auf die Schaltfläche. Der Dienst sollte „Stuhl: Preis = 88,99“ zurückgeben.

    ![Beispiel-App, die Stuhlpreis = 88,99 anzeigt](images/appserviceclientapp.png)

Wenn der APP Service-Rückruf fehlschlägt, überprüfen Sie Folgendes im **ClientApp** -Projekt:

1.  Vergewissern Sie sich, dass der der Inventar Dienst Verbindung zugewiesene Paket Familienname mit dem Paket Familiennamen der **appserviceprovider** -App übereinstimmt. Weitere Informationen finden Sie in der **Schaltfläche mit der Schaltfläche \_ ** `this.inventoryService.PackageFamilyName = "...";`
2.  Über **prüfen \_ Sie unter Schaltflächen Klick**, ob der APP Service-Name, der der Inventur Dienst Verbindung zugewiesen ist, mit dem App Service-Namen in der Datei " **Package. appxmanifest** " von **appserviceprovider**übereinstimmt. Weitere Informationen finden Sie unter `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Stellen Sie sicher, dass die **appserviceprovider** -App bereitgestellt wurde. (Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Lösung**bereitstellen aus.)

## <a name="debug-the-app-service"></a>Debuggen des App-Diensts

1.  Stellen Sie sicher, dass die Projekt Mappe vor dem Debuggen bereitgestellt wird, da die APP Service-Anbieter-App bereitgestellt werden muss, bevor der Dienst (Klicken Sie in Visual Studio auf **Erstellen &gt; Projektmappe bereitstellen**.)
2.  Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **appserviceprovider** , und wählen Sie **Eigenschaften**aus. Ändern Sie auf der Registerkarte **Debuggen** die **Startaktion** in **Eigenen Code zunächst nicht starten, sondern debuggen**. (Beachten Sie, dass Sie, wenn Sie C++ zum Implementieren Ihres App Service-Anbieters verwendet haben, auf der Registerkarte " **Debuggen** " die **Anwendung starten** in **Nein**ändern.
3.  Legen Sie im Projekt **myappservice** in der Datei **Inventory.cs** einen Haltepunkt in **onrequestempfangenen**fest.
4.  Legen Sie das Projekt **appserviceprovider** als Startprojekt fest, und drücken Sie **F5**.
5.  Starten Sie **ClientApp** über das Startmenü (nicht über Visual Studio).
6.  Geben Sie 1 in das Textfeld ein, und klicken Sie auf die Schaltfläche. Der Debugger stoppt im App-Dienstaufruf am Haltepunkt in Ihrem App-Dienst.

## <a name="debug-the-client"></a>Debuggen des Clients

1.  Befolgen Sie die Anweisungen im vorherigen Schritt, um den Client zu debuggen, der APP Service aufruft.
2.  Starten Sie **ClientApp** über das Startmenü.
3.  Fügen Sie den Debugger an den **ClientApp.exe** Prozess an (nicht an den **ApplicationFrameHost.exe** Prozess). (Klicken Sie in Visual Studio auf **Debuggen &gt; An den Prozess anhängen**.)
4.  Legen Sie im **ClientApp** -Projekt einen Haltepunkt in **Button \_ Click**fest.
5.  Die Haltepunkte im Client und App Service werden nun angezeigt, wenn Sie die Zahl 1 in das Textfeld von **ClientApp** eingeben und auf die Schaltfläche klicken.

## <a name="general-app-service-troubleshooting"></a>Allgemeine Problembehandlung bei App Service

Wenn Sie nach dem Versuch, eine Verbindung mit einem App Service herzustellen, auf den **appunavailable** -Status stoßen, überprüfen Sie Folgendes:

- Stellen Sie sicher, dass das App Service-Anbieter Projekt und das App Service-Projekt bereitgestellt werden Beide müssen vor der Ausführung des Clients bereitgestellt werden, da andernfalls keine Verbindung mit dem Client hergestellt werden muss. Sie können die Bereitstellung über Visual Studio mithilfe von **Build**bereitstellen  >  **Deploy Solution**
- Stellen Sie im **Projektmappen-Explorer**sicher, dass Ihr App Service-Anbieter Projekt über einen Projekt-zu-Projekt-Verweis auf das Projekt verfügt, das den App Service implementiert.
- Überprüfen Sie, ob der `<Extensions>` Eintrag und seine untergeordneten Elemente der Datei **Package. appxmanifest** hinzugefügt wurden, die zum App Service-Anbieter Projekt gehört, wie oben in [Hinzufügen einer APP Service-Erweiterung zu "Package. appxmanifest](#appxmanifest)" angegeben.
- Stellen Sie sicher, dass die [appserviceconnetction. appservicename](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) -Zeichenfolge in Ihrem Client, die den App Service-Anbieter aufruft, mit dem `<uap3:AppService Name="..." />` in der Datei " **Package. appxmanifest** " des App Service-Anbieter Projekts angegebenen übereinstimmt.
- Stellen Sie sicher, dass " [appserviceconnetction. packagefamilyname](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) " mit dem Paket Familiennamen der APP Service-Anbieter Komponente übereinstimmt, wie oben in [Hinzufügen einer APP Service-Erweiterung zu "Package. appxmanifest](#appxmanifest) " angegeben.
- Für Out-of-proc-App-Dienste, wie z. b. die in diesem Beispiel, überprüfen Sie, ob die `EntryPoint` im `<uap:Extension ...>` -Element der Datei " **Package. appxmanifest** " des App Service-Anbieter Projekts angegebene mit dem Namespace-und Klassennamen der öffentlichen Klasse übereinstimmt, die [ibackgroundtask](/uwp/api/windows.applicationmodel.background.ibackgroundtask) in Ihrem App Service-Projekt implementiert.

### <a name="troubleshoot-debugging"></a>Behandeln von Problemen beim Debuggen

Wenn der Debugger an Haltepunkten in Ihrem App Service-Anbieter oder in ihren App Service-Projekten nicht angehalten wird, überprüfen Sie Folgendes:

- Stellen Sie sicher, dass das App Service-Anbieter Projekt und das App Service-Projekt bereitgestellt werden Beide müssen vor der Ausführung des-Clients bereitgestellt werden. Sie können Sie über Visual Studio bereitstellen, indem Sie die Lösung **Build**bereitstellen verwenden  >  **Deploy Solution**.
- Stellen Sie sicher, dass das Projekt, das Sie debuggen möchten, als Startprojekt festgelegt ist und die Debugeigenschaften für dieses Projekt so festgelegt sind, dass das Projekt beim Drücken von **F5** nicht ausgeführt wird Klicken **Sie mit der** rechten Maustaste auf das Projekt, und klicken **Sie dann auf** **Eigenschaften**. Ändern Sie in c# die **Start Aktion** in **nicht starten, sondern Debuggen Sie den eigenen Code, wenn er gestartet wird**. Legen Sie in C++ die Einstellung **Anwendung starten** auf **Nein**fest.

## <a name="remarks"></a>Bemerkungen

Dieses Beispiel enthält eine Einführung in das Erstellen eines App Service, der als Hintergrundaufgabe ausgeführt und aus einer anderen APP aufgerufen wird. Beachten Sie die folgenden wichtigen Punkte:

* Erstellen Sie eine Hintergrundaufgabe zum Hosten des App Service.
* Fügen Sie die `windows.appService` Erweiterung der Datei " **Package. appxmanifest** " des App Service-Anbieters hinzu.
* Rufen Sie den Paket Familiennamen des App Service-Anbieters ab, damit wir über die Client-App eine Verbindung damit herstellen können.
* Fügen Sie dem App Service-Projekt einen Projekt-zu-Projekt-Verweis aus dem App Service-Anbieter Projekt hinzu.
* Verwenden Sie [Windows. applicationmodel. Appservice. appserviceconnetction](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) , um den Dienst aufzurufen.

## <a name="full-code-for-myappservice"></a>Vollständiger Code für „MyAppService”

```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Umwandeln eines App-Diensts für die Ausführung im gleichen Prozess wie die Host-App](convert-app-service-in-process.md)
* [Unterstützen Ihrer App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md)
* [App Service-Codebeispiel (c#, C++ und VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)