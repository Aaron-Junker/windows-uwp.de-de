---
author: mijacobs
Description: The Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. This provides a mechanism to deliver new updates to your users in a power-efficient and dependable way.
title: Übersicht über Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 633fd26a7dfc799f9b9c9058f88ba6b1fa40ac57
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "7556152"
---
# <a name="windows-push-notification-services-wns-overview"></a>Übersicht über Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)
 

Mithilfe des Windows-Pushbenachrichtigungsdiensts (WNS) können Drittanbieterentwickler Popup-, Kachel-, Signalupdates und unformatierte Updates von ihren eigenen Clouddiensten aus senden. Dadurch steht ein Mechanismus zur Verfügung, mit dem Sie Ihren Benutzern auf energieeffiziente und verlässliche Weise neue Updates bereitstellen können.

## <a name="how-it-works"></a>Funktionsweise


Das folgende Diagramm gibt Aufschluss über den vollständigen Datenfluss beim Senden einer Pushbenachrichtigung. Er umfasst die folgenden Schritte:

1.  Ihre App fordert einen Pushbenachrichtigungskanal von der universellen Windows-Plattform an.
2.  Windows fordert WNS zum Erstellen eines Benachrichtigungskanals auf. Dieser Kanal wird an das aufrufende Gerät in Form eines URIs (Uniform Resource Identifier) zurückgegeben.
3.  Windows gibt den URI des Benachrichtigungskanals an Ihre App zurück.
4.  Ihre App sendet den URI an Ihren eigenen Clouddienst. Dann speichern Sie den URI in Ihrem eigenen Clouddienst, damit Sie auf den URI zugreifen können, wenn Sie Benachrichtigungen senden. Der URI ist eine Schnittstelle zwischen Ihrer App und Ihrem Dienst. Es liegt in Ihrer Verantwortung, diese Schnittstelle mit sicheren Webstandards zu implementieren.
5.  Wenn Ihr Clouddienst über ein zu sendendes Update verfügt, benachrichtigt er WNS über den Kanal-URI. Zu diesem Zweck wird eine HTTP POST-Anforderung (einschließlich der Benachrichtigungsnutzlast) über SSL (Secure Sockets Layer) ausgegeben. Dieser Schritt erfordert eine Authentifizierung.
6.  WNS empfängt die Anforderung und leitet die Benachrichtigung an das entsprechende Gerät weiter.

![WNS-Datenflussdiagramm für Pushbenachrichtigungen](images/wns-diagram-01.png)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>Registrieren Ihrer App und Empfangen der Anmeldeinformationen für Ihren Clouddienst


