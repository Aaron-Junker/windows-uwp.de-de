---
title: Erstellen und Hosten einer App-Erweiterung
description: Schreiben und Hosten Sie die App-Erweiterungen der universellen Windows-Plattform (UWP), mit denen Sie Ihre App über Pakete erweitern können, die Benutzer aus dem Microsoft Store installieren können.
keywords: App-Erweiterung, App-Dienst, Hintergrund
ms.date: 10/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2cfb3be556cb681bc9ed2d9d46bb86304182e5ca
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67821020"
---
# <a name="create-and-host-an-app-extension"></a>Erstellen und Hosten einer App-Erweiterung

In diesem Artikel wird erläutert, wie Sie eine UWP-App-Erweiterung erstellen und in einer UWP-App hosten.

Dieser Artikel wird zusammen mit einem Codebeispiel angezeigt:
- Laden Sie [Math Extension-Codebeispiel](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip)herunter und entzippen Sie die Dateien.
- Öffnen Sie in Visual Studio-2019 MathExtensionSample.sln ein. Legen Sie den Buildtyp auf x86 fest (**Build** > **Konfigurationsmanager**, ändern Sie dann **Plattform** auf **x86** für beide Projekte).
- Bereitstellen der Lösung an: **Erstellen Sie** > **Lösung bereitstellen**.

## <a name="introduction-to-app-extensions"></a>Einführung in App-Erweiterungen

In einer App für die Universelle Windows-Plattform (UWP) stellen Erweiterungen Funktionen bereit, die mit Plug-Ins, Add-Ins und Add-Ons auf anderen Plattformen vergleichbar sind. Microsoft Edge-Erweiterungen sind beispielsweise UWP-App-Erweiterungen. UWP-App-Erweiterungen wurden in der Windows 10 Anniversary-Edition (Version 1607, Build 10.0.14393) eingeführt.

UWP-App-Erweiterungen sind UWP-Apps mit einer Erweiterungsdeklaration, die ihnen ermöglicht, Inhalte und Bereitstellungsereignisse mit einer Host-App zu teilen. Eine App-Erweiterung stellt mehrere Erweiterungen bereit.

Da App-Erweiterungen UWP-Apps sind, sind sie auch voll funktionsfähige Apps, Host-Erweiterungen und bieten Erweiterungen für andere Apps an – ohne dabei separate App-Pakete zu erstellen.

Wenn Sie einen App-Erweiterungshost erstellen, bieten Sie die Möglichkeit, ein Ökosystem in Ihrer App zu entwickeln, in dem anderen Entwickler Ihre App auf für Sie möglicherweise unerwartet Weise oder mit unerwarteten Ressourcen verbessern können. Sollten Sie Microsoft Office-Erweiterungen, Visual Studio-Erweiterungen, Browsererweiterungen usw. ein. Umfangreichere Funktionen für diese apps, die die Funktionalität hinausgehen, die, denen Sie im Lieferumfang, erstellen. Erweiterungen können der App mehr Wert und eine längere Lebensdauer bieten.

**Übersicht**

Allgemein gesagt, muss zum Einrichten einer App-Erweiterungsbeziehung folgendes ausgeführt werden:

1. Die App wird als Erweiterungshost deklariert.
2. Die App wird als Erweiterung deklariert.
3. Es wird entschieden, ob die Erweiterung als App-Dienst, Hintergrundaufgabe oder anderweitig implementiert wird.
4. Es wird festgelegt, wie die Hosts und die Erweiterungen kommunizieren.
5. Verwenden Sie die [Windows.ApplicationModel.AppExtensions](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppExtensions)-API in der Host-App, um auf die Erweiterungen zuzugreifen.

Sehen wir uns diese Vorgehensweise durch Untersuchen des [Math Extension-Codebeispiels](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip) an, das einen hypothetischen Rechner implementiert, für den Sie mithilfe der Erweiterungen neue Funktionen hinzufügen können. Laden Sie in Microsoft Visual Studio-2019, **MathExtensionSample.sln** gegenüber dem Codebeispiel.

