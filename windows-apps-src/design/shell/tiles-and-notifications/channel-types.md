---
Description: Mithilfe von Windows Push Notification Services (WNS) können Entwickler von Drittanbietern Popup-, Kachel-, Badge-und rohupdates von Ihrem eigenen clouddienst senden. Es gibt viele Möglichkeiten, die Benachrichtigungen abhängig von den Anforderungen Ihrer Anwendung zu senden.
title: Auswählen des richtigen Kanaltypen für die Pushbenachrichtigungen
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 326012a38f2d4a8cd7d5c406c160db5168c9877d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219223"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>Auswählen des richtigen Kanaltypen für die Pushbenachrichtigungen

In diesem Artikel werden die drei Arten von Windows-pushbenachrichtigungskanälen (primär, Sekundär und Alternative) erläutert, die Sie bei der Bereitstellung von Inhalten für Ihre APP unterstützen. 

(Ausführliche Informationen zum Erstellen von Pushbenachrichtigungen finden Sie in der [Übersicht über Windows Push Notification Services (WNS)](../tiles-and-notifications/windows-push-notification-services--wns--overview.md).) 

## <a name="types-of-push-channels"></a>Typen von Push-Kanälen 

Es gibt drei Arten von Push-Kanälen, die zum Senden von Benachrichtigungen an eine Windows-App verwendet werden können. Sie lauten wie folgt: 

[Primärer Kanal](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) : der "herkömmliche" pushkanal. Kann von jeder App im Store verwendet werden, um Popup-, Kachel-, RAW-oder Badge-Benachrichtigungen zu senden. [Hier erhalten Sie weitere Informationen](windows-push-notification-services--wns--overview.md).

[Sekundärer Kachel Kanal](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) : wird zum Übertragung von Kachel Aktualisierungen auf eine sekundäre Kachel verwendet. Kann nur zum Senden von Kachel-oder Badge-Benachrichtigungen an eine sekundäre Kachel verwendet werden, die auf dem Startbildschirm des Benutzers fixiert ist.

[Alternativer Kanal](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) : ein neuer Kanaltyp, der in der Creators Aktualisierung hinzugefügt wird. Dadurch können unformatierte Benachrichtigungen an jede Windows-App gesendet werden, einschließlich derjenigen, die nicht im Store registriert sind. 

> [!NOTE]
> Unabhängig davon, welcher pushkanal Sie verwenden, ist es immer möglich, lokale Popup-, Kachel-oder Badge-Benachrichtigungen zu senden, sobald die APP auf dem Gerät ausgeführt wird. Sie kann lokale Benachrichtigungen von den Vorgängen der Vordergrund-APP oder von einer Hintergrundaufgabe senden. 


## <a name="primary-channels"></a>Primäre Kanäle

Dies sind derzeit die am häufigsten verwendeten Kanäle unter Windows, die für nahezu jedes Szenario geeignet sind, in dem Ihre APP über die Microsoft Store verteilt wird. Sie ermöglichen es Ihnen, alle Arten von Benachrichtigungen an die APP zu senden. 

### <a name="what-do-primary-channels-enable"></a>Was aktivieren primäre Kanäle?

-   **Das Senden von Kachel-oder Badge-Aktualisierungen an die primäre Kachel.** Wenn der Benutzer die Kachel an den Startbildschirm angeheftet hat, können Sie diese anzeigen. Senden Sie Updates mit nützlichen Informationen oder Erinnerungen in der app. 
-   **Popup Benachrichtigungen werden gesendet.** Popup Benachrichtigungen sind die Möglichkeit, sofort vor dem Benutzer einige Informationen zu erhalten. Sie werden von der Shell über den meisten apps gezeichnet und befinden sich im Aktions Center, sodass der Benutzer später wieder interagieren kann. 
-   **Senden von unformatierten Benachrichtigungen, um eine Hintergrundaufgabe zu initiieren.** Manchmal möchten Sie im Namen des Benutzers auf der Grundlage einer Benachrichtigung einige Aufgaben durchführen. Mit unformatierten Benachrichtigungen können die Hintergrundaufgaben ihrer app ausgeführt werden. 
-   **Nachrichten Verschlüsselung während der Übertragung wird von Windows mithilfe von TLS bereitgestellt.** Nachrichten werden über das Netzwerk verschlüsselt und werden auf das Gerät des Benutzers übertragen.  

### <a name="limitations-of-primary-channels"></a>Einschränkungen von primären Kanälen

