---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Übersicht über das Windows-Geräteportal
description: Hier erfahren Sie, wie Sie mit dem Windows Device Portal Ihr Gerät per Remotezugriff über ein Netzwerk oder eine USB-Verbindung konfigurieren und verwalten.
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254761"
---
# <a name="windows-device-portal-overview"></a>Übersicht über das Windows-Geräteportal

Mit dem Windows Device Portal können Sie Ihr Gerät per Remotezugriff über ein Netzwerk oder eine USB-Verbindung konfigurieren und verwalten. It also provides advanced diagnostic tools to help you troubleshoot and view the real-time performance of your Windows device.

Windows Device Portal is a web server on your device that you can connect to from a web browser on a PC. If your device has a web browser, you can also connect locally with the browser on that device.

Windows Device Portal is available on each device family, but features and setup vary based on each device's requirements. Dieser Artikel enthält eine allgemeine Beschreibung des Device Portals und Links zu Artikeln mit ausführlicheren Informationen für jede Gerätefamilie.

The functionality of the Windows Device Portal is implemented with [REST APIs](device-portal-api-core.md) that you can use directly to access data and control your device programmatically.

## <a name="setup"></a>Setup

Für jedes Gerät gelten spezielle Anweisungen zum Herstellen der Verbindung mit dem Device Portal, diese allgemeinen Schritte sind jedoch für jedes Gerät erforderlich:

1. Enable Developer Mode and Device Portal on your device (configured in the Settings app).

2. Connect your device and PC through a local network or with USB.

3. Navigieren Sie im Browser zu der Seite für das Geräteportal. This table shows the ports and protocols used by each device family.

Gerätefamilie | Standardmäßig aktiviert? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Ja, im Entwicklermodus | 80 (Standard) | 443 (Standard) | http://127.0.0.1:10080
IoT | Ja, im Entwicklermodus | 8080 | Über Registrierungsschlüssel aktivieren | n. v.
Xbox-Taste | Im Entwicklermodus aktivieren | Deaktiviert | 11443 | n. v.
Desktop| Im Entwicklermodus aktivieren | 50080\* | 50043\* | n. v.
Telefone | Im Entwicklermodus aktivieren | 80| 443 | http://127.0.0.1:10080

