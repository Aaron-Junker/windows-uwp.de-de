---
title: Starten der Windows-Einstellungs-App
description: Erfahren Sie, wie Sie die Windows-Einstellungs-App aus Ihrer App starten können. In diesem Thema wird das ms-settings-URI-Schema beschrieben. Verwenden Sie dieses URI-Schema, um die Windows-Einstellungs-App mit bestimmten Einstellungsseiten zu starten.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 01/08/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 87074e9871fae6be32e2862fc547097898baede8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646955"
---
# <a name="launch-the-windows-settings-app"></a>Starten der Windows-Einstellungs-App


**Wichtige APIs**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Erfahren Sie, wie Sie die Einstellungs-App von Windows starten. In diesem Thema wird beschrieben, die **ms-Einstellungen:** URI-Schema. Verwenden Sie dieses URI-Schema, um die Windows-Einstellungs-App mit bestimmten Einstellungsseiten zu starten.

Das Starten der Einstellungs-App ist ein wichtiger Bestandteil beim Schreiben einer datenschutzbewussten App. Wenn Ihre App nicht auf eine sensible Ressource zugreifen kann, wird empfohlen, dem Benutzer einen praktischen Link zu den Datenschutzeinstellungen für diese Ressource bereitzustellen. Weitere Informationen finden Sie unter [Richtlinien für Apps mit Berücksichtigung von Datenschutz](https://msdn.microsoft.com/library/windows/apps/hh768223).

## <a name="how-to-launch-the-settings-app"></a>So starten Sie die Einstellungs-App

Um die **Einstellungs**-App zu starten, verwenden Sie das in den folgenden Beispielen beschriebene URI-Schema `ms-settings:`.

In diesem Beispiel wird ein Hyperlink-XAML-Steuerelement verwendet, um die Datenschutzeinstellungsseite für das Mikrofon mit der `ms-settings:privacy-microphone`-URI zu starten.

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

Alternativ kann Ihre App die [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)-Methode aufrufen, um die **Einstellungs**-App zu starten. In diesem Beispiel wird gezeigt, wie die Datenschutzeinstellungsseite für die Kamera mit dem `ms-settings:privacy-webcam`-URI gestartet werden kann.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

Der eben gezeigte Code startet die Datenschutzeinstellungsseite für die Kamera:

![Datenschutzeinstellungen für die Kamera.](images/privacyawarenesssettingsapp.png)

Weitere Informationen zum Starten von URIs finden Sie unter [Starten der Standard-App für einen URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI-Schema-Verweis

Verwenden Sie die folgenden URIs, um verschiedenen Seiten der Einstellungs-App zu öffnen.

> Hinweis: ob die Seite "Einstellungen" verfügbar ist, variiert je nach Windows-SKU. Nicht alle Einstellungsseiten von Windows 10 für Desktop sind für Windows 10 Mobile verfügbar und umgekehrt. Der Bereich „Anmerkungen” in dieser Spalte erfasst auch zusätzliche Anforderungen, die erfüllt sein müssen, damit eine Seite zur Verfügung steht.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:cortana-windowssearch
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

## <a name="accounts"></a>Konten

|Seite „Einstellungen“| URI |
|-------------|-----|
| Geschäfts- oder Schulkonto öffnen | ms-settings:workplace |
| E-Mail- & App-Konten  | ms-settings:emailandaccounts |
| Familie und andere Personen | ms-settings:otherusers |
| Richten Sie ein kiosk | MS-Einstellungen: Assignedaccess |
| Anmeldeoptionen | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| Synchronisieren von Einstellungen | ms-settings:sync |
| Einrichten von Windows Hello | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| Ihre Informationen | ms-settings:yourinfo |

## <a name="apps"></a>Apps

|Seite „Einstellungen“| URI |
|-------------|-----|
| Apps & Features | ms-settings:appsfeatures |
| App-Features | ms-settings:appsfeatures-app (Zusätzliche und heruntergeladene Inhalte für die App zurücksetzen, verwalten etc.)|
| Apps für Websites | ms-settings:appsforwebsites |
| Standard-Apps | ms-settings:defaultapps |
| Optionale Funktionen verwalten | ms-settings:optionalfeatures |
| Offlinekarten | ms-settings:maps<br/>MS-Einstellungen: Maps-Downloadmaps (Download Maps) |
| Startup-Apps | ms-settings:startupapps |
| Videowiedergabe | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|Seite „Einstellungen“| URI |
|-------------|-----|
| Berechtigungen für & Verlauf | ms-settings:cortana-permissions |
| Weitere Details | ms-settings:cortana-moredetails |
| Cortana auf allen meinen Geräten | ms-settings:cortana-notifications |
| Sprechen mit Cortana | ms-settings:cortana-language<br/>MS-Einstellungen: Cortana<br/>MS-Einstellungen: Cortana-Talktocortana |

> [!NOTE] 
> In diesem Abschnitt "Einstellungen" auf Desktop wird Suche aufgerufen werden, wenn der PC in Regionen festgelegt wird, Cortana ist zurzeit nicht verfügbar oder Cortana wurde deaktiviert. Cortana-spezifische Seiten (Cortana auf meinen Geräten), und sich an Cortana werden in diesem Fall nicht aufgeführt. 

## <a name="devices"></a>Geräte

|Seite „Einstellungen“| URI |
|-------------|-----|
| Audio und Sprache | ms-settings:holographic-audio (nur verfügbar, wenn das App Mixed Reality-Portal installiert ist – verfügbar im Microsoft Store) |
| Automatische Wiedergabe | ms-settings:autoplay |
| Bluetooth | ms-settings:bluetooth |
| Verbundene Geräte | ms-settings:connecteddevices |
| Standardkamera | MS-Einstellungen: Kamera (**in Windows 10, Version 1809 und höher veraltet**) |
| Maus und Touchpad | ms-settings:mousetouchpad (Touchpadeinstellungen nur auf Geräten mit Touchpad verfügbar) |
| Stift & Windows Ink | ms-settings:pen |
| Drucker und Scanner | ms-settings:printers |
| Touchpad | ms-settings:devices-touchpad (Touchpadeinstellungen nur auf Geräten mit Touchpad verfügbar) |
| Tippen | ms-settings:typing |
| USB | ms-settings:usb |
| Rad | ms-settings:wheel (nur verfügbar, wenn das Rad gekoppelt ist) |
| Ihr Smartphone | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>Erleichterte Bedienung

|Seite „Einstellungen“| URI |
|-------------|-----|
| Audio | ms-settings:easeofaccess-audio |
| Untertitel | ms-settings:easeofaccess-closedcaptioning |
| Filter "Farbe" | MS-Einstellungen: Easeofaccess-Colorfilter |
| Größe des Cursors & Zeiger | MS-Einstellungen: Easeofaccess-Cursorandpointersize |
| Anzeige | ms-settings:easeofaccess-display |
| Augensteuerung | ms-settings:easeofaccess-eyecontrol |
| Schriftarten | ms-settings:fonts |
| Hoher Kontrast | ms-settings:easeofaccess-highcontrast |
| Holografisches Headset | ms-settings:holographic-headset (holografische Hardware erforderlich) |
| Tastatur | ms-settings:easeofaccess-keyboard |
| Bildschirmlupe | ms-settings:easeofaccess-magnifier |
| Maus | ms-settings:easeofaccess-mouse |
| Sprachausgabe | ms-settings:easeofaccess-narrator |
| Weitere Optionen | MS-Einstellungen: Easeofaccess-Otheroptions (**in Windows 10, Version 1809 und höher veraltet**) |
| Spracherkennung | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>Extras

|Seite „Einstellungen“| URI |
|-------------|-----|
| Extras | ms-settings:extras (nur verfügbar, wenn "Apps-Einstellungen" installiert sind (z. B. von Drittanbietern) |

## <a name="gaming"></a>Spiele

|Seite „Einstellungen“| URI |
|-------------|-----|
| Senden | ms-settings:gaming-broadcasting |
| Spieleleiste | ms-settings:gaming-gamebar |
| Game DVR | ms-settings:gaming-gamedvr |
| Spielmodus | ms-settings:gaming-gamemode |
| Ein Spiel im Vollbildmodus wiedergeben | ms-settings:quietmomentsgame |
| TruePlay | MS-Einstellungen: Gaming-Trueplay (**in Windows 10, Version 1809 und höher veraltet**) |
| Xbox Netzwerk | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>Startseite

|Seite „Einstellungen“| URI |
|-------------|-----|
| Startseite „Einstellungen“ | ms-settings: |


## <a name="network--internet"></a>Netzwerk und Internet

|Seite „Einstellungen“| URI |
|-------------|-----|
| Flugzeugmodus | ms-settings:network-airplanemode<br/>ms-settings:proximity |
| Mobilfunk und SIM | ms-settings:network-cellular |
| Datennutzung | ms-settings:datausage |
| DFÜ-Verbindung | ms-settings:network-dialup |
| DirectAccess | ms-Settings:network-directaccess (nur verfügbar, wenn die DirectAccess aktiviert ist) |
| Ethernet | ms-settings:network-ethernet |
| Bekannte Netzwerke verwalten | ms-settings:network-wifisettings |
| Mobiler Hotspot | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| Status | ms-settings:network-status<br/>MS-Einstellungen: Netzwerk |
| VPN | ms-settings:network-vpn |
| WLAN | ms-settings:network-wifi (nur verfügbar, wenn das Gerät über einen WLAN-Adapter verfügt) |
| WLAN-Anruf | ms-settings:network-wificalling (nur verfügbar, wenn WLAN-Anruf aktiviert ist) |

## <a name="personalization"></a>Personalization

|Seite „Einstellungen“| URI |
|-------------|-----|
| Hintergrund | ms-settings:personalization-background |
| Ordner auswählen, die im Menü „Start“ angezeigt werden | ms-settings:personalization-start-places |
| Farben | ms-settings:personalization-colors<br/>MS-Einstellungen: Farben |
| Blick | MS-Einstellungen: Personalisierung-Blick (**in Windows 10, Version 1809 und höher veraltet**) |
| Sperrbildschirm | ms-settings:lockscreen |
| Navigationsleiste | MS-Einstellungen: Personalisierung-Navigationsleiste (**in Windows 10, Version 1809 und höher veraltet**) |
| Personalisierung (Kategorie) | ms-settings:personalization |
| Beginn | ms-settings:personalization-start |
| Taskleiste | ms-settings:taskbar |
| Designs | ms-settings:themes |

## <a name="phone"></a>Phone

|Seite „Einstellungen“| URI |
|-------------|-----|
| Ihr Smartphone | ms-settings:mobile-devices<br/>MS-Einstellungen: Mobile-Geräte-Addphone<br/>MS-Einstellungen: Mobile-Geräte-Addphone-direct (öffnet **Ihr Telefon** app) |

## <a name="privacy"></a>Vertraulichkeit

|Seite „Einstellungen“| URI |
|-------------|-----|
| Zubehör-Apps | MS-Einstellungen: Datenschutz-Accessoryapps (**in Windows 10, Version 1809 und höher veraltet**) |
| Kontoinformationen | ms-settings:privacy-accountinfo |
| Aktivitätsverlauf | ms-settings:privacy-activityhistory |
| Werbe-ID | MS-Einstellungen: Datenschutz-Advertisingid (**in Windows 10, Version 1809 und höher veraltet**) |
| App-Diagnose | ms-settings:privacy-appdiagnostics |
| Automatische Dateidownloads | ms-settings:privacy-automaticfiledownloads |
| Hintergrund-Apps | ms-settings:privacy-backgroundapps |
| Calendar | ms-settings:privacy-calendar |
| Anrufliste | ms-settings:privacy-callhistory |
| Kamera | ms-settings:privacy-webcam |
| Kontakte | ms-settings:privacy-contacts |
| Dokumente | ms-settings:privacy-documents |
| E-Mail | ms-settings:privacy-email |
| Eyetracker. | ms-settings:privacy-eyetracker (Eyetracker-Hardware erforderlich) |
| Feedback und Diagnose | ms-settings:privacy-feedback |
| Dateisystem | ms-settings:privacy-broadfilesystemaccess |
| Allgemein | ms-settings:privacy-general |
| Pfad | ms-settings:privacy-location |
| Messaging | ms-settings:privacy-messaging |
| Mikrofon | ms-settings:privacy-microphone |
| Bewegung | ms-settings:privacy-motion |
| Benachrichtigungen | ms-settings:privacy-notifications |
| Weitere Geräte | ms-settings:privacy-customdevices |
| Bilder | ms-settings:privacy-pictures |
| Telefonanrufe | MS-Einstellungen: Datenschutz-Phonecall (**in Windows 10, Version 1809 und höher veraltet**) |
| Funkempfang | ms-settings:privacy-radios |
| Spracherkennung, Freihand und Eingabe |ms-settings:privacy-speechtyping |
| Aufgaben | ms-settings:privacy-tasks |
| Videos | ms-settings:privacy-videos |

## <a name="surface-hub"></a>Surface Hub

|Seite „Einstellungen“| URI |
|-------------|-----|
| Konten | ms-settings:surfacehub-accounts |
| Bereinigung der Sitzung | ms-settings:surfacehub-sessioncleanup |
| Team-Konferenz | ms-settings:surfacehub-calling |
| Team-Geräteverwaltung | ms-settings:surfacehub-devicemanagenent |
| Willkommensseite | ms-settings:surfacehub-welcome |

## <a name="system"></a>System

|Seite „Einstellungen“| URI |
|-------------|-----|
| Info | ms-settings:about |
| Erweiterte Anzeigeeinstellungen | ms-settings:display-advanced (nur verfügbar auf Geräten, die erweiterte Anzeigeeinstellungen) |
| Stromsparmodus | ms-settings:batterysaver (nur verfügbar auf Geräten mit Akku, z. B. Tablets) |
| Einstellungen „Stromsparmodus” | ms-settings:batterysaver-settings (nur verfügbar auf Geräten mit Akku, z. B. Tablets) |
| Akkubetrieb | ms-settings:batterysaver-usagedetails (nur verfügbar auf Geräten mit Akku, z. B. Tablets) |
| Zwischenablage | MS-Einstellungen für Zwischenablage: |
| Anzeige | ms-settings:display |
| Standard-Speicherorte | ms-settings:savelocations |
| Anzeige | ms-settings:screenrotation |
| Duplizieren meines Bildschirms | ms-Settings:quietmomentspresentation |
| Zu dieser Zeit | ms-settings:quietmomentsscheduled |
| Verschlüsselung | ms-settings:deviceencryption |
| Fokus-Assist | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| Einstellungen für Grafiken | ms-settings:display-advancedgraphics (nur verfügbar auf Geräten, die erweiterte Anzeigeeinstellungen) |
| Messaging | ms-settings:messaging |
| Multitasking | ms-settings:multitasking |
| Einstellungen „Nachtmodus” | ms-settings:nightlight |
| Phone | ms-settings:phone-defaultapps |
| Projizieren auf diesen PC | ms-settings:project |
| Gemeinsame Erfahrung | ms-settings:crossdevice |
| Tabletmodus | ms-settings:tabletmode |
| Taskleiste | ms-settings:taskbar |
| Benachrichtigungen & Infos | ms-settings:notifications |
| Remotedesktop | Einstellungen „Remotedesktop” |
| Phone | MS-Einstellungen: Phone (**in Windows 10, Version 1809 und höher veraltet**) |
| Ein/Aus und Standbymodus | ms-settings:powersleep |
| Sound | MS-Einstellungen: Sound |
| Speicher | ms-settings:storagesense |
| Speicheroptimierung | ms-settings:storagepolicies |

## <a name="time-and-language"></a>Zeit und Sprache

|Seite „Einstellungen“| URI |
|-------------|-----|
| Datum und Uhrzeit | ms-settings:dateandtime |
| Japan IME-Einstellungen | ms-settings:regionlanguage-jpnime (nur verfügbar, wenn der Eingabemethodeneditor „Microsoft Japan” installiert ist) |
| Sprache | MS-Einstellungen: Tastenkombinationen<br/>ms-settings:regionlanguage<br/>MS-Einstellungen: Regionlanguage-Bpmfime<br/>MS-Einstellungen: Regionlanguage-Cangjieime<br/>MS-Einstellungen: Regionlanguage-Chsime-Pinyin-Domainlexicon<br/>MS-Einstellungen: Regionlanguage-Chsime-Pinyin-Keyconfig<br/>MS-Einstellungen: Regionlanguage-Chsime-Pinyin-Udp<br/>MS-Einstellungen: Regionlanguage-Chsime-Wubi – Udp<br/>MS-Einstellungen: Regionlanguage-Quickime |
| Pinyin-IME-Einstellungen | ms-settings:regionlanguage-chsime-pinyin (nur verfügbar, wenn der Eingabemethodeneditor „Microsoft Pinyin” installiert ist) |
| Spracherkennung | ms-settings:speech |
| Wubi-IME-Einstellungen  | ms-settings:regionlanguage-chsime-wubi (nur verfügbar, wenn der Eingabemethodeneditor „Microsoft Wubi” installiert ist) |

## <a name="update--security"></a>Update und Sicherheit

|Seite „Einstellungen“| URI |
|-------------|-----|
| Aktivierung | ms-settings:activation |
| Sicherung | ms-settings:backup |
| Übermittlungsoptimierung | ms-settings:delivery-optimization |
| Mein Gerät suchen | ms-settings:findmydevice |
| Für Entwickler | ms-settings:developers |
| Wiederherstellung | ms-settings:recovery |
| Problembehandlung | ms-settings:troubleshoot |
| Windows-Sicherheit | ms-settings:windowsdefender |
| Windows-Insider-Programm | ms-settings:windowsinsider (nur verfügbar, wenn der Benutzer bei WIP registriert ist)<br/>MS-Einstellungen: Windowsinsider-Optin |
| Windows Update | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Windows Update-Advanced options | ms-settings:windowsupdate-options |
| Windows Update-Restart options | ms-settings:windowsupdate-restartoptions |
| Windows-Update – Updateverlauf anzeigen | ms-settings:windowsupdate-history |

## <a name="user--accounts"></a>Benutzerkonten

|Seite „Einstellungen“| URI |
|-------------|-----|
| Bereitstellung | ms-settings:workplace-provisioning (nur verfügbar, wenn das Unternehmen ein Bereitstellungspaket bereitgestellt hat) |
| Bereitstellung | ms-settings:provisioning (nur mobil verfügbar und nur, wenn das Unternehmen ein Bereitstellungspaket bereitgestellt hat) |
| Windows Anywhere | ms-settings:windowsanywhere (Gerät muss Windows Anywhere-fähig sein) |
