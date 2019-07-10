---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Geräteportal für Windows-Desktop
description: Hier erfahren Sie, wie das Windows Device Portal Diagnose und Automatisierung auf dem Windows-Desktop öffnet.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, Uwp, Device-portal
ms.localizationpriority: medium
ms.openlocfilehash: 00cf497d5d57f5a3cdc5c52ecfeead7885ff7d56
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713809"
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

* "Localhost": `http://127.0.0.1:<PORT>` oder `http://localhost:<PORT>`
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

- Klicken Sie unter `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: Einen DWORD-Wert erforderlich. Legen Sie den Wert auf 0 fest, um die von Ihnen ausgewählten Portnummern beizubehalten.
    - `HttpPort`: Einen DWORD-Wert erforderlich. Enthält die Portnummer, an der Device Portal nach HTTP-Verbindungen lauscht.    
    - `HttpsPort`: Einen DWORD-Wert erforderlich. Enthält die Portnummer, an der Device Portal nach HTTPS-Verbindungen lauscht.
    
Unter dem gleichen regkey-Pfad können Sie auch die Authentifizierungsanforderung deaktivieren:
- `UseDefaultAuthorizer` - `0` für deaktiviert `1` für aktiviert.  
    - Dies steuert sowohl die grundlegende Authentifizierungsanforderung für jede Verbindung als auch die Weiterleitung von HTTP nach HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Befehlszeilenoptionen für das Geräteportal
Über eine administrative Eingabeaufforderung können Sie Teile des Geräteportal aktivieren und konfigurieren. Um den aktuellen Satz von Befehlen, die auf Ihrem Build unterstützt anzuzeigen, können Sie ausführen `webmanagement /?`

- `sc start webmanagement` oder `sc stop webmanagement` 
    - Schalten Sie den Dienst ein oder aus. Dazu muss ebenfalls der Entwicklermodus aktiviert sein. 
- `-Credentials <username> <password>` 
    - Legen Sie einen Benutzernamen und ein Passwort für das Geräteportal fest. Der Benutzername muss den Basic-Auth-Standards entsprechen, darf also keinen Doppelpunkt (:) enthalten und sollte aus Standard-ASCII-Zeichen wie z. B. [a-z, A-Z, 0-9] aufgebaut sein, da Browser standardmäßig nicht den gesamten Zeichensatz verarbeitet.  
- `-DeleteSSL` 
    - Dadurch wird der für HTTPS-Verbindungen verwendete SSL-Zertifikat-Cache zurückgesetzt. Wenn Sie auf TLS-Verbindungsfehler stoßen, die nicht umgangen werden können (im Gegensatz zur erwarteten Zertifikatswarnung), kann diese Option das Problem für Sie beheben. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Weitere Informationen finden Sie unter [Bereitstellen eines Geräteportals mit einem benutzerdefinierten SSL-Zertifikat](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl).  
    - Auf diese Weise können Sie Ihr eigenes SSL-Zertifikat installieren, um die SSL-Warnseite zu reparieren, die normalerweise im Geräteportal angezeigt wird. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Führen Sie eine eigenständige Version des Geräteportals mit einer bestimmten Konfiguration und sichtbaren Debug-Meldungen aus. Dies ist besonders nützlich für die Erstellung eines [gepackten Plugins](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin). 
    - Lesen Sie den [Artikel im MSDN Magazine](https://msdn.microsoft.com/en-us/magazine/mt826332.aspx) mit Details darüber, wie Sie es als „System” ausführen können, um Ihr gepacktes Plugin vollständig zu testen.

## <a name="common-errors-and-issues"></a>Häufige Fehler und Probleme

Im folgenden sind einige häufige Fehler, die bei der Einrichtung des Device Portal auftreten können.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbseinvalidwindowsupdatecount"></a>WindowsUpdateSearch returns invalid number of updates (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Sie können diese Fehlermeldung beim Versuch, die Entwickler-Pakete auf einer Vorabversion Build von Windows 10 installiert. Diese Features bei Bedarf (Feature-On) Pakete werden über Windows Update gehostet, und sie für Builds von Vorabversionen handelt herunterzuladen erfordert, dass Sie in Test-flighting entscheiden. Wenn die Installation in Test-flighting für die richtige Build- und die Kombination der Ring nicht aktiviert ist, wird die Nutzlast nicht heruntergeladen werden. Überprüfen Sie Folgendes:

1. Navigieren Sie zu **Einstellungen > Update und Sicherheit > Windows-Insider-Programm** und überprüfen Sie, ob die **Windows Insider-Konto** Abschnitt hat die richtigen Kontoinformationen. Wenn Sie diesen Abschnitt nicht sehen, wählen Sie **Verknüpfen eines Windows-Insider-Kontos**Ihr e-Mail-Konto hinzu, und vergewissern Sie sich, dass er unter wird angezeigt der **Windows Insider-Konto** Überschrift (müssen Sie möglicherweise auswählen**Verknüpfen eines Windows-Insider-Kontos** ein zweites Mal tatsächlich Link eine neu hinzugefügte Konto).
 
2. Klicken Sie unter **welchen Inhalt Sie erhalten möchten?** , stellen Sie sicher, dass **aktiven Entwicklung von Windows** ausgewählt ist.
 
3. Klicken Sie unter **welchem Zeitrahmen dies geschehen soll neue Builds erhalten?** , stellen Sie sicher, dass **Windows Insider Fast** ausgewählt ist.
 
4. Sie sollten jetzt die FoDs installieren können. Wenn Sie bestätigt haben, sind auf Windows-Insider Fast und noch nicht die FoDs installiert, bitte Ihr Feedback und fügen Sie die Log-Dateien unter **C:\Windows\Logs\CBS**.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService: OpenService Fehler 1060: Der angegebene Dienst ist nicht als installierter Dienst vorhanden.

Sie erhalten diesen Fehler, die Developer-Pakete nicht installiert werden. Ohne die Developer-Pakete ist es keine Web-Management-Dienst. Versuchen Sie es erneut, die Entwickler-Pakete zu installieren.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbsemeterednetwork"></a>CBS kann nicht Download gestartet werden, da das System in getakteten Netzwerken (CBS_E_METERED_NETWORK) ist.

Sie können diese Fehlermeldung, wenn Sie mit einer getakteten Internetverbindung sind. Sie wird nicht auf die Developer-Pakete auf einer getakteten Verbindung herunterladen können.

## <a name="see-also"></a>Siehe auch

* [Übersicht über die Windows Device Portal](device-portal.md)
* [Device Portal-Core-API-Referenz](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
