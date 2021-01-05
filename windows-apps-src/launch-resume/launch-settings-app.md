---
title: Starten der Windows-Einstellungs-App
description: Erfahren Sie, wie Sie die Windows-Einstellungs-APP über Ihre APP mit dem MS-Settings-URI-Schema starten.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 11/18/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: contperf-fy21q2
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 78d843ff9bdd0625b7172af0bc973581d8bd988a
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860413"
---
# <a name="launch-the-windows-settings-app"></a>Starten der Windows-Einstellungs-App

**Wichtige APIs**

-   [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)
-   [**PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
-   [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)

Erfahren Sie, wie Sie die Einstellungs-App von Windows starten. In diesem Thema wird das Schema **MS-Settings:** URI beschrieben. Verwenden Sie dieses URI-Schema, um die Windows-Einstellungs-App mit bestimmten Einstellungsseiten zu starten.

Das Starten der Einstellungs-App ist ein wichtiger Bestandteil beim Schreiben einer datenschutzbewussten App. Wenn Ihre App nicht auf eine sensible Ressource zugreifen kann, wird empfohlen, dem Benutzer einen praktischen Link zu den Datenschutzeinstellungen für diese Ressource bereitzustellen. Weitere Informationen finden Sie unter [Richtlinien für Apps mit Berücksichtigung von Datenschutz](../security/index.md).

## <a name="how-to-launch-the-settings-app"></a>So starten Sie die Einstellungs-App

Um die **Einstellungs**-App zu starten, verwenden Sie das in den folgenden Beispielen beschriebene URI-Schema `ms-settings:`.

### <a name="xaml-hyperlink-control"></a>XAML-Hyperlink-Steuerelement

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

### <a name="calling-launchuriasync"></a>Launchuriasync wird aufgerufen

Alternativ kann die APP die [**launchuriasync**](/uwp/api/windows.system.launcher.launchuriasync) -Methode aufrufen, um die app " **Einstellungen** " zu starten. In diesem Beispiel wird gezeigt, wie die Datenschutzeinstellungsseite für die Kamera mit dem `ms-settings:privacy-webcam`-URI gestartet werden kann.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

```cppwinrt
bool result = co_await Windows::System::Launcher::LaunchUriAsync(Windows::Foundation::Uri(L"ms-settings:privacy-webcam"));
```

Der eben gezeigte Code startet die Datenschutzeinstellungsseite für die Kamera:

:::image type="content" source="images/privacyawarenesssettingsapp.png" alt-text="Datenschutzeinstellungen für die Kamera.":::

Weitere Informationen zum Starten von URIs finden Sie unter [Starten der Standard-App für einen URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: URI-Schemareferenz

In den folgenden Abschnitten werden verschiedene Kategorien von MS-Settings-URIs beschrieben, die zum Öffnen verschiedener Seiten der App "Einstellungen" verwendet werden:

* [Konten](#accounts)
* [Apps](#apps)
* [Cortana](#cortana)
* [Geräte](#devices)
* [Erleichterte Bedienung](#ease-of-access)
* [Extras](#extras)
* [Spiele](#gaming)
* [Startseite](#home-page)
* [Mixed Reality](#mixed-reality)
* [Netzwerk und Internet](#network-and-internet)
* [Personalisierung](#personalization)
* [Phone](#phone)
* [Datenschutz](#privacy)
* [Surface Hub](#surface-hub)
* [System](#system)
* [Zeit und Sprache](#time-and-language)
* [Update und Sicherheit](#update-and-security)
* [Benutzerkonten](#user-accounts)



> [!NOTE]
> Ob eine Einstellungsseite verfügbar ist, hängt von der Windows-SKU ab. Nicht alle Seite "Einstellungen", die unter Windows 10 für Desktop verfügbar ist, sind unter Windows 10 Mobile verfügbar und umgekehrt. In der Spalte Notizen werden auch zusätzliche Anforderungen erfasst, die erfüllt sein müssen, damit eine Seite verfügbar ist.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

### <a name="accounts"></a>Konten

|Seite "Einstellungen"| URI |
|-------------|-----|
| Auf Arbeits- oder Schulkonto zugreifen | ms-settings:workplace |
| E-Mail- und App-Kontos  | ms-settings:emailandaccounts |
| Familie und andere Personen | ms-settings:otherusers |
| Einrichten eines Kiosks | MS-Settings: assignedaccess |
| Anmeldeoptionen | ms-settings:signinoptions<br>MS-Settings: signinoptions-dynamiclock |
| Synchronisieren Ihrer Einstellungen | ms-settings:sync |
| Windows Hello-Setup | MS-Settings: signinoptions-launchfaceregistriment<br>MS-Settings: signinoptions-launchfingerprintenrollment |
| Ihre Daten | ms-settings:yourinfo |

### <a name="apps"></a>Apps

|Seite "Einstellungen"| URI |
|-------------|-----|
| Apps & Features | MS-Settings: appsfeatures |
| App-Features | MS-Settings: appsfeatures-app (zurücksetzen, Add-on & herunterladbare Inhalte usw. für die APP)|
| Apps für Websites | MS-Settings: appsforwebsites |
| Standard-Apps | MS-Settings: defaultapps |
| Optionale Features verwalten | MS-Settings: OptionalFeatures |
| Offlinekarten | ms-settings:maps<br/>MS-Settings: Maps-downloadmaps (Karten herunterladen) |
| Start-apps | MS-Settings: StartupApps |
| Videowiedergabe | MS-Settings: videoplayback |

### <a name="cortana"></a>Cortana

|Seite "Einstellungen"| URI |
|-------------|-----|
| Cortana auf allen meinen Geräten | MS-Settings: Cortana-Benachrichtigungen |
| Weitere Informationen | MS-Settings: Cortana-moredetails |
| Berechtigungen & Verlauf | MS-Settings: Cortana-Berechtigungen |
| Suchen von Fenstern | MS-Settings: Cortana-Windows Search |
| Mit Cortana kommunizieren | MS-Settings: Cortana-Sprache<br/>MS-Settings: Cortana<br/>MS-Settings: Cortana-talkdecortana |

> [!NOTE] 
> Der Abschnitt "Einstellungen" auf dem Desktop wird "suchen" genannt, wenn der PC auf Regionen festgelegt ist, in denen Cortana zurzeit nicht verfügbar ist oder Cortana deaktiviert wurde. Cortana-spezifische Seiten (Cortana auf meinen Geräten und Talk mit Cortana) werden in diesem Fall nicht aufgeführt. 

### <a name="devices"></a>Geräte

|Seite "Einstellungen"| URI |
|-------------|-----|
| Automatische Wiedergabe | MS-Settings: AutoPlay |
| Bluetooth | ms-settings:bluetooth |
| Verbundene Geräte | ms-settings:connecteddevices |
| Standardkamera | MS-Settings: Kamera (**veraltet in Windows 10, Version 1809 und** höher) |
| Maus und Touchpad | MS-Settings: moustouchpad (Touchpad-Einstellungen sind nur auf Geräten verfügbar, die über ein Touchpad verfügen) |
| Stift & Windows-Ink | MS-Settings: Stift |
| Drucker & Scanner | MS-Settings: Drucker |
| Touchpad | MS-Settings: Devices-Touchpad (nur verfügbar, wenn Touchpad-Hardware vorhanden ist) |
| Eingabe | MS-Settings: Eingabe |
| USB | MS-Settings: USB |
| Wheel | MS-Settings: Wheel (nur verfügbar, wenn die Ziffer gekoppelt ist) |
| Ihr Telefon | MS-Settings: Mobile Geräte  |

### <a name="ease-of-access"></a>Erleichterte Bedienung

|Seite "Einstellungen"| URI |
|-------------|-----|
| Audio | MS-Settings: easeofakcess-Audiodaten |
| Untertitel für Hörgeschädigte | ms-settings:easeofaccess-closedcaptioning |
| Farbfilter | MS-Settings: easeofakcess-ColorFilter |
| Cursor & Zeiger | MS-Settings: easeofakcess-currsorandpointersize |
| Anzeige | MS-Settings: easeofakcess-Display |
| Eye-Steuerelement | MS-Settings: easeofakcess-eyecontrol |
| Schriftarten | MS-Settings: Schriftarten |
| Hoher Kontrast | ms-settings:easeofaccess-highcontrast |
| Tastatur | ms-settings:easeofaccess-keyboard |
| Bildschirmlupe | ms-settings:easeofaccess-magnifier |
| Maus | ms-settings:easeofaccess-mouse |
| Narrator | ms-settings:easeofaccess-narrator |
| Weitere Optionen | MS-Settings: easeofakcess-otheroptions (**veraltet in Windows 10, Version 1809 und** höher) |
| Spracheingabe/-ausgabe | MS-Settings: easeofakcess-Sprechererkennung |

### <a name="extras"></a>Extras

|Seite "Einstellungen"| URI |
|-------------|-----|
| Extras | MS-Settings: Extras (nur verfügbar, wenn "Einstellungs-Apps" installiert ist, z. b. von einem Drittanbieter) |

### <a name="gaming"></a>Spiele

|Seite "Einstellungen"| URI |
|-------------|-----|
| Übertragung | MS-Settings: Gaming-Broadcast |
| Spiel Leiste | MS-Settings: Gaming-gamebar |
| Game DVR | MS-Settings: Gaming-gamedvr |
| Spielmodus | MS-Settings: Gaming-Gamemode |
| Abspielen eines Spiel Vollbild | MS-Settings: quietmomentsgame |
| TruePlay | MS-Settings: Gaming-trueplay (**ab Windows 10, Version 1809 (10,0; Build 17763), diese Funktion wird aus Windows entfernt**.) |
| Xbox-Netzwerke | MS-Settings: Gaming-xboxnetworking |

### <a name="home-page"></a>Startseite

|Seite "Einstellungen"| URI |
|-------------|-----|
| Startseite für Einstellungen | ms-settings: |

### <a name="mixed-reality"></a>Mixed Reality

> [!NOTE]
> Diese Einstellungen sind nur verfügbar, wenn die Mixed Reality Portal-App installiert ist.

| Seite "Einstellungen" | URI |
|---------------|-----|
| Audiodaten und Spracheingaben | MS-Settings: Holographic-Audiodaten |
| Umgebung | MS-Settings: Privacy-Holographic-Environment |
| Headset-Anzeige | MS-Settings: Holographic-Headset |
| Deinstallieren | MS-Settings: Holographic-Management |

### <a name="network-and-internet"></a>Netzwerk und Internet

|Seite "Einstellungen"| URI |
|-------------|-----|
| Flugzeugmodus | ms-settings:network-airplanemode<br/>ms-settings:proximity |
| Mobilfunk und SIM | ms-settings:network-cellular |
| Datennutzung | ms-settings:datausage |
| Einwählgerät | ms-settings:network-dialup |
| DirectAccess | MS-Settings: Network-DirectAccess (nur verfügbar, wenn DirectAccess aktiviert ist) |
| Ethernet | ms-settings:network-ethernet |
| Bekannte Netzwerke verwalten | MS-Settings: Network-wifsettings |
| Mobiler Hotspot | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| Status | ms-settings:network-status<br/>MS-Settings: Netzwerk |
| VPN | MS-Settings: Netzwerk-VPN |
| WLAN | MS-Settings: Network-WiFi (nur verfügbar, wenn das Gerät über einen WiFi-Adapter verfügt) |
| Wi-Fi aufrufen | MS-Settings: Network-wifcall (nur verfügbar, wenn Wi-Fi Aufrufen von aktiviert ist) |

### <a name="personalization"></a>Personalization

|Seite "Einstellungen"| URI |
|-------------|-----|
| Hintergrund | ms-settings:personalization-background |
| Auswählen der Ordner, die beim Start angezeigt werden | MS-Settings: Personalization-Start-Places |
| Farben | ms-settings:personalization-colors<br/>MS-Settings: Farben |
| Mag | MS-Settings: Personalization-Blick (**veraltet in Windows 10, Version 1809 und** höher) |
| Sperrbildschirm | ms-settings:lockscreen |
| Navigationsleiste | MS-Settings: Personalization-navbar (**veraltet in Windows 10, Version 1809 und** höher) |
| Personalisierung (Kategorie) | ms-settings:personalization |
| Start | MS-Settings: Personalization-Start |
| Taskleiste | MS-Settings: Taskleiste |
| Designs | MS-Settings: Designs |

### <a name="phone"></a>Phone

|Seite "Einstellungen"| URI |
|-------------|-----|
| Ihr Telefon | MS-Settings: Mobile Geräte<br/>MS-Settings: Mobile-Devices-addphone<br/>MS-Settings: Mobile-Devices-addphone-Direct (öffnet **Ihre Phone** -APP) |

### <a name="privacy"></a>Datenschutz

|Seite "Einstellungen"| URI |
|-------------|-----|
| Zubehör-Apps | MS-Settings: Privacy-accessoryapps (**veraltet in Windows 10, Version 1809 und** höher) |
| Kontoinformationen | ms-settings:privacy-accountinfo |
| Aktivitätsverlauf | MS-Settings: Privacy-activityhistory |
| Werbe-ID | MS-Settings: Privacy-Werbung-Werbung (**veraltet in Windows 10, Version 1809 und** höher) |
| App-Diagnosen | MS-Settings: Privacy-appdiagnostics |
| Automatisches Herunterladen von Dateien | MS-Settings: Privacy-automaticfiledownloads |
| Hintergrund-Apps | ms-settings:privacy-backgroundapps |
| Calendar | ms-settings:privacy-calendar |
| Anrufliste | ms-settings:privacy-callhistory |
| Kamera | ms-settings:privacy-webcam |
| Kontakte | ms-settings:privacy-contacts |
| Dokumente | MS-Settings: Privacy-Documents |
| E-Mail | ms-settings:privacy-email |
| Eyetracker. | MS-Settings: Privacy-Eyetracker (erfordert die "pipetracker"-Hardware) |
| Feedback und Diagnose | ms-settings:privacy-feedback |
| Dateisystem | MS-Settings: Privacy-broadfilesystemaccess |
| Allgemein | MS-Settings: Privacy oder MS-Settings: Privacy (allgemein) |
| Eingabe & Eingabe |ms-settings:privacy-speechtyping |
| Standort | ms-settings:privacy-location |
| Nachrichten | ms-settings:privacy-messaging |
| Mikrofon | ms-settings:privacy-microphone |
| Bewegung | ms-settings:privacy-motion |
| Benachrichtigungen | MS-Settings: Privacy-Benachrichtigungen |
| Weitere Geräte | ms-settings:privacy-customdevices |
| Telefonanrufe | MS-Settings: Privacy-phonecalls |
| Bilder | MS-Settings: Privacy-Bilder |
| Funkempfang | ms-settings:privacy-radios |
| Spracheingabe/-ausgabe | MS-Settings: Privacy-Speech |
| Aufgaben | MS-Settings: Privacy-Tasks |
| Videos | MS-Settings: Privacy-Videos |
| Sprach Aktivierung | MS-Settings: Privacy-voiceactivation |

### <a name="surface-hub"></a>Surface Hub

|Seite "Einstellungen"| URI |
|-------------|-----|
| Konten | MS-Settings: surfakehub-Accounts |
| Sitzungs Bereinigung | MS-Settings: surfakehub-sessioncleanup |
| Team Konferenzen | MS-Settings: surfakehub-Aufruf |
| Team Geräteverwaltung | MS-Settings: surfakehub-devicemanagenent |
| Bildschirm „Willkommen“ | MS-Settings: surfakehub-Willkommen |

### <a name="system"></a>System

|Seite "Einstellungen"| URI |
|-------------|-----|
| Info | ms-settings:about |
| Erweiterte Anzeigeeinstellungen | MS-Settings: Display-Advanced (nur auf Geräten verfügbar, die Erweiterte Anzeigeoptionen unterstützen) |
| App-Volumen und Geräteeinstellungen | MS-Settings: apps-Volume (**in Windows 10, Version 1903 hinzugefügt**)|
| Stromsparmodus | MS-Settings: Akku Schoner (nur auf Geräten verfügbar, die über einen Akku verfügen, z. b. ein Tablet) |
| Akku Spar Einstellungen | MS-Settings: Akku Schoner-Settings (nur verfügbar auf Geräten, die über einen Akku verfügen, z. b. ein Tablet) |
| Akkubetrieb | MS-Settings: Akku Schoner-usagedetails (nur auf Geräten verfügbar, die über einen Akku verfügen, z. b. ein Tablet) |
| Zwischenablage | MS-Settings: Zwischenablage |
| Anzeige | ms-settings:display |
| Standard Speicherorte | MS-Settings: savelocations |
| Anzeige | ms-settings:screenrotation |
| Duplizieren der Anzeige | MS-Settings: quietmomentationsentations |
| Während dieser Stunden | MS-Settings: quietmomentsscheduled |
| Verschlüsselung | ms-settings:deviceencryption |
| Benachrichtigungsassistent | MS-Settings: quiethours <br> MS-Settings: quietmomentshome |
| Grafikeinstellungen | MS-Settings: Display-advancedgraphics (nur auf Geräten verfügbar, die erweiterte Grafikoptionen unterstützen) |
| Nachrichten | ms-settings:messaging |
| Multitasking | MS-Settings: Multitasking |
| Nachtlicht Einstellungen | MS-Settings: Nightlight |
| Phone | MS-Settings: Phone-defaultapps |
| Projizieren auf diesen PC | MS-Settings: Projekt |
| Gemeinsame Erfahrung | MS-Settings: crossdevice |
| Tablet-Modus | MS-Settings: tabletmode |
| Taskleiste | MS-Settings: Taskleiste |
| Benachrichtigungen & Infos | ms-settings:notifications |
| Remotedesktop | MS-Settings: Remotedesktop |
| Phone | MS-Settings: Phone (**veraltet in Windows 10, Version 1809 und** höher) |
| Ein/Aus und Standbymodus | ms-settings:powersleep |
| Sound | MS-Settings: Sound |
| Storage | ms-settings:storagesense |
| Speicheroptimierung | MS-Settings: storagepolicies |

### <a name="time-and-language"></a>Zeit und Sprache

|Seite "Einstellungen"| URI |
|-------------|-----|
| Datum und Uhrzeit | ms-settings:dateandtime |
| Einstellungen für das IME | MS-Settings: regionlanguage-jpnime (verfügbar, wenn der Microsoft-Eingabemethoden-Editor von Microsoft installiert ist) |
| Region | MS-Settings: regionformatierung |
| Sprache | MS-Settings: Tastatur<br/>ms-settings:regionlanguage<br/>MS-Settings: regionlanguage-bpmfme<br/>MS-Settings: regionlanguage-cangjieime<br/>MS-Settings: regionlanguage-chsime-Pinyin-domainlexicon<br/>MS-Settings: regionlanguage-chsime-Pinyin-keyconfig<br/>MS-Settings: regionlanguage-chsime-Pinyin-UDP<br/>MS-Settings: regionlanguage-chsime-Wubi-UDP<br/>MS-Settings: regionlanguage-quickime |
| Pinyin IME-Einstellungen | MS-Settings: regionlanguage-chsime-Pinyin (verfügbar, wenn der Microsoft Pinyin-Eingabemethoden-Editor installiert ist) |
| Spracheingabe/-ausgabe | ms-settings:speech |
| Wubi-IME-Einstellungen  | MS-Settings: regionlanguage-chsime-Wubi (verfügbar, wenn der Microsoft Wubi-Eingabemethoden-Editor installiert ist) |

### <a name="update-and-security"></a>Update und Sicherheit

|Seite "Einstellungen"| URI |
|-------------|-----|
| Aktivierung | MS-Settings: Aktivierung |
| Backup | MS-Settings: Backup |
| Übermittlungsoptimierung | MS-Settings: Delivery-Optimierung |
| Mein Gerät suchen | MS-Settings: findmydevice |
| Für Entwickler | ms-settings:developers |
| Wiederherstellung | MS-Settings: Wiederherstellung |
| Problembehandlung | MS-Settings: Problembehandlung |
| Windows-Sicherheit | MS-Settings: windowsdefender |
| Windows-Insider-Programm | MS-Settings: windowsinsider (nur vorhanden, wenn der Benutzer in WIP registriert ist)<br/>MS-Settings: windowsinsider-optin |
| Windows-Update | ms-settings:windowsupdate<br>MS-Settings: windowsupdate-Action |
| Optionen für Windows Update-Advanced | MS-Settings: windowsupdate-Optionen |
| Optionen für Windows Update-Restart | MS-Settings: windowsupdate-restartoptions |
| Windows Update-View Update Verlauf | MS-Settings: windowsupdate-History |

### <a name="user-accounts"></a>Benutzerkonten

|Seite "Einstellungen"| URI |
|-------------|-----|
| Bereitstellung | MS-Settings: Workplace-Bereitstellung (nur verfügbar, wenn Enterprise ein Bereitstellungs Paket bereitgestellt hat) |
| Bereitstellung | MS-Settings: Bereitstellung (nur auf mobilen Geräten verfügbar und wenn das Unternehmen ein Bereitstellungs Paket bereitgestellt hat) |
| Windows Anywhere | MS-Settings: windowsanywhere (Gerät muss Windows Anywhere-fähig sein) |
