---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Windows-Geräteportal für Desktop
description: Erfahren Sie, wie das Windows-Geräteportal Einstellungen, Diagnosedaten und Automatisierungsfunktionen auf Ihrem Desktop-PC bereitstellt.
ms.date: 01/08/2021
ms.topic: article
ms.custom: contperf-fy21q3
keywords: Windows 10, UWP, Geräteportal
ms.localizationpriority: medium
ms.openlocfilehash: 11bfc81f045887dfdbba9d380eedd6b9ff40709a
ms.sourcegitcommit: 02d220ef0ec0ecd7ed733086ba164ee9653d9602
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2021
ms.locfileid: "98055983"
---
# <a name="windows-device-portal-for-desktop"></a>Windows-Geräteportal für Desktop

Das Windows-Geräteportal (WDP) ist ein Tool zum Verwalten und Debuggen von Geräten, mit dem Sie Geräteeinstellungen konfigurieren und verwalten sowie Diagnoseinformationen über HTTP in einem Webbrowser anzeigen können. WDP-Details zu anderen Geräten finden Sie unter [Übersicht über das Windows-Geräteportal](device-portal.md).

Sie können das WDP für folgende Aufgaben verwenden:

- Verwalten von Geräteeinstellungen (ähnlich der **Windows-Einstellungen**-App)
- Anzeigen und Bearbeiten einer Liste laufender Prozesse
- Installieren, Löschen, Starten und Beenden von Apps
- Ändern von WLAN-Profilen und Anzeigen der Signalstärke und der ipconfig-Details
- Anzeigen von Live-Diagrammen zur Auslastung von CPU, Arbeitsspeicher, E/A, Netzwerk und GPU
- Sammeln von Prozesssicherungen
- Sammeln von ETW-Ablaufverfolgungen
- Bearbeiten des isolierten Speichers quergeladener Apps

## <a name="set-up-windows-device-portal-on-a-desktop-device"></a>Einrichten des Windows-Geräteportals auf einem Desktop-Gerät

### <a name="turn-on-developer-mode"></a>Aktivieren des Entwicklermodus

Ab Windows 10, Version 1607, sind einige der neueren Funktionen für den Desktop nur verfügbar, wenn der Entwicklermodus aktiviert ist. Informationen zum Aktivieren des Entwicklermodus findest du unter [Aktivieren Ihres Geräts für die Entwicklung](/windows/apps/get-started/enable-your-device-for-development).

> [!IMPORTANT]
> In manchen Fällen wird der Entwicklermodus aufgrund von Problemen mit dem Netzwerk oder der Kompatibilität nicht ordnungsgemäß installiert. Lies den [entsprechenden Abschnitt von „Aktivieren Sie Ihr Gerät für die Entwicklung”](/windows/apps/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package), um Hilfe bei der Behebung dieser Probleme zu erhalten.

### <a name="turn-on-windows-device-portal"></a>Aktivieren des Windows-Geräteportals

Sie können das WDP im Bereich **Für Entwickler** unter **Einstellungen** aktivieren. Wenn du es aktivierst, musst du auch einen entsprechenden Benutzernamen und ein Kennwort erstellen. Verwenden Sie nicht Ihr Microsoft-Konto oder andere Windows-Anmeldeinformationen.

![Abschnitt des Windows-Geräteportals in der Einstellungen-App](images/device-portal/device-portal-desk-settings.png)

Sobald das WDP aktiviert ist, werden unten im Abschnitt Links angezeigt. Beachten Sie die Portnummer am Ende der aufgeführten URLs: Diese Nummer wird zufällig generiert, wenn das WDP aktiviert ist, sollte aber zwischen den Neustarts des Desktops konsistent bleiben.

Diese Links bieten zwei Möglichkeiten, sich mit dem WDP zu verbinden: über das lokale Netzwerk (einschließlich VPN) oder über den lokalen Host. Sobald Sie eine Verbindung hergestellt haben, sollte es ungefähr so aussehen:

![Windows-Geräteportal](images/device-portal/device-portal-example.png)

### <a name="turn-off-windows-device-portal"></a>Deaktivieren des Windows-Geräteportals

Sie können das WDP im Bereich **Für Entwickler** unter **Windows-Einstellungen** deaktivieren.

