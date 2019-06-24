---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Geräteportal für Xbox
description: Hier erfahren Sie, wie Sie das Geräteportal für Xbox One aktivieren.
ms.date: 4/9/2019
ms.topic: article
keywords: Windows 10, Uwp, Device-portal
ms.localizationpriority: medium
ms.openlocfilehash: c701dc7eef4d37f28db9f9cabd7bbb0f39a92a4c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322084"
---
# <a name="device-portal-for-xbox"></a>Geräteportal für Xbox

## <a name="set-up-device-portal-on-xbox"></a>Einrichten des Geräteportals für Xbox

Die folgenden Schritte zeigen, wie Sie das Xbox-Geräteportal aktivieren, das Ihnen den Remotezugriff auf Ihre Xbox ermöglicht.

1. Öffnen Sie Dev Home. Dies sollte standardmäßig beim Booten der Xbox geöffnet werden, aber Sie können es auch auf von der Startseite aus öffnen.

    ![„Dev Home“ im Geräteportal](images/device-portal-xbox-1.png)

2. Navigieren Sie in Dev Home zur Registerkarte **Home**, und wählen Sie im Abschnitt **Remotezugriff** die Option **Remotezugriffseinstellungen** aus.

    ![Geräteportal-RemoteManagement-Tool](images/device-portal-xbox-15.png)

3. Markieren Sie die Einstellung **Xbox-Geräteportal aktivieren**.

4. Wählen Sie unter **Authentifizierung** die Option **Benutzername und Kennwort festlegen**. Geben Sie einen **Benutzernamen** und ein **Kennwort** ein, um den Zugriff auf Ihr Dev-Kit über einen Browser zu authentifizieren und **speichern** Sie diese.

5. **Schließen** Sie die Seite **Remotezugriff** und notieren Sie die unter **Remotezugriff** auf der Registerkarte **Startseite** aufgeführte URL.

6. Geben Sie die URL in Ihrem Browser ein, und melden Sie sich mit den Anmeldeinformationen an, die Sie konfiguriert haben.

