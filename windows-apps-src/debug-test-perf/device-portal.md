---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Übersicht über das Windows Device Portal
description: Hier erfahren Sie, wie Sie mit dem Windows Device Portal Ihr Gerät per Remotezugriff über ein Netzwerk oder eine USB-Verbindung konfigurieren und verwalten.
ms.date: 4/9/2019
ms.topic: article
keywords: Windows 10, Uwp, Device-portal
ms.localizationpriority: medium
ms.openlocfilehash: 39715dc3f4b88a2e9a91a7cb659208f8370f16f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362417"
---
# <a name="windows-device-portal-overview"></a>Übersicht über das Windows Device Portal

Mit dem Windows Device Portal können Sie Ihr Gerät remote über ein Netzwerk oder eine USB-Verbindung konfigurieren und verwalten. Darüber hinaus erweiterten Diagnosetools, mit denen Sie die Problembehandlung und die Leistung Ihrer Windows-Gerät in Echtzeit anzeigen.

Windows Device Portal ist ein Webserver auf Ihrem Gerät, das Sie über einen Webbrowser auf einem PC mit verbinden können. Wenn Ihr Gerät einen Webbrowser verfügt, können Sie auch lokal mit dem Browser auf diesem Gerät verbinden.

Windows Device Portal ist für jede Gerätefamilie verfügbar, aber Funktionen und die Einrichtung variieren basierend auf jedes Gerät die Anforderungen an. Dieser Artikel enthält eine allgemeine Beschreibung des Device Portals und Links zu Artikeln mit ausführlicheren Informationen für jede Gerätefamilie.

Die Funktionalität der Windows Device Portal wird mit implementiert [REST-APIs](device-portal-api-core.md) , Sie verwenden können, direkt auf Daten zugreifen und Ihr Gerät programmgesteuert steuern.

## <a name="setup"></a>Setup

Für jedes Gerät gelten spezielle Anweisungen zum Herstellen der Verbindung mit dem Device Portal, diese allgemeinen Schritte sind jedoch für jedes Gerät erforderlich:

1. Aktivieren Sie den Entwicklermodus und Device Portal, auf dem Gerät (in der Einstellungs-app konfiguriert).

2. Verbinden Sie über ein lokales Netzwerk oder mit USB-Gerät und vom PC.

3. Navigieren Sie im Browser zu der Seite für das Geräteportal. Diese Tabelle zeigt die Ports und Protokolle verwendet, die für jede Gerätefamilie.

Gerätefamilie | Standardmäßig aktiviert? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Ja, im Entwicklermodus | 80 (Standard) | 443 (Standard) | http://127.0.0.1:10080
IoT | Ja, im Entwicklermodus | 8080 | Über Registrierungsschlüssel aktivieren | Nicht zutreffend
Xbox | Im Entwicklermodus aktivieren | Disabled | 11443 | Nicht zutreffend
Desktop| Im Entwicklermodus aktivieren | 50080\* | 50043\* | Nicht zutreffend
Phone | Im Entwicklermodus aktivieren | 80| 443 | http://127.0.0.1:10080

