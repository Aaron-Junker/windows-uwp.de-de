---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Geräteportal für Xbox
description: Erfahren Sie, wie Sie das Xbox-Geräte Portal für Xbox One aktivieren, mit dem Sie Remote Zugriff auf Ihre Entwicklungs-Xbox erhalten.
ms.date: 04/09/2019
ms.topic: article
keywords: Windows 10, UWP, Geräteportal
ms.localizationpriority: medium
ms.openlocfilehash: a7df29b6c1446d65c8e5224eede3030a25888364
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933171"
---
# <a name="device-portal-for-xbox"></a>Geräteportal für Xbox

## <a name="set-up-device-portal-on-xbox"></a>Einrichten des Geräteportals für Xbox

Die folgenden Schritte zeigen, wie Sie das Xbox-Geräte Portal aktivieren, mit dem Sie Remote Zugriff auf Ihre Entwicklungs-Xbox erhalten.

1. Öffnen Sie Dev Home. Diese Einstellung sollte standardmäßig geöffnet werden, wenn Sie Ihre Entwicklungs-Xbox starten, aber Sie können Sie auch über den Startbildschirm öffnen.

    ![„Dev Home“ im Geräteportal](images/device-portal-xbox-1.png)

2. Wählen Sie in dev Home auf der Registerkarte **Startseite** unter **Remote Zugriff**die Option **Remote Zugriffs Einstellungen**aus.

    ![Tool „Remoteverwaltung“ für das Geräteportal](images/device-portal-xbox-15.png)

3. Aktivieren Sie die Einstellung **Xbox-Geräte Portal aktivieren** .

4. Wählen Sie unter **Authentifizierung**die Option **Benutzername und Kennwort festlegen**aus. Geben Sie einen **Benutzernamen** und ein **Kennwort** ein, die zum Authentifizieren des Zugriffs auf Ihr dev-Kit über einen Browser verwendet werden, und **Speichern** Sie Sie.

5. **Schließen** Sie die Seite **Remote Zugriff** , und notieren Sie sich die URL unter **Remote Zugriff** auf der Registerkarte **Start** .

6. Geben Sie die URL in Ihrem Browser ein, und melden Sie sich mit den Anmeldeinformationen an, die Sie konfiguriert haben.

