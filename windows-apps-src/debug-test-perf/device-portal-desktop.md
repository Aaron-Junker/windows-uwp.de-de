---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Geräteportal für Windows-Desktop
description: Hier erfahren Sie, wie das Windows Device Portal Diagnose und Automatisierung auf dem Windows-Desktop öffnet.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP, Geräteportal
ms.localizationpriority: medium
ms.openlocfilehash: 73f7e827c0ec8ca289d3523da06601de978a91d2
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "79210186"
---
# <a name="device-portal-for-windows-desktop"></a>Geräteportal für Windows-Desktop

Im Windows-Geräteportal kannst du Diagnoseinformationen anzeigen und über HTTP aus dem Browserfenster mit deinem Desktop interagieren. Sie haben im Device Portal folgende Möglichkeiten:
- Anzeigen und Bearbeiten einer Liste laufender Prozesse
- Installieren, Löschen, Starten und Beenden von Apps
- Ändern von WLAN-Profilen und Anzeigen der Signalstärke und der ipconfig
- Anzeigen von Live-Diagrammen zur Auslastung von CPU, Arbeitsspeicher, E/A, Netzwerk und GPU
- Sammeln von Prozesssicherungen
- Sammeln von ETW-Ablaufverfolgungen 
- Bearbeiten des isolierten Speichers quergeladener Apps

## <a name="set-up-device-portal-on-windows-desktop"></a>Einrichten des Geräteportals unter Windows-Desktop

### <a name="turn-on-developer-mode"></a>Aktivieren des Entwicklermodus

Ab Windows 10, Version 1607, sind einige der neueren Funktionen für den Desktop nur verfügbar, wenn der Entwicklermodus aktiviert ist. Informationen zum Aktivieren des Entwicklermodus findest du unter [Aktivieren Ihres Geräts für die Entwicklung](../get-started/enable-your-device-for-development.md).

> [!IMPORTANT]
> In manchen Fällen wird der Entwicklermodus aufgrund von Problemen mit dem Netzwerk oder der Kompatibilität nicht ordnungsgemäß installiert. Lies den [entsprechenden Abschnitt von „Aktivieren Sie Ihr Gerät für die Entwicklung”](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package), um Hilfe bei der Behebung dieser Probleme zu erhalten.

### <a name="turn-on-device-portal"></a>Aktivieren des Geräteportals

Du kannst das Geräteportal im Bereich **Für Entwickler** unter **Einstellungen** aktivieren. Wenn du es aktivierst, musst du auch einen entsprechenden Benutzernamen und ein Kennwort erstellen. Verwenden Sie nicht Ihr Microsoft-Konto oder andere Windows-Anmeldeinformationen. 

![Geräteportalabschnitt der Einstellungen-App](images/device-portal/device-portal-desk-settings.png) 

Sobald das Geräteportal aktiviert ist, werden unten im Abschnitt Links angezeigt. Beachte die Portnummer am Ende der aufgeführten URLs: Diese Nummer wird zufällig generiert, wenn das Geräteportal aktiviert ist, sollte aber zwischen den Neustarts des Desktops konsistent bleiben. 

Diese Links bieten zwei Möglichkeiten, sich mit dem Geräteportal zu verbinden: über das lokale Netzwerk (einschließlich VPN) oder über den lokalen Host.

### <a name="connect-to-device-portal"></a>Verbinden mit dem Geräteportal

Um eine Verbindung über einen lokalen Host herzustellen, öffne ein Browserfenster und gib die hier angezeigte Adresse für den von dir verwendeten Verbindungstyp ein.

* Localhost: `http://127.0.0.1:<PORT>` oder `http://localhost:<PORT>`
* Lokales Netzwerk: `https://<IP address of the desktop>:<PORT>`

Für die Authentifizierung und sichere Kommunikation ist HTTPS erforderlich.

Du kannst die Authentifizierung deaktivieren, wenn du das Geräteportal in einer geschützten Umgebung verwendest, z. B. in einem Testlabor, in dem du allen im lokalen Netzwerk vertraust, keine persönlichen Informationen auf dem Gerät gespeichert hast und in dem spezielle Anforderungen bestehen. Dies ermöglicht eine unverschlüsselte Kommunikation, und jeder, der die IP-Adresse deines Computers kennt, kann sich verbinden und ihn steuern.