### <a name="connect-to-windows-device-portal"></a>Herstellen einer Verbindung mit dem Windows-Geräteportal

Um eine Verbindung über einen lokalen Host herzustellen, öffnen Sie ein Browserfenster, und geben eine der hier angezeigte URIs (basierend auf dem von Ihnen verwendeten Verbindungstyp) ein.

- Localhost: `http://127.0.0.1:<PORT>` oder `http://localhost:<PORT>`
- Lokales Netzwerk: `https://<IP address of the desktop>:<PORT>`

Für die Authentifizierung und sichere Kommunikation ist HTTPS erforderlich.

Sie können die Authentifizierungsoption deaktivieren, wenn Sie das WDP in einer geschützten Umgebung verwenden, z. B. in einem Testlabor, in dem Sie allen Benutzern im lokalen Netzwerk vertrauen, keine persönlichen Informationen auf dem Gerät gespeichert haben und eindeutige Anforderungen bestehen. Dies ermöglicht eine unverschlüsselte Kommunikation, und jeder, der die IP-Adresse deines Computers kennt, kann sich verbinden und ihn steuern.

## <a name="windows-device-portal-content"></a>Inhalt des Windows-Geräteportals

WDP stellt die folgenden Seiten bereit.

- App-Manager
- Xbox Live
- Datei-Explorer
- Ausgeführte Prozesse
- Leistung
- Debuggen
- ETW-Protokollierung (Event Tracing for Windows, Ereignisablaufverfolgung für Windows)
- Leistungsüberwachung
- Geräte-Manager
- Bluetooth
- Netzwerk
- Absturzdaten
- Features
- Mixed Reality
- Streaming Install-Debugger
- Position
- Entwurf

## <a name="using-windows-device-portal-to-test-and-debug-msix-apps"></a>Verwenden des Windows-Geräteportals zum Testen und Debuggen von MSIX-Apps


