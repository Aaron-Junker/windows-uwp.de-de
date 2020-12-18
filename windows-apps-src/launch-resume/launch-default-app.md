---
title: Starten der Standard-App für einen URI
description: Erfahren Sie, wie Sie die Standard-App für einen Uniform Resource Identifier (URI) starten. URIs ermöglichen den Start einer anderen App zum Ausführen einer bestimmten Aufgabe. Dieses Thema enthält auch eine Übersicht über die zahlreichen in Windows integrierten URI-Schemas.
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.date: 06/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2f550fc90a8035d2c7e355d70b7ddd2b9a9e17fc
ms.sourcegitcommit: f83f2f582f8c7c3447ec62df40a8f0724f7f3bbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2020
ms.locfileid: "97675791"
---
# <a name="launch-the-default-app-for-a-uri"></a>Starten der Standard-App für einen URI


**Wichtige APIs**

- [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)
- [**PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
- [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)

Erfahren Sie, wie Sie die Standard-App für einen Uniform Resource Identifier (URI) starten. URIs ermöglichen den Start einer anderen App zum Ausführen einer bestimmten Aufgabe. Dieses Thema enthält auch eine Übersicht über die zahlreichen in Windows integrierten URI-Schemas. Sie können außerdem benutzerdefinierte URIs starten. Weitere Informationen zum Registrieren eines benutzerdefinierten URI-Schemas und Behandeln der URI-Aktivierung finden Sie unter [Behandeln der URI-Aktivierung](handle-uri-activation.md).

Mit URI-Schemas können Sie Apps öffnen, indem Sie auf Hyperlinks klicken. Genau wie Sie eine neue E-Mail mit **mailto:** öffnen, können Sie den Standardwebbrowser mit **http:** öffnen.

In diesem Thema werden einige der folgenden URI-Schemas beschrieben, die in Windows integriert sind:

| URI-Schema | Startet |
| ----------:|----------|
|[bingmaps:, MS-Drive-to: und MS-Walk-to: ](#maps-app-uri-schemes) | Karten-App |
|[http](#http-uri-scheme) | Standardwebbrowser |
|[mailto](#email-uri-scheme) | Standard-E-Mail-App |
|[ms-call:](#call-app-uri-scheme) |  Anruf-App |
|[ms-chat:](#messaging-app-uri-scheme) | Messaging-App |
|[ms-people:](#people-app-uri-scheme) | Kontakte-App |
|[MS-Fotos:](#photos-app-uri-scheme) | Fotos-App |
|[MS-Settings:](#settings-app-uri-scheme) | Einstellungs-App |
|[ms-store:](#store-app-uri-scheme)  | Store-App |
|[ms-tonepicker:](#tone-picker-uri-scheme) | Tonauswahl |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | Nearby Numbers-App |
|[msnweather:](#weather-app-uri-scheme) | Wetter-App |
|[Microsoft-Edge:](#microsoft-edge-uri-scheme) | Microsoft Edge |

<br>
Der folgende URI öffnet beispielsweise den Standardbrowser und zeigt die Bing-Website an.

`https://bing.com`

Sie können außerdem benutzerdefinierte URI-Schemas starten. Wenn zum Behandeln dieses URI keine App installiert ist, können Sie dem Benutzer die Installation einer App empfehlen. Weitere Informationen finden Sie unter [empfehlen einer APP, wenn Sie für die Behandlung des URIs nicht verfügbar ist](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

Im Allgemeinen kann die App nicht die App auswählen, die gestartet wird. Der Benutzer entscheidet, welche App gestartet wird. Zum Behandeln desselben URI-Schemas kann mehr als eine App registriert werden. Die einzige Ausnahme sind reservierte URI-Schemas. Registrierungen der reservierten URI-Schemas werden ignoriert. Die vollständige Liste der reservierten URI-Schemas finden Sie unter [Behandeln der URI-Aktivierung](handle-uri-activation.md). In Fällen, in denen mehrere Apps das gleiche URI-Schema registriert haben, kann Ihre App das Starten einer bestimmten App empfehlen. Weitere Informationen finden Sie unter [empfehlen einer APP, wenn Sie für die Behandlung des URIs nicht verfügbar ist](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

### <a name="call-launchuriasync-to-launch-a-uri"></a>Aufruf von „LaunchUriAsync“ zum Starten eines URI

Verwenden Sie die Methode [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync), um einen URI zu starten. Beim Aufrufen dieser Methode muss Ihre App im Vordergrund ausgeführt werden, d. h., sie muss für den Benutzer sichtbar sein. Durch diese Anforderung wird sichergestellt, dass der Benutzer zu jedem Zeitpunkt die Kontrolle behält. Verknüpfen Sie alle URI-Startvorgänge direkt mit der Benutzeroberfläche Ihrer App, um sicherzustellen, dass diese Anforderung erfüllt wird. Der Benutzer muss immer eine Aktion ausführen, um einen URI zu starten. Wenn Sie versuchen, einen URI zu starten, und Ihre App befindet sich nicht im Vordergrund, schlägt der Start fehl und Ihr Fehlerrückruf wird aufgerufen.

Erstellen Sie zunächst ein [**System.Uri**](/dotnet/api/system.uri)-Objekt, das den URI darstellt, und übergeben Sie es anschließend an die Methode [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync). Verwenden Sie das Ergebnis, um zu überprüfen, ob der Aufruf erfolgreich war, wie im folgenden Beispiel gezeigt.

```cs
private async void launchURI_Click(object sender, RoutedEventArgs e)
{
   // The URI to launch
   var uriBing = new Uri(@"http://www.bing.com");

   // Launch the URI
   var success = await Windows.System.Launcher.LaunchUriAsync(uriBing);

   if (success)
   {
      // URI launched
   }
   else
   {
      // URI launch failed
   }
}
```

In einigen Fällen wird der Benutzer vom Betriebssystem aufgefordert, zu überprüfen, ob die apps tatsächlich gewechselt werden sollen.

![Überlagerndes Warndialogfeld in der App auf grauem Hintergrund. Im Dialogfeld wird der Benutzer gefragt, ob die App gewechselt werden soll. Unten rechts sind die Schaltflächen „Ja“ und „Nein“ vorhanden. Die Schaltfläche „Nein“ ist hervorgehoben.](images/warningdialog.png)

Wenn diese Aufforderung immer auftreten soll, verwenden Sie das [**Windows.System. Launcheroptions. treatasuntrusted**](/uwp/api/windows.system.launcheroptions.treatasuntrusted) -Eigenschaft, die dem Betriebssystem mitteilt, dass eine Warnung angezeigt werden soll.

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### <a name="recommend-an-app-if-one-is-not-available-to-handle-the-uri"></a>Empfehlen einer App, wenn keine App zur Behandlung des URI verfügbar ist

Es kann jedoch sein, dass der Benutzer nicht über die erforderliche App zum Bearbeiten des aufgerufenen URI verfügt. In diesen Fällen bietet das Betriebssystem standardmäßig einen Link an, über den Benutzer im Store nach einer geeigneten App suchen können. Wenn Sie dem Benutzer eine bestimmte App für dieses spezifische Szenario empfehlen möchten, können Sie die Empfehlung zusammen mit dem gestarteten URI übergeben.

Empfehlungen sind auch nützlich, wenn mehr als eine App zum Behandeln eines URI-Schema registriert wurde. Durch die Empfehlung einer bestimmten App öffnet Windows die App, wenn sie bereits installiert ist.

Rufen Sie dazu die Methode [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](/uwp/api/windows.system.launcher.launchuriasync#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_) auf, wobei [**LauncherOptions.preferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) auf den Paketfamiliennamen der empfohlenen App im Store festgelegt ist. Diese Info wird vom Betriebssystem verwendet, um die allgemeine Option zum Suchen einer App im Store durch eine spezifische Option zum Erwerben der empfohlenen App im Store zu ersetzen.

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### <a name="set-remaining-view-preference"></a>Festlegen der verbleibenden Ansichtseinstellung

Quell-Apps, die [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) aufrufen, können anfordern, nach dem Start eines URIs auf dem Bildschirm zu verbleiben. Standardmäßig wird von Windows versucht, den gesamten verfügbaren Speicherplatz gleichmäßig zwischen der Quell- und der Ziel-App aufzuteilen, die den URI verarbeitet. Quell-Apps können die Eigenschaft [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) verwenden. Hiermit geben sie dem Betriebssystem an, mehr oder weniger des verfügbaren Speicherplatzes für ihr App-Fenster zu verwenden. **DesiredRemainingView** kann auch verwendet werden, um anzugeben, dass die Quell-App nach dem Start des URIs nicht auf dem Bildschirm verbleiben muss und vollständig durch die Ziel-App ersetzt werden kann. Mit dieser Eigenschaft wird nur die bevorzugte Fenstergröße der aufrufenden App angegeben. Es wird nicht das Verhalten anderer Apps angegeben, die ggf. zur gleichen Zeit auf dem Bildschirm angezeigt werden.

**Hinweis**  Windows bestimmt die endgültige Fenstergröße einer Quell-App anhand zahlreicher Faktoren (z. B. Einstellung der Quell-App, Anzahl der Apps auf dem Bildschirm, Bildschirmausrichtung usw.). Das Festlegen von [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) garantiert kein bestimmtes Fensterverhalten für die Quell-App.

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## <a name="uri-schemes"></a>URI-Schemas ##

Die verschiedenen URI-Schemas werden im Folgenden beschrieben.

### <a name="call-app-uri-scheme"></a>URI-Schema für das Aufrufen einer App

Verwenden Sie das Schema **MS-aufrufen:** URI, um die app "aufrufen" zu starten.

| URI-Schema       | Ergebnis                   |
|------------------|--------------------------|
| ms-call:settings | Einstellungsseite der Anruf-App. |

### <a name="email-uri-scheme"></a>URI-Schema für E-Mail

Verwenden Sie das Schema **mailto:** URI, um die Standard-Mail-APP zu starten.

| URI-Schema |Ergebnisse                          |
|------------|---------------------------------|
| mailto:    | Startet die Standard-E-Mail-App. |
| mailto: \[ e-Mail-Adresse\] | Startet die E-Mail-App und erstellt eine neue Nachricht mit der angegebenen E-Mail-Adresse in der Zeile „An“. Beachten Sie, dass die E-Mail nicht gesendet wird, bis der Benutzer auf „Senden“ tippt. |

### <a name="http-uri-scheme"></a>HTTP-URI-Schema

Verwenden Sie das **http:** URI-Schema, um den Standard Webbrowser zu starten.

| URI-Schema | Ergebnisse                           |
|------------|-----------------------------------|
| http:      | Startet den Standardwebbrowser. |

### <a name="maps-app-uri-schemes"></a>URI-Schemas für die Karten-App

Verwenden Sie die Schemas **bingmaps:**, **MS-Drive-to:** und **MS-Walk-to:** URI, um [die Windows Maps-APP](launch-maps-app.md) für bestimmte Zuordnungen, Richtungen und Suchergebnisse zu starten. Der folgende URI startet beispielsweise die Windows-Karten-App und zeigt eine über New York City zentrierte Karte an.

`bingmaps:?cp=40.726966~-74.006076`

![Ein Beispiel der Windows-Karten-App.](images/mapnyc.png)

Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](launch-maps-app.md). Informationen zum Verwenden des Kartensteuerelements in Ihrer eigenen App finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](../maps-and-location/display-maps.md).

### <a name="messaging-app-uri-scheme"></a>URI-Schema für die Messaging-App

Verwenden Sie das Schema **MS-Chat:** URI, um die Windows-Messaging-APP zu starten.

| URI-Schema |Ergebnisse |
|------------|--------|
| ms-chat:   | Startet die Messaging-App. |
| ms-chat:?ContactID={contacted}  |  Damit kann die Messaging-Anwendung mit den Informationen eines bestimmten Kontakts gestartet werden.   |
| ms-chat:?Body={body} | Damit kann die Messaging-Anwendung mit einer Zeichenfolge gestartet werden, die als Inhalt der Nachricht verwendet wird.|
| ms-chat:?Addresses={address}&Body={body} | Damit kann die Messaging-Anwendung mit bestimmten Adressinformationen und einer Zeichenfolge gestartet werden, die als Inhalt der Nachricht verwendet werden soll. Hinweis: Die Adressen können verkettet werden. |
| ms-chat:?TransportId={transportId}  | Ermöglicht das Starten der Messaging-Anwendung mit einer bestimmten Transport-ID. |

### <a name="tone-picker-uri-scheme"></a>URI-Schema für die Tonauswahl

Verwenden Sie das Schema **MS-tonepicker:** URI, um Ringtöne, Alarme und Systemtöne auszuwählen. Außerdem können Sie neue Klingeltöne speichern und den Anzeigenamen eines Tons abrufen.

| URI-Schema | Ergebnisse |
|------------|---------|
| ms-tonepicker: | Wählen Sie Klingeltöne, Alarmtöne und Systemtöne aus. |

Die Parameter werden über einen [ValueSet](/uwp/api/windows.foundation.collections.valueset) an die LaunchURI-API übergeben. Weitere Informationen finden Sie unter [Auswählen und Speichern von Tönen mit dem URI-Schema „ms-tonepicker“](launch-ringtone-picker.md).

### <a name="nearby-numbers-app-uri-scheme"></a>URI-Schema für die Nearby Numbers-App

Verwenden Sie das Schema **MS-yellowpage:** URI, um die app in der Nähe der APP zu starten.

| URI-Schema | Ergebnisse |
|------------|---------|
| MS-yellowpage:? input = \[ Schlüsselwort \]&Methode = \[ String oder T9\] | Startet die Nearby Numbers-App.<br>`input` bezieht sich auf das Schlüsselwort, das Sie durchsuchen möchten.<br>`method` bezieht sich auf den Suchtyp (String-oder T9-Suche).<br>Wenn `method``T9` ist (eine Art der Tastatur), dann sollte `keyword` eine numerische Zeichenfolge sein, die der T9-Tastatur Buchstaben zuordnet, nach denen gesucht werden soll.<br>Wenn `method``String` ist, dann ist `keyword` das Schlüsselwort, nach dem gesucht werden soll. |

### <a name="people-app-uri-scheme"></a>URI-Schema für die Kontakte-App

Verwenden Sie das Schema **MS-People:** URI, um die People-APP zu starten.
Weitere Informationen finden Sie unter [Starten der Kontakte-App](launch-people-apps.md).

### <a name="photos-app-uri-scheme"></a>Fotos-App-URI-Schema

Verwenden Sie das Schema **MS-Photos:** URI, um die Fotos-App zum Anzeigen eines Bilds oder zum Bearbeiten eines Videos zu starten. Beispiel:  
So zeigen Sie ein Bild an: `ms-photos:viewer?fileName=c:\users\userName\Pictures\image.jpg`  
Oder zum Bearbeiten eines Videos: `ms-photos:videoedit?InputToken=123abc&Action=Trim&StartTime=01:02:03`  

> [!NOTE]
> Die URIs zum Bearbeiten eines Videos oder zum Anzeigen eines Bilds sind nur auf dem Desktop verfügbar.

| URI-Schema |Ergebnisse |
|------------|--------|
| MS-Photos: Viewer? Dateiname = {filename} | Öffnet die Fotos-APP, um das angegebene Bild anzuzeigen, wobei "{filename}" ein voll qualifizierter Pfadname ist. Beispiel: `c:\users\userName\Pictures\ImageToView.jpg` |
| MS-Photos: VideoEdit? Inputtoken = {Eingabe Token} | Starten der Fotos-App im Video Bearbeitungsmodus für die Datei, die durch das Datei Token dargestellt wird. **Inputtoken** ist erforderlich. Verwenden Sie den  [sharedstorageaccessmanager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) , um ein Token für eine Datei abzurufen. |
| MS-Photos: VideoEdit? Action = {Action} | Ein Parameter, der den Bearbeitungsmodus des Videos in zum Öffnen der Fotos-app in angibt, wobei {Action} einer der folgenden ist: **Slowmotion**, **frameextraktion**, **Trim**, **View**, **Ink**. Eine **Aktion** ist erforderlich. |
| MS-Photos: VideoEdit? StartTime = {TimeSpan} | Ein optionaler Parameter, der angibt, wo mit dem Abspielen des Videos begonnen werden soll. `{timespan}` muss das Format aufweisen `"hh:mm:ss.ffff"` . Wenn nicht angegeben, wird standardmäßig `00:00:00.0000` |

### <a name="settings-app-uri-scheme"></a>URI-Schema für die Einstellungs-App

Verwenden Sie das Schema **MS-Settings:** URI, um [die Windows-Einstellungs-APP zu starten](launch-settings-app.md). Das Starten der Einstellungs-App ist ein wichtiger Bestandteil beim Schreiben einer datenschutzbewussten App. Wenn Ihre App nicht auf eine sensible Ressource zugreifen kann, wird empfohlen, dem Benutzer einen praktischen Link zu den Datenschutzeinstellungen für diese Ressource bereitzustellen. Folgender URI öffnet beispielsweise die Einstellungs-App und zeigt die Datenschutzeinstellungen für die Kamera an.

`ms-settings:privacy-webcam`

![Datenschutzeinstellungen für die Kamera.](images/privacyawarenesssettingsapp.png)

Weitere Informationen finden Sie unter [Starten der Einstellungs-App von Windows](launch-settings-app.md) und [Richtlinien für Apps mit Berücksichtigung von Datenschutz](../security/index.md).

### <a name="store-app-uri-scheme"></a>URI-Schema für die Store-App

Verwenden Sie das Schema **MS-Windows-Store:** URI, um [die UWP-APP zu starten](launch-store-app.md). Öffnen Sie Produktdetailseiten, Produkt Prüfungs Seiten und Suchseiten usw. Beispielsweise wird mit dem folgenden URI die UWP-app geöffnet und die Startseite des Stores gestartet.

`ms-windows-store://home/`

Weitere Informationen finden Sie unter [Starten der UWP-App](launch-store-app.md).

### <a name="weather-app-uri-scheme"></a>URI-Schema für Wetter-App

Verwenden Sie das Schema **msnweather:** URI, um die Wetter-App zu starten.

| URI-Schema | Ergebnisse |
|------------|---------|
| msnweather://Forecast? La = \[ Breitengrad \]&Lo = \[ Längengrad\] | Hiermit wird die Wetter-App auf der Seite "Vorhersage" basierend auf geografischen Koordinaten des Standorts gestartet.<br>`latitude` bezieht sich auf den Breitengrad der Position.<br> `longitude` bezieht sich auf den Längengrad der Position.<br> |

### <a name="microsoft-edge-uri-scheme"></a>Microsoft Edge-URI-Schema

Verwenden Sie das Schema **Microsoft-Edge:** URI, um den Microsoft Edge-Browser auf eine angegebene URL zu starten.

| URI-Schema | Ergebnisse |
|------------|---------|
| Microsoft-Edge: https://example.com/ ] | Öffnet den Microsoft Edge-Browser und navigiert zu https://example.com/<br> |
