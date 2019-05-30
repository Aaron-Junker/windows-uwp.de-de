---
title: Starten einer App für Ergebnisse
description: Erfahren Sie, wie Sie eine App über eine andere App starten und Daten zwischen den beiden Apps austauschen. Dieser Vorgang wird als Starten einer App für Ergebnisse bezeichnet.
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 23d4a4e0159fc18ac524937326e69d6fbc3a627e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370708"
---
# <a name="launch-an-app-for-results"></a>Starten einer App für Ergebnisse




**Wichtige APIs**

-   [**LaunchUriForResultsAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.windows)
-   [**ValueSet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet)

Erfahren Sie, wie Sie eine App über eine andere App starten und Daten zwischen den beiden Apps austauschen. Dieser Vorgang wird als *Starten einer App für Ergebnisse* bezeichnet. Dieses Beispiel veranschaulicht die Verwendung von [**LaunchUriForResultsAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.windows) zum Starten einer App für Ergebnisse.

Neue app zur app-Kommunikations-APIs in Windows 10 können für Windows-apps (und Windows-Web-apps) zum Starten einer app und Exchange-Daten und Dateien. Dies ermöglicht Ihnen das Erstellen kombinierter Lösungen aus mehreren Apps. Dank dieser neuen APIs können komplexe Aufgaben, für die Benutzer bisher mehrere Apps nutzen mussten, jetzt nahtlos durchgeführt werden. Ihrer App kann z. B. eine App für ein soziales Netzwerk starten, um einen Kontakt auszuwählen, oder eine Kassen-App, um einen Bezahlvorgang durchzuführen.

Die App, die für Ergebnisse geöffnet wird, wird als gestartete App bezeichnet. Die App, die die App startet, wird als aufrufende App bezeichnet. In diesem Beispiel schreiben Sie die aufrufende App und die gestartete App.

## <a name="step-1-register-the-protocol-to-be-handled-in-the-app-that-youll-launch-for-results"></a>Schritt 1: Registrieren Sie das Protokoll in die app verarbeitet werden, die Sie, um Ergebnisse zu erzielen starten müssen


Fügen Sie in der Datei „Package.appxmanifest“ der gestarteten App dem Abschnitt **&lt;Application&gt;** eine Protokollerweiterung hinzu. Im folgenden Beispiel wird ein fiktives Protokoll mit dem Namen **test-app2app** verwendet.

Das **ReturnResults**-Attribut in der Protokollerweiterung akzeptiert einen der folgenden Werte:

-   **optional**: Die App kann mit der [**LaunchUriForResultsAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.windows)-Methode für Ergebnisse oder mit der [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)-Methode nicht für Ergebnisse gestartet werden. Bei der Verwendung von **optional** muss die gestartete App ermitteln, ob sie für Ergebnisse gestartet wurde. Sie überprüft dazu das [**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated)-Ereignisargument. Wenn die [**IActivatedEventArgs.Kind**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind)-Eigenschaft des Arguments[**ActivationKind.ProtocolForResults**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) zurückgibt oder der Typ des Ereignisarguments [**ProtocolActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) ist, wurde die App über **LaunchUriForResultsAsync** gestartet.
-   **always**: Die App kann nur für Ergebnisse gestartet werden, d. h. sie kann nur auf [**LaunchUriForResultsAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.windows) reagieren.
-   **none**: Die App kann nicht für Ergebnisse gestartet werden, d. h. sie kann nur auf [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) reagieren.

Im folgenden Beispiel für eine Protokollerweiterung kann die App nur für Ergebnisse gestartet werden. Dadurch wird die im Folgenden erläuterte Logik innerhalb der **OnActivated**-Methode vereinfacht, da wir nur das „Starten für Ergebnisse“ behandeln müssen und nicht die anderen Möglichkeiten zum Aktivieren der App.

```xml
<Applications>
   <Application ...>

     <Extensions>
       <uap:Extension Category="windows.protocol">
         <uap:Protocol Name="test-app2app" ReturnResults="always">
           <uap:DisplayName>Test app-2-app</uap:DisplayName>
         </uap:Protocol>
       </uap:Extension>
     </Extensions>

   </Application>
</Applications>
```

## <a name="step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results"></a>Schritt 2: Überschreiben Sie Application.OnActivated in der app, die Sie, um Ergebnisse zu erzielen starten müssen


