---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal für HoloLens
description: Hier erfahren Sie, wie Sie mit dem Windows Device Portal für HoloLens Ihr HoloLens-Gerät per Fernzugriff konfigurieren und verwalten können.
ms.date: 01/3/2019
ms.topic: article
keywords: Windows 10, Uwp, Device-portal
ms.localizationpriority: medium
ms.openlocfilehash: 2561f18e2ac054c8b378b0c7c0a9689bebcc4140
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611255"
---
# <a name="device-portal-for-hololens"></a>Device Portal für HoloLens


## <a name="set-up-device-portal-on-hololens"></a>Einrichten des Geräteportals für HoloLens

### <a name="enable-device-portal"></a>Aktivieren des Geräteportals

1. Schalten Sie die HoloLens ein, und setzen Sie sie auf.
2. Führen Sie die [Blütengeste](https://dev.windows.com/holographic/Gestures.html#Bloom) aus, um das Hauptmenü zu starten.
3. Betrachten Sie die Kachel **Einstellungen**, und führen Sie die [Zeigefingergeste](https://dev.windows.com/holographic/Gestures.html#Press_and_release) aus. Führen Sie die Zeigefingergeste ein zweites Mal aus, um die App in Ihrer Umgebung zu platzieren. Die Einstellungs-App wird gestartet, nachdem Sie sie platziert haben.
4. Wählen Sie das Menüelement **Aktualisieren** aus.
5. Wählen Sie das Menüelement **Für Entwickler** aus.
6. Aktivieren Sie den **Entwicklermodus**.
7. [Scrollen Sie nach unten](https://dev.windows.com/holographic/Gestures.html#Navigation), und aktivieren Sie Device Portal.


### <a name="pair-your-device"></a>Koppeln des Geräts

#### <a name="connect-over-wi-fi"></a>Herstellen einer WLAN-Verbindung 

1. Verbinden Sie die HoloLens mit dem WLAN.
2. Suchen Sie die IP-Adresse Ihres Geräts. Suchen Sie die IP-Adresse auf dem Gerät unter **Einstellungen > Netzwerk und Internet > Wi-Fi > Hardwareeigenschaften**. Sie können auch fragen „Hey Cortana, wie lautet meine IP-Adresse?“

3. Rufen Sie in einem Webbrowser auf dem PC „`https://<YOUR_HOLOLENS_IP_ADDRESS>`“ auf.
    - Die folgende Meldung wird im Browser angezeigt werden: "Es ist ein Problem mit dem Sicherheitszertifikat der Website". Der Grund dafür ist, dass das für das Geräteportal ausgestellte Zertifikat ein Testzertifikat ist. Sie können diesen Zertifikatfehler vorerst ignorieren und fortfahren.

#### <a name="connect-over-usb"></a>Herstellen einer Verbindung über USB 

1. Installieren Sie die Tools, um sicherzustellen, dass auf dem PC Visual Studio Update 1 mit den Windows 10-Entwicklertools installiert ist. Hierdurch wird USB-Konnektivität aktiviert.
2. Schließen Sie die HoloLens mit einem Micro-USB-Kabel am PC an.
3. Rufen Sie in einem Webbrowser auf dem PC „`http://127.0.0.1:10080`“ auf.

> [!IMPORTANT]
> Wenn Ihr PC das Gerät nicht finden kann, verwenden Sie die tatsächliche Netzwerk-IP-Adresse des Geräts HoloLens, statt `http://127.0.0.1:10080`.

#### <a name="connect-to-an-emulator"></a>Herstellen einer Verbindung mit einem Emulator 

Sie können das Geräteportal auch mit dem Emulator verwenden. Verwenden Sie die Symbolleiste, um die Verbindung mit dem Geräteportal herzustellen. Klicken Sie auf dieses Symbol:
- Device-Portal zu öffnen: Öffnen Sie die Windows Device Portal für das Betriebssystem HoloLens im Emulator.

#### <a name="create-a-username-and-password"></a>Erstellen eines Benutzernamens und Kennworts 

Wenn Sie das erste Mal eine Verbindung der HoloLens mit dem Geräteportal herstellen, müssen Sie einen Benutzernamen und ein Kennwort erstellen.
1. Geben Sie in einem Webbrowser auf dem PC die IP-Adresse der HoloLens ein. Die Seite „Set up access“ wird geöffnet.
2. Klicken oder tippen Sie auf „Request pin“, und betrachten Sie die HoloLens-Anzeige, um die generierte PIN abzurufen.
3. Geben Sie im Textfeld „PIN displayed on your device“ die PIN ein.
4. Geben Sie den Benutzernamen ein, den Sie zum Herstellen der Verbindung mit dem Geräteportal verwenden. Dabei muss es sich nicht um den Namen eines Microsoft-Kontos (MSA) oder einen Domänennamen handeln.
5. Geben Sie ein Kennwort ein, und bestätigen Sie es. Das Kennwort muss mindestens sieben Zeichen lang sein. Es muss kein MSA- oder Domänenkennwort sein.
6. Klicken Sie auf „Koppeln“, um auf der HoloLens eine Verbindung mit dem Windows Device Portal herzustellen.

Wenn Sie den Benutzernamen oder das Kennwort ändern möchten, können Sie diesen Vorgang jederzeit wiederholen, indem Sie die Seite für Gerätesicherheit besuchen. Klicken Sie hierzu oben rechts auf den Link „Sicherheit“, oder navigieren Sie zu „`https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`“.

#### <a name="security-certificate"></a>Sicherheitszertifikat 

Wenn im Browser eine Meldung zu einem Zertifikatfehler angezeigt wird, können Sie diesen beheben, indem Sie eine Vertrauensstellung mit dem Gerät erstellen.

Jede HoloLens generiert ein eindeutiges selbstsigniertes Zertifikat für die SSL-Verbindung. Standardmäßig wird dieses Zertifikat vom Webbrowser des PC nicht als vertrauenswürdig angesehen, und Sie erhalten möglicherweise eine Meldung zu einem Zertifikatfehler. Sie können dieses Zertifikat von der HoloLens herunterladen (über USB oder ein vertrauenswürdiges WLAN-Netzwerk) und es auf dem PC als vertrauenswürdig einstufen, um eine sichere Verbindung mit dem Gerät herzustellen.
1. Vergewissern Sie sich, dass Sie sich in einem sicheren Netzwerk (USB-Verbindung oder vertrauenswürdiges WLAN-Netzwerk) befinden.
2. Laden Sie das Zertifikat des Geräts von der Seite „Sicherheit“ im Geräteportal herunter. Klicken Sie in der rechten oberen Liste der Symbole auf den Link „Sicherheit“, oder navigieren Sie zu „`https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`“.

3. Installieren Sie das Zertifikat, das in der "Vertrauenswürdige Stammzertifizierungsstellen" auf Ihrem PC - Typ im Windows-Menü zu speichern: Verwalten von Zertifikaten für Computer, und starten Sie das Applet.
    - Erweitern Sie den Ordner „Vertrauenswürdige Stammzertifizierungsstellen“.
    - Klicken Sie auf den Ordner „Zertifikate“.
    - Wählen Sie im Menü Aktion: Alle Aufgaben > Import...
    - Führen Sie den Zertifikatimport-Assistenten mit der Zertifikatdatei aus, die Sie vom Geräteportal heruntergeladen haben.

4. Starten Sie den Browser neu.


## <a name="device-portal-pages"></a>Seiten des Geräteportals 

### <a name="home"></a>Startseite 

Die Geräteportalsitzung beginnt auf der Startseite. Der Zugriff auf andere Seiten erfolgt über die Navigationsleiste links von der Startseite.

Die Symbolleiste am oberen Rand der Seite ermöglicht den Zugriff auf häufig verwendete Status und Features.
- **Online**: Gibt an, ob das Gerät auf-WLAN verbunden ist.
- **Herunterfahren**: Das Gerät wird deaktiviert.
- **Starten Sie neu**: Zyklen Einschalten des Geräts.
- **Sicherheit**: Öffnet die Seite Sicherheit für Geräte.
- **Cool**: Gibt die Temperatur des Geräts an.
- **A/C**: Gibt an, ob das Gerät im Netzbetrieb befindet, und geladen.
- **Hilfe**: Öffnet die Dokumentationsseite für REST-Schnittstelle.

Auf der Startseite werden die folgenden Informationen angezeigt:
- **Gerätestatus**: Überwacht die Integrität des Geräts und meldet schwerwiegende Fehler.
- **Windows-Informationen**: Zeigt den Namen der HoloLens und die derzeit installierte Version von Windows an.
- **Einstellungen**: Dieser Abschnitt enthält die folgenden Einstellungen:
    - **IPD**: Abstand der interpupillary (IPD), d. h. die Entfernung, in Millimeter zwischen den Mittelpunkt der Schüler des Benutzers, bei der Suche direkt fort. Die Einstellung wird sofort wirksam. Der Standardwert wurde beim Einrichten des Geräts automatisch berechnet.
    - **Name des Geräts**: Weisen Sie einen Namen, um die HoloLens. Nach dem Ändern dieses Werts müssen Sie das Gerät neu starten, damit er wirksam wird. Nach dem Klicken auf „Speichern“ wird ein Dialogfeld mit der Frage angezeigt, ob Sie das Gerät sofort oder später neu starten möchten.
    - **Energiesparmoduseinstellungen**: Legt fest, wie lange gewartet, bevor das Gerät wird in den Standbymodus, wenn es im Netzbetrieb befindet und wenn es im Akkubetrieb befindet.

### <a name="3d-view"></a>3D View 

Auf der Seite „3D View“ können Sie erkennen, wie die HoloLens Ihre Umgebung interpretiert. Navigieren Sie mit der Maus in der Ansicht:
- **Drehen**: Linksklick + Bewegen der Maus
- **Schwenken**: Rechtsklick + Bewegen der Maus
- **Zoomen**: Drehen des Mausrads
- **Überwachungsoptionen für**: Aktivieren Sie fortlaufende visuelle Verfolgung durch Überprüfen der visuelle Verfolgung erzwingen. Mit „Anhalten“ wird die visuelle Nachverfolgung angehalten.
- **Ansichtsoptionen**: Festlegen von Optionen auf der 3D-Ansicht: – verfolgen: Gibt an, ob visuelle Verfolgung aktiv ist.
- **Anzeigen von Floor**: Zeigt eine kariertes Floor-Ebene.
- **Anzeigen von Frustums**: Zeigt die Ansicht Frustums an.
- **Anzeigen der Stabilisierung Ebene**: Zeigt die Ebene, die HoloLens zur Stabilisierung während der Übertragung verwendet.
- **Anzeigen von Mesh**: Zeigt die Mesh Oberfläche Zuordnung, das Ihrer Umgebung darstellt.
- **Anzeigen von Details**: Zeigt übergeben Positionen, Head Drehung Quaternionen und der Gerät-Ursprung-Vektor, wie sie in Echtzeit zu ändern.
- **Schaltfläche "Vollbild"**: Zeigt die 3D-Ansicht im Vollbildmodus an. Drücken Sie die ESC-Taste, um die Vollbildansicht zu beenden.

- Surface-Wiederaufbau: Klicken Sie oder tippen Sie Update aus, um das aktuelle räumliche Zuordnung Netz vom Gerät angezeigt. Ein vollständiger Durchlauf kann bis zu einige Sekunden lang dauern. Das Gitter wird in der 3D-Ansicht nicht automatisch aktualisiert. Sie müssen auf „Update“ klicken, um das aktuelle Gitter vom Gerät abzurufen. Klicken Sie auf „Speichern“ um das aktuelle Spatial-Mapping-Gitter als OBJ-Datei auf dem PC zu speichern.

### <a name="mixed-reality-capture"></a>Mixed Reality Capture 

Auf der Seite „Mixed Reality Capture“ können Sie Mediendatenströme von der HoloLens speichern.
- Einstellungen: Steuern Sie die Media-Datenströme, die erfasst werden, indem Sie die folgenden Einstellungen zu überprüfen:-Hologramme: Erfasst den holographic Inhalt in den video Stream an. Hologramme werden in Mono und nicht in Stereo gerendert.
- **PV Kamera**: Erfasst den Videostream zwischen der Kamera und Fotos/Video.
- **Mic Audio**: Erfasst die Audiodaten aus dem Mikrofon-Array.
- **App Audio**: Erfasst die Audiodaten aus dem die derzeit ausgeführte app.
- **Live-Vorschau-Qualität**: Wählen Sie die Bildschirmauflösung, Framerate und streaming-Rate für die Livevorschau an.

- Klicken oder tippen Sie auf die Schaltfläche „Live preview“, um den Aufnahmedatenstrom anzuzeigen. Mit „Stop live preview“ wird der Aufnahmedatenstrom beendet.
- Klicken oder tippen Sie auf „Aufzeichnen“, um die Aufzeichnung des Mixed-Reality-Datenstroms mit den angegebenen Einstellungen zu starten. Mit „Aufnahme beenden“ wird die Aufzeichnung beendet und gespeichert.
- Klicken oder tippen Sie auf „Take photo“, um ein Standbild des Aufnahmedatenstroms zu erstellen.
- **Videos und Fotos**: Zeigt eine Liste von Videos und Fotos Erfassungen, die auf dem Gerät ausgeführt.

Beachten Sie, dass HoloLens-Apps kein MRC-Foto oder -Video aufnehmen können, während Sie über das Geräteportal eine Live-Vorschau aufzeichnen oder streamen.

### <a name="system-performance"></a>Systemleistung 

Das Tool „Systemleistung“ der HoloLens bietet drei zusätzliche Metriken, die aufgezeichnet werden können. 

Die folgenden Metriken sind verfügbar:
- **SoC Power**: Sofortige energienutzung System on a Chip, gemittelt über eine minute
- **Stromnetz**: Sofortige System energienutzung, gemittelt über eine minute
- **Framerate**: Frames pro Sekunde verpasst VBlanks pro Sekunde und aufeinander folgende verpasst VBlanks

### <a name="app-crash-dumps-page"></a>Seite „App Crash Dumps“ 

Auf dieser Seite können Sie Absturzabbilder für die quergeladenen Apps erfassen. Aktivieren Sie das Kontrollkästchen „Crash Dumps Enabled“ für jede App, für die Sie Absturzabbilder erfassen möchten. Kehren Sie zum Erfassen von Absturzabbildern zu dieser Seite zurück. Speicherabbilddateien können zum Debuggen in Visual Studio geöffnet werden.

### <a name="kiosk-mode"></a>Kioskmodus 

Aktiviert den Kioskmodus, in dem die Möglichkeiten des Benutzers zum Starten neuer Apps oder Ändern der derzeit ausgeführten App beschränkt sind. Wenn der Kioskmodus aktiviert ist, sind die Blütengeste und Cortana deaktiviert, und platzierte Apps werden in der Umgebung des Benutzers nicht angezeigt.

Markieren Sie „Enable Kiosk Mode“, um die HoloLens in den Kioskmodus zu versetzen. Wählen Sie in der Dropdownliste „Startup app“ die App aus, die beim Starten ausgeführt werden soll. Klicken oder tippen Sie auf „Speichern“, um die Einstellungen zu übernehmen.

Beachten Sie, dass die App auch dann beim Starten ausgeführt wird, wenn der Kioskmodus nicht aktiviert ist. Wählen Sie „Keine“ aus, wenn beim Starten keine App ausgeführt werden soll.

### <a name="simulation"></a>Simulation 

Ermöglicht Ihnen das Aufzeichnen und Wiedergeben von Eingabedaten für Testzwecke.
- **Erfassen Sie Raum**: Verwendet, um eine simulierte Platz-Datei herunterzuladen, die das Netz räumliche Zuordnung für die benutzerumgebung enthält. Benennen Sie den Raum, und klicken Sie auf „Aufnahme“, um die Daten als XEF-Datei auf dem PC zu speichern. Diese Raumdatei kann in den HoloLens-Emulator geladen werden.
- **Aufzeichnung**: Überprüfen Sie die Datenströme aufzuzeichnen, Name der Aufzeichnung, und klicken bzw. Tippen Sie auf der Eintrag zum Starten der Aufzeichnung. Führen Sie mit der HoloLens Aktionen aus, und klicken Sie dann auf „Beenden“, um die Daten als XEF-Datei auf dem PC zu speichern. Diese Datei kann im HoloLens-Emulator oder auf dem Gerät geladen werden.
- **Playback**: Klicken Sie oder tippen Sie zum auswählen eine Xef Datei von Ihrem PC und senden die Daten an die HoloLens aufzeichnen hochladen.
- **Steuerungsmodus**: Standard oder einer Simulation in der Dropdownliste auswählen, und klicken oder tippen Sie auf die Schaltfläche "festlegen", um den Modus für die HoloLens auszuwählen. Durch Auswahl von „Simulation“ werden die realen Sensoren auf der HoloLens deaktiviert und stattdessen hochgeladene simulierte Daten verwendet. Wenn Sie zu „Simulation“ wechseln, reagiert die HoloLens nicht auf den realen Benutzer, bis Sie zurück zu „Standard“ wechseln.


### <a name="virtual-input"></a>Virtual Input 

Sendet die Tastatureingabe vom Remotecomputer an die HoloLens.

Klicken oder tippen Sie auf den Bereich unter „Virtual keyboard“, um das Senden von Tastatureingaben an die HoloLens zu aktivieren. Geben Sie im Textfeld „Eingabetext“ Text ein, und klicken oder tippen Sie auf „Senden“, um die Tastatureingaben an die aktive App zu senden.

## <a name="see-also"></a>Siehe auch

* [Übersicht über die Windows Device Portal](device-portal.md)
* [Geräteportal Kern-API-Referenz](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core) (APIs für alle Windows 10-Geräte)
* [Geräteportal Mixed-Reality-API-Referenz](https://docs.microsoft.com/windows/mixed-reality/device-portal-api-reference) (einer erweiterte Liste aller REST-APIs für HoloLens)
