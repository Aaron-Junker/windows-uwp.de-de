---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Übersicht über das Windows-Geräteportal
description: Hier erfahren Sie, wie Sie mit dem Windows Device Portal Ihr Gerät per Remotezugriff über ein Netzwerk oder eine USB-Verbindung konfigurieren und verwalten.
ms.date: 04/09/2019
ms.topic: article
keywords: Windows 10, UWP, Geräte Portal
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254761"
---
# <a name="windows-device-portal-overview"></a>Übersicht über das Windows-Geräteportal

Mit dem Windows Device Portal können Sie Ihr Gerät per Remotezugriff über ein Netzwerk oder eine USB-Verbindung konfigurieren und verwalten. Außerdem werden erweiterte Diagnosetools bereitstellt, die Ihnen bei der Problembehandlung und Anzeige der Echtzeitleistung Ihres Windows-Geräts helfen.

Das Windows-Geräte Portal ist ein Webserver auf Ihrem Gerät, mit dem Sie über einen Webbrowser auf einem PC eine Verbindung herstellen können. Wenn Ihr Gerät über einen Webbrowser verfügt, können Sie auch lokal eine Verbindung mit dem Browser auf diesem Gerät herstellen.

Das Windows-Geräte Portal ist für jede Gerätefamilie verfügbar, aber Features und Setup sind abhängig von den Anforderungen der einzelnen Geräte. Dieser Artikel enthält eine allgemeine Beschreibung des Device Portals und Links zu Artikeln mit ausführlicheren Informationen für jede Gerätefamilie.

Die Funktionalität des Windows-Geräte Portals wird mit [Rest-APIs](device-portal-api-core.md) implementiert, die Sie direkt verwenden können, um auf Daten zuzugreifen und Ihr Gerät Programm gesteuert zu steuern.

## <a name="setup"></a>Setup

Für jedes Gerät gelten spezielle Anweisungen zum Herstellen der Verbindung mit dem Device Portal, diese allgemeinen Schritte sind jedoch für jedes Gerät erforderlich:

1. Aktivieren Sie den Entwicklermodus und das Geräte Portal auf Ihrem Gerät (konfiguriert in der App "Einstellungen").

2. Verbinden Sie Ihr Gerät und Ihren PC über ein lokales Netzwerk oder mit USB.

3. Navigieren Sie im Browser zu der Seite für das Geräteportal. In dieser Tabelle werden die Ports und Protokolle angezeigt, die von den einzelnen Gerätefamilien verwendet werden.

Gerätefamilie | Standardmäßig aktiviert? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Ja, im Entwicklermodus | 80 (Standard) | 443 (Standard) | http://127.0.0.1:10080
IoT | Ja, im Entwicklermodus | 8080 | Über Registrierungsschlüssel aktivieren | n. v.
Xbox | Im Entwicklermodus aktivieren | Deaktiviert | 11443 | n. v.
Desktop| Im Entwicklermodus aktivieren | 50080\* | 50043\* | n. v.
Telefone | Im Entwicklermodus aktivieren | 80| 443 | http://127.0.0.1:10080

