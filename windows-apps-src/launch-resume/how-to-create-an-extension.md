---
title: Erstellen und Hosten einer App-Erweiterung
description: Schreiben und Hosten Sie App-Erweiterungen, die es Ihnen ermöglichen, Ihre APP über Pakete zu erweitern, die Benutzer über die Microsoft Store installieren können.
keywords: App-Erweiterung, App Service, Hintergrund
ms.date: 01/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 122c7c4d206c014d7d76cdab0b1b8fc66c0c9371
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158844"
---
# <a name="create-and-host-an-app-extension"></a>Erstellen und Hosten einer App-Erweiterung

In diesem Artikel erfahren Sie, wie Sie eine Windows 10-App-Erweiterung erstellen und in einer APP hosten. App-Erweiterungen werden in UWP-apps und [gepackten Desktop-Apps](/windows/apps/desktop/modernize/#msix-packages)unterstützt.

Um zu veranschaulichen, wie eine APP-Erweiterung erstellt wird, verwendet dieser Artikel Paket Manifest-XML und Code Ausschnitte aus dem [Codebeispiel für die mathematische Erweiterung](https://github.com/MicrosoftDocs/windows-topic-specific-samples/tree/MathExtensionSample). Bei diesem Beispiel handelt es sich um eine UWP-APP, aber die im Beispiel gezeigten Funktionen gelten auch für gepackte Desktop-Apps. Befolgen Sie diese Anweisungen für die ersten Schritte mit dem Beispiel:

- Herunterladen und Entpacken des Code Beispiels für die [mathematische Erweiterung](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip).
- Öffnen Sie in Visual Studio 2019 mathextensionsample. sln. Legen Sie den Buildtyp auf x86 (**Build**  >  **Configuration Manager**und dann **Platform** in **x86** für beide Projekte) fest.
- Stellen Sie die Lösung bereit: **Erstellen**Sie die  >  **Lösung**.

## <a name="introduction-to-app-extensions"></a>Einführung in App-Erweiterungen

In Windows 10 bieten App-Erweiterungen ähnliche Funktionen wie Plug-ins, Add-Ins und Add-ons auf anderen Plattformen. App-Erweiterungen wurden in der Windows 10 Anniversary Edition (Version 1607, Build 10.0.14393) eingeführt.

App-Erweiterungen sind UWP-Apps oder gepackte Desktop-Apps mit einer Erweiterungs Deklaration, die es Ihnen ermöglichen, Inhalts-und Bereitstellungs Ereignisse mit einer Host-App freizugeben. Eine Erweiterungs-App kann mehrere Erweiterungen bereitstellen.

Da es sich bei App-Erweiterungen lediglich um UWP-Apps oder gepackte Desktop-Apps handelt, kann es sich auch um voll funktionsfähige apps, Host Erweiterungen und Erweiterungen für andere apps handeln, ohne dass separate App-Pakete erstellt werden müssen.

Wenn Sie einen app-Erweiterungs Host erstellen, erstellen Sie eine Gelegenheit, ein Ökosystem um Ihre APP zu entwickeln, in dem andere Entwickler Ihre APP so verbessern können, dass Sie möglicherweise nicht erwartet wurde oder die Ressourcen für Sie hätten. Beachten Sie Microsoft Office Erweiterungen, Visual Studio-Erweiterungen, Browser Erweiterungen usw. Diese sorgen für eine umfangreichere Umgebung für apps, die über die Funktionen hinausgehen, mit denen Sie ausgeliefert wurden. Erweiterungen können Ihrer APP Wert und Langlebigkeit hinzufügen.

Um eine APP-Erweiterungs Beziehung einzurichten, müssen Sie auf hoher Ebene folgende Aktionen ausführen:

1. Deklarieren Sie eine App als Erweiterungs Host.
2. Deklarieren Sie eine App als Erweiterung.
3. Entscheiden Sie, ob die Erweiterung als App Service, Hintergrundaufgabe oder auf andere Weise implementiert werden soll.
4. Hiermit wird definiert, wie die Hosts und deren Erweiterungen kommunizieren.
5. Verwenden Sie die [Windows. applicationmodel. appextensions](/uwp/api/Windows.ApplicationModel.AppExtensions) -API in der Host-APP, um auf die Erweiterungen zuzugreifen.

Sehen wir uns an, wie dies geschieht, indem wir das [Codebeispiel für die mathematische Erweiterung](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip) untersuchen, das einen hypothetischen Rechner implementiert, dem Sie mithilfe von Erweiterungen neue Funktionen hinzufügen können. Laden Sie in Microsoft Visual Studio 2019 **mathextensionsample. sln** aus dem Codebeispiel.

![Codebeispiel für mathematische Erweiterung](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>Deklarieren einer App als Erweiterungs Host

Eine APP identifiziert sich selbst als App-Erweiterungs Host, indem das- `<AppExtensionHost>` Element in der Datei "Package. appxmanifest" deklariert wird. Sehen Sie sich die Datei " **Package. appxmanifest** " im **mathextensionhost** -Projekt an, um zu sehen, wie dies geschieht.

_"Package. appxmanifest" im mathextensionhost-Projekt_
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

Beachten Sie `xmlns:uap3="http://..."` und das vorhanden sein von `uap3` in `IgnorableNamespaces` . Diese sind erforderlich, da der uap3-Namespace verwendet wird.

`<uap3:Extension Category="windows.appExtensionHost">` identifiziert diese APP als Erweiterungs Host.

Das **Name** -Element in `<uap3:AppExtensionHost>` ist der Name des _Erweiterungsvertrags_ . Wenn eine Erweiterung denselben Erweiterungsvertrags Namen angibt, kann Sie vom Host gefunden werden. Gemäß der Konvention wird empfohlen, den Namen des Erweiterungsvertrags mithilfe Ihres APP-oder Herausgeber namens zu entwickeln, um potenzielle Konflikte mit anderen Erweiterungsvertrags Namen zu vermeiden.

Sie können mehrere Hosts und mehrere Erweiterungen in derselben app definieren. In diesem Beispiel deklarieren wir einen Host. Die Erweiterung wird in einer anderen APP definiert.

## <a name="declare-an-app-to-be-an-extension"></a>Deklarieren einer App als Erweiterung

Eine APP identifiziert sich selbst als App-Erweiterung, indem das- `<uap3:AppExtension>` Element in der Datei " **Package. appxmanifest** " deklariert wird. Öffnen Sie die Datei " **Package. appxmanifest** " im Projekt " **mathextension** ", um zu sehen, wie dies geschieht.

_"Package. appxmanifest" im mathextension-Projekt:_
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

Beachten Sie die `xmlns:uap3="http://..."` Zeile und das vorhanden sein von `uap3` in `IgnorableNamespaces` . Diese sind erforderlich, da der- `uap3` Namespace verwendet wird.

`<uap3:Extension Category="windows.appExtension">` identifiziert diese APP als Erweiterung.

Die `<uap3:AppExtension>` Attribute lauten wie folgt:

|attribute|BESCHREIBUNG|Erforderlich|
|---------|-----------|:------:|
|**Name**|Dies ist der Name des Erweiterungsvertrags. Wenn Sie mit dem in einem Host deklarierten **Namen** übereinstimmt, kann dieser Host diese Erweiterung finden.| :heavy_check_mark: |
|**ID**| Identifiziert diese Extension eindeutig. Da es mehrere Erweiterungen geben kann, die denselben Erweiterungsvertrags Namen verwenden (stellen Sie sich eine Paint-APP, die mehrere Erweiterungen unterstützt), können Sie die ID verwenden, um sie voneinander zu unterscheiden. App-Erweiterungs Hosts können die ID verwenden, um etwas über den Typ der Erweiterung abzuleiten. Beispielsweise könnten Sie eine Erweiterung für Desktop und eine andere für mobile Geräte mit der ID unterscheiden. Sie können auch das **Eigenschaften** -Element verwenden, das weiter unten erläutert wird.| :heavy_check_mark: |
|**DisplayName**| Kann von Ihrer Host-App verwendet werden, um die Erweiterung für den Benutzer zu identifizieren. Das [neue Resource Management-System](../app-resources/using-mrt-for-converted-desktop-apps-and-games.md) () kann für die Lokalisierung abgefragt und verwendet werden `ms-resource:TokenName` . Der lokalisierte Inhalt wird aus dem App-Erweiterungspaket geladen, nicht aus der Host-app. | |
|**Beschreibung** | Kann von Ihrer Host-App verwendet werden, um die Erweiterung für den Benutzer zu beschreiben. Das [neue Resource Management-System](../app-resources/using-mrt-for-converted-desktop-apps-and-games.md) () kann für die Lokalisierung abgefragt und verwendet werden `ms-resource:TokenName` . Der lokalisierte Inhalt wird aus dem App-Erweiterungspaket geladen, nicht aus der Host-app. | |
|**PublicFolder**|Der Name eines Ordners, der relativ zum Stamm des Pakets ist, in dem Sie Inhalte für den Erweiterungs Host freigeben können. Gemäß der Konvention lautet der Name "Public", aber Sie können einen beliebigen Namen verwenden, der mit einem Ordner in ihrer Erweiterung übereinstimmt.| :heavy_check_mark: |

`<uap3:Properties>` ist ein optionales Element, das benutzerdefinierte Metadaten enthält, die Hosts zur Laufzeit lesen können. Im Codebeispiel wird die Erweiterung als App Service implementiert, sodass der Host eine Möglichkeit zum Abrufen des Namens dieses app Service benötigt, damit er ihn abrufen kann. Der Name des App-Dienstanbieter wird im- <Service> Element definiert, das definiert wurde (wir hätten es möglicherweise beliebig aufgerufen). Der Host im Codebeispiel sucht zur Laufzeit nach dieser Eigenschaft, um den Namen des App Service zu erlernen.

## <a name="decide-how-you-will-implement-the-extension"></a>Entscheiden Sie, wie Sie die Erweiterung implementieren werden.

In der [Build 2016-Sitzung zu app-Erweiterungen](https://channel9.msdn.com/Events/Build/2016/B808) wird veranschaulicht, wie der öffentliche Ordner verwendet wird, der vom Host und den Erweiterungen gemeinsam genutzt wird. In diesem Beispiel wird die Erweiterung durch eine JavaScript-Datei implementiert, die im öffentlichen Ordner gespeichert wird, den der Host aufruft. Diese Vorgehensweise hat den Vorteil, dass Sie einfach ist, keine Kompilierung erfordert, und unterstützt die Erstellung der Standard Startseite, die Anweisungen für die Erweiterung und einen Link zur Microsoft Store Seite der Host-App bereitstellt. Weitere Informationen finden Sie im [Codebeispiel Build 2016-App-Erweiterung](https://github.com/Microsoft/App-Extensibility-Sample) . Informationen hierzu finden Sie im Projekt " **invertimageextension** " und `InvokeLoad()` in ExtensionManager.cs im Projekt " **extensibilitysample** ".

In diesem Beispiel verwenden wir einen app Service, um die Erweiterung zu implementieren. App-Dienste haben die folgenden Vorteile:

- Wenn die Erweiterung abstürzt, wird die Host-APP nicht herunterfährt, da die Host-app in einem eigenen Prozess ausgeführt wird.
- Sie können die Sprache Ihrer Wahl verwenden, um den Dienst zu implementieren. Sie muss nicht mit der Sprache identisch sein, die zum Implementieren der Host-App verwendet wird.
- Der APP Service kann auf seinen eigenen App-Container zugreifen, der möglicherweise andere Funktionen als der Host hat.
- Es gibt eine Isolation zwischen den Daten im Dienst und der Host-app.

### <a name="host-app-service-code"></a>Hosten von App Service-Code

Dies ist der Hostcode, der den App Service der Erweiterung aufruft:

_ExtensionManager.cs im mathextensionhost-Projekt_
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

Dies ist der typische Code zum Aufrufen eines App Service. Ausführliche Informationen zum Implementieren und Abrufen von App Service finden Sie unter [Erstellen und Verwenden von App Service](how-to-create-and-consume-an-app-service.md).

Beachten Sie, dass der Name des aufzurufenden App Service bestimmt wird. Da der Host keine Informationen zur Implementierung der Erweiterung enthält, muss die Erweiterung den Namen des App Service bereitstellen. Im Codebeispiel deklariert die Erweiterung den Namen des App Service in der zugehörigen Datei im- `<uap3:Properties>` Element:

_"Package. appxmanifest" im mathextension-Projekt_
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

Sie können im-Element einen eigenen XML-Code definieren `<uap3:Properties>` . In diesem Fall definieren wir den Namen des App Service, damit der Host ihn aufrufen kann, wenn er die Erweiterung aufruft.

Wenn der Host eine Erweiterung lädt, extrahiert Code wie this den Namen des Diensts aus den Eigenschaften, die in der Datei "Package. appxmanifest" der Erweiterung definiert sind:

_`Update()` in ExtensionManager.cs im mathextensionhost-Projekt_
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

Mit dem Namen des App Service, der in gespeichert ist `_serviceName` , kann der Host ihn zum Aufrufen von App Service verwenden.

Zum Aufrufen eines App Service ist auch der Familienname des Pakets erforderlich, das den App Service enthält. Glücklicherweise stellt die APP-Erweiterungs-API diese Informationen zur Verfügung, die in der folgenden Zeile abgerufen werden: `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>Hiermit wird definiert, wie der Host und die Erweiterung kommunizieren.

App-Dienste verwenden [ValueSet](/uwp/api/windows.foundation.collections.valueset) zum Austauschen von Informationen. Als Autor des Hosts müssen Sie über ein Protokoll für die Kommunikation mit flexiblen Erweiterungen verfügen. Im Codebeispiel bedeutet das, dass Erweiterungen berücksichtigt werden, die in der Zukunft 1, 2 oder mehr Argumente annehmen können.

In diesem Beispiel ist das Protokoll für die Argumente ein **valueset** , das die Schlüssel-Wert-Paare "Arg" und die Argument Nummer enthält, z `Arg1` `Arg2` . b. und. Der Host übergibt alle Argumente im **valueset**, und die Erweiterung nutzt die erforderlichen Argumente. Wenn die Erweiterung ein Ergebnis berechnen kann, erwartet der Host, dass das von der Erweiterung zurückgegebene **valueset** über einen Schlüssel namens verfügt, `Result` der den Wert der Berechnung enthält. Wenn der Schlüssel nicht vorhanden ist, geht der Host davon aus, dass die Erweiterung die Berechnung nicht vervollständigen konnte.

### <a name="extension-app-service-code"></a>Erweiterungs-App Service-Code

Im Codebeispiel wird der APP Service der Erweiterung nicht als Hintergrundaufgabe implementiert. Stattdessen wird das einzelne proc App Service-Modell verwendet, in dem der APP Service in demselben Prozess ausgeführt wird wie die Erweiterungs-APP, die ihn hostet. Dabei handelt es sich immer noch um einen anderen Prozess als die Host-APP, der die Vorteile der Prozess Trennung bietet und Leistungsvorteile erzielt, indem die prozessübergreifende Kommunikation zwischen dem Erweiterungsprozess und dem Hintergrundprozess vermieden wird, der den App Service implementiert. Weitere Informationen finden Sie unter [Konvertieren eines App Service für die Ausführung in demselben Prozess wie die](convert-app-service-in-process.md) zugehörige Host-APP, um den Unterschied zwischen einem App Service, der als Hintergrundaufgabe ausgeführt wird, im Vergleich zum gleichen Prozess anzuzeigen.

Das System erstellt, `OnBackgroundActivate()` Wenn der APP Service aktiviert wird. Dieser Code richtet Ereignishandler ein, um den eigentlichen App Service-Befehl zu verarbeiten, wenn er () ist, und verarbeitet Ereignisse wie das Abrufen eines Verzögerungs Objekts, das `OnAppServiceRequestReceived()` einen Abbruch oder ein geschlossenes Ereignis verarbeitet.

_App.XAML.cs im mathextension-Projekt._
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

Der Code, der die Arbeit der Erweiterung erledigt, befindet sich in `OnAppServiceRequestReceived()` . Diese Funktion wird aufgerufen, wenn der APP Service aufgerufen wird, um eine Berechnung auszuführen. Die benötigten Werte werden aus dem **valueset**extrahiert. Wenn die Berechnung durchgeführt werden kann, wird das Ergebnis unter einem Schlüssel mit dem Namen **Result**in dem **valueset** eingefügt, das an den Host zurückgegeben wird. Beachten Sie, dass das vorhanden sein eines **Ergebnis** Schlüssels gemäß dem Protokoll, das für die Kommunikation dieses Hosts und seiner Erweiterungen definiert ist, den Erfolg angibt. andernfalls ein Fehler.

_App.XAML.cs im mathextension-Projekt._
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

Nachdem Sie nun gesehen haben, wie die Beziehung zwischen einem Host und seinen Erweiterungen implementiert wird, sehen wir uns an, wie ein Host auf dem System installierte Erweiterungen findet und auf das Hinzufügen und Entfernen von Paketen mit Erweiterungen reagiert.

Der Microsoft Store übermittelt Erweiterungen als Pakete. Der [appextensioncatalog](/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) findet installierte Pakete, die Erweiterungen enthalten, die mit dem Namen des Erweiterungsvertrags des Hosts übereinstimmen, und stellt Ereignisse bereit, die ausgelöst werden, wenn ein für den Host relevantes App-Erweiterungspaket installiert oder entfernt wird.

Im Codebeispiel umschließt die- `ExtensionManager` Klasse (definiert in **ExtensionManager.cs** im **mathextensionhost** -Projekt) die Logik zum Laden von Erweiterungen und antwortet auf Installationen und Deinstallieren von Erweiterungs Paketen.

Der `ExtensionManager` Konstruktor verwendet das `AppExtensionCatalog` , um die APP-Erweiterungen auf dem System zu suchen, die denselben Erweiterungsvertrags Namen wie der Host aufweisen:

_ExtensionManager.cs im mathextensionhost-Projekt._
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

Wenn ein Erweiterungspaket installiert ist, `ExtensionManager` sammelt die Informationen zu den Erweiterungen im Paket, die denselben Erweiterungsvertrags Namen wie der Host aufweisen. Eine Installation kann ein Update darstellen. in diesem Fall werden die Informationen der betroffenen Erweiterung aktualisiert. Wenn ein Erweiterungspaket deinstalliert wird, `ExtensionManager` entfernt die Informationen zu den betroffenen Erweiterungen, sodass der Benutzer weiß, welche Erweiterungen nicht mehr verfügbar sind.

Die- `Extension` Klasse (definiert in **ExtensionManager.cs** im **mathextensionhost** -Projekt) wurde für das Codebeispiel erstellt, um auf die ID, Beschreibung, das Logo und die APP-spezifischen Informationen einer Erweiterung zuzugreifen, z. b. ob der Benutzer die Erweiterung aktiviert hat.

Um zu sagen, dass die Erweiterung geladen wird (siehe `Load()` in **ExtensionManager.cs**), bedeutet dies, dass der Paketstatus gut ist und dass wir die ID, das Logo, die Beschreibung und den öffentlichen Ordner abgerufen haben (die wir in diesem Beispiel nicht verwenden, um zu veranschaulichen, wie Sie Sie erhalten). Das Erweiterungspaket selbst wird nicht geladen.

Das Entlade Konzept wird verwendet, um zu verfolgen, welche Erweiterungen dem Benutzer nicht mehr angezeigt werden sollen.

Stellt eine Auflistungs `ExtensionManager` `Extension` Instanz bereit, sodass die Erweiterungen, ihre Namen, Beschreibungen und Logos Daten an die Benutzeroberfläche gebunden werden können. Die **extensionstab** -Seite wird an diese Auflistung gebunden und bietet eine Benutzeroberfläche zum Aktivieren/Deaktivieren von Erweiterungen sowie zum Entfernen der Erweiterungen.

![Beispiel Benutzeroberfläche der Erweiterungs Registerkarte](images/mathextensionhost-extensiontab.png)

 Wenn eine Erweiterung entfernt wird, fordert das System den Benutzer auf, zu überprüfen, ob das Paket mit der Erweiterung (und möglicherweise andere Erweiterungen) deinstalliert werden soll. Wenn der Benutzer zustimmt, wird das Paket deinstalliert, und der `ExtensionManager` entfernt die Erweiterungen im nicht installierten Paket aus der Liste der Erweiterungen, die der Host-App zur Verfügung stehen.

 ![Benutzeroberfläche deinstallieren](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>Debugging von App-Erweiterungen und-Hosts

Häufig sind der Erweiterungs Host und die Erweiterung nicht Teil derselben Projekt Mappe. In diesem Fall zum Debuggen des Hosts und der Erweiterung:

1. Laden Sie das Host Projekt in einer Instanz von Visual Studio.
2. Laden Sie die Erweiterung in eine andere Instanz von Visual Studio.
3. Starten Sie die Host-App im Debugger.
4. Starten Sie die Erweiterungs-App im Debugger. (Wenn Sie die Erweiterung bereitstellen möchten, anstatt Sie zu debuggen, um das Paket Installations Ereignis des Hosts zu testen, erstellen Sie stattdessen eine ** &gt; Lösung**für die Bereitstellung.)

Nun können Sie auf dem Host und der Erweiterung Haltepunkte erreichen.
Wenn Sie mit dem Debuggen der Erweiterungs-APP selbst beginnen, wird ein leeres Fenster für die App angezeigt. Wenn Sie das leere Fenster nicht sehen möchten, können Sie die Debugeinstellungen für das Erweiterungsprojekt so ändern, dass die APP nicht gestartet wird, sondern stattdessen Debuggen, wenn Sie gestartet wird (Klicken Sie mit der rechten Maustaste auf das Erweiterungsprojekt, **Eigenschaften**  >  **Debuggen** > wählen Sie den **Code beim Start nicht starten, sondern Debuggen**aus. Sie müssen weiterhin das Debuggen (**F5**) des Erweiterungsprojekts starten. es wird jedoch gewartet, bis der Host die Erweiterung aktiviert hat, und dann werden die Breakpoints in der Erweiterung getroffen.

**Debuggen des Code Beispiels**

Im Codebeispiel befinden sich der Host und die Erweiterung in derselben Projekt Mappe. Gehen Sie zum Debuggen wie folgt vor:

1. Stellen Sie sicher, dass **mathextensionhost** das Startprojekt ist. Klicken Sie mit der rechten Maustaste auf das **mathextensionhost** -Projekt, und klicken Sie auf **als Startprojekt festlegen**.
2. Platzieren Sie einen Haltepunkt in `Invoke` ExtensionManager.cs im **mathextensionhost** -Projekt.
3. **F5** zum Ausführen des **mathextensionhost** -Projekts.
4. Fügen Sie in `OnAppServiceRequestReceived` app.XAML.cs im **mathextension** -Projekt einen Haltepunkt ein.
5. Starten Sie das Debuggen des **mathextension** -Projekts (Klicken Sie mit der rechten Maustaste auf das **mathextension** -Projekt, **Debuggen Sie > neue Instanz starten**), um es bereitzustellen, und löst das Paket Installations Ereignis
6. Navigieren Sie in der **mathextensionhost** -App zur Seite **Berechnung** , und klicken Sie auf **x ^ y** , um die Erweiterung zu aktivieren. Der `Invoke()` Breakpoint wird zuerst angezeigt, und Sie können sehen, wie der APP Service-Erweiterungs aufzurufen ist. Anschließend `OnAppServiceRequestReceived()` wird die-Methode in der Erweiterung angezeigt, und Sie sehen, dass der APP-Dienst das Ergebnis berechnet und zurückgibt.

**Problembehandlung bei Erweiterungen, die als App Service implementiert sind**

Wenn Ihr Erweiterungs Host Probleme beim Herstellen einer Verbindung mit dem App Service für Ihre Erweiterung hat, stellen Sie sicher, dass das `<uap:AppService Name="...">` Attribut mit dem übereinstimmt, das Sie in das `<Service>` Element einfügen Wenn Sie nicht identisch sind, entspricht der Dienst Name, den die Erweiterung für den Host bereitstellt, nicht dem von Ihnen implementierten App Service-Namen, und der Host kann die Erweiterung nicht aktivieren.

_"Package. appxmanifest" im mathextension-Projekt:_
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

## <a name="a-checklist-of-basic-scenarios-to-test"></a>Eine Prüfliste mit grundlegenden Testszenarien

Wenn Sie einen Erweiterungs Host erstellen und bereit sind, zu testen, wie gut Erweiterungen unterstützt werden, können Sie die folgenden grundlegenden Szenarios ausprobieren:

- Ausführen des Hosts und anschließendes Bereitstellen einer Erweiterungs-App  
    - Nimmt der Host neue Erweiterungen auf, die während seiner Ausführung auftreten?  
- Stellen Sie die Erweiterungs-App bereit, und führen Sie dann den Host aus.
    - Übernimmt der Host bereits vorhandene Erweiterungen?  
- Führen Sie den Host aus, und entfernen Sie die Erweiterungs-app.
    - Erkennt der Host die Entfernung ordnungsgemäß?
- Führen Sie den Host aus, und aktualisieren Sie dann die Erweiterungs-App auf eine neuere Version.
    - Nimmt der Host die Änderung an, und Entladen Sie die alten Versionen der Erweiterung ordnungsgemäß?  

**Erweiterte Szenarien zum Testen:**

- Ausführen des Hosts, Verschieben der Erweiterungs-App auf ein Wechselmedium, Entfernen der Medien
    - Erkennt der Host die Änderung des Paketstatus und deaktiviert die Erweiterung?
- Führen Sie den Host aus, und beschädigen Sie dann die Erweiterungs-app (ungültig, anders signiert usw.).
    - Erkennt der Host die Manipulations Erweiterung und verarbeitet sie ordnungsgemäß?
- Führen Sie den Host aus, und stellen Sie eine Erweiterungs-App mit ungültigem Inhalt oder Eigenschaften bereit.
    - Erkennt der Host ungültigen Inhalt und verarbeitet ihn ordnungsgemäß?

## <a name="design-considerations"></a>Überlegungen zum Entwurf

- Stellen Sie eine Benutzeroberfläche bereit, die den Benutzer anzeigt, welche Erweiterungen verfügbar sind, und ermöglicht Ihnen, diese zu aktivieren/deaktivieren. Sie können auch das Hinzufügen von Symbolen für Erweiterungen in Erwägung nehmen, die nicht mehr verfügbar sind, da ein Paket offline geschaltet wird usw.
- Leiten Sie den Benutzer an die Stelle, an der er Erweiterungen erhalten kann. Möglicherweise kann Ihre Erweiterungs Seite eine Microsoft Store Suchabfrage bereitstellen, die eine Liste von Erweiterungen bereitstellt, die mit Ihrer APP verwendet werden können.
- Beachten Sie, wie der Benutzer über das Hinzufügen und Entfernen von Erweiterungen benachrichtigt wird. Sie können eine Benachrichtigung erstellen, wenn eine neue Erweiterung installiert ist, und den Benutzer zum Aktivieren auffordern. Erweiterungen sollten standardmäßig deaktiviert werden, damit die Benutzer die Kontrolle haben.

## <a name="how-app-extensions-differ-from-optional-packages"></a>Unterschiede zwischen App-Erweiterungen und optionalen Paketen

Der Hauptunterschied zwischen [optionalen Paketen](/windows/msix/package/optional-packages) und App-Erweiterungen besteht darin, dass ein offenes Ökosystem und ein geschlossenes Ökosystem und das abhängige Paket im Vergleich zum unabhängigen Paket sind.

App-Erweiterungen nehmen an einem offenen Ökosystem Teil. Wenn Ihre APP App-Erweiterungen hosten kann, kann jeder Benutzer eine Erweiterung für Ihren Host schreiben, sofern diese Ihrer Methode zum übergeben/empfangen von Informationen aus der Erweiterung entsprechen. Dies unterscheidet sich von optionalen Paketen, die an einem geschlossenen Ökosystem beteiligt sind, in dem der Herausgeber entscheidet, wer ein optionales Paket erstellen darf, das mit der APP verwendet werden kann.

App-Erweiterungen sind unabhängige Pakete und können eigenständige apps sein. Sie können keine Bereitstellungs Abhängigkeit von einer anderen APP haben.Optionale Pakete erfordern das primäre Paket und können nicht ohne das Paket ausgeführt werden.

Ein Erweiterungspaket für ein Spiel wäre ein guter Kandidat für ein optionales Paket, da es eng an das Spiel gebunden ist, nicht unabhängig von dem Spiel ausgeführt werden kann und Sie nicht möchten, dass Erweiterungs Pakete von nur einem Entwickler im Ökosystem erstellt werden.

Wenn das gleiche Spiel über anpassbare Benutzeroberflächen-Add-ons oder Designs verfügt, könnte eine APP-Erweiterung eine gute Wahl sein, da die APP, die die Erweiterung bereitstellt, eigenständig ausgeführt werden kann, und jeder dritte Anbieter diese erstellen könnte.

## <a name="remarks"></a>Bemerkungen

Dieses Thema enthält eine Einführung in App-Erweiterungen. Beachten Sie die wichtigsten Punkte, die Sie bei der Erstellung des Hosts und dem kennzeichnen der Hosts als solche im Paket beachten müssen. appxmanifest-Datei, Erstellen der Erweiterung und Markieren der Erweiterung in der Datei "Package. appxmanifest", um zu bestimmen, wie die Erweiterung implementiert werden soll (z. b. ein App Service, eine Hintergrundaufgabe oder andere Mittel), definieren, wie der Host mit Erweiterungen kommuniziert, und Verwenden der [appextensions-API](/uwp/api/windows.applicationmodel.appextensions) , um auf Erweiterungen zuzugreifen und diese zu verwalten.

## <a name="related-topics"></a>Zugehörige Themen

* [Einführung in App-Erweiterungen](/windows/msix/)
* [Build 2016-Sitzung zu app-Erweiterungen](https://channel9.msdn.com/Events/Build/2016/B808)
* [Codebeispiel für Build 2016-App-Erweiterung](https://github.com/Microsoft/App-Extensibility-Sample)
* [Unterstützen Ihrer App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md)
* [Erstellen und Nutzen eines App Service](how-to-create-and-consume-an-app-service.md).
* [Appextensions-Namespace](/uwp/api/windows.applicationmodel.appextensions)
* [Erweitern der App mit Diensten, Erweiterungen und Paketen](./extend-your-app-with-services-extensions-packages.md)