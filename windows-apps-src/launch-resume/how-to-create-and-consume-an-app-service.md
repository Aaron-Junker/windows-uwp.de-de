---
title: Erstellen und Verwenden eines App-Diensts
description: Hier erfahren Sie, wie Sie eine App für die Universelle Windows-Plattform (UWP) erstellen, die Dienste für andere UWP-Apps bereitstellen kann, und wie Sie diese Dienste nutzen.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: App zu app Communication, prozessübergreifende Kommunikation IPC, messaging, Kommunikation, app zu app-app Hintergrunddienst Hintergrund
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 96ecad8494ad82dc4927b3675ad322f07a8ca7f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643575"
---
# <a name="create-and-consume-an-app-service"></a>Erstellen und Verwenden eines App-Diensts

App-Dienste sind UWP-Apps, die Dienste für andere UWP-Apps bereitstellen. Sie entsprechen Webdiensten auf einem Gerät. Ein App-Dienst wird als Hintergrundaufgabe in der Host-App ausgeführt und kann seine Dienste auch anderen Apps bereitstellen. Beispielsweise kann der Barcode-Scanner eines App-Dienstes auch anderen Apps nützlich sein. Oder möglicherweise verfügt eine Enterprise-Suite von Apps über einen gemeinschaftlichen App-Dienst für die Rechtschreibprüfung, der auch den anderen Apps in der Suite zur Verfügung steht.  App-Dienste ermöglichen Ihnen, Dienste ohne UI zu erstellen, die von Apps auf demselben Gerät aufgerufen werden können. Ab Windows 10, Version 1607 ist dies auch für Remote-Geräte möglich.

Ab Windows 10, Version 1607, können Sie App-Dienste erstellen, die im gleichen Prozess wie die Vordergrund-App ausgeführt werden. In diesem Artikel geht es um das Erstellen und Verwenden von App-Diensten, die in einem separaten Hintergrundprozess ausgeführt werden. Unter [Konvertieren eines App-Diensts für die Ausführung im gleichen Prozess wie seine Host-App](convert-app-service-in-process.md) finden Sie weitere Informationen zum Ausführen eines App-Dienstes, der im gleichen Prozess wie der Anbieter ausgeführt werden.

