---
ms.assetid: 41ac0142-4d86-4bb3-b580-36d0d6956091
title: Referenz zu Geräteportal-APIs für HoloLens
description: Hier erhalten Sie Informationen zu den HoloLens-REST-APIs für das Windows Device Portal, die Sie für den Zugriff auf die Daten und die programmatische Steuerung des Geräts verwenden können.
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, Uwp, Device-portal
ms.localizationpriority: medium
ms.openlocfilehash: 3aeb068908adf6d6c40a50cee3aececba1861ee8
ms.sourcegitcommit: 81511fddf1393dffcfc069c769bb149da99529b1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2019
ms.locfileid: "59013337"
---
# <a name="device-portal-api-reference-for-hololens"></a>Referenz zu Geräteportal-APIs für HoloLens

Alle Komponenten im Windows Device Portal basieren auf REST-APIs, die Sie für den Zugriff auf die Daten und die programmatische Steuerung des Geräts verwenden können.

## <a name="holographic-os"></a>Holographic – Betriebssystem

### <a name="get-https-requirements-for-the-device-portal"></a>Abrufen der HTTPS-Anforderungen für das Geräteportal

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die HTTPS-Anforderungen für das Geräteportal abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/os/webmanagement/settings/https |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-the-stored-interpupillary-distance-ipd"></a>Abrufen des gespeicherten Pupillenabstands (Interpupillary Distance, IPD)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den gespeicherten IPD-Wert abrufen. Der Wert wird in Millimeter zurückgegeben.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/os/settings/ipd |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-a-list-of-hololens-specific-etw-providers"></a>Abrufen einer Liste mit ETW-Anbietern für HoloLens

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Liste mit ETW-Anbietern für HoloLens abrufen, die nicht im System registriert sind.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/os/etw/customproviders |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes


### <a name="return-the-state-for-all-active-services"></a>Gibt den Status für alle aktiven Dienste zurück.

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Status aller Dienste abrufen, die derzeit ausgeführt werden.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/os/services |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes


### <a name="set-the-https-requirement-for-the-device-portal"></a>Festlegen der HTTPS-Anforderung für das Geräteportal

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die HTTPS-Anforderungen für das Geräteportal festlegen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/management/settings/https |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Erforderlich   | (**Erforderlich**) Bestimmt, ob für das Geräteportal HTTPS erforderlich ist. Die möglichen Werte lauten **yes**, **no** und **default**. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes


### <a name="set-the-interpupillary-distance-ipd"></a>Festlegen des Pupillenabstands (Interpupillary Distance, IPD)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den gespeicherten IPD-Wert festlegen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/os/settings/ipd |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| ipd   | (**erforderlich**) Der neue IPD-Wert, der gespeichert werden soll. Dieser Wert sollte in Millimeter angegeben werden. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes


## <a name="holographic-perception"></a>Holographic – Wahrnehmung

### <a name="accept-websocket-upgrades-and-run-a-mirage-client-that-sends-updates"></a>Akzeptieren von WebSocket-Upgrades und Ausführen eines Mirage-Clients, der Updates sendet

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie WebSocket-Upgrades akzeptieren und einen Mirage-Client ausführen, der Updates mit 30 fps sendet.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET/WebSocket | /api/holographic/perception/client |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| clientmode   | (**erforderlich**) Bestimmt den Nachverfolgungsmodus. Mit dem Wert **active** wird der visuelle Nachverfolgungsmodus erzwungen, wenn er nicht passiv erreicht werden kann. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes


## <a name="holographic-thermal"></a>Holographic – Wärme

### <a name="get-the-thermal-stage-of-the-device"></a>Abrufen des Wärmestatus des Geräts

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Wärmestatus des Geräts abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/ |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

In der folgenden Tabelle werden die möglichen Werte angegeben.

| Wert | Beschreibung |
| --- | --- |
| 1 | Normal |
| 2 | Normal Auslastung |
| 3 | Kritisch |

**Statuscode**

- Standardstatuscodes

## <a name="hsimulation-control"></a>HSimulation-Steuerung
### <a name="create-a-control-stream-or-post-data-to-a-created-stream"></a>Erstellen eines Steuerungsdatenstroms oder Senden von Daten an einen erstellten Datenstrom

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie einen Steuerungsdatenstrom erstellen oder Daten an einen erstellten Datenstrom senden. Die gesendeten Daten müssen vom Typ **application/octet-stream** sein.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simulation/control/stream |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| priority   | (**erforderlich, wenn ein Steuerungsdatenstrom erstellt wird**) Gibt die Priorität des Datenstroms an. |
| streamid   | (**erforderlich, wenn an einen erstellten Datenstrom gesendet wird**) Der Bezeichner für den Datenstrom, an den gesendet werden soll. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="delete-a-control-stream"></a>Löschen eines Steuerungsdatenstroms

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie einen Steuerungsdatenstrom löschen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/control/stream |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-a-control-stream"></a>Abrufen eines Steuerungsdatenstroms.

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WebSocket-Verbindung für einen Steuerungsdatenstrom öffnen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET/WebSocket | /api/holographic/simulation/control/stream |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-the-simulation-mode"></a>Wurde der Simulationsmodus abrufen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Simulationsmodus abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/simulation/control/mode |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="set-the-simulation-mode"></a>Legen Sie den Simulationsmodus

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Simulationsmodus festlegen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simluation/control/mode |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| mode   | (**erforderlich**) Gibt den Simulationsmodus an. Die möglichen Werte lauten **default**, **simulation**, **remote** und **legacy**. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

