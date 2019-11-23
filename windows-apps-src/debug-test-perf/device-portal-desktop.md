---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Geräteportal für Windows-Desktop
description: Hier erfahren Sie, wie das Windows Device Portal Diagnose und Automatisierung auf dem Windows-Desktop öffnet.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP, Geräte Portal
ms.localizationpriority: medium
ms.openlocfilehash: 0f25e882f53bb4f673aa5003495f37d553208721
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282001"
---
# <a name="device-portal-for-windows-desktop"></a>Geräteportal für Windows-Desktop

Im Windows-Geräteportal können Sie Diagnoseinformationen anzeigen und über HTTP aus dem Browserfenster mit Ihrem Desktop interagieren. Sie haben im Device Portal folgende Möglichkeiten:
- Anzeigen und Bearbeiten einer Liste laufender Prozesse
- Installieren, Löschen, Starten und Beenden von Apps
- Ändern von WLAN-Profilen und Anzeigen der Signalstärke und der ipconfig
- Anzeigen von Live-Diagrammen zur Auslastung von CPU, Arbeitsspeicher, E/A, Netzwerk und GPU
- Sammeln von Prozesssicherungen
- Sammeln von ETW-Ablaufverfolgungen 
- Bearbeiten des isolierten Speichers quergeladener Apps

## <a name="set-up-device-portal-on-windows-desktop"></a>Einrichten von des Geräteportals unter Windows-Desktop

### <a name="turn-on-developer-mode"></a>Entwicklermodus aktivieren

Ab Windows 10, Version 1607, sind einige der neueren Funktionen für den Desktop nur verfügbar, wenn der Entwicklermodus aktiviert ist. Informationen zum Aktivieren des Entwicklermodus finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](../get-started/enable-your-device-for-development.md).

> [!IMPORTANT]
> In manchen Fällen wird der Entwicklermodus aufgrund von Problemen mit dem Netzwerk oder der Kompatibilität nicht ordnungsgemäß installiert. Lesen Sie den [entsprechenden Abschnitt von „Aktivieren Sie Ihr Gerät für die Entwicklung”](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package), um Hilfe bei der Behebung dieser Probleme zu erhalten.

### <a name="turn-on-device-portal"></a>Geräteportal einschalten

Sie können das Geräteportal im Bereich **Für Entwickler** unter **Einstellungen** aktivieren. Wenn Sie es aktivieren, müssen Sie auch einen entsprechenden Benutzernamen und ein Kennwort erstellen. Verwenden Sie nicht Ihr Microsoft-Konto oder andere Windows-Anmeldeinformationen. 

![Geräteportal-Abschnitt der Einstellungen-App](images/device-portal/device-portal-desk-settings.png) 

Nachdem Geräteportal aktiviert wurde, werden unten im Abschnitt Links angezeigt. Beachten Sie die Portnummer am Ende der aufgeführten URLs: Diese Nummer wird zufällig generiert, wenn Geräteportal aktiviert ist, sollte aber zwischen den Neustarts des Desktops konsistent bleiben. 

Diese Links bieten zwei Möglichkeiten, sich mit dem Geräteportal zu verbinden: über das lokale Netzwerk (einschließlich VPN) oder über den lokalen Host.

### <a name="connect-to-device-portal"></a>Mit dem Geräteportal verbinden

Um eine Verbindung über einen lokalen Host herzustellen, öffnen Sie ein Browserfenster und geben Sie die hier angezeigte Adresse für den von Ihnen verwendeten Verbindungstyp ein.

* Localhost: `http://127.0.0.1:<PORT>` oder `http://localhost:<PORT>`
* Lokales Netzwerk: `https://<IP address of the desktop>:<PORT>`

Für die Authentifizierung und sichere Kommunikation ist HTTPS erforderlich.

Sie können die Authentifizierung deaktivieren, wenn Sie Geräteportal in einer geschützten Umgebung verwenden, z. B. in einem Testlabor, in dem Sie allen im lokalen Netzwerk vertrauen, keine persönlichen Informationen auf dem Gerät gespeichert haben und in dem spezielle Anforderungen bestehen. Dies ermöglicht eine unverschlüsselte Kommunikation, und jeder, der die IP-Adresse Ihres Computers kennt, kann sich verbinden und es steuern.

## <a name="device-portal-content-on-windows-desktop"></a>Geräteportal-Inhalte auf dem Windows-Desktop

Das Geräteportal auf dem Windows-Desktop bietet die Standardseiten. Ausführliche Beschreibungen finden Sie unter [Übersicht über das Windows-Geräteportal](device-portal.md).

- App-Manager
- Datei-Explorer
- Laufende Prozesse
- Leistung
- Debugging
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
    - `UseDynamicPorts`: ein erforderliches DWORD. Legen Sie den Wert auf 0 fest, um die von Ihnen ausgewählten Portnummern beizubehalten.
    - `HttpPort`: ein erforderliches DWORD. Enthält die Portnummer, an der Geräteportal nach HTTP-Verbindungen lauscht.    
    - `HttpsPort`: ein erforderliches DWORD. Enthält die Portnummer, an der Device Portal nach HTTPS-Verbindungen lauscht.
    
