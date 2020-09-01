---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: In diesem Artikel werden die für UWP-Apps verfügbaren Kamera-Features aufgeführt, sowie die Links zu den Anleitungen für ihre Verwendung.
title: Kamera
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e5b78364f5d59889f249d6d23d414528461368b4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161014"
---
# <a name="camera"></a>Kamera

Dieser Abschnitt enthält Richtlinien für das Erstellen von UWP (Universelle Windows-Plattform)-Apps, die die Kamera oder das Mikrofon verwenden, um Fotos, Videos oder Audiodateien zu erfassen.

## <a name="use-the-windows-built-in-camera-ui"></a>Verwenden der in Windows integrierten Kamera-UI

| Thema | BESCHREIBUNG |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Aufnehmen von Fotos und Videos mit der in Windows integrierten Kamera-UI](capture-photos-and-video-with-cameracaptureui.md) | Zeigt, wie Sie die [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse zum Aufnehmen von Fotos oder Videos mit der in Windows integrierten Kamera-UI verwenden. Wenn Sie einfach den Benutzer dazu befähigen möchten, ein Foto oder Video aufzuzeichnen und das Ergebnis an Ihre App zurückzugeben, ist dies die schnellste und einfachste Möglichkeit dafür.  |

## <a name="basic-mediacapture-tasks"></a>Grundlegende MediaCapture-Funktionen

| Thema | BESCHREIBUNG |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Anzeigen der Kameravorschau](simple-camera-preview-access.md) | Beschreibt, wie Sie in einer UWP-App innerhalb einer XAML-Seite den Kameravorschau-Stream schnell anzeigen können. |
| [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) | Zeigt die einfachste Möglichkeit zum Aufnehmen von Fotos und Videos mit der [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture)-Klasse. Die **MediaCapture**-Klasse stellt einen leistungsfähigen Satz von APIs bereit, der eine Steuerung der Aufnahmepipeline auf unterster Ebene sowie fortgeschrittene Aufnahmeszenarien ermöglicht. Dieser Artikel soll Ihnen jedoch helfen, Ihre App schnell und einfach durch grundlegende Medienaufnahmefunktionen zu erweitern. |
| [Kamera-UI-Features für mobile Geräte](camera-ui-features-for-mobile-devices.md) | Beschreibt, wie Sie spezielle Kamera-UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>Erweiterte MediaCapture-Funktionen   
                                                                                                               
| Thema                                                                                             | BESCHREIBUNG                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“](handle-device-orientation-with-mediacapture.md) | Beschreibt, wie Sie beim Aufnehmen von Fotos und Videos mit einer Hilfsklasse die Geräteausrichtung handhaben. | 
| [Entdecken und Auswählen von Kamerafunktionen mit Kameraprofilen](camera-profiles.md) | Beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten. Dazu gehören Aufgaben, z. B. das Auswählen von Profilen, die bestimmte Auflösungen oder Bildfrequenzen unterstützen, von Profilen, die gleichzeitigen Zugriff auf mehrere Kameras unterstützen, sowie von Profilen, die HDR unterstützen. |
| [Festlegen von Format, Auflösung und Bildfrequenz für „MediaCapture“](set-media-encoding-properties.md) | Beschreibt, wie Sie die [**IMediaEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties)-Schnittstelle verwenden, um die Auflösung und Bildfrequenz des Kameravorschau-Datenstroms sowie von aufgenommenen Fotos und Videos festzulegen. Außerdem wird beschrieben, wie Sie sicherstellen, dass das Seitenverhältnis des Vorschaudatenstroms mit dem Seitenverhältnis der aufgenommenen Medien übereinstimmt. |
| [Aufnehmen von HDR-Fotos und Fotos bei schlechten Lichtverhältnissen](high-dynamic-range-hdr-photo-capture.md) | Beschreibt, wie Sie die [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)-Klasse verwenden, um HDR (High Dynamic Range)-Fotos und Fotos bei schlechten Lichtverhältnissen aufzunehmen. |
| [Manuelle Kamerasteuerelemente für Foto- und Videoaufnahmen](capture-device-controls-for-photo-and-video-capture.md) | Beschreibt, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Foto- und Videoaufnahmeszenarien (einschließlich optischer Bildstabilisierung und fließendem Zoom) zu ermöglichen. |
| [Manuelle Kamerasteuerelemente für die Videoaufnahme](capture-device-controls-for-video-capture.md) | Beschreibt, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Videoaufnahmeszenarien, einschließlich HDR-Video und Belichtungspriorität, zu ermöglichen.  |
| [Videostabilisierungseffekt für die Videoaufnahme](effects-for-video-capture.md) | Beschreibt, wie Sie den Videostabilisierungseffekt verwenden.  |
| [Szenenanalyse für „MediaCapture“](scene-analysis-for-media-capture.md) | Beschreibt, wie Sie den [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) und den [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) verwenden, um den Inhalt des Vorschaudatenstroms der Medienaufnahme zu analysieren.  |
| [Aufnehmen einer Fotosequenz mit „VariablePhotoSequence“](variable-photo-sequence.md) | Beschreibt, wie Sie eine variable Fotosequenz aufnehmen, um mehrere Frames von Bildern in schneller Folge aufzunehmen und jeden Frame so zu konfigurieren, dass unterschiedliche Einstellungen für Fokus, Blitz, ISO, Belichtung und Belichtungskorrektur verwendet werden.  |
| [Verarbeiten von Medienframes mit „MediaFrameReader“](process-media-frames-with-mediaframereader.md) | Beschreibt, wie Sie [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) mit [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) verwenden, um Medienframes aus einer oder mehreren verfügbaren Quellen abzurufen. Hierzu zählen Farb-, Tiefen- und Infrarotkameras sowie Audiogeräte und sogar benutzerdefinierte Framequellen (etwa für Skeletal-Tracking-Frames). Dieses Feature wurde für die Verwendung von Apps entworfen, die Medienframes in Echtzeit verarbeiten, wie beispielsweise Augmented-Reality- und Tiefenkamera-Apps.  |
| [Abrufen eines Vorschauframes](get-a-preview-frame.md) | Beschreibt, wie Sie einen einzelnen Vorschauframe aus dem Vorschaustream der Medienaufnahme abrufen.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>UWP-App-Beispiele für Kamera

* [Beispiel für Gesichtserkennung durch Kamera](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)
* [Beispiel für Kamera-Vorschauframe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraGetPreviewFrame)
* [Beispiel für Kamera-HDR](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)
* [Beispiel für manuelle Kamerasteuerelemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)
* [Beispiel für Kameraprofil](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraProfile)
* [Beispiel für Kameraauflösung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)
* [Kamera-Starterkit](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)
* [Beispiel für Videostabilisierung für Kamera](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraVideoStabilization)

## <a name="related-topics"></a>Zugehörige Themen

* [Audio, Video und Kamera](index.md)
 

 