## <a name="hsimulation-playback"></a>HSimulation-Wiedergabe

### <a name="delete-a-recording"></a>Löschen einer Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Aufzeichnung löschen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/playback/file |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Aufzeichnung   | (**erforderlich**) Der Name der zu löschenden Aufzeichnung. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-all-recordings"></a>Abrufen aller Aufzeichnungen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie alle verfügbaren Aufzeichnungen abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/files |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-the-types-of-data-in-a-loaded-recording"></a>Abrufen der Typen von Daten in einer geladenen Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Typen der Daten in einer geladenen Aufzeichnung abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session/types |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Aufzeichnung   | (**erforderlich**) Der Name der betreffenden Aufzeichnung. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-all-the-loaded-recordings"></a>Abrufen aller geladenen Aufzeichnungen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie alle geladenen Aufzeichnungen abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session/files |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-the-current-playback-state-of-a-recording"></a>Abrufen des aktuellen Wiedergabestatus einer Aufzeichnung 

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den aktuellen Wiedergabestatus einer Aufzeichnung abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/simulation/playback/session |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Aufzeichnung   | (**erforderlich**) Der Name der betreffenden Aufzeichnung. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="load-a-recording"></a>Laden einer Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Aufzeichnung laden.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/file |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Aufzeichnung   | (**erforderlich**) Der Name der zu ladenden Aufzeichnung. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="pause-a-recording"></a>Anhalten einer Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Aufzeichnung anhalten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/pause |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Aufzeichnung   | (**erforderlich**) Der Name der anzuhaltenden Aufzeichnung. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="play-a-recording"></a>Wiedergeben einer Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Aufzeichnung wiedergeben.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/play |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Aufzeichnung   | (**erforderlich**) Der Name der wiederzugebenden Aufzeichnung. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="stop-a-recording"></a>Beenden einer Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Aufzeichnung beenden.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/session/stop |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Aufzeichnung   | (**erforderlich**) Der Name der zu beendenden Aufzeichnung. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="unload-a-recording"></a>Entladen einer Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Aufzeichnung entladen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/holographic/simulation/playback/session/file |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Aufzeichnung   | (**erforderlich**) Der Name der zu entladenden Aufzeichnung. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="upload-a-recording"></a>Hochladen einer Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Aufzeichnung hochladen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simulation/playback/file |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

## <a name="hsimulation-recording"></a>HSimulation-Aufzeichnung

### <a name="get-the-recording-state"></a>Abrufen des Aufzeichnungsstatus

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den aktuellen Aufzeichnungsstatus abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/simulation/recording/status |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="start-a-recording"></a>Starten einer Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Aufzeichnung starten. Es kann nur jeweils eine aktive Aufzeichnung vorhanden sein. 
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simulation/recording/start |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| head   | (**siehe unten**) Legen Sie diesen Wert auf 1 fest, um anzugeben, dass das System Kopfdaten aufzeichnen soll. |
| hands   | (**siehe unten**) Legen Sie diesen Wert auf 1 fest, um anzugeben, dass das System Handdaten aufzeichnen soll. |
| spatialMapping   | (**siehe unten**) Legen Sie diesen Wert auf 1 fest, um anzugeben, dass das System Spatial-Mapping-Daten aufzeichnen soll. |
| environment   | (**siehe unten**) Legen Sie diesen Wert auf 1 fest, um anzugeben, dass das System Umgebungsdaten aufzeichnen soll. |
| name   | (**erforderlich**) Der Name der Aufzeichnung. |
| singleSpatialMappingFrame   | (**optional**) Legen Sie diesen Wert auf 1 fest, um anzugeben, dass nur ein einzelner Spatial-Mapping-Frame aufgezeichnet werden soll. |

Für diese Parameter muss genau einer der folgenden Parameter auf 1 festgelegt werden: *head*, *hands*, *spatialMapping* oder *environment*.

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="stop-the-current-recording"></a>Beenden der aktuellen Aufzeichnung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die aktuelle Aufzeichnung beenden. Die Aufzeichnung wird als Datei zurückgegeben.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/simulation/recording/stop |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

## <a name="mixed-reality-capture"></a>Mixed-Reality-Aufnahme