## <a name="device-portal-content-on-windows-desktop"></a>Geräteportal-Inhalte auf dem Windows-Desktop

Das Geräteportal auf dem Windows-Desktop bietet die Standardseiten. Ausführliche Beschreibungen findest du unter [Übersicht über das Windows-Geräteportal](device-portal.md).

- App-Manager
- Datei-Explorer
- Laufende Prozesse
- Leistung
- Debugprotokolle
- Ereignisablaufverfolgung für Windows (ETW)
- Leistungsüberwachung
- Geräte-Manager
- Netzwerk
- Absturzdaten
- Features
- Mixed Reality
- Streaming Install-Debugger
- Speicherort
- Entwurf

## <a name="more-device-portal-options"></a>Weitere Optionen für das Geräteportal

### <a name="registry-based-configuration-for-device-portal"></a>Registrierungsbasierte Konfiguration für das Geräteportal

Wenn Sie Portnummern für Device Portal auswählen möchten (z. B. 80 und 443), können Sie die folgenden Registrierungsschlüssel festlegen:

- Unter `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: Ein erforderlicher DWORD-Wert. Legen Sie den Wert auf 0 fest, um die von Ihnen ausgewählten Portnummern beizubehalten.
    - `HttpPort`: Ein erforderlicher DWORD-Wert. Enthält die Portnummer, an der Device Portal nach HTTP-Verbindungen lauscht.    
    - `HttpsPort`: Ein erforderlicher DWORD-Wert. Enthält die Portnummer, an der Device Portal nach HTTPS-Verbindungen lauscht.
    
Unter dem gleichen regkey-Pfad kannst du auch die Authentifizierungsanforderung deaktivieren:
- `UseDefaultAuthorizer` - `0` für deaktiviert, `1` für aktiviert.  
    - Dies steuert sowohl die grundlegende Authentifizierungsanforderung für jede Verbindung als auch die Weiterleitung von HTTP nach HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Befehlszeilenoptionen für das Geräteportal
Über eine administrative Eingabeaufforderung kannst du Teile des Geräteportal aktivieren und konfigurieren. Um den neuesten Satz von Befehlen zu sehen, die von deinem Build unterstützt werden, kannst du `webmanagement /?` ausführen.

- `sc start webmanagement` oder `sc stop webmanagement` 
    - Schalte den Dienst ein oder aus. Dazu muss ebenfalls der Entwicklermodus aktiviert sein. 
- `-Credentials <username> <password>` 
    - Lege einen Benutzernamen und ein Kennwort für das Geräteportal fest. Der Benutzername muss den Basic-Auth-Standards entsprechen, darf also keinen Doppelpunkt (:) enthalten und sollte aus Standard-ASCII-Zeichen wie z. B. [a-z, A-Z, 0-9] aufgebaut sein, da Browser standardmäßig nicht den gesamten Zeichensatz verarbeiten.  
- `-DeleteSSL` 
    - Dadurch wird der für HTTPS-Verbindungen verwendete SSL-Zertifikat-Cache zurückgesetzt. Wenn du auf TLS-Verbindungsfehler stößt, die nicht umgangen werden können (im Gegensatz zur erwarteten Zertifikatswarnung), kann diese Option das Problem für dich beheben. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Weitere Informationen findest du unter [Bereitstellen eines Geräteportals mit einem benutzerdefinierten SSL-Zertifikat](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl).  
    - Auf diese Weise kannst du dein eigenes SSL-Zertifikat installieren, um die SSL-Warnseite zu reparieren, die normalerweise im Geräteportal angezeigt wird. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Führe eine eigenständige Version des Geräteportals mit einer bestimmten Konfiguration und sichtbaren Debugmeldungen aus. Dies ist besonders nützlich für die Erstellung eines [gepackten Plugins](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin). 
    - Lies den [Artikel im MSDN Magazine](https://msdn.microsoft.com/magazine/mt826332.aspx) mit Details darüber, wie du es als „System” ausführen kannst, um dein gepacktes Plugin vollständig zu testen.

## <a name="common-errors-and-issues"></a>Häufige Fehler und Probleme

Im folgenden findest du einige häufige Fehler, die beim Einrichten des Geräteportals auftreten können.

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

## <a name="see-also"></a>Siehe auch

* [Übersicht über das Windows-Geräteportal](device-portal.md)
* [Referenz zur Kern-API des Geräteportals](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
