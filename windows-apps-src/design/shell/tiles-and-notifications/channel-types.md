---
Description: Mithilfe des Windows-Pushbenachrichtigungsdiensts (WNS) können Drittanbieterentwickler Popup-, Kachel-, Signalupdates und unformatierte Updates von ihren eigenen Clouddiensten aus senden. Das Senden der Benachrichtigungen kann je nach den Anforderungen der Anwendung unterschiedlich geschehen
title: Auswählen des richtigen Kanaltypen für die Pushbenachrichtigungen
ms.date: 07/07/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 075eaf5c02e5bddb4b87d7e4aaf931cbfde53cdd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616415"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>Auswählen des richtigen Kanaltypen für die Pushbenachrichtigungen

Dieser Artikel behandelt die drei Arten von UWP-Kanälen für die Push-Benachrichtigungen (primär, sekundär und alternativ), über die Sie die Inhalte für Ihre App übermitteln können. 

(Weitere Informationen darüber, wie Sie Pushbenachrichtigungen zu erstellen, finden Sie unter den [Übersicht über die Gruppenrichtlinien-Verwaltungskonsole (Windows Push Notification Services, WNS)](../tiles-and-notifications/windows-push-notification-services--wns--overview.md).) 

## <a name="types-of-push-channels"></a>Pushkanaltypen 

Es gibt drei Arten an Pushkanälen, die zum Senden von Benachrichtigungen an eine UWP-App verwendet werden können. Diese Berichte sind: 

[Primärer Kanal](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) -der "traditionelle" Pushkanal. Kann von jeder App im Store zum Senden von Popupbenachrichtigungen, Kacheln, unformatierten Benachrichtigungen oder Signalbenachrichtigungen verwendet werden (Link für Beschreibungen von Popups/Kacheln/Signalen)

