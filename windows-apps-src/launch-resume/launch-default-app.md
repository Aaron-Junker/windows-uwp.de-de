---
title: Starten der Standard-App für einen URI
description: Erfahren Sie, wie Sie die Standard-App für einen Uniform Resource Identifier (URI) starten. URIs ermöglichen den Start einer anderen App zum Ausführen einer bestimmten Aufgabe. Dieses Thema enthält auch eine Übersicht über die zahlreichen in Windows integrierten URI-Schemas.
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.date: 06/26/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8cf682a77f9010f71adc9d6709bb345ac02270e5
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321821"
---
# <a name="launch-the-default-app-for-a-uri"></a>Starten der Standard-App für einen URI


**Wichtige APIs**

- [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)
- [**PreferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
- [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview)

Erfahren Sie, wie Sie die Standard-App für einen Uniform Resource Identifier (URI) starten. URIs ermöglichen den Start einer anderen App zum Ausführen einer bestimmten Aufgabe. Dieses Thema enthält auch eine Übersicht über die zahlreichen in Windows integrierten URI-Schemas. Sie können außerdem benutzerdefinierte URIs starten. Weitere Informationen zum Registrieren eines benutzerdefinierten URI-Schemas und Behandeln der URI-Aktivierung finden Sie unter [Behandeln der URI-Aktivierung](handle-uri-activation.md).

Mit URI-Schemas können Sie Apps öffnen, indem Sie auf Hyperlinks klicken. Genau wie Sie eine neue E-Mail mit **mailto:** öffnen, können Sie den Standardwebbrowser mit **http:** öffnen.

In diesem Thema werden einige der folgenden URI-Schemas beschrieben, die in Windows integriert sind:

| URI-Schema | Startet |
| ----------:|----------|
|[Bingmaps:, ms-Laufwerk-to:, und ms-Walk-to: ](#maps-app-uri-schemes) | Karten-App |
|[http:](#http-uri-scheme) | Standardwebbrowser |
|[mailto:](#email-uri-scheme) | Standard-E-Mail-App |
|[ms-call:](#call-app-uri-scheme) |  Anruf-App |
|[ms-chat:](#messaging-app-uri-scheme) | Messaging-App |
|[ms-people:](#people-app-uri-scheme) | Kontakte-App |
|[ms-photos:](#photos-app-uri-scheme) | Fotos-App |
|[ms-settings:](#settings-app-uri-scheme) | Einstellungs-App |
|[ms-store:](#store-app-uri-scheme)  | Store-App |
|[ms-tonepicker:](#tone-picker-uri-scheme) | Tonauswahl |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | Nearby Numbers-App |
|[msnweather:](#weather-app-uri-scheme) | Wetter-app |

<br>
Der folgende URI öffnet beispielsweise den Standardbrowser und zeigt die Bing-Website an.

`https://bing.com`

Sie können außerdem benutzerdefinierte URI-Schemas starten. Wenn zum Behandeln dieses URI keine App installiert ist, können Sie dem Benutzer die Installation einer App empfehlen. Weitere Informationen erhalten Sie unter [Empfehlen einer App, wenn keine App zur Behandlung des URI verfügbar ist](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

Im Allgemeinen kann die App nicht die App auswählen, die gestartet wird. Der Benutzer entscheidet, welche App gestartet wird. Zum Behandeln desselben URI-Schemas kann mehr als eine App registriert werden. Die einzige Ausnahme sind reservierte URI-Schemas. Registrierungen der reservierten URI-Schemas werden ignoriert. Die vollständige Liste der reservierten URI-Schemas finden Sie unter [Behandeln der URI-Aktivierung](handle-uri-activation.md). In Fällen, in denen mehrere Apps das gleiche URI-Schema registriert haben, kann Ihre App das Starten einer bestimmten App empfehlen. Weitere Informationen erhalten Sie unter [Empfehlen einer App, wenn keine App zur Behandlung des URI verfügbar ist](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

### <a name="call-launchuriasync-to-launch-a-uri"></a>Aufruf von „LaunchUriAsync“ zum Starten eines URI

Verwenden Sie die Methode [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync), um einen URI zu starten. Beim Aufrufen dieser Methode muss Ihre App im Vordergrund ausgeführt werden, d. h., sie muss für den Benutzer sichtbar sein. Durch diese Anforderung wird sichergestellt, dass der Benutzer zu jedem Zeitpunkt die Kontrolle behält. Verknüpfen Sie alle URI-Startvorgänge direkt mit der Benutzeroberfläche Ihrer App, um sicherzustellen, dass diese Anforderung erfüllt wird. Der Benutzer muss immer eine Aktion ausführen, um einen URI zu starten. Wenn Sie versuchen, einen URI zu starten, und Ihre App befindet sich nicht im Vordergrund, schlägt der Start fehl und Ihr Fehlerrückruf wird aufgerufen.

Erstellen Sie zunächst ein [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN)-Objekt, das den URI darstellt, und übergeben Sie es anschließend an die Methode [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync). Verwenden Sie das Ergebnis, um zu überprüfen, ob der Aufruf erfolgreich war, wie im folgenden Beispiel gezeigt.

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

In einigen Fällen fordert das Betriebssystem den Benutzer dazu auf, zu prüfen, ob er tatsächlich Apps wechseln möchte.

![Überlagerndes Warndialogfeld in der App auf grauem Hintergrund. Im Dialogfeld wird der Benutzer gefragt, ob die App gewechselt werden soll. Unten rechts sind die Schaltflächen „Ja“ und „Nein“ vorhanden. Die Schaltfläche „Nein“ ist hervorgehoben.](images/warningdialog.png)

Wenn diese Aufforderung stets erfolgen soll, verwenden Sie die Eigenschaft [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.treatasuntrusted), um anzugeben, dass das Betriebssystem eine Warnung anzeigen soll.

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

Rufen Sie dazu die Methode [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)** ](https://docs.microsoft.com/en-us/uwp/api/windows.system.launcher.launchuriasync#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_) auf, wobei [**LauncherOptions.preferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) auf den Paketfamiliennamen der empfohlenen App im Store festgelegt ist. Diese Info wird vom Betriebssystem verwendet, um die allgemeine Option zum Suchen einer App im Store durch eine spezifische Option zum Erwerben der empfohlenen App im Store zu ersetzen.

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

Quell-Apps, die [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) aufrufen, können anfordern, nach dem Start eines URIs auf dem Bildschirm zu verbleiben. Standardmäßig wird von Windows versucht, den gesamten verfügbaren Speicherplatz gleichmäßig zwischen der Quell- und der Ziel-App aufzuteilen, die den URI verarbeitet. Quell-Apps können die Eigenschaft [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview) verwenden. Hiermit geben sie dem Betriebssystem an, mehr oder weniger des verfügbaren Speicherplatzes für ihr App-Fenster zu verwenden. **DesiredRemainingView** kann auch verwendet werden, um anzugeben, dass die Quell-App nach dem Start des URIs nicht auf dem Bildschirm verbleiben muss und vollständig durch die Ziel-App ersetzt werden kann. Mit dieser Eigenschaft wird nur die bevorzugte Fenstergröße der aufrufenden App angegeben. Es wird nicht das Verhalten anderer Apps angegeben, die ggf. zur gleichen Zeit auf dem Bildschirm angezeigt werden.

**Beachten Sie**  Windows berücksichtigt mehrere verschiedene Faktoren, wenn es der quellanwendung abschließende Fenstergröße, z. B. ermittelt, die Einstellung der Quell-app, die Anzahl der apps auf dem Bildschirm, bildschirmausrichtung und So weiter. Das Festlegen von [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview) garantiert kein bestimmtes Fensterverhalten für die Quell-App.

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

Verwenden der **ms-Aufruf:** URI-Schema zum Starten der app aufrufen.

| URI-Schema       | Ergebnis                   |
|------------------|--------------------------|
| ms-call:settings | Einstellungsseite der Anruf-App. |

### <a name="email-uri-scheme"></a>URI-Schema für E-Mail

Verwenden der **Mailto:** URI-Schema, um die Standard-Mail-app zu starten.

| URI-Schema |Ergebnisse                          |
|------------|---------------------------------|
| mailto:    | Startet die Standard-E-Mail-App. |
| Mailto:\[e-Mail-Adresse\] | Startet die E-Mail-App und erstellt eine neue Nachricht mit der angegebenen E-Mail-Adresse in der Zeile „An“. Beachten Sie, dass die E-Mail nicht gesendet wird, bis der Benutzer auf „Senden“ tippt. |

### <a name="http-uri-scheme"></a>HTTP-URI-Schema

Verwenden der **http:** URI-Schema, um die Standard-Webbrowser zu starten.

| URI-Schema | Ergebnisse                           |
|------------|-----------------------------------|
| http:      | Startet den Standardwebbrowser. |

### <a name="maps-app-uri-schemes"></a>URI-Schemas für die Karten-App

Verwenden der **Bingmaps:** , **ms-Laufwerk-to:** , und **ms-Walk-to:** URI der Systeme auf [starten Sie die app Windows ordnet](launch-maps-app.md) spezifische Zuordnungen, Anweisungen und Suchergebnisse. Der folgende URI startet beispielsweise die Windows-Karten-App und zeigt eine über New York City zentrierte Karte an.

`bingmaps:?cp=40.726966~-74.006076`

![Ein Beispiel der Windows-Karten-App.](images/mapnyc.png)

Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](launch-maps-app.md). Informationen zum Verwenden des Kartensteuerelements in Ihrer eigenen App finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](https://docs.microsoft.com/windows/uwp/maps-and-location/display-maps).

### <a name="messaging-app-uri-scheme"></a>URI-Schema für die Messaging-App

Verwenden der **ms-Chat:** URI-Schema zum Starten der Windows-Messaging-app.

| URI-Schema |Ergebnisse |
|------------|--------|
| ms-chat:   | Startet die Messaging-App. |
| ms-chat:?ContactID={contacted}  |  Damit kann die Messaging-Anwendung mit den Informationen eines bestimmten Kontakts gestartet werden.   |
| ms-chat:?Body={body} | Damit kann die Messaging-Anwendung mit einer Zeichenfolge gestartet werden, die als Inhalt der Nachricht verwendet wird.|
| ms-chat:?Addresses={address}&Body={body} | Damit kann die Messaging-Anwendung mit bestimmten Adressinformationen und einer Zeichenfolge gestartet werden, die als Inhalt der Nachricht verwendet werden soll. Hinweis: Adressen können verkettet werden. |
| ms-chat:?TransportId={transportId}  | Ermöglicht das Starten der Messaging-Anwendung mit einer bestimmten Transport-ID. |

### <a name="tone-picker-uri-scheme"></a>URI-Schema für die Tonauswahl

Verwenden der **ms-Tonepicker:** URI-Schema Klingeltöne, Alarme und System-Töne auswählen. Außerdem können Sie neue Klingeltöne speichern und den Anzeigenamen eines Tons abrufen.

| URI-Schema | Ergebnisse |
|------------|---------|
| ms-tonepicker: | Wählen Sie Klingeltöne, Alarmtöne und Systemtöne aus. |

Die Parameter werden über einen [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) an die LaunchURI-API übergeben. Weitere Informationen finden Sie unter [Auswählen und Speichern von Tönen mit dem URI-Schema „ms-tonepicker“](launch-ringtone-picker.md).

### <a name="nearby-numbers-app-uri-scheme"></a>URI-Schema für die Nearby Numbers-App

Verwenden der **ms-Yellowpage:** URI-Schema zum Starten der app in Ihrer Nähe Zahlen.

| URI-Schema | Ergebnisse |
|------------|---------|
| MS-Yellowpage:? Eingabe =\[Schlüsselwort\]& Methode =\[Zeichenfolge oder T9\] | Startet die Nearby Numbers-App.<br>`input` bezieht sich auf das Schlüsselwort, die, das Sie suchen möchten.<br>`method` bezieht sich auf den Typ der Suche (Zeichenfolge oder T9 suchen).<br>Wenn `method``T9` ist (eine Art der Tastatur), dann sollte `keyword` eine numerische Zeichenfolge sein, die der T9-Tastatur Buchstaben zuordnet, nach denen gesucht werden soll.<br>Wenn `method``String` ist, dann ist `keyword` das Schlüsselwort, nach dem gesucht werden soll. |

### <a name="people-app-uri-scheme"></a>URI-Schema für die Kontakte-App

Verwenden der **ms-Personen:** URI-Schema, um die Kontakte-app zu starten.
Weitere Informationen finden Sie unter [Starten der Kontakte-App](launch-people-apps.md).

### <a name="photos-app-uri-scheme"></a>URI-Schema für die Fotos-App

Verwenden der **ms-Fotos:** URI-Schema, um die Fotos-app, um ein Bild anzeigen oder Bearbeiten eines Videos zu starten. Zum Beispiel:  
So zeigen Sie ein Bild an: `ms-photos:viewer?fileName=c:\users\userName\Pictures\image.jpg`  
Oder um ein Video zu bearbeiten: `ms-photos:videoedit?InputToken=123abc&Action=Trim&StartTime=01:02:03`  

> [!NOTE]
> Die URIs zum Bearbeiten von Videos oder Anzeigen von Bildern sind nur auf dem Desktop verfügbar.

| URI-Schema |Ergebnisse |
|------------|--------|
| ms-photos:viewer?fileName={filename} | Startet die Fotos-App, um das gewünschte Bild anzuzeigen. {filename} muss ein vollqualifizierter Pfadname sein. Beispiel: `c:\users\userName\Pictures\ImageToView.jpg` |
| ms-photos:videoedit?InputToken={input token} | Startet die Fotos-App im Videobearbeitungsmodus für die Datei, die durch das Dateitoken dargestellt wird. **InputToken** ist erforderlich. Sie können den [SharedStorageAccessManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) verwenden, um ein Dateitoken zu erhalten. |
| ms-photos:videoedit?Action={action} | Ein optionaler Parameter, der die Fotos-app in der angegebenen video Bearbeitungsmodus geöffnet wird, wobei {Action} gibt: **SlowMotion**, **FrameExtraction**, **Trim**, **View**, **Ink**. Wenn keine Angabe erfolgt, wird standardmäßig **Ansicht** verwendet |
| ms-photos:videoedit?StartTime={timespan} | Ein optionaler Parameter, der angibt, wo die Wiedergabe des Videos beginnt. `{timespan}` muss im Format `"hh:mm:ss.ffff"`. Wenn nicht angegeben ist, wird standardmäßig `00:00:00.0000` |

### <a name="settings-app-uri-scheme"></a>URI-Schema für die Einstellungs-App

Verwenden der **ms-Einstellungen:** URI-Schema, [starten Sie die Einstellungen für Windows-app](launch-settings-app.md). Das Starten der Einstellungs-App ist ein wichtiger Bestandteil beim Schreiben einer datenschutzbewussten App. Wenn Ihre App nicht auf eine sensible Ressource zugreifen kann, wird empfohlen, dem Benutzer einen praktischen Link zu den Datenschutzeinstellungen für diese Ressource bereitzustellen. Folgender URI öffnet beispielsweise die Einstellungs-App und zeigt die Datenschutzeinstellungen für die Kamera an.

`ms-settings:privacy-webcam`

![Datenschutzeinstellungen für die Kamera.](images/privacyawarenesssettingsapp.png)

Weitere Informationen finden Sie unter [Starten der Einstellungs-App von Windows](launch-settings-app.md) und [Richtlinien für Apps mit Berücksichtigung von Datenschutz](https://docs.microsoft.com/windows/uwp/security/index).

### <a name="store-app-uri-scheme"></a>URI-Schema für die Store-App

Verwenden der **ms-Windows-Store:** URI-Schema, [starten Sie die UWP-app](launch-store-app.md). Detailseiten Product, Product Review-Seiten, und Suchseiten usw. zu öffnen. Beispielsweise wird der folgende URI öffnet die UWP-app und startet die Startseite der Store.

`ms-windows-store://home/`

Weitere Informationen finden Sie unter [Starten der UWP-App](launch-store-app.md).

### <a name="weather-app-uri-scheme"></a>Wetter-app-URI-Schema

Verwenden der **Msnweather:** URI-Schema, um das Wetter-app zu starten.

| URI-Schema | Ergebnisse |
|------------|---------|
| msnweather://forecast?la=\[latitude\]&lo=\[longitude\] | Startet die Wetter-app in der Vorhersage-Seite, die basierend auf einen Speicherort geografische Koordinaten an.<br>`latitude` bezieht sich auf der Breitengrad des Standorts.<br> `longitude` bezieht sich auf der Längengrad des Standorts.<br> |