Wenn diese Methode in der gestarteten App noch nicht vorhanden ist, erstellen Sie sie innerhalb der in „App.xaml.cs“ definierten `App`-Klasse.

In einer App, in der Sie Freunde in einem sozialen Netzwerk auswählen können, wird mit dieser Funktion beispielsweise die Seite für die Kontaktauswahl geöffnet. Im folgenden Beispiel wird eine Seite mit dem Namen **LaunchedForResultsPage** angezeigt, wenn die App für Ergebnisse aktiviert wird. Stellen Sie sicher, dass die folgende **using**-Anweisung oben in der Datei vorhanden ist:

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnActivated(IActivatedEventArgs args)
{
    // Window management
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        rootFrame = new Frame();
        Window.Current.Content = rootFrame;
    }

    // Code specific to launch for results
    var protocolForResultsArgs = (ProtocolForResultsActivatedEventArgs)args;
    // Open the page that we created to handle activation for results.
    rootFrame.Navigate(typeof(LaunchedForResultsPage), protocolForResultsArgs);

    // Ensure the current window is active.
    Window.Current.Activate();
}
```

Da die Protokollerweiterung in der Datei „Package.appxmanifest“ **ReturnResults** als **always** angibt, kann der eben gezeigte Code `args` direkt in [**ProtocolForResultsActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ProtocolForResultsActivatedEventArgs) umwandeln. Dabei wird sichergestellt, dass für diese App nur **ProtocolForResultsActivatedEventArgs** an **OnActivated** gesendet wird. Wenn für Ihre App andere Aktivierungsoptionen als „Starten für Ergebnisse“ verfügbar sind, können Sie überprüfen, ob die [**IActivatedEventArgs.Kind**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind)-Eigenschaft [**ActivationKind.ProtocolForResults**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) zurückgibt, um festzustellen, ob die App für Ergebnisse gestartet wurde.

## <a name="step-3-add-a-protocolforresultsoperation-field-to-the-app-you-launch-for-results"></a>Schritt 3: Fügen Sie ein ProtocolForResultsOperation-Feld hinzu, um die app, die Sie, um Ergebnisse zu erzielen starten


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

Sie verwenden das [**ProtocolForResultsOperation**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.protocolforresultsactivatedeventargs.protocolforresultsoperation)-Feld, um zu signalisieren, wenn die gestartete App zur Zurückgabe des Ergebnisses an die aufrufende App bereit ist. In diesem Beispiel wird das Feld der **LaunchedForResultsPage**-Klasse hinzugefügt, da wir den Vorgang zum Starten für Ergebnisse auf dieser Seite durchführen und Zugriff darauf benötigen.

## <a name="step-4-override-onnavigatedto-in-the-app-you-launch-for-results"></a>Schritt 4: Überschreiben Sie OnNavigatedTo() in der app, die Sie, um Ergebnisse zu erzielen starten


Überschreiben Sie die [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)-Methode auf der Seite, die Sie anzeigen, wenn die App für Ergebnisse gestartet wird. Wenn diese Methode in der App noch nicht vorhanden ist, erstellen Sie sie innerhalb der in &lt;Seitenname&gt;.xaml.cs“ definierten Klasse. Stellen Sie sicher, dass die folgende **using**-Anweisung oben in der Datei vorhanden ist:

```cs
using Windows.ApplicationModel.Activation
```

Das [**NavigationEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation.NavigationEventArgs)-Objekt in der [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)-Methode enthält die von der aufrufenden Anwendung übergebenen Daten. Die Daten sind auf 100 KB begrenzt und werden in einem [**ValueSet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet)-Objekt gespeichert.

Im folgenden Beispielcode erwartet die gestartete App, dass sich die von der aufrufenden Anwendung gesendeten Daten in einem [**ValueSet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) unter einem Schlüssel mit dem Namen **TestData** befinden, da das Startprogramm der Beispiel-App entsprechend codiert ist.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var protocolForResultsArgs = e.Parameter as ProtocolForResultsActivatedEventArgs;
    // Set the ProtocolForResultsOperation field.
    _operation = protocolForResultsArgs.ProtocolForResultsOperation;

    if (protocolForResultsArgs.Data.ContainsKey("TestData"))
    {
        string dataFromCaller = protocolForResultsArgs.Data["TestData"] as string;
    }
}
...
private Windows.System.ProtocolForResultsOperation _operation = null;
```