7. Sie erhalten eine Warnung zum Zertifikat, das bereitgestellt wurde, ähnlich der Warnung in der folgenden Abbildung. Klicken Sie in Microsoft Edge auf **Details** und auf **Weiter zur Webseite**, um auf das Xbox-Geräteportal zuzugreifen. Geben Sie in dem sich öffnenden Dialog den Benutzernamen und das Kennwort ein, die Sie zuvor auf Ihrer Xbox eingegeben haben.

    ![Fehler beim Zertifikat für das Device Portal](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Seiten von Device Portal

Das Xbox-Geräteportal bietet eine Reihe von Standardseiten ähnlich dem, was im Windows-Geräteportal verfügbar ist. Dazu gibt es mehrere Seiten, die einzigartig sind. Ausführliche Beschreibungen der gemeinsamen Seiten finden Sie unter [Übersicht über das Windows-Geräteportal](../debug-test-perf/device-portal.md). Die folgenden Abschnitte beschreiben die Seiten, die es nur im Xbox-Geräteportal gibt.

### <a name="home"></a>Startseite

Ähnlich wie die **Apps-Manager**-Seite des Windows-Geräteportals zeigt die **Startseite** des Xbox-Geräteportals unter **Meine Spiele und Apps** eine Liste der installierten Spiele und Apps an. Sie können auf den Namen eines Spiels oder einer App klicken, um weitere Details zu sehen, z. B. den **Paketfamiliennamen**. In der Dropdown-Liste **Aktionen** können Sie Aktionen für das Spiel oder die App ausführen, z. B. **Starten**.

Unter **Xbox Live-Testkonten** können Sie die mit Ihrer Xbox verbundenen Konten verwalten. Sie können Benutzer und Gastkonten hinzufügen, neue Benutzer anlegen, Benutzer an- und abmelden und Konten entfernen.

![Startseite](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (Gespeicherte Spiele)

Sowohl das Windows-Geräteportal als auch das Xbox-Geräteportal haben eine **Xbox Live**-Seite. Allerdings hat das Xbox-Geräteportal einen einzigartigen Bereich namens **Gespeicherte Xbox Live-Spiele**, in dem Sie Daten für Spiele, die auf Ihrer Xbox installiert sind, speichern können. Geben Sie die **Service-Konfigurations-ID (SCID)** (siehe [Xbox Live-Service-Konfiguration](https://docs.microsoft.com/gaming/xbox-live/xbox-live-service-configuration.md#get-your-ids)), den **Membernamen (MSA)** und den **Paketfamiliennamen (PFN)** ein, die dem Titel und dem Spiel zugeordnet sind, suchen Sie nach der **Eingabedatei (JSON oder XML)** und wählen Sie dann eine der Schaltflächen (**Zurücksetzen**, **Importieren**, **Exportieren** und **Löschen**) aus, um die gespeicherten Daten zu bearbeiten.

Im Abschnitt **Generieren** können Sie Dummy-Daten erzeugen und in der angegebenen Eingabedatei speichern. Geben Sie einfach **Container (Standardwert: 2)** , **Blobs (Standardwert: 3)** und **Blob-Größe (Standardwert: 1024)** ein und wählen Sie **Generieren** aus.

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>HTTP-Monitor

Der HTTP-Monitor ermöglicht es Ihnen, den entschlüsselten HTTP- und HTTPS-Datenverkehr Ihrer Anwendung oder Ihres Spiels anzuzeigen, wenn dieses auf Ihrer Xbox One läuft.

![HTTP-Monitor](images/device-portal-xbox-18.png)

Um ihn zu aktivieren, öffnen Sie Dev Home auf Ihrer Xbox One, gehen Sie auf die Registerkarte **Einstellungen** und aktivieren Sie im Feld **HTTP-Monitor-Einstellungen** die Option **HTTP-Monitor aktivieren**.

![Dev-Startseite: Netzwerk](images/device-portal-xbox-14.png)

Sobald aktiviert, können Sie im Xbox-Geräteportal den HTTP- und HTTPS-Datenverkehr **stoppen**, **löschen** und **speichern**, indem Sie die entsprechenden Schaltflächen auswählen.

### <a name="network-fiddler-tracing"></a>Netzwerk (Fiddler-Ablaufverfolgung)

Die Seite **Netzwerk** im Xbox-Geräteportal ist fast identisch mit der Seite **Networking** im Windows-Geräteportal. **Fiddler-Nachverfolgung** gibt es jedoch nur im Xbox-Geräteportal. Dadurch können Sie Fiddler auf Ihrem PC ausführen, um den HTTP- und HTTPS-Datenverkehr zwischen Ihrer Xbox One und dem Internet zu protokollieren und zu überprüfen. Weitere Informationen finden Sie unter [Verwendung von Fiddler mit Xbox One bei der Entwicklung für UWP](../xbox-apps/uwp-fiddler.md).

![Network](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>Medienaufzeichnung

Auf der Seite **Medienaufzeichnung** können Sie **Screenshot erstellen** auswählen, um einen Screenshot von Ihrer Xbox One zu machen. Danach werden Sie von Ihrem Browser aufgefordert, die Datei herunterzuladen. Sie können **HDR bevorzugen** aktivieren, wenn Sie den Screenshot in HDR machen wollen (wenn die Konsole dies unterstützt).

![Medienaufzeichnung](images/device-portal-xbox-12.png)

### <a name="settings"></a>Einstellungen

Auf der Seite **Einstellungen** können Sie verschiedene Einstellungen für Ihre Xbox One anzeigen und bearbeiten. Oben können Sie **Importieren** auswählen, um Einstellungen aus einer Datei zu importieren und **Exportieren**, um die aktuellen Einstellungen in eine TXT-Datei zu exportieren. Das Importieren von Einstellungen kann die Massenbearbeitung erleichtern, insbesondere bei der Konfiguration mehrerer Konsolen. Um eine zu importierende Einstellungsdatei zu erstellen, ändern Sie die Einstellungen so, wie Sie sie haben möchten, und exportieren Sie die Einstellungen. Dann können Sie diese Datei verwenden, um Einstellungen schnell und einfach für andere Konsolen zu importieren.

Es gibt mehrere Abschnitte mit unterschiedlichen Einstellungen zum Anzeigen und/oder Bearbeiten, die im Folgenden erläutert werden.

![Einstellungen](images/device-portal-xbox-20.png)

![Einstellungen](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>Geräteinformationen

* **Name des Geräts**: Der Name des Geräts. Zum Bearbeiten ändern Sie den Namen im Feld und wählen Sie **Speichern** aus.

* **Version des Betriebssystems**: Schreibgeschützt. Die Versionsnummer des Betriebssystems.

* **Betriebssystemedition**: Schreibgeschützt. Der Name der Hauptversion des Betriebssystems.

* **Xbox Live-Geräte-ID**: Schreibgeschützt.

* **-Konsole ID**: Schreibgeschützt.

* **Seriennummer**: Schreibgeschützt.

* **-Konsole Typ**: Schreibgeschützt. Der Typ des Xbox One-Geräts (Xbox One, Xbox One S oder Xbox One X).

* **Dev-Modus**: Schreibgeschützt. Der Entwicklermodus, in dem sich das Gerät befindet.

#### <a name="audio-settings"></a>Audioeinstellungen

* **Audio bitstromformat**: Das Format der Audiodaten.

* **HDMI-Audio**: Der Typ der Audio über den HDMI-Port.

* **Kopfhörer Format**: Das Format des Audios, die über die Kopfhörer geliefert wird.

* **Optische Audio**: Der Typ der Audio über den optischen Port.

* **Verwenden Sie HDMI- oder optischen audio Kopfhörer**: Aktivieren Sie dieses Kontrollkästchen, wenn Sie einen Kopfhörer über HDMI verbundenen oder optischen verwenden.

#### <a name="display-settings"></a>Anzeigeeinstellungen

* **Farbtiefe**: Die Anzahl der Bits, die für jede Farbkomponente eines einzelnen Pixels verwendet.

* **Farbraum**: Die Farbskala für die Anzeige verfügbar.

* **Bildschirmauflösung**: Die Auflösung der Anzeige.

* **Anzeigen der Verbindung**: Der Typ der Verbindung mit der Anzeige.

* **Zulassen der hoch dynamischen Bereichs (HDR)** : HDR ermöglicht auf die Anzeige. Nur für kompatible Displays verfügbar.

* **Zulassen von 4K**: Ermöglicht die Auflösung von 4K auf die Anzeige. Nur für kompatible Displays verfügbar.

* **Variable Aktualisierungsrate (VRR) ermöglichen**: Aktivieren Sie auf der Anzeige VRR. Nur für kompatible Displays verfügbar.

#### <a name="kinect-settings"></a>Kinect Einstellungen

Um diese Einstellungen zu ändern, muss ein Kinect-Sensor an die Konsole angeschlossen werden.

* **Aktivieren von Kinect**: Aktivieren Sie den angefügten Kinect-Sensor.

* **Force Kinect, die bei Änderung der app erneut laden**: Laden Sie die angefügten Kinect-Sensor neu, wenn eine andere app oder Ihrem Spiel ausgeführt wird.

#### <a name="localization-settings"></a>Lokalisierungseinstellungen

* **Geografische Region**: Die geografische Region, die das Gerät festgelegt ist. Muss ein spezifischer, zweistellige Ländercode sein (z. B. **DE** für Deutschland).

* **Bevorzugte Sprache(n)** : Die Sprache, die das Gerät festgelegt ist.

* **Zeitzone**: Die Zeitzone, die das Gerät festgelegt ist.

#### <a name="network-settings"></a>Netzwerkeinstellungen

* **Radio drahtloseinstellungen**: Die drahtlose Einstellungen des Geräts (gibt an, ob bestimmte Aspekte, z. B. w-LAN aktiviert oder deaktiviert sind).

#### <a name="power-settings"></a>Energieeinstellungen

* **Wenn im Leerlauf dim Bildschirm nach (Minuten)** : Der Bildschirm abgeblendet, wenn das Gerät für diesen Zeitraum im Leerlauf befunden hat. Auf **0** setzen, um den Bildschirm nie zu dimmen.

* **Wenn im Leerlauf ist, deaktivieren Sie nach dem**: Das Gerät wird heruntergefahren, nachdem sie für diesen Zeitraum im Leerlauf befunden hat.

* **Energiesparmodus**: Der Energiesparmodus des Geräts. Weitere Informationen finden Sie unter [Informationen zum Energiesparmodus und schnellen Hochfahren](https://support.xbox.com/xbox-one/console/learn-about-power-modes).

* **Automatisch Startkonsole beim Verbinden mit Power**: Das Gerät wird automatisch einschalten, wenn er mit einer Stromquelle verbunden ist.

#### <a name="preference-settings"></a>Präferenzeinstellungen

* **Standard Zuhause**: Legt fest, welche home-Bildschirm wird angezeigt, wenn das Gerät eingeschaltet wird.

* **Zulassen von Verbindungen von der Xbox App**: Die Xbox App auf einem anderen Gerät (z. B. eines Windows 10-PCs) kann in dieser Konsole eine Verbindung herstellen.

* **Behandeln von UWP-apps als Spiele standardmäßig**: Spiele und apps erhalten verschiedene Ressourcen, die sie auf der Xbox zugeordnet. Wenn Sie dieses Kontrollkästchen aktivieren, werden alle UWP-Pakete als Spiele identifiziert und erhalten somit mehr Ressourcen.

#### <a name="user-settings"></a>Benutzereinstellungen

* **Melden Sie sich die Benutzer automatisch**: In den ausgewählten Benutzer automatisch angemeldet, wenn das Gerät eingeschaltet wird.

* **Automatische Anmeldung der Benutzer Controller**: Ein bestimmter Domänencontroller ordnet automatisch zu einem bestimmten Benutzer.

#### <a name="xbox-live-sandbox"></a>Xbox Live-Sandbox

Hier können Sie die Xbox Live-Sandbox ändern, in der sich das Gerät befindet. Geben Sie den Namen der Sandbox in das Feld ein und wählen Sie **Ändern** aus.

### <a name="scratch"></a>Entwurf

Dies ist ein leerer Arbeitsbereich, den Sie nach Belieben anpassen können. Sie können das Menü verwenden (klicken Sie auf die Menü-Schaltfläche oben links), um Tools hinzuzufügen (wählen Sie **Tools zum Arbeitsbereich hinzufügen**, dann die Tools und dann **Hinzufügen** aus). Beachten Sie, dass Sie dieses Menü verwenden können, um Tools zu jedem Arbeitsbereich hinzuzufügen und die Arbeitsbereiche selbst zu verwalten.

![Tools zum Arbeitsbereich hinzufügen](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>Spielereignisdaten

Auf der **spielen Ereignisdaten** Seite sehen Sie ein, die Datenströme in Echtzeit-Diagramm in die Anzahl der derzeit auf Ihre Xbox One erfassten Ereignisse der Ereignisablaufverfolgung für Windows (ETW)-Spiele. Wenn es sich bei Spielen Ereignisse aufgezeichnet, auf dem System vorhanden sind, können Sie auch Details (Ereignisname, ereignisvorkommen und den Titel des Spiels) anzeigen, die jedes Ereignis in einer Datentabelle, unter dem Datendiagramm beschreibt. In der Tabelle ist nur verfügbar, wenn Ereignisse aufgezeichnet vorhanden sind.

![Spielereignisdaten](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>Siehe auch

* [Übersicht über die Windows Device Portal](../debug-test-perf/device-portal.md)
* [Device Portal-Core-API-Referenz](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