\* dies nicht immer der Fall ist, da das Geräte Portal auf den Desktops Ports im kurzlebigen Bereich (> 50000) anfordert, um Konflikte mit vorhandenen Port Ansprüchen auf dem Gerät zu verhindern. Weitere Informationen hierzu finden Sie im Abschnitt zu [Porteinstellungen](device-portal-desktop.md#registry-based-configuration-for-device-portal) für den Desktop.  

Gerätespezifische Anweisungen zum Einrichten finden Sie in folgenden Artikeln:

- [Geräteportal für HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Geräteportal für IoT](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [Geräteportal für Mobilgeräte](device-portal-mobile.md)
- [Geräteportal für Xbox](../xbox-apps/device-portal-xbox.md)
- [Geräteportal für Windows-Desktop](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Features

### <a name="toolbar-and-navigation"></a>Symbolleiste und Navigation

Die Symbolleiste oben auf der Seite bietet Zugriff auf häufig verwendete Funktionen.

- **Power**: greifen Sie auf Energieoptionen zu.
  - **Herunterfahren**: Schaltet das Gerät aus.
  - **Neu starten**: Schaltet das Gerät aus und wieder ein.
- **Hilfe**: Öffnet die Hilfeseite.

Verwenden Sie die Links im Navigationsbereich am linken Rand der Seite, um zu den verfügbaren Verwaltungs- und Überwachungstools für Ihr Gerät zu navigieren.

Hier werden Tools beschrieben, die über Gerätefamilien hinweg gemeinsam sind. Je nach Gerät sind möglicherweise andere Optionen verfügbar. Weitere Informationen finden Sie auf der jeweiligen Seite für Ihren Gerätetyp.

### <a name="apps-manager"></a>App-Manager

Der apps-Manager bietet Funktionen für die Installation, Deinstallation und Verwaltung von App-Paketen und Paketen auf dem Hostgerät.

![Geräte Portal-apps-Manager-Seite](images/device-portal/WDP_AppsManager2.png)

* Bereitstellen von **apps**: Bereitstellen von App-Paketen aus lokalen, Netzwerk-oder Webhosts und Registrieren von lockeren Dateien von Netzwerkfreigaben.
* **Installierte apps**: Verwenden Sie das Dropdown Menü, um apps, die auf dem Gerät installiert sind, zu entfernen oder zu starten.
* **Ausführen von apps**: erhalten Sie Informationen zu den apps, die zurzeit ausgeführt werden, und schließen Sie Sie bei Bedarf.

#### <a name="install-sideload-an-app"></a>Installieren (Sideload) einer APP

Sie können apps während der Entwicklung mithilfe des Windows-Geräte Portals querladen:

1. Wenn Sie ein App-Paket erstellt haben, können Sie es Remote auf Ihrem Gerät installieren. Nachdem Sie es in Visual Studio erstellt haben, wird ein Ausgabeordner generiert.

    ![App-Installation](images/device-portal/iot-installapp0.png)

2. Navigieren Sie im Windows-Geräte Portal zur Seite **App-Manager** .

3. Wählen Sie im Abschnitt Bereitstellen von **apps** die Option **lokaler Speicher**aus.

4. Wählen Sie unter **Anwendungspaket auswählen**die Option **Datei auswählen** aus, und navigieren Sie zu dem App-Paket, das Sie Sideload durchlaufen möchten.

5. Wählen Sie unter **Zertifikat Datei (. cer) zum Signieren des App-Pakets auswählen**die Option **Datei** auswählen aus, und navigieren Sie zu dem Zertifikat, das dem App-Paket zugeordnet ist.

6. Aktivieren Sie die entsprechenden Kontrollkästchen, wenn Sie optionale-oder Framework-Pakete zusammen mit der App-Installation installieren möchten, und wählen Sie **weiter** aus, um Sie auszuwählen.

7. Wählen Sie **Installieren** aus, um die Installation zu initiieren.

8. Wenn auf dem Gerät Windows 10 im S-Modus ausgeführt wird und das angegebene Zertifikat zum ersten Mal auf dem Gerät installiert ist, starten Sie das Gerät neu.

#### <a name="install-a-certificate"></a>Installieren eines Zertifikats

Alternativ können Sie das Zertifikat über das Windows-Geräte Portal installieren und die APP auf andere Weise installieren:

1. Navigieren Sie im Windows-Geräte Portal zur Seite **App-Manager** .

2. Wählen Sie im Abschnitt Bereitstellen von **apps** die Option **Zertifikat installieren**aus.

3. Wählen Sie unter **Zertifikat Datei (. cer), die zum Signieren eines App-Pakets verwendet**wird die Option **Datei** auswählen aus, und navigieren Sie zu dem Zertifikat, das dem App-Paket zugeordnet ist, das Sie querladen möchten.

4. Wählen Sie **Installieren** aus, um die Installation zu initiieren.

5. Wenn auf dem Gerät Windows 10 im S-Modus ausgeführt wird und das angegebene Zertifikat zum ersten Mal auf dem Gerät installiert ist, starten Sie das Gerät neu.

#### <a name="uninstall-an-app"></a>Deinstallieren einer App

1. Stellen Sie sicher, dass die App nicht ausgeführt wird.
2. Wenn dies der Fall ist, navigieren Sie zu **laufende Apps** , und schließen Sie Sie. Wenn Sie versuchen, die APP zu deinstallieren, während die app ausgeführt wird, führt dies zu Problemen, wenn Sie versuchen, die APP erneut zu installieren.
3. Wählen Sie in der Dropdown Liste die APP aus, und klicken Sie auf **Entfernen**

### <a name="running-processes"></a>Ausführen von Prozessen

Auf dieser Seite werden Details zu Prozessen angezeigt, die zurzeit auf dem Hostgerät ausgeführt werden. Diese umfassen Apps und Systemprozesse. Auf einigen Plattformen (Desktop, IOT und hololens) können Sie Prozesse beenden.

![Seite "Prozesse" des Geräte Portals](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Datei-Explorer

Auf dieser Seite können Sie Dateien anzeigen und bearbeiten, die von beliebigen Sideload-apps gespeichert werden. Weitere Informationen zum Datei-Explorer und deren Verwendung finden Sie im Blogbeitrag [Verwenden des App-Datei-Explorers](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) .

![Datei-Explorer-Seite des Geräte Portals](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Leistung

Die Seite Leistung zeigt Echtzeitdiagramme der Diagnoseinformationen des Systems, z. b. Energieverbrauch, Framerate und CPU-Auslastung.

Die folgenden Metriken sind verfügbar:

- **CPU**: Prozentualer Anteil der insgesamt verfügbaren CPU-Auslastung
- Arbeits **Speicher**: Gesamt, Verwendung, verfügbar, committet, Auslagerungsseiten und nicht-Auslagerungsseiten
- E **/** a: Datenmengen für Lese-und Schreibvorgänge
- **Netzwerk**: empfangene und gesendete Daten
- **GPU**: Prozentsatz der gesamten verfügbaren GPU-Engine-Auslastung

![Seite "Geräte Portal Leistung"](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Protokollierung der Ereignis Ablauf Verfolgung für Windows (ETW)

Auf der etw-Protokollierungs Seite werden Informationen zur Ereignis Ablauf Verfolgung für Windows (Event Tracing for Windows, etw) auf dem Gerät verwaltet.

![Seite mit ETW-Protokollierung des Geräte Portals](images/device-portal/mob-device-portal-etw.png)

Aktivieren Sie **Anbieter ausblenden**, um nur die Liste der Ereignisse anzuzeigen.

- **Registrierte Anbieter**: Wählen Sie den Ereignis Anbieter und die Ablauf Verfolgungs Ebene aus. Die Ablauf Verfolgungs Ebene ist einer der folgenden Werte:
  1. Abnormal exit or termination
  2. Severe errors
  3. Warnungen
  4. Non-error warnings
  5. Ausführliche Ablauf Verfolgung

  Klicken oder tippen Sie auf **Aktivieren**, um die Ablaufverfolgung zu starten. Der Anbieter wird der Liste **Aktivierte Anbieter** hinzugefügt.
- **Benutzerdefinierte Anbieter**: Wählen Sie einen benutzerdefinierten ETW-Anbieter und die Ablaufverfolgungsebene aus. Identifizieren Sie den Anbieter anhand seiner GUID. Fügen Sie keine Klammern in die GUID ein.
- **Aktivierte Anbieter**: Hiermit werden die aktivierten Anbieter aufgelistet. Wählen Sie einen Anbieter aus der Dropdownliste aus, und klicken oder tippen Sie auf **Deaktivieren**, um die Ablaufverfolgung zu beenden. Klicken oder tippen Sie auf **Beenden**, um sämtliche Ablaufverfolgung anzuhalten.
- **Anbieter Verlauf**: Hiermit werden die ETW-Anbieter angezeigt, die während der aktuellen Sitzung aktiviert wurden. Klicken oder tippen Sie auf **Aktivieren**, um einen Anbieter zu aktivieren, der deaktiviert war. Klicken oder tippen Sie auf **Löschen**, um den Verlauf zu löschen.
- **Filter/Ereignisse**: im Abschnitt **Ereignisse** sind ETW-Ereignisse der ausgewählten Anbieter im Tabellenformat aufgelistet. Die Tabelle wird in Echtzeit aktualisiert. Mit dem Menü **Filter** können Sie benutzerdefinierte Filter einrichten, für die Ereignisse angezeigt werden. Klicken Sie **auf die Schaltfläche löschen,** um alle ETW-Ereignisse aus der Tabelle zu löschen. Hierdurch werden keine Anbieter deaktiviert. Klicken Sie auf **in Datei speichern** , um die aktuell gesammelten ETW-Ereignisse in eine lokale CSV-Datei zu exportieren.

Weitere Informationen zur Verwendung der ETW-Protokollierung finden Sie im Blogbeitrag [Verwenden des Geräte Portals zum Anzeigen von Debug-Protokollen](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) .

### <a name="performance-tracing"></a>Leistungsüberwachung

Auf der Seite "Leistungs Ablauf Verfolgung" können Sie die [Windows Performance Recorder (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) -Ablauf Verfolgungen vom Hostgerät anzeigen.

![Ablauf Verfolgungs Seite des Geräte Portals](images/device-portal/mob-device-portal-perf-tracing.png)

- **Verfügbare Profile**: Wählen Sie in der Dropdownliste das WPR-Profil aus, und klicken oder tippen Sie auf **Starten**, um die Ablaufverfolgung zu starten.
- **Benutzerdefinierte Profile**: Klicken oder tippen Sie auf **Durchsuchen**, um ein WPR-Profil vom PC auszuwählen. Klicken oder tippen Sie auf **Hochladen und starten**, um die Ablaufverfolgung zu starten.

Klicken Sie auf **Beenden**, um die Ablaufverfolgung zu beenden. Bleiben Sie auf dieser Seite bis zur Ablauf Verfolgungs Datei (. ETL) hat den Download abgeschlossen.

Gefasst. ETL-Dateien können für die Analyse in der [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10))geöffnet werden.

### <a name="device-manager"></a>Geräte-Manager

Auf der Seite Geräte-Manager werden alle Peripheriegeräte aufgelistet, die an Ihr Gerät angeschlossen sind. Sie können auf die Einstellungs Symbole klicken, um die Eigenschaften der einzelnen anzuzeigen.

![Geräte-Manager-Seite des Geräte Portals](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Networking

Die Netzwerkseite verwaltet Netzwerkverbindungen auf dem Gerät. Wenn Sie nicht über USB mit dem Geräte Portal verbunden sind, werden Sie durch das Ändern dieser Einstellungen wahrscheinlich vom Geräte Portal getrennt.

- **Verfügbare Netzwerke**: zeigt die für das Gerät verfügbaren WLAN-Netzwerke an. Durch Klicken oder Tippen auf ein Netzwerk können Sie eine Verbindung mit ihm herstellen und ggf. ein Kennwort eingeben. Das Geräte Portal unterstützt die Enterprise-Authentifizierung noch nicht. Sie können auch die Dropdown Liste **profile** verwenden, um zu versuchen, eine Verbindung mit einem der WiFi-profile herzustellen, die dem Gerät bekannt sind.
- **IP-Konfiguration**: zeigt Adressinformationen zu den einzelnen Netzwerkports des Host Geräts an.

![Geräte Portal-Netzwerkseite](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Dienst Features und Notizen

### <a name="dns-sd"></a>DNS-SD

Das Geräteportal kündigt seine Präsenz im lokalen Netzwerk mithilfe von DNS-SD an. Alle Geräteportalinstanzen, unabhängig von deren Gerätetyp, kündigen sich unter „WDP._wdp._tcp.local“ an. Die TXT-Datensätze für die Instanz des Dienstes liefern Folgendes:

Schlüssel | Typ | Beschreibung
----|------|-------------
S | int | Sicherer Port für Geräteportal. Wenn 0 (null), lauscht das Geräteportal nicht auf HTTPS-Verbindungen.
D | String | Typ des Geräts. Dies hat das Format "Windows. *", z. b. Windows. Xbox oder Windows. Desktop.
A | String | Gerätearchitektur. Diese ist ARM, x86 oder AMD64.  
T | Mit NULL-Zeichen getrennt Liste mit Zeichenfolgen | Vom Benutzer angewendete Tags für das Gerät. Informationen zur Verwendung finden Sie unter der Tags-REST-API. Liste wird durch Doppelnull beendet.  

Es wird vorgeschlagen, die Verbindung über den HTTPS-Anschluss herzustellen, da nicht alle Geräte auf dem vom DNS-SD-Datensatz angekündigten HTTP-Port lauschen.

### <a name="csrf-protection-and-scripting"></a>CSRF-Schutz und -Skripting

Zum Schutz vor [CSRF-Angriffen](https://en.wikipedia.org/wiki/Cross-site_request_forgery) ist bei allen Nicht-GET-Anfragen ein eindeutiges Token erforderlich. Dieses Token, der X-CSFR-Token-Anforderungsheader, wird von einem Sitzungscookie CSRF-Token, abgeleitet. In der Web-Benutzeroberfläche des Geräteportals wird das CSRF-Token-Cookie bei jeder Anforderung in den X-CSRF-Token-Header kopiert.

> [!IMPORTANT]
> Dieser Schutz verhindert die Verwendung der Rest-APIs von einem eigenständigen Client (z. b. Befehlszeilen-Hilfsprogramme). Dies kann auf drei Arten gelöst werden:
> - Verwenden Sie den Benutzernamen "Auto". Clients, die ihrem Benutzernamen „Auto-“ voranstellen, umgehen CSRF-Schutz. Es ist wichtig, dass dieser Benutzername nicht zur Anmeldung beim Geräteportal über den Browser verwendet wird, da dies den Dienst für CSRF-Angriffe öffnet. Beispiel: Wenn der Benutzername des Geräteportals „Admin“ lautet, sollte ```curl -u auto-admin:password <args>``` zum Umgehen des CSRF Schutzes verwendet werden.
> - Implementieren des Cookie-zu-Header-Schemas in den Client. Dies erfordert eine GET-Anforderung zur Erstellung des Sitzungscookies und dann die Aufnahme von Header und Cookie in alle nachfolgenden Anforderungen.
> - Deaktivieren der Authentifizierung und Verwenden von HTTP. CSRF-Schutz bezieht sich nur auf HTTPS-Endpunkte, sodass für Verbindungen auf HTTP-Endpunkten keine der oben genannten Schritte ausgeführt werden müssen.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Schutz vor websiteübergreifendem WebSocket-Hijacking (Cross-Site WebSocket Hijacking, CSWSH)

Zum Schutz vor [CSWSH-Angriffen](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html) müssen alle Clients, die eine WebSocket-Verbindung mit einem Geräteportal herstellen, einen dem Hostheader entsprechenden Origin-Header angeben. Dadurch wird gegenüber dem Geräteportal belegt, dass die Anforderung entweder von der Benutzeroberfläche des Geräteportals oder von einer gültigen Clientanwendung stammt. Anforderungen ohne Origin-Header werden abgelehnt.
