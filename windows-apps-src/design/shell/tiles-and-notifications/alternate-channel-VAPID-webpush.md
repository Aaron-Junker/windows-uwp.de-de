---
title: Alternative Pushkanäle mit VAPID in UWP
description: Anleitungen zum Verwenden alternativer pushkanäle mit dem vapid-Protokoll aus einer Windows-App
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, WinRT-API, WNS
localizationpriority: medium
ms.openlocfilehash: 79ea88cb457e9a0d7ba33ef51a184e6f52ab19c4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219273"
---
# <a name="alternate-push-channels-using-vapid-in-windows"></a>Alternative pushkanäle mit vapid in Windows 
Ab dem Fall Creators Update können Windows-Apps die vapid-Authentifizierung verwenden, um Pushbenachrichtigungen zu senden.  

> [!NOTE]
> Diese APIs sind für Webbrowser gedacht, in denen andere Websites gehostet werden und in deren Namen Kanäle erstellt werden.  Wenn Sie Ihrer Web-App webpush-Benachrichtigungen hinzufügen möchten, empfehlen wir Ihnen, die W3C-und die WHATWG-Standards für die Erstellung eines Service Workers und das Senden einer Benachrichtigung zu befolgen.

## <a name="introduction"></a>Einführung
Durch die Einführung des webpush-Standards können Websites mehr wie apps agieren und Benachrichtigungen senden, auch wenn sich die Benutzer nicht auf der Website befinden.

Das Authentifizierungsprotokoll "vapid" wurde erstellt, um Websites die Authentifizierung mit pushservern auf Hersteller agnostische Weise zu ermöglichen. Bei allen Anbietern, die das vapid-Protokoll verwenden, können Websites Pushbenachrichtigungen senden, ohne den Browser zu kennen, in dem Sie ausgeführt wird. Dies ist eine bedeutende Verbesserung gegenüber der Implementierung eines anderen pushprotokolls für jede Plattform. 

Windows-Apps können mit vapid auch Pushbenachrichtigungen mit diesen Vorteilen senden. Diese Protokolle können Entwicklungszeit für neue apps einsparen und plattformübergreifende Unterstützung für vorhandene apps vereinfachen. Darüber hinaus können Unternehmens-Apps oder Sideload-Apps jetzt Benachrichtigungen senden, ohne sich im Microsoft Store registrieren zu müssen. Hoffentlich werden dadurch neue Möglichkeiten zum Einbinden von Benutzern auf allen Plattformen geöffnet.  

## <a name="alternate-channels"></a>Alternative Kanäle 
In UWP werden diese vapid-Kanäle als alternative Kanäle bezeichnet und bieten eine ähnliche Funktionalität wie ein webpush-Channel. Sie können eine APP-Hintergrundaufgabe für die Ausführung, die Aktivierung der Nachrichten Verschlüsselung und die Ermöglichung mehrerer Kanäle aus einer einzelnen App auslöst. Weitere Informationen zum Unterschied zwischen den verschiedenen Kanaltypen finden Sie unter [auswählen des richtigen Kanals](channel-types.md).

Die Verwendung alternativer Kanäle ist eine gute Möglichkeit, um auf Pushbenachrichtigungen zuzugreifen, wenn Ihre APP keinen primären Kanal verwenden kann oder wenn Sie Code zwischen Ihrer Website und App freigeben möchten. Das Einrichten eines Kanals ist einfach und mit jedem vertraut, der zuvor den webpushstandard verwendet oder mit Windows-Pushbenachrichtigungen gearbeitet hat.

## <a name="code-example"></a>Codebeispiel

Der grundlegende Prozess der Einrichtung eines alternativen Kanals für eine Windows-App ähnelt dem Einrichten eines primären oder sekundären Kanals. Registrieren Sie sich zunächst für einen Kanal mit dem [WNS-Server](windows-push-notification-services--wns--overview.md). Registrieren Sie sich dann, um als Hintergrundaufgabe auszuführen. Nachdem die Benachrichtigung gesendet und die Hintergrundaufgabe ausgelöst wurde, behandeln Sie das-Ereignis.  

### <a name="get-a-channel"></a>Kanal erhalten 
Um einen alternativen Kanal zu erstellen, muss die APP zwei Informationen bereitstellen: den öffentlichen Schlüssel für den Server und den Namen des zu erstellenden Kanals. Die Details zu den Server Schlüsseln sind in der webpushspezifikation verfügbar. es wird jedoch empfohlen, eine Standardweb-pushbibliothek auf dem Server zu verwenden, um die Schlüssel zu generieren.  

Die Kanal-ID ist besonders wichtig, da eine APP mehrere alternative Kanäle erstellen kann. Jeder Kanal muss durch eine eindeutige ID identifiziert werden, die in alle an diesem Kanal gesendeten Benachrichtigungs Nutzlasten eingeschlossen wird.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
Die APP sendet den Kanal an den Server zurück und speichert Sie lokal. Wenn die Kanal-ID lokal gespeichert wird, kann die APP zwischen Kanälen unterscheiden und Kanäle erneuern, um zu verhindern, dass Sie abläuft.

Wie bei jeder anderen Art von Push-Benachrichtigungs Kanal können Webchannels ablaufen. Wenn Sie verhindern möchten, dass Kanäle ablaufen, ohne dass Ihre APP bekannt ist, erstellen Sie jedes Mal einen neuen Kanal, wenn Ihre APP gestartet wird.    

### <a name="register-for-a-background-task"></a>Registrieren für eine Hintergrundaufgabe 

Nachdem Ihre APP einen alternativen Kanal erstellt hat, sollte Sie sich registrieren, um die Benachrichtigungen entweder im Vordergrund oder im Hintergrund zu empfangen. Im folgenden Beispiel wird registriert, um das Modell mit einem Prozess zu verwenden, um die Benachrichtigungen im Hintergrund zu empfangen.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>Benachrichtigungen empfangen 

Nachdem die APP für den Empfang der Benachrichtigungen registriert wurde, muss Sie in der Lage sein, eingehende Benachrichtigungen zu verarbeiten. Da eine einzelne APP mehrere Kanäle registrieren kann, sollten Sie die Kanal-ID vor der Verarbeitung der Benachrichtigung überprüfen.  

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

Beachten Sie, dass die Kanal-ID nicht festgelegt wird, wenn die Benachrichtigung von einem primären Kanal stammt.  

## <a name="note-on-encryption"></a>Hinweis zur Verschlüsselung 

Sie können jedes beliebige Verschlüsselungsschema verwenden, das für Ihre APP nützlicher ist. In einigen Fällen genügt es, sich auf den TLS-Standard zwischen dem Server und jedem Windows-Gerät zu verlassen. In anderen Fällen ist es möglicherweise sinnvoller, das Web-Push-Verschlüsselungsschema oder ein anderes Schema des Entwurfs zu verwenden.  

Wenn Sie eine andere Form der Verschlüsselung verwenden möchten, ist der Schlüssel die Verwendung des RAW-Werts. Headers-Eigenschaft. Sie enthält alle Verschlüsselungs Header, die in der Post-Anforderung an den Push-Server eingeschlossen wurden. Von dort aus kann Ihre APP die Schlüssel verwenden, um die Nachricht zu entschlüsseln.  

## <a name="related-topics"></a>Zugehörige Themen
- [Benachrichtigungskanaltypen](channel-types.md)
- [Windows-Pushbenachrichtigungsdienste (WNS)](windows-push-notification-services--wns--overview.md)
- [Pushnotificationchannel-Klasse](/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [Pushnotificationchannelmanager-Klasse](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)
