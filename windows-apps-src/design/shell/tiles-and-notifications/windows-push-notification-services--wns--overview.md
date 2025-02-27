---
description: Mithilfe des Windows-Pushbenachrichtigungsdiensts (WNS) können Drittanbieterentwickler Popup-, Kachel-, Signalupdates und unformatierte Updates von ihren eigenen Clouddiensten aus senden. Dadurch steht ein Mechanismus zur Verfügung, mit dem Sie Ihren Benutzern auf energieeffiziente und verlässliche Weise neue Updates bereitstellen können.
title: Übersicht über Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 09/28/2020
ms.topic: article
ms.custom: contperf-fy21q1
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6003db167d2a36205ac02f98750295d32a03ca62
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860263"
---
# <a name="windows-push-notification-services-wns-overview"></a>Übersicht über Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS) 

Mit dem Windows Push Notification Services (WNS) können Entwickler von Drittanbietern Popup-, Kachel-, Badge-und rohupdates von Ihrem eigenen clouddienst senden. Dadurch steht ein Mechanismus zur Verfügung, mit dem Sie Ihren Benutzern auf energieeffiziente und verlässliche Weise neue Updates bereitstellen können.

## <a name="how-it-works"></a>Funktionsweise

Das folgende Diagramm gibt Aufschluss über den vollständigen Datenfluss beim Senden einer Pushbenachrichtigung. Sie umfasst die folgenden Schritte:

1.  Ihre APP fordert von WNS einen pushbenachrichtigungskanal an.
2.  Windows fordert WNS zum Erstellen eines Benachrichtigungskanals auf. Dieser Kanal wird an das aufrufende Gerät in Form eines URIs (Uniform Resource Identifier) zurückgegeben.
3.  Der Benachrichtigungs Kanal-URI wird von WNS an Ihre APP zurückgegeben.
4.  Ihre App sendet den URI an Ihren eigenen Clouddienst. Dann speichern Sie den URI in Ihrem eigenen Clouddienst, damit Sie auf den URI zugreifen können, wenn Sie Benachrichtigungen senden. Der URI ist eine Schnittstelle zwischen Ihrer App und Ihrem Dienst. Es liegt in Ihrer Verantwortung, diese Schnittstelle mit sicheren Webstandards zu implementieren.
5.  Wenn Ihr Clouddienst über ein zu sendendes Update verfügt, benachrichtigt er WNS über den Kanal-URI. Zu diesem Zweck wird eine HTTP POST-Anforderung (einschließlich der Benachrichtigungsnutzlast) über SSL (Secure Sockets Layer) ausgegeben. Dieser Schritt erfordert eine Authentifizierung.
6.  WNS empfängt die Anforderung und leitet die Benachrichtigung an das entsprechende Gerät weiter.

![WNS-Datenflussdiagramm für Pushbenachrichtigungen](images/wns-diagram-01.jpg)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>Registrieren Ihrer App und Empfangen der Anmeldeinformationen für Ihren Clouddienst

Bevor Sie Benachrichtigungen mithilfe von WNS senden können, muss Ihre APP im Store-Dashboard registriert werden, wie [hier](/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification)beschrieben.

## <a name="requesting-a-notification-channel"></a>Anfordern eines Benachrichtigungskanals

Wenn eine App ausgeführt wird, die Pushbenachrichtigungen empfangen kann, muss sie zunächst mithilfe von [**CreatePushNotificationChannelForApplicationAsync**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_) einen Benachrichtigungskanal anfordern. Eine umfassende Erläuterung sowie Beispielcode finden Sie in [Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](/previous-versions/windows/apps/hh465412(v=win.10)). Diese API gibt einen Kanal-URI zurück, der eindeutig mit der aufrufenden Anwendung und der zugehörigen Kachel verknüpft ist und über den alle Benachrichtigungstypen gesendet werden können.