\* Dies ist nicht immer der Fall, wie der Device-Portal auf Desktop-Ports im kurzlebigen Bereich Ansprüche (> 50.000) um Konflikte mit vorhandenen Port Ansprüche auf dem Gerät zu verhindern. Weitere Informationen hierzu finden Sie im Abschnitt zu [Porteinstellungen](device-portal-desktop.md#registry-based-configuration-for-device-portal) für den Desktop.  

Gerätespezifische Anweisungen zum Einrichten finden Sie in folgenden Artikeln:

- [Geräteportal für HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Für IoT-Geräteportal](https://go.microsoft.com/fwlink/?LinkID=616499)
- [Device-Portal für mobile Geräte](device-portal-mobile.md)
- [Geräteportal für Xbox](../xbox-apps/device-portal-xbox.md)
- [Für Desktop-Geräteportal](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Features

### <a name="toolbar-and-navigation"></a>Symbolleiste und Navigation

Auf die Symbolleiste am oberen Rand der Seite ermöglicht den Zugriff auf häufig verwendete Funktionen.

- **Power**: Access-Power-Optionen.
  - **Herunterfahren**: Das Gerät wird deaktiviert.
  - **Starten Sie neu**: Zyklen Einschalten des Geräts.
- **Hilfe**: Öffnet die Seite "Hilfe" an.

Verwenden Sie die Links im Navigationsbereich am linken Rand der Seite, um zu den verfügbaren Verwaltungs- und Überwachungstools für Ihr Gerät zu navigieren.

Tools, die auf allen gerätefamilien gelten, werden hier beschrieben. Je nach Gerät sind möglicherweise andere Optionen verfügbar. Weitere Informationen finden Sie in der spezifischen Seite für Ihren Gerätetyp.

### <a name="apps-manager"></a>App-Manager

Der Apps-Manager ermöglicht das Installieren/Deinstallieren und Verwaltungsfunktionen für app-Pakete und bündelt auf dem Hostgerät.

![Seite "Device Portal-Apps-Manager"](images/device-portal/WDP_AppsManager2.png)

* **Bereitstellen von apps**: Bereitstellen von App-Pakete von einer lokalen, Netzwerk- oder Web-Hosts, und registrieren Sie Lose Dateien aus Netzwerkfreigaben.
* **Installierte apps**: Verwenden Sie im Dropdown-Menü zu entfernen oder apps starten, die auf dem Gerät installiert werden.
* **Ausführen von apps**: Abrufen von Informationen zu den apps, die derzeit ausgeführt, und schließen Sie diese nach Bedarf.

#### <a name="install-sideload-an-app"></a>Installieren Sie (querladen) einer app

Sie können apps Sideloaden, während der Entwicklung mit Windows Device Portal:

1. Wenn Sie ein app-Paket erstellt haben, können Sie es Remote auf Ihrem Gerät installieren. Nachdem Sie es in Visual Studio erstellt haben, wird ein Ausgabeordner generiert.

    ![App-Installation](images/device-portal/iot-installapp0.png)

2. Windows Device Portal, navigieren Sie zu der **Apps Manager** Seite.

3. In der **Bereitstellen von apps** wählen Sie im Abschnitt **lokalen Speicher**.

4. Klicken Sie unter **wählen Sie das Anwendungspaket**Option **Choose File** und navigieren Sie zu der app-Paket, die Sie querladen möchten.

5. Unter **wählen Zertifikatdatei (. cer), die zum Signieren der app-Paket**Option **Choose File** und navigieren Sie zu dieser app-Paket zugeordnete Zertifikat.

6. Überprüfen Sie die entsprechenden Felder aus, ob zum Installieren optionaler oder Framework-Pakete zusammen mit der app-Installation werden soll, und wählen Sie **Weiter** um sie auszuwählen.

7. Wählen Sie **installieren** um die Installation initiieren.

8. Wenn das Gerät Windows 10 im S-Modus ausgeführt wird, und es ist beim ersten, die das angegebene Zertifikat auf dem Gerät installiert wurde, das Gerät neu gestartet.

#### <a name="install-a-certificate"></a>Installieren eines Zertifikats

Alternativ können Sie installieren das Zertifikat über die Windows Device Portal, und installieren Sie die app auf andere Weise:

1. Windows Device Portal, navigieren Sie zu der **Apps Manager** Seite.

2. In der **Bereitstellen von apps** wählen Sie im Abschnitt **Zertifikat installieren**.

3. Unter **wählen Zertifikatdatei (. cer), die zum Signieren eines app-Pakets**Option **Choose File** , und navigieren Sie zu dem Zertifikat verknüpft ist, mit dem app-Paket, die Sie querladen möchten.

4. Wählen Sie **installieren** um die Installation initiieren.

5. Wenn das Gerät Windows 10 im S-Modus ausgeführt wird, und es ist beim ersten, die das angegebene Zertifikat auf dem Gerät installiert wurde, das Gerät neu gestartet.

#### <a name="uninstall-an-app"></a>Deinstallieren einer App

1. Stellen Sie sicher, dass die App nicht ausgeführt wird.
2. Wenn es sich handelt, wechseln Sie zu **Ausführen von apps** und schließen Sie sie. Wenn Sie versuchen, die deinstalliert werden, während die app ausgeführt wird, wird dies zu Problemen führen, wenn Sie versuchen, die app erneut installieren.
3. Wählen Sie die app in der Dropdownliste auf **entfernen**.

### <a name="running-processes"></a>Laufende Prozesse

Diese Seite enthält Informationen zu Prozessen, die derzeit auf dem Hostgerät ausgeführt. Diese umfassen Apps und Systemprozesse. Auf einigen Plattformen (Desktop, IoT und HoloLens) können Sie Prozesse beenden.

![Gerät-Portal ausgeführt verarbeitet Seite](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Datei-Explorer

Auf dieser Seite können Sie zum Anzeigen und Bearbeiten von alle mittels sideload übertragenen apps gespeicherte Dateien. Finden Sie unter den [mit dem App-Datei-Explorer](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) Blogbeitrag, um weitere Informationen zu den Datei-Explorer und dessen Verwendung.

![Device Portal-Datei-Explorer-Seite](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Leistung

Die Seite "Leistung" zeigt in Echtzeit Diagramme mit System Diagnoseinformationen wie Energieverbrauch, Framerate, und CPU-Last.

Die folgenden Metriken sind verfügbar:

- **CPU**: Prozent der insgesamt verfügbaren CPU-Auslastung
- **Arbeitsspeicher**: Insgesamt verwendet werden, verfügbar ist, ein Commit ausgeführt wurde, per Pager benachrichtigt, und nicht ausgelagerten
- **I/O**: Lesen und Schreiben von Mengen von Daten
- **Netzwerk**: Erhaltene und gesendete Daten
- **GPU**: Prozent des insgesamt verfügbaren GPU-Engine-Auslastung

![Leistung der Portal-Seite "Geräte"](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Ereignisprotokollierung für die Ereignisablaufverfolgung für Windows (ETW)

Die ETW-protokollierungsseite verwaltet Event Tracing for Windows (ETW)-Informationen in Echtzeit auf dem Gerät.

![Portal-ETW-Protokollierung-Seite "Geräte"](images/device-portal/mob-device-portal-etw.png)

Aktivieren Sie **Anbieter ausblenden**, um nur die Liste der Ereignisse anzuzeigen.

- **Die registrierten Anbieter**: Wählen Sie den Ereignisanbieter und die Ablaufverfolgungsebene. Die Ablaufverfolgungsebene ist eine der folgenden Werte:
  1. Abnormal exit or termination
  2. Severe errors
  3. Warnungen
  4. Non-error warnings
  5. Ausführliche Ablaufverfolgung

  Klicken oder tippen Sie auf **Aktivieren**, um die Ablaufverfolgung zu starten. Der Anbieter wird der Liste **Aktivierte Anbieter** hinzugefügt.
- **Benutzerdefinierte Anbieter**: Wählen Sie eine benutzerdefinierte ETW-Anbieter und der Ablaufverfolgungsebene. Identifizieren Sie den Anbieter anhand seiner GUID. Die GUID darf nicht Klammern enthalten.
- **Aktivierte Anbieter**: Gibt die aktivierten Anbietern ist. Wählen Sie einen Anbieter aus der Dropdownliste aus, und klicken oder tippen Sie auf **Deaktivieren**, um die Ablaufverfolgung zu beenden. Klicken oder tippen Sie auf **Beenden**, um sämtliche Ablaufverfolgung anzuhalten.
- **Anbieter-Verlauf**: Dadurch wird die ETW-Anbieter, die während der aktuellen Sitzung aktiviert wurden. Klicken oder tippen Sie auf **Aktivieren**, um einen Anbieter zu aktivieren, der deaktiviert war. Klicken oder tippen Sie auf **Löschen**, um den Verlauf zu löschen.
- **Filter / Ereignisse**: Die **Ereignisse** Abschnitt werden die ETW-Ereignisse von den ausgewählten Anbieter im Tabellenformat aufgelistet. Die Tabelle wird in Echtzeit aktualisiert. Verwenden der **Filter** Menü zum Einrichten benutzerdefinierter Filter für die Ereignisse angezeigt werden. Klicken Sie auf die **löschen** Schaltfläche, um alle ETW-Ereignisse aus der Tabelle zu löschen. Hierdurch werden keine Anbieter deaktiviert. Klicken Sie auf **in Datei speichern** , die derzeit gesammelten ETW-Ereignisse in eine lokale CSV-Datei zu exportieren.

Weitere Informationen zur Verwendung von ETW-Protokollierung finden Sie unter den [Device-Portal verwenden, um Debugprotokolle anzuzeigen](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) Blogbeitrag.

### <a name="performance-tracing"></a>Leistungsüberwachung

Die Leistung nachrichtenablaufverfolgungs-Seite können Sie für die Ansicht der [Windows Performance Recorder (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) ablaufverfolgungen aus dem Hostgerät.

![Device Portal Leistung nachrichtenablaufverfolgungs-Seite](images/device-portal/mob-device-portal-perf-tracing.png)

- **Verfügbare Profile**: Wählen Sie aus der Dropdown-Liste und klicken oder tippen Sie auf die WPR Profil **starten** zu Ablaufverfolgung starten.
- **Benutzerdefinierte Profile**: Klicken oder tippen Sie auf **Durchsuchen** auswählen ein Profils WPR von Ihrem PC. Klicken oder tippen Sie auf **Hochladen und starten**, um die Ablaufverfolgung zu starten.

Klicken Sie auf **Beenden**, um die Ablaufverfolgung zu beenden. Auf dieser Seite zu bleiben, bis die Datei (. ETL) vollständig heruntergeladen wurde.

Erfasst. ETL-Dateien geöffnet werden können, für die Analyse in die [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Geräte-Manager

Seite mit dem Geräte-Manager Listet alle Peripheriegeräte, die auf Ihrem Gerät angefügt. Sie können die Symbole "Einstellungen" zum Anzeigen der Eigenschaften der einzelnen klicken.

![Geräteseite Portal-Geräte-manager](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Netzwerk

Die Seite "Netzwerke" verwaltet Netzwerkverbindungen enthaltenen arbeitsplatzverbindungen auf dem Gerät. Es sei denn, Sie Device Portal über USB verbunden sind, werden Änderungen an diesen Einstellungen wahrscheinlich Sie Device Portal trennen.

- **Verfügbare Netzwerke**: Zeigt die WLAN-Netzwerken auf dem Gerät verfügbar. Durch Klicken oder Tippen auf ein Netzwerk können Sie eine Verbindung mit ihm herstellen und ggf. ein Kennwort eingeben. Device-Portal unterstützt noch keine Authentifizierung von Unternehmen. Sie können auch die **Profile** Dropdownliste, um zu versuchen, sich mit einem WLAN-Profile, die bekannt, dass das Gerät verbinden.
- **IP-Konfiguration**: Zeigt Informationen zu den einzelnen ApplicationWorkingSetLimit des Netzwerk-Ports zu beheben.

![Seite "Portal Netzwerk Geräte"](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Dienstfunktionen und die Anmerkungen zu dieser Version

### <a name="dns-sd"></a>DNS-SD

Das Geräteportal kündigt seine Präsenz im lokalen Netzwerk mithilfe von DNS-SD an. Alle Geräteportalinstanzen, unabhängig von deren Gerätetyp, kündigen sich unter „WDP._wdp._tcp.local“ an. Die TXT-Datensätze für die Instanz des Dienstes liefern Folgendes:

Key | Typ | Beschreibung
----|------|-------------
S | ssNoversion | Sicherer Port für Geräteportal. Wenn 0 (null), lauscht das Geräteportal nicht auf HTTPS-Verbindungen.
D | String | Typ des Geräts. Dieser wird das Format „Windows.*“ aufweisen, z. B. Windows.Xbox oder Windows.Desktop
A | String | Gerätearchitektur. Diese ist ARM, x86 oder AMD64.  
T | Mit NULL-Zeichen getrennt Liste mit Zeichenfolgen | Vom Benutzer angewendete Tags für das Gerät. Informationen zur Verwendung finden Sie unter der Tags-REST-API. Liste wird durch Doppelnull beendet.  

Es wird vorgeschlagen, die Verbindung über den HTTPS-Anschluss herzustellen, da nicht alle Geräte auf dem vom DNS-SD-Datensatz angekündigten HTTP-Port lauschen.

### <a name="csrf-protection-and-scripting"></a>CSRF-Schutz und -Skripting

Zum Schutz vor [CSRF-Angriffen](https://wikipedia.org/wiki/Cross-site_request_forgery) ist bei allen Nicht-GET-Anfragen ein eindeutiges Token erforderlich. Dieses Token, der X-CSFR-Token-Anforderungsheader, wird von einem Sitzungscookie CSRF-Token, abgeleitet. In der Web-Benutzeroberfläche des Geräteportals wird das CSRF-Token-Cookie bei jeder Anforderung in den X-CSRF-Token-Header kopiert.

> [!IMPORTANT]
> Diesen Schutz verhindert die Verwendung der REST-APIs von einem eigenständigen Client (z. B. Befehlszeilen-Hilfsprogramme). Dies kann auf drei Arten gelöst werden:
> - Verwenden Sie einen Benutzernamen "Auto-". Clients, die ihrem Benutzernamen „Auto-“ voranstellen, umgehen CSRF-Schutz. Es ist wichtig, dass dieser Benutzername nicht zur Anmeldung beim Geräteportal über den Browser verwendet wird, da dies den Dienst für CSRF-Angriffe öffnet. Beispiel: Wenn Device Portal Benutzernamen "Admin", ist ```curl -u auto-admin:password <args>``` umgehen von CSRF-Schutz verwendet werden soll.
> - Implementieren des Cookie-zu-Header-Schemas in den Client. Dies erfordert eine GET-Anforderung zur Erstellung des Sitzungscookies und dann die Aufnahme von Header und Cookie in alle nachfolgenden Anforderungen.
> - Deaktivieren der Authentifizierung und Verwenden von HTTP. CSRF-Schutz bezieht sich nur auf HTTPS-Endpunkte, sodass für Verbindungen auf HTTP-Endpunkten keine der oben genannten Schritte ausgeführt werden müssen.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Schutz vor websiteübergreifendem WebSocket-Hijacking (Cross-Site WebSocket Hijacking, CSWSH)

Zum Schutz vor [CSWSH-Angriffen](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html) müssen alle Clients, die eine WebSocket-Verbindung mit einem Geräteportal herstellen, einen dem Hostheader entsprechenden Origin-Header angeben. Dadurch wird gegenüber dem Geräteportal belegt, dass die Anforderung entweder von der Benutzeroberfläche des Geräteportals oder von einer gültigen Clientanwendung stammt. Anforderungen ohne Origin-Header werden abgelehnt.