\* This is not always the case, as Device Portal on desktop claims ports in the ephemeral range (>50,000) to prevent collisions with existing port claims on the device. Weitere Informationen hierzu finden Sie im Abschnitt zu [Porteinstellungen](device-portal-desktop.md#registry-based-configuration-for-device-portal) für den Desktop.  

Gerätespezifische Anweisungen zum Einrichten finden Sie in folgenden Artikeln:

- [Geräteportal für HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Geräteportal für IoT](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [Geräteportal für Mobilgeräte](device-portal-mobile.md)
- [Geräteportal für Xbox](../xbox-apps/device-portal-xbox.md)
- [Geräteportal für Windows-Desktop](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Features

### <a name="toolbar-and-navigation"></a>Symbolleiste und Navigation

The toolbar at the top of the page provides access to commonly used features.

- **Power**: Access power options.
  - **Herunterfahren**: Schaltet das Gerät aus.
  - **Neu starten**: Schaltet das Gerät aus und wieder ein.
- **Hilfe**: Öffnet die Hilfeseite.

Verwenden Sie die Links im Navigationsbereich am linken Rand der Seite, um zu den verfügbaren Verwaltungs- und Überwachungstools für Ihr Gerät zu navigieren.

Tools that are common across device families are described here. Je nach Gerät sind möglicherweise andere Optionen verfügbar. For more info, see the specific page for your device type.

### <a name="apps-manager"></a>App-Manager

The Apps manager provides install/uninstall and management functionality for app packages and bundles on the host device.

![Device Portal Apps manager page](images/device-portal/WDP_AppsManager2.png)

* **Deploy apps**: Deploy packaged apps from local, network, or web hosts and register loose files from network shares.
* **Installed apps**: Use the dropdown menu to remove or start apps that are installed on the device.
* **Running apps**: Get information about the apps that are currently running and close them as necessary.

#### <a name="install-sideload-an-app"></a>Install (sideload) an app

You can sideload apps during development using Windows Device Portal:

1. When you've created an app package, you can remotely install it onto your device. Nachdem Sie es in Visual Studio erstellt haben, wird ein Ausgabeordner generiert.

    ![App-Installation](images/device-portal/iot-installapp0.png)

2. In Windows Device Portal, navigate to the **Apps manager** page.

3. In the **Deploy apps** section, select **Local Storage**.

4. Under **Select the application package**, select **Choose File** and browse to the app package that you want to sideload.

5. Under **Select certificate file (.cer) used to sign app package**, select **Choose File** and browse to the certificate associated with that app package.

6. Check the respective boxes if you want to install optional or framework packages along with the app installation, and select **Next** to choose them.

7. Select **Install** to initiate the installation.

8. If the device is running Windows 10 in S mode, and it is the first time that the given certificate has been installed on the device, restart the device.

#### <a name="install-a-certificate"></a>Install a certificate

Alternatively, you can install the certificate via Windows Device Portal, and install the app through other means:

1. In Windows Device Portal, navigate to the **Apps manager** page.

2. In the **Deploy apps** section, select **Install Certificate**.

3. Under **Select certificate file (.cer) used to sign an app package**, select **Choose File** and browse to the certificate associated with the app package that you want to sideload.

4. Select **Install** to initiate the installation.

5. If the device is running Windows 10 in S mode, and it is the first time that the given certificate has been installed on the device, restart the device.

#### <a name="uninstall-an-app"></a>Deinstallieren einer App

1. Stellen Sie sicher, dass die App nicht ausgeführt wird.
2. If it is, go to **Running apps** and close it. If you attempt to uninstall while the app is running, it will cause issues when you attempt to reinstall the app.
3. Select the app from the dropdown and click **Remove**.

### <a name="running-processes"></a>Running processes

This page shows details about processes currently running on the host device. Diese umfassen Apps und Systemprozesse. On some platforms (Desktop, IoT, and HoloLens), you can terminate processes.

![Device Portal Running processes page](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Datei-Explorer

This page allows you to view and manipulate files stored by any sideloaded apps. See the [Using the App File Explorer](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) blog post to learn more about the File explorer and how to use it.

![Device Portal File explorer page](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Leistung

The Performance page shows real-time graphs of system diagnostic info like power usage, frame rate, and CPU load.

Die folgenden Metriken sind verfügbar:

- **CPU**: Percent of total available CPU utilization
- **Memory**: Total, in use, available, committed, paged, and non-paged
- **I/O**: Read and write data quantities
- **Network**: Received and sent data
- **GPU**: Percent of total available GPU engine utilization

![Device Portal Performance page](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Event Tracing for Windows (ETW) logging

The ETW logging page manages real-time Event Tracing for Windows (ETW) information on the device.

![Device Portal ETW logging page](images/device-portal/mob-device-portal-etw.png)

Aktivieren Sie **Anbieter ausblenden**, um nur die Liste der Ereignisse anzuzeigen.

- **Registered providers**: Select the event provider and the tracing level. The tracing level is one of these values:
  1. Abnormal exit or termination
  2. Severe errors
  3. Warnungen
  4. Non-error warnings
  5. Detailed trace

  Klicken oder tippen Sie auf **Aktivieren**, um die Ablaufverfolgung zu starten. Der Anbieter wird der Liste **Aktivierte Anbieter** hinzugefügt.
- **Benutzerdefinierte Anbieter**: Wählen Sie einen benutzerdefinierten ETW-Anbieter und die Ablaufverfolgungsebene aus. Identifizieren Sie den Anbieter anhand seiner GUID. Do not include brackets in the GUID.
- **Enabled providers**: This lists the enabled providers. Wählen Sie einen Anbieter aus der Dropdownliste aus, und klicken oder tippen Sie auf **Deaktivieren**, um die Ablaufverfolgung zu beenden. Klicken oder tippen Sie auf **Beenden**, um sämtliche Ablaufverfolgung anzuhalten.
- **Providers history**: This shows the ETW providers that were enabled during the current session. Klicken oder tippen Sie auf **Aktivieren**, um einen Anbieter zu aktivieren, der deaktiviert war. Klicken oder tippen Sie auf **Löschen**, um den Verlauf zu löschen.
- **Filters / Events**: The **Events** section lists ETW events from the selected providers in table format. The table is updated in real time. Use the **Filters** menu to set up custom filters for which events will be displayed. Click the **Clear** button to delete all ETW events from the table. Hierdurch werden keine Anbieter deaktiviert. You can click **Save to file** to export the currently collected ETW events to a local CSV file.

For more details on using ETW logging, see the [Use Device Portal to view debug logs](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) blog post.

### <a name="performance-tracing"></a>Leistungsüberwachung

The Performance tracing page allows you for view the [Windows Performance Recorder (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) traces from the host device.

![Device Portal performance tracing page](images/device-portal/mob-device-portal-perf-tracing.png)

- **Verfügbare Profile**: Wählen Sie in der Dropdownliste das WPR-Profil aus, und klicken oder tippen Sie auf **Starten**, um die Ablaufverfolgung zu starten.
- **Benutzerdefinierte Profile**: Klicken oder tippen Sie auf **Durchsuchen**, um ein WPR-Profil vom PC auszuwählen. Klicken oder tippen Sie auf **Hochladen und starten**, um die Ablaufverfolgung zu starten.

Klicken Sie auf **Beenden**, um die Ablaufverfolgung zu beenden. Stay on this page until the trace file (.ETL) has finished downloading.

Captured .ETL files can be opened for analysis in the [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Geräte-Manager

The Device manager page enumerates all peripherals attached to your device. You can click the settings icons to view the properties of each.

![Device Portal Device manager page](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>-Netzwerk

The Networking page manages network connections on the device. Unless you are connected to Device Portal through USB, changing these settings will likely disconnect you from Device Portal.

- **Available networks**: Shows the WiFi networks available to the device. Durch Klicken oder Tippen auf ein Netzwerk können Sie eine Verbindung mit ihm herstellen und ggf. ein Kennwort eingeben. Device Portal does not yet support Enterprise Authentication. You can also use the **Profiles** dropdown to attempt to connect to any of the WiFi profiles known to the device.
- **IP configuration**: Shows address information about each of the host device's network ports.

![Device Portal Networking page](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Service features and notes

### <a name="dns-sd"></a>DNS-SD

Das Geräteportal kündigt seine Präsenz im lokalen Netzwerk mithilfe von DNS-SD an. Alle Geräteportalinstanzen, unabhängig von deren Gerätetyp, kündigen sich unter „WDP._wdp._tcp.local“ an. Die TXT-Datensätze für die Instanz des Dienstes liefern Folgendes:

Schlüssel | Geben Sie in das Suchfeld auf der Taskleiste | Beschreibung
----|------|-------------
E | int | Sicherer Port für Geräteportal. Wenn 0 (null), lauscht das Geräteportal nicht auf HTTPS-Verbindungen.
D | String | Typ des Geräts. This will be in the format "Windows.*", for example, Windows.Xbox or Windows.Desktop
Eine | String | Gerätearchitektur. Diese ist ARM, x86 oder AMD64.  
T | Mit NULL-Zeichen getrennt Liste mit Zeichenfolgen | Vom Benutzer angewendete Tags für das Gerät. Informationen zur Verwendung finden Sie unter der Tags-REST-API. Liste wird durch Doppelnull beendet.  

Es wird vorgeschlagen, die Verbindung über den HTTPS-Anschluss herzustellen, da nicht alle Geräte auf dem vom DNS-SD-Datensatz angekündigten HTTP-Port lauschen.

### <a name="csrf-protection-and-scripting"></a>CSRF-Schutz und -Skripting

Zum Schutz vor [CSRF-Angriffen](https://en.wikipedia.org/wiki/Cross-site_request_forgery) ist bei allen Nicht-GET-Anfragen ein eindeutiges Token erforderlich. Dieses Token, der X-CSFR-Token-Anforderungsheader, wird von einem Sitzungscookie CSRF-Token, abgeleitet. In der Web-Benutzeroberfläche des Geräteportals wird das CSRF-Token-Cookie bei jeder Anforderung in den X-CSRF-Token-Header kopiert.

> [!IMPORTANT]
> This protection prevents usages of the REST APIs from a standalone client (such as command-line utilities). Dies kann auf drei Arten gelöst werden:
> - Use an "auto-" username. Clients, die ihrem Benutzernamen „Auto-“ voranstellen, umgehen CSRF-Schutz. Es ist wichtig, dass dieser Benutzername nicht zur Anmeldung beim Geräteportal über den Browser verwendet wird, da dies den Dienst für CSRF-Angriffe öffnet. Beispiel: Wenn der Benutzername des Geräteportals „Admin“ lautet, sollte ```curl -u auto-admin:password <args>``` zum Umgehen des CSRF Schutzes verwendet werden.
> - Implementieren des Cookie-zu-Header-Schemas in den Client. Dies erfordert eine GET-Anforderung zur Erstellung des Sitzungscookies und dann die Aufnahme von Header und Cookie in alle nachfolgenden Anforderungen.
> - Deaktivieren der Authentifizierung und Verwenden von HTTP. CSRF-Schutz bezieht sich nur auf HTTPS-Endpunkte, sodass für Verbindungen auf HTTP-Endpunkten keine der oben genannten Schritte ausgeführt werden müssen.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Schutz vor websiteübergreifendem WebSocket-Hijacking (Cross-Site WebSocket Hijacking, CSWSH)

Zum Schutz vor [CSWSH-Angriffen](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html) müssen alle Clients, die eine WebSocket-Verbindung mit einem Geräteportal herstellen, einen dem Hostheader entsprechenden Origin-Header angeben. Dadurch wird gegenüber dem Geräteportal belegt, dass die Anforderung entweder von der Benutzeroberfläche des Geräteportals oder von einer gültigen Clientanwendung stammt. Anforderungen ohne Origin-Header werden abgelehnt.