Den Code für einen App-Dienst finden Sie unter [Beispiel-Apps für die Universelle Windows-Plattform (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Erstellen eines neuen App-Dienstanbieterprojekts

In dieser Anleitung erstellen wir der Einfachheit halber alles in einer Projektmappe.

1. In Visual Studio 2015 oder höher, erstellen Sie ein neues UWP-app-Projekt, und nennen Sie sie **AppServiceProvider**.
    1. Wählen Sie **Datei > Neu > Projekt...** 
    2. In der **neues Projekt** wählen Sie im Dialogfeld **installiert > Visual C# > leere App (Universelles Windows)**. Dies ist die App, die den App-Dienst für andere UWP-Apps verfügbar macht.
    3. Nennen Sie das Projekt **AppServiceProvider**, Auswählen eines Speicherorts für sie und klicken Sie auf **OK**.

2. Wenn Sie gefragt werden, wählen Sie eine **Ziel** und **Mindestversion** für das Projekt, wählen Sie mindestens **10.0.14393**. Wenn Sie die Verwendung der neuen möchten **SupportsMultipleInstances** -Attribut, Sie müssen Visual Studio 2017 und Ziel verwenden **10.0.15063** (**Windows 10 Creators Update**) oder höher.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Datei "Package.appxmanifest" eine app Service-Erweiterung hinzufügen

In der **AppServiceProvider** -Projekt, öffnen die **"Package.appxmanifest"** -Datei in einem Text-Editor: 

1. Mit der rechten Maustaste in den **Projektmappen-Explorer**. 
2. Wählen Sie **Öffnen mit**. 
3. Wählen Sie **XML (Text)-Editor**. 

Fügen Sie die folgenden `AppService` Erweiterung in die `<Application>` Element. Durch dieses Beispiel wird der `com.microsoft.inventory`-Dienst angekündigt, und die App wird als App-Dienstanbieter identifiziert. Der eigentliche Dienst wird als Hintergrundaufgabe implementiert. Das Projekt, das den App-Dienst bereitstellt, macht den Dienst für andere Apps verfügbar. Wir empfehlen, einen umgekehrten Domänennamen für den Dienstnamen zu verwenden.

Beachten Sie, dass der Namespacepräfix `xmlns:uap4` und das Attribut `uap4:SupportsMultipleInstances` nur dann gültig sind, wenn Sie die Windows SDK-Version 10.0.15063 oder höher als Ziel setzen. Sie können diese sicher entfernen, wenn Sie ältere SDK-Versionen als Zielgruppe setzen.

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

Die `Category` -Attribut identifiziert diese Anwendung als app Service-Anbieter.

Die `EntryPoint` Attribut identifiziert die namespacequalifizierter-Klasse, die den Dienst implementiert, die wir als Nächstes implementieren müssen.

Die `SupportsMultipleInstances` Attribut gibt an, dass jedes Mal, der app-Dienst aufgerufen, die wird, in einem neuen Prozess ausgeführt werden soll. Dies ist nicht erforderlich, jedoch ist für Sie verfügbar, wenn Sie diese Funktion benötigen, und beziehen sich auf die 10.0.15063 SDK (**Windows 10 Creators Update**) oder höher. Der Präfix `uap4` Namespace sollte dabei angegeben werden.

## <a name="create-the-app-service"></a>Erstellen des App-Diensts

1.  Ein App-Dienst kann als Hintergrundaufgabe implementiert werden. Dadurch kann eine Vordergrund-App einen App-Dienst in einer anderen App aufrufen. Zum Erstellen einer app Service als Hintergrundaufgabe fügen Sie ein neues Windows-Runtime-Komponente-Projekt der Projektmappe (**Datei &gt; hinzufügen &gt; neues Projekt**) mit dem Namen **MyAppService**. In der **neues Projekt hinzufügen** Dialogfeld wählen **installiert > Visual C# > Windows-Runtime-Komponente (Universal Windows)**.
2.  In der **AppServiceProvider** fügen einen Projekt-zu-Projekt-Verweis auf das neue **MyAppService** Projekt (in der **Projektmappen-Explorer**, mit der rechten Maustaste auf die  **AppServiceProvider** Projekt > **hinzufügen** > **Verweis** > **Projekte**  >   **Lösung**Option **MyAppService** > **OK**). Dieser Schritt ist unerlässlich, da der App-Dienst ohne das Hinzufügen der Referenz keine Laufzeit damit verbindet.
3.  In der **MyAppService** Projekt, fügen Sie die folgenden **mit** Anweisungen am Anfang **"Class1.cs"**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Benennen Sie **"Class1.cs"** zu **Inventory.cs**, und Ersetzen Sie den Stubcode für **Class1** mit einer neuen Hintergrund-Task-Klasse mit dem Namen **Inventur**:

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

    **Führen Sie** wird aufgerufen, wenn die Hintergrundaufgabe erstellt wird. Da Hintergrundaufgaben nach Abschluss von **Run** beendet werden, implementiert der Code eine Verzögerung, damit die Hintergrundaufgabe zum Verarbeiten von Anforderungen aktiv bleibt. Ein app-Dienst, der als ein Hintergrundtask implementiert wird, verbleibt aktiv für ungefähr 30 Sekunden, nach dem Erhalt eines Aufrufs, es sei denn, sie innerhalb dieses Zeitfensters erneut aufgerufen wird oder eine Verzögerung wird entfernt. Wenn die app Service in demselben Prozess wie der Aufrufer implementiert wird, ist die Lebensdauer des app Service an die Lebensdauer des Aufrufers gebunden.

    Die Lebensdauer des App-Diensts hängt vom Aufrufer ab:

    * Wenn der Aufrufer im Vordergrund ist, entspricht die Lebensdauer des app-Diensts den Aufrufer.
    * Wenn der Aufrufer im Hintergrund ist, ruft der app Service 30 Sekunden ausgeführt. Das Entfernen einer Verzögerung stellt ein Mal zusätzlich 5 Sekunden bereit.

    **OnTaskCanceled** wird aufgerufen, wenn die Aufgabe abgebrochen wird. Die Aufgabe abgebrochen wird, wenn die Client-app löscht die [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704), die Client-app wurde angehalten, das Betriebssystem wird heruntergefahren oder im Ruhezustand ist oder das Betriebssystem nicht mehr genügend Ressourcen zum Ausführen des Tasks.

## <a name="write-the-code-for-the-app-service"></a>Schreiben des Codes für den App-Dienst

**OnRequestReceived** wird der Code für die app Service eingefügt. Ersetzen Sie den stubwebdienst **OnRequestReceived** in **MyAppService**des **Inventory.cs** durch den Code in diesem Beispiel. Dieser Code ruft einen Index für ein Bestandselement ab und übergibt ihn zusammen mit einer Befehlszeichenfolge an den Dienst, um den Namen und Preis des angegebenen Bestandselements abzurufen. Fügen Sie Ihren eigenen Projekten einen Code zur Fehlerbehandlung hinzu.

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

Beachten Sie, dass **OnRequestReceived** ist **Async** darin, dass wir eine awaitable-Methode aufrufen machen [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) in diesem Beispiel.

Eine Verzögerung wird verwendet, damit der Dienst verwenden, kann **Async** Methoden in der **OnRequestReceived** Handler. Dadurch wird sichergestellt, dass der Aufruf von **OnRequestReceived** erst abgeschlossen wird, wenn die Nachricht verarbeitet wurde.  [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) sendet das Ergebnis an den Aufrufer. **SendResponseAsync** signalisiert nicht den Abschluss des Aufrufs. Ist die Verzögerung, die auf signalisiert den Abschluss [SendMessageAsync](https://msdn.microsoft.com/library/windows/apps/dn921712) , **OnRequestReceived** wurde abgeschlossen. Der Aufruf von **SendResponseAsync** in einem Try/finally-Block umschlossen wird, da Sie, auch wenn die Verzögerung ausführen müssen **SendResponseAsync** löst eine Ausnahme aus.

App-Dienste verwenden [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) Objekte, Informationen auszutauschen. Die Größe der Daten, die übergeben werden können, ist nur durch die Systemressourcen begrenzt. Es gibt keine vordefinierten Schlüssel zur Verwendung in Ihrem **ValueSet**. Sie müssen bestimmen, welche Schlüsselwerte Sie zum Definieren des Protokolls für Ihren App-Dienst verwenden. Dieses Protokoll muss beim Schreiben des Aufrufers berücksichtigt werden. In diesem Beispiel haben wir einen Schlüssel namens `Command` ausgewählt, dessen Wert angibt, ob der App-Dienst den Namen des Bestandselements oder seinen Preis bereitstellen soll. Der Index des Bestandsnamens wird unter dem Schlüssel `ID` gespeichert. Der Rückgabewert wird unter dem Schlüssel `Result` gespeichert.

Ein [AppServiceClosedStatus](https://msdn.microsoft.com/library/windows/apps/dn921703) Enumeration wird an den Aufrufer zurückgegeben, um anzugeben, ob der Aufruf von app Service erfolgreich war oder nicht. Beim Aufruf des App-Diensts könnte z. B. ein Fehler auftreten, wenn das Betriebssystem den Dienstendpunkt abbricht, da keine Ressourcen mehr verfügbar sind. Sie können zusätzliche Fehlerinformationen über Zurückgeben der [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131). In diesem Beispiel verwenden wir einen Schlüssel mit dem Namen `Status`, um detailliertere Fehlerinformationen an den Aufrufer zurückzugeben.

Der Aufruf von [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) gibt die [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) an den Aufrufer.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Bereitstellen der Dienstanbieter-App und Abrufen des Paketfamiliennamens

Die app Service-Anbieter muss bereitgestellt werden, bevor es von einem Client aufgerufen werden kann. Sie können es durch Auswahl bereitstellen **erstellen > Projektmappe bereitstellen** in Visual Studio.

Sie benötigen auch den paketfamiliennamen für die app Service-Anbieter, um es aufzurufen. Sie erhalten sie durch Öffnen der **AppServiceProvider** des Projekts **"Package.appxmanifest"** Datei in der Entwurfsansicht (Doppelklicken sie im der **Projektmappen-Explorer**). Wählen Sie die **Paketerstellung** Registerkarte, kopieren Sie den Wert neben **paketfamilienname**, und fügen Sie sie an einer beliebigen Stelle wie Editor jetzt.

## <a name="write-a-client-to-call-the-app-service"></a>Schreiben eines Clients zum Aufrufen des App-Diensts

1.  Fügen Sie der Projektmappe ein neues leeres Projekt für eine Universelle Windows-App mit dem Namen **Datei &gt; Hinzufügen &gt; Neues Projekt** hinzu. In der **neues Projekt hinzufügen** Dialogfeld wählen **installiert > Visual C# > leere App (Universelles Windows)** und nennen Sie sie **ClientApp**.

2.  In der **ClientApp** Projekt, fügen Sie die folgenden **mit** Anweisung am Anfang **"MainPage.Xaml.cs"**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Hinzufügen eines Textfelds namens **Textfeld** und eine Schaltfläche zur **"MainPage.xaml"**.

4.  Eine Schaltfläche "hinzufügen" click-Ereignishandler der Schaltfläche mit dem Namen **Button_Click**, und fügen Sie das Schlüsselwort **Async** des schaltflächenhandlers Signatur.

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
    
    Ersetzen Sie den Paketfamiliennamen in der Zeile `this.inventoryService.PackageFamilyName = "Replace with the package family name";` durch den Paketfamiliennamen des **AppServiceProvider**-Projekts, das Sie in [Bereitstellen der Dienstanbieter-App und Abrufen des Paketfamiliennamens](#deploy-the-service-app-and-get-the-package-family-name) erhalten haben.

    > [!NOTE]
    > Stellen Sie sicher, dass fügen in das Zeichenfolgenliteral, anstatt es in einer Variablen. Es funktioniert nicht, wenn Sie eine Variable verwenden.

    Der Code richtet zunächst eine Verbindung mit dem App-Dienst ein. Die Verbindung bleibt offen, bis Sie `this.inventoryService` verwerfen. Der Name des app Service muss übereinstimmen. die `AppService` des Elements `Name` -Attribut, das Sie hinzugefügt haben die **AppServiceProvider** des Projekts **"Package.appxmanifest"** Datei. In diesem Beispiel ist dies `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Ein [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) mit dem Namen `message` wird erstellt, um den Befehl anzugeben, die an den app-Dienst gesendet werden soll. Der Beispiel-App-Dienst erwartet einen Befehl, der angibt, welche der beiden Aktionen ausgeführt werden soll. Wir den Index aus dem Textfeld in die Client-app, und rufen Sie dann den Dienst mit der `Item` Befehl, um die Beschreibung des Elements zu erhalten. Anschließend wird der Aufruf mit dem Befehl `Price` ausgeführt, um den Preis des Artikels zu erhalten. Der Text der Schaltfläche wird auf das Ergebnis festgelegt.

    Da [AppServiceResponseStatus](https://msdn.microsoft.com/library/windows/apps/dn921724) gibt nur an, ob das Betriebssystem den Aufruf an den app-Dienst herstellen war, überprüfen wir die `Status` -Schlüssel in der [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) wir erhalten, von der app Dienst, um sicherzustellen, dass es zur Verarbeitung der Anforderung konnte.

6. Festlegen der **ClientApp** Projekt das Startprojekt sein (mit der rechten Maustaste in die **Projektmappen-Explorer** > **als Startprojekt festlegen**), und führen Sie die Lösung. Geben Sie 1 in das Textfeld ein, und klicken Sie auf die Schaltfläche. Sollte "Chair: Preis = 88.99" vom Dienst.

    ![Beispiel-App, die Stuhlpreis = 88,99 anzeigt](images/appserviceclientapp.png)

Wenn es sich bei der app Service-Aufruf ein Fehler auftritt, überprüfen Sie Folgendes die **ClientApp** Projekt:

1.  Überprüfen, ob den paketfamiliennamen zugewiesen, die zum Herstellen einer Verbindung Inventory den paketfamiliennamen der entspricht der **AppServiceProvider** app. Finden Sie unter der Zeile im **Schaltfläche\_klicken Sie auf** mit `this.inventoryService.PackageFamilyName = "...";`.
2.  In **Schaltfläche\_klicken Sie auf**, stellen Sie sicher, dass der Name des app Service, der zum Herstellen einer Verbindung Inventory zugewiesen ist der Name des app Service in entspricht der **AppServiceProvider**des  **Datei "Package.appxmanifest"** Datei. Weitere Informationen finden Sie unter `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Sicherstellen, dass die **AppServiceProvider** app bereitgestellt wurde. (In der **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und wählen Sie **Projektmappe bereitstellen**).

## <a name="debug-the-app-service"></a>Debuggen des App-Diensts

1.  Stellen Sie vor dem Debuggen sicher, dass die Projektmappe bereitgestellt wurde. Die App Dienstanbieter-App muss bereitgestellt werden, bevor der Dienst aufgerufen werden kann. (Klicken Sie in Visual Studio auf **Erstellen &gt; Projektmappe bereitstellen**.)
2.  In der **Projektmappen-Explorer**, mit der rechten Maustaste die **AppServiceProvider** Projekt, und wählen **Eigenschaften**. Ändern Sie auf der Registerkarte **Debuggen** die **Startaktion** in **Eigenen Code zunächst nicht starten, sondern debuggen**. (Hinweis: wenn Sie nicht C++ zum Implementieren Ihres App-Dienstanbieters verwenden, müssen Sie auf der Registerkarte **Debuggen** die Option **Anwendung starten** auf **Nein** ändern).
3.  In der **MyAppService** in-Projekt die **Inventory.cs** Datei, das Festlegen eines Haltepunkts im **OnRequestReceived**.
4.  Legen Sie die **AppServiceProvider** Projekt das Startprojekt und drücken Sie **F5**.
5.  Starten Sie **ClientApp** über das Menü "Start" (nicht von Visual Studio).
6.  Geben Sie 1 in das Textfeld ein, und klicken Sie auf die Schaltfläche. Der Debugger stoppt im App-Dienstaufruf am Haltepunkt in Ihrem App-Dienst.

## <a name="debug-the-client"></a>Debuggen des Clients

1.  Folgen Sie zum Debuggen des Clients, der den App-Dienst aufruft, den Anweisungen im vorherigen Schritt.
2.  Starten Sie **ClientApp** über das Startmenü.
3.  Fügen Sie den Debugger an den **ClientApp.exe** Prozess (nicht die **ApplicationFrameHost.exe** Prozess). (Klicken Sie in Visual Studio auf **Debuggen &gt; An den Prozess anhängen**.)
4.  In der **ClientApp** Projekt, das Festlegen eines Haltepunkts im **Schaltfläche\_klicken Sie auf**.
5.  Die Haltepunkte in dem Client und den app Service werden nun erreicht wird, wenn Sie die Zahl 1 in das Textfeld der eingeben **ClientApp** , und klicken Sie auf die Schaltfläche.

## <a name="general-app-service-troubleshooting"></a>Allgemeine Problembehandlung von App-Diensten

Wenn beim Auftreten einer **AppUnavailable** Status nach dem Verbindung mit einer app Service, überprüfen Sie Folgendes:

- Stellen Sie sicher, dass das App-Dienstanbieterprojekt und das App-Dienstprojekt bereitgestellt sind. Beide müssen bereitgestellt werden, bevor der Client ausgeführt werden kann, da andernfalls der Client keine Verbindung zulassen kann. Sie können von Visual Studio aus mithilfe von **Erstellen** > **Projektmappe bereitstellen** bereitstellen.
- In der **Projektmappen-Explorer**, stellen Sie sicher, dass Ihr app Service-Anbieter-Projekt einen Projekt-zu-Projekt-Verweis auf das Projekt hat, die die app-Dienst implementiert.
- Überprüfen Sie, ob die `<Extensions>` Eintrag, und seine untergeordneten Elemente wurden hinzugefügt die **"Package.appxmanifest"** Datei gehört, für das app Service-Anbieter-Projekt als oben angegeben, in [fügen Sie eine app Service-Erweiterung für Datei "Package.appxmanifest"](#appxmanifest).
- Sicherstellen, dass die [AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) Zeichenfolge in Ihrem Client, die den app Service-Anbieter aufruft, entspricht die `<uap3:AppService Name="..." />` in der app Service-Anbieter des Projekts angegeben **"Package.appxmanifest"**  Datei.
- Sicherstellen, dass die [AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) entspricht den paketfamiliennamen der app Service-Anbieter-Komponente wie oben angegeben, in ["Package.appxmanifest" eine app Service-Erweiterung hinzufügen](#appxmanifest)
- Für die Out-of-Proc-app-Dienste wie z. B. die in diesem Beispiel, zu überprüfen, die die `EntryPoint` Angabe in der `<uap:Extension ...>` Element des app Service-Anbieter des Projekts **"Package.appxmanifest"** Datei übereinstimmt, den Namespace und Namen der öffentlichen Klasse, die implementiert [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) im app Service-Projekt.

### <a name="troubleshoot-debugging"></a>Problembehandlung beim Debuggen

Wenn der Debugger nicht an Haltepunkten in Ihrem App-Dienstanbieter oder App-Dienst-Projekte beendet wird, überprüfen Sie Folgendes:

- Stellen Sie sicher, dass das App-Dienstanbieterprojekt und das App-Dienstprojekt bereitgestellt sind. Beide müssen bereitgestellt werden, bevor der Client ausgeführt wird. Sie können diese von Visual Studio aus mithilfe von **Erstellen** > **Projektmappe bereitstellen** bereitstellen.
- Stellen Sie sicher, dass das zu debuggende Projekt als Startprojekt festgelegt ist und die Debugeigenschaften für das Projekt festgelegt sind, nicht ausgeführt. das Projekt beim **F5** gedrückt wird. Klicken Sie mit der rechten Maustaste auf **Eigenschaften** und dann **Debug** (oder **Debugging** in C++). In C#, ändern Sie die **Startaktion** in **Eigenen Code zunächst nicht starten, sondern debuggen**. Legen Sie in C++ **Anwendung starten** auf **Nein** fest.

## <a name="remarks"></a>Hinweise

Dieses Beispiel ist eine Einführung in das Erstellen eines App-Diensts, das als Hintergrundaufgabe ausgeführt wird, und Aufrufen des Diensts in einer anderen App. Die wichtigsten Punkte zu beachten sind:

* Erstellen Sie ein Hintergrundtask zum Hosten des app-Diensts.
* Hinzufügen der `windows.appService` Erweiterung des app Service-Anbieters **"Package.appxmanifest"** Datei.
* Erhalten Sie den paketfamiliennamen für die app Service-Anbieter, so dass wir über die Client-app hergestellt werden kann.
* Das app Service-Projekt einen Projekt-zu-Projekt-Verweis aus dem app Service-Anbieter-Projekt hinzugefügt.
* Verwendung [Windows.ApplicationModel.AppService.AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704) zum Aufrufen des Diensts.

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

## <a name="related-topics"></a>Verwandte Themen

* [Umwandeln eines App-Diensts für die Ausführung im gleichen Prozess wie die Host-App](convert-app-service-in-process.md)
* [Unterstützen Ihrer App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md)
* [App Service-Codebeispiel (C#, C++ und VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