## <a name="step-5-write-code-to-return-data-to-the-calling-app"></a>Schritt 5: Schreiben von Code zum Zurückgeben von Daten an die aufrufende Anwendung


In der für Ergebnisse gestarteten App verwenden Sie [**ProtocolForResultsOperation**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.protocolforresultsactivatedeventargs.protocolforresultsoperation), um Daten an die aufrufende App zurückzugeben. Im folgenden Beispielcode wird ein [**ValueSet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet)-Objekt erstellt, das den Wert enthält, der an die aufrufende App zurückgegeben werden soll. Anschließend wird das **ProtocolForResultsOperation**-Feld verwendet, um den Wert an die aufrufende App zu senden.

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## <a name="step-6-write-code-to-launch-the-app-for-results-and-get-the-returned-data"></a>Schritt 6: Schreiben von Code zum Starten der app um Ergebnisse zu erzielen, und rufen Sie die zurückgegebenen Daten


Starten Sie die App aus einer asynchronen Methode in der aufrufenden App, wie in diesem Beispielcode dargestellt. Beachten Sie die **using**-Anweisungen, die zum Kompilieren des Codes erforderlich sind:

```cs
using System.Threading.Tasks;
using Windows.System;
...

async Task<string> LaunchAppForResults()
{
    var testAppUri = new Uri("test-app2app:"); // The protocol handled by the launched app
    var options = new LauncherOptions();
    options.TargetApplicationPackageFamilyName = "67d987e1-e842-4229-9f7c-98cf13b5da45_yd7nk54bq29ra";

    var inputData = new ValueSet();
    inputData["TestData"] = "Test data";

    string theResult = "";
    LaunchUriResult result = await Windows.System.Launcher.LaunchUriForResultsAsync(testAppUri, options, inputData);
    if (result.Status == LaunchUriStatus.Success &&
        result.Result != null &&
        result.Result.ContainsKey("ReturnedData"))
    {
        ValueSet theValues = result.Result;
        theResult = theValues["ReturnedData"] as string;
    }
    return theResult;
}
```

In diesem Beispiel wird ein [**ValueSet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet) mit dem Schlüssel **TestData** an die gestartete App übergeben. Die gestartete App erstellt einen **ValueSet** mit einem Schlüssel mit dem Namen **ReturnedData**, der das Ergebnis enthält, das an den Aufrufer zurückgegeben wird.

Sie müssen die App, die Sie für Ergebnisse starten, erstellen und bereitstellen, bevor Sie die aufrufende App ausführen können. Andernfalls wird von [**LaunchUriResult.Status**](https://docs.microsoft.com/uwp/api/Windows.System.LaunchUriStatus) der Status **LaunchUriStatus.AppUnavailable** gemeldet.

Sie benötigen den Familiennamen der gestarteten App beim Festlegen von [**TargetApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.targetapplicationpackagefamilyname). Eine Möglichkeit, den Familiennamen abzurufen besteht darin, folgenden Aufruf in der gestarteten App auszuführen:

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## <a name="remarks"></a>Hinweise


Das Beispiel in dieser Anleitung bietet eine Einführung in das Starten einer App für Ergebnisse anhand von „Hello World“. Als wichtigster Punkt ist zu beachten, dass die neue [**LaunchUriForResultsAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.windows)-API Ihnen das asynchrone Starten einer App und die Kommunikation über die [**ValueSet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet)-Klasse ermöglicht. Das Übergeben von Daten über einen **ValueSet** ist auf 100 KB begrenzt. Wenn Sie größere Datenmengen übergeben müssen, können Sie Dateien mithilfe der [**SharedStorageAccessManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager)-Klasse freigeben, um Dateitoken zu erstellen, die zwischen Apps ausgetauscht werden können. Wenn Sie beispielsweise über **ValueSet** mit dem Namen `inputData` verfügen, könnten Sie das Token in einer Datei speichern, die Sie für die gestartete App freigeben möchten:

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

Anschließend übergeben Sie sie über **LaunchUriForResultsAsync** an die gestartete App.

## <a name="related-topics"></a>Verwandte Themen


* [**LaunchUri**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)
* [**LaunchUriForResultsAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.windows)
* [**ValueSet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.ValueSet)

 

 