Nach erfolgreicher Erstellung eines Kanal-URIs sendet die App den URI zusammen mit den App-spezifischen Metadaten, die dem URI zugeordnet werden sollen, an den zugehörigen Clouddienst.

### <a name="important-notes"></a>Wichtige Hinweise

-   Wir können nicht garantieren, dass der Benachrichtigungskanal-URI für eine App jederzeit gleich bleibt. Wenn sich der URI ändert, empfehlen wir, die App immer einen neuen Kanal anfordern zu lassen, wenn sie ausgeführt wird und ihren Dienst aktualisiert. Der Entwickler sollte den Kanal-URI niemals ändern und ihn als Blackbox-Zeichenfolge betrachten. Derzeit laufen Kanal-URIs nach 30 Tagen ab. Wenn Ihre App für Windows 10 in regelmäßigen Abständen den Kanal im Hintergrund erneuert, können Sie das [Beispiel für Pushbenachrichtigungen und regelmäßige Benachrichtigungen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Push%20and%20periodic%20notifications%20client-side%20sample%20(Windows%208)) für Windows 8.1 herunterladen und den Quellcode und/oder das veranschaulichte Muster wiederverwenden.
-   Die Schnittstelle zwischen dem Clouddienst und dem der Client-App wird von Ihnen (dem Entwickler) implementiert. Wir empfehlen, die App mit dem eigenen Dienst einen Authentifizierungsprozess durchlaufen zu lassen und die Daten über ein sicheres Protokoll (beispielsweise HTTPS) zu übermitteln.
-   Der Clouddienst muss stets sicherstellen, dass der Kanal-URI die Domäne „notify.windows.com” verwendet. Der Dienst darf Benachrichtigungen niemals per Push an einen Kanal aus einer anderen Domäne übertragen. Im Falle einer Kompromittierung des Rückrufs für Ihre App könnte ein böswilliger Angreifer einen Kanal-URI übermitteln, um WNS zu täuschen. Ohne die Domäne zu überprüfen, könnte der clouddienst dem Angreifer möglicherweise unwissentlich Informationen offenlegen.
-   Wenn der Clouddienst versucht, eine Benachrichtigung an einen abgelaufenen Kanal zu senden, wird von WNS der [Antwortcode 410](/previous-versions/windows/apps/hh465435(v=win.10)) zurückgegeben. Als Reaktion auf diesen Code sollte der Dienst keine Benachrichtigungen mehr an diesen URI senden.

## <a name="authenticating-your-cloud-service"></a>Authentifizieren Ihres Clouddiensts

Zum Senden einer Authentifizierung muss der Clouddienst per WNS authentifiziert werden. Der erste Schritt in diesem Vorgang tritt auf, wenn Sie Ihre APP mit dem Microsoft Store-Dashboard registrieren. Im Rahmen der Registrierung erhält Ihre App eine Paket-Sicherheits-ID (Security Identifier, SID) sowie einen geheimen Schlüssel. Anhand dieser Informationen kann sich Ihr Clouddienst gegenüber WNS authentifizieren.

Das WNS-Authentifizierungsschema wird unter Verwendung des Client-Anmeldeinformationsprofils aus dem Protokoll [OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-v2-23) implementiert. Der Clouddienst authentifiziert sich gegenüber WNS durch Angeben seiner Anmeldeinformationen (Paket-SID und geheimer Schlüssel). Im Gegenzug erhält er ein Zugriffstoken. Dieses Zugriffstoken ermöglicht einem Clouddienst das Senden von Benachrichtigungen. Das Token wird bei jeder an WNS gesendeten Benachrichtigungsanforderung benötigt.

Im Anschluss finden Sie eine Übersicht über die Informationskette:

1.  Der Clouddienst sendet seine Anmeldeinformationen per HTTPS und gemäß OAuth 2.0-Protokoll an WNS. Dadurch wird der Dienst gegenüber WNS authentifiziert.
2.  Bei erfolgreicher Authentifizierung gibt WNS ein Zugriffstoken zurück. Dieses Zugriffstoken wird bis zu seinem Ablauf in allen weiteren Benachrichtigungsanforderungen verwendet.