Unter dem gleichen regkey-Pfad können Sie auch die Authentifizierungsanforderung deaktivieren:
- `UseDefaultAuthorizer` - `0` deaktiviert, `1` aktiviert ist.  
    - Dies steuert sowohl die grundlegende Authentifizierungsanforderung für jede Verbindung als auch die Weiterleitung von HTTP nach HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Befehlszeilenoptionen für das Geräteportal
Über eine administrative Eingabeaufforderung können Sie Teile des Geräteportal aktivieren und konfigurieren. Um den aktuellen Satz von Befehlen anzuzeigen, die für Ihren Build unterstützt werden, können Sie ausführen `webmanagement /?`

- `sc start webmanagement` oder `sc stop webmanagement` 
    - Schalten Sie den Dienst ein oder aus. Dazu muss ebenfalls der Entwicklermodus aktiviert sein. 
- `-Credentials <username> <password>` 
    - Legen Sie einen Benutzernamen und ein Passwort für das Geräteportal fest. Der Benutzername muss den Standard Authentifizierungs Standards entsprechen und darf daher keinen Doppelpunkt enthalten (:) und sollten aus standardmäßigen ASCII-Zeichen bestehen, z. b. [a-Za-z0-9], da Browser den vollständigen Zeichensatz nicht auf eine standardmäßige Weise analysieren.  
- `-DeleteSSL` 
    - Dadurch wird der für HTTPS-Verbindungen verwendete SSL-Zertifikat-Cache zurückgesetzt. Wenn Sie auf TLS-Verbindungsfehler stoßen, die nicht umgangen werden können (im Gegensatz zur erwarteten Zertifikatswarnung), kann diese Option das Problem für Sie beheben. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Weitere Informationen finden Sie unter [Bereitstellen eines Geräteportals mit einem benutzerdefinierten SSL-Zertifikat](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl).  
    - Auf diese Weise können Sie Ihr eigenes SSL-Zertifikat installieren, um die SSL-Warnseite zu reparieren, die normalerweise im Geräteportal angezeigt wird. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Führen Sie eine eigenständige Version des Geräteportals mit einer bestimmten Konfiguration und sichtbaren Debug-Meldungen aus. Dies ist besonders nützlich für die Erstellung eines [gepackten Plugins](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin). 
    - Lesen Sie den [Artikel im MSDN Magazine](https://msdn.microsoft.com/en-us/magazine/mt826332.aspx) mit Details darüber, wie Sie es als „System” ausführen können, um Ihr gepacktes Plugin vollständig zu testen.

## <a name="common-errors-and-issues"></a>Häufige Fehler und Probleme

Im folgenden finden Sie einige häufige Fehler, die beim Einrichten des Geräte Portals auftreten können.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>Windowsupdatesearch gibt eine ungültige Anzahl von Updates zurück (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT).

Dieser Fehler kann auftreten, wenn Sie versuchen, die Entwickler Pakete in einem vorab Release von Windows 10 zu installieren. Diese FOD-Pakete (Feature-on-Demand) werden auf Windows Update gehostet. Wenn Sie Sie herunterladen, müssen Sie sich für das flighting entscheiden. Wenn Ihre Installation nicht für die richtige Build-und Ring Kombination aktiviert ist, kann die Nutzlast nicht heruntergeladen werden. Überprüfen Sie Folgendes:

1. Navigieren Sie zu **Einstellungen > aktualisieren Sie & Sicherheit > Windows Insider-Programm** , und vergewissern Sie sich, dass im Abschnitt **Windows Insider-Konto** die richtigen Kontoinformationen angezeigt werden. Wenn dieser Abschnitt nicht angezeigt wird, wählen Sie **Windows Insider-Konto verknüpfen**, Ihr e-Mail-Konto hinzufügen aus, und vergewissern Sie sich, dass es unter der Überschrift **Windows Insider-Konto** angezeigt wird (möglicherweise müssen Sie ein zweites Mal ein **Windows Insider-Konto verknüpfen** , um ein neu hinzugefügtes Konto zu verknüpfen).
 
2. Stellen **Sie unter welche Art von Inhalt möchten Sie erhalten?** sicher, dass die **aktive Windows-Entwicklung** ausgewählt ist.
 
3. Stellen **Sie unter welches Tempo möchten Sie neue Builds erhalten?** sicher, dass **Windows Insider fast** ausgewählt ist.
 
4. Sie sollten jetzt in der Lage sein, die fods zu installieren. Wenn Sie sich vergewissert haben, dass Sie mit Windows Insider fast arbeiten und die fods weiterhin nicht installieren können, geben Sie bitte Feedback, und fügen Sie die Protokolldateien unter " **c:\Windows\Logs\CBS**" an.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>SC StartService: Fehler bei OpenService 1060: der angegebene Dienst ist nicht als installierter Dienst vorhanden.

Dieser Fehler wird möglicherweise angezeigt, wenn die Entwickler Pakete nicht installiert sind. Ohne die Entwickler Pakete gibt es keinen Webverwaltungsdienst. Versuchen Sie erneut, die Entwickler Pakete zu installieren.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>CBS kann den Download nicht starten, da sich das System im getakteten Netzwerk (CBS_E_METERED_NETWORK) befindet.

Dieser Fehler wird möglicherweise angezeigt, wenn Sie über eine getaktete Internetverbindung verfügen. Sie können die Entwickler Pakete nicht über eine getaktete Verbindung herunterladen.

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über das Windows-Geräte Portal](device-portal.md)
* [Referenz zur kernapi des Geräte Portals](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
