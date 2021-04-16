---
title: Web Account Manager
description: In diesem Artikel wird beschrieben, wie Sie accountsSettingsPane verwenden, um Ihre Universelle Windows-Plattform-App (UWP) mit externen Identitätsanbietern wie Microsoft oder Facebook über die Windows 10 Web Account Manager-APIs zu verbinden.
ms.date: 12/06/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: 229f02769c213b1fab04d3694040eb3271f88649
ms.sourcegitcommit: 6cd970686d1ea7176b7e6651f349a14551709820
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107559377"
---
# <a name="web-account-manager"></a>Web Account Manager

In diesem Artikel wird beschrieben, wie Sie **[accountsSettingsPane](/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** verwenden, um Ihre Universelle Windows-Plattform-App (UWP) mit externen Identitätsanbietern wie Microsoft oder Facebook über die Windows 10 Web Account Manager-APIs zu verbinden. Sie erfahren, wie Sie die Berechtigung eines Benutzers anfordern, seine Microsoft-Konto zu verwenden, ein Zugriffstoken abzurufen und es für grundlegende Vorgänge (z. B. Abrufen von Profildaten oder Hochladen von Dateien in ihr OneDrive-Konto) zu verwenden. Die Schritte ähneln denen, die zum Abrufen einer Benutzerberechtigung und für den Zugriff über einen beliebigen Identitätsanbieter ausgeführt werden, der Web Account Manager unterstützt.

> [!NOTE]
> Ein vollständiges Codebeispiel finden Sie im [WebAccountManagement-Beispiel auf GitHub.](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement)

## <a name="get-set-up"></a>Vorbereiten

Erstellen Sie zunächst eine neue leere App in Visual Studio. 

Um eine Verbindung mit Identitätsanbietern herzustellen, müssen Sie die App als Nächstes mit dem Store verknüpfen. Klicken Sie hierzu mit der rechten Maustaste auf Ihr Projekt, wählen Sie **Store/Publish Associate** app with the store (App mit Store verknüpfen/veröffentlichen) aus, und befolgen Sie die  >  Anweisungen des Assistenten. 

Erstellen Sie drittens eine allgemein verwendete Benutzeroberfläche, die eine einfache XAML-Schaltfläche und zwei Textfelder umfasst.

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

Im CodeBehind ist ein Ereignishandler an die Schaltfläche angefügt:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

Fügen Sie zum Schluss die folgenden Namespaces hinzu, damit später keine Verweisprobleme auftreten: 

```csharp
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## <a name="show-the-accounts-settings-pane"></a>Anzeigen des Bereichs "Kontoeinstellungen"

Das System bietet eine integrierte Benutzeroberfläche zum Verwalten von Identitätsanbietern und Webkonten namens **AccountsSettingsPane.** Sie können sie wie folgt anzeigen:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

Wenn Sie Ihre App ausführen und auf die Anmeldeschaltfläche klicken, sollte ein leeres Fenster angezeigt werden. 

![Screenshot des Fensters "Konto auswählen", in dem keine Konten aufgeführt sind](images/tb-1.png)

Der Bereich ist leer, weil das System nur eine UI-Shell bereitstellt. Der Entwickler kann den Bereich programmgesteuert mit Identitätsanbietern auffüllen. 

> [!TIP]
> Optional können Sie **[ShowAddAccountAsync](/uwp/api/windows.ui.applicationsettings.accountssettingspane.showaddaccountasync)** anstelle von **[Show](/uwp/api/windows.ui.applicationsettings.accountssettingspane.show#Windows_UI_ApplicationSettings_AccountsSettingsPane_Show)** verwenden, wodurch eine **[IAsyncAction](/uwp/api/Windows.Foundation.IAsyncAction)** zurückgegeben wird, um den Status des Vorgangs abzufragen. 

## <a name="register-for-accountcommandsrequested"></a>Registrieren für AccountCommandsRequested

Um dem Bereich Befehle hinzuzufügen, führen wir zunächst eine Registrierung für den AccountCommandsRequested-Ereignishandler durch. Dadurch wird das System aufgefordert, unsere Buildlogik ausführen, wenn der Benutzer den Bereich anzeigen möchte (z. B. klickt auf unsere XAML-Schaltfläche). 

Überschreiben Sie im CodeBehind das OnNavigatedTo-Ereignis und das OnNavigatedFrom-Ereignis, und fügen Sie ihnen den folgenden Code hinzu: 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

Da Benutzer nicht sehr häufig mit Konten interagieren, empfiehlt es sich, den Ereignishandler in der beschriebenen Weise zu registrieren bzw. seine Registrierung aufzuheben, um Arbeitsspeicherverluste zu vermeiden. Dadurch wird der angepasste Bereich nur im Arbeitsspeicher vorgehalten, wenn eine hohe Wahrscheinlichkeit vorliegt, dass er vom Benutzer angefordert wird (der Benutzer befindet sich z. B. auf der Seite „Einstellungen“ oder „Anmelden“). 

## <a name="build-the-account-settings-pane"></a>Erstellen des Bereichs mit Kontoeinstellungen

Die BuildPaneAsync-Methode wird immer dann aufgerufen, wenn **AccountsSettingsPane** angezeigt wird. Dort legen wir den Code zur Anpassung der im Bereich angezeigten Befehle ab. 

Sie beginnen, indem Sie eine Verzögerung abrufen. Dies weist das System an, die Anzeige von **AccountsSettingsPane** zu verzögern, bis wir die Entwicklung abgeschlossen haben.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

Als Nächstes rufen Sie einen Anbieter mit der WebAuthenticationCoreManager.FindAccountProviderAsync-Methode ab. Die Anbieter-URL ist je nach Anbieter verschieden und kann in der Anbieterdokumentation nachgeschlagen werden. Für Microsoft-Konten und Azure Active Directory ist dies "https \: ://login.microsoft.com". 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

Beachten Sie, dass auch die Zeichenfolge „consumers“ an den optionalen *authority*-Parameter übergeben wird. Dies liegt daran, dass Microsoft zwei verschiedene Authentifizierungsmethoden bereitstellt: Microsoft-Konten (MSA) für „Heimanwender“ und Azure Active Directory (AAD) für „Organisationen“. Die Autorität „consumers“ gibt an, dass die MSA-Option verwendet werden soll. Wenn Sie eine Unternehmens-App entwickeln, verwenden Sie stattdessen die Zeichenfolge „organizations“.

Fügen Sie schließlich den Anbieter zu **AccountsSettingsPane hinzu, indem** Sie einen neuen **[WebAccountProviderCommand](/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** wie den folgenden erstellen: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

Die GetMsaToken-Methode, die wir an unseren neuen **WebAccountProviderCommand** übergeben haben, ist noch nicht vorhanden (wir erstellen sie im nächsten Schritt). Daher können Sie sie vorerst als leere Methode hinzufügen.

Führen Sie den vorangehenden Code aus. Der Bereich sollte jetzt wie folgt aussehen: 

![Screenshot des Fensters "Konto auswählen" mit aufgelisteten Konten](images/tb-2.png)

### <a name="request-a-token"></a>Anfordern eines Tokens

Sobald die Option Microsoft-Konto in **AccountsSettingsPane** angezeigt wird, müssen wir die Aufgaben behandeln, wenn der Benutzer sie auswählt. Die GetMsaToken-Methode wurde so registriert, dass sie ausgelöst wird, wenn sich der Benutzer für die Anmeldung mit seinem Microsoft-Konto entscheidet. Deshalb fordern wir an dieser Stelle das Token an. 

Um ein Token anzufordern, verwenden Sie die RequestTokenAsync-Methode wie folgt: 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

In diesem Beispiel übergeben wir die Zeichenfolge "wl.basic" an den _scope-Parameter._ „scope“ steht für den Typ von Informationen, die Sie vom bereitstellenden Dienst für einen bestimmten Benutzer anfordern. Bestimmte Bereiche bieten nur Zugriff auf die grundlegenden Informationen eines Benutzers, z. B. Name und E-Mail-Adresse, während andere Bereiche möglicherweise Zugriff auf vertrauliche Informationen gewähren, z. B. fotos oder E-Mail-Posteingang des Benutzers. Im Allgemeinen sollte Ihre App den am wenigsten freizügigen Bereich verwenden, der erforderlich ist, um ihre Funktion zu erreichen. Dienstanbieter stellen Dokumentation zu den Bereichen bereit, die zum Abrufen von Token für die Verwendung mit ihren Diensten erforderlich sind. 

* Informationen zu Microsoft 365 und Outlook.com Bereichen finden Sie unter [Verwenden der Outlook-REST-API (Version 2.0).](/previous-versions/office/office-365-api/api/version-2.0/use-outlook-rest-api) 
* Informationen zu OneDrive-Bereichen finden Sie unter [OneDrive-Authentifizierung und -Anmeldung.](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes) 

> [!TIP]
> Wenn Ihre App optional einen Anmeldehinweis (zum Auffüllen des Benutzerfelds mit einer Standard-E-Mail-Adresse) oder eine andere spezielle Eigenschaft im Zusammenhang mit der Anmeldeerfahrung verwendet, listen Sie ihn in der **[WebTokenRequest.AppProperties-Eigenschaft](/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** auf. Dadurch ignoriert das System die -Eigenschaft beim Zwischenspeichern des Webkontos, wodurch Kontokonflikte im Cache verhindert werden.

Wenn Sie eine Unternehmens-App entwickeln, möchten Sie wahrscheinlich eine Verbindung mit einer Azure Active Directory (AAD)-Instanz herstellen und die Microsoft Graph-API anstelle regulärer MSA-Dienste verwenden. Verwenden Sie in diesem Szenario stattdessen folgenden Code: 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

Im weiteren Verlauf dieses Artikels wird das MSA-Szenario beschrieben, der Code für AAD ist allerdings sehr ähnlich. Weitere Informationen zu AAD/Graph einschließlich eines vollständigen Beispiels auf GitHub finden Sie in der [Microsoft Graph-Dokumentation](https://developer.microsoft.com/graph).

## <a name="use-the-token"></a>Verwenden des Tokens

Die RequestTokenAsync-Methode gibt ein WebTokenRequestResult-Objekt zurück, das die Ergebnisse für Ihre Anforderung enthält. Wenn die Anforderung erfolgreich war, enthält sie ein Token.  

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> [!NOTE]
> Wenn beim Anfordern eines Tokens ein Fehler auftritt, stellen Sie sicher, dass Sie Ihre App dem Store zugeordnet haben, wie in Schritt 1 beschrieben. Ihre App wird nicht in der Lage sein, ein Token abzurufen, wenn Sie diesen Schritt übersprungen haben. 

Sobald Sie im Besitz des Tokens sind, können Sie darüber die API Ihres Anbieters aufrufen. Im folgenden Code rufen wir die [Microsoft Live-API](/office/) für Benutzerinformationen auf, um grundlegende Informationen zum Benutzer abzurufen und auf unserer Benutzeroberfläche anzuzeigen. Beachten Sie jedoch, dass es in den meisten Fällen empfohlen wird, das Token nach dem Abrufen zu speichern und es dann in einer separaten Methode zu verwenden.

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

Die Methode zum Aufrufen verschiedener REST-APIs ist je nach Anbieter verschieden. Informationen zur Verwendung des Tokens finden Sie in der API-Dokumentation des Anbieters. 

## <a name="store-the-account-for-future-use"></a>Speichern des Kontos für die zukünftige Verwendung

Token sind hilfreich, um sofort Informationen über einen Benutzer abzurufen, in der Regel haben sie jedoch unterschiedliche Ablaufzeiten. MSA-Token sind beispielsweise nur wenige Stunden gültig. Glücklicherweise müssen Sie **accountsSettingsPane** nicht jedes Mal erneut anzeigen, wenn ein Token abläuft. Nachdem Ihre App einmal von einem Benutzer autorisiert wurde, können Sie die Kontoinformationen des Benutzers für die zukünftige Verwendung speichern. 

Verwenden Sie hierzu die **[WebAccount-Klasse.](/uwp/api/windows.security.credentials.webaccount)** Ein **WebAccount wird** von derselben Methode zurückgegeben, die Sie zum Anfordern des Tokens verwendet haben:

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

Sobald Sie über eine **WebAccount-Instanz** verfügen, können Sie sie problemlos speichern. Im folgenden Beispiel verwenden wir LocalSettings. Weitere Informationen zur Verwendung von LocalSettings und anderen Methoden zum Speichern von Benutzerdaten finden Sie unter Speichern und Abrufen [von App-Einstellungen und -Daten.](../design/app-settings/store-and-retrieve-app-data.md)

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

Anschließend können wir eine asynchrone Methode wie die folgende verwenden, um zu versuchen, ein Token im Hintergrund mit dem gespeicherten **WebAccount abzurufen.**

```csharp
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

Platzieren Sie die obige Methode direkt vor dem Code, der **accountsSettingsPane erstellt.** Wenn das Token im Hintergrund erhalten wird, muss der Bereich nicht angezeigt werden. 

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    string silentToken = await GetMsaTokenSilentlyAsync();

    if (silentToken != null)
    {
        // the token was obtained. store a reference to it or do something with it here.
    }
    else
    {
        // the token could not be obtained silently. Show the AccountsSettingsPane
        AccountsSettingsPane.Show();
    }
}
```

Da der Tokenabruf im Hintergrund sehr einfach ist, sollten Sie diese Methode verwenden, um Ihr Token zwischen Sitzungen zu aktualisieren, anstatt ein vorhandenes Token zwischenzuspeichern (da das Token jederzeit ablaufen kann).

> [!NOTE]
> Im obigen Beispiel werden nur grundlegende Erfolgs- und Fehlerfälle behandelt. Ihre App sollte auch außergewöhnliche Szenarien berücksichtigen (z. B. wenn ein Benutzer die Berechtigung für die App widerruft oder das Konto aus Windows entfernt) und sie angemessen behandeln.  

## <a name="remove-a-stored-account"></a>Entfernen eines gespeicherten Kontos

Wenn Sie ein Webkonto beibehalten, sollten Sie Ihren Benutzern die Möglichkeit geben, ihre Zuordnung zu Ihrer App zu löschen. Auf diese Weise können sie sich effektiv von der App abmelden: Ihre Kontoinformationen werden beim Start nicht mehr automatisch geladen. Entfernen Sie hierzu zuerst alle gespeicherten Konto- und Anbieterinformationen aus dem Speicher. Rufen Sie dann **[SignOutAsync auf,](/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** um den Cache zu löschen und alle vorhandenen Token ungültig zu machen, die Ihre App möglicherweise hat. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>Hinzufügen von Anbietern, die WebAccountManager nicht unterstützen

Wenn Sie die Authentifizierung von einem Dienst in Ihre App integrieren möchten, aber webAccountManager nicht unterstützt ( z. B. Google+ oder Twitter), können Sie diesen Anbieter weiterhin manuell zu **AccountsSettingsPane hinzufügen.** Erstellen Sie dazu ein neues WebAccountProvider-Objekt, geben Sie Ihren eigenen Namen und das PNG-Symbol an, und fügen Sie es dann der Liste WebAccountProviderCommands hinzu. Stubcode-Beispiel: 

 ```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

