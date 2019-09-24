---
Description: Mithilfe eines Webansichtssteuerelements betten Sie eine Ansicht in Ihre App ein, die Webinhalte mit dem Microsoft Edge-Renderingmodul rendert. In einem Webansichtssteuerelement können auch Links angezeigt und verwendet werden.
title: Webansicht
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 799913688f04421cae6b47f0d4b7db2fb1074ed4
ms.sourcegitcommit: bf95c8b29145a224957a940512394e6aa97cb90f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71061929"
---
# <a name="web-view"></a>Webansicht

Mithilfe eines Webansichtssteuerelements betten Sie eine Ansicht in Ihre App ein, die Webinhalte mit dem Microsoft Edge-Renderingmodul rendert. In einem Webansichtssteuerelement können auch Links angezeigt und verwendet werden.

> **Wichtige APIs:** [WebView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie ein Webansichtssteuerelement zum Anzeigen von grafisch ansprechenden HTML-Inhalten von einem Remotewebserver, dynamisch generiertem Code oder Inhaltsdateien in Ihrem App-Paket. Attraktive Inhalte können auch Skriptcode enthalten und zwischen dem Skript und dem Code Ihrer App kommunizieren.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/WebView">die App zu öffnen und WebView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-web-view"></a>Erstellen einer Webansicht

**Ändern der Darstellung einer Webansicht**

[WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) ist keine [Control](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)-Unterklasse und verfügt daher über keine Steuerelementvorlage. Sie können jedoch verschiedene Eigenschaften festlegen, um einige visuelle Aspekte der Webansicht zu steuern.
- Legen Sie zum Einschränken des Anzeigebereichs die [Width](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)-Eigenschaft und die [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)-Eigenschaft fest. 
- Verwenden Sie zum Verschieben, Skalieren, Neigen und Drehen einer Webansicht die [RenderTransform](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)-Eigenschaft.
- Legen Sie zum Steuern der Deckkraft der Webansicht die [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.opacity)-Eigenschaft fest.
- Legen Sie zum Angeben einer Farbe, die als Webseitenhintergrund verwendet wird, wenn der HTML-Inhalt keine Farbe vorgibt, die [DefaultBackgroundColor](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.defaultbackgroundcolor)-Eigenschaft fest. 

**Abrufen des Webseitentitels**

Sie können den Titel des derzeit in der Webansicht angezeigten HTML-Dokuments mithilfe der [DocumentTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.documenttitle)-Eigenschaft abrufen. 

**Eingabeereignisse und Aktivierreihenfolge**

Obwohl WebView keine Control-Unterklasse ist, erhält sie den Tastatureingabefokus und ist Teil der Aktivierreihenfolge. Sie stellt eine [Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.focus)-Methode sowie ein [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)-Ereignis und ein [LostFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)-Ereignis bereit, enthält aber keine registerkartenbezogenen Eigenschaften. Ihre Position in der Aktivierreihenfolge ist identisch mit ihrer Position in der XAML-Dokumentreihenfolge. Die Aktivierreihenfolge enthält alle Elemente im Webansichtsinhalt, die den Eingabefokus erhalten können. 

Wie in der Events-Tabelle auf der Seite zur [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)-Klasse angegeben, werden die meisten von [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) geerbten Benutzereingabeereignisse (z. B. [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown), [KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) und [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)) von der Webansicht nicht unterstützt. Stattdessen können Sie [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) mit der JavaScript-Funktion **eval** verwenden, um die HTML-Ereignishandler zu nutzen, und **window.external.notify** aus dem HTML-Ereignishandler, um die Anwendung mithilfe von [WebView.ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) zu benachrichtigen.

### <a name="navigating-to-content"></a>Navigieren zum Inhalt

Die Webansicht bietet mehrere APIs für die grundlegende Navigation: [GoBack](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.goback), [GoForward](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.goforward), [Stop](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.stop), [Refresh](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.refresh), [CanGoBack](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cangoback) und [CanGoForward](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cangoforward). Mit diesen APIs können Sie Ihrer App typische Funktionen für das Webbrowsen hinzufügen. 

Legen Sie zum Einrichten des anfänglichen Inhalts der Webansicht die [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.source)-Eigenschaft in XAML fest. Der XAML-Parser konvertiert die Zeichenfolge automatisch in einen [Uri](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri). 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

Die Source-Eigenschaft kann grundsätzlich im Code festgelegt werden. In der Regel verwenden Sie jedoch eine der **Navigate**-Methoden, um Inhalt in den Code zu laden. 

Verwenden Sie zum Laden des Webinhalts die [Navigate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigate)-Methode mit einem **Uri**, der das HTTP- oder HTTPS-Schema verwendet. 

```csharp
webView1.Navigate("http://www.contoso.com");
```

Um zu einem URI mit POST-Anforderung und HTTP-Headern zu navigieren, verwenden Sie die [NavigateWithHttpRequestMessage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage)-Methode. Die Methode unterstützt nur [HttpMethod.Post](https://docs.microsoft.com/uwp/api/windows.web.http.httpmethod.post) und [HttpMethod.Get](https://docs.microsoft.com/uwp/api/windows.web.http.httpmethod.get) als Wert der [HttpRequestMessage.Method](https://docs.microsoft.com/uwp/api/windows.web.http.httprequestmessage.method)-Eigenschaft. 

Um nicht komprimierten und unverschlüsselten Inhalt aus den Datenspeichern [LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder) oder [TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder) der App zu laden, verwenden Sie die **Navigate**-Methode mit einem **Uri**, der das [ms-appdata scheme](/windows/uwp/app-resources/uri-schemes) verwendet. Damit die Webansicht für dieses Schema unterstützt wird, müssen Sie Ihren Inhalt in einem Unterordner des lokalen oder temporären Ordners platzieren. Dies ermöglicht die Navigation zu URIs wie „ms-appdata:///local/*folder*/*file*.html“ und „ms-appdata:///temp/*folder*/*file*.html“. (Informationen zum Laden komprimierter oder verschlüsselter Dateien finden Sie unter [NavigateToLocalStreamUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri).) 

Jeder dieser Unterordner auf oberster Ebene ist vom Inhalt anderer Unterordner auf oberster Ebene isoliert. Beispielsweise können Sie zu „ms-appdata:///temp/Ordner1/Datei.html“ navigieren, aber keinen Link zu „ms-appdata:///temp/Ordner2/Datei.html“ in die Datei aufnehmen. Sie können aber trotzdem eine Verknüpfung mit HTML-Inhalt im App-Paket erstellen, indem Sie das **Schema „ms-appx-web“** verwenden, und mit Webinhalt, indem Sie die URI-Schemas **http** und **https** verwenden.

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

Verwenden Sie zum Laden von Inhalt aus Ihrem App-Paket die **Navigate**-Methode mit einem **Uri**, der das [Schema „ms-appx-web“](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10)) verwendet. 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

Sie können lokalen Inhalt über einen benutzerdefinierten Resolver laden, indem Sie die [NavigateToLocalStreamUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri)-Methode verwenden. Diese Vorgehensweise ermöglicht erweiterte Szenarien wie das Herunterladen und Zwischenspeichern webbasierter Inhalte für die Offlineverwendung oder das Extrahieren von Inhalten aus einer komprimierten Datei.

### <a name="responding-to-navigation-events"></a>Reagieren auf Navigationsereignisse

Das Webansichtssteuerelement stellt mehrere Ereignisse bereit, mit denen Sie auf Zustände bei der Navigation und beim Laden von Inhalten reagieren können. Die Ereignisse treten für den Stamminhalt der Webansicht in der folgenden Reihenfolge auf: [NavigationStarting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigationstarting), [ContentLoading](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.contentloading), [DOMContentLoaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.domcontentloaded), [NavigationCompleted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigationcompleted)


**NavigationStarting** – tritt ein, bevor die Webansicht zu neuem Inhalt navigiert. Sie können die Navigation in einem Handler für dieses Ereignis abbrechen, indem Sie die WebViewNavigationStartingEventArgs.Cancel-Eigenschaft auf „true“ festlegen. 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** – tritt ein, nachdem die Webansicht mit dem Laden neuer Inhalte begonnen hat. 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** – tritt ein, nachdem die Webansicht die Analyse des aktuellen HTML-Inhalts beendet hat. 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** – Tritt ein, nachdem die Webansicht das Laden des aktuellen Inhalts beendet hat oder ein Navigationsfehler aufgetreten ist. Um festzustellen, ob ein Navigationsfehler aufgetreten ist, überprüfen Sie die [IsSuccess](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess)-Eigenschaft und die [WebErrorStatus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus)-Eigenschaft der [WebViewNavigationCompletedEventArgs](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewNavigationCompletedEventArgs)-Klasse. 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

Ähnliche Ereignisse treten für jeden **iframe** im Webansichtsinhalt in der gleichen Reihenfolge auf: 
- [FrameNavigationStarting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framenavigationstarting) – tritt ein, bevor ein Frame in einer Webansicht zu neuem Inhalt navigiert. 
- [FrameContentLoading](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framecontentloading) – tritt ein, nachdem ein Frame in der Webansicht mit dem Laden neuer Inhalte begonnen hat. 
- [FrameDOMContentLoaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framedomcontentloaded) – tritt ein, nachdem ein Frame in der Webansicht die Analyse seines aktuellen HTML-Inhalts beendet hat. 
- [FrameNavigationCompleted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framenavigationcompleted) – tritt ein, nachdem ein Frame in der Webansicht das Laden seines Inhalts beendet hat. 

### <a name="responding-to-potential-problems"></a>Reagieren auf mögliche Probleme

Sie können auf potenzielle Inhaltsprobleme reagieren, z. B. auf Skripts mit langer Ausführungszeit, von der Webansicht nicht ladbare Inhalte und Warnungen vor unsicherem Inhalt. 

Bei der Ausführung von Skripts scheint die App nicht mehr zu reagieren. Das [LongRunningScriptDetected](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.longrunningscriptdetected)-Ereignis tritt regelmäßig ein, während die Webansicht JavaScript ausführt, und bietet die Möglichkeit zur Unterbrechung des Skripts. Sie stellen die bisherige Ausführungszeit des Skripts mithilfe der [ExecutionTime](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime)-Eigenschaft von [WebViewLongRunningScriptDetectedEventArgs](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewLongRunningScriptDetectedEventArgs) fest. Legen Sie zum Anhalten der Skriptausführung die [StopPageScriptExecution](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution)-Eigenschaft für die Ereignisargumente auf **true** fest. Das angehaltene Skript wird erst wieder ausgeführt, wenn es in einer späteren Navigation erneut in der Webansicht geladen wird. 

Das Webansichtssteuerelement kann keine beliebigen Dateitypen hosten. Wenn versucht wird, Inhalte zu laden, die von der Webansicht nicht gehostet werden können, tritt das [UnviewableContentIdentified](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unviewablecontentidentified)-Ereignis ein. Sie können das Ereignis behandeln und den Benutzer benachrichtigen oder die Datei mithilfe der [Launcher](https://docs.microsoft.com/uwp/api/Windows.System.Launcher)-Klasse an einen externen Browser oder eine andere App umleiten.

Entsprechend tritt das [UnsupportedUriSchemeIdentified](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unsupportedurischemeidentified)-Ereignis ein, wenn ein nicht unterstütztes URI-Schema wie „fbconnect://“ oder „mailto://“ im Webinhalt aufgerufen wird. Sie können das Ereignis so behandeln, dass es ein benutzerdefiniertes Verhalten zeigt, anstatt dem Standard-Systemstartprogramm das Starten des URIs zu erlauben.

[UnsafeContentWarningDisplayingevent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying) tritt ein, wenn die Webansicht eine Warnungsseite für Inhalte anzeigt, die vom SmartScreen-Filter als unsicher gemeldet wurden. Wenn sich der Benutzer entscheidet, die Navigation fortzusetzen, wird bei einer späteren Navigation zu der Seite weder die Warnung angezeigt noch das Ereignis ausgelöst.

### <a name="handling-special-cases-for-web-view-content"></a>Behandeln von Sonderfällen für Webansichtsinhalte

Sie können die [ContainsFullScreenElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelement)-Eigenschaft und das [ContainsFullScreenElementChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelementchanged)-Ereignis verwenden, um Vollbilddarstellungen von Webinhalt (z. B. Videowiedergabe im Vollbildmodus) zu erkennen, darauf zu reagieren und sie zu aktivieren. Mit dem ContainsFullScreenElementChanged-Ereignis können Sie die Größe der Webansicht beispielsweise so ändern, dass sie die gesamte App-Ansicht ausfüllt, oder eine App mit Fenstern in den Vollbildmodus versetzen, wenn die Webnutzung in Vollbild gewünscht wird (siehe das folgende Beispiel).

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

Sie können das [NewWindowRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.newwindowrequested)-Ereignis zur Handhabung von Situationen verwenden, in denen gehosteter Webinhalt die Anzeige eines neuen Fensters (z. B. ein Popupfenster) anfordert. Sie können ein anderes WebView-Steuerelement verwenden, um den Inhalt des angeforderten Fensters anzuzeigen.

Verwenden Sie das [PermissionRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.permissionrequested)-Ereignis zum Aktivieren von Webfeatures, die besondere Funktionen erfordern. Dazu gehören momentan Geolocation, IndexedDB-Speicher sowie Audio- und Videodaten des Benutzers (z. B. von einem Mikrofon oder einer Webcam). Wenn Ihre App auf den Standort oder Medien des Benutzers zugreift, müssen Sie diese Funktion trotzdem im App-Manifest deklarieren. Für eine App, die Geolocationdaten verwendet, müssen mindestens die folgenden Funktionen in „Package.appxmanifest“ deklariert werden:

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

Zusätzlich zur App, die das [PermissionRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.permissionrequested)-Ereignis behandelt, muss der Benutzer Standard-Systemdialogfelder für Apps genehmigen, die Standort- oder Medienfunktionen anfordern. Erst dann können diese Features aktiviert werden.

Das folgende Beispiel zeigt, wie eine App Geolocation in einer Bing-Karte aktivieren würde:

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

Wenn Ihre App eine Benutzereingabe oder andere asynchrone Vorgänge erfordert, um auf eine Berechtigungsanforderung zu reagieren, erstellen Sie mithilfe der [Defer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer)-Methode von [WebViewPermissionRequest](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewPermissionRequest) eine [WebViewDeferredPermissionRequest](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewDeferredPermissionRequest), die zu einem späteren Zeitpunkt Gegenstand einer Aktion sein kann. Weitere Informationen finden Sie unter [WebViewPermissionRequest.Defer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer). 

Wenn sich die Benutzer sicher von einer Website in einer Webansicht gehosteten Website abmelden müssen oder in anderen Fällen, in denen Sicherheit wichtig ist, rufen Sie die statische Methode [ClearTemporaryWebDataAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cleartemporarywebdataasync) auf, um den gesamten lokal zwischengespeicherten Inhalt aus einer Webansichtssitzung zu löschen. Dadurch werden böswillige Benutzer am Zugriff auf vertrauliche Daten gehindert. 

### <a name="interacting-with-web-view-content"></a>Interaktion mit Webansichtsinhalten

Sie können mit dem Inhalt der Webansicht interagieren, indem Sie mithilfe der [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync)-Methode Skripts aufrufen oder in den Webansichtsinhalt einfügen, und über das [ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify)-Ereignis Informationen aus dem Webansichtsinhalt zurückerhalten.

Verwenden Sie zum Aufrufen von JavaScript im Webansichtsinhalt die [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync)-Methode. Das aufgerufene Skript kann nur Zeichenfolgenwerte zurückgeben. 

Wenn der Inhalt einer Webansicht namens `webView1` eine Funktion namens `setDate` enthält, die drei Parameter akzeptiert, können Sie sie wie folgt aufrufen. 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


Sie können **InvokeScriptAsync** mit der JavaScript-Funktion **eval** verwenden, um Inhalt in die Webseite einzufügen.

In diesem Fall wird der Text eines XAML-Textfeld (`nameTextBox.Text`) in ein div-Element einer in `webView1` gehosteten HTML-Seite geschrieben. 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Skripts im Webansichtsinhalt können **window.external.notify** mit einem Zeichenfolgenparameter verwenden, um Informationen zurück an Ihre App zu senden. Behandeln Sie zum Empfangen dieser Nachrichten das [ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify)-Ereignis. 

Damit eine externe Webseite das **ScriptNotify**-Ereignis beim Aufrufen von „window.external.notify“ auslösen kann, müssen Sie den URI der Seite in den **ApplicationContentUriRules**-Abschnitt des App-Manifests einfügen. (Sie können diesen Vorgang in Microsoft Visual Studio auf der Registerkarte „Inhalts-URIs“ des Package.appxmanifest-Designers ausführen.) Die URIs in dieser Liste müssen HTTPS verwenden, und sie können Unterdomänenplatzhalter enthalten (z. B. `https://*.microsoft.com`), sie dürfen jedoch keine Domänenplatzhalter (z. B. `https://*.com` und `https://*.*`) enthalten. Die Manifestanforderung gilt nicht für Inhalte, die aus dem App-Paket stammen, die einen URI vom Typ „ms-local-stream:// URI“ verwenden, oder die mit [NavigateToString](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetostring) geladen werden. 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>Zugreifen auf die Windows-Runtime in einer Webansicht

Sie können die [AddWebAllowedObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject)-Methode verwenden, um eine Instanz einer systemeigenen Klasse aus einer Komponente für Windows-Runtime in den JavaScript-Kontext der Webansicht einzufügen. Dadurch wird der vollständige Zugriff auf die systemeigenen Methoden, Eigenschaften und Ereignisse des Objekts im JavaScript-Inhalt der betreffenden Webansicht gewährleistet. Die Klasse muss mit dem [AllowForWeb](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.AllowForWebAttribute)-Attribut versehen werden. 

Durch den folgenden Code wird beispielsweise eine Instanz von `MyClass` eingefügt, die aus einer Komponente für Windows-Runtime in eine Webansicht importiert wurde.

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

Weitere Informationen finden Sie unter [WebView.AddWebAllowedObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject). 

Zusätzlich kann vertrauenswürdigem JavaScript-Inhalt in einer Webansicht gestattet werden, direkt auf Windows-Runtime-APIs zuzugreifen. Dadurch werden leistungsfähige systemeigene Funktionen für in einer Webansicht gehostete Web-Apps bereitgestellt. Um dieses Feature zu aktivieren, muss der URI für vertrauenswürdigen Inhalt in der ApplicationContentUriRules-Whitelist der App in „Package.appxmanifest“ aufgeführt sein, wobei WindowsRuntimeAccess ausdrücklich auf „all“ festgelegt ist. 

Dieses Beispiel zeigt einen Abschnitt des App-Manifests. Hier wird einem lokalen URI Zugriff auf die Windows-Runtime gewährt. 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### <a name="options-for-web-content-hosting"></a>Optionen für das Hosten von Webinhalten

Mit der [WebView.Settings](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.settings)-Eigenschaft (des Typs [WebViewSettings](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewSettings)) können Sie steuern, ob JavaScript und IndexedDB aktiviert werden. Wenn Sie beispielsweise eine Webansicht zur Anzeige streng statischen Inhalts verwenden, können Sie JavaScript deaktivieren, um die Leistung zu optimieren.

### <a name="capturing-web-view-content"></a>Erfassen von Webansichtsinhalten

Um die Freigabe von Webansichtsinhalten für andere Apps zu aktivieren, verwenden Sie die [CaptureSelectedContentToDataPackageAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync)-Methode, die den ausgewählten Inhalt als [DataPackage](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) zurückgibt. Diese Methode ist asynchron. Sie müssen also eine Verzögerung verwenden, um Rückgaben des [DataRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)-Ereignishandlers zu verhindern, solange der asynchrone Aufruf nicht abgeschlossen ist. 

Verwenden Sie zum Abrufen eines Vorschaubilds des aktuellen Webansichtsinhalts die [CapturePreviewToStreamAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.capturepreviewtostreamasync)-Methode. Durch diese Methode wird ein Bild des aktuellen Inhalts erstellt und in den angegebenen Datenstrom geschrieben. 

### <a name="threading-behavior"></a>Threadingverhalten

Webansichtsinhalte werden auf Geräten der Desktopgerätefamilie standardmäßig im UI-Thread und auf allen anderen Geräten in anderen Bereichen gehostet. Sie können die statische [WebView.DefaultExecutionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.defaultexecutionmode)-Eigenschaft verwenden, um das Standardthreadingverhalten für den aktuellen Client abzufragen. Wenn erforderlich, können Sie dieses Verhalten mit dem [WebView(WebViewExecutionMode)](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.webview.-ctor#Windows_UI_Xaml_Controls_WebView__ctor_Windows_UI_Xaml_Controls_WebViewExecutionMode_)-Konstruktor überschreiben. 

> **Note**&nbsp;&nbsp;Beim Hosten von Inhalten im UI-Thread mobiler Geräte können Leistungsprobleme auftreten. Wenn Sie DefaultExecutionMode ändern, sollten Sie die Leistung deshalb auf allen Zielgeräten testen.

Eine Webansicht, die Inhalte nicht im UI-Thread hostet, ist nicht mit übergeordneten Steuerelementen kompatibel, die Gesten zur Weitergabe des Webansichtssteuerelements an das übergeordnete Steuerelement (wie [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView), [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)) und andere verwandte Steuerelemente erfordern. Diese Steuerelemente können keine Gesten empfangen, die in der Hintergrundthread-Webansicht initiiert werden. Darüber hinaus wird der Ausdruck von Hintergrundthread-Webinhalten nicht direkt unterstützt. Sie sollten ein Element stattdessen mit der [WebViewBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)-Füllung ausdrucken.

## <a name="recommendations"></a>Empfehlungen


-   Stellen Sie sicher, dass die geladene Website für das Gerät korrekt formatiert wird und die verwendeten Farben sowie die verwendete Typografie und Navigation mit dem Rest der App konsistent sind.
-   Eingabefelder sollten in der Größe angepasst werden. Benutzern ist möglicherweise nicht bewusst, dass sie die Ansicht zum Eingeben von Text vergrößern können.
-   Wenn eine Webansicht nicht wie die restliche App aussieht, ziehen Sie alternative Steuerelemente oder Methoden in Erwägung, um relevante Aufgaben auszuführen. Wenn die Webansicht mit dem Rest der App übereinstimmt, nehmen Benutzer sie als einheitliche Benutzeroberfläche wahr.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Verwandte Themen

- [WebView-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)
