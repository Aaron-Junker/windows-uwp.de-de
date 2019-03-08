---
title: Web Account Manager
description: In diesem Artikel wird beschrieben, wie Sie AccountsSettingsPane verwenden, um Ihre App für die Universelle Windows-Plattform (UWP) mit externen Identitätsanbietern wie Microsoft oder Facebook zu verbinden. Dazu verwenden Sie die Web Account Manager-APIs in Windows 10.
ms.date: 12/06/2017
ms.topic: article
keywords: Windows 10, Uwp, Sicherheit
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: a0a16ac9a2d810f7f4cbe2be403713b5cec4997b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641025"
---
# <a name="web-account-manager"></a>Web Account Manager

In diesem Artikel wird beschrieben, wie Sie **[AccountsSettingsPane](https://docs.microsoft.com/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** verwenden, um Ihre App für die Universelle Windows-Plattform (UWP) mit externen Identitätsanbietern wie Microsoft oder Facebook zu verbinden. Dazu verwenden Sie die Web Account Manager-APIs in Windows 10. Sie erfahren, wie Sie die Berechtigung eines Benutzers für die Verwendung des Microsoft-Kontos des Benutzers anfordern können, ein Zugriffstoken erhalten und mit diesem grundlegende Vorgänge wie das Abrufen von Profildaten oder das Hochladen von Daten in das OneDrive-Konto des Benutzers durchführen können. Die Schritte ähneln denen, die zum Abrufen einer Benutzerberechtigung und für den Zugriff über einen beliebigen Identitätsanbieter ausgeführt werden, der Web Account Manager unterstützt.

> [!NOTE]
> Ein vollständiges Codebeispiel finden Sie im [WebAccountManagement-Beispiel auf GitHub](https://go.microsoft.com/fwlink/p/?LinkId=620621).

## <a name="get-set-up"></a>Einrichten

Erstellen Sie zunächst eine neue leere App in Visual Studio. 

Um eine Verbindung mit Identitätsanbietern herzustellen, müssen Sie die App als Nächstes mit dem Store verknüpfen. Klicken Sie dazu mit der rechten Maustaste auf das Projekt, wählen Sie **Store** > **App mit Store verknüpfen** aus, und folgen Sie den Anweisungen des Assistenten. 

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

## <a name="show-the-accounts-settings-pane"></a>Anzeigen des Bereichs mit Kontoeinstellungen

Das System bietet mit **AccountsSettingsPane** eine integrierte Benutzeroberfläche für die Verwaltung von Identitätsanbietern und Webkonten. Sie können sie wie folgt anzeigen:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

Wenn Sie Ihre App ausführen und auf die Anmeldeschaltfläche klicken, sollte ein leeres Fenster angezeigt werden. 

![Bereich mit Kontoeinstellungen](images/tb-1.png)

Der Bereich ist leer, weil das System nur eine UI-Shell bereitstellt. Der Entwickler kann den Bereich programmgesteuert mit Identitätsanbietern auffüllen. 

> [!TIP]
> Optional können Sie **[ShowAddAccountAsync](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.accountssettingspane.showaddaccountasync)** anstelle von  **[anzeigen](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.accountssettingspane.show#Windows_UI_ApplicationSettings_AccountsSettingsPane_Show)**, wodurch ein  **[ IAsyncAction](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncAction)**, um den Status des Vorgangs abgefragt. 

## <a name="register-for-accountcommandsrequested"></a>Registrieren für AccountCommandsRequested

Um dem Bereich Befehle hinzuzufügen, führen wir zunächst eine Registrierung für den AccountCommandsRequested-Ereignishandler durch. Dadurch wird das System angewiesen, unsere Buildlogik auszuführen, wenn der Benutzer die Anzeige des Bereichs anfordert (d. h. auf die XAML-Schaltfläche klickt). 

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

Sie beginnen, indem Sie eine Verzögerung abrufen. Dadurch wird das System angewiesen, die Anzeige von **AccountsSettingsPane** so lange zu verzögern, bis der Buildvorgang abgeschlossen ist.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

Als Nächstes rufen Sie einen Anbieter mit der WebAuthenticationCoreManager.FindAccountProviderAsync-Methode ab. Die Anbieter-URL ist je nach Anbieter verschieden und kann in der Anbieterdokumentation nachgeschlagen werden. Für Microsoft-Konten und Azure Active Directory (Azure AD) lautet sie „https://login.microsoft.com“. 

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

Schließlich fügen Sie **AccountsSettingsPane** den Anbieter hinzu, indem Sie wie folgt einen neuen **[WebAccountProviderCommand](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** erstellen: 

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

Die an unseren neuen **WebAccountProviderCommand** übergebene GetMsaToken-Methode ist noch nicht vorhanden (da sie erst im nächsten Schritt erstellt wird). Sie können sie vorläufig als leere Methode hinzufügen.

Führen Sie den vorangehenden Code aus. Der Bereich sollte jetzt wie folgt aussehen: 

![Bereich mit Kontoeinstellungen](images/tb-2.png)

### <a name="request-a-token"></a>Anfordern eines Tokens

Sobald die Option für das Microsoft-Konto in **AccountsSettingsPane** angezeigt wird, müssen wir festlegen, was bei der Auswahl der Option durch den Benutzer passiert. Die GetMsaToken-Methode wurde so registriert, dass sie ausgelöst wird, wenn sich der Benutzer für die Anmeldung mit seinem Microsoft-Konto entscheidet. Deshalb fordern wir an dieser Stelle das Token an. 

Um ein Token anzufordern, verwenden Sie die RequestTokenAsync-Methode wie folgt: 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

In diesem Beispiel wird die Zeichenfolge „wl.basic“ an den Parameter _scope_ übergeben. „scope“ steht für den Typ von Informationen, die Sie vom bereitstellenden Dienst für einen bestimmten Benutzer anfordern. Bestimmte Bereiche ermöglichen nur den Zugriff auf grundlegende Informationen eines Benutzers, z. B. Name und E-Mail-Adresse, während andere Bereiche möglicherweise Zugriff auf vertrauliche Informationen wie die Fotos oder den E-Mail-Posteingang des Benutzers gewähren. Im Allgemeinen sollte Ihre App Berechtigungen in dem Umfang verwenden, der für die Durchführung ihrer Funktion am geringsten erforderlich ist. Dienstanbieter stellen in ihrer Dokumentation Informationen dazu bereit, welche Bereiche erforderlich sind, um die für den betreffenden Dienst zu verwendenden Token anzufordern. 

* Informationen zu Office 365- und Outlook.com-Bereichen finden Sie unter [Authentifizieren von Office 365- und Outlook.com-APIs mit dem v2.0-Authentifizierungsendpunkt](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2). 
* Informationen zu OneDrive-Bereichen finden Sie unter [Authentifizierung und Anmeldung bei OneDrive](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes). 

> [!TIP]
> Optional, wenn Ihre app Anmeldung Hinweis (für das Feld "Benutzer" mit einer Standard-e-Mail-Adresse zu füllen) oder andere spezielle Eigenschaft, die im Zusammenhang mit der anmeldeerfahrung verwendet, führen Sie sie in der **[WebTokenRequest.AppProperties](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** Eigenschaft. Dadurch wird das System, um die Eigenschaft zu ignorieren, beim Zwischenspeichern von der Web-Konto, dem, das Konto-Diskrepanzen in den Cache verhindert wird.

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

Im weiteren Verlauf dieses Artikels wird das MSA-Szenario beschrieben, der Code für AAD ist allerdings sehr ähnlich. Weitere Informationen zu AAD/Graph einschließlich eines vollständigen Beispiels auf GitHub finden Sie in der [Microsoft Graph-Dokumentation](https://graph.microsoft.io/docs/platform/get-started).

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
> Wenn Sie eine Fehlermeldung erhalten, wenn ein Token angefordert haben, stellen Sie sicher, dass Sie Ihre App im Store wie in Schritt 1 beschrieben zugeordnet haben. Ihre App wird nicht in der Lage sein, ein Token abzurufen, wenn Sie diesen Schritt übersprungen haben. 

Sobald Sie im Besitz des Tokens sind, können Sie darüber die API Ihres Anbieters aufrufen. Im folgenden Code wird die [Microsoft Live-API für Benutzerinformationen](https://msdn.microsoft.com/library/hh826533.aspx) aufgerufen, um grundlegende Informationen zum Benutzer zu erhalten und sie auf der Benutzeroberfläche anzuzeigen. Beachten Sie jedoch, dass in den meisten Fällen empfohlen wird, die einmal erhaltenen Tokens zu speichern und sie dann in einer separaten Methode zu verwenden.

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

## <a name="store-the-account-for-future-use"></a>Speichern des Kontos für zukünftige Verwendung

Token sind hilfreich, um sofort Informationen über einen Benutzer abzurufen, in der Regel haben sie jedoch unterschiedliche Ablaufzeiten. MSA-Token sind beispielsweise nur wenige Stunden gültig. Sie müssen **AccountsSettingsPane** jedoch nicht jedes Mal erneut anzeigen, wenn ein Token abläuft. Nachdem Ihre App einmal von einem Benutzer autorisiert wurde, können Sie die Kontoinformationen des Benutzers für die zukünftige Verwendung speichern. 

Zu diesem Zweck verwenden Sie die **[WebAccount](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount)**-Klasse. Ein **WebAccount** wird von derselben Methode zurückgegeben, die Sie verwendet haben, um das Token anzufordern:

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

Sobald Sie über eine **WebAccount**-Instanz verfügen, können Sie es ganz einfach speichern. Im folgenden Beispiel verwenden wir LocalSettings. Weitere Informationen zur Verwendung von LocalSettings und anderen Methoden zum Speichern von Benutzerdaten finden Sie unter [Speichern und Abrufen von App-Einstellungen und -Daten](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

Anschließend können wir eine asynchrone Methode wie die folgende verwenden, um zu versuchen, ein Token im Hintergrund mit dem gespeicherten **WebAccount** abzurufen.

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

Platzieren Sie die oben beschriebene Methode unmittelbar vor den Code, der **AccountsSettingsPane** aufbaut. Wenn das Token im Hintergrund abgerufen wird, muss der Bereich nicht angezeigt werden. 

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
> Im vorangehenden Beispiel sind nur einfache Erfolgs- und Fehlerereignisse dargestellt. Ihre App sollte auch außergewöhnliche Szenarien berücksichtigen (z. B. wenn ein Benutzer die Berechtigung für die App widerruft oder das Konto aus Windows entfernt) und sie angemessen behandeln.  

## <a name="remove-a-stored-account"></a>Entfernen eines gespeicherten Kontos

Wenn Sie ein Webkonto beibehalten, sollten Sie Benutzern die Möglichkeit geben, ihr Konto von Ihrer App zu trennen. Auf diese Weise diese können effektiv "Abmelden" der app: ihre Kontoinformationen werden nicht mehr automatisch beim Start geladen werden. Entfernen Sie dafür zuerst alle gespeicherten Konto- und Anbieterinformationen aus dem Speicher. Rufen Sie dann **[SignOutAsync](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** auf, um den Cache zu löschen und vorhandene App-Tokens für ungültig zu erklären. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>Hinzufügen von Anbietern, die WebAccountManager nicht unterstützen

Wenn Sie die Authentifizierung über einen Dienst in Ihre App integrieren möchten, der Dienst WebAccountManager jedoch nicht unterstützt (z. B. Google+ oder Twitter), können Sie **AccountsSettingsPane** den Anbieter trotzdem manuell hinzufügen. Erstellen Sie dazu ein neues WebAccountProvider-Objekt unter Angabe Ihres eigenen Namens und PNG-Symbols. Fügen Sie das Objekt dann zur WebAccountProviderCommands-Liste hinzu. Stubcode-Beispiel: 

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
> Dadurch wird **AccountsSettingsPane** lediglich ein Symbol hinzugefügt und die Methode ausgeführt, die beim Klicken auf das Symbol ausgeführt werden soll (in diesem Fall GetTwitterTokenAsync). Sie müssen den Code angeben, durch den die eigentliche Authentifizierung behandelt wird. Weitere Informationen finden Sie unter (Webauthentifizierungsbroker)[web-authentication-broker], der Hilfsmethoden für die Authentifizierung mithilfe von REST-Diensten bereitstellt. 

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

![Bereich mit Kontoeinstellungen](images/tb-3.png)

Halten Sie Headertext kurz und einfach, und vermeiden Sie überflüssige Informationen. Wenn der Anmeldevorgang komplex ist und weitere Informationen angezeigt werden müssen, verknüpfen Sie den Benutzer über einen benutzerdefinierten Link mit einer separaten Seite. 

## <a name="add-custom-links"></a>Hinzufügen von benutzerdefinierten Links

Sie können dem AccountsSettingsPane benutzerdefinierte Befehle hinzufügen, die als Links unter den unterstützten WebAccountProviders angezeigt werden. Benutzerdefinierte Befehle eignen sich hervorragend für einfache Aufgaben in Verbindung mit Benutzerkonten, z. B. das Anzeigen einer Datenschutzrichtlinie oder das Öffnen einer Supportseite, wenn auf Benutzerseite ein Problem auftritt. 

Beispiel: 

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

![Bereich mit Kontoeinstellungen](images/tb-4.png)

Einstellungsbefehle lassen sich grundsätzlich überall verwenden. Es wird jedoch empfohlen, diese Art Befehle auf die oben beschriebenen intuitiven Kontoszenarien zu beschränken. 

## <a name="see-also"></a>Siehe auch

[Windows.Security.Authentication.Web.Core-namespace](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Windows.Security.Credentials-namespace](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[AccountsSettingsPane-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Web Authentication broker](web-authentication-broker.md)

[Beispiel für Web-Konto-Verwaltung](https://go.microsoft.com/fwlink/p/?LinkId=620621)

[Mittagessen Aufgabenplanungs-app](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