-   Erfordert die Verwendung der WNS-Rest-API für Pushbenachrichtigungen, bei denen es sich nicht um Standardgeräte Anbieter handelt. 
-   Pro App kann nur ein Kanal erstellt werden. 
-   Erfordert die Registrierung Ihrer APP im Microsoft Store

### <a name="creating-a-primary-channel"></a>Erstellen eines primären Kanals 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>Sekundäre Kachel Kanäle

Hierbei handelt es sich um Kanäle, mit denen Kachel-und Signal Aktualisierungen per Push an eine sekundäre Kachel über gestellt werden können. Diese werden von apps verwendet, um Benutzer über interessante Aktionen oder Informationen zu benachrichtigen, mit denen Sie in der APP interagieren können, wie z. b. neue Nachrichten in einem Gruppenchat oder eine aktualisierte Sport Bewertung. 

### <a name="what-do-secondary-tile-channels-enable"></a>Was aktivieren sekundäre Kachel Kanäle?

-   Senden von Kachel-oder Badge-Benachrichtigungen an sekundäre Kacheln. Sekundäre Kacheln sind eine gute Möglichkeit, um Benutzer zurück in Ihre APP zu ziehen. Dabei handelt es sich um einen Deep-Link zu Informationen, die Sie interessieren, und das Platzieren relevanter Informationen auf den Kacheln hilft Ihnen, Sie wieder und wieder zu wiederholen.
-   Trennung von Kanälen (und Abläufen) zwischen verschiedenen Kacheln. Auf diese Weise können Sie die Logik im Backend zwischen den verschiedenen Typen von sekundären Kacheln trennen, die ein Benutzer an den Startbildschirm anheften kann. 
-   Nachrichten Verschlüsselung während der Übertragung wird von Windows mithilfe von TLS bereitgestellt. Nachrichten werden über das Netzwerk verschlüsselt und werden auf das Gerät des Benutzers übertragen.  

### <a name="limitations-of-secondary-tile-channels"></a>Einschränkungen bei sekundären Kachel Kanälen

-   Es sind keine Popup-oder unformatierten Benachrichtigungen zulässig. Popup-oder rohbenachrichtigungen, die an eine sekundäre Kachel gesendet werden, werden vom System ignoriert.
-   Erfordert die Registrierung Ihrer APP im Microsoft Store


### <a name="creating-a-secondary-tile-channel"></a>Erstellen eines sekundären Kachel Kanals 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>Alternative Kanäle

Alternative Kanäle ermöglichen es apps, Pushbenachrichtigungen zu senden, ohne sich beim Microsoft Store registrieren oder pushkanäle außerhalb der primären Datenbank zu erstellen, die für die APP verwendet wird. 
 
### <a name="what-do-alternate-channels-enable"></a>Was aktivieren alternative Kanäle?
-   Senden von Pushbenachrichtigungen an ein Windows-Gerät, das auf einem beliebigen Windows-Gerät ausgeführt wird Alternative Kanäle lassen nur unformatierte Benachrichtigungen zu (Sie können jedoch dennoch eine Hintergrundaufgabe aktivieren, um Popup-oder Kachel Benachrichtigungen lokal anzuzeigen).
-   Ermöglicht es apps, mehrere rohdatenpushkanäle für verschiedene Funktionen innerhalb der APP zu erstellen. Eine APP kann bis zu 1000 alternative Kanäle erstellen, die jeweils 30 Tage gültig sind. Jeder dieser Kanäle kann separat von der APP verwaltet oder widerrufen werden.
-   Alternative pushkanäle können erstellt werden, ohne dass eine APP beim Microsoft Store registriert wird. Wenn Ihre APP auf Geräten installiert werden soll, ohne Sie in der Microsoft Store zu registrieren, kann Sie weiterhin Pushbenachrichtigungen empfangen.
-   Server können Pushbenachrichtigungen mithilfe der W3C-Standard-Rest-APIs und des vapid-Protokolls Übertragung. Alternative Kanäle verwenden das W3C-Standardprotokoll. Dadurch können Sie die Server Logik vereinfachen, die gewartet werden muss.
-   Vollständige End-to-End-Nachrichten Verschlüsselung. Während der primäre Kanal während der Übertragung Verschlüsselung bereitstellt, können alternative Kanäle Ihre APP zum Schutz einer Nachricht über die Verschlüsselungs Header weiterleiten, wenn Sie besonders sicher sein möchten. 

