---
title: Alternative Push-Kanäle mit Webpush VAPID auf UWP
description: Erfahren Sie, wie mithilfe von alternativen Push-Kanälen mit dem VAPID-Protokoll über eine UWP-app
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, Uwp, WinRT-API, WNS
localizationpriority: medium
ms.openlocfilehash: ba8630a2e877345adeac7eb443dd3e418d3ed277
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639525"
---
# <a name="alternate-push-channels-using-webpush-and-vapid-in-uwp"></a>Alternative Push-Kanäle mit Webpush VAPID auf UWP 
Ab dem Fall Creators Update können können UWP-apps Web Push mit VAPID Authentifizierung Sie Pushbenachrichtigungen senden.  

## <a name="introduction"></a>Einführung
Die Einführung von der Web-Push-Standard ermöglicht, dass Websites wie bei apps, die Senden von Benachrichtigungen, auch wenn der Benutzer auf der Website werden nicht mehr reagieren können.

Das VAPID Authentifizierungsprotokoll wurde entwickelt, um Websites für die Authentifizierung mit Push-Server in einem Anbieter ermöglicht das hostagnostische Weise. Mit aller Hersteller, die mit dem VAPID-Protokoll können Websites Pushbenachrichtigungen senden, ohne Kenntnis des Browsers auf dem er ausgeführt wird. Dies ist eine bedeutende Verbesserung über eine andere Push-Protokoll für jede Plattform implementieren. 

UWP-apps können zum Senden von Pushbenachrichtigungen mit diesen Vorteilen sowie Webpush und VAPID verwenden. Diese Protokolle können die Entwicklungszeit für neue apps zu speichern und plattformübergreifende Unterstützung für vorhandene apps vereinfachen. Darüber hinaus können Unternehmens-apps oder mittels sideload übertragenen apps jetzt Benachrichtigungen senden, ohne Registrierung in der Microsoft Store. Hoffentlich wird dadurch neue Möglichkeiten zum interagieren mit Benutzern auf allen Plattformen geöffnet.  

## <a name="alternate-channels"></a>Alternative Kanäle 
In UWP sind diese VAPID Kanäle alternativen Kanäle und bieten eine ähnliche Funktionalität in einem Web-Push-Kanal. Sie können Auslösen einer app-Hintergrundaufgabe ausführen, aktivieren Sie die Verschlüsselung von Nachrichten und für mehrere Kanäle, über eine einzige app ermöglichen. Weitere Informationen zu den Unterschieden zwischen den anderen Kanal-Typen finden Sie unter [Auswählen der richtigen Channel](channel-types.md).

Mithilfe von anderen Kanälen ist eine hervorragende Möglichkeit, erste Schritte mit Pushbenachrichtigungen zugreifen, wenn Ihre app keinen primären Kanal verwenden kann oder wenn Sie eine Website oder app-Code freigeben möchten. Das Einrichten eines Kanals ist einfach und jemand hat die Web-Push-Standard verwendet oder arbeitet mit Windows-Push-Benachrichtigungen, bevor Sie vertraut.

## <a name="code-example"></a>Codebeispiel

Das grundlegende Verfahren zum Einrichten von einen anderen Kanal für UWP-Apps ähnelt das Einrichten eines primären oder sekundären Kanals. Registrieren Sie sich zuerst für einen Kanal mit der [WNS Server](windows-push-notification-services--wns--overview.md). Registrieren Sie anschließend zur Ausführung als Hintergrundaufgabe ausgeführt. Nachdem die Benachrichtigung wird gesendet, und die Hintergrundaufgabe ausgelöst wird, Behandeln des Ereignisses.  

### <a name="get-a-channel"></a>Abrufen eines Kanals 
Um einen anderen Kanal zu erstellen, muss die app zwei Informationen angeben: der öffentliche Schlüssel für den Server und den Namen des Kanals wird erstellt. Die Details zu den Server-Schlüsseln finden Sie in der Web-Push-Spezifikation, aber es wird empfohlen, eine standard-Web-Push-Bibliothek auf dem Server, die um die Schlüssel zu generieren.  

Die Kanal-ID ist besonders wichtig, da eine app mehrere alternativen Kanäle erstellt werden kann. Jeder Channel muss durch eine eindeutige ID identifiziert werden, die eingeschlossen, mit jeder benachrichtigungsnutzlasten, die diesem Kanal gesendet werden.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
Die app sendet den Kanal Sichern auf einem Server und speichert sie lokal. Speichern lokal die Kanal-ID ermöglicht der app die Unterscheidung zwischen Kanälen und die Kanäle zu erneuern, um diese berichtsserversitzung zu verhindern.

Wie jeden anderen Typ von pushbenachrichtigungskanal ablaufen webkanäle. Um zu verhindern, dass Kanäle ablaufen, ohne zu Ihrer app kennen, erstellen Sie einen neuen Kanal, jedes Mal, wenn Ihre app gestartet wird.    

### <a name="register-for-a-background-task"></a>Registrieren Sie sich für eine Hintergrundaufgabe 

Sobald Ihre app einen anderen Kanal erstellt wurde, sollten sie registrieren, zum Empfangen von Benachrichtigungen in den Vordergrund oder Hintergrund. Im folgenden Beispiel wird registriert, um das Modell mit einer-Prozess zu verwenden, die Benachrichtigungen im Hintergrund zu erhalten.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>Empfangen von Benachrichtigungen 

Sobald die app registriert hat, um die Benachrichtigungen zu erhalten, muss sie eingehenden Benachrichtigungen verarbeiten können. Da eine einzelne app mehrere Kanäle registrieren kann, achten Sie darauf, dass Sie die Kanal-ID zu überprüfen, bevor die Verarbeitung der benachrichtigungs.  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

Beachten Sie, dass die Kanal-ID nicht festgelegt werden, wenn die Benachrichtigung von einem primären Kanal stammen, werden.  

## <a name="note-on-encryption"></a>Notieren Sie sich auf die Verschlüsselung 

Sie können beliebige Verschlüsselungsschema Sie hilfreich für Ihre app finden. In einigen Fällen ist es ausreichend, um auf dem TLS-Standard zwischen dem Server und einem beliebigen Windows-Gerät verlassen. In anderen Fällen kann es das Verschlüsselungsschema des Web-Push, oder ein anderes Schema Ihres Entwurfs mit mehr Sinn.  

Wenn Sie eine andere Form der Verschlüsselung verwenden möchten, ist der Schlüssel die Verwendung der unformatierten an. Header-Eigenschaft. Sie enthält alle Verschlüsselung-Header, die in der POST-Anforderung an den Push-Server enthalten sind. Von dort aus kann Ihre app die Schlüssel zum Entschlüsseln der Nachricht verwenden.  

## <a name="related-topics"></a>Verwandte Themen
- [Benachrichtigung Kanaltypen](channel-types.md)
- [Windows-Pushbenachrichtigungsdienste (WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel-Klasse](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager-Klasse](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