> [!NOTE] 
> Dadurch wird nur ein Symbol zu **AccountsSettingsPane** addiert und die Methode ausgeführt, die Sie beim Klicken auf das Symbol angeben (in diesem Fall GetTwitterTokenAsync). Sie müssen den Code angeben, durch den die eigentliche Authentifizierung behandelt wird. Weitere Informationen finden Sie unter [Webauthentifizierungsbroker,](web-authentication-broker.md)der Hilfsmethoden für die Authentifizierung mithilfe von REST-Diensten bietet. 

## <a name="add-a-custom-header"></a>Hinzufügen eines benutzerdefinierten Headers

Sie können den Bereich mit den Kontoeinstellungen mithilfe der HeaderText-Eigenschaft wie folgt anpassen: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![Screenshot des Fensters "Konto auswählen", in dem keine Konten aufgeführt sind, und einer Meldung, die besagt, dass "My Awesome App" am besten funktioniert, wenn Sie angemeldet sind.](images/tb-3.png)

Halten Sie Headertext kurz und einfach, und vermeiden Sie überflüssige Informationen. Wenn der Anmeldevorgang komplex ist und weitere Informationen angezeigt werden müssen, verknüpfen Sie den Benutzer über einen benutzerdefinierten Link mit einer separaten Seite. 