[Sekundärer Kachelkanal](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - wird für Kachel-Updates im Pushverfahren für eine sekundäre Kachel verwendet. Kann nur zum Senden von Kachel- oder Signalbenachrichtigungen an eine sekundäre Kachel verwendet werden, die auf der Startseite angeheftet ist

[Alternativer Kanal](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) -eine neue Art von Kanal, der im Creators Update hinzugefügt wurde. Sendet unformatierte Benachrichtigungen an alle UWP-Apps, einschließlich derjenigen, die im Store nicht registriert sind. 

> [!NOTE]
> Unabhängig vom verwendeten Pushkanal kann die App, wenn Sie auf dem Gerät ausgeführt, wird immer lokale Popupbenachrichtigungen sowie Benachrichtigungen für Kacheln oder Signalbenachrichtigungen senden. Es können lokale Benachrichtigungen von im Vordergrund ausgeführten App-Prozessen oder über Hintergrundaufgaben gesendet werden. 


## <a name="primary-channels"></a>Primäre Kanäle

Diese Kanäle sind die derzeit am häufigsten verwendeten Kanäle auf Windows und eignen sich für nahezu alle Szenarien, in denen Ihre App über den Microsoft Store verteilt werden soll. Sie können dadurch alle Arten von Benachrichtigungen an die App senden. 

### <a name="what-do-primary-channels-enable"></a>Was aktivieren die primären Kanäle?

-   **Kachel oder eines Badge-Updates an die primäre Kachel gesendet.** Wenn der Benutzer die Kachel an die Startseite angeheftet hat, ist dies eine großartige Möglichkeit, Ihre Fertigkeit zu zeigen. Senden Sie Aktualisierungen mit nützlichen Informationen oder Erinnerungen innerhalb Ihrer App. 
-   **Senden von toastbenachrichtigungen.** Popupbenachrichtigungen sind eine Gelegenheit, dem Benutzer sofort einige Informationen zu unterbreiten. Sie werden von der Shell im Vordergrund der meisten Apps angezeigt sowie Live im Info-Center, so dass der Benutzer darauf zurückkehren und später mit ihnen interagieren kann. 
-   **Senden von unformatierten Benachrichtigungen zum Auslösen einer Hintergrundaufgabe.** Manchmal möchten Sie basierend auf einer Benachrichtigung einige Aktionen für den Benutzer ausführen. Unformatiert Benachrichtigungen ermöglichen das Ausführen der Hintergrundaufgaben Ihrer Apps 
-   **Verwenden Sie die Verschlüsselung von Nachrichten, während der Übertragung von Windows mithilfe von TLS bereitgestellt.** Nachrichten werden bei der Übertragung auf der Leitung beim Eingang auf WNS und beim Ausgang auf das Gerät des Benutzers verschlüsselt.  

### <a name="limitations-of-primary-channels"></a>Einschränkungen der primären Kanäle

-   Erfordert WNS REST API, um Pushbenachrichtigungen zu senden, was bei Geräteherstellern kein Standard ist. 
-   Pro App kann nur ein Kanal erstellt werden 
-   Dabei muss die App im Microsoft Store registriert sein

### <a name="creating-a-primary-channel"></a>Erstellen eines primären Kanals 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>Sekundärer Kachelkanal

Hierbei handelt es sich um Kanäle, die Kachel- und Signalaktualisierungen über eine Pushbenachrichtigung an eine sekundäre Kachel senden. Diese werden von Apps zum Benachrichtigen der Benutzer über interessante Aktivitäten oder Informationen genutzt, mit denen sie in der App interagieren können, wie neue Nachrichten in einem Gruppenchat oder aktualisierte Sportergebnisse. 

### <a name="what-do-secondary-tile-channels-enable"></a>Was aktivieren sekundäre Kachelkanäle?

-   Kachel- oder Signalbenachrichtigungen an eine sekundäre Kachel senden. Sekundäre Kacheln sind eine hervorragende Möglichkeit, Benutzer wieder auf Ihre App zu locken. Sie sind ein Deep-Link für interessante Informationen und das Platzieren relevanter Informationen über die Kacheln lässt diese immer wieder auftauchen.
-   Trennung von Kanälen (und abgelaufenen Objekten) zwischen den verschiedenen Kacheln. Dadurch können Sie die Logik im Back-End-zwischen den verschiedenen Typen von sekundären Kacheln trennen, die ein Benutzer an ihre Startseite anheften kann. 
-   Nachrichtenverschlüsselung während der Übertragung durch Windows mithilfe von TLS. Nachrichten werden bei der Übertragung auf der Leitung beim Eingang auf WNS und beim Ausgang auf das Gerät des Benutzers verschlüsselt.  

### <a name="limitations-of-secondary-tile-channels"></a>Einschränkungen der sekundären Kachelkanäle

-   Popup- oder unformatierte Benachrichtigungen sind nicht zulässig. Popup- oder unformatierte Benachrichtigungen, die an eine sekundäre Kachel gesendet werden, werden vom System ignoriert.
-   Dabei muss die App im Microsoft Store registriert sein


### <a name="creating-a-secondary-tile-channel"></a>Erstellen eines sekundären Kachelkanals 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>Alternative Kanäle

Alternativen Kanäle ermöglichen Apps, Pushbenachrichtigungen zu senden, ohne beim Microsoft Store registriert zu sein oder Pushkanäle außerhalb des primären Kanals zu verwendet, der für die App verwendet wird. 
 
### <a name="what-do-alternate-channels-enable"></a>Was aktivieren alternative Kanäle?
-   Senden von unformatierten Pushbenachrichtigungen an eine UWP, die auf jedem Windows-Gerät ausgeführt wird. Alternative Kanäle werden nur für unformatierte Benachrichtigungen verwendet.
-   Damit können Apps mehrere unformatierte Pushkanäle für verschiedene Features innerhalb der App erstellen. Eine App kann bis zu 1.000 alternativen Kanäle erstellen und ist jeweils 30 Tage lang gültig. Jeder dieser Kanäle kann separat verwaltet oder von der App widerrufen werden.
-   Alternative Pushkanäle können erstellt werden, ohne die App im Microsoft Store zu registrieren. Wenn Ihre App auf Geräten ohne Registrierung im Microsoft Store installiert werden soll, kann sie auch weiterhin Pushbenachrichtigungen empfangen.
-   Server können mit W3C-Standard-REST-APIs und dem VAPID Protokoll Pushbenachrichtigungen senden. Alternative Kanäle nutzen das W3C-Standard-Protokoll. Dadurch können Sie die Serverlogik vereinfachen, die verwaltet werden muss.
-   Vollständige Verschlüsselung von End-to-End-Nachrichten. Der primäre Kanal stellt Verschlüsselung während der Übertragung bereit. Wenn Sie eine zusätzliche Sicherheit wünschen, ermöglichen alternative Kanäle Ihrer App durch verschlüsselte Header eine Nachricht geschützt zu übertragen. 

### <a name="limitations-of-alternate-channels"></a>Einschränkungen alternativer Kanäle
-   Apps können keine Popup-, Kachel- oder Signalpushbenachrichtigungen übertragen. Der alternative Kanal beschränkt die Möglichkeit, andere Arten der Benachrichtigung zu senden. Ihre App kann auch weiterhin lokale Benachrichtigungen über Ihre Hintergrundaufgabe senden. 
-   Erfordert eine andere REST-API als die primären oder sekundären Kachelkanäle. Die Verwendung der standardmäßigen W3C-REST-API bedeutet, dass Ihre App eine andere Logik für das Senden von Push-, Kachel- oder Popupbenachrichtigungen verwendet werden muss

### <a name="creating-an-alternate-channel"></a>Erstellen eines alternativen Kanals 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>Vergleich von Kanaltypen
Hier ist eine kurze Gegenüberstellung der unterschiedlichen Arten von Kanälen:

<table>

<tr class="header">
<th align="left"><b>Typ</b></th>
<th align="left"><b>Mithilfe von Push übertragen Popup?</b></th>
<th align="left"><b>Migrieren Sie die Kachel "WNS/Badge?</b></th>
<th align="left"><b>Pushbenachrichtigungen unformatierte?</b></th>
<th align="left"><b>Authentifizierung</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>Store-Registrierung erforderlich?</b></th>
<th align="left"><b>Kanäle</b></th>
<th align="left"><b>Verschlüsselung</b></th>
</tr>


<tr class="odd">
<td align="left">Primär</td>
<td align="left">Ja</td>
<td align="left">Ja – nur primäre Kachel</td>
<td align="left">Ja</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">Ja</td>
<td align="left">Einer pro App</td>
<td align="left">Während der Übertragung</td>
</tr>
<tr class="even">
<td align="left">Sekundäre Kachel</td>
<td align="left">Nein</td>
<td align="left">Ja – nur sekundäre Kachel</td>
<td align="left">Nein</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">Ja</td>
<td align="left">Einer pro sekundärer Kachel</td>
<td align="left">Während der Übertragung</td>
</tr>
<tr class="odd">
<td align="left">Alternativ</td>
<td align="left">Nein</td>
<td align="left">Nein</td>
<td align="left">Ja</td>
<td align="left">VAPID</td>
<td align="left">WebPush W3C-Standard</td>
<td align="left">Nein</td>
<td align="left">1.000 Pro App</td>
<td align="left">Während der Übertragung + End-to-End-Verschlüsselung ist mit Pass-Through-Header möglich (erfordert einen App-Code)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>Auswählen des richtigen Kanals

Im Allgemeinen wird empfohlen, den primären Kanal in Ihrer App zu verwenden – mit einigen Ausnahmen: 

1. Wenn Sie eine Kachelaktualisierung an eine sekundäre Kachel senden, verwenden Sie den sekundären Kachel-Pushkanal.
2. Wenn Sie Kanäle an andere Dienste übergeben (z. B. an einen Browser), verwenden Sie den alternativen Kanal.
3. Wenn Sie eine App erstellen, die nicht im Windows Store aufgeführt wird (z. B. eine Branchen-App) verwenden Sie einen alternativen Kanal.
4. Wenn Sie einen vorhandenen Web-Benachrichtigungscode auf dem Server haben, den Sie wiederverwenden möchten oder wenn Sie mehrere Kanäle in Ihrem Back-End-Dienst benötigen, verwenden Sie alternative Kanäle.

## <a name="related-articles"></a>Verwandte Artikel

* [Senden einer lokalen Kachelbenachrichtigung](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Adaptive und interaktive Popupbenachrichtigungen](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [Schnellstart: Eine Pushbenachrichtigung senden](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [Vorgehensweise beim Aktualisieren eines Badges durch Pushbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Wie Sie anfordern, erstellen und speichern einen Benachrichtigungskanal](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [Gewusst wie: Abfangen von Benachrichtigungen für die Ausführung von Anwendungen](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Gewusst wie: authentifizieren mit den Windows Push Notification Service (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [Push Notification Service Anforderungs- und Antwortheader](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [Richtlinien und Prüfliste für erste Schritte mit Pushbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Unformatierte Benachrichtigungen](raw-notification-overview.md)