![Math Extension-Codebeispiel](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>Deklarieren einer App als Erweiterungshost

Eine App identifiziert sich selbst durch das Deklarieren des `<AppExtensionHost>`-Elements in der Datei "Package.appxmanifest" als App-Erweiterungshost. Weitere Informationen finden Sie in der **Package.appxmanifest**-Datei im **MathExtensionHost**-Projekts, um zu sehen wie es funktioniert.

_Datei "Package.appxmanifest" im Projekt MathExtensionHost_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
            <uap3:Extension Category="windows.appExtensionHost">
                <uap3:AppExtensionHost>
                  <uap3:Name>com.microsoft.mathext</uap3:Name>
                </uap3:AppExtensionHost>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

Beachten Sie `xmlns:uap3="http://..."` und `uap3` in `IgnorableNamespaces`. Diese sind erforderlich, da wir den uap3-Namespace verwenden.

`<uap3:Extension Category="windows.appExtensionHost">` Diese app identifiziert als erweiterungshost für eine.

Das Element **Name** im `<uap3:AppExtensionHost>` ist der _Erweiterungsvertrags_name. Wenn eine Erweiterung den gleichen Erweiterungsvertragsnamen angibt, kann der Host diese finden. Üblicherweise empfehlen wir, den Namen der App oder den Namen des Herausgebers als Erweiterungsvertragsnamen zu verwenden, um potenzielle Konflikte mit anderen Erweiterungsvertragsnamen zu vermeiden.

Sie können mehrere Hosts und mehrere Erweiterungen in derselben App definieren. In diesem Beispiel deklarieren wir einen Host. Die Erweiterung wird in einer anderen App definiert.

## <a name="declare-an-app-to-be-an-extension"></a>Deklarieren einer App als Erweiterung

Eine App identifiziert sich selbst durch das Deklarieren des `<uap3:AppExtension>`-Elements in der Datei **Package.appxmanifest** als App-Erweiterung. Öffnen Sie die **Package.appxmanifest**-Datei im **MathExtension**-Projekt, um zu sehen wie es funktioniert.

_"Package.appxmanifest" im Projekt MathExtension:_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
          ...
          <uap3:Extension Category="windows.appExtension">
            <uap3:AppExtension Name="com.microsoft.mathext"
                               Id="power"
                               DisplayName="x^y"
                               Description="Exponent"
                               PublicFolder="Public">
              <uap3:Properties>
                <Service>com.microsoft.powservice</Service>
              </uap3:Properties>
              </uap3:AppExtension>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

Beachten Sie erneut die Zeile `xmlns:uap3="http://..."` und `uap3` in `IgnorableNamespaces`. Diese sind erforderlich, weil wir den `uap3`-Namespace verwenden.

`<uap3:Extension Category="windows.appExtension">` Diese app identifiziert als eine Erweiterung.

Die `<uap3:AppExtension>`-Attribute haben folgende Bedeutungen:

|Attribut|Beschreibung|Erforderlich|
|---------|-----------|:------:|
|**Name**|Dies ist der Erweiterungsvertragsname. Wenn sie dem auf einem Host deklarierten **Namen** entspricht, kann dieser Host diese Erweiterung finden.| :heavy_check_mark: |
|**ID**| Identifiziert diese Erweiterung eindeutig. Da es möglicherweise mehrere Erweiterungen mit dem gleichen Erweiterungsvertragsnamen gibt (wie eine Paint-App, die mehrere Erweiterungen unterstützt), können Sie die ID verwenden, um sie auseinanderzuhalten. App-Erweiterungshosts können die ID verwenden, um den Typ der Erweiterung zu überprüfen. Beispielsweise können Sie eine Erweiterung für den Desktop und eine weitere für Mobilgeräte haben, wobei die ID das Unterscheidungsmerkmal ist. Sie können ebenfalls dafür das Element **Eigenschaften** verwenden.| :heavy_check_mark: |
|**DisplayName**| Kann von der Host-App verwendet werden, um die Erweiterung für den Benutzer zu identifizieren. Ist abfragbar vom und verwendet das [Neue Ressourcenverwaltungssystem](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) für die Lokalisierung. Der lokalisierte Inhalt wird vom App-Erweiterungspaket und nicht von der Host-App geladen. | |
|**Beschreibung** | Kann von der Host-App verwendet werden, um die Erweiterung für den Benutzer zu beschreiben. Ist abfragbar vom und verwendet das [Neue Ressourcenverwaltungssystem](https://docs.microsoft.com/windows/uwp/app-resources/using-mrt-for-converted-desktop-apps-and-games) (`ms-resource:TokenName`) für die Lokalisierung. Der lokalisierte Inhalt wird vom App-Erweiterungspaket und nicht von der Host-App geladen. | |
|**PublicFolder**|Der Name eines Ordners, relativ zum Stammverzeichnis des Pakets, in dem Sie Inhalte mit dem Erweiterungshost teilen können. Üblicherweise ist der Name "Public", Sie können allerdings einen beliebigen Namen verwenden, der einem Ordner in der Erweiterung entspricht.| :heavy_check_mark: |

`<uap3:Properties>` ist ein optionales Element, das benutzerdefinierte Metadaten enthält, den Hosts zur Laufzeit gelesen werden können. Im Codebeispiel ist die Erweiterung als App-Dienst implementiert, damit der Host den Namen des App-Diensts abrufen kann und ihn damit aufrufen kann. Der Name des App-Diensts ist im <Service>-Element definiert, das wir definiert haben (der Name spielt dabei keine Rolle). Der Host im Codebeispiel sucht zur Laufzeit nach dieser Eigenschaft, um den Namen des App-Diensts zu erfahren.

## <a name="decide-how-you-will-implement-the-extension"></a>Entscheiden Sie, wie Sie die Erweiterung implementiert möchten.

In [Build 2016-Sitzung zu App-Erweiterungen](https://channel9.msdn.com/Events/Build/2016/B808) wird veranschaulicht, wie der öffentliche Ordner verwendet wird, der zwischen dem Host und den Erweiterungen freigegeben wurde. In diesem Beispiel wird die Erweiterung von einer Javascript-Datei implementiert, die im öffentlichen Ordner gespeichert ist, der vom Host aufgerufen wird. Diese Methode hat den Vorteil, leichter zu sein und erfordert keine Kompilierung. Sie unterstützt eventuell das Erstellen der Standardstartseite, die Anweisungen für die Erweiterung und einen Link zur Microsoft Store-Seite der Host-App bietet. Weitere Details finden Sie im [Build 2016-App-Erweiterung – Codebeispiel](https://github.com/Microsoft/App-Extensibility-Sample). Genauer gesagt finden Sie die Informationen im **InvertImageExtension**-Projekt und `InvokeLoad()` in ExtensionManager.cs im **ExtensibilitySample**-Projekt.

In diesem Beispiel wird ein App-Dienst verwendet, um die Erweiterung zu implementieren. App-Dienste bieten folgende Vorteile:

- Wenn die Erweiterung abstürzt, wirkt sich dies nicht auf die Host-App aus, da die Host-App in einem eigenen Prozess ausgeführt wird.
- Sie können die Sprache Ihrer Wahl verwenden, um den Dienst zu implementieren. Es muss nicht die gleiche Sprache sein, die zum Implementieren der Host-App verwendet wurde.
- Der App-Dienst hat Zugriff auf seinen eigenen App-Container – der unterschiedliche Funktionen als der Host haben kann.
- Zwischen den Daten im Dienst und der Host-App besteht eine Isolierung.

### <a name="host-app-service-code"></a>Host-App-Dienstcode

Hier ist der Hostcode, der den App-Dienst der Erweiterung aufruft:

_ExtensionManager.cs im Projekt MathExtensionHost_
```cs
public async Task<double> Invoke(ValueSet message)
{
    if (Loaded)
    {
        try
        {
            // make the app service call
            using (var connection = new AppServiceConnection())
            {
                // service name is defined in appxmanifest properties
                connection.AppServiceName = _serviceName;
                // package Family Name is provided by the extension
                connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;

                // open the app service connection
                AppServiceConnectionStatus status = await connection.OpenAsync();
                if (status != AppServiceConnectionStatus.Success)
                {
                    Debug.WriteLine("Failed App Service Connection");
                }
                else
                {
                    // Call the app service
                    AppServiceResponse response = await connection.SendMessageAsync(message);
                    if (response.Status == AppServiceResponseStatus.Success)
                    {
                        ValueSet answer = response.Message as ValueSet;
                        if (answer.ContainsKey("Result")) // When our app service returns "Result", it means it succeeded
                        {
                            return (double)answer["Result"];
                        }
                    }
                }
            }
        }
        catch (Exception)
        {
             Debug.WriteLine("Calling the App Service failed");
        }
    }
    return double.NaN; // indicates an error from the app service
}
```

Dies ist der übliche Code zum Aufrufen eines App-Diensts. Weitere Informationen zum Implementieren und Aufrufen eines App-Diensts finden Sie unter [Erstellen und Nutzen eines App-Diensts](how-to-create-and-consume-an-app-service.md).

Beachten Sie, wie der Name des aufzurufenden App-Diensts bestimmt wird. Da der Host keine Informationen über die Implementierung der Erweiterung besitzt, muss die Erweiterung den Namen des App-Diensts angeben. Im Codebeispiel deklariert die Erweiterung den Namen des App-Diensts in ihrer Datei im `<uap3:Properties>`-Element:

_Datei "Package.appxmanifest" im Projekt MathExtension_
```xml
    ...
    <uap3:Extension Category="windows.appExtension">
      <uap3:AppExtension ...>
        <uap3:Properties>
          <Service>com.microsoft.powservice</Service>
        </uap3:Properties>
        </uap3:AppExtension>
    </uap3:Extension>
```

Definieren Sie Ihren eigenen XML-Code im `<uap3:Properties>`-Element. In diesem Fall definieren wir den Namen des App-Diensts, damit der Host diesen beim Aufruf der Erweiterung verwendet.

Wenn der Host eine Erweiterung lädt, extrahiert ein solcher Code den Namen des Diensts aus den Eigenschaften, die in der Erweiterung "Package.appxmanifest" definiert wurden:

_`Update()` in ExtensionManager.cs, im Projekt MathExtensionHost_
```cs
...
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;

...
#region Update Properties
// update app service information
_serviceName = null;
if (_properties != null)
{
   if (_properties.ContainsKey("Service"))
   {
       PropertySet serviceProperty = _properties["Service"] as PropertySet;
       this._serviceName = serviceProperty["#text"].ToString();
   }
}
#endregion
```

Dank dem Namen des App-Diensts, der in `_serviceName` gespeichert ist, kann der Host diesen zum Aufrufen des App-Diensts verwenden.

Das Aufrufen von App-Diensten erfordert auch den Familiennamen des Pakets, das den App-Dienst enthält. Glücklicherweise bietet die API-app-Erweiterung diese Informationen, die in der Zeile abgerufen wird: `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>Festlegen, wie der Host und die Erweiterung kommunizieren

App-Dienste verwenden [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) zum Austauschen von Informationen. Als Autor des Hosts müssen Sie ein Protokoll für die Kommunikation mit Erweiterungen anbieten, das flexibel ist. Im Codebeispiel bedeutet dies, Erweiterungen zu berücksichtigen, die in Zukunft 1, 2 oder mehr Argumente annehmen können.

In diesem Beispiel ist das Protokoll für die Argumente ein **ValueSet**, das die Schlüssel-Wert-Paare mit dem Namen "Argument" und der Anzahl der Argumente enthält, z. B. `Arg1` und `Arg2`. Der Host übergibt alle Argumente im **ValueSet**, und die Erweiterung verwendet diejenigen, die es benötigt. Wenn die Erweiterung das Ergebnis berechnen kann, erwarte der Host, dass das on der Erweiterung zurückgegebene **ValueSet** einen Schlüssel namens `Result` mit dem Wert der Berechnung enthält. Wenn dieser Schlüssel nicht vorhanden ist, geht der Host davon aus, dass die Erweiterung die Berechnung nicht abschließen konnte.

### <a name="extension-app-service-code"></a>App-Dienstcode der Erweiterung

Im Codebeispiel wird der App-Dienst der Erweiterung nicht als Hintergrundaufgabe implementiert. Stattdessen wird das einzelne Proc App-Service-Modell verwendet, in dem der App-Dienst im gleichen Prozess wie die Erweiterungs-App ausgeführt wird, die ihn hostet. Dieser Prozess unterscheidet sich immer noch von der Host-App und bietet die Vorteile einer Trennung der Prozesse, während durch das Vermeiden prozessübergreifender Kommunikation zwischen dem Erweiterungs- und dem Hintergrundprozess, der den App-Dienst implementiert, Leistungsvorteile entstehen. Unter [Umwandeln eines App-Diensts für die Ausführung im gleichen Prozess wie die Host-App](convert-app-service-in-process.md) sehen Sie den Unterschied zwischen einem App-Dienst, der als Hintergrundaufgabe anstatt in demselben Prozess ausgeführt wird.

Das System stellt `OnBackgroundActivate()` bereit, wenn der App-Dienst aktiviert ist. Dieser Code übernimmt die Ereignishandler zum Behandeln des eigentlichen App-Dienstaufrufs, wenn dieser eingeht (`OnAppServiceRequestReceived()`), und geht mit Wartungsaufgaben-Ereignissen um, wie mit Verzögerungsobjekten für das Behandeln oder Schließen eines Ereignisses.

_"App.Xaml.cs" im Projekt MathExtension._
```cs
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    base.OnBackgroundActivated(args);

    if ( _appServiceInitialized == false ) // Only need to setup the handlers once
    {
        _appServiceInitialized = true;

        IBackgroundTaskInstance taskInstance = args.TaskInstance;
        taskInstance.Canceled += OnAppServicesCanceled;

        AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        _appServiceDeferral = taskInstance.GetDeferral();
        _appServiceConnection = appService.AppServiceConnection;
        _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
        _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
    }
}
```

Der Code, der die Arbeit der Erweiterung durchführt befindet sich in `OnAppServiceRequestReceived()`. Diese Funktion wird aufgerufen, wenn der App-Dienst zum Durchführen einer Berechnung aufgerufen wird. Sie extrahiert die erforderlichen Werte aus dem **ValueSet**. Wenn die Berechnung durchgeführt werden kann, wird das Ergebnis unter einem Schlüssel mit dem Namen **Ergebnis**im **ValueSet** an den Host zurückgegeben. Denken Sie daran, dass entsprechend der im Protokoll definierten Kommunikation zwischen diesem Host und seinen Erweiterungen das Vorhandensein eines **Ergebnis**-Schlüssels den Erfolg oder Misserfolg angibt.

_"App.Xaml.cs" im Projekt MathExtension._
```cs
private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below (SendResponseAsync()) to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    AppServiceDeferral messageDeferral = args.GetDeferral();
    ValueSet message = args.Request.Message;
    ValueSet returnMessage = new ValueSet();

    double? arg1 = Convert.ToDouble(message["arg1"]);
    double? arg2 = Convert.ToDouble(message["arg2"]);
    if (arg1.HasValue && arg2.HasValue)
    {
        returnMessage.Add("Result", Math.Pow(arg1.Value, arg2.Value)); // For this sample, the presence of a "Result" key will mean the call succeeded
    }

    await args.Request.SendResponseAsync(returnMessage);
    messageDeferral.Complete();
}
```

## <a name="manage-extensions"></a>Verwalten von Erweiterungen

Nachdem wir nun gesehen haben, wie die Beziehung zwischen einem Host und den dazugehörigen Erweiterungen implementier wird, wird als Nächstes beschrieben, wie der Host auf dem System installierte Erweiterungen findet, darauf reagiert und wie Pakete mit Erweiterungen entfernt werden.

Im Microsoft Store werden Erweiterungen als Pakete angeboten. Der [AppExtensionCatalog](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) sucht nach installierten Paketen, deren Erweiterungen dem Erweiterungsvertragsnamen des Hosts entsprechen und bietet Ereignisse, die ausgelöst werden, wenn ein relevantes App-Erweiterungspaket für den Host installiert oder entfernt wird.

Im Codebeispiel umschließt die `ExtensionManager`-Klasse (unter **ExtensionManager.cs** im **MathExtensionHost**-Projekt definiert) die Logik des Ladens von Erweiterungen und der Reaktion auf die Installation und Deinstallation von Erweiterungspaketen.

Der `ExtensionManager`-Konstruktor nutzt die `AppExtensionCatalog`, um die App-Erweiterungen auf dem System zu suchen, die den gleichen Erweiterungsvertragsnamen wie der Host haben:

_ExtensionManager.cs im MathExtensionHost-Projekt._
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

Wenn ein Erweiterungspaket installiert ist, sammelt der `ExtensionManager` Informationen über die Erweiterungen im Paket, die den gleichen Erweiterungsvertragsnamen wie der Host haben. Eine Installation kann ein Update sein, wodurch möglicherweise die betroffenen Erweiterungsinformationen aktualisiert werden. Bei der Deinstallation eines Erweiterungspakets entfernt der `ExtensionManager` Informationen zu den betroffenen Erweiterungen, damit der Benutzer weiß, welche Erweiterungen nicht mehr verfügbar sind.

Die `Extension`-Klasse (im **ExtensionManager.cs** des **MathExtensionHost**-Projekts definiert) wurde für das Codebeispiel erstellt, um die ID, Beschreibung, das Logo und die App-spezifischen Informationen der Erweiterung zu beschreiben wie z. B., ob der Benutzer die Erweiterung aktiviert hat.

Wenn die Erweiterung geladen wird (siehe `Load()` im **ExtensionManager.cs**), bedeutet dies, dass der Paketstatus in Ordnung ist und die ID, das Logo, die Beschreibung und öffentlichen Ordner erhalten wurden (die wir nicht in diesem Beispiel verwenden, hier wird nur gezeigt nur, wie Sie diese Informationen erhalten). Das Erweiterungspaket selbst wird nicht geladen.

Das Konzept des Entladens dient dazu, die Erweiterungen zu überprüfen, die für den Benutzer nicht mehr angezeigt werden sollen.

Der `ExtensionManager` bietet eine Sammlung von `Extension`-Instanzen, damit die Erweiterungen, deren Namen, Beschreibungen und Logos Datenbindungen zur Benutzeroberfläche enthält. Die Seite **ExtensionsTab** ist an diese Sammlung gebunden und bietet eine Benutzeroberfläche zum Aktivieren/Deaktivieren und Entfernen von Erweiterungen.

![Beispiel der Registerkarte "Erweiterungen" für eine Benutzeroberfläche](images/mathextensionhost-extensiontab.png)

 Wenn eine Erweiterung entfernt wird, fordert das System den Benutzer auf, zu überprüfen, ob das Paket deinstalliert werden soll, das die Erweiterung enthält (und möglicherweise andere Erweiterungen). Wenn der Benutzer dies bestätigt, wird das Paket deinstalliert und der `ExtensionManager` entfernt die Erweiterungen aus dem deinstallierten Paket aus der Liste der Erweiterungen, die in der Host-App zur Verfügung stehen.

 ![Deinstallieren der Benutzeroberfläche](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>Debuggen von App-Erweiterungen und Hosts

Oftmals sind der Erweiterungshost und die Erweiterung nicht Teil der gleichen Lösung. In diesem Fall können Sie den Host und die Erweiterung folgendermaßen debuggen:

1. Laden Sie Ihr Hostprojekt in eine Instanz von Visual Studio.
2. Laden Sie die Erweiterung in eine andere Instanz von Visual Studio.
3. Starten Sie die Host-App im Debugger.
4. Starten Sie die Erweiterungs-App im Debugger. (Wenn Sie die Erweiterung bereitstellen möchten anstatt sie zu debuggen, gehen Sie folgendermaßen vor, um das Installationsereignis des Host-Pakets zu testen: **Build &gt; Lösung bereitstellen**).

Sie können jetzt Haltepunkte auf dem Host und in der Erweiterung treffen.
Wenn Sie die Erweiterungs-App selbst debuggen, wird ein leeres Fenster für die App angezeigt. Wenn kein leeres Fenster angezeigt werden sollen, können Sie die Debugeinstellungen für das Erweiterungsprojekt ändern, um die App nicht zu starten, sondern sie stattdessen beim Start zu debuggen (klicken Sie mit der rechten Maustaste auf das Erweiterungsprojekt **Eigenschaften** > **Debuggen** > wählen Sie **Eigenen Code zunächst nicht starten, sondern debuggen**). Sie müssen das Erweiterungsprojekt trotzdem debuggen (**F5**). Dies wartet allerdings so lange, bis der Host die Erweiterung aktiviert. Danach wird der Haltepunkt getroffen.

**Debuggen Sie das Codebeispiel**

Im Codebeispiel befinden sich der Host und die Erweiterung in der gleichen Lösung. So gehen Sie zum Debuggen vor:

1. Stellen Sie sicher, dass **MathExtensionHost** das Startprojekt ist (klicken Sie mit der rechten Maustaste auf das **MathExtensionHost**-Projekt und dann auf **Als Startprojekt festlegen**).
2. Setzen Sie einen Haltepunkt auf `Invoke` im ExtensionManager.cs im **MathExtensionHost**-Projekt.
3. Drücken Sie **F5**, um das **MathExtensionHost**-Projekt auszuführen.
4. Setzen Sie einen Haltepunkt auf `OnAppServiceRequestReceived` in App.xaml.cs im **MathExtension**-Projekt.
5. Starten Sie mit dem Debuggen des **MathExtension**-Projekts (klicken Sie mit der rechten Maustaste auf das **MathExtension**-Projekt **Debuggen > Neue Instanz starten**). Dadurch wird es bereitgestellt und das Installationsereignis des Pakets auf dem Host ausgelöst.
6. In der **MathExtensionHost**-App, wechseln Sie zur Seite **Berechnung**, und klicken Sie auf **x^y**, um die Erweiterung zu aktivieren. Der `Invoke()`-Haltepunkt wird zuerst getroffen und Sie sehen, dass der App-Dienstaufruf der Erweiterungen durchgeführt wird. Danach wird die `OnAppServiceRequestReceived()`-Methode in der Erweiterung erreicht, und Sie sehen, wie der App-Diensts das Ergebnis berechnet und zurückgibt.

**Problembehandlung bei Erweiterungen, die implementiert werden, als app service**

Wenn Ihre Erweiterungshost Probleme beim Herstellen einer Verbindung mit dem App-Dienst für die Erweiterung hat, stellen Sie sicher, dass das `<uap:AppService Name="...">`-Attribut Ihrer Eingabe des `<Service>`-Elements entspricht. Wenn diese nicht übereinstimmen, wird der Dienstname, den Ihre Erweiterung an den Host bereitgestellt nicht mit dem implementierten Namen des App-Dienstnamens übereinstimmen, und der Hosts kann Ihre Erweiterung nicht aktivieren.

_"Package.appxmanifest" im Projekt MathExtension:_
```xml
<Extensions>
   <uap:Extension Category="windows.appService">
     <uap:AppService Name="com.microsoft.sqrtservice" />      <!-- This must match the contents of <Service>...</Service> -->
   </uap:Extension>
   <uap3:Extension Category="windows.appExtension">
     <uap3:AppExtension Name="com.microsoft.mathext" Id="sqrt" DisplayName="Sqrt(x)" Description="Square root" PublicFolder="Public">
       <uap3:Properties>
         <Service>com.microsoft.powservice</Service>   <!-- this must match <uap:AppService Name=...> -->
       </uap3:Properties>
     </uap3:AppExtension>
   </uap3:Extension>
</Extensions>   
```

## <a name="a-checklist-of-basic-scenarios-to-test"></a>Hier ist eine Prüfliste, um einfache Szenarien zu testen

Wenn Sie nach der Entwicklung eines Erweiterungshosts diesen testen möchten, um zu sehen, wie gut die Erweiterungen unterstützt werden, finden Sie hier einige grundlegende Testszenarien:

- Führen Sie den Host aus, und stellen Sie dann eine Erweiterungs-App bereit  
    - Übernimmt der Host neue Erweiterungen, die während der Ausführung auftreten?  
- Stellen Sie die Erweiterungs-App bereit und stellen Sie anschließend den Host bereit und führen Sie ihn aus
    - Übernimmt der Host bereits vorhandene Erweiterungen?  
- Führen Sie den Host aus, und entfernen Sie dann die Erweiterungs-App.
    - Erkennt der Host die Entfernung ordnungsgemäß?
- Führen Sie den Host aus, und ersetzen Sie die Erweiterungs-App anschließend durch eine neuere Version.
    - Übernimmt der Host die Änderung und entlädt die alten Versionen der Erweiterung ordnungsgemäß?  

**Erweiterte Szenarien testen:**

- Führen Sie den Host aus, verschieben Sie die Erweiterungs-App auf Wechselmedien, entfernen Sie das Medium
    - Erkennt der Host die Änderung des Paketstatus und deaktiviert die Erweiterungen?
- Führen Sie den Host aus, beschädigen Sie die Erweiterungs-App (machen Sie diese ungültig, ändern Sie die Signatur usw.)
    - Erkennt der Host die manipulierte Erweiterung und behandelt sie ordnungsgemäß?
- Führen Sie den Host aus, und stellen Sie dann eine Erweiterungs-App bereit, die ungültige Inhalte oder Eigenschaften hat
    - Erkennt der Host den ungültigen Inhalt und behandelt ihn ordnungsgemäß?

## <a name="design-considerations"></a>Überlegungen zum Entwurf

- Stellen Sie Benutzeroberflächen bereit, die dem Benutzer anzeigen, welche Erweiterungen zur Verfügung stehen, und ermöglichen Sie ihnen, diese zu aktivieren oder zu deaktivieren. Sie können außerdem Glyphen für Erweiterungen hinzufügen, die nicht mehr verfügbar sind, wenn das Paket offline geschaltet wurde usw. hinzufügen.
- Geben Sie dem Benutzer an, wo die Erweiterungen erhalten werden können. Ihre Seite der Erweiterung kann möglicherweise eine Suchabfrage des Microsoft Store bereitstellen, die eine Liste der Erweiterungen anbietet, die mit Ihrer App verwendet werden können.
- Überlegen Sie, wie Sie den Benutzer über das Hinzufügen und Entfernen von Erweiterungen benachrichtigen. Erstellen Sie eine Benachrichtigung, wenn eine neue Erweiterung installiert wird und fordern Sie den Benutzer auf, diese zu aktivieren. Erweiterungen sollte standardmäßig deaktiviert werden, damit Benutzer die Kontrolle haben.

## <a name="how-app-extensions-differ-from-optional-packages"></a>So unterscheiden sich App-Erweiterungen von optionalen Paketen

Der Hauptunterschied zwischen [optionalen Paketen](https://docs.microsoft.com/windows/uwp/packaging/optional-packages) und App-Erweiterungen ist ein offenes Ökosystem im Vergleich zu einem geschlossenen Ökosystem und abhängige Pakete im Vergleich zu unabhängigen Paketen.

App-Erweiterungen sind Teil eines offenen Ökosystems. Wenn Ihre App-Erweiterungen hosten kann, kann jeder Benutzer eine Erweiterung für den Host schreiben, solange er die Methode des Übergebens und Empfangens von Informationen aus der Erweiterung verwendet. Dies unterscheidet sich von optionalen Paketen, die Teil eines geschlossenen Ökosystem sind. Dort entscheidet der Herausgeber, wer ein optionales Paket erstellen darf, das mit der App verwendet werden kann.

App-Erweiterungen sind unabhängige Pakete und eigenständige Apps. Sie dürfen keine Abhängigkeit in puncto Bereitstellung auf einer anderen App besitzen. Bei optionalen Paketen ist das primäre Pakets erforderlich und diese können nicht ohne ausgeführt werden.

Ein Erweiterungspaket für ein Spiel wäre ein guter Kandidat für ein optionales Paket, da es eng mit dem Spiel verbunden ist und nicht unabhängig auf dem Spiel ausgeführt werden kann. Erweiterungspakete sollten nicht von beliebigen Entwickler im Ökosystem erstellt werden.

Hätte das gleiche Spiel anpassbare UI-Add-Ons oder Designs, wäre eine App-Erweiterung eine gute Wahl, da die App, die die Erweiterung anbietet, eigenständig ausgeführt werden kann und alle Drittanbieter diese erstellen kann.

## <a name="remarks"></a>Hinweise

Dieses Thema stellt eine Einführung zur App-Erweiterungen bereit. Die wichtigsten Punkte sind die Erstellung des Hosts und das Markieren in der Datei "Package.appxmanifest", das Erstellen der Erweiterung und das Markieren als solche in der Datei "Package.appxmanifest", festzulegen, wie die Erweiterung implementiert wird (z. B. als App-Dienst, als Hintergrundaufgaben oder anderweitig), zu definieren, wie der Host mit den Erweiterungen kommunizieren soll, und das Verwenden der [AppExtensions API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) für den Zugriff und das Verwalten der Erweiterungen.

## <a name="related-topics"></a>Verwandte Themen

* [Einführung in die App-Erweiterungen](https://blogs.msdn.microsoft.com/appinstaller/2017/05/01/introduction-to-app-extensions/)
* [Build 2016-Veranstaltung zu app-Erweiterungen](https://channel9.msdn.com/Events/Build/2016/B808)
* [Codebeispiel der Build 2016-app-Erweiterung](https://github.com/Microsoft/App-Extensibility-Sample)
* [Unterstützen Ihrer App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md)
* [Erstellen und Nutzen eines App-Diensts](how-to-create-and-consume-an-app-service.md).
* [Anwendungserweiterungen-namespace](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)
* [Erweitern Sie Ihre app mit Diensten, Erweiterungen und Pakete](https://docs.microsoft.com/windows/uwp/launch-resume/extend-your-app-with-services-extensions-packages)