Um Benachrichtigungen mithilfe von WNS senden zu können, muss Ihre App zunächst beim Dashboard des Store registriert werden. Dadurch erhalten Sie die Anmeldeinformationen für Ihre App, mit denen sich Ihr Clouddienst gegenüber WNS authentifizieren kann. Diese Anmeldeinformationen bestehen aus einer Paket-Sicherheits-ID (Security Identifier, SID) und einem geheimen Schlüssel. Um diese Registrierung durchzuführen, melden Sie sich beim [Partner Center](https://partner.microsoft.com/dashboard). Nachdem Sie Ihre App erstellt haben, können Sie die Anmeldeinformationen abrufen, gemäß den Anweisungen auf der Seite **App-Verwaltung – WNS/MPNS**. Wenn Sie die Live Services-Lösung verwenden möchten, folgen Sie dem Link **Live Services-Website** auf dieser Seite.

Jede App verfügt über einen eigenen Satz von Anmeldeinformationen für den zugehörigen Clouddienst. Mit diesen Anmeldeinformationen können keine Benachrichtigungen an andere Apps gesendet werden.

Ausführlichere Informationen zum Registrieren Ihrer App finden Sie unter [So wird's gemacht: Authentifizieren mit den Windows-Pushbenachrichtigungsdiensten (Windows Push Notification Services, WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407).

## <a name="requesting-a-notification-channel"></a>Anfordern eines Benachrichtigungskanals


Wenn eine App ausgeführt wird, die Pushbenachrichtigungen empfangen kann, muss sie zunächst mithilfe von [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_) einen Benachrichtigungskanal anfordern. Eine umfassende Erläuterung sowie Beispielcode finden Sie in [Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](https://msdn.microsoft.com/library/windows/apps/hh465412). Diese API gibt einen Kanal-URI zurück, der eindeutig mit der aufrufenden Anwendung und der zugehörigen Kachel verknüpft ist und über den alle Benachrichtigungstypen gesendet werden können.

Nach erfolgreicher Erstellung eines Kanal-URIs sendet die App den URI zusammen mit den App-spezifischen Metadaten, die dem URI zugeordnet werden sollen, an den zugehörigen Clouddienst.

### <a name="important-notes"></a>Wichtige Hinweise

-   Wir können nicht garantieren, dass der Benachrichtigungskanal-URI für eine App jederzeit gleich bleibt. Wenn sich der URI ändert, empfehlen wir, die App immer einen neuen Kanal anfordern zu lassen, wenn sie ausgeführt wird und ihren Dienst aktualisiert. Der Entwickler sollte den Kanal-URI niemals ändern und ihn als Blackbox-Zeichenfolge betrachten. Derzeit laufen Kanal-URIs nach 30Tagen ab. Wenn Ihre Windows 10-app in regelmäßigen Abständen den Kanal im Hintergrund verlängert werden, können Sie die [Pushbenachrichtigungen und regelmäßige Benachrichtigungen-Beispiel](http://go.microsoft.com/fwlink/p/?linkid=231476) für Windows8.1 herunterladen und erneut verwenden Sie den Quellcode und/oder das Muster, das es veranschaulicht.
-   Die Schnittstelle zwischen dem Clouddienst und dem der Client-App wird von Ihnen (dem Entwickler) implementiert. Wir empfehlen, die App mit dem eigenen Dienst einen Authentifizierungsprozess durchlaufen zu lassen und die Daten über ein sicheres Protokoll (beispielsweise HTTPS) zu übermitteln.
-   Der Clouddienst muss stets sicherstellen, dass der Kanal-URI die Domäne „notify.windows.com” verwendet. Der Dienst darf Benachrichtigungen niemals per Push an einen Kanal aus einer anderen Domäne übertragen. Im Falle einer Kompromittierung des Rückrufs für Ihre App könnte ein böswilliger Angreifer einen Kanal-URI übermitteln, um WNS zu täuschen. Ohne Überprüfung der Domäne gibt Ihr Clouddienst unter Umständen unwissentlich Informationen an diesen Angreifer weiter.
-   Wenn der Clouddienst versucht, eine Benachrichtigung an einen abgelaufenen Kanal zu senden, wird von WNS der [Antwortcode 410](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#WNSResponseCodes) zurückgegeben. Als Reaktion auf diesen Code sollte der Dienst keine Benachrichtigungen mehr an diesen URI senden.

## <a name="authenticating-your-cloud-service"></a>Authentifizieren Ihres Clouddiensts


Zum Senden einer Authentifizierung muss der Clouddienst per WNS authentifiziert werden. Der erste Schritt in diesem Prozess erfolgt, wenn Sie Ihre App beim Microsoft Store-Dashboard registrieren. Im Rahmen der Registrierung erhält Ihre App eine Paket-Sicherheits-ID (Security Identifier, SID) sowie einen geheimen Schlüssel. Anhand dieser Informationen kann sich Ihr Clouddienst gegenüber WNS authentifizieren.

Das WNS-Authentifizierungsschema wird unter Verwendung des Client-Anmeldeinformationsprofils aus dem Protokoll [OAuth2.0](http://go.microsoft.com/fwlink/p/?linkid=226787) implementiert. Der Clouddienst authentifiziert sich gegenüber WNS durch Angeben seiner Anmeldeinformationen (Paket-SID und geheimer Schlüssel). Im Gegenzug erhält er ein Zugriffstoken. Dieses Zugriffstoken ermöglicht einem Clouddienst das Senden von Benachrichtigungen. Das Token wird bei jeder an WNS gesendeten Benachrichtigungsanforderung benötigt.

Im Anschluss finden Sie eine Übersicht über die Informationskette:

1.  Der Clouddienst sendet seine Anmeldeinformationen per HTTPS und gemäß OAuth2.0-Protokoll an WNS. Dadurch wird der Dienst gegenüber WNS authentifiziert.
2.  Bei erfolgreicher Authentifizierung gibt WNS ein Zugriffstoken zurück. Dieses Zugriffstoken wird bis zu seinem Ablauf in allen weiteren Benachrichtigungsanforderungen verwendet.

![WNS-Diagramm für Clouddienstauthentifizierung](images/wns-diagram-02.png)

Im Rahmen der Authentifizierung gegenüber WNS übermittelt der Clouddienst eine HTTP-Anforderung per SSL (Secure Sockets Layer). Die Parameter werden im Format „application/x-www-for-urlencoded” angegeben. Geben Sie im Feld „client\_id” Ihre Paket-SID und im Feld „client\_secret” Ihren geheimen Schlüssel an. Ausführliche Informationen zur Syntax finden Sie in der Referenz zur [Zugriffstokenanforderung](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#access_token_request).

**Hinweis:** Dies ist nur ein Beispiel, nicht ausschneiden und Einfügen-Code, der Sie erfolgreich in Ihrem eigenen Code verwenden können.

 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

WNS authentifiziert den Clouddienst und sendet bei erfolgreicher Authentifizierung die Antwort „200 OK”. Das Zugriffstoken wird in den Parametern innerhalb des Texts der HTTP-Antwort zurückgegeben. Hierbei wird der Medientyp „application/json” verwendet. Sobald Ihr Dienst das Zugriffstoken erhalten hat, können Sie Benachrichtigungen senden.

Das folgende Beispiel zeigt eine erfolgreiche Authentifizierungsantwort einschließlich Zugriffstoken. Ausführliche Informationen zur Syntax finden Sie unter [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](https://msdn.microsoft.com/library/windows/apps/hh465435).

``` http
 HTTP/1.1 200 OK   
 Cache-Control: no-store
 Content-Length: 422
 Content-Type: application/json
 
 {
     "access_token":"EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=", 
     "token_type":"bearer"
 }
```

### <a name="important-notes"></a>Wichtige Hinweise

-   Das in dieser Prozedur unterstützte OAuth2.0-Protokoll folgt der EntwurfsversionV16.
-   Die OAuth-RFC (Request for Comments) verwendet für den Clouddienst den Begriff "Client".
-   Diese Prozedur wird bis zur Finalisierung des OAuth-Entwurfs unter Umständen noch geändert.
-   Das Zugriffstoken kann für mehrere Benachrichtigungsanforderungen wiederverwendet werden. Dadurch muss sich der Clouddienst zum Senden mehrerer Benachrichtigungen lediglich einmal authentifizieren. Nach Ablauf des Zugriffstokens muss sich der Clouddienst allerdings erneut authentifizieren, um ein neues Zugriffstoken zu erhalten.

## <a name="sending-a-notification"></a>Senden einer Benachrichtigung


Mit dem Kanal-URI kann der Clouddienst eine Benachrichtigung senden, sobald ein Update für den Benutzer zur Verfügung steht.

Das weiter oben beschriebene Zugriffstoken kann für mehrere Benachrichtigungsanforderungen wiederverwendet werden. Der Cloudserver muss nicht für jede Benachrichtigung ein neues Zugriffstoken anfordern. Nach Ablauf des Zugriffstokens gibt die Benachrichtigungsanforderung einen Fehler zurück. Wir empfehlen, das Senden der Benachrichtigung bei Ablehnung des Zugriffstokens höchstens einmal zu wiederholen. Im Falle dieses Fehlers müssen Sie ein neues Zugriffstoken anfordern und die Benachrichtigung erneut senden. Den genauen Fehlercode finden Sie unter [Antwortcodes für Pushbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh465435).

1.  Der Clouddienst führt eine HTTP-POST-Anforderung an den Kanal-URI aus. Diese Anforderung enthält die erforderlichen Header sowie die Benachrichtigungsnutzlast und muss per SSL erfolgen. Der Autorisierungsheader muss das erhaltene Zugriffstoken für die Autorisierung enthalten.

    Hier sehen Sie eine Beispielanforderung. Ausführliche Informationen zur Syntax finden Sie unter [Antwortcodes für Pushbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh465435).

    Ausführliche Informationen zum Erstellen der Benachrichtigungsnutzlast finden Sie unter [Schnellstart: Senden einer Pushbenachrichtigung](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252). Die Nutzlast einer Kachel-, Popup- oder Signalpushbenachrichtigung wird als XML-Inhalt bereitgestellt, der den entsprechenden definierten [Schemas für adaptive Kacheln](adaptive-tiles-schema.md) oder [Schema für Legacykacheln](https://msdn.microsoft.com/library/windows/apps/br212853) entspricht. Die Nutzlast einer unformatierten Benachrichtigung muss keine festgelegte Struktur aufweisen. Sie wird durch die App definiert.

    ``` http
     POST https://cloud.notify.windows.com/?token=AQE%bU%2fSjZOCvRjjpILow%3d%3d HTTP/1.1
     Content-Type: text/xml
     X-WNS-Type: wns/tile
     Authorization: Bearer EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=
     Host: cloud.notify.windows.com
     Content-Length: 24

     <body>
     ....
    ```

2.  WNS antwortet, um anzugeben, dass die Benachrichtigung empfangen wurde und bei nächster Gelegenheit übermittelt wird. WNS liefert jedoch keine End-to-End-Bestätigung, dass Ihre Benachrichtigung vom Gerät oder von der Anwendung empfangen wurde.

Dieses Diagramm veranschaulicht den Datenfluss:

![WNS-Diagramm für das Senden einer Benachrichtigung](images/wns-diagram-03.png)

### <a name="important-notes"></a>Wichtige Hinweise

-   WNS garantiert nicht die Zuverlässigkeit oder Latenz einer Benachrichtigung.
-   Benachrichtigungen dürfen niemals vertrauliche oder sensible Daten enthalten.
-   Zum Senden einer Benachrichtigung muss sich der Clouddienst zunächst gegenüber WNS authentifizieren und ein Zugriffstoken erhalten.
-   Mit einem Zugriffstoken kann der Clouddienst nur Benachrichtigungen an genau die App senden, für die das Token erstellt wurde. Ein einzelnes Zugriffstoken kann nicht dazu verwendet werden, Benachrichtigungen an mehrere Apps zu senden. Unterstützt Ihr Clouddienst also mehrere Apps, muss er das korrekte Zugriffstoken für die jeweilige App angeben, wenn er eine Benachrichtigung per Push an die einzelnen Kanal-URIs übermittelt.
-   Ist das Gerät offline, speichert WNS standardmäßig für jede App bis zu fünfKachelbenachrichtigungen (bei aktivierter Warteschlange, ansonsten nur eine), eine Signalbenachrichtigung für jeden Kanal-URI und keine unformatierten Benachrichtigungen. Dieses standardmäßige Zwischenspeicherungsverhalten kann über den [X-WNS-Cache-Policy-Header](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache) geändert werden. Beachten Sie, dass Popupbenachrichtigungen nie gespeichert werden, wenn das Gerät offline ist.
-   In Szenarien mit personalisiertem Benachrichtigungsinhalt für den Benutzer empfiehlt WNS, dass der Clouddienst die Updates umgehend nach Eingang übermittelt. Beispiele für ein solches Szenario wären Feedupdates für soziale Medien, Chateinladungen, Benachrichtigungen über neue Nachrichten oder Warnungen. Als Alternative ist auch denkbar, dass das gleiche allgemeine Update häufig an eine große Untergruppe von Benutzern gesendet wird (beispielsweise Wetterinfos, Börsendaten oder Nachrichten). Gemäß den WNS-Richtlinien muss der zeitliche Abstand zwischen diesen Updates mindestens 30Minuten betragen. Häufigere Routineupdates werden vom Endbenutzer oder von WNS unter Umständen als Missbrauch wahrgenommen.

## <a name="expiration-of-tile-and-badge-notifications"></a>Ablauf der Kachel- und Signalbenachrichtigungen


Standardmäßig laufen die Kachel- und Signalbenachrichtigungen drei Tage, nachdem sie heruntergeladen wurden, ab. Wenn eine Benachrichtigung abläuft, wird der Inhalt von der Kachel oder aus der Warteschlange entfernt und nicht mehr angezeigt. Daher wird empfohlen, für alle Kachel- und Signalbenachrichtigungen eine Gültigkeitsdauer festzulegen. Verwenden Sie eine Ablaufzeit, die für Ihre App sinnvoll ist. So können Sie sicherstellen, dass der Inhalt einer Kachel nur so lange beibehalten wird, wie er von Bedeutung ist. Eine explizite Ablaufzeit ist für Inhalte mit definierter Lebensdauer von großer Bedeutung. Durch sie wird außerdem sichergestellt, dass veraltete Inhalte entfernt werden, wenn Ihr Clouddienst keine Benachrichtigungen mehr sendet oder der Benutzer die Verbindung mit dem Netzwerk für längere Zeit trennt.

Ihr Clouddienst kann eine Gültigkeitsdauer für jede Benachrichtigung festlegen, indem der X-WNS-TTL-HTTP-Header so festgelegt wird, dass er die Dauer (in Sekunden) angibt, die Ihre Benachrichtigung nach dem Senden gültig bleibt. Weitere Informationen finden Sie unter [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_ttl).

Während eines aktiven Börsenhandelstags können Sie beispielsweise die Gültigkeitsdauer für eine Aktienpreisaktualisierung gegenüber dem Sendeintervall verdoppeln (wie z.B. eine Stunde nach Empfang beim Senden von Benachrichtigungen zu jeder halben Stunde). Als weiteres Beispiel dient eine News-App, bei der festgestellt wird, dass ein Intervall von einem Tag für eine tägliche Kachelaktualisierung angemessen ist.

## <a name="push-notifications-and-battery-saver"></a>Pushbenachrichtigungen und Stromsparmodus


Der Stromsparmodus schränkt Hintergrundaktivitäten auf dem Gerät ein und verlängert dadurch die Akkulaufzeit. Windows 10 ermöglicht den Benutzer das Festlegen der Stromsparmodus automatisch aktivieren, wenn der Akkustand einen bestimmten Schwellenwert unterschreitet. Wenn der Stromsparmodus aktiviert ist, ist der Empfang von Pushbenachrichtigungen deaktiviert, um Energie zu sparen. Es gibt allerdings einige Ausnahmen. Die folgenden Windows 10 Einstellungen für den Stromsparmodus (finden Sie in den **Einstellungen** ) ermöglichen Ihrer app zum Empfangen von Pushbenachrichtigungen, auch wenn der Stromsparmodus aktiviert ist.

-   **Im Stromsparmodus Pushbenachrichtigungen von jeder App zulassen**: Alle Apps können Pushbenachrichtigungen empfangen, wenn der Stromsparmodus aktiviert ist. Beachten Sie, dass diese Einstellung nur für Windows 10 für Desktopeditionen (Home, Pro, Enterprise und Education) gilt.
-   **Immer zugelassen**: Bestimmte Apps können im Hintergrund ausgeführt werden, wenn der Stromsparmodus aktiviert ist, und dabei Pushbenachrichtigungen empfangen. Die Liste wird manuell vom Benutzer verwaltet.

Es ist nicht möglich, den Status dieser beiden Einstellungen zu überprüfen, Sie können aber den Status des Stromsparmodus feststellen. Verwenden Sie in Windows 10 die [**EnergySaverStatus**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus) -Eigenschaft, um den Status des Stromsparmodus zu überprüfen. Ihre App kann auch mithilfe des [**EnergySaverStatusChanged**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged)-Ereignisses Änderungen des Stromsparmodus überwachen.

Falls Pushbenachrichtigungen bei Ihrer App sehr wichtig sind, sollten Sie die Benutzer darüber informieren, dass sie bei aktiviertem Stromsparmodus keine Benachrichtigungen erhalten, und eine einfache Möglichkeit zum Anpassen der **Einstellungen für den Stromsparmodus** vorsehen. Mit URI-Schema der Akku Stromsparmodus in Windows 10, `ms-settings:batterysaver-settings`, Sie können einen praktischen Link zu der Einstellungs-app bereitstellen.

**Tipp:**  , wenn den Benutzer über die Einstellungen für den Stromsparmodus informieren, wir empfehlen, eine Möglichkeit, die Nachricht in der Zukunft zu unterdrücken. Das Kontrollkästchen `dontAskMeAgainBox` im folgenden Beispiel speichert z.B. die Benutzereinstellung in [**LocalSettings**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalSettings).

 

Hier ist ein Beispiel dazu, wie Sie überprüfen, ob der Stromsparmodus in Windows 10 aktiviert ist. In diesem Beispiel wird der Benutzer benachrichtigt, und die Einstellungs-App wird gestartet, um **Einstellungen für Stromsparmodus** anzuzeigen. Mit dem Kontrollkästchen `dontAskAgainSetting` kann der Benutzer die Meldung unterdrücken, wenn er nicht erneut benachrichtigt werden möchte.

```cs
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;
using Windows.System;
using Windows.System.Power;
...
...
async public void CheckForEnergySaving()
{
   //Get reminder preference from LocalSettings
   bool dontAskAgain;
   var localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
   object dontAskSetting = localSettings.Values["dontAskAgainSetting"];
   if (dontAskSetting == null)
   {  // Setting does not exist
      dontAskAgain = false;
   }
   else
   {  // Retrieve setting value
      dontAskAgain = Convert.ToBoolean(dontAskSetting);
   }
   
   // Check if battery saver is on and that it's okay to raise dialog
   if ((PowerManager.EnergySaverStatus == EnergySaverStatus.On)
         && (dontAskAgain == false))
   {
      // Check dialog results
      ContentDialogResult dialogResult = await saveEnergyDialog.ShowAsync();
      if (dialogResult == ContentDialogResult.Primary)
      {
         // Launch battery saver settings (settings are available only when a battery is present)
         await Launcher.LaunchUriAsync(new Uri("ms-settings:batterysaver-settings"));
      }

      // Save reminder preference
      if (dontAskAgainBox.IsChecked == true)
      {  // Don't raise dialog again
         localSettings.Values["dontAskAgainSetting"] = "true";
      }
   }
}
```

Dies ist der XAML-Code für das in diesem Beispiel vorgestellte [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog).

```xaml
<ContentDialog x:Name="saveEnergyDialog"
               PrimaryButtonText="Open battery saver settings"
               SecondaryButtonText="Ignore"
               Title="Battery saver is on."> 
   <StackPanel>
      <TextBlock TextWrapping="WrapWholeWords">
         <LineBreak/><Run>Battery saver is on and you may 
          not receive push notifications.</Run><LineBreak/>
         <LineBreak/><Run>You can choose to allow this app to work normally
         while in battery saver, including receiving push notifications.</Run>
         <LineBreak/>
      </TextBlock>
      <CheckBox x:Name="dontAskAgainBox" Content="OK, got it."/>
   </StackPanel>
</ContentDialog>
```

## <a name="related-topics"></a>Verwandte Themen


* [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md)
* [Schnellstart: Senden einer Pushbenachrichtigung](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [So wird's gemacht: Aktualisieren eines Signals durch Pushbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [So wird's gemacht: Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [So wird's gemacht: Abfangen von Benachrichtigungen für ausgeführte Anwendungen](https://msdn.microsoft.com/library/windows/apps/xaml/jj709907.aspx)
* [So wird's gemacht: Authentifizieren mit dem Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [Richtlinien und Prüfliste für Pushbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Unformatierte Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761488)
 

 