![WNS-Diagramm für Clouddienstauthentifizierung](images/wns-diagram-02.jpg)

Im Rahmen der Authentifizierung gegenüber WNS übermittelt der Clouddienst eine HTTP-Anforderung per SSL (Secure Sockets Layer). Die Parameter werden im Format „application/x-www-for-urlencoded” angegeben. Geben Sie die Paket-sid im Feld "Client- \_ ID" und ihren geheimen Schlüssel im Feld "Client \_ Geheimnis" an, wie im folgenden Beispiel gezeigt. Ausführliche Informationen zur Syntax finden Sie in der Referenz zur [Zugriffstokenanforderung](/previous-versions/windows/apps/hh465435(v=win.10)).

> [!NOTE]
> Dies ist nur ein Beispiel, kein Ausschneide-und Einfüge Code, den Sie in Ihrem eigenen Code erfolgreich verwenden können. 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

WNS authentifiziert den Clouddienst und sendet bei erfolgreicher Authentifizierung die Antwort „200 OK”. Das Zugriffstoken wird in den Parametern innerhalb des Texts der HTTP-Antwort zurückgegeben. Hierbei wird der Medientyp „application/json” verwendet. Sobald Ihr Dienst das Zugriffstoken erhalten hat, können Sie Benachrichtigungen senden.

Das folgende Beispiel zeigt eine erfolgreiche Authentifizierungsantwort einschließlich Zugriffstoken. Ausführliche Informationen zur Syntax finden Sie unter [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](/previous-versions/windows/apps/hh465435(v=win.10)).

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

-   Das in dieser Prozedur unterstützte OAuth 2.0-Protokoll folgt der Entwurfsversion V16.
-   Die OAuth-RFC (Request for Comments) verwendet für den Clouddienst den Begriff "Client".
-   Diese Prozedur wird bis zur Finalisierung des OAuth-Entwurfs unter Umständen noch geändert.
-   Das Zugriffstoken kann für mehrere Benachrichtigungsanforderungen wiederverwendet werden. Dadurch muss sich der Clouddienst zum Senden mehrerer Benachrichtigungen lediglich einmal authentifizieren. Nach Ablauf des Zugriffstokens muss sich der Clouddienst allerdings erneut authentifizieren, um ein neues Zugriffstoken zu erhalten.

## <a name="sending-a-notification"></a>Senden einer Benachrichtigung


Mit dem Kanal-URI kann der Clouddienst eine Benachrichtigung senden, sobald ein Update für den Benutzer zur Verfügung steht.

Das weiter oben beschriebene Zugriffstoken kann für mehrere Benachrichtigungsanforderungen wiederverwendet werden. Der Cloudserver muss nicht für jede Benachrichtigung ein neues Zugriffstoken anfordern. Nach Ablauf des Zugriffstokens gibt die Benachrichtigungsanforderung einen Fehler zurück. Wir empfehlen, das Senden der Benachrichtigung bei Ablehnung des Zugriffstokens höchstens einmal zu wiederholen. Im Falle dieses Fehlers müssen Sie ein neues Zugriffstoken anfordern und die Benachrichtigung erneut senden. Den genauen Fehlercode finden Sie unter [Antwortcodes für Pushbenachrichtigungen](/previous-versions/windows/apps/hh465435(v=win.10)).