### <a name="limitations-of-alternate-channels"></a>Einschränkungen von alternativen Kanälen
-   Der Server Ihrer APP kann keine pushpopup-, Kachel-oder Badge-typbenachrichtigungen senden. Sie können nur Pushbenachrichtigungen senden. Ihre APP kann weiterhin lokale Benachrichtigungen von ihrer Hintergrundaufgabe senden. 
-   Erfordert eine andere Rest-API als primäre oder sekundäre Kachel Kanäle. Die Verwendung der W3C-Standard-Rest-API bedeutet, dass Ihre APP eine andere Logik zum Senden von pushtoast-oder Kachel Aktualisierungen benötigt.

### <a name="creating-an-alternate-channel"></a>Erstellen eines alternativen Kanals 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>Kanaltyp Vergleich
Im folgenden finden Sie einen schnellen Vergleich zwischen den verschiedenen Arten von Kanälen:

<table>

<tr class="header">
<th align="left"><b>Typ</b></th>
<th align="left"><b>Pushtoast?</b></th>
<th align="left"><b>Kachel/Badge über Push?</b></th>
<th align="left"><b>Pushbenachrichtigungen per Push ausführen?</b></th>
<th align="left"><b>Authentifizierung</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>Store-Registrierung erforderlich?</b></th>
<th align="left"><b>Channels</b></th>
<th align="left"><b>Verschlüsselung</b></th>
</tr>


<tr class="odd">
<td align="left">Primär</td>
<td align="left">Ja</td>
<td align="left">Ja, nur primär Kachel</td>
<td align="left">Ja</td>
<td align="left">OAuth</td>
<td align="left">WNS-Rest-API</td>
<td align="left">Ja</td>
<td align="left">Eine pro App</td>
<td align="left">Während der Übertragung</td>
</tr>
<tr class="even">
<td align="left">Sekundäre Kachel</td>
<td align="left">Nein</td>
<td align="left">Ja, nur sekundäre Kachel</td>
<td align="left">Nein</td>
<td align="left">OAuth</td>
<td align="left">WNS-Rest-API</td>
<td align="left">Ja</td>
<td align="left">Eine pro Sekundär Kachel</td>
<td align="left">Während der Übertragung</td>
</tr>
<tr class="odd">
<td align="left">Alternativ</td>
<td align="left">Nein</td>
<td align="left">Nein</td>
<td align="left">Ja</td>
<td align="left">Vapid</td>
<td align="left">Webpush-W3C-Standard</td>
<td align="left">Nein</td>
<td align="left">1.000 pro App</td>
<td align="left">Bei Transit-und End-to-End-Verschlüsselung mit Header Pass-Through (erfordert app-Code)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>Auswählen des richtigen Kanals

Im Allgemeinen wird empfohlen, den primären Kanal in Ihrer APP mit wenigen Ausnahmen zu verwenden: 

1. Wenn Sie ein Kachel Update auf eine sekundäre Kachel übertragen, verwenden Sie den Push-Kanal der sekundären Kachel.
2. Wenn Sie Kanäle an andere Dienste weiterleiten (z. b. im Fall eines Browsers), verwenden Sie den alternativen Kanal.
3. Wenn Sie eine APP erstellen, die nicht im Windows Store (z. b. einer Lob-APP) aufgelistet wird, verwenden Sie einen alternativen Kanal.
4. Wenn Sie über vorhandenen webpushcode auf dem Server verfügen, den Sie wieder verwenden möchten, oder wenn Sie mehrere Kanäle in Ihrem Back-End-Dienst benötigen, verwenden Sie alternative Kanäle.

## <a name="related-articles"></a>Verwandte Artikel

* [Senden einer lokalen Kachelbenachrichtigung](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Adaptive und interaktive Popupbenachrichtigungen](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [Schnellstart: Senden einer Pushbenachrichtigung](/previous-versions/windows/apps/hh868252(v=win.10))
* [So wird's gemacht: Aktualisieren eines Signals durch Pushbenachrichtigungen](/previous-versions/windows/apps/hh465450(v=win.10))
* [So wird's gemacht: Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](/previous-versions/windows/apps/hh465412(v=win.10))
* [So wird's gemacht: Abfangen von Benachrichtigungen für ausgeführte Anwendungen](/previous-versions/windows/apps/hh465450(v=win.10))
* [So wird's gemacht: Authentifizieren mit dem Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](/previous-versions/windows/apps/hh465407(v=win.10))
* [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](/previous-versions/windows/apps/hh465435(v=win.10))
* [Richtlinien und Prüfliste für Pushbenachrichtigungen](./windows-push-notification-services--wns--overview.md)
* [Unformatierte Benachrichtigungen](raw-notification-overview.md)