> [!VIDEO https://www.youtube.com/embed/PdgXeOMt4hk]


## <a name="more-windows-device-portal-options"></a>Weitere Optionen des Windows-Geräteportals

### <a name="registry-based-configuration"></a>Registrierungsbasierte Konfiguration

Wenn Sie Portnummern für WDP auswählen möchten (z. B. 80 und 443), können Sie die folgenden Registrierungsschlüssel festlegen:

- Unter `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: Ein erforderlicher DWORD-Wert. Legen Sie den Wert auf 0 fest, um die von Ihnen ausgewählten Portnummern beizubehalten.
    - `HttpPort`: Ein erforderlicher DWORD-Wert. Enthält die Portnummer, die das WDP auf HTTP-Verbindungen überwacht.
    - `HttpsPort`: Ein erforderlicher DWORD-Wert. Enthält die Portnummer, die das WDP auf HTTPS-Verbindungen überwacht.
    
Unter dem gleichen regkey-Pfad kannst du auch die Authentifizierungsanforderung deaktivieren:
- `UseDefaultAuthorizer` - `0` für deaktiviert, `1` für aktiviert.  
    - Dies steuert sowohl die grundlegende Authentifizierungsanforderung für jede Verbindung als auch die Weiterleitung von HTTP nach HTTPS.  
    
### <a name="command-line-options-for-windows-device-portal"></a>Befehlszeilenoptionen für das Windows-Geräteportal

Über eine administrative Eingabeaufforderung können Sie Teile des WDPs aktivieren und konfigurieren. Um den neuesten Satz von Befehlen zu sehen, die von deinem Build unterstützt werden, kannst du `webmanagement /?` ausführen.

- `sc start webmanagement` oder `sc stop webmanagement`
    - Schalte den Dienst ein oder aus. Dazu muss ebenfalls der Entwicklermodus aktiviert sein. 
- `-Credentials <username> <password>` 
    - Legen Sie einen Benutzernamen und ein Kennwort für das WDP fest. Der Benutzername muss den Basic-Auth-Standards entsprechen, darf also keinen Doppelpunkt (:) enthalten und sollte aus Standard-ASCII-Zeichen wie z. B. [a-z, A-Z, 0-9] aufgebaut sein, da Browser standardmäßig nicht den gesamten Zeichensatz verarbeiten.  
- `-DeleteSSL` 
    - Dadurch wird der für HTTPS-Verbindungen verwendete SSL-Zertifikat-Cache zurückgesetzt. Wenn du auf TLS-Verbindungsfehler stößt, die nicht umgangen werden können (im Gegensatz zur erwarteten Zertifikatswarnung), kann diese Option das Problem für dich beheben. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Weitere Informationen finden Sie unter [Bereitstellen eines Windows-Geräteportals mit einem benutzerdefinierten SSL-Zertifikat](./device-portal-ssl.md).  
    - Auf diese Weise können Sie Ihr eigenes SSL-Zertifikat installieren, um die SSL-Warnseite zu reparieren, die normalerweise im WDP angezeigt wird. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Führen Sie eine eigenständige Version des WDPs mit einer bestimmten Konfiguration und sichtbaren Debugmeldungen aus. Dies ist besonders nützlich für die Erstellung eines [gepackten Plugins](./device-portal-plugin.md). 
    - Lies den [Artikel im MSDN Magazine](/archive/msdn-magazine/2017/october/windows-device-portal-write-a-windows-device-portal-packaged-plug-in) mit Details darüber, wie du es als „System” ausführen kannst, um dein gepacktes Plugin vollständig zu testen.

## <a name="troubleshooting"></a>Problembehandlung

Im folgenden finden Sie einige häufige Fehler, die beim Einrichten des Windows-Geräteportals auftreten können.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch gibt eine ungültige Anzahl von Updates zurück (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT).

Dieser Fehler kann auftreten, wenn du versuchst, die Entwicklerpakete in einem Vorabversionsbuild von Windows 10 zu installieren. Diese FOD-Pakete (Feature-on-Demand) werden auf Windows Update gehostet. Wenn du sie in Vorabversionsbuilds herunterlädst, musst du dich für das Flighting entscheiden. Wenn deine Installation nicht für das Flighting für die richtige Build-and-Ring-Kombination aktiviert ist, kann die Nutzlast nicht heruntergeladen werden. Überprüfe Folgendes gründlich:

1. Navigiere zu **Einstellungen > Update und Sicherheit > Windows Insider-Programm**, und vergewissere dich, dass der **Windows-Insider-Konto**-Abschnitt die richtigen Kontoinformationen enthält. Wenn dieser Abschnitt nicht angezeigt wird, wähle **Windows-Insider-Konto verknüpfen** aus, füge dein E-Mail-Konto hinzu und bestätige, dass es in der **Windows-Insider-Konto**-Überschrift angezeigt wird (möglicherweise musst du **Windows-Insider-Konto verknüpfen** ein zweites Mal auswählen, um tatsächlich ein neu hinzugefügtes Konto zu verknüpfen).
 
2. Stelle unter **Welche Inhalte möchten Sie erhalten?** sicher, dass **Aktive Entwicklung von Windows** ausgewählt ist.
 
3. Stelle unter **In welchem Intervall möchten Sie neue Builds erhalten?** sicher, dass **Windows-Insider: schnell** ausgewählt ist.
 
4. Du solltest jetzt FoDs installieren können. Wenn du dich vergewissert hast, dass du mit „Windows-Insider: schnell“ arbeitest und die FoDs weiterhin nicht installieren kannst, gib bitte Feedback, und füge die Protokolldateien unter **C:\Windows\Logs\CBS** an.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService: OpenService FAILED 1060: Der angegebene Dienst ist nicht als installierter Dienst vorhanden.

Dieser Fehler wird möglicherweise angezeigt, wenn die Entwicklerpakete nicht installiert sind. Ohne die Entwicklerpakete gibt es keinen Webverwaltungsdienst. Versuche erneut, die Entwicklerpakete zu installieren.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>CBS kann den Download nicht starten, da sich das System im getakteten Netzwerk (CBS_E_METERED_NETWORK) befindet.

Dieser Fehler wird möglicherweise angezeigt, wenn du über eine getaktete Internetverbindung verfügst. Du kannst die Entwicklerpakete nicht über eine getaktete Verbindung herunterladen.

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über das Windows-Geräteportal](device-portal.md)
* [Referenz zur Kern-API des Windows-Geräteportals](./device-portal-api-core.md)