1.  Der Clouddienst führt eine HTTP-POST-Anforderung an den Kanal-URI aus. Diese Anforderung enthält die erforderlichen Header sowie die Benachrichtigungsnutzlast und muss per SSL erfolgen. Der Autorisierungsheader muss das erhaltene Zugriffstoken für die Autorisierung enthalten.

    Hier sehen Sie eine Beispielanforderung. Ausführliche Informationen zur Syntax finden Sie unter [Antwortcodes für Pushbenachrichtigungen](/previous-versions/windows/apps/hh465435(v=win.10)).

    Ausführliche Informationen zum Erstellen der Benachrichtigungsnutzlast finden Sie unter [Schnellstart: Senden einer Pushbenachrichtigung](/previous-versions/windows/apps/hh868252(v=win.10)). Die Nutzlast einer Kachel-, Popup- oder Signalpushbenachrichtigung wird als XML-Inhalt bereitgestellt, der den entsprechenden definierten [Schemas für adaptive Kacheln](adaptive-tiles-schema.md) oder [Schema für Legacykacheln](/uwp/schemas/tiles/tiles-xml-schema-portal) entspricht. Die Nutzlast einer unformatierten Benachrichtigung muss keine festgelegte Struktur aufweisen. Sie wird durch die App definiert.

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

![WNS-Diagramm für das Senden einer Benachrichtigung](images/wns-diagram-03.jpg)

### <a name="important-notes"></a>Wichtige Hinweise

-   WNS garantiert nicht die Zuverlässigkeit oder Latenz einer Benachrichtigung.
-   Benachrichtigungen dürfen niemals vertrauliche oder sensible Daten enthalten.
-   Zum Senden einer Benachrichtigung muss sich der Clouddienst zunächst gegenüber WNS authentifizieren und ein Zugriffstoken erhalten.
-   Mit einem Zugriffstoken kann der Clouddienst nur Benachrichtigungen an genau die App senden, für die das Token erstellt wurde. Ein einzelnes Zugriffstoken kann nicht dazu verwendet werden, Benachrichtigungen an mehrere Apps zu senden. Unterstützt Ihr Clouddienst also mehrere Apps, muss er das korrekte Zugriffstoken für die jeweilige App angeben, wenn er eine Benachrichtigung per Push an die einzelnen Kanal-URIs übermittelt.
-   Ist das Gerät offline, speichert WNS standardmäßig für jede App bis zu fünf Kachelbenachrichtigungen (bei aktivierter Warteschlange, ansonsten nur eine), eine Signalbenachrichtigung für jeden Kanal-URI und keine unformatierten Benachrichtigungen. Dieses standardmäßige Zwischenspeicherungsverhalten kann über den [X-WNS-Cache-Policy-Header](/previous-versions/windows/apps/hh465435(v=win.10)) geändert werden. Beachten Sie, dass Popupbenachrichtigungen nie gespeichert werden, wenn das Gerät offline ist.
-   In Szenarien mit personalisiertem Benachrichtigungsinhalt für den Benutzer empfiehlt WNS, dass der Clouddienst die Updates umgehend nach Eingang übermittelt. Beispiele für ein solches Szenario wären Feedupdates für soziale Medien, Chateinladungen, Benachrichtigungen über neue Nachrichten oder Warnungen. Als Alternative ist auch denkbar, dass das gleiche allgemeine Update häufig an eine große Untergruppe von Benutzern gesendet wird (beispielsweise Wetterinfos, Börsendaten oder Nachrichten). Gemäß den WNS-Richtlinien muss der zeitliche Abstand zwischen diesen Updates mindestens 30 Minuten betragen. Häufigere Routineupdates werden vom Endbenutzer oder von WNS unter Umständen als Missbrauch wahrgenommen.
-   Die Windows-Benachrichtigungs Plattform unterhält eine regelmäßige Datenverbindung mit WNS, um den Socket aktiv und fehlerfrei zu halten. Wenn keine Anwendungen Benachrichtigungs Kanäle anfordern oder verwenden, wird der Socket nicht erstellt.

## <a name="expiration-of-tile-and-badge-notifications"></a>Ablauf der Kachel- und Signalbenachrichtigungen