### <a name="delete-a-mixed-reality-capture-mrc-recording-from-the-device"></a>Löschen einer Mixed Reality-Aufnahme (Mixed Reality Capture, MRC) vom Gerät

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine MRC-Aufzeichnung löschen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
DELETE | /api/holographic/mrc/file |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Dateiname   | (**erforderlich**) Der Name der zu löschenden Videodatei. Der Name sollte hex64-codiert sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="download-a-mixed-reality-capture-mrc-file"></a>Herunterladen einer MRC-Datei (Mixed Reality Capture)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine MRC-Datei vom Gerät herunterladen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/mrc/file |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Dateiname   | (**erforderlich**) Der Name der Videodatei, die Sie abrufen möchten. Der Name sollte hex64-codiert sein. |
| op   | (**optional**) Legen Sie diesen Wert auf **stream** fest, wenn Sie einen Datenstrom herunterladen möchten. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-the-mixed-reality-capture-mrc-settings"></a>Abrufen der MRC-Einstellungen (Mixed Reality Capture)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die MRC-Einstellungen abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/mrc/settings |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-the-status-of-the-mixed-reality-capture-mrc-recording"></a>Abrufen des Status der MRC-Aufzeichnung (Mixed Reality Capture)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Status der MRC-Aufzeichnung abrufen. Mögliche Werte: **running** und **stopped**.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/mrc/status |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="get-the-list-of-mixed-reality-capture-mrc-files"></a>Abrufen der Liste der MRC-Dateien (Mixed Reality Capture)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die auf dem Gerät gespeicherten MRC-Dateien abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/mrc/files |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="set-the-mixed-reality-capture-mrc-settings"></a>Festlegen der MRC-Einstellungen (Mixed Reality Capture)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die MRC-Einstellungen festlegen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/mrc/settings |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="starts-a-mixed-reality-capture-mrc-recording"></a>Starten einer MRC-Aufzeichnung (Mixed Reality Capture)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine MRC-Aufzeichnung starten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/mrc/video/control/start |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="stop-the-current-mixed-reality-capture-mrc-recording"></a>Beenden der aktuellen MRC-Aufzeichnung (Mixed Reality Capture)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die aktuelle MRC-Aufzeichnung beenden.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/holographic/mrc/video/control/stop |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="take-a-mixed-reality-capture-mrc-photo"></a>Aufnehmen eines MRC-Fotos (Mixed Reality Capture)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie ein MRC-Foto aufnehmen. Das Foto wird im JPEG-Format zurückgegeben.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/mrc/photo |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

## <a name="mixed-reality-streaming"></a>Mixed-Reality-Streaming

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Starten des portionsweisen Downloads einer fragmentierten MP4-Datei

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den portionsweisen Download einer fragmentierten MP4-Datei starten. Diese API verwendet die Standardqualität.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/stream/live.mp4 |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| pv   | (**optional**) Gibt an, ob Aufnahmen der Foto-/Videokamera aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| holo   | (**optional**) Gibt an, ob Hologramme aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| mic   | (**optional**) Gibt an, ob Aufnahmen des Mikrofons aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| loopback   | (**optional**) Gibt an, ob Audioaufnahmen der Anwendung aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Starten des portionsweisen Downloads einer fragmentierten MP4-Datei

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den portionsweisen Download einer fragmentierten MP4-Datei starten. Diese API verwendet die hohe Qualität.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_high.mp4 |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| pv   | (**optional**) Gibt an, ob Aufnahmen der Foto-/Videokamera aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| holo   | (**optional**) Gibt an, ob Hologramme aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| mic   | (**optional**) Gibt an, ob Aufnahmen des Mikrofons aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| loopback   | (**optional**) Gibt an, ob Audioaufnahmen der Anwendung aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Starten des portionsweisen Downloads einer fragmentierten MP4-Datei

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den portionsweisen Download einer fragmentierten MP4-Datei starten. Diese API verwendet die niedrige Qualität.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_low.mp4 |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| pv   | (**optional**) Gibt an, ob Aufnahmen der Foto-/Videokamera aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| holo   | (**optional**) Gibt an, ob Hologramme aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| mic   | (**optional**) Gibt an, ob Aufnahmen des Mikrofons aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| loopback   | (**optional**) Gibt an, ob Audioaufnahmen der Anwendung aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes

### <a name="initiates-a-chunked-download-of-a-fragmented-mp4"></a>Starten des portionsweisen Downloads einer fragmentierten MP4-Datei

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den portionsweisen Download einer fragmentierten MP4-Datei starten. Diese API verwendet die mittlere Qualität.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/holographic/stream/live_med.mp4 |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| pv   | (**optional**) Gibt an, ob Aufnahmen der Foto-/Videokamera aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| holo   | (**optional**) Gibt an, ob Hologramme aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| mic   | (**optional**) Gibt an, ob Aufnahmen des Mikrofons aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |
| loopback   | (**optional**) Gibt an, ob Audioaufnahmen der Anwendung aufgezeichnet werden sollen. Sollte **true** oder **false** sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

- Standardstatuscodes