## <a name="add-custom-links"></a>Hinzufügen von benutzerdefinierten Links

Sie können dem AccountsSettingsPane benutzerdefinierte Befehle hinzufügen, die als Links unter den unterstützten WebAccountProviders angezeigt werden. Benutzerdefinierte Befehle eignen sich hervorragend für einfache Aufgaben in Verbindung mit Benutzerkonten, z. B. das Anzeigen einer Datenschutzrichtlinie oder das Öffnen einer Supportseite, wenn auf Benutzerseite ein Problem auftritt. 

Ein Beispiel: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![Screenshot des Fensters "Konto auswählen", in dem keine Konten aufgeführt sind, und einem Link zu einer Datenschutzrichtlinie](images/tb-4.png)

Einstellungsbefehle lassen sich grundsätzlich überall verwenden. Es wird jedoch empfohlen, diese Art Befehle auf die oben beschriebenen intuitiven Kontoszenarien zu beschränken. 

## <a name="see-also"></a>Weitere Informationen

[Windows.Security.Authentication.Web.Core-Namespace](/uwp/api/windows.security.authentication.web.core)

[Windows.Security.Credentials-Namespace](/uwp/api/windows.security.credentials)

[AccountsSettingsPane-Klasse](/uwp/api/windows.ui.applicationsettings.accountssettingspane)

[Webauthentifizierungsbroker](web-authentication-broker.md)

[Beispiel für die Webkontoverwaltung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement)

[Lunch Scheduler-App](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