Standardmäßig laufen die Kachel- und Signalbenachrichtigungen drei Tage, nachdem sie heruntergeladen wurden, ab. Wenn eine Benachrichtigung abläuft, wird der Inhalt von der Kachel oder aus der Warteschlange entfernt und nicht mehr angezeigt. Daher wird empfohlen, für alle Kachel- und Signalbenachrichtigungen eine Gültigkeitsdauer festzulegen. Verwenden Sie eine Ablaufzeit, die für Ihre App sinnvoll ist. So können Sie sicherstellen, dass der Inhalt einer Kachel nur so lange beibehalten wird, wie er von Bedeutung ist. Eine explizite Ablaufzeit ist für Inhalte mit definierter Lebensdauer von großer Bedeutung. Durch sie wird außerdem sichergestellt, dass veraltete Inhalte entfernt werden, wenn Ihr Clouddienst keine Benachrichtigungen mehr sendet oder der Benutzer die Verbindung mit dem Netzwerk für längere Zeit trennt.

Ihr clouddienst kann für jede Benachrichtigung einen Ablauf festlegen, indem Sie den HTTP-Header "X-WNS-TTL" festlegen, um die Zeit (in Sekunden) anzugeben, die die Benachrichtigung nach dem Senden gültig bleibt. Weitere Informationen finden Sie unter [Push Notification Service Request und Response Headers](/previous-versions/windows/apps/hh465435(v=win.10)).

Während eines aktiven Börsenhandelstags können Sie beispielsweise die Gültigkeitsdauer für eine Aktienpreisaktualisierung gegenüber dem Sendeintervall verdoppeln (wie z. B. eine Stunde nach Empfang beim Senden von Benachrichtigungen zu jeder halben Stunde). Als weiteres Beispiel dient eine News-App, bei der festgestellt wird, dass ein Intervall von einem Tag für eine tägliche Kachelaktualisierung angemessen ist.

## <a name="push-notifications-and-battery-saver"></a>Pushbenachrichtigungen und Stromsparmodus

Der Stromsparmodus schränkt Hintergrundaktivitäten auf dem Gerät ein und verlängert dadurch die Akkulaufzeit. In Windows 10 kann der Benutzer den Stromsparmodus auf die automatische Aktivierung festlegen, wenn der Akku einen bestimmten Schwellenwert unterschreitet. Wenn der Stromsparmodus aktiviert ist, ist der Empfang von Pushbenachrichtigungen deaktiviert, um Energie zu sparen. Es gibt allerdings einige Ausnahmen. Mit den folgenden Einstellungen in Windows 10 (in der **Einstellungs**-App) kann der Stromsparmodus so eingerichtet werden, dass Ihre App auch bei aktiviertem Stromsparmodus Pushbenachrichtigungen empfängt.

-   **Im Stromsparmodus Pushbenachrichtigungen von jeder App zulassen**: Alle Apps können Pushbenachrichtigungen empfangen, wenn der Stromsparmodus aktiviert ist. Beachten Sie, dass diese Einstellung nur für Desktop-Editionen von Windows 10 gilt (Home, Pro, Enterprise und Education).
-   **Immer zugelassen**: Bestimmte Apps können im Hintergrund ausgeführt werden, wenn der Stromsparmodus aktiviert ist. Dies beinhaltet auch den Empfang von Pushbenachrichtigungen. Die Liste wird manuell vom Benutzer verwaltet.

Es ist nicht möglich, den Status dieser beiden Einstellungen zu überprüfen, Sie können aber den Status des Stromsparmodus feststellen. Verwenden Sie in Windows 10 die [**EnergySaverStatus**](/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus)-Eigenschaft, um den Status des Stromsparmodus zu überprüfen. Ihre App kann auch mithilfe des [**EnergySaverStatusChanged**](/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged)-Ereignisses Änderungen des Stromsparmodus überwachen.

Falls Pushbenachrichtigungen bei Ihrer App sehr wichtig sind, sollten Sie die Benutzer darüber informieren, dass sie bei aktiviertem Stromsparmodus keine Benachrichtigungen erhalten, und eine einfache Möglichkeit zum Anpassen der **Einstellungen für den Stromsparmodus** vorsehen. Sie können mit dem URI-Schema für die Einstellungen für den Stromsparmodus in Windows 10, `ms-settings:batterysaver-settings`, direkt einen Link zur Einstellungs-App bereitstellen.

