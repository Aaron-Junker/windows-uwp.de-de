---
title: Webauthentifizierungsbroker
description: In diesem Artikel wird erläutert, wie Ihre App für die universelle Windows-Plattform (UWP) eine Verbindung mit einem Onlineidentitätsanbieter herstellen kann, der Authentifizierungsprotokolle wie OpenID oder OAuth verwendet, z. B. Twitter, Facebook, Flickr, Instagram usw.
ms.assetid: 05F06961-1768-44A7-B185-BCDB74488F85
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, Uwp, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: d354f0babec3ec2346c6e76fcae8666f40f3f6be
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "4468387"
---
# <a name="web-authentication-broker"></a>Webauthentifizierungsbroker




In diesem Artikel wird erläutert, wie Ihre App für die universelle Windows-Plattform (UWP) eine Verbindung mit einem Onlineidentitätsanbieter herstellen kann, der Authentifizierungsprotokolle wie OpenID oder OAuth verwendet, z. B. Twitter, Facebook, Flickr, Instagram usw. Die [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066)-Methode sendet eine Anforderung an den Onlineidentitätsanbieter und erhält als Antwort ein Zugriffstoken, das die für die App zugänglichen Anbieterressourcen beschreibt.

>[!NOTE]
>Um ein vollständiges, funktionsfähiges Codebeispiel zu erhalten, klonen Sie [WebAuthenticationBroker-Repository auf GitHub](http://go.microsoft.com/fwlink/p/?LinkId=620622).

 

## <a name="register-your-app-with-your-online-provider"></a>Registrieren der App beim Onlineanbieter


Sie müssen Ihre App bei dem Onlineidentitätsanbieter registrieren, mit dem Sie eine Verbindung herstellen möchten. Informationen zur Registrierung erhalten Sie vom Identitätsanbieter. Nach der Registrierung weist der Onlineanbieter Ihnen normalerweise eine ID oder einen geheimen Schlüssel für Ihre App zu.

## <a name="build-the-authentication-request-uri"></a>Erstellen des Authentifizierungsanforderungs-URIs


Der Anforderungs-URI besteht aus der Adresse, unter der Sie die Authentifizierungsanforderung an den Onlineanbieter senden, und angefügten anderen benötigten Informationen, z.B. eine App-ID oder ein Schlüssel, ein Umleitungs-URI, zu dem der Benutzer nach Abschluss der Authentifizierung umgeleitet wird, und der erwartete Antworttyp. Informationen zu den erforderlichen Parametern erhalten Sie von Ihrem Anbieter.

Der Anforderungs-URI wird als *requestUri*-Parameter der [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066)-Methode gesendet. Es muss sich dabei um eine sichere Adresse (beginnend mit `https://`) handeln.

Das folgende Beispiel zeigt, wie der Anforderungs-URI erstellt wird.

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>&scope=<scopes>&response_type=token";
string endURL = "http://<appendpoint>";

System.Uri startURI = new System.Uri(startURL);
System.Uri endURI = new System.Uri(endURL);
```

## <a name="connect-to-the-online-provider"></a>Herstellen einer Verbindung mit dem Onlineanbieter


Um eine Verbindung mit dem Onlineidentitätsanbieter herzustellen und ein Zugriffstoken abzurufen, rufen Sie die [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066)-Methode auf. Für die Methode wird der im vorhergehenden Schritt erstellte URI als *requestUri*-Parameter und ein URI, zu dem der Benutzer umgeleitet werden soll, als *callbackUri*-Parameter angegeben.

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI, 
        endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

>[!WARNING]
>Zusätzlich zu [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) enthält der [**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044)-Namespace eine [**AuthenticateAndContinue**](https://msdn.microsoft.com/library/windows/apps/dn632425)-Methode. Rufen Sie diese Methode nicht auf. Sie ist für Apps vorgesehen, die auf Windows Phone 8.1 ausgerichtet sind und ab Windows 10 veraltet.

## <a name="connecting-with-single-sign-on-sso"></a>Herstellen einer Verbindung mit einmaligen Anmelden (Single Sign-On, SSO).


Der Webauthentifizierungsbroker lässt das Beibehalten von Cookies standardmäßig nicht zu. Daher muss sich der App-Benutzer jedes Mal anmelden, wenn er auf Ressourcen für diesen Anbieter zugreifen möchte – auch wenn er angibt, dass er angemeldet bleiben möchte (z.B. durch Aktivieren eines Kontrollkästchens im Anmeldedialogfeld des Anbieters). Um die Anmeldung mit Single Sign-On (SSO) durchführen zu können, muss der Onlineidentitätsanbieter SSO für den Webauthentifizierungsbroker aktiviert haben, und Ihre App muss die Überladung von [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212068) aufrufen, für die kein *callbackUri*-Parameter angegeben wird. Auf diese Weise können persistente Cookies vom Webauthentifizierungsbroker gespeichert werden, so dass durch zukünftige Authentifizierungsaufrufe von derselben App wiederholte Anmeldungen des Benutzers nicht erforderlich sind (der angemeldete Benutzer bleibt solange angemeldet, bis das Zugriffstoken abgelaufen ist).

Zur Unterstützung von SSO muss der Onlineanbieter die Registrierung eines Umleitungs-URIs im Format `ms-app://<appSID>` zulassen (`<appSID>` ist die SID für Ihre App). Die SID Ihrer App finden Sie auf der App-Entwicklerseite Ihrer App. Sie können auch die [**GetCurrentApplicationCallbackUri**](https://msdn.microsoft.com/library/windows/apps/br212069)-Methode aufrufen, um die SID festzustellen.

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

## <a name="debugging"></a>Debuggen


Es gibt verschiedene Möglichkeiten, die Problembehandlung für die Webauthentifizierungsbroker-APIs durchzuführen. Beispiele hierfür sind die Untersuchung von Betriebsprotokollen und die Untersuchung von Webanforderungen und -antworten mit Fiddler.

### <a name="operational-logs"></a>Betriebsprotokolle

Häufig können Sie anhand der Betriebsprotokolle ermitteln, was nicht funktioniert. Dank des dedizierten Ereignisprotokollkanals „Microsoft-Windows-WebAuth\\Operational“ können Websiteentwickler nachvollziehen, wie ihre Webseiten vom Webauthentifizierungsbroker verarbeitet werden. Starten Sie zur Aktivierung des Brokers „eventvwr.exe“, und aktivieren Sie das Betriebsprotokoll unter „Anwendungen und Dienste\\Microsoft\\Windows\\WebAuth“. Darüber hinaus hängt der Webauthentifizierungsbroker eine eindeutige Zeichenfolge an die Zeichenfolge des Benutzer-Agents an, um sich selbst auf dem Webserver zu identifizieren. Die Zeichenfolge lautet „MSAuthHost/1.0“. Beachten Sie, dass sich die Versionsnummer ändern kann. Verlassen Sie sich also in Ihrem Code nicht auf diese Versionsnummer. Im Folgenden finden Sie ein Beispiel für die vollständigen Zeichenfolge des Benutzer-Agenten, gefolgt von den vollständigen Anweisungen zum Debuggen.

`User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0; MSAuthHost/1.0)`

1.  Aktivieren Sie die Betriebsprotokolle.
2.  Führen Sie die Contoso-App für soziale Netzwerke aus. ![Ereignisanzeige mit webauth-Betriebsprotokollen](images/wab-event-viewer-1.png)
3.  Anhand der generierten Protokolleinträge kann das Verhalten des Webauthentifizierungsbrokers genauer nachvollzogen werden. In diesem Fall können sie Folgendes enthalten:
    -   Navigation – Starten: Protokolliert, wann AuthHost gestartet wird, und enthält Informationen zu den Start- und End-URLs.
    -   ![Veranschaulicht die Details von „Navigation – Starten“"](images/wab-event-viewer-2.png)
    -   Navigation abgeschlossen: Protokolliert den Abschluss des Ladevorgangs einer Webseite.
    -   META-Tag: Protokolliert, wann ein META-Tag auftritt (einschließlich Details).
    -   Navigation – Beenden: Die Navigation wurde vom Benutzer beendet.
    -   Navigationsfehler: AuthHost ermittelt einen Navigationsfehler bei einer URL, einschließlich HttpStatusCode.
    -   Navigationsende: End-URL liegt vor.

### <a name="fiddler"></a>Fiddler

Der Fiddler-Webdebugger kann Apps verwendet werden.

1.  Da AuthHost in einem eigenen app-Container ausgeführt wird, um die VPN-Funktion zu ermöglichen Sie müssen einen Registrierungsschlüssel festlegen: Windows Registry Editor Version 5.00

    **HKEY\_LOCAL\_MACHINE**\\**SOFTWARE**\\**Microsoft**\\**Windows NT**\\**CurrentVersion**\\**Image File Execution Options**\\**authhost.exe**\\**EnablePrivateNetwork** = 00000001

    Wenn Sie diesen Registrierungsschlüssel nicht verfügen, können Sie es in einem Eingabeaufforderungsfenster mit Administratorrechten erstellen.

    ```cmd 
    REG ADD "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe" /v EnablePrivateNetwork /t REG_DWORD /d 1 /f
    ```

2.  Fügen Sie eine Regel für AuthHost hinzu, da darüber ausgehender Datenverkehr generiert wird.
    ```syntax
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.a.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
    D:\Windows\System32>CheckNetIsolation.exe LoopbackExempt -s
    List Loopback Exempted AppContainers
    [1] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
        SID:  S-1-15-2-1973105767-3975693666-32999980-3747492175-1074076486-3102532000-500629349
    [2] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
        SID:  S-1-15-2-166260-4150837609-3669066492-3071230600-3743290616-3683681078-2492089544
    [3] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.a.p_8wekyb3d8bbwe
        SID:  S-1-15-2-3506084497-1208594716-3384433646-2514033508-1838198150-1980605558-3480344935
    ```

3.  Fügen Sie Fiddler eine Firewallregel für eingehenden Datenverkehr hinzu.