7. Sie erhalten eine Warnung zu dem bereitgestellten Zertifikat, ähnlich wie unten dargestellt. Klicken Sie in Edge auf **Details** , und **fahren Sie dann mit der Webseite fort** , um auf das Xbox-Geräte Portal zuzugreifen. Geben Sie im angezeigten Dialogfeld den Benutzernamen und das Kennwort ein, die Sie zuvor auf der Xbox eingegeben haben.

    ![Fehler beim Zertifikat für das Device Portal](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Seiten von Device Portal

Das Xbox-Geräte Portal bietet eine Reihe von Standardseiten, die mit den verfügbaren Optionen im Windows-Geräte Portal vergleichbar sind, sowie mehrere eindeutige Seiten. Ausführliche Beschreibungen der ersten finden Sie unter [Übersicht über das Windows-Geräte Portal](../debug-test-perf/device-portal.md). In den folgenden Abschnitten werden die Seiten beschrieben, die für das Xbox-Geräte Portal eindeutig sind.

### <a name="home"></a>-Startseite

Ähnlich wie bei der **App-Manager** -Seite des Windows-Geräte Portals wird auf der **Start** Seite des Xbox-Geräte Portals eine Liste der installierten Spiele und apps unter **Meine Spiele & apps**angezeigt. Sie können auf den Namen eines Spiels oder einer APP klicken, um weitere Details anzuzeigen (z. b. den **Paket Familiennamen**). In der **Dropdown** -Dropdown-Datei können Sie Aktionen für das Spiel oder die APP durchführen, z. b. **starten** .

Unter **Xbox Live-Testkonten**können Sie die Konten verwalten, die mit Ihrer Xbox verknüpft sind. Sie können Benutzer und Gastkonten hinzufügen, neue Benutzer erstellen, Benutzer ein-und abmelden und Konten entfernen.

![-Startseite](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (Spiel speichert)

Sowohl das Windows-Geräte Portal als auch das Xbox-Geräte Portal verfügen über eine **Xbox Live** -Seite. Das Xbox-Geräte Portal verfügt jedoch über einen eindeutigen Abschnitt, das **Xbox Live-Spiel speichert**, wo Sie Daten für Spiele speichern können, die auf der Xbox installiert sind. Geben Sie die **Dienstkonfigurations-ID (SCID)** ein (Weitere Informationen finden Sie in der [Xbox Live Service-Konfiguration](/gaming/xbox-live/xbox-live-service-configuration.md#get-your-ids) ). **Mitgliedsname (MSA)** und **paketfamilienname (PFN)** , die dem Titel und dem Spiel speichern zugeordnet sind, suchen Sie nach der **Eingabedatei (. JSON oder. Xml)**, und wählen Sie dann eine der Schaltflächen (**Zurücksetzen**, **importieren**, **exportieren**und **Löschen**) aus, um die Daten zu speichern.

Im Abschnitt **generieren** können Sie Dummydaten generieren und in der angegebenen Eingabedatei speichern. Geben Sie einfach die **Container (standardmäßig 2)**, die **BLOBs (Standardwert 3)** und die **blobgröße (standardmäßig 1024**) ein, und wählen Sie **generieren**aus.

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>HTTP-Monitor

Der HTTP-Monitor ermöglicht es Ihnen, entschlüsselten http-und HTTPS-Datenverkehr von Ihrer APP oder Ihrem Spiel aus anzuzeigen, wenn Sie auf Ihrer Xbox One ausgeführt wird.

![HTTP-Monitor](images/device-portal-xbox-18.png)

Um es zu aktivieren, öffnen Sie dev Home auf Ihrer Xbox One, navigieren Sie zur Registerkarte **Einstellungen** , und aktivieren Sie im Feld **http-Monitoreinstellungen** die Option **http-Monitor aktivieren**.

![Dev Home: Networking](images/device-portal-xbox-14.png)

Nach der Aktivierung können Sie im Xbox-Geräte Portal den Datenverkehr für http-und HTTPS-Dateien **Beenden**, **Löschen**und **Speichern** , indem Sie die entsprechenden Schaltflächen auswählen.

### <a name="network-fiddler-tracing"></a>Netzwerk (Fiddler-Ablauf Verfolgung)

Die **Netzwerk** Seite im Xbox-Geräte Portal ist nahezu identisch mit der **Netzwerkseite im** Windows-Geräte Portal, mit Ausnahme der **Fiddler**-Ablauf Verfolgung, die für das Xbox-Geräte Portal eindeutig ist. Auf diese Weise können Sie den HTTP-und HTTPS-Datenverkehr zwischen Ihrer Xbox One und dem Internet auf Ihrem PC ausführen. Weitere Informationen finden [Sie unter Verwenden von "fddler mit Xbox One" bei der Entwicklung für UWP](../xbox-apps/uwp-fiddler.md) .

![Netzwerk](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>Medienerfassung

Auf der Seite **Medien Erfassung** können Sie **Screenshot erfassen** auswählen, um einen Screenshot der Xbox One zu erstellen. Nachdem Sie dies getan haben, werden Sie von Ihrem Browser aufgefordert, die Datei herunterzuladen. Sie können " **HDR bevorzugen** " aktivieren, wenn Sie den Screenshot in HDR verwenden möchten (wenn die Konsole ihn unterstützt).

![Medienerfassung](images/device-portal-xbox-12.png)

### <a name="settings"></a>Einstellungen

Auf der Seite **Einstellungen** können Sie mehrere Einstellungen für die Xbox One anzeigen und bearbeiten. Im oberen Bereich können Sie **importieren** auswählen, um die Einstellungen aus einer Datei zu importieren und zu **exportieren** , um die aktuellen Einstellungen in eine txt-Datei zu exportieren. Durch das Importieren von Einstellungen können Sie die Massenbearbeitung vereinfachen, insbesondere bei der Konfiguration mehrerer Konsolen. Um eine Einstellungsdatei für den Import zu erstellen, ändern Sie die Einstellungen wie gewünscht, und exportieren Sie dann die Einstellungen. Anschließend können Sie diese Datei verwenden, um Einstellungen schnell und einfach für andere Konsolen zu importieren.

Es gibt mehrere Abschnitte mit unterschiedlichen Einstellungen zum Anzeigen und/oder bearbeiten, die unten erläutert werden.

![Screenshot der Seite "Einstellungen", auf der die Abschnitte Geräteinformationen und Anzeigeeinstellungen angezeigt werden.](images/device-portal-xbox-20.png)

![Screenshot der Seite "Einstellungen" mit den Abschnitten "Lokalisierungs Einstellungen", "Energie Einstellungen" und "Benutzereinstellungen". ](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>Geräteinformationen

* **Gerätename**: der Name des Geräts. Um den Namen zu bearbeiten, ändern Sie den Namen im Feld, und wählen Sie **Speichern**aus.

* **Betriebssystemversion**: schreibgeschützt. Die Versionsnummer des Betriebssystems.

* **Betriebssystem Edition**: schreibgeschützt. Der Name der Hauptversion des Betriebssystems.

* **Xbox Live-Geräte-ID**: schreibgeschützt.

* **Konsolen-ID**: schreibgeschützt.

* **Seriennummer**: schreibgeschützt.

* **Console Type**: schreibgeschützt. Der Typ des Xbox One-Geräts (Xbox One, Xbox One S oder Xbox One X).

* **Dev-Modus**: schreibgeschützt. Der Entwicklermodus, in dem sich das Gerät befindet.

#### <a name="audio-settings"></a>Audioeinstellungen

* **Audiobitstream-Format**: das Format der Audiodaten.

* **HDMI-Audiodatei**: der Audiotyp über den HDMI-Port.

* **Headset-Format**: das Audioformat, das über Kopfhörer angezeigt wird.

* **Optische Audiodaten**: der Audiotyp über den optischen Port.

* **Verwenden Sie das-Codec-oder optische audioheadset**: Aktivieren Sie dieses Kontrollkästchen, wenn Sie ein mit HDMI oder optisch verbundenes Headset verwenden.

#### <a name="display-settings"></a>Anzeigeeinstellungen

* **Farbtiefe**: die Anzahl der Bits, die für jede Farbkomponente eines einzelnen Pixels verwendet werden.

* **Farbraum**: die Farbpalette, die für die Anzeige verfügbar ist.

* **Bildschirmauflösung**: die Auflösung der Anzeige.

* **Verbindung anzeigen**: der Typ der Verbindung mit der Anzeige.

* **High Dynamic Range (HDR) zulassen**: aktiviert HDR auf der Anzeige. Nur für kompatible anzeigen verfügbar.

* **4K zulassen**: aktiviert 4K-Auflösung auf der Anzeige. Nur für kompatible anzeigen verfügbar.

* **Variable Aktualisierungs Rate zulassen (VRR)**: Aktivieren Sie VRR für die Anzeige. Nur für kompatible anzeigen verfügbar.

#### <a name="kinect-settings"></a>Kinect-Einstellungen

Ein kinect-Sensor muss mit der Konsole verbunden sein, um diese Einstellungen zu ändern.

* **Kinect aktivieren**: Aktivieren Sie den angeschlossenen kinect-Sensor.

* " **Kinect neu laden" bei App-Änderung erzwingen**: lädt den angefügten kinect-Sensor neu, wenn eine andere APP oder ein anderes Spiel ausgeführt wird

#### <a name="localization-settings"></a>Lokalisierungs Einstellungen

* **Geografische Region**: die geografische Region, auf die das Gerät festgelegt ist. Muss der spezielle Ländercode mit zwei Zeichen sein (z. b. " **US** " für USA).

* **Bevorzugte Sprache (n)**: die Sprache, auf die das Gerät festgelegt ist.

* **Zeitzone: die**Zeitzone, auf die das Gerät festgelegt ist.

#### <a name="network-settings"></a>Netzwerkeinstellungen

* Funk **Funk Einstellungen**: die drahtlos Einstellungen des Geräts (ob bestimmte Aspekte, z. b. drahtlose LAN, aktiviert oder deaktiviert sind).

#### <a name="power-settings"></a>Energieeinstellungen

* **Wenn der Bildschirm im Leerlauf ist, Bildschirm nach (Minuten)**: der Bildschirm wird angezeigt, nachdem sich das Gerät für diesen Zeitraum im Leerlauf befunden hat. Legen Sie den Wert auf **0** fest, um den Bildschirm nie zu

* **Wenn Sie sich im Leerlauf befinden,** deaktivieren Sie nach: das Gerät wird heruntergefahren, nachdem es sich für diesen Zeitraum im Leerlauf befunden hat.

* **Energie Modus**: der Energie Modus des Geräts. Weitere Informationen finden Sie [unter Informationen zum Energiesparmodus und zum sofort Strom](https://support.xbox.com/xbox-one/console/learn-about-power-modes) .

* **Automatisch starten der Konsole, wenn eine Verbindung mit der Stromversorgung**besteht: das Gerät wird automatisch eingeschaltet, wenn es mit einer Stromquelle verbunden ist.

#### <a name="preference-settings"></a>Einstellungs Einstellungen

* **Standardmäßige Startseite**: legt fest, welcher Startbildschirm angezeigt wird, wenn das Gerät eingeschaltet wird.

* **Verbindungen von der Xbox-App zulassen**: die Xbox-App auf einem anderen Gerät (z. b. einem Windows 10-PC) kann eine Verbindung mit dieser Konsole herstellen.

* **UWP-apps standardmäßig als Spiele behandeln**: Spiele und apps erhalten verschiedene Ressourcen, die Ihnen auf Xbox zugewiesen sind. Wenn Sie dieses Kontrollkästchen aktivieren, werden alle UWP-Pakete als Spiele identifiziert, sodass Sie mehr Ressourcen erhalten.

#### <a name="user-settings"></a>Benutzereinstellungen

* **Benutzer automatisch anmelden**: signiert den ausgewählten Benutzer automatisch, wenn das Gerät eingeschaltet ist.

* **Automatischer Anmelde Benutzer Controller**: ordnet einem bestimmten Benutzer automatisch einen bestimmten Controllertyp zu.

#### <a name="xbox-live-sandbox"></a>Xbox Live Sandbox

Hier können Sie die Xbox Live Sandbox ändern, in der sich das Gerät befindet. Geben Sie den Namen des Sandkastens in das Feld ein, und wählen Sie **ändern**aus.

### <a name="scratch"></a>Entwurf

Dies ist ein leerer Arbeitsbereich, den Sie an Ihre Vorlieben anpassen können. Sie können das Menü verwenden (Klicken Sie oben links auf die Menü Schaltfläche), um Tools hinzuzufügen (Wählen Sie Extras **zu Arbeitsbereich hinzufügen**und dann die Tools aus, die Sie hinzufügen und dann **Hinzufügen**möchten). Beachten Sie, dass Sie dieses Menü zum Hinzufügen von Tools zu einem beliebigen Arbeitsbereich und zum Verwalten der Arbeitsbereiche selbst verwenden können.

![Hinzufügen von Tools zum Arbeitsbereich](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>Spiel Ereignisdaten

Auf der Seite **Spiel Ereignisdaten** können Sie ein Echtzeit-Diagramm anzeigen, in dem die Anzahl von etw-Spielereignissen (Ereignis Ablauf Verfolgung für Windows) gestreamt wird, die derzeit auf Ihrer Xbox One aufgezeichnet werden. Wenn auf dem System Spielereignisse aufgezeichnet werden, können Sie auch Details (Ereignis Name, Ereignis vorkommen und Spieltitel) anzeigen, in denen jedes Ereignis in einer Datentabelle unterhalb des Daten Diagramms beschrieben wird. Die Tabelle ist nur verfügbar, wenn Ereignisse aufgezeichnet wurden.

![Spiel Ereignisdaten](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über das Windows-Geräteportal](../debug-test-perf/device-portal.md)
* [Referenz zu Kern-APIs des Device Portal](../debug-test-perf/device-portal-api-core.md)