> [!TIP]
> Bei der Benachrichtigung des Benutzers über die Einstellungen des Akku Schoners empfiehlt es sich, die Nachricht in Zukunft zu unterdrücken. Das Kontrollkästchen `dontAskMeAgainBox` im folgenden Beispiel speichert z. B. die Benutzereinstellung in [**LocalSettings**](/uwp/api/Windows.Storage.ApplicationData.LocalSettings).

Im folgenden finden Sie ein Beispiel dafür, wie Sie überprüfen können, ob Akku Schoner in Windows 10 aktiviert ist. In diesem Beispiel wird der Benutzer benachrichtigt, und die Einstellungs-App wird gestartet, um **Einstellungen für Stromsparmodus** anzuzeigen. Mit dem Kontrollkästchen `dontAskAgainSetting` kann der Benutzer die Meldung unterdrücken, wenn er nicht erneut benachrichtigt werden möchte.

```csharp
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

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.System.h>
#include <winrt/Windows.System.Power.h>
#include <winrt/Windows.UI.Xaml.h>
#include <winrt/Windows.UI.Xaml.Controls.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
using namespace winrt;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Storage;
using namespace winrt::Windows::System;
using namespace winrt::Windows::System::Power;
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Controls;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
winrt::fire_and_forget CheckForEnergySaving()
{
    // Get reminder preference from LocalSettings.
    bool dontAskAgain{ false };
    auto localSettings = ApplicationData::Current().LocalSettings();
    IInspectable dontAskSetting = localSettings.Values().Lookup(L"dontAskAgainSetting");
    if (!dontAskSetting)
    {
        // Setting doesn't exist.
        dontAskAgain = false;
    }
    else
    {
        // Retrieve setting value
        dontAskAgain = winrt::unbox_value<bool>(dontAskSetting);
    }

    // Check whether battery saver is on, and whether it's okay to raise dialog.
    if ((PowerManager::EnergySaverStatus() == EnergySaverStatus::On) && (!dontAskAgain))
    {
        // Check dialog results.
        ContentDialogResult dialogResult = co_await saveEnergyDialog().ShowAsync();
        if (dialogResult == ContentDialogResult::Primary)
        {
            // Launch battery saver settings
            // (settings are available only when a battery is present).
            co_await Launcher::LaunchUriAsync(Uri(L"ms-settings:batterysaver-settings"));
        }

        // Save reminder preference.
        if (dontAskAgainBox().IsChecked())
        {
            // Don't raise the dialog again.
            localSettings.Values().Insert(L"dontAskAgainSetting", winrt::box_value(true));
        }
    }
}
```

Dies ist der XAML-Code für das in diesem Beispiel vorgestellte [**ContentDialog**](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog).

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

## <a name="related-topics"></a>Zugehörige Themen

* [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md)
* [Schnellstart: Senden einer Pushbenachrichtigung](/previous-versions/windows/apps/hh868252(v=win.10))
* [So wird's gemacht: Aktualisieren eines Signals durch Pushbenachrichtigungen](/previous-versions/windows/apps/hh465450(v=win.10))
* [So wird's gemacht: Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](/previous-versions/windows/apps/hh465412(v=win.10))
* [So wird's gemacht: Abfangen von Benachrichtigungen für ausgeführte Anwendungen](/previous-versions/windows/apps/jj709907(v=win.10))
* [So wird's gemacht: Authentifizieren mit dem Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](/previous-versions/windows/apps/hh465407(v=win.10))
* [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](/previous-versions/windows/apps/hh465435(v=win.10))
* [Richtlinien und Prüfliste für Pushbenachrichtigungen]()
* [Unformatierte Benachrichtigungen](/previous-versions/windows/apps/hh761488(v=